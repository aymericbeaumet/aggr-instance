---
title: Holy Shit Holy Shit Holy Shit Holy Shit [Algorithmic Complexity Lower Bounds]
link: https://scottaaronson.blog/?p=10379
source: hnrss-org
published: 2026-11-25T19:11:27Z
updated: 2026-11-25T19:11:27Z
first_seen: 2026-11-25T20:07:11.161777435Z
authors:
- Kotlopou
content: extracted
html: 2026-11-25-holy-shit-holy-shit-holy-shit-holy-shit-algorithmic.html
remote_preview:
  url: https://s0.wp.com/_si/?t=eyJpbWciOiJodHRwczpcL1wvc2NvdHRhYXJvbnNvbi5ibG9nXC93cC1jb250ZW50XC91cGxvYWRzXC8yMDIxXC8xMFwvY3JvcHBlZC1KYWNrZXQuZ2lmIiwidHh0IjoiU2h0ZXRsLU9wdGltaXplZCIsInRlbXBsYXRlIjoiZWRnZSIsImZvbnQiOiIiLCJibG9nX2lkIjoxMjk1MjA1ODB9.siOtN7gHw4tefA_rZickBw4GfI6tPxGgOQ1AXr2ZoOQMQ
---

I feel like my previous posts (and post titles), and podcasts, and talks, have failed to do full justice to the magnitude of what’s now happening, so I’m trying again.

Last night, we had friends and colleagues over for dinner. We were still trying to process OpenAI’s and Anthropic’s [proofs of VP ≠ VNP](https://scottaaronson.blog/?p=10362)—hot on the heels, obviously, of the Alpöge/Anthropic [proof of the Riemann Hypothesis](https://scottaaronson.blog/?p=10344), which already feels like last week’s news. But then before dessert was served, we saw that OpenAI claims to prove that [ω, the exponent of matrix multiplication, is exactly 9/4](https://github.com/openai/math/blob/main/preprints/The-matrix-multiplication-exponent-is-nine-fourths-November-24-2026/paper.pdf)—a **lower bound (!!)**, matching the upper bound that OpenAI proved in the great Mathocalypse drop of October 6.

There are two separate shocks here. The first shock is that we now, finally, know the right answer to this central mystery of algorithms going back to the late 1960s, when [Volker Strassen first showed that ω<3](https://en.wikipedia.org/wiki/Strassen_algorithm) (he got ω≤log27) … and that the true answer is actually a *rational* number (!). But the even bigger shock is that the AI was able to *prove* any lower bound of the form ω>2. I never expected this challenge to be all that much easier than P ≠ NP.

Over the decades, I often heard colleagues express extreme confidence that ω=2, and I expressed skepticism whenever this came up—not because I had any strong argument for ω>2, but just because I saw that there was no strong argument for ω=2 either. So, I’m glad to have been vindicated about *that*!

* * *

But wait, there was more. During dessert, we saw [another OpenAI release](https://github.com/openai/math/blob/main/preprints/The-logarithmic-exponent-of-finite-Fourier-transforms-November-24-2026/paper.pdf) proving that the true number of linear gates needed to compute the Fourier Transform is N (log N)1/2+o(1). The upper bound here improves, not only over the O(N log N) that stood since the 1960s (i.e., the Fast Fourier Transform or FFT), but also over the O(N log0.9999999999999 N) (give or take some 9’s) that OpenAI published back in the October 6 Mathocalypse. Again, though, the real shock here is that there’s also a **matching lower bound (!!)**. All these questions from the ancient days are now just getting … straight-up *answered*, rather than slowly illuminated with a drip-drip-drip of clues.

To give an idea of how much things have changed, the other day Yupan Liu put a [preprint on the arXiv proving that QIP(2) = PSPACE](https://arxiv.org/pdf/2611.31236)—that is, quantum interactive protocols with only two messages have the full power of those with three, or any polynomial number. As recently as 2025, this would’ve considered been one of the top complexity results of the year, and the fact that Yupan had used AI to help achieve it would’ve been strange and wonderful. These days, I had to remind myself it had even happened, and the use of AI was so standard that it didn’t even raise an eyebrow.

* * *

It’s now obvious that we’ll be getting, not one-off solutions to this or that complexity question, but a whole new continent of mathematical truth, one that we’d previously only glimpsed through fog. Picture van Leeuwenhoek first training a microscope on pondwater, or Galileo first training his telescope on the stars. That might give you *some* idea of how our resolution into the complexity-theoretic universe, and the whole universe of mathematical truth beyond it, has now improved.

All through last night, as OpenAI and Anthropic uploaded more files to their GitHub repositories—because that’s what they’re doing now, they don’t even bother to issue press releases anymore, they just upload papers—my guests and I sat at the dinner table with phones out and a laptop, wondering which shoe was going to drop next. It was as though the clouds had parted above Mount Sinai, and we were waiting for the next sentence uttered by the Holy One Himself.

I now feel in my bones, for the first time ever, that a proof of P ≠ NP is not far.

* * *

For all I care, despise the AI companies for bulldozing our garden. Or love them for opening a new vista, or anything in between. Just please don’t *deny* what’s happening. Now that the companies have repeatedly shown that they can back up their claims with Lean certificates, don’t be a raging doofus who seizes on some triviality (e.g., a minor discrepancy between the Lean and informal proofs, or unused sections of the Lean proof) to pretend that dozens of great open problems of math and theoretical computer science are not, in fact, getting solved. Because they *are* getting solved. It might take us years to *understand* the solutions, but my working assumption by this point is that any proof that comes directly from OpenAI or Anthropic is correct barring information to the contrary.

I can’t possibly put the matter better than one of my friends on Facebook:

> After years of crackpot “solutions” claiming Millennium Problems are solved, now that we have actual solutions, we have crackpot “refutations” claiming Millennium Problems are not solved. It is really poetic, in a way.

* * *

Along the way to so many lower bounds that we wondered whether ever we’d live to see, there will also, of course, be further shocking *upper* bounds: that is, ultimate barriers to lower bounds, also known as “algorithms.” And yes, some of those algorithms will break currently deployed cryptosystems.

Because people have been hounding me about it: yes, I’m virtually certain, from multiple reliable sources, that the AI labs are now sitting on one or more bombshells in that area. **However,** until I either see details or hear directly from someone at a lab, I feel much, much less confident about the specific *nature* of those bombshells. I do not wish to be responsible for spreading unverified rumors. And I worry that exact statements of what was done may have been mangled in a game of Telephone.

I also, crucially, don’t want the cryptography part of the story to overshadow the intellectual enormity of what’s happening. If, in these days of long-prophesied revelation and wonders and terrors, your main worry is your Bitcoin wallet, then go somewhere else for rumors about whether or not ECDSA is broken or whatever. I don’t have what you’re looking for here.

* * *

Let me be clear: for those of my readers who celebrate, I believe that tomorrow would be a *superb* Thanksgiving to spend some quality time with your kids and parents and siblings and old friends.

Right now—this past week—is very much like I would’ve always imagined the beginning of an AI singularity to look and feel like. I just hope we’ll get lucky, and it will “merely” be a singularity for our little corner of the world that involves hard math problems.

At some point at dinner last night, Dana had a sneezing fit. So I said to her, I hope it isn’t a swarm of nanobots that’s come to infect us! But if it *is*, then I hope it’s the friendly kind, the kind that will usher us all into an AI utopia.

Now, of course, another morning has dawned, and we’re still here. May we find a world where we and our children will be happy to live at the bottom of this rollercoaster. May we work to make it so.

 This entry was posted on Wednesday, November 25th, 2026 at 1:01 pm and is filed under [AI](https://scottaaronson.blog/?cat=48), [Complexity](https://scottaaronson.blog/?cat=5), [The Fate of Humanity](https://scottaaronson.blog/?cat=8). You can follow any responses to this entry through the [RSS 2.0](https://scottaaronson.blog/?feed=rss2&p=10379) feed. You can [leave a response](https://scottaaronson.blog/?p=10379#respond), or [trackback](https://scottaaronson.blog/wp-trackback.php?p=10379) from your own site.

You can use rich HTML in comments! You can also use basic TeX, by enclosing it within $$ $$ for displayed equations or \\( \\) for inline equations.

After two decades of mostly-open comments, in July 2024 *Shtetl-Optimized* transitioned to the following policy:

All comments are treated, by default, as personal missives to me, Scott Aaronson---with no expectation either that they'll appear on the blog or that I'll reply to them.

At my leisure and discretion, and in consultation with the [*Shtetl-Optimized* Committee of Guardians](https://scottaaronson.blog/?p=6576), I'll put on the blog a curated selection of comments that I judge to be particularly interesting or to move the topic forward, and I'll do my best to answer those. But it will be more like Letters to the Editor. Anyone who feels unjustly censored is welcome to the rest of the Internet.
