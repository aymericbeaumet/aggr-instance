---
title: 'Show HN: LLM Attention Visualization'
link: https://ishamf.dev/p/llm-attention-visualizer/
source: hnrss-org-frontpage
published: 2026-09-08T16:59:41Z
updated: 2026-09-08T16:59:41Z
first_seen: 2026-09-08T22:56:52.093968329Z
authors:
- ifz
summary: 'Article URL: https://ishamf.dev/p/llm-attention-visualizer/ Comments URL: https://news.ycombinator.com/item?id=49613068 Points: 102 # Comments: 19'
content: extracted
html: 2026-09-08-show-hn-llm-attention-visualization.html
---

One interesting thing about transformer-based large language models are that, during the generation phase, it is able to draw information from any of its previous tokens. But it needs to be selective; if every token affects the generation equally, it won't be very effective. This process needs a mechanism to decide how much a token affects the next token.

Turns out, we can visualize this mechanism!

You can tap or hover over any of the generated tokens to see the past tokens that affected\* the generation.

Loading... (JavaScript required)

\* "Affected" might not be fully accurate, as this visualization is highly simplified. It's calculating the attention weight, scaled by the magnitude of the value vector, aggregated across all attention heads, and summed across all layers. This is then used to control the opacity of the previous tokens. The largest values always have an opacity of 1 and the rest are interpolated.

A lot of information had to be thrown away to limit the visualization to just one numeric value per past token. Because of that, when I started implementing this, I actually thought it might not be comprehensible. But it actually can produce some interesting patterns!

For example, in the default "Office Move Summary" prompt, you can hover over the text that are copied verbatim like the address and dates. You can then see the original data stand out quite a bit, because the generated token takes up a lot of the information from the source data.

This addresses one thing that I've previously found unintuitive about LLMs. If they work by predicting the next tokens probabilistically, why are they somehow so good at copy-pasting stuff? Won't they eventually make a mistake just by random chance?

But with this mechanism, you can see that it doesn't predict the entire sequence from some limited internal states. Since it has access to all past tokens, it can just decide which past tokens to draw from when copying, and so the probability of errors can be very low. In the "Debugging an Average Function" example, you can see that this quite small model (600 million parameters) can easily reproduce an entire JS function except for the intended modification. (Although it's not actually capable of finding the issue by itself, so it needed some hints.)

Another interesting part is when you hover over the "remain" in "Existing access cards and phone numbers **remain**" in the "Office Move Summary" prompt. You can see that it draws from "work" in "Existing employee access cards will **work**" and "stay the same" in "company phone numbers will **stay the same**". So it's kind of combining the information from the words in both phrases, which I find quite cool.

## Implementation

The visualization itself is a pretty basic React app using Transformers.js to generate the text. But, since we need to pull more data out of the model to visualize it, it can't use the regular generation loop. I had to vibe-code the generation loop in the app so we can actually keep track of the values to visualize.

Despite using a smaller model for this, it's still hundreds of megabytes, and waiting for it to download before showing anything just won't work. So I pre-generated a bunch of prompts that can be loaded and viewed instantly.

Another tricky thing is that some of the things in the visualization are not actually meant to be read, so they're not defined as outputs. I suppose if you're implementing this using Python ML libraries, it would still be easy to access them. But Transformers.js uses .onnx files that contains the entire computation graph. The model loading and computation logic is implemented in wasm, so there's no easy way to access anything other than the predefined outputs, as far as I can tell.

In the end, I used a small script to modify the onnx file just enough to expose those internal values. But that means I can't just use the regular .onnx model. Since I want to have a browser-based generation feature, I have to upload a separate instrumented model to my own [Hugging Face repo](https://huggingface.co/ishamf/Qwen3-0.6B-ONNX-Instrumented) and point the app there.

You can find the code in the [GitHub repo](https://github.com/ishamf/llm-visualizer).
