---
title: datasette-charts 0.1a0
link: https://simonwillison.net/2026/Nov/23/datasette-charts/
source: simonwillison-net
published: 2026-11-23T00:37:20Z
updated: 2026-11-23T00:37:20Z
first_seen: 2026-11-24T05:54:25.686228927Z
labels:
- ai
- claude-mythos-fable
- coding-agents
- datasette
- generative-ai
- llms
- projects
- visualization
summary: 'Release: datasette-charts 0.1a0 I''ve been thinking about this problem for over a year now, but on Sunday I finally fired up Claude Fable 5.2 to build an initial prototype. The prototype went so well that this is now a full plugin. You can try it out in Datasette Lite here. It looks like this: The new charts plugin works against any table or arbitrary SQL query in Datasette. It defines several different chart types (using vendored D3, Observable Plot, and topojson-client), each with a set of heuristics for identifying when they might be a good match for a specific set of rows. Those suggestion thumbnails at the top are actual visualizations driven by the data, and selecting one pulls up the full chart along with tools for modifying it. You can also manually construct a chart from scratch. As with other Datasette visualization plugins, the current state of your chart is persisted in a #fragment in the URL - so you can bookmark charts or share them with others. The most important feature is that the charts take all rows into account. Previous charting plugins like datasette-vega would only show charts for the current page of results, generally up to 100 rows. The new plugin compiles each chart to a SQL query that aggregates across the entire result set (potentially millions of rows) using GROUP BY, time bucketing, and window functions. Each query returns a result capped at 5,000 rows, suitable for display on the chart. The plugin uses sampling to help suggest charts for larger tables without evaluating the full result set. I''m really happy with this initial release of the plugin. It''s still very much an alpha, but it''s the closest I''ve got to the dream of Datasette helping visualize data with as little required direction from the user as possible. Tags: coding-agents, generative-ai, projects, ai, llms, datasette, visualization, claude-mythos-fable'
content: extracted
html: 2026-11-23-datasette-charts-0-1a0.html
remote_preview:
  url: https://static.simonwillison.net/static/2026/datasette-charts-0.1a0.webp
  alt: 'Screenshot of a Datasette table page for a "stats" table showing a new Charts panel. A row of suggested chart thumbnail cards sits at the top, each with a small preview, a title and a plain-English description: "Total downloads per week of date", "Total downloads by package", "Distribution of downlo'
---

I've been thinking about this problem for over a year now, but on Sunday I finally fired up Claude Fable 5.2 to build an initial prototype. The prototype went so well that this is now a full plugin.

You can [try it out in Datasette Lite here](https://lite.datasette.io/?install=datasette-charts#/content/stats#chart=%7B%22v%22%3A1%2C%22type%22%3A%22line%22%2C%22encoding%22%3A%7B%22x%22%3A%7B%22column%22%3A%22date%22%2C%22timeUnit%22%3A%22week%22%7D%2C%22y%22%3A%7B%22column%22%3A%22downloads%22%2C%22aggregate%22%3A%22sum%22%7D%2C%22series%22%3A%7B%22column%22%3A%22package%22%2C%22limit%22%3A5%2C%22other%22%3Atrue%7D%7D%2C%22options%22%3A%7B%7D%7D). It looks like this:

![Screenshot of a Datasette table page for a \"stats\" table showing a new Charts panel. A row of suggested chart thumbnail cards sits at the top, each with a small preview, a title and a plain-English description: \"Total downloads per week of date\", \"Total downloads by package\", \"Distribution of downloads\", \"Total downloads per week of date by package\" (selected) and \"Rows per week of date\". A note says suggestions are based on a sample of rows while charts use every row. Below, the selected chart is shown full size as a multi-series line chart of total downloads over time, with a color legend for sqlite-utils, csv-diff, datasette, llm, shot-scraper and Other. A sidebar on the right has dropdowns for Chart type (Line chart), Time (date, per week), Value (downloads, show Total) and Split by (package, keep top 5), plus a checked \"Group the rest as Other\" option. Under the chart are a \"View and edit this chart's SQL\" link, a \"Copy link to this chart\" button, an expandable Data section and a Close button.](https://static.simonwillison.net/static/2026/datasette-charts-0.1a0.webp)

The new charts plugin works against any table or arbitrary SQL query in Datasette. It defines several different chart types (using vendored [D3](https://d3js.org), [Observable Plot](https://observablehq.github.io/plot/), and [topojson-client](https://www.npmjs.com/package/topojson-client)), each with a set of heuristics for identifying when they might be a good match for a specific set of rows.

Those suggestion thumbnails at the top are actual visualizations driven by the data, and selecting one pulls up the full chart along with tools for modifying it. You can also manually construct a chart from scratch.

As with other Datasette visualization plugins, the current state of your chart is persisted in a `#fragment` in the URL - so you can bookmark charts or share them with others.

The most important feature is that the charts take *all rows into account*. Previous charting plugins like [datasette-vega](https://github.com/simonw/datasette-vega) would only show charts for the current page of results, generally up to 100 rows.

The new plugin compiles each chart to a SQL query that aggregates across the entire result set (potentially millions of rows) using GROUP BY, time bucketing, and window functions. Each query returns a result capped at 5,000 rows, suitable for display on the chart. The plugin uses sampling to help suggest charts for larger tables without evaluating the full result set.

I'm really happy with this initial release of the plugin. It's still very much an alpha, but it's the closest I've got to the dream of Datasette helping visualize data with as little required direction from the user as possible.
