---
title: Grok Code Fast 1
link: https://x.ai/news/grok-code-fast-1
source: x-ai-news
published: 2025-08-28T00:00:00Z
first_seen: 2026-09-08T10:17:10.166315Z
summary: We're thrilled to introduce grok-code-fast-1, a speedy and economical reasoning model that excels at agentic coding.
content: extracted
html: 2025-08-28-grok-code-fast-1.html
preview:
  file: 2025-08-28-grok-code-fast-1.preview-fd6b4c224ec1.webp
  width: 256
  height: 144
  color: '#5c5060'
images:
- source: https://media.x.ai/v1/website/grok-code-fast-32a61c44.webp
  original:
    file: 2025-08-28-grok-code-fast-1.image-04850377004e.webp
    width: 1486
    height: 836
  variants:
  - file: 2025-08-28-grok-code-fast-1.image-0cb259551485.webp
    width: 48
    height: 27
  - file: 2025-08-28-grok-code-fast-1.image-d320c0f7b7b5.webp
    width: 320
    height: 180
  - file: 2025-08-28-grok-code-fast-1.image-283e535b8d29.webp
    width: 640
    height: 360
  color: '#05090c'
- source: https://x.ai/_next/image?url=https%3A%2F%2Fpbs.twimg.com%2Fprofile_images%2F1783313134117376000%2FhWT1AJWp_400x400.jpg&w=64&q=75
  original:
    file: 2025-08-28-grok-code-fast-1.image-d3303f6f0817.webp
    width: 64
    height: 64
  color: '#fefefe'
---

![Abstract digital cheetah](https://x.ai/news/f=auto/v1/website/grok-code-fast-32a61c44.webp)

## [A speedy daily driver](https://x.ai/news/grok-code-fast-1#a-speedy-daily-driver)

While today's models are undeniably powerful, they often don't feel purpose-built for agentic coding workflows, where loops of reasoning and tool calls can feel frustratingly slow. As heavy users of agentic coding tools, our engineers saw room for a more nimble, responsive solution optimized for our day-to-day tasks.

We built `grok-code-fast-1` from scratch, starting with a brand-new model architecture. To lay a robust foundation, we carefully assembled a pre-training corpus rich with programming-related content. For post-training, we curated high-quality datasets that reflect real-world pull requests and coding tasks.

Throughout the training process, we collaborated closely with our launch partners to refine and sharpen the model’s behavior inside their agentic platforms. `grok-code-fast-1` has mastered the use of common tools like grep, terminal, and file editing, and thus should feel right at home in your favorite IDE.

We've teamed up with select launch partners to offer `grok-code-fast-1` for free for a limited time, including GitHub Copilot, Cursor, Cline, Roo Code, Kilo Code, opencode, and Windsurf.

## [Blazing fast](https://x.ai/news/grok-code-fast-1#blazing-fast)

Our inference and supercomputing teams developed several innovative techniques to dramatically accelerate our serving speed, creating a uniquely responsive experience where the model will have already called dozens of tools before you even finish reading the first paragraph of the thinking trace. We've also invested in prompt caching optimizations, regularly achieving cache hit rates above 90% when used with our launch partners.

## [A versatile programmer](https://x.ai/news/grok-code-fast-1#a-versatile-programmer)

`grok-code-fast-1` is exceptionally versatile across the full software development stack and is particularly adept at TypeScript, Python, Java, Rust, C++, and Go. It can complete common programming tasks with minimal oversight, ranging from building zero-to-one projects and providing insightful answers to codebase questions to performing surgical bug fixes.

Example 1 of 2 Battle Simulator

## [An economical choice](https://x.ai/news/grok-code-fast-1#an-economical-choice)

We designed `grok-code-fast-1` to be widely accessible, priced at:

- $0.20 per million input tokens
- $1.50 per million output tokens
- $0.02 per million cached input tokens

`grok-code-fast-1` was crafted to shine in the tasks developers face every day, striking a compelling balance between performance and cost. Its strength lies in delivering strong performance in a economical, compact form factor, making it a versatile choice for tackling common coding tasks quickly and cost-effectively.

### Model Performance

Tokens per Second vs Output Price

Tokens per second (TPS)

190

Output price / per 1M tokens

$18

### Methodology

### TPS metrics were calculated by directly measuring response generation speed via each modelprovider's API, considering only the final response tokens.

\- Gemini 2.5 Pro, GPT-5, and Claude Sonnet 4: Measured using their respective public APIs.

\- Grok Code Fast 1 and Grok 4: Measured using the xAI API.

\- Qwen3-Coder: Hosted on DeepInfra at low precision (fp4), which reduces response quality.

We took a holistic approach to evaluating model performance, blending public benchmarks with real-world testing. On the full subset of SWE-Bench-Verified, `grok-code-fast-1` scored 70.8% using our own internal harness.

While benchmarks like SWE-Bench provide valuable insights, we've found they don't fully reflect the nuances of real-world software engineering, particularly the end-user experience in agentic coding workflows.

To guide our model training, we pair these benchmarks with routine human assessments, where experienced developers rate the model's end-to-end performance on everyday tasks. We've also built automated evaluations to track key aspects of behavior, helping us balance trade-offs in design.

When developing `grok-code-fast-1`, we focused on usability and user satisfaction, guided by real-world human evaluations. The result is a model rated by programmers as fast and reliable for everyday coding tasks.

## [Grok Code for everyone](https://x.ai/news/grok-code-fast-1#grok-code-for-everyone)

For a limited time, we’re excited to offer `grok-code-fast-1` for free on exclusive launch partners. Here’s what our launch partners had to say about our model, which was recently released in stealth under the codename `sonic`.

### Free for a limited time

We’re excited to offer Grok Code Fast 1 for free on exclusive launch partners.

![GitHub Copilot](https://x.ai/_next/static/media/copilot.3zc6mafl6ew3f.svg?dpl=a2d6b77a75dae8f07c881cf892692231d4dce27a)

GitHub Copilot

"In early testing, Grok Code Fast has shown both its speed and quality in agentic coding tasks. Empowering developers with powerful tools is a core part of our mission at GitHub Copilot, and this is a compelling new option for our developers."

[![Mario Rodriguez](https://x.ai/_next/image?url=https%3A%2F%2Fpbs.twimg.com%2Fprofile_images%2F1783313134117376000%2FhWT1AJWp_400x400.jpg&w=64&q=75)

Mario Rodriguez (@mariorod1)

Chief Product Officer, GitHub](https://x.com/mariorod1)

Instructions

The model is generally available via the xAI API, priced at $0.20 / 1M input tokens, $1.50 / 1M output tokens, and $0.02 / 1M cached input tokens.

## [What to expect in the next few weeks](https://x.ai/news/grok-code-fast-1#what-to-expect-in-the-next-few-weeks)

Last week, we quietly released `grok-code-fast-1` under the codename `sonic`. During this stealth phase, our team carefully monitored community channels and deployed multiple new model checkpoints to address feedback.

As we advance this new model family, we're excited to iterate rapidly on your input. We highly value the developer community's support and encourage you to freely [share all feedback](https://discord.gg/x-ai), positive and negative.

We'll focus on delivering consistent updates to `grok-code-fast-1`, with improvements arriving in days rather than weeks. A new variant that supports multimodal inputs, parallel tool calling, and extended context length is already in training.

Read the `grok-code-fast-1` [model card here](https://data.x.ai/2025-08-26-grok-code-fast-1-model-card.pdf). We’re excited to see what you build!
