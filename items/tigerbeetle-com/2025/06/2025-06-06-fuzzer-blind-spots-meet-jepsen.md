---
title: Fuzzer Blind Spots (Meet Jepsen!)
link: https://tigerbeetle.com/blog/2025-06-06-fuzzer-blind-spots-meet-jepsen
source: tigerbeetle-com
published: 2025-06-06T00:00:00Z
updated: 2025-06-06T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- djg
summary: Systems inevitably have components which are easier or harder to test.
content: extracted
html: 2025-06-06-fuzzer-blind-spots-meet-jepsen.html
remote_preview:
  url: https://tigerbeetle.com/blog/2025-06-06-fuzzer-blind-spots-meet-jepsen/banner-adb8dd25.webp
---

Systems inevitably have components which are easier or harder to test.

Test coverage tends to mirror this gradient—deterministic components with strong properties readily support easy test harnesses and workload generation, in contrast to components with IO and side effects. Accordingly, we expect more bugs in these less tested “dark and dusty” corners of the system, and fewer bugs in the middle of the bright spotlight that is fuzz testing.

Last year, to invest in finding bugs, we contracted Kyle Kingsbury to test TigerBeetle with [Jepsen](https://jepsen.io). For the most part, Jepsen’s findings matched our expectations—but with a notable exception. Jepsen found a correctness bug in what appeared to be a well-fuzzed component of the code: TigerBeetle’s query engine.

This was surprising. TigerBeetle is fuzzed extensively, and this query component in particular was covered by four separate fuzzers. Why didn’t they catch the bug?

(Check out the [Jepsen report](https://jepsen.io/analyses/tigerbeetle-0.16.11) for Kyle’s full analysis!)

The story begins with Jepsen spotting an anomaly in some query results.[^1]

First, Jepsen creates a transfer with id `143087`:

```
{
  :type :invoke,
  :f :create-transfers,
  :value [{
    :amount 178,
    :ledger 1,
    :debit-account-id 29042N,
    :credit-account-id 1495N,
    :user-data-64 184,
    :id 143087N,
    :code 18,
    :flags #{},
  }],
}
```

This transfer is acknowledged as created by the cluster:

```
{
  :type :ok,
  :f :create-transfers,
  :value [:ok],
  :timestamp 1733255844108408881,
}
```

Then, minutes later, we send a query that should return the transfer:

```
{
  :type :invoke,
  :f :get-account-transfers,
  :value {
    :flags #{:reversed :debits},
    :account-id 29042N,
    :limit 2,
    :timestamp-max 1733255962740337920,
    :user-data-64 184,
  },
}
```

…but (unexpectedly) the cluster returns an empty set of results:

```
{
  :index 189424,
  :type :ok,
  :f :get-account-transfers,
  :value [],
  :timestamp 1733256018656717948,
}
```

Jepsen compares this result to the expected result from its own independent reference model and flags the discrepancy.

Further test runs indicate that this missing query result anomaly only occurs when the query is such that multiple fields are intersected. (In the case from Jepsen above, this is specifically `user_data_64=184` intersecting with `debit_account_id=29042`.)

Jepsen found convincing evidence of this bug. The natural question was: *Why didn’t TigerBeetle’s fuzzers find the bug?*

Of TigerBeetle’s ~20 fuzzers, 4 perform queries. Namely, the [VOPR](https://github.com/tigerbeetle/tigerbeetle/blob/e44c8a6957aded892daa7490875d4899bc941619/src/vopr.zig) (TigerBeetle’s deterministic simulator), the [LSM forest fuzzer](https://github.com/tigerbeetle/tigerbeetle/blob/e44c8a6957aded892daa7490875d4899bc941619/src/lsm/forest_fuzz.zig), the [LSM tree fuzzer](https://github.com/tigerbeetle/tigerbeetle/blob/e44c8a6957aded892daa7490875d4899bc941619/src/lsm/tree_fuzz.zig), and the [LSM query fuzzer](https://github.com/tigerbeetle/tigerbeetle/blob/e44c8a6957aded892daa7490875d4899bc941619/src/lsm/scan_fuzz.zig):

- The LSM tree fuzzer only tests a single tree, so it wouldn’t hit this bug, since it involves multiple trees.
- The LSM forest fuzzer tests multiple trees, but only queries a single tree at a time, and never intersects multiple trees.
- The VOPR and the LSM query fuzzer both test and intersect multiple trees, so they are the only tests which could have possibly caught this bug. (The VOPR and the LSM query fuzzer use similar logic for their workloads, so we’ll focus on the VOPR’s logic below.)

Still, the VOPR and LSM query fuzzer both missed the bug, with two possibilities:

- The fuzzer workloads never generate an input which triggers the bug.
- The fuzzer workloads generate an input which triggers the bug, but the verification is incorrect (or incomplete).

In this case, the problem is the former.

During workload initialization, the VOPR pre-registers the queries it will send over the course of the test:

```
for (query_intersections, 1..) |*query_intersection, index| {
    query_intersection.* = .{
        .user_data_64 = @intCast(index * 1_000_000),
        .user_data_32 = @intCast(index * 1_000),
        // The `code` will be used to recover the index.
        .code = @intCast(index),
    };
}
```

Each transfer, which the workload creates, matches exactly one of these disjoint queries:

```
const query_intersection_index =
  self.prng.index(self.auditor.query_intersections);

const query_intersection =
  self.auditor.query_intersections[query_intersection_index];

transfer.* = .{
    // ...
    .user_data_64 = query_intersection.user_data_64,
    .user_data_32 = query_intersection.user_data_32,
    .code         = query_intersection.code,
    // ...
};
```

When the workload successfully creates a transfer, it uses `query_intersection` to count how many total (created) transfers match the corresponding filter:

```
const query_intersection_index = transfer.code - 1;
const query_intersection =
  &self.query_intersections[query_intersection_index];
query_intersection.transfers.count += 1;
```

Then the workload periodically executes the query described by `query_intersection` and verifies (among other things) that it returns the expected number of results.

The intent of this strategy was to simplify the fuzzer workload (which is quite complex).

In generative testing, you generate inputs, and verify the corresponding outputs. By constructing the queries in advance we can check properties (like “result count == number of matching transfers”) without needing to implement a full-fidelity model of the query engine.

But this approach has a fatal flaw: because of the common prefix for each query’s target fields, the objects matching any given query are consecutive in each index. We have accidentally introduced a blind spot (!) in the fuzzer.

![Chart contrasting structured/unstructured input/output generative testing](https://tigerbeetle.com/blog/2025-06-06-fuzzer-blind-spots-meet-jepsen/structured-inputs-outputs-526c9d62.webp)\
Chart contrasting structured/unstructured input/output generative testing

The solution is to both:

- allow less-structured inputs—arbitrary inserts and queries, and
- check the outputs more precisely, by using a detailed model of the database.

After [rewriting the fuzzer](https://github.com/tigerbeetle/tigerbeetle/commit/73f13ee2d092fe6998c315b0c4ae5fbc2983154a) to simply generate random objects, insert them into a model, and then test it with random queries, the fuzzer can trivially reproduce the missing-results bug. Now that the model is more detailed, we can check the exact results in all cases, rather than just a subset.

Now that we have a deterministic reproduction of the bug, we can isolate and fix it.

To execute a query such as the one above, TigerBeetle will:

1. Get an iterator over the `user_data_64` secondary index LSM tree of transfer timestamps for which `transfer.user_data_64 = 184`.
2. Get an iterator over the `debit_account_id` secondary index LSM tree of transfer timestamps for which `transfer.debit_account_id = 29042`.
3. Intersect these two iterators.
4. Fetch and return the transfers which correspond to each of the timestamps in the intersection.

The iterators in steps 1 and 2 are asynchronous, yielding the values (in timestamp order) from a secondary index, horizontally across consecutive tables within an LSM level, and vertically across all levels.

The intersection in step 3 is implemented with the Zig-Zag Merge Join algorithm, which shines at intersecting large indexes efficiently. In the Zig-Zag Merge Join, when one iterator returns a higher key than the other iterators, the other iterators skip ahead to that key (or the next highest key). This is called “probing” the iterators.

Suppose an iterator is probed—this shrinks the query’s target range (`scan.key_min`/`scan.key_max`) from one side. Then if the iterator’s currently buffered table is exhausted, we try to fetch the next table in the level:

```
const key_exclusive_next = iterating.key_exclusive_next;
if (self.scan.key_min <= key_exclusive_next and
    key_exclusive_next <= self.scan.key_max)
{
    // Load the next `table_info`.
    self.state = .loading_manifest;
    self.values = .fetching;
    self.move_next_manifest_table(key_exclusive_next); // <---
} else {
    // The next `table_info` is out of the key range, so it's finished.
    self.state = .{ .finished = .{} };
    self.values = .finished;
}
```

In the code above, `key_exclusive_next` tracks the table’s highest key (for ascending query results) or the table’s lowest key (for descending query results) of the most recent table we iterated, to ensure that we don’t scan the same table twice. Typically the condition would check whether we are done iterating tables for this level, because the extreme key of the most recent iterated table lies outside of our query’s (newly shrunk) target range.

But, because the target range has shrunk, our exclusive key may now lie outside of that range from the wrong end, causing the `ScanTreeLevel` to incorrectly consider itself finished. Then it returns the (prematurely truncated) results that it has accumulated so far.

To [fix](https://github.com/tigerbeetle/tigerbeetle/pull/2544) this, we simply split that condition:

- When the `key_exclusive_next` is out of bounds because we iterated all the tables we need to iterate: Stop scanning (same as before).
- When the `key_exclusive_next` is out of bounds because our target range shrunk: Keep scanning, but ignore the `key_exclusive_next`.

The bug only occurs when there are multiple iterators and one of them probes the other. The original query fuzzer missed it because its workload had a blind spot—probing was never necessary, as the two secondary indexes were always synced up.

Fuzz testing searches for bugs by probabilistically exploring the state space of a program, which would be too massive to check exhaustively.

But fuzzers typically don’t sample the space of possible program inputs and states uniformly. Even when this would be possible, it is often not ideal. Some states are more interesting (more likely to hide bugs) than others. And interesting states are hard to hit with uniform random input. Fuzzer workloads can take this into account, biasing the input to make “interesting” states more likely.

The hazard here is the risk of the fuzzer’s workload unintentionally projecting away dimensions of the system’s reachable states, leaving you with a false sense of security of the fuzzer’s coverage. In our case, the VOPR’s seemingly sophisticated approach to query generation created a blind spot that hid a real bug.

When a fuzzer stops finding bugs, that doesn’t mean its job is done—it may simply mean it has exhausted the particular slice of state space it can reach. Be wary of introducing unintended constraints in fuzzer workloads, and consider complementing targeted fuzzers with simpler approaches that sample more broadly, even if less efficiently.

And lastly, for more detail on this and the other bugs Jepsen found, sit back, brew a coffee, and read the [full report](https://jepsen.io/analyses/tigerbeetle-0.16.11). It was an absolute pleasure working with Kyle, and we look forward to working together again in future.

*Update: For a systems engineering perspective on why TigerBeetle’s “Requests Never Time Out (#206)”, a philosophical issue raised in the Jepsen report, Dominik Tornow has [written an excellent follow-up analysis](https://dtornow.substack.com/p/jepsen-and-tigerbeetle).*

[^1]: Logs are from Jepsen but simplified for clarity.
