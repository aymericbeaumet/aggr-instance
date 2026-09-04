---
title: New self-serve plans for Devin
link: https://cognition.com/blog/new-self-serve-plans-for-devin
source: cognition-com-blog
published: 2026-04-14T00:00:00Z
first_seen: 2026-09-04T12:14:41.802665207Z
content: extracted
html: 2026-04-14-new-self-serve-plans-for-devin.html
---

This week, we will be updating Devin’s self-serve pricing.

We’re retiring the old **Core** and **Team** plans and introducing a new lineup: **Free, Pro, Max, Teams, and Enterprise**. We’re also beginning to charge for products that have been free until now, including **Ask Devin**, **DeepWiki**, and **Devin Review**.

For many teams, this lowers the barrier to adopt Devin by replacing the old $500/month Team entry point with more flexible options. For some lighter Core users, the removal of the old no-minimum Core plan is a real change. And for products that have been free until now, pricing will now reflect the real compute they use.

We think this is the right long-term direction for two reasons: it gives customers a clearer path from trying Devin to using it regularly, and it lets us keep investing in products that are genuinely expensive to run.

## **What’s changing**

Our new self-serve plan lineup is:

- **Free** — Free
- **Pro** — $20/month, with included quota
- **Max** — $200/month, with a larger included quota
- **Teams** — Usage based, with a minimum spend of $80/month
- **Enterprise** — Custom pricing

At a high level:

- **Free** is for getting started, with limited access to Devin and selected features.
- **Pro** is for individual users who want ongoing paid usage.
- **Max** is for heavier individual usage.
- **Teams** adds collaboration, centralized billing, and admin controls.
- **Enterprise** remains for larger organizations with custom requirements.

As part of this change, the old **Core** and **Team** plans will go away. And there is one other change: for self-serve customers, usage that is included in your plan will count against quota. If you go beyond your included quota, additional usage will be priced and billed in dollars rather than ACUs. Enterprise billing will continue to use ACUs

## **What this means for current customers**

If you’re on **Core** today, you’ll move to **Free**.

Free remains free, but it does **not** preserve the old no-minimum pay-as-you-go structure. If you want ongoing paid usage beyond Free, you’ll move to one of the new paid plans.

For some lighter Core users, this will be a higher-commitment starting point than before. At the same time, many active Core users already use Devin enough that one of the new paid plans should better match how they use the product today.

If you’re on **Team** today, you’ll move to **Teams**. The new Teams plan lowers the entry point relative to the old $500/month Team plan and gives teams a simpler path to adopt Devin with shared billing and collaboration built in.

If you’re on **Enterprise**, there is no change to your agreement as part of this update.

## **We’re also starting to charge for Ask Devin, DeepWiki, and Devin Review**

We’ve offered several newer products for free or in preview while we learned how people use them. Going forward, that will change.

We’ll begin charging for:

- **Ask Devin**
- **Devin Review**
- **DeepWiki**

These products do substantial work on your behalf, and in each case their cost depends on how much model usage they consume. We want pricing to reflect that more clearly. For self-serve customers, usage that is included in your plan will count against quota. Usage beyond included quota will be priced in dollars based on the underlying model cost of each run.

The goal here is not just to offset cost. It’s also to give customers better control over when these products run, and better visibility into what they cost.

## **What that will look like in product**

We want this rollout to be predictable. That means we are pairing pricing changes with better controls and clearer cost guidance inside the product.

### **Ask Devin**

Deep Mode in Ask Devin will move to usage-based billing. All other usage remains free.

### **Devin Review**

Devin Review has been in a temporary free preview. We’re now introducing a 2-week free trial for Devin Review, and reviews after the trial will move to usage-based billing. As we begin billing for it, we’ll also add better controls for when reviews run. Similar to DeepWiki, Devin Review for open source repos will remain free.

We want customers to have much better control over when those runs happen. Instead of a one-size-fits-all “run on every commit”, as many users are doing today, Devin Review will support options like:

- Manual only
- Run when a PR is first opened
- Run on every commit

That matters because we do not want review costs to feel open-ended or hard to reason about. If Devin is going to run work automatically, customers should be able to decide how often that happens. Customers who currently have “run on every commit” turned on will be migrated to “run when a PR is first opened”.

### **DeepWiki**

DeepWiki and [deepwiki.com](http://deepwiki.com/) will remain free with the existing generation experience.

Going forward, we’ll also offer higher quality and more expensive generation options that will be subject to usage-based billing. These use stronger models and deeper thinking to create more detailed, more comprehensive wikis. You’ll be able to choose the mode that fits the job.

## **Why we’re making this change**

There are three reasons for this update.

First, we want a clearer self-serve path for Devin. The old Core and Team structure left too much distance between trying the product and adopting it more seriously. The new lineup gives customers a more gradual path from Free to Pro to Max to Teams. It also makes self-serve pricing easier to understand: included usage is part of your plan, and extra usage beyond that is shown directly in dollars.

Second, we want to lower the barrier for teams. The old Team plan’s $500/month entry point was too high for many teams that wanted to use Devin regularly but were not ready to start there.

Third, we need pricing for compute-heavy products to reflect the real work they do. Ask Devin, DeepWiki, and Devin Review all consume meaningful resources. Charging for them lets us keep improving them, while also giving us a much better signal about which workflows customers truly value.
