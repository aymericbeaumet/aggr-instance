---
title: 'Dayjob: Tax – evaluating AI agents on 50 professional tax assignments'
link: https://surgehq.ai/blog/dayjob-tax
source: hnrss-org
published: 2026-10-28T15:46:30Z
updated: 2026-10-28T15:46:30Z
first_seen: 2026-10-28T21:53:36.421640500Z
authors:
- landonxi
content: extracted
html: 2026-10-28-dayjob-tax-evaluating-ai-agents-on-50-professional-tax.html
remote_preview:
  url: https://cdn.prod.website-files.com/68dcd2ceb173c46fa029931c/6ae0beffc0ef7d2c805991ec_6ae0bef7bc59db92222e060b_dayjob-tax-cover-v3.png
  alt: 'DAYJOB: Tax — Benchmarking AI Agents on Professional Tax Work'
---

Anna was planning to sell her condo. She had moved out in August 2024 and wanted to know whether closing in October 2026 or January 2027 would be better for taxes.

An AI agent reviewed both options and correctly concluded that either date would satisfy the residence requirement for a tax break worth up to $250,000 in excluded gains.

But it left out an important deadline.

Under the usual rules, homeowners must have lived in a property for at least two of the five years before selling it to qualify for the exclusion. Because Anna moved out in August 2024, she would stop satisfying that requirement around the end of August 2027. If her sale was delayed beyond August 2027, she could lose eligibility.

Across five attempts, Astra never identified the deadline. Opus identified it in all five attempts.

The agent evaluated the proposed dates correctly, but its advice left Anna unaware of a deadline that could affect a major financial decision. A tax adviser should recognize consequential issues even when the client doesn't know to ask about them.

This is the kind of professional judgment we're testing with **DAYJOB: Tax**, the latest addition to [Surge AI's DAYJOB benchmark family](https://surgehq.ai/blog/dayjob).

## Introducing DAYJOB: Tax

DAYJOB evaluates AI agents on professional tax assignments requiring specialized expertise, complex materials, and decisions that aren't fully specified in the initial instructions. Our earlier releases covered Healthcare and Finance. DAYJOB: Tax extends that work into tax preparation, compliance, and advisory services.

The benchmark contains 50 environments across eight areas of tax practice. Agents receive client requests and supporting records, then produce tax returns, workpapers, technical reviews, or client advice.

These assignments test professional judgment alongside tax knowledge, calculations, retrieval, and instruction following. Agents must decide what deserves investigation, resolve conflicting evidence, recognize when information is insufficient, and carry those decisions into their final work.

## Results: Leaderboard and Pareto Frontiers

**Claude Sonnet 5.5 leads with a 10.8% pass rate** on the [DAYJOB: Tax leaderboard](https://surgehq.ai/benchmarks/dayjob-tax), followed by Claude Fable 5.1 at 9.6% and Claude Opus 5.5 at 9.2%.

![DAYJOB: Tax leaderboard](https://cdn.prod.website-files.com/68dcd2ceb173c46fa029931c/6ae0be39ff7dfbf1d1c0be10_6ae0be167d69fdd5f5b61e46_dayjob-tax-leaderboard.png)

We report pass@1 at a 95% rubric threshold. An attempt passes when it satisfies at least 95% of the task's criteria, which assess calculations, supporting evidence, consequential issues, and the completed deliverable.

### Performance versus cost

We also compared model performance against average cost per trial.

![](https://cdn.prod.website-files.com/68dcd2ceb173c46fa029931c/6ae0c6ccbb53b2b878fdafa0_pareto.png)

Claude Sonnet 5.5 and Opus 5.5 occupy the high-performance end of the measured Pareto frontier.

## The breadth of professional tax work

DAYJOB: Tax covers eight areas, each requiring different rules, records, and expertise:

- **Individual and family tax**
- **Small-business tax**
- **Partnerships**
- **Property and investments**
- **Estates, gifts, and trusts**
- **International tax**
- **Corporate transactions**
- **Tax review and disputes**

Some assignments require complete filing packages with supporting workpapers. Others ask agents to investigate transactions, review previous filings, or prepare advice about decisions clients haven't yet made.

## What's inside the assignments?

The benchmark contains **1,789 workspace files**, with a median of 30 per assignment and as many as 123. These include 1,487 PDFs, 117 Excel workbooks, 38 Word documents, and additional emails, images, CSVs, JSON records, and text files.

The workspaces include filed and draft tax returns, K-1s, W-2s, IRS notices, general ledgers, payroll records, contracts, and correspondence. They also contain the less standardized evidence that tax professionals routinely work with.

One assignment includes a photograph of a **handwritten receipt for merchandise purchased for resale**. The agent must read the item descriptions and amounts, reconcile them with spending records, and determine how the purchases affect inventory and cost of goods sold.

![Photograph of a handwritten receipt for merchandise purchased for resale, from a DAYJOB: Tax workspace](https://cdn.prod.website-files.com/68dcd2ceb173c46fa029931c/6ae0dfece016abfe1a6d9a88_6ae0dfdf124711b53397acf1_dayjob-tax-handwritten-receipt-wide.png)

Another assignment involves an industrial silo-services company whose client assumes it operates only in Alabama. But its **field dispatch log shows crews working across five states**. The job locations, crew sizes, and days on site provide evidence relevant to the company's multistate tax obligations.

![Field dispatch log from a DAYJOB: Tax workspace](https://cdn.prod.website-files.com/68dcd2ceb173c46fa029931c/6ae0be38ff7dfbf1d1c0be0d_6ae0be2fc839b1bb6c17f682_dayjob-tax-field-dispatch-log.jpeg)

Other environments are similarly varied. One involves a cross-border household whose Polish employment and property records must be reconciled with cryptocurrency exchange exports, wallet histories, and historical purchase lots.

## Created and reviewed by tax experts

DAYJOB tasks begin with real-world professional workflows. Tax experts turn these situations into realistic assignments, developing the supporting records, expected outcomes, and grading criteria. Each task undergoes a three-layer expert review for correctness, completeness, and solvability.

Tax experts bring experience across corporate taxation, partnerships, individual returns, trusts, and multistate compliance. Their backgrounds include:

- **Corporate and private-equity taxation:** A tax attorney with over 10 years of experience at PwC, Deloitte, and Exxon, covering tax planning, compliance, audits, and controversy.
- **Partnership and corporate taxation:** A Baker Tilly Tax Director with experience at PwC, Moss Adams, and Windes, specializing in complex partnership allocations, consolidated corporate returns, REITs, and high-net-worth clients.
- **Real estate and individual taxation:** A tax professional with experience at Deloitte and EY, specializing in real-estate partnerships, individual taxation, and trusts.

## A retirement plan built on an unverified assumption

A separating couple asked an AI agent how to divide their Roth IRA and 401(k). Opus 5.5 told them they weren't legally married and recommended keeping their accounts separate, with a cash payment to even out the balances.

It opened one response:

> You aren't legally married, so neither account can be split without triggering taxes and penalties.

But the couple's own records raised doubts about that conclusion. Although they now lived in Florida, where new common-law marriages generally aren't recognized, they had previously lived together in Texas and held a ceremony in Colorado. Texas recognizes informal marriages under certain conditions, and a valid marriage established elsewhere may remain valid in Florida.

That distinction could change the tax consequences of dividing their retirement accounts.

**Across all five attempts, Opus based its recommendations around the assumption that the couple was unmarried**, even when it acknowledged uncertainty. Astra recognized the need to verify their marital status in four of five attempts. One response advised:

> We first need to confirm your legal marital status.

The client asked how to divide the accounts. The agent needed to establish whether they were married before recommending a course of action. The evidence that should have prompted that investigation was already in the files.

## Where DAYJOB: Tax fits

Most existing tax benchmarks begin with a defined problem: a legal question to answer, a return to calculate, or missing taxpayer information to collect. These evaluations measure important capabilities, but much of a tax professional's work involves figuring out what the problem actually is. Clients provide incomplete instructions, records contain conflicting evidence, and consequential issues may never appear in the original request.

**DAYJOB tests whether agents can handle that end-to-end responsibility**, including identifying issues the client overlooked, evaluating conflicting evidence, and deciding what needs to be done in the first place.

### TaxBench

*Can the model answer a difficult tax question?*

[TaxBench](https://huggingface.co/datasets/intelligenceconsortium/TaxBench) evaluates 114 tax questions across multiple specialties, each with a defined problem and grading rubric.

**DAYJOB also asks whether the client asked the right question.** Anna wants to compare two dates for selling her condo. Both satisfy the residence requirement for a tax exclusion, but does the agent warn her when that eligibility could expire?

### TaxCalcBench

*Can the model calculate an accurate return?*

[TaxCalcBench](https://github.com/column-tax/tax-calc-bench) evaluates tax-return accuracy against expected filing values, using realistic tax documents and taxpayer scenarios.

**DAYJOB also asks whether the underlying records actually support the tax treatment.** A studio's ledger references an attached receipt, but the receipt is missing. Does the agent flag the missing evidence or incorrectly treat the reimbursement as verified?

### TaxLab

*Can the agent gather missing information and file correctly?*

[TaxLab](https://daplab.cs.columbia.edu/benchmarks/taxlab/) tests whether agents can question a simulated taxpayer, obtain missing information, and complete a federal individual tax return.

**DAYJOB extends that responsibility across the wider tax profession.** The deliverable might be a partnership loss analysis, an estate return, a review of a disputed tax position, or a strategy letter.

**The central distinction is how much responsibility remains with the agent.** DAYJOB evaluates whether agents can discover consequential issues, exercise professional judgment, and carry their findings through to a completed filing, review, or recommendation.

![](https://cdn.prod.website-files.com/68dcd2ceb173c46fa029931c/6ae0c6e13cf0140c3efbcbcf_summary.png)

## What DAYJOB: Tax measures

DAYJOB tests whether AI agents can take ownership of a professional assignment from start to finish. That means navigating messy records, identifying issues the client never raised, making sound judgments with incomplete information, and delivering work that meets professional standards.

The results show how difficult that remains for frontier models. Even when agents can perform the individual steps, exercising professional judgment and reliably carrying out real-world tax workflows remain out of reach.

Agents haven’t yet passed the audit.

The full leaderboard, with every model we evaluated, is on the [DAYJOB: Tax benchmark page](https://surgehq.ai/benchmarks/dayjob-tax).

To learn more or request access to the full benchmark, contact [**benchmarks@surgehq.ai**](mailto:benchmarks@surgehq.ai).
