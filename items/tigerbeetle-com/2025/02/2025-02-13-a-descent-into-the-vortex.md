---
title: A Descent Into the Vörtex
link: https://tigerbeetle.com/blog/2025-02-13-a-descent-into-the-vortex
source: tigerbeetle-com
published: 2025-02-13T00:00:00Z
updated: 2025-02-13T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Oskar Wickström
summary: We’re huge fans of Deterministic Simulation Testing (DST) at TigerBeetle, which enables us to perfectly reproduce complex failures of a distributed system on a single laptop. But that’s not today’s story! We’ve added an intentionally non-deterministic testing harness, the Vörtex!
content: extracted
html: 2025-02-13-a-descent-into-the-vortex.html
remote_preview:
  url: https://tigerbeetle.com/blog/2025-02-13-a-descent-into-the-vortex/banner-0a59fcba.webp
---

We’re huge fans of Deterministic Simulation Testing (DST) at TigerBeetle, which enables us to perfectly reproduce complex failures of a distributed system on a single laptop. But that’s not today’s story! We’ve added an intentionally non-deterministic testing harness, the Vörtex!

TigerBeetle applies defense-in-depth to testing, with [Deterministic Simulation Testing](https://tigerbeetle.com/blog/2023-07-11-we-put-a-distributed-database-in-the-browser) (DST), fuzzers, integration tests, unit tests, [snap tests](https://tigerbeetle.com/blog/2024-05-14-snapshot-testing-for-the-masses), large scale tests of up to 100 billion transactions, and 6,000+ assertions in production for anything that slips past. While this covers some ground, we recently asked the question: how could we invest in yet another defense, to test TigerBeetle in a non-deterministic environment?

To increase coverage, we’re building Vortex (Vörtex for a heavy metal vibe). It’s a generative full-system test suite, checking safety and liveness properties, multiple client languages, through fault injection. By not only testing “from the inside out” (with DST), but also testing compiled binaries and client libraries “from the outside in”, to subject them to the stress they might see in a real deployment, we can only increase the probability that we find and fix bugs before they reach users.

Vortex therefore focuses on increasing our coverage of the non-deterministic parts of TigerBeetle. For example, while our client libraries build on a shared client library that is covered by DST, their (purposefully thin) native language binding and [`tb_client`](https://tigerbeetle.com/blog/2023-02-21-writing-high-performance-clients-for-tigerbeetle) wrapper nevertheless are not covered by DST, and may contain bugs that slip past other tests, since the integration of native code into a managed runtime can be tricky.

Further, TigerBeetle replicas and clients communicate by message passing over the network, and this implementation is stubbed out in our DST. The same goes for the storage interface used to read and write data to disk. These interfaces were carefully designed in the first place to “promise nothing” and be as faulty as our underlying network and storage fault models (i.e. a message or disk write may be dropped). While they were subjected to fuzz tests, integration tests, and unit tests, nevertheless, they were not yet subjected to generative testing.

We wanted Vortex to work on “the real thing”, the actual production binaries, rather than instrumented, customized or “resource constrained” builds. In addition, the need to test multiple language clients, possibly running concurrently in a single test, led us to the following architecture:

- *Supervisor*: the main process, to coordinate the workload, a cluster of replicas, and a driver. It also injects faults and runs liveness checks.
- *Workload*: a driver to create accounts and transfers, run queries, and check safety properties.
- *Driver*: a small language-specific program that accepts requests from the workload, runs them using a client library, and returns the results.
- *Replicas*: stock standard TigerBeetle release binaries forming a cluster.

Vortex injects network faults (delay, loss, corruption) by wiring everything together through a TCP proxy controlled by the supervisor. We simulate process faults by killing or pausing replicas, and restarting or resuming them after some time.

So far, we have Vortex drivers for the Zig and Java clients. This covers plenty of shared code already, but we aim for all clients to have drivers, including the [new](https://github.com/tigerbeetle/tigerbeetle/tree/main/src/clients/python) [clients](https://www.youtube.com/playlist?list=PL9eL-xg48OM0iMz7n4bggGlFGdwB4Y6Sh) [coming](https://x.com/errebino/status/1882851898623214036) [out](https://www.youtube.com/watch?v=4HcAcnJm5ig).

If you’re interested in learning more, go ahead and explore [the source code](https://github.com/tigerbeetle/tigerbeetle/tree/main/src/testing/vortex).

Vortex is four months old and has already found two bugs!

- [#2401](https://github.com/tigerbeetle/tigerbeetle/pull/2401): the shared client library incorrectly batched some combinations of requests, potentially creating open chains of linked events. The bug resided in `tb_client`.
- [#2430](https://github.com/tigerbeetle/tigerbeetle/pull/2430): connections were terminated twice, caused by an interaction between process faults and clients being closed with a particular timing.

Further, we’ve used Vortex to verify a recently discovered memory issue ([#2413](https://github.com/tigerbeetle/tigerbeetle/pull/2413)), and we’re investigating a possible liveness issue that Vortex provokes within hours.

We see Vortex as the perfect canary. After DST has already fast-forwarded (like a movie) the finding of rare bugs, and then replayed them again and again to fix them rapidly—if there’s anything that slips past, Vortex tells us where we have coverage gaps.

This is just the beginning. We have many things in the pipeline for Vortex:

- Automation for running Vortex continuously for long stretches of time.
- Version upgrading procedures for replicas and clients.
- More advanced and non-global network faults, e.g. symmetric and asymmetric partitions.
- Storage faults, e.g. latent sector errors, corruption and torn writes.
- Concurrent heterogenous drivers, under a single workload.

To maximize coverage, we’ll continue investing in defense-in-depth and venturing into the state space abyss, both deterministically and non-deterministically!
