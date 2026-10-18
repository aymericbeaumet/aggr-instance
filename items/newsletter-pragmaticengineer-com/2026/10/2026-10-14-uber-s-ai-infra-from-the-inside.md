---
title: Uber’s AI infra, from the inside
link: https://newsletter.pragmaticengineer.com/p/ubers-ai-infra-from-the-inside
source: newsletter-pragmaticengineer-com
published: 2026-10-14T17:06:56Z
updated: 2026-10-14T17:06:56Z
first_seen: 2026-10-14T17:58:19.452435153Z
authors:
- Gergely Orosz
summary: Uber Distinguished Engineer Uday Kiran Medisetty shares how Uber builds internal developer tools, integrates AI agents into engineering workflows, and measures their impact on productivity.
content: extracted
html: 2026-10-14-uber-s-ai-infra-from-the-inside.html
remote_preview:
  url: https://substackcdn.com/image/fetch/$s_!Qhi0!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1228ff6a-514b-4643-9360-02cda14dc5ac_800x70.png
extra:
  audio_type: audio/mpeg
  audio_url: https://api.substack.com/feed/podcast/220071064/bd8e63e86d1fcf09a618b7402be21faa.mp3
  duration_seconds: 5598
---

**Listen and watch now on [YouTube](https://youtu.be/MDUYSMJmi8A), [Apple](https://podcasts.apple.com/us/podcast/the-pragmatic-engineer/id1769051199), and [Spotify](https://open.spotify.com/show/2Bho9xCbOQMWMJ7UKmqCzD).** See the episode transcript at the top of this page, and timestamps for the episode at the bottom.

![](https://substackcdn.com/image/fetch/$s_!Qhi0!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1228ff6a-514b-4643-9360-02cda14dc5ac_800x70.png)

• **[turbopuffer](http://turbopuffer.com/pragmatic)** – The turbopuffer engineering team is doing something really cool: they are completely redesigning their storage architecture from first principles to make search faster, cheaper, and more reliable at scale. And they’re documenting all of it! Follow along their rewrite at [turbopuffer.com/v3](http://turbopuffer.com/v3)

• **[Linear](https://linear.app/pragmatic)** – one of the big challenges for us devs is the sheer amount of code to review. [Linear Diffs](https://linear.app/build#diffs) is a new feature to review PRs inside of Linear, with all the context in one place. It even supports stacked PRs. [Check it out](https://linear.app/pragmatic)

**• [WorkOS](https://workos.com/)** – how do you deal with agents and permissions? WorkOS built [Airlock](http://workos.com/airlock), the authorization layer for AI agents. It evaluates every request against the agent’s intent and your rules and either allows it, denies it, or routes it to a human for approval. Works with Claude Code, Codex and MCP gateways. [Give it a spin](http://workos.com/airlock)

What do developer tools look like inside Uber, today? They are *very* different from what you would see at most places. For example, can you guess what service a system called DevPods, Code Inbox, Slate or Wisdom would be providing to engineers?

[![](https://substackcdn.com/image/fetch/$s_!KXrA!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc0423aa8-e9d7-4359-af46-27597d633df1_1942x1532.png)](https://substackcdn.com/image/fetch/$s_!KXrA!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc0423aa8-e9d7-4359-af46-27597d633df1_1942x1532.png)

*DevPods, Code Inbox, Slate and Wisdom: four of the 20+ internal tools Uber engineers use,day-to-day*

Today’s episode is a deepdive into the day-to-day tools of devs at Uber, our tour guide being on of Uber’s longest-tenured engineers: [Uday Kiran Medisetty](https://x.com/udaykiran?lang=en). Uday is one of the few Distinguished Engineers at the company, and leads much of the work on agentic AI, today.

With Uday, we talk through how Uber’s developer infrastructure prepared it for AI agents. We look at the systems Uber built before generative AI and the new tools that connect agents to its code, internal services, and engineering knowledge. Uday explains how engineers use background agents, why tools like Slate make development (and bugfixing!) more efficient, and how code review is changing. We also cover Uber’s approach to token costs and how it measures productivity as the engineer’s role changes.

[www.youtube.com](https://www.youtube.com/watch?v=MDUYSMJmi8A)

Here are 11 interesting learnings from today’s episode:

**1\. Uber’s service count changed erratically:** in 2015, when Uday joined, there were two services in total. By 2019, there were thousands. In 2026, there will be about the same number of services than in 2019, even though the business is about 10x larger than back then!

**2\. Uber built most services in-house because off-the-shelf software could not handle its scale – or simply did not exist**! Examples of services Uber built because it had to:

- **uOwn**: shows who owns a service or asset. Back in 2015, no such vendor existed.

- **M3DB**: Uber was emitting about 500 million metrics per second, with no vendor being able to handle this load, so they built it themselves. Observability company [Chronosphere](https://chronosphere.io/) was built on top of the open sourced M3DB, and we previously covered [how Uber built its observability platform](https://newsletter.pragmaticengineer.com/p/how-uber-built-its-observability-platform).

- **Cadence**: a durable workflow engine for multi-day flows like driver onboarding. No such system existed at the time when Uber needed it.[Temporal](https://temporal.io/) was built on top of the open sourced Cadence framework.

- **Morpheus**: a system combining feature flags *and* experimentation, including switchback tests, with support for two- and three-sided marketplaces that Uber operates. Again, no such solution existed when Uber needed it.

**3\. At Uber, cities are a first-class concept that go all the way down to physical infrastructure**. Another reason Uber had to build a lot of its custom tooling is how the concept of cities is a “basic primitive” that needs to be handled across systems like observability, feature flagging, deployments, and even service sharding. For example, in the early days, some services had their database sharded by cities. Even today, each city is pinned to a cloud region, and can fall over to another region at any time. *We previously covered* [how Uber moved from bare metal to the cloud](https://newsletter.pragmaticengineer.com/p/uber-move-to-cloud)*.*

**4\. A single AI model gateway has paid lots of dividends**. In 2023, Uber’s platform team made the decision that everything built with AI needs to be model- and harness-agnostic. They built Model Gateway to support switching between Anthropic, OpenAI, other vendor models, and now also supports open models they run. Having the Model Gateway in place made it easy to extend with these parts:

- **AI Guard**: a middleware run by the security team. It checks all requests and responses, and measures latencies (and flags overly slow models or queries)

- **Spend tracking:** spend is tracked per user

- **Visibility**: audit logs and session traces

**5\. AIFX allows engineers to switch harnesses without losing settings**. Uber currently supports Claude Code, Codex, and OpenCode as harnesses. AIFX (short for AI Foundations) is a wrapper on top of all of these, so when an engineer changes harnesses, all their settings, skills and plugins move with them.

**6\. Uber has a hybrid approach to AI spend**: engineers get allocated a spend tier. When they hit the limit, their manager can sign off to spend more in that month. Basically, for the right type of work, there is no spend limit, but Uber doesn’t want engineers to carelessly burn tokens.

A plugin called CostCheck works across sessions and gives tips on how to improve an engineer’s cost profile. *Uber has reduced token costs by about 50% the last few months, as we* [previously covered.](https://newsletter.pragmaticengineer.com/i/214037819/uber-ai-costs-down-50)

**7\. All PRs are still reviewed by engineers, but Uber built tools to help devs decide how much time they need to spend on each code change**. Uday admits that code review has become a big bottleneck inside the company, and Uber built Code Review to make it easier and more efficient for devs to do these reviews.

By the time a dev reviews a change, AI code review has run, screenshots and videos of the feature have been added (when applicable), and risk scores are assigned to the change.

**8\. Uber had to declare an internal “code red” for its CI system earlier this year**. This was because CI jobs more than doubled in a 12-month timeframe, pushing load on CI systems overly high. To keep build speeds at bay, the team moved simple CI jobs (eg linting, build and template rules) to the “inner loop:” so agents run these commands with the *afx validate* command before opening a PR.

**9\. Uber tags PRs by the type of work each of those PRs do**. In the past, Uber measured “PR velocity:” how long it took for a PR (or, as Uber calls it: a “diff”) to get to production. With agents, doing so makes less sense, because it’s easy to create dozens or even hundreds of PRs, and many of those PRs might have no business impact on the product.

So Uber now tags all PRs by type: feature, bugfix, maintenance, testing, or docs.

They are seeing the percentage of “feature PRs” going up, and the percentage of “maintenance PRs” going down.

I find this categorization to be a clever approach!

**10\. The job of a software engineer inside a place like Uber is to build context**. As Uday reflects on how the job has changed:

> “The job of engineering is, in some sense: ‘how do you build the right context for the agents to produce reliable output?’
>
> You need to get really good at context engineering.
>
> Also, \[being good at\] multitasking has become a thing. Before, it would take so much time to create a PR, so you would be laser-focused on it. But now you’re constantly switching between so many things. You need to be able to verify your work and pick any code smells from the agent \[to avoid\] shipping regressions into production.
>
> Context engineering, ability to multitask, and ability to review and verify the outputs: these skills have become much more important now because you’re able to parallelize a lot more than before.”

**11\. Uber’s AI tooling was seemingly built in a year: in reality, it was built on top of a decade worth of dev tooling investment**. Minion – built in 2025 – is powered by DevPods, which Uber created in 2017. Uber’s MCP Gateway was fast to build, thanks to existing Thrift and Protobuf registries. Jaeger tracing made Slate routing and Context graph easier, and so on.

In Uday’s words:

> “All the things that we built to make life easier for engineers: it makes it easier for our agents as well!”

• [How Uber uses AI for development: inside look](https://newsletter.pragmaticengineer.com/p/how-uber-uses-ai-for-development)

• [Scaling Uber with Thuan Pham (Uber’s first CTO)](https://newsletter.pragmaticengineer.com/p/scaling-uber-with-thuan-pham-ubers)

• [Inside Uber’s move to the Cloud](https://newsletter.pragmaticengineer.com/p/uber-move-to-cloud)

• [The Platform and Program Split at Uber](https://newsletter.pragmaticengineer.com/p/program-platform-split-uber)

• [Hypergrowth startups: Uber and CloudKitchens with Charles-Axel Dein](https://newsletter.pragmaticengineer.com/p/high-growth-startups-uber-and-cloudkitchens)

• [How Uber Built its Observability Platform](https://newsletter.pragmaticengineer.com/p/how-uber-built-its-observability-platform)

• [How Uber is Measuring Engineering Productivity](https://newsletter.pragmaticengineer.com/p/uber-eng-productivity)

• [Developer Experience at Uber with Gautam Korlam](https://newsletter.pragmaticengineer.com/p/developer-experience-at-uber)

[00:00](https://www.youtube.com/watch?v=MDUYSMJmi8A) Intro

[02:48](https://www.youtube.com/watch?v=MDUYSMJmi8A&t=168s) Uday’s early days at Uber

[05:40](https://www.youtube.com/watch?v=MDUYSMJmi8A&t=340s) Uber’s move to microservices

[07:55](https://www.youtube.com/watch?v=MDUYSMJmi8A&t=475s) Uber’s engineering team, then and now

[09:05](https://www.youtube.com/watch?v=MDUYSMJmi8A&t=545s) Developer tooling before AI

[16:03](https://www.youtube.com/watch?v=MDUYSMJmi8A&t=963s) DevPods

[19:28](https://www.youtube.com/watch?v=MDUYSMJmi8A&t=1168s) Building for AI agents

[29:27](https://www.youtube.com/watch?v=MDUYSMJmi8A&t=1767s) MCP gateway

[34:37](https://www.youtube.com/watch?v=MDUYSMJmi8A&t=2077s) Agent Skills repo

[38:46](https://www.youtube.com/watch?v=MDUYSMJmi8A&t=2326s&pp=0gcJCWMAwfN6Pr3D) The context graph and Cortana

[47:54](https://www.youtube.com/watch?v=MDUYSMJmi8A&t=2874s) PRD and ERD tool

[50:45](https://www.youtube.com/watch?v=MDUYSMJmi8A&t=3045s) Minion

[54:46](https://www.youtube.com/watch?v=MDUYSMJmi8A&t=3286s&pp=0gcJCWMAwfN6Pr3D) Testing and code review with Bazel, Slate, and Capri

[1:12:26](https://www.youtube.com/watch?v=MDUYSMJmi8A&t=4346s) Edge Gateway, Shepherd, and Wisdom

[1:21:33](https://www.youtube.com/watch?v=MDUYSMJmi8A&t=4893s) The changing role of engineer and measuring productivity

[1:30:40](https://www.youtube.com/watch?v=MDUYSMJmi8A&t=5440s) Uday’s favorite new products

**Where to find Uday Kiran Medisetty:**

• X: [https://x.com/udaykiran](https://x.com/udaykiran)

• LinkedIn: [https://www.linkedin.com/in/udaykiran](https://www.linkedin.com/in/udaykiran)

**Mentions during the episode:**

• Scaling Uber with Thuan Pham (Uber’s first CTO): [https://newsletter.pragmaticengineer.com/p/scaling-uber-with-thuan-pham-ubers](https://newsletter.pragmaticengineer.com/p/scaling-uber-with-thuan-pham-ubers)

• How Uber uses AI for development: inside look: [https://newsletter.pragmaticengineer.com/p/how-uber-uses-ai-for-development](https://newsletter.pragmaticengineer.com/p/how-uber-uses-ai-for-development)

• Bazel: [https://opensource.google/projects/bazel](https://opensource.google/projects/bazel)

• Developer Experience at Uber with Gautam Korlam: [https://newsletter.pragmaticengineer.com/p/developer-experience-at-uber](https://newsletter.pragmaticengineer.com/p/developer-experience-at-uber)

• Hypergrowth startups: Uber and CloudKitchens with Charles-Axel Dein: [https://newsletter.pragmaticengineer.com/p/high-growth-startups-uber-and-cloudkitchens](https://newsletter.pragmaticengineer.com/p/high-growth-startups-uber-and-cloudkitchens)

• Instinct: [https://instinct.com](https://instinct.com)

• Muse: [https://ai.meta.com/muse](https://ai.meta.com/muse)

• Grok Bot: [https://x.ai/bot](https://x.ai/bot)

—

Production and marketing by [Pen Name](https://penname.co/).
