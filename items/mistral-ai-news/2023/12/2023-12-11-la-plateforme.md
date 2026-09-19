---
title: La Plateforme
link: https://mistral.ai/news/la-plateforme/
source: mistral-ai-news
published: 2023-12-11T07:00:00Z
updated: 2023-12-11T07:00:00Z
first_seen: 2026-09-19T21:30:23.395188495Z
content: extracted
html: 2023-12-11-la-plateforme.html
preview:
  file: 2023-12-11-la-plateforme.preview-9bd6fd8a55c8.webp
  width: 256
  height: 153
  color: '#fc880c'
images:
- source: https://mistral.ai/cms-media/api/media/file/thumbnail-10.jpg
  original:
    file: 2023-12-11-la-plateforme.image-538f58e708ea.jpg
    width: 1800
    height: 1074
  color: '#fd7a03'
- source: https://mistral.ai/_astro/f745e94a-8f9b-414b-a23a-ed2aa7f359a3_Z2rdT9v.webp?dpl=6aad049eaf4c2d00095b91e5
  original:
    file: 2023-12-11-la-plateforme.image-a33199e27bce.webp
    width: 1920
    height: 1443
  variants:
  - file: 2023-12-11-la-plateforme.image-2fef2d754394.webp
    width: 320
    height: 241
  - file: 2023-12-11-la-plateforme.image-e2450ca7d498.webp
    width: 640
    height: 481
  - file: 2023-12-11-la-plateforme.image-22c6e2b1883f.webp
    width: 960
    height: 722
  - file: 2023-12-11-la-plateforme.image-197d47fc4891.webp
    width: 1280
    height: 962
  color: '#fdfdfd'
---

Mistral AI brings the strongest open generative models to the developers, along with efficient ways to deploy and customise them for production.

We're opening a beta access to our [first platform services](https://console.mistral.ai) today. We start simple: la plateforme serves three chat endpoints for generating text following textual instructions and an embedding endpoint. Each endpoint has a different performance/price tradeoff.

#### Generative endpoints

The two first endpoints, mistral-tiny and mistral-small, currently use our two released open models; the third, mistral-medium, uses a prototype model with higher performances that we are testing in a deployed setting.

We serve instructed versions of our models. We have worked on consolidating the most effective alignment techniques (efficient fine-tuning, direct preference optimisation) to create easy-to-control and pleasant-to-use models. We pre-train models on data extracted from the open Web and perform instruction fine-tuning from annotations.

**Mistral-tiny**. Our most cost-effective endpoint currently serves Mistral 7B Instruct v0.2, a new minor release of Mistral 7B Instruct. Mistral-tiny only works in English. It obtains 7.6 on MT-Bench. The instructed model can be downloaded [here](https://huggingface.co/mistralai/Mistral-7B-Instruct-v0.2).

**Mistral-small**. This endpoint currently serves our newest model, Mixtral 8x7B, described in more detail in our [blog post](https://mistral.ai/news/mixtral-of-experts/). It masters English/French/Italian/German/Spanish and code and obtains 8.3 on MT-Bench.

**Mistral-medium**. Our highest-quality endpoint currently serves a prototype model, that is currently among the top serviced models available based on standard benchmarks. It masters English/French/Italian/German/Spanish and code and obtains a score of 8.6 on MT-Bench. The following table compare the performance of the base models of Mistral-medium, Mistral-small and the endpoint of a competitor.

![mistral-medium](https://mistral.ai/_astro/f745e94a-8f9b-414b-a23a-ed2aa7f359a3_Z2rdT9v.webp?dpl=6aad049eaf4c2d00095b91e5)

#### Embedding endpoint

Mistral-embed, our embedding endpoint, serves an embedding model with a 1024 embedding dimension. Our embedding model has been designed with retrieval capabilities in mind. It achieves a retrieval score of **55.26** on MTEB.

#### API specifications

Our API follows the specifications of the popular chat interface initially proposed by our dearest competitor. We provide a Python and Javascript client library to query our endpoints. Our endpoints allow users to provide a system prompt to set a higher level of moderation on model outputs for applications where this is an important requirement.

#### Ramping up from beta access to general availability

Anyone can [register](https://console.mistral.ai) to use our API as of today as we progressively ramp up our capacity. Our business team can help qualify your needs and accelerate access. Expect rough edges as we stabilise our platform towards fully self-served availability.

#### Acknowledgement

We are grateful to NVIDIA for supporting us in integrating TensorRT-LLM and Triton and working alongside us to make a sparse mixture of experts compatible with TRT-LLM.
