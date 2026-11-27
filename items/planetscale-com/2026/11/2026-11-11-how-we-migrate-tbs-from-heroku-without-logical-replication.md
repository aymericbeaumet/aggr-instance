---
title: How we migrate TBs from Heroku without logical replication
link: https://planetscale.com/blog/migrate-from-heroku-to-planetscale
source: planetscale-com
published: 2026-11-11T00:00:00Z
updated: 2026-11-11T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Dmitrii Anisovich
labels:
- engineering
- postgres
summary: We've equipped our customers with an open source tool that not only quickly transfers your data but also provides a replication-based experience, so that any user can migrate from Heroku without extended downtime or the need to purchase a third-party product.
content: extracted
html: 2026-11-11-how-we-migrate-tbs-from-heroku-without-logical-replication.html
remote_preview:
  url: https://planetscale.com/assets/migrate-from-heroku-to-planetscale-social-DTB0sjs0.png
---

Since we launched PlanetScale Postgres, companies have been moving to it from just about every Postgres provider out there, and Heroku Postgres is no exception. Our team has helped many customers leave Heroku, ranging from small databases on the order of hundreds of gigabytes to others with many terabytes of data.

Migrations from Heroku present an extra challenge: its standard plans offer no logical replication. This leaves a lot of Heroku customers stuck. They want to move away from Heroku, but a typical offline database migration requires several hours of downtime, which their mission-critical applications cannot tolerate, and neither can we.

So we took a different route. We've equipped our customers with an open source tool that not only quickly transfers your data but also provides a replication-based experience, so that any user can migrate from Heroku without extended downtime or the need to purchase a third-party product. That's how our Heroku Migrator was born.

This post covers how it works, the problems that we encountered on large migrations, and how v2.0 of the migrator solves these: more reliable, production-tested and faster than ever before.

## [Why leaving Heroku is unique](https://planetscale.com/blog/migrate-from-heroku-to-planetscale#why-leaving-heroku-is-unique)

Most online Postgres migrations rely on logical replication. You take a consistent snapshot, copy data, then stream changes from the source's WAL until the target catches up, and do a clean, seconds-scale cutover. Tools like [pgcopydb](https://github.com/planetscale/pgcopydb) are built based on this model.

Unfortunately, Heroku's standard tiers don't give you WAL access or logical replication. Without them, a slot-based migration is off the table. Logical replication is available on Heroku Postgres Advanced since October 2026, but that doesn't help the databases already running on the standard tiers. To use it, you'd first have to move to the Advanced tier, which means yet another migration!

For everyone on a standard plan, there's really only one good online migration option: trigger-based replication. Triggers on every table record changes and a distinct process replays those changes on the target. The problem is that it tends to be more fragile than WAL-based replication. We are set on solving this pain for our customers and have built and continue to iterate on tooling to make this process effortless.

## [Before you migrate: the discovery tool and Liftoff](https://planetscale.com/blog/migrate-from-heroku-to-planetscale#before-you-migrate-the-discovery-tool-and-liftoff)

Before you begin, you need to do your homework, and that starts with a discovery phase. We help customers move their data every day, and that experience led us to build tooling that spots problems early.

The PlanetScale [discovery tool](https://planetscale.com/docs/postgres/imports/discovery-tool) (`ps-discovery`) is an open-source CLI you run against a source database. It has two components:

- The database component connects with a dedicated read-only user and collects statistics about your setup. Version, configuration, extensions, schemas, tables, indexes and sizes. It doesn't read row data, so your security team can be at peace.
- The cloud component takes a Heroku API key and lists your Heroku Postgres add-ons with their plan, size, replicas and connection pooling.

After setting up a configuration file with your database info, run:

```bash
./ps-discovery --config config.yaml
```

The results go into a JSON output file, for example `planetscale_discovery_results_<timestamp>.json`.

The next step is to plug this information into PlanetScale's [Liftoff](https://planetscale.com/liftoff) tool. You upload it and get a migration assessment back. The analysis runs entirely in your browser.

Liftoff shows extension compatibility issues, schema defects and blockers, and a recommended migration path with steps and an estimated duration. For a Heroku source it recommends our internal tool, the Heroku Migrator. It also flags tables without a primary key or unique index, since trigger-based replication can't migrate these.

In essence, discovery collects the facts and Liftoff turns them into action items:

- Which extensions to enable on PlanetScale before you start
- Which tables need a primary key first
- What indexes can be removed before migration because they're unused or duplicates
- How to size the cluster: match the CPU and RAM of your Heroku plan, and provision enough storage
- Where to run the migrator: on a Heroku dyno, or on EC2 if the copy could run close to a day (Heroku restarts every dyno at least once every 24 hours)

## [The Heroku Migrator](https://planetscale.com/blog/migrate-from-heroku-to-planetscale#the-heroku-migrator)

The [Heroku Migrator](https://github.com/planetscale/heroku-migrator) is a temporary app you deploy for the duration of a migration. Small and medium databases can run it on Heroku with the Deploy button. Larger ones (starting from 500 GB) should run it on a high-performance EC2 instance, because the last thing we want is for our migration tool to become a bottleneck. You can drive the migration from the web dashboard, or you can have your agent handle this for you if clicking buttons isn't your thing.

The migrator copies the schema into PlanetScale based on the results of a `pg_dump --schema-only` against Heroku, skipping Heroku-specific and extension-owned schemas. You have to **freeze schema changes during the migration.** Making changes will break the migration.

Under the hood, the migrator runs [Bucardo](https://bucardo.org/Bucardo/), an open-source trigger-based replication tool. First, the migrator installs a `bucardo_delta` trigger on every source table. From that moment on, every insert, update and delete is recorded in per-table delta and track tables. Next, a one-time `COPY` loads the existing data into PlanetScale. This part can take hours on a large database. Once the copy finishes, a Bucardo worker process (Bucardo calls it a `KID`) reads the recorded deltas and applies them to PlanetScale. It first works through the backlog that built up during the copy, then continuously applies new changes, so the target stays up to date.

We use triggers because they're the only change-capture mechanism available on a standard Heroku plan. With this technique, every replicated table needs a primary key or a unique index. Otherwise, Bucardo can't identify which row to update on the target.

In addition, the tool incorporates a verification step. You can compare Heroku and PlanetScale side by side and check:

- That every table exists on both sides with the same columns
- That indexes and constraints match
- That sequences are in place, so new IDs won't collide
- That extensions are at the same versions
- That row counts match on a sample of tables
- That no replicated changes are still waiting to be applied

Put together, the migrator copies the schema and the data, keeps source and target in sync, and gives you a controlled switchover with a verification step at the end.

## [The migration path, step by step](https://planetscale.com/blog/migrate-from-heroku-to-planetscale#the-migration-path-step-by-step)

A typical Heroku migration goes through 8 steps:

1. **Run the discovery tool and review the result in Liftoff.**
2. **Deploy the migrator.** On a Heroku dyno (Performance-L for anything over 100 GB), or run the same container on an EC2 instance or VM instead.
3. **Start Migration.** The migrator copies your schema to PlanetScale and sets up replication. This usually takes a minute or two, then it waits for you before continuing. From this point until the migration is complete, don't run any DDL on Heroku, including your framework's auto-migrations. If you use pg\_partman, pause its maintenance job too.
4. **Click Start Data Copy, then wait.** This ranges from minutes to hours depending on data size. You can close the browser and come back here no problem. When the table data copy is finished, the migrator automatically rebuilds the indexes and shows you the progress. Bucardo applies all the changes your app wrote to Heroku during the copy and the rebuild. From then on it applies new writes as they happen.
5. **Switch Traffic.** Once you switch, writes to Heroku will fail, while reads still work. For a production app, turn on Heroku maintenance mode first.
6. **Click Verify Migration.** If it reports a backlog of pending changes, wait a minute and run it again.
7. **Point your app at PlanetScale.** Set `DATABASE_URL` to the PlanetScale connection string and test your app. If something is wrong, the Revert Switch makes Heroku writable again.
8. **Click Complete Migration and delete the migrator app.** This removes the triggers from Heroku. Remove the Heroku Postgres add-on once you're confident.

## [Three problems, three solutions](https://planetscale.com/blog/migrate-from-heroku-to-planetscale#three-problems-three-solutions)

Historically, Heroku migrations were some of the most challenging due to these limitations. However, most of them are solvable with careful engineering work in the migrator tool.

The three issues described below are different from one another, but they have one thing in common: they all happened when moving big databases! We've taken learnings from each and applied them as fixes to our Heroku migrator, passing along the learnings for our future customers' benefit.

### [Copy restarting from zero](https://planetscale.com/blog/migrate-from-heroku-to-planetscale#copy-restarting-from-zero)

Here, the symptom is simple and painful. After hours of data copying, the migration encounters an unexpected failure near the end. In this case, Bucardo needs to re-start copying again from the first table.

One example came from a single row in the source database: a large text value in a column with a B-tree index. The index existed on the target database during the copy process (because the entire schema was applied first), but the entry for that value exceeded the B-tree size limit there. When the `COPY` process reached that row, Postgres refused to insert it.

```text
DBD::Pg::db pg_putcopyend failed: ERROR: index row size 3328 exceeds btree version 4 maximum 2704
for index "index_<table>_on_<col_a>_and_<col_b>"
CONTEXT: COPY <table>, line 3732862 ... DB planetscale state: 54000 Error: 7
```

We were already 12 hours into the data copy at this point, and resolving it required fixing the index and restarting from the beginning. The reason is how Bucardo does the initial copy. The migrator creates the sync with `onetimecopy=1`. Bucardo then works through the tables one at a time: it empties each target table and fills it with a `COPY` from the source. All of this happens in one transaction on the target, committed once at the very end. Nothing records which tables are already done, so after any interruption the whole transaction rolls back and the next run empties every table and starts over. We've seen this in other situations as well:

- Heroku's 24-hour dyno restart
- The migrator running out of memory
- Pausing and resuming during the copy
- PlanetScale's database-level `transaction_timeout`. Bucardo runs the initial copy as a single transaction, so any copy longer than the timeout (30 minutes on the databases we migrated into) was killed.

The issue was further compounded by the data copy rate. With all the indexes in place on the target, the migration rate fluctuates between 50 and 100 GB/h. At that rate, a multi-TB copy runs for days.

As a solution, we decided to split this stage into two phases: transferring the data and rebuilding the indexes.

Before the copy, the migrator drops every secondary and unique index on the target database (PlanetScale). Primary keys are necessary, because Bucardo needs them to apply changes later. The dropped index definitions are saved from `pg_get_indexdef` into a `_ps_migrator.dropped_indexes` table on the target, together with their build status.

After the copy, the migrator pauses replication and rebuilds the indexes on the target database with parallel workers. Each worker runs with `maintenance_work_mem = 1GB` and 2 parallel maintenance workers. Foreign keys that depended on a dropped unique index come back as `NOT VALID` and are validated afterwards. Only when every index is rebuilt does Bucardo resume and catch up on the changes that queued on Heroku in the meantime.

For the migration above, the copy went from about 60 GB/h to nearly 300 GB/h, the rebuild took about 2 hours, and the whole process ran roughly 2.5 times faster. Now, a bad index like the one above fails in the rebuild step, where you can retry it from the dashboard instead of killing the copy.

On top of that, we now run Bucardo's connections with `transaction_timeout=0`, so long copies are no longer cut off.

### [Silent network drops](https://planetscale.com/blog/migrate-from-heroku-to-planetscale#silent-network-drops)

This was the most confusing problem we hit, because initially nothing appeared broken. About 30 hours into copying a huge table, Bucardo was gone. The controller processes and the worker doing the copy were all dead.

On another similar migration, the copy went fine and replication started. Then, during the quiet stretches between batches of changes, the replication worker stopped applying said changes. It sat waiting for a reply on a connection that looked open from its side, while changes on Heroku piled up into the millions.

Two migrations with two different symptoms, but turns out, the same cause.

Both migrations relied on a single connection had to stay alive for many hours, but unknowlingly, something on the migrator host's network path dropped long-lived connections without warning.

During the copy, the connection was busy streaming rows, so the drop surfaced as a connection error, the worker died, and the rest of Bucardo also went down. libpq left TCP keepalives at the Linux defaults, which send the first probe only after two hours of silence and give up on the peer about 11 minutes later. So the worker kept waiting on a socket that was already dead.

The migrator now sets keepalives on Bucardo's connections, so a dropped connection turns into an error in about a minute instead of a hang:

```bash
bucardo set tcp_keepalives_idle=60
bucardo set tcp_keepalives_interval=10
bucardo set tcp_keepalives_count=6
```

For the very long `COPY`, widening the TCP buffers on the EC2 host also helped. These are host settings we borrowed from a [thread on the Bucardo mailing list](https://bucardo.org/pipermail/bucardo-general/2019-September/003148.html):

```bash
sudo sysctl -w net.core.rmem_max=134217728
sudo sysctl -w net.core.wmem_max=134217728
sudo sysctl -w net.ipv4.tcp_rmem="4096 87380 134217728"
sudo sysctl -w net.ipv4.tcp_wmem="4096 65536 134217728"
```

### [The delta\_check stall](https://planetscale.com/blog/migrate-from-heroku-to-planetscale#the-delta_check-stall)

This one surfaces after the initial data copy. After completing, we observed the migrator stall completely. The Bucardo log shows:

```text
KID (planetscale_import) Checking truncate_trigger table on database "heroku"
```

`bucardo status` shows the sync stuck in the same state for hours:

```text
 planetscale_import | delta_check on db heroku | 02:50:10 | 4h 3m 5s | 339623/4580806 | none |
```

Meanwhile, the backlog of unreplicated changes on the source keeps growing while Bucardo is stuck. Killing the stuck query showed us exactly where Bucardo was spending its time:

```text
Kid has died, error is: ... terminating connection due to administrator command
CONTEXT: SQL statement "SELECT 1 FROM bucardo.delta_public_<table> d
  WHERE NOT EXISTS (SELECT 1 FROM bucardo.track_public_<table> t
  WHERE d.txntime = t.txntime AND (t.target = '...' OR t.target ~ '^T:')) LIMIT 1"
PL/pgSQL function bucardo.bucardo_delta_check(text,text)
```

At the start of every sync cycle, Bucardo runs `bucardo_delta_check()` to ask "is there anything left to replicate?". The function does that with an anti-join between each delta table and its track table on `txntime`. But Bucardo indexes the track tables on `(target, txntime)`, and a SQL predicate on only `txntime` can't use that index. On a small database this isn't noticeable, but on a large migration, the delta and track tables grow for hours during the initial copy, and by the end the check scans the track table for every delta row.

The fix is a plain B-tree index on `txntime` for every track table. The migrator now builds it during setup, right after Bucardo creates the track tables and before the sync starts. At that point the tables are empty, so each index builds instantly and takes no locks your application would notice.

This is how it looks for each track table:

```sql
CREATE INDEX IF NOT EXISTS dex4_public_<table>
  ON bucardo.track_public_<table> (txntime);
```

## [All of this is in the latest Heroku Migrator](https://planetscale.com/blog/migrate-from-heroku-to-planetscale#all-of-this-is-in-the-latest-heroku-migrator)

Everything above is in [the latest version of the Heroku Migrator on GitHub](https://github.com/planetscale/heroku-migrator). We also recently added support for Postgres 18 as the source, and for the pg\_partman extension.

Heroku, unfortunately, attempts to keep you "locked in" to their database on the standard plan by not offering logical replication. No customer likes vendor lock-in. However, we have the tools and expertise to get you off Heroku Postgres and onto a faster and more reliable Postgres platform.

If you're planning a Heroku migration, try our tooling out today. Start with the [discovery tool](https://planetscale.com/docs/postgres/imports/discovery-tool) and drop the result into [Liftoff](https://planetscale.com/liftoff).

If you need help, [let us know](https://planetscale.com/contact), [hop in our Discord](https://pscale.link/community), or open an issue [on the repo](https://github.com/planetscale/heroku-migrator).
