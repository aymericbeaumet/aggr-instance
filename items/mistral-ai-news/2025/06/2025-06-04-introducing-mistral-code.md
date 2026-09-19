---
title: Introducing Mistral Code
link: https://mistral.ai/news/mistral-code/
source: mistral-ai-news
published: 2025-06-04T12:00:00Z
updated: 2025-06-04T12:00:00Z
first_seen: 2026-09-19T21:30:23.395188495Z
content: extracted
html: 2025-06-04-introducing-mistral-code.html
preview:
  file: 2025-06-04-introducing-mistral-code.preview-9bd6fd8a55c8.webp
  width: 256
  height: 153
  color: '#fc880c'
images:
- source: https://mistral.ai/cms-media/api/media/file/thumbnail-10.jpg
  original:
    file: 2025-06-04-introducing-mistral-code.image-538f58e708ea.jpg
    width: 1800
    height: 1074
  color: '#fd7a03'
- source: https://mistral.ai/_astro/6b71f3c5-d753-41b2-8ac5-6ec41534f0f0_ZLSMF5.webp?dpl=6aad049eaf4c2d00095b91e5
  original:
    file: 2025-06-04-introducing-mistral-code.image-f656b645f8e4.webp
    width: 1078
    height: 348
  variants:
  - file: 2025-06-04-introducing-mistral-code.image-1c472086577c.webp
    width: 320
    height: 103
  color: '#1d1d1d'
- source: https://mistral.ai/_astro/0c2975a6-0964-4a39-9af7-6cabb8bec71e_diGA8.webp?dpl=6aad049eaf4c2d00095b91e5
  original:
    file: 2025-06-04-introducing-mistral-code.image-509af9245a67.webp
    width: 1454
    height: 531
  variants:
  - file: 2025-06-04-introducing-mistral-code.image-543dc571acf3.webp
    width: 320
    height: 117
  - file: 2025-06-04-introducing-mistral-code.image-a4f8d2c90ea8.webp
    width: 640
    height: 234
  color: '#fec351'
- source: https://mistral.ai/_astro/46ca04b7-f62f-4855-b888-2c9ff09f90ce_ZwfvNt.webp?dpl=6aad049eaf4c2d00095b91e5
  original:
    file: 2025-06-04-introducing-mistral-code.image-7138ba2bac02.webp
    width: 1054
    height: 937
  variants:
  - file: 2025-06-04-introducing-mistral-code.image-c9e0178a9334.webp
    width: 320
    height: 284
  - file: 2025-06-04-introducing-mistral-code.image-7945b1d208c2.webp
    width: 640
    height: 569
  color: '#212528'
---

Software engineering teams in enterprises can finally bring frontier-grade AI coding into their workflow in a secure, compliant manner. Mistral Code is an AI-powered coding assistant that bundles powerful models, an in-IDE assistant, local deployment options, and enterprise tooling into one fully supported package, so developers can 10X their productivity with the full backing of their IT and security teams.

Mistral Code builds on the proven open-source project [Continue](https://github.com/continuedev/continue), reinforced with the controls and observability that large enterprises require. Private beta is open today for [JetBrains IDEs](https://plugins.jetbrains.com/plugin/27493-mistral-code-enterprise) and [VSCode](https://marketplace.visualstudio.com/items?itemName=mistralai.mistral-code), with general availability planned soon. Mistral Code is a continuation of our efforts to make developers successful with Al, following last month’s releases of [Devstral](https://mistral.ai/news/devstral) and [Codestral Embed](https://mistral.ai/news/codestral-embed).

![Mistral Code Hero](https://mistral.ai/_astro/6b71f3c5-d753-41b2-8ac5-6ec41534f0f0_ZLSMF5.webp?dpl=6aad049eaf4c2d00095b91e5)

## Why we built Mistral Code

Our goal with Mistral Code is simple: deliver best-in-class coding models to enterprise developers, enabling everything from instant completions to multi-step refactoring—through an integrated platform deployable in the cloud, on reserved capacity, or air-gapped on-prem GPUs.

Unlike typical SaaS copilots, all parts of the stack—from models to code—are delivered by one provider subject to a single set of SLAs, and every line of code resides inside the customer’s enterprise boundary.

When we spoke with VPs of engineering, platform leads, and CISOs, they surfaced four recurring blockers that stop mainstream copilots at the proof-of-concept stage:

1. Limited connectivity to proprietary repos and internal services.

2. Minimal customisation of the underlying models or prompts.

3. Shallow task coverage that ends at “autocomplete” instead of finishing multi-step work.

4. Fragmented SLAs spread across one vendor for the plug-in, another for the model, and a third for infra.

Mistral Code addresses those pain points with a single, vertically-integrated offering: models, plugin, admin controls, and 24X7 support—so platform teams retain visibility and can tie AI-powered productivity back to ROI.

![Architecture](https://mistral.ai/_astro/0c2975a6-0964-4a39-9af7-6cabb8bec71e_diGA8.webp?dpl=6aad049eaf4c2d00095b91e5)

## Code with intelligence, control with confidence

At its core, Mistral Code is powered by four models that are state of the art in coding:

1. [Codestral](https://mistral.ai/news/codestral) for fill-in-the-middle / code autocomplete

2. [Codestral Embed](https://mistral.ai/news/codestral-embed) for code search and retrieval

3. [Devstral](https://mistral.ai/news/devstral) for agentic coding

4. And [Mistral Medium](https://mistral.ai/news/mistral-medium-3) for chat assistance

Critically, customers can fine-tune or post-train the underlying models on private repositories or distill lightweight variants—capabilities that simply don’t exist in closed copilots tied to proprietary APIs.

Mistral Code is proficient in 80+ programming languages and can reason over files, Git diffs, terminal output, and issues. We’re currently testing the product in helping engineers move beyond coding assistance and suggestions to complete fully scoped tickets: opening files, writing new modules, updating tests, and even executing shell commands—all under configurable approval workflows so senior engineers stay in control.

For IT managers, a rich admin console exposes granular platform controls, deep observability, seat management, and usage analytics.

## Adopted by global enterprises

Our customers validate the approach: [Abanca](https://www.abanca.com/es/), a leading bank in Spain and Portugal, has deployed Mistral Code at scale for development teams in a hybrid configuration so they can prototype in the cloud while core banking code stays on-prem. [SNCF](https://www.groupe-sncf.com/en), France's national railway company, is empowering its 4000 developers with AI through Mistral Code Serverless, and [Capgemini](https://www.capgemini.com/news/press-releases/capgemini-partners-with-mistral-ai-to-spearhead-the-adoption-of-new-frontier-generative-ai-models/), our first global systems-integrator partner, is to deploy Mistral Code on-premises for 1500+ developers in the service of client projects in regulated industries.

![Capture Code](https://mistral.ai/_astro/46ca04b7-f62f-4855-b888-2c9ff09f90ce_ZwfvNt.webp?dpl=6aad049eaf4c2d00095b91e5)

We owe a debt of gratitude to the Continue community for pioneering the original developer experience; our fork preserves their extensibility but layers on significant improvements to multi-line editing, chat, and enterprise-grade extras such as fine-grained RBAC, audit logging, issue resolution / suggestion acceptance metrics, and so on. While our initial release is a private beta to collect customer feedback, we are excited to announce general availability soon, and aim to start making contributions to the upstream repo in forthcoming releases.

## Getting started

[Request access](https://mistral.ai/contact) from your Mistral account team to spin up a pilot. You can choose serverless, cloud, or self-hosted deployment—and get coding with frontier intelligence in minutes.
