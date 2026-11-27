---
title: Introducing Dedicated Read Replicas for Postgres
link: https://planetscale.com/blog/introducing-postgres-dedicated-read-replicas
source: planetscale-com
published: 2026-10-06T00:00:00Z
updated: 2026-10-06T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Etienne Berube
- Nick Holden
- Chris Sinjakli
labels:
- postgres
- product
summary: Use dedicated read replicas to isolate read-only workloads and distribute your data across data centers around the world.
content: extracted
html: 2026-10-06-introducing-dedicated-read-replicas-for-postgres.html
remote_preview:
  url: https://planetscale.com/assets/introducing-postgres-dedicated-read-replicas-social-0_VVRuwl.png
---

Starting today, you can create dedicated read replicas for Postgres databases on PlanetScale. This has been a widely requested feature on our platform, and you now have full control over creating, sizing, and connecting to them.

## [Why dedicated read replicas?](https://planetscale.com/blog/introducing-postgres-dedicated-read-replicas#why-dedicated-read-replicas)

Dedicated read replicas are additional Postgres instances that replicate asynchronously from the primary cluster.

A common use case for this is reducing read latency for a global set of clients. Your primary database cluster exists in one region (say, `us-east-1`), while dedicated read replicas are stood up in European data centers (`eu-west-1` and `eu-central`). Read traffic for clients and customers in Europe can be directed to one of these nodes, speeding up the end-user experience.

In your PlanetScale dashboard, this looks like:

You may not require a globally distributed database, but dedicated read replicas are useful even within a single cloud region.

You can create one or more dedicated read replicas within the same region and create connection credentials targeting them individually, allowing you to control precisely which work each is doing. For example, you can have your primary set up to handle all writes and general OLTP traffic, a dedicated read replica for vector search with `pgvector`, and an additional replica to handle analytics and longer-running queries that you don't want impacting production.

## [Creation and management](https://planetscale.com/blog/introducing-postgres-dedicated-read-replicas#creation-and-management)

You can create and manage dedicated read replicas from the "Clusters" page in the PlanetScale dashboard, the API, or the CLI.

Each replica may have different requirements for RAM, latency, and performance. You can configure the instance size and disk characteristics of every node, letting you tune them for each use case.

For example, above we can see the `eu-read-node` instance is running on an `M-640` with 8 vCPUs, 64GB of RAM, and a fast local NVMe drive. The `dublin-double` is configured as two `PS-320` instances, with storage options like disk size, IOPS, and throughput being configurable separately from the primary configuration.

## [Get started](https://planetscale.com/blog/introducing-postgres-dedicated-read-replicas#get-started)

Learn more about using dedicated read replicas and their limitations [in the docs](https://planetscale.com/docs/postgres/scaling/dedicated-read-replicas) and give them a try on your database today.
