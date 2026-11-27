---
title: Building PlanetScale with Insights Agent
link: https://planetscale.com/blog/building-planetscale-with-insights-agent
source: planetscale-com
published: 2026-11-19T00:00:00Z
updated: 2026-11-19T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Enrique Gonzalez
labels:
- engineering
- mysql
summary: 'Three fixes Insights Agent made to our own Rails API: a reordered feature flag check, an index that made a query slower, and an index that took a query from seconds to milliseconds.'
content: extracted
html: 2026-11-19-building-planetscale-with-insights-agent.html
remote_preview:
  url: https://planetscale.com/assets/building-planetscale-with-insights-agent-social-IEdF-Yp_.png
---

We've always used PlanetScale to build PlanetScale. So when we built Insights Agent, our own databases were the first place we ran it to see what kind of gains we could get. Insights Agent identifies problematic queries or schema using Query Insights, finds the code responsible for them, and opens a pull request with a fix. Our Rails API is a large, busy app, which makes it a good testing ground.

We know a thing or two about databases over here, so our databases are already quite well optimized. Even so, Insights Agent has been able to find a bunch of wins for us, continuing our drive towards the most efficient database possible.

Here are three (of many) examples of how we tested and used Insights Agent in the weeks leading up to launch, and how we used these experiences to make the product better.

## [A feature flag check we didn't need](https://planetscale.com/blog/building-planetscale-with-insights-agent#a-feature-flag-check-we-didnt-need)

Our Rails API checks feature flags with Flipper. On top of Flipper, we have an override that can turn a flag off for specific records. Checking the flag requires reading it from the database. Our code checked the override first, every time:

Each lookup took about a millisecond, which doesn't raise any significant slow-query concerns at first look. However, when this happens ~28 million times per day, it added up to more than nine hours of database CPU time. Insights Agent found it by the aggregate time instead of individual execution time. The override can only turn a flag off, so Insights Agent moved it after the Flipper check.

```diff-ruby
  def feature_enabled?(name)
-   if override_disables?(name)
-     false
-   else
-     Flipper.enabled?(name, actor)
-   end
+   Flipper.enabled?(name, actor) && !override_disables?(name)
  end
```

If a flag is already off, there's nothing to override. The app behavior remains exactly the same as before, but now with far fewer lookups on these values:

|                         | Before       | After       |
| ----------------------- | ------------ | ----------- |
| Calls per day           | 28.7 million | 8.3 million |
| Database time per day   | 33,379 s     | 9,625 s     |
| Share of all query time | 3.6%         | 1.1%        |

In this case, the traffic volume to the database did not change, but by careful inspection of both the database load and the codebase, the Insights Agent was able to significantly reduce load here.

## [An index that made a query slower](https://planetscale.com/blog/building-planetscale-with-insights-agent#an-index-that-made-a-query-slower)

Next, we asked Insights Agent to fix a slower query. This query looks up billing prices for an account, about 570,000 times a day (names simplified):

```sql
select prices.* from prices
where prices.owner_id = ?
  and prices.metric in (<elements>)
  and prices.owner_type = ?
  and prices.region_id is not null
```

Insights showed this query reading about 122 rows for every returned row. This is often a tell that a query can be sped up. Insights Agent observed that the index MySQL used for this lookup had a range on `region_id` ahead of `metric`, and decided to add a new index with `metric` first. This might seem reasonable at first glance, but we merged it, and the query got slower:

|         | Before the index | After the index |
| ------- | ---------------- | --------------- |
| Average | 27.3 ms          | 47.6 ms         |
| p99     | 129 ms           | 222 ms          |

MySQL wasn't using the new index!

## [Insights Agent was missing... something](https://planetscale.com/blog/building-planetscale-with-insights-agent#insights-agent-was-missing-something)

Insights Agent was working from a summary that left out a few important bits of context.

First, almost none of these calls return a row, because most accounts have no prices to show. When the number of rows returned is zero (or near-zero), rows read per row returned looks terrible.

Second, our Insights extension collapses `IN` lists when it normalizes SQL, which means `<elements>` was hiding 645 metric names, about 18 KB of SQL on every call. MySQL builds a lookup range for each of these values for every index that could use them. The new index gave it one more index to do that for.

So we stopped sending the list of elements. The query now loads the few dozen regional prices an account has and filters them in Ruby:

```ruby
account.regional_prices.select do |price|
  price.region_id == region.id && BILLABLE_METRICS.include?(price.metric)
end
```

|                       | Old query | New query |
| --------------------- | --------- | --------- |
| Database time per day | 16,212 s  | 1,129 s   |
| Average               | 28.5 ms   | 1.95 ms   |
| p99                   | 137 ms    | 8.0 ms    |

That's about 14 times less database time. The new query also uses the index Insights Agent added, so we kept it.

### [Teaching Insights Agent what we learned](https://planetscale.com/blog/building-planetscale-with-insights-agent#teaching-insights-agent-what-we-learned)

We have updated Insights Agent to look at more evidence to be better at these types of investigations. For these, it must look at rows read and returned per call *and* the size of the SQL sent, including how many values are collapsed. And we're extending our eval harness to cover cases like this one, including the cases where the best option is to leave the query as-is. Sometimes no change is better than adding an index.

## [More indexing work](https://planetscale.com/blog/building-planetscale-with-insights-agent#more-indexing-work)

Yet another indexing adventure here. A job in our Rails API runs every minute to find schema snapshots that need to be reconciled (query simplified):

```sql
select snapshot.id from snapshot
join branch on branch.id = snapshot.branch_id
where snapshot.ready = ?
  and snapshot.failed_at is null
  and snapshot.created_at > ?
  and branch.ready = ?
order by snapshot.id asc
limit ?
```

The only index MySQL could use for this was one created on `(ready, migration_applied_at)`. This index helps MySQL find unready snapshots. However, it does not help with the other filters because `failed_at` and `created_at` are not in it (as well as a few others which were removed). MySQL had to read every unready snapshot (`ready = false`) and check the additional columns on every row.

There are a lot of unready snapshots in the database, but only a very small subset are both `recent` and not failed. MySQL had to find every match before it could sort by `id` and return the first few. Each call was reading 100s of thousands of rows to return, typically, less than 100 and was taking multiple seconds. The rows read / rows returned ratio on this query was really bad.

Insights Agent suggested adding an index on `(ready, failed_at, created_at)`:

```ruby
add_index :snapshot, [:ready, :failed_at, :created_at]
```

With `ready` and `failed_at` now included, MySQL can jump straight to the right range of `created_at` instead of reading every unready snapshot. Another query that counts unready snapshots for alerting had a similar issue, and was given a fix by the agent. In this case, the only change needed to the app-side code was a database migration.

Both p50 and p95 dropped from seconds per call to millisecond-scale.

## [Wrapup](https://planetscale.com/blog/building-planetscale-with-insights-agent#wrapup)

We spent weeks testing, improving, and optimizing the experience with Insights Agent in the weeks leading up to launch. While doing so, we used it to automate the discovery of a bunch of places where we could improve our code, our database schema, and our queries. If you want a faster database (and we know you do) go give [Insights Agent](https://planetscale.com/docs/insights-agent) a try on your database, today.

If you prefer doing all these kinds of optimizations in your own agent harness, we have some [docs to get you up and running](https://planetscale.com/docs/self-improving-database) on this as well.
