---
title: 'Building resilient webhook handlers in AWS: Implementing DLQs for Stripe events'
link: https://stripe.dev/blog/building-resilient-webhook-handlers-aws-dlqs-stripe-events
source: stripe-com
published: 2025-01-30T00:00:00Z
updated: 2025-01-30T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- James Beswick
summary: Discover how to build reliable webhook handlers for Stripe events using AWS in this comprehensive guide. Learn about the challenges of processing webhook events at scale and how to address them with an enterprise-grade architecture.
content: extracted
html: 2025-01-30-building-resilient-webhook-handlers-in-aws-implementing.html
remote_preview:
  url: https://stripe.dev/og/blog/building-resilient-webhook-handlers-aws-dlqs-stripe-events.png
  alt: 'Stripe Developer Blog: Building resilient webhook handlers in AWS: Implementing DLQs for Stripe events'
extra:
  content:interactive: true
---

Processing webhook events reliably at scale presents significant challenges for modern distributed systems. As businesses grow their payment processing operations with Stripe, the need for robust webhook handling becomes increasingly critical. In this post, we'll explore an enterprise-grade architecture for processing Stripe webhooks using AWS services, with particular attention to handling failures, implementing retry mechanisms, and maintaining consistent event ordering.

## Understanding the Webhook Reliability Challenge

When building systems that depend on webhooks for critical business processes like payment processing, several reliability challenges emerge. Network issues or service outages can result in lost webhook events, leading to inconsistencies between your system and Stripe's state. Events may arrive out of sequence due to network conditions or retry attempts, potentially causing race conditions and invalid state transitions. Additionally, Stripe's built-in retry mechanism can result in duplicate webhook deliveries, requiring careful handling to prevent double-processing of events.

Before diving into the implementation, it's important to understand how Stripe events work:

1. **Event Generation**: Stripe generates events for all actions in your account (payments, refunds, disputes, etc.)
2. **Delivery Guarantees**: Stripe attempts to deliver each event for up to 3 days with an exponential backoff.
3. **Ordering**: While events are generally delivered in order, network conditions can cause out-of-order delivery.
4. **Idempotency**: Each event has a unique ID that should be used to prevent double-processing.

Let's explore an approach that addresses these challenges using AWS services while adhering to [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/) principles.

## Architecture Overview

![](https://stripe.dev/images/building-resilient-webhook-handlers-aws-dlqs-stripe-events/1.png)

This solution uses the following AWS services:

- **API Gateway**: Webhook endpoint and request validation.
- **Amazon SQS FIFO Queues**: Ordered event processing and deduplication.
- **AWS Lambda**: Event processing with retry logic.
- **Amazon DynamoDB**: Idempotency tracking.
- **Amazon CloudWatch**: Monitoring and alerting.
- **Dead Letter Queues (SQS)**: Failed event handling.

[API Gateway](https://aws.amazon.com/api-gateway/) serves as the secure entry point for Stripe webhook events in this architecture, offering several critical features that make it ideal for this use case. Its request validation capabilities allow us to [verify Stripe's webhook signatures before processing](https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-use-lambda-authorizer.html), while its native integration with Lambda enables either direct event processing or forwarding to SQS.

API Gateway's built-in throttling protects our backend services from traffic spikes, and its CloudWatch integration provides detailed metrics about request patterns and latencies. The service's ability to handle high concurrent connections makes it suitable for webhook processing at scale, while features like custom domain names and TLS termination ensure secure communication with Stripe's servers.

You can also use API Gateway's resource policies to restrict incoming traffic to Stripe's IP ranges, adding an extra layer of security. When combined with [AWS WAF](https://aws.amazon.com/waf/), you can implement additional protection against common web exploits. The service's integration with [AWS X-Ray](https://aws.amazon.com/xray/) enables detailed request tracing, making it easier to debug issues in the webhook processing pipeline. From a cost perspective, API Gateway's pay-per-use pricing model aligns well with webhook processing's event-driven nature.

## Implementation Details

The infrastructure can be defined in an infrastructure-as-code (IaC) tool, such as CloudFormation. The following template snippet defines the SQS FIFO queue, the dead-letter queue (DLQ), and DynamoDB table for idempotency tracking:

```
AWSTemplateFormatVersion: '2010-09-09'
Description: 'Stripe Webhook Handler Infrastructure'

Resources:
  # FIFO Queue for ordered event processing
  StripeEventQueue:
    Type: AWS::SQS::Queue
    Properties:
      QueueName: stripe-events.fifo
      FifoQueue: true
      ContentBasedDeduplication: true
      DeduplicationScope: messageGroup
      FifoThroughputLimit: perMessageGroupId
      VisibilityTimeout: 300
      RedrivePolicy:
        deadLetterTargetArn: !GetAtt StripeEventDLQ.Arn
        maxReceiveCount: 3

  # Dead Letter Queue for failed events
  StripeEventDLQ:
    Type: AWS::SQS::Queue
    Properties:
      QueueName: stripe-events-dlq.fifo
      FifoQueue: true
      ContentBasedDeduplication: true

  # DynamoDB table for idempotency tracking
  IdempotencyTable:
    Type: AWS::DynamoDB::Table
    Properties:
      TableName: stripe-idempotency
      AttributeDefinitions:
        - AttributeName: event_id
          AttributeType: S
      KeySchema:
        - AttributeName: event_id
          KeyType: HASH
      BillingMode: PAY_PER_REQUEST
      TimeToLiveSpecification:
        AttributeName: ttl
        Enabled: true
```

This architecture uses SQS FIFO queues to maintain event ordering while providing message deduplication capabilities. The DLQ captures failed processing attempts for analysis and replay. DynamoDB serves as our idempotency store, with automatic cleanup through time-to-live (TTL) to manage storage costs. This means that the DynamoDB service will automatically delete items once their age passes the TTL defined.

The webhook processing logic is implemented through a Lambda function that handles the core event processing. This function parses events from SQS, checks idempotency IDs against the DynamoDB table store, then processes the event and updates the idempotency ID store if the event is new.

```
import json
import os
import time
from datetime import datetime, timedelta
import boto3
import stripe
from botocore.exceptions import ClientError

dynamodb = boto3.resource('dynamodb')
idempotency_table = dynamodb.Table(os.environ['IDEMPOTENCY_TABLE'])

def lambda_handler(event, context):
    for record in event['Records']:
        stripe_event = json.loads(record['body'])
        
        if not is_duplicate_event(stripe_event['id']):
            try:
                process_stripe_event(stripe_event)
                store_processed_event(stripe_event['id'])
            except Exception as e:
                print(f"Error processing event {stripe_event['id']}: {str(e)}")
                raise e
                
def is_duplicate_event(event_id):
    try:
        response = idempotency_table.get_item(
            Key={'event_id': event_id}
        )
        return 'Item' in response
    except ClientError as e:
        print(f"Error checking idempotency: {str(e)}")
        # In case of error, assume not duplicate to ensure processing
        return False

def store_processed_event(event_id):
    ttl = int((datetime.now() + timedelta(days=7)).timestamp())
    try:
        idempotency_table.put_item(
            Item={
                'event_id': event_id,
                'processed_at': int(time.time()),
                'ttl': ttl
            }
        )
    except ClientError as e:
        print(f"Error storing processed event: {str(e)}")
        raise e

def process_stripe_event(stripe_event):
    event_type = stripe_event['type']
    
    if event_type.startswith('payment_intent'):
        handle_payment_intent_event(stripe_event)
    elif event_type.startswith('charge'):
        handle_charge_event(stripe_event)
    elif event_type.startswith('invoice'):
        handle_invoice_event(stripe_event)
    elif event_type.startswith('subscription'):
        handle_subscription_event(stripe_event)
    elif event_type.startswith('refund'):
        handle_refund_event(stripe_event)
    else:
        print(f"Unhandled event type: {event_type}")
```

The Lambda service by default implements sophisticated retry logic with exponential backoff to handle transient failures gracefully. When processing fails, the system implements a graduated retry strategy, doubling with each attempt up to a maximum of three retries. This exponential backoff helps prevent system overload during recovery while maximizing the chances of successful processing.

By defining a [dead-letter queue](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-properties-lambda-function-deadletterconfig.html) in the CloudFormation template, the Lambda function failed events are automatically routed to the DLQ after multiple retry attempts, ensuring no events are lost while preventing infinite retry loops. This combination of Lambda's built-in retry capabilities and the custom backoff strategy provides robust handling of transient failures while maintaining system stability.

## Managing Idempotency with DynamoDB

DynamoDB serves as an ideal idempotency store for webhook processing due to its consistent single-digit millisecond performance at scale and built-in TTL capabilities. We store each processed event ID as a partition key, along with metadata such as processing timestamp and outcome. The TTL attribute automatically removes old records, typically after 24-72 hours, preventing table growth while maintaining a sufficient window for redelivery detection.

For high-volume systems processing millions of events daily, you can use DynamoDB's on-demand capacity mode to handle spiky workloads without pre-provisioning. One important pattern is implementing conditional writes with DynamoDB's atomic operations - attempting to insert the event ID with a condition that it doesn't exist ensures thread-safe deduplication even under concurrent processing conditions.

## Monitoring and Alerting Strategy

A comprehensive monitoring strategy ensures operational visibility and helps maintain a quick response to issues. This CloudFormation definition can set up an alarm in Amazon CloudWatch for when messages appear in the DLQ:

```
DLQAlarm:  
  Type: AWS::CloudWatch::Alarm  
  Properties:  
    AlarmName: StripeWebhookDLQNotEmpty  
    AlarmDescription: Alert when messages appear in DLQ  
    MetricName: ApproximateNumberOfMessagesVisible  
    Namespace: AWS/SQS  
    Statistic: Sum  
    Period: 300  
    EvaluationPeriods: 1  
    Threshold: 0  
    ComparisonOperator: GreaterThanThreshold  
    Dimensions:  
      \- Name: QueueName  
        Value: stripe-events-dlq.fifo  
    AlarmActions:  
      \- \!Ref AlertingTopic
```

CloudWatch is a powerful tool for observing the state of the webhook processing application. You can monitor processing latency, error rates, and queue depths to provide early warning of potential issues, and custom metrics can track business-specific success rates and processing patterns. What you choose to monitor will be determined by your use-case, and also any cost limitations.

## Implementing Regional Failover

For global applications requiring very high availability, implementing regional failover capabilities requires careful consideration of several factors. The secondary region maintains parallel infrastructure, ready to handle traffic if the primary region experiences issues, but this comes with additional complexity and costs. When using VPCs, you may need to implement cross-region VPC connectivity through VPC peering or AWS Transit Gateway, ensuring secure communication between regions while managing the associated data transfer costs.

[DynamoDB Global Tables](https://aws.amazon.com/dynamodb/global-tables/) provide consistent idempotency checking across regions, but they introduce additional latency for writes which must be replicated across regions. This replication also incurs costs for both the data transfer and the additional write capacity needed in each region. The multi-region deployment significantly impacts the total cost of the solution: you'll pay for redundant infrastructure in each region (including API Gateway endpoints, Lambda executions, and SQS queues), cross-region data transfer, and Global Tables replication.

Route 53 health checks enable automatic failover when needed, but proper testing is crucial to ensure failover behavior works as expected under various failure conditions. While this level of redundancy provides excellent availability, many applications may not require this complexity—in single-region implementations, Stripe will continue to retry webhook delivery during AWS regional outages, often providing sufficient reliability for most use cases. Before implementing cross-region failover, carefully evaluate your actual availability requirements against the operational complexity and cost implications of a multi-region architecture.

## Performance Testing and Operational Considerations

Before deploying to production, thorough performance testing validates the system's behavior under various conditions. Using tools like [Apache JMeter](https://jmeter.apache.org/) or [Distributed Load Testing on AWS](https://aws.amazon.com/solutions/implementations/distributed-load-testing-on-aws/), you can simulate different webhook delivery patterns including steady-state load, sudden traffic spikes, and component failure scenarios. The architecture can handle millions of events daily, with SQS FIFO queues processing up to 300 messages per second with batching, or 3,000 messages per second per message group ID.

## Conclusion

Building reliable webhook handlers in AWS requires careful attention to event ordering, idempotency, and error handling. The architecture presented here provides a robust foundation for processing Stripe webhooks at scale while maintaining data consistency and operational excellence. Through comprehensive monitoring, regional failover capabilities, and careful attention to performance characteristics, this solution supports enterprise-grade webhook processing needs while remaining cost-effective and maintainable.

Remember to test thoroughly, especially failure scenarios, and maintain comprehensive monitoring of your webhook processing pipeline in production. The combination of SQS FIFO queues, Lambda, and DynamoDB provides a scalable and reliable solution for webhook processing that can grow with your business needs.

For more Stripe learning resources, subscribe to our [YouTube channel](https://www.youtube.com/stripedevelopers).
