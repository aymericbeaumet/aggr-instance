---
title: Mistral Moderation API
link: https://mistral.ai/news/mistral-moderation/
source: mistral-ai-news
published: 2024-11-07T10:00:00Z
updated: 2024-11-07T10:00:00Z
first_seen: 2026-09-19T21:30:23.395188495Z
content: extracted
html: 2024-11-07-mistral-moderation-api.html
preview:
  file: 2024-11-07-mistral-moderation-api.preview-de3a19728e3c.webp
  width: 256
  height: 153
  color: '#e9380a'
images:
- source: https://mistral.ai/cms-media/api/media/file/thumbnail-12.jpg
  original:
    file: 2024-11-07-mistral-moderation-api.image-e08145e7b6fe.jpg
    width: 1800
    height: 1074
  color: '#e20a01'
- source: https://mistral.ai/_astro/c25144ad-9d0b-4cbb-a577-ee03fe7b2647_1PFSzu.webp?dpl=6aad049eaf4c2d00095b91e5
  original:
    file: 2024-11-07-mistral-moderation-api.image-02fdb07c591f.webp
    width: 1920
    height: 1234
  variants:
  - file: 2024-11-07-mistral-moderation-api.image-64b7dd176660.webp
    width: 320
    height: 206
  - file: 2024-11-07-mistral-moderation-api.image-1ff31d25379d.webp
    width: 640
    height: 411
  - file: 2024-11-07-mistral-moderation-api.image-0661821cde9d.webp
    width: 960
    height: 617
  color: '#e8e8e9'
- source: https://mistral.ai/_astro/eb1e3716-6a40-43e1-90d6-732913f5e30f_ZpbTGC.webp?dpl=6aad049eaf4c2d00095b91e5
  original:
    file: 2024-11-07-mistral-moderation-api.image-e1470f92581a.webp
    width: 1920
    height: 1634
  variants:
  - file: 2024-11-07-mistral-moderation-api.image-cb9b154c2b66.webp
    width: 320
    height: 272
  - file: 2024-11-07-mistral-moderation-api.image-b704d99ac90c.webp
    width: 640
    height: 545
  - file: 2024-11-07-mistral-moderation-api.image-edcfc72cffd6.webp
    width: 960
    height: 817
  - file: 2024-11-07-mistral-moderation-api.image-dc0fbffdf7d2.webp
    width: 1280
    height: 1089
  color: '#f77315'
---

Research

November 7, 2024

By Mistral AI team

Safety plays a key role in making AI useful. At Mistral AI, we believe that system level guardrails are critical to protecting downstream deployments.That's why we are releasing a new content moderation API. It is the same API that powers the moderation service in Le Chat. We are launching it to empower our users to utilize and tailor this tool to their specific applications and safety standards.

Over the past few months, we've seen growing enthusiasm across the industry and research community for new LLM based moderation systems, which can help make moderation more scalable and robust across applications. Our model is an LLM classifier trained to classify text inputs into 9 categories defined below. We are releasing two end-points: one for raw text and one for conversational content. Undesirable content is very specific to a given context, therefore we've trained our model to classify the last message of conversation within a conversational context. Check out our technical documentation for more information. The model is natively multilingual and in particular trained on Arabic, Chinese, English, French, German, Italian, Japanese, Korean, Portuguese, Russian, Spanish.

![Detailed benchmarks](https://mistral.ai/_astro/c25144ad-9d0b-4cbb-a577-ee03fe7b2647_1PFSzu.webp?dpl=6aad049eaf4c2d00095b91e5)

The Content Moderation classifier leverages the most relevant policy categories for effective guardrails and introduces a pragmatic approach to LLM safety by addressing model-generated harms such as unqualified advice and PII. The full set of policy definitions and details on how to get started are available in our [technical documentation](https://docs.mistral.ai/capabilities/guardrailing/).

#### Performance

We are sharing AUC PR across policies on our internal testset below.

![Detailed benchmarks](https://mistral.ai/_astro/eb1e3716-6a40-43e1-90d6-732913f5e30f_ZpbTGC.webp?dpl=6aad049eaf4c2d00095b91e5)

We're working with our customers to build and share scalable, lightweight and customizable moderation tooling, and will continue to engage with the research community to contribute safety advancements to the broader field.

0%
