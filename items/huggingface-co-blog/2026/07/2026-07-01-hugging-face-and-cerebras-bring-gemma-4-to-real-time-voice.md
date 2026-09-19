---
title: Hugging Face and Cerebras bring Gemma 4 to real-time voice AI
link: https://huggingface.co/blog/cerebras-gemma4-voice-ai
source: huggingface-co-blog
published: 2026-07-01T00:00:00Z
updated: 2026-07-01T00:00:00Z
first_seen: 2026-09-19T21:30:23.395188495Z
content: extracted
html: 2026-07-01-hugging-face-and-cerebras-bring-gemma-4-to-real-time-voice.html
preview:
  file: 2026-07-01-hugging-face-and-cerebras-bring-gemma-4-to-real-time-voice.preview-aa903e415d92.webp
  width: 256
  height: 128
  color: '#ece5d1'
images:
- source: https://huggingface.co/blog/assets/cerebras-gemma4-voice-ai/thumbnail.png
  original:
    file: 2026-07-01-hugging-face-and-cerebras-bring-gemma-4-to-real-time-voice.image-aa14ab205d04.png
    width: 1300
    height: 650
  variants:
  - file: 2026-07-01-hugging-face-and-cerebras-bring-gemma-4-to-real-time-voice.image-39df7e2e9a8d.webp
    width: 320
    height: 160
  - file: 2026-07-01-hugging-face-and-cerebras-bring-gemma-4-to-real-time-voice.image-7b8d43d924c6.webp
    width: 640
    height: 320
  - file: 2026-07-01-hugging-face-and-cerebras-bring-gemma-4-to-real-time-voice.image-479c01332774.webp
    width: 960
    height: 480
  - file: 2026-07-01-hugging-face-and-cerebras-bring-gemma-4-to-real-time-voice.image-cb1b10dccb1d.webp
    width: 1300
    height: 650
  color: '#f7f4ed'
---

For voice AI, latency is a critical parameter. Developers have made tremendous progress in model quality, but the user experience is still often limited by response times. Hugging Face and Cerebras are changing that experience. Today, we demonstrate what becomes possible when an open, modular voice AI architecture is paired with industry-leading inference speed.

The result is a speech-to-speech experience that feels dramatically more natural. Instead of waiting for an AI to respond, conversations flow with the responsiveness users expect from human interaction.

## [](https://huggingface.co/blog/cerebras-gemma4-voice-ai#architecture-an-open-cascaded-speech-to-speech-stack) Architecture: an Open, Cascaded Speech-to-Speech stack

The demo is built as a real-time speech-to-speech pipeline. Each part of the system is modular, open, and replaceable, making it easy for developers to adapt the stack for different assistants, robots, products, or research projects.

This creates a fully open speech-to-speech loop:

```
Speech input
  -> speech recognition with Nvidia's Parakeet
  -> Gemma 4 VLM inference on Cerebras
  -> text-to-speech with Alibaba's Qwen3TTS
  -> spoken response
```

The architecture brings together the strength of the open-source AI ecosystem: Cerebras for fast inference, Google DeepMind’s Gemma 4 31B for the language model, and Qwen for text-to-speech. Every layer can be inspected, modified, and extended by the developers

## [](https://huggingface.co/blog/cerebras-gemma4-voice-ai#cerebras-and-hugging-face-partnership) Cerebras and Hugging Face Partnership

Today, some production systems see a reasonable median latency while still experiencing frustrating multi-second delays at the P95. Those delays become even more noticeable when tool calls or multimodal steps require multiple turns.

Cerebras helps solve one of the most important bottlenecks in the stack: the language-model response time. By making inference dramatically faster and more stable, Cerebras allows the rest of the Hugging Face pipeline to shine.

That stability is especially important at the long tail. Many systems can deliver acceptable median response times, but occasional slow responses still make conversations feel unreliable.

## [](https://huggingface.co/blog/cerebras-gemma4-voice-ai#built-for-real-world-interaction) Built for real-world interaction

This same Hugging Face speech-to-speech pipeline already powers Reachy Mini robots, with more than 9,000 robots in the wild. For robots, voice assistants, and embodied AI, responsiveness is not a cosmetic improvement. It is what makes the interaction feel alive.

The motivation to use Cerebras is therefore not simply cost reduction. It is low latency, predictable performance, and the ability to create real-time experiences that feel natural at scale.

This collaboration reflects a shared belief that the future of AI will be both open and performant. Open-source models, open infrastructure, and breakthrough inference speed together create a foundation for the next generation of conversational AI.

We invite developers to explore the demo, experiment with the code, and help shape what comes next for real-time voice AI.

Demo: [Hugging Face Space](https://huggingface.co/spaces/smolagents/hf-realtime-voice)

Repository: [huggingface/speech-to-speech](https://github.com/huggingface/speech-to-speech)
