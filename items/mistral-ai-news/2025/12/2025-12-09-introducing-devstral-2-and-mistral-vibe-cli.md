---
title: 'Introducing: Devstral 2 and Mistral Vibe CLI.'
link: https://mistral.ai/news/devstral-2-vibe-cli/
source: mistral-ai-news
published: 2025-12-09T12:00:00Z
updated: 2025-12-09T12:00:00Z
first_seen: 2026-09-19T21:30:23.395188495Z
content: extracted
html: 2025-12-09-introducing-devstral-2-and-mistral-vibe-cli.html
preview:
  file: 2025-12-09-introducing-devstral-2-and-mistral-vibe-cli.preview-8be9edef61c6.webp
  width: 256
  height: 153
  color: '#5694c1'
images:
- source: https://mistral.ai/cms-media/api/media/file/Thumbnail-Model-Devstral.jpg
  original:
    file: 2025-12-09-introducing-devstral-2-and-mistral-vibe-cli.image-ca4cb3ff4d1c.jpg
    width: 1800
    height: 1074
  color: '#0287e8'
- source: https://mistral.ai/_astro/9c36eef1-2b4c-4fb8-8ef0-d531116ec53a_1P2Y1J.webp?dpl=6aad049eaf4c2d00095b91e5
  original:
    file: 2025-12-09-introducing-devstral-2-and-mistral-vibe-cli.image-fce00afcc9bd.webp
    width: 1686
    height: 1093
  variants:
  - file: 2025-12-09-introducing-devstral-2-and-mistral-vibe-cli.image-594f6a098b9f.webp
    width: 320
    height: 207
  - file: 2025-12-09-introducing-devstral-2-and-mistral-vibe-cli.image-ab876b5f5844.webp
    width: 640
    height: 415
  color: '#fdf9ea'
- source: https://mistral.ai/_astro/49e0d71c-436c-4334-9fff-fa68c9f60380_1S4vKf.webp?dpl=6aad049eaf4c2d00095b91e5
  original:
    file: 2025-12-09-introducing-devstral-2-and-mistral-vibe-cli.image-28e136530a2f.webp
    width: 1686
    height: 969
  variants:
  - file: 2025-12-09-introducing-devstral-2-and-mistral-vibe-cli.image-fbfe782773a8.webp
    width: 320
    height: 184
  - file: 2025-12-09-introducing-devstral-2-and-mistral-vibe-cli.image-21c70cda87ea.webp
    width: 640
    height: 368
  color: '#fefaeb'
- source: https://mistral.ai/_astro/48b2b0fc-f8d8-44da-a3a2-4961aad2f10e_1htA3b.webp?dpl=6aad049eaf4c2d00095b91e5
  original:
    file: 2025-12-09-introducing-devstral-2-and-mistral-vibe-cli.image-13522ff790b5.webp
    width: 1371
    height: 670
  variants:
  - file: 2025-12-09-introducing-devstral-2-and-mistral-vibe-cli.image-9c5f407e3cf4.webp
    width: 320
    height: 156
  - file: 2025-12-09-introducing-devstral-2-and-mistral-vibe-cli.image-12c958c5d320.webp
    width: 640
    height: 313
  color: '#fef9ea'
---

Today, we're releasing Devstral 2—our next-generation coding model family available in two sizes: Devstral 2 (123B) and Devstral Small 2 (24B). Devstral 2 ships under a modified MIT license, while Devstral Small 2 uses Apache 2.0. Both are open-source and permissively licensed to accelerate distributed intelligence.

Devstral 2 is currently free to use via [our API](https://console.mistral.ai/).

We are also introducing Mistral Vibe, a native CLI built for Devstral that enables end-to-end code automation.

## Highlights.

1. Devstral 2: SOTA open model for code agents with a fraction of the parameters of its competitors and achieving 72.2% on SWE-bench Verified.

2. Up to 7x more cost-efficient than Claude Sonnet at real-world tasks.

3. Mistral Vibe CLI: Native, open-source agent in your terminal solving software engineering tasks autonomously.

4. Devstral Small 2: 24B parameter model available via API or deployable locally on consumer hardware.

5. Compatible with on-prem deployment and custom fine-tuning.

## Devstral: the next generation of SOTA coding.

Devstral 2 is a 123B-parameter dense transformer supporting a 256K context window. It reaches 72.2% on SWE-bench Verified—establishing it as one of the best open-weight models while remaining highly cost efficient. Released under a modified MIT license, Devstral sets the open state-of-the-art for code agents.

Devstral Small 2 scores 68.0% on SWE-bench Verified, and places firmly among models up to five times its size while being capable of running locally on consumer hardware.

![Devstral   Swe Bench Verified  Open Weights Vs Proprietary Models (light) (1)](https://mistral.ai/_astro/9c36eef1-2b4c-4fb8-8ef0-d531116ec53a_1P2Y1J.webp?dpl=6aad049eaf4c2d00095b91e5)

Devstral 2 (123B) and Devstral Small 2 (24B) are 5x and 28x smaller than DeepSeek V3.2, and 8x and 41x smaller than Kimi K2—proving that compact models can match or exceed the performance of much larger competitors. Their reduced size makes deployment practical on limited hardware, lowering barriers for developers, small businesses, and hobbyists.hardware.

![Devstral   Swe Bench Verified Regular Performance X Modelsize (light)](https://mistral.ai/_astro/49e0d71c-436c-4334-9fff-fa68c9f60380_1S4vKf.webp?dpl=6aad049eaf4c2d00095b91e5)

### Built for production-grade workflows.

Devstral 2 supports exploring codebases and orchestrating changes across multiple files while maintaining architecture-level context. It tracks framework dependencies, detects failures, and retries with corrections—solving challenges like bug fixing and modernizing legacy systems.

The model can be fine-tuned to prioritize specific languages or optimize for large enterprise codebases.

We evaluated Devstral 2 against DeepSeek V3.2 and Claude Sonnet 4.5 using human evaluations conducted by an independent annotation provider, with tasks scaffolded through Cline. Devstral 2 shows a clear advantage over DeepSeek V3.2, with a 42.8% win rate versus 28.6% loss rate. However, Claude Sonnet 4.5 remains significantly preferred, indicating a gap with closed-source models persists.

![Devstral   Model Performance Comparison (light) (1)](https://mistral.ai/_astro/48b2b0fc-f8d8-44da-a3a2-4961aad2f10e_1htA3b.webp?dpl=6aad049eaf4c2d00095b91e5)

“Devstral 2 is at the frontier of open-source coding models. In Cline, it delivers a tool-calling success rate on par with the best closed models; it's a remarkably smooth driver. This is a massive contribution to the open-source ecosystem.” — Cline.

“Devstral 2 was one of our most successful stealth launches yet, surpassing 17B tokens in the first 24 hours. Mistral AI is moving at Kilo Speed with a cost-efficient model that truly works at scale.” — Kilo Code.

Devstral Small 2, a 24B-parameter model with the same 256K context window and released under Apache 2.0, brings these capabilities to a compact, locally deployable form. Its size enables fast inference, tight feedback loops, and easy customization—with fully private, on-device runtime. It also supports image inputs, and can power multimodal agents.

## Mistral Vibe CLI.

Mistral Vibe CLI is an open-source command-line coding assistant powered by Devstral. It explores, modifies, and executes changes across your codebase using natural language—in your terminal or integrated into your preferred IDE via the Agent Communication Protocol. It is released under the Apache 2.0 license.

Vibe CLI provides an interactive chat interface with tools for file manipulation, code searching, version control, and command execution. Key features:

- Project-aware context: Automatically scans your file structure and Git status to provide relevant context

- Smart references: Reference files with @ autocomplete, execute shell commands with !, and use slash commands for configuration changes

- Multi-file orchestration: Understands your entire codebase—not just the file you're editing—enabling architecture-level reasoning that can halve your PR cycle time

- Persistent history, autocompletion, and customizable themes.

You can run Vibe CLI programmatically for scripting, toggle auto-approval for tool execution, configure local models and providers through a simple config.toml, and control tool permissions to match your workflow.

## Get started.

Devstral 2 is currently offered free via [our API](http://console.mistral.ai). After the free period, the API pricing will be $0.40/$2.00 per million tokens (input/output) for Devstral 2 and $0.10/$0.30 for Devstral Small 2.

We’ve partnered with leading, open agent tools [Kilo Code](https://kilo.ai/) and [Cline](https://cline.bot/) to bring Devstral 2 to where you already build.

Mistral Vibe CLI is available as an extension in [Zed](https://zed.dev/extensions), so you can use it directly inside your IDE.

### Recommended deployment for Devstral.

Devstral 2 is optimized for data center GPUs and requires a minimum of 4 H100-class GPUs for deployment. You can try it today on [build.nvidia.com](http://build.nvidia.com). Devstral Small 2 is built for single-GPU operation and runs across a broad range of NVIDIA systems, including DGX Spark and GeForce RTX. NVIDIA NIM support will be available soon.

Devstral Small runs on consumer-grade GPUs as well as CPU-only configurations with no dedicated GPU required.

For optimal performance, we recommend a temperature of 0.2 and following the best practices defined for [Mistral Vibe CLI](https://github.com/mistralai/mistral-vibe/blob/main/README.md).

## Contact us.

We’re excited to see what you will build with Devstral 2, Devstral Small 2, and Vibe CLI!

Share your projects, questions, or discoveries with us on [X/Twitter](https://x.com/mistralai), [Discord](https://discord.com/invite/mistralai), or [GitHub](https://github.com/mistralai).

## We’re hiring!

If you’re interested in shaping open-source research and building world-class interfaces that bring truly open, frontier AI to users, we welcome you to [apply to join our team](https://mistral.ai/careers).
