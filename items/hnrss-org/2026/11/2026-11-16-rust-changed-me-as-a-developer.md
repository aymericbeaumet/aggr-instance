---
title: Rust changed me as a developer
link: https://blog.startifact.com/posts/how-rust-changed-me/
source: hnrss-org
published: 2026-11-16T20:16:50Z
updated: 2026-11-16T20:16:50Z
first_seen: 2026-11-17T01:00:47.489879523Z
authors:
- speckx
content: extracted
html: 2026-11-16-rust-changed-me-as-a-developer.html
remote_preview:
  url: https://blog.startifact.com/social_icons/rss.svg
  alt: feed
---

The programming language you use affects how you think as a software developer [1](https://blog.startifact.com/posts/how-rust-changed-me/#why). That may seem surprising to you; you may have picked up JavaScript pretty quickly knowing Python, and it didn't really affect how you think or develop. That's because those languages are pretty similar. The more different a programming language is from what you're used to, the more it can change you. Rust was quite different to me, so it did.

I've been developing with Rust for over 6 years; more than 4 years [professionally](https://github.com/faassen/xot) [now](https://github.com/Paligo/xee). One of the reasons I really wanted to become more proficient with Rust is because I knew it would challenge me, and hopefully change me for the better as a developer. I think it did.

What I learned and am going to discuss here is not exclusive to Rust. You can encounter many of these concepts in other contexts, and indeed I had encountered quite a few of them already. But in this case, Rust helped bring them into better focus.

## Where were enums all my life?

We're all familiar with product types, though not usually under that name:

```rust
struct Human {
    first_name: String,
    last_name: String,
}

```

The fields of the `Human` struct [2](https://blog.startifact.com/posts/how-rust-changed-me/#struct) can vary independently from each other, and we have to deal with all possible combinations: all possible first names and last names, combined. The *product* of their combinations, to to speak.

But it turns out there's another type thing that's been around forever but that the programming community has mostly been been ignoring [3](https://blog.startifact.com/posts/how-rust-changed-me/#impoverished): the sum type. A sum type expresses variants: something is either one thing, or another, out of a bunch of variants:

```rust
enum Person {
    Human { first_name: String, last_name: String },
    Cat { name: String, cuteness: Cuteness }
}
```

A `Person` is *either* a human or a cat: that's the *sum* of the possiblities. Each variant can have data associated with it, and this can be a product type, but the `Person` type is just these options (two in this case). In code where you need to deal with a person, you need to handle each variant by `match` in on the object. The Rust compiler enforces this.

```rust
match some_person {
    Human { first_name, last_name } => { 
	    // do a thing with first_name and last_name
    },
    Cat { name, cuteness } => {
	   // do a thing with name and cutenes
    }
}
```

When you handle the human variant, you only need to worry about `first_name` and `last_name`. You don't have to worry about `name` or `cuteness` at all. These cases are distinct from each other.

It turns out that it's really helpful to model stuff using sum types. People using functional programming languages already knew this. You can model optionality (`null`) this way: you got either some value, or nothing. You can also model errors: it's either okay (with whatever value you return in case of success), or an error (with a value describing the error).

Any language without sum types now feels impoverished to me. I know I can simulate it with some type field, but the job of a programming language is to help me handle basic stuff, and this is basic stuff.

## Enums and abstraction

In an object oriented programming language, you handle variants using dynamic dispatch and an interface (or inheritance). You do this:

```
interface Person {
    def do_one_thing()
    def do_another_thing
}

class Human implements Person {
    ...
}

class Cat implements Person {
   ...
}
```

The idea is that you can now use `Person` anywhere without worrying about the details inside: it's loosely coupled. You only use their interface.

This is indeed a great idea, but Rust taught me that this has tradeoffs I may not want to pay for. You pay two things: performance, and the need to create the interface.

Let's talk about the need to create the interface. You need to devise an interface that actually makes sense for your application: *how* are you using humans and cats everywhere? Doing this work of abstraction may be extremely worthwhile to do, especially if you anticipate unknown future persons that you need to be able to plug in later without having to modify the code that uses them -- you want your code use the [open/closed principle](https://en.wikipedia.org/wiki/Open%E2%80%93closed_principle).

But what if you are developing a single domain, and inside that you just want to be able to handle cats and humans and that's it? There's just no need to plug in other types of persons, and certainly not from outside.

Or let's say you do want to add a new variant once every while. Say you want to add `Dog`. Maybe you're actually okay going through the code that handles `Human` and `Cat` and add handling for dogs too? Maybe it actually is helpful to see those other handlers right there too when you do so? The compiler is going to ensure you don't miss any spots!

Or what if the abstraction you are trying to devise *is* the variance and that's more or less what you can say about it? For error handling, the abstraction is either okay with some value, *or* some error value.

In this case you may be paying the price of creating an interface without it being worthwhile.

`enum` may be all you actually need -- low ceremony, and statically checked. Finally I'll note that enums don't lock you out from creating that abstraction - you can add methods to an enum too, just like on a struct.

**Enums made me a better programmer as now I have another elementary, simple, easy to understand language construct for abstraction in my mental toolbox.**

## Dispatch galore

In many languages there are two types of dispatch: dynamic dispatch, which we've discussed in case of our `Person` interface, and conditional dispatch, like this:

```
if something {
   do_this_thing()
} else
   do_another()
}
```

Rust has dynamic dispatch, if you ask for it. It also has the powered up version of conditional dispatch I already discussed: match on enums. It has conditional dispatch too - that's just \[a special case\]\[https://docs.rs/bool/latest/bool/enum.Boolean.html].

There's another type of dispatch that Rust makes you think about: dispatch that's statically compiled.

What if you're dealing *either* with groups of humans *or* groups of cats, and you want to do some operations on them, and those operations have common behavior (say, you want to count them). The group, or collection, doesn't really care about what's in it, as long as every item fulfills certain basic properties.

You could create an enum `Person`; either an enum or an interface with multiple implementations, and then dispatch during runtime to every member of the group based on whether that entry is a human or a cat. But this means you have to account for mixed groups of humans and cats, even though we just said those don't actually exist.

Instead, you can express this differently in Rust. You still have a `Person` interface (trait), but you compose your individual type of `Person` (`Human` or `Cat`) during compile time.

```rust
struct Group<T> {
   // some fields	
}

// compile time assembly, 
// then runtime creation of the assembled group of cats
let cat_group: Group<Cat> = Group::new();
```

Rust now refuses to compile if you use non-cat operations on items in the group, That's an invariant you now do not need to worry about anymore. This will be familiar to you with generics in other languages like TypeScript or Python.

In Rust, when you process your group of cats, there *is* no individual check somewhere whether an entry is a human or a cat, as it already knows it's all cats before the program even ran. Rust directly dispatches to the cat code, and the human code is not play. You avoid indirect dispatch for every group member, which can matter in situations where performance is important.

**Rust made me a better programmer as I now better understand the connection between dispatch and performance.**

## Type driven development

When I discovered Python coming from C++ back in the late 1990s, dynamic typing was a revelation. You could so much with so little ceremony. For the last decade or so I have been on a long winding road back to types (far more ergonomic types) and more ceremony. I have the impression many have been on this journey along with me [4](https://blog.startifact.com/posts/how-rust-changed-me/#if_types_anyway).

Types can statically check your code before it runs. In Rust, they can help the compiler compile stuff down to really high performance code. Both are great. But types can also have more subtle benefits. Let me get into this by analogy.

Why create automated tests?

**Obvious**: Automated tests ensure various properties of your code work and remain working.

**Subtle**: Writing tests forces you to think about the architecture and APIs you're implementing more carefully. Making code testable can lead to better architecture. This is the insight of test driven development.

Why write documentation?

**Obvious**: People (and now machines) can understand [5](https://blog.startifact.com/posts/how-rust-changed-me/#understand) the why and what of your software by reading the documentation.

**Subtle**: The act of writing the documentation can force you think about the architecture and APIs of your software. If you can't write about it, maybe you haven't gotten it right yet? That's documentation driven development.

Why write types?

**Obvious**: Statically check your code before it runs! Prevent runtime errors! Performance!

**Subtle**: You model the domain model using types, trying to make sure it hangs together. Enums are really valuable here. You express [parse, don't validate](https://lexi-lambda.github.io/blog/2019/11/05/parse-don-t-validate/) in them. The types guide the architecture and APIs of your software.

This perhaps sounds like big design up front processes where people write all the tests, or all the docs, or all the types, in advance, and *then* do the implementation. [6](https://blog.startifact.com/posts/how-rust-changed-me/#big_design) I'm not talking about this -- I'm talking about iterative development where you keep going back to tests, docs and types as you learn.

Rust brought into focus how types are yet another tool to help me think while I develop.

**Rust made me a better programmer as it let me fully embrace type driven development.**

## The Biggest Surprise

Here is the biggest surprise I had coming to Rust. Much of the behavior I associated with dynamic interpreted languages with a rich runtime like Python can actually be done statically in Rust using macros.

Let's talk about magic.

Say you want to dump the state of a Python object -- you inspect all its fields, look at their type, turn each into a string, combine all the strings together into a big one. Now you can store it somewhere. Later on you restore that string back into an object. You can save and load any objects: magic! Python calls this [pickle](https://docs.python.org/3/library/pickle.html).

What if you need to serialize to JSON? Python has a type system, and a library like [Pydantic](https://pydantic.dev) inspects the type objects during runtime and then uses this information to let you automatically serialize a Python object to JSON, or parse it back into Python again. More magic!

Rust is a compiled language. Types aren't runtime objects. Without reflection, which [takes special effort](https://facet.rs/), runtime introspection of types is not a thing in Rust.

But Rust can still do what Pickle and Pydantic do, at compile time.

With [serde](https://serde.rs/), Rust's answer to pickle and Pydantic, you use a normal Rust type, annotate it with a few declarations, and then you can serialize it to JSON, or toml config format, and many other formats, and load it back again. The annotation looks like this:

```rust
#[derive(Serialize, Deserialize)]
struct Human {
    first_name: String,
    last_name: String
}
```

With [clap](https://docs.rs/clap/latest/clap/), you create the type that expresses exactly what command line arguments you expect (with `enum`s too!), document it using the normal way you document any API in Rust, and boom, you have a CLI with built in help. The code that handles the various cases gets well-typed data - at that point it's been parsed correctly already, using the same type you used to express the CLI.

Like this:

```rust
/// My first CLI
#[derive(Parser)]
struct MyFirstCli {
    /// How many cats do you want?
    #[arg(long, short)]
    amount_of_cats: usize
}
```

Again declarative `#[]` stuff going on. It looks a lot like decorators in Python. But Python decorators are executed during runtime, and we just said Rust doesn't normally let you do that.

How does that work in Rust? Macros! Macros lets you write code that generates code during compile time. With serde, the macro generates a whole bunch of serialization code and deserialization code for the types annotated with it. This generated code gets compiled along with the rest of your code. The compiler can optimize it just like any other code.

Code generation is scary. Macros are scary. Doing introspection in a dynamically typed language is a lot easier than using Rust macros, where code is parsed and generated. Macros shouldn't be randomly mixed with application code; macros belong in frameworks. That said, something being easy doesn't mean it should be used casually: in Python you typically want to carefully encapsulate your magic in a declarative framework too.

**Rust made me a better programmer as I learned about a new class of framework patterns.**

## Ownership and Architecture

Rust famously lets you write code without memory leaks that doesn't use a garbage collector. It also stops you from mutating data you're also reading elsewhere, making it impossible to stumble into whole classes of weird undefined behavior, especially with code that runs in parallel.

Rust's ownership system is one of the main things that makes learning Rust hard. Python, JavaScript, Java and many other languages have mutable references everywhere, and you can just create cyclic data structures. Rust won't let you do either out of the box.

The equivalent of this does not compile in Rust:

```python
class Item:
    def __init__(self, value):
        self.value = value
      
class Container:
    def __init__(self, items):
        self.items = items

my_item = Item()
my_container = Container()
my_container.items.append(my_item)
my_item.value = "I change you!"
```

This also doesn't compile:

```python
class Node:
   def __init__(self, parent, value):
       self.parent = parent
       self.value = value
       self.children = []

   def add_child(self, node):
       node.parent = self
       self.children.append(node)
```

Rust won't just let you have circular references, in this case both children *and* a parent reference.

This feels very limiting at first! What do you do instead? For mutating stuff, you learn how to separate mutation (often during construction) from using your data. That is a thing that's good to do in many contexts -- less invariants makes it's easier to reason about, but it takes a bit of getting used to Rust strictly enforcing it.

For circular references it's trickier. I will go into it next, when I discuss arenas.

A weird thing happens: if you lean into this friction it can improve the architecture of your codebase. Code becomes less coupled. It's hard to create weird backchannels with action at a distance that makes code difficult to understand, test and debug [7](https://blog.startifact.com/posts/how-rust-changed-me/#not_impossible).

Rust code locally can be difficult to understand. But how the data flows, what does what, in the larger codebase, tends to be cleaner.

**Rust made me a better programmer as I now think about ownership more carefully, which has a positive impact on architecture.**

## The arena pattern everywhere

A few years ago I analyzed a bunch of XML tree libraries in Rust. The best I found, [roxmltrree](https://docs.rs/roxmltree), has an immutable tree. But I needed a mutable tree. I found quite a few libraries that did support mutability, but many did not support parent references. You couldn't walk up the ancestor chain of a node. This, along with missing support for some XML features, is why I created my own in the form of [Xot](https://docs.rs/xot/latest/xot/).

The reason for this surprising lack of parent references is the ownership limitation I discussed before: Rust makes circular references difficult. You can get around it using reference counted types and internal mutability, but in a lot of cases this pattern isn't really optimal -- it's verbose, less performant, and reference count cycles risk memory leaks. What you do instead is store nodes in an array and you use the array indexes to node as your reference. You can then have an index referencing your parent, and use index references to your children, or any other circular reference.

A fancy word for this pattern is an 'arena' and in Rust they show up everywhere.

In an arena, you store a lot of objects together in the same region of memory (in an array). What does it let you do?

- iterate fast over its contents. Objects being in the same memory region means the CPU can use its cache effectively. This is *much* faster than arbitrary main memory access and loops really benefit.

- fast allocation. The arena is allocated from the heap as a block, and after that your allocations may just be a few increments.

- interning: you store something once in the arena and use it in many places, so you don't duplicate data.

- the arena owns its stuff. Everything in the arena has the same lifetime. Indexes between objects in an arena can be circular without worrying.

Rust pushes you to use arenas. This involves more ceremony than just using a direct reference, but it also means Rust pushes you to write code that computers can execute really efficiently.

## Performance has a quality all its own

I mentioned performance a number of times. But what about [premature optimization](https://blog.startifact.com/posts/is-premature-optimization-the-root-of-all-evil/)? The great irony of this quote is that it comes from a paper where the author, Knuth, argues in favor of keeping the `goto` statement around, as it's essential for writing efficient software.

Rust lets you know what you pay for. Reference counting? Dynamic dispatch? You can do it, but it's going to be visible in your code. Rust *also* often lets you know when you *don't* want to pay for it: just look at [tower middleware](https://docs.rs/tower/) and despair about all the generic types and type constraints that get piled onto a relatively simple abstraction.

When you have these tools available, need to develop new wisdom about tradeoffs that never seemed to matter. If you write Python code you're not too worried about hash tables being a performance bottle neck, but in Rust code they certainly can be. You can get lost along the way into hyper optimization land. Often you need to just get go [8](https://blog.startifact.com/posts/how-rust-changed-me/#clone_and_own). Many patterns that are just plain readable Rust code anyway happen to also be friendly to the CPU executing it.

But Rust overwhelming you with details you may not want to think about is not the whole story, because performance is not just quantitative: if your code can truly be more performance, it can access new qualities. When you can write code that uses no memory and no CPU time [9](https://blog.startifact.com/posts/how-rust-changed-me/#exaggeration), it changes how you think. It changes what's possible.

Sometimes the quality of performance is obvious: if you write numerical code, or a simulation, of course you don't want to wait for the answer. Being able to do many computations really fast is key to making AI possible.

But it's more subtle that that: modern computers are *really* powerful, and *ridicously* fast, and we generally ignore that and just throw more resources at our problems. There are good reasons [why this happens](https://blog.startifact.com/posts/bloat-retrofuturism/). But if we open our eyes a little, there are new architectural possibilities we can explore.

While the power of macros were the biggest language surprise, I think this is the biggest shift in my thinking: access to performance unlocks possibilities I hadn't considered before. Take my experiment [Colchis](https://github.com/faassen/colchis): a library can store vast amounts of JSON in memory, while retaining full access, even indexed access (I still hope to add jsonpath one day!), to JSON data. Before Rust, this was a possibility that wasn't even on my radar.

**Rust made me a better programmer because I can make the computer do more with less, opening up new areas of opportunity.**

## Conclusion

Rust is one of the most difficult programming languages I've learned -- and thus one of the languages that changed me the most. There's a simple part of Rust that isn't much harder to manage than any language, and in some cases may actually make your life easier -- `enum`s, declarative type-driven frameworks, and the excellent dev tooling come to mind. Even arenas are fundamentally not that difficult to understand. The complexity is in unlearning some habits and learning new ones. And then there's a more challenging complex Rust, mostly, but not always under the surface, there when you need it.

Rust expands the reach of what I can build. That's what makes me happy I went on this journey. [10](https://blog.startifact.com/posts/how-rust-changed-me/#all_the_tradeoffs)

1

AI agents write a lot of code. They know the programming language, so do you need to know it? Does the language still affect the developer? If you share my belief it's good [to look at the code](https://blog.startifact.com/posts/sigh-coding/), then it helps if you understand the code you're reading. I also think it's never been more important to think about how the systems you use affect the way you think and behave.

2

If you don't know what a struct is, just call it a class in your head, and you're good.

3

You have them in many languages but in impoverished forms, like `true`/`false`, or C's `enum`.

4

If you're going to do static type checking anyway for the guarantees it brings, perhaps use a language designed around it (enums!) that also gives you performance benefits?

5

Does AI truly understand? Do you want to have a philosophical debate? What do we mean by *understand*? Natural language is [inherently stretchable](https://en.wikipedia.org/wiki/Pragmatics).

6

With AI we've seen the return of big design up front. I may be wrong, but I'm still skeptical of this approach, and still a fan of iterative development and incremental learning and theory building.

7

Not impossible. I've seen it. People can do it if they really try, and it still makes life harder; it makes it much harder to test. But at least it's more obvious and more difficult to do.

8

Clone and own all the stuff! Which incidentally can also lead to more contiguous memory layouts...

9

No memory and no CPU time is an exaggeration, but little Rust programs sometimes feel that way.

10

AI development is shifting all the tradeoffs in software development - they're not gone, but they have shifted. The AI can write Rust, even if you don't know it. But I trust plenty of contexts remain where my Rust experience plus AI leads to a better outcome than AI by itself.
