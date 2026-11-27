---
title: Why you shouldn't do application-level sharding
link: https://planetscale.com/blog/why-you-shouldnt-do-app-level-sharding
source: planetscale-com
published: 2026-11-23T00:00:00Z
updated: 2026-11-23T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Ahmed Darwich
labels:
- engineering
summary: Application-level sharding feels like the right solution. But it isn't your core business, and it causes more headaches than you think.
content: extracted
html: 2026-11-23-why-you-shouldn-t-do-application-level-sharding.html
remote_preview:
  url: https://planetscale.com/assets/why-you-shouldnt-do-app-level-sharding-social-DYgF36VU.png
---

This is application-level sharding.

When an OLTP database like Postgres or MySQL grows so large that it no longer "fits" on one server, the common approach to scaling is sharding. One way to do it is app-level sharding, where all the logic for which data goes to which server is embedded in your application code.

Companies that grow fast tend to face this problem, and app-level sharding seems like the easy, obvious solution. However, there be dragons. To find them, let's walk through the first thousand days of a fast-growing online store after sharding.

## [Sharding in the application](https://planetscale.com/blog/why-you-shouldnt-do-app-level-sharding#sharding-in-the-application)

The store is a marketplace: buyers, sellers, orders and payments all live in one Postgres database, and as traffic grows, that database starts to fall behind.

You might think the answer is read replicas. And you should use them. But they have no impact on your writes, which still all go to the primary. A bigger server can buy you time, but there's a cap to how big a server you can buy. Even before that moment, the sheer size of the database will cause you issues: backups and restores taking hours, or even replicas falling behind on write spikes. The operational challenges of running a large database are not for the faint of heart.

So you decide to shard. You pick `user_id` as the shard key and split the store's users across four databases, where each one stores and serves its own share. Buyers and sellers are both users, and each user's data lives on their shard: a buyer's orders and balance, a seller's listings and payouts. If sharding is new to you, [What is database sharding and how does it work?](https://planetscale.com/blog/what-is-database-sharding-and-how-does-it-work) and [Making 768 servers look like 1](https://planetscale.com/blog/making-768-servers-look-like-1) cover the basics.

The first version is usually one function.

```python
SHARDS = [
    connect("db0"), connect("db1"),
    connect("db2"), connect("db3"),
]


def shard_for(user_id):
    return SHARDS[user_id % 4]


def get_user(user_id):
    return shard_for(user_id).query(
        "SELECT * FROM users WHERE id = %s", user_id
    )
```

Writing it takes an afternoon. The new infrastructure is three additional databases to run. But your queries look the same as yesterday, as long as you have a `user_id` in hand.

Before `shard_for` can be deployed, however, every user needs to move to their assigned shard. For example, user 13 belongs on shard 1, since 13 % 4 = 1. Moving them with minimal downtime takes six steps:

1. Copy user 13's rows from the original database to shard 1, and move shard 1's ID counter past them.
2. Keep copying new writes from the original database to shard 1 until the deploy.
3. Check that user 13's rows match on both databases.
4. Deploy `shard_for`, so user 13's reads and writes go to shard 1.
5. Keep copying new writes from shard 1 back to the original database, in case you need to roll back.
6. Once you're sure you won't, delete user 13's rows from the original database.

At [Intercom](https://www.intercom.com/blog/evolving-intercoms-database-infrastructure-lessons-and-progress/), which ran its own sharding layer for years, sharding even a single table "took months and required careful coordination, manual data migrations, and application-layer changes."

`% 4` as a sharding algorithm is brittle. But it's easy to follow, so it's what we'll use to walk through the problems. Most production teams will use something better, like consistent hashing, but they will face similar problems.

Day 1 is over and you have four functioning shards.

## [Resharding from four shards to eight](https://planetscale.com/blog/why-you-shouldnt-do-app-level-sharding#resharding-from-four-shards-to-eight)

The store keeps growing, and with Black Friday a few weeks out, it's clear your four shards won't keep up.

You can double the number of shards from four to eight. This is a pretty easy change in your code. Just add four more database connections to `SHARDS` and change `% 4` to `% 8`.

But user 13's rows are on shard 1, where `% 4` put them. Once you deploy `% 8`, `shard_for` sends user 13's queries to shard 5, which is currently empty. Half the users on the original four shards have the same problem.

So you run the same six steps again, this time from shard 1 to shard 5.

Step 4, deploying `% 8`, is the riskiest part, because your store doesn't run as one process. All of your processes (web apps, workers, cron jobs) have their own `shard_for`. They don't deploy at the same time either, so for a few minutes they can disagree about where user 13 lives.

Remember that at step 4 the copying flips direction. Before the deploy, writes on shard 1 get copied to shard 5. After it, writes on shard 5 get copied back to shard 1.

The issue arises when you have a worker and your web app on different versions. For example, say user 13 cancels an order on the web, which is deployed using `% 8`, so the cancel goes to shard 5. Behind the scenes, you are copying that back to shard 1. Your worker, which sets up shipments, marks the order as shipped on shard 1, since it hasn't been deployed with `% 8` yet. But nothing is copying that change from shard 1 to shard 5 anymore. And so when you deploy the new worker, it reads shard 5, and you lose that shipment.

You can prevent this by making the old shards read-only during the switch. But then every write to them fails until every process has the new code, which means customers cannot check out, add items to cart, etc during that window. Teams that have been through this move `SHARDS` and `% 8` into a config service that every process watches and updates as needed. This creates another service for your team to maintain and just shrinks the window where issues like this can happen. You will still have to pause writes for users being moved when it updates.

## [When a query doesn't have a `user_id`](https://planetscale.com/blog/why-you-shouldnt-do-app-level-sharding#when-a-query-doesnt-have-a-user_id)

You make it through Black Friday with your new topology of eight shards. A week later though, a customer emails support with an order number of 1042 and nothing else. This exposes a number of issues in your application layer that you now have to handle. How do you know which shard holds the order you are querying for if you don't have `user_id`?

### [Pass the `user_id` along](https://planetscale.com/blog/why-you-shouldnt-do-app-level-sharding#pass-the-user_id-along)

Most of the time, this issue doesn't come up, because your chain of function calls has a `user_id` *somewhere*. You decide to just pass it down to any function that needs it:

```python
# Day 1
def get_order(order_id): ...
def refund_order(order_id): ...
```

Every function that didn't take `user_id` as a parameter now does.

```python
# Day 1000
def get_order(order_id, user_id): ...
def refund_order(order_id, user_id): ...
def send_receipt(order_id, user_id): ...
```

In this case, with the customer support email, however, you don't have a `user_id`.

Here's how the store keeps orders:

```sql
CREATE TABLE orders (
    id      bigserial PRIMARY KEY,
    user_id bigint NOT NULL,  -- the buyer, and the shard key
    total   numeric NOT NULL
);
```

Every order lives on its buyer's shard, so each of the eight shards has its own `orders` table holding its own buyers' orders. So you need to find the shard from the order ID alone, and there are three ways to do it.

### [Ask every shard](https://planetscale.com/blog/why-you-shouldnt-do-app-level-sharding#ask-every-shard)

The simplest way is to send `SELECT * FROM orders WHERE id = 1042` to all eight shards and check each response for a result. That's eight Postgres queries for one order lookup. And in this situation, it actually returns two orders.

You might be wondering why. `bigserial` gives each `orders` table its own counter, and now there are eight `orders` tables. Shard 1 and shard 5 both reached 1042 at some point, for two different customers. So asking every shard is slow, and it can't even tell you which 1042 the customer meant.

### [Put the shard in the ID](https://planetscale.com/blog/why-you-shouldnt-do-app-level-sharding#put-the-shard-in-the-id)

Both of those problems come from the ID itself. You can use UUIDs instead to solve half of the problem. No two orders share a UUID, but nothing in a UUID points to a shard, so you'd still need eight queries to find one order.

[Another way, used by Instagram,](https://media.postgresql.org/sfpug/instagram_sfpug.pdf) is to build the shard into the ID itself.

```sql
-- 64-bit id: 41 bits of milliseconds | 13 bits of shard id | 10 bits of sequence
result := (now_millis - our_epoch) << 23;
result := result | (shard_id << 10);
result := result | (seq_id);
```

With this ID structure, you can read a row's shard straight out of the ID with a bit shift. This fixes the duplicate orders problem, since every ID carries its own shard number.

But there is a catch. This assumes that any future reshard would not move the order. For example, if the ID said "shard 5" and the next reshard moved it to "shard 1", every old ID for a moved order would point to the wrong shard. Instagram's IDs instead pointed to one of a few thousand small buckets, called logical shards. They used a separate table to map each of those buckets to whatever server held it. That way, a reshard would move whole buckets and update that table, so their IDs would never change.

### [Keep a lookup table](https://planetscale.com/blog/why-you-shouldnt-do-app-level-sharding#keep-a-lookup-table)

Putting the shard in the ID works for IDs you create. Not every ID comes from you, though. Imagine your payments team now ships a webhook for disputed charges. When a customer disputes a charge with their bank, the payment provider calls your server to tell you. They provide you with the charge ID, but you have no way of routing that to the correct shard.

The third way is to keep a lookup table that holds the mapping between order IDs, charge IDs and `user_id`. Every request, though, would first need to access the lookup table to determine which shard to go to.

Now your code has to keep that table in sync with the shards, reshards included. And since every request goes through it, if it's down, so is the store. [At Etsy](https://www.etsy.com/codeascraft/migrating-etsyas-database-sharding-to-vitess), sharded queries looked up their shard in one unsharded index database, and "if the index database went down, it could cause a full site outage."

### [What none of these fix](https://planetscale.com/blog/why-you-shouldnt-do-app-level-sharding#what-none-of-these-fix)

Everything so far that we have discussed has helped you find one order. A real store needs more than that. What if seller 27 wants to see all of this week's orders on their dashboard? If you remember, each order lives on whatever shard its buyer is on. So seller 27's orders are spread across all eight shards. No lookup table or clever IDs will help with that. The app has to query all eight shards, merge, sort, and page the results for the user.

## [When one seller becomes a hot shard](https://planetscale.com/blog/why-you-shouldnt-do-app-level-sharding#when-one-seller-becomes-a-hot-shard)

Black Friday was a massive success, and a few months later, your store's marketplace attracts a big brand. One issue, though: its inventory updates and payouts write thousands of rows a second. All of those rows are keyed by the brand's `user_id`, so it all goes to the same shard. The orders spread out fine, since each lives on the buyer's shard, but the brand's shard becomes a [hot shard](https://planetscale.com/blog/hot-shards-hotter-tenants). Every other user on that shard is impacted, and their requests slow down. This never happened before, since sellers were mostly the same size.

A reshard won't fix it, since this brand will still be hot on its new shard. Another option is to give the brand its own shard. That way, its neighbors stop feeling the load of the big brand. Doing that means a reshard, but just for the big brand. `shard_for` also now needs a list of exceptions:

```python
DEDICATED = {48213: connect("db-brand")}  # the brand's user_id

def shard_for(user_id):
    if user_id in DEDICATED:
        return DEDICATED[user_id]
    return SHARDS[user_id % 8]
```

And you'll do this again and again and again for each big customer that comes after.

Another option is to give every shard plenty of headroom up front, so you never have to move anyone. That's what [Slack](https://slack.engineering/scaling-datastores-at-slack-with-vitess/) did. It was hard for them to split shards and move teams, so they "over provisioned most of the shards, leaving the long tail underutilized."

Giving the brand its own shard or even leaving extra headroom stops working, though, once you can't buy a bigger server. At that point, you have to split the big brand's data across several shards. That means sharding its rows by a different key, for example, the listing each row belongs to. Everyone else would stay on `user_id`. And that seller will need its own routing-specific code in `shard_for`.

## [Cross-shard transactions](https://planetscale.com/blog/why-you-shouldnt-do-app-level-sharding#cross-shard-transactions)

Up until now, every write that moved money only touched one user. Even the big brand's. But a year in, the store decides to launch store credit. Think of store credit as gift-card money you spend in the store. Writes can now involve two users' balances, and both have to happen correctly and at the same time. It has to be all or nothing, because you can't show a user an incorrect balance.

This might seem like an easy feature, but on a sharded database, it's a little bit more tricky. Say user 14 spends $10 of store credit with seller 27. Two things have to happen for this to be successful. User 14's balance has to go down by $10, and seller 27's balance has to go up by the same amount. The problem with this is that these two balances live on two different shards. User 14 is on shard 6 (14 % 8 = 6), and seller 27 is on shard 3 (27 % 8 = 3). So one store-credit purchase has to write to two separate databases.

On a single database, this is easy. It's a committed transaction that is promised to be all or nothing. Either both changes stick or neither does. But with two databases, it doesn't work that way. There isn't a shared transaction. Each shard completes its own operation, and each can independently fail.

The result of a failure is a user reporting an incorrect balance.

You might be asking, "OK, how do we fix this?" Well, this is exactly why two-phase commit exists. You effectively have a coordinator that goes to both shards and prepares them for the transaction. Each shard locks the row first, and then responds that it's able to handle the change. Once both have said yes, the coordinator writes COMMIT to its own log and tells both shards to commit. When both are done, they unlock the rows.

You can tell there are two downsides to this. First, both rows are locked the whole time, so this isn't an optimal write. Second, if the coordinator crashes after both shards have said yes, the rows will stay locked until the coordinator comes back and finishes the operation.

Two-phase commit also isn't easy to run. You have to build and operate the coordinator yourself. It has to keep a reliable log, recover correctly after crashes, and never lose track of a transaction. That's a lot to get right, and it impacts all of your users. Most teams decide not to do this and instead use a saga.

A saga breaks up the operation into two separate saves. In our example, it takes $10 from user 14 on shard 6, then gives $10 to seller 27 on shard 3. To do this, it writes to an outbox in the same transaction that takes the $10 from user 14 on shard 6. An outbox is essentially a to-do note living on user 14's shard. A worker then reads the outbox and keeps retrying the credit on shard 3 (seller 27's shard) until it succeeds. Not only does it have to work, it also has to be idempotent. You can't pay seller 27 multiple times for the same order. If it hits an error that retrying won't fix, like seller 27's account being closed, it runs an undo and returns the $10 to user 14 on shard 6. Until the worker finishes, the $10 has left user 14 but hasn't reached seller 27, so your application needs a "pending" state to show in the meantime.

## [Schema changes across every shard](https://planetscale.com/blog/why-you-shouldnt-do-app-level-sharding#schema-changes-across-every-shard)

Store credit is a massive hit, and with more purchases come more refunds. The price of success as some will call it. Internal analytics starts asking your dev team, "When was this order refunded?" If you remember the original table, there is no `refunded_at` column. You might think changing the shape of the tables is easy, but once again, sharding will make you reconsider. With a single database, it's a single DDL statement:

```sql
ALTER TABLE orders ADD COLUMN refunded_at timestamptz;
```

Run that once, and you can give analytics what they need. With nine shards (the eight plus the brand's), however, it's not as simple, and there are nine chances for it to fail. Not only do you have to apply the DDL to each shard, you have to track which failed and retry them. Furthermore, your app has to be able to work with and without the column until the DDL is complete, creating even more operational overhead.

But wait, it can get worse. Adding a column always takes a brief lock on the table, and one long-running transaction on a shard can jam that shard:

```sql
-- shard 5: a report that has been running for 20 minutes
BEGIN;
SELECT user_id, sum(total) FROM orders GROUP BY user_id;

-- your migration, on the same shard: waits for the report to finish
ALTER TABLE orders ADD COLUMN refunded_at timestamptz;

-- every new query on orders now waits behind the ALTER
SELECT * FROM orders WHERE id = 1042;
```

Even worse, index builds and backfills can fail halfway, leaving one shard in a different state from the rest:

```sql
CREATE INDEX CONCURRENTLY orders_refunded_at_idx ON orders (refunded_at);
-- fails on shard 3, which is left with an INVALID index that someone has to find and drop
```

To handle this, your team ends up building a complex rollout tool. The rollout tool has to go shard by shard, make sure each completes successfully, retry whatever fails at the point of failure, and warn about any shard that has drifted.

## [Failover, backups and connections](https://planetscale.com/blog/why-you-shouldnt-do-app-level-sharding#failover-backups-and-connections)

Soon enough, you get to experience your second Black Friday. And with the increased throughput and popularity of your store, shard 3's primary dies. Now, a normal failover is fairly easy to deal with. You promote a replica to be the new primary, point your connections to it, and move on. It's routine; every DBA has done it. With nine primaries, though, it happens about nine times more often.

Let's consider what you have to handle just at the application level. DNS or a proxy can help hide that switch for a failover. But you will need to handle it for all nine shards. Every web server, worker, and cron job has its own connections open to the old primary. Each one has to reconnect to the new one. And until they do, customers on shard 3 get errors. That's lost revenue and upset customers. And if a human gets paged to handle this, you are down for a subset of your customers during the entirety of them getting paged, finding a laptop, diagnosing the issue, and pointing everything at the new primary. Most teams instead build a tool to handle this: an orchestrator that promotes the replica and tells every process where the shard lives now.

Even on quiet days, the operational challenges of running a single database are nine-fold in this case. Backups, restores, and Postgres upgrades all happen more often, and each can go wrong.

Another aspect of this that sometimes gets forgotten is the scale you have to apply to every layer of your stack. When you increase the number of app servers, workers, or even cron jobs, each one opens its own pool of connections to every shard. Adding shards doesn't reduce the load of connections on each shard, and every new server increases the number of connections to all of them. [Notion](https://www.notion.com/blog/the-great-re-shard) actually ran into this same problem. They were using PgBouncer with about 100 instances, each with up to six connections to every shard. When they resharded in 2023, they would have had to "either cut the number of downstream server connections by a similar factor… or increase our total connections per database by 3x," so they had to shard their pooler. Your store is probably going to go through the same headache. Not only will you need to add a connection pooler like PgBouncer in front of every shard, but you'll also be managing and sharding the poolers too.

## [You built a sharding layer anyway](https://planetscale.com/blog/why-you-shouldnt-do-app-level-sharding#you-built-a-sharding-layer-anyway)

We've finally hit day 1000. Your store is thriving. It's important to reflect on everything your team has built to maintain app-level sharding:

- Reshard tooling, plus a config service for `SHARDS` and `% 8`
- An ID scheme, a lookup table, `user_id` in every function, and code to merge results across shards
- An exceptions list for customers too big to share a shard
- A saga: an outbox, a pending state and an undo
- A schema rollout tool
- A failover orchestrator and a connection pooler on every shard

While this is all impressive, time and time again in the world of software, you should avoid doing work you don't need to. You ended up building a sharding layer, even though your team's actual job is running a marketplace.

## [Use a sharding layer instead](https://planetscale.com/blog/why-you-shouldnt-do-app-level-sharding#use-a-sharding-layer-instead)

This wasn't necessary. There are well-known systems like [Vitess](https://planetscale.com/vitess) that are battle-tested and trusted by the world's largest companies. They sit in between your app and the shards, and your application can speak to them like one database. [Neki](https://planetscale.com/neki), currently [in platform preview](https://planetscale.com/blog/introducing-neki), is horizontally sharded Postgres. It's built by the same team behind Vitess, horizontally sharded MySQL, who have taken their learnings and experiences to build an architecture so your team doesn't have to.

[Neki](https://planetscale.com/docs/neki) handles most of your issues from the past 1000 days. Resharding is built in as an automated workflow. The router buffers queries during the switch, so the cancel/shipment problem can't happen. The router also hands out unique IDs, so you won't get duplicate user IDs or order IDs. Global secondary indexes can map a column like an order ID or charge ID to its shard, so you can keep your application layer simple and not pass unnecessary parameters like `user_id` everywhere. Unlike the lookup table from earlier, Neki shards the index like any other table and keeps it in sync on every write, so it isn't one database that can take the whole store down. The slice of keys holding a hot seller can even be moved to its own shard through the same reshard workflow, to avoid the exceptions list. Schema changes occur with zero downtime, failover is automatic, and connection pooling is built into every shard.

You still have control over how your data is sharded and the compute size of every shard. One thing Neki doesn't do today is make writes across shards atomic. A purchase like user 14's is still your app's job, so pick a shard key that keeps most writes on one shard.

You also don't have to shard right away. Neki and Vitess can start as a single primary with replicas, allowing you to reshard when you hit that scale. Slack, Etsy, and Intercom all went down a similar path. They built their own sharding first and later turned to Vitess. [Slack's move](https://slack.engineering/scaling-datastores-at-slack-with-vitess/) took three years to reach 99% of its traffic, while [Etsy's](https://www.etsy.com/codeascraft/migrating-etsyas-database-sharding-to-vitess) took five. It's very difficult to migrate later on. Start on a sharding layer first. Shard when you need to. It's fewer headaches.

## [FAQs](https://planetscale.com/blog/why-you-shouldnt-do-app-level-sharding#faqs)

### [What is application-level sharding?](https://planetscale.com/blog/why-you-shouldnt-do-app-level-sharding#what-is-application-level-sharding)

Application-level sharding is when your application code decides which shard holds each row. It's also called app-level, client-side or application-side sharding. It usually starts as one function, like `shard_for(user_id)`, that picks a database connection based on the shard key.

### [Why shouldn't you do application-level sharding?](https://planetscale.com/blog/why-you-shouldnt-do-app-level-sharding#why-shouldnt-you-do-application-level-sharding)

Because routing is the easy part. Once your app decides where each row lives, every problem that comes from spreading data across servers becomes your team's to solve:

- Adding shards changes where rows belong, so you need a way to move them without losing writes.
- Each shard's ID counter runs on its own, so two shards can hand out the same ID.
- A support ticket with only an order number has no shard key to route on.
- One big customer can overload its shard and slow down everyone else on it.
- A write that touches two shards can commit on one and fail on the other.
- Every schema change runs on every shard, and it can fail on some of them.
- When a shard fails over, every process has to find the new primary.
- Every new app server opens connections to every shard.

Together, these [make up a sharding layer](https://planetscale.com/blog/why-you-shouldnt-do-app-level-sharding#you-built-a-sharding-layer-anyway) your team builds and maintains itself. Slack, Etsy and Intercom all started with their own sharding and turned to Vitess years later.

### [What's the alternative to application-level sharding?](https://planetscale.com/blog/why-you-shouldnt-do-app-level-sharding#whats-the-alternative-to-application-level-sharding)

A sharding layer, sometimes called a sharding proxy or sharding middleware. It sits between your app and the shards, so your app connects to it as if it were one database, and it handles routing, resharding, failover and connection pooling. Your team still picks the shard key. For Postgres that's [Neki](https://planetscale.com/neki), and for MySQL, [Vitess](https://planetscale.com/vitess).

### [Do you need to shard from day one?](https://planetscale.com/blog/why-you-shouldnt-do-app-level-sharding#do-you-need-to-shard-from-day-one)

Not on day one, but probably sooner than you think. Sharding is usually treated as a fix for write throughput, but a large database gets hard to operate well before writes run out: backups and restores take hours, schema changes on big tables run for days, and replicas fall behind under heavy writes. Smaller shards make each of those faster. With application-level sharding, every shard you add is more work for your team. With [Neki](https://planetscale.com/neki), you can start on a single Postgres primary with replicas and reshard as soon as operations start to hurt, as a workflow on the cluster you already have.

### [Why is resharding hard with application-level sharding?](https://planetscale.com/blog/why-you-shouldnt-do-app-level-sharding#why-is-resharding-hard-with-application-level-sharding)

Because changing the number of shards changes where existing rows belong, and those rows have to move while the app keeps writing. Moving them with minimal downtime means copying the rows, keeping new writes in sync until the switch, checking the copies match and then switching traffic. The switch is the risky part. Every process has its own copy of the routing code and deploys at a different moment, so for a few minutes some send a user to the old shard and others to the new one, and [writes can be lost](https://planetscale.com/blog/why-you-shouldnt-do-app-level-sharding#resharding-from-four-shards-to-eight).

### [How do you query a sharded database without the shard key?](https://planetscale.com/blog/why-you-shouldnt-do-app-level-sharding#how-do-you-query-a-sharded-database-without-the-shard-key)

The query either goes to every shard or finds its shard some other way. Asking every shard costs one query per shard and waits on the slowest. Passing the shard key through every function works for code you control, but not for IDs that come from outside it, like a payment provider's charge ID. A [lookup table](https://planetscale.com/blog/why-you-shouldnt-do-app-level-sharding#keep-a-lookup-table) from ID to shard has to stay in sync, and if every query checks it first, it becomes a single point of failure. Putting a logical shard number in the ID removes the lookup for IDs you generate. A sharding layer can keep a global index for this and do the fan-out and merge for you.

### [How do you generate unique IDs across shards?](https://planetscale.com/blog/why-you-shouldnt-do-app-level-sharding#how-do-you-generate-unique-ids-across-shards)

Not with auto-increment counters, because each shard counts on its own and two shards will hand out the same ID. UUIDs don't collide, but they don't say which shard holds the row, so looking one up still means asking every shard or keeping a lookup table. IDs that embed a logical shard number, like [Instagram's](https://planetscale.com/blog/why-you-shouldnt-do-app-level-sharding#put-the-shard-in-the-id), solve both problems. A sharding layer like Neki hands out unique IDs from a sequence and can find their shard with a global index.

### [How do cross-shard transactions work?](https://planetscale.com/blog/why-you-shouldnt-do-app-level-sharding#how-do-cross-shard-transactions-work)

Usually with a saga or two-phase commit, because separate databases don't share a `COMMIT` and one shard can commit while the other fails. Two-phase commit uses a coordinator that asks every shard to prepare and only commits once all of them agree, but the shards hold their locks until it decides. A saga splits the write into local transactions that each commit on their own. The first one also records the second in an outbox table in the same transaction, and a worker retries it until it commits, so it has to be safe to run twice. If the second step hits an error retrying won't fix, a compensating transaction undoes the first. Until one of those happens, the app shows the write as pending. Choosing a shard key where most writes stay on one shard reduces the problem.
