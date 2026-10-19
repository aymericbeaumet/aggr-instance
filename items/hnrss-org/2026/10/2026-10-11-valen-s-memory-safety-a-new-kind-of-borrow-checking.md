---
title: 'Valen''s Memory Safety: A New Kind of Borrow Checking'
link: https://verdagon.dev/blog/valen-group-borrowing
source: hnrss-org
published: 2026-10-11T12:06:22Z
updated: 2026-10-11T12:06:22Z
first_seen: 2026-10-11T20:11:56.661477988Z
authors:
- verdagon
content: extracted
html: 2026-10-11-valen-s-memory-safety-a-new-kind-of-borrow-checking.html
extra:
  comments_url: https://news.ycombinator.com/item?id=50042362
  num_comments: 54
  points: 112
---

References that remember where they point to!

Oct 11, 2026  —

This post is about [Valen](https://github.com/valen-lang/valen)'s new **flexible borrow checker.** Welcome!

You all know me, I *love* exploring new memory safety approaches and blending them together in weird ways.

It's part of my eternal quest to find the **memory safety holy grail:** something as powerful as Rust's borrow checking, yet something as simple and flexible as reference counting or garbage collection. [^1] [^2]

I've long suspected that to find the holy grail, we'd have to solve **three memory-safety challenges:**

- Can we make a *more relaxed* borrow checker? [^3]
- Can we make a borrow checker read *more complex data?* [^4] [^5]
- Can we make those two systems work together at the same time?

Vague and arcane, I know! Further below I'll explain what that all means, and how [Valen](https://github.com/valen-lang/valen) might have found a solution for them. [^6]

This post mainly focuses on the first piece: Valen's **more relaxed borrow checker.**

So in this post, I'll talk about **what it is, what it can do, and how it works.**

In this article, I take some liberties with syntactic sugar for clarity: auto-deref, directly indexing a struct (my\_vec\[0\]), and inferring types from groups (entity in world.entities\[\]) are coming in November. Aside from those syntactic adjustments, the borrow checker works for everything we talk about. [^7]

And, along the way, I'll explain some of the **weirder tidbits,** like:

- How Valen's borrow checker is shaped to let [Valen code call Rust code](https://verdagon.dev/blog/golden-spike-reviving-vale-valen).
- How a compiler can remember *where* a reference is pointing.
- How Valen has *one* kind of borrow reference, as opposed to the two that other borrow checkers have.

Also, this article is *gloriously long* and has a lot of context, so I'll let you know when to skip ahead.

Let's dive in!

### Group borrowing: a near miss, and huge potential

(This is just backstory that I love telling, feel free to !)

Valen's approach is built on the "Group Borrowing" proposal which was designed by my friend [Nick Smith](https://github.com/nmsmith).

It's built around the core concept that **the compiler remembers where a reference points to,** (I'll explain this later) and it uses that information to check that programs are memory-safe.

The design's life had a rough start; it was almost buried in the flows of time. It was [originally designed with Mojo in mind](https://gist.github.com/nmsmith/cdaa94aa74e8e0611221e65db8e41f7b), but alas, despite my best efforts I couldn't convince the higher-ups that we should use it. [^8] Group borrowing was dead before it had the chance to succeed.

But I knew it had *massive* potential, if it could just *make it into the world somehow.*

So we spent months sharpening the explanation and figuring out its benefits, and wrote a post [^9] on it last year, hoping other languages would pick it up.

And they did! **It spread far and wide.** [^10]

- Google's [Carbon](https://github.com/carbon-language/carbon-lang) language pivoted completely to group borrowing (and even [credited](https://github.com/carbon-language/carbon-lang/discussions/6397) our article, which was nice of them!)
- [Ante](https://antelang.org/) started working it into their language, see [Shared Mutability and Stability](https://antelang.org/docs/language/#shared-mutability-and-stability).
- Plecra's [exclusion typing](https://dilated.me/blog/posts/introduction-to-exclusion-typing/) is redesigning the Rust borrow checker with concepts from group borrowing and GhostCell. [^11]
- [Zeta](https://github.com/zeta-lang/zeta) blended group borrowing with [disjointness](https://gist.github.com/FlameyosSnowy/a49a35ba006238be55d8c3e410c31061) [^12] to factor run-time facts into compile-time memory safety.
- [Fang](https://github.com/devast8a/Fang) is working it into their designs, see [Cyclic References in Fang](https://gist.github.com/devast8a/128988bf7dde7911a237e3e37a9e94de).
- And now, my [Valen](https://github.com/valen-lang/Valen) combines group borrowing with [Vale](https://vale.dev/)'s [^13] approach, and makes it [interop with Rust code](https://verdagon.dev/blog/golden-spike-reviving-vale-valen) too.

Most of these languages are circling the same sort of challenge: how to make a more flexible borrow checker.

For example, Valen wants to make a borrow checker that's flexible enough to work with reference counting and generational references, and Carbon wants to make a borrow checker flexible enough to work with C++ code.

This is difficult, because there's been a **fundamental conflict** between borrow checking and other approaches. It can be summed up like this: [^14]

1. Borrow checking has the "shared-xor-mutable" restriction: if you hold a reference to an object, nobody else can change the object.
2. Other approaches want to allow holding a reference to an object while letting others change it. [^15] [^16]

You're probably thinking, "There's definitely no way to resolve this."

It does seem that way!

But group borrowing actually **resolves the conflict,** by relaxing "shared-xor-mutable" to "no use-after-free".

That was cryptic and won't make any sense, but it will later when I explain how group borrowing works.

So let's see **what group borrowing can do,** and then I'll explain how it works.

## What group borrowing can do

Group borrowing gives a program memory safety without run-time cost. [^17]

It catches use-after-free problems at compile time, and it does it in a way that has less restrictions than previous approaches.

Here's an example where a **use-after-free error** is caught at compile time:

```valen
struct Entity {
  hp int;
}
struct World {
  entities Vec<Entity>;
}
func main() int {
  let world = World(Vec<Entity>.new());

  world.entities.push(Entity(42));
  let first_ref = &world.entities[0];

  world.entities = Vec<Entity>.new();

  // Compile error: Used a borrow after invalidated
  return first_ref.hp;
}
```

It's an unusually flexible approach. Usually, compilers have trouble with the below program, but this approach **understands that it's safe:** [^18]

```valen
func main() int {
  let list = Vec<int>.new();

  // Make two refs to the list
  let ref_a = &list;
  let ref_b = &list;

  // Mutate the list through both refs
  ref_a.push(42);
  ref_b.push(73);

  return 42;
}
```

Group borrowing accepts a lot of patterns that are normally hard for borrow checkers, such as:

- Having multiple local variables that point to the same object, and write through any of them (like above).
- Take a parameter pointing at an object, and another parameter pointing somewhere *inside* that object, and write through only the latter (we'll see this in the next section).
- Having multiple function parameters that point to the same object, and write through any of them. [^19]
- Make a "rollback" struct, that will change an object when it goes out of scope, like the [ScopeGuard](https://stackoverflow.com/a/19683195) pattern. [^20] [^21]
- Having multiple structs that have mutable references to a common subsystem. [^22]
- Plus a lot more that I'll explain further below. [^23]

These are patterns we love from C++, but no language has figured out how to do all of these in a memory-safe way at compile time.

### A more interesting example

(Or, jump to [how it works](https://verdagon.dev/blog/valen-group-borrowing#how-valens-borrow-checker-works))

For example, here's a step function that takes in a World reference, and a reference to one of the entities inside it.

The important part is the entity in world.entities\[\] mut, which I'll explain below.

```valen
struct World {
  entities Vec<Entity>;
}
func step(world &World, entity in world.entities[] mut) {
  entity.advance();
  let collision = world.get_collision_for_entity(entity);
  entity.resolve(collision);
}
```

Note these parts:

- in means "a reference to one of".
- world.entities\[\] means "world's entities's elements"
- mut there means "we can modify it." [23](https://verdagon.dev/blog/valen-group-borrowing#note23) [24](https://verdagon.dev/blog/valen-group-borrowing#note24)

Put together: entity is a reference to one of world's entities's elements, and we can modify it.

This is the core concept of the approach: **the compiler knows where a reference points to,** [25](https://verdagon.dev/blog/valen-group-borrowing#note25) because the user specifies (via in) where it points to.

Now let's see how this looks in other languages, to see why this is so nice. (Or jump to [how it works](https://verdagon.dev/blog/valen-group-borrowing#how-valens-borrow-checker-works)!)

First, the equivalent in C++:

```c++
struct World {
  vector<Entity> entities;
};
void step(const World& world, Entity& entity) {
  entity.advance();
  auto collision = world.getCollisionForEntity(entity);
  entity.resolve(collision);
}
```

Of course, the above requires care to uphold memory safety, since C++ doesn't check for memory safety at compile time.

Let's see an equivalent in Rust, where we *do* get automatic memory safety: [26](https://verdagon.dev/blog/valen-group-borrowing#note26)

```rs
struct World {
  entities: Vec<Entity>,
}
fn step(world: &mut World, entity_id: usize) {
  let entity_mut = &mut world.entities[entity_id]; // bounds check, possible panic
  entity_mut.advance();
  let entity_read = &world.entities[entity_id]; // bounds check, possible panic
  let collision = world.get_collision_for_entity(entity_read);
  let entity_mut = &mut world.entities[entity_id]; // bounds check, possible panic
  entity_mut.resolve(collision);
}
```

Rust is generally nice to work with, but in some cases (like this one), it isn't quite as fast or simple as the C++ case, because we need to re-fetch the reference every time someone needed a &mut to something else in the same hierarchy.

Another option is to refactor our program to be "flatter" (so to speak) which has its own tradeoffs. [27](https://verdagon.dev/blog/valen-group-borrowing#note27)

If you want to get really fancy, you can use [GhostCell](https://docs.rs/ghost-cell/latest/ghost_cell/)s and add brands to your data and access them with a GhostToken:

```rs
struct World<'brand> {
  entities: Vec<GhostCell<'brand, Entity>>,
}
fn step<'brand>(
  world: &World<'brand>,
  entity: &GhostCell<'brand, Entity>,
  token: &mut GhostToken<'brand>,
) {
  entity.borrow_mut(token).advance();
  let collision = world.get_collision_for_entity(entity, token);
  entity.borrow_mut(token).resolve(collision);
}
```

You can see the conundrum.

- The C++ example was simple, [28](https://verdagon.dev/blog/valen-group-borrowing#note28) but not memory-safe.
- Vanilla Rust was memory-safe, but a little slower, and a little more complex. [29](https://verdagon.dev/blog/valen-group-borrowing#note29)
- Adding GhostCell gave us the same speed as C++, but it's still a bit complex.

With Valen's approach, we get something fast, safe, and simple:

```valen
struct World {
  entities Vec<Entity>;
}
func step(world &World, entity in world.entities[] mut) {
  entity.advance();
  let collision = world.get_collision_for_entity(entity);
  entity.resolve(collision);
}
```

A pretty nice simplification!

Now, let's finally get to the explanation of *how it works.*

23

This is not like Rust's unique references (&mut). There can be other references to that same entity.

24

This function cannot modify the whole world, just the entities inside it (this is actually a huge superpower, and means the optimizer can better optimize this function *and* its callers, but I'll explain that more later).

25

Or more specifically, it knows *roughly* where it's pointing to; entity in world.entities\[\] points at *somewhere in the group* of objects that live in the entities array.

26

In Rust, we need a &mut Entity for entity\_mut.advance(), and then have to throw it away to get the containing &World back to give to the get\_collision\_for\_entity call. Then later, we need to re-fetch the &mut Entity for the resolve call.

27

Specifically, it becomes easier to work with the borrow checker (and faster if your game happens to do ECS-style iteration), but you lose some of the RAII benefits of single ownership.

For example, if you flatten a heterogeneous tree into multiple arrays, nodes aren't automatically deleted when their parents are.

Also, if you split into parallel collections too much, you might risk "ID chasing" cache-miss slowdowns.

In the end, I'm wary of any memory safety approach that influences us into one architecture over another, because it [might not be the best](https://www.reddit.com/r/roguelikedev/comments/i3xekn/ec_vs_ecs_for_roguelikes/).

Even group borrowing does this to some extent, which is why I'm excited to blend in reference counting or generational references.

28

Yes, I know how ironic it is to use "C++" and "simple" in the same sentence!

29

There's some nuance here. You can generally rearrange your Rust code to match group borrowing's speed, even without GhostCell.

## How Valen's borrow checker works

**TL;DR:** The approach is made of five mechanisms:

- Every object is described by a **path,** according to **single ownership.**
- Every reference **remembers the path** of the object it's pointing to. [30](https://verdagon.dev/blog/valen-group-borrowing#note30)
- When we destroy an object, everything pointing at its path is **no longer usable.**
- A function signature describes **the paths it modifies.**
- When we call a function, everything pointing inside its modified paths are **no longer usable.**

That was pretty abstract, so I'll explain all of these more.

### Single Ownership

**TL;DR:** Valen's borrow checker is based on single ownership, like C++ and Rust. Every value is "owned" by a containing object, array, stack frame, or global. [31](https://verdagon.dev/blog/valen-group-borrowing#note31)

If you know how C++ or Rust works already, !

If you don't know (or just like reading!), I'll explain what single ownership is.

In single ownership, every piece of data has one "owner". For example...

If we have this C++ program:

```c++
#include <vector>
struct Engine { int fuel; };
struct Ship { unique_ptr<Engine> engine; };
void foo(vector<Ship>* ships) { ... }
void main() {
    vector<Ship> ships;
    ...
    foo(&ships);
}
```

...or this Valen program:

```valen
struct Engine { fuel int; };
struct Ship { engine Box<Engine>; }
func foo(ships &Vec<Ship>) { ... }
func main() {
    ships = Vec<Ship>.new();
    ...
    foo(&ships);
}
```

...we can say this: [32](https://verdagon.dev/blog/valen-group-borrowing#note32)

- main's stack frame "owns" vector\<Ship> ships;
- The vector\<Ship> ships; owns each Ship.
- Each Ship owns its Engine (via the unique\_ptr).
- Each Engine owns its int fuel;
- foo does **not** own the vector, it just has a raw pointer.
- main is the only thing without an owner.

If you've coded in C++ or Rust, you're probably familiar with this mindset.

If you've coded in C, you might think like this too, even though C doesn't explicitly track single ownership. If you trace an object's journey all the way from its malloc() call to its free() call, all of the variables/fields that the pointer passes through are dealing with the "owning pointer", so to speak. It's almost like how detectives track the "chain of custody" for evidence. In other words, who is "responsible" for it at any given moment. [33](https://verdagon.dev/blog/valen-group-borrowing#note33)

Heck, even Java and C# programmers sometimes think in terms of single ownership. If you're supposed to call an object's "dispose"/"cleanup"/"destroy"/"unregister"/etc. method at some point, you can trace that object's journey all the way from new to that (conceptually destructive) method call, and those are the variables/fields that are handling its "owning reference", so to speak.

Single ownership, as explained so far, is the foundation for a lot of languages:

- If you add regular (unrestricted) pointers and references, you get [C++](https://en.wikipedia.org/wiki/C%2B%2B).
- If you add generational references and region borrowing, you get [Vale](https://verdagon.dev/blog/generational-references).
- If you add exclusivity-checked references, you get [Rust](https://www.rust-lang.org/).
- If you add group borrowing and a few other mechanisms, you get Valen.

With single ownership, every object can be described by a "path", which I'll explain next.

30

I'm sensing strange resonance between "Every reference remembers the path of the object it's pointing to." and generational references where "every reference remembers the ID ("generation") of the object it's pointing at." It *could* be said that group borrowing is a sort of compile-time generational reference. But more on that in the next article!

31

We relax this later with reference counting.

32

You could also say that main's stack frame owns foo's stack frame. Or you could say the reverse (that's how async works).

33

Or more specifically, responsible for freeing it, or responsible for giving it to someone who will make sure it's eventually freed.

### Paths

Every object can be described by a path.

For this example:

```valen
struct Entity { hp int; }
struct World {
  diameter int;
  entities Vec<Entity>;
}
func main() int {
  let x = 42;
  let world = World(73, Vec<Entity>.new());

  ...
}
```

Here are all the paths in this program:

- x
- world
- world.diameter
- world.entities
- world.entities\[\]
- world.entities\[\].hp

A path always starts with one of these:

- Local variable (like world).
- A function parameter.
- A function's "group parameter" (we'll see this later).

After that start, it will have things like:

- .diameter to name a member (like above).
- \[\] to name the *elements* of a collection (like above).
- .SomeType to name a union's variant. [34](https://verdagon.dev/blog/valen-group-borrowing#note34)
- plus a few other options that I'll get into later. [35](https://verdagon.dev/blog/valen-group-borrowing#note35)

Every piece of data has only one owner (because single ownership), so every object has one path. [36](https://verdagon.dev/blog/valen-group-borrowing#note36)

So what are paths used for?

References!

The compiler knows what path each reference points to, and that helps uphold memory safety.

34

So for example, if we have a union/enum Engine containing either a WarpEngine or a ImpulseEngine, the path might be my\_engine.WarpEngine.

35

Such as an associated group, or sub-types of a multi-typed group. More on these later!

36

This isn't *strictly* true; objects can actually be reached through multiple paths, if we use things like associated groups. That's a whole topic for another time though.

### Every reference knows what path it points to

For example, in this program:

```valen
struct Entity { hp int; }
struct World {
  entities Vec<Entity>;
}
func main() int {
  let world = World(Vec<Entity>.new());

  world.entities.push(Entity(42));

  let entity_ref = &world.entities[0];

  ...
}
```

entity\_ref's type is &Entity in world.entities\[\].

That means it's a reference to one of world's entities's elements.

And in the example we saw before:

```valen
struct World {
  entities Vec<Entity>;
}
func step(world &World, entity in world.entities[] mut) {
  entity.advance();
  let collision = world.get_collision_for_entity(entity);
  entity.resolve(collision);
}
```

You can see the entity in world.entities\[\], specifying an entity parameter that points at one of world's entities's elements.

If you know Rust: a path isn't really like a lifetime. It's easier to grok when you think about them as different things. [37](https://verdagon.dev/blog/valen-group-borrowing#note37)

Now let's find out why we're doing all of this. Let's see how we use paths for memory safety!

37

In practice a lifetime can be used as a very coarse-grained location, but a lifetime is more like a span of instructions.

### Using paths for memory safety [38](https://verdagon.dev/blog/valen-group-borrowing#note38)

The compiler uses paths to **detect use-after-free;** it detects when we're dereferencing a reference that is pointing at data that has already been destroyed.

It does this with these two rules:

- When we modify an object, we mark any reference pointing *inside* that object as "invalidated".
- When we try to use an "invalidated" reference, the compiler shows an error.

Take a look at this main function.

```valen
struct Entity { hp int; }
struct World {
  entities Vec<Entity>;
}
func main() int {
  let world = World(Vec<Entity>.new());

  world.entities.push(Entity(42));
  let first_ref = &world.entities[0];

  world.entities = Vec<Entity>.new();

  // Compile error: Used a borrow after invalidated
  return first_ref.hp;
}
```

In it, these two things happened:

- At world.entities = ..., the compiler invalidated all references pointing inside it (world.entities\[\]); it invalidated first\_ref.
- At return first\_ref.hp, the compiler sees we're using first\_ref, so shows an error.

Note how I said:

When we modify an object, we mark any reference pointing *inside* that object as "invalidated".

We don't invalidate references pointing *to* an object. We invalidate references pointing *inside* the object.

Take a look at this main function.

```valen
struct Ship { fuel int; }
func main() int {
  let vec = Vec<Ship>.new();
  vec.push(Ship(42));

  let vec_ref = &vec;
  let elem_ref = &vec[0];

  // Mutate `vec`.
  // Don't invalidate `vec_ref`.
  // Invalidate `elem_ref`.
  vec.push(Ship(73));

  // Can still use `vec_ref`.
  vec_ref.push(Ship(81));

  // Can't use `elem_ref`,
  // Compile error: Used a borrow after invalidated
  return elem_ref.fuel;
}
```

When we call push, that modifies the vec.

That invalidates all references pointing *inside* vec.

That means that elem\_ref is invalidated (though vec\_ref is still valid).

So we can still use vec\_ref, but we can't use elem\_ref.

Notice how this is all happening without describing whether a reference is mutable or not.

Valen, however, has only **one kind of borrow reference**.

This can be confusing if you're coming from C++ and Rust, which both have multiple kinds of references (C++ has const and non-const, Rust has & and &mut).

In Valen, the mutability of something **isn't part of its type,** the surrounding function determines it.

Valen does put mut on function parameters, but that's syntactic sugar. mut is not part of the parameter's type. [39](https://verdagon.dev/blog/valen-group-borrowing#note39)

38

You've found the easter egg note!

[Sergeant Reckless](https://en.wikipedia.org/wiki/Sergeant_Reckless) was a horse who made it to the rank of Staff Sergeant in the United States Marine Corps.

Sergeant Reckless liked eating anything, including scrambled eggs, beer, coca-cola, and apparently poker chips.

She achieved high honors, and was awarded the Animals in War & Peace Medal of Bravery.

If you see this easter egg, sneak "morning sun" (her Korean name) into the comments on [HN](https://news.ycombinator.com/item?id=50042362), [lobste.rs](https://lobste.rs/s/gmnqhj/valen_s_memory_safety_new_kind_borrow), or [reddit](https://www.reddit.com/r/programming/comments/1x37b3z/valens_memory_safety_a_new_kind_of_borrow_checking/) to receive your own award!

(Congrats to [q2dominic](https://www.reddit.com/r/programming/comments/1x37b3z/comment/pfafycz/), [timpi4ever](https://www.reddit.com/r/programming/comments/1x37b3z/comment/pfbg94l/), and [joonazan](https://www.reddit.com/r/ProgrammingLanguages/comments/1x37bbs/comment/pffaf0q/) for finding this first!)

39

mut is part of the function signature. It's similar to how languages put async on the function, instead of on parameters.

### Function signatures communicate invalidations

So how did the compiler know that push modified the vec?

push has to say so, in its signature, with the mut keyword.

Here's push's signature:

```valen
func push<T>(self &Vec<T> mut) { ... }
```

...which is syntactic sugar for this:

```valen
func push<T>(self &Vec<T>) mut(self) { ... }
```

Notice how mut(self) isn't part of self's type. It's more part of the *function.*

So when main called vec.push(...), main was passing vec in for self.

The compiler then sees the mut(self), knows self is actually vec, so knows vec was mutated.

That's how it knew to invalidate anything pointing *inside* vec.

Here's another example:

```valen
func step(world &World, entity in world.entities[] mut) {
  entity.advance();
  let collision = world.get_collision_for_entity(entity);
  entity.resolve(collision);
}
```

We're not mutating the whole world; we're only mutating something in world.entities\[\].

This is good because this won't invalidate callers' references to entities.

For example, this step\_and\_read is calling the above step function which *doesn't* invalidate the entity.

```valen
func step_and_read(world &World, entity in world.entities[] mut) {
  // Only modifies an entity's contents.
  step(world, entity);

  // ...so can still use `entity`!
  entity.read_book();
}
```

And if we craft the Valen compiler right (and patch LLVM), this *might* let us harness the optimizer in a way that no language ever has before. [40](https://verdagon.dev/blog/valen-group-borrowing#note40)

40

I talk more about this further below, but TL;DR, if the optimizer knows that two functions only overlap in reads and not writes, then it can reorder them, perhaps even hoisting calculations out of a loop.

## A twist on group borrowing

The original group borrowing designs included a lot of the above (and more!). Here's the things that Valen will be adding on top of it, to make it *sing.*

### Mutable-in-Immutable

Recall this program, where we took in a world that was immutable *except* for its entities elements:

```valen
struct World {
  entities Vec<Entity>;
}
func step(world &World, entity in world.entities[] mut) {
  entity.advance();
  let collision = world.get_collision_for_entity(entity);
  entity.resolve(collision);
}
```

We're receiving two parameter references, where we can mutate through the more specific one, and we can't mutate anything outside of it.

Valen adds this so that we don't have to do the classic borrow-checking pattern of taking in the entire world mutably.

This will have a couple benefits.

First, it could help with a program's architecture, because it helps us make **stronger APIs.**

We can enforce that nothing in this entire function can change any part of the world that we don't expect.

I imagine that will be particularly helpful when working in a team with enterprising newhires (or with product managers who love to throw vibe code at you).

Second, I suspect this feature could make Valen code **optimize better.**

I won't go too deeply into it here, [41](https://verdagon.dev/blog/valen-group-borrowing#note41) but basically, when you give the optimizer more fine-grained information about what might change and what won't change, it can better identify places where it can rearrange code to be faster. [42](https://verdagon.dev/blog/valen-group-borrowing#note42) [43](https://verdagon.dev/blog/valen-group-borrowing#note43)

### Temporary Uniqueness

One of Valen's goals is to be able to **seamlessly call into Rust code**, as easily as Kotlin calls into Java, or C++ into C.

Of course, this will be tricky, because group borrowing and Rust have different kinds of references:

- Group borrowing allows others to change the data that your reference is pointing at.
- In Rust, every reference is either shared (&) or unique (&mut). When you hold a reference, nobody else can change what your reference is pointing at.

So the question that arose was: Can we turn a group borrowing reference into a Rust reference, temporarily?

It turns out, yes!

The mechanism is pretty simple. When Valen is calling Rust:

- We can hand a Valen reference into a Rust &mut if it's the *only* reference that can reach that data during that call.
- If we have multiple Valen references that can point to the same object, they can only be handed into Rust & references.

I talk a little bit about this in [Memory Safety Across the Valen/Rust Boundary](https://verdagon.dev/blog/boundary-memory-safety#temporary-uniqueness-and-noalias), check it out!

This temporary conversion *could* evolve into a broader feature one day. We want Valen to support something like Rust's Vec<&mut Ship>, where each element is a unique reference, and group borrowing doesn't have that.

I suspect Valen can have something like a "unique group", e.g. a unique(world.ships\[\]) annotation on a function, which would prevent making new references pointing at that part of the hierarchy.

This aspect needs more thought though. One of Valen's greatest strengths is that it only has one kind of borrow reference... so introducing a second "unique" reference requires some careful consideration.

41

Ask me on Mastodon or Bluesky and I'm happy to explain!

42

This might require patches to LLVM to make it better able to harness the information. It tends to think in terms of noalias, not in terms of "read-only". Tricky, but possible!

43

This won't necessarily mean Valen will be faster than C++, Rust, etc. I *think* you can do extra refactoring or use unsafe in those languages to get equivalent speedups. Valen's main benefit here would be in ergonomics and flexibility, not necessarily speed.

### Wildcard Descendant Paths

This is another feature that helps Valen call into Rust code.

While trying to get the [Golden Spike](https://verdagon.dev/blog/golden-spike-reviving-vale-valen) to work, I ran into a conundrum.

When Valen calls a Rust function that returns a reference (like -> &Gem below), how does it know *where* that reference points?

For example:

```rs
impl Chest {
  pub fn gem(&self) -> &Gem { &self.gem }
}
```

Valen was confused by this, because Valen likes to know the path for every reference.

In Valen's perfect world, it would see the above like this:

```valen
func gem(self &Chest) -> &Gem in self.gem { &self.gem }
```

But alas! Valen can't infer the in self.gem from the Rust code.

But if you zoom out a bit, the Rust code is trying to express that it's returning something to **"somewhere inside"** self.

So, let's add that concept to Valen!

Valen now interprets the above Rust function as if it were this:

```valen
func gem(self &Chest) -> &Gem in self.gem... { &self.gem }
```

The in self.gem... means "points *somewhere inside* self.gem".

This is called a **wildcard descendant path**, and it definitely needs a better name.

I describe this a little more in [Memory Safety Across the Valen/Rust Boundary](https://verdagon.dev/blog/boundary-memory-safety#temporary-uniqueness-and-noalias).

I suspect this will be useful for more than just Rust interop. It could erase inner details from signatures, and make library APIs a little more flexible and forward-compatible. However, we should be careful here too, because that same purpose can be served by other features in theory. [44](https://verdagon.dev/blog/valen-group-borrowing#note44)

44

For example, we could have a type publicly re-export a private group under a public name. Something like an "associated group", so to speak.

## How this all fits in

Above, I mentioned the "memory safety holy grail", and the **three challenges** that we need to solve to find it:

- Can we make a *more relaxed* borrow checker?
- Can we make a borrow checker read *more complex data?*
- Can we make those two systems work together at the same time?

Valen's borrow checker is the key to #1: it lets us have multiple references to any object, and lets us read and write through any of them.

#2 and #3 will be in another post (since this post is getting massive!) but I can't resist giving a **quick preview of what it's all about.**

These are my hasty scribbles to try and explain it in half a page. Nobody should try to understand it. Quick, [jump ahead](https://verdagon.dev/blog/valen-group-borrowing#thats-all-for-now)!

Recall this example from above:

```valen
struct World {
  entities Vec<Entity>;
}
func step(world &World, entity in world.entities[] mut) {
  ...
}
```

I want users to have the option of using reference counted classes when they don't want to specify the paths. Like this:

```valen
class World {
  entities Vec<Entity>;
}
func step(world World, entity Entity) mut {
  ...
}
```

It would be amazing to give the users the freedom to simplify like that. [45](https://verdagon.dev/blog/valen-group-borrowing#note45)

Of course, blending reference counting and borrowing is **infamously difficult.** [46](https://verdagon.dev/blog/valen-group-borrowing#note46)

The answer lies in a thought experiment from 2023 I called [Arrrlang](https://verdagon.dev/blog/myth-zero-overhead-memory-safety#arrrlang-a-pirate-themed-language-with-zero-overhead-memory-safety). Its name was a joke, but it had an interesting premise: represent the heap as N global arrays, where N is the number of types in your program. [47](https://verdagon.dev/blog/valen-group-borrowing#note47)

It occurred to me: group borrowing *understands* arrays. And reference counting can be thought of as N arrays, all having references to each other.

If we put those two facts together, then the key insight emerges: if we **describe reference-counted classes to a borrow checker as if it's a bunch of arrays,** then we might be able to have borrow references into them in a memory-safe way.

A compiler doesn't need to *actually compile* reference counted objects to an array, of course. **But it *can* describe it that way to the borrow checker,** using group-borrowing terms.

If you do it that way, suddenly a lot of problems disappear, and it clarifies exactly what building blocks we need to add to group borrowing:

- A "multi-typed group", similar to a Vale region, as opposed to vanilla group borrowing where every group needs a type. [48](https://verdagon.dev/blog/valen-group-borrowing#note48) [49](https://verdagon.dev/blog/valen-group-borrowing#note49)
- A "run-time" group. References into the run-time group don't get invalidated.

Like I said, none of this will make sense. Keep an eye out for the next post that talks about all this!

45

Reference counting has a lot less restrictions (e.g. references don't need to be temporary, like in borrow checking), which means you unlock a lot more patterns (observers, intrusive linked lists, etc). It also means nobody can free an object while you're accessing it, which is most often a good thing.

But it depends on your perspective. Reference counting occasionally has problems with cycles, and it's not very compatible with linear types. For most things, I like reference counting. For the more complex parts, or parts that need speed, I like borrowing.

I think it's good for a language to support both.

46

It's difficult for all the same reasons as why Rc\<RefCell\<T>> has that RefCell in there.

47

It was an example of a "zero-cost" language that actually had some cost, in the form of ID-chasing and bounds checking.

48

This will also be useful for enums!

49

Fun fact, this is how they represent heaps under the hood in [Iris](https://iris-project.org/pdfs/2015-popl-iris1-final.pdf), which was used to formally verify that parts of Rust are safe.

## That's all for now

Thanks for reading!

In the next few articles, I'll explain more about how we'll blend in reference counting and generational references, and how Valen's group borrow checker is implemented.

This is just the tip of the iceberg, so stay tuned by subscribing to my [RSS feed](https://verdagon.dev/rss.xml), [r/valen](https://www.reddit.com/r/Valen/), or joining the [Valen discord](https://discord.gg/SNB8yGH). And follow me on [Bluesky](https://bsky.app/profile/verdagon.bsky.social) and [Mastodon](https://fosstodon.org/@verdagon)!

Cheers,

- Evan Ovadia

## Appendix: The type-stability exception

This part isn't implemented in Valen yet. Stay tuned!

Group borrowing can also be smart enough to know that if you say my\_vec.push(42), then even though push says mut(self), it won't invalidate pointers to my\_vec.size.

This makes sense because no matter how anyone modifies a my\_vec (which is a Vec), the thing at my\_vec.size will still be there. Nobody can modify a Vec's size to be anything other than an integer. In other words, it's "type-stable".

Generalizing a bit, group borrowing will never invalidate any type-stable data.

It won't invalidate pointers to any contained type-stable things (primitives, structs, or fixed-size arrays), or any type-stable things inside them. It only invalidates pointers pointing inside any type-unstable things (enums' contents, pointer's pointee).

For example, in this program, nobody modifying the contents of Level can invalidate your reference to its Terrain.

```valen
struct Level {
  terrain Terrain;
  entities HashMap<EntityId, Entity>;
  location_to_entity HashMap<Location, EntityId>;
}
func add_entity(level &Level mut, entity Entity) {
  level.location_to_entity.insert(entity.loc, entity.id);
  level.entities.insert(entity.id, entity);
}
func main() {
  let level = ...;
  let terrain_ref = &level.terrain;

  // Doesn't invalidate terrain_ref!
  level.add_entity(Entity(1, Loc(4, 5), 42));

  print(terrain_ref.tiles.len());
}
```

[^1]: This quest led to a lot of interesting experiments with [constraint references](https://verdagon.dev/blog/raii-next-steps), [linear types](https://verdagon.dev/blog/higher-raii-uses-linear-types), [reference counting](https://github.com/verdagon/RegionsRCCellularAutomataBenchmark), and a [lot more](https://verdagon.dev/grimoire/grimoire).

[^2]: 0% of this article was written by AI. [More thoughts here](https://verdagon.dev/blog/personal-ai-policy).

    My stance: if you want someone to take the time to read something, take the time to write it by hand.

    Thanks for reading =)

[^3]: Or more specifically, "can we make a borrow checker that supports multiple *read-write* borrow references to a single object?"

    This post explains that in full, so keep reading!

[^4]: I'll explain this more below, but by "more complex data" I mean entire graphs of data, where objects can freely have references to other objects without restriction, like you see in C++, Java, etc. as opposed to something like Rust or Fortran where things generally can't have references to each other.

    So when I say "read more complex data", I mean: how can we have immutable borrow references pointing at that kind of arbitrary graph of data?

    (Note, Rust structs can have references to other Rust structs, but only temporarily. This is why ECS is such a popular choice in Rust games. That's why I still count Rust as more on the Fortran side of things.)

[^5]: In short, this one is answered by [Vale](https://vale.dev/)'s blend of [generational references and region borrowing](https://verdagon.dev/blog/generational-references); its [immutable region borrowing](https://verdagon.dev/blog/zero-cost-borrowing-regions-part-1-immutable-borrowing) lets the compiler track all pre-existing data as a temporarily frozen "region" of data, that we can have borrow references into.

[^6]: For now, I'll just hint that Valen might have solved these by blending [Vale](https://vale.dev/)'s approach, a [certain thought experiment](https://verdagon.dev/blog/myth-zero-overhead-memory-safety) from 2023, Nick Smith's Group Borrowing approach, and a few twists to make things work together harmoniously.

[^7]: Specifically, in today's compiler, one needs to manually dereference, index into arrays directly like my\_vec.data\[0\] instead of the struct, and put types on all parameters like entity &Entity in world.entities\[\].

[^8]: Such is life when working on someone else's language!

[^9]: This post you're reading right now is a much better explanation of group borrowing, but you can see the old post at [Group Borrowing: Zero-Cost Memory Safety with Fewer Restrictions](https://verdagon.dev/blog/group-borrowing).

[^10]: Soon I'll be writing an article on how all of these languages' approaches work and compare with each other, stay tuned!

[^11]: It rejects less code by using first class permissions to merge lifetimes with the type checker. Definitely check it out!

[^12]: Zeta's borrow checker can prove when two references point to *different parts* of the same data (they're "disjoint"). For example, it can prove list.get\_mut(i) and list.get\_mut(j) are disjoint when it knows that i != j, even if both indices are runtime values. Read [more here](https://gist.github.com/FlameyosSnowy/a49a35ba006238be55d8c3e410c31061)!

[^13]: The main differences between Valen and Vale:

    - Valen will have [group borrowing](https://verdagon.dev/blog/group-borrowing), Vale had [region borrowing](https://verdagon.dev/blog/generational-references).
    - Valen will have normal generational references, Vale had probabilistic generational references.
    - Valen will have (and Vale didn't have) Rust interop, Rc, comptime.
    - Valen won't have (and Vale did have) perfect replayability, fearless FFI.

    Both have linear types.

    Their syntax is still mostly the same, but Valen might change to be more of a "simpler Rust" syntactically.

[^14]: I often called this conflict "the language killer" because it destroyed many versions of many experimental languages, including one of Vale's earlier approaches.

    Between [constraint references](https://verdagon.dev/blog/raii-next-steps) and Vale's [current blend](https://verdagon.dev/blog/generational-references), there was an approach I called "hybrid-generational memory", where you could keep a reference alive by "tethering" a generational reference for the duration of a scope. I went through *thirty two* versions of it before giving up.

[^15]: C++ wants this because that's often how we use it; we want a single unique\_ptr owning an object, and a bunch of non-temporary raw pointers referring to it. Reference counting and garbage collection want the same sort of thing. In both cases, difficulties arise when someone else can modify an object while you have a borrow reference to it.

[^16]: This can be a controversial goal; some of us believe that a reference's referent should never change out from under you, but an ID's/index's referent should be able to (like in Rust). Others believe that that's too restrictive. I think the user should be able to choose.

[^17]: Though it's debatable if any language can get memory safety with *zero* run-time cost. See [Chasing the Myth of Zero-Overhead Memory Safety](https://verdagon.dev/blog/myth-zero-overhead-memory-safety), it applies to group borrowing as well.

[^18]: Normally, borrow checkers don't understand how to have two references that can both modify an object, so they give [compile errors](https://play.rust-lang.org/?version=stable&mode=debug&edition=2024&gist=805c3432a7bdca38352367c512b257f9). Group borrowing is flexible enough to not forbid these multiple read-write references, because it knows how to more precisely guard against use-after-free problems specifically.

[^19]: For an example of this, see the "fn attack" examples in [this page](https://verdagon.dev/blog/group-borrowing#nicks-borrowing-system). This is usually hard for borrow checkers. Though, [GhostCell](https://docs.rs/ghost-cell/latest/ghost_cell/) sometimes helps!

[^20]: Normal borrow checkers usually have trouble with this because the user wants two read-write references (which violates shared-xor-mutable): one to use directly, and one that lives inside the ScopeGuard object.

[^21]: This could also be the foundation for a user-defined defer statement!

[^22]: I don't have an example of this handy, but imagine that you wanted a bunch of File objects to each have a reference to a common FileSystem object. Borrow checking usually has problems with that (too many read-write references to the same object), but it should work fine in group borrowing.

[^23]: I'll cover it more below, but using group borrowing as a starting point, we can:

    - Make programs that are faster at run-time, because we can give more information to the optimizer.
    - Use reference counting without Cell or RefCell or Mutex.
    - Use generational references, for the same reason.
