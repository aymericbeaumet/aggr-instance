---
title: DuckDB 2.0.0 LTS “Cinnamon Teal”
link: https://duckdb.org/2026/10/21/announcing-duckdb-200
source: hnrss-org
published: 2026-10-21T14:01:22Z
updated: 2026-10-21T14:01:22Z
first_seen: 2026-10-21T19:14:42.600331207Z
authors:
- nnx
content: extracted
html: 2026-10-21-duckdb-2-0-0-lts-cinnamon-teal.html
remote_preview:
  url: https://duckdb.org/images/blog/thumbs/duckdb-release-2-0-0.png
---

*TL;DR: The DuckDB team is happy to announce that today we're releasing DuckDB version 2.0.0 LTS, codenamed “Cinnamon Teal” (Spatula cyanoptera). This is our biggest feature release so far, and it ships a polished version of the Quack protocol, asynchronous I/O, a first-class `VARIANT` type, agent mode, triggers, an extensible SQL parser and much more, built from close to 20,000 commits since DuckDB 1.5.*

We are proud to release DuckDB 2.0.0, codenamed “Cinnamon Teal” after the [*Spatula cyanoptera*](https://en.wikipedia.org/wiki/Cinnamon_teal), a strikingly reddish-brown duck found in the western Americas.

While [DuckDB 1.0](https://duckdb.org/2024/06/03/announcing-duckdb-100.html) was all about stability and backward compatibility, DuckDB 2.0 is above all a feature release. In this blog post, we summarize the highlights and then walk through the breaking changes and how to migrate. We showcased many of the new features in [“A Preview of DuckDB v2.0”](https://duckdb.org/2026/08/17/duckdb-20-highlights.html), which covers each one in more depth. For the complete release notes, see the [release page on GitHub](https://github.com/duckdb/duckdb/releases/tag/v2.0.0).

> Tip To install the new version, please visit the [installation page](https://duckdb.org/install/). Some clients, such as the CLI and Python, are already available. Other client libraries (e.g., Go, R, Java) and some extensions can take a few days to release due to the extra changes and review rounds required.

With this release, there are currently three DuckDB lines available. DuckDB 2.0 is a long-term support (LTS) release with support until at least October 2027. The previous LTS release, version 1.4, is still supported until late November 2026. To ensure a smooth transition and allow sufficient time for testing, we extended the support period of DuckDB 1.5 until March 2027, effectively also turning it into an LTS release. For details, see the [release calendar](https://duckdb.org/release_calendar.html#lts-releases).

To find your way around the documentation for all of the new features, we also published a [“What's New in the Docs for 2.0”](https://duckdb.org/docs/2.0/whats_new_in_the_docs.html) page that links each feature below to its documentation.

## [DuckDB as a Server](https://duckdb.org/2026/10/21/announcing-duckdb-200#duckdb-as-a-server)

DuckDB has been an in-process database since day one. In [DuckDB 1.5.3](https://duckdb.org/2026/05/20/announcing-duckdb-153.html), released in May, we introduced the Quack protocol that lets you run DuckDB in a traditional client-server setup, along with the [`quack` extension](https://duckdb.org/docs/2.0/core_extensions/quack.html), which implements DuckDB's native protocol, so any DuckDB process can [serve its databases](https://duckdb.org/docs/2.0/quack/overview.html#server-side-usage).

### [Quack and `CONNECT`](https://duckdb.org/2026/10/21/announcing-duckdb-200#quack-and-connect)

DuckDB 2.0.0 ships a polished version of the Quack protocol with substantial performance and stability improvements. It also introduces the new [`CONNECT` statement](https://duckdb.org/docs/2.0/sql/statements/connect.html) that allows attaching and switching to a remote database in a single command for better ergonomics:

#### [DuckDB server](https://duckdb.org/2026/10/21/announcing-duckdb-200#duckdb-server)

```sql
CALL quack_serve(
    token = 'my_token'
);

CREATE TABLE events
    (date DATE, name VARCHAR);

INSERT INTO events VALUES
    ('2027-04-07',
     'DuckCon #8');
```

quack:

#### [DuckDB client](https://duckdb.org/2026/10/21/announcing-duckdb-200#duckdb-client)

```sql
ATTACH 'quack:localhost'
    AS qk (TOKEN 'my_token');

CONNECT qk;
SELECT * FROM events;

┌────────────┬────────────┐
│    date    │    name    │
│    date    │  varchar   │
├────────────┼────────────┤
│ 2027-04-07 │ DuckCon #8 │
└────────────┴────────────┘
```

For simplicity, this example runs the server and the client on the same machine. By default, the server only listens on `localhost`. To accept connections from other machines, keep the server on `localhost` and put it behind a [TLS-terminating reverse proxy](https://duckdb.org/docs/2.0/quack/setup/reverse_proxy.html). The [`allow_other_hostname` option](https://duckdb.org/docs/2.0/quack/overview.html#starting-a-server), which exposes the server directly, is only meant for testing.

The `CONNECT` statement is not limited to Quack: it points your session at [any remote database that supports it](https://duckdb.org/docs/2.0/sql/statements/connect.html#supported-databases), including PostgreSQL and MySQL. For these, the new remote pushdown optimizer ships queries directly to the remote server instead of pulling tables over the wire.

### [Quack in the CLI](https://duckdb.org/2026/10/21/announcing-duckdb-200#quack-in-the-cli)

The CLI also has shortcuts for setting up a Quack client-server pair: the new [`-serve` and `-connect` arguments](https://duckdb.org/docs/2.0/clients/cli/arguments.html#serving-and-connecting-to-remote-databases) start a Quack server or connect to one directly from the command line:

#### [CLI server](https://duckdb.org/2026/10/21/announcing-duckdb-200#cli-server)

```batch
duckdb -serve
```

```plsql
CREATE TABLE t AS
      SELECT 42 AS x;
```

quack:

#### [CLI client](https://duckdb.org/2026/10/21/announcing-duckdb-200#cli-client)

```batch
duckdb -connect
```

```plsql
FROM t;

┌───────┐
│   x   │
│ int32 │
├───────┤
│    42 │
└───────┘
```

## [Performance](https://duckdb.org/2026/10/21/announcing-duckdb-200#performance)

### [Asynchronous I/O](https://duckdb.org/2026/10/21/announcing-duckdb-200#asynchronous-io)

DuckDB 2.0 introduces [asynchronous I/O](https://duckdb.org/docs/2.0/data/io.html) for reading Parquet and CSV files. The I/O layer now scales independently from the query processing layer, issuing reads in the background and keeping multiple requests in flight. This means far more parallelism for remote reads and dramatically faster queries on object storage such as [`s3://`](https://duckdb.org/docs/2.0/core_extensions/httpfs/s3api.html).

We described the design in detail in a [dedicated blog post](https://duckdb.org/2026/07/31/asynchronous-io.html). In that post, we showed the network throughput over the course of a query with and without asynchronous I/O:

![Network throughput](https://duckdb.org/images/blog/v20-release/sync-vs-async-network-throughput-light.svg) ![Network throughput](https://duckdb.org/images/blog/v20-release/sync-vs-async-network-throughput-dark.svg) *Network throughput. Higher utilization and shorter runtime are better.*

### [The `VARIANT` Type Becomes a First-Class Citizen](https://duckdb.org/2026/10/21/announcing-duckdb-200#the-variant-type-becomes-a-first-class-citizen)

The [`VARIANT` type](https://duckdb.org/docs/2.0/sql/data_types/variant.html) stores semi-structured, schema-less data such as JSON while keeping the performance of columnar execution. So you can think of it as JSON on steroids: a `VARIANT` column can store differently shaped data in every row, but unlike JSON it is not a text format.

[DuckDB 1.5 already introduced `VARIANT`](https://duckdb.org/2026/03/09/announcing-duckdb-150.html#variant-type), but DuckDB 2.0 fully integrates it. DuckDB 2.0 detects the common structure in your data and “shreds” it, so it compresses well and executes fast. The pipeline works end to end, from shredded execution straight out of storage to shredded `VARIANT` reading and writing for Parquet, backed by a family of [`variant_*` functions](https://duckdb.org/docs/2.0/sql/functions/variant.html). For example:

```sql
CREATE TABLE events (payload VARIANT);
INSERT INTO events
VALUES
    ('{"user": {"id": 42, "tags": ["a", "b"]}}'::JSON::VARIANT),
    ('{"event": "click", "x": 10, "y": 20}'::JSON::VARIANT);

SELECT variant_type(payload), variant_keys(payload)
FROM events;
```

```text
┌───────────────────────┬───────────────────────┐
│ variant_type(payload) │ variant_keys(payload) │
│        varchar        │       varchar[]       │
├───────────────────────┼───────────────────────┤
│ OBJECT                │ [user]                │
│ OBJECT                │ [event, x, y]         │
└───────────────────────┴───────────────────────┘
```

### [Agent Mode in the CLI](https://duckdb.org/2026/10/21/announcing-duckdb-200#agent-mode-in-the-cli)

Coding agents such as Claude Code, Codex and Cursor increasingly drive the DuckDB CLI on their users' behalf. The 2.0 CLI detects when it is being run by an agent and switches to [agent mode](https://duckdb.org/2026/10/09/agent-mode.html): it prints compact Markdown tables instead of padded boxes, says clearly when a result was truncated, stops runaway queries early and reports errors as JSON.

Long-running SQL queries report their expected cost prior to running, so the agent can decide whether to wait. In our experiments on TPC-H, agent mode cut the amount of DuckDB output the model had to read by 59%. You can turn it *on* for manual testing using the [`-agent` flag](https://duckdb.org/docs/2.0/clients/cli/agent_mode.html#activating-agent-mode) or instruct your agent to turn it *off* using the `-no-agent` flag.

## [Extensibility](https://duckdb.org/2026/10/21/announcing-duckdb-200#extensibility)

### [A Brand New SQL Parser](https://duckdb.org/2026/10/21/announcing-duckdb-200#a-brand-new-sql-parser)

DuckDB has always used a parser derived from PostgreSQL's. In 2.0, our own modern, extensible [PEG-based parser](https://duckdb.org/2026/08/20/duckdb-20-peg-parser.html), which debuted as an opt-in feature in [DuckDB 1.5](https://duckdb.org/2026/03/09/announcing-duckdb-150.html#peg-parser), becomes the default. Extensions can now hook into the grammar itself (an idea we first explored in our [research on runtime-extensible parsers](https://duckdb.org/2024/11/22/runtime-extensible-parsers.html)), so expect extensions that expose entirely new SQL syntax. Error messages now come with precise source locations. For details, see the [PEG parser documentation](https://duckdb.org/docs/2.0/sql/peg_parser.html). The new parser is designed to be compatible with the old one, so you should not notice the swap. If you do, please file an issue. In the meantime, you can switch back to the old parser using the [`legacy_parser` extension](https://duckdb.org/docs/2.0/core_extensions/legacy_parser.html) by running `SET enable_legacy_parser = true;`.

The old parser had served DuckDB since its first commit in 2018. Its grammar was generated with YACC/Bison, which produces an LALR(1) parser, so as DuckSQL grew, even small additions could collide with existing rules and raise `shift/reduce` or `reduce/reduce` conflicts. A PEG grammar avoids this by trying alternatives in the order they are written, so adding syntax no longer risks breaking unrelated rules. Python itself moved to a PEG-based parser in [version 3.9](https://peps.python.org/pep-0617/) for the same flexibility.

### [Write Extensions Once and Host Them Yourself](https://duckdb.org/2026/10/21/announcing-duckdb-200#write-extensions-once-and-host-them-yourself)

DuckDB 2.0 ships a revamped [C API v2](https://duckdb.org/docs/2.0/clients/c/overview.html#c-api-v2), which is considerably more powerful than its predecessor and exposes components such as file systems and table functions to extensions. Before marking it as stable, we would like to test it in the field, so please give it a try and [send us your feedback](https://github.com/duckdb/duckdb/discussions/27525)!

You can also register and host your own trusted extension repositories, so an organization can host and sign its own extensions and have them install and load just like the built-in ones:

```sql
SET allow_extension_repositories = 'allowed';

CREATE EXTENSION REPOSITORY my_repo
    WITH PREFIX 'https://extensions.example.org';

INSTALL AND LOAD my_ext FROM my_repo;
```

A repository is a name, a URL prefix and one or more RSA public keys that are trusted to sign the extensions served from it. The prefix can point at anything DuckDB can read: a local path, `https`, `s3`, you name it. At `CREATE` time, DuckDB fetches the repository's public keys and pins them into the repository definition, printing each key's SHA-256 fingerprint so you can compare it against one published out of band.

This feature is still a work in progress and its syntax may change in future releases. For more details, see the [Extension Repositories page](https://duckdb.org/docs/2.0/extensions/extension_repositories.html).

### [Core Extension Updates](https://duckdb.org/2026/10/21/announcing-duckdb-200#core-extension-updates)

DuckDB's [core extensions](https://duckdb.org/docs/2.0/core_extensions/overview.html) are also shipping several new features, fixes and performance optimizations. These are too numerous to all list here, so we only highlight a few of them and will cover them in dedicated blog posts in the coming weeks:

- We released [DuckLake 1.1](https://ducklake.select/2026/10/21/ducklake-11/), including a new specification and [reference implementation](https://duckdb.org/docs/2.0/core_extensions/ducklake.html).
- The [Iceberg extension](https://duckdb.org/docs/2.0/core_extensions/iceberg/overview.html) strives to become a feature-complete implementation. To this end, it adds REST catalog [`ATTACH` options](https://duckdb.org/docs/2.0/core_extensions/iceberg/iceberg_options.html#attach-options) that derive a table's location from its namespace or load table metadata from the `metadata-location` file returned by the catalog. Further options control the URL prefix and the namespace separator.
- The [Delta extension](https://duckdb.org/docs/2.0/core_extensions/delta.html) can create Delta tables with `CREATE TABLE` (including partitioning and Delta table properties), supports time travel and improves `VARIANT` support.

## [SQL Features](https://duckdb.org/2026/10/21/announcing-duckdb-200#sql-features)

### [Triggers](https://duckdb.org/2026/10/21/announcing-duckdb-200#triggers)

DuckDB 2.0 delivers triggers: the [`CREATE TRIGGER` statement](https://duckdb.org/docs/2.0/sql/statements/create_trigger.html) with `BEFORE` and `AFTER` triggers, `FOR EACH ROW` and `FOR EACH STATEMENT`, transition tables via `REFERENCING OLD/NEW TABLE`, multiple triggers per event, `RETURNING` on triggered tables and `DROP TRIGGER`. The classic use case is audit tables, where a trigger records what changed in response to a table modification. Some combinations, such as `BEFORE ... FOR EACH ROW` and `UPDATE ... FOR EACH ROW` triggers, are not yet supported; see the [limitations](https://duckdb.org/docs/2.0/sql/statements/create_trigger.html#limitations).

```sql
CREATE TRIGGER trg_audit
    AFTER UPDATE ON target
    REFERENCING
        OLD TABLE AS o
        NEW TABLE AS n
    FOR EACH STATEMENT
        INSERT INTO audit
        SELECT n.id, o.val, n.val
        FROM o JOIN n ON o.id = n.id;
```

Triggers can also maintain materialized views incrementally! For example, to keep track of the average age of people per nationality in a table `age_statistics`, you can use two triggers – one for insert and another for delete operations – and store the sum and the count alongside the average.

To see more details, click here.

```sql
CREATE TABLE people (name VARCHAR, nationality VARCHAR, age INTEGER);
CREATE TABLE age_statistics (
    nationality VARCHAR PRIMARY KEY,
    sum_age INTEGER,
    num_people INTEGER,
    average_age DOUBLE
);
```

On insertion, we aggregate the new rows per nationality and merge them into `age_statistics`, adding a row for each new nationality:

```sql
CREATE TRIGGER people_insert
    AFTER INSERT ON people
    REFERENCING NEW TABLE AS n
    FOR EACH STATEMENT
        MERGE INTO age_statistics s
        USING (
            SELECT nationality, sum(age) AS sum_age, count(*) AS num_people
            FROM n
            GROUP BY nationality
        ) d
        ON s.nationality = d.nationality
        WHEN MATCHED THEN UPDATE SET
            sum_age = s.sum_age + d.sum_age,
            num_people = s.num_people + d.num_people,
            average_age = (s.sum_age + d.sum_age) / (s.num_people + d.num_people)
        WHEN NOT MATCHED THEN INSERT
            VALUES (d.nationality, d.sum_age, d.num_people, d.sum_age / d.num_people);
```

On deletion, we subtract the deleted rows and drop a nationality once its last person has left:

```sql
CREATE TRIGGER people_delete
    AFTER DELETE ON people
    REFERENCING OLD TABLE AS o
    FOR EACH STATEMENT
        MERGE INTO age_statistics s
        USING (
            SELECT nationality, sum(age) AS sum_age, count(*) AS num_people
            FROM o
            GROUP BY nationality
        ) d
        ON s.nationality = d.nationality
        WHEN MATCHED AND s.num_people = d.num_people THEN DELETE
        WHEN MATCHED THEN UPDATE SET
            sum_age = s.sum_age - d.sum_age,
            num_people = s.num_people - d.num_people,
            average_age = (s.sum_age - d.sum_age) / (s.num_people - d.num_people);
```

Let's see people come and go:

```sql
INSERT INTO people VALUES
    ('Alice', 'US', 34), ('Bram', 'NL', 27), ('Csilla', 'HU', 45),
    ('Dave', 'US', 58), ('Eva', 'NL', 19), ('Femke', 'NL', 41);
FROM age_statistics ORDER BY nationality;
```

```text
┌─────────────┬─────────┬────────────┬─────────────┐
│ nationality │ sum_age │ num_people │ average_age │
│   varchar   │  int32  │   int32    │   double    │
├─────────────┼─────────┼────────────┼─────────────┤
│ HU          │      45 │          1 │        45.0 │
│ NL          │      87 │          3 │        29.0 │
│ US          │      92 │          2 │        46.0 │
└─────────────┴─────────┴────────────┴─────────────┘
```

```sql
DELETE FROM people WHERE name = 'Alice';
DELETE FROM people WHERE nationality = 'HU';
FROM age_statistics ORDER BY nationality;
```

```text
┌─────────────┬─────────┬────────────┬─────────────┐
│ nationality │ sum_age │ num_people │ average_age │
│   varchar   │  int32  │   int32    │   double    │
├─────────────┼─────────┼────────────┼─────────────┤
│ NL          │      87 │          3 │        29.0 │
│ US          │      58 │          1 │        58.0 │
└─────────────┴─────────┴────────────┴─────────────┘
```

Triggers bring us one step closer to materialized views, a long-standing item in the [“future work” section of our roadmap](https://duckdb.org/roadmap.html#future-work).

### [SQL Dialect Additions](https://duckdb.org/2026/10/21/announcing-duckdb-200#sql-dialect-additions)

As always, DuckDB's [friendly SQL dialect](https://duckdb.org/docs/2.0/sql/dialect/friendly_sql.html) keeps growing. A few favorites from this release cycle:

- The [`$ variable` shorthand](https://duckdb.org/docs/2.0/sql/statements/set_variable.html#the-name-shorthand) lets you write `$x` in most places where an expression is allowed, instead of `getvariable ('x')`.
- [Data-modifying CTEs](https://duckdb.org/docs/2.0/sql/query_syntax/with.html#data-modifying-ctes) let you use `INSERT`, `UPDATE`, `DELETE` and `COPY` as pipeline steps.
- [`NEAREST` joins](https://duckdb.org/docs/2.0/sql/query_syntax/from.html#nearest-joins) turn top-k similarity search into a join clause, handy for vector and embedding workloads.
- [Nested schemas](https://duckdb.org/docs/2.0/sql/statements/create_schema.html#nested-schemas) allow schemas within schemas, e.g., `CREATE SCHEMA finance.reports`.
- [JSON mutation functions](https://duckdb.org/docs/2.0/data/json/json_functions.html#json-mutation-functions) `json_set`, `json_insert`, `json_replace` and `json_remove` return modified copies of JSON documents (see also our [blog post on JSON patches](https://duckdb.org/2026/08/18/reconciling-json.html)).

There is more: DuckDB 2.0.0 supports the SQL-standard [`FETCH FIRST`](https://duckdb.org/docs/2.0/sql/query_syntax/limit.html#fetch-first) and allows [`UNNEST` in the `GROUP BY` clause](https://duckdb.org/docs/2.0/sql/query_syntax/groupby.html#grouping-by-unnest). It also ensures well-defined [`MERGE`](https://duckdb.org/docs/2.0/sql/statements/merge_into.html) and [`UPDATE ... FROM`](https://duckdb.org/docs/2.0/sql/statements/update.html) semantics for cases when multiple rows are matched.

## [Migrating to 2.0](https://duckdb.org/2026/10/21/announcing-duckdb-200#migrating-to-20)

DuckDB 2.0 comes with a small set of breaking changes. For most users, upgrading is straightforward, but complex deployments running many DuckDB processes need more care. The storage format is the most likely source of problems: DuckDB versions before 2.0 cannot read database files created by DuckDB 2.0. DuckDB 2.0 can still read and write your existing database files, and these remain readable by older versions. We recommend testing 2.0.0 on a shadow system before switching production over. And, if your workload is mission-critical, you may want to wait for the first patch release, [coming in November](https://duckdb.org/release_calendar.html#upcoming-releases).

For the full details, see the [migration guide](https://duckdb.org/docs/2.0/operations_manual/migration.html).

## [Internals and Deprecations](https://duckdb.org/2026/10/21/announcing-duckdb-200#internals-and-deprecations)

### [Compiler Requirements](https://duckdb.org/2026/10/21/announcing-duckdb-200#compiler-requirements)

Following DuckDB's long stint on C++11, DuckDB 2.0 now [requires a C++17 compiler](https://duckdb.org/docs/2.0/dev/building/overview.html#prerequisites).

We also switched to Clang as the default compiler for our release builds. On Linux and macOS, both DuckDB and its core extensions are built with Clang. On Windows, the CLI is built with Clang, while most core extensions are still built with MSVC. We may move more Windows builds to Clang in the future, but this requires further changes.

### [Timezones, Calendars and Collations Without ICU](https://duckdb.org/2026/10/21/announcing-duckdb-200#timezones-calendars-and-collations-without-icu)

Timezone-aware timestamps, calendars and collations have always been powered by the ICU library. In 2.0, the ICU library is gone entirely: the [`icu` extension](https://duckdb.org/docs/2.0/core_extensions/icu.html) now implements timezones, calendars and collations itself, with the timezone data built directly from the IANA database and compressed down to around 45 kB.

Everything keeps working exactly as before, and the native implementation is also faster. If you find any issues, please [let us know](https://github.com/duckdb/duckdb/issues)!

### [Deprecations](https://duckdb.org/2026/10/21/announcing-duckdb-200#deprecations)

#### [Lambda Syntax](https://duckdb.org/2026/10/21/announcing-duckdb-200#lambda-syntax)

The old single-arrow lambda syntax (`x -> x + 1`) is now disabled by default. Use the Python-style syntax instead:

```sql
SELECT list_transform([1, 2, 3], lambda x: x + 1);
```

You can temporarily re-enable the old syntax with `SET lambda_syntax = 'enable_single_arrow';`. Both the flag and the single-arrow syntax will be removed entirely in v2.1.

#### [Amalgamation Builds](https://duckdb.org/2026/10/21/announcing-duckdb-200#amalgamation-builds)

The [amalgamation build](https://duckdb.org/docs/1.4/dev/building/overview), which bundles DuckDB's entire source code into a single header and a single source file, is now deprecated. The amalgamation has grown too large to be realistically useful in most cases.

#### [Gradual Phase-Out of macOS x86\_64](https://duckdb.org/2026/10/21/announcing-duckdb-200#gradual-phase-out-of-macos-x86_64)

We are gradually phasing out `osx_amd64` from the list of fully supported [platforms](https://duckdb.org/docs/2.0/operations_manual/installing_duckdb/requirements.html#supported-platforms). For the time being, DuckDB and [core extensions](https://duckdb.org/docs/2.0/core_extensions/overview.html) are still built and supported, but [community extensions](https://duckdb.org/docs/2.0/extensions/community_extensions.html) are no longer built for this platform. DuckDB 2.1 will stop supporting `osx_amd64`.

## [Summary](https://duckdb.org/2026/10/21/announcing-duckdb-200#summary)

These were a few highlights, but there are many more features and improvements in this release. There have been almost 20,000 commits by many contributors since DuckDB 1.5, shipping smaller features as well as performance and usability improvements. The full [release notes can be found on GitHub](https://github.com/duckdb/duckdb/releases/tag/v2.0.0).

We would like to thank our community for the detailed issue reports and feedback that have driven this release, and our special thanks go to our contributors!

![Benchmarking DuckDB v2.0-dev on NVIDIA Vera](https://duckdb.org/images/blog/thumbs/nvidia-vera-benchmark.svg)

### Benchmarking DuckDB v2.0-dev on NVIDIA Vera

![](https://duckdb.org/images/blog/authors/gabor_szarnyas.png)

Gábor Szárnyas

![From JSON Strings to Typed Columns](https://duckdb.org/images/blog/thumbs/json.svg)

### From JSON Strings to Typed Columns

The DuckDB team

![What Is Inside an Iceberg Table?](https://duckdb.org/images/blog/thumbs/iceberg-writes.svg)

### What Is Inside an Iceberg Table?

The DuckDB team
