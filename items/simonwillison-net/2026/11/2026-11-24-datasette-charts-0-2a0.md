---
title: datasette-charts 0.2a0
link: https://simonwillison.net/2026/Nov/24/datasette-charts/
source: simonwillison-net
published: 2026-11-24T04:24:29Z
updated: 2026-11-24T04:24:29Z
first_seen: 2026-11-24T05:54:25.686228927Z
labels:
- datasette
- datasette-agent
- visualization
summary: 'Release: datasette-charts 0.2a0 I introduced this new charting plugin for Datasette yesterday. Today it''s gained integration with Datasette Agent, so now you can ask chat for a chart and it will create one for you using the same underlying chart suggestion and generation tools. Here''s a demo video, recorded on agent.datasette.io where you can sign in with GitHub to try the plugin yourself: Download the video (MP4) I prompt Datasette Agent to "Use #stats to show sqlite-utils downloads over time as a chart" - where typing "#stats" opens a table search box. It shows me a chart inline in the chat, and when I click "Open in the chart builder" it opens the chart in a panel along with the table page. I use the table page to switch to a chart of "datasette", then when I ask the agent "what does the chart show?" it notices that I changed the content in the panel and describes the new Datasette chart to me. Here''s a Markdown export of the chat from the video. Tags: datasette-agent, datasette, visualization'
content: extracted
html: 2026-11-24-datasette-charts-0-2a0.html
remote_preview:
  url: https://static.simonwillison.net/static/cors-allow/2026/datasette-charts-agent-demo.webp
  alt: Demo of the Datasette charts agent (silent screen recording)
---

I introduced this new charting plugin for Datasette [yesterday](https://simonwillison.net/2026/Nov/23/datasette-charts/). Today it's gained integration with [Datasette Agent](https://agent.datasette.io/), so now you can ask chat for a chart and it will create one for you using the same underlying chart suggestion and generation tools.

Here's a demo video, recorded on [agent.datasette.io](https://agent.datasette.io/) where you can sign in with GitHub to try the plugin yourself:

[Download the video (MP4)](https://static.simonwillison.net/static/cors-allow/2026/datasette-charts-agent-demo.mp4) \
 I prompt Datasette Agent to "Use #stats to show sqlite-utils downloads over time as a chart" - where typing "#stats" opens a table search box. It shows me a chart inline in the chat, and when I click "Open in the chart builder" it opens the chart in a panel along with the table page. I use the table page to switch to a chart of "datasette", then when I ask the agent "what does the chart show?" it notices that I changed the content in the panel and describes the new Datasette chart to me.

Here's [a Markdown export](https://gist.github.com/simonw/12646fcc72dddae3d4ac04b496c20af2) of the chat from the video.
