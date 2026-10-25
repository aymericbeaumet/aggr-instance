---
title: Small Programs and Languages
link: https://ratfactor.com/cards/pl-small
source: hnrss-org
published: 2026-10-24T21:44:23Z
updated: 2026-10-24T21:44:23Z
first_seen: 2026-10-25T10:04:58.814034362Z
authors:
- andsoitis
content: extracted
html: 2026-10-24-small-programs-and-languages.html
remote_preview:
  url: https://ratfactor.com/cards/images/david_ungar_self_and_self2.jpg
  alt: snippet of a slide that reads Simplicity trumps expressiveness
---

Page started: 2025-06-02

My [Implementing a Forth](https://ratfactor.com/forth/implementing) article got some great feedback, particularly around the subject of tiny Forth implementations. (And it was an excuse to list some of the tiniest Forths I’ve seen.)

I know I’m not alone in seeing the appeal of tiny Forths, tiny languages, tiny programs, and just small stuff in general. What’s up with that, anyway?

## The appeal of tiny programs

Tiny programs are approachable.

If I see that something is 200 lines of code, I’m *much* more likely to read it than if it’s 2,000 or 20,000 lines.

What’s more, if it’s a program that does something interesting in 20 lines when I was *expecting* 2,000, then I’m going to be very, very interested to read those 20 lines!

When I was researching DOM diffing libraries prior to creating [RetroV](https://ratfactor.com/retrov/), one of the repos I looked at was **ijk** by Luke Jackson.

I was pretty used to looking at fat JavaScript libraries at that point, so when I cracked open the source file for ijk, I was really surprised. Here’s the source: [index.js](https://raw.githubusercontent.com/lukejacksonn/ijk/refs/heads/master/index.js) (githubusercontent.com).

**Just 25 lines!** I was instantly intrigued.

More recently, one of the responses to my aforementioned Forth article was Philippe Brochard with a 46 byte "Forth".

**46 bytes!**

It’s *so* small, I can just put the hexdump right here:

```
50b8 8e00 31d8 e8ff 0017 003c 0575 00ea
5000 3c00 7401 eb02 e8ee 0005 0588 eb47
b8e6 0200 d231 14cd e480 7580 c3f4
```

Sure, very few people jump at a chance to read raw machine code. But that’s exactly my point. Something *this* small doesn’t feel so scary, right? I mean, 46 bytes. If you put the effort into it, you *know* you can figure out how it works eventually.

This next one almost serves as a counter-example because the code is so dense and frightening, but I can’t help mentioning it anyway.

One of my favorite software stories is Roger K.W. Hui’s Appendix A "Incunabulum" from *An Implementation of J* in which he describes Arthur Whitney writing an array-based language interpreter,

> "…on **one page** and in one afternoon — an interpreter fragment on the AT&T 3B1 computer. I studied this interpreter for about a week for its organization and programming style…"

Hui can list the entire interpreter in the appendix because it’s just that single page, **122 lines** of incredibly cryptic C, which you can view here:

I haven’t personally tackled that puzzle yet, but it’s in my unwritten list of dense computer things that I would print out and take with me if I suddenly found myself cast out of society and forced to go live alone up on a mountain or something.

(By the way, J is in the family of array languages which include APL. If you enjoy brevity, cryptic programming puzzles, and high performance numerical computing, these languages have got you covered!)

**Update July 2026:** Speaking of Arthur Whitney, [Alex](https://alexalejandre.com/) (alexalejandre.com) sent me a link that I knew I’d have to add here. Check this out: [https://fall.tr/kos](https://fall.tr/kos)

It’s a transcript of a Geo Carncross talk describing an operating system, kOS, featuring Whitney’s `k` array language, `kdb` database, and a GUI and window manager in a **90 Kb** bootable image. Individual source code stats are just as impressive (such as **60 lines** of C for the aforementioned GUI and window manager). And apparently kdb is 100 lines of C? Is that right? Even Whitney C, that’s still impressive. Now I’m even *more* intrigued by kdb than ever! And if any of that seems interesting, you’ll definitely enjoy the article [Impending kOS](https://vector.org.uk/impending-kos/) (vector.org.uk).

Something else that would certainly need to be in the collection I take to that lonely mountain would have to be *The Most Beautiful Program Ever Written*, according to the title of a talk by William Byrd. Spoiler alert: It’s this Lisp interpreter in Lisp:

```
(define eval-expr
  (lambda (expr env)
    (pmatch expr
      [`,x (guard (symbol? x))
        (env x)]
      [`(lambda (,x) ,body)
        (lambda (arg)
          (eval-expr body (lambda (y)
                            (if (eq? x y)
                                arg
                                (env y)))))]
      [`(,rator ,rand)
       ((eval-expr rator env)
        (eval-expr rand env))])))
```

## Recreation or fundamental truths?

Now, we all know that [code golfing](https://en.wikipedia.org/wiki/Code_golf) (wikipedia.org) is a programmer’s recreational game, and just for fun, right?

But is any of this actually useful?

I say yes. And I’ll go one step further: They are *meaningful*.

When you see a [ray tracer that fits on a business card](https://fabiensanglard.net/rayTracing_back_of_business_card/) (fabiensanglard.net), it makes you realize that a ray tracer is *something that can fit on a business card*!

That’s an actual *fact* about the minimum complexity of ray tracers.

> "…the length of a shortest computer program (in a predetermined programming language) that produces the object as output."

(I learned about Kolmogorov complexity in a book about Kurt Gödel and it was my favorite thing in what was otherwise a pretty dry and depressing book.)

The meaning I get from this is that code golfers, for the benefit of the whole human race, produce versions of programs that represent the minimum *complexity* of those programs. Way to go code golfers!

What do we know about the fundamental complexity of the Lisp programming language?

That’s a *fact* about Lisps with garbage collection. When you strip everything else away, you can get the essentials down to **436 bytes!**

I take comfort in this because that means the concept can be boiled down to this hand-held size. And *that* means I can probably fit it in my head.

Thanks to Frank Force, we know that a 3D browser game can fit in 2Kb of (insanely compressed) JavaScript source: [A 3D game in 2Kb of JS](https://frankforce.com/how-i-made-a-3d-game-in-only-2k-of-javascript/) (frankforce.com)

And we haven’t even scraped the *surface* of what the demoscene has to offer for computers going back to the 1980s. Here’s a good place to start: [https://demoscene.assembly.org/](https://demoscene.assembly.org/)

Mini-things are fun *and* useful *and* meaningful.

My point here is that you don’t have to actually study the intricate clockwork puzzle of a miniaturized/golfed program to benefit from it. The miniature simply serves to prove that the concept *can* fit on a business card and it *can* fit in your head. Go ahead and find a readable version to study instead. (The miniature will likely have many things to teach, but some of them will be about how to make miniature programs.)

## Small languages

Small programs are interesting. But I’m also interested in small programming languages.

*Generally* speaking, the smaller the language, the less expressive it is.

One of the most well-known, least expressive languages is assembly.

Assembly languages are *very* syntactically and conceptually simple. There’s not much language at all: it’s mostly a rigid sequence of opcode mnemonics. Even with macros, writing in assembly is mostly about understanding the puzzle of a particular CPU’s instruction set architecture, or ISA. (You can prove this by comparing the syntax of assembly for CISC and RISC processors. The puzzle is totally different, while the language remains practically unchanged.)

An honorable mention must be made here for [SNOBOL](https://ratfactor.com/snobol/), which is kind of like assembly language both for the rigid syntactic reasons listed above and *also* because it presents a similar control flow challenge. Like a CPU, it only understands jumps and call/return. (In every other way, SNOBOL is wildly different, being a string matching and replacing language. It’s weird and fun and highly effective.)

Moving on to syntactically tiny "high-level" languages, these three are brilliant:

- **Forths** win just about any "smallness" contest. The only syntactical construct is the space character. Forths are mind-bendingly flexible and powerful in a way that is downright disturbing.

- **Lisps**, (especially Scheme?), have small core languages with very simple syntax and enormous expressive power and flexibility.

- **Tcl** deserves to be mentioned with Forth and Lisp. Tcl is wild because *everything* is a string in a way that is hard to appreciate at first. You can construct your own language in Tcl just as you can with Forth or Lisp.

Forth, Lisp, and Tcl only require you to understand a few core ideas and give you limitless power in return. You could fit the syntactic core of these languages on an index card or less.

And yet, to wield these tools effectively requires a massive shift in thinking if you’re used to "normal" Algol-like procedural languages.

So how about something that is both small and simple and doesn’t require a blood sacrifice?

I think the Lua programming language fits the bill. Check out how small the core language is:

I’ve got the printed book version of this document and it’s a thin book! The Lua language is covered in the beginning and it is just 27 pages. **27 pages!** I have game manuals longer than that.

The C language is pretty small. I don’t think it’s super easy to program effectively and safely with C, but there’s no denying it’s compact and pretty expressive for its size.

Newcomer Zig is C-like. But I think it’s actually quite large and you need to know a lot of it to use it effectively. (I’m also a fan of Zig and think it’s [totally worth it](https://ratfactor.com/zig/hard). The size is there for a reason.)

JavaScript has a core language that is quite small. Probably on par with Lua, actually. Functions, arrays, and objects (used as dictionaries) are pretty much all the JS I use and I like it that way!

As with the natural human languages, the trade-off with a bigger programming language is, hopefully, greater expressivity.

Small or expressive. Which way should you lean?

In a wonderful talk by [David Ungar](https://en.wikipedia.org/wiki/David_Ungar) (wikipedia.org) titled *Self and Self: Whys and Wherefores*, he laments the mistake of Self’s multiple inheritance:

> "It’s far better to make a system a little too simple so a person has to write extra stuff, even write it twice with a comment that says, 'Look over here,' than to have a system that acts in strange ways and nobody can understand why. So, simplicity trumps expressiveness."

Got that? Here’s the bullet from the slide to help you remember:

![snippet of a slide that reads Simplicity trumps expressiveness](https://ratfactor.com/cards/images/david_ungar_self_and_self2.jpg)

I think small languages can *also* be fairly expressive, but only with very, very careful design. Recall Forth, Lisp, and Tcl.

**Microworlds:** The idea of small, self-contained, learnable programming environments *also* has a David Ungar connection I discovered while watching his talk. See "ARK" on my [Microworlds](https://ratfactor.com/cards/microworlds) card. (Smith and Ungar co-created the [Self language](https://selflanguage.org/) (selflanguage.org), which is a notable milestone in programming and still under active development to this day.)

Languages which come with full-featured interactive IDEs are obvious potential microworlds.

But I also think that traditional text file-based programming can give you the microworld experience as well. For that, a language needs high quality documentation that installs alongside the compiler/interpreter. And it should strive to have error output that can be understood by beginners, including explanations of what went wrong and hints for how to fix it or or where to find more information.

**Libraries:** Another easily overlooked aspect of a programming language’s apparent size is its "standard library" of functions, data structures, and methods.

I think function libraries are subject to the exact same size versus expressiveness challenge.

For example, learning the [Ramda](https://ramdajs.com/) (ramdajs.com) functional programming library for JavaScript was very much like learning a new programming language and I never managed to learn all of it. The composability of Ramda and the consistency of its curried functions was extremely eye-opening to me. My advice: A little bit of Ramda goes a long way!

**The feels:** Concision in our tools has a certain quality that goes beyond utility. Little things just *spark joy*, you know?

Why are small things so delightful?

## The appeal of tiny things in general

I’ve always had a fascination with miniatures, dioramas, and tiny scale models.

I know I’m not alone!

**Stephanie M. Langin-Hooper**, author of *Figurines in Hellenistic Babylonia* writes in [Tiny Things and Why We Love Them](https://cambridgeblog.org/2020/02/tiny-things-and-why-we-love-them-and-what-they-do-to-us/) (cambridgeblog.org):

> "…the act of shrinking a life-size thing to a smaller scale has an influential effect. The object is now cute, more accessible, more delicate, more 'squee'. It is also more helpless, and thus controllable – comforting and completely non-threatening due to its diminutive size.

Which is exactly the reaction I have when I see a tiny bit of code that does something useful.

Also, I’m super okay with "controllable" in the context of a computer, but the feeling of that paragraph changes a bit if we’re talking about, say, dolls.

Small scale helps make "big" things approachable:

> "What might be threatening to think about in the real-scale world can be more manageable and more conceivable in miniature."

Small things, "allow low-stakes experimentation," which is why we often build tiny models of things before we scale them up.

**Simon Garfield**, author of *In Miniature: How Small Things Illuminate the World* has many of the same answers. He writes in [Small world: Why we love tiny things](https://www.theguardian.com/global/2018/nov/04/small-world-why-we-love-tiny-things) (theguardian.com):

> "The answer lies in our desire for mastery and elucidation. The ability to enhance a life by bringing scaled-down order and illumination to an otherwise chaotic world – a world over which we may otherwise feel we have little control – cannot be overvalued. \[…\] At its simplest, the miniature shows us how to see, learn and appreciate more with less."

I think this is a **big** part of what I get out of [Assembly Nights](https://ratfactor.com/assembly-nights) with the small scale and orderly logic and internal consistency. Unlike the vast, fathomless Real World, an offline computer is a fantasy place where I can believe I could understand *everything* if I just tried hard enough for long enough.

> "The satisfaction of observing small things becomes a desire to make small things, and both stages address the human need for comprehension and order. We live in a huge and doomy universe, and controlling just a tiny scaled-down part of it restores our sense of order and worth."

I’ve been studying computer history for a little while and I’ve only scratched the surface. But one of the things I’ve been finding is that a lot of the "big" stuff in our field are just accumulations of lots of "little" victories as individuals have solved "little" problems in new and different ways.

> "We create small universes in which we may bury ourselves to the exclusion of all else. Blocking out real life for a while – always the prerequisite of the dedicated domestic hobby, from doll’s house modeller to jigsaw enthusiast to adult book colourist – may be contemplative, meditative, blinkered and essential. The people crouching over tiny details as if the world depended on it are only doing it because their world does depend on it."
