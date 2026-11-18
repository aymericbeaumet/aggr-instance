---
title: Harvard researchers find AI coding agents shifted the bottleneck to code review
link: https://complexdiscovery.com/harvard-researchers-find-ai-coding-agents-shifted-the-bottleneck-to-code-review/
source: hnrss-org
published: 2026-11-17T21:16:12Z
updated: 2026-11-17T21:16:12Z
first_seen: 2026-11-18T09:02:33.277923620Z
authors:
- judahmeek
content: extracted
html: 2026-11-17-harvard-researchers-find-ai-coding-agents-shifted-the.html
remote_preview:
  url: https://complexdiscovery.com/wp-content/uploads/2026/10/AI-Report-One.jpg
---

Editor’s Note: Engineers at firms that adopted AI coding agents added about 30 percent more code, yet finished work did not measurably rise and getting code through review took 49 percent longer, according to a working paper by Harvard Ph.D. candidates Fiona Chen and James Stratton covering 718 firms. The paper, a job-market paper with no indication of completed peer review, measures what happens at the human checkpoint once drafting speeds up. Harvey’s motion-to-dismiss workflow and Cooley’s redaction agent, still in development with Google Cloud, both keep a lawyer at a comparable checkpoint, and neither announcement reports a measure of that review stage.

For eDiscovery, privacy and compliance professionals, the study suggests a measurement approach for legal AI buyers: consider approved work product, revision rates and reviewer time alongside drafting output. It also shows that AI authorship can leave metadata in collaboration systems that records and legal hold programs may need to reach. Watch for a revised version of the paper and for any legal vendor that publishes review-stage figures, keeping in mind that the study examined software, not legal work.

* * *

##### Information - 92%

##### Insight - 90%

##### Relevance - 88%

##### Objectivity - 90%

##### Authority - 92%

### 90%

#### Excellent

A short percentage-based assessment of the qualitative benefit expressed as a percentage of positive reception of the recent article from ComplexDiscovery OÜ titled, "Harvard researchers find AI coding agents shifted the bottleneck to code review."

* * *

News Analysis – Artificial Intelligence Beat

## Harvard researchers find AI coding agents shifted the bottleneck to code review

*ComplexDiscovery OÜ Staff*

Engineers at firms that adopted AI coding agents added about 30 percent more code, but finished work did not measurably rise and getting code through review took 49 percent longer, a Harvard working paper found.

In the study’s data, the extra coding produced no detectable rise in finished work, and code review became the bottleneck. Fiona Chen and James Stratton, both Ph.D. candidates at Harvard University, measured that pattern across 725,938 workers at 718 companies. Two legal AI announcements this month, Harvey’s motion-to-dismiss workflow and a court-filing redaction agent Cooley is developing with Google Cloud, place a lawyer at a comparable review step, and neither reports a measure of it.

* * *

### ![](https://complexdiscovery.com/wp-content/uploads/2026/10/ai-coding-agents-review-bottleneck-header.jpg)

* * *

### What the Harvard data measured

How much weight that pattern can bear depends on where the numbers came from. Chen and Stratton built the study on data from Jellyfish, a company that sells an analytics platform tracking engineering activity, and joined it to LinkedIn employment records collected by Revelio Labs. The sample runs from January 2021 to March 2026 and covers Jellyfish client firms that consented to research use of their data. The paper’s tables list 68.3 million Jira issues, 200.9 million commits, 49.1 million pull requests and 21.3 million calendar events, though only 213 of the 718 firms supplied calendar data.

The design is a staggered difference-in-differences comparison, which tracks month by month how outcomes changed at adopting firms against firms that had not yet adopted. A comparison like that is only as good as the reasons adoption dates differed, and here the authors point to a process legal and security teams will recognize, because they sit inside it. The paper says timing was shaped mainly by when firms learned about the tools and by how long procurement took, meaning sourcing, legal and security approvals and pilot programs. One interviewee’s firm approved a tool in about a day; another interviewee put the average approval time at six to 12 months.

The authors call those procurement delays plausibly unrelated to how fast a firm was growing, which they argue supports comparing early and late adopters. They acknowledge one known imbalance, that larger firms adopted earlier, and they control for firm size. If some hidden difference remains, the authors say, it would more likely make AI look more productive than it is, not less. A bias in that direction would make the flat output result, if anything, generous to AI.

The current version of the paper is dated Aug. 4, 2026, and the first was dated Jan. 7. Chen circulated it as her job-market paper, and nothing in the version reviewed for this article indicates journal acceptance or completed peer review. Her website lists coverage links to The Wall Street Journal and The Economist, and legal trade press picked it up this month, including an Oct. 12 eDiscovery Today post by Doug Austin that relayed an Ars Technica report.

### Drafts went up and finished issues did not

With that provenance in view, the coding results are large. Lines of code added rose by 1,494 per worker-month against a baseline of 5,017, about 30 percent. Commits rose 20 percent, and pull requests, the bundles of code submitted for review, rose 23 percent. The authors note that these estimates capture the added effect of agents at firms that already had AI coding assistants, not the effect of AI from a standing start.

Output is where the gain stops. The paper counts finished work as Jira issues resolved, and an issue counts as complete when its code is deployed to production. Agents moved that measure by 0.12 issues per worker-month on a 3.67 baseline, with a standard error of 0.17. That result is statistically indistinguishable from zero, and the 95 percent confidence interval rules out an increase above about 12 percent of the baseline, well short of the 30 percent rise in code written.

The authors tested the obvious escape route. If engineers had shifted to larger tasks, a flat issue count would understate real output. So the researchers labeled a 0.5 percent sample of issues by size with GPT-4o and extended those labels to every issue with a text-embedding model. They found no shift in average issue size, and pull requests did not grow larger either, which the authors use to set aside the simplest explanation for what happened in review.

### Where the shortfall shows up: the review queue

The paper locates the shortfall in code review. The time from pull request submission to merge rose 3.45 days on a 7.03-day baseline, a 49 percent increase. The share of pull requests receiving a formal change request increased by about 12 percentage points from a baseline of 13 percent, nearly doubling, and review comments per pull request rose 35 percent. The share of workers doing any code review rose 14 percent from a base of about 29 percent, while the share writing code showed no measurable change.

AI review tools did not remove the bottleneck. By March 2026, nearly 80 percent of the firms had used one, yet AI generated only 23.3 percent of review comments, and only 10.8 percent of pull requests received even one AI comment. The authors write that the bottleneck persists after firms adopt such tools. The paper’s interviews show what that looks like on a team: a junior software engineer described a project where only one senior engineer could approve changes and could review the interviewee’s code only once a week. “After some outages that were caused by AI-generated commits, there were also new rules requiring senior engineer approval for AI-generated code, which created even more review bottlenecks,” the engineer said, in a quote the authors say they lightly edited for clarity.

That account fits the model the authors use to explain their numbers, which has two channels. In the first, faster drafting sends more code to a fixed pool of reviewers, a problem that moving labor into review can largely relieve. In the second, the rise in change requests and comments, which faster drafting alone does not explain, reads through the model as a sign of lower-quality drafts. That problem, the authors say, would require better technology. Anyone buying an agent should care which channel dominates, because the two call for different spending.

### Employment did not move either

Nor did the coding gains show up as job cuts. The estimates rule out a decline in overall employment larger than 2.9 percent. A noisier estimate rules out a decline in engineering employment larger than 13.8 percent. The authors caution that these are short-run effects measured in partial equilibrium, which could mask job losses at competing firms, and that the assistant estimate for engineering employment shows upward pre-trends that make it hard to read causally.

### Legal AI tools are designed around a similar checkpoint

Those software results arrive as legal AI tools adopt a related design, and the Harvey and Cooley announcements both follow it: automate a first pass, then keep a lawyer at the checkpoint. Harvey’s workflow, co-developed with LexisNexis and announced Oct. 7, is the drafting case. Harvey says the multi-step process of reading the complaint, researching and drafting a motion to dismiss “commonly takes 40 to 50+ hours of work” and that its tool produces the research and a first draft “in a matter of hours.” The announcement describes “human checkpoints at the moments where a lawyer’s judgment matters most,” but it does not state how long lawyers spend at those checkpoints or how long it takes to reach a motion ready to file.

Cooley’s Oct. 8 announcement applies the same posture to a different task, since its agent recommends redactions rather than drafting arguments. Cooley says the agent is still being developed and that attorneys make all redaction determinations before a filing goes to the court, which puts the human checkpoint at the last step before submission. As ComplexDiscovery reported Oct. 10, the announcement carries no error rate.

For a motion filed in federal court, the checkpoint is a legal duty rather than a design choice. Under Federal Rule of Civil Procedure 11(b), an attorney presenting a motion certifies several things about it to the best of the attorney’s knowledge, information and belief, “formed after an inquiry reasonable under the circumstances.” Its legal contentions must be “warranted by existing law or by a nonfrivolous argument for extending, modifying, or reversing existing law or for establishing new law.” Its factual contentions must have evidentiary support or, if specifically so identified, be likely to have it after a reasonable opportunity for further investigation or discovery. A first draft can arrive in hours, but that inquiry stays with the attorney, and the software evidence raises the possibility that review could absorb some of the drafting savings.

The paper’s method carries a second lesson, this one for information governance teams. The researchers identified agent use partly from traces inside the firms’ own systems, including bot accounts tied to AI tools and signatures such as “Co-authored-by” lines in commit messages and pull request descriptions. AI authorship can leave its own metadata trail in collaboration systems. That trail is a record that a retention schedule and a legal hold may need to reach.

None of this means legal work will behave like software, and the limits of the comparison matter. Document review is not code review, a production is not a pull request, and Chen and Stratton studied no legal work. A separate National Bureau of Economic Research working paper, revised in September 2026, reports a similar falloff using data on over 500,000 GitHub developers. Its authors, Mert Demirer, Leon Musolff and Liyuan Yang, found a 240 percent cumulative rise in commits from autonomous agents shrinking to 30 percent for actual releases. That paper carries its own disclosure: Demirer and Musolff each previously held a postdoctoral research position at Microsoft, which owns GitHub, and each now works as a paid research consultant for the company. Two software studies point to a pattern in software, not a finding about law firms.

### Document review offers a partial precedent

Discovery practitioners have argued a version of this question before, and Austin’s post draws that comparison. He wrote that discovery has seen a similar debate over generative AI review, with some practitioners saying manual validation offsets the gains and others, including him, saying statistical sampling of the kind used in predictive coding is enough. For coding agents, though, he wrote that the extra review time may be warranted, since code that is 90 percent correct can still fail to run.

His comparison suggests a useful split between two kinds of legal output. Population-level work such as first-pass relevance review can be validated by sampling. A single filing is different, because its citations, factual assertions and sensitive data generally need direct examination when one fabricated citation or one missed Social Security number is the failure. Buyers can turn that split into a test. Ask a vendor which kind of output its agent produces, then ask for the downstream measure rather than the drafting one: hours from upload to an approved work product, the share of drafts a reviewing attorney sent back and reviewer time per matter. Budget review capacity before scaling drafting volume, and log what reviewers change, because that log shows whether the bottleneck is volume or quality.

### Read the paper’s limits as closely as its findings

Those questions apply to the paper’s own sourcing, too. The data partner has a commercial interest in the subject, and the relationship runs deeper than a data license. Jellyfish describes itself as an “AI Impact Platform,” and the authors have published on its blog: a Sept. 25, 2025, post by Chen and Stratton described itself as “part of our ongoing work with Jellyfish.” The paper thanks the Jellyfish team “for their partnership” and states that Jellyfish reviewed the manuscript only to check for proprietary disclosure, not for scientific content, and that the conclusions are the authors’. Its funders were the Stripe Economics of AI Fellowship, a Harvard Kennedy School program on wealth distribution and the Schmidt Sciences AI at Work Program.

Jellyfish has also promoted an earlier version of the work. A Jan. 28 post on its blog by Nicholas Arcolano called it “a preliminary version of a new paper from our collaborators at Harvard Economics, Fiona Chen and James Stratton,” based on 100,000 software engineers across 500 companies. That post, and a March 17 Jellyfish press release after it, reported that AI was making coding faster and that “code quality doesn’t seem to be suffering.” The August version, built on a larger sample running through March 2026, reads the near-doubling in change requests through its model as a possible sign of lower draft quality. The two descriptions rest on different versions and samples, so they need not conflict, but a reader relying on Jellyfish’s summary would miss the quality question the current paper raises.

The paper also carries small internal inconsistencies that do not touch the headline agent estimates. Its introduction rounds the sample to about 700 firms and 700,000 workers, and its main text describes 17 interviews where an appendix describes 21. Other limits are structural rather than clerical: the underlying data are proprietary, and use of AI tools through individual licenses goes unrecorded.

None of those caveats overturns the central result, though the March 2026 data cutoff means a revision could move the figures. What the paper offers is a standard the two legal announcements discussed here do not meet: named authors, a disclosed method, a confidence interval and a measured checkpoint. If an agent cuts 40 hours of research and drafting to a few, how many of those hours reappear on the desk of the lawyer who signs the motion?

* * *

![](https://complexdiscovery.com/wp-content/uploads/2026/10/ai-coding-agents-review-bottleneck-footer.jpg)

* * *

News sources

- [Artificial Intelligence in the Firm: Bottlenecks in Software Production](https://fion.ac/jellyfish.pdf) (Fiona Chen and James Stratton, Harvard University)
- [Fiona Chen](https://fion.ac/) (Fiona Chen)
- [James Stratton | Department of Economics](https://www.economics.harvard.edu/people/james-stratton) (Harvard University Department of Economics)
- [Writing Code vs. Shipping Code: Productivity Effects Across Generations of AI Coding Tools](https://www.nber.org/papers/w35275) (National Bureau of Economic Research)
- [Rule 11. Signing Pleadings, Motions, and Other Papers; Representations to the Court; Sanctions](https://www.law.cornell.edu/rules/frcp/rule_11) (Legal Information Institute, Cornell Law School)
- [Introducing the Motion to Dismiss Workflow, Co-Developed With LexisNexisⓇ](https://www.harvey.ai/blog/motion-to-dismiss-workflow-lexisnexis) (Harvey)
- [Cooley Collaborates With Google Cloud to Develop AI Agent for Complex Litigation](https://www.cooley.com/news/coverage/2026/2026-10-08-cooley-collaborates-with-google-cloud-to-develop-ai-agent-for-complex-litigation) (Cooley)
- [Jellyfish Reveals AI’s Real Impact on Engineering Teams with Industry’s Largest Ongoing Benchmark Study](https://jellyfish.co/newsroom/jellyfish-reveals-ais-real-impact-on-engineering-teams/) (Jellyfish)
- [New Research from Harvard & Jellyfish: AI Is Making Developers Faster. So Where’s the Business Impact?](https://jellyfish.co/blog/harvard-jellyfish-ai-is-making-developers-faster/) (Jellyfish)
- [Does AI Assistance Mean Engineers Are Doing Less Coding Work? Maybe Not.](https://jellyfish.co/blog/does-ai-assistance-mean-engineers-are-doing-less-coding-work-maybe-not/) (Jellyfish)
- [AI Coding Agents Productivity Gains Offset by Human Review](https://ediscoverytoday.com/2026/10/12/ai-coding-agents-productivity-gains-offset-by-human-review-artificial-intelligence-trends/) (eDiscovery Today)
- [Cooley and Google Cloud are building a court-filing redaction agent without a published error rate](https://complexdiscovery.com/cooley-and-google-cloud-are-building-a-court-filing-redaction-agent-without-a-published-error-rate/) (ComplexDiscovery)

* * *

[![](https://complexdiscovery.com/wp-content/uploads/2022/09/Organization-EDRM-Standard.jpeg)](https://edrm.net/)

* * *

[Assisted by GAI and LLM Technologies](https://complexdiscovery.com/generative-artificial-intelligence-and-large-language-model-policy/)

Additional reading

- [MonsterCloud indictment exposes the ransom payment a victim’s own file may not record](https://complexdiscovery.com/monstercloud-indictment-exposes-the-ransom-payment-a-victims-own-file-may-not-record/)
- [Wikimedia publishes its own account of suspected OpenAI agent activity on its wikis](https://complexdiscovery.com/wikimedia-publishes-its-own-account-of-suspected-openai-agent-activity-on-its-wikis/)
- [Cyber Law Toolkit tests AI deception, cyber torture and peacetime cyber duties](https://complexdiscovery.com/cyber-law-toolkit-tests-ai-deception-cyber-torture-and-peacetime-cyber-duties/)
- [ENISA Threat Landscape 2026 finds DDoS leads incident counts while ransomware stays most impactful in the short term](https://complexdiscovery.com/enisa-threat-landscape-2026-finds-ddos-leads-incident-counts-while-ransomware-stays-most-impactful-in-the-short-term/)
- [Cyber Resilience Act reporting starts Sept. 11 on an unfinished platform](https://complexdiscovery.com/cyber-resilience-act-reporting-starts-sept-11-on-an-unfinished-platform/)
- [Europe’s draft cloud rule would put vendor ownership in the audit file](https://complexdiscovery.com/europes-draft-cloud-rule-would-put-vendor-ownership-in-the-audit-file/)
- [The eight-hour clock starts today: EU e-evidence orders now land on covered U.S. providers’ EU addressees](https://complexdiscovery.com/the-eight-hour-clock-starts-today-eu-e-evidence-orders-now-land-on-covered-u-s-providers-eu-addressees/)
- [California’s AI Transparency Act arrives alongside Europe’s Article 50](https://complexdiscovery.com/californias-ai-transparency-act-arrives-alongside-europes-article-50/)
- [Federal magistrate judge treats LinkedIn’s Relativity aiR workflow as TAR](https://complexdiscovery.com/federal-magistrate-judge-treats-linkedins-relativity-air-workflow-as-tar/)
- [One benchmark, three directions: 2026 legal rates rise, flatten and fall at once](https://complexdiscovery.com/one-benchmark-three-directions-2026-legal-rates-rise-flatten-and-fall-at-once/#)
- [Confidence cools, commitment holds: full results from the 1H 2026 eDiscovery Business Confidence Survey](https://complexdiscovery.com/confidence-cools-commitment-holds-full-results-from-the-1h-2026-ediscovery-business-confidence-survey/)
- [Complete look: ComplexDiscovery OÜ’s 2025 to 2030 eDiscovery market size mashup](https://complexdiscovery.com/complete-look-complexdiscovery-ous-2025-to-2030-ediscovery-market-size-mashup/#)
- [The workstream of eDiscovery: Considering processes and tasks](https://complexdiscovery.com/the-workstream-of-ediscovery-considering-processes-and-tasks/)

Source: ComplexDiscovery OÜ

![](https://complexdiscovery.com/wp-content/uploads/2026/10/AI-Report-Two.jpg)

> ComplexDiscovery’s mission is to enable clarity for complex decisions by providing independent, data‑driven reporting, research, and commentary that make digital risk, legal technology, and regulatory change more understandable for practitioners, policymakers, and business leaders.

**Have a Request?**

If you have information or offering requests that you would like to ask us about, please let us know, and we will make our response to you a priority.

**ComplexDiscovery OÜ** is an independent digital publication and research organization based in Tallinn, Estonia. ComplexDiscovery covers cybersecurity, data privacy, regulatory compliance, and eDiscovery, with reporting that connects legal and business technology developments—including high-growth startup trends—to international business, policy, and global security dynamics. Focusing on technology and risk issues shaped by cross-border regulation and geopolitical complexity, ComplexDiscovery delivers editorial coverage, original analysis, and curated briefings for a global audience of legal, compliance, security, and technology professionals. Learn more at [ComplexDiscovery.com](http://ComplexDiscovery.com).

**Generative Artificial Intelligence and Large Language Model Use**

ComplexDiscovery OÜ recognizes the value of GAI and LLM tools in streamlining content creation processes and enhancing the overall quality of its research, writing, and editing efforts. To this end, ComplexDiscovery OÜ regularly employs GAI tools, including ChatGPT, Claude, Gemini, Grammarly, Midjourney, and Perplexity, to assist, augment, and accelerate the development and publication of both new and revised content in posts and pages published (initiated in late 2022).
