---
title: What Is Reasoning
link: https://lucumr.pocoo.org/2026/8/19/what-is-reasoning/
source: lucumr-pocoo-org
published: 2026-08-19T00:00:00Z
updated: 2026-08-19T00:00:00Z
first_seen: 2026-09-19T21:30:23.395188495Z
authors:
- Armin Ronacher
content: extracted
html: 2026-08-19-what-is-reasoning.html
preview:
  file: 2026-08-19-what-is-reasoning.preview-eafc2cf59486.webp
  width: 256
  height: 134
  color: '#1f3559'
images:
- source: https://lucumr.pocoo.org/social/2026-08-19-what-is-reasoning-social.png
  original:
    file: 2026-08-19-what-is-reasoning.image-e053d9fc35cb.png
    width: 1200
    height: 630
  variants:
  - file: 2026-08-19-what-is-reasoning.image-6e47ca9ed69b.webp
    width: 320
    height: 168
  - file: 2026-08-19-what-is-reasoning.image-9bb5f8224075.webp
    width: 640
    height: 336
  - file: 2026-08-19-what-is-reasoning.image-10ff8931dbf6.webp
    width: 1200
    height: 630
  color: '#1b3156'
- source: https://lucumr.pocoo.org/static/gpt-5.6-terra-spell-check.png
  original:
    file: 2026-08-19-what-is-reasoning.image-4ece7bb1fce1.png
    width: 1430
    height: 954
  variants:
  - file: 2026-08-19-what-is-reasoning.image-ce8bb184c675.webp
    width: 320
    height: 213
  - file: 2026-08-19-what-is-reasoning.image-f09cc36b4844.webp
    width: 640
    height: 427
  - file: 2026-08-19-what-is-reasoning.image-c34b43101285.webp
    width: 960
    height: 640
  - file: 2026-08-19-what-is-reasoning.image-a36806e67f02.webp
    width: 1430
    height: 954
  color: '#f6f7f6'
---

A few weeks ago [a paper was shared](https://arxiv.org/html/2608.09867v1) that showed how to extract reasoning traces from closed-weight models. Together with online discussions about tricking models into leaking them, it made me investigate it more out of curiosity. Twitter seems full of half-truths and confusion about how this works, so perhaps this helps some to understand what is happening.

## Hiding Traces

Reasoning traces are usually hidden from us. [We have lamented this](https://earendil.com/posts/session-portability/), but mostly have to accept it. Open-weight models thankfully reveal them, and from their behavior you can see that their traces can be long and confusing. This is probably a good reason to separate them from what is normally shown to users.

At minimum, UIs need to detect them. The industry has done a good job at making reasoning traces sound special and exotic, but they really are just text: the model is trained to emit its thinking into a scratchpad as part of its response, before its final answer.

GPT-OSS’s Harmony response format makes this easy to see:

```
<|channel|>analysis<|message|>
I need to work this out ...
<|end|><|start|>assistant<|channel|>final<|message|>
The answer is ...
<|return|>
```

The markers are special tokens, but the reasoning between them uses “the same text” as the final answer (just that GPT chain-of-thought text sounds really funny). When the model samples the `analysis` channel token, a parser routes the following text into a separate stream exposed through the Responses API. For closed models, presumably a simple model redacts and summarizes it.

## Reasoning Effort

How much budget goes to reasoning? Earlier APIs exposed reasoning token budgets, making it seem like a property of the sampling process. In reality, reasoning effort is baked into the system prompt. GPT-OSS puts this into the system prompt:

```
Reasoning: low
```

That’s it. Training produces the resulting behavior, such as emitting the token sequence that switches to the `analysis` channel. This also explains why changing the effort invalidates the KV cache. I think closed GPT models call reasoning effort “juice,” since you can ask most models how much juice they have.

In [DwarfStar](https://github.com/antirez/ds4) for DeepSeek with max reasoning this is added to the system prompt:

```
Reasoning Effort: Absolute maximum with no shortcuts permitted.
You MUST be very thorough in your thinking and comprehensively decompose the
problem to resolve the root cause, rigorously stress-testing your logic against
all potential paths, edge cases, and adversarial scenarios.
```

## Don’t Think

The destination of reasoning tokens is therefore a learned convention: the model is trained to keep scratch work out of the `final` channel. Trick it into thinking it is in that channel and it may leak tokens. We have even seen older models, when thinking is disabled, reason into the bash tool and echo their thoughts to `/dev/null`.

So in some sense the only “special” behavior for some models is not to think. That at times is done by “mechanically” removing the model’s usual ways to think. In [DwarfStar](https://github.com/antirez/ds4), disabled thinking uses the prefill `</think>`, while enabled thinking uses `<think>`, which are the tokens that close and start thinking. GPT-OSS doesn’t prefill but lets the model decide either way on its own.

But presumably, some inference APIs prefill the opening token when reasoning is enabled, so the model never samples it itself and might prevent the sampling of the reasoning token when disabled since it can be trivially detected. This may explain why a [custom `think` tool](https://gist.github.com/mitsuhiko/0904a3d89741e8e3bcca1ca93ea076de) can trick models into putting some reasoning where it should not go — but only when native reasoning is disabled.

Fun fact: this blog post triggered safey checks

Hilariously enough I was unable to use GPT 5.6 terra for spell and grammar checking on this blog post because of safety filters. Had to switch to Kimi.

![GPT-5.6-terra refusing to spell-check this blog post](https://lucumr.pocoo.org/static/gpt-5.6-terra-spell-check.png)

This entry was tagged [ai](https://lucumr.pocoo.org/tags/ai/)

[copy as](https://lucumr.pocoo.org/2026/8/19/what-is-reasoning.md) / [view](https://lucumr.pocoo.org/2026/8/19/what-is-reasoning.md) markdown
