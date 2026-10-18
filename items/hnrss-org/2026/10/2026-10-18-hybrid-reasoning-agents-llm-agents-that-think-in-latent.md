---
title: 'Hybrid Reasoning Agents: LLM agents that think in latent space, act through text'
link: https://arxiv.org/abs/2610.18861
source: hnrss-org
published: 2026-10-18T20:48:14Z
updated: 2026-10-18T20:48:14Z
first_seen: 2026-10-18T23:27:08.867657739Z
authors:
- allinonai
summary: 'https://x.com/Jiaru_Zou/status/2110400256647340476 https://github.com/togethercomputer/HybridAgent https://huggingface.co/collections/togethercomputer/hybridag... Evaluations: https://arxiv.org/html/2610.18861v1#S5 Comments URL: https://news.ycombinator.com/item?id=50145337 Points: 131 # Comments: 75'
content: extracted
html: 2026-10-18-hybrid-reasoning-agents-llm-agents-that-think-in-latent.html
remote_preview:
  url: https://arxiv.org/static/browse/0.3.4/images/arxiv-logo-fb.png
  alt: arXiv logo
---

## Title:Hybrid Reasoning Agents

[View PDF](https://arxiv.org/pdf/2610.18861) [HTML (experimental)](https://arxiv.org/html/2610.18861v1)

> Abstract:How can we build an agent that thinks beyond human language, yet still acts through it in the external environment? Modern LLM agents still reason and interact through text, while latent reasoning offers an efficient internal medium but lacks native tool access and reasoning transparency. We bridge these two directions with Hybrid Reasoning Agents (HybridAgents), which dynamically navigate between latent reasoning and text-based interaction throughout a unified generation process. Architecturally, a lightweight HybridLink enables seamless transitions between text and latent modes, while the agent's continuous working memory preserves lossless hybrid context across multiple turns. These designs preserve tool-integrated agentic capabilities while making latent thoughts directly inspectable. Building on this architecture, we further develop dedicated post-training algorithms for hybrid reasoning: HybridSFT stabilizes tool-integrated hybrid generation by teaching the backbone to coordinate with HybridLink, then HybridRL scales reasoning by learning when to think in latent space and when to act through text. We evaluate HybridAgent at 3 model scales across 11 benchmarks covering mathematics, science, medicine, search, and agentic reasoning. Compared with frontier LLM Agents and agentic frameworks spanning both text-based and latent methods, HybridAgent consistently improves performance by 10.4%, while delivering 35.5%-54.7% token reduction, 1.8x-2.6x faster inference, and 2.7x-3.7x cost savings. Code and data are available at [this https URL](https://github.com/togethercomputer/HybridAgent).

|           |                                                                                                                                               |
| --------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| Comments: | 45 pages, 12 figures                                                                                                                          |
| Subjects: | Computation and Language (cs.CL); Artificial Intelligence (cs.AI); Machine Learning (cs.LG)                                                   |
| Cite as:  | [arXiv:2610.18861](https://arxiv.org/abs/2610.18861) \[cs.CL\]                                                                               |
|           | (or [arXiv:2610.18861v1](https://arxiv.org/abs/2610.18861v1) \[cs.CL\] for this version)                                                     |
|           | [https://doi.org/10.48550/arXiv.2610.18861](https://doi.org/10.48550/arXiv.2610.18861)  arXiv-issued DOI via DataCite (pending registration) |

## Submission history

From: Jiaru Zou \[[view email](https://arxiv.org/show-email/d71dd478/2610.18861)\] \
 **\[v1\]** Tue, 13 Oct 2026 17:59:31 UTC (8,500 KB)
