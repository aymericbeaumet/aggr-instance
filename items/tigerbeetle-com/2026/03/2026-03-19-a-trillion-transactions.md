---
title: A Trillion Transactions
link: https://tigerbeetle.com/blog/2026-03-19-a-trillion-transactions
source: tigerbeetle-com
published: 2026-03-19T00:00:00Z
updated: 2026-03-19T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Joran Dirk Greef
summary: We processed a trillion transactions to test TigerBeetle’s scale.
content: extracted
html: 2026-03-19-a-trillion-transactions.html
remote_preview:
  url: https://tigerbeetle.com/blog/2026-03-19-a-trillion-transactions/banner-3186611b.webp
---

We [processed a trillion transactions](https://youtu.be/y2_BqkKTbD8) to test TigerBeetle’s scale.

But what is scale? And what does it mean to be scalable?

We often talk about throughput and latency, or adding more machines. But scale isn’t just performance and distribution. Scale is survivability.

Because the hardest part of scale isn’t how much work you can do, when everything works. It’s staying alive, when everything that can break, does. You can’t scale when you’re dead.

So, how do we stay alive? With this definition of scale as survivability, how can we design an architecture from first principles—not for analytics (OLAP), nor general purpose (OLGP)—but for the hardest workload of them all: transaction processing (OLTP)?

Low latency. Write-heavy, Black Friday throughput. Power law contention. Shared hot keys. And coordination through strict serializability. Because you’re in the money making hot path.

Of course, some would relax isolation for performance.

But give me strict serializability or give me death. As the saying goes, “those who would trade a little safety for performance, deserve neither safety nor performance”.

![Graffiti: Give Me Strict Serializability or Give Me Death](https://tigerbeetle.com/blog/2026-03-19-a-trillion-transactions/serializability-dacc0fa3.png)

This is the challenge of transaction processing:

There’s nowhere to hide. It’s live. You have to be correct and fast.

Jim Gray, the Turing Award winner who coined [ACID](https://www.youtube.com/watch?v=lGyMiW6PnKI) and wrote [the book on transaction processing](https://dl.acm.org/doi/10.5555/573304), defined the “[measure of transaction processing power](https://arxiv.org/pdf/cs/0701162)” as debit/credit.

Simple, yet powerful.

Simple: debit one account, and credit another. Powerful because you can express the exchange of anything from one entity to another. The essence of a transaction.

If we trace the family tree of databases, they go back to the banking, brokerage, airline, manufacturing, inventory, ordering and shipping systems that do the equivalent of debit/credit.

And this is what makes OLTP hard, because it’s not just people’s money. It’s all the goods and services. If your OLTP breaks, you don’t just lose your money, but your customer loses their delivery, their flight.

So we’re going to design an architecture for OLTP, and then see how it survives. Even to a trillion transactions.

Why a trillion? Let’s start with the physics.

If each transaction inserts on the order of 128 bytes of primary information (the essential who/what/when/where/why and how much of transaction processing) then a trillion transactions is around 128 TiB, just in storage, before indexing, before replication.

But 128 TiB is the breaking point for many architectures. And this is true whether that data lives on one machine, or a thousand.

For example, most cloud databases promise “unlimited” “infinite” scale. But when we look at the fine print, most of them set hard limits of either 64, 128 or 256 TiB in the docs:

```
“Up to 64 TB”
“grow from 10 GB up to 128 TB”
“256 TiB maximum”
```

It’s rare that any cloud database can scale beyond a few hundred TiB. To be fair, these limits are public, and I want to be clear that knowing the limits and documenting them upfront is a hallmark of great design.

But still, few architectures operate comfortably at petabyte scale. Why is that?

Disks are easy to fill, but recovery from disk and machine failure takes a bit more thought.

For example, to rebuild a failed 128 TiB machine might take 2.9 hours across the network, assuming a 100 gigabit NIC and perfect packet transmission.

Add index reconstruction, and you’re near 6 hours, as your Mean Time To Recovery, or MTTR.

But if your MTTR exceeds your Mean Time Between Failure, you’re in a death spiral. While you’re recovering, another machine fails. The system scales (and fails) faster than it recovers.

Of course, this is a naive example, you can multiplex recovery across machines, but the point is that scalability is hard, because survivability is hard.

Yet without survivability, the system becomes too big to fail, because it’s really too big to recover. And when you can’t recover a system, you no longer own the system. The system owns you.

In other words, the maximum size of a database is dictated not by disk, but by architecture, and whether every algorithm is designed with explicit limits for scale, and, crucially, to recover that scale.

In a paradox of composition, to have unbounded scale, every component must be bounded. You start to move from dynamic allocation to static allocation.

Yet this focus on worst-case behavior is rare where systems use off-the-shelf dependencies. And systems tend to fail to scale in a far simpler way: time.

Let’s estimate that an average general-purpose (OLGP) database can sustain between: 10,000 and 100,000 transactions per second. With strict serializability.

Depending on the rate, a trillion transactions would take us between 115 and 1,157 days.

That’s 3 months to 3 years. If we’re going to design and demo an architecture through a trillion transactions, we don’t want to finish in 2029.

In the last decade, [India’s national payments system](https://www.npci.org.in/product/upi/product-statistics) grew 10,000x, processing tens of billions of transactions per month. There’s almost no transaction database on Earth that can survive this kind of increase in scale.

But this is what happens when economies move to real-time. Everything becomes instant. Smaller. Volumes explode. And not just payments, all sectors.

This is Jevons’ Paradox: efficiency increases consumption. The faster your OLTP, the more transactions you’ll want to process, the faster you’ll need to recover. The need for more scalable transaction processing is not going away.

Of course, none of this is impressive if we look at where the world is going. But the systems we design today will decide whether this future is scalable, survivable, or impossible.

To survive a trillion transactions then, before we build something better, we first need to understand where existing architectures break.

And here, OLTP tends to evolve through seven predictable stages, each stage solving one problem while creating another.

In the beginning was the log. The database is the log and the rest—just an in-memory cache. The log gives order, durability, and recovery from power loss. But the longer the log, the longer the replay.

And so, in stage two, we snapshot the log to accelerate restarts. This works for small state. But a 1 TiB snapshot at 10 GB/s takes 100 seconds, and you can’t scale beyond memory.

Stage three adds an LSM tree storage engine for incremental snapshotting across L1, L2, L3, main memory, and NVMe.

![LSM-Tree Storage Engine](https://tigerbeetle.com/blog/2026-03-19-a-trillion-transactions/lsm-03524b9c.png)

LSMs are beautiful from a systems perspective. They match the memory hierarchy. You get a natural separation of hot and cold data.

But everything is on one machine. If you lose the machine, you need to recover from backup, and as you approach 128 TiB, the time this takes limits the scale you can address.

So what do we do? We add redundancy to mask machine failure.

The fourth stage replicates the log and state across machines, with a replicated state machine architecture. Which I think makes sense if you say it backwards: a machine, with state, that you replicate.

![Replicated State Machine](https://tigerbeetle.com/blog/2026-03-19-a-trillion-transactions/rsm-7c4f748d.png)

The [Viewstamped Replication](https://dspace.mit.edu/handle/1721.1/71763) consensus protocol from MIT, pioneered this approach in 1988 (a year before Paxos, and inspiring Raft years later).

VSR provides split-second recovery to a new primary if the old primary fails, with no durability loss during failover, and no consistency loss, not even temporarily. This is an improvement for availability. You can’t scale when you’re down.

At this stage, with an RSM and VSR, we’re surviving most recovery problems, but if you lose one of the replica machines, you need to recover across the network, and as you scale to 128 TiB, so too MTTR approaches several hours.

If another machine fails while you’re recovering, you start to lose quorum and risk losing the cluster. Again, survivability limits scalability.

Therefore, around at least 10 years ago, we saw a fifth stage emerge, which was to shard across multiple replicated state machines. Now, each machine isn’t storing all the state, but a shard of the state.

![Horizontal Scaling](https://tigerbeetle.com/blog/2026-03-19-a-trillion-transactions/horizontalscaling-d2c2865f.png)

This seems like an obvious step. And for a while, it was.

Horizontal scaling! Infinite scalability! But there’s a problem. And it’s fundamental.

Sharding takes what we call the four primary colors of computer science: network, storage, memory and compute, couples them, and scales them all the same way, with the same horizontal brush stroke.

This works for storage. But for compute you need to transact across keys. And so sharding destroys what OLTP needs most: cache locality.

Imagine taking your CPU’s L1 cache, splitting your L1 cache across machines, and then making your CPU wait on the network.

Forget cache misses.

Contention kills throughput because you hit [Amdahl’s Law](https://en.wikipedia.org/wiki/Amdahl%27s_law) on every transaction: throughput is always limited by the serialized portion of the workload.

Even if you subdivide your keys on the write path, to split your counters or balances, it’s a hack, because you have to join them on the read path if you want to be able to execute any meaningful business logic. You can’t shard your way around strict serializability.

Of course, there will be people who think they can outspend Gene Amdahl.

And so we spun up a horizontally scaled cluster last year, that would have cost a million dollars a year to run, as much as a McLaren, and we found that for OLTP, [with as little as 10% contention, even 1% or 0% contention](https://www.youtube.com/watch?v=yKgfk8lTQuE), you would be better off running single node open source Postgres, which also costs less.

So how do we scale compute and storage? Clearly, we can’t couple them. The industry’s latest answer? Go the other extreme.

Decouple everything. Separate storage from compute and push everything to object storage, to run queries directly against object storage.

![Separation of Storage and Compute](https://tigerbeetle.com/blog/2026-03-19-a-trillion-transactions/storagecompute-890dd2cf.png)

This solves storage scaling. Object storage has no practical limits.

However, object storage has high latency. Remember our workload? You can’t serve transactions from object storage. It’s one thing to partition your L1 cache across the network, and another to replace your memory hierarchy with cold storage.

We need horizontal storage but vertical compute, so we can keep hot data close to the CPU, cold data cheap, and recovery time bounded. In other words, an LSM tree, or an iceberg, that doesn’t submerge all the way.

This is diagonal scaling. To put a new name on a classic architecture, the memory hierarchy.

Keep your hot transactions, your working set, in a non-sharded replicated state machine. This scales compute vertically, where you need microsecond performance: bigger machines, faster chips, more memory, faster NVMe.

Then tier 90% of your historical transactions, the lower levels of your LSM tree—to object storage. This scales storage horizontally, where you need cost capacity.

![Diagonal Scaling](https://tigerbeetle.com/blog/2026-03-19-a-trillion-transactions/diagonalscaling-310dddd5.png)

But the insight is that with diagonal scaling, now, in terms of survivability, you don’t need to rebuild 128 TiB across the network, because your replicated state machine is no longer that big. You can cap it at say 16 TiB, to bound recovery time, while scaling out indefinitely against object storage.

You know, someone will object that you can prune historical data into a separate system. But if we think for a second, what’s the difference?

Diagonal scaling is simple: RSM + LSM + object storage. And again, it’s nothing new. But most implementations tier for cost, not recovery.

We’ve decoupled recovery time from total storage size, without sacrificing strict serializability or the memory hierarchy. The replicated state machine stays small and fast. The object storage scales infinitely.

This is how you survive a trillion transactions. In theory.

Now, let’s see it in practice. And to do this, we took TigerBeetle, the financial transactions database we designed for OLTP (and only OLTP).

Where general-purpose and analytics databases design for strings, TigerBeetle designs instead for integers and counting. It’s obvious in hindsight, but the reason that TigerBeetle is not a string database, is because transaction processing in the Jim Gray tradition is debit/credit.

TigerBeetle has a replicated state machine architecture, and we extended TigerBeetle’s LSM engine with a connector to object storage for the lower levels, for petabyte scale, quick recovery, and strict serializability.

Then we spun up a TigerBeetle cluster with local NVMe plus object storage with half a petabyte of flash.

And we started sending transactions in…

(But now, you will need to [watch the last five minutes of the talk.](https://youtu.be/y2_BqkKTbD8?t=999))
