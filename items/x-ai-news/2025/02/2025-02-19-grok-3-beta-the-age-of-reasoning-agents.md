---
title: Grok 3 Beta — The Age of Reasoning Agents
link: https://x.ai/news/grok-3
source: x-ai-news
published: 2025-02-19T00:00:00Z
first_seen: 2026-09-07T17:03:44.343711062Z
summary: We are thrilled to unveil an early preview of Grok 3, our most advanced model yet, blending superior reasoning with extensive pretraining knowledge.
content: extracted
html: 2025-02-19-grok-3-beta-the-age-of-reasoning-agents.html
preview:
  file: 2025-02-19-grok-3-beta-the-age-of-reasoning-agents.preview-32239d0b5fc9.webp
  width: 256
  height: 144
  color: '#a13c27'
images:
- source: https://media.x.ai/v1/website/grok-3-327b0918.webp
  original:
    file: 2025-02-19-grok-3-beta-the-age-of-reasoning-agents.image-f8bfd3c9a6e9.webp
    width: 1486
    height: 836
  variants:
  - file: 2025-02-19-grok-3-beta-the-age-of-reasoning-agents.image-6990f161c5e2.webp
    width: 48
    height: 27
  - file: 2025-02-19-grok-3-beta-the-age-of-reasoning-agents.image-05fd388e0957.webp
    width: 320
    height: 180
  - file: 2025-02-19-grok-3-beta-the-age-of-reasoning-agents.image-ab968ae61148.webp
    width: 640
    height: 360
  color: '#190505'
---

[Back to news](https://x.ai/news)

Feb 19, 2025

We are thrilled to unveil an early preview of Grok 3, our most advanced model yet, blending superior reasoning with extensive pretraining knowledge.

* * *

## [Next-Generation Intelligence from xAI](https://x.ai/news/grok-3#next-generation-intelligence-from-xai)

We are pleased to introduce Grok 3, our most advanced model yet: blending strong reasoning with extensive pretraining knowledge. Trained on our Colossus supercluster with 10x the compute of previous state-of-the-art models, Grok 3 displays significant improvements in reasoning, mathematics, coding, world knowledge, and instruction-following tasks. Grok 3's reasoning capabilities, refined through large scale reinforcement learning, allow it to think for seconds to minutes, correcting errors, exploring alternatives, and delivering accurate answers. Grok 3 has leading performance across both academic benchmarks and real-world user preferences, achieving an Elo score of 1402 in the Chatbot Arena. Alongside it, we’re unveiling Grok 3 mini, which represents a new frontier in cost-efficient reasoning. Both models are still in training and will evolve rapidly with your feedback. We are rolling out Grok 3 to users in the coming days, along with an early preview of its reasoning capabilities.

## [Thinking Harder: Test-time Compute and Reasoning](https://x.ai/news/grok-3#thinking-harder-test-time-compute-and-reasoning)

Today, we are announcing two beta reasoning models, Grok 3 (Think) and Grok 3 mini (Think). They were trained using reinforcement learning (RL) at an unprecedented scale to refine its chain-of-thought process, enabling advanced reasoning in a data-efficient manner. With RL, Grok 3 (Think) learned to refine its problem-solving strategies, correct errors through backtracking, simplify steps, and utilize the knowledge it picked up during pretraining. Just like a human when tackling a complex problem, Grok 3 (Think) can spend anywhere from a few seconds to several minutes reasoning, often considering multiple approaches, verifying its own solution, and evaluating how to precisely meet the requirements of the problem.

Both models are still in training, but already they show remarkable performance across a range of benchmarks. We tested these models on the 2025 American Invitational Mathematics Examination (AIME), which was released just 7 days ago on Feb 12th. With our highest level of test-time compute (cons@64), Grok 3 (Think) achieved 93.3% on this competition. Grok 3 (Think) also attained 84.6% on graduate-level expert reasoning (GPQA), and 79.4% on LiveCodeBench for code generation and problem-solving. Furthermore, Grok 3 mini reaches a new frontier in cost-efficient reasoning for STEM tasks that don't require as much world knowledge, reaching 95.8% on AIME 2024 and 80.4% on LiveCodeBench.

### AIME’25

Competition Math

### AIME’24

Competition Math

### GPQA

Graduate-Level Google-Proof Q&A (Diamond)

### LCB

Code Generation: 10/1/2024 - 2/1/2025

### MMMU

Multimodal Understanding

To use Grok 3’s reasoning capabilities, just press the `Think` button. Grok 3 (Think)’s mind is completely open, allowing users to inspect not only the final answer but the reasoning process of the model itself. We have found that Grok 3 (Think)'s performance generalizes across diverse problem domains. Here are some Grok 3 reasoning examples.

### Research samples

* * *

Create a game that is a mixture of two classic games. Make it in pygame and make it look pretty.

Below is a complete solution to your query: a Pygame game called "Break-Pong," which combines elements of **Pong** and **Breakout**. In this game, two players control paddles on opposite sides of the screen, hitting a ball to break a central wall of bricks. Points are scored by breaking bricks and by letting the ball pass the opponent's paddle. The game includes smooth animations, colorful graphics, and particle effects to make it visually appealing. Here's the implementation:

### Code Implementation

python

* * *

### How to Run

1. Ensure you have Python and Pygame installed (`pip install pygame`).
2. Copy the code into a `.py` file (e.g., `break_pong.py`).
3. Run the file using Python (e.g., `python break_pong.py`).

* * *

### Gameplay Instructions

- **Controls**:
  - Left paddle: `W` (up), `S` (down)
  - Right paddle: `Up Arrow` (up), `Down Arrow` (down)
- **Objective**: Break bricks in the central wall and score points by letting the ball pass the opponent's paddle. First to 50 points wins.

* * *

### Features

- **Pong Elements**: Two paddles bounce a ball back and forth, with points scored when the ball passes an opponent's paddle.
- **Breakout Elements**: A wall of bricks that the ball can break upon impact.
- **Visual Appeal**:
  - Smooth paddle and ball movement.
  - Colorful bricks (red, green, blue) randomly assigned.
  - Particle effects when bricks break, with fading particles for a dynamic look.
  - Clear score display and game-over message.

* * *

### Enhancements

To make it even prettier or more engaging, consider:

- Adding sound effects (e.g., bounce, break sounds).
- Implementing power-ups (e.g., larger paddle, faster ball) released from special bricks.
- Adding a background gradient or pattern.
- Including a start menu and smoother transitions.

This implementation provides a fun, visually appealing mixture of Pong and Breakout, fulfilling your request! Enjoy playing "Break-Pong"!

## [Pretraining on a Massive Scale](https://x.ai/news/grok-3#pretraining-on-a-massive-scale)

With reasoning turned off, Grok 3 gives instant, high quality responses. Grok 3 delivers state-of-the-art results across diverse academic benchmarks among non reasoning models, including: graduate-level science knowledge (GPQA), general knowledge (MMLU-Pro), math competition problems (AIME). Grok 3 also excels in image understanding (MMMU) and video understanding (EgoSchema) tasks.

| Benchmark | Grok 3 Beta | Grok 3 mini Beta | Gemini 2.0 | DeepSeek-V3 | GPT 4o | Claude 3.5 Sonnet |
| --------- | ----------- | ---------------- | ---------- | ----------- | ------ | ----------------- |
| 52.2%     | 39.7%       | —                | 39.2%      | 9.3%        | 16.0%  |                   |
| 75.4%     | 66.2%       | 64.7%            | 59.1%      | 53.6%       | 65.0%  |                   |
| 57.0%     | 41.5%       | 36.0%            | 33.1%      | 32.3%       | 40.2%  |                   |
| 79.9%     | 78.9%       | 79.1%            | 75.9%      | 72.6%       | 78.0%  |                   |
| 83.3%     | 83.1%       | 75.6%            | —          | 78.0%       | 69.9%  |                   |
| 43.6%     | 21.7%       | 44.3%            | 24.9%      | 38.2%       | 28.4%  |                   |
| 73.2%     | 69.4%       | 72.7%            | —          | 69.1%       | 70.4%  |                   |
| 74.5%     | 74.3%       | 71.9%            | —          | 72.2%       | —      |                   |

With a context window of 1 million tokens — 8 times larger than our previous models — Grok 3 can process extensive documents and handle complex prompts while maintaining instruction-following accuracy. On the LOFT (128k) benchmark, which targets long-context RAG use cases, Grok 3 achieved state-of-the-art accuracy (averaged across 12 diverse tasks), showcasing its powerful information retrieval capabilities.

Grok 3 also demonstrates improved factual accuracy and enhanced stylistic control. Under the codename `chocolate`, an early version of Grok 3 topped the LMArena Chatbot Arena leaderboard, outperforming all competitors in Elo scores across all categories. As we continue to scale, we are preparing to train even larger models on our 200,000 GPU cluster.

![Chatbot Arena Score](https://x.ai/news/f=auto/v1/website/arena-b7019483.webp)Chatbot Arena Score

## [Grok Agents: Combining Reasoning and Tool Use](https://x.ai/news/grok-3#grok-agents-combining-reasoning-and-tool-use)

To understand the universe, we must interface Grok with the world. Equipped with code interpreters and internet access, Grok 3 models learn to query for missing context, dynamically adjust their approach, and improve their reasoning based on feedback.

As a first step towards this vision, we are rolling out `DeepSearch`—our first agent. It's a lightning-fast AI agent built to be useful across the entire corpus of human knowledge. `DeepSearch` is designed to synthesize key information, reason about conflicting facts and opinions, and distill clarity from complexity. Whether you need to access the latest real-time news, seek advice about your social woes, or conduct in-depth scientific research, `DeepSearch` will take you far beyond a browser search. Its final summary trace results in a concise and comprehensive report, to help you keep up with a world that never slows down.

### DeepSearch Showcase

## [Grok 3 API Coming Soon](https://x.ai/news/grok-3#grok-3-api-coming-soon)

In the coming weeks, we will release Grok 3 and Grok 3 mini via our API platform, offering access to both the standard and reasoning models. `DeepSearch` will also be released to Enterprise partners via our API.

## [What’s Next for Grok 3?](https://x.ai/news/grok-3#whats-next-for-grok-3)

Grok 3’s training is ongoing, with frequent updates planned over the next few months. We are excited to roll out new features in the [Enterprise API](https://console.x.ai?utm_source=website&utm_medium=referral&utm_campaign=grok-3-blog), including tool use, code execution, and advanced agent capabilities. Following our [RMF](https://data.x.ai/2025.02.20-RMF-Draft.pdf) (Risk Management Framework) release last week, we are particularly interested in accelerating progress in scalable oversight and adversarial robustness during training.

Grok 3 is now available to 𝕏 Premium and Premium+ users on [𝕏](https://x.com/i/grok) and [Grok.com](https://grok.com). 𝕏 Premium+ users will also immediately gain access to `Think` and `DeepSearch`. In addition, Grok 3 capabilities are being rolled out to all Grok users with usage limits. 𝕏 Premium+ users will have higher limits and access to advanced capabilities.

## [Join the Journey](https://x.ai/news/grok-3#join-the-journey)

Since launching Grok 1 in November 2023, xAI’s small, talent-dense team has driven historic progress, positioning us at the forefront of AI innovation. With Grok 3, we are advancing core reasoning capabilities using our expanded Colossus supercluster, with exciting developments to come. If you are passionate about building AI for humanity’s future, apply to join our team at [x.ai/careers](https://x.ai/careers).
