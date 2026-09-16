---
title: A scorecard for the AI age
link: https://openai.com/index/a-scorecard-for-the-ai-age
source: openai-com-news
published: 2026-07-17T10:00:00Z
updated: 2026-07-17T10:00:00Z
first_seen: 2026-09-07T17:03:44.343711062Z
replicated_at: 2026-09-16T20:24:49.200228376Z
labels:
- company
summary: Sarah Friar, CFO of OpenAI, introduces a practical AI scorecard to measure ROI through useful work, cost per successful task, dependability, and return on compute.
content: extracted
html: 2026-07-17-a-scorecard-for-the-ai-age.html
preview:
  file: 2026-07-17-a-scorecard-for-the-ai-age.preview-410f89d7f467.webp
  width: 256
  height: 144
  color: '#99a0df'
images:
- source: https://images.ctfassets.net/kftzwdyauwt9/2AALaD60DKuRkqenMyd9By/02ee9fe9a433a13f51bcb8cff7657d91/Frame-1.png?w=1600&h=900&fit=fill
  original:
    file: 2026-07-17-a-scorecard-for-the-ai-age.image-7c777101f757.png
    width: 1600
    height: 900
  color: '#8896e7'
---

The question I hear from CFOs everywhere is simple: how do we get more value from our AI spend?

For years, the market measured the success of software through adoption: seats purchased, users active, licenses renewed. Understanding the value of AI demands a more powerful measure: work accomplished.

The basic economic question facing CFOs and other business leaders is whether the value of the work AI completes grows faster than the cost of producing it.

Answering that question requires looking more deeply than a metric such as cost per token. A lower-cost model may have cheaper tokens, but getting great results may require more attempts, more time, or more human review. A more capable model may have more expensive tokens, but complete the same task in one pass. What matters is the full cost of producing a successful outcome, measured against the value that outcome creates.

The ultimate scorecard for the age of AI could be looked at as “Useful Intelligence per Dollar.” This metric answers four key questions:

1. Is AI completing work that matters?
2. What does each successful task cost?
3. Can people depend on the result?
4. Does each AI dollar produce more value as usage grows?

## 1\. How much useful work gets done?

Start with the work itself.

How many customer issues did AI help resolve? How many code changes did it help ship? How many contracts did it review? How much time did it give back to people? How many decisions improved because the right context was available at the right moment?

Tokens create value when they transform into work people can use. As models become more capable, they can take on longer and more complex tasks: maintaining context, reasoning through multiple steps, working across tools, and adapting as they go.

The best place to begin is with one workflow. Define what “done” means and measure that outcome in the system where the work happens.

For a support team, “done” might mean a customer issue resolved. For an engineering team, it might mean a code change that passes its tests. For a legal team, it might mean a contract reviewed accurately and on time.

Consider a finance team preparing for a forecast review. Much of the work happens before a final decision is made: finding the latest forecast, moving data into Excel or Sheets, identifying changes, reconciling tabs, rebuilding slides, and checking that everything adds up perfectly.

ChatGPT Work can take on much of that process, giving the team more time to focus on the questions that matter: What changed? Why? What should we do next?

That is useful intelligence per dollar in practice. More work gets completed, faster, while people spend more of their time applying judgment, creativity, and expertise.

## 2\. What does a successful task actually cost?

The next question is what it costs to complete that work well.

AI tasks vary widely. A quick answer may require little compute. A coding, research, or financial workflow may involve deeper reasoning, tool use, and many actions. Those more complex tasks can require more compute, but they can create much more value.

At the model level, cost per successful task depends on price, the amount of compute used, and the likelihood of reaching the right result. For a business, the full cost also includes employee time, human review, retries, and rework.

The calculation is straightforward:

- Add the full cost of completing the work.
- Count the tasks that met the required quality bar.
- Divide the full cost by the number of successful tasks.

This is why the lowest price per token does not always produce the lowest cost per outcome. A frontier model may deliver the best value even for a routine request if it produces the right answer in one pass, reducing retries, latency, review, and total compute.

A tiered model family gives customers more ways to optimize this equation. GPT‑5.6, which we released last week, has three tiers: Sol is our flagship; Terra balances performance and cost; Luna is our fastest and most affordable model.

These tiers provide useful starting points. The economics of the full task should ultimately determine the right model. A customer might use Luna for a fast, high-volume workflow, Terra for work requiring greater depth, or Sol when stronger reasoning delivers the best result with fewer attempts.

We trained GPT‑5.6 to get more useful work from every token. On the Artificial Analysis Coding Agent Index, GPT‑5.6 Sol with max reasoning set a new state of the art while using 54% fewer output tokens than another leading model. The chart below illustrates the comparison.

DeepSWE v1.1 — Coding (chart data)
| Juice level | Model                  | X label       | X metric       | Juice index | Score  | X value   |
| ----------- | ---------------------- | ------------- | -------------- | ----------- | ------ | --------- |
| low         | Claude Fable 5         | 10.58 minutes | latency\_s     | 1           | 0.5958 | 10.5784   |
| medium      | Claude Fable 5         | 13.51 minutes | latency\_s     | 2           | 0.6537 | 13.515    |
| high        | Claude Fable 5         | 17.71 minutes | latency\_s     | 3           | 0.686  | 17.7051   |
| xhigh       | Claude Fable 5         | 23.53 minutes | latency\_s     | 4           | 0.6991 | 23.5264   |
| max         | Claude Fable 5         | 34.87 minutes | latency\_s     | 5           | 0.6972 | 34.8668   |
| high        | Gemini 3.1 Pro Preview | 36.64 minutes | latency\_s     | 3           | 0.1175 | 36.6448   |
| low         | GPT-5.5                | 3.14 minutes  | latency\_s     | 1           | 0.2699 | 3.1408    |
| medium      | GPT-5.5                | 7.08 minutes  | latency\_s     | 2           | 0.5398 | 7.0772    |
| high        | GPT-5.5                | 11.73 minutes | latency\_s     | 3           | 0.6438 | 11.7339   |
| xhigh       | GPT-5.5                | 18.44 minutes | latency\_s     | 4           | 0.6704 | 18.4416   |
| none        | GPT-5.6 Sol            | 5.73 minutes  | latency\_s     | 0           | 0.4447 | 5.7302    |
| low         | GPT-5.6 Sol            | 3.45 minutes  | latency\_s     | 1           | 0.4535 | 3.4512    |
| medium      | GPT-5.6 Sol            | 6.31 minutes  | latency\_s     | 2           | 0.6106 | 6.3114    |
| high        | GPT-5.6 Sol            | 10.16 minutes | latency\_s     | 3           | 0.694  | 10.161    |
| xhigh       | GPT-5.6 Sol            | 14.89 minutes | latency\_s     | 4           | 0.7073 | 14.8913   |
| max         | GPT-5.6 Sol            | 24.02 minutes | latency\_s     | 5           | 0.7267 | 24.0151   |
| none        | GPT-5.6 Luna           | 1.54 minutes  | latency\_s     | 0           | 0.0221 | 1.5386    |
| low         | GPT-5.6 Luna           | 0.69 minutes  | latency\_s     | 1           | 0.0155 | 0.6854    |
| medium      | GPT-5.6 Luna           | 1.94 minutes  | latency\_s     | 2           | 0.1128 | 1.9444    |
| high        | GPT-5.6 Luna           | 7.61 minutes  | latency\_s     | 3           | 0.4425 | 7.6053    |
| xhigh       | GPT-5.6 Luna           | 15.22 minutes | latency\_s     | 4           | 0.5686 | 15.2193   |
| max         | GPT-5.6 Luna           | 28.52 minutes | latency\_s     | 5           | 0.6719 | 28.517    |
| none        | GPT-5.6 Terra          | 1.86 minutes  | latency\_s     | 0           | 0.14   | 1.8562    |
| low         | GPT-5.6 Terra          | 2.1 minutes   | latency\_s     | 1           | 0.2405 | 2.1004    |
| medium      | GPT-5.6 Terra          | 2.9 minutes   | latency\_s     | 2           | 0.3511 | 2.9038    |
| high        | GPT-5.6 Terra          | 5.44 minutes  | latency\_s     | 3           | 0.5376 | 5.4442    |
| xhigh       | GPT-5.6 Terra          | 10.3 minutes  | latency\_s     | 4           | 0.6018 | 10.302    |
| max         | GPT-5.6 Terra          | 23.15 minutes | latency\_s     | 5           | 0.6962 | 23.1456   |
| low         | Claude Opus 4.8        | 10.86 minutes | latency\_s     | 1           | 0.408  | 10.8638   |
| medium      | Claude Opus 4.8        | 14.67 minutes | latency\_s     | 2           | 0.4867 | 14.6668   |
| high        | Claude Opus 4.8        | 17.78 minutes | latency\_s     | 3           | 0.5177 | 17.7817   |
| xhigh       | Claude Opus 4.8        | 36.65 minutes | latency\_s     | 4           | 0.5436 | 36.6462   |
| max         | Claude Opus 4.8        | 58.21 minutes | latency\_s     | 5           | 0.5897 | 58.2124   |
| low         | Claude Fable 5         | $$3.76        | api\_cost\_usd | 1           | 0.5958 | 3.7579    |
| medium      | Claude Fable 5         | $$6.09        | api\_cost\_usd | 2           | 0.6537 | 6.0882    |
| high        | Claude Fable 5         | $$9.18        | api\_cost\_usd | 3           | 0.686  | 9.1776    |
| xhigh       | Claude Fable 5         | $$13.41       | api\_cost\_usd | 4           | 0.6991 | 13.4145   |
| max         | Claude Fable 5         | $$21.63       | api\_cost\_usd | 5           | 0.6972 | 21.6347   |
| high        | Gemini 3.1 Pro Preview | $$9.48        | api\_cost\_usd | 3           | 0.1175 | 9.4815    |
| low         | GPT-5.5                | $$1.23        | api\_cost\_usd | 1           | 0.2699 | 1.2315    |
| medium      | GPT-5.5                | $$2.85        | api\_cost\_usd | 2           | 0.5398 | 2.8497    |
| high        | GPT-5.5                | $$5.29        | api\_cost\_usd | 3           | 0.6438 | 5.2865    |
| xhigh       | GPT-5.5                | $$7.47        | api\_cost\_usd | 4           | 0.6704 | 7.4691    |
| none        | GPT-5.6 Sol            | $$2.43        | api\_cost\_usd | 0           | 0.4447 | 2.4303    |
| low         | GPT-5.6 Sol            | $$1.19        | api\_cost\_usd | 1           | 0.4535 | 1.1856    |
| medium      | GPT-5.6 Sol            | $$2.01        | api\_cost\_usd | 2           | 0.6106 | 2.0119    |
| high        | GPT-5.6 Sol            | $$3.81        | api\_cost\_usd | 3           | 0.694  | 3.8106    |
| xhigh       | GPT-5.6 Sol            | $$5.03        | api\_cost\_usd | 4           | 0.7073 | 5.0312    |
| max         | GPT-5.6 Sol            | $$8.56        | api\_cost\_usd | 5           | 0.7267 | 8.5615    |
| none        | GPT-5.6 Luna           | $$0.19        | api\_cost\_usd | 0           | 0.0221 | 0.1867    |
| low         | GPT-5.6 Luna           | $$0.08        | api\_cost\_usd | 1           | 0.0155 | 0.0831    |
| medium      | GPT-5.6 Luna           | $$0.25        | api\_cost\_usd | 2           | 0.1128 | 0.2456    |
| high        | GPT-5.6 Luna           | $$0.86        | api\_cost\_usd | 3           | 0.4425 | 0.8563    |
| xhigh       | GPT-5.6 Luna           | $$1.62        | api\_cost\_usd | 4           | 0.5686 | 1.6207    |
| max         | GPT-5.6 Luna           | $$2.88        | api\_cost\_usd | 5           | 0.6719 | 2.8781    |
| none        | GPT-5.6 Terra          | $$0.41        | api\_cost\_usd | 0           | 0.14   | 0.4092    |
| low         | GPT-5.6 Terra          | $$0.48        | api\_cost\_usd | 1           | 0.2405 | 0.4775    |
| medium      | GPT-5.6 Terra          | $$0.65        | api\_cost\_usd | 2           | 0.3511 | 0.646     |
| high        | GPT-5.6 Terra          | $$1.25        | api\_cost\_usd | 3           | 0.5376 | 1.2517    |
| xhigh       | GPT-5.6 Terra          | $$2.32        | api\_cost\_usd | 4           | 0.6018 | 2.325     |
| max         | GPT-5.6 Terra          | $$5.05        | api\_cost\_usd | 5           | 0.6962 | 5.0495    |
| low         | Claude Opus 4.8        | $$2.29        | api\_cost\_usd | 1           | 0.408  | 2.2934    |
| medium      | Claude Opus 4.8        | $$3.44        | api\_cost\_usd | 2           | 0.4867 | 3.4439    |
| high        | Claude Opus 4.8        | $$4.28        | api\_cost\_usd | 3           | 0.5177 | 4.2819    |
| xhigh       | Claude Opus 4.8        | $$8.01        | api\_cost\_usd | 4           | 0.5436 | 8.0064    |
| max         | Claude Opus 4.8        | $$13.22       | api\_cost\_usd | 5           | 0.5897 | 13.2226   |
| low         | Claude Fable 5         | 25.24K        | output\_tokens | 1           | 0.5958 | 25242.83  |
| medium      | Claude Fable 5         | 40.2K         | output\_tokens | 2           | 0.6537 | 40201.35  |
| high        | Claude Fable 5         | 57.29K        | output\_tokens | 3           | 0.686  | 57287.07  |
| xhigh       | Claude Fable 5         | 80.35K        | output\_tokens | 4           | 0.6991 | 80352.17  |
| max         | Claude Fable 5         | 118.59K       | output\_tokens | 5           | 0.6972 | 118592.77 |
| high        | Gemini 3.1 Pro Preview | 196.39K       | output\_tokens | 3           | 0.1175 | 196386.1  |
| low         | GPT-5.5                | 9,443         | output\_tokens | 1           | 0.2699 | 9442.77   |
| medium      | GPT-5.5                | 19.63K        | output\_tokens | 2           | 0.5398 | 19625.43  |
| high        | GPT-5.5                | 31.16K        | output\_tokens | 3           | 0.6438 | 31159.5   |
| xhigh       | GPT-5.5                | 46.3K         | output\_tokens | 4           | 0.6704 | 46294.72  |
| none        | GPT-5.6 Sol            | 14.41K        | output\_tokens | 0           | 0.4447 | 14413.81  |
| low         | GPT-5.6 Sol            | 10.58K        | output\_tokens | 1           | 0.4535 | 10579.14  |
| medium      | GPT-5.6 Sol            | 18.43K        | output\_tokens | 2           | 0.6106 | 18425.22  |
| high        | GPT-5.6 Sol            | 28.45K        | output\_tokens | 3           | 0.694  | 28450.32  |
| xhigh       | GPT-5.6 Sol            | 40.74K        | output\_tokens | 4           | 0.7073 | 40744.59  |
| max         | GPT-5.6 Sol            | 60.01K        | output\_tokens | 5           | 0.7267 | 60013.64  |
| none        | GPT-5.6 Luna           | 5,891         | output\_tokens | 0           | 0.0221 | 5891.33   |
| low         | GPT-5.6 Luna           | 3,128         | output\_tokens | 1           | 0.0155 | 3127.75   |
| medium      | GPT-5.6 Luna           | 8,180         | output\_tokens | 2           | 0.1128 | 8179.57   |
| high        | GPT-5.6 Luna           | 25.78K        | output\_tokens | 3           | 0.4425 | 25778.27  |
| xhigh       | GPT-5.6 Luna           | 44.68K        | output\_tokens | 4           | 0.5686 | 44677.9   |
| max         | GPT-5.6 Luna           | 73.4K         | output\_tokens | 5           | 0.6719 | 73399.71  |
| none        | GPT-5.6 Terra          | 6,737         | output\_tokens | 0           | 0.14   | 6737.29   |
| low         | GPT-5.6 Terra          | 8,572         | output\_tokens | 1           | 0.2405 | 8572.26   |
| medium      | GPT-5.6 Terra          | 11.75K        | output\_tokens | 2           | 0.3511 | 11746.56  |
| high        | GPT-5.6 Terra          | 21.52K        | output\_tokens | 3           | 0.5376 | 21517.04  |
| xhigh       | GPT-5.6 Terra          | 39.62K        | output\_tokens | 4           | 0.6018 | 39616.54  |
| max         | GPT-5.6 Terra          | 71.94K        | output\_tokens | 5           | 0.6962 | 71938.63  |
| low         | Claude Opus 4.8        | 28.92K        | output\_tokens | 1           | 0.408  | 28922.74  |
| medium      | Claude Opus 4.8        | 41.31K        | output\_tokens | 2           | 0.4867 | 41313.45  |
| high        | Claude Opus 4.8        | 50.06K        | output\_tokens | 3           | 0.5177 | 50063.53  |
| xhigh       | Claude Opus 4.8        | 86.09K        | output\_tokens | 4           | 0.5436 | 86088.79  |
| max         | Claude Opus 4.8        | 135.03K       | output\_tokens | 5           | 0.5897 | 135031.67 |

***DeepSWE v1.1****: Long-horizon engineering tasks; GPT‑5.6 Sol reaches a new high of 72.7%, above Claude Fable 5’s 69.9%, at 36.2% lower estimated API cost.*

Across the GPT‑5.6 family, the goal is the same: more successful work per dollar. Greater efficiency makes existing tasks more affordable. Greater capability makes entirely new kinds of work possible.

Each new model generation should improve both sides of that equation. Customers should be able to accomplish more valuable work while, at the same time, the cost of completing each task continues to fall.

## 3\. How often does AI get the work right?

The third measure is dependability.

AI adoption tends to deepen in stages. First, AI helps draft. Then it finds context and reasons across tools and data. Over time, it begins taking action, handling exceptions, and completing workflows, with people providing judgment and control where needed.

Each step creates more value and asks more of the system.

Dependability has direct economic value. When results are accurate, well-sourced, consistent, and escalated appropriately, people spend less time reviewing, correcting, and repeating the work. Successful tasks cost less, and organizations gain the confidence to use AI in more important workflows.

Teams can make this concrete by tracking three outcomes:

- Ready to use: The result met the quality bar as delivered.
- Needs correction: The result required another attempt or human edits.
- Needs escalation: A person needed to step in and finish the work.

These measures tell a richer story than model accuracy alone. They show whether AI is genuinely reducing the work involved in completing the project.

Dependability also requires clear boundaries. Before AI moves from drafting to taking action, organizations should define:

- What data the system can access.
- What systems it can use or change.
- When a person should review or approve an action.

Safety, security, privacy, and control create the foundation for deeper use. People need to understand how the system behaves, how their data is handled, and how its actions are governed.

ChatGPT Work builds on the security, privacy, compliance, and workspace-management foundation of ChatGPT Enterprise. This allows organizations to give AI more context and access to more valuable workflows while maintaining appropriate oversight.

Capability earns first use. Dependability makes AI part of how work gets done.

## 4\. Does each AI dollar buy more work as usage grows?

The final question is whether the economics improve at scale.

Companies can measure this by following the same workflow over time. Track how many tasks met the quality bar, the total cost of completing them, and the cost per successful task. If completed work grows faster than total cost while quality holds or improves, each AI dollar is producing more value.

Compute sits at the center of this equation.

Compute powers research and every task that AI completes. It shapes product quality, speed, dependability, availability, and cost. Training compute builds future capability. Inference compute delivers useful work today. Both should translate into better outcomes for customers.

Better models, more efficient inference, purpose-built hardware, higher utilization, smarter routing, and stronger product design all improve the return on compute. Each generation of infrastructure helps train more capable models. Better algorithms, hardware, and software then help serve those models more efficiently.

Customers experience those improvements in human terms: better answers, faster results, fewer corrections, more dependable products, and a lower cost for the work they need done.

The gains compound. Better infrastructure accelerates research. Research produces more capable and efficient models. Better models improve products. Better products drive adoption, learning, and revenue. That growth supports continued investment in the next generation of research, compute, deployment, and safety.

OpenAI brings these pieces together through one shared intelligence platform. People use it through ChatGPT and ChatGPT Work. Developers build with it through Codex and the API. Enterprises deploy it into the systems where work happens.

When one layer improves, every product and customer can benefit.

## A scorecard for the AI age

Taken together, these four measures tell us whether useful intelligence per dollar is improving.

Useful work tells us what AI produces. Cost per successful task tells us what it takes to reach the outcome. Dependability tells us how much of the work people can confidently use. Value at scale tells us whether each dollar, and each unit of compute, accomplish more over time.

The goal is AI that helps people do more meaningful work, make better decisions, and spend more time on the parts of their jobs that require distinctly human judgment and creativity.

Our job is to make that equation better with every generation: more capable models, faster and more dependable results, and lower costs for the work customers need done.

That is how AI becomes more useful to more people and organizations over time.
