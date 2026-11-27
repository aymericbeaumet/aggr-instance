---
title: Using an AWS microservice architecture for subscription management
link: https://stripe.dev/blog/aws-microservice-architecture-subscription-management
source: stripe-com
published: 2025-04-08T00:00:00Z
updated: 2025-04-08T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- James Beswick
summary: Explore the complexities of enterprise subscription management and discover how an AWS microservice architecture can streamline subscription lifecycles. This guide uses AWS services to create a scalable, reliable, and auditable subscription management system, addressing challenges like mid-cycle plan changes and usage-based billing adjustments.
content: extracted
html: 2025-04-08-using-an-aws-microservice-architecture-for-subscription.html
remote_preview:
  url: https://stripe.dev/og/blog/aws-microservice-architecture-subscription-management.png
  alt: 'Stripe Developer Blog: Using an AWS microservice architecture for subscription management'
extra:
  content:interactive: true
---

Managing subscription lifecycles in enterprise applications presents unique challenges that go well beyond simple recurring billing. As organizations scale, they need robust systems that can handle complex scenarios like mid-cycle plan changes, usage-based billing adjustments, and synchronized updates across multiple services. In this post, we'll explore a sample architecture that uses AWS services to create a reliable and scalable subscription management system.

## The challenge of enterprise subscription management

Enterprise subscription management is fundamentally different from basic recurring billing. Consider a scenario where a customer wants to upgrade their plan mid-billing cycle. This seemingly simple operation triggers a cascade of requirements:

- The billing system needs to calculate prorated charges for both the old and new plans.
- All associated services need to be updated to reflect new entitlements.
- The change needs to be atomic - either all services update successfully, or none do.
- The system must handle failed operations gracefully and maintain data consistency.
- All operations must be auditable for compliance and debugging.

Traditional monolithic approaches to subscription management often struggle with these requirements. They typically involve complex state management within a single application, often buried in code, making it difficult to maintain consistency across services and handle failures gracefully.

## Choosing the right services

This sample architecture presents a general pattern that uses several AWS services, each chosen for specific capabilities that address different aspects of the problem:

### Amazon EventBridge

[EventBridge](https://aws.amazon.com/eventbridge/) serves as the backbone of our event-driven architecture. It provides reliable event delivery with at-least-once processing guarantees, making it ideal for handling subscription-related events. We use it to decouple our subscription management logic from the actual service updates, allowing for better scalability and maintainability.

EventBridge's key features that make it suitable for this architecture include content-based filtering for routing different types of subscription events, dead-letter queues for handling failed event processing, and integration with AWS Step Functions for workflow orchestration.

### AWS Step Functions

[Step Functions](https://aws.amazon.com/step-functions/) manages the complex workflows involved in subscription changes. It's particularly valuable because it handles state management automatically, and supports long-running operations (up to 1 year), which is particularly useful for payment operations. It offers built-in error handling and retry mechanisms and maintains a detailed execution history for auditing.

### Amazon DynamoDB

[DynamoDB](https://aws.amazon.com/dynamodb/) stores subscription state and metadata. Its choice is driven by several factors, including consistent single-digit millisecond latency, automatic scaling to handle varying loads, and [point-in-time recovery](https://en.wikipedia.org/wiki/Point-in-time_recovery) for data protection. Additionally, it offers strong integration with other AWS services.

### AWS Lambda

[Lambda](https://aws.amazon.com/lambda/) functions handle the actual business logic and integration with external services like Stripe. They're ideal for this architecture because they scale automatically with demand, they support multiple runtime environments, and the stateless nature can simplify debugging and error handling.

## Architecture overview

The architecture follows an event-driven pattern where subscription changes trigger a series of coordinated updates across multiple services. Here's how the components work together:

![](https://stripe.dev/images/aws-microservice-architecture-subscription-management/image1.png)

When a subscription change is initiated, an event is published to EventBridge. It appears in the SaaS event bus in the configured region of your AWS account. This event includes details about the change (e.g., plan upgrade, downgrade, cancellation) and any relevant metadata. EventBridge routes this event to a Step Functions workflow based on event rules.

The Step Functions workflow orchestrates the entire process, which typically includes:

1. Validating the requested change.
2. Using a Lambda function to calculate prorated charges using Stripe's API.
3. Updating the subscription in Stripe.
4. Updating the internal subscription state in DynamoDB.
5. Triggering service-specific updates through Lambda functions.
6. Handling any compensation actions if steps fail.

### Event structure and routing

Events from Stripe in this system follow a consistent structure, for example:

```
{
  "version": "1.0",
  "id": "evt_123",
  "detail-type": "SubscriptionUpdate",
  "source": "subscription.api",
  "account": "123456789012",
  "time": "2025-02-04T19:52:00Z",
  "region": "us-east-1",
  "detail": {
    "subscriptionId": "sub_123",
    "operation": "upgrade",
    "fromPlan": "business",
    "toPlan": "enterprise",
    "effectiveDate": "2025-02-04T19:52:00Z"
  }
}
```

You can configure EventBridge rules on the event bus to route these events based on the `detail-type` and `operation` attributes. For example, a rule filtering on upgrades and downgrades can be defined as:

```
{
  "detail-type": ["SubscriptionUpdate"],
  "source": ["subscription.api"],
  "detail": {
    "operation": ["upgrade", "downgrade"]
  }
}
```

### Step Functions workflow

The Step Functions workflow handles the complex orchestration of subscription changes. While the exact workflow for your application will vary, here are some the important states defined for this use case:

```
{
  "StartAt": "ValidateChange",
  "States": {
    "ValidateChange": {
      "Type": "Task",
      "Resource": "arn:aws:lambda:REGION:ACCOUNT:function:validate-subscription-change",
      "Next": "CalculateProration",
      "Catch": [{
        "ErrorEquals": ["ValidationError"],
        "Next": "HandleValidationError"
      }]
    },
    "CalculateProration": {
      "Type": "Task",
      "Resource": "arn:aws:lambda:REGION:ACCOUNT:function:calculate-proration",
      "Next": "UpdateStripeSubscription"
    },
    "UpdateStripeSubscription": {
      "Type": "Task",
      "Resource": "arn:aws:lambda:REGION:ACCOUNT:function:update-stripe-subscription",
      "Next": "UpdateDynamoDB",
      "Retry": [{
        "ErrorEquals": ["StripeTemporaryError"],
        "IntervalSeconds": 1,
        "MaxAttempts": 3,
        "BackoffRate": 2.0
      }]
    }
    // Additional states omitted for brevity
  }
}
```

These states invoke Lambda functions that run the business logic and define the next steps in the process. The `UpdateStripeSubscribe` state encapsulates complex error handling logic without needing custom code.

The retry configuration is designed this way for several important reasons. First, we specifically catch `StripeTemporaryError` rather than all errors because we want to differentiate between transient failures (like network timeouts or rate limits) and permanent failures (like invalid card numbers). The initial retry interval is set to 1 second with a backoff rate of 2.0, meaning subsequent retries will wait 2 seconds, then 4 seconds. This exponential backoff prevents overwhelming downstream systems while still maintaining responsiveness for the customer. We limit to 3 attempts because Stripe's API typically recovers quickly from transient issues, and waiting longer would degrade the user experience.

If all retries fail, the error is propagated to the main error handler which can initiate compensation transactions to maintain system consistency. This is particularly important because Stripe operations are financial transactions – we need to ensure we don't double-charge customers or leave the system in an inconsistent state.

### Amazon DynamoDB data model

The DynamoDB table design uses a composite key structure to efficiently support various access patterns. A composite key in DynamoDB consists of two elements: a Partition Key (PK) and a Sort Key (SK):

```
{
  PK: "SUB#sub_123",  // Partition key
  SK: "META#current", // Sort key
  subscriptionId: "sub_123",
  customerId: "cust_456",
  plan: "enterprise",
  status: "active",
  effectiveDate: "2025-02-04T19:52:00Z",
  features: {
    userLimit: 1000,
    storageLimit: "5TB"
  },
  // Additional metadata
}
```

This Lambda function can use DynamoDB transactions to maintain consistency when updating subscription state:

```
const params = {
  TransactItems: [
    {
      Update: {
        TableName: "Subscriptions",
        Key: { PK: "SUB#sub_123", SK: "META#current" },
        UpdateExpression: "SET #plan = :plan, #status = :status",
        ExpressionAttributeNames: {
          "#plan": "plan",
          "#status": "status"
        },
        ExpressionAttributeValues: {
          ":plan": "enterprise",
          ":status": "active"
        },
        ConditionExpression: "attribute_exists(PK)"
      }
    },
    {
      Put: {
        TableName: "Subscriptions",
        Item: {
          PK: "SUB#sub_123",
          SK: "HISTORY#2025-03-04T19:52:00Z",
          // Historical record details
        }
      }
    }
  ]
};
```

### Stripe integration

The Lambda function handling the Stripe integration calls out to the Stripe API and also includes error handling and idempotency:

```
const stripe = require('stripe')(process.env.STRIPE_SECRET_KEY);

exports.handler = async (event) => {
  const idempotencyKey = event.detail.id;
  
  try {
    const subscription = await stripe.subscriptions.update(
      event.detail.subscriptionId,
      {
        proration_behavior: 'always_invoice',
        items: [{
          id: event.detail.subscriptionItemId,
          price: event.detail.newPriceId
        }]
      },
      {
        idempotencyKey
      }
    );
    
    return {
      statusCode: 200,
      body: JSON.stringify(subscription)
    };
  } catch (error) {
    if (error.type === 'StripeInvalidRequestError') {
      throw new Error('ValidationError');
    }
    throw error;
  }
};
```

An architecture using this combination of Step Functions and Lambda can include several layers of error handling. Step Functions has retry logic for transient failures, and workflows can route to dead-letter queues for failed events. For failed states, you can design compensation workflows for rolling back partial changes. Additionally, you can use [Amazon CloudWatch](https://aws.amazon.com/cloudwatch/) for detailed logging and monitoring.

### Conclusion

This sample architecture shows a general pattern for managing complex subscription scenarios in enterprise applications. By using AWS services like EventBridge and Step Functions, you can create a system that is reliable, using built-in retry mechanisms and error handling, and maintainable, providing clear separation of concerns and modular design. The comprehensive logging and execution history can also help with auditability.

For teams building subscription management systems, this architecture provides a proven pattern that can be adapted to specific business needs while maintaining the rigor required for handling financial transactions and service provisioning.

The combination of event-driven architecture and workflow orchestration provides a flexible foundation that can be extended to handle additional requirements as your subscription management needs evolve. This can help you avoid updating old “spaghetti code” with encoded business logic.

For more information about the services used in this architecture, refer to:

- [AWS Step Functions Developer Guide](https://docs.aws.amazon.com/step-functions/latest/dg/welcome.html)
- [Amazon EventBridge User Guide](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-what-is.html)
- [Stripe API Documentation](https://stripe.com/docs/api)
- [DynamoDB Developer Guide](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Introduction.html)

For more Stripe developer learning resources, subscribe to our [YouTube Channel](https://www.youtube.com/@StripeDev).
