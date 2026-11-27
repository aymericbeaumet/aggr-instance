---
title: Blocking cutovers to save replication slots
link: https://planetscale.com/blog/how-to-make-a-replication-slot-survive-a-cutover
source: planetscale-com
published: 2026-09-21T00:00:00Z
updated: 2026-09-21T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Simeon Griggs
labels:
- engineering
summary: Postgres understands high-availability architecture, but its defaults assume you don't have replication slots. This can get you into trouble in a cutover event. The combination of what Postgres can do and what PlanetScale lets you do is what keeps connected applications safe.
content: extracted
html: 2026-09-21-blocking-cutovers-to-save-replication-slots.html
remote_preview:
  url: https://planetscale.com/assets/how-to-make-a-replication-slot-survive-a-cutover-social-Dq998112.png
---

Reliability is kinda our whole thing at PlanetScale. We maintain a flawless uptime record and preach the gospel of high availability. It might seem counterintuitive, but this involves embracing failure.

A resilient system anticipates the server failures inherent to cloud-native environments.

## [When we'll block a cutover](https://planetscale.com/blog/how-to-make-a-replication-slot-survive-a-cutover#when-well-block-a-cutover)

Most stuff in your primary database makes its way into its replicas without issue. The write-ahead log (WAL) streams updates from the primary to replicas, so after a brief moment (replication lag), the databases are effectively the same.

In the event of a resize or configuration change, a replica that is an exact match of the primary is "promoted" to become the new primary. The only penalty is a few seconds of primary unavailability and dropped connections (except to PgBouncers).

Most commonly, you might think that a replica is promotion-ready because it has replayed WAL to completion. A less obvious condition is whether the replica has a synchronized, usable copy of logical replication slots.

In vanilla Postgres, you can proceed with a promotion even if replication slots aren't caught up, potentially breaking your connected applications.

On PlanetScale Postgres, we'll block you. It's for your own good.

## [Wait, what's a replication slot?](https://planetscale.com/blog/how-to-make-a-replication-slot-survive-a-cutover#wait-whats-a-replication-slot)

Postgres contains two types of replication slots, which act as "bookmarks" in the WAL.

- **Physical slots** belong to the replicas. Each one tracks how far a replica has gotten through the WAL.
- **Logical slots** decode WAL into per-row change events, which are typically piped to external subscribers such as search indexes, analytics tools, and queues.

This post is concentrated on the latter. Postgres is designed to be okay with replica promotion so long as the data is caught up, but with no concern for whether logical slots exist on the replica.

Without a copy of that bookmark on the promoted replica, the database is fine, but the change stream is not. Consumers of that slot miss every event since they last acknowledged one, or they stall until you take a new snapshot. Promoting an incomplete replica to primary is a data-loss event for your downstream applications.

## [Who's responsible for a cutover?](https://planetscale.com/blog/how-to-make-a-replication-slot-survive-a-cutover#whos-responsible-for-a-cutover)

It's helpful to put a box around PlanetScale and Postgres and define their roles and responsibilities in creating a seamless cutover experience.

*Click on any of the boxes in the diagram below for more details*

### [Postgres' cutover features](https://planetscale.com/blog/how-to-make-a-replication-slot-survive-a-cutover#postgres-cutover-features)

Postgres provides a lot of functionality to create a high-availability architecture. It understands the concept of a primary and replicas, and the WAL allows the former to stream updates to the latter, keeping their data synchronized.

(Note: What PlanetScale calls *replicas*, Postgres documentation calls *standbys*. The additional layer of confusion this adds to writing about *replication slots* is not lost on the author of this post.)

Postgres can report a replica's current state, including whether it's connected, how far through the WAL it is, replication lag, and more.

Postgres won't provision servers, decide which replica to promote, or decide whether a replica is ready for promotion. The operator makes these decisions.

### [The PlanetScale operator](https://planetscale.com/blog/how-to-make-a-replication-slot-survive-a-cutover#the-planetscale-operator)

The joy of PlanetScale Postgres is its custom Kubernetes operator, which, among other things, makes critical operations like resizing, reconfiguring, or reviving a database from failure much safer.

In relation to logical replication slots, it will:

- **Detect misconfigured slots.** The operator watches `pg_replication_slots` and records any misconfigurations such as missing `failover = true` or whether `hot_standby_feedback` or `sync_replication_slots` are `off`. The next section covers the correct configuration.
- **Alert you.** In the event of a misconfiguration being detected you will receive email alerts and a promenant banner is displayed in the PlanetScale dashboard with details on how to correct.
- **Block problematic planned cutovers.** A resize, parameter change, or maintenance that would silently drop a slot is blocked by the operator, protecting downstream applications from data loss events.
- **Wait for the slot to be usable.** Before a promotion event takes place, the operator waits for the failover-enabled slots to report a ready state. It also manages `synchronized_standby_slots` so one dead replica doesn't block all logical replication.

If you take no action, we will allow blocked cutovers to proceed after the grace period. However, you risk downstream consumers missing updates with no trustworthy position from which to resume.

This blog post isn't a full guide to setting up replication slots; it just highlights how to do it correctly on PlanetScale.

Say you're creating a slot called `analytics_cdc`. The last parameter matters most: it ensures the slot stays in sync with replicas. You must set `failover = true`. Double-check any implementation code from your CDC tooling.

```sql
SELECT pg_create_logical_replication_slot(
  'analytics_cdc',
  'pgoutput',
  false, -- temporary
  false, -- two_phase
  true   -- 👈 failover
);
```

If you already have a replication slot with `failover = false`, you can modify it, just be aware this will hang if the slot is already being consumed. In a separate session you'll have to terminate the consumer to apply this change.

```sql
ALTER_REPLICATION_SLOT analytics_cdc (FAILOVER true);
```

If you haven't already updated your database configuration for replication slots, you should soon receive an email from PlanetScale notifying you that changes are required, and you'll see a new banner in the dashboard.

Setting `failover = true` in the replication slot makes it sync to replicas.

Additionally, you'll need to set two Postgres settings to `on` in your parameters configuration.

- `hot_standby_feedback = 'on'` keeps that bookmark copy valid so it can be used after promotion
- `sync_replication_slots = 'on'` instructs Postgres to copy slot state to replicas

This is a one-time operation that will cover all replication slots.

With this done, your replicas and their replication slots are cutover-ready.

### [Why are these parameters off by default?](https://planetscale.com/blog/how-to-make-a-replication-slot-survive-a-cutover#why-are-these-parameters-off-by-default)

Setting these two parameters to `off` is the perfect default for a database with no CDC consumers. Some folks believe they should be `on` by default. Since you're using logical replication slots, you need both on, but it's worth knowing the consequences.

- `hot_standby_feedback` determines if a replica tells a primary which old rows it is still reading. The `off` default means a replica cannot pin the primary's vacuum horizon, while `on` deliberately pins that vacuum for correctness, but at the cost of adding bloat to the primary. Be aware, long-running transactions against replicas can cause problems with this enabled.
- `sync_replication_slots` is the worker that copies slot state onto replicas. Turn it `on` without `hot_standby_feedback` and the copy can be invalidated the first time vacuum runs past the slot's horizon.

Most databases never create a logical slot, so Postgres' defaults assume you're better off without the extra work that these introduce.

## [Conclusion](https://planetscale.com/blog/how-to-make-a-replication-slot-survive-a-cutover#conclusion)

While Postgres understands high-availability architecture, its default behavior doesn't have downstream applications' best interests in mind. The combination of what Postgres *can* do and what PlanetScale *lets you* do saves you from finding that out the hard way.
