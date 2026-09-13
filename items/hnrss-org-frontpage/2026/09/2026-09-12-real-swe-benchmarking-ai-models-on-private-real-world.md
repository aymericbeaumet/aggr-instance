---
title: 'Real-SWE: Benchmarking AI models on private, real-world, enterprise codebases'
link: https://withspecific.com/benchmarks/real-swe
source: hnrss-org-frontpage
published: 2026-09-12T20:25:48Z
updated: 2026-09-12T20:25:48Z
first_seen: 2026-09-13T01:23:10.731749865Z
authors:
- theanonymousone
summary: 'Article URL: https://withspecific.com/benchmarks/real-swe Comments URL: https://news.ycombinator.com/item?id=49676820 Points: 100 # Comments: 59'
content: extracted
html: 2026-09-12-real-swe-benchmarking-ai-models-on-private-real-world.html
---

September 2026

## Introducing Real-SWE

Benchmarking frontier AI models on private, real-world, enterprise codebases.

[Results](https://withspecific.com/benchmarks/real-swe#leaderboard)\
[Analysis](https://withspecific.com/benchmarks/real-swe#task-by-task)\
[Effort](https://withspecific.com/benchmarks/real-swe#effort)\
[Setup](https://withspecific.com/benchmarks/real-swe#evaluation-setup)

## 01Introduction

Today we are releasing Real-SWE, a benchmark that evaluates frontier AI models on private, real-world, enterprise codebases. Each task comes from a private production codebase that we licensed from a real-world company. These are problems their engineers work on, with all the context and complexity that comes with an existing product.

- **Private codebases.** Agents must navigate proprietary systems whose code and solutions aren’t available on the public internet.
- **Work with business consequences.** Getting billing right, calculating taxes, migrating customers. Changes that affect how a business runs, often across multiple services.
- **Company-specific complexity.** Every company has its own rules and ways of writing code. Agents have to understand those conventions and make changes that work with what’s already there.

**Can a coding agent actually do the work of a software engineer in the real world?**

1. 1![](https://withspecific.com/logos/anthropic.svg?dpl=dpl_ABGtodWDkmm7BqTikqe2cpVTakLw)

   Fable 5.1

   Claude Code

   Resolution rate: 38.8%

2. 2![](https://withspecific.com/logos/openai.png?dpl=dpl_ABGtodWDkmm7BqTikqe2cpVTakLw)

   GPT-6 Astra

   Codex CLI

   Resolution rate: 33.8%

3. 3![](https://withspecific.com/logos/google.png?dpl=dpl_ABGtodWDkmm7BqTikqe2cpVTakLw)

   Gemini 3.8 Flash

   Gemini CLI

   Resolution rate: 31.2%

4. 4![](https://withspecific.com/logos/zai.svg?dpl=dpl_ABGtodWDkmm7BqTikqe2cpVTakLw)

   GLM 5.3

   Claude Code

   Resolution rate: 28.8%

5. \=5![](https://withspecific.com/logos/xai.svg?dpl=dpl_ABGtodWDkmm7BqTikqe2cpVTakLw)

   Grok 4.6

   Grok Build

   Resolution rate: 23.8%

6. \=5![](https://withspecific.com/logos/meta.svg?dpl=dpl_ABGtodWDkmm7BqTikqe2cpVTakLw)

   Muse Spark 1.3

   Muse Code

   Resolution rate: 23.8%

7. 7![](https://withspecific.com/logos/kimi.svg?dpl=dpl_ABGtodWDkmm7BqTikqe2cpVTakLw)

   Kimi K3

   Kimi Code

   Resolution rate: 18.8%

8. 8![](https://withspecific.com/logos/openai.png?dpl=dpl_ABGtodWDkmm7BqTikqe2cpVTakLw)

   GPT-5.6 Sol

   Codex CLI

   Resolution rate: 16.2%

| #   | Model       | Harness | Resolution rate |
| --- | ----------- | ------- | --------------- |
| 1   | Claude Code | 38.8%   |                 |
| 2   | Codex CLI   | 33.8%   |                 |
| 3   | Gemini CLI  | 31.2%   |                 |
| 4   | Claude Code | 28.8%   |                 |
| \=5 | Grok Build  | 23.8%   |                 |
| \=5 | Muse Code   | 23.8%   |                 |
| 7   | Kimi Code   | 18.8%   |                 |
| 8   | Codex CLI   | 16.2%   |                 |

Resolution rate is equivalent to pass@1, averaged over eight independent runs per task. 95% confidence intervals are shown.

Expert-generated or synthetic tasks can be well designed, but they aren’t the verbatim, actual tasks that engineers in real companies need to do. Our tasks differ on two axes: the underlying coding artifact and specificity of the instruction. Both add complexities that challenge today’s frontier models.

We use native harnesses to reflect how enterprise engineers work in practice, evaluating model-and-harness combinations rather than models in isolation.

### Real company tasks require company-specific context

### Correct billing depends on business rules and external services

Fix invoice billing so each business charges the right tax and exempt customers aren't taxed.

View full instruction Hide full instruction

Billing reopens on Monday and every invoice this service issues is coming out untaxed. Each business on the platform settles its tax a different way: some maintain a rate themselves, some want each invoice priced against the buyer's destination by our tax authority provider, and some collect nothing at all, while a customer we hold an exemption for is charged nothing whichever way its business is configured. Pricing a destination means going to the authority with both addresses, the priced lines and the product category that business sells under, on the sandbox or the production authority according to the account the business is on; an address the authority refuses must be reported without stopping the invoice. The rate, the tax and the gross belong on the issued invoice, and once an invoice is settled the sale is filed back to the authority under that invoice's number so the returns reconcile. Invoices between European parties show both sides' VAT registrations. The authority and ledger are available at TAX\_JAR\_URL, PROD\_TAX\_JAR\_URL and INFLUX\_URL.

Services in the sandbox

- TaxJar sandbox
- TaxJar production
- InfluxDB ledger
- NestJS service
- TypeScript

### Agents work across code, infrastructure, and business tools

Tools and services across Real-SWE task environments. Each task exposes only the services its workflow needs.

- AWS emulator
- Docker
- Kubernetes
- GitHub
- Linear MCP
- PostgreSQL
- MySQL
- MongoDB
- Gel
- Redis
- Go
- Python
- Node.js
- Vitest
- Slack
- Intercom
- Google Drive
- Email
- ClickUp

### Codebase Selection

We selected codebases through a rigorous screening process, focusing on real companies with substantial usage, strong engineering teams, and demanding production workloads. The sample tasks analyzed below come from these codebases, including:

- A Luma/Partiful competitor with **200K+ users** and a **top 100 App Store ranking**
- A consumer fintech platform processing **100K+ bank statements**
- Enterprise AI sales platforms supporting complex business workflows

We prioritize code written to meet an actual user or business need over code written solely to create a benchmark task. Production engineering requires understanding existing architecture, preserving behavior that users rely on, and making changes within real operational constraints.

### Brief instructions can require changes across many files

Our tasks describe the change needed, leaving agents to discover implementation details in the codebase and surrounding tools. Any behavior required by the verifier must be stated or reasonably discoverable. This leads to our prompts being slightly underspecified, about par with DeepSWE and Terminal Bench, but specific enough to not omit instructions.

The work is cross-functional and complex: a single change can span multiple parts of the application. Agents must understand existing business logic and company coding patterns while keeping the surrounding system working.

Prompt length · median

A typical Real-SWE instruction is 1,742 characters.

- FrontierCode 2,056 chars

- DeepSWE 1,975 chars

- Terminal-Bench 3 1,584 chars

- FrontierSWE v2 992 chars

- Real-SWE 1,742 chars

Files edited by the reference solution · median

11 files in Real-SWE, compared with 6 in FrontierCode and DeepSWE.

- FrontierCode 6

- DeepSWE 6

- Real-SWE 11

All figures are medians. FrontierCode and DeepSWE use [Cognition's published comparison](https://cognition.com/blog/frontier-code); FrontierCode includes task descriptions and codebase guidelines. We measured instruction files from [Terminal-Bench 3's 74 tasks](https://github.com/harbor-framework/terminal-bench/tree/2b0442c3c583b710ca8da14c8e601b99f2f1f244/tasks), [FrontierSWE v2's 34 tasks](https://github.com/Proximal-Labs/frontier-swe-v2/tree/9e3f71cac38ef3d7e14a41b361c7b2b54c59899b/tasks), and Real-SWE's eight repository-backed sample tasks. Character counts are rounded to the nearest whole character. No comparable files-edited figure is included for Terminal-Bench 3 or FrontierSWE v2.

### Models fail even in short rollouts.

71.4% of rollouts under 10 minutes failed, compared with 73.4% of longer rollouts.

Triaging multiple systems and understanding requirements in codebases riddled with existing business logic and coding patterns is difficult.

Under 10 min

70/98 failed

10 min or longer

398/542 failed

- Failed
- Passed

Every task is inspired or lifted verbatim from a private, real-world codebase. We find these types of tasks super interesting for three reasons:

1. **Tasks on private codebases are natively out of distribution.** These types of coding tasks are not available anywhere on the internet and are unlikely to have ever been trained on by any other ai model. 99% of tokens in real-world enterprises are hidden away from the frontier models.
2. **These tasks are economically viable work.** Each task here has a direct relationship to spend and was assigned to an engineer earning a salary. Most benchmarks test interesting, experimental capabilities that are often unlikely to be widespread in the real-world.
3. **Company-specific engineering patterns matter.** Does AI code match the bar of a real-world enterprise? Our results show us that we're far from that reality. Many enterprises care about code standards and patterns. We've found that today's models are weaker at understanding company coding patterns and frequently miss requirements or don't verify their assumptions.

## 02Analysis

Here's an analysis of a small sample of tasks from our benchmark. If you're interested in the sample, [request access here](https://withspecific.com/benchmarks/real-swe/request-access).

### 6 of 10 tasks have resolution rates below 15%

| Task | ![](https://withspecific.com/logos/anthropic.svg?dpl=dpl_ABGtodWDkmm7BqTikqe2cpVTakLw)Fable 5.1 | ![](https://withspecific.com/logos/openai.png?dpl=dpl_ABGtodWDkmm7BqTikqe2cpVTakLw)GPT-6 Astra | ![](https://withspecific.com/logos/google.png?dpl=dpl_ABGtodWDkmm7BqTikqe2cpVTakLw)Gemini 3.8 Flash | ![](https://withspecific.com/logos/zai.svg?dpl=dpl_ABGtodWDkmm7BqTikqe2cpVTakLw)GLM 5.3 | ![](https://withspecific.com/logos/xai.svg?dpl=dpl_ABGtodWDkmm7BqTikqe2cpVTakLw)Grok 4.6 | ![](https://withspecific.com/logos/meta.svg?dpl=dpl_ABGtodWDkmm7BqTikqe2cpVTakLw)Muse Spark 1.3 | ![](https://withspecific.com/logos/kimi.svg?dpl=dpl_ABGtodWDkmm7BqTikqe2cpVTakLw)Kimi K3 | ![](https://withspecific.com/logos/openai.png?dpl=dpl_ABGtodWDkmm7BqTikqe2cpVTakLw)GPT-5.6 Sol | Resolution rate |
| ---- | ----------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------- | --------------- |
| 7/8  | 8/8                                                                                             | 8/8                                                                                            | 2/8                                                                                                 | 3/8                                                                                     | 8/8                                                                                      | 2/8                                                                                             | 5/8                                                                                      | 67.2%                                                                                          |                 |
| 8/8  | 5/8                                                                                             | 7/8                                                                                            | 5/8                                                                                                 | 4/8                                                                                     | 6/8                                                                                      | 0/8                                                                                             | 7/8                                                                                      | 65.6%                                                                                          |                 |
| 8/8  | 7/8                                                                                             | 5/8                                                                                            | 3/8                                                                                                 | 1/8                                                                                     | 1/8                                                                                      | 6/8                                                                                             | 1/8                                                                                      | 50.0%                                                                                          |                 |
| 3/8  | 1/8                                                                                             | 3/8                                                                                            | 4/8                                                                                                 | 8/8                                                                                     | 3/8                                                                                      | 4/8                                                                                             | 0/8                                                                                      | 40.6%                                                                                          |                 |
| 3/8  | 1/8                                                                                             | 2/8                                                                                            | 2/8                                                                                                 | 0/8                                                                                     | 0/8                                                                                      | 1/8                                                                                             | 0/8                                                                                      | 14.1%                                                                                          |                 |
| 1/8  | 5/8                                                                                             | 0/8                                                                                            | 1/8                                                                                                 | 0/8                                                                                     | 0/8                                                                                      | 1/8                                                                                             | 0/8                                                                                      | 12.5%                                                                                          |                 |
| 0/8  | 0/8                                                                                             | 0/8                                                                                            | 3/8                                                                                                 | 2/8                                                                                     | 1/8                                                                                      | 1/8                                                                                             | 0/8                                                                                      | 10.9%                                                                                          |                 |
| 0/8  | 0/8                                                                                             | 0/8                                                                                            | 2/8                                                                                                 | 1/8                                                                                     | 0/8                                                                                      | 0/8                                                                                             | 0/8                                                                                      | 4.7%                                                                                           |                 |
| 1/8  | 0/8                                                                                             | 0/8                                                                                            | 1/8                                                                                                 | 0/8                                                                                     | 0/8                                                                                      | 0/8                                                                                             | 0/8                                                                                      | 3.1%                                                                                           |                 |
| 0/8  | 0/8                                                                                             | 0/8                                                                                            | 0/8                                                                                                 | 0/8                                                                                     | 0/8                                                                                      | 0/8                                                                                             | 0/8                                                                                      | 0.0%                                                                                           |                 |

Each task had 8 rollouts per model.

### Missed requirements are the most common failure

Failures are grouped by observed submission behavior using the same taxonomy across models, following [DeepSWE](https://arxiv.org/html/2607.07946v1#A3).

![](https://withspecific.com/logos/anthropic.svg?dpl=dpl_ABGtodWDkmm7BqTikqe2cpVTakLw)Fable 5.1

![](https://withspecific.com/logos/openai.png?dpl=dpl_ABGtodWDkmm7BqTikqe2cpVTakLw)GPT-6 Astra

![](https://withspecific.com/logos/google.png?dpl=dpl_ABGtodWDkmm7BqTikqe2cpVTakLw)Gemini 3.8 Flash

![](https://withspecific.com/logos/zai.svg?dpl=dpl_ABGtodWDkmm7BqTikqe2cpVTakLw)GLM 5.3

![](https://withspecific.com/logos/xai.svg?dpl=dpl_ABGtodWDkmm7BqTikqe2cpVTakLw)Grok 4.6

![](https://withspecific.com/logos/meta.svg?dpl=dpl_ABGtodWDkmm7BqTikqe2cpVTakLw)Muse Spark 1.3

![](https://withspecific.com/logos/kimi.svg?dpl=dpl_ABGtodWDkmm7BqTikqe2cpVTakLw)Kimi K3

![](https://withspecific.com/logos/openai.png?dpl=dpl_ABGtodWDkmm7BqTikqe2cpVTakLw)GPT-5.6 Sol

Unverified assumption Missed requirement Integration error Regression Wrong file

#### No model solves every task

One square per rollout: each row is a task, each column a trial, eight trials per task for every model.

![](https://withspecific.com/logos/anthropic.svg?dpl=dpl_ABGtodWDkmm7BqTikqe2cpVTakLw)Fable 5.1

01

02

03

04

05

06

07

08

09

10

![](https://withspecific.com/logos/openai.png?dpl=dpl_ABGtodWDkmm7BqTikqe2cpVTakLw)GPT-6 Astra

01

02

03

04

05

06

07

08

09

10

![](https://withspecific.com/logos/google.png?dpl=dpl_ABGtodWDkmm7BqTikqe2cpVTakLw)Gemini 3.8 Flash

01

02

03

04

05

06

07

08

09

10

![](https://withspecific.com/logos/zai.svg?dpl=dpl_ABGtodWDkmm7BqTikqe2cpVTakLw)GLM 5.3

01

02

03

04

05

06

07

08

09

10

![](https://withspecific.com/logos/xai.svg?dpl=dpl_ABGtodWDkmm7BqTikqe2cpVTakLw)Grok 4.6

01

02

03

04

05

06

07

08

09

10

![](https://withspecific.com/logos/meta.svg?dpl=dpl_ABGtodWDkmm7BqTikqe2cpVTakLw)Muse Spark 1.3

01

02

03

04

05

06

07

08

09

10

![](https://withspecific.com/logos/kimi.svg?dpl=dpl_ABGtodWDkmm7BqTikqe2cpVTakLw)Kimi K3

01

02

03

04

05

06

07

08

09

10

![](https://withspecific.com/logos/openai.png?dpl=dpl_ABGtodWDkmm7BqTikqe2cpVTakLw)GPT-5.6 Sol

01

02

03

04

05

06

07

08

09

10

Pass Unverified assumption Missed requirement Integration error Regression Wrong file

#### Different models fail in different ways

Percentages are out of each model's failed runs, not all runs.

## 03Effort & the Frontier

Estimated frontier

### Estimated rollout costs range from $2.50 to $6.96

| Rank | Model | Estimated cost (USD) |
| ---- | ----- | -------------------- |
| 1    | $2.50 |                      |
| 2    | $2.65 |                      |
| 3    | $2.74 |                      |
| 4    | $3.44 |                      |
| 5    | $3.90 |                      |
| 6    | $4.67 |                      |
| 7    | $5.12 |                      |
| 8    | $6.96 |                      |

mean per rollout, by task

Swipe the chart to see all tasks.

Fable 5.1 · 64k overall GPT-6 Astra · 24k overall Gemini 3.8 Flash · 94k overall GLM 5.3 · 117k overall Grok 4.6 · 67k overall Muse Spark 1.3 · 87k overall Kimi K3 · 43k overall GPT-5.6 Sol · 23k overall

View task values

Task

- Fable 5.1 34k
- GPT-6 Astra 13k
- Gemini 3.8 Flash 78k
- GLM 5.3 68k
- Grok 4.6 7k
- Muse Spark 1.3 36k
- Kimi K3 30k
- GPT-5.6 Sol 12k

## 04Evaluation Setup

Each agent was run in an isolated sandbox. All tasks are in Harbor format, and verifiers are injected at grading time. The verifiers are inspired by existing test suites in the codebase or use those tests verbatim.
