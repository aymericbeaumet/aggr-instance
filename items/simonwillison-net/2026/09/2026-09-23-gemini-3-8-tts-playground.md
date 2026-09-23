---
title: Gemini 3.8 TTS Playground
link: https://simonwillison.net/2026/Sep/23/gemini-tts-playground/
source: simonwillison-net
published: 2026-09-23T17:12:27Z
updated: 2026-09-23T17:12:27Z
first_seen: 2026-09-23T22:13:44.124112615Z
labels:
- gemini
- text-to-speech
summary: 'Tool: Gemini 3.8 TTS Playground Google released two new Gemini text-to-speech models today - gemini-3.8-flash-tts and gemini-3.8-flash-lite-tts. They come with a library of over 2,000 voices, plus the ability to create a custom voice with "just a 30-second audio sample of your voice or a voice you have the rights to use". I vibe coded this bring-your-own-key playground interface with GPT-6 Astra, taking advantage of the open CORS policy of the underlying Gemini API. A notable feature of the API is that it makes it easy to define a full conversation between multiple characters, each with different voices and voice style instructions. Here''s a short demo clip of a conversation between two pelicans debating if they should move to the Pacifica Pier. I had Claude 4.5 Opus write the script and generate a URL to render it using the tool. Your browser does not support the audio element. It took ~20 seconds to generate 1m 18s of audio using Gemini 3.8 Flash TTS (not the cheaper Flash-Lite), at a cost of 2.74 cents. Tags: text-to-speech, gemini'
content: extracted
html: 2026-09-23-gemini-3-8-tts-playground.html
preview:
  file: 2026-09-23-gemini-3-8-tts-playground.preview-6f3926e3ebcb.webp
  width: 256
  height: 225
  alt: 'Screenshot of a web app for composing multi-speaker text-to-speech conversations, with a Compose panel on the left and Connection and Under the hood panels on the right. Left panel: "01 Compose" with a "Load example" button. "Compose settings are saved in the URL for bookmarking or sharing. Your API'
  color: '#f6f7f4'
images:
- source: https://static.simonwillison.net/static/2026/tts-playground.webp
  original:
    file: 2026-09-23-gemini-3-8-tts-playground.image-b25010720b85.webp
    width: 2062
    height: 1812
  variants:
  - file: 2026-09-23-gemini-3-8-tts-playground.image-fc77713e4a48.webp
    width: 320
    height: 281
  color: '#fbfbf8'
---

[Tool](https://simonwillison.net/elsewhere/tool/) [Gemini 3.8 TTS Playground](https://tools.simonwillison.net/gemini-tts-playground) — Test and experiment with Google's Gemini 3.8 text-to-speech API through an interactive playground where you can compose single-voice narration or multi-speaker conversations, preview the generated audio, and explore request and response details. Save your compose settings to bookmarkable URLs for easy sharing, and generate high-quality speech synthesis powered by your Gemini API key.

Google [released two new Gemini text-to-speech models](https://blog.google/innovation-and-ai/models-and-research/gemini-models/gemini-3-8-text-to-speech/) today - `gemini-3.8-flash-tts` and `gemini-3.8-flash-lite-tts`.

They come with a library of over 2,000 voices, plus the ability to create a custom voice with "just a 30-second audio sample of your voice or a voice you have the rights to use".

I [vibe coded](https://tools.simonwillison.net/markdown-svg-renderer?url=https%3A%2F%2Fgist.github.com%2Fsimonw%2Fa82f3aef2677c623d1776092d6b21224) this bring-your-own-key playground interface with GPT-6 Astra, taking advantage of the open CORS policy of the underlying Gemini API.

![Screenshot of a web app for composing multi-speaker text-to-speech conversations, with a Compose panel on the left and Connection and Under the hood panels on the right. Left panel: \"01 Compose\" with a \"Load example\" button. \"Compose settings are saved in the URL for bookmarking or sharing. Your API key is excluded.\" Toggle with \"Single voice\" and \"Conversation\" (Conversation selected). \"Cast\" section with \"+ Add speaker\" button. Speaker \"Gus\", Voice \"Puck\", with a remove × button. Speaker \"Pearl\", Voice \"Kore\", with a remove × button. \"Give each speaker a unique name and a voice. Type to search the loaded catalog by voice ID, name, or language.\" \"Dialogue\" section with \"+ Add line\" button. \"LINE 01\" with up, down and × buttons; Speaker dropdown \"Gus\"; Delivery style \"excited and gossipy\"; text \"Pearl, have you heard? Half the flock just packed up and moved to the Pacifica pier!\" \"LINE 02\" with up, down and × buttons; Speaker dropdown \"Pearl\"; Delivery style \"calm and unimpressed\"; text \"I heard. Honestly, Gus, I don't see the appeal. We've got everything we need right here at Pillar Point Harbor.\" Right panel: \"Connection\" with a \"DIRECT API\" badge. \"Gemini API key\" field showing masked dots with a \"Show\" button. \"2,089 voices loaded. Type in any Voice field to search.\" \"Your key stays in this page's memory and is sent directly to Google. It is never saved to browser storage.\" \"Model\" dropdown \"gemini-3.8-flash-tts\". \"Uses your Gemini API account and quota.\" \"Under the hood\" panel with an expanded \"▼ Request JSON\" section showing a JSON code excerpt, a \"Copy JSON\" button, and an expanded \"▼ Response details\" section showing a JSON code excerpt.](https://static.simonwillison.net/static/2026/tts-playground.webp)

A notable feature of the API is that it makes it easy to define a full conversation between multiple characters, each with different voices and voice style instructions.

Here's a short demo clip of a conversation between two pelicans debating if they should move to [the Pacifica Pier](https://simonwillison.net/2026/Sep/12/sighting-399708714/). I had Claude 4.5 Opus [write the script](https://claude.ai/share/3597fc77-9323-4583-aeb9-b0e4c3f654a3) and generate [a URL to render it using the tool](https://simonwillison.net/u/vj).

It took ~20 seconds to generate 1m 18s of audio using Gemini 3.8 Flash TTS (not the cheaper Flash-Lite), at a cost of 2.74 cents.
