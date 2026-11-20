---
title: PostgreSQL 19 Released
link: https://www.postgresql.org/about/news/postgresql-19-released-3449/
source: lobste-rs
published: 2026-11-19T15:12:08Z
updated: 2026-11-19T15:12:08Z
first_seen: 2026-11-20T06:59:33.283911750Z
authors:
- postgresql.org via bio
labels:
- databases
- release
summary: Comments
content: extracted
html: 2026-11-19-postgresql-19-released.html
remote_preview:
  url: https://www.postgresql.org/media/img/about/press/elephant.png
  alt: PostgreSQL Elephant Logo
---

Posted on **2026-11-19** by PostgreSQL Global Development Group

 PostgreSQL Project

November 19, 2026 - The [PostgreSQL Global Development Group](https://www.postgresql.org) today announced the release of [PostgreSQL 19](https://www.postgresql.org/docs/19/release-19.html), the latest version of the world's most advanced open source database.

Mission-critical usage of PostgreSQL continues to rapidly rise, and PostgreSQL 19 responds to this by simplifying both administration and development for these workloads at scale. This release introduces the [`REPACK (CONCURRENTLY)`](https://www.postgresql.org/docs/19/sql-repack.html) command so users can optimize tables for their current workload without disruption to ongoing operations. PostgreSQL 19 gives users better control over plan stability and query performance with the new [`pg_plan_advice`](https://www.postgresql.org/docs/19/pgplanadvice.html) extension. Developers will find many delightful features in this release, from support for "read-your-writes" consistency on standbys to "get or create" behavior that saves a network round trip.

"For over 30 years, the global open source community has built PostgreSQL into a database that organizations trust with their most critical data," said Jonathan Katz, a member of the PostgreSQL core team. "PostgreSQL 19 continues that commitment to reliability, giving users new ways to manage their workloads without disruption and to build applications that span multiple systems."

PostgreSQL, an innovative data management system known for its reliability, robustness, and extensibility, benefits from 30 years of open source development from a global developer community and has become the preferred open source relational database for organizations of all sizes.

### Simplifying workload management

Tables that receive many updates and deletes can build up unused space, known as "bloat," which can also significantly impact query performance. The `VACUUM FULL` and `CLUSTER` commands were previously used to handle this, but had tradeoffs, including preventing access to the table until the cleanup finished. PostgreSQL 19 introduces the [`REPACK`](https://www.postgresql.org/docs/19/sql-repack.html) command, which reclaims this space and can reorder a table's data to match an index, and with `REPACK (CONCURRENTLY)`, can do so without blocking reads or writes. `REPACK` replaces the `VACUUM FULL` and `CLUSTER` commands, both of which remain available for compatibility.

The PostgreSQL query planner usually picks an efficient plan for each query, but the plan can change as your data changes, and sometimes the new plan is slower. PostgreSQL 19 adds the [`pg_plan_advice`](https://www.postgresql.org/docs/19/pgplanadvice.html) extension, which allows stabilization of plan choices and the ability to experiment with different plans using the "plan advice" mini-language. The companion [`pg_stash_advice`](https://www.postgresql.org/docs/19/pgstashadvice.html) extension stores advice and automatically applies it to queries based on their query identifier, so you can fix a slow plan without changing your application.

PostgreSQL 19 also helps [autovacuum](https://www.postgresql.org/docs/19/routine-vacuuming.html#AUTOVACUUM) keep up on busy systems. Autovacuum can now vacuum indexes using parallel workers, controlled by the new [`autovacuum_max_parallel_workers`](https://www.postgresql.org/docs/19/runtime-config-vacuum.html#GUC-AUTOVACUUM-MAX-PARALLEL-WORKERS) setting, and uses a new [scoring system](https://www.postgresql.org/docs/19/routine-vacuuming.html#AUTOVACUUM-PRIORITY) to decide which tables to process first. Queries that read a table can now automatically mark pages as all-visible in the [visibility map](https://www.postgresql.org/docs/19/routine-vacuuming.html#VACUUM-FOR-VISIBILITY-MAP) during execution, reducing the work that future vacuums need to do. Additionally, [`vacuumdb --analyze-only`](https://www.postgresql.org/docs/19/app-vacuumdb.html) now analyzes partitioned tables when no tables are specified.

This release adds new views for monitoring workloads, including [`pg_stat_lock`](https://www.postgresql.org/docs/19/monitoring-stats.html#PG-STAT-LOCK-VIEW) for per-lock-type statistics, [`pg_stat_recovery`](https://www.postgresql.org/docs/19/monitoring-stats.html#PG-STAT-RECOVERY-VIEW) for recovery status, and [`pg_stat_autovacuum_scores`](https://www.postgresql.org/docs/19/monitoring-stats.html#PG-STAT-AUTOVACUUM-SCORES-VIEW) to show how autovacuum prioritizes tables. The new `IO` option for [`EXPLAIN ANALYZE`](https://www.postgresql.org/docs/19/sql-explain.html) and the matching [`auto_explain`](https://www.postgresql.org/docs/19/auto-explain.html#AUTO-EXPLAIN-CONFIGURATION-PARAMETERS-LOG-IO) setting report asynchronous I/O activity. PostgreSQL 19 also reports the amount of WAL written for full-page images in [`pg_stat_wal`](https://www.postgresql.org/docs/19/monitoring-stats.html#PG-STAT-WAL-VIEW), `EXPLAIN (ANALYZE, WAL)`, and in vacuum and analyze log events.

### Distributed data improvements

Previously, PostgreSQL often required users to restart their server to enable [logical replication](https://www.postgresql.org/docs/19/logical-replication.html), which could be disruptive in production workloads. PostgreSQL 19 simplifies logical replication initialization by automatically raising the effective WAL level to `logical` when you create the first logical replication slot. You can see the WAL level in use with the new [`effective_wal_level`](https://www.postgresql.org/docs/19/runtime-config-preset.html#GUC-EFFECTIVE-WAL-LEVEL) setting.

PostgreSQL 19 also adds logical replication of [sequence values](https://www.postgresql.org/docs/19/logical-replication-sequences.html), which you can publish using `FOR ALL SEQUENCES` with [`CREATE PUBLICATION`](https://www.postgresql.org/docs/19/sql-createpublication.html). `CREATE PUBLICATION` also adds an `EXCEPT` clause to exclude tables when publishing `FOR ALL TABLES`, and [`CREATE SUBSCRIPTION`](https://www.postgresql.org/docs/19/sql-createsubscription.html) can now get its connection settings from a foreign server, such as one defined with [`postgres_fdw`](https://www.postgresql.org/docs/19/postgres-fdw.html), using the `SERVER` clause. Additionally, the new [`retain_dead_tuples`](https://www.postgresql.org/docs/19/sql-createsubscription.html#SQL-CREATESUBSCRIPTION-PARAMS-WITH-RETAIN-DEAD-TUPLES) subscription option lets PostgreSQL detect updates that conflict with a concurrent delete, and these conflicts are counted in [`pg_stat_subscription_stats`](https://www.postgresql.org/docs/19/monitoring-stats.html#MONITORING-PG-STAT-SUBSCRIPTION-STATS).

For applications that send reads to standbys, PostgreSQL 19 adds "read-your-writes" consistency with the new [`WAIT FOR LSN`](https://www.postgresql.org/docs/19/sql-wait.html) command. A session on a standby can use `WAIT FOR LSN` to wait until the standby has replayed the write-ahead log (WAL) up to a specific [log sequence number (LSN)](https://www.postgresql.org/docs/19/wal-internals.html) before running a query, so the application can read data it just wrote on the primary.

PostgreSQL 19 also improves queries on foreign tables. With the new [`import_stats`](https://www.postgresql.org/docs/19/postgres-fdw.html#POSTGRES-FDW-OPTIONS-COST-ESTIMATION) option, [`postgres_fdw`](https://www.postgresql.org/docs/19/postgres-fdw.html) can import table statistics from the remote server when you run `ANALYZE`, instead of sampling rows from the remote table, which helps the query planner choose better plans.

### Developer experience and performance enhancements

PostgreSQL 19 adds [`INSERT ... ON CONFLICT DO SELECT`](https://www.postgresql.org/docs/19/sql-insert.html#SQL-ON-CONFLICT), which lets the `RETURNING` clause return existing rows that conflict with an insert, simplifying the common "get or create" pattern. [`COPY`](https://www.postgresql.org/docs/19/sql-copy.html) can now output data as JSON, including as a single JSON array, and the [SQL/JSON path language](https://www.postgresql.org/docs/19/functions-json.html#FUNCTIONS-SQLJSON-PATH-OPERATORS) adds string methods such as `lower()`, `upper()`, `replace()`, `split_part()`, and `btrim()`. Additionally, the `lead()`, `lag()`, `first_value()`, `last_value()`, and `nth_value()` [window functions](https://www.postgresql.org/docs/19/functions-window.html) can now skip `NULL` values using `IGNORE NULLS`. The [`random()`](https://www.postgresql.org/docs/19/functions-datetime.html#FUNCTIONS-DATETIME-TABLE) function can now also generate random dates and timestamps within a range.

This release also makes many workloads faster without changes to your application. PostgreSQL 19 improves the performance of foreign key checks, which in testing made bulk inserts nearly 2x faster, and adds more query planner optimizations, such as converting `NOT IN` clauses to more efficient anti-joins when `NULL` values are not present, and performing [eager aggregation](https://www.postgresql.org/docs/19/runtime-config-query.html#GUC-ENABLE-EAGER-AGGREGATE) before joins to reduce the number of rows to process. PostgreSQL 19 also builds on the [asynchronous I/O](https://www.postgresql.org/docs/19/runtime-config-resource.html#GUC-IO-METHOD) subsystem added in PostgreSQL 18. When `io_method` is set to `worker` (the default), PostgreSQL now automatically adjusts the number of I/O workers between the new [`io_min_workers`](https://www.postgresql.org/docs/19/runtime-config-resource.html#GUC-IO-MIN-WORKERS) and [`io_max_workers`](https://www.postgresql.org/docs/19/runtime-config-resource.html#GUC-IO-MAX-WORKERS) settings, which replace the `io_workers` setting. There are also scalability improvements when using [`LISTEN`](https://www.postgresql.org/docs/19/sql-listen.html) and [`NOTIFY`](https://www.postgresql.org/docs/19/sql-notify.html) to exchange notifications in multi-channel workloads.

Finally, the new [`password_expiration_warning_threshold`](https://www.postgresql.org/docs/19/runtime-config-connection.html#GUC-PASSWORD-EXPIRATION-WARNING-THRESHOLD) setting warns users when their password is about to expire.

### Other notable changes

PostgreSQL 19 adds server-side support for [Server Name Indication (SNI)](https://www.postgresql.org/docs/19/ssl-tcp.html#SSL-SNI), letting a single PostgreSQL server present a different TLS certificate based on the hostname a client connects to. You can map hostnames to their certificates and keys in the new [`pg_hosts.conf`](https://www.postgresql.org/docs/19/runtime-config-file-locations.html#GUC-HOSTS-FILE) file. PostgreSQL 19 also now issues a warning after a successful `md5` password authentication, which you can turn off with the [`md5_password_warnings`](https://www.postgresql.org/docs/19/runtime-config-connection.html#GUC-MD5-PASSWORD-WARNINGS) setting. `md5` password authentication was deprecated in PostgreSQL 18 and will be removed in a future release. If you require password-based authentication, use [SCRAM authentication](https://www.postgresql.org/docs/19/auth-password.html). This release also removes support for RADIUS authentication.

[Just-in-time compilation (JIT)](https://www.postgresql.org/docs/19/jit.html) is now disabled by default, as the cost estimates PostgreSQL used to decide when to use it proved unreliable. If you run many large analytical queries, you may want to enable JIT. When PostgreSQL is built with LZ4 support, the default [TOAST](https://www.postgresql.org/docs/19/storage-toast.html) compression method is now `lz4`.

### Additional Features

Many other new features and improvements have been added to PostgreSQL 19 that may also be helpful for your use cases. Please see the [release notes](https://www.postgresql.org/docs/19/release-19.html) for a complete list of new and changed features.

### About PostgreSQL

[PostgreSQL](https://www.postgresql.org) is the world's most advanced open source database, with a global community of thousands of users, contributors, companies and organizations. Since its beginnings at the University of California, Berkeley over 40 years ago, PostgreSQL has continued with an unmatched pace of development. PostgreSQL's mature feature set not only matches top proprietary database systems, but exceeds them in advanced database features, extensibility, security, and stability.

### Links

- [Download](https://www.postgresql.org/download/)
- [Release Notes](https://www.postgresql.org/docs/19/release-19.html)
- [Press Kit](https://www.postgresql.org/about/press/)
- [Security Page](https://www.postgresql.org/support/security/)
- [Versioning Policy](https://www.postgresql.org/support/versioning/)
- [Contact](https://www.postgresql.org/about/contact/)
- [Donate](https://www.postgresql.org/about/donate/)

## More About the Features

For explanations of the above features and others, please see the following resources:

- [Release Notes](https://www.postgresql.org/docs/19/release-19.html)
- [Feature Matrix](https://www.postgresql.org/about/featurematrix/)

## Where to Download

There are several ways you can download PostgreSQL 19, including:

- The [Official Downloads](https://www.postgresql.org/download/) page, which contains installers and tools for [Windows](https://www.postgresql.org/download/windows/), [Linux](https://www.postgresql.org/download/linux/), [macOS](https://www.postgresql.org/download/macosx/), and more.
- [Source Code](https://www.postgresql.org/ftp/source/v19.0)

Other tools and extensions are available on the [PostgreSQL Extension Network](http://pgxn.org/).

## Documentation

PostgreSQL 19 comes with HTML documentation as well as man pages, and you can also browse the documentation online in both [HTML](https://www.postgresql.org/docs/19/) and [PDF](https://www.postgresql.org/files/documentation/pdf/19/postgresql-19-US.pdf) formats.

## Licence

PostgreSQL uses the [PostgreSQL License](https://www.postgresql.org/about/licence/), a BSD-like "permissive" license. This [OSI-certified license](http://www.opensource.org/licenses/postgresql/) is widely appreciated as flexible and business-friendly, since it does not restrict the use of PostgreSQL with commercial and proprietary applications. Together with multi-company support and public ownership of the code, our license makes PostgreSQL very popular with vendors wanting to embed a database in their own products without fear of fees, vendor lock-in, or changes in licensing terms.

## Contacts

Website

- [https://www.postgresql.org/](https://www.postgresql.org/)

Email

- [press@postgresql.org](mailto:press@postgresql.org)

## Images and Logos

Postgres, PostgreSQL, and the Elephant Logo (Slonik) are all registered trademarks of the [PostgreSQL Community Association](https://www.postgres.ca). If you wish to use these marks, you must comply with the [trademark policy](https://www.postgresql.org/about/policies/trademarks/).

## Corporate Support and Donations

PostgreSQL enjoys the support of numerous companies, who sponsor developers, provide hosting resources, and give us financial support. See our [sponsors](https://www.postgresql.org/about/sponsors/) page for some of these project supporters.

There is also a large community of [companies offering PostgreSQL Support](https://www.postgresql.org/support/professional_support/), from individual consultants to multinational companies.

If you wish to make a financial contribution to the PostgreSQL Global Development Group or one of the recognized community non-profit organizations, please visit our [donations](https://www.postgresql.org/about/donate/) page.
