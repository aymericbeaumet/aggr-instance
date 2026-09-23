---
title: Claude Opus 5.5, GPT-6 Sol, GPT-6 Luna, and a new price war
link: https://simonwillison.net/2026/Sep/22/opus-and-sol-and-luna/
source: simonwillison-net
published: 2026-09-22T23:46:41Z
updated: 2026-09-22T23:46:41Z
first_seen: 2026-09-23T00:20:14.260676283Z
labels:
- ai
- anthropic
- claude
- generative-ai
- gpt
- llm-pricing
- llms
- openai
- pelican-riding-a-bicycle
summary: 'Yesterday was Grok 4.7 (pelicans) and MiMo v2.6 Flash/Pro (more pelicans). Today Anthropic released Claude Opus 5.5, and around an hour later OpenAI released GPT-6 Sol and GPT-6 Luna. It''s going to take a while to get a good read on all of these new models, but here are my impressions so far. GPT-6 Sol and Luna are half the price of their GPT-5.6 equivalents GPT-5.6 Luna was already my favorite model for building applications against, because it combined excellent performance with being really cheap. Somehow GPT-6 Luna is half the price of that again - and GPT-6 Sol had a similar reduction compared to GPT-5.6 Sol. Here''s what the pricing landscape looks like today: Model Input Cached input Output GPT-6 Luna $0.10/M $0.01/M $0.50/M GPT-5.6 Luna $0.20/M $0.02/M $1.20/M Grok 4.7 $2/M $0.50/M $6/M GPT-6 Sol $2/M $0.20/M $10/M GPT-5.6 Terra $2/M $0.20/M $12/M Claude Opus 5.5 $4/M $0.20/M $20/M GPT-5.6 Sol $4/M $0.40/M $20/M Claude Fable 5.1 $10/M $0.25/M $50/M GPT-6 Astra $10/M $1/M $50/M Note that GPT-5.6 has a scheduled 25% price increase for November, so GPT-6 is half the price of the promotional pricing for those models. (With GPT-5.6 Terra priced the same as GPT-6 Sol, any remaining reasons to use Terra just evaporated.) It''s hard to overstate how competitive this pricing is. Grok 4.7 priced itself at $2/$6, less than half the price of GPT-5.6 Sol, but is now equally priced to GPT-6 Sol on input and closer on output. At $0.10/$0.50 GPT-6 Luna is one of the cheapest models OpenAI have ever released, beaten only by the far weaker GPT-4.1 Nano ($0.10/$0.40, April 2025) and GPT-5 Nano ($0.05/$0.40, August 2025). I rendered pelicans for GPT-6 Luna and for GPT-6 Sol, then I combined them all together in this comparison grid along with the GPT-5.6 pelicans. I like how you can instantly see that the 5.6 family chose bolder, brighter colors, while the 6 family is a lot more muted. I still think GPT-6 Astra on max produced the best pelican. Claude Opus 5.5 got a price cut too Opus 5.5 looks like it addresses the biggest complaints people had about Opus in terms of its communication style. Thariq Shihipar: Opus 5.5 is the result of your feedback. It communicates clearly, it''s cheaper per token than Opus 5.0 with the intelligence of Fable 5.1 it''s very token efficient and works across every effort level. It''s also meant to be better at Blender. I''m looking forward to putting it through its paces there. Opus 4.5, 4.6, 4.7, 4.8, and 5 all shared the same price: $5/million tokens for input and $25/million for output. 5.5 is a 20% reduction - $4/million and $20/million. The price for cache reads fell 60%. That''s significant for longer agentic conversations, where 90%+ of input tokens are processed at cached token prices. The new price for Opus 5.5 is the same as the price for GPT-5.6 Sol, but that was before OpenAI dropped their Sol prices by half. GPT-6 Astra and Claude Fable 5.1 are both priced at $10/million input and $50/million output. The price war currently affects the next tier of models below that. Anthropic say that Sonnet 5.5 and Haiku 5.5 are coming soon. It''s going to be interesting to see if Haiku can regain its price competitiveness at the lower end, given current Haiku 4.5 is $1/$5 while the latest GPT-6 Luna is one tenth of that price at $0.10/$0.50. Claude Opus 5.5 max over-thinks to the point of breaking In a first for my "Generate an SVG of a pelican riding a bicycle" test, Claude Opus 5.5 at "max" thinking level failed to return a response! It started by calling this "a classic test request", and then thought really, really hard about what it was doing: This is a classic test request, so I want to plan out a well-composed pelican with its distinctive beak and pouch riding a bicycle with proper wheels, frame, and pedals, set against a simple sky and ground backdrop. [...] Verifying the shin length checks out at roughly 95.2, close enough. Now I''m working out the near leg path from hip to knee to ankle, then sketching the foot shape resting on the pedal — outlining the heel, toe tips, and sole contour with a path using lines and curves to sit naturally on the pedal surface around y=478-494. [...] I like the fish sticking prominently out of the basket with the pelican eyeing it as a fun detail worth keeping. I''m also confirming the eye placement near the bill base matches typical pelican anatomy, and considering giving it a slightly happier expression. [...] The far leg reads correctly as passing behind the frame, so I''m moving on to check the chainring teeth and confirm layer ordering—the far crank arm should be mostly hidden by the seat tube and chainring. I''m settling on the final SVG''s width and height attributes alongside the viewBox to ensure proper scaling, noting there''s no text so no font-family is needed. [...] I was so excited to see this pelican... but then it stopped. Opus 5.5 has a 128,000 maximum output token limit (as do the other Claude models), and it hit that while it was still reasoning about the SVG! I tried a second time and got the same result. This makes me suspect that "max" is effectively useless - if it over-thinks to breaking point on a stupid SVG prompt I don''t trust it not to do the same for more interesting work. (Those two failures each cost me $2.56 and took nearly 20 minutes.) Fable 5.1 on "max" didn''t over-think and did give me the best pelican I''ve seen from any Anthropic model. Here are the Opus 5.5 pelicans, excluding 5.5 max. I also built this comparison grid comparing them with pelicans by Opus 5, Fable 5.1, and Sonnet 5: Comparing different model vendors by how well they draw a pelican riding a bicycle may not make much sense now (if it ever did), but I''m still finding value in using them for comparisons of the same model families at different reasoning levels. I''m now using GPT-6 Sol and Claude Opus 5.5 as my default models in Codex and Claude Code. I''ve upgraded the Datasette Agent demo at agent.datasette.io to use GPT-6 Luna, and it seems to be fast and competent at both SQL queries and building HTML and JavaScript for Datasette Apps. Tags: ai, openai, generative-ai, llms, anthropic, claude, llm-pricing, pelican-riding-a-bicycle, gpt'
content: extracted
html: 2026-09-22-claude-opus-5-5-gpt-6-sol-gpt-6-luna-and-a-new-price-war.html
preview:
  file: 2026-09-22-claude-opus-5-5-gpt-6-sol-gpt-6-luna-and-a-new-price-war.preview-99de72a6ea47.webp
  width: 256
  height: 128
  color: '#dce5e0'
images:
- source: https://static.simonwillison.net/static/2026/gpt-grid-card.jpg
  original:
    file: 2026-09-22-claude-opus-5-5-gpt-6-sol-gpt-6-luna-and-a-new-price-war.image-a3cc03c8cbda.jpg
    width: 1200
    height: 600
  color: '#f7f7f7'
- source: https://static.simonwillison.net/static/2026/gpt-pelicans-grid.webp
  original:
    file: 2026-09-22-claude-opus-5-5-gpt-6-sol-gpt-6-luna-and-a-new-price-war.image-00732d1a0416.webp
    width: 1568
    height: 1418
  color: '#f6f6f6'
- source: https://static.simonwillison.net/static/2026/claude-pelicans-grid.webp
  original:
    file: 2026-09-22-claude-opus-5-5-gpt-6-sol-gpt-6-luna-and-a-new-price-war.image-9b917ab84e26.webp
    width: 1248
    height: 1460
  color: '#f7f8f8'
---

Yesterday was [Grok 4.7](https://x.ai/news/grok-4-7) ([pelicans](https://news.ycombinator.com/item?id=49788838#49790209)) and [MiMo v2.6 Flash/Pro](https://mimo.xiaomi.com/mimo-v2-6) ([more pelicans](https://news.ycombinator.com/item?id=49792730#49793480)). Today Anthropic [released Claude Opus 5.5](https://www.anthropic.com/claude-opus-5-5), and around an hour later OpenAI [released GPT-6 Sol and GPT-6 Luna](https://openai.com/index/introducing-gpt-6-sol-and-luna/). It’s going to take a while to get a good read on all of these new models, but here are my impressions so far.

#### GPT-6 Sol and Luna are half the price of their GPT-5.6 equivalents

GPT-5.6 Luna was already my favorite model for building applications against, because it combined excellent performance with being *really cheap*. Somehow GPT-6 Luna is half the price of that again—and GPT-6 Sol had a similar reduction compared to GPT-5.6 Sol.

Here’s what the pricing landscape looks like today:

| Model            | Input   | Cached input | Output  |
| ---------------- | ------- | ------------ | ------- |
| GPT-6 Luna       | $0.10/M | $0.01/M      | $0.50/M |
| GPT-5.6 Luna     | $0.20/M | $0.02/M      | $1.20/M |
| Grok 4.7         | $2/M    | $0.50/M      | $6/M    |
| GPT-6 Sol        | $2/M    | $0.20/M      | $10/M   |
| GPT-5.6 Terra    | $2/M    | $0.20/M      | $12/M   |
| Claude Opus 5.5  | $4/M    | $0.20/M      | $20/M   |
| GPT-5.6 Sol      | $4/M    | $0.40/M      | $20/M   |
| Claude Fable 5.1 | $10/M   | $0.25/M      | $50/M   |
| GPT-6 Astra      | $10/M   | $1/M         | $50/M   |

Note that GPT-5.6 has a scheduled 25% price increase for November, so GPT-6 is half the price of the *promotional* pricing for those models.

(With GPT-5.6 Terra priced the same as GPT-6 Sol, any remaining reasons to use Terra just evaporated.)

It’s hard to overstate how competitive this pricing is. Grok 4.7 priced itself at $2/$6, less than half the price of GPT-5.6 Sol, but is now equally priced to GPT-6 Sol on input and closer on output.

At $0.10/$0.50 GPT-6 Luna is one of the cheapest models OpenAI have ever released, beaten only by the far weaker GPT-4.1 Nano ($0.10/$0.40, April 2025) and GPT-5 Nano ($0.05/$0.40, August 2025).

I rendered [pelicans for GPT-6 Luna](https://tools.simonwillison.net/markdown-svg-renderer?url=https%3A%2F%2Fgist.github.com%2Fsimonw%2F40d129fc140faca378b9c9f4f16c6ec2) and [for GPT-6 Sol](https://tools.simonwillison.net/markdown-svg-renderer?url=https%3A%2F%2Fgist.github.com%2Fsimonw%2Fbe7ae25af2634b68bc34b7b7aaf02cb2), then I combined them all together in [this comparison grid](https://static.simonwillison.net/static/2026/gpt-pelicans-grid.html) along with the GPT-5.6 pelicans. I like how you can instantly see that the 5.6 family chose bolder, brighter colors, while the 6 family is a lot more muted. I still think GPT-6 Astra on max produced the best pelican.

![A grid of pelicans for six GPT models at different thinking efforts.](https://static.simonwillison.net/static/2026/gpt-pelicans-grid.webp)

#### Claude Opus 5.5 got a price cut too

Opus 5.5 looks like it addresses the biggest complaints people had about Opus in terms of its communication style. [Thariq Shihipar](https://twitter.com/trq212/status/2102437686967738431):

> Opus 5.5 is the result of your feedback.
>
> It communicates clearly, it’s cheaper per token than Opus 5.0 with the intelligence of Fable 5.1 it’s very token efficient and works across every effort level.

It’s also meant to be [better at Blender](https://twitter.com/alexalbert__/status/2102466523164274839). I’m looking forward to putting it through its paces there.

Opus 4.5, 4.6, 4.7, 4.8, and 5 all shared the same price: $5/million tokens for input and $25/million for output. 5.5 is a 20% reduction—$4/million and $20/million.

The price for cache reads fell 60%. That’s significant for longer agentic conversations, where 90%+ of input tokens are processed at cached token prices.

The new price for Opus 5.5 is the same as the price for GPT-5.6 Sol, but that was *before* OpenAI dropped their Sol prices by half.

GPT-6 Astra and Claude Fable 5.1 are both priced at $10/million input and $50/million output. The price war currently affects the next tier of models below that.

Anthropic say that Sonnet 5.5 and Haiku 5.5 are coming soon. It’s going to be interesting to see if Haiku can regain its price competitiveness at the lower end, given current Haiku 4.5 is $1/$5 while the latest GPT-6 Luna is *one tenth* of that price at $0.10/$0.50.

#### Claude Opus 5.5 max over-thinks to the point of breaking

In a first for my "[Generate an SVG of a pelican riding a bicycle](https://simonwillison.net/tags/pelican-riding-a-bicycle/)" test, Claude Opus 5.5 at "max" thinking level failed to return a response!

It started by calling this “a classic test request”, and then thought really, *really* hard about what it was doing:

> This is a classic test request, so I want to plan out a well-composed pelican with its distinctive beak and pouch riding a bicycle with proper wheels, frame, and pedals, set against a simple sky and ground backdrop. \[...\]
>
> Verifying the shin length checks out at roughly 95.2, close enough. Now I’m working out the near leg path from hip to knee to ankle, then sketching the foot shape resting on the pedal — outlining the heel, toe tips, and sole contour with a path using lines and curves to sit naturally on the pedal surface around y=478-494. \[...\]
>
> I like the fish sticking prominently out of the basket with the pelican eyeing it as a fun detail worth keeping. I’m also confirming the eye placement near the bill base matches typical pelican anatomy, and considering giving it a slightly happier expression. \[...\]
>
> The far leg reads correctly as passing behind the frame, so I’m moving on to check the chainring teeth and confirm layer ordering—the far crank arm should be mostly hidden by the seat tube and chainring. I’m settling on the final SVG’s width and height attributes alongside the viewBox to ensure proper scaling, noting there’s no text so no font-family is needed. \[...\]

I was so excited to see this pelican... but then it [stopped](https://tools.simonwillison.net/markdown-svg-renderer?url=https%3A%2F%2Fgist.github.com%2Fsimonw%2F61fd7c3683fffce9a3ab7c43d1180024#response-4). Opus 5.5 has a 128,000 maximum output token limit (as do the other Claude models), and it hit that while it was still reasoning about the SVG!

I tried a second time and got the same result. This makes me suspect that “max” is effectively useless—if it over-thinks to breaking point on a stupid SVG prompt I don’t trust it not to do the same for more interesting work.

(Those two failures each cost me [$2.56](https://www.llm-prices.com/#it=27&ot=128000&sel=claude-opus-5-5) and took nearly 20 minutes.)

Fable 5.1 on “max” didn’t over-think and did give me [the best pelican I’ve seen](https://simonwillison.net/2026/Sep/1/claude-fable-5-1/#max) from any Anthropic model.

Here are [the Opus 5.5 pelicans](https://tools.simonwillison.net/markdown-svg-renderer?url=https%3A%2F%2Fgist.github.com%2Fsimonw%2F61fd7c3683fffce9a3ab7c43d1180024), excluding 5.5 max.

I also built [this comparison grid](https://static.simonwillison.net/static/2026/claude-pelicans-grid.html) comparing them with pelicans by Opus 5, Fable 5.1, and Sonnet 5:

![A grid of pelicans for four Claude models at different thinking efforts.](https://static.simonwillison.net/static/2026/claude-pelicans-grid.webp)

Comparing different model vendors by how well they draw a pelican riding a bicycle may not make much sense now (if it ever did), but I’m still finding value in using them for comparisons of the same model families at different reasoning levels.

I’m now using GPT-6 Sol and Claude Opus 5.5 as my default models in Codex and Claude Code. I’ve upgraded the Datasette Agent demo at [agent.datasette.io](https://agent.datasette.io/) to use GPT-6 Luna, and it seems to be fast and competent at both SQL queries and building HTML and JavaScript for [Datasette Apps](https://simonwillison.net/2026/Jun/18/datasette-apps/).
