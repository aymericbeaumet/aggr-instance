---
title: 'From naive webhooks to durable sync: Queue-based and event-driven data reconciliation patterns'
link: https://stripe.dev/blog/database-reconciliation-growing-businesses-part-2
source: stripe-com
published: 2025-08-22T00:00:00Z
updated: 2025-08-22T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- James Beswick
summary: Learn advanced database reconciliation patterns for Stripe integration. Explore queue-based architectures, event-driven sync, and robust error handling to maintain product data consistency at scale.
content: extracted
html: 2025-08-22-from-naive-webhooks-to-durable-sync-queue-based-and-event.html
remote_preview:
  url: https://stripe.dev/og/blog/database-reconciliation-growing-businesses-part-2.png
  alt: 'Stripe Developer Blog: From naive webhooks to durable sync: Queue-based and event-driven data reconciliation patterns'
extra:
  content:interactive: true
---

In digital commerce, maintaining consistent product data across your systems and third-party platforms is critical yet challenging. Whether you're managing a handful of subscription products or orchestrating a catalog with thousands of SKUs, reconciliation failures can lead to pricing discrepancies, failed transactions, and ultimately, lost revenue. This series looks at this problem in detail.

This post dives into common reconciliation patterns that bridge the gap between your product catalog and Stripe's representation of the data. We'll explore both simple first implementations that may serve early-stage products and more robust architectures designed for scale and resilience. Through practical examples and architectural diagrams, you'll gain insights into creating systems that maintain data integrity even as your business grows and becomes more complex. By understanding these patterns, you can build synchronization mechanisms that silently but effectively keep your critical product data in harmony across systems.

## Common reconciliation scenarios

Let's examine two prevalent scenarios that software architects must address when reconciling product data with Stripe, exploring both naive first implementations and more robust approaches.

### Scenario 1: One-way synchronization from your system to Stripe

In this common pattern, your internal system serves as the single source of truth for product information, with changes propagating to Stripe whenever products or prices are updated. A naive implementation might look like this:

![](https://stripe.dev/images/database-reconciliation-growing-businesses-part-2/image3.png)

A typical first implementation might look like this:

1\. Product changes in your system trigger an event or webhook\
 2. This event is processed by a Lambda function or similar serverless worker\
 3. The function calls the Stripe API to create or update the product data\
 4. Stripe returns a 200 response, and the process considers the product successfully updated\
 5. The system moves on to the next change with no persistent record of the synchronization

The Lambda function code simply packages the events received by the function and calls the Stripe API:

```
const AWS = require('aws-sdk');
const stripe = require('stripe')('your-secret-key-here');

exports.handler = async (event) => {
    for (const record of event.Records) {
        const messageBody = JSON.parse(record.body);

        const { productId, updates } = messageBody;

        try {
            // Update the product in Stripe
            const updatedProduct = await stripe.products.update(productId, updates);
            console.log('Product updated successfully:', updatedProduct);
        } catch (error) {
            console.error('Error updating product:', error);
            // The message will remain in the queue for reprocessing if there's an error
        }
    }
};
```

This approach is straightforward and works well for low volumes, but has several critical flaws that emerge as scale increases:

If the Stripe API is temporarily unavailable due to outages or maintenance, the Lambda function might fail, and the change event is lost. There's no built-in mechanism to retry failed operations, leading to data drift between systems.

At scale, Lambda functions may be throttled as concurrent executions increase, especially during bulk product updates or catalog refreshes. This throttling can cause some functions to fail without processing their changes, resulting in data loss.

High-volume operations may exceed your Stripe API quota limits, causing requests to be rejected. Without proper rate limiting and backoff strategies, your system will continue to hit these limits, potentially triggering temporary bans or additional request failures.

The lack of observability makes it difficult to know which products failed to synchronize, leaving the actual state of reconciliation unknown and forcing time-consuming manual verification.

#### A more robust implementation

![](https://stripe.dev/images/database-reconciliation-growing-businesses-part-2/image2.png)

A more resilient approach introduces a queue-based architecture that addresses these limitations:

1\. Product changes are captured and stored durably in a message queue (for example, Amazon SQS, Apache Kafka)\
 2. A worker service processes the queue at a controlled, configurable rate\
 3. Items are only removed from the queue upon confirmed successful processing\
 4. Failed operations are automatically retried with exponential backoff\
 5. Processing rates are configured to respect Stripe API quota limits\
 6. Dead-letter queues capture persistently failing operations for human review

The Lambda function is modified to handle batches of messages, and then throw an error if there is a poison pill message in the batch:

```
const AWS = require('aws-sdk');
const stripe = require('stripe')('your-secret-key-here');

exports.handler = async (event) => {
    let allSuccessful = true; // Flag to track success of all updates
    
    for (const record of event.Records) {
        const messageBody = JSON.parse(record.body);
        const { productId, updates } = messageBody;

        // Try to update the product in Stripe
        try {
            const updatedProduct = await stripe.products.update(productId, updates);
            console.log('Product updated successfully:', updatedProduct);
        } catch (error) {
            console.error('Error updating product:', error);
            allSuccessful = false; // Mark update as failed
        }
    }

    // Return a success status if all updates were successful
    if (!allSuccessful) {
        throw new Error("One or more updates failed."); // This will cause the function to fail, keeping messages in the SQS queue
    }
};
```

This pattern introduces important guarantees. Changes are stored durably and won't be lost if Stripe is temporarily unavailable. The queue acts as a buffer that can absorb spikes in update volume and weather API outages without losing data. When Stripe becomes available again, processing resumes automatically.

Queue depth provides visibility into synchronization backlogs, allowing operations teams to monitor reconciliation health and detect issues before they become critical. Increasing queue depth can trigger alerts or scaling operations to address backlogs.

Throttling and retry logic can be implemented independently of your product service, creating a clean separation of concerns. The product service can focus on core business logic while the synchronization service handles the complexities of maintaining consistency with external systems.

Processing rates can be dynamically adjusted based on API quota availability, time of day, or business priorities. During critical sales periods, you might increase quota allocation to ensure product updates propagate quickly, while reverting to lower rates during maintenance windows.

### Scenario 2: Bidirectional synchronization

The more complex scenario involves changes originating from both your system and Stripe, requiring bidirectional reconciliation. This often occurs when some product attributes are managed in Stripe directly (perhaps by finance teams) while others are managed in your product information system.

#### Traditional ETL approach

Many organizations initially implement a periodic [ETL](https://en.wikipedia.org/wiki/Extract,_transform,_load) (Extract, Transform, Load) process:

1\. **Extract**: At scheduled intervals, data is pulled from both systems via SQL queries, database dumps, or API calls. This creates point-in-time snapshots of both product catalogs.

2\. **Transform**: The extraction data undergoes transformation processes including schema alignment (mapping fields between systems), data cleaning (fixing formatting inconsistencies), normalization (standardizing values), and aggregation (combining related records).

3\. **Load**: After transformation, a reconciliation engine identifies differences between the systems and applies updates to bring them into alignment. This typically involves a staging area where changes are prepared before being pushed to the target systems.

This approach can have significant limitations. It's periodic rather than real-time, creating windows where systems are known to be out of sync. Depending on the frequency of the ETL jobs, these windows could be hours or even days long, potentially impacting business operations.

Complex transformation logic is error-prone and difficult to maintain as both systems evolve over time. Each schema change in either system requires updates to the transformation rules, creating ongoing maintenance overhead.

Reconciliation rules become increasingly complicated as the business grows. How do you handle situations where the same data was changed in two places? Which system should win when there are conflicts? What happens if a product was updated in one system but deleted in the other? These edge cases multiply over time.

The process can become increasingly expensive and time-consuming as data volumes grow, potentially leading to longer reconciliation windows and more opportunities for conflicts to arise.

#### Event-driven approach

A more modern implementation uses [event-driven architecture](https://en.wikipedia.org/wiki/Event-driven_architecture) to maintain near real-time consistency:

![](https://stripe.dev/images/database-reconciliation-growing-businesses-part-2/image1.png)

1\. Configure Stripe Event Destinations to stream changes directly to your AWS account via an EventBridge partner event bus. This notifies your systems almost immediately when changes occur in Stripe.

2\. Integrate these webhooks with a durable message bus like SQS and EventBridge for reliable change processing. This combination ensures that events aren't lost even if processing services are temporarily unavailable.

3\. Implement similar event-emitting patterns from your internal systems when product data changes, creating symmetrical event flows from both sources.

4\. Process these events through dedicated synchronization services that apply updates to the opposite system, maintaining bidirectional consistency.

This approach offers significant advantages. Near real-time consistency is achieved as changes are propagated as they occur rather than waiting for batch windows. This reduces the reconciliation window from hours to seconds or minutes.

The event-driven model provides natural auditability of changes, making it easier to trace how and when data evolved in each system. This audit trail is invaluable for debugging and compliance purposes.

Change isolation means each update is processed independently, reducing the complexity of conflict resolution compared to batch processes where multiple changes must be reconciled simultaneously.

The architecture scales more gracefully as systems grow, with the ability to parallelize processing and dynamically adjust throughput based on load and priority.

## Conclusion

Reconciling product data between your systems and Stripe represents a critical architectural challenge that evolves as your business scales. We've explored two common synchronization patterns—one-way and bidirectional—and witnessed how naive implementations can quickly break down under real-world conditions of scale, outages, and competing updates.

The evolution from simple direct-call architectures to robust queue-based and event-driven systems highlights a fundamental principle: durable, observable processes are essential for maintaining data consistency across distributed systems. By implementing proper buffering mechanisms, retry logic, conflict resolution, and monitoring, you can create synchronization pipelines that gracefully handle the inevitable disruptions of distributed computing.

Remember that reconciliation isn't just a technical challenge but a business imperative. When product data falls out of sync, the consequences ripple through the entire customer experience. Investing in resilient synchronization architecture pays dividends in reliable operations and customer trust.

For more Stripe developer learning resources, subscribe to our [YouTube Channel](https://www.youtube.com/@StripeDev).
