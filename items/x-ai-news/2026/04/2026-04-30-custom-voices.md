---
title: Custom Voices
link: https://x.ai/news/grok-custom-voices
source: x-ai-news
published: 2026-04-30T00:00:00Z
first_seen: 2026-09-08T10:17:10.166315Z
summary: Your voice. Your brand. Clone a voice from a short recording and use it across Grok Text to Speech and Voice Agent APIs.
content: extracted
html: 2026-04-30-custom-voices.html
preview:
  file: 2026-04-30-custom-voices.preview-c64aeb3be15d.webp
  width: 256
  height: 144
  color: '#222d49'
images:
- source: https://media.x.ai/v1/website/grok-custom-voices-d5eafa89.webp
  original:
    file: 2026-04-30-custom-voices.image-532c2bebac6e.webp
    width: 1486
    height: 836
  variants:
  - file: 2026-04-30-custom-voices.image-23c92cb93fea.webp
    width: 48
    height: 27
  color: '#000102'
---

Today, we're introducing **Custom Voices**. Clone your voice from a few seconds of audio and use it instantly across [Grok Text to Speech](https://docs.x.ai/developers/model-capabilities/audio/text-to-speech?campaign=custom-voices-blog) and [Voice Agent APIs](https://docs.x.ai/developers/model-capabilities/audio/speech-to-speech?campaign=custom-voices-blog).

## [Use Cases](https://x.ai/news/grok-custom-voices#use-cases)

Custom Voices unlock a new class of applications.

## [Custom Voices](https://x.ai/news/grok-custom-voices#custom-voices)

**Clone your voice in under two minutes. Use it everywhere.**

Record about a minute of natural speech in the [xAI console](https://console.x.ai/team/default/voice/voice-library?campaign=custom-voices-blog&utm_source=website&utm_medium=referral&utm_campaign=custom-voices-blog). Our pipeline verifies you're the voice owner, processes your recording, and delivers a production-ready voice model, all in under two minutes. Your custom voice inherits every TTS capability: [speech tags](https://docs.x.ai/developers/model-capabilities/audio/text-to-speech?campaign=custom-voices-blog), multilingual output, and both REST and WebSocket streaming.

Custom voices work everywhere our built-in voices do. Pass the `voice_id` to any [TTS endpoint](https://docs.x.ai/developers/model-capabilities/audio/text-to-speech?campaign=custom-voices-blog) or use it with the [Voice Agent API](https://docs.x.ai/developers/model-capabilities/audio/speech-to-speech?campaign=custom-voices-blog) for real-time conversational agents.

There is no extra charge to use Text to Speech or Voice Agent APIs with custom voices.

## [Voice Safety](https://x.ai/news/grok-custom-voices#voice-safety)

Every custom voice goes through a two-stage verification process before it can be created. First, the speaker reads a verification phrase that our STT engine transcribes and matches in real time, confirming intent and presence. Then we compute speaker embeddings from the verification clip and the full recording to confirm they belong to the same person.

You can't clone a voice from a pre-existing recording, and you can't clone someone else's voice.

### Passphrase Check

Read a verification phrase aloud. Our STT engine transcribes and matches it in real time, verifying your consent and presence.

### Speaker Similarity

Speaker embeddings from the passphrase and the full recording are compared to confirm they belong to the same person.
