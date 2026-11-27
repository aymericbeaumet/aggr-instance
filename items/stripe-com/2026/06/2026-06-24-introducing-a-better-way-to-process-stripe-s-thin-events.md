---
title: Introducing a better way to process Stripe's thin events
link: https://stripe.dev/blog/event-notification-handlers-thin-events
source: stripe-com
published: 2026-06-24T00:00:00Z
updated: 2026-06-24T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- David Brownman
summary: Event destinations are central to Stripe integrations but hard to get right. Event notification handlers encapsulate the setup and wiring, letting you focus on business logic and surface errors at development time instead of in production.
content: extracted
html: 2026-06-24-introducing-a-better-way-to-process-stripe-s-thin-events.html
remote_preview:
  url: https://stripe.dev/og/blog/event-notification-handlers-thin-events.png
  alt: 'Stripe Developer Blog: Introducing a better way to process Stripe''s thin events'
extra:
  content:interactive: true
---

Handling events is a cornerstone of a successful Stripe integration. But, correctly setting up and maintaining an [event destination](https://docs.stripe.com/event-destinations) is an error-prone process. While authoring, there are a lot of event shapes and keys to manage; static typing doesn't give you much guidance in our non-TypeScript SDKs.

Once your event destination is set up, upgrades can be tricky. Because the shape of events are tied to a specific API version, upgrading either your event destination or SDK (especially in strongly typed languages) can lead to unexpected runtime errors.

Stripe's solution to the maintenance problem is [thin events](https://docs.stripe.com/event-destinations#thin-events), which are unversioned JSON payloads. Because they're identical across API versions, you can use them with *any* SDK version, making upgrades much safer. But the authoring problem remains: how can the SDKs present as much information about the shape of events as early as possible?

Enter [event notification handlers](https://docs.stripe.com/webhooks/event-notification-handlers). They encapsulate everything your event handler needs to run and let you focus on your business logic. They surface errors when you're writing code instead of later, in production.

> If you'd like early access to thin events for v1 resources, please [fill out this form](https://forms.gle/tgPakyrUakYoDm6L7).

## How it works today

When handling thin events today, you need to write a lot of code to validate and route the event correctly. You're also responsible for managing the Stripe-Context header when handling events for connected accounts.

Here's what a TypeScript handler might look like today:

```
const app = express();
const stripe = new Stripe("sk_test_YOUR_KEY_HERE");

app.post(
  '/webhook',
  express.raw({type: 'application/json'}),
  async (req, res) => {
    const sig = req.headers['stripe-signature']?.[0] ?? '';

    try {
      const eventNotification = stripe.parseEventNotification(
        req.body,
        sig,
        webhookSecret
      );

      if (eventNotification.type == 'v1.billing.meter.error_report_triggered') {
        console.log(
          `Meter w/ id ${eventNotification.related_object.id} had a problem`
        );

        const event = await eventNotification.fetchEvent();
        console.log(`More info: ${event.data.developer_message_summary}`);
      } else if (eventNotification.type === 'v1.billing.meter.no_meter_found') {
        const meters = await stripe.billing.meters.list(undefined, {
          // important!
          stripeContext: eventNotification.context,
        });

        console.log(
          `Meter not found. Available meters for this account are: ${meters.data
            .map((m) => m.id)
            .join(', ')}`
        );
        // ...
      } else {
        console.log(`Received unhandled event type: ${eventNotification.type}`);
      }

      res.sendStatus(200);
    } catch (err) {
      console.log(`Webhook Error: ${(err as any).stack}`);
      res.status(400).send(`Webhook Error: ${(err as any).message}`);
    }
  }
);
```

Notice how all of your business logic is interwoven in webhook-related hardware? Even if you split your logic into small functions, it's still up to you to wire everything up correctly. Plus you have to hope (and verify) that you haven't made any typos in those webhook type strings.

## A better way

Event notification handlers abstract away as much of this grunt work as possible, keeping you focused on the unique parts of your implementation. The Stripe SDK takes care of all the casting, routing, validating, and conversions behind the scenes. Let's walk through some examples.

### Create a handler

The first step is initializing the handler itself. If you're not using `StripeClient` yet, you can use it for just this bit of your integration without changing anything else. There's more info [in our docs](https://docs.stripe.com/sdks/server-side#stripeclient).

Once you have an authenticated client, create your handler. Make sure to provide a "fallback callback", the function that gets called when the event isn't otherwise handled:

```
const stripe = new Stripe("sk_test_YOUR_KEY_HERE");

const handler = stripe.notificationHandler(
  webhookSecret,
  async (unhandledEvent, client, details) => {
    console.log(
      `Received unhandled event type: ${unhandledEvent.type}; the SDK does${
        details.isKnownEventType ? '' : ' not'
      } have types for it`
    );
  }
);
```

### Handle a thin event

You can declare callback functions anywhere in your codebase:

```
// webhookCallbacks.ts
const handleNoMeterFound = async (
  notification: Stripe.V2.Core.Events.V1BillingMeterNoMeterFoundEventNotification,
  client: Stripe
) => {
  // don't need to specify stripeContext! it just works
  const meters = await client.billing.meters.list();

  console.log(
    `Meter not found. Available meters are: ${meters.data
      .map((m) => m.id)
      .join(', ')}`
  );
};

// eventDestination.ts
import {handleNoMeterFound} from './webhookCallbacks.ts'

const handler = stripe.notificationHandler(...) // same as above

handler.on('v1.billing.meter.no_meter_found', handleNoMeterFound);
```

That `handler.on(...)` call validates (at compile time) that the argument types for your callback match those expected for the `v1.billing.meter.no_meter_found` event, so you don't have any surprises at runtime.

The SDK will supply the parsed event notification and a special instance of `StripeClient` that's pre-bound to the context of the event. It also retains all the settings from the `client` instance that created the handler, so no need to re-configure anything. You can make any additional API calls you want.

Best of all, because `handleNoMeterFound` is just a function, you can test it as part of your normal development flow.

In some languages (like TypeScript), you can even declare callbacks inline, meaning they have the exact correct types without having to specify anything:

```
handler.on(
  'v1.billing.meter.error_report_triggered',
  async (eventNotification, client) => { // these are automatically typed correctly
    console.log(
      `Meter w/ id ${eventNotification.related_object.id} had a problem`
    );

    const event = await eventNotification.fetchEvent();
    console.log(`More info: ${event.data.developer_message_summary}`);
  }
);
```

### The final result

Put it all together and you've got a much more maintainable event destination:

```
const app = express();
const stripe = new Stripe("sk_test_YOUR_KEY_HERE");

const apiKey = process.env.STRIPE_API_KEY ?? '';
const webhookSecret = process.env.WEBHOOK_SECRET ?? '';

const handler = stripe.notificationHandler(
  webhookSecret,
  async (unhandledEvent, client, details) => {
    console.log(
      `Received unhandled event type: ${unhandledEvent.type}; the SDK does${
        details.isKnownEventType ? '' : ' not'
      } have types for it`
    );
  }
);

handler.on("v1.billing.meter.no_meter_found", handleNoMeterFound);
handler.on(
  "v1.billing.meter.error_report_triggered",
  handleErrorReportTriggered
);

app.post(
  "/webhook",
  express.raw({ type: "application/json" }),
  async (req, res) => {
    try {
      const sig = req.headers["stripe-signature"]?.[0] ?? "";
      handler.handle(req.body, sig);
      res.sendStatus(200);
    } catch (err) {
      console.log(`Webhook Error: ${(err as any).stack}`);
      res.status(400).send(`Webhook Error: ${(err as any).message}`);
    }
  }
);
```

This pattern is supported in all seven of our [server-side SDKs](https://docs.stripe.com/sdks/server-side), though the exact implementation details will differ between them. In each case, we designed a clean and idiomatic experience for the language.

## What comes next

While event notification handlers represent an exciting leap forward, we're not done yet. This more structured approach to event handling sets the stage for even better experiences going forward. While we don't have concrete plans yet, we're working on better validations (like ensuring your handler is listening for all the events your webhook endpoint is sending) and better integrations with popular web frameworks so we can make the integration experience even more seamless.

## How you can help

Ahead of their GA launch later this summer, **we're looking for developer feedback** on the handlers to ensure they're ready for prime time. Though we're confident in their overall design, there's no replacing adoption in a real production codebase. If you have a chance to try them, **please report any feedback** (good or bad) in your SDK's corresponding GitHub issue:

- [Node](https://github.com/stripe/stripe-node/issues/2763)
- [Python](https://github.com/stripe/stripe-python/issues/1835)
- [Java](https://github.com/stripe/stripe-java/issues/2239)
- [Go](https://github.com/stripe/stripe-go/issues/2379)
- [Ruby](https://github.com/stripe/stripe-ruby/issues/1895)
- [PHP](https://github.com/stripe/stripe-php/issues/2087)
- [.NET](https://github.com/stripe/stripe-dotnet/issues/3404)
