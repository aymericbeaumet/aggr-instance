---
title: Building a Linux GPU Driver for the M4 Mac Mini in One Month
link: https://codyho.dev/blog/gpu-driver/
source: hnrss-org-frontpage
published: 2026-09-15T19:30:03Z
updated: 2026-09-15T19:30:03Z
first_seen: 2026-09-16T01:36:55.400042967Z
authors:
- ADevWithAnIdea
summary: 'Article URL: https://codyho.dev/blog/gpu-driver/ Comments URL: https://news.ycombinator.com/item?id=49717638 Points: 154 # Comments: 95'
content: extracted
html: 2026-09-15-building-a-linux-gpu-driver-for-the-m4-mac-mini-in-one-month.html
preview:
  file: 2026-09-15-building-a-linux-gpu-driver-for-the-m4-mac-mini-in-one-month.preview-fda1e7b432e1.webp
  width: 256
  height: 144
  alt: Chrome and Firefox running three.js WebGL demos on our driver.
  color: '#858e85'
images:
- source: https://codyho.dev/images/blog/gpu-driver/threejs-webgl-demos.jpg
  original:
    file: 2026-09-15-building-a-linux-gpu-driver-for-the-m4-mac-mini-in-one-month.image-3b1f74492cba.jpg
    width: 1920
    height: 1080
  color: '#e6e8e6'
- source: https://codyho.dev/images/blog/gpu-driver/minecraft.png
  original:
    file: 2026-09-15-building-a-linux-gpu-driver-for-the-m4-mac-mini-in-one-month.image-62ccc00d2d08.png
    width: 1946
    height: 1095
  variants:
  - file: 2026-09-15-building-a-linux-gpu-driver-for-the-m4-mac-mini-in-one-month.image-37341500fcb3.webp
    width: 320
    height: 180
  - file: 2026-09-15-building-a-linux-gpu-driver-for-the-m4-mac-mini-in-one-month.image-cd4e7a2e5bf4.webp
    width: 640
    height: 360
  - file: 2026-09-15-building-a-linux-gpu-driver-for-the-m4-mac-mini-in-one-month.image-b3f465581cff.webp
    width: 960
    height: 540
  - file: 2026-09-15-building-a-linux-gpu-driver-for-the-m4-mac-mini-in-one-month.image-ce5d7118dbde.webp
    width: 1280
    height: 720
  - file: 2026-09-15-building-a-linux-gpu-driver-for-the-m4-mac-mini-in-one-month.image-8c200ae7d0ac.webp
    width: 1600
    height: 900
  - file: 2026-09-15-building-a-linux-gpu-driver-for-the-m4-mac-mini-in-one-month.image-e3f919f67f52.webp
    width: 1946
    height: 1095
  color: '#797873'
- source: https://codyho.dev/images/blog/gpu-driver/m1-firmware-abi.png
  original:
    file: 2026-09-15-building-a-linux-gpu-driver-for-the-m4-mac-mini-in-one-month.image-7426511ce8dd.png
    width: 2300
    height: 3114
  variants:
  - file: 2026-09-15-building-a-linux-gpu-driver-for-the-m4-mac-mini-in-one-month.image-9dda3bd56be0.webp
    width: 320
    height: 433
  - file: 2026-09-15-building-a-linux-gpu-driver-for-the-m4-mac-mini-in-one-month.image-1d4cf60e43d2.webp
    width: 640
    height: 867
  - file: 2026-09-15-building-a-linux-gpu-driver-for-the-m4-mac-mini-in-one-month.image-153746645920.webp
    width: 960
    height: 1300
  - file: 2026-09-15-building-a-linux-gpu-driver-for-the-m4-mac-mini-in-one-month.image-529f491050e2.webp
    width: 1280
    height: 1733
  - file: 2026-09-15-building-a-linux-gpu-driver-for-the-m4-mac-mini-in-one-month.image-081ffd84bf0e.webp
    width: 1600
    height: 2166
  - file: 2026-09-15-building-a-linux-gpu-driver-for-the-m4-mac-mini-in-one-month.image-785c201ba6f6.webp
    width: 2300
    height: 3114
  color: '#fbfbfb'
- source: https://codyho.dev/images/blog/gpu-driver/a18-pro-firmware-abi.png
  original:
    file: 2026-09-15-building-a-linux-gpu-driver-for-the-m4-mac-mini-in-one-month.image-964789e50217.png
    width: 2300
    height: 4246
  variants:
  - file: 2026-09-15-building-a-linux-gpu-driver-for-the-m4-mac-mini-in-one-month.image-a1d6dcbf9412.webp
    width: 320
    height: 591
  - file: 2026-09-15-building-a-linux-gpu-driver-for-the-m4-mac-mini-in-one-month.image-2b3c4d6063fc.webp
    width: 640
    height: 1181
  - file: 2026-09-15-building-a-linux-gpu-driver-for-the-m4-mac-mini-in-one-month.image-3ee7306d9037.webp
    width: 960
    height: 1772
  - file: 2026-09-15-building-a-linux-gpu-driver-for-the-m4-mac-mini-in-one-month.image-23448b2f016f.webp
    width: 1280
    height: 2363
  - file: 2026-09-15-building-a-linux-gpu-driver-for-the-m4-mac-mini-in-one-month.image-337078a64e6b.webp
    width: 1600
    height: 2954
  - file: 2026-09-15-building-a-linux-gpu-driver-for-the-m4-mac-mini-in-one-month.image-ac40486bb89d.webp
    width: 2300
    height: 4246
  color: '#fbfafa'
- source: https://codyho.dev/images/blog/gpu-driver/opengl-stack.png
  original:
    file: 2026-09-15-building-a-linux-gpu-driver-for-the-m4-mac-mini-in-one-month.image-05416d863f98.png
    width: 1116
    height: 2295
  variants:
  - file: 2026-09-15-building-a-linux-gpu-driver-for-the-m4-mac-mini-in-one-month.image-f1e3b711e358.webp
    width: 320
    height: 658
  - file: 2026-09-15-building-a-linux-gpu-driver-for-the-m4-mac-mini-in-one-month.image-d42da0d6159a.webp
    width: 640
    height: 1316
  - file: 2026-09-15-building-a-linux-gpu-driver-for-the-m4-mac-mini-in-one-month.image-1fcc24dd581f.webp
    width: 960
    height: 1974
  - file: 2026-09-15-building-a-linux-gpu-driver-for-the-m4-mac-mini-in-one-month.image-5800a0c8f853.webp
    width: 1116
    height: 2295
  color: '#fbfbfb'
- source: https://codyho.dev/images/blog/gpu-driver/opengl-test-results.png
  original:
    file: 2026-09-15-building-a-linux-gpu-driver-for-the-m4-mac-mini-in-one-month.image-d1209fa48ac6.png
    width: 1108
    height: 200
  variants:
  - file: 2026-09-15-building-a-linux-gpu-driver-for-the-m4-mac-mini-in-one-month.image-ec8c0cbedf16.webp
    width: 320
    height: 58
  - file: 2026-09-15-building-a-linux-gpu-driver-for-the-m4-mac-mini-in-one-month.image-84a3592cb981.webp
    width: 640
    height: 116
  - file: 2026-09-15-building-a-linux-gpu-driver-for-the-m4-mac-mini-in-one-month.image-5e1a4cc5e629.webp
    width: 1108
    height: 200
  color: '#020202'
- source: https://codyho.dev/images/blog/gpu-driver/goal-resume-spam-1.png
  original:
    file: 2026-09-15-building-a-linux-gpu-driver-for-the-m4-mac-mini-in-one-month.image-e486c84de2cb.png
    width: 1716
    height: 1212
  color: '#111317'
- source: https://codyho.dev/images/blog/gpu-driver/goal-resume-spam-2.png
  original:
    file: 2026-09-15-building-a-linux-gpu-driver-for-the-m4-mac-mini-in-one-month.image-46c1f5d767da.png
    width: 968
    height: 1238
  color: '#111317'
---

Previous blog post: [https://codyho.dev/blog/hypervisor-macbook-neo/](https://codyho.dev/blog/hypervisor-macbook-neo/)

## What We Did

**TL;DR:** Niklas and I built a fully OpenGL ES 3.0 compliant GPU driver for the M4 Mac Mini and MacBook Neo in about a month, a process which normally takes years. Here is Chrome and Firefox running WebGL on the M4 Mac Mini with working compositing:

![Chrome and Firefox running three.js WebGL demos on our driver.](https://codyho.dev/images/blog/gpu-driver/threejs-webgl-demos.jpg)

Most importantly, the driver is fast enough to run Minecraft at 200fps:

![Minecraft running at 212fps on the M4 Mac Mini.](https://codyho.dev/images/blog/gpu-driver/minecraft.png)

Building this driver involved reverse engineering the AGX’s (Apple’s name for the GPU) incredibly complicated firmware ABI and user-space components. This was all done in a transparent, verifiably clean room manner using well established techniques. The code is not yet ready for end users, but we are looking to get it to end users as soon as possible.

## How We Did It

Previously, I built a hypervisor to reverse engineer macOS. Now the goal became to actually do something useful with it, and what better target than writing a GPU driver. The GPU is effectively a requirement for any modern system, otherwise everything needs to be CPU rendered which is orders of magnitude slower and less power efficient. Our goal was to implement conformant OpenGL (and soon, Vulkan) drivers for the M4 Mac Mini and MacBook Neo.

Normally, building a GPU driver is an endeavor that takes years; our goal was to do it in days. It turns out that days was overly optimistic, but weeks is still a massive improvement. In those weeks we have:

- Reverse engineered the M4, A18 Pro, and (mostly) M5 user space using only live probing, discovering hardware-supported features and instructions not emitted by Apple’s driver
- Built a fully working user-space driver, including a new custom IR/shader compiler, command stream builder, and many more components
- Reverse engineered, from scratch, the full AGX firmware ABI using traces from the hypervisor I previously built
- Implemented a full Linux kernel driver for said firmware ABI

Throughout this process, we have not looked at any Apple binaries, only hardware traces (from our hypervisor) and shaders we built ourselves. For user-space graphics RE, we were careful to treat any required Apple blobs as opaque objects. We had a friend write documentation on these blobs [1](https://codyho.dev/blog/gpu-driver/#fn:1) so we could write a clean room implementation ourselves (which was mostly built by just blindly trying stuff until it worked). We have published all of our experiments so that anyone can verify the provenance of our work (see the twin `agx-re` repos under [Deliverables](https://codyho.dev/blog/gpu-driver/#deliverables)).

This blog post is divided into two parts, user and kernel space. This mirrors the split in all modern GPU drivers: the kernel is responsible for interfacing with the firmware, allocating buffers, and managing scheduling, while the actual contents of those buffers and what is being scheduled are opaque. User space is responsible for actually understanding how the GPU works and filling those buffers with stuff.

## Kernel Space

On Apple Silicon, the kernel driver does not interface directly with the hardware. Instead, it talks to the GPU firmware running a custom RTOS called RTKit. That means that the first step to a kernel driver is not talking to hardware, it’s figuring out the firmware ABI.

The firmware ABI was by far the most annoying part of this project, because rather than doing the sane thing of coming up with a reasonable ABI with nice interfaces, Apple essentially took a regular kernel driver, cut it in half, and then put half of it in the AGX and called it firmware, with the other half of the kernel driver communicating using shared structs in memory. Many of these structs have firmware owned fields (which we must never modify and which we must learn from reverse engineering) interleaved with host controlled fields. For an idea of how complicated the ABI is, this is what the shared memory tree looks like on the M1/M2:

![The M1/M2 firmware ABI](https://codyho.dev/images/blog/gpu-driver/m1-firmware-abi.png)

Asahi Lina famously figured all of this out over grueling 12-hour days to build the M1/M2 kernel driver, an amazing technical accomplishment. Unfortunately, the A18 Pro firmware ABI (I started my RE work on the MacBook Neo and later pivoted to the M4 Mac Mini) is *significantly* more complicated than the already very complicated M1 firmware ABI:

![The A18 Pro firmware ABI](https://codyho.dev/images/blog/gpu-driver/a18-pro-firmware-abi.png)

**What the F@!#, Apple.** Note how the A18 has:

- 1.5x as many structs
- twice as many pointers
- a significantly more complicated process for submitting work

There are many other issues that add friction to the RE process [2](https://codyho.dev/blog/gpu-driver/#fn:2). I did have some documentation on the firmware ABI, but it was highly incomplete and honestly was not very useful [3](https://codyho.dev/blog/gpu-driver/#fn:3).

My approach was simple and based on the approach used to successfully reverse engineer the M1/M2 machines: watch what macOS did, replay it, then try to do it ourselves, which is made possible by the hypervisor.

When I described this approach to the LLM, it took replay extremely literally: the first thing it did was wait for the first firmware visible event (these are called “kicks”), then *saved a copy of the entire GPU memory state*. After a reboot, it copied the saved memory state straight back into host memory, performed the kick, and saw the output pages change. It would then try to reconstruct these objects in code, following all the pointers and making sense of the contents. Over successive experiments, Codex would reduce the number of pages it copied until there was no more replayed state and everything was built from source. [4](https://codyho.dev/blog/gpu-driver/#fn:4) Amazingly, I noticed Codex had good taste regarding when it should poke the hardware some more and when it should just run the hypervisor and capture the state itself.

There were three major issues, and all were caused by our inability to get a clean capture of host work:

The first issue was render work submitted *after* the GPU firmware started. We could prestage work before the firmware started, start the GPU, and that work would be completed as expected, but once the firmware started any work submitted would just be ACKed and retired without actually doing anything. Once the firmware has started, capturing state is much harder because everything becomes dynamic and the firmware becomes a stateful object with state you can’t easily replay.

I had to step in at this point and examine Codex’s process. It turns out it was trying to replay a capture very late in the AGX’s lifecycle, where there had already been many previous events. When I told it to choose a capture far earlier in the AGX’s lifecycle, the very first capture after firmware start, Codex was able to almost immediately discover the issue (it was missing a single byte descriptor). This took a few days.

The second, and only major blocking, issue was compute. The AGX, broadly, supports two kinds of work: compute and render. In the regular GUI path, compute work is only scheduled after a significant amount of render work was already executed. Thus, it took a long time to get a clean capture of a compute workload, and when Codex finally did it was 336 MB and impossible to replay (it tried, for a long time). It also tried to construct the objects itself by looking at the capture, and spent over a week doing this, but was ultimately unsuccessful. There was just too much nonsense to sift through. This was exacerbated by issues on my side– after getting render working, I expected that submitting compute work would be simpler (the firmware ABI for compute is indeed simpler, so I was correct here), but lost my humility and thought it would be a cakewalk that would only take a few hours. Thus, I didn’t scaffold out the task properly for the LLM.

The fix actually was given to me in another Codex session. In essence:

1. Disable the GUI by booting into single-user mode; this means no render work would be done.
2. Install a LaunchDaemon to run at the earliest possible point, the moment Metal (Apple’s proprietary graphics framework) became available.
3. Run a tiny Metal program that we supplied
4. Capture and replay this tiny, pure compute trace.

The trace was captured successfully. Within a few hours, Codex had deconstructed it, and within a few days, Codex had compute working. As for why the original compute codebase didn’t work… Codex has no idea. The working one and the broken one look very similar.

In hindsight, this should have been the strategy from the start– smallest possible capture, run in single-user mode so as not to perturb results. I learned from my mistakes here for the final issue:

Partial renders ended up being one of the hardest things to figure out. They occur when the Tiled Vertex Buffer (TVB) isn’t large enough to store the current geometry (ie, there’s just too many triangles to draw). In these cases, there are two options, and the driver needs to support both: either increase the size of the TVB, or perform a partial render, ie, render part of the geometry, then reload the buffer with the rest of the triangles, and finish the partial render. These partial renders turned out to be very, very finicky, even more so than the rest of the work because they essentially mean adding save and resume to the GPU driver.

The workflow I discovered earlier came in very handy here. Codex was able to replay one partial render transaction, and then modified our Metal shader to perform multiple partial renders (this is pretty easy by just hammering a single tile with thousands of triangles until a partial render is triggered) and then learned how to replay these. Once Codex had a successful replay, it was only a matter of time until it learned how to build it ourselves.

### Building the Kernel Driver

Moving from a Python prototype driver to a fully featured Linux driver took three days, and one of those days was almost totally wasted because Codex, for some reason I still do not understand, chose to tackle partial renders first (by far the hardest task) instead of doing compute first (the easiest task). Once I told it to do compute first, everything went smoothly.

At all high level, the entire process was, basically:

1. Rewrite the existing `drm-shim` in Rust following the exact same pattern; this gives us a synchronous Rust driver.
2. Rewrite the frontend to be asynchronous; the actual GPU submission remains synchronous.
3. Refactor the GPU submissions to be asynchronous and, instead of polling, listen for firmware events and associate work with a fence
4. Implement some low-hanging optimizations, such as batched work submission.

This is all pretty routine engineering work that LLMs are definitely capable of.

The only notable thing I found is that Codex aggressively used the hypervisor to debug why its code didn’t work, including capturing the full address space and comparing it to known good samples. This sort of systematic debugging is why Codex is by far my favorite coding agent.

## User Space

The A18 Pro user space is very different from the M1/M2; it has new descriptor formats, a new ISA, and a bunch of other new things. Aside from being a tile based deferred renderer designed to run Metal, it’s just a different GPU.

The good news is user-space RE has a very well defined process. Simply write a small Metal program, compile it, run it, see what changed, then take it apart and start fiddling with the bits until we understand what all of them do. If you’re thinking this sounds like the sort of boring, repetitive, rote work that LLMs are very good at, you would be correct.

The RE work occurred in two phases. For the first phase, I had Claude look at every possible Metal program it could find and trying to build a disassembler, assembler, and understand the format of all the other descriptors/command streams/etc required for the GPU driver. I had Claude enumerate everything, including stuff Linux can’t use (like tessellation) for completeness. This was successful, but just because Claude could disassemble and then reassemble programs doesn’t mean it knew how to build one itself. When trying to close this gap, ie, understand every instruction enough to actually be able to compile our own arbitrary programs, Claude did a horrible job and made basically zero progress.

At this point, Niklas finished his `drm-shim` for the M4 Mac Mini and joined me for the second phase of user-space RE. We had two different approaches to actually finishing the user-space driver:

My approach was to prioritize hardware RE, and focus on just figuring out how the hardware and all the instructions worked. Then I would write a spec and let the LLM implement it, hopefully ending with full OpenGL and Vulkan compliance. This means that most of my LLM’s time was spent writing experiments on hardware, not actually implementing Mesa code. The idea was that once I understood the hardware, everything else followed.

Niklas took a different approach, that I’d describe as “Mesa first”. Essentially, he tried to build out Mesa first and would only do RE in order to build out some functionality. His time was split between building and testing Mesa, and performing RE.

It turns out that Niklas made *significantly* faster progress than I did, because my agent would spend a lot of time on minor, inconsequential tasks in the name of completeness. By contrast, his agent was grounded by the need to actually build Mesa, so it used time and resources a lot more effectively. He ended up moving so much faster than me that ended up just trying to support his work by investigating any behavior he didn’t yet understand.

This was one big limitation of Codex I noticed. The best word I can think of to describe it is “pedantic”– it is extremely thorough all the time, which can be a major benefit in some scenarios, but other times it gets stuck in the weeds on some random tangent to the detriment of the overall goal.

During our RE, we found behavior that was supported by the hardware but *not* supported by Metal; this was found by directly messing with the bits of the different instructions and extrapolating what *might* exist based off what we know did exist, just like Alyssa Rosenzweig did when REing the M1/M2. This included:

- A native single-instruction 64-bit add
- Anisotropy to 128x (Metal caps at 16x)
- A new mode of the matrix unit
- 7-bit immediate support for `uniform_mov`

### Mesa Development

There are a few things that massively work in our favor when building out user-space graphics. Most notably, the Khronos compatibility test suite (CTS) is already an exhaustive corpus of tests our driver must pass. In other words, the hardest and most sensitive part of working with LLMs, giving them good tests to ground them, is already done for us.

Additionally, Mesa already has great abstractions that make our lives significantly easier. This is what the modern OpenGL stack on Linux looks like:

![Diagram of the modern Linux graphics stack.](https://codyho.dev/images/blog/gpu-driver/opengl-stack.png)

All we have to do is translate between Gallium, Mesa’s internal API, and AGX hardware semantics. One of the biggest parts of this process is translating from NIR, Mesa’s internal IR that’s quite similar to LLVM IR, to the AGX’s proprietary ISA. As a bonus, this compiler can be reused for a future Vulkan driver.

Niklas was able to slowly iterate through OpenGL features, REing the user space as he went, until he finally achieved full OpenGL ES 3.0 compliance (the unsupported tests are optional extensions):

![The OpenGL ES 3.0 conformance test suite passing on our driver.](https://codyho.dev/images/blog/gpu-driver/opengl-test-results.png)

Throughout the process, we benefited from the existing M1/M2 work: while the exact hardware semantics differ, the overall shape remains similar and thus many of the right tradeoffs/decisions were already made for us. Throughout my time building this driver, it became clear that Alyssa Rosenzweig and the others who built the M1/M2 driver are utter wizards– hats off to them!

## Deliverables

Mesa: [https://github.com/niklassheth/mesa](https://github.com/niklassheth/mesa)

Linux Kernel Driver: [https://github.com/GravityLinux/linux/gravity-m4](https://github.com/GravityLinux/linux/tree/gravity-m4)

User-Space RE Documentation (horrible pile of LLM slop, but functional): [Cody](https://github.com/ADevWithAnIdea/agx-re) [Niklas](https://github.com/niklassheth/agx-re)

## Remaining Work

Vulkan 1.4, OpenGL 4.6, OpenGL ES 3.2, OpenCL 3.1, Direct3D 12 (via Proton), and ray tracing are all in scope. We want our driver to be as good as the best graphics drivers in the world.

Additionally, Niklas and I want to upstream all of this, but there are some significant obstacles. We used the unmodified Asahi UAPI, so there are no policy issues with Mesa upstreaming, but it needs far more testing, human review, and to be refactored into a reviewable PR. We also expect significant skepticism given that this is likely the first ever fully LLM-written GPU driver, and that our code will be held to a *higher* standard than human-written code. We are ready for these challenges, but they are primarily human and nontechnical, which LLMs cannot help with.

The Linux kernel driver will be an even bigger problem, since the M1/M2 driver is not yet upstream, and practically we are not in a position to change this. We think the best approach here is just to wait for that driver to be upstreamed, and then upstream our driver after M1/M2 is upstream (after all the required refactoring + review + decomposition + whatever). This may be a while unfortunately.

## When Can I Use It?

Patience young grasshopper, we’re looking forward to getting this code into your hands soon enough, and the wait may be much less than you may expect. After all, the apple doesn’t fall far from the tree.

## Join Us

If you’re interested in being a part of this, we invite you to join our [Discord Server](https://discord.gg/2UeuP9x632). Feel free to come by to discuss ideas, chat, or just hang out!

## Addendum: Really Sam?

I used Codex with GPT-5.6 Sol (later GPT-6 Astra when it came out) for the kernel RE task. One of the biggest issues I hit was the overly aggressive cybersecurity restrictions (I am not enrolled in trusted access).

Ninety-nine percent of the time a simple `/goal resume` or “keep going” was enough to have the LLM continue (which also shows the restrictions were overly aggressive), but they still broke my unattended workflow. I coded the fastest, dumbest possible solution: a daemon that takes a screenshot every minute, diffs it against the last screenshot, and if identical (because Codex stopped making progress) types `/goal resume`. I accidentally left it on in some group chats:

![The /goal resume daemon spamming a group chat.](https://codyho.dev/images/blog/gpu-driver/goal-resume-spam-1.png)

![It kept going.](https://codyho.dev/images/blog/gpu-driver/goal-resume-spam-2.png)

That said, GPT-6 Astra and GPT-5.6 Sol are absolutely insane and by far the best performers at firmware ABI RE, so this hack was more than worth it.

## Addendum: M4 vs A18 Pro vs M5

As far as I can tell, the user-space implementations of the M4 and A18 Pro are effectively identical, with the only difference I can find being one value is slightly larger on the M4, consistent with it having more cores. The firmware ABIs of the two differ significantly: the second RTKit coprocessor on the A18 Pro makes everything more complicated. In this way, the A18 Pro is more akin to the M5 in terms of firmware than it is to M4. The M5’s user space has some similarities to M4, with the ISA being mostly a superset, but some parts are completely different, such as its texture descriptors. The M5’s user space has been partially reverse engineered; its firmware ABI is fully reverse engineered; a prototype `drm-shim` has been built and thoroughly tested; and I don’t think it would take long to promote the prototype to a full Rust driver. My primary targets remain the M4 Mac Mini and MacBook Neo.

## Footnotes

* * *

1. [Metal Helper Programs](https://gist.github.com/ckissane/3f2817e5113337b8d3a8961df32135dc/raw/54e31ca6c2fb51aec8771aa01bceaa5408608281/Metal_Helper_Programs.md)

   I’m not sure what’s with Claude’s header about this not being clean room, I think it got confused about the whole “don’t look at Apple binaries” instruction in `agx-re`. The doc itself clearly does not contain any tainted information. [↩︎](https://codyho.dev/blog/gpu-driver/#fnref:1)

2. A brief list:

   - If we ever crashed the firmware, the only recovery is a full reboot
   - If anything was wrong with the work handshake, no error would be reported, the work would be ACKed and then never completed.
   - Some problems resulted in the output changing as expected, but also arbitrary corruption on pages that weren’t supposed to be touched.
    [↩︎](https://codyho.dev/blog/gpu-driver/#fnref:2)
3. [https://github.com/mischa85/apple-gpu-firmware-abi](https://github.com/mischa85/apple-gpu-firmware-abi) [↩︎](https://codyho.dev/blog/gpu-driver/#fnref:3)

4. If we found a page containing a shader, we threw this page away and substituted our own shader as an additional copyright measure. [↩︎](https://codyho.dev/blog/gpu-driver/#fnref:4)
