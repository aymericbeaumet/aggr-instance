---
title: 'How we built Stripe Credits: A programmable, auditable way to pay your Stripe fees'
link: https://stripe.dev/blog/how-we-built-stripe-credits-a-programmable-auditable-way-to-pay-your-stripe-fees
source: stripe-com
published: 2026-04-16T00:00:00Z
updated: 2026-04-16T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Pratik Gupta
summary: In this post we’ll dive into how we built a virtual cashless payment method that works for prepaid and Stripe-issued credits, and cleanly integrates with our accounting, compliance, and other frameworks.
content: extracted
html: 2026-04-16-how-we-built-stripe-credits-a-programmable-auditable-way-to.html
remote_preview:
  url: https://stripe.dev/og/blog/how-we-built-stripe-credits-a-programmable-auditable-way-to-pay-your-stripe-fees.png
  alt: 'Stripe Developer Blog: How we built Stripe Credits: A programmable, auditable way to pay your Stripe fees'
---

For years, Stripe collected all fees from a user’s primary payment balance. We implemented incentive programs using ad-hoc discounts or manual cash adjustments. While functional, this approach introduced significant friction:

- **Product silos.** Discounts applied only to card processing, which limited exploration of other Stripe products like Atlas, Billing, Connect, and Radar.
- **Confusing accounting.** Users paid full fees up front, then received retroactive refunds or discounts later. Users couldn’t see how or when offers were applied, or track their remaining balance.

We built a system, centered on a single, auditable programmable primitive, to manage credits. We built **Stripe Fee Credits**, a virtual, nonwithdrawable balance dedicated solely to paying eligible Stripe fees, kept separate from a user’s primary payment balance.

In this post we’ll dive into how we built a virtual cashless payment method that works for prepaid and Stripe-issued credits, and cleanly integrates with our accounting, compliance, and other frameworks.

### Our new fee credit architecture

Delivering this experience required a new internal architecture that could work at global scale. We think of it in three layers: the credits engine, logical financial accounts, and fee settlement.

### The credits engine

The **credits engine** is a standalone gRPC service that acts as the system of record for credit metadata, policies, lifecycle, and audit history.

Because Stripe operates globally with complex tax and product requirements, we couldn’t just create a generic “bucket” of money. We needed a robust data model to define exactly *what* a credit is and *how* it behaves. The core `Credit` object encapsulates policy, economic purpose, and which entities can use the credit, allowing us to share the credit across multiple user accounts.

We model the credit lifecycle as a finite-state machine and record each state change in a **credit papertrail** that ensures strict dual-approval flows for compliance. We built compliance in from the beginning, rather than treating it as an afterthought.

### Logical financial accounts

With the credits engine defining how credits operate within Stripe’s system, we needed a place to store their value. We needed to represent a “virtual” balance that holds no real cash. We’d been treating them as real funds, but realized that credits aren’t user funds in the regulatory sense. Whether they represent a prepaid amount already received by Stripe, or an incentive Stripe is granting at its own cost, we can classify the money sitting behind a credit as corporate funds. It doesn’t need to be safeguarded, swept to a bank account, or reconciled against a real cash position. This classification unlocked a clean architecture built off our existing abstractions, and a way to issue Stripe-funded credits without needing to issue real cash.

### Extending Money Movement and Storage

Rather than building a bespoke balance system just for credits, we extended Stripe’s existing Money Movement and Storage (MMS) platform, the infrastructure that handles real money movement across all of Stripe. We used our existing well-tested primitive, the financial account, which represents an entity capable of holding a balance and participating in money transfers. Every real Stripe financial account—merchant payment balances, bank settlement accounts, treasury accounts—is backed by a real bank account.

We created a new variant: purely logical financial accounts that aren’t backed by any real bank account. They carry a balance through Stripe’s internal ledger, but it never corresponds to real cash. This single abstraction eliminated an enormous amount of infrastructure complexity.

### Fee settlement orchestration

Orchestration is the final piece: determining *when* and *how* to apply credits to a user’s fees. In the legacy model, discounts were retroactive, which was an accounting headache. Now, our fee settlement engine queries applicable credits and applies them **just in time**—fees are paid using credits *before* they touch the user’s primary balance.

The three-step flow checks for eligibility, minimizes wasted credits, then debits the logical credit account, while enforcing critical guardrails and gracefully handling failures.

![](https://stripe.dev/images/how-we-built-stripe-credits-a-programmable-auditable-way-to-pay-your-stripe-fees/image1.png)

### Guardrails against logical-to-real leakage

With this new credits architecture, we needed to protect against the most dangerous failure mode—a logical ledger entry triggering a real cash movement. To prevent this, we built two critical guardrails: movement restriction and cash sweep isolation.

#### Movement restriction

The mechanism for all value movement between logical accounts is the Originated Money Transfer (OMT), an existing MMS workflow that we repurposed for cashless operations. Every OMT follows a three-phase lifecycle through the ledger: submitted → prepared → disbursed.

We added checks in MMS to enforce that logical financial accounts can only participate in OMTs with other logical financial accounts. We reject attempts to create a transfer between a logical account and a real account at the platform level, an important boundary that prevents logical ledger entries from accidentally triggering real cash movements downstream. Using OMTs—the same infrastructure that handles real money—gave us structured, auditable state transitions for every credit funding and every fee drawdown.

#### Cash sweep isolation

MMS runs automated intercompany sweeps based on ledger data. If logical financial accounts were allowed in sweeps calculations, the system could see a $100 incentive credit and attempt to sweep $100 in real cash—cash that doesn’t exist—between Stripe entities. We worked with the team that owns Stripe’s cash management infrastructure to ensure that ledger entries written to logical accounts are excluded from sweep calculations.

### Why our solution works

Logical financial accounts, the heart of our credits system, give us the best of both worlds: the rigor and auditability of Stripe’s battle-tested money movement infrastructure, without the operational overhead of managing real cash. Every credit funding, fee drawdown, and tax write-off produces the same structured ledger events that Stripe’s accounting, reporting, and compliance systems already know how to consume, so we didn’t need to build parallel reporting pipelines or special-case our audit tools. The financial data warehouse receives the same fields it expects for any other funds flow, and downstream teams can trace prepayment-to-revenue linkages through Stripe’s existing tooling.

The hard part of building a virtual balance system wasn’t tracking the balance; it was integrating it cleanly with the rest of the financial system without undermining auditability or accidentally moving real money. By integrating with MMS and building thoughtfully off our existing abstractions, we could focus our engineering effort on credit-specific logic like funds flows, policies, lifecycle, multisettlement, and idempotency challenges.

## Deep dive: Two funds flows

Our logical account primitive supports both Stripe-funded and user-funded credits. They have the same user experience, but **completely different accounting under the hood**. Prepaid credits create deferred revenue at funding time and collect tax at drawdown, while Stripe-funded credits are booked as a Stripe cost, and taxes are written off entirely since the credit is effectively a discount. We needed two distinct funds flows to handle the two credit types correctly—and sign off from teams across Stripe—before a single dollar could flow through the system.

### User-funded credits

Prepayments, where a user pays for fees up front, involve real cash entering the system, deferred revenue accounting, and a split between credit-settled fees and tax settled through the merchant’s regular payment balance. The flow spans multiple internal systems—our invoicing platform, the credits engine, MMS, and the fee settlement engine—and was the first funds flow at Stripe to enable merchants to pay up front annually for fees.

In this funds flow, let’s say a merchant signs a $1,000 prepayment deal. Stripe creates the invoice, establishing $1,000 in uncollected deferred revenue on our internal platform. When it’s sent to the merchant, the ledger establishes a credit that represents Stripe’s obligation to deliver future services and a corresponding debit that represents the merchant’s outstanding payment.

After the merchant pays the invoice, the credits engine triggers a funding event that initiates an inbound OMT to fund the merchant’s credit balance account with $1,000, which becomes deferred revenue that Stripe receives in a real financial account.

When the fee settlement engine assesses $10 in fees plus $1 in tax, the drawdown is split into the $10 fee paid from the credit balance via an outbound OMT that carries the correct product identifier for the account, and the $1 tax collected from the merchant’s payment balance through the existing fee collection path. The $10 fee is cleared from deferred revenue to the revenue account, and Stripe can identify the actual revenue against the product used by the user. If the prepaid credits expire with an unused balance, Stripe recognizes this as breakage revenue and applies the appropriate tax treatment. Every step produces structured ledger events that downstream systems—reporting, data warehouse, general ledger—can consume without needing to recompute.

### Stripe-funded credits

Though we initially set out to handle user prepayments, our solution was extensible. All we needed was a new funds flow to apply it to Stripe-funded credits, which has now become the most used credit type.

For example, when Stripe grants an incentive credit to a merchant who signed an enterprise deal, or a courtesy credit issued after a service incident, no real money changes hands. The funding is entirely virtual.

The credits engine triggers an inbound OMT from a cashless corporate account to the merchant’s credit balance account. The ledger records the funding, distinguishing between incentive fundings and drawdowns, then clears it through an OMT lifecycle until the balance lands in the merchant’s logical account, with no cash movement.

When eligible fees are assessed, the fee settlement engine debits the credit balance account and credits the Stripe corporate account via an outbound OMT. A tax write-off event moves the tax amount from the uncollected taxes ledger account to a dedicated write-off account. Neither the merchant nor Stripe pays this tax.

The accrual event uses the fee record’s creation date as its effective timestamp, rather than the invoice date, which is often later, ensuring revenue is recognized in the correct accounting period even when the invoice crosses a month boundary.

### Same experience, different ledgers

These two distinct paths through the general ledger have the same user experience, where credits automatically apply to fees, and users can see their running balance in the Dashboard. Collapsing these into a single accounting flow would have been an auditing nightmare—the tax treatments alone are incompatible. Maintaining two funds flows, each with its own ledger accounts, events, and sign-off process, is well worth it.

* * *

## Lessons learned

**Logical accounts are a powerful abstraction.** We require careful coordination to ensure logical ledger entries don’t accidentally trigger real cash movements downstream.

**Auditability is an engineering requirement.** Every state change is logged with an actor, timestamp, and approval link. Compliance teams never had to ask for logging—it was there from the first commit.

**Different economics need different funds flows.** Prepaid and Stripe-funded credits look identical to users but have fundamentally different ledger models. Collapsing them into a single accounting flow would have been an auditing nightmare. The apparent duplication is worth it.

## Impact and what’s next

Stripe Fee Credits replaced a patchwork of ad-hoc cash incentives, manual adjustments, and discounts with a single programmable primitive that now powers several commercial models: incentives embedded in sales contracts, prepaid annual fee agreements, startup acquisition credits, Atlas onboarding credits, and courtesy credits.

Each program has fundamentally different economics, tax treatment, and lifecycle characteristics, but they all run on the same infrastructure, and we can launch new commercial programs without bespoke engineering work.

Now, sales teams can structure more flexible deals, startups are incentivized to explore more of Stripe’s product suite, enterprise users can prepay on better terms, and our finance teams can audit the entire incentive portfolio in real time instead of reconstructing it from scattered systems.

We’re continuing to expand what credits can do. Here’s a look at some of what we’re building next:

- **Realtime settlement and credit application:** Allowing fees to be settled in near real time on credits and other payment methods, enabling low latency use-cases. We’ll explore how we handled multisettlement in a future post.
- **Global expansion:** Stripe Fee Credits is available in 26+ countries today, and we’re accelerating rollout as we satisfy jurisdiction-specific requirements.

By treating credits as a programmable, auditable financial primitive rather than a backend accounting hack, we’ve laid the groundwork for more flexible commercial programs.

Stripe Fee Credits is built by the Commercial Constructs and Settlement Platform teams within Stripe’s Commerce Systems organization. If you’re interested in building complex yet elegant financial infrastructure at scale, [we’re hiring](https://stripe.com/jobs).
