---
title: How do I store inventory data in my Stripe application
link: https://stripe.dev/blog/how-do-i-store-inventory-data-in-my-stripe-application
source: stripe-com
published: 2024-12-16T00:00:00Z
updated: 2024-12-16T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Ben Smith
summary: Explore the critical importance of managing real-time inventory updates for both online marketplaces and physical stores. This blog post details the development of an event-driven architecture designed to synchronize inventory levels with Stripe payment events using AWS cloud services. The demonstration centers around the DevRel Swag Store, showcasing a practical application used at the GOTO Chicago event, which integrates serverless technologies like Amazon DynamoDB, AWS Lambda, and IoT Core for efficient inventory management. Learn about the challenges of using Stripe metadata for inventory, and discover robust solutions for handling race conditions and implementing dynamic inventory checks. Additionally, understand the trade-offs between using Stripe Payment Links and custom payment processes for real-time stock validation.
content: extracted
html: 2024-12-16-how-do-i-store-inventory-data-in-my-stripe-application.html
remote_preview:
  url: https://stripe.dev/og/blog/how-do-i-store-inventory-data-in-my-stripe-application.png
  alt: 'Stripe Developer Blog: How do I store inventory data in my Stripe application'
---

Whether you're running a global online marketplace or operating a physical store, persisting and updating inventory quantities is critical to ensure accurate stock levels, prevent overselling, and maintain customer satisfaction by fulfilling orders promptly and reliably. This post walks through setting up an event-driven architecture that reacts to Stripe payment events, processes these events, updates inventory levels using AWS cloud services, and pushes changes to a frontend client in near real-time.

## Real-Time Inventory updates

The demo application for this post is the DevRel Swag Store, a real app used during the [GOTO Chicago technology event.](https://gotochgo.com/2024) Attendees could purchase swag by scanning a QR code from a headless frontend SPA (single page application). The QR code loads a [Stripe payment link](https://stripe.com/gb/payments/payment-links) page for the customer to complete the transaction. Each successful transaction triggers an update to the store's inventory numbers, the payment link is disabled once all the inventory for a product is sold. Additionally, inventory levels are reflected immediately in both the inventory management system (backend) and the SPA (frontend) for all customers to see.

When designing the inventory management system for the DevRel Swag Store, we faced a critical decision: whether to use [Stripe metadata](https://docs.stripe.com/api/metadata) for managing inventory data or to adopt a separate database solution. Each approach offered distinct advantages and challenges.

Stripe's metadata attribute enables you to store additional product data directly within Stripe. For example, you can embed inventory levels as metadata in each product object. Here’s a sample representation of a product using Stripe metadata:

```
{
  "id": "prod_ABC123",
  "name": "DevRel T-Shirt",
  "description": "High-quality cotton t-shirt for tech enthusiasts.",
  "metadata": {
    "inventory": "20",
    "size": "Large",
    "color": "Black"
  }
}
```

 Each time a purchase is completed, an event such as **checkout.session.completed** is posted to a backend application via a webhook endpoint. This backend application verifies the event and makes an API call to Stripe to update the metadata attribute, reflecting the new inventory level. If the new inventory level is zero, it makes an additional API call to disable the payment link to prevent future payments from being processed.

Since the application architecture [requires a backend to handle these events,](https://docs.google.com/document/u/0/d/14zjPjmONL5E4qxT7XY6C81qJVVonlrukqcXlWPwYrmo/edit) and call back to the Stripe API securely, we evaluated the necessity and efficiency of storing the inventory data within the backend infrastructure, and not within Stripe metadata.

## A Serverless Database for Inventory Management

Storing the inventory levels within the backend infrastructure eliminates the need for additional API calls to Stripe, simplifying our operational workflow. Given that the backend infrastructure was already running on AWS services, it was decided to use [Amazon DynamoDB](https://aws.amazon.com/dynamodb/) to persist product inventory data. Here’s an example of how we represented a product in DynamoDB in instead of using stripe metadata attributes:

```
{
  "PK": "prod_ABC123",
  "SK": "store_XYZ789",
  "name": "DevRel T-Shirt",
  "description": "High-quality cotton t-shirt for tech enthusiasts.",
  "inventory": 20,
  "attributes": {
    "size": "Large",
    "color": "Black",
    "supplier": "Local Merchandiser"
  }
}
```

DynamoDB provided additional capability to manage high transaction volumes, particularly for the burst of activity the store experiences during peak times. The final application architecture is made up of [Amazon EventBridge](https://aws.amazon.com/eventbridge/), [AWS Lambda](https://aws.amazon.com/lambda/), [Amazon DynamoDB](https://aws.amazon.com/dynamodb/), and [AWS IoT Core](https://aws.amazon.com/iot-core/), integrated with Stripe to handle payments. Each product in the store exists as a product object in Stripe with essential information such as a title, image, description and a corresponding price ID. All additional information, such as colors, sizes, long descriptions, tags, inventory numbers, and more, is stored in DynamoDB. The Stripe product ID is used as the mapping ID and stored in DynamoDB as the PK (Partition Key). The physical Store ID is used as the secondary key and stored in Dynamo DB as the SK (Sort Key).

![](https://stripe.dev/images/how-do-i-store-inventory-data-in-my-stripe-application/image1.png)

With the backend architecture decided, the next decision is how best to update inventory levels in real time. Since the architecture is running on serverless services in the AWS cloud, we are able to make use of [Stripe Event Destinations](https://docs.stripe.com/event-destinations), and publish events to EventBridge instead of standing up a webhook endpoint.

The following architecture diagram shows the data flow when a successful payment is taken:

![](https://stripe.dev/images/how-do-i-store-inventory-data-in-my-stripe-application/image4.png)

1. A Customer scans a QR code to load the Stripe payment link in their smartphone.
2. Customer purchases item using payment link.
3. A \`Checkout.session.complete\` event is raised, and published to Amazon EventBridge
4. The event is routed downstream to a Lambda function.
5. A Lambda function transforms the event payload, updates DynamoDB with the new inventory number, and publishes a message into an IoT Core topic.
6. The IoT Core topic pushes the message to the frontend application, where the new inventory level is updated for all customers to see.

## Handling race conditions

A key challenge in this application is managing race conditions, where simultaneous events try to update the inventory levels in DynamoDB at the same time, leading to potential data inconsistencies. To prevent this and ensure data integrity, especially in high-volume environments, we've implemented an atomic decremental counter on the backend. DynamoDB facilitates this by providing atomic operations through its \`UpdateItem\` feature, which allows us to precisely adjust inventory counts as a single, indivisible action. This means that even when multiple transactions occur simultaneously, each update to the inventory is processed in isolation, without interference from other operations. The use of condition expressions ensures that decrements occur only if sufficient stock is available, thereby maintaining consistency and preventing negative stock levels. This robust approach guarantees that inventory levels remain accurate and conflicts are avoided, even under intense transactional loads.

However, there is a short delay between the update in DynamoDB and the calls to the Stripe API to deactivate the payment link, both on Stripe's platform and on the frontend QR code. During this brief window, which might last a second or so, a customer could potentially complete a payment for an item that is no longer in stock. To automatically resolve this issue, we have implemented a post-payment validation process.

This post-payment check involves verifying the inventory immediately after a payment is completed. When a payment event is received, the backend system cross-references the available inventory levels using a \`checkout.session.completed\` event from Stripe. If the inventory levels are found to be zero or inadequate, indicating that the purchase cannot be fulfilled, we automatically initiate a refund for the transaction by making a request to the [Stripe refund API](https://docs.stripe.com/api/refunds). This approach ensures that customers are not charged for unavailable products and are promptly informed about the initiation of a refund.

![](https://stripe.dev/images/how-do-i-store-inventory-data-in-my-stripe-application/image2.png) Issuing refunds in this way can lead to losses from non-refundable transaction fees collected by Stripe. Payment Links do not natively allow for backend inventory checks to conditionally facilitate the payment based on stock availability. Therefore, they don't inherently support pre-payment stock checks unless used in conjunction with additional backend logic for post-payment validation and potential refunds if stock isn't available.

## Going a step further with dynamic inventory checks

As an alternative to Stripe Payment Links, a custom payment process allows the backend to verify stock levels before creating a Payment Intent using Stripe’s APIs directly.

When a customer initiates a purchase, the system's frontend sends product information and the desired quantity to an API endpoint on the backend. This endpoint queries the inventory data in DynamoDB. If the stock levels are sufficient, it responds affirmatively, allowing the customer to proceed toward payment. If inventory is insufficient, it immediately notifies the customer, preventing them from advancing in the checkout process and offering alternatives or waitlist options as appropriate.

Upon confirming inventory availability, the next step involves creating a Payment Intent using Stripe's API. In this implementation, the `capture_method` is set to `manual`. This means that the payment is authorized but not immediately captured, providing an additional safeguard against potential inventory discrepancies with a 7 day [authorisation validity window](https://docs.stripe.com/payments/place-a-hold-on-a-payment-method?locale=en-GB#authorization-validity-windows). Once the Payment Intent is established, the client secret is sent to the frontend of the application, which uses Stripe Elements to collect and securely handle customer card details. The frontend then confirms the Payment Intent with Stripe, facilitating the payment process on the customer’s side.

As payments are confirmed, the `payment_intent.succeeded` event is published. This event notifies the backend system of successful payments, at which point the inventory database is updated to reflect the new stock levels post-purchase. The final step is to manually capture the payment after a successful inventory update.

In this custom payment processing implementation, the chance of issues such as insufficient stock occurring between the inventory check and payment confirmation is greatly reduced. Additionally, should any issues occur, these are handled gracefully with customers promptly refunded, thus minimizing the risk associated with non-refundable transaction fees.

![](https://stripe.dev/images/how-do-i-store-inventory-data-in-my-stripe-application/image3.png) This robust and responsive payment infrastructure not only ensures accurate pre-payment inventory validation but also optimizes the purchasing experience, maintaining customer trust and satisfaction by dynamically handling inventory in real-time and mitigating the risk of overselling.

## Summary

Managing real-time inventory updates is crucial for maintaining accurate stock levels and ensuring a smooth customer experience. The DevRel Swag Store, created for the GOTO Chicago event, exemplifies a robust system where inventory changes are reflected in near real-time through an event-driven architecture, integrating AWS services and Stripe for payment processing.

The system's ability to update inventory plays a vital role in maintaining transparency with customers and preventing overselling. By using AWS and WebSocket connections, the store ensures that every purchase immediately impacts inventory visibility for all customers.

To address race conditions—where multiple transactions could simultaneously affect inventory levels—the application employs DynamoDB's atomic operations. This approach safeguards data integrity by handling updates as singular, consistent actions, even during high transaction volumes.

While Stripe Payment Links offer a straightforward solution for payments, a custom payment process allows for pre-payment inventory validation. This ensures that stock levels are confirmed before payment processing. If any discrepancies occur, automatic refunds are promptly issued using the Stripe refund API.

This architecture not only strengthens inventory reliability but also optimizes the purchasing experience, fostering trust and customer satisfaction in a fast-paced retail environment.

To learn more about developing applications with Stripe, [visit our YouTube Channel.](https://www.youtube.com/stripedevelopers)
