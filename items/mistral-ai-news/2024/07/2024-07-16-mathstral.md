---
title: MathΣtral
link: https://mistral.ai/news/mathstral/
source: mistral-ai-news
published: 2024-07-16T08:00:00Z
updated: 2024-07-16T08:00:00Z
first_seen: 2026-09-19T21:30:23.395188495Z
content: extracted
html: 2024-07-16-mathstral.html
preview:
  file: 2024-07-16-mathstral.preview-9ed34db45828.webp
  width: 256
  height: 153
  color: '#7e8d7e'
images:
- source: https://mistral.ai/cms-media/api/media/file/Thumbnail-Model-Mathstral.jpg
  original:
    file: 2024-07-16-mathstral.image-d6a08a501be6.jpg
    width: 1800
    height: 1074
  color: '#0288e8'
- source: https://mistral.ai/_astro/996b99d1-b61b-46ce-b1eb-4ff4e981aeb4_29uhG3.webp?dpl=6aad049eaf4c2d00095b91e5
  original:
    file: 2024-07-16-mathstral.image-28ef9186e444.webp
    width: 1920
    height: 1014
  variants:
  - file: 2024-07-16-mathstral.image-ff1aaf503f8b.webp
    width: 320
    height: 169
  - file: 2024-07-16-mathstral.image-a14b5e25f8c2.webp
    width: 640
    height: 338
  - file: 2024-07-16-mathstral.image-11c1a4fb84ca.webp
    width: 960
    height: 507
  - file: 2024-07-16-mathstral.image-f7d4c9caf9e2.webp
    width: 1280
    height: 676
  color: '#fdfdfd'
- source: https://mistral.ai/_astro/d2d77e13-903b-4b86-a0fb-771b0c7c9b15_Zv72La.webp?dpl=6aad049eaf4c2d00095b91e5
  original:
    file: 2024-07-16-mathstral.image-2897add1940a.webp
    width: 1920
    height: 594
  variants:
  - file: 2024-07-16-mathstral.image-715ef5886863.webp
    width: 320
    height: 99
  - file: 2024-07-16-mathstral.image-12e5188144c1.webp
    width: 640
    height: 198
  - file: 2024-07-16-mathstral.image-a8b66dc83372.webp
    width: 960
    height: 297
  color: '#fafafa'
---

Research

July 16, 2024

By Mistral AI team

We're contributing Mathstral to the science community to bolster efforts in advanced mathematical problems requiring complex, multi-step logical reasoning. The Mathstral release is part of our broader effort to support academic projects—it was produced in the context of our collaboration with [Project Numina](https://projectnumina.ai/).

Akin to Isaac Newton in his time, Mathstral stands on the shoulders of Mistral 7B and specializes in STEM subjects. It achieves state-of-the-art reasoning capacities in its size category across various industry-standard benchmarks. In particular, it achieves 56.6% on MATH and 63.47% on MMLU, with the following MMLU performance difference by subject between Mathstral 7B and Mistral 7B.

![Mathstral 7B breakdown by subject](https://mistral.ai/_astro/996b99d1-b61b-46ce-b1eb-4ff4e981aeb4_29uhG3.webp?dpl=6aad049eaf4c2d00095b91e5)

Mathstral is another example of the excellent performance/speed tradeoffs achieved when building models for specific purposes – a development philosophy we actively promote in la Plateforme, particularly with its new [fine-tuning capabilities](https://docs.mistral.ai/capabilities/finetuning/).

![Mathstral 7B detailed benchmarks](https://mistral.ai/_astro/d2d77e13-903b-4b86-a0fb-771b0c7c9b15_Zv72La.webp?dpl=6aad049eaf4c2d00095b91e5)

Mathstral can achieve significantly better results with more inference-time computation: Mathstral 7B scores **68.37%** on MATH with majority voting and **74.59%** with a strong reward model among 64 candidates.

Mathstral is an instructed model – use it or fine-tune it as such, referring to our documentation. Weights are hosted on [HuggingFace](https://huggingface.co/mistralai/mathstral-7B-v0.1). You can try Mathstral now with [mistral-inference](https://github.com/mistralai/mistral-inference/releases/tag/v1.2.0) and adapt it with [mistral-finetune](https://github.com/mistralai/mistral-finetune).

We thank Professor [Paul Bourdon](https://uva.theopenscholar.com/paul-bourdon/) for curating the GRE Math Subject Test problems used in our evaluation.

0%
