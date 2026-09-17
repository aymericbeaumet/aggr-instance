---
title: Everybody's Lost Their Minds
link: https://www.netmeister.org/blog/everybodys-lost-their-minds.html
source: hnrss-org-frontpage
published: 2026-09-17T19:43:34Z
updated: 2026-09-17T19:43:34Z
first_seen: 2026-09-17T23:03:54.779334643Z
authors:
- ibobev
summary: 'Article URL: https://www.netmeister.org/blog/everybodys-lost-their-minds.html Comments URL: https://news.ycombinator.com/item?id=49745570 Points: 251 # Comments: 167'
content: extracted
html: 2026-09-17-everybody-s-lost-their-minds.html
preview:
  file: 2026-09-17-everybody-s-lost-their-minds.preview-d8c4fbb6a6b1.webp
  width: 256
  height: 137
  color: '#6a615f'
images:
- source: https://www.netmeister.org/blog/images/tired.jpg
  original:
    file: 2026-09-17-everybody-s-lost-their-minds.image-640f202ee734.jpg
    width: 585
    height: 314
  color: '#7a7476'
---

Men. Some would rather vomit up a rambling blog post wall of text that nobody's going to read than go to therapy. So here we are.

I've seen my share of stupid over the years, but now people with no engineering background have started pitching "industry changing" solutions they cooked up in their agent infested homelab; people's emails read like bozotic LinkedIn-fluencer posts with punchy "it's not this, it's that" single-sentence paragraphs; online articles suffer a similar fate in their own convergence on Meh; and half of the people you interact with have turned into [meat proxies](https://sfisms.org/meat-proxy).

Spending upwards of 75% of my time directly or indirectly dealing with AI every day has absolutely robbed me of most of my enjoyment of my work. Most days feel like that Twilight Zone where you wake up and you're the same, but everyone else is different. ([They were all like that.](https://youtu.be/mV8WzFa2COQ))

### "Ethics aside..."

The cyber hype train has been going "choo choo" for a while, with the main AI companies trying to one-up each other [committing crimes](https://www.felonybench.com/) and somehow we let them; the conscious choice of anthropomorphic language by the companies is adapted unquestioned by the media, thereby absolving AI companies of their incompetence to secure their programs.

Built on unapologetic exploitation of intellectual property and concentrating power in the hands of a very small number of US companies and oligarchs, these AI models not only lend themselves to [generation of Child Sexual Abuse Material](https://www.theguardian.com/technology/2026/jan/02/elon-musk-grok-ai-children-photos)—a product *feature* for [logged-in users](https://arstechnica.com/tech-policy/2026/01/musk-still-defending-groks-partial-nudes-as-california-ag-opens-probe/)—[1](https://www.netmeister.org/blog/everybodys-lost-their-minds.html#1)but our continued use of them also directly supports their role in, e.g., military target selection, such as [elementary schools](https://en.wikipedia.org/wiki/2026_Minab_school_attack).

Meanwhile, every single company is happy to "ethics aside..." all of that and spend unimaginable amounts of "tokens"—a made-up currency following the casino model [2](https://www.netmeister.org/blog/everybodys-lost-their-minds.html#2)—while staring at you blankly when you ask whether anybody has bothered to check if that support chatbot you vibe coded and which you fed all of your very mediocre at best "documentation" has *any* ROI.[3](https://www.netmeister.org/blog/everybodys-lost-their-minds.html#3)

### Project Sisyphus

"Frontier Models" and AI-assisted vulnerability research is another topic with questionable results. Anthropic and OpenAI tried to one-up each other with how dangerous their models are and everybody who considers themselves an industry leader is now part of some mysteriously named "project" (like [Glasswing](https://www.anthropic.com/glasswing) and [Daybreak](https://openai.com/daybreak/), or [Athena](https://www.chainguard.dev/athena) and [Akrites](https://akrites.org/)) or co-signed various open letters (like [this](https://openai.com/collective-cyberdefense/) or [this](https://www.microsoft.com/en-us/corporate-responsibility/topics/open-weight/)) to signal just how much they're totally not left out.

Every participant in these projects has thrown absolutely incredible amounts of engineering resources at the FOMO-induced, time-limited, "the first one's free" offer from Anthropic and OpenAI. Dozens of highly-paid security engineers had all of their priorities shifted and spent *all of their time* on this; the cost of the engineering hours spent on developing and adjusting AI vulnerability discovery harnesses, building new processes and pipelines to shoehorn thousands of findings into their vulnerability management processes, and of course working with the product owners on assessing and fixing the findings... all that must run in the many, many millions of dollars for each organization.

And yet, despite having found literally *thousands* of new vulnerabilities (only [a fraction of which were reported to Open Source projects](https://www.vulncheck.com/blog/anthropic-glasswing-receipts), by the way), I don't think that we're any safer than before. That's because *finding* vulnerabilities has never been the bottleneck in information security. The bottleneck isn't even *verifying* a vulnerability report and validating its severity, as time consuming as that is. The bottleneck isn't determining the fix, creating the patch, or publishing a new release. The bottleneck is still, as ever before, *getting the goddamn packages updated*. Patching is still [hard](https://www.netmeister.org/blog/patching-is-hard.html).

Now imagine that we had spent all these resources on doing the basics: ensuring your organization has an up-to-date and comprehensive asset inventory with fine-grained package listings; building infrastructure that supports regular, frequent, and automated OS and applіcation updates; automatically rebooting systems when they hit, say, 30 days of uptime to ensure these updates are picked up; establishing comprehensive attack surface enumeration across all your IP space *as well as* all your cloud providers (what a concept!); the list of basic, fundamental defenses that nobody seems to actually do well goes on. Having a few dozen senior engineers dedicated for 6 months to overhauling all that, focusing on making *patching* easier, would, in my book, have been a much better investment, but that's just not very cyber at all.

No matter what AI promises, human resources are still a zero-sum game, and every individual feeling super busy in their agentic silo doing a thousand things at once does not, in the end, help solve the kinds of projects that require cross-functional collaboration and team work.

### A strange game

At the same time, AI companies are falling over themselves once again facetiously calling for their own regulation because, you know, they could accidentally end all mankind.

If you actually thought your product will kill all humans, then you could, you know, like, just stop building the torment nexus. All by yourself, no government regulations required. Nobody's forcing you to play "Theaterwide Biotoxic and Chemical Warfare" or "Global Thermonuclear War". I mean, except your future shareholders and your greed. Alas, that wouldn't cockblock your competition...

But you don't need to imagine AI destroying all humankind within the next few years via some sort of Skynet or Paperclip Maximizer scenario when in reality AI has of course already been hard at work here. The environmental impact of these companies is [staggering](https://news.bloomberglaw.com/environment-and-energy/us-data-centers-set-to-burn-more-natural-gas-than-most-nations). The AI race demands more and more water wasting, air polluting, fossil fuel powered data centers that absolutely nobody wants to live close to, and governments lift any and all environmental regulations for these companies who not too long ago at least *pretended* to have even the feeblest greenwashing commitments to carbon neutrality or renewable energy sources.

But "[the world looks different now](https://www.nytimes.com/2026/08/08/climate/amazon-data-center-texas-pollution.html)", to which I can only say "No fucking shit, Sherlock. [IT'S ON FUCKING FIRE.](https://static01.nyt.com/images/2026/07/24/multimedia/24int-spain-france-fires-mpkz/24int-spain-france-fires-mpkz-superJumbo.jpg) Because of *you*."

### Superhuman Intelligence

You know there are two ways for AI to achieve superhuman intelligence, right? One (theoretical) way is the mystical "recursive self-improvement" by AI. The other one is the path we're very clearly on: The agentic brain worms have been spreading, and it looks increasingly like everybody's lost their goddamn minds already. AI is the tool that dulls its users; it incrementally replaces *understanding* with a new dependency and addiction as you actively de-skill yourself.

AI helps people find more vulnerabilities in existing code. To address those vulnerabilities, people use AI to generate patches. The resulting pull requests are then "reviewed" by AI. That is, the more AI is in the loop, the less we understand the code base. The mystical "human in the loop" often is nothing more than a [rubber stamp](https://www.netmeister.org/blog/secret-language-of-coders.html#lgtm).

So what happens when things go bump? Complex systems fail in complex ways, and debugging code is an order of magnitude [harder than writing code](https://www.laws-of-software.com/laws/kernighan/). Debugging somebody else's code is harder still. Trying to debug large, complex, distributed systems consisting of components that are effectively opaque to your entire organization is going to be impossible.

So no, I'm not going to set ethics aside and then actively offer myself up as tribute to self-amputate my brain. I'm sorry if everybody else can't get rid of the brain slugs, but at this point I'm just looking to get off this ride.

September 16th, 2026

 P.S.: And no, Claude is [not conscious](https://www.theatlantic.com/philosophy/2026/06/no-artificial-intelligence-is-not-conscious/687378/?gift=qrZWaSGx876y4WTlWSgdyE2DRbvX5US2rO-FfZV-AcQ&). J-Space my ass.

* * *

Footnotes:

 \[1\] Damn straight I use an emdash. Fuck you for devaluing it. [↩](https://www.netmeister.org/blog/everybodys-lost-their-minds.html#b1)

 \[2\] "Results showed that participants gambled significantly more with chips than with real cash." \[[citation provided](https://search.informit.org/doi/abs/10.3316/informit.223149445402224)\] [↩](https://www.netmeister.org/blog/everybodys-lost-their-minds.html#b2)

 \[3\] It doesnt: LLMs are Garbage-In/Garbage-Out—if you have shitty docs, the AI can at best polish that turd and still only spit out nothing of use. [↩](https://www.netmeister.org/blog/everybodys-lost-their-minds.html#b3)

* * *

Links:

- [Patching is hard. Knowing what to patch is harder still](https://www.netmeister.org/blog/patching-is-hard.html)
- [Discussion on HackerNews](https://news.ycombinator.com/item?id=49745570) ([dupe](https://news.ycombinator.com/item?id=49735721))
- [Discussion on Lobsters](https://lobste.rs/s/rok1za/everybody_s_lost_their_minds)
