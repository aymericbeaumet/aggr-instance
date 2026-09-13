---
title: Generating running routes with GPT-6 Astra and ChatGPT Work
link: https://simonwillison.net/2026/Sep/12/astra-running-routes/
source: simonwillison-net
published: 2026-09-12T23:56:42Z
updated: 2026-09-12T23:56:42Z
first_seen: 2026-09-13T01:23:10.731749865Z
labels:
- ai
- chatgpt
- d3
- generative-ai
- geospatial
- gpt-6-astra
- llms
- openai
- skills
summary: 'Here''s a neat thing I had ChatGPT Work with GPT-6 Astra (Max) do this morning: I live at <my address>. Figure out 5K and 10K running routes from me that loop from my house. Use OSM data. It worked for 27 minutes and produced exactly what I''d asked for, as both an embedded visualization and downloadable GPX file and GeoJSON files. Here''s that 5K route: When I asked it how it had created the route, it replied: I used Nominatim to locate the address and Overpass to download local OpenStreetMap roads and trails, then calculated the loops locally. Frustratingly, the actual code it ran and exact details of what it did weren''t visible to me in the ChatGPT UI. I see this lack of transparency is an anti-feature. By the time I thought to ask for a copy of the Python code it had used, ChatGPT was unable to provide it. This appears to be because the thread had been compacted. I think any LLM system that uses compaction needs to both preserve the pre-compacted text and make that text available via agent tool calls, to protect against this kind of problem. As for displaying the map to me, that used the visualize skill. It created a file called /workspace/el-granada-5k-share.html to embed directly into the ChatGPT UI. Here''s a copy of that HTML, which starts like this: <div id="eg-share-loop"> <div class="viz-row"><h3>El Granada harbor loop</h3><span class="text-small">5.1 km</span></div> <div id="eg-share-stage"></div> <div class="text-small text-muted">Map data © <a href="https://www.openstreetmap.org/copyright" target="_blank" rel="noopener">OpenStreetMap contributors</a></div> <style> #eg-share-loop { width:100%; } #eg-share-loop #eg-share-stage { width:100%; margin:8px 0; } #eg-share-loop .eg-share-map { display:block; width:100%; touch-action:none; } #eg-share-loop .eg-share-map text { fill:var(--foreground); font-size:12px; font-weight:400; } #eg-share-loop .eg-share-label { paint-order:stroke; stroke:var(--background); stroke-width:3px; stroke-linejoin:round; } </style> <script type="application/json" id="eg-share-data">{"route":{"type":"LineString","coordinates":[[-122.467425,37.4997753] ...</script> <script src="https://cdn.jsdelivr.net/npm/d3@7.9.0/dist/d3.min.js"></script> <script> (() => { const root=document.getElementById(''eg-share-loop''); The <script type="application/json"> element contains the full geometry needed to render both the running route and the map itself, using D3, which is loaded from an allow-listed CDN location described in this section of the visualize skill: External resources The CSP allows only cdnjs.cloudflare.com, esm.sh, cdn.jsdelivr.net, unpkg.com, fonts.googleapis.com, fonts.gstatic.com, and fonts.bunny.net. Other origins are blocked and fail silently. Tags: geospatial, ai, d3, openai, generative-ai, chatgpt, llms, skills, gpt-6-astra'
content: extracted
html: 2026-09-12-generating-running-routes-with-gpt-6-astra-and-chatgpt-work.html
preview:
  file: 2026-09-12-generating-running-routes-with-gpt-6-astra-and-chatgpt-work.preview-8cb60e4e2d87.webp
  width: 256
  height: 224
  alt: 'Map screenshot showing a blue route line over a light grey street map. Text: El Granada harbor loop 5.1 km. N ↑ (top right). Street labels along the route: Carmel Avenue, Paloma Avenue, San Carlos Avenue, Avenue Granada, Capistrano Road, Francisco Street, Coastal Trail. The loop runs from the harbor'
  color: '#f5f7fa'
images:
- source: https://static.simonwillison.net/static/2026/5k-route.webp
  original:
    file: 2026-09-12-generating-running-routes-with-gpt-6-astra-and-chatgpt-work.image-4bece8257811.webp
    width: 1336
    height: 1168
  variants:
  - file: 2026-09-12-generating-running-routes-with-gpt-6-astra-and-chatgpt-work.image-18c90fb6e719.webp
    width: 48
    height: 42
  - file: 2026-09-12-generating-running-routes-with-gpt-6-astra-and-chatgpt-work.image-898a47caf9b2.webp
    width: 320
    height: 280
  color: '#fbfbfb'
---

Here’s a neat thing I had [ChatGPT Work](https://simonwillison.net/2026/Aug/30/understanding-chatgpt-work/) with GPT-6 Astra (Max) do this morning:

> `I live at <my address>. Figure out 5K and 10K running routes from me that loop from my house. Use OSM data.`

It worked for 27 minutes and produced exactly what I’d asked for, as both an embedded visualization and downloadable GPX file and GeoJSON files. Here’s that 5K route:

![Map screenshot showing a blue route line over a light grey street map. Text: El Granada harbor loop 5.1 km. N ↑ (top right). Street labels along the route: Carmel Avenue, Paloma Avenue, San Carlos Avenue, Avenue Granada, Capistrano Road, Francisco Street, Coastal Trail. The loop runs from the harbor at the bottom left, north along Avenue Granada and Paloma Avenue to a northern point near Carmel Avenue, then east along San Carlos Avenue and south down Francisco Street to the far right, before returning west along the Coastal Trail beside the coastline. Footer: Map data © OpenStreetMap contributors. Give feedback.](https://static.simonwillison.net/static/2026/5k-route.webp)

When I asked it how it had created the route, it replied:

> I used **Nominatim to locate the address** and **Overpass to download local OpenStreetMap roads and trails**, then calculated the loops locally.

Frustratingly, the actual code it ran and exact details of what it did weren’t visible to me in the ChatGPT UI. I see this lack of transparency is an anti-feature.

By the time I thought to ask for a copy of the Python code it had used, ChatGPT was unable to provide it. This appears to be because the thread had been compacted. I think any LLM system that uses compaction needs to both preserve the pre-compacted text and make that text available via agent tool calls, to protect against this kind of problem.

As for displaying the map to me, that used the [visualize skill](https://codex-tool-reference.simonw.chatgpt.site/skills/visualize). It created a file called `/workspace/el-granada-5k-share.html` to embed directly into the ChatGPT UI.

Here’s [a copy of that HTML](https://gist.github.com/simonw/ea652573c8ff5378b218cb10c8c5a480), which starts like this:

```
<div id="eg-share-loop">
  <div class="viz-row"><h3>El Granada harbor loop</h3><span class="text-small">5.1 km</span></div>
  <div id="eg-share-stage"></div>
  <div class="text-small text-muted">Map data © <a href="https://www.openstreetmap.org/copyright" target="_blank" rel="noopener">OpenStreetMap contributors</a></div>
  <style>
    #eg-share-loop { width:100%; }
    #eg-share-loop #eg-share-stage { width:100%; margin:8px 0; }
    #eg-share-loop .eg-share-map { display:block; width:100%; touch-action:none; }
    #eg-share-loop .eg-share-map text { fill:var(--foreground); font-size:12px; font-weight:400; }
    #eg-share-loop .eg-share-label { paint-order:stroke; stroke:var(--background); stroke-width:3px; stroke-linejoin:round; }
  </style>
  <script type="application/json" id="eg-share-data">{"route":{"type":"LineString","coordinates":[[-122.467425,37.4997753] ...</script>
  <script src="https://cdn.jsdelivr.net/npm/d3@7.9.0/dist/d3.min.js"></script>
  <script>
  (() => {
    const root=document.getElementById('eg-share-loop');
```

The `<script type="application/json">` element contains the full geometry needed to render both the running route and the map itself, using D3, which is loaded from an allow-listed CDN location described in this section of [the visualize skill](https://codex-tool-reference.simonw.chatgpt.site/skills/visualize):

> ### External resources
>
> - The CSP allows only `cdnjs.cloudflare.com`, `esm.sh`, `cdn.jsdelivr.net`, `unpkg.com`, `fonts.googleapis.com`, `fonts.gstatic.com`, and `fonts.bunny.net`. Other origins are blocked and fail silently.
