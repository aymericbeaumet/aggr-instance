---
title: PostgreSQL 19 Released
link: https://simonwillison.net/2026/Nov/19/postgresql-19/
source: simonwillison-net
published: 2026-11-19T20:43:57Z
updated: 2026-11-19T20:43:57Z
first_seen: 2026-11-19T23:46:32.431702950Z
labels:
- ai-security-research
- mysql
- postgresql
- replication
- scaling
- wikipedia
summary: 'PostgreSQL 19 Released Most years PostgreSQL ships a new release in September, but this year''s release came two months later thanks mainly to the flurry of issues that the PostgreSQL team needed to address as part of the ongoing AI-reported vulnerability apocalypse. This release was definitely worth the wait. My favorite new feature is this one: For applications that send reads to standbys, PostgreSQL 19 adds "read-your-writes" consistency with the new WAIT FOR LSN command. A session on a standby can use WAIT FOR LSN to wait until the standby has replayed the write-ahead log (WAL) up to a specific log sequence number (LSN) before running a query, so the application can read data it just wrote on the primary. In practice, that looks like this: UPDATE movie SET genre = ''Dramatic'' WHERE genre = ''Drama''; SELECT pg_current_wal_insert_lsn(); That returns a value like 0/0306EE20. Store that somewhere (a signed cookie would work) such that on a subsequent request your application can do: WAIT FOR LSN ''0/0306EE20'' WITH (TIMEOUT ''0.1s''); SELECT * FROM movie WHERE genre = ''Drama''; The timeout is optional - if you provide one you''ll get an error if the replica you are talking to doesn''t catch up within the specified time. I first encountered this pattern in Wikipedia, which has a hand-rolled version of this built against MySQL replication: In addition to this, MediaWiki attempts to ensure that the user sees events occurring on the wiki in chronological order. A few seconds of lag can be tolerated, as long as the user sees a consistent picture from subsequent requests. This is done by saving the primary binlog position in the session, and then at the start of each request, waiting for the replica to catch up to that position before doing any reads from it. Here''s the relevant MediaWiki code, which these days uses MySQL''s WAIT_FOR_EXECUTED_GTID_SET() function and falls back to SOURCE_POS_WAIT(). Via Lobste.rs Tags: mysql, postgresql, replication, scaling, wikipedia, ai-security-research'
content: extracted
html: 2026-11-19-postgresql-19-released.html
---

**[PostgreSQL 19 Released](https://www.postgresql.org/about/news/postgresql-19-released-3449/)** ([via](https://lobste.rs/s/pqyuhk/postgresql_19_released "Lobste.rs")) Most years PostgreSQL ships a new release in September, but this year's release came two months later [thanks mainly](https://rhaas.blogspot.com/2026/10/postgres-19-is-late-and-its-ais-fault.html) to the flurry of issues that the PostgreSQL team needed to address as part of the ongoing AI-reported vulnerability apocalypse.

This release was definitely worth the wait. My favorite new feature is this one:

> For applications that send reads to standbys, PostgreSQL 19 adds "read-your-writes" consistency with the new [`WAIT FOR LSN`](https://www.postgresql.org/docs/19/sql-wait.html) command. A session on a standby can use `WAIT FOR LSN` to wait until the standby has replayed the write-ahead log (WAL) up to a specific [log sequence number (LSN)](https://www.postgresql.org/docs/19/wal-internals.html) before running a query, so the application can read data it just wrote on the primary.

In practice, that looks like this:

```
UPDATE movie SET genre = 'Dramatic' WHERE genre = 'Drama';
SELECT pg_current_wal_insert_lsn();
```

That returns a value like `0/0306EE20`. Store that somewhere (a signed cookie would work) such that on a subsequent request your application can do:

```
WAIT FOR LSN '0/0306EE20' WITH (TIMEOUT '0.1s');
SELECT * FROM movie WHERE genre = 'Drama';
```

The timeout is optional - if you provide one you'll get an error if the replica you are talking to doesn't catch up within the specified time.

I first encountered this pattern [in Wikipedia](https://www.mediawiki.org/wiki/Manual:Database_access#Lag), which has a hand-rolled version of this built against MySQL replication:

> In addition to this, MediaWiki attempts to ensure that the user sees events occurring on the wiki in chronological order. A few seconds of lag can be tolerated, as long as the user sees a consistent picture from subsequent requests. This is done by saving the primary binlog position in the session, and then at the start of each request, waiting for the replica to catch up to that position before doing any reads from it.

Here's [the relevant MediaWiki code](https://github.com/wikimedia/mediawiki/blob/1ff78194b182ee4b5b3678f7d9fc9aefd0852cf7/includes/libs/Rdbms/Replication/MysqlReplicationReporter.php#L238-L299), which these days uses MySQL's [WAIT\_FOR\_EXECUTED\_GTID\_SET()](https://dev.mysql.com/doc/refman/9.7/en/gtid-functions.html#function_wait-for-executed-gtid-set) function and falls back to [SOURCE\_POS\_WAIT()](https://dev.mysql.com/doc/refman/8.4/en/replication-functions-synchronization.html#function_source-pos-wait).
