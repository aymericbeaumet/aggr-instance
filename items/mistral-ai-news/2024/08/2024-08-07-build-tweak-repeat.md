---
title: Build, tweak, repeat
link: https://mistral.ai/news/build-tweak-repeat/
source: mistral-ai-news
published: 2024-08-07T14:00:00Z
updated: 2024-08-07T14:00:00Z
first_seen: 2026-09-19T21:30:23.395188495Z
content: extracted
html: 2024-08-07-build-tweak-repeat.html
preview:
  file: 2024-08-07-build-tweak-repeat.preview-159ccdda9cb1.webp
  width: 256
  height: 153
  color: '#b65204'
images:
- source: https://mistral.ai/cms-media/api/media/file/thumbnail-09.jpg
  original:
    file: 2024-08-07-build-tweak-repeat.image-8ce5007f9ea9.jpg
    width: 1800
    height: 1074
  color: '#b95303'
- source: https://mistral.ai/_astro/9e1164c6-20f7-4ef5-b961-69add4a76fe2_Z18nasm.webp?dpl=6aad049eaf4c2d00095b91e5
  original:
    file: 2024-08-07-build-tweak-repeat.image-f7879d55c82e.webp
    width: 1920
    height: 1035
  variants:
  - file: 2024-08-07-build-tweak-repeat.image-a0e15fc5a265.webp
    width: 320
    height: 173
  - file: 2024-08-07-build-tweak-repeat.image-c2860b2b3cc0.webp
    width: 640
    height: 345
  - file: 2024-08-07-build-tweak-repeat.image-9704e23c1c13.webp
    width: 960
    height: 518
  - file: 2024-08-07-build-tweak-repeat.image-c5b8116699fb.webp
    width: 1280
    height: 690
  color: '#fe480a'
---

Research

August 7, 2024

By Mistral AI team

![Detailed benchmarks](https://mistral.ai/_astro/9e1164c6-20f7-4ef5-b961-69add4a76fe2_Z18nasm.webp?dpl=6aad049eaf4c2d00095b91e5)

Language models are changing the way we build software, serving as a flexible orchestrator in between knowledge sources and user interfaces. Building such software comes with new challenges to improve quality, reduce latency, and prototype quickly. Today, we're announcing various advancements in this direction.

### Simpler, more efficient model customization

Because large language models are rapidly finding newer and more specialised use cases, it is critical that developers are able to quickly and efficiently tailor frontier models to their specific applications. To that end, we're announcing the ability to customise any of our flagship and specialist models on La Plateforme, including Mistral Large 2 and Codestral.

Models can be customised using a base prompt, few-shot prompting, or fine-tuning, and you can bring your own dataset. Crucially, model customization follows the techniques developed by the Mistral AI science team for making strong reference models, so you can expect similar performance from your fine-tuned models. Developers can use model customization to integrate generative AI capabilities into their application with specific domain knowledge, context, or tone.

We expect fine-tuning on our highly capable models to unlock a wealth of groundbreaking applications, and are eager to see what will be built with it. Check out our [fine-tuning documentation](https://docs.mistral.ai/capabilities/finetuning/), and try model customization on [La Plateforme](https://console.mistral.ai/).

#### Alpha release of Agents

We're also introducing an early version of Agents, that wraps models with additional context and instruction, for exposure on Le Chat or API. Agents help you create custom behaviour and workflows with a simple set of instructions and examples. With the advanced reasoning capabilities of Mistral Large 2, you can layer on increasingly complex workflows with multiple agents that are easy to share within your organisation. We're working on connecting Agents to tools and data sources and are looking forward to your feedback on it.

[Learn more about Agents](https://docs.mistral.ai/capabilities/agents/).

### Stable version of our client SDK

We have made significant updates to the `mistralai` library to improve its usability and consistency, and today we are releasing mistralai 1.0, available for both [Python](https://github.com/mistralai/client-python) and [Typescript](https://github.com/mistralai/client-ts). Learn more about our new SDK and check out the [migration guide](https://github.com/mistralai/client-python/blob/main/MIGRATION.md).

0%
