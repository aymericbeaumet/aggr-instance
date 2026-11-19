---
title: Software factories and code reviews with Harjot Gill
link: https://newsletter.pragmaticengineer.com/p/software-factories-and-code-reviews
source: newsletter-pragmaticengineer-com
published: 2026-11-18T18:08:52Z
updated: 2026-11-18T18:08:52Z
first_seen: 2026-11-19T00:14:19.077761094Z
authors:
- Gergely Orosz
summary: CodeRabbit CEO Harjot Gill explains how he shipped over 5,000 pull requests in a month, why he believes human code review is dead, and what he gets from spending $250,000 a month on AI.
content: extracted
html: 2026-11-18-software-factories-and-code-reviews-with-harjot-gill.html
remote_preview:
  url: https://substackcdn.com/image/youtube/w_728,c_limit/4Cjb7691GJ8
extra:
  audio_type: audio/mpeg
  audio_url: https://api.substack.com/feed/podcast/225268679/5269cb4d06b91eb4208035b19c1b1dd8.mp3
  duration_seconds: 5198
---

**Listen and watch now on [YouTube](https://youtu.be/4Cjb7691GJ8), [Apple](https://podcasts.apple.com/us/podcast/the-pragmatic-engineer/id1769051199), and [Spotify](https://open.spotify.com/show/2Bho9xCbOQMWMJ7UKmqCzD).** See the episode transcript at the top of this page, and timestamps for the episode at the bottom.

![](https://substackcdn.com/image/fetch/$s_!jCup!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff3b0c36f-48f4-4d26-a750-244e2f6e2e66_1600x140.png)

• **[turbopuffer](http://turbopuffer.com/pragmatic)** – a ridiculously scalable, fast and cheap hybrid search engine, built on top of object storage, used by cutting-edge companies like Anthropic, Linear, Ramp and many others. Many companies that switch to turbopuffer are cutting their search bills by 50% or more and getting much faster results. [Check it out.](http://turbopuffer.com/pragmatic)

• **[Entire](https://entire.io/pragmatic)** – Entire [launched](https://entire.com/about?utm_source=pragmatic#code-hosting) its own Git forge, shipped [Continuous Memory,](https://entire.com/about?utm_source=pragmatic#continuous-memory) and reinvented the pull request (called [Trails](https://entire.com/about?utm_source=pragmatic#trails)). Trails are a much more natural workflow when agents keep pushing new changes, after iterating on feedback – and you have more context and control on when a change is ready to merge. [Check it out](https://entire.com/pragmatic)

• **[WorkOS](https://workos.com/)** – WorkOS built Airlock, the authorization layer for AI agents. It evaluates every request against the agent’s intent + your rules, and either allows it, denies it, or routes it to a human. Works with Claude Code, Codex & MCP gateways. [Learn more](http://workos.com/airlock)

In this conversation, we step into the world of an AI-pilled CEO to explore how their startup works. This episode might as well be a way of succeeding that contradicts the growing convention that AI spending costs are too high and must be clamped down on.

[Harjot Gill](https://x.com/harjotsgill) is founder and CEO of [CodeRabbit](https://www.coderabbit.ai), an AI code review business, and he shipped over 5,000 pull requests last month, spending on AI at a monthly run rate of $250,000(!) It’s certainly not a conventional approach and Harjot gets into detail about the thinking and reasoning behind it.

In this episode of *The Pragmatic Engineer Podcast*, we talk about his background in building software and founding infra startups for the last 30 years, and how things changed into the era of building software factories at CodeRabbit. We talk about why he believes human code review is dead, and how his team maintains quality while shipping at unprecedented speed.

We also get into the rising costs of AI development, why Harjot is convinced that tech startups need more funding than ever, and how AI is changing recruitment and the skills for success.

[www.youtube.com](https://www.youtube.com/watch?v=4Cjb7691GJ8)

**1\. Janitorial work formed a large part of his 5,000 PRs last month, pushed with agents.** He sets off his agents to improve systems like the CI, fixing flaky tests, and refining caching. A lot of the PRs are generated during his commute, and at evenings and nights. In a few cases, he steers agents to make the UI feel better. Examples of stuff Harjot has built:

- A [write-ahead log](https://en.wikipedia.org/wiki/Write-ahead_logging) (WAL) on top of object stores using Rust (the agent ran for six weeks!). This WAL is now in production.

- Migrating all of CodeRabbit’s services over to Bun – running over a weekend

- A new CI system after the previous one had performance issues

**2\. CodeRabbit spends millions of dollars per month on AI, with a team of 40 engineers**. Harjot estimates that a PR costs him around $150 in a personal spend of around $250-$275K per month. He compares it to marketing teams with 10 marketers at VC-funded companies who can drive $100M in spend. *I point out that his startup is an outlier; the AI spend I hear about elsewhere is in the region of between [$200-$2,000 per month, per dev](https://newsletter.pragmaticengineer.com/p/whats-the-roi-of-software-engineers). Finding out how this spend makes sense was one reason I was keen to meet Harjot.*

**3\. For dev tooling startups like CodeRabbit, massive AI spend can be justified by launching new, AI-related products faster**. It feels to me that CodeRabbit is not a typical company: its AI spending is a form of R&D if it creates new solutions that can be commercialized and sold. It’s not the same at places with fixed customer bases willing to pay a certain amount for an existing product, and no more.

Also, Harjot says that they got quoted $13M/year for a CI solution to handle their PR output, but the price made him decide they’d build their own CI system using agents. It took a week, and the new CI cost 50% less than the quote, with similar performance.

**4\. In 2021, Harjot believed AI would write most of the code**. When working at FluxNinja, Harjot found that code review was already becoming a bottleneck, with GitHub Copilot’s autocompletions somewhat speeding up writing code. He misjudged the potential market size, assuming only larger companies that bought code verification solutions like Snyk and SonarSource, would buy AI-code review tools – instead, it’s pretty much everybody!

**5\. Harjot believes agents need to be “hill climbing**”; they need to have ways to verify their own work

These are the layers that he believes are necessary, including ones he and the team use:

- **Formal models like TLA+ and Lean.** For one of their products, the engineering team utilized TLA+, and found race conditions that the code review overlooked. More recently, Harjot is experimenting with another formal method, Lean.

- **Local deterministic simulations.** One of the infra pieces built at Harjot’s startup is a simulation harness to fuzz different interleavings (concurrent operations).

- **Emulating real APIs**. Harjot mocks up and emulates APIs like Google Cloud APIs as part of testing.

- **Dev environments**. Agents can spin up services in environments like Google Cloud and Vercel, and confirm they work correctly on development deployments

- **Production: staged rollouts.** Harjot believes in releasing features with feature flags, and doing progressive rollouts. He said they often have an agent observe the performance characteristics of the rollout, check the logs, and continue the wider rollout when things are stable. *This last part reminds me of OpenAI’s [Perf Factory](https://newsletter.pragmaticengineer.com/i/215812293/3-openais-agentic-software-factory) inside [its software factory](https://newsletter.pragmaticengineer.com/i/215812293/3-openais-agentic-software-factory).*

**6\. Human code review is dead, long live the human review**! Harjot thinks that we will stop reviewing code generated by agents. Instead, we’ll spend more time reviewing the planning and final asset. Inside of CodeRabbit, engineers are reviewing *releases* (multiple PRs), not individual PRs.

**7\. Engineers can be banned from committing without human review**. The company has a “review exempt list” of devs who can push to production without anyone reviewing their releases. When an outage happens, the team traces the PR that broke prod, and the dev who pushed it gets a ‘strike.’ Rack up enough strikes and releases have to be reviewed by colleagues. *I personally like this idea of giving trust to devs to push to prod, but pairing it with accountability: the trust gets revoked if you cause production to be unstable!*

**8\. Fine-tuning models stopped being worthwhile**. CodeRabbit used to invest in fine-tuning models used in AI review, but Harjot says that new frontier models moving faster means that it would be tough to maintain their own fine-tuned ones, so it’s not worth it.

**9\. There are “war rooms**”. To get important stuff done, engineers, product folks, and designers gather in the room and steer the agents as together they build a prototype, then a working product. *This approach reminds me of Uber in 2016, except ‘war room’ efforts took 2-3 weeks, people flew in from different offices, and it meant aligning on the plan and putting a rudimentary prototype together. The teams finished the remaining work over a matter of months.*

**10\. What Harjot looks for when hiring engineers**: agency, breadth of experience, and systems thinking. He posted an [unconventional job ad](https://lnkd.in/p/guZiRvTk), only inviting candidates who had shipped 1,000+, non-slop PRs in the past 30 days, or had played 500+ hours of strategy and management games like Civilization, Age of Empires, Transport Tycoon, or Factorio. Harjot says he cannot unsee the similarities between running dozens of agents in parallel and playing a strategy video game involving tracking what dozens of units do.

This conversation gave me lots to think about. My kneejerk reaction was to question why anyone would spend so much on AI each month; I would assume it’s just wasteful. So, I’m delighted to learn more about where Harjot is coming from. I sense he is pushing the limit of what a single human developer can do when running hundreds of agents in parallel, and he *still* has more ideas than can be built. Plus, he doesn’t have to worry about his AI spending budget!

**Could spending be much cheaper for the same usage?** Harjot and CodeRabbit are paying frontier *API costs*, not using open models. We cover how profitable coding use cases are for inference vendors and frontier companies [due to KV cache nuances](https://newsletter.pragmaticengineer.com/i/225360725/2-an-aside-kv-cache-is-why-inference-is-so-profitable-for-vendors) in this week’s [inference deepdive](https://newsletter.pragmaticengineer.com/p/inference-engineering-for-software-engineers), and how the $200/month Max plans for individuals and small businesses offer usage closer to $8,000-14,000/month in API usage, due to the same [KV cache profitability detail.](https://newsletter.pragmaticengineer.com/i/225360725/2-an-aside-kv-cache-is-why-inference-is-so-profitable-for-vendors) Moving to open models could *surely* reduce a good part of this spend, but at the cost of some capability.

**The engineer + strategy gamer combination helps push AI agent usage**? Harjot is not some nontechnical “vibe coder”: he’s been coding for 30 years, and mostly used Neovim before moving over to AI agents. He’s long been obsessed with infra and distributed systems, and in his younger years, played Civilization. He detects parallels between the two activities and it’s interesting that he incorporated them into a job description.

**The “Dracula effect” is real with AI agents**. I often see the “Dracula effect” of AI agents draining people, like Steve Yegge [mentioned](https://newsletter.pragmaticengineer.com/i/187563524/4-the-dracula-effect-the-drain-of-using-ai) in February. Harjot also reports disturbed sleep, like waking up at night to nudge his agents.

**Well-funded AI-pilled startups could enjoy a massive upside of building with AI**. Over the course of our conversation, I started to understand why it makes sense for Harjot and CodeRabbit to spend so much on AI. They need to build solutions that AI-native startups want to use, and CodeRabbit being AI-pilled itself confers the advantages of firsthand insights and empathy with customers. There’s much that can be learned from how Harjot and CodeRabbit work, all while this approach looks set to remain niche at a time when most companies are tightening the reins on AI spend.

CodeRabbit’s business is being an AI infra startup, and the time-to-market with new products is a massive differentiator. In that context, it makes sense for CodeRabbit to build a lot of software *internally* and then productionize some of it, contributing to their revenue and valuation possibly increasing down the road. *I find it useful to learn about such approaches, even if you decide that this direction would not be feasible for yourself, your team, or company.*

**•** [Inside OpenAI’s agentic software factory](https://newsletter.pragmaticengineer.com/p/openai-software-factory)

**•** [What is happening with code reviews?](https://newsletter.pragmaticengineer.com/p/what-is-happening-with-code-reviews)

**•** [What’s the ROI of software engineers’ spending on AI?](https://newsletter.pragmaticengineer.com/p/whats-the-roi-of-software-engineers)

**•** [Why Ramp built its own in-house coding agent, Inspect](https://newsletter.pragmaticengineer.com/p/why-ramp-built-inspect)

**•** [AI SRE: what is it, how could it change on-call – and does it work?](https://newsletter.pragmaticengineer.com/p/ai-sre)

**•** [Context engineering with Dex Horthy](https://newsletter.pragmaticengineer.com/p/context-engineering-with-dex-horthy)

**•** [Formal methods with Hillel Wayne](https://newsletter.pragmaticengineer.com/p/formal-methods-with-hillel-wayne)

**•** [Building Codex with Tibo Sottiaux](https://newsletter.pragmaticengineer.com/p/building-codex-with-tibo-sottiaux)

[00:00](https://www.youtube.com/watch?v=4Cjb7691GJ8) Intro

[03:17](https://www.youtube.com/watch?v=4Cjb7691GJ8&t=197s) How Harjot shipped over 5,000 pull requests in a month

[08:46](https://www.youtube.com/watch?v=4Cjb7691GJ8&t=526s) Harjot’s path into tech and his first startups

[12:25](https://www.youtube.com/watch?v=4Cjb7691GJ8&t=745s) Early conviction around AI and founding CodeRabbit

[14:40](https://www.youtube.com/watch?v=4Cjb7691GJ8&t=880s) Switching to AI coding

[20:35](https://www.youtube.com/watch?v=4Cjb7691GJ8&t=1235s) Software factories explained

[25:37](https://www.youtube.com/watch?v=4Cjb7691GJ8&t=1537s) What’s replacing human code review

[38:44](https://www.youtube.com/watch?v=4Cjb7691GJ8&t=2324s&pp=0gcJCWMAwfN6Pr3D) Technical debt and deslopping tests

[46:05](https://www.youtube.com/watch?v=4Cjb7691GJ8&t=2765s) Evals

[50:25](https://www.youtube.com/watch?v=4Cjb7691GJ8&t=3025s&pp=0gcJCWMAwfN6Pr3D) Formal methods

[52:05](https://www.youtube.com/watch?v=4Cjb7691GJ8&t=3125s) AI cost at CodeRabbit

[54:57](https://www.youtube.com/watch?v=4Cjb7691GJ8&t=3297s&pp=0gcJCWMAwfN6Pr3D) Startup funding in 2026 and the value of AI spend

[1:08:20](https://www.youtube.com/watch?v=4Cjb7691GJ8&t=4100s) Hiring

[1:13:03](https://www.youtube.com/watch?v=4Cjb7691GJ8&t=4383s) Work pace, sleep, and burnout

[1:21:55](https://www.youtube.com/watch?v=4Cjb7691GJ8&t=4915s) Skills for the AI era

[1:24:30](https://www.youtube.com/watch?v=4Cjb7691GJ8&t=5070s) Book recommendation

**Where to find Harjot Gill:**

• X: [https://x.com/harjotsgill](https://x.com/harjotsgill)

• LinkedIn: [https://www.linkedin.com/in/harjotsgill](https://www.linkedin.com/in/harjotsgill)

**Book recommendation by Harjot The Black Swan by Nassim Nicholas Taleb**:[https://www.penguinrandomhouse.com/books/176226/the-black-swan-second-edition-by-nassim-nicholas-taleb/](https://www.penguinrandomhouse.com/books/176226/the-black-swan-second-edition-by-nassim-nicholas-taleb/)

**Mentions during the episode:**

- Harjot’s post on his pull request stats and token spend:

> Harjot Gill @harjotsgill
>
> I shipped roughly 3000 PRs in a month (similar ballpark as @poteto) and my spend is roughly ~$200K a month in tokens. The value created by all that code is in millions, if not in billions!
>
> > Theo - t3.gg @theo
> >
> > I got some issues with this. The $132m/year is not real at all. Closer to $3m at today's prices, and as low as $1.2k within a year. Yes, $132m -> $1.2k. Hear me out. 1. The price listed is not accurate at all. $11m/month is just wrong. Real "api cost" is prolly $100k-$500k https://t.co/SYv99MvfTY
>
> [@harjotsgill on X](https://x.com/harjotsgill/status/2107961503173472317)

![](https://pbs.substack.com/media/HUDVbN4bgAAg_3t.png)

10:29 PM · Oct 7, 2026 · 10.6K Views

6 Replies · 3 Reposts · 49 Likes

- Harjot’s dotfiles:[https://github.com/coderabbitai/dotfiles](https://github.com/coderabbitai/dotfiles)

- Tesla Full Self-Driving (Supervised):[https://www.tesla.com/fsd](https://www.tesla.com/fsd)

- AI tools for software engineers, but without the hype – with Simon Willison (co-creator of Django):[https://newsletter.pragmaticengineer.com/p/ai-tools-for-software-engineers-simon-willison](https://newsletter.pragmaticengineer.com/p/ai-tools-for-software-engineers-simon-willison)

- Alcatel-Lucent:[https://en.wikipedia.org/wiki/Alcatel-Lucent](https://en.wikipedia.org/wiki/Alcatel-Lucent)

- University of Pennsylvania: [https://www.upenn.edu](https://www.upenn.edu)

- When AI writes almost all code, what happens to software engineering?:[https://newsletter.pragmaticengineer.com/p/when-ai-writes-almost-all-code-what](https://newsletter.pragmaticengineer.com/p/when-ai-writes-almost-all-code-what)

- Git at any scale (Cursor’s post on Origin):[https://cursor.com/blog/git-at-any-scale](https://cursor.com/blog/git-at-any-scale)

- Write-ahead logging:[https://en.wikipedia.org/wiki/Write-ahead_logging](https://en.wikipedia.org/wiki/Write-ahead_logging)

- What is SOC 2 Compliance?:[https://www.paloaltonetworks.com/cyberpedia/soc-2](https://www.paloaltonetworks.com/cyberpedia/soc-2)

- Jake Runzer’s post on the Railway incident:

> Jake @jakerunzer
>
> @JustJake @joshmanders @Railway @ngeloxyz Ah yup. That was indeed me. No mallace intended. I've been running some agents to do signups + agentic testing on similar platforms. Just halted the harness I was designing around this as it was definitely going off the rails and reaching too far
>
> [@jakerunzer on X](https://x.com/jakerunzer/status/2117474337682780643)

- The Pulse: a new trend of CPU shortages:[https://newsletter.pragmaticengineer.com/p/the-pulse-a-new-trend-of-cpu-shortages](https://newsletter.pragmaticengineer.com/p/the-pulse-a-new-trend-of-cpu-shortages)

- How Kubernetes is Built with Kat Cosgrove:[https://newsletter.pragmaticengineer.com/p/how-kubernetes-is-built-with-kat](https://newsletter.pragmaticengineer.com/p/how-kubernetes-is-built-with-kat)

- Formal methods with Hillel Wayne:[https://newsletter.pragmaticengineer.com/p/formal-methods-with-hillel-wayne](https://newsletter.pragmaticengineer.com/p/formal-methods-with-hillel-wayne)

- Harjot’s post with CodeRabbit’s weekly commits chart:

> Harjot Gill @harjotsgill
>
> This week we didn’t see an uptick in shipping velocity. We stalled due to CI capacity issues with our provider, and to make things worse, even our cloud providers have run out of capacity to host our runners. VCs are getting excited about funding faster/cheaper runners while the…
>
> > Harjot Gill @harjotsgill
> >
> > We at @coderabbitai shipped 5K PRs last week. Our tiny team is shipping more than a thousand-person dev team. And we are nowhere near hitting our maximum potential. If we industrialize software development correctly, 50K PRs a week are totally achievable, if not more. At this https://t.co/CjtLhs3oII
>
> [@harjotsgill on X](https://x.com/harjotsgill/status/2114823385872224373)

![](https://pbs.substack.com/media/HVFzpLQbMAApduL.png)

8:56 PM · Oct 26, 2026 · 16.2K Views

16 Replies · 5 Reposts · 58 Likes

- Harjot’s post on $8M+ seed rounds no longer being enough:

> Harjot Gill @harjotsgill
>
> What we are seeing at @coderabbitai and elsewhere on this parabolic trajectory is that even larger seed rounds ($8M+) are no longer enough to build a software business. Software factories need quality tokens and large-scale compute to run CI, sandboxes, and other infrastructure…
>
> [@harjotsgill on X](https://x.com/harjotsgill/status/2115932303528747189)

- Why Ramp built its own in-house coding agent, Inspect:[https://newsletter.pragmaticengineer.com/p/why-ramp-built-inspect](https://newsletter.pragmaticengineer.com/p/why-ramp-built-inspect)

- Spotify Confidence: [https://confidence.spotify.com](https://confidence.spotify.com)

- Harjot’s post on shipping 5K PRs in a week, with CodeRabbit’s hiring criteria:[https://www.linkedin.com/posts/harjotsgill_we-at-coderabbit-shipped-5k-prs-last-week-share-7518364258942758912-Bts_/](https://www.linkedin.com/posts/harjotsgill_we-at-coderabbit-shipped-5k-prs-last-week-share-7518364258942758912-Bts_/)

- Why We Sleep by Matthew Walker:[https://www.amazon.com/dp/1501144324](https://www.amazon.com/dp/1501144324)

- Polyphasic sleep:[https://en.wikipedia.org/wiki/Polyphasic_sleep](https://en.wikipedia.org/wiki/Polyphasic_sleep)

- Harjot’s post on burnout:

> Harjot Gill @harjotsgill
>
> @GergelyOrosz CodeRabbit is my third startup now and I have worked just as hard for my previous two (Netsil and FluxNinja). Burnout never happens due to hard work. It kicks in when hard work doesn’t lead to the desired outcome for a long time. On the family front, my two daughters were born…
>
> [@harjotsgill on X](https://x.com/harjotsgill/status/1956001908147282390)

- Inside Fast’s rapid collapse:[https://newsletter.pragmaticengineer.com/p/the-scoop-fast](https://newsletter.pragmaticengineer.com/p/the-scoop-fast)

**Technology, tool and vendor mentions:**

- CodeRabbit: [https://www.coderabbit.ai](https://www.coderabbit.ai)

- Codex:[https://chatgpt.com/codex](https://chatgpt.com/codex)

- Opus:[https://www.anthropic.com/claude/opus](https://www.anthropic.com/claude/opus)

- Claude Code:[https://claude.com/product/claude-code](https://claude.com/product/claude-code)

- Neovim: [https://neovim.io](https://neovim.io)

- Netsil:[https://www.linkedin.com/company/netsil](https://www.linkedin.com/company/netsil)

- New Relic: [https://newrelic.com](https://newrelic.com)

- AppDynamics:[https://en.wikipedia.org/wiki/AppDynamics](https://en.wikipedia.org/wiki/AppDynamics)

- eBPF: [https://ebpf.io](https://ebpf.io)

- Nutanix: [https://www.nutanix.com](https://www.nutanix.com)

- FluxNinja: [https://www.fluxninja.com](https://www.fluxninja.com)

- Tabnine:[https://en.wikipedia.org/wiki/Tabnine](https://en.wikipedia.org/wiki/Tabnine)

- GitHub Copilot:[https://github.com/features/copilot](https://github.com/features/copilot)

- LTX (SQLite replication file format):[https://github.com/superfly/ltx](https://github.com/superfly/ltx)

- Effect: [https://effect.website](https://effect.website)

- XState:[https://stately.ai/docs/xstate](https://stately.ai/docs/xstate)

- OrbStack: [https://orbstack.dev](https://orbstack.dev)

- Snyk: [https://snyk.io](https://snyk.io)

- SonarSource: [https://www.sonarsource.com](https://www.sonarsource.com)

- TLA+:[https://lamport.azurewebsites.net/tla/tla.html](https://lamport.azurewebsites.net/tla/tla.html)

- Lean: [https://lean-lang.org](https://lean-lang.org)

- Sid Meier’s Civilization: [https://civilization.2k.com](https://civilization.2k.com)

- Datadog: [https://www.datadoghq.com](https://www.datadoghq.com)

- Devin: [https://devin.ai](https://devin.ai)

- Bun: [https://bun.com](https://bun.com)

- Rippling: [https://www.rippling.com](https://www.rippling.com)

- Age of Empires: [https://www.ageofempires.com](https://www.ageofempires.com)

- Transport Tycoon: [https://www.transporttycoon.com](https://www.transporttycoon.com)

- Factorio: [https://www.factorio.com](https://www.factorio.com)

—

Production and marketing by [Pen Name](https://penname.co/).
