---
title: How did AMD Ryzen get 50% faster in two years?
link: https://lemire.me/blog/2026/09/18/how-did-amd-ryzen-get-50-faster-in-two-years/
source: hnrss-org-frontpage
published: 2026-09-18T19:01:03Z
updated: 2026-09-18T19:01:03Z
first_seen: 2026-09-23T00:20:14.260676283Z
authors:
- ibobev
summary: 'Article URL: https://lemire.me/blog/2026/09/18/how-did-amd-ryzen-get-50-faster-in-two-years/ Comments URL: https://news.ycombinator.com/item?id=49758709 Points: 185 # Comments: 63'
content: extracted
html: 2026-09-18-how-did-amd-ryzen-get-50-faster-in-two-years.html
preview:
  file: 2026-09-18-how-did-amd-ryzen-get-50-faster-in-two-years.preview-69ba00429ba2.webp
  width: 256
  height: 158
  color: '#494945'
images:
- source: https://lemire.me/blog/wp-content/uploads/2026/09/ryzen-featured-1024x633.jpg
  original:
    file: 2026-09-18-how-did-amd-ryzen-get-50-faster-in-two-years.image-98a44d82285e.jpg
    width: 1024
    height: 633
  color: '#141718'
- source: https://lemire.me/blog/wp-content/uploads/2026/09/ryzen-featured.jpg
  original:
    file: 2026-09-18-how-did-amd-ryzen-get-50-faster-in-two-years.image-d0b2e544c867.webp
    width: 1650
    height: 1020
  color: '#141719'
- source: https://lemire.me/blog/wp-content/uploads/2026/09/ryzen-geekbench6.jpg
  original:
    file: 2026-09-18-how-did-amd-ryzen-get-50-faster-in-two-years.image-f3c69a0dc87b.webp
    width: 1199
    height: 497
  variants:
  - file: 2026-09-18-how-did-amd-ryzen-get-50-faster-in-two-years.image-364aae87a849.webp
    width: 320
    height: 133
  color: '#fcfdfd'
- source: https://lemire.me/blog/wp-content/uploads/2026/09/ryzen-frequency.jpg
  original:
    file: 2026-09-18-how-did-amd-ryzen-get-50-faster-in-two-years.image-170f298142fe.webp
    width: 1200
    height: 882
  variants:
  - file: 2026-09-18-how-did-amd-ryzen-get-50-faster-in-two-years.image-fa07d9c665d3.webp
    width: 320
    height: 235
  color: '#fdfdfd'
- source: https://lemire.me/blog/wp-content/uploads/2026/09/ryzen-transistors.jpg
  original:
    file: 2026-09-18-how-did-amd-ryzen-get-50-faster-in-two-years.image-154da4a83f4d.webp
    width: 1200
    height: 661
  variants:
  - file: 2026-09-18-how-did-amd-ryzen-get-50-faster-in-two-years.image-60cf222ee1d4.webp
    width: 320
    height: 176
  color: '#fdfdfd'
- source: https://lemire.me/blog/wp-content/uploads/2026/09/ryzen-core-changes.jpg
  original:
    file: 2026-09-18-how-did-amd-ryzen-get-50-faster-in-two-years.image-f58eafa182df.webp
    width: 1200
    height: 699
  variants:
  - file: 2026-09-18-how-did-amd-ryzen-get-50-faster-in-two-years.image-4c8f1b7bec60.webp
    width: 320
    height: 186
  color: '#fefefe'
- source: https://lemire.me/blog/wp-content/uploads/2026/09/ryzen-simd.jpg
  original:
    file: 2026-09-18-how-did-amd-ryzen-get-50-faster-in-two-years.image-15e6c593a512.webp
    width: 1199
    height: 399
  variants:
  - file: 2026-09-18-how-did-amd-ryzen-get-50-faster-in-two-years.image-7752d6a3becb.webp
    width: 320
    height: 106
  color: '#fdfdfd'
- source: https://secure.gravatar.com/avatar/a0c6c34cf4a45ff5083d5ea541e7f27c5e4457ac393889e077af10688f6b3831?s=112&d=mm&r=g
  original:
    file: 2026-09-18-how-did-amd-ryzen-get-50-faster-in-two-years.image-71046faff53a.jpg
    width: 112
    height: 112
  color: '#c5c5c5'
---

![Three desktop processors in a row, from nearest to farthest](https://lemire.me/blog/wp-content/uploads/2026/09/ryzen-featured.jpg)

18 September 2026 · 3 min

People still tell me that CPUs are boring. That nothing much happens anymore.

Let us look at AMD Ryzen 7 processors from 2022 to 2024: the 5800X3D (Zen 3), the 7800X3D (Zen 4) and the 9800X3D (Zen 5). They are comparable 8-core chips with 3D V-Cache. I have written about them before, in [How stagnant is CPU technology?](https://lemire.me/blog/2026/01/14/how-stagnant-is-cpu-technology/)

On Geekbench 6, performance went up by about 50% in two years.

[![Geekbench 6 single-core and multi-core scores for Ryzen 7 5800X3D, 7800X3D and 9800X3D](https://lemire.me/blog/wp-content/uploads/2026/09/ryzen-geekbench6.jpg)](https://lemire.me/blog/wp-content/uploads/2026/09/ryzen-geekbench6.jpg)

|             |                    |                    |                    |
| ----------- | ------------------ | ------------------ | ------------------ |
|             | 2022 5800X3D Zen 3 | 2023 7800X3D Zen 4 | 2024 9800X3D Zen 5 |
| Single-core | 2,016              | 2,426              | 2,969              |
| Multi-core  | 11,832             | 15,508             | 18,751             |

The 2024 chip is 47% faster on a single core than the 2022 chip, and 58% faster with all cores.

The clock did not do that. Max boost went from 4.5 GHz to 5.2 GHz, a 15% increase.

[![Base and max boost frequencies for Ryzen 7 5800X3D, 7800X3D and 9800X3D](https://lemire.me/blog/wp-content/uploads/2026/09/ryzen-frequency.jpg)](https://lemire.me/blog/wp-content/uploads/2026/09/ryzen-frequency.jpg)

|           |            |            |            |
| --------- | ---------- | ---------- | ---------- |
|           | 2022 Zen 3 | 2023 Zen 4 | 2024 Zen 5 |
| Base      | 3.4 GHz    | 4.2 GHz    | 4.7 GHz    |
| Max boost | 4.5 GHz    | 5.0 GHz    | 5.2 GHz    |

The number of transistors is way up, by about 50%, from roughly 11 billion to 16 billion. Most of the extra transistors went into the core, not the cache.

[![Transistor counts for Ryzen 7 5800X3D, 7800X3D and 9800X3D, split into core, I/O and cache](https://lemire.me/blog/wp-content/uploads/2026/09/ryzen-transistors.jpg)](https://lemire.me/blog/wp-content/uploads/2026/09/ryzen-transistors.jpg)

How do you turn extra transistors into extra performance?

You make the core wider, and you give it more to work with. Dispatch width went from a maximum of 6 instructions per cycle to 8. The L2 cache per core doubled, from 512 KB to 1 MB. The L1 data cache went from 32 KB to 48 KB. Integer ALUs went from 4 to 6. The reorder buffer grew from 256 to 448 entries, so the processor can keep more instructions in flight and schedule them better.

[![Cache sizes, dispatch width, integer ALUs and reorder buffer from Zen 3 to Zen 5](https://lemire.me/blog/wp-content/uploads/2026/09/ryzen-core-changes.jpg)](https://lemire.me/blog/wp-content/uploads/2026/09/ryzen-core-changes.jpg)

|                   |            |            |            |
| ----------------- | ---------- | ---------- | ---------- |
|                   | Zen 3 2022 | Zen 4 2023 | Zen 5 2024 |
| L2 cache per core | 512 KB     | 1 MB       | 1 MB       |
| L1 data cache     | 32 KB      | 32 KB      | 48 KB      |
| Dispatch width    | 6          | 6          | 8          |
| Integer ALUs      | 4          | 4          | 6          |
| Reorder buffer    | 256        | 320        | 448        |

For data parallelism (SIMD), Zen 5 is a different machine. Zen 3 and Zen 4 had four 256-bit SIMD arithmetic units. Zen 5 has four 512-bit units. Loads and stores widened the same way: two 512-bit loads per cycle, one 512-bit store.

[![SIMD arithmetic units, loads per cycle and stores per cycle from Zen 3 to Zen 5](https://lemire.me/blog/wp-content/uploads/2026/09/ryzen-simd.jpg)](https://lemire.me/blog/wp-content/uploads/2026/09/ryzen-simd.jpg)

|                       |             |             |             |
| --------------------- | ----------- | ----------- | ----------- |
|                       | Zen 3 2022  | Zen 4 2023  | Zen 5 2024  |
| SIMD arithmetic units | 4 × 256-bit | 4 × 256-bit | 4 × 512-bit |
| Loads per cycle       | 2 × 256-bit | 2 × 256-bit | 2 × 512-bit |
| Stores per cycle      | 1 × 256-bit | 1 × 256-bit | 1 × 512-bit |

I already made the point that [processors are getting wider](https://lemire.me/blog/2025/09/01/processors-are-getting-wider/). This is what that looks like on a desktop chip you can buy.

What about the next step? Zen 6 is arriving. AMD is talking about a 256-core Epyc part (Venice) with a gigabyte of L3 cache. We do not yet know what the desktop cores will look like. It could be wild.

Further reading: [AMD’s 256-core Epyc 9996 ‘Venice’](https://www.tomshardware.com/pc-components/cpus/amds-256-core-epyc-9996-venice-claims-up-to-a-3-4x-jump-over-intel-xeon-competition-20-percent-over-nvidia-vera-zen-6-comes-with-up-to-1024mb-of-l3-16-channel-memory-and-5ghz-clock-speeds) (Tom’s Hardware).

## Published by

![](https://secure.gravatar.com/avatar/a0c6c34cf4a45ff5083d5ea541e7f27c5e4457ac393889e077af10688f6b3831?s=112&d=mm&r=g)
