---
title: The architecture of Neki
link: https://planetscale.com/blog/the-architecture-of-neki
source: planetscale-com
published: 2026-09-18T00:00:00Z
updated: 2026-09-18T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Harshit Gangal
labels:
- engineering
- neki
summary: Sharded Postgres, from the team behind Vitess
content: extracted
html: 2026-09-18-the-architecture-of-neki.html
remote_preview:
  url: https://planetscale.com/assets/the-architecture-of-neki-social-CvHXrMho.png
---

Meet Neki: sharding for Postgres. Neki allows applications to connect to massive, sharded databases over a single connection string. This post takes apart the architecture from the bottom up, one piece at a time, starting with what's underneath all of it.

## [Real Postgres](https://planetscale.com/blog/the-architecture-of-neki#real-postgres)

Neki is built as a sharding and scaling solution for real Postgres. It's not a fork, nor a wire-compatible reimplementation, nor a MySQL sharding idea wearing a Postgres label. Neki uses ordinary PostgreSQL instances that store rows in Postgres data pages using MVCC, carry out transactions, and work as you would expect with `psql` and other Postgres drivers. Neki builds around those instances to let you shard them, scale them, and manage them as one database.

Let's take a look:

## [PostgresManager](https://planetscale.com/blog/the-architecture-of-neki#postgresmanager)

Using vanilla Postgres means Neki needs a way to run and manage each instance. That includes starting and stopping Postgres, owning its data directory, and configuring replication so a new instance can join a shard. **PostgresManager** handles this coordination, running as the first process in the Postgres container and managing the `postgres` process directly.

## [Sidecar](https://planetscale.com/blog/the-architecture-of-neki#sidecar)

Postgres uses a separate backend process for each connection and limits how many can be open at once. Neki’s **Sidecar** sits in front of each instance and pools connections, letting many client connections share fewer Postgres backends.

The Router, which is the component that accepts external client connections, communicates with the Postgres nodes via these Sidecars.

It also reports each Postgres instance's health and whether it is a primary or replica, so the rest of the cluster knows whether it can receive write queries.

The pool doesn't treat every connection the same way. The length of time a connection is checked out for use varies depending on what it's being used for. A multi-statement transaction holds on to its connection until `commit` or `rollback`. A session-scoped advisory lock needs a connection of its own, because the lock has to outlive whatever transaction is open at the time and can't share that connection. Everything else checks a connection out and hands it back the moment the statement finishes.

The Sidecar knows which of the three to use because the Router sends the necessary information with the query: autocommit, an open transaction, or a session that has to stay on one backend.

## [Shards](https://planetscale.com/blog/the-architecture-of-neki#shards)

Each Postgres instance gets its own Sidecar and PostgresManager pair. Real deployments need more than one instance: a primary and its replicas. Neki calls that group a **shard**, the unit it splits data across. It's always advised to run a shard with a primary and 2+ replicas for high availability, as well as for additional read query capacity.

A shard is considered one Postgres cluster. Its replicas are physical copies of the primary, so they share a catalog and the same object identifiers.

Object Identifiers (OIDs) are how Postgres tracks objects internally, rather than by name. A client reads a column’s type OID off the wire to interpret its bytes and may cache that OID for later re-use. A custom type therefore needs to carry the same OID no matter which shard answers the query. Independent shards can assign that type different OIDs, so Neki designates one shard in the entire Neki cluster as the **authoritative shard**. This shard is the source of truth for translating custom type OIDs in responses from other shards to match. It ensures OIDs are consistent across the many shards of the Neki cluster.

The authoritative shard's Sidecar also watches for schema changes and reports them to the Routers. This keeps the Routers' view of the schema current when a table is renamed or a column is dropped.

## [Admin](https://planetscale.com/blog/the-architecture-of-neki#admin)

In a distributed system, instances can fail independently while the rest of the system lives on. Neki is no different. A primary or replica can go down at any moment while its fellow instances on the shard are healthy. The **Admin**'s job is to detect failures, promote a replica, and maintain each shard’s durability policy.

It health-checks every Sidecar, tracks replication lag for each replica, and decides when a shard needs a new primary. When a primary goes down, it coordinates an emergency failover, promoting a replica to take its place. It can also coordinate a planned switchover, which are needed for intentional node resizes and version upgrades. In both situations, Admin uses `pg_rewind` to bring diverged instances onto the new primary’s timeline, copying only the data that changed since the timelines diverged.

Each shard has a durability policy that determines when a commit is acknowledged:

- **Async:** The primary acknowledges the commit without waiting for a replica.
- **Sync:** The primary waits for a replica to confirm the commit, protecting against the loss of a single node.
- **Cross-zone sync:** The primary waits for confirmation from a replica in another availability zone, protecting against the loss of the primary’s zone.

Postgres enforces whichever one is configured, using its own synchronous replication machinery. The Admin keeps that configuration correct as replicas join or leave shards, or a failover moves the primary to a different zone.

Much of Admin’s work, however, doesn’t involve changing the primary. It repoints replicas to the correct replication source and corrects roles when Postgres and the topology disagree.

## [Operator](https://planetscale.com/blog/the-architecture-of-neki#operator)

Neki’s components need to be deployed, updated, and replaced when their machines fail. Neki is built Kubernetes-first, and the **Operator** manages this full lifecycle.

The Operator models a cluster as a hierarchy. A cluster owns routers and shards, and each shard owns the pods running its Postgres instances and Sidecars. When the Neki cluster configuration changes, the Operator works out which pods need to be created, updated, or removed.

How it replaces an instance depends on whether that instance is still running. For a live instance, the Operator builds a replacement and confirms it has caught up before deleting the old one. If a node fails and loses its ephemeral storage, the Operator rebuilds the lost instance from scratch once its safety checks pass.

Admin and the Router handle the database side of those disruptions. Admin coordinates a switchover for planned primary replacements or a failover when a primary goes down. The Router can buffer queries that are safe to retry while a healthy primary becomes available.

## [Router](https://planetscale.com/blog/the-architecture-of-neki#router)

We've talked a lot about how the Neki cluster operates and handles failure internally. What we've yet to dive into is how applications use the thing!

The **Router** is the entry point for clients connecting to a Neki cluster, presenting a single Postgres wire-protocol endpoint to connect to a (potentially) massive sharded database. Applications use Postgres drivers to send SQL and open transactions without managing connections to individual shards.

Authentication and role checks are done as if it were the Postgres instance itself, and the protocol's own extended-query flow and prepared-statement lifecycle are all built into the Router.

Once a query arrives, the Router runs a Postgres-compatible parser against the authoritative shard's catalog, plans it against the current sharding layout, and sends it to whichever Sidecar needs to run it over gRPC.

Not every query can run on a single shard. A join may need data from several shards or an aggregate may need to read from all of them. The **Router** coordinates that work as a [distributed query](https://planetscale.com/blog/what-is-a-neki-router).

Whenever possible, it leaves the work to the Postgres instances. If both sides of a join are on the same shard, the Router sends the join to that shard. When a join needs to run across shards, the Router executes it itself, choosing between nested-loop, hash, and merge joins based on cost estimations.

Earlier, we covered how Admin promotes a new primary during a switchover or failover. If that happens, the Router can buffer queries, giving the Admin time to complete the handover. For queries that can safely be retried after failing against a primary, the Router buffers the query and waits, for a fixed time, for a healthy primary. Once a healthy primary is available, the Router releases queued queries gradually.

## [Data Topology](https://planetscale.com/blog/the-architecture-of-neki#data-topology)

Router, Sidecars, and Admin all need a consistent picture of which shards exist, what key ranges they own, and which tables are sharded at all. If the Router's copy is wrong, a query can land on the wrong shard. This is all specified with a [Data Topology](https://planetscale.com/blog/what-is-a-data-topology), and **etcd** holds the single, authoritative copy of it. When the Data Topology changes, the Router, Sidecars, and Admin pick up the updated configuration without a restart or manual synchronization.

The Data Topology defines **shard groups**, named sets of physical shards, each owning a range of routing keys. Each table belongs to a shard group. **Shard indexes** specify the columns or expressions and the strategy used to turn row values into routing keys. Those keys determine which shard receives each row.

## [Replicator](https://planetscale.com/blog/the-architecture-of-neki#replicator)

As a database grows, its layout may need to change. Tables need to be imported, shards need to be split, and schemas need to change all while applications keep using the database.

Neki's **Replicator** handles the data movement behind all such operations. It runs as a separate process colocated with a shard's Sidecar and Postgres. It is responsible for copying existing rows to new destinations, and also keeping the data current by decoding changes from a Postgres logical replication stream and applying them as SQL.

Three workflows use the Replicator:

- **MoveTables** relocates a set of tables, including imports from an external Postgres instance
- **Reshard** redistributes data across shard key ranges, allowing a shard to be split when it outgrows its capacity
- **OnlineDDL** changes a table's schema by building a shadow table alongside the original and keeping it current through the same change-data-capture pipeline MoveTables and Reshard use to relocate rows. A final rename swaps the new table into place. This supports changes such as repartitioning a table, alongside changes that would otherwise require a blocking operation.

Once the data has been copied and the destination is caught up, the workflow switches from the original tables or shards to their replacements. This is the cutover. The Router uses the same buffering mechanism that handles primary changes for this step. It buffers queries during that switch and releases them afterward.

Together, these components let Neki scale Postgres horizontally while presenting a single database to applications.

## [Get started](https://planetscale.com/blog/the-architecture-of-neki#get-started)

Neki is in Platform Preview right now.

Start a [Neki](https://app.planetscale.com/new) cluster today: build on it from scratch, or import an existing Postgres database.
