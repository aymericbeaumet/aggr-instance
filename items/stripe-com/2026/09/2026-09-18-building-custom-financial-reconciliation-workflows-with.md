---
title: Building custom financial reconciliation workflows with Stripe
link: https://stripe.dev/blog/building-custom-financial-reconciliation-workflows-with-stripe
source: stripe-com
published: 2026-09-18T00:00:00Z
updated: 2026-09-18T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Maia Robinson
summary: Stripe’s next generation of reporting APIs are focused on enterprise scalability, and this post shows you how to use them in your financial reporting infrastructure.
content: extracted
html: 2026-09-18-building-custom-financial-reconciliation-workflows-with.html
remote_preview:
  url: https://stripe.dev/og/blog/building-custom-financial-reconciliation-workflows-with-stripe.png
  alt: 'Stripe Developer Blog: Building custom financial reconciliation workflows with Stripe'
extra:
  content:interactive: true
---

Many traditional enterprises still rely on SSH File Transfer Protocol ([SFTP](https://en.wikipedia.org/wiki/SSH_File_Transfer_Protocol)) for core financial reconciliation activities that are critical for finance teams. While SFTP has remained a common standard, it falls short in the age of event-driven architecture, hyper-customization, and continuous iteration. Stripe’s next generation of reporting APIs are focused on enterprise scalability, and this post shows you how to use them in your financial reporting infrastructure.

## Why SFTP falls short

It is true that SFTP remains common in enterprise environments. However, we consistently hear that SFTP proves to be fragile, hard to scale, and lacks the level of self-service customization that complex, dynamic businesses need.

1. Polling dependency and data delays: Statically scheduled batch jobs can leave data behind operational reality. SFTP reporting pipelines are often coupled with cron jobs and polling processes to detect new files, which is inefficient when Stripe offers real-time payout and report run completion events.
2. Customization limitations: Oftentimes, legacy processors have SFTP reports with a fixed set of available columns and set delivery schedules. With Stripe’s API-based custom reports, when your finance team makes a request for a new report field or cadence, there’s no need to go back and forth with Stripe, you can just implement it.
3. Security overhead: Reporting APIs allow you to use the same API key-based permissioning and audit trail as the rest of your services, not a separate SFTP credential/IP-allowlist to manage.

## Stripe’s reporting philosophy

Stripe’s reporting APIs, including the [Reports API v2](https://docs.stripe.com/reports/v2-api) and [Query Run API](https://docs.stripe.com/reports/query-runs), are focused on delivering a developer-first approach to financial reconciliation.

1. Data platform consistency: Businesses often have completely separate datasets and schemas being used for financial reconciliation activities versus BI analytics. By offering a consistent schema across data products like Sigma and Data Pipeline, which also closely align with the API conventions, Stripe provides a more consistent experience across your business’ use cases.
2. Self-service: Developer agency is a non-negotiable for Stripe products, and reporting is no different. Whether you need to add custom metadata as a report column, join multiple resource tables, add a new derived field, or update the report run schedule, you can make changes autonomously.

## Building financial reconciliation reports

Let’s take a fictional business, Galtee, an outdoor equipment retailer. Galtee has a direct-to-consumer business line as well as a business-to-business line that sells products to campgrounds and adventure tourism companies. The Galtee finance team has shared financial reconciliation reporting requirements that Galtee’s engineering team is working on fulfilling using [Sigma](https://docs.stripe.com/data/sigma). The [Query Run API](https://docs.stripe.com/reports/query-runs) allows Galtee to programmatically execute SQL queries as part of a reporting workflow, making it possible to create fully custom reports that don’t need a post-extraction transformation layer.

### Multi-account payout reconciliation

Galtee has created two different Stripe accounts, representing the D2C and B2B business lines, that are grouped under an [Organization](https://docs.stripe.com/get-started/account/orgs). Galtee’s finance team needs a report that includes transaction information for all funds paid out to the Galtee bank accounts for a given day.

The `balance_transactions` Sigma table is the main table for financial reconciliation, because it includes all transactions that contributed to the Stripe account balance. It can be joined with the transfers table to list all transactions that are part of a bank payout on a given date:

```
SELECT
  date(transfers.date) as payout_date,
  transfers.id as payout_id,
  balance_transactions.id AS balance_transaction_id,
  balance_transactions.source_id,
  balance_transactions.type,
  balance_transactions.net,
  balance_transactions.currency,
  balance_transactions.amount
FROM transfers
JOIN balance_transactions
  ON balance_transactions.automatic_transfer_id = transfers.id
WHERE
  date(transfers.date) = date({{PAYOUT_REPORT_DATE}})
```

Then, to run this query across both of Galtee’s Stripe accounts, Galtee can set the API key to be the Organization secret key:

```
curl -X POST https://api.stripe.com/v2/data/reporting/query_runs \
  -H "Authorization: Bearer {{ORGANIZATION_SECRET_KEY}}" \
  -H "Stripe-Version: 2026-07-29.preview" \
  --json '{
    "sql": "{{QUERY_STRING}}"
  }'
```

Once the query completes, Galtee will receive a [v2.data.reporting.query\_run.succeeded](https://docs.stripe.com/api/v2/data/reporting/query-runs/event-types?api-version=2026-07-29.preview#v2_query_runs_event_types-v2.data.reporting.query_run.succeeded) webhook event that can trigger automated report retrieval.

This returns a clean table listing all balance-affecting activity, including payments, refunds, and fees included in a given payout. The sum of the “net” column amounts, minus the successful payout rows, should equal the total cash paid out to Galtee’s bank accounts that day.

### Local presentment and FX in payout reconciliation

Now, let’s say Galtee has started selling cross-border to European buyers and is manually setting prices in EUR. Galtee’s finance team now needs the payout reconciliation report updated to include the presentment amounts and the exchange rate used for the currency conversion of each transaction. This can be easily incorporated by adding the `exchange_rate` field from the `balance_transactions` table and adding the presentment data by joining with the charges table:

```
SELECT
  date(transfers.date) as payout_date,
  transfers.id as payout_id,
  charges.id AS charge_id,
  charges.created AS charge_created_at,
  balance_transactions.id AS balance_transaction_id,
  balance_transactions.source_id,
  balance_transactions.type,
  balance_transactions.net,
  balance_transactions.currency AS settlement_currency,
  balance_transactions.amount AS settlement_amount,
  charges.currency AS presentment_currency,
  charges.amount AS presentment_amount,
  balance_transactions.exchange_rate
FROM transfers
JOIN balance_transactions
  ON balance_transactions.automatic_transfer_id = transfers.id
LEFT JOIN charges
  ON charges.balance_transaction_id = balance_transactions.id
WHERE
  date(transfers.date) = date({{PAYOUT_REPORT_DATE}})
```

Now the reconciliation report supports local currency presentment scenarios and Galtee’s engineering team can immediately deploy this change by updating their [Query Run API](https://docs.stripe.com/reports/query-runs) sql parameter.

### Itemized IC+ transaction fees in payout reconciliation

Next, Galtee’s finance team wants to know the total IC+ fees associated with each card charge on the payout report. The original query can be modified to also join with the `icplus_fees` table and sum all of the IC+ fee rows for each charge:

```
SELECT
  date(transfers.date) as payout_date,
  transfers.id as payout_id,
  balance_transactions.id AS balance_transaction_id,
  balance_transactions.source_id,
  balance_transactions.type,
  balance_transactions.net,
  balance_transactions.currency AS settlement_currency,
  balance_transactions.amount AS settlement_amount,
  charges.currency AS presentment_currency,
  charges.amount AS presentment_amount,
  balance_transactions.exchange_rate,
  CASE
    WHEN balance_transactions.type = 'charge' THEN (
      SELECT SUM(icplus_fees.billing_amount)
      FROM icplus_fees
      WHERE icplus_fees.charge_id = balance_transactions.source_id
    )
    ELSE NULL
  END AS icplus_fee_total
FROM transfers
JOIN balance_transactions
  ON balance_transactions.automatic_transfer_id = transfers.id
LEFT JOIN charges
  ON charges.balance_transaction_id = balance_transactions.id
WHERE
  date(transfers.date) = date({{PAYOUT_REPORT_DATE}})
```

Galtee can also take it a step further and create an itemized row of all IC+ fees to add to each card charge row:

```
WITH

-- Create a table with a single row of all IC+ fees for each charge
icplus_by_charge AS (
  SELECT
    charge_id,
    SUM(CASE WHEN fee_name = 'interchange' THEN total_amount END) AS interchange,
    SUM(CASE WHEN fee_name = 'card_scheme' THEN total_amount END) AS card_scheme,
    SUM(CASE WHEN fee_name = 'volume_fee' THEN total_amount END) AS volume_fee,
    SUM(CASE WHEN fee_name = 'per_auth_fee' THEN total_amount END) AS per_auth_fee,
    SUM(total_amount) AS total_icplus_fee
  FROM icplus_fees
  GROUP BY charge_id
)

-- Final query using the IC+ fees by charge table
SELECT
  date(transfers.date) as payout_date,
  transfers.id as payout_id,
  charges.id AS charge_id,
  charges.created AS charge_created_at,
  balance_transactions.id AS balance_transaction_id,
  balance_transactions.source_id,
  balance_transactions.type,
  balance_transactions.net,
  balance_transactions.currency AS settlement_currency,
  balance_transactions.amount AS settlement_amount,
  charges.currency AS presentment_currency,
  charges.amount AS presentment_amount,
  balance_transactions.exchange_rate,
  
  --icplus_fees columns
  icplus_by_charge.total_icplus_fee,
  COALESCE(icplus_by_charge.interchange, 0) AS interchange_fee,
  COALESCE(icplus_by_charge.card_scheme, 0) AS card_scheme_fee,
  COALESCE(icplus_by_charge.volume_fee, 0) AS stripe_volume_fee,
  COALESCE(icplus_by_charge.per_auth_fee, 0) AS stripe_per_auth_fee,
  COALESCE(icplus_by_charge.interchange, 0)
    + COALESCE(icplus_by_charge.card_scheme, 0) AS total_network_fees,
  COALESCE(icplus_by_charge.volume_fee, 0)
    + COALESCE(icplus_by_charge.per_auth_fee, 0) AS total_stripe_fee
FROM transfers
JOIN balance_transactions
  ON balance_transactions.automatic_transfer_id = transfers.id
LEFT JOIN charges
  ON charges.balance_transaction_id = balance_transactions.id
LEFT JOIN icplus_by_charge
  ON icplus_by_charge.charge_id = charges.id
WHERE
  date(transfers.date) = date({{PAYOUT_REPORT_DATE}})
```

The final Galtee report output looks like this:

| payout\_id   | balance\_transaction\_id | source\_id   | settlement\_currency | settlement\_amount | presentment\_currency | presentment\_amount | exchange\_rate |
| ------------ | ------------------------ | ------------ | -------------------- | ------------------ | --------------------- | ------------------- | -------------- |
| po\_\*\*\*\* | txn\_\*\*\*\*            | ch\_\*\*\*\* | usd                  | 1000               | usd                   | 1000                |                |
| po\_\*\*\*\* | txn\_\*\*\*\*            | ch\_\*\*\*\* | usd                  | 1000               | usd                   | 1000                |                |
| po\_\*\*\*\* | txn\_\*\*\*\*            | ch\_\*\*\*\* | usd                  | 59                 | eur                   | 50                  | 1.18541        |
| po\_\*\*\*\* | txn\_\*\*\*\*            | ch\_\*\*\*\* | usd                  | 1000               | usd                   | 1000                |                |

And the itemized IC+ fee columns for those same rows:

| balance\_transaction\_id | total\_icplus\_fee | interchange\_fee | card\_scheme\_fee | stripe\_volume\_fee | stripe\_per\_auth\_fee |
| ------------------------ | ------------------ | ---------------- | ----------------- | ------------------- | ---------------------- |
| txn\_\*\*\*\*            | -21.05             | -20.5            | 1.95              | -2.5                | 0                      |
| txn\_\*\*\*\*            | -14.95             | -16              | 3.55              | -2.5                | 0                      |
| txn\_\*\*\*\*            | -2.6005            | 0.15             | 0.303             | 0.1475              | 25                     |
| txn\_\*\*\*\*            | -9.55              | -10.25           | 1.95              | -1.25               | 0                      |

*Note: the example above is for illustration of the report format only. Some columns have been hidden for brevity and actual data and amounts have been redacted/modified.*

## Conclusion

Financial reporting has historically been a siloed process that relies on push-based files with fixed templates and static schedules. Whether the reports are delivered directly to finance team members or fed into ERP integrations, Stripe’s [Query Run API](https://docs.stripe.com/reports/query-runs) makes it possible to build an on-demand, flexible reporting pipeline that can evolve alongside your business.
