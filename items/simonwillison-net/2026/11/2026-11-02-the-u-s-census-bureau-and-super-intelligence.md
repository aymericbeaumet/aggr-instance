---
title: The U.S. Census Bureau and “Super Intelligence”
link: https://simonwillison.net/2026/Nov/2/census-bureau-super-intelligence/
source: simonwillison-net
published: 2026-11-02T13:31:26Z
updated: 2026-11-02T13:31:26Z
first_seen: 2026-11-02T16:59:20.971254334Z
labels:
- ai
- census
- generative-ai
- llms
- surveys
summary: 'I was reminded today of AI Adoption Rate Trending Down for Large Companies, a chart produced by Apollo Global Management in September 2025, which appeared to show a slowdown in U.S. corporate AI adoption based on numbers collected by the U.S. Census Bureau. At the time I used that chart to demonstrate GPT-5 data analysis, and then to experiment with Claude''s new Code Interpreter. I decided to try running those numbers again to see what had changed over the past year, and in doing so learned that the Bureau has fallen foul of Executive Order 14434, aka the asinine Super Intelligence one. First, the updated chart Every two weeks the U.S. Census Bureau runs a survey called the Business Trends and Outlook Survey (aka BTOS). Each time it goes to 200,000 businesses out of 1.2 million total - the surveyed group (a "panel") cycles such that an individual business only completes the survey every twelve weeks. The latest data is published on alternating Thursdays at 10am. I used the most recent numbers (published October 22, 2026) to create a new version (GPT-6 Astra Extra High transcript) of the AI adoption chart, updated to reflect both the last twelve months and a change that was made to the survey question sent out by the Bureau: For comparison, here''s that AI adoption chart from last year (my GPT-5 version): In September 2025 the largest (250+ employees) companies answered "yes" around 12% of the time to the question about whether they used AI in "producing goods or services". Under the new "any business function" wording, by February 2026, those same large companies answered "yes" around 30% of the time, and by October 2026 that had increased to around 40%. The data in the above chart comes from the "Employment size class" XLSX file, available from here. Here''s a copy of the file I used. You can view the survey document that is sent out to businesses - here''s my snapshot copy of that 346KB PDF. The changing language of the questions As the chart demonstrates, the question asked by the Bureau changed in a material way on November 17th, 2025. Prior to that date, the question they asked was this (emphasis mine): In the last two weeks, did this business use Artificial Intelligence (AI) in producing goods or services? (Examples of AI: machine learning, natural language processing, virtual agents, voice recognition, etc.) On December 3rd, 2025, the Bureau announced a significant change to that question. Here''s the replacement: In the last two weeks, did this business use Artificial Intelligence (AI) in any of its business functions? (Examples of AI: machine learning, natural language processing, virtual agents, voice recognition, etc.) No more "producing goods or services" - now it''s "in any of its business functions"! This means that the answers to the question before and after November 17th are not directly comparable. From October 19, 2026, they ask about Super Intelligence When I went looking for the updated question language in the latest XLSX file, I couldn''t find it. I found this instead: In the last two weeks, did this business use Super Intelligence or SI (formerly referred to as Artificial Intelligence or AI) in any of its business functions? (Examples of SI use include: SI-powered data analysis; generative SI for marketing, product design, or coding; virtual SI agents for customer service; SI-powered forecasting or pricing; SI-driven computer vision for quality control, etc.) The BTOS about page explains what happened: Beginning with the October 19, 2026 collection, instrument wording was updated due to Executive Order 14434. The current numbers reflect the old Artificial Intelligence survey wording - the first survey results reflecting the Super Intelligence wording are due for publication on Thursday. Is this going to affect the survey? The obvious open question here is whether this wording change is going to affect the survey results. The businesses answering this survey will have last seen that question twelve weeks ago, when it was still called AI. Hopefully the new language isn''t going to confuse them, especially since the survey helpfully spells out "formerly referred to as Artificial Intelligence or AI". I can''t help but laugh at the absurdity of "SI-powered data analysis; generative SI for marketing, product design, or coding; virtual SI agents for customer service" though. Tags: census, surveys, ai, generative-ai, llms'
content: extracted
html: 2026-11-02-the-u-s-census-bureau-and-super-intelligence.html
remote_preview:
  url: https://static.simonwillison.net/static/2026-11-01/5821A414-FAF6-4E61-B037-952C1FD80D39.png
  alt: Line chart titled "AI use by U.S. firm size", subtitled "Six-survey trailing average • Share of employer businesses reporting use in the last two weeks" and "Updated with the October 22, 2026 Census release". Seven lines for firm sizes 250+, 100–249, 50–99, 20–49, 10–19, 5–9 and 1–4, on a y-axis fro
---

I was reminded today of [AI Adoption Rate Trending Down for Large Companies](https://www.apolloacademy.com/ai-adoption-rate-trending-down-for-large-companies/), a chart produced by Apollo Global Management in September 2025, which appeared to show a slowdown in U.S. corporate AI adoption based on numbers collected by the U.S. Census Bureau. At the time I used that chart [to demonstrate GPT-5 data analysis](https://simonwillison.net/2025/Sep/9/apollo-ai-adoption/), and then [to experiment with Claude’s new Code Interpreter](https://simonwillison.net/2025/Sep/9/claude-code-interpreter/#something-much-harder-recreating-the-ai-adoption-chart). I decided to try running those numbers again to see what had changed over the past year, and in doing so learned that the Bureau has fallen foul of [Executive Order 14434](https://www.whitehouse.gov/presidential-actions/2026/09/inaugurating-the-era-of-super-intelligence/), aka the asinine Super Intelligence one.

#### First, the updated chart

Every two weeks the U.S. Census Bureau runs a survey called the [Business Trends and Outlook Survey](https://www.census.gov/hfp/btos/data) (aka BTOS). Each time it goes to 200,000 businesses out of 1.2 million total—the surveyed group (a “panel”) cycles such that an individual business only completes the survey every twelve weeks.

The latest data is published on alternating Thursdays at 10am.

I used the most recent numbers (published October 22, 2026) to create a new version ([GPT-6 Astra Extra High transcript](https://chatgpt.com/share/6ae89e86-8658-83e8-85fd-c4ef2d4f3b89?ogimg=plain)) of the AI adoption chart, updated to reflect both the last twelve months and a change that was made to the survey question sent out by the Bureau:

![Line chart titled \"AI use by U.S. firm size\", subtitled \"Six-survey trailing average • Share of employer businesses reporting use in the last two weeks\" and \"Updated with the October 22, 2026 Census release\". Seven lines for firm sizes 250+, 100–249, 50–99, 20–49, 10–19, 5–9 and 1–4, on a y-axis from 0% to 45% and an x-axis from Jan 2024 to Oct 2026. Left section, \"AI in producing goods or services\": all sizes start at 3–5% in late 2023 and climb slowly to 7–13% by September 2025, with 250+ highest. A shaded gap is labeled \"Question broadened Nov 2025\" and \"Series not comparable\". Right section, \"AI in any business function\": all lines restart higher in February 2026 and rise steadily to \"250+: 40.7%\", \"100–249: 33.5%\", \"50–99: 27.9%\", \"20–49: 23.8%\", \"1–4: 23.3%\", \"10–19: 22.1%\", \"5–9: 21.1%\". Footnotes: \"Source: U.S. Census Bureau, Business Trends and Outlook Survey (BTOS). Size groups are numbers of employees.\" \"Method: arithmetic mean of six successive survey estimates, calculated separately for each question wording; no bridging of the break.\" \"Dates use reference-period end, matching the original chart. Latest: Oct 4, 2026 (collected Oct 5–18). First new six-wave average: Jan 25, 2026.\" \"The gap includes the 2025 shutdown and the wait for six observations under the broader question. Survey uncertainty is not displayed.\"](https://static.simonwillison.net/static/2026-11-01/5821A414-FAF6-4E61-B037-952C1FD80D39.png)

For comparison, here’s that AI adoption chart from last year (my GPT-5 version): ![AI adoption rates starting to decline for larger firms. A chart of AI adoption rate by firm size. Includes lines for 250+, 100-249, 50-99, 20-49, 10-19, 5-9 and 1-4 sized organizations. Chart starts in November 2023 with percentages ranging from 3 to 5, then all groups grow through August 2025 albeit with the 250+ group having a higher score than the others. That 250+ group peaks in July 2025 at around 14% and then appears to slope slightly downwards to 12% by August. Some of the other lines also start to tip down, though not as much.](https://static.simonwillison.net/static/2025/apollo-ai-chart.jpg)

In September 2025 the largest (250+ employees) companies answered “yes” around 12% of the time to the question about whether they used AI in “producing goods or services”.

Under the new “any business function” wording, by February 2026, those same large companies answered “yes” around 30% of the time, and by October 2026 that had increased to around 40%.

The data in the above chart comes from the “Employment size class” XLSX file, [available from here](https://www.census.gov/hfp/btos/data_downloads). Here’s [a copy](https://static.simonwillison.net/static/2026/Employment-Size-Class-Nov-1-2026-copy.xlsx) of the file I used.

You can view [the survey document](https://www.census.gov/hfp/btos/downloads/BTOS%20Content%20V5%20Core%20Round%203,%204.pdf) that is sent out to businesses—here’s [my snapshot copy](https://static.simonwillison.net/static/2026/BTOS-Content-V5-Core-Round-3-4.pdf) of that 346KB PDF.

#### The changing language of the questions

As the chart demonstrates, the question asked by the Bureau changed in a material way on November 17th, 2025. Prior to that date, the question they asked was this (emphasis mine):

> In the last two weeks, did this business use Artificial Intelligence (AI) in **producing goods or services**? (Examples of AI: machine learning, natural language processing, virtual agents, voice recognition, etc.)

On December 3rd, 2025, [the Bureau announced](https://www.census.gov/hfp/btos/downloads/AI%20Question%20Wording%20Updates.pdf) a significant change to that question. Here’s the replacement:

> In the last two weeks, did this business use Artificial Intelligence (AI) in **any of its business functions**? (Examples of AI: machine learning, natural language processing, virtual agents, voice recognition, etc.)

No more “producing goods or services”—now it’s “in any of its business functions”! This means that the answers to the question before and after November 17th are not directly comparable.

#### From October 19, 2026, they ask about Super Intelligence

When I went looking for the updated question language in the latest XLSX file, I couldn’t find it. I found this instead:

> In the last two weeks, did this business **use Super Intelligence or SI** (formerly referred to as Artificial Intelligence or AI) in any of its business functions? (Examples of SI use include: SI-powered data analysis; generative SI for marketing, product design, or coding; virtual SI agents for customer service; SI-powered forecasting or pricing; SI-driven computer vision for quality control, etc.)

The [BTOS about page](https://www.census.gov/hfp/btos/about) explains what happened:

> Beginning with the October 19, 2026 collection, [instrument wording](https://www.census.gov/hfp/btos/downloads/BTOS%20Content%20V5%20Core%20Round%203,%204.pdf) was updated due to [Executive Order 14434](https://www.whitehouse.gov/presidential-actions/2026/09/inaugurating-the-era-of-super-intelligence/).

The current numbers reflect the old Artificial Intelligence survey wording—the first survey results reflecting the Super Intelligence wording are due for publication on Thursday.

#### Is this going to affect the survey?

The obvious open question here is whether this wording change is going to affect the survey results.

The businesses answering this survey will have last seen that question twelve weeks ago, when it was still called AI. Hopefully the new language isn’t going to confuse them, especially since the survey helpfully spells out “formerly referred to as Artificial Intelligence or AI”.

I can’t help but laugh at the absurdity of “SI-powered data analysis; generative SI for marketing, product design, or coding; virtual SI agents for customer service” though.
