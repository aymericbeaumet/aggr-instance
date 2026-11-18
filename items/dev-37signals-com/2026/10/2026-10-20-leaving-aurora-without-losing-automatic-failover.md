---
title: Leaving Aurora without losing automatic failover
link: https://dev.37signals.com/leaving-aurora-without-losing-automatic-failover/
source: dev-37signals-com
published: 2026-10-20T17:00:00Z
updated: 2026-10-20T17:00:00Z
first_seen: 2026-11-18T09:02:33.277923620Z
authors:
- Paul S.
summary: How we replaced Amazon Aurora in our data centers without losing automated failovers.
content: extracted
html: 2026-10-20-leaving-aurora-without-losing-automatic-failover.html
remote_preview:
  url: https://dev.37signals.com/assets/images/opengraph/leaving-aurora-without-losing-automatic-failover.png
  alt: 37signals Dev
extra:
  thumbnail: https://dev.37signals.com/assets/images/opengraph/leaving-aurora-without-losing-automatic-failover.png
---

How we replaced Amazon Aurora in our data centers without losing automated failovers.

Before we [left the cloud](https://basecamp.com/cloud-exit), HEY and most of our smaller apps kept their MySQL databases on Amazon Aurora. Aurora was one of our biggest bills: we spent **$473,196** on RDS in 2022, and HEY alone was [$355,950 of that](https://dev.37signals.com/our-cloud-spend-in-2022/).

Moving the data was the easy part. Basecamp has run MySQL on our own hardware for decades. The hard part was failover. In Aurora, when a writer dies, a replica takes over and the app reconnects. Nobody has to do it by hand. We didn’t want to give that up.

Today every production MySQL cluster we run fails over on its own, using [Orchestrator](https://github.com/percona/orchestrator), [Consul](https://www.consul.io/) and the F5 load balancers we already had. Here’s how it works and what we learned along the way.

* * *

## Why we left Aurora

Cost was the first reason, but not the only one:

- **Forced upgrades.** In early 2023 AWS announced the end of Aurora MySQL 2.10 and scheduled automatic upgrades for every cluster still on it. We asked for an extension, since we were already moving off. The answer was no. The last round of upgrades had cost HEY 3.5 minutes of downtime.
- **Bugs we couldn’t fix.** In March 2022 the HEY writer crashed and Aurora failed it over. Later we found a bug in Aurora Global Database that had kept up to 13 TB of binlogs on the HEY primary for months.

The one thing we wanted to keep was automatic failover. As [Eron](https://dev.37signals.com/author/eron/) put it when we planned the work in 2022: “We’ve come to appreciate automated cluster failover management in AWS and we don’t want to lose that in our move back to our own hardware.”

* * *

## What we wanted

Matt Kent and I started the project in November 2022 with 4 goals:

1. Fail over automatically, with no manual step.
2. Replace our homegrown role-swap scripts, which needed a person at a keyboard.
3. Use tooling the community maintains, not something we’d have to write from scratch.
4. Only add complexity that pays for itself.

We looked at a lot of options and passed on most of them:

- **Pacemaker, Corosync and MHA.** Too many moving parts.
- **Galera, Group Replication and NDB.** We didn’t want synchronous or semi-synchronous replication across data centers.
- **Commercial tools** like ClusterControl, MaxScale and Tungsten. Per-node pricing or licenses we didn’t like, for problems we could solve ourselves.
- **ProxySQL and HAProxy** in front of MySQL. Another fleet of proxies to run, when we already had F5s in front of everything.

We landed on Orchestrator for topology management and failover, Consul as the source of truth for “who’s the primary”, and our existing F5s to route traffic. Orchestrator was the biggest risk. Its upstream was barely maintained, so I read through the code before we committed to it. It was small enough and clear enough that we were happy to own it if we had to. Since then, the original repo has been archived and [Percona maintains Orchestrator](https://github.com/percona/orchestrator). We run [our own fork](https://github.com/basecamp/orchestrator) of Percona’s.

* * *

## How it fits together

We run MySQL in 2 main data centers, Chicago and Ashburn. Each cluster has 1 primary and a few replicas spread across both. The big clusters also keep a replica in Amsterdam and San Jose, close to our app servers there. Those replicas serve reads and never get promoted.

### Orchestrator and Consul

Orchestrator watches every MySQL server, knows the replication topology, and decides when and where to fail over. Consul stores the answer.

We run them together on the same 5 VMs per Orchestrator cluster: 2 in Chicago, 2 in Ashburn and 1 tiebreaker in a third site. Both use Raft. Putting them on the same machines means they share the same voters, so a site that gets cut off loses quorum for both at once. Orchestrator uses SQLite as its backend, so it doesn’t depend on a MySQL database of its own. A shared MySQL backend for the tool that fails over MySQL was a non-starter.

The tiebreaker only votes. If the link between Chicago and Ashburn goes down, the side that can still reach the tiebreaker keeps quorum. A cron job moves Raft leadership off the tiebreaker if it ever lands there. The tiebreaker started out in AWS. Today it runs in one of our smaller sites: Amsterdam for HEY and 37signals ID, San Jose for Basecamp and the shared cluster.

We have 4 of these Orchestrator clusters: one each for HEY, Basecamp and 37signals ID, and a shared one for everything else. They manage 23 production MySQL clusters.

### Finding the primary

Apps don’t know any of this exists. They connect to 2 addresses per cluster on the F5: one for writes and one for reads. Nothing about a failover changes those addresses.

Behind them:

1. After a failover, Orchestrator writes the new primary to Consul’s key-value store.
2. A job on the Orchestrator leader writes the list of healthy replicas every 15 seconds. It drops any replica that’s lagging, hasn’t been seen in 15 seconds, or that we’ve taken out for maintenance.
3. [consul-template](https://github.com/hashicorp/consul-template) on every MySQL host watches those keys and writes them to local files.
4. A small nginx Lua handler on each host answers `/<cluster>/primary` and `/<cluster>/replica` with a 200 or a 503.
5. The F5 health-checks those endpoints. Only the primary passes the write check. Replicas pass the read check.

When a host fails its check, the F5 resets its connections, and the app reconnects to whichever server is now healthy. Reads prefer replicas in the same data center. If every replica is down, an iRule sends reads to the primary.

The nginx handler fails closed. If it can’t get a quorum read from Consul, it returns a 503, so a host that’s cut off from the cluster drops out of the pool. The one exception is a Consul read that times out: the handler serves its last answer for up to 30 seconds, so a short Consul leader election doesn’t empty the pools. More on why below.

### Failing over

Orchestrator polls every server every 5 seconds, from every node in the Raft cluster. When the primary dies:

1. Orchestrator runs a pre-failover hook. The hook tells Dash, our internal ops app, which pages whoever is on call and posts in our Ops Campfire room. For automatic failovers, the hook also asks Dash whether this cluster is allowed to fail over right now, so we can turn it off per cluster during maintenance.
2. Orchestrator promotes the best replica, using GTID to repoint the others, and sets `read_only=0` on the new primary.
3. A post-failover hook refreshes the replica list in Consul and tells Dash it’s done.

Here’s the part of our Orchestrator config that controls this:

```json
{
  "BackendDB": "sqlite",
  "RaftEnabled": true,
  "InstancePollSeconds": 5,
  "RecoverMasterClusterFilters": ["*"],
  "ApplyMySQLPromotionAfterMasterFailover": true,
  "DetachLostReplicasAfterMasterFailover": true,
  "RecoveryPeriodBlockSeconds": 900,
  "FailureDetectionPeriodBlockMinutes": 60,
  "RecoveryBlockCrossDatacenterFailovers": true,
  "KVClusterMasterPrefix": "mysql/primary",
  "ReplicationLagQuery": "SELECT timestampdiff(SECOND, ts, now()) FROM percona.heartbeat ORDER BY ts DESC LIMIT 1",
  "DetectPromotionRuleQuery": "SELECT IFNULL(value, '') FROM 37s.metadata WHERE `key` = 'promotion_rule' AND `server` = @@hostname"
}
```

A few settings worth explaining:

- **`RecoveryPeriodBlockSeconds: 900`** allows 1 automatic failover per cluster every 15 minutes, unless someone acknowledges the last one. A flapping primary gets one failover and a page, not a loop.
- **`DetectPromotionRuleQuery`** reads promotion rules from a small metadata table on each server, instead of hardcoding them in Orchestrator. Dash owns those rules and syncs them every minute. Replicas in the active data center are preferred, the other main data center is neutral, and the outpost replicas are never promoted.
- **`RecoveryBlockCrossDatacenterFailovers`** isn’t in upstream Orchestrator. We added it to our fork. Automatic failovers stay inside the data center the primary was in. Moving a primary to the other data center is a person’s call.

When we tested the proof of concept under load (400 client threads doing inserts), Orchestrator noticed a dead primary in about 3 seconds and updated Consul in about 1 more. Every client was writing again 12 seconds after we killed the primary. A planned switchover took 13 to 20 seconds, depending on how often the F5 checked.

In production, a failover is over within seconds. In a recent planned switchover, we moved the primaries of 2 production clusters ahead of an upgrade. Orchestrator had each new primary in place 3 seconds after it started.

* * *

## Replication and backups

We run [Percona Server](https://www.percona.com/mysql/software/percona-server-for-mysql) 8.0 on most clusters and 8.4 on the newest ones.

- **GTID** on everywhere. It’s what lets Orchestrator repoint replicas safely.
- **Asynchronous replication.** No semi-sync. We accept that a hard crash can lose the last few transactions that hadn’t reached a replica, in exchange for not paying cross-data-center latency on every commit.
- **Parallel replication** with `WRITESET` dependency tracking, so replicas keep up with busy primaries.
- **Durability by role.** A cron job checks whether a server is read-only and sets its flush settings to match. The primary flushes on every commit. Replicas relax that.
- **[pt-heartbeat](https://docs.percona.com/percona-toolkit/pt-heartbeat.html)** writes a timestamp every 10 milliseconds. Orchestrator uses it to measure lag, and so do our alerts.
- **New replicas** come from MySQL’s [clone plugin](https://dev.mysql.com/doc/refman/8.0/en/clone-plugin.html), not from backups.

Backups run nightly with [Percona XtraBackup](https://www.percona.com/mysql/software/percona-xtrabackup) on a replica in the data center that isn’t serving writes, so they never touch the primary. They go to our [Pure Storage](https://dev.37signals.com/pure-storage-monitoring/) arrays. Binlogs get copied every 15 minutes, so we can restore to a point in time. A monthly refresh of our staging databases from production backups keeps the restore path tested.

* * *

## Moving off Aurora

Matt Kent led the migrations. Every app moved the same way:

1. Restore an Aurora snapshot to a temporary instance and export it.
2. Load the export on our own servers.
3. Replicate from the live Aurora cluster until the new servers caught up.
4. In a short maintenance window, set Aurora read-only, wait until our servers had applied the last of its writes, stop replication, make our primary writable, and point the app at it.

Writeboard went first, in February 2023. HEY was the big one: it moved from Aurora MySQL 5.7 to our own MySQL 8 servers on June 8 and 9, 2023, and we deleted the Aurora clusters 4 days later. We had AWS Direct Connect links into AWS, and Ashburn is about a millisecond from us-east-1. Those links bridged the gap during the cutover and let us move HEY one piece at a time. By October 2023 our RDS bill was close to zero. We spent **$163,172** on RDS that year, down from $473,196 in 2022.

Getting off Aurora and getting onto Orchestrator were separate steps. Apps first moved to plain MySQL on our hardware. The shared Orchestrator cluster went live on June 20, 2023, and the smaller apps moved onto it over the next 3 weeks. HEY got automatic failover in September 2023. Basecamp, which was never on Aurora, moved from MariaDB to MySQL 8 and onto its own Orchestrator cluster in May 2024.

HEY got faster, too. Compared to AWS, web requests are 20 to 25% faster on average, time spent in the database is about 40% lower, and the slowest queries are about 6 times faster.

* * *

## What went wrong

Some things broke along the way:

- **Consul is now critical.** In January and March 2025, Consul leader elections on the shared cluster briefly took the primary out of the F5 pools for every app on it. Basecamp went down with them because it depends on 37signals ID, which lived there. 37signals ID got its own Orchestrator cluster, and we changed the nginx handler to fall back to its last answer for 30 seconds when a Consul read times out.
- **“Topology too complex.”** In December 2024 a HEY failover stopped partway because Orchestrator couldn’t reach one of the Amsterdam replicas, and we repointed it by hand. Orchestrator refuses to guess when it can’t see the whole topology. That’s the right call, but you need a runbook for it.
- **A recovery that never let go.** Once, Orchestrator held a lock on a recovery and never released it, which blocked the next one. We now alert on blocked and unacknowledged recoveries.
- **The UI is a footgun.** You can drag servers around in Orchestrator’s web UI and build a topology you didn’t mean to, like ending up with co-masters. We do failovers through runbooks and chat commands instead.

* * *

## The app follows the database

Failing over MySQL is half the job. If HEY’s primary moves from Ashburn to Chicago, every write now crosses data centers, and Redis is still in Ashburn.

So we built what we call Failover v2: when the primary moves to the other data center, the app and Redis follow it on their own. We’ll cover how in a future post.

* * *

## Where we are now

Orchestrator has been failing over our databases since 2023, and every production MySQL cluster we run is on it. Nobody has to wake up to run a role-swap script.

If you’re thinking about leaving a managed database, the data is the easy part. Decide early how your apps will find the primary after a failover, and keep that part as boring as you can. Ours is the F5 health-checking a small nginx endpoint on every MySQL host.
