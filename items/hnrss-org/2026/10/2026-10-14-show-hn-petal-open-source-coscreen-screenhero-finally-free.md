---
title: 'Show HN: Petal, open-source CoScreen/ScreenHero finally free'
link: https://petal.live/
source: hnrss-org
published: 2026-10-14T18:23:14Z
updated: 2026-10-14T18:23:14Z
first_seen: 2026-10-15T00:11:15.830933315Z
authors:
- thedracle
summary: 'Hi HN! I’m Jason Thomas, one of the founders of CoScreen. As some of you may know, Datadog sunset the CoScreen service on July 31st. Four of us from the original CoScreen team have spent the last couple months building a free and open source pair programming tool inspired by those that came before us: It’s called Petal (https://github.com/kiruna-labs/petal) Some history first, because when we started CoScreen we had hoped to break the sad and established pattern of this industry. Screenhero was an exceptional tool that a lot of us loved for pairing. Multiple cursors, low latency, you could click your partner’s editor, and just start typing. It felt like magic while we had it. Then Slack bought it in 2015, and shut down the standalone app in December 2017. When Till, Max, and I started CoScreen, we were aware of this history, and wanted to build an even more magical collaboration and screen sharing experience: the entire concept was “throw a window from your laptop up on someone else’s screen, and start collaborating with zero friction.” Datadog acquired us in 2022, and thus began the great adventure of adapting our real-time technology into Datadog products, while also continue developing CoScreen as a standalone tool from the Datadog ecosystem. Datadog invested in the product for years and we’re thankful for that. But CoScreen itself proved, like so many other screen sharing tool acquisitions, to be too tangential from the customer base and core mission. We instead built other communication features in Datadog, the integrations with other video platforms, and also its wildly successful MCP server. We weren''t the only ones. Around was bought by Miro and shut down in March 2025. Multi (formerly Remotion) was acqui-hired by OpenAI in 2024 and shut down about a month later. It''s nearly always the same story: a small team builds something hard that people love, a platform buys it for the team or the feature, and the standalone product doesn''t survive the integration. So we decided firmly to do things differently this time. Petal is completely free and open source and we aren''t monetizing it. If we lose interest or get hit by a bus, you can self-host it or fork it, and nobody can sunset it on you. Honestly it''s a love letter to everyone in this community that supported us, believed in us, and to liberate collaborative screen sharing from this vicious cycle. We hope through this we can help foster a large and vibrant community around innovating in this product sector. What Petal does: shared windows show up as real windows on your desktop, movable and resizable, not tiles in a grid. Several people can share at once, everyone gets a labeled cursor, and you can click into any shared window to type and scroll. Rooms are persistent, so you click once to join "eng-sync" instead of passing links around. It runs on macOS and Windows (Linux is next, this time for real and you can help us!), plus a web app. Media goes over LiveKit. You can use our hosted instance with zero config, or point the app at your own LiveKit server with one docker-compose if you want the traffic to stay on your network or even use an entirely different video backend if you want to. The part that still feels strange to us: CoScreen took almost six years to build. The first version of Petal took under eight weeks, from scratch, with frontier models like Fable and Astra doing a lot of the work. Real-time video and window capture is some of the most unforgiving code we have ever written, so we didn''t expect that. We will do a proper writeup on what worked and what didn''t, and are happy to answer questions about it here. Till, Darren, Max and I will be in the thread. Comments URL: https://news.ycombinator.com/item?id=50092470 Points: 103 # Comments: 25'
content: extracted
html: 2026-10-14-show-hn-petal-open-source-coscreen-screenhero-finally-free.html
---

Open Source\
Pair Programming
---

Low latency, high resolution screensharing and remote control

[Download for Mac](https://app.petal.live/api/download) [Download for Windows](https://app.petal.live/api/download?platform=windows) [View source](https://github.com/kiruna-labs/petal)

free (Linux support coming next)

### Real windows

Shared windows appear as windows on your desktop. Movable, resizable, sharp, not tiles in a grid.

### Interactive

Labeled user cursors on all shared windows. Instant remote mouse and keyboard input.

### Simultaneous

Several users can share their windows at the same time.

### Persistent

Create rooms like eng-sync. Click once and you're in — no links to send, nothing to schedule.

## Run Petal

Only the video backend changes.

quick

### Petal Cloud

Download and join your meeting on our LiveKit video infrastructure.

Setup Install the desktop app and join. Zero config.

You run (nothing)

Best for Most teams

[Download](https://petal.live/#download)

### Self-host

The same app, pointed at a LiveKit server you run.

Setup One docker-compose, set the server URL.

You run The media server — traffic stays yours

Best for Compliance & data control

[Open on GitHub](https://github.com/kiruna-labs/petal)

### Fork & build

Full control. Change the app, bring your own stack.

Setup Fork, build, ship your own.

You run Everything

Best for Deep customization

[Open on GitHub](https://github.com/kiruna-labs/petal)
