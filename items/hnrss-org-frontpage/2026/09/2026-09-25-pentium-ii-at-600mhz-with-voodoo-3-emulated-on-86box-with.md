---
title: Pentium II at 600Mhz with Voodoo 3 Emulated on 86Box with M6 Mac Mini
link: https://nyaa.sh/reviews/mac-mini-m6-emulation
source: hnrss-org-frontpage
published: 2026-09-25T07:27:45Z
updated: 2026-09-25T07:27:45Z
first_seen: 2026-09-25T13:39:35.685325101Z
authors:
- hugh4life
summary: 'Article URL: https://nyaa.sh/reviews/mac-mini-m6-emulation Comments URL: https://news.ycombinator.com/item?id=49841285 Points: 131 # Comments: 53'
content: extracted
html: 2026-09-25-pentium-ii-at-600mhz-with-voodoo-3-emulated-on-86box-with.html
preview:
  file: 2026-09-25-pentium-ii-at-600mhz-with-voodoo-3-emulated-on-86box-with.preview-4e159376c756.webp
  width: 256
  height: 144
  color: '#adc9e9'
images:
- source: https://nyaa.sh/images/mac-mini-m6-emulation.webp
  original:
    file: 2026-09-25-pentium-ii-at-600mhz-with-voodoo-3-emulated-on-86box-with.image-e2d346e65ab3.webp
    width: 1672
    height: 941
  color: '#a5cbf4'
- source: https://nyaa.sh/images/m6_86box_650mhz_p2_cb2000.webp
  original:
    file: 2026-09-25-pentium-ii-at-600mhz-with-voodoo-3-emulated-on-86box-with.image-101d208d72e2.webp
    width: 1136
    height: 967
  variants:
  - file: 2026-09-25-pentium-ii-at-600mhz-with-voodoo-3-emulated-on-86box-with.image-50a29467ce2d.webp
    width: 320
    height: 272
  color: '#c5c4c4'
- source: https://nyaa.sh/data/86box/86box_w98_cpu_settings.webp
  original:
    file: 2026-09-25-pentium-ii-at-600mhz-with-voodoo-3-emulated-on-86box-with.image-663c91ca256b.webp
    width: 832
    height: 612
  color: '#eaeaea'
---

## [Why 86Box Cares About One Core](https://nyaa.sh/reviews/mac-mini-m6-emulation#why-86box-cares-about-one-core)

[86Box](https://86box.net/) emulates an old PC at the hardware level. CPU timing, chipset behaviour, ISA and PCI buses, graphics chipsets, sound devices, and disk controllers all matter to getting period software to behave properly. That does not guarantee identical performance to the original hardware, as the Cinebench results below illustrate. Frankly the project is fascinating and the effort has me in awe, but that accuracy is expensive, and almost all of the cost lands on a single host thread.

The practical consequence is quite straightforward in that core count barely matters here. What really matters is how fast one core can run, and how long it can hold that speed without throttling. Luckily for the Mac Mini here that's exactly where Apple Silicon has been strongest, and where the M6 shines.

![A little overclock... the M6 running a 650MHz Pentium II in my custom 86Box 6.0 build, with Cinebench 2000 and Winamp. One or two audio underruns kept 650MHz from passing, so 600MHz remains the stable result.](https://nyaa.sh/images/m6_86box_650mhz_p2_cb2000.webp)

A little overclock... the M6 running a 650MHz Pentium II in my custom 86Box 6.0 build, with Cinebench 2000 and Winamp. One or two audio underruns kept 650MHz from passing, so 600MHz remains the stable result.

## [The Machines](https://nyaa.sh/reviews/mac-mini-m6-emulation#the-machines)

Every 86Box test uses the same machine configurations, the same disk images and emulated hardware. The comparison set is:

- **Mac Mini M6** (12-core CPU, 24GB, this review unit)
- **Mac Mini M4** (base 10-core CPU, 16GB)

Testing was using a slightly modified build of [86Box 6.0](https://github.com/86Box/86Box/releases/tag/v6.0), released on May 31, 2026. The team improved CPU emulation performance on ARM hosts and added an ARM64 just-in-time recompiler for Voodoo graphics. That second change is particularly relevant here, since the Windows 98 machine is running an emulated Voodoo 3. Some credit certainly belongs to the software, Apple Silicon has fast cores, but 86Box is also getting better at using them.

I have not measured the uplift from an older 86Box version, given this is a review of the M6 and not 86Box.

## [Why 100% Is the Only Acceptable Number](https://nyaa.sh/reviews/mac-mini-m6-emulation#why-100-is-the-only-acceptable-number)

86Box reports an effective emulation speed as a percentage of its target speed. 100% means the host is keeping pace with the emulator's timing model, so anything less is a genuine problem. It does not guarantee that a benchmark will score exactly as it would on a physical CPU at the same clock.

Consistency also matters more than the average here. Even brief dips produce audible artefacts because sound hardware is fed in real time and starved buffers are heard as brief dropouts which are irritating. Basically any 86box setup that regularly dips below 100% will not feel right, and having adequate headroom on the system to comfortably emulate the target speed will be a much more stable experience.

The result is that turns each host machine into a ceiling rather than a score, and for any given emulated configuration there is a maximum CPU clock the host can sustain at a flat 100%. Because almost all of that work falls on one thread, single-threaded performance moves this ceiling substantially, which is the whole reason the M6 is interesting for this.

## [Test Method](https://nyaa.sh/reviews/mac-mini-m6-emulation#test-method)

To find the ceiling, I made a custom build from the same commit as the 6.0 release (build 9001), extending the frequency tables in **50MHz steps up to 800MHz**. The [Deschutes frequency table patch](https://nyaa.sh/downloads/86box-6.0-deschutes-500-800mhz.patch) is available if you want to try it yourself against that tagged release. It adds 500–800MHz entries with memory and cache timings scaled to retain approximately the same access latencies, and keeps the AT bus at 8.33MHz. The emulation code is otherwise unchanged. Both Macs used this build for the extended tests.

Some might argue a Pentium II at these speeds is not era appropriate. I argue it is just a little overclock!

The emulated machine is otherwise fixed across every run:

- **Slot 1 motherboard** with Pentium II (Deschutes), clock varied per run
- **256MB** of memory
- **Voodoo 3** emulated VGA with 16MB of video memory (2 threads)
- **Windows 98 SE**

![86Box CPU configuration for the emulated Pentium II machine](https://nyaa.sh/data/86box/86box_w98_cpu_settings.webp)

86Box CPU configuration for the emulated Pentium II machine

The load is deliberately a little awkward, **Cinebench 2000** running its CPU test while **Winamp 2.76** plays a 16-bit 44,100Hz PCM WAV in the background. The audio is a bit of an achor as it's a real-time consumer of the emulated hardware, so any moment it falls behind is immediately audible.

The pass condition is ultimately subjective but strict. If I hear a dropout, or the reported emulation speed falls below 100% at any point, the run fails.

## [Results](https://nyaa.sh/reviews/mac-mini-m6-emulation#results)

The base **M4 Mac Mini holds 500MHz**, with both Macs extremely stable throughout the full Cinebench 2000 and 3DMark 2000 SE runs at that speed. At **550MHz**, the M4 starts to hitch. They are slight interruptions in Cinebench, but more noticeable during the 3DMark demo, and enough to fail the run.

The M6 passes **550MHz** and **600MHz**, which is incredible. At 600MHz it held a flat 100% through both Cinebench 2000 runs and the 3DMark 2000 SE demo, the latter giving it **7–8 minutes of uninterrupted testing**. That makes the highest passing clock **20% higher than the M4's** in this setup.

The CB2000 scores, for anyone who cares:

#### Cinebench 2000 by emulated Pentium II clock

| Emulated clock | Cinebench 2000 | M6   | M4   |
| -------------- | -------------- | ---- | ---- |
| 300MHz         | 4.62 CB        | Pass | Pass |
| 350MHz         | 5.34 CB        | Pass | Pass |
| 400MHz         | 6.16 CB        | Pass | Pass |
| 450MHz         | 7.02 CB        | Pass | Pass |
| 500MHz         | 7.73 CB        | Pass | Pass |
| 550MHz         | 8.54 CB        | Pass | Fail |
| 600MHz         | 9.28 CB        | Pass | Fail |
| 650MHz         | 10.08 CB       | Fail | Fail |

One run per clock on the custom 86Box 6.0 build. A pass requires a flat 100% emulation speed with no audible dropouts for the whole run. Hover a result for the detail.

Those scores describe the emulated CPU at each clock. A completed render is not enough to pass, the 10.08 CB result at 650MHz came with one or two audible underruns. In reality, I think I may be being too firm on that run, and background activity could have caused the hitches. But staying true to the methodology, **600MHz is the result** and leaves a lick of headroom. The 800MHz option is there to extend the test range but clearly not a speed either Mac can sustain.

There is an interesting wrinkle when comparing these scores with real hardware. An [Ars Technica forum thread collecting Cinebench 2000 results](https://arstechnica.com/civis/threads/cinebench-2000-results.946288/) includes the following user-reported figures:

#### Period hardware, user-reported Cinebench 2000 scores

| Period hardware                               | Cinebench 2000 |
| --------------------------------------------- | -------------- |
| Pentium II 300MHz                             | 2.38           |
| Pentium II 450MHz                             | 4.35           |
| Celeron 800MHz                                | 7.52           |
| Celeron 533MHz overclocked to 760MHz (95 x 8) | 8.03           |
| Pentium III Coppermine 800MHz                 | 9.25           |
| Athlon Classic 600MHz                         | 7.66           |

User-reported results from the Ars Technica Cinebench 2000 thread. Different systems, memory and operating systems, so period context rather than a controlled comparison.

Our emulated 450MHz Pentium II scores **7.02 CB**, about **61% higher** than that physical PII 450MHz result. At 300MHz the gap is larger still, with 4.62 CB against 2.38 CB. Bizarrely, our 600MHz result of 9.28 CB lands almost exactly alongside that 800MHz Coppermine. These are individual forum submissions from different systems, so they provide period context rather than a controlled comparison, but the discrepancy is substantial. It's also worth a mention this benchmark version is very old and long before it became the popular benchmark it is today.

I do not yet know why, it's possible memory bandwidth and cache timing within the emulated machine might have something to do with it. There is some relevant history in [86Box's v3.0 release notes](https://86box.net/2021/12/01/86box-v3-0.html), which explain that P6 emulation was not fully accurate because of the complexity of out-of-order execution and L2 cache behaviour. Deschutes timings were tuned to get reasonably close to real hardware. But yeah, not sure.

The discrepancy is already present at 300MHz and 450MHz, below the entries added by my patch. For this review, **600MHz remains the highest passing setting in this 86Box configuration**, with a 20% higher stable clock than the M4. The CB2000 scores are useful for showing how that configuration scales, but I would not use them to claim equivalent performance across software on a real Pentium II or Pentium III.

Even the M4's 500MHz is remarkable alongside the [overclocked 9950X3D demonstration](https://www.youtube.com/watch?v=sec5WMOjhCY) I was using for context. I do not have a matched x86 run on this custom build, but the base M4 was already a much more capable retro machine than the original 450MHz limit let me establish.

The video below is from the earlier **450MHz** run on the M6, where OBS was also compositing and encoding 720p30 H.264. It documents that run, rather than the new 600MHz result.

3DMark 2000 SE running under 86Box on the Mac Mini M6, emulating a 450MHz Pentium II with a Voodoo 3. Emulation speed holds at 100% while OBS encodes the capture.

## [What the Host Is Actually Doing](https://nyaa.sh/reviews/mac-mini-m6-emulation#what-the-host-is-actually-doing)

86Box parks the work on two 'super' cores, visible as **cpu6 and cpu7**, with the rest of the package largely idle. On the M6 those two cores average about **4,483MHz** during the 450MHz Cinebench 2000 run and about **4,710MHz** during the 600MHz run, peaking at **4,788MHz**. Core activity climbs with the emulated clock too, from roughly **41%** on each busy core at 450MHz to **45 to 49%** at 600MHz, so there is still real headroom left at the highest passing clock. Whole package usage never passes **26%**.

The M4 running the same configuration keeps its busy cores closer to **3.7GHz**, and that gap is likely most of the reason it stops at 500MHz.

## [Where This Leaves the Mini](https://nyaa.sh/reviews/mac-mini-m6-emulation#where-this-leaves-the-mini)

For this specific use, the cheapest new Mac desktop is an unusually good machine. The base M4 already gives me a stable 500MHz Pentium II, the M6 takes that to **600MHz**, with intact audio and a full 3DMark demo at 100%. That is an outstanding result from a small, silent box under €1,600. It is the clearest case I have found so far where the M6's single-core lead translates into something you can actually feel rather than something you read off a chart.

Looking at the benchmark numbers, and anecdotal evidence available online from era appropraite benchmarks, this places my virtual system in Pentium III territory. **One day I'll look to try XP emulation in 86box.**

[Power and Thermals On hold until powermetrics reports M6 CPU package power. Check back.](https://nyaa.sh/reviews/mac-mini-m6-power-thermals)

 [Gaming Benchmarks Cyberpunk 2077 at native 1080p, where the 12-core GPU takes over.](https://nyaa.sh/reviews/mac-mini-m6-gaming)
