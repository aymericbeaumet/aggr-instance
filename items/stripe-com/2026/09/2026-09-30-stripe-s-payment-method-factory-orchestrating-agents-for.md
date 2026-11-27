---
title: 'Stripe’s Payment Method Factory: Orchestrating agents for repeated, custom integrations'
link: https://stripe.dev/blog/stripes-payment-method-factory-orchestrating-agents-for-repeated-custom-integrations
source: stripe-com
published: 2026-09-30T00:00:00Z
updated: 2026-09-30T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- David Dunne
summary: Stripe built an agentic payment method factory from over 100 reusable prompts, hardened by agents observing implementation runs, and kept them current by feeding back each run's learnings.
content: extracted
html: 2026-09-30-stripe-s-payment-method-factory-orchestrating-agents-for.html
remote_preview:
  url: https://stripe.dev/og/blog/stripes-payment-method-factory-orchestrating-agents-for-repeated-custom-integrations.png
  alt: 'Stripe Developer Blog: Stripe’s Payment Method Factory: Orchestrating agents for repeated, custom integrations'
---

The first time you build something new, you start from scratch. The second time, you spot the patterns and build a platform. But to truly scale, you build a factory.

We built our payment method factory from over 100 reusable prompts, hardened by agents observing implementation runs, and kept them current by feeding back each run's learnings.

Stepping back, Stripe offers over 125 payment methods across the globe, ranging from credit cards, to PayPal wallets, to Buy Now, Pay Later services like Klarna. At a high level, each payment method does the same thing:

- Makes a payment
- Issues a refund
- Handles a dispute

Under the hood, they have different behaviors and different API shapes. Despite these differences, the process of building each integration follows a familiar pattern. Yet with little reusable tooling, teams were starting from scratch each time, meaning it could take six months to build even a basic integration.

In 2025, this led us to ask two important questions.

- Could we prompt an LLM to build an entire payment method integration?
- And, more importantly, could we reuse those prompts for every future integration?

The factory we built has so far created three new payment method integrations and migrated ten existing integrations to our modern stack, demonstrating its effectiveness for legacy migrations, too. The payment method factory reduced integration timelines from as long as six months to two-to-six weeks.

Along the way, we learned how valuable it was to harden our prompts to make them reusable, keep them up to date as the codebase evolved, orchestrate the agents in a human-guided workflow, and get agents testing their own changes.

## Building the foundation: Hardened reusable ‘one-shot’ prompts

An agent skill and a simple prompt are often enough to complete part of an integration. If the agent needs steering, we can refine the original prompt until it completes the task without our help. Saving these prompts for the next integration makes sense, but how do we know whether they’re actually good prompts, or whether the agent just muddled its way through despite them? Even in a successful ‘one-shot’ run, we can still see the agent perform unnecessary investigations or repeatedly attempt to fix failing tests. We hypothesized that evaluating and refining these prompts could get us the same results faster, at a lower cost, and with more confidence in the output.

To see how much difference this approach could make, we did a bake-off, two engineers completed the same integration task: add payment method support for a frontend-facing API. One engineer used vanilla Claude Code, while the other used Claude Code with a reusable set of prompts built from our prior experience. The difference was substantial: the vanilla setup required roughly 20 days of engineering effort, compared to 4 days when using the reusable prompts.

### Writing a good one-shot prompt

We began with over 125 past implementations of payment methods as the base of our prompt. We gave the agent these examples and a template for structuring the prompt, then refined the output by hand. We designed the resulting template to be payment-method agnostic so it could be reused across other payment methods. Here’s an example:

![](https://stripe.dev/images/stripes-payment-method-factory-orchestrating-agents-for-repeated-custom-integrations/image4.png)

Excerpt from a prompt for handling payment confirmations

This gave us a strong starting point for evaluating and hardening the prompt. The process starts with an implementation agent that’s following the prompt to build a payment method integration. A second agent, acting as an observer, runs in parallel to the implementation agent. The observer knows what the agent is trying to implement and what the end state should look like. It reads the implementation agent’s transcript, and notes down which steps took extra time to investigate, where the agent got lost, and what the agent had to re-learn. It checks if any of our internal tooling failed during the process, like a command timing out. If the first pass generated a red CI, it looks at the failures. The engineer takes the observer agent’s report and refines the prompt further.

![](https://stripe.dev/images/stripes-payment-method-factory-orchestrating-agents-for-repeated-custom-integrations/image6.png)

The first step when starting on a new payment method integration is the scaffolding, which  could take an engineer up to a week to finish. It involves adding an ‘adapter’ so that other Stripe products can interact with the payment method, adding feature gates to restrict access, wiring it in our API specs and SDKs without exposing it, and setting the partner details. This change alone is more than 5000 lines long, touches over 100 files, and is produced in part by 15 commands that generate code and have to run in a set order. When something broke, an engineer had to work out what each command does and where the payment method still needed to be registered. With a prompt that explains what each piece does and where to look if something breaks, we brought this step down to a day of implementation time.

### Continuous knowledge loop: Keeping prompts up to date

The issue with reusable prompts, unlike creating a new one each time, is that we have to find a way to effectively maintain them. We now have over 100 saved prompts. If the way we provision databases changes, or an investigation pointer isn’t valid, we need to update any affected prompts.

To take on this maintenance overhead, we developed a system that continuously improves the prompts. While the payment method factory is working on an integration, it collects some artifacts:

- Learning logs from the individual implementation agents about anything they had to learn about our systems that wasn’t covered by the initial prompt.
- Decision logs for all the decisions they had to make unattended to give the reviewers more context on why the agent went down a certain path.
- Full transcripts of the entire session.
- Engineer’s review comments on the created pull request.

When the integration is complete, an agent takes these artifacts and suggests potential improvements to the prompts. Some are straightforward enough and can be implemented with the click of a single button using [minions](https://stripe.dev/blog/minions-stripes-one-shot-end-to-end-coding-agents), Stripe’s one-shot coding agents, while others might require further work before we apply them.

![](https://stripe.dev/images/stripes-payment-method-factory-orchestrating-agents-for-repeated-custom-integrations/image2.png)

## Building the factory: Orchestrating our collection of prompts

We now have a collection of prompts that covers every step of building a new payment method integration. Stripe uses a monorepo, so theoretically, we could combine all the prompts into one master prompt and be done in one pull request. In practice, the current limits of an agent’s context window make this impossible and the resulting PR would be too large to review and prone to merge conflicts.

We kept the development lifecycle we already know works: start an agentic session, run a prompt, review the result, and open a PR. The difference is that an engineer no longer has to drive every step themselves. An orchestrator can delegate each step to an isolated implementation agent.

There are two phases to our orchestration. The first is creating a plan, a sequence of discrete steps, each linked to one of the prompts we developed earlier. The goal is for each step to map to one pull request. As we construct the plan, the agent interviews the engineer for context on the payment method being built, asking about requirements like what currencies are supported in which regions. This context is exported to a shared knowledge base that implementation agents can later reference.

![](https://stripe.dev/images/stripes-payment-method-factory-orchestrating-agents-for-repeated-custom-integrations/image1.png)

The second phase executes the plan, with implementation (sub)agents handling each step. To coordinate their work, we needed an orchestrator that could assign each task to the right implementation agent and track its progress.

Our first approach used a high-level orchestrator agent for this role. It worked, but it was expensive, slow, and sometimes unreliable. In some cases, the orchestrator agent tried to implement the step itself rather than delegating to an implementation agent, which ultimately ate up its context.

We learned that although an LLM can orchestrate a workflow, predictable coordination is better implemented in code.

![](https://stripe.dev/images/stripes-payment-method-factory-orchestrating-agents-for-repeated-custom-integrations/image5.png)

## Building confidence: Letting agents test their own work

As code generation becomes faster, code reviews started becoming a bottleneck. We help reviewers by keeping pull requests small and providing all relevant context, but ultimately, we found that giving agents the tools to test their integrations autonomously gives the reviewer the most confidence.

Let’s take the service that talks with the payment method provider as an example. If a provider has a sandbox environment, the factory creates test payments and runs different end-to-end scenarios against it. The factory checks requests, responses, internal state changes, and emitted events against our expectations. It then snapshots the provider’s API responses and creates immutable contracts that are used throughout our stack, rather than manually creating hand-written mocks. All of this is then presented to the engineer and is used in our system to emulate recorded payment method behavior.

We still have room to improve here. Agents should have the tools to interact with every part of our systems and we’d like to automate remaining manually tested steps, like UI testing.

## Beyond payment methods

Payment methods are not the only repeated, customizable integration we have. Teams at Stripe were inspired by the payment method factory to build their own versions. For example, our tax team uses one to build connectors that bring Stripe Tax to businesses selling on other platforms, and our verification team built one for the providers we use to check business details during onboarding.

If making other engineers more productive interests you, [we’re hiring](https://grnh.se/b5wzjpge1us).
