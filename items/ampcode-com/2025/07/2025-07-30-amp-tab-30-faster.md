---
title: Amp Tab 30% Faster
link: https://ampcode.com/news/amp-tab-30-percent-faster
source: ampcode-com
published: 2025-07-30T00:00:00Z
updated: 2025-07-30T00:00:00Z
first_seen: 2026-10-04T12:50:30.804479567Z
summary: Response times of Amp Tab, our in-editor completion engine, are now 30% faster, with up to 50% improvements during peak usage. We worked together with Baseten to optimize our custom deployment. The new infrastructure delivers roughly 2x performance improvements by switching to TensorRT-LLM as the inference engine and implementing KV caching with speculative decoding. This new infrastructure also includes a modified version of lookahead decoding that uses an improved n-gram candidate selection algorithm and variable-length speculations, which reduces both draft tokens and compute per iteration compared to standard implementations.
content: extracted
html: 2025-07-30-amp-tab-30-faster.html
remote_preview:
  url: https://static.ampcode.com/news/next-cursor-latency-trend-dark.png
  alt: Chart showing Amp Tab latency improvements over time
---

Response times of [Amp Tab](https://ampcode.com/manual#amp-tab), our in-editor completion engine, are now 30% faster, with up to 50% improvements during peak usage.

We worked together with [Baseten](https://www.baseten.co/) to optimize our custom deployment. The new infrastructure delivers roughly 2x performance improvements by switching to TensorRT-LLM as the inference engine and implementing KV caching with speculative decoding.

This new infrastructure also includes a modified version of lookahead decoding that uses an improved n-gram candidate selection algorithm and variable-length speculations, which reduces both draft tokens and compute per iteration compared to standard implementations.

![Chart showing Amp Tab latency improvements over time](https://static.ampcode.com/news/next-cursor-latency-trend-dark.png)
