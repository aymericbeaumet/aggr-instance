---
title: 'Aurora DSQL: Scalable, Multi-Region OLTP'
link: http://brooker.co.za/blog/2026/07/19/dsql-paper.html
source: brooker-co-za
published: 2026-07-19T00:00:00Z
updated: 2026-07-19T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Marc Brooker
summary: 'Aurora DSQL: Scalable, Multi-Region OLTP A paper! Our new paper, Aurora DSQL: Scalable, Multi-Region OLTP, is now available on Arxiv. I’m excited about this one: it’s a fully end-to-end look at how Aurora DSQL works, from query processing, to transactions, to replication, to the control plane. We’ve shared most of this content before in other forms, on this blog, on Marc Bowes’ Blog, Werner’s Blog, in talks, and on the AWS blog. But this version covers all the ground, all in one place. You should read it. Or check out the interactive transaction simulator to get a feel for how it works. Some highlights Our overall goal was to build a relational database system that simplifies the work of application building and operations, freeing builders from worrying about scale, reliability, durability, and even multi-region fault tolerance. A database of first resort, which is simple and easy to adopt at low scale, and grows with the application, without adding complexity. I’ve written about this argument before (in DSQL: Simplifying Architectures), and it remains the thing that excites me most about the Aurora DSQL product. I believe we’ve made it much easier (and, in many cases, cheaper) to build and operate resilient, highly available, cloud applications at all scales. That applies to systems built by humans and agents, with the benefits magnified for agentic builders. Agents are great at building, but not (yet?) so great at long-horizon tasks like database operations. A database designed to simplify operations really helps. DSQL’s architecture is disaggregated. Multiple independent services, each focused on a small number of well-defined concerns. DSQL isn’t the first disaggregated OLTP database. Many, including Aurora and DynamoDB, came before. What’s interesting here is how we learned from those systems, and the lessons that came from operating them at scale: avoiding large caches (they make failovers and read scale tricky), offering strongly consistent scalable reads (avoiding whole classes of hot key and partition problems), and pushing expensive operations like scans down to storage where they run right next to local SSD. We’ve also taken the approach further than others, especially in completely disaggregating replication from transaction isolation and storage. Packing multiple workloads reduces the peak-to-average ratio of load on each physical machine (approximately at the rate of √loads). Given that resource cost typically scales with peak, and revenue scales with average, the economic benefit is obvious. Here we’re cheekily replicating a point we also made in the Firecracker paper. Another practical advantage of OCC is preventing clients from ever being able to block other clients. At scale, we have found that performance cross talk between clients is a significant contributor to tail latency in apps backed by pessimistic relational databases (for example clients which pause for garbage collection while holding a write lock). Operational experience at Amazon has shown that contention on locks, and retries on failure to acquire locks, are frequent contributors to system outages, and metastable failures which drive long recovery times … I’ve written about metastable failures (have you tried Stability Sim?) many times, and this is a key part of how we designed DSQL to help applications avoid them. It also helps avoid a large class of other, more mundane, failures cause by misbehaving clients and client code. And figure 6, another big advantage of the architecture, showing fast SELECTs and UPDATEs, even in the multi-region setting (faster than the speed of light!): The 2007 Amazon Dynamo paper’s embrace of eventual consistency [12], and Werner Vogels 2009 article Eventually Consistent [36], reflected the thinking at the time that cloud-scale systems needed to embrace eventual consistency to achieve their availability and latency goals. Since then, advancements in time distribution, data-center networks, power and cooling infrastructure, and distributed protocols have changed the trade-offs. A perfect illustration of ongoing benefits to databases and software systems coming from hardware, datacenter design, and networking innovations. It’s hard to overstate the power of this trend (and the opportunity still available). I continue to believe that strong consistency is the right default, and the right choice for nearly all applications. Check out out paper here: Aurora DSQL: Scalable, Multi-Region OLTP'
content: extracted
html: 2026-07-19-aurora-dsql-scalable-multi-region-oltp.html
remote_preview:
  url: https://brooker.co.za/blog/images/dsql_fig6.png
---

A paper!

Our new paper, [Aurora DSQL: Scalable, Multi-Region OLTP](https://arxiv.org/abs/2607.13276), is now available on Arxiv. I’m excited about this one: it’s a fully end-to-end look at how Aurora DSQL works, from query processing, to transactions, to replication, to the control plane. We’ve shared most of this content before in other forms, on this blog, on [Marc Bowes’ Blog](https://marc-bowes.com/), [Werner’s Blog](https://www.allthingsdistributed.com/2025/05/just-make-it-scale-an-aurora-dsql-story.html), in talks, and on the AWS blog. But this version covers all the ground, all in one place.

You should [read it](https://arxiv.org/abs/2607.13276).

Or check out the [interactive transaction simulator](https://brooker.co.za/dsql-transaction-flow.html) to get a feel for how it works.

*Some highlights*

> Our overall goal was to build a relational database system that simplifies the work of application building and operations, freeing builders from worrying about scale, reliability, durability, and even multi-region fault tolerance. A database of first resort, which is simple and easy to adopt at low scale, and grows with the application, without adding complexity.

I’ve written about this argument before (in [DSQL: Simplifying Architectures](https://brooker.co.za/blog/2025/11/02/thinking-dsql.html)), and it remains the thing that excites me most about the Aurora DSQL product. I believe we’ve made it much easier (and, in many cases, cheaper) to build and operate resilient, highly available, cloud applications at all scales. That applies to systems built by humans and agents, with the benefits magnified for agentic builders. Agents are great at building, but not (yet?) so great at long-horizon tasks like database operations. A database designed to simplify operations really helps.

> DSQL’s architecture is disaggregated. Multiple independent services, each focused on a small number of well-defined concerns.

DSQL isn’t the first disaggregated OLTP database. Many, including Aurora and DynamoDB, came before. What’s interesting here is how we learned from those systems, and the lessons that came from operating them at scale: avoiding large caches (they make failovers and read scale tricky), offering strongly consistent scalable reads (avoiding whole classes of hot key and partition problems), and pushing expensive operations like scans down to storage where they run right next to local SSD.

We’ve also taken the approach further than others, especially in completely disaggregating replication from transaction isolation and storage.

> Packing multiple workloads reduces the peak-to-average ratio of load on each physical machine (approximately at the rate of √loads). Given that resource cost typically scales with peak, and revenue scales with average, the economic benefit is obvious.

Here we’re cheekily replicating a point we also made in the [Firecracker paper](https://www.usenix.org/conference/nsdi20/presentation/agache).

> Another practical advantage of OCC is preventing clients from ever being able to block other clients. At scale, we have found that performance cross talk between clients is a significant contributor to tail latency in apps backed by pessimistic relational databases (for example clients which pause for garbage collection while holding a write lock). Operational experience at Amazon has shown that contention on locks, and retries on failure to acquire locks, are frequent contributors to system outages, and metastable failures which drive long recovery times …

I’ve written about metastable failures (have you tried [Stability Sim](https://stability-sim.systems/)?) many times, and this is a key part of how we designed DSQL to help applications avoid them. It also helps avoid a large class of other, more mundane, failures cause by misbehaving clients and client code.

And figure 6, another big advantage of the architecture, showing fast `SELECT`s and `UPDATE`s, even in the multi-region setting (faster than the speed of light!):

![](https://brooker.co.za/blog/images/dsql_fig6.png)

> The 2007 Amazon Dynamo paper’s embrace of eventual consistency \[12\], and Werner Vogels 2009 article Eventually Consistent \[36\], reflected the thinking at the time that cloud-scale systems needed to embrace eventual consistency to achieve their availability and latency goals. Since then, advancements in time distribution, data-center networks, power and cooling infrastructure, and distributed protocols have changed the trade-offs.

A perfect illustration of ongoing benefits to databases and software systems coming from hardware, datacenter design, and networking innovations. It’s hard to overstate the power of this trend (and the opportunity still available). I continue to believe that [strong consistency](https://brooker.co.za/blog/2025/11/18/consistency.html) is the right default, and the right choice for nearly all applications.

Check out out paper here: [Aurora DSQL: Scalable, Multi-Region OLTP](https://arxiv.org/abs/2607.13276)
