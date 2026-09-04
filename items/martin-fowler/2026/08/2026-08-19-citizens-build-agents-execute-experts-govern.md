---
title: Citizens Build, Agents Execute, Experts Govern
link: https://martinfowler.com/rachels-ramblings/citizens-agents-experts.html
source: martin-fowler
published: 2026-08-19T18:30:00Z
updated: 2026-08-19T18:30:00Z
first_seen: 2026-09-04T09:21:49.209340827Z
authors:
- Rachel Laycock
labels:
- rachels-ramblings
content: extracted
html: 2026-08-19-citizens-build-agents-execute-experts-govern.html
---

**TL;DR**\
*Why building an app over the weekend isn't the same as building enterprise software*

I’ve noticed an interesting gap opening up over the last six months. It isn’t really a gap in technology. It’s a gap in what different people think software engineering actually is.

The conversation usually starts the same way. A non-techie, maybe an executive, tells me about something they’ve built over the weekend. Sometimes it’s a chatbot. Sometimes it’s an internal workflow. Sometimes it’s a surprisingly polished application that solves a real business problem. They’re excited, and they should be. Twelve months ago they probably couldn’t have built it at all. Then comes the question.

“If AI can do this now, why aren’t our engineering teams delivering ten times faster?”

It’s a perfectly reasonable question, after all we’ve all seen the demos. The first thing that would come to my head is “you don’t know what it takes to build enterprise grade software”. But then I think about what I mean and how to explain it to a non-technical person without sounding super patronising. And then it hit me, we did this to ourselves. We’ve spent so many years banging on about how to write good software that everyone has assumed writing software is the same as software engineering.

The application someone builds over the weekend is real software. It likely solves a real problem or demonstrates an idea. Sometimes it’s genuinely impressive. I don’t want to diminish that because I think one of the most exciting things AI has done is dramatically increase the number of people who can turn ideas into working software. That’s cool, I totally get it. The first apps and “hello worlds” I ever built excited me enough to choose this as an actual career so the excitement is real and I don’t want to temper it too much.

But your first hello world, which these days can be an entire app with all kinds of features, is very, very (extra very on purpose) different from introducing software into a production environment in a highly regulated enterprise, as an example. But why?

The moment that application becomes something the business depends on, the questions change completely. Is customer data protected? What happens when a dependency fails? Can someone else understand this system in two years’ time? Will it survive an audit? Can it cope with a thousand times more users than it has today, what about millions in one day? How will we know something is wrong before our customers do? Those questions don’t show up in a demo or in the build phase at all unless an experienced engineer is in the room. I certainly wasn’t asking them when I was building my first apps. I only cared about features!

This is where experienced engineers become more important, not less. Not because they’re the only people who can build the software anymore, but because they have the judgement to know whether we can trust it: whether the design is good, the risks are understood, and the thing that works today won’t become somebody else’s nightmare six months from now.

[At FOSE](https://martinfowler.com/bliki/FutureOfSoftwareDevelopment.html) a few weeks ago, we spent surprisingly little time talking about coding. We talked about whether code was still the source of truth, and occasionally about how much we missed writing it, but mostly we talked about design, architecture, governance, learning and judgement. One team described spending the day designing a specification, letting agents work overnight and reviewing the results the next morning. The interesting bit for me wasn’t the overnight pipeline, cool as that was. It was what the humans were doing: deciding what good looked like, making trade-offs and judging whether what came back was actually what they wanted. We also kept coming back to good design, because it turns out that when agents can generate lots of code very quickly, good design matters more, not less.

That made me wonder whether we’ve been thinking about scarcity in the wrong way. We’ve spent decades optimising around people who can write code because they were scarce and expensive. I’m not convinced that was ever the real scarcity, but that’s probably another ramble. What feels scarce now is good engineering judgement: knowing what good looks like, understanding the risks and knowing when something that works is actually safe to trust in production. Because software doesn’t exist to be built. It exists to run in production and safely solve the problem it was created for. **Organisations don’t run on code. They run on trust.**

A few months ago I found myself saying something in a conversation almost without thinking.

**Citizens build. Agents execute. Experts govern.**

It sounded cool and I thought marketing would like it, so I wrote it down. Then I left it alone for a while. The funny thing about writing these ramblings is that I don’t know whether I believe something until I’ve let it bounce around in my head for a while and also said it to other people I trust like senior engineers at Thoughtworks. Sometimes I come back convinced I was talking nonsense. Occasionally I realise there was something more interesting hiding underneath. This was one of those occasions where the latter was true.

At first I thought I was talking about roles. Citizens build software (essentially non-engineers). Agents write the code. Engineers become governors. But I don’t actually think that’s what I meant. I think I was talking about where value is moving. AI has given everyone a new way to express their ideas. The execution is increasingly handled by agents. They write the code, refactor it, generate tests, fix bugs and iterate at a speed that simply wasn’t possible before. But neither of those things reduces the need for expertise.

In fact, I think it does exactly the opposite. When everyone can create software, somebody still has to decide whether that software deserves to exist inside an enterprise system in PRODUCTION. Somebody still has to think about architecture. Security. Resilience. Operability. Compliance. Cost. The boring stuff that nobody gets excited about in a demo but that becomes painfully important the first time a customer can’t log in or an auditor comes knocking.

That’s why I don’t think experienced engineers become less important. I think they become dramatically more leveraged. Their job shifts from building every feature themselves to creating the environment in which thousands of features can be built safely by other people and by agents. They become the people who design the guardrails, the platforms, the engineering practices and the feedback loops that allow everyone else to move quickly without creating chaos.

Perhaps that’s the future software organisation. Not one where everyone becomes a software engineer. Not one where software engineers disappear. One where almost anyone can create software, agents increasingly execute it, and engineering expertise becomes the thing that allows all of that creativity to scale safely. And to be clear I do not mean people build stuff and throw it to engineers to fix, that is a total antipattern for another ramble.

Perhaps that’s why the executives and engineers I’ve been speaking to sometimes sound as though they’re describing completely different futures. The executive sees that anyone can now build software. The engineer sees that somebody still has to live with it. Both are right. They’re simply looking at different parts of the same system we have to solve to create whatever the future actually ends up being.
