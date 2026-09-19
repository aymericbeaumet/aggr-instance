---
title: 'Tin: full-text search for Postgres'
link: https://planetscale.com/blog/introducing-tin
source: hnrss-org-frontpage
published: 2026-09-19T13:52:06Z
updated: 2026-09-19T13:52:06Z
first_seen: 2026-09-19T19:18:40.363745098Z
authors:
- ksec
summary: 'Article URL: https://planetscale.com/blog/introducing-tin Comments URL: https://news.ycombinator.com/item?id=49766611 Points: 125 # Comments: 57'
content: extracted
html: 2026-09-19-tin-full-text-search-for-postgres.html
preview:
  file: 2026-09-19-tin-full-text-search-for-postgres.preview-cd75a6bd1ef5.webp
  width: 256
  height: 134
  color: '#2c2a22'
images:
- source: https://planetscale.com/assets/introducing-tin-social-BxZcr7Qp.png
  original:
    file: 2026-09-19-tin-full-text-search-for-postgres.image-f844d4667202.png
    width: 1200
    height: 628
  color: '#191919'
---

One of the Postgres features our customers ask us for the most is full-text search. Today, we are excited to announce TIN: a fast, full-featured, reliable full-text search extension for Postgres. TIN stands for "Text INdex," and that is what it does.

TIN is [available immediately](https://planetscale.com/docs/postgres/search/get-started) as a GA release for all Postgres and Neki databases. Check it out:

```
CREATE INDEX an_index_name ON table_name USING tin(text_column_name);
SELECT * FROM table_name
  WHERE text_column_name ==> 'some words';
```

We built TIN because we believe a good text index should support:

- Boolean expressions, phrase queries, and span queries
- Fuzzy, wildcard, and regular-expression matching for terms
- Case and accent folding
- `COUNT(*)` queries and BM25-scored top-k queries

A good text index *in Postgres* must support all of those things while also handling joins, complicated `WHERE` clauses across full-text and other column types, continuous updates, replication, backups, and correct transaction visibility.

Although there are at least three existing text-search indexes for Postgres already, none of them met all of those requirements. TIN does. TIN is also really, mind-blowingly fast.

## [What TIN is for](https://planetscale.com/blog/introducing-tin#what-tin-is-for)

Application developers use text indexes to build a variety of search features. An e-commerce platform might need to search for the top ten products containing all keywords in the search:

```
SELECT * FROM products
  WHERE description ==> 'stretch denim jeans'
  ORDER BY tin.score(ctid) DESC
  LIMIT 10
```

A legal discovery platform might be required to return every document containing one or more of a set of keywords, but not care at all about ranking:

```
SELECT * FROM emails
  WHERE body ==> '[insider trading conspiracy]'
```

A photo tagging platform might show an exact count of photographs with a particular tag:

```
SELECT COUNT(*) FROM photos
  WHERE tags ==> '"san francisco"';
```

Most applications also need to insert, update, and delete documents, even while continuing to query the index. Search queries must return matches based on new or changed rows as soon as they've been committed.

## [TIN performance and benchmarking](https://planetscale.com/blog/introducing-tin#tin-performance-and-benchmarking)

We ran benchmarks to assess performance for all the above use cases and more. We tried workloads:

- With conjunction (must contain all words), disjunction (must contain any word), and phrase (must contain all words in sequence) queries and a mix of all three.
- That count documents or that ask for the top *k* by BM25 score.
- With and without clients writing new data to the index concurrently with the benchmark query workload.

### [Workloads and corpus](https://planetscale.com/blog/introducing-tin#workloads-and-corpus)

We have measured TIN against a variety of text corpora: all of Wikipedia, a collection of Reddit comments totaling 2.3 TB, and a mixed workload we call simply "pile" with 797 GB of open-access research papers, legal documents, public domain books, and Enron emails. The benchmark results we share in this article are from an export of questions and answers from Stack Exchange: an 85 GB corpus with 150 million documents. Because the corpus has no standard query trace, we generated a synthetic one by sampling substrings ranging from 2 to 15 terms. We interpreted each substring three ways: as a conjunction, as a disjunction, and as a phrase query, for a total of 1,719 queries.

### [Test environment](https://planetscale.com/blog/introducing-tin#test-environment)

We ran our benchmarks on an AWS i7i.8xlarge EC2 instance with local NVMe storage and a modern, AVX-512-capable CPU. For each text-search extension, we set up Postgres 18.6 in an isolated container limited to 8 vCPUs and 32 GB of RAM. That's small enough to show how each index system performs when the index doesn't just fit in Postgres buffers. The benchmark phases ran sequentially, so the engines did not compete for resources. We chose a standalone EC2 instance to minimize the impact of operational overhead and replication and to ensure that anyone who wants to reproduce our benchmarks of competing text-search indexes can do so using the same instance type and container limits.

To drive the search traffic against the Postgres containers, we used the [ParadeDB Benchmarker](https://github.com/paradedb/benchmarker). We have [a forked version](https://github.com/planetscale/paradedb-benchmarker) that pre-warms before beginning measurement and adds metrics for bytes read and WAL bytes written. We left all Postgres parameters at the defaults that the Benchmarker supplies, except for three: we set `max_parallel_workers` to 8 (from 40), `shared_buffers` to 24 GB (from 128 MB), and `maintenance_work_mem` to 24 GB (from 64 MB), to best match the resources of the container. We ran the Benchmarker on the same EC2 instance as the target Postgres server, to ensure that network latency did not impact the measurements.

For each scenario, we measured the performance of TIN v1.0.2 against all the other Postgres text-search indexes that were capable of running the workload at all: ParadeDB v0.25.2, pg\_textsearch v1.4.0, and the GIN index built into Postgres v18.6. Aside from TIN, only ParadeDB was able to complete all of the benchmarks.

### [Index build time and size](https://planetscale.com/blog/introducing-tin#index-build-time-and-size)

Indexes range from 33% to 61% of the size of the corpus, and they took from 8 to 129 minutes to prepare, build, and finalize. The three engines other than TIN failed with the container's configured 32 GB limit, so for index builds only, we increased the available RAM as shown in the table. Before running queries, we set the container back to 32 GB of RAM for everyone.

|                | Total time | Index size | Required RAM |
| -------------- | ---------- | ---------- | ------------ |
| TIN            | 8m10s      | 50.7 GB    | 32 GB        |
| ParadeDB       | 19m20s     | 52.1 GB    | 64 GB        |
| pg\_textsearch | 26m49s     | 41.5 GB    | 128 GB       |
| Postgres GIN   | 2h09m04s   | 28.0 GB    | 64 GB        |

### [Mixed queries, top-10 ranked](https://planetscale.com/blog/introducing-tin#mixed-queries-top-10-ranked)

Our first benchmark compares TIN against ParadeDB, for a workload with mixed (conjunction, disjunction, and phrase) queries, top-10 results by BM25 score, with no concurrent writes to the index. TIN handles 25× as many queries per second as ParadeDB does, with p99 latencies 26× lower. GIN can't complete this benchmark, because it runs out of memory performing the disjunction searches. pg\_textsearch can't complete the benchmark because it handles *only* disjunction searches.

### [Conjunction and phrase queries, top-10 ranked](https://planetscale.com/blog/introducing-tin#conjunction-and-phrase-queries-top-10-ranked)

Our next benchmark compares TIN against ParadeDB and Postgres GIN, for top-10 conjunction and phrase queries, with no concurrent writes. TIN and ParadeDB rank using BM25, while GIN ranks using `ts_rank_cd`. TIN handles 10× as many queries as ParadeDB and 541× as many as GIN, with p99 latencies 6× and 1,356× lower, respectively. pg\_textsearch is again absent because it handles only disjunction queries.

### [Disjunction queries with concurrent writes](https://planetscale.com/blog/introducing-tin#disjunction-queries-with-concurrent-writes)

Our third result compares TIN against both ParadeDB and pg\_textsearch, for a workload with disjunction queries, top-10 results by BM25 score, and a concurrent client targeting 1,000 `UPDATE` queries per second. TIN handles 36× as many queries as pg\_textsearch and 57× as many queries as ParadeDB, with p99 latencies 24× and 36× lower, respectively. Over the course of a ten-minute run, TIN completes 270,279 updates, while ParadeDB completes 185,584, and pg\_textsearch completes only 735.

ParadeDB's approach to accepting writes sacrifices read throughput and latency. pg\_textsearch maintains the same 3.5 QPS for readers both with and without writes because continuous read traffic prevents write traffic from ever getting the locks it needs, so writes stall after just a few seconds. GIN is again absent because it runs out of memory on disjunction queries.

### [When the index fits in memory](https://planetscale.com/blog/introducing-tin#when-the-index-fits-in-memory)

In the intro, we claimed that TIN is mind-blowingly fast.

Our final graph shows what TIN, ParadeDB, and Postgres GIN can do when the index fully fits in shared buffers. This workload counts (but does not rank) the documents that match a disjunction query against Wikipedia, an 8.0 GB corpus. pg\_textsearch is absent here because it can only perform top-k queries, not counting queries.

### [Full results](https://planetscale.com/blog/introducing-tin#full-results)

That is perhaps enough graphs, but it doesn't cover all of our use cases. Here are those same scenarios, plus several more, in table form. The "MB/query" column shows how much data each index read from the disk or block cache for each query. TIN's lower numbers for MB/query are part of why it's faster, and they also reduce the impact of TIN queries on the block cache and I/O capacity, meaning that other queries on the same server stay fast, too.

```
Conjunction, disjunction, and phrase queries; top-10
┌────────────────────────────────────────────────────────────────────┐
│                            QPS        p99    MB/query     Updates  │
├─────────────────────────┬───────┬──────────┬───────────┬───────────┤
│ TIN - read-only         │  199  │   256ms  │       65  │           │
│     - with updates      │  172  │   284ms  │       88  │  271,398  │
├─────────────────────────┼───────┼──────────┼───────────┼───────────┤
│ ParadeDB - read-only    │  7.9  │ 6,765ms  │      582  │           │
│          - with updates │  6.0  │ 7,990ms  │      591  │  193,487  │
└─────────────────────────┴───────┴──────────┴───────────┴───────────┘
```

```
Conjunction and phrase queries; top-10 (read-only)
┌───────────────────────────────────────────────┐
│                  QPS         p99    MB/query  │
├───────────────┬───────┬───────────┬───────────┤
│ TIN           │  242  │     212ms │        73 │
├───────────────┼───────┼───────────┼───────────┤
│ ParadeDB      │   24  │   1,279ms │       668 │
├───────────────┼───────┼───────────┼───────────┤
│ Postgres GIN  │  0.4  │ 288,066ms │       595 │
└───────────────┴───────┴───────────┴───────────┘
```

```
Disjunction queries; top-10
┌────────────────────────────────────────────────────────────────────────┐
│                                 QPS         p99    MB/query   Updates  │
├──────────────────────────────┬───────┬───────────┬─────────┬───────────┤
│ TIN - read-only              │  148  │    324ms  │     48  │           │
│     - with updates           │  125  │    354ms  │     77  │  270,279  │
├──────────────────────────────┼───────┼───────────┼─────────┼───────────┤
│ ParadeDB - read-only         │   17  │  2,385ms  │    303  │           │
│          - with updates      │  2.2  │ 12,634ms  │    394  │  185,584  │
├──────────────────────────────┼───────┼───────────┼─────────┼───────────┤
│ pg_textsearch - read-only    │  3.5  │  8,646ms  │ 11,639  │           │
│               - with updates │  3.5  │  8,409ms  │ 11,656  │      735  │
└──────────────────────────────┴───────┴───────────┴─────────┴───────────┘
```

```
Conjunction, disjunction, and phrase queries; COUNT(*) (read-only)
┌─────────────────────────────────────────┐
│              QPS      p99     MB/query  │
├───────────┬───────┬──────────┬──────────┤
│ TIN       │  179  │   438ms  │      97  │
├───────────┼───────┼──────────┼──────────┤
│ ParadeDB  │   10  │ 2,704ms  │     544  │
└───────────┴───────┴──────────┴──────────┘
```

```
Disjunction queries; COUNT(*); Wikipedia corpus (read-only)
┌───────────────────────────────────────────────────┐
│                     QPS         p99     MB/query  │
├───────────────┬──────────┬─────────────┬──────────┤
│ TIN           │  10,260  │        2ms  │     1.7  │
├───────────────┼──────────┼─────────────┼──────────┤
│ ParadeDB      │     291  │       95ms  │      22  │
├───────────────┼──────────┼─────────────┼──────────┤
│ Postgres GIN  │     1.4  │   30,292ms  │     2.5  │
└───────────────┴──────────┴─────────────┴──────────┘
```

As you can see, in a wide variety of scenarios, TIN has throughput at least 8× higher than the alternatives, reads far less data from the disk, and experiences only a small performance drop even while the index is updating hundreds of rows per second.

## [Why TIN is fast](https://planetscale.com/blog/introducing-tin#why-tin-is-fast)

TIN's performance in benchmarks may be hard to believe. In the hopes of making it more believable, or at least satisfying the reader's curiosity, we'll explain a bit about architectural choices that make TIN so fast. In short: all document postings are Postgres `ctid`s rather than contiguous document identifiers, and this lends itself to highly vectorized intersection and union operations on modern CPUs.

### [Document identification](https://planetscale.com/blog/introducing-tin#document-identification)

A text index needs an identifier for each version of each document it indexes. It groups those identifiers into highly compressed postings lists; each postings list tracks all the documents that contain one given word. In a large corpus, a postings list for a common word like "the" may contain billions of postings, while the postings list for a term like "xyz-9876" would contain only a few.

Most text search systems organize their indexes into **segments**. The *n* documents whose postings exist in a segment are usually assigned document identifiers 1 to *n*. Sequential document identifiers allow postings lists to be highly compressed using various techniques such as delta-encoding and bit-packing. But it also means document identifiers in different segments are assigned independently; document ID `42` in segment 4 is a completely different document than ID `42` in segment 7.

TIN also organizes its index into segments, but not for purposes of document numbering. Instead, TIN directly uses Postgres' `ctid` value as a document identifier.

Every version of every row (tuple) stored in a Postgres table has an associated `ctid` value. `ctid` is short for "current tuple identifier." Any row inserted or updated gets a new `ctid`. It is a 48-bit number that directly identifies a tuple's physical location in the Postgres heap. Represented textually as `(<block number>, <offset number>)`, the upper 32 bits indicate the block number and the lower 16 indicate the offset within that block. From now on, we will refer to the `<block number>` part as the "page number" or "page."

Given the `ctid` of `(190, 17)` we know that the tuple it represents is the one at the 17th slot on page 190. Instant O(1) lookup! You can even query and retrieve rows from the heap directly using `ctid`s:

```
-- retrieve the first 10 rows from "books" in physical heap order
SELECT ctid, id, title FROM books ORDER BY ctid LIMIT 10;

-- no scan required!  instant O(1) lookup of the row
SELECT * FROM books WHERE ctid = '(190, 17)';
```

TIN directly uses `ctid`s because Postgres internally uses `ctid`s. Postgres extensions that implement a new index type must return `ctid`s. Postgres bitmap scans are backed by potentially lossy bitmaps of `ctid`s. Postgres' internal index types (b-tree, GIN, GiST, and hash) use `ctid`s as their postings. `ctid`s are everywhere within Postgres.

To operate within Postgres, a text search system that assigns sequential identifiers must, at some point, convert those identifiers back into a `ctid` in order for Postgres to work with it. Both ParadeDB and pg\_textsearch maintain a separate data structure just to perform this mapping. If a text search matches 10 million rows, ParadeDB and pg\_textsearch have to look up 10 million identifiers in their `ctid` mappings. TIN avoids that work completely.

### [48-bit identifiers are crazy](https://planetscale.com/blog/introducing-tin#48-bit-identifiers-are-crazy)

Normal postings-list compression techniques don't work well with discontiguous 48-bit numbers. Delta encoding breaks at each page boundary, and bitmaps are too sparse to be efficient. Fortunately, some interesting properties of Postgres pages make two-level bitmap encoding practical. An 8KB page can never contain more than 291 tuples ([8192 bytes, minus 24 for the page header, divided by at least 28 per non-empty tuple](https://www.postgresql.org/docs/current/storage-page-layout.html)), and for table schemas with `TEXT` and other columns, pages often contain 32 or fewer tuples. So the list of page numbers is dense enough to use a bitmap, and within each page, the list of offset numbers is dense enough (and small enough) to use tiny bitmaps per page.

Savings relative to naively storing 48-bit `ctid` values can be quite significant. Over an entire corpus, high-frequency terms approach 1 bit per posting, medium-frequency terms settle around 7 bits per posting, and rare-frequency terms can approach 25 bits per posting. Terms that appear only once are not stored as bitmaps at all.

### [Work elision and vectorization](https://planetscale.com/blog/introducing-tin#work-elision-and-vectorization)

TIN's page-level bitmaps (which pages contain a given term) have 256 bits, which fits nicely into vector registers on any x86 CPU with AVX2 or higher. That allows several optimizations.

Consider the query `the AND rareword`. TIN `AND`s the page-level bitmaps, 256 bits (pages) at a time. Any bit that's absent from the intersection is a page whose offset-level bitmaps TIN doesn't need to decode at all.

For `COUNT(*)` disjunction queries such as `the OR rareword`, TIN often skips reading postings lists entirely. TIN's index metadata stores each term's exact posting counts. If the page-level bitmaps for two words have no bits in common, the count of their disjunction is just the sum of those exact posting counts.

Every page-level bitmap fits into a single AVX2 register, and every offset-level bitmap fits into either one AVX-512 register or two AVX2 registers. Conjunction and disjunction queries are just `AND` and `OR` instructions on those vector registers, respectively. Queries that count the number of matches can use CPU-native `POPCNT` instructions to count the bits in the resulting bitmap. Expensive loops and branch instructions are largely avoidable.

A query that wants rows rather than counts computes the `ctid` from the bit position rather than looking it up on disk. The position of a set bit *is* the `ctid`.

The document `ctid`s that TIN returns to Postgres from a given segment naturally identify pages, and tuples within a page, in heap order. This means that when Postgres needs to read matched tuples from the heap, it happens in heap order. Even with modern NVMe disks, sequential access is far faster than random access; TIN gets this optimization for free.

### [Solving MVCC](https://planetscale.com/blog/introducing-tin#solving-mvcc)

TIN returns results that are MVCC-correct, meaning a statement executed at any point in time sees or operates only on tuples that are currently visible to it. This means every heap-backed query result needs to be checked for visibility relative to the current snapshot.

#### [Heap checks](https://planetscale.com/blog/introducing-tin#heap-checks)

There are a few different approaches to this. Some queries are inherently heap checked:

```
SELECT a, b, c FROM lyrics WHERE content ==> 'give you up'
```

Because the query returns actual heap data (the `a, b, c` columns), TIN must fetch from the heap all matching `ctid`s returned by `==> 'give you up'` anyway. When TIN asks Postgres for the physical tuple data behind each `ctid`, Postgres tells TIN whether that tuple is visible to the current snapshot. If it is, TIN returns it; otherwise, TIN moves to the next matching `ctid`, until all visible matches have been returned.

#### [Visibility map](https://planetscale.com/blog/introducing-tin#visibility-map)

Other query shapes can be executed similarly to Postgres' "Index Only Scan" where the answer is returned directly from the index without touching the heap (or at least hopefully not all of the heap). Consider a count-only query like this:

```
SELECT COUNT(*) FROM lyrics WHERE content ==> 'give you up'
```

If every heap page is marked all-visible, TIN can return that count without touching a single heap page.

Not all data is static, of course, and in the case of mutated heaps, TIN does additional optimizations to ensure it's only counting visible rows by performing direct intersections with Postgres' visibility map. TIN's page-level bitmaps are exactly the right mechanism to intersect efficiently against Postgres visibility maps, which are also page-level bitmaps. Only `ctid`s on not-all-visible pages need to be checked against the heap. Normally, a Postgres index returns all `ctid`s that match regardless of visibility, and the Postgres executor checks visibility for each one. TIN plans custom scans that move visibility checks into TIN itself, where they can take advantage of vector instructions on page-level bitmaps.

#### [VACUUM and TIN's liveness bitmap](https://planetscale.com/blog/introducing-tin#vacuum-and-tins-liveness-bitmap)

Text indexes that support deleting documents typically keep some kind of "tombstone" list that's appropriate to their engine. TIN is no different. TIN keeps a per-segment liveness bitmap, one bit per `ctid`, organized the same way the page-level and offset bitmaps work. When VACUUM runs and determines a `ctid` has been deleted from the heap (as the result of an UPDATE or DELETE), TIN clears that `ctid`'s liveness bit. Groups of pages with at least one cleared bit are marked, and when a query touches a marked page group, TIN also ANDs the offset bitmaps from the postings list against the liveness bitmap, so it never returns or counts a tuple that has truly been deleted.

### [Segments and merging](https://planetscale.com/blog/introducing-tin#segments-and-merging)

When it first creates a new index for a table, TIN creates *n* immutable segments, each containing postings for *1/n* of the pages associated with that table in the heap. As data is changed, TIN creates mutable segments, which are less efficient for searches but allow easy insertion of new documents. Eventually, a background worker promotes each mutable segment to an immutable segment: unchanging, but much more efficient to search.

After a while, TIN will begin to merge immutable segments into larger immutable segments. This also happens in the background.

Text indexing systems that use [sequential document identifiers](https://planetscale.com/blog/introducing-tin#document-identification) are required to renumber all documents when they create a new, merged segment. As mentioned above, document ID `42` in segment 4 is not the same as ID `42` in segment 7. So when segments 4 and 7 are merged, a new numbering must be applied to the combined set of documents and the entirety of each segment's data gets repacked, recompressed, and rewritten. While it's not quite 2× the storage to merge two segments, it can be close.

TIN does not suffer the renumbering problem nor its downstream write-amplification effects.

Because TIN uses Postgres' `ctid` values as its document identifiers, there is nothing to renumber. A posting like `(190, 17)` means the same thing in every segment. Page-level and offset-level bitmaps mean the same thing in every segment. When TIN merges segments, many bitmaps from each old segment can be reused intact in the new segment. They don't have to be recompressed *or even copied*; TIN can simply transfer ownership of bitmaps stored on disk from the old segments to the new one. This reduces write amplification and saves most of the CPU and I/O costs normally associated with merging segments.

## [Summary](https://planetscale.com/blog/introducing-tin#summary)

So that's why TIN is at least 8× faster in every benchmark: the downstream effects of choosing `ctid` as the native format for each posting in the index.

If you want to see how fast TIN is on your text data, read more about the [features](https://planetscale.com/docs/postgres/search) or jump straight to the [getting started guide](https://planetscale.com/docs/postgres/search/get-started). We look forward to seeing what you build with it.
