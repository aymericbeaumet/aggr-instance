---
title: Enhance your monitoring by integrating Stripe events with AWS CloudWatch Log Groups
link: https://stripe.dev/blog/enhance-your-monitoring-by-integrating-stripe-events-with-aws-cloudwatch-log-groups
source: stripe-com
published: 2024-12-09T00:00:00Z
updated: 2024-12-09T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Andrew Robinson
summary: Integrating Stripe events with AWS CloudWatch Log Groups enhances monitoring by enabling real-time tracking and analysis of Stripe transactions. This setup allows for seamless transfer of Stripe data to CloudWatch via Amazon EventBridge, streamlining payment operations. It provides the tools to visualize, monitor, and respond to events efficiently, optimizing strategic decisions.
content: extracted
html: 2024-12-09-enhance-your-monitoring-by-integrating-stripe-events-with.html
remote_preview:
  url: https://stripe.dev/og/blog/enhance-your-monitoring-by-integrating-stripe-events-with-aws-cloudwatch-log-groups.png
  alt: 'Stripe Developer Blog: Enhance your monitoring by integrating Stripe events with AWS CloudWatch Log Groups'
---

Stripe events are an essential tool for notifying you about changes to objects in your Stripe account—whether it's a successful charge, a failed invoice payment, or an available reconciliation report.

These events, triggered whenever the status of an object changes, are critical for tracking the lifecycle of Stripe objects and building event-driven processes. For instance, they enable automatic provisioning of services once a customer completes their subscription payment, or to automatically start preparing an order for shipment. Additionally, maintaining your own records of these events supports long-term analysis and troubleshooting. While Stripe retains events for 13 months, those older than 30 days are only accessible as summaries, and you might have a business need to store these events for a longer period of time.

With Stripe's recent integration with [Amazon EventBridge](https://aws.amazon.com/eventbridge/), routing these events directly into your AWS account is now seamless, freeing you from managing the underlying logic. EventBridge allows you to integrate with over 20 AWS services, such as [AWS Lambda](https://aws.amazon.com/lambda/) for serverless processing or [Amazon SQS](https://aws.amazon.com/sqs/) for queue management, to enable asynchronous processing, allowing for decoupling of the publisher and processor of events to help support increased scalability.

If you require extended retention and analysis capabilities, integrating Amazon EventBridge with [Amazon CloudWatch](https://aws.amazon.com/cloudwatch/) offers a powerful solution. Using the [Log Groups feature](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/Working-with-log-groups-and-streams.html) in Amazon CloudWatch you can collate sequences of events originating from your Stripe account, enabling you to create advanced filter patterns for targeted insights. For example, if you wanted to build a custom metric based on the number of successful payments, you can write your own filter pattern regex for charge.succeeded events, and then create a custom metric based off of this.

In this post, we will explore how you can leverage this integration to build dynamic dashboards and conduct in-depth analyses of your Stripe events, as show in the following example:

![An example of this integration to have Stripe events into a CloudWatch dashboard](https://stripe.dev/images/enhance-your-monitoring-by-integrating-stripe-events-with-aws-cloudwatch-log-groups/image1.png)

By integrating Stripe events with CloudWatch, you gain enhanced capabilities for monitoring and storing event data. This setup allows you to efficiently process and analyse the data in several ways. You can create CloudWatch alarms to notify you of specific events, ensuring timely responses to critical issues. With [CloudWatch Logs Insights,](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/AnalyzingLogData.html) you can run queries on your log data to identify patterns and anomalies, helping you troubleshoot problems faster. Additionally, you can set up dashboards to visualise trends and metrics related to your Stripe events, making it easier to understand the behaviour of your payment processes. This integration simplifies data handling and provides you with robust tools to turn raw event data into actionable insights for your applications.

## Real-Time Event Monitoring

By visualising key metrics on custom CloudWatch dashboards, you can monitor key financial indicators at a glance. Widgets such as event counts for successful charges or failed payments enable immediate recognition of transaction trends and anomalies. This real-time insight is vital for maintaining the financial health of your operations and provides cues for timely interventions when anomalies occur.

Using metric filters, you can extract and quantify specific event types, creating custom metrics that capture crucial aspects of your Stripe activity. For example, tracking charge.succeeded events allows you to monitor transaction success rates, providing a direct measure of your payment processing efficiency, or tracking invoice.payment\_failed events allows you to monitor failed invoice payments. These metrics can be visualised on dashboards or set as alarms, alerting you to unexpected fluctuations, all in near real time:

![](https://stripe.dev/images/enhance-your-monitoring-by-integrating-stripe-events-with-aws-cloudwatch-log-groups/image2.png)

We can expand on this, for example, if you wanted to build an alarm to notify you for failed invoice payments, this can be achieved using native AWS functionality. To do this you’ll need to go through a few steps which are detailed in this section, at a high level the steps are:

1. Creating a filter pattern to match events
2. Create a metric filter based on that pattern
3. Build relevant alarms based on the value of that metric filter
4. Create notifications from those alarms

### Create a filter pattern

To build real-time monitoring for invoice.payment\_failed events in CloudWatch, start by accessing the CloudWatch console. Here, navigate to the Logs section where you'll select the log group designated for Stripe events. Begin by creating a metric filter designed to detect invoice.payment\_failed events. Craft a filter pattern to match these events accurately, such as { $.type = "invoice.payment\_failed" }. This pattern targets just the logs where an invoice payment has failed, ensuring that only relevant events are captured.

### Create a metric filter

You’ll now need to create a metric filter, provide a suitable name, such as “InvoiceFailedFilter” and then in the metric details provide the metric namespace (to allow you to group similar metrics together in a single namespace), a metric name (to identify this specific metric), and a default value (typically we’d set this to 1), and then create the filter.

Now you’ve got a filter pattern and metric filter created, you can go onto creating the alarms and notifications.

### Create an alarm

Begin by navigating to the CloudWatch console and selecting Alarms from the menu to initiate the process of creating a new alarm. Here, explore the StripeEvents namespace and choose the FailedInvoices metric, which you've set up earlier. Selecting this metric allows you to track the frequency of failed invoices in real time.

With the metric selected, the next step is to configure the alarm settings. Set the observation Period to every 5 minutes or a timeframe that suits your operational needs, ensuring timely incident detection. For Statistics, opt for 'Sum' to monitor the total number of failed events over each period. You will then set a threshold value; this should be a number that indicates an unusually high volume of failures, potentially affecting business processes. You may need to analyse the number of failed invoice payment events you typically expect to see to set this threshold, alternatively you can also look at using the built in [anomaly detection feature](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Create_Anomaly_Detection_Alarm.html) in CloudWatch.

### Create notifications

In the Notification settings, define actions to be triggered when the alarm state changes. Set the alarm to notify stakeholders via Amazon SNS, selecting existing topics or creating new ones for email or SMS alerts. This immediate notification allows teams to respond swiftly to potential issues. Complete the setup by providing a clear and descriptive alarm name and description before creating the alarm.

You should now have an event driven solution to handling Stripe events for failed invoice payments, along with robust monitoring when the number of failed payments breaches a set threshold, allowing you real time insights into potential issues.

## Analysing Long-Term Trends

Visualising long-term trends is key for strategic planning. By charting historical data on transaction volumes and success rates, you can identify seasonal spikes or patterns, informing marketing strategies and operational scaling. This aids in optimising service readiness to align with business cycles and maximise customer satisfaction.

Using CloudWatch Logs Insights enables sophisticated querying of your logs for deeper analysis. Whether performing detailed audits of financial transactions or spotting patterns of recurring errors, Logs Insights empowers you to unlock hidden insights within your data. This analytical depth supports strategic planning and strengthens your operational efficiency.

## Enhancing Troubleshooting and Debugging

Dashboards and Metric Filters are vital for proactive issue resolution. By setting up detailed monitoring of transaction failures or unexpected patterns, you can swiftly pinpoint root causes. For example, by creating a filter pattern for charge.failed events and then building a metric from this, you can see the failed charges in real time, both plotting the historical trend on a graph, but also being able to have real time alerting if the number of failed charges exceeds a set threshold. Real-time alerts coupled with historical data visibility enable you to deploy corrective measures swiftly, minimising downtime and enhancing customer satisfaction.

Detailed analysis of logs using Logs Insights aids in thorough debugging and performance evaluation. Querying error logs for contextual information about transaction failures provides clarity, helping diagnose and rectify issues effectively, ensuring smooth operational continuity.

## Conclusion

By integrating Stripe events with Amazon EventBridge and Amazon CloudWatch, you unlock a powerful suite of tools for monitoring, analysing, and optimising your payment operations. This setup not only enhances your ability to respond to real-time events but also supports long-term strategic initiatives, ensuring your business remains agile and resilient in the face of a dynamic financial landscape.
