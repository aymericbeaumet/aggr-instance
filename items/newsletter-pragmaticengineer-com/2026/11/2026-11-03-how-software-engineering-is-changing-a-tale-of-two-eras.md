---
title: 'How software engineering is changing: a tale of two eras'
link: https://newsletter.pragmaticengineer.com/p/how-software-engineering-is-changing
source: newsletter-pragmaticengineer-com
published: 2026-11-03T16:58:22Z
updated: 2026-11-03T16:58:22Z
first_seen: 2026-11-03T20:48:58.593592327Z
authors:
- Gergely Orosz
summary: We invited readers to write about how building software changed, in an essay challenge. Many of you took part, and today we’re sharing the winning piece from a strong set of entries
content: extracted
html: 2026-11-03-how-software-engineering-is-changing-a-tale-of-two-eras.html
remote_preview:
  url: https://substackcdn.com/image/fetch/$s_!WYGs!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F25c874c2-9b02-4a24-9046-879cb1500023_1920x899.png
---

We invited readers to write about how building software changed, in an essay challenge. Many of you took part, and today we’re sharing the winning piece from a strong set of entries

*Before we start: I’m in San Francisco this week and there are a few remaining spots for these live events (registration required):*

- *Wednesday (4 Nov): **[The Pragmatic Engineer Unplugged](https://luma.com/pragmatic-engineer-unplugged)** at the iconic SFJAZZ Center. With Diogo Almeida (creator of Jev & co-inventor of ChatGPT), and Bryan Cantrill (cofounder and CTO of Oxide).*

- *Thursday (5 Nov): **[System Update](https://luma.com/systemupdate)** at The Howard SF. With Karri Saarinen (Linear), Kath Korevec (OpenAI), David Cramer (Sentry) and Adam Rogal (DoorDash)*

*Also, this week there will be no issue of The Pulse on Thursday. Regular scheduling returns next week!*

In September, when The Pragmatic Engineer [turned five](https://newsletter.pragmaticengineer.com/p/the-pragmatic-engineer-five-years), I [announced](https://blog.pragmaticengineer.com/how-software-engineering-is-changing-an-essay-challenge/) an essay challenge, inviting readers to write about how things have changed in tech workplaces and in our projects, for the chance to win a grand prize of $10,000 awarded to our favorite article.

We received 125 submissions, covering a wide range of viewpoints and experiences. *Many thanks to everyone who took part.* In today’s issue, we publish the winning piece in full.

Congratulations to software engineer [Neil Kakkar](https://neilkakkar.com/)! We appreciate the sheer volume of data that Neil has collected about how his own output has changed over time. Neil is a dev based in London, with nine years of professional experience; he has interned at Google, and been a software engineer at Bloomberg, Posthog, and Tano. His previous workplace, PostHog, is an open source project, which must’ve been useful for gathering historic data.

In a follow-up issue, we’ll highlight runner-up essays, and link to the ones that impressed us for the effort on show and interesting thoughts.

*With that, it’s over to Neil. He covers:*

1. **Two eras of shipping**. Until last year, he was merging between eight and 20 pull requests (PRs) per month. But since joining an AI-native startup, Neil now ships 200+ PRs per month, approximately a tenfold increase.

2. **Old constraints that disappeared**. No more communications overhead or code reviews. The tradeoff: moving faster and breaking things.

3. **New constraints that appeared**. Too much context switching (thrashing), losing shared context, pacing yourself is harder, less focus on design, and worries about AI plan limits.

4. **How AI is changing software engineering**. Everyone needs to be onboard with AI to get the benefits at work, encouraging experimentation makes a difference, and many existing technologies and processes crack under the change.

I’ve been working at an AI native startup for the past eight months. Before joining [Tano](https://www.tano.ai/pragmatic), I was at [PostHog](https://posthog.com/) for four years, a fast moving pre-AI startup, and started my career at [Bloomberg](https://www.bloomberg.com/uk), a big fintech private company, where I spent three years. All three workplaces are different: Bloomberg is huge, with 20,000 people and very defined processes. PostHog is smaller, at around 10 people when I joined, and now at 200. At Tano, I was the first engineer and we’re now around 10 people in total.

In each job, I’ve seen what a difference speed and culture make, and more recently, how culture needs to change to keep up with AI.

To back things up, I use in-depth stats about my work at PostHog and Tano. I also have some very rough numbers from Bloomberg. By far, I’ve written more code and made a lot more PRs at Tano than I did at PostHog, and a major part of this is down to AI. Facilitating the shift is engineering culture.

*One disclaimer before we get into it: the PostHog stats are from pre-AI. I’ve been following their journey, and PostHog is as “AI-pilled” as it gets now, and output is increasing significantly. In this article, I treat it as a ‘pre-AI baseline’ for excellent engineering work. Tano is a fast-moving AI-native startup, founded after AI agents emerged.*

Let’s start with some raw numbers. I’m shipping 11x more PRs at Tano than at PostHog, where I shipped 2.4x as many as I did at Bloomberg.

At Bloomberg, I was just starting my career, so I wasn’t the fastest, and moving to PostHog opened my eyes to what a fast-moving startup culture really looks like: we were encouraged to take responsibility, ship things quickly, listen to customers, and solve their issues ASAP.

At Bloomberg and PostHog there was a high bar for code: everything had to be peer reviewed, and as I became senior, I spent a big chunk of my time reviewing code.

[![](https://substackcdn.com/image/fetch/$s_!N8p2!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F094d1c3e-53a2-443d-bc87-5eecbd3e3b5e_1920x566.png)](https://substackcdn.com/image/fetch/$s_!N8p2!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F094d1c3e-53a2-443d-bc87-5eecbd3e3b5e_1920x566.png)

Pull requests merged per month at Bloomberg, PostHog, and Tano

PRs can be an easily gamed metric, but I was not trying to game the numbers, so they’re an honest representation of the work done. Below are details of everything I worked on to justify the 11x PR multiplier.

I joined PostHog as a full stack software engineer and worked across the product, from the ingestion pipeline to optimising our analytics queries for funnels and retention on Clickhouse, scaling it up to handle a billion+ events scale, and eventually building out the experimentation product from scratch. I later led the Feature Success team, where we built out two new products: surveys and early access features, while improving experimentation and feature flags.

I joined Tano as a founding engineer and focused on a similar breadth of product as at PostHog, but all compressed into eight months instead of 45. I built out our new affiliate platform, made our content monitoring system a lot more efficient, and did some big migrations, like moving the entire agent architecture to a new system with persistent memory, better tools, and superior observability.

Here’s a summary of work I did at both companies:

[![](https://substackcdn.com/image/fetch/$s_!JGVx!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5fca7a67-ccd1-4fc3-8e69-bb36e3bc5fa5_2200x2998.png)](https://substackcdn.com/image/fetch/$s_!JGVx!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5fca7a67-ccd1-4fc3-8e69-bb36e3bc5fa5_2200x2998.png)

Things I worked on during four years at PostHog, and one at Tano

At PostHog until late 2024, the extent of AI was tab completion. It was great for automating menial work like repetitive test cases, but was a long way away from today’s autonomous agents that can build an entire feature with no human input.

When I joined Tano this year, Claude Code Desktop was already present with really good agent support, and could build out entire features with little hand-holding. I never got used to the command line interface (CLI), and stuck to Claude Code Desktop. I really like the integrated browser and let Claude control it, rather than with my own browser where it’s very annoying if Claude takes over and blocks me from working. Over the last year, the desktop version has caught up to the CLI in features, and has some, like the browser, that a CLI doesn’t directly have.

**At any one time, I’m working on 3-5 different things.** I start by running through the plan, but mostly let the agent run until it has a PR ready and the comments addressed. Then I run through what it did via a digestible HTML artifact which Claude has instructions to generate with every PR. It looks something like this:

[![](https://substackcdn.com/image/fetch/$s_!012q!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Facd253e3-5e19-4cb0-8dba-a7dda4445cba_2048x1309.png)](https://substackcdn.com/image/fetch/$s_!012q!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Facd253e3-5e19-4cb0-8dba-a7dda4445cba_2048x1309.png)

HTML summary that Claude generates with every PR

**My agents can take 1-2 hours to run**. This is for building a full feature, end to end, and given all of the machinery of reviews, verification, and screenshots, it usually takes my agents that long. The run can go even longer if I’m doing a research task that requires investigating a lot of data, processing of images and videos, or iterating towards a goal. I don’t yet use cloud agents because the native Claude agents seem too limited to run things as you would locally.

Larger companies have invested in building their own cloud agents where these integrations are present. It makes sense for them, but not so much at our scale – just yet.

Eleven times more PRs per month at Tano wouldn’t be possible without AI; I couldn’t write 100k+ lines of code a month by hand, let alone think through that many things. Furthermore, distractions during a normal work day – like new bugs, small feature requests, emergency incidents – break flow and decrease productivity. With AI, these can be passed onto a parallel work thread and not take up focus, front and centre.

**How much does the 11x multiplier stack because of everything else, apart from AI**? Imagine visiting a pre-AI PostHog where reviews are mandatory as a time traveller from 2026, and you’re the only engineer there with secret access to AI tech from the future. At best, I think you’ll hit a 1-2x productivity gain because the bottleneck shifts from writing to PR approvals – but *only for you*. Others still aren’t free to review your completed PRs, and your PR output being much faster makes the review queue longer.

At Tano where we’re shipping 10x more, bottlenecks like this and others appear, like communicating with colleagues about what you’re doing, reviewing code, and figuring out what to build next. Let’s talk about some of these.

Engineers at Tano work as independently as possible. We come together and talk through big architecture decisions that affect the whole product, but for most things, there’s enough trust at the company that I can go do what I think is best. They also trust that if I’m not sure about what to do, or if there’s risks involved, that I’ll flag the risks and account for trade-offs.

**A big part of moving fast is having just one person responsible for things.** Of course, this assumes that they can figure out how to do it themselves, and go ship it.

At PostHog, when building out surveys, two engineers and one designer worked together to ship V1 in a month. All three of us iterated on the design, backend, and frontend. When I discovered something that needed to change on the backend, it often meant the other engineer needed to change something on the frontend as well. This was the communication overhead. Even so, engineers who worked together went faster than I would, figuring out the backend, *then* building out the frontend, *and then* iterating over any new constraints.

**Now that writing code is faster, a communication overhead makes little sense**. If I imagine that we’d be building the same survey functionality today, it’s now a lot more efficient if one person owns the entire stack end-to-end; design, frontend, backend. With Claude Code, this is doable by one engineer.

There’s still value in talking about unknown unknowns and things I’m not sure about with other engineers, but where the output is mostly known and it’s a straightforward feature or product, then I think one person working solo is more effective.

**Code reviews are optional at Tano**. That is, they are optional once you have a handle on the codebase. The time to confidence is variable, depending on how quickly you can get onboarded and how quickly human PR review comments go down to zero. I felt comfortable enough three weeks after I started, but depending on experience it varies, going up to eight weeks.

I built a code review agent via Claude Code Github Actions that runs on every PR, which is now standard across the codebase for everyone and runs in continuous integration (CI). There’s also a deeper, stronger agent we can run on demand if there’s a risky change.

In most cases, this automated reviewer is the only review on PRs. There are no human PR reviews.

[![](https://substackcdn.com/image/fetch/$s_!VTpg!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F91ab7783-b32e-44d7-beb1-6b183a9dcce5_921x790.jpeg)](https://substackcdn.com/image/fetch/$s_!VTpg!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F91ab7783-b32e-44d7-beb1-6b183a9dcce5_921x790.jpeg)

An example PR code review

Sometimes, things break. For example, in one reporting update, we needed to deal with minor currency units, like cents in the dollar. The agent decided to take these minor units, parse them as floats, and multiply by 100.

Do you see the mistake?

If you’ve worked with financial systems, you’ve probably been here. It’s a dumb mistake to assume every currency has 100 minor units. For example, the minor unit of the Bahraini dinar is the fulūs, and one dinar is 1,000 fulūs. But the generated code assumed it was 100 and this caused issues!

Plus, we already had a utility to handle all currency calculations and a human reviewer would have caught the generated code not using it.

**We’re willing to make the trade-off between automated agent review & moving quickly and fixing things as they break**. We make it at the cost of not going through a solid human review for every PR because we can do forward fixes on these issues quickly. In this case, we noticed the issue soon after deployment that reporting numbers were not matching up, and fixed them within the hour.

Sometimes, things break in new ways. For example, our posts table in Postgres was getting large, at about 25 GB and 6 million rows. We wanted to add a new partial index to this table, and migrations at 5 million+ rows aren’t straightforward. We realised this a bit later, when we did a blocking migration that blocked writes, and our ingestion pipeline stopped saving new posts. It also timed out, so new deploys failed to run. Now, every such migration needs to happen [concurrently](https://www.postgresql.org/docs/current/sql-createindex.html), which ensures it doesn’t block deploys or writes to the table.

We make sure to add any new way that things can break to the review agent. Now, it has a list of the largest tables and doesn’t allow blocking migrations on them.

[![](https://substackcdn.com/image/fetch/$s_!VG2h!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F393beaf2-631b-4a7a-8cc4-ee0cbff935d6_2048x576.png)](https://substackcdn.com/image/fetch/$s_!VG2h!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F393beaf2-631b-4a7a-8cc4-ee0cbff935d6_2048x576.png)

*Migration safety guards in code review prompt*

Given no code reviews and only agents writing the code, how has it changed velocity, PR-revert rate, and the median size of PRs? I have some interesting data. The revert rate is 6x lower at Tano because we fix things forward most of the time, rather than revert. But another part of it is that automated code reviews and agents are writing more bug-free code as the models improve.

Since I don’t have to wait for anyone to merge my PRs, the median time to merge is less than an hour.

[![](https://substackcdn.com/image/fetch/$s_!yjA3!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa7a1e0bf-25e3-4317-bd27-0e3fa750808c_1920x1523.png)](https://substackcdn.com/image/fetch/$s_!yjA3!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa7a1e0bf-25e3-4317-bd27-0e3fa750808c_1920x1523.png)

*Throughput and PR size stats from PostHog & Tano*

[![](https://substackcdn.com/image/fetch/$s_!ZOYb!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa472b684-dee9-4c7b-b1e5-dd77817a2074_1920x1689.png)](https://substackcdn.com/image/fetch/$s_!ZOYb!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa472b684-dee9-4c7b-b1e5-dd77817a2074_1920x1689.png)

*Speed and review stats from PostHog (May 2021–Jan 2025) & Tano (Jan–Sep 2026)*

At Bloomberg, I was shielded from customers and interacted only with our product manager who helped us prioritise what to work on. Having a clear-cut distinction between engineers and product managers is great when working at huge companies, but these roles merge as workplaces get smaller. At Bloomberg, every team of engineers had a dedicated product manager. At PostHog, there was one product manager for several teams. At Tano, there are none.

The product managers at PostHog helped with lots of research and setting of the product direction, but engineers were also expected to dive in, talk to customers, do our own research, and form our own opinions on product direction as and when needed. I thrived in that environment.

**In my current role, I am the product manager, too**. We have a roadmapping session every Monday where everyone comes up with issues they’ve faced, things we need to fix, things that will become a problem soon, such as the number of support tickets per customer increasing – which means that as we get more customers, humans could not keep up with the number of tickets to solve.

The CEO and CTO provide the most context on things to be done, and usually have a better idea of prioritisation. They give engineers context on things like customer churn risk, features needed for a new sale to close, etc. Together, we decide what makes sense to tackle, then divide the work among engineers. Usually, every engineer picks up a separate thing to work on.

**What used to take a month now often takes a week.** This is because agents build things much faster, and I’m typically working on multiple projects at the same time, and no longer have to wait on a product manager to feed me items to build. Hence, we just share context and priorities, and go build things to achieve our weekly goals.

Over my career, I’ve been part of five big migrations; one at Bloomberg, one at PostHog, and I’ve led three at Tano.

**At PostHog, the big migration took a year**. It was about [person-on-events](https://posthog.com/blog/persons-on-events). We have two huge tables at PostHog: the events table, with 20 billion+ events of what some anonymous id did; the persons table, which stores persons and their properties. When building funnels or unique user queries, we needed to join these two tables, then aggregate by the person. These queries were very slow, and downright unusable for larger customers. The person-on-events migration moved the person identifier and properties onto events, alongside the anonymous id. It was a massive company-wide effort to update every query to not use joins.

This person-on-events migration was very painful, and all of the code updates were written by hand. We took the time to make sure we future-proofed it because doing another similar migration on events was unfeasible.

**At Tano, we finished two complex migrations in just a few weeks**. Tano is an influencer marketing agency that builds AI agents to talk to creators, negotiate with them, go through content review with them, then pass it on to brands for final approval so that content can go live. The first iteration of this agent was a free-form checklist of what we wanted the agent to do, and lots of tools for it to accomplish those things. We didn’t have good observability in this system, and the agent would get pretty confused from time to time, using the wrong tool. The solution was to move to a workflow where each task is well defined with its own limited set of tools.

The migration at Tano moved every creator over to the new workflow, mapping existing checklist items into workflow tasks, and building new agent tools for each workflow. As we scaled this system over time, we realised the agent often lacked context on multiple threads with the same creator. The next migration addressed these deficiencies. Both these migrations took weeks, instead of a year.

**How fast we want to move is a conscious decision now**. In the second migration, I missed some guardrails, which led to creators getting paid twice for the same work. Duplicate payments are not great, but moving fast and breaking things was a trade-off we were very willing to make because getting the new architecture in place meant we could scale faster. At PostHog, we didn’t have the option to migrate quickly.

Our product architecture went from a mostly irreversible decision to a reversible one, which changes the calculus of what kind of experiments you can run, what kind of products you can build, and how specialised they can be for specific clients. We don’t need to get every detail right at the beginning, and can optimize for more information in production, then quickly iterate based on that information.

Of course, not everything is ideal with an AI-native work culture. I’ve gone into what’s possible with it, and now I’m going to focus on its costs. The bottlenecks aren’t gone, they just move on to me. Fast-paced work comes at the real cost of exhausting myself quickly, keeping a lot more things in my head, how easily new things distract me, and how little I know about what everyone else is working on.

I’m almost always working on multiple projects at once. Working on more than one at a time involves a fair amount of context switching, and I find it can get hard to keep up with all the PRs generated by my agents. I limit the number of things I work on at the same time, but it still feels a lot more mentally draining and exhausting than it was before AI agents.

In the charts above, notice how the columns representing multiple things touched in a month are much fuller at Tano than at PostHog. I feel more mentally tired at the end of each day than I used to.

Since things are so easy to fix and build now, I notice engineering is becoming a lot more reactive. A new issue comes in, and immediately I fire a Claude session up to fix it because I know that it won’t take more than an hour end to end, with no real active input from me.

**I used to be much more careful about choosing to work on something new**. For example, at PostHog, I’d make a measured decision based on the criticality of an issue, and how it competed on priority with my sprint goals. And at Bloomberg, I wouldn’t hear about new work to do at all until a sprint planning session.

In computer science, thrashing occurs when a system spends most of its time switching between tasks, rather than doing them. The fix is to limit concurrency and get the highest priority tasks done first.

Thanks to working on a lot more things, I get pulled in so many different directions that I’m reacting to everything and fixing new bugs that come up day-to-day, instead of what I should be doing. Like a computer, I’m spending more time switching between tasks and getting bugs fixed, than focusing on my own sprint goals.

Sometimes, when bugs are critical, changing priorities is valid and useful. But when I do it just because each individual task doesn’t take long, then it’s not great at moving the company forward.

It’s no wonder that the thrashing problem makes context-maxxing worse. I end up more mentally exhausted because I picked up more work in fixing quick bugs, when I should’ve been working on sprint goals.

Since there are no more code reviews, I’ve stopped reviewing most code as well. Of course, we review our own code and new team members’, but seldom for experienced colleagues.

Code reviews used to be one of my favourite things at PostHog. I had a feed of every PR coming in and I read them all. Reading helped me keep context of what everyone was working on, and I could jump in to help where needed, i.e., by suggesting things that already might exist in another part of the codebase. As PostHog grew from 20 to ~100 people, the number of new PRs per working day also grew from 15 to 30. It was impossible to keep reading every PR.

At Tano, we are merging about 30 PRs a day as well, and I’m not a big outlier. Another engineer at Tano has very similar output, and newer engineers + CTO are at roughly a third of that because of being new or having other responsibilities.

**Having no time to read all PRs means missing context across the business.** Doing code reviews was one way I used to keep up with what other engineers were doing, and how the product was growing. Also, isolated contexts affect team culture, as we end up working in silos and this discourages collaboration. On the upside, one person owns big patches of the product and can move fast. On the downside, it also means only one person has that context.

In software engineering, the “bus factor” is the number of people who need to disappear from a project in order for it to stall. If only one person has the context for the project, the bus factor is one. Having a low bus factor is an open problem for every team I’ve worked on. With AI, I do alone what my whole team used to do, which makes the bus factor problem more acute. I think AI provides the solution: if I’m good at writing down context in [AGENTS.md](http://agents.md) files in my product areas, anyone can hit the ground running, learn with AI, and contribute.

[![The review ratio](https://substackcdn.com/image/fetch/$s_!9mGk!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F43bc7d31-570e-4a3f-80c9-67dec40ee11c_1920x652.png "The review ratio")](https://substackcdn.com/image/fetch/$s_!9mGk!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F43bc7d31-570e-4a3f-80c9-67dec40ee11c_1920x652.png)

More merges, fewer reviews: number of PRs merged and reviewed monthly at Tano

The difference is even more striking when I visualize it as the ratio of PRs reviewed, divided by PRs merged:

[![](https://substackcdn.com/image/fetch/$s_!z2Bz!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbfe0009c-10dd-438f-a89f-76255e6ec98e_1920x817.png)](https://substackcdn.com/image/fetch/$s_!z2Bz!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbfe0009c-10dd-438f-a89f-76255e6ec98e_1920x817.png)

PRs reviewed/merged ratio. At Tano it’s nearly zero

Writing code used to define how my day looked at Bloomberg and PostHog. I’d spend time writing code in the morning, open PRs in the afternoon, and get them merged once I got approvals on them. As you can see in the graph below, I opened most of my PRs at PostHog between 10 AM and 3 PM and then waited for approvals, then got them merged later in the day.

At Tano, this graph is a lot more diffuse. PRs go up as and when the agent finishes working, and I then review the code, depending on risk profile and context. I instruct the agent, talk to it about what to build, read a lot more code, and merge much more often throughout the day, and I need to pace myself well.

At Tano, we have core working hours of 9AM to 6PM and many work longer. Early on when I joined Tano, I wanted to get as much done as possible, which led to front-loading tasks in the morning, running several workstreams at once, and multitasking. I’d get mentally exhausted by 3PM and have nothing left in the tank. Before Tano, I was constrained by my speed at writing code, and I can’t write two different lines of code at the same time, so I couldn’t multitask.

**I learned to pace myself.** This was in order to prioritise the most important tasks, not multitask ten things at once, and to not try and do everything in the morning. If I try doing important changes in the evening when mentally drained, I make mistakes and don’t catch issues I otherwise would.

Every evening, I leave my laptop at work and let my agents run locally. They usually stop an hour or two later, so when I come in in the morning, my PRs are ready to go. The difference in morning PRs is clearest in the graph below. At Tano, between 9 and 11 AM, I open 18% of my PRs, while at PostHog, it was only 9%. Except in early mornings and during lunch, the percentage of PRs per hour opened at Tano is pretty stable at roughly 11% per hour because the agents run all day. PostHog peaked around lunch time, and variance throughout the day got slightly larger because as evening approached, my efficiency at writing code went down, so I reserved some time for other tasks, like reviewing code, admin work, and catching up on Slack memes.

PostHog was fully remote, so I’d work from home with no proper end-time for work. My laptop was always with me, and I’d spend some evenings working late because I wanted to get something done, rather than think about it all night. At Tano, I always leave my laptop open at work and I switch off at home. Leaving my laptop at work was a bit weird to colleagues, but it’s been amazing for me: not only do my agents keep running like I want them to, but more importantly, I have a clear switching-off point for the end of the day.

**I put this guardrail in place to stop being overwhelmed by AI**. Sometimes I do feel that I’m not living up to my potential with AI; it’s here, waiting to be used, waiting to build more things with, waiting for direction, and I’m *sleeping*? The sense of not doing enough while working harder than ever feels overwhelming.

My guardrails – like no work laptop at home – save me from getting carried away. Routines like always ending the work day at 6pm help me stay grounded. It took a while to come to terms with the feeling of not living up to my potential, but I’ve accepted and reframed this feeling: there’s more to life than just building.

I can be [more ambitious with the things](https://neilkakkar.com/ai-made-me-braver.html) I want to build, and I should prioritise what to build over building absolutely everything just because that’s possible now. For example, I recently started a new pseudo-anonymous [Instagram](https://www.instagram.com/rathersurprising/) AI animations channel, which takes ideas from my blog and converts them into cool animations. I always wanted to make animations, but found it too time-consuming. I’m focusing on this side project over the music generator, reel editor, podcast app, and tv streaming app I also want.

Among the first ten hires at PostHog, one was an artist, and one a designer. Both are heavily responsible for the quirky, well designed, beautiful brand that is PostHog.

**With AI, I observe early-stage startups discounting design work.** I assume they do so because AI can do a good-enough job at making things look nice. But at the same time, AI designs are very similar to a lot of other designs out there. Neo-brutalist design tries to stand out, but most AI-generated neo-brutalist design is still pretty similar. At Tano, it’s what we’ve ended up with, and we’re taking steps to figure out what our brand should look like.

[![](https://substackcdn.com/image/fetch/$s_!US_S!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F40910634-d975-45db-9f34-8a582190de68_2048x550.png)](https://substackcdn.com/image/fetch/$s_!US_S!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F40910634-d975-45db-9f34-8a582190de68_2048x550.png)

Left: *PostHog’s hand-crafted bran; middle: Tano’s AI-generated neo-brutalist site; right: an off-the-shelf neo-brutalism component kit*

Tokenmaxxing is the idea of using as many AI tokens as possible, often with no regard for the actual productivity gains of doing it.

**At Tano, there is no ‘infinite’ AI budget.** Everyone has one 20x Claude Max plan subscription, and that’s it.

We don’t have infinite funding, and use AI as a tool to help us move faster instead of it being an autonomous teammate. If we were paying API rates, my usage would come to $8,000+ per month.

**Hitting limits on my Claude max plan does still happen**. It is more frequent when experimenting with using AI as an autonomous teammate running loops and fixing any errors it finds from our logs. Hitting the limit forces me to focus on what’s the most important thing to be doing.

Weirdly, I want more out of my tools. I’m already shipping 11x more thanks to AI, but why stop there? I want to explore what’s possible if I had no token limits: how effective is Claude at running autonomously, watching logs, session recordings on PostHog, and acting on this information by opening PRs on its own? Can it monitor agent behaviour at Tano, read every conversation with every creator, and tell us when we paid one creator twice because of a bug in our migration?

Already, AI agents are writing my code. But some seem far more capable if you have the tokens to spend. How much further can I push them? I don’t know the answer yet, but it’s something I would love to experiment further on.

Overall, it seems like the world is changing very quickly, and I’m super excited to be in the middle of it all.

So, how much of the change is due to AI, and how much is from everything else around it?

**AI is a speed multiplier.** Speed is a defining feature of start ups, and how they outcompete incumbents by experimenting and adapting to market demand quickly, and catching up to incumbents in terms of features. PostHog was not the first product analytics platform – in fact, it was ten years late to the party. But people at PostHog executed quickly, caught up with the competition, and the product has now expanded into several different categories, like experimentation, data warehouse, error tracking, and session recordings.

We built these products before AI. Today, a startup with AI can out-execute another without AI, and since speed is a defining feature, the startup without AI will have a very difficult time.

**Your productivity gains are limited if the rest of the company is not onboard with AI**. It’s clear that AI is necessary, but it’s not sufficient. For example, if you were a time-traveller visiting PostHog, pre-AI, you’d still not be all that productive with all of today’s AI tools. This is because with mandatory code reviews from fellow engineers, the review queue keeps increasing endlessly since we produce faster than we can review.

**Encouraging experimentation in ways of working can pay off.** I’ve seen teams with this approach end up better off. A team like Tano encourages experimentation by not frowning upon me for leaving my laptop at work and letting agents run overnight.

Further, a company that is flexible, recognises new constraints when they show up, and works towards removing them so it can move even faster, seems to do better than ones that adopt one way of working and stick to it. For example, Anthropic also automated code reviews away, but then ended up [constrained by continuous integration (CI) pipelines](https://claude.com/resources/articles/agentic-coding-is-straining-ci-heres-how-we-scaled-test-impact-analysis-at-anthropic). They unblocked CI as the new constraint and then found out their source control couldn’t keep up. In order to keep moving faster, they [redesigned source control](https://www.anthropic.com/research/redesigning-source-control).

**As you adopt AI to move faster, a lot of working technologies and team processes will crack.** At the very least, they will strain under the load of getting more work done. A lot of startups will probably not end up at the scale where they have to redesign source control, but the general principle stands: you can restrict AI for what I believe suboptimal results, or you can lean into it, find out where new constraints end up – like in code reviews – and relax those constraints by automating those reviews so you can continue moving faster.

I value a team culture where we experiment, are open to being proven wrong, humble, curious, and try to make the most of the tools we have. At Tano, this culture is a defining factor in our successful AI adoption, and I believe culture is a necessary – and sufficient – condition of success at every AI-era startup.

*Gergely, again.*

Thanks to Neil for this data-driven essay on how he sees things changing in the industry, and how quickly it’s happening.

Neil’s story about how his PR output increased tenfold from the beginning of the year is an interesting one. Two forces are at play:

- Since January, the industry has started to adopt AI coding agents more. We’re seeing around a twofold increase in PR output on average, across the industry, as covered in our [deepdive on PR frequency and AI cost](https://newsletter.pragmaticengineer.com/p/whats-the-roi-of-software-engineers)

- Neil moved from a startup that was fast-moving in the pre-AI era (PostHog) to an even younger, truly “AI-native” startup where nothing could be put at risk by moving too fast

I appreciate this contrast.

**The sudden change in shipping pace when going from a pre-AI startup to an AI-native one is hard to ignore**. Of course, we know that iteration speed is pretty meaningless by itself, without looking at things like finding product-market-fit, generating revenue, or having healthy unit economics.

[![](https://substackcdn.com/image/fetch/$s_!pBhe!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F192d7b8f-6865-4f84-94c4-df6257cb7ae6_1920x899.png)](https://substackcdn.com/image/fetch/$s_!pBhe!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F192d7b8f-6865-4f84-94c4-df6257cb7ae6_1920x899.png)

*PRs are just one metric, but this contrast between an “AI-native” and a “pre-AI” startup is still a big one. See Neil’s [interactive chart](https://neilkakkar.com/assets/html/two-eras-charts.html?card=cumulative-merged-prs)*

Still, I wonder if this new generation of “AI-native” startups have this temporary advantage of iterating much faster than companies that are only a few years older than they are. *I have no doubt that “pre-AI” startups will eventually catch up, but when you have a business that works well, you want to be more careful and deliberate in changing your processes and tooling. The last thing you want to do is to break your business by being careless!*

**I also like that Neil talks a lot about the pressure that comes with using AI.** He feels the need to put several guardrails in place, so that he has personal time to unwind and come back refreshed and focused. This is positive because it’s really easy to use AI too much and never stop working: in March, we talked [about this “Dracula-effect”](https://newsletter.pragmaticengineer.com/p/from-ides-to-ai-agents-with-steve) with Steve Yegge, including how using AI agents seems to drain the energy of engineers.

*Congratulations to Neil once more for his personal, highly detailed account of what’s changed in his work. Thanks to everyone who took part in this essay challenge! Look out for more essays to come.*

No posts
