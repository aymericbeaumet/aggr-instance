---
title: 'Workflows: Creating early fraud alerts for streamlined refunds'
link: https://stripe.dev/blog/workflows-creating-early-fraud-alerts-for-streamlined-refunds
source: stripe-com
published: 2025-07-10T00:00:00Z
updated: 2025-07-10T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Ashley Ansari
summary: Workflow patterns provide repeatable solutions to common developer problems. This pattern helps automate creating early fraud alerts when streamlining refunds.
content: extracted
html: 2025-07-10-workflows-creating-early-fraud-alerts-for-streamlined.html
remote_preview:
  url: https://stripe.dev/og/blog/workflows-creating-early-fraud-alerts-for-streamlined-refunds.png
  alt: 'Stripe Developer Blog: Workflows: Creating early fraud alerts for streamlined refunds'
---

/Article

Detecting potentially fraudulent transactions before they are processed can help you mitigate losses, reduce financial impact, and earn customer trust.

Using this Stripe Workflows pattern, you can automate refund operations, quickly issuing refunds when disputing a charge is more costly. This approach streamlines operations while increasing customer satisfaction by automatically refunding customers under the threshold before a formal dispute is filed, ensuring a smoother and more responsive transaction experience.

![](https://stripe.dev/images/workflows-creating-early-fraud-alerts-for-streamlined-refunds/image1.png)

## Building an early fraud warning workflow

To build an early fraud warning workflow:

1. Select the trigger: **Early fraud warning is created in Radar**.
2. Add a step: Choose add action - **Retrieve a charge**.
3. Charge ID is required: Select **Radar early fraud warning | Charge ID** to pass through the Charge ID.
4. Add a condition.
5. Click the “If this condition is met” box and select **Charge | Amount**. Choose “is less than” and type in 15 and select USD. Click done.
6. Add a step: Add action - select **Create a refund**.

*Optional email notification for manual review:*

7. Click the “If the condition isn’t met,” then select Add action - Choose Email a team member. From the dropdown, select a team member and draft the email body text.

## What is Stripe Workflows?

[Stripe Workflows](https://docs.stripe.com/workflows/define-workflows) provides a visual builder in the Stripe dashboard, to help you automate tasks and processes by defining a series of actions that happen sequentially. Workflows is ideal for multi-step processes and is compatible across multiple Stripe products, allowing you to streamline processes, enforce business rules, and reduce manual effort.

Each workflow consists of a trigger and a series of steps that run in order. A step is either an action or a condition.

To learn more, visit [Get started with Stripe Workflows](https://stripe.dev/blog/introducing-stripe-workflows).

## What is Stripe Radar?

[Stripe Radar](https://docs.stripe.com/radar) provides real-time fraud protection and requires no additional development time. [Radar for Fraud Teams](https://stripe.com/radar/fraud-teams) adds customization capabilities and deeper insights and trend analysis for your business. Radar for Platforms, currently in public preview, provides protection against both transaction and account risk.

To learn more about developing applications with Stripe, visit our [YouTube Channel](https://www.youtube.com/stripedevelopers).

- [Subscribe to Stripe Developers on YouTube.](https://www.youtube.com/stripedevelopers)
- [Check out the docs for the in-depth developer guidance.](https://docs.stripe.com/)
- [Join the Stripe Discord server to chat live with other developers.](https://discord.com/invite/RuJnSBXrQn)
- [Join a local Stripe Developer Meetup to learn about the latest features and network with your community.](https://www.meetup.com/pro/stripe/)

\[ Fig. 1 \]

10x

\[ Fig. 2 \]

10x

/Docs

Explore our guides and examples to integrate Stripe.

[Learn more](https://docs.stripe.com)

/Social

/Resources

© 2026 Stripe, Inc.
