---
title: 'Fragments: June 2'
link: https://martinfowler.com/fragments/2026-06-02.html
source: martinfowler-com
published: 2026-06-02T15:21:00Z
updated: 2026-06-02T15:21:00Z
first_seen: 2026-09-04T12:14:41.802665207Z
authors:
- Martin Fowler
content: extracted
html: 2026-06-02-fragments-june-2.html
---

Greg Wilson has noticed that lots of folks are [using dodgy metrics](https://third-bit.com/2026/05/20/twelve-ways-to-be-wrong/) to figure out if AI tools are worth their costs.

> Would you measure lines of code generated, or tickets closed? Or would you send out a survey asking whether developers feel more productive? Each of those approaches is flawed in a different way;

He lists lots of common metrics, and why they are flawed. Sadly he doesn’t give any suggestions on what would be better. In my view, since we [cannot measure productivity](https://martinfowler.com/bliki/CannotMeasureProductivity.html), any metrics are weak evidence at the best of times.

I do somewhat use one of his flawed measures: “Asking Developers If They Feel More Productive”. While I acknowledge the problems he gives with this measure, I find that in an environment where decent measures are hard to find, even such a dim light is the best we have. In this situation these kinds of qualitative metrics may not be conclusive, [but they are useful](https://martinfowler.com/articles/measuring-developer-productivity-humans.html).

 ❄                ❄                ❄                ❄                ❄

Benedict Evans observes that [extensive automation didn’t mean the demise of professions](https://www.ben-evans.com/benedictevans/2026/5/24/ai-job-exposure) in the past.

> we spent a century automating accounting: we built calculating machines, punch cards, mainframes, data processing, databases, PCs, spreadsheets, ERPs, cloud… in fact, we built half of the tech industry around automating this. Yet the number of accountants kept going up.

He goes into the myriad of problems that exist when we’re trying to forecast the impact of a technology on jobs. There’s the much-talked-about [Jevons paradox](https://en.wikipedia.org/wiki/Jevons_paradox) - once something becomes cheaper, people do it more, which can increase demand. Often this leads to the nature of jobs changing, even if it’s called the same thing.

> Accountants today aren’t doing exactly the same work that they did in 1970 or 1980 ‘but more’ - they’re still called ‘accountants’ but the job is different. New technology often starts out being used for ‘the old thing but more’, but it rarely ends up like that.

Technologies often affect whole businesses - consider the impact of the internet on news publishing. Did anyone observing the rise of smart phones in the early 2000s realize that a consequence of this would change the economics of taxis due to the rise of ride-sharing apps? The conclusion is that it is, at the very least, almost impossible to forecast the impact of AI on our work.

 ❄                ❄                ❄                ❄                ❄

Stephen O’Grady looks at how [closed and open models have performed on benchmarks over time](https://redmonk.com/sogrady/2026/05/15/open-ai-models/).

> Closed models are setting the pace of innovation, and constantly breaking new ground from a capabilities standpoint. Open models are chasing them, and the cycle times seem to be getting shorter. There are no clear capability moats, and what is frontier today is table stakes tomorrow.

It tooks 13-18 months for open models to catch up to GPT-4 on these benchmarks, but only 2-7 months to catch up to GPT-4o.

There’s a bunch of caveats to this analysis, that he lists, but it’s a worthwhile survey of how various kinds of models perform against the various measures we are trying to assess them with.

 ❄                ❄                ❄                ❄                ❄

One of the starkest examples of sloppy AI use is hallucinated citations - a give-away of both usage of LLMs and carelessness driving them. GPTZero is a company that makes tools to detect AI writing. I’ve no insight as to whether their tool is effective or not, but they do publish investigations of AI usage, and have published several articles highlighting hallucinated citations. [One post focuses on Ernst & Young Canada’s report](https://gptzero.me/investigations/ey) on cyber threats to loyalty systems and found that more than half its references were hallucinations. The post uses a lot of extremely annoying animations in how it presents its information (breaking Safari’s reader mode in the process). But the harm that these kind of AI generated reports can do goes further than just some misled humans:

> Publishing a report online is essentially a form of data injection into the pool of knowledge that is the internet. When the report includes fake information (either vibed citations or false claims) it can “poison the well” by misleading future researchers, especially if the report is published by a well-known consulting firm and hosted on a high-traffic website.

 ❄                ❄                ❄                ❄                ❄

As LLMs get more capable in programming, we are rightly worried that people will use them attack software systems. But these models can also be used for defense, allowing teams to find bugs before attackers do. Some folks from Mozilla posted an article on how they’ve used AI model to [identify and fix an unprecedented number of latent security bugs in Firefox](https://hacks.mozilla.org/2026/05/behind-the-scenes-hardening-firefox/).

> Just a few months ago, AI-generated security bug reports to open source projects were mostly known for being unwanted slop. Dealing with reports that look plausibly correct but are wrong imposes an asymmetric cost on project maintainers: it’s cheap and easy to prompt an LLM to find a “problem” in code, but slow and expensive to respond to it.
>
> It is difficult to overstate how much this dynamic changed for us over a few short months. This was due to a combination of two main factors. First, the models got a lot more capable. Second, we dramatically improved our techniques for harnessing these models — steering them, scaling them, and stacking them to generate large amounts of signal and filter out the noise.

During 2025, there were 17-31 security bugs fixed each month. In April 2026, they fixed 423.

 ❄                ❄                ❄                ❄                ❄

[Pavel Voronin riffs](https://pavelvoronin.com/technical-debt-is-a-prompt-now/) on Unmesh Joshi’s post on [What is Code](https://martinfowler.com/articles/what-is-code.html). He observes that cruft in a codebase (technical debt) has always added friction to software development. But the consequences of this cruft are compounded when LLMs are using existing code as context for future work.

> In a degraded codebase, the model does not see “technical debt” as debt. It sees examples. It sees precedent. It sees a style to continue.

LLMs multiply what’s currently happening. I hear reports that good code might take the place of much of what’s put in markdown, because LLMs will imitate what’s already in the code base. But bad code multiplies too. Inevitably he introduces another variation of rampant debt metaphors:

> Cognitive debt accumulates when a team uses abstractions it no longer understands. Generative debt accumulates when a codebase contains confused concepts that models are likely to continue.
>
> Cognitive debt is about what the team no longer understands. Generative debt is about what the model is now likely to reproduce.

 ❄                ❄                ❄                ❄                ❄

Jason Koebler, from the very worthwhile 404 media, has written a plaintive essay on how [AI-generated slop is driving us crazy](https://www.404media.co/your-ai-use-is-breaking-my-brain/). Not just because its filling the web with this slop, but also because how it’s making us humans react to slop and the threat of slop. We review our own writing and notice: it’s not just reading AI slop that hurts us, it’s the risk that we write something that looks like AI slop. If I use phrasing that AI copied from me, does it seem like I’m copying AI? This has led to the appearance of “humanizers” - AI tools that make our writing look less like AI.

> Humanizers add typos, randomly replaces words, removes “AI tells,” and sometimes inserts random characters.

It’s another step on the way to the Zombie internet:

> I called it the Zombie Internet because the truth is that large parts of the internet are not just bots talking to bots or bots talking to people. It’s people talking to bots, people talking to people, people creating “AI agents” and then instructing them to interact with people. \[…\] It’s my email inbox, in which I used to occasionally get poorly-formatted, poorly written, extremely long emails from delusional people who were positive the CIA had imprisoned them in a virtual torture chamber using undisclosed secret technology but where I now get well-formatted, passably written, extremely long emails from delusional people who are positive they have proven AI sentience and have the AI transcripts to prove it.

 ❄                ❄                ❄                ❄                ❄

Andy Osmani points out that spawning lots of agents is like launching a bunch of parallel processes that all rely on a [single orchestrating thread - yourself](https://x.com/addyosmani/status/2059844244907696186).

> Python has the Global Interpreter Lock (GIL). You can spawn as many threads as you want but only one executes python bytecode at a time because they must acquire the lock. You are the GIL of your AI agents. They all can run at once. But when any of their work needs genuine understanding of the architecture or resolving merge conflicts, that work has to acquire the lock. There is one lock. You hold it.

This means you must design the workflow with the agents with that GIL in mind. You shouldn’t launch more agents than you can properly review. It’s handy to separate background tasks that can be offloaded to an agent from complex tasks that require applied attention. Don’t use that precious brain for things that the machine can verify itself. \[And I’d add - do get the machine to build tools that ease human verification. For example, it’s better to surface test case data in tables rather than buried in assert statements.\]

> Spawning agents is not the skill. Anyone can run 20.
>
> The real skill is designing the system around the one serial resource that cannot be cloned or parallelized. That resource is your attention.

 ❄                ❄                ❄                ❄                ❄

Jamie Hurst is a Principal Engineer at booking.com, where he works in developer experience with a focus on AI tooling. He’s written realistically about [the gains and losses of using LLMs](https://jamiehurst.co.uk/2026-05-24_ai-sustainable) in this work.

> The cost of building has collapsed, but the cost of aligning organisationally has not. If anything, it’s gone up. When three different teams can each produce a working solution to the same problem in the time it used to take to write a proposal, the bottleneck moves from engineering to coordination.

He thinks he’s able to do more as a senior engineer, but is concerned about how sustainable it is, both for him personally and for the organization he works for. He’s able to shape directions for multiple workstreams at once, in a way that he couldn’t three years ago. But one loss is that he doesn’t have enough time for mentoring, which will exact a toll on his employer in the longer term. He also finds he doesn’t have enough time to think.

> The productivity gains from AI got captured by output volume rather than output quality. The org’s expectations rose to absorb the speed-up, and the slack that used to exist between tasks, the unstructured time where strategic thinking actually happens, got eaten first because it’s invisible on a dashboard. I’m at a point in my career where thinking is supposed to be most of the job, and most of it now happens on holiday because the working week doesn’t accommodate it.
