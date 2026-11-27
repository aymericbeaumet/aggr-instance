---
title: 'Agentic payments for marketplaces: How a smart fridge can shop safely'
link: https://stripe.dev/blog/agentic-payments-for-marketplaces
source: stripe-com
published: 2026-08-07T00:00:00Z
updated: 2026-08-07T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Ana Andres
summary: While a smart fridge can detect when a household is running out of milk, the more interesting engineering problem begins after that.
content: extracted
html: 2026-08-07-agentic-payments-for-marketplaces-how-a-smart-fridge-can.html
remote_preview:
  url: https://stripe.dev/og/blog/agentic-payments-for-marketplaces.png
  alt: 'Stripe Developer Blog: Agentic payments for marketplaces: How a smart fridge can shop safely'
extra:
  content:interactive: true
---

How does the fridge turn a sensor signal into a trusted grocery order? How does it find an available product, choose a delivery window, and pay on the household’s behalf, without receiving unrestricted access to the household’s card or wallet?

For the fictional grocery marketplace GreenMart, the first architecture choice is not about payment. It is about how GreenMart becomes discoverable to agents. GreenMart could build and operate its own ACP product feed, discovery document, and checkout implementation. Instead, it uses the [Stripe Agentic Commerce Suite](https://stripe.com/blog/agentic-commerce-suite) to upload or connect its catalog and expose a dedicated, hosted ACP endpoint.

That choice lets GreenMart share current product, price, and availability data with AI agents without building and maintaining a protocol-specific public surface itself.

The solution includes:

- Stripe [Agentic Commerce Suite](https://docs.stripe.com/agentic-commerce/for-sellers) to turn GreenMart’s existing catalog and commerce data into a hosted ACP storefront. Stripe can also expose UDP storefront and eventually manage any changes to protocols.
- [Agentic Commerce Protocol](https://docs.stripe.com/agentic-commerce/acp) (ACP) for the fridge buying agent and the companion mobile app to interact with that storefront.
- Household authorization policies to define what the shopping agent may buy.
- A household payment authorization experience—for example, a fridge companion app—that decides when the agent may use household payment authority.
- [Link for AI wallet](https://link.com)  for agents as an optional way to give the shopping agent bounded payment capability without exposing a card to the fridge or marketplace.
- [Shared Payment Tokens](https://docs.stripe.com/agentic-commerce/concepts/shared-payment-tokens) (SPTs) as the underlying Stripe payment primitive for direct Stripe integrations; Link can handle those mechanics for the agent.

We follow this core principle: an agent should be able to make approved decisions autonomously, but it should not receive more authority than the buyer intended.

We assume the fridge’s sensors and inventory-detection model already exist. This post focuses on the commerce flow after the fridge detects that something needs to be bought.

## Why GreenMart uses the Suite instead of building ACP itself

A marketplace’s product information is usually scattered across catalog, inventory, pricing, delivery, and order-management systems. Implementing ACP directly means turning those systems into a protocol-facing product feed and checkout API, keeping that interface available, and maintaining compatibility as the protocol evolves.

GreenMart already needs to own the source data and business decisions: what is in stock, what substitutions are acceptable, which delivery windows are possible, and whether an order can be fulfilled. It does not need to own the agent-facing protocol surface as well.

With the Stripe Agentic Commerce Suite, GreenMart provides its catalog and relevant commerce information to Stripe. The Suite exposes the hosted ACP endpoint that agents call. GreenMart retains its role as the merchant and source of truth for inventory, pricing, fulfillment, and order operations.

The benefit is practical:

- GreenMart can make its catalog available to agentic surfaces without separately implementing discovery, feed hosting, and protocol routing.
- The fridge agent can use a standard ACP interaction rather than a GreenMart-specific integration.
- The companion mobile app can display the same authoritative checkout state that the agent received.
- GreenMart can concentrate engineering effort on catalog quality, inventory freshness, and fulfillment—not protocol infrastructure.

The hosted endpoint is an abstraction boundary, not a replacement for GreenMart’s commerce system. GreenMart must still keep the underlying information accurate and re-check price and availability before an order is accepted.

Every Friday, the fridge checks the household’s inventory and prepares a grocery order for Saturday-morning delivery. The household has configured preferred brands, dietary requirements, acceptable substitutions, delivery windows, and a weekly spending limit.

The fridge also supports an urgent mode. If milk or eggs run out, the agent can request same-day delivery—but only for approved items, within a lower budget, and without automatically accepting substitutions.

The roles stay deliberately separate:

- The fridge identifies what is missing.
- The shopping agent builds a basket and communicates with GreenMart through ACP.
- Stripe Agentic Commerce Suite hosts GreenMart’s ACP-facing catalog and checkout surface.
- GreenMart provides the source information and performs its normal inventory, fulfillment, and order operations.
- Household policy decides whether the agent may request payment.
- The household payment authorization experience decides whether payment authority is issued.

## One hosted marketplace surface, two ACP clients

GreenMart sends catalog, price, inventory, and delivery information from its commerce systems to Stripe Agentic Commerce Suite. The Suite exposes that information through a hosted ACP endpoint.

The fridge buying agent is one ACP client. It uses the endpoint to discover products and create a checkout. The companion mobile app is the household-facing ACP client: it uses the final checkout state to show what the agent wants to buy, apply household policy, and initiate the supported payment-authorization flow.

## ![A diagram of ACP data flow](https://stripe.dev/images/agentic-payments-for-marketplaces/image2.png)

This is why the hosted endpoint is useful: GreenMart avoids a bespoke protocol implementation, while the agent and app still have a consistent way to exchange discovery and checkout information.

## The end-to-end flow

Here is how the scheduled or urgent grocery order moves through the system.

![A diagram of the entire flow](https://stripe.dev/images/agentic-payments-for-marketplaces/image1.png)

The agent never needs to understand GreenMart’s internal catalog schema. It sees the information through ACP. GreenMart never needs to build a custom API for this agent. It supplies information to the Suite and receives the resulting order in its normal fulfillment flow.

## The household policy is core to the companion app

Household policy is a concrete configuration owned by the people who control the bridge between the household’s agent and payment authority. The companion app should give them a way to define who may buy, what they may buy, and when the agent must stop and ask.

A policy setup flow can combine:

- Guided controls: approved agents, merchants or categories, item exclusions, per-order and weekly limits, delivery locations and time windows, substitution rules, and approval thresholds.
- Policy templates: starting points for common arrangements, such as routine household replenishment, emergency purchases, or a child’s limited spending allowance.
- Natural-language authoring: as an optional convenience, an LLM can turn a request such as “restock essentials up to €80 per week, but always ask before changing brands” into proposed structured rules. The household should review and confirm the resulting rules before they are enabled.
- Lifecycle controls: named policy owners, policy versions, an audit trail, expiration dates, and a quick way to pause or revoke the agent’s authority.

Evaluate the final checkout, not an estimate

The household policy is not a payment credential. It determines whether the agent may request payment for this specific final checkout. If the amount, item, seller, or delivery terms violate policy, the agent can choose another option or ask the household to approve an exception.

That evaluation uses a deterministic policy engine against the merchant, items, quantities, total, currency, selected fulfillment option, delivery address or window, substitutions, and any applicable budget already spent.

The result should be explicit: approved, requires household approval, or denied.

An approved result allows the app to request bounded payment authority through Link or another supported payment path. A checkout that exceeds a limit or introduces an unapproved substitution can be revised by the agent, escalated to a household member, or canceled. This keeps the policy understandable and auditable: an LLM may help people write it, but it should not be the component that decides whether a particular payment is allowed.

Below is an example of authoritative results after the shopping agent creates or updates the checkout through GreenMart’s Stripe-hosted ACP endpoint.

```
{
  "id": "chk_greenmart_123",
  "status": "ready_for_payment",
  "currency": "usd",
  "line_items": [
    {
      "id": "li_milk_organic_2l",
      "item": {
        "id": "gm_milk_organic_2l",
        "name": "Organic 2% milk",
        "unit_amount": 449
      },
      "name": "Organic 2% milk",
      "unit_amount": 449,
      "quantity": 2,
      "availability_status": "in_stock",
      "totals": [
        {
          "type": "subtotal",
          "display_text": "Organic 2% milk",
          "amount": 898
        }
      ]
    },
    {
      "id": "li_eggs_free_range_12",
      "item": {
        "id": "gm_eggs_free_range_12",
        "name": "Free-range eggs, 12 pack",
        "unit_amount": 487
      },
      "name": "Free-range eggs, 12 pack",
      "unit_amount": 487,
      "quantity": 2,
      "availability_status": "in_stock",
      "totals": [
        {
          "type": "subtotal",
          "display_text": "Free-range eggs, 12 pack",
          "amount": 974
        }
      ]
    }
  ],
  "fulfillment_options": [
    {
      "id": "delivery_sat_0900_1100",
      "type": "shipping",
      "title": "Saturday delivery, 09:00–11:00",
      "selected": true,
      "totals": [
        {
          "type": "shipping",
          "display_text": "Delivery fee",
          "amount": 499
        }
      ]
    },
    {
      "id": "delivery_sat_1100_1300",
      "type": "shipping",
      "title": "Saturday delivery, 11:00–13:00",
      "selected": false,
      "totals": [
        {
          "type": "shipping",
          "display_text": "Delivery fee",
          "amount": 299
        }
      ]
    }
  ],
  "totals": [
    {
      "type": "subtotal",
      "display_text": "Subtotal",
      "amount": 1872
    },
    {
      "type": "shipping",
      "display_text": "Delivery fee",
      "amount": 499
    },
    {
      "type": "tax",
      "display_text": "Tax",
      "amount": 13
    },
    {
      "type": "total",
      "display_text": "Total",
      "amount": 2384
    }
  ],
  "messages": []
}
```

Which becomes the following policy-evaluation view derived by the fridge:

```
{
  "checkout_id": "chk_greenmart_123",
  "seller": "GreenMart",
  "total": 2384,
  "currency": "usd",
  "selected_delivery": "Saturday delivery, 09:00–11:00",
  "items": [
    {
      "product_id": "gm_milk_organic_2l",
      "quantity": 2,
      "amount": 898
    },
    {
      "product_id": "gm_eggs_free_range_12",
      "quantity": 2,
      "amount": 974
    }
  ]
}
```

`id`, `status`, `line_items`, `fulfillment_options`, and `totals` are ACP checkout concepts. The actual names and optional fields should follow the current [ACP checkout schema](https://github.com/agentic-commerce-protocol/agentic-commerce-protocol/blob/main/spec/2026-04-17/json-schema/schema.agentic_checkout.json).

## Where payment authority lives

The app does not store card details. It uses the final ACP checkout as the purchase context, applies the household’s policy, and can either ask for an explicit approval or let the household grant the shopping agent bounded access to its Link wallet for agents.

That gives the full system two controls:

1. Household policy decides whether the agent may request payment for this particular checkout.
2. The household chooses how payment authority is issued: explicit approval for a purchase, or a pre-authorized Link capability with guardrails.

Link is an optional payment-authority layer, not a prerequisite for ACP or the Agentic Commerce Suite. An implementation can still use a different supported wallet or a direct SPT integration when that is the right product choice.

## Optional: let Link handle the payment credential

If the household gives the shopping agent access to its Link wallet for agents, the agent does not need to handle card data—or, for a Stripe-processing marketplace, the details of an SPT—at all. It still needs the final checkout and household policy decision before it asks Link to pay.

### GreenMart processes payments on Stripe

For a marketplace like GreenMart that processes payments on Stripe, Link can provide the agent’s payment capability and handle the Stripe-specific credential handoff. Under the hood, the Stripe payment flow uses a Shared Payment Token and a `PaymentIntent` to process the transaction. GreenMart receives the payment result, not the household’s card details.

That means the buyer-facing flow can stay simple:

1. The agent creates an ACP checkout through GreenMart’s Stripe-hosted endpoint.
2. The companion app evaluates the final checkout against household policy.
3. Link authorizes the agent to pay GreenMart within the household’s limits.
4. The hosted checkout completes and GreenMart receives the confirmed order.

The agent and companion app do not need to create, transport, or interpret an spt\_… identifier in this path. SPTs remain the Stripe plumbing behind the authorization. For the lower-level model, a merchant creates a `PaymentIntent` using the granted SPT; see [Shared Payment Tokens](https://docs.stripe.com/agentic-commerce/concepts/shared-payment-tokens).

### GreenMart does not process payments on Stripe

Link can also support an agent purchasing from a merchant that is not processing payments on Stripe by issuing a virtual card for the transaction. The virtual card can carry the same kind of bounded authority the household intended—for example, a capped amount and a short expiry window of a few hours—rather than exposing the household’s reusable card credentials.

This gives the agent a payment credential that a non-Stripe merchant can accept through its existing card-processing stack. Availability and the exact guardrails depend on the Link and Issuing-for-agents capabilities enabled for the household and merchant, so production teams should validate the current product and integration requirements before relying on this path.

## Use direct SPTs only when you need the lower-level integration

A direct SPT flow remains useful when the agent, wallet, or marketplace needs to control the token exchange explicitly. In that model, after policy approval a supported wallet returns a GreenMart-specific SPT; the marketplace then uses it to create a `PaymentIntent` on Stripe.

Whether Link abstracts the token or the integration passes an SPT directly, the security boundary is the same:

- The agent receives bounded payment authority, not a card number.
- GreenMart receives a confirmed order through its commerce integration.
- Retries must be idempotent, and payment/order events must be reconciled through verified webhooks.
- Revoked or expired payment authority must prevent order completion.

## Design for the moments when the agent should stop

Suppose GreenMart’s preferred milk is unavailable. The marketplace offers a larger replacement, but the replacement raises an urgent order from $23.84 to $26.10.

The agent should not silently accept the change if it exceeds the household’s policy. It can find another eligible product, remove the unavailable item, select a lower-cost delivery window, or ask the household to approve the higher total.

That is what bounded autonomy looks like: the agent can take routine action quickly, and it can recognize when the decision belongs to a person.

## Where the Model Context Protocol fits

This architecture does not require the Model Context Protocol ([MCP](https://modelcontextprotocol.io/docs/getting-started/intro))  on the purchase path. Stripe Agentic Commerce Suite gives GreenMart a hosted ACP endpoint, and the fridge agent and companion app use ACP to work with the marketplace.

MCP can still be useful for optional capabilities in MCP-enabled hosts: meal planning, loyalty lookup, recipe-based baskets, delivery recommendations, customer support, or fridge-side tools that interpret sensor data. Those extensions remain separate from GreenMart’s hosted commerce surface.

## Reference implementation

The companion sample implements mocked fridge inventory signals, household policy evaluation, both an ACP- and ACS- based checkout, a household payment-authorization step, SPT-based payment, and webhook-safe order confirmation. Its developer console shows the full flow, including the distinction between a policy decision and payment authorization.

[Explore the Smart Fridge Agentic Commerce reference implementation](https://github.com/aandresdelvalle-stripe/smart-fridge-agentic-commerce) on GitHub.
