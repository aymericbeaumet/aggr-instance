---
title: Introducing Grok 4.5
link: https://x.ai/news/grok-4-5
source: x-ai-news
published: 2026-07-16T00:00:00Z
first_seen: 2026-09-08T10:17:10.166315Z
summary: Grok 4.5 is SpaceXAI's smartest model built for coding, agentic tasks, and knowledge work.
content: extracted
html: 2026-07-16-introducing-grok-4-5.html
preview:
  file: 2026-07-16-introducing-grok-4-5.preview-3ddff23bc077.webp
  width: 256
  height: 134
  color: '#191919'
images:
- source: https://media.x.ai/v1/website/grok-4-5-og-786d2a81.png
  original:
    file: 2026-07-16-introducing-grok-4-5.image-b99a2f9da986.jpg
    width: 1024
    height: 537
  variants:
  - file: 2026-07-16-introducing-grok-4-5.image-2570292c9817.webp
    width: 48
    height: 25
  color: '#0a0a0a'
---

Today, we're launching **Grok 4.5**, SpaceXAI's smartest model built to excel at coding, agentic tasks, and knowledge work. It's our strongest model ever and was trained alongside [Cursor](https://cursor.com/blog/spacex-model-training).

## [Real-world engineering excellence](https://x.ai/news/grok-4-5#real-world-engineering-excellence)

Grok 4.5 was trained on datasets spanning knowledge in coding, science, engineering, and math. With both intelligent and efficient reasoning, Grok 4.5 excels at real engineering tasks and exceeds comparable leading models at these tasks.

Competitor figures are drawn from the respective developers’ published system cards or benchmark leaderboards

Benchmark bar charts of model scores. DeepSWE 1.0 (eval created by Datacurve, run with each model provider’s harnesses by AA): Fable (max) 66.1%, GPT 5.5 (xhigh) 64.31%, Grok 4.5 62.0%, Opus 4.8 (max) 55.75%, Opus 4.7 (max) 40.12%. DeepSWE 1.1 (mini-swe-agent harness run by Datacurve): Fable (max) 70%, GPT 5.5 (xhigh) 67%, Opus 4.8 (max) 59%, Grok 4.5 53%, GLM 5.2 44%. SWE Marathon resolution rate (pass@1): Grok 4.5 29.0%, Opus 4.8 (max) 26.0%, Fable (max) 24.0%, Opus 4.7 (max) 16.0%. Terminal Bench 2.1: Fable (max) 84.3%, GPT 5.5 (xhigh) 83.4%, Grok 4.5 83.3%, Opus 4.8 (max) 78.9%, Opus 4.7 (max) 78.9%. SWE Bench Pro resolve rate: Fable (max) 80.4%, Opus 4.8 (max) 69.2%, Grok 4.5 64.7%, Opus 4.7 (max) 64.3%, GLM 5.2 62.1%, GPT 5.5 (xhigh) 58.6%. Competitor figures are drawn from the respective developers’ published system cards or benchmark leaderboards.

## [Training Grok 4.5](https://x.ai/news/grok-4-5#training-grok-45)

Grok 4.5 was trained across tens of thousands of NVIDIA GB300 GPUs, with training and stability techniques designed for large-scale runs. Beyond raw token volume, we invested heavily in data filtering and curation: deduplication, quality scoring, and domain-focused selection so that the data mixture stayed high-coverage and high-signal.

We scaled reinforcement learning with a strong focus on per-token intelligence. Our RL training covers hundreds of thousands of tasks, centered on multi-step software engineering and other technical work, with automated and model-based grading. Our stack is built for highly asynchronous training, so agentic rollouts can run for many hours while learning continues across tens of thousands of GPUs. The result is more intelligent and efficient reasoning on real engineering and agentic tasks.

### Built with one prompt

Grok 4.5 is incredibly capable at coding, from challenging Rust and C/C++ tasks to end-to-end app building from prompt to production. Below are some examples built by the model with one prompt. Grok 4.5 is highly proficient at creating well-designed, end-to-end functional apps even with minimal specification.

Make a beautiful simulation of the universe and solar system. should be sped up with adjustable time, realistic motion, orbits, stars. use threejs. Make the HUD well styled and conform to modern design principles.

## [Faster than flash models](https://x.ai/news/grok-4-5#faster-than-flash-models)

Grok 4.5 is served at fast-model speeds of **80 TPS**. Combined with twice greater token efficiency than the latest leading models at the same tasks, the model delivers intelligent results to you more quickly and at far lower costs.

Token efficiency

avg. output tokens per SWE Bench Pro task

Grok 4.5 0

Opus 4.8 (max) 0

4.2× fewer tokens

0 70k tokens

Token efficiency, average output tokens per SWE Bench Pro task — Grok 4.5 resolves tasks with 15,954 output tokens on average, about 4.2× fewer than Opus 4.8 (max) at 67,020

## [Excels at Office work](https://x.ai/news/grok-4-5#excels-at-office-work)

Grok 4.5 is now the default model in [Grok Build](https://x.ai/build). In addition to its coding proficiency, Grok Build is capable of building complex Excel models that involve research from the web, multi-sheet formula use, and even leaves stickies or notes behind for future reference.

In PowerPoint and Word, Grok 4.5 is similarly meticulous. The model is capable of using native PowerPoint shapes to build complex diagrams, designing intuitive slide content, and writing clear prose in Word.

Outline a 5-slide quarterly business review

Learn more about our plugins for [Word](https://marketplace.microsoft.com/en-us/product/office/WA200011055?tab=Overview), [PowerPoint](https://marketplace.microsoft.com/en-us/product/office/WA200011057?tab=Overview), [Excel](https://marketplace.microsoft.com/en-us/product/office/WA200011056?tab=Overview), and [Outlook](https://marketplace.microsoft.com/en-us/product/office/WA200011330?tab=Overview).

## [Pricing](https://x.ai/news/grok-4-5#pricing)

Grok 4.5 is delivered at an incredibly competitive cost compared to other leading models. Grok 4.5 is priced at $2 per million input tokens and $6 per million output tokens. The model also achieves roughly 2x the token efficiency of comparable leading models, solving tasks in under half the number of steps. Overall, Grok 4.5 delivers the highest intelligence per unit of time and cost.

## [Getting started](https://x.ai/news/grok-4-5#getting-started)

Grok 4.5 is available today in Grok Build, in Cursor on all plans, and from the [SpaceXAI console](https://console.x.ai/?utm_source=website&utm_medium=referral&utm_campaign=grok-4-5-blog). Simply grab an API key and get started in a few lines of code:

bash

### Create an API Key

Start building with Grok 4.5 today via the SpaceXAI API.

### API Docs

Read the docs and integrate Grok 4.5 into your stack.

### Try it in Grok Build for free

We’re offering free Grok 4.5 usage for a limited time in [Grok Build](https://x.ai/build) and Cursor. Get started today at [x.ai/build](https://x.ai/build).
