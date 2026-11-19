---
title: 'DaisyRose-27B: 1,200-task RL took Qwen3.8 from 5.6% to 12% on Terminal-Bench 4.1'
link: https://www.radixark.com/blog/daisyrose-27b
source: hnrss-org
published: 2026-11-18T19:39:16Z
updated: 2026-11-18T19:39:16Z
first_seen: 2026-11-19T00:14:19.077761094Z
authors:
- de_sty
content: extracted
html: 2026-11-18-daisyrose-27b-1-200-task-rl-took-qwen3-8-from-5-6-to-12-on.html
remote_preview:
  url: https://www.radixark.com/images/daisyrose-27b/hero.png
  alt: 'DaisyRose-27B: RadixArk’s first model'
---

![DaisyRose-27B: a daisy and a rose carrying the model name.](https://www.radixark.com/_next/image?url=%2Fimages%2Fdaisyrose-27b%2Fhero.png&w=3840&q=75&dpl=dpl_2WZt4617DGLJ3EDD7HwJmGV2VG4H)

Today we are releasing **DaisyRose-27B**, RadixArk’s first model, built on Qwen3.8-27B and trained with reinforcement learning using [Miles](https://miles.radixark.com/). We trained it on long-horizon agent tasks with sessions up to 1M tokens, with just 1,200 curated tasks in 18 days on 144 H200 GPUs. DaisyRose-27B improves Terminal-Bench 4.1 pass@1 from 5.6% to 12.0%. Beyond Terminal-Bench 4.1, it also delivers gains on a variety of other reasoning, instruction following, and coding benchmarks.

We are releasing [**BF16**](https://huggingface.co/RadixArk/DaisyRose-27B)**,** [**FP8**](https://huggingface.co/RadixArk/DaisyRose-27B-FP8)**,** and [**NVFP4**](https://huggingface.co/RadixArk/DaisyRose-27B-NVFP4) checkpoints for everyone to try and build on. In this post, we explain how carefully designed tasks and efficient infrastructure made these gains possible on a limited compute budget.

[![DaisyRose-27B compared with eight models across seven benchmarks. DaisyRose scores 12.0 on Terminal Bench 4.1, 75.5 on Terminal Bench 2.1, 46.2 on DeepSWE v1.1, 94.7 on SWE Bench Pro v2, 34.3 on HLE (text only), 82.4 on IFBench, and 90.7 on GPQA Diamond.](https://www.radixark.com/_next/image?url=%2Fimages%2Fdaisyrose-27b%2Fbenchmarks.png&w=3840&q=75&dpl=dpl_2WZt4617DGLJ3EDD7HwJmGV2VG4H)](https://www.radixark.com/images/daisyrose-27b/benchmarks.png)

[![Benchmark comparison table with model sizes and scores. DaisyRose-27B has 27B activated and total parameters. Its Qwen3.8-27B baseline scores 5.6, 73.0, 42.2, 91.3, 33.9, 79.5, and 89.2 on the seven benchmarks, respectively.](https://www.radixark.com/_next/image?url=%2Fimages%2Fdaisyrose-27b%2Fbenchmarks-table-a3665b9d.png&w=3840&q=75&dpl=dpl_2WZt4617DGLJ3EDD7HwJmGV2VG4H)](https://www.radixark.com/images/daisyrose-27b/benchmarks-table-a3665b9d.png)

DaisyRose-27B scores higher than the original Qwen3.8-27B model on every benchmark above[^1]. The largest gain is on Terminal Bench 4.1, a notoriously hard long-horizon benchmark. Our magic lies in the following three key ingredients: high-quality training data, efficient training backend, and robust sandboxes.

## High-quality Training Data

We built the tasks in partnership with [Eigent AI](https://www.eigent.ai/), starting from Eigent AI’s large pool of tasks in machine learning, software engineering, science, and other fields. Together we built thousands of candidates. About 1,200 of them made it into the final training set. The training tasks are selected based on three criteria.

- **Each task targets a mistake the original model makes.** We ran the original Qwen3.8-27B on every training task and carefully redesigned each one so that it brings out the mistakes we want the model to stop making. We then tuned each task until its solve rate fell within a suitable range, neither too easy nor too hard for the model to learn from.
- **Every task is a long job with several problems chained together.** Each task entails a chain of interdependent problems, so an agent has to navigate through multiple stages of reasoning, planning, and execution. Each attempt gets up to 1.5 hours in the sandbox, with more than 100 tool-calls and more than 256k output tokens.
- **There is no room for reward hacking.** We reviewed every task along with the model's attempts at it. Any task that could be passed without really solving it, or that had a design flaw, was fixed or removed before training.

## Efficient training backend

RadixArk trained DaisyRose-27B with Miles, our open-source reinforcement learning framework for large-scale model post-training, in fully asynchronous mode: the agent keeps working while the model learns, and neither waits for the other. While 128 H200s run the inference on agent tasks, 16 more run the trainer and learn from the trajectories. After every step, the updated model goes out to the 128 GPUs that run the agent. The learning runs on our internal training backend, which is built for agent training: each step learns from a handful of very long sessions, some as long as 1M tokens, folded into multiple trajectories via harness summarization.

**57% lower training cost.** On this release, 37% of each GPU's peak compute went into useful training work (model FLOPs utilization, or MFU) on our backend. The open-source trainer, built on Megatron, reached 16% on comparable runs. So the same training needs 57% less GPU time. The gain is even larger on mixture-of-experts models: 6.2× on Qwen3.6-35B-A3B, and more than 10× on the roughly 300B-parameter GLM-5.3-Flash.

[![GPU efficiency: the internal backend reaches 37% versus 16% for the open-source trainer on Qwen3.8-27B, 16.1% versus 2.6% on Qwen3.6-35B-A3B, and 11.7% versus less than 1% on GLM-5.3-Flash.](https://www.radixark.com/_next/image?url=%2Fimages%2Fdaisyrose-27b%2Fgpu-efficiency.png&w=3840&q=75&dpl=dpl_2WZt4617DGLJ3EDD7HwJmGV2VG4H)](https://www.radixark.com/images/daisyrose-27b/gpu-efficiency.png)

**2× faster end to end.** Counting the time the agent spends on its tasks, the release run trained on about 16,800 tokens per second across its 144 GPUs. The open-source trainer reached about 8,300 on the same GPUs.

[![End-to-end training speed on 144 H200s: the internal backend trains 16.8k tokens per second versus 8.3k with the open-source trainer, a 2.0 times speedup.](https://www.radixark.com/_next/image?url=%2Fimages%2Fdaisyrose-27b%2Ftraining-speed.png&w=3840&q=75&dpl=dpl_2WZt4617DGLJ3EDD7HwJmGV2VG4H)](https://www.radixark.com/images/daisyrose-27b/training-speed.png)

**Stable over weeks.** The release run trained for weeks on 144 H200s, 63,000 GPU-hours in total. From the first step to the last, its speed and efficiency held steady.

[![Training speed and GPU efficiency across 18 days on 144 H200s. Median training-step speed is 19.3k tokens per second and median model FLOPs utilization is 37%; dashed open-source baselines are 13.5k and 16%.](https://www.radixark.com/_next/image?url=%2Fimages%2Fdaisyrose-27b%2Ftraining-stability.png&w=3840&q=75&dpl=dpl_2WZt4617DGLJ3EDD7HwJmGV2VG4H)](https://www.radixark.com/images/daisyrose-27b/training-stability.png)\
*Every training step of the DaisyRose-27B release run on 144 H200s, plotted against total training time. Dashed lines: the open-source trainer on comparable runs.*

## Robust sandbox service

During training, every attempt the agent makes runs in a sandbox: a fresh copy of the task's environment where the agent runs commands, edits files, and is then graded. All of the RadixArk team's work on this release ran in sandboxes on our own service, built on the open-source [AgentENV](https://github.com/kvcache-ai/AgentENV) project, to which we also contribute fixes. Between mid-September and mid-November, the service ran about 944,000 sandboxes for this project, with up to 1,566 running at the same time.

**Reliable as it grew.** The service grew from a single machine to as many as 12. We shipped ten upgrades to it, nearly all of them while training was running. Across all nine weeks, 99% of sandbox creations succeeded, and since late October more than 99.95% have. Getting there took a steady stream of fixes and improvements, such as keeping a machine from freezing when it runs out of memory.

[![944K total sandboxes, 1,566 peak concurrent sandboxes, and a 99.95% success rate in the final run.](https://www.radixark.com/_next/image?url=%2Fimages%2Fdaisyrose-27b%2Fsandboxes.png&w=3840&q=75&dpl=dpl_2WZt4617DGLJ3EDD7HwJmGV2VG4H)](https://www.radixark.com/images/daisyrose-27b/sandboxes.png)

**80% lower cost.** Our cloud infrastructure cost for the project's sandboxes was about 80% lower than the cost of running the same sandboxes with popular cloud sandbox providers. One way we keep costs down is to give each sandbox memory that grows and shrinks with what it actually uses. A sandbox usually uses only a small part of the memory it asks for, so each machine can run many more sandboxes.

## Try DaisyRose-27B, or train your own

You can download DaisyRose-27B from [Hugging Face](https://huggingface.co/RadixArk/DaisyRose-27B), and you can train your own agents with our open source reinforcement learning framework [Miles](https://miles.radixark.com/).

DaisyRose-27B is one example of what high-quality data and efficient infrastructure can make possible. We are bringing this approach to teams building specialized models, with a focus on improving the capabilities they need while preserving the model’s broader strengths. If you are exploring post-training for your own model, reach out at [contact@radixark.ai](mailto:contact@radixark.ai).

We are deeply grateful to Eigent AI for working closely with us on training data, and to the Qwen team for creating Qwen3.8-27B, the foundation of our work. We also thank the Miles and [SGLang](https://www.sglang.io/) communities, as well as other open-source projects, including slime, AgentENV, and Harbor.

[^1]: *How we evaluated: Terminal Bench 4.1 is averaged over 8 runs per task, using our internal mini-swe-agent harness with summarization turned on. Terminal Bench 2.1 is averaged over 5 runs per task, using the Terminus 2 harness. DeepSWE v1.1 and SWE Bench Pro v2 are averaged over 8 and 5 runs per task, both using the mini-swe-agent harness. HLE (text-only) is averaged over 5 runs per problem, graded by GPT-6 Luna, with no tools. IFBench is averaged over 4 runs on each of its 300 prompts, scored by the official allenai/IFBench scorer as prompt-level loose accuracy on the final response, with the thinking left out. GPQA Diamond is averaged over 8 runs on each of its 198 questions, using the simple-evals multiple-choice template and answer extraction, with the answer order reshuffled on every run.*
