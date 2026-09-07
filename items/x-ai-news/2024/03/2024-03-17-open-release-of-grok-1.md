---
title: Open Release of Grok-1
link: https://x.ai/news/grok-os
source: x-ai-news
published: 2024-03-17T00:00:00Z
first_seen: 2026-09-07T17:03:44.343711062Z
summary: We are releasing the weights and architecture of our 314 billion parameter Mixture-of-Experts model Grok-1.
content: extracted
html: 2024-03-17-open-release-of-grok-1.html
preview:
  file: 2024-03-17-open-release-of-grok-1.preview-eb3fbb11cf56.webp
  width: 256
  height: 144
  color: '#7c6c69'
images:
- source: https://media.x.ai/v1/website/default-06ae9dd9.webp
  original:
    file: 2024-03-17-open-release-of-grok-1.image-0649b4038aaa.webp
    width: 1486
    height: 836
  variants:
  - file: 2024-03-17-open-release-of-grok-1.image-120229e29daa.webp
    width: 48
    height: 27
  - file: 2024-03-17-open-release-of-grok-1.image-4ade8c21565d.webp
    width: 320
    height: 180
  - file: 2024-03-17-open-release-of-grok-1.image-2c7104b40501.webp
    width: 640
    height: 360
  color: '#030203'
---

**We are releasing the base model weights and network architecture of [Grok-1](https://x.ai/blog/grok), our large language model. Grok-1 is a 314 billion parameter Mixture-of-Experts model trained from scratch by xAI.**

This is the raw base model checkpoint from the Grok-1 pre-training phase, which concluded in October 2023. This means that the model is not fine-tuned for any specific application, such as dialogue.

We are releasing the weights and the architecture under the Apache 2.0 license.

To get started with using the model, follow the instructions at [github.com/xai-org/grok](https://github.com/xai-org/grok).

## [Model Details](https://x.ai/news/grok-os#model-details)

- Base model trained on a large amount of text data, not fine-tuned for any particular task.
- **314B** parameter Mixture-of-Experts model with 25% of the weights active on a given token.
- Trained from scratch by xAI using a custom training stack on top of JAX and Rust in October 2023.
