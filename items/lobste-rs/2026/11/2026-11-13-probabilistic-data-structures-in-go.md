---
title: Probabilistic data structures in Go
link: https://antonz.org/probabilistic-data-structures/
source: lobste-rs
published: 2026-11-13T16:03:44Z
updated: 2026-11-13T16:03:44Z
first_seen: 2026-11-15T00:13:30.261415141Z
authors:
- antonz.org via cgrinds
labels:
- go
summary: Comments
content: extracted
html: 2026-11-13-probabilistic-data-structures-in-go.html
remote_preview:
  url: https://antonz.org/probabilistic-data-structures/cover.png
---

Probabilistic data structures give you approximate answers to questions about the data they handle. Does the set contain a certain item? How many unique items are in the collection? Which items appear most often? Unlike deterministic structures like sets and frequency tables, the answers might not be exact, but in return, you can drastically reduce memory usage.

In this post, we'll talk about what probabilistic structures are, how they work, and what kinds of guarantees they provide. I'll also introduce [**unsure**](https://github.com/nalgeon/unsure) — a package that implements these structures in Go. It's simple (the core logic for each data structure is 100–300 LOC), fast (usually several times faster than other implementations), and has no dependencies except for the standard library.

[Bloom filter](https://antonz.org/probabilistic-data-structures/#bloom-filter) • [Count-min sketch](https://antonz.org/probabilistic-data-structures/#count-min-sketch) • [HyperLogLog](https://antonz.org/probabilistic-data-structures/#hyperloglog) • [T-digest](https://antonz.org/probabilistic-data-structures/#t-digest) • [Top-k](https://antonz.org/probabilistic-data-structures/#top-k-sketch)

## Bloom filter

A Bloom filter is a set that trades certainty for space. It can tell you that an item is *definitely not* in the set, or that it *probably is* (which might be a false positive). The filter doesn't actually store the items, so it's much more compact than a regular set.

For example, a regular set with 1,000,000 distinct 8-byte strings takes about 64 MB. A Bloom filter for the same strings, with a 1% false positive rate, uses only 1.24 MB (about 10 bits per item) — that's 50 times smaller. The size of the filter doesn't depend on the item size, so the difference gets even bigger with longer strings.

A Bloom filter often resides in memory and helps minimize expensive I/O operations such as disk reads or network calls. You check if an item is in the filter, and if the answer is "no", you skip the costly lookup. If the answer is "yes", you still have to perform the lookup because it might be a false positive.

```go
// A filter for 1000 items with a 1% false positive rate.
filter := bloom.New(1000, 0.01)

filter.Add([]byte("alice"))
filter.Add([]byte("bob"))

filter.Contains([]byte("alice")) // true
filter.Contains([]byte("carol")) // false (or, rarely, true)
```

The filter is sized for the given number of items with a given false positive rate. You can add more, but the false positive rate will go up.

You can add an item to a Bloom filter and check if an item is in the filter, but you cannot delete an item, count the number of items, or iterate over them.

### How it works

**Bloom filter**[^1] is a large array of *m* bits, all set to zero at first. To add an item, you compute *k* hash values for it and set the k bits at those positions:

```text
        ┌──────┐                       ┌───────┐
item ──►│ hash │──► h1, h2, ..., hk ──►│ mod m │──► i1, i2, ..., ik
        └──────┘                       └───────┘

          i1          i2                  ik
┌───┬───┬─▼─┬───┬───┬─▼─┬───┬───┬───┬───┬─▼─┬───┐
│ 0 │ 0 │ 1 │ 0 │ 0 │ 1 │ 0 │ 0 │ 0 │ 0 │ 1 │ 0 │
└───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┘
  0   1   2                                  m-1
```

To check if an item is in the filter, you compute the same k hashes and check if all k bits are set:

- If any of the bits is not set, the item definitely was never added.
- If all the bits are set, the item was probably added, but it's not guaranteed (the bits could have been set by other items).

```text
          i1          i2                  ik
┌───┬───┬─▼─┬───┬───┬─▼─┬───┬───┬───┬───┬─▼─┬───┐
│ 1 │ 0 │ 1 │ 0 │ 0 │ 1 │ 1 │ 0 │ 1 │ 0 │ 1 │ 0 │  all 1 → probably in the set
└───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┘

┌───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┐
│ 1 │ 0 │ 1 │ 0 │ 0 │ 0 │ 1 │ 0 │ 1 │ 0 │ 1 │ 0 │  any 0 → definitely not
└───┴───┴───┴───┴───┴─▲─┴───┴───┴───┴───┴───┴───┘          in the set
                     (!)
```

The k bits are randomly distributed throughout the entire array. When the filter is larger than the CPU cache, each bit access could result in a cache miss, so a lookup may wait on memory k times.

**Unsure** uses a blocked Bloom filter[^2]. The bit array is divided into 512-bit blocks, each matching the size of a CPU cache line. When you add or check an item, it is hashed once: part of the hash selects a block, and the rest selects k bits within that block. Each add or check only accesses one cache line, so the filter stays fast even when it's very large.

```text
        ┌──────┐         ┌──► high 32 bits ──► block
item ──►│ hash │──► h ───┤                   ┌─────────┐
        └──────┘         └──► low 32 bits ──►│ × salts │──► i1, i2, ..., ik
                                             └─────────┘

          block
┌───────┬───▼───┬───────┬───────┬───────┬───────┐
│   0   │   1   │   2   │   3   │  ...  │  n-1  │
└───────┴───────┴───────┴───────┴───────┴───────┘
        │       │
┌───────┘       └───────────────────────────────┐
│         i1          i2                  ik    │
├───┬───┬─▼─┬───┬───┬─▼─┬───┬───┬───┬───┬─▼─┬───┤
│ 0 │ 0 │ 1 │ 0 │ 0 │ 1 │ 0 │ 0 │ 0 │ 0 │ 1 │ 0 │
└───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┴───┘
  0   1   2                                  511
```

In a blocked Bloom filter, some blocks end up with more items than others, which increases the false positive rate. To make up for this, the filter needs to be a bit larger. Compared to a standard Bloom filter, it uses about 3% more memory at a 1% false positive rate, 8% more at 0.1%, and 15% more at 0.01%.

The filter uses the local *wyhash*[^3] implementation for hashing. It performs roughly the same as *xxh3*[^4] but has a much simpler implementation, without SIMD or assembly.

### Benchmarks

Here's an excerpt from the full benchmarks[^5], comparing *unsure* to *bits-and-blooms*[^6] (a classic Bloom filter) and *gloom*[^7] (a blocked one) (ns/op, lower is better):

| Operation      | Items | unsure | bits-and-blooms | gloom |
| -------------- | ----- | ------ | --------------- | ----- |
| Add            | 10K   | **9**  | 37              | 17    |
|                | 1M    | **10** | 36              | 19    |
|                | 50M   | **38** | 170             | 63    |
| Contains, hit  | 10K   | **8**  | 32              | 16    |
|                | 1M    | **9**  | 32              | 18    |
|                | 50M   | **34** | 129             | 60    |
| Contains, miss | 10K   | **8**  | 41              | 27    |
|                | 1M    | **9**  | 48              | 33    |
|                | 50M   | **34** | 113             | 60    |

Memory, bits per item at 10M items (lower is better):

| False positive rate | unsure | bits-and-blooms | gloom |
| ------------------- | ------ | --------------- | ----- |
| 10%                 | 4.84   | **4.80**        | 5.39  |
| 1%                  | 9.90   | **9.59**        | 10.79 |
| 0.1%                | 15.49  | **14.38**       | 16.18 |

*Unsure* is about 4× faster than *bits-and-blooms* and 2× faster than *gloom*, using 1–8% more memory than *bits-and-blooms* and 4–10% less than *gloom*.

## Count-min sketch

A count-min sketch is a frequency table that trades accuracy for space. It tells you approximately how many times each item has appeared. The estimate is *never less* than the actual count, but it can be *higher*. The sketch doesn't actually store the items, so it's much more compact than a regular map of per-item counts.

For example, a regular map counting 1,000,000 distinct 8-byte strings takes about 64 MB. A count-min sketch for the same strings, with estimates within 0.01% of the total count (with 99% probability), uses only 0.55 MB — that's 116 times smaller. The size of the sketch doesn't depend on the number of distinct items or their size, only on the accuracy you choose. So the difference gets even bigger with more or longer strings.

A count-min sketch is useful when you want to track how many times an item shows up in a large stream of events. It's often used to check if something has happened a certain number of times, like rate-limiting an IP after X requests or sending an alert after an error happens Y times. A sketch can also help decide if a key is worth keeping in the cache given how often it was requested.

```go
// Estimates within 0.1% of the total count, with 99% probability.
sketch := countmin.New(0.001, 0.01)

sketch.Add([]byte("alice"))
sketch.Add([]byte("bob"))
sketch.AddN([]byte("alice"), 2)

sketch.Count([]byte("alice")) // 3 (or, rarely, more)
sketch.Count([]byte("bob"))   // 1 (or, rarely, more)
sketch.Count([]byte("carol")) // 0 (or, rarely, more)
sketch.Total()                // 4
```

The sketch uses two settings: epsilon *ε* and delta *δ*. If the actual count for any given item is X and the total of all counts is N, then the estimated count for this item will usually be between X and X+ε·N. However, rarely (with a probability of δ) the estimate could be even higher.

For example, if ε is 0.001, δ is 0.01, and N is 1 million, then the estimate could be off by up to 1,000 (ε·N) with at least 99% probability (1−δ), and by more than 1,000 with at most 1% probability (δ).

The counting error in a sketch depends on the total of all counts, not on each individual item. Therefore, frequent items are estimated more or less accurately relative to their count, but rare items are not — the absolute error is too high for them.

You can increase the per-item counter in a sketch and get the estimated count for an item, but you cannot decrease a counter or iterate over the items.

### How it works

**Count-min sketch**[^8] is a table of counters with *d* rows and *w* columns, all starting at zero. Each row uses its own hash function that maps an item to one of the w counters. To add an item, you increment its counter in each row:

```text
        ┌──────┐                       ┌───────┐
item ──►│ hash │──► h1, h2, ..., hd ──►│ mod w │──► i1, i2, ..., id
        └──────┘                       └───────┘

       ┌───┬───┬─▼─┬───┬───┬───┬───┬───┬───┬───┐
row 1  │ 0 │ 2 │ 4 │ 0 │ 1 │ 0 │ 0 │ 3 │ 0 │ 1 │  4 → 5
       ├───┼───┼───┼───┼───┼───┼─▼─┼───┼───┼───┤
row 2  │ 1 │ 0 │ 0 │ 0 │ 2 │ 0 │ 3 │ 0 │ 1 │ 0 │  3 → 4
       ├───┼───┼───┼───┼───┼───┼───┼───┼─▼─┼───┤
row d  │ 0 │ 1 │ 0 │ 2 │ 0 │ 0 │ 1 │ 0 │ 5 │ 0 │  5 → 6
       └───┴───┴───┴───┴───┴───┴───┴───┴───┴───┘
                 i1              i2      id
```

To estimate an item's count, you take the smallest value from its d counters:

- Other items might hash to the same counter, so each counter value is ≥ true count.
- The smallest counter is the one with the fewest collisions, so it's the best estimate.

```text
       ┌───┬───┬─▼─┬───┬───┬───┬───┬───┬───┬───┐
row 1  │ 0 │ 2 │ 4 │ 0 │ 1 │ 0 │ 0 │ 3 │ 0 │ 1 │  4
       ├───┼───┼───┼───┼───┼───┼─▼─┼───┼───┼───┤
row 2  │ 1 │ 0 │ 0 │ 0 │ 2 │ 0 │ 3 │ 0 │ 1 │ 0 │  3
       ├───┼───┼───┼───┼───┼───┼───┼───┼─▼─┼───┤
row d  │ 0 │ 1 │ 0 │ 2 │ 0 │ 0 │ 1 │ 0 │ 5 │ 0 │  5   min = 3
       └───┴───┴───┴───┴───┴───┴───┴───┴───┴───┘
```

With w = e/ε columns, each counter's error is at most ε·N, with probability at least 1 − 1/e. If you use d = ln(1/δ) rows, the chance that all d of the item's counters exceed its true count by more than ε·N is ≤ δ.

**Unsure** uses a *conservative update*. When you add n to an item, it first finds the current estimate E (the smallest counter), then increases only the counters that are below E+n, leaving the larger ones unchanged. This keeps the same accuracy guarantee, but the actual error is about 2× lower. The tradeoff is that counts can't be decreased.

```text
       ┌───┬───┬─▼─┬───┬───┬───┬───┬───┬───┬───┐
row 1  │ 0 │ 2 │ 4 │ 0 │ 1 │ 0 │ 0 │ 3 │ 0 │ 1 │  4 → 4
       ├───┼───┼───┼───┼───┼───┼─▼─┼───┼───┼───┤
row 2  │ 1 │ 0 │ 0 │ 0 │ 2 │ 0 │ 3 │ 0 │ 1 │ 0 │  3 → 4
       ├───┼───┼───┼───┼───┼───┼───┼───┼─▼─┼───┤
row d  │ 0 │ 1 │ 0 │ 2 │ 0 │ 0 │ 1 │ 0 │ 5 │ 0 │  5 → 5
       └───┴───┴───┴───┴───┴───┴───┴───┴───┴───┘
```

Counters are 32-bit instead of the usual 64-bit, so the sketch uses 2× less memory. The item is hashed once, and each row gets its own position from that hash by salting it (multiplying by a random per-row constant).

```text
        ┌──────┐         ┌─────────┐
item ──►│ hash │──► h ──►│ × salts │──► i1, i2, ..., id
        └──────┘         └─────────┘
```

The sketch uses *wyhash* for hashing.

### Benchmarks

Here's an excerpt from the full benchmarks[^9], comparing *unsure* to *BoomFilters*[^10] (a classic count-min sketch with 64-bit counters) (ns/op, lower is better):

| Operation | ε        | Size (unsure) | unsure | BoomFilters |
| --------- | -------- | ------------- | ------ | ----------- |
| Add       | 0.001    | 55 KB         | **13** | 19          |
|           | 0.00001  | 5.4 MB        | **17** | 32          |
|           | 0.000001 | 54 MB         | **81** | 98          |
| Count     | 0.001    | 55 KB         | **9**  | 51          |
|           | 0.00001  | 5.4 MB        | **14** | 64          |
|           | 0.000001 | 54 MB         | **52** | 139         |

Accuracy at ε=0.0001, N=10M additions over 1M distinct items, mean overestimate (lower is better):

| Distribution  | unsure     | BoomFilters |
| ------------- | ---------- | ----------- |
| Uniform       | **139**    | 302         |
| Zipf, s = 1.1 | **23**     | 44          |
| Zipf, s = 1.5 | **0.3**    | 0.8         |
| Size          | **536 KB** | 1080 KB     |

*Unsure* is about 1.5× faster for Add and 4× faster for Count, using 2× less memory with 2–2.7× lower error.

## HyperLogLog

HyperLogLog is a unique items counter that trades accuracy for space. It tells you approximately how many *distinct items* it has seen, no matter how many times each one appeared. HyperLogLog (also called a *sketch* from here on) doesn't actually store the items, so it's much more compact than a regular set.

For example, a regular set of 1,000,000 distinct 8-byte strings takes about 64 MB. A HyperLogLog sketch counting the same strings with about 0.8% error uses only 16 KB — that's almost 4,000 times smaller. The size of the sketch doesn't depend on the number of items or their size, only on the accuracy you choose. A 16 KB sketch counts a billion items as accurately as a thousand.

HyperLogLog is useful when you need to count unique things in a large stream: unique visitors to a website, unique search queries per day, or distinct values in a database column.

```go
// Standard error about 0.8% of the true count.
sketch := hll.New()

sketch.Add([]byte("alice"))
sketch.Add([]byte("bob"))
sketch.Add([]byte("alice"))
sketch.Add([]byte("carol"))

sketch.Count() // 3 (approximately)
```

You can merge sketches to gather data from multiple machines or segments (like by hour, page, or region). When you merge two sketches, you get the total number of unique items from both, without counting shared items twice.

```go
home := hll.New() // visitors to the home page
docs := hll.New() // visitors to the docs
// ...
home.Merge(docs)
home.Count() // visitors to either page
```

The sketch has one setting: the precision *p*, which gives a relative standard error (stdErr) of 1.04/√(2^p). A common default is p=14, which gives 0.81% error.

With p=14 and 1 million distinct items:

- 68% of the time the estimate is within 0.81% (8,100) of the true count,
- 95% within 1.6% (2·stdErr),
- 99.7% within 2.4% (3·stdErr).

The error is relative to the count, so the standard error is the same 0.81% for a thousand items or a billion.

You can add items to a sketch, get the estimated count, and merge sketches. You cannot check if a specific item was added, remove items, or list them.

### How it works

**HyperLogLog**[^11] is based on a simple observation. If you hash items to random bit strings, about half of them start with 1, a quarter with 01, an eighth with 001, and so on. Seeing a hash with *k* leading zeros suggests there were about 2^(k+1) distinct items. Duplicates hash to the same value, so they don't affect the result.

A single observation is unreliable, so the sketch divides the items into m = 2^p groups, where *p* is the selected precision. The first p bits of the hash pick a group (a *register*), and the register keeps the highest *rank* (leading zeros plus one) it has seen in the rest of the hash:

```text
        ┌──────┐   ┌──────────┬────────────────────────┐
item ──►│ hash │──►│ 01101001 │ 0001101...             │
        └──────┘   └────┬─────┴───────────┬────────────┘
                        │ register i      └ rank = 3 zeros + 1 = 4
          ┌───┬───┬───┬─▼─┬───┬───┬───┬───┬───┬───┐
registers │ 3 │ 0 │ 5 │ 2 │ 1 │ 0 │ 4 │ 2 │ 0 │ 1 │  2 → 4
          └───┴───┴───┴───┴───┴───┴───┴───┴───┴───┘
```

The count is estimated from all the registers together, using a harmonic mean of 2^register. The standard error is about 1.04/√m. Merging two sketches is just taking the maximum value from each pair of registers.

The original estimator is biased when dealing with small counts (up to a few times m), so in these cases it uses a different formula called *linear counting*. This switch creates a sudden change in error, and some implementations (like HyperLogLog++[^12]) address it by using large tables of bias corrections based on real data.

**Unsure** uses the improved estimator by Otmar Ertl[^13]. Instead of a harmonic mean of the registers, it works from a *histogram*: how many registers have value 0, value 1, and so on. A formula applied to the histogram gives an accurate estimate for any number of items, from zero to billions, so there's no need for a separate formula for small counts or for correction tables.

The sketch updates the histogram whenever registers change. Most adds don't change any registers, so this is almost free. Counting always takes the same amount of time, no matter how big the sketch is. Other implementations scan all the registers on every count, which takes from microseconds to milliseconds.

Registers are one byte each, and the histogram has 64 four-byte counters (256 bytes). The item is hashed once with *wyhash*, and the 64-bit hash is split into the register index and the rank.

### Benchmarks

Here's an excerpt from the full benchmarks[^14], comparing *unsure* to *axiom*[^15] (HyperLogLog with LogLog-Beta estimator and sparse mode) (ns/op, lower is better):

| Operation | p  | Size   | unsure      | axiom     |
| --------- | -- | ------ | ----------- | --------- |
| Add       | 10 | 1 KB   | **5.5**     | 7.6       |
|           | 14 | 16 KB  | **5.6**     | 7.6       |
|           | 18 | 256 KB | 8.1         | **7.9**   |
| Count     | 10 | 1 KB   | **68**      | 18,300    |
|           | 14 | 16 KB  | **62**      | 350,000   |
|           | 18 | 256 KB | **57**      | 4,710,000 |
| Merge     | 10 | 1 KB   | **730**     | 1,710     |
|           | 14 | 16 KB  | **20,100**  | 58,600    |
|           | 18 | 256 KB | **341,000** | 951,000   |

Accuracy at p=14 (0.81% standard error), relative RMSE over 100 sketches (lower is better):

| Distinct items | unsure    | axiom     |
| -------------- | --------- | --------- |
| 1,000          | 0.63%     | **0.02%** |
| 40,000         | **0.64%** | 0.71%     |
| 1,000,000      | **0.79%** | 0.89%     |

Memory at p=14 after adding the given number of distinct items (lower is better):

| Distinct items | unsure | axiom      |
| -------------- | ------ | ---------- |
| 100            | 16 KB  | **1.2 KB** |
| 1,000          | 16 KB  | **4.2 KB** |
| 10,000         | 16 KB  | 16 KB      |
| 1,000,000      | 16 KB  | 16 KB      |

*Unsure* adds about as fast as *axiom*, counts 270–83,000× faster, and merges 2.3–2.9× faster, with the same accuracy and memory for counts above the number of registers. When there are fewer items than registers, *axiom*'s sparse mode makes it more accurate and compact.

## T-digest

A t-digest is a summary of a stream of numbers that trades accuracy for space. It can give you an approximate median, 99th percentile, or any other *quantile* of the data. It can also estimate what fraction of the numbers are below a certain value (the CDF). The digest doesn't actually store the numbers, so it's much more compact than a sorted list.

For example, to find the exact 99th percentile of 1,000,000 numbers, you'd have to keep all of them. With 64-bit floats, that takes 8 MB. A t-digest estimates the same percentile within about 0.03% of the rank (between the 98.97th and the 99.03rd percentile) using only 8 KB — 1,000 times smaller. The size of the digest doesn't depend on the number of values, only on the accuracy you choose. An 8 KB digest summarizes a billion values as well as a million.

A t-digest is useful when you need quantiles over a large stream: latency percentiles for a service, the distribution of request sizes or prices, or the outlier thresholds in sensor data.

```go
// Response times in milliseconds.
digest := tdigest.New()

digest.Add(12.5)
digest.Add(8.1)
digest.Add(130.0)
// ...

digest.Quantile(0.5)  // median
digest.Quantile(0.99) // 99th percentile
digest.CDF(100)       // fraction of responses faster than 100 ms
```

You can merge digests to gather data from multiple machines or segments (like by hour, endpoint, or region). A merged digest estimates the percentiles of all values, but with less accuracy than individual digests do.

```go
east := tdigest.New() // latencies on one server
west := tdigest.New() // latencies on another
// ...
east.Merge(west)
east.Quantile(0.99) // 99th percentile across both servers
```

The digest has one setting: the compression *δ*, which limits the number of centroids (see *How it works* below) to δ+2. A common default is δ=100, which takes 8 KB. Increasing δ uses more memory but reduces estimation error.

The error is measured in ranks: a rank error of 0.03% at quantile q=0.99 means the value returned by `Quantile(0.99)` is somewhere between the 98.97th and the 99.03rd percentile of the actual values. Tail estimates, such as q=0.99 or q=0.999, are more accurate than the median (q=0.5).

You can add numbers to a digest, estimate quantiles and the CDF, and merge digests. You cannot remove numbers or list them.

### How it works

**T-digest**[^16] summarizes the numbers as a sorted list of *centroids*. Each centroid has a mean and a weight — the number of values it represents. To estimate a quantile, the digest finds the centroids around the requested rank and interpolates between their means.

The trick is in how big the centroids are allowed to grow. A *scale function* k(q) maps a quantile *q* to a number from -δ/4 to δ/4, and each centroid may only span 1 unit of k: if a centroid covers the quantiles from q₁ to q₂, then k(q₂) − k(q₁) ≤ 1.

The arcsine scale function, k(q) = δ/(2π)·asin(2q-1), is steep near q=0 and q=1 and flat near the median. This means the centroids it produces are larger in the center and smaller at the ends:

```text
weight
  │                ┌──┐
  │             ┌──┤  ├──┐
  │          ┌──┤  │  │  ├──┐
  │       ┌──┤  │  │  │  │  ├──┐
  │  ┌─┬──┤  │  │  │  │  │  │  ├──┬─┐
  └──┴─┴──┴──┴──┴──┴──┴──┴──┴──┴──┴─┴──► value
    min      q=0.25   median  q=0.75     max
```

With 1M values and δ=100, the centroids around the median hold about 27,000 values each, those around the 99th percentile about 5,000, and the outermost ones about 500. This keeps the error near the tails small.

**Unsure** is the merging t-digest from the Dunning and Ertl paper. New values go to a buffer of 5·δ numbers. When the buffer is full, the digest sorts it and merges it with the centroids (already sorted) in a single pass, building new centroids from left to right as long as each one fits within its span of k. Every other pass goes right to left, so that neither tail ends up with fuller centroids. `Quantile`, `CDF` and `Merge` flush the buffer first.

The centroids are kept in two fixed arrays of δ+2 entries (the current list and the next one being built), along with the cumulative weights for a binary search in `Quantile` and `CDF`. With δ=100, that's 16·102·2 + 8·103 bytes for the centroids, and 8·500 bytes for the buffer, about 8 KB in total.

Centroids with weight 1 are treated as exact values rather than spread around their mean, and the exact minimum and maximum are used at the ends. So digests with few values give exact results, and `Quantile(0)` and `Quantile(1)` are always exact.

### Benchmarks

Here's an excerpt from the full benchmarks[^17], comparing *unsure* to *influxdata*[^18] (merging t-digest with the same scale function) (ns/op, lower is better):

| Operation | δ   | unsure    | influx |
| --------- | --- | --------- | ------ |
| Add       | 50  | **44**    | 78     |
|           | 100 | **49**    | 86     |
|           | 500 | **59**    | 105    |
| Quantile  | 50  | 11        | 13     |
|           | 100 | 13        | 13     |
|           | 500 | 15        | 16     |
| Merge     | 50  | **296**   | 2,060  |
|           | 100 | **630**   | 4,330  |
|           | 500 | **2,720** | 23,950 |

Accuracy at δ=100, mean rank error over 1M normal values (lower is better):

| Digest | Quantile | unsure | influx |
| ------ | -------- | ------ | ------ |
| Single | 0.5      | 0.007% | 0.015% |
|        | 0.99     | 0.018% | 0.016% |
|        | 0.999    | 0.011% | 0.009% |
| Merged | 0.5      | 0.055% | 0.037% |
|        | 0.99     | 0.023% | 0.021% |
|        | 0.999    | 0.006% | 0.016% |

Memory at δ=100 (lower is better): *unsure* 8.5 KB, *influxdata* 14.4 KB.

*Unsure* adds values about 1.8× faster than *influxdata*, queries at the same speed, merges 7–9× faster, and uses 40% less memory at about the same accuracy.

## Top-k sketch

A Top-k sketch finds the *k most frequent* items in a stream (the "heavy hitters"), along with their approximate counts. It uses a fixed amount of memory based only on k, no matter how many different items are in the stream.

To find the exact top items, you'd have to count every unique item, for example with a map of per-item counts. With millions of distinct items, that takes tens or hundreds of megabytes. A Top-k sketch for the top 1000 items takes about 360 KB, and it usually finds all of them.

A Top-k sketch is useful when you want to identify the "hottest" data: the most requested URLs, the most active users, or the most common search queries. Unlike a count-min sketch, it keeps the actual items, so it can list them.

```go
// Track the 2 most frequent items.
sketch := topk.New(2)

sketch.Add([]byte("alice"))
sketch.Add([]byte("bob"))
sketch.Add([]byte("carol"))
sketch.AddN([]byte("alice"), 4)
sketch.AddN([]byte("carol"), 2)

sketch.Top()                  // [{alice 5} {carol 3}]
sketch.Count([]byte("alice")) // 5
sketch.Count([]byte("bob"))   // 0 (not in the top 2)
sketch.Total()                // 9
```

`Top` returns up to k items, from the most frequent to the least frequent. Counts are estimates: they're usually exact for the most frequent items, but may be slightly lower than the actual count for less common items.

The sketch works best when the stream is skewed, meaning a few items appear much more often than the others. This is common in real situations. If all items are about equally frequent, there is no clear top k, and the results become less accurate.

You can add items to the sketch and get the top items with their counts, but you can't decrease counts or check items that aren't in the top k.

### How it works

**HeavyKeeper**[^19] is a table of buckets that estimates item counts. It has *d* rows and *w* columns, and each item maps to one bucket in each row. Each bucket holds a fingerprint of the item and a count. To add an item, you update its bucket in each row:

- If the bucket is empty or owned by the item, increase its count.
- If the bucket is owned by another item, *decay* it: decrease its count by 1 with probability b^count (b < 1). When the count reaches 0, the new item takes over the bucket with a count of 1.

```text
          ┌──────┐
add x ──► │ hash │──► h ──► high 32 bits ──► fingerprint = 7
          └──────┘    │
                      │ map to buckets
                   ┌──┴────────────┐
       ┌───────┬───▼───┬───────┬───│───┬───────┐
row 1  │       │ fp=7  │ fp=3  │   │   │ fp=5  │  column 2 is owned by x:
       │       │  41   │   2   │   │   │  980  │  41 → 42
       ├───────┼───────┼───────┼───▼───┼───────┤
row 2  │ fp=9  │       │       │ fp=2  │ fp=4  │  column 4 is owned by another:
       │  17   │       │       │   3   │   1   │  3 → 2 with probability b^3
       └───────┴───────┴───────┴───────┴───────┘
         col 1   col 2   col 3   col 4   col 5
```

The item's estimated count is the largest count among its buckets. Frequent items quickly reach counts that make decay very unlikely, so they keep their buckets. Rare items keep replacing each other without affecting the frequent ones. An item that becomes frequent later in the stream can still outnumber a less frequent owner and take over its bucket.

Next to the table, the sketch keeps a min-heap with the k items that have the largest estimates so far. After an item's buckets are updated, if its estimate is larger than the smallest count in the heap, the item is added to the heap, and the smallest item is removed. If the item is already in the heap, its count is increased.

```text
                   ┌───────────┐
add x (15) ───────►│ bob    12 │  the root has the smallest count
                   └─────┬─────┘
              ┌──────────┴────────┐
        ┌─────┴─────┐       ┌─────┴─────┐
        │ alice  30 │       │ dave   14 │  15 > 12: x replaces bob at the root
        └─────┬─────┘       └───────────┘  15 > 14: x moves down, dave moves up
      ┌───────┴──────┐
┌─────┴─────┐  ┌─────┴─────┐
│ eve   980 │  │ fay    41 │
└───────────┘  └───────────┘
```

**Unsure** uses d = 2 rows of max(16k, 256) buckets, and a decay base of b = 0.8. The probabilities b^count are precomputed as 64-bit thresholds, so decay is cheap: the count *c* decreases if a random number is below the c-th threshold.

The original HeavyKeeper uses b = 1.08⁻¹ ≈ 0.93, and more rows. A smaller b lets frequent items lock their buckets faster, which makes the sketch more precise on flat streams. However, if b is too small (like 0.5), the first items to claim buckets keep them permanently, so items that become frequent later are never counted, even if they end up being the most frequent. Using b = 0.8 keeps most of the precision and still catches most late-arriving frequent items. As for the rows, two are enough to protect frequent items from collisions in a single row, and adding more rows only makes the sketch slower.

The item is hashed once. The hash is mapped to a position in each row by salting it (multiplying by a random per-row constant). The fingerprint is the top 32 bits of the hash. The heap uses the 64-bit hash to identify items, so it doesn't compare the actual item bytes.

Each bucket is 8 bytes (a 32-bit fingerprint and a 32-bit count), so the table takes 256·k bytes. The heap takes about 100 bytes per item plus the item itself.

The sketch uses *wyhash* for hashing and *wyrand* for random numbers.

### Benchmarks

Here's an excerpt from the full benchmarks[^20], comparing *unsure* to *keiler*[^21] (HeavyKeeper with b = 0.9 and d = ln k) and *dgryski*[^22] (Filtered Space-Saving[^23]) on the Zipf s=0.8 stream (time/op, lower is better):

| Operation | k    | unsure      | keiler  | dgryski |
| --------- | ---- | ----------- | ------- | ------- |
| Add       | 10   | **24 ns**   | 64 ns   | 59 ns   |
|           | 100  | **25 ns**   | 84 ns   | 70 ns   |
|           | 1000 | **33 ns**   | 130 ns  | 73 ns   |
| Top       | 10   | **0.34 µs** | 0.64 µs | 0.77 µs |
|           | 100  | **3.7 µs**  | 13 µs   | 14 µs   |
|           | 1000 | **75 µs**   | 270 µs  | 292 µs  |

Precision is the fraction of the reported top k that is in the true top k (higher is better):

| k    | Zipf s | unsure     | keiler    | dgryski  |
| ---- | ------ | ---------- | --------- | -------- |
| 100  | 0.6    | **0.90**   | 0.38      | 0.19     |
|      | 0.8    | **1.00**   | 0.78      | 0.85     |
|      | 1.0    | 0.99       | **1.00**  | **1.00** |
|      | Size   | 39 KB      | **27 KB** | 39 KB    |
| 1000 | 0.6    | **0.97**   | 0.70      | 0.52     |
|      | 0.8    | **1.00**   | 0.98      | **1.00** |
|      | 1.0    | **1.00**   | **1.00**  | **1.00** |
|      | Size   | **364 KB** | 414 KB    | 453 KB   |

Count error is the mean of |estimate − true| / true over the reported items (lower is better):

| k    | Zipf s | unsure    | keiler | dgryski   |
| ---- | ------ | --------- | ------ | --------- |
| 100  | 0.6    | **14%**   | 94%    | 428×      |
|      | 0.8    | **0.10%** | 46%    | 230%      |
|      | 1.0    | 0.01%     | 0.12%  | **0.0%**  |
| 1000 | 0.6    | **2.4%**  | 62%    | 18×       |
|      | 0.8    | **0.23%** | 1.9%   | 0.31%     |
|      | 1.0    | 0.05%     | 0.24%  | **0.02%** |

*Unsure* is 2–4× faster than the others for Add and 1.8–4× faster for Top. On flat streams (s=0.6), it finds the true top k with 90–97% precision, while *keiler* drops to 38–70% precision with a count error of 62–94%, and *dgryski* to 19–52% with counts 18× or more too high. On skewed streams (s=1.0), all three are about equally accurate.

## Wrapping up

As engineers, we usually want exact answers from our data. But sometimes, allowing a bit of uncertainty can cut memory usage by tens, hundreds, or even thousands of times. That's where probabilistic data structures come in:

- *Bloom filter* checks if an item might be in a set.
- *Count-min sketch* estimates how many times each item has occurred.
- *HyperLogLog* estimates the number of distinct items.
- *T-digest* estimates quantiles of a stream of numbers.
- *Top-k sketch* identifies the most frequent items in a stream.

To use them in Go, try the [nalgeon/unsure](https://github.com/nalgeon/unsure) package:

```text
go get github.com/nalgeon/unsure
```

[★ Subscribe](https://antonz.org/subscribe/) to keep up with new posts.

[^1]: [Space/time trade-offs in hash coding with allowable errors](https://doi.org/10.1145/362686.362692) by Burton H. Bloom, 1970.

[^2]: [Cache-, hash-, and space-efficient Bloom filters](https://doi.org/10.1145/1498698.1594230) by Felix Putze et al., 2009.

[^3]: [Reference wyhash implementation](https://github.com/wangyi-fudan/wyhash) by Wang Yi.

[^4]: [Reference xxHash implementation](https://github.com/Cyan4973/xxHash) by Yann Collet.

[^5]: [Bloom filter benchmarks](https://github.com/nalgeon/unsure/tree/main/bloom#benchmarks).

[^6]: [bits-and-blooms/bloom](https://github.com/bits-and-blooms/bloom) by Daniel Lemire.

[^7]: [jcalabro/gloom](https://github.com/jcalabro/gloom) by Jim Calabro.

[^8]: [An improved data stream summary: the count-min sketch and its applications](https://doi.org/10.1016/j.jalgor.2003.12.001) by Graham Cormode and S. Muthukrishnan, 2005.

[^9]: [Count-min sketch benchmarks](https://github.com/nalgeon/unsure/tree/main/countmin#benchmarks).

[^10]: [tylertreat/BoomFilters](https://github.com/tylertreat/BoomFilters) by Tyler Treat.

[^11]: [HyperLogLog: the analysis of a near-optimal cardinality estimation algorithm](https://doi.org/10.46298/dmtcs.3545) by Philippe Flajolet et al., 2007.

[^12]: [HyperLogLog in practice](https://doi.org/10.1145/2452376.2452456) by Stefan Heuleet et al., 2013.

[^13]: [New cardinality estimation algorithms for HyperLogLog sketches](https://arxiv.org/abs/1702.01284) by Otmar Ertl, 2017.

[^14]: [HyperLogLog benchmarks](https://github.com/nalgeon/unsure/tree/main/hll#benchmarks).

[^15]: [axiomhq/hyperloglog](https://github.com/axiomhq/hyperloglog) by Axiom.

[^16]: [Computing extremely accurate quantiles using t-digests](https://arxiv.org/abs/1902.04023) by Ted Dunning and Otmar Ertl, 2019.

[^17]: [T-digest benchmarks](https://github.com/nalgeon/unsure/tree/main/tdigest#benchmarks).

[^18]: [influxdata/tdigest](https://github.com/influxdata/tdigest) by InfluxData.

[^19]: [HeavyKeeper: An Accurate Algorithm for Finding Top-k Elephant Flows](https://www.usenix.org/conference/atc18/presentation/gong) by Junzhi Gong et al., 2018.

[^20]: [Top-k benchmarks](https://github.com/nalgeon/unsure/tree/main/topk#benchmarks).

[^21]: [keilerkonzept/topk](https://github.com/keilerkonzept/topk) by KEILERKONZEPT.

[^22]: [dgryski/go-topk](https://github.com/dgryski/go-topk) by Damian Gryski.

[^23]: [Efficient Computation of Frequent and Top-k Elements in Data Streams](https://doi.org/10.1007/978-3-540-30570-5_27) by Ahmed Metwally et al., 2005.
