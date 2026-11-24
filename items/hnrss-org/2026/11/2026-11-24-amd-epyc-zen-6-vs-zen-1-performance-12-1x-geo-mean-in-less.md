---
title: 'AMD EPYC Zen 6 vs. Zen 1 Performance: 12.1x Geo Mean in Less Than 10 Years'
link: https://www.phoronix.com/review/amd-epyc-9006-7601
source: hnrss-org
published: 2026-11-24T05:15:22Z
updated: 2026-11-24T05:15:22Z
first_seen: 2026-11-24T17:30:21.295672362Z
authors:
- zdw
content: extracted
html: 2026-11-24-amd-epyc-zen-6-vs-zen-1-performance-12-1x-geo-mean-in-less.html
remote_preview:
  url: https://www.phoronix.net/image.php?id=amd-epyc-9006-7601&image=amd_epyc_naples_venice_3
---

This month brought the availability of the new [EPYC 9006](https://www.phoronix.com/search/EPYC+9006) "Venice" SP7 Zen 6 server processors. We have seen excellent generational gains with the likes of the [EPYC 9686F](https://www.phoronix.com/review/amd-epyc-9686f) and [EPYC 9996](https://www.phoronix.com/review/amd-epyc-9996) over Zen 5 Turin CPUs as well as [sharply outperforming the competition](https://www.phoronix.com/review/amd-venice-nvidia-vera) from Intel and NVIDIA. With the first-generation EPYC "Naples" processors having launched nearly ten years ago, I was curious to quantify AMD's server performance gains over the past decade. So up today is a look at how the EPYC 9996 and EPYC 9686F Zen 6 processors compare to the original EPYC Zen 1 flagship with the EPYC 7601 across real-world workloads for performance as well as power efficiency for some pretty epic gains.

[![AMD EPYC Naples and Venice CPUs](https://www.phoronix.net/image.php?id=amd-epyc-9006-7601&image=amd_epyc_naples_venice_1_med)](https://www.phoronix.com/image-viewer.php?id=amd-epyc-9006-7601&image=amd_epyc_naples_venice_1_lrg)

The AMD EPYC 7601 launched back in June 2017 as the flagship of the EPYC Naples "Zen 1" processors. The EPYC 7601 was notable for its time with 32 x Zen 1 cores / 64 threads, 2.2GHz base frequency, 3.2GHz max boost frequency, and 64 MB of L3 cache. On the memory side was eight channel DDR4-2666 memory and for connectivity were up to 128 PCIe 3.0 lanes. The TDP of each EPYC 7601 processor is 180 Watts.

[![AMD EPYC Naples and Venice processor bottoms](https://www.phoronix.net/image.php?id=amd-epyc-9006-7601&image=amd_epyc_naples_venice_2_med)](https://www.phoronix.com/image-viewer.php?id=amd-epyc-9006-7601&image=amd_epyc_naples_venice_2_lrg)

Now just shy of a decade later, the EPYC 9006 SP7 processors have begun shipping. With the AMD EPYC 9996 256-core flagship are 256 Zen 6C cores / 512 threads per socket, 2.55GHz base clock, 4.1GHz max boost frequency, and an incredible 1024MB of L3 cache. EPYC 9006 SP7 processors support DDR5 RDIMMs or up to MRDIMM-12800 memory at 16 channels. There is also 96 lanes of PCI Express 6.0. The EPYC 9996 has a 600W default power rating.

[![AMD EPYC Zen 1 Naples and Zen 6 Venice CPUs](https://www.phoronix.net/image.php?id=amd-epyc-9006-7601&image=amd_epyc_naples_venice_3_med)](https://www.phoronix.com/image-viewer.php?id=amd-epyc-9006-7601&image=amd_epyc_naples_venice_3_lrg)

Or for the frequency-optimized flagship is the EPYC 9686F with 96 Zen 5 cores / 192 threads with a 4.05GHz base frequency and up to 5.3GHz maximum boost clock. The EPYC 9686F has a 384MB L3 cache size while also having a 600 Watt default power rating.

[![AMD EPYC 9996 CPU](https://www.phoronix.net/image.php?id=amd-epyc-9006-7601&image=amd_epyc_naples_venice_5_med)](https://www.phoronix.com/image-viewer.php?id=amd-epyc-9006-7601&image=amd_epyc_naples_venice_5_lrg)

Over the past ten years of AMD EPYC there has been incredible architectural advancements too not reflected in the key specs alone such as AVX-512 with Zen 4 and further refined in Zen 5, security improvements to reduce the need for software-based security mitigations, and countless other architectural improvements each step of the way from Zen 1 to Zen 6.

[![AMD EPYC Zen 1 Dell PowerEdge server](https://www.phoronix.net/image.php?id=amd-epyc-9006-7601&image=amd_epyc_naples_venice_4_med)](https://www.phoronix.com/image-viewer.php?id=amd-epyc-9006-7601&image=amd_epyc_naples_venice_4_lrg)

For comparing EPYC Zen 1 to Zen 6, I dusted off the Dell PowerEdge R7425 server originally provided by AMD for EPYC Naples testing at Phoronix. Nearly ten years later, the Dell PowerEdge server with the dual AMD EPYC 7601 processors still fires up and runs reliably. The AMD EPYC 7601 2P was running with 16 x 32GB DDR4-2666 memory, Samsung SSD 860 SSD storage, and other stock components of this Naples review unit.

[![AMD EPYC Venice server](https://www.phoronix.net/image.php?id=amd-epyc-9006-7601&image=amd_epyc_naples_venice_6_med)](https://www.phoronix.com/image-viewer.php?id=amd-epyc-9006-7601&image=amd_epyc_naples_venice_6_lrg)

With the AMD EPYC 9686F 2P and EPYC 9996 2P testing was the Nigeria reference server with 32 x 64GB MRDIMM-12800 memory, and Micron MTFDMBQ7T6THM-1BP1JABDC PCIe Gen6 SSD.

![AMD EPYC Zen 6 Venice vs. EPYC Zen 1 Naples Linux Benchmarks](https://phoronix.com/benchmark/result/amd-epyc-zen-6-venice-vs-epyc-zen-1-naples-linux-benchmarks/result.svgz)

The AMD EPYC Naples and EPYC Venice servers were tested on the same software stack with using Ubuntu 26.10 for a very up-to-date open-source software experience. Ubuntu 26.10 ships with the Linux 7.3 kernel and GCC 15.3 compiler by default.

In addition to the raw performance testing for EPYC Zen 1 vs. EPYC Zen 6, the CPU power consumption and performance-per-Watt were also compared using the exposed CPU PowerCap/RAPL sensors of 2P processor configuration.
