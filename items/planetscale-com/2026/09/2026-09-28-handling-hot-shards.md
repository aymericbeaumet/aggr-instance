---
title: Handling hot shards
link: https://planetscale.com/blog/hot-shards-hotter-tenants
source: planetscale-com
published: 2026-09-28T00:00:00Z
updated: 2026-09-28T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Simeon Griggs
labels:
- engineering
- neki
- postgres
- vitess
summary: What makes a good sharding strategy today might not work so well tomorrow. What happens when you have one tenant hitting your database orders of magnitude more than any other? Or product managers knocking down your door with new requirements? If you're already on Neki, fortunately these are issues we can solve.
content: extracted
html: 2026-09-28-handling-hot-shards.html
remote_preview:
  url: https://planetscale.com/assets/hot-shards-hotter-tenants-social-BQfttOr9.png
---

Since the launch of Neki we've talked a lot about sharding with basic examples to demonstrate how Neki splits a table's rows evenly across shards. Neki's router reads your [data topology](https://planetscale.com/blog/what-is-a-data-topology) to handle the placement of writes and to find the correct shard for reads.

But life in production is never so simple.

Imagine your application's Neki database is sharding on a `tenant_id` column. Makes sense. You want a nice even distribution of tenants across shards. But your app takes off, congratulations! Now, some tenants are hitting their shard at a greater size or volume than most. New product opportunities emerge that require cross-tenant queries.

Suddenly an even distribution of *tenants* is less useful than an even distribution of *data*.

You probably just have the wrong shard key, at least for some of your data. No problem, Neki gives you the tools to mitigate increased pressure on shards, and adapt your resharding strategy, without downtime.

## [A story of resharding success](https://planetscale.com/blog/hot-shards-hotter-tenants#a-story-of-resharding-success)

While Neki is new, sharding isn't. Neki is built by the maintainers of Vitess: a proven, scalable and flexible solution that has a great history of solving these exact problems.

Slack has published multiple articles ([1](https://planetscale.com/blog/hot-shards-hotter-tenants#footnotes), [2](https://planetscale.com/blog/hot-shards-hotter-tenants#footnotes)) and talks ([3](https://planetscale.com/blog/hot-shards-hotter-tenants#footnotes)) on how they had to modify their approach to sharding as product demand and requirements changed over time thanks to Slack's rapid rise in popularity.

Their original tidy and *obvious* way to shard **message** and **channel** data was by the ID of the **workspace** to which they belonged. The logic of which **workspace** belonged to which **shard** was maintained in a cluster dedicated to sharding metadata.

The assumption was reasonable, that no single customer would ever outgrow the biggest database. But then a workspace of 10s of 1000s of users lands. Then 100s of 1000s. Vertical scaling and isolation bought time, but not enough.

Over time this became problematic.

*With per-workspace sharding, a single hot tenant's messages table quickly overwhelms the shard*

As the product got more successful, some workspaces were far more demanding than others. Workspaces for large enterprises could contain over 100,000 users, which ballooned the initial payload the client application needed. Some **workspaces' messages** tables were getting too large for any one shard. Cross-workspace messaging became a requirement. A new enterprise grid feature needed to organize multiple workspaces under a single enterprise organization. All of this was being made complicated by the current "shard by workspace" strategy.

Their solution involved Vitess, a sharding solution for MySQL, to simplify resharding, starting with sharding **messages** by **channel** instead of **workspace**.

Not every table changed. Each table was explicitly sharded by the column that made the most sense for it: user ID, channel ID, *or* workspace ID. This created a simpler path to spreading out data load across shards and enabling cross-workspace communication.

*Sharding the messages table by channel across shards smoothed out load and unlocked new product opportunities*

### [Explicit vs automatic sharding](https://planetscale.com/blog/hot-shards-hotter-tenants#explicit-vs-automatic-sharding)

Among other benefits, shard allocation was no longer hidden in application logic and a separate metadata cluster. It was defined explicitly in Vitess and enforced by VTGate. Neki's equivalent is the data topology: a declarative configuration that agents and humans can read to reason about where data is written and where it can be read from. You define the columns on which specific tables are sharded and the range of values each shard receives.

This is in contrast to automatic sharding solutions, where the database decides placement for you, which can result in unexpected or unpredictable placement of rows.

Agents love declarative configuration like a data topology because it's foolproof to reason about exactly where data will land and why. Which tables are sharded, which key they're sharded by, and which shards sharded rows are sharded to are all determined by the data topology.

At any time, you can add shards and change sharding strategies. After which, [Reshard](https://planetscale.com/docs/neki/data-migration#reshard-existing-tables) copies existing rows to new shards as required and streams ongoing writes while the source keeps serving. When you switch traffic, Neki moves reads and writes to the new placement without taking the application offline.

## [Solving resharding with Neki](https://planetscale.com/blog/hot-shards-hotter-tenants#solving-resharding-with-neki)

Should you suffer from the same success, and have already migrated to Neki, you're in a great position as it gives you the tools to mitigate increased demand and/or adjust your sharding strategy with minimal effort and disruption.

Here's three ways to handle increased demand and requirements. The first two buy you time, the last one is the best long-term solution.

### [Fix 1: Scale up](https://planetscale.com/blog/hot-shards-hotter-tenants#fix-1-scale-up)

If increased demand has your databases hitting their limits, the easy answer is just "add more resources." You could do that and stop here.

Each shard in a Neki database is a distinct Postgres cluster with its own resources. Configuration profiles can be distinct or shared across shards. You can temporarily solve your large tenant problem by assigning its shard a unique profile, vertically scaling it, and going about your business.

On Vitess, [per-shard sizing](https://planetscale.com/docs/vitess/cluster-configuration/per-shard-sizing) does the same job by giving a hot shard a larger cluster size than the rest of its keyspace.

### [Fix 2: Isolate large tenants](https://planetscale.com/blog/hot-shards-hotter-tenants#fix-2-isolate-large-tenants)

Additionally, if your larger tenants are causing issues for their neighbours, you can isolate the range of tenants on any one shard.

Here's a visual representation of distributing rows across shards. Note that `xxhash` doesn't create a perfectly even balance of this small dataset, but would be relatively even over 1000s or more rows.

Since you're in control, distribution can be as broad or fine grained as you like. In a simplified sharding example, an even distribution of hashed IDs across two shards would look like this:

```json
{
  "key_ranges": [
    { "shard_uid": "shard-a", "end": "80" },
    { "shard_uid": "shard-b", "start": "80" }
  ]
}
```

The start and end ranges in the code example above are only matching the first two characters of a hashed ID. You can go much finer and create a tighter range around the whale's `tenant_id`.

Isolating the whale's data will require resharding all tenants' data. Reshard copies rows onto new shards, so we cannot re-use the original source shards `shard-a` and `shard-b`. With three new shards created, the new layout below describes an updated placement to isolate the hot tenant's data.

```json
{
  "key_ranges": [
    { "shard_uid": "shard-c", "end": "80a3f1" },
    { "shard_uid": "whale", "start": "80a3f1", "end": "80a3f2" },
    { "shard_uid": "shard-d", "start": "80a3f2" }
  ]
}
```

However, you've now created an environment where one very large tenant can have one extremely large table. A table so large it too would benefit from being sharded. If CPU demands don't get you, storage will. It may be time to make some structural sharding changes.

### [Fix 3: Reshard some tables](https://planetscale.com/blog/hot-shards-hotter-tenants#fix-3-reshard-some-tables)

Vertical scaling and isolation of a tenant will only get you so far, but neither of the previous two fixes solves the root cause of your problem. Yesterday's sharding strategy is unsuitable for today's requirements.

Changing sharding strategy doesn't mean sharding or resharding everything. Unless you're Meta or Google you probably don't need to shard your users table.

Investigate your application's access patterns and query shapes to work out what other dimensions data can be sharded by. Look for tables which are regularly joined by a common column key, reshard so they are kept together.

Back to Slack's example, messages were always queried by channel, never by workspace, and channel ID was already part of the messages table's primary key. The right shard key had been there all along. Sharding messages by channel ID made fetching a channel and its messages a single-shard query, even when those messages were authored by users in different workspaces.

Cross-channel queries for messages, which would now be cross-shard queries, were limited to administrative or batch operations, not the critical path. This change cooled off hot spots and gave the team a lot more runway in terms of CPU and storage. Meanwhile, keeping the users table together was critical, as searching for "all users in this workspace" was a common access path.

While resharding isn't something you'll want to do often, it's simpler once you are already within a sharded database. During resharding, Neki will continue to serve queries to existing data until the operation is complete.

## [Conclusion](https://planetscale.com/blog/hot-shards-hotter-tenants#conclusion)

Landing big customers and growing your product are good problems to have, but how much stress it causes you depends on the foundation you already have in place. With Neki as that foundation, you are choosing something you and your agents can easily understand, change, and adapt to, no matter which dimension your data grows by.

The longer you wait, the more tables outgrow their original shard key. What could have been one change becomes several at once. Don't wait for a perfect future state. Shard the table that hurts today and address the rest as they need it.

Shard what hurts now.

## [Footnotes](https://planetscale.com/blog/hot-shards-hotter-tenants#footnotes)

1. [Scaling Datastores at Slack with Vitess](https://slack.engineering/scaling-datastores-at-slack-with-vitess/)
2. [Unified Grid: How We Re-Architected Slack for Our Largest Customers](https://slack.engineering/unified-grid-how-we-re-architected-slack-for-our-largest-customers/)
3. [Scaling Slack](https://www.infoq.com/presentations/slack-scalability-2018/)
