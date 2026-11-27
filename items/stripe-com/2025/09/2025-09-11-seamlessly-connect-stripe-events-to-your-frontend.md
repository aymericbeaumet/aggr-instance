---
title: Seamlessly connect Stripe events to your frontend
link: https://stripe.dev/blog/seamlessly-connect-stripe-events-to-your-frontend
source: stripe-com
published: 2025-09-11T00:00:00Z
updated: 2025-09-11T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Josh Kahn
summary: Learn how to connect Stripe events to your frontend applications using real-time subscriptions, webhooks, and AWS services. Build responsive payment UIs that react instantly to transaction updates, subscription changes, and account modifications with event-driven architecture.
content: extracted
html: 2025-09-11-seamlessly-connect-stripe-events-to-your-frontend.html
remote_preview:
  url: https://stripe.dev/og/blog/seamlessly-connect-stripe-events-to-your-frontend.png
  alt: 'Stripe Developer Blog: Seamlessly connect Stripe events to your frontend'
extra:
  content:interactive: true
---

Every day, we navigate a world filled with asynchronous events. My coffee order is ready for pick-up. A package arrives at my front step. A payment succeeds. Events are occurrences or changes in state.

Airports are one of my favorite settings to think about events. As you walk through the terminal to your gate, numerous things are happening: an inbound aircraft lands, a gate changes, a flight is delayed. While these types of events have been around since the advent of commercial air travel, it is only in recent years that airlines have started to publish these events to travelers through mobile apps and websites. This transparency benefits both travelers and airlines alike.

Stripe makes extensive use of events to communicate changes in state to our users through [event destinations](https://docs.stripe.com/event-destinations). Event destinations enable Stripe users to receive real-time notifications of state changes across multiple channels, including webhook endpoints and [Amazon EventBridge](https://docs.stripe.com/event-destinations). This design allows users to build responsive applications that react promptly to payment status changes, subscription updates, or account modifications, minimizing latency between Stripe backend changes and the user interface.

Stripe events are available for many API resource changes, including:

- Successful or failed payment transactions
- Updates to existing subscriptions
- Modifications to connected account profiles, like business information or compliance status

Currently, Stripe supports two types of events: [snapshot (v1)](https://docs.stripe.com/api/events) and [thin (v2)](https://docs.stripe.com/api/v2/core/events) events. These two event types differ primarily in the information encoded within the event payload. Snapshot events carry a full copy of the resource state at the time of the event, whereas thin events include only a reference object, requiring additional API calls for complete details. Depending on filtering needs and system design, either type may be appropriate.

Similar to the airport scenario, updating user-facing applications (e.g., mobile apps, websites) when Stripe API resources change can significantly enhance user experience and operational efficiency. Imagine sellers receiving near-instant notifications confirming payments, or merchants building rich integrations with Stripe Terminal that inform employees when payment processing is underway. Real-time alerts could also notify users that an outbound payment has been successfully sent or a subscription has been paused.

In this blog post, we will explore approaches for connecting Stripe events received by your backend services to your frontend applications. We will delve into asynchronous, event-driven integrations that foster agility, responsiveness, and scalability across client-server interactions.

## Push or pull?

Integration between frontend applications and backend servers has traditionally been built using a synchronous request/response pattern. This approach is well understood and supported by mature technologies such as REST APIs. The client requests data, and the server responds immediately—a straightforward pull model. However, this method can become inefficient in highly dynamic systems.

Polling tends to generate excessive network traffic as the client repetitively asks the server for updates, even when no changes have occurred. This chatter increases latency and resource consumption on both ends. Polling frequency determines responsiveness, but increasing it can tax infrastructure, while decreasing it can make the app feel sluggish or outdated. Another subtle challenge lies in computing the delta — the difference between current and previous responses — which is necessary to update only changed data efficiently but can quickly become complicated as the application evolves.

A push or subscription model offers distinct advantages, especially within event-driven systems. Instead of endlessly querying for updates, clients subscribe to receive messages when relevant changes occur. More sophisticated subscription patterns enable clients to filter and receive only updates pertinent to their interests—for example, notifications tied to specific payment intents or account IDs.

![](https://stripe.dev/images/seamlessly-connect-stripe-events-to-your-frontend/image1.png)

While subscriptions mitigate many inefficiencies and improve the user experience with near-instant updates, they also introduce additional architectural complexity. Implementing robust subscription models often requires new technologies such as WebSockets, MQTT, or managed pub/sub services.

Depending on the nature of your workload — for instance, the frequency of state changes or scale of concurrent users — these trade-offs may or may not justify the complexity. Polling may still suffice for infrequent updates or simple applications.

In practice, real-world applications often use a hybrid model: a synchronous API request to load the initial or bulk state, complemented by an asynchronous subscription to receive incremental updates. Returning to the airport analogy, this is akin to checking the departures board once before your trip, then watching live updates for delays, gate changes, or boarding announcements as they happen.

## Connecting Stripe events to your frontend

Stripe supports two primary options for receiving events: webhook endpoints and Amazon EventBridge. Choosing between these options depends largely on your needs around resiliency, scalability, and integration into your existing system architecture.

- **Webhook Endpoints**: Stripe delivers HTTP POST requests to URL(s) you configure whenever specific events occur. You must handle retries, idempotency, security verification, and potential spikes in traffic.
- **Amazon EventBridge**: A serverless event bus service that can natively ingest Stripe events, route them, and integrate with various AWS services. EventBridge supports event filtering, transformation, and sophisticated routing patterns out of the box.

Regardless of the event ingestion method, a robust event-processing architecture must address key challenges such as:

- **Idempotency**: Ensuring that re-delivery of the same event does not cause duplicate processing or side effects.
- **Message ordering**: Preserving event sequence to maintain data consistency.
- **Duplication**: Detecting and discarding duplicate events.
- **Failure handling**: Graceful recovery from transient network or service failures.

Once your backend receives a Stripe event, several technical strategies are available to publish relevant information to your frontend clients. Managing these communication channels yourself can be complex, so many teams leverage managed services that offload service maintenance, scalability, and security concerns.

Examples of such managed services include:

- **AWS AppSync**: A managed GraphQL service that supports real-time subscriptions and integrates with various data sources.
- **Momento**: A real-time cache and event streaming platform designed to minimize latency for event-driven applications.

When selecting an approach, consider several key factors:

- **Messaging pattern**: Will your messages be sent to individual clients (peer-to-peer) or broadcast to many subscribers?
- **Message direction**: Is communication one-way (server to client) or bidirectional (client and server over the same connection)?
- **Latency requirements**: How quickly must updates reach your subscribers for an acceptable user experience?

The following table outlines some common communication patterns and technologies suitable for pushing or polling updates between frontend and backend applications, including latency expectations and sample implementations:

| Technology             | Model | Patterns Supported             | Latency        | Sample Implementations\*        |
| ---------------------- | ----- | ------------------------------ | -------------- | ------------------------------- |
| **WebSockets**         | Push  | Broadcast, P2P, One or Two-Way | Low            | AWS AppSync, Amazon API Gateway |
| **MQTT**               | Push  | Broadcast, P2P, Two-Way        | Low to Medium  | AWS IoT Core                    |
| **gRPC**               | Push  | Broadcast, P2P, Two-Way        | Very Low       | Google Pub/Sub, Momento Topics  |
| **Server-Sent Events** | Push  | Broadcast, P2P, One-Way        | Medium         | Express SSE, Django             |
| **Polling**            | Pull  | Broadcast, P2P, One-Way        | Medium to Long | Amazon API Gateway, Apigee      |

*Note that this table is non-exhaustive. Latencies and capabilities may vary based on implementation details, network conditions, and system architecture.*

Ultimately, your choice depends on your application's technical ecosystem and business needs. For example, if you publish events to Amazon EventBridge, it's possible to send messages directly using [AWS AppSync Events](https://docs.aws.amazon.com/appsync/latest/eventapi/event-api-welcome.html), enabling sophisticated user experiences without writing extensive custom connectors. Filtering and authorization are also differentiating features to investigate.

## Connecting card present payment events

Let’s return to the airport. During a long layover, you decide to visit an airport lounge but need to purchase a day pass for entry. The lounge integrates [Stripe Terminal](https://stripe.com/terminal) with their management software to enable visitors to buy access conveniently with credit cards.

The lounge’s management software uses a [server-side integration with Stripe Terminal](https://docs.stripe.com/terminal/payments/collect-card-payment?terminal-sdk-platform=server-driven) to facilitate payment collection. This pattern allows the server to orchestrate payment workflows and control the local Terminal reader, which accepts card-present payments securely.

The payment process begins by creating a new `PaymentIntent`:

```
let pi = await stripe.paymentIntents.create({
  amount: 5000, // one-time payment of $50 (in cents)
  currency: "usd",
  capture_method: "automatic",
  payment_method_types: [ "card_present" ],
});
```

Next, the lounge software instructs the Terminal reader to authorize and confirm the transaction represented by the `PaymentIntent`:

```
await stripe.terminal.readers.processPaymentIntent(
  "{{TERMINAL_READER_ID}}",
  {
    payment_intent: pi.id,
  }
);
```

At this point, the reader will light up, ready to accept a card-present payment. Meanwhile, Stripe publishes events to the configured event destinations, informing other parts of the system of state changes.

The lounge management software can take advantage of these events by publishing relevant portions to the frontend interface using the technologies described earlier. For instance, the software can instantly notify the lounge agent when the reader is ready for payment, indicated by the `payment_intent.requires_payment_method` event.

If the lounge software uses Amazon EventBridge as an event destination, it can take advantage of the native routing and filtering capabilities to route the event to AWS AppSync Events:

![](https://stripe.dev/images/seamlessly-connect-stripe-events-to-your-frontend/image2.png)

Though a full walkthrough of an EventBridge + AWS AppSync setup is beyond this post’s scope, here’s an abbreviated sketch of the solution:

1. Create an [event pattern](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-event-patterns.html) to match the `payment_intent.requires_payment_method` event:

```
{
  "source": [{
    "prefix": "aws.partner/stripe.com"
  }],
  "detail-type": ["payment_intent.requires_payment_method"]
}
```

2. Use an [EventBridge API destination](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-api-destinations.html) to publish a message to the AWS AppSync Events endpoint. The message structure is flexible; the payload might resemble:

```
{
  "channel": "payment/lounges/123",
  "events": [
    "{ \"event\": \"payment_required\", \"id\": \"<pid>\", \"amount\": \"<amount>\" }"
  ]
}
```

3. On the client side, subscribe to the AWS AppSync Events channel to receive and react to updates. For example, a React application could use the [AWS Amplify](https://docs.aws.amazon.com/amplify/) library:

```
await eventsClient.connect(`payments/lounges/${lounge_id}`).subscribe({
  next: (data) => {
    if (data.type === "requires_payment_method") {
    setReadyToCollect(true);
    }
  },
})
```

While the example above leverages AWS services, you could implement similar flows using any of the previously discussed technologies, trading off complexity, latency, and cost. For instance, you might configure a webhook event destination that publishes a message to a [Momento Topic](https://www.gomomento.com/platform/topics/) when payment data changes, enabling real-time frontend updates without AWS.

### Conclusion

Extending Stripe events to the frontend empowers you to build rich, responsive, and interactive applications powered by real-time data streams. With this foundation, retailers can notify merchants of individual sales moments after they occur, subscription platforms can instantly alert users to changes in billing status, and support teams can monitor account events as they happen to provide proactive service.

Consider additional real-world use cases such as:

- **Inventory Updates**: Automatically adjust stock levels shown on e-commerce frontends once payments settle.
- **Customer Support Dashboards**: Display real-time payment and subscription changes to customer success teams.
- **Analytics and Reporting**: Feed live metrics into dashboards for operational visibility.

By combining Stripe’s powerful event architecture with modern frontend technologies and subscription patterns, developers can deliver compelling experiences that delight users and drive business outcomes.

To learn more about developing applications with Stripe and unlock new possibilities with event-driven integrations, visit our [YouTube Channel](https://www.youtube.com/stripedevelopers). Or check out [my conversation with James Beswick](https://www.youtube.com/watch?v=DIsGgXfp7RI) on this topic.
