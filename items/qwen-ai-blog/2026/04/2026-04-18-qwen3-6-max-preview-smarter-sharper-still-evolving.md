---
title: 'Qwen3.6-Max-Preview: Smarter, Sharper, Still Evolving'
link: https://qwen.ai/blog?id=qwen3.6-max-preview
source: qwen-ai-blog
published: 2026-04-18T02:00:00Z
first_seen: 2026-09-08T10:17:10.166315Z
authors:
- QwenTeam
labels:
- release
content: feed
html: 2026-04-18-qwen3-6-max-preview-smarter-sharper-still-evolving.html
preview:
  file: 2026-04-18-qwen3-6-max-preview-smarter-sharper-still-evolving.preview-668ecb0a0845.webp
  width: 256
  height: 154
  color: '#adb4fe'
images:
- source: https://img.alicdn.com/imgextra/i4/O1CN01G7Tcjx1TKJ2SRtYll_!!6000000002363-2-tps-1590-954.png
  original:
    file: 2026-04-18-qwen3-6-max-preview-smarter-sharper-still-evolving.image-5f8b6523ae61.png
    width: 1590
    height: 954
  variants:
  - file: 2026-04-18-qwen3-6-max-preview-smarter-sharper-still-evolving.image-63495b40c79b.webp
    width: 48
    height: 29
  color: '#8ba8ff'
- source: https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen3.6/Figures/3.6_max_preview_banner.png
  original:
    file: 2026-04-18-qwen3-6-max-preview-smarter-sharper-still-evolving.image-bdae75f09092.png
    width: 1920
    height: 1080
  variants:
  - file: 2026-04-18-qwen3-6-max-preview-smarter-sharper-still-evolving.image-960c7cd823a7.webp
    width: 48
    height: 27
  - file: 2026-04-18-qwen3-6-max-preview-smarter-sharper-still-evolving.image-10f1d9d08a5b.webp
    width: 320
    height: 180
  - file: 2026-04-18-qwen3-6-max-preview-smarter-sharper-still-evolving.image-a317e3a8c25f.webp
    width: 640
    height: 360
  - file: 2026-04-18-qwen3-6-max-preview-smarter-sharper-still-evolving.image-f8340afe261b.webp
    width: 960
    height: 540
  - file: 2026-04-18-qwen3-6-max-preview-smarter-sharper-still-evolving.image-824b110fe297.webp
    width: 1280
    height: 720
  - file: 2026-04-18-qwen3-6-max-preview-smarter-sharper-still-evolving.image-41657f4446c8.webp
    width: 1600
    height: 900
  - file: 2026-04-18-qwen3-6-max-preview-smarter-sharper-still-evolving.image-831b29d18b24.webp
    width: 1920
    height: 1080
  color: '#e7e7f9'
---

![Qwen3.6-Max-Preview Main Image](https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen3.6/Figures/3.6_max_preview_banner.png)

[QWEN STUDIO](https://chat.qwen.ai) [DISCORD](https://discord.gg/yPEP2vHTu4)

Following the release of [Qwen3.6-Plus](https://qwen.ai/blog?id=qwen3.6), we are sharing an early preview of our next proprietary model: **Qwen3.6-Max-Preview**. Compared to Qwen3.6-Plus, this preview release brings **stronger world knowledge and instruction following**, along with **significant agentic coding improvements** across a wide range of benchmarks. As a preview, the model is still under active development — we are continuing to iterate and expect further gains in subsequent versions.

- **Qwen3.6-Max-Preview** is the hosted proprietary model available via [Alibaba Cloud Model Studio](https://modelstudio.alibabacloud.com/), featuring:
  - improved agentic coding capability over Qwen3.6-Plus
  - stronger world knowledge and instruction following
  - improved real-world agent and knowledge reliability performance
- You can chat interactively on [Qwen Studio](https://chat.qwen.ai) or call via API as `qwen3.6-max-preview` on [Alibaba Cloud Model Studio API](https://modelstudio.alibabacloud.com/) (coming soon).

## Performance [#](https://qwen.ai/blog?id=qwen3.6-max-preview#performance)

Below we present evaluations of Qwen3.6-Max-Preview against leading frontier models. Compared to Qwen3.6-Plus, the preview release delivers significant improvements in agentic coding (e.g., SkillsBench +9.9, SciCode +6.3, NL2Repo +5.0, Terminal-Bench 2.0 +3.8), stronger world knowledge (SuperGPQA +2.3, QwenChineseBench +5.3), and better instruction following (ToolcallFormatIFBench +2.8).

![](https://qianwen-res.oss-cn-beijing.aliyuncs.com/Qwen3.6/Figures/qwen3.6_max_preview_score.png)

## Build with Qwen3.6-Max-Preview [#](https://qwen.ai/blog?id=qwen3.6-max-preview#build-with-qwen36-max-preview)

Qwen3.6-Max-Preview is coming soon to Alibaba Cloud Model Studio. Please stand by until we are fully ready.

Qwen3.6-Max-Preview is available through the [Alibaba Cloud Model Studio](https://modelstudio.alibabacloud.com/) API as `qwen3.6-max-preview`. You can also try it instantly on [Qwen Studio](https://chat.qwen.ai).

### API Usage [#](https://qwen.ai/blog?id=qwen3.6-max-preview#api-usage)

This release supports the `preserve_thinking` feature: preserving thinking content from all preceding turns in messages, which is **recommended for agentic tasks**.

#### Alibaba Cloud Model Studio [#](https://qwen.ai/blog?id=qwen3.6-max-preview#alibaba-cloud-model-studio)

Alibaba Cloud Model Studio supports industry-standard protocols, including chat completions and responses APIs compatible with OpenAI’s specification, as well as an API interface compatible with Anthropic.

Example code for chat completions API is provided below:

```python
"""
Environment variables (per official docs):
  DASHSCOPE_API_KEY: Your API Key from https://modelstudio.console.alibabacloud.com
  DASHSCOPE_BASE_URL: (optional) Base URL for compatible-mode API.
    - Beijing: https://dashscope.aliyuncs.com/compatible-mode/v1
    - Singapore: https://dashscope-intl.aliyuncs.com/compatible-mode/v1
    - US (Virginia): https://dashscope-us.aliyuncs.com/compatible-mode/v1
  DASHSCOPE_MODEL: (optional) Model name; override for different models.
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

messages = [{"role": "user", "content": "Introduce vibe coding."}]

model = os.environ.get(
    "DASHSCOPE_MODEL",
    "qwen3.6-max-preview",
)
completion = client.chat.completions.create(
    model=model,
    messages=messages,
    extra_body={
        "enable_thinking": True,
        # "preserve_thinking": True,
    },
    stream=True
)

reasoning_content = ""  # Full reasoning trace
answer_content = ""  # Full response
is_answering = False  # Whether we have entered the answer phase
print("\n" + "=" * 20 + "Reasoning" + "=" * 20 + "\n")

for chunk in completion:
    if not chunk.choices:
        print("\nUsage:")
        print(chunk.usage)
        continue

    delta = chunk.choices[0].delta

    # Collect reasoning content only
    if hasattr(delta, "reasoning_content") and delta.reasoning_content is not None:
        if not is_answering:
            print(delta.reasoning_content, end="", flush=True)
        reasoning_content += delta.reasoning_content

    # Received content, start answer phase
    if hasattr(delta, "content") and delta.content:
        if not is_answering:
            print("\n" + "=" * 20 + "Answer" + "=" * 20 + "\n")
            is_answering = True
        print(delta.content, end="", flush=True)
        answer_content += delta.content
```

For more information, please visit the [API doc](https://modelstudio.console.alibabacloud.com/?tab=doc#/doc/?type=model&url=2840915).

## Summary [#](https://qwen.ai/blog?id=qwen3.6-max-preview#summary)

Qwen3.6-Max-Preview is an early preview of our next proprietary model, delivering meaningful improvements over Qwen3.6-Plus in agentic coding, world knowledge, and instruction following. It achieves the top score on six major coding benchmarks — SWE-bench Pro, Terminal-Bench 2.0, SkillsBench, QwenClawBench, QwenWebBench, and SciCode — with substantial gains over its predecessor. It also demonstrates stronger knowledge (SuperGPQA, QwenChineseBench) and better instruction following (ToolcallFormatIFBench).

As a preview release, Qwen3.6-Max-Preview is still under active development. We are continuing to iterate on the model and expect further improvements in subsequent versions. We welcome community feedback and look forward to seeing what you build. Stay tuned!

## Citation [#](https://qwen.ai/blog?id=qwen3.6-max-preview#citation)

Feel free to cite the following article if you find Qwen3.6-Max-Preview helpful:

```bibtex
@misc{qwen36_max_preview,
    title = {{Qwen3.6-Max-Preview}: Smarter, Sharper, Still Evolving},
    url = {https://qwen.ai/blog?id=qwen3.6-max-preview},
    author = {{Qwen Team}},
    month = {April},
    year = {2026}
}
```
