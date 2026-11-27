---
title: Scaling up your microservice testing with Apache Spark
link: https://stripe.dev/blog/microservice-testing-with-apache-spark
source: stripe-com
published: 2026-06-01T00:00:00Z
updated: 2026-06-01T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Vivek Yadav
summary: Some microservices are difficult to test because their behavior depends on a long tail of inputs that are hard to model by hand. For that class of problem, Apache Spark's massive parallelism and linear scalability can be leveraged to build a regression test harness. That makes it possible to compare old and new implementations, model upcoming rule changes, and quantify impact before a change reaches production.
content: extracted
html: 2026-06-01-scaling-up-your-microservice-testing-with-apache-spark.html
remote_preview:
  url: https://stripe.dev/og/blog/microservice-testing-with-apache-spark.png
  alt: 'Stripe Developer Blog: Scaling up your microservice testing with Apache Spark'
---

## Why replay testing matters

Some microservices are difficult to test because their behavior depends on a long tail of inputs that are hard to model by hand. Payment cost estimation is a good example.

The cost of a card transaction is not determined by one rule. It depends on a changing set of network rules, merchant-specific context, transaction attributes, and program qualifications. The behavior of the system often comes from the interaction among many conditions rather than any single rule.

Unit tests can validate individual rules. Integration tests can validate a few representative flows. But they do not answer the question teams often care about most: what happens across the actual distribution of historical traffic if one rule, rate table, dependency, or qualification condition changes?

That question is becoming even more important in the age of AI-assisted coding. AI tools can help teams refactor, migrate, and generate candidate implementations faster than before. But for systems that affect money movement, billing, pricing, eligibility, or customer-facing explanations, faster code changes need stronger evaluation. A change can look locally correct and still alter behavior in a narrow but important part of the input distribution.

For that class of problem, [Apache Spark](https://spark.apache.org/docs/latest/)’s massive parallelism and linear scalability can be leveraged to build a regression test harness. If the core logic of a service is separated from the request path, the same logic can be run offline across months or years of historical inputs. That makes it possible to compare old and new implementations, model upcoming rule changes, and quantify impact before a change reaches production.

This post describes that pattern: wrapping deterministic service logic in a Spark execution environment so production history becomes an executable testing asset.

### The core idea

A request handler and a Spark transformation often have the same basic shape.

A service receives an input, loads supporting state, applies business logic, and produces an output. It usually does this one request at a time. A Spark job reads a dataset, joins in supporting state, applies logic to each record, and writes an output dataset. The execution model is different, but the computation is often similar.

That symmetry is the useful part. If the service’s core logic is cleanly separated from transport, framework wiring, and side effects, the same logic can be invoked from a Spark job. Instead of sending one request through an HTTP endpoint, the replay job sends millions of historical records through the same decision path.

The output is no longer a single response. It is a dataset that can be queried, joined, diffed, aggregated, and reviewed. For high-impact services, that changes the testing conversation. The team can move from "the tests pass" to "we replayed the candidate implementation across production-shaped traffic and here is exactly where behavior changed".

### A running example: network cost estimation

Consider a service that estimates [network costs](https://stripe.com/guides/guide-to-managing-network-costs) for card payments. For each transaction, the service receives payment context and uses it to estimate the applicable [interchange and scheme fees](https://support.stripe.com/questions/understanding-ic-fees).

A simplified replay input might include three categories of data:

- Transaction facts, such as amount, currency, country, card network, card product, card-present or card-not-present status, authentication method, tokenization state and more.
- Merchant and integration context, such as merchant category, merchant configuration, billing-data availability, [enhanced line-item data](https://docs.stripe.com/payments/payment-line-items) and more.
- Rule and dependency state, such as versioned rate tables, network program rules, eligibility lists, region-specific configuration and more.

The service’s output is not just one number. It may include estimated interchange, estimated scheme fees, the rules that applied, the programs the transaction qualified for, and explanation fields that help downstream teams understand why a cost was assigned. This kind of detail is especially important for businesses on [IC+ pricing](https://support.stripe.com/questions/understanding-ic-fees), where payment costs are exposed to them.

This kind of service is hard to validate with synthetic tests alone. A rule may be simple in isolation, but the behavior of the system depends on how many rules combine for a specific transaction. A small change in qualification logic can move a transaction from one cost outcome to another, and those movements are difficult to predict from hand-written examples alone.

The system also needs to model change. Card networks can introduce new fees, modify existing rates, change qualification criteria, alter incentives for tokenized or authenticated transactions, or change when a fee is assessed. The impact of those changes is rarely uniform. It depends on the shape of the merchant’s historical traffic. That is where replay becomes valuable.

### Regression testing against historical inputs

The most direct use case is regression testing. When refactoring critical logic, the question is not only whether the new implementation passes existing tests. The more useful question is whether it preserves behavior across the actual distribution of inputs the service has processed.

A Spark-based harness makes that measurable. The job can load historical inputs from a data lake, reconstruct the dependency state needed by the service, run the current production implementation, run the candidate implementation, and compare the outputs.

Historical replay also needs a privacy boundary. The useful test case is the decision-relevant shape of the request, not every raw field that appeared in production, so replay inputs should be minimized to the fields needed for the behavior under test. Sensitive identifiers can be tokenized or replaced with stable test identifiers, and fields that are not needed should be redacted, aggregated, or left out entirely. Expected outputs, diffs, logs, and golden datasets should avoid exposing raw customer, user, or payment PII, with access controls and retention policies that match the sensitivity of the source data.

For a network-cost service, the comparison might show how many transactions changed estimated cost, which rule produced the difference, whether the difference came from interchange or scheme fees, and whether the movement is concentrated in a specific part of the business. Instead of relying on a few hand-picked examples, reviewers can evaluate a quantified diff: how many records changed, where the changes occurred, and whether the differences are expected.

This is useful for algorithm refactors, dependency upgrades, framework migrations, and rewrites of business-critical logic. A perfect match is not always the goal. Some changes are intended to alter behavior. Some diffs reveal bugs in the old implementation. The value is that the differences become visible before the change reaches production.

That makes replay especially useful for AI-assisted development. If an AI tool helps produce a refactor or candidate implementation, the replay harness provides a concrete way to evaluate the result against production-shaped data. The question becomes less subjective: did the behavior change, where did it change, and are those changes acceptable?

### Modeling future rule changes

The same harness can also be used for what-if analysis. For the running example, many important changes are not code refactors. They are external rule changes. A network may introduce a new program, change an incentive, add a scheme fee, alter qualification logic, or move a fee so it applies earlier in the payment lifecycle. The engineering problem is not only to encode the new rule. The harder problem is to estimate how that rule will behave across real historical traffic.

A Spark replay harness gives the team a way to answer that question before the change takes effect. The job can load historical payments, enrich them with the relevant decision context, apply the current cost logic, and then apply the candidate future logic. The output is a pair of estimated cost datasets: one representing the current world and one representing the proposed or upcoming world.

The difference between those datasets is the useful artifact. It can show which merchants are affected, which transaction types move, how much of the impact comes from interchange versus scheme fees, and whether the change is concentrated in a particular segment of traffic. This is different from ordinary analytics. The team is not only querying what happened in the past. It is asking what the service would have returned if the decision environment had been different.

That distinction matters. Network-cost changes often involve interacting conditions. Looking at one rule in isolation can be misleading. Replaying the full decision logic across historical traffic gives a more realistic estimate of impact.

The result is useful to both engineers and business teams. Engineers can validate that the new rule implementation behaves as expected. Product and operations teams can understand which users or segments are affected. Account teams can explain likely impacts with more precision. Leadership can evaluate tradeoffs before committing to a rollout. Historical data does not perfectly predict the future, but it is a much better baseline than a hand-selected sample.

### Golden datasets for pull requests

Full historical replay is the heavy-duty version of this idea. For pull requests, teams need a smaller version first: a golden dataset. A golden dataset is a curated set of inputs and expected outputs that represents important service behavior. For the running example, it should cover common flows, qualification boundaries, historical incidents, and cases where small changes in input or dependency state produce materially different outcomes.

The [CI workflow](https://en.wikipedia.org/wiki/Continuous_integration) is straightforward. Store the golden inputs in a durable location, store the expected outputs next to them, run the candidate implementation against those inputs on every pull request, and fail the build if there is an unexpected difference. This gives reviewers a concrete signal: did the observable behavior change, and if so, where? The dataset also becomes a living specification. When an incident exposes a missed edge case or a migration uncovers a subtle difference, that case can be preserved.

Golden datasets need the same privacy posture as larger replays. They should contain only the fields needed to exercise the behavior under test, with sensitive identifiers tokenized or replaced by stable test identifiers. Expected outputs and failure diffs should avoid raw customer, user, or payment PII. This does not replace unit tests. Unit tests are still the right place to validate small pieces of logic and intentionally constructed edge cases. Golden datasets validate service-level behavior using realistic inputs.

### Where this fits in the testing stack

Spark-based replay testing sits above unit and integration testing. Unit tests are still the right tool for small, intentionally constructed cases. Integration tests still validate interactions between systems. Golden datasets cover known service-level examples. Historical replays add coverage from the real production distribution, including inputs the team did not manually design.

That last layer is especially useful for mature systems and AI-assisted development. As services accumulate years of production behavior, the long tail of historical inputs often becomes more informative than any hand-written test suite.

Once the value of replay is clear, the next question is how to build it without creating a parallel version of the production service. We will cover that in [Part 2](https://stripe.dev/blog/microservice-testing-with-apache-spark-part-2) of this post.
