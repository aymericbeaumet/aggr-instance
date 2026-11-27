---
title: What today's software owes to Bronze Age clocks
link: https://planetscale.com/blog/leaky-buckets-in-software-explained
source: planetscale-com
published: 2026-10-07T00:00:00Z
updated: 2026-10-07T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Simeon Griggs
labels:
- engineering
- postgres
summary: Admission controls in software determine what does or doesn't happen next, and the most common implementation has ancient roots.
content: extracted
html: 2026-10-07-what-today-s-software-owes-to-bronze-age-clocks.html
remote_preview:
  url: https://planetscale.com/assets/leaky-buckets-in-software-explained-social-DUvB1rD9.jpg
---

Here's a deceptively simple question: should this function execute?

Sometimes, the answer is obvious. Is the user logged in? Does their role have the required permissions?

Sometimes it's less obvious. Are there sufficient resources available? Is this work a higher priority than everything else?

There are many names for the mechanisms that block or allow actions in software. Most commonly they are referred to as **admission control**. Perhaps you'll be familiar with a **leaky bucket** implementation.

## [A brief history of leaky buckets](https://planetscale.com/blog/leaky-buckets-in-software-explained#a-brief-history-of-leaky-buckets)

Engineering applications for leaky buckets predate computers. From around 1600 BC the Babylonians, Egyptians, and Persians all used vessels with a small hole in the bottom as primitive clocks, measuring elapsed time by the volume of the container as it drained at a constant rate.

By around 200 BC the Greeks had advanced leaky buckets into accurate clocks. A floating regulator kept water flowing steadily from one bucket into a second, where a rising float moved a pointer to show the hour. A siphon emptied the second bucket once it was full. Most notably, Ctesibius added gears to regularly advance each "tick" of the clock, turning the display which indicated the current season.

A mechanical movement driven by a leaky bucket was born, and it became the gold standard of timekeeping for centuries.

Water-based clocks were used as a form of admission control. In Athenian courts, the length of time a speaker had to plead their case was measured by the time water took to drain from a vessel. The amount of water poured into the vessel was determined by the severity of the case.

Finally, in the computer age, Jonathan Turner is said to have used the term "leaky bucket" first in 1986, writing "New directions in communications" for IEEE Comms Magazine. The piece describes how a system can police the pace at which packets are sent over broadband connections. (If you haven't read it, spoiler: it's not much slower than your connection at home today.)

## [Leaky buckets in software](https://planetscale.com/blog/leaky-buckets-in-software-explained#leaky-buckets-in-software)

There are two kinds of leaky buckets in software, to implement a **meter** or a **queue**.

In both, each unit of "work" is assigned a value which is checked against the current volume of a "bucket." If the bucket lacks capacity for the size of new work, that work is rejected. Otherwise the work is allowed and its value adds to the bucket's current volume. The bucket drains (or leaks, if you will), so over time, new work can't be admitted faster than the bucket drains. The drain rate is based on time elapsed, not on the completion of work.

When used as a **meter**, the bucket defines if and how much work is permitted to *immediately proceed*. As a **queue**, the bucket defines the volume of work that can *wait to proceed*, but work only begins as it drains from the bucket.

In this post we'll focus on leaky buckets as a meter.

Typically, leaky buckets are implemented in software to keep systems healthy. A sort of resource management to ensure the system is not overwhelmed.

It's not hard to imagine this being useful in a database. Imagine being able to block or allow queries, before they execute, based on available resources or concurrency limits. One can dream!

*The leaky bucket can be tweaked by modifying its volume and the rate at which it drains.*

At its core a leaky bucket prevents work, but it also forces us to reason about the total volume of work that results in a block, how quickly more capacity becomes available, and how to handle the expected failure state when work is rejected.

### [Leaky buckets in API design](https://planetscale.com/blog/leaky-buckets-in-software-explained#leaky-buckets-in-api-design)

Shopify's GraphQL Admin API implements rate limiting with a [documented leaky bucket](https://shopify.dev/docs/apps/build/apis/graphql-admin/rate-limits). Each app on each store gets a bucket with a fixed size and restore rate.

Each request costs a different amount based on what the query asks for, not how many requests are made. Before a query runs, the bucket must have room for its estimated cost. Once it completes, the bucket is refunded or charged any difference between the estimated and actual cost. Apps can burst faster than the restore rate until the bucket has no room left, at which point requests are throttled.

Responses include the current state of the bucket, so apps can slow down before they're throttled and treat throttling as an expected outcome, not a surprise.

### [Classification as admission control](https://planetscale.com/blog/leaky-buckets-in-software-explained#classification-as-admission-control)

Now let's consider the AI age. Jev, a "System One" model (perhaps better named a "[Decision Model](https://x.com/Mappletons/status/2101560333441610133)"), recently launched, making classification through inference cheap and fast. It could classify "should this work proceed or not" based on a predefined set of options and return a response in hundreds of milliseconds.

Note

Fun full-circle fact: In 1907, the French scientist Lapicque modeled neurons as a charge that accumulates, leaks away, and fires at a threshold, later called the "leaky integrate-and-fire" model. So something like a leaky bucket has long been used to describe **neurons**, making it part of the origin story of **neural networks**, and so of Jev.

Whether our decision gate is *mechanical* (leaky bucket) or requires *inference* (Jev), the requirement is the same: answering "should this proceed?"

### [Low cost throttling](https://planetscale.com/blog/leaky-buckets-in-software-explained#low-cost-throttling)

One of the great benefits of leaky buckets in software is how cheap they are. What we've described as a slowly draining bucket isn't a long-running process that is using CPU to "drain".

Simply put, a bucket is a stateful object that holds two values:

1. **how full** it was the last time anyone looked
2. **when** that was

From those values, you can work out how full the bucket is now:

```
level_now = max(0, level_then - drain_rate × (now - then))
```

Any work rejected or admitted by the bucket updates its current state. In this way, buckets are lazily drained (as in, only when checked) and empty buckets hold no values.

## [Admission controls and databases](https://planetscale.com/blog/leaky-buckets-in-software-explained#admission-controls-and-databases)

Just like hardware, operating systems, applications, and ancient Greeks, databases benefit from leaky buckets.

However, open-source databases like Postgres and MySQL don't have this functionality baked in.

One of the most common failure modes for databases in production is the inability to deliberately reject work they cannot complete. Blast a billion queries at your typical relational database, and it'll try its best to execute them. Success isn't likely. Some hardware, configuration, or connection limitation will unwittingly play the role of an admission control system and start blocking work in some unhandled, undesirable way.

Some proprietary databases have a form of resource management so that workloads can be throttled (but not rejected). For example, in 1999 the Oracle Database Resource Manager added mapping rules to sessions for users, services, clients, and more, which could then be used to apply limits on things like resource use and concurrency.

In 2008, SQL Server added the SQL Server Resource Governor, which added classifier functions to sessions that can then apply rules to queries in that session.

Postgres has some configuration options to control expensive queries that have *already* consumed resources, such as `statement_timeout`, but nothing to prevent expensive work before it happens. And so in 2026, PlanetScale brought decision gates to Postgres with Database Traffic Control®.

## [Database Traffic Control](https://planetscale.com/blog/leaky-buckets-in-software-explained#database-traffic-control)

The health of your Postgres database is laid bare at the mercy of your applications. An unexpected connection storm or [failure of the query planner](https://planetscale.com/blog/when-the-postgres-query-planner-goes-rogue) can ask more of your database than it can reasonably be expected to handle.

To protect your Postgres database from your application (and at times, from itself), we built Traffic Control, admission control for Postgres. In it, resource budgets determine how many resources or how much concurrency one or many queries can consume.

Each budget admits or blocks queries that match a specified set of tags. Those tags can be specified as comments in the query, or they can refer to the user or application that sent it. If the resource budget determines the query should not proceed, it will log a warning or block it, depending on your resource budget configuration.

Creating a resource budget opts you into an expected failure state, so it forces you to reason about the behavior your application should exhibit when queries are blocked. For this reason, we recommend creating a new budget in warning mode first, monitoring how much traffic is affected, preparing your application for blocked queries, and then updating the mode.

### [Admission controls but for traffic](https://planetscale.com/blog/leaky-buckets-in-software-explained#admission-controls-but-for-traffic)

You configure [three major settings](https://planetscale.com/docs/postgres/traffic-control/concepts#resource-budget-limits) on a resource budget.

**Server share and burst limit**, the token\* bucket. Server share determines how fast the bucket replenishes with tokens, expressed as a percentage of server resources. The burst limit sets the maximum capacity of the bucket, expressed in whole-server seconds.

Because a database doesn't know the cost of a query before execution, Traffic Control estimates its cost based on the query plan and historical behavior of queries with that shape. If the budget has sufficient capacity the query proceeds. After execution, the cost of the query is calculated and any difference is either refunded or charged to the token bucket.

Why is the refill rate measured in server capacity instead of seconds? Because not all time spent executing a query is equal. A query that took 8 cores a single second is different from a query that took one core a single second. Consider different server sizes as well: a 25% server share means a quarter of your server's capacity drains back every second, whether that server has 2 cores or 64.

Note

\*Hold on, **token** bucket?

A *token* bucket works the same as a *leaky* bucket, but with a reversed metaphor. The bucket starts filled with "tokens" and each piece of work is assigned a "cost." Work is only admitted if there are sufficient tokens remaining for that work. Tokens refill over time. It's a bit easier to reason about the remaining capacity of a resource budget in terms of a bucket's remaining tokens than its current fill level.

Naming things, right?

**Per-query limit** sets an upper limit on the maximum capacity a single query can consume. This makes an admission-control decision directly on a single query instead of measuring it against available capacity in a bucket.

**Maximum concurrent workers** similarly controls the resources a single query can consume, but measures it by available workers instead of execution time.

A query may be targeted by multiple resource budgets and must satisfy all of them to proceed.

For specific scenarios where resource budgets help, along with how to tag queries in your codebase, see [Patterns for Postgres Traffic Control](https://planetscale.com/blog/patterns-for-postgres-traffic-control).

## [Summary](https://planetscale.com/blog/leaky-buckets-in-software-explained#summary)

Admission control forces you to decide what should happen when work can't proceed, and leaky buckets have been a cheap way to make that decision for a very long time. Traffic Control makes that decision for Postgres before an expensive query runs, not after.

Get started with [Database Traffic Control](https://planetscale.com/docs/postgres/traffic-control)
