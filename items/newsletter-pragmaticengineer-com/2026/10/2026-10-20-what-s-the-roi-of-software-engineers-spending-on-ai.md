---
title: What’s the ROI of software engineers’ spending on AI?
link: https://newsletter.pragmaticengineer.com/p/whats-the-roi-of-software-engineers
source: newsletter-pragmaticengineer-com
published: 2026-10-20T17:34:18Z
updated: 2026-10-20T17:34:18Z
first_seen: 2026-10-20T17:55:58.869157456Z
authors:
- Gergely Orosz
summary: AI spending has greatly increased at most companies this year, but has it actually changed shipping velocity and PR output? We analyse data from 27 companies – with some interesting findings
content: extracted
html: 2026-10-20-what-s-the-roi-of-software-engineers-spending-on-ai.html
remote_preview:
  url: https://substackcdn.com/image/fetch/$s_!hu5Y!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F10fd6c73-8cd3-4d75-8cd1-07df134a9b78_1600x1040.png
---

AI spending has greatly increased at most companies this year, but has it actually changed shipping velocity and PR output? We analyse data from 27 companies – with some interesting findings

A software engineer whose company is looking into its own spending on AI tools pinged me with a very timely question recently, asking what other companies spend on AI, per engineer. They were also keen to find out what the PR throughput was before and after the spending on AI at other places.

Spending on AI agents is ballooning at all the tech companies I talk with this year. With 2027 coming around fast, many companies are seeking to put a realistic number on how much they really spend on AI.

The change in PR throughput is a very simple metric, and one of those “readily-available” measurements at nearly all companies, It’s also one that probably has not been “gamed” by devs yet, and it’s possible to compare a “before AI” and “after AI” measurement with this metric.

To find out more and to try and answer the reader’s question, I asked around and got to talk with engineering leaders and senior+ engineers at 27 workplaces. I also gathered aggregate data from CodeRabbit and Gitar. This article summarizes what I found, covering:

1. **Stats from CodeRabbit and Gitar**. Weekly PRs by devs using the AI code review tool increased by around 2x compared to a year ago. Meanwhile, Gitar sees “AI-native” teams ship 60+ PRs/dev/week.

2. **AI spending patterns of 27 companies.** Spending caps are common, and there’s no connection between company size and AI spend.

3. **High spenders ($1,000+ per month**). A quant shop has seen AI costs increase 15x since January, while a two-person .NET shop spends $3,000/dev/month, plus details from six more companies.

4. **AI spend at Big Tech.** Meta and NVIDIA encourage devs to use AI heavily, while engineers at Apple are far more cautious.

5. **Medium spenders**. Details from five companies spending $300-500/month/dev.

6. **Low spenders**. Companies spending below $300/month/dev, many of which use subsidized subscriptions. Including details from a publicly traded company, a nonprofit, fintech [Lendscape](https://lendscape.com/) (UK), and startups [Lonestone](https://lonestone.io/) (France), [Paulina24](https://paulina24.de/) (Germany) and 5 others.

7. **What’s the ROI of AI?** A question that few companies seem able to answer – at least for now.

*The bottom of this article could be cut off in some email clients. [Read the full article uninterrupted, online.](https://newsletter.pragmaticengineer.com/p/whats-the-roi-of-software-engineers)*

[Read the full article online](https://newsletter.pragmaticengineer.com/p/whats-the-roi-of-software-engineers)

The team at [CodeRabbit](https://www.coderabbit.ai/) polled data engineers using the platform, anonymized it, and has kindly shared it with us. They saw a 113% increase in weekly PRs:

[![](https://substackcdn.com/image/fetch/$s_!9hsP!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff2f03ca3-c41e-450c-836f-979e28a67c1a_1600x1040.png)](https://substackcdn.com/image/fetch/$s_!9hsP!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff2f03ca3-c41e-450c-836f-979e28a67c1a_1600x1040.png)

On average, devs are creating more than twice as many PRs as they did a year ago. Source: CodeRabbit

There’s a 127% rise in monthly PRs:

[![](https://substackcdn.com/image/fetch/$s_!BnFh!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe83a6c97-c0b2-4344-9da1-1c972511433b_1600x1040.png)](https://substackcdn.com/image/fetch/$s_!BnFh!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe83a6c97-c0b2-4344-9da1-1c972511433b_1600x1040.png)

Monthly PRs have a very similar trajectory as weekly ones. Source: CodeRabbit

When tracking the same group of 20,000 developers, the increase is nearly identical across the platform: a 128% increase:

[![](https://substackcdn.com/image/fetch/$s_!hu5Y!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F10fd6c73-8cd3-4d75-8cd1-07df134a9b78_1600x1040.png)](https://substackcdn.com/image/fetch/$s_!hu5Y!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F10fd6c73-8cd3-4d75-8cd1-07df134a9b78_1600x1040.png)

Tracking 20,000 devs for a year. Source: CodeRabbit

**Across customer groups, there seems to be “tiers” of PR frequency**. Here’s data from [Gitar](https://www.sonarsource.com/products/gitar/) (Sonar’s AI code verification agent) on PRs/week/dev across hundreds of customers:

[![](https://substackcdn.com/image/fetch/$s_!ZSJl!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8e8ead51-c635-4dd4-82a0-9aa298a1490c_1018x426.png)](https://substackcdn.com/image/fetch/$s_!ZSJl!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8e8ead51-c635-4dd4-82a0-9aa298a1490c_1018x426.png)

Gitar’s categorization of teams, based on PR output, per dev, per week

I suspect that a lot has to do with company culture: whenever you join a new team, you pick up the pace pretty quickly. If the company is one where ~5 PRs per week is considered a high output, then new joiners get used to that pace. If it’s a place where most people ship 20-30 PRs to production per week, then new joiners pick up that pace!

I got details on AI spending and changes in the rate of PRs from 27 different companies. *I anonymized most names by request*. At a glance, here’s how spend looks like:

[![](https://substackcdn.com/image/fetch/$s_!siUW!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6907329f-9202-4621-96bf-db2c19605b15_990x724.png)](https://substackcdn.com/image/fetch/$s_!siUW!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6907329f-9202-4621-96bf-db2c19605b15_990x724.png)

*Spend split across the 27 companies*

A few common themes from this data set:

- **Company size and spend on AI are unrelated**. In the data set, a 2-person .NET startup spends $3,000/month/dev, and a publicly traded company with 1,500 engineers, limits devs to $150 of AI spend per month.

- **Spend peaked during the summer, and is falling**. June and July saw peak monthly spend at nearly all companies, and costs have roughly halved for those on API plans. A mix of Opus 5.5 and moving to open models is the reason for the drop at most companies.

- **Spending caps are common at medium-and-above spenders**. It’s normal to have a spending cap in place at medium-and-above spenders, where additional spend needs to be justified. Most people say the caps are easily avoided.

- **Subscriptions conceal concrete AI costs, but some subsidies are ending**. Several companies mentioned that their subsidized AI subscriptions are coming to an end, so they need to prepare to either optimize costs, or have their bill increase 5-10x.

- **Claude and Opus 5.5 seem to be the favorite models**. Within this rather limited data, Claude and Opus 5.5 dominated answers about where most of the spending goes. Cursor, Codex and GitHub Copilot were also mentioned.

Here are details of companies spending considerable amounts each month on AI coding tools.

This company has seen its AI spend balloon overall: total spend was **$8M** /month in September, a 16x increase on the $500K/month spend in January!

The average spend is **$1,600** /dev/month (up from around $80 in January). Most are going to Claude, and the biggest spenders are not devs, but traders. A current engineer there told me:

> “Lots of traders are using Claude. Engineering is learning the importance of rate limiting for traders who accidentally run expensive queries without realizing it.”

The company has a **$25,000 limit per employee** for AI, which is extremely high, more than at NVIDIA! The engineer explained this generous limit:

> “The last few years have, generally, been extremely profitable for trading firms. For us, last year was our most profitable year ever – and we’re already on track to beat it this year. So, the spend on AI is well within what these companies can afford.”

AI outlay is expected to grow: this firm is building its own internal GPU and CPU farms to run models, and are building models expected to beat state-of-the-art (SOTA) models like Fable, according to their own data.

**ROI is measured differently at trading firms**. The ROI of any spend is the profit or loss made by a trading desk, and AI spend is now part of the costbase. So, provided the desk’s profit increases, no questions are expected.

Outside of Big Tech where the monthly AI spend can be $10K+ for some people, the largest per-developer bill comes from a two-person .NET shop in Europe. This agency builds assessment games for a niche, global HR platform. Everything is built in C# and Blazor, running on Azure. A single tech stack was a great decision, especially now, they told me.

**$3,000** /month/dev (€2,500/dev/month) is what they spend on AI tools. Most of the spend goes on Claude Opus, and GitHub Copilot. One of the devs told me:

> “We barely type code anymore. We do a lot of feature development, but also a ton of backlog cleanup. Most of our systems are 5-10 years old.
>
> We do about 60 PRs/dev/week (so around 10-15/day). It’s amazing how fast we’re clearing out our ~10 years-old backlog.
>
> If you have high agency and know what to build, these are fun times!”

**PR frequency: up 5-10x**. As this engineer told me:

> “We used to do 1-2 PRs per day, and now it’s 10+. It feels like our two-person team is doing the work that would have taken about 5-7 people before AI.”

**$1,500** /dev/month is the default spending cap, but devs can ask for an increase. A current software engineer told me they spent $2,200/month in September. with Opus 5. With the latest version, Opus 5.5, this dev expects their spending to be cut in half.

**PR frequency has not much changed**. This engineer told me:

> “We aim to maintain the previous PR standard in terms of PR size and quality. We have, however, built an agent to review PRs and point out, e.g., large PRs, verbose code, and similar things, and that part is going well.”

**$1,200** /month/dev is the budget limit, but a Staff Engineer at the company reports that fewer than 5% of devs hit this mark.

**PR volume: it’s up (no data).** However, the velocity of PRs merged has not seemed to change much! From the same engineer:

> “We have a complicated codebase in my team and even frontier models like Opus 5.5 are still doing weird things when someone delegates thinking to them.
>
> We still do human PR reviews, and they catch a lot of stuff that agents and agentic review misses, like changes that are not needed at all, or those that are 10 times bigger than they should be.”

The engineer identified areas where AI has helped a lot:

- **Triaging** of customer issues. What previously took a whole day sometimes, is now finished in a few minutes with agent work and a maximum one hour of human verification.

- **Research**. Devs can get the info on why something was done in code based on Slack/design docs/PRs history, within minutes. Previously, this would take a day or two of manually digging into stuff.

- **Proof-of-concept (POC) prototyping**. Creating a POC with very complicated features can be done in a few hours for Product Managers to play with. Previously, this would take a week or two!

When the focus is on quality, velocity probably does not increase all that much. The engineer had this observation:

> “Overall, I personally see accelerated slop produced all around me. However, when the team cares about the quality in production code and wants to keep it like before AI, then actual development speed had not increased much.”

**$1,000** /dev/month spend is the average, according to an engineering lead. While not in place yet, the company is considering a $1,000/dev/month soft cap.

**PR frequency: 2x** In 2025 there were around 4 PRs/dev/week; Today, this is closer to 8 PRs/dev/week. The lead engineer observed a 2-3x increase in PRs by experienced engineers compared to a year ago.

**$1,000** /dev/month spending cap: this is the default, companywide, and can be spent on any provider, any way that devs want. It’s a “soft” spending cap and it’s trivial to request a larger budget: just fill out a short form to justify it, and it’s approved fast.

A current team lead shared with me the numbers they’re seeing:

- Most backend engineers use around $1,500/month

- Most team leads / Staff+ engineers are using around $2,500/month.

**PR frequency: flat.** The team lead observed no real increase compared to a year ago.

**PR size: 3x.** Meanwhile, PRs have exploded in size and have 3x as many lines of code per change request as before. I wonder if this could be attributed to LLMs being pretty verbose, even when coding.

**$2,000** /month/dev allowance for devs. This is through a Claude or Codex Max subscription, plus credits used.

This company has a public #ai-limits Slack channel, which publicly pings those nearing their limit, as well as top spenders and reminds them to rein in their AI usage, if possible; it sounds a bit like a “public embarrassment” strategy!

**Getting an increase is more complicated than at most companies**. As a current engineer explained to me:

> “When needing a limit increase, you run an AI skill that looks for common reasons why you might be burning tokens. It looks through things like whether you are running too many loops, or using subagents, and more. It gives suggestions on how to fix those.
>
> If the skill doesn’t find things to improve, then submit an approval request to the manager for a budget increase. Personally, I’ve rarely seen these increases granted, so everyone treats the $2K as a hard cap.”

**Velocity: unchanged**. The engineer told me they don’t see anything getting done faster than before:

> “We are perhaps marginally faster in a few places. AI has not fundamentally changed any of our processes, and I wonder if this is one reason. Most people are still sleeping on AI to be honest, and are adopting it very slowly. There are the odd outliers like myself and a few others who’ve been able to push a ton more, particularly on greenfield projects or anything without blockers around OSS.”

**$4,000**/dev/month maximum capped spend. This is a hard cap. Engineers at this company use GitHub Copilot and can choose between the models. Right now, Opus 5.5 is the top choice for devs across the company, a current team lead told me.

**$1,000** /dev/month is what the average spend feels like, though from this team lead looking around the org. People are not getting close to the $4K monthly cap.

**PR frequency: 4x** compared to the start of this year for a single team, led by a team lead, who told me:

> “We had about 1.5 PRs/dev/week in January, and this is around 4 PR/dev/week today. However, these numbers are skewed because we have one engineer whose productivity has skyrocketed, while the others have not seen all that much change.”

**Velocity: 3x more stories closed** than last year. This team lead has been using Scrum and Kanban for years at the company, which helps compare “business value delivered”, not just PRs. They told me:

> “For the last 5 years, we used Scrum + Kanban and always used a plan board. On average we close 3x more user stories per month since feb/march. It tells me we deliver more business value.
>
> And, for the first time ever we close more user stories than we have incoming! Keep in mind, this is all while not the whole team is there yet with AI.”

Some of the most “AI-pilled” places are in Big Tech, which is also home to some of the least “AI-pilled” approaches of all. Here are details about Meta, NVIDIA, and Apple:
