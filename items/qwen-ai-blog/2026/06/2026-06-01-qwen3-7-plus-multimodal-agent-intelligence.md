---
title: 'Qwen3.7-Plus: Multimodal Agent Intelligence'
link: https://qwen.ai/blog?id=qwen3.7-plus
source: qwen-ai-blog
published: 2026-06-01T02:00:00Z
first_seen: 2026-09-08T10:17:10.166315Z
authors:
- QwenTeam
labels:
- release
content: feed
html: 2026-06-01-qwen3-7-plus-multimodal-agent-intelligence.html
preview:
  file: 2026-06-01-qwen3-7-plus-multimodal-agent-intelligence.preview-4fa14c93d989.webp
  width: 256
  height: 154
  color: '#d0b1eb'
images:
- source: https://img.alicdn.com/imgextra/i3/O1CN01dlIhjJ1Zi1FO5WGsc_!!6000000003227-2-tps-1590-954.png
  original:
    file: 2026-06-01-qwen3-7-plus-multimodal-agent-intelligence.image-d1e09c0d2ffd.png
    width: 1590
    height: 954
  variants:
  - file: 2026-06-01-qwen3-7-plus-multimodal-agent-intelligence.image-f0b4547946e6.webp
    width: 48
    height: 29
  color: '#d8a6e9'
- source: https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen3.7/demo/svg/raw_1.png
  original:
    file: 2026-06-01-qwen3-7-plus-multimodal-agent-intelligence.image-f876bb2604f2.png
    width: 826
    height: 628
  variants:
  - file: 2026-06-01-qwen3-7-plus-multimodal-agent-intelligence.image-ccfab72d0ac7.webp
    width: 48
    height: 36
  color: '#b699a6'
- source: https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen3.7/demo/svg/raw_2.png
  original:
    file: 2026-06-01-qwen3-7-plus-multimodal-agent-intelligence.image-9d02918f4d5d.png
    width: 400
    height: 400
  variants:
  - file: 2026-06-01-qwen3-7-plus-multimodal-agent-intelligence.image-95575ac6593d.webp
    width: 48
    height: 48
  color: '#fefefe'
- source: https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen3.7/demo/svg/raw_5.png
  original:
    file: 2026-06-01-qwen3-7-plus-multimodal-agent-intelligence.image-080739e5dcaa.png
    width: 862
    height: 602
  variants:
  - file: 2026-06-01-qwen3-7-plus-multimodal-agent-intelligence.image-4eec2537a9cd.webp
    width: 48
    height: 34
  color: '#1b2226'
---

![Qwen3.7 Main Image](https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen3.7/Figures/qwen3.7-plus-banner.png)

[DISCORD](https://discord.gg/yPEP2vHTu4)

Today we introduce **Qwen3.7-Plus** — a multimodal agent model that unifies vision and language into a single, versatile agent foundation. Building on Qwen3.7’s strong text backbone, Qwen3.7-Plus delivers a comprehensive upgrade in vision-language capabilities while retaining full agentic strength in coding, tool use, and productivity workflows.

What sets Qwen3.7-Plus apart is its ability to operate as a **multimodal interactive hybrid agent**. It perceives real-world scenes, reads screens and operates GUIs, writes code from visual references, navigates mobile apps end-to-end, and answers visual questions grounded in web knowledge — seamlessly blending GUI and CLI interactions within a single agent loop. As a versatile coding agent and productivity assistant, it handles the full spectrum from frontend prototyping to complex software engineering and multi-step workflow automation with full-modality input. It generalizes across agent scaffolds, performing consistently whether deployed through Claude Code, OpenClaw, Qwen Code, or other frameworks.

- **Qwen3.7-Plus** — now available via [Alibaba Cloud Model Studio](https://modelstudio.alibabacloud.com/):
  - Multimodal interactive hybrid agent: unified GUI & CLI operation across visual and text tasks
  - Versatile coding agent & productivity assistant with full-modality input
  - Visual Agent: perception, reasoning, grounding, and search-augmented QA
  - Cross-harness generalization across diverse agent frameworks
- Call via API on [Alibaba Cloud Model Studio](https://modelstudio.alibabacloud.com/).

## Performance [#](https://qwen.ai/blog?id=qwen3.7-plus#performance)

![](https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen3.7/Figures/Qwen3.7-Plus-Score.png)

### Text Benchmarks [#](https://qwen.ai/blog?id=qwen3.7-plus#text-benchmarks)

|                             | Opus-4.6 Max | K2.6 Thinking | GLM-5.1 Thinking | DeepSeek-V4-Pro Max | Qwen3.6-Plus | Qwen3.7-Plus |
| --------------------------- | ------------ | ------------- | ---------------- | ------------------- | ------------ | ------------ |
| Coding Agent                |              |               |                  |                     |              |              |
| Terminal Bench 2.0-Terminus | 65.4         | 66.7          | 63.5             | 67.9                | 61.6         | 70.3         |
| SWE-Verified                | 80.8         | 80.2          | --               | 80.6                | 78.8         | 77.7         |
| SWE-Pro                     | 57.3         | 59.5          | 58.8             | 59.0                | 56.6         | 57.6         |
| SWE-Multilingual            | 77.5         | 76.7          | --               | 76.2                | 73.8         | 75.8         |
| NL2repo                     | 47.6         | 42.8          | 41.0             | 35.5                | 34.4         | 41.1         |
| SciCode                     | 51.9         | 52.2          | 45.1             | --                  | 41.4         | 51.3         |
| QwenWebDev                  | 1617         | --            | 1564             | 1570                | 1500         | 1536         |
| QwenSVG                     | 1541         | 1325          | 1605             | 1506                | 1432         | 1588         |
| General Agent               |              |               |                  |                     |              |              |
| Qwenclaw                    | 65.5         | 54.7          | 58.7             | 59.2                | 57.2         | 61.8         |
| CoWorkBench                 | 68.2         | 58.2          | 66.0             | 66.3                | 64.5         | 65.1         |
| ClawEval                    | 70.4         | 61.5          | 62.7             | 58.4                | 57.1         | 62.7         |
| Skillsbench                 | --           | 56.2          | 53.1             | 52.3                | 45.7         | 54.9         |
| BFCL-V4                     | 76.7         | 71.3          | 70.9             | 70.6                | 68.9         | 72.9         |
| MCP-Mark                    | 56.7         | 55.9          | 57.5             | 57.1                | 48.2         | 58.7         |
| MCP-Atlas                   | 75.8         | 66.6          | 71.8             | 73.6                | 74.1         | 73.2         |
| Vitabench                   | --           | 39.1          | 45.1             | 51.9                | 42.8         | 45.6         |
| Deep-Planning               | 58.9         | 42.3          | 34.1             | 44.6                | 40.9         | 62.3         |
| SpreadSheetBench-v1         | 89.3         | 84.5          | 85.2             | 84.9                | 80.2         | 86.3         |
| Kernel Bench L3             | 2.63/98%     | 1.41/80%      | 2.00/78%         | 1.07/54%            | 1.03/48%     | 2.06/98%     |
| QwenWorldBench              | 56.1         | 50.9          | 50.2             | 52.3                | 47.6         | 62.1         |
| STEM & Reasoning            |              |               |                  |                     |              |              |
| GPQA Diamond                | 91.3         | 90.5          | 86.2             | 90.1                | 90.4         | 90.3         |
| HLE                         | 40.0         | 36.4          | 34.7             | 37.7                | 28.8         | 34.7         |
| LiveCodeBench               | 88.8         | 89.6          | --               | 93.5                | 87.1         | 89.6         |
| HMMT 2026 Feb               | 96.2         | 92.7          | 89.4             | 95.2                | 87.8         | 92.9         |
| IMOAnswerBench              | 75.3         | 86.0          | 83.8             | 89.8                | 83.8         | 86.0         |
| CritPT                      | 12.6         | 8.0           | 4.6              | 12.9                | 2.9          | 6.0          |
| Apex                        | 34.5         | 24.0          | 11.5             | 38.3                | 8.8          | 22.7         |
| General Capability          |              |               |                  |                     |              |              |
| MMLU-Pro                    | 89.7         | 87.1          | 86.3             | 87.5                | 88.5         | 88.5         |
| MMLU-Redux                  | 95.2         | 95.3          | 94.3             | 94.8                | 94.5         | 94.5         |
| SuperGPQA                   | 72.5         | 71.3          | 68.0             | 69.9                | 71.6         | 71.4         |
| IFEval                      | 91.9         | 94.5          | 94.5             | 91.9                | 94.3         | 94.6         |
| IFBench                     | 62.5         | 76.0          | 76.0             | 77.0                | 74.2         | 79.1         |
| MRCR-v2 128k                | 84.0         | 63.1          | 62.0             | 74.4                | 85.9         | 91.7         |
| Multilingualism             |              |               |                  |                     |              |              |
| WMT24++                     | 82.7         | 81.6          | 81.8             | 82.2                | 84.3         | 84.6         |
| MAXIFE                      | 81.3         | 87.7          | 87.7             | 88.9                | 88.2         | 88.8         |
| MMMLU                       | 90.6         | 87.5          | 87.2             | 87.9                | 89.5         | 89.0         |
| MMLU-ProX                   | 86.1         | 83.7          | 83.9             | 83.9                | 84.7         | 85.4         |
| NOVA-63                     | 59.1         | 56.7          | 54.6             | 52.8                | 57.9         | 58.8         |
| INCLUDE                     | 87.4         | 84.2          | 84.3             | 86.1                | 85.1         | 83.0         |
| Global PIQA                 | 91.2         | 89.2          | 89.5             | 90.5                | 89.8         | 90.3         |
| PolyMATH                    | 80.2         | 82.7          | 67.6             | 72.0                | 77.4         | 84.0         |

\* Terminal-Bench 2.0: Harbor/Terminus-2 harness; 5h timeout, 12 CPU/24 GB RAM; temp=1.0, top\_p=0.95, top\_k=20, max\_tokens=80K, 256K ctx; avg of 5 runs. All experiments prepend a \<think> token at each turn, allowing the model to decide whether to engage extended thinking.\
\* SWE-Bench Series: Internal agent scaffold (bash + file-edit tools); temp=1.0, top\_p=0.95, 200K context window.\
\* SWE-bench Pro: Problematic tasks corrected and all baselines evaluated on the refined benchmark.\
\* QwenClawBench: a real-user-distribution Claw agent benchmark; open-source: [https://github.com/SKYLENAGE-AI/QwenClawBench](https://github.com/SKYLENAGE-AI/QwenClawBench).\
\* CoWorkBench: an internal cowork benchmark; long-horizon tasks across computer science, finance, law, medical, and other productivity domains.\
\* SkillsBench: Evaluated via OpenCode on 78 tasks (excluding 9 external API-dependent tasks); avg of 5 runs.\
\* MCP-Mark: GitHub MCP v0.30.3; Playwright responses truncated at 32K tokens.\
\* MCP-Atlas: Public set score; gemini-2.5-pro judger.\
\* VITA-Bench: Avg subdomain scores; using claude-4.5-sonnet as judger, as the older official judgers are no longer available.\
\* Kernel Bench L3: Metrics reported: median of per-problem speedup over PyTorch eager reference / fraction of problems faster than torch.compile, across 50 problems. Each test sample runs in an isolated Docker container with one H100 80GB GPU, with internet access restricted to the CUTLASS codebase and official CUDA documentation, limited to 500 tool calls with early stopping after 100 non-improving turns. GPT-5.4 (xhigh) is applied to detect potential hacking behaviors. CUPTI is used for kernel-level timing.\
\* Reasoning scenarios: Recommended system prompt: "Reasoning effort is set to xhigh. Please think carefully through the task, validate key assumptions, consider plausible alternatives, and prioritize correctness, consistency, and clarity in the final answer."\
\* WMT24++: Harder WMT24 subset; avg scores on 55 langs via XCOMET-XXL.\
\* MAXIFE: Accuracy on EN + multilingual prompts (23 settings total).\
\* MMLU-ProX: Avg accuracy across 29 languages.\
\* Empty cells (--) indicate scores not yet available.

Qwen3.7-Plus delivers competitive text performance that approaches Max-tier models across the board. In **coding agents**, it performs strongly on Terminal Bench 2.0, SWE-bench series, and SciCode, handling both real-world software engineering and scientific programming tasks effectively. In **general-purpose agents**, it demonstrates robust tool-use and planning capabilities across MCP-Mark, Deep-Planning, and Kernel Bench L3, showing particular strength in complex multi-step planning and GPU kernel optimization. Its **reasoning** performance on GPQA Diamond, HMMT, and IMOAnswerBench places it among the strongest Plus-tier models on hard STEM benchmarks. In **instruction following and multilingual tasks**, it delivers consistent quality across IFBench, WMT24++, and PolyMATH, with strong coverage across diverse languages.

### Multimodal Benchmarks [#](https://qwen.ai/blog?id=qwen3.7-plus#multimodal-benchmarks)

|                                  | GPT-5.4 (xhigh) | Opus-4.6 Max | Gemini-3.1 Pro | Qwen3.6-Plus | Qwen3.7-Plus |
| -------------------------------- | --------------- | ------------ | -------------- | ------------ | ------------ |
| Multimodal Reasoning             |                 |              |                |              |              |
| MMMU-Pro                         | 81.2            | 73.9         | 81.8           | 78.8         | 79.0         |
| MathVision                       | 91.0            | 65.5         | 87.4           | 88.0         | 90.3         |
| BabyVision                       | 53.1            | 12.6         | 55.9           | 37.4         | 70.4 / 64.7  |
| CharXiv(RQ)                      | 84.5            | 66.0         | 84.4           | 81.5         | 85.9 / 84.4  |
| HiPhO                            | 65.0            | 40.8         | 85.4           | 80.4         | 84.1         |
| ERQA                             | 67.8            | 40.8         | 68.0           | 65.7         | 69.8         |
| VisFactor                        | 40.8            | 24.4         | 39.8           | 36.0         | 42.8         |
| MedXpertQA-MM                    | 77.3            | 64.4         | 80.7           | 68.7         | 71.0         |
| Visual Agent & Coding            |                 |              |                |              |              |
| ScreenSpot Pro                   | 67.4            | 49.5         | 68.1           | 68.2         | 79.0         |
| OSWorld-Verified                 | 75.0            | 72.7         | --             | 62.5         | 73.3         |
| AndroidWorld                     | --              | 62.0         | 70.7           | 67.2         | 81.0         |
| QwenVision2Code                  | 1884.0          | 1518.0       | 1632.0         | 1522.0       | 1772.0       |
| ClawEval-MM                      | 54.4            | 54.7         | 45.7           | 49.1         | 55.7         |
| Multimodal Search & Knowledge QA |                 |              |                |              |              |
| SimpleVQA                        | 69.4            | 79.6         | 76.9           | 69.4         | 81.7         |
| WorldVQA                         | 45.9            | 65.4         | 56.1           | 33.6         | 61.1         |
| MMSearchPlus                     | 19.7            | 38.9         | 42.0           | 19.6         | 41.4         |
| BC-VL                            | 48.1            | 51.5         | 49.9           | 26.1         | 51.1         |
| MMBC                             | 18.8            | 46.3         | 28.2           | 18.3         | 46.3         |
| General Visual Understanding     |                 |              |                |              |              |
| RealWorldQA                      | 83.8            | 73.9         | 83.5           | 85.4         | 86.9         |
| CountQA                          | 58.4            | 32.5         | 72.8           | 71.7         | 77.0         |
| OmniDocBench1.5                  | 85.5            | 86.6         | 90.0           | 91.2         | 91.4         |
| OCR-Bench-V2(EN)                 | 59.1            | 54.3         | 64.6           | 67.0         | 70.7         |
| OCR-Bench-V2(ZH)                 | 57.7            | 54.9         | 58.2           | 63.6         | 67.1         |
| ODinW13                          | --              | --           | --             | 51.8         | 51.1         |
| Autonomous Driving               |                 |              |                |              |              |
| LingoQA                          | 78.2            | 77.6         | 66.8           | 76.0         | 83.4         |
| Ego3D-Bench↓                     | 6.9             | 8.1          | 10.4           | 6.1          | 5.9          |
| SURDS                            | 64.6            | 58.3         | 64.0           | 73.2         | 77.2         |
| VLADBench                        | 77.1            | 48.0         | 73.1           | 75.6         | 77.2         |
| Video Understanding              |                 |              |                |              |              |
| VideoMME (w/ sub.)               | 89.5            | 86.1         | 88.4           | 87.8         | 88.0         |
| VideoMMMU                        | 82.4            | 85.2         | 85.3           | 84.0         | 85.4         |
| MLVU (M-Avg)                     | 86.1            | 81.7         | 84.7           | 86.7         | 87.4         |
| TVBench                          | 82.5            | 69.8         | 73.0           | 76.0         | 78.2         |
| LVBench                          | 77.4            | 63.0         | 75.1           | 74.8         | 76.2         |

\* Multimodal Search & Knowledge QA: All models evaluated with search augmentation enabled.\
\* BabyVision and CharXiv(RQ): Scores are reported as "with CI / without CI".\
\* VideoMME (w/ sub.): Scores are reported with subtitles.\
\* BC-VL and MMBC: Scores are reported with the recommended presence penalty 1.5 in BC tasks.\
\* ScreenSpot Pro and OSWorld-Verified: Scores are reported with "enable\_thinking=False".\
\* Empty cells (--) indicate the scores are not yet available.

Qwen3.7-Plus’s multimodal improvements are not limited to isolated gains in visual understanding. Instead, they reflect a systematic enhancement of the core capabilities required by multimodal agents: **understanding complex visual inputs, reasoning over visual information, using tools to solve problems, and ultimately executing tasks in code or GUI environments**.

In **Multimodal Reasoning**, Qwen3.7-Plus delivers strong performance on challenging visual reasoning benchmarks such as BabyVision, MathVision, HiPhO, ERQA, and VisFactor. These results demonstrate the model’s ability to integrate fine-grained visual perception, spatial relationships, physical commonsense, and multi-step logical reasoning. In particular, its significant improvement on BabyVision over Qwen3.6-Plus suggests stronger generalization on tasks that are closer to early human visual cognition and spatial reasoning.

In **Visual Agent & Coding**, Qwen3.7-Plus shows substantial gains on ScreenSpot Pro, OSWorld-Verified, and AndroidWorld. This indicates that the model can not only recognize screen content, but also localize key UI elements, understand task intent, and complete multi-step interactions. On QwenVision2Code, the model also demonstrates strong vision-to-code generation capabilities, turning images, videos, and design references into executable code. These capabilities form the foundation for multimodal agents to move from “understanding interfaces” to “operating interfaces” and even “building interfaces.”

In **Multimodal Search & Knowledge QA**, Qwen3.7-Plus achieves clear improvements on SimpleVQA, WorldVQA, MMSearchPlus, BC-VL, and MMBC. The model can combine visual inputs with external knowledge retrieval to answer questions that cannot be solved from image content alone. This makes it better suited for real-world tasks, where users do not simply ask “what is in the image,” but expect the model to combine visual evidence, commonsense, and up-to-date knowledge to provide reliable answers.

In **General Visual Understanding**, Qwen3.7-Plus maintains strong performance across real-world scenes, document parsing, chart understanding, OCR, counting, and spatial localization. It performs strongly on tasks such as RealWorldQA, CountQA, OmniDocBench, CharXiv, and OCR-Bench-V2. These capabilities are essential for robustly handling real business inputs, including screenshots, receipts, tables, reports, posters, product images, and complex UI pages.

Beyond images, Qwen3.7-Plus further strengthens **video understanding and driving-scene understanding**. On video benchmarks such as VideoMMMU, MLVU, TVBench, and LVBench, it can reason over events, actions, temporal dynamics, and semantic relationships in both short and long videos. On driving-related evaluations such as LingoQA, Ego3D-Bench, SURDS, and VLADBench, it also demonstrates strong understanding of dynamic scenes, traffic participants, and spatial relationships. These capabilities lay an important foundation for real-world multimodal agents, autonomous driving understanding, and embodied AI scenarios.

## Build with Qwen3.7-Plus [#](https://qwen.ai/blog?id=qwen3.7-plus#build-with-qwen37-plus)

Qwen3.7-Plus is now available through [Alibaba Cloud Model Studio](https://modelstudio.alibabacloud.com/).

### API Usage [#](https://qwen.ai/blog?id=qwen3.7-plus#api-usage)

As a multimodal model, Qwen3.7-Plus accepts both text and image/video inputs. It also supports the `preserve_thinking` feature: preserving thinking content from all preceding turns in messages, which is **recommended for agentic tasks**.

#### Alibaba Cloud Model Studio [#](https://qwen.ai/blog?id=qwen3.7-plus#alibaba-cloud-model-studio)

Alibaba Cloud Model Studio supports industry-standard protocols, including chat completions and responses APIs compatible with OpenAI’s specification.

```python
"""
Environment variables:
  DASHSCOPE_API_KEY: Your API Key from https://modelstudio.console.alibabacloud.com
  DASHSCOPE_BASE_URL: (optional) Base URL for compatible-mode API.
    - Beijing: https://dashscope.aliyuncs.com/compatible-mode/v1
    - Singapore: https://dashscope-intl.aliyuncs.com/compatible-mode/v1
    - US (Virginia): https://dashscope-us.aliyuncs.com/compatible-mode/v1
"""
from openai import OpenAI
import os

api_key = os.environ.get("DASHSCOPE_API_KEY")
if not api_key:
    raise ValueError(
        "DASHSCOPE_API_KEY is required. "
        "Set it via: export DASHSCOPE_API_KEY='your-api-key'"
    )

client = OpenAI(
    api_key=api_key,
    base_url=os.environ.get(
        "DASHSCOPE_BASE_URL",
        "https://dashscope-intl.aliyuncs.com/compatible-mode/v1",
    ),
)

messages = [{"role": "user", "content": "Write a Python function to merge two sorted linked lists."}]

completion = client.chat.completions.create(
    model="qwen3.7-plus",
    messages=messages,
    extra_body={
        "enable_thinking": True,
        # "preserve_thinking": True,
    },
    stream=True
)

reasoning_content = ""
answer_content = ""
is_answering = False
print("\n" + "=" * 20 + "Reasoning" + "=" * 20 + "\n")

for chunk in completion:
    if not chunk.choices:
        print("\nUsage:")
        print(chunk.usage)
        continue

    delta = chunk.choices[0].delta

    if hasattr(delta, "reasoning_content") and delta.reasoning_content is not None:
        if not is_answering:
            print(delta.reasoning_content, end="", flush=True)
        reasoning_content += delta.reasoning_content

    if hasattr(delta, "content") and delta.content:
        if not is_answering:
            print("\n" + "=" * 20 + "Answer" + "=" * 20 + "\n")
            is_answering = True
        print(delta.content, end="", flush=True)
        answer_content += delta.content
```

For more information, please visit the [API doc](https://modelstudio.console.alibabacloud.com/?tab=doc#/doc/?type=model&url=2840915).

### Multimodal Interactive Hybrid Agent [#](https://qwen.ai/blog?id=qwen3.7-plus#multimodal-interactive-hybrid-agent)

Qwen3.7-Plus features multimodal hybrid-agent capabilities designed for closed-loop execution of real-world tasks. It can not only understand visual interfaces, perceive on-screen content, and perform both GUI interactions and CLI operations, but also leverage environmental feedback for code generation, application manipulation, testing, validation, and iterative optimization. By integrating the full workflow of “see, think, write, act, and verify” into a unified agent loop, it enables end-to-end automation of complex software tasks from initial understanding to final delivery.

We built the Hybrid-Agent intelligent agent system based on Qwen3.7, deeply integrating the code generation capabilities of large language models with GUI automation execution, achieving full-chain APP development from requirement analysis to version iteration. The Agent operated continuously and stably for over 11 hours, fully automating the complete R&D cycle of an English vocabulary learning APP. It generated more than 10,000+ lines of code, triggered over 1,000+ Agent calls, and covered core stages across the entire software development lifecycle: requirement document generation, automated coding, installation and deployment, test case creation, GUI-based automated testing, multi-scenario parallelized testing, automatic product documentation updates, and autonomous version evolution.

For professional desktop application scenarios, the Hybrid-Agent system deeply integrates the model’s GUI perception and code generation capabilities to enable one-click autonomous replication of professional desktop applications. The Agent autonomously completed a high-fidelity recreation of the native macOS Stocks app, covering the full pipeline from requirement understanding to delivery validation: autonomously interacting with the native app to comprehend UI layout and feature details, generating SwiftUI source code from interaction records, integrating with the LongBridge real-world market API for live data, automatically compiling and launching the recreated app, and finally conducting 10 functional verification tests autonomously – including real-time quote loading, stock selection and switching, multi-period view toggling, search filtering, and detailed stats panel display – all passed. The delivered application faithfully reproduces the native Stocks app’s dark theme, split-view layout, real-time market data, and full interactivity.

### Visual Agent [#](https://qwen.ai/blog?id=qwen3.7-plus#visual-agent)

Qwen3.7-Plus can serve as a powerful visual agent, combining visual understanding with tool use to solve complex visual tasks. Through integration with a code interpreter, it can analyze images to spot differences, complete missing puzzle pieces, solve sliding-block puzzles, navigate mazes, and assemble jigsaw puzzles—all by autonomously generating and executing code. With search augmentation, it can also leverage web knowledge to reason over real-world visual questions and provide multimodal answers across single-image, multi-image, and video inputs.

Below, we showcase several examples that demonstrate the multimodal agent capabilities of Qwen3.7-Plus.

#### Multimodal Reasoning [#](https://qwen.ai/blog?id=qwen3.7-plus#multimodal-reasoning)

For multimodal reasoning, we introduce code execution to further enhance the model’s problem-solving ability. The model first understands the structure and constraints in the visual input, then transforms the visual task into a computable representation, and finally writes and executes code to solve, search, or verify the answer.

In tasks such as spot-the-difference, missing-block completion, sliding-block puzzles, mazes, and jigsaw puzzles, the model needs to go beyond recognizing visual content. It must also perform spatial modeling, path search, state simulation, and result verification. These examples highlight Qwen3.7-Plus’s ability to move from visual perception to programmatic problem solving.

Find the differences Next

Qwen3.7

Fill with patches Next

Qwen3.7

Klotski Next

Qwen3.7

Maze simulator Next

Qwen3.7

Jigsaw Next

Qwen3.7

#### Multimodal Search [#](https://qwen.ai/blog?id=qwen3.7-plus#multimodal-search)

In search-augmented visual question answering, Qwen3.7-Plus can combine image, video, or multi-image inputs with web search to answer real-world knowledge questions. The model first extracts key entities, scenes, text, and contextual clues from the visual input, then retrieves external knowledge through search, and finally synthesizes visual evidence with retrieved information to produce the answer.

This enables the model to handle a wide range of open-world questions, such as identifying locations, understanding the background of events, analyzing products or objects, and answering visual questions that depend on up-to-date knowledge.

Realworld VQA Next

Qwen3.7

Realworld VQA Next

Qwen3.7

Multi-hop Multimodal Browsing Agent Next

Qwen3.7

video search simplevqa Next

Qwen3.7

### Visual Coding [#](https://qwen.ai/blog?id=qwen3.7-plus#visual-coding)

Qwen3.7-Plus demonstrates strong vision-to-code generation capabilities. It can transform images, videos, UI screenshots, and design references into executable code, covering a broad range of scenarios from SVG reconstruction to full webpage generation.

#### Image/Video to SVG [#](https://qwen.ai/blog?id=qwen3.7-plus#imagevideo-to-svg)

In image/video-to-SVG tasks, the model needs to understand geometric structures, colors, layouts, hierarchical relationships, and dynamic changes in visual content, and then express these elements precisely in code. This requires not only visual understanding, but also structured representation and code generation.

For icons, illustrations, animations, graphic design, and information visualization, this capability can significantly reduce the cost of turning visual references into editable code assets.

vision to svg Next

User

![image](https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen3.7/demo/svg/raw_1.png) Please generate svg code according to the image.

Qwen3.7

vision to svg Next

User

![image](https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen3.7/demo/svg/raw_2.png) Please generate svg code according to the image.

Qwen3.7

vision to svg Next

User

Please generate svg code according to the video.

User

Qwen3.7

vision to svg Next

User

Please generate svg code according to the video.

User

Qwen3.7

vision to svg Next

User

![image](https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen3.7/demo/svg/raw_5.png) Please generate svg code according to the image.

Qwen3.7

#### Vision-Driven Web Design [#](https://qwen.ai/blog?id=qwen3.7-plus#vision-driven-web-design)

In vision-driven web design, Qwen3.7-Plus can generate complete interactive webpages based on visual references, video materials, or design intent. The model can also use generation tools to produce assets for webpage design.

It not only reproduces the visual style of a reference page, but also organizes layout, writes frontend code, handles interaction logic, and integrates multimodal assets into the final page. This demonstrates the potential of Qwen3.7-Plus as a visual coding assistant: moving from “given a reference image” to “generate a runnable web prototype.”

Web Design with Video-Generation Next

Qwen3.7

Web Design with Video-Generation Next

Qwen3.7

Web Design with Video-Generation Next

Qwen3.7

### Browser Agent [#](https://qwen.ai/blog?id=qwen3.7-plus#browser-agent)

Built on Qwen3.7-Plus, the browser Agent is demonstrated and recorded through Qwen for Chrome, a browser extension embedded in Chrome. Users can interact with Qwen directly from the browser sidebar and, with authorization, switch it into Agent mode. In this mode, Qwen can perceive the current webpage, understand the user’s task, plan the next steps, and operate as a Browser Agent to perform clicks, typing, navigation, configuration, and verification directly in the real browser environment.

With this setup, the Qwen3.7 browser Agent integrates page understanding, task planning, and GUI automation to operate inside real web-based work environments. Given a non-technical user’s request to purchase the cheapest ECS server, the Agent can navigate the cloud console, compare instance options, select a low-cost configuration, set up images, storage, security groups, and order details, while dynamically adjusting its strategy when prices change, inventory is limited, or purchase constraints arise. In the follow-up task, the Agent further handles instance scaling and maintenance, completing shutdown, configuration updates, disk expansion, service recovery, and final verification. This scenario covers the real cloud workflow from server purchase to upgrade, turning a complex console-based process into a continuous, efficient, and deliverable browser automation task.

### Real-world Perception & Reasoning [#](https://qwen.ai/blog?id=qwen3.7-plus#real-world-perception--reasoning)

Qwen3.7-Plus also shows strong performance in real-world perception and multimodal reasoning. Real-world scenes are often much more complex than standard visual question answering. They may involve occlusion, cluttered backgrounds, small objects, relationships among multiple entities, cross-image comparison, and implicit physical commonsense.

To answer these questions reliably, the model must first identify visual details robustly, then combine them with spatial relationships, commonsense knowledge, and logical reasoning.

realworld counting Next

Qwen3.7

multi-image reasoning Next

Qwen3.7

puzzle Next

Qwen3.7

Grounding Next

Qwen3.7

### Coding Assistants [#](https://qwen.ai/blog?id=qwen3.7-plus#coding-assistants)

Qwen3.7-Plus integrates seamlessly with popular agent frameworks and coding assistants:

#### Claude Code [#](https://qwen.ai/blog?id=qwen3.7-plus#claude-code)

Qwen APIs support the Anthropic API protocol, enabling direct use with **Claude Code**:

```bash
npm install -g @anthropic-ai/claude-code

export ANTHROPIC_MODEL="qwen3.7-plus"
export ANTHROPIC_SMALL_FAST_MODEL="qwen3.7-plus"
export ANTHROPIC_BASE_URL=https://dashscope-intl.aliyuncs.com/apps/anthropic
export ANTHROPIC_AUTH_TOKEN=<your_api_key>

claude
```

#### OpenClaw [#](https://qwen.ai/blog?id=qwen3.7-plus#openclaw)

Connect to [OpenClaw](https://openclaw.ai) via [Model Studio](https://www.alibabacloud.com/help/en/model-studio/openclaw):

```bash
curl -fsSL https://molt.bot/install.sh | bash
export DASHSCOPE_API_KEY=<your_api_key>
openclaw dashboard
```

Configure `~/.openclaw/openclaw.json`:

```json
{
  "models": {
    "mode": "merge",
    "providers": {
      "modelstudio": {
        "baseUrl": "https://dashscope-intl.aliyuncs.com/compatible-mode/v1",
        "apiKey": "DASHSCOPE_API_KEY",
        "api": "openai-completions",
        "models": [
          {
            "id": "qwen3.7-plus",
            "name": "qwen3.7-plus",
            "reasoning": true,
            "input": ["text"],
            "contextWindow": 1000000,
            "maxTokens": 65536
          }
        ]
      }
    }
  },
  "agents": {
    "defaults": {
      "model": {
        "primary": "modelstudio/qwen3.7-plus"
      }
    }
  }
}
```

#### Qwen Code [#](https://qwen.ai/blog?id=qwen3.7-plus#qwen-code)

[Qwen Code](https://qwen.ai/qwencode) is deeply optimized for the Qwen series:

```bash
npm install -g @qwen-code/qwen-code@latest
qwen
```

## Summary [#](https://qwen.ai/blog?id=qwen3.7-plus#summary)

Qwen3.7-Plus is our most capable multimodal agent model, unifying vision understanding and language reasoning into a versatile agent foundation. It operates as a multimodal interactive hybrid agent — perceiving real-world scenes, operating graphical interfaces, writing code from visual references, and completing end-to-end tasks across both GUI and CLI environments. As a versatile coding agent and productivity assistant, it handles the full range of tasks from frontend prototyping to complex software engineering and multi-step workflow automation. It generalizes across agent scaffolds, performing consistently whether deployed through Claude Code, OpenClaw, Qwen Code, or other frameworks. We welcome community feedback and look forward to seeing what you build.

## Citation [#](https://qwen.ai/blog?id=qwen3.7-plus#citation)

```bibtex
@misc{qwen37plus,
    title = {{Qwen3.7-Plus}: Multimodal Agent Intelligence},
    url = {https://qwen.ai/blog?id=qwen3.7-plus},
    author = {{Qwen Team}},
    month = {May},
    year = {2026}
}
```
