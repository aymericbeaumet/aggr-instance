---
title: Load balancing Stripe API calls from multiple AWS regions
link: https://stripe.dev/blog/load-balancing-stripe-api-calls-multiple-aws-regions
source: stripe-com
published: 2025-03-06T00:00:00Z
updated: 2025-03-06T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- James Beswick
summary: Implementing a resilient multi-region payment processing system using AWS and Stripe ensures reliable webhook handling, minimizes outages, and complies with regulations. This guide covers architecture design, challenges, and AWS service integration for optimal global performance and rate limit management.
content: extracted
html: 2025-03-06-load-balancing-stripe-api-calls-from-multiple-aws-regions.html
remote_preview:
  url: https://stripe.dev/og/blog/load-balancing-stripe-api-calls-multiple-aws-regions.png
  alt: 'Stripe Developer Blog: Load balancing Stripe API calls from multiple AWS regions'
extra:
  content:interactive: true
---

Ensuring reliable payment webhook processing is critical for business success. Failures can lead to lost revenue, damaged customer trust, and operational headaches. This guide explores implementing a resilient, multi-region payment processing architecture using AWS services and Stripe's API, following [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected) principles.

## The challenges of global payment processing

Payment processing systems face unique challenges when operating at a global scale. Regional API outages, network latency, and rate limiting can all impact the ability to process transactions successfully. Traditional single-region architectures are vulnerable to these issues, potentially leading to service disruptions and lost transactions.

Consider a scenario where your primary payment processing region experiences an outage. Without proper redundancy, your business could lose thousands or even millions in revenue during the downtime. Additionally, customers in different geographic regions may experience high latency when their payments are processed through a distant data center. You must also consider Stripe's API rate limits, which must be managed across multiple regions to ensure optimal throughput without exceeding quotas.

Beyond the technical challenges, regulatory requirements often mandate specific data residency and processing requirements for payments. For instance, the European Union's GDPR and PSD2 regulations impose strict requirements on payment data handling and customer authentication. A multi-region architecture must account for these compliance requirements while maintaining system reliability.

## Architecture overview

This sample solution implements a multi-region payment processing system that provides high availability, disaster recovery, and consistent performance across global deployments. The architecture uses several AWS services including [Amazon Route 53](https://aws.amazon.com/route53/), [Amazon DynamoDB Global Tables](https://aws.amazon.com/dynamodb/global-tables/), and [AWS Lambda](https://aws.amazon.com/lambda/), integrated with Stripe's payment processing API.

![](https://stripe.dev/images/load-balancing-stripe-api-calls-multiple-aws-regions/image1.png)

This architecture diagram illustrates the key components and their interactions:

1. Route 53 serves as the initial entry point, performing DNS resolution and health checks
2. Traffic is then routed to the appropriate region.
3. Each region maintains identical infrastructure:

- API Gateway endpoints for payment processing.
- Lambda functions for business logic and rate limit management.
- DynamoDB Global Tables for state management and rate limiting.

4. Stripe API integration is handled consistently across regions

The flow begins with Route 53 DNS resolution, which directs traffic to the most appropriate region. This ensures optimal routing and automatic failover capabilities and then forwards requests to the regional API Gateway endpoint, maintaining low latency and high availability.

First, Route 53 serves as the entry point for payment API requests. Health checks monitor the availability of payment processing endpoints in each region. DynamoDB Global Tables maintain consistent payment state across regions, while Lambda functions handle the actual payment processing logic and Stripe API interactions.

Each component plays a role in ensuring system reliability:

- **Route 53 Health Checking**: The health checking system continuously monitors endpoint availability across all regions. It uses sophisticated failure detection algorithms that consider both endpoint health and regional AWS health status. DNS failover can be configured with different routing policies (latency-based, weighted, or geolocation) to match your specific requirements.
- **DynamoDB Global Tables**: This fully managed multi-master database service automatically replicates payment state across regions with conflict resolution. It provides single-digit millisecond read and write performance at any scale, with built-in encryption and point-in-time recovery.
- **Lambda Payment Processing**: Serverless functions handle the payment logic with automatic scaling and fail-safe operation. Each function is configured with appropriate timeouts, memory allocation, and concurrent execution limits to ensure reliable operation under load.

## Implementation details

Let's walk through implementing each component of this architecture.

### DNS and health checks with Route 53

First, we'll set up Route 53 health checks to monitor the payment processing endpoints. Here's the CloudFormation template for configuring health checks and DNS failover:

```
AWSTemplateFormatVersion: '2010-09-09'
Description: 'Payment Processing Health Checks and DNS Failover'

Resources:
  PaymentEndpointHealthCheck:
    Type: 'AWS::Route53::HealthCheck'
    Properties:
      HealthCheckConfig:
        Port: 443
        Type: HTTPS
        ResourcePath: '/payment-health'
        FullyQualifiedDomainName: !Sub 'api.${AWS::StackName}.example.com'
        RequestInterval: 30
        FailureThreshold: 3
      HealthCheckTags:
        - Key: Name
          Value: PaymentEndpointHealth

  PaymentDNSRecord:
    Type: 'AWS::Route53::RecordSet'
    Properties:
      HostedZoneName: example.com.
      Name: !Sub 'api.${AWS::StackName}.example.com.'
      Type: A
      SetIdentifier: !Sub '${AWS::Region}-primary'
      Region: !Ref 'AWS::Region'
      Failover: PRIMARY
      HealthCheckId: !Ref PaymentEndpointHealthCheck
      AliasTarget:
        DNSName: !GetAtt PaymentDistribution.DomainName
        HostedZoneId: Z2FDTNDATAQYW2
        EvaluateTargetHealth: true
```

Visit the [AWS Route53 documentation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-route53-healthcheck.html) for a complete description of each property.

### DynamoDB Global Tables for State Management

Payment state consistency is critical across regions, and you can use DynamoDB Global Tables to maintain this state. Here’s an example of a definition (see the [Global Tables documentation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-dynamodb-globaltable.html) for a description of each property):

```
Resources:
  PaymentStateTable:
    Type: 'AWS::DynamoDB::Table'
    Properties:
      TableName: !Sub '${AWS::StackName}-payment-state'
      AttributeDefinitions:
        - AttributeName: PaymentId
          AttributeType: S
        - AttributeName: Status
          AttributeType: S
      KeySchema:
        - AttributeName: PaymentId
          KeyType: HASH
        - AttributeName: Status
          KeyType: RANGE
      StreamSpecification:
        StreamViewType: NEW_AND_OLD_IMAGES
      BillingMode: PAY_PER_REQUEST
      SSESpecification:
        SSEEnabled: true
      ReplicaSpecification:
        - Region: us-east-1
        - Region: eu-west-1
        - Region: ap-southeast-1
```

### Lambda Payment Processing Function

The core payment processing Lambda function interfaces with the Stripe API. This CloudFormation template defines the function attributes and the logic:

```
Resources:
  PaymentProcessorFunction:
    Type: 'AWS::Lambda::Function'
    Properties:
      Handler: index.handler
      Runtime: nodejs22.x
      Code:
        ZipFile: |
          const stripe = require('stripe')(process.env.STRIPE_SECRET_KEY);
          const AWS = require('aws-sdk');
          const dynamodb = new AWS.DynamoDB.DocumentClient();

          exports.handler = async (event) => {
            try {
              // Extract payment details from event
              const { amount, currency, paymentMethod } = JSON.parse(event.body);
              
              // Initialize payment in DynamoDB
              await dynamodb.put({
                TableName: process.env.PAYMENT_STATE_TABLE,
                Item: {
                  PaymentId: event.requestContext.requestId,
                  Status: 'PENDING',
                  Timestamp: Date.now(),
                  Region: process.env.AWS_REGION
                }
              }).promise();

              // Create payment intent with Stripe
              const paymentIntent = await stripe.paymentIntents.create({
                amount,
                currency,
                payment_method: paymentMethod,
                confirm: true,
                automatic_payment_methods: {
                  enabled: true,
                  allow_redirects: 'never'
                }
              });

              // Update payment state
              await dynamodb.update({
                TableName: process.env.PAYMENT_STATE_TABLE,
                Key: { PaymentId: event.requestContext.requestId },
                UpdateExpression: 'SET #status = :status, StripePaymentId = :stripeId',
                ExpressionAttributeNames: {
                  '#status': 'Status'
                },
                ExpressionAttributeValues: {
                  ':status': paymentIntent.status,
                  ':stripeId': paymentIntent.id
                }
              }).promise();

              return {
                statusCode: 200,
                body: JSON.stringify({
                  paymentId: event.requestContext.requestId,
                  stripePaymentId: paymentIntent.id,
                  status: paymentIntent.status
                })
              };
            } catch (error) {
              console.error('Payment processing error:', error);
              
              // Update payment state with error
              await dynamodb.update({
                TableName: process.env.PAYMENT_STATE_TABLE,
                Key: { PaymentId: event.requestContext.requestId },
                UpdateExpression: 'SET #status = :status, ErrorMessage = :error',
                ExpressionAttributeNames: {
                  '#status': 'Status'
                },
                ExpressionAttributeValues: {
                  ':status': 'ERROR',
                  ':error': error.message
                }
              }).promise();

              return {
                statusCode: 500,
                body: JSON.stringify({
                  error: 'Payment processing failed',
                  paymentId: event.requestContext.requestId
                })
              };
            }
          };
      Environment:
        Variables:
          STRIPE_SECRET_KEY: '{{resolve:secretsmanager:StripeSecrets:SecretString:SecretKey}}'
          PAYMENT_STATE_TABLE: !Ref PaymentStateTable
      Role: !GetAtt PaymentProcessorRole.Arn
      Timeout: 30
      MemorySize: 256
```

### Rate limit management

Managing Stripe's API rate limits across multiple regions requires coordination to prevent exceeding global quotas while maximizing throughput. This implementation uses a distributed token bucket algorithm with DynamoDB Global Tables as the coordination mechanism.

First, consider Stripe’s various rate limits:

- Request limits per second (e.g., 100 requests/second)
- Request limits per minute (e.g., 1000 requests/minute)
- Concurrent request limits (e.g., 25 concurrent requests)
- Account-level limits that apply across all regions

The system needs to manage these limits while allowing each region to process payments independently.

First, let's set up the DynamoDB table for rate limit management:

```
Resources:
  RateLimitTable:
    Type: 'AWS::DynamoDB::Table'
    Properties:
      TableName: !Sub '${AWS::StackName}-rate-limits'
      AttributeDefinitions:
        - AttributeName: ApiKey
          AttributeType: S
        - AttributeName: LimitType
          AttributeType: S
      KeySchema:
        - AttributeName: ApiKey
          KeyType: HASH
        - AttributeName: LimitType
          KeyType: RANGE
      TimeToLiveSpecification:
        AttributeName: ExpirationTime
        Enabled: true
      BillingMode: PAY_PER_REQUEST
      StreamSpecification:
        StreamViewType: NEW_AND_OLD_IMAGES
      GlobalSecondaryIndexes:
        - IndexName: LimitTypeIndex
          KeySchema:
            - AttributeName: LimitType
              KeyType: HASH
          Projection:
            ProjectionType: ALL
```

Here's a sample Lambda function that manages rate limiting:

```
const BUCKET_SIZE = 100; // Maximum tokens
const REFILL_RATE = 10; // Tokens per second
const REFILL_INTERVAL = 1000; // 1 second in milliseconds

async function acquireToken(apiKey, limitType, tokensNeeded = 1) {
  const ddb = new AWS.DynamoDB.DocumentClient();
  const now = Date.now();

  try {
    // Optimistic locking with condition expression
    const result = await ddb.update({
      TableName: process.env.RATE_LIMIT_TABLE,
      Key: {
        ApiKey: apiKey,
        LimitType: limitType
      },
      UpdateExpression: `
        SET tokens = if_not_exists(tokens, :bucket_size),
            lastRefillTimestamp = if_not_exists(lastRefillTimestamp, :now),
            lastUpdateRegion = :region
      `,
      ConditionExpression: `
        attribute_not_exists(lockUntil) OR 
        lockUntil < :now
      `,
      ExpressionAttributeValues: {
        ':bucket_size': BUCKET_SIZE,
        ':now': now,
        ':region': process.env.AWS_REGION
      },
      ReturnValues: 'ALL_NEW'
    }).promise();

    const bucket = result.Attributes;
    
    // Calculate token refill
    const timePassed = now - bucket.lastRefillTimestamp;
    const tokensToAdd = Math.floor(timePassed / REFILL_INTERVAL) * REFILL_RATE;
    const newTokens = Math.min(BUCKET_SIZE, bucket.tokens + tokensToAdd);

    // Check if we have enough tokens
    if (newTokens < tokensNeeded) {
      return false;
    }

    // Consume tokens with another optimistic lock
    await ddb.update({
      TableName: process.env.RATE_LIMIT_TABLE,
      Key: {
        ApiKey: apiKey,
        LimitType: limitType
      },
      UpdateExpression: `
        SET tokens = :newTokens,
            lastRefillTimestamp = :now,
            lastUpdateRegion = :region
      `,
      ConditionExpression: `
        lastUpdateRegion = :oldRegion AND
        lastRefillTimestamp = :oldTimestamp
      `,
      ExpressionAttributeValues: {
        ':newTokens': newTokens - tokensNeeded,
        ':now': now,
        ':region': process.env.AWS_REGION,
        ':oldRegion': bucket.lastUpdateRegion,
        ':oldTimestamp': bucket.lastRefillTimestamp
      }
    }).promise();

    return true;
  } catch (error) {
    if (error.code === 'ConditionalCheckFailedException') {
      // Handle race condition by retrying
      await sleep(Math.random() * 100); // Random backoff
      return acquireToken(apiKey, limitType, tokensNeeded);
    }
    throw error;
  }
}
```

To handle rate limits across regions:

1. Each region maintains its own token bucket in DynamoDB.
2. Global Tables replicate the state across regions.
3. [Optimistic locking](https://en.wikipedia.org/wiki/Optimistic_concurrency_control) prevents race conditions.
4. Each region gets a share of the global rate limit.

The code that makes Stripe API calls can be wrapped to handle rate limiting with this approach:

```
const TOKEN_BUCKET_SIZE = 10; // Maximum number of tokens
const REFILL_RATE = 1; // How many tokens to add per second
let tokensAvailable = TOKEN_BUCKET_SIZE;
let lastRefillTimestamp = Date.now();

// Function to simulate token refilling
function refillTokens() {
  const now = Date.now();
  const elapsedSeconds = Math.floor((now - lastRefillTimestamp) / 1000);
  if (elapsedSeconds > 0) {
    // Refill tokens based on elapsed time, making sure not to exceed the bucket size
    tokensAvailable = Math.min(TOKEN_BUCKET_SIZE, tokensAvailable + elapsedSeconds * REFILL_RATE);
    lastRefillTimestamp = now;
  }
}

async function acquireToken(apiKey, limitType) {
  // Refill tokens before trying to acquire one
  refillTokens();

  if (tokensAvailable > 0) {
    // Token successfully acquired
    tokensAvailable--;
    return true;
  } else {
    // No tokens available
    return false;
  }
}

async function sleep(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

async function makeStripeRequest(apiKey, limitType, requestFn) {
  const maxRetries = 3;
  const baseBackoffMs = 100;
  let retries = 0;

  while (retries < maxRetries) {
    try {
      // Try to acquire a token
      const acquired = await acquireToken(apiKey, limitType);
      if (!acquired) {
        // No tokens available, implement backoff
        const backoffMs = Math.min(Math.pow(2, retries) * baseBackoffMs, 30000); // Max backoff of 30 seconds
        await sleep(backoffMs);
        retries++;
        continue;
      }

      // Make the actual Stripe API call
      return await requestFn(); // Ensure requestFn is invoked correctly
    } catch (error) {
      // Handle Stripe rate limit errors and implement backoff logic
      if (error && error.code === 'rate_limit_exceeded') {
        const backoffMs = Math.min(Math.pow(2, retries) * baseBackoffMs, 30000); // Max backoff of 30 seconds
        await sleep(backoffMs);
        retries++;
        continue;
      }

      // Handle any other errors (log or rethrow)
      console.error('Error while making Stripe request:', error);
      throw error; // Rethrow unhandled errors
    }
  }

  throw new Error('Rate limit retries exceeded');
}
```

## Conclusion

Multi-region processing architecture provides a robust foundation for applications integrating with Stripe for global payment operations.

By using AWS services and following Well-Architected Framework principles, you can create an architecture that's resilient to regional failures, maintains consistent payment state, and efficiently manages API rate limits.

For further reading:

- [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/)
- [Stripe API Documentation](https://stripe.com/docs/api)
- [DynamoDB Global Tables](https://aws.amazon.com/dynamodb/global-tables/)
- [Route 53 Application Recovery Controller](https://aws.amazon.com/route53/application-recovery-controller/)
- [Using latency-based routing with Amazon CloudFront for a multi-Region active-active architecture](https://aws.amazon.com/blogs/networking-and-content-delivery/latency-based-routing-leveraging-amazon-cloudfront-for-a-multi-region-active-active-architecture/)

Remember to thoroughly test your architecture in a staging environment before deploying to production, and always follow security best practices when handling payment information.

For more Stripe learning resources, subscribe to our [YouTube channel](https://www.youtube.com/stripedevelopers).
