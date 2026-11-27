---
title: Making Failure Diagnosis Reliable with Pensive AI
link: https://netflixtechblog.com/making-failure-diagnosis-reliable-with-pensive-ai-6b0811a1c015?source=rss----2615bd06b42e---4
source: netflixtechblog-com
published: 2026-10-26T17:04:57Z
updated: 2026-10-26T17:04:58.324Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Netflix Technology Blog
labels:
- data-engineering
- data-science
- maestros
- orchestration
- pensive
content: feed
html: 2026-10-26-making-failure-diagnosis-reliable-with-pensive-ai.html
remote_preview:
  url: https://cdn-images-1.medium.com/max/1024/1*2TSVpCdPYj4fMMlFPuheCg.png
---

By [Praneeth Yenugutala](https://www.linkedin.com/in/praneethy91/), [Victor Dusa](https://www.linkedin.com/in/victor-dusa/), [Anastasia Samoilova](https://www.linkedin.com/in/anastasia-samoilova01/), [Megan Ren](https://www.linkedin.com/in/megan-ren-7b78a81a8/), [Justin Wei](https://www.linkedin.com/in/justinwei96/), [Marcelo Mayworm](https://www.linkedin.com/in/mayworm/)

At Netflix, data and machine learning practitioners run millions of workloads each day across orchestration systems and compute engines such as Titus, Spark, Trino, and Druid. On peak days, roughly 100,000 workloads fail. When workloads fail, the failure itself is only the beginning. Someone still needs to find the useful signal in several logs, decide whether the cause sits in their code or the platform, and determine whether to retry, reconfigure, or escalate. Even a small share of difficult failures can consume engineering time, waste compute, and interrupt critical data pipelines.

Our first answer to that problem was [Pensive](https://netflixtechblog.com/auto-diagnosis-and-remediation-in-netflix-data-platform-5bcc52d853d1), an auto-diagnosis and remediation system in Netflix’s data platform. Pensive collected logs from failed workloads, used regular expressions and hand-authored Java rules to extract and classify errors, then passed retry or remediation guidance to products such as [Maestro](https://netflixtechblog.com/maestro-netflixs-workflow-orchestrator-ee13a06f9c78). It made ~ 50% of failures self-service and, in some cases, automatic.

Rules work well for known patterns, but their coverage grows only when someone recognizes a new failure mode, understands the logs, writes a rule, and keeps it current as engines and log formats change. Over one three-month period, Pensive processed roughly seven million job-failure classifications, yet about half fell into the unknown bucket. We needed a way to expand coverage without giving up the deterministic parts of Pensive that already worked.

To solve this ambiguous and complex problem at Netflix scale, we invested in Pensive AI to add AI-assisted error extractions and focused diagnosis and remediation guidance to our data and ML practitioners. The goal was not simply to place a model in front of a large collection of logs. We needed bounded extraction, predictable evaluation inputs, responsive product behavior, and quality checks that could follow the system from local development through production. Those requirements shaped every layer of the design, which we will discuss in this blog. Pensive AI wraps AI around deterministic processes, which gives broader coverage, low latency, controlled AI costs, and easier maintenance across the software development lifecycle. Building on this foundation, we plan to explore more agentic debugging experiences, as Part 6 describes.

Early results are encouraging. AI error extraction now covers 100% of failures, including the failures that legacy Pensive classified as unknown. Failed step attempts where no one used Pensive AI averaged 321% more manual debug-log exploration actions than attempts where someone did (an observational comparison, not a causal claim).

![](https://cdn-images-1.medium.com/max/1024/1*2TSVpCdPYj4fMMlFPuheCg.png)\
*Figure 1. Pensive AI architecture. Always-on error extraction and on-demand debugging complement each other and share log sources, the LLM gateway, tracing, and the cache. Archived logs feed offline evals and release gates.*

The two paths complement each other. Error extraction improves an existing capability and is always on. It lets the user of a failed Maestro workload quickly check the parts of the logs that may point to an immediate root cause. It has low latency and low cost, and it can help in a large share of cases. On-demand debugging enables a new capability. It looks at symptoms across the logs and adds the LLM’s own knowledge and reasoning for a deeper analysis, at slightly higher latency and cost. Extraction gives a fast first look at every failure, and on-demand debugging triggered by the user goes deeper when that first look is not enough.

### Part 1: Finding the useful errors

Legacy Pensive rules are precise, but each one captures a particular log shape. A simplified rule looks like this:

```
- name: SparkContainerOOM
logSource: GENIE_STDERR
restartable: false
regex: "Container.*is running beyond physical memory limits.*Killing container"
summary: Spark AM container runs out of memory
```

With Pensive AI, we wanted to find useful errors that had no corresponding rule and adapt more easily when familiar errors changed shape after a platform upgrade. Data Portal (our UI for data pipelines) surfaces these extracted errors when available and links back to the source logs, giving practitioners a focused starting point for triage. That raises a question: how can we ask a model to identify an error without asking it to repeat a long stack trace token by token?

Producing output tokens is much slower than reading input tokens. A direct transcription approach would make long errors expensive and slow. Instead, Pensive numbers the lines of each log and gives the model a sequence of {lineNumber, text} records. The model identifies the first and last lines of the error span, and Pensive reconstructs the selected text from its own log copy, leading to lower cost and better latency. This way, the model always returns two integers (start and end line number), whether the selected logs span a few lines or hundreds of lines. The displayed content comes directly from the original log, reducing hallucinations. The model can still choose the wrong range, which is why extraction quality remains an integral measurement for our AI evals, as we will discuss later.

For orchestrated workflows, “Pensive AI Error Extraction” runs automatically whenever a step fails. A step attempt is one execution of a step within a Maestro workflow. Maestro already calls Pensive for a regex-derived error and restartability classification, so AI extraction runs in the same request and returns its results alongside the established fields. The overall Pensive response must return within Maestro’s hard 90-second post-processing timeout for a failed step attempt.

![](https://cdn-images-1.medium.com/max/1024/1*LwR_-PjFMZiZzmw8QrkyzQ.png)\
*Figure 2. Error extraction. Legacy rules always run. The AI call is bound by a shared deadline, and late calls are excluded. Pensive merges both results into one response and caches it.*

That deadline shaped the extraction path. Pensive extracts errors from the logs in parallel to reduce latency, and the per-log calls share one deadline set well inside Maestro’s timeout limit. A per-log extraction call that misses the deadline is excluded from the result, allowing the overall request to return on time.

The data contract between Pensive AI and the various consumers needed the same care. The legacy data model contained one flattened error string; AI extraction instead returns a list of errors, one for each log source, with deep links back to the originating logs. Changing the old field in place would have broken existing clients. We introduced the new data model beside the legacy non-AI error extraction data model and continued producing both. Data Portal, our user interface for monitoring and observing data pipelines, preferred AI errors when available and fell back to the legacy value when they were not during the rollout. That allowed a gradual rollout and ensured that historical failures still display an error, since we chose not to backfill the old data due to log retention issues.

The migration is still in progress today. The Data Portal UI surface used for failed Maestro steps reads the new AI extracted error field today, while the query editor (for editing and running SQL), Workbench notebooks (managed compute platform and UI to edit and run jupyter based notebooks) and some slack clients calling Pensive still use the legacy error extraction path. Each client needs a compatible schema change before it can consume the new data. Keeping both fields populated lets teams adopt the new behavior at their own pace without forcing a coordinated cutover as we continue evolving Pensive AI.

### Part 2: Full failure diagnosis and the product experience

Beyond error extraction, we built Pensive AI Debugging, a tool that analyzes failed step attempts and provides root-cause analysis and suggested remediations. It is available through programmatic APIs, MCP, and the product UI (Data Portal), all backed by a common entry point across the Netflix Data Platform. This shared model supports consistent behavior and custom product experiences.

This diagnosis still has to arrive quickly and remain consistent across investigations. A debugging response can take tens of seconds or a minute to generate, compared with a few seconds for error extraction. We run full AI-based debugging on demand instead of automatically, which keeps costs under control because most failed steps are never opened. We treat the failed step attempt, not the person who triggered it, as the unit of work, so everyone investigating the same failed attempt sees the same diagnosis.

That choice gives us a stable unit of work for caching. We cache each debugging response by step attempt. A second request becomes a key-value read instead of another AI inference call. The cache also prevents duplicate work, reducing latency and lowering costs further. This also means that a debugging generated in the Data Portal is cached and immediately retrieved for the MCP path instead of incurring another AI inference call. A conditional write creates an in-progress record, one caller starts generation, and other callers wait for the shared result to avoid conflicts when concurrent requests come in for the same step attempt failure. If generation remains in progress past an abandonment timeout, Pensive treats the record as abandoned and reclaims it. Completed responses expire after 90 days to help control costs and optimize storage because debugging requests for much older workloads are uncommon.

Pensive AI supports multiple ways to initiate debugging responses. A caller can (1) read a cached response, (2) generate one if no cache entry exists, or (3) regenerate and overwrite an existing response. Those semantics support the product UI, programmatic access, and the evaluation harness while keeping the response associated with a single immutable workload failure.

![](https://cdn-images-1.medium.com/max/1024/1*2TdUH-b7FY1G2AvfHVu4QA.png)\
*Figure 3. On-demand debugging. Three client actions share one entry point. A conditional write claims the step attempt. A cache hit returns the cached result. A won claim generates and streams a response, and the result persists even if the client disconnects.*

For users, the most important behavior is that a long-running answer feels alive. Pensive streams the response as it is produced. The service emits server-sent events, and the GraphQL layer republishes them as a WebSocket subscription for the Data Portal client.

Each event carries the complete response generated so far. That duplicates a small amount of text, but it keeps the client simple: it does not need a replay buffer or token reassembly logic, and the next event corrects the display after a dropped or late event. Generation is independent of the connection that requested it. A disconnect does not cancel the work, and the final answer still reaches the cache. A hard reload can show a loading state and poll for the completed result. Within the single-page application backing the Data Portal UI, active streams are maintained independently of view and component lifecycle, so navigation does not interrupt a response in progress.

### Part 3: Measuring and improving the behavior we want

LLM-assisted systems need more than a demo or a few working examples. Before shipping in production, we need confidence in both error extraction and debugging guidance, and we need to know that we have not regressed on failures that legacy Pensive already handled. We built separate evaluation frameworks focused on the two distinct jobs.

Building labeled data is often the expensive part of evaluation. For error extraction, legacy Pensive’s hand-written rules provide reference labels for failures they recognize. They also serve as excellent ground truth in most cases. We run those rules over production logs and compare their extracted spans with Pensive AI’s output. This gives us a reliable regression set without relabeling every example by hand, though it is not an absolute ground truth (since our human-built rules could also have been stale). For now, we keep these rules frozen as a label source. This lets us continue to build comparison evals and run them on top of the existing system.

Stable inputs are essential for repeatable evaluation experiments where we A/B test new models/configurations or context injections to improve response quality. The logs Pensive AI uses can expire after as few as seven days or as many as 30 days. Production Pensive AI requests archive fetched logs to S3 object storage asynchronously during workload execution time so the upload is not on the user-facing critical path. Uploads do not overwrite an existing object, preserving the exact snapshot used to create an evaluation case. Evals use those archived logs, so score changes remain attributable to the system under test. The archive also prevents parallel experiments from overloading the source systems and lets us run them without affecting production. Cached S3 log inputs further reduce costs when running evaluations.

Concretely, we measure four complementary signals for error extraction:

- **Inclusion:** An LLM judge compares the legacy Pensive error for a failure with the errors Pensive AI extracted and determines whether the known error is still represented, even when wording differs.
- **Precision:** A second judge sees only the extracted AI errors and evaluates whether each is meaningful and actionable on its own.
- **Repeated noise:** A script looks across the whole dataset for lines that recur too often. A generic status line may look harmless in one answer but become distracting if it appears in a large share of failures. Lines appearing in more than about 30% of workflows are candidates for explicit exclusion. In practice, the exclusion list has only three lines, each of which otherwise appeared in nearly every extracted error.
- **Latency:** We record the latency of every extraction call. Quality scores alone cannot reveal when an apparent improvement requires too much additional work, which often correlates with increased AI costs.

This combination gives us an actionable feedback loop. Prompt changes can reduce general noise, while the dataset-level view exposes the stubborn boilerplate that prompt changes do not eliminate. Existing rules act as a regression floor, and archived logs give us a repeatable way to compare changes. At Pensive AI’s scale, model and system choices must satisfy quality, latency, maintainability and cost constraints together.

To seed the debugging ground-truth dataset for evaluation, we mine internal engineering-support Slack threads and use an LLM to identify threads that link to an orchestrated workflow, describe a resolved problem, and match the class of issue Pensive AI handles. A second model synthesizes a reference debugging response from the relevant discussion. Paired with logs from that workflow failure, it becomes a concrete evaluation row in our evals dataset. This approach works only retrospectively, because Pensive AI in production aims to provide debugging guidance before such a Slack discussion happens.

We also made the debugging generation process visible. Tracing follows the request through Pensive, with child spans for each log fetch and extraction tool call. Our tracing backend based on Braintrust brings the prompt, completion, tool calls, and latency into a single view. During early product development, that visibility helped us inspect low-performing debugging responses in context instead of guessing which input or operation caused them. Tracing helped us improve the responses and fill context gaps even before we had comprehensive evals. In production, the API and UI return the trace identifier with the response, so feedback can lead directly to the trace for in-depth analysis.

### Part 4: Keeping quality in the release path

Evaluation is not only for experiments with LLM orchestration, models, prompts, or context changes. Changes to log selection, trimming logic, or a source system can change what the model sees and therefore change extraction quality. We use the same evaluation harness and evaluators throughout local development and Pensive AI deployment so those effects are visible before users encounter them.

For each pull request, the code branch is deployed to an isolated preview environment and a CI job runs an experiment with our evals against that isolated environment. Precision and inclusion thresholds gate the merge. The job also runs after a build is promoted, catching interactions that were not present when individual pull requests were evaluated in isolation. If the promoted build misses its thresholds, it alerts the on-call engineer to investigate or roll back.

The evaluator runs on a schedule against production to surface gradual drift or external changes. Developers can also trigger evaluation experiments remotely while iterating, using the same deployed-environment path as CI. Consistent infrastructure makes the results comparable across local iteration, preview builds, promoted builds, and production.

### Part 5: Looking for feedback signal from users

The system is only valuable if it reduces the amount of manual debugging and log chasing done by our users. We wanted a measurement that was available from product telemetry and close to the behavior Pensive AI was designed to change. Manual debug-log exploration became one of the main signals. The Pensive interface already offers links to logs for a failed step attempt. To estimate whether the feature reduced raw-log hunting, we compared failed step attempts where someone interacted with Pensive AI with those where no one did. The second group averaged 321% more manual debug-log exploration actions. This observational comparison does not establish causality or a specific reduction in mean time to resolution, but it suggests that focused diagnosis often allows practitioners to inspect fewer raw logs. Programmatic MCP access also lets teams build operational flows with their terminals or custom agentic experiences around the same diagnosis service.

Another more explicit signal we collect is thumbs up and thumbs down on the debugging response. These feedback signals land directly alongside the internal traces so we can quickly associate feedback to the complete log of inputs/outputs and the path the AI took to arrive at the answer. More importantly, we created an internal business-hours alerting system where each thumbs down response sends us a Slack alert. This was valuable to resolve a lot of rough edges and context gaps initially in the product and is helping us improve the product continuously even today.

![](https://cdn-images-1.medium.com/max/1024/1*b_G74zKbN6phiKnMAcDSzA.png)\
*Figure 4. Feedback loop. A thumbs up or down joins the exact LLM trace that produced the response, by the same logId stamped during generation. Traces and feedback feed ground truth and eval datasets, and each thumbs down sends a Slack alert during business hours.*

### Part 6: Future Work building on these foundations

Pensive AI is the foundation for more work that goes beyond diagnosis and recommended remediations. We are exploring “Pensive AI Deepthink,” an opt-in managed agentic experience with higher cost and latency. It builds on the precise, low-latency analysis produced by Pensive AI Debugging and gathers additional context from internal documentation, manuals, past Slack discussions, source code, read-only warehouse queries, and Spark execution details. A routing step directs the investigation to the appropriate skill, such as Spark failures, data-audit failures, or late workflows. This keeps the shared starting point stable and deterministic while allowing the investigation to become more specialized, further improving recall. We are also in talks with the Spark team to leverage Pensive AI responses for failure debugging analysis and A/B comparison for upcoming platform migrations.

We also plan to invest in “Pensive AI Error Classification”. Hard-to-maintain legacy rules also classify failures by restartability and by whether they are platform or user caused. Maestro acts on those labels and changes its retry behavior accordingly like failing fast or tuning the default number of retries (platform errors are tried more than user errors by default). Error classifications must meet a higher bar than error extractions and debugging alone. Moving the majority of the error classifications to AI would let us retire most of the remaining rule-maintenance burden and avoid retrying unknown failures that were unlikely to succeed. For restartability, the costliest error is classifying a recoverable failure as non-restartable, because Maestro will turn a step that could have been recovered into a hard failure, leading to end user alerts and manual remediations. The evaluation framework will need to be tuned around that outcome before AI-enabled classifications can take on those decisions. Equally important, we must avoid retrying non-transient failures that will not succeed, because retries waste compute and delay data pipelines.

Our work on Pensive AI in Data Platform addresses concrete user pain points in debugging, improves maintainability for platform owners, and improves precision and recall for error extraction. Pensive AI extends coverage to failures that legacy Pensive previously classified as unknown. Reliability comes from the system around the AI model calls: bounded error extractions, compatible client data contracts, responsive delivery, reproducible evaluations, caching and tracing, and release controls that keep quality and cost visible as the platform changes.

### Part 7: Acknowledgements

This work would not have been possible without our stunning colleagues across Data Platform and our Data Engineering and Analytics partners who provided early feedback. While we cannot name everyone individually, we are deeply grateful for their contributions, which shaped both response quality and the product experience. Special thanks to [Adith Swaminathan](https://www.linkedin.com/in/adith-swaminathan-98198a21/) for advising on evaluations and inference improvements and acting as a thought partner. Thank you to the Orchestration team members, especially [Deepak Ramalingam](https://www.linkedin.com/in/rdeepak2002/) and [Malcolm Ramirez](https://www.linkedin.com/in/malcolm-ramirez/) for their additional engineering contributions.

For dogfooding and integrating Pensive AI as part of user support, we also thank the engineering support organization (ESO Team). We also thank Abhishek Kapatkar (Data Developer Experiences Team) and Emmie Dong (Data Movement Team) for their feedback to improve the Pensive AI integrations with the rest of the ecosystem. Special thanks to the AI Platform team (Kyle Ahn, Rob Mann) and Braintrust Team for helping with distributed tracing, evals infrastructure and model gateway runtime support.

We also thank Kamran Masood, Ely Spears, Ronald Leung, Anjali Norwood, Prashanth Ramdas, Charles Smith, Harikrishna Menon and other leaders for their constructive feedback and support on the Pensive AI project.

![](https://medium.com/_/stat?event=post.clientViewed&referrerSource=full_rss&postId=6b0811a1c015)

* * *

[Making Failure Diagnosis Reliable with Pensive AI](https://netflixtechblog.com/making-failure-diagnosis-reliable-with-pensive-ai-6b0811a1c015) was originally published in [Netflix TechBlog](https://netflixtechblog.com) on Medium, where people are continuing the conversation by highlighting and responding to this story.
