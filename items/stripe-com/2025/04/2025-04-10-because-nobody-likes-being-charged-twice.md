---
title: Because nobody likes being charged twice
link: https://stripe.dev/blog/because-nobody-likes-being-charged-twice
source: stripe-com
published: 2025-04-10T00:00:00Z
updated: 2025-04-10T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Ben Smith
summary: In complex, high-volume systems, even minor failures—like a dropped internet connection—can lead to major headaches, such as duplicate charges. This post explores advanced patterns for integrating Stripe into your enterprise applications with a focus on building fault-tolerant, user-friendly payment systems. Learn how strategies like idempotency and message queues can protect your users from double charges, reduce operational errors, and improve reliability as your system scales.
content: extracted
html: 2025-04-10-because-nobody-likes-being-charged-twice.html
remote_preview:
  url: https://stripe.dev/og/blog/because-nobody-likes-being-charged-twice.png
  alt: 'Stripe Developer Blog: Because nobody likes being charged twice'
---

Imagine a customer checks out on your eCommerce site, enters their payment details, and chooses the "Pay" button. Just as the request is processed, their internet connection drops. They refresh the page and try again. Meanwhile, your backend is retrying the original request due to a timeout. The result? The customer gets charged twice.

Duplicate charges lead to refund requests, customer support headaches, and loss of trust. These issues not only hurt your revenue but also damage the customer experience. Customers expect smooth, seamless transactions, and even the smallest hiccup can lead to frustration.

This post explores how to design resilient payment systems that prevent duplicate charges, ensuring your system can handle interruptions, retries, and failures without compromising user trust. By implementing strategies like [**queues**](https://en.wikipedia.org/wiki/Message_queue) and [**idempotency**](https://en.wikipedia.org/wiki/Idempotence), you can protect your business from these issues, improve reliability, and ultimately deliver a better experience for your customers.

![](https://stripe.dev/images/because-nobody-likes-being-charged-twice/image1b.png)

*Retrying failed payments without using idempotency keys*

### **Everything fails all the time**

In distributed systems, failure is not an edge case, it’s the norm. Network timeouts, server crashes, database locks, downstream API errors, and even user interruptions (like closing a browser tab mid-transaction) are all part of the operational landscape.

When building a payment system, each of these failure modes has the potential to leave transactions in an uncertain state, leading to lost revenue, duplicate charges, or degraded user trust. Designing for resilience means anticipating these failures and building systems that can tolerate them gracefully. This is where techniques like message queues and idempotency keys become essential. They allow your architecture to retry operations safely, without unwanted side effects or inconsistencies.

The good news is that you can increase reliability by using **idempotency** and **message queues**.

## **Using idempotency keys**

Idempotency is a concept from mathematics and computer science that refers to the property of an operation where performing it multiple times with the same input results in the same outcome, without causing any side effects. In other words, no matter how many times you repeat the operation, the result will be the same as the first execution, and no additional changes or effects will occur after the initial action. In Stripe, an [idempotency key](https://docs.stripe.com/error-low-level#sending-idempotency-keys) is a unique identifier that ensures repeated requests result in the same action being performed only once.

### **How it works**

1. Before making a payment request, generate a unique idempotency key (e.g., a UUID).
2. Include this key in your payment request to Stripe.
3. Stripe stores the key and ensures that any subsequent request with the same key returns the same response instead of creating a new charge.

![](https://stripe.dev/images/because-nobody-likes-being-charged-twice/image1a.png)

*Retrying failed payments with Idempotency keys*

### **Why It works**

Idempotency ensures that if a request is retried (either by the client or your backend), it won't result in multiple charges. Stripe recognizes the idempotency key for 24 hours and returns the original response instead of processing a new transaction.

## **Using message queues**

While idempotency prevents duplicate charges at the payment provider level, message queues ensure your system handles payments reliably even during failures.

### **How it works**

1. When a customer submits a payment request, instead of processing it immediately, enqueue it in a message queue (e.g., [RabbitMQ](https://www.rabbitmq.com/), [Amazon SQS](https://aws.amazon.com/sqs/), [Kafka](https://kafka.apache.org/)).
2. A background worker (or pool of workers) consumes messages from the queue, ensuring payments are processed reliably and independently of user traffic.
3. If payment processing fails (e.g., due to a network error or Stripe outage), the message remains in the queue and is retried automatically after a delay.
4. If a message repeatedly fails after the maximum retry attempts, it can be sent to a [dead-letter queue (DLQ)](https://aws.amazon.com/what-is/dead-letter-queue/) for inspection and manual recovery.

### **Example workflow:**

1. The user chooses “Pay” and the request is added to the queue.
2. The worker processes payment using the idempotency key.
3. If the payment is successful, the message is removed from the queue.
4. If the payment processing fails the message is placed back onto the queue and retried later.

If the [AWS Lambda](https://aws.amazon.com/lambda/) function times out while processing the message or encounters an error, Amazon SQS automatically returns the message to the queue after a [visibility timeout,](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-visibility-timeout.html) allowing for a retry attempt. The message will be retried up to the configured maximum retries before being sent to a [dead-letter queue (DLQ)](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-dead-letter-queues.html) for further inspection.

![](https://stripe.dev/images/because-nobody-likes-being-charged-twice/image3.png)

*Adding resilience to payment workflows with Amazon SQS queues.*

### **Why it works**

Queues decouple the payment request from real-time processing, ensuring that even if the payment gateway is temporarily down, the request isn’t lost. When combined with idempotency keys, it prevents duplicate charges when retries happen.

**Common pitfalls**

While idempotency keys are a powerful safeguard against duplicate charges, they’re not foolproof unless used correctly. One common mistake is reusing the same key across different operations or users. An idempotency key should uniquely identify one specific action, such as “create a payment for Order #123”, not a general session or API call. Using the same key for different users or operations can lead to unexpected behavior, like retrieving someone else’s payment response.

Another pitfall is generating the idempotency key too early in the flow. For example, at application startup or when the user loads the page. If the user later modifies their order before checking out, that stale key could bind the new request to an old, now-incorrect response. It’s best to generate the key as close as possible to the point of execution, ideally just before placing the payment request into a queue or sending it to Stripe.

Some developers also forget to include the idempotency key on retries—especially when retry logic is handled at the infrastructure layer (like a queue worker or a load balancer). In those cases, retries without the original key are treated as entirely new requests, negating the benefits of idempotency and potentially duplicating charges.

Lastly, relying solely on idempotency without queues can leave your system vulnerable. If Stripe is temporarily unavailable and your frontend doesn’t retry, or your backend drops the request, there’s no second chance. Queues ensure that the operation gets retried, and idempotency ensures that retry is safe.

**Real-world use case**

At Stripe, we often see high-growth platforms and marketplaces implement resilient payment architectures using a combination of queues and idempotency. Take the fictional example of a developer tooling company that runs an eCommerce store for conference merchandise. During peak traffic, like during a keynote announcement or swag drop, the team must ensure payments are processed reliably, even if mobile connections are flaky or frontend requests time out.

To handle this, they introduce an SQS message queue. Each time a customer clicks “Pay,” the frontend sends a request to their backend, which generates a [UUID](https://en.wikipedia.org/wiki/Universally_unique_identifier) as the idempotency key and enqueues the payment job. A worker service (a Lambda function) then picks up that job, calls the Stripe API using the key in the `Idempotency-Key` header, and confirms the PaymentIntent.

If the worker crashes mid-request or Stripe returns a transient error, the message is returned to the queue. When retried, it reuses the same idempotency key, ensuring that only one PaymentIntent is ever created, no matter how many times the worker retries.

This pattern gives durability, safety, and observability. And most importantly, it avoided the scenario of double-charging a conference attendee while they were still standing at the checkout booth.

**Conclusion**

Building a resilient payment system is crucial for both operational reliability and customer trust. By incorporating idempotency keys and message queues into your payment flow, you can ensure that even in the event of temporary failures, payments are processed correctly without duplication. These techniques provide a safety net that not only prevents costly mistakes but also enhances the user experience by making transactions more reliable.

As your business scales, having a resilient system in place will reduce support overhead, prevent chargebacks, and provide confidence that your payment infrastructure can handle both expected and unexpected disruptions. Implementing these practices is an investment in the long-term health of your platform, ensuring that customers can trust your payment system to handle their transactions safely.

For more Stripe developer learning resources, subscribe to our [YouTube Channel](https://www.youtube.com/@StripeDev).
