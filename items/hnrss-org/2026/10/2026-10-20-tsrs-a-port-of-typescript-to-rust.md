---
title: 'Tsrs: A Port of TypeScript to Rust'
link: https://blog.cloudflare.com/typescript-rust-port/
source: hnrss-org
published: 2026-10-20T13:18:07Z
updated: 2026-10-20T13:18:07Z
first_seen: 2026-10-20T17:55:58.869157456Z
authors:
- hnisjafx40
content: extracted
html: 2026-10-20-tsrs-a-port-of-typescript-to-rust.html
remote_preview:
  url: https://blog.cloudflare.com/_emdash/api/media/file/01M5C82XAYSWX571T82A8AZRKS.png
---

Cloudflare and VoidZero are now working with [Max Schwenk](https://building.owner.com/tsrs) of Owner.com on [tsrs](https://github.com/oxc-project/tsrs), a Rust port of the TypeScript compiler. We are doing this work in close collaboration with the TypeScript team, and incubating tsrs as part of the [Oxc JavaScript compiler toolchain](https://github.com/oxc-project).

Our goal is to help developers spend less time waiting for their code to be checked and more time building. Making TypeScript faster and more memory-efficient also helps bring complete development environments to more places, including right in your browser.

To be clear, we are ***not*** forking TypeScript. We believe the TypeScript team should remain in full control of the language’s design and direction. They have maintained a long-term vision while guiding TypeScript’s adoption across the JavaScript community. We do not want to change that.

The TypeScript team can move quite fast on many things, but for some features they may spend years intentionally exploring options and making prototypes before picking the right implementation. A Rust port has to follow those decisions closely, or it will eventually fall apart and become incompatible. We have to preserve the TypeScript language strictly based on its original implementation – and the TypeScript team should remain solely responsible for deciding what that looks like.

## Performance and memory improvements

Early benchmarks of our Rust port of TypeScript are promising: `tsrs` is up to 8x faster and uses ~70% less memory compared to TypeScript 7.0 when using the same number of type check workers:

![](https://blog.cloudflare.com/_image?href=https%3A%2F%2Fblog.cloudflare.com%2F_emdash%2Fapi%2Fmedia%2Ffile%2F01M5C82YQFJQ7JY8JZPCKSP6EG.png&w=1430&h=780&f=webp&fit=cover&position=center)

As you can see, the next version of TypeScript already brings many performance improvements, making TypeScript 7.1 beta up to 2.5x faster than TypeScript 7.0. Some of these optimizations were upstreamed from `tsrs`, and we’re hoping to find more.

You may have seen recent benchmarks of different TypeScript ports with various performance claims, often due to using larger machine types and different options compared to TypeScript’s defaults. We worked with the TypeScript team to ensure [the above benchmarks](https://github.com/oxc-project/tsrs/tree/main/bench) are comparing the same configuration.

By default, tsrs ships with an adaptive configuration dependent on the machine type that uses more memory but is even faster:

![](https://blog.cloudflare.com/_image?href=https%3A%2F%2Fblog.cloudflare.com%2F_emdash%2Fapi%2Fmedia%2Ffile%2F01M5C8302F3MQTH2SKC6MMGDTV.png&w=1430&h=780&f=webp&fit=cover&position=center)

## Try `tsrs`

TypeScript in Rust aims to be a faithful port of TypeScript. You can try an experimental release by installing the [`tsrs`](https://npmx.dev/package/tsrs) package from npm:

```
npm install tsrs
npx tsrs
```

## Why now?

We’ve seen many attempts at porting TypeScript to other languages over the years. Because of the complexity involved, only one of the ports succeeded: the TypeScript team’s port of TypeScript from JavaScript to Go.

However, frontier AI models can now do large-scale ports almost independently in a matter of days. We have now seen slopforks, faithful ports, and completely re-architected versions of TypeScript in Rust or C++. There is clearly a demand for even faster TypeScript.

## Why Rust?

You might be thinking: “TypeScript’s Go port already made TypeScript 10x faster and far more memory efficient than the previous version written in TypeScript. Why isn’t that enough?”

However, it’s not just about speed. Rust offers some unique advantages: [running in WebAssembly](https://www.npmjs.com/package/@tsrs/wasm). The Rust + Wasm story has always been strong, and one of the reasons comes down to the fundamentals of Rust’s language design. When you compile a Go program to Wasm, you have to compile Go’s garbage collector along with it. But Rust’s memory management model enables straightforward memory interactions between a WebAssembly guest and host. This is one of the key things that makes Rust a much better fit for the WebAssembly use-case: memory efficiency.

Cloudflare Workers have supported Wasm for a long time. Just a few weeks ago we announced [supporting native Rust in Workers](https://blog.cloudflare.com/rust-workers-emscripten-target/) with the new Emscripten target for wasm-bindgen. When you run entire software build and deployment systems in tight loops on Workers, the benefits of a small memory footprint pay off quickly. It means you can run these workloads so efficiently that it’s now possible to run fully encapsulated development systems in places where they couldn’t have been run before.

This enables new possibilities when combined with agents: You are now able to run a development toolchain end-to-end in a fully sandboxed environment on Cloudflare. But with Wasm, you can also run those environments safely client-side in your browser, or on an embedded device, or an IoT application.

## How will the `tsrs` port be maintained?

First, we are working with Theo Browne to merge [tsc-rs](https://github.com/pingdotgg/ts-rust) and `tsrs` into a single package that supports all of TypeScript’s features including Language Server Protocol (LSP) and content mappers. Second, we’ve seen a lot of success with how we maintain [Vinext](https://github.com/cloudflare/vinext) in sync with its upstream, Next.js. We explained how we do this in [a recent blog post](https://blog.cloudflare.com/vinext-nextjs-on-vite):

> Each morning, an agent reviews the changes, fetches diffs, and opens tracking issues for anything that could affect Vinext. Every night, the compatibility matrix is regenerated as we run the Next.js test suite against Vinext. When one of these tests or issues reveals a gap, agents are now in the position where they can identify the change across both codebases, build a reproduction, port any relevant tests, and propose a fix.

We will apply the same process to TypeScript, taking extreme care that we don’t introduce diverging behaviors from upstream TypeScript. When we come across performance improvements, we’ll attempt to contribute them back to the TypeScript Go codebase so everyone wins.

Finally, we recognize that there are some differences to the upstream version of TypeScript right now, like for example the order in which diagnostics are printed. We aim to reduce those differences to a minimum by the time we publish a stable release.

## What’s next?

Cloudflare is already the home of many free and open source Rust tools that power the JavaScript ecosystem, such as Vite, Rolldown, Oxlint, and Oxfmt. With more than 10 million developers building on Cloudflare, we care a lot about making the underlying developer toolchain faster, more portable, and easier to run in more places. We are excited to experiment with deeper `tsrs` integrations into our stack, for example for type-aware lint rules in Oxlint.

Like with Vinext, our goal is to run `tsrs` as a well-maintained healthy open source project — without getting in the way of TypeScript itself. The Oxc team has already begun refactoring the codebase to make it faster, use less memory, and make it maintainable. We know the TypeScript team is taking this seriously – they are experimenting with memory and performance improvements for `tsc`. We are excited to collaborate with the TypeScript team and the JavaScript community to build the best and fastest implementations of TypeScript together.
