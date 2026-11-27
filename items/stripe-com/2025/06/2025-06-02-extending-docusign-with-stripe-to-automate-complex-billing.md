---
title: Extending Docusign with Stripe to automate complex billing workflows
link: https://stripe.dev/blog/docusign-stripe-integration-automating-billing-workflows
source: stripe-com
published: 2025-06-02T00:00:00Z
updated: 2025-06-02T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Paige Rossi
summary: Discover how to streamline your agreement process by connecting DocuSign with Stripe using extension apps. Learn to build automated billing workflows without complex coding.
content: extracted
html: 2025-06-02-extending-docusign-with-stripe-to-automate-complex-billing.html
remote_preview:
  url: https://stripe.dev/og/blog/docusign-stripe-integration-automating-billing-workflows.png
  alt: 'Stripe Developer Blog: Extending Docusign with Stripe to automate complex billing workflows'
extra:
  content:interactive: true
---

*This post is contributed by Docusign Developer Advocacy.*

As any developer who has integrated Stripe with [Docusign Payments](https://developers.docusign.com/docs/esign-rest-api/esign101/concepts/tabs/payment/) knows, agreements and payments go hand in hand. But payments are just one piece of the larger agreement lifecycle. Historically, Docusign has been the go-to solution for electronic signatures. But once a digital document has been signed, it has often been stored as a static PDF, trapping the valuable agreement data inside. Businesses have had to rely on manual processes to transfer this data across different platforms and systems, often losing economic value in the process.

That’s where the [Docusign Intelligent Agreement Management (IAM) platform](https://www.docusign.com/blog/developers/streamline-end-to-end-agreement-management-with-docusign-a-developer) comes in, seeking to help organizations unlock the value of their agreement data and simplify the complex workflows involved in the agreement lifecycle beyond the signing stage.

A key feature of the IAM platform is [extension apps](https://developers.docusign.com/extension-apps/), which empower developers to build third-party functionality into Docusign. With the [Stripe extension app](https://apps-d.docusign.com/app-center/app/3e8105e3-08a6-48cc-b63a-856954184e65), you can now automate more complex billing workflows to include customer and invoice management in conjunction with other agreement processes inside Docusign. In this post, I’ll give you an overview of extension apps, then walk you through how to configure a workflow featuring the Stripe extension app and trigger it using the [Maestro API](https://developers.docusign.com/docs/maestro-api/).

Agreement processes are rarely limited to eSignature and frequently involve multiple platforms. For example, a common use case that many businesses face is the need to archive documents to a cloud storage system such as [Google Drive](https://workspace.google.com/products/drive/) or [Box](https://www.box.com/home) after signing. Or a business might need to update a system of record such as Salesforce to reflect any updates made in an agreement with Docusign.

In the past, it would require a complex API integration to connect data between Docusign and an external platform like Stripe. Extension apps solve this problem by packaging the external authentication and external API calls into an app that can be triggered from various [extension points](https://developers.docusign.com/extension-apps/extension-apps-101/concepts/extensions-and-extension-points/#extension-points) inside Docusign, including [Maestro workflows](https://developers.docusign.com/docs/maestro-api/maestro101/workflows/).

This demonstrates how extension apps connect an external platform API to a Docusign workflow or envelope:

![](https://stripe.dev/images/docusign-stripe-integration-automating-billing-workflows/image7.png)

Developers can build extension apps for both private and public distribution. If you want your app to be available to any Docusign customer, you can publish it on the [Docusign App Center](https://developers.docusign.com/extension-apps/extension-apps-101/app-center/). But if you have specific business needs that could be met by an extension app using proprietary or internal systems, you can opt to build a [private app](https://www.docusign.com/blog/developers/introducing-private-extension-apps) (beta) that will be shared with only a predefined list of Docusign production accounts.

Hopefully, the wheels inside your head are already turning, and you’re starting to imagine how building an extension app might simplify some of your own existing agreement processes. But if you’re looking to further integrate Stripe and Docusign, you can leverage extension apps to do that without building anything custom. You can install the [Stripe extension app](https://apps-d.docusign.com/app-center/app/3e8105e3-08a6-48cc-b63a-856954184e65) from the Docusign App Center and use it in a Maestro workflow, eliminating the need to write any code at all. The Stripe extension app lets you create and update customer records in Stripe based on agreement data and fields in Docusign. You can also use it to build out steps that generate invoices in Stripe inside your Docusign workflows.

This maps out a common workflow incorporating Docusign steps and the Stripe extension app:

![](https://stripe.dev/images/docusign-stripe-integration-automating-billing-workflows/image2.png)

The workflow above begins when a customer enters their data using a [Docusign web form](https://support.docusign.com/s/document-item?bundleId=gmi1660583110357&topicId=ofz1660589243255.html). After doing so, they see a confirmation screen letting them know that their information has been received. Then, the extension app takes over, eliminating the need for manual data entry and automatically creating a new customer and invoice in Stripe.

## Getting started with Docusign

The only prerequisite to start building extension apps and workflows is a free [Docusign Developer Account](https://www.docusign.com/developers/sandbox). Once you’ve created your account, you can start testing out Docusign features and making API calls. To build an existing extension app into your workflow, you’ll need to install it from the [Docusign App Center](https://apps-d.docusign.com/app-center), where you can browse and install all of the publicly available extension apps. To build the workflow described in this blog post, you’ll need to install the [Stripe extension app](https://apps-d.docusign.com/app-center/app/3e8105e3-08a6-48cc-b63a-856954184e65).

## Building the workflow

A workflow like this can be configured through the workflow designer UI. This exact workflow is available as a [workflow template](https://support.docusign.com/s/document-item?bundleId=yff1696971835267&topicId=irb1736981148403.html). To use the template, navigate to the **Templates** tab in your Docusign account and select **Workflow Templates** in the left menu. Then choose the **Send new customer data to Stripe for invoicing** template, shown in the red box in the screenshot below.

![](https://stripe.dev/images/docusign-stripe-integration-automating-billing-workflows/image8.png)

Then, select **Use Template** to create a new workflow from the template in your account. You’ll be directed to the Maestro workflow designer UI, where you can configure each step in the workflow. If you want to trigger the workflow from an API call, you’ll want to change the workflow start method to **From an API Call**.

![](https://stripe.dev/images/docusign-stripe-integration-automating-billing-workflows/image9.png)

Then you can begin to edit the preconfigured workflow steps. The first step that collects customer data in a web form uses a web form provided by the workflow template. If you’d like to collect additional information from the customer, you can edit the form by choosing **Preview this web form**, then selecting **Edit Form.**

![](https://stripe.dev/images/docusign-stripe-integration-automating-billing-workflows/image6.png)

The confirmation screen step has already been configured with a custom message letting the customer know that they will receive an invoice soon, but you can customize this message with any information that you’d like.

![](https://stripe.dev/images/docusign-stripe-integration-automating-billing-workflows/image3.png)

Now, you need to configure the Stripe steps to create a new customer and invoice in Stripe using the information collected through the web form. First, configure the **Create a Customer in Stripe** step. Select **Customer** from the dropdown of Stripe objects and map the data fields from the web form in the first step to the required Stripe fields.

![](https://stripe.dev/images/docusign-stripe-integration-automating-billing-workflows/image5.png)

You’ll need to do the same for the step that creates a new draft invoice, choosing **Invoice** from the dropdown as the Stripe object to write to and mapping the fields accordingly, taking the record ID for the customer from the previous step.

![](https://stripe.dev/images/docusign-stripe-integration-automating-billing-workflows/image10.png)

Then, you’ll configure the step that adds an InvoiceItem to the draft invoice, choosing **InvoiceItem** as the Stripe object to write to. Map the record IDs from the previous two steps to the **customer** and **invoice** fields.

![](https://stripe.dev/images/docusign-stripe-integration-automating-billing-workflows/image1.png)

Finally, configure the step that sends the invoice to the new Stripe customer. Choose Email Invoice as the object to write to and map the record ID from the **Create Invoice Draft** step to the **invoiceId** field.

![](https://stripe.dev/images/docusign-stripe-integration-automating-billing-workflows/image4.png)

The workflow template is meant to be a starting point, so you can always add additional steps or customize these steps further. But once you’re done with your configuration, you’re ready to trigger the workflow.

## Triggering the workflow through the Maestro API

You’ve seen how easy it is to configure custom workflows with no code necessary. But developers can take the customization a step further by triggering workflows through the [Maestro API](https://developers.docusign.com/docs/maestro-api/). After [authenticating](https://developers.docusign.com/docs/maestro-api/auth/), you can trigger a workflow with just two API calls. The first step is to retrieve the trigger URL and other requirements necessary to trigger a specific workflow. To do this, call the [Workflows: getWorkflowTriggerRequirements](https://developers.docusign.com/docs/maestro-api/reference/maestro/workflows/getworkflowtriggerrequirements/) endpoint as shown in the snippet below. If you don’t already have your workflow ID, you can call the [Workflows: getWorkflowsList](https://developers.docusign.com/docs/maestro-api/reference/maestro/workflows/getworkflowslist/) endpoint to get a list of all available Maestro workflows for your account, including their IDs.

```
response=$(mktemp /tmp/response-wftmp.XXXXXX)
Status=$(curl -s -w "%{http_code}\n" -i --request GET "${base_path}/accounts/${account_id}/workflows/${workflow_id}/trigger-requirements" \
    "${Headers[@]}" \
    --output ${response})
```

The response of this call looks like this:

```
{
  "trigger_id": "wfTrigger",
  "trigger_event_type": "HTTP",
  "trigger_http_config": {
    "method": "POST",
    "url": "https://api-d.docusign.com/v1/accounts/0820f9c5-xxxx-xxxx-xxxx-8a0df87f44aa/workflows/36e119db-xxxx-xxxx-xxxx-e91731fe95cd/actions/trigger"
  },
  "trigger_input_schema": [
    {
      "field_name": "signerName"
    },
    {
      "field_name": "signerEmail"
    },
    {
      "field_name": "ccName"
    },
    {
      "field_name": "ccEmail"
    }
  ],
  "metadata": {
    "created_at": "2025-05-13T21:52:02.983+00:00",
    "created_by": "8cb9aa3f-xxxx-xxxx-xxxx-f6ce2e16dad1",
    "modified_at": "2025-05-13T21:52:22.114+00:00",
    "response_timestamp": "2025-05-20T18:27:04.2896661Z",
    "response_duration_ms": 1122
  }
}
```

The response contains the url property, which you’ll use to actually trigger the workflow. It also includes a list of any input fields that need to be passed in the request body when triggering the workflow. To trigger the workflow, construct a request body that includes the instance name and any necessary trigger inputs, like the example request body below.

```
request_data=$(mktemp /tmp/request-wf-001.XXXXXX)
printf \
'{
  "instance_name": "'"$instance_name"'",
  "trigger_inputs": {
    "signerEmail": "'"${signer_email}"'",
    "signerName": "'"${signer_name}"'",
    "ccEmail": "'"${cc_email}"'",
    "ccName": "'"${cc_name}"'"
  }
}' >$request_data
```

Then make a POST request to the trigger URL that you extracted from the response of the Workflows: getWorkflowTriggerRequirements endpoint.

```
response=$(mktemp /tmp/response-wftmp.XXXXXX)
Status=$(curl -s -w "%{http_code}\n" -i --request POST ${trigger_url} \
    "${Headers[@]}" \
    --data-binary @${request_data} \
    --output ${response})
```

You’ll receive a response that includes the instance ID and instance URL, which can be used to complete the workflow steps.

```
{
  "instance_id": "2fca39a8-xxxx-xxxx-xxxx-acd6bf2c5fd7",
  "instance_url": "https://apps-d.docusign.com/api/maestro/v1/accounts/0820f9c5-xxxx-xxxx-xxxx-8a0df87f44aa/instances/2fca39a8-xxxx-xxxx-xxxx-acd6bf2c5fd7/execution?mtid=9ee64ac9-xxxx-xxxx-xxxx-bf9682cc9466&mtsec=Mo2tZXHOSeS_xJ2hsIklLC5hs_xxxxxxxxxxxx8ceTE"
}
```

You can direct users from your application to this instance URL, or you can [embed it in an iframe](https://developers.docusign.com/docs/maestro-api/maestro101/embed-workflow/) so workflow participants can complete their workflow tasks directly inside your app.

Thanks to the Stripe extension app, what might have once required a complex integration with many API calls to both Docusign and Stripe endpoints can now be accomplished through a no-code Maestro workflow and just two API calls. You can download the Stripe extension app, and many other apps, from the [Docusign App Center](https://developers.docusign.com/extension-apps/extension-apps-101/app-center/).

## Conclusion

In this blog post, I walked through how to incorporate the Stripe extension app into a Maestro workflow, automating a complex billing workflow into a streamlined process that can be kicked off with a single API call. This is just one example of how you can leverage extension apps and Maestro to manage agreements through their whole lifecycle, beyond signing. You can build your own private or public extension apps to easily incorporate any third-party API into Docusign, and build custom workflows featuring those extension apps that automate your agreement processes across multiple platforms.

Learn more about how to build your own custom extension apps to fit your use cases on the [Docusign Developer Center](https://developers.docusign.com/extension-apps/), and join the [Docusign Developer Community](https://community.docusign.com/developer-59) to connect with other developers, ask questions, and stay up to date on the latest events.

For more Stripe developer learning resources, subscribe to our [YouTube Channel](https://www.youtube.com/@StripeDev).
