---
title: When to choose x86-64 vs aarch64
link: https://planetscale.com/blog/when-to-choose-x86-64-vs-aarch64
source: planetscale-com
published: 2026-09-25T00:00:00Z
updated: 2026-09-25T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Ahmed Darwich
labels:
- engineering
- neki
- postgres
summary: ARM and x86 can give you different performance at the same vCPU count. Here's what differs, what to measure, and how to choose.
content: extracted
html: 2026-09-25-when-to-choose-x86-64-vs-aarch64.html
remote_preview:
  url: https://planetscale.com/assets/when-to-choose-x86-64-vs-aarch64-social-D9ZrLOED.png
---

Not all cloud vCPUs are created equal.

When you create a PlanetScale Postgres or Neki database, you have to choose between `aarch64` (ARM) and `x86-64`. Two clusters on different architectures can have the same vCPU count and RAM, yet perform very differently.

It's worth understanding the implications, since you can't easily switch the CPU architecture on your cluster later.

## [ARM and x86 were built for different problems](https://planetscale.com/blog/when-to-choose-x86-64-vs-aarch64#arm-and-x86-were-built-for-different-problems)

x86 grew up on the desktop, prioritizing backward compatibility and performance. It began at Intel in 1978 and IBM’s decision to use an x86 processor in the original PC helped make it a fixture in personal computers.

Acorn, the British computer company that developed the first ARM processor, wanted to keep manufacturing costs down by using a cheap plastic package for the chip. To keep it from overheating, the team set a power budget of one watt.

When Acorn’s engineers first powered up the chip in April 1985, it worked, but the [power supply meter read zero](https://www.theregister.com/on-prem/2012/05/03/arm-creators-sophie-wilson-and-steve-furber/616100). A fault on the test board meant the chip wasn't connected to its power supply at all. It was running on current leaking in from the surrounding circuits. Once connected properly, it drew about a tenth of a watt, well under the one-watt budget. ARM designer Sophie Wilson later called that efficiency [a complete accident](https://arstechnica.com/gadgets/2022/09/a-history-of-arm-part-1-building-the-first-chip/).

ARM’s low power consumption made it a good fit for phones. As datacenter operators looked for ways to cut electricity and cooling costs, that efficiency became attractive for servers too. AWS and Google now build their own ARM processors for cloud servers: Graviton and Axion, respectively.

## [How CPU architecture affects Postgres](https://planetscale.com/blog/when-to-choose-x86-64-vs-aarch64#how-cpu-architecture-affects-postgres)

For your database, the differences start with what the cloud provider counts as a vCPU.

### [What is hyperthreading?](https://planetscale.com/blog/when-to-choose-x86-64-vs-aarch64#what-is-hyperthreading)

A CPU core has several execution units, but a single thread rarely keeps them all busy. Often it's stalled, waiting on data from memory.

Hyperthreading lets one core keep two threads loaded at once. When one stalls, the core runs instructions from the other.

Think of two cooks in one kitchen. They can get more food out, but they're sharing the oven and the sink. On x86 instances with hyperthreading, the cloud sells each thread as a vCPU.

ARM server chips like AWS Graviton and Google Axion don't use hyperthreading. Each vCPU is a whole core. Each has its own oven and sink.

### [Many workers, or one heavy query](https://planetscale.com/blog/when-to-choose-x86-64-vs-aarch64#many-workers-or-one-heavy-query)

This is where ARM does well: those extra cores help when many queries arrive at once. An online store, for example, might be fetching products, checking stock, and updating shopping carts for hundreds of customers. Each query is small, but the database can spread them across its cores.

Those cores also leave room for parallel query workers and autovacuum, Postgres’s background cleanup, to run alongside application queries.

Now consider a reporting dashboard where one query spends several seconds calculating totals across millions of sales records. If that query is CPU-bound and doesn’t use parallel workers, extra cores won’t make it finish sooner. Here the speed of a single core matters more, and that's where x86 tends to have the edge. x86 server chips usually run at a higher clock speed, the rate at which a core works through instructions, so one core can get through a single query faster.

### [Wide vector registers](https://planetscale.com/blog/when-to-choose-x86-64-vs-aarch64#wide-vector-registers)

A core’s performance also depends on how much work it can do per instruction. Some instructions process several values at once.

Suppose you need to compare two lists of 32-bit numbers. A vector instruction can compare several pairs in one step: four with 128-bit vectors, eight with 256-bit vectors, or sixteen with 512-bit vectors.

A wider vector lets one instruction process more data. That can mean fewer instructions to do the same work.

On x86, AVX2 is a set of vector instructions that works with 256-bit vectors. Some chips also support AVX-512, which extends that width to 512 bits. AWS’s ARM server chips, [Graviton4 and Graviton5, use 128-bit vectors](https://aws.github.io/graviton/).

[TIN, PlanetScale’s full-text search index for Postgres](https://planetscale.com/blog/introducing-tin), uses vector instructions to compare bitmaps. Each page-level bitmap contains 256 bits, with each bit recording whether a database page contains a search term.

For an `articles` table with a text column called `body`, we can create a TIN index and search for articles containing both “postgres” and “replication”:

```sql
CREATE INDEX articles_body_tin ON articles USING tin (body);

SELECT *
FROM articles
WHERE body ==> 'postgres AND replication';
```

The `==>` operator performs the TIN search. TIN intersects the two terms’ bitmaps to find pages that could contain both words. On x86, it can intersect all 256 bits with one AVX2 `AND`.

`pgvector`, a Postgres extension for vector similarity search, has some optimizations specific to x86 as well. Its [AVX-512 code](https://github.com/pgvector/pgvector/blob/efa08fda9ec485d80292d0487a77939c087dedcc/src/bitutils.c) calculates Hamming and Jaccard distances, which measure how two bit vectors differ or overlap. On ARM, those calculations use its general-purpose code. So for some index types, x86 has a distinct advantage.

## [Why you can’t change architectures on an existing cluster](https://planetscale.com/blog/when-to-choose-x86-64-vs-aarch64#why-you-cant-change-architectures-on-an-existing-cluster)

Postgres runs on both architectures, but the files it writes to disk aren't portable between them.

The usual way to move a Postgres database to new hardware is to set up a replica there and then promote it to primary. That doesn't work across architectures, because of how replicas are built.

A replica doesn't rebuild the database from scratch. It starts with a byte-for-byte copy of the files the primary keeps on disk, including its tables and indexes. Then it replays the write-ahead log, a running record of every change, to stay up to date. Differences in how the two machines interpret those files can produce incorrect results.

`pg_trgm`, Postgres's extension for fast text matching and similarity searches, shows how subtle this can get. It wasn't designed with moving its indexes between architectures in mind.

Postgres is written in C, where the compiler decides whether a plain `char` is signed or unsigned. GCC and Clang, two common C compilers, default to signed `char` on x86-64 Linux and unsigned `char` on ARM64 Linux. So the same byte, `0xFF`, means −1 on one and 255 on the other.

`pg_trgm` uses `char` comparisons to sort trigrams (groups of three characters) inside its indexes. Copy the index to the other architecture, and the new machine could disagree with its ordering.

[Postgres addressed this in version 18](https://www.postgresql.org/message-id/E1tlXkn-000VZr-2D%40gemulon.postgresql.org). But it's a good illustration of the problem: copying the bytes doesn't guarantee both machines agree on what they mean.

PlanetScale keeps the primary and its replicas on the same CPU architecture. In Neki, the architecture is fixed in the configuration profile used by the shards.

None of this affects everyday changes. Resizing a cluster or adding replicas keeps the same architecture, so those work as usual.

If you do decide to switch architectures, you'll need to migrate to a new cluster. You set up the tables and indexes there, then use logical replication to copy the rows and keep up with incoming writes. The destination then builds its own index entries from those rows, so the ordering matches its comparison rules.

## [Which one to pick](https://planetscale.com/blog/when-to-choose-x86-64-vs-aarch64#which-one-to-pick)

For a new application doing ordinary reads and writes, ARM is a good default when creating the cluster. It has a great price:performance ratio, and handles parallelism nicely. On PlanetScale, ARM sizes usually cost a bit less than x86 sizes with the same vCPUs and memory, though it varies by size and region. See [pricing](https://planetscale.com/docs/postgres/pricing) for current rates.

Give x86 a closer look if a few CPU-heavy queries account for most of your workload.

If performance really matters for your workload and you want to pick the best architecture for it, run it on separate ARM and x86 clusters before committing to either. Keep the data and settings the same, and test at the concurrency you expect during busy periods. Compare throughput, slower-query response times, and cost.

Moving an existing PlanetScale database to the other architecture requires a migration, so make sure the performance or cost improvement is worth that work.
