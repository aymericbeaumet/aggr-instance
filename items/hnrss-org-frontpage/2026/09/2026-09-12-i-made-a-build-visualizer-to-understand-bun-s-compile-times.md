---
title: I made a build visualizer to understand Bun's compile times
link: https://lalitm.com/post/buildprof/
source: hnrss-org-frontpage
published: 2026-09-12T14:45:28Z
updated: 2026-09-12T14:45:28Z
first_seen: 2026-09-13T06:43:54.722704267Z
authors:
- lalitmaganti
summary: 'Article URL: https://lalitm.com/post/buildprof/ Comments URL: https://news.ycombinator.com/item?id=49672842 Points: 115 # Comments: 22'
content: extracted
html: 2026-09-12-i-made-a-build-visualizer-to-understand-bun-s-compile-times.html
preview:
  file: 2026-09-12-i-made-a-build-visualizer-to-understand-bun-s-compile-times.preview-68c7867de146.webp
  width: 256
  height: 134
  alt: Social card for I made a build visualizer to understand Bun’s compile times
  color: '#f5f6f6'
images:
- source: https://lalitm.com/social-cards/buildprof.png
  original:
    file: 2026-09-12-i-made-a-build-visualizer-to-understand-bun-s-compile-times.image-4e8a7c147f43.png
    width: 1200
    height: 630
  variants:
  - file: 2026-09-12-i-made-a-build-visualizer-to-understand-bun-s-compile-times.image-85218afd8342.webp
    width: 48
    height: 25
  - file: 2026-09-12-i-made-a-build-visualizer-to-understand-bun-s-compile-times.image-2264e17f9cec.webp
    width: 320
    height: 168
  - file: 2026-09-12-i-made-a-build-visualizer-to-understand-bun-s-compile-times.image-359728e0ba26.webp
    width: 640
    height: 336
  - file: 2026-09-12-i-made-a-build-visualizer-to-understand-bun-s-compile-times.image-bd2fff16796f.webp
    width: 960
    height: 504
  - file: 2026-09-12-i-made-a-build-visualizer-to-understand-bun-s-compile-times.image-b67d242b06b4.webp
    width: 1200
    height: 630
  color: '#fefefe'
- source: https://lalitm.com/assets/buildprof/ripgrep-overview.png
  original:
    file: 2026-09-12-i-made-a-build-visualizer-to-understand-bun-s-compile-times.image-073c7569ff79.png
    width: 1600
    height: 750
  variants:
  - file: 2026-09-12-i-made-a-build-visualizer-to-understand-bun-s-compile-times.image-f32b74130eca.webp
    width: 48
    height: 23
  - file: 2026-09-12-i-made-a-build-visualizer-to-understand-bun-s-compile-times.image-067d5f28db94.webp
    width: 320
    height: 150
  - file: 2026-09-12-i-made-a-build-visualizer-to-understand-bun-s-compile-times.image-7d32ba19a8fa.webp
    width: 640
    height: 300
  - file: 2026-09-12-i-made-a-build-visualizer-to-understand-bun-s-compile-times.image-0ae9cf5ae96e.webp
    width: 960
    height: 450
  - file: 2026-09-12-i-made-a-build-visualizer-to-understand-bun-s-compile-times.image-31a48097902e.webp
    width: 1600
    height: 750
  color: '#fdfdfc'
- source: https://lalitm.com/assets/buildprof/jarred-bun-rust-compile-times.png
  original:
    file: 2026-09-12-i-made-a-build-visualizer-to-understand-bun-s-compile-times.image-949f4a4ff3eb.png
    width: 1486
    height: 1820
  color: '#2a2b36'
- source: https://lalitm.com/assets/buildprof/ripgrep-process-chain.png
  original:
    file: 2026-09-12-i-made-a-build-visualizer-to-understand-bun-s-compile-times.image-c65b22898d56.png
    width: 2200
    height: 450
  variants:
  - file: 2026-09-12-i-made-a-build-visualizer-to-understand-bun-s-compile-times.image-00a2f10fc567.webp
    width: 48
    height: 10
  color: '#fcfcfc'
- source: https://lalitm.com/assets/buildprof/zig-ci-overview-notes.png
  original:
    file: 2026-09-12-i-made-a-build-visualizer-to-understand-bun-s-compile-times.image-88011c0c1835.png
    width: 1600
    height: 540
  variants:
  - file: 2026-09-12-i-made-a-build-visualizer-to-understand-bun-s-compile-times.image-36a4858e4efb.webp
    width: 48
    height: 16
  - file: 2026-09-12-i-made-a-build-visualizer-to-understand-bun-s-compile-times.image-05d912114e0c.webp
    width: 320
    height: 108
  - file: 2026-09-12-i-made-a-build-visualizer-to-understand-bun-s-compile-times.image-21eec2a26401.webp
    width: 640
    height: 216
  - file: 2026-09-12-i-made-a-build-visualizer-to-understand-bun-s-compile-times.image-47a3ba0267c6.webp
    width: 960
    height: 324
  - file: 2026-09-12-i-made-a-build-visualizer-to-understand-bun-s-compile-times.image-fff0e1bd8e5f.webp
    width: 1600
    height: 540
  color: '#fdfdfb'
- source: https://lalitm.com/assets/buildprof/zig-full-lto-command-notes.png
  original:
    file: 2026-09-12-i-made-a-build-visualizer-to-understand-bun-s-compile-times.image-be5d3506807b.png
    width: 1600
    height: 565
  variants:
  - file: 2026-09-12-i-made-a-build-visualizer-to-understand-bun-s-compile-times.image-dca068cff7ee.webp
    width: 48
    height: 17
  - file: 2026-09-12-i-made-a-build-visualizer-to-understand-bun-s-compile-times.image-c6be63295dd8.webp
    width: 320
    height: 113
  - file: 2026-09-12-i-made-a-build-visualizer-to-understand-bun-s-compile-times.image-f33041292d43.webp
    width: 640
    height: 226
  - file: 2026-09-12-i-made-a-build-visualizer-to-understand-bun-s-compile-times.image-e4df7149a80f.webp
    width: 960
    height: 339
  - file: 2026-09-12-i-made-a-build-visualizer-to-understand-bun-s-compile-times.image-adf31c216a05.webp
    width: 1280
    height: 452
  - file: 2026-09-12-i-made-a-build-visualizer-to-understand-bun-s-compile-times.image-8e2e74a5f558.webp
    width: 1600
    height: 565
  color: '#fcfcfb'
- source: https://lalitm.com/assets/buildprof/zig-full-lto-linker-internals-notes.png
  original:
    file: 2026-09-12-i-made-a-build-visualizer-to-understand-bun-s-compile-times.image-6dafa8b5da19.png
    width: 1600
    height: 620
  variants:
  - file: 2026-09-12-i-made-a-build-visualizer-to-understand-bun-s-compile-times.image-d42044d8c770.webp
    width: 48
    height: 19
  - file: 2026-09-12-i-made-a-build-visualizer-to-understand-bun-s-compile-times.image-6f95541b7556.webp
    width: 320
    height: 124
  - file: 2026-09-12-i-made-a-build-visualizer-to-understand-bun-s-compile-times.image-58e33b4e14d6.webp
    width: 640
    height: 248
  - file: 2026-09-12-i-made-a-build-visualizer-to-understand-bun-s-compile-times.image-f65e29fb9841.webp
    width: 1600
    height: 620
  color: '#fcfcf9'
- source: https://lalitm.com/assets/buildprof/rust-ci-overview-notes.png
  original:
    file: 2026-09-12-i-made-a-build-visualizer-to-understand-bun-s-compile-times.image-a83c93e0b13a.png
    width: 1600
    height: 640
  variants:
  - file: 2026-09-12-i-made-a-build-visualizer-to-understand-bun-s-compile-times.image-2e0ba355721c.webp
    width: 48
    height: 19
  - file: 2026-09-12-i-made-a-build-visualizer-to-understand-bun-s-compile-times.image-3adf49ba89dc.webp
    width: 320
    height: 128
  - file: 2026-09-12-i-made-a-build-visualizer-to-understand-bun-s-compile-times.image-6292717a7e90.webp
    width: 640
    height: 256
  - file: 2026-09-12-i-made-a-build-visualizer-to-understand-bun-s-compile-times.image-e2e2571b0c87.webp
    width: 1600
    height: 640
  color: '#fdfdfc'
- source: https://lalitm.com/assets/buildprof/rust-ci-thinlto-notes.png
  original:
    file: 2026-09-12-i-made-a-build-visualizer-to-understand-bun-s-compile-times.image-def9b317255a.png
    width: 1600
    height: 780
  variants:
  - file: 2026-09-12-i-made-a-build-visualizer-to-understand-bun-s-compile-times.image-f1a95a9c9e73.webp
    width: 48
    height: 23
  - file: 2026-09-12-i-made-a-build-visualizer-to-understand-bun-s-compile-times.image-c3712af5543a.webp
    width: 320
    height: 156
  - file: 2026-09-12-i-made-a-build-visualizer-to-understand-bun-s-compile-times.image-67c67215e0c9.webp
    width: 640
    height: 312
  - file: 2026-09-12-i-made-a-build-visualizer-to-understand-bun-s-compile-times.image-c27c89f59130.webp
    width: 960
    height: 468
  - file: 2026-09-12-i-made-a-build-visualizer-to-understand-bun-s-compile-times.image-c81534a4d740.webp
    width: 1280
    height: 624
  - file: 2026-09-12-i-made-a-build-visualizer-to-understand-bun-s-compile-times.image-6c95345b8eb0.webp
    width: 1600
    height: 780
  color: '#fcfcfb'
- source: https://lalitm.com/assets/buildprof/zig-lto-pair-comparison-notes.png
  original:
    file: 2026-09-12-i-made-a-build-visualizer-to-understand-bun-s-compile-times.image-8d9d54b7f204.png
    width: 1600
    height: 660
  color: '#fcfcf8'
- source: https://lalitm.com/assets/buildprof/zig-thinlto-webkit-inputs-notes.png
  original:
    file: 2026-09-12-i-made-a-build-visualizer-to-understand-bun-s-compile-times.image-8277adec3614.png
    width: 3200
    height: 660
  variants:
  - file: 2026-09-12-i-made-a-build-visualizer-to-understand-bun-s-compile-times.image-7e87e9c86bc7.webp
    width: 48
    height: 10
  - file: 2026-09-12-i-made-a-build-visualizer-to-understand-bun-s-compile-times.image-fd0f3a41b9f7.webp
    width: 320
    height: 66
  - file: 2026-09-12-i-made-a-build-visualizer-to-understand-bun-s-compile-times.image-02dc9dbe22f3.webp
    width: 640
    height: 132
  - file: 2026-09-12-i-made-a-build-visualizer-to-understand-bun-s-compile-times.image-be2dfc4dc1e7.webp
    width: 960
    height: 198
  - file: 2026-09-12-i-made-a-build-visualizer-to-understand-bun-s-compile-times.image-26f160e3f407.webp
    width: 1280
    height: 264
  - file: 2026-09-12-i-made-a-build-visualizer-to-understand-bun-s-compile-times.image-ac3183000b73.webp
    width: 1600
    height: 330
  - file: 2026-09-12-i-made-a-build-visualizer-to-understand-bun-s-compile-times.image-0b92b16be8a7.webp
    width: 3200
    height: 660
  color: '#fdfcfa'
- source: https://lalitm.com/assets/buildprof/zig-ci-dependency-combined-notes.png
  original:
    file: 2026-09-12-i-made-a-build-visualizer-to-understand-bun-s-compile-times.image-1c031f02e76e.png
    width: 1600
    height: 1176
  color: '#fcfcf9'
---

I built [buildprof](https://buildprof.lalitm.com) ([Github](https://github.com/lalitMaganti/buildprof)), an open-source tracing tool that shows where the time goes when you compile software on Linux. Here’s a realtime video of it profiling a clean build of ripgrep:

[Watch the buildprof demo](https://lalitm.com/assets/buildprof/buildprof-initial-demo-v2.mp4)

Sometimes, builds are slow because there is simply a lot of code to compile. But more often than not, there are fixable problems: poor parallelism, repeated work, dependency downloads or a huge compiler/linker invocation. buildprof makes all of this clearly visible, so you can see what’s worth investigating and optimizing.

You run it by putting `buildprof --` in front of any build command you already use:

```sh
buildprof -- make -j16
buildprof -- cargo build
buildprof -- ninja -C out/target
buildprof -- just build
buildprof -- ./dev/custom-build-script.sh
```

buildprof records every process your build command launches, including their subprocesses (and their subprocesses…), and lays them out on one timeline. Time moves from left to right, bar width shows duration, and child processes appear beneath whatever launched them.

![A ripgrep build: Cargo spans the whole build, rustc invocations compile crates in parallel, and the final rustc invocation launches a linker chain.](https://lalitm.com/assets/buildprof/ripgrep-overview.png)

I made buildprof because [this tweet](https://x.com/jarredsumner/status/2090619419059974620) from Jarred Sumner, chief architect of the Bun JavaScript runtime, was living rent free in my head:

![Jarred Sumner’s comparison showing a 30 minute 6 second median Linux build for Bun 1.3.14 and 5 minute 37 second median for Bun 1.4.0.](https://lalitm.com/assets/buildprof/jarred-bun-rust-compile-times.png)

Specifically, the claim that Bun’s new Rust build was >5× faster on Linux than its old Zig build really bothered me. In my experience, Zig projects had usually compiled *much* faster than Rust projects of similar complexity. That intuition was enough to make me feel there was a mystery to solve.

This was further compounded by another important, yet easily missed, detail in the tweet: the Zig build used Full LTO, while the Rust build used ThinLTO.

Compilers normally optimize separate compilation units largely in isolation.[1](https://lalitm.com/post/buildprof/#fn:1) Link-time optimization (LTO) lets them optimize **across** those boundaries. Full LTO brings those units together into one large optimization job, while ThinLTO preserves more separation so much of the work can run in parallel.

From past experience, this difference can have an **enormous** effect on build time. The tweet mentioned it in passing, but I wondered how much of the headline improvement it explained.

I started by trying to reproduce the numbers.

## The numbers reproduced. But now what?[#](https://lalitm.com/post/buildprof/#the-numbers-reproduced-but-now-what)

I checked out [Bun 1.3.14](https://github.com/oven-sh/bun/releases/tag/bun-v1.3.14) and [Bun 1.4.0](https://github.com/oven-sh/bun/releases/tag/bun-v1.4.0) and wrote [some scripts](https://github.com/LalitMaganti/blog-code/tree/main/buildprof-bun) to replay their Linux x64 CI builds on a 6-core, 12-thread Linux VM. The scripts preserved the build steps and their dependencies, running everything on one machine.[2](https://lalitm.com/post/buildprof/#fn:2)

My timings were in the same ballpark as Jarred’s:

| Linux x64 build                     | Zig era | Rust era |
| ----------------------------------- | ------- | -------- |
| Bun’s reported CI median            | 30m06s  | 5m37s    |
| My single-machine CI-profile replay | 24m24s  | 5m40s    |

OK, so the gap showed up on my machine too. But a lot had changed between the two measurements besides the language; so what was actually responsible? Was it the Zig compiler that was taking all that extra time? Or maybe it was the Full LTO link? Or perhaps there was something else in Bun’s build I hadn’t even thought to look at?

This is where my profiling and developer-tools brain kicked in. Usually, when I’m trying to understand why something is slow, I want a trace: what happened, when it happened and how long it took. It would be really cool to have that for these builds, to put them on a timeline and see where their time actually went.

But a build involves a lot of different tools, each with its own idea of what’s happening. What could I record that would let me see across all of them?

## Builds are process trees [#](https://lalitm.com/post/buildprof/#builds-are-process-trees)

When you type `cargo build` or `zig build`, it feels like you are running one program. The build system works out what needs to be rebuilt, the ordering between those pieces and what can run in parallel. But generally, it does not perform all that work itself; it launches compilers, code generators, archivers, linkers and arbitrary scripts. Which can launch more programs which launch some more…

Different build systems describe that work in different ways. Cargo sees crates, Ninja sees build edges and CMake generates instructions for another build system. From the operating system’s point of view, however, they (mostly) look like processes launching other processes.[3](https://lalitm.com/post/buildprof/#fn:3)

A Rust build, for example, might contain a chain like this:

```text
cargo
└── rustc
    └── cc
        └── collect2
            └── ld.lld
```

If we record when each subprocess starts and ends, we can lay them out on a timeline. Here’s what that chain looks like in buildprof:

![The final link in a ripgrep build, showing cargo launching rustc, then cc, collect2 and ld.lld beneath it.](https://lalitm.com/assets/buildprof/ripgrep-process-chain.png)

There are also several nice properties to visualizing a build at this layer:

1. **It’s build-system agnostic**: Cargo, Ninja, Zig, Make and most other build systems do much of their work by spawning processes, so we do not need to write a special integration for each one.
2. **It naturally includes custom scripts**: This includes both scripts above the build system (repository setup, dependency fetching) and scripts underneath it (code generators, asset processors).
3. **We can follow the files between build steps**: recording which files each process reads and writes lets us see which steps produce the inputs for others. This even works across build systems!

This gave me a starting point for buildprof: record the process tree, then turn it into a timeline I could explore. There are plenty more details to get into, which I will do later. But once I had that working, I could finally go back to my initial question: what was Bun doing for those twenty-four minutes?

## Pointing it at Bun [#](https://lalitm.com/post/buildprof/#pointing-it-at-bun)

### Why was the Zig CI build so much slower?[#](https://lalitm.com/post/buildprof/#why-was-the-zig-ci-build-so-much-slower)

I started by recording the Zig-era CI build with buildprof, using the [same scripts as before](https://github.com/LalitMaganti/blog-code/blob/main/buildprof-bun/scripts/record-original-zig-ci.sh):

![The complete Zig-era CI build](https://lalitm.com/assets/buildprof/zig-ci-overview-notes.png)

*[Explore in buildprof](https://buildprof.lalitm.com/v0.2.2/#!/?url=https%3A%2F%2Fblogexamples.lalitm.com%2Fbuildprof-bun%2F2026-09-04%2Fbun-zig-original-ci-b5a45845003ecae0.buildprof)*

Right away we can see a huge problem: the `ld.lld` linker invocation *dominates* the build time. It ran alone at the very end for over sixteen minutes, about two-thirds of the entire build. What the heck was it doing for all that time?

Clicking on the linker shows its command line, which buildprof captures automatically:

![The selected Zig-era linker and its Full LTO flag](https://lalitm.com/assets/buildprof/zig-full-lto-command-notes.png)

There’s Full LTO, just as Jarred said. Given how long the link was taking, it was now my main suspect.

But the process tree alone couldn’t tell me whether LTO was actually responsible for those sixteen minutes. Thankfully, LLD records its own internal timing events, and buildprof can include them when you use `--compiler-traces`.

I [recorded the final link again](https://github.com/LalitMaganti/blog-code/blob/main/buildprof-bun/scripts/record-zig-full-lto-link-detail.sh), this time with `--compiler-traces` enabled:

![LLD’s internal phases](https://lalitm.com/assets/buildprof/zig-full-lto-linker-internals-notes.png)

*[Explore in buildprof](https://buildprof.lalitm.com/v0.2.2/#!/?url=https%3A%2F%2Fblogexamples.lalitm.com%2Fbuildprof-bun%2F2026-09-04%2Fbun-zig-full-lto-link-detail-d1e7a0e1dd7750d4.buildprof)*

Now we can see that LTO *is* where almost all the time goes. The linker is running compiler passes over the program, not just combining already-compiled files. The `OptModule` bar alone takes just over ten minutes and includes the passes which generate machine code.[4](https://lalitm.com/post/buildprof/#fn:4)

#### How did the Rust CI build differ?[#](https://lalitm.com/post/buildprof/#how-did-the-rust-ci-build-differ)

With so much of the Zig build spent in LTO, I wanted to see how much time the Rust build spent linking. I recorded that build too:

![The complete Rust-era CI build](https://lalitm.com/assets/buildprof/rust-ci-overview-notes.png)

*[Explore in buildprof](https://buildprof.lalitm.com/v0.2.2/#!/?url=https%3A%2F%2Fblogexamples.lalitm.com%2Fbuildprof-bun%2F2026-09-04%2Fbun-rust-original-ci-4c9dbcbe6d041d98.buildprof)*

Just 2m24s. And this time, as expected, the linker command contains `-plugin-opt=thinlto`:

![The Rust linker invocation with ThinLTO enabled](https://lalitm.com/assets/buildprof/rust-ci-thinlto-notes.png)

Both builds were doing LTO, but with different settings and very different link times. What if I kept Bun’s Zig code and changed Full LTO to ThinLTO? How much of the gap would that close?

#### Trying ThinLTO [#](https://lalitm.com/post/buildprof/#trying-thinlto)

I [switched Zig Bun’s build flags to ThinLTO](https://github.com/LalitMaganti/blog-code/blob/main/buildprof-bun/patches/zig-thinlto.patch) and recorded another clean build, along with a fresh Full-LTO build for comparison:

![The matched Full-LTO build and partial ThinLTO experiment](https://lalitm.com/assets/buildprof/zig-lto-pair-comparison-notes.png)

*Explore in buildprof: [Full LTO](https://buildprof.lalitm.com/v0.2.2/#!/?url=https%3A%2F%2Fblogexamples.lalitm.com%2Fbuildprof-bun%2F2026-09-04%2Fbun-zig-ci-dag-full-lto-6111eb9fd9486a35.buildprof) · [partial ThinLTO](https://buildprof.lalitm.com/v0.2.2/#!/?url=https%3A%2F%2Fblogexamples.lalitm.com%2Fbuildprof-bun%2F2026-09-04%2Fbun-zig-ci-dag-thin-lto-fdfb05e3d506d810.buildprof)*

The link got 3m40s faster in this pair of recordings, but it was still taking nearly thirteen minutes. Why was linking still so expensive?

Looking back at the compiler trace, a lot of the work was on functions with `JSC` in their names. That’s JavaScriptCore, the engine Bun uses to execute JavaScript. The linker was spending time compiling the JavaScript engine too.[5](https://lalitm.com/post/buildprof/#fn:5)

Clicking on the linker invocation showed the [WebKit](https://github.com/oven-sh/WebKit/tree/5488984d20e0dbfe4be2c3ba8fb18eb81a5e0e8b) libraries among its inputs, including `libJavaScriptCore.a`:

![The linker command has ThinLTO enabled but still includes WebKit’s libraries, including libJavaScriptCore.a.](https://lalitm.com/assets/buildprof/zig-thinlto-webkit-inputs-notes.png)

Following those inputs back through the build, I found that Bun wasn’t compiling these libraries itself. It was downloading them from a separate WebKit build. And when I checked [that build’s flags](https://github.com/oven-sh/WebKit/blob/5488984d20e0dbfe4be2c3ba8fb18eb81a5e0e8b/Dockerfile#L4), there it was again: `-flto=full`. The Rust build used a newer WebKit revision whose [build recipe selected ThinLTO](https://github.com/oven-sh/WebKit/blob/0f966e81b78c84bb/Dockerfile#L4).

Even though I had changed how Bun compiled its own code, those downloaded libraries still contained Full-LTO inputs and so the linker still had to optimize that code and turn it into machine code. To change that, I would have to rebuild WebKit too.

#### Rebuilding WebKit [#](https://lalitm.com/post/buildprof/#rebuilding-webkit)

I checked out the historical WebKit revision and rebuilt it and its ICU dependencies with compatible ThinLTO settings. Then I replaced the downloaded libraries with the ones I had built, keeping the ThinLTO changes to Bun.

Here are the recorded builds:[6](https://lalitm.com/post/buildprof/#fn:6)

| Zig-era build                               | Whole build | Final linker |
| ------------------------------------------- | ----------- | ------------ |
| Original Full LTO                           | 24m24s      | 16m35s       |
| Bun ThinLTO; original WebKit archives       | 20m20s      | 12m55s       |
| Bun ThinLTO; rebuilt ThinLTO WebKit and ICU | 15m11s      | 7m22s        |

The link now took 7m22s. Still slower than the Rust build, but enough of an improvement that I wanted to look beyond the linker.

#### What about the rest of the build?[#](https://lalitm.com/post/buildprof/#what-about-the-rest-of-the-build)

The build still took fifteen minutes, and nearly eight of those passed before the linker even started. What was it waiting for? I went back to the original CI trace to follow the inputs from Bun’s own code.

buildprof also records which files each process reads and writes. If a process reads a file another wrote, it links the two together under the hood. Turning on “Show on timeline” draws those links as arrows. Here, the linker reads `libbun-profile.a` from the C++ compilation and `bun-zig.o` from Zig. Both arrive through copy steps; following those back takes us to the processes which produced them:

![Following the linker’s dependency arrows through the copy steps to the C++ and Zig producers. The producer panels use the same time scale; C++ finishes first.](https://lalitm.com/assets/buildprof/zig-ci-dependency-combined-notes.png)

The C++ side of the compilation finished first. The linker was waiting for `bun-zig.o`, so it could not begin until the Zig branch had finished too.

It was at this point I went back to the Rust build and compared against how it worked, and the main reason the Rust build was faster became obvious: Bun has been split into >90 crates, while in Zig it was all trying to compile as a single Zig module!

![Cargo fanning out into named rustc processes across Bun’s crates, next to the single zig build-obj process which spawns nothing at all.](https://lalitm.com/assets/buildprof/rust-crates-vs-zig-object-notes.png)

This meant that the Zig build cannot parallelise the same way Rust can. I also suspect, though I did not prove this, that it explains the slow linking: the linker has to optimize one huge ThinLTO bitcode module instead of the same work spread across crates.

It was at this point I had to stop: to go any further, I would have to split up the Zig module myself, and given that this code is all obsolete anyway, I didn’t think it was worth doing that.

Summarizing:

- The huge outlier in the initial Zig build vs the Rust build was the massive linker step which ran alone at the end of the build.
- Changing the LTO settings for just Bun was not sufficient as WebKit, a significant part of the build, still used Full LTO.
- Once I had done this, the Zig build dropped from twenty-four minutes to fifteen.
- Even after this, linking still took 7 minutes and the whole build 15 minutes.
- The overwhelming difference which remained was structural: Rust spreads compilation across >90 crates while the Zig build funnelled everything through a single module.

And fwiw, the traces had also turned up a few things I couldn’t resist poking at…

### Other things hiding in the build [#](https://lalitm.com/post/buildprof/#other-things-hiding-in-the-build)

#### A build can contain almost anything [#](https://lalitm.com/post/buildprof/#a-build-can-contain-almost-anything)

In the middle of Bun’s CI build, I found commands asking the public internet for the machine’s IP address, inspecting running Docker containers and reading the latest Git commit message.

![Small CI setup commands visible in the process tree](https://lalitm.com/assets/buildprof/zig-ci-diagnostic-probes-notes.png)

These take well under a second altogether. Nothing to optimize but I just wasn’t expecting to find them in a build trace.

#### A cold dependency fetch [#](https://lalitm.com/post/buildprof/#a-cold-dependency-fetch)

The builds above reused downloaded dependencies, so I also [recorded a fresh WebKit fetch](https://github.com/LalitMaganti/blog-code/blob/main/buildprof-bun/scripts/capture-bun-cold-prebuilt.sh). Downloading and extracting the archive took about twenty seconds. For the first twelve, all we see is Node running. Then it launches `tar` and `gzip`, and we can see the extraction separately.

![A cold WebKit download and extraction](https://lalitm.com/assets/buildprof/zig-cold-webkit-download-article.png)

#### Looking inside one C++ compilation [#](https://lalitm.com/post/buildprof/#looking-inside-one-c-compilation)

Earlier, we followed the linker’s inputs back to Bun’s C++ compilation. We can look inside those compiler invocations too. I picked one of the last files to finish, `ZigGeneratedClasses.cpp`, and [replayed its Ninja command](https://github.com/LalitMaganti/blog-code/blob/main/buildprof-bun/scripts/capture-bun-compiler-detail.sh) with `--compiler-traces`. For Clang, buildprof enables `-ftime-trace` and adds its internal timings to the process timeline.[7](https://lalitm.com/post/buildprof/#fn:7)

![Clang’s frontend and backend phases while compiling ZigGeneratedClasses.cpp](https://lalitm.com/assets/buildprof/zig-generated-classes-compiler-phases-article.png)

The replay took about twelve seconds, split almost evenly between Clang’s frontend and backend. Zooming in further, we see `ModuleInlinerWrapperPass`, one of the phases of Clang, accounts for over four seconds of the backend’s work.

## How buildprof works under the hood [#](https://lalitm.com/post/buildprof/#how-buildprof-works-under-the-hood)

The recording side of buildprof uses `ptrace`, the same Linux interface used by debuggers. I did consider both eBPF and ftrace, but `ptrace` is just straight up perfect for exactly this type of problem; eBPF tracing means `CAP_BPF` and `CAP_PERFMON` permissions and hooking into potentially unstable tracepoints/kernel functions. While with ftrace, I’d have to juggle tracing instances to avoid interfering with other users, and getting the filters perfect for just the build process and all its descendants is cumbersome.[8](https://lalitm.com/post/buildprof/#fn:8)

With `ptrace`, I can launch the build and follow its children directly. Its built-in events tell buildprof when processes fork, exec a new program or exit. And for filesystem activity, buildprof uses a seccomp filter to intercept only the calls it needs.

How much buildprof costs is almost entirely down to how many files the build opens. For ripgrep, recording barely changed the build time. Redis opened files much more often, and recording added about five seconds:[9](https://lalitm.com/post/buildprof/#fn:9)

| Build           | Untraced | Processes only | Processes + files |
| --------------- | -------- | -------------- | ----------------- |
| ripgrep / Cargo | 12.27s   | 12.30s         | 12.43s            |
| Redis / Make    | 26.78s   | 27.04s         | 31.89s            |

If that overhead gets in the way, you can turn off filesystem tracing with `--no-file-events` and keep the process timeline.

I work on [Perfetto](https://github.com/google/perfetto), so it was a natural starting point for the UI; buildprof’s UI is a soft fork of the Perfetto UI. I could have just opened the recordings on [ui.perfetto.dev](https://ui.perfetto.dev), but I wanted control over how the process tree was laid out, which details appeared when you clicked a command, and things like those on-demand arrows between file producers and consumers.

Fortunately, we’ve spent the last several years working on making the Perfetto UI extensible through [plugins](https://perfetto.dev/docs/contributing/ui-plugins). Most of buildprof’s UI is reusing that infrastructure. Perfetto handles the hard stuff (parsing traces, querying events, rendering the timeline and managing workspaces) and I get to focus on what makes those things useful for builds.

I plan on going into a lot more detail about the recorder and UI in a separate technical post. [Subscribe](https://lalitm.com/page/subscribe/) if you’d like to be notified when it comes out! :)

## Did I need to build something new?[#](https://lalitm.com/post/buildprof/#did-i-need-to-build-something-new)

These days it’s very easy to make a tool just because you can. But that wasn’t the case here; before building buildprof, I looked long and hard for an existing tool that could give me this view.

I started with [ninjatracing](https://github.com/nico/ninjatracing), which I’ve used many times. It turns Ninja’s build log into a timeline showing what ran and how much ran in parallel.

[Here’s the Ninja log from the Zig-era build](https://ui.perfetto.dev/#!/?url=https%3A%2F%2Fraw.githubusercontent.com%2FLalitMaganti%2Fblog-code%2Fmain%2Fbuildprof-bun%2Fresults%2Fbun-zig-ci-ninjatracing.json).

But Ninja only sees part of Bun’s build. The scripts which invoke it are missing from its log, and commands it runs appear as single blocks even when they launch whole trees of subprocesses.

There were several other tools, each covering different parts of the problem:

- [Cargo timings](https://doc.rust-lang.org/cargo/reference/timings.html) works well for Cargo-managed builds, but cannot break down arbitrary work inside `build.rs` or see wrapper scripts above Cargo. In Bun, Cargo is only part of the build: [the report I captured](https://lalitm.com/assets/buildprof/bun-rust-ci-cargo-timings.html) covered 1m51s of a 5m40s CI build.
- [Clang’s `-ftime-trace`](https://clang.llvm.org/docs/ClangCommandLineReference.html#cmdoption-clang-ftime-trace) gave us the detail inside a compiler invocation, but cannot show what the rest of the build is doing while [Zig’s Tracy integration](https://ziglang.org/learn/overview/#performance-and-safety-choose-two) goes deeper still and is intended more for understanding the compiler itself.
- [`strace`](https://strace.io/) and [`tracexec`](https://github.com/kxxt/tracexec) can follow arbitrary processes through `fork` and `exec`, but show general process events rather than a build-oriented timeline.

[What the Fork](https://danielchasehooper.com/posts/syscall-build-snooping/) ([via](https://news.ycombinator.com/item?id=44902127)) came closest: it follows processes across build systems and presents a build-specific view. But as far as I could tell, it still appears to be in private beta and there don’t seem to be any plans to make it open source.

## What’s next for buildprof [#](https://lalitm.com/post/buildprof/#whats-next-for-buildprof)

buildprof already does what I wanted it to do, and I plan to keep working on it as I use it on my own builds. But there are a few things I’d like to improve.

Recording overhead is one; the Redis measurements showed there’s room to improve filesystem tracing, especially for builds which open lots of files. I’d also like to support [macOS](https://github.com/LalitMaganti/buildprof/issues/2) where I do some of my work and maybe [Windows](https://github.com/LalitMaganti/buildprof/issues/3) if there’s interest.

There are also more build systems and toolchains I’d like to test, including npm, Gradle and Bazel. Computing critical paths would also be a big improvement: we followed dependencies by hand in this post, but buildprof could help identify the chain of work holding up the build and automatically annotate it.

I’ll probably tackle these as and when I need them. But if you try buildprof and there’s something you wish it could do, I’d be interested to [hear about it](https://github.com/LalitMaganti/buildprof/issues). What people find useful will help me decide where to spend more time.

## Conclusion [#](https://lalitm.com/post/buildprof/#conclusion)

I managed to satiate my curiosity, though I ended up spending rather more time on this than I expected. Along the way I built a tool I now want to have around whenever a build is taking too long.

I know I’ll come back to buildprof the next time a slow build annoys me. If you have one of those builds too, [give it a try](https://github.com/LalitMaganti/buildprof#quick-start). I’d love to hear what you find!
