---
title: Testing Connect onboarding with Sandboxes
link: https://stripe.dev/blog/testing-connect-onboarding-with-sandboxes
source: stripe-com
published: 2024-12-19T00:00:00Z
updated: 2024-12-19T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- David Edoh-Bedi
summary: As a platform, you can accelerate your Connect development with Sandboxes, by replicating your live settings to test out the merchant onboarding flow.
content: extracted
html: 2024-12-19-testing-connect-onboarding-with-sandboxes.html
remote_preview:
  url: https://stripe.dev/og/blog/testing-connect-onboarding-with-sandboxes.png
  alt: 'Stripe Developer Blog: Testing Connect onboarding with Sandboxes'
---

Developers often tell us they face some challenges while testing Stripe Connect. When you take into account different aspects of a marketplace business such as payment methods, various US states and/or international countries supported, the Connect test matrix can get quite unwieldy. Additionally, the existing test mode that comes with each Stripe account maintains synchronized settings with live mode, making it challenging to test your integration independently without impacting live payment traffic.

Stripe’s new Sandbox feature aims to make this easier by providing an isolated way for platform developers to replicate their live setup for testing, while keeping the two environments separate. This blog post shows how you can use Sandboxes to accelerate your Connect testing.

## **Setting up Connect**

Before you can use Connect to accept payments for your business, there are 2 major configuration steps you must take:

1. First, you need to set up and activate your actual Stripe account itself: this means providing information about yourself and your business, such as address, ownership, bank account for payouts etc. As part of this account setup process, Stripe performs various verification checks to ensure that your business is in compliance with financial regulations. For instance, you are required to provide a valid address that Stripe validates.

2. After successfully setting up your Stripe account, you can then configure it to be a Connect platform. This is done using the Connect onboarding wizard which you can access [in your dashboard](https://dashboard.stripe.com/connect/accounts/overview). It walks you through all the key steps in configuring a Connect platform and provides a [custom integration guide](https://docs.stripe.com/connect/onboarding/quickstart#init-stripe) with code at the end of the process. For more information, check out the [video](https://www.youtube.com/watch?v=UtN3c6hnils) on Connect onboarding, and the [Connect documentation](https://docs.stripe.com/connect/onboarding/quickstart).

   Once you’re done configuring your platform, the wizard presents you with a summary of your setup. Consider a platform with the following configuration.

![](https://stripe.dev/images/testing-connect-onboarding-with-sandboxes/image_1.png)

You can use the new [Workbench debugging tool](https://docs.stripe.com/workbench) to view the underlying JSON object corresponding to your platform account. To do that, first make sure Workbench is enabled by navigating to Settings > Developers > Workbench in your dashboard, then launch Workbench, navigate to the Inspector tab and insert the ID of your platform account. You can copy your account ID from Settings > Business > Account Details.

You can even use API Explorer in Workbench to directly edit your account - if you want to update your email address, for instance. Check other posts on our [developer blog](https://stripe.dev/) for more on how to use Workbench.

![](https://stripe.dev/images/testing-connect-onboarding-with-sandboxes/image_2.png)

## **Onboarding accounts to your platform**

Once your platform account is activated, you can proceed with onboarding connected accounts. Depending on your platform settings, this onboarding process either involves merchants going through a Stripe provided onboarding flow or a custom one that you have built. The previously mentioned example uses Stripe [embedded components](https://docs.stripe.com/connect/get-started-connect-embedded-components) as the building blocks for its onboarding process. Similar to what you did with your platform, onboarding merchant accounts involves them providing various details to register their businesses with Stripe.

The account verification process is a common stumbling block for businesses as they onboard, which can delay their ability to fully go live and get paid. This involves issues such as incorrect ID numbers, typos in the website URL and others.

The following screenshot shows a typical example where Stripe cannot verify a merchant’s website, which means they’re blocked from conducting business.

![](https://stripe.dev/images/testing-connect-onboarding-with-sandboxes/image_3.png)

You can use Sandboxes to reproduce the verification issue, update your onboarding flow as needed and send your merchant back through the verification process so they can update their website.

## **Creating Sandboxes**

With Sandboxes, you can create an environment completely separate from live mode to simulate payment traffic and test out your integration.

You can also create [Sandboxes](https://dashboard.stripe.com/sandboxes) while replicating the same settings from your live account. Prior to Sandboxes, most users created a separate account and manually configured it to match their live account. In this example, that would have meant creating another Connect platform account from scratch and making sure its settings matched the original live account. This is a longer process, which affects your ability to properly test your integration and can ultimately cause delays and cost your business money. Sandboxes save you from that hassle by creating this new platform account with matching settings for you.

To start the Sandbox creation process, navigate to the account list dropdown on the left hand side of your dashboard, and choose the “Sandboxes” option. On the resulting page, choose the “Create” button and you should be presented with the popup below.

![](https://stripe.dev/images/testing-connect-onboarding-with-sandboxes/image_4.png)

Visually, a Sandbox is just like any other Stripe account. Once yours is created, navigate to the [Connect page](https://dashboard.stripe.com/test/connect/accounts/overview) in its dashboard.

## **Testing Connect in your Sandbox**

Your Connect account list shows up as empty, but you can create some test accounts by using the wizard in the dashboard. To start that process, click on the “Create” button in the upper right-hand corner of the Connect overview page.

In the resulting popup, you are able to validate that the Connect settings which you chose in the live mode onboarding wizard were preserved in the Sandbox. For instance, you can see that the Sandbox platform is set up for Stripe to handle the negative liability, conforming to the live mode configuration. All the other settings were kept as well.

![](https://stripe.dev/images/testing-connect-onboarding-with-sandboxes/image_5.png)

To replicate the issue with URL verification, you can either proceed with the OAuth flow to complete the onboarding process for the connected account, or choose to create a different account type for your testing.

For example, if you choose to create a “custom” account type, you can use the dashboard UI to fill in the required fields to complete the onboarding process. You can set the business website to a [test value](https://docs.stripe.com/connect/testing#test-business-url-verification) of “[https://inaccessible.stripe.com](https://inaccessible.stripe.com)” to reproduce the issue with the URL.

Once you’re able to simulate the merchant’s problem in your Sandbox, you can use either the dashboard or Workbench API Explorer to update the URL field to “[https://accessible.stripe.com”](https://accessible.stripe.com%E2%80%9D), which is the test value for simulating a functioning business website.

Now that you’ve confirmed that your onboarding flow works and you’re able to successfully update the URL in your sandbox, you can send the merchant back through the onboarding flow in production so they can update the URL on their end and get unblocked processing their payments.

## **Conclusion**

With Sandboxes, Connect platforms can replicate the exact settings from their live environment to test out the onboarding flow that their merchants go through. Just like in the old test mode, platforms can use all the existing [Connect test materials](https://docs.stripe.com/connect/testing) to simulate failure conditions in their Sandboxes. After reproducing and testing a fix in their Sandbox, platforms can port it to their production environment to make sure their merchants are able to update their accounts as needed so they can conduct their business.

To learn more about developing applications with Stripe, visit our [YouTube Channel](https://www.youtube.com/stripedevelopers).
