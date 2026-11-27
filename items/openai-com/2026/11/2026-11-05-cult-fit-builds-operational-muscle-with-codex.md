---
title: Cult.fit builds operational muscle with Codex
link: https://openai.com/index/cultfit-builds-with-codex
source: openai-com
published: 2026-11-05T20:00:00Z
updated: 2026-11-05T20:00:00Z
first_seen: 2026-11-06T03:37:44.957751002Z
summary: Cult.fit is using Codex to help teams across functions build tools, move faster, and change how work gets done.
content: extracted
html: 2026-11-05-cult-fit-builds-operational-muscle-with-codex.html
---

Cult.fit is one of India’s leading technology-enabled fitness companies, operating 708 fitness centers across 77 cities and serving nearly one million paid members. Its business combines subscription-based access to gyms and trainer-led fitness classes with a growing consumer products portfolio spanning fitness equipment, accessories, workout footwear and activewear.

As the company grew, it wanted to solve more operational and product challenges without adding layers of process and complexity. By standardizing engineering on Codex and then extending it beyond engineering, cult.fit is helping teams across the organization analyze data, build tools, and move ideas into production. Alongside its Codex rollout, it is using the OpenAI API to develop more personalized member experiences, including through its AI Trainer.

For Santhosh Kumar, CTO at cult.fit, AI presented an opportunity to change how the organization works. “We wanted to become more AI-native in the way we run things,” he says. “How can we run our operations or transform our operations to be AI-first? What new customer experiences can we offer? And how can we transform how we run our organization?”

## Standardizing software development with Codex

Cult.fit’s first major shift was in engineering. Before standardizing on Codex, developers were using a mix of coding agents and assistants. Different tools meant different conventions for repositories, agent instructions, skills, and development workflows, making it harder to share knowledge and reuse capabilities across teams.

Cult.fit wanted a shared platform around which it could standardize the software development lifecycle. The company chose Codex as that foundation. Repository structures, skills, testing, code review, and feedback loops could now follow common conventions across teams.

> “Once we standardized on Codex, we could build our software development lifecycle around a single platform.”

—Santhosh Kumar, CTO, cult.fit

The team then extended Codex into more specialized engineering workflows. Its Site Reliability Engineering agent can aggregate information from observability platforms and logs, investigate incidents, and, in many cases, create a pull request with a proposed fix for an engineer to review. Security agents help identify and address vulnerabilities, while other on-call agents investigate recurring production issues.

Cult.fit is also moving toward a more fully spec-driven development model, where a specification can guide work through the software development lifecycle toward a pull request for an engineer to review. This gives teams a more repeatable path from a defined requirement to working code.

One recent example is a social feed for the cult.fit app. Kumar estimates the project would previously have taken more than a year to build. Using the new workflow, the team built and shipped it end to end in two months.

## Giving every team the ability to build

Codex has since expanded beyond engineering. cult.fit made its company data easier for Codex to work with, allowing teams to find answers and perform routine analysis themselves. Previously, even small data requests had to go through a central analyst team, creating a bottleneck.

> “Codex has been our biggest step toward data democratization. Teams can now do the analysis they need themselves.”

—Santhosh Kumar, CTO, cult.fit

Teams are also building their own applications and workflows. HR, for example, built a workflow in Codex to manage performance reviews, scheduling, feedback collection, and the work of bringing that information together. More than 20 internal applications have been built by non-engineering teams in recent months.

That shift is changing how cult.fit thinks about the software it needs to run its business. When the cost of its internal dashboard software increased sharply, one engineer rebuilt the functionality with Codex over a weekend. Cult.fit now runs the capability on its own application, avoiding more than $100,000 in annual software costs.

Making this shift required more than giving employees access to Codex. A large hackathon gave employees an early view of what was possible. The company then brought its management team together for a one-day session where leaders across functions built something relevant to their own work with Codex.

Engineering also runs twice-weekly office hours to help employees move projects from an idea to production, while engineers are embedded in selected functions to provide technical and domain support.

> “Software has been democratized. With OpenAI’s solutions, our people are able to build their own applications around whatever their teams need.”

—Santhosh Kumar, CTO, cult.fit

## Enriching the member experience with AI

Cult.fit is also using the OpenAI API to extend AI into the member experience through its AI Trainer. With over 45% of cult.fit members being first time gym goers, guidance is a key access for helping them build a fitness habit. The company first built the AI Trainer several years ago to give members a guided personalized workout experience without a personal trainer.

Around 20% of Cult.fit’s gym users currently utilize the AI Trainer product. With advances in language models, cult.fit saw an opportunity to make the experience more interactive and conversational.

The latest version is designed around a voice-first workout experience. Using GPT Realtime through an OpenAI API integration, the trainer can provide coaching cues and support workout logging through two-way voice interaction.

## Making AI part of the operating model

Cult.fit is also applying AI to customer support workflows that previously required human agents to assess each case. One example is requests to waive penalties for missed group fitness classes.

When a member says they missed a class for a valid reason, an AI agent can assess the request by checking signals such as local weather and attendance patterns for that class before deciding whether the penalty should be waived.

Cult.fit built these agents into its existing Sprinklr support platform and connected them to its backend systems, moving customer support from a predominantly human-led chat model toward an AI-first workflow.

## Results at a glance

- 20+ internal applications built by non-engineering teams in recent months

- $100,000+ annual software savings by rebuilding an internal capability with Codex

- 50%+ reduction in customer support costs

## What’s next

Cult.fit’s next step is to move from individual AI workflows toward an organization where AI is built into how work gets done. In operations, Kumar describes the ambition as an “autopilot operations org,” with AI taking on more of the support work around frontline teams.

In the member experience, the goal is a “trainer in your pocket,” extending personalized guidance beyond what a human trainer can provide at scale. Inside the organization, cult.fit continues to look for bottlenecks teams can remove with AI. As more employees build applications, analyze information, and automate workflows themselves, more ideas can move directly into working solutions.
