---
title: Are Harnesses Worth the Cost?
link: https://martinfowler.com/articles/are-harnesses-worth-the-cost.html
source: martinfowler-com
published: 2026-10-13T13:35:00Z
updated: 2026-10-13T13:35:00Z
first_seen: 2026-10-13T18:08:43.541012300Z
authors:
- Martin Fowler
content: extracted
html: 2026-10-13-are-harnesses-worth-the-cost.html
remote_preview:
  url: https://martinfowler.com/articles/exploring-gen-ai/donkey-card.png
---

I went into this experiment expecting a behavioral harness to help a coding agent meet the requirements more reliably, save tokens and prevent wasted work. Giving an agent clear architectural contracts and deterministic test feedback while it coded seemed like an obvious way to help it get the implementation right on the first pass.

For GPT-6 Sol, the harness used about 10.5 times as many tokens, took nearly 3 times as long, and cost about 7 times more, yet both conditions succeeded in all 10 runs.

The results flatly contradicted that expectation and surprised me. Using Codex with GPT-6 Sol at low reasoning effort, the agent produced an application that passed all 178 automated acceptance tests in each of the 10 runs, both with and without the harness. By this measure, the harness produced no observable improvement in the final applications.

**GPT-6 Sol** (10-run averages for tokens, time, and cost):

| Harness | Tokens     | Time            | Estimated cost | Accepted runs |
| ------- | ---------- | --------------- | -------------- | ------------- |
| Yes     | 11,331,970 | 19 mins 53 secs | \~$2.679       | 10 of 10      |
| No      | 1,076,371  | 6 mins 51 secs  | \~$0.406       | 10 of 10      |

The harness increased average time and cost without changing the observed acceptance result. In exploratory runs with Claude Sonnet 4.6, Claude Opus 4.6, and Gemini 3.8 Flash (all at low reasoning effort), each also managed to complete the task unassisted on the first try.

These frontier models handled this task consistently with and without the harness. Could executable feedback help a cost-optimized model achieve similarly consistent outcomes?

Before looking at the results for a cost-optimized model, here is how I set up the experiment:

## Experiment setup

Borrowing [Birgitta Böckeler’s framing](https://martinfowler.com/articles/harness-engineering.html), a behavioral harness combines *guides* and *sensors*. Guides anticipate where the agent might go wrong and steer it before it acts, increasing the chance of a correct first attempt. Sensors observe the results after it acts and provide feedback that helps it self-correct.

For each model, I compared agents working with and without the behavioral harness. Both conditions began with the application brief and baseline repository instructions in separate, private workspaces. Harnessed agents also received the API specifications, examples, and tools described in the inner-loop section below.

![Two isolated coding-agent conditions, with and without a behavioral harness, feed their generated services into an independent evaluator](https://martinfowler.com/articles/are-harnesses-worth-the-cost/experiment-setup.png)

Figure 1: The two development conditions and their independent evaluation

The experiment separated development from evaluation into two distinct loops. The inner loop provided feedback to harnessed agents while they built. Once an agent finished, the outer loop ran 178 automated acceptance tests against its generated service. Both conditions faced the same evaluation, and the inner harness never had access to the evaluation suite’s answer key. Finally, I collated the accepted outcomes, token use, time, and estimated cost.

In this experiment, I treated an application as acceptable only if it started successfully and passed all 178 external acceptance tests. Reliability meant how consistently a model produced an acceptable application across repeated runs. Token use, elapsed time, and estimated cost were separate measures of run efficiency.

## The application brief

Every attempt began with a detailed [application brief](https://martinfowler.com/articles/are-harnesses-worth-the-cost/application-brief.html). I described what the service should do, but left the code-level implementation details to the agent.

The task was to build a backend-for-frontend (BFF) for product discovery. It had to expose a consumer-facing HTTP API, validate incoming requests and downstream responses, and call an Order API. After a successful availability search, it also had to publish only the first returned product to Kafka. Publishing every product would have allowed the agent to pass the data through without implementing the selection behavior.

![The client calls the BFF, which calls the Order service and publishes events to a Kafka topic consumed by an audit service](https://martinfowler.com/articles/are-harnesses-worth-the-cost/application-architecture.png)

Figure 2: The application architecture described by the brief

The brief also specified less obvious behavior. A timed-out product creation had to become a deferred request with a `202` response code and a link in the response header that consumers could poll for a response. A transient search failure had to return `429` with a usable recovery interval; unavailable dependencies or failed event publication had to become `503`; and successful downstream responses still had to obey requested product filters or otherwise return `502`.

![A successful search calls the Order service, publishes the first matching product to Kafka, waits for acknowledgement, then returns the search response](https://martinfowler.com/articles/are-harnesses-worth-the-cost/sequence-diagram.png)

Figure 3: A successful availability search and its Kafka publication

In short, the service was more than a set of CRUD endpoints: the agent had to coordinate HTTP semantics, asynchronous events, schema boundaries, and fault-tolerance policies simultaneously.

This was also a favorable starting point for an unaided agent: the brief spelled out the functional requirements, architectural and technology constraints, detailed descriptions of the external interfaces, response shapes, and failure behavior in unusual detail. I do not yet know how the results would change with a less complete brief.

## The inner loop: behavioral harness

In this experiment, the guides included OpenAPI and AsyncAPI specifications that expressed the interface agreements in machine-readable form, providing a basis for executable checks. They complemented the brief rather than defining a separate set of requirements. The guides also contained request-response examples for these interfaces. The examples covered downstream timeouts, malformed payloads, filter violations, retry sequences, and Kafka event confirmations. An API-development skill supplied implementation practices and guidance on dependency simulation, behavioral verification, and how to diagnose and respond to failures.

The sensors checked contract conformance, used API tests to verify HTTP response values, and asserted observable side effects, including calls to downstream mocks and Kafka publication. Test results, CTRF reports, and API coverage reports returned those observations to the agent during development.

Birgitta also distinguishes *computational* controls, executed by deterministic tools, from *inferential* controls, which rely on model interpretation or judgment. This harness combined both. The behavioral checks produced computational feedback without an LLM judging correctness. Guided by the skill's instructions, the agent still had to interpret those results and decide how to fix failures.

![The coding agent uses an API skill, tests, an HTTP mock, and a Kafka mock while building the BFF](https://martinfowler.com/articles/are-harnesses-worth-the-cost/inner-harness.png)

Figure 4: The behavioral harness and its feedback paths

An agent plugin supplied the skill and MCP tools, accompanied by the contracts and examples. The OpenAPI specifications covered the BFF and downstream Order API; the AsyncAPI specification covered Kafka.

Before implementing the service, the agent read the specifications and examples alongside the brief. The skill directed it to map the application’s components and link each required behavior to its downstream interactions and verification checks. Helper scripts scaffolded those documents, a record of verification evidence, and the commands for building, starting, and testing the service; the agent completed the behavior-specific details.

Configuration tools wired the contracts to the application and dependency mocks. Contract-driven dependency mocks made downstream interactions executable, allowing the agent to exercise its implementation against defined downstream responses, failures, and recovery sequences. Other helpers verified timing constraints. Together, these tools provided both implementation guidance and an executable environment in which the agent could check its decisions.

The agent invoked the harness through the project’s test command. That command validated the configuration and examples, ran the required build and unit checks, started the downstream mocks, and launched the application against them. It then ran the checks described above.

Report summaries identified specific contract, response value, and interaction mismatches. The agent used the skill’s instructions to diagnose them, change the implementation or execution setup, and use a helper to rerun the affected scenarios. Once those scenarios passed, the agent ran the complete suite as a final gate. The feedback loop depended on the agent invoking these tools during development; it did not run automatically after every code edit.

![The agent runs API tests that arrange mock expectations, exercise the BFF, assert HTTP and Kafka behavior, and return results and coverage](https://martinfowler.com/articles/are-harnesses-worth-the-cost/inner-harness-sequence-diagram.png)

Figure 5: The inner-loop test and feedback sequence

## Keeping guides and sensors in sync

One of the big challenges of designing this type of behavioral harness is to make sure the guides and sensors don't contradict each other and confuse the agents. I addressed this issue with the following approach:

Before running any tests, an `“examples validate”` command checked that the example payloads complied with the respective API specifications. Tests then used those specifications and examples to check the implementation, keeping the sensors aligned with their source guides.

The source of those checks mattered. If an agent interprets requirements to write both the implementation and its tests, the same misreading or omission can appear in both. Allowing it to change the expected results when tests fail can also obscure a defect. Here, the contracts and examples represent agreed human expectations in executable form. Contract checks were generated deterministically from supplied specifications, while supplied examples with behavioral fixtures defined value and side-effect assertions. The agent was instructed to preserve those inputs and the skill’s verification helpers, investigate failures, and correct the implementation or execution setup rather than weaken the expectations.

This gave verification expectations a source independent of the implementing agent’s interpretation. Used as a deterministic computational control, they make correctness checkable without relying on the agent's own claims or an LLM's judgment, freeing humans to focus review on what the contracts don't cover.

The same sensors therefore serve two distinct purposes depending on when they run:

- **During development:** Sensor results feed back to the agent as executable attention, steering construction and helping it recover from lapses.
- **At completion:** Sensor results become the basis for accepting the work: an independent check of the agent's interpretation against human expectations, which matters for every model.

The skill also required reviewable evidence of dependency interactions and the final verification result, so completion was supported by recorded observations rather than the agent’s claim alone.

In this experiment, I implemented the behavioral harness using [Specmatic](https://specmatic.io), though equivalent contract-testing and service virtualization tools could provide the same capabilities.

## The outer loop: evaluation harness

To keep the comparison fair, each attempt ran inside an isolated Docker container with its own workspace and a fresh, attempt-specific Codex home. Apart from shared authentication and language dependency caches, each agent began without sessions, memories, plugins, skills, or other state from an earlier run.

The two conditions differed in what agents could use during development; within each comparison, completed applications faced the same external evaluation.

After development stopped, the outer-loop evaluation harness tested each application against the same corpus of 178 combined functional checks, including invalid inputs, downstream failures, recovery sequences, and the messages produced by successful requests. These checks exercised the behavior specified in the [application brief](https://martinfowler.com/articles/are-harnesses-worth-the-cost/application-brief.html), including the client-facing API, downstream interactions, and Kafka publication. The evaluation harness controlled its own downstream fixtures and message capture rather than relying on the application's development tests.

## Testing a cost-optimized model

OpenAI positions [GPT-6 Sol](https://developers.openai.com/api/docs/models/gpt-6-sol) as a flagship model for complex professional work, while [GPT-6 Luna](https://developers.openai.com/api/docs/models/gpt-6-luna) is optimized for cost-sensitive, high-volume workloads and roughly corresponds to the nano tier in earlier GPT-5 families.

With the methodology in place, the core question is whether a model with a smaller capability profile would behave differently.

I ran the exact same experiment on GPT-6 Luna at low reasoning effort. Without a harness, it succeeded in 3 of 10 runs. With the harness, it succeeded in all 10.

**GPT-6 Luna** (10-run averages for tokens, time, and cost):

| Harness | Tokens     | Time            | Estimated cost | Accepted runs |
| ------- | ---------- | --------------- | -------------- | ------------- |
| Yes     | 22,933,648 | 35 mins         | \~$0.273       | 10 of 10      |
| No      | 5,860,736  | 15 mins 56 secs | \~$0.086       | 3 of 10       |

In this sample, the harness did not unlock a capability Luna lacked; it turned a 3-in-10 baseline into 10-for-10 reliability.

Providing that feedback added an average of 17,072,912 tokens, 19 minutes 4 seconds of autonomous runtime, and about $0.187 of estimated API-equivalent cost per attempt. The harnessed attempt used roughly 3.9 times as many tokens overall as an unharnessed Luna attempt. But look at the comparative economics: even with the harness overhead, a harnessed Luna attempt cost about 33% less than an unharnessed Sol attempt ($0.273 vs $0.406), and both conditions achieved the same 10-for-10 acceptance rate in this sample.

## What went better than expected

Looking past the headline pass rates into the codebase, several implementations stood out:

- **GPT-6 Sol solved the brief unaided in one shot.**

  A single `/goal` prompt was enough. All 10 unharnessed GPT-6 Sol applications passed every functional check on the first pass without human debugging or corrective follow-ups.

- **GPT-6 Luna came remarkably close on its own.**

  Unharnessed GPT-6 Luna runs passed 94.8% of the functional checks. It was stumbling over subtle edge cases rather than missing the task entirely.

- **Asynchronous 202 workflows held together end-to-end.**

  Returning a `202 Accepted` status code is straightforward. What surprised me was seeing the agent wire up the monitor endpoint, track pending requests, retry downstream creation with the original idempotency key, and surface the final payload at the polling URL.

- **Kafka publication stayed tied to the search transaction.**

  Rather than firing off a message asynchronously and returning an immediate `200`, accepted applications waited for Kafka acknowledgement before responding, and correctly translated publication failures or timeouts into `503 Service Unavailable`.

- **Boundary validation was consistently defensive.**

  Both models validated incoming payloads against schemas and validated downstream response structures and business constraints before using the data, rather than blindly trusting downstream services.

## Where unharnessed runs slipped up

The failed unharnessed Java applications did not contain invalid syntax or fundamentally misunderstand the brief. Each still passed at least 144 of the 178 combined functional checks. Their failures clustered around the deferred creation workflow and a smaller set of protocol details:

The observed gaps appeared at protocol boundaries, resiliency paths, and integration seams, where a convincing implementation could still fail as a system.

- **Deferred creation broke at the timeout boundary.**

  When a downstream product creation exceeded the configured timeout, the BFF was supposed to return `202 Accepted` with a monitor link, retry in the background, and eventually expose the downstream `201` response. Instead, 4 unharnessed applications returned `503`, 2 returned `400`, and 1 returned `429`. An application already counted above also created monitors in some scenarios, but its retries never reached the terminal `201` response. Several implementations recognized timeouts by matching a narrow exception type or cause-chain shape. The artifacts establish the incorrect responses, but often do not record the actual client exception, so that mechanism is a likely explanation rather than a proven common cause.

- **Missing monitor links caused secondary failures.**

  In 7 runs, the incorrect response to the timed-out creation also meant that no monitor URL was returned. A later evaluator check then failed while trying to use that missing URL. I treat those errors as consequences of the broken `202` flow, not as separate application defects.

- **Kafka delivery succeeded while contract conformance failed.**

  In 3 runs, every expected message reached Kafka, but the evaluator still rejected the publication. Two applications used Spring's JSON serializer and added an unexpected `__TypeId__` header. Another emitted `“categories”: null`, even though the optional field had to contain an array when present. The integration worked mechanically while still violating the wire contract.

- **Error responses missed exact HTTP contracts.**

  Other observed misses were narrower. Although `415 Unsupported Media Type` is a conventional response for an unsupported media type, the application brief explicitly required `400` for invalid requests of this kind, so the evaluator counted `415` as a contract violation. In other cases, invalid requests reached a generic `500` handler, and a missing monitor returned the wrong representation. These were small differences in code, but enough to make the application unacceptable under the external evaluation.

- **Configuration names diverged at an integration seam.**

  Ordinary product creation returned `503` instead of `201`. The generated client read `ORDER_API_API_KEY`, while the recorded launch supplied `ORDER_API_KEY`. That mismatch is a plausible explanation for the downstream failures. In harnessed runs, contract-driven dependency mocks and configuration checks enabled validations of these environment variables before deployment.

## The harness as executable attention

A behavioral harness turns written requirements into environmental feedback, bringing dropped details back into focus when a sensor goes red.

I find myself thinking about this dynamic in terms of *executable attention*.

The behaviors behind these failures were specified in the supplied requirements. The application brief clearly specified the deferred creation workflow, Kafka message contract, and exact error responses. A requirement can be explicit in the brief and still lose influence during implementation. I think of this as an *attention lapse*: the resulting code omits or incompletely implements a requirement the agent was given.

Juggling library choices, boilerplate setup, dependency injection, and asynchronous wiring across a growing context may contribute to such lapses, although this experiment did not establish the cause. Executable attention makes those gaps observable, giving the agent another opportunity to act on the requirement.

- A timed-out creation isn't deferred until the response includes a monitor link and polling eventually reaches the downstream result.
- A Kafka publication isn't valid merely because a message arrived; its payload and headers must both conform to the wire contract.
- An optional field cannot be serialized as `null` when the contract permits only an array or omission.
- An error response isn't correct unless its status, content type, and body all match the client-facing contract.

GPT-6 Sol preserved these details from the written brief in all observed runs. GPT-6 Luna also implemented much of the behavior correctly without the harness, but its accepted outcomes varied. The misses in Luna are consistent with requirements losing influence during implementation. My interpretation is that the harness served as an external memory and attention aid, pulling dropped requirements back into focus whenever a sensor went red.

## The economics of consistent outcomes

Putting both model samples side by side makes the cost of that feedback visible. These are API-equivalent estimates based on recorded token use and model prices, not actual billed costs.

![Average estimated cost and time across both models](https://martinfowler.com/articles/are-harnesses-worth-the-cost/cost-vs-time.png)

Figure 6: Average estimated cost and time across both models

Within the GPT-6 runs, harnessed Luna averaged about $0.273 per attempt, compared with $0.406 for unharnessed Sol; both had 10 accepted applications in 10 attempts. The harnessed Luna attempt therefore cost about 33% less on average in this sample.

All 10 harnessed GPT-6 Luna applications passed the external evaluation and achieved a 100% success rate. This small sample cannot tell us the long-run failure rate or the total cost of achieving an acceptable application.

These figures measure only autonomous generation cost and machine time. They exclude the upfront effort of authoring, reviewing, and maintaining the contracts, examples, assertions, and harness. They also leave out the effort of finding, diagnosing, and repairing an unacceptable result. Reusing interface contracts and checks across implementations could spread the setup cost over time, but I did not measure that reuse or the resulting total cost.

Sol’s 10-for-10 result still depended on independent evaluation. That suggests another comparison: make the sensors available to the agent only at a final verification gate, rather than throughout development. A final-gate workflow would provide an independent acceptance check while minimizing overhead when the first implementation passes. Failures, however, could still require diagnosis and repair cycles. I did not measure that workflow’s cost.

## The boundaries of this experiment

This experiment covered 10 paired runs per model on 1 greenfield Java service, with both models at low reasoning effort. That sample describes the observed outcomes; it cannot establish long-run reliability.

A more demanding architecture or a trickier resiliency scenario might expose gaps in Sol that require a harness. Conversely, a simpler task might run reliably on Luna without this behavioral harness.

The brief specified functional behavior, architecture, technology, and interfaces in unusual detail. I have not tested whether the pattern holds when modifying legacy code, using other languages or reasoning settings, or working from vague or incomplete requirements. A less complete brief could affect both the unaided implementation and the quality of the contracts and examples supplied to the harness. I would be cautious about extrapolating these exact figures to different briefs, architectures, codebases, or team workflows.

Because the harness added guides and sensors together, this experiment also cannot isolate their individual contributions.

## What this means in practice

I would treat these results as a starting point for choosing a model and feedback cadence for a particular task. What changes with model capability is when and how often the sensors run, not whether an independent check is needed.

Model capability changes when and how often sensors run, not whether an independent verification gate is needed.

- **For cost-optimized models:** When a cost-optimized model produces inconsistent results, run a behavioral harness throughout development. It serves as executable attention, catching drift as it happens and turning inconsistent attempts into dependable delivery.
- **For frontier models:** Retain an independent acceptance check even when a model passes consistently on the first attempt. Instead of running continuous checks during development, run the automated tests once as a deterministic final verification gate before acceptance, providing an independent check with minimal overhead.
- **Invest in clear specifications and examples:** The checks can enforce only the expectations humans encode. Review and maintain those expectations alongside the implementation, and use human review to address gaps in coverage.

## Conclusion

My original assumption was too blunt. A behavioral harness is not an automatic win for every model on every task. For Sol, running the harness throughout development added overhead without a detected acceptance benefit; for Luna, harnessed attempts were accepted more consistently and cost less on average than unharnessed Sol attempts in this sample.

That leaves me with a revised, conditional hypothesis:

What changes with model capability is not whether independent sensors are needed, but when they run. For models whose unaided attempts produce inconsistent outcomes, running sensors continuously during development provides executable attention that broadens the set of models a team can use successfully. For frontier models capable of solving the brief in one pass, running those same sensors once as an independent final acceptance gate provides the verification every system requires without the development overhead.

In practice, engineering teams rarely pick a model on raw benchmark intelligence alone. Unit economics, latency budgets, data privacy, and the ability to run on premises often dominate the decision. If a behavioral harness can take a cheaper, lighter model that already fits those operational constraints and make it reliably deliver the goods, that is a compelling option.

I have tested this so far on a single greenfield service with an unusually detailed brief. The next step is to test a “final gate only” condition for frontier models to measure its overhead and how often the gate catches discrepancies. Beyond that, I plan to vary the quality of the brief and the consequences of a missed requirement, then test how well this hypothesis holds up when modifying an existing codebase, switching programming languages, or running open-weights models locally.
