---
title: 'How to Automate Failure Triages and 10x Test Generation: What We''ve Learned Deploying AI Across HIL/SIL Workflows'
link: https://cognition.com/blog/how-to-automate-failure-triages-and-10x-test-generation-what-weve-learned-deploying-ai-across-hilsil-workflows
source: cognition-com-blog
published: 2026-05-11T00:00:00Z
first_seen: 2026-09-04T12:14:41.802665207Z
content: extracted
html: 2026-05-11-how-to-automate-failure-triages-and-10x-test-generation.html
---

Over the past year, we’ve deployed Devin across multiple OEMs on HIL/SIL workflows. While companies have been augmenting hardware-in-the-loop (HIL) testing with software-in-the-loop (SIL) for over a decade, there is significant variance in actual adoption by companies. There are even fewer examples of how AI can be effectively deployed across HIL/SIL workflows. The problem remains - requirements and ticket volumes continue to grow, while engineering capacity hasn’t kept up.

Through working with customers like RV Tech and Mercedes, we've found that AI can shorten development **->** validation **->** release timelines, improve test quality and increase test coverage. We've also seen teams save thousands of engineering hours monthly through automated failure triaging. On the frontier, we are also beginning to see what AI can do in terms of moving bottlenecked HIL tests to SIL, reducing late-stage HIL failures.

* * *

## What We've Learned

### Building Automated Triage Pipelines

Typically, teams wake up to thousands of test failures and user issues that are difficult to understand and find the root cause of. Due to the volume, in practice many failures don’t even get triaged until they become a bigger problem.

**Build end-to-end pipelines that analyze failures on a schedule**

Result: One team had Devin trigger a pipeline every day after code-complete, which triggers a GitLab program that flashes tests onto the HIL bench. Devin gets the outputs in the morning and then triages and sends a report to the engineers before they wake up. **As a result, this team reclaimed 2K–4K engineering hours per month across ~4,000 tickets, equating to $1.7M–$3.5M in annual savings.**

**Use parallel agents to triage multiple tickets at once**

Result: At RV Tech, when a vehicle issue arrives in Slack, Devin is auto-triggered to pull logs, run diagnostics, and deliver a structured triage report for teams to review. In a separate deployment, **tens of parallel agents triaged 52 tickets in less than 15 minutes**. Because Devin runs in the cloud, engineers can work on something else while multiple agents triage tickets in parallel - one agent per ticket.

### How to Speed Up Test Generation and Requirement Mapping Workflows

For automotive teams, test generation capacity is bottlenecked by framework & requirement familiarity as well as bench availability and HIL cycle times. This results in significant backlogs of test development and coverage.

**Separate test planning (mapping requirements to gaps) and test implementation (generating code) for higher quality results**

Result:For a large US automotive company, we found that separating out a dedicated planning agent from an implementation agent improved quality and speed. Their program **test development took 1 day instead of half a week, with ~80% of generated tests passing.**

**Create reusable and modular Playbooks per framework and per subsystem**

Result: One engineer works out the initial [Playbook](https://docs.devin.ai/product-guides/creating-playbooks), and the rest of the team can immediately generate tests from that encoded knowledge. In multiple cases, Devin identified conflicting logic between two requirements which were flagged to a human reviewer to confirm. For RV Tech, implementing **these best practices led to a 10x increase in test-generation, from 1-2/day manually to 10-15 tests/day with AI-support.**

When test coverage and quality scale with playbooks instead of headcount, engineers can focus on higher-value work.

### Move Bottlenecked HIL Tests to SIL

HIL tests are slow to write and to run - often 1–8 hours per run, limited by bench availability and needing to take into account complex requirements. For one team we spoke to, only 150 SIL tests had been written against 700–1,000 HIL tests needed per program due to staffing capacity. Hardware tests are critical and will always be needed, but there’s growing sophistication around converting HIL tests into SIL equivalents in order to iterate faster and catch problems earlier.

**Use Devin to write SIL tests from bottlenecked HIL tests**

Result: Devin can help convert existing HIL tests to SIL equivalents. By running unit and functional tests in software first, teams catch errors and edge cases preventing these from becoming expensive HIL failures.

## What We've Seen: Organizational AI Maturity Journey

### Stage 1: AI-Supported

Example: Engineering uses Devin to review error logs for a HIL failure investigation. Devin iterates on the root cause hypotheses and pulls data to validate or invalidate each hypothesis. The root cause analysis is documented and delivered for engineering to review.

**What Changes:** investigations are done faster with Devin systematically reviewing evidence and documenting root cause analyses for engineers to review.

### Stage 2: Scheduled Automations

Failures that used to sit unreviewed for days are now triaged before the next sprint starts.

**Example:** One team schedules Devin to run every night auto-triggering HIL tests in which the outputs are fed back into Devin. Devin then correlates failure logs against diagnostic protocols and posts the root cause analysis to Slack before engineers arrive in the morning.

**Example:** Another team has Devin continuously polling for new crash reports, which are then deduplicated against known issues. Devin reviews the codebase, drafts a root cause analysis, and creates a Jira issue often with a fix PR attached.

**What Changes:** use time-driven and event-driven Devin agents to proactively respond to issues.

### Stage 3: Self-Improving Playbooks Shared Across the Organization

**Example:** Devin polls logs for issues. As issues are received, parallel Devin agents begin diagnosing each issue and creating fix PRs. As Devin sees issues, it improves its own playbook between runs to increase its speed and accuracy.

**What Changes:** teams set up agents to recursively improve playbooks for each part of the V model, across the company.

**What’s Still Out of Reach**

We are just at the beginning of how AI will transform HIL/SIL workflows. There are still many limitations. Devin excels at turning specific engineering requirements into functional test scripts. What remains hard is mapping broad regulatory language such as 'limitations on braking torque' to the right sections of code. Compliance standards are written for humans, not machines, and bridging that gap is still a challenge. We’ve also found that having the AI understand proprietary HIL software is quite challenging, which is why we use a dedicated Forward Deployed Engineering team to help set up the integrations for the specific customers' tech stack. Nevertheless, the industry continues to rapidly shift towards software-defined machines that get better over time.

*"This will help us be way more predictable in our execution, deliver software with way higher quality, and dramatically increase the velocity of our engineering team." — Wassym Bensaid, Co-CEO & CTO, RV Tech.*

If you’re exploring AI for HIL/SIL and want to learn how Devin has been used by other teams, please [reach out](https://cognition.ai/contact).
