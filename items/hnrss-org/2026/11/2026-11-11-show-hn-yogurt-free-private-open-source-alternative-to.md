---
title: 'Show HN: Yogurt – Free, private, open-source alternative to Granola'
link: https://github.com/FrigadeHQ/yogurt
source: hnrss-org
published: 2026-11-11T15:33:06Z
updated: 2026-11-11T15:33:06Z
first_seen: 2026-11-11T21:51:32.649893059Z
authors:
- brownrout
summary: 'Hey HN, I’m Eric and I want to share an OSS project that I’ve been building in my free time. Yogurt is a free, MIT licensed desktop app that uses on-device AI models to transcribe, summarize, and chat with your meetings. Here''s a quick demo video: https://www.youtube.com/watch?v=BFfbXAwRq98 Similar to Granola, Yogurt runs in the background and does not join your calls. But unlike Granola, Yogurt is open-source, free to use, and private. Your meeting transcripts, call recordings, and summaries never leave your computer. As small OSS models have gotten faster and more powerful, Yogurt seemed like a pretty obvious win for privacy and cost conscious folks. You do not need a frontier model to summarize meeting notes or answer questions about them. And if you want to make your notes available to a frontier model, you can just share it with Anthropic, OpenAI, etc. directly via Yogurt’s local MCP server. There’s no need to use a paid cloud service as a middleman. I tried some of the most popular OSS note taking tools, and most were missing features like calendar integration, MCP server, etc. or required you to connect or create an account. And more than that, they didn’t feel polished enough for it to be something I actually wanted to use daily, and so I built Yogurt to scratch that itch. I spent a lot of time fine-tuning every UI interaction/animation and optimizing the model calls to get the most out of your hardware + make it snappy. To support Windows/Mac/Linux, I decided to go with Electron. I use whisper.cpp for transcription and Apple’s on-device SpeechAnalyzer if your system supports it. For call summaries and questions, I use a local open weight LLM that automatically gets downloaded and set up based on what your hardware can handle without your cooling fans firing off like crazy (this part uses llama.cpp). I’ve spent a lot of time working on details like echo detection/reduction, global/folder/local level search respectively, and auto-end recordings. Hope this is useful for any of you who have held out on using AI note tools for privacy or cost reasons, or for folks using cloud tools who want to switch to something free and open source that still feels great to use. Would love to hear what you think about the onboarding and note taking experience :) Comments URL: https://news.ycombinator.com/item?id=50454625 Points: 124 # Comments: 59'
content: extracted
html: 2026-11-11-show-hn-yogurt-free-private-open-source-alternative-to.html
remote_preview:
  url: https://repository-images.githubusercontent.com/1453132706/355ef15b-b53b-4211-bd76-47cdaad9ef5b
  alt: Private AI meeting notes. Free and open source. Contribute to FrigadeHQ/yogurt development by creating an account on GitHub.
---

[![Yogurt. Take note.](https://github.com/FrigadeHQ/yogurt/raw/main/docs/assets/readme-header.svg)](https://yogurt.so?utm_source=github&utm_medium=readme&utm_campaign=yogurt)

Private AI meeting notes. Free and open source.

Download for [Mac](https://github.com/FrigadeHQ/yogurt/releases/latest/download/Yogurt-mac-arm64.dmg) · [Windows](https://github.com/FrigadeHQ/yogurt/releases/latest/download/Yogurt-windows-x64.exe) · [Linux](https://github.com/FrigadeHQ/yogurt/releases/latest/download/Yogurt-linux-x86_64.AppImage)

[![Yogurt showing the Summary of a launch planning meeting: the plan, next steps by person, and your own reminders](https://github.com/FrigadeHQ/yogurt/raw/main/docs/assets/yogurt-screenshot.webp)](https://github.com/FrigadeHQ/yogurt/blob/main/docs/assets/yogurt-screenshot.webp)

Yogurt records your calls without adding a bot to the meeting.

macOS 14.4+ (Apple Silicon) · Windows 10 2004+ (x64) · Ubuntu 22.04+ or similar (x64 AppImage) · 8 GB memory

[yogurt.so](https://yogurt.so?utm_source=github&utm_medium=readme&utm_campaign=yogurt) · [Help](https://github.com/FrigadeHQ/yogurt/blob/main/docs/README.md) · [Build and run locally](https://github.com/FrigadeHQ/yogurt/blob/main/docs/building.md) · [MIT license](https://github.com/FrigadeHQ/yogurt/blob/main/LICENSE)

Made by [Frigade](https://frigade.com?utm_source=github&utm_medium=readme&utm_campaign=yogurt).
