---
title: 'TIN v1.0.6: 2-3x faster, with support for stemming'
link: https://planetscale.com/blog/tin-v106
source: planetscale-com
published: 2026-10-08T00:00:00Z
updated: 2026-10-08T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Patrick Reynolds
- Eric Ridge
labels:
- engineering
- postgres
summary: TIN v1.0.6 supports stemming in 18 languages and is 2-3x as fast as the GA version
content: extracted
html: 2026-10-08-tin-v1-0-6-2-3x-faster-with-support-for-stemming.html
remote_preview:
  url: https://planetscale.com/assets/tin-v106-social-Cc1d8LiY.png
---

Three weeks ago, we [released TIN](https://planetscale.com/blog/introducing-tin), the fastest full-text search index for Postgres.

Some customers asked for stemming support. Another customer asked for heap attribute tiebreakers in top-k `ORDER BY` clauses. One competitor optimized their full-text index and identified one read-only use case where their index was briefly faster than TIN. So we got to work. The result is [TIN v1.0.4](https://planetscale.com/changelog/tin-1-0-4), released last week, and TIN v1.0.6, which is rolling out to all customers over the next week.

Spoiler alert: TIN is back to being the fastest.

## [Stemming](https://planetscale.com/blog/tin-v106#stemming)

In full-text search, **stemming** means replacing each word with its stem form when indexing and querying. The stem form of a word is often a base word, but not always. For example, the stem of `database` is `databas`. The idea is to map a group of similar words to a single stem. In a search index, for example, if the text we want to index contains the word `running`, we would store an entry for the word `run` in the index. Then, when a user searches for the word `runs`, we would also search for `run`. As a result, a query for any of `run`, `runs`, and `running` would match any document containing any of those three.

Because plural words, noun forms, and verb conjugations vary from one language to another, stemming is language specific. TIN uses the `rust_stemmers` crate, which means that we now support [the same 18 languages](https://github.com/CurrySoftware/rust-stemmers#supported-algorithms) that crate supports, using stemming algorithms from the [Snowball project](https://snowballstem.org). Those languages are, in alphabetical order: Arabic, Armenian, Danish, Dutch, English, French, German, Greek, Hungarian, Italian, Norwegian, Portuguese, Romanian, Russian, Spanish, Swedish, Tamil, and Turkish.

Because stemming changes what content goes into an index, you'll have to rebuild TIN indexes if you want to take advantage of the feature. First, you have to update your TIN to at least v1.0.4. If you do not already have TIN v1.0.4, [update your cluster](https://planetscale.com/docs/postgres/cluster-configuration/updates) to get it. Then rebuild the index. For example:

```sql
ALTER EXTENSION tin UPDATE;
CREATE INDEX CONCURRENTLY new_idx_name ON some_table
  USING tin(some_column) WITH (stemmer = 'en');
DROP INDEX CONCURRENTLY old_idx_name;
```

## [Tiebreakers](https://planetscale.com/blog/tin-v106#tiebreakers)

Normally, a ranked, top-k TIN search looks like this:

```sql
SELECT fields
  FROM table
  WHERE column ==> 'query'
  ORDER BY tin.score(ctid) DESC
  LIMIT 10;
```

For some cases, though, multiple rows have equal scores, and we want a deterministic tiebreaker, like insert time, or just a primary key ID:

```sql
SELECT fields
  FROM table
  WHERE column ==> 'query'
  ORDER BY tin.score(ctid) DESC, created_at DESC, id
  LIMIT 10;
```

In the GA version of TIN, that first query used a block-max algorithm, but the tiebreaker query had to identify and score all documents matching the specified keywords. TIN v1.0.6 uses the same block-max plan for both sorts of queries.

## [Benchmarks, again](https://planetscale.com/blog/tin-v106#benchmarks-again)

Competition is good. It's our goal for TIN to be the fastest Postgres full-text index for all supported use cases. ParadeDB optimized one use case until their extension was faster: BM25-ranked, top-10 queries of read-only data. TIN remained faster at counting queries and [dramatically faster at any workload that involved concurrent writes](https://planetscale.com/blog/introducing-tin#disjunction-queries-with-concurrent-writes), because [ctid-based document identifiers](https://planetscale.com/blog/introducing-tin#document-identification) make both counting and [segment merging](https://planetscale.com/blog/introducing-tin#segments-and-merging) much more efficient than they would be with ordinal document identifiers. We believe TIN's on-disk index format and opportunities for SIMD optimizations meant we could be the fastest at top-k queries, too. So we got to work optimizing top-k queries. Results are below; TIN is the fastest again.

### [Corpus and test environment](https://planetscale.com/blog/tin-v106#corpus-and-test-environment)

After our optimizations, we ran another round of benchmarks with the same benchmark methodology [as before](https://planetscale.com/blog/introducing-tin#workloads-and-corpus), which ParadeDB [also used](https://www.paradedb.com/blog/opening-a-closed-tin) for their tests. It's still the same corpus: 85 GB of questions and answers from Stack Exchange, totaling 150 million documents. It's the same search trace: 1,254 synthetic queries, each interpreted as a disjunction query (match any word), a conjunction query (match all words), and a phrase query (match all words in consecutive order). It's the same EC2 instance: an i7i.8xlarge. It's the same parameters as before: Postgres defaults from [our fork of the ParadeDB Benchmarker](https://github.com/planetscale/paradedb-benchmarker), except for three: `max_parallel_workers` is 8, `shared_buffers` is 24 GB, and `maintenance_work_mem` is 24 GB, to best match the resources of the container. As before, we ran the Benchmarker on the same EC2 instance as the target Postgres server, to ensure that network latency did not impact the measurements. We have increased the resources of the containers on that host in which we run each target Postgres instance; they now have 8 vCPUs and 64 GB of RAM, so that anyone who wants to reproduce the results can use a PlanetScale M-640 i7i database to do it. Finally, we now use CPU pinning in the target container to minimize the effects of CPU migration and contention on measurements.

ParadeDB ran their benchmarks on [version 0.26.0-rc.2](https://www.paradedb.com/blog/opening-a-closed-tin#closing-thoughts) of their extension. Our benchmarks here are based on the more recent, final release version: 0.26.0. Following [ParadeDB's recommendations](https://www.paradedb.com/blog/opening-a-closed-tin#paradedb-syntax), we used the `|||`, `&&&`, and `###` operators specific to disjunction, conjunction, and phrase queries. Our benchmarks for TIN use v1.0.6, which is rolling out to all customers over the course of the next week.

### [Benchmark results](https://planetscale.com/blog/tin-v106#benchmark-results)

We ran the four scenarios from ParadeDB's blog post: conjunction queries, disjunction queries, phrase queries, and a mix of all three types. For all scenarios, the query requests the top ten results by decreasing BM25 score. All four benchmarks have an unchanging table, an index built in one shot, and a clean VACUUM. Aside from the fact that the index is about twice the size of the shared buffers (24 GB), this is basically a best-case scenario for any text-search index.

Each table and graph shows performance for both an Intel CPU ([i7i EC2 instances](https://aws.amazon.com/ec2/instance-types/i7i/) use 5th generation Xeon processors) and an ARM64 CPU ([i8g EC2 instances](https://aws.amazon.com/ec2/instance-types/i8g/) use Graviton4 processors). ARM CPUs are a little bit cheaper, but x86-64 CPUs are significantly faster for text-search workloads, most likely because AVX-512 has wider vector registers and larger SIMD issue width than Neoverse V2. There's enough of a difference from one CPU type to another that we strongly encourage anyone performing benchmarks to disclose the specific instance types (either the EC2 node type, or the CPU model number), for the sake of both consistency and reproducibility.

Throughout the results, "TIN" refers to TIN in its default configuration, which omits certain common terms from BM25 scoring. For "TIN\_FULL" runs, we executed the same queries with BM25 scores based on all query terms. More on that trade-off [below](https://planetscale.com/blog/tin-v106#scoring-common-terms).

**Conjunction queries, top-10 ranked**

```
┌──────────────────────────────────────────────────────────────────┐
│                  QPS    p99    MB/query   QPS    p99    MB/query │
│                │    x86-64 (i7i)        │    ARM64 (i8g)         │
├────────────────┼─────┬───────┬──────────┼─────┬───────┬──────────┤
│ TIN            │ 842 │  57ms │       21 │ 594 │  94ms │       21 │
├────────────────┼─────┼───────┼──────────┼─────┼───────┼──────────┤
│ TIN_FULL       │ 501 │ 117ms │       34 │ 336 │ 195ms │       34 │
├────────────────┼─────┼───────┼──────────┼─────┼───────┼──────────┤
│ ParadeDB       │ 169 │ 253ms │       44 │ 142 │ 278ms │       43 │
└────────────────┴─────┴───────┴──────────┴─────┴───────┴──────────┘
```

**Disjunction queries, top-10 ranked**

```
┌───────────────────────────────────────────────────────────────────┐
│                  QPS     p99    MB/query   QPS    p99    MB/query │
│                │    x86-64 (i7i)         │    ARM64 (i8g)         │
├────────────────┼─────┬────────┬──────────┼─────┬───────┬──────────┤
│ TIN            │ 236 │  132ms │       31 │ 143 │ 243ms │       31 │
├────────────────┼─────┼────────┼──────────┼─────┼───────┼──────────┤
│ TIN_FULL       │  85 │  331ms │      106 │  58 │ 548ms │      106 │
├────────────────┼─────┼────────┼──────────┼─────┼───────┼──────────┤
│ ParadeDB       │  65 │  532ms │      128 │  45 │ 684ms │      126 │
└────────────────┴─────┴────────┴──────────┴─────┴───────┴──────────┘
```

**Phrase queries, top-10 ranked**

```
┌────────────────────────────────────────────────────────────────────┐
│                  QPS     p99    MB/query   QPS     p99    MB/query │
│                │    x86-64 (i7i)         │    ARM64 (i8g)          │
├────────────────┼─────┬────────┬──────────┼─────┬────────┬──────────┤
│ TIN            │ 514 │  120ms │       28 │ 381 │  202ms │       28 │
├────────────────┼─────┼────────┼──────────┼─────┼────────┼──────────┤
│ TIN_FULL       │ 386 │  157ms │       37 │ 284 │  246ms │       37 │
├────────────────┼─────┼────────┼──────────┼─────┼────────┼──────────┤
│ ParadeDB       │ 166 │  327ms │       73 │ 122 │  415ms │       72 │
└────────────────┴─────┴────────┴──────────┴─────┴────────┴──────────┘
```

**Conjunction, disjunction, and phrase queries, top-10 ranked**

```
┌───────────────────────────────────────────────────────────────────┐
│                         QPS     p99    MB/q   QPS    p99     MB/q │
│                       │    x86-64 (i7i)     │    ARM64 (i8g)      │
├───────────────────────┼─────┬────────┬──────┼─────┬────────┬──────┤
│ TIN                   │ 413 │  119ms │   27 │ 265 │  218ms │   27 │
├───────────────────────┼─────┼────────┼──────┼─────┼────────┼──────┤
│ TIN_FULL              │ 180 │  275ms │   59 │ 125 │  444ms │   59 │
├───────────────────────┼─────┼────────┼──────┼─────┼────────┼──────┤
│ TIN v1.0.2            │ 110 │  643ms │   64 │ 135 │  435ms │   63 │
├───────────────────────┼─────┼────────┼──────┼─────┼────────┼──────┤
│ TIN_FULL v1.0.2       │  36 │ 2150ms │  128 │  44 │ 1743ms │  130 │
├───────────────────────┼─────┼────────┼──────┼─────┼────────┼──────┤
│ ParadeDB              │  98 │  442ms │   81 │  79 │  548ms │   80 │
└───────────────────────┴─────┴────────┴──────┴─────┴────────┴──────┘
```

### [Scoring common terms](https://planetscale.com/blog/tin-v106#scoring-common-terms)

The discussion in ParadeDB's benchmark post [takes issue with](https://www.paradedb.com/blog/opening-a-closed-tin#handling-common-terms) an important TIN optimization: omitting terms that appear in more than 10% of documents from BM25 scores. We believe that's a good trade-off, which is why we made it the default. But if you need BM25 scores to include all the words in your query, that's tunable at query time: order by `tin.full_score(ctid)`. If you want to omit scores for some terms, but at a different threshold than the 10% default, make your queries order by `tin.score(ctid, dense_ratio => F)`, where F is the fraction (between 0 and 1) of documents below which a term will be included in scoring.

Terms that appear in more than 10% of documents do not affect BM25 scores very much, since each term's score increases with the [inverse document frequency](https://en.wikipedia.org/wiki/Okapi_BM25#The_ranking_function), or rarity, of the term. But it takes a lot of CPU time to include those common terms in the scores. TIN still ensures that conjunctions, disjunctions, and phrases match as usual; the scoring optimization only affects the scores.

Does TIN's version of scoring still qualify as BM25? That's a bit of a semantic question. Most text indexes omit a fixed list of common words, called stop words, from the index, which removes the ability to search for those terms or use them for scoring. TIN doesn't. Instead, it includes all words in the index and in search logic, and it omits a configurable, corpus-specific set from scoring. The words TIN scores use the standard BM25 formula.

For completeness, we included both scoring approaches in today's benchmarks. TIN is faster than TIN\_FULL, as expected; both are once again faster than ParadeDB.

### [Free lunch, or lack thereof](https://planetscale.com/blog/tin-v106#free-lunch-or-lack-thereof)

One of the optimizations in ParadeDB 0.26.0 increases the index size from 52.1 GB to 67.3 GB for the 85 GB Stack Exchange corpus, an increase of 29%. Benefiting from that optimization requires customers to rebuild all their text indexes. TIN v1.0.6 is a drop-in replacement; the index format did not change or grow, and customers do not need to take any action.

## [Summary](https://planetscale.com/blog/tin-v106#summary)

TIN is once again the fastest Postgres full-text index. When used with its default scoring methodology, TIN has 3.1-5.0x higher throughput and 2.7-4.4x lower p99 latency than ParadeDB on x86-64; on ARM64, it has 3.1-4.2x higher throughput and 2.1-3.0x lower p99 latency. For queries that use `tin.full_score`, TIN has 1.3-3.0x higher throughput and 1.6-2.2x lower p99 latency on x86-64, and 1.3-2.4x higher throughput and 1.2-1.7x lower p99 latency on ARM64.

When creating a new database, consider an x86-64 instance with AVX-512 support if text search is a large part of your workload.

Get started with TIN [here](https://planetscale.com/docs/postgres/search/get-started).
