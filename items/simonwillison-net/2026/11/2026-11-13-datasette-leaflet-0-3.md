---
title: datasette-leaflet 0.3
link: https://simonwillison.net/2026/Nov/13/datasette-leaflet/
source: simonwillison-net
published: 2026-11-13T22:30:54Z
updated: 2026-11-13T22:30:54Z
first_seen: 2026-11-14T02:15:15.124110058Z
labels:
- datasette
- leaflet
- openstreetmap
summary: 'Release: datasette-leaflet 0.3 OpenStreetMap have a very reasonable policy that hits to their hosted tile server should send a referrer header. Datasette table pages used the Referrer-Policy: no-referrer HTTP header, causing plugins that loaded tiles from OpenStreetMap to violate that policy. datasette-leaflet is the plugin that loads a bundled Leaflet so other Datasette plugins can depend on it. It now sets the referrer policy on the tile requests themselves to strict-origin-when-cross-origin, passing the host but not the full path to the page. Tags: leaflet, datasette, openstreetmap'
content: extracted
html: 2026-11-13-datasette-leaflet-0-3.html
---

OpenStreetMap have a very reasonable policy that hits to their hosted tile server [should send a referrer header](https://wiki.openstreetmap.org/wiki/Referer). Datasette table pages used the `Referrer-Policy: no-referrer` HTTP header, causing plugins that loaded tiles from OpenStreetMap to violate that policy.

`datasette-leaflet` is the plugin that loads a bundled Leaflet so other Datasette plugins can depend on it. It now sets the referrer policy on the tile requests themselves to `strict-origin-when-cross-origin`, passing the host but not the full path to the page.
