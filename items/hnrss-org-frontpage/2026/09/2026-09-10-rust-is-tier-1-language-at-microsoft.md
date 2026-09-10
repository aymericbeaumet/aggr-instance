---
title: Rust Is Tier-1 Language at Microsoft
link: https://rustfoundation.org/media/guest-post-rust-is-tier-1-language-at-microsoft/
source: hnrss-org-frontpage
published: 2026-09-10T13:39:16Z
updated: 2026-09-10T13:39:16Z
first_seen: 2026-09-10T17:08:19.315390560Z
authors:
- mmastrac
summary: 'Article URL: https://rustfoundation.org/media/guest-post-rust-is-tier-1-language-at-microsoft/ Comments URL: https://news.ycombinator.com/item?id=49643546 Points: 295 # Comments: 139'
content: extracted
html: 2026-09-10-rust-is-tier-1-language-at-microsoft.html
preview:
  file: 2026-09-10-rust-is-tier-1-language-at-microsoft.preview-bd0ef80e334c.webp
  width: 256
  height: 144
  color: '#433f5f'
images:
- source: https://rustfoundation.org/wp-content/uploads/2026/09/Guest-Blog-Graphic-2.png
  original:
    file: 2026-09-10-rust-is-tier-1-language-at-microsoft.image-cb0c94291131.png
    width: 800
    height: 450
  variants:
  - file: 2026-09-10-rust-is-tier-1-language-at-microsoft.image-bdbc58613572.webp
    width: 48
    height: 27
  color: '#27325d'
---

By now it’s no surprise that Rust is of strategic importance to Microsoft. From bold mission statements when Azure CTO Mark Russinovich outlined our future strategy for native code, to millions of dollars invested by Microsoft into supporting the Rust Project, over the following years. Watch [Mark’s keynote](https://www.youtube.com/watch?v=uDtMuS7BExE) from last year’s RustConf to get a glimpse of some of our core projects powered by Rust.

Today, Rust sits among C++, C#, and TypeScript as one of the best-supported languages for internal development at Microsoft. This “Tier-1 language” engineering status for Rust means giving internal teams a paved path from local development to production: secure toolchain builds, productive developer tooling, quality workflows, deep platform integration, and compliance with the SDL requirements Microsoft software must meet.

The Windows platform and MSVC have co-evolved for decades, as C and C++ have been the building blocks of our dev platform. As MSVC and Windows develop innovations, we need to ensure that these features and functionality are available across both C++ and Rust, and that we have *seamless interoperability* between them.

MSVC *is* the native platform compiler for Windows, and Rust needs to participate fully in that ecosystem as its usage grows across Microsoft: from firmware and drivers, kernel and hypervisors, to microservices and apps. This is extremely important in hybrid Rust/C++ projects. One of our most important investments towards this goal is `rustc_codegen_utc`.

### **Introducing rustc\_codegen\_utc**

`rustc_codegen_utc` is an alternative code generation backend for rustc, in the same architectural family as `rustc_codegen_llvm`, `rustc_codegen_gcc` and `rustc_codegen_cranelift`. It plugs into the same backend interface and connects rustc’s shared compiler machinery to the MSVC backend (aka “[UTC](https://devblogs.microsoft.com/cppblog/optimizing-c-code-overview/)“).

It participates in a broader platform ecosystem strategy to support:

- High compatibility with the Windows tooling eco-system and ABI
- Binary hardening and code security features
- Post-link compliance, analysis, and servicing (including Hotpatch)
- Seamless Rust/C++ interop for hybrid projects
- Cross-language inlining, optimization, and [SPGO](https://devblogs.microsoft.com/cppblog/introducing-sample-profile-guided-optimization-in-msvc/) (Sample Profile Guided Optimization)
- Debugging and crash-dump analysis
- Profiling, diagnostics, and coverage

These capabilities and workflows have been built over many years with the MSVC tech-stack. Connecting rustc to that backend lets Rust build on the same platform investment, with *perfect compatibility* out of the box, rather than requiring a parallel implementation of every Windows-specific capability.

The result is a *unified* code generation platform for Rust and C++ on Windows.

### **Native platform evolution for Rust and C++**

Production software at Microsoft moves through extensive security and quality workflows. Rust has become a Tier-1 language at Microsoft, but C++ still dominates after decades of development. Using a *unified* codegen platform will minimize both maintenance and evolution costs for us. MSVC and Windows continue to innovate. New code generation capabilities, security features, diagnostics, and servicing technologies are added all the time.

With rustc\_codegen\_utc, those investments land on a common foundation for both C++ and Rust. That matters operationally because it reduces duplicated engineering, but it also matters strategically: Rust participates directly in Microsoft’s Windows-native engineering ecosystem.

This is especially valuable for hybrid Rust/C++ projects, and many systems will use both languages for years to come. A shared backend creates the *foundation* for both sides of those systems to participate equally in all development workflows.

This alignment is one of the main reasons rustc\_codegen\_utc exists. It allows Rust to meet the engineering expectations of the environment where Microsoft teams already build, validate, diagnose, and service Windows software.

### **A stronger Rust and C++ interop foundation**

On other platforms, if a hybrid Rust/C++ project builds its C++ code with Clang, both languages can already reach the same LLVM code generation platform. rustc\_codegen\_utc creates the same shared-backend opportunity for Windows-native C++ projects where MSVC is the native platform compiler.

Codegen, platform quirks, ABI, EH and post-link tooling are only *half* of the interop challenge. High-fidelity language-level interop, FFI contracts, bindings, language semantics differences… and build systems are just *some* of the problems that we’re working on internally *and across the industry*. See the Rust Foundation’s ***Interoperability Initiative*** for a quick tour of the problem space and the ongoing community efforts: \
[https://rustfoundation.org/interop-initiative/](https://rustfoundation.org/interop-initiative/)

### **Rollout as part of the Microsoft Rust Paved Path**

`rustc_codegen_utc` has been a major investment by a dedicated team in Microsoft DevDiv and has been **production-ready** since early 2026. It has been *self-hosted* since Rust 1.90 and is part of a broader internal Rust platform that includes secure supply-chain builds of rustc, standard library, associated tools, integration with local development, production pipelines and common quality & compliance workflows in our engineering systems.

More than 100 Microsoft project repositories build with it today. This rollout continues each week as more repositories adopt it.

This is part of a sustained investment in the full engineering lifecycle for Rust at Microsoft: acquisition, tooling, quality, security, platform integration, production deployment, and long-term support.

`rustc_codegen_utc` is a major part of that journey. Its differentiator is simple: it connects rustc to the native backend and platform ecosystem that Microsoft has built around Windows for decades.

That gives us a unified foundation for Rust and C++, a path for Windows platform investments to reach both languages, and a stronger base for the hybrid native systems Microsoft will continue to build.

If you want to ask questions or share feedback with us, join [this dedicated discussion topic](https://rust-lang.zulipchat.com/#narrow/channel/131828-t-compiler/topic/rustc-codegen-utc/with/623119950) on the Rust Project’s Zulip.
