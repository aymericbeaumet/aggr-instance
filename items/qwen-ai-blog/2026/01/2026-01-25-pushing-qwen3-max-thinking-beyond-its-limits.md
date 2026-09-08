---
title: Pushing Qwen3-Max-Thinking Beyond its Limits
link: https://qwen.ai/blog?id=qwen3-max-thinking
source: qwen-ai-blog
published: 2026-01-25T20:00:00Z
first_seen: 2026-09-08T10:17:10.166315Z
authors:
- QwenTeam
labels:
- release
content: feed
html: 2026-01-25-pushing-qwen3-max-thinking-beyond-its-limits.html
preview:
  file: 2026-01-25-pushing-qwen3-max-thinking-beyond-its-limits.preview-7594ce333254.webp
  width: 256
  height: 154
  color: '#d0b1ec'
images:
- source: https://img.alicdn.com/imgextra/i3/O1CN01wdW0HW1e4Eu3CE2Jb_!!6000000003817-2-tps-1590-954.png
  original:
    file: 2026-01-25-pushing-qwen3-max-thinking-beyond-its-limits.image-6913143ac0c0.png
    width: 1590
    height: 954
  variants:
  - file: 2026-01-25-pushing-qwen3-max-thinking-beyond-its-limits.image-6a1270b1985c.webp
    width: 48
    height: 29
  color: '#d8a6e9'
- source: https://qianwen-res.oss-accelerate-overseas.aliyuncs.com/Qwen3-Max-Thinking/banner.png
  original:
    file: 2026-01-25-pushing-qwen3-max-thinking-beyond-its-limits.image-bbfd26f60a0f.png
    width: 2566
    height: 766
  variants:
  - file: 2026-01-25-pushing-qwen3-max-thinking-beyond-its-limits.image-c4f2c84febeb.webp
    width: 48
    height: 14
  - file: 2026-01-25-pushing-qwen3-max-thinking-beyond-its-limits.image-72a0b09c3bd8.webp
    width: 320
    height: 96
  - file: 2026-01-25-pushing-qwen3-max-thinking-beyond-its-limits.image-e14759a956c3.webp
    width: 640
    height: 191
  - file: 2026-01-25-pushing-qwen3-max-thinking-beyond-its-limits.image-b253df504ed7.webp
    width: 960
    height: 287
  - file: 2026-01-25-pushing-qwen3-max-thinking-beyond-its-limits.image-afad400029c9.webp
    width: 1280
    height: 382
  - file: 2026-01-25-pushing-qwen3-max-thinking-beyond-its-limits.image-db52e1073db6.webp
    width: 1600
    height: 478
  - file: 2026-01-25-pushing-qwen3-max-thinking-beyond-its-limits.image-224b1bad12a6.webp
    width: 2566
    height: 766
  color: '#e4fafe'
---

![Qwen3 Main Image](https://qianwen-res.oss-accelerate-overseas.aliyuncs.com/Qwen3-Max-Thinking/banner.png)

[QWEN CHAT](https://chat.qwen.ai) [API](https://www.alibabacloud.com/help/en/model-studio/models#c2d5833ae4jmo) [DISCORD](https://discord.gg/yPEP2vHTu4)

## Introduction [#](https://qwen.ai/blog?id=qwen3-max-thinking#introduction)

We present **Qwen3-Max-Thinking**, our latest flagship reasoning model. By scaling up model parameters and leveraging substantial computational resources for reinforcement learning, Qwen3-Max-Thinking achieves significant performance improvements across multiple dimensions, including factual knowledge, complex reasoning, instruction following, alignment with human preferences, and agent capabilities. On 19 established benchmarks, it demonstrates performance comparable to leading models such as GPT-5.2-Thinking, Claude-Opus-4.5, and Gemini 3 Pro.

We further enhance Qwen3-Max-Thinking with two key innovations: (1) adaptive tool-use capabilities that enable on-demand retrieval and code interpreter invocation, now available at [chat.qwen.ai](https://chat.qwen.ai); and (2) advanced test-time scaling techniques that significantly boost reasoning performance, surpassing Gemini 3 Pro on key reasoning benchmarks.

![](https://qianwen-res.oss-accelerate-overseas.aliyuncs.com/Qwen3-Max-Thinking/score.png)

The table below presents a more comprehensive set of evaluation scores.

| Capability                            | Benchmark                                                          | GPT-5.2\ -Thinking | Claude-Opus\ -4.5 | Gemini 3 Pro | DeepSeek V3.2 | Qwen3-Max\ -Thinking |
| ------------------------------------- | ------------------------------------------------------------------ | ------------------ | ----------------- | ------------ | ------------- | -------------------- |
| **Knowledge**                         | MMLU-Pro                                                           | 87.4               | 89.5              | 89.8         | 85.0          | 85.7                 |
|                                       | MMLU-Redux                                                         | 95.0               | 95.6              | 95.9         | 94.5          | 92.8                 |
|                                       | C-Eval                                                             | 90.5               | 92.2              | 93.4         | 92.9          | 93.7                 |
| **STEM**                              | GPQA                                                               | 92.4               | 87.0              | 91.9         | 82.4          | 87.4                 |
|                                       | HLE [1](https://qwen.ai/blog?id=qwen3-max-thinking#fn:1)            | 35.5               | 30.8              | 37.5         | 25.1          | 30.2                 |
| **Reasoning**                         | LiveCodeBench v6                                                   | 87.7               | 84.8              | 90.7         | 80.8          | 85.9                 |
|                                       | HMMT Feb 25                                                        | 99.4               | -                 | 97.5         | 92.5          | 98.0                 |
|                                       | HMMT Nov 25                                                        | -                  | -                 | 93.3         | 90.2          | 94.7                 |
|                                       | IMOAnswerBench                                                     | 86.3               | 84.0              | 83.3         | 78.3          | 83.9                 |
| **Agentic Coding**                    | SWE Verified                                                       | 80.0               | 80.9              | 76.2         | 73.1          | 75.3                 |
| **Agentic Search**                    | HLE (w/ tools)[2](https://qwen.ai/blog?id=qwen3-max-thinking#fn:2) | 45.5               | 43.2              | 45.8         | 40.8          | 49.8                 |
| **Instruction Following & Alignment** | IFBench                                                            | 75.4               | 58.0              | 70.4         | 60.7          | 70.9                 |
|                                       | MultiChallenge                                                     | 57.9               | 54.2              | 64.2         | 47.3          | 63.3                 |
|                                       | Arena-Hard v2 [3](https://qwen.ai/blog?id=qwen3-max-thinking#fn:3)  | 80.6               | 76.7              | 81.7         | 66.5          | 90.2                 |
| **Tool Use**                          | Tau² Bench [4](https://qwen.ai/blog?id=qwen3-max-thinking#fn:4)     | 80.9               | 85.7              | 85.4         | 80.3          | 82.1                 |
|                                       | BFCL-V4 [5](https://qwen.ai/blog?id=qwen3-max-thinking#fn:5)        | 63.1               | 77.5              | 72.5         | 61.2          | 67.7                 |
|                                       | Vita Bench                                                         | 38.2               | 56.3              | 51.6         | 44.1          | 40.9                 |
|                                       | Deep Planning [6](https://qwen.ai/blog?id=qwen3-max-thinking#fn:6)  | 44.6               | 33.9              | 23.3         | 21.6          | 28.7                 |
| **Long Context**                      | AA-LCR                                                             | 72.7               | 74.0              | 70.7         | 65.0          | 68.7                 |

* * *

1. We evaluated only on the text subset. [↩︎](https://qwen.ai/blog?id=qwen3-max-thinking#fnref:1)

2. We blocked access to Hugging Face and other HLE-related websites to prevent data leakage. [↩︎](https://qwen.ai/blog?id=qwen3-max-thinking#fnref:2)

3. For reproducibility of Arena-Hard v2, we report the win rates evaluated by GPT-4.1. [↩︎](https://qwen.ai/blog?id=qwen3-max-thinking#fnref:3)

4. We followed the official setting of Tau² Bench with no custom scaffolding. [↩︎](https://qwen.ai/blog?id=qwen3-max-thinking#fnref:4)

5. BFCL-V4 is configured with a maximum of 100 interaction turns. [↩︎](https://qwen.ai/blog?id=qwen3-max-thinking#fnref:5)

6. Deep Planning is a built-in agentic benchmark. [↩︎](https://qwen.ai/blog?id=qwen3-max-thinking#fnref:6)

## Adaptive Tool-Use Capabilities [#](https://qwen.ai/blog?id=qwen3-max-thinking#adaptive-tool-use-capabilities)

Unlike earlier approaches that required users to manually select tools before each task, Qwen3-Max-Thinking autonomously selects and leverages its built-in Search, Memory, and Code Interpreter capabilities during conversations. This capability emerges from a focused training process: after initial fine-tuning for tool use, the model underwent further training on diverse tasks using both rule-based and model-based feedback. Empirically, we observe that the Search and Memory tools effectively mitigate hallucinations, provide access to real-time information, and enable more personalized responses. The Code Interpreter allows users to execute code snippets and apply computational reasoning to solve complex problems. Together, these features deliver a seamless and capable conversational experience.

## Test-time Scaling Strategy [#](https://qwen.ai/blog?id=qwen3-max-thinking#test-time-scaling-strategy)

Test-time scaling refers to techniques that allocate additional computation during inference to improve model performance. We propose an experience-cumulative, multi-round test-time scaling strategy for the heavy mode. Instead of simply increasing parallel trajectories $N$, which often yields redundant reasoning, we limit $N$ and redirect saved computation to iterative self-reflection guided by a “take-experience” mechanism. This mechanism distills key insights from past rounds, allowing the model to avoid re-deriving known conclusions and focus on unresolved uncertainties. Crucially, it achieves higher context efficiency than naively referencing raw trajectories, enabling richer integration of historical information within the same context window. This approach consistently outperforms standard parallel sampling and aggregation with roughly the same token consumption: GPQA (90.3 → 92.8), HLE (34.1 → 36.5), LiveCodeBench v6 (88.0 → 91.4), IMO-AnswerBench (89.5 → 91.5), and HLE (w/ tools) (55.8 → 58.3).

## Develop with Qwen3-Max-Thinking [#](https://qwen.ai/blog?id=qwen3-max-thinking#develop-with-qwen3-max-thinking)

Qwen3-Max-Thinking is now available in [Qwen Chat](https://chat.qwen.ai), where users can interact with the model and its adaptive tool-use capabilities. Meanwhile, the API of Qwen3-Max-Thinking (whose model name is `qwen3-max-2026-01-23`) is available. You can first [register an Alibaba Cloud account](https://account.alibabacloud.com/register/intl_register.htm) and activate Alibaba Cloud Model Studio service, and then navigate to the console and create an API key.

Since the APIs of Qwen are OpenAI-API compatible, we can directly follow the common practice of using OpenAI APIs. Below is an example of using Qwen3-Max-Thinking in Python:

```python
from openai import OpenAI
import os

client = OpenAI(
    api_key=os.getenv("API_KEY"),
    base_url="https://dashscope-intl.aliyuncs.com/compatible-mode/v1",
)

completion = client.chat.completions.create(
    model="qwen3-max-2026-01-23",
    messages=[
      {'role': 'user', 'content': 'Give me a short introduction to large language model.'}
    ],
    extra_body={"enable_thinking": True}
)

print(completion.choices[0].message)
```

The APIs of Qwen are also compatible with the Anthropic API protocol, enabling Qwen3-Max-Thinking to work seamlessly with Claude Code. Simply use the API key created at [Alibaba Cloud account](https://account.alibabacloud.com/register/intl_register.htm) and install Claude Code to elevate your coding experience. Below is the quick start script.

```
# Install Claude Code
npm install -g @anthropic-ai/claude-code

# Configure Environment Variables
export ANTHROPIC_MODEL="qwen3-max-2026-01-23" 
export ANTHROPIC_SMALL_FAST_MODEL="qwen3-max-2026-01-23"
export ANTHROPIC_BASE_URL=https://dashscope.aliyuncs.com/apps/anthropic
export ANTHROPIC_AUTH_TOKEN=your-dashscope-apikey

# Execute
claude
```

## Citation [#](https://qwen.ai/blog?id=qwen3-max-thinking#citation)

Feel free to cite the following article if you find Qwen3-Max-Thinking helpful.

```
@misc{qwen3maxthinking,
    title = {Pushing Qwen3-Max-Thinking Beyond its Limits},
    url = {https://qwen.ai/blog?id=qwen3-max-thinking},
    author = {Qwen Team},
    month = {January},
    year = {2026}
}
```
