---
title: Twelve years of distributed SQLite
link: https://philipotoole.com/twelve-years-of-distributed-sqlite/
source: hnrss-org
published: 2026-10-24T01:16:58Z
updated: 2026-10-24T01:16:58Z
first_seen: 2026-10-24T09:25:49.691482789Z
authors:
- otoolep
labels:
- database
- distributed systems
- go
- open-source
- programming
- rqlite
- sqlite
- uncategorized
content: extracted
html: 2026-10-24-twelve-years-of-distributed-sqlite.html
remote_preview:
  url: https://philipotoole.com/wp-content/uploads/2014/09/rqlite-design.png
---

![](https://philipotoole.com/wp-content/uploads/2014/09/rqlite-design.png)In September 2014 I published a [short post announcing](https://philipotoole.com/replicating-sqlite-using-raft-consensus/) [rqlite](https://rqlite.io). The idea was simple: take [SQLite](https://sqlite.org/), put a [Raft log](https://raft.github.io) in front of it, and replicate every write to a cluster so that the loss of a node did not mean the loss of the data. The [source went on GitHub](https://github.com/rqlite/rqlite) that same weekend. As far as I can tell, that was the first open-source system to combine Raft and SQLite. Nobody else was doing it, and for several years nobody else tried.

Today “distributed SQLite” is a category with a dozen projects in it, a few of them venture-funded, two of them acquired. Since the people now writing about the category tend to start the story around 2021, I thought I should write down the version that starts in 2014. I was there for all of it.

## 2014: rqlite

The need was concrete. A clustered system [I worked on at jut.io](https://philipotoole.com/resume/) needed a small amount of relational storage, and the question of which node should hold it had no good answer. Replicating SQLite with a consensus protocol solved it. We went another way at work, but [I kept building rqlite](https://philipotoole.com/tag/rqlite/) because the problem was general and the solution was small. The first version used [goraft](https://github.com/goraft), the same library [InfluxDB](https://github.com/influxdata/influxdb) and [etcd](https://etcd.io/) used at the time (I subsequently joined InfluxDB as a core developer around the same time). In early 2016 I [replaced it with](https://github.com/rqlite/rqlite/blob/master/CHANGELOG.md#20-april-5th-2016) [Hashicorp’s Raft implementation](https://github.com/hashicorp/raft) which rqlite still uses.

The design decision that mattered most was made then and never changed. rqlite is a [CP system](https://www.scylladb.com/glossary/cap-theorem/). Writes must go through the leader, a majority must acknowledge, and a partitioned minority refuses writes. Everything else, the [HTTP API](https://rqlite.io/docs/api/api/), the [read-consistency levels](https://rqlite.io/docs/api/read-consistency/), the [snapshotting](https://youtu.be/8XbxQ1Epi5w?t=493), followed from that.

## Before 2014, and alongside it

Two things deserve mention for honesty’s sake. [Expensify](https://www.expensify.com/) had been replicating SQLite inside its own infrastructure for years with a [Paxos](https://en.wikipedia.org/wiki/Paxos_\(computer_science\))-style protocol; they released it as [Bedrock](https://bedrockdb.com/) in [2016](https://dbdb.io/db/bedrock). And ActorDB, an Erlang system that gave each “actor” its own SQLite database replicated by Raft, [appeared around the same time as rqlite](https://dbdb.io/db/actordb). Neither was widely adopted outside its origin, but both show the idea was in the air. What rqlite did was make it a standalone, general-purpose, open-source thing anyone could run.

## 2017: dqlite

[Canonical](https://canonical.com/) built [dqlite](https://canonical.com/dqlite) to get [LXD](https://canonical.com/lxd) and later MicroK8s an embedded replicated store without running etcd. [Mark Shuttleworth](https://en.wikipedia.org/wiki/Mark_Shuttleworth) actually called me once, and we talked, hoping that rqlite could be made to work, but it never quite fit. dqlite [began in Go](https://www.reddit.com/r/golang/comments/8a8h8y/dqlite_distributed_sqlite_for_go_applications/) and was rewritten in C with its own Raft library. It is the project most similar to rqlite in intent, and the main difference is of one of packaging: dqlite is a library you embed, rqlite is a server you run. Canonical’s need was a database inside their products: mine was a database beside anyone’s product.

## 2021: Litestream, and a change of approach

[Ben Johnson](https://github.com/benbjohnson) (whom I worked with at InfluxDB) released [Litestream](https://github.com/benbjohnson/litestream) in [early 2021](https://litestream.io/blog/why-i-built-litestream/). Litestream does not use consensus at all. It watches the SQLite [WAL](https://www.sqlite.org/wal.html) and streams it to [S3](https://aws.amazon.com/s3/), giving you disaster recovery for a single-node SQLite database at almost no cost. Ben’s argument was that most applications do not need a cluster; they use one machine and need a way to recover if it dies. He was right about a great many applications, and the “SQLite is enough” movement of the next few years, with blog posts and conference talks, owes a lot to that argument.

Litestream and rqlite answer different questions. Litestream: what if the machine dies? rqlite: what if the machine dies and I cannot wait to restore? The first question is more common, the second is the one people have when they come to rqlite.

[Fly.io](https://fly.io/) hired Ben in 2022 and shipped [LiteFS](https://docs.fly.io/litefs), which replicates SQLite at filesystem level with leases from [Consul](https://developer.hashicorp.com/consul). It was the first attempt to make replicated SQLite a platform feature rather than a thing you deploy. And in a final twist, Litestream [inspired](https://philipotoole.com/rqlite-8-0-large-dataset-support-and-core-feature-upgrades/) rqlite’s v8.0 approach to handling the SQLite WAL.

## 2022: forks and extensions

Two projects in 2022 took approaches that neither rqlite nor Litestream had. [cr-sqlite](https://github.com/vlcn-io/cr-sqlite) added [CRDTs](https://en.wikipedia.org/wiki/Conflict-free_replicated_data_type) to SQLite as an extension, so that multiple writers could diverge and merge without a leader. That is the AP corner of the design space, and it suits local-first applications where rqlite’s CP guarantees would be a hindrance.

[libSQL](https://github.com/tursodatabase/libsql) forked SQLite itself. ChiselStrike, later [Turso](https://turso.tech/), wanted to change things inside the engine that the SQLite project, which does not accept outside contributions, and would not change. On top of the fork they built a hosted service with replicas at the edge. Turso raised venture money on that, and in 2026 [Supabase acquired them](https://turso.tech/blog/turso-is-joining-supabase).

Around the same time, [SQLite Cloud](https://www.sqlite.ai/cloud) appeared, offering hosted SQLite replicated with Raft. It is the project closest to rqlite’s original design, wrapped in a cloud service.

## 2022 onward: the platforms

[Cloudflare](https://www.cloudflare.com/) announced [D1](https://developers.cloudflare.com/d1/) in 2022, SQLite as a serverless database inside their edge network, and later put SQLite storage inside [Durable Objects](https://developers.cloudflare.com/durable-objects/concepts/what-are-durable-objects/). This is replicated SQLite as a managed product from a company that does not need to tell you how it works. Turso’s service is the same shape. The category had gone from “a thing you run” to “a thing you buy.”

## What the twelve years show

Three things, I think.

The core was right. SQLite as a storage engine, with replication added on the outside, has been reinvented by at least eight teams with different needs and different protocols. Nobody has found a reason to stop doing it.

The design space has three corners, and they appeared in order. Consensus first (rqlite, dqlite): one leader, strong consistency, a cluster. Streaming second (Litestream): one writer, cheap recovery, no cluster. Merge third (cr-sqlite, and parts of the local-first movement): many writers, eventual consistency. Each is correct for a different question, and most of the arguments are arguments between their users about which question is being asked.

And the money went to the platforms. rqlite, dqlite, and Litestream are open source and free. The projects that raised the capital or got acquired are the ones that turned replicated SQLite into a service with a bill. That is the usual fate of infrastructure ideas, and I do not mind it. But it is worth recording that the idea was open source for eight years before anyone sold it.

[rqlite](https://rqlite.io/) is still here. It does the same thing it did in 2014, a good deal better, and it is still the thing you reach for when you want a fault-tolerant relational database you can run yourself, understand fully, and not pay anyone for. Twelve years in, I am content with that position in the story.
