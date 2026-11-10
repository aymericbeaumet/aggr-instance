---
title: 'Fragments: November 10'
link: https://martinfowler.com/fragments/2026-11-10.html
source: martinfowler-com
published: 2026-11-10T14:30:00Z
updated: 2026-11-10T14:30:00Z
first_seen: 2026-11-10T19:38:28.320395710Z
authors:
- Martin Fowler
content: extracted
html: 2026-11-10-fragments-november-10.html
remote_preview:
  url: https://martinfowler.com/fragments/fragments-thumb.png
---

It’s that time of the year when the [November Thoughtworks Technology Radar](https://www.thoughtworks.com/radar) appears. Again my colleagues have taken a sweep of the technologies that our teams have used in the field, and pulled together a summary of what’s popped up in the last six months. It’s been a busy half-year, one that again has been dominated by AI, and that’s reflected in the many AI blips on the radar.

The mass of blips makes it hard to develop a coherent story around trends, but Chris Ford has done a fine job of the writing the [accompanying article on macro trends](https://www.thoughtworks.com/insights/blog/technology-strategy/macro-trends-tech-industry-november-2026).

> One team of passionate people experimenting with agents can make almost anything work. They share a context, a credit card and a mental model. They review each other’s work because they sit together, and they know what to check and how much latitude to give the coding agent.
>
> Two hundred people, though, cannot work that way. The moment agentic AI becomes an operating model rather than a pilot, a second layer of problems appears: How is context shared between teams? What’s the token bill? How do we deal with increased review demands? What’s the agent allowed to reach, read and change? And who’s now responsible for the harness everyone depends on?

Experience so far suggests that the questions that enterprises face are about: shared context, token cost, cognitive load, security, and platform building. While these issues are distinct, they are not separate, so strategies to answer them must take all of them into account. I also echo Chris in reminding us that however impressive these tools are, we must remember that while we have to rethink systems and workflows to utilize these technologies, they should be built to serve humans - and we should all benefit from its bounty.

* * *

Ben Thompson’s Stratechery newsletter [reflects on the recent election’s impact on data centers](https://stratechery.com/).

> I watch a lot of sports, and particularly this fall, watched a lot of baseball. My Milwaukee Brewers — excuse me, my World Champion Milwaukee Brewers — demanded my full attention, which also meant I saw a lot of campaign ads, and by far the most common topic, at least here in Wisconsin, was data centers. Moreover, it was never entirely clear which way it cut politically: both parties were anti-data center, accusing their opponent of being the opposite.

He points out that this opposition is recent, and tied to a shift in focus from the internet and streaming movies to AI:

> you know, the technology that in the best case scenario is going to take everyone’s jobs, and in the worst case scenario end the human race.

There’s going to be a lot said about how Democratic officials will be at the heart of blocking data center building, but as his point above about campaign ads illustrates, the opposition to data centers is (at least at the moment) a bi-partisan opposition.

He follows up with “ten takes” from these consequences. The big beneficiaries of this opposition to building data centers will be those who own the data centers that have already been built. The opposition will reduce supply of tokens, meaning that token costs will go up, thus increasing the profits for those who already have their token generators plugged in and humming.

There’s also benefits to those who aren’t constrained by the need to build more big data centers. Other countries aren’t affected by a change in US policy, China is the most talked-about case here, but it’s not the only country that may benefit from a drop in US data center building. Apple may also benefit, if it means more AI work is pushed to edge devices that are their territory, in addition to having not sunk so many dollars into AI-specific investments.

To his analysis, I’ll add that this may also have a somewhat counter-intuitive effect on regulation, and corporate attitudes to it. The common expectation is that businesses are opposed to government regulation, since that interferes with their free pursuit of profit. That’s true to an extent, but it’s quite common to see established businesses lobby (quietly) in favor of regulation that acts as a barrier to new competitors. So I wouldn’t be at all surprised to see companies that have already built data centers to be pushing for regulations that impede or prevent new ones from being built.

* * *

I’ve been a long-time fan of Hadley Wickham, for how he has tamed the rather messy world of R programming into a [well-designed tidyverse](https://tidyverse.org/). He is another thoughtful voice to listen to as we move into the world of [agentic engineering](https://martinfowler.com/bliki/AgenticEngineering.html), in this case by trying to figure out [what the real cost of tokens is](https://substack.hadley.nz/p/how-much-do-tokens-really-cost).

> I hear a lot of suspicion that AI tokens are unsustainably priced and that frontier labs are heavily subsidising costs with their VC money. They want to get us hooked now, so they can pull the rug out from under us whenever they want to make a bunch of money. Is that true? My sense is that it’s mostly not. But can I back that up with some data?

His conclusion is that it doesn’t seem that token costs are significantly subsidized on per-token API basis, but they are with subscription plans. (He caveats his analysis, saying it doesn’t take into account recouping the costs of training the models.)

* * *

There’s a creepy excitement at the thought that models are clever enough to edit the weights of models, an activity that [Takehi et al refer to as Weight-Space Programming](https://rikiyat.github.io/blog/weight-space-programming/).

> Just by looking at the raw weights, GPT-6 Astra can tell what a network was trained to do. And without any environment feedback, it has no trouble writing, from scratch, small game-playing networks that outplay networks trained with PPO for 100 million steps.

While we don’t know if we’ll ever be able to download skills directly into our brains ([“I know kung fu”](https://www.youtube.com/watch?v=fhrNgXJ__n8) - or for those in my particular demographic: [Joe 90](https://en.wikipedia.org/wiki/Joe_90)), this does raise the possibility of downloading capabilities into models by manipulating their weights.

Simon Wilison riffed on this paper by [examining whether an LLM can understand model behaviors from the weights alone](https://simonwillison.net/2026/Nov/3/qwen-can-interpret-model-weights/), by examining four simple neural networks, each trained to play a specific game, and figuring out which game it was by just looking at the weights.

* * *

> Jessica Kerr (jessitron):
>
> [@jessitron on X](https://x.com/jessitron/status/2117400669044793405)

> My days of teaching AI to do stuff are numbered.
>
> Getting it to teach me stuff is more fun.

* * *

Houck et al (that “al” includes some folks who I’ve learned to pay a lot of attention to), look at how we can assess the quality of context supplied to agents.

> An agent may draw from source code, specifications, issue trackers, chat histories, architectural documents, tool output, and user instructions, then act on that combination with limited human supervision. Information that was written for different audiences, at different times, and for different purposes is brought together into a temporary operating environment for a particular task. Whether each source is individually useful does not reveal whether the resulting context is fit for the work the agent is being asked to do.

They’ve come up with a [framework for describing the quality of this assembled context](https://queue.acm.org/doi/10.1145/3847288), which they refer to as CAFE(S). The acronym refers to five dimensions they look at:

- Clarity: Can the agent interpret the request as intended?
- Actionability: Can the agent proceed, and know when it is done?
- Fidelity: Can the agent trust that this context is true?
- Efficiency: Can the agent focus on what matters?
- Security: Should the agent have this context at all?

They also list a set of half-a-dozen “context smells”: common cases where these break down (and a nod to Kent’s [original metaphor](https://martinfowler.com/bliki/CodeSmell.html))

I’m not sure about the name, (how exactly should I pronounce “CAFE(S)”?) and the article exhibits rather more [LLM Voice](https://martinfowler.com/bliki/LLM-Voice.html) that I prefer. But on the whole I think this is a good framing for thinking about an important part of feeding our silicon servants/overlords.

* * *

Cindy Sridharan noted that [the problems she ran into six months ago are no longer around](https://x.com/copyconstruct/status/2118529738532925725):

> What was missing in my setup 6 months ago was the verification process I now use.
>
> 70% of my daily token spend goes into verifying patches, and only ~20-30% into creating them.

That resonated with conversations I’ve had with folks using agents in enterprise settings. They are using far more tokens to check the working of models (through guardrails and evals) than they are to generate a response to a request. A single request to a model from a customer may trigger a dozen turns to other models to probe its input and verify its output.
