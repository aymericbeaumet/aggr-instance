---
title: 'Le Chonk: Built for Coders, Trusted by Defenders.'
link: https://mistral.ai/news/mistral-large-4-oss/
source: mistral-ai
published: 2026-10-29T12:00:07Z
updated: 2026-10-29T12:00:07Z
first_seen: 2026-10-29T15:30:03.869733718Z
summary: The weights of Mistral Large 4 (Le Chonk) are now open. A 1T parameter multimodal model, and best open-weight model for coding, trained in Europe.
content: extracted
html: 2026-10-29-le-chonk-built-for-coders-trusted-by-defenders.html
remote_preview:
  url: https://mistral.ai/cms-media/api/media/file/cover-mistral-sovereign%20copie.webp
---

Thinking

Summary

Mistral released Mistral Large 4 (Le Chonk), a 1-trillion-parameter multimodal model, now available with improved coding, cybersecurity, and reasoning efficiency. It leads open-weight benchmarks in coding and defensive security while offering three reasoning modes for cost-performance balance. Fully open-weight and European-trained, it empowers sovereign AI with customizable, self-hosted deployment options.

**We release the weights of Le Chonk, advancing the frontier of open models in agentic coding and cybersecurity.**

Earlier this month, we released Mistral Large 4 (Le Chonk), our flagship 1-trillion-parameter, natively multimodal model, in public preview. The response to the preview has exceeded our expectations, and we're grateful to our developer community, early-access customers, and partners for their support, collaboration, and feedback.

![](https://mistral.ai/_astro/partners-ml4-oss_Z17Vyq1.webp?dpl=6ae35be2d40739ee25f710ce)

Since then, we've put that feedback to work while continuing to scale reinforcement learning, driving massive improvements in ***coding*** and defensive ***cybersecurity***. Gains continue to accelerate, with no signs of saturation.

**Today, we're releasing the weights of this improved Mistral Large 4.** The new model also features substantial gains in reasoning efficiency that give developers greater performance while lowering inference costs.

Mistral Large 4 ranks as the **best open-weight model for coding** and among the world's most capable open models overall. Trained from scratch on European infrastructure with clear lineage from inception to release, it gives organizations a trusted foundation for Sovereign AI

Mistral is defining the sovereign AI category and leading the open frontier. We're committed to building the world's most capable models and releasing them openly, giving developers, enterprises, and institutions the power to own, customize, and control their AI.

## Built for the Builders.

Mistral Large 4 entered public preview with strong results across leading coding benchmarks. Just three weeks of additional reinforcement learning have since propelled it to the forefront of open-weight coding, taking the lead on FrontierCode and Terminal-Bench while matching the best open-weight models on the Artificial Analysis Coding Agent Index with substantially greater token efficiency.

On Cognition's private FrontierCode 2.0 benchmark, which assesses end-to-end code quality and whether a repository maintainer would merge the agent's change, Mistral Large 4 leads with a score of 19.7%.

### Cognition - FrontierCode 2.0

## Pushing the pareto frontier for agentic coding.

Mistral Large 4 delivers frontier coding performance with significantly fewer tokens, making sophisticated coding agents more economical to run at scale.

On the Artificial Analysis Coding Agent Index, Mistral Large 4 scores 48%, effectively tied with the leading open-weight model, GLM-5.3, while using 47% fewer tokens per task on average.

### Artificial Analysis Coding Agent Index vs. Token Efficiency

- Mistral

- Other models

Coding Agent Index scores and token efficiencies for reasoning levels available on Artificial Analysis

The gains extend to terminal-based coding workflows. On Terminal-Bench 4.0, Mistral Large 4 improved by 13 percentage points to 41% compared to its public preview, taking the lead among open-weight models while reducing output tokens by 30%.

### Artificial Analysis Coding Agent Index - Terminal-Bench 4.0

To give developers greater control over this tradeoff between performance and cost, Mistral Large 4 now supports three reasoning modes:

|                |                                                                   |
| -------------- | ----------------------------------------------------------------- |
| **Mode**       | **Recommended Use**                                               |
| Low            | Simple tasks; replaces ‘None’ reasoning mode                      |
| Medium         | Everyday tasks                                                    |
| High (default) | Hardest use cases requiring high precision and thorough reasoning |

Beyond benchmarks, Mistral Large 4's coding capabilities are validated by human preference. In Surge Code's blind pairwise evaluations, Mistral Large 4 was preferred over leading open-weight models including GLM-5.3, Kimi K3, and DeepSeek V4.1 Flash, ranking behind only Claude Opus 5.5 among the models evaluated.

### Surge Code - Human preference on code

Blind side-by-side win rates against other models

- Mistral Large 4 win

- Other model

Share of prompts where graders preferred Mistral Large 4's answer over the opponent's.

## Built for the Defenders.

Since its preview, we've been working with leading cybersecurity vendors to evaluate Mistral Large 4 in real-world environments to test Mistral Large 4’s strengths in vulnerability reproduction and patching.

Mistral Large 4 scores 51% on the Artificial Analysis Cyber Index, which measures a model’s ability to discover vulnerabilities in a codebase, reproduce and validate them, and patch them without breaking existing functionality. Crucially, Mistral Large 4 handles these legitimate defensive security tasks without unnecessary refusals.

### Artificial Analysis - Cyber Index

Safety has improved alongside capability. Our model’s built-in guardrails distinguishes between defensive and malicious, offensive cybersecurity tasks. It retains strong performance on Cyber benchmarks, while achieving an average refusal rate of 96.6% on malicious cyber requests, the highest we measure among open models.

### Refusal of harmful cyber requests

Average refusal on the cyber-related prompts of JailbreakBench, StrongREJECT and AgentHarm

Here’s what global leaders in cybersecurity have to say about Mistral Large 4:

In the time since the preview, we carefully monitored cybersecurity-related use in our live traffic. Though we observed the model refusing to comply with black-box attacks against live targets, we see a potential for uplift to conduct real attacks. We are modifying our guardrails accordingly, and will continue updating them in the months to come.

We are also providing enterprise design partners such as Orange Cyberdefense, with preview access to a specialized variant of Mistral Large 4 optimized for advanced cybersecurity workflows, including penetration testing, vulnerability assessment, and red-teaming. Access is subject to rigorous vetting and contractual safeguards, with testing restricted to owned or explicitly authorized systems in controlled environments.

Meanwhile, customers are tailoring Mistral Large 4 with proprietary security data, policies, detection logic, and threat environments using Mistral [Forge](https://mistral.ai/products/forge/). For well-defined cybersecurity tasks, it can be further optimized into smaller, task-specific variants for greater accuracy, making security applications more effective.

[Learn more about sovereign AI for cybersecurity and how to work with us.](https://mistral.ai/industry/cybersecurity)

## Built for Work.

Mistral Large 4 has continued improving on a range of broad enterprise-use metrics.

**For finance-related work**, we improved the model’s ability to perform real-world workflows, especially ones involving the creation and editing of spreadsheets for finance and accounting use cases. This is reflected in our Finch scores climbing to 73.3%, second only to Kimi K3 for open models.

### FinWorkBench (Finch)

**Web browsing** is a critical part of a majority of knowledge work. Our RL pipeline enabled us to reach 84.5% on BrowseComp (starting from an SFT checkpoint that was achieving 75%). This brings Mistral Large 4 at a higher performance than models like GLM-5.3 or DeepSeek Flash 4.1 1017.

### Web browsing - BrowseComp

**Agents deployed by an organization need to be secure.** Whether deployed to internal members or external users, agents shouldn’t open you up to threats like system prompt exfiltration or malicious code injection. RL increased our gains on the B3 Agent Security Benchmark, with Mistral Large 4 resisting 97.1% of attacks vs 93.3% in Preview. This places it at the top of the OSS frontier, meaning anyone hosting the model can run a safe agent without additional tuning.

### B3 Agent security benchmark - Attack resistance

This major investment by Mistral AI reinforces the choice La Banque Postale made over a year ago and underscores the relevance of the long-term strategic partnership we are building together: supporting the emergence of an open and sovereign European AI leader. For a public and citizen bank, technological sovereignty is not optional; it is a key prerequisite for maintaining our customers' trust. The introduction of this open model unlocks new opportunities and use cases. By partnering with Mistral AI, our ambition is to combine the best of human expertise and digital technology to deliver an enhanced experience of both customers and employees over the long term

Zakaria Moursli, Executive Vice President, Operational Performance, Payments and Digital Services, La Banque Postale

## See it in Action

Cyber Vulnerability Triage: an agent reads incoming vulnerability reports, investigates the source code and product context, reproduces the flaws in an isolated sandbox, then writes and iterates on a patch until a canary exploit no longer works.

Agentic webdev: with no human help, Mistral Large 4 builds Orbital Trace, a live 3D map of roughly 19,000 satellites and debris pieces orbiting Earth. It pulls real tracking data, writes the app, uses it in a browser to catch its own bugs and slowdowns, then fixes what an independent reviewer flags until all requirements pass.

Quantum chemistry solver: Mistral Large 4 writes, from scratch, software that calculates where electrons sit in a molecule, and checks its answers against a standard chemistry tool. It then builds a 3D page around it: pick a molecule such as benzene or water, drag a slider to pull it apart, and watch the electrons regroup as the bond breaks.

## Built for Sovereignty.

As organizations build AI around their knowledge and workflows, sovereignty means that they retain control of the models they depend on and the intelligence they create.

With Mistral Large 4, that control begins with the model itself. Organizations have the ability to inspect, customize, and deploy the model on their own infrastructure. They can use and improve Mistral Large 4 without depending on Mistral to serve it through an API. Mistral Large 4 was developed and trained from scratch in Europe, giving organizations clarity about the origins of the technology they’re building on.

Control also extends to how AI operates inside an organization. Organizations can customize Mistral Large 4 around their proprietary data with Mistral [Forge](https://mistral.ai/products/forge/), and they can build, govern, and improve the resulting agents and applications using Mistral [Studio.](https://mistral.ai/products/studio/) Mistral’s Applied AI experts can work alongside customer teams to build and deploy these tailored systems. Through that work, teams understand the architecture and decisions behind what they’ve built, along with the methods used to train and operate it, so they can run and improve their AI independently.

Sovereignty does not always require self-hosting; sometimes it just requires deployments that meet your control requirements. For organizations that prefer a managed service, Mistral Large 4 is available through Microsoft Foundry, with private deployment options for Azure developers.

Microsoft and Mistral AI share a commitment to expanding customer choice in next-generation AI. As the first cloud provider to bring Mistral Large 4 to customers, Microsoft is making Mistral’s latest open-weight model available through Microsoft Foundry, an enterprise AI platform built for evaluating, deploying, and operating models at scale. For organizations advancing sovereign AI strategies, this combination offers greater flexibility and control across their data, infrastructure, and AI operations.

Tina Schuchman, Corporate Vice President, Foundry, Microsoft

## Start building with Le Chonk

Mistral Large 4 is available to try, deploy, and customize today.

Get started in [Mistral Studio](https://console.mistral.ai/home), through the Mistral [API](https://docs.mistral.ai/models/mistral-large-4-0#use-mistral-large-4-0), or on [OpenRouter](https://openrouter.ai/mistralai/mistral-large-4-0). For full control, download the open weights from [Hugging Face](https://huggingface.co/mistralai/Mistral-Large-4-1T-A52B) and deploy on your own infrastructure with vLLM or SGLang. The [model card](https://docs.mistral.ai/models/mistral-large-4-0) and [license](https://huggingface.co/mistralai/Mistral-Large-4-1T-A52B/blob/main/LICENSE) have everything you need to get started.

You can also try the new model in [Vibe Code](https://chat.mistral.ai/code?model=mistral-large-4-0); give it a feature to build or a bug to fix in your own repository.

Mistral Large 4, is also available through [NVIDIA-hosted endpoints](https://build.nvidia.com/mistralai/mistral-large-4-1t-a52b), as an optimized [NVIDIA NIM container](https://catalog.ngc.nvidia.com/orgs/nim/nvidia/containers/vllm-model-free-nim-experimental/-) for serving with vLLM. Developers can use the open source [NeMo AutoModel library](https://github.com/NVIDIA-NeMo/Automodel/blob/main/docs/model-coverage/vlm/mistralai/mistral-large-4.mdx) for customization or the open source [TensorRT LLM](https://github.com/NVIDIA/TensorRT-LLM/tree/feat/mistral-large-4) library for high-throughput serving.

Enterprises can also tailor Mistral Large 4 to their proprietary data, use cases, and guardrails with Mistral Forge. [Talk to our team](https://mistral.ai/contact/) to learn more.

The model is available to use under the [*Mistral Ecosystem Open-Weight license*](https://huggingface.co/mistralai/Mistral-Large-4-1T-A52B/blob/main/LICENSE)*,* a modified MIT license. The model is free to use, modify, fine-tune, and commercialize, with a single condition: businesses that operate a Model-as-a-Service offering must obtain a separate commercial license from Mistral before using the model commercially.

Starting today, Mistral Large 4 is priced at ***$0.70 input*** / M, ***$0.07 cached input*** / M, and ***$2.10 output*** / M tokens.

We encourage the community to put Mistral Large 4 to the test. Run your own evaluations, push its limits, and share your findings with us on [X](https://x.com/MistralAI?lang=en), [Discord](https://discord.com/invite/mistralai), or [GitHub](https://github.com/mistralai).

## What's Next

Mistral Large 4 will serve as the foundation for a new generation of specialized and optimized Mistral models. Building on recent releases such as [Voxtral](https://mistral.ai/news/voxtral-3/) and [Timestral](https://mistral.ai/news/timestral/), we'll continue expanding the frontier of open-weight AI, continuing to give developers and enterprises greater freedom to build, customize, and own their intelligence.
