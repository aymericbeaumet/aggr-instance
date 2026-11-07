---
title: GitHub's new Git storage architecture
link: https://github.blog/engineering/architecture-optimization/inside-githubs-new-git-storage-architecture/
source: hnrss-org
published: 2026-11-06T19:24:21Z
updated: 2026-11-06T19:24:21Z
first_seen: 2026-11-07T10:39:02.057754401Z
authors:
- DAlperin
content: extracted
html: 2026-11-06-github-s-new-git-storage-architecture.html
remote_preview:
  url: https://github.blog/wp-content/uploads/2026/01/4ba0cd42388a255e04c78e5143548f22e577d68e0f15f68e6a3c76c18b927981-1920x1080-1.png
---

Last week at GitHub Universe, we announced our new Git storage architecture, capable of handling thousands of writes per second with reads that scale on demand. Through internal testing, we’ve demonstrated the ability to support 5,000 concurrent agents working on a repository, sending 7,200 pushes per second to a repo, all while serving read traffic with consistently low latency. This new architecture is the foundation for GitHub to support agentic development for hundreds of millions of developers, and we’ve been building it underneath GitHub while it keeps running.

Over the past year, agents changed what a busy repository looks like on GitHub. In our [first post](https://github.blog/engineering/architecture-optimization/building-git-infrastructure-for-agent-scale-development/) in this series, we laid out the two principles behind our next-generation Git storage architecture: minimize coordination, and decouple storage from compute. In our [last post](https://github.blog/engineering/architecture-optimization/the-bridge-to-agent-scale-git-infrastructure/), we covered the bridge: moving 99.99999% of repositories into Azure, scoping the write quorum to individual references, and standing up virtual replicas backed by Azure Blob Storage.

We also left off with an observation: very little of a write needs to be ordered. Git objects are immutable and named by their contents, meaning two writers can store them at the same time without agreeing on anything. The part of a push that needs agreement is the reference update, and a reference update is tiny in comparison. Virtual replicas were the first step toward building around that, and the means by which we onboard repositories to the new architecture.

The rest of the architecture builds on that observation. Only reference updates need agreement. That lets the parts of Git infrastructure that used to be tightly coupled (storing data, serving requests, and maintaining repositories) scale independently. For developers, Git stays Git: they push, fetch, and branch the same way, with the same protections in place. Below, we walk through the requirements behind the design, follow a request through the system, and dive deep into two of its components.

## Building for agent scale

We think about the scale of the GitHub platform along two axes:

1. **Horizontal scale** is how many developers and agents can work across many repositories. Many of GitHub’s current approaches on this axis have been battle-tested over the years and are carried forward to the new architecture: partitioning of tenants across infrastructure, isolation and/or fairness enforcement of tenant traffic, and load-aware routing, to name a few.
2. **Vertical scale** is how many developers and agents can work on **one** repository. As agents write more code, we expect individual repositories to get larger and busier, so most of our requirements came from the vertical axis.

Today, the busiest repositories on GitHub.com receive at most dozens of writes in a single second, typically in short bursts. As more of that work shifts to agents running in parallel and around the clock, we expect those bursts to become a sustained rate within the next year. Each push leads to more reads: CI runs, deployment systems pull the new commit, and other agents fetch it for context. This read fan-out results in a large amplification effect on overall traffic that scales with writes.

We’re already seeing this trend play out on repositories at the top of the curve. The chart below shows daily requests to one repository on GitHub from March to May, when its traffic grew about 4x. Its growth has continued on an exponential trajectory since then.

![Line chart of daily requests to a single repository from March through May 2026. The March average is 44M requests per day and the May average is 181M requests per day, about a 4x increase.](https://github.blog/wp-content/uploads/2026/11/customer-growth.png?w=1600)

We have to plan for a world where agent-driven development keeps accelerating at this speed. Instead of planning for the next increment, we set a target of at least two orders of magnitude more scale. For a single repository, that meant the new architecture needed to:

- Accept thousands of writes per second.
- Support multiple writer nodes. There are two reasons for this:
  - Experience has taught us that indexing incoming packs and checking object connectivity are the most CPU-expensive parts of a write, and that cost scales with the size of the write, so a single node will become compute-bound quickly.
  - Spreading that work across many nodes improves fault tolerance and shortens recovery when a node fails.
- Minimize commit latency. An agent waits on each push before it moves to its next step.
- Record changes in order for each reference with the ability to quickly linearize across references. A virtual replica that falls behind needs to catch up by downloading *only* the data it needs to serve the incoming request.
- Keep reads cheap when nothing has changed, which is the common case given the read-to-write ratio.
- Let each component scale independently.

On top of that, the system had to be able to improve overall reliability and scale globally.

## Introducing “Project Rubicon”

In May of 2026, hot on the heels of the initial Azure migration, we started working on a design that could meet those demands. We tried several approaches. In all of them, pack data lived in Azure Blob Storage, and in some we experimented with keeping reference state there too. We’ll cover some of the trade-offs we found below, but we settled on a design we felt maximized throughput while preserving the correctness, durability, availability, and latency guarantees a platform like GitHub requires. We called it “Rubicon,” a nod to the point of no return the industry crossed with agentic development.

Let’s walk through Rubicon from the top down. Many things stay the same: requests from Git clients, the API, and the web UI arrive at Spokes, which still handles routing and load balancing, and serves the internal API that the rest of GitHub uses to read and write repository data. Spokes also maintains its role as a placement and balancing service, maintaining the partitioning, isolation, and tenant fairness required for horizontal scale. The data plane underneath Spokes, in contrast, is new.

![Architecture diagram. Git, API, and Web requests pass through Spokes to groups of virtual replicas per repository. Virtual replicas hold only cached state. Ref-CAS stores refs and the pack manifest, and Azure Blob Storage stores pack data. Compaction nodes compact and garbage collect packs in Blob Storage and update the pack manifest in Ref-CAS.](https://github.blog/wp-content/uploads/2026/11/request-flow.png?w=1600)

Spokes routes each request to one of a small group of virtual replicas assigned to that repository. Virtual replicas are a single fleet of disposable compute that serves reads, writes, or both. A virtual replica’s local disk is a cache that can be rebuilt at any time.

Spokes routes a repository’s traffic to the same virtual replicas when it can, which keeps their caches warm and latency low, but nothing about correctness depends on a request landing on any particular virtual replica. When a virtual replica goes away, the repository’s other virtual replicas keep serving, and a replacement fills its cache as traffic arrives.

When a repository gets busy, we add more virtual replicas. Separately, a fleet of compaction nodes works in the background, directly against Blob Storage and Ref-CAS, to merge each repository’s packs and remove unreachable objects. That work never runs on a virtual replica, meaning their CPU and memory can be almost entirely dedicated to serving requests.

Durable state lives in two places:

1. **Git objects**, in the form of packs and auxiliary read acceleration data, live in Azure Blob Storage.
2. **References** and the list of packs that make up the repository (the pack manifest) live in a separate replicated log service we call Ref-CAS (for “Reference Compare-and-Swap”).

Let’s walk the architecture from the perspective of a write and a read:

- **Write:** a virtual replica indexes the incoming pack and uploads it to Blob Storage, then commits one transaction to Ref-CAS that lists the pack in the manifest and updates the references. Later, a compaction node merges that pack with others to keep reads fast.
- **Read:** a virtual replica checks its reference version with Ref-CAS, pulls the packs it needs from Blob Storage into its cache, and serves the read from there.

There are many distinct approaches we’ve taken within the new architecture, from the software stack on the virtual replicas to how traffic routing and workload scheduling are tuned for different use cases (e.g., many small repos vs. one large one). Let’s dive deep into two components of the architecture and tell their stories centered on the two design principles.

## Minimizing coordination: mutating reference state at high throughput

Object storage services like Azure Blob Storage are incredibly good at what they do, and natural fits for many data-heavy applications like source code storage. They are durable and replicated, hold as much as we need them to, have a relatively low cost per byte, and deliver exceptionally high aggregate throughput so long as traffic is spread across many keys. As a result, nearly everything durable in the new Git storage architecture lives in Azure Blob Storage, including:

- Packs and indexes written by pushes, compaction, and garbage collection.
- Various read accelerators (like multi-pack indexes, reachability bitmaps, and some custom formats) that our maintenance process builds from those packs.
- Records that let compaction and garbage collection resume or audit their work, along with the leases that compaction nodes use to claim work.
- Cached responses for frequently repeated fetches.
- Checkpoints for the reference state of the repository, though not the authoritative state used to serve requests.

![Diagram of one repository's data in Azure Blob Storage. Push, Compaction, Maintenance, GC, and Ref-CAS write into it, and fetches read from it by exact key. It holds packs and indexes, records and leases, read accelerators such as the multi-pack index and reachability bitmap, cached fetch responses, and reference checkpoints.](https://github.blog/wp-content/uploads/2026/11/blob-contents.png?w=1600)

Most of that data is written once to a distinct key and never changes afterward, which is what makes it highly suitable for Blob Storage.

References are the opposite. A busy repository’s references are a relatively small piece of state that many writes race to update at the same time. That access pattern is harder to serve from Blob Storage, and our first prototypes showed us why. Object storage for this kind of workload is an intuitive choice, and one that works well up to a certain level of scale: conditional writes give a compare-and-swap for free, and the data is already durable and replicated. However, we ran into a difficult set of choices, illustrated below by two approaches we tried.

Our **first approach** was a write-ahead log for all of a repository’s references, indexed by a single blob. Every update is a conditional write against that blob. It’s correct, and reads are cheap: a virtual replica asks whether the blob has changed, and if it hasn’t, it’s done. Writes are where it falls short. Only one conditional write to a blob can be in flight at a time, so even when we batch every push that arrives during a write into the next one, the repository’s write ceiling is one write per round trip to a single blob. In practice, this netted us write throughput in the mid-hundreds per second.

Our **second approach** split the references across several segment blobs, with each reference hashing to one of them. Writes to references in different segments no longer wait on each other, which raises the ceiling. But a read now has to check every segment to know whether anything changed, and it’s only as fast as the slowest segment. We’d traded write latency for read latency, and reads outnumber writes by orders of magnitude. We can visualize this by showing multiple writes and a read side by side:

Neither option met every requirement, so we built something else: a replicated log service dedicated to references, or the Ref-CAS service. This is the (small) consensus group we said at the end of the [last post](https://github.blog/engineering/architecture-optimization/the-bridge-to-agent-scale-git-infrastructure/) was still somewhere in the stack. Here’s how it works.

Each reference update in Ref-CAS is a compare-and-swap committed by a quorum. A transaction locks only the references it touches, checks that each one still has the value the writer expects, and becomes visible once a majority of the log’s replicas have recorded it. Agents mostly push to their own branches, so most updates don’t contend, and they replicate in parallel. When two pushes do update the same reference, only one will append to the log.

Each commit to Ref-CAS gets a log sequence number (LSN), and the repository’s version is the LSN of its latest commit. Reads start with a version check against that version. A virtual replica sends the version of the reference state it already holds. If that version is still current, the reply carries no references at all, and an unchanged repository costs a single comparison. If the virtual replica is behind, it gets back the current references and version, along with pointers to pack data and anything else that needs to be known, such as when the last garbage collection cycle occurred.

Unlike the consensus group in our legacy architecture, Ref-CAS never holds or validates object data. It only stores pointers to it. A virtual replica uploads a push’s pack to Blob Storage before it commits the reference transaction. A reference only ever points at objects that are already durable.

These approaches allow us to transact reference state alone in the tens of thousands of operations per second, and provide a baseline for additional annotations on the replication log that help reads decide what to download for a given request.

But supporting thousands of writes per second on a single repository creates a second-order problem: maintaining the performance of reads and writes. For that, we needed a different approach to compaction.

## Decoupling storage from compute: compaction at thousands of writes per second

Every push adds a pack to Blob Storage. Git can serve a repository from many packs, but each additional pack makes lookups slightly more expensive, so the pack count can’t grow without bound. In our legacy architecture, compaction runs on the same replicas that serve traffic, and it has to run on every replica. In the new architecture, it runs entirely in the background on separate nodes, directly against durable storage, using the Ref-CAS service to notify virtual replicas of the evolving state.

Compaction nodes each claim ownership over a subset of the repository’s data via a lease, which allows us to scale compaction horizontally. Here’s how one compaction node works through a single shard of a repository’s packs:

The node first takes a lease on the shard in Blob Storage, so only one node compacts a shard at a time. Meanwhile, virtual replicas keep uploading packs and listing them in the pack manifest. The node reads the manifest to find new packs and collects them into an open segment, which is effectively a holding ground for data about to be compacted. When the segment reaches a defined limit based on pack count, size, or age, it seals the segment so it accepts no new packs, then combines its packs into fewer, larger ones. While compaction happens, the next segment for the shard can begin capturing its input packs. Before anything is published, the node checks that the new packs contain exactly the same objects as the inputs. It then uploads the new packs and adds them to the manifest. A second transaction removes the inputs from the manifest, and cleanup deletes them later.

To scale out, this process runs independently for many shards at once. Each pack’s hash maps to a shard, so adding shards and nodes adds compaction throughput. A compacted pack that’s still under the size limit goes back into a shard by its own hash and merges again, so packs grow in tiers until they reach the limit. The net result is an effectively flat pack count while pushes climb:

Compaction merges packs, but it keeps every object. Garbage collection is the step that removes objects nothing references anymore, like the ones left behind by deleted branches and force-pushes. In our new architecture, garbage collection works very similarly to compaction by using dedicated workers to handle the compute-heavy portion. It also has a few special behaviors for safely removing data, which are worthy of their own discussion.

Using these approaches, we’ve been able to consistently demonstrate a flat latency curve as writes climb, and we have other planned optimizations to make the compaction and garbage collection processes themselves more performant.

## What comes next

The two principles we started with let each part of this architecture scale on its own. We scale reads and writes by adding virtual replicas. Ref-CAS orders each reference independently instead of the whole repository. We scale maintenance by adding compaction nodes. Our goal is for every repository, from one developer’s side project to the busiest monorepo, to run on the same architecture with no special handling for the busy ones. Repositories don’t stay small. A side project can pick up a fleet of agents overnight, and it shouldn’t need a migration or special handling from us to keep up. One architecture also means every improvement we make reaches every repository at once.

This foundation lets us do a lot more. We’re focused on three areas next: scaling the architecture beyond what we can predict we’ll need today, making agents more efficient at the work they do on GitHub, and evolving Git itself for agentic development.

### Improving the architecture

We have plans to take this architecture further. Each of these improvements lets a repository handle more activity while reducing the work behind each request:

- **Read only what’s needed.** Fetch a single object from Blob Storage instead of the whole pack that holds it.
- **Organize packs by type.** Group commits, trees, and blobs into their own packs. Many requests then touch less data.
- **Partial replicas.** Serve requests without every pack on local disk.
- **Materialized views.** Keep repository data in ready-to-use forms, like prepared archives, a near-live shallow copy of the default branch, and agent session context stored alongside each commit.
- **Global replicas.** Place replicas in any region, close to the workloads that use them. Reads stay within bounded staleness of the latest write.
- **Scale to zero.** Grow and shrink the fleet with the repositories that are actually active.

### Efficient agent operations on GitHub

Scaling the infrastructure is one side of supporting agents. The other side is making each agent operation cheaper: fewer bytes to fetch, fewer round trips, and less waiting on changes. Each of these fast-follow items builds on the storage work in this post:

- **Work without a clone.** Many agent tasks never need a full working tree. They need a handful of files and enough context to make a change. They can read both straight from the server.
- **Atomic commit API.** Land a multi-file change in one operation. The whole change commits, or it’s rejected cleanly on conflict.
- **Event streams.** Agents and the systems around them work best with real-time information. A generic event stream lets them subscribe to changes and react as they happen.

### Improving Git for agents

We also have work to do in Git itself. GitHub has been a longtime contributor to the Git project, and we’re not stopping now. A few examples of the work we’re excited to continue:

- **Lightweight clones.** Partial clone, shallow clone, and sparse checkout each let an agent fetch less. Each one has gaps. They don’t always work well together, either. We want to improve each piece and make them fit.
- **Remove hidden scaling costs.** Git still has code paths that slow down much faster than the repository grows. Recent releases fixed several of them, but we expect to find more.
- **Faster history walks.** Metadata like the commit-graph can answer merge-base and reachability questions without walking every commit. That matters most when an agent’s API call turns into Git work on the server.
- **Avoid common gotchas.** Agents make the same mistakes developers do. Git can catch more of the common ones and warn before they cause damage.

Agents have changed how software gets built. GitHub and Git are changing with it. We’re excited to take this new architecture across the starting line. It’s available in a limited private preview now, with a broader rollout early next year. We can’t wait to see what teams build when the number of agents working on a repository is limited by their ideas. We’ll keep evolving Git and GitHub alongside our customers and the Git community.

* * *

## Written by

![Karthik Raman](https://github.blog/wp-content/uploads/2026/11/cropped-image-3-200x200.jpeg)

Distinguished Engineer

![Brian Celenza](https://avatars.githubusercontent.com/u/196120?v=4&s=200)

Brian is a Principal Software Engineer working on the storage and core services that power all repository interactions on GitHub.
