---
title: Introducing Grok Voice 2.0
link: https://x.ai/news/grok-voice-think-fast-2
source: x-ai
published: 2026-07-29T00:00:00Z
first_seen: 2026-10-19T01:26:53.998049939Z
summary: Introducing our most capable speech-to-speech voice model.
content: extracted
html: 2026-07-29-introducing-grok-voice-2-0.html
remote_preview:
  url: https://media.x.ai/v1/website/grok-voice-think-fast-2-19ad1e0b.webp
---

Today, we're announcing Grok Voice 2.0, our next-generation voice model with improved intelligence, transcription accuracy, and conversational capabilities.

## [Intelligence](https://x.ai/news/grok-voice-think-fast-2#intelligence)

Grok Voice 2.0 is our most intelligent voice model yet, building on its predecessor with meaningful gains in speech reasoning, conversational ability, and tool use reliability.

Speech-to-speech benchmark comparison between Grok Voice 2.0 and competing models, per Artificial Analysis.
| Benchmark | Grok Voice 2.0 (this release) | Grok Voice 1.0 | GPT-Realtime-2.1 (High) | Gemini 3.1 Flash (High) |
| --------- | ----------------------------- | -------------- | ----------------------- | ----------------------- |
| 82.9%     | 75.7%                         | 79.1%          | 69.5%                   |                         |
| 97.2%     | 97.1%                         | 96.0%          | 96.6%                   |                         |
| 95.1%     | 77.8%                         | 95.7%          | 74.3%                   |                         |
| 56.5%     | 52.1%                         | 45.7%          | 37.7%                   |                         |
| 0.70s     | 1.25s                         | —              | 2.98s                   |                         |

Source: [Artificial Analysis](https://artificialanalysis.ai/speech-to-speech).

## [Transcription Accuracy](https://x.ai/news/grok-voice-think-fast-2#transcription-accuracy)

Grok Voice 2.0 outperforms even dedicated, state of the art transcription models when it comes to accuracy. In our evaluation across thousands of short phrases in 24 different languages, we've demonstrated a 1.5–2.0× improvement relative to Deepgram Nova 3 and ElevenLabs Scribe v2, and a 1.4× improvement relative to Grok Voice 1.0.

The gap between Grok Voice 2.0 and dedicated speech-to-text models widens to ~10× in noisy settings. We've focused on making Grok Voice 2.0 perform exceptionally well in real-world settings, with substantial background noise and telephony compression.

### Transcription accuracy

Word Error Rate (%). Lower is better.

Grok Voice 2.0

Grok Voice 1.0

Deepgram Nova 3

ElevenLabs Scribe v2

## [Reasoning Efficiency](https://x.ai/news/grok-voice-think-fast-2#reasoning-efficiency)

Grok Voice models have a unique characteristic: they reason through queries while speaking. Reasoning in parallel with speech makes the model substantially smarter than other speech-to-speech models with no impact on latency.

Grok Voice 2.0 has been trained to be very efficient with reasoning tokens relative to its predecessor. In production settings, this means that tool calls are snappier, usually executing before the end of the agent's first sentence.

### Reasoning efficiency

Relative reasoning tokens per response (P50). Lower is better.

Grok Voice 2.0

0.4×

Grok Voice 1.0

1.0×

## [Conversational Capability](https://x.ai/news/grok-voice-think-fast-2#conversational-capability)

We've trained Grok Voice 2.0 to be a better conversationalist. By using extensive reinforcement learning to push the model towards patterns we see in real human conversation, we've found that broadly the model speaks in shorter sentences, asks one question at a time, and avoids fluff. From the user's perspective, conversations with the agent are simple and fluid, even though the model is often guiding the conversation through complex workflows and thinking several steps ahead behind the scenes.

## [Migration & Pricing](https://x.ai/news/grok-voice-think-fast-2#migration--pricing)

Grok Voice 2.0 is expected to result in improved performance across almost all use cases without any edits to existing prompts. In A/B testing this model on Starlink (+1 888 GO STARLINK), we've seen a significant increase in sales conversion rate and support containment rate.

On August 5, 2026, `grok-voice-latest` will move from Grok Voice 1.0 to Grok Voice 2.0 (`grok-voice-2.0`). No action needed to upgrade.

We believe pricing should be predictable and transparent. Grok Voice 2.0 is priced at [$0.08 / min of audio](https://docs.x.ai/developers/pricing?campaign=think-fast-2-blog#voice-api-pricing).

## Start building with Grok Voice

[Try It Free](https://console.x.ai/team/default/voice/agents?campaign=think-fast-2-blog&utm_source=website&utm_medium=referral&utm_campaign=think-fast-2-blog) [View Docs](https://docs.x.ai/developers/model-capabilities/audio/speech-to-speech?campaign=think-fast-2-blog)
