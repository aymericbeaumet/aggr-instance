---
title: Optimizing Stripe API performance in Lambda with caching strategies
link: https://stripe.dev/blog/optimizing-stripe-api-performance-lambda-caching-elasticache-dynamodb
source: stripe-com
published: 2025-04-21T00:00:00Z
updated: 2025-04-21T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- James Beswick
summary: Discover advanced caching strategies to optimize Stripe API performance in AWS Lambda using Amazon ElastiCache and DynamoDB. Learn how to manage API rate limits, reduce latency, and minimize costs while ensuring data consistency and scalability in high-volume applications.
content: extracted
html: 2025-04-21-optimizing-stripe-api-performance-in-lambda-with-caching.html
remote_preview:
  url: https://stripe.dev/og/blog/optimizing-stripe-api-performance-lambda-caching-elasticache-dynamodb.png
  alt: 'Stripe Developer Blog: Optimizing Stripe API performance in Lambda with caching strategies'
extra:
  content:interactive: true
---

When building systems that heavily interact with any external API, developers often encounter challenges around performance, cost optimization, and rate limiting. Stripe's API is powerful and well-documented, but as your application scales, you'll need to carefully manage your interactions with it, as with any external API.

There are several key challenges to consider:

1. **API rate limits**: Stripe API [rate limiting](https://docs.stripe.com/rate-limits) is a mechanism that controls the number of API requests an application can make over a specified period, primarily measured in requests per second (RPS). When users exceed these limits, they receive [429 errors](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Status/429). Rate limits vary by endpoint and may depend on the account's characteristics and transaction volumes.

2. [**API latency**](https://blog.postman.com/what-is-api-latency/): Each call to Stripe's API introduces latency to your application, typically ranging from 100ms to 500ms depending on the endpoint and operation.

3. **Costs**: While Stripe doesn't charge for API usage directly, the cumulative effect of unnecessary API calls impacts your application's performance and infrastructure costs in AWS.

In this post, we'll dive deep into implementing an advanced caching architecture using [AWS Lambda](https://aws.amazon.com/lambda/), [Amazon ElastiCache for Redis](https://aws.amazon.com/elasticache/redis/), and [Amazon DynamoDB](https://aws.amazon.com/dynamodb/) to create a robust and scalable solution for high-volume Stripe API interactions. Let's explore how we can architect a solution to address these challenges using AWS services.

## Architecture overview

This sample solution implements a multi-layer caching strategy that combines the speed of ElastiCache for Redis with the durability and TTL ([Time To Live](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/TTL.html)) capabilities of DynamoDB. Here's how the components work together:

![](https://stripe.dev/images/optimizing-stripe-api-performance-lambda-caching-elasticache-dynamodb/image1.png)

Redis serves as the first-level cache, providing sub-millisecond access to frequently requested data. DynamoDB acts as the second-level cache, storing less frequently accessed data and providing automatic TTL management. Lambda functions orchestrate the interaction between these services and the Stripe API.

## Implementing the cache layer

This caching strategy implements the following flow:

1. Check Redis for the requested data.

2. If not in Redis, check DynamoDB.

3. If not in DynamoDB, fetch from Stripe API.

4. Update both cache layers with the new data.

Here's a more detailed implementation:

```
const AWS = require('aws-sdk');
const dynamodb = new AWS.DynamoDB.DocumentClient();

async function getCustomerData(customerId) {
  // Check Redis first
  const redis = getRedisClient();   // TODO: Implementation-dependent
  const cachedData = await redis.get(`customer:${customerId}`);
  
  if (cachedData) {
    return JSON.parse(cachedData);
  }
  
  // Check DynamoDB
  const dynamoResult = await dynamodb.get({
    TableName: 'stripe-cache',
    Key: { pk: `customer:${customerId}` }
  }).promise();
  
  if (dynamoResult.Item && dynamoResult.Item.data) {
    // Store in Redis for future requests
    await redis.set(
      `customer:${customerId}`,
      JSON.stringify(dynamoResult.Item.data),
      'EX',
      3600 // 1 hour expiry
    );
    return dynamoResult.Item.data;
  }
  
  // Fetch from Stripe
  const customer = await stripe.customers.retrieve(customerId);
  
  // Update both cache layers
  await Promise.all([
    redis.set(
      `customer:${customerId}`,
      JSON.stringify(customer),
      'EX',
      3600
    ),
    dynamodb.put({
      TableName: 'stripe-cache',
      Item: {
        pk: `customer:${customerId}`,
        data: customer,
        ttl: Math.floor(Date.now() / 1000) + 86400 // 24 hour TTL
      }
    }).promise()
  ]);
  
  return customer;
}
```

## DynamoDB table design

The DynamoDB table design needs to support efficient lookups, automatic TTL cleanup, and flexible data storage for various Stripe entity types. Here the table design defined in CloudFormation:

```
AWSTemplateFormatVersion: '2010-09-09'
Description: 'DynamoDB table for Stripe API caching with GSI and TTL'

Parameters:
  Environment:
    Type: String
    Default: dev
    AllowedValues:
      - dev
      - staging
      - prod
    Description: Environment name for the stack

  TableReadCapacity:
    Type: Number
    Default: 5
    Description: Read capacity units for the table
    MinValue: 1

  TableWriteCapacity:
    Type: Number
    Default: 5
    Description: Write capacity units for the table
    MinValue: 1

  GSIReadCapacity:
    Type: Number
    Default: 5
    Description: Read capacity units for GSI
    MinValue: 1

  GSIWriteCapacity:
    Type: Number
    Default: 5
    Description: Write capacity units for GSI
    MinValue: 1

Conditions:
  IsProd: !Equals 
    - !Ref Environment
    - prod

Resources:
  StripeCacheTable:
    Type: AWS::DynamoDB::Table
    Properties:
      TableName: !Sub stripe-cache-${Environment}
      BillingMode: PROVISIONED
      ProvisionedThroughput:
        ReadCapacityUnits: !If 
          - IsProd
          - 50
          - !Ref TableReadCapacity
        WriteCapacityUnits: !If 
          - IsProd
          - 50
          - !Ref TableWriteCapacity
      AttributeDefinitions:
        - AttributeName: pk
          AttributeType: S
        - AttributeName: sk
          AttributeType: S
        - AttributeName: gsi1pk
          AttributeType: S
        - AttributeName: gsi1sk
          AttributeType: S
      KeySchema:
        - AttributeName: pk
          KeyType: HASH
        - AttributeName: sk
          KeyType: RANGE
      GlobalSecondaryIndexes:
        - IndexName: gsi1
          KeySchema:
            - AttributeName: gsi1pk
              KeyType: HASH
            - AttributeName: gsi1sk
              KeyType: RANGE
          Projection:
            ProjectionType: ALL
          ProvisionedThroughput:
            ReadCapacityUnits: !If 
              - IsProd
              - 25
              - !Ref GSIReadCapacity
            WriteCapacityUnits: !If 
              - IsProd
              - 25
              - !Ref GSIWriteCapacity
      TimeToLiveSpecification:
        AttributeName: ttl
        Enabled: true
      Tags:
        - Key: Environment
          Value: !Ref Environment
        - Key: Service
          Value: stripe-cache
        - Key: ManagedBy
          Value: CloudFormation

Outputs:
  TableName:
    Description: Name of the DynamoDB table
    Value: !Ref StripeCacheTable
    Export:
      Name: !Sub ${AWS::StackName}-TableName

  TableArn:
    Description: ARN of the DynamoDB table
    Value: !GetAtt StripeCacheTable.Arn
    Export:
      Name: !Sub ${AWS::StackName}-TableArn
```

The CloudFormation template implements a production-ready DynamoDB table design optimized for caching Stripe API responses. At its core, the table uses a composite key structure with partition and sort keys (pk and sk) to enable flexible querying patterns, while a [Global Secondary Index](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GSI.html) (gsi1) supports efficient lookups by cache type and timestamp. The template incorporates TTL functionality for automatic cleanup of expired cache entries. The infrastructure is environment-aware, with built-in parameter customization that allows for different capacity settings across development, staging, and production environments.

The table uses a composite key structure to support various access patterns:

```
// Example item structure
{
  pk: 'CUSTOMER#cus_123',           // Partition key
  sk: 'METADATA#latest',            // Sort key
  gsi1pk: 'CACHE#customer',         // GSI partition key
  gsi1sk: '2025-02-05T12:00:00Z',  // GSI sort key for TTL ordering
  data: {
    // Stripe customer object
    id: 'cus_123',
    email: 'customer@example.com',
    // ... other Stripe fields
  },
  ttl: 1707225600,                  // Unix timestamp for TTL
  lastUpdated: '2025-02-05T12:00:00Z',
  version: 1,
  checksum: 'abc123'                // For data integrity verification
}
```

Some of the notable design decisions here include:

1. **Composite Keys**: The composite key structure (pk + sk) supports multiple item types per customer:

```
// Primary keys for different item types
const keys = {
  customerMetadata: {
    pk: `CUSTOMER#${customerId}`,
    sk: 'METADATA#latest'
  },
  customerSubscriptions: {
    pk: `CUSTOMER#${customerId}`,
    sk: 'SUBSCRIPTIONS#latest'
  },
  customerPaymentMethods: {
    pk: `CUSTOMER#${customerId}`,
    sk: 'PAYMENT_METHODS#latest'
  }
};
```

2. **Global Secondary Index** (GSI): The GSI enables efficient queries for finding all cached items of a specific type, identifying stale cache entries, and supporting bulk cache invalidation.

```
// Query all cached customers updated before a timestamp
async function findStaleCustomers(timestamp) {
  const result = await dynamodb.query({
    TableName: 'stripe-cache',
    IndexName: 'gsi1',
    KeyConditionExpression: 
      'gsi1pk = :type AND gsi1sk < :timestamp',
    ExpressionAttributeValues: {
      ':type': 'CACHE#customer',
      ':timestamp': timestamp
    }
  }).promise();
  return result.Items;
}
```

3. **Data Versioning**: A version field can help handle concurrent updates:

```
async function updateCustomerCache(customerId, data) {
  try {
    await dynamodb.put({
      TableName: 'stripe-cache',
      Item: {
        pk: `CUSTOMER#${customerId}`,
        sk: 'METADATA#latest',
        data: data,
        version: 1,
        ttl: Math.floor(Date.now() / 1000) + 86400,
        lastUpdated: new Date().toISOString(),
        checksum: calculateChecksum(data)
      },
      ConditionExpression: 
        'attribute_not_exists(version) OR version < :newVersion',
      ExpressionAttributeValues: {
        ':newVersion': 1
      }
    }).promise();
  } catch (error) {
    if (error.code === 'ConditionalCheckFailedException') {
      // Handle concurrent update conflict
      await handleUpdateConflict(customerId, data);
    }
    throw error;
  }
}
```

4. **TTL Management**: The TTL attribute enables automatic cleanup of expired items:

```
// Calculate TTL based on item type
function calculateTTL(itemType) {
  const ttlMap = {
    'customer': 86400,      // 24 hours
    'subscription': 3600,   // 1 hour
    'payment_method': 7200  // 2 hours
  };
  
  const ttlSeconds = ttlMap[itemType] || 86400;
  return Math.floor(Date.now() / 1000) + ttlSeconds;
}
```

This table design provides a flexible and scalable foundation for caching Stripe API responses while supporting various access patterns and maintaining data integrity. The combination of composite keys, GSI, and TTL enables efficient queries and automatic cleanup of stale data.

### Cache invalidation strategy

[Cache invalidation](https://en.wikipedia.org/wiki/Cache_invalidation) is important for maintaining data consistency. You can implement a webhook-based invalidation strategy, enabling you to invalidate individual items when new data arrives from Stripe:

```
async function handleStripeWebhook(event) {
  const { type, data } = event;
  
  // Determine cache keys to invalidate based on event type
  const keysToInvalidate = getCacheKeysForEvent(type, data);
  
  // Invalidate Redis cache
  const redis = getRedisClient();
  await Promise.all(
    keysToInvalidate.map(key => redis.del(key))
  );
  
  // Invalidate DynamoDB cache by updating TTL
  await Promise.all(
    keysToInvalidate.map(key => 
      dynamodb.update({
        TableName: 'stripe-cache',
        Key: { pk: key },
        UpdateExpression: 'set ttl = :ttl',
        ExpressionAttributeValues: {
          ':ttl': Math.floor(Date.now() / 1000)
        }
      }).promise()
    )
  );
}
```

### Performance optimization tips

There are a number of steps that can help improve the performance of this approach:

1. **Redis Connection Pooling**: Configure your Redis client with appropriate connection pool settings for Lambda:

```
const redisOptions = {
  maxRetriesPerRequest: 1,
  enableReadyCheck: false,
  connectTimeout: 500,
  disconnectTimeout: 2000
};
```

2. **DynamoDB Capacity Planning**: Use [on-demand capacity mode](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/on-demand-capacity-mode.html) for unpredictable workloads, or carefully provision capacity based on your access patterns:

```
const tableParams = {
  BillingMode: 'PAY_PER_REQUEST',
  // Or for provisioned capacity:
  ProvisionedThroughput: {
    ReadCapacityUnits: 100,
    WriteCapacityUnits: 50
  }
};
```

3. **Lambda Configuration**: Optimize memory allocation and timeout settings. Memory is a component of Lambda billing, so ensure your functions are not allocated more memory than necessary. Timeout settings can ensure that a function is terminated if it unexpectedly takes too long. Both of these settings can help reduce costs, especially in high throughput workloads. These can be set in CloudFormation and other infrastructure-as-code (IaC) tools, as well as the Lambda console:

```
Resources:
  StripeCacheFunction:
    Type: AWS::Serverless::Function
    Properties:
      MemorySize: 1024
      Timeout: 10
```

## Cost considerations

For a distributed caching solution for Stripe API integration, several key AWS service costs must be carefully managed. ElastiCache nodes accrue charges on an hourly basis whether you're actively using them or not. This forms one of your primary ongoing expenses, alongside the dual costs of DynamoDB – both for storing your cached data and for the request units consumed during your read and write operations. Your serverless compute layer, running on Lambda, adds another dimension to the cost structure through execution time and memory allocation charges. Tying all of these components together is the often-overlooked cost of data transfer between services, which can become significant in high-throughput systems.

Fortunately, several strategies can help optimize these costs without compromising performance. The foundation of cost optimization starts with right-sizing your ElastiCache nodes – selecting instance types that match your actual working set size rather than overprovisioning for potential future growth. This goes hand-in-hand with implementing intelligent TTL policies across both your cache layers, ensuring that stale data doesn't consume costly storage space in DynamoDB or memory in ElastiCache. For your Lambda functions, implementing batch processing patterns can significantly reduce the number of invocations and their associated costs. Perhaps most importantly, maintaining visibility into your cache hit rates through careful monitoring allows you to continuously adjust your storage allocations and caching policies based on real usage patterns, ensuring you're not paying for capacity you don't need while maintaining optimal performance for your users.

## Conclusion

This example caching architecture provides a solution for scaling Stripe API access in high-volume applications. By using ElastiCache, DynamoDB, and Lambda effectively, you can significantly reduce API latency and costs while maintaining data consistency and respecting rate limits.

Remember to:

1. Regularly monitor and adjust cache TTLs based on your data freshness requirements.

2. Implement proper error handling and fallback mechanisms.

3. Keep your Stripe webhook endpoints updated for cache invalidation.

4. Monitor costs across all services to ensure optimal resource utilization.

Additional Resources:

- [Stripe API Documentation](https://stripe.com/docs/api)

- [AWS ElastiCache Best Practices](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/BestPractices.html)

- [DynamoDB Time to Live](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/TTL.html)

- [AWS Lambda Best Practices](https://docs.aws.amazon.com/lambda/latest/dg/best-practices.html)

For more Stripe learning resources, subscribe to our [YouTube channel](https://www.youtube.com/stripedevelopers).
