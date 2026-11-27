---
title: 'Stay within limits: API rate-limit-friendly pattern for Stripe webhooks'
link: https://stripe.dev/blog/stay-within-limits-api-rate-limit-friendly-pattern-for-stripe-webhooks
source: stripe-com
published: 2025-07-03T00:00:00Z
updated: 2025-07-03T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Phil Leggetter
summary: Learn how to build a resilient, rate-limit-friendly system for handling Stripe webhooks at scale. This guide explains the fetch-before-process pattern, its risks under high volume, and how to use Hookdeck to queue and throttle webhooks—ensuring reliable processing without exceeding Stripe API limits.
content: extracted
html: 2025-07-03-stay-within-limits-api-rate-limit-friendly-pattern-for.html
remote_preview:
  url: https://stripe.dev/og/blog/stay-within-limits-api-rate-limit-friendly-pattern-for-stripe-webhooks.png
  alt: 'Stripe Developer Blog: Stay within limits: API rate-limit-friendly pattern for Stripe webhooks'
extra:
  content:interactive: true
---

When you're working with Stripe webhooks, you might be using a popular pattern: treat the incoming event as a signal, then fetch the full, up-to-date resource from the Stripe API. This [fetch-before-process](https://hookdeck.com/webhooks/guides/webhooks-fetch-before-process-pattern?ref=stripe-dev) approach is solid because it protects you from issues like duplicate or out-of-order webhooks. At scale, these problems aren't just possibilities - they're guarantees.

But here’s the catch: as your application grows, this pattern can backfire. A sudden spike in events, such as a burst of invoice.payment\_succeeded or checkout.session.completed webhooks can lead to a surge in API calls. If this exceeds your account’s rate limits (typically 100 read requests per second), Stripe begins returning 429 Too Many Requests responses to help protect service reliability.

When you hit a rate limit, you can let the request fail and rely on Stripe’s automatic retries, or implement your own retry logic. A more resilient approach is to queue incoming events from the start and throttle outbound API requests, ensuring you stay within rate limits and handle spikes gracefully.

This post walks you through a practical way to achieve the reliability of the fetch-before-process pattern without overwhelming the Stripe API. By using [Hookdeck Event Gateway](https://hookdeck.com/event-gateway?ref=stripe-dev) to queue and control incoming webhooks, it’s possible to process them at a safe pace.

## Why fetch-before-process can be risky

Treating webhooks as signals and then fetching the latest data is a growing trend for a good reason. Webhook payloads can be outdated, contain partial data, or arrive in the wrong order. And you might get the same event more than once.

As Alex, Hookdeck's CEO, explained in his [Stripe Meetup talk](https://www.youtube.com/watch?v=u2O-QS-A7jo), webhook-driven systems are event-driven by nature. This means you have to design for things like:

- **Idempotency:** So duplicate events don’t cause problems.
- **Event ordering:** Handling cases where an `update` event arrives before a `create` event.
- **Retries:** Having a solid strategy for reprocessing failed events.

Fetching the resource from Stripe’s API right before you process it helps you:

- Work with the most current data.
- Validate the resource's state.
- Simplify your retry logic, since the fetch will always get the latest version.

Here's what the code might look like in an Express.js application:

```
const stripe = new Stripe(process.env.STRIPE_API_KEY);

app.post(
  "/api/stripe/invoices",
  async (req: Request, res: Response) => {
    try {
      // Check Stripe signature and construct the Stripe event
      const sig = req.headers["stripe-signature"] as string;
      let event: Stripe.Event;
      try {
        event = stripe.webhooks.constructEvent(
          req.body,
          sig,
          process.env.STRIPE_WEBHOOK_SECRET
        );
      } catch (err) {
        console.log(`❌ Error:`, err);
        res.status(400).json(err);
        return;
      }

      if (event.type.startsWith("invoice.") === false) {
        res.status(400).send("Unexpected event type");
        return;
      }

      // Initially the webhook contains a snapshot of the invoice
      const invoiceSnapshot = event.data.object as Stripe.Invoice;
      const invoiceId = invoiceSnapshot.id;
      if (!invoiceId) {
        res.status(400).send("Invoice ID is missing");
        return;
      }

      // Retrieve the latest version of the invoice
      // You will hit the Stripe API rate limit here
      const invoice = await stripe.invoices.retrieve(invoiceId);
      console.log(`Processing event type: ${event.type}`);
      console.log(`Invoice:`, invoice);

      // Now you can process the invoice, knowing it's the latest version
      // ...

      res.sendStatus(200);
    } catch (err) {
      // Handle errors or network issues
      console.error("Error fetching event:", err);
      // Send Stripe error to Hookdeck for full observability
      res.status(500).json(err);
    }
  }
);
```

That’s all great, until your event volume grows. Stripe sends events fast. Imagine a flash sale, a large data migration, or a monthly subscription renewal run that generates thousands of events in just a few seconds. If your system tries to process events as they arrive, it may generate hundreds of API requests per second, quickly exceeding Stripe’s general 100 requests per second (RPS) limit and triggering 429 errors.

## A simple, scalable pattern

Here’s a more resilient approach:

1. **Receive webhooks with Hookdeck:** All your Stripe webhooks go to a single Hookdeck URL.
2. **Throttle delivery to your service:** Control the rate at which Hookdeck sends events to your application.
3. **Fetch the resource from Stripe:** Your code receives the throttled event and safely fetches the data.

The idea is simple: instead of hitting Stripe’s API as fast as webhooks arrive, decouple ingestion from processing. Hookdeck queues the events, and you decide how many your app should handle per second.

## Step-by-step: Building the flow

### 1\. Create a Hookdeck connection

Create a Hookdeck **Connection** in the [Hookdeck dashboard](https://dashboard.hookdeck.com?ref=stripe-dev). From the **Connections** page, click **Create Connection**.

**Note**: You can also create a connection with the [Hookdeck Terraform Provider](https://github.com/hookdeck/terraform-provider-hookdeck) or directly with the Hookdeck API. Examples of both of these are in the [Hookdeck Stripe Fetch Before Process example](https://github.com/hookdeck/hookdeck-demo/tree/main/stripe-fetch-before-process) on GitHub.

First, define the **Source** and select **Stripe** as the source type. Give the source a name such as `stripe_invoice_webhooks`. Don't enable **Authentication** yet as you need the Stripe webhook secret for that.

![](https://stripe.dev/images/stay-within-the-limits/image1.png)

Next, define the **Destination** for your connection.

![](https://stripe.dev/images/stay-within-the-limits/image5.png)

Select **HTTP** as the destination type and enter the URL for the endpoint where you want to receive the events.

Enable **Max delivery rate** to control how fast Hookdeck sends events to your service. Since you'll likely be working in a sandbox Stripe environment at the moment, set the rate-limit to 25 requests per second. In a live/production environment, you can increase this to 100 requests per second.

Hookdeck automatically queues any incoming events and delivers them to your service at the rate you've set. This protects your Stripe API quota, your database, and your server's capacity.

Name your connection something like `conn_stripe_invoices`, and click **Save**.

![](https://stripe.dev/images/stay-within-the-limits/image5.png)

You'll be presented with a URL in the form `https://hkdk.events/your-unique-id`, which is the Hookdeck endpoint that receives your Stripe webhooks. Copy this URL, as you'll need it to set up the webhook in Stripe.

![](https://stripe.dev/images/stay-within-the-limits/image8.png)

### 2\. Create a webhook event destination in Stripe

From the [Stripe Dashboard](https://dashboard.stripe.com/), use the search and go to the **Create a webhook** (**Create an event destination**) page.

Select the events you are interested in, such as `invoice.created`, `invoice.updated`, and `invoice.deleted`. Click **Continue**.

Choose **Webhook endpoint** as the destination type, and click **Continue**.

Enter a **Destination name,** such as `hookdeck: invoice webhook handler`, and paste the Hookdeck URL you copied earlier into the **Endpoint URL** field. Click **Create destination**.

![](https://stripe.dev/images/stay-within-the-limits/image6.png)

Once your webhook event destination is created, you'll see a **Signing secret**. Copy this secret, as you'll need it to authenticate incoming webhooks from Stripe in Hookdeck.

![](https://stripe.dev/images/stay-within-the-limits/image9.png)

### 3\. Configure Hookdeck to authenticate Stripe webhooks

In the Hookdeck dashboard, go to the **Connections** page. Click on the Stripe **Source** and click **Open Source**.

Within the Source page, enable **Authentication** and paste the Stripe webhook secret you copied earlier into the **Webhook Signing Secret** field. This ensures that Hookdeck verifies incoming webhooks from Stripe.

![](https://stripe.dev/images/stay-within-the-limits/image4.png)

Back on the **Connections** page, the Stripe source shows a small icon next to it, indicating that it is authenticated.

![](https://stripe.dev/images/stay-within-the-limits/image2.png)

### 4\. Handle Webhook and fetch the Stripe event

When your endpoint receives an event from Hookdeck, you can use the [Stripe Software Development Kit (SDK)](https://docs.stripe.com/sdks) to fetch the full event object. Because you've throttled the delivery, you don't have to worry about rate limits.

Here's the slightly modified Express.js code:

```
const stripe = new Stripe(process.env.STRIPE_API_KEY);export const verifyHookdeck = (
  req: Request,
  res: Response,
  next: NextFunction
) => {
  const hmacHeader = req.get("x-hookdeck-signature");
  const hmacHeader2 = req.get("x-hookdeck-signature-2");

  const hash = crypto
    .createHmac("sha256", process.env.HOOKDECK_WEBHOOK_SECRET as string)
    .update(req.body)
    .digest("base64");

  if (hash === hmacHeader || (hmacHeader2 && hash === hmacHeader2)) {
    next();
  } else {
    console.error("Signature is invalid, rejected");
    res.sendStatus(403);
  }
};

app.post(
  "/api/stripe/invoices",
  verifyHookdeck,
  async (req: Request, res: Response) => {
    try {
      // Check Stripe signature and construct the Stripe event
      const sig = req.headers["stripe-signature"] as string;
      let event: Stripe.Event;
      try {
        event = stripe.webhooks.constructEvent(
          req.body,
          sig,
          STRIPE_WEBHOOK_SECRET,
          // Disable timestamp checking since event was already check by Hookdeck
          // This also allows for failed events to be replayed.
          -1
        );
      } catch (err) {
        console.log(`❌ Error:`, err);
        res.status(400).json(err);
        return;
      }

      if (event.type.startsWith("invoice.") === false) {
        res.status(400).send("Unexpected event type");
        return;
      }

      // Initially the webhook contains a snapshot of the invoice
      const invoiceSnapshot = event.data.object as Stripe.Invoice;
      const invoiceId = invoiceSnapshot.id;
      if (!invoiceId) {
        res.status(400).send("Invoice ID is missing");
        return;
      }

      // Retrieve the latest version of the invoice
      // You will not hit the Stripe API rate limit here
      const invoice = await stripe.invoices.retrieve(invoiceId);
      console.log(`Processing event type: ${event.type}`);
      console.log(`Invoice:`, invoice);

      // Now you can process the invoice, knowing it's the latest version
      // ...

      res.sendStatus(200);
    } catch (err) {
      // Handle errors or network issues
      console.error("Error fetching event:", err);
      // Send Stripe error to Hookdeck for full observability
      res.status(500).json(err);
    }
  }
);
```

You may notice that the example uses a `verifyHookdeck` middleware function. This verifies that the incoming request is from Hookdeck, not directly from Stripe. However, the Stripe headers are still present, so you can verify the webhook signature if needed.

You can find the full code for the Express.js server in the [Hookdeck Stripe Fetch Before Process example](https://github.com/hookdeck/hookdeck-demo/tree/main/stripe-fetch-before-process) on GitHub.

### 5\. Test your endpoint locally

Run your application locally and expose it to the public internet with a localtunnel solution such as ngrok.

***Note**: The [Hookdeck Command Line Interface (CLI](https://hookdeck.com/docs/cli?ref=stripe-dev)) provides localtunnel functionality. However, CLI destinations can't presently have a delivery rate configured.*

Update the Destination URL to point to your localtunnel URL. Go to the **Connections** page in the Hookdeck dashboard, click on the **Destination**, update the URL (remember to include the `/api/stripe/invoices` path), and click **Save**.

Ensure you have the [Stripe Command Line Interface (CLI](https://docs.stripe.com/stripe-cli)) installed, and use it to send test webhooks to your Hookdeck endpoint:

```
stripe trigger invoice.created
```

![](https://stripe.dev/images/stay-within-the-limits/image3.png)

This helps with testing the general functionality. However, to test that Hookdeck is only delivering events to your endpoint at the defined delivery rate, you can change the rate to a very low value such as 10 per minute and trigger a number of test events and check how quickly the events reach your endpoint.

## Monitoring and backpressure

![](https://stripe.dev/images/stay-within-the-limits/image7.png)

When you use Hookdeck to queue and control your webhooks, you gain visibility into your event processing. You can monitor the queue depth and delivery delays in real-time, which helps you understand how your system is performing.

With the Hookdeck Event Gateway, it's simple to see what’s happening with your event queues:

- View queue depth and see delivery delays in real-time.
- Set up [alerts](https://hookdeck.com/issues) for when backpressure is building.
- [Retry](https://hookdeck.com/docs/retries) failed events, either manually or automatically.

If your processing starts to lag, you’ll know right away. You can then decide if you need to reach out to Stripe and ask for an API rate-limit increase, and then increase the delivery rate to catch up.

## When to use the Hookdeck and the fetch-before-process pattern

This setup is useful if:

- You’re already hitting Stripe API rate limits.
- You expect bursts of traffic, like from end-of-month billing runs, or you run batch updates.
- You want more control and better observability over your webhooks.
- You want to standardize how you process events.

The fetch-before-process pattern isn't always the right fit for every project, especially given the upper limits of Stripe's API rate limits. However, when used appropriately, it can significantly improve the reliability and resilience of your system.

## Conclusion

Stripe’s webhook system is reliable, and the fetch-before-process pattern is a strong foundation. But as your volume grows, it becomes increasingly important to manage event handling with care to avoid bottlenecks and rate limits.

Using Hookdeck Event Gateway to queue and throttle your webhooks gives you control over how your system consumes them. It’s a simple way to build a more resilient application, stay within API rate limits, and set your system up for sustainable growth.

Catch more details on using Stripe with Hookdeck in [this interview](https://www.youtube.com/watch?v=uwcSJFsQ83c) with co-founder Alexandre Bouchard, on the Stripe Developers YouTube channel.
