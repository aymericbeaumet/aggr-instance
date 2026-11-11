---
title: 'AMD''s Zen 6 Family: Feeding the Beast'
link: https://chipsandcheese.com/p/amds-zen-6-family-feeding-the-beast
source: hnrss-org
published: 2026-11-10T17:02:32Z
updated: 2026-11-10T17:02:32Z
first_seen: 2026-11-11T00:34:12.222309031Z
authors:
- pixelpoet
content: extracted
html: 2026-11-10-amd-s-zen-6-family-feeding-the-beast.html
remote_preview:
  url: https://substackcdn.com/image/fetch/$s_!R25V!,w_1200,h_675,c_fill,f_jpg,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fff0ea2d4-ea2d-440b-ad51-1bc99a695511_2022x1382.png
---

Hello you fine Internet folks,

Today we’re looking at AMD’s next-generation Zen 6 CPU core family. We’ll explore the changes from Zen 5 and what the differences between Zen 6 and Zen 6c tell us about AMD’s approach to microarchitecture.

Hope y’all enjoy!

From a bird's eye view, Zen 6 is the natural evolution of Zen5, with the expansion of many OoO structures and iterative improvements found throughout the core.

Digging in deeper however tells a different story, with AMD heavily re-working several parts of the core, reminding me of the changes from Zen 1 to Zen 2.

[![](https://substackcdn.com/image/fetch/$s_!R25V!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fff0ea2d4-ea2d-440b-ad51-1bc99a695511_2022x1382.png)](https://substackcdn.com/image/fetch/$s_!R25V!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fff0ea2d4-ea2d-440b-ad51-1bc99a695511_2022x1382.png)

At a high level the changes AMD made to the integer schedules stand out, returning to a layout reminiscent of Zen 3 and Zen 4.

Zen 5 had the best direction predictor of any core we have measured and Zen 6 seemingly builds upon that same direction predictor.

[![](https://substackcdn.com/image/fetch/$s_!LKuF!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F91946bdf-c5e2-4c04-b8f1-b332854c24d7_1365x762.webp)](https://substackcdn.com/image/fetch/$s_!LKuF!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F91946bdf-c5e2-4c04-b8f1-b332854c24d7_1365x762.webp)

[![](https://substackcdn.com/image/fetch/$s_!P8g0!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9971593c-59b3-49bf-b0e3-37a2c468992e_1300x625.png)](https://substackcdn.com/image/fetch/$s_!P8g0!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9971593c-59b3-49bf-b0e3-37a2c468992e_1300x625.png)

The biggest difference we do see is in what happens once the direction predictor gets overwhelmed. Both cores hit that wall in about the same place and past it, the predictor is left guessing at random data, so the time per branch mostly reflects how long the core takes to recover from a wrong guess.

That plateau sits at roughly 3.5 to 4 ns of extra time per branch on Zen 5 and roughly 3 to 3.5 ns on Zen 6 so Zen 6 is paying less for each branch it gets wrong, which speaks to AMD reducing the latency of a branch mispredict compared to Zen 5.

Zen 6 has also improved the ability of the core to do 2 branches per cycle with Zen 6 able to do 2 branches per cycle within the L1 instruction cache.

[![](https://substackcdn.com/image/fetch/$s_!oG7B!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F92cb7d6d-0772-42c7-9200-b5c2dd4c93bb_977x526.png)](https://substackcdn.com/image/fetch/$s_!oG7B!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F92cb7d6d-0772-42c7-9200-b5c2dd4c93bb_977x526.png)

When the footprint exceeds the L1i, then it falls to just a single branch per cycle for the rest of the BTB except for the 64B test which goes up to 4 cycles per branch for the L2 BTB.

When comparing the BTB of Zen 6 to Zen 5, Zen 6 appears to have focused on improving the speed of the BTB compared to beefing up the capacity.

[![](https://substackcdn.com/image/fetch/$s_!24kn!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1cac7812-c368-4eba-8911-f8e31f3e59dd_2307x917.png)](https://substackcdn.com/image/fetch/$s_!24kn!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1cac7812-c368-4eba-8911-f8e31f3e59dd_2307x917.png)

The accuracy of Zen 6’s Branch Predictor Unit is very similar to Zen 5’s while also being faster which is a good thing considering that Zen 5 already had a very accurate Branch Predictor.

Zen 6 carries forward the same dual decode cluster schema that Zen 5 introduced but, unlike Zen 5 where a single thread could only effectively use a single decode cluster, now with Zen 6 a single thread can use both decode clusters.

[![](https://substackcdn.com/image/fetch/$s_!0X5u!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F362e75a6-f958-4fde-be89-f692cabd76d0_1417x709.png)](https://substackcdn.com/image/fetch/$s_!0X5u!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F362e75a6-f958-4fde-be89-f692cabd76d0_1417x709.png)

Outside of the L1i, Zen 6 looks very similar to Zen 5’s behavior on the instruction side of things.

The execution engine of Zen 6 is overall very similar to Zen 5, but there are some important differences to be mindful of.

[![](https://substackcdn.com/image/fetch/$s_!O9I-!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff142a3ba-44cd-4928-a9b3-bbd87d6c2d22_1365x1184.webp)](https://substackcdn.com/image/fetch/$s_!O9I-!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff142a3ba-44cd-4928-a9b3-bbd87d6c2d22_1365x1184.webp)

On the integer side, Zen 6 brings back Zen 4’s NOP pairing behavior, retiring up to 16 NOPs per cycle. Zen 6 also comes closer to theoretical peak throughput for simple integer instructions than Zen 5, sustaining 5.96 64-bit adds per cycle against Zen 5’s 5.25 with the same six ALUs.

AMD has reduced the number of integer multiply units from three to two, with PDEP and PEXT throughput also falling from three instructions per cycle to two. According to AMD, Zen 5’s third integer multiply unit did not justify its area cost.

[![](https://substackcdn.com/image/fetch/$s_!e2zc!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8dfd7d9a-3bee-408d-b28d-ed8cd3be6bc9_1418x738.png)](https://substackcdn.com/image/fetch/$s_!e2zc!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8dfd7d9a-3bee-408d-b28d-ed8cd3be6bc9_1418x738.png)

On the vector side, the floating-point units are unchanged, with Zen 6 still handling two 512-bit FADDs, FMULs, or FMAs per cycle. The change is in VNNI, where Zen 6 doubles the number of units from two to four and our 512-bit VNNI test goes from 1.99 to 3.88 instructions per cycle, with Zen 6c matching it. VNNI instructions are useful for operations like calculating the inner loop of quantized inference as well as for video codecs using them for video filters as well.

Moving over to the other notable change, Zen 5 could already perform four per cycle, but our test shows only two of them could be vector loads, regardless of if they were 256 or 512 bit loads. Zen 6 raises 256-bit loads to four per cycle on both cores while leaving 512-bit loads at two, so both widths now reach the same 128 bytes per cycle which matters because most shipped vector code still targets AVX1/2.

In the past the difference between the Classic cores and the Dense cores in terms of the architecture were, for all intents and purposes, functionally identical with the biggest changes being in the number of cores on a CCD and the amount of L3 cache per core. With the Zen 6 family, that has changed.

Starting inside of the core, the changes occur after micro-op dispatch and seemingly end after the load queue.

![](https://substackcdn.com/image/fetch/$s_!bjoI!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb906e342-aadb-45ef-9f80-716e6f69feb2_2191x262.png)

The main differences between Zen 6 and Zen 6c lie in the capacities of their out-of-order structures. Several of these structures are larger on Zen 6c, allowing it to keep more work in flight and absorb more latency than Zen 6. The reorder buffer grows from 576 to 640 entries, the integer register file from 288 to 352, the vector scheduler from 138 to 180, and the load queue from 280 to 330.

[![](https://substackcdn.com/image/fetch/$s_!_Lrh!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1159fcc-3ba0-4ab3-957a-b444fff2038c_2032x1382.png)](https://substackcdn.com/image/fetch/$s_!_Lrh!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa1159fcc-3ba0-4ab3-957a-b444fff2038c_2032x1382.png)

The differences suggest that AMD is specializing the two designs. Zen 6 appears tuned for high frequency, while Zen 6c has more capacity to tolerate comparatively high memory latency.

[![](https://substackcdn.com/image/fetch/$s_!Tce3!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F631c910d-0976-4d8a-9f7c-16188456e5cc_2032x1382.png)](https://substackcdn.com/image/fetch/$s_!Tce3!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F631c910d-0976-4d8a-9f7c-16188456e5cc_2032x1382.png)

Zen 6 and Zen 6c use 12-core and 32-core CCDs, respectively, but both provide 4 MB of L3 cache per core. That gives the 12-core CCD 48 MB of L3 cache and the 32-core CCD 128 MB.

[![](https://substackcdn.com/image/fetch/$s_!naz1!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb33907f2-c445-46cd-b7d3-942814d9da4a_1417x768.png)](https://substackcdn.com/image/fetch/$s_!naz1!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb33907f2-c445-46cd-b7d3-942814d9da4a_1417x768.png)

[![](https://substackcdn.com/image/fetch/$s_!noA_!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd46fd82d-44d2-426b-86fa-bc7e865729ef_1417x767.png)](https://substackcdn.com/image/fetch/$s_!noA_!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd46fd82d-44d2-426b-86fa-bc7e865729ef_1417x767.png)

L3 latency has increased from about 47 cycles on Zen 5 to 55 cycles on Zen 6 and 66 cycles on Zen 6c. The larger caches and greater number of mesh stops likely contribute to the increase. In nanoseconds, Zen 6’s higher clock hides most of the increase, at 10.7 ns against 10.3 ns on the Zen 5-based EPYC 9355P, while Zen 6c’s lower clock and larger CCX put it at 16.4 ns.

[![](https://substackcdn.com/image/fetch/$s_!ZqTi!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F85e4f4a3-ad42-4e77-aa6e-6d8612854e66_1417x708.png)](https://substackcdn.com/image/fetch/$s_!ZqTi!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F85e4f4a3-ad42-4e77-aa6e-6d8612854e66_1417x708.png)

[![](https://substackcdn.com/image/fetch/$s_!AsrM!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F69556c91-5e6a-4b60-8a02-1b667633b8b2_1418x709.png)](https://substackcdn.com/image/fetch/$s_!AsrM!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F69556c91-5e6a-4b60-8a02-1b667633b8b2_1418x709.png)

Moving over to bandwidth, we see that Zen 6 fixes Zen 5’s odd L2 bandwidth behavior, where a core pulled only about 42 bytes per cycle across most of the L2’s capacity, in Zen 6 the read path now reliably delivers 64 bytes per cycle. The rest of the cache hierarchy is largely unchanged in bandwidth per cycle.

Store-to-load forwarding is seemingly unchanged from Zen 5 and is where the behavior of Zen 6 and Zen 6c re-unify.

[![](https://substackcdn.com/image/fetch/$s_!QoPw!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F73e67dfc-f241-4f8f-8adc-621a4f73a3b1_1365x407.webp)](https://substackcdn.com/image/fetch/$s_!QoPw!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F73e67dfc-f241-4f8f-8adc-621a4f73a3b1_1365x407.webp)

EPYC 9686F (Zen 6) STLF test

[![](https://substackcdn.com/image/fetch/$s_!Cwp0!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F10a0e23e-f215-4434-b5be-b47e239850c3_1536x458.webp)](https://substackcdn.com/image/fetch/$s_!Cwp0!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F10a0e23e-f215-4434-b5be-b47e239850c3_1536x458.webp)

EPYC 9996 (Zen 6c) STLF test

[![](https://substackcdn.com/image/fetch/$s_!ZGc8!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ffb80deba-7b37-43c3-a0a8-a30c8fbc768a_2718x1321.webp)](https://substackcdn.com/image/fetch/$s_!ZGc8!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ffb80deba-7b37-43c3-a0a8-a30c8fbc768a_2718x1321.webp)

Zen 5 STLF test

A 32-bit load from the same address as a 64-bit store completes with effectively zero latency which lets Zen 6 handle two dependent store-load pairs per cycle. Loads fully contained within the store complete in 7 cycles regardless of alignment with partial overlaps costing 14 cycles, matching Zen 5.

The EPYC 9686F is near the top of our estimated single-threaded SPEC CPU 2026 results. It roughly matches the Ryzen 9 9950X3D2, AMD’s highest-end client CPU from the previous generation, in integer rate and beats it in floating-point rate. At a matched 4.1 GHz, the 9686F leads the 9950X3D2 by about 4% in integer and 11% in floating point, despite the X3D part’s much larger L3.

[![https://media.discordapp.net/attachments/809099718189121536/1569240935466803300/image.png?ex=6af2bd02&is=6af16b82&hm=1233642c0815dc5a6265c7be59547965aa82df42f26fcb8e10f63055f641f2d8&=&format=webp&quality=lossless&width=2616&height=1359](https://substackcdn.com/image/fetch/$s_!GPq_!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa7e2762f-c9ce-4fd2-ad96-5209f2b7d9b7_1744x906.webp "https://media.discordapp.net/attachments/809099718189121536/1569240935466803300/image.png?ex=6af2bd02&is=6af16b82&hm=1233642c0815dc5a6265c7be59547965aa82df42f26fcb8e10f63055f641f2d8&=&format=webp&quality=lossless&width=2616&height=1359")](https://substackcdn.com/image/fetch/$s_!GPq_!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa7e2762f-c9ce-4fd2-ad96-5209f2b7d9b7_1744x906.webp)

The EPYC 9996 is no slouch either. It roughly matches the previous-generation EPYC 9575F in single-threaded performance, despite running at lower clocks and having four times as many cores. It also edges out NVIDIA’s Vera in integer and leads it by about 20% in floating point.

[![](https://substackcdn.com/image/fetch/$s_!Sxqq!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F876774f6-7ec0-4b2e-bedf-338b28bfd808_2302x920.png)](https://substackcdn.com/image/fetch/$s_!Sxqq!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F876774f6-7ec0-4b2e-bedf-338b28bfd808_2302x920.png)

The EPYC 9686F leads on almost every subtest and finishes about 18% ahead of the EPYC 9996 in both suites. It scores 6.20 versus 5.25 in integer and 10.6 versus 8.99 in floating point, with some of its largest leads in 714.cpython, 766.femflow, and 772.marian.

Against the Zen 5-based EPYC 9575F, the 9996 scores 5.25 versus 5.13 in integer and 8.99 versus 9.01 in floating point while running about 900 MHz slower. The 9996 also edges out the 9686F in 721.gcc and 765.roms, two subtests where the 9686F’s higher clock provides no advantage.

![](https://substackcdn.com/image/fetch/$s_!MAal!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe1956404-6e80-42f0-82fd-9322502a842c_5005x1148.png)

Top-down breakdowns of pipeline slots on the two Zen 6 chips show where the 9686F’s lead comes from. The integer-rate profiles look almost identical in core-bound subtests. In 750.sealcrypto, 710.omnetpp, 735.gem5, and 753.ns3, retiring slots account for nearly the same share on both chips, with differences of less than one percentage point.

Memory-heavy subtests show larger gaps. Moving from the EPYC 9996 to the EPYC 9686F, backend memory stalls rise from 37.9% to 50.0% of pipeline slots in 729.abc, from 19.0% to 34.1% in 723.llvm, and from 24.8% to 33.2% in 708.sqlite. The share of retiring slots falls accordingly. Zen 6c’s lower clock and larger shared L3 cache may help explain its lower memory-stall share.

[![https://media.discordapp.net/attachments/1329409886676848660/1569271555379175535/image.png?ex=6af2d986&is=6af18806&hm=cdc4d18e51b664a0098b0958ffe6992b13c9f835fa23072ae98cc20e3f1f9375&=&format=webp&quality=lossless&width=2997&height=870](https://substackcdn.com/image/fetch/$s_!a7Y6!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F94b61755-8643-479d-bac2-83a81c1582e1_2997x870.webp "https://media.discordapp.net/attachments/1329409886676848660/1569271555379175535/image.png?ex=6af2d986&is=6af18806&hm=cdc4d18e51b664a0098b0958ffe6992b13c9f835fa23072ae98cc20e3f1f9375&=&format=webp&quality=lossless&width=2997&height=870")](https://substackcdn.com/image/fetch/$s_!a7Y6!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F94b61755-8643-479d-bac2-83a81c1582e1_2997x870.webp)

The floating-point profiles look similar for most subtests. The retiring share in 731.astcenc, 772.marian, and 767.nest differs by about one percentage point or less, and 709.cactus spends over half its slots stalled on front-end bandwidth on both chips.

The larger gaps again appear in memory-heavy subtests. Moving from the EPYC 9996 to the EPYC 9686F, backend memory stalls rise from 42.5% to 55.3% of slots in 765.roms, from 60.9% to 67.6% in 749.fotonik3d, and from 17.8% to 33.8% in 782.lbm. These are also tests where the 9686F’s higher clock helps least: it trails the 9996 in 765.roms, roughly ties in 749.fotonik3d, and leads by about 11% in 782.lbm, despite clocking up to roughly 29% higher.

[![](https://substackcdn.com/image/fetch/$s_!q4cv!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8a4638d3-0fca-4cbe-a9c8-6e170555a4d1_2935x863.png)](https://substackcdn.com/image/fetch/$s_!q4cv!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8a4638d3-0fca-4cbe-a9c8-6e170555a4d1_2935x863.png)

Running the 9686F at 4.1 GHz to match the 9996 removes the clock advantage, and the overall picture flips: the 9686F scores 4.92 and 8.64 at that clock, putting Zen 6c about 6.7% ahead per clock in integer and 4% in floating point. At the same clock speed, Zen 6 and Zen 6c look almost identical in core-bound integer subtests. In 750.sealcrypto, retiring slots account for 67.3% of the total on the EPYC 9686F versus 67.6% on the EPYC 9996.The floating-point profiles also look similar in several subtests at matched clocks. Both chips spend over half their pipeline slots stalled on frontend bandwidth in 709.cactus. Their retiring shares differ by about two percentage points in 722.palm and 766.femflow.

The 9686F still spends a larger share of its pipeline slots stalled on memory. Backend memory stalls account for 50.3% versus 42.5% on the 9996 in 765.roms, 24.9% versus 17.8% in 782.lbm, and 63.3% versus 60.9% in 749.fotonik3d. Zen 6c’s 128 MB L3 cache is likely serving more of these requests than Zen 6’s 48 MB cache, reducing the need to access main memory.

The clearest difference is in frontend latency stalls, which account for a much larger share of slots on the 9686F in subtests with large codebases. In 727.cppcheck, that share is 35.2% versus 16.6% on the 9996; in 721.gcc, it is 25.8% versus 10.0%. With clock speed matched and both designs sharing the same frontend, those stalls most likely reflect differences in the cache and memory hierarchy.

[![](https://substackcdn.com/image/fetch/$s_!l7sZ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0e773bb0-2977-4a0b-9a1c-ded7975bf1e8_2929x858.png)](https://substackcdn.com/image/fetch/$s_!l7sZ!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0e773bb0-2977-4a0b-9a1c-ded7975bf1e8_2929x858.png)

The floating-point profiles also look similar in several subtests at matched clocks. Both chips spend over half their pipeline slots stalled on frontend bandwidth in 709.cactus. Their retiring shares differ by about two percentage points in 722.palm and 766.femflow.

The 9686F still spends a larger share of its pipeline slots stalled on memory. Backend memory stalls account for 50.3% versus 42.5% on the 9996 in 765.roms, 24.9% versus 17.8% in 782.lbm, and 63.3% versus 60.9% in 749.fotonik3d. Zen 6c’s 128 MB L3 cache is likely serving more of these requests than Zen 6’s 48 MB cache, reducing the need to access main memory.

The Zen 6 Family improves upon its predecessor in various ways, an upgraded frontend allows one thread to utilize both decoders, while a faster branch predictor makes it more forgiving on misses, all tied together with larger OoO structures allowing it to better hide memory stalls. All of this coming together to provide a generational uplift from Zen 5.

However, I think the more interesting change is the split between Zen 6 and Zen 6c. The classic and dense cores now differ within the out-of-order engine, with Zen 6 appearing tuned for higher clocks and Zen 6c providing more capacity to absorb latency. The shared foundation is still clear, but those differences make the choice between them more dependent on workload behavior.

The performance results are encouraging as well with the estimated single-threaded SPEC CPU 2026 scores, the EPYC 9686F leads the Zen 5-based EPYC 9575F by about 21% in integer and 18% in floating point. The EPYC 9996 roughly matches the 9575F despite running about 900 MHz slower which are strong results for both core designs.

AMD’s answer to the classic-versus-dense question used to be the same core at two densities but now with Zen 6 it is two cores, each shaped for where it will run. That now leaves me wondering what AMD will do with the Zen 7 Family and if we will see even more specialization of the same basic core design or if we will see a larger split of the designs.
