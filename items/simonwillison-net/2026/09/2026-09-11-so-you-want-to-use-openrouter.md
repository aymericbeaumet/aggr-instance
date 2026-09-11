---
title: So you want to use OpenRouter?
link: https://simonwillison.net/2026/Sep/11/so-you-want-to-use-openrouter/
source: simonwillison-net
published: 2026-09-11T22:49:18Z
updated: 2026-09-11T22:49:18Z
first_seen: 2026-09-11T23:11:51.018809841Z
labels:
- ai
- generative-ai
- llms
- openrouter
summary: 'So you want to use OpenRouter? One of OpenRouter''s selling points is that it "handles fallbacks automatically and picks the most cost-effective option for each request", so you can call a single API endpoint for a model and get routed to the best available backend provider. Mohamed Moustafa points out a whole set of ways that this can cause you problems. Different providers run different serving software with different optimizations and settings, which means that the same OpenRouter endpoint can serve model requests that behave in different ways. Some providers even lack vision capability for vision models, and the way the reasoning effort option is processed can differ as well. Thankfully you can control which provider is routed to using the provider.only option. The /endpoints method returns the list of available providers for a specific model ID. Via Hacker News Tags: ai, generative-ai, llms, openrouter'
content: extracted
html: 2026-09-11-so-you-want-to-use-openrouter.html
---

**[So you want to use OpenRouter?](https://mmoustafa.com/blog/so-you-want-to-use-openrouter/)** ([via](https://news.ycombinator.com/item?id=49621546 "Hacker News")) One of OpenRouter's selling points is that it "handles fallbacks automatically and picks the most cost-effective option for each request", so you can call a single API endpoint for a model and get routed to the best available backend provider.

Mohamed Moustafa points out a whole set of ways that this can cause you problems. Different providers run different serving software with different optimizations and settings, which means that the same OpenRouter endpoint can serve model requests that behave in different ways.

Some providers even lack vision capability for vision models, and the way the reasoning effort option is processed can differ as well.

Thankfully you can control which provider is routed to using [the provider.only option](https://openrouter.ai/docs/guides/routing/provider-selection#allowing-only-specific-providers). The [/endpoints method](https://openrouter.ai/docs/api/api-reference/endpoints/list-all-endpoints-for-a-model) returns the list of available providers for a specific model ID.
