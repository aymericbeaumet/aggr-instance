---
title: A new way to think about borrow checking (in reverse)
link: https://verdagon.dev/blog/reverse-borrow-checking
source: hnrss-org
published: 2026-10-18T12:11:13Z
updated: 2026-10-18T12:11:13Z
first_seen: 2026-10-19T12:57:26.464953148Z
authors:
- verdagon
content: extracted
html: 2026-10-18-a-new-way-to-think-about-borrow-checking-in-reverse.html
---

Travel backwards in time, so that you already know the future

Oct 18, 2026  —

Have you ever had a weird idea, and then it worked, but you can't really believe it?

That's what I'm feeling right now, about Valen's borrow checker.

Because it **travels backward through time.**

Or, to be less dramatic, it **walks functions backwards,** which is still *very* weird.

Years ago, if you told me it would be good to iterate a function in reverse, I would have called you crazy.

It turns out, there's actually some really good reasons to do this, and it's fairly common when you're doing e.g. optimization passes. [^1] But I don't think anyone's done this for checking memory safety.

If you want a really deep look into the world of borrow checking, read on!

*We're about to see some weird things.* [^2]

This is a pretty deep under-the-hood look into borrow checking algorithms. If you want something epic and entertaining instead, check out my recent post on [The Golden Spike, and Resurrecting the Vale(n) Programming Language](https://verdagon.dev/blog/golden-spike-reviving-vale-valen)!

## A weird kind of borrow checker

This isn't going to be your normal shared-xor-mutable borrow checking.

This is [Valen](https://github.com/valen-lang/valen)'s borrow checker, the one that [makes shared-xor-mutable optional](https://verdagon.dev/blog/valen-group-borrowing), and can [call into Rust functions](https://verdagon.dev/blog/golden-spike-reviving-vale-valen) with [memory safety across the boundary](https://verdagon.dev/blog/boundary-memory-safety).

Valen is still very experimental. Many things have holes and sharp edges, and likely will until v0.1 comes sometime in February. The glorious months-long refactoring spree is already underway (hence this new borrow checker!). Feel free to check out the [Valen repo](https://github.com/valen-lang/valen) and beware, here be dragons! [2](https://verdagon.dev/blog/reverse-borrow-checking#note2)

It's kind of like Rust's borrow checker, but it also has memory-safe mutable aliasing. If you want to know more about that, check out [this post](https://verdagon.dev/blog/valen-group-borrowing), though you won't need to understand that for most of this post.

2

Green dragons, specifically.

## ...and a weird, backwards implementation!

Valen compiles a function in four phases: [3](https://verdagon.dev/blog/reverse-borrow-checking#note3)

1. It **parses** the function, turning source text into an AST.
2. It does **static typing**, figuring out the type for every variable, argument, and expression.
3. It does **static grouping**, where every reference knows **what variable** (or what *part* of what variable) it's pointing at. [4](https://verdagon.dev/blog/reverse-borrow-checking#note4)
4. It **safety-checks** the function, to make sure that a reference isn't used after it's invalidated.

I'll explain all of this further below.

**The weirdest part is that #4 happens in reverse.** It looks at the last statement in the function, and then walks backwards until it reaches the top of the function.

This is weird because, as far as I know, it's the **only borrow checker whose safety check goes backwards.** [5](https://verdagon.dev/blog/reverse-borrow-checking#note5)

Further below, I'll explain why we're going backwards (hint: convenience, loops, and speed).

But first, let's see **how it works,** and what it even *means* to go backwards.

Let's skip phase 1, and start with phase 2.

3

There's also a secret fifth phase, where we calculate what attributes to send to LLVM to describe what operations might change what groups. But that's out of scope for this article.

4

It also figures out all the groups invalidated by any mut effects from any instructions.

5

For example:

- Rust (NLL and the current Polonius) does a constraint-solve for region inference, then a **forward** pass to check for conflicts. (Note that rustc does do a backwards scan to figure out where each variable is used, but its actual memory safety checks happen in a forward pass.)
- Vale's typing pass determines regions, then a later **forward** pass does [region borrow checking](https://verdagon.dev/blog/zero-cost-borrowing-regions-part-1-immutable-borrowing) to determine when it can safely skip run-time [generation checks](https://verdagon.dev/blog/generational-references). (Vale's region borrow checker was actually fused with its instantiator/monomorphizer.)
- Valen's previous borrow checker did it like Vale too; its typing pass determines groups, then a later **forward** pass would check at every use that it was declared more recently than it was "invalidated".
- There are a lot of other borrow checkers out there, but I haven't heard of any doing their safety checks in reverse. If anyone knows of any, let me know!

### Phase 2: Static Typing

The static typing pass will be familiar to some of you, it assigns types to variables.

For example, it turns this program:

```valen
exported func main() {
  let fuel = 5;
  let force = fuel;
}
```

into this program:

```valen
exported func main() {
  let fuel i64 = 5;
  let force i64 = fuel;
}
```

This is pretty normal, lots of compilers do it.

Compilers usually do this as a **forward** pass, walking from the top of the function to the bottom, figuring out types as it goes.

We're going to do something *similar but weirder* in phase 3; phase 3 is going to do **static grouping.**

### Phase 3: Static Grouping [6](https://verdagon.dev/blog/reverse-borrow-checking#note6)

This is another forward pass, which figures out where each reference points to.

For example, in this program:

```valen
func print_first_fuel_cell(s &Ship) {
  let first_ref = &s.fuel_cells[0];
  print_int(first_ref);
}
```

Static grouping figures out that first\_ref is pointing in s.fuel\_cells\[\]: [7](https://verdagon.dev/blog/reverse-borrow-checking#note7)

```valen
func print_first_fuel_cell(s &Ship) {
  // Static typing says:   first_ref &int
  // Static grouping says: first_ref &int in s.fuel_cells[]
  let first_ref = &s.fuel_cells[0];
  print_int(first_ref);
}
```

In other words, it's pointing to an int somewhere in the s.fuel\_cells\[\] array.

This information is now *part of the variable's type.*

We'll be using this information in the next phase, which actually checks for use-after-free.

6

I'll probably start moving away from the term "group" soon, switching to "path" instead. Opinions welcome!

7

In V1's static grouping phase, it would also figure out *when* a reference was made. So for example, instead of &int in s.fuel\_cells\[\], it would know that it was &int in s.fuel\_cells\[\] from line 7.2. This was pretty awesome, we were literally **smuggling time and space** information into a variable's type. This new V2 version doesn't need to do that because it does a backwards scan, as we'll see next.

### Phase 4: Safety Checking

This post is half for fun, but also to **ask for feedback on the algorithm.** I don't think anyone's done it this way before, and there might be a good reason for that. If you see any problems with this approach, please help me out by [leaving a comment](https://www.reddit.com/r/Valen/comments/1x92s1o/a_new_way_to_think_about_borrow_checking_in/), so I can fix (or scrap!) the approach. [8](https://verdagon.dev/blog/reverse-borrow-checking#note8)

Now we can use everything Phase 3 calculated!

This is the phase that **checks for use-after-free.**

It does it with a **backwards scan,** *as was foretold.*

Let's see it happen in a slightly bigger example:

```valen
struct Entity { hp int; buffs []int; }
func add_buff(self &Entity mut) { ... }

func foo(e &Entity mut) {
  let buff = &e.buffs[0];
  e.add_buff();
  print_int(buff);
}
```

This program is doing a **use after free,** and we want the compiler to complain.

First, I'll desugar it into a more explicit syntax:

```valen
struct Entity { hp int; buffs []int; }
func add_buff<g'>(self &Entity in g) mut(g) { ... }

func foo<g'>(e &Entity in g) mut(g) {
  let buff = &e.buffs[0];
  e.add_buff();
  print_int(buff);
}
```

Now I'm going to add in some comments, all determined by the backwards scan as it goes.

Further below, I'll explain what "DECLARE", "USE", and "MUTATE" mean.

```valen
struct Entity { hp int; buffs []int; }
func add_buff<g'>(self &Entity in g) mut(g) { ... }

func foo<g'>(
  e &Entity in g          // DECLARE e (group g)
) mut(g) {
  let buff = &e.buffs[0]; // DECLARE buff (group g.buffs[])
  e.add_buff();           // USE e (group g)
                          // MUTATE group g
  print_int(buff);        // USE buff (group g.buffs[])
}
```

The algorithm walks **in reverse,** from the end of the function to the start.

As it scans upward, it looks for these events:

- When we USE a variable, **add** it to the pending list.
- When we MUTATE anything, ensure no variables in pending are invalidated.
- When we DECLARE a variable, **remove** it from the pending list.

pending contains all of the things USEd below, that we hope to see DECLAREd above.

We hope to never see a MUTATE to anything pending.

I think the best way to think of this is that pending contains all variables that **will be used** at some point below/later in the function, and therefore should be valid until then.

To really understand it, let's see it in action.

First, we **look at the last line:**

```valen
  print_int(buff);        // USE buff (group g.buffs[])
```

This is a USE, so we add it to the pending list. pending now has:

- buff (group g.buffs\[\])

Now, still going backwards, we encounter the line above it:

```valen
  e.add_buff();           // USE e (group g)
                          // MUTATE group g
```

We see the MUTATE so we **ensure no variables in pending are invalidated.**

Specifically, we'll ensure that nothing in pending is pointing at anything under e.

- buff (group g.buffs\[\]) ... **This matches!** Show a compile error.

Here's what just happened: buff was pointing at g.buffs\[\], which starts with the g we just mutated with add\_buff. That means that buff is **invalidated above the line where it's used,** which is a use-after-free.

So the compiler shows an error here. [Huzzah!](https://www.youtube.com/watch?v=y13n1CeyqjY&t=132s)

8

First person to break it gets a prize!

### Another Example

Let's see a **correct program,** so we can see the backwards walk encountering a declaration.

Here's the same program, but with e.add\_buff(); moved up, to be the first line in the function.

```valen
struct Entity { hp int; buffs []int; }
func add_buff<g'>(self &Entity in g) mut(g) { ... }

func foo<g'>(
  e &Entity in g          // DECLARE e (group g)
) mut(g) {
  e.add_buff();           // USE e (group g)
                          // MUTATE group g
  let buff = &e.buffs[0]; // DECLARE buff (group g.buffs[])
  print_int(buff);        // USE buff (group g.buffs[])
}
```

Like before, we start at the end, and get to this line:

```valen
  print_int(buff);        // USE buff (group g.buffs[])
```

...and see that it's a USE so we add buff to the pending list:

- buff (group g.buffs\[\])

Now we go one line upward, to this line:

```valen
  let buff = &e.buffs[0]; // DECLARE buff (group g.buffs[])
```

This DECLAREs buff, so we **remove** it from pending.

pending is now empty.

(That means that **there are no variables that are used below/later in the function.**)

Now we go one line upward, to this line:

```valen
  e.add_buff();           // USE e (group g)
                          // MUTATE group g
```

We see the MUTATE, so we **ensure no variables in pending are invalidated.**

There are no variables in pending, so nothing interesting happens.

Then we see it USE e, so we **add** it to pending:

- e (group g)

And that's the end of the function!

We made it all the way to the end without seeing any MUTATEing any pending variables, which means **the program is memory-safe.**

## Why backwards?

There are three reasons: convenience, loops, and speed.

Generally, going backwards is convenient for figuring out **when a thing is last used.**

For example, look at this function:

```valen
func attack(a &Entity in g, d &Entity in g) mut(g) {
  let a_power = a.calculate_attack_power();
  let a_energy_cost = a.calculate_attack_cost(d);
  let d_armor = d.calculate_defense();
  let d_energy_cost = d.calculate_defend_cost(a);
  a.use_energy(a_energy_cost);
  d.use_energy(d_energy_cost);
  d.damage(a_power - d_armor);
}
```

If we want to do *bookkeeping X* when a is declared and do *bookkeeping Y* when a is no longer needed, [9](https://verdagon.dev/blog/reverse-borrow-checking#note9) we have two options:

- Do a **forward** scan. We encounter a's declaration, and do X. Then we continue, recording uses of a as we go. Once we hit the end of the function, we **fetch/navigate to** the last recorded location, and do Y.
- Do a **backward** scan. We encounter the last use of a, and do Y. Then we continue, looking for a's declaration. When we find it, we do X.

Notice how the forward scan involved that extra fetch/navigate, because we couldn't recognize the last use when we encountered it.

The backward scan was simpler.

If you want to see another backwards scan help with destructor insertion, check out my [LLVM talk](https://www.youtube.com/watch?v=IpuvQUVB8Cg) from 2024! As many compiler people will tell you, scanning in reverse is useful for a lot of things.

Going backwards is also better for loops.

Here are a couple examples that I've been using to think about loops: [10](https://verdagon.dev/blog/reverse-borrow-checking#note10)

This is a correct program, but it shows how tricky it is to know if that observe(e) is valid.

```valen
exported func main() {
  let arr = ArrayList<int>(3);
  let e = &arr[0];
  
  while ... {
    observe(e); // Valid `e` use.
    add_element(&arr);
    set e = &arr[0];
  }
}
```

That e actually can come from two different places:

- The e = &arr\[0\]; before the loop.
- The set e = &arr\[0\]; at the end of the loop.

A forward scan encountering observe(e) would need to **look ahead** to the set e to conclude it's valid.

This is a similar example without the set e.

```valen
exported func main() {
  let arr: ArrayList<int> = ...;
  let e = &arr[0];

  while ... {
    observe(e); // Invalid `e` use.
    add_element(&arr);

  }
}
```

Without the set e, the e in the loop is invalidated by a *previous iteration* of the loop.

If we did a forward scan, we'd need to **look ahead** to see that there's no set e reviving e.

This is a tricky problem. One can:

- Scan ahead looking for those sets (then jump back).
- Do repeated forward passes, gaining more information each time, until things settle (a "dataflow fixpoint").
- Do a forward pass to collect the information, and defer checking until later.

The last one was tempting. Alas, its data structures ended up looking very complex, and it had the same "fetch/navigate" problem that we saw in the func attack example above.

It was simpler to just go straight to V2's reverse-walking approach.

I'm also fairly certain V2's approach is faster, because it only has to process each node once in the phase 3 forward pass, and once in the phase 4 backward pass. [11](https://verdagon.dev/blog/reverse-borrow-checking#note11)

I haven't benchmarked that, so take it with a huge grain of salt. [12](https://verdagon.dev/blog/reverse-borrow-checking#note12)

But of course, the reverse-walk approach doesn't get loops for free. We still need some adjustments.

9

This bookkeeping might be decrementing a reference count, or [adding a destructor call](https://www.youtube.com/watch?v=IpuvQUVB8Cg), or checking for use-after-frees, etc.

10

Note, in these examples, I use the name ArrayList, but in the real Valen compiler those are called Array, for weird historical reasons. That'll change soon.

11

Note, this doesn't mean that the algorithm is O(n) yet. The MUTATE still needs to check everything in the pending list. However, I think there's a way to speed that check up a lot. Fingers crossed!

12

V1 never had full loop support, so there's really nothing to benchmark against.

## Reverse-Walking with Loops [13](https://verdagon.dev/blog/reverse-borrow-checking#note13)

The algorithm needs a little bit more to handle loops.

Specifically, we'll do MUTATE and DECLARE differently, and we also react to LOOP START.

We scan upward, looking for these events:

- When we USE a variable, **add** it to the pending list.
- When we MUTATE anything:

- Ensure no variables in pending are invalidated.
- Add the mutation to the containing loop's mutations\_after\_this list.

- When we DECLARE a variable:

- **remove** it from the pending list.
- If it wasn't invalidated by anything in mutations\_after\_this, add it to the containing loop's revived\_after\_this list.

- When we reach a LOOP START:

- For each thing in pending, if it's invalidated by something in the loop's mutations\_after\_this, ensure it's also in the loop's revived\_after\_this. [14](https://verdagon.dev/blog/reverse-borrow-checking#note14)

Let's see what this does to this example. It's a **correct** program, so there should be no compile error.

```valen
exported func main() {
  let arr: ArrayList<int> = ...; // DECLARE arr (group arr)
  let e = &arr[0];               // DECLARE e (group arr[])
  
  while ... {                    // LOOP START
    observe(e);                  // USE e (group arr[])
    add_element(&arr);           // MUTATE arr (group arr)
    set e = &arr[0];             // DECLARE e (group arr[])
  }
}
```

We're walking backwards, so we start at the last line:

```valen
    set e = &arr[0];             // DECLARE e (group arr[])
```

It's a DECLARE [15](https://verdagon.dev/blog/reverse-borrow-checking#note15) so we do two things:

- **remove** it from the pending list. It's not in it, so nothing happens.
- If it wasn't invalidated by anything in the containing mutations\_after\_this (it wasn't, because it's empty) add it to revived\_after\_this.

revived\_after\_this now contains one entry: e.

Now we walk up one line, seeing this:

```valen
    add_element(&arr);           // MUTATE arr (group arr)
```

It's a MUTATE, so we do two things:

- Ensure no variables in pending are invalidated. pending is empty, so nothing happens.
- Add the mutation to the containing loop's mutations\_after\_this list.

mutations\_after\_this now contains one entry: arr.

Now we walk up one line, seeing this:

```valen
    observe(e);                  // USE e (group arr[])
```

It's a USE, so we **add** it to the pending list.

pending now contains one entry: e (group arr\[\]).

Now we walk up one line, seeing the loop start:

```valen
  while ... {                    // LOOP START
```

So we do the LOOP START check: For each thing in pending, if it's invalidated by something in the loop's mutations\_after\_this, ensure it's in the loop's revived\_after\_this.

In this case, pending has e (group arr\[\]). e *is* invalidated by something in mutations\_after\_this. So check it's in the loop's revived\_after\_this (it is). Check passed, we're good.

I'll skip the explanation for the top two DECLAREs, since we covered that before.

And then we reach the start of the function! We encountered no errors, so the borrow checker can conclude **this function is memory-safe.**

And that's it! That's how we handle loops with this new reverse borrow checker.

13

Easter egg note!

Sergeant Stubby was the star of the 102nd Infantry Regiment in World War I and travelled with his division to France to fight alongside the French. He served for 18 months and participated in 17 battles and four offensives on the Western Front.

He received many awards, including a gold medal, a wound stripe, and two purple hearts.

Sergeant Stubby has been called the most decorated war dog of the Great War and the only dog to be nominated and promoted to sergeant through combat.

([Source: Wikipedia](https://en.wikipedia.org/wiki/Sergeant_Stubby))

If you see this note, sneak the word "gold" into a [comment](https://www.reddit.com/r/programming/comments/1x92rhh/a_new_way_to_think_about_borrow_checking_in/)!

(Congrats to [timpi4ever](https://www.reddit.com/r/Valen/comments/1x92s1o/comment/phi1ieo/) for finding this first!)

14

We don't remove anything from pending here, because those pending uses still need to be satisfied by the code above the loop.

15

Yes, I regret calling it DECLARE now, since really this is just re-pointing an existing variable.

## More towards Rust's borrow checker?

There's one more big thing I want to change, eventually.

Rust has this very nice ability to use facts from *anywhere* in the function to figure out the lifetime of reference.

For example, [this Rust program](https://play.rust-lang.org/?version=stable&mode=debug&edition=2024&gist=70b5f84ea984e1a3a60dff9e0110bcf9) looks at the later r = &b\[0\]; to figure out that r points at a *or* b.

```rs
fn main() {
  let a = vec![1, 2, 3];
  let b = vec![4, 5, 6];

  let mut r = &a[0];
  if some_condition() {
    r = &b[0];
  }
  println!("{}", r);
}
```

Valen doesn't do this yet; Valen's phase 3 (static grouping) is still doing normal one-directional inference.

But if we change phase 3 into a full solve like rustc does, we can get some similar superpowers.

The neat part: this wouldn't need to change phase 4's backward pass. In the end, we'd have a solver for phase 3, and a backward pass for phase 4.

It's hard to explain how cool that is to me. It's moments like these I suspect that I might be a compiler nerd. [Huzzah!](https://www.youtube.com/watch?v=y13n1CeyqjY&t=132s)

## That's all for now

Valen's borrow checker is still pretty young, a mere 3,600 lines.

And it's far from complete! We still have to add a lot of things to it, like interrupting control flow (break, continue), [16](https://verdagon.dev/blog/reverse-borrow-checking#note16) associated groups, closures, [17](https://verdagon.dev/blog/reverse-borrow-checking#note17) all the fun stuff.

We'll get there one day. That day will be glorious.

Thanks for reading!

In the next few articles, I'm hoping to explain more about how we'll blend in reference counting and generational references. I'm also going to write about all the other languages exploring new memory safety approaches like this ([Ante](https://antelang.org/), [Carbon](https://github.com/carbon-language/carbon-lang), [Zeta](https://github.com/zeta-lang/zeta), [Fang](https://github.com/devast8a/Fang), [exclusion typing](https://dilated.me/blog/posts/introduction-to-exclusion-typing/), and more!).

This is just the tip of the iceberg, so stay tuned by subscribing to my [RSS feed](https://verdagon.dev/rss.xml), [r/valen](https://www.reddit.com/r/Valen/), or joining the [Valen discord](https://discord.gg/SNB8yGH). And follow me on [Bluesky](https://bsky.app/profile/verdagon.bsky.social) and [Mastodon](https://fosstodon.org/@verdagon)!

Cheers,

- Evan Ovadia

16

The current plan is to first refactor things so the borrow checker is looking at a [control-flow graph](https://en.wikipedia.org/wiki/Control-flow_graph) instead of a typed AST. That should make interrupting control flow much easier.

17

I'm pretty excited about closures. I think I found a *very* elegant way to make them work. Stay tuned!

[^1]: Or [destructor insertion](https://www.youtube.com/watch?v=IpuvQUVB8Cg), and now, checking memory safety!

[^2]: 0% of this article was written by AI. [More thoughts here](https://verdagon.dev/blog/personal-ai-policy).

    My stance: if you want someone to take the time to read something, take the time to write it by hand.

    Thanks for reading =)
