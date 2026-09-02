---
title: Introducing Hy4 Preview
link: https://simonwillison.net/2026/Aug/29/hy4/
source: simon-willison
published: 2026-08-29T23:53:13Z
updated: 2026-08-29T23:53:13Z
first_seen: 2026-09-02T14:07:29.769486161Z
tags:
- ai
- generative-ai
- llms
- pelican-riding-a-bicycle
- llm-reasoning
- llm-release
- ai-in-china
summary: 'Introducing Hy4 Preview New open weight text input (no vision) LLM from Chinese company Tencent today: 770B total parameters, 49B active parameters, 1M token context window, 1.56TB on Hugging Face. This is a big size increase from their previous Hy3 in July, which was 295B, 21B active, 256,000 context, 598GB. I recently started using model chat templates to better understand their capabilities. Here''s Hy4''s chat_template.jinja on Hugging Face, which includes this section: {%- if not reasoning_effort is defined %} {%- set reasoning_effort = ''high'' %} {%- elif reasoning_effort not in [''high'', ''no_think''] %} {%- if reasoning_effort is none %} {{- raise_exception(''reasoning_effort error : None, should be no_think/high'') }} {%- else %} {{- raise_exception(''reasoning_effort error : '' + reasoning_effort + '', should be no_think/high'') }} {%- endif %} {%- endif %} So it looks like there are just two reasoning effort levels: "high" (the default) and "no_think" (reason by disabled). I tried my "Generate an SVG of a pelican riding a bicycle" prompt with the default high reasoning via OpenRouter and got this: Quoting the reasoning trace: [...] Let''s maybe add a helmet? It could improve riding theme, but may obscure head. Maybe a small cycling cap or helmet? The user didn''t ask; can add red helmet? Might be cute. But pelican with big beak; a helmet might obscure. Better maybe no. Maybe add sunglasses? no. Maybe add water? no. It''s interesting how the reasoning trace uses slightly truncated English, presumably because perfect grammar isn''t useful or token efficient for hidden reasoning text. Tags: ai, generative-ai, llms, pelican-riding-a-bicycle, llm-reasoning, llm-release, ai-in-china'
content: feed
html: 2026-08-29-introducing-hy4-preview.html
---

**[Introducing Hy4 Preview](https://hy.tencent.ai/research/hy4-preview)**

New open weight text input (no vision) LLM from Chinese company Tencent today: 770B total parameters, 49B active parameters, 1M token context window, [1.56TB on Hugging Face](https://huggingface.co/tencent/Hy4-preview).

This is a big size increase from their previous [Hy3](https://huggingface.co/tencent/Hy3) in July, which was 295B, 21B active, 256,000 context, 598GB.

I recently started using model chat templates to better understand their capabilities. Here's Hy4's [chat\_template.jinja](https://huggingface.co/tencent/Hy4-preview/blob/main/chat_template.jinja) on Hugging Face, which includes this section:

{%- if not reasoning_effort is defined %}
    {%- set reasoning_effort = 'high' %}
{%- elif reasoning_effort not in ['high', 'no_think'] %}
    {%- if reasoning_effort is none %}
        {{- raise_exception('reasoning_effort error : None, should be no_think/high') }}
    {%- else %}
        {{- raise_exception('reasoning_effort error : ' + reasoning_effort + ', should be no_think/high') }}
    {%- endif %}
{%- endif %}

So it looks like there are just two reasoning effort levels: "high" (the default) and "no\_think" (reason by disabled).

I tried my "Generate an SVG of a pelican riding a bicycle" prompt with the default high reasoning [via OpenRouter](https://openrouter.ai/tencent/hy4-preview#apps) and [got this](https://tools.simonwillison.net/markdown-svg-renderer#url=https%3A%2F%2Fgist.github.com%2Fsimonw%2Fcb69816b3fb940f2782569a82a523af1):

![Flat vector cartoon illustration of a white pelican with a large orange bill riding a red bicycle to the right along a grey road with a dashed white centre line, its orange webbed feet on the pedals and grey tail feathers fanned out behind, against a pale blue sky with a yellow sun, white clouds and horizontal white motion lines suggesting speed](https://static.simonwillison.net/static/2026-08-29/IMG_7725.jpeg)

Quoting the reasoning trace:

> \[...\] Let's maybe add a helmet? It could improve riding theme, but may obscure head. Maybe a small cycling cap or helmet? The user didn't ask; can add red helmet? Might be cute. But pelican with big beak; a helmet might obscure. Better maybe no.
>
> Maybe add sunglasses? no.
>
> Maybe add water? no.

It's interesting how the reasoning trace uses slightly truncated English, presumably because perfect grammar isn't useful or token efficient for hidden reasoning text.

Tags: [ai](https://simonwillison.net/tags/ai), [generative-ai](https://simonwillison.net/tags/generative-ai), [llms](https://simonwillison.net/tags/llms), [pelican-riding-a-bicycle](https://simonwillison.net/tags/pelican-riding-a-bicycle), [llm-reasoning](https://simonwillison.net/tags/llm-reasoning), [llm-release](https://simonwillison.net/tags/llm-release), [ai-in-china](https://simonwillison.net/tags/ai-in-china)
