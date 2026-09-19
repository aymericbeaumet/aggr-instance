---
title: High-Performance Database Architecture
link: https://www.computerenhance.com/p/spacetimedb
source: computerenhance-com
published: 2026-09-09T16:03:33Z
updated: 2026-09-09T16:03:33Z
first_seen: 2026-09-19T21:30:23.395188495Z
authors:
- Casey Muratori
summary: An interview with Tyler Cloutier
content: extracted
html: 2026-09-09-high-performance-database-architecture.html
preview:
  file: 2026-09-09-high-performance-database-architecture.preview-ffd8f0542120.webp
  width: 256
  height: 144
  color: '#69857b'
images:
- source: https://substack-post-media.s3.amazonaws.com/public/images/4ff2712d-2159-4aeb-a4df-54b768f7acdf_3840x2160.png
  original:
    file: 2026-09-09-high-performance-database-architecture.image-52e02143006c.png
    width: 3840
    height: 2160
  variants:
  - file: 2026-09-09-high-performance-database-architecture.image-ce4b981c55a5.webp
    width: 320
    height: 180
  - file: 2026-09-09-high-performance-database-architecture.image-65ac11da3b2d.webp
    width: 640
    height: 360
  - file: 2026-09-09-high-performance-database-architecture.image-bf82b21578e6.webp
    width: 960
    height: 540
  - file: 2026-09-09-high-performance-database-architecture.image-a5f0daecbcee.webp
    width: 1280
    height: 720
  - file: 2026-09-09-high-performance-database-architecture.image-2d47b8bc09e8.webp
    width: 1600
    height: 900
  - file: 2026-09-09-high-performance-database-architecture.image-e045302de211.webp
    width: 3840
    height: 2160
  color: '#062a26'
---

The conventional wisdom holds that you wouldn’t use an ACID-style, traditional database for holding the runtime state of a real-time application. Certainly you could use one on the backend to hold persistent state, or to handle financial transactions, but you wouldn’t expect an ACID database to be employed as the primary way you’d do something like complete world state management for a videogame.

But does that actually have to be the case, or is that an antiquated notion of what a database can do? I sat down with Tyler Cloutier from the SpacetimeDB team to talk about that specific question.
