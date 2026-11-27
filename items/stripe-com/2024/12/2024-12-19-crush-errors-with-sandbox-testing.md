---
title: Crush errors with Sandbox testing
link: https://stripe.dev/blog/crush-errors-with-sandbox-testing
source: stripe-com
published: 2024-12-19T00:00:00Z
updated: 2024-12-19T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- David Edoh-Bedi
summary: With Sandboxes, you can reproduce and troubleshoot errors in your integration without impacting live transactions.
content: extracted
html: 2024-12-19-crush-errors-with-sandbox-testing.html
remote_preview:
  url: https://stripe.dev/og/blog/crush-errors-with-sandbox-testing.png
  alt: 'Stripe Developer Blog: Crush errors with Sandbox testing'
extra:
  content:interactive: true
---

Errors are an inevitable reality of any live integration. In a payment flow, it is crucial to adequately handle them to avoid missing out on transactions from your customers. This blog post covers some of the different classes of errors your payment integration may encounter and provides a strategy for handling them. It also shows how you can use the newly introduced [Sandboxes](https://docs.stripe.com/sandboxes) feature to reproduce the errors in an isolated environment.

## **Sandboxes**

One major frustration developers encounter is that Stripe’s live mode and test mode share the same general [account settings](https://dashboard.stripe.com/settings) (such as payment methods, etc), which can’t be independently modified. This can be a hindrance when testing your integration because whatever changes you make to your test mode settings are replicated in live mode. Developers often worked around this limitation by creating dedicated test accounts.

Sandboxes address this problem by allowing you to spin up a new environment completely isolated from your live mode. This way, you can make modifications and test various configurations without the risk of impacting your live payment traffic. You can access Sandboxes by clicking on your account list and selecting “Sandboxes” in the dropdown. . ![](https://stripe.dev/images/crush-errors-with-sandbox-testing/image_1.png)

The following example shows how you may use Sandboxes to reproduce and test payment errors in your integration.

## **Error types**

There are many types of errors your Stripe integration may run into, but they fit in two broad categories:

- Errors due to external events, like network issues or outages.

- Errors due to bugs in your integration.

Regardless of the cause, your integration must be prepared to handle errors gracefully to avoid breaking the payment flow and jeopardizing your business.

Consider a payment [integration](https://docs.stripe.com/payments/quickstart?lang=python&platform=web) using Stripe’s UI building blocks (i.e [Elements](https://stripe.com/payments/elements)) as the frontend with a Python server on the backend. On the server side, the following code block is used to create a [Payment Intent](https://docs.stripe.com/api/payment_intents) which initiates the payment process.

```
def create_payment():
	# Create a PaymentIntent with the amount, currency, and a payment method type.
        intent: PaymentIntent = stripe.PaymentIntent.create(
               amount=1000,
               currency='usd',
               automatic_payment_methods={
                   'enabled': True,
               }
```

Creating a Payment Intent is the prerequisite to render the Payment Element at the frontend. It’s possible to hit errors during the Payment Intent creation. If that happens, the code in this example most likely results in the stalled page, with no actionable steps for the customer. This results in a bad user experience for the customer and lost business for the merchant. The process below gives you a solution for preventing this type of scenario.

## **Diagnosing the error**

First, you need to diagnose and understand the error. You can do that using use Stripe’s new Workbench debugging tool, which allows you to monitor all aspects of your integration without leaving the dashboard - check out this [blog post](https://stripe.com/blog/workbench-a-new-way-to-debug-monitor-and-grow-your-stripe-integration) for more information on using Workbench

Starting in the dashboard of your integration, launch Workbench and check out the Errors tab. You see a summarized view of all the recent errors in your integration

![](https://stripe.dev/images/crush-errors-with-sandbox-testing/image_2.png)

Select the most recent error chronologically (i.e the one at the top of the summarized list). In this case, the customer received a [*card\_declined*](https://docs.stripe.com/error-codes#card-declined) error. This happens when the customer’s card is declined by the issuing bank. It’s one of the error types that happens outside of Stripe, but can stall your payment flow if not handled properly.

![](https://stripe.dev/images/crush-errors-with-sandbox-testing/image_3.png)

There’s also a detailed view of the API requests which triggered the error, which can be helpful for more granular debugging.

![](https://stripe.dev/images/crush-errors-with-sandbox-testing/image_4.png)

## **Reproducing the error**

Now that you’ve identified the error, proceed with reproducing it:

1. Navigate to the Sandboxes menu as previously shown and create a new sandbox - make sure that the option to copy settings from your live account is selected. This saves you from having to reconfigure your sandbox to match your live account.

![](https://stripe.dev/images/crush-errors-with-sandbox-testing/image_5.png)

This feature allows you to preserve some settings and make it faster to spin up a test environment, similar to your live mode. This is particularly useful here, given that you want to replicate the exact conditions that led to the error in your live traffic.

For instance, suppose your business operates in France. You’d likely enable “Cartes Bancaires” in your dashboard [payment settings](https://dashboard.stripe.com/test/settings/payment_methods), as it’s a popular payment method in this country. You’re able to carry this setting forward with this feature.

Once your sandbox is created, the settings don’t get synchronized with your live mode. This enables you to modify your test integration as much as you like, without impacting your live settings.

The complete list of settings that can be copied from live mode to a new sandbox is available [here](https://docs.stripe.com/sandboxes/dashboard/sandbox-settings).

2. You are taken to your sandbox once it’s created. One important feature of your sandbox is that, unlike the old test mode, it is completely isolated from your live mode. Any changes you make to it persists only here and don’t affect any of your actual payment traffic.\
    The sandbox is a full Stripe account, so its look and feel should be familiar to you. You can enable Workbench by navigating to the Developers page in your [dashboard setting](https://dashboard.stripe.com/settings), just like you would in a normal account.

![](https://stripe.dev/images/crush-errors-with-sandbox-testing/image_6.png)

3. To simulate the customer flow that triggered the error, you can use one of Stripe’s test cards to reproduce the [card\_declined](https://docs.stripe.com/testing#declined-payments) error. Update your test code to use this payment method and make sure to use the [secret and public keys](https://dashboard.stripe.com/apikeys) from your sandbox, so that your test payment traffic is routed appropriately.
4. You now see the *card\_declined* error in the Logs tab of your sandbox Workbench.

Now that you’ve reproduced the error in your sandbox, proceed to see how to handle it to prevent bugs in your payment flow.

## **Handling the error**

The customer's UI issue arose from the existing code's failure to handle errors properly. At the minimum, your code needs to catch and handle exceptions. This stops your application from crashing if an error occurs which prevents Stripe from proceeding through the payment flow.

The updated code snippet below shows an example of handling exceptions using Python’s *try/except* syntax. Code samples from other Stripe supported languages are available [here](https://docs.stripe.com/error-handling?lang=ruby#catch-exceptions).

```
def create_payment():
   # Create a PaymentIntent with the amount, currency, and a payment method type.
   try:
           intent: PaymentIntent = stripe.PaymentIntent.create(
               amount=1000,
               currency='usd',
               automatic_payment_methods={
                   'enabled': True,
              }
           )
       # Send PaymentIntent details to the frontend.
       return jsonify({'clientSecret': intent.client_secret})
   except stripe.error.StripeError as e:
       return jsonify({'error': {'message': str(e)}}), 400
   except Exception as e:
       return jsonify({'error': {'message': str(e)}}), 400
```

It’s written to handle the different types of errors your integration might encounter:

- The *stripe.error.CardError* handles the *card\_declined* error you diagnosed previously.

- The *stripe.error.InvalidRequestError* for invalid requests in your code.

- A generic *Exception* to address all other cases.

With this code configuration, you handle the 2 broad categories of errors [discussed earlier in the post](https://stripe.dev/blog/crush-errors-with-sandbox-testing#error-types), i.e. those errors due to external events (the generic *Exception)* and those due to code bugs (*InvalidRequestError).*

## **Sandbox management**

There is currently a limit of 5 sandboxes per Stripe account, so you may need to reclaim some of them once your updated error handling code is deployed to your production environment. You have a couple of options:

1. Delete the sandbox - this frees up space for a new sandbox. You can do this by navigating to the Sandboxes list as previously shown and clicking the recycle bin icon on the sandbox you want to delete.

![](https://stripe.dev/images/crush-errors-with-sandbox-testing/image_7.png)

2. Delete the test data and preserve your sandbox - the advantage here is that your settings are preserved, so you wouldn’t have to start from scratch if you want to run more tests in an environment like your previous one. You can delete test data from the Workbench Overview tab.

![](https://stripe.dev/images/crush-errors-with-sandbox-testing/image_8.png)

## **Conclusion**

Using Sandboxes, you’re able to reproduce errors from your live integration in a new isolated test environment. This improves on the old test mode which shared synchronized settings with your live mode. With this updated approach, you can test your fixes and change settings without impacting your live payment traffic and without needing to create a new separate Stripe account just for testing.

To prevent various errors from crashing your payment flow, you should handle exceptions at the very least. Other [error handling](https://docs.stripe.com/error-handling) strategies can be found here.

To learn more about developing applications with Stripe, visit our [YouTube Channel](https://www.youtube.com/stripedevelopers).
