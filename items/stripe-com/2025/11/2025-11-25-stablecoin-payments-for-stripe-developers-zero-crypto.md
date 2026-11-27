---
title: 'Stablecoin payments for Stripe developers: zero crypto knowledge required'
link: https://stripe.dev/blog/using-stripe-stablecoin-payments-no-crypto-knowledge
source: stripe-com
published: 2025-11-25T00:00:00Z
updated: 2025-11-25T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Robert Love
summary: Learn how to add stablecoin payments to your Stripe integration in under an hour. Accept USDC on Ethereum, Solana, Polygon, and Base with zero blockchain knowledge—just add 'crypto' to your existing payment methods.
content: extracted
html: 2025-11-25-stablecoin-payments-for-stripe-developers-zero-crypto.html
remote_preview:
  url: https://stripe.dev/og/blog/using-stripe-stablecoin-payments-no-crypto-knowledge.png
  alt: 'Stripe Developer Blog: Stablecoin payments for Stripe developers: zero crypto knowledge required'
extra:
  content:interactive: true
---

Traditional crypto integrations require managing private keys, estimating gas fees, handling network selection, and building wallet connection flows. That's 50+ lines of complex code before you even process a payment.

But what if accepting stablecoin payments was as simple as adding `crypto` to your existing `payment_method_types` array?

With Stripe's stablecoin payments, you get all the benefits of crypto—instant global settlement, lower international fees, access to crypto-native users—using the exact same APIs, webhooks, and debugging tools you already know. No blockchain knowledge required.

Stripe currently supports USDC payments on [Ethereum](https://ethereum.org), [Solana](https://solana.com), [Polygon](https://polygon.technology), and [Base](https://base.org) networks, with transactions settling as fiat in your Stripe balance at a 1.5% fee. Your customers connect their crypto wallets, you get paid in USD. Everyone wins.

Here's how to add crypto payments to your existing Stripe integration in under an hour.

## Compare the implementation

**Example: Traditional crypto integration complexity:**

```
// Complex wallet connection, network selection, gas estimation...
const provider = new ethers.providers.Web3Provider(window.ethereum);
const signer = provider.getSigner();
const contract = new ethers.Contract(contractAddress, ABI, signer);
const gasPrice = await provider.getGasPrice();
const gasLimit = await contract.estimateGas.transfer(recipient, amount);
const tx = await contract.transfer(recipient, amount, {
  gasPrice: gasPrice,
  gasLimit: gasLimit
});
await tx.wait();
// Plus error handling, network switching, wallet detection, etc.
```

**Stripe crypto integration:**

```
// Same Payment Intent pattern you already use
const paymentIntent = await stripe.paymentIntents.create({
  amount: 2000,
  currency: 'usd',
  payment_method_types: ['card', 'crypto'], // Just add 'crypto'
});
```

The webhook you receive uses identical formatting. Your existing error handling, confirmation logic, and webhook processing work unchanged.

## Implementation steps

### Step 1: Enable crypto payments

Navigate to your [Payment methods settings](https://dashboard.stripe.com/settings/payment_methods) in the Stripe Dashboard and request the Crypto payment method. Stripe will review your business information and activate the feature once approved.

**Note:** Currently only US businesses can accept stablecoin payments.

### Step 2: Update your Payment Intent creation

Add `crypto` to your existing payment method types:

```
// Before: card payments only
const paymentIntent = await stripe.paymentIntents.create({
  amount: calculateOrderAmount(items),
  currency: 'usd',
  payment_method_types: ['card'],
  metadata: { order_id: '12345' }
});

// After: card + crypto payments  
const paymentIntent = await stripe.paymentIntents.create({
  amount: calculateOrderAmount(items),
  currency: 'usd',
  payment_method_types: ['card', 'crypto'], // Add crypto here
  metadata: { order_id: '12345' }
});
```

### Step 2a: Recommended approach with dynamic payment methods

For the most streamlined integration, Stripe recommends using dynamic payment methods instead of manually specifying payment method types. This approach lets you manage payment methods entirely through the Dashboard:

```
// Recommended: Use dynamic payment methods
const paymentIntent = await stripe.paymentIntents.create({
  amount: calculateOrderAmount(items),
  currency: 'usd',
  automatic_payment_methods: { enabled: true }, // Let Stripe handle payment methods
  metadata: { order_id: '12345' }
});
```

With dynamic payment methods enabled:

- **Manage payment methods in the Dashboard** - No code changes needed to add new payment methods
- **AI-optimized ordering** - Stripe's AI models automatically order payment methods for optimal conversion
- **Smart filtering** - Payment methods are automatically filtered based on amount, currency, and customer location
- **A/B testing capabilities** - Test new payment methods on a percentage of traffic

To enable crypto payments with this approach:

1. Navigate to your [Payment methods settings](https://dashboard.stripe.com/settings/payment_methods)
2. Turn on the **Crypto** payment method
3. Your integration automatically supports crypto payments—no code changes required

**Excluding specific payment methods**

If you need to exclude certain payment methods for specific transactions, you can still do so:

```
const paymentIntent = await stripe.paymentIntents.create({
  amount: calculateOrderAmount(items),
  currency: 'usd',
  automatic_payment_methods: { enabled: true },
  excluded_payment_method_types: ['affirm', 'klarna'], // Exclude specific methods
  metadata: { order_id: '12345' }
});
```

### Step 3: Frontend integration (no changes required)

If you're using [Stripe Checkout](https://stripe.com/payments/checkout), crypto payments appear automatically:

```
// Dynamic payment methods with Checkout (recommended)
const session = await stripe.checkout.sessions.create({
  // Remove payment_method_types to use dynamic payment methods
  line_items: [{
    price_data: {
      currency: 'usd',
      product_data: { name: 'Llama Lamp' },
      unit_amount: 2000,
    },
    quantity: 1,
  }],
  mode: 'payment',
  success_url: `${YOUR_DOMAIN}/success?session_id={CHECKOUT_SESSION_ID}`,
  cancel_url: `${YOUR_DOMAIN}/cancel`,
});

// Or specify payment methods explicitly
const session = await stripe.checkout.sessions.create({
  payment_method_types: ['card', 'crypto'],
  line_items: [{
    price_data: {
      currency: 'usd',
      product_data: { name: 'Llama Lamp' },
      unit_amount: 2000,
    },
    quantity: 1,
  }],
  mode: 'payment',
  success_url: `${YOUR_DOMAIN}/success?session_id={CHECKOUT_SESSION_ID}`,
  cancel_url: `${YOUR_DOMAIN}/cancel`,
});
```

For [Elements](https://stripe.com/payments/elements) implementations, the crypto option appears automatically in your payment method selection:

```
const elements = stripe.elements({
  clientSecret: paymentIntent.client_secret,
});
const paymentElement = elements.create('payment');
paymentElement.mount('#payment-element');
// Crypto appears as an option automatically
```

### Step 4: Handle the payment flow

When you choose crypto, you're redirected to complete your payment with your preferred wallet. The flow looks like this:

1. You select "Pay with Crypto" on checkout
2. Redirect to crypto.stripe.com where you connect your wallet ([MetaMask](https://metamask.io/), [Coinbase Wallet](https://www.coinbase.com/wallet), etc.)
3. Payment confirmation happens on-chain
4. Return to the success page with the same confirmation flow as card payments

Your confirmation handling remains identical:

```
app.get('/success', async (req, res) => {
  try {
    const { payment_intent } = req.query;
    
    if (!payment_intent) {
      return res.status(400).send('Missing payment_intent parameter');
    }
    
    const paymentIntent = await stripe.paymentIntents.retrieve(payment_intent);
    
    if (paymentIntent.status === 'succeeded') {
      // Process successful payment (card OR crypto)
      await fulfillOrder(paymentIntent.metadata.order_id);
      res.render('success', { payment_intent: paymentIntent });
    } else {
      res.redirect('/payment-failed');
    }
  } catch (error) {
    console.error('Error retrieving payment:', error);
    res.status(500).send('Payment verification failed');
  }
});
```

### Step 5: Webhook handling (zero changes)

Your existing webhook handlers work with crypto payments without modification:

```
app.post('/webhook', express.raw({type: 'application/json'}), (req, res) => {
  const sig = req.headers['stripe-signature'];
  let event;
  
  try {
    event = stripe.webhooks.constructEvent(req.body, sig, process.env.STRIPE_WEBHOOK_SECRET);
  } catch (err) {
    console.log(`Webhook signature verification failed:`, err.message);
    return res.status(400).send(`Webhook Error: ${err.message}`);
  }
  
  switch (event.type) {
    case 'payment_intent.succeeded':
      const paymentIntent = event.data.object;
      console.log(`Payment ${paymentIntent.id} succeeded!`);
      // Your fulfillment logic here
      break;
    case 'payment_intent.payment_failed':
      const failedPayment = event.data.object;
      console.log(`Payment ${failedPayment.id} failed:`, failedPayment.last_payment_error?.message);
      break;
    default:
      console.log(`Unhandled event type ${event.type}`);
  }
  
  res.status(200).end();
});
```

## Testing and debugging

### Sandbox integration

Stripe's crypto payments work in [Sandbox environments](https://docs.stripe.com/test-mode) just like card payments—no testnets or fake cryptocurrency required. Sandbox is Stripe's isolated testing environment where you can simulate payments without real money changing hands. When you create a Payment Intent with crypto as a payment method in a Sandbox, you'll see the same crypto payment flow that customers experience, but using simulated transactions.

Stripe's Sandbox environments give you multiple isolated testing spaces to experiment with different configurations, perfect for testing crypto payment flows alongside your existing card payment tests.

```
const testPaymentIntent = await stripe.paymentIntents.create({
  amount: 1000,
  currency: 'usd',
  automatic_payment_methods: { enabled: true }, // Test with dynamic payment methods
}, {
  stripeVersion: '2025-06-30', // Use latest API version
});
```

Use the same [test card numbers](https://stripe.com/docs/testing#cards) and test scenarios you're already familiar with to simulate different payment outcomes.

### Debugging with Stripe Workbench

[Stripe Workbench](https://stripe.com/workbench) works with crypto payments using the same interface you know. You can inspect payment flows end-to-end, debug webhook delivery, monitor success rates, and test error scenarios without blockchain complexity.

- Inspect payment flows end-to-end, including crypto payment redirects
- Debug webhook delivery with the same tools
- Monitor success rates for crypto vs. card payments
- Test error scenarios without dealing with blockchain complexity

### Local development

Test webhooks locally using the [Stripe CLI](https://stripe.com/docs/stripe-cli):

```
# Same CLI command, handles crypto webhooks automatically
stripe listen --forward-to localhost:4242/webhook
```

## Production considerations

### Performance characteristics

Crypto payments have slightly different timing than card payments:

- **Initial authorization:** Similar to cards (~2-3 seconds)
- **Blockchain confirmation:** Additional 30 seconds to 5 minutes depending on network congestion
- **Settlement in your Stripe balance:** Settles as USD after blockchain confirmation

Handle this timing difference the same way you handle any payment method with variable processing times.

### Error scenarios

Crypto payments can fail for blockchain-specific reasons, but Stripe abstracts these into familiar payment failure patterns:

```
const paymentIntent = await stripe.paymentIntents.retrieve(pi_id);

if (paymentIntent.status === 'requires_payment_method') {
  // Handle crypto-specific and general payment failures
  const errorCode = paymentIntent.last_payment_error?.code;
  const errorType = paymentIntent.last_payment_error?.type;
  
  switch (errorCode) {
    case 'card_declined':
    case 'insufficient_funds':
    case 'authentication_required':
      // Standard error handling patterns apply
      console.log('Payment failed:', paymentIntent.last_payment_error.message);
      break;
    default:
      console.log('Payment requires new method:', errorCode);
  }
}
```

### Monitoring and alerts

Your existing Stripe monitoring works with crypto payments:

- Dashboard metrics include crypto payment volume and success rates
- Webhook monitoring uses standard alerting for failed deliveries
- Revenue reporting combines crypto and card payments seamlessly

The main operational difference: crypto payments charge 1.5% vs. typical 2.9% + $0.30 for cards, which may improve your unit economics.

## What you get with crypto payments

Instead of simply listing features, let's look at what this means for your business. When you add crypto payments to your Stripe integration, you're not just adding another checkbox to your payment form—you're opening your business to a global audience that values the speed and transparency of blockchain-based transactions.

Your crypto-native customers, who might otherwise abandon their cart when they see only traditional payment options, can now complete purchases using the USDC they already hold. This is particularly valuable for businesses like Llama Lamps, where your customers might be distributed globally and prefer the instant settlement that crypto provides.

The lower processing fees (1.5% compared to typical card rates) can meaningfully impact your margins, especially for higher-value transactions. For a $200 lamp purchase, you save approximately $2.80 compared to standard card processing—small per transaction, but significant at scale when you're selling hundreds of lamps monthly.

Most importantly, this all happens without requiring you to learn blockchain development, manage private keys, or worry about gas fees. Your existing Stripe expertise transfers completely.

**Implementation timeline:**

- **Dashboard setup:** 5 minutes to request access
- **Code changes:** 15 minutes to add `'crypto'` to existing integration (or 5 minutes with dynamic payment methods)
- **Testing:** Same timeframe as testing any new payment method
- **Production deployment:** No additional operational complexity

## Next steps

Ready to add crypto payments to your application? Start by enabling crypto in your [Payment methods settings](https://dashboard.stripe.com/settings/payment_methods) and add `'crypto'` to your next Payment Intent—or better yet, switch to dynamic payment methods for the most flexible approach.

Monitor adoption rates to track what percentage of users choose crypto payments, and consider geographic optimization by enabling crypto primarily in regions where it performs best.

For advanced use cases requiring custom stablecoin workflows, explore [Bridge APIs](https://docs.stripe.com/bridge) when you outgrow Stripe's standard crypto integration.

**Resources:**

- [Stripe Crypto Payments Documentation](https://docs.stripe.com/crypto): Complete API reference and setup guide
- [Dynamic Payment Methods Guide](https://docs.stripe.com/payments/payment-methods/dynamic-payment-methods): Learn about Dashboard-based payment method management
- [Crypto Payments Demo](https://buy.stripe.com/test_28o4ig0SY9Xq8co3cc): See the customer experience in action
- [Stripe Discord](https://discord.gg/stripe): Get help from other developers implementing crypto payments
- [Workbench](https://stripe.com/workbench): Debug crypto payment flows with familiar tools

Stripe's crypto payments deliver all the benefits of blockchain-based payments—instant settlement, global reach, lower fees—using the APIs and developer tools you already know. No crypto expertise required, no operational complexity added.

To learn more about developing applications with Stripe, visit our [YouTube Channel](https://www.youtube.com/stripedevelopers).
