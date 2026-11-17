---
title: 'Rows vs. Columns: How different are they, really?'
link: https://spiraldb.com/blog/rows-vs-columns
source: hnrss-org
published: 2026-11-12T17:04:41Z
updated: 2026-11-12T17:04:41Z
first_seen: 2026-11-17T18:26:16.162530996Z
authors:
- eatonphil
labels:
- compression
- data-formats
- query-execution
content: extracted
html: 2026-11-12-rows-vs-columns-how-different-are-they-really.html
remote_preview:
  url: https://spiraldb.com/blog/rows-vs-columns/opengraph-image-d09wn6?02662a7c34412feb
  alt: Spiral blog post
---

[Back to blog](https://spiraldb.com/blog)

Connor Tsui

Published Nov 11, 2026 · 12 min read

When we think about structured data, we generally think in terms of rows. Take this simple CSV file, which records some data about bike rides on a bike-sharing app in New York City:

Data in this CSV file is organized into rows, where each row corresponds to a *record* of data. Each record here is one ride, or one "thing", with everything we know about it on one line. We tend to organize data like this almost everywhere: think of spreadsheets, JSON objects, C-style structs in an array, SQL tables, etc. This is because it matches how we think: one "thing", and all of the details of that "thing".

This way of thinking fits a lot of the software we write. When you log in to an app, post a comment on a video, add an item to a shopping cart, send a bank transfer, or book a ride, the application will change just one record (or a few, at most). So it makes sense that the databases behind this software, like Postgres and MySQL, "think in" (or are modeled around) rows as well: they keep each record's details together, so finding, updating, or creating a record means going to one place.

But not everything we ask of a database is limited to one record.

If you run the bike-share service that produced the CSV file above, you'll eventually want to learn something from all the data it collects. This kind of work is called *analytics*. For example, which neighborhood is busiest on Monday and Friday mornings? Do people take longer rides on electric bikes than on classic ones? Observe how no single record can answer these questions. The answers come from the dataset as a whole, so the only way to get them is to look at *all* of the records.[^1]

Notice what these questions don't need, however. Finding the busiest neighborhood only needs the `start_area` and `started_at` columns, and comparing bike types only needs `bike_type` and `duration_min`. Each question may have to look at every row, but it only needs a few of the columns from each one. And a database that keeps each record's details together can't take advantage of that: to read two columns, it has to read all five. We may *think* in rows, but to answer questions like these quickly, we have to program in columns.

Programming in columns begins with how the data is stored, but that's only the start. Once data is stored by column, the way a database runs a query changes dramatically: how much data it reads, how many values it handles at a time, how many values the CPU can process with one instruction, how well the data compresses, etc.

This post will go over these changes, and by the end, you'll see just how much follows from the "simple" choice to store columns instead of rows.

## [Row stores and column stores](https://spiraldb.com/blog/rows-vs-columns#row-stores-and-column-stores)

Let's start with what *doesn't* change. Whether a database stores rows or columns, the table it shows you when you do `SELECT * FROM rides;` is the same. You create it with the same SQL, you query it with the same SQL, and you get the same answers. We can say that the two tables are logically equivalent.

Rows vs columns is a physical distinction rather than a logical one: the data *represents* the same thing, but how that data is actually *arranged* in memory and on disk is different.

Databases are named after the grouping they choose. A row store keeps each record's values next to each other, just like the CSV we started with. Postgres and MySQL, which we mentioned before, are both row stores. They're built for the small reads and writes behind an application, which is called *online transaction processing* (OLTP).[^2]

A column store does the opposite: it takes every record apart and keeps each column's values next to each other instead.[^3] DuckDB and ClickHouse are examples of column stores. They're built for analytics, which is called *online analytical processing* (OLAP).[^4]

### [Rows and columns in memory](https://spiraldb.com/blog/rows-vs-columns#rows-and-columns-in-memory)

To make this concrete, imagine that we have to hold our rides in memory ourselves. This isn't how a real database would be implemented, but it's a good approximation of how each kind arranges its data. In Rust, a row store would be a `Vec` of structs:

A column store will have the same fields, but it will look like a struct of `Vec`s:

Here is what these data layouts look like side by side:

Both of these forms represent the same logical values, but their layout is dramatically different.

### [Reading only the columns you need](https://spiraldb.com/blog/rows-vs-columns#reading-only-the-columns-you-need)

Let's do a fun exercise.[^5] These are the two SQL queries shown above. Which layout is better for executing each of them?

For the `AVG` query, computing it requires only the `duration_min` of every ride and nothing else. In the `Columns` layout, values are stored together in a single `Vec<u32>`, so the query can scan that one array without touching the other four. For `Vec<Ride>` (the `Rows` layout), there is simply no way to read only the durations. Each duration is stored within a `Ride`, so the query has to read through every `Ride` and pass over the other four fields every time.

Note that the code can "ignore" those other fields, but the hardware cannot. A CPU doesn't fetch memory one value at a time: it will fetch fixed-size blocks called *cache lines* (which are 64 bytes on most machines[^6]), fetching the whole line even when the program needs only a few bytes of it. A `Ride` is 48 bytes[^7] and `duration_min` is only 4 of them, so with the row layout, we have to fetch all 48 bytes to read those 4, which is 12x the amount that we actually need. The CPU can't skip any of these lines either because a `Ride` is smaller than a cache line, so every cache line of the array has a duration somewhere in it.

This diagram only shows 8 rides for simplicity (which is why the `duration_min` line is half empty), but the factor of 12 doesn't depend on the size of the table. With one million rides, we might be fetching 48 MB vs 4 MB.

The second query goes the other way. `SELECT * FROM rides WHERE ride_id = 1004` wants *every* field of a single ride, not just one. Once the database finds the record with that ride (probably via an index[^8]), `Vec<Ride>` has all of its fields next to each other, so reading all of them touches only one or two cache lines (it might overlap two cache lines).[^9] On the other hand, `Columns` has spread the same ride over five different arrays, so the query has to fetch at least five cache lines to put it back together. For this query, the `Rows` layout wins.

Now that we have a better idea of what we're working with, let's go a bit deeper into how we can speed up queries over columnar data! Analytical data processing is an active field of research spanning several decades, so we won't be able to go over every possible optimization. Nevertheless, the rest of this post will give a good overview of the techniques that most modern analytical database systems utilize.

## [Vectorized execution](https://spiraldb.com/blog/rows-vs-columns#vectorized-execution)

The simplest way to run `SELECT AVG(duration_min) FROM rides;` is to visit one ride at a time: take a ride, add its duration, and repeat, dividing by the length of the column at the end. However, a column store doesn't necessarily have to execute this way. Since all of the durations sit in one contiguous array, it can process them in *batches* of a few thousand values, where each batch is small enough to stay in the CPU's cache.[^10] This is called vectorized execution.[^11]

But why would a batch be any faster than one value at a time? To understand this, we first need to understand how a database execution engine actually runs these queries.[^12]

A database has to be able to run *any* query, so it builds and plans each one with general-purpose pieces called *operators*. For example, we might have an operator that reads a column, another that performs filters, another that computes a sum, and so on. If the operators pass each other one `Ride` at a time (this is called the Volcano model[^13]), then for every single ride, the database has to figure out which operator runs next, which column it wants, and what type that column is. All of this bookkeeping and interpretation cost can easily take more time than the summation itself.[^14] With batches, we only pay for the bookkeeping once per batch instead of once per value, and the actual work turns into a tight loop over an array.

So why stop at a few thousand values, and not just process the entire column at once? Operators hand their results to each other, so a small batch of results can stay in the CPU's cache until the next operator picks it up. An operator result the size of an entire column generally cannot fit inside the CPU's cache, so the CPU would have to write it back out to main memory and then read it back in.

For our query, the work on each batch is a single loop:

In addition to vectorized execution having better cache locality, we can speed up our execution of the query with data-parallel operations. Modern CPUs have instructions that operate on several values at once, called [SIMD](https://en.wikipedia.org/wiki/Single_instruction,_multiple_data) (*single instruction, multiple data*). They work best when the values are the same type and are contiguous in memory, which is exactly what a batch of a column is.

So when the compiler sees the `for` loop above, it can emit SIMD instructions on its own. This is a technique called *autovectorization*[^15] that is supported well in compiler backends such as LLVM.[^16]

With [AVX-512](https://en.wikipedia.org/wiki/AVX-512) (which can load 512 bits at once), one [`vpaddq`](https://www.felixcloutier.com/x86/paddb:paddw:paddd:paddq) instruction adds 8 64-bit durations at once.[^17] Note that `vpaddq` doesn't add those 8 durations to each other. A 512-bit register has 8 slots of 64 bits, and each slot holds its own running sum: the first duration is added to the first slot, the second duration to the second slot, and so on. Only after the loop does the code add the slots together to get the final sum (the figure below leaves that last step out).

LLVM IR and AVX-512 assembly

This is the output of `rustc` 1.99.1 with `-C opt-level=3 -C target-feature=+avx512f`, with the attributes, metadata, and assembler directives removed.

Note that the compiler keeps four registers of running sums (`zmm0` to `zmm3` in the assembly) instead of one. This is a separate optimization from SIMD called *loop unrolling*:[^18] the 8 slots in each register come from SIMD, and the four registers come from the compiler doing four steps of the loop in every pass. Together, every pass of the loop adds 4 × 8 = 32 durations.

This is the optimized LLVM IR:

This is the x86-64 assembly that LLVM generates:

Note that autovectorization is not unique to sums. Much of the time executing a query (especially a relatively simple one) is spent in tight loops over batches like this one, and the compiler can vectorize many of them in the same way without anyone writing a single SIMD instruction by hand.

## [Compression](https://spiraldb.com/blog/rows-vs-columns#compression)

So far, we've only looked at data that is already in memory. In most database systems, data lives on persistent storage (a disk, an SSD, or object storage like S3), and bringing it into memory is usually the bottleneck of query execution (in other words, queries are I/O bound). So the size of our data is very important: the fewer bytes a query has to read, the less time it spends waiting on I/O.

The naive way to make the data smaller is to take each block of it and run it through a general-purpose compression algorithm.[^19] This kind of compression should be familiar to anyone who has ever dealt with zip files! It treats the data as just a big pile of bytes, and it doesn't care what those bytes actually mean, which is why it works on everything (including both rows and columns).

The problem is that this kind of compression is a "black box": once a block is compressed, the database can't see inside of it anymore. For example, if you wanted to read the one-millionth line of a zipped CSV file, there is no way to jump straight to that line, so you would have to unzip (decompress) everything that comes before it just to access a single line.[^20] The ability to read any one piece of the data without reading the rest of it is called *random access*, and this kind of compression is unable to support fast random access.

A database system will have the exact same problem with black-box compressed blocks of data: to read the duration of ride 1004, it has to decompress the entire block (potentially multiple blocks!) that the ride lives in.

This is another place where columns help. The values in a column all have the same type, and they tend to be similar to each other, which means that a column compresses much better than a row does. We can already see this in our example above: `start_area` only has 3 distinct values, `bike_type` only has 2, and `ride_id` is always increasing.

More importantly, we don't actually *need* to use black-box compression. We can instead use lightweight encodings: simple schemes that compress the data while retaining the structure and the meaning of the values.[^21] Instead of turning a column into a big pile of bytes, a lightweight encoding takes advantage of one specific pattern in it (for example, a column with only a few distinct values, or a column of numbers that are all small) to store the same values in less space.

Retaining the structure means that we can keep random access. With most lightweight encodings, the database system can jump straight to the value that it wants without decoding everything around it.

Retaining the *meaning* helps as well. Lightweight encodings are "lighter weight" than black-box compression, but they aren't free, since decoding them still takes CPU time. However, because the encoded values still *mean* something, a query can often do its work directly on the encoded data without decoding it first, which skips much of that cost. This computation over compressed data is a form of "late materialization".[^22]

This is exactly how we built [Vortex](https://github.com/vortex-data/vortex), an open-source, extensible columnar file format and data processing library. It has a whole collection of lightweight encodings, and it can stack them on top of each other to make the data even smaller.

> We've written a lot about how this works. Check out our other blogs!
>
> - [Vortex: One Format for Any Shape](https://spiraldb.com/blog/vortex-one-format-for-any-shape) for an overview of how Vortex works
> - [FastLanes](https://spiraldb.com/blog/life-in-the-fastlanes) for integers
> - [ALP](https://spiraldb.com/blog/alp-rust-is-faster-than-c) for floats
> - [FSST](https://spiraldb.com/blog/compressing-strings-with-fsst) for strings
> - [BtrBlocks](https://spiraldb.com/blog/cascading-compression-with-btrblocks) for choosing an encoding for each column
> - [Push-down compute](https://spiraldb.com/blog/what-if-we-just-didnt-decompress-it) for working on compressed data
> - [`LIKE` on compressed strings](https://spiraldb.com/blog/pattern-matching-strings-without-decompression) without decompressing them

## [Thinking in columns](https://spiraldb.com/blog/rows-vs-columns#thinking-in-columns)

We started with data from a CSV file and one "simple" choice of storing data in rows vs columns. Yet we have seen just how much follows from that choice, as well as the consequences of choosing one or the other.[^23]

Those consequences are the reason that most database systems are built for either transactions or analytics, and not both.[^24] The way a database executes queries and the way it compresses data are both built on top of this one choice, so it isn't something that a system can just flip later. A column store's strength in serving analytical queries is exactly what makes it bad for reading and changing one record at a time, and the opposite is true for a row store.

There is also a lot more to columnar data than what we can fit in a single introductory post. For example, we never talked about how a column store actually organizes its data on persistent storage,[^25] how it avoids reading data that a query doesn't need,[^26] how vectorized execution isn't the only way to run a query more efficiently,[^27] and we completely ignored writes and modifications to data![^28] Each one of these topics is a whole area of active research. If you want to keep going, the CMU database courses are a great place to start, and all of their lectures are free to watch online.[^29]

In the future, we hope to post more about these kinds of topics, as we engage with them quite a lot working at Spiral!

[^1]: In practice, a database can often skip records that it knows it doesn't need, for example with an index, or with the minimum and maximum values it keeps for each block of data ([zone maps](https://spiraldb.com/blog/zone-maps-or-queries-go-brrr)). But logically speaking, the answers still *depend* on every record.

[^2]: Think of things like bank transfers and airline bookings: lots of small operations (transactions) that read or change only a few records, all happening in parallel, potentially thousands of times per second.

[^3]: The idea is a lot older than both of those systems. [A Decomposition Storage Model](https://doi.org/10.1145/318898.318923) (SIGMOD 1985) already proposed storing each column on its own. The CMU 15-445 [lecture notes on storage models](https://15445.courses.cs.cmu.edu/fall2026/notes/06-storage3.pdf) compare the two layouts in more detail.

[^4]: These are generally queries that read a large part of a dataset, usually worried about only a few of its columns, summarizing the data with counts, sums, and averages.

[^5]: Personally, I consider these fun...

[^6]: Apple's M-series chips use 128-byte cache lines, but the point remains the same.

[^7]: This is 8 bytes each for `ride_id` and `started_at` (a 64-bit timestamp), 24 for the `String` (a pointer to the text, a length, and a capacity), 1 for `bike_type`, 3 bytes of padding for alignment, and 4 for `duration_min`. Note that the 24 bytes for `start_area` are specific to Rust's `String`. A real database will store strings differently. Also, Rust is free to reorder the fields of a struct unless you add `#[repr(C)]`, and `rustc` 1.99.1 does: it moves `start_area` to the front and the padding to the end. The struct is still 48 bytes with 3 bytes of padding either way, so we draw the fields in the order that we wrote them.

[^8]: An [index](https://en.wikipedia.org/wiki/Database_index) is a separate data structure (usually a [B+ tree](https://en.wikipedia.org/wiki/B%2B_tree)) that maps a "key" (usually a unique primary key) like `1004` to the location of its stored record. This allows the database to quickly find the data instead of scanning the entire table to find it. See the CMU 15-445 [lecture notes on indexes](https://15445.courses.cs.cmu.edu/fall2026/notes/08-indexes1.pdf) for more detail.

[^9]: These counts leave out the text of `start_area`. The 24 bytes of a `String` only point to the text, which lives somewhere else in memory, so actually reading the name of the neighborhood costs at least one more cache line. That is true for both layouts, though, so it doesn't change which one wins.

[^10]: [MonetDB/X100](https://www.cidrdb.org/cidr2005/papers/P19.pdf), the system that introduced this design, uses 1024 values per batch by default. [DuckDB](https://duckdb.org/docs/stable/internals/vector) uses 2048, and [DataFusion](https://datafusion.apache.org/user-guide/configs.html) uses 8192.

[^11]: Also, a real database table will generally live on persistent storage (HDD, SSD) if the data is larger than memory, so the database will not be able to load a whole column at once even if it wanted to. So the query has to read the column in pieces anyways.

[^12]: The CMU 15-445 [lecture notes on query execution](https://15445.courses.cs.cmu.edu/fall2026/notes/13-queryexecution1.pdf) go into much more detail, and the CMU 15-721 notes on [query processing](https://15721.courses.cs.cmu.edu/spring2024/notes/04-execution1.pdf) and [vectorized execution](https://15721.courses.cs.cmu.edu/spring2024/notes/06-vectorization.pdf) go further still.

[^13]: Named after [Volcano](https://doi.org/10.1109/69.273032), a query engine by [Goetz Graefe](https://scholar.google.com/citations?user=pdDeRScAAAAJ&hl=en) from the early 90s. It is also sometimes called the "iterator" model, and almost every row store still executes queries this way.

[^14]: Vectorized execution comes from [MonetDB/X100: Hyper-Pipelining Query Execution](https://www.cidrdb.org/cidr2005/papers/P19.pdf) (CIDR 2005). The paper profiled MySQL on an analytical query and found that the operations that do the actual work take only about 10% of the execution time. X100's ran every operator on small vectors of values, which is where the name comes from.

[^15]: Note that *vectorized execution* is the database processing values in batches, whereas *autovectorization* is the compiler emitting SIMD instructions. They are separate but similar ideas.

[^16]: [Autovectorization](https://en.wikipedia.org/wiki/Automatic_vectorization) is easy to break, though. If `duration_min` were a float, LLVM would [not vectorize this sum](https://llvm.org/docs/Vectorizers.html#reductions) by default: adding 8 values at a time changes the order of the additions, and floating-point addition isn't associative, so the result could change.

[^17]: 8 and not 16 because every 32-bit duration is widened to 64 bits so that the sum doesn't overflow.

[^18]: [Loop unrolling](https://en.wikipedia.org/wiki/Loop_unrolling) helps here because each `vpaddq` has to wait for the previous one that wrote to the same register, so with only one register, the additions would have to run one after another. With four independent registers, the CPU can [run several of them at the same time](https://llvm.org/docs/Vectorizers.html#partial-unrolling-during-vectorization).

[^19]: Zip files use an algorithm called [DEFLATE](https://en.wikipedia.org/wiki/Deflate). Databases tend to use newer ones like [Zstandard](https://en.wikipedia.org/wiki/Zstd) and [LZ4](https://en.wikipedia.org/wiki/LZ4_\(compression_algorithm\)), but the idea is the same.

[^20]: This is a bit of a lie. A plain CSV file doesn't have real random access since its lines have different lengths, so you can't know where line one million starts without counting. A better example is black-box compressing a `Vec<u32>` of durations, where value number N always starts at byte 4 × N. Once the array is block-box compressed, we can no longer index into the vector with random access.

[^21]: Two recent papers on lightweight encodings are [The FastLanes Compression Layout](https://www.vldb.org/pvldb/vol16/p2132-afroozeh.pdf) (VLDB 2023) and [BtrBlocks: Efficient Columnar Compression for Data Lakes](https://www.cs.cit.tum.de/fileadmin/w00cfj/dis/papers/btrblocks.pdf) (SIGMOD 2023). The CMU 15-721 lecture notes on data formats and encodings ([part 1](https://15721.courses.cs.cmu.edu/spring2024/notes/02-data1.pdf), [part 2](https://15721.courses.cs.cmu.edu/spring2024/notes/03-data2.pdf)) show how real file formats put encodings like these to use.

[^22]: This term comes from research on column stores, where it essentially means waiting as long as possible (late) before putting the columns of a row back together (materializing). See [Materialization Strategies in a Column-Oriented DBMS](https://doi.org/10.1109/ICDE.2007.367892). Compute over compressed data is the same idea one level lower: wait as long as possible before decompressing a value. Executing a query directly on compressed columns goes all the way back to [Integrating Compression and Execution in Column-Oriented Database Systems](https://www.cs.umd.edu/~abadi/papers/abadisigmod06.pdf) (SIGMOD 2006), though Vortex takes this concept several steps further.

[^23]: [Column-Stores vs. Row-Stores: How Different Are They Really?](https://www.cs.umd.edu/~abadi/papers/abadi-sigmod08.pdf) (SIGMOD 2008) looks at what happens if you only change the storage.

[^24]: People have tried to build one system that does both in the past ([HTAP](https://en.wikipedia.org/wiki/Hybrid_transactional/analytical_processing), or hybrid transactional/analytical processing), but those attempts generally didn't work out. Some newer systems seem to be bridging the gap, however. [CedarDB](https://cedardb.com/), the commercial version of [Umbra](https://umbra-db.com/) (a research system from TU Munich), runs transactions and analytics on just one engine and one copy of the data. Databricks' [Lakebase](https://www.databricks.com/product/lakebase) keeps Postgres (a row store) as the engine for transactional queries, but its storage layer rewrites the same data into columnar formats in the background, allowing an analytical engine to query it.

[^25]: Most formats split the table into large groups of rows and store the columns separately inside of each group (this is known as a "row group"). This idea goes back to PAX, from [Weaving Relations for Cache Performance](https://www.vldb.org/conf/2001/P169.pdf) (VLDB 2001), and it's how [Apache Parquet](https://parquet.apache.org/docs/file-format/) files work today. [An Empirical Evaluation of Columnar Storage Formats](https://www.vldb.org/pvldb/vol17/p148-zeng.pdf) (VLDB 2024) looks at how well these formats hold up on modern hardware and workloads. For columns in memory instead of on disk, the standard is the [Apache Arrow columnar format](https://arrow.apache.org/docs/format/Columnar.html).

[^26]: The most common trick is to keep "zone maps" of the minimum and maximum value of every block of data so that a query can completely skip the blocks that can't contain certain data. We wrote about this in [Zone maps, or “queries go brrr”](https://spiraldb.com/blog/zone-maps-or-queries-go-brrr).

[^27]: The other big approach is to compile each query into machine code that is specialized for exactly that query. [Everything You Always Wanted to Know About Compiled and Vectorized Queries But Were Afraid to Ask](https://www.vldb.org/pvldb/vol11/p2209-kersten.pdf) (VLDB 2018) compares the two approaches, and neither one is a clear winner.

[^28]: Changing data can be expensive in a column store because a single new record has to go to a different place for each of its columns. [C-Store](https://www.vldb.org/archives/website/2005/program/paper/thu/p553-stonebraker.pdf) (VLDB 2005) deals with this by keeping a small store that is built for writes next to the big one that is built for reads, and moving data from one to the other in the background.

[^29]: [CMU 15-445](https://15445.courses.cs.cmu.edu/fall2026/schedule.html) is the introductory course ([Fall 2025 lecture videos](https://www.youtube.com/playlist?list=PLSE8ODhjZXjYMAgsGH-GtY5rJYZ6zjsd5)), and [CMU 15-721](https://15721.courses.cs.cmu.edu/spring2024/schedule.html) is the advanced one ([lecture videos](https://www.youtube.com/playlist?list=PLSE8ODhjZXjYa_zX-KeMJui7pcN1rIaIJ)). Note that the author of this post learned most of this material from the CMU courses! If you'd rather read, [The Design and Implementation of Modern Column-Oriented Database Systems](https://ir.cwi.nl/pub/21772/1900000024-Abadi-Vol5-DBS-024.pdf) (2013) is a survey of the whole field.
