---
title: 'parking_lot: ffffffffffffffff...'
link: https://fly.io/blog/parking-lot-ffffffffffffffff/
source: fly-io
published: 2025-05-28T00:00:00Z
updated: 2026-11-05T00:08:46Z
first_seen: 2026-11-27T00:01:05.630507547Z
content: extracted
html: 2025-05-28-parking-lot-ffffffffffffffff.html
remote_preview:
  url: https://fly.io/blog/parking-lot-ffffffffffffffff/assets/ffff-cover.webp
extra:
  thumbnail: https://fly.io/blog/parking-lot-ffffffffffffffff/assets/ffff-cover.webp
---

![](https://fly.io/blog/parking-lot-ffffffffffffffff/assets/ffff-cover.webp)\
 Image by [Annie Ruygt](https://annieruygtillustration.com/)

We’re Fly.io, a public cloud that runs apps in a bunch of locations all over the world. This is a post about a gnarly bug in our Anycast router, the largest Rust project in our codebase. You won’t need much of any Rust to follow it.

The basic idea is simple: customers give us Docker containers, and tell us which one of 30+ regions around the world they want them to run in. We convert the containers into lightweight virtual machines, and then link them to an Anycast network. If you boot up an app here in Sydney and Frankfurt, and a request for it lands in Narita, it’ll get routed to Sydney. The component doing that work is called `fly-proxy`. It’s a Rust program, and it has been ill behaved of late.

### Dramatis Personae

`fly-proxy`, our intrepid Anycast router.

`corrosion`, our intrepid Anycast routing protocol.

`Rust`, a programming language you probably don’t use.

`read-write locks`, a synchronization primitive that allows for many readers *or* one single writer.

`parking_lot`, a well-regarded optimized implementation of locks in Rust.

Gaze not into the abyss, lest you become recognized as an ***abyss domain expert***, and they expect you keep gazing into the damn thing

*Mathewson [6:31](https://x.com/nickm_tor/status/860234274842324993?lang=en "")*

### Anycast Routing

You already know how to build a proxy. Connection comes in, connection goes out, copy back and forth. So for the amount of time we spend writing about `fly-proxy`, you might wonder what the big deal is.

To be fair, in the nuts and bolts of actually proxying requests, `fly-proxy` does some interesting stuff. For one thing, it’s [written in Rust](https://github.com/jedisct1/yes-rs ""), which is apparently a big deal all on its own. It’s a large, asynchronous codebase that handles multiple protocols, TLS termination, and certificate issuance. It exercises a lot of [Tokio](https://tokio.rs/ "") features.

But none of this is the hard part of `fly-proxy`.

We operate thousands of servers around the world. A customer Fly Machine might get scheduled onto any of them; in some places, the expectation we set with customers is that their Machines will potentially start in less than a second. More importantly, a Fly Machine can terminate instantly. In both cases, but especially the latter, `fly-proxy` potentially needs to know, so that it does (or doesn’t) route traffic there.

This is the hard problem: managing millions of connections for millions of apps. It’s a lot of state to manage, and it’s in constant flux. We refer to this as the “state distribution problem”, but really, it quacks like a routing protocol.

### Our Routing Protocol is Corrosion

Corrosion2, to be precise.

We’ve been through multiple iterations of the state management problem, and the stable place we’ve settled is a [system called Corrosion](https://github.com/superfly/corrosion "").

Corrosion is a large SQLite database mirrored globally across several thousand servers. There are three important factors that make Corrosion work:

1. The SQLite database Corrosion replicates is CRDT-structured.
2. In our orchestration model, individual worker servers are the source of truth for any Fly Machines running on them; there’s no globally coordinated orchestration state.
3. We use [SWIM gossip](https://fly.io/blog/building-clusters-with-serf/#what-serf-is-doing "") to publish updates from those workers across the fleet.

This works. A Fly Machine terminates in Dallas; a `fly-proxy` instance in Singapore knows within a small number of seconds.

### Routing Protocol Implementations Are Hard

A routing protocol is a canonical example of a distributed system. We’ve certainly hit distsys bugs in Corrosion. But this story is about basic implementation issues.

A globally replicated SQLite database is an awfully nice primitive, but we’re not actually doing SQL queries every time a request lands.

In somewhat the same sense as a router works both with a [RIB and a FIB](https://www.dasblinkenlichten.com/rib-and-fib-understanding-the-terminology/ ""), there is in `fly-proxy` a system of record for routing information (Corrosion), and then an in-memory aggregation of that information used to make fast decisions. In `fly-proxy`, that’s called the Catalog. It’s a record of everything in Corrosion a proxy might need to know about to forward requests.

Here’s a fun bug from last year:

At any given point in time, there’s a lot going on inside `fly-proxy`. It is a highly concurrent system. In particular, there are many readers to the Catalog, and also, when Corrosion updates, writers. We manage access to the Catalog with a system of [read-write locks](https://doc.rust-lang.org/std/sync/struct.RwLock.html "").

Rust is big on pattern-matching; instead of control flow based (at bottom) on simple arithmetic expressions, Rust allows you to `match` exhaustively (with compiler enforcement) on the types of an expression, and the type system expresses things like `Ok` or `Err`.

But `match` can be cumbersome, and so there are shorthands. One of them is `if let`, which is syntax that makes a pattern match read like a classic `if` statement. Here’s an example:

```wrapper
if let (Some(Load::Local(load))) = (&self.load.read().get(...)) {
    // do a bunch of stuff with `load`
} else {
    self.init_for(...);
}
```

The “if” arm of that branch is taken if `self.load.read().get()` returns a value with the type `Some`. To retrieve that value, the expression calls `read()` to grab a lock.

though Rust programmers probably notice the bug quickly

The bug is subtle: in that code, the lock `self.load.read().get()` takes is held not just for the duration of the “if” arm, but also for the “else” arm — you can think of `if let` expressions as being rewritten to the equivalent `match` expression, where that lifespan is much clearer.

Anyways that’s real code and it occurred on a code path in `fly-proxy` that was triggered by a Corrosion update propagating from host to host across our fleet in millisecond intervals of time, converging quickly, like a good little routing protocol, on a global consensus that our entire Anycast routing layer should be deadlocked. Fun times.

### The Watchdog, and Regionalizing

The experience of that Anycast outage was arresting, and immediately altered our plans. We set about two tasks, one short-term and one long.

In the short term: we made deadlocks nonlethal with a “watchdog” system. `fly-proxy` has an internal control channel (it drives a REPL operators can run from our servers). During a deadlock (or dead-loop or exhaustion), that channel becomes nonresponsive. We watch for that and bounce the proxy when it happens. A deadlock is still bad! But it’s a second-or-two-length arrhythmia, not asystole.

Meanwhile, over the long term: we’re confronting the implications of all our routing state sharing a global broadcast domain. The update that seized up Anycast last year pertained to an app nobody used. There wasn’t any real reason for any `fly-proxy` to receive it in the first place. But in the *status quo ante* of the outage, every proxy received updates for every Fly Machine.

They still do. Why? Because it scales, and fixing it turns out to be a huge lift. It’s a lift we’re still making! It’s just taking time. We call this effort “regionalization”, because the next intermediate goal is to confine most updates to the region (Sydney, Frankfurt, Dallas) in which they occur.

I hope this has been a satisfying little tour of the problem domain we’re working in. We have now reached the point where I can start describing the new bug.

### New Bug, Round 1: Lazy Loading

We want to transform our original Anycast router, designed to assume a full picture of global state, into a regionalized router with partitioned state. A sane approach: have it lazy-load state information. Then you can spare most of the state code; state for apps that never show up at a particular `fly-proxy` in, say, Hong Kong simply doesn’t get loaded.

For months now, portions of the `fly-proxy` Catalog have been lazy-loaded. A few weeks ago, the decision is made to get most of the rest of the Catalog state (apps, machines, &c) lazy-loaded as well. It’s a straightforward change and it gets rolled out quickly.

Almost as quickly, proxies begin locking up and getting bounced by the watchdog. Not in the frightening Beijing-Olympics-level coordinated fashion that happened during the Outage, but any watchdog bounce is a problem.

We roll back the change.

From the information we have, we’ve narrowed things down to two suspects. First, lazy-loading changes the read/write patterns and thus the pressure on the RWLocks the Catalog uses; it could just be lock contention. Second, we spot a suspicious `if let`.

### New Bug, Round 2: The Lock Refactor

Whichever the case, there’s a reason these proxies locked up with the new lazy-loading code, and our job is to fix it. The `if let` is easy. Lock contention is a little trickier.

At this point it’s time to introduce a new character to the story, though they’ve been lurking on the stage the whole time: it’s [`parking_lot`](https://github.com/Amanieu/parking_lot ""), an important, well-regarded, and widely-used replacement for the standard library’s lock implementation.

Locks in `fly-proxy` are `parking_lot` locks. People use `parking_lot` mostly because it is very fast and tight (a lock takes up just a single 64-bit word). But we use it for the feature set. The feature we’re going to pull out this time is lock timeouts: the RWLock in `parking_lot` exposes a [`try_write_for`](https://amanieu.github.io/parking_lot/parking_lot/struct.RwLock.html#method.try_write_for "")method, which takes a `Duration`, after which an attempt to grab the write lock fails.

Before rolling out a new lazy-loading `fly-proxy`, we do some refactoring:

- our Catalog write locks all time out, so we’ll get telemetry and a failure recovery path if that’s what’s choking the proxy to death,
- we eliminate RAII-style lock acquisition from the Catalog code (in RAII style, you grab a lock into a local value, and the lock is released implicitly when the scope that expression occurs in concludes) and replace it with explicit closures, so you can look at the code and see precisely the interval in which the lock is held, and
- since we now have code that is very explicit about locking intervals, we instrument it with logging and metrics so we can see what’s happening.

We should be set. The suspicious `if let` is gone, lock acquisition can time out, and we have all this new visibility.

Nope. Immediately more lockups, all in Europe, especially in `WAW`.

### New Bug, Round 3: Telemetry Inspection

That we’re still seeing deadlocks is f'ing weird. We’ve audited all our Catalog locks. You can look at the code and see the lifespan of a grabbed lock.

We have a clue: our lock timeout logs spam just before a proxy locks up and is killed by the watchdog. This clue: useless. But we don’t know that yet!

Our new hunch is that something is holding a lock for a very long time, and we just need to find out which thing that is. Maybe the threads updating the Catalog from Corrosion are dead-looping?

The instrumentation should tell the tale. But it does not. We see slow locks… just before the entire proxy locks up. And they happen in benign, quiet applications. Random, benign, quiet applications.

`parking_lot` has a [deadlock detector](https://amanieu.github.io/parking_lot/parking_lot/deadlock/index.html ""). If you ask it, it’ll keep a waiting-for dependency graph and detect stalled threads. This runs on its own thread, isolate outside the web of lock dependencies in the rest of the proxy. We cut a build of the proxy with the deadlock detector enabled and wait for something in `WAW` to lock up. And it does. But `parking_lot` doesn’t notice. As far as it’s concerned, nothing is wrong.

We are at this moment very happy we did the watchdog thing.

### New Bug, Round 4: Descent Into Madness

When the watchdog bounces a proxy, it snaps a core dump from the process it just killed. We are now looking at core dumps. There is only one level of decompensation to be reached below “inspecting core dumps”, and that’s “blaming the compiler”. We will get there.

Here’s Pavel, at the time:

> I’ve been staring at the last core dump from `waw` . It’s quite strange. First, there is no thread that’s running inside the critical section. Yet, there is a thread that’s waiting to acquire write lock and a bunch of threads waiting to acquire a read lock. That’s doesn’t prove anything, of course, as a thread holding catalog write lock might have just released it before core dump was taken. But that would be quite a coincidence.

The proxy is locked up. But there are no threads in a critical section for the catalog. Nevertheless, we can see stack traces of multiple threads waiting to acquire locks. In the moment, Pavel thinks this could be a fluke. But we’ll soon learn that *every single stack trace* shows the same pattern: everything wants the Catalog lock, but nobody has it.

It’s hard to overstate how weird this is. It breaks both our big theories: it’s not compatible with a Catalog deadlock that we missed, and it’s not compatible with a very slow lock holder. Like a podcast listener staring into the sky at the condensation trail of a jetliner, we begin reaching for wild theories. For instance: `parking_lot` locks are synchronous, but we’re a Tokio application; something somewhere could be taking an async lock that’s confusing the runtime. Alas, no.

On the plus side, we are now better at postmortem core dump inspection with `gdb`.

### New Bug, Round 5: Madness Gives Way To Desperation

Fuck it, we’ll switch to `read_recursive`.

A recursive read lock is an eldritch rite invoked when you need to grab a read lock deep in a call tree where you already grabbed that lock, but can’t be arsed to structure the code properly to reflect that. When you ask for a recursive lock, if you already hold the lock, you get the lock again, instead of a deadlock.

Our theory: `parking_lot`goes through some trouble to make sure a stampede of readers won’t starve writers, who are usually outnumbered. It prefers writers by preventing readers from acquiring locks when there’s at least one waiting writer. And `read_recursive` sidesteps that logic.

Maybe there’s some ultra-slow reader somehow not showing up in our traces, and maybe switching to recursive locks will cut through that.

This does not work. At least, not how we hoped it would. It does generate a new piece of evidence: `RwLock reader count overflow` log messages, and lots of them.

### There Are Things You Are Not Meant To Know

You’re reading a 3,000 word blog post about a single concurrency bug, so my guess is you’re the kind of person who compulsively wants to understand how everything works. That’s fine, but a word of advice: there are things where, if you find yourself learning about them in detail, something has gone wrong.

One of those things is the precise mechanisms used by your RWLock implementation.

The whole point of `parking_lot` is that the locks are tiny, marshalled into a 64 bit word. Those bits are partitioned into [4 signaling bits](https://github.com/Amanieu/parking_lot/blob/master/src/raw_rwlock.rs "") (`PARKED`, `WRITER_PARKED`, `WRITER`, and `UPGRADEABLE`) and a 60-bit counter of lock holders.

> Me, a dummy: sounds like we overflowed that counter.
>
> Pavel, a genius: we are not overflowing a 60-bit counter.

Ruling out a genuine overflow (which would require paranormal activity) leaves us with corruption as the culprit. Something is bashing our lock word. If the counter is corrupted, maybe the signaling bits are too; then we’re in an inconsistent state, an artificial deadlock.

Easily confirmed. We cast the lock words into `usize` and log them. Sure enough, they’re `0xFFFFFFFFFFFFFFFF`.

This is a smoking gun, because it implies all 4 signaling bits are set, and that includes `UPGRADEABLE`. Upgradeable locks are read-locks that can be “upgraded” to write locks. We don’t use them.

This looks like classic memory corruption. But in our core dumps, memory doesn’t appear corrupted: the only thing set all `FFh` is the lock word.

We compile and run our test suites [under `miri`](https://github.com/rust-lang/miri ""), a Rust interpreter for its [MIR IR](https://rustc-dev-guide.rust-lang.org/mir/index.html ""). `miri` does all sorts of cool UB detection, and does in fact spot some UB in our tests, which we are at the time excited about until we deploy the fixes and nothing gets better.

At this point, Saleem suggests guard pages. We could `mprotect` memory pages around the lock to force a panic if a wild write hits *near* the lock word, or just allocate zero pages and check them, which we are at the time excited about until we deploy the guard pages and nothing hits them.

### The Non-Euclidean Horror At The Heart Of This Bug

At this point we should recap where we find ourselves:

- We made a relatively innocuous change to our proxy, which caused proxies in Europe to get watchdog-killed.
- We audited and eliminated all the nasty `if-letses`.
- We replaced all RAII lock acquisitions with explicit closures, and instrumented the closures.
- We enabled `parking_lot` deadlock detection.
- We captured and analyzed core dumps for the killed proxies.
- We frantically switched to recursive read locks, which generated a new error.
- We spotted what looks like memory corruption, but only of that one tiny lock word.
- We ran our code under an IR interpreter to find UB, fixed some UB, and didn’t fix the bug.
- We set up guard pages to catch wild writes.

In Richard Cook’s essential [“How Complex Systems Fail”](https://how.complexsystems.fail/ ""), rule #5 is that “complex systems operate in degraded mode”. *The system continues to function because it contains so many redundancies and because people can make it function, despite the presence of many flaws*. Maybe `fly-proxy` is now like the RBMK reactors at Chernobyl, a system that works just fine as long as operators know about and compensate for its known concurrency flaws, and everybody lives happily ever after.

We are, in particular, running on the most popular architecture for its RWLock implementation.

We have reached the point where serious conversations are happening about whether we’ve found a Rust compiler bug. Amusingly, `parking_lot` is so well regarded among Rustaceans that it’s equally if not more plausible that Rust itself is broken.

Nevertheless, we close-read the RWLock implementation. And we spot this:

```wrapper
let state = self.state.fetch_add(
   prev_value.wrapping_sub(WRITER_BIT | WRITER_PARKED_BIT),
                           Ordering::Relaxed);
```

This looks like gibberish, so let’s rephrase that code to see what it’s actually doing:

```wrapper
let state = self.state & ~(WRITER_BIT|WRITER_PARKED_BIT);
```

If you know exactly the state of the word you’re writing to, and you know exactly the limited set of permutations the bits of that word can take (for instance, because there’s only 4 signaling bits), then instead of clearing bit by fetching a word, altering it, and then storing it, you can clear them *atomically* by adding the inverse of those bits to the word.

This pattern is self-synchronizing, but it relies on an invariant: you’d better be right about the original state of the word you’re altering. Because if you’re wrong, you’re adding a very large value to an uncontrolled value.

In `parking_lot`, say we have `WRITER` and `WRITER_PARKED` set: the state is `0b1010`. `prev_value`, the state of the lock word when the lock operation started, is virtually always 0, and that’s what we’re counting on. `prev_value.wrapping_sub()`then calculates `0xFFFFFFFFFFFFFFF6`, which exactly cancels out the `0b1010` state, leaving 0.

As a grace note, the locking code manages to set that one last unset bit before the proxy deadlocks.

Consider though what happens if one of those bits isn’t set: state is `0b1000`. Now that add doesn’t cancel out; the final state is instead `0xFFFFFFFFFFFFFFFE`. The reader count is completely full and can’t be decremented, and all the waiting bits are set so nothing can happen on the lock.

`parking_lot` is a big deal and we’re going to be damn sure before we file a bug report. Which doesn’t take long; Pavel reproduces the bug in a minimal test case, with a forked version of `parking_lot` that confirms and logs the condition.

[The `parking_lot` team quickly confirms](https://github.com/Amanieu/parking_lot/issues/465 "") and fixes the bug.

### Ex Insania, Claritas

Here’s what we now know to have been happening:

1. Thread 1 grabs a read lock.
2. Thread 2 tries to grab a write lock, with a `try_write_for` timeout; it’s “parked” waiting for the reader, which sets `WRITER` and `WRITER_PARKED` on the raw lock word.
3. Thread 1 releases the lock, unparking a waiting writer, which unsets `WRITER_PARKED`.
4. Thread 2 wakes, but not for the reason it thinks: the timing is such that it thinks the lock has timed out. So it tries to clear both `WRITER` and `WRITER_PARKED` — a bitwise “double free”. Lock: corrupted. Computer: over.

[The fix is simple](https://github.com/Amanieu/parking_lot/pull/466 ""): the writer bit is cleared separately in the same wakeup queue as the reader. The fix is deployed, the lockups never recur.

At a higher level, the story is this:

1. We’re refactoring the proxy to regionalize it, which changes the pattern of readers and writers on the catalog.
2. As we broaden out lazy loads, we introduce writer contention, a classic concurrency/performance debugging problem. It looks like a deadlock at the time! It isn’t.
3. `try_write_for` is a good move: we need tools to manage contention.
4. But now we’re on a buggy code path in `parking_lot` — we don’t know that and can’t understand it until we’ve lost enough of our minds to second-guess the library.
5. We stumble on the bug out of pure dumb luck by stabbing in the dark with `read_recursive`.

Mysteries remain. Why did this only happen in `WAW`? Some kind of crazy regional timing thing? Something to do with the Polish *kreska* diacritic that makes L’s sound like W’s? The wax and wane of caribou populations? Some very high-traffic APIs local to these regions? All very plausible.

We’ll never know because we fixed the bug.

But we’re in a better place now, even besides the bug fix:

- we audited all our catalog locking, got rid of all the iflets, and stopped relying on RAII lock guards.
- the resulting closure patterns gave us lock timing metrics, which will be useful dealing with future write contention
- all writes are now labeled, and we emit labeled logs on slow writes, augmented with context data (like app IDs) where we have it
- we also now track the last and current holder of the write lock, augmented with context information; next time we have a deadlock, we should have all the information we need to identify the actors without `gdb` stack traces.
