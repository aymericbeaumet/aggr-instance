---
title: Your AI agent can move money in Stripe - here's how to stay in control
link: https://stripe.dev/blog/your-ai-agent-can-move-money-in-stripe
source: stripe-com
published: 2026-10-21T00:00:00Z
updated: 2026-10-21T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Daniel Wood
summary: With Stripe’s MCP server, AI agents can build your checkout page, run financial reports, and automate refunds or price adjustments. Now, agents can even send money to others, using Stripe Treasury.
content: extracted
html: 2026-10-21-your-ai-agent-can-move-money-in-stripe-here-s-how-to-stay.html
remote_preview:
  url: https://stripe.dev/og/blog/your-ai-agent-can-move-money-in-stripe.png
  alt: 'Stripe Developer Blog: Your AI agent can move money in Stripe - here''s how to stay in control'
extra:
  content:interactive: true
---

With Stripe’s MCP server, AI agents can build your checkout page, run financial reports, and automate refunds or price adjustments. Now, agents can even send money to others, using [Stripe Treasury](https://stripe.com/treasury).

Automation makes running your business easier than ever. But how do you stay in control, when an agent can make refunds or payouts for potentially hundreds of dollars?

We’ll cover the two ways to give an agent access to Stripe - acting on your behalf, or as an independent actor - and how Stripe always keeps you in control.

## Connect your agent to Stripe's MCP server on your behalf

The easiest way to get started is to [add Stripe’s MCP to your client](https://docs.stripe.com/mcp#manual) and authorize access to Stripe on behalf of your user account. You can select which accounts or sandboxes you want to grant access, and select read or write access to resources in each one. Once you’ve authenticated, your agent will determine which tools to call depending on your task.

![Adding Stripe's MCP to your client](https://stripe.dev/images/your-ai-agent-can-move-money-in-stripe/image3.png)

## Confirming sensitive actions

If your agent makes a refund or payout in live mode, Stripe requires you to confirm the action before money leaves your account. After confirmation, tell your agent to try again so the action can be completed.

![Refund daniel.smith@example.com for $65](https://stripe.dev/images/your-ai-agent-can-move-money-in-stripe/image5.png)

Human confirmation prevents an agent acting on your behalf from making mistakes, and is only required for certain write actions - even if you’ve disabled your client from asking for permission before making tool calls to Stripe.

## Giving your agent its own identity

Human confirmation keeps you in control when you're the one driving. But you need a different control when giving an agent its own identity in Stripe, independent of any one team member. For example, you may create a headless agent to monitor a customer support queue and reply to customer requests for refunds or price adjustments. OAuth doesn’t work here, because it’s tied to your login session, and every confirmation routes to you personally.

To create an agent with its own identity in Stripe, create an agent-tagged API key so it has long-lived, independent access to your Stripe account. Just like regular API keys, agent-tagged API keys support permissions, rotation and expiration, but they are subject to Approval rules, which treat the agent as an independent actor and require approval by designated reviewers for sensitive actions. To get started, [create an API key](https://docs.stripe.com/keys) from the Dashboard and select the option of "Giving an AI agent access to operate your account".

![Select the option of \"Giving an AI agent access to operate your account\"](https://stripe.dev/images/your-ai-agent-can-move-money-in-stripe/image2.png)

## Approval rules for agents acting on their own

With [Approvals](https://docs.stripe.com/account/approvals), you can define rules to conditionally require approval from designated approvers before sensitive actions. For a refund bot, that might mean refunds under $50 go through automatically, while refunds over $500 need sign-off from someone on the support team, and no single actor (human or agent) can approve their own request by default.

This is a general-purpose control. It applies to team members and agent keys alike, whether the action comes through the Dashboard or the API. Stripe also ships default rules scoped specifically to agent-tagged keys - covering refunds, payouts, and outbound transfers - so there's a baseline of oversight in place the moment you create the key. From there, you tune it: who reviews, what conditions trigger review, and whether to require approval or block the action outright.

## Create an approval rule

Setting this up takes a few minutes in the Dashboard:

1. Go to Settings > Approvals > Create rule.
2. Choose a trigger action, such as Refund is created.
3. Add a condition, such as Amount > $500.
4. \[Optional\] Set the actor condition to agent-tagged API keys (by default, rules apply to all team members and agent keys).
5. Choose designated reviewers, such as users with the Support Specialist role.
6. Set the control to Require approval, and activate.

![Creating an approval rule in the Stripe Dashboard](https://stripe.dev/images/your-ai-agent-can-move-money-in-stripe/image4.png)

When an agent performs an action that’s subject to an approval rule, the API returns an `approval_required` error instead of completing the action, and Stripe automatically submits an approval request for review. The agent can optionally provide additional justification.

```
curl -X POST https://api.stripe.com/v2/core/approval_requests/apreq_123/update \
  -H "Authorization: Bearer sk_live_..." \
  --json '{"reason": "Refund requested via support ticket #4821"}'
```

## Reviewing agent requests

Designated reviewers can approve or deny requests in the Dashboard under Settings > Approvals > [Requests](https://dashboard.stripe.com/settings/approvals/requests). Here, you can see all the details of the request, including a justification provided by the agent summarizing the customer’s request.

![Request details in the Stripe Dashboard](https://stripe.dev/images/your-ai-agent-can-move-money-in-stripe/image1.png)

When a request is approved, Stripe automatically attempts the captured action. The request then transitions to `succeeded` or `failed`. When denied, the action is not completed. Requests that haven't been approved within 14 days expire automatically.

## Closing the loop by notifying agents about approval

Your agent can listen for the outcome over webhooks, so that it knows when a request has been approved or denied, and whether the action was successfully completed. These are the supported webhook events:

```
v2.core.approval_request.created
v2.core.approval_request.approved
v2.core.approval_request.canceled
v2.core.approval_request.rejected
v2.core.approval_request.expired
v2.core.approval_request.succeeded
v2.core.approval_request.failed
```

## How Stripe keeps you in control

Together, scoped credentials, human confirmation, and configurable approval let you decide which actions an agent may perform automatically and which require review.

- Confirmation protects you automatically the moment you connect an agent via OAuth - no setup required, and it holds even if you've relaxed your own harness's safety checks.
- Two-party approval is what you design once your agent becomes its own actor - deciding exactly which actions need a second set of eyes, and whose eyes those should be.

You don't have to choose one. Most teams start with the first and layer on the second the moment automation stops being an experiment and starts being infrastructure.
