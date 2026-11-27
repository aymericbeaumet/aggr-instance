---
title: Marketplace monetization with Stripe
link: https://stripe.dev/blog/marketplace-monetization-with-stripe
source: stripe-com
published: 2025-09-04T00:00:00Z
updated: 2025-09-04T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Ana Andres
summary: Complete guide to monetizing online marketplaces with Stripe Connect. Configure transaction fees, usage-based billing, and automated invoicing with code examples.
content: extracted
html: 2025-09-04-marketplace-monetization-with-stripe.html
remote_preview:
  url: https://stripe.dev/og/blog/marketplace-monetization-with-stripe.png
  alt: 'Stripe Developer Blog: Marketplace monetization with Stripe'
extra:
  content:interactive: true
---

An online marketplace is a digital platform connecting buyers and sellers, allowing them to conduct transactions over the internet. Typically, a marketplace relies on third-party vendors to fulfil products and services but may also host a mix of third and first-party products.

To succeed, a marketplace should run on a business model that generates profits from funneling customers to sellers by managing all commercial relationships. Marketplace business models rely on monetizing the added value they provide to both customers and third-party sellers.

Also read [Stripe Connect programmatically powers marketplaces](https://stripe.dev/blog/stripe-marketplaces-mapping-commercial-relationships-code), which covers how to configure [Stripe Connect](https://docs.stripe.com/connect) to support different monetization strategies.

## **Meet Greens & Dairy Mart**

This example uses *Greens & Dairy Mart*, a fictitious online marketplace that allows independent farmers to sell their produce directly to consumers.

When using *Greens & Dairy Mart,* buyers choose products from multiple farms and add them to a *Greens & Dairy Mart* basket. Customers pay for the products on the website and receive the products directly from the farmers. Farmers receive the money once the product has been delivered.

*Greens & Dairy Mart* uses [Stripe Connect](https://docs.stripe.com/connect) and [Stripe Billing](https://docs.stripe.com/billing).

## The basics of marketplace monetization

Marketplaces match buyers with sellers; they earn money from both sides of the commercial exchange. Typically, marketplaces add commission or fees to the base price provided by the sellers and, additionally, try to monetize each of the steps that adds value to the exchange.

Example of commercial activities monetized by marketplaces:

- **Marketing:** on top of the standard platform options, marketplaces can also offer sellers the possibility to do bespoke marketing on their platform and position exclusive deals and offers.
- **Shipping/delivery/returns:** sellers usually manage order fulfilment. Sometimes, marketplaces offer shipping and delivery services to streamline the customer buying experience. Most of the time, sellers pay for the service, although it is not unusual for buyers to contribute to the cost of shipping too.
- **Payment and money management:** when facilitating payments through their platform, marketplaces get the opportunity to monetise financial operations. Typical categories of service are: pay-ins, payouts, refunds, disputes, currency exchange, tax management. Most of these services bring value to the sellers.
- **Insurance:** When the value of the goods or services proposed is high, marketplaces might also offer insurance on the goods. This service is typically offered to both buyers and sellers.

All these value-added services might be delivered directly by the marketplace or, alternatively, by a third-party provider. In this latter case, the marketplace will have to ensure payments for those services are disbursed correctly and compliantly. For instance, marketplaces do not usually hold insurance licenses, therefore, if they offer insurance, they offer a third-party product and often monetise the referral.

To monetize services, marketplaces have multiple options:

- Take its fees directly from the transactions processed through the platform: common for buyer fees and seller payment-processing fees;
- Charge its fees recurrently (e.g. at the end of the month): common for volume based services like marketing leads.
- Invoice just once when the service is requested: e.g. common when offering insurance.

Stripe helps marketplaces remain compliant with money management by staying outside the flow of funds and, at the same time, monetise:

- by enabling to programmatically add monetization fees on top of each money transaction,
- by providing a [user-based-billing API](https://docs.stripe.com/billing/subscriptions/usage-based) and a [recurring payment API](https://docs.stripe.com/subscriptions);
- by providing an [invoice API](https://docs.stripe.com/invoicing);
- and by providing other APIs that help embed value-added financial services [like loans and other on top of their regular services](https://docs.stripe.com/money-management).

## **Greens & Dairy Mart monetization strategy**

1. *Greens & Dairy Mart* charges sellers 15% flat rate to their farmers on the items they sell to cover for payment processing and operational costs.
2. They offer sellers exclusive marketing options on the platform (e.g. premium positioning on page), and charge farmers a pay-per-click fee and additional fee based on conversion.
3. Shipping is usually managed by the farmers and added at check-out. For those farmers who do not want to manage shipping logistics; *Greens & Dairy Mart* offers running deliveries on their behalf. Their fee structure is a combined additional 5% per delivery charged to sellers and a tiered 5-10 GBP flat fee to buyers, based on the weight of the order. This service is provided by Green & Dairy Mart's employees.
4. *Green & Dairy Mart* usually pays out orders after they have been fulfilled. They have started offering faster payouts, at a fee, to those farms with a good track record. And those same sellers can opt into a “damaged goods” insurance, to cover the losses from refunding goods that did not arrive in acceptable condition.

This diagram depicts the fund flows associated with the commercial exchanges:

![](https://stripe.dev/images/managing-marketplace-monetization-with-stripe/image1.png)

## How to code per-transaction fees to sellers

Stripe charges marketplaces [payment fees on each payment transaction](https://stripe.com/pricing#connect) it processes. Marketplaces can mirror this behavior and charge sellers a fee that covers this cost and other operational costs. This fee comes back to the marketplace from the connected account balance after the payment is successfully processed.

**Charging fees using the Stripe Platform pricing tool**

Stripe provides a [platform pricing tool](https://docs.stripe.com/connect/platform-pricing-tools), which helps marketplaces create rules that apply different fees based on multiple payment conditions.

When *Greens & Dairy Mart* introduced the shipping service for sellers and charged an additional 5% to those that used it, they decided to move from managing fees explicitly on the business backend to adopting the Stripe platform pricing tool. They identify payments for orders whose delivery they will fulfil using the following [metadata](https://docs.stripe.com/metadata) key:value pair:

- `delivery_by: [farm / platform]`

And configure the pricing tool as follows:

![](https://stripe.dev/images/managing-marketplace-monetization-with-stripe/image3.png)

Stripe pricing tools allow *Greens & Dairy Mart* to create [pricing schemes](https://docs.stripe.com/connect/platform-pricing-tools/pricing-schemes) to group farms that might be under better commercial terms and thus need different fees.

The Stripe pricing tool can be tested in a [Stripe sandbox](https://docs.stripe.com/connect/platform-pricing-tools/testing#test-in-a-sandbox). Before activating a rule, it’s possible to [test its impact across historical transactions](https://docs.stripe.com/connect/platform-pricing-tools/testing#platform-pricing-testing-tool).

## **Charging fees using application fees**

For those use cases where the marketplace prefers to control the fees on their own business logic, Stripe Connect allows marketplaces to add an *application\_fee\_amount* to the [PaymentIntent API call](https://docs.stripe.com/api/payment_intents/object#payment_intent_object-application_fee_amount).

## How to code invoiced fees

Marketplaces fees that are not linked 1:1 to transactions are usually invoiced separately. Stripe offers a [billing product suite](https://docs.stripe.com/billing) which includes an [Invoice API](https://docs.stripe.com/invoicing) and a [Subscriptions API](https://docs.stripe.com/subscriptions) that allow to programmatically automate the marketplace-fee requests.

Invoices and subscriptions are usually paid using funds outside Stripe Connect. However, in certain countries, Stripe offers marketplaces the option to use Stripe account balances to pay for SaaS fees.

## **Invoicing at regular intervals**

*Greens & Dairy Mart* uses Stripe usage-based billing to invoice monthly for click-to-pay marketing services and to pass on any penalties generated from payment disputes or missed deliveries.

The full process to invoice regularly for a metered service is:

1. [Create a product](https://docs.stripe.com/api/products/create)
2. [Create a billing meter](https://docs.stripe.com/api/billing/meter/create)
3. [Create the price as a combination of 1 and 2](https://docs.stripe.com/api/prices/create).
4. [Create subscription](https://docs.stripe.com/api/subscriptions/create)
5. [Generate meter events per each chargeable action](https://docs.stripe.com/api/v2/billing/meter-event/create)

They use three [billing meters](https://docs.stripe.com/billing/subscriptions/usage-based/meters/configure): pay-per-click; pay-per-conversions and penalties.

POST /v1/billing/meters:

```
curl -X POST "https://api.stripe.com/v1/billing/meters" \
  -u 'REPLACE_WITH_YOUR_SECRET_KEY':  \
  -d "display_name"="Pay per click" \
  -d "event_name"="clicks" \
  -d "default_aggregation[formula]"="sum" \
  -d "value_settings[event_payload_key]"="value" \
  -d "customer_mapping[type]"="by_id" \
  -d "customer_mapping[event_payload_key]"="stripe_customer_id"
```

All three meters will be managed by adding the values passed by the events. For instance, each time a customer clicks on a product ad, the meter event is triggered:

POST /v2/billing/meter\_events:

```
curl -X POST "https://api.stripe.com/v2/billing/meter_events" \
  -u 'REPLACE_WITH_YOUR_SECRET_KEY':  \
  -d "identifier"="idmp_12345678" \
  -d "event_name"="clicks" \
  -d "timestamp"="2025-07-31T12:00:00.000Z" \
  -d "payload[stripe_customer_id]"="cus_SrNgH2tpLj0nI3" \
  -d "payload[value]"="1"
```

With the API call above, we assign the event to the `customer_id` linked to the connected account of the farm whose ad is clicked.

The Stripe dashboard helps monitor events and analyse overall performance of the meters.

![](https://stripe.dev/images/managing-marketplace-monetization-with-stripe/image2.png)

To periodically invoice and charge for the metered services, the marketplace will create a subscription for the Stripe prices related to the meters:

```
curl -X POST "https://api.stripe.com/v1/subscriptions" \
  -u 'REPLACE_WITH_YOUR_SECRET_KEY':  \
  -d "customer"="cus_SrNgH2tpLj0nI3" \
  -d "collection_method"="charge_automatically" \  -d "items[0][price]"="price_1RvgklRf11bwQ2gfntzciS2d" \ 
  -d "items[1][price]"="price_2RvgklRf11bwQ2gluigaffea" \ 
  -d "items[2][price]"="price_3RvgklRhhefkhfksdkazcS2d" \
```

Where `customer` is the customer ID associated with the Stripe account of the farm whose meter we are using. The `collection_method` is set to `charge_automatically` because this API call assumes that at the moment we are creating the subscription we have already saved the default payment method to pay with. Each price item is a line in the subscription invoice.

For instance, the price for the pay per click service is:

```
curl -X POST "https://api.stripe.com/v1/prices" \
  -u 'REPLACE_WITH_YOUR_SECRET_KEY':  \
  -d "currency"="gbp" \
  -d "product_data[active]"="true" \
  -d "product_data[name]"="pay per click" \
  -d "product_data[statement_descriptor]"="pay per click" \
  -d "recurring[meter]"="mtr_test_61T7aNt0FSlSNmCCc41I2RyGN8ci3UsS" \
  -d "recurring[interval]"="month" \
  -d "recurring[usage_type]"="metered" \
  -d "unit_amount"=2
```

This uses the meter ID associated with this counter and a price of 0.02 GBP per click.

## **One-off invoices**

For services requested ad hoc, the Invoicing API can be used.

*Greens & Dairy Mart* offer farms insurance for damaged deliveries. If food arrives to customers in suboptimal conditions, they refund the customer, no questions asked, and not pass the cost of that refund to the farms. This insurance is invoiced once a year.

Generally the process for creating invoices goes as follows:

1. Create an empty invoice with `pending_invoice_items_behaviour` set to “include”.
2. Add the invoice items.
3. Finalize the invoice to activate the invoice and get paid.

POST /v1/invoices:

```
curl -X POST "https://api.stripe.com/v1/invoices" \
  -u 'REPLACE_WITH_YOUR_SECRET_KEY':  \
  -d "customer"="cus_SrNgH2tpLj0nI3" \
  -d "collection_method"="charge_automatically" \
  -d "description"="Damaged goods insurance" \
  -d "pending_invoice_items_behavior"="include"
```

POST /v1/invoiceitems:

```
curl -X POST "https://api.stripe.com/v1/invoiceitems" \
  -u 'REPLACE_WITH_YOUR_SECRET_KEY':  \
  -d "customer"="cus_SrNgH2tpLj0nI3" \
  -d "pricing[price]"="price_1RwIifRf11bwQ2gfORhzCrkX" \
  -d "invoice"="in_1RwIl2Rf11bwQ2gfw90wN1Bd" \
  -d "description"="Damaged goods insurance"
```

POST /v1/{insert your invoice ID}/finalize:

```
curl -X POST "https://api.stripe.com/v1/invoices/{invoice}/finalize" \
  -u 'REPLACE_WITH_YOUR_SECRET_KEY':  \
  -d "auto_advance"="true"
```

This invoice will be automatically paid when it's due provided a payment method has been saved before. Alternatively, Stripe can send an email with the invoice by setting the `collection_method` to `send_invoice`.

## Conclusion

The success of a marketplace relies on having a monetization strategy that generates profits from the activities they monetise. With Stripe Connect, marketplaces can enable the payment disbursement flows and manage their monetization programmatically. This provides a central point to monitor and report the health of their different monetization activities.

To learn more about developing applications with Stripe, visit our [YouTube Channel](https://www.youtube.com/stripedevelopers).
