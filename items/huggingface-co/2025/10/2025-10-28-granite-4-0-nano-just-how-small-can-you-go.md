---
title: 'Granite 4.0 Nano: Just how small can you go?'
link: https://huggingface.co/blog/ibm-granite/granite-4-nano
source: huggingface-co
published: 2025-10-28T14:59:38Z
updated: 2025-10-28T14:59:38Z
first_seen: 2026-10-04T12:50:30.804479567Z
content: extracted
html: 2025-10-28-granite-4-0-nano-just-how-small-can-you-go.html
---

Today we are excited to share [Granite 4.0 Nano](https://huggingface.co/collections/ibm-granite/granite-40-nano-language-models), our smallest models yet, released as part of IBM's Granite 4.0 model family. Designed for the edge and on-device applications, these models demonstrate excellent performance for their size and represent IBM's continued commitment to develop powerful, useful, models that don't require hundreds of billions of parameters to get the job done.

Like all [Granite 4.0 models](https://huggingface.co/collections/ibm-granite/granite-40-language-models), the Nano models are released under an Apache 2.0 license with native architecture support on popular runtimes like vLLM, llama.cpp, and MLX. The models were trained with the same improved training methodologies, pipelines, and over 15T tokens of training data developed for the original Granite 4.0 models. This release includes variants benefiting from the Granite 4.0’s [new, efficient hybrid architecture](https://www.ibm.com/new/announcements/ibm-granite-4-0-hyper-efficient-high-performance-hybrid-models#The+Granite+4+architecture), and like all Granite language models, the Granite 4.0 Nano models also carry with them IBM's [ISO 42001 certification](https://www.ibm.com/new/announcements/ibm-granite-iso-42001) for responsible model development, giving users added confidence that models are built and governed to global standards.

Specifically, Granite 4.0 Nano comprises of 4 instruct models and their base model counterparts:

- **Granite 4.0 H 1B** – A ~1.5B parameter, dense LLM featuring a hybrid-SSM based architecture.
- **Granite 4.0 H 350M** – A ~350M parameter, dense LLM featuring a hybrid-SSM based architecture.
- **Granite 4.0 1B and Granite 4.0 350M** – Alternative traditional transformer versions of our 1B and 350M Nano models, designed to enable workloads where hybrid architectures may not yet have optimized support (e.g. Llama.cpp).

Building sub-billion to ~1 billion parameter models is an active and competitive space, with advancements in performance and architectures recently made by a number of model developers such as Alibaba (Qwen), LiquidAI (LFM), Google (Gemma) and others. When compared to these other models, Granite 4.0 Nano models demonstrate a significant increase in capabilities that can be achieved with a minimal parameter footprint, as measured by a series of general benchmarks across General Knowledge, Math, Code, and Safety domains.

[![granite-4-nano-chart1](https://cdn-uploads.huggingface.co/production/uploads/65c1173865086cabf46fe1b9/vx93cgRtkLdDvirdCGF18.png)](https://cdn-uploads.huggingface.co/production/uploads/65c1173865086cabf46fe1b9/vx93cgRtkLdDvirdCGF18.png) *Chart 1. Average accuracy of 0.2B–2B parameter models across Knowledge, Math, Code, and Safety benchmarks. See Appendix I for full details.*

In addition to more general benchmarks, Granite Nano models outperformed several similarly sized models on tasks critical for agentic workflows, including instruction following and tool calling, as measured by IFEval and Berkley's Function Calling Leaderboard v3 (BFCLv3) benchmarks.

[![granite-4-nano-chart2](https://cdn-uploads.huggingface.co/production/uploads/65c1173865086cabf46fe1b9/Xzmabcv6MzAvGapHFhXg4.png)](https://cdn-uploads.huggingface.co/production/uploads/65c1173865086cabf46fe1b9/Xzmabcv6MzAvGapHFhXg4.png) *Chart 2. Accuracy on IFEval and BFCLv3 benchmarks.*

Full details of the Granite 4.0 Nano can be found on the [Hugging Face model cards](https://huggingface.co/collections/ibm-granite/granite-40-nano-language-models). Moving forward, expect to see more releases from IBM as we continue to grow the Granite 4.0 family and work to make AI a more efficient and effective tool for developers.

*Appendix I. Breakdown of General Performance Benchmarks* [![granite-4-nano-chart3](https://cdn-uploads.huggingface.co/production/uploads/65c1173865086cabf46fe1b9/8fdBK5XHiGvsaEkvqzLq4.png)](https://cdn-uploads.huggingface.co/production/uploads/65c1173865086cabf46fe1b9/8fdBK5XHiGvsaEkvqzLq4.png)
