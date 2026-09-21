---
title: The Golden Spike, and Resurrecting the Vale(n) Programming Language
link: https://verdagon.dev/blog/golden-spike-reviving-vale-valen
source: lobste-rs-top-1w
published: 2026-09-17T15:10:44Z
updated: 2026-09-17T15:10:44Z
first_seen: 2026-09-21T17:01:01.954977951Z
authors:
- verdagon.dev by Verdagon
labels:
- plt
- rust
summary: Comments
content: extracted
html: 2026-09-17-the-golden-spike-and-resurrecting-the-vale-n-programming.html
preview:
  file: 2026-09-17-the-golden-spike-and-resurrecting-the-vale-n-programming.preview-bdd5f6f11d54.webp
  width: 171
  height: 256
  color: '#691910'
images:
- source: https://upload.wikimedia.org/wikipedia/commons/e/ec/The-Golden-Spike-7Oct2012.jpg?utm_source=en.wikipedia.org&utm_campaign=imageinfo&utm_content=thumbnail_unscaled
  original:
    file: 2026-09-17-the-golden-spike-and-resurrecting-the-vale-n-programming.image-8caa85fcfa10.jpg
    width: 1024
    height: 1537
  color: '#69060b'
- source: https://i.imgflip.com/8twhkv.jpg
  original:
    file: 2026-09-17-the-golden-spike-and-resurrecting-the-vale-n-programming.image-3441c4150eb1.jpg
    width: 651
    height: 383
  color: '#47250d'
- source: https://verdagon.dev/images/golden-spike-first-render.png
  original:
    file: 2026-09-17-the-golden-spike-and-resurrecting-the-vale-n-programming.image-ad77299e3248.png
    width: 2624
    height: 2080
  color: '#004c7d'
---

Let's do something ridiculously ambitious and very inadvisable

Sep 17, 2026  —

[![](https://upload.wikimedia.org/wikipedia/commons/e/ec/The-Golden-Spike-7Oct2012.jpg?utm_source=en.wikipedia.org&utm_campaign=imageinfo&utm_content=thumbnail_unscaled)](https://en.wikipedia.org/wiki/Golden_spike)

There was an incredible moment on May 10th, 1869, when railroad builders finally reached their goal of **joining the east coast rail network with the west coast rail network.**

In that moment, the first United States transcontinental railroad was born. [0](https://verdagon.dev/blog/golden-spike-reviving-vale-valen#note0)

After six years of work, [1](https://verdagon.dev/blog/golden-spike-reviving-vale-valen#note1) the two rail networks finally joined up in Promontory Summit, Utah.

To commemmorate the moment, they drove a 17.6-karat **golden spike** into the final tie of the railroad.

To me, the phrase "[golden spike](https://en.wikipedia.org/wiki/Golden_spike)" means an incredibly difficult task joining two separate, distant systems. [2](https://verdagon.dev/blog/golden-spike-reviving-vale-valen#note2)

Here in the compiler world, it often feels like every compiler is *worlds* apart from every other compiler.

When a language wants to call into another language, one usually has to do acrobatic rituals involving the C ABI, writing "C bindings" (wrapper functions), and sometimes making deals with various deities.

And even with all that, cross-language generics [3](https://verdagon.dev/blog/golden-spike-reviving-vale-valen#note3) don't work (C doesn't have generics), and things definitely won't be memory-safe across the boundary (because C!).

**This is why it's so tricky to build a language on top of Rust.** And then, having memory safety and generics across the C boundary is impossible. And even if it were possible, integrating two compilers is *very, very difficult.*

And so I thought to myself, that *this sounds like a worthy goal for 2026!*

So here we are!

This post is going to be about the start of my *ridiculously over-ambitious* endeavor to create a language with **true Rust interop,** with a compiler that talks to rustc seamlessly enough that we can use my Rust graphics library [4](https://verdagon.dev/blog/golden-spike-reviving-vale-valen#note4) with cross-language generics, memory safety, linear types, Nick Smith's [group borrow checking](https://verdagon.dev/blog/group-borrowing), and a bunch of other juicy, juicy features.

Tentatively, I'm calling this new language "Valen" [5](https://verdagon.dev/blog/golden-spike-reviving-vale-valen#note5) [6](https://verdagon.dev/blog/golden-spike-reviving-vale-valen#note6) since it's similar (but different enough) to my existing language [Vale](https://vale.dev/).

So read on, and this post will explain the journey so far!

This entire endeavor is *extremely experimental* and many things have holes and sharp edges (see side-note [7](https://verdagon.dev/blog/golden-spike-reviving-vale-valen#note7)). It's going to be a glorious few months of cleaning, rewriting, and solidifying this horror before I unleash it on the world. Feel free to check out the [source code](https://github.com/valen-lang/valen), and beware, here be dragons! [8](https://verdagon.dev/blog/golden-spike-reviving-vale-valen#note8)

0

0% of this article was written by AI. [More thoughts here](https://verdagon.dev/blog/personal-ai-policy).

My stance: if you want someone to take the time to read something, take the time to write it by hand.

Thanks for reading =)

1

It was an *absolute clusterfuck,* to put it lightly. My favorite part ([source](https://cprr.org/Museum/Bowman_Last_Spike_CHS.html)):

...2 days before this date new troubles arose for the Union Pacific at Piedmont where the car in which Dr. T. C. Durant, the vice-president, was riding was disconnected from the train and shunted onto a side line by some 400 workers (one reporter says 500) who demanded their pay, unpaid since January 1.

Absolute madlads!

2

On Google Earth, I joined the local-editing app to cloud storage, finally enabling online editing. We called that project "golden spike" too. This whole rust interop endeavor brings me back to that moment!

3

Explained more below, but basically, being able to use SomeRustStruct\<OtherLangStruct>.

4

It's a [wgpu](https://wgpu.rs/)-based screen-space-refraction graphics library, which I call "Glass Domino".

Oh man, I *really* want to ramble more about how it works, and how it actually pulls off overlapping refractors in a performant way. But stay focused, Evan! This post is about compilers and languages!

5

And since its compiler is valenc, we can pronounce it like "Valence", which sounds nice!

6

Also, it's pretty funny to me that now V, Val, Vale, Vala, and Valen are all languages that have existed. I'm tempted to release a tiny Rust-interop language for others to use, and call it Va.

7

To be clear about what works today:

- Valen has linear types, but we can't yet declare that an existing Rust type is linear.
- Valen has group borrowing (except for closures), and it borrow checks across the boundary.
- Structs work across the boundary, even Valen structs that implement Rust traits. But they must be zero-sized (filled structs work in my separate prototype, not yet in Valen).
- Generational references are temporarily disabled, hopefully coming back soon.

8

Green dragons, specifically.

### A wish, a hope, and a dream language

Rust is one of my favorite languages [9](https://verdagon.dev/blog/golden-spike-reviving-vale-valen#note9) because of its speed, safety, and ecosystem... but there are definitely some things that would make it **simpler and overall nicer** for my use cases.

My wish list:

- A [borrow checker without shared-xor-mutable](https://verdagon.dev/blog/group-borrowing)
- Faster run-time performance [10](https://verdagon.dev/blog/golden-spike-reviving-vale-valen#note10)
- Resource safety via [linear types](https://www.youtube.com/watch?v=IpuvQUVB8Cg&t=2s)
- [Zig-style comptime](https://verdagon.dev/blog/impossible-optimization) [11](https://verdagon.dev/blog/golden-spike-reviving-vale-valen#note11)
- [Generational references](https://verdagon.dev/blog/generational-references)
- An Rc that can hold mutable data without RefCell or Cell
- ...and a lot of other features [12](https://verdagon.dev/blog/golden-spike-reviving-vale-valen#note12)

But I don't just want a new language, because I wouldn't have access to Rust's ecosystem of libraries.

So how do we do all that, while being able to call into Rust code?

9

You guessed it, it's a three-way tie between C++, Scala, and Rust!

10

I *think* this is possible, but I still have to implement it to benchmark it, so take it with a grain of salt:

To see what I mean, look at the "sneak peek" example on the [group borrowing post](https://verdagon.dev/blog/group-borrowing). I'm pretty sure that's faster than the corresponding Rust program, because the Rust program has to do two extra entities.get\_mut(id) lookups (plus some extra error-handling branching).

Also, group borrowing should be able to express noalias/alias.scope information to LLVM, so it doesn't lose those optimizations. For more on this, see [this doc](https://docs.google.com/document/d/1OxnwYe704m3TtUhNnR5lQzyOxEoZKUV-omMA5sGJHmQ/edit?usp=sharing).

11

Zero-cost compiler-optimized DSLs, anyone?

12

...because I ran out of space. But we're in a side-note now! The other things are: a better async story, better enums, better [mustprogress](https://llvm.org/docs/LangRef.html) optimizations, closures implementing traits, universal function call syntax, good compile times, and *maybe* if we're lucky we can bring back some subset of [perfect replayability](https://verdagon.dev/blog/perfect-replayability-prototyped).

### The Goal

This seemed impossible for a *lot* of reasons, explained in [Crossing the Impossible FFI Boundary, and My Gradual Descent Into Madness](https://verdagon.dev/blog/exploring-seamless-rust-interop-part-2).

I had forgotten that the post included this image:

![](https://i.imgflip.com/8twhkv.jpg)

Hopefully we don't have to do that! (*...foreshadowing intensifies...* [13](https://verdagon.dev/blog/golden-spike-reviving-vale-valen#note13))

Anyway, the **more concrete, specific goal** was for this program to work: [14](https://verdagon.dev/blog/golden-spike-reviving-vale-valen#note14)

```
import rust.nobiliav.NobiliaWindow;
import rust.nobiliav.FrameInput;
import rust.nobiliav.MainLoopCallback;
... // Import constants

exported func main() int {
  w = NobiliaWindow.new(1200, 900);
  ... // set up the terrain and entities

  w.main_loop(
    // Make a Valen closure that implements the Rust `trait MainLoopCallback`
    &MainLoopCallback((win, input) => {
      key = input.key();
      if (key == key_arrow_left()) {
        win.rotate_camera(-4, 0);
      } else if (key == key_arrow_right()) {
        win.rotate_camera(4, 0);
      } else if (key == key_arrow_up()) {
        win.rotate_camera(0, 4);
      } else if (key == key_arrow_down()) {
        win.rotate_camera(0, -4);
      }
    }));
  
  return 0;
}
```

Of course, this required answering a *lot* of very hard questions.

I'll ask them in the code:

```
// How do we know what things are importable from Rust?
import rust.nobiliav.NobiliaWindow;
import rust.nobiliav.FrameInput;
import rust.nobiliav.MainLoopCallback;
...
exported func main() int {
  // How do we know what static methods exist in a Rust type?
  // How does Valen know what parameters rustc expects here?
  w = NobiliaWindow.new(1200, 900);
  ...
  w.main_loop(
    // How do we make a Valen closure implement a Rust trait?
    // How do we know the methods on a Rust trait?
    &MainLoopCallback(
      // How will Rust call BACK into Valen code? Monomorphizer integration?
      // Can the optimizer inline a Valen function into a Rust function and vice versa?
      (win, input) => {
        key = input.key();
        if (key == key_arrow_left()) {
          win.rotate_camera(-4, 0);
        } else if (key == key_arrow_right()) {
          win.rotate_camera(4, 0);
        } else if (key == key_arrow_up()) {
          win.rotate_camera(0, 4);
        } else if (key == key_arrow_down()) {
          win.rotate_camera(0, -4);
        }
      }
    )
  );
  return 0;
}
```

However, these questions hide the real, central question underneath it all.

13

Back then, I thought I would need to reimplement Rust's generics and traits systems.

That would be unfortunate, especially because Valen *already has* a generics and traits system (evolved from the Vale compiler).

Then I realized I could do something clever: when some Valen code wants to call a Rust function, the Valen compiler *shouldn't* look at the Rust signature. Instead, it should first (lazily) generate the *corresponding function signature in Valen AST,* which Valen already understands.

That probably doesn't make much sense, but swing by the [Valen discord](https://discord.gg/SNB8yGH) and I'm happy to explain more.

14

Okay, I *have to* geek out about this part real quick. See the MainLoopCallback((win, input) => { ... }) part?

That's us *creating a new subclass* of the Rust trait MainLoopCallback, *inline.*

This is called an [open interface constructor](https://vale.dev/guide/interfaces#open-interface-constructors), and existed in the Vale compiler too.

It was one of its most complicated features, because it sat at the intersection of interfaces, generics, *and* closures. The perfect storm.

When I was migrating the Vale compiler to Rust for this endeavor (which was a whole ordeal worthy of its own blog post), I had to triage which features to bring back up, and I *definitely* didn't want to deal with open interface constructors yet. And then I did anyway!

### The central question

As many of you know, the most common way for other languages to call into Rust is if the Rust library exposes its functions as extern "C", and exposes its types as repr(C).

This is because C is the de-facto universal translator between low-level languages.

It's also because [Rust doesn't have a stable ABI yet.](https://www.reddit.com/r/rust/comments/ss2p6c/what_does_it_mean_when_people_say_that_rust_does/) [15](https://verdagon.dev/blog/golden-spike-reviving-vale-valen#note15)

"What's an ABI?" you might ask.

To simplify a bit, an "ABI" is basically how Rust answers these questions:

- "If the user calls a function, passing a struct by value, do we compile it to pass-by-reference, or do we pass it in a register?"
- "If the user specifies a bool, then a u64 integer, then a bool, do we put those two bools next to each other to save space?" [16](https://verdagon.dev/blog/golden-spike-reviving-vale-valen#note16)

Alas, Rust hasn't yet committed to an ABI.

So, using the C ABI (with extern "C" and repr(C)) is the only option for other languages to call into Rust.

**But C doesn't have generics.** There's no such thing as void do\_something\<T>(T\* thing) in C.

This means that other languages can't call into generic Rust functions.

**This is a problem for us,** because... well, recall this main\_loop call from above:

```
exported func main() int {
  ...
  w.main_loop(&MainLoopCallback((win, input) => { ... }));
  ...
}
```

Here's what Rust's main\_loop actually is:

```
impl NobiliaWindow {
  pub fn main_loop<C: MainLoopCallback>(&mut self, cb: &mut C) {
    ...
  }
  ...
}
```

That's right, that parameter right there is *generic*, taking anything that implements trait MainLoopCallback.

But the C ABI has no generics! But we need generics. Blast! We're stuck.

So **the central question,** the central mystery to solve, is *how do we do cross-language generics?*

15

"ABI" stands for "Application Binary Interface".

### Cross-language generics

There are actually two levels to this question:

1. How do we call into a Rust generic function?
2. How do we call into a Rust generic function *that calls back into Valen?* [17](https://verdagon.dev/blog/golden-spike-reviving-vale-valen#note17)

Here's a simpler example to illustrate the first level:

```
import rust.std.vec.Vec;

exported func main() int {
  my_vec = Vec<int>();
  my_vec.push(21);
  my_vec.push(42);
  // Returns 42
  return my_vec.pop().unwrap();
}
```

When we say my\_vec.push(21), we're calling the generic function Vec\<T>::push.

Luckily, in the [Madness](https://verdagon.dev/blog/exploring-seamless-rust-interop-part-2) post, I managed to get it working for C:

```
// Import a rust type directly (no bindings!)
#pragma rsuse VecInt = std::vec::Vec<u64>
// Must specify each method you want to use
#pragma rsfn VecInt_with_capacity = VecInt::with_capacity
#pragma rsfn VecInt_capacity = VecInt::capacity
#pragma rsfn VecInt_drop = VecInt::drop
// Plus the magic incantation, and...
#include "rust_deps/rust_deps.h"

#include <stdio.h>

int main() {
  // ...presto, we can use rust libraries!
  VecInt argv = VecInt_with_capacity(42);
  printf("Capacity: %lu\n", VecInt_capacity(&argv));
  VecInt_drop(argv);
  return 0;
}
```

```
Capacity: 42
```

I called this my "most cursed exploration" because of the acrobatics it had to do:

- It ran rustdoc (yes, the documentation generator!) and parsed its json output with the rustdoc\_types library, to figure out what types were usable, and what impls had what methods.
- It ran rustc with a "scouting program" that literally just prints sizes; println!("{}", size\_of::\<Vec::\<u64>>()) prints 24 bytes. (Thanks to matklad and literallyvoid for helping me improve this part!)
- It then ran rustc again with a "instantiation program" which generated a "wrapper C library" that the C program could statically link to.
- Vale's memory safety approach didn't really line up with Rust's, so a user had to stick to certain patterns.

And it had some limitations:

- We need to fully spell out the generic args, like VecInt = std::vec::Vec\<u64>.
- We need to import *every* method we use, in a #pragma rsfn.

But the hardest limitation is that we can't make a C type implement a Rust trait.

That means you can't use HashMap::get, because its k key must implement the Hash and Eq traits.

Still, as cursed as it was, the 2024 solution was actually a pretty good start.

Above, I said that there are two levels to the cross-language generics question:

1. How do we call into a Rust generic function?
2. How do we call into a Rust generic function that calls back into Valen?

The 2024 solution solved most of #1 in spirit, even though it got information from Rust in an odd way.

And it hinted that there might be an answer to #2, if a language was designed with it in mind and Did Things Properly™.

17

How do we stare into the void, and have the [void stare back?](https://verdagon.dev/images/voidcat.jpg)

### Interop, Done Properly

Of course, there's no such thing as "doing things properly", because **everything I'm about to say is *very* unsupported by the Rust compiler.**

But I felt like the next step in the endeavor would be to talk to rustc directly, [18](https://verdagon.dev/blog/golden-spike-reviving-vale-valen#note18) instead of invoking it (twice!) from the outside. [19](https://verdagon.dev/blog/golden-spike-reviving-vale-valen#note19)

And it turns out, one can actually **run the rust compiler as a library,** using [rustc\_driver](https://rustc-dev-guide.rust-lang.org/rustc-driver/intro.html)'s run\_compiler function!

```
use rustc_driver::{Callbacks, Compilation};
...
fn main() {
  ...
  let callbacks = ValenRustInteropCallbacks { ... };
  ...
  rustc_driver::run_compiler(&rustc_args, &mut callbacks);
```

The two arguments to run\_compiler influence what rustc does.

- rustc\_args tells rustc **what Rust libraries we want to use**, so it loads and compiles them, so the Valen typing pass can ask questions about them. [20](https://verdagon.dev/blog/golden-spike-reviving-vale-valen#note20)
- callbacks specifies observers that tell us when rustc has loaded and compiled all of its dependencies. [21](https://verdagon.dev/blog/golden-spike-reviving-vale-valen#note21) Then, the Valen typing pass can ask rustc questions about the Rust crates it depends on, and generate Rust MIR functions from our Valen code.

However, there was a problem with that: Valen couldn't lower to Rust's MIR, because it didnt't have the capabilities Valen needed:

- MIR's mutable references are unique, which means it can't do our [Group Borrowing](https://verdagon.dev/blog/group-borrowing) memory safety approach, which allows shared mutable references.
- MIR doesn't let us specify LLVM's alias groups / !alias.scope on load/store instructions, which is something that makes group borrowing faster.
- MIR can't express comptime metaprogramming, which is on my dream language wish list.

Rust's MIR was designed for Rust. So it's a great target for Rust code, not Valen code... alas.

If anyone wants to know how to do this the MIR way, let me know! Happy to point you in the right direction.

Of course, if Valen used its own IR and backend instead of Rust's, that would mean that the compilers would need to **work with each other** in a very interesting dance.

18

Of course, doing this requires using the private, unstable APIs. Yikes!

19

Well... the current one *also* invokes it twice. But... in a better way. In my opinion. Not so much cursed. At most "hexed" perhaps.

20

To do this, we actually:

- Parse the Valen source files, looking for any imports (like import rust.glass\_domino.Window).
- Generate a Rust lib.rs file containing equivalents (like use glass\_domino::Window;).
- Add that "lib.rs" path into rustc\_args.
- Add dependency libraries' paths to rustc\_args (like --extern glass\_domino=target/.../libglass\_domino.rlib) so rustc knows where to find it (well, cargo does this).

(This is how it still works today)

21

This is rustc's after\_expansion callback, run after rustc has processed various things, including the use statements that bring dependencies' information into the current crate's compile.

### The dance

"The dance" is how rustc's monomorphizer and valenc's monomorphizer **need to work together** to fully discover all of the functions that they are calling in each other.

A "monomorphizer" is what turns a generic function foo\<T> into its substituted functions foo\<i32>, foo\<bool>, foo\<String> etc. depending on what types one calls foo\<T> with. [22](https://verdagon.dev/blog/golden-spike-reviving-vale-valen#note22)

To illustrate, here's an example program showing **Valen calling Rust calling Valen:**

```
exported func main() {
  vs = MyStruct();
  rust_func<MyStruct>(&vs);
}

struct MyStruct { }
impl RustTrait for MyStruct {
  func method<N int>(self: &MyStruct) {
    print("hello from valen! " + N);
  }
}
```

```
trait RustTrait {
  fn method<const N: i32>(&self);
}

fn rust_func<T: RustTrait>(x: &T) {
  x.method::<4>();
  x.method::<6>();
}
```

For a second, **let's pretend that** main, rust\_func, and method were **all Rust functions.**

If that were the case, rustc would do this:

(For readability, I'll shorten "monomorphizer" and "monomorphize" to "mono")

- rustc starts mono'ing main.
  - rustc sees rust\_func\<MyStruct>, wants it to be mono'd.
    - rustc starts mono'ing rust\_func\<MyStruct>.
      - rustc sees x.method::<4>, wants it to be mono'd.
        - rustc monos MyStruct::method::<4>.
      - rustc sees x.method::<6>, wants it to be mono'd.
        - rustc monos MyStruct::method::<6>.

And that would have been easy; a simple task for rustc.

**However,** main and method **are Valen functions.**

So it's more like this, where rustc and valenc need to **talk to each other:**

(Only difference below: four "rustc"s become "**valenc**"s)

- **valenc** starts mono'ing main.
  - **valenc** sees rust\_func\<MyStruct>, wants it to be mono'd.
    - rustc starts mono'ing rust\_func\<MyStruct>.
      - rustc sees x.method::<4>, wants it to be mono'd.
        - **valenc** monos MyStruct.method<4>.
      - rustc sees x.method::<6>, wants it to be mono'd.
        - **valenc** monos MyStruct.method<6>.

In other words, we need to make rustc's monomorphizer and valenc's monomorphizer able to call each other.

So our design needs to enable that. [23](https://verdagon.dev/blog/golden-spike-reviving-vale-valen#note23)

22

It's sometimes also called "instantiator" (or very rarely, "elaborator").

23

Even this explanation is a simplification though. valenc doesn't actually call *into* rustc's monomorphizer. Instead, it monomorphizes the Valen function then returns to rustc a list of the Rust functions it would like to be mono'd. This let me do this all with one less patch in the Rust fork.

### The current design

The current design works like this:

- (Like before) rustc\_args tells rustc **what Rust libraries we want to use**, so it loads and compiles them, so the Valen typing pass can ask questions about them.
- callbacks specifies observers that tell us:

- When rustc has loaded and compiled all of its dependencies, so the Valen typing pass can ask rustc questions about the Rust crates it depends on, and generate **empty** Rust functions corresponding to the Valen exported functions (such as main).
- When rustc is monomorphizing one of those empty Rust MIR functions, we can intercept that call, make Valen monomorphize it instead, and also tell rustc what *other* Rust functions this Valen function calls (so rustc can monomorphize those too).
- When rustc is about to ask the rustc LLVM backend to lower this (empty) function to LLVM, we intercept that, have Valen's LLVM backend lower it to LLVM instead.

Of course, those last two capabilities definitely don't exist in rustc.

So, I did the nuclear option. I **patched the Rust compiler** to add those last two callbacks.

Honestly, this was the riskiest part of the whole endeavor. The patches are small (~100 lines), but one of them is basically a hack that assumes rustc is using LLVM, which of course isn't always true. [24](https://verdagon.dev/blog/golden-spike-reviving-vale-valen#note24) It works, but it's definitely not upstreamable.

Luckily, I already have a better approach in mind for the next iteration...

**If you want to help me with this, please [email me](https://verdagon.dev/blog/revval@verdagon.dev)!** And it would benefit more than just Valen; when I last chatted with the [Carbon](https://github.com/carbon-language/carbon-lang) team, they said they were interested in reusing Rust libraries too. We might be trailblazing for other languages!

Of course, that's just the rustc side of this. I then had to rearchitect the Valen compiler to actually use these capabilities. That took most of 2026, and was a massive endeavor that will definitely get its own post (stay tuned!).

### The Golden Spike, but even more Golden

I have a little game engine I've been experimenting with, to try and figure out new ways to do [screen-space refraction](https://lettier.github.io/3d-game-shaders-for-beginners/screen-space-refraction.html), like this: [25](https://verdagon.dev/blog/golden-spike-reviving-vale-valen#note25)

I thought, this would be a perfect goal to set. This would be Valen's first contact with Rust, like the golden spike that completed the first transcontinental railroad across the US.

If I could get it working, then I could finally start developing the game that I've been [trying to start on since the dawn of time](https://verdagon.dev/blog/yak-shave-language-engine-game).

My first goal was actually something like this, where main owns the while loop:

```
import rust.nobiliav.NobiliaWindow;
import rust.nobiliav.FrameInput;
import rust.nobiliav.MainLoopCallback;
...
exported func main() int {
  w = NobiliaWindow.new(1200, 900);
  ...
  while w.running() {
    input = w.tick();
    key = input.key();
    if (key == key_arrow_left()) {
      w.rotate_camera(-4, 0);
    } else if (key == key_arrow_right()) {
      w.rotate_camera(4, 0);
    } else if (key == key_arrow_up()) {
      w.rotate_camera(0, 4);
    } else if (key == key_arrow_down()) {
      w.rotate_camera(0, -4);
    }
  }
  return 0;
}
```

but *apparently* wgpu (and apps in general nowadays?!) don't let us own our own event loop. I remember the good ol' days when you could just GetMessage(&msg, ...) or EvtGetEvent(&event, ...) in a while(true) and everyone was happy.

I was faced with a choice: do something less awesome, or expand the scope of my golden spike to include Rust calling *back into Valen.*

You all know me. You know what I chose!

I decided to implement something like a closure:

```
import rust.nobiliav.NobiliaWindow;
import rust.nobiliav.FrameInput;
import rust.nobiliav.MainLoopCallback;
...
exported func main() int {
  w = NobiliaWindow.new(1200, 900);
  ...
  w.main_loop(&MainLoopCallback((win, input) => {
    key = input.key();
    if (key == key_arrow_left()) {
      win.rotate_camera(-4, 0);
    } else if (key == key_arrow_right()) {
      win.rotate_camera(4, 0);
    } else if (key == key_arrow_up()) {
      win.rotate_camera(0, 4);
    } else if (key == key_arrow_down()) {
      win.rotate_camera(0, -4);
    }
  }));
  return 0;
}
```

After a *lot* of work (that led to discovering *the dance* and the current design), it finally all connected!

At last, I got my first render: [26](https://verdagon.dev/blog/golden-spike-reviving-vale-valen#note26)

![](https://verdagon.dev/images/golden-spike-first-render.png)

And with that first render, **the golden spike was hammered into place, and the transcontinental compiler was born.**

Over *half a year* of work, theorizing, planning, hacking, refactoring, and rearchitecting had **finally come to fruition.**

Of course, we aren't done yet!

We don't just want Rust interop, we want *memory-safe* Rust interop!

24

rustc also has GCC and Cranelift backends.

25

Rendered by a working Valen program calling into a Rust graphics library!

26

Don't look too closely at it. You'll start noticing oddities (like semitransparent refractive grass!)

### Memory safety across the boundary

Like I said above, I'm designing a memory safety approach that builds on [Group Borrowing](https://verdagon.dev/blog/group-borrowing), which is a more flexible form of borrow checking that allows for shared mutable references.

If my guess is right, a well-designed blend could have even more benefits than we talked about in that article:

- We could have a mutable reference into an otherwise-immutable object
- Rc could hold mutable objects without RefCell or Cell (something I call "the holy grail", long story!)
- In some cases, it could even be faster at run-time [27](https://verdagon.dev/blog/golden-spike-reviving-vale-valen#note27)

These three points are theoretical until I can prove them, so take them with a grain of salt! Stay tuned for more posts on them.

With careful enough language design, group borrowing can actually be a superset of Rust's borrow checking, making it so we can borrow-check over the boundary.

This post is long so I'll save the details for the next one, but TL;DR: it's not complete yet, but this prototype is doing some borrow checking over the boundary!

27

To see what I mean, look at the "sneak peek" example on the [group borrowing post](https://verdagon.dev/blog/group-borrowing). I'm pretty sure that's faster than the corresponding Rust program, because the Rust program has to do two extra entities.get\_mut(id) lookups (plus some extra error-handling branching).

Disclaimer: this is unproven still, since I haven't yet implemented and benchmarked it, so take it with a grain of salt.

Also, group borrowing should be able to express noalias/alias.scope information to LLVM, so it doesn't lose those optimizations. For more on this, see [this doc](https://docs.google.com/document/d/1OxnwYe704m3TtUhNnR5lQzyOxEoZKUV-omMA5sGJHmQ/edit?usp=sharing).

### Resurrecting Vale(n)

Vale is my greatest achievement, my own personal crown jewel of memory safety design. It was a blend of generational references and region borrowing, both of which had never been seen before. It directly inspired Mojo to add linear types, even before I worked for them.

Vale was also pretty unique in that it was a "high-level high-performance" language (as opposed to a lower-level "systems programming" language). This let it be much more strict about memory safety, which allowed for a lot of interesting features like [Perfect Replayability](https://verdagon.dev/blog/perfect-replayability-prototyped).

This new language is a different beast altogether.

- It uses group borrowing (purely compile-time memory safety) instead of generational references + region borrowing.
- It's a systems programming language, which means even more freedom and speed (via group borrowing), though it wouldn't be as safe (because it'll have unsafe like Rust).

It's so different, that it truly deserves its own name. [28](https://verdagon.dev/blog/golden-spike-reviving-vale-valen#note28)

And thus, **Valen** is born!

28

Besides, switching Vale's memory safety model *for the third time* would really confuse people. First it was [constraint references](https://verdagon.dev/blog/raii-next-steps), then it was normal generational references, then it was probabilistic generational references + region borrowing. Asking people to keep track of what it is at any given moment is becoming a tall order!

### That's all for now!

These are ideas I've been itching to try for *years*, and they're finally all coming together into something beautiful. [29](https://verdagon.dev/blog/golden-spike-reviving-vale-valen#note29)

This post represents my work for most of a year, and I had to keep it from exploding into a 40-page tome like some of my other posts, so I'll wrap things up here.

This is just the tip of the iceberg, so stay tuned by subscribing to my [RSS feed](https://verdagon.dev/rss.xml), [r/valen](https://www.reddit.com/r/Valen/), or joining the [Valen discord](https://discord.gg/SNB8yGH).

Cheers!

- Evan Ovadia

PS. If anyone has a Fosstodon account, please [send me an invite](https://fosstodon.org/invites) (my email is revval át verdagon dot dev), thank you!

29

And more than that: benefits are emerging from the design that even I didn't foresee. Group borrowing and linear types combine to form something *very interesting...*
