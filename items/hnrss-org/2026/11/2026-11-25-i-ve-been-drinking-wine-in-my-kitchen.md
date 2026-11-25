---
title: I've been drinking wine in my kitchen
link: https://scottaaronson.blog/?p=10362
source: hnrss-org
published: 2026-11-25T03:02:10Z
updated: 2026-11-25T03:02:10Z
first_seen: 2026-11-25T10:22:41.845233465Z
authors:
- nill0
content: extracted
html: 2026-11-25-i-ve-been-drinking-wine-in-my-kitchen.html
remote_preview:
  url: https://s0.wp.com/_si/?t=eyJpbWciOiJodHRwczpcL1wvc2NvdHRhYXJvbnNvbi5ibG9nXC93cC1jb250ZW50XC91cGxvYWRzXC8yMDIxXC8xMFwvY3JvcHBlZC1KYWNrZXQuZ2lmIiwidHh0IjoiU2h0ZXRsLU9wdGltaXplZCIsInRlbXBsYXRlIjoiZWRnZSIsImZvbnQiOiIiLCJibG9nX2lkIjoxMjk1MjA1ODB9.siOtN7gHw4tefA_rZickBw4GfI6tPxGgOQ1AXr2ZoOQMQ
---

See I don’t even *like* alcohol much. Like, I’ll have a half-glass of wine at a fancy dinner if everyone else is having some, or one beer at a reception, but that’s it. These days, though, can you blame me?

* * *

Yesterday, as you all probably saw, hot on the heels of its [proof](https://scottaaronson.blog/?p=10344) of the Generalized Riemann Hypothesis, Anthropic released a [proof of Valiant’s VP≠VNP conjecture](https://github.com/anthropics/formal-math/blob/vp-vnp/vp-vnp/paper.pdf)—a central algebraic analogue of the P≠NP conjecture, and considered a “warmup” to proving P≠NP since the late 1970s. Then, later the same day, OpenAI released a [proof](https://github.com/openai/math/blob/main/preprints/VP-and-VNP-are-different-November-23-2026/paper.pdf) of the VP≠VNP conjecture as well.

You can read all about the VP vs. VNP problem in Section 6.5 of my [P vs. NP survey](https://www.scottaaronson.com/papers/pnp.pdf). But briefly, for those tuning in from home, Valiant’s conjecture says that the [permanent](https://en.wikipedia.org/wiki/Permanent_\(mathematics\)) of an n×n matrix has no algebraic circuits (that is, circuits built out of addition and multiplication gates and constants) of size polynomial in n, unlike the [determinant](https://en.wikipedia.org/wiki/Determinant), which does. Anthropic’s result says that a circuit of size exp(Ω(log4/3 n)) is needed, whereas OpenAI gets exp(Ω(log5/4 n)): so, more than polynomial albeit less than exponential. As is often the case, the best known upper bound here is ~2n. The lower bounds are said to work over all fields of characteristic other than 2. (What’s special about 2? Over a field of characteristic 2, addition and subtraction are the same, so the permanent just *is* the determinant, and is easy to compute!)

From what little I understand so far, the proof techniques in the Anthropic and OpenAI papers look similar. They both do some sort of highly nontrivial “compression” or “shortening,” followed in the end by a diagonalization—the oldest trick in the book! Diagonalization is what should let these arguments evade the infamous [natural proofs](https://en.wikipedia.org/wiki/Natural_proof) barrier. There’s probably also a story to tell about how the arguments evade an algebraic-circuit version of the relativization barrier, but I don’t know how to tell it yet.

One immediate implication of these results is that, if you want to map an n×n matrix A to an m×m matrix B whose entries are affine functions of the entries of A, and have the permanent of A be equal to the determinant of B, then m must be exp(Ω(log4/3 n)). This was the question directly targeted by Ketan Mulmuley’s [Geometric Complexity Theory](https://en.wikipedia.org/wiki/Geometric_complexity_theory) program—which, however, these papers don’t use, taking a more direct route to the desired lower bound.

As is de rigeur these days, the proofs come with Lean, formally certifying their correctness to a level considerably *beyond* what we’d normally demand for a math paper published in a journal. If recent experience is any guide, this will not prevent boatloads of morons from denying that AI could *possibly* have solved these problems, and latching onto anything whatsoever (e.g., unused Lean code or Lean that deviates slightly from the natural-language version) to declare the proofs to be fallacious … and such claims will then go viral, lapped up by an eager audience on Bluesky and other places hungry for AI denial. If there’s any real technical issue with the proofs, I’ll be happy to hear about it from experts, but my patience for arguing with the flat-earthers is at an end.

Daniel Harlow asked me whether the proof of VP≠VNP raises my confidence in P≠NP itself, which I’d lowered from 97% to 95% in light of OpenAI’s and Anthropic’s recent refutation of the Exponential Time Hypothesis. I said sure, I’m now back up to 96%.

The possibility that this will be followed up soon by a proof of P≠NP itself has not escaped my notice, or anyone else’s, *certainly* not at the AI companies. Will the big one come tomorrow? Next week? Next month? In a year? Who the hell knows? Hence the wine drinking, which I wouldn’t call sad, but just … *thirsty*.

* * *

If that wasn’t enough for you—if you still want *more*—yesterday Sanjeev Khanna, Aaron Putterman, and Junkai Song posted an [ECCC preprint](https://eccc.weizmann.ac.il/report/2026/339/) with a deterministic linear-time algorithm for finding minimum spanning trees in graphs. (The best previously-known algorithm was ridiculously *close* to linear time—n α(n) where α is the inverse of the [Ackermann function](https://en.wikipedia.org/wiki/Ackermann_function)—but not quite. A *randomized* linear-time algorithm was known.) This was a huge open problem for decades, and one that my wife Dana spent roughly two years working on.

Khanna et al. write, “the main ideas in \[our\] paper were entirely generated by AI—specifically 6.2 Astra, on a mixture of Ultra and Max effort—with minimal human intellectual intervention. The authors then worked to first carefully verify, and then to simplify and clarify the treatment.”

* * *

Anyway, yeah, tonight I’ll again raise my glass, I’ll pray that at the bottom of this terrifying rollercoaster lies a world that my children and I will be glad to inhabit, and I’ll mutter to myself the eternal words:

*God would like us to be joyful\
Even though our hearts lie panting on the floor;\
How much more can we be joyful,\
When there’s really something\
To be joyful for.*

*To life, to life, l’chaim!\
L’chaim, l’chaim, to life!\
It gives you something to think about,\
Something to drink about,\
Drink l’chaim, to life!*

* * *

**Update:** See [Lance’s post](https://blog.computationalcomplexity.org/2026/11/vp-vnp.html) for more.

 This entry was posted on Tuesday, November 24th, 2026 at 5:17 pm and is filed under [Complexity](https://scottaaronson.blog/?cat=5), [The Fate of Humanity](https://scottaaronson.blog/?cat=8). You can follow any responses to this entry through the [RSS 2.0](https://scottaaronson.blog/?feed=rss2&p=10362) feed. You can [leave a response](https://scottaaronson.blog/?p=10362#respond), or [trackback](https://scottaaronson.blog/wp-trackback.php?p=10362) from your own site.

You can use rich HTML in comments! You can also use basic TeX, by enclosing it within $$ $$ for displayed equations or \\( \\) for inline equations.

After two decades of mostly-open comments, in July 2024 *Shtetl-Optimized* transitioned to the following policy:

All comments are treated, by default, as personal missives to me, Scott Aaronson---with no expectation either that they'll appear on the blog or that I'll reply to them.

At my leisure and discretion, and in consultation with the [*Shtetl-Optimized* Committee of Guardians](https://scottaaronson.blog/?p=6576), I'll put on the blog a curated selection of comments that I judge to be particularly interesting or to move the topic forward, and I'll do my best to answer those. But it will be more like Letters to the Editor. Anyone who feels unjustly censored is welcome to the rest of the Internet.
