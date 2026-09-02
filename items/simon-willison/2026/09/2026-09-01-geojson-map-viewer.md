---
title: GeoJSON Map Viewer
link: https://simonwillison.net/2026/Sep/1/geojson/
source: simon-willison
published: 2026-09-01T18:05:45Z
updated: 2026-09-01T18:05:45Z
first_seen: 2026-09-02T14:07:29.769486161Z
tags:
- geospatial
- tools
- geojson
- chatgpt
summary: 'Tool: GeoJSON Map Viewer I was helping Natalie gather some maps of local political boundaries (for the Granada Community Services District and the Midcoast Community Council) and found a need to display some GeoJSON files on a map and export that as a PNG. I asked GPT-5.6-Sol for suggestions of tools and it proactively built one. After some iterations using Claude Code for web and Fable 5.1 we got to this finished tool. As for the GeoJSON.. it turns out if you ask ChatGPT Work to provide boundaries for almost anything it will churn away extracting and combining files from different Government data sources and build exactly what you need. I got this polygon from: I want a polygon that represents the exact boundary of the El Granada GCSD And this one from: Get me a GeoJSON file for the boundary (or boundaries if that makes sense) for the MCC - Midcoast Community Council - that operates near Half Moon Bay CA Here''s a link that displays both of them at the same time on the new GeoJSON map viewing tool. Tags: geospatial, tools, geojson, chatgpt'
content: feed
html: 2026-09-01-geojson-map-viewer.html
---

**Tool:** [GeoJSON Map Viewer](https://tools.simonwillison.net/geojson)

I was helping Natalie gather some maps of local political boundaries (for the [Granada Community Services District](https://granada.ca.gov) and the [Midcoast Community Council](https://midcoastcommunitycouncil.org)) and found a need to display some GeoJSON files on a map and export that as a PNG. I asked GPT-5.6-Sol for suggestions of tools and it proactively built one. After [some iterations](https://tools.simonwillison.net/colophon#geojson.html) using Claude Code for web and Fable 5.1 we got to this finished tool.

As for the GeoJSON.. it turns out if you ask ChatGPT Work to provide boundaries for almost anything it will churn away extracting and combining files from different Government data sources and build exactly what you need.

I got [this polygon](https://gist.github.com/simonw/27d243c9d1cb5d9047fff7360dd49d3c) from:

> `I want a polygon that represents the exact boundary of the El Granada GCSD`

And [this one](https://gist.github.com/simonw/b51f9e0190a13932ac64a0cb407a709d) from:

> `Get me a GeoJSON file for the boundary (or boundaries if that makes sense) for the MCC - Midcoast Community Council - that operates near Half Moon Bay CA`

[Here's a link](https://tools.simonwillison.net/geojson#url=https%3A%2F%2Fgist.github.com%2Fsimonw%2Fb51f9e0190a13932ac64a0cb407a709d&color=%23028FC3&opacity=50&url=https%3A%2F%2Fgist.github.com%2Fsimonw%2F27d243c9d1cb5d9047fff7360dd49d3c&color=%23E4572E&opacity=50&preset=map&resolution=2&lat=37.52640&lng=-122.48297&zoom=13) that displays both of them at the same time on the new GeoJSON map viewing tool.

![Screenshot of a web app with a left sidebar of controls and a large map on the right. Top left: OPENSTREETMAP OVERLAY GeoJSON Map Viewer. Top right: Your GeoJSON stays in this browser. Sidebar Shape 1 panel with blue accent: Shape 1, Remove button, URL https://gist.github.com/simonw/b51f9 with Load button, textarea containing { \"type\": \"FeatureCollection\", \"bbox\": -122.51951044732655, 37.47967619478576, -122.44141365271285, 37.55146379902639, Fill colour #028FC3 with blue swatch, Opacity slider at 50%. Shape 2 panel with red accent: Shape 2, Remove button, URL https://gist.github.com/simonw/27d24 with Load button, textarea containing { \"type\": \"FeatureCollection\", \"name\": \"Granada Community Services District boundary\", \"bbox\": -122.500791193774, 37.4803905345399, Fill colour #E4572E with red swatch, Opacity slider at 50%. Buttons: Render map, Add shape, Load example, Clear. The map shows the coast around Montara, Moss Beach, El Granada and Half Moon Bay with a large semi-transparent blue polygon covering Montara, Moss Beach and Rancho Corral de Tierra extending into the ocean, and an overlapping red polygon covering El Granada and Quarry Park. Map labels include Cabrillo Highway, San Pedro Mountain 325 m, Peak Mountain 545 m, South Peak, CA 1, Montara, Rancho Corral de Tierra, Golden Gate National Recreation Area, 489 m, Scarper Ridge, 552 m, Moss Beach, Montara State Marine Reserve, Ox Hill 542 m, 512 m, Fitzgerald Marine Reserve, Airport Street, Pillar Point Bluff, Quarry Park, El Granada, Pillar Point State Marine Conservation Area, Ox Mountain Landfill, Half Moon Bay State Beach, plus and minus zoom buttons, and attribution Leaflet | © OpenStreetMap contributors.](https://static.simonwillison.net/static/2026/geojson-tool.webp)

Tags: [geospatial](https://simonwillison.net/tags/geospatial), [tools](https://simonwillison.net/tags/tools), [geojson](https://simonwillison.net/tags/geojson), [chatgpt](https://simonwillison.net/tags/chatgpt)
