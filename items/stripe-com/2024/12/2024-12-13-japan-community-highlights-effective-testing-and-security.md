---
title: 'Japan community highlights: Effective testing and security'
link: https://stripe.dev/blog/japan-community-highlights-2024-09
source: stripe-com
published: 2024-12-13T00:00:00Z
updated: 2024-12-13T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Hidetaka Okamoto
summary: This article highlights development tips for efficiently developing and operating Stripe at a lower cost, as presented by the Japanese Stripe user community JP_Stripes. We'll focus on content from two events held in September 2024 in Aizuwakamatsu, Fukushima Prefecture, and Sapporo, Hokkaido.
content: extracted
html: 2024-12-13-japan-community-highlights-effective-testing-and-security.html
remote_preview:
  url: https://stripe.dev/og/blog/japan-community-highlights-2024-09.png
  alt: 'Stripe Developer Blog: Japan community highlights: Effective testing and security'
extra:
  content:interactive: true
---

This article highlights development tips for efficiently developing and operating Stripe at a lower cost, as presented by the Japanese Stripe user community "JP\_Stripes". We'll focus on content from two events held in September 2024 in Aizuwakamatsu, Fukushima Prefecture, and Sapporo, Hokkaido. You'll learn about Stripe's new development environment, Sandbox, how to streamline product launches using Stripe Connect, and methods to address credit card fraud and the resulting revenue decline, all based on real-world cases. Discover insights from long-time Stripe users and in-house experts, based on actual projects and experiences, to find hints for improving your ongoing projects and web services.

At an event in Aizuwakamatsu, Fukushima Prefecture, a developer from a local Mobility as a Service company shared insights on testing Stripe-integrated applications. The session highlighted the challenges of E2E testing for features that interact with external services, such as increased test duration, flaky tests due to rate limits, and difficulty reproducing error scenarios.

![](https://stripe.dev/images/japan-community-highlights-2024-09/image1.jpg) [https://x.com/hidetaka_dev/status/1837079154245914739/photo/1](https://x.com/hidetaka_dev/status/1837079154245914739/photo/1)

However, these blockers were largely non-existent when using Stripe. The presenter emphasized that Stripe's rich API allows for intentionally triggering error cases like payment failures, reproducing state transitions using only the API, and smooth design, implementation, and operation of E2E tests.

The speaker also noted that using actual service APIs for testing is feasible due to high rate limits and the availability of CI-specific Sandboxes. This prevents noise from test-created resources during development and debugging. This approach significantly simplifies the testing process for Stripe-integrated applications, allowing developers to create more robust and reliable tests with less effort.

### Sandboxes helps developers to improve their development and test flow

As introduced in this session, Stripe offers sandboxes where you can set up to five test environments for different purposes. By using these sandboxes, you can prevent common issues that occur during team development, such as webhook events triggered by other developers becoming noise, or unintentionally modifying existing resources and interfering with the development of other features.

Furthermore, you can set developer access rights for each sandbox. Create users with the Sandbox User role from [Settings > Team](https://dashboard.stripe.com/settings/team) and security. [Sandbox User role](https://docs.stripe.com/get-started/account/teams/roles#sandbox_user) users are not allowed to operate or view the production account. By using this when external or partner company developers implement or investigate Stripe integration, you can prevent risks such as leakage of important data.

![](https://stripe.dev/images/japan-community-highlights-2024-09/image2.png)

If you are developing an API that integrates with Stripe Webhooks in a sandbox, the Stripe CLI command to forward events to your local API changes slightly. Run the [*stripe preview use* comman](https://docs.stripe.com/sandboxes/dashboard/manage#switch-to-a-sandbox-in-the-cli)d to specify the sandbox account from which you want to receive events.

```
stripe preview use

You are currently operating on Default Sandbox ({{SANDBOX_ID}})
* indicates your active workspace. Use the arrow keys to navigate: ↓ ↑ → ←
? Select the sandbox you'd like to use:
  ▸  * Default Sandbox      {{SANDBOX_ID}}
       QA Team Sandbox      {{SANDBOX_ID}}
       Developer Sandbox    {{SANDBOX_ID}}
```

## Rapid Platform Development with Stripe Connect

At an event in Sapporo, Hokkaido, the CEO of a content monetization platform (codoc) built using Stripe Connect shared their experience. They revealed that their platform was launched by a team of three in just nine months using Stripe Connect. To ensure smooth user onboarding and content monetization, they implemented two key strategies. First, they staggered the timing between service account creation and Stripe Connect account creation. Second, they implemented embedded UIs for payouts and payment management within the service dashboard.

These approaches allowed users to seamlessly integrate with the platform and start monetizing their content quickly. The speaker emphasized how Stripe Connect's features enabled them to focus on their core business logic while relying on Stripe for complex payment processing and management tasks.

### Leveraging Embedded Component reduces time to market

As of 2024, Stripe Connect has significantly simplified the implementation of its features into service dashboards. Traditional methods required developers to fetch data using Stripe API and implement UI based on that information. The new approach for platforms allows for substantial reduction in UI implementation costs and effort for feature additions by [embedding Stripe-provided iframes](https://docs.stripe.com/connect/supported-embedded-components) into the site.

![](https://stripe.dev/images/japan-community-highlights-2024-09/image3.png)

To embed the iframe, all you need is the Connected Account ID from Stripe Connect. Retrieve this ID from your user database, then specify the data and operational features you want to display on the user's page through parameters.

```
app.post('/account_session', async (req, res) => {
 try {
   const accountSession = await stripe.accountSessions.create({
     account: "{{CONNECTED_ACCOUNT_ID}}",
     components: {
       payments: {
         enabled: true,
         features: {
           refund_management: true,
           dispute_management: true,
           capture_payments: true,
         }
       }
     }
   });
   res.json({
     client_secret: accountSession.client_secret,
   });
 } catch (error) {
   console.error('An error occurred when calling the Stripe API to create an account session', error);
   res.status(500);
   res.send({error: error.message});
 }
});
```

After creating a session on the server-side API and obtaining the client\_secret, pass the data to the [loadConnectAndInitialize()](https://docs.stripe.com/connect/get-started-connect-embedded-components#load-and-initialize-connect.js) function provided by Stripe.js. Finally, specify the HTML tag where you want to mount the component.

```
import { loadConnectAndInitialize } from "@stripe/connect-js";
const fetchClientSecret = async () => {
 // Fetch the AccountSession client secret
 const response = await fetch('/account_session', { method: "POST" });
 const {client_secret: clientSecret} = await response.json();
 document.querySelector('#container').removeAttribute('hidden');
 document.querySelector('#error').setAttribute('hidden', '');
 return clientSecret;
}
 const instance = loadConnectAndInitialize({
   // This is your test publishable API key.
   publishableKey: "pk_test_xxxx",
   fetchClientSecret: fetchClientSecret,
   appearance: {
     overlays: 'dialog',
     variables: {
       colorPrimary: '#625afa',
     },
   },
     });
     const container = document.getElementById("container");
     const paymentsComponent = instance.create("payments");
     container.appendChild(paymentsComponent);
```

By utilizing Stripe Connect in this way to provide your platform, you can create three types of user experiences: redirect, component embedding, and full scratch implementation. Choose the integration method based on your business stage, development resources, and the user experience you wish to provide.

This approach not only streamlines the development process but also ensures that your platform stays up-to-date with the latest Stripe Connect features without requiring constant updates to your custom UI.

## Early Fraud Detection Using Webhooks

A Stripe Technical Account Manager presented a session on fraud prevention and 3D Secure authentication, addressing the mandatory 3D Secure authentication requirement in Japan from March 2025 and Stripe's fraud prevention capabilities.

![](https://stripe.dev/images/japan-community-highlights-2024-09/image4.jpg)\
 [https://x.com/hide69oz/status/1839629967648493925/photo/1](https://x.com/hide69oz/status/1839629967648493925/photo/1)

The session highlighted the significant increase and sophistication of credit card fraud, as well as the issue of false positives in fraud prevention tools. These false positives can lead to failed legitimate orders and potential customer loss, with up to 75% of users experiencing failures on valid orders and 40% of those users stating they wouldn't attempt to purchase from that site again.

To address these challenges efficiently, the presenter recommended enabling Stripe's fraud prevention tools (Radar / Radar for Teams) and implementing automation using Stripe Webhooks and APIs. A key strategy for fraud detection involves utilizing [Early fraud warning](https://docs.stripe.com/api/radar/early_fraud_warnings) Webhook events (radar.early\_fraud\_warning.created and radar.early\_fraud\_warning.updated) to identify high-risk transactions. For cases where the dispute fee exceeds the transaction amount, businesses can preemptively cancel orders and issue refunds to minimize fraud-related costs and occurrences.

By combining Stripe's features with Webhooks and APIs, developers can mitigate the impact of fraud while reducing response costs and additional fees. This approach allows businesses to tackle the growing global issue of fraud more effectively, minimizing both direct financial losses and the indirect costs associated with fraud prevention and management.

## Conclusion

Participating in Stripe's developer community provides valuable insights from experienced users who have developed and operated e-commerce and SaaS applications using Stripe. This knowledge can help you optimize your projects, increase sales, and improve operational efficiency. As community members share their experiences, it creates a cycle of continuous learning and improvement.

Stripe offers various community engagement options:

1. User communities: [JP\_Stripes in Japan](https://jpstripes.connpass.com/).
2. Developer events: Hosted by the Developer Advocate team in the US, UK, and other countries [Meetup group](https://www.meetup.com/pro/stripe/).
3. Online communication: the [Discord group](https://discord.gg/stripe) and [Stripe Insiders](https://insiders.stripe.dev/) forum.

To get involved, choose the format and area that best suits your interests. By engaging with these communities, you can stay updated on Stripe's latest developments and best practices, enhancing your skills and knowledge as a developer. To learn more about developing applications with Stripe, visit our [YouTube Channel](https://www.youtube.com/stripedevelopers).

[![](https://stripe.dev/images/YouTube.png)](https://www.youtube.com/stripedevelopers) [https://www.youtube.com/stripedevelopers](https://www.youtube.com/stripedevelopers)
