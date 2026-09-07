---
title: Is mathematics about to enter the conservatory?
link: https://mbmccoy.dev/posts/mathematical-conservatory/
source: hnrss-org-frontpage
published: 2026-09-06T23:02:11Z
updated: 2026-09-06T23:02:11Z
first_seen: 2026-09-07T17:03:44.343711062Z
authors:
- _alternator_
summary: 'Article URL: https://mbmccoy.dev/posts/mathematical-conservatory/ Comments URL: https://news.ycombinator.com/item?id=49591793 Points: 102 # Comments: 120'
content: extracted
html: 2026-09-06-is-mathematics-about-to-enter-the-conservatory.html
preview:
  file: 2026-09-06-is-mathematics-about-to-enter-the-conservatory.preview-a20e79686d5e.webp
  width: 256
  height: 171
  color: '#5e4130'
images:
- source: https://mbmccoy.dev/images/DSC02430.jpg
  original:
    file: 2026-09-06-is-mathematics-about-to-enter-the-conservatory.image-22eeb542f9e2.jpg
    width: 2048
    height: 1365
  color: '#070707'
---

The same week that Claude finished [formalizing the proof of Fermat’s Last Theorem in Lean](https://www.anthropic.com/research/formalizing-fermats-last-theorem), a paper landed in my inbox titled, [The Spherical Hadwiger Theorem](https://arxiv.org/pdf/2608.27305). The Spherical Hadwiger *Conjecture*[1](https://mbmccoy.dev/posts/mathematical-conservatory/#fn:1), which has been open since about 1974, describes a niche-but-important piece of integral-geometric machinery. I’m not going to get into the details of the conjecture here; if you are interested you can see a discussion in [my previous post](https://mbmccoy.dev/posts/tqc-lemma) where the theorem (then still a conjecture [2](https://mbmccoy.dev/posts/mathematical-conservatory/#fn:2)) greatly simplifies the proof of a little lemma of mine from grad school.

But to the point: this new preprint by Wang & Wu of Hunan University apparently proves the conjecture using AI assistance. The final section contains the disclaimer:

> During the preparation of this manuscript, OpenAI Codex was used to assist with developing proof details, identifying gaps and points requiring clarification, organizing and typesetting the manuscript, and editing the English. The authors reviewed and verified all AI-assisted mathematical content and suggested changes, made all final mathematical and editorial decisions, and take full responsibility for the manuscript.

This disclaimer leaves open the possibility that Codex did a substantial portion of the work that, until very recently, required a research-level mathematician: developing proof details [3](https://mbmccoy.dev/posts/mathematical-conservatory/#fn:3), finding and fixing gaps, and apparently writing the paper. Moreover, the work is *very polished* and readable (if you are a research mathematician in this field).

To be clear, I haven’t fully verified the proof; I worked through it with Claude Fable and it passes the sniff test, but fully digesting it will take a bit more energy than I have right now. None of this is a knock on Wang & Wu—this seems to be a great paper, and is worth digesting. They’ve even followed all the principles for AI use laid out in the [Leiden Declaration](https://leidendeclaration.ai/).

## A milestone, close to home

For me, the proof of the Spherical Hadwiger Theorem hits home. I tried to prove it in grad school, and made a half-hearted attempt again with AI assistance earlier this year. It’s not a headline-grabbing theorem. That didn’t save it.

I shouldn’t have been surprised. When GPT-4 launched, OpenAI released a [report](https://arxiv.org/pdf/2303.10130) on the potential labor impact of LLMs. The exposure of the work of mathematicians to disruptions from AI was the *highest* of any category they modeled; the whitepaper estimated that 100% of a mathematician’s job was exposed to LLMs, across three distinct labor models. Higher than writers, translators, artists, and graphic designers. The only difference is that it took a bit longer for mathematicians to begin to feel the pain.

It’s tempting, if somewhat arrogant, to claim that this delay in LLM dominance in mathematics arose because research-level mathematics is among the most challenging human endeavors. I suspect the delay owes as much to research mathematics having less economic value—and less training data—than these other creative domains.

Consider classical music. Our society does not support classical musicians in the same way that we support ‘popular’ musicians. Classical music has been institutionalized, sent to the conservatory as a relic. A small segment of society has decided the ability to perform it is worth preserving, and devotes a sliver of capital to that end: training young people, and paying a few of the best players in the biggest cities to do it professionally.

Could this model work for mathematicians? It’s easy to imagine: in Euclid’s time, mathematics largely existed as an intellectual pursuit worthy of a few inclined people. A mathematical conservatory could help math flourish even when it is no longer hard to create new results, just hard to understand and communicate their import.

But pure mathematics is *already* in a conservatory, better known as *the academy*. Outside of the university, the jobs for pure mathematicians remain slim. The results are only understood by a select few. Does it even matter that the hard results are all going to be proved by computers soon?

## The bottom line

It’s worth supporting people to continue the cultural endeavor that we currently call “research mathematics.” Mathematics, especially pure mathematics, has always been about communicating stories that help us understand reality more deeply. By simplifying and abstracting, we begin to see the hidden structure: parallel lines never cross, the sphere looks the same in every direction, the primes never end. But the current support systems for mathematicians, like so many other human creative fields, need to change drastically to handle the new realities of AI.

But the incentive structures that support this work, like those in so many other creative fields, are ill-suited to what AI is bringing. These tools make it harder to tell whether a complex argument is even correct, much less who deserves funding, tenure, and fame. The choice before us is *how do we continue to make humans matter* when their intellectual labors simply don’t compare to computers. When intelligence is limited only by silicon and electricity, will we be willing to continue to support a culture that has mathematicians? I sure hope so.

Get new posts by email. No spam, just posts.

* * *

1. See Problem 3 in [Glasauer’s thesis](https://www.tha.de/~glasauer/publ/diss.pdf). [↩︎](https://mbmccoy.dev/posts/mathematical-conservatory/#fnref:1)

2. In fact, it was only after I wrote the initial blog post that [Prof. Rolf Schneider](https://en.wikipedia.org/wiki/Rolf_Schneider) reminded me that the conjecture was still open at the time. [↩︎](https://mbmccoy.dev/posts/mathematical-conservatory/#fnref:2)

3. *Which* proof details, I wonder? I’d love to see the full chat transcript. While the proof follows the rough approach in Klain and Rota for the Euclidean theorem, the details require overcoming several major obstructions, and I’m curious how many of them were identified and resolved by GPT 5.6. [↩︎](https://mbmccoy.dev/posts/mathematical-conservatory/#fnref:3)
