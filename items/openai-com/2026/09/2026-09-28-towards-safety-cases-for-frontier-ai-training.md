---
title: Towards safety cases for frontier AI training
link: https://openai.com/index/towards-safety-cases-for-frontier-ai-training
source: openai-com
published: 2026-09-28T19:00:00Z
updated: 2026-09-28T19:00:00Z
first_seen: 2026-09-29T07:30:54.171276122Z
labels:
- safety
summary: Our early guidelines for safety cases in frontier AI training cover technical safeguards, operational practices, and investigating misalignment incidents
content: extracted
html: 2026-09-28-towards-safety-cases-for-frontier-ai-training.html
---

We believe we are entering a [new era](https://openai.com/index/an-alien-mind/) in which structured safety documentation should be required before continuing any frontier reinforcement learning training run. Ideally, such documentation would rise to the level of “safety cases”—comprehensive, structured, evidence-based arguments about risk which are used in other safety-critical industries. We treat safety cases as an aspirational north star we are building towards, while acknowledging the challenges of making them as rigorous for AI models as for aviation or nuclear power, due to the emergent complexity at each new level of AI capability. We’re working on a framework to codify these practices.

Below are some initial guidelines that we think should be part of such safety cases for frontier AI training. These best practices reflect our current learnings, and we expect them to evolve as we continue iterating on internal processes for careful development. We’re sharing them now to make our current thinking transparent, and invite feedback from the community. Note that this document is focused on frontier reinforcement learning training; internal and external deployment require considering a much broader set of alignment properties.

## 1\. Technical safeguards

Safety cases should cover three aspects of the technical stack: alignment training, containment, and monitoring. These safeguards help ensure that the model does not try to take misaligned actions, and that even if it did, that it would be hard to break containment, and that monitoring would catch it before harm could occur.

- **Model alignment**: The first line of defense should be training models to be aligned; i.e., to act reliably in ways we intend. This could include:

  - **Training environments and grading:** Decrease risks of models developing misaligned behavior, by preventing positive reinforcement of reward hacks during training. This could include:

    - **Automated dataset reviews:** Use agents to find and fix broken RL environments that could allow misaligned trajectories to receive high reward through exploits rather than intended behavior, reducing opportunities for misalignment to be reinforced during training.

    - **Manual dataset reviews**: Supplement automated red-teaming with manual review and quality checks over datasets to identify flawed tasks that could inadvertently reinforce misaligned behavior.

    - **Grader tuning**: Tune graders to penalize attempts from the model trying to exploit or hack RL environments during training.

    - **Prior run analysis**: Run classifiers over traces from prior experiments to make sure graders are working as intended.
  - **Alignment measurement**: Run evaluations that track misalignment propensities of the model to measure the effectiveness of alignment training. This could include:

    - **Offline alignment evals:** Run alignment evaluations during frontier runs to assess whether training is working as intended, and investigate any material regressions.

    - **Backtesting:** Backtest alignment evaluations on previous incidents to confirm that evaluations detect previously misaligned models and are not being overfit to particular incidents.

    - **Track evaluation gaming:** Track eval awareness or metagaming (models recognizing they are being tested) that could induce a behavioral change (e.g., enable eval-awareness and metagaming monitors on alignment evals), with specific blocking thresholds.

    - **Worst-case stress tests:** Elicit worst-case misbehavior and check that at least one evaluation would detect this misbehavior.
  - **Prevent training on chain-of-thought**: Do not let automated graders see the chain-of-thought in reinforcement learning to prevent models from evolving to evade chain-of-thought monitors.
- **Containment**: In the case that a model becomes misaligned, make sandboxing and security strong enough to prevent harmful actions from occurring. This could include:

  - **Multiple layers of infrastructure security:** Invest heavily in hardening both the sandbox the model runs in and the research infrastructure hosting the sandbox, to ensure we have an extra layer of protection in the event of a sandbox escape.

  - **Containment red-teaming**: Improve sandbox, research infrastructure, and perimeter security by iteratively red-teaming these systems using checkpoints from frontier training runs, subject to a safety analysis, to find and close any gaps. Also red-team all services that the model could touch during training (e.g., internet cache).

  - **Cross-sample communication:** Limit high-bandwidth cross-sample communication to approved pathways, such as multi-agent tooling, and close infrastructure side channels that are high-bandwidth and easily discoverable by agents.

  - **Immutable transcripts:** Save agent transcripts immutably for a reasonable period of time (for example, in write-once storage accounts) for all RL training, evaluation, and internal deployment, to enable reliable incident investigation and prevent inadvertent edits.
- **Monitoring**: In the case that models begin to take misaligned actions, have a live monitoring system to catch any potential misalignment, and rapidly address priority issues before a serious incident can occur. This could include:

  - **Enforce monitorability:** Ensure models remain monitorable, as measured by monitorability and monitor evasion evals with clear thresholds, to catch concerning behavior early.

  - **High recall on known issues:** Ensure the monitoring system has high recall on past incidents in a held-out eval.

  - **Eval freshness for monitors:** Create eval datapoints representative of net-new risks for new runs, so measures of monitor recall and precision do not become stale.

  - **Rapid response:** Implement priority alerts that result in action within a defined SLA, either by paging an on-call person or by automatically pausing the affected run, before the model causes harm (e.g., page security on-call and the run owner on incidents during the day, and auto-pause runs for unacknowledged alerts at night).

## 2\. Operational guidelines

Along with recommendations about technical safeguards, we have been working on operational best practices for safety cases for a frontier AI training run. These could include:

- **Dissents (pre-mortems)**: After a safety case is drafted, a member of another team should write a dissent to find potential holes in the safety case and share a calibrated take on risk, which the training team should then address, to help make safety cases stronger.

- **Approvals**: The safety case should be reviewed by members of senior leadership, who should each have the ability to veto the run in order to ensure there are multiple internal checks on the run (e.g., research org lead / VP, Head of Safety, and Chief Scientist).

- **Accountability**: The senior leader responsible for a training run (e.g., research org lead / research VP) should be accountable for the safety case and any incident response (including as part of performance reviews) so that training teams are also incentivized to push for safety and alignment.

- **Pausing**: If issues are found that would invalidate a safety case (e.g., a new security issue), ensure there are runbooks, technical controls, and SLAs for pausing all covered runs.

- **Internal transparency**: Safety cases should be made available to internal oversight groups (e.g., the Safety and Security Committee).

- **Audits:** Auditors should be provided with sufficient access to verify that the claims of the safety case are valid and sound, and to raise gaps if found.

- **Escalations**: There should be a clear process to raise misalignment escalations, with a table of defined misalignment severity levels, and a misalignment on-call that can page executives (e.g., CEO) when a run hits a certain severity level.

- **Technical controls**: It should be challenging for humans and agents to start noncompliant runs. Safety features such as monitoring and auto-pausing should fail closed (e.g., it should not be possible to start runs without appropriate monitoring enabled, or to disable the monitor from within RL training, evaluation, or an internal deployment).

- **Rollback ability**: It should be easy to identify all downstream uses of a misaligned model in the training process (for example: data generation or grading) to enable undoing the effect of misaligned outputs if necessary.

- **Residual risk completeness:** Safety cases should enumerate a comprehensive list of residual risks which are not covered by currently implemented mitigations, as best possible, to enable informed risk-acceptance decisions.

These represent our current recommendations and are in the process of being implemented at OpenAI. We expect our practices to continue to evolve over the coming weeks.

## 3\. Investigations of misalignment incidents

We also have been developing some best practices for investigating severe AI misalignment incidents**.** Labs should seek to learn as much as possible from individual incidents (similar to [investigation practices](https://www.ntsb.gov/Advocacy/Activities/Pages/homendy-20210930.aspx) in other high-stakes industries) to be able to prevent such instances from happening in the future. Some examples of such actions could include:

- **Internal transparency**: As an investigation may take significant time to complete, incident investigations should have periodic updates presented internally (e.g., daily updates for ongoing investigations). Employees should have defined pathways to get more access, including raw transcripts and sampling from misaligned models, if it is safe and relevant for their work.

- **Misalignment root-cause**: Researchers should root-cause training dynamics (e.g., via targeted ablations or resampling experiments) to understand how misaligned behaviors were introduced to better understand the science of misalignment and better prevent it in the future.

- **Postmortem**: An operational and cultural postmortem should be conducted to understand all contributing causes to the incident, such as why issues were introduced and left undetected or unescalated prior to the incident.

- **Detection:** We should develop alignment testing methods capable of discovering the propensity to cause the incident, without directly hillclimbing on information derived from the incident (e.g., transcripts or incident summaries). Incident-derived evals should be created as “regression tests,” to ensure future models do not exhibit propensity for misalignment on very similar incidents.

- **Public disclosures:** Investigation results, postmortems, and operational changes should be [shared with the public](https://openai.com/index/model-misalignment-reporting-framework/) following the conclusion of the investigation. Affected third parties should be notified as soon as possible.
