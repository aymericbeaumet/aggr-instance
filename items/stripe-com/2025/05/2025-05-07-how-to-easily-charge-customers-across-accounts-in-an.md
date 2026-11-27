---
title: How to easily charge customers across accounts in an organization
link: https://stripe.dev/blog/how-to-charge-customers-across-accounts
source: stripe-com
published: 2025-05-07T00:00:00Z
updated: 2025-05-07T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Daniel Wood
summary: Learn how to simplify customer charging across multiple accounts in your organization using Stripe's new sharing feature.
content: extracted
html: 2025-05-07-how-to-easily-charge-customers-across-accounts-in-an.html
remote_preview:
  url: https://stripe.dev/og/blog/how-to-charge-customers-across-accounts.png
  alt: 'Stripe Developer Blog: How to easily charge customers across accounts in an organization'
extra:
  content:interactive: true
---

Over 1 million businesses on Stripe manage multiple accounts to represent different business lines or to acquire locally in a new country. Having multiple accounts allows you to isolate finances and reporting, or accept money in multiple currencies, but it makes it challenging to maintain a unified payment experience for your customers, because you can only charge cards in the same account in which you collected them.

With [Organizations](https://docs.stripe.com/get-started/account/orgs), you can now automatically [share customers and payment methods across accounts](https://docs.stripe.com/get-started/account/orgs/sharing/customers-payment-methods) to create a seamless checkout experience across parts of your business without recollecting cards or billing details from your customers. This blog post explains how sharing works to allow you to charge customers across multiple accounts.

This capability is currently in preview and you can [request access](https://docs.stripe.com/get-started/account/orgs/sharing/customers-payment-methods) now. If you have any questions or feedback, we’d love to hear from you at [organizations-feedback@stripe.com](mailto:organizations-feedback@stripe.com).

![image 1](https://stripe.dev/images/how-to-charge-customers-across-accounts/image5.png)

### Why sharing matters

Sharing customers and payment methods across accounts makes both your payment integration simpler and the checkout experience smoother for your customers.

- **One-click checkout**: Reduce friction by enabling customers to make a purchase on a new business line using a card previously saved from another business line.

- **Consistent and personalized experience:** Maintain a canonical customer profile across various accounts, so you and your customers don't need to update payment methods, billing, or contact information in multiple places.

- **Holistic reporting:** View a customer’s complete transaction history and lifetime spend across your entire business. Shared customers and payment methods have the same ID across accounts, so you only need to store one ID in your database.

- **Easy expansion:** Launch a new business line or geography without conducting a lengthy data migration, maintaining ID mappings, or manually syncing information across accounts.

### How sharing works

You can turn on customer and payment method sharing between two or more accounts belonging to the same organization. When enabled, all new and existing customers and their saved cards are automatically shared between accounts.

### Important developer considerations

- You can’t remove or disable an account from sharing after enabling the feature.
- All new and existing customers are shared. You can’t selectively share individual customers.
- You can only share payment methods if the type is a card. You can still save other reusable payment method types to an account, but you won’t be able to charge the customer with them from another account.
- You can’t enable sharing between connected accounts under a platform.
- You must ensure that you collect consent from all customers to share their payment information between accounts in your organization prior to enabling sharing. Stripe gathers this consent for you if you are using a hosted checkout surface like Elements or Checkout. If your checkout experience uses a custom frontend, make sure to update your consumer terms and conditions.

### Shared customers

Shared customers have the same customer ID across accounts. When you create or update a shared customer, only certain fields of the customer object are automatically shared between accounts, including:

- [name](https://docs.stripe.com/api/customers/object#customer_object-name)
- [email](https://docs.stripe.com/api/customers/object#customer_object-email)
- [address](https://docs.stripe.com/api/customers/object#customer_object-address)
- [phone](https://docs.stripe.com/api/customers/object#customer_object-phone)
- [tax\_ids](https://docs.stripe.com/api/customers/object#customer_object-tax_ids)
- [description](https://docs.stripe.com/api/customers/object#customer_object-description)
- [preferred\_locales](https://docs.stripe.com/api/customers/object#customer_object-preferred_locales)

The remaining fields (for example, [metadata](https://docs.stripe.com/api/customers/object#customer_object-metadata), [shipping](https://docs.stripe.com/api/customers/object#customer_object-shipping)) are not shared between accounts, since that information may frequently diverge between business lines. For example, suppose a customer Jenny Rosen is shared between two accounts, Rocket Rides and Rocket Deliveries. Here, we’ll add metadata to Jenny that is specific to Rocket Rides.

```
// Jenny Rosen in Rocket Rides
{
  "id": "cus_9DrHraV8v0dsx0",
  "object": "customer",
  "address": {
    "city": "San Francisco",
    "country": "US",
    "line1": "123 Market Street",
    "postal_code": "94103",
    "state": "CA"
  },
  "created": 1745976923,
  "email": "jenny@example.com",
  "name": "Jenny Rosen",
  "phone": "4142079823",
  "metadata": {                     // Metadata is not shared across accounts
    "RocketRidesId": "rr_4165059"
  },
  // ... other Stripe customer fields
}
```

When Jenny is shared to Rocket Deliveries, she can be retrieved in that account using the same customer ID, and certain fields including her name, email, and phone number are shared from Rocket Deliveries. However, any metadata added in Rocket Rides is not accessible from Rocket Deliveries.

```
// Jenny Rosen in Rocket Rides
{
  "id": "cus_9DrHraV8v0dsx0",       // Same customer ID as Rocket Rides
  "object": "customer",
  "address": {
    "city": "San Francisco",
    "country": "US",
    "line1": "123 Market Street",
    "postal_code": "94103",
    "state": "CA"
  },
  "created": 1745976923,
  "email": "jenny@example.com",
  "name": "Jenny Rosen",
  "phone": "4142079823",
  "metadata": {}, 			  // Metadata from Rocket Rides is not accessible
  // ... other Stripe customer fields
}
```

If you update any of the shared fields for a customer, Stripe generates separate `customer.updated` events for each account where sharing is enabled. If you update an unshared field for the customer, Stripe sends an update event to only that account.

#### Shared payment methods

Sharing customers across accounts enables you to charge their saved payment methods from any account as well (as long as the payment method is of type card, for now). Shared payment methods have the same ID across all accounts, and updating or deleting a shared payment method affects all accounts.

If you update or attach a payment method to a shared customer in one account, Stripe generates a single `payment_method.attached` or `payment_method.updated` event to only that account. We recommend using [organization-level webhooks](https://docs.stripe.com/webhooks#webhook-endpoint-def) to listen to all events related to shared customers and payment methods.

## Getting started with your integration

The following guide will walk through how to test charging a customer across multiple accounts in your organization’s sandbox and help you understand how sharing works without charging real money.

### Request access to customer and payment method sharing

Before you can begin, [request access](https://docs.stripe.com/get-started/account/orgs/sharing/customers-payment-methods) for your organization. We’ll let you know once your account has been enabled.

### Create a sandbox for your organization

To get started, you’ll create an organization inside of a sandbox to test out customer and payment method sharing without charging real money. Before you begin, you’ll need to have already [created your live mode organization](https://docs.stripe.com/get-started/account/orgs/build).

1. Navigate to your organization in the Dashboard, and from the account picker, click on **Switch to sandbox** and **Create sandbox**.

![image 7](https://stripe.dev/images/how-to-charge-customers-across-accounts/image7.png)

2. Select the **Organization** sandbox type and **Name** your organization. Choose **Copy my organization structure** to clone your live mode organization in a sandbox.

![image 12](https://stripe.dev/images/how-to-charge-customers-across-accounts/image12.png)

### Enable customer and payment method sharing

1. Once you’ve created your organization, navigate to **Settings.** Click on **Customer and payment method sharing**.

![image 4](https://stripe.dev/images/how-to-charge-customers-across-accounts/image4.png)

2. Click **Get started** to start enabling sharing between your accounts.

![image 9](https://stripe.dev/images/how-to-charge-customers-across-accounts/image9.png)

3. Select the accounts for which you want to share customers and payment methods. Remember that only some customer information, such as name, email, address, and phone number are shared between accounts. Other customer information, such as metadata and default payment method remain separate.

![image 2](https://stripe.dev/images/how-to-charge-customers-across-accounts/image2.png)

4. Select **Share**. You will be required to confirm that you have already collected customer consent to share your customers’ payment information between accounts.

Once you’ve enabled sharing, customers and payment methods you create or update in one account will be reflected across accounts in your organization. This makes it easy to charge customers using payment methods you’ve previously collected across any of your accounts. In this example, you’ll create and charge a customer using one saved card on two different accounts, using Stripe Checkout.

### Create a checkout session in the first account

In your server-side integration, create a new customer in the first account, Rocket Rides, as part of a checkout session.

```
const stripe = require('stripe')('{{ROCKET_RIDES_SECRET_KEY}}');
const session = await stripe.checkout.sessions.create({
  customer_creation: 'always',
  line_items: [
    {
      price_data: {
        currency: 'usd',
        product_data: {
          name: 'Ride service',
        },
        unit_amount: 7500,
      },
      quantity: 1,
    },
  ],
  mode: 'payment',
  ui_mode: 'embedded',
  return_url: 'https://checkout.rocket-rides.com/success',
  saved_payment_method_options: {
    payment_method_save: 'enabled',
  },
}, {stripeAccount: '{{ACCT_ROCKET_RIDES}}'});   // Account context for customer creation
```

![image 3](https://stripe.dev/images/how-to-charge-customers-across-accounts/image3.png)

Once the customer completes the checkout process, if the customer elects to save their payment method, Stripe will create a customer object and attach the payment method. Both the customer and payment method will be automatically shared to the other account in the organization without requiring any additional API calls.

### Create a checkout session in the second account

When you create a checkout session for the customer in the second account, you can pass in the same customer ID from the first account, and Stripe will automatically populate the customer’s saved card.

```
const stripe = require('stripe')('{{ROCKET_DELIVERIES_SECRET_KEY}}');
const session = await stripe.checkout.sessions.create({
  customer: '{{CUSTOMER ID}}',		 // Shared customer ID
  line_items: [
    {
      price_data: {
        currency: 'usd',
        product_data: {
          name: 'Delivery service',
        },
        unit_amount: 5000,
      },
      quantity: 1,
    },
  ],
  mode: 'payment',
  ui_mode: 'embedded',
  return_url: 'https://checkout.rocket-deliveries.com/success',
  saved_payment_method_options: {
    payment_method_save: 'enabled',
  },
}, {stripeAccount: '{{ACCT_ROCKET_DELIVERIES}}'});   // Account context for checkout session
```

![image 8](https://stripe.dev/images/how-to-charge-customers-across-accounts/image8.png)

With Organizations, you now see a unified view of your shared customers, including their payment methods and transactions across all accounts. In this example, you can see the customer’s saved card which was used to process transactions in both accounts.

This new view enables you to easily understand a customer’s holistic relationship with your business. From the organization, you can manage refunds, update customer details, and add or remove payment methods for shared customers.

![image 10](https://stripe.dev/images/how-to-charge-customers-across-accounts/image10.png)

## Centralize event management with an organization webhook

If you use webhooks to subscribe to API events across multiple accounts, you can create a single organization webhook to centrally subscribe to all API events across your organization. This replaces the need for you to create and maintain separate webhooks for each of your accounts, and also ensures that you will only receive one de-duplicated event for updates to customers and payment methods shared between multiple accounts. To create an organization webhook:

1. Open Workbench by clicking on the **Developers** menu in the lower left corner of the dashboard and selecting **Webhooks.** Click **Add destination**.

![image 6](https://stripe.dev/images/how-to-charge-customers-across-accounts/image6.png)

2. Select **Accounts in your organization** and select the events you want to subscribe to.

![image 1](https://stripe.dev/images/how-to-charge-customers-across-accounts/image1.png)

3. Choose the destination type and [configure your event destination](https://docs.stripe.com/event-destinations#supported-destination-types).

![image 11](https://stripe.dev/images/how-to-charge-customers-across-accounts/image11.png)

4. Create an event handler in your server side code. In the following example, parse the session, account context, and customer ID from the event. The [account context](http://docs.stripe.com/context) contains the account ID where the event was emitted, which may be required if your event handler takes different action depending on the account. Adding the account context to webhook events makes it possible for you to create a single event handler for events received across your organization.

```
app.post('/webhook', express.json({ type: 'application/json' }), async (request, response) => {
  const event = request.body;

  // Handle the checkout.session.completed event
  if (event.type === 'checkout.session.completed') {
    const session = event.data.object; 		// Get the session object
    const accountContext = event.context;	// Get the account context
    const customerId = session.customer; 	// Get the customer ID

    try {
    // Define event handler behavior
    } catch (error) {
      console.error("Error:", error);
    }
  } 
  response.json({ received: true });      // Acknowledge receipt of the event
});
```

## Conclusion

With customer and payment method sharing, it is now easier than ever to create a unified payment experience for your customers across multiple business lines. Sharing replaces the need to recollect customers’ payment methods, migrate customers and payment methods between accounts, or set up Connect to clone customers across your accounts.

This capability is currently in preview and you can [request access](https://docs.stripe.com/get-started/account/orgs/sharing/customers-payment-methods) now. If you have any questions or feedback, we’d love to hear from you at [organizations-feedback@stripe.com](mailto:organizations-feedback@stripe.com).

For more Stripe learning resources, subscribe to our [YouTube channel](https://www.youtube.com/stripedevelopers).
