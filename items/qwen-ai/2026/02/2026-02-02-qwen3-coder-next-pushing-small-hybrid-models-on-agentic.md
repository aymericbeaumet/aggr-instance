---
title: 'Qwen3-Coder-Next: Pushing Small Hybrid Models on Agentic Coding'
link: https://qwen.ai/blog?id=qwen3-coder-next
source: qwen-ai
published: 2026-02-02T20:00:00Z
first_seen: 2026-10-18T23:27:08.867657739Z
authors:
- QwenTeam
labels:
- open-source
content: feed
html: 2026-02-02-qwen3-coder-next-pushing-small-hybrid-models-on-agentic.html
remote_preview:
  url: https://img.alicdn.com/imgextra/i1/O1CN01LwK7vH21HjjuHvHWQ_!!6000000006960-2-tps-1590-954.png
---

## [Tech Report](https://github.com/QwenLM/Qwen3-Coder/blob/main/qwen3_coder_next_tech_report.pdf) [GitHub](https://github.com/QwenLM/Qwen3-Coder) [Hugging Face](https://huggingface.co/collections/Qwen/qwen3-coder-next) [ModelScope](https://modelscope.cn/collections/Qwen/Qwen3-Coder-Next) [DISCORD](https://discord.gg/yPEP2vHTu4)

## Introduction

We introduce **Qwen3-Coder-Next**, an open-weight language model designed specifically for coding agents and local development. Built on top of **Qwen3-Next-80B-A3B-Base**, which adopts a novel architecture with hybrid attention and MoE, Qwen3-Coder-Next has been agentically trained at scale on large-scale executable task synthesis, environment interaction, and reinforcement learning, obtaining strong coding and agentic capabilities with significantly lower inference costs.

## Scaling Agentic Training

Rather than relying solely on parameter scaling, Qwen3-Coder-Next focuses on **scaling agentic training signals**. We train the model using large collections of *verifiable coding tasks* paired with executable environments, enabling the model to learn directly from environment feedback. This includes:

- Continued pretraining on code- and agent-centric data
- Supervised fine-tuning on data containing high-quality agent trajectories
- Domain-specialized expert training (e.g., software engineering, QA, web/UX)
- Expert distillation into a single deployment-ready model

This recipe emphasizes **long-horizon reasoning, tool usage, and recovery from execution failures**, which are essential for real-world coding agents.

## Performance on Coding Agent Benchmarks

### Agent-Centric Benchmark Results

The figure below summarizes performance across several widely used **coding agent benchmarks**, including SWE-Bench (Verified, Multilingual, and Pro), TerminalBench 2.0, and Aider.

![Qwen3 Coder Next Benchmarks](https://qianwen-res.oss-accelerate-overseas.aliyuncs.com/Qwen3-Coder-Next/benchmarks.png)

The figure demonstrates that:

- Qwen3-Coder-Next achieves **over 70% on SWE-Bench Verified** using the SWE-Agent scaffold.
- Performance remains competitive across **multilingual settings** and the more challenging **SWE-Bench Pro** benchmark.
- Despite its small active footprint, the model matches or exceeds several much larger open-source models across agent-centric evaluations.

As shown in the figure below, Our model achieves strong results on SWE-Bench Pro by scaling the number of agent turns, providing evidence that the model excels at long-horizon reasoning in multi-turn agentic tasks.

![SWE-Bench Pro turns](https://qianwen-res.oss-accelerate-overseas.aliyuncs.com/Qwen3-Coder-Next/swebench_pro_vs_turns.png)

### Efficiency–Performance Tradeoff

This figure highlights how Qwen3-Coder-Next achieves an improved Pareto tradeoff between efficiency and performance.

![Qwen3 Coder Next Main Image](https://qianwen-res.oss-accelerate-overseas.aliyuncs.com/Qwen3-Coder-Next/swebench_pro.png)

This comparison makes the efficiency story clear:

- **Qwen3-Coder-Next (3B active)** achieves SWE-Bench-Pro performance comparable to models with **10×–20× more active parameters**.
- Qwen3-Coder-Next sits on a strong Pareto frontier for **cost-effective agent deployment**.

## Demo

The small and fast coder model can be integrated into different downstream applications, and below we demonstrate showcases of OpenClaw, Qwen Code, Claude Code, Web dev, browser use, Cline, etc.

### Web Dev

Creating a Chat Interface Next

Create an Interactive Game Next

Building an Interactive ASCII Art Drawing Tool Next

### CLI

Desktop Cleanup Next

Implementing a Web Game: Zombies vs. Plants Next

### Cline

Creating a Multicolor Animation Next

Building an Interactive Sound Art Tool Next

### OpenClaw

Web Page for Qwen3 Coder Next Next

### Browser Use Agent

Searching for a Product on Amazon Next

Testing a Website Next

### coder.qwen.ai

## Summary and Future Work

Qwen3-Coder-Next shows promising results on coding agent benchmarks, providing good speed and reasoning abilities for practical use. While it performs competitively—even compared to some larger open-source models—there is still much room for improvement.

Looking ahead, we believe strong agent skills—like using tools by itself, handling tough problems, and managing complex tasks—are key for better coding agents. Next, we plan to improve the model’s reasoning and decision-making, support more tasks, and update quickly based on how people use it.

## Citation

If you find our work helpful feel free to give us a cite.

```
@techreport{qwen_qwen3_coder_next_tech_report,
  title        = {Qwen3-Coder-Next Technical Report},
  author       = {{Qwen Team}},
  url          = {https://github.com/QwenLM/Qwen3-Coder/blob/main/qwen3_coder_next_tech_report.pdf},
  note         = {Accessed: 2026-02-03}
}
```
