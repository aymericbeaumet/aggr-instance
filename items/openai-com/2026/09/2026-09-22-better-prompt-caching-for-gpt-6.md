---
title: Better prompt caching for GPT-6
link: https://openai.com/index/better-prompt-caching-for-gpt-6
source: openai-com
published: 2026-09-22T21:00:00Z
updated: 2026-09-22T21:00:00Z
first_seen: 2026-10-19T01:26:53.998049939Z
labels:
- product
summary: Learn how GPT-6 improves prompt caching with higher cache hit rates, new diagnostics, explicit breakpoints, and controls that reduce latency and costs.
content: extracted
html: 2026-09-22-better-prompt-caching-for-gpt-6.html
---

GPT‑6 enables persistent agents to work for hours on complex tasks, from refactoring codebases to producing well-researched documents and presentations. The applications behind these agents make a series of API requests that build on one another, often carrying forward the same instructions, tool definitions, and context from earlier turns. OpenAI caches that shared context to reuse computation across requests, reducing response times and giving developers discounts of up to 90% on cached input tokens.

With the GPT‑6 family, we launched an improved prompt caching system that delivers higher cache hit rates by default. We now give cache discounts for eligible shared prefixes reused within a 30-minute window. We’re also introducing new tools to help developers monitor cache performance, diagnose misses, and choose how much of a prompt to cache.

## Monitor caching and diagnose cache misses

The new [Prompt Caching Dashboard](https://platform.openai.com/usage?usage_section=prompt-caching) shows how much of your application’s input is served from cache. Track hit rates over time and use the input composition chart to compare cached and uncached tokens. These views help you spot drops in cache hits and evaluate how changes to your application impact caching performance.

When you see an unexpected cache miss, use the [prompt caching diagnostics tool](https://developers.openai.com/api/docs/guides/prompt-caching/diagnostics) to understand what happened. Compare a request with a recent response to identify changes to the model, tools, settings, or input that prevented reuse. The estimated number of affected tokens helps you assess the size of the impact and decide how you can optimize your integration to maximize cache hit rates.

```json
{
  "prompt_cache_diagnostics": {
    "type": "cache_miss",
    "reason": "tools_changed",
    "comparison_reusable_tokens": 5629,
    "cache_missed_tokens": 5629
  }
}
```

## Optimize caching for your application

**Choose what to cache.** Explicit cache breakpoints let you choose which prompt prefixes to reuse. The refreshed [prompt caching guide](https://developers.openai.com/api/docs/guides/prompt-caching) explains how to use them, how long cached prefixes remain eligible, and how changes to tools and inputs affect reuse.

**Adjust reasoning effort without breaking cache.** On GPT‑6 models, you can now [change reasoning effort](https://developers.openai.com/api/docs/guides/reasoning?api-mode=responses#change-reasoning-mid-conversation) between responses without breaking cache. Raise effort for a harder task or lower it for a routine follow-up by appending a `configuration_update` while leaving request-level reasoning effort unchanged. This lets you adjust how much reasoning a task needs while preserving reusable context.

**Preserve cache as tools and instructions change.** As your agent’s tool use needs change, keep tool definitions, schemas, and ordering stable so earlier context stays reusable. Use `allowed_tools` to make only the relevant tools callable, or set tool\_choice to none when no tools are needed, instead of removing definitions. Use new developer messages to append new instructions towards the end of the context to override older ones. See our [guidance on managing tool changes](https://developers.openai.com/api/docs/guides/prompt-caching#manage-tools-with-append-only-updates).

**Prewarm the cache to reduce latency.** [Prewarming](https://developers.openai.com/api/docs/guides/prompt-caching#prewarm-the-cache) prepares known context ahead of time so the model can start responding sooner when a request arrives. For example, an application can prewarm shared instructions, tool definitions, or reference material during startup, before the user asks their first question. This moves processing out of the user’s wait time.

These optional controls build on the engine’s default performance, helping you tailor caching to your workload.

1 of 3

## Get started

- Monitor cache hit rates in the [Prompt Caching Dashboard](https://platform.openai.com/usage?usage_section=prompt-caching).

- Investigate unexpected misses with the [diagnostics tool](https://developers.openai.com/api/docs/guides/prompt-caching/diagnostics).

- Follow the [prompt caching guide](https://developers.openai.com/api/docs/guides/prompt-caching) to improve your setup, or [use Codex](https://developers.openai.com/api/docs/guides/prompt-caching#how-to-optimize-prompt-caching) to review your code, apply improvements, and measure results.
