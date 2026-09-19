---
title: 'Introducing Mellum2: A 12B Mixture-of-Experts Model by JetBrains'
link: https://huggingface.co/blog/JetBrains/mellum2-launch
source: huggingface-co-blog
published: 2026-06-01T15:45:17Z
updated: 2026-06-01T15:45:17Z
first_seen: 2026-09-19T21:30:23.395188495Z
content: extracted
html: 2026-06-01-introducing-mellum2-a-12b-mixture-of-experts-model-by.html
preview:
  file: 2026-06-01-introducing-mellum2-a-12b-mixture-of-experts-model-by.preview-1185d9f3fbc2.webp
  width: 256
  height: 58
  color: '#01161e'
images:
- source: https://cdn-uploads.huggingface.co/production/uploads/60ef2a438432bc401cd0abbe/tFjSaWUOM_pVsgKjHrAHt.png
  original:
    file: 2026-06-01-introducing-mellum2-a-12b-mixture-of-experts-model-by.image-1056194aee81.png
    width: 760
    height: 172
  variants:
  - file: 2026-06-01-introducing-mellum2-a-12b-mixture-of-experts-model-by.image-89cdcc6e43ff.webp
    width: 320
    height: 72
  - file: 2026-06-01-introducing-mellum2-a-12b-mixture-of-experts-model-by.image-b27a6a7010b2.webp
    width: 640
    height: 145
  - file: 2026-06-01-introducing-mellum2-a-12b-mixture-of-experts-model-by.image-a977cf10039d.webp
    width: 760
    height: 172
  color: '#000000'
- source: https://huggingface.co/JetBrains/Mellum2-12B-A2.5B-Thinking/resolve/main/mellum_evals_grid_1700.jpg
  original:
    file: 2026-06-01-introducing-mellum2-a-12b-mixture-of-experts-model-by.image-ca522f499c5d.jpg
    width: 5100
    height: 4608
  color: '#020202'
---

- Mellum2 is a 12B-parameter Mixture-of-Experts model trained from scratch on natural language and code.
- The model activates only 2.5B parameters per token, making it efficient for high-throughput, low-latency inference. Mellum2 is can be used for routing, RAG, summarization, sub-agents, high-throughput coding features, and private deployments.
- It is released under the Apache 2.0 license.
- Compared with similar-sized models, Mellum2 delivers competitive benchmark performance while achieving more than 2x faster inference.
- Download the model on Hugging Face: [https://huggingface.co/collections/JetBrains/mellum-2](https://huggingface.co/collections/JetBrains/mellum-2)
- For architecture details, training setup, benchmarks, and evaluation methodology, read the full technical report: [https://arxiv.org/pdf/2605.31268](https://arxiv.org/pdf/2605.31268)

Today we’re releasing Mellum2, an open Mixture-of-Experts model optimized for low-latency text-and-code workloads. Mellum originally started as a code completion model. With Mellum2, we extend that foundation to a broader set of natural language and software engineering tasks while keeping the model focused on efficient inference and deployability. Modern AI systems increasingly rely on multiple model calls: routing, retrieval, summarization, planning, validation, and tool use. Many of these operations are latency-sensitive and do not require the largest available model. Mellum2 targets these workloads.

## [](https://huggingface.co/blog/JetBrains/mellum2-launch#benchmark-highlights) Benchmark highlights

[![Mellum 2 Evals](https://huggingface.co/JetBrains/Mellum2-12B-A2.5B-Thinking/resolve/main/mellum_evals_grid_1700.jpg)](https://huggingface.co/JetBrains/Mellum2-12B-A2.5B-Thinking/resolve/main/mellum_evals_grid_1700.jpg)

In our technical report, we evaluate Mellum2 across code generation, reasoning, science, and math benchmarks. Mellum2 is competitive with similarly sized open models while delivering more than 2x faster inference, making it suitable for high-throughput production workloads. Model architecture Mellum2 is a Mixture-of-Experts model:

| Model   | Total parameters | Active parameters per token | Modality      | License    |
| ------- | ---------------- | --------------------------- | ------------- | ---------- |
| Mellum2 | 12B              | 2.5B                        | Text and code | Apache 2.0 |

The MoE architecture keeps total model capacity high while activating only a subset of parameters for each token. This makes inference more efficient and helps reduce serving cost for real-time workloads. Mellum2 is intentionally focused on text and code rather than multimodal tasks. This specialization keeps the model compact and efficient for software engineering workloads.

## [](https://huggingface.co/blog/JetBrains/mellum2-launch#key-use-cases) Key use cases

### [](https://huggingface.co/blog/JetBrains/mellum2-launch#routing-and-orchestration) Routing and orchestration

Mellum2 works well as a lightweight routing and orchestration model in multi-model systems, including prompt classification, tool selection, and intermediate control-flow steps.

### [](https://huggingface.co/blog/JetBrains/mellum2-launch#rag-pipelines) RAG pipelines

The model is well suited for latency-sensitive retrieval pipelines, including context compression, summarization, and retrieval post-processing.

### [](https://huggingface.co/blog/JetBrains/mellum2-launch#sub-agents) Sub-agents

Mellum2 can be used for agent subtasks such as planning, validation, transformation, and context preparation, reducing the need to invoke larger models for intermediate operations.

### [](https://huggingface.co/blog/JetBrains/mellum2-launch#private-deployment) Private deployment

Because Mellum2 is open and efficient to serve, it can be deployed in self-hosted environments involving proprietary code or internal data.

## [](https://huggingface.co/blog/JetBrains/mellum2-launch#why-well-scoped-models-matter) Why well-scoped models matter

As AI systems mature, the most effective architectures are becoming less monolithic. A single frontier model can be powerful, but production systems often need several specialized components working together: retrievers, routers, code-aware models, validators, tool callers, and larger reasoning models. We think of Mellum2 as a “focal” model: a fast, well-scoped model optimized for high-frequency tasks inside larger AI systems. The goal is not to replace every model in the stack. The goal is to make the stack faster, cheaper, and easier to control.

## [](https://huggingface.co/blog/JetBrains/mellum2-launch#getting-started-with-mellum2) Getting started with Mellum2

If you are building AI systems for software engineering – inside an IDE, in a RAG pipeline, as part of an agent workflow, or on private infrastructure – Mellum2 is [ready to try](https://huggingface.co/collections/JetBrains/mellum-2).
