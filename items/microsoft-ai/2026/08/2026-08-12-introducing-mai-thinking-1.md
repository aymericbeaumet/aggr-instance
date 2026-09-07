---
title: Introducing MAI-Thinking-1
link: https://microsoft.ai/news/introducing-mai-thinking-1/
source: microsoft-ai
published: 2026-08-12T16:00:00Z
updated: 2026-08-12T16:00:00Z
first_seen: 2026-09-07T17:03:44.343711062Z
authors:
- kyleburgess@microsoft.com
summary: The post Introducing MAI-Thinking-1 appeared first on Microsoft AI.
content: extracted
html: 2026-08-12-introducing-mai-thinking-1.html
preview:
  file: 2026-08-12-introducing-mai-thinking-1.preview-766760c87f4e.webp
  width: 252
  height: 256
  color: '#ada99a'
images:
- source: https://microsoft.ai/wp-content/uploads/2026/05/HeroCollageBuild-warm38jpg-8.jpg
  original:
    file: 2026-08-12-introducing-mai-thinking-1.image-1482f7fdf4b5.jpg
    width: 1019
    height: 1034
  variants:
  - file: 2026-08-12-introducing-mai-thinking-1.image-9921c13ad574.webp
    width: 48
    height: 49
  color: '#e7e3d7'
---

Today we are introducing MAI-Thinking-1, Microsoft AI’s reasoning model. It is a medium-sized model that stands among the strongest models in its weight class. It matches leading models on key software engineering benchmarks, demonstrates advanced mathematical reasoning capabilities, and is preferred to Sonnet 4.6 in our blind human side-by-side evaluations. We don’t distill from other labs and we don’t rely on opaque data. Our datasets are clean, traceable, and enterprise-grade.

MAI-Thinking-1 is a step in our broader work to build towards Humanist Superintelligence: advanced AI capabilities designed to serve people and organizations, not to replace them. The model matters on both axes: what it can do, and how it was built.

## The Hill-Climbing Machine

More than a single model, we are excited to introduce our Hill-Climbing Machine: a co-designed pipeline built to make every component of model development climbable, so capabilities improve continually and reliably over time. The aim is a repeatable system that can absorb better data, stronger rewards, more capable environments, and more compute.

Three main pillars guide our philosophy.

**First, capabilities should be learned, not inherited.** Although faster to acquire, inherited intelligence lacks the steerability essential for real world usage: an imitator is fundamentally tied to the design choices of its teacher and struggles to adapt to new situations. MAI-Thinking-1 was trained without distillation from third party models, forcing our model to truly learn the tasks at hand.

**Second, clean data.** We trained it from the ground up on clean, traceable and enterprise-grade data, without distillation from third-party models. This matters for quality, provenance, and control. If we cannot account for what shaped a model, we cannot fully understand its behavior or credibly improve it.

**Third, self-sufficiency across the entire stack.** All the way from co-design of our models with MSFT’s own accelerators through to our reinforcement learning framework, we have focused efforts on in-house training infrastructure. This is a crucial part of building our hill-climbing machine, to ensure we can fully optimize and shape our systems end-to-end to best serve our needs.

## Medium-sized model, with strong software engineering performance

MAI-Thinking-1 is a 35B-active, ~1T-total parameters, sparse Mixture of Experts model, a smaller inference footprint than much larger models. Despite this, our model is toe-to-toe with Claude Opus 4.6 on SWE-Bench Pro. That matters for developers and enterprises because model size determines where advanced coding assistance can be deployed, how often it can be used, and whether it can move from exceptional tasks into daily workflows.

We have invested heavily in the training environments needed for agentic coding. Each verified environment is deterministic, executable, and graded by real test suites. This gives the model practice on the kind of multi-step work developers actually do: reading code, editing files, running tests, observing failures, and recovering from intermediate mistakes.

## Advanced mathematical reasoning capabilities

MAI-Thinking-1 reaches 97.0% on AIME 2025, and 94.5% on AIME 2026, showing strong mathematical and scientific reasoning for its weight class. Strong performance here gives us confidence that our training loop can create real reasoning gains – climbing all the way from the ground up – from our own data, rewards, and evaluation process, enabling this intelligence to generalize to other domains over time.
