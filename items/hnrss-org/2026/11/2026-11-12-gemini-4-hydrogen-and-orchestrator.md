---
title: Gemini 4 Hydrogen and Orchestrator
link: https://antigravity.google/blog/gemini-4-hydrogen-in-google-antigravity/
source: hnrss-org
published: 2026-11-12T18:52:01Z
updated: 2026-11-12T18:52:01Z
first_seen: 2026-11-13T00:08:02.707796831Z
authors:
- jonbaer
content: extracted
html: 2026-11-12-gemini-4-hydrogen-and-orchestrator.html
remote_preview:
  url: https://antigravity.google/assets/image/blog/4-Hydrogen---Wide.png
  alt: Gemini 4 Hydrogen and Orchestrator | Google Antigravity Blog
---

Gemini 4 Hydrogen and Orchestrator - Blog blog Blog Blog Gemini 4 Hydrogen and Orchestrator - Blog

Today, we’re rolling out [Gemini 4 Hydrogen](https://blog.google/innovation-and-ai/models-and-research/gemini-models/introducing-gemini-4-hydrogen/) inside Google Antigravity alongside a new model dropdown option called Orchestrator, which pairs Hydrogen with our strongest models for long-horizon tasks, including Gemini 4 Carbon, in an optimized balance of quality, efficiency, and latency.

Selecting Orchestrator (Carbon) from the model selector in Google Antigravity.

## Put Gemini 4 Carbon and Gemini 4 Hydrogen to work with Orchestrator

Earlier this week, we introduced [Gemini 4 Carbon](https://antigravity.google/blog/gemini-4-carbon-in-google-antigravity) alongside [`/swarm`](https://antigravity.google/docs/swarm) for tackling massive, multi-worktree engineering tasks with parallel agent teams. In just a couple of days, tens of thousands of swarms have been launched on Antigravity, pushing frontier intelligence on the hardest problems.

For everyday feature work and debugging inside a single session, running a frontier reasoning model with multiple parallel agents on every step of every routine task slows down your loop and burns frontier quota. A common industry approach is to use a static “model router” that guesses task difficulty from your first prompt and routes the entire task to the cheapest model that is good enough to complete the task. Unfortunately, these start breaking down as agents become longer running, requiring different levels of complexity at different stages, often discovered as the task evolves.

Orchestrator takes a dynamic, bidirectional approach between Carbon and Gemini 4 Hydrogen within the evolution of a single trajectory. For example, Carbon can start in a read-only phase to explore dependencies, resolve ambiguities, and build a concrete plan with explicit re-planning triggers. Hydrogen could then take over to write edits, update packages, and run tests. If Hydrogen hits friction, it would hand back to Carbon to inspect the state, repair the plan, and resume execution. This allows Orchestrator to reach near-Carbon task success while being faster and more quota-efficient. On Terminal-Bench 4.1, Orchestrator reached near-Carbon task success rates while, on average, consuming 24% less quota and completing 19% faster.

- **Explicit multi-model pipelines with the Antigravity SDK:** When you want programmatic control over which model handles each role, you can wire Hydrogen and Carbon together directly with the Antigravity SDK. In the demo below, an open-source issue triage system built with the Antigravity SDK pairs multiple agents together. The Hydrogen agents handle triaging and coding to keep up with incoming issue volume, searching the codebase, asking clarifying questions, and fast UI iteration from PR comments while the Carbon agent is assigned to larger refactors, new features, and security checks.

Pairing Gemini 4 Hydrogen and Gemini 4 Carbon with the Antigravity SDK for automated open-source issue triage and code generation.

- **Fast, dependable execution with Gemini 4 Hydrogen:** Hydrogen also works as a standalone model for everyday coding and agentic work, scoring 51 on the [Artificial Analysis Intelligence Index](https://artificialanalysis.ai/) (surpassing Argon’s 50, and reaching 54 at its highest setting) and 57% on Terminal-Bench 4.1 (more than tripiling 3.8 Flash’s 18.2%). When selecting Hydrogen directly, you can dial reasoning depth across four thinking levels, low, medium, high (default), and a new `xhigh` tier.

Using Gemini 4 Hydrogen in Google Antigravity to upgrade dependencies, refactor shared utilities, and generate accessible image alt text.

## Getting started

Gemini 4 Hydrogen brings cutting-edge intelligence for everyday coding and agentic workflows. When you want frontier planning paired with speed without managing model switches, Orchestrator puts both Gemini 4 Carbon and Gemini 4 Hydrogen behind one prompt. Starting today, Gemini 4 Hydrogen is rolling out across paid Google AI Pro and Ultra subscribers. Orchestrator is rolling out as Orchestrator (Carbon) and Orchestrator (Argon) for Google AI Ultra subscribers, and as Orchestrator (Argon) for paid Google AI Pro subscribers, with additional models and plans supported in the future. Enterprises can access both in [Gemini for business](https://docs.cloud.google.com/gemini/enterprise/docs/ai-developer-tools-overview).
