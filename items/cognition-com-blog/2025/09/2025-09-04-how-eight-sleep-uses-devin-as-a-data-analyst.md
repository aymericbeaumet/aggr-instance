---
title: How Eight Sleep Uses Devin as a Data Analyst
link: https://cognition.com/blog/how-eight-sleep-uses-devin-as-a-data-analyst
source: cognition-com-blog
published: 2025-09-04T00:00:00Z
first_seen: 2026-09-04T12:14:41.802665207Z
content: extracted
html: 2025-09-04-how-eight-sleep-uses-devin-as-a-data-analyst.html
---

Building the future of sleep requires us to make data-driven product, operations, and finance decisions. But as a small team, we faced a challenge every scaling company knows well: the flood of *random data asks*.

> "Can you pull yesterday's sales by channel? Can you check why this number looks off? Can we get a new dashboard for tomorrow's launch"

Even with dbt, Looker, and Snowflake powering our analytics stack, these requests piled up. Our queue of ad-hoc tasks grew faster than we could ship. When a new report looked suspicious, we had to stop everything, investigate, and reassure stakeholders. It slowed us down.

Our [Eight Sleep](https://www.eightsleep.com/) [metrics](https://www.eightsleep.com/blog/hrv-accuracy/?srsltid=AfmBOorrreRU0f1x367XcGJ2p0BkSJIK7XAaQcDEPgxjg4lKU-cDtqTU) took a hit.

![How Eight Sleep Uses Devin as a Data Analyst](https://cdn.sanity.io/images/2mc9cv2v/production/bc67c8fa956ff3bedc6e226482ce1465ba65d4dd-2639x1508.png?w=1600&fit=max)

We needed leverage. So we hired an analyst who never sleeps: **Devin AI.**

## **Bringing Devin into the workflow**

Instead of forcing people in the team to funnel every question through our small data team, we integrated [Cognition’s Devin](https://devin.ai/) directly into our workflows:

- **Slack as the interface.** Anyone can tag Devin in our data channel.

![How Eight Sleep Uses Devin as a Data Analyst](https://cdn.sanity.io/images/2mc9cv2v/production/2721ebbe00d925a01ef5cf1f8aed31073e0aface-1388x148.png?w=1600&fit=max)

![How Eight Sleep Uses Devin as a Data Analyst](https://cdn.sanity.io/images/2mc9cv2v/production/0214ae377a0de358093bf00dc1ef3af27c8fd1cc-1096x508.png?w=1600&fit=max)

- **Access to our stack.** Devin understands our dbt repo, queries Snowflake, uses Looker, and even checks data on Amplitude via its web browser.
- **Context is king, but needs to be built.** We started by just giving Devin our repo and tools (“vibe context”). Then we layered in curated knowledge to help it find the right data. Like training a new employee or intern, we fed it more specific knowledge to support it for each task it took on.

The setup only took a couple of hours from getting access to our account to asking Devin my first question via Slack. From there, Devin began fielding real data requests immediately.

![How Eight Sleep Uses Devin as a Data Analyst](https://cdn.sanity.io/images/2mc9cv2v/production/923c0142f66f73d7584a4de0c8e6dc0c8e0e1304-2638x1509.png?w=1600&fit=max)

## **Hero moment: When revenue looked suspicious**

A few weeks ago, our brand-new daily sales dashboard lit up with an unusually high revenue number. Theories flew:

- Was the new data model wrong?
- Did Looker mis-aggregate something?
- Was there a real issue with orders?

![How Eight Sleep Uses Devin as a Data Analyst](https://cdn.sanity.io/images/2mc9cv2v/production/c43385800f7ed97323c253930135087a8a3935cb-1252x1568.png?w=1600&fit=max)

Instead of a human analyst burning hours to untangle the logic, we asked Devin to investigate. It traced the Looker dashboard back to the underlying queries, ran checks in Snowflake, and quickly found the culprit: an email campaign that performed better than expected.

![How Eight Sleep Uses Devin as a Data Analyst](https://cdn.sanity.io/images/2mc9cv2v/production/6f3759f8faca93ab05cee05f6560c3352b37db5c-1270x1108.png?w=1600&fit=max)

The revenue was real. The model was fine. And Devin had defused what could have turned into a fire drill.

## **The impact**

Since bringing Devin into our analytics stack, we’ve seen transformative results:

- **Faster shipping.** We’re shipping **3x as many data features and data investigations** each week compared to before (large requests involve: new dbt models, semantic models, dashboards).
- **Democratized access.** Our queue of random “pull data” asks is near zero. Devin now fields ad-hoc requests.
- **Truth under pressure** Devin has helped us resolve “this number looks off” moments multiple times, writing the exact queries needed to validate results.

![How Eight Sleep Uses Devin as a Data Analyst](https://cdn.sanity.io/images/2mc9cv2v/production/b24b1a71a5901357532ee720dbc70137cf5fe9fb-1166x898.png?w=1600&fit=max)

- **New voices, increased curiosity.** People who never asked our team for data before are now using Devin. It turns out folks might hesitate to ping if they’re just curious, but never feel guilty about asking an AI. Devin is now answering questions for Product, Ops, Finance, Growth, and R&D.
- **Organic adoption.** As teammates saw Devin in action, more asked to join. Every day a few new folks see the magic in our Slack channel and ask for access. It really feels like internal product market fit.

For our small team, Devin has become an invaluable teammate. It’s helped us work more efficiently and make better decisions.

## \
**Building at the frontier of AI analysts**

Reading more about how different people approach the AI Analyst problem, it seems there are usually two approaches at opposite extremes.

- **Approach A:** Provide as much curated context as possible. Highly prescriptive, but labor-intensive. E.g. When a user asks specifically about revenue, you can only use this table.
- **Approach B:** Give the AI broad access to what a new analyst would get and let it explore (“vibe context?”). Fastest to the “aha moment”/activation, but less accurate. Devin was great at this, just as you’d expect a coding agent to be.

![How Eight Sleep Uses Devin as a Data Analyst](https://cdn.sanity.io/images/2mc9cv2v/production/767e88192c7cdfbe2f2c1cba3c29926603505c2f-1536x1024.png?w=1600&fit=max)

Our experience shows the future lies in between but towards exploration. AI analysts should be able to:

- **Grok your stack.** Understand dbt, LookML, and dashboards directly.
- **Leverage curated heuristics.** Analysts can capture *how we think about data* without having to prescribe every dataset.
- **Learn like an employee.** Through memory, context injection, and feedback loops, AI analysts should improve just like new hires do.
- **Operate with autonomy within clear boundaries.** Last, but certainly not least, giving Devin the right permissions is critical (just like your human teammates). By setting clear boundaries on what Devin can query, we make sure it’s powerful where we want it to be, and restricted where compliance and privacy demand it.

## **Growing Devin from a Jr Analyst to a Sr Analyst**

With Devin, you really get what you put into it 100 fold. Two months ago, Devin started as our Jr Analyst, and is close to becoming our Senior Analyst. We are experimenting with:

**Augmenting our semantic layer.** Context is king. There’s no reason not to improve your documentation and semantic layer. So we will be trialing adding additional useful context into our dbt yaml files using the [meta](https://docs.getdbt.com/reference/resource-configs/meta) field. This should help improve how Devin approaches using a dataset.

**Building a context/feedback loop.** We’re experimenting with building an automated way to provide Devin with relevant data models and context from prior sessions to improve its retrieval. We’re also experimenting with crowd sourcing our knowledge creation by building a Slack bot to ask users for feedback on Devin’s answers. These should help with relevance and groundedness.

**Experimenting with charting.** Sometimes you just want a quick chart, and you don’t really want to create a new Looker object with a visualization. Devin just launched a [Metabase MCP server](https://www.npmjs.com/package/@cognitionai/metabase-mcp-server) that Devin can use to create charts and screenshot them back to users in Slack. Check out their guide [here](https://devin.ai/ai-data-analyst-2#before-you-begin).

## \
**Back to building**

Our partnership with Cognition has been instrumental. We’re in constant contact through a shared Slack channel, collaborating on everything from semantic layer design ideas to suggestions for Devin’s roadmap.

Together, we’re pushing toward a future where AI analysts don’t just assist, they *own* the repetitive work, while assisting humans focusing on the highest-leverage problems.

At Eight Sleep, Devin has already proven the model: democratized data, faster shipping, and sharper decisions.

The next frontier is scaling this across the industry. And we can’t wait to see what the future holds.

PS: It looks like Devin, our AI intern, will indeed be getting a return offer
