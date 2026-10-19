---
title: NVIDIA Releases 6 Million Multi-Lingual Reasoning Dataset
link: https://huggingface.co/blog/nvidia/multilingual-reasoning-v1
source: huggingface-co
published: 2025-08-20T22:13:18Z
updated: 2025-08-20T22:13:18Z
first_seen: 2026-10-04T12:50:30.804479567Z
content: extracted
html: 2025-08-20-nvidia-releases-6-million-multi-lingual-reasoning-dataset.html
---

Authors: Dhruv Nathawani, Shuoyang Ding US, Vitaly Lavrukhin US, Jane Polak Scowcroft US, Oleksii Kuchaiev US

NVIDIA continues releasing permissive datasets in support of the open ecosystem with [6 Million Multilingual Reasoning Dataset](https://huggingface.co/datasets/nvidia/Nemotron-Post-Training-Dataset-v2).

Continuing the success of the recent [Nemotron Post-Training Dataset v1](https://huggingface.co/datasets/nvidia/Nemotron-Post-Training-Dataset-v1) release used in [Llama Nemotron Super model](https://developer.nvidia.com/blog/build-more-accurate-and-efficient-ai-agents-with-the-new-nvidia-llama-nemotron-super-v1-5/), and our [Llama Nemotron Post-Training Dataset](https://huggingface.co/datasets/nvidia/Llama-Nemotron-Post-Training-Dataset) release earlier this year, we’re excited to release the reasoning dataset translated into five target languages: French, Spanish, German, Italian, and Japanese.

The newly released [NVIDIA Nemotron Nano 2 9B](https://huggingface.co/nvidia/NVIDIA-Nemotron-Nano-9B-v2) brings these capabilities to the edge with leading accuracy and efficiency with a hybrid Transformer–Mamba architecture and a configurable thinking budget—so you can dial accuracy, throughput, and cost to match your real‑world needs.

## Model Highlights (TL;DR)

- **Model size**: 9B parameters
- **Architecture**: Hybrid Transformer–Mamba (Mamba‑2 + a small number of attention layers) for higher throughput at similar accuracy to Transformer‑only peers
- **Throughput**: Up to 6× higher token generation than other leading models in its size class
- **Cost**: Thinking budget lets you control how many “thinking” tokens are used—saving up to 60% lower reasoning costs
- **Target**: Agents for customer service, support chatbots, analytics copilots, and edge/RTX deployments
- **Availability**: The model weights are available on Hugging Face, you can try the endpoint on build.nvidia.com, and the model will be available as NVIDIA NIM for high throughput and low latency
- **License**: nvidia-open-model-license

The release represents a significant step forward in our continued commitment to openness and transparency in model development and improvement. By releasing training data, in addition to the training tools and final model weights, NVIDIA supports continued improvement of open‑weight models.

## What’s in the dataset and how we built it

At a high level, the Nemotron Post-Training Dataset V2 takes our previously released English reasoning data and translates them into five target languages (French, German, Italian, Japanese, Spanish). To best take advantage of English knowledge instilled during pre‑training, we translate the user prompt and model response while preserving the original English reasoning chain.

[![image/png](https://cdn-uploads.huggingface.co/production/uploads/67ee17721b341f4d5608e585/Tov1aIsRIX16n12P22NTE.png)](https://cdn-uploads.huggingface.co/production/uploads/67ee17721b341f4d5608e585/Tov1aIsRIX16n12P22NTE.png)

According to results from the WMT 2024 general translation shared task, LLMs are achieving state‑of‑the‑art results for machine translation tasks. However, for synthetic generation of post‑training data, our preliminary studies have shown that:

- LLMs are more prone to hallucinations when translating SFT datasets compared to translating common machine translation test sets (e.g., FLORES).
- The translation quality and hallucination rate of open‑source LLMs deteriorate significantly as input length increases.

Hence, we incorporate several mechanisms to maintain high translation quality and easy hallucination detection. To summarize:

- We break down sentences by newline and translate line‑by‑line. If a line is non‑translatable (e.g., only tabs) or is part of a code block, it won’t be translated.
- We enforce a specific format (“Wrap the translated text in brackets 〘〙”) and use this special matching bracket to extract translations. Other examples are discarded (see Table 1).
- We run fastText language ID on the translation of prompt inputs to filter out off‑target data points. We discarded another 55,567 examples (another 1.1% of all multilingual examples).

*Table 1: Ratio of discarded data (measured by bytes) by enforcing output format*

| Language | code   | qa    | math  |
| -------- | ------ | ----- | ----- |
| de       | 2.28%  | 1.11% | 2.47% |
| es       | 26.14% | 5.15% | 6.38% |
| fr       | 11.01% | 1.37% | 1.96% |
| it       | 4.94%  | 1.36% | 0.75% |
| ja       | 7.68%  | 2.51% | 3.86% |

After benchmarking, we selected `Qwen2.5-32B-Instruct-AWQ` (for German) and `Qwen2.5-14B-Instruct` (for others) to conduct the translation. The considerations for selecting these models include:

- Robust translation quality
- Can fit onto a single A100 GPU for inference
- Wide domain coverage in training data
- Open license (Apache 2.0)

## How to use it

```python
from datasets import load_dataset
ds = load_dataset("nvidia/Nemotron-Post-Training-Dataset-v2")
```

👉 Explore the dataset here: [Hugging Face dataset page](https://huggingface.co/datasets/nvidia/Nemotron-Post-Training-Dataset-v2)
