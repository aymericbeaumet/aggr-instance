---
title: 'Fragments: June 16'
link: https://martinfowler.com/fragments/2026-06-16.html
source: martinfowler-com
published: 2026-06-16T12:44:00Z
updated: 2026-06-16T12:44:00Z
first_seen: 2026-09-04T12:14:41.802665207Z
authors:
- Martin Fowler
content: extracted
html: 2026-06-16-fragments-june-16.html
---

“Prag Dave” Thomas (co-author of the outstanding “Pragmatic Programmer”) [has loved programming](https://articles.pragdave.me/p/castles-in-the-air?triedRedirect=true) since he was young.

> Programming was how I could express myself. I wasn’t an artist. When I sing, dogs howl. When I draw, friends say, “Very nice. What is it?” I didn’t connect particularly well with people, even though I wanted to. And yet, when I wrote my first program, I discovered a medium which let me convert thought into action. All the ideas that were bottled up behind a wall of frustration suddenly had an outlet.

The LLM revolution worried him. Would they remove all that fun stuff? Happily he found it was the opposite. Like Kent Beck and others have told me, programming with LLMs is more fun than ever. His post lists reasons why: removing drudgery, speeding up feedback loops, reviving long abandoned projects, and exploring new technologies.

 ❄                ❄                ❄                ❄                ❄

I’ve spent a few days at the [DDD Europe](https://2026.dddeurope.com) conference, which was a very enjoyable event. With all the changes to programming due to LLMs, I suspect [Domain-Driven Design](https://martinfowler.com/bliki/DomainDrivenDesign.html) is going to be one of those things that will continue to be useful, indeed may become even more important. The highlight of the conference was the opening keynote by Eric Evans, who gave a fascinating description of some of his experimentation with LLMs over the last couple of years. Once the video for that talk becomes available, I’ll link to it - hopefully it won’t be too long. I also particularly enjoyed talks from Violetta Pidvolotska, Kiran Prakash, Tom de Wolf, and Chelsea Troy. Gien Verschatse interviewed Eric Evans and me for an hour so so - again I’ll pass that link on when the video gets published.

One snippet that stood out to me was from [Chelsea Troy](https://chelseatroy.com). The main thrust of her talk was managing the context window of LLMs so that it was kept in a healthy state. Much of what she said was familiar, but one thing I hadn’t thought about was her thoughts about the **different registers of conversations** with LLMs. These registers are different styles of conversation we can have with The Genie (or indeed other humans). She classified them in four ways:

- Exploring: I want to understand before touching anything
- Brainstorming: Generate options, I’ll evaluate them separately
- Deciding: I need a recommendation with a rationale, not a list
- Implementing: The decision is made, help me build it

Her point is that whenever I have a session with an LLM, I need to be conscious about which register I’m using. And should I change register, I should start a new conversation with a fresh context.

 ❄                ❄                ❄                ❄                ❄

Charity Majors raises an alarm about [the crevasse forming between AI enthusiasts and AI skeptics](https://charitydotwtf.substack.com/p/ai-enthusiasts-are-in-a-race-against).

> The enthusiasts are not wrong. We are starting to see real, non-imaginary, discontinuous leaps in capabilities from teams that lean in hard to working with AI. And this does not feel like a normal technology cycle where you can wait for the dust to settle; teams that sit this out while competitors are hustling could be out of business before the dust settles. That’s a real, existential threat.
>
> The skeptics are also not wrong. When you ship code faster than engineers can read it, in domains where nobody has full context, you are making withdrawals from a trust account that took years to build. Reliability degrades, institutional knowledge evaporates. You end up with systems nobody understands, products burbling into incoherence, and on-call rotations that grind people up and spit them out. That is ALSO a real existential threat.

The heart of the problem is that there is no common feedback connecting these two groups. She posts some concrete ways forward. Enthusiasts tend to talk most publicly, and focus on the wins, but they need to tell the whole story, highlighting the costs too. We all need to look at working with AI as an engineering problem. Don’t just assume that we can or can’t ship code to production without review: instead ask what would make it possible to reduce review effort. She asserts that engineering discipline is becoming more important, echoing others who’ve observed that AI is an amplifier of our current practices. She wants us to avoid getting into arguments with maximalist stances and speculation, but stick to the reality we observe. Authority comes from engagement:

> The engineers who shape how AI gets used will be the ones with credibility: they understand the opportunity, the stakes, and the tradeoffs, and they own enough of the consequences to have standing when they push back. Earning that position takes work, but it is work worth doing.

 ❄                ❄                ❄                ❄                ❄

[Simon Willison noticed](https://simonwillison.net/2026/May/27/product-market-fit/) that Anthropic and Open AI have increased their enterprise pricing:

> Why these sudden aggressive moves on pricing? Both Anthropic and OpenAI are planning to IPO, but I suspect there’s a more important factor here: I think they’ve finally found product-market fit, with the coding/general-purpose agent products embodied by Claude Code/Cowork and Codex.

His view is that since the [November Inflection](https://martinfowler.com/bliki/NovemberInflection.html) there’s increasing evidence that LLMs are making a big difference to programming, and this is translating to a viable business model for these companies. He suspects there’s another inflection point that happened in April:

> I think April 2026 is a new inflection point where the revenue implications of this have started to land, to the benefit of the frontier AI labs and with material impacts on the budgets of large companies.

 ❄                ❄                ❄                ❄                ❄

Mike Masnick looks back at the [internet’s failed promise of decentralization](https://www.liberalism.org/p/enshittification-despotification-and-the-open-internet).

> The early-internet conventional wisdom was that the network would kill the middleman. Yet the promised era of “disintermediation” never quite happened. What actually happened was that the middlemen changed character.
>
> The pre-internet middlemen—record labels, book publishers, movie studios, magazine editors—were gatekeepers. Their entire job was rejecting 99% of what came through the door so that a curated few could reach an audience. The internet middlemen who replaced them did the opposite. Their approach depended on letting nearly everything through. They became enablers rather than gatekeepers, and an entire generation of cultural production—music, video, writing, podcasts—flowed through them in volumes the old gatekeepers couldn’t have imagined.
>
> But the new middlemen still needed a business model.

That business model came from realizing that when content is bountiful, the new scarce resource is attention. The enablers built advertising machines to sell attention, and focused attention with algorithmic curation. Seeking to increase profits, the enablers rode down the road of [enshittification](https://www.wired.com/story/tiktok-platforms-cory-doctorow/?utm_campaign=enshittification-despotification-and-the-open-internet&utm_medium=referral&utm_source=www.liberalism.org):

> The companies that embraced centralized control over the user experience did so initially because they were, in fact, making things better for their users. Information overload was a real issue. Having a better system for managing it was a good thing. It’s why so many people flocked to these centralized social media platforms and became so enraptured by their algorithms.
>
> The problem of centralized systems is that they create an irresistible temptation to control and exploit. Users who found value early on feel stuck: they can leave, but doing so means abandoning their community. That lack of easy exit creates lock-in, and lock-in enables enshittification.

Masnick’s view is that we have to fight this process of digital despotism, realizing that decentralization, despite it’s ergonomic problems, is worth striving for as it’s the key to cognitive liberty. The key to achieving this is the user’s control over their data with the ability to easily move from one service to another. Ease of exit promotes competition, and competition is what pushes back against the ills of despotic centralization.

> Truly decentralized tools push power to the ends. Users control their data and choose which intermediaries operate on it—and that arrangement is a poison pill to both enshittification and despotification. Any move in those directions only pushes users across the deliberately low barrier to exit, taking their content, data, and community with them.

The lessons of the past are important to guide our AI-enabled future. We need to create an environment that encourages decentralization and competition.
