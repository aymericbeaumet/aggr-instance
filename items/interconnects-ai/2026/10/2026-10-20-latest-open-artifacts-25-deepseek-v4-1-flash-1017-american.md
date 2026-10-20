---
title: 'Latest open artifacts (#25): DeepSeek V4.1 Flash 1017, American-made models, and new regulatory controls in the US & China'
link: https://www.interconnects.ai/p/latest-open-artifacts-25-deepseek
source: interconnects-ai
published: 2026-10-20T17:04:21Z
updated: 2026-10-20T17:04:21Z
first_seen: 2026-10-20T17:55:58.869157456Z
authors:
- Florian Brand
summary: Western open models strike back
content: extracted
html: 2026-10-20-latest-open-artifacts-25-deepseek-v4-1-flash-1017-american.html
remote_preview:
  url: https://substackcdn.com/image/fetch/$s_!ptGw!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd500e8ff-94e8-4cf5-b99d-5d9aac7f0a8a_1024x768.png
---

A maturing open model ecosystem steps towards regulatory uncertainty.

The geopolitics of open-weight AI are accelerating again this fall, as we would expect with the risks of all AI systems (open and closed) rising rapidly. We’ve seen many news items in the last week showing both the U.S. and Chinese governments positioning themselves for some controls on open models.

1. MOFCOM, the Ministry of Commerce of the People’s Republic of China, released a policy proposal (it’s not yet a law) that would require [frontier AI models](https://x.com/zephyr_z9/status/2111071903469478291) and some other technologies to get pre-approval from the government before they can be shared internationally. We found the reaction to this to be overblown – as the document states that open-source technologies will be [set up for automatic approvals](https://x.com/ZilanQian/status/2111073916194013274) – but many in geopolitical circles say this is a big deal because it proposes a regulatory framework that could be used in the future to regulate China’s AI industry.

   Others we trust on this issue say this proposal could be more about preventing issues like the [sale of Chinese startup Manus](https://www.npr.org/2026/04/27/g-s1-118892/china-blocks-meta-from-acquiring-ai-startup-manus) to Western companies, rather than being about frontier AI models. Either way, the wheels of control are moving around AI, and open-weight models are not free from any influence.

2. The Department of Homeland Security (DHS) released an [order on DeepSeek](https://public-inspection.federalregister.gov/2026-21447.pdf), which restricts DeepSeek itself, as a company, and DeepSeek’s models, as a technology platform, from being a vendor in federal civilian procurement. It is not clear how this will relate to finetuned models or models trained on the outputs of DeepSeek (e.g., Nvidia’s Nemotron models have long relied heavily on DeepSeek models in their training recipes – will they be banned?), but this is an expected action and reinforces the need for a domestic open model ecosystem.

   Other model makers in China, such as Zhipu AI, makers of GLM, are already on the Entity List under Commerce/BIS, which restricts their access to American technology.

DeepSeek followed the above news up by releasing their internal inference stack publicly, [DeepInfer](https://github.com/deepseek-ai/DeepInfer), which is likely far more efficient than existing open-source tools like vLLM/SGLang for their models. [Estimates](https://x.com/zephyr_z9/status/2112499092031185134?s=46) point to DeepSeek making up to 90% gross margin when selling tokens on this inference stack. It is well-known that the top AI labs have much more inference-efficient tools internally, and this is the first time we’ve seen a leading lab share them. Inference companies will be quick to adopt these tools, which will drive prices for DeepSeek’s models down across the ecosystem (or margins up). While not an open model or dataset, we consider this release to be one of the bigger open-source AI artifacts in the last few months.

[Share](https://www.interconnects.ai/p/latest-open-artifacts-25-deepseek?utm_source=substack&utm_medium=email&utm_content=share&action=share)

As of writing this, Kimi K3.1 is rumored to be released very soon, with metadata associated with it [starting to appear in public libraries](https://x.com/mylifcc/status/2112397528272695299). Exciting times!

You can view the models in this email, and all previous issues, on our Artifacts Hub. It includes commentary and other ecosystem metadata.

[Browse the Artifacts Hub](https://artifactshub.ai/)

- **[MiMo-V2.6-Pro-RL](https://huggingface.co/XiaomiMiMo/MiMo-V2.6-Pro-RL)** by [XiaomiMiMo](https://huggingface.co/XiaomiMiMo): an update for Mimo, which reuses the same bases as MiMo V2.5, but used a lot more resources on RL training. They also release an [in-depth tech report](https://huggingface.co/XiaomiMiMo/MiMo-V2.6-Pro-RL/blob/main/MiMo_V2_6_technical_report.pdf), as well as some of the used environments, which is rare at this scale. The final model from this recipe, V2.6-Pro, is the top open model on Artificial Analysis’s ranking today, and the scaled RL runs came with a fun [live dashboard](https://mimo.xiaomi.com/rl/) of training. We’ve been fans of Xiaomi for months now, and their efforts are bearing fruit.

  [![](https://substackcdn.com/image/fetch/$s_!ZT8F!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4b349893-9ca4-441a-8f91-c47cb0ed746f_2908x1752.png)](https://substackcdn.com/image/fetch/$s_!ZT8F!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4b349893-9ca4-441a-8f91-c47cb0ed746f_2908x1752.png)

- **[DeepSeek-V4.1-Flash-1017](https://huggingface.co/deepseek-ai/DeepSeek-V4.1-Flash-1017)** by [deepseek-ai](https://huggingface.co/deepseek-ai): Since the latest Artifacts, the whale has not released one, but two new model iterations with a new size (552B-A16B) which also support image inputs. The model has gained a lot of (well-deserved) attention for its performance at a small price. It also carries the MIT license, continuing the trend established by DeepSeek themselves. This is notable in times where licenses seem to become more bespoke and exclusionary. Below is the Pareto of performance vs price on DeepSWE, a favorite agentic software engineering benchmark today, which DeepSeek dominates.

  [![Image](https://substackcdn.com/image/fetch/$s_!_dJv!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Feafdaf3b-422d-42a3-800b-bf2a88fe62e8_2937x2531.jpeg "Image")](https://substackcdn.com/image/fetch/$s_!_dJv!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Feafdaf3b-422d-42a3-800b-bf2a88fe62e8_2937x2531.jpeg)

- **[clef](https://huggingface.co/Cloudflare/clef)** by [Cloudflare](https://huggingface.co/Cloudflare): Decision models are all the rage after being introduced by the startup [Typesafe](https://typesafe.ai/blog/introducing-system-one-models-and-jev). In short, they are general classification models that output few tokens with their probabilities. Its usefulness has led to quick adoption and also a lot of open model finetunes such as Clef.

- **[NVIDIA-Nemotron-3.5-Super-VL-120B-A12B-NVFP4](https://huggingface.co/nvidia/NVIDIA-Nemotron-3.5-Super-VL-120B-A12B-NVFP4)** by [nvidia](https://huggingface.co/nvidia): The mid-sized model of the Nemotron family got its long awaited update. It is strong for its size and can be run at home (given you own a beefy laptop or RTX Spark). As usual, NVIDIA releases the vast majority of the data and an [in-depth tech report](https://research.nvidia.com/labs/nemotron/files/NVIDIA-Nemotron-3.5-Super-and-Lightning-Technical-Report.pdf).

  [![image](https://substackcdn.com/image/fetch/$s_!PsYX!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F62b03a7e-d894-49a1-a158-fced8869a5b6_11400x4500.png "image")](https://substackcdn.com/image/fetch/$s_!PsYX!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F62b03a7e-d894-49a1-a158-fced8869a5b6_11400x4500.png)

- **[Beam-1.0-501B-A23B](https://huggingface.co/reflection/Beam-1.0-501B-A23B)** by [reflection](https://huggingface.co/reflection): The long awaited open model by Reflection, making it the strongest open American model released under a permissive license (Apache 2.0). While not the smartest open model available, Beam is very impressively token efficient (second to only GPT-6-Astra below). This is a sign of a well-developed post-training stack.

  [![](https://substackcdn.com/image/fetch/$s_!_p5J!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2afd5177-2bb3-4d1d-9eb6-f61c0791f5a6_2358x1276.png)](https://substackcdn.com/image/fetch/$s_!_p5J!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2afd5177-2bb3-4d1d-9eb6-f61c0791f5a6_2358x1276.png)

- **[Ternary-Bonsai-2-27B-gguf](https://huggingface.co/prism-ml/Ternary-Bonsai-2-27B-gguf)** by [prism-ml](https://huggingface.co/prism-ml): A ternary version of Qwen3.8-27B for on-device deployments.

- **[Ornith-1.5-397B](https://huggingface.co/ornith-ai/Ornith-1.5-397B)** by [ornith-ai](https://huggingface.co/ornith-ai): A series of Qwen3.5 and Gemma 4 finetunes that gained attention in the local AI community for its strong performance over the respective (strong) baselines.

  [![Ornith 1.5 397B Benchmark Results](https://substackcdn.com/image/fetch/$s_!saYK!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd676e721-6918-43a8-8724-5e5bcf224377_2787x1568.png "Ornith 1.5 397B Benchmark Results")](https://substackcdn.com/image/fetch/$s_!saYK!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd676e721-6918-43a8-8724-5e5bcf224377_2787x1568.png)

- **[Ling-3.0-flash-VL-fp4](https://huggingface.co/inclusionAI/Ling-3.0-flash-VL-fp4)** by [inclusionAI](https://huggingface.co/inclusionAI): A 124B-A5.5B model by Ant Ling (within Ant Group), which continues to churn out models consistently.

- **[Muse-Spark-Open-816B-A42B](https://huggingface.co/meta-models/Muse-Spark-Open-816B-A42B)** by [meta-models](https://huggingface.co/meta-models): META has released their open version of Muse, which we [have written about in more detail](https://www.interconnects.ai/p/meta-shakes-up-the-frontiers-open)!
