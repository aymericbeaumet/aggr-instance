---
title: 'Nemotron 3.5 Content Safety: Customizable Multimodal Safety for Global Enterprise AI'
link: https://huggingface.co/blog/nvidia/nemotron-3-5-content-safety
source: huggingface-co-blog
published: 2026-06-04T18:57:45Z
updated: 2026-06-04T18:57:45Z
first_seen: 2026-09-19T21:30:23.395188495Z
content: extracted
html: 2026-06-04-nemotron-3-5-content-safety-customizable-multimodal-safety.html
preview:
  file: 2026-06-04-nemotron-3-5-content-safety-customizable-multimodal-safety.preview-de4cacc74a58.webp
  width: 256
  height: 138
  color: '#ececec'
images:
- source: https://cdn-thumbnails.huggingface.co/social-thumbnails/blog/nvidia/nemotron-3-5-content-safety.png
  original:
    file: 2026-06-04-nemotron-3-5-content-safety-customizable-multimodal-safety.image-9b9051a1a523.png
    width: 1200
    height: 648
  variants:
  - file: 2026-06-04-nemotron-3-5-content-safety-customizable-multimodal-safety.image-c1e0b32c92fb.webp
    width: 320
    height: 173
  - file: 2026-06-04-nemotron-3-5-content-safety-customizable-multimodal-safety.image-97878b1bb8c3.webp
    width: 640
    height: 346
  - file: 2026-06-04-nemotron-3-5-content-safety-customizable-multimodal-safety.image-ff48c6aae500.webp
    width: 960
    height: 518
  - file: 2026-06-04-nemotron-3-5-content-safety-customizable-multimodal-safety.image-d5e12b87c96a.webp
    width: 1200
    height: 648
  color: '#fefefe'
- source: https://cdn-uploads.huggingface.co/production/uploads/644c4b804ef896a09019a5b4/uomUY8i9DOEdH9YfOCCB0.png
  original:
    file: 2026-06-04-nemotron-3-5-content-safety-customizable-multimodal-safety.image-4764b8fb7e39.png
    width: 1025
    height: 634
  variants:
  - file: 2026-06-04-nemotron-3-5-content-safety-customizable-multimodal-safety.image-14470b124b18.webp
    width: 320
    height: 198
  - file: 2026-06-04-nemotron-3-5-content-safety-customizable-multimodal-safety.image-b57d7c88fd03.webp
    width: 640
    height: 396
  - file: 2026-06-04-nemotron-3-5-content-safety-customizable-multimodal-safety.image-7bdd7b3d5ccb.webp
    width: 960
    height: 594
  - file: 2026-06-04-nemotron-3-5-content-safety-customizable-multimodal-safety.image-f810268e05cf.webp
    width: 1025
    height: 634
  color: '#fdfdfd'
- source: https://cdn-uploads.huggingface.co/production/uploads/644c4b804ef896a09019a5b4/6vntaUhBuotVodz-9BkaX.png
  original:
    file: 2026-06-04-nemotron-3-5-content-safety-customizable-multimodal-safety.image-5f0a0dbac3a3.png
    width: 878
    height: 543
  variants:
  - file: 2026-06-04-nemotron-3-5-content-safety-customizable-multimodal-safety.image-8a104debc28b.webp
    width: 320
    height: 198
  - file: 2026-06-04-nemotron-3-5-content-safety-customizable-multimodal-safety.image-a33a47ebbf02.webp
    width: 640
    height: 396
  - file: 2026-06-04-nemotron-3-5-content-safety-customizable-multimodal-safety.image-7d2eedc0f9a8.webp
    width: 878
    height: 543
  color: '#fdfdfc'
- source: https://cdn-uploads.huggingface.co/production/uploads/644c4b804ef896a09019a5b4/w8u_dXJ_iRg3GDRzq5I3-.png
  original:
    file: 2026-06-04-nemotron-3-5-content-safety-customizable-multimodal-safety.image-92751f3b2a90.png
    width: 900
    height: 556
  variants:
  - file: 2026-06-04-nemotron-3-5-content-safety-customizable-multimodal-safety.image-3f81a5f166a9.webp
    width: 320
    height: 198
  - file: 2026-06-04-nemotron-3-5-content-safety-customizable-multimodal-safety.image-1e8e73e47188.webp
    width: 640
    height: 395
  - file: 2026-06-04-nemotron-3-5-content-safety-customizable-multimodal-safety.image-3769677a6045.webp
    width: 900
    height: 556
  color: '#fcfcfc'
- source: https://cdn-uploads.huggingface.co/production/uploads/644c4b804ef896a09019a5b4/5drKmlTOcLxVobY03RJ7_.png
  original:
    file: 2026-06-04-nemotron-3-5-content-safety-customizable-multimodal-safety.image-2236c7e92c7e.png
    width: 1534
    height: 920
  variants:
  - file: 2026-06-04-nemotron-3-5-content-safety-customizable-multimodal-safety.image-05beba4bce2b.webp
    width: 320
    height: 192
  - file: 2026-06-04-nemotron-3-5-content-safety-customizable-multimodal-safety.image-48ff58f5e62f.webp
    width: 640
    height: 384
  - file: 2026-06-04-nemotron-3-5-content-safety-customizable-multimodal-safety.image-1a03268492f7.webp
    width: 960
    height: 576
  - file: 2026-06-04-nemotron-3-5-content-safety-customizable-multimodal-safety.image-2d16977473c6.webp
    width: 1534
    height: 920
  color: '#fcfdfc'
---

The last two years have seen NVIDIA's content safety stack grow from a focused English text classifier into a family of specialized models—each extending coverage to new modalities, languages, and inference modes. [Nemotron 3 Content Safety](https://huggingface.co/nvidia/Nemotron-3-Content-Safety), released in March 2026, combined multimodal and multilingual capabilities for the first time in a single 4B-parameter model. Today, we are releasing [Nemotron 3.5 Content Safety](https://huggingface.co/nvidia/Nemotron-3.5-Content-Safety), which completes that arc: a single model that unifies multimodal input, multilingual reach, custom enterprise policy enforcement, and auditable reasoning into one inference call.

This post covers what changes in 3.5, the design decisions behind each new capability, and how to integrate the model into production safety pipelines.

## [](https://huggingface.co/blog/nvidia/nemotron-3-5-content-safety#whats-new-in-nemotron-35-content-safety) What's New in Nemotron 3.5 Content Safety

### [](https://huggingface.co/blog/nvidia/nemotron-3-5-content-safety#1-unified-multimodal-evaluation) 1. Unified Multimodal Evaluation

Nemotron 3 introduced image understanding; Nemotron 3.5 deepens the multimodal integration. The model takes a **user prompt, an optional image, and an optional assistant response** as a single context window and produces a coherent safety verdict over the combined input. Evaluating all three together—rather than scoring each independently—closes a well-known gap in multimodal safety scenarios: policy violations that only emerge from the *interaction* between text and image, or between request and response, are now caught in a single pass.

### [](https://huggingface.co/blog/nvidia/nemotron-3-5-content-safety#2-global-language-coverage) 2. Global Language Coverage

Nemotron 3.5 maintains the 12-language explicit training coverage of its predecessors—**English, French, Spanish, German, Chinese, Japanese, Korean, Arabic, Hindi, Russian, Portuguese, and Italian**—while also inheriting strong zero-shot generalization across approximately 140 languages from the Gemma 3 base model. This means deployments in markets where training data is sparse (e.g., Southeast Asian languages, Scandinavian languages, less-resourced African languages) benefit from base-model multilingual transfer without requiring separate fine-tuning.

### [](https://huggingface.co/blog/nvidia/nemotron-3-5-content-safety#3-custom-policy-enforcement) 3. Custom Policy Enforcement

This is the most significant architectural addition in 3.5 relative to Nemotron 3. Production deployments rarely operate under a single universal safety taxonomy. A healthcare platform has a different risk profile than a financial services chatbot, a developer tools IDE, or a children's education app. Nemotron 3.5 accepts a custom policy specification alongside the input. The model reasons over that policy when producing its verdict rather than deferring entirely to the built-in taxonomy. This extends the work first introduced in [Nemotron Content Safety Reasoning 4B](https://huggingface.co/nvidia/Nemotron-Content-Safety-Reasoning-4B) to the full multimodal, multilingual setting.

### [](https://huggingface.co/blog/nvidia/nemotron-3-5-content-safety#4-reasoning-traces-think-mode) 4. Reasoning Traces (THINK Mode)

Every safety verdict in Nemotron 3.5 can be accompanied by an auditable reasoning trace via an optional **think mode**. When enabled, the model outputs its step-by-step reasoning before delivering a final `safe` / `unsafe` label and, optionally, the violated categories.

```
<think>
The user prompt asks for guidance on acquiring a controlled substance without a prescription.
The assistant response provides specific sourcing steps and references an online marketplace.
This interaction violates the Criminal Planning/Confessions and Controlled Substances categories.
The image (a pharmacy exterior) provides locational context but does not alter the verdict.
</think>

User Safety: unsafe
Response Safety: unsafe
Safety Categories: Criminal Planning/Confessions, Controlled Substances
```

When latency is the primary constraint, THINK mode can be disabled to return to the same low-latency binary verdict available in Nemotron 3.

### [](https://huggingface.co/blog/nvidia/nemotron-3-5-content-safety#5-safety-dataset) 5. Safety Dataset

With Nemotron 3.5, we are releasing our safety dataset. This is an important milestone since most OSS safety models don't generally provide the training or evaluation sets. This problem is worse for the multimodal space where artifacts such as images or videos are often derived from resources with restrictive licensing terms. The Nemotron 3.5 Content Safety Dataset is multimodal, multilingual, and includes safety reasoning traces that were used to train the model. These reasoning traces were generated in a 2-step manner to make them concise, similar to the [Nemotron Content Safety Reasoning 4B](https://huggingface.co/nvidia/Nemotron-Content-Safety-Reasoning-4B) model.

* * *

## [](https://huggingface.co/blog/nvidia/nemotron-3-5-content-safety#model-architecture) Model Architecture

Nemotron 3.5 Content Safety is built on **Google Gemma 3 4B IT** (4B parameters), providing a 128K context window, strong vision-language reasoning, and broad multilingual coverage. NVIDIA fine-tunes this base with a LoRA adapter that installs targeted safety classification behavior while keeping the model compact enough for real-time deployment on 8GB+ VRAM GPUs.

The inference interface supports three output modes:

**Mode 1 — Low-latency binary verdict:**

```
User Safety: safe
Response Safety: unsafe
```

**Mode 2 — Binary verdict with categories:**

```
User Safety: safe
Response Safety: unsafe
Safety Categories: Violence, Criminal Planning/Confessions
```

**Mode 3 — THINK mode (reasoning + verdict):**

```
<think>
[step-by-step reasoning trace]
</think>

User Safety: unsafe
Response Safety: unsafe
Safety Categories: [categories]
```

The safety taxonomy follows the **Aegis 2.0** framework: 13 core categories aligned with the MLCommons safety taxonomy, plus 10 fine-grained subcategories. This alignment allows direct comparison with other open and closed guard systems benchmarked on Aegis-taxonomy datasets.

* * *

## [](https://huggingface.co/blog/nvidia/nemotron-3-5-content-safety#reasoning) Reasoning

Reasoning is a supercharger for content safety classification because it provides the necessary context, customization, and accountability required for production AI systems, especially in enterprise and regulated environments.

**Enables Custom and Contextual Policy Enforcement**

Reasoning allows a content safety model to dynamically interpret and enforce custom, domain-specific policies defined in natural language at the time of inference. This is necessary because production deployments rarely operate under a single, universal safety taxonomy. A financial services chatbot has a different risk profile than a children's education app which may have a lower tolerance for profanity. This capability supports:

- **Category Suppression:** Disabling irrelevant categories, such as preventing a "violence" category trigger when a DevOps tool handles the phrase "terminate a process".
- **Custom Category Injection:** Defining proprietary risk categories specific to an organization's regulatory or product policies.

**Provides Auditable and Documented Justification**

The reasoning traces show the model's step-by-step logic before it delivers a final safe or unsafe verdict. This documented justification serves several purposes:

- **Compliance and Audit Logging:** Regulated industries often require documented justifications for content moderation decisions.
- **Human Review:** Reviewers can audit *why* a verdict was reached to identify systematic model errors.
- **Policy Iteration:** The traces reveal how the model interprets edge cases, allowing teams to iteratively refine and improve custom policy language.

**Latency**

While reasoning can introduce latency, the Nemotron model addresses this by condensing reasoning chains into concise summaries to limit output tokens and increase efficiency. This is done in a 2-step process similar to what was done in the predecessor model [Nemotron-Content-Safety-Reasoning-4B](https://huggingface.co/nvidia/Nemotron-Content-Safety-Reasoning-4B). In the first step, we use larger, more powerful models such as Qwen 397B to generate chain-of-thought reasoning traces based upon provided prompts, images, and responses. We also provided the ground-truth labels of the samples to avoid any misclassification that can find its way into the reasoning traces. In step 2, we make these reasoning traces more concise by using another large model such as Qwen 80B. We specifically instruct this model to rephrase the original traces (from step 1) so that it fits in no more than 3 sentences. Based on our experiments, most reasoning traces generated are under 3 sentences.

The efficient reasoning traces optimization allows for low-latency custom policy enforcement. Furthermore, the reasoning traces provide a valuable training signal that can be used for training specialized moderator models. Developers can choose a dual-mode operation, disabling reasoning for minimal latency in generic tasks or enabling it for complex policies.

* * *

## [](https://huggingface.co/blog/nvidia/nemotron-3-5-content-safety#training-data) Training Data

The dataset driving Nemotron 3.5 is an evolution of the multimodal, multilingual blends used for Nemotron 3, with additions targeting the reasoning and custom-policy capabilities. We have used the following sources of data:

- **Multilingual text safety data** from [Nemotron Safety Guard Dataset v3](https://huggingface.co/datasets/nvidia/Nemotron-Safety-Guard-Dataset-v3), sampled from culturally nuanced subsets with proportional representation across safety categories and safe/unsafe splits.
- **Human-annotated multimodal data** collected in English by NVIDIA, translated into 12 languages. Critically, **99% of training images are real photographs**—not synthetic generations. This directly addresses a known weakness in the multimodal safety benchmark landscape, where existing datasets like VLGuard and MM-SafetyBench rely heavily on SDXL-generated images that lack the cultural texture and adversarial complexity of production content. While not all of these real images could be released due to licensing constraints, we are still able to release a subset of images from Wikimedia and synthetic generation.
- **Safe multimodal data** from [Nemotron VLM Dataset v2](https://huggingface.co/datasets/nvidia/Nemotron-VLM-Dataset-v2), covering scanned documents, charts, papers, and diagrams with associated queries—ensuring the model does not over-flag benign professional content.
- **Reasoning traces** derived from chain-of-thought outputs produced by larger teacher models—Qwen 397B and then shortened using Qwen 80B—are used to teach the model how to reason.
- **Topic following data** from the [CantTalkAboutThis](https://huggingface.co/datasets/nvidia/CantTalkAboutThis-Topic-Control-Dataset) dataset consisting of policy-specification/verdict pairs across a range of enterprise deployment scenarios (healthcare, finance, banking, education, etc.).
- **Synthetic data** accounting for roughly 10% of total training volume, used primarily to diversify jailbreak patterns, generate rare policy violation examples, and produce multimodal adversarial cases.

* * *

## [](https://huggingface.co/blog/nvidia/nemotron-3-5-content-safety#benchmarking) Benchmarking

Nemotron 3.5 Content Safety was evaluated across multilingual, multimodal, and custom-policy safety benchmarks, including VLGuard, MM-SafetyBench, PolyGuard, RTP-LX, Aya Redteaming, XSafety, MultiJail, Aegis, Dynaguardrail, and CoSA. These evaluations reflect the core production challenge for enterprise safety: applying consistent guardrails across global languages, text and image inputs, and domain-specific policies without adding significant latency.

Nemotron 3 set a strong baseline with 84% average accuracy on multimodal harmful-content tests and roughly half the latency of LlamaGuard-4-12B. Nemotron 3.5 maintains that compact 4B efficiency while adding custom policy support and reasoning traces.

Across multilingual and multimodal safety benchmarks, Nemotron 3.5 delivers strong harmful-content classification accuracy while maintaining a compact footprint. This matters because many safety models remain English-first, text-only, or too costly to run repeatedly in production pipelines. Nemotron 3.5 is designed to combine multilingual coverage, multimodal classification, custom-policy support, and low-latency deployment in one model.

[![figure1](https://cdn-uploads.huggingface.co/production/uploads/644c4b804ef896a09019a5b4/uomUY8i9DOEdH9YfOCCB0.png)](https://cdn-uploads.huggingface.co/production/uploads/644c4b804ef896a09019a5b4/uomUY8i9DOEdH9YfOCCB0.png)

*Figure 1. Nemotron 3.5 Content Safety delivers strong harmful-content classification accuracy across multilingual and multimodal safety benchmarks, averaging about 85% across the evaluated benchmark set.*

The language-level results highlight why multilingual safety matters for global enterprise AI. On Multilingual Aegis, Nemotron 3.5 averages 96.5% harmful-content classification accuracy across 12 languages. On RTP-LX, it averages 88.8%, for a combined Aegis and RTP-LX average of 92.7%. This consistency helps teams apply the same safety posture across customer, employee, and partner-facing workflows instead of relying on English-only moderation or separate regional safety models.

[![figure2](https://cdn-uploads.huggingface.co/production/uploads/644c4b804ef896a09019a5b4/6vntaUhBuotVodz-9BkaX.png)](https://cdn-uploads.huggingface.co/production/uploads/644c4b804ef896a09019a5b4/6vntaUhBuotVodz-9BkaX.png) *Figure 2. Nemotron 3.5 Content Safety averages 97% harmful-content classification accuracy on Multilingual Aegis Cultural + Adapted (prompt classification) (harmful-f1) across 12 languages.*

[![figure3](https://cdn-uploads.huggingface.co/production/uploads/644c4b804ef896a09019a5b4/w8u_dXJ_iRg3GDRzq5I3-.png)](https://cdn-uploads.huggingface.co/production/uploads/644c4b804ef896a09019a5b4/w8u_dXJ_iRg3GDRzq5I3-.png) *Figure 3. Nemotron 3.5 Content Safety averages 89% harmful-content classification accuracy on RTPLX (prompt classification) (harmful-f1) across 12 languages.*

Accuracy alone is not enough for production guardrails. Safety models must also be efficient enough to run before content is processed, returned, or routed downstream. Nemotron 3.5 Content Safety's compact 4B design helps reduce the cost and latency of repeated safety checks, making multilingual and multimodal guardrails practical for real-world AI applications.

## [](https://huggingface.co/blog/nvidia/nemotron-3-5-content-safety#latency) Latency

The latency profile is unchanged from Nemotron 3 in the default (no THINK) mode. THINK mode adds inference time proportional to trace length, but this overhead is predictable and can be budgeted separately from the synchronous moderation loop—for instance, by running THINK-mode evaluation asynchronously as part of an audit pipeline while the default mode handles real-time decisions.

[![figure4](https://cdn-uploads.huggingface.co/production/uploads/644c4b804ef896a09019a5b4/5drKmlTOcLxVobY03RJ7_.png)](https://cdn-uploads.huggingface.co/production/uploads/644c4b804ef896a09019a5b4/5drKmlTOcLxVobY03RJ7_.png) *Figure 4. Nemotron 3.5 Content Safety achieves 3x lower end-to-end latency on a multimodal benchmark compared to an alternative multimodal safety model.*

Compared to another reasoning safety model, our model generated up to 50% fewer tokens when reasoning is enabled, making it efficient in terms of cost and latency.

* * *

## [](https://huggingface.co/blog/nvidia/nemotron-3-5-content-safety#addressing-the-benchmark-gap) Addressing the Benchmark Gap

A recurring theme in multimodal safety research is the gaps in existing evaluation infrastructure. Nemotron 3.5's development encountered the same gaps documented in the broader literature:

- **Text-only coverage**: The most widely cited safety benchmarks (WildGuard, XSTest, HarmBench) are text-only. Multimodal performance cannot be inferred from text-benchmark results.
- **Synthetic image quality**: Most multimodal benchmarks that exist use AI-generated images (typically SDXL) rather than real photographs, understating the difficulty of real production content.
- **Real-image licensing**: Stock photo licenses prohibit redistribution in AI datasets, creating a structural gap between research and production conditions.

NVIDIA's multimodal training data—with real images and culturally nuanced multilingual prompts—is designed to fill some of these gaps for model training. The benchmark gap for evaluation remains an open problem for the broader safety research community.

## [](https://huggingface.co/blog/nvidia/nemotron-3-5-content-safety#getting-started) Getting Started

Nemotron 3.5 Content Safety is available on [Hugging Face](https://huggingface.co/nvidia/Nemotron-3.5-Content-Safety) under the NVIDIA Open Model License for research and commercial use, along with the training [dataset](https://huggingface.co/datasets/nvidia/Nemotron-3.5-Content-Safety-Dataset). It supports transformers, vLLM, and SGLang, and is available as a production-grade [NVIDIA NIM](https://nvcr.io/nim/nvidia/nemotron-3.5-content-safety:2.0.5-variant) on build.nvidia.com for teams that need a pre-packaged, GPU-optimized inference microservice.

Developers can also access the model through inference platforms including [Baseten](https://www.baseten.co/library/nemotron-3-5-content-safety/), [Eigen AI](https://www.eigenai.com/blog/2026-06-04-eigenai-delivers-day-0-inference-nvidia-nemotron-3-x-family-ultra-asr-content-safety), [DeepInfra](https://deepinfra.com/nvidia/Nemotron-Content-Safety-3.5), [OpenRouter](https://openrouter.ai/nvidia/nemotron-3.5-content-safety:free), and [Vultr](https://blogs.vultr.com/nemotron-3-5-content-safety).

For custom policy workflows, NVIDIA provides a Claude- and Codex-compatible [skill for generating custom policies](https://github.com/NVIDIA-NeMo/Nemotron/tree/main/skills/nemotron-policy-generator), along with [cookbooks showing how to use the model](https://github.com/NVIDIA-NeMo/Nemotron/tree/main/usage-cookbook/Nemotron-3.5-Content-Safety). Custom policies and reasoning traces help teams adapt safety behavior to domain-specific rules while keeping decisions auditable.
