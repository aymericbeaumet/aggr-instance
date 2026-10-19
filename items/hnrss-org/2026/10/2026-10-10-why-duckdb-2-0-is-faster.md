---
title: Why DuckDB 2.0 is faster
link: https://motherduck.com/blog/why-duckdb-20-is-faster/
source: hnrss-org
published: 2026-10-10T18:08:49Z
updated: 2026-10-10T18:08:49Z
first_seen: 2026-10-10T23:27:28.788969945Z
authors:
- tosh
content: extracted
html: 2026-10-10-why-duckdb-2-0-is-faster.html
extra:
  comments_url: https://news.ycombinator.com/item?id=50035530
  num_comments: 34
  points: 109
---

DuckDB 2.0 is coming this fall and the alpha is out! I ran the interesting features on my own laptop, and against S3, to see what actually changes for people who build tables and pipelines rather than database engines.

Because yes, DuckDB 2.0 is faster. But to get the speed bump you need to understand how your data is shaped, and sometimes how to model it.

This post covers the three features I think matter most, with the numbers I got, plus a few hidden gems I found in the commit logs.

Every number below is from one machine (an M5 laptop) and my home internet, which slows both versions about equally. Run your own before quoting them ;)

Let's start with the easiest and most exciting one: async I/O.

## Async I/O: querying over AWS S3 is much faster

This is the one I like most, because nothing in your query changes. Here is a query that reads a 2.2 GB Parquet file on S3 (Stack Overflow votes, 228 million rows, 2268 row groups) and counts votes per type. It reads one column out of four, about 230 MB.

```sql
Copy codeCREATE SECRET s3 (TYPE s3, PROVIDER credential_chain, REGION 'us-east-1');
SET enable_external_file_cache = false;   -- so every run really hits S3

SELECT VoteTypeId, count(*) AS n
FROM read_parquet('s3://us-prd-motherduck-open-datasets/stackoverflow/parquet/2023-05/votes.parquet')
GROUP BY ALL ORDER BY 1;
```

| Same query, same laptop |        |
| ----------------------- | ------ |
| DuckDB 1.5.5            | 18.8 s |
| DuckDB 2.0 alpha        | 7.7 s  |

Quick caveat: as I said in the introduction, this is my home internet to us-east-1, so both numbers are slow. Expect things to go faster if you run this from cloud compute.

So what's the black magic here? The file is cut into 2268 row groups of about 122,000 rows. For every row group DuckDB downloads the bytes, decodes the Parquet, counts votes per type, and merges the partial counts at the end. Two kinds of work: waiting on the network, and crunching on the CPU.

In 1.5.5 each of the 18 workers does both jobs in turn: download, wait, decode, download, wait. While a worker waits its CPU is idle, while it decodes it has no download in flight, and you never get more than 18 downloads going.

In 2.0 a separate pool of threads only downloads, keeping dozens of row groups in flight and parking the bytes in a buffer. The workers only decode, and there is always a row group ready for them. **Network and CPU are busy at the same time.**

One setting drives this: `read_ahead_depth`, how many row groups the download pool may fetch ahead of the workers. It defaults to `-1` (automatic, sized from your thread count), so **async I/O is on out of the box**. Set it to `0` and you get the 1.5 behaviour back.

| S3 read                                     | 1.5.5  | 2.0 alpha |
| ------------------------------------------- | ------ | --------- |
| One 2.2 GB Parquet file, one column         | 18.8 s | 7.7 s     |
| 23 large Parquet files, 13.6 GB, one column | 11.8 s | 3.9 s     |
| One 1.7 GB plain CSV                        | 116 s  | 55 s      |
| 30 tiny Parquet files, about 1 MB each      | 3.7 s  | 3.3 s     |

One comment on the tiny files: no meaningful change, because the time there is per-file round trips (footer, then data) that reading ahead cannot remove. Storing a lake as thousands of 1 MB Parquet files is a bad practice anyway, and 2.0 does not rescue it. Fundamentals still matter!

**TL;DR**: **reading data over S3 is 2x to 3x faster in 2.0 with zero query changes**, because a separate pool downloads ahead of the workers. It is on by default (`read_ahead_depth = -1`); `0` gives you the old behaviour.

## Recursive CTEs: a boost for deep parent/child datasets

The DuckDB team rewrote the recursive CTE engine and claims 40x on graph reachability. Don't worry, I'll explain what that means on a table you already know.

A recursive CTE is a loop over a table. Take an `employees` table with two columns, `manager` and `employee`. You want to answer a simple question: who reports to whom.

| manager | employee |
| ------- | -------- |
| Ana     | Ben      |
| Ana     | Cléa     |
| Ben     | Dev      |
| Ben     | Eli      |
| Cléa    | Fay      |
| Dev     | Gus      |
| Fay     | Hal      |

To do that, you start with one row: Ana, the CEO. Round one, find everyone whose manager is Ana (Ben, Cléa). Round two, find everyone whose manager is one of those people (Dev, Eli, Fay). Keep going until a round finds nobody new. Every level of the org chart is one round.

The query looks like this:

```sql
Copy codeWITH RECURSIVE team(person) AS (
    SELECT 'Ana'
  UNION
    SELECT e.employee FROM team t JOIN employees e ON e.manager = t.person
)
SELECT count(*) FROM team;
```

Org chart, folder tree, bill of materials, reply thread, data lineage, git history: these are all kinds of data where the table is often the same two columns, parent and child. The only thing that differs is how deep it goes, and the depth is the number of rounds for the query. An org chart is maybe eight levels. A git history is tens of thousands.

Here is the problem 1.5 had. Every round, it went back and re-read the whole table to find the next level. Eight levels mean eight full reads. Thousands of levels, thousands of full reads of the same table. In 2.0 the table is read once, a lookup on the parent column is built once, and each round only looks up the few rows it just found. **The cost is now about the rows you actually touch**, not rounds times table size.

Coming back to git history, that's typically where you will see the boost. Every commit points to its parent, so the table is just `commit_id, parent_id`, and walking the ancestry of HEAD is one round per commit. I generated a 20,000-commit repo with a few merges and walked it back to the root with a recursive CTE like this:

```sql
Copy codeWITH RECURSIVE ancestors(id) AS (
    SELECT max(id) FROM commits              -- HEAD
  UNION
    SELECT p.parent_id FROM ancestors a JOIN commit_parents p ON p.commit_id = a.id
)
SELECT count(*) FROM ancestors;
```

And the query times:

| Ancestry walk, 20,000 commits |                           |
| ----------------------------- | ------------------------- |
| DuckDB 1.5.5                  | 1.8 s to 16 s across runs |
| DuckDB 2.0 alpha              | 0.10 s, every run         |

**TL;DR**: if you walk deep parent/child chains (git history, lineage, reply threads, a full bill of materials), **2.0 turns a job you used to push to a graph database into a normal query**. If your hierarchy is shallow, like an org chart, you won't see much. Either way, keep the hierarchy as one parent/child table with integer ids, and reach for `USING KEY` when the recursion carries a value like depth or cost.

## VARIANT: smaller and faster than JSON

Everybody loves JSON. VARIANT is now a first-class data type in DuckDB, and the word to remember is shredding.

Not the guitar kind. Shredding in our VARIANT context means: when DuckDB writes a row group to disk, it looks at your JSON column and finds the fields that show up in most rows with the same kind of value every time.

Here is a common example, one event out of five million:

```json
Copy code{"type": "purchase", "user": {"id": 42, "country": "FR", "premium": true},
 "props": {"amount": 12.5, "currency": "EUR", "items": 2}, "tags": ["a", "b"]}
```

The event kind is always text, `user.id` is always a number, `props.amount` is always a decimal. Those **fields get pulled out into their own real columns under the hood**. The rare fields, and the fields that are a number in one row and text in the next, stay together in a binary remainder. So the consistent part of your JSON is stored like a normal table, and only the messy part is stored as a blob.

The perfect case is structured logs. `level`, `service`, `latency_ms`, `trace_id` are in every line and always the same kind of value, so they all shred. The odd `extra` object stays in the remainder, still queryable, just slower. The trap: a `latency_ms` that is `231` in one line and `"231ms"` in the next falls into the remainder too. Keep value kinds consistent.

VARIANT is not only about speed. Text is greedy on storage as much as on CPU. I took the five million events and stored the same data three ways:

```sql
Copy code-- A: the JSON string
CREATE TABLE ev AS SELECT json AS payload FROM read_ndjson_objects('events.ndjson');
-- B: VARIANT
CREATE TABLE ev AS SELECT json::VARIANT AS payload FROM read_ndjson_objects('events.ndjson');
-- C: a normal table, one typed column per field
CREATE TABLE ev AS SELECT * FROM read_json('events.ndjson');
```

Then three queries on each: a filter on two fields, a sum of a numeric field grouped by country, and a list lookup.

```sql
Copy code-- Q1: filter on two fields
SELECT count(*) FROM ev
WHERE payload.type::VARCHAR = 'purchase' AND payload.user.country::VARCHAR = 'FR';

-- Q2: sum of a numeric sub-field, grouped
SELECT payload.user.country::VARCHAR AS country, sum(payload.props.amount::DOUBLE) AS amount
FROM ev WHERE payload.type::VARCHAR = 'purchase' GROUP BY ALL ORDER BY 1;

-- Q3: list lookup
SELECT count(*) FROM ev WHERE list_contains(payload.tags::VARCHAR[], 'c');
```

|                   | JSON string | VARIANT (2.0 alpha) | VARIANT (1.5.5) | One typed column per field |
| ----------------- | ----------- | ------------------- | --------------- | -------------------------- |
| On disk           | 224 MB      | 85 MB               | 81 MB           | 45 MB                      |
| Q1 filter         | 366 ms      | 63 ms               | 4.96 s          | 52 ms                      |
| Q2 sum by country | 408 ms      | 61 ms               | 5.28 s          | 51 ms                      |
| Q3 list contains  | 357 ms      | 1.96 s              | 4.65 s          | 67 ms                      |

What can we see here?

- VARIANT is **2.7 times smaller than the JSON string**, and you can see why with `pragma_storage_info('ev')`: the object is split into sub-columns, the event kind is stored as a dictionary. `EXPLAIN` shows the filter pushed into the scan, so a query on `payload.type` reads one sub-column.
- On the queries that touch shredded fields, a filter or a sum on a numeric sub-field, **VARIANT is about 6 times faster than parsing the JSON text** and within 20 percent of the typed columns. Compared to VARIANT in 1.5.5 it is 78 times faster, because 1.5.5 had the type but not the shredding.
- The list query is the exception. Casting a VARIANT list to `VARCHAR[]` costs two seconds in this alpha, slower than the JSON path. Field access is where shredding pays today; lists are not there yet.

So the golden rule of modeling is still valid: **model what you know**. The fields every query touches deserve real columns, and promoting one is two statements:

```sql
Copy codeALTER TABLE ev ADD COLUMN type VARCHAR;
UPDATE ev SET type = payload.type::VARCHAR;
```

**TL;DR**: if your events share a consistent set of fields with consistent value kinds, **store them as VARIANT rather than a JSON string**. You get a third of the storage and field queries that run like real columns. Promote the fields every query touches to real columns, keep the long tail in the VARIANT, and avoid list casts in hot queries for now.

## Quick hits from the release, and from the commit log

**Triggers**. The table runs a bit of SQL by itself when rows change. For instance, update some prices, and the trigger sees each row before and after the change (the "transition tables") and writes both into a history table. Before, every tool that touched the table had to remember to write that history row somewhere. Now it can be done directly on the database side.

```sql
Copy codeCREATE TRIGGER price_history AFTER UPDATE ON prices
REFERENCING OLD TABLE AS before_rows NEW TABLE AS after_rows FOR EACH STATEMENT
  INSERT INTO price_history (id, old_price, new_price)
  SELECT a.id, b.price, a.price FROM before_rows b JOIN after_rows a USING (id);
```

**Nested schemas**: `CREATE SCHEMA finance.reports;` and tables inside it.

**DML in a CTE**: a `DELETE ... RETURNING` inside a `WITH`, then `INSERT` from it, which is the move-rows-atomically pattern.

```sql
Copy codeWITH moved AS MATERIALIZED (DELETE FROM staging RETURNING *)
INSERT INTO prod SELECT * FROM moved;
```

Other fun things:

- `SET dialect_compatibility_mode = 'spark'`: a compatibility mode for Spark SQL if you need to migrate certain pipelines from Spark SQL to DuckDB.
- `SET external_file_cache_spill = true`: DuckDB caches remote file blocks in memory; when they get evicted, this spills them to your temp directory instead of downloading them again. With a 300 MB memory limit, the second read of an 854 MB Parquet file on S3 went from 23.9 s to 0.35 s.
- The CLI got a SQL formatter (`duckdb -format`, or `.auto_format on`), a queryable history (`.history`, `FROM shell_history()`), `.about` and `.manual <function>`.
- `read_json` now detects ISO-8601 timestamps with an offset as `TIMESTAMPTZ` instead of silently dropping the offset.
- `CREATE SECRET s IN CONNECTION (...)` scopes a secret to one connection.

**Quack**, the client-server protocol, is the other half of this release and goes to 1.0 with it. I covered it in [its own video](https://www.youtube.com/watch?v=GZulGjfKPGM), so I won't repeat it here.

## Try it and give feedback!

The alpha is one line for the CLI, and the other clients are on the [DuckDB installation page](https://duckdb.org/install/preview):

```bash
Copy codecurl https://install.duckdb.org | DUCKDB_VERSION=alpha bash
~/.duckdb/cli/latest/duckdb -c "SELECT version()"
```

And of course, MotherDuck will support 2.0 close to the release, so feel free to get your hands on the duck in the cloud and enjoy all the nuggets we talked about here.
