---
title: 'Show HN: Redis City – Explore how Redis works in an interactive 3D model'
link: https://poltora.dev/redis
source: hnrss-org-frontpage
published: 2026-09-12T19:43:34Z
updated: 2026-09-12T19:43:34Z
first_seen: 2026-09-15T12:38:22.798633021Z
authors:
- poltora
summary: 'Article URL: https://poltora.dev/redis Comments URL: https://news.ycombinator.com/item?id=49676425 Points: 122 # Comments: 20'
content: extracted
html: 2026-09-12-show-hn-redis-city-explore-how-redis-works-in-an.html
preview:
  file: 2026-09-12-show-hn-redis-city-explore-how-redis-works-in-an.preview-e61d1e07e1ca.webp
  width: 256
  height: 134
  alt: 'Redis City: How Redis works, in 3D. An interactive map of commands, memory, and persistence.'
  color: '#dadbd7'
images:
- source: https://poltora.dev/redis/redis-city-og-v1.jpg
  original:
    file: 2026-09-12-show-hn-redis-city-explore-how-redis-works-in-an.image-5d1960a29c2a.jpg
    width: 1200
    height: 630
  color: '#e8e9e4'
---

▱\
▱\
▱

## Redis City / engine room

One Redis instance, opened up.

KEYSPACE **keys**

ALLOCATED *≈*

MODEL TIME **s**

[Main site Site ↗](https://poltora.dev/)☰ System>\_ Console

×

SINGLE INSTANCE DB 0

## Inside Redis

Code runs on an OS thread; the CPU executes that thread. Data structures live in process memory. The kernel manages RAM and files. The lower decks show RAM and persistent storage, not more Redis servers. The CPU itself is not drawn.

### Experiments

REDIS 8.10.1 · STANDALONE · DB 0

Memory cutaway

command / lookup → reply ← memory access → AOF journal bytes RDB snapshot · BGSAVE only*object: dark header · orange value · gray pointer · violet TTL*

⌂＋−?

×

INSPECTOR

Select a key…

·

REDIS 8.10.1 SOURCE

### Process memory model ≈

Payload#db7458Allocated#70a88eActive pages#8fb5c4Allocator resident#809facProcess RSS#536d7d

Colored blocks represent structures, not adjacent physical addresses. Sizes and RSS come from the educational model.

Expired Evicted

Limits and persistencemaxmemory144 KiB160 KiB176 KiB192 KiB512 KiBPolicynoevictionallkeys-lruPersistenceRDB snapshotAOF everysecCreate snapshotPower loss → restart

Step →+1 s model time×↺ Reset MODEL · NOT A LIVE SERVER×

Model console browser only

❯Run ↵Sources and accuracy boundaries of the Redis prototype

MODEL BOUNDARIES

Real mechanisms at an educational scale.

The component map follows Redis 8.10.1. This is a standalone setup: one process, DB 0, and one dict inside the keys kvstore.

The command path and component names were checked against the 8.10.1 source. Redis and jemalloc do not run here. UTF-8 payload bytes are counted, while compact-encoding thresholds, structure sizes, bins, pages, background overhead, and RSS are approximations.

Model pages are allocated in 4 KiB multiples; large slots may span several pages. Real jemalloc may use multi-page slabs, several arenas, and thread caches. The scene omits swap, huge pages, replication, cluster mode, Lua, modules, transactions, blocking commands, hash-field TTL and AOF rewrite. ACL/TLS and threaded I/O are source context, not implemented services.

Experiments start from clean, fixed data and wait for you between commands. Guided console playback preserves previous commands. Its model clock also waits: use “+1 s model time” after playback to observe TTL, background snapshots or AOF synchronization. The clock advances only in an explicit time step; animation speed does not change TTL or snapshot outcomes. BGSAVE lasts six model seconds so it can be observed. AOF everysec and TTL use model time. Commands are calculated once; the scene, counters and inspector reveal the changes at their corresponding playback checkpoints. Load and ambient mode advance continuously. The load showcase enables AOF and also runs BGSAVE, matching a Redis setup that uses both persistence mechanisms.

Tubes show requests, references, allocation requests or serialized file bytes, depending on the branch—not one packet travelling through independent servers. Context returns have no moving bytes. During guided playback, each connection names the operation or payload and its destination. Moving markers illustrate that operation, not a count of bytes; object references and allocation calls are in-process memory accesses, not network transfers. Kernel pages and jemalloc slabs are illustrative; their source links point to Redis call sites, not to OS/allocator implementations.

### Usage analytics

On poltora.dev, a few usage events help improve this project: visits, experiment starts and completions, and Telegram link clicks. Console commands, keys and values are never included. No session recordings or additional tracking scripts. Browser Do Not Track and Global Privacy Control are respected.

Allow usage events on this browserBack to the scene
