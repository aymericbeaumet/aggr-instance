---
title: Building a faster, lazier router for Postgres
link: https://planetscale.com/blog/designing-neki-for-performance
source: planetscale-com
published: 2026-10-01T00:00:00Z
updated: 2026-10-01T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Dirkjan Bussink
labels:
- engineering
- neki
summary: Nothing is as fast as not doing anything. How Neki keeps great performance by only decoding the parts of a PostgreSQL result the router actually needs.
content: extracted
html: 2026-10-01-building-a-faster-lazier-router-for-postgres.html
remote_preview:
  url: https://planetscale.com/assets/designing-neki-for-performance-social-kkdgmfR3.png
---

Nothing is as fast as not doing anything. That sounds obvious, but it is also surprisingly easy to forget when building software.

[Neki](https://planetscale.com/blog/announcing-neki) adds a layer between a PostgreSQL client and PostgreSQL. We need that layer to route queries to shards and to make [many PostgreSQL servers look like one](https://planetscale.com/blog/making-768-servers-look-like-1). The downside is that every query now has another network hop and has to pass through the [router](https://planetscale.com/blog/what-is-a-neki-router).

We can't make that hop disappear, so a lot of the performance work in Neki comes down to what we can avoid doing while the query passes through it. If the router doesn't need to understand a result, why decode it? If it only needs one column, why decode the other ten columns in the same row?

The key part here is that we can design this to be lazy and composable. Walking the PostgreSQL messages in a response does not mean we also need to construct rows. Finding the rows does not mean we need to decode their columns either. Even if the router needs one value, the rest of the row can stay as protocol bytes.

## [Why not decode everything up front?](https://planetscale.com/blog/designing-neki-for-performance#why-not-decode-everything-up-front)

The usual way to build a layer like this is to parse whatever comes over the wire into your own types as soon as it arrives. Every row becomes a list of values, and the rest of the code only ever deals with those. It's a sensible default because it gives the rest of the system one consistent thing to work with.

Vitess, the MySQL sharding system we maintain, works this way, so it's a useful comparison. Vitess sends query results between its components using protobuf. Leaving out the fields that aren't important here, [`QueryResult` and `Row`](https://github.com/vitessio/vitess/blob/main/proto/query.proto) look like this:

```json
message QueryResult {
  repeated Field fields = 1;
  uint64 rows_affected = 2;
  uint64 insert_id = 3;
  repeated Row rows = 4;
  string info = 6;
  string session_state_changes = 7;
  bool insert_id_changed = 8;
}

message Row {
  // -1 means NULL.
  repeated sint64 lengths = 1;

  // All non-NULL values concatenated together.
  bytes values = 2;
}
```

This is a pretty compact protobuf representation. `lengths` tells us where the values are in the shared `values` byte string, so the protobuf doesn't need a separate allocation for every value. The query engine can't use the protobuf rows directly though. At the RPC boundary, [`proto3ToRows`](https://github.com/vitessio/vitess/blob/main/go/sqltypes/proto3.go) converts all of them:

```go
func proto3ToRows(fields []*querypb.Field, rows []*querypb.Row) [][]Value {
    result := make([][]Value, len(rows))
    for i, r := range rows {
        result[i] = MakeRowTrusted(fields, r)
    }
    return result
}
```

And [`MakeRowTrusted`](https://github.com/vitessio/vitess/blob/main/go/sqltypes/result.go) walks every column in every row:

```go
func MakeRowTrusted(fields []*querypb.Field, row *querypb.Row) []Value {
    sqlRow := make([]Value, len(row.Lengths))
    var offset int64
    for i, length := range row.Lengths {
        if length < 0 {
            continue
        }
        sqlRow[i] = MakeTrusted(
            fields[i].Type,
            row.Values[offset:offset+length],
        )
        offset += length
    }
    return sqlRow
}
```

`MakeTrusted` doesn't turn every integer into a Go integer. A `Value` can still point at the bytes in the protobuf row. What we do get is a slice for every row and a `Value` for every non-NULL column before the query engine can do anything with the result. Going back across an RPC boundary does the reverse. [`RowToProto3Inplace`](https://github.com/vitessio/vitess/blob/main/go/sqltypes/proto3.go) first walks all the values to build the lengths and then walks them again to concatenate their bytes.

The protobuf result sits between separate Vitess components, while the query engine works with `[][]Value`. Converting at the boundary gives the engine one consistent type to work with. The downside is that every result gets converted, even when the router never looks at a value.

We wanted Neki to perform better, and therefore do less work in such cases.

## [Keeping the PostgreSQL response](https://planetscale.com/blog/designing-neki-for-performance#keeping-the-postgresql-response)

Neki also uses protobuf between the router and the [sidecar](https://planetscale.com/blog/the-architecture-of-neki#sidecar) that sits in front of each PostgreSQL instance, but the result protobuf looks very different. Our `ExecuteResponse` is mostly a byte string:

```protobuf
message ExecuteResponse {
  bytes raw = 1;
  int64 conn_id = 2;
  ...
}
```

`raw` contains PostgreSQL response messages. A `DataRow` in there is already a valid PostgreSQL `DataRow` that we can send to the client. Of course we still have to decode the `ExecuteResponse` protobuf to get the `raw` byte slice. That still leaves us with one byte slice. What we avoid is a protobuf object for every row, another slice of lengths for that row and a value for every column.

That doesn't mean the router ignores the response. `NewQueryResponse` walks the five-byte message headers, validates the lengths, handles `CommandComplete` and finds the region containing the `DataRow` and `NoticeResponse` messages. This is stored as one byte slice in `DataRows`. We only open it further if the query needs us to.

For the examples here, we'll use an `orders` table sharded by `account_id`:

```sql
CREATE TABLE orders (
    id uuid NOT NULL,
    account_id bigint NOT NULL,
    status text NOT NULL,
    created_at timestamptz NOT NULL,
    total numeric NOT NULL,
    PRIMARY KEY (account_id, id)
);
```

## [A result we don't need to decode](https://planetscale.com/blog/designing-neki-for-performance#a-result-we-dont-need-to-decode)

The simplest example is a point lookup:

```sql
SELECT id, status, total
FROM orders
WHERE account_id = $1
  AND id = $2;
```

Because `account_id` is the sharding key, Neki needs the value in `$1` to pick the shard. It doesn't need to understand `id`, `status` or `total` when they come back. Those values only need to make it from PostgreSQL to the client. We can use the same approach on the way into the router too.

### [Bind parameters are rows going the other way](https://planetscale.com/blog/designing-neki-for-performance#bind-parameters-are-rows-going-the-other-way)

With PostgreSQL's extended query protocol, `$1` and `$2` arrive in a [`Bind` message](https://www.postgresql.org/docs/current/protocol-message-formats.html). Each parameter is length-prefixed as a column in a `DataRow`:

```text
NULL:     Int32(-1)
non-NULL: Int32(length), followed by value bytes
```

The difference is where the format comes from. `Bind` includes format codes for its parameters, while the formats for `DataRow` columns come from `RowDescription`. The [PostgreSQL protocol message documentation](https://www.postgresql.org/docs/current/protocol-message-formats.html) has all the details if you want to know all the fun little bits about the protocol.

Neki keeps the original `Bind` message and a cache for the parameters it has decoded. For our point lookup, the router scans the parameter frames, decodes the bigint in `$1` and caches it. We don't need `$2` for routing. When the query is sent to the shard, Neki writes the original parameter frames directly as long as the client encoding hasn't changed.

Keeping the original frames also helps when we rewrite a query. A rewritten query might use only some parameters, put them in a different order or use the same one multiple times. A mapping like `[1, 0, 1]` copies the parameter frames into a new `Bind`. We can use the same approach when adding parameters to the existing list.

[Nested loop joins](https://en.wikipedia.org/wiki/Nested_loop_join) have a similar case. A column from a row on the left becomes a parameter for a query on the right. Neki copies the complete length-prefixed column frame from the `DataRow` into the new `Bind`. We only turn the join key into a typed value if we also need it for routing or evaluation.

### [Text stays text and binary stays binary](https://planetscale.com/blog/designing-neki-for-performance#text-stays-text-and-binary-stays-binary)

There is one more wrinkle: PostgreSQL values can be sent as text or binary. The client uses `Bind` to tell PostgreSQL which formats it used for the parameters and wants for the result columns. Instead of up-front conversions like Vitess would require, Neki keeps values in the original formats.

When a query selects `id` and `account_id` but orders by `created_at` without selecting it, `id` and `account_id` use the formats requested by the client. The extra `created_at` column uses binary because only Neki needs it. The client will never see those column values.

If Neki needs to decode a value, `RowDescription` tells it whether to use the text or binary decoder. If it doesn't, the bytes stay untouched. When Neki produces a new value, it uses the format requested by the client.

Conversion is still needed if `client_encoding` changes between `Bind` and `Execute`. Affected text parameters are recoded before they are sent to PostgreSQL. The other parameters stay as they are.

Now let's go back to the result from the point lookup. `DataRows` can hold one raw byte slice, a slice of rows that point into those bytes or a slice of pointers used to select and reorder those rows. It only creates the next representation when a query needs it.

In this case, Neki writes the raw bytes straight to the client.

No `DataRow` structs and no columns! Just the PostgreSQL messages that we already had in the correct Postgres wire format. This also works for cross-shard queries. In the query below, `status` isn't the sharding key so it will have to get passed along to every shard to collect results:

```sql
SELECT id, account_id, status
FROM orders
WHERE status = 'pending';
```

There is no global order and no sums / averages to calculate across the results. This means we can optimize. After sending the scatter queries, Neki streams response chunks from each shard, combines the separate `CommandComplete` messages with `Collapse`, and `DataRow`s can be passed along unmodified. It's a "complex" distributed query, but there is nothing in the rows we need to decode.

## [Adding `LIMIT`](https://planetscale.com/blog/designing-neki-for-performance#adding-limit)

Now let's add a limit:

```sql
SELECT id, account_id, status
FROM orders
WHERE status = 'pending'
LIMIT 100;
```

Now we need to know when we've sent 100 rows, which means the response cannot be an opaque byte slice. `Limit` calls `DataRows.Len()`, and on the first call we walk the raw response and count the `DataRow` messages so the row slice can be allocated once. In a second pass, we create the `DataRow` wrappers which all point into the original response buffer.

Once we have the row boundaries, `Limit` builds a pointer slice for the requested window:

```go
ptrs := make([]*runtime.DataRow, n)
for i := range n {
    ptrs[i] = response.Rows.Row(int(startIdx) + int(i))
}

response.Rows = runtime.NewDataRowsFromPtrsWithNotices(
    ptrs,
    notices,
)
```

`ptrs` is the third representation in `DataRows`. It lets us take a subset of the rows or put them in a different order without moving row messages.

For this query Neki knows where every row starts and ends. What it *doesn't* know is the value of a single `id`, `account_id` or `status` column. Counting rows doesn't require that... but what if it did?

## [Sorting means decoding the sort key](https://planetscale.com/blog/designing-neki-for-performance#sorting-means-decoding-the-sort-key)

For a pending orders page, we'd probably want a stable sorting:

```sql
SELECT id, account_id, created_at
FROM orders
WHERE status = 'pending'
ORDER BY created_at DESC
LIMIT 100;
```

Each shard sorts its subset of results (within Postgres), but the router has to merge those distinct result sets into a global order. Now we finally have to decode a column. First we need the rows, the same as for `LIMIT`, and then we need to find `created_at` inside each row.

At the protocol level, a `DataRow` keeps the complete PostgreSQL message and a view of the bytes containing its columns. The column bytes start after the five-byte message header and the two-byte column count. Each column has a four-byte length followed by the value bytes. A length of `-1` means `NULL`.

To get to the third column, we have to read the lengths of the first two columns and skip their payloads. Reading the lengths of these columns doesn't decode the values. We now know where `id` and `account_id` are, but the bytes still have no meaning to the router. With the offsets, we can jump to the correct bytes to grab `created_at` for each row.

These timestamps are cached because a row will normally be compared more than once while merging results. At this point all rows have been exposed and we've looked at their column framing. The actual values look like this though:

```text
id          frame located, payload not decoded
account_id  frame located, payload not decoded
created_at  frame located, payload decoded and cached
```

The merge produces another `DataRows` with pointers to the original rows, now in the right order. Sorting rows doesn't require copying the rows.

## [Dropping a column without decoding](https://planetscale.com/blog/designing-neki-for-performance#dropping-a-column-without-decoding)

A small change can have a big impact on what work we can and cannot skip. How about we don't select `created_at`:

```sql
SELECT id, account_id
FROM orders
WHERE status = 'pending'
ORDER BY created_at DESC
LIMIT 100;
```

Ordering by a column without returning it is a very normal query, but it does give the router one extra problem. The shards have to return `created_at` because Neki needs it for the merge. Before we send the rows to the client, that extra column has to be removed.

Recall from the last section that by the end we reached the point where `created_at` had been decoded, but `id` and `account_id` were still only protocol frames. For this one, there's more to do.

The naive implementation would read `id` and `account_id` as values, build a new row and serialize them again. That's a lot of work to remove a few bytes from the end of a message.

Instead, `SimpleProjection` stores the projection as runs of adjacent input columns. Each run contains the first and last column, so `{0,2}` is one run covering columns 0, 1 and 2. This lets `ProjectInto` copy all three column frames as one byte range. Skipping, reordering or duplicating columns creates more runs, stored in output order:

```text
stored runs          byte ranges copied
[{0,2}]              columns 0–2 in one copy
[{0,2}, {4,6}]       columns 0–2, then 4–6 in two copies
[{3,3}, {0,2}]       column 3, then 0–2 in two copies
[{0,0}, {0,0}]       column 0 twice in two copies
```

Our query needs only one run, `{0,1}`. At execution time we find the byte range containing both length-prefixed columns, write a new `DataRow` header with a column count of two and append both columns at once.

The byte-backed projection does one append per run. `Values` is the row's view of the column bytes, and `offsets` contains the frame boundaries found while scanning it:

```go
for _, r := range runs {
    dst = append(
        dst,
        dr.Values[offsets[r.Start]:offsets[r.End+1]]...,
    )
}
```

This isn't zero-copy because we are creating a different PostgreSQL message. The new header and the columns have to go somewhere. We do copy the exact `id` and `account_id` frames though, still saving a good amount of work through careful optimization.

Normally we're projecting a complete response batch, not one random row. Neki appends all the new rows to one shared wire buffer. The finished rows are then bounded views into that buffer, so there isn't a separate backing buffer for every output row.

The slightly funny part is that we decoded `created_at`, which the client didn't ask for, and didn't decode `id` or `account_id`, which it did ask for. That's exactly the work this query required. In Neki, we take every opportunity to skip unnecessary work.

## [When we really need values](https://planetscale.com/blog/designing-neki-for-performance#when-we-really-need-values)

Of course some queries do require the router to understand and produce values. A cross-shard aggregate is a good example:

```sql
SELECT status, count(*)
FROM orders
GROUP BY status;
```

The same `status` can exist on many shards, so we can't rely on PostgreSQL to do the full `GROUP BY`. The Neki router must handle it.

We need the group key and the partial counts as values. There isn't a useful byte-level trick for grouping equal values and adding the counts. The final count is also a new value produced by the router, so there are no original PostgreSQL bytes to copy.

For a row like this, we cannot get away with just passing along bytes from Postgres to clients.

When a results row from this query is sent to the client, Neki serializes those values into a new PostgreSQL `DataRow` in the format the client requested. This query MUST go through every layer:

```text
response messages
    -> result batches
        -> rows
            -> column frames
                -> typed values
                    -> newly computed values
```

## [Bytes and values at the same time](https://planetscale.com/blog/designing-neki-for-performance#bytes-and-values-at-the-same-time)

Neki doesn't have one internal row representation. A row received from PostgreSQL starts wire-backed, while a row produced by an aggregate can be completely value-backed. The sorted rows from a few sections ago are a mix of the two: they keep the original message and have a cached `created_at` value.

```text
Original: complete PostgreSQL DataRow

values[0]: no decoded value
values[1]: no decoded value
values[2]: decoded created_at
```

The original message is still available for forwarding or projection, while the cache avoids decoding `created_at` again every time the row is compared.

Cached values survive projection too. If a projection keeps or moves a decoded column, Neki copies the original frame and remaps the cached value to its new position. We took the performance hit in decoding the timestamp because the query needed it, but we want to make sure we don't decode or serialize it again.

For PostgreSQL records this can also be necessary for correctness. A record can have a transient `typmod` describing it, which isn't always recoverable from the wire bytes. Raw projection won't allocate a cache in the case where none of the projected columns have decoded values.

## [Using this trick elsewhere](https://planetscale.com/blog/designing-neki-for-performance#using-this-trick-elsewhere)

This way of working with protocol frames isn't limited to sorting and projection. For joins, we decode the keys needed to match rows. The output can still be assembled from contiguous runs of columns from the left and right rows. We avoid doing the extra work of producing values whenever we can!

When we need to spill rows to disk, we also keep things in the self-framing PostgreSQL `DataRow` format. We can write and read from disk with minimal additional decoding overhead.

Keeping bind parameters in their original length-prefixed representation is not just about performance either. Parsing and serializing a value isn't guaranteed to give us identical bytes. A floating point NaN payload, for example, can survive PostgreSQL's receive function but change when it is sent again.

PostgreSQL extension types are another useful case. The router might have no idea how to evaluate a value from an extension, but it can still pass the protocol bytes through.

## [Keeping the layers composable](https://planetscale.com/blog/designing-neki-for-performance#keeping-the-layers-composable)

The nice part of this model is that each representation builds on the previous one. `DataRows` can start with raw response bytes, expose those bytes as a row slice and then add a pointer slice for a subset or a different order. An individual row can keep its original PostgreSQL message while also caching the values that have been decoded. Going one layer deeper for one part of a query doesn't force all columns / values to do so.

Of course that doesn't make the complexity disappear. Byte-level projection still has to calculate the right offsets, write a new message header and copy the right column frames in the right order. The key thing for Neki's performance though, is that it's all contained in the row projection and batch builder.

It also gives us a small surface we can test directly. The projection tests construct PostgreSQL messages byte by byte and cover reordering, `NULL`, empty strings, empty `bytea`, duplicate columns and the protocol-backed path.

There are a few invariants we have to maintain for that containment to work. Shared response buffers have to remain alive as long as a row still points into them. We use three-index slices because an append through one row must not overwrite the next PostgreSQL message in the same buffer. Two consumers replaying the same buffered row need their own row wrapper and value cache, while the immutable protocol bytes can still be shared.

Memory accounting also has to know that a row can grow. It starts out retaining the wire bytes, but every string, numeric or record added to the cache uses more memory. And then it all has to work with `NULL`, text and binary formats, client encodings, arrays, domains, records and user-defined types. If moving the bytes around changes the result, we've built a very fast bug 😉.

## [Nothing is still the fastest](https://planetscale.com/blog/designing-neki-for-performance#nothing-is-still-the-fastest)

Neki does a lot of real work. It routes queries and sometimes has to merge, compare, group or compute values across shards, but it doesn't have to do all of that for every column in every query. A point lookup can keep the result as PostgreSQL messages. `LIMIT` needs row boundaries. Sorting needs the sort keys. Projection can move existing column frames around. An aggregate generates newly computed values because there is no earlier layer where it can stop.

For every query the router tries to do the least possible work. The best way to make software fast is to avoid work at all costs.
