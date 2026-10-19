---
title: Claude Haiku 5.5
link: https://simonwillison.net/2026/Oct/7/claude-haiku-5-5/
source: simonwillison-net
published: 2026-10-07T20:56:21Z
updated: 2026-10-07T20:56:21Z
first_seen: 2026-10-07T23:45:59.494481067Z
labels:
- ai
- anthropic
- claude
- generative-ai
- llm-pricing
- llm-release
- llms
- pelican-riding-a-bicycle
summary: 'As previously promised, here''s Anthropic''s new fast, low cost model: Introducing Claude Haiku 5.5. The previous Haiku, 4.5, was very much showing its age. It came out almost a year ago, and was priced at $1/million input and $5/million output - relatively expensive even back then, and a full 10x the price of OpenAI''s GPT-6 Luna, released last month. The new Haiku exactly matches the price of GPT-6 Luna - $0.10/$0.50 - up to 100,000 tokens. Beyond 100,000 tokens the price increases 5x to $0.50/$2.50. Luna itself has a price increase at 272,000 tokens but only to $0.20/$0.75. Haiku 5.5 also uses a new, less generous tokenizer. My Claude Token Counter tool shows that the same long prompt uses around 1.25x as many tokens with Haiku 5.5 compared to Haiku 4.5, so there''s a hidden price increase there. If your workloads fit in 100,000 tokens, Haiku is the same price as Luna and reports higher benchmark scores. Above 100,000 tokens, Luna looks like a much better deal. The most recent release of llm-anthropic finally fixed it so I don''t need to ship a new version of that plugin for every new model. I tested the new model like this: llm install -U llm-anthropic llm anthropic refresh llm -m claude-haiku-5.5 "Generate an SVG of a pelican riding a bicycle" -o thinking_effort low Pelicans Here are pelicans for low, medium, high, xhigh, and max. The new Haiku doesn''t let you disable reasoning, and defaults to medium. I got a good bicycle frame for everything beyond low. The low effort pelican cost 0.0936 cents and took 7 seconds. This max effort pelican took 5 minutes 9 seconds to generate, but still only cost me 3.3826 cents: For comparison, here''s the pelican I got a year ago from Haiku 4.5 (for 0.7583 cents - Haiku 4.5 did not support reasoning levels). It sucked at drawing pelicans: And a generous API credit scheme for subscribers In addition to Haiku 5.5, Anthropic announced today that they are halving the price of cache reads for Sonnet 5.5. They''ve also added API credits to subscription plans: Second, this week, we’ll roll out a new monthly API credit to all Max and Team subscribers for use on the Claude Platform. Max 5x users will get $100 in credits per month, Max 20x users will get $200, and Team subscribers will receive up to $500, pooled across their users. Claiming this is pleasantly easy: navigate to Settings -> Billing and select the API organization that should benefit from the credits every month: The API credits exactly match the cost of the subscription itself. This is really generous - it makes it much easier for subscribers to use the API. Anthropic also let you disable auto-reload for the API, with the consequence that "API requests will stop when your balance runs out" - exactly what you want if you''re planning to burn through those API credits without risk of a nasty billing surprise. Note that the monthly credits do not roll over - use them or lose them. OpenAI still allow you to use your Codex subscription for personal API use, which works out as a better deal for heavy API users. This new credit scheme goes at least some way to overcoming that difference. Tags: ai, generative-ai, llms, anthropic, claude, llm-pricing, pelican-riding-a-bicycle, llm-release'
content: extracted
html: 2026-10-07-claude-haiku-5-5.html
remote_preview:
  url: https://static.simonwillison.net/static/2026-10-07/haiku-5.5-max.webp
  alt: 'Flat vector illustration: a white pelican wearing a red cap rides a dark grey bicycle from left to right across a green grassy strip, its long orange legs reaching down to the orange pedals, a grey wing stretched forward to grip the curved handlebars, and its large yellow-orange beak with a bulging'
---

As previously [promised](https://simonwillison.net/2026/Sep/28/claude-sonnet-5-5/), here’s Anthropic’s new fast, low cost model: [Introducing Claude Haiku 5.5](https://www.anthropic.com/claude-haiku-5-5).

The previous Haiku, 4.5, was very much showing its age. It came out [almost a year ago](https://simonwillison.net/2025/Oct/15/claude-haiku-45/), and was priced at $1/million input and $5/million output—relatively expensive even back then, and a full 10x the price of OpenAI’s [GPT-6 Luna](https://simonwillison.net/2026/Sep/22/opus-and-sol-and-luna/#gpt-6-sol-and-luna-are-half-the-price-of-their-gpt-5-6-equivalents), released last month.

The new Haiku exactly matches the price of GPT-6 Luna—$0.10/$0.50—up to 100,000 tokens. Beyond 100,000 tokens the price increases 5x to $0.50/$2.50. Luna itself has a price increase at 272,000 tokens but only to $0.20/$0.75.

Haiku 5.5 also uses a new, less generous tokenizer. My [Claude Token Counter](https://tools.simonwillison.net/claude-token-counter) tool shows that the same long prompt uses around 1.25x as many tokens with Haiku 5.5 compared to Haiku 4.5, so there’s a hidden price increase there.

If your workloads fit in 100,000 tokens, Haiku is the same price as Luna and reports higher benchmark scores. Above 100,000 tokens, Luna looks like a much better deal.

The [most recent release of llm-anthropic](https://github.com/simonw/llm-anthropic/releases/tag/0.30) finally fixed it so I don’t need to ship a new version of that plugin for every new model. I tested the new model like this:

```
llm install -U llm-anthropic
llm anthropic refresh
llm -m claude-haiku-5.5 "Generate an SVG of a pelican riding a bicycle" -o thinking_effort low
```

#### Pelicans

Here are [pelicans for low, medium, high, xhigh, and max](https://tools.simonwillison.net/markdown-svg-renderer?url=https%3A%2F%2Fgist.github.com%2Fsimonw%2F485dfa24b4efa1267ee2d0911594fb81). The new Haiku doesn’t let you disable reasoning, and defaults to `medium`. I got a good bicycle frame for everything beyond `low`. The [low effort pelican](https://tools.simonwillison.net/markdown-svg-renderer?url=https%3A%2F%2Fgist.github.com%2Fsimonw%2F485dfa24b4efa1267ee2d0911594fb81#response) cost 0.0936 cents and took 7 seconds.

This `max` effort pelican (with [a reasoning trace](https://tools.simonwillison.net/markdown-svg-renderer?url=https%3A%2F%2Fgist.github.com%2Fsimonw%2F485dfa24b4efa1267ee2d0911594fb81#reasoning-3) that starts “This is the classic pelican-on-bicycle SVG test...”) took 5 minutes 9 seconds to generate, but still only cost me [3.3826 cents](https://www.llm-prices.com/#it=27&ot=67647&sel=claude-haiku-5.5):

![Flat vector illustration: a white pelican wearing a red cap rides a dark grey bicycle from left to right across a green grassy strip, its long orange legs reaching down to the orange pedals, a grey wing stretched forward to grip the curved handlebars, and its large yellow-orange beak with a bulging orange throat pouch pointing ahead; behind it, white speed lines show motion, and above, a yellow sun with a pale halo and two white clouds sit in a gradient blue sky.](https://static.simonwillison.net/static/2026-10-07/haiku-5.5-max.webp)

(Since the reasoning trace exhibits awareness of the benchmark, here’s [Generate an SVG of an armadillo in fishnet tights jaywalking on Mars (on xhigh)](https://tools.simonwillison.net/markdown-svg-renderer?url=https%3A%2F%2Fgist.github.com%2Fsimonw%2F375ae4f2bb68999976b8da50548c788d#response), and the same prompt against [some other recent models](https://tools.simonwillison.net/markdown-svg-renderer?url=https%3A%2F%2Fgist.github.com%2Fsimonw%2F18c9f7fc3b3705cf88514cb9170ec246). [Background on that](https://news.ycombinator.com/item?id=49977979#49982139).)

For comparison, here’s the pelican I got [a year ago](https://simonwillison.net/2025/Oct/15/claude-haiku-45/) from Haiku 4.5 (for [0.7583 cents](https://www.llm-prices.com/#it=18&ot=1513&ic=1&oc=5)—Haiku 4.5 did not support reasoning levels). It *sucked* at drawing pelicans:

![Described by Haiku 4.5: A whimsical illustration of a bird with a round tan body, pink beak, and orange legs riding a bicycle against a blue sky and green grass background.](https://static.simonwillison.net/static/2025/claude-haiku-4.5-pelican.jpg)

#### And a generous API credit scheme for subscribers

In addition to Haiku 5.5, Anthropic announced today that they are halving the price of cache reads for Sonnet 5.5. They’ve also added API credits to subscription plans:

> Second, this week, we’ll roll out **a new monthly API credit to all Max and Team subscribers for use on the Claude Platform**. Max 5x users will get $100 in credits per month, Max 20x users will get $200, and Team subscribers will receive up to $500, pooled across their users.

Claiming this is pleasantly easy: navigate to [Settings -> Billing](https://claude.ai/new#settings/billing) and select the API organization that should benefit from the credits every month:

![Screenshot of a Settings dialog with a close X button at top right and a row of tabs reading \"General\", \"Account\", \"Privacy\", \"Billing\" (selected), \"Usage\", \"Capabilities\", \"Memory\" and \"Design sys\" (cut off at the edge). Beside a line-drawn icon of a branching plant with circular buds, the panel reads \"Max plan\", \"20x more usage than Pro\" and \"Your subscription will auto renew on Nov 2, 2026.\" with an \"Adjust plan\" button on the right. Below is a section headed \"API credits\" with a blue \"New\" badge, reading \"Your Max 20x plan includes $200 USD in API credits each month.\" followed by grey text \"Link an API organization to start receiving credits.\" with a \"Link organization\" button on the right.](https://static.simonwillison.net/static/2026-10-07/claude-credits.webp)

The API credits exactly match the cost of the subscription itself. This is really generous—it makes it much easier for subscribers to use the API. Anthropic also let you disable auto-reload for the API, with the consequence that “API requests will stop when your balance runs out”—exactly what you want if you’re planning to burn through those API credits without [risk of a nasty billing surprise](https://simonwillison.net/2026/Oct/3/default-hard-budget-caps/).

Note that the monthly credits [do not roll over](https://support.claude.com/en/articles/17154008-monthly-api-credits-for-max-and-team-plans#h_dfc1659cee)—use them or lose them.

OpenAI still allow you to use your Codex subscription for personal API use, which works out as a better deal for heavy API users. This new credit scheme goes at least some way to overcoming that difference.
