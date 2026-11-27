---
title: Introducing Stripe's new public preview release channel
link: https://stripe.dev/blog/introducing-stripes-new-public-preview-release-channel
source: stripe-com
published: 2025-05-07T00:00:00Z
updated: 2025-05-07T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Paul Anderson
summary: Discover the benefits of Stripe's new public preview release channel, designed to provide early access to cutting-edge API features before their general availability. Start innovating faster with Stripe’s latest offerings while sharing your feedback to shape future developments!
content: extracted
html: 2025-05-07-introducing-stripe-s-new-public-preview-release-channel.html
remote_preview:
  url: https://stripe.dev/og/blog/introducing-stripes-new-public-preview-release-channel.png
  alt: 'Stripe Developer Blog: Introducing Stripe''s new public preview release channel'
extra:
  content:interactive: true
---

At Stripe, we’re continually refining the way we release new features, empowering you to innovate faster. Today, we’re excited to announce our new and improved public preview release channel. We’re launching a new category of API versions, which give early access to upcoming features ahead of their General Availability (GA) launches.

Starting today, you can use the public preview release channel to access new Stripe features such as the [Accounts v2 API](https://docs.stripe.com/connect/accounts-v2/api) to represent your connected accounts and [Global Payouts](https://docs.stripe.com/global-payouts) to send payouts to customers, affiliates, contractors, or other third parties.

Check out all the new public preview features by visiting the [public preview changelog](https://docs.stripe.com/changelog?category=all&channel=preview).

## About public preview

The public preview release channel is designed to let you access brand new API features before they become generally available. Although these features are operationally stable and production-ready, they may receive breaking changes more often than GA features, and may be subject to some restrictions.

Here’s what you can expect:

- **Early access**: Public preview lets you work with new features ahead of their GA release, so you can build, test, and launch your integrations earlier.
- **Distinct API versions**: Public preview features will utilize preview API versions, which are distinct from GA versions. For example, `2025-04-30.preview` rather than `2025-04-30.basil`.
- **More frequent changes**: Unlike our GA API releases (which introduce breaking changes approximately twice a year), each new public preview API version may contain breaking changes. This cadence allows us to iterate quickly based on your feedback.
- **Some other restrictions may apply**: There may be additional restrictions on certain public preview features. For example, some features may not yet be available in certain geographic regions. Others may require one-time onboarding or approval from Stripe before use. Each feature’s documentation describes any restrictions.

## Get started with public preview

To make public preview API calls, you’ll send a **preview API version** via the `Stripe-Version` header. These versions end in `preview`, for example: `2025-04-30.preview`. You can view information for all preview versions in the [preview section of the changelog](https://docs.stripe.com/changelog?category=all&channel=preview).

Note that when upgrading from one preview version to another, there may be breaking changes. Any changes will be described in the changelog.

### SDKs

Going forward, Stripe’s beta [SDKs](https://docs.stripe.com/sdks) will use these new preview versions. The beta SDK releases are differentiated from their GA counterparts by special suffixes in their versions, such as `5.2.0-beta.1`. Beta SDKs will include preview features in addition to everything that is already in GA.

### API reference

Preview versions now appear in Stripe’s [API reference documentation](https://docs.stripe.com/api?api-version=preview); use the dropdown at the top-right to select a preview version. Code snippets will be populated with the preview version and corresponding types.

![](https://stripe.dev/images/introducing-stripes-new-public-preview-release-channel/image1.png)

## Illustrating the benefits of public preview

Now we’ll walk through a scenario that demonstrates the mechanics of the new preview API versions.

Let’s imagine Stripe is launching a new parameter `automatic_vacuum_vat` that can be set when creating an extraterrestrial payment. If set, the payment’s Vacuum VAT (the UN tax on payments that transit the vacuum of space) line items will be automatically calculated using the merchant and customer’s location, and then added to the checkout session.

The automatic Vacuum VAT feature already has high availability and low latency, and certain Stripe users want to start using it right away. However, for the time being, automatic Vacuum VAT only works when both the merchant and customer locations are planetary. In other words, the feature doesn’t yet support the merchant or customer being located in the asteroid belt or other non-planetary orbits. There are many users who are okay with that; they still want to use the feature, and would build around the planetary orbit-only restriction.

### In a GA-only world

Suppose `automatic_vacuum_vat` were immediately launched in a GA version—say, `2300-04-30.ziziphus`.

Some time later, an unsuspecting Stripe user working at a space freight platform noticed an AI coding agent attempting to use the `automatic_vacuum_vat` parameter. Upon conversing with the agent to learn the parameter’s purpose, the user is struck by the feature’s usefulness and applicability to their business. Without noticing the location restrictions in the official documentation, they instruct the coding agent to add it to their integration:

```
curl https://api.stripe.com/v1/extraterrestrial_payments \
  -H "Stripe-Version: 2300-04-30.ziziphus" \
  ...
  -d "automatic_vacuum_vat"=true
```

It works as expected in testing, and they even write a comprehensive automated test suite, and all the tests pass (the test merchants and customers all have Mars addresses). So the user rolls out the change to production.

Soon enough, the platform’s customer support channels are awash with space freighter SMBs who enabled the feature—confused why some payments have Vacuum VAT line items and others don’t.

### **With public preview**

Say instead that Stripe released `automatic_vacuum_vat` only in `2300-04-30.preview`, a public preview version, and not in GA.

In this world, our protagonist user tries to test out `automatic_vacuum_vat` in their integration:

```
curl https://api.stripe.com/v1/extraterrestrial_payments \
  -H "Stripe-Version: 2300-04-30.ziziphus" \
  ...
  -d "automatic_vacuum_vat"=true
```

But this time, Stripe returns a [`parameter_unknown` 400 error](https://docs.stripe.com/error-codes#parameter-unknown), because `automatic_vacuum_vat` doesn’t exist in `2300-04-30.ziziphus`, which is a GA version.

If the user had been using SDKs, they would have gotten this feedback even earlier:

```
ExtraterrestrialPaymentCreateParams.builder()
    ...
    .addAutomaticVacuumVat(true)  // compile error! method doesn't exist in the GA SDK
    .build();
```

Upon receiving this error, the user checks the documentation and sees that `automatic_vacuum_vat` is a public preview feature with certain caveats. To use it, they would have to make a conscious decision to change the API version to the appropriate preview version.

If they end up deciding to accept the restriction and integrate with public preview to use this feature, the `curl` would look like:

```
curl https://api.stripe.com/v1/extraterrestrial_payments \
  -H "Stripe-Version: 2300-04-30.preview"  \ # changed!
  ...
  -d "automatic_vacuum_vat"=true
```

But this is only for creating extraterrestrial payment API requests—the rest of the company’s API calls continue to use the GA version `2300-04-30.ziziphus`.

Although contrived, this story illustrates how the public preview release channel can provide additional guardrails for new features, while still giving you the option to use those new features as early as possible.

## Conclusion

Stripe’s new public preview release channel offers an opportunity for you to access and experiment with upcoming API features before they become generally available. Public preview versions use new versions that are distinct from GA versions. This approach enables you to integrate new functionality earlier while reducing risk, and lets you give more feedback to Stripe at an early stage.

We look forward to hearing what you think about the public preview release channel, and we’re excited to see what you build with it. As always, we welcome your thoughts and feedback on [Stripe Insiders](https://insiders.stripe.dev/).

For an overview of all of Stripe’s release channels, see [Product release phases](https://docs.stripe.com/release-phases).

To learn more about developing applications with Stripe, visit our [YouTube Channel](https://www.youtube.com/stripedevelopers).
