---
title: Mixtral of experts
link: https://mistral.ai/news/mixtral-of-experts/
source: mistral-ai-news
published: 2023-12-11T07:00:00Z
updated: 2023-12-11T07:00:00Z
first_seen: 2026-09-19T21:30:23.395188495Z
content: extracted
html: 2023-12-11-mixtral-of-experts.html
preview:
  file: 2023-12-11-mixtral-of-experts.preview-7c991c49d441.webp
  width: 256
  height: 153
  color: '#e93410'
images:
- source: https://mistral.ai/cms-media/api/media/file/thumbnail-01.jpg
  original:
    file: 2023-12-11-mixtral-of-experts.image-7c935e4cd3c3.jpg
    width: 1800
    height: 1074
  color: '#e51300'
- source: https://mistral.ai/_astro/70328687-9d7a-4b98-b186-b531a4e4625e_pvtvd.webp?dpl=6aad049eaf4c2d00095b91e5
  original:
    file: 2023-12-11-mixtral-of-experts.image-caa8eebd1437.webp
    width: 1920
    height: 1236
  variants:
  - file: 2023-12-11-mixtral-of-experts.image-73bcfe7be966.webp
    width: 320
    height: 206
  - file: 2023-12-11-mixtral-of-experts.image-656cef2882c4.webp
    width: 640
    height: 412
  - file: 2023-12-11-mixtral-of-experts.image-e20415b08a3f.webp
    width: 960
    height: 618
  - file: 2023-12-11-mixtral-of-experts.image-2980d31fc660.webp
    width: 1280
    height: 824
  color: '#fefefe'
- source: https://mistral.ai/_astro/ebf2a066-f080-4e0b-9afa-e99c0a59127e_Z2kVF6E.webp?dpl=6aad049eaf4c2d00095b91e5
  original:
    file: 2023-12-11-mixtral-of-experts.image-891e3828b3ee.webp
    width: 1920
    height: 1067
  variants:
  - file: 2023-12-11-mixtral-of-experts.image-d3dcc3f270eb.webp
    width: 320
    height: 178
  - file: 2023-12-11-mixtral-of-experts.image-c71a8a436d1c.webp
    width: 640
    height: 356
  - file: 2023-12-11-mixtral-of-experts.image-3ef8ac4935d7.webp
    width: 960
    height: 534
  color: '#fdfdfd'
- source: https://mistral.ai/_astro/813bb158-9d1b-4423-9d5d-a62b9a862809_Z2699WS.webp?dpl=6aad049eaf4c2d00095b91e5
  original:
    file: 2023-12-11-mixtral-of-experts.image-fe29a9883f21.webp
    width: 1920
    height: 665
  variants:
  - file: 2023-12-11-mixtral-of-experts.image-27d517de2a7e.webp
    width: 320
    height: 111
  - file: 2023-12-11-mixtral-of-experts.image-c13d830c72e9.webp
    width: 640
    height: 222
  - file: 2023-12-11-mixtral-of-experts.image-2d830ba1a91e.webp
    width: 960
    height: 333
  - file: 2023-12-11-mixtral-of-experts.image-f071a80e0b1d.webp
    width: 1280
    height: 443
  color: '#fbfbfb'
- source: https://mistral.ai/_astro/fb50feb2-df2d-4504-aa56-1b59c2790668_Z1kpoTh.webp?dpl=6aad049eaf4c2d00095b91e5
  original:
    file: 2023-12-11-mixtral-of-experts.image-1e15fb0f82db.webp
    width: 1920
    height: 1137
  variants:
  - file: 2023-12-11-mixtral-of-experts.image-85501a1be086.webp
    width: 320
    height: 190
  - file: 2023-12-11-mixtral-of-experts.image-07027421f6c1.webp
    width: 640
    height: 379
  - file: 2023-12-11-mixtral-of-experts.image-6e92ed238656.webp
    width: 960
    height: 569
  - file: 2023-12-11-mixtral-of-experts.image-07a3e91f6b76.webp
    width: 1280
    height: 758
  color: '#fdfdfd'
- source: https://mistral.ai/_astro/a7dcd2c0-7086-4265-bd4d-b51fe117328f_Z8QwhE.webp?dpl=6aad049eaf4c2d00095b91e5
  original:
    file: 2023-12-11-mixtral-of-experts.image-e3f8eb9bf3ca.webp
    width: 1920
    height: 482
  variants:
  - file: 2023-12-11-mixtral-of-experts.image-6183d5b8e958.webp
    width: 320
    height: 80
  - file: 2023-12-11-mixtral-of-experts.image-a9cb74a16d78.webp
    width: 640
    height: 161
  - file: 2023-12-11-mixtral-of-experts.image-753f6adeb42c.webp
    width: 960
    height: 241
  - file: 2023-12-11-mixtral-of-experts.image-89346c42b602.webp
    width: 1280
    height: 321
  color: '#fdfdfd'
---

Mistral AI continues its mission to deliver the best open models to the developer community. Moving forward in AI requires taking new technological turns beyond reusing well-known architectures and training paradigms. Most importantly, it requires making the community benefit from original models to foster new inventions and usages.

Today, the team is proud to release Mixtral 8x7B, a high-quality sparse mixture of experts model (SMoE) with open weights. Licensed under Apache 2.0. Mixtral outperforms Llama 2 70B on most benchmarks with 6x faster inference. It is the strongest open-weight model with a permissive license and the best model overall regarding cost/performance trade-offs. In particular, it matches or outperforms GPT3.5 on most standard benchmarks.

Mixtral has the following capabilities.

- It gracefully handles a context of 32k tokens.

- It handles English, French, Italian, German and Spanish.

- It shows strong performance in code generation.

- It can be finetuned into an instruction-following model that achieves a score of 8.3 on MT-Bench.

#### Pushing the frontier of open models with sparse architectures

Mixtral is a sparse mixture-of-experts network. It is a decoder-only model where the feedforward block picks from a set of 8 distinct groups of parameters. At every layer, for every token, a router network chooses two of these groups (the “experts”) to process the token and combine their output additively.

This technique increases the number of parameters of a model while controlling cost and latency, as the model only uses a fraction of the total set of parameters per token. Concretely, Mixtral has 46.7B total parameters but only uses 12.9B parameters per token. It, therefore, processes input and generates output at the same speed and for the same cost as a 12.9B model.

Mixtral is pre-trained on data extracted from the open Web – we train experts and routers simultaneously.

#### Performance

We compare Mixtral to the Llama 2 family and the GPT3.5 base model. Mixtral matches or outperforms Llama 2 70B, as well as GPT3.5, on most benchmarks.

![Performance overview](https://mistral.ai/_astro/70328687-9d7a-4b98-b186-b531a4e4625e_pvtvd.webp?dpl=6aad049eaf4c2d00095b91e5)

On the following figure, we measure the quality versus inference budget tradeoff. Mistral 7B and Mixtral 8x7B belong to a family of highly efficient models compared to Llama 2 models.

![Scaling of performances](https://mistral.ai/_astro/ebf2a066-f080-4e0b-9afa-e99c0a59127e_Z2kVF6E.webp?dpl=6aad049eaf4c2d00095b91e5)

The following table give detailed results on the figure above.

![Detailed benchmarks](https://mistral.ai/_astro/813bb158-9d1b-4423-9d5d-a62b9a862809_Z2699WS.webp?dpl=6aad049eaf4c2d00095b91e5)

**Hallucination and biases.** To identify possible flaws to be corrected by fine-tuning / preference modelling, we measure the *base* model performance on BBQ/BOLD.

![BBQ BOLD benchmarks](https://mistral.ai/_astro/fb50feb2-df2d-4504-aa56-1b59c2790668_Z1kpoTh.webp?dpl=6aad049eaf4c2d00095b91e5)

Compared to Llama 2, Mixtral presents less bias on the BBQ benchmark. Overall, Mixtral displays more positive sentiments than Llama 2 on BOLD, with similar variances within each dimension.

**Language.** Mixtral 8x7B masters French, German, Spanish, Italian, and English.

![Multilingual benchmarks](https://mistral.ai/_astro/a7dcd2c0-7086-4265-bd4d-b51fe117328f_Z8QwhE.webp?dpl=6aad049eaf4c2d00095b91e5)

#### Instructed models

We release Mixtral 8x7B Instruct alongside Mixtral 8x7B. This model has been optimised through supervised fine-tuning and direct preference optimisation (DPO) for careful instruction following. On MT-Bench, it reaches a score of 8.30, making it the best open-source model, with a performance comparable to GPT3.5.

Note: Mixtral can be gracefully prompted to ban some outputs from constructing applications that require a strong level of moderation, as exemplified [here](https://docs.mistral.ai/platform/guardrailing). A proper preference tuning can also serve this purpose. Bear in mind that without such a prompt, the model will just follow whatever instructions are given.

#### Deploy Mixtral with an open-source deployment stack

To enable the community to run Mixtral with a fully open-source stack, we have submitted changes to the vLLM project, which integrates Megablocks CUDA kernels for efficient inference.

Skypilot allows the deployment of vLLM endpoints on any instance in the cloud.

#### Use Mixtral on our platform.

We're currently using Mixtral 8x7B behind our endpoint *mistral-small*, which is [available in beta](https://mistral.ai/news/la-plateforme/). [Register](https://console.mistral.ai) to get early access to all generative and embedding endpoints.

#### Acknowledgement

We thank CoreWeave and Scaleway teams for technical support as we trained our models.
