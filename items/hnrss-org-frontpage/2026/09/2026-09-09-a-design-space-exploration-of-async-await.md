---
title: A Design Space Exploration of Async/Await
link: https://cel.cs.brown.edu/blog/design-space-async-await/
source: hnrss-org-frontpage
published: 2026-09-09T13:59:50Z
updated: 2026-09-09T13:59:50Z
first_seen: 2026-09-12T19:37:44.082475059Z
authors:
- wcrichton
summary: 'Article URL: https://cel.cs.brown.edu/blog/design-space-async-await/ Comments URL: https://news.ycombinator.com/item?id=49626718 Points: 398 # Comments: 114'
content: extracted
html: 2026-09-09-a-design-space-exploration-of-async-await.html
preview:
  file: 2026-09-09-a-design-space-exploration-of-async-await.preview-43787186adf5.webp
  width: 209
  height: 256
  color: '#66474a'
images:
- source: https://cel.cs.brown.edu/assets/cel-spaced-BGjllCyA.webp
  original:
    file: 2026-09-09-a-design-space-exploration-of-async-await.image-ce505368148a.webp
    width: 749
    height: 916
  variants:
  - file: 2026-09-09-a-design-space-exploration-of-async-await.image-b55dfcde6394.webp
    width: 48
    height: 59
  color: '#fafafa'
---

Many programming languages now provide the `async`/`await` keywords for expressing concurrency. The design rationale is pretty consistent: to make concurrent programs look more like straight-line code (see: [Python](https://peps.python.org/pep-0492/), [Rust](https://rust-lang.github.io/rfcs/2394-async_await.html), or [Swift](https://github.com/swiftlang/swift-evolution/blob/main/proposals/0296-async-await.md)). We therefore describe the paradigm which encompasses async/await as *straight-line asynchrony*, as opposed to using event loops or callbacks.

Language designs for straight-line asynchrony have been brewing for over 15 years. In this project, we wanted to understand: how similar or different is async/await between languages? The short answer is a lot more different than we expected. We wrote a paper, [“A Design Space Exploration of Async/Await”](https://cel.cs.brown.edu/paper/a-design-space-exploration-of-aa/), to explain how.

## So you think you know async/await?

To demonstrate how much modern languages can diverge, here’s a small async program written in pseudocode. One function writes to a log, and another fires off the log write as a background task and moves on.

```
async fn write_to_log():
  print("A")
  // simulate a slow log write
  await sleep(2)
  print("B")

async fn fire_and_forget():
  task = spawn write_to_log()
  // return without awaiting the task

async fn main():
  await fire_and_forget()
  await sleep(1)
  print("C")
```

What would you expect this program to print?

There isn’t really a right answer, because you were probably right *for some language*. Below is how seven modern async runtimes actually behave:

Four different answers, for a program whose entire job is to write a log line in the background. And it gets worse. [In the paper](https://cel.cs.brown.edu/paper/a-design-space-exploration-of-aa/), we show that across the seven runtimes, *no two* produce the same output for three variations of this simple program!

Do you *actually* know your language’s async semantics?

## Why the disagreement?

While watching videos from [a programming influencer](https://youtu.be/hEIBsqP63Pg?si=i875SGEnUkcUQkOS) you may have have heard the terms “cold” or “hot” async function calls. The idea is that “hot starts” return a task that is immediately running in the runtime, while “cold starts” return an inert object that does nothing until awaited.

Hot vs. cold functions is what we call an async *design dimension:* a design decision that affects the observable semantics of program execution (as opposed to matters of pure performance). We call this particular dimension “Eagerness”, and [in the paper](https://cel.cs.brown.edu/paper/a-design-space-exploration-of-aa/) we identify nine such dimensions from modern implementations of straight-line asynchrony. Below we’ve grouped these nine dimension into three categories that roughly correspond to the lifetime of a task: Start of Life, End of Life, and Cancellation.

Click a language to trace its design choices through the table.

Start of Life

Eagerness

How to evaluate an async function application.

Lazy

Evaluate to a coroutine without executing further.

·

Eager

Evaluate in current thread, and schedule as task on await.

·

Suspension

Guarantees on whether await points suspend.

Static

Await points guaranteed to suspend.

Dynamic

No guarantees on awaiting tasks.

· · · · ·

End of Life

Extent

The default interval of time during which a task may exist.

Indefinite

Tasks by default may exist until the end of the runtime.

· · · ·

Dynamic

Tasks by default may exist until the end of their spawning scope.

·

Reference Strength

*\[For Indefinite Extent\]* The type of reference to a task held by the runtime.

Strong

The runtime holds a strong reference.

· ·

Weak

The runtime holds a weak reference.

·

Destruction

How a task is cleaned up at the end of its extent.

Awaited

The task is awaited to completion.

·

Cancelled

The task is cancelled, and then possibly awaited.

· · ·

Terminated

The program exits.

Propagation

What happens to exceptions in unawaited tasks.

Destructive

Exceptions are reraised by dependents.

Never

The exception is kept within the task.

· · · · ·

Cancellation

Awareness

Whether a task is able to respond to being cancelled.

Unaware

The task cannot respond to being cancelled.

Aware

The task can respond to being cancelled.

· ·

Direction

How cancellation is communicated through the task graph.

Top-Down

Starting from the root task, and communicated from dependents to dependencies.

Bottom-Up

Starting from the root’s dependencies and communicated to dependents.

·

Simultaneous

To all transitive dependencies at once.

Persistence

*\[For Aware Cancellation\]* How long a cancellation of a task lasts.

Transient

A task can ignore cancellation and proceed as normal.

Persistent

A task can ignore cancellation but remains cancelled.

·

Two of these axes are particularly relevant for our example program. Languages with *Dynamic Extent* do not allow tasks to outlive the functions in which they were spawned. Unlike the other languages, Swift and Python+Trio chose Dynamic Extent. This means that within the function `fire_and_forget`, the task associated with `write_to_log` cannot outlive the function `fire_and_forget`.

Although Swift and Trio both chose Dynamic Extent, they differ in choice of Destruction. At the end of the `fire_and_forget` function scope, Swift uses *Cancelled Destruction,* and cancels `task` while Trio uses *Awaited Destruction* and politely waits for `write_to_log` to finish. The choices of Extent and Destruction explain why Swift prints “AC” and Trio prints “ABC”.

Each design dimension has trade-offs of performance, memory usage, ergonomics, semantics, etc. There’s no right or wrong answers, and each language has its own design rationale. But with so many decisions, explaining the output for even small programs becomes quite involved!

To make our design space more precise, we translated it into a formal semantics on a core calculus of asynchronous programs. This model lets us explain exactly why the sample program diverges by tracing its execution.

The figure below provides a glimpse of how this formal model can be used to explain different execution outcomes. The figure shows the trace of the model, highlighting the semantic decisions that lead to different outcomes. Each box is an abstract-machine state. Each arrow is a small-step reduction, labeled with the rules that fire. Most rules behave identically in every runtime; the highlighted ones are the design decisions, and each highlight is a fork in the road.

async fn write\_to\_log():

 print “A”; await sleep(2); print “B"

async fn fire\_and\_forget():

 task = spawn write\_to\_log()

async fn main():

 await fire\_and\_forget();

 await sleep(1); print “C"

block\_on(main())

out: ε

acjkmrs

C\[ spawn write\_to\_log() \]

out: ε

acjkmrs

C\[ task \]

T: (2, write\_to\_log, print “B”)

out: A

cj

C\[ task;

 cancel task;

 try

 await task

 catch e -> ()

\]

Q: print “A”; await sleep(2); print “B"

out: ε

s

C\[ spawn coro \]

out: ε

akmr

C\[ task \]

Q: print “A”; await sleep(2); print “B"

out: ε

akm

C\[ task; await task \]

Q: print “A”; await sleep(2); print “B"

out: ε

r

block\_on(())

T: (2, write\_to\_log, print “B”)

out: AC

acjk

C\[ throw “cancelled” \]

out: A

s

(); await sleep(1); print “C"

out: A

s

(); await sleep(1); print “C"

out: ε

m

(); await sleep(1); print “C"

out: AB

r

out: ACB

j

out: C

m

out: AC

acks

out: ABC

r

Block-Wait Async-App Await

Async-Appeager OS-IO

Async-Appsemi-eager

Async-Applazy

Spawndynamic

Spawnindefinite

OS-IO Signal Schedule

Schedule OS-IO Signal

Cancel-Unstarted

OS-IO Signal Schedule Block-Done

Schedule OS-IO Signal Await-Task

OS-IO Signal Schedule Block-Done

Schedule OS-IO

Await Catch-Exn

OS-IO Signal Schedule Block-Done

Block-Doneterminated

Signal Schedule Block-Doneawaited

languages

a = Asyncio

c = C#

j = JavaScript

k = Tokio

m = Smol

r = Trio

s = Swift

To understand this diagram, and the decisions that went into designing your favorite language’s async/await system, read our new paper [“A Design Space Exploration of Async/Await”](https://cel.cs.brown.edu/paper/a-design-space-exploration-of-aa/)!
