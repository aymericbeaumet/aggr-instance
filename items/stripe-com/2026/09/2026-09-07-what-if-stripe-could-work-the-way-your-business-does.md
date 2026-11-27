---
title: What if Stripe could work the way your business does
link: https://stripe.dev/blog/what-if-stripe-could-work-the-way-your-business-does
source: stripe-com
published: 2026-09-07T00:00:00Z
updated: 2026-09-07T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Ben Smith
summary: Discover how Stripe Scripts, Workflows, Custom Objects, and UI extensions help businesses customize logic, data, and operations directly within Stripe.
content: extracted
html: 2026-09-07-what-if-stripe-could-work-the-way-your-business-does.html
remote_preview:
  url: https://stripe.dev/og/blog/what-if-stripe-could-work-the-way-your-business-does.png
  alt: 'Stripe Developer Blog: What if Stripe could work the way your business does'
---

Stripe provides the financial infrastructure businesses need to accept payments, manage subscriptions, send invoices, and move money. For many businesses, Stripe’s default behavior is exactly what they need - it works without any additional customization.

Sooner or later, you need something specific: a custom proration rule, a multi-step fraud response, data that doesn’t fit an existing Stripe object, or a purpose-built Dashboard view for your operations team.

Traditionally, that meant building around Stripe. A webhook sends an event to your server, your application runs some logic, and then calls the Stripe API. It’s a flexible pattern, but every customization adds more infrastructure, and more responsibility for availability, retries, state, secrets, security, and error handling.

[Stripe extensibility](https://docs.stripe.com/extensibility) lets you move more of that customization into Stripe. In this post, you’ll learn how to extend Stripe across three dimensions: logic, data, and user interface, using [scripts](https://docs.stripe.com/billing/scripts), [Workflows](https://docs.stripe.com/workflows), [Custom Objects](https://docs.stripe.com/custom-objects), and [UI extensions](https://docs.stripe.com/stripe-apps/build-ui), and see how they work together in a complete example.

Prefer to watch? See how these capabilities work together in our Stripe Sessions talk, [Extensibility with Scripts and Workflows](https://www.youtube.com/watch?v=1v5ik1t0azc).

## **Three dimensions of extensibility**

Most Stripe customizations fall into three dimensions:

- **Logic** defines what should happen when something occurs in your business.
- **Data** represents the concepts and relationships that are unique to your business.
- **User interface** gives your team the tools it needs to view that data and take action.

You can use each independently, but they become much more powerful when they work together. Your interface can display your custom data, your data can contain business-specific actions, and those actions can invoke your logic.

## **Logic: From default behavior to custom rules**

The best place to begin is usually Stripe’s default behavior. If the default meets your needs, there is nothing else to build or maintain. When you need something different, configuration gives you additional options supplied by Stripe or a partner. You can change how something behaves without writing the logic yourself.

Some rules, however, belong only to your business, and that is where custom logic fits. For example, when a customer changes plans midway through a billing period, Stripe can calculate the credit for the old plan and the charge for the new one down to the second - this is called proration. That works for most businesses. Yours might round to the nearest day, waive adjustments below $5, or use an entirely different calculation.

With a **script**, you can write that rule in TypeScript and deploy it to Stripe. When Stripe reaches the proration decision in its billing flow, it invokes your code, receives an answer, and continues processing. The code runs in a secure environment on Stripe, without requiring a server for you to manage. Scripts are for singular decisions: Stripe asks a question and your code returns an answer.

![Stripe Scripts](https://stripe.dev/images/what-if-stripe-could-work-the-way-your-business-does/image2.png)

More complex requirements need a different process. For example, when an early fraud warning arrives, you might need to check the charge amount, issue a refund when it falls below a threshold, and notify your risk team in Slack.

**Stripe Workflows** coordinates these steps in the correct order. Workflows can be triggered by a Stripe event, started by someone in the Dashboard, or called from your backend. It can combine Stripe API calls, third-party integrations, and custom actions without requiring you to build the surrounding orchestration infrastructure.

Scripts run code. Workflows coordinate work.

## **Data: Teach Stripe the language of your business**

Everything in Stripe is an object. Customers, Products, Payments, Invoices, and Subscriptions are all objects that represent the core concepts Stripe understands. But your business likely has other concepts that do not fit neatly into those objects. Imagine a vehicle rental company called Rocket Rides. A Driver maps naturally to a Stripe Customer, but the company also needs to represent Vehicles and Rentals.

A Vehicle has a make, model, year, and vehicle identification number. A Rental has a start date, status, assigned vehicle, and driver. These are structured entities with validation rules and relationships, not simply additional labels.

You could store some of this information in metadata, but metadata is not a complete data model. It does not provide typed fields, enforce a schema, or express relationships between objects.

**Custom objects** let you define these business-specific models inside Stripe. Rocket Rides can require every Vehicle to have a 17-character VIN and define a Rental that references both a Vehicle custom object and Stripe’s native Customer object.

In simplified TypeScript, that relationship looks like this:

```
interface RentalFields {
  startsAt: Date
  status: 'upcoming' | 'in_progress' | 'completed'
  vehicle: Ref<Vehicle>
  driver: Ref<Customer>
}

@CustomObject
class Rental extends BaseObject<RentalFields> {}
```

Once deployed, Stripe provides APIs for creating, reading, updating, and deleting these objects. Your business-specific data now has structure, relationships, and the same platform foundation as native Stripe data.

![Custom objects](https://stripe.dev/images/what-if-stripe-could-work-the-way-your-business-does/image1.png)

## **User interface: Build where your team already works**

APIs give developers access to your custom data, but operations and finance teams should not need to query an endpoint to understand what is happening.

With **UI extensions**, you can create pages and side panels inside the Stripe Dashboard using Stripe’s design system. You choose where the experience appears and which information it presents.

For Rocket Rides, that could be a rental operations page containing the vehicle details, driver, payment history, rental status, and inspection information. Instead of switching between Stripe and a separate internal tool, the operations team gets a single view designed around the way it works. Viewing the data is only part of the job. When a vehicle is returned, the team also needs to end the rental.

![Custom UI](https://stripe.dev/images/what-if-stripe-could-work-the-way-your-business-does/image3.png)

## **Bringing logic, data, and UI together**

Rocket Rides can add an **End Rental** button to its custom Dashboard page. That button invokes an action defined on the Rental custom object.

When an operator selects it, the action updates the rental status, triggers the inspection workflow, and marks the vehicle as available again. The team remains in the Dashboard while custom logic runs against custom data and coordinates the next stage of the process.

This is where the three dimensions of extensibility become one connected system:

- Custom objects model Rentals and Vehicles.
- A custom Dashboard page gives the operations team the right view.
- An action provides a business-specific operation.
- A workflow coordinates everything that must happen next.

The team clicks one button, but behind it Stripe is operating in the language of the business.

## **Start with what you need**

Extensibility does not mean every Stripe integration needs custom code. Start with the default. Use configuration when an existing option meets your requirements. Build custom logic, data, or interfaces when the requirement is genuinely unique to your business.

Scripts let you define individual decisions. Workflows coordinate processes over time. Custom objects represent the concepts that Stripe does not model natively. UI extensions give your team tailored pages, side panels, and actions inside the Dashboard.

Together, they let you extend Stripe without turning every customization into another standalone system your team must operate.

Stripe provides the foundation. Extensibility lets you build the business only you can build.

Start building with [Stripe extensibility](https://docs.stripe.com/extensibility).
