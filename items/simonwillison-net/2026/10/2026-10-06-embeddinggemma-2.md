---
title: EmbeddingGemma 2
link: https://simonwillison.net/2026/Oct/6/hn-49983751/
source: simonwillison-net
published: 2026-10-06T20:37:53Z
updated: 2026-10-06T20:37:53Z
first_seen: 2026-10-06T23:45:23.660353536Z
labels:
- ai
- embeddings
- gemma
- generative-ai
- google
summary: 'My comment on EmbeddingGemma 2 — Hacker News. I really appreciate that EmbeddingGemma 2 is under the Apache 2.0 license. For embedding models in particular, I don''t think it makes sense to use a closed, proprietary, hosted-only model. Most applications of embedding models involve calculating thousands or even millions of embedding vectors and storing them for later comparison. If your model is proprietary, the vendor is likely someday going to decide to stop offering that model. They''ll have a better model to replace it, but you still need to pay to re-calculate those millions of stored existing vectors. (In April 2024 OpenAI offered to "cover the financial cost of users re-embedding content with these new models" - https://openai.com/index/gpt-4-api-general-availability/ - but I don''t think that''s something we can rely on from every provider.) Notably, I don''t want to host the model myself. I''d much rather pay a provider for a hosted model while knowing that if they ever stop hosting it I can run the open weights version myself - or find another vendor who can do that for me. Tags: google, ai, generative-ai, embeddings, gemma'
content: extracted
html: 2026-10-06-embeddinggemma-2.html
---

I really appreciate that [EmbeddingGemma 2](https://blog.google/innovation-and-ai/technology/developers-tools/embeddinggemma-2/) is under the Apache 2.0 license.

For embedding models in particular, I don't think it makes sense to use a closed, proprietary, hosted-only model.

Most applications of embedding models involve calculating thousands or even millions of embedding vectors and storing them for later comparison.

If your model is proprietary, the vendor is likely someday going to decide to stop offering that model. They'll have a better model to replace it, but you still need to pay to re-calculate those millions of stored existing vectors.

(In April 2024 OpenAI offered to "cover the financial cost of users re-embedding content with these new models" - [https://openai.com/index/gpt-4-api-general-availability/](https://openai.com/index/gpt-4-api-general-availability/) - but I don't think that's something we can rely on from every provider.)

Notably, I *don't want to host the model myself*. I'd much rather pay a provider for a hosted model while knowing that if they ever stop hosting it I can run the open weights version myself - or find another vendor who can do that for me.
