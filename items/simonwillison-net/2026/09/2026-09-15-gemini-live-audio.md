---
title: Gemini Live audio
link: https://simonwillison.net/2026/Sep/15/gemini-live/
source: simonwillison-net
published: 2026-09-15T22:47:07Z
updated: 2026-09-15T22:47:07Z
first_seen: 2026-09-16T01:36:55.400042967Z
labels:
- gemini
- generative-ai
- google
- llm-release
- llms
- speech-to-text
- tools
- websockets
summary: 'Tool: Gemini Live audio Google released Gemini 3.8 Live and 3.8 Live Extended Thinking today - two new speech-to-speech models that are a similar shape to OpenAI''s GPT-Live family. I pointed GPT-6 Astra Extra High at the documentation and had it build me this web UI for trying out the new models. You can select a model and voice preset, enter an optional system prompt and then start a voice conversation through your browser, including the ability to interrupt the model while it is talking. The implementation uses no libraries. It connects to the wss://generativelanguage.googleapis.com/ws/google.ai.generativelanguage.v1alpha.GenerativeService.BidiGenerateContent?key=... WebSocket endpoint and uses a Web Audio API AudioContext for both capture and playback. Here''s the Gemini Live tutorial for getting started with that WebSockets API. Tags: google, tools, websockets, generative-ai, llms, gemini, llm-release, speech-to-text'
content: extracted
html: 2026-09-15-gemini-live-audio.html
preview:
  file: 2026-09-15-gemini-live-audio.preview-ef515f136909.webp
  width: 256
  height: 209
  alt: 'Screenshot of a voice chat web interface with a transcript. Top buttons: Start session, End session, Mute mic, plus a Mic level meter and a timer showing 0:33. Status: Listening. Use headphones to reduce echo. Starting a session asks for microphone access. Transcript (with Download transcript and Cl'
  color: '#f5f6f8'
images:
- source: https://static.simonwillison.net/static/2026/gemini-live-tool.webp
  original:
    file: 2026-09-15-gemini-live-audio.image-d325bd47b4c6.webp
    width: 1972
    height: 1608
  variants:
  - file: 2026-09-15-gemini-live-audio.image-b26914c6e7d4.webp
    width: 320
    height: 261
  color: '#fbfcfd'
---

Google released [Gemini 3.8 Live and 3.8 Live Extended Thinking](https://blog.google/innovation-and-ai/models-and-research/gemini-models/gemini-3-8-live-gemini-3-8-live-extended-thinking/) today - two new speech-to-speech models that are a similar shape to OpenAI's [GPT-Live](https://openai.com/index/introducing-gpt-live/) family.

I pointed GPT-6 Astra Extra High at the documentation and [had it build me this web UI](https://gist.github.com/simonw/067b7430c5b1f743af9419b0184c38ef) for trying out the new models. You can select a model and voice preset, enter an optional system prompt and then start a voice conversation through your browser, including the ability to interrupt the model while it is talking.

![Screenshot of a voice chat web interface with a transcript. Top buttons: Start session, End session, Mute mic, plus a Mic level meter and a timer showing 0:33. Status: Listening. Use headphones to reduce echo. Starting a session asks for microphone access. Transcript (with Download transcript and Clear buttons). Gemini: Yes, it's working perfectly. I can hear you clearly. How can I help you today? You: Okay, this is pretty good. Tell me some interesting facts about the California brown pelican. Gemini: They're famous for their spectacular plunge-dives to catch fish and have huge throat pouches that can hold up to three gallons of water and fish. They also made a great recovery after being endangered — marked as Interrupted. You: No, tell me different facts actually. Gemini: They nest in colonies on offshore islands and can hold up to three gallons of water in their throat pouches. At the bottom is a text input reading Or type a message… with a Send button, and the note: Sending a message interrupts the current response. Transcripts may include speech interrupted before playback.](https://static.simonwillison.net/static/2026/gemini-live-tool.webp)

The [implementation](https://github.com/simonw/tools/blob/main/gemini-live.html) uses no libraries. It connects to the `wss://generativelanguage.googleapis.com/ws/google.ai.generativelanguage.v1alpha.GenerativeService.BidiGenerateContent?key=...` WebSocket endpoint and uses a Web Audio API `AudioContext` for both capture and playback.

Here's [the Gemini Live tutorial](https://ai.google.dev/gemini-api/docs/live-api/get-started-websocket) for getting started with that WebSockets API.
