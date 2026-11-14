---
title: Formalizing US Tax Code (Using Lean)
link: https://lexifina.com/blog/formalizing-the-us-tax-code
source: hnrss-org
published: 2026-11-14T15:41:20Z
updated: 2026-11-14T15:41:20Z
first_seen: 2026-11-14T20:36:53.628480974Z
authors:
- dnw
content: extracted
html: 2026-11-14-formalizing-us-tax-code-using-lean.html
remote_preview:
  url: https://lexifina.com/social-preview-v3.png?v=20261018
  alt: Formalizing the US tax code
---

Yesterday, we verified that known tax loopholes could be identified using a Lean search. Today, we set out to formalize the entire Internal Revenue Code to hunt for unknown loopholes (bugs).

## Pointing a proof checker at statutes

Lean is a programming language whose type checker can also check mathematical proofs: if Lean accepts a proof, every step follows from the definitions, with nothing taken on trust. An earlier post discussed how far that kind of formalisation can reach in law,[^1] and yesterday we used it on the retirement-account rules, where a blind search rediscovered the backdoor Roth.[^2]

This work is part of [global-law](https://github.com/lexifina/global-law), a CC0 repository of versioned legislation and case law.[^3] Our argument is that pointing AI and a proof checker at a versioned record of the law finds the contradictions and mechanical mistakes that people miss. In particular, we can catch mistakes in the text, and see what the rules allow, including what they allow by accident. To do so, we translated each section of the law into Lean, staying close to the statute’s own words, and tested it against the worked examples the IRS prints in its forms and instructions.

## 1\. The tax law’s text has 138 drafting mistakes

The tax law is Title 26 of the US Code. Its official text is published as structured data by the Office of the Law Revision Counsel, the office of the House of Representatives that compiles the Code.[^4] We used the release current to 3 November 2026.

A parser then reads every cross-reference, including the ones written in words (“paragraph (2) of subsection (b)”, “such subparagraph”, “sections 922 through 927”), which carry no hyperlink. Lean checks each one. Does the provision it points to exist? Is it still in force? (verifiable via our versioned history) The checks run as a test suite, which fails whenever a new release adds a fault the last one did not have.

The suite reports 257 findings, each checked by an AI agent against the official text and its editorial notes: 98 intended (rules for past periods, and provisions enacted but not yet in effect), 21 false alarms in the editors’ own hyperlinks, and 138 real faults:

| Fault                                                                                | Count | Example                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| ------------------------------------------------------------------------------------ | ----- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Reference to something since repealed, moved or rewritten, with no conforming change | 67    | [§1(f)(7)(A) cites a rewritten provision](https://uscode.house.gov/us/usc/t26/s1/#f-7-A "Cites §68(b)(2), which the 2025 Act rewrote without updating this reference.")\ [§168(n)(2)(G) cites a moved provision](https://uscode.house.gov/us/usc/t26/s168/#n-2-G "Added by the 2025 Act itself, and already cites a provision that has since moved.")                                                                                                                              |
| Typo, wrong subdivision or unnamed Act                                               | 24    | [§1400Z-2(f)(1) cites the wrong subsection](https://uscode.house.gov/us/usc/t26/s1400Z-2/#f-1 "Cites subsection (c)(1). The provision meant is (d)(1).")\ [§72(t)(2)(A)(viii) has a typo in a citation](https://uscode.house.gov/us/usc/t26/s72/#t-2-A-viii "Cites 5 U.S.C. 8366a. The section meant is 8336a.")                                                                                                                                                                   |
| Wrong level word                                                                     | 8     | [§9818(a)(1)(A) calls a subsection a paragraph](https://uscode.house.gov/us/usc/t26/s9818/#a-1-A "Says “paragraph (b)”. In this section (b) is a subsection.")                                                                                                                                                                                                                                                                                                                      |
| Two provisions with the same number                                                  | 9     | [§1202(b)(4) appears twice](https://uscode.house.gov/us/usc/t26/s1202/#b-4 "Two different paragraphs are both numbered (b)(4).")\ [§45X(d)(4) appears twice](https://uscode.house.gov/us/usc/t26/s45X/#d-4 "Two different paragraphs are both numbered (d)(4).")                                                                                                                                                                                                                   |
| Parentheses that never close (13 footnoted by the editors, 17 not)                   | 30    | [§165(h)(1) leaves a parenthesis open](https://uscode.house.gov/us/usc/t26/s165/#h-1 "“($500 in the case of any qualified disaster-related personal casualty losses” is opened and never closed. The editors footnote it: “So in original.”")\ [§38(c)(6)(C) leaves a parenthesis open](https://uscode.house.gov/us/usc/t26/s38/#c-6-C "“(as determined under section 46(e)(2) (as so in effect) of such amount” never closes its first parenthesis. The editors do not note it.") |

Each of the 138 has a certificate: a short Lean statement that carries its evidence from the official XML and proves the fault. For a dead reference, the evidence is the full list of what does exist under the cited provision. Lean re-checks all 138 in about two and a half minutes.

Our record of the US Code is a git history, one commit per public law,[^5] so each fault can be traced to the law that wrote it. For Title 26 the record reaches back to the 1994 edition.

This check is cheap to repeat. Run against each new release of the Code, it flags a new mistake as soon as it appears, and names the law that caused it.

## 2\. Rewriting the income tax as code that can be checked

Next we wrote down what the law calculates: the 2025 federal income tax for one household, meaning the numbers on Form 1040, including the changes made by the One Big Beautiful Bill Act in July 2025.[^6],[^7] We did it in four parts:

- **Income**, down to adjusted gross income (AGI). AGI is total income minus a few items, such as IRA contributions and student-loan interest. Most other tax rules depend on it.
- **Deductions**: the standard deduction or itemized deductions, and the new deductions for tips, overtime, car-loan interest and people over 65.
- **The tax itself**, including the alternative minimum tax (AMT), a second calculation that stops high earners from cutting their tax too far with deductions.
- **Credits**, such as the child tax credit and the earned income tax credit (EITC), a credit for people on low and moderate wages.

We used AI to translate the statute into Lean. This is the lossy step, therefore we reinforce it with the following measures:

- Each part is tested against figures the IRS publishes (the Tax Table, the EITC table, the worksheets and the examples in its publications), and checked against three independent tax programs on 5,000 households (section 3).
- A second agent, which did not write that part, reviews it adversarially, looking for errors and for cheating, such as code written to pass the tests rather than to follow the statute.

What Lean ultimately adds is proofs that cover every input instead of a sample. For example, it proves that the SALT cap and the car-loan and overtime deductions never grow when income rises, at every income and for every filing status.

The comparison with other tax software helps us catch logical errors. That is how we caught two mistakes in our own model. It is also how we found bugs in the other tools that were logical rather than arithmetic: Tax-Calculator gives the EITC to separate filers without checking whether the spouses lived apart, and UsTaxes never attaches Schedule C to the 2025 return, so self-employment income disappears.

![Dependency map of the Lean tax code: 2,159 definitions and theorems by depth, in three columns: drafting checks and certificates, the 2025 income tax, and the loophole searches](https://lexifina.com/blog-media/formalizing-the-us-tax-code/anatomy.svg)

Writing the model turned up five things:

- **The published Code mixes 2025 and 2026 law.** In six places the consolidated Code already shows the 2025 Act’s amendments that start in 2026. Read without the effective-date notes, it gives the wrong 2025 tax: the AMT phase-out at 50% instead of 25%, a charity floor and a cap on itemized deductions that do not yet apply.
- **The same Act rounds in opposite directions.** The new tips and overtime deductions shrink by $100 for each full $1,000 of income over the limit, so one more dollar of income can cost $100 of deduction. The new car-loan deduction shrinks by $200 for each $1,000 “or portion thereof”, which rounds the other way, so it runs out $999 sooner than the round numbers suggest.
- **A married couple can deduct more state and local tax by filing separately.** The deduction for state and local taxes (SALT) is capped, and the cap shrinks as income rises: by 30 cents for each dollar over $500,000 on a joint return. On a separate return, the law halves the cap after shrinking it, so it shrinks by only 15 cents a dollar. A couple earning $300,000 each can deduct up to $25,000 on two separate returns, but only $10,000 on one joint return.
- **The Tax Table is not the tax formula.** Most people with taxable income under $100,000 look up their tax in the IRS Tax Table instead of working it out. The table differs from the formula in the law by between −$5 and +$6, and Lean proves that range for every income under $100,000 and every filing status.
- **One dollar can cost $8,046.** Some benefits switch off at a line instead of fading out. A family with three children can lose its whole EITC, up to $8,046, because its investment income is one dollar over $11,950. The saver’s credit, a credit for retirement saving, works the same way: a couple who each put $2,000 into retirement accounts gets $2,000 at an income of $47,500, and $800 at $47,501.

## 3\. Testing it against tax software people already use

A model is only useful if it gives the right answers. To check ours, we built 5,000 sample households and worked out each one’s 2025 tax four ways: with our model, and with three tools built by other teams, independently of each other and of us:

- **PolicyEngine-US**, an open-source model that researchers and policymakers use to estimate how tax and benefit changes affect households;[^8]
- **Tax-Calculator**, the Policy Simulation Library’s open-source model of the federal income tax;[^9]
- **UsTaxes**, a free, open-source app for preparing your own tax return.[^10]

Two thousand households were ordinary. The other 3,000 were deliberately awkward: couples filing separately, widows and widowers, students, people with tips or overtime, IRAs and health savings accounts, education and saver’s credits, and incomes over $1 million with large investment gains.

**Our model and PolicyEngine-US agreed on every figure for 4,993 of the 5,000 households.** The other seven differ by at most $11, all in the EITC: our model reads the IRS’s printed EITC table, while PolicyEngine-US uses the formula behind it.[^11]

**Tax-Calculator** gave the EITC and the child and dependent care credit to married people filing separately without checking that the spouses lived apart, which the law requires. It also computed no AMT for one high-earning separate filer, where two other calculators computed $14,881.

**UsTaxes** had 14 bugs, each confirmed by recalculating an affected household. It drops self-employment income without a warning, because its 2025 return never attaches Schedule C, the form for business income. Its child-care credit never reaches the return. Its age test for the EITC always passes. And its self-employment tax counts a spouse’s wages against the cap on Social Security tax. Together, these bugs affect about one ordinary household in six.

The comparison caught two mistakes in our own model too. We had not counted self-employment profit toward the 20% deduction for small-business income. And we always took the larger of the standard and itemized deductions, although the law lets you choose to itemize a smaller amount, which can pay when it lowers the AMT. With that fixed, one household owed $2,070 less. Both mistakes showed up because two independent tools agreed with each other and not with us.

## 4\. A search found the loopholes tax advisers already use

With a model that matches the reference software, we used it as we had used the retirement rules: as the oracle for a search that knows no tax planning. The first search tried ordinary choices for each of the 5,000 households, 195,787 returns in all:

- file separately, and which spouse holds which investments, pays which bills and claims which children;
- contribute to an IRA;
- earn a little less;
- realize more or less gain.

The second search used a multi-year family model, which adds the kiddie tax, trust taxation, Roth conversions, required withdrawals, gifts and death. It tried about 30,000 plans for four families over 34 to 47 years. Everything large that the two searches found is already in the planning literature:

| What the search found                                              | Best case                  | Already described by       |
| ------------------------------------------------------------------ | -------------------------- | -------------------------- |
| A business owner files separately to get under the §199A threshold | $6,317 a year              | Kitces[^12]                 |
| The lower earner claims the children on a separate return          | $3,090 a year              | practitioner guides[^13]    |
| Realize less gain to stay under the EITC investment limit          | $6,798 for $5,000 deferred | IRS Publication 596[^11]    |
| Itemize a smaller amount to cut the AMT                            | $2,070 a year              | Form 1040 instructions     |
| Roth conversions in the gap years before Social Security           | +$0.84M over a lifetime    | Kitces                     |
| Pay your children wages, and open Roth IRAs for them               | +$0.46M                    | Kitces[^14]                 |
| Convert so the heirs do not stack ten-year withdrawals             | +$0.43M–0.57M              | Journal of Accountancy[^15] |

As with the backdoor Roth, none of these was described to the search. It found them because the rules, written down plainly, permit them.

## 5\. Novel loopholes we found

Three results did not turn up in practitioner sources, IRS guidance or tax-software forums.

**Filing separately can cut tax on Social Security, even for a couple living together.** The standard advice is that a couple living together should never file separately when one draws Social Security.[^16] A separate filer who lived with their spouse gets no base amount at all. But taxable benefits are capped at the lesser of 85% of benefits or 85% of provisional income,[^17],[^18] and the search found couples for whom that cap makes separate returns cheaper. On a joint return the working spouse’s wage counts in provisional income, so a moderate wage already pushes the benefits to the 85% maximum. On a separate return the retiree’s benefits are measured against the retiree’s income alone. With $48,000 of benefits and a $60,000 wage, the couple owes $7,050 jointly and $5,568 separately, $1,482 less. A second reviewer recomputed it by hand and got the same answer. At a $100,000 wage the result flips, so the window is real but narrow.

**Wages on one return, investments on the other.** The 3.8% tax on investment income applies to the smaller of the investment income and the excess of income over $250,000 jointly, or $125,000 on a separate return.[^19] On a joint return, wages use up that threshold first. On separate returns the investing spouse’s income excludes the other’s wages, and their gains start in their own 0% band. With about $200,000 of each, that saves $1,621. Every explainer gives the threshold. We found none that describes the split or where it pays.

**Schedule 8812 pays more than the statute allows.** This is a gap between the IRS form and the law, not a plan. The statute caps the refundable child credit per qualifying child, computed without the $500 credit for other dependents. The form caps the combined credit instead, so in the phase-out range, with no tax left to absorb it, it refunds the $500 credits in cash.[^20],[^21] Example: one child and one other dependent, $240,000 of income, no tax left. The statute gives $200, the form $700. Lean proves the form is never below the statute and agrees with it exactly when there are no other dependents.

## 6\. Why one year of the tax code holds no big loopholes

The backdoor Roth is big because two provisions combine to remove an income limit entirely, and the money then compounds tax-free for decades. A single year of the individual income tax is full of cliffs and per-return thresholds, but each is bounded: the largest swing we found was a few thousand dollars. Over decades, the large gains come from moves that are already published. The one combination we could not find written up was paying the children in the same years as Roth conversions, which adds about 0.5%. Our guess is that anything Roth-sized and unpublished lives where value moves between entities and their owners: partnerships, S corporations and C corporations.

## 7\. Most of this can now run automatically

> “The long-term problem is how this kind of bug-hunting can be done routinely, e.g. as part of a test suite. Some approaches aren’t really automatable, because they require a lot of both human and AI agent work.”

What still needs a person is deciding what a finding means. Some cliffs are deliberate. Others are in the IRS’s forms, not in the law, whilst others come from the way the official text is compiled, yet:

- **The text checks are routine.** A new release is one download and one command, which fails on any fault the last release did not have, and git blame names the Act responsible.
- **The model is a one-off cost.** Once written and proved, it is a build: if a later change breaks a proved property, the build fails. When the law changes, only the changed provisions need rewriting.
- **The comparison is routine** once the input mappings are right. Getting them right was the hard part, and the same comparison found every mapping error.

## Limits

- **Coverage.** The model covers the 2025 individual income tax for a household record. It leaves out the kiddie tax on the single-year return, 28% and §1250 gain, foreign income, passive activity rules, and most AMT preferences. The multi-year model holds 2025 law constant.
- **Who checked what.** The drafting triage and the reviews were done by AI agents against the official text, and have not been checked by a lawyer or CPA.
- **Agreement is not truth.** Where all four calculators agree they may share a misreading. The theorems and the IRS-example tests are the check on the model itself.

The code, the certificates, the comparison and the searches are in the lean-proofs folder of global-law.[^3]

## References

[^1]: Alan Yahya, ‘The auto-formalization of mathematics versus law’ (*Lexifina*, 7 September 2026) [\<lexifina.com/blog/auto-formalisation-of-law>](https://lexifina.com/blog/auto-formalisation-of-law)

[^2]: Alan Yahya, ‘LEAN proof to search for US tax loopholes’ (*Lexifina*, 9 November 2026) [\<lexifina.com/blog/searching-the-tax-code-for-loopholes>](https://lexifina.com/blog/searching-the-tax-code-for-loopholes)

[^3]: Lexifina, *global-law*, lean-proofs [\<github.com/lexifina/global-law/tree/main/lean-proofs>](https://github.com/lexifina/global-law/tree/main/lean-proofs)

[^4]: Office of the Law Revision Counsel, ‘United States Code: download’ (release point 119-138) [\<uscode.house.gov/download/download.shtml>](https://uscode.house.gov/download/download.shtml)

[^5]: Alan Yahya, ‘Git for the US Code’ (*Lexifina*) [\<lexifina.com/blog/git-for-the-us-code>](https://lexifina.com/blog/git-for-the-us-code)

[^6]: Internal Revenue Service, Rev Proc 2024-40 [\<www.irs.gov/pub/irs-drop/rp-24-40.pdf>](https://www.irs.gov/pub/irs-drop/rp-24-40.pdf)

[^7]: An Act to provide for reconciliation pursuant to title II of H. Con. Res. 14, Pub L 119-21 (4 July 2025) [\<www.congress.gov/bill/119th-congress/house-bill/1>](https://www.congress.gov/bill/119th-congress/house-bill/1)

[^8]: PolicyEngine, *policyengine-us* (version 2.116.9) [\<github.com/PolicyEngine/policyengine-us>](https://github.com/PolicyEngine/policyengine-us)

[^9]: Policy Simulation Library, *Tax-Calculator* (version 6.8.5) [\<github.com/PSLmodels/Tax-Calculator>](https://github.com/PSLmodels/Tax-Calculator)

[^10]: *UsTaxes* (commit 49cd0f2, 7 November 2026) [\<github.com/ustaxes/UsTaxes>](https://github.com/ustaxes/UsTaxes)

[^11]: Internal Revenue Service, Publication 596, ‘Earned Income Credit’ (2025) [\<www.irs.gov/publications/p596>](https://www.irs.gov/publications/p596)

[^12]: ‘Section 199A QBI deduction strategies for small business owners’ (*Kitces.com*) [\<www.kitces.com/blog/small-business-owner-section-199a-qualified-business-income-qbi-deduction-strategies/>](https://www.kitces.com/blog/small-business-owner-section-199a-qualified-business-income-qbi-deduction-strategies/)

[^13]: ‘Child tax credit for married filing separately’ (*Taxstra*) [\<taxstra.com/child-tax-credit-married-filing-separately/>](https://taxstra.com/child-tax-credit-married-filing-separately/)

[^14]: ‘Tax rules for hiring children in a family business’ (*Kitces.com*) [\<www.kitces.com/blog/tax-rules-hiring-children-family-business-flsa-tax-savings-standard-deduction-roth-contribution/>](https://www.kitces.com/blog/tax-rules-hiring-children-family-business-flsa-tax-savings-standard-deduction-roth-contribution/)

[^15]: ‘Avoiding a big tax bill on inherited IRAs’ (*Journal of Accountancy*, May 2026) [\<www.journalofaccountancy.com/issues/2026/may/avoiding-a-big-tax-bill-on-inherited-iras/>](https://www.journalofaccountancy.com/issues/2026/may/avoiding-a-big-tax-bill-on-inherited-iras/)

[^16]: ‘Should my wife and I file separate tax returns to avoid paying income taxes on her benefits?’ (*Maximize My Social Security*) [\<www.maximizemysocialsecurity.com/ask-larry/should-my-wife-and-i-file-separate-tax-returns-avoid-paying-income-taxes-her-benefits>](https://www.maximizemysocialsecurity.com/ask-larry/should-my-wife-and-i-file-separate-tax-returns-avoid-paying-income-taxes-her-benefits)

[^17]: Congressional Research Service, ‘Social Security: Calculation and History of Taxing Benefits’ (RL32552) [\<www.everycrsreport.com/files/20150120\_RL32552\_fa08c40941ceb4e9565c7c7cbd9a4e8e8ba46cdc.html>](https://www.everycrsreport.com/files/20150120_RL32552_fa08c40941ceb4e9565c7c7cbd9a4e8e8ba46cdc.html)

[^18]: Congressional Research Service, ‘Taxation of Social Security Benefits and the Senior Deduction in P.L. 119-21: In Brief’ (R48613) [\<www.everycrsreport.com/reports/R48613.html>](https://www.everycrsreport.com/reports/R48613.html)

[^19]: 26 USC § 1411 [\<www.law.cornell.edu/uscode/text/26/1411>](https://www.law.cornell.edu/uscode/text/26/1411)

[^20]: 26 USC § 24(d), (h)(4)–(5) [\<www.law.cornell.edu/uscode/text/26/24>](https://www.law.cornell.edu/uscode/text/26/24)

[^21]: Internal Revenue Service, Schedule 8812 (Form 1040) (2025) [\<www.irs.gov/forms-pubs/about-schedule-8812-form-1040>](https://www.irs.gov/forms-pubs/about-schedule-8812-form-1040)
