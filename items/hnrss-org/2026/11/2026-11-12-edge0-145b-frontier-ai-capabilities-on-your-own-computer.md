---
title: 'Edge0-145B: Frontier AI Capabilities on Your Own Computer'
link: https://edge0.ai/blog/introducing-edge0-145b-frontier-ai-capabilities-on-your-own-computer
source: hnrss-org
published: 2026-11-12T23:06:17Z
updated: 2026-11-12T23:06:17Z
first_seen: 2026-11-13T07:09:29.179831632Z
authors:
- sroussey
content: extracted
html: 2026-11-12-edge0-145b-frontier-ai-capabilities-on-your-own-computer.html
remote_preview:
  url: https://cdn.hashnode.com/uploads/covers/6aea03219fa4aa99923b5acf/de5ebdea-3436-4b9a-b3f2-e9910f3a0abc.png
---

Today, we're open-sourcing **Edge0-145B and Edge0-120B**, together with our unified inference framework and deployment tools. Edge0-145B is the world's first open-source on-device AI model with 145 billion parameters. It brings coding, reasoning, and agent capabilities that rival frontier cloud models to your own computer.

On-device AI has long forced a tradeoff between model capability and what the hardware can run. Larger models demand more memory, bandwidth, and power; smaller models can struggle with complex work. Moving that work to the cloud adds model-provider fees, data-handling constraints, and a dependence on connectivity.

This release addresses those constraints in four ways:

| **The constraint**                                                                         | **What Edge0-145B makes possible**                                                                                                                                                           |
| ------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Large models are difficult to run on personal hardware.                                    | A 145B model runs locally on a compatible laptop, with reported peak memory of 29GB and decoding above 80 tokens/s.                                                                          |
| Small on-device models can fall short on demanding tasks.                                  | Coding, reasoning, and instruction-following results are competitive with specific frontier cloud models, as the comparisons below show.                                                     |
| A useful assistant needs to finish work, not just answer questions.                        | The model plans, writes and runs code, debugs errors, and checks results visually for projects such as 3D scenes, games, hardware programs, and PCB designs.                                 |
| Cloud workflows bring recurring fees, sensitive-data restrictions, and network dependence. | Local inference incurs no corresponding cloud-model token or API fees; data can stay on the computer, and core capabilities keep working offline. Hardware and electricity still cost money. |

We're releasing the full stack so developers can reproduce the results, evaluate or fine-tune the models, and integrate them into their own products.

Get the [model weights](https://huggingface.co/Edge0), explore the [inference framework](https://github.com/Edge0-AI/Edge0), or try the [Edge0 App](https://github.com/Edge0-AI/Edge0/releases/tag/v0.2.0).

## How We Make 100B+ Parameter AI Run on a Personal Computer

### Edge0-145B Laptop Performance: Memory, Speed, and Power

Edge0-145B makes a 145-billion-parameter model usable on a personal laptop. A useful local model has to fit in memory, process prompts promptly, generate fast enough to use, and keep power consumption under control. It also needs to sustain longer tasks.

| Requirement       | Reported Edge0-145B result           | What it enables                                                                      |
| ----------------- | ------------------------------------ | ------------------------------------------------------------------------------------ |
| Memory            | **29GB peak memory**                 | Runs on a compatible personal laptop, such as a MacBook Pro, without a server.       |
| Prompt processing | **1,100+ tokens/s prefill**          | Processes long prompts, including dozens of pages of code, before generation begins. |
| Generation        | **80+ tokens/s decode**              | Generates faster than a person reads.                                                |
| Power             | **Approximately 120W typical power** | Sustains operation without throttling in the reported setup.                         |
| Long tasks        | **128K–256K context**                | Maintains essentially the same speed on long inputs, supporting extended work.       |

Our earlier Edge0-35B phone release reported peak memory below 2.5GB. On an iPhone 18 Pro Max, Edge0-35B delivers 37+ tokens/s using approximately 2.5GB of memory. Edge0-145B has roughly four times as many parameters; its laptop comparison reports 80+ tokens/s and 28GB peak memory. These measurements apply to different models and hardware.

![Image](https://cdn.hashnode.com/uploads/gql/6aea03219fa4aa99923b5acf/aeeae022-73c9-4d31-847a-d495a5af3200.png)

*On-device model scale over time. The chart covers 67 open models of at least 1B parameters from 14 labs, intended for phones, laptops, and AI PCs. Same-size base, instruct, and reasoning variants count once; each line tracks a lab's largest model, including releases before July 2025. Filled points mark new highs and hollow points other releases. The scale is logarithmic below 40B and linear above it. Apple is excluded because its weights are not released. Sources: vendor announcements and model cards.*

### Why On-Device Models Have Stayed Small

Memory limits kept most on-device models below 10B parameters before September 2026. Apple Intelligence's on-device foundation model had approximately 3B parameters, while Google's Gemini Nano came in 1.8B and 3.25B versions. Even a 7B model could consume approximately 4GB of memory.

Larger models faced a memory wall. Model weights occupy storage and must be read during generation, competing for limited memory and bandwidth with the operating system and other apps. Even an aggressively compressed 35B model could need approximately 20GB of memory, far more than a phone could make available.

Compute, bandwidth, and power created competing constraints. Larger models offered more capability, but strained memory capacity, generation speed, and battery life. Making models smaller helped them fit, often at the cost of capability.

Privacy alone could not close that gap. Apple Intelligence protected user data, but Apple acknowledged that its on-device model was not designed for agentic applications requiring general world knowledge. The prevailing assumption was that on-device intelligence could not match the cloud.

### Under the Hood of Edge0: Four Layers of Model and Inference Design

We designed Edge0-145B and its inference engine together to reduce the memory and data movement that make large models difficult to run locally. The design spans architecture, training, loading, and scheduling.

**Architecture: share large matrices across sparse experts.** Sharing matrices substantially reduces the weights that must be stored and moved while keeping the expert count unchanged. Each group of 64 experts shares two large matrices, while each expert retains a small set of parameters that captures its differences. Previously, every expert stored a complete set of matrices. Of the model's 145B total parameters, only approximately 10B participate in computation on each inference step.

**Training: build for local execution from day one.** We train the model in its on-device architecture from the outset, with deployment constraints built into its design. This avoids treating local deployment as a compression or pruning step after training a cloud model.

**Loading: pipeline GPU memory, system memory, and SSD access.** Reading and computation overlap across three storage tiers, avoiding serial waits. Frequently used “hot experts” remain in GPU memory. An expert library of approximately 40GB sits in system memory, with experts brought into GPU memory as needed. Approximately 48GB of vocabulary data resides on the SSD and is read on demand.

**Scheduling: use the CPU and GPU together.** The GPU computes cached experts while the CPU handles the remaining work in parallel. Speculative decoding verifies multiple candidate tokens at once, spreading the cost of reading weights across more output tokens. These choices support the reported 80+ tokens/s decode rate.

![Image](https://cdn.hashnode.com/uploads/gql/6aea03219fa4aa99923b5acf/ad8efc79-2b69-42a0-8ed2-cf582b7c0b58.png)

*Model and inference-engine design work together to make 100B+ parameter inference practical on personal computers. Source: internal testing.*

Together, these changes let parameter counts grow from tens of billions to more than a hundred billion without proportional increases in memory use, power consumption, and latency.

## What Edge0 Can Do: Anything From Chat to Complex Work on Your Computer

Edge0-145B can carry a complex task through to a usable result. Its coding and reasoning capabilities support local workflows that combine planning, tool use, execution, debugging, and visual checks. The benchmarks establish its capabilities; the demonstrations show how it puts them to work.

### Coding and reasoning that rival frontier cloud models

Edge0-145B is competitive with specific frontier cloud models across coding, reasoning, knowledge, and instruction following:

- **Coding:** It scores **64.6 on DeepSWE 1.1**, ahead of Qwen3.8-Flash-Next at 58.7 and DeepSeek-V4-Flash-0731 at 54.4.
- **Reasoning and knowledge:** It scores **90.4 on GPQA-Diamond**, close to DeepSeek-V4-Flash-0731 at 90.8 and Claude-Opus-4.6 \\(Max\\) at 91.3.
- **Instruction following:** It scores **80.6 on IFBench**, ahead of DeepSeek-V4-Flash-0731 at 79.2 and Claude-Opus-4.6 \\(Max\\) at 62.5.

Its **86.52 on Terminal-Bench 2.1** also exceeds Qwen3.8-Flash-Next's 83.3, DeepSeek-V4-Flash-0731's 82.7, and Claude's 78.2. On AA-LCR, Edge0-145B scores 79.3 to Claude's 78.0. These comparisons apply to the named models and evaluations.

| Model                     | DeepSWE 1.1 | Terminal-Bench 2.1 | HLE      | GPQA-Diamond | IFBench  | AA-LCR   |
| ------------------------- | ----------- | ------------------ | -------- | ------------ | -------- | -------- |
| **Edge0-145B**            | **64.6**    | **86.52**          | **35.4** | **90.4**     | **80.6** | **79.3** |
| Edge0-120B                | 53.6        | 81.3               | 31.0     | 87.4         | 79.3     | 81.0     |
| Qwen3.8-Flash-Next        | 58.7        | 83.3               | 35.9     | 91.7         | 81.3     | 79.1     |
| Qwen3.8-27B               | 42.2        | 73.0               | 30.8     | 89.2         | 79.5     | 82.0     |
| DeepSeek-V4-Flash-0731    | 54.4        | 82.7               | 33.8     | 90.8         | 79.2     | 79.7     |
| Flint 40B                 | —           | 54.9               | —        | —            | 89.2     | —        |
| Muse Glimmer              | —           | 51.7               | 22.0     | 83.5         | 77.0     | 83.3     |
| Gemma 4                   | —           | 43.4               | 22.7     | 85.7         | 76.0     | 62.0     |
| Claude-Opus-4.6 \\(Max\\) | —           | 78.2               | 40.0     | 91.3         | 62.5     | 78.0     |

*Scores are percentages; higher is better. A dash means no public score is available, rather than a score of zero.*

Edge0, Qwen, DeepSeek, and Claude results come from Edge0's internal evaluations. Claude-Opus-4.6 has no public DeepSWE 1.1 score. For Flint, Muse Glimmer, and Gemma 4, Terminal-Bench 2.1 and IFBench results come from the Recursive Flint evaluation; HLE, GPQA, and AA-LCR results come from Artificial Analysis.

The largest gains over Qwen3.8-27B, a mainstream on-device model baseline, are in agent and coding work: **18.5% on Terminal-Bench and 53.1% on DeepSWE**. The relative gains on HLE, GPQA, and IFBench are 14.9%, 1.3%, and 1.4%, respectively.

DeepSWE 1.1 and Terminal-Bench 2.1 evaluate agent and coding work. HLE, GPQA-Diamond, and IFBench cover reasoning, knowledge, and instruction following. The complete evaluation setup, hardware environment, and reproduction instructions are being released with our technical report.

![Image](https://cdn.hashnode.com/uploads/gql/6aea03219fa4aa99923b5acf/6eef0e15-dc4b-4e23-ac01-3ab5daf5287e.png)

### Competitive capability with a smaller memory footprint

Edge0-145B combines a **72.8 mean score across six benchmarks with 29GB measured peak memory**. Qwen3.8-Flash-Next averages 71.7, with an estimated 70GB footprint for standard 4-bit deployment. That puts Edge0-145B at about 40% of the estimated baseline memory while delivering comparable or higher capability, enabling local use on a 48GB laptop.

| Model                  | Mean across six benchmarks | Memory footprint \\(measured or estimated\\) |
| ---------------------- | -------------------------- | -------------------------------------------- |
| **Edge0-145B**         | **72.8**                   | **29GB**                                     |
| Edge0-120B             | 68.9                       | 29GB                                         |
| Qwen3.8-27B            | 66.1                       | Approximately 18GB\*                         |
| Qwen3.8-Flash-Next     | 71.7                       | Approximately 70GB\*                         |
| DeepSeek-V4-Flash-0731 | 70.1                       | Approximately 87GB\*                         |

The means cover DeepSWE 1.1, Terminal-Bench 2.1, HLE, GPQA-Diamond, IFBench, and AA-LCR. Edge0 memory figures are measured internal peaks. Asterisked baseline figures are estimates for standard 4-bit deployment and remain to be measured. Claude is excluded because it is cloud-only and has no DeepSWE score.

![Image](https://cdn.hashnode.com/uploads/gql/6aea03219fa4aa99923b5acf/72bc6257-5a9e-4eca-b73c-e4b9c274bf81.png)

\[Figure: Quality versus peak memory, with 48GB laptop and 64GB workstation capacities marked. Lower memory and higher scores are better; the chart's vertical axis does not start at zero.\]

### Local agents that plan, code, run, and check their work

A single natural-language request can start an entire local workflow. Edge0-145B breaks down the requirements, plans the steps, writes code, and calls tools to run it. When something fails, it debugs and repairs the code. It then takes screenshots to inspect the result and keeps iterating until the work is complete.

These projects often take dozens of rounds of planning, execution, and review. Local inference means those additional conversations, code revisions, and visual analyses incur no corresponding cloud-model token or API charges. Code and data can stay on the computer, and core model capabilities work offline.

### Video production: generating Tesla's Roadster explainer within an hour

Edge0-145B produced an explainer about how Roadster could fly, handling technical research, scriptwriting, asset generation, and editing. We asked it to make the principles understandable to a general audience. The model ran locally without cloud-model calls or cloud token consumption, avoiding network round trips for inference.

The video was generated within an hour. Producing comparable work manually would have taken a director and scriptwriter, a 3D modeler, an editor, and a physics expert a month. An AI video workflow could previously cost more than hundreds of dollars; local inference eliminates the corresponding cloud-model token and API fees.

[YouTube video](https://www.youtube.com/watch?v=TefX0rfGNkk)

### 3D modeling: a Manhattan city scene in Blender

Edge0-145B built a 3D scene of Manhattan within an hour. Working in Blender from text and reference images, it constructed the scene, adjusted materials and lighting, and iterated on the rendered output.

The workflow brings visual inspection, code generation, and revision together inside Blender. The model works with objects, spatial relationships, and the creative goal, going beyond the pixels in a reference image. It inspects the result, writes code, and makes changes without handing those steps back to a person. The entire process stays local.

Creating a Manhattan-scale city scene manually would typically cost approximately **$12,000-18,000** and take at least a month. A leading cloud model would still incur more than $150 in cloud token charges. Edge0-145B completes the local inference without those cloud-model token fees.

[YouTube video](https://www.youtube.com/watch?v=YR9KeFT1p68)

### Game development: playable games and a medieval city

Edge0-145B can create playable games locally. We also used its 3D modeling capabilities to build an original medieval capital for a fantasy RPG and render a **20-second fly-through** at game-trailer quality.

The model planned the city independently and built it from scratch in Blender: city walls, more than a thousand old-town homes, a plaza fountain, a Gothic twin-tower facade, a rib-vaulted nave, and stained-glass windows.

The fixed camera path made errors visible. Geometry clipping, occlusion, or incorrect proportions would appear along the route, so the model inspected the renders and corrected its work as it built. The workflow ran entirely locally.

A leading cloud model required hundreds of tool calls and hundreds of millions of tokens for a single completion, using the same prompt, reference images, and camera route. Edge0-145B delivered it on a laptop without cloud-model token or API fees for local inference. An art team would typically spend weeks and thousands of dollars producing a city scene to this standard.

[YouTube video](https://www.youtube.com/watch?v=6Rfuh7lBiqY)

### Hardware control: code that flies a real drone

Edge0-145B wrote the program for a real drone from one instruction: “Write a drone program that navigates through obstacles and finds a red backpack.”

It generated the flight-control and visual-recognition programs locally. The drone then took off indoors, navigated through a dense row of obstacle poles, searched autonomously, and located the red backpack beside a wall. The task required no human-written code and no cloud call.

Hardware developers can describe the behavior they want and have the model turn it into a program for a real device, without moving repeatedly between an editor, documentation, and debugging tools.

### PCB design: from KiCad layout to an assembly animation

Edge0-145B took a PCB design from component placement through an assembly visualization. We asked it to design a board for itself. It started from scratch in KiCad, placed components, routed a two-layer board, and imported the design into Unreal Engine 5. It then set up cameras and lighting and rendered an animation of PCB fabrication and assembly.

A single computer delivered work that previously required a hardware engineer and a 3D designer to hand tasks back and forth over several days. Schematics and board layouts can stay on the team's own hardware, supporting trade-secret protection and data-handling requirements without uploading the designs to a cloud model.

## Why Edge0: A Full Stack for On-Device AI

Useful on-device AI requires model development, inference engineering, and hardware adaptation to work together. Our progress rests on three capabilities: developing complementary models quickly, optimizing the full stack, and carrying lessons from each deployment into the next.

Our September release of Edge0-35B showed what that approach could deliver on a phone. Its launch post passed 1.2 million views on its first day without paid promotion in overseas communities. Researchers and technical experts at OpenAI, NVIDIA, Qualcomm, and other companies shared it, and the model reached No. 1 on Hugging Face's overall Trending chart.

We released the model weights and unified inference framework together, giving companies and researchers a standard approach they could reproduce. The release helped move the discussion of on-device AI from a few billion parameters to the 35B tier.

### Four Model Families for On-Device AI

Over the past year, we released 200 models across four families. Each handles a different part of the work, from listening and speaking to fast decisions, memory, and complex agent tasks.

| Model family   | Role                                | Models and capabilities                                                                                                                                                       |
| -------------- | ----------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Edge0**      | Complex agent tasks and vibe coding | 145B and 120B models for laptops; 35B and 8B models for phones. The laptop models support vibe coding, while the phone models support agent tasks.                            |
| **Audio8**     | Listening and speaking              | ASR-Infinite and 3B, 0.6B, and 0.1B ASR models for speech recognition; 0.6B and 0.1B TTS models for speech synthesis; and a 0.1B Chat model for end-to-end voice interaction. |
| **Decision0**  | Fast decisions                      | 0.6B and 3B models covering text, speech, and vision, with multimodal decisions in approximately 40 milliseconds.                                                             |
| **Embedding0** | Memory                              | A 0.6B multimodal embedding model for text, speech, images, video, and files, supporting local long-term memory and retrieval-augmented generation \\(RAG\\).                 |

![Image](https://cdn.hashnode.com/uploads/gql/6aea03219fa4aa99923b5acf/ad51cd8b-07a0-4b4c-9267-c0c33f2d9698.png)

The portfolio spans tens of millions to more than a hundred billion parameters. Both Edge0 and Audio8 have reached No. 1 on Hugging Face's Trending chart.

### Models and Inference Systems Designed Together

We develop the model and the inference framework together, building hardware constraints into training and optimizing the engine for different chips. This connects model capability with the software and hardware needed to use it locally.

That work requires expertise in model development, inference frameworks, and low-level hardware adaptation. The combination is difficult for a company focused only on model development to reproduce.

### Hardware Optimizations That Carry into Each Deployment

Each deployment gives us reusable optimizations for the next chip, model, or product. This matters because on-device hardware is fragmented: NPU architectures differ, as does support for numerical formats such as INT4, INT8, and FP4. A general-purpose model cannot automatically make full use of every chip.

We adapt to individual platforms. Our portfolio now covers 95% of mainstream chip platforms, including Qualcomm, BES, Rockchip, Allwinner, D-Robotics, and NVIDIA, across headphones, glasses, phones, PCs, and robots. This is portfolio-wide coverage; model requirements vary, and it does not mean every device can run Edge0-145B.

That deployment work now spans more than 5 million production devices. Our open-source models passed 15 million downloads within three months. By October 2026, we had more than 110,000 customer organizations.

The portfolio figures come from company papers and internal testing. They describe the broader Edge0 portfolio; the 145B performance results in this article apply specifically to Edge0-145B.

For Edge0-145B, day-one support covers macOS, Windows, and Linux, including Apple M-series chips and NVIDIA platforms such as RTX Spark, RTX 4090, and RTX 5090. The release includes one-click deployment tools and hardware guides.

## The Future of On-Device AI and the Cloud

More complex work can stay on the device as local models approach cloud-level capability. Privacy, cost control, and offline access have long favored local execution; the remaining question was whether an on-device model could do the work. Edge0-145B expands the range of tasks for which it can.

### Why More AI Work Can Stay Local

Local execution addresses four practical obstacles to adopting cloud-only AI:

- **Control cloud-model costs.** Local inference adds no corresponding cloud-model token or API fees as usage grows.
- **Keep sensitive data local.** Code, documents, and business records can stay on the hardware, giving teams control over data handling.
- **Sustain a responsive experience.** Model and inference-engine design address response speed, power use, system responsiveness, and continuous operation together.
- **Keep core capabilities available offline.** Local tasks continue when connectivity drops; features that depend on online information or services still need a connection.

### From Cloud First to More Complex Work On-Device

We see the division of work between devices and the cloud shifting through three stages.

**Before September 2026: cloud first, with on-device support.** Apple Intelligence reflected this stage: it protected privacy, but handled a limited range of tasks locally while more demanding work relied on cloud capabilities.

**After Edge0-35B's September 10 release: everyday tasks locally, complex tasks in the cloud.** Local models could take on frequent requests that needed immediate responses. For one leading headphone manufacturer, our hybrid solution now handles 92% of AI usage on-device and 8% in the cloud. Everyday conversation accounts for most use cases on consumer wearables.

**With Edge0-145B: toward on-device first, with cloud support.** Vibe coding and other complex tasks can now run on a personal computer, giving teams more scope to choose local execution. This extends the range of work that can stay on the device while preserving the cloud for tasks and hardware that need it.

![Image](https://cdn.hashnode.com/uploads/gql/6aea03219fa4aa99923b5acf/e20c5a72-6ca4-4c8e-b96a-bf89393b7eda.png)

*The diagram shows the shift from cloud-first AI to everyday local tasks and then more complex local work. Its colored areas are schematic, not measured usage shares.*

### Where Cloud AI Still Fits

Hardware constraints still determine which model fits. Compute capacity, heat, and battery life matter: Edge0-35B runs on phones, while Edge0-145B runs on suitably equipped laptops. A very low-compute device cannot currently run a 145B model locally. It needs a smaller model suited to its hardware or cloud access to a model of that size.

Cross-device long-term memory and searches requiring highly current information still require cloud services. Local and cloud models can work together; the appropriate split depends on the task, the hardware, and the information it needs.

The aim is to make local execution a practical choice for demanding work, with a capable model that can keep working close to the person using it. That is the next step toward our goal of enabling intelligence at zero distance for everybody.

## Get Started with Edge0

Download the models to run locally, evaluate or fine-tune them, and build them into your own products:

- **Model weights:** [Edge0-145B](https://huggingface.co/Edge0/Edge0-145B-NVFP4) and [Edge0-120B](https://huggingface.co/Edge0/Edge0-120B-NVFP4) on Hugging Face.
- **Inference framework and deployment guides:** [Edge0 on GitHub](https://github.com/Edge0-AI/Edge0).
- **Try it without environment setup:** [Download the Edge0 App](https://github.com/Edge0-AI/Edge0/releases/tag/v0.2.0). Its core model capabilities work offline, without an account or usage quota.
- **Community:** [Join us on Discord](https://discord.com/invite/Tg3W2FEuQ).

For hardware or product integrations, reach the Edge0 team through GitHub or [contact@autoark.ai](mailto:contact@autoark.ai).
