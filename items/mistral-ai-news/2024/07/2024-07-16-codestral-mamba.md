---
title: Codestral Mamba
link: https://mistral.ai/news/codestral-mamba/
source: mistral-ai-news
published: 2024-07-16T08:00:00Z
updated: 2024-07-16T08:00:00Z
first_seen: 2026-09-19T21:30:23.395188495Z
content: extracted
html: 2024-07-16-codestral-mamba.html
preview:
  file: 2024-07-16-codestral-mamba.preview-267f7f427e06.webp
  width: 256
  height: 153
  color: '#94bc92'
images:
- source: https://mistral.ai/cms-media/api/media/file/Thumbnail-Model-Codestral%20Mamba.jpg
  original:
    file: 2024-07-16-codestral-mamba.image-260f9af20532.jpg
    width: 1800
    height: 1074
  color: '#c8e4fd'
- source: https://mistral.ai/_astro/fdd5d6fa-0e15-47b6-a40e-fb81a9f65514_Z25sWTJ.webp?dpl=6aad049eaf4c2d00095b91e5
  original:
    file: 2024-07-16-codestral-mamba.image-3604ccad65dc.webp
    width: 1920
    height: 627
  variants:
  - file: 2024-07-16-codestral-mamba.image-44f5c68224e6.webp
    width: 320
    height: 105
  - file: 2024-07-16-codestral-mamba.image-03880dcac8f6.webp
    width: 640
    height: 209
  - file: 2024-07-16-codestral-mamba.image-e91aa2018eb4.webp
    width: 960
    height: 314
  color: '#fdfcfb'
---

Research

July 16, 2024

By Mistral AI team

Following the publishing of the Mixtral family, Codestral Mamba is another step in our effort to study and provide new architectures. It is available for free use, modification, and distribution, and we hope it will open new perspectives in architecture research. Codestral Mamba was designed with help from Albert Gu and Tri Dao.

Unlike Transformer models, [Mamba models](https://arxiv.org/abs/2312.00752) offer the advantage of linear time inference and the theoretical ability to model sequences of infinite length. It allows users to engage with the model extensively with quick responses, irrespective of the input length. This efficiency is especially relevant for code productivity use cases—this is why we trained this model with advanced code and reasoning capabilities, enabling it to perform on par with SOTA transformer-based models.

![Detailed Codestral Mamba benchmarks](https://mistral.ai/_astro/fdd5d6fa-0e15-47b6-a40e-fb81a9f65514_Z25sWTJ.webp?dpl=6aad049eaf4c2d00095b91e5)

We have tested Codestral Mamba on in-context retrieval capabilities up to 256k tokens. We expect it to be a great local code assistant!

You can deploy Codestral Mamba using the [mistral-inference](https://github.com/mistralai/mistral-inference/releases/tag/v1.2.0) SDK, which relies on the reference implementations from Mamba's GitHub repository. The model can also be deployed through [TensorRT-LLM](https://github.com/NVIDIA/TensorRT-LLM/tree/main/examples/mamba). For local inference, keep an eye out for support in llama.cpp. You may download the raw weights from [HuggingFace](https://huggingface.co/mistralai/mamba-codestral-7B-v0.1). This is an instructed model, with 7,285,403,648 parameters.

For easy testing, we made Codestral Mamba available on [la Plateforme](https://console.mistral.ai/) (`codestral-mamba-2407`), alongside its big sister, Codestral 22B. While Codestral Mamba is available under the Apache 2.0 license, Codestral 22B is available under a [commercial license](https://mistral.ai/contact/) for self-deployment or a community license for testing purposes.

0%
