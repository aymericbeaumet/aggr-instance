---
title: Git for US Legislation
link: https://lexifina.com/blog/git-for-the-us-code
source: hnrss-org
published: 2026-10-28T01:47:50Z
updated: 2026-10-28T01:47:50Z
first_seen: 2026-10-28T05:25:47.188168527Z
authors:
- alansaber
content: extracted
html: 2026-10-28-git-for-us-legislation.html
remote_preview:
  url: https://lexifina.com/social-preview-v3.png?v=20261018
  alt: Git for the US Code
---

A programmer can point at any line of code and see who wrote it, when, and what the file looked like on any past day. A lawyer reading a federal statute usually cannot, at least not without an afternoon in the notes at the foot of the section. But why shouldn't they have the same, convenient system of record? It is especially relevant given that so much first-pass legal research now runs through language models, which are often unable to utilise the proper lineage of a law, handcuffing its ability to offer interpretation.

## The US code

Outside the titles Congress has enacted as positive law, the code is only prima facie evidence of the law, and the Statutes at Large govern where the two differ.[^1] The Office of the Law Revision Counsel publishes it as a series of release points, each current through a named public law, and keeps reorganising it; a bill to enact title 41 as positive law was introduced in September 2025.[^2] Which public law wrote a sentence is recorded in amendment notes, in prose. When a change took effect is often in a separate effective-date note, and where Congress said nothing the default rule is that a law takes effect on the day it is enacted.[^3]

The definition of a small business debtor in 11 USC § 1182(1) shows how far apart those answers can be. It was created in 2019 with the existing small business debt limit.[^4] The CARES Act raised the limit to $7.5 million in March 2020 as a temporary measure, later extended to March 2022.[^5],[^6] That increase lapsed on 27 March 2022, and on 21 June 2022 Congress restored it for two more years, for cases commenced on or after 27 March 2020 and still pending, which reached back over the gap.[^7] The higher limit expired again in June 2024, and after the triennial adjustment in April 2025 it stands at $3,424,000.[^8],[^9] For a case filed in May 2022, the release point printed at the time gives the lower limit, and only a later Act changes the answer. A history that is useful to a lawyer has to carry the text, the date each change took force, and the scope of any law that reaches back.

A study from the Technical University of Munich this year found two failures in models answering questions on German statutes: they apply rules that were superseded after their training cutoff, and they prefer the newer provision even when an older one governs the facts. One model answered 24% of post-cutoff questions correctly on its own and 87% when the relevant statutory text was retrieved for it, and the authors conclude that temporal validity has to be treated as a hard constraint.[^10] Other recent work trains search agents to respect the date a question is about, with large gains in consistency.[^11]

Asking a model to produce the old text does not solve this. When the same Munich group had a model apply German amending laws to earlier versions, the output looked 93–99% similar to the official text, but only half of single amendments and a fifth of chains of amendments matched exactly, and experts found legally significant errors in the near misses.[^12] The history has to be checkable.

For each pair of release points from 2017 onward, we took the public laws enacted in between, carried out their amending instructions on the older text in date order, and compared the result with the newer version. Of 365 changes across the nine titles, 361 reproduced the next release point exactly. The other four are shown with a label saying how far they could be confirmed. Each change then carries the law and the date it took effect: 261 took effect on enactment by default, 72 by an effective-date note and 29 by a date in the amendment itself. One depends on an agency determination that may never come, and we leave it out of the text rather than guess.

With that record, the reader can answer the questions lawyers ask. You pick a date and get the text in force on it, with the words that read differently today marked. Each phrase is coloured by the Act that wrote it. A change to a defined term lists every other provision that uses it, and you can follow a section to see what has changed since your last visit.

## Limits

It does not yet show a later law’s reach back on the earlier dates it covers, so for a case filed in the 2022 gap it shows the lower limit without warning that the June 2022 Act changed the answer.

Effective-date notes are read by pattern, and unusual wording falls back to the date of enactment.

Each change links to the law’s own words, and for anything that matters the original should be read.

We're also still thinking about the right form factor this should take.

## References

[^1]: 1 USC § 204(a) [\<www.law.cornell.edu/uscode/text/1/204>](https://www.law.cornell.edu/uscode/text/1/204)

[^2]: Office of the Law Revision Counsel, ‘Positive Law Codification: Title 41’ (H.R. 5185, 119th Congress, introduced 8 September 2025) [\<uscode.house.gov/codification/t41/index.html>](https://uscode.house.gov/codification/t41/index.html)

[^3]: *Gozlon-Peretz v United States* 498 US 395, 404 (1991) [\<www.law.cornell.edu/supremecourt/text/498/395>](https://www.law.cornell.edu/supremecourt/text/498/395)

[^4]: Small Business Reorganization Act of 2019, Pub L 116-54 [\<www.govinfo.gov/content/pkg/PLAW-116publ54/html/PLAW-116publ54.htm>](https://www.govinfo.gov/content/pkg/PLAW-116publ54/html/PLAW-116publ54.htm)

[^5]: Coronavirus Aid, Relief, and Economic Security Act, Pub L 116-136, s 1113 [\<www.govinfo.gov/content/pkg/PLAW-116publ136/html/PLAW-116publ136.htm>](https://www.govinfo.gov/content/pkg/PLAW-116publ136/html/PLAW-116publ136.htm)

[^6]: COVID-19 Bankruptcy Relief Extension Act of 2021, Pub L 117-5 [\<www.govinfo.gov/content/pkg/PLAW-117publ5/html/PLAW-117publ5.htm>](https://www.govinfo.gov/content/pkg/PLAW-117publ5/html/PLAW-117publ5.htm)

[^7]: Bankruptcy Threshold Adjustment and Technical Corrections Act, Pub L 117-151, s 2(h)–(i) [\<www.govinfo.gov/content/pkg/PLAW-117publ151/html/PLAW-117publ151.htm>](https://www.govinfo.gov/content/pkg/PLAW-117publ151/html/PLAW-117publ151.htm)

[^8]: Judicial Conference of the United States, ‘Adjustment of Certain Dollar Amounts Applicable to Bankruptcy Cases’ 90 Fed Reg 8941 (4 February 2025) [\<www.govinfo.gov/content/pkg/FR-2025-02-04/html/2025-02207.htm>](https://www.govinfo.gov/content/pkg/FR-2025-02-04/html/2025-02207.htm)

[^9]: US Trustee Program, ‘Subchapter V’ (US Department of Justice) [\<www.justice.gov/ust/subchapter-v>](https://www.justice.gov/ust/subchapter-v)

[^10]: Max Prior, Andreas Schultz and Matthias Grabmair, ‘Asking For An Old Friend: Diagnosing and Mitigating Temporal Failure Modes in LLM-based Statutory Question Answering’ (arXiv:2605.23497, 22 May 2026) [\<arxiv.org/abs/2605.23497>](https://arxiv.org/abs/2605.23497)

[^11]: Wei Fan and others, ‘Can LLMs Time Travel? Enhancing Temporal Consistency in Legal Agentic Search through Reinforcement Learning’ (arXiv:2605.25920, revised 19 September 2026) [\<arxiv.org/abs/2605.25920>](https://arxiv.org/abs/2605.25920)

[^12]: Max Prior, Adrian Hof, Niklas Wais and Matthias Grabmair, ‘Risks and Limits of Automatic Consolidation of Statutes’ in *Proceedings of the Natural Legal Language Processing Workshop 2025* (ACL, November 2025) 396 [\<aclanthology.org/2025.nllp-1.29/>](https://aclanthology.org/2025.nllp-1.29/)
