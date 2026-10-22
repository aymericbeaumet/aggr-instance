---
title: 'Pg_vectorsearch: An index that is faster than pgvector''s IVFFlat and HNSW'
link: https://github.com/timescale/pg_vectorsearch
source: hnrss-org
published: 2026-10-14T11:47:53Z
updated: 2026-10-14T11:47:53Z
first_seen: 2026-10-14T17:58:19.452435153Z
authors:
- jascha_eng
content: extracted
html: 2026-10-14-pg-vectorsearch-an-index-that-is-faster-than-pgvector-s.html
extra:
  comments_url: https://news.ycombinator.com/item?id=50086165
  num_comments: 42
  points: 144
---

[![Build](https://github.com/timescale/pg_vectorsearch/actions/workflows/build.yml/badge.svg?branch=main&event=push)](https://github.com/timescale/pg_vectorsearch/actions/workflows/build.yml?query=branch%3Amain+event%3Apush) [![Coverage](https://camo.githubusercontent.com/94d6957c816eb3e23de12a7667353594703591c1e342e0ec2e23e4dde4b807f0/68747470733a2f2f696d672e736869656c64732e696f2f656e64706f696e743f75726c3d68747470733a2f2f74696d657363616c652e6769746875622e696f2f70675f766563746f727365617263682f636f7665726167652d62616467652e6a736f6e)](https://timescale.github.io/pg_vectorsearch/coverage/) [![Coverity](https://camo.githubusercontent.com/b14b414026fcae54c331173f9b694d0fd26adaa1cba9e00a20df430d217ab016/68747470733a2f2f7363616e2e636f7665726974792e636f6d2f70726f6a656374732f74696d657363616c652d70675f766563746f727365617263682f62616467652e737667)](https://scan.coverity.com/projects/timescale-pg_vectorsearch)

A PostgreSQL extension for high-performance approximate nearest neighbor (ANN) search.

> **Status: alpha.** Meant for testing, not production. Upgrading between prereleases may not be possible; plan to drop and recreate the extension and its indexes. See the [changelog](https://github.com/timescale/pg_vectorsearch/blob/main/CHANGELOG.md) for known issues.

- **PRISM index** (100M+ vectors) — IVF-style, with hierarchical routing
- **RaBitQ quantization** with theoretical error bounds for two-stage search
- **SIMD-optimized distance computation** (AVX2, AVX512, NEON)
- **Dynamic updates** — inserts, updates and deletes, with posting-list splits via `prism_rebalance` (see [Maintenance](https://github.com/timescale/pg_vectorsearch#maintenance))
- **pgvector compatible** — index existing `vector` and `halfvec` columns as-is (see [Existing pgvector tables](https://github.com/timescale/pg_vectorsearch#existing-pgvector-tables))

## PRISM Index

[![PRISM](https://github.com/timescale/pg_vectorsearch/raw/main/images/prism-logo.jpg)](https://github.com/timescale/pg_vectorsearch/blob/main/images/prism-logo.jpg)

PRISM (Partitioned Routing Index for Similarity Matching) is a PostgreSQL-native index access method that is conceptually similar to [pgvector](https://github.com/pgvector/pgvector)'s IVFFlat, but it is built around state-of-the-art quantization, routing, and partitioning techniques. It is implemented in C for high performance and seamless integration with PostgreSQL.

The result: an [index that is significantly faster](https://www.tigerdata.com/blog/announcing-pg_vectorsearch-scalable-open-source-vector-search-postgresql) than pgvector's IVFFlat and HNSW, for both querying and index building. In fact, compared to graph-based HNSW, PRISM is faster at equivalent recall on a 100-million-vector dataset, on an instance with a quarter of the memory.

To achieve this performance, PRISM uses a hierarchical centroid tree to partition vectors into clusters, and relies on the PostgreSQL buffer cache to keep that tree warm in memory. Vectors are [RaBitQ](https://github.com/gaoj0017/RaBitQ)-quantized and stored in posting lists with a page layout optimized for SIMD distance computation. The RaBitQ encoding carries error bounds that let a search rule out vectors that cannot possibly be nearest neighbors, so far fewer of them need reranking against the full-precision vectors stored in the table.

```
Query Flow:
1. Traverse centroid tree to find nearest clusters (buffer cache)
2. Scan posting lists for candidates (buffer cache, async I/O)
3. RaBitQ two-stage filtering: estimate → error bound → rerank
4. Full-precision reranking from heap
5. Return k nearest neighbors
```

For detailed architecture, see [docs/architecture.md](https://github.com/timescale/pg_vectorsearch/blob/main/docs/architecture.md).

## Usage

Install the extension first; see [Building](https://github.com/timescale/pg_vectorsearch#building).

There are two ways to use a PRISM index. Pick the one that matches where your vectors live:

- **[Native types](https://github.com/timescale/pg_vectorsearch#native-types).** Starting fresh, or not using pgvector? Store vectors in `pg_vectorsearch`'s own `vec32` (float32) or `vec16` (float16) columns. No other extension is needed.
- **[Existing pgvector tables](https://github.com/timescale/pg_vectorsearch#existing-pgvector-tables).** Already have `vector` or `halfvec` columns? Install `pg_vectorsearch` next to pgvector and build a PRISM index on those columns as they are. Your table and queries stay unchanged.

Both paths build the same index and use the same operators (`<->`, `<#>`, `<=>`) and [tuning](https://github.com/timescale/pg_vectorsearch#tuning) knobs.

### Native types

```sql
CREATE EXTENSION pg_vectorsearch;

-- PLAIN keeps the vector inline. EXTERNAL (the default) toasts a value
-- over ~2 kB, and rerank then pays an extra fetch the planner does not cost.
CREATE TABLE items (
    id serial PRIMARY KEY,
    embedding vec32(3) STORAGE PLAIN
);

-- Load first; the index is sized from the row count at CREATE INDEX.
-- 500 rows is enough for the planner to use the index.
INSERT INTO items (embedding)
SELECT ARRAY[i::real, (i % 7)::real, (i % 5)::real]::vec32
FROM generate_series(1, 1000) i;

-- For inner product or cosine, see Operator classes below.
CREATE INDEX ON items USING prism (embedding vec32_l2_ops);

-- 0 = auto (~0.95 recall). Raise for recall, lower for speed.
SET prism.nprobe = 40;

SELECT * FROM items
ORDER BY embedding <-> '[3,1,2]'
LIMIT 5;
```

Use `vec16` instead of `vec32` to halve the space vectors take in the table, with the matching `vec16_*` [operator classes](https://github.com/timescale/pg_vectorsearch#operator-classes).

### Existing pgvector tables

If you already have a table with pgvector's `vector` or `halfvec` columns, you can build a PRISM index on it directly. Nothing about the table or your queries needs to change. `vec32` and `vec16` are binary-compatible with pgvector's `vector` and `halfvec` but have different names, so the two extensions can be installed side by side, even in the same schema.

```sql
-- The table you already have, included so the example runs on its own.
CREATE EXTENSION vector;

CREATE TABLE documents (
    id        bigserial PRIMARY KEY,
    body      text,
    embedding vector(384)
);
CREATE INDEX documents_embedding_hnsw
    ON documents USING hnsw (embedding vector_l2_ops);

-- Install pg_vectorsearch next to pgvector and build the index on the
-- existing column. The column is not rewritten or copied.
CREATE EXTENSION pg_vectorsearch;

CREATE INDEX documents_embedding_prism
    ON documents USING prism (embedding vec32_l2_ops);

-- Your queries keep working as before. PRISM is normally faster, so
-- the planner should prefer it over the HNSW index while both exist.
-- Verify with an EXPLAIN:
EXPLAIN (COSTS OFF)
SELECT id, body
FROM documents
ORDER BY embedding <-> (SELECT embedding FROM documents WHERE id = 42)
LIMIT 10;
--  Limit
--    InitPlan 1
--      ->  Index Scan using documents_pkey on documents documents_1
--            Index Cond: (id = 42)
--    ->  Index Scan using documents_embedding_prism on documents
--          Order By: (embedding <-> (InitPlan 1).col1)

-- Once you are happy with it, drop the HNSW index.
DROP INDEX documents_embedding_hnsw;
```

Use the PRISM [operator class](https://github.com/timescale/pg_vectorsearch#operator-classes) for the same distance metric as your HNSW index.

### Operator classes

Pick the operator class for your distance metric and column type. L2 is the default.

| Distance      | Operator | `vec32` / `vector` | `vec16` / `halfvec` |
| ------------- | -------- | ------------------ | ------------------- |
| L2            | `<->`    | `vec32_l2_ops`     | `vec16_l2_ops`      |
| Inner product | `<#>`    | `vec32_ip_ops`     | `vec16_ip_ops`      |
| Cosine        | `<=>`    | `vec32_cosine_ops` | `vec16_cosine_ops`  |

When replacing an HNSW index, pgvector's `vector_<metric>_ops` maps to `vec32_<metric>_ops` and `halfvec_<metric>_ops` to `vec16_<metric>_ops`.

### Tuning

Defaults are tuned from large-scale benchmarks; most deployments only ever adjust `prism.nprobe`. See the [tuning guide](https://github.com/timescale/pg_vectorsearch/blob/main/docs/tuning.md) for every index parameter and GUC, their tradeoffs, and when changing them makes sense.

### Maintenance

An insert appends to whichever posting list its vector routes to, so lists grow as rows arrive and never split on their own. Besides `REINDEX`, the `prism_rebalance` procedure splits oversized lists in place. See the [maintenance guide](https://github.com/timescale/pg_vectorsearch/blob/main/docs/maintenance.md) for how it works and its current limitations.

## Building

**Requirements:**

- PostgreSQL 18+ (with development headers)
- GNU C11 compiler (GCC 11+ or Clang 14+)
- Meson build system
- Optional: CBLAS, for faster RaBitQ encoding. See [Development](https://github.com/timescale/pg_vectorsearch/blob/main/docs/development.md#blas).

```shell
# Release build
meson setup builddir --buildtype=release

# Build and install to PostgreSQL
meson install -C builddir
```

See the [development guide](https://github.com/timescale/pg_vectorsearch/blob/main/docs/development.md#build-options) for build options.

## Documentation

- [Tuning](https://github.com/timescale/pg_vectorsearch/blob/main/docs/tuning.md) - Index parameters, GUCs, tradeoffs, defaults
- [Maintenance](https://github.com/timescale/pg_vectorsearch/blob/main/docs/maintenance.md) - Rebalancing and splitting posting lists
- [Architecture](https://github.com/timescale/pg_vectorsearch/blob/main/docs/architecture.md) - High-level design and data structures
- [SIMD](https://github.com/timescale/pg_vectorsearch/blob/main/docs/simd.md) - SIMD build options and distance computation
- [Development](https://github.com/timescale/pg_vectorsearch/blob/main/docs/development.md) - Build options, testing, CI scripts

## References

### Research

- [RaBitQ](https://github.com/gaoj0017/RaBitQ) - Binary quantization with theoretical error bounds (SIGMOD 2024)
- [SPFresh](https://dl.acm.org/doi/10.1145/3600006.3613166) - LIRE protocol for incremental updates (SOSP 2023)
- [SPANN](https://www.microsoft.com/en-us/research/wp-content/uploads/2021/11/SPANN_finalversion1.pdf) - Billion-scale ANN with boundary replication (NeurIPS 2021)
- [ScaNN for AlloyDB](https://services.google.com/fh/files/misc/scann_for_alloydb_whitepaper.pdf) - Hierarchical clustering (Google)

### Related Projects

- [pgvector](https://github.com/pgvector/pgvector) - Vector type and operators for PostgreSQL
- [pgvectorscale](https://github.com/timescale/pgvectorscale) - DiskANN-based vector index for PostgreSQL

## License

[The PostgreSQL License](https://github.com/timescale/pg_vectorsearch/blob/main/LICENSE)
