---
title: The hidden latency inside your database
link: https://planetscale.com/blog/the-hidden-latency-inside-your-database
source: planetscale-com
published: 2026-11-03T00:00:00Z
updated: 2026-11-03T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Simeon Griggs
labels:
- engineering
summary: Network-attached storage is a fine choice and a bad default.
content: extracted
html: 2026-11-03-the-hidden-latency-inside-your-database.html
remote_preview:
  url: https://planetscale.com/assets/the-hidden-latency-inside-your-database-social-CRNekkCo.png
---

Network-attached storage is a fine choice and a bad default.

Application performance 101 says you want to move your application as close as possible to your database. The shorter that distance across the network, the faster reads and writes move between them.

But did you know there is network latency *inside* your database cluster?

Most commonly, cloud-hosted databases have **network-attached** storage: a disk on a separate server, connected over the network, with impressively small, but still measurable, latency.

Alternatively, it's possible to have **locally attached** storage: a disk inside the same machine as the CPU, connected directly to the motherboard.

Network-attached storage powers an extraordinarily high volume of data throughput with flexibility and durability. But local disks are closer, closer is faster, and faster is better.

## [How'd we get here](https://planetscale.com/blog/the-hidden-latency-inside-your-database#howd-we-get-here)

The work performed by the device you're reading this blog post on has two separate yet equally important components: a processor to perform work, and storage to save it. *Dun dun*.

It's also extremely likely these two are contained in one piece of hardware, like a smartphone or laptop. Computers have been like this forever.

Internet infrastructure started this way, too. The original database servers, like Sun's SPARC servers, had their own disks inside the chassis or in a cabinet next to it, connected by SCSI and later SAS cables. Storage and compute were in the same physical hardware.

### [Separating compute and storage](https://planetscale.com/blog/the-hidden-latency-inside-your-database#separating-compute-and-storage)

But a single entity for storage is not *durable*. RAID (Redundant Array of Inexpensive Disks) was popularized in the late 1980s. With multiple storage disks being kept in sync, should one die, it could be swapped out for another. The system relied on the still-active disks while the fresh one caught up.

Into the 1990s and early 2000s, enterprises put storage in dedicated arrays. A SAN (Storage Area Network) connected storage to compute over a network. But these were tremendously expensive and often maintained by the enterprise itself.

### [The cloud](https://planetscale.com/blog/the-hidden-latency-inside-your-database#the-cloud)

a.k.a. *renting someone else's computer*.

(For simplicity, we're focusing on AWS offerings in this post, but PlanetScale also runs on Google Cloud Platform and Azure.)

The most notable cloud storage service is AWS S3 (Simple Storage Service). It is a key-value store for objects. You can't attach it as a block device or modify subsets of objects in place.

Enter AWS EBS (Elastic Block Store), which mounts like a disk and can be provisioned on demand. EBS solves for typical storage use cases like that of a database at a lower cost than the previous generations' enterprise SANs.

Dedicated storage like EBS is durable, in that data is replicated across multiple disks within the same availability zone. Losing any one disk within that availability zone won't lead to data loss.

Note that for this reason PlanetScale replicates data *across* availability zones, as the failure of a single zone would be a data unavailability or loss event.

It's also flexible: you aren't constrained to a single volume's storage size as adding capacity is as simple as resizing the same volume.

### [Disks got fast](https://planetscale.com/blog/the-hidden-latency-inside-your-database#disks-got-fast)

At launch in 2008, EBS used spinning disks like the enterprise servers of the past. And so throughout the history of network-attached storage, the speed of the spinning disk was often of more significance than the speed of data transfer over the network. The average random read on an HDD is 10-20ms, whereas a network round trip in the same data-center is usually 1 millisecond or less.

In 2012, EBS began swapping out spinning disks for SSDs. Disk operations got ~100x faster and suddenly network latency was the biggest bottleneck to performance.

Initially the swap to SSDs didn't yield the sort of free speed increase you might expect.

> ... it certainly did address the problems that came from the mechanics of hard drives. But what surprised us was that the system didn't improve nearly as much as we had hoped ...

— [Continuous reinvention: A brief history of block storage at AWS](https://www.allthingsdistributed.com/2024/08/continuous-reinvention-a-brief-history-of-block-storage-at-aws.html), Marc Olson, All Things Distributed

To address this, engineers at AWS created new custom hardware and a unique network transfer protocol (SRD). Network latency improved.

So, over the brief history of data centers, storage and compute got separated when storage was slower than the network and were then penalized for that once the opposite was true.

You can continue to try and solve problems, or you can eliminate them.

## [Hops hiding in plain sight](https://planetscale.com/blog/the-hidden-latency-inside-your-database#hops-hiding-in-plain-sight)

Back to our original Performance 101 lesson. Moving your application closer to your database makes as much sense as moving your database's compute closer to its storage.

Network latency is a long-established expectation of working with remote servers and cloud storage, but it doesn't have to be.

In a typical cloud-hosted database scenario, you buy an EC2 instance for compute to perform work and attach it to an EBS volume for storage.

The EBS volume shows up in the operating system as if it were an attached storage device. On current-generation instances, a dedicated Nitro card turns every read and write into a network request.

For every non-Metal PlanetScale database, this is what you have and it works just fine.

However, some EC2 instance types also come with local NVMe disks, physically attached to the host server. These aren't EBS volumes. As they're locally attached, there's no network hop for reads and writes.

If all you wanted was speed, it *just makes sense* to put your database and its storage on a self-contained instance.

But if this is so obvious, why is it so rarely offered? Because without careful management the risk is too high. Locally attached storage lives and dies with compute. Its size is rigid and its data is not automatically replicated.

It's no good being fast if you're not also flexible, reliable, and durable. That's what we solved with PlanetScale Metal.

## [Durable local disks](https://planetscale.com/blog/the-hidden-latency-inside-your-database#durable-local-disks)

So, for a faster database you want to eliminate the network hop between storage and compute. But you can't afford to lose data, so storage still has to be durable.

Get you somebody who can do both.

Firstly, every Metal database cluster has a primary and at least two replicas. Each runs on its own VM with its own local NVMe drive, spread across distinct availability zones.

Each replica is treated as a potential primary that can be failed over to. To ensure the integrity and suitability of these replicas and the durability of this storage, commits must be acknowledged on at least one replica before the primary sends acknowledgement to the client.

You might ask, doesn't this introduce a network hop into every commit? Yes, but it happens only once per commit. A Metal database confirming a write landed in a replica in another availability zone ensures durability. EBS-backed databases require network hops for every disk operation, including something as simple as a cache miss in a read query. It's also likely the majority of your queries are reads, and reads on Metal will never cross the network.

With each replica treated like a primary in waiting, should the primary fail, a replica is immediately promoted. On Vitess, queries are buffered during the switch. On Postgres, queries through PgBouncer are buffered and then directed to the new primary.

PlanetScale Metal is a mature alternative to network-attached storage and has already powered trillions of queries across petabytes of data.

## [Network vs local vs your budget](https://planetscale.com/blog/the-hidden-latency-inside-your-database#network-vs-local-vs-your-budget)

Incredibly, we haven't yet touched on IOPS (Input/Output Operations Per Second). That is, the throughput limits of your database. On EBS your IOPS are limited, defaulting to 3,000. This makes sense: the transfer of data across the network between the two services has to be curbed to some number or else one noisy neighbor risks flooding the network.

But with locally attached disks this isn't a problem. Unlike network-attached, there's no artificial cap on IOPS.

In our [Postgres 17 vs 18 benchmarks](https://planetscale.com/blog/benchmarking-postgres-17-vs-18), an `r7i` with a 700 GB `io2` volume provisioned for 16,000 IOPS cost $1,513.82 a month on demand. An `i7i` with a 1.8 TB local NVMe drive, capable of around 300,000 IOPS, cost $551.15.

At launch, PlanetScale Metal only offered large compute and storage sizes. But with the launch of $50 Metal in December 2025, you can provision virtual machines as small as 1/8 vCPU with 1 GB of memory and 10 GB NVMe storage. vCPU and memory sizes go up from there, but storage can be individually configured in different sizes.

So while I spent much of this article telling you how inflexible local disks can be, we've done the hard work to make using them easy and reliable.

More incredible than the late mention of IOPS in this article is finally getting around to RAM. Volatile storage that also lives and dies with compute, but which can hold the most frequently and recently requested data as a cache.

Our own documentation for PlanetScale Metal mentions that you simply might not need Metal if the majority of your read workloads fit in RAM and you have a low volume of writes. Reads stay fast because they're read from memory, not storage, and so never make that annoyingly slow network hop to EBS.

And so truthfully, depending on your workload, network-attached storage might be just fine, and you can use both on PlanetScale.

If your most frequently requested data is too large or changes too often to fit in RAM, as in frequent cache misses, you want to eliminate those network-storage round-trips. Retrieving data from locally attached storage is closer to RAM speeds than a network round-trip.

*PlanetScale Insights can reveal the cache hit ratio of your queries. Also available to your agents via the MCP server.*

Or if you're hitting those expensive IOPS limits on EBS, it's likely your workload will not only be faster on Metal, but cheaper, too.

Or if your CPU usage is low but latency is high, you're being throttled by disk reads and paying for compute you can't use. In [some cases](https://planetscale.com/docs/metal/metal-performance) we've seen CPU usage go from 20-25% to 80% or more after moving to Metal.

Whether you want network-attached or locally attached storage, you can get either from PlanetScale. But if you've not considered the benefits of locally attached storage before, there's never been a better time to [get in touch](https://planetscale.com/contact).
