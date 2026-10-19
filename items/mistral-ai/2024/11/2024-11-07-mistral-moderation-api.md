---
title: Mistral Moderation API
link: https://mistral.ai/news/mistral-moderation/
source: mistral-ai
published: 2024-11-07T10:00:00Z
updated: 2024-11-07T10:00:00Z
first_seen: 2026-10-19T01:26:53.998049939Z
content: extracted
html: 2024-11-07-mistral-moderation-api.html
remote_preview:
  url: https://mistral.ai/cms-media/api/media/file/thumbnail-12.jpg
---

Research

November 7, 2024

By Mistral team

Safety plays a key role in making AI useful. At Mistral AI, we believe that system level guardrails are critical to protecting downstream deployments.That's why we are releasing a new content moderation API. It is the same API that powers the moderation service in Le Chat. We are launching it to empower our users to utilize and tailor this tool to their specific applications and safety standards.

Over the past few months, we've seen growing enthusiasm across the industry and research community for new LLM based moderation systems, which can help make moderation more scalable and robust across applications. Our model is an LLM classifier trained to classify text inputs into 9 categories defined below. We are releasing two end-points: one for raw text and one for conversational content. Undesirable content is very specific to a given context, therefore we've trained our model to classify the last message of conversation within a conversational context. Check out our technical documentation for more information. The model is natively multilingual and in particular trained on Arabic, Chinese, English, French, German, Italian, Japanese, Korean, Portuguese, Russian, Spanish.

![Detailed benchmarks](https://mistral.ai/_astro/c25144ad-9d0b-4cbb-a577-ee03fe7b2647_1PFSzu.webp?dpl=6ad1d7b6946342e26a4d2b4f)

The Content Moderation classifier leverages the most relevant policy categories for effective guardrails and introduces a pragmatic approach to LLM safety by addressing model-generated harms such as unqualified advice and PII. The full set of policy definitions and details on how to get started are available in our [technical documentation](https://docs.mistral.ai/capabilities/guardrailing/).

#### Performance

We are sharing AUC PR across policies on our internal testset below.

![Detailed benchmarks](https://mistral.ai/_astro/eb1e3716-6a40-43e1-90d6-732913f5e30f_ZpbTGC.webp?dpl=6ad1d7b6946342e26a4d2b4f)

We're working with our customers to build and share scalable, lightweight and customizable moderation tooling, and will continue to engage with the research community to contribute safety advancements to the broader field.

0%
