---
title: Speaking of Voxtral
link: https://mistral.ai/news/voxtral-tts/
source: mistral-ai-news
published: 2026-03-23T16:00:00Z
updated: 2026-03-23T16:00:00Z
first_seen: 2026-09-19T21:30:23.395188495Z
summary: 'Voxtral TTS: A frontier, open-weights text-to-speech model that’s fast, instantly adaptable, and produces lifelike speech for voice agents.'
content: extracted
html: 2026-03-23-speaking-of-voxtral.html
preview:
  file: 2026-03-23-speaking-of-voxtral.preview-17d9d3f80113.webp
  width: 256
  height: 153
  color: '#536e92'
images:
- source: https://mistral.ai/cms-media/api/media/file/Thumbnail-Model-Voxtral-1.jpg
  original:
    file: 2026-03-23-speaking-of-voxtral.image-9c6562e1ea8a.jpg
    width: 1200
    height: 716
  color: '#171627'
- source: https://mistral.ai/_astro/16f25438-cacc-4786-8de7-7852ad0929d4_18F7kk.webp?dpl=6aad049eaf4c2d00095b91e5
  original:
    file: 2026-03-23-speaking-of-voxtral.image-1ecc0948ce12.webp
    width: 1404
    height: 593
  variants:
  - file: 2026-03-23-speaking-of-voxtral.image-957655ec553e.webp
    width: 320
    height: 135
  - file: 2026-03-23-speaking-of-voxtral.image-a41701cb0253.webp
    width: 640
    height: 270
  color: '#fefaeb'
- source: https://mistral.ai/_astro/549bcfdd-e418-48ff-a7a1-45ddd79e32af_Z2mKEiF.webp?dpl=6aad049eaf4c2d00095b91e5
  original:
    file: 2026-03-23-speaking-of-voxtral.image-b3afa110b1b0.webp
    width: 1331
    height: 962
  variants:
  - file: 2026-03-23-speaking-of-voxtral.image-1ec0dc1ebee8.webp
    width: 320
    height: 231
  color: '#fefaeb'
---

Thinking

Summary

Mistral AI has launched Voxtral TTS, a 4B-parameter text-to-speech model delivering state-of-the-art multilingual voice generation with realistic, emotionally expressive speech in 9 languages, low latency, and easy voice adaptation. The model excels in contextual understanding, speaker modeling, and zero-shot cross-lingual voice adaptation, making it ideal for enterprise voice workflows and scalable AI agents. Available via API and Mistral Studio, it offers cost-effective, high-quality voice generation starting at $0.016 per 1k characters.

Today we’re releasing Voxtral TTS, our first text-to-speech model with state-of-the-art performance in multilingual voice generation. The model is lightweight at 4B parameters, making Voxtral-powered agents natural, reliable, and cost-effective at scale.

## Highlights.

1. Realistic, emotionally expressive speech in 9 popular languages with support for diverse dialects.

2. Very low latency for time-to-first-audio.

3. Easily adaptable to new voices.

4. Available to test out in [Mistral Studio](https://console.mistral.ai/build/audio/text-to-speech).

5. Enterprise-grade text-to-speech, powering critical voice agent workflows.

A natural voice generation hinges on the model’s ability to not only recite but interpret a text accurately. Contextual understanding - like neutral, happy, sarcastic, etc. - determines whether the listener considers the generation accurate or robotic. Our model excels at both contextual understanding and speaker modeling: capturing how a specific person naturally speaks. Our voice adaptation goes beyond traditional read-speech by capturing a speaker’s personality, including their natural pauses, rhythm, intonation, and emotional dexterity. With its compact size, low cost and latency, and easy adaptability, Voxtral TTS gives full control and customization for enterprises looking to own their voice AI stack.

Audio is the new UX. Create new interactions for collaboration and understanding only found in speech. Begin now in AI Studio with our Mistral Voices in American, British, and French dialects.

## Listen and decide: can you tell the difference?

Our team speaks dozens of languages in multiple dialects, we understand the importance of cultural nuance and built a model that is a reflection of us. Speech generation builds trust via natural-like rhythm, emotion, and even the use of humor. That’s why with voice emulation, we focused on authenticity and emotional expressiveness.

### Voice emulation

## State-of-the-art performance.

Automated metrics such as word-error-rate and audio quality scores for multilingual text-to-speech systems are unable to measure naturalness of speech. What makes speech natural is extremely nuanced and requires a deep understanding of cultural differences and typical speaking patterns. Hence, comparative human evaluations performed by native speakers are crucial.

For voice agents, latency and quality are in constant tension. Human evaluations show that Voxtral TTS achieves superior naturalness compared to ElevenLabs Flash v2.5 while maintaining similar Time-to-First-Audio (TTFA). Voxtral also performs at parity with the quality of ElevenLabs v3, successfully supporting emotion-steering for more lifelike interactions.

![Voxtral TTS Winrate (1)](https://mistral.ai/_astro/16f25438-cacc-4786-8de7-7852ad0929d4_18F7kk.webp?dpl=6aad049eaf4c2d00095b91e5)

We conducted a comparative human evaluation of Voxtral TTS and ElevenLabs v2.5 Flash in a zero-shot custom voice context. Using two recognizable voices in their native dialects for each of the 9 supported languages, 3 annotators performed a side-by-side preference test per pair on naturalness, accent adherence, and acoustic similarity to the original reference. Voxtral TTS widens the quality gap to v2.5 Flash in this zero-shot multilingual custom voice setting, highlighting the instant customizability of Voxtral TTS to any voice.

## Spoken natively.

Trained on a large speech dataset, Voxtral TTS is built for global application. It supports state-of-the-art performance in 9 languages: English, French, German, Spanish, Dutch, Portuguese, Italian, Hindi, and Arabic.

The model was trained to adapt to a custom voice with a reference as little as 3s and capture not just the voice but also nuances like subtle accent, inflections, intonations and even disfluencies similar to those expressed in the reference. We offer some preset voice options in the API but it is simple to extend to your in-house voice library customizing it to the use-case, localize it to the language and accent, keep it neutral or more emotive, casual or formal, more natural and conversational or robotic.

The model also demonstrates zero-shot cross-lingual voice adaptation even though it’s not explicitly trained for it. For example, the model can generate English speech with a French voice prompt and English text. The resulting speech sounds natural while adopting the accent of the provided voice prompt (in this example, the generated speech has a natural French-accented English). This makes the model useful for building cascaded speech-to-speech translation systems.

### Cascaded speech-to-speech translation

Click a speaker to run cascaded speech-to-speech translation.

Prompt

Before we begin, I'll need to verify a few details. Can you confirm your full name and date of birth?

Generated Audio

Voxtral TTS

## Built for low-latency streaming.

Latency is critical for voice agent applications. Voxtral TTS achieves a model latency of 70ms for a typical input voice sample of 10 seconds and 500 characters, with a real-time factor (RTF) of ≈9.7x. The model natively generates up to two minutes of audio, and our API handles arbitrarily long generations with smart interleaving.

## Voxtral TTS architecture.

The model is a transformer-based, autoregressive, flow-matching model, built on [Ministral 3B](https://docs.mistral.ai/models/ministral-3-3b-25-12). It consists of the following components:

- 3.4B parameters transformer decoder backbone

- 390M flow-matching acoustic transformer

- 300M neural audio codec (symmetric encoder-decoder)

The model takes a voice prompt (5 to 25 seconds) and a text prompt in 9 supported languages. For each audio frame, the transformer backbone predicts a semantic token, then the flow-matching transformer runs 16 function evaluations (NFEs) to produce the acoustic latent.

We developed an in-house codec, which processes audio causally using a semantic VQ (8192 vocabulary) and an acoustic FSQ (36 dim and 21 levels) latent and produces them at 12.5Hz frame rate.

![Audio Infographic](https://mistral.ai/_astro/549bcfdd-e418-48ff-a7a1-45ddd79e32af_Z2mKEiF.webp?dpl=6aad049eaf4c2d00095b91e5)

## Powering enterprise voice workflows.

\
Voxtral TTS closes the loop on audio intelligence, giving enterprise voice pipelines an output layer that passes the human test. It works alongside [Voxtral Transcribe](https://mistral.ai/news/voxtral-transcribe-2) for full speech-to-speech, or integrates into any existing speech-to-text and LLM stack, with cross-lingual support.

### Workflows

### Customer Support

Voice agents that route and resolve queries across channels with natural, brand-appropriate speech. Place Voxtral TTS into existing contact support call systems for automated spoken responses, with output that integrates into existing workflows.

## Test-run the model in Mistral Studio.

Experiment with Voxtral TTS directly in the [Mistral Studio playground](https://console.mistral.ai/build/audio/text-to-speech). Select one of the Mistral voices or record your own.

## Get started with Voxtral TTS.

Voxtral TTS is available now via API at $0.016 per 1k characters.

Try it now in [Mistral Studio](https://console.mistral.ai/build/audio/text-to-speech) or in [Le Chat](http://chat.mistral.ai/).

A model with several reference voices is available as open weights on [Hugging Face](https://huggingface.co/mistralai/Voxtral-4B-TTS-2603) under CC BY NC 4.0 license.

Explore the model’s [documentation](https://docs.mistral.ai/capabilities/audio/text_to_speech) or read our [research paper](https://arxiv.org/abs/2603.25551).

Sign up for our upcoming [webinar](https://learn.mistral.ai/home/events/building-conversation-ai-voxtral) to learn more!

## We’re hiring!

We are building the voice layer for AI, and If this is the kind of problem you want to work on, [we'd love to hear from you](https://mistral.ai/careers).
