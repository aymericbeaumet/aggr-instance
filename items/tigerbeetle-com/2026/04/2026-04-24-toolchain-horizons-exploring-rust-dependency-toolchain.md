---
title: 'Toolchain Horizons: Exploring Rust Dependency-Toolchain Compatibility'
link: https://tigerbeetle.com/blog/2026-04-24-toolchain-horizons
source: tigerbeetle-com
published: 2026-04-24T00:00:00Z
updated: 2026-04-24T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Brian Anderson
summary: I tested the top 100 Rust crates on crates.io for backwards compatibility. Then I removed every dependency from the TigerBeetle Rust client and backported it to Rust 1.39, from 2019.
content: extracted
html: 2026-04-24-toolchain-horizons-exploring-rust-dependency-toolchain.html
remote_preview:
  url: https://tigerbeetle.com/blog/2026-04-24-toolchain-horizons/banner-34465fa7.webp
---

I tested the top 100 Rust crates on crates.io for backwards compatibility. Then I removed every dependency from the TigerBeetle Rust client and backported it to Rust 1.39, from 2019.

Here’s why: last year I created the Rust client for [TigerBeetle](https://github.com/tigerbeetle/tigerbeetle), the financial transactions database. One morning, after weeks of work reducing the client’s minimum supported Rust version, I found CI broken — a point release of a dependency of a dependency had bumped its [`rust-version`](https://doc.rust-lang.org/cargo/reference/manifest.html#the-rust-version-field) from 1.61 to 1.68.

This annoyed me a lot, so I ran an experiment.

TigerBeetle has a client-server architecture, and provides client libraries for most popular languages: Python, Java, Go, Node, .NET, and Rust. Each of these is a bindings/FFI project that binds to the single `tb_client` library, written in Zig, exposing a C ABI, wrapped in the idioms of the embedding language. We share one core across every language client so that as much of the client-side code path as possible is covered by our deterministic simulation testing (the [VOPR](https://github.com/tigerbeetle/tigerbeetle/blob/main/docs/internals/vopr.md)).

![TigerBeetle client architecture: language clients (Python, Java, Go, Node, .NET, Rust) all connect to the central tb_client library written in Zig](https://tigerbeetle.com/blog/2026-04-24-toolchain-horizons/tb-client-arch-733e2e43.svg)\
TigerBeetle client architecture: language clients (Python, Java, Go, Node, .NET, Rust) all connect to the central tb\_client library written in Zig

The Rust client is less than two thousand lines of production Rust code, some of it generated, some of it written by hand. It provides a simple asynchronous API for use with `async` / `await`; and it is runtime-agnostic, requiring no dependencies on specific Rust async runtimes.

In use it looks like this:

```
use tigerbeetle as tb;

// Connect to TigerBeetle.
let client = tb::Client::new(0, "127.0.0.1:3000")?;

// Create two accounts on the same ledger.
let accounts = [
    tb::Account {
        id: tb::id(),
        ledger: 1,
        code: 1,
        ..Default::default()
    },
    tb::Account {
        id: tb::id(),
        ledger: 1,
        code: 1,
        ..Default::default()
    },
];
client.create_accounts(&accounts).await?;

// Transfer 100 units from the first account to the second.
let transfers = [tb::Transfer {
    id: tb::id(),
    debit_account_id: accounts[0].id,
    credit_account_id: accounts[1].id,
    amount: 100,
    ledger: 1,
    code: 1,
    ..Default::default()
}];
client.create_transfers(&transfers).await?;
```

It’s a small codebase with basic requirements but it does need some common dependencies.

The Rust ecosystem has a concept of the “Minimum Supported Rust Version” (MSRV) for its crates. It relates crates to the Rust compiler version, and it is separate from SemVer, Rust’s primary versioning scheme.

Some crates encode this information in their `Cargo.toml` manifest’s optional [`rust-version`](https://doc.rust-lang.org/cargo/reference/rust-version.html) field. It is a best practice for crate maintainers to know and document their minimum supported Rust version and test against that version in their CI.

When I do the initial development of a new Rust crate, I don’t worry about the minimum Rust version; I save that work for just before publication, a process like:

1. Start with the oldest version I know I support.

2. Test against the previous version.

3. Fix the build.

   This usually involves removing or replacing dependencies, and replacing newer language features with older ones or dependencies that fill that role. Open-coded polyfills are often involved.

4. Update CI to verify that as the minimum supported Rust version.

5. Do it again.

When I posted the [initial pull request](https://github.com/tigerbeetle/tigerbeetle/pull/2617) for the Rust TigerBeetle client in June 2025, I had not done this yet, and expected our minimum supported Rust version to be a recent one. Without any effort to support older releases, the client’s initial MSRV was Rust 1.81, published September 5, 2024.

About 9 months of supported toolchains. Not satisfactory, but not surprising.

TigerBeetle has a set of strict and opinionated coding guidelines, [TigerStyle](https://github.com/tigerbeetle/tigerbeetle/blob/main/docs/TIGER_STYLE.md). They are focused on three pillars: safety, performance, and developer UX.

TigerStyle emphasizes fully understanding and owning your code and radically reducing dependencies. It has this to say about them:

> TigerBeetle has a “zero dependencies” policy, apart from the Zig toolchain. Dependencies, in general, inevitably lead to supply chain attacks, safety and performance risk, and slow install times. For foundational infrastructure in particular, the cost of any dependency is further amplified throughout the rest of the stack.

In order to support older Rust toolchains — and as a matter of TigerStyle — one of my first tasks to land the Rust client was to judiciously remove crate dependencies.

At the start of the process the client’s dependencies were thus:

```
name = "tigerbeetle"
version = "0.1.0"
edition = "2021"

[dependencies]
bitflags = "2.6.0"
futures = "0.3.31"
thiserror = "2.0.3"

[build-dependencies]
anyhow = "1.0.93"
ignore = "0.4.23"

[dev-dependencies]
anyhow = "1.0.93"
tempfile = "3.15.0"
```

To Rust programmers this is common stuff, dependencies most of us use. Most of these are easy enough to remove.

The `futures` crate is not easy to remove.

The `futures` crate is critical to the Rust ecosystem. It was where the original futures implementation was prototyped, and continues to maintain code that is so important that it remains all but required for using async Rust. Most sizable Rust projects depend on the `futures` crate. It is an official crate maintained by the Rust project.

It’s a big dependency, as shown in this dependency graph from `cargo tree`:

```
futures v0.3.31
├── futures-channel v0.3.31
│   ├── futures-core v0.3.31
│   └── futures-sink v0.3.31
├── futures-core v0.3.31
├── futures-executor v0.3.31
│   ├── futures-core v0.3.31
│   ├── futures-task v0.3.31
│   └── futures-util v0.3.31
│       ├── futures-channel v0.3.31 (*)
│       ├── futures-core v0.3.31
│       ├── futures-io v0.3.31
│       ├── futures-macro v0.3.31 (proc-macro)
│       │   ├── proc-macro2 v1.0.90
│       │   │   └── unicode-ident v1.0.14
│       │   ├── quote v1.0.37
│       │   │   └── proc-macro2 v1.0.90 (*)
│       │   └── syn v2.0.88
│       │       ├── proc-macro2 v1.0.90 (*)
│       │       ├── quote v1.0.37 (*)
│       │       └── unicode-ident v1.0.14
│       ├── futures-sink v0.3.31
│       ├── futures-task v0.3.31
│       ├── memchr v2.7.4
│       ├── pin-project-lite v0.2.15
│       ├── pin-utils v0.1.0
│       └── slab v0.4.9
├── futures-io v0.3.31
├── futures-sink v0.3.31
├── futures-task v0.3.31
└── futures-util v0.3.31 (*)
```

Notice that `futures` transitively depends on [`syn`](https://crates.io/crates/syn) and [`proc-macro2`](https://crates.io/crates/proc-macro2). These two are fundamental ecosystem crates in their own right, used when implementing macros. They are not official crates but they are critical and tightly related to the Rust compiler, tracking its lexical structure, syntax and macro interfaces.

This is a sticky dependency, tough to eliminate from large Rust programs.

So I did my rework on the Rust client pull request, removing one dependency at a time, reducing the MSRV.

I had succeeding in reducing the MSRV to 1.63 after removing dependencies on those crates which were simplest to remove: `ignore`, `anyhow`, `thiserror`, `tempfile`. Most everything but `futures`.

Then one day I resumed my work and found the Rust client no longer built on our CI: the `syn` crate had published a point release that broke our build. It wasn’t an accidental breaking change. It was [`rust-version`](https://doc.rust-lang.org/cargo/reference/manifest.html#the-rust-version-field). In `syn` version `2.0.107` its `rust-version` changed from 1.61 to 1.68.

My work undone.

This is common in the Rust ecosystem — it is considered a reasonable maintenance strategy to bump `rust-version` in a point release. The reasons are nuanced. Since the original incident the same CI-breaking scenario of a transitive dependency bumping `rust-version` has affected TigerBeetle twice more, and not both due to `syn`.

That was the moment of maximum annoyance, and when I decided to do an experiment to learn more about the state of crate-toolchain compatibility.

I tested the top 100 crates from crates.io by download count, the most recent major releases of each, to find the oldest Rust version each could compile with. For each crate I created a minimal project depending on it, then binary-searched through Rust releases from 1.0 to 1.94 to find the oldest toolchain for which `cargo check` succeeds.

The test run for this blog post was conducted on April 8, 2026.

> Caution: while I have iterated on this experiment and run it many times there are surely mistakes. Further, while there exist techniques to munge lockfiles etc. to achieve compatibility, this experiment is just letting cargo resolve how it wants and seeing what happens.

The chart below shows the results. Each bar represents a crate’s compatibility window — the span of Rust versions from its oldest compatible release to the present.

![Rust Toolchain Horizons - April 2026](https://tigerbeetle.com/blog/2026-04-24-toolchain-horizons/compatibility-timeline-rust-3c724f4c.svg)\
Rust Toolchain Horizons - April 2026

Good news first: I was surprised that old toolchains still install and work, and that some crates actually do remain compatible with them: super-kudos to [`autocfg`](https://crates.io/crates/autocfg), [`fnv`](https://crates.io/crates/fnv), [`mime`](https://crates.io/crates/mime), [`version_check`](https://crates.io/crates/version_check), [`memoffset`](https://crates.io/crates/memoffset), and [`scopeguard`](https://crates.io/crates/scopeguard).

Big kudos to [`serde`](https://crates.io/crates/serde) as well. A lynchpin of the ecosystem, it is compatible back to [Rust 1.31](https://blog.rust-lang.org/2018/12/06/Rust-1.31-and-rust-2018.html), from 2018. That is the toolchain that introduced edition 2018. As a practical matter supporting Rust prior to edition 2018 is unnecessary for any but the super-kudos crates mentioned above. Rust 2015 edition is very old Rust.

There are a large handful of crates hanging out with `serde` in the “yellow” zone in the chart. That zone is the epoch prior to the introduction of `async` / `await`, in [Rust 1.39](https://blog.rust-lang.org/2019/11/07/Rust-1.39.0.html), 2019. Curiously no crates landed directly on 1.39 for their MSRV. As another practical matter this release is probably the hard MSRV cutoff for any async Rust crates. `futures-core` is in this zone, providing ongoing support for the entire `async` / `await` epoch.

Through the remaining gradient of decreasing support we see other scattered futures crates, and many other familiar crates, including `syn` and `proc-macro2`. You can see for yourself the crates at the puny end of the spectrum.

I’ll conclude with some examples of what it looks like to aggressively reduce dependencies and language features to increase toolchain compatibility.

The `futures` crate presents an interesting support situation: it reexports other crates in the same family of futures crates, and each of those subcrates seems to have their own support levels, with `futures-core`, where the key traits are, having great support; `futures-executor`, with its dependency on `syn`, having poor support.

I said before it is “sticky” — used ubiquitously in the ecosystem, providing features that are unsafe, with tricky semantics, that should be implemented once with close scrutiny and reused.

That’s what dependencies are for!

But how can we get rid of it? Here are the 4 steps I needed to take.

The first step was to depend only on the specific subcrates I actually used. For the TigerBeetle client I needed:

- `futures-channel` for [oneshot channels](https://docs.rs/futures-channel/latest/futures_channel/oneshot/index.html)
- `futures-executor` for [`block_on`](https://docs.rs/futures-executor/latest/futures_executor/fn.block_on.html) in tests
- `futures-util` for the [`unfold`](https://docs.rs/futures-util/latest/futures_util/stream/fn.unfold.html) function on [`StreamExt`](https://docs.rs/futures-util/latest/futures_util/stream/trait.StreamExt.html)

The [`block_on`](https://docs.rs/futures-executor/latest/futures_executor/fn.block_on.html) function from `futures-executor` is used to run a future to completion on the current thread. It’s commonly used in tests, examples, and for scheduling non-I/O asynchronous work.

```
pub fn block_on<F: Future>(mut future: F) -> F::Output {
    let mut future = unsafe { Pin::new_unchecked(&mut future) };
    let waker = /* ... */;
    let mut cx = Context::from_waker(&waker);
    loop {
        match future.as_mut().poll(&mut cx) {
            Poll::Ready(result) => return result,
            Poll::Pending => std::thread::park(),
        }
    }
}
```

The tricky part is constructing the “waker”. I won’t go into detail here but it requires some unsafe code.

We use the [`unfold`](https://docs.rs/futures-util/latest/futures_util/stream/fn.unfold.html) function from `futures-util` to write one test case and one doc-comment example, both demonstrating how to use the TigerBeetle client API.

`unfold`’s function signature looks like this:

```
pub fn unfold<T, F, Fut, Item>(init: T, f: F) -> Unfold<T, F, Fut>where
    F: FnMut(T) -> Fut,
    Fut: Future<Output = Option<(Item, T)>>,
```

It’s like a closure-to-iterator adapter but for streams. It converts repeated calls to a future-returning closure into a stream of futures.

[Oneshot channels](https://docs.rs/futures-channel/latest/futures_channel/oneshot/index.html) send a single value between tasks. The TigerBeetle client uses them internally to communicate with an internal I/O thread.

A oneshot channel is just a shared `Option<T>` to transfer a value plus a signal to wake up the reciever. Plus it needs to conform to the [`Future`](https://doc.rust-lang.org/std/future/trait.Future.html) trait.

Easy to implement naively.

After removing every crate dependency, I started removing language and standard library features to achieve even greater compatibility. Below is a summary of the features I had to remove.

| Rust | Date     | Features                                                                                                                                                                                                                                                                                                                  |
| ---- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1.56 | Oct 2021 | [format string captures](https://blog.rust-lang.org/2021/10/21/Rust-1.56.0.html#captured-identifiers-in-format-strings), [`Path::try_exists`](https://doc.rust-lang.org/std/path/struct.Path.html#method.try_exists), [`const Mutex::new`](https://doc.rust-lang.org/std/sync/struct.Mutex.html#method.new)            |
| 1.55 | Sep 2021 | [`rust-version`](https://doc.rust-lang.org/cargo/reference/manifest.html#the-rust-version-field) (stabilized [1.56](https://blog.rust-lang.org/2021/10/21/Rust-1.56.0.html))                                                                                                                                            |
| 1.55 | Sep 2021 | [Edition 2021](https://doc.rust-lang.org/edition-guide/rust-2021/index.html)→2018, [`TryFrom`](https://doc.rust-lang.org/std/convert/trait.TryFrom.html)                                                                                                                                                                |
| 1.53 | Jun 2021 | [`CARGO_TARGET_TMPDIR`](https://doc.rust-lang.org/cargo/reference/environment-variables.html#environment-variables-cargo-sets-for-crates) (stabilized [1.54](https://blog.rust-lang.org/2021/07/29/Rust-1.54.0.html))                                                                                                   |
| 1.51 | Mar 2021 | [`IntoIterator` for arrays](https://blog.rust-lang.org/2021/06/17/Rust-1.53.0.html#intoiterator-for-arrays) (stabilized [1.53](https://blog.rust-lang.org/2021/06/17/Rust-1.53.0.html))                                                                                                                                 |
| 1.50 | Feb 2021 | [const generics](https://blog.rust-lang.org/2021/03/25/Rust-1.51.0.html#const-generics-mvp) (stabilized [1.51](https://blog.rust-lang.org/2021/03/25/Rust-1.51.0.html))                                                                                                                                                 |
| 1.45 | Jul 2020 | [array impls](https://blog.rust-lang.org/2020/10/08/Rust-1.47.html#traits-on-larger-arrays) for lengths > 32 (stabilized [1.47](https://blog.rust-lang.org/2020/10/08/Rust-1.47.html))                                                                                                                                  |
| 1.42 | Mar 2020 | [associated constants](https://blog.rust-lang.org/2020/04/23/Rust-1.43.0.html#associated-constants-for-floats-and-integers) ([`u64::MAX`](https://doc.rust-lang.org/std/primitive.u64.html#associatedconstant.MAX)) (stabilized [1.43](https://blog.rust-lang.org/2020/04/23/Rust-1.43.0.html))                        |
| 1.41 | Jan 2020 | [`matches!`](https://doc.rust-lang.org/std/macro.matches.html) (stabilized [1.42](https://blog.rust-lang.org/2020/03/12/Rust-1.42.html))                                                                                                                                                                                |
| 1.39 | Nov 2019 | [`todo!`](https://doc.rust-lang.org/std/macro.todo.html), [`mem::take`](https://doc.rust-lang.org/std/mem/fn.take.html), [`non_exhaustive`](https://doc.rust-lang.org/reference/attributes/type_system.html#the-non_exhaustive-attribute) (stabilized [1.40](https://blog.rust-lang.org/2019/12/19/Rust-1.40.0.html)) |

No details today, but I’ll do a followup post with more about every crate and every language feature I removed from the TigerBeetle Rust client as well as how I did it. I think it might be interesting to Rust historians.

I achieved compatibility with Rust 1.39, from November 2019. I’m not landing that code. It has too many tradeoffs I’m still uncomfortable with. The in-tree Rust client targets Rust 1.63, from August 2022. About 3.5 years of support.

People have asked me why we should support older toolchains. Some easy answers:

1. **Professional responsibility.** Library authors should know and publish their supported toolchain range. The range itself matters less than having one.
2. **Trust.** When users see effort to support older compilers, they know the maintainer is present and cares.
3. **Reality.** Enterprise deployments, embedded systems, and distro packages often lag the latest toolchain by years.

But there’s a harder answer.

The Rust compiler is stable. The Rust crate ecosystem is not. Crate authors have strong incentives to adopt new features and break from the past. Based on this experiment, I estimate a roughly 2-year window in which any particular Rust compiler remains viable for a project that takes dependencies. After that, we’re all forced to upgrade — not by language changes, but by our crate neighbors.

We can widen that window slowly, but it requires individual crate authors to expand their toolchain horizons. TigerBeetle’s horizons? They’re wide.
