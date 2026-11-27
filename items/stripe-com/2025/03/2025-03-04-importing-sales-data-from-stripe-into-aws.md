---
title: Importing sales data from Stripe into AWS
link: https://stripe.dev/blog/importing-sales-data-from-stripe-into-aws
source: stripe-com
published: 2025-03-04T00:00:00Z
updated: 2025-03-04T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Hidetaka Okamoto
summary: This article focuses on how to process Stripe's data in your AWS account with minimal code. You'll learn how to query your business and customer data using SQL, assisted by an LLM (Large Language Model) for query generation. Additionally, it demonstrates how to enhance the security of your data integration by using the native Stripe-AWS integration, specifically the Stripe Event Destination to Amazon EventBridge feature.
content: extracted
html: 2025-03-04-importing-sales-data-from-stripe-into-aws.html
remote_preview:
  url: https://stripe.dev/og/blog/importing-sales-data-from-stripe-into-aws.png
  alt: 'Stripe Developer Blog: Importing sales data from Stripe into AWS'
extra:
  content:interactive: true
---

This article focuses on how to process Stripe's data in your AWS account with minimal code. You'll learn how to query your business and customer data using SQL, assisted by an LLM ([Large Language Model](https://en.wikipedia.org/wiki/Large_language_model)) for query generation. Additionally, it demonstrates how to enhance the security of your data integration by using the native Stripe-AWS integration, specifically the [Stripe Event Destination to Amazon EventBridge](https://docs.stripe.com/event-destinations/eventbridge) feature.

## Using Stripe Sigma to gain business insights

Stripe stores not only the payment data of each user, but also payment-related data, such as subscription contracts and sales information for each plan. You can gain insights from the reports displayed on the dashboard, but sometimes more detailed and complex data is required to support hypotheses in business planning and marketing strategies. This data can help verify the results of implementation.

[Stripe Sigma](https://stripe.com/sigma) allows you to use SQL to obtain data on Stripe. It also provides a SQL generation support function using generation AI, so you can obtain information that can be used for analysis by entering text such as "Tell me the conversion rate from trial to contract" or "Identify users who have not purchased for more than a year".

To run SQL queries, navigate to the Sigma dashboard and access the query execution screen. You can perform analysis using SQL while referring to the schema displayed in the Schema tab.

![](https://stripe.dev/images/importing-sales-data-from-stripe-into-aws/image1.png)

If you're unfamiliar with writing SQL, [Sigma Assistant](https://docs.stripe.com/stripe-data/write-queries#use-assistant) can help. At the top of the SQL input screen, there's a form where you can enter prompts. For example, you could enter: "Please compile the reasons for canceling subscriptions for the past year by product." Sigma Assistant then generates and executes an SQL query based on your input. In this case, it might generate:

```
WITH cancelled_subscriptions AS (
    SELECT
        s.id AS subscription_id,
        s.cancellation_details_reason,
        s.cancellation_details_comment,
        s.cancellation_details_feedback,
        si.price_id
    FROM
        subscriptions s
    JOIN subscription_items si ON s.id = si.subscription_id
    WHERE
        s.status = 'canceled'
        AND s.canceled_at >= DATE_ADD('year', -1, CURRENT_DATE)
),
price_product_mapping AS (
    SELECT
        p.id AS price_id,
        p.product_id
    FROM
        prices p
),
product_info AS (
    SELECT
        pr.product_id,
        prd.name AS product_name
    FROM
        price_product_mapping pr
    JOIN products prd ON pr.product_id = prd.id
)
SELECT
    pi.product_name,
    cs.cancellation_details_reason AS reason,
    COUNT(cs.subscription_id) AS total_cancellations,
    ARRAY_AGG(DISTINCT cs.cancellation_details_comment) AS comments,
    ARRAY_AGG(DISTINCT cs.cancellation_details_feedback) AS feedbacks
FROM
    cancelled_subscriptions cs
JOIN price_product_mapping ppm ON cs.price_id = ppm.price_id
JOIN product_info pi ON ppm.product_id = pi.product_id
GROUP BY
    pi.product_name,
    cs.cancellation_details_reason
ORDER BY
    total_cancellations DESC
```

You can also check the results of SQL execution on the Sigma dashboard. Here you can review whether the data you want to analyze has been correctly retrieved.

![](https://stripe.dev/images/importing-sales-data-from-stripe-into-aws/image2.png)

## Synchronizing report data to AWS without code

Stripe Sigma analyses can be [scheduled daily, weekly, monthly, etc](https://docs.stripe.com/stripe-data/schedule-queries). In this case, when data acquisition is completed, you can receive the execution results in the Webhook event of `sigma.scheduled_query_run.created`. In a Node.js application, you can obtain the analysis results with the following code:

```
// This is a public sample test API key.
// Don't submit any personally identifiable information in requests made with this key.
// Sign in to see your own test API key embedded in code samples.
const stripe = require('stripe')(process.env.STRIPE_SECRET_API_KEY);
// Replace this endpoint secret with your endpoint's unique secret
// If you are testing with the CLI, find the secret by running 'stripe listen'
// If you are using an endpoint defined with the API or dashboard, look in your webhook settings
// at https://dashboard.stripe.com/webhooks
const endpointSecret = 'whsec_...';
const express = require('express');
const app = express();

app.post('/webhook', express.raw({type: 'application/json'}), (request, response) => {
  let event = request.body;
  // Only verify the event if you have an endpoint secret defined.
  // Otherwise use the basic event deserialized with JSON.parse
  if (endpointSecret) {
    // Get the signature sent by Stripe
    const signature = request.headers['stripe-signature'];
    try {
      event = stripe.webhooks.constructEvent(
        request.body,
        signature,
        endpointSecret
      );
    } catch (err) {
      console.log(`⚠️  Webhook signature verification failed.`, err.message);
      return response.sendStatus(400);
    }
  }

  // Handle the event
  switch (event.type) {
    case 'sigma.scheduled_query_run.created':
      const report = event.data.object;
      // await handleSigmaScheduledReport(report);
      break;
    default:
      // Unexpected event type
      console.log(`Unhandled event type ${event.type}.`);
  }

  // Return a 200 response to acknowledge receipt of the event
  response.send();
});

app.listen(4242, () => console.log('Running on port 4242'));
```

One of the reasons why the Webhook API source code can be complex is security. Almost all Webhook APIs provided by SaaS services require a public HTTP API. This means attackers could potentially call this API to attack your service by sending malicious data if they understand its usage or purpose. Therefore, you must protect your Webhook API from attackers by adding verification for request and event data.

If you're using AWS and a serverless stack like [AWS Lambda](https://aws.amazon.com/lambda/) or [AWS Step Functions](https://aws.amazon.com/step-functions/) to build your workload, you can simplify this process by using [Stripe Event Destinations](https://docs.stripe.com/event-destinations/eventbridge) and [Amazon EventBridge](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-what-is.html). With this setup, Stripe sends your event data directly into your AWS account. Let's try integrating this to capture report data from Stripe Sigma.

Open the [Stripe Workbench](https://docs.stripe.com/workbench) and visit the "Event destinations" tab. You can start creating a new Webhook endpoint by clicking the 'Create an event destination' button.

![](https://stripe.dev/images/importing-sales-data-from-stripe-into-aws/image3.png)

You can choose the event type that Stripe will send to your AWS account or webhook API endpoint. If you want to subscribe to the event that occurs when Sigma succeeds in creating a new report, you should choose the `sigma.scheduled_query_run.created` event at this step. Moreover, if you want to get reports regarding your connected accounts, click **Connected accounts** at the top of this form.

![](https://stripe.dev/images/importing-sales-data-from-stripe-into-aws/image4.png)

Then, you can choose the event destination. If you selected EventBridge, you need to enter your AWS account number and the region where you want to process the data. It's helpful to use a descriptive destination name to easily identify the purpose of each destination for future modifications or deletions.

![](https://stripe.dev/images/importing-sales-data-from-stripe-into-aws/image5.png)

![](https://stripe.dev/images/importing-sales-data-from-stripe-into-aws/image6.png)

Stripe starts to create a new event bus in your AWS account as a Partner event source. You must click **Associate AWS partner event source** to accept this request.

![](https://stripe.dev/images/importing-sales-data-from-stripe-into-aws/image7.png)

Once this event bus is created in your AWS account, you can build a custom workload in your AWS environment. Create a new event rule on this event bus and apply the event pattern JSON to it:

```
{
  "source": [{
    "prefix": "aws.partner/stripe.com"
  }],
  "detail-type": ["sigma.scheduled_query_run.created"]
}
```

You can then execute AWS resources like Lambda, Step Functions, and more, triggered from Stripe's events through EventBridge.

![](https://stripe.dev/images/importing-sales-data-from-stripe-into-aws/image8.png)

Now let's build a simple example workflow using Lambda. This function sends a notification containing the report details. The function is triggered by EventBridge rules and downloads the report result sent from the Event Destinations. It then converts the CSV data to JSON format for sending a notification via [Amazon SNS](https://aws.amazon.com/sns/). This example can help you understand how to download the report result and parse it for processing according to your business needs.

```
import https from 'https';
import { SNSClient, PublishCommand } from "@aws-sdk/client-sns";
export const handler = async (event) => {
   try {
       // Get the file URL from the event details
       const fileUrl = event.detail.data.object.file.url;

       // Stripe API key (preferably from an environment variable)
       const stripeApiKey = process.env.STRIPE_API_KEY;

       // Download the file
       const csvData = await downloadFile(fileUrl, stripeApiKey);

       // Convert the CSV data to a JS object
       const results = parseCSV(csvData);

       // Send a message to SNS
       await sendToSNS(results);

       return {
           statusCode: 200,
           body: JSON.stringify(results)
       };
   } catch (error) {
       console.error('Error:', error);
       return {
           statusCode: 500,
           body: JSON.stringify({ error: 'An error occurred while processing the file.' })
       };
   }
};

function downloadFile(url, apiKey) {
   return new Promise((resolve, reject) => {
       const options = {
           headers: {
               'Authorization': `Bearer ${apiKey}`
           }
       };

       https.get(url, options, (res) => {
           let data = '';
           res.on('data', (chunk) => {
               data += chunk;
           });
           res.on('end', () => {
               resolve(data);
           });
       }).on('error', (error) => {
           reject(error);
       });
   });
}
function parseCSV(csvString) {
   const lines = csvString.split('\n');
   const headers = parseLine(lines[0]);
   const results = [];

   for (let i = 1; i < lines.length; i++) {
       if (lines[i].trim() === '') continue;
       const values = parseLine(lines[i]);
       const obj = {};
       for (let j = 0; j < headers.length; j++) {
           obj[headers[j]] = j < values.length ? values[j] : '';
       }
       results.push(obj);
   }

   return results;
}

function parseLine(line) {
   const re = /(?:^|,)(?:"([^"]*(?:""[^"]*)*)"|([^,]*))/g;
   const result = [];
   let matches;
   while ((matches = re.exec(line)) !== null) {
       if (matches[1] !== undefined) {
           // Field surrounded by double quotes
           result.push(matches[1].replace(/""/g, '"').trim());
       } else {
           // Field not surrounded by double quotes
           result.push(matches[2].trim());
       }
   }
   return result;
}
async function sendToSNS(data) {
    const snsClient = new SNSClient({ region: process.env.AWS_REGION });
    const topicArn = process.env.SNS_TOPIC_ARN;

    const params = {
        TopicArn: topicArn,
        Message: JSON.stringify(data),
        Subject: 'Processed CSV Data'
    };

    try {
        await snsClient.send(new PublishCommand(params));
        console.log(`Message successfully sent to SNS topic: ${topicArn}`);
    } catch (error) {
        console.error('Error sending message to SNS:', error);
        throw error;
    }
}
```

As you can see, the integration between Stripe and AWS helps you build your workflow more simply and effectively. You don't need to set up and secure a separate Webhook API for Stripe, as Stripe sends your event data directly into your AWS account. This allows you to focus solely on creating event rules and data processing workflows. Additionally, Stripe Sigma helps you analyze your business and customer data more effectively. Sigma and Event Destinations will share the report CSV data with your AWS account, enabling you to process it using AWS services such as [Lambda](https://aws.amazon.com/lambda/), [Step Functions](https://aws.amazon.com/step-functions/), or [Glue](https://aws.amazon.com/glue/).

## Conclusion

In a competitive business, it is essential to analyze business data such as identifying loyal users, upselling and cross-selling opportunities, and factors affecting cash flow like contract trends and payment issues leading to cancellations.

Stripe can provide essential information on payments, billing management, and subscription contracts. Stripe Sigma allows you to easily collect and search the data you need for analysis. Many businesses collect information from multiple data sources, such as application usage and email subscription rates, for more detailed analysis.

Users who have built an analysis platform on AWS can send information collected with Stripe Sigma to AWS without coding. You can build a platform that enables quick analysis of applications, development of marketing plans, price revisions, and consideration of new pricing models by using Stripe data transferred to [Amazon S3](https://aws.amazon.com/s3/) through Amazon EventBridge.

To learn more about developing applications with Stripe, visit our [YouTube Channel](https://www.youtube.com/stripedevelopers).

[![](https://stripe.dev/images/YouTube.png)](https://www.youtube.com/stripedevelopers)
