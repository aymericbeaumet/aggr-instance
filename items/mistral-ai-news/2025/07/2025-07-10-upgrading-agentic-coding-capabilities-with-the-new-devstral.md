---
title: Upgrading agentic coding capabilities with the new Devstral models
link: https://mistral.ai/news/devstral-2507/
source: mistral-ai-news
published: 2025-07-10T16:00:00Z
updated: 2025-07-10T16:00:00Z
first_seen: 2026-09-19T21:30:23.395188495Z
content: extracted
html: 2025-07-10-upgrading-agentic-coding-capabilities-with-the-new-devstral.html
preview:
  file: 2025-07-10-upgrading-agentic-coding-capabilities-with-the-new-devstral.preview-8be9edef61c6.webp
  width: 256
  height: 153
  color: '#5694c1'
images:
- source: https://mistral.ai/cms-media/api/media/file/Thumbnail-Model-Devstral.jpg
  original:
    file: 2025-07-10-upgrading-agentic-coding-capabilities-with-the-new-devstral.image-ca4cb3ff4d1c.jpg
    width: 1800
    height: 1074
  color: '#0287e8'
- source: https://mistral.ai/_astro/a8227ebf-fba7-4ad7-9d6b-83ce5da42a3e_Z1yAyas.webp?dpl=6aad049eaf4c2d00095b91e5
  original:
    file: 2025-07-10-upgrading-agentic-coding-capabilities-with-the-new-devstral.image-e1f23cb1ad22.webp
    width: 1920
    height: 1080
  variants:
  - file: 2025-07-10-upgrading-agentic-coding-capabilities-with-the-new-devstral.image-ca76356e9220.webp
    width: 320
    height: 180
  - file: 2025-07-10-upgrading-agentic-coding-capabilities-with-the-new-devstral.image-673bbe15fbb4.webp
    width: 640
    height: 360
  - file: 2025-07-10-upgrading-agentic-coding-capabilities-with-the-new-devstral.image-be97d7af9de6.webp
    width: 960
    height: 540
  - file: 2025-07-10-upgrading-agentic-coding-capabilities-with-the-new-devstral.image-e5eed366e223.webp
    width: 1280
    height: 720
  color: '#fefefe'
- source: https://mistral.ai/_astro/ae27ff96-c7b6-4a6b-a3a1-0e02e156198a_Yoxxh.webp?dpl=6aad049eaf4c2d00095b91e5
  original:
    file: 2025-07-10-upgrading-agentic-coding-capabilities-with-the-new-devstral.image-3b6eb51c03c0.webp
    width: 1920
    height: 935
  variants:
  - file: 2025-07-10-upgrading-agentic-coding-capabilities-with-the-new-devstral.image-b655a0e6d373.webp
    width: 320
    height: 156
  - file: 2025-07-10-upgrading-agentic-coding-capabilities-with-the-new-devstral.image-281844966d47.webp
    width: 640
    height: 312
  - file: 2025-07-10-upgrading-agentic-coding-capabilities-with-the-new-devstral.image-39c73a8c71fa.webp
    width: 960
    height: 468
  - file: 2025-07-10-upgrading-agentic-coding-capabilities-with-the-new-devstral.image-923048eee407.webp
    width: 1280
    height: 623
  color: '#fdfdfd'
---

Today, we introduce Devstral Medium, as well as an upgrade to Devstral Small. These models are released under the collaboration between Mistral AI and [All Hands AI](https://www.all-hands.dev/) 🙌, with a strong emphasis on generalization to different prompts and agentic scaffolds.

The new Devstral Small 1.1 is released under the Apache 2.0 license, and is state-of-the-art amongst open models for code agents. Devstral Medium is available through our API, and sets a new point on the cost/performance pareto frontier, surpassing Gemini 2.5 Pro and GPT 4.1 for a quarter of the price.

## Devstral Small 1.1

As with the previous version of Devstral Small, we release Devstral Small 1.1 under the Apache 2.0 license. While the architecture remains the same, with only 24B parameters, Devstral Small 1.1 comes with significant improvements over its predecessor:

### Enhanced Performance

Devstral Small 1.1 achieves a score of 53.6% on SWE-Bench Verified, and sets a new state-of-the-art for open models without test-time scaling.

### Versatility and Generalization

Devstral Small 1.1 excels when paired with OpenHands, and also demonstrates better generalization to different prompts and coding environments. Its versatility is further enhanced by supporting both Mistral function calling and XML formats, making it adaptable to a wide range of applications and agentic scaffolds.

![Devstral Oss.001 (1)](https://mistral.ai/_astro/a8227ebf-fba7-4ad7-9d6b-83ce5da42a3e_Z1yAyas.webp?dpl=6aad049eaf4c2d00095b91e5)

## Devstral Medium

Devstral Medium builds upon the strengths of Devstral Small and takes performance to the next level with a score of 61.6% on SWE-Bench Verified. Devstral Medium is available through our public API, and offers exceptional performance at a competitive price point, making it an ideal choice for businesses and developers looking for a high-quality, cost-effective model.

For those who prefer on-premise solutions, Devstral Medium can be directly deployed on private infrastructure, offering enhanced data privacy and control. We also support custom finetuning for Devstral Medium, allowing enterprises to customize the model for specific use cases, and achieve optimal performance tailored to their specific requirements.

![Devstral Main Graph](https://mistral.ai/_astro/ae27ff96-c7b6-4a6b-a3a1-0e02e156198a_Yoxxh.webp?dpl=6aad049eaf4c2d00095b91e5)

## Availability

Both models are available through our API under the the following names:

- devstral-small-2507 at the same price as Mistral Small 3.1: $0.1/M input tokens and $0.3/M output tokens.

- devstral-medium-2507 at the same price as Mistral Medium 3: $0.4/M input tokens and $2/M output tokens.

We release Devstral Small 1.1 under the Apache 2.0 license for the community to build on, customize, and accelerate autonomous software development. To try it for yourself, head over to our [model card](https://huggingface.co/mistralai/Devstral-Small-2507).

Devstral Medium will also be available on [Mistral Code](https://mistral.ai/news/mistral-code) for enterprise customers and on our [finetuning API](https://docs.mistral.ai/guides/finetuning/). To deploy and customize the model in your environment, please [contact us](https://mistral.ai/contact).

We are dedicated to open-sourcing our most accessible and impactful models, ensuring the open-source community can easily utilize and benefit from our advanced technology. While Devstral Small is easily usable for local deployment and available under the Apache 2.0 license for everyone to use and build upon, Devstral Medium is available on our API and offers high performance for developers and enterprises.
