---
title: Five memorable books about programming (2008)
link: https://prog21.dadgum.com/19.html
source: hnrss-org
published: 2026-11-07T17:30:49Z
updated: 2026-11-07T17:30:49Z
first_seen: 2026-11-08T06:37:14.114093134Z
authors:
- andsoitis
content: extracted
html: 2026-11-07-five-memorable-books-about-programming-2008.html
---

I've read the classics--*Structure and Interpretation of Computer Programs*, *Paradigms of Artificial Intelligence Programming*--but I'd like to highlight some of the more esoteric books which affected my thinking.

***Zen of Assembly Language**\
 Michael Abrash, 1990*

I spent much of the 1980s writing 8-bit computer games (which you can [read about](http://www.dadgum.com/giantlist/archive/hague.html) if you like). Odd as it may seem in retrospect, considering the relative power of an 8-bit 6502 running at sub 2 MHz, I wasn't obsessed with optimizing for performance. If anything, I wanted the code to be *small*, a side effect of using a line-based editor and writing programs that someone would have to painstakingly type in from a magazine listing. Pages of DATA 0AFF6CA900004021... ugh.

Right when that period of my life came to a close, along came *Zen of Assembly Language*, by an author I had never heard of, which dissected, explained, and extended the self-taught tricks from my years as a lone assembly hacker. Even though Abrash was focused on the 8086 and not the 6502, it felt like the book was written personally to me.

This is also one of the most bizarrely delayed technical book releases I can recall. The majority of the book was about detailed optimization for the 8088 and 8086, yet it was published when the 80486 was showing up in high-end desktop PCs.

***Scientific Forth**\
 Julian Noble, 1992*\
 ***Fractals, Visualization, and J**\
 Clifford Reiter, 2000*

These two books are about entirely different subjects. One is about pure scientific computation. The other is about generating images. Each uses a different, wildly non-mainstream language for the included code.

And yet these two books follow the same general approach, one I wish were more commonly used. Superficially, the authors have written introductions to particular programming languages (which is why the language name is in the title). But in reality it's more that each author has an area of deep expertise and has found a language that enables experimenting with and writing code to solve problems in that field. As such, there aren't forced examples and toy problems, but serious, non-trivial programs that show a language in actual use. Dr. Noble demonstrates how he uses Forth for hardcore matrix work and, when he realizes that RPN notation isn't ideal in all circumstances, develops a translator from infix expressions to Forth. Clifford Reiter jumps into image processing algorithms, plus veers into lighter weight diversions with titles like "R/S Analysis, the Hurst Exponent, and Sunspots."

Both books are wonderful alternatives to the usual "Learning Programing Language of the Month" texts. Sadly, Julian Noble died in 2007.

***Programmers At Work**\
 Susan Lammers, 1986*

I used to soak up printed interviews with programmers and game designers (and typically game designers were programmers as well). I was enthralled by Levy's *Hackers*, more the game development chapter than the rest. *Programmers At Work* was in the same vein: philosophies, ideas, and experiences directly from an odd mix of famous and quirky programmers. But the book wasn't primarily about tech. It was about creativity. Most of the people interviewed didn't have degrees in computer science. There wasn't an emphasis on math, proving programs correct, lambda calculus--just people coming up with ideas and implementing them. And the game connection was there: Jaron Lanier talking about the psychedelic **Moon Dust** for the Commodore 64, Bill Budge's bold (and still unfulfilled) plan to build a "Construction Set Construction Set."

Lammers's book was the model I used when I put together [Halcyon Days](http://www.dadgum.com/halcyon). I also pulled *Hackers* into the mix by interviewing John Harris about his dissatisfaction with Levy's presentation of him. In an odd twist of fate, *Programmers at Work* and *Halcyon Days* were packaged together on a single CD sold through the Dr. Dobb's Journal library. The pairing has been around for ten years and is still available, much to my surprise.

*[**Thinking Forth: A Language and Philosophy for Solving Problems**](http://thinking-forth.sourceforge.net/)\
 Leo Brodie, 1984*

Yes, another book about Forth.

But this one is worth reading less for the Forth and more because it's one of the few books about how to decompose problems and structure code. You'd think this book was written by Fowler and friends, until you realize it's from the mid-1980s. That Brodie uses "factor" (which originated in the Forth community) instead of "refactor" is also a giveaway. What's impressive here is there's no OOP, no discussion of patterns, no heavy terminology. It's a book about understanding what you're trying to achieve, avoiding redundancy, and writing dead simple code.

It's worth it for the Forth, too, especially the interspersed bits of wisdom from experts, including Forth creator Chuck Moore.

[permalink](https://prog21.dadgum.com/19.html) *February 18, 2008*

## previously

- [Back to the Basics of Functional Programming](https://prog21.dadgum.com/18.html)
- [Don't Structure Data All The Way Down](https://prog21.dadgum.com/17.html)
- [Garbage Collection in Erlang](https://prog21.dadgum.com/16.html)
- [Why Garbage Collection Paranoia is Still (sometimes) Justified](https://prog21.dadgum.com/15.html)
- [Functional Programming Archaeology](https://prog21.dadgum.com/14.html)
