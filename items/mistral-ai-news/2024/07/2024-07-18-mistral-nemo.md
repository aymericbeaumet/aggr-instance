---
title: Mistral NeMo
link: https://mistral.ai/news/mistral-nemo/
source: mistral-ai-news
published: 2024-07-18T08:00:00Z
updated: 2024-07-18T08:00:00Z
first_seen: 2026-09-19T21:30:23.395188495Z
content: extracted
html: 2024-07-18-mistral-nemo.html
preview:
  file: 2024-07-18-mistral-nemo.preview-3ca724984aa1.webp
  width: 256
  height: 153
  color: '#c88b6d'
images:
- source: https://mistral.ai/cms-media/api/media/file/Thumbnail-Model-Nemo.jpg
  original:
    file: 2024-07-18-mistral-nemo.image-3055df2cef07.jpg
    width: 1800
    height: 1074
  color: '#fdb699'
- source: https://mistral.ai/_astro/41a3984b-6dd3-47c1-862f-174ff153c577_ZefJVa.webp?dpl=6aad049eaf4c2d00095b91e5
  original:
    file: 2024-07-18-mistral-nemo.image-40a72139499d.webp
    width: 1920
    height: 419
  variants:
  - file: 2024-07-18-mistral-nemo.image-f1dfa56610c6.webp
    width: 320
    height: 70
  - file: 2024-07-18-mistral-nemo.image-d9f4779ec920.webp
    width: 640
    height: 140
  - file: 2024-07-18-mistral-nemo.image-ddeec7822e0a.webp
    width: 960
    height: 210
  color: '#f9f9f9'
- source: https://mistral.ai/_astro/20165ae8-c76a-4093-a5e9-4ece72e25ca7_2wCRCE.webp?dpl=6aad049eaf4c2d00095b91e5
  original:
    file: 2024-07-18-mistral-nemo.image-3b42f815a2a7.webp
    width: 1920
    height: 500
  variants:
  - file: 2024-07-18-mistral-nemo.image-316d5df34564.webp
    width: 320
    height: 83
  - file: 2024-07-18-mistral-nemo.image-91c35113f2de.webp
    width: 640
    height: 167
  - file: 2024-07-18-mistral-nemo.image-1dc1180dc346.webp
    width: 960
    height: 250
  color: '#fdfcfc'
- source: https://mistral.ai/_astro/d6dab8cf-bb81-41db-a6d1-3eccea0774aa_Z1HvHuS.webp?dpl=6aad049eaf4c2d00095b91e5
  original:
    file: 2024-07-18-mistral-nemo.image-c4facbf89394.webp
    width: 1920
    height: 718
  variants:
  - file: 2024-07-18-mistral-nemo.image-a83319e69a80.webp
    width: 320
    height: 120
  - file: 2024-07-18-mistral-nemo.image-b484ff12d07b.webp
    width: 640
    height: 239
  - file: 2024-07-18-mistral-nemo.image-7c09aae30e99.webp
    width: 960
    height: 359
  - file: 2024-07-18-mistral-nemo.image-695fab580caa.webp
    width: 1280
    height: 479
  color: '#fcfcfc'
- source: https://mistral.ai/_astro/77bc9654-2cfd-469d-a4dd-1b53074c18e4_Z13VWc4.webp?dpl=6aad049eaf4c2d00095b91e5
  original:
    file: 2024-07-18-mistral-nemo.image-2e2770815fa0.webp
    width: 1400
    height: 249
  variants:
  - file: 2024-07-18-mistral-nemo.image-ce8a6b94527c.webp
    width: 320
    height: 57
  - file: 2024-07-18-mistral-nemo.image-41e85e75893b.webp
    width: 640
    height: 114
  color: '#fefdfd'
---

Today, we are excited to release Mistral NeMo, a 12B model built in collaboration with NVIDIA. Mistral NeMo offers a large context window of up to 128k tokens. Its reasoning, world knowledge, and coding accuracy are state-of-the-art in its size category. As it relies on standard architecture, Mistral NeMo is easy to use and a drop-in replacement in any system using Mistral 7B.

We have released pre-trained base and instruction-tuned checkpoints checkpoints under the Apache 2.0 license to promote adoption for researchers and enterprises. Mistral NeMo was trained with quantisation awareness, enabling FP8 inference without any performance loss.

The following table compares the accuracy of the Mistral NeMo base model with two recent open-source pre-trained models, Gemma 2 9B, and Llama 3 8B.

![Mistral NeMo base model performance compared to Gemma 2 9B and Llama 3 8B](https://mistral.ai/_astro/41a3984b-6dd3-47c1-862f-174ff153c577_ZefJVa.webp?dpl=6aad049eaf4c2d00095b91e5)

Table 1: Mistral NeMo base model performance compared to Gemma 2 9B and Llama 3 8B.

## Multilingual Model for the Masses

The model is designed for global, multilingual applications. It is trained on function calling, has a large context window, and is particularly strong in English, French, German, Spanish, Italian, Portuguese, Chinese, Japanese, Korean, Arabic, and Hindi. This is a new step toward bringing frontier AI models to everyone’s hands in all languages that form human culture.

![Mistral NeMo performance on multilingual benchmarks](https://mistral.ai/_astro/20165ae8-c76a-4093-a5e9-4ece72e25ca7_2wCRCE.webp?dpl=6aad049eaf4c2d00095b91e5)

Figure 1: Mistral NeMo performance on multilingual benchmarks.

### Tekken, a more efficient tokenizer

Mistral NeMo uses a new tokenizer, Tekken, based on Tiktoken, that was trained on over more than 100 languages, and compresses natural language text and source code more efficiently than the SentencePiece tokenizer used in previous Mistral models. In particular, it is ~30% more efficient at compressing source code, Chinese, Italian, French, German, Spanish, and Russian. It is also 2x and 3x more efficient at compressing Korean and Arabic, respectively. Compared to the Llama 3 tokenizer, Tekken proved to be more proficient in compressing text for approximately 85% of all languages.

![Tekken compression rate](https://mistral.ai/_astro/d6dab8cf-bb81-41db-a6d1-3eccea0774aa_Z1HvHuS.webp?dpl=6aad049eaf4c2d00095b91e5)

Figure 2: Tekken compression rate.

## Instruction fine-tuning

Mistral NeMO underwent an advanced fine-tuning and alignment phase. Compared to Mistral 7B, it is much better at following precise instructions, reasoning, handling multi-turn conversations, and generating code.

![Mistral NeMo instruction-tuned model accuracy](https://mistral.ai/_astro/77bc9654-2cfd-469d-a4dd-1b53074c18e4_Z13VWc4.webp?dpl=6aad049eaf4c2d00095b91e5)

Table 2: Mistral NeMo instruction-tuned model accuracy. Evals done with GPT4o as judge on official references.

## Links

Weights are hosted on HuggingFace both for the [base](https://huggingface.co/mistralai/Mistral-Nemo-Base-2407) and for the [instruct](https://huggingface.co/mistralai/Mistral-Nemo-Instruct-2407) models. You can try Mistral NeMo now with mistral-inference and adapt it with mistral-finetune. Mistral NeMo is exposed on la Plateforme under the name `open-mistral-nemo-2407`. This model is also packaged in a container as NVIDIA NIM inference microservice and available from [ai.nvidia.com](https://ai.nvidia.com/).
