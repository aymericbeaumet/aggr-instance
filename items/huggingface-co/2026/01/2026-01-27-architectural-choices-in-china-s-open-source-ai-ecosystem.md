---
title: 'Architectural Choices in China''s Open-Source AI Ecosystem: Building Beyond DeepSeek'
link: https://huggingface.co/blog/huggingface/one-year-since-the-deepseek-moment-blog-2
source: huggingface-co
published: 2026-01-27T15:01:45Z
updated: 2026-01-27T15:01:45Z
first_seen: 2026-10-04T12:50:30.804479567Z
content: extracted
html: 2026-01-27-architectural-choices-in-china-s-open-source-ai-ecosystem.html
---

* * *

This is the second blog in a three-part series on China's open source community's historical advancements since January 2025's "DeepSeek Moment." The first blog is available [here](https://huggingface.co/blog/huggingface/one-year-since-the-deepseek-moment), and the third blog is available [here](https://huggingface.co/blog/huggingface/one-year-since-the-deepseek-moment-blog-3).

In this second piece we turn our focus from models to the architectural and hardware choices Chinese companies have made as openness becomes the norm.

For AI researchers and developers contributing to and relying on the open source ecosystem and for policymakers understanding the rapidly changing environment, **architectural preferences, modality diversification, license permissiveness, small model popularity, and growing adoption of Chinese hardware point to leadership strategies across a multitude of paths. DeepSeek R1's own characteristics inspired overlap and competition, and contributed to heavier focus on domestic hardware in China.**

### Mixture of Experts (MoE) as the Default Choice

In the past year, **leading models from the Chinese community had almost unanimously moved toward Mixture-of-Experts (MoE) architectures**, including [Kimi K2](https://huggingface.co/collections/moonshotai/kimi-k2), [MiniMax M2](https://huggingface.co/MiniMaxAI/MiniMax-M2), and [Qwen3](https://huggingface.co/collections/Qwen/qwen3). R1 itself was an MoE model, it also proved a crucial point: **strong reasoning could be open, reproducible, and engineered in practice.** Under China's real-world constraints, maintaining high capability while controlling cost, and ensuring models could be trained, deployed, and widely adopted, MoE emerged as a natural solution.

MoE is like a controllable compute distribution system; under a single capability framework, compute resources are allocated across requests and deployment environments by dynamically activating different numbers of experts according to task complexity and value. More importantly, it does not require every inference to consume the full set of resources, nor does it assume that all deployment environments share identical hardware conditions.

The overall direction of Chinese open-source models in 2025 was clear: **not necessarily the strongest possible performance, but ability to operate sustainably, deploy flexibly, and evolve continuously, achieving the best cost performance balance.**

### The Rush for Supremacy by Modality

Starting in February 2025, open-source activity was no longer focused only on text models. It quickly expanded into multimodal and agent-based directions: **Any-to-Any models, text-to-image, image-to-video, text-to-video, TTS, 3D,** and **agents** all progressed in parallel. What the community pushed forward was not just model weights, but a full set of engineering assets, including inference deployment, datasets and evaluation, toolchains, workflows, and edge-to-cloud coordination. The parallel emergence of video generation tools, 3D components, distillation datasets, and agent frameworks pointed to something larger than isolated breakthroughs---it pointed to reusable system-level capabilities.

The competition to lead similar to DeepSeek in a non-text modality heated up. StepFun's released high performance multimodal models, excelling in [audio](https://huggingface.co/collections/stepfun-ai/step-audio), [video](https://huggingface.co/stepfun-ai/stepvideo-t2v), and [image](https://huggingface.co/stepfun-ai/Step1X-Edit) generation and processing or editing. Their latest speech-to-speech model [Step-Audio-R1.1](https://huggingface.co/stepfun-ai/Step-Audio-R1.1) boasts state-of-the-art performance, beating proprietary models.  Tencent also reflected this shift through open-source work in video and 3D. Its [Hunyuan Video](https://huggingface.co/collections/tencent/hunyuanvideo) models and projects such as [Hunyuan 3D](https://huggingface.co/collections/tencent/hunyuan3d) reflect growing competition beyond text-centric models.

### Big Preferences for Small Models

Models in the 0.5B--30B range were easier to run locally, fine-tune, and integrate into business systems and agent workflows. For example: Among the Qwen series, [Qwen 1.5-0.5B](https://huggingface.co/Qwen/Qwen1.5-0.5B) has the most derivative models. In environments with limited compute or strict compliance requirements, these models were far better suited for long-term operation. At the same time, leading players often used large MoE models in the 100B--700B range as capability ceilings or "teacher models," then distilled those capabilities down into many smaller models. This created a clear structure: a few very large models at the top, and many practical models underneath. The growing share of small models in monthly summaries reflected real usage needs in the community.

[https://huggingface.co/spaces/cfahlgren1/hub-model-tree-stats](https://huggingface.co/spaces/cfahlgren1/hub-model-tree-stats)

### More Permissive Open Source Licenses

After R1, Apache 2.0 became close to the default choice for open models from the Chinese community. **More permissive licenses lowered the friction around using, modifying, and deploying models in production, making it much easier for companies to move open models into real systems.** Familiarity with standard licenses, such as Apache 2.0 and MIT, similarly eased usage; prescriptive and tailored licenses add friction through unfamiliarity and new legal barriers, contributing to the decline seen in the graph below.

[![output (8)](https://cdn-uploads.huggingface.co/production/uploads/63a369d98c0c89dcae3b8329/-aFgz1QVgGBJZ6pxsNU0z.png)](https://cdn-uploads.huggingface.co/production/uploads/63a369d98c0c89dcae3b8329/-aFgz1QVgGBJZ6pxsNU0z.png)

Based on the releases of all organizations shown in Chinese Open Source Heatmap

### From Model-First to Hardware-First

In 2025, model releases increasingly aligned with inference frameworks, quantization formats, serving engines, and edge runtimes. **A prominent goal was no longer just to make weights downloadable, but to ensure that models could run directly on target domestic hardware---and run reliably and efficiently.** This change was most visible on the inference side. For example, With [DeepSeek-V3.2-Exp](https://huggingface.co/deepseek-ai/DeepSeek-V3.2-Exp), both Huawei Ascend and Cambricon chips achieved day-zero support, not as cloud demos, but as reproducible inference pipelines released alongside the weights, enabling developers to validate real-world performance directly.

At the same time, training-side signals began to appear. Ant Group's [Ling](https://huggingface.co/collections/inclusionAI/ling) open models use optimized training on domestic AI chips to achieve near NVIDIA H800 performance, cutting the cost of training 1 trillion tokens by about 20%. Baidu's open [Qianfan-VL](https://huggingface.co/collections/baidu/qianfan-vl) models clearly documented that the model was trained on a cluster of more than 5,000 Baidu Kunlun P800 accelerators, their flagship AI chip, with details on parallelization and efficiency. At the beginning of 2026, Zhipu's [GLM-Image](https://huggingface.co/zai-org/GLM-Image) and China Telecom's latest open model - [TeleChat3](https://huggingface.co/Tele-AI/TeleChat3-36B-Thinking), were both announced as being trained entirely on domestic chips. These disclosures showed that domestic computers were no longer limited to inference, but had started to enter key stages of the training pipeline.

**On the serving and infrastructure side, engineering capabilities are being systematically open-sourced.** **Moonshot AI** released its serving system: [Mooncake](https://github.com/kvcache-ai/Mooncake), and explicitly supported features such as prefill/decoding separation. **By open-sourcing production-grade experience, these efforts significantly raised the baseline for deployment and operations across the community, making it easier to run models reliably at scale.** This direction was echoed across the ecosystem. Baidu's [FastDeploy 2.0](https://github.com/PaddlePaddle/FastDeploy) emphasized extreme quantization and cluster-level optimization to reduce inference costs under tight compute budgets. Alibaba's [Qwen](https://huggingface.co/Qwen) ecosystem pursued full-stack integration, tightly aligning models, inference frameworks, quantization strategies, and cloud deployment workflows to minimize friction from development to production. Still, reports of compute constraints in China threaten expansion; Zhipu AI is [reportedly](https://www.scmp.com/tech/tech-trends/article/3341000/computing-constraints-force-chinas-zhipu-restrict-sign-ups-ai-coding-service?module=top_story&pgtype=section) restricting usage amid a computing crunch.

When models, tools, and engineering are delivered together, the ecosystem no longer grows by adding projects, but by structurally differentiating on a shared foundation-and beginning to evolve on its own. How China will respond to U.S. hardware sales and export controls as NVIDIA sells H200s continues to be an [open question](https://www.forbes.com/sites/joetoscano1/2026/01/26/temporary-nvidia-h200-halt-shows-chinas-desire-for-ai-independence/). Read more about the shifting global compute landscape [here](https://huggingface.co/blog/huggingface/shifting-compute-landscape).

### Reconstruction In Progress

The "DeepSeek Moment" of January 2025 did more than trigger a wave of new open models. It forced a deeper reconsideration of how AI systems should be built when open source is no longer optional but foundational and why those underlying choices now carry strategic weight.

Chinese companies are no longer optimizing isolated models. Instead, they are pursuing distinct architectural paths aimed at building full ecosystems suited to an open-source world. In an increasingly commoditized model landscape, these decisions signal a clear shift in competition from model performance to system design.

Our next blog will go deeper into organizational wins and share some of what we expect to see in 2026.
