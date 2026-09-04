---
title: Estimating the Productivity of an Autonomous AI Software Engineer
link: https://cognition.com/blog/ai-productivity
source: cognition-com-blog
published: 2026-06-04T00:00:00Z
first_seen: 2026-09-04T12:14:41.802665207Z
content: extracted
html: 2026-06-04-estimating-the-productivity-of-an-autonomous-ai-software.html
---

Six months ago, every CTO was concerned their team wasn't using enough tokens. That trend has reversed as token usage and AI spend have skyrocketed. Engineering leaders are now trying to figure out how to measure actual output, because not every token delivers real value. Some save engineering hours and accelerate projects; others are wasted on useless sessions and bad prompting.

For any organization of sufficient scale, it's essentially impossible to measure value over thousands of sessions & billions of tokens. We set out to automate this with AI.

Predicting real ROI is hard, for reasons we’ll go into later. So we focused on a key sub-task: estimating how many productive engineering hours each Devin session is worth.

LLMs are notoriously bad at time estimates — so we expected this to be a struggle. After carefully tuning the system, however, our model has an of and appears to be unbiased. Individual predictions aren’t perfect, but the model is good enough to be used for estimating aggregated totals. They’re also convertible to dollar amounts using engineering salaries, getting us closer to business value.

In our system, an agent reviews each completed Devin session — first classifying whether it produced useful output, then estimating how long a human engineer would have taken to produce the same work. We validated it by asking human engineers how long they would have spent on the same tasks.

The system is now running with customers. To our knowledge, this is the first automated system measuring AI engineering productivity in production.

### **Choosing a Metric**

How did we land on productive engineering hours as our metric? The first question we needed to answer was what to estimate. Ideally, we'd measure dollar impact directly, such as revenue attributable to features shipped or costs avoided by bugs fixed. In practice, this is still an unsolved problem in our field. It’s incredibly hard for an engineer to know how many dollars of business value they created through the PRs shipped last week.

On the other end of the spectrum, we could measure raw activity: lines of code, commits, PRs, tokens consumed. These are easy to collect but don't correspond to effort. A mechanical refactor can touch thousands of lines in an afternoon; a two-line bug fix can represent hours of investigation. Many valuable tasks — triaging bugs, running analytics queries, reviewing code — produce no code at all.

The middle ground we decided to measure is human engineering hours: how long would a human engineer have taken to produce the same output? Hours are already how organizations value engineering work — salaries and contractor rates are denominated in time. When leadership evaluates an investment, they think in terms of time and cost savings. Hours are standardized across organizations, independent of business context, and convertible to dollars via engineering rates.

But not all hours are equal. For example, if all PRs created by a session were closed, it likely wasn’t valuable. We wanted to measure only productive engineering hours. So, we also had to build a system to classify whether the sessions were actually productive.

### **Collecting a Dataset**

We collected a ground-truth dataset by asking Devin users to review recent representative sessions, and estimate how long each completed session would have taken without Devin. Our dataset consists of sessions from users across a diverse set of enterprise customers. We collected the data via live interviews and a survey.

Every Devin session has a full execution trace: the user's request, every action taken, the resulting code, codebase context. This gives us a record of production engineering work at a level of detail that is difficult to obtain from surveys, aggregate activity metrics, or open-source benchmarks alone.

In the charts below we analyze our dataset. Our dataset consists of a distribution representing real enterprise workloads, spanning a variety of languages, frameworks, session types, and hour estimates.

![Estimating the Productivity of an Autonomous AI Software Engineer](https://cdn.sanity.io/images/2mc9cv2v/production/ece724048769a06de8edc0f2f0d6d450cffd25b1-321x405.svg?w=1600&fit=max)

![Estimating the Productivity of an Autonomous AI Software Engineer](https://cdn.sanity.io/images/2mc9cv2v/production/62dcf097fe86800089156c0d62298b6e7a49e461-321x239.svg?w=1600&fit=max)

![Estimating the Productivity of an Autonomous AI Software Engineer](https://cdn.sanity.io/images/2mc9cv2v/production/4614fe0dba9104666f87ebe8e06c66d03ad32e9d-321x211.svg?w=1600&fit=max)

### **Filtering for Useful Work**

As we reviewed our dataset, we realized that not all sessions correspond to useful work, and that we would need to build a classifier to filter out unproductive sessions.

For sessions with a PR, this is relatively straightforward: if any PR from the session is merged, we include the estimate; if not, we discard it. This is slightly lossy; sessions with all closed PRs can still have delivered productive work, but we wanted to err conservative.

Sessions without a PR are more complicated. We built a classifier to filter out unproductive sessions, which removes around of sessions, depending on the customer. Many non-PR sessions are genuinely productive — e.g., finding unused dependencies, scanning for security vulnerabilities, reviewing a pull request, running analytics queries, triaging a bug — and we retain those. We discard sessions where the agent lacked access to carry out the task, sessions where the agent asked for clarification and the user never replied, and other scenarios where Devin was unable to meaningfully advance the task.

### **Building the Estimator**

After we identified which sessions were productive, we needed to actually estimate their equivalent engineering hours. To do this, we built an estimator agent with two key components: context and prompts.

On the context side, we included as much information about the session as possible — the user's messages, the PR produced (if applicable), the full agent trace (viewing logs, tracing through code, fixing lint, etc.), and additional codebase context from [DeepWiki](https://deepwiki.com/).

On the prompt side, we set aside sessions as a development set for iteration. By manually triaging agent runs on these sessions and reasoning critically about failure modes, we arrived at the following design principles: credit only the work Devin actually saved and compare Devin against a conservative human reference. Concretely, we:

- **Reason about the human's path instead of the agent's**. The agent's own trajectory is sometimes a poor proxy for human effort. Agents take detours, recover from environment and setup failures, and produce artifacts like summary reports that a solo engineer wouldn't. The estimator reasons about the path a human would have taken to the same output, discounting things like retries, environment setup, and artifacts the agent produced along the way.
- **Credit only the work the user did not specify.** The same output can represent very different amounts of effort depending on how much the user already figured out before asking Devin. The estimator looks at what the user actually said to determine how much of the problem Devin had to solve on its own. For example, if a user comes with a bug report and no proposed fix, we include the time to triage the bug, whereas if the user comes with an implementation plan, we only count the implementation time.
- **Account for codebase familiarity.** The same task can take minutes or a day depending on how well the engineer knows the code. Users told us this is one of Devin’s clearest advantages: tasks in unfamiliar or legacy codebases that would cost a developer a day of ramp-up are often delivered quickly. The estimator infers the relevant level of familiarity from what the session reveals. When the user asks how parts of the system work, it includes the exploration time that orienting in the code would have required. When there's no signal either way, it assumes typical familiarity: an engineer who knows the high-level architecture but hasn't memorized every function.
- **Assume relevant expertise**. A recurring theme in our interviews was that the agent lets people do things they could not have done on their own — a backend engineer delivering work that would previously have required frontend and data-science colleagues. Crediting that cross-disciplinary reach would inflate estimates, so we conservatively assume the reference engineer already has the expertise the task demands. This understates the effort in many cases where a human would first have had to learn an unfamiliar language or framework.

### **Evaluation**

![Estimating the Productivity of an Autonomous AI Software Engineer](https://cdn.sanity.io/images/2mc9cv2v/production/63032c3e37c01abfe0ca657580c0299aa436bb71-291x323.svg?w=1600&fit=max)

On the held-out evaluation set of sessions, our estimator has an of and of . The correlation is highly statistically significant (, ). Around of sessions fall within a factor of of the true estimate. Individual estimates are noisy — –× errors in either direction are common — but because errors are roughly unbiased and independent, they cancel as session count grows and the aggregate converges toward the human-reported total.

A lot of the noise comes from variance between users, both in how they estimate and in genuine differences in speed. Roughly half the residual disagreement lies between users rather than within a user's own sessions ( ). We considered per-user calibration, for example, prompting users in-product to give a few estimates for bootstrapping. We decided against it for simplicity and since, for our purpose of aggregation, estimating relative to an "average" user is sufficient.

Our initial, uncalibrated model consistently underestimated. To correct this, we fit a linear regression in log-space: . This is close to a constant multiplier, with a slope slightly below one. Constraining the slope to one (a single multiplicative constant of ) changes every metric by at most . Residuals by bucket show no systematic trend after calibration.

![Estimating the Productivity of an Autonomous AI Software Engineer](https://cdn.sanity.io/images/2mc9cv2v/production/3a453c1422305f789ef0a1b1ad43345ad828b39b-321x168.svg?w=1600&fit=max)

Even after this correction, the total of the human estimates remains the total of the corrected model estimates. This gap is expected: an estimator that is unbiased in log-space becomes biased once its predictions are summed in linear space, systematically underestimating the total. To see why, consider a simplified estimator that is equally likely to be off by a factor of two in either direction. When it predicts hours, the true value is equally likely to be hour or hours, giving an expected value of — above the prediction. Summing such predictions therefore underestimates the true total. Rather than apply a further correction for this, we report the unadjusted figure as a deliberately conservative underestimate.

We also tested simpler predictors to understand how much of the signal comes from the final code change versus the full Devin session. The first regresses a single scalar, the total lines changed (additions + deletions summed across all PRs in the session), against our human estimates. It performed poorly, with an of , confirming that code volume is a weak proxy for engineering effort. We then evaluated an estimator agent given only the trace of the agent's edit tool calls as context, with no user messages or other session activity. It performed better, but still lagged the full estimator, suggesting that important signal lives outside the diff.

These results match how engineering work actually happens. The effort in a session often comes from investigation, diagnosis, environment setup, reasoning through tradeoffs, or producing useful non-code outputs. Those signals are visible in the session trace (user messages, actions taken, intermediate observations, and codebase context) but not always in the final code change.

The regression results below are for the sessions in our evaluation dataset for which we have line-count data (the number of lines added and deleted across the session's pull requests). A total of sessions in our evaluation dataset created PRs, but our integrations with some of our customer's git hosting platforms do not capture diff statistics.

![Estimating the Productivity of an Autonomous AI Software Engineer](https://cdn.sanity.io/images/2mc9cv2v/production/3dcbf349062bffe29769b80408e563d7f3cad2b2-2120x640.png?w=1600&fit=max)

### **Comparison to Prior Work**

Two recent studies have also used LLMs for effort estimation, both positively confirming the feasibility of this approach. [METR (2026)](http://metr.org/notes/2026-02-17-exploratory-transcript-analysis-for-estimating-time-savings-from-coding-agents) used a combination of GPT-4o and GPT-5 to estimate the human-equivalent times from compressed Claude Code transcripts. These transcripts were collected from METR technical staff. On sessions labeled on human ground truth, their estimator had an of . Our is lower likely due to our data being collected from a much more diverse set of users.

[Anthropic (2026)](https://www.anthropic.com/research/estimating-productivity-gains) estimated task duration on open-source Jira tickets using Claude, but the estimator only had the ticket title and description to work with. They had an of ; human developers estimating the same tickets reached . Our system establishes stronger correlation than Anthropic’s ( vs ) because we have far more granular data per session. As we’ve shown in our evaluation experiments, that significantly improves the accuracy.

### **Threats to Validity**

- **Ground truth bias**. Our ground truth is self-reported. In our video user interviews, users were aware they were talking to Cognition, which might bias them.
- **Sampling**. Users who voluntarily responded may skew toward more engaged users. We asked for "representative" sessions, so abandoned or failed sessions might be underrepresented. Nonetheless, since we filter unproductive sessions before aggregating, the evaluation distribution should be closer to the filtered production population than to raw traffic.
- **Hours are not business value**. We measure engineering capacity, not whether that capacity was deployed on high-value work. One hour spent fixing a critical production bug has very different business value than one hour spent on a project that eventually gets canceled. We also don't capture second-order effects, like freeing engineers for higher-leverage tasks.
- **Hours don't account for quality**. If the agent introduces a subtle bug that takes a long time to debug and fix, its uplift on that task is negative. The merged-PR filter removes the clearest failures but not defects discovered after merging.

### **Conclusion**

We presented a system for estimating the engineering output delivered by an autonomous coding agent, measured in equivalent human engineering hours. Validated against users across eight deployments, the estimator has an of on held-out sessions. Individual estimates are noisy but approximately unbiased; aggregated across a deployment, errors cancel and the total converges toward what engineers report. The system is calibrated to underestimate rather than overestimate delivered output. It is currently in use with Devin customers.
