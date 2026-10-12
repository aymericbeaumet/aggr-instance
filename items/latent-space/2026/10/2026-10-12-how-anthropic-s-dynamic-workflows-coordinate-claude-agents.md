---
title: How Anthropic’s Dynamic Workflows Coordinate Claude Agents
link: https://www.latent.space/p/claude-dynamic-workflows
source: latent-space
published: 2026-10-12T18:18:17Z
updated: 2026-10-12T18:18:17Z
first_seen: 2026-10-12T22:35:36.108955126Z
authors:
- Richard MacManus
summary: At AI Engineer New York, Anthropic drew on lessons from Unix and Linux to explain how agents can tackle bigger projects without overwhelming developers.
content: feed
html: 2026-10-12-how-anthropic-s-dynamic-workflows-coordinate-claude-agents.html
remote_preview:
  url: https://substack-post-media.s3.amazonaws.com/public/images/dda42c05-6ce1-4e54-b544-f7d1557f04b2_1280x720.jpeg
---

[![](https://substackcdn.com/image/fetch/$s_!wcn_!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbee9915a-7027-4d1c-90c8-69803c5b44f4_1280x960.jpeg)](https://substackcdn.com/image/fetch/$s_!wcn_!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbee9915a-7027-4d1c-90c8-69803c5b44f4_1280x960.jpeg)

CJ Avilla and Harrison Stall, Members of Technical Staff at Anthropic, at AI Engineer NYC today.

The [AI Engineer New York 2026 conference](https://ai.engineer/nyc/2026) kicked off this morning with a series of workshops, including Anthropic discussing **how to orchestrate its Claude Managed Agents**. Of particular interest was a brand new feature, [announced on Friday](https://x.com/ClaudeDevs/status/2108591328732856655), called **dynamic workflows** — or simply “flows,” as the presenters repeatedly called it.

Flows are defined as “a new type of multiagent orchestration, built for your most ambitious workloads.”

**[CJ Avilla](https://www.linkedin.com/in/cjavilla/)** and **[Harrison Stall](https://www.linkedin.com/in/harrisonstall/)**, both **Members of Technical Staff at Anthropic**, began by invoking some operating system projects of the past — from Ken Thompson building the original Unix operating system, through to Linux evolving from one person (Linus Torvalds) to **layers of trusted maintainers**. The lesson: agents need **the same level of organization** in order to do the types of big, ambitious projects Anthropic envisions for dynamic workflows.

* * *

**Sponsored by Union.ai**

Build a sovereign AI factory

The best engineering teams are building AI factories where they own and control not only their data, compute, and infrastructure, but also a durable runtime on their cloud.

Union.ai gives your workflows the durability to adapt and right-size infra resources and recover from failures without re-running completed work.

Get started directly through AWS Marketplace and **[try Union.ai free for 30 days](https://signup.hosted.unionai.cloud/sign-in?utm_source=Influencer&utm_medium=partnership&utm_campaign=Latent%20Space)**.

To learn more, visit us at AI Engineer New York booth #48.

* * *

“I think a lot of us who are orchestrating and managing lots of agents now might start to feel **this feeling of being the bottleneck and being the breaking point** as we have lots of agents contributing tons and tons of PRs,” Avilla said. “And we’re sitting there in our GitHub trying to approve PRs and make sure that everything works as expected.”

[![](https://substackcdn.com/image/fetch/$s_!_WMP!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2bbe55bc-0357-4a4c-8903-d5b93ef53a4a_1646x988.png)](https://substackcdn.com/image/fetch/$s_!_WMP!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2bbe55bc-0357-4a4c-8903-d5b93ef53a4a_1646x988.png)

## From agent rosters to dynamic workflows

The pair noted that dynamic workflows began in Claude Code as the “a harness for every task” concept. But **flows can run longer than a single agent or a roster.** The basic idea is that Claude chooses the organization of agents.

**“Flows are essentially the next version of multi-agent orchestration,”** said Stall, “where you can let Claude fully orchestrate the entirety of its own multi-agent topology.”

The way this works is that the lead agent creates a plan for how it wants to accomplish tasks. Claude then writes an orchestration program, which launches agents, collects results, and manages information flow.

[![](https://substackcdn.com/image/fetch/$s_!ybT5!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fda2eb043-e1ae-4aab-adb2-cac56c13d44e_1548x1174.png)](https://substackcdn.com/image/fetch/$s_!ybT5!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fda2eb043-e1ae-4aab-adb2-cac56c13d44e_1548x1174.png)

Image [via Anthropic](https://x.com/ClaudeDevs/status/2108591331660468538/photo/1)

Previously, Claude Managed Agents required developers to configure a coordinator and a roster of subagents. **With dynamic workflows, Claude itself determines how to divide and orchestrate the work.**

“Across multiple different phases of the multi-agent orchestration, it’s able to accomplish discrete, verifiable units of work without you having to manage any of the interconnections between these things,” Stall said.

[![](https://substackcdn.com/image/fetch/$s_!cNfD!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2ec1518b-8dad-41f1-b3c8-f453f3752cec_1260x1190.png)](https://substackcdn.com/image/fetch/$s_!cNfD!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2ec1518b-8dad-41f1-b3c8-f453f3752cec_1260x1190.png)

That said, Avilla noted that **the user can introduce specialist sub-agents if they wish.**

“It’s also possible in the agent config to pass in pre-configured sub-agents,” he said.

All of this is available in **Claude Console**, the interface used by developers accessing Claude through API — the whole point of Claude Managed Agents.

[![](https://substackcdn.com/image/fetch/$s_!rDfm!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd16fc0e8-d7a4-4b1b-b6e9-7cd0e95cc34a_2048x1124.png)](https://substackcdn.com/image/fetch/$s_!rDfm!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd16fc0e8-d7a4-4b1b-b6e9-7cd0e95cc34a_2048x1124.png)

*Claude Console, where Managed Agents is located.*

The presenters then showed a relatively simple workflow of a stock watchlist scanner. Although this was far removed from the big, ambitious projects Anthropic is encouraging, it did illustrate **the basic pattern**: a scan phase **distributed individual tickers to agents with isolated contexts**, and a later review phase examined the findings before producing a report.

## Q&A: Budgets and the limits of self-improvement

The audience Q&A surfaced a number of potential problems. One audience member asked **how Claude decides how many agents a task needs**, and could a simple ticker scan trigger hundreds of sub-agents?

“The answer really is **evals and lower defaults**,” Stall replied, noting that ambitious tasks generally require explicitly encouraging greater parallelization.

One enduring concern with agent orchestration is **maintaining control over your token budget**. Someone asked whether the lead agent knows how much money it has available?

Stall replied that the model does not know its budget while running, but that developers can set a session budget. He added that **informing models of their budgets could make the agents “a little bit anxious,”** which elicited chuckles from the audience.

[![](https://substackcdn.com/image/fetch/$s_!RJ39!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff79ff38b-6346-410b-9257-28bdc64bd868_1438x1064.png)](https://substackcdn.com/image/fetch/$s_!RJ39!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff79ff38b-6346-410b-9257-28bdc64bd868_1438x1064.png)

Scanner in the stock watchlist example.

A related question was how can developers make **repeated flows cheaper and more efficient over time?**

Outside of asking Claude to examine the costs incurred, Avilla suggested another relatively new feature be used: **dreaming**. [In the documentation](https://platform.claude.com/docs/en/managed-agents/dreams), “dreaming” is defined as a research preview feature. A dream “reads an existing memory store alongside past session transcripts, then **produces a new, reorganized memory store,**” with duplicates merged, “stale or contradicted entries” replaced, and “new insights surfaced.”

Avilla described dreaming in the flow context as being able to **“crystallize and basically improve the memory store that is related to those sessions.”**

For the stock watchlist example, someone asked **whether the watchlist flow can improve itself?** For example, recognize that its scanning model is inadequate and change the next scheduled run?

Stall said this wasn’t built in, but he and Avilla suggested **hill-climbing and dreaming** could be used to improve the loop.

[![](https://substackcdn.com/image/fetch/$s_!-s5o!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6e94b828-cc7d-418d-83cf-4f1b555aeddc_1474x1116.png)](https://substackcdn.com/image/fetch/$s_!-s5o!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6e94b828-cc7d-418d-83cf-4f1b555aeddc_1474x1116.png)

The brain/hands conceptual overview of Managed Agents.

What about **visibility into the actions of the sub-agents**, another audience member asked.

Avilla replied that yes, developers can always inspect that through graphing functionality inside Claude Console.

**“You can tell, what did the lead agent actually pass to the sub-agent?** You can look at that payload here to deeply understand what is actually happening for every transition in this entire graph.”

Another interesting question was **whether the subagents can communicate with each other?**

Stall said the **agents share a filesystem** and can use it to exchange information, but developers cannot directly instruct one workflow subagent to message another through the API.

## Flows cost much more, but are designed for big tasks

In the watchlist-scanner comparison, Avilla said flows cost “much more” than a single-agent approach, but were also **“much more effective at finding all of the details.”**

[![](https://substackcdn.com/image/fetch/$s_!fJVm!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F08c9072d-bef4-4262-a582-974384824502_1494x1114.png)](https://substackcdn.com/image/fetch/$s_!fJVm!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F08c9072d-bef4-4262-a582-974384824502_1494x1114.png)

Setting a budget is key!

The bigger question is **how ambitious can flows be, really?** One audience member asked whether dynamic workflows can power large scientific research projects, referencing OpenAI’s recently announced proof concerning the **Navier–Stokes Millennium Prize problem**.

While Stall wouldn’t be drawn on specifics about how Anthropic computes large scientific problems, he said **it is their goal to enable general users to tackle those problems too.**

“Flows are basically a way to **throw test-time compute at hard problems** and get more capability faster out of the models,” he said. “We think, especially as an API service, that unlocking the power of this is the first step for us making these types of discoveries, or scientific progress, **broadly doable by the general public.**”
