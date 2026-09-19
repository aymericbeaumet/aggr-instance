---
title: What is happening with code reviews?
link: https://newsletter.pragmaticengineer.com/p/what-is-happening-with-code-reviews
source: newsletter-pragmaticengineer-com
published: 2026-09-08T16:32:01Z
updated: 2026-09-08T16:32:01Z
first_seen: 2026-09-19T21:30:23.395188495Z
authors:
- Gergely Orosz
summary: AI generates more code than devs can track in 2026, so will the code review process have to adapt – or is it doomed? A look into this decades-old practice and the approaches that could replace it
content: extracted
html: 2026-09-08-what-is-happening-with-code-reviews.html
preview:
  file: 2026-09-08-what-is-happening-with-code-reviews.preview-90a923003327.webp
  width: 256
  height: 233
  color: '#f8f9fa'
images:
- source: https://substackcdn.com/image/fetch/$s_!SuQb!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0b62d5e2-7663-4910-8580-d565d3c7d0a4_1554x1416.png
  original:
    file: 2026-09-08-what-is-happening-with-code-reviews.image-3aab99b2bb9f.png
    width: 1554
    height: 1416
  variants:
  - file: 2026-09-08-what-is-happening-with-code-reviews.image-b195785df365.webp
    width: 320
    height: 292
  - file: 2026-09-08-what-is-happening-with-code-reviews.image-f3af058439f0.webp
    width: 640
    height: 583
  - file: 2026-09-08-what-is-happening-with-code-reviews.image-5eaa73cb8145.webp
    width: 960
    height: 875
  - file: 2026-09-08-what-is-happening-with-code-reviews.image-fc5ba8687620.webp
    width: 1280
    height: 1166
  - file: 2026-09-08-what-is-happening-with-code-reviews.image-7dd32aadca32.webp
    width: 1554
    height: 1416
  color: '#fdfdfd'
- source: https://substackcdn.com/image/fetch/$s_!ssZ-!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4146150e-65bc-4aa2-b140-a565bebbac68_1686x1048.png
  original:
    file: 2026-09-08-what-is-happening-with-code-reviews.image-ddce6b502a49.jpg
    width: 1456
    height: 905
  color: '#0f1410'
- source: https://substackcdn.com/image/fetch/$s_!gDPg!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fcd59c34e-d1f3-4caf-aaec-fc391478d03c_1714x1534.png
  original:
    file: 2026-09-08-what-is-happening-with-code-reviews.image-db14b5d0f933.jpg
    width: 1456
    height: 1303
  color: '#fdfdfd'
- source: https://substackcdn.com/image/fetch/$s_!gxRG!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F09ac3a7c-557e-4860-88ce-4a46958c8fd3_1808x772.png
  original:
    file: 2026-09-08-what-is-happening-with-code-reviews.image-e4baf9904ca6.jpg
    width: 1456
    height: 622
  color: '#fefefe'
- source: https://substackcdn.com/image/fetch/$s_!SuQb!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0b62d5e2-7663-4910-8580-d565d3c7d0a4_1554x1416.png
  original:
    file: 2026-09-08-what-is-happening-with-code-reviews.image-2486912eef0e.jpg
    width: 1456
    height: 1327
  color: '#fdfdfd'
---

One question haunting the minds of CTOs and heads of engineering whom I’ve been talking with, is how to deal with large quantities of code review which have only been growing now that AI agents generate most code at many tech companies.

Since the end of 2025, it has seemed that the era of devs writing code by hand [is over](https://newsletter.pragmaticengineer.com/p/when-ai-writes-almost-all-code-what) at startups and in Big Tech. AI agents work faster and generate more pull requests (PRs) than devs ever did, and the size of those pull requests is also increasing.

Today’s article summarizes some approaches to code review at various workplaces in this new paradigm, covering:

1. **Humans review the AI code reviews.** The most popular approach: AI code review tools go through code changes, and devs review the review itself.

2. **Triage by “blast radius” & decide an approach.** Low-risk changes don’t need human review, and high-risk ones do. Adopted by OpenAI, Anthropic, and others.

3. **Review the plan/tests/database schema, but not implementation.** Focus on reviewing the “before” and “after” states of an implementation, rather than the implementation itself.

4. **Produce less code.** Set up AI agents to produce smaller PRs that are easier to review and reason about.

5. **Review everything by hand.** Not everyone has adopted AI code review tools – even those that have sometimes still expect devs to read through all the new code, before allowing it to go to prod.

6. **No human code review?** There’s more talk about dropping human code reviews than there is evidence of this actually happening, so far. The most I could find was AI startups doing it and building additional layers for safer production rollouts.

7. **Why do we review code, anyway?** Before figuring out whether or not code review should stay, it’s worth going back to the fundamental technical, team, and organizational reasons for code reviews.

Unsurprisingly, it’s clear there’s no one-size-fits-all solution to the question of how to handle a deluge of AI-generated code review. *Please leave a comment below about how your team or company deals with this new, pressing issue!*

A snapshot of what’s going on in code review at this stage of AI development is provided by the graphic from GitHub, below. The background context it provides is pretty stark. It shows the stats for the number of PRs and commits over the course of three years on the popular platform:

[![](https://substackcdn.com/image/fetch/$s_!ssZ-!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4146150e-65bc-4aa2-b140-a565bebbac68_1686x1048.png)](https://substackcdn.com/image/fetch/$s_!ssZ-!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4146150e-65bc-4aa2-b140-a565bebbac68_1686x1048.png)

*Change in number of PRs, commits, and new repos across three years. Source: [GitHub](https://github.blog/news-insights/company-news/the-august-17-outage-and-the-work-ahead/)*

Over that time, the number of PRs opened has increased fivefold, which is a lot! And growth sped up from the end of 2025, when PRs and commits nearly doubled *just* in that period alone! So, how are teams dealing with this avalanche of extra work? To find out more, I asked around.

The most common approach is to add an AI code review step to every pull request in a variety of ways:

- **Use one or more vendors to review PRs.** There are dozens of vendors offering this functionality – ones like CodeRabbit, Gitar, Greptile, GitHub Copilot Code Review, Qodo, Claude Code Review, Ellipsis and more. Many teams choose one or more, and the bots then review PRs, leaving comments for devs. For example, the Bun project by Anthropic has CodeRabbit, GitHub Code Review, and Claude Code Review [all generating comments](https://github.com/oven-sh/bun/pull/41946) on PRs.

- **Multi-agent code review.** Build a custom solution which triggers several models/agents to review the code and suggest fixes.

- **Agents update PRs with fixes.** Vendors and home-grown solutions can instruct agents to update PRs with fixes and then re-trigger reviews – if you trust agents to make sensible fixes, that is!

Typical processes:

[![](https://substackcdn.com/image/fetch/$s_!gDPg!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fcd59c34e-d1f3-4caf-aaec-fc391478d03c_1714x1534.png)](https://substackcdn.com/image/fetch/$s_!gDPg!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fcd59c34e-d1f3-4caf-aaec-fc391478d03c_1714x1534.png)

AI code reviews increasingly part of the development cycle

In the above cases, engineers typically review the *review* itself, and not usually the code. Here’s Etienne Dilocker, cofounder and CTO at AI database software, Weaviate, [explaining](https://x.com/etiennedi/status/2096595963372552673?s=20) why he likes their approach:

> “**It’s very hard for agents to get the balance \[of the code review\] right.** If you ignore human code review entirely and leave it to agents, every PR will either suffer from scope creep or ship critical issues. But, of course, you can’t review everything by hand. So my current favorite setup is:
>
> 1\. an (adversarial) agent does a review
>
> 2\. a human makes a scope decision
>
> 3\. an agent implements the feedback
>
> 4\. either repeat or break the loop (likely a human decision)
>
> So basically, 90% is left to agents, with humans in the loop for critical scope decisions and exit criteria.”

**Noise is a big problem with AI code reviews.** WeTravel, a Series C travel tech company, [decided](https://newsletter.pragmaticengineer.com/i/186760687/5-how-one-company-accurately-measures-code-review-usefulness) to not use AI for code reviews because of the amount of noise it generated. In June, they did an updated evaluation which showed lots of improvement, but still not enough to justify adopting AI for the task.

As things stand, custom tooling is probably needed to reduce code-review noise. Uber built a clever approach for this; an agentic pipeline called [uReview:](https://newsletter.pragmaticengineer.com/i/190542498/ureview-a-custom-code-review-tool)

[![](https://substackcdn.com/image/fetch/$s_!gxRG!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F09ac3a7c-557e-4860-88ce-4a46958c8fd3_1808x772.png)](https://substackcdn.com/image/fetch/$s_!gxRG!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F09ac3a7c-557e-4860-88ce-4a46958c8fd3_1808x772.png)

What uReview does:

- Bots generate lots of code review comments

- Comments are graded, and low-confidence comments removed

- Comments are merged, categorized, and unimportant ones removed

- … in the end, the AI review results in important comments being shown to devs

Another common approach is to decide whether to review code by hand or with AI, based on how “risky” a change is:

- **Low-risk change:** only AI, without human review. It can ship to production once AI agents are happy

- **High-risk change:** mandatory human review

This is the approach that Anthropic and OpenAI follow, which I confirmed by talking with both companies. At Anthropic, Jarred Sumner [told me](https://newsletter.pragmaticengineer.com/i/208848940/code-review-and-more-testing-by-ai) that a human merges even low-risk changes, but that their goal is eventually to get another Claude instance to merge low-risk changes.

And it’s not just at leading AI labs: fifteen-person startup, Duckbill, a cloud and AI compute and contracts management company, changed their process, as [explained](https://x.com/mikejulian/status/2096450476170694785?s=20) by cofounder and CEO Mike Julian:

> “We ditched code review at Duckbill Group (mostly)
>
> About a month ago, we found ourselves with 60 open PRs for a team of five. They had been accumulating for a few weeks and we all had the sudden realization we were looking at two days of just code review.
>
> I had been tossing around the idea for a while about having AI do all code review and so I just asked the team: what if we just didn’t review the PRs?
>
> We decided to do a couple of things:
>
> - **Switch to a risk-based system.** With a risk-based system, we agreed that if your change touched the public API/MCP, auth, design system, non-additive database schema changes, or agent skills, it needed a human review. We then enforced that with a shell script to add a GitHub label.
>
> - **Improve our guardrails** (unit and end-to-end testing, post-deploy observability, stricter linting and type checking, etc). Improving guardrails was pretty easy, just expensive in tokens and attention. We enabled nearly every rule in ruff/prettier/eslint/ty, and we improved our unit test coverage to a floor of 85%.
>
> Results before vs after:
>
> - PRs merged: 353 → 684 (80/wk → 154/wk, +94%)
>
> - Merged within 1h: 28% → 45%; within 24h: 76% → 80%
>
> - Human-reviewed PRs median merge time: 26h
>
> - No human-review median merge time: 1h.”

Here’s how I’d visualize this approach:

[![](https://substackcdn.com/image/fetch/$s_!SuQb!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0b62d5e2-7663-4910-8580-d565d3c7d0a4_1554x1416.png)](https://substackcdn.com/image/fetch/$s_!SuQb!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0b62d5e2-7663-4910-8580-d565d3c7d0a4_1554x1416.png)

Selecting a code review approach by “blast radius”

Some companies have built additional tooling to make it easier for devs to know which reviews to focus on. For example, Uber’s custom-built Code Review Inbox highlights high-impact changes, so devs know to spend more time and effort on them:

Some devs and teams have stopped reviewing the code (the implementation), and instead review the “before” and “after” states:

**Review the plan:** spend a lot more time on the plan than before, to get a much more detailed spec. Using [The /grill-me skill](https://www.aihero.dev/skills-grill-me) by Matt Pocock is a popular method, and I’m also a fan of it for thorough upfront planning, [as is](https://x.com/andreafspeziale/status/2096667130582216767?s=20) Andrea Francesco Speziale, Principal Engineer at Musixmatch:

> “After 3 hours of /grill-me, it better one-shot the implementation. I’m not spending a single minute on any review!”

**Review the tests:** via Test Driven Development (TDD) – which is much easier with agents when writing the tests upfront is a chore – or by focusing the review to ensure the software is tested.

One argument for this approach is that customers and users of software usually don’t care about the code. There’s a caveat that automated tests can verify a lot of different software – and are great at verifying business logic – but they don’t do a good job at verifying whether a UI *looks* and *feels* good.

**Review the database schema.** Jackie Luo, cofounder and CEO of AI startup Sigil, and formerly an engineer at Square, says:

> “My current take is that all that really matters is the database schema. Speaking from a fast-moving startup perspective:
>
> 1\. Everything, besides data, is fluid and recoverable.
>
> 2\. The schema is the “hard” representation of what’s been built and reveals the riskiest changes, so it’s a good attention/impact tradeoff.
>
> 3\. Business logic only matters because product behavior matters—so ideally align on that before interacting with a coding agent at all. Then, once the code is written, use abstractions to understand any other significant decisions made.
>
> Understand the product over the code. Use abstractions to translate the latter to the former – except in the case of schemas!”

Jackie’s point is that data (that is, the state) is the most “rigid” part of any system. Stateless business logic is now easy to change because it’s “just” code, and code is easy and fast to generate and regenerate. For startups, it’s worth getting the data schema – and thereby your state machine – right. Then, everything else will be easy and fast to iterate on.

*My sense is this approach makes perfect sense for a startup iterating to get product-market fit. However, once you have a business, you’ll want to “guard” the business logic with tests: else your product could break, and existing users will be unhappy when this happens!*
