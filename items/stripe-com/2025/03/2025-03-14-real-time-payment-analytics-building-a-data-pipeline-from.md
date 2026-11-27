---
title: 'Real-time payment analytics: Building a data pipeline from Stripe to AWS'
link: https://stripe.dev/blog/real-time-payment-analytics-stripe-to-aws-data-pipeline
source: stripe-com
published: 2025-03-14T00:00:00Z
updated: 2025-03-14T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- James Beswick
summary: Discover how to build a scalable real-time payment analytics pipeline from Stripe to AWS. This guide explores the challenges, architectural components, and implementation details to help businesses monitor transactions, enhance security, and gain insights into customer behavior.
content: extracted
html: 2025-03-14-real-time-payment-analytics-building-a-data-pipeline-from.html
remote_preview:
  url: https://stripe.dev/og/blog/real-time-payment-analytics-stripe-to-aws-data-pipeline.png
  alt: 'Stripe Developer Blog: Real-time payment analytics: Building a data pipeline from Stripe to AWS'
extra:
  content:interactive: true
---

In today’s fast-paced business environment, building a data pipeline from Stripe to AWS for real-time payment analytics offers a wealth of opportunities. One of the primary advantages is the ability to monitor transactions as they occur, enabling businesses to swiftly identify issues such as fraud, chargebacks, and disputes. This not only enhances security but also minimizes financial losses.

By analyzing payment data in real-time, companies can gain deep insights into customer behavior and preferences, allowing for targeted marketing strategies and personalized offers. This proactive approach can lead to increased customer engagement and loyalty. Additionally, real-time analytics facilitates accurate financial reporting and forecasting, empowering businesses to make informed decisions regarding resource allocation and budget planning.

The capability to predict churn using real-time payment trends allows companies to implement effective retention strategies. Moreover, automated anomaly detection can uncover unusual payment activities, bolstering security measures against fraud.

Integrating payment data with AWS analytics tools provides a comprehensive view of business performance, enabling metrics tracking such as conversion rates and refund rates. In essence, a real-time data pipeline from Stripe to AWS equips businesses with the tools they need to adapt to market changes, optimize operational performance, and make quicker, informed decisions, ultimately driving growth and profitability.

In this post, we'll explore how to build a scalable, real-time payment analytics pipeline using Stripe, [Amazon Kinesis](https://aws.amazon.com/kinesis/), and [OpenSearch](https://aws.amazon.com//what-is/opensearch/). This architecture enables near real-time visibility into payment metrics while maintaining the flexibility to perform complex historical analyses.

## The challenges of payment analytics at scale

Payment processors like Stripe generate numerous events for each transaction—from initial authorization attempts to final settlement. While Stripe provides a dashboard for many metrics, organizations often need deeper analytics capabilities, historical trend analysis, and real-time visibility into their payment flows.

Traditional approaches to payment analytics often rely on periodic batch processing or direct database queries against the payment provider's API. While functional for smaller scales, these approaches break down as transaction volumes grow. Common pain points include:

- API rate limits preventing timely data access
- High latency for complex aggregations
- Limited ability to correlate payment data with other business metrics
- Difficulty maintaining historical trending data
- Schema evolution challenges as payment models change

Many organizations face common challenges when dealing with payment data. Transaction volumes can vary dramatically throughout the day, making capacity planning difficult. Payment events often need enrichment with business context before they become truly valuable for analytics. Additionally, different teams within an organization may need different views of the payment data, from real-time fraud detection to monthly revenue analysis.

## Architectural overview

![](https://stripe.dev/images/real-time-payment-analytics-stripe-to-aws-data-pipeline/image1.png) This sample solution uses AWS's managed services to create a robust, scalable pipeline with minimum custom coding:

1. **Stripe Payment System**: Source of payment events, generating webhooks for various transaction states and activities.
2. [**Amazon API Gateway**](https://aws.amazon.com/api-gateway/): Managed API endpoint service that provides authentication, throttling, and request validation for incoming Stripe webhooks.
3. [**AWS Lambda**](https://aws.amazon.com/lambda/) Processor: Serverless function that validates webhook signatures, enriches payment data with business context, and prepares events for streaming.
4. [**Amazon Kinesis Data Streams**](https://aws.amazon.com/kinesis/data-streams/): Managed streaming service that acts as a buffer and enables real-time processing of payment events at scale.
5. **Lambda Consumer**: Serverless function that reads from Kinesis stream, transforms data, and loads it into OpenSearch for analytics.
6. [**Amazon OpenSearch Service**](https://aws.amazon.com/opensearch-service/): Managed search and analytics engine that indexes payment data and enables complex queries and aggregations.
7. [**OpenSearch Dashboards**](https://docs.aws.amazon.com/opensearch-service/latest/developerguide/dashboards.html): Visualization platform for creating real-time dashboards and exploring payment analytics.
8. [**Amazon CloudWatch Metrics**](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/working_with_metrics.html): Monitoring service tracking performance metrics and health of the entire pipeline.

When a payment event occurs in Stripe, it triggers a webhook that sends the event to an API Gateway endpoint. This endpoint is configured with appropriate authentication and rate limiting to handle high volumes of incoming events securely. The API Gateway invokes a Lambda function that performs initial validation and enrichment of the event data.

The enriched events are then published to a Kinesis Data Stream, which acts as a buffer and enables parallel processing of events. Kinesis provides configurable retention periods and the ability to replay events if needed, making it an ideal choice for this architecture.

A second Lambda function processes the Kinesis stream, transforming the events into a format suitable for analytics and indexing them into OpenSearch. OpenSearch provides near real-time search and analytics capabilities, while OpenSearch Dashboards enables the creation of rich visualizations and dashboards.

It’s also possible to route events from Kinesis to Kinesis Data Firehose to OpenSearch, without the need for the second Lambda function. This may create added latency due to the batching in Kinesis Data Firehose but may reduce costs by removing the extra function. View this [code sample](https://serverlessland.com/patterns/kinesis-data-firehose-firehose-opensearch-sam) for an example of this architecture.

## Implementation details

### Configuring Stripe webhooks

First, you need to configure Stripe to send payment events to the pipeline. Stripe's webhook system supports signing of events for security and provides retry logic for failed deliveries. In the Stripe Dashboard, create a webhook endpoint pointing to your API Gateway URL. For production environments, enable the following event types:

- `payment_intent.succeeded`
- `payment_intent.failed`
- `charge.succeeded`
- `charge.failed`
- `charge.refunded`

### API Gateway and event validation

API Gateway serves as the secure entry point for Stripe events. You can implement webhook signature validation using a [Lambda authorizer](https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-use-lambda-authorizer.html):

```
const crypto = require('crypto');

exports.handler = async (event) => {  
  const signature = event.headers['stripe-signature'];  
  const webhookSecret = process.env.STRIPE_WEBHOOK_SECRET;  
    
  const body = event.body;  
  const timestamp = event.headers['stripe-timestamp'];  
    
  const signedPayload = `${timestamp}.${body}`;  
  const expectedSignature = crypto  
    .createHmac('sha256', webhookSecret)  
    .update(signedPayload)  
    .digest('hex');  
      
  if (expectedSignature !== signature) {  
    throw new Error('Invalid signature');  
  }  
    
  return {  
    principalId: 'stripe',  
    policyDocument: {  
      Version: '2012-10-17',  
      Statement: [{
        Action: 'execute-api:Invoke',  
        Effect: 'Allow',  
        Resource: event.methodArn  
      }]  
    }  
  };  
};
```

### Kinesis Stream configuration

Kinesis Data Streams serves as the backbone of the real-time pipeline, providing the scalability and reliability needed for processing payment events. Let's dive deep into the configuration and best practices for setting up Kinesis streams effectively.

Proper capacity planning is crucial for Kinesis streams. Each shard can support writes of up to 1,000 records per second, up to a maximum data write total of 1 MB per second. Each PutRecords request can support up to 500 records. Each record in the request can be as large as 1 MB, up to a limit of 5 MB for the entire request, including partition keys.

For payment events, calculate your required shard count using:

```
required_shards = ceil(max(
    peak_records_per_second / 1000,
    peak_mb_per_second / 1
))
```

You can also implement automated scaling using the [Kinesis Auto Scaling API](https://aws.amazon.com/blogs/big-data/auto-scaling-amazon-kinesis-data-streams-using-amazon-cloudwatch-and-aws-lambda/). This can be set up in infrastructure-as-code (IaC) tools, like [Terraform](https://www.terraform.io/):

```
resource "aws_appautoscaling_target" "kinesis_target" {  
  max_capacity       = 10  
  min_capacity       = 2  
  resource_id        = "stream/${aws_kinesis_stream.payment_events.name}"  
  scalable_dimension = "kinesis:stream:ShardCount"  
  service_namespace  = "kinesis"  
}

resource "aws_appautoscaling_policy" "kinesis_scaling_policy" {  
  name               = "payment-events-scaling"  
  policy_type        = "TargetTrackingScaling"  
  resource_id        = aws_appautoscaling_target.kinesis_target.resource_id  
  scalable_dimension = aws_appautoscaling_target.kinesis_target.scalable_dimension  
  service_namespace  = aws_appautoscaling_target.kinesis_target.service_namespace

  target_tracking_scaling_policy_configuration {  
    target_value = 70  
    scale_in_cooldown  = 300  
    scale_out_cooldown = 60

    predefined_metric_specification {  
      predefined_metric_type = "KinesisWriteProvisionedThroughputExceeded"  
    }  
  }  
}  
```

For more detailed information, see the [GitHub Kinesis Autoscaling repo](https://github.com/aws-samples/kinesis-auto-scaling).

### Partition key strategy

The partition key is important as it determines which shard in your Kinesis stream receives a given record. A well-designed partition key strategy is essential for:

- Even distribution of data across shards
- Maintaining ordered processing when needed
- Optimal throughput utilization
- Cost efficiency

For payment events, there are several strategies to consider:

#### 1\. Customer-based partitioning

```
def generate_partition_key(payment_event):  
    return payment_event['customer_id']
```

This approach guarantees that all payments from the same customer go to the same shard, maintaining order. This is useful when you need to process a customer's payments in sequence (for example, handling subscription payments or maintaining running balances). However, if you have "hot" customers generating many transactions, this can lead to shard hot-spotting.

#### 2\. Random distribution

```
import uuid

def generate_partition_key(payment_event):  
    return str(uuid.uuid4())
```

This provides excellent distribution across shards but sacrifices ordering guarantees. It's ideal for use cases where each payment is independent and order doesn't matter.

#### 3\. Hybrid approach (recommended)

```
import uuid  
from datetime import datetime

def generate_partition_key(payment_event):  
    # If order matters for this type of payment  
    if payment_event.get('requires_ordering'):  
        return f"{payment_event['customer_id']}"  
      
    # For subscriptions, maintain order per subscription  
    if payment_event.get('subscription_id'):  
        return f"{payment_event['subscription_id']}"  
      
    # For regular one-off payments, distribute randomly  
    return str(uuid.uuid4())
```

This strategy balances ordering requirements with distribution:

- Maintains order when needed (subscriptions, dependent transactions)
- Distributes independent transactions evenly
- Prevents hot-spotting by isolating high-volume customers

#### 4\. Time-based distribution

```
def generate_partition_key(payment_event):  
    timestamp = datetime.fromtimestamp(payment_event['created'])  
    # Partition by hour to balance distribution and time-based querying  
    hour_bucket = timestamp.strftime('%Y%m%d%H')  
    # Add random suffix to distribute within the hour  
    return f"{hour_bucket}#{uuid.uuid4().hex[:8]}"
```

This helps when you need to query data for specific time periods, while still maintaining good distribution within each time bucket.

Remember that changing your partition key strategy on an existing stream can be disruptive, as it will change how records are distributed across shards. Plan such changes carefully and consider creating a new stream with the new strategy while maintaining the old one during transition.

### Event processing and enrichment

A Lambda function processes events from Kinesis, performing any necessary transformations and enrichment before sending them to OpenSearch. This function can perform various useful tasks, including:

- Flattening nested JSON structures
- Converting timestamps to ISO 8601 format
- Adding derived fields (e.g., payment method categories)
- Enriching with business context (e.g., customer segments)

```
exports.handler = async (event) => {  
  const records = event.Records.map(record => {  
    const payment = JSON.parse(Buffer.from(record.kinesis.data, 'base64'));  
      
    return {  
      payment_id: payment.id,  
      amount: payment.amount / 100, // Convert cents to dollars  
      currency: payment.currency,  
      status: payment.status,  
      payment_method: {  
        type: payment.payment_method_details.type,  
        category: categorizePaymentMethod(payment.payment_method_details),  
        country: payment.payment_method_details.card?.country  
      },  
      timestamp: new Date(payment.created * 1000).toISOString(),  
      // Add additional enriched fields  
    };  
  });  
    
  // TODO: sendToOpenSearch(records);  
};

function categorizePaymentMethod(details) {  
  if (details.type === 'card') {  
    return details.card.brand === 'amex' ? 'premium_card' : 'standard_card';  
  }  
  return 'alternative_payment';  
}  
```

Read more about [Best practices for consuming Amazon Kinesis Data Streams using AWS Lambda](https://aws.amazon.com/blogs/big-data/best-practices-for-consuming-amazon-kinesis-data-streams-using-aws-lambda/) on the AWS Big Data Blog.

### Conclusion

This architecture provides a solid foundation for real-time payment analytics. It provides sub-minute visibility into payment metrics, scalable to millions of transactions per day, with flexible querying capabilities and historical trend analysis. The design is maintainable and extensible, thanks to offloading the harder tasks to services and using minimal custom code.

The combination of Stripe's reliable webhook system, Kinesis's real-time processing capabilities, and OpenSearch's powerful analytics features creates a solution that can grow with your business while providing immediate visibility into critical payment metrics.

Before designing your own solution based on this sample, remember to follow security best practices, including encryption at rest and in transit, proper IAM configurations, and regular security audits.

For more Stripe developer learning resources, subscribe to our [YouTube Channel](https://www.youtube.com/@StripeDev).
