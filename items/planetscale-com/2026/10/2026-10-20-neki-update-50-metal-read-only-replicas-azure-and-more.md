---
title: 'Neki update: $50 Metal, read-only replicas, Azure, and more'
link: https://planetscale.com/blog/neki-updates-and-preparing-for-production
source: planetscale-com
published: 2026-10-20T04:00:00Z
updated: 2026-10-20T04:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Nick Van Wiggeren
labels:
- neki
- product
summary: Postgres compatibility, read-only replicas, smaller Metal instances, Azure, and more have landed for Neki since platform preview.
content: extracted
html: 2026-10-20-neki-update-50-metal-read-only-replicas-azure-and-more.html
remote_preview:
  url: https://planetscale.com/assets/neki-updates-and-preparing-for-production-social-zwomWUMY.png
---

Neki, our sharded Postgres, has been in platform preview for about a month. The team continues to work towards the path to production-readiness, and we've added quite a few new features and capabilities since launch.

Here's a quick look at what's landed since the preview opened and how you can start evaluating Neki for your workload today.

## [What's shipped](https://planetscale.com/blog/neki-updates-and-preparing-for-production#whats-shipped)

We've recently added both read-only replicas and smaller Metal instances to Neki starting at just $50 per month.

[Read-only replicas](https://planetscale.com/changelog/neki-read-only-replicas) were a hotly-requested feature for Postgres, particularly for customers with a large, global userbase. Neki now also supports read-only replicas for shards in your cluster.

We also [now support smaller Metal node sizes](https://planetscale.com/changelog/neki-metal-m-10-to-m-160), going all the way down to database nodes with 1/8th vCPU, 1GB RAM, and 10GB of locally-attached NVMe SSD.

You can try both of these capabilities on Neki in PlanetScale today. These are just a few amongst many others, including:

- **[Manage Neki extensions with Terraform and the CLI](https://planetscale.com/changelog/neki-extensions-terraform-cli)** (Sept 18). Enable extensions by name with `extensions` in a `planetscale_neki_configuration_profile` resource or with `pscale branch config-profile update --extensions`, and change their settings in the same update.
- **[Branch metrics in the PlanetScale MCP server](https://planetscale.com/changelog/mcp-branch-metrics)** (Oct 9). `planetscale_get_branch_metrics` pulls CPU, memory, disk, replication lag, and connections into your AI agent, or wherever you connect to your infrastructure via MCP. It works for Neki, Postgres, and Vitess.
- **[PlanetScale on Azure](https://planetscale.com/changelog/planetscale-on-azure)** (Oct 13). You can now create Neki and Postgres databases in Azure regions, alongside AWS and Google Cloud.
- **[Neki router parameters in Terraform](https://planetscale.com/changelog/neki-router-parameters-terraform)** (Oct 14). Terraform provider v1.13.0 adds parameter management to `planetscale_neki_router`, and the `planetscale_databases` data source can now filter by `kind` (`mysql`, `postgresql`, or `neki`).
- **[CLI shows which parameters need an update](https://planetscale.com/changelog/cli-parameters-need-update)** (Oct 14). CLI v0.346.0 adds a "requires update" column for Neki configuration profiles, admins, and sidecars, plus a `requires_update` field in JSON output.
- **[Huge pages for Neki and Postgres](https://planetscale.com/changelog/postgres-neki-huge-pages)** (Oct 15). Set `huge_pages` to `on` in a configuration profile's Parameters tab on supported cluster sizes. The change requires a restart.

## [Under the hood: query layer](https://planetscale.com/blog/neki-updates-and-preparing-for-production#under-the-hood-query-layer)

Much of the engineering effort since preview has gone into the query layer: making more of your existing SQL run on a sharded database without changes, and making the standard Postgres tools you already use work against Neki.

The team has merged more than 650 changes since the preview opened. A few more highlights:

- More of your SQL just works. INSERT ... SELECT ... ON CONFLICT, cross-shard `UPDATE ... FROM`, `DELETE ... USING` and `MERGE`, correlated `EXISTS` almost anywhere in a query, `ARRAY(SELECT ...)`, SQL cursors, `LISTEN`/`NOTIFY`, materialized views, and PL/pgSQL exception handlers are all now supported.
- Export with standard tools. `COPY ... TO STDOUT` now works on sharded tables in text, CSV, and binary, so pg\_dump and other export tools can read data out of a sharded Neki database.
- Faster bulk writes. Distributed `INSERT ... SELECT` is about 76x faster than at launch, and COPY ingest is roughly 5x faster.
- See how your query runs on every shard. `EXPLAIN (NEKI_PG_PLAN)` shows Neki's distributed plan together with the Postgres plan each shard ran. The output follows Postgres's own EXPLAIN format, so pgAdmin, DBeaver, `explain.dalibo.com`, and `explain.depesz.com` can read it.

## [Start evaluating now](https://planetscale.com/blog/neki-updates-and-preparing-for-production#start-evaluating-now)

If you're running Postgres and expect to need sharding or any other Neki features like online schema changes or no downtime version upgrades, now is a good time to start the conversation. Email us at [neki@planetscale.com](mailto:neki@planetscale.com) or [fill out the form](https://planetscale.com/contact), and we'll set up a call with our solutions team.

If you want to learn more about Neki, we recommend starting with [When to shard](https://planetscale.com/docs/neki/when-to-shard), [Neki best practices](https://planetscale.com/docs/neki/best-practices), and [Data topology](https://planetscale.com/docs/neki/data-topology).
