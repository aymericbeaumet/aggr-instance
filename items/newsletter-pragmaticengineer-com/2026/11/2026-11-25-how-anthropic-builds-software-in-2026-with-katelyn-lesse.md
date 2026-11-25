---
title: How Anthropic builds software in 2026 with Katelyn Lesse
link: https://newsletter.pragmaticengineer.com/p/how-anthropic-builds-software-in
source: newsletter-pragmaticengineer-com
published: 2026-11-25T17:31:32Z
updated: 2026-11-25T17:31:32Z
first_seen: 2026-11-25T20:07:11.161777435Z
authors:
- Gergely Orosz
summary: Katelyn Lesse explains how Anthropic rewrote Claude’s API in Rust and how AI agents are changing engineering at the company.
content: extracted
html: 2026-11-25-how-anthropic-builds-software-in-2026-with-katelyn-lesse.html
remote_preview:
  url: https://substackcdn.com/image/fetch/$s_!MUG0!,w_1200,h_600,c_fill,f_jpg,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff7f42984-2bb8-4041-bee8-3c81b2fa48ab_1280x720.png
extra:
  audio_type: audio/mpeg
  audio_url: https://api.substack.com/feed/podcast/226291266/48f1ade2511a24d25234b3abca7031c0.mp3
  duration_seconds: 6585
---

*Programming note: there will be no The Pulse tomorrow. For those of you who celebrate, Happy Thanksgiving! The regular schedule returns next week.*

**Listen and watch now on [YouTube](https://youtu.be/9oNvKMd7xJo), [Apple](https://podcasts.apple.com/us/podcast/the-pragmatic-engineer/id1769051199), and [Spotify](https://open.spotify.com/show/2Bho9xCbOQMWMJ7UKmqCzD).** See the episode transcript at the top of this page, and timestamps for the episode at the bottom.

![](https://substackcdn.com/image/fetch/$s_!AlJu!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8e3d3c6f-6a37-411e-aa1a-dfcefc979af3_1600x140.png)

• **[turbopuffer](http://turbopuffer.com/pragmatic)** – companies like Anthropic, Cognition, and Notion build their search infrastructure on turbopuffer because it is insanely reliable and performant at the frontier of AI scale. It also powers the [The Pragmatic Engineer search](http://pragmaticengineer.com/search) site. [Sign up today](http://turbopuffer.com/pragmatic) and start building.

• **[Antithesis](https://antithesis.com/pragmatic)** – Verify your system’s correctness without human review or traditional integration tests, and avoid bugs and outages. Antithesis [launched a plugin](https://antithesis.com/blog/2026/plugin-launch/?utm_medium=podcast&utm_campaign=pragmatic_2026&utm_source=pragmatic&utm_content=pragmatic_20260513&discount=PRAGMATIC) that makes your coding agent incredibly effective at finding and fixing bugs, and is dead simple to set up. There’s also a new, self-service Team plan for **$2,999/month**. Use the code PRAGMATIC50 for 50% off your first month. [Learn more](https://antithesis.com/pragmatic)

• **[Entire](https://entire.io/pragmatic)** – Entire has launched its own Git forge, and you can now host private repos directly in the US, Europe, India, Australia, Brazil, and Singapore. They’ve also reinvented the pull request with [Trails](https://entire.com/about?utm_source=pragmatic#trails), which is a much more natural workflow when agents are pushing new changes after iterating on feedback. You get more context and control on when a change is ready to merge. [Check it out](https://entire.com/pragmatic)

How did Anthropic rewrite the API serving every Claude token from Python to Rust?

Today’s guest is Katelyn Lesse, who leads platform engineering at Anthropic and previously led engineering teams at Stripe. Her team owns the Claude API and the infrastructure behind products like Claude Code and Claude Desktop.

We start with Anthropic’s move from a Python monolith to Rust. Katelyn explains why adding CPU capacity is tricky due to a global CPU shortage, how Claude helped engineers get up to speed in Rust, and how the team used shadowing to verify the rewrite against production traffic. *Just today, Katelyn and the team published [a deepdive on their Python → Rust migration.](https://www.anthropic.com/research/migrating-api-to-rust)*

Katelyn walks us through what the software engineering job looks like today at Anthropic, and how code review and oncall interact with agentic workflows. We also get into the frontier lab’s “software factory”, how a product manager shipped a feature to production, and why Anthropic is replacing traditional coding interviews with agent-based exercises.

*As of last week, Anthropic is a publicly traded company, with a market capitalization of around $1T, all while having a tenth of the headcount of other businesses with similar market capitalizations. Learning how Anthropic does software engineering might be instructive for companies that want a similar structure.*

Here are 12 interesting learnings on how software engineering is changing inside of Anthropic:

[www.youtube.com](https://www.youtube.com/watch?v=9oNvKMd7xJo)

**1\. Anthropic rewrote its API from Python to Rust in around five months**. The API called ‘Messages API’ serves all Claude tokens externally, as well as for Claude Code and Claude Desktop. This service does authentication, tokenization, billing, and safety checks, then calls the GPU stack to generate tokens. The service was a Python monolith and the team initially optimized Python, but found the performance overhead was still large. Eventually, they decided to rebuild it in Rust. It took a few engineers and a few months in total; all without anyone being expert in Rust!

*The Anthropic team published a longer walkthrough of the migration – [check it out.](https://www.anthropic.com/research/migrating-api-to-rust)*

**2\. Hang on, why was the monolith written in Python in the first place**? Back in 2023, Anthropic was still a research company when it decided to create an API, and the team that built it was small, with a large crossover to Research. It was the case that researchers worked mostly with Python, which made it very easy to integrate the API with inference and other infrastructure code. Similarly, OpenAI’s API [was also written in Python](https://newsletter.pragmaticengineer.com/i/215812293/solving-load-challenges) – and later rewritten [to Rust](https://newsletter.pragmaticengineer.com/i/215812293/solving-load-challenges). Anthropic and OpenAI faced the same challenge with the Python monolith: the CPU footprint of the monolith started to become a growing problem.

**3\. The rewrite was done without Rust experts on the team or strict code reviews**. A year earlier, the same team picked Go for their new services because it was a language that team members knew how to read and write. But by 2026, they felt that Claude was good enough at Rust, and Claude helped engineers learn more about Rust during the rewrite. The team did not do strict code reviews, but had the “unfair” advantage of the existing Python API being a perfect test harness. They set up the new Rust API to shadow the current API, and iterated until all responses were identical. Also, Katelyn estimates that engineers looked closely at only ~20% of the code generated:

> “As long as you can really boil something down to make it verifiable in some way, then you don’t have to look at every single line of Rust code that’s getting written.”

**4\. Anthropic has its own code review agent running at all times**. The company has a Claude code review classifier running for all pull requests. It decides:

- **Does it need a human review?** For “mechanical” and low risk changes like an SDK version bump or using a newer model, Claude can decide there’s no human review needed. This is also the case for fixing flaky tests, in which case, another Claude does the review, then merges and deploys it.

- **How many human reviews does it need?** For changes that do need human reviews, Claude scores the risk and takes into account additional rules to decide how many human reviews are needed.

- **Which team needs to do a review**? Claude routes the PR to the right teams.

**5\. A Claude agent is paged before a human for API oncall**. For months, a Claude agent has been shadowing the API oncall rotation of the API team for training. As the team have gained confidence that Claude can tell if an alert is “real” or “noise,” they now let this agent categorize an incoming alert first. The long-term goal is for Claude to handle actual incidents without waking up a human – but they’re not there yet. *We cover more on how AI SRE startups are tackling the same challenge in our recent deepdive, [AI SRE: what is it, how could it change on-call – and does it work?](https://newsletter.pragmaticengineer.com/p/ai-sre)*

**6\. Onboarding for new Anthropic engineers is “talk to Claude**.” New joiners at tech companies used to have “onboarding buddies” and engineers who explained the architecture of systems to them, often by drawing boxes and diagrams. Not so at Anthropic, where Claude is connected to the codebase and internal systems, and engineers just ask it to explain how things work. Claude can even create interactive tutorials and visualizations. It’s pretty similar to how OpenAI engineers use Codex for onboarding, as covered in our [podcast episode with Tibo Sottiaux](https://newsletter.pragmaticengineer.com/p/building-codex-with-tibo-sottiaux).

**7\. One thing that hasn’t changed is the need for planning and alignment**. Katelyn’s team interacts with Inference, Research, Safety, and App teams. When kicking off a project, a lot of time and effort is spent on getting everyone on the same page. Katelyn says that planning time has not changed much from “pre-AI”. The average engineer actually spends more time coordinating than on implementation!

**8\. Another thing that has not changed is two-pizza teams**. Teams at Anthropic are still two-pizza teams of around 5-8 people. Katelyn says it’s because this is the right size for understanding a complex system and staffing an oncall rotation. Teams still own systems, and get paged. And it is a *team* – not an individual – that’s responsible for each system!

**9\. Engineers at Anthropic spend a lot more time building tooling than before**. Even on product teams, engineers are spending a lot more time building tools that help them to accelerate their own work. For example, an engineer on Katelyn’s team set up a Slack channel that runs a multi-agent team towards a single goal, even though it was not a task assigned to the engineer.

**10\. Knowing a programming language well is starting to not matter.** Katelyn invested a lot of time, pre-AI, into learning languages. I asked if it hurts as an engineer to consider that programming languages matter less these days, with AI. Katelyn pointed out that some engineers always cared little about languages, and for engineers who love languages, she says there’s still work to be done on languages and frameworks themselves.

**11\. Katelyn’s team phased out the coding interview after strong candidates kept failing it**. Candidates who crushed other interviews were starting to fail the coding one because they no longer write code by hand. In the absence of a set replacement, the team is actively experimenting with interviews conducted together with coding agents. The design (architecture) and project deepdive interviews remain unchanged.

**12\. For promotions, the “time to complete a project” is now meaningless**. Anthropic still has levels and a promotion process. But the time taken to completion is pretty meaningless when a year-long project can now be done in days or weeks with AI! Katelyn says that they fall back on evaluating the impact of projects; for example, how much CPU usage was saved, how much was latency reduced by, or how many incidents were avoided.

**Katelyn’s message to engineers with an identity crisis: humans have plenty of engineering to solve, related to agents**. She has written about this [in detail](https://www.katelynlesse.com/blog/new-problems-for-engineers-to-solve). The takeaway:

> “This is for the engineers feeling an identity crisis because they think their knowledge and skills won’t be important: you and your skills can be the difference in whether or not agentic infrastructure-related problems get tackled, and whether multiagent systems can solve the world’s hardest problems. You can and should go deep in these domains, learn new things, and form opinions. So much of this problem space is wide open for new solutions. It’s an incredibly exciting time to put your skills to work.”

**•** [How building software is changing at Anthropic](https://newsletter.pragmaticengineer.com/p/inside-anthropic)

**•** [How Claude Code is built](https://newsletter.pragmaticengineer.com/p/how-claude-code-is-built)

**•** [Inside OpenAI’s agentic software factory](https://newsletter.pragmaticengineer.com/p/openai-software-factory)

**•** [What is happening with code reviews?](https://newsletter.pragmaticengineer.com/p/what-is-happening-with-code-reviews)

**•** [When AI writes almost all code, what happens to software engineering?](https://newsletter.pragmaticengineer.com/p/when-ai-writes-almost-all-code-what)

**•** [AI SRE: what is it, how could it change on-call – and does it work?](https://newsletter.pragmaticengineer.com/p/ai-sre)

**•** [Migrations Done Well](https://newsletter.pragmaticengineer.com/p/migrations)

**•** [Inside Stripe’s Engineering Culture](https://newsletter.pragmaticengineer.com/p/stripe)

**•** [How Anthropic built Artifacts](https://newsletter.pragmaticengineer.com/p/how-anthropic-built-artifacts)

[00:00](https://www.youtube.com/watch?v=9oNvKMd7xJo) Intro

[03:09](https://www.youtube.com/watch?v=9oNvKMd7xJo&t=189s) Stripe

[07:04](https://www.youtube.com/watch?v=9oNvKMd7xJo&t=424s) How the Claude API works

[10:21](https://www.youtube.com/watch?v=9oNvKMd7xJo&t=621s) The Rust rewrite of the Claude API

[23:40](https://www.youtube.com/watch?v=9oNvKMd7xJo&t=1420s) Verifying the rewrite with shadow traffic

[31:11](https://www.youtube.com/watch?v=9oNvKMd7xJo&t=1871s) Onboarding new hires with Claude

[44:35](https://www.youtube.com/watch?v=9oNvKMd7xJo&t=2675s&pp=0gcJCWMAwfN6Pr3D) Building Claude Managed Agents

[53:59](https://www.youtube.com/watch?v=9oNvKMd7xJo&t=3239s) How engineering teams work at Anthropic

[58:43](https://www.youtube.com/watch?v=9oNvKMd7xJo&t=3523s) What agents handle on their own

[1:04:47](https://www.youtube.com/watch?v=9oNvKMd7xJo&t=3887s) Human code review

[1:06:41](https://www.youtube.com/watch?v=9oNvKMd7xJo&t=4001s) Oncall with Claude

[1:09:31](https://www.youtube.com/watch?v=9oNvKMd7xJo&t=4171s) Product managers shipping to production

[1:16:25](https://www.youtube.com/watch?v=9oNvKMd7xJo&t=4585s) Anthropic’s software factory

[1:24:13](https://www.youtube.com/watch?v=9oNvKMd7xJo&t=5053s) Phasing out traditional coding interviews

[1:28:24](https://www.youtube.com/watch?v=9oNvKMd7xJo&t=5304s) Levels, promotions, and performance reviews

[1:33:21](https://www.youtube.com/watch?v=9oNvKMd7xJo&t=5601s) The software engineer identity crisis

[1:47:23](https://www.youtube.com/watch?v=9oNvKMd7xJo&t=6443s&pp=0gcJCWMAwfN6Pr3D) Book recommendation

[1:48:12](https://www.youtube.com/watch?v=9oNvKMd7xJo&t=6492s) Closing takeaways

**Where to find Katelyn Lesse:**

• Website: [https://www.katelynlesse.com/](https://www.katelynlesse.com/)

• X:[https://x.com/katelyn_lesse](https://x.com/katelyn_lesse)

• LinkedIn:[https://www.linkedin.com/in/katelynlesse/](https://www.linkedin.com/in/katelynlesse/)

**Book recommendation:**

• Project Hail Mary by Andy Weir:[https://www.penguinrandomhouse.com/books/611060/project-hail-mary-by-andy-weir/](https://www.penguinrandomhouse.com/books/611060/project-hail-mary-by-andy-weir/)

**Mentions during the episode:**

• Ruby on Rails Tutorial by Michael Hartl: [https://www.railstutorial.org/](https://www.railstutorial.org/)

• Scaling Managed Agents with stable interfaces:[https://www.anthropic.com/research/managed-agents](https://www.anthropic.com/research/managed-agents)

• Anthropic’s Super Bowl campaign, Claude is a space to think:[https://www.anthropic.com/news/claude-is-a-space-to-think](https://www.anthropic.com/news/claude-is-a-space-to-think)

• What the CPU shortage means for software teams by Katelyn Lesse:[https://www.katelynlesse.com/blog/cpu-shortage-software-teams](https://www.katelynlesse.com/blog/cpu-shortage-software-teams)

• Why Rust is different, with Alice Ryhl:[https://newsletter.pragmaticengineer.com/p/why-rust-is-different-with-alice](https://newsletter.pragmaticengineer.com/p/why-rust-is-different-with-alice)

• Hunger beats tenure: Advice for junior engineers by Felix Rieseberg:[https://felixrieseberg.com/hunger-beats-tenure-advice-for-junior-engineers/](https://felixrieseberg.com/hunger-beats-tenure-advice-for-junior-engineers/)

• The new set of problems for engineers to solve by Katelyn Lesse:[https://www.katelynlesse.com/blog/new-problems-for-engineers-to-solve](https://www.katelynlesse.com/blog/new-problems-for-engineers-to-solve)

• Thariq’s post on how engineers and non-engineers feel about their skills:

> Thariq @trq212
>
> very weird thing happening rn technical people are sad, they think their skills are now useless non-technical people are desperately wishing they were more technical so they could use agents better
>
> [@trq212 on X](https://x.com/trq212/status/2115544056038350960)

**Technologies, tools, and vendors:**

• Claude Messages API:[https://platform.claude.com/docs/en/api/http/messages](https://platform.claude.com/docs/en/api/http/messages)

• Model Context Protocol (MCP): [https://modelcontextprotocol.io/](https://modelcontextprotocol.io/)

• Stripe Accounts v2:[https://docs.stripe.com/connect/accounts-v2](https://docs.stripe.com/connect/accounts-v2)

• Claude Managed Agents:[https://platform.claude.com/docs/en/managed-agents/overview](https://platform.claude.com/docs/en/managed-agents/overview)

• Claude Tag:[https://support.claude.com/en/articles/15594475-what-is-claude-tag](https://support.claude.com/en/articles/15594475-what-is-claude-tag)

• Claude prompt-cache diagnostics:[https://platform.claude.com/docs/en/build-with-claude/cache-diagnostics](https://platform.claude.com/docs/en/build-with-claude/cache-diagnostics)

• Redesigning source control at Anthropic (Scion):[https://www.anthropic.com/research/redesigning-source-control](https://www.anthropic.com/research/redesigning-source-control)

• Betterment: [https://www.betterment.com/](https://www.betterment.com/)

• Stripe Capital:[https://stripe.com/capital](https://stripe.com/capital)

—

Production and marketing by [Pen Name](https://penname.co/).
