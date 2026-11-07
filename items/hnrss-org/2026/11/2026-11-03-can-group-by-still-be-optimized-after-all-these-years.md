---
title: Can GROUP BY still be optimized after all these years?
link: https://pivotlake.io/blog/know-your-limit-faster-top-k-aggregation/
source: hnrss-org
published: 2026-11-03T16:30:23Z
updated: 2026-11-03T16:30:23Z
first_seen: 2026-11-07T16:49:08.933078383Z
authors:
- ntur1337
content: extracted
html: 2026-11-03-can-group-by-still-be-optimized-after-all-these-years.html
remote_preview:
  url: https://pivotlake.io/assets/pivot-logo.png
  alt: pivot
---

One of the things I like the most about the world of databases is how a simple operation can become extremely complex when it's optimized to its highest levels.

As part of that line of thought - I thought it might be interesting and fun to share a novel optimization we recently found that improved Pivot's performance significantly on GROUP BY queries that find the top K groups by aggregate value. For example, "which 10 items have the most orders?":

```sql
SELECT item_id, COUNT(*) AS c
FROM orders
GROUP BY item_id
ORDER BY c DESC
LIMIT 10;
```

## How do "regular" GROUP BYs run on analytical databases?

Although every database wants to think it's unique - most of them (Pivot included ;)) performs GROUP BY in a very similar way when the grouping keys have high cardinality (IE there are many groups in the GROUP BY).

First, each CPU thread in the database receives a slice of the input data to read from disk, decode, and prepare for aggregation:

Each thread then divides its decoded data into X partitions based on the grouping key and aggregates each partition separately. For example, ClickHouse uses 256 partitions, so each thread maintains its own set of 256 hash tables, one per partition:

\* Pivot uses a different method here instead of a hash table, but that’s a story for another blog post ;)

Keeping these hash tables private to each thread lets the threads aggregate the data independently, without acquiring locks or writing to the same memory locations, which would cause contention between CPU caches.

Once all threads finish this initial aggregation, each worker is assigned a range of partitions to merge. For each partition, the worker gathers the partial aggregates produced by all threads and combines them into the final aggregates for that partition’s groups:

Once every partition has been merged, the database produces the complete GROUP BY result by concatenating the results from all partitions:

## Is all this work actually needed?

While this process definitely outputs a correct result, in some (very common) query patterns, this GROUP BY algorithm performs much more work than is actually needed. Specifically, in queries where not all the aggregation results are used in the final answer - many of the groups and partitions are aggregated when they don't actually need to.

Take our earlier example: “Which 10 items have the most orders?”

```sql
SELECT item_id, COUNT(*) AS c
FROM orders
GROUP BY item_id
ORDER BY c DESC
LIMIT 10;
```

If our system uses 4,096 partitions, the top 10 items can belong to at most 10 of them. That leaves at least 4,086 partitions whose results contribute nothing to the final answer—yet we still spend a-lot of time and compute merging them.

Queries in this pattern are pretty common in workloads we see: “Which 100 items in our warehouse have the highest review scores?”, “Who are our 50 biggest paying customers?”, “Which posts on our platform received the most likes in the last 24 hours?”. Given that, we thought it was worth exploring whether these queries could be optimized!

## So how can we avoid all this work?

To solve this redundant work issue, we looked for a structure that could help us prune some of the partitions as we progress through the aggregation phase. As big lovers of Bloom filters (and the proud owners of [i-love-bloom-filters.com](https://i-love-bloom-filters.com)!), we thought a probabilistic, hash-based structure might help us achieve this goal.

The most "naive" implementation we could think about was to simply maintain a global counter or aggregate for each partition and use it later to decide which partitions to skip.

For our “items with the most orders” query, that means keeping a counter of the total number of orders in each partition:

During the merge stage, before merging each new partition, we can compare its total order count against the smallest count in our current TopK results.

If the partition’s total is smaller, we can safely skip merging it entirely: no individual group can have more orders than the partition as a whole, so none of its groups can affect our current TopK result.

If the partition’s total is larger, though, we still need to merge it, do the heavy CPU work, and find out whether any of its groups is actually large enough to affect our TopK result.

While this approach is pretty cool and can help prune partitions in some scenarios, it requires a pretty extreme distribution of aggregate values across groups.

To prune a partition, the average aggregate value across its groups must be orders of magnitude smaller than the smallest value in our current TopK.

For example, suppose the top 10 order counts we’ve calculated so far range from 1,000 to 1,300. If a partition contains 100 items with exactly 15 orders each, our pruning logic can’t skip it: the partition’s total is 1,500 orders, which exceeds our smallest TopK value - even though none of its individual groups comes close.

Because our pruning logic only considers the total number of orders in each partition, it can’t distinguish between two cases: 100 items with 15 orders each, which we could safely prune, and 99 items with one order each plus one item with 1,401 orders, which we must merge because that item belongs in our TopK. Both partitions contain 1,500 orders, but only the second contains an item that would change the result.

To overcome this, we looked for a method that builds on the same idea but gives us more control over the granularity of pruning.

The method we came up with was to give each partition a fixed number of “slots.” Each group maps to one slot, and that slot tracks the combined order count of all groups mapped to it. Since no group’s count can exceed its slot’s total, we can safely prune a partition if *every slot’s total is below the smallest value in our current TopK*.

For example, suppose again our current top order counts range from 1,000 to 1,300. A partition containing 100 items with 15 orders each has a total of 1,500, so the partition-level counter alone won’t let us prune it. But if those items are spread roughly evenly across eight slots, each slot will contain only about 180–195 orders, which means that now we can prune the entire partition :)

We can assign groups to slots using the same hash we already use for partitioning. If, for example, the first 3 bits of a group ID’s hash select one of eight partitions, the next 3 bits can select one of eight slots within that partition:

The fun part is that the number of slots is independent of the number of partitions. We can add more slots to get a finer-grained view of the counts within each partition based on how "spread out" the values are, without adding the overhead of maintaining too many partitions.

Some scenarios, such as a very flat distribution of values - where, for example, all items have the same number of orders - might not benefit from this optimization at all, and the engine should turn this optimization off when it detects this kind of distribution in the dataset.

## Results

Theory is always fun, but now let's talk $$.

Beyond the significant improvements we saw in our internal workloads, the benefits of this approach also show up in public benchmarks. For example, most (63%!) of [ClickBench](https://benchmark.clickhouse.com/)’s queries contain this pattern of GROUP BY followed by ORDER BY on an aggregate result, and get a nice performance boost from this optimization in Pivot.

For example, one of its simplest queries with this pattern (Q16):

```sql
SELECT UserID, SearchPhrase, COUNT(*)
FROM hits
GROUP BY UserID, SearchPhrase
ORDER BY COUNT(*) DESC
LIMIT 10;
```

Got a nice 1.6× speed boost from this approach, going from 35.6 ms to 22.2 ms, while pruning more than 70% of its partitions during execution:

While this approach already gives a nice boost when the aggregation fits entirely in memory, we think the gains could be even bigger once Pivot adds MPP and spill-to-disk capabilities, because skipping entire partitions would also save a lot of shuffle and disk I/O!
