---
title: Setting Grok Bot loose on procurement
link: https://x.ai/news/grok-bot-procurement
source: x-ai-news
published: 2026-09-04T00:00:00Z
first_seen: 2026-09-08T10:17:10.166315Z
content: extracted
html: 2026-09-04-setting-grok-bot-loose-on-procurement.html
preview:
  file: 2026-09-04-setting-grok-bot-loose-on-procurement.preview-e436b486e2e4.webp
  width: 256
  height: 134
  color: '#766892'
---

Enterprises have a lot of spend that is hard to track closely. That includes unused SaaS seats that accumulate as teams change, and renewals that come up before anyone has checked whether the contract still matches how the product is being used. The same problem shows up in recurring purchases, where it is often easier to reorder what was bought last time than to adjust quantities to current needs or shop around for a better price.

A lot of this work is worth doing, but hard to justify doing by hand. We wanted to see how much of it Grok Bot could take on.

Grok Bot makes agents much easier to set up and use. You tell or show a Bot what you want it to do, give it access to the right tools, and let it execute without designing the workflow step by step.

For procurement, we created a Bot we affectionately named Haggle Bot. It reads our vendor spend alongside contracts and usage data, then uses market pricing and competitive quotes to find savings and prepare negotiations. So far, it has identified more than $100,000 in direct savings, worked through larger SaaS renewals, and applied the same approach to recurring purchases like office supplies.

We think Haggle Bot points to a broader role for Bots inside a company. Give a Bot a clear job and access to the tools it needs, and it can keep taking on the work within that role without being told each task.

## [Procurement is a good task for a Bot](https://x.ai/news/grok-bot-procurement#procurement-is-a-good-task-for-a-bot)

The goal of procurement is to support the business and get the most out of every dollar spent on vendors. A lot of existing procurement tools help with this at the vendor intake and contract-management level.

But much of the most important procurement work hinges on questions those systems do not address:

- Who is the owner for this?
- Why do we need it?
- Have we explored alternatives?
- Why do we have X and Y if they do the same thing?
- Why are our costs spiking?
- Everyone has a license. Are they using it?

These questions are common, but depending on the stage of the company, they may not be worth answering by hand. Fast-growing companies often leave money on the table to avoid spending time chasing them down. That is where a Bot can be useful.

## [A simple expression of intent](https://x.ai/news/grok-bot-procurement#a-simple-expression-of-intent)

We started Haggle Bot by describing the job we wanted it to do. Its instruction was to learn our vendor spend and turn that knowledge into evidence-backed savings, with a person making the final decisions.

Procurement

### Haggle Bot current system prompt

We then gave the Bot access to Slack, Notion, Drive, Gmail, Hex, and Ramp. Haggle Bot used those systems to build a working map of roughly 125 active vendors.

That record gave Haggle Bot enough context to make decisions and keep working without a person spelling out each next step. We also defined where Haggle Bot should stop and hand back control. We let it handle internal research and coordination on its own, but required explicit approval for spending money, accepting terms, or sending something to a vendor.

## [Finding unused SaaS seats](https://x.ai/news/grok-bot-procurement#finding-unused-saas-seats)

An easy place to start was auditing SaaS spend. Haggle Bot asked our IT team for assigned-seat and last-used data, then compared that with what we were paying for. For one product, it found 43 paid seats with no activity in the previous 90 days and sent the names back for review and downgrade. That added up to $14,220 in savings.

We applied the same approach to another SaaS product and Haggle Bot found $85,662 a year in unused SKUs. Because the product was month-to-month, those cuts reduced spend immediately.

One thing we noticed in these SaaS audits was how often Haggle Bot took the next step without being asked. In one case, Haggle Bot needed to figure out who owned the relationship and what our plans were for the vendor. It started with the owners listed in Ramp, messaged them, and followed each handoff until it reached the engineers with the right context to make a decision. Rather than stop when the first answer was incomplete, it identified what additional information it needed and then proactively sought it out.

## [Negotiating a SaaS renewal](https://x.ai/news/grok-bot-procurement#negotiating-a-saas-renewal)

When a SaaS renewal came up, we asked Haggle Bot to review the quote and explore alternatives. Haggle Bot compared the renewal offer with our current annualized spend and recommended against the options that added seats ahead of demonstrated usage.

![Haggle Bot reviews a contract renewal proposal, breaks the quote down against current spend, and recommends the cheaper option](https://x.ai/news/f=auto/v1/website/haggle-bot-image1-75f1097a.png)

It then priced credible alternatives against our current footprint and used those comparisons alongside current usage data to work out where we had negotiating leverage.

![Haggle Bot compares Webex, webinar platforms, and Zoom pricing against the renewal quote](https://x.ai/news/f=auto/v1/website/haggle-bot-image2-7f0e2a56.png)

From there, Haggle Bot worked out the negotiation and drafted the response for us to edit. We set the minimum quantities we wanted to keep and approved the send. Haggle Bot made an opening bid while setting an internal price target we were willing to go up to.

Every Friday, our office team orders tech, snacks, and hygiene supplies for the next group of new hires. We use another Bot to place that order, which we call "Amazon Bot." It's logged into our corporate account and can work across Gmail, Ramp, Google Sheets, Rippling, and Vercel to understand headcount and the office floor plan.

![Amazon Bot creates standing office supply orders based on weekly new-hire counts from Slack](https://x.ai/news/f=auto/v1/website/haggle-bot-image3-b5f9709c.png)

Rather than treat that as a fixed Amazon order, we gave Haggle Bot the job of shopping the weekly order around. Haggle Bot can see how quickly supplies are being used, the seat map for each building, and the quotes and carts from the last four orders. From that, it builds an editable Google Sheet where office ops can change the number of incoming hires and see the quantities needed for each new-hire kit.

Haggle Bot then shops the order across Amazon, Costco, Uline, and Walmart. If it cannot find the same product for less, it can look for an equivalent from another brand, then collect the comparisons in a sheet for review.

It then drafts an email to our Amazon procurement rep with same-day competitor prices and asks for lower pricing on specific line items through our business discount program.

![Haggle Bot drafts an email to our Amazon procurement rep citing same-day competitor prices on specific line items](https://x.ai/news/f=auto/v1/website/haggle-bot-image6-f62bac1c.png)

Once pricing is settled, Haggle Bot delegates the order back to Amazon Bot to send. In one run, the process brought a $14,629 tech order down to $6,143, a 58% reduction.

## [Where Haggle Bot is most useful](https://x.ai/news/grok-bot-procurement#where-haggle-bot-is-most-useful)

Haggle Bot shows how much work can be done behind a simple expression of intent. Once a Bot has a purpose and the access to pursue it, it can keep making progress across systems without additional intervention.

Haggle Bot has also helped us see where Bots fit naturally into a workflow, and where they need more guidance. A lot of procurement work depends on good judgment about how to communicate with vendors, and we still revise its emails to calibrate on tone and make sure we're providing vendors with the right level of information.

Haggle Bot has made us interested in what Bots can do when they keep working on the same job over time. The examples above came from a short window, but many of the signals worth acting on only emerge gradually as spend and usage change. Over the next year, we expect Haggle Bot to find more of that work on its own and take it further before a person needs to step in.

Join the Grok Bot for Enterprise [waitlist](https://cursor.com/contact-sales?product=grok-bot).
