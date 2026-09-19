---
title: Mistral Small 3.1
link: https://mistral.ai/news/mistral-small-3-1/
source: mistral-ai-news
published: 2025-03-17T12:00:00Z
updated: 2025-03-17T12:00:00Z
first_seen: 2026-09-19T21:30:23.395188495Z
content: extracted
html: 2025-03-17-mistral-small-3-1.html
preview:
  file: 2025-03-17-mistral-small-3-1.preview-97f993d1d33e.webp
  width: 256
  height: 153
  color: '#8cb1cf'
images:
- source: https://mistral.ai/cms-media/api/media/file/Thumbnail-Model-Small.jpg
  original:
    file: 2025-03-17-mistral-small-3-1.image-05115e0ca6de.jpg
    width: 1800
    height: 1074
  color: '#58b6fa'
- source: https://mistral.ai/_astro/1977a3f1-eae3-4eed-b2c7-e1701c4692ed_yuvl0.webp?dpl=6aad049eaf4c2d00095b91e5
  original:
    file: 2025-03-17-mistral-small-3-1.image-ffdb900a07a2.webp
    width: 1920
    height: 1014
  variants:
  - file: 2025-03-17-mistral-small-3-1.image-5acd11de857d.webp
    width: 320
    height: 169
  - file: 2025-03-17-mistral-small-3-1.image-70f6a169e887.webp
    width: 640
    height: 338
  - file: 2025-03-17-mistral-small-3-1.image-a6a7b8b5cc14.webp
    width: 960
    height: 507
  color: '#fdfdfd'
---

Today we announce Mistral Small 3.1: the best model in its weight class.

Building on [Mistral Small 3](https://mistral.ai/news/mistral-small-3), this new model comes with improved text performance, multimodal understanding, and an expanded context window of up to 128k tokens. The model outperforms comparable models like Gemma 3 and GPT-4o Mini, while delivering inference speeds of 150 tokens per second.

Mistral Small 3.1 is released under an Apache 2.0 license.

![Perf Gpqa Diamond Mistral](https://mistral.ai/_astro/1977a3f1-eae3-4eed-b2c7-e1701c4692ed_yuvl0.webp?dpl=6aad049eaf4c2d00095b91e5)

Modern AI applications demand a blend of capabilities—handling text, understanding multimodal inputs, supporting multiple languages, and managing long contexts—with low latency and cost efficiency. As shown below, Mistral Small 3.1 is the first open source model that not only meets, but in fact surpasses, the performance of leading small proprietary models across all these dimensions.

Below you will find more details on model performance. Whenever possible, we show numbers reported previously by other providers, otherwise we evaluate models through our common evaluation harness.

### Instruct Performance

#### Text instruct benchmarks

#### Multimodal Instruct Benchmarks

*MM-MT-Bench scaled to between 0 and 100.*

#### Multilingual

#### Long Context

### Pretrained Performance

We also release the pretrained base model for Mistral Small 3.1.

#### All pretrain

## Use cases

Mistral Small 3.1 is a versatile model designed to handle a wide range of generative AI tasks, including instruction following, conversational assistance, image understanding, and function calling. It provides a solid foundation for both enterprise and consumer-grade AI applications.

## Key Features and Capabilities

- Lightweight: Mistral Small 3.1 can run on a single RTX 4090 or a Mac with 32GB RAM. This makes it a great fit for on-device use cases.

- Fast-response conversational assistance: Ideal for virtual assistants and other applications where quick, accurate responses are essential.

- Low-latency function calling: Capable of rapid function execution within automated or agentic workflows

- Fine-tuning for specialized domains: Mistral Small 3.1 can be fine-tuned to specialize in specific domains, creating accurate subject matter experts. This is particularly useful in fields like legal advice, medical diagnostics, and technical support.

- Foundation for advanced reasoning: We continue to be impressed by how the community builds on top of open Mistral models. Just in the last few weeks, we have seen several excellent reasoning models built on Mistral Small 3, such as the [DeepHermes 24B](https://huggingface.co/NousResearch/DeepHermes-3-Mistral-24B-Preview) by Nous Research. To that end, we are releasing both base and instruct checkpoints for Mistral Small 3.1 to enable further downstream customization of the model.

Mistral Small 3.1 can be used across various enterprise and consumer applications that require multimodal understanding, such as document verification, diagnostics, on-device image processing, visual inspection for quality checks, object detection in security systems, image-based customer support, and general purpose assistance.

## Availability

Mistral Small 3.1 is available to download on the huggingface website [Mistral Small 3.1 Base](https://huggingface.co/mistralai/Mistral-Small-3.1-24B-Base-2503) and [Mistral Small 3.1 Instruct](https://huggingface.co/mistralai/Mistral-Small-3.1-24B-Instruct-2503). For enterprise deployments with private and optimized inference infrastructure, please [contact us](https://mistral.ai/contact).

You can also try the model via API on Mistral AI’s developer playground [La Plateforme](https://mistral.ai/news/la-plateforme) starting today. The model is also available on [Google Cloud Vertex AI](https://cloud.google.com/vertex-ai/generative-ai/docs/partner-models/mistral). Mistral Small 3.1 will be available on [NVIDIA NIM](https://developer.nvidia.com/nim) and [Microsoft Azure AI Foundry](https://ai.azure.com/explore/models?&selectedCollection=mistral) in the coming weeks.

Happy building!
