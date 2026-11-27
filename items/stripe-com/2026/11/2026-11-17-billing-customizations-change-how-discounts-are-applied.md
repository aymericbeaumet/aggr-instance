---
title: 'Billing customizations: change how discounts are applied'
link: https://stripe.dev/blog/billing-customizations-change-how-discounts-are-applied
source: stripe-com
published: 2026-11-17T00:00:00Z
updated: 2026-11-17T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Anjali Gola
summary: Standard Stripe coupons apply a fixed amount or percent off a product. When your discounting strategy is more specific - price-targeted offers, BOGO, interval-aware discounts, or caps that follow your prices - you can customize discount calculation in Billing. Start with dashboard-authored coupons, or write a TypeScript script that runs during the discount calculation extension point.
content: extracted
html: 2026-11-17-billing-customizations-change-how-discounts-are-applied.html
remote_preview:
  url: https://stripe.dev/og/blog/billing-customizations-change-how-discounts-are-applied.png
  alt: 'Stripe Developer Blog: Billing customizations: change how discounts are applied'
extra:
  content:interactive: true
---

Developers using Stripe implement advanced marketing strategies that don't always fit the standard amount-off or percent-off coupons supported by Stripe Billing. Previously, this meant managing multiple coupons on your own or restructuring your Stripe product catalog so that you could apply coupons to selected products. Now, Stripe provides new ways to [customize discount calculation](https://docs.stripe.com/billing/scripts/discount-calculation) so that you can implement new discounting strategies with minimal work and maintenance commitment.

The following discount customizations are available with just a [few clicks in the dashboard](https://docs.stripe.com/billing/scripts/stripe-authored/discount-calculation#create-a-custom-coupon), without writing any code:

- Fixed percent off up to a maximum
- Price-targeted amount off
- Price-targeted percent off
- Buy one, get one (BOGO)

You can customize these even further by writing your own script for the discount calculation extension point. Extension points are designated places in Stripe where you can run custom logic. The discount calculation extension point lets you replace Stripe's discount calculation logic with your custom logic. Writing this logic as a script extension, a TypeScript script that runs in Stripe's runtime, makes it readable, testable, and easy to maintain.

Here are some scenarios where this customization can be useful to you:

#### Discount by price instead of product

Standard coupons target products. If your product has multiple prices (e.g. monthly and annual plans) or has variations (e.g. starter or pro tiers), a standard coupon will apply to all of them. Previously, to discount only for one targeted price, you would need to restructure your product catalog and create different coupons per product. Now, a discount calculation script can apply discount based on price. Depending on what you want to match on, there are two approaches:

##### 1\. Use metadata on the price object

You can add metadata to price objects to identify pricing categories in your business. For example, add the following key-value pair to prices for enterprise plans:

```
"metadata": { "plan_type": "enterprise" }
```

Then, create a coupon with the type set to “Custom” and select “Price-targeted amount off” to give a $100 discount on each matching enterprise plan item on the invoice.

![Discount calculator configured for a $100 price-targeted amount off when price metadata plan_type is enterprise](https://stripe.dev/images/billing-customizations-change-how-discounts-are-applied/image1.png)

You can apply discount to any information you put in price metadata such as:

- Apply discount to certain regions such as Asia-Pacific only for regional or seasonal marketing campaigns
- Apply discount based on eligibility metadata such as partners-only
- Apply discount to legacy or old pricing plans only

##### 2\. Use native properties on the price object

The [price object](https://docs.stripe.com/api/prices/object) is available to the script for discount calculation. It already includes information which can be used to target discounts selectively:

- `price.recurring.interval` to discount only annual or monthly plans
- `price.recurring.usage_type` to discount only metered usage and not discount licensed seats
- `price.type` to discount only one-time prices or only recurring prices
- `price.unit_amount` to compute caps or thresholds based on the per-unit cost
- `price.billing_scheme` to avoid double discounts to customers who already get better price through tiered pricing

The following script calculates a discount from the subtotal of annual line items, excluding monthly line items, and caps it at the request's gross amount.

```
  export default class AnnualOnlyDiscount
      implements Commerce.DiscountCalculation<AnnualOnlyConfig>
    { 
      computeDiscounts(request, config, _context) {
        const percent = Decimal.from(config.discountPercent)
          .div(Decimal.from('100'), 10, 'half-up');
        // Sum subtotals of annual line items only
        let annualSubtotal = Decimal.zero;
        for (const li of request.lineItems) {
          if (li.priceKind === 'price' && li.price.recurring?.interval === 'year') {
            annualSubtotal = annualSubtotal.add(li.subtotal.amount);
          } 
        } 
        const discount = annualSubtotal.mul(percent);
        const capped = discount.gt(request.grossAmount.amount)
          ? request.grossAmount.amount
          : discount;
        return {
          discount: {
            amount: {amount: capped, currency: request.grossAmount.currency},
          },
        };
      }
    }
```

Scripts are made available to a Stripe account by bundling them in a [Stripe App](https://docs.stripe.com/stripe-apps). Inside the app, you define an [extension](https://docs.stripe.com/extensions/how-extensions-work) that targets a specific extension point like discount calculation. The extension contains your TypeScript script. By default, when you upload the app to your account, it is available only on your account. To share it with others, use app [distribution options](https://docs.stripe.com/stripe-apps/distribution-options). When the app with the custom discount script code is installed into an account, the Script starts showing as an option in the list of Custom types in the coupon creation page.

#### Get different behaviors from the same coupon

Instead of selectively applying or not applying the coupon to different products or prices, the same coupon can behave differently for different products or prices. For example, if you are running a campaign that offers $50 off a subscription which is priced at $25/month or $300/year, you might want to give the annual subscribers the full $50 off but split the discount across two months for monthly subscribers. Previously, you would need two different coupons - one for the annual plan and one for the monthly plan. Now, in a script you can code what you want to happen and use the same coupon.

```
export default class IntervalAwareDiscount
      implements Commerce.DiscountCalculation<IntervalAwareConfig>
    { 
      computeDiscounts(request, config, _context) {
        const totalValue = config.totalDiscountValue?.amount ?? Decimal.zero;
        const months = Decimal.from(config.durationInMonths);
        const isMonthly = request.lineItems.some(
          (li) => li.priceKind === 'price' && li.price.recurring?.interval === 'month'
        );
        // Annual: full $50 off one invoice
        // Monthly: $50 / 2 = $25 off per invoice (coupon duration_in_months handles repeat)
        const discount = isMonthly
          ? totalValue.div(months, 2, 'half-up')
          : totalValue;
        const capped = discount.gt(request.grossAmount.amount)
          ? request.grossAmount.amount
          : discount;
        return {
          discount: {
            amount: {amount: capped, currency: request.grossAmount.currency},
          },
        };
      }
    }
```

In this example, the two fields `totalDiscountValue` and `durationInMonths` are part of the configuration and are rendered in the Stripe Dashboard for the coupon-creating user to provide values. Using the SDK's `MonetaryAmount` type for the discount value ensures the amount is in the correct currency units regardless of currency type.

This pattern can be used to apply different strategies on the same coupon:

- Time-based distribution of discounts such as spreading out onboarding discounts across weeks to give customers more time to explore your product
- Tiered pricing incentives where you give a flat discount for lower tiers and percentage discount for higher tiers using `price.unit_amount` to define thresholds.
- Commitment-length discounts where 6-month plans get lower rates than 12-month commitments
- Product bundling so that you only discount when both products are present. Standard coupons can apply to two products but not conditionally based on only when those two products are present.
- Add-on bundling so that a coupon discounts an add-on only when a base product is on the invoice.

#### Dynamic caps and leaving room for growth

A percentage discount with a fixed amount cap can be implemented using standard coupons but requires you to hardcode the amount cap. If you are an AI SaaS company selling a base model at $150 with incremental seats at $20/month, you want to be prepared for unpredictable price changes. You can offer a discount of 20% off seat costs, capped at 5 free seats, where the capped amount works out to $100 today. However, if your seat price changes to $25 with your next specialized model, you would need to go and update the coupon details. Avoid this by using a script that looks at the seat's `price.unit_amount` at the time of the coupon being *used* by your customers to compute the cap. The cap adjusts automatically when the price changes and the same coupon works across multiple prices.

![Low-volume 25-seat invoice comparing a standard $100-capped coupon with a script whose cap follows unit price](https://stripe.dev/images/billing-customizations-change-how-discounts-are-applied/image2.jpg)\
 You can also use different properties to calculate the discount value and the cap value. You could cap by the base subscription cost instead of the seat cost.

As you grow in scale, you offer volume based discounts because your infrastructure or support costs grow sub-linearly with seats: serving 70 seats doesn't cost 7x what serving 10 seats costs and your margins per seat improve with volume. This can be an opportunity to expand revenue, encourage product entrenchment and higher volume purchases. Tiered discounts can also result in more savings for your business compared to flat-threshold discounts; e.g. offering 20% off on the next 40 seats is better than 20% off up to 70 seats. You can use scripts to offer this tiered pricing as shown in the image below.

![High-volume 70-seat invoice comparing a flat $100 cap with a script that applies tiered discounts](https://stripe.dev/images/billing-customizations-change-how-discounts-are-applied/image3.jpg)

Lastly, even if you use a complicated discount strategy, its thresholds can be configured through the Stripe Dashboard without needing to change the custom coupon code. Use the scripts config form to provide input values to the script at the time of coupon creation. This means that even those who don't know TypeScript or use the Stripe CLI can have control over how a custom coupon is created.

Here is the example code to implement the previous example:

```
   export default class TieredDynamicCapDiscount
      implements Commerce.DiscountCalculation<TieredDynamicCapConfig>
    { 
      computeDiscounts(request, config, _context) {
        const percent = Decimal.from(config.discountPercent)
          .div(Decimal.from('100'), 10, 'half-up');
        const tier1Max = Decimal.from(config.tier1MaxSeats);
        const freeSeats = Decimal.from(config.tier1FreeSeats);
        
        let seatPrice = Decimal.zero;
        let seatQty = Decimal.zero;
        let basePlanCost = Decimal.zero;
        for (const li of request.lineItems) {
          if (li.priceKind !== 'price') continue;
          if (li.price.product?.metadata[config.seatProductMetadataKey]
              === config.seatProductMetadataValue) {
            seatPrice = li.price.unitAmount ?? Decimal.zero;
            seatQty = li.quantity ?? Decimal.zero;
          } 
          if (li.price.product?.metadata[config.basePlanProductMetadataKey]
              === config.basePlanProductMetadataValue) {
            basePlanCost = li.subtotal.amount;
          } 
        } 
        // Tier 1: first N seats, capped at free seats × unit price
        const t1Seats = seatQty.gt(tier1Max) ? tier1Max : seatQty;
        const t1Raw = seatPrice.mul(t1Seats).mul(percent);
        const t1Cap = seatPrice.mul(freeSeats);
        const tier1 = t1Raw.gt(t1Cap) ? t1Cap : t1Raw;
        // Tier 2: remaining seats, capped at base plan cost
        let tier2 = Decimal.zero;
        if (seatQty.gt(tier1Max)) {
          const t2Raw = seatPrice.mul(seatQty.sub(tier1Max)).mul(percent);
          tier2 = t2Raw.gt(basePlanCost) ? basePlanCost : t2Raw;
        }
        return {
          discount: {
            amount: {
              amount: tier1.add(tier2),
              currency: request.grossAmount.currency,
            },
          },
        };
      }
    }
```

You might have been using workarounds to implement these discount strategies in your Stripe integration. Scripts remove the need to maintain and sync logic between your system and Stripe. If you are using self-hosted checkout purely to manage coupons, you can evaluate using Stripe-hosted checkout with scripts instead. Learn more about [discount calculation](https://docs.stripe.com/billing/scripts/discount-calculation) and [writing your own script](https://docs.stripe.com/billing/scripts/author-your-own).
