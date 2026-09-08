---
title: Qwen3-Omni-Flash-2025-12-01：Hear You. See You. Follow Smarter!
link: https://qwen.ai/blog?id=qwen3-omni-flash-20251201
source: qwen-ai-blog
published: 2025-12-08T21:00:00Z
first_seen: 2026-09-08T10:17:10.166315Z
authors:
- QwenTeam
labels:
- release
content: feed
html: 2025-12-08-qwen3-omni-flash-2025-12-01-hear-you-see-you-follow-smarter.html
preview:
  file: 2025-12-08-qwen3-omni-flash-2025-12-01-hear-you-see-you-follow-smarter.preview-d910de3faa82.webp
  width: 256
  height: 154
  color: '#e3b6eb'
images:
- source: https://img.alicdn.com/imgextra/i1/O1CN01CBLbrb1NJx0kSCqbk_!!6000000001550-2-tps-1590-954.png
  original:
    file: 2025-12-08-qwen3-omni-flash-2025-12-01-hear-you-see-you-follow-smarter.image-0e97c6bec888.png
    width: 1590
    height: 954
  variants:
  - file: 2025-12-08-qwen3-omni-flash-2025-12-01-hear-you-see-you-follow-smarter.image-b6e28a47a075.webp
    width: 48
    height: 29
  color: '#f6dae5'
- source: https://qianwen-res.oss-accelerate-overseas.aliyuncs.com/Qwen3-Omni-Flash-2025-12-01/q3o251201.png
  original:
    file: 2025-12-08-qwen3-omni-flash-2025-12-01-hear-you-see-you-follow-smarter.image-d5c8ede113ea.png
    width: 3039
    height: 1416
  variants:
  - file: 2025-12-08-qwen3-omni-flash-2025-12-01-hear-you-see-you-follow-smarter.image-bb03f3fbfd3b.webp
    width: 48
    height: 22
  - file: 2025-12-08-qwen3-omni-flash-2025-12-01-hear-you-see-you-follow-smarter.image-21acbfa203c7.webp
    width: 320
    height: 149
  - file: 2025-12-08-qwen3-omni-flash-2025-12-01-hear-you-see-you-follow-smarter.image-2ad5ba593866.webp
    width: 640
    height: 298
  - file: 2025-12-08-qwen3-omni-flash-2025-12-01-hear-you-see-you-follow-smarter.image-78ff8d044c8e.webp
    width: 960
    height: 447
  - file: 2025-12-08-qwen3-omni-flash-2025-12-01-hear-you-see-you-follow-smarter.image-53079cc40749.webp
    width: 1280
    height: 596
  - file: 2025-12-08-qwen3-omni-flash-2025-12-01-hear-you-see-you-follow-smarter.image-c2a9cb925ce4.webp
    width: 1600
    height: 746
  - file: 2025-12-08-qwen3-omni-flash-2025-12-01-hear-you-see-you-follow-smarter.image-fe42713d9ac4.webp
    width: 3039
    height: 1416
  color: '#d3ccf3'
---

![](https://qianwen-res.oss-accelerate-overseas.aliyuncs.com/Qwen3-Omni-Flash-2025-12-01/q3o251201.png#center)

[QWEN CHAT](https://chat.qwenlm.ai) [HUGGING FACE](https://huggingface.co/collections/Qwen/qwen3-omni-68d100a86cd0906843ceccbe) [MODELSCOPE](https://modelscope.cn/collections/Qwen3-Omni-867aef131e7d4f) [DASHSCOPE](https://www.alibabacloud.com/help/en/model-studio/qwen-omni) [GITHUB](https://github.com/QwenLM/Qwen3-Omni) [PAPER](https://github.com/QwenLM/Qwen3-Omni/tree/main/assets/Qwen3_Omni.pdf) [HUGGING FACE DEMO](https://huggingface.co/spaces/Qwen/Qwen3-Omni-Demo) [MODELSCOPE DEMO](https://modelscope.cn/studios/Qwen/Qwen3-Omni-Demo)

**Qwen3-Omni** is a next-generation native multimodal large model capable of seamlessly processing multiple input modalities—including text, images, audio, and video—and generating both text and natural-sounding speech outputs simultaneously via real-time streaming responses. This version introduces multiple enhancements to improve model performance and efficiency.

**Qwen3-Omni-Flash-2025-12-01** is a comprehensively upgraded iteration built upon Qwen3-Omni.

Key highlights of this upgraded version include:

- **Greatly Enhanced Audio-Visual Interaction Experience**: Dramatically improved understanding and execution of audio-visual instructions, effectively resolving the “intelligence drop” issue commonly seen in casual spoken scenarios. Multi-turn audio-visual conversations now achieve significantly higher stability and coherence, enabling more natural and seamless interactions.

- **Strengthened System Prompt Control**: Full customization of system prompts is now supported, enabling precise control over model behavior. Whether it’s persona style (e.g., sweet, cool, anime-inspired), colloquial tone preferences, or output length constraints—every detail can be finely tuned, offering unprecedented command over response characteristics.

- **More Reliable Multilingual Compliance**: Supports text-based interaction in **119 languages**, speech recognition in **19 languages**, and speech synthesis in **10 languages**. Language-following instability from the previous version has been fully addressed, ensuring accurate and consistent performance across diverse linguistic contexts.

- **More Human-Like and Fluent Speech Synthesis**: Eliminates sluggish or robotic speech by significantly enhancing adaptive control over prosody. The model now intelligently adjusts speaking rate, pauses, and intonation based on textual context, delivering expressive, natural-sounding voice output that closely mimics real human speech.\
  \

## Performance [#](https://qwen.ai/blog?id=qwen3-omni-flash-20251201#performance)

On objective benchmarks, **Qwen3-Omni-Flash-2025-12-01** achieves substantial improvements across all modalities compared to **Qwen3-Omni-Flash**:

- **🧠 Stronger Text Understanding & Generation**:\
  Major gains in logical reasoning (**ZebraLogic +5.6**), code generation (**LiveCodeBench-v6 +9.3**, **MultiPL-E +2.7**), and holistic writing quality (**WritingBench +2.2**), enabling more reliable execution of complex, multi-step instructions.

- **👂 More Accurate Speech Understanding**:\
  Significantly lower word error rate on **Fleurs-zh**, along with a **+3.2** improvement on **VoiceBench**, reflecting enhanced comprehension of spoken language in real-world dialogue scenarios.

- **🎙️ More Natural Speech Synthesis**:\
  Higher-quality, human-like voice generation across multiple languages—especially in Chinese and multilingual contexts—with improved prosody, pacing, and pausing that closely mirrors natural human speech.

- **👁️ Deeper Image Understanding**:\
  Breakthrough performance on visual reasoning tasks, including **+4.7** on **MMMU**, **+4.8** on **MMMU-Pro**, and **+2.2** on **MathVision\_full**, demonstrating a stronger ability to “see,” interpret, and reason about complex visual content—from diagrams to mathematical figures.

- **🎬 More Coherent Video Understanding**:\
  Steady improvement in video semantic comprehension (**MLVU +1.6**), further strengthened by tighter audio-visual synchronization, laying a solid foundation for seamless real-time video conversations.

With this upgrade, **Qwen3-Omni-Flash-2025-12-01** truly embodies the vision of **“Hear You. See You. Follow Smarter.”**—delivering an AI interaction experience that is more natural, precise, and vivid than ever before.

![](http://qianwen-res.oss-accelerate.aliyuncs.com/Qwen3-Omni-Flash-2025-12-01/q3o251201_metric.png#center)

## What’s Next [#](https://qwen.ai/blog?id=qwen3-omni-flash-20251201#whats-next)

We are eager to hear your feedback and see the innovative applications you create with Qwen3-Omni. In the near future, we will further advance the model along multiple axes, including multi-speaker ASR, video OCR, audio–video proactive learning, and enhance support for agent-based workflows and function calling.

## Citation [#](https://qwen.ai/blog?id=qwen3-omni-flash-20251201#citation)

If you find our model helpful in your research, we’d appreciate a citation!

```bibtex
@misc{qwen3_omni_20251201,
author = {{Qwen Team, Alibaba}},
title = {{Qwen3-Omni-Flash-2025-12-01：Hear You. See You. Follow Smarter!}},
year = {2025},
url = {https://qwen.ai/blog?id=qwen3-omni-20251201},
urldate = {2025-12-05}
}
```
