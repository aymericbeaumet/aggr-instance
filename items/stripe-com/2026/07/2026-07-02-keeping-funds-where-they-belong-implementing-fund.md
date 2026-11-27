---
title: 'Keeping funds where they belong: Implementing fund segregation in Stripe Connect'
link: https://stripe.dev/blog/fund-segregation-stripe-connect
source: stripe-com
published: 2026-07-02T00:00:00Z
updated: 2026-07-02T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Andrew Robinson
summary: Learn how to implement fund segregation in Stripe Connect using allocated balances to ring-fence seller funds from your platform's operational balance - covering payment creation, transfers, refunds, disputes, and preparing for upcoming PSD3 regulatory requirements.
content: extracted
html: 2026-07-02-keeping-funds-where-they-belong-implementing-fund.html
remote_preview:
  url: https://stripe.dev/og/blog/fund-segregation-stripe-connect.png
  alt: 'Stripe Developer Blog: Keeping funds where they belong: Implementing fund segregation in Stripe Connect'
extra:
  content:interactive: true
---

Many marketplaces or platforms operate on [Stripe Connect](https://stripe.com/connect), using the [separate charges and transfers](https://docs.stripe.com/connect/separate-charges-and-transfers) charge type. This allows marketplaces to offer different purchase combinations for their customers, such as items from different marketplace sellers, in a single transaction, and then perform separate transfers.

If you operate in this model, when your platform collects a payment, those funds land into the Stripe balance on your platform account, pooled together with your own revenue. Stripe handles the accounting and reporting, but the funds themselves aren’t ring-fenced until you explicitly transfer them to a seller's connected account.

PSD2 is the 2nd revision of the Payment Services Directive, a European Union law that regulates electronic payments. The latest version of this, PSD3, is due to take effect from late 2027. Under PSD2, most marketplaces and platforms operate in this way using the Commercial Agent Exemption (CAE), allowing you to process payments without a formal payment institution license. Due to upcoming changes to PSD3 and the likely tightening of Commercial Agent Exemptions, Stripe has created a new charge type for Stripe Connect called "[Segregated Separate Charges and Transfers](https://docs.stripe.com/connect/funds-segregation)".

### The problem with pooled balances

If we take a marketplace that is processing payments for thousands of different sellers, without fund segregation, a single platform balance on Stripe holds funds for many different connected accounts (sellers), meaning that:

- **Automatic payouts** scheduled on your platform account could inadvertently consume funds meant for a seller.
- **Unrelated chargebacks** or disputes on one transaction can draw from the shared pool, affecting funds earmarked for a completely different seller.
- **Stripe fees** are debited from your platform balance - the same pool that holds seller money.
- **Operational errors** such as an accidental refund, or misconfigured payout schedule, can leave sellers short with no clear audit trail.

In isolation, each scenario may seem unlikely, but at scale they can compound into systemic risk.

### Why this matters now

Under PSD2 and EMD2 regulations in the EU, regulators are increasingly requiring platforms and marketplaces to demonstrate that customers’ funds are safeguarded, that they are held separately from operational funds and protected from the platform’s own creditors. Within the UK, the FCA enforces similar client money rules.

Even in geographies outside of this regulation, fund segregation is often treated as a trust signal. Sellers want to know their money is protected, investors or auditors want to see clear separation, and engineering teams want deterministic fund flows, rather than a shared pool of funds where edge cases can compound.

### How fund segregation works

Fund segregation in Stripe Connect introduces a new holding state for funds. Funds from payments are allocated; they exist on your platform account, but don’t appear in your available balance, can’t be used for payouts, and can only be transferred to a connected account. The diagram below shows a high level overview of the fund flow:

![High level overview of the fund flow](https://stripe.dev/images/fund-segregation-stripe-connect/diagram.png)

The full end-to-end flow can be described as follows:

#### Create a payment with allocated funds

When creating a [PaymentIntent](https://docs.stripe.com/payments/payment-intents), set `allocated_funds` to `enabled: true`. At capture time, Stripe automatically places the funds into the allocated balance within your Stripe Platform account.

You should also specify the `transfer_group` with your value; this value lets you track the flow of funds from payment through to transfer, and helps with reconciliation and audit flows.

```
{
  "amount": 10000,
  "currency": "eur",
  "allocated_funds": {
    "enabled": true
  },
  "transfer_group": "ORDER-1234"
}
```

After successful capture of funds, the full amount becomes available in your allocated balance, subject to normal payment method settlement timing.

#### Check the allocated balance

You can inspect the allocated funds balance for a specific payment intent by expanding the `latest_charge` property within the PaymentIntent object. This gives you per-payment visibility, so you know exactly how much is in your allocated balance, and how much has been transferred into a connected account.

In this example, we can see that `allocated_funds` is enabled, so we know the charge amount is using your allocated balance, and €98.00 is available to be transferred (€100 minus €2 example Stripe fees).

```
{
  "id": "pi_xxx",
  "latest_charge": {
    "id": "ch_xxx",
    "allocated_funds": {
      "enabled": true,
      "balance": {
        "available": 9800,
        "pending": 0,
        "currency": "eur"
      }
    }
  }
}
```

#### Transfer to connected accounts and collect your platform fee

When creating the transfer object to transfer allocated funds to a connected account, set the `source_transaction` parameter to the charge ID from the original payment intent. This tells Stripe to draw from that specific payment’s allocated balance. You can also collect your platform fee in the same call by specifying the `application_fee_amount` parameter. This debits the fee from the allocated funds and credits it to your platform’s payment balance.

It’s important to note that the `application_fee_amount` is additive to the transfer amount. For example, if you want to collect a €5.00 application fee for a €100.00 charge, and then transfer the remaining €93.00 to a connected account, you’d need to specify this as follows:

```
{
  "amount": 9300,
  "currency": "eur",
  "destination": "acct_seller_123",
  "source_transaction": "ch_xxx",
  "transfer_group": "ORDER-1234",
  "application_fee_amount": 500
}
```

You don’t need to transfer everything at once, you can split allocated funds across multiple transfers to connected accounts; this is useful if you have mixed first-party and third-party purchases, or purchases from multiple sellers in a single transaction. One rule to remember is that the total transferred cannot exceed the original charge amount.

As the `application_fee_amount` is taken directly from the allocated funds on your platform account, it avoids unnecessary FX conversions when the platform and connected account operate in different currencies.

#### Handling the hard parts

In the real world, we know our users don’t just process happy path payments. Refunds, disputes, and reversals are also common, so it’s key to understand how these work using allocated balances.

#### Refunds

When you process a refund, Stripe will first draw from the remaining allocated balance before touching the balance of your platform account. As an example, if you had a €100.00 charge in your allocated balance, you transferred €60.00 to a connected account (leaving €40.00 in the allocated balance), and then did a €100.00 refund:

1. €40.00 would come from the remaining allocated funds
2. €60.00 would come from your platform’s account balance

This predictable ordering means allocated funds are always used first, reducing the frequency of debits from your platform’s account balance.

#### Transfer reversals

If you need to claw back funds from a connected account balance, for example before processing a refund or while awaiting a dispute outcome, you can use a transfer reversal. Reversed funds will return to the allocated balance, not your platform’s account balance.\
 When creating a reversal, if you set the `refund_application_fee` to `true`, the application fee is also returned to the allocated balance. This is an important decision for your business; some marketplaces choose to refund the application fee, while others choose to retain it.

#### Disputes

When a dispute is raised on an allocated payment, you can reverse the transfer to pull funds back from the connected account into your allocated balance whilst awaiting the outcome.

If the dispute is won, you can then transfer the allocated funds back to the connected account. If the dispute is lost, you can use a Balance Transfer to move these funds into your platform balance, providing the currency matches the original charge settlement currency.

Dispute fees are always debited from your platform balance, they never affect the allocated funds balance.

#### Hanging balances

You may find that you end up with allocated funds that no longer need to be in your allocated balance, for example after a dispute is refunded from your platform balance and you later reverse the transfer from the connected account.

In this scenario, you can use a Balance Transfer to move these funds from the allocated balance to your platform balance, providing the currency matches the original charge settlement currency.

### Getting ready

Before starting, there are some pre-requisite steps worth being aware of:

1. Your platform account must be responsible for negative balances on connected accounts
2. Include the `allocated_funds_preview=v1` parameter as part of the header on all API requests. For example, with Node you include this as a parameter when defining the Stripe const variable:

```
const stripe = require('stripe')(
  'sk_test_',
    {
      'allocated_funds_preview=v1',
    }
  );
```

3. Allocated funds currently only work with card payment methods, so you should avoid using `automatic_payment_methods` and specify payment methods explicitly
4. Allocated balances cannot be combined with overcapture, multi capture, or incremental authorisations
5. For testing, allocated balances can only be used with sandbox accounts, they do not work in legacy test mode
6. The Stripe dashboard doesn’t distinguish between allocated balances and other account balances, you must use the API for visibility

### Benefits

With segregated separate charges and transfers and allocated balances, your platform can achieve:

- **Deterministic fund flows** — every payment's funds are traceable from capture to transfer, with no possibility of leakage into unrelated operations.
- **Regulatory alignment** — demonstrable safeguarding of customer funds, separated from platform operational balances.
- **Operational safety** — auto-payouts, unrelated chargebacks, and fee debits can't touch allocated funds.
- **Clear audit trail** — per-payment balance visibility via the API at every stage of the lifecycle.

For platforms or marketplaces that operate in regulated markets, or those handling significant seller volumes, this allows you to move to a world in which fund safety becomes enforced by the infrastructure, rather than being purely on a trust basis.

### Next steps

- Review the full [fund segregation documentation](https://docs.stripe.com/connect/funds-segregation)
- Set up a [Sandbox environment](https://docs.stripe.com/sandboxes) to test your integration
- If you're evaluating whether fund segregation fits your regulatory requirements, reach out to your Stripe account team for guidance on your specific jurisdiction
