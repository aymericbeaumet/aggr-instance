---
title: 'From charging a blended rate to IC++: pass network costs to your connected accounts'
link: https://stripe.dev/blog/pass-network-costs-to-your-connected-accounts
source: stripe-com
published: 2026-08-10T00:00:00Z
updated: 2026-08-10T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Safia El Bayed
summary: SaaS platforms don't fit a single mold when it comes to payments pricing. An early-stage vertical software company needs simple, flexible tools to experiment and iterate. A platform moving upmarket needs granular cost control and the ability to offer sophisticated pricing to enterprise customers. And as AI features reshape how platforms monetize, that diversity is only going to increase.
content: extracted
html: 2026-08-10-from-charging-a-blended-rate-to-ic-pass-network-costs-to.html
remote_preview:
  url: https://stripe.dev/og/blog/pass-network-costs-to-your-connected-accounts.png
  alt: 'Stripe Developer Blog: From charging a blended rate to IC++: pass network costs to your connected accounts'
extra:
  content:interactive: true
---

SaaS platforms don't fit a single mold when it comes to payments pricing. An early-stage vertical software company needs simple, flexible tools to experiment and iterate. A platform moving upmarket needs granular cost control and the ability to offer sophisticated pricing to enterprise customers. And as AI features reshape how platforms monetize, that diversity is only going to increase.

Connect has made significant strides on both ends of that spectrum. For platforms that want flexibility without complexity, segmented pricing lets you set and manage rates directly from the Dashboard with the ability to differentiate by payment method, geography, and other attributes. For platforms moving upmarket, Interchange Plus Plus (IC++) takes things further: it gives you the ability to offer cost-plus pricing to your users, passing network costs transparently while protecting your margins, without the operational burden of managing rate tables, navigating messy reconciliation, or tracking constant interchange updates from the networks.

That last capability is proving to be a meaningful unlock. In the article below, we dig into how network cost passthrough actually works: what it means for your pricing model, how to set it up and how it shows up in your users' fee reporting.

## Why blended pricing hurts platform margins

On a blended rate, the platform absorbs all interchange variance. When your card mix is predictable, this isn’t a problem. But premium rewards cards, corporate purchasing cards, and high-interchange consumer cards can cost materially more in network fees than your blended rate assumes. The difference comes straight out of your spread, and as platforms scale and attract larger, more sophisticated merchants, that card mix tends to shift in the higher cost direction.

Network cost passthrough solves this by moving the platform to IC++: interchange and card scheme fees flow directly to connected accounts at cost, and the platform earns a clean, protected margin on top. Regardless of card mix, the economics become predictable. The operational headache of tracking interchange schedules, managing rate table updates, or reconciling cost variances disappears. And for connected accounts, the model is actually more transparent: they receive reporting on the network costs attributed to their payment, not a blended approximation of it.

That combination: margin protection for the platform and cost transparency for users, is why IC++ is increasingly the default choice for platforms moving upmarket.

## Setting the stage: IC+ vs IC++

IC+ and IC++ are related but distinct offerings for Connect platforms:

- IC+ (Interchange Plus): A pricing model where the platform is charged the underlying network costs (interchange + scheme fees) plus a Stripe markup, instead of a single blended rate.
- IC++ (also referred to as network cost passthrough for platforms): A Connect capability built on top of IC+ that lets a platform pass its network costs through to its connected accounts. In this case, the platform can offer IC+pricing to its own connected accounts rather than charging them a blended rate.

## How network cost passthrough (IC++) works

### Eligibility requirements

For your platform to be eligible for network cost passthrough, the following must be true:

- Your platform must be on IC+ pricing
- Your platform must control pricing (IC++ is only relevant if the platform is on a buy-rate, not on revenue share)
- The connected account must have the transfers capability enabled
- The connected account should be the Merchant of Record of the transaction: in practice, this means you’re either using Direct Charges or Destination Charges with the `on_behalf_of` parameter fund flows
- The platform and connected account pair must be in eligible geographies (you can work with your account team to ensure geographical availability)

### Enable network cost passthrough for your connected accounts

You can enable Network Cost Passthrough for a connected account either on the Dashboard or through the Stripe API.

In the Dashboard, navigate to the connected account's detail page. In the monetization section, toggle on "Pass through network costs" in the Network costs tab.

Through the API, create a Pricing Config Scheme of type `network_costs` for the connected account using the Stripe-Account header:

```
curl POST https://api.stripe.com/v1/pricing_configs/network_costs/schemes \
  -u "sk_live_xxxx:" \
  -H "Stripe-Version: 2026-06-24.preview" \
  -H "Stripe-Account: CONNECTED_ACCOUNT_ID" \
  -d enabled=true
```

To check the current status for a connected account:

```
curl https://api.stripe.com/v1/pricing_configs/network_costs/schemes/current_at \
  -u "sk_live_xxxx:" \
  -H "Stripe-Version: 2026-06-24.preview" \
  -H "Stripe-Account: CONNECTED_ACCOUNT_ID"
```

Once enabled, IC++ passes eligible card-network costs, including interchange, scheme fees, and applicable adjustments, through to the connected account at cost. It does not pass through separate Stripe product fees.

In practice, Stripe first charges network costs to your platform account on your IC+ billing schedule (typically within two days of the payment occuring). Within a few hours, we recover these costs from the connected account’s balance and add them to the platform’s balance via a Balance Transaction of type `platform_fee_transfer`. Note that if the connected account has paid out its funds, issued refunds, or otherwise reduced its balance before the recovery occurs, the network cost recovery can take the account negative. In this case, payouts are paused until the connected account’s balance goes positive again.

### Adjust the pricing for your connected accounts

To accommodate this change, your platform’s existing blended application fee, which was priced to absorb network costs, needs to come down.

If you use Stripe's [Platform Pricing Tool](https://docs.corp.stripe.com/connect/platform-pricing-tools) (no-code pricing management), the recommended approach is to create a dedicated pricing group for IC++ accounts rather than modifying your existing rules. Add a condition of "Network cost passthrough = Enabled" to scope the rule correctly, then set a lower volume fee that reflects your Stripe fees and your markup, without the network costs.

Alternatively, if you manage pricing programmatically, you can update your own internal pricing engine to accommodate for this change.

![Blended and passthrough pricing diagrams](https://stripe.dev/images/pass-network-costs-to-your-connected-accounts/image1.png)

## Reporting and reconciliation for your merchants

As you change your pricing strategy for your connected accounts, you must accompany this change by providing them with new reporting capabilities. You can help your connected accounts understand their network costs by sharing the following reports with them:

- IC+ plan-level report: This report shows data aggregated at the calendar month level. This helps connected accounts understand the monthly network costs computed across all their charges. It provides interchange plan names and an aggregated scheme fee total
- IC+ transaction-level report: This report helps connected accounts understand transaction costs at an individual-transaction level. It provides information about which charges led to higher network costs and how refunds and disputes impact network costs. This report shows data aggregated by calendar month.

Both reports provide complementary information. You can access the reports for a connected account in several ways:

- Stripe Dashboard (for connected accounts with Dashboard access): this route requires no technical lift at all from you as a platform.
- Network cost passthrough embedded component: you can use Connect embedded components to add connected account dashboard functionality to your website. These libraries allow you to grant your users access to Stripe products directly in your dashboard. This route requires very little technical lift from you as a platform.
- [Reporting API](https://docs.stripe.com/api/reporting/report_run): our Reporting API lets you programmatically access the same financial data that's in your Dashboard’s financial reports. This route requires some technical lift from you as a platform.
- Sigma or Stripe Data Pipeline (for platforms with access to these two products):  to run custom analyses and integrate network cost data into your existing workflows, you can access this report data in Sigma and Stripe Data Pipeline. Similarly, this route requires some technical lift from you as a platform.

Regardless of how you access the report, the network cost schema is the same and is described [here in our documentation](https://docs.stripe.com/connect/network-cost-passthrough-platforms?core-dashboard-or-api=dashboard#network-cost-report-schema).

## How to deploy this new pricing strategy

### Start with a pilot cohort

Enabling IC++ is a pricing change with real financial implications for your connected accounts. We recommend starting with a pilot cohort of a few connected accounts that are good candidates:

- High-volume, payment-aware accounts: merchants who already think carefully about their processing costs and will understand the change quickly
- Commercially engaged: accounts you have a close relationship with and can get feedback from

### Communicate before you enable

Connected accounts need context before they see new line items on their statements. Before enabling this change for a cohort, send a short explanation covering: what is changing, why (transparency and cost accuracy), what their new fee structure will look like, and where the connected accounts can find the IC++ reports.

### Embed IC++ into your go-to-market motion

Once the pilot validates your rollout approach, IC++ becomes a commercial differentiator in addition to being an operational change. For prospecting, IC++ is a key value proposition with enterprise and payment-aware buyers who have outgrown blended pricing elsewhere. Position IC++ as a premium tier with greater transparency for sophisticated operators.

## Get started

Network cost passthrough is available for eligible Connect platforms today. If you're ready to move your connected accounts to IC++, the [full documentation](https://docs.stripe.com/connect/network-cost-passthrough-platforms) covers everything from enabling passthrough per account to reading the network cost reports.

If you're not yet enabled for IC++ at the platform level, or you want to talk through whether this pricing model is the right fit for your business, reach out to your Stripe account team. They can confirm your eligibility and help you build a rollout plan.
