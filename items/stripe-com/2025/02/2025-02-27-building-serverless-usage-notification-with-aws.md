---
title: Building serverless usage notification with AWS
link: https://stripe.dev/blog/building-serverless-usage-notification-with-aws
source: stripe-com
published: 2025-02-27T00:00:00Z
updated: 2025-02-27T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Hidetaka Okamoto
summary: In this article, we will guide you on how to set up usage threshold alerts based on consumption for customers with pay-as-you-go plans. By integrating AWS's serverless tools with Stripe, you can create a streamlined and easy-to-manage notification system with minimal coding.
content: extracted
html: 2025-02-27-building-serverless-usage-notification-with-aws.html
remote_preview:
  url: https://stripe.dev/og/blog/building-serverless-usage-notification-with-aws.png
  alt: 'Stripe Developer Blog: Building serverless usage notification with AWS'
extra:
  content:interactive: true
---

In this article, we will guide you on how to set up usage threshold alerts based on consumption for customers with pay-as-you-go plans. By integrating AWS's serverless tools with Stripe, you can create a streamlined and easy-to-manage notification system with minimal coding.

This post introduces a new feature based on Sandbox, a new feature launched in 2024. Therefore, several features and APIs may not be available in the conventional test mode. Please refer to the [Stripe documentation](https://docs.stripe.com/sandboxes/dashboard/manage) and prepare a sandbox environment for testing in advance, as several features and APIs may not be available in the conventional test mode.

## Stripe can set up usage alerts for customers

To establish a usage alert, you typically create a dedicated database table and build a mechanism to monitor customer usage. This often involves aggregating data or employing batch processing to determine if the alert conditions set by customers are met, followed by triggering a notification process. While this approach allows for independence from payment and cloud providers, it requires the maintenance and operation of systems such as databases, aggregation processes, and notification workflows, along with ongoing monitoring.

However, with Stripe, you can bypass the complexity of developing these processes yourself. By using the [Billing Alert API](https://docs.stripe.com/api/billing/alert) provided by Stripe, you can receive webhook notifications for billing alerts, allowing you to promptly inform your customers. To configure an alert for a pay-as-you-go plan, you need the following information:

- `customer_id`: Customer ID for which notifications are to be set.
- `meter_id`: ID for aggregating usage of the plan requiring alerts.
- `usage_threshold`: Threshold for sending notifications to that customer.

Collect this information from the Stripe API or your system's database, then use these values to send an API request:

```
// Set your secret key. Remember to switch to your live secret key in production.
// See your keys here: https://dashboard.stripe.com/apikeys
const stripe = require('stripe')('{{TEST_SECRET_KEY}}');

const alert = await stripe.billing.alerts.create({
  title: 'Usage notification',
  usage_threshold: {
    filters: [
      {
        customer: '{{CUSTOMER_ID}}',
        type: 'customer',
      },
    ],
    meter: 'mtr_test_61RF5DZqblCUECsq641F3VmqeBU0qEMC',
    gte: 100,
    recurrence: 'one_time',
  },
  alert_type: 'usage_threshold',
});
```

If you want to create an alert in another programming language, enter the following command in the **Shell** tab of [Stripe Workbench](https://docs.stripe.com/workbench). You can obtain the SDK code in your desired language by clicking the Print SDK request button.

```
stripe billing alerts create \
 --title="Usage notification" \
 -d "usage_threshold[filters][0][customer]={{CUSTOMER_ID}}"  \
 -d "usage_threshold[filters][0][type]=customer" \
 -d "usage_threshold[meter]=mtr_test_61RF5DZqblCUECsq641F3VmqeBU0qEMC" \
 -d "usage_threshold[gte]=100" \
 -d "alert_type=usage_threshold" \
 -d "usage_threshold[recurrence]=one_time"
```

![](https://stripe.dev/images/building-serverless-usage-notification-with-aws/image1.png)

Let's test whether the alert we created works. First, set up your pay-as-you-go subscription pricing and products according to the [Stripe documentation](https://docs.stripe.com/billing/subscriptions/usage-based/implementation-guide). Then, use the API to send pay-as-you-go usage to Stripe and send the number that exceeds the threshold.

```
curl -X POST https://api.stripe.com/v2/billing/meter_events \
  -H "Authorization: Bearer sk_test_...kni1" \
  -H "Stripe-Version: 2024-09-30.acacia" \
  --json '{
    "event_name": "api_request",
    "payload": {
        "stripe_customer_id": "{{CUSTOMER_ID}}",
        "value": "25"
    }
  }'
```

After executing this command, check the Events tab in Workbench. If you refresh the status, you will see that the `billing.alert.triggered` event has been triggered.

![](https://stripe.dev/images/building-serverless-usage-notification-with-aws/image2.png)

By integrating this event into your system, you can trigger usage alerts without the need for a separate aggregation database.

## Sending alert events to Amazon EventBridge without writing code

Now you can trigger a usage alert event by using Stripe's Billing Alert API. The next step is to integrate with a system to handle the triggered event. When integrating a SaaS application with a system, the Webhook API is usually used to subscribe to the events. However, to prepare a Webhook API, you need to add a process to verify whether the request is coming from the intended service to prevent unauthorized API requests from third parties.

If your system is built on AWS, you can use [Amazon EventBridge](https://aws.amazon.com/eventbridge/) to skip the implementation of this Webhook API and its protection process.

When launching the Workbench in the Stripe Dashboard, you can see the ***Event destinations*** tab on the panel. Note: If you don't see the *Event destinations* tab, it's likely that you're not using a sandbox environment or that you haven't enabled the Amazon EventBridge integration. Check the [Stripe Docs](https://docs.stripe.com/event-destinations/eventbridge) to make sure you're in a test or production environment that you can use and that it's enabled.

![](https://stripe.dev/images/building-serverless-usage-notification-with-aws/image3.png)

Click the **Create an event destination** button to open the Stripe event sending configuration screen. First, you will be asked which type of event you want to send, so select `billing.alert.triggered`.

![](https://stripe.dev/images/building-serverless-usage-notification-with-aws/image4.png)

Next, select the type of event destination. As of the time of writing, you can specify two destinations: (1) **Webhook endpoint**, which specifies an HTTPS REST API URL, and (2) **Amazon EventBridge**, which will be introduced later.

![](https://stripe.dev/images/building-serverless-usage-notification-with-aws/image5.png)

This post uses EventBridge as an example to subscribe to the events. After choosing EventBridge as the destination, enter your AWS account ID and the AWS region in which you will use EventBridge for integration, and the name of the event destination as optional data. There is no need to create an IAM user or role, so use your account ID from the AWS management console. Click the **Create destination** button to prepare Stripe to send events to the specified AWS account and region.

![](https://stripe.dev/images/building-serverless-usage-notification-with-aws/image6.png)

Finally, on the AWS account side, set up EventBridge to accept events sent from Stripe. Click the **Associate AWS partner event source** button displayed on the details page of the created event destination.

![](https://stripe.dev/images/building-serverless-usage-notification-with-aws/image7.png)

This takes you to the screen where you can [associate the EventBridge Partner event source](https://docs.stripe.com/event-destinations/eventbridge#associate-partner-event-source) with the event bus on the AWS management console. Once you complete the association, you can route events triggered by Stripe using EventBridge rules.

![](https://stripe.dev/images/building-serverless-usage-notification-with-aws/image8.png)

## Building usage notifications within AWS

Now you are ready to process events triggered by Stripe on AWS. Finally, let's create a simple demo on AWS to notify an email address when an event is received. To send notifications to email, Slack, etc., it is easy to use [Amazon SNS](https://aws.amazon.com/sns/). Using the AWS CLI, you can easily set up a mechanism to send notifications to any email address.

First, create an SNS topic and subscribe to it with the following commands:

```
aws sns create-topic  \
 --name my-notification-topic aws sns subscribe  \
 --topic-arn <TOPICARN>  \
 --protocol email  \
 --notification-endpoint your-email@example.com
```

Replace `<topicARN\>` with the ARN of the topic you created earlier, and replace [your-email@example.com](mailto:your-email@example.com) with the email address where you want to receive notifications. When you execute the above command, a confirmation email will be sent to the specified email address. Click the link in the email to confirm your subscription.

Next, let's create a new rule using the AWS CLI to handle the billing.alert.triggered event sent from Stripe:

```
aws events put-rule \
 --name "UsageNotification" \
 --event-bus-name "Event Bus name starting with aws.partner/stripe.com/ed_test_" \
 --event-pattern "{"source":[{"prefix":"aws.partner/stripe.com"}],"detail-type":["billing.alert.triggered"]}" \
 --state ENABLED
```

Then, specify the SNS topic as the target of the rule you created:

```
aws events put-targets \
 --rule "UsageNotification" \
 --event-bus-name "Event Bus name starting with aws.partner/stripe.com/ed_test_" \
 --targets "Id"="1","Arn"="<SNS topic ARN>"
```

The connection is now complete. Use the [Stripe Meter Events AP](https://docs.stripe.com/api/v2/billing/meter-event/create)I to send data so that usage exceeds the alert threshold. If the connection is correct, event information will be sent to the email address registered in SNS.

![](https://stripe.dev/images/building-serverless-usage-notification-with-aws/image9.png)

By using [AWS Lambda](https://aws.amazon.com/pm/lambda/) and [Step Functions](https://aws.amazon.com/step-functions/), you can implement a variety of workloads, such as notifying customers by email or reflecting the results in your application. Below is a sample of an email sending process using Lambda and [Amazon SES](https://aws.amazon.com/ses/).

```
import { SendEmailCommand, SESClient } from "@aws-sdk/client-ses";
import Stripe from 'stripe';
const stripe = Stripe(process.env.STRIPE_SECRET_KEY);
const sesClient = new SESClient({ region: "us-east-1" }); 
export const handler = async (event) => {
  try {
    const customer = event.detail.data.object.customer;
    const value = event.detail.data.object.value;

    const stripeCustomer = await stripe.customers.retrieve(customer);
    const email = stripeCustomer.email;

    const subject = "Usage notification";
    const body = `Usage exceeded ${value}!`;
    const sendEmailCommand = new SendEmailCommand({
      Destination: {
        ToAddresses: [email],
      },
      Message: {
        Body: {
          Text: {
            Charset: "UTF-8",
            Data: body,
          },
        },
        Subject: {
          Charset: "UTF-8",
          Data: subject,
        },
      },
      Source: "your-verified-email@example.com",
    });

    const response = await sesClient.send(sendEmailCommand);
    console.log(`Alert email sent to ${email}`);
    return { statusCode: 200, body: 'Email sent successfully', messageId: response.MessageId };

  } catch (error) {
    console.error('Error:', error);
    if (error.name === "MessageRejected") {
      return { statusCode: 400, body: 'Email rejected', error: error.message };
    }
    return { statusCode: 500, body: 'Error processing request', error: error.message };
  }
};
```

This approach allows you to implement a usage notification flow with minimal resources and application code, using AWS services.

## **Conclusion**

A pay-as-you-go pricing model is an effective way to enhance the value of services for customers. This approach allows customers to pay only for what they actually use, helping service providers minimize the risk of contract cancellations that can arise from cost concerns.

However, customers may worry about potential charges exceeding their budgets under a pay-as-you-go model. To address this concern, it's essential to implement a mechanism that notifies customers when their usage approaches an upper limit. In addition to the standard billing process, a system must be developed to monitor customer usage and send alerts. This added complexity may discourage some service providers from embracing a pay-as-you-go model.

Fortunately, when using AWS and Stripe, you can establish a streamlined alert notification system with minimal coding. By using Stripe's Event Destinations to send alert events to Amazon EventBridge, and integrating communication services like Amazon SNS and SES, you can create a mechanism that keeps customers informed before exceeding their budgets.

To learn more about developing applications with Stripe, visit our [YouTube Channel](https://www.youtube.com/stripedevelopers).

[![](https://stripe.dev/images/YouTube.png)](https://www.youtube.com/stripedevelopers) [https://www.youtube.com/stripedevelopers](https://www.youtube.com/stripedevelopers)
