---
title: The stochastic parrots have apparently proved the Riemann Hypothesis
link: https://scottaaronson.blog/?p=10344
source: hnrss-org
published: 2026-11-20T20:03:07Z
updated: 2026-11-20T20:03:07Z
first_seen: 2026-11-20T22:37:07.598874728Z
authors:
- stratos123
content: extracted
html: 2026-11-20-the-stochastic-parrots-have-apparently-proved-the-riemann.html
remote_preview:
  url: https://s0.wp.com/_si/?t=eyJpbWciOiJodHRwczpcL1wvc2NvdHRhYXJvbnNvbi5ibG9nXC93cC1jb250ZW50XC91cGxvYWRzXC8yMDIxXC8xMFwvY3JvcHBlZC1KYWNrZXQuZ2lmIiwidHh0IjoiU2h0ZXRsLU9wdGltaXplZCIsInRlbXBsYXRlIjoiZWRnZSIsImZvbnQiOiIiLCJibG9nX2lkIjoxMjk1MjA1ODB9.siOtN7gHw4tefA_rZickBw4GfI6tPxGgOQ1AXr2ZoOQMQ
---

> “… it is very probable that all roots are real. Of course one would wish for a rigorous proof here; I have for the time being, after some fleeting vain attempts, provisionally put aside the search for this, as it appears dispensable for the immediate objective of my investigation.” –Bernhard Riemann, 1859

\[**Update:** According to “Poe’s Law,” there is no irony or sarcasm on the Internet so obvious that it won’t fly over countless people’s heads. In line with that, lots of people on social media are now saying variants of “how dare Scott Aaronson call LLMs ‘stochastic parrots,’ that demeaning and trivializing term from 2021? Doesn’t he understand that these so-called ‘stochastic parrots’ just proved the Riemann Hypothesis?” ROFL.\]

I’m in California to visit [Oratomic](https://www.oratomic.com/) and then [Inkhaven](https://www.inkhaven.blog/fall-26). I woke up this morning to the news that Levent Alpöge says that he’s gotten Anthropic models to [prove the Generalized Riemann Hypothesis](https://alpo.ge/remark.pdf), producing [17.4 million (!!) lines of Lean code](https://github.com/CocoSgt/riemann-hypothesis-lean).

At a dinner last night, someone asked me “so, how much longer until an AI proves the Riemann Hypothesis? Still several more years?” I replied, don’t be sure of such things anymore. After all, AI has already proved that the nontrivial zeros of the Riemann zeta function are [at most 3/8 away from the critical line](https://www.quantamagazine.org/ai-proved-the-quasi-riemann-hypothesis-what-happens-now-20261019/) (*Quanta* just published its article on that breakthrough yesterday!), and that already carries most of the implications for number theory that a full proof of RH would have. So who the hell knows what else is possible, and how quickly?

For those tuning in from home, the [Riemann Hypothesis](https://en.wikipedia.org/wiki/Riemann_hypothesis) says (in its easiest-to-understand version) that the number of prime numbers from 1 to n is very close to what it would be if each integer x>1 had an independent 1/ln(x) probability of being prime, up to a standard statistical error term that never exceeds Õ(√n). While we’ve known since 1896 that the number of primes from 1 to n is asymptotic to n/ln(n) (that’s the famous [Prime Number Theorem](https://en.wikipedia.org/wiki/Prime_number_theorem)), you could’ve imagined the deviations being much larger.

David Hilbert famously said, “If I were to awaken after having slept for a thousand years, my first question would be: Has the Riemann Hypothesis been proven?” Meanwhile G. H. Hardy, on a ship that he feared might sink, sent his colleagues a telegram claiming to have proven RH, so that if the worst came to pass, at least the world might believe that a proof sank with him.

Many will of course question whether the proof announced today will stand. They’ll point out that, despite how heavily the world is now “lean-ing” on it, the Lean formal verification system still has issues, from kernel bugs (which have, in the past, allowed proofs of 0=1) to uncertain soundness of the underlying type theory. In my view, these issues can and should urgently be fixed! Until they are, a sufficiently motivated AI could almost certainly exploit them to prove a false statement!

All the same, I see zero indications that that’s happening right now. I note that, so far, not one of the AI results with Lean certificates has needed to be retracted (while several without Lean certificates *have* needed to be). So I’m not 100% sure, but my bet is that the proof will stand.

Mathematicians of course won’t be satisfied with 17 million lines of Lean.  They’ll be satisfied when, and only when, they’re able to explain and distill and reconstruct the ideas for themselves.  And that will probably take years, since human mathematicians move only at biological speeds.  And while they’re working on it, how many *more* such wonders will there be, in this age of wonders and terrors?

My 9-year-old son, informed that RH was proved, replied that he’s not even surprised at this point. Maybe you need to have come of age in the human math era to be surprised by such things.

The P=?NP problem now moves firmly into place as the Final Boss of the Math We’ve Cared About, the position I always thought it should occupy. But for how much longer? For how much longer will *anything* last that we currently take for granted?

* * *

It was only a couple days ago that my wife and I had the following conversation:

**Dana:** I turned off the irrigation, yet somehow water in our house is still leaking. We need to…

**Me:** Look, a lot of the smartest people I know now think that recursive self-improvement and the end of the entire known world are coming in 12-18 months. This leak…

**Dana:** We should still fix the leak.

 This entry was posted on Friday, November 20th, 2026 at 12:29 pm and is filed under [AI](https://scottaaronson.blog/?cat=48), [The Fate of Humanity](https://scottaaronson.blog/?cat=8). You can follow any responses to this entry through the [RSS 2.0](https://scottaaronson.blog/?feed=rss2&p=10344) feed. You can [leave a response](https://scottaaronson.blog/?p=10344#respond), or [trackback](https://scottaaronson.blog/wp-trackback.php?p=10344) from your own site.

You can use rich HTML in comments! You can also use basic TeX, by enclosing it within $$ $$ for displayed equations or \\( \\) for inline equations.

After two decades of mostly-open comments, in July 2024 *Shtetl-Optimized* transitioned to the following policy:

All comments are treated, by default, as personal missives to me, Scott Aaronson---with no expectation either that they'll appear on the blog or that I'll reply to them.

At my leisure and discretion, and in consultation with the [*Shtetl-Optimized* Committee of Guardians](https://scottaaronson.blog/?p=6576), I'll put on the blog a curated selection of comments that I judge to be particularly interesting or to move the topic forward, and I'll do my best to answer those. But it will be more like Letters to the Editor. Anyone who feels unjustly censored is welcome to the rest of the Internet.
