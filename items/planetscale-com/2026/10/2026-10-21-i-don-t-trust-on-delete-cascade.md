---
title: I don't trust ON DELETE CASCADE
link: https://planetscale.com/blog/i-dont-like-delete-cascade
source: planetscale-com
published: 2026-10-21T00:00:00Z
updated: 2026-10-21T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Tom Pang
labels:
- engineering
- postgres
summary: A one-line convenience in your database schema can become an outage. There are better ways to delete data that won't risk locking up your database.
content: extracted
html: 2026-10-21-i-don-t-trust-on-delete-cascade.html
remote_preview:
  url: https://planetscale.com/assets/i-dont-like-delete-cascade-social-CKmBvjQD.png
---

Postgres `ON DELETE CASCADE` is very tempting to use at first. Fully integrity checked, atomic deletion of your data. You don't need to write any application code to clean up related rows, you simply set up your foreign keys, then issue a single `DELETE` statement, Postgres takes care of the rest.

What could be simpler?

```sql
CREATE TABLE customer_organizations (
    id SERIAL PRIMARY KEY,
    name text NOT NULL
);
CREATE TABLE customer_users (
    id SERIAL PRIMARY KEY,
    name text NOT NULL,
    organization_id INT NOT NULL,
    FOREIGN KEY(organization_id)
       REFERENCES customer_organizations(id)
       ON DELETE CASCADE
);
```

## [How cascades grow](https://planetscale.com/blog/i-dont-like-delete-cascade#how-cascades-grow)

Over time, your application grows, you add more tables, all doing a `ON DELETE CASCADE` pointing at `customer_organizations`. To keep the simplicity of `ON DELETE CASCADE`, you even have this happening recursively - tables that point at tables that point at tables, all doing `ON DELETE CASCADE`. Your application doesn't need to think about the relationship tree when deleting, it just picks the record to delete (an organization, a user etc etc) and Postgres takes care of things.

## [Cascade plus migration equals outage](https://planetscale.com/blog/i-dont-like-delete-cascade#cascade-plus-migration-equals-outage)

Then one day, you get paged. Your production database is not responding properly, nearly all writes to most of your tables are timing out, and your customers are complaining. The cause: very large `ON DELETE CASCADE` interacting with a simple, innocent schema change adding a column to one of your tables. You fire up the [PlanetScale live connections CLI](https://planetscale.com/docs/cli/connections) and cancel the offending transaction, restoring service, outage over.

Let's dig in to what happened:

When Postgres processes a delete that causes a cascading delete like this, it has to issue a `DELETE` for matching rows, generating dead tuples, index updates and WAL, all in one transaction. While doing that, it holds `RowExclusiveLock` on every table involved. Your simple migration (e.g. adding a nullable text column to one of the tables involved) requests `AccessExclusiveLock`. The `AccessExclusiveLock` then blocks subsequent `INSERT` behind it.

*The `ALTER TABLE` can't start until the cascading `DELETE` finishes, so every new query on `customer_users` piles up behind it.*

You can work around this by setting a short `lock_timeout` on your migrations, and retrying them - instead of breaking things, your migration gets retried until there's a period where it can work.

But that `DELETE CASCADE` causes other problems. In larger environments we've seen hundreds of gigabytes of WAL written by a single transaction, leaving other transactions waiting behind synchronous replication.

We [previously wrote](https://planetscale.com/blog/the-only-scalable-delete) about large scale deletes and other patterns for avoiding them. But `DELETE CASCADE` isn't as easy to move to a `DROP TABLE` based approach.

## [Unindexed foreign keys](https://planetscale.com/blog/i-dont-like-delete-cascade#unindexed-foreign-keys)

There's an even worse failure mode that can happen with `ON DELETE CASCADE` - unindexed foreign keys. Typically in Postgres, you index your foreign keys, so lookups are fast. This also means `ON DELETE CASCADE` can use those indexes to find relevant rows. But, applications can sometimes miss this, leading to full table seq scans during single row deletes, a very confusing performance situation, and a bad one.

## [Cascades and logical replication](https://planetscale.com/blog/i-dont-like-delete-cascade#cascades-and-logical-replication)

Built-in Postgres logical replication (a publisher streaming row changes to subscribers) adds another surprise.

On the publisher, deleting org 42 shows up as one transaction of ordinary row deletes: org, then users, then grandchildren. On the subscriber, the apply worker (the process replaying those changes) runs with `session_replication_role = replica`, which by default skips the internal triggers that check foreign keys and run their cascades. Replicated changes get neither checks nor cascades unless you enable those triggers for replica sessions.

That's deliberate: the publisher already sent the child deletes. Any child row whose delete isn't replicated is left orphaned once its parent disappears, still pointing at a row that's gone.

Give a subscriber its own table:

```sql
-- only on the subscriber
CREATE TABLE user_activity_rollups (
    user_id INT PRIMARY KEY REFERENCES customer_users(id) ON DELETE CASCADE,
    events_last_30d bigint
);
```

Delete org 42 on the publisher: org 42's users vanish from the subscriber, but their rollup rows don't. No error, no warning, only a violated foreign key. A hand-picked table list that skips a child does the same.

The publisher has a trap too. A table needs a usable replica identity (how subscribers find the row, by default its primary key) to publish deletes.

A publication selects which tables and changes to replicate. Put `user_sessions(user_id, token)`, a cascade child of `customer_users` with no primary key and the default replica identity, in one that includes deletes. Now deleting an org with users fails, even when those users have no sessions:

```text
DELETE FROM customer_organizations WHERE id = 43;
ERROR:  cannot delete from table "user_sessions" because it does not have a replica identity and publishes deletes
HINT:  To enable deleting from the table, set REPLICA IDENTITY using ALTER TABLE.
```

If you cascade and replicate, publish and subscribe to the whole foreign key tree on the publisher: `FOR ALL TABLES` publishes every table in the database, and `FOR TABLES IN SCHEMA` (Postgres 15+) publishes whole schemas, so list every relevant one. Subscriber-only children need separate cleanup. An orphan check tells you what slipped through.

Or shrink the problem with a soft delete: marking the org deleted replicates as a one-row `UPDATE`, and the real deletes come later, in batches you control.

## [Soft deletes are the better alternative](https://planetscale.com/blog/i-dont-like-delete-cascade#soft-deletes-are-the-better-alternative)

A competing (and I'd argue much better in many ways) approach for doing customer data deletion, especially at scale is soft deletes. Instead of having to synchronously delete millions of rows in one transaction (and replicate every one of them you publish), you update a single row:

```sql
UPDATE customer_organizations SET deleted_at = now() WHERE id = 42;
```

This requires more up-front work - in your application code, reads (even to child tables) have to check `deleted_at` on relevant rows. Soft deletes can also help with restoring customer data after accidental deletion. Then later on, you also will want code that runs real `DELETE` on rows impacted (potentially after a timeout, e.g. keep deleted rows for 30 days, or more depending on business requirements). With soft deletes, this background "reaping" of dead rows can happen more slowly, at times of low customer traffic, and carefully batch to ensure transactions remain fast. One neat trick you can also do is vacuum after batched deletes finish, marking the space as reusable.

*Each batched `DELETE` finishes quickly, so the `ALTER TABLE` and the queries behind it only wait for a moment before everything flows again.*

The result is a design that scales to very large customers, keeping logical deletes fast, bounding transaction size and giving you more flexibility.

## [Keep transactions short and bounded](https://planetscale.com/blog/i-dont-like-delete-cascade#keep-transactions-short-and-bounded)

Ultimately this is a subset of general high-scale OLTP database design: keep your transactions short, small and bounded in what they do. Single transactions should touch small numbers of rows (hundreds to low thousands is a great starting point, and often far fewer, but tune for your workloads).
