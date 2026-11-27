---
title: 'Animations for System Builders: Metastability and Lost Writes'
link: http://brooker.co.za/blog/2026/10/23/animations.html
source: brooker-co-za
published: 2026-10-23T00:00:00Z
updated: 2026-10-23T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Marc Brooker
summary: 'Animations for System Builders: Metastability and Lost Writes Pretty pictures help us learn, maybe? One of the things I love using LLMs for is helping me understand systems better. In the Strands Decider post I wrote about how I often ask ask me questions to check I understand this. The other technique I’m enjoying right now, and am delighted to see folks around me using too, is building animations. Animations can help us build an intuition for how and why systems work, or how and why they fail. Here’s one example: did you know that a Raft cluster can lose writes if even one of the cluster members has a storage layer that lies about fsync()? Your browser does not support the video tag. When I built Stability Sim, I wanted to help engineers understand metastable failures better. That worked pretty well on an abstract level (e.g. retries and queues make metastability really easy to trigger), but I still hear doubts that these behaviors happen in real systems. After all, those real systems are production-proven, and wouldn’t we have shaken out the behaviors by now if they existed? Let’s start with Postgres. Your browser does not support the video tag. In this example, captured from traces on real Postgres 16 running in a real MicroVM. Before a load spike, the database is easily able to handle its 20 queries-per-second load, thanks to the whole working set sitting in the page cache. But after the load spike, we’ve filled up a lot of the memory running queries (each takes 12MB), which now run slower because they’re served off the disk rather than the page cache. I like this example, because it shows that even simple, non-distributed systems have these behaviors. What about etcd? Your browser does not support the video tag. Again, we have load spike triggering the metastable state. Writes, in this case. The overloaded peers look like they’re unavailable, which triggers a storm of application-level resends. I like this example, because etcd is widely-used quality software. Operational experience doesn’t seem to effectively shake out these kinds of issues. When I wrote Garbage Collection and Metastability, a widely-known systems academic told me that it was a fantasy. I’ve seen it happen in the wild in ways I couldn’t share, and I can forgive people for not believing this can happen based on the hand waving in the blog. So it’s nice to have a real world example, picking on etcd again. Your browser does not support the video tag. At the start, we’re ticking along nicely at 500 reads/second, before spiking load to 1000 reads/s. The read spike increases the time spent in GC, increasing average tenure, and making less CPU available for the read workload. This is a self-reinforcing loop, and the system doesn’t recover without intercession. I believe the same thing could happen on a runtime without GC, due to increasing work in the allocator, but haven’t been able to reproduce that yet. The animations here are all generated with Kiro and KiroCrew, mostly with Opus 5.5. The actual results came from a real test environment, which captures the traces that back the animations.'
content: extracted
html: 2026-10-23-animations-for-system-builders-metastability-and-lost-writes.html
remote_preview:
  url: https://brooker.co.za/blog/images/feed-icon-14x14.png
---

## About Me

My name is Marc Brooker. I like to build things that work, and do cool stuff. I like building big things. I also dabble in machining, welding, cooking, and skiing.

I am an engineer at Amazon Web Services (AWS) in Seattle, where I work on agentic AI, especially safety and policy for agentic AI. Before that, I worked on EC2, EBS, databases, serverless, and serverless databases.\
 All opinions are my own.

## Links

[My Publications and Videos](https://brooker.co.za/blog/publications.html)\
 [@marcbrooker on Mastodon](https://fediscience.org/@marcbrooker) [@MarcJBrooker on Twitter](https://twitter.com/MarcJBrooker)

[Is this blog written by AI?](https://brooker.co.za/blog/2026/06/18/my-blog-and-ai.html)

Pretty pictures help us learn, maybe?

One of the things I love using LLMs for is helping me understand systems better. In the [Strands Decider post](https://brooker.co.za/blog/2026/09/28/engineering-system-one.html) I wrote about how I often ask *ask me questions to check I understand this*. The other technique I’m enjoying right now, and am delighted to see folks around me using too, is building animations.

Animations can help us build an intuition for how and why systems work, or how and why they fail.

Here’s one example: did you know that a Raft cluster can lose writes if even one of the cluster members has a storage layer that lies about `fsync()`?

Your browser does not support the video tag.

When I built [Stability Sim](https://stability-sim.systems/), I wanted to help engineers understand metastable failures better. That worked pretty well on an abstract level (e.g. [retries and queues make metastability really easy to trigger](https://stability-sim.systems/?example=metastable-retry)), but I still hear doubts that these behaviors happen in real systems. After all, those real systems are production-proven, and wouldn’t we have shaken out the behaviors by now if they existed?

Let’s start with Postgres.

Your browser does not support the video tag.

In this example, captured from traces on real Postgres 16 running in a real MicroVM. Before a load spike, the database is easily able to handle its 20 queries-per-second load, thanks to the whole working set sitting in the page cache. But after the load spike, we’ve filled up a lot of the memory running queries (each takes 12MB), which now run slower because they’re served off the disk rather than the page cache. I like this example, because it shows that even simple, non-distributed systems have these behaviors.

What about `etcd`?

Your browser does not support the video tag.

Again, we have load spike triggering the metastable state. Writes, in this case. The overloaded peers look like they’re unavailable, which triggers a storm of application-level resends. I like this example, because `etcd` is widely-used quality software. Operational experience doesn’t seem to effectively shake out these kinds of issues.

When I wrote [Garbage Collection and Metastability](https://brooker.co.za/blog/2024/08/14/gc-metastable.html), a widely-known systems academic told me that it was a *fantasy*. I’ve seen it happen in the wild in ways I couldn’t share, and I can forgive people for not believing this can happen based on the hand waving in the blog.

So it’s nice to have a real world example, picking on `etcd` again.

Your browser does not support the video tag.

At the start, we’re ticking along nicely at 500 reads/second, before spiking load to 1000 reads/s. The read spike increases the time spent in GC, increasing average tenure, and making less CPU available for the read workload. This is a self-reinforcing loop, and the system doesn’t recover without intercession.

I believe the same thing could happen on a runtime without GC, due to increasing work in the allocator, but haven’t been able to reproduce that yet.

The animations here are all generated with [Kiro](https://kiro.dev) and [KiroCrew](https://github.com/kirodotdev/KiroCrew), mostly with Opus 5.5. The actual results came from a real test environment, which captures the traces that back the animations.
