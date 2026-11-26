---
title: datasette-charts 0.3a0
link: https://simonwillison.net/2026/Nov/25/datasette-charts/
source: simonwillison-net
published: 2026-11-25T18:16:50Z
updated: 2026-11-25T18:16:50Z
first_seen: 2026-11-26T12:13:26.788658858Z
labels:
- datasette
- datasette-charts
- visualization
summary: 'Release: datasette-charts 0.3a0 I added chart annotations to datasette-charts! You can now render a chart and then annotate it with lines, shaded areas, labels, or a combination of the three. A chart with annotations looks like this: Here''s that same chart in Datasette Lite, where you can tweak the annotations using the chart builder UI: If you also install Datasette Agent the agent can build annotated charts for you. Here''s what I got on the agent.datasette.io demo for this prompt: Show a chart comparing Simon''s tagged content about LLMs with his tagged content about Datasette, with annotations Tags: datasette, visualization, datasette-charts'
content: extracted
html: 2026-11-25-datasette-charts-0-3a0.html
remote_preview:
  url: https://static.simonwillison.net/static/2026/chart-annotate.gif
  alt: Animated screen recording of an "Edit annotation" dialog for a chart titled "Total capacity_mw by decade". A form on the left has fields for "Kind", "Capacity_mw", "Label (optional)", "Label position", "Color" and "Line style", and a "PREVIEW" panel on the right shows a blue bar chart of total capac
---

I added chart annotations to [datasette-charts](https://github.com/datasette/datasette-charts)!

You can now render a chart and then annotate it with lines, shaded areas, labels, or a combination of the three.

A chart with annotations looks like this:

![Screenshot of an annotated bar chart with the y-axis label \"↑ Total capacity_mw\", showing blue bars for each decade from 1950 to 2010. Capacity is low in the earlier decades, then jumps sharply for 2000 and 2010. The chart carries three annotations: a gray shaded band across 1950 to 1990 with the note \"Much smaller summed capacity in earlier decades\", a dot on top of the 2000 bar labeled \"Largest decade total in these records: 1.157 million MW\", and a dashed horizontal line labeled \"1,000,000 MW reference\". A caption below reads \"Global plant records with commissioning years 1950–2019; coverage and missing years may affect comparisons.\"](https://static.simonwillison.net/static/2026/chart-with-annotations.webp)

Here's [that same chart in Datasette Lite](https://lite.datasette.io/?install=datasette-charts&url=https://datasette.io/global-power-plants.db#/global-power-plants/-/query?sql=SELECT+CAST%28commissioning_year+%2F+10+AS+INTEGER%29+%2A+10+AS+decade%2C+capacity_mw+FROM+%22global-power-plants%22+WHERE+commissioning_year+BETWEEN+1950+AND+2019+AND+capacity_mw+IS+NOT+NULL#chart=%7B%22v%22%3A1%2C%22type%22%3A%22bar%22%2C%22encoding%22%3A%7B%22x%22%3A%7B%22column%22%3A%22decade%22%2C%22sort%22%3A%22key%22%2C%22limit%22%3A20%2C%22other%22%3Afalse%7D%2C%22y%22%3A%7B%22column%22%3A%22capacity_mw%22%2C%22aggregate%22%3A%22sum%22%7D%7D%2C%22options%22%3A%7B%22orientation%22%3A%22vertical%22%7D%2C%22annotations%22%3A%5B%7B%22type%22%3A%22point%22%2C%22x%22%3A2000%2C%22text%22%3A%22Largest%20decade%20total%20in%20these%20records%3A%201.157%20million%20MW%22%7D%2C%7B%22type%22%3A%22range%22%2C%22x%22%3A1950%2C%22x2%22%3A1990%2C%22text%22%3A%22Much%20smaller%20summed%20capacity%20in%20earlier%20decades%22%7D%2C%7B%22type%22%3A%22line%22%2C%22y%22%3A1000000%2C%22text%22%3A%221%2C000%2C000%20MW%20reference%22%7D%2C%7B%22type%22%3A%22text%22%2C%22left%22%3A0.02%2C%22top%22%3A0.04%2C%22text%22%3A%22Global%20plant%20records%20with%20commissioning%20years%201950–2019%3B%20coverage%20and%20missing%20years%20may%20affect%20comparisons.%22%7D%5D%7D), where you can tweak the annotations using the chart builder UI:

![Animated screen recording of an \"Edit annotation\" dialog for a chart titled \"Total capacity_mw by decade\". A form on the left has fields for \"Kind\", \"Capacity_mw\", \"Label (optional)\", \"Label position\", \"Color\" and \"Line style\", and a \"PREVIEW\" panel on the right shows a blue bar chart of total capacity by decade from 1950 to 2010, with a dashed horizontal line labeled \"1,000,000 MW reference\". The preview updates live as the user edits: changing the Capacity_mw value from \"1000000\" to \"900000\" and then \"800000\" moves the dashed line down the chart in step, and editing the label text, first adding an exclamation mark, then removing it and deleting the word \"reference\", changes the label on the line to \"1,000,000 MW reference!\" and finally \"1,000,000 MW\". The user then clicks the \"Save\" button.](https://static.simonwillison.net/static/2026/chart-annotate.gif)

If you also install [Datasette Agent](https://agent.datasette.io/) the agent can build annotated charts for you. Here's what I got on the [agent.datasette.io](https://agent.datasette.io/) demo for this prompt:

> `Show a chart comparing Simon's tagged content about LLMs with his tagged content about Datasette, with annotations`

![Screenshot of a chat-style AI agent interface that has produced a chart in response to a question. At the top is a collapsed reasoning line reading \"Thinking: …26 partial year is labeled correctly in the output. Let's go with this approach!\" followed by a collapsed tool call labeled \"chart\". Below that is a line chart titled \"Total content_count per year by topic\", with a legend for \"LLMs\" (blue) and \"Datasette\" (orange), the y-axis label \"↑ Total content_count\" and years 2017 to 2026 on the x-axis. The Datasette line stays low throughout, with an annotation \"Datasette peak year\" pointing at 2020. The LLMs line sits near zero until 2022, then climbs steeply to a peak in 2025 before dropping in 2026, with an annotation \"LLM-tagged content rises\" pointing at 2023. A note on the chart reads \"Counts include posts, notes and links; 2026 is year-to-date.\" Under the chart are the controls \"Open in the chart builder\", \"Copy link\", \"This chart's SQL\", \"Copy TSV\" and \"Copy CSV\". Another collapsed reasoning line reads \"Thinking: …rends start low for LLMs in 2020, and they're projected to rise sharply by 2026.\" The agent's reply then says: \"The chart compares annual counts of non-draft posts, notes, and link posts tagged with LLM-related terms or Datasette-related terms. The annotations mark Datasette's peak year (2020) and the sharp rise in LLM-tagged content beginning in 2023. 2026 is year-to-date.\" At the bottom is a message box with the placeholder \"Type a message... (# to tag a table or query)\" and a \"Send\" button.](https://static.simonwillison.net/static/2026/annotations-agent.webp)
