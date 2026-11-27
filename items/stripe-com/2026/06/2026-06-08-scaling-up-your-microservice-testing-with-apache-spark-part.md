---
title: Scaling up your microservice testing with Apache Spark—Part 2
link: https://stripe.dev/blog/microservice-testing-with-apache-spark-part-2
source: stripe-com
published: 2026-06-08T00:00:00Z
updated: 2026-06-08T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Vivek Yadav
summary: Some microservices are difficult to test because their behavior depends on a long tail of inputs that are hard to model by hand. This post focuses on how to build a repeatable Apache Spark replay harness for testing without creating a second implementation of the service.
content: extracted
html: 2026-06-08-scaling-up-your-microservice-testing-with-apache-spark-part.html
remote_preview:
  url: https://stripe.dev/og/blog/microservice-testing-with-apache-spark-part-2.png
  alt: 'Stripe Developer Blog: Scaling up your microservice testing with Apache Spark—Part 2'
extra:
  content:interactive: true
---

## Building the replay harness

In [Part 1](https://stripe.dev/blog/microservice-testing-with-apache-spark) of this blog series on microservice testing, we covered why historical replay is useful for high-impact microservices. Unit and integration tests are still necessary, but they do not show how a candidate implementation behaves across the actual distribution of production-shaped inputs.

This section focuses on the implementation side: how to turn that idea into a repeatable Spark replay harness without creating a second implementation of the service.

### Representing dependencies as data

The main implementation challenge is dependency state. A production service rarely makes a decision from the request payload alone. It may read database rows, load configuration, check feature flags, call another service, or use previously computed state. A replay is only useful if it can reconstruct enough of that context to make the result meaningful.

In a Spark harness, these dependencies need to become datasets. In the running example, live reads can become joins against historical snapshots. Versioned rules can be loaded as replay inputs. Merchant configuration can be joined by merchant and effective date. Program eligibility can be represented as a time-aware state. Calls to other services can be replaced with logged request and response pairs. Side effects can be written as output rows instead of being executed.

The right level of fidelity depends on the decision being tested. A replay designed to validate a rate-table refactor requires a different context from one designed to model a new authentication-related cost rule. A replay designed to validate enhanced-data logic may need a different view of the world than a broad scheme-fee update. The harness does not need to simulate the entire production environment; it needs to capture the state that affects the decision being tested.

### Code structure matters

This approach works best when the service has a clean separation between orchestration and decision logic. The core logic should accept explicit inputs and dependencies. It should not reach directly into request-scoped framework objects, global state, live databases, or external services. Side effects should be isolated behind interfaces so they can be replaced or captured in the replay environment. Some systems cannot make that separation cleanly, especially when distributed state or event-driven dependencies affect future inputs; in those cases, the replay should model the assumptions it can support rather than claim to simulate production perfectly.

Before that extraction, the request path often looks like this:

```
CostResponse call(CostRequest request) {
  Context context = loadContext(request);
  CostResult result = evaluateCost(request, context);

  storage.write(request, result);
  publisher.publish(result);
  metrics.emit(request, result);

  return CostResponse.from(result);
}
```

After the extraction, the deterministic decision is callable from both the online service and the replay harness:

```
CostResponse call(CostRequest request) {
  ReplayInput input = requestMapper
.toReplayInput(request, liveContextLoader.load(request));
  
  // The same decision engine can be invoked by Spark as it behaves as a library
  CostResult result = decisionEngine.evaluate(input); 

  sideEffects.record(request, result);
  metrics.emit(request, result);

  return CostResponse.from(result);
}
```

The important change is the boundary. The decision engine receives a complete input and returns a result. The online service still owns request parsing, live dependency loading, writes, publishing, and metrics. The Spark harness can build the same input from historical datasets and call the same decision engine offline.

If the service is already structured this way, the Spark wrapper can stay thin: read data, reconstruct context, invoke the core logic, and write outputs. If not, building the harness will expose useful coupling to remove. The same refactoring that makes the logic replayable also makes it easier to unit test, migrate, review, and reason about.

This is especially valuable during migrations. When a team is already creating a new implementation, it is a natural time to define cleaner boundaries and make the core logic callable outside the request path. The harness can then compare the old and new implementations across historical traffic.

In that sense, the Spark harness is not just a testing tool. It is also a forcing function for better service boundaries.

### The shape of the Spark wrapper

The Spark job should be a wrapper around the production logic, not a second implementation of it. A common structure looks like this:

![Diagram of Spark wrapper structure](https://stripe.dev/images/microservice-testing-with-apache-spark/diagram.png)

In code, the wrapper might look like this:

```
Dataset<HistoricalEvent> events = spark.read().table("historical_events")
        .as(Events.encoder());

Dataset<RuleSnapshot> rules = spark.read().table("rule_snapshots")
        .as(Rules.encoder());

Dataset<MerchantContext> merchants = spark.read().table("merchant_context")
        .as(Merchants.encoder());

Dataset<ReplayInput> inputs = replayInputBuilder
        .build(events, rules, merchants);

Dataset<ReplayDiff> diffs = inputs
        .map(
                input -> {
                  CostResult current = currentEngine.evaluate(input);
                  CostResult candidate = candidateEngine.evaluate(input);
                  return ReplayDiff.from(input.traceId(), current, candidate);
                },
                ReplayDiff.encoder()
        );

diffs.write().mode("overwrite").saveAsTable("cost_replay_diffs");
```

The wrapper is responsible for data loading, joins, object construction, distributed execution, and output writing. Spark’s [DataFrame and Dataset APIs](https://spark.apache.org/docs/latest/sql-programming-guide.html) are a natural fit for this kind of workload because replay inputs and outputs are structured datasets, and the final analysis often uses [Spark SQL](https://spark.apache.org/sql/) to group and inspect differences.

The core business logic remains in the service library. That boundary is important. If the Spark job reimplements the rules, the replay system can drift from production and produce false confidence. For JVM- or Python-based services, this usually means extracting the deterministic logic into a library that can be used by both the online service and the offline replay job. The online service handles request parsing, live dependency calls, and side effects, while the replay job handles historical data reconstruction and bulk execution. Both paths should execute the same core decision logic.

The output schema should be designed for debugging, not just comparison. For each record, it should include identifiers that allow the team to trace the original input, the current output, the candidate output, selected intermediate values, and explanation fields for the rule path taken. For aggregate analysis, it should include the dimensions needed to explain impact to the relevant engineering, product, or business audience.

A diff that only says "cost changed" is not enough. The useful diff explains why it changed.

### Practical limits of replay

Replay is useful because it makes historical production behavior executable, but it is only as good as the decision state the job can reconstruct. The transaction payload alone is not enough. The replay also needs the relevant configuration, rule versions, eligibility state, and dependency outputs that would have affected the original decision. If that context is incomplete, the result can look precise while being wrong.

Some side effects also cannot be cleanly isolated. Services with distributed state, event-driven dependencies, asynchronous workflows, or live dependencies with time-varying behavior may have effects that change future inputs. In those cases, the replay harness should capture the assumptions it can model, write side effects as observable output where possible, and avoid claiming to be a full production simulation.

The harness does not need to simulate the entire production environment, but it does need to model the parts that affect the decision being tested. A rate-table migration may need exact historical rule versions. A program-qualification change may need eligibility state and data-quality signals. An authentication-related cost change may need clear scope assumptions.

Historical replay also has limits when future behavior depends on external actors. It can estimate how cost logic would apply to past traffic under a new rule, but it may not fully predict issuer behavior, latency, authorization rates, or changes in customer mix. In those cases, replay should be treated as one layer of evidence, not the entire validation strategy.

The output should make these limits visible. A useful replay does not only produce a changed cost value; it also records the rule path, the relevant inputs, and enough explanation to debug why the candidate result differed from the baseline. That makes the replay useful for review, not just measurement.

The goal is not a perfect simulation. The goal is to reconstruct enough of the decision environment to make high-risk changes safer before they reach production.

### Conclusion

For many microservices, the most valuable test cases already exist in logs, warehouses, data lakes, and historical event streams. A Spark-based harness makes those inputs executable, so teams can measure regressions, model future rule changes, and review high-risk changes with concrete evidence.

Migrations are often the best time to build this kind of harness. The team is already creating a new implementation, defining new boundaries, and deciding how the service logic should be packaged. That makes it easier to separate deterministic business logic from request handling, live dependencies, and side effects. The same harness then becomes useful immediately: it can replay historical traffic through the old and new implementations and produce the diff needed to validate the migration.

That is why the setup effort can pay off quickly. The harness is not only an investment in future testing; it directly reduces the risk of the migration that motivated the work in the first place. Once the harness exists, the payoff compounds. Major refactors, rate-table updates, configuration changes, AI-assisted rewrites, and future migrations can be evaluated against production-shaped data before they ship.

Start by choosing one decision path where regressions are expensive. Extract the deterministic logic behind a clear interface, build a small privacy-safe golden dataset, and run it in CI. Once that gives a useful signal, expand the dependency model and run larger historical replays for migrations, rate-table updates, configuration changes, AI-assisted rewrites, and future refactors.

The benefit is a different kind of confidence: high-risk changes can be evaluated against the history of how the system actually behaved.
