---
title: 'Compliance versus conversion: Tax ID validation at checkout'
link: https://stripe.dev/blog/tax-id-validation-at-checkout
source: stripe-com
published: 2026-05-29T00:00:00Z
updated: 2026-05-29T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Jake Estes
summary: For product-led businesses, conversion is paramount, so an important consideration is deciding whether to implement synchronous or asynchronous tax ID validation in a checkout flow. One is friendlier for conversion, but the other can keep your business compliant.
content: extracted
html: 2026-05-29-compliance-versus-conversion-tax-id-validation-at-checkout.html
remote_preview:
  url: https://stripe.dev/og/blog/tax-id-validation-at-checkout.png
  alt: 'Stripe Developer Blog: Compliance versus conversion: Tax ID validation at checkout'
extra:
  content:interactive: true
---

For product-led businesses, conversion is paramount. Every pixel on a checkout page is carefully thought out to maximize conversion. It’s a modern game of friction vs motivation, rooted in the [Fogg Behavior Model](https://www.behaviormodel.org/): the buyer’s motivation must be greater than the friction they encounter when making a purchase for the transaction to complete.

For PLG companies with business customers, the checkout experience is different, and often requires additional information such as a business name and address to ensure downstream invoices are issued correctly.

Merchants selling to foreign customers will often collect a tax ID at checkout, in part to ensure they’re applying tax correctly on invoices to international customers.

Whether your GTM strategy is product-led (PLG), sales-led (SLG), or both, one requisite step in your customer onboarding process is collecting the buyer’s tax ID. The tax ID serves two purposes:

1. It represents a mandatory data point on [compliant tax invoices](https://stripe.com/resources/more/tax-invoices-101-a-quick-guide)
2. It gives your business a key data point for [tax determination](https://docs.stripe.com/tax/calculating)

## Tax invoices

Countries with a VAT or GST regime are typically quite prescriptive about the contents and structure of invoices. As it relates to tax data, the tax invoice typically must have line-level tax rates and amounts with descriptions of each good or service, as well as a valid tax ID of each counterparty. A valid tax invoice allows your customer to properly deduct any tax paid on purchases on their own VAT return.

Tax invoices have come under closer scrutiny with the advent of [e-invoicing](https://stripe.com/resources/more/electronic-invoicing-101-a-quick-guide-for-businesses) mandates across the globe in recent years. Now in certain countries if you issue a tax invoice that does not adhere to the local data requirements, the automated checks of the e-invoicing system will flag the invoice and reject it.

## Tax determination: applying the reverse charge

Despite the name, the reverse charge is not a payment reversal nor is it related to the process of refunding customers. In simple terms, the reverse charge is a classification of a specific type of transaction that occurs between two tax-registered businesses in two different countries (there is a domestic reverse charge in some countries, which is not addressed here).

While this sounds straightforward, the OECD provides a [56-page document](https://www.oecd.org/content/dam/oecd/en/publications/reports/2017/10/mechanisms-for-the-effective-collection-of-vat-gst-where-the-supplier-is-not-located-in-the-jurisdiction-of-taxation_4ba05a97/5269dc5a-en.pdf) describing the framework for the reverse charge mechanism and its relevance to modern businesses. While countries are free to implement their own versions of the OECD framework, generally this serves as the basis for many VAT and GST regimes.

To bring the reverse charge to life, consider the following scenario:

- Company A is an Irish SaaS business
- Company B is a VAT-registered Dutch business
- Company A sells a one-year SaaS agreement for €10,000 to Company B
- Company B provides its Dutch VAT number to the Irish seller during onboarding
- Company A validates the Dutch business’ VAT number and issues an invoice without VAT, with an invoice message stating, *No VAT - tax to be paid on a reverse charge basis*

In this scenario the Dutch buyer must ‘self-account’ for the VAT, which results in a net-zero accounting entry with no payment due to the local tax authority.

The reverse charge exists for two primary reasons:

- **Fraud**: Imagine if the Irish software company collected the 21% Dutch VAT but never paid it to the Dutch tax authorities, or if the Dutch buyer fraudulently claimed they paid VAT on the foreign-issued invoice and tried to claim it back as a refund on their VAT return. It is simpler and less risky for the buyer to self-report the liability. For a more sophisticated example of tax fraud beyond the scope of this blog post, [carousel fraud](https://taxation-customs.ec.europa.eu/taxation/vat/fight-against-vat-fraud/vat-carousel-fraud_en) is an interesting topic to explore.
- **Level Playing Field:** To avoid giving foreign suppliers a pricing advantage over local suppliers who are required to charge VAT, the internal accounting of the reverse charge levels the playing field. The buyer self-reports the VAT (say 20%) they would’ve been charged, while simultaneously declaring that amount as deductible input VAT on their tax return.

Note: we’ve been covering cross-border transactions and the reverse charge, but bear in mind tax IDs are also required for domestic transactions, where VAT is generally applied to the invoice.

With the foundational concepts now understood, let’s dive into the mechanics of tax ID validation.

## Implementation options

Collecting the customer’s tax ID is the first step; the next step is to validate the tax ID against an official government source, often a national business registry. Many tax authorities expose APIs for [direct integration](https://abr.business.gov.au/Tools/WebServices) to their registries, offering a free, convenient way to validate tax IDs for a single jurisdiction. If your customers are based in many different countries, managing multiple integrations, API updates, and the IF-THEN routing logic for each country becomes difficult to scale.

Alternatively, you can integrate with a reputable third party for global tax ID validation. Many privately-run tax ID validation services are available on the market, with different pricing models and varying degrees of quality. Functionally they’re similar, providing a single validation endpoint with routing and retries handled automatically.

The downside to this path is many of these vendors are point-solutions: your business will also need a tax engine to properly apply the tax treatment to your invoices, and a tax reporting product to consolidate tax transactions and map this data to corresponding boxes on tax returns. In this context we recommend exploring vendors who offer other tax products beyond just tax ID validation.

One important consideration for frontend developers and product managers is deciding whether to implement synchronous or asynchronous tax ID validation in their checkout flow.

- **Synchronous** validation requires the buyer to wait until their tax ID has been validated against the government database before their transaction is finalized.
- **Asynchronous** validation allows the purchase to complete regardless of the validation outcome.

## Synchronous tax ID validation

In a synchronous validation implementation, the risk of cart abandonment is higher as the buyer has to wait for the result, and they may not be willing to re-enter their tax information. The buyer and seller may also be at the mercy of how that [validation service is performing](https://viesapi.eu/vies-unavailability-of-vat-information-exchange-system/) that day: some validation services have historically been highly available (Netherlands) while others have experienced regular service disruptions (Denmark). Most of these government web services will have scheduled downtime for maintenance.

From a tax compliance perspective, synchronous validation can be positive, despite the risk of decreased conversion. This is twofold:

- Compliant tax invoices can be issued immediately, no corrections or customer contact necessary.
- Under audit, the local tax authority will expect all VAT numbers to have been validated against the official database at the point of sale. The tax ID validation databases are used as a real-time search engine, not a historical archive. Sellers cannot retroactively validate their customers’ tax IDs using these tools.

Ensuring tax IDs are validated upfront before the [tax invoice](https://stripe.com/resources/more/what-is-a-vat-invoice#what-information-should-be-included-on-a-vat-invoice) is issued reduces the downstream administrative burden of correcting tax IDs, and lowers audit risk.

## Asynchronous tax ID validation

An asynchronous implementation of tax ID validation allows the customer to complete their purchase while the validation happens on the backend.

While many tax IDs will be valid, the business will need to run a report (e.g. daily, weekly, monthly) of all of their failed customer validations and reach out to these customers to update their records. While communications can largely be automated (email copy, reminders, etc), some customers may not come back on-session to update their tax ID.

As a merchant, the question becomes how to handle customers who do not reply *at all*. While the exposure may be less for smaller, one-time purchases, it can be problematic for subscription businesses, as each subsequent invoice is not technically compliant. Under audit, most governments will make you, the seller, liable for the VAT you didn’t charge, plus penalties and interest. If you’re a seller of physical goods and your customers are in other countries, goods may be held in customs until a valid tax ID is provided, creating a poor customer experience.

When you’ve decided on an implementation approach, the next decision is selecting a validation service to integrate with. For Stripe users we recommend implementing Stripe’s tax ID validation service, as it seamlessly integrates with your Stripe [`customer`](https://docs.stripe.com/api/customers/object#customer_object-tax_ids) objects and [Stripe Tax](https://stripe.com/tax) to apply the correct tax treatment on your invoices.

## Integrating with Stripe for tax ID validation

Stripe offers both synchronous and asynchronous tax ID validation at checkout, depending on your business’ preferences and risk appetite. This is available for both `CheckoutSessions` and `PaymentIntents` integrations with Stripe Elements (private preview and public preview, respectively).

For synchronous validation, when creating a [CheckoutSession](https://docs.stripe.com/api/checkout/sessions) with the booleans `automatic_tax.enabled` and `tax_id_collection.enabled` set to `true` , the buyer can input their tax ID into the Stripe-hosted Checkout page or into Stripe’s [Tax ID Element](https://docs.stripe.com/elements/tax-id-element).

##### Synchronous validation with `CheckoutSessions`:

```
const taxIdElement = checkout.createTaxIdElement({
  // ...other options
  verification: {
    taxId: {
      mode: 'if_supported', 
    },
  },
});
```

##### Asynchronous validation with `CheckoutSessions`:

```
const taxIdElement = checkout.createTaxIdElement({
  // ...other options
  verification: {
    taxId: {
      mode: 'none',
    },
  },
});
```

##### Synchronous validation with `PaymentIntents`:

```
const taxIdElement = elements.create('taxId', {
  verification: {
    taxId: {
      mode: 'if_supported',
    },
  },
});
```

##### Asynchronous validation with `PaymentIntents`:

```
const taxIdElement = elements.create('taxId', {
  verification: {
    taxId: {
      mode: 'none',
    },
  },
});
```

When rendering the Tax ID Element, the [verification](https://docs.stripe.com/js/elements_object/create_tax_id_element#tax_id_element_create-options-verification) object should be set to `if_supported` to trigger synchronous validation. If the field is populated with `none` then asynchronous validation will be triggered. Regardless of which method is chosen, when a buyer enters their tax ID, Stripe initially performs a regex check to ensure the tax ID is formatted correctly (e.g. contains the correct number of digits for that country and contains a leading country ISO code).

After passing this check, Stripe automatically routes the tax ID request to the relevant government APIs. Validation is often complete in a second or two. In the event the government service takes more than four seconds to process the request, the transaction can proceed as if the tax ID was valid (asynchronous fallback).

Post-purchase, you can retrieve a customer’s tax ID validation status by fetching the information from the [customer](https://docs.stripe.com/api/customers/object#customer_object-tax_ids-data-verification) object; specifically look for the `tax_ids.data.verification.status` field. Stripe also emits an event called `customer.tax_id.updated`, so you can listen for a webhook which contains the verification status.

Verification status is critical because it allows your business to take action and communicate to customers who provided invalid tax IDs, ensuring your invoices and tax calculations are compliant, both mitigating risk and providing a better customer experience by avoiding misbillings.

As this engineering decision impacts cross-functional teams, we recommend it be made with input from stakeholders such as tax, finance and product. During initial deployment it is also worth considering running an A/B test to see how each version performs, and how prevalent the issue of invalid tax IDs is with your customer segment.

Ready to get started? [Enable tax ID collection and verification](https://docs.stripe.com/tax/checkout/tax-ids) today.

#### Disclaimer

The content in this article is for general information and education purposes only and should not be construed as legal or tax advice. Stripe does not warrant or guarantee the accuracy, completeness, adequacy, or currency of the information in the article. You should seek the advice of a competent attorney or accountant licensed to practice in your jurisdiction for advice on your particular situation.
