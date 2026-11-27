---
title: Stop juggling multiple POS devices with Stripe Terminal
link: https://stripe.dev/blog/stripe-terminal-on-screen-input-collection
source: stripe-com
published: 2025-06-12T00:00:00Z
updated: 2025-06-12T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Andrew Robinson
summary: Eliminate separate devices and manual data entry. Stripe Terminal's on-screen input collection consolidates customer interactions in one place.
content: extracted
html: 2025-06-12-stop-juggling-multiple-pos-devices-with-stripe-terminal.html
remote_preview:
  url: https://stripe.dev/og/blog/stripe-terminal-on-screen-input-collection.png
  alt: 'Stripe Developer Blog: Stop juggling multiple POS devices with Stripe Terminal'
extra:
  content:interactive: true
---

Building point-of-sale systems that efficiently capture customer data, beyond just payment details, presents unique technical and operational hurdles. For developers, this often means integrating multiple disparate systems. In traditional checkout processes, customers frequently need to provide various forms of data, which can require interactions with multiple systems and devices.

Take car rentals, for example. Customers typically sign agreements on a separate signature pad, then verbally share details like email or phone for receipts or loyalty programs. The operator manually keys this into the POS, increasing errors and slowing down the process. In addition, some customers might be reluctant to share this information verbally, within earshot of strangers. Receipt preferences often add another manual step. This fragmented data collection—relying on verbal input, juggling separate signature pads, and manual entry—consumes staff time, invites errors, and creates bottlenecks, leading to frustrated customers and longer queues, especially during busy periods.

These multi-step interactions not only consume valuable time but can also result in long queues, especially during peak business hours. The need to juggle multiple input devices, like signature pads, customer-facing displays, operator-facing displays, or keyboards, contributes to a cluttered and inefficient workspace. This setup not only hampers the checkout process but can lead to a subpar experience as customers grow impatient or frustrated with delays.

This technical fragmentation and operational friction doesn't just create delays—it directly impacts customer satisfaction and perception. The challenge is finding a way to consolidate data collection directly within the payment flow, simplifying the technical stack and improving the in-person experience.

### Introducing Stripe Terminal's on-screen input collection

Stripe Terminal's on-screen input collection feature directly addresses these challenges by allowing businesses to collect diverse customer data—from loyalty numbers to signatures—directly on the Stripe reader. Critically, this eliminates the need for separate devices like signature pads, consolidating the interaction workflow.

Using the [Stripe API](https://docs.stripe.com/terminal/payments/setup-integration?terminal-sdk-platform=server-driven) or [Terminal SDKs](https://docs.stripe.com/terminal/payments/setup-integration?terminal-sdk-platform=react-native&locale=en-GB), developers can integrate custom input forms into their POS flow, before or after a payment. For example, if you want to collect a loyalty number pre-transaction, or an email for a receipt post-transaction, or a signature for an agreement, it's all handled by the reader.

### Key features of Stripe’s input collection

#### Versatile input options

The feature supports six core input types, designed for common POS data collection needs:

- **Selection**: for choosing what type of receipt a customer wants or for closed questions/surveys
- **Signature**: collecting a signature as part of a rental agreement
- **Email**: collecting email address for a receipt or enrollment in a loyalty program
- **Phone**: collecting phone number for an SMS receipt or enrollment in a loyalty program
- **Text**: collecting a customer's name, postal code, or other type of free text information
- **Numeric**: collecting a loyalty program number

Developers can customize forms with features like required fields, skip buttons, and visual styles (like toggle switches). Importantly, you can include metadata in the API request, which is returned in the webhook response, allowing you to associate inputs with specific transactions or objects. (Note: This feature is not for collecting sensitive personal information like health records or payment details.)

#### Seamless integration

Integration is straightforward using the `collect_inputs` command in the Stripe API. Your POS application constructs the form details in the API call, and the Stripe Terminal reader handles rendering the prebuilt UI. Once the customer completes the input, Stripe sends a `terminal.reader.action_succeeded` webhook event to your application, containing the customer's response. For signatures, the webhook includes a file ID pointing to the signature's SVG file. Multiple inputs can be requested with one API call - Stripe Terminal will simply sequence them.

#### Improved customer experience

By consolidating input steps directly onto the terminal, businesses dramatically reduce friction for customers. Instead of juggling devices or repeating information verbally, customers interact directly with the reader, leading to a smoother, faster, and more modern checkout experience. The accurate data collected can power personalized follow-ups or loyalty programs, increasing customer satisfaction and encouraging repeat business.

### Sample integration: Collecting customer address and signature

Consider a rental car company looking to upgrade its POS. Today, they likely use a separate signature pad in addition to their card reader. They also need to manually collect customer details for lookups or receipts. Stripe Terminal's on-screen input collection can replace these fragmented steps. Here's one possible flow:

1. Ask the customer how to look up their booking—email, loyalty number, booking number
2. Collect relevant input from the reader—email address, loyalty card number, booking number
3. The agent can then handle any aspects related to their booking, confirming dates and times, vehicle type etc., and show them the rental agreement
4. Collect customer signature to agree to the rental agreement on the reader

Let's walk through an example flow where the customer selects to use their email address to look up their reservation. This example uses a server-driven integration using the Stripe API.

#### Booking lookup

Let's detail a flow for initial customer lookup using a server-driven (API-based) integration. Your POS system uses the API to present a "select lookup method" form on the reader. Once the customer taps "Booking reference," your POS gets the selection via webhook. Your POS then triggers a new form on the reader asking for the booking number input. The customer enters it, the response comes back to your POS via webhook, and your system uses that ID to look up their reservation.

![](https://stripe.dev/images/stripe-terminal-on-screen-input-collection/image2.png)

The first code snippet creates a selection form with three options for the customer to choose how they'd like to look up their booking—email, loyalty number, or booking reference. The email and loyalty number are "primary" options, which means they are shown in a different color to highlight them. A title and description are provided for the page, and we've made it a required option.

```
import stripe
stripe.api_key = "sk_test_xxxxxxxxxx"

reader = stripe.terminal.Reader.collect_inputs(
  '{{READER_ID}}',
  inputs=[
    {
      "type": "selection",
      "selection": {
        "choices": [
          {"style": "primary", "text": "E-mail", "id": "email_id"},
          {"style": "primary", "text": "Loyalty Number", "id": "loyalty_id"},
          {"style": "secondary", "text": "Booking reference", "id": "booking_ref_id"},
        ],
      },
      "custom_text": {
        "title": "Lookup your reservation",
        "description": "How would you like to lookup your reservation",
      },
      "required": True,
    },
    ],
)
```

After the customer has made their selection, in this case their email address, you can then render the next form to collect the customer's email address.

```
import stripe
stripe.api_key = "sk_test_xxxxxxxxxx"

reader = stripe.terminal.Reader.collect_inputs(
  '{{READER_ID}}',
  inputs=[
    {
      "type": "email",
      "custom_text": {
        "title": "Email address",
        "description": "Please enter your email address to look up your reservation",
      },
      "required": True,
    },
    ],
)
```

#### Signature

Once the customer has agreed to the terms of the rental agreement, the POS renders a signature form on the Stripe Terminal. Once the customer has signed this, a webhook response is sent to the POS. The POS system can then download the signature image file (within 24 hours of the webhook event), store it, and mark that the rental agreement has been signed. Note that Stripe only stores the signature image file for 24 hours so you will need to ensure you keep a copy of the SVG file.

![](https://stripe.dev/images/stripe-terminal-on-screen-input-collection/image1.png)

The next code snippet shows how to render a signature collection form on the reader. This example includes the text to display on the submit button, and also passes in the booking reference number as metadata which will then be included on the webhook response from Stripe.

```
import stripe
stripe.api_key = "sk_test_xxxxxxxxxx"

reader = stripe.terminal.Reader.collect_inputs(
  '{{READER_ID}}',
  inputs=[
    {
      "type": "signature",
      "custom_text": {
        "title": "Agreement signature",
        "description": "Please sign below to accept your agreement",
        "submit_button": "Submit"
      },
      "required": True,
    },
    ],
)
```

### Benefits to businesses

Efficient data collection through Stripe Terminal's on-screen input functionality directly impacts business operations in several significant ways:

#### Improved operational efficiency and data accuracy

Moving data collection directly onto the terminal screen eliminates manual transcription and separate devices. This not only drastically cuts down on manual entry errors but also speeds up the entire checkout flow, freeing up staff and reducing queues. The result is a more efficient operation and more reliable customer data.

#### Enhanced ability to offer tailored services or promotions

Collecting data like emails for receipts, phone numbers for loyalty lookups, or preference selections directly at the point of interaction provides richer, more accurate customer profiles. This allows businesses to follow up with personalized offers or tailor future interactions based on captured data, building loyalty and increasing relevance for the customer.

#### Reduction in time

By automating data capture on the terminal, businesses drastically cut down the time staff spend on manual data entry or managing separate input devices. This frees up staff to focus on customer service or other critical tasks, boosting overall productivity and reducing operational costs.

### Conclusion

Resolving fragmented data collection at the POS is key to improving efficiency and customer satisfaction. Stripe Terminal's on-screen input collection provides a powerful, integrated solution. By allowing you to collect signatures, loyalty details, receipt preferences, and more—directly on the terminal screen via the API and webhooks—you eliminate separate devices and manual steps, significantly reducing errors and speeding up checkout.

This streamlined process enhances the customer experience and equips businesses with accurate data for personalization. For developers building POS systems, this feature simplifies architecture while enabling richer customer interactions on compatible readers like the S700.

If you're ready to enhance your business operations and elevate customer interactions, we invite you to explore the capabilities of Stripe Terminal further. Whether you're looking to streamline your checkout process, gather valuable customer data, or create a seamless point-of-sale experience, Stripe Terminal's on-screen input collection offers a scalable solution to meet your needs.

### Additional resources

To get started and access more information on integrating with Stripe Terminal, check out the following resources:

- [Collect on-screen inputs | Stripe Documentation](https://docs.stripe.com/terminal/features/collect-inputs?locale=en-GB)
- [Collect inputs using a Reader | Stripe API Reference](https://docs.stripe.com/api/terminal/readers/collect_inputs)
- [Terminal | Stripe Documentation](https://docs.stripe.com/terminal)
- [Accept in-person payments | Stripe Documentation](https://docs.stripe.com/terminal/quickstart)
- [Order Stripe Terminal readers (via Stripe Dashboard)](https://dashboard.stripe.com/terminal/shop)

For more Stripe developer learning resources, subscribe to our [YouTube Channel](https://www.youtube.com/@StripeDev).
