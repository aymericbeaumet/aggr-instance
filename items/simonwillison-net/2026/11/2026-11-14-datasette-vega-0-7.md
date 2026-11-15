---
title: datasette-vega 0.7
link: https://simonwillison.net/2026/Nov/14/datasette-vega/
source: simonwillison-net
published: 2026-11-14T17:09:09Z
updated: 2026-11-14T17:09:09Z
first_seen: 2026-11-15T00:13:30.261415141Z
labels:
- datasette
- datasette-lite
- visualisation
summary: 'Release: datasette-vega 0.7 A new release of Datasette''s longest untouched plugin, for rendering charts (using Vega-Lite against data in a table or custom query. The last release was in July 2018! I finally bumped it for compatibility with the latest Datasette alphas, plus some cosmetic tweaks to make it work better with Datasette Lite. (Datasette Lite just gained the ability to run JavaScript plugins like this! I''ll write more about that soon.) Since it works with Datasette Lite, the live demo in the README now links to this demo on Datasette Lite. Tags: datasette, visualisation, datasette-lite'
content: extracted
html: 2026-11-14-datasette-vega-0-7.html
remote_preview:
  url: https://static.simonwillison.net/static/2026/datasette-vega.webp
  alt: 'Screenshot of a charting tool. A panel headed "Charting options" has radio buttons for "Bar" (selected), "Line" and "Scatter". Below are select menus: "X column" set to "airline" with "Type" set to "Label", and "Y column" set to "fatalities_00_14" with "Type" set to "Numeric". A "Swap X and Y" butto'
---

A new release of Datasette's longest untouched plugin, for rendering charts (using [Vega-Lite](https://vega.github.io/vega-lite/) against data in a table or custom query. The last release was in July 2018!

I finally bumped it for compatibility with the latest Datasette alphas, plus some cosmetic tweaks to make it work better with [Datasette Lite](https://lite.datasette.io/).

(Datasette Lite just gained the ability to run JavaScript plugins like this! I'll write more about that soon.)

Since it works with Datasette Lite, the live demo in the README now [links to this demo on Datasette Lite](https://lite.datasette.io/?install=datasette-vega&csv=https%3A%2F%2Fraw.githubusercontent.com%2Ffivethirtyeight%2Fdata%2Fmaster%2Fairline-safety%2Fairline-safety.csv#/data/airline-safety?_size=max#g.mark=bar&g.x_column=airline&g.x_type=ordinal&g.y_column=fatalities_00_14&g.y_type=quantitative).

![Screenshot of a charting tool. A panel headed \"Charting options\" has radio buttons for \"Bar\" (selected), \"Line\" and \"Scatter\". Below are select menus: \"X column\" set to \"airline\" with \"Type\" set to \"Label\", and \"Y column\" set to \"fatalities_00_14\" with \"Type\" set to \"Numeric\". A \"Swap X and Y\" button sits beneath those, followed by \"Color\" and \"Size\" menus both set to \"-- none --\". Underneath the panel is a blue bar chart with airline names along the X axis and fatalities_00_14 on the Y axis, running from 0 to 550 - most airlines have no bar, a handful have bars, and Malaysia Airlines is the tallest.](https://static.simonwillison.net/static/2026/datasette-vega.webp)
