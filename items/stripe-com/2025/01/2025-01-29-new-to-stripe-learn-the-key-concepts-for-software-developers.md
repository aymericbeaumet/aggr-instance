---
title: New to Stripe? Learn the key concepts for software developers.
link: https://stripe.dev/blog/new-to-stripe-learn-key-concepts-software-developers
source: stripe-com
published: 2025-01-29T00:00:00Z
updated: 2025-01-29T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- James Beswick
summary: Discover the key concepts and terms for integrating Stripe in your applications. This guide covers Payment Intents Customer management, Subscription billing, Webhooks, Disputes, Refunds, and the benefits of expanded responses, providing developers with the tools they need to streamline payment processing efficiently.
content: extracted
html: 2025-01-29-new-to-stripe-learn-the-key-concepts-for-software-developers.html
remote_preview:
  url: https://stripe.dev/og/blog/new-to-stripe-learn-key-concepts-software-developers.png
  alt: 'Stripe Developer Blog: New to Stripe? Learn the key concepts for software developers.'
extra:
  content:interactive: true
---

Stripe is designed for developers, making it easy to integrate payments into your applications and workloads quickly. However, due to its extensive feature set and specific terminology, knowing a few key concepts can help you build your integrations even faster. This post walks you through the most important concepts and terms you'll encounter when integrating Stripe into your applications for the first time.

### It all starts with the Payment Intent

At the heart of Stripe's functionality is the [Payment Intent](https://docs.stripe.com/api/payment_intents), which represents your intent to collect payment from a customer. A Payment Intent tracks the entire payment lifecycle, from initial creation through successful completion or failure. When you create a Payment Intent, you specify the amount and currency you want to collect, and Stripe generates a [client secret](https://docs.stripe.com/api/payment_intents/object#payment_intent_object-client_secret) that you can use to complete the payment flow on the frontend.

Here's a simple example of creating a Payment Intent:

```
const stripe = require('stripe')('sk_test_your_key');

const paymentIntent = await stripe.paymentIntents.create({
  amount: 2000, // Amount in smallest currency unit (e.g., cents)
  currency: 'usd',
  payment_method_types: ['card'],
});
```

It’s recommended to create the Payment Intent once the chargeable amount is known (it can be updated later in the payment flow if needed). Each has a unique ID you can use to retrieve it later, and you can store the ID with the shopping cart or session in your application to make it easy to reference. You can reuse Payment Intents for the same transaction, which gives you access to any failed payment attempts for a cart or session.

Connected to Payment Intents are [Payment Methods](https://docs.stripe.com/payments/payment-methods/overview), which represent the various ways customers can pay. These include credit cards, bank transfers, digital wallets like [Apple Pay](https://www.apple.com/apple-pay/), and many other local payment methods. Each Payment Method has its own specific properties and requirements, but Stripe abstracts away much of this complexity through a unified API.

### Managing your customers

The [Customer object](https://docs.stripe.com/api/customers/object) is central to recurring billing and customer relationship management in Stripe, and helps you track who is making payments. While you can track this in your workload and store it in a database, Stripe can make this tracking easier by storing it for you. This is the preferred choice for many developers, since it simplifies the organization process and tracking payment history.

Instead of processing one-off payments, you can create a Customer and attach Payment Methods to them for future use. This is particularly useful for subscription-based businesses or platforms where users make repeated purchases.

```
const stripe = require('stripe')(process.env.STRIPE_SECRET_KEY);

async function createCustomer(email, name) {
  try {
    const customer = await stripe.customers.create({
      email,
      name,
      description: 'New customer created via API'
    });
    
    console.log('Customer created successfully:', customer.id);
    return customer;
  } catch (error) {
    console.error('Error creating customer:', error.message);
    throw error;
  }
}

async function createSetupIntent(customerId) {
  try {
    const setupIntent = await stripe.setupIntents.create({
      customer: customerId,
      payment_method_types: ['card'],
      usage: 'off_session' // Indicates the intent to use the payment method for future payments
    });
    
    console.log('Setup Intent created successfully:', setupIntent.id);
    return setupIntent;
  } catch (error) {
    console.error('Error creating setup intent:', error.message);
    throw error;
  }
}

async function attachPaymentMethod(paymentMethodId, customerId) {
  try {
    const paymentMethod = await stripe.paymentMethods.attach(
      paymentMethodId,
      { customer: customerId }
    );
    
    // Set this payment method as the default for the customer
    await stripe.customers.update(customerId, {
      invoice_settings: {
        default_payment_method: paymentMethodId
      }
    });
    
    console.log('Payment method attached successfully:', paymentMethod.id);
    return paymentMethod;
  } catch (error) {
    console.error('Error attaching payment method:', error.message);
    throw error;
  }
}

// Example usage
async function setupCustomerPayment(email, name, paymentMethodId) {
  try {
    // 1. Create a new customer
    const customer = await createCustomer(email, name);
    
    // 2. Create a setup intent for the customer
    const setupIntent = await createSetupIntent(customer.id);
    
    // 3. Attach the payment method to the customer
    const paymentMethod = await attachPaymentMethod(paymentMethodId, customer.id);
    
    return {
      customerId: customer.id,
      setupIntentId: setupIntent.id,
      paymentMethodId: paymentMethod.id,
      setupComplete: true
    };
  } catch (error) {
    console.error('Error in payment setup process:', error.message);
    throw error;
  }
}
```

## Subscription and billing concepts

Subscriptions are a common billing approach in many software-as-a-service products but can be difficult to manage by yourself. Typically, an end-user pays a set amount of money for a billing period to retain access to a service. However, there are many complexities, such as trial periods, discounts for longer commitments, and handling business flows if payments fail. These are all simplified by using Stripe, and the subscription model builds on the basics outlined above.

For subscription-based businesses, Stripe provides several key objects. A [Product](https://docs.stripe.com/api/products/object) represents what you're selling, while a Price defines how much it costs and the billing frequency. Together, these objects power Stripe's subscription system.

Products can be either goods or services, and they can have multiple Prices associated with them. For example, a software service might have monthly and annual pricing tiers:

```
// Create a product
const product = await stripe.products.create({
  name: 'Premium Subscription',
  description: 'Monthly access to premium features',
});

// Create prices for the product
const monthlyPrice = await stripe.prices.create({
  product: product.id,
  unit_amount: 1999, // $19.99
  currency: 'usd',
  recurring: {
    interval: 'month',
  },
});
```

A Subscription ties together a Customer with a Price, establishing recurring billing. The Subscription object tracks important details like the current period, status, and any trial periods:

```
const subscription = await stripe.subscriptions.create({
  customer: 'cus_123',
  items: [{ price: 'price_H5ggYwtDq4fbrJ' }],
  trial_period_days: 14
});
```

Once a subscription is established, Stripe will continue to take payments on the frequency you’ve established. Stripe uses webhooks to let you know the result of these transactions. Developers should listen for relevant events such as `invoice.payment_succeeded`, `invoice.payment_failed`, `customer.subscription.updated`, etc. This allows your application to respond to changes in subscription status and process any necessary actions (e.g., access permissions, updates to the subscription status in your database).

## Working with events and webhooks

Many payment processes are asynchronous, meaning that the state will change after the completion of the original API call. To avoid needing to poll for changes, Stripe uses events and webhooks to alert your application of important changes in state.

Stripe's event system is crucial for keeping your application in sync with payment-related activities. Events are notifications about specific occurrences in your Stripe account, such as successful payments, failed charges, or subscription updates. Each event has a type (e.g., 'payment\_intent.succeeded') and contains the relevant object data.

To handle these events, you'll need to set up Webhooks. A [Webhook Endpoint](https://docs.stripe.com/api/webhook_endpoints/create) is a URL where Stripe sends event notifications. To set up a webhook, first you need to let Stripe know which events you are listening to, and where they should be routed:

```
const stripe = require('stripe')('sk_test_key');
const webhookEndpoint = await stripe.webhookEndpoints.create({
  enabled_events: ['charge.succeeded', 'charge.failed'],
  url: 'https://example.com/my-webhook-handler,
});
```

Once this is created, Stripe will send events to that Webhook Endpoint. Securing your Stripe webhook with a signing secret is essential for ensuring that the requests your application receives from Stripe are genuine and have not been tampered with. When you set up a webhook endpoint in your application, Stripe provides a unique signing secret that you should store securely and use to verify incoming webhook requests.

For every event sent to your webhook URL, Stripe includes a signature in the `Stripe-Signature header`. By using the signing secret and the timestamp provided in the signature, you can construct a hash using the request body and confirm that it matches the signature sent by Stripe. This verification step helps protect against replay attacks and ensures that only payments and events originating from your Stripe account are processed, thereby enhancing the security of your financial transactions and user data. Read [Verify webhook signatures with official libraries](https://docs.stripe.com/webhooks#verify-webhook-signatures-with-official-libraries) to learn more about this process.

In a Node.js Express application, here's an example of handling a webhook:

```
app.post('/webhook', async (req, res) => {
  const sig = req.headers['stripe-signature'];
  let event;

  try {
    event = stripe.webhooks.constructEvent(
      req.body,
      sig,
      'whsec_your_webhook_secret'
    );
  } catch (err) {
    return res.status(400).send(`Webhook Error: ${err.message}`);
  }

  switch (event.type) {
    case 'payment_intent.succeeded':
      const paymentIntent = event.data.object;
      // Handle successful payment
      break;
    case 'customer.subscription.deleted':
      const subscription = event.data.object;
      // Handle subscription cancellation
      break;
  }

  res.json({received: true});
});
```

## Handling disputes and refunds

For any application taking payments, you must also handle exceptions to the regular billing process, either initiated by your application or by your customer.

In the payment world, [Disputes](https://docs.stripe.com/disputes) (also known as chargebacks) occur when customers contest charges with their bank. Stripe provides objects and workflows to handle these situations. A Dispute object contains details about the challenge and lets you submit evidence to respond. [Refunds](https://docs.stripe.com/refunds), on the other hand, are when you voluntarily return funds to a customer. A new refund must specify either a Charge or a Payment Intent, and the Refund object tracks these transactions:

```
const refund = await stripe.refunds.create({
  payment_intent: 'pi_123',
  amount: 1000, // Partial refund of $10
});
```

## Expanding responses to simplify your development

Expanded responses in the Stripe API provide developers with a powerful tool to access additional information about certain resources within a single response. This feature allows developers to receive more comprehensive data without the need for multiple API calls, significantly reducing both the number of requests made and the associated latency. By consolidating related data - such as customer information, payment methods, and transaction details - expanded responses streamline the development process and simplify the handling of data. This enhances clarity by offering a complete view of the information tied to a specific resource, making it easier for developers to understand the relationships between various entities within their applications.

The benefits of using expanded responses extend to improved efficiency in both development and application performance. By minimizing the number of separate API calls required, developers can quickly prototype and test their applications, leading to faster iteration cycles. This ultimately contributes to quicker feature rollouts and a better overall user experience. For instance, when retrieving subscription details, an expanded response can present crucial information like upcoming charges or related invoices in a single call, rather than necessitating multiple requests. By leveraging expanded responses, developers can build more performant and effective applications while reducing the complexity of data management.

Using this feature is straight forward, as shown below:

```
const paymentIntent = await stripe.paymentIntents.retrieve(
  'pi_123',
  {
    expand: ['customer', 'payment_method']
  }
);
```

## Conclusion

For more detailed information about any of these concepts, you can refer to [Stripe's comprehensive documentation](https://docs.stripe.com/). The documentation includes detailed API references, tutorials, and best practices for implementing these features in your application.

Remember that while this guide covers the most important concepts terminology, Stripe's feature set is extensive and constantly evolving. Staying updated with Stripe's documentation and the changelog will help you make the most of the platform's capabilities.

When building with Stripe, always start in test mode and make use of the dashboard's extensive debugging tools. The dashboard provides detailed logs of all API requests, webhook events, and other activities, making it invaluable for development and troubleshooting.

For more Stripe learning resources, subscribe to our [YouTube Channel](https://www.youtube.com/@StripeDev).
