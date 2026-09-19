---
title: AI in abundance
link: https://mistral.ai/news/september-24-release/
source: mistral-ai-news
published: 2024-09-17T03:00:00Z
updated: 2024-09-17T03:00:00Z
first_seen: 2026-09-19T21:30:23.395188495Z
content: extracted
html: 2024-09-17-ai-in-abundance.html
preview:
  file: 2024-09-17-ai-in-abundance.preview-a3d888371629.webp
  width: 256
  height: 153
  color: '#272638'
images:
- source: https://mistral.ai/cms-media/api/media/file/thumbnail-06.jpg
  original:
    file: 2024-09-17-ai-in-abundance.image-26b14a52cdae.jpg
    width: 1800
    height: 1074
  color: '#242334'
- source: https://mistral.ai/_astro/6e8ac5ab-e20c-4249-9584-fdcb6ffbfe62_ZVR1Eh.webp?dpl=6aad049eaf4c2d00095b91e5
  original:
    file: 2024-09-17-ai-in-abundance.image-cf1a805563d9.webp
    width: 1920
    height: 2592
  variants:
  - file: 2024-09-17-ai-in-abundance.image-7234ffceeb10.webp
    width: 320
    height: 432
  - file: 2024-09-17-ai-in-abundance.image-f688f6c218d8.webp
    width: 640
    height: 864
  - file: 2024-09-17-ai-in-abundance.image-d5c536101f35.webp
    width: 960
    height: 1296
  - file: 2024-09-17-ai-in-abundance.image-29b1be3c1833.webp
    width: 1280
    height: 1728
  color: '#fdfdfd'
- source: https://mistral.ai/_astro/b9f3b2b1-db41-496a-81f3-62490d7fe498_205spi.webp?dpl=6aad049eaf4c2d00095b91e5
  original:
    file: 2024-09-17-ai-in-abundance.image-74c034fa6021.webp
    width: 1920
    height: 2592
  variants:
  - file: 2024-09-17-ai-in-abundance.image-84531f744e45.webp
    width: 320
    height: 432
  - file: 2024-09-17-ai-in-abundance.image-c88a4619655c.webp
    width: 640
    height: 864
  - file: 2024-09-17-ai-in-abundance.image-40230dbfc3e3.webp
    width: 960
    height: 1296
  - file: 2024-09-17-ai-in-abundance.image-4a3d1e3f49c0.webp
    width: 1280
    height: 1728
  color: '#fdfdfd'
---

We’re taking new steps in our mission to bring frontier AI in the hands of everyone. Today, we are releasing:

- A free tier on la Plateforme

- A pricing update over our entire family of models

- A new, better Mistral Small

- Free vision capabilities on le Chat with Pixtral 12B

### Free tier on la Plateforme

[La Plateforme](https://console.mistral.ai/), the serverless platform to tune and build with Mistral models as API endpoints, now offers a free tier enabling developers to get started with experimentation, evaluation, and prototyping at no cost. Users can seamlessly evolve their endpoints into a commercial tier, and benefit from full data isolation (with a free zero-retention option) and higher rate limits. Users can also choose to deploy our models to different infrastructure: whether using our cloud partners (Azure / AWS / GCP), or choosing to deploy our solutions on their own [tenant](https://mistral.ai/contact/).

### Reduced prices across the board

We’ve worked hard on making our endpoints faster and more efficient. This enables us to reduce prices across the board, with the following prices

| Model         | New price               | Old price            | Price drop |
| ------------- | ----------------------- | -------------------- | ---------- |
| Mistral Nemo  | $0.15 / M input tokens  | $0.3 / M tokens      | 50%        |
|               | $0.15 / M output tokens | $0.3 / M tokens      |            |
| Pixtral 12B   | $0.15 / M input tokens  |                      |            |
|               | $0.15 / M output tokens |                      |            |
| Mistral Small | $0.2 / M input tokens   | $1 / M input tokens  | 80%        |
|               | $0.6 / M output tokens  | $3 / M output tokens |            |
| Codestral     | $0.2 / M input tokens   | $1 / M input tokens  | 80%        |
|               | $0.6 / M output tokens  | $3 / M output tokens |            |
| Mistral Large | $2 / M input tokens     | $3 / M input tokens  | 33%        |
|               | $6 / M output tokens    | $9 / M output tokens |            |

This price update makes Mistral Large 2 the most cost-efficient frontier model, make our smaller models extremely cost efficient, and allows customers to realize significantly faster returns on their AI investments. Updated pricing will also reflect on our cloud platform partner offerings (Azure AI Studio, Amazon Bedrock, Google Vertex AI).

### Small gets a big update

We are proud to unveil Mistral Small v24.09, our latest enterprise-grade small model, an upgrade of Mistral Small v24.02. Available under the Mistral Research License, this model offers customers the flexibility to choose a cost-efficient, fast, yet reliable option for use cases such as translation, summarization, sentiment analysis, and other tasks that do not require full-blown general purpose models.

With 22 billion parameters, Mistral Small v24.09 offers customers a convenient mid-point between Mistral NeMo 12B and Mistral Large 2, providing a cost-effective solution that can be deployed across various platforms and environments. As shown below, the new small model delivers significant improvements in human alignment, reasoning capabilities, and code over the previous model.

![Detailed benchmarks](https://mistral.ai/_astro/6e8ac5ab-e20c-4249-9584-fdcb6ffbfe62_ZVR1Eh.webp?dpl=6aad049eaf4c2d00095b91e5)

![Detailed benchmarks](https://mistral.ai/_astro/b9f3b2b1-db41-496a-81f3-62490d7fe498_205spi.webp?dpl=6aad049eaf4c2d00095b91e5)

We’re releasing Mistral Small v24.09 under the MRL license. You may self-deploy it for non-commercial purposes, using e.g. [vLLM](https://docs.mistral.ai/deployment/self-deployment/vllm/)

### Eye of the Tiger - Pixtral on le Chat

Following our latest Apache model release, [Pixtral 12B](https://mistral.ai/news/pixtral-12b/), a vision-capable model with image understanding capabilities, is now freely available on [le Chat](https://chat.mistral.ai/). Pixtral 12B is the first open source model to support images of any size without degradation in text-based performance, and you can now use it on le Chat to scan, analyze, search, caption, and better understand your personal or enterprise knowledge files.

Importantly, the model is available under the Apache 2.0 license, so you can bring visual understanding capabilities to your own environment without having to upload your files to a third-party provider. This is a critical capability for customers that operate with sensitive or proprietary information.

### Do more with less

All the above announcements are now available. Head over to [le Chat](https://chat.mistral.ai/) to try the new image understanding capabilities. To try the free tier of la Plateforme, sign in at [console.mistral.ai](https://console.mistral.ai). To learn more about Mistral Small v24.09, Pixtral 12B, and other Mistral models and pricing, click [here](https://mistral.ai/technology/).
