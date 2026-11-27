---
title: What gets TOASTed in Postgres
link: https://planetscale.com/blog/what-gets-toasted-in-postgres
source: planetscale-com
published: 2026-10-12T00:00:00Z
updated: 2026-10-12T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Ahmed Darwich
labels:
- engineering
- postgres
summary: Postgres 19 switches TOAST compression from pglz to LZ4. LZ4 is faster, but it isn't always smaller, and some reads get slower with it.
content: extracted
html: 2026-10-12-what-gets-toasted-in-postgres.html
remote_preview:
  url: https://planetscale.com/assets/what-gets-toasted-in-postgres-social-ChgMkmF3.png
---

Insert 2,000 copies of the letter `x` into a table with an `int` ID and a `text` column, and Postgres stores all 2,000 bytes uncompressed. Insert 2,001, and Postgres compresses the value to 26 bytes with LZ4.

That one extra byte is enough to trigger **TOAST**, the part of Postgres that compresses values once their row gets too big, and moves them out of the row when compressing isn't enough.

Postgres 19, now in beta, changes how TOAST compresses, switching the [default](https://github.com/postgres/postgres/commit/34dfca293) from pglz to LZ4 in builds with LZ4 support. PlanetScale already made this switch on Postgres 17 and 18.

Here, we look at which values get TOASTed, what TOAST does to them, and when LZ4 is the right choice. Let's dig in!

## [What a row costs in bytes](https://planetscale.com/blog/what-gets-toasted-in-postgres#what-a-row-costs-in-bytes)

Compression is all about saving bytes, so we'll start with where the bytes in a row go.

Postgres stores table data in fixed 8 KB pages. A row can't be split across pages, so every row has to fit inside one.

Each row starts with a 24-byte header that Postgres uses to [track which transactions can see it](https://planetscale.com/blog/postgresql-mvcc), followed by the values of its columns. The space those values take depends on their type, and every type falls into one of two groups:

- **Fixed-width types** like `bigint`, `timestamptz`, and `boolean` always take the same number of bytes.
- **Variable-length types** like `text`, `jsonb`, and `bytea` take as many bytes as the value needs, plus a length header: 1 byte for values up to 126 bytes, and 4 bytes for anything longer.

`pg_column_size` shows both groups in a single row:

```sql
SELECT pg_column_size(ROW(1::bigint, now(), true, 'Ada'::text));
 pg_column_size
----------------
             45
(1 row)
```

That's the 24-byte header, 17 bytes for the `bigint`, `timestamptz`, and `boolean` (8, 8, and 1), and 4 bytes for `'Ada'`: three characters plus a 1-byte length header. Change `'Ada'` to `'Ada Lovelace'`, and the row grows to 54 bytes while the other columns stay at 17.

Variable-length values are what make a row grow, and the only ones Postgres can compress.

## [Small rows don't trigger compression](https://planetscale.com/blog/what-gets-toasted-in-postgres#small-rows-dont-trigger-compression)

TOAST stands for The Oversized-Attribute Storage Technique. As the name suggests, TOAST only steps in when a row gets too big.

Postgres treats a row as oversized once it's larger than 2,032 bytes, about a quarter of an 8 KB page. This [threshold](https://github.com/postgres/postgres/blob/REL_18_STABLE/src/include/access/heaptoast.h) was set so that at least four rows fit on every page, after room for the page header and a pointer to each row.

Here's what happens when we store the most compressible value possible, a single repeated letter, at lengths just below and just above the threshold. `pg_column_compression` returns the algorithm that compressed each value, or null if the value isn't compressed, which we display as `none`. The `COMPRESSION lz4` clause picks the algorithm.

```sql
CREATE TABLE t (id int PRIMARY KEY, body text COMPRESSION lz4);

INSERT INTO t VALUES
  (1500, repeat('x', 1500)),
  (2000, repeat('x', 2000)),
  (2001, repeat('x', 2001));

SELECT id AS chars,
       pg_column_size(body) AS stored_bytes,
       coalesce(pg_column_compression(body), 'none') AS compression
FROM t ORDER BY id;
 chars | stored_bytes | compression
-------+--------------+-------------
  1500 |         1504 | none
  2000 |         2004 | none
  2001 |           26 | lz4
(3 rows)
```

The first two values aren't compressed. Each takes 4 bytes more than its length, for the length header. The third value, however, is just one character longer, and it's compressed to 26 bytes.

The 2,000-character row is exactly 2,032 bytes: 24 for the row header, 4 for the `int`, and 2,004 for the text. That's right on the threshold, so TOAST leaves it alone. The 2,001-character row is one byte over.

## [TOAST does two different jobs](https://planetscale.com/blog/what-gets-toasted-in-postgres#toast-does-two-different-jobs)

The threshold applies to the whole row, but TOAST works on individual variable-length values. Once a row is over the threshold, TOAST has two ways to shrink it:

- **Compress** a value, and keep the smaller version in the row, like the 2,001-character row.
- **Move** a value out of the row, into a separate table.

That separate table is called a **TOAST table**. Tables with columns that can hold large values get one automatically in the `pg_toast` schema.

A value moved to the TOAST table isn't stored in one piece, though. TOAST splits it into chunks of up to 1,996 bytes and stores each chunk as its own row, tagged with the value's ID (`chunk_id`) and its position in the value (`chunk_seq`). The rest of the 2,032 bytes holds the chunk row's header and IDs, so four chunk rows fit on a page. The original row keeps an 18-byte pointer, so Postgres can find the chunks and reassemble them.

TOAST works through a row's values from largest to smallest. As soon as the row is back under 2,032 bytes, the table's `toast_tuple_target`, it stops.

To see every outcome, we store four very different values in one table: a short string, 10 KB of repeated text, 200 KB of random characters, and 1.1 MB of a repeated sentence. `stored_bytes` is the value's size wherever it ended up, including the TOAST table for a moved-out value. `octet_length` returns a value's uncompressed size, and `pg_column_toast_chunk_id` (Postgres 17+) returns its ID in the TOAST table, or null if the value is still in the row.

```sql
CREATE TABLE docs (id int PRIMARY KEY, body text COMPRESSION lz4);

INSERT INTO docs VALUES
  (1, repeat('x', 200)),
  (2, repeat('abc', 3400)),
  (3, (SELECT string_agg(md5(random()::text), '') FROM generate_series(1, 6250))),
  (4, repeat('The quick brown fox jumps over the lazy dog. ', 25000));

SELECT id,
       octet_length(body) AS raw_bytes,
       pg_column_size(body) AS stored_bytes,
       coalesce(pg_column_compression(body), 'none') AS compression,
       pg_column_toast_chunk_id(body) IS NOT NULL AS moved_out
FROM docs ORDER BY id;
 id | raw_bytes | stored_bytes | compression | moved_out
----+-----------+--------------+-------------+-----------
  1 |       200 |          204 | none        | f
  2 |     10200 |           60 | lz4         | f
  3 |    200000 |       200000 | none        | t
  4 |   1125000 |         4471 | lz4         | t
(4 rows)
```

Each value lands somewhere different:

- **Row 1** is under the threshold, so TOAST never touches it.
- **Row 2** compresses from 10 KB to 60 bytes, small enough to stay in the row.
- **Row 3** is random, so compressing it doesn't save space. TOAST moves the value out uncompressed.
- **Row 4** compresses to 4,471 bytes, which is still too big for the row, so TOAST compresses it *and* moves it out.

Each column's **storage strategy** decides which of those jobs TOAST can do:

- **`PLAIN`**: neither. Used by fixed-width types like `int`.
- **`EXTENDED`**: compress, then move out if needed. The default for `text`, `jsonb`, and `bytea`.
- **`EXTERNAL`**: move out, but never compress.
- **`MAIN`**: compress, but keep the value in the row even above the usual TOAST target if the row still fits on a page. Move it out only as a last resort. The default for `numeric`.

You can change a column's strategy with `ALTER TABLE docs ALTER COLUMN body SET STORAGE EXTERNAL`. It doesn't, however, change values already stored.

## [TOAST isn't a large object](https://planetscale.com/blog/what-gets-toasted-in-postgres#toast-isnt-a-large-object)

Postgres has a second way to store big values, called **large objects**, built for whole files like videos or PDFs.

In [Can you put a video in Postgres?](https://www.youtube.com/watch?v=pfu8OoCIU6g), we stored a 4 GB video as a large object. `lo_import` loads the file into the `pg_largeobject` system table, split into about 2 million 2 KB chunks, and returns an OID that our `video` table keeps as a reference. To stream the video to a browser, `lo_get` reads any byte range straight from those chunks.

TOAST and large objects both store big values in chunks, but they differ in three ways:

- **TOAST is automatic.** It applies to any variable-length column without you asking. A large object only exists if you create one.
- **TOAST values top out at 1 GB.** Large objects can hold up to 4 TB.
- **TOAST values are deleted with their row.** A large object stays until you remove it with `lo_unlink`, even after the row that references it is gone.

For files over 1 GB, you need a large object. Anything smaller fits in a regular column, where TOAST can also compress it.

## [How pglz and LZ4 work](https://planetscale.com/blog/what-gets-toasted-in-postgres#how-pglz-and-lz4-work)

Postgres can compress a TOAST value with one of two algorithms, **pglz** or **LZ4**. A column uses the one its `COMPRESSION` clause names, or the `default_toast_compression` setting if it doesn't name one. Changing either only affects values compressed from then on. Values already compressed keep their algorithm, even when you copy them with `INSERT ... SELECT` or rewrite the table with `VACUUM FULL`.

Note

**zstd** isn't an option for column compression. Postgres uses it elsewhere, like write-ahead log (WAL) compression through `wal_compression` (Postgres 15+) and backups.

Both algorithms work the same basic way. They scan a value from start to finish, looking for bytes that already appeared earlier. When they find a repeat, they replace it with a short note saying where the earlier copy is and how long it is.

In `{"status":"active"},{"status":"active"}`, the second object is stored as a few-byte note pointing back to the first.

There's a limit, though, to how far back that note can point, called the **window**. If the earlier copy is farther back than the window, the algorithm can't see it and stores the repeat in full.

**pglz** is Postgres's own compressor. Its window is 4 KB, and at each position it checks several earlier matches and keeps the longest. Postgres throws a pglz result away unless it saves more than 25%, and pglz gives up early on data where it finds no repeats at all.

**LZ4** is a general-purpose compressor used well beyond Postgres. Postgres has supported it since version 14. Its window is 64 KB, but instead of searching, it checks a single earlier candidate from a small lookup table, which makes it much faster. That table keeps overwriting older entries with newer ones, so the farther back a repeat is, the less likely LZ4 is to find it. Postgres keeps an LZ4 result as long as it saves more than 2 bytes.

To test the window, we build a long string out of 30,000 random six-character words. With a space after each one, every word takes 7 bytes, so the string is about 210 KB.

We build it twice, once picking from 500 different words and once from 5,000. With 500, a word shows up again about 500 words later on average, or 3.5 KB back. With 5,000, it's about 35 KB back.

We store each string in both a pglz column and an LZ4 column. `setseed` makes the random words the same on every run, so the sizes below repeat on Postgres 18:

```sql
CREATE TABLE words (vocab int, pglz text COMPRESSION pglz, lz4 text COMPRESSION lz4);

SELECT setseed(0.42);

INSERT INTO words
SELECT vocab, body, body
FROM (
  SELECT vocab, string_agg(substr(md5(floor(random() * vocab)::text), 1, 6), ' ') AS body
  FROM (VALUES (500), (5000)) AS v(vocab), generate_series(1, 30000)
  GROUP BY vocab
) s;

SELECT vocab,
       octet_length(pglz) AS raw_bytes,
       pg_column_size(pglz) AS pglz_bytes,
       pg_column_size(lz4) AS lz4_bytes
FROM words ORDER BY vocab;
 vocab | raw_bytes | pglz_bytes | lz4_bytes
-------+-----------+------------+-----------
   500 |    209999 |     105457 |     96022
  5000 |    209999 |     209999 |    177590
(2 rows)
```

With 500 words, the average gap is inside pglz's 4 KB window, but the gaps vary, and about 30% of repeats land farther back. LZ4 finds most of those, so its value comes out about 9% smaller than pglz's.

With 5,000 words, most repeats are out of pglz's reach. It can't save more than 25%, so Postgres stores the value uncompressed. LZ4 still reduces it by about 15%, from the repeats that happen to land close.

## [pglz wins on these JSON documents](https://planetscale.com/blog/what-gets-toasted-in-postgres#pglz-wins-on-these-json-documents)

In a JSON array, the same keys come back in every object, close enough for both algorithms to find them.

We loaded 10,000 order documents into three tables on PlanetScale Postgres 18: one using `EXTERNAL` so nothing gets compressed, one using pglz, and one using LZ4. Each document has 25 to 100 line items, and every line item repeats the same five keys: `sku`, `name`, `qty`, `price_cents`, and `tags`.

The uncompressed table is 103 MB. The pglz table is 19 MB and the LZ4 table is 24 MB.

Since both find the keys, the size comes down to pglz's extra searching, which finds longer matches. LZ4 makes up for it in speed: in two runs each, it loaded the same documents in 0.48 and 1.09 s, against 1.59 and 1.73 s for pglz.

## [pglz is faster for previews](https://planetscale.com/blog/what-gets-toasted-in-postgres#pglz-is-faster-for-previews)

pglz also wins when you only read the start of a large value, like a 200-character preview with `left(body, 200)`. pglz never needs more than 9 bits to store a byte of the original, plus 2 bytes overall, so Postgres can put a ceiling on how many compressed bytes a prefix needs. A character takes 1 to 4 bytes, so 200 characters are at most 800 bytes, and 800 bytes of output need at most 902 compressed bytes. That's less than the 1,996 bytes in a chunk, so Postgres fetches only the first one. Postgres has no equivalent ceiling for LZ4, so it fetches the whole value first.

We stored 100 text values of about 1 MB each, which both algorithms compress to about half their size. For the first 200 characters of each, pglz needed 301 page lookups, all from memory, in about 1 ms, while LZ4 needed 6,234 in about 16 ms. The uncompressed `EXTERNAL` version also needed 301, but takes about twice the disk.

## [When to skip compression](https://planetscale.com/blog/what-gets-toasted-in-postgres#when-to-skip-compression)

Compressed values take fewer pages, which means less disk, more of a table in memory, and less WAL to send to replicas and back up. The cost is CPU, on every write that crosses the threshold and every read of a compressed value. Compression also doesn't reduce [egress](https://planetscale.com/blog/database-egress), since Postgres decompresses values before sending them.

Two kinds of data are better off uncompressed, with `EXTERNAL`:

- **Already-compressed data**, like the JPEG frames in our [video conferencing demo](https://planetscale.com/blog/video-conferencing-with-postgres). LZ4 spends CPU on every frame and saves little or nothing.
- **Large `bytea` values read from the middle**, like seeking in a file.

With `EXTERNAL`, every chunk but the last holds exactly 1,996 bytes of the value, so Postgres can work out which chunk any byte is in. The first 250 chunks cover 499,000 bytes, so the 200 bytes starting at 500,000 are all in chunk 250, and Postgres fetches only that one. Compressed, a chunk still holds 1,996 bytes, but there's no telling how much of the value they decompress to. So with LZ4, Postgres fetches every chunk and decompresses from the start. pglz has its ceiling from `left()`, but for byte 500,000 that ceiling is larger than the whole stored value, so it reads every chunk too.

Doing that read on the same 100 values, stored as `bytea`, took 0.6 ms uncompressed with `EXTERNAL`, against 43 ms with LZ4 and 116 ms with pglz.

Note

In a UTF-8 database, jumping to a chunk only works with `bytea`. `text` characters take 1 to 4 bytes, so Postgres reads from the start.

## [Wrapup](https://planetscale.com/blog/what-gets-toasted-in-postgres#wrapup)

TOAST only steps in once a row passes 2,032 bytes, so plenty of values, like the 2,000 `x`s from the start, never get compressed at all. For the ones that do, LZ4 is the right default for most tables: its speed matters more than the size difference, which can go either way. For a few kinds of data, it's worth choosing pglz or `EXTERNAL` by hand:
