---
title: 'Async Rust: Where does the scheduler live?'
link: https://herecomesthemoon.net/2026/10/async-rust-where-does-the-scheduler-live/
source: lobste-rs
published: 2026-10-05T18:31:29Z
updated: 2026-10-05T18:31:29Z
first_seen: 2026-10-06T19:48:54.930461426Z
authors:
- herecomesthemoon.net by mond
labels:
- rust
summary: Comments
content: extracted
html: 2026-10-05-async-rust-where-does-the-scheduler-live.html
---

The original idea was to kick off this article with a short list of complaints people have about async Rust. Y’know, the type you often see in the wild, justified or otherwise.

I then realized that it’d easily be possible to find enough material riffing on async Rust to cover a whole bingo card, and that this would make for a *much funnier* format. The next time you see people arguing about async Rust, see if you can get a bingo!

![A 5×5 bingo card of common async Rust complaints.](https://herecomesthemoon.net/2026/10/async-rust-where-does-the-scheduler-live/images/bingo_hu_75a868d403b9a6d5.webp)

 A 5×5 bingo card of common async Rust complaints.

 Toggle original/dithered image

Let me know if I missed any!!

Bonus points for people discussing [Zig’s approach to async](https://andrewkelley.me/post/zig-new-async-io-text-version.html).

Since you are reading this article, you’ve (most likely) already heard some of these. For the others, I assembled a dropdown of suggested reading material, which should get you up to speed. It’s a good way to spend an afternoon, but by no means required reading for this article.

Async Rust Bingo Cheat Sheet

**NOTE:** This list is in grid-reading order.

* * *

- [Scoped task trilemma](https://without.boats/blog/the-scoped-task-trilemma/): Roughly speaking, you can only pick two out of three: Concurrency, parallelism, borrowing. This is downstream of the [leakpocalypse](https://faultlore.com/blah/everyone-poops/). tl;dr: It’s possible to leak memory, so you cannot guarantee that destructors run. As a result, the original scoped threads API allowed creating data races, very bad.
- Function coloring: Originally the classic [Bob Nystrom post](https://journal.stuffwithstuff.com/2015/02/01/what-color-is-your-function/). Much ink has been spilled on the topic, see e.g. Without Boats’ [own take](https://without.boats/blog/let-futures-be-futures/). There’s so many posts discussing function coloring that I’m not even going to bother to list them, and people regularly disagree on what function coloring even means.
- [Async Closures](https://rust-lang.github.io/rfcs/3668-async-closures.html): Basically, it took like 6 years to land async closures. The short summary is “async closures need to be lending, since they return futures that may borrow from the closure’s captures”, see [this post](https://hackmd.io/@compiler-errors/async-closures). You may hear some mumbling about [higher-kinded types](https://langdev.stackexchange.com/questions/3222/what-is-the-difference-between-gat-and-hkt) if you spend too much time looking into the topic, so brace yourself.
- Tokio Defaultism: The notion of Tokio as the only runtime that matters. Bonus points for people not realizing that what they’re talking about only applies to Tokio, not to async Rust in general. See this [Corrode.dev blog article](https://corrode.dev/blog/async/) for an overview.
- “just use threads(tm)”: The notion that “almost no one needs async”, and that most people should “just use a lot of threads”. The most common variant of this reasoning is the idea that async only ever becomes necessary if you are writing a server that has to support 10k+ connections at the same time, and that otherwise you can “just use a thread pool”.

* * *

- Complex compiler errors / traces: One of the single most common complaints about async Rust.
- [Async Drop](https://without.boats/blog/poll-drop/): Due to the nature of the Rust `Drop` trait, all destructors are synchronous. The issue is that we may have to perform blocking clean-up actions. In an async context, we want to be able to yield control, so we don’t block any threads.
- Futures are lazy: In JavaScript, any created Future immediately starts executing (they call them Promises over there, but shhh). In Rust, this is not the case! A future is “just” a struct. You have to `.await` (or spawn) it for it to make any progress, otherwise it doesn’t move at all. Forgetting to do this is a common beginner mistake.
- “rust used to have green threads” / stackful coroutines: Green threading is a model in which Rust brings its own runtime. Think of Go’s goroutines. The RFC that removed them can be found [here](https://rust-lang.github.io/rfcs/0230-remove-runtime.html). Rust creator Graydon Hoare’s [reflection on his old vision for Rust](https://web.archive.org/web/20260724025051/https://graydon2.dreamwidth.org/307291.html) is also insightful. (Archived link.)
- Futures are not runtime agnostic: Task spawning is dependent on the runtime and has various assumptions baked into it. Certain matters are baked into traits. Do it wrong, and you get [errors like this](https://users.rust-lang.org/t/there-is-no-reactor-running-must-be-called-from-the-context-of-a-tokio-1-x-runtime/75393/3). See also, [this discussion](https://github.com/rust-lang/wg-async/issues/45) of writing libraries that can be used across runtimes. In general, when people talk about an “ecosystem split”, this is one of the issues they’re referring to. The other one is that async splits the entire Rust ecosystem into async and non-async libraries.

* * *

- `select!` / cancellation safety: See the following excellent transcribed talk, [‘Cancelling Async Rust’](https://sunshowers.io/posts/cancelling-async-rust/). The Tokio `select!` macro specifically is notorious, which is why its docs have a whole section on [cancellation safety](https://docs.rs/tokio/latest/tokio/macro.select.html#cancellation-safety). Accidentally dropping futures in `select!` branches might be *the* poster child of async Rust footguns.
- `Send + Sync + 'static` bound proliferation: See e.g. [the tokio docs](https://tokio.rs/tokio/tutorial/spawning) on how `Send` bounds etc. creep into the program. People complain about having to annotate everything with these bounds.
- lack of ergonomics (free space): [No comment](https://goals.rust-lang.org/2024h2/async.html). See also, for [2026 goals](https://goals.rust-lang.org/2026/roadmap-just-add-async.html).
- [Futurelock](https://rfd.shared.oxide.computer/rfd/0609): “a type of deadlock where a resource owned by Future `A` is required for another Future `B` to proceed, while the Task responsible for both Futures is no longer polling `A`.” Please don’t ask me to explain this thing!
- effect system / keyword generics / maybe async: See the [keyword generics initiative](https://blog.rust-lang.org/inside-rust/2022/07/27/keyword-generics/). The idea here is that instead of having to write sync and async functions, you only write a single “maybe async” function. Then, the compiler creates both versions of the function beneath the hood (similar to how generics work) and dispatches the correct one based on calling context. I was always skeptical of this.

* * *

- APIT, RPIT, TAIT, RPITIT: See [here](https://rust-lang.github.io/impl-trait-initiative/explainer.html) for an explainer. These all refer to the ability to use `impl Trait` in various places where it was previously not possible. Most of them are easy to look up, but also highly technical.
- dyn compatibility, `Pin<Box<dyn Future<Output = T> + Send>>`: The ability to do dynamic dispatch. Due to limitations this may require boxing, which is a bit frustrating, and results in these hilariously verbose types.
- io\_uring: `io_uring` is a new (2019) asynchronous I/O interface of the Linux kernel. It has better performance than the older `epoll` (or at least requires fewer syscalls). In `io_uring` APIs the kernel owns your buffer until the operation finishes, which fights with futures being cancellable by drop. Fixing it in Tokio requires moving buffers around, instead of passing `&mut` references to them. This would require reworking Tokio’s APIs. See [here](https://github.com/tokio-rs/tokio-uring/blob/master/DESIGN.md) for the `tokio-uring` design. Afaik Tokio [still doesn’t fully support](https://github.com/tokio-rs/tokio/issues/7266) `io_uring`, partially due to API compatibility guarantees. Some other runtimes do.
- “actually, async is important for the embedded space”: [Embassy](https://embassy.dev/) is the prominent example of an embedded async Rust executor. Also, see [Without Boats’ Why Async Rust](https://without.boats/blog/why-async-rust/). One important thing to understand is that Rust cannot have a built-in runtime or green threads since that conflicts with the use case of Rust for embedded devices. It’s an *incredibly valid* point, but people bringing up embedded programming during async Rust discussions has almost become a meme in its own right.
- `Box::pin(future)` overflowing the stack: Futures are a state-machine describing all possible states some async functions can be in. If these async functions are severely nested, they may blow the stack. This is especially funny if you tried to put it in a `Box`, and ran into the lack of in-place heap construction. See e.g. [here](https://hegdenu.net/posts/how-big-is-your-future/).

* * *

- thread per core vs. work stealing: See e.g. [this post](https://maciej.codes/2022-06-09-local-async.html). Tokio is work-stealing, which has attracted some discussion over the years. Work-stealing = tasks can be moved from one CPU core to another, ensuring that each thread always has work to do, but requiring all tasks to have `Send` bounds. There are some alternative runtimes, e.g. [Glommio](https://www.datadoghq.com/blog/engineering/introducing-glommio/), which move away from that model for various reasons. See [Without Boats’ response](https://without.boats/blog/thread-per-core/) to the above post.
- “the std should ship an executor”: Broadly, the notion that Rust should ship with some sort of runtime (e.g. Tokio or a super primitive executor). The obvious problem is that there is *genuine need* for different executors, and adding one to the standard library would conflict with Rust’s goals of shipping a lean standard library. The more sympathetic notion is the idea that the Rust standard library should (somehow) [standardize APIs for spawning tasks](https://without.boats/blog/global-executors/), and move in a direction that ensures that different runtimes can play nicely together. Making this work would be a hard problem, since runtimes have different expectations: Work stealing ones (e.g. Tokio) require `Send` bounds because Futures may be sent between threads, but this limitation does not apply to thread per core runtimes.
- generators / yield / AsyncIterator / coroutines: See e.g. [here](https://without.boats/blog/poll-next/). These are still unstable, [and not a priority for 2026](https://goals.rust-lang.org/2026/roadmap-just-add-async.html#what-about-async-iterators--streams). Afaik there were large disagreements over the shape of the precise trait that should be used to encode the concept of async iterators. Also, you run into lending iterators again. tl;dr, the scope goes beyond Python-like `yield` keyword blocks.
- [spawn\_blocking](https://ryhl.io/blog/async-what-is-blocking/): Broadly, the whole topic of “How do I spawn a blocking task without causing problems for my program?”. Specifically, the footgun is that if you accidentally block a Tokio worker thread, your program may keep limping along (work-stealing, so the remaining work of that worker thread will be stolen by other threads), but your worker pool is now down one full worker thread, for as long as the call blocks. This can be really hard to notice as long as traffic is low.
- Pin issues + [Move trait](https://goals.rust-lang.org/2026/move-trait.html): Many, many words have been spilled on how nice it would be to have a proper `Move` trait instead of having to deal with `Pin`. As always, read [Without Boats’ post](https://without.boats/blog/pin/) to understand `Pin`.

### What’s the deal?

The thing that bothers me about async Rust is that it requires a runtime.

Why? Why does it require a runtime? Nothing else in Rust requires a runtime[^1]. The single claim to fame of Rust is that it gives you memory safety *without requiring a runtime (such as a garbage collector)*. Going back to a runtime-based model feels like a regression.

The instant you have a runtime, you are putting yourself in the hands of a diffuse (potentially completely inscrutable) prioritization and scheduling mechanism. You don’t run your own functions, you instead hand them to an engine which runs them for you, according to its own rules, instead of going through the code one step at a time as god intended, darn it.

And it gets worse! Scheduling algorithms are necessarily optimization problems, requiring you to pick a specific point on a *tradeoff boundary*! There is no one-size-fits-all solution! How frustrating! How can we talk about Rust’s performance being world-class if I still end up tweaking the Tokio runtime like it’s the Java garbage collector, just to squeeze out a little bit more performance?

That’s exactly what I wanted to get away from!

So.

At this point, if you’ve made it through the previous paragraphs without closing the tab to send me a strongly worded e-mail, congratulations!

In short, that whole gripe was nonsense.

Let me explain.

These issues (runtime-feeling, scheduler optimization tradeoffs, complexity) are *general issues of concurrent code*. If you want code to run concurrently, you need a runtime to handle scheduling. That’s practically built into the definition of concurrency.

If you avoid async Rust altogether and use threads, all that changes is that now *the kernel* is your scheduler instead of Tokio. You still have a scheduler, with all (or well, at least some) of the same problems!

In other words, please don’t blame Rust for *exposing complexity* to you, the user. Rust is a low-level language (with high abstractions, and ambition for high performance). Exposing this complexity is what it does. Rust cannot choose your scheduler for you, because there is no perfect scheduler, and different schedulers have different use cases.

Languages like Go have it easier: No one expects Go to have bare-metal performance. Everyone understands that there’s a tradeoff here. You sacrifice some performance in exchange for convenience, and that’s why Go has goroutines(tm), i.e. green threads.

The humble Gopher stays in its lane, unbothered, moisturized, flourishing, and forever blissfully ignorant of the call of [sum types](https://herecomesthemoon.net/2025/03/multiple-return-values-in-go/) and pattern matching.

Unlike Go, Rust is aiming a little higher.

side note: async Rust performance

For the record, there’s this belief that async Rust is “as good as it gets”, in some vague, diffuse way. This isn’t really the case. Obviously “as good as it gets” is too vague and undefined to have any truth value assigned to it, but there is at least one genuine misconception here. (Ignoring the THERE IS NO PERFECT SCHEDULER misconception.)

Example: A common belief is that Rust futures compile down to an “optimally sized state-machine, that is exactly as large as it has to be to hold all the data”. I assume that notion may have originated in [Aaron Turon’s ‘Zero-cost futures in Rust’](https://aturon.github.io/blog/2016/08/11/futures/) and in [Without Boats’ ‘Why Async Rust?’](https://without.boats/blog/why-async-rust/) and then took on a life of its own. That’s the idea, but there exist a variety of long-standing issues, [such as arguments being twice as expensive if held across yield points](https://github.com/rust-lang/rust/issues/62958):

```rust
async fn test(arg: [u8; 8192]) {
    wait().await;
    drop(arg);
}

async fn wait() {}

fn main() {
    // Expected: 8194 == 2 + 8192
    // Actual: 16386 == 2 + 2 * 8192
    println!("{}", std::mem::size_of_val(&test([0; 8192])));
}
```

[Link to Rust Playground. Fun fact: Remove the `drop(arg);` call and see what happens](https://play.rust-lang.org/?version=nightly&mode=release&edition=2018&code=async+fn+test%28arg%3A+%5Bu8%3B+8192%5D%29+%7B%0A++++wait%28%29.await%3B%0A++++drop%28arg%29%3B%0A%7D%0A%0Aasync+fn+wait%28%29+%7B%7D%0A%0Afn+main%28%29+%7B%0A++++%2F%2F+Expected%3A+8194+%3D%3D+2+%2B+8192%0A++++%2F%2F+Actual%3A+16386+%3D%3D+2+%2B+2+*+8192%0A++++println%21%28%22%7B%7D%22%2C+std%3A%3Amem%3A%3Asize_of_val%28%26test%28%5B0%3B+8192%5D%29%29%29%3B%0A%7D%0A).

Looking at it from the outside, the idea that this is twice as expensive as it “has to be” and doesn’t get optimized away is absurd. What’s even more macabre is that the size blowup is exponential if you’re passing futures as arguments. (Shoutout to [Ding](https://github.com/dingxiangfei2009), who’s been fighting a valiant battle to resolve this problem once and for all. See [this thread](https://github.com/rust-lang/rust/pull/135527).)

### So, what’s the problem here?

First, concurrency (in some form or another) is necessary.

Second, concurrency *always requires a runtime*. You cannot have concurrency without a runtime. Where does the runtime live, and who manages the stack?

Third, if you wanted to avoid a runtime, the best you can do is to write your own event-loop (congratulations, now you are maintaining your own runtime, great job), or to “just use threads” (congratulations, now you are using the kernel as a runtime, requiring you to juggle kernel threads that are a quintillion times as heavy as specialized Tokio tasks, great job).

Fourth, you cannot do “what Go does” and ship a runtime [without making Rust harder to embed and imposing a cost at the FFI boundary](https://without.boats/blog/why-async-rust/). (Seriously, read that post to understand why. See [RFC 230 to see the rationale for removing Rust’s original green threading](https://rust-lang.github.io/rfcs/0230-remove-runtime.html).)

So fifth, having exhausted the available options, we end up where we are today: A runtime is a crate. You pull it in, initialize it, maybe pass it around, and use it to schedule your futures.

Great.

We’ve reinvented exactly what we started with.

I spent a lot of time going over the design decisions involved here, trying my best to complain about async Rust, only to end up going “Oh. Yeah. That’s reasonable. That makes sense. I can see why they did it that way.” every step of the way.

I still don’t know if there’s some yet-undiscovered abstraction that magically evaporates half of the problems people have with async Rust, but I’m inclined to say no: Many of them are general concurrency problems. The scheduler has to live somewhere, and Rust doesn’t get to trade performance for convenience.

It makes sense to split those problems between inherent concurrency problems, and issues downstream of the decision to have the scheduler live in a crate.

The crate thing is how you get Tokio defaultism, and a lack of runtime agnosticism. It’s also why you have to deal with `Send + 'static` bounds. `Send` bounds spread through generic code since the language itself cannot know whether your runtime will move tasks between threads.

In a lot of ways, having the scheduler live *in library code* is fine. It was the right tradeoff for Rust.

It’s important to understand that the decision to eschew green threads codified a hard design principle about Rust that was, at the time, still in flux: For a while it was not clear that Rust was going to go down the route of being a C++-replacement, usable in embedded `no_std` environments, and with “zero-cost abstractions”.

In hindsight, it looks like this direction was the right call. It allows Rust to stand out next to C#, or D, or Swift, as a language that’s truly a stand-in for C++, but memory safe. No ifs or buts, no attempt to sneak in garbage collecting or refcounting through the backdoor while no one is looking. Bare metal, everything that C++ can do, but memory safe.

We don’t know what the counterfactual world in which Rust doubled down on green threading looks like, but frankly, I assume that it’s not a world in which Rust entered the Linux kernel. It doesn’t seem likely, on a technical and political level.

### So, what’s left?

Back to my actual annoyances.

#### User-level threading?

Isn’t it *incredibly silly* how much time we spent reinventing and litigating new runtimes (such as Tokio or Go’s) *inside* of our programming languages?

Here’s a spicy take: Maybe we should “just” somehow fix kernel threading and scheduling, or introduce a new type of kernel thread, such that “just use threads” suddenly becomes efficient, and we can “just” multi-thread our code?

Step three above is predicated on the assumption that kernel threads are expensive, and that you lose something by moving to them. In the ideal world, *surely* this would not have to be the case. I don’t care how, give the kernel a laughably cheap Go-like scheduler with cheap and simple threads.

You may assume that’s impossible, somehow, but *we are operating at a lower level of abstraction, and more closely with the kernel*. In other words, there are *fewer* limitations binding us, not more. Goroutines are an abstraction running on top of the kernel. If it’s possible to make goroutines efficient, cheap, and fast, then it should be possible for the kernel to support some variant of efficient, cheap, and fast threading.

Apparently, yes, [people have tried this a few times](https://lkml.iu.edu/hypermail/linux/kernel/2007.2/09075.html), usually under a name like ‘M:N user-level threading’. I don’t have it in me to do research on that today, but figuring out which types of programs would benefit from these and what the limitations are is an interesting question.

In practice, the kernel will never know as much about your code as a language-specific runtime. This limits its ability to schedule your threads, and probably means that language-specific runtimes will always(?) come out on top, unless you find a way to hand the kernel all the additional data which it needs.

In either case, even in the ideal case this just has us moving back to a world in which everyone uses threads for everything… just with higher performance. Is that better? I don’t know. Opinions may vary.

#### Tokio

Oh, before I forget it: This is another gripe, but Tokio’s implicit ambient executor model is a little frustrating, and damages the spirit of Rust’s [golden rule](https://steveklabnik.com/writing/rusts-golden-rule/)[^2].

A spawned Tokio-task will magically attach itself to an ambiently-looming thread-local context/executor, and break with a runtime panic if such an executor doesn’t exist. [Example](https://play.rust-lang.org/?version=stable&mode=debug&edition=2024&code=fn+record_metric%28_value%3A+u64%29+%7B%0A++++tokio%3A%3Aspawn%28async+move+%7B%0A++++++++%2F%2F+do+stuff%0A++++%7D%29%3B%0A%7D%0A%0Afn+checksum%28data%3A+%26%5Bu8%5D%29+-%3E+u64+%7B%0A++++let+sum+%3D+data.iter%28%29.map%28%7C%26b%7C+b+as+u64%29.sum%28%29%3B%0A++++record_metric%28sum%29%3B%0A++++sum%0A%7D%0A%0A%23%5Btokio%3A%3Amain%5D%0Aasync+fn+main%28%29+%7B%0A++++let+data+%3D+vec%21%5B1u8%3B+1024%5D%3B%0A++++checksum%28%26data%29%3B+%2F%2F+fine%0A%0A++++let+%28tx%2C+rx%29+%3D+tokio%3A%3Async%3A%3Aoneshot%3A%3Achannel%28%29%3B%0A++++rayon%3A%3Aspawn%28move+%7C%7C+%7B%0A++++++++let+_+%3D+tx.send%28checksum%28%26data%29%29%3B+%2F%2F+panics%2C+process+abort%0A++++%7D%29%3B%0A++++println%21%28%22%7B%3A%3F%7D%22%2C+rx.await%29%3B%0A%7D%0A):

```rust
fn record_metric(_value: u64) {
    tokio::spawn(async move {
        // do stuff
    });
}

fn checksum(data: &[u8]) -> u64 {
    let sum = data.iter().map(|&b| b as u64).sum();
    record_metric(sum);
    sum
}

#[tokio::main]
async fn main() {
    let data = vec![1u8; 1024];
    checksum(&data); // fine

    let (tx, rx) = tokio::sync::oneshot::channel();
    rayon::spawn(move || {
        let _ = tx.send(checksum(&data)); // panics, process abort
    });
    println!("{:?}", rx.await);
}
```

In practice, it’s as if there’s a ‘secret argument’ that gets passed around by all of your Tokio functions. Except if you forget to speak the magic incantation (i.e. you try to use Tokio outside of the context of a Tokio runtime), you get a runtime error. Side effects! Hidden global-ish variables! Hidden dynamic scoping! Bad![^3]

In the “ideal world” (which many other than me would hate, since I’m a sicko) you cannot spawn tasks without having to pass the executor all the way through your code, to the place where you spawn the task.

If people *really* want to reach for the Tokio executor without passing it through, it should be sitting in a global variable, and needs to be grabbed from there, *at the call-site*. You want this property to be [greppable](https://morizbuesing.com/blog/greppability-code-metric/), not hidden.

#### Zig

(Here’s where I claim my ‘discussing Zig’ bingo bonus points.)

For an example of how a principled version of that might look in practice, we can take a look at Zig: You pass around an I/O interface to all functions that need it. Async-ness and I/O are then properties baked into the exact implementation which you are passing around.

```zig
const std = @import("std");
const Io = std.Io;

fn saveData(io: Io, data: []const u8) !void {
    const file = try Io.Dir.cwd().createFile(io, "save.txt", .{});
    defer file.close(io);

    try file.writeAll(io, data);

    const out: Io.File = .stdout();
    try out.writeAll(io, "save complete");
}
```

Example from [Loris Cro’s post on the topic](https://kristoff.it/blog/zig-new-async-io/).

The I/O interface defines the contract for the runtime.

This (in principle) gives you three magical features, assuming everyone is strict about passing the interface around:

1. You can track exactly where blocking operations may happen.
2. You avoid function coloring[^4]. It’s a function parameter.
3. All library code will be agnostic to the executor or scheduler: You can pass a different implementation of the I/O interface into the code. No split ecosystem.

re: The question posed by the title, putting the scheduler into a parameter we pass around feels right. It’s verbose, but it solves a lot of problems. Hell, it solves like a third of the bingo card.

That’s where I’d like the scheduler to live. Not in the kernel, not in some ambient execution context, just make it something we can pass around as a parameter and swap out as needed.

That said, Zig is not stable yet, and async is *very difficult* to get right. In other words, this approach hasn’t been proven in the wild yet.

If the Zig team can make it work, then I am confident that an experiment to evolve Rust in that direction would be worthwhile.

Going all-in would require a rewrite of the Rust standard library APIs (e.g. `std::fs`), so that’s almost certainly off the table, but the crate ecosystem lives under no such constraints. Maybe it’s possible.

I wish the Zig team all the success that they deserve in these exciting times.

[^1]: There’s going to be at least one person in the comments who’ll point out that Rust’s `unwind` / panic stack is considered a runtime. They will be right, but deserve a wedgie anyway.

[^2]: I know, I know. Rust doesn’t have an effect system. It cannot even track whether code is capable of panicking, let alone track side effects. I’m asking for too much here, but I don’t like that it’s not tracked at compile time.

[^3]: Yes, Tokio has [`Handle::spawn`](https://docs.rs/tokio/latest/tokio/runtime/struct.Handle.html), but the ambient form is the default.

[^4]: Or rather, function coloring is encoded as a parameter passed to the function, not as a special language feature. Whether this is function coloring or not is something people will never be able to agree on.
