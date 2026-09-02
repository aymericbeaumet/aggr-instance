---
title: Claude Fable 5.1 made me a really nice animated pelican
link: https://simonwillison.net/2026/Sep/1/claude-fable-5-1/
source: simon-willison
published: 2026-09-01T23:57:28Z
updated: 2026-09-01T23:57:28Z
first_seen: 2026-09-02T14:07:29.769486161Z
tags:
- ai
- generative-ai
- llms
- anthropic
- claude
- pelican-riding-a-bicycle
- llm-reasoning
- llm-release
summary: 'Today is Claude Fable (and Mythos) 5.1 day. Anthropic say that Fable 5.1 "sets a new standard for coding, knowledge work, and long-running problem-solving tasks". Their announcement spends a notable amount of time on scientific research, boasting of a 52.6% score on the brand new Terminal-Bench-Science 0.1 benchmark (first announced on August 27th), up from 24.7% for Fable 5, 29.0% for Opus 5 and 22.4% for GPT-5.6 Sol. Other benchmarks show slightly improved scores, but none as impressive as the Science one. But how well can it pelican? Back in July I wrote about how I was losing faith in the pelican benchmark - its connection to how good the models were at other tasks didn''t seem to hold as strongly as it did back in 2025. The most interesting insights I get from it now are comparisons within model families, and particularly comparisons for the same prompt at different reasoning effort levels. Fable 5.1 has five reasoning levels: low, medium, high, xhigh, max - and no option to turn off reasoning entirely. I fixed an issue in llm-anthropic which caused reasoning traces not to be correctly recorded, then ran some prompts. Here''s the full set of pelicans for all of the reasoning levels, each with the full reasoning transcript. I''ll replicate them here: Low and medium, both without reasoning? Next, a bit of a mystery. This is what I got for effort low: The transcript doesn''t show any summarized reasoning tokens, and the output token count is 1,998. With Claude that output token count includes reasoning tokens. It took 23.8 seconds and cost 10.017 cents. I bumped that up to medium and got this: Weirdly, that one also shows no reasoning text and used 1,977 output tokens - 21 tokens less than low. It took 23 seconds and cost 9.912 cents. So for this particular prompt ("Generate an SVG of a pelican riding a bicycle") Fable 5.1 appeared to skip reasoning entirely at both low and medium settings. High Here''s high - 29.6 seconds, 2,612 output tokens, 13.087 cents: This one did do a bit of reasoning, summary here: I''m planning the SVG layout for a pelican riding a bicycle, with a sky and ground background, a bicycle with two spoked wheels, frame, seat and handlebars, and a white-bodied pelican with a long neck and orange beak positioned on top. Really not much difference from low and medium, though. Extra High At xhigh things got radically different. 36,767 output tokens, 7 minutes 51 seconds, $1.83! The reasoning trace is pretty lengthy, and includes details like this: Adding the eye, wings stretching down to the handlebar grip, orange legs reaching to the pedals, and a small tail feather, while keeping the pelican intentionally oversized compared to the bike for comic effect. [...] I''ll accept the slight thickness as charming rather than overengineering it. Max Setting effort to max gave me the best pelican I''ve seen from any of Anthropic''s models. 65,927 output tokens, 13 minutes and 54 seconds, $3.30: There''s a lot to like about this. The background is tasteful, the legs are clearly on either side of the frame, the feet are on the pedals, the wing is on the handlebars, the pelican has a cute blue hat and there''s a basket with a fish. It''s still not showing nearly the same level of flair as Gemini 3.7 Flash, but I didn''t ask for flair - I asked for an SVG, and that''s what I got. Some highlights from that reasoning trace: Adding pedal shapes near both feet, with the far foot on the second leg partially visible behind the frame. I''m considering whether to add a small scarf or cap for extra character, but leaning toward keeping it simple to avoid clutter. Now I''m debating a bicycle helmet on the head versus the pelican''s signature crest—the beak and pouch already read clearly as "pelican," so a helmet could reinforce the bicycle theme without losing identity, though it might compete with the crest for visual space. I realize the beak at (484,84) would overlap with the dome helmet, so I need to shrink the helmet so it only covers the top of the head, adjusting its arc endpoints to sit higher and narrower so the beak can attach cleanly at the front without collision. [...] I''m adding a darker tip region to represent the primary feathers, then reconsidering the trailing edge to include scalloped feather curves instead of one smooth line for a more natural look. [...] Now I''m checking the vent line placements on the helmet, making sure they sit far enough inside the helmet''s edge given the stroke width and rounded caps, and confirming each vent stays within the helmet''s circular boundary. [...] I decide skipping a handlebar bell and tire highlights since they''re unnecessary additions. Now I''m reconsidering the front fork''s curve — the current control point pulls the shape backward when it should bow forward for a proper rake, so I need to shift the control point rightward to fix the fork''s lean. OK, let''s animate it On Hacker News, swalsh commented on that Max pelican: Now that it''s a solved benchmark, can we get the animated version? I didn''t want to spend another $3 so I took the Max pelican and piped it into the default thinking level of High: llm logs -cx | llm -m claude-fable-5.1 -s ''animate this'' 6,121 input, 26,201 output = $1.37. The result looked like this, exported here as video since some people have trouble viewing animated SVGs: Your browser does not support HTML5 video. The wheels in the video are rotating in the wrong direction, but I think that''s an artifact of the conversion to MP4 - they seem to be going in the correct direction in the original SVG. Tags: ai, generative-ai, llms, anthropic, claude, pelican-riding-a-bicycle, llm-reasoning, llm-release'
content: feed
html: 2026-09-01-claude-fable-5-1-made-me-a-really-nice-animated-pelican.html
---

Today is [Claude Fable (and Mythos) 5.1 day](https://www.anthropic.com/claude-fable-and-mythos-5-1). Anthropic say that Fable 5.1 "sets a new standard for coding, knowledge work, and long-running problem-solving tasks". Their announcement spends a notable amount of time on scientific research, boasting of a 52.6% score on the brand new [Terminal-Bench-Science 0.1](https://www.terminal-bench-science.ai) benchmark (first announced [on August 27th](https://www.tbench.ai/news/terminal-bench-science-0-1)), up from 24.7% for Fable 5, 29.0% for Opus 5 and 22.4% for GPT-5.6 Sol. Other benchmarks show slightly improved scores, but none as impressive as the Science one.

But how well can it pelican?

Back in July [I wrote about](https://simonwillison.net/2026/Jul/16/kimi-k3/) how I was losing faith in the pelican benchmark - its connection to how good the models were at other tasks didn't seem to hold as strongly as it did [back in 2025](https://simonwillison.net/2025/Jun/6/six-months-in-llms/). The most interesting insights I get from it now are comparisons within model families, and particularly comparisons for the same prompt at different reasoning effort levels.

Fable 5.1 has five reasoning levels: low, medium, high, xhigh, max - and no option to turn off reasoning entirely.

I fixed [an issue](https://github.com/simonw/llm-anthropic/issues/88) in [llm-anthropic](https://github.com/simonw/llm-anthropic) which caused reasoning traces not to be correctly recorded, then ran some prompts.

Here's [the full set of pelicans](https://tools.simonwillison.net/markdown-svg-renderer?url=https%3A%2F%2Fgist.github.com%2Fsimonw%2F17318f748f8c2b476051ddc2ebeb94a7) for all of the reasoning levels, each with the full reasoning transcript. I'll replicate them here:

#### Low and medium, both without reasoning?

Next, a bit of a mystery. This is what I got for effort `low`:

![Minimalist flat illustration of a white pelican with an orange beak riding a black bicycle to the left, its orange legs pedaling and wings gripping the handlebars, with motion lines behind on a light blue background.](https://static.simonwillison.net/static/2026/fable-5.1-low.png)

The [transcript](https://tools.simonwillison.net/markdown-svg-renderer?url=https%3A%2F%2Fgist.github.com%2Fsimonw%2F17318f748f8c2b476051ddc2ebeb94a7#options) doesn't show any summarized reasoning tokens, and the output token count is 1,998. With Claude that output token count includes reasoning tokens. It took 23.8 seconds and cost [10.017 cents](https://www.llm-prices.com/#it=27&ot=1998&sel=claude-fable-5-1).

I bumped that up to `medium` and got this:

![Minimalist flat-style illustration of a white pelican with an orange beak riding a black bicycle to the right, with motion lines behind it, on a light blue background.](https://static.simonwillison.net/static/2026/fable-5.1-medium.png)

Weirdly, that one also shows [no reasoning text](https://tools.simonwillison.net/markdown-svg-renderer?url=https%3A%2F%2Fgist.github.com%2Fsimonw%2F17318f748f8c2b476051ddc2ebeb94a7#options-1) and used 1,977 output tokens - 21 tokens *less* than `low`. It took 23 seconds and cost [9.912 cents](https://www.llm-prices.com/#it=27&ot=1977&sel=claude-fable-5-1).

So for this particular prompt ("Generate an SVG of a pelican riding a bicycle") Fable 5.1 appeared to skip reasoning entirely at both `low` and `medium` settings.

#### High

Here's `high` - 29.6 seconds, 2,612 output tokens, [13.087 cents](https://www.llm-prices.com/#it=27&ot=2612&sel=claude-fable-5-1):

![Minimalist flat illustration of a white pelican with an orange beak riding a black bicycle, its orange legs pedaling, with motion lines behind it on a light blue background.](https://static.simonwillison.net/static/2026/fable-5.1-high.png)

This one did do a *bit* of reasoning, [summary here](https://tools.simonwillison.net/markdown-svg-renderer?url=https%3A%2F%2Fgist.github.com%2Fsimonw%2F17318f748f8c2b476051ddc2ebeb94a7#reasoning):

> I'm planning the SVG layout for a pelican riding a bicycle, with a sky and ground background, a bicycle with two spoked wheels, frame, seat and handlebars, and a white-bodied pelican with a long neck and orange beak positioned on top.

Really not much difference from `low` and `medium`, though.

#### Extra High

At `xhigh` things got *radically* different. 36,767 output tokens, 7 minutes 51 seconds, [$1.83](https://www.llm-prices.com/#it=27&ot=36767&sel=claude-fable-5-1)!

![Minimalist flat illustration of a white pelican with an orange beak riding a black bicycle to the left, its orange legs pedaling, with motion lines behind it on a light blue background.](https://static.simonwillison.net/static/2026/fable-5.1-xhigh.png)

The reasoning trace [is pretty lengthy](https://tools.simonwillison.net/markdown-svg-renderer?url=https%3A%2F%2Fgist.github.com%2Fsimonw%2F17318f748f8c2b476051ddc2ebeb94a7#reasoning-1), and includes details like this:

> Adding the eye, wings stretching down to the handlebar grip, orange legs reaching to the pedals, and a small tail feather, while keeping the pelican intentionally oversized compared to the bike for comic effect. \[...\]
>
> I'll accept the slight thickness as charming rather than overengineering it.

#### Max

Setting effort to `max` gave me the best pelican I've seen from any of Anthropic's models. 65,927 output tokens, 13 minutes and 54 seconds, [$3.30](https://www.llm-prices.com/#it=27&ot=65927&sel=claude-fable-5-1):

![Minimalist flat illustration of a white pelican with an orange beak riding a black bicycle, its orange legs pedaling, with motion lines behind to indicate speed, on a light blue background.](https://static.simonwillison.net/static/2026/fable-5.1-max.webp)

There's a lot to like about this. The background is tasteful, the legs are clearly on either side of the frame, the feet are on the pedals, the wing is on the handlebars, the pelican has a cute blue hat and there's a basket with a fish.

It's still not showing nearly the same level of flair [as Gemini 3.7 Flash](https://simonwillison.net/2026/Aug/13/llm-gemini/), but I didn't *ask* for flair - I asked for an SVG, and that's what I got.

Some highlights from [that reasoning trace](https://tools.simonwillison.net/markdown-svg-renderer?url=https%3A%2F%2Fgist.github.com%2Fsimonw%2F17318f748f8c2b476051ddc2ebeb94a7#reasoning-2):

> Adding pedal shapes near both feet, with the far foot on the second leg partially visible behind the frame. I'm considering whether to add a small scarf or cap for extra character, but leaning toward keeping it simple to avoid clutter.
>
> Now I'm debating a bicycle helmet on the head versus the pelican's signature crest—the beak and pouch already read clearly as "pelican," so a helmet could reinforce the bicycle theme without losing identity, though it might compete with the crest for visual space.
>
> I realize the beak at (484,84) would overlap with the dome helmet, so I need to shrink the helmet so it only covers the top of the head, adjusting its arc endpoints to sit higher and narrower so the beak can attach cleanly at the front without collision. \[...\]
>
> I'm adding a darker tip region to represent the primary feathers, then reconsidering the trailing edge to include scalloped feather curves instead of one smooth line for a more natural look. \[...\]
>
> Now I'm checking the vent line placements on the helmet, making sure they sit far enough inside the helmet's edge given the stroke width and rounded caps, and confirming each vent stays within the helmet's circular boundary. \[...\]
>
> I decide skipping a handlebar bell and tire highlights since they're unnecessary additions. Now I'm reconsidering the front fork's curve — the current control point pulls the shape backward when it should bow forward for a proper rake, so I need to shift the control point rightward to fix the fork's lean.

#### OK, let's animate it

On Hacker News, [swalsh commented](https://news.ycombinator.com/item?id=49525378#49526455) on that Max pelican:

> Now that it's a solved benchmark, can we get the animated version?

I didn't want to spend another $3 so I took the Max pelican and piped it into the default thinking level of High:

llm logs -cx | llm -m claude-fable-5.1 -s 'animate this'

6,121 input, 26,201 output = [$1.37](https://www.llm-prices.com/#it=6121&ot=26201&sel=claude-fable-5-1). The result [looked like this](https://tools.simonwillison.net/markdown-svg-renderer?url=https%3A%2F%2Fgist.github.com%2Fsimonw%2F87282467acb3652e0f99c85155554a32#response), exported here as video since some people have trouble viewing animated SVGs:

Your browser does not support HTML5 video.

The wheels in the video are rotating in the wrong direction, but I think that's an artifact of the conversion to MP4 - they seem to be going in the correct direction in the original SVG.

Tags: [ai](https://simonwillison.net/tags/ai), [generative-ai](https://simonwillison.net/tags/generative-ai), [llms](https://simonwillison.net/tags/llms), [anthropic](https://simonwillison.net/tags/anthropic), [claude](https://simonwillison.net/tags/claude), [pelican-riding-a-bicycle](https://simonwillison.net/tags/pelican-riding-a-bicycle), [llm-reasoning](https://simonwillison.net/tags/llm-reasoning), [llm-release](https://simonwillison.net/tags/llm-release)
