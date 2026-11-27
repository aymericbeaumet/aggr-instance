---
title: Synadia and TigerBeetle Pledge $512,000 to the Zig Software Foundation
link: https://tigerbeetle.com/blog/2025-10-25-synadia-and-tigerbeetle-pledge-512k-to-the-zig-software-foundation
source: tigerbeetle-com
published: 2025-10-25T00:00:00Z
updated: 2025-10-25T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Joran Dirk Greef
summary: Synadia and TigerBeetle have together pledged $512,000 to the Zig Software Foundation over the next two years in support of the language, leadership, and communities building the future of simpler systems software.
content: extracted
html: 2025-10-25-synadia-and-tigerbeetle-pledge-512-000-to-the-zig-software.html
remote_preview:
  url: https://tigerbeetle.com/blog/2025-10-25-synadia-and-tigerbeetle-pledge-512k-to-the-zig-software-foundation/banner-680a60e1.webp
---

[Synadia](https://www.synadia.com/blog/synadia-tigerbeetle-zig-foundation-pledge) and TigerBeetle have together pledged $512,000 to the [Zig Software Foundation](https://ziglang.org) over the next two years in support of the language, leadership, and communities building the future of simpler systems software.

I first saw Zig in 2018, seven years ago. Two years later, I chose Zig over C or Rust for TigerBeetle.

In 2020, I was following Rust closely. At the time, Rust’s default memory philosophy was to crash when out of memory (OOM). However, for TigerBeetle, I wanted explicit static allocation, following [NASA’s Power of Ten Rules for Safety-Critical Code](https://spinroot.com/gerard/pdf/P10.pdf), which would become a part of [TigerStyle](https://github.com/tigerbeetle/tigerbeetle/blob/main/docs/TIGER_STYLE.md), a methodology for creating [safer software in less time](https://www.youtube.com/watch?v=w3WYdYyjek4).

What I learned is that if you could centralize resource allocation in time and space (the dimensions that prove tricky for humans writing software) then this could not only simplify memory management, to design away some of the need for a borrow checker in the first place, but, more importantly, also be a forcing function for propagating good design, to encourage teams to think through the explicit limits or physics of the software (you have no choice).

From a performance perspective, I didn’t want TigerBeetle to be fearlessly multithreaded. [Transaction processing workloads tend to have inherent contention](https://www.youtube.com/watch?v=yKgfk8lTQuE), even to the point of power law, precluding partitioning and necessitating a single-threaded architecture. Therefore, Rust’s borrow checker, while a phenomenal tool for the class of problems it targets, made less sense for TigerBeetle. TigerBeetle never frees memory and never runs multithreaded, instead using explicit submission/completion queue interfaces by design.

Finally, while the borrow checker could achieve local memory safety, TigerBeetle needed more correctness properties. TigerBeetle needed to be always correct, and across literally thousands of invariants. [As matklad would say, this is a harder problem!](https://matklad.github.io/2023/03/26/zig-and-rust.html) I had also spent enough time in memory safe languages to know that local memory safety is no guarantee of local correctness, let alone distributed system correctness. Per systems thinking, I believe that total correctness is a design problem, not a language problem. Language is valuable. But no human language can guarantee the next Hemingway or Nabokov. For this you need philosophy. Even then it’s not a guarantee but a probability.

With Rust off the table, the choice fell to C or Zig. A language of the past or future?

Zig was early, which gave me pause, but I felt that the quality of Andrew Kelley’s design decisions in the language, the standard library (e.g. the unmanaged hashmap interface) and the cross-compilation toolchain, even five years ago, was already exceptional.

Andrews’s philosophy resonated with what I wanted to explore in TigerStyle. No hidden memory allocations. No hidden control flow. No preprocessor. No macros. And then you get things like comptime, reducing the grammar and dimensionality of the language, while simultaneously multiplying its power. The primary benefit of Zig is the favorable ratio of expressivity to language complexity.

As a replacement for C, Zig fixed not only the small cuts, such as explicit alignment in the type system for Direct I/O, or safer casts, but the showstoppers of spatial memory safety through bounds checking, and, to a lesser degree (but not guarantee), temporal memory safety through the debug allocator.

Zig also enabled checked arithmetic by default in safe builds, which is something I believe only Ada and Swift do (remarkably, Rust disables checked arithmetic by default in safe builds—a default I would love to see changed). TigerBeetle separates the data plane from the control plane by design, through batching, so the runtime cost of these safety checks was not material, being amortized in the data plane across bigger buffers. While a borrow checker or static allocation can simplify memory management, getting logic and arithmetic correct remains hard. Of course, you can enable checked arithmetic in other languages, but I appreciated Andrew’s concern for checked arithmetic and stricter operands by default.

In all these things, what impressed me most was Zig’s approach to safety when working with the metal. Not in terms of an on/off decision, but as a spectrum. Not aiming for 100% guarantees across 1 or 2 categories, but 90% and then across more categories. Not eliminating classes of bugs, but downgrading their probability. All while preserving the power-to-weight ratio of the language, to keep the language beautifully simple.

Many languages start simple and grow complex as features are added. Zig’s simplicity is unusual in that it comes from a subtractive discipline (e.g. no private fields) rather than a deferred complexity; minimizing surface area is part of the ethos of the language. The simplicity of Zig meant that we could hire great programmers from any language background—they could pick up Zig in a weekend. Indeed, I’ve never had to talk to a new hire about learning Zig.

Finally, there was the timing. Recognizing that TigerBeetle would take time to reach production (we shipped production in 2024, after 3.5 years of development), giving Zig time to mature, for our trajectories to intersect.

Investing in creating a database like TigerBeetle is a long term effort. Databases tend to have a long half life (e.g. Postgres is 30 years old). And so, while Zig being early in 2020 did give me pause, nevertheless Zig’s quality, philosophy and simplicity made sense for a multi-decade horizon.

How has the decision for Zig panned out?

TigerBeetle is tested end-to-end under some pretty extreme fuzzing. We did have three bugs that would have been prevented by the borrow checker, but these were caught by our fuzzers and online verification. We run a fuzzing fleet of 1,000 dedicated CPU cores 24/7. We invest in deterministic simulation testing (e.g. [VOPR](https://github.com/tigerbeetle/tigerbeetle/blob/main/docs/internals/vopr.md)), as well as non-deterministic fault-injection harnesses (e.g. [Vörtex](https://tigerbeetle.com/blog/2025-02-13-a-descent-into-the-vortex/)). We engaged Kyle Kingsbury in [one of the longest Jepsen audits to date](https://jepsen.io/analyses/tigerbeetle-0.16.11)—four times the typical duration. Through all this, Zig’s quality held up flawlessly.

Zig has also been a huge part of our success as a company. TigerBeetle is only 5 years old but is already migrating some of the largest brokerages, exchanges and wealth managements in their respective jurisdictions. Several of our key enterprise contracts were thanks to the CTOs and even CEOs of these companies also following Zig and seeing the quality we wanted to achieve with it. I don’t think we could have written TigerBeetle [as it is, in any other language](https://lobste.rs/s/uhtjdz/rust_vs_zig_reality_somewhat_friendly#c_8rtrto), at least not to the same tight tolerances, let alone with the same velocity.

Zig’s language specification will only reach 1.0 when all experimental areas of the language (e.g. async I/O) are finally done. For TigerBeetle, we care only about the stable language features we use, testing our binaries end to end, as we would for any language. Nevertheless, upgrading to new versions, even with breaking changes, has only been a pleasure for us as a team. The upgrade work is usually fully paid for by compilation time reduction. For example, the upgrade from Zig 0.14.1 to Zig 0.15.2 (with the native x86\_64 backend) makes debug builds 2x faster, and even LLVM release builds become 1.6x faster. With each release, you can literally feel the sheer amount of effort that the entire Zig core team put into making Zig the world’s most powerful programming language—and toolchain.

Back in 2020, from a production perspective, Zig was more or less a frontend to LLVM, the same compiler used by Rust, Swift and other languages. However, by not shying away from also investing in its own independent compiler backends and toolchain, by appreciating the value of replacing LLVM long term, Zig is becoming well positioned to gain a low-level precision and compilation speed that generic LLVM won’t always be able to match.

We want Andrew to take his time, to get these things right for the long term. Fred Brooks once said that [conceptual integrity](https://en.wikipedia.org/wiki/The_Mythical_Man-Month) is “the most important consideration” in system design, that the design must proceed from one mind.

In this spirit, I am grateful for Andrew’s remarkably strong leadership (and taste) in the design of the language and toolchain. There can be thankless pressure on an open source project to give in to the tragedy of the commons. But if anything, in hindsight I think this is what I’ve most appreciated about choosing Zig for TigerBeetle, that Zig has a strong BDFL.

Of course, some may hear “BDFL” and see governance risk. But I fear the opposite: conceptual risk, the harder problem. Brooks was right—conceptual integrity is almost certainly doomed by committee. Whereas governance is easier solved: put it in the hands, not of the corporates, but of the people. The individuals who choose each day to continue to donate.

This is why our pledge today, along with all other ZSF donors, is a simple donation with no strings attached. The Zig Software Foundation is well managed, transparent and independent. We want it to remain this way. The last thing we want is some kind of foundation “seat”. Andrew is Chef. We want to let him cook, and pay his core team sustainably (e.g. [92% percent of budget goes to directly paying contributors](https://ziglang.org/news/2025-financials/)).

If cooking is one metaphor, then surfing is another. I believe that technology moves in waves. The art is not in paddling to the wave with a thousand surfers on it. But in spotting the swell before it breaks. And then enjoying the ride with the early adopters who did the same. [River](https://isaacfreund.com/software/river/), [Ghostty](https://ghostty.org), [Bun](https://bun.com), [Mach](https://github.com/hexops/mach) and many fellow surfers.

In fact, it was through Zig that I met [Derek Collison](https://x.com/derekcollison), who like me had been sponsoring the language in his personal capacity since 2018. As a former CTO at VMware, Derek was responsible for backing [antirez](https://antirez.com) to work full time on Redis. Derek later went on to create [NATS](https://nats.io), founding [Synadia](https://www.synadia.com).

As we were about to increase TigerBeetle’s yearly donation to Zig, I reached out to Derek, and we decided to do a joint announcement, [following Mitchell Hashimoto’s lead](https://mitchellh.com/writing/zig-donation). For each of our companies to donate $256,000 in monthly installments over the next two years, with Synadia matching TigerBeetle, for a total of $512,000—the first installment already made.

Please consider [donating](https://ziglang.org/zsf/) or increasing your donation if you can. And if you are a CEO or CTO, please team up with another company to outmatch us! Thanks Andrew for creating something special, and to all who code for the joy of the craft:

Together we serve the users.
