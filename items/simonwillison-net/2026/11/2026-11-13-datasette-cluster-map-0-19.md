---
title: datasette-cluster-map 0.19
link: https://simonwillison.net/2026/Nov/13/datasette-cluster-map/
source: simonwillison-net
published: 2026-11-13T23:02:45Z
updated: 2026-11-13T23:02:45Z
first_seen: 2026-11-14T02:15:15.124110058Z
labels:
- datasette
- geospatial
- openstreetmap
summary: 'Release: datasette-cluster-map 0.19 One of the oldest Datasette plugins, and still one of my favorites. It now works with Datasette 1.0a42, and depends on the new datasette-leaflet to ensure OpenStreetMap tiles are requested with a referrer header. Here''s a map of power plants owned by California''s PG&E, also demonstrating Datasette''s recently redesigned facets: Tags: datasette, geospatial, openstreetmap'
content: extracted
html: 2026-11-13-datasette-cluster-map-0-19.html
remote_preview:
  url: https://static.simonwillison.net/static/2026/power-plants-pge.webp
  alt: 'Screenshot of a faceted map interface. Three facet panels at the top: "country_long 1" with United States of America 81 selected; "owner 1" with Pacific Gas & Electric Co. 81 selected; "primary_fuel 5" listing Hydro 63, Solar 10, Gas 5, Storage 2, Nuclear 1. Each primary_fuel row has a pale blue hor'
---

One of the oldest Datasette plugins, and still one of my favorites. It now works with Datasette 1.0a42, and depends on [the new datasette-leaflet](https://simonwillison.net/2026/Nov/13/datasette-leaflet/) to ensure OpenStreetMap tiles are requested with a referrer header.

Here's [a map of power plants owned by California's PG&E](https://datasette.io/global-power-plants/global-power-plants?owner=Pacific+Gas+%26+Electric+Co.), also demonstrating Datasette's recently [redesigned facets](https://github.com/simonw/datasette/pull/3032):

![Screenshot of a faceted map interface. Three facet panels at the top: \"country_long 1\" with United States of America 81 selected; \"owner 1\" with Pacific Gas & Electric Co. 81 selected; \"primary_fuel 5\" listing Hydro 63, Solar 10, Gas 5, Storage 2, Nuclear 1. Each primary_fuel row has a pale blue horizontal bar behind its label, sized in proportion to its count: Hydro's bar fills most of the row width, Solar's is a short sliver, Gas's is shorter still, and Storage and Nuclear have only a thin strip at the left edge. Below is a Leaflet map with + and − zoom controls showing California and Nevada, with marker clusters across northern and central California: 9 near Redding, 21 near Chico, 2 to the west of Chico, 13 northeast of Sacramento, 7 east of Stockton, 6 at Oakland, 10 at Fresno, 9 west of Visalia, 2 north of Bakersfield, plus two individual blue pin markers on the coast, one in the far north and one north of Santa Maria. Attribution: Leaflet | © OpenStreetMap contributors](https://static.simonwillison.net/static/2026/power-plants-pge.webp)
