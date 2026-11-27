---
title: Run recurring jobs on Stripe without a cron server
link: https://stripe.dev/blog/run-recurring-jobs-on-stripe-workflows
source: stripe-com
published: 2026-10-13T00:00:00Z
updated: 2026-10-13T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Tanya Boiteau
summary: Stripe Workflows already could react to events and run on demand. Now, it can start a workflow on its own at a time you specify.
content: extracted
html: 2026-10-13-run-recurring-jobs-on-stripe-without-a-cron-server.html
remote_preview:
  url: https://stripe.dev/og/blog/run-recurring-jobs-on-stripe-workflows.png
  alt: 'Stripe Developer Blog: Run recurring jobs on Stripe without a cron server'
---

When a customer's renewal payment fails, their subscription moves to past due while Stripe retries the payment. Retries recover some of these payments on their own, but your collections team usually wants to contact the others before the retries run out. So every weekday at 9:00 AM, someone on your finance team opens the Stripe Dashboard, filters for past-due subscriptions, and copies each one into a Slack channel so the team knows who to contact. On a quiet day, it's a small chore. On a busy day, it doesn't happen at all.

Nobody notices the skipped day until the retries run out, and a customer who would have updated their card after one friendly message has churned instead. The check itself is predictable: the same list, the same filters, and the same message, every morning. What's unpredictable is whether a person remembers to run it.

This pattern shows up across finance and operations teams. Daily balance checks, weekly payout reports, and month-end reconciliation all depend on the calendar, not on something happening in your account. [Stripe Workflows](https://docs.stripe.com/workflows) already could react to events and run on demand. Now, it can start a workflow on its own at a time you specify.

The usual fix: a cron job

Teams that want to automate a recurring check usually end up writing a scheduled job. A typical version runs on your own infrastructure, lists past-due subscriptions through the Stripe API, and posts each one to Slack through an incoming webhook:

```
// past-due-subscriptions.js, run by cron on a UTC server: 0 16 * * 1-5
const stripe = require('stripe')(process.env.STRIPE_SECRET_KEY);

async function main() {
  for await (const subscription of stripe.subscriptions.list({
    status: 'past_due',
    expand: ['data.customer'],
    limit: 100,
  })) {
    await fetch(process.env.SLACK_WEBHOOK_URL, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        text: `Subscription for ${subscription.customer.email} is past due: https://dashboard.stripe.com/subscriptions/${subscription.id}`,
      }),
    });
  }
}

main().catch((err) => {
  console.error(err);
  process.exit(1);
});
```

The script is short, but the system around it isn't. You need a server or a scheduled function to run it, a place to store a Stripe secret key and a Slack webhook URL, and a deploy process for every change to the message. Time zones are your problem too. A cron expression on a UTC server doesn't follow daylight saving time, so a job set for 9:00 AM in Los Angeles runs at 8:00 AM local time for part of the year. If the job fails, it often fails silently. You find out when someone asks why the channel has been quiet for a week.

The script also lives outside Stripe, and usually outside the finance team's reach. If the collections lead wants to change the channel, add the customer's name to the message, or check twice a day instead of once, they file a ticket with engineering. A 9:00 AM check that should belong to the team that relies on it ends up depending on someone else's sprint.

## Introducing recurring schedule triggers in Stripe Workflows

Stripe Workflows now supports [recurring schedule triggers](https://docs.stripe.com/workflows/triggers#recurring-schedule-triggers). You choose when a workflow runs, such as every weekday at 9:00 AM, every other Monday, or the last day of each month, and Stripe starts a run at each scheduled time. There's no server to run, no cron expression to maintain, and no API key to store.

A recurring schedule is one of three trigger types that Stripe Workflows supports:

- Event triggers run when something happens in your account, such as a payment succeeding or a dispute being created.
- Recurring schedule triggers run at the times you set.
- On-demand triggers run when you start them from the Dashboard or call the API.

Scheduled workflows pair well with list actions and loops. A common pattern is to schedule the workflow, retrieve a list of objects, and loop over that list to act on each item, which is the shape of the past-due subscription check.

Because the workflow lives in the Dashboard, the team that owns the process can read it, change it, and see every run.

## Walkthrough: notify your team about past-due subscriptions

The past-due subscription check uses a recurring schedule trigger, a list action, and a loop. Inside the loop, the workflow looks up the customer, skips subscriptions the team already knows about, sends a Slack notification, and records the notification in the subscription's metadata.

![A recurring schedule workflow diagram](https://stripe.dev/images/run-recurring-jobs-on-stripe-workflows/image2.png)

1. Set the schedule

In the workflow builder, click Add a trigger, then select Tools, where recurring schedule and on-demand triggers live, and choose Recurring schedule. Set Repeat every to 1, select Week, and select Monday through Friday under Days of the week. Set Starting to the date and time of the first run, and choose a Time zone, such as America - Los Angeles. The workflow runs at the time in Starting on each scheduled day, beginning with the first scheduled time on or after it. After you click Done, the trigger shows a summary so you can confirm the schedule before you publish.

![Setting the recurring schedule](https://stripe.dev/images/run-recurring-jobs-on-stripe-workflows/image3.png)

Schedules can run as often as every hour or as rarely as once a year. They follow the local clock, so a 9:00 AM workflow runs at 9:00 AM all year, and the Triggers docs cover the hour when clocks change.

2. List the subscriptions

Add a List subscriptions action and set the status filter to `past_due`. A subscription stays past due until a payment succeeds or the retries run out, so the same subscription shows up every morning until it's resolved.

3. Look up the customer

List subscriptions returns the customer's ID, not their email, so add a Retrieve a customer action and set its customer ID to the current subscription's customer. Because this action references a field from the list results, the builder adds a For each item in the Subscriptions loop, and every step after it runs once per subscription.

4. Skip subscriptions the team already knows about

Because past-due subscriptions show up every morning, the workflow needs to remember which ones it already reported. It does that with a metadata key, `collections_notified_invoice`, that the last step in the loop writes after the Slack message goes out. You don't create the key ahead of time, so on the first run, it's empty for every subscription.

Inside the loop, add a condition. For the field, choose Subscription, then Metadata, and enter `collections_notified_invoice` as the key. Set the operator to isn't equal to, and for the value, insert Subscription | Latest invoice ID. Put the next two steps on the If this condition is met branch.

![Adding a condition](https://stripe.dev/images/run-recurring-jobs-on-stripe-workflows/image1.png)

On the first run, the empty key isn't equal to the latest invoice ID, so the team gets notified. The next morning, the key holds that invoice ID, so the loop skips the subscription. If the subscription later goes past due on a new invoice, the IDs differ again and the team hears about it. A yes-or-no flag wouldn't work, because it stays set after the first notification and hides every later one.

5. Notify the team

On that branch, add another action, click Apps, and under Slack, choose Send Slack Notification. If you haven't installed the Slack app, the builder prompts you to install it from the [Stripe App Marketplace](https://marketplace.stripe.com/apps/slack-workflows) and connect your Slack workspace. Choose a channel, such as #collections, write a message template that includes the customer's email and the subscription ID, and add a Dashboard link to the subscription so the team can open it from Slack.

6. Record the notification

Finally, on the same branch, add an Update subscription action that sets `collections_notified_invoice` to the subscription's latest invoice ID. Anyone who opens the subscription can then see that the collections team has already been notified.

The loop handles pagination for you. If an action fails for one subscription, the loop stops there, and run details show which subscriptions were processed. Any subscription the loop didn’t reach gets picked up on the next run. If the Slack notification succeeds but the metadata update fails, the workflow might notify the team about that subscription again. That’s why the Slack step comes first - a duplicate message is better than a missed one. Each action inside a loop counts once per item. Retrieve a customer runs for every past-due subscription, but the condition limits the Slack and update steps to the ones that are new.

## Monitor and manage the schedule

After you publish, the workflow's page shows Next run, and Recent runs lists every run with its status. Click a run to see each step's input and output and, for a failed run, which step failed and why. To get alerted, create a second workflow triggered by the Workflow run failed event. Search for it in the trigger picker, or find it under More > Advanced > Workflow run. Then have it send a Slack notification to a different channel or Email a team member. To stop on a date, set Ending under Advanced. To pause, deactivate the workflow. Times missed while it's inactive are skipped.

A scheduled workflow runs only on its schedule, so you can't start it by hand. To test it, build it in a sandbox, set Starting a few minutes ahead, and check Recent runs. To run the same check on demand, create a separate workflow with an on-demand trigger.

## What changes for your team

Recurring schedule triggers move calendar-based work into the same place as the rest of your Stripe automation. The past-due subscription check runs every weekday at 9:00 AM local time whether or not anyone remembers it, each past-due subscription reaches the collections team once, and the subscription itself records that the team was notified.

There's no server, secret key, or daylight saving time logic to maintain. The team that relies on the check can change the channel, the message, or the schedule themselves, and when something goes wrong, the run shows which step failed and why.

Recurring schedule triggers are available in Workflows today. To build your first scheduled workflow, go to Workflows in the Dashboard.
