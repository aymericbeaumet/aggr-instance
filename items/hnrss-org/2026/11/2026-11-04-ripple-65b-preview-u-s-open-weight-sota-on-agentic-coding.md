---
title: 'Ripple-65B Preview: U.S. Open-Weight SOTA on Agentic Coding'
link: https://www.ricursive.com/ripple
source: hnrss-org
published: 2026-11-04T18:53:29Z
updated: 2026-11-04T18:53:29Z
first_seen: 2026-11-04T21:49:17.908350494Z
authors:
- Philpax
content: extracted
html: 2026-11-04-ripple-65b-preview-u-s-open-weight-sota-on-agentic-coding.html
remote_preview:
  url: https://framerusercontent.com/images/u4OXzV0utwAtaPgcVxQtTXmkg.png
---

Rather than investing in a brute-force RL climb, we carefully iterated on our SFT and RL recipes with a focus on compute efficiency. We opted not to buy any posttraining data, instead devising our own synthetic data pipelines, quality filtering, and coverage metrics.

Our mixed RL recipe uses [truncated importance sampling](https://openreview.net/forum?id=xsonlP8DRV), [score centering](https://arxiv.org/abs/2609.20807), and [linear length penalty](https://microsoft.ai/pdf/mai-thinking-1.pdf) with high-quality long-horizon synthetic environments. The RL tasks are tuned for our model capabilities, and we continuously added harder tasks as the model improved.

The end result was a steep RL climb from a strong initial SFT checkpoint. We reached a TerminalBench 2.1 score of 80.5 with only 1.4M rollouts, roughly 70x fewer rollouts than Reflection Beam, the prior SOTA.
