---
title: 'Stripe for marketplaces: Mapping commercial relationships in code'
link: https://stripe.dev/blog/stripe-marketplaces-mapping-commercial-relationships-code
source: stripe-com
published: 2025-07-03T00:00:00Z
updated: 2025-07-03T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Ana Andres
summary: Learn how to use Stripe Connect to power your marketplace. Configure your fund flows, onboard merchants and manage payments in a few lines of code.
content: extracted
html: 2025-07-03-stripe-for-marketplaces-mapping-commercial-relationships-in.html
remote_preview:
  url: https://stripe.dev/og/blog/stripe-marketplaces-mapping-commercial-relationships-code.png
  alt: 'Stripe Developer Blog: Stripe for marketplaces: Mapping commercial relationships in code'
extra:
  content:interactive: true
---

A marketplace is a digital platform that hosts multiple merchants. It offers them marketing, selling, and other services and creates a bespoke buying experience to consumers.

Typically, marketplaces involve three types of users:

- **Buyers:** entities that engage in a commercial relationship with the marketplace to obtain a product or service.
- **Sellers:** entities that provide their goods to the buyers through the marketplace.
- **Marketplace platform:** the company that provides the technology and services supporting the commercial exchanges between buyers and sellers. They do not own any stock; they simply make the commercial exchanges possible.

One defining characteristic of an online marketplace is that buyers and sellers rarely engage in a direct commercial agreement. A marketplace offers terms and conditions of use of their platform to buyers and, equally, it offers other terms and conditions of use to sellers.

This post covers how to set up unlicensed marketplaces to manage payments. These marketplaces do not require money or banking license in a given jurisdiction to process payments on behalf of their sellers following regulatory compliance using [Stripe Connect](https://docs.stripe.com/connect). It also shows how to use Stripe’s APIs to control monetary commercial exchanges.

## Meet Greens & Dairy Mart

This example uses Greens & Dairy Mart, a fictitious online marketplace that allows independent farmers to sell their produce directly to consumers.

Greens & Dairy Mart allows families to choose products from multiple farms and add them to a Greens & Dairy Mart basket. Customers can use bank cards to pay for the products.

## Enabling payments for marketplaces with Stripe

Greens & Dairy Mart manages payments using Stripe Connect. Connect does three important things for this marketplace:

1. Enables payment interfaces and processing for the marketplace checkout.
2. Provides the [Know Your Customer](https://docs.stripe.com/connect/required-verification-information) (KYC) tech capabilities to onboard farmers.
3. Helps it monetize and exchange money between parties programmatically.

Thanks to the last two points, marketplaces can now automate complex commercial relationships and embed their terms and conditions in code. This allows them to grow fast without having to exponentially scale their operations.

In this marketplace, each farm is responsible for preparing and delivering the products they sell through the website, proposing their own costs and terms of delivery.

Greens & Dairy Mart charges customers when each farmer has confirmed their products are on their way to the customer. They only pay out the order to the farmer once the customer has confirmed that the order has been delivered correctly. Greens & Dairy Mart takes 15% of the final price to cover their running costs (for example, website management, customer facing support, Stripe payments, etc.)

## Steps to set up and test a marketplace in Stripe

1. Create a Stripe account for free: [https://dashboard.stripe.com/register](https://dashboard.stripe.com/register).
2. Use a [Sandbox](https://docs.stripe.com/sandboxes) to test the implementation. Payments are simulated so you can test API functionality before onboarding and activating the marketplace in Stripe.
3. Select Connect as product to start testing the onboarding of sellers (farmers):
   1. Search for **Connect > Onboarding Interface** and click **Get Started**.
   2. Follow the personalized setup and make sure that you select:
      1. **Buyers will purchase from you** - in a marketplace the marketplace is the merchant of record.
      2. **Sellers will be paid out individually** - since each farmer has their own order and delivery terms, Greens & Dairy Mart has opted to simplify the fund management.
   3. Stripe Connect's product activation screens remind you that as a marketplace:
      1. You are responsible for refunds, chargebacks and potential fines from the payment-rail networks (for example, Greens & Dairy Mart is ultimately responsible for charge disputes).
      2. You are responsible for onboarding and making sure that your farmers are compliant with payment regulations.
      3. As the main point of contact to farmers, you have to inform them and educate them on the management of risks as well as on your process for remediation for lack of compliance, when detecting fraud or if disputes are raised.

## Using Stripe’s API to map commercial relationships

This section explains how Stripe Connect supports coding the key commercial relationships that govern a marketplace.

### Service onboarding and KYC

Using the Stripe Account API marketplaces create Stripe Connected Accounts for sellers. These accounts are linked to their own Stripe account. During this process, the marketplace configures a *controller*, stating roles and responsibilities of sellers, buyers, the marketplace and Stripe, and the *capabilities* of the connected account, stating what the sellers’ accounts can do. The use of Stripe and the operational control that the marketplace has over the pay-ins and payouts for each seller should be reflected on the legal terms and conditions agreed between the two parties.

The Account API generates links to a Stripe hosted page that can be used to request KYC information. Marketplaces usually add information they already have gathered from their sellers to the API call so that the hosted flow is pre-filled. The generated links are shared with the sellers who complete the full onboarding. KYC onboarding with Stripe is fast, sellers with the right information available can start selling immediately.

This is how Greens & Dairy Mart uses the Account API:

```
curl -X POST "https://api.stripe.com/v1/accounts" \
  -u 'REPLACE_WITH_YOUR_SECRET_KEY':  \
  -d "business_type"="company" \
  -d "capabilities[transfers][requested]"="true" \
  -d "controller[fees][payer]"="application" \
  -d "controller[losses][payments]"="application" \
  -d "controller[requirement_collection]"="application" \
  -d "controller[stripe_dashboard][type]"="none" \
  -d "email"="farmer1@example.com"
```

The only capability required for connected accounts belonging to a marketplace is Transfers. This notably reduces the [KYC requirements](https://docs.stripe.com/connect/required-verification-information) for the farmers.

Greens & Dairy Mart sets `fees`, `losses`, and `requirements_collection` to “application” instead of “Stripe” to reflect the commercial relationship between the marketplace, the farmer and Stripe. Specifically, there is no commercial relationship between the farmers and Stripe and there are no direct interactions or commercial liabilities.

Because Greens & Dairy Mart wants farmers to control their sales and finance through their Greens & Dairy Mart portal, they have set the `stripe_dashboard` type to `none`. Stripe provides a Stripe-hosted [Express dashboard](https://docs.stripe.com/connect/express-dashboard) for those marketplaces that are okay with a separate payment experience.

Greens & Dairy Mart has the option to use [Stripe Embedded Onboarding](https://docs.stripe.com/connect/embedded-onboarding) to have farms do the KYC process inside their applications or use Stripe hosted onboarding links:

```
curl -X POST "https://api.stripe.com/v1/account_links" \
  -u 'REPLACE_WITH_YOUR_SECRET_KEY':  \
  -d "account"="acct_1RHRSfRdprr9w4wp" \
  -d "refresh_url"="URL on website once done" \
  -d "return_url"="URL if farmer hits back button" \
  -d "type"="account_onboarding" \
  -d "collection_options[future_requirements]"="include" \
  -d "collection_options[fields]"="eventually_due"
```

This call returns the onboarding link:

```
{
  "object": "account_link",
  "created": 1745508543,
  "expires_at": 1745508843,
  "url": "https://connect.stripe.com/setup/c/acct_1RHRhnRdSY5bmOIP/1hPK06miXaZK"
}
```

By default, Stripe only asks for the minimum amount of data required for sellers to start transacting immediately. Stripe then informs the marketplace using webhooks when more requirements are due.

However, Greens & Dairy Mart has opted to use the `collection_options` field to ask farms all KYC details at once. This is the recommended approach for marketplaces that use their own business onboarding flows. There is no need to pull additional information in the future and no risk of pausing operations because of lack of KYC compliance.

### Payments, disbursements and basic monetisation strategy

Stripe offers two mechanisms to charge customers and disburse to sellers:

- **Destination Charges:** A one-API call option which embeds the disbursement transfer information as part of the PaymentIntent API call.
- **Separate Charges and Transfers:** A two-API call process, where after charging the customer using the PaymentIntent API, the marketplace creates as many Transfers as sellers need to be paid.

With regards to payment monetization, often marketplaces charge sellers for payment processing only when customers buy products. With Stripe Connect, marketplaces can deduct this fee directly by transferring adjusted amounts to sellers or can select to transfer the full amount and charge a fee.

Finally, marketplaces deliver payments into the sellers’ bank account via [Payouts](https://docs.stripe.com/connect/payouts-connected-accounts). With Connect, payouts can be managed by Stripe automatically in a regular cadence (for example, daily or weekly) or it can be triggered by the marketplace manually.

In the example marketplace, because sales and delivery conditions are different for each farm, Greens & Dairy Mart has decided to charge the customer per farm order, Greens & Dairy Mart has implemented Destination Charges. For each farmer, they create a [PaymentIntent](https://docs.stripe.com/api/payment_intents) that specifies the amount to be transferred to each farmer, charging a 15% fee on each transaction that customers pay to cover Greens & Dairy Mart's fees.

Greens & Dairy Mart uses Stripe's [web payment element](https://docs.stripe.com/payments/payment-element) and [mobile payment element](https://docs.stripe.com/payments/elements/mobile-payment-element) to store the card details of the customer during the first checkout inside Stripe. On each checkout page, they show the items each farmer will deliver, the amount for each farmer's order and the estimated delivery time for each order.

When the customer receives the items from one of the farms, Greens & Dairy Mart creates a manual Payout to the farm's bank account.

![](https://stripe.dev/images/stripe-marketplaces-mapping-commercial-relationships-code/image1.png)

Greens & Dairy Mart uses the PaymentIntents API. During the first purchase, they set `setup_future_usage=offline` in the PaymentIntent object, so that Stripe stores the card details as a payment method attached to a Customer object (learn more about this approach [here](https://docs.stripe.com/payments/save-during-payment)). Greens & Dairy Mart keeps track of the IDs as part of their Customer Record Management integration.

In future purchases, they will charge that customer directly confirming automatically the PaymentIntent created, unless the card attached needs to be updated.

To distribute money to the correct farm, the PaymentIntent contains two additional fields: `transfer_data` and `application_fee_amount`. `Transfer_data` indicates the Stripe account of the farm where the money will be settled. `Application_fee_amount` indicates the amount the farm will pay Greens & Dairy Mart for their services.

This is the API call for the payment transaction once the card details have been saved for future use:

```
curl -X POST "https://api.stripe.com/v1/payment_intents" \
  -u 'REPLACE_WITH_YOUR_SECRET_KEY':  \
  -d "amount"=1000 \
  -d "currency"="gbp" \
  # farm account
  -d "transfer_data[destination]"="acct_1OvhPzD123PLHk8z" \
  -d "metadata[order]"="257" \
  -d "payment_method"="pm_1O5ovIDIWNRw8AogmHeuq3Jq" \
  -d "customer"="cus_OtbQdWuzAnLITm" \
  -d "confirm"="true" \
  # Greens & Dairy Mart's fee: 15%
  -d "application_fee_amount"=150  
```

Events triggered:

- At the platform level (the marketplace):
  - [payment\_intent.created](https://docs.stripe.com/api/events/types#event_types-payment_intent.created)
  - [charge.succeeded](https://docs.stripe.com/api/events/types#event_types-charge.succeeded)
  - [payment\_intent.succeeded](https://docs.stripe.com/api/events/types#event_types-payment_intent.succeeded)
  - [transfer.created](https://docs.stripe.com/api/events/types#event_types-transfer.created)
  - [charge.updated](https://docs.stripe.com/api/events/types#event_types-charge.updated)
  - [application\_fee.created](https://docs.stripe.com/api/events/types#event_types-application_fee.created)
- At connected account level (the farm):
  - [payment.created](https://docs.stripe.com/api/events/types#event_types-payment.created)

It's worth noticing that transactions are represented by PaymentIntents (pi\_xxxxx IDs) for the platform account and reflect the commercial relationships from Greens & Dairy Mart's perspective. The farm's connected account generated a payment too (py\_xxxx) which is linked to the platform Transfer transaction. This payment represents the transaction from the perspective of the farm.

This is the API call for the payout:

```
curl -X POST "https://api.stripe.com/v1/payouts" \
  -u 'REPLACE_WITH_YOUR_SECRET_KEY':  \
  -H "Stripe-Account: {{STRIPE_CONNECTED_ACCOUNT_ID}}" \
  -d "amount"=850 \
  -d "currency"="gbp" \
  -d "metadata[order]"="257" \
  -d "statement_descriptor"="Greens&DairyMart257" \
  -d "method"="instant"
```

Events triggered:

- At the connected account level (the farm):
  - [payout.created](https://docs.stripe.com/api/events/types#event_types-payout.created)
  - [payout.updated](https://docs.stripe.com/api/events/types#event_types-payout.update)
  - [payout.paid](https://docs.stripe.com/api/events/types#event_types-payout.paid)
  - [balance.available](https://docs.stripe.com/api/events/types#event_types-balance.available) (update on available balance)

The farms do not receive the money immediately. The funds reach their bank account on average 3 days after the payout has been created.

Marketplaces can [finely tune payout schedules](https://www.google.com/url?q=https://docs.stripe.com/connect/manage-payout-schedule&sa=D&source=docs&ust=1749142406225916&usg=AOvVaw0jffu182MprKln5hD6oB0W).

### Reversing payments: refunds and disputes

At payment level, the liability for refunds and disputes lies with the marketplace. This adds operational flexibility which helps maintain cordial relationships with both customers and merchants (for example, the marketplace can issue a goodwill refund without impacting the merchant). However, marketplaces can explicitly state on their terms of service in which circumstances their merchants will be sharing liability.

Connect separates the refund in two steps: a payment Refund; and a TransferReversal. Both can be partial or full amounts. If Destination Charges were used, the refund can be done in one single API call.

Disputes raised by a customer's issuer bank automatically impact the marketplace's Stripe account balance. In this instance, the marketplace could also reverse the transfer should the dispute be lost.

In our example, Greens & Dairy Mart automatically refunds the full amount if a farm has not fulfilled the delivery of the goods:

```
curl -X POST "https://api.stripe.com/v1/refunds" \
  -u 'REPLACE_WITH_YOUR_SECRET_KEY':  \
  -d "payment_intent"="pi_REPLACE_WITH_PAYMENT_INTENT_ID" \
  -d "reason"="duplicate" \
  -d "metadata[order]"="257" \
  -d "reverse_transfer"="true" \ 
  -d "refund_application_fee"="true" \
```

Events triggered at the platform level (the marketplace):

- [transfer.reversed](https://docs.stripe.com/api/events/types#event_types-transfer.reversed)
- [application\_fee.refunded](https://docs.stripe.com/api/events/types#event_types-application_fee.refunded)
- [refund.created](https://docs.stripe.com/api/events/types#event_types-refund.created)
- [charge.refunded](https://docs.stripe.com/api/events/types#event_types-charge.refunded)
- [refund.updated](https://docs.stripe.com/api/events/types#event_types-refund.updated) / charge.refund.updated

Greens & Dairy Mart recovers disputed transactions from the farms’ balance by using transfer reversals:

```
curl -X POST "https://api.stripe.com/v1/transfers/{id}/reversals" \
  -u 'REPLACE_WITH_YOUR_SECRET_KEY':  \
  -d "amount"=850 \
  -d "description"="dispute" \
  -d "metadata[order]"="257"
```

This call obtains the transfer ID (for example, {id}) of the payment transaction). Transfer groups are created and returned when Payment Intents are confirmed successfully.

```
curl -X GET "https://api.stripe.com/v1/transfers" \
  -u 'REPLACE_WITH_YOUR_SECRET_KEY':  \
  -d "transfer_group"="group_pi_3RVujPDIWNRw8Aog0SmAtcp0"
```

Events triggered at the platform level:

- [transfer.reversed](https://docs.stripe.com/api/events/types#event_types-transfer.reversed)

### Additional fees owed by the sellers

Sometimes marketplaces are required to move funds from their sellers' Stripe accounts back to their own. Some situations where this could happen include:

- To charge the connected account directly for products or services
- To recover funds for a previous refund
- To make other adjustments to connected [account balances](https://docs.stripe.com/connect/account-balances) (for example, to correct an error)

To get paid by the seller, the marketplace can use the [charges API](https://docs.stripe.com/connect/account-debits), which creates a transfer from the sellers’ connected account to the marketplace's platform account.

Greens & Dairy Mart has decided to pass a fee of 20 GBP to all sellers whose payments are disputed because of lack of quality or problems in the delivery of the products:

```
curl -X POST "https://api.stripe.com/v1/charges" \
  -u 'REPLACE_WITH_YOUR_SECRET_KEY':  \
  -d "amount"=2000 \
  -d "currency"="gbp" \
  -d "metadata[dispute_fee_reason]"="delivery_problem" \
  -d "metadata[order]"="257" \
  -d "source"="acct_YOUR_ACCOUNT_ID"
```

Events triggered:

- At the platform level (the marketplace):
  - [payment.created](https://docs.stripe.com/api/events/types#event_types-payment.created)
  - [charge.succeeded](https://docs.stripe.com/api/events/types#event_types-charge.succeeded)
  - [balance.available](https://docs.stripe.com/api/events/types#event_types-balance.available)
- At connected account level (the farm):
  - [transfer.created](https://docs.stripe.com/api/events/types#event_types-transfer.created)

## Conclusion

With Stripe, marketplaces can automate the money interactions between them, sellers and buyers. Stripe Connect provides a family of APIs to manage payments, fees, payouts, contractual and regulatory obligations. This helps marketplaces manage complex contracts programmatically.

Stripe Connect can also be used by marketplaces to develop additional services. Once Stripe Connect is adopted, marketplaces can start embedding and monetizing financial products optimizing their sellers' [money management](https://docs.stripe.com/money-management).

To learn more about developing applications with Stripe, visit our [YouTube Channel](https://www.youtube.com/stripedevelopers).
