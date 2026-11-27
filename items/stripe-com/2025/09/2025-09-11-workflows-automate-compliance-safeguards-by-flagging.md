---
title: 'Workflows: Automate compliance safeguards by flagging transactions'
link: https://stripe.dev/blog/workflows-automate-compliance-safeguards
source: stripe-com
published: 2025-09-11T00:00:00Z
updated: 2025-09-11T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Ashley Ansari
summary: Learn how to automate compliance safeguards with Stripe Workflows by flagging high-value transactions and suspicious orders to enhance risk management.
content: extracted
html: 2025-09-11-workflows-automate-compliance-safeguards-by-flagging.html
remote_preview:
  url: https://stripe.dev/og/blog/workflows-automate-compliance-safeguards.png
  alt: 'Stripe Developer Blog: Workflows: Automate compliance safeguards by flagging transactions'
---

/Article

Many industries require more specific or stricter compliance requirements. With Stripe Workflows, you can automate additional compliance requirements to align with your business needs. You can implement a workflow to automatically flag transactions based on criteria such as high order values, unfamiliar sales regions, or other potential fraud indicators. Creating a workflow to streamline compliance enhances risk management and boosts operational efficiency by prompting for further review before order processing.

For example, a new customer order exceeding a certain monetary threshold might indicate an accidental over-purchase or potential fraud. Similarly, an order from a region where you’ve never sold before requires additional scrutiny to verify the customer’s legitimacy. Additionally, transactions exhibiting behavioral patterns commonly associated with fraud can trigger alerts for your team’s assessment.

In this sample workflow pattern, we’ll trigger an email alert to a designated team member whenever an order surpasses the predefined threshold, enabling your team to address any issues with the order and earn customer trust.

![](https://stripe.dev/images/workflows-automate-compliance-safeguards/image1.png)

## Building a workflow to flag transactions

To build a workflow when a customer order exceeds a monetary threshold:

1. Add trigger: **Payment intent succeeded**.
2. Add a step: **Retrieve a customer,** for Customer ID choose **Payment intent | Customer ID**.
3. Add a condition.
4. Click the “If this condition is met” box and select **Retrieve a customer**, then select **Metadata**, then select **custom\_status**. Select "isn't equal to” and type “Approved.”
5. Click Add more.
6. Select **Payment intent succeeded**, select Amount. Select is “greater than” and type your threshold of $5,000.
7. Click Done.
8. Under the condition click Add step and Add action: Select **Email a team member**, select a team member or multiple team members from the list, type instructions in the email body field, and click Done.

## What is Stripe Workflows?

[Stripe Workflows](https://docs.stripe.com/workflows/define-workflows) provides a visual building interface in the Stripe dashboard, to help you automate tasks and processes by defining a series of actions that happen sequentially. Workflows is ideal for multi-step processes and is compatible across multiple Stripe products, allowing you to streamline processes, enforce business rules, and reduce manual effort.

To explore additional workflow patterns, see:

- [Automatically customize an object with metadata](https://stripe.dev/blog/workflows-customize-objects-metadata)
- [Creating early fraud alerts for streamlined refunds](https://stripe.dev/blog/workflows-creating-early-fraud-alerts-for-streamlined-refunds)

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
