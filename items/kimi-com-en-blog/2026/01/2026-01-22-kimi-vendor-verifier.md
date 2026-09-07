---
title: Kimi Vendor Verifier
link: https://www.kimi.com/en/blog/kimi-vendor-verifier
source: kimi-com-en-blog
published: 2026-01-22T00:00:00Z
first_seen: 2026-09-07T17:03:44.343711062Z
summary: 2026-01-22
content: extracted
html: 2026-01-22-kimi-vendor-verifier.html
preview:
  file: 2026-01-22-kimi-vendor-verifier.preview-d6745d52a103.webp
  width: 256
  height: 144
  alt: Kimi Vendor Verifier
  color: '#272c38'
images:
- source: https://kimi-file.kimi.ai/prod-chat-kimi/kfs/4/2/2026-02-13/1d67h0k2av1fc645nt0h0?x-tos-process=image%2Fauto-orient%2C1%2Fstrip%2Fignore-error%2C1
  original:
    file: 2026-01-22-kimi-vendor-verifier.image-7e1ef35f47ab.webp
    width: 1104
    height: 621
  variants:
  - file: 2026-01-22-kimi-vendor-verifier.image-3a0d23d82613.webp
    width: 48
    height: 27
  color: '#010104'
---

1. [Research](https://www.kimi.com/en/blog/)

## Rebuilding the "Chain of Trust": Kimi Vendor Verifier [![GitHub](https://img.shields.io/badge/GitHub-181717?style=flat&logo=github&logoColor=white)](https://github.com/MoonshotAI/Kimi-Vendor-Verifier)

Alongside the release of the Kimi K2.6 model, we are open-sourcing the Kimi Vendor Verifier (KVV) project, designed to help users of open-source models verify the accuracy of their inference implementations.

Not as an afterthought, but because we learned the hard way that open-sourcing a model is only half the battle. The other half is ensuring it runs correctly everywhere else.

## Official Evaluation Results

Think

Non-Think

| Benchmark       | Metric | Temperature | TopP | MaxTokens | Kimi API |
| --------------- | ------ | ----------- | ---- | --------- | -------- |
|                 |        |             |      |           |          |
| OCRBench        | acc    | 1.0         | 0.95 | 16384     | 91.0     |
| AIME2025        | avg@32 | 1.0         | 0.95 | 98304     | 98.4     |
| MMMU Pro Vision | acc    | 1.0         | 0.95 | 65536     | 78.8     |

You can [click here](https://statics.kimi.ai/k2vv/kimi-k2.6_t0.6_m8192_nothink.zip) to access the Kimi API K2VV evaluation results for calculating the F1 score.

## Why We Built KVV

**From Isolated Incidents to Systemic Issues**

Since the release of K2 Thinking, we have received frequent feedback from the community regarding anomalies in benchmark scores. Our investigation confirmed that a significant portion of these cases stemmed from the misuse of Decoding parameters. To mitigate this immediately, we built our first line of defense at the API level: enforcing Temperature=1.0 and TopP=0.95 in Thinking mode, with mandatory validation that thinking content is correctly passed back.

However, more subtle anomalies soon triggered our alarm. In a specific evaluation on [LiveBenchmark](https://www.reddit.com/r/LocalLLaMA/comments/1osglws/kimi_k2_thinking_scores_lower_than_gemini_25/?rdt=41412), we observed a stark contrast between third-party API and official API. After extensive testing of various infrastructure providers, we found this difference is widespread.

This exposed a deeper problem in the open-source model ecosystem: The more open the weights are, and the more diverse the deployment channels become, the less controllable the quality becomes.

If users cannot distinguish between "model capability defects" and "engineering implementation deviations," trust in the open-source ecosystem will inevitably collapse.

## Our Solution

**Six Critical Benchmarks** (selected to expose specific infra failures):

1. [Pre-Verification](https://github.com/MoonshotAI/Kimi-Vendor-Verifier?tab=readme-ov-file#3-pre-flight-check): Validates that API parameter constraints (temperature, top\_p, etc.) are correctly enforced. All tests must pass before proceeding to benchmark evaluation.
2. [OCRBench](https://github.com/MoonshotAI/Kimi-Vendor-Verifier?tab=readme-ov-file#ocrbench-quick-validation): 5 minutes smoke test for multimodal pipelines.
3. [MMMU Pro](https://github.com/MoonshotAI/Kimi-Vendor-Verifier?tab=readme-ov-file#mmmu-pro-vision): Verify Vision input preprocessing by testing diverse visual inputs.
4. [AIME2025](https://github.com/MoonshotAI/Kimi-Vendor-Verifier?tab=readme-ov-file#aime-2025): Long-output stress test. Catches KV cache bugs and quantization degradation that short benchmarks hide.
5. [K2VV ToolCall](https://github.com/MoonshotAI/K2-Vendor-Verifier): Measures trigger consistency (F1) and JSON Schema accuracy. Tool errors compound in agents; we catch them early.
6. SWE-Bench: Full agentic coding test. (Not open sourced due to dependency of sandbox)

**Upstream Fix**: We embed with vLLM/SGLang/KTransformers communities to fix root causes, not just detect symptoms.

**Pre-Release Validation**: Rather than waiting for post-deployment complaints, we provide early access to test models. This lets infrastructure providers validate their stacks before users encounter issues.

**Continuous Benchmarking**: We will maintain a public leaderboard of vendor results. This transparency encourages vendors to prioritize accuracy.

## Testing Cost Estimation

We completed full evaluation workflow validation on Two NVIDIA H20 8-GPU servers, with sequential execution taking approximately 15 hours. To improve evaluation efficiency, scripts have been optimized for long-running inference scenarios, including streaming inference, automatic retry, and checkpoint resumption mechanisms.

## An Open Invitation

**Weights are open. The knowledge to run them correctly must be too.**

We are expanding vendor coverage and seeking lighter agentic tests. **Contact Us: [\[email protected\]](https://www.kimi.com/cdn-cgi/l/email-protection#89eae6e7fde8eafda4e2ffffc9e2e0e4e0a7eae6e4)**
