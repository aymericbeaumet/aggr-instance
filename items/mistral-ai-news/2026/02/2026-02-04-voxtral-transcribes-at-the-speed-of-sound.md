---
title: Voxtral transcribes at the speed of sound.
link: https://mistral.ai/news/voxtral-transcribe-2/
source: mistral-ai-news
published: 2026-02-04T16:00:00Z
updated: 2026-02-04T16:00:00Z
first_seen: 2026-09-19T21:30:23.395188495Z
content: extracted
html: 2026-02-04-voxtral-transcribes-at-the-speed-of-sound.html
preview:
  file: 2026-02-04-voxtral-transcribes-at-the-speed-of-sound.preview-52eb53520d00.webp
  width: 256
  height: 153
  color: '#536e92'
images:
- source: https://mistral.ai/cms-media/api/media/file/Thumbnail-Model-Voxtral.jpg
  original:
    file: 2026-02-04-voxtral-transcribes-at-the-speed-of-sound.image-4774be3cb0b1.jpg
    width: 1800
    height: 1074
  color: '#171626'
- source: https://mistral.ai/_astro/a21cafea-208d-4290-b65e-36c0514dc179_PoxbS.webp?dpl=6aad049eaf4c2d00095b91e5
  original:
    file: 2026-02-04-voxtral-transcribes-at-the-speed-of-sound.image-549218be6691.webp
    width: 1920
    height: 947
  variants:
  - file: 2026-02-04-voxtral-transcribes-at-the-speed-of-sound.image-059227effbf5.webp
    width: 320
    height: 158
  - file: 2026-02-04-voxtral-transcribes-at-the-speed-of-sound.image-1521c3209290.webp
    width: 640
    height: 316
  - file: 2026-02-04-voxtral-transcribes-at-the-speed-of-sound.image-cea65af7d84c.webp
    width: 960
    height: 474
  color: '#fefaeb'
- source: https://mistral.ai/_astro/6b8d07b0-1526-4e94-ac66-a861f092aa41_Z1CuQUW.webp?dpl=6aad049eaf4c2d00095b91e5
  original:
    file: 2026-02-04-voxtral-transcribes-at-the-speed-of-sound.image-1f1125e506a8.webp
    width: 1775
    height: 1103
  variants:
  - file: 2026-02-04-voxtral-transcribes-at-the-speed-of-sound.image-abe470d57706.webp
    width: 320
    height: 199
  - file: 2026-02-04-voxtral-transcribes-at-the-speed-of-sound.image-3ff59f3bc631.webp
    width: 640
    height: 398
  color: '#fefaeb'
- source: https://mistral.ai/_astro/58664549-fe38-420a-86fd-5c7003476689_cu0re.webp?dpl=6aad049eaf4c2d00095b91e5
  original:
    file: 2026-02-04-voxtral-transcribes-at-the-speed-of-sound.image-cf5e599bb2c2.webp
    width: 1775
    height: 1111
  variants:
  - file: 2026-02-04-voxtral-transcribes-at-the-speed-of-sound.image-d1023464e409.webp
    width: 320
    height: 200
  - file: 2026-02-04-voxtral-transcribes-at-the-speed-of-sound.image-5b186e0fdc7e.webp
    width: 640
    height: 401
  color: '#fefaeb'
- source: https://mistral.ai/_astro/97f4a4ee-7448-4a2f-889e-17409821e503_Z1PpeEA.webp?dpl=6aad049eaf4c2d00095b91e5
  original:
    file: 2026-02-04-voxtral-transcribes-at-the-speed-of-sound.image-c68ac4ae98d6.webp
    width: 1920
    height: 915
  variants:
  - file: 2026-02-04-voxtral-transcribes-at-the-speed-of-sound.image-88556913f86d.webp
    width: 320
    height: 153
  - file: 2026-02-04-voxtral-transcribes-at-the-speed-of-sound.image-157f34f8109d.webp
    width: 640
    height: 305
  - file: 2026-02-04-voxtral-transcribes-at-the-speed-of-sound.image-011eb4be680b.webp
    width: 960
    height: 458
  color: '#fdf9eb'
---

Today, we're releasing Voxtral Transcribe 2, two next-generation speech-to-text models with state-of-the-art transcription quality, diarization, and ultra-low latency. The family includes Voxtral Mini Transcribe V2 for batch transcription and Voxtral Realtime for live applications. Voxtral Realtime is open-weights under the Apache 2.0 license.

We're also launching an [audio playground in Mistral Studio](https://console.mistral.ai/build/audio/speech-to-text) to test transcription instantly, powered by Voxtral Transcribe 2, with diarization and timestamps.

## Highlights.

- Voxtral Mini Transcribe V2: State-of-the-art transcription with speaker diarization, context biasing, and word-level timestamps in 13 languages.

- Voxtral Realtime: Purpose-built for live transcription with latency configurable down to sub-200ms, enabling voice agents and real-time applications.

- Best-in-class efficiency: Industry-leading accuracy at a fraction of the cost, with Voxtral Mini Transcribe V2 achieving the lowest word error rate, at the lowest price point.

- Open weights: Voxtral Realtime ships under Apache 2.0, deployable on edge for privacy-first applications.

## Voxtral Realtime.

Voxtral Realtime is purpose-built for applications where latency matters. Unlike approaches that adapt offline models by processing audio in chunks, Realtime uses a novel streaming architecture that transcribes audio as it arrives. The model delivers transcriptions with delay configurable down to sub-200ms, unlocking a new class of voice-first applications.

![Fleur Voxtral 2](https://mistral.ai/_astro/a21cafea-208d-4290-b65e-36c0514dc179_PoxbS.webp?dpl=6aad049eaf4c2d00095b91e5)

*Word error rate (lower is better) across languages in the FLEURS transcription benchmark.*

At 2.4 seconds delay, ideal for subtitling, Realtime matches Voxtral Mini Transcribe V2, our latest batch model. At 480ms delay, it stays within 1-2% word error rate, enabling voice agents with near-offline accuracy.

The model is natively multilingual, achieving strong transcription performance in 13 languages, including English, Chinese, Hindi, Spanish, Arabic, French, Portuguese, Russian, German, Japanese, Korean, Italian, and Dutch. With a 4B parameter footprint, it runs efficiently on edge devices, ensuring privacy and security for sensitive deployments.

We’re releasing the model weights under Apache 2.0 on the [Hugging Face Hub.](https://huggingface.co/mistralai/Voxtral-Mini-4B-Realtime-2602)

## Voxtral Mini Transcribe V2.

![Voxtral 2.0   Avg Diarization Error Rate   Priceper Min](https://mistral.ai/_astro/6b8d07b0-1526-4e94-ac66-a861f092aa41_Z1CuQUW.webp?dpl=6aad049eaf4c2d00095b91e5)

*Average diarization error rate (lower is better) across five English benchmarks (Switchboard, CallHome, AMI-IHM, AMI-SDM, SBCSAE) and the TalkBank multilingual benchmark (German, Spanish, English, Chinese, Japanese).*

![Voxtral 2.0   Transcription Performance Fleurs   Priceper Min](https://mistral.ai/_astro/58664549-fe38-420a-86fd-5c7003476689_cu0re.webp?dpl=6aad049eaf4c2d00095b91e5)

*Average word error rate (lower is better) across the top-10 languages in the FLEURS transcription benchmark.*

Voxtral Mini Transcribe V2 delivers significant improvements in transcription and diarization quality across languages and domains. At approximately 4% word error rate on FLEURS and $0.003/min, Voxtral offers the best price-performance of any transcription API. It outperforms GPT-4o mini Transcribe, Gemini 2.5 Flash, Assembly Universal, and Deepgram Nova on accuracy, and processes audio approximately 3x faster than ElevenLabs’ Scribe v2 while matching on quality at one-fifth the cost.

### Model features.

Voxtral Mini Transcribe 2 introduces key capabilities.

#### Speaker diarization.

Generate transcriptions with speaker labels and precise start/end times. Ideal for meeting transcription, interview analysis, and multi-party call processing. Note: with overlapping speech, the model typically transcribes one speaker.

#### Context biasing.

Provide up to 100 words or phrases to guide the model toward correct spellings of names, technical terms, or domain-specific vocabulary. Particularly useful for proper nouns or industry terminology that standard models often miss. Context biasing is optimized for English; support for other languages is experimental.

#### Word-level timestamps.

Generate precise start and end timestamps for each word, enabling applications like subtitle generation, audio search, and content alignment.

#### Expanded language support.

Like Realtime, this model now supports 13 languages: English, Chinese, Hindi, Spanish, Arabic, French, Portuguese, Russian, German, Japanese, Korean, Italian, and Dutch. Non-English performance significantly outpaces competitors.

#### Noise robustness.

Maintains transcription accuracy in challenging acoustic environments, such as factory floors, busy call centers, and field recordings.

#### Longer audio support.

Process recordings up to 3 hours in a single request.

![FlEURS](https://mistral.ai/_astro/97f4a4ee-7448-4a2f-889e-17409821e503_Z1PpeEA.webp?dpl=6aad049eaf4c2d00095b91e5)

*Word error rate (lower is better) across languages in the FLEURS transcription benchmark.*

## Audio playground.

Test Voxtral Transcribe 2 directly in [Mistral Studio](https://console.mistral.ai/build/audio/speech-to-text). Upload up to 10 audio files, toggle diarization, choose timestamp granularity, and add context bias terms for domain-specific vocabulary. Supports .mp3, .wav, .m4a, .flac, .ogg up to 1GB each.

## Transforming voice applications.

Voxtral powers voice workflows in diverse applications and industries.

- ### Meeting intelligence.

  Transcribe multilingual recordings with speaker diarization that clearly attributes who said what and when. At Voxtral's price point, annotate large volumes of meeting content at industry-leading cost efficiency.
- ### Voice agents and virtual assistants.

  Build conversational AI with sub-200ms transcription latency. Connect Voxtral Realtime to your LLM and TTS pipeline for responsive voice interfaces that feel natural.
- ### Contact center automation.

  Transcribe calls in real time, enabling AI systems to analyze sentiment, suggest responses, and populate CRM fields while conversations are still happening. Speaker diarization ensures clear attribution between agents and customers.
- ### Media and broadcast.

  Generate live multilingual subtitles with minimal latency. Context biasing handles proper nouns and technical terminology that trip up generic transcription services.
- ### Compliance and documentation.

  Monitor and transcribe interactions for regulatory compliance, with diarization providing clear speaker attribution and timestamps enabling precise audit trails.

Both models support GDPR and HIPAA-compliant deployments through secure on-premise or private cloud setups.

## Get started.

[Voxtral Mini Transcribe V2](https://docs.mistral.ai/models/voxtral-mini-transcribe-26-02) is available now via API at $0.003 per minute. Try it now in the new Mistral Studio [audio playground](https://console.mistral.ai/build/audio/speech-to-text) or in [Le Chat](http://chat.mistral.ai).

[Voxtral Realtime](https://docs.mistral.ai/models/voxtral-mini-transcribe-realtime-26-02) is available via API at $0.006 per minute and as open weights on [Hugging Face](https://huggingface.co/mistralai/Voxtral-Mini-3B-Realtime-2602).

[Explore documentation](https://docs.mistral.ai/capabilities/audio_transcription) on Mistral’s audio and transcription capabilities.

## We’re hiring.

If you're excited about building world-class speech AI and putting frontier models into the hands of developers everywhere, we'd love to hear from you. [Apply to join our team](https://mistral.ai/careers).
