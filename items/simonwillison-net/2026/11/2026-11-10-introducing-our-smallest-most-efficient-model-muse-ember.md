---
title: 'Introducing Our Smallest, Most Efficient Model: Muse Ember'
link: https://simonwillison.net/2026/Nov/10/muse-ember/
source: simonwillison-net
published: 2026-11-10T07:28:21Z
updated: 2026-11-10T07:28:21Z
first_seen: 2026-11-10T11:40:21.505980627Z
labels:
- ai
- artificial-analysis
- generative-ai
- llm-pricing
- llm-release
- llms
- meta
- muse
summary: 'Introducing Our Smallest, Most Efficient Model: Muse Ember Meta AI just launched their entry in the Claude Haiku/GPT Luna category: Muse Ember, which they describe as: [...] our smallest and most efficient model designed for complex reasoning execution and the work agents repeat all day: tool calls, coding subtasks, routing, extraction, and long document parsing. For shorter prompts it''s priced the same as GPT-6 Luna and Claude Haiku 5.5 - both are $0.10/million input tokens and $0.50/million output. Today''s other big model release, Claude Fable 5.2, is 80x that price at $8/$40. The pricing is a little less straightforward as prompts get longer, though. Luna, Ember, and Haiku all support up to a million input tokens, but they also all have a threshold after which the price goes up. Taking that threshold into account Ember is priced between the two others: Model Prompt size Input ($/M) Cached input Output GPT-6 Luna ≤272k $0.10 $0.01 $0.50 >272k $0.20 $0.02 $0.75 Muse Ember 1.0 ≤200k $0.10 $0.004 $0.50 >200k $0.30 $0.012 $1.50 Claude Haiku 5.5 ≤100k $0.10 $0.01 $0.50 >100k $0.50 $0.05 $2.50 But comparing price-per-token doesn''t really make sense these days, when reasoning tokens mean that different models might use dramatically different numbers of tokens to solve the same problem. For that, one measure that''s useful is the Artificial Analysis. And even more useful than that is their Intelligence Index vs. Cost per Task chart, which shows the following for those three models: The Muse Ember models sit right on that Pareto line - the line at which you can only improve one dimension by giving up another. Meta call this out in their announcement: It also sits on the efficiency frontier of the full Artificial Analysis leaderboard: no model scores higher for less per task. (That grey dot on the Pareto line in the middle of the green area is GPT-6.1 Terra (low) - clearly I should be paying more attention to the Terra family!) Muse Ember has six reasoning levels - minimal, low, medium, high, xhigh, and max - so I rendered six pelicans riding bicycles. The "high" reasoning level failed to return a full SVG, and the other SVGs were surprising in that the reasoning level didn''t cleanly map to the visual quality of the response. Here''s the "max" one, but the others are worth a look too: The Ember pelicans are comparable in quality to those I got from Muse Wonder, which is 40x the price of Muse Ember. Tags: ai, generative-ai, llms, meta, llm-pricing, llm-release, artificial-analysis, muse'
content: extracted
html: 2026-11-10-introducing-our-smallest-most-efficient-model-muse-ember.html
remote_preview:
  url: https://static.simonwillison.net/static/2026/muse-ember.webp
  alt: Bicycle frame is OK, basket has fish, pelican has a weird blue hat on.
---

**[Introducing Our Smallest, Most Efficient Model: Muse Ember](https://research.meta.ai/blog/muse-ember)**. Meta AI just launched their entry in the Claude Haiku/GPT Luna category: Muse Ember, which they describe as:

> \[...\] our smallest and most efficient model designed for complex reasoning execution and the work agents repeat all day: tool calls, coding subtasks, routing, extraction, and long document parsing.

For shorter prompts it's priced the same as GPT-6 Luna and Claude Haiku 5.5 - both are $0.10/million input tokens and $0.50/million output. Today's other big model release, Claude Fable 5.2, is *80x that price* at $8/$40.

The pricing is a little less straightforward as prompts get longer, though. Luna, Ember, and Haiku all support up to a million input tokens, but they also all have a threshold after which the price goes up. Taking *that* threshold into account Ember is priced between the two others:

| Model            | Prompt size | Input ($/M) | Cached input | Output |
| ---------------- | ----------- | ----------- | ------------ | ------ |
| GPT-6 Luna       | ≤272k       | $0.10       | $0.01        | $0.50  |
| \>272k           | $0.20       | $0.02       | $0.75        |        |
| Muse Ember 1.0   | ≤200k       | $0.10       | $0.004       | $0.50  |
| \>200k           | $0.30       | $0.012      | $1.50        |        |
| Claude Haiku 5.5 | ≤100k       | $0.10       | $0.01        | $0.50  |
| \>100k           | $0.50       | $0.05       | $2.50        |        |

But comparing price-per-token doesn't really make sense these days, when reasoning tokens mean that different models might use dramatically different numbers of tokens to solve the same problem.

For that, one measure that's useful is the Artificial Analysis.

![Stacked bar chart from Artificial Analysis comparing a cost in dollars across six models, each bar split into four unlabeled stacked segments, sorted from lowest to highest total. Claude Haiku 5.5 (low): $0.04* total, bottom segment $0.02, three thin unlabeled segments above. GPT-6 Luna (non-reasoning): $0.07 total, bottom segment $0.06, thin unlabeled segments above. GPT-6 Luna (xhigh): $0.10 total, segments from bottom $0.07, $0.02, $0.02, plus a thin unlabeled top segment. Muse Ember (xhigh): $0.12 total, segments from bottom $0.06, $0.02, $0.02, plus a thin unlabeled top segment. Muse Ember (max): $0.14 total, segments from bottom $0.07, $0.03, $0.03, $0.02. Claude Haiku 5.5 (xhigh): $0.25* total, segments from bottom $0.14, $0.04, $0.05, $0.02. Anthropic bars are terracotta, OpenAI bars are dark gray, Meta bars are blue, each with the company logo beneath.](https://static.simonwillison.net/static/2026/artificial-analysis-haiku-class.webp)

And even more useful than that is their Intelligence Index vs. Cost per Task chart, which shows the following for those three models:

![Scatter plot from Artificial Analysis titled \"Intelligence Index vs. Cost per Task\", plotting Artificial Analysis Intelligence Index (y-axis, 5 to 40) against cost per task in USD on a log scale (x-axis, $0.04 to $0.3), with a green \"most attractive quadrant\" in the upper left and a dotted Pareto line. Claude Haiku 5.5 (low)* scores about 19 at roughly $0.044. GPT-6 Luna (non-reasoning) scores about 9.5 at roughly $0.07. GPT-6 Luna (xhigh) scores about 23.5 at roughly $0.10. Muse Ember (xhigh) scores about 34 at roughly $0.12 and Muse Ember (max) about 36 at roughly $0.14, both on the Pareto line. Claude Haiku 5.5 (xhigh)* scores about 34.5 at roughly $0.25, well to the right of the Pareto line. Three unlabeled gray dots also sit on the Pareto line.](https://static.simonwillison.net/static/2026/artificial-analysis-pareto.webp)

The Muse Ember models sit right on that Pareto line - the line at which you can only improve one dimension by giving up another. Meta call this out in their announcement:

> It also sits on the efficiency frontier of the full Artificial Analysis leaderboard: no model scores higher for less per task.

(That grey dot on the Pareto line in the middle of the green area is GPT-6.1 Terra (low) - clearly I should be paying more attention to the Terra family!)

Muse Ember has six reasoning levels - minimal, low, medium, high, xhigh, and max - so I [rendered six pelicans riding bicycles](https://tools.simonwillison.net/markdown-svg-renderer?url=https%3A%2F%2Fgist.github.com%2Fsimonw%2Fc7666e8a54d84f724ac8121f9fa6cc4d). The "high" reasoning level failed to return a full SVG, and the other SVGs were surprising in that the reasoning level didn't cleanly map to the visual quality of the response.

Here's the "max" one, but the others are worth a look too:

![Bicycle frame is OK, basket has fish, pelican has a weird blue hat on.](https://static.simonwillison.net/static/2026/muse-ember.webp)

The Ember pelicans are comparable in quality to those [I got from Muse Wonder](https://tools.simonwillison.net/markdown-svg-renderer?url=https://gist.github.com/simonw/185cff7ff0b11379725abf7f78d5d2b2), which is [40x the price](https://www.llm-prices.com/#sel=muse-ember-1.0%2Cmuse-ember-1.0-200k%2Cmuse-wonder-1.0) of Muse Ember.
