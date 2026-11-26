---
title: OpenAI may be in cost control mode
link: https://micjm.com/blog/openai-is-in-cost-control-mode/
source: hnrss-org
published: 2026-11-26T07:33:12Z
updated: 2026-11-26T07:33:12Z
first_seen: 2026-11-26T21:52:31.976721529Z
authors:
- seizethecheese
content: extracted
html: 2026-11-26-openai-may-be-in-cost-control-mode.html
remote_preview:
  url: https://micjm.com/blog/openai-is-in-cost-control-mode/cost.png
  alt: Sol and Astra Elo versus median cost per task compared to opponents
---

OpenAI’s recent release of Astra 6.2 and Sol 6.2 was widely panned. What happened?

Based on our data, it looks like they’ve been tuned for cost savings. Specifically output tokens per task and reasoning tokens per task have dropped.

Here is a chart of high-level cost change from the GPT-6 to GPT 6.2:

[![Sol and Astra Elo versus median cost per task compared to opponents](https://micjm.com/blog/openai-is-in-cost-control-mode/cost.png)](https://micjm.com/blog/openai-is-in-cost-control-mode/cost.svg)

The X axis in the above chart is the ratio of spending of the GPT model compared to opponents in our [battle system for real work](https://github.com/bottomless/agent-duel). The Y axis on the above chart is Elo from [our leaderboard](https://agentduel.com/leaderboard).

The moves in the chart are significant, here’s the same chart with confidence intervals:

[![The same cost and Elo estimates with 95% developer-bootstrap intervals on both axes](https://micjm.com/blog/openai-is-in-cost-control-mode/cost-intervals.png)](https://micjm.com/blog/openai-is-in-cost-control-mode/cost-intervals.svg)

## What changed

Compared to opponents, Astra and Sol seem to be reasoning a lot less. Here is a chart of how much models reason compared to opponents in the same battle. Above the line means more reasoning and below means less.

[![Median recorded reasoning tokens for the same models versus their matched opponents](https://micjm.com/blog/openai-is-in-cost-control-mode/reasoning-tokens-vs-opponents.png)](https://micjm.com/blog/openai-is-in-cost-control-mode/reasoning-tokens-vs-opponents.svg)

You can see that both Astra 6 and Sol 6.1 were pretty normal models, but Astra 6.2 and Sol 6.2 become outliers.

We can see the same effect with output tokens:

[![Median output tokens for each of the 18 models with over 100 eligible votes versus their opponents on the same completed tasks](https://micjm.com/blog/openai-is-in-cost-control-mode/output-tokens-vs-opponents.png)](https://micjm.com/blog/openai-is-in-cost-control-mode/output-tokens-vs-opponents.svg)

Are these changes significant? The p-value on output dropping is 0.0055 for Sol and 0.022 for Astra. The p-value on reasoning dropping is 0.00013 for Sol and 0.0064 for Astra.

In conclusion, Astra and Sol appear to be cost tuned compared to their predecessors, and to a degree that makes them underthinking outliers.
