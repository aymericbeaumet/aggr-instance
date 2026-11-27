---
title: 'Workflows: Automatically customize an object with metadata'
link: https://stripe.dev/blog/workflows-customize-objects-metadata
source: stripe-com
published: 2025-07-22T00:00:00Z
updated: 2025-07-22T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Ashley Ansari
summary: Workflow patterns provide repeatable solutions to common developer problems. This pattern helps automate customize a Stripe object with metadata.
content: extracted
html: 2025-07-22-workflows-automatically-customize-an-object-with-metadata.html
remote_preview:
  url: https://stripe.dev/og/blog/workflows-customize-objects-metadata.png
  alt: 'Stripe Developer Blog: Workflows: Automatically customize an object with metadata'
---

Automatically customizing objects with metadata allows developers to attach meaningful, contextual information to payments, customers, invoices, subscriptions, or any other objects within Stripe without altering the core data model. For example, developers might tag a charge with a customer’s internal ID, a campaign name, or the order number enabling easier reconciliation, reporting, and integration with internal systems.

Use this workflow pattern to customize an object with metadata using [Stripe Workflows](https://docs.stripe.com/workflows/define-workflows). In this specific example, you will add custom data to high-value customers, customers that left a tip. This approach helps you map Stripe objects to your business logic. Metadata can trigger or guide automated actions in your backend systems, support detailed analytics, and simplify debugging. This approach persists additional context directly on Stripe objects without the need to store that data separately or modify the core payment schema.

This workflow pattern serves as the foundation for more complex workflows where you need to pass information down the workflow for subsequent actions.

![](https://stripe.dev/images/workflows-customize-objects-metadata/image1.png)

## Building a workflow to customize an object

To build a workflow to customize an object:

1. Add trigger: select **Payment intent succeeded**
2. Add action: select **Retrieve a customer**, for Customer ID choose **Payment intent | Customer ID**
3. Add action: select **Email a team member**, choose a team member from the dropdown, and type an email body. Click done.
4. Add a condition.
5. Click the “If this condition is met” box and select **Payment intent succeeded,** select **Amount details,** select **Tip,** and select **Amount.** Choose **isn’t empty**. Click Done.
6. Add action: select **Update a customer,** in the Customer ID field select **Payment intent succeeded** and choose **Customer ID.** To add custom data, in the Metadata field click Add item and in the key field type **Tier** and add value **VIP**. Click Done.

## What is Stripe Workflows?

[Stripe Workflows](https://docs.stripe.com/workflows/define-workflows) provides a visual builder in the Stripe dashboard, to help you automate tasks and processes by defining a series of actions that happen sequentially. Workflows is ideal for multi-step processes and is compatible across multiple Stripe products, allowing you to streamline processes, enforce business rules, and reduce manual effort.

Each workflow consists of a trigger and a series of steps that run in order. A step is either an action or a condition.

To learn how to [get started with Stripe Workflows](https://stripe.dev/blog/introducing-stripe-workflows).

[Metadata](https://docs.stripe.com/metadata) is an attribute on certain Stripe objects (Account, Charge, Customer, PaymentIntent, Refund, Subscription, and Transfer) that lets you store more information, structured as key-value pairs, to these objects for your own use and reference. For example, you can store your user’s unique identifier from your system on a Stripe Customer object.

Learn [how to use metadata](https://docs.stripe.com/metadata) to store additional information.

To learn more about developing applications with Stripe, visit our [YouTube Channel](https://www.youtube.com/stripedevelopers).
