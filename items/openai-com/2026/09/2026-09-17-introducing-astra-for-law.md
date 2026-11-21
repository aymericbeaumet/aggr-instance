---
title: Introducing Astra for Law
link: https://openai.com/index/astra-for-law
source: openai-com
published: 2026-09-17T00:00:00Z
updated: 2026-09-17T00:00:00Z
first_seen: 2026-10-19T01:26:53.998049939Z
labels:
- company
summary: OpenAI for Law brings frontier intelligence for law, custom firm workflows, connected legal data sources, and legal-grade controls for confidential client work.
content: extracted
html: 2026-09-17-introducing-astra-for-law.html
---

Today, we’re introducing Astra for Law: a new foundation for law firms and legal technology companies to build AI products and workflows around their expertise. It combines GPT‑6 Astra, our latest and most powerful model, with settings, tools, and context tailored for professional legal work.

API customers including Harvey and Legora will be able to build on Astra for Law, bringing this intelligence into their own products and workflows. As our frontier models advance, we’ll bring these legal capabilities to our latest models.

We are also expanding our work on privacy and governance to give law firms specific controls for confidential client work. Firms can also customize Astra for Law using our 26 new ecosystem plugins that connect ChatGPT to the specialist tools firms already use, like Relativity and Clio.

## Frontier intelligence for law

Astra for Law combines GPT‑6 Astra with a powerful legal search index and instructions for legal analysis and writing. Together, they amplify Astra’s capabilities across the legal practice, while giving firms and legal technology companies the freedom to build their own applications and workflows.

### Legal research: from facts to a supported answer

Our new legal search index is one of the tools Astra for Law can use. Legal research often begins with finding the exact right authority, locating the relevant passages, and understanding how relevant and binding they are to the situation at hand. The index helps Astra for Law do that work, and complements the licensed content and specialist products firms rely on from providers such as Thomson Reuters.

By using the legal search index, Astra for Law can search U.S. case law, statutes, regulations, court rules, and administrative decisions across a corpus of more than 230 million URLs, with sources added daily. Our work with Free Law Project, the nonprofit behind CourtListener, brings its case-law collection covering [more than 99.9% of published U.S. precedential case law](https://wiki.free.law/c/courtlistener/help/data-coverage/case-law) into this research experience.

To measure how this configuration improves legal research, we tested Astra for Law’s complete setup on 200 U.S. legal research questions from the private validation set of [Vals AI’s Legal Research Bench](https://vals.ai). This benchmark measures how well the model can find relevant sources and passages, and how well its research answers meet the evaluation criteria.

At the highest reasoning effort for both systems, Astra for Law passed the evaluation’s overall correctness check on 54.0% of questions, compared with 38.7% for GPT‑6 Astra using web search alone – a 40% relative improvement. Astra for Law also produces more comprehensive answers.

On case-law-focused questions, Astra for Law found 24% more reference cases than GPT‑6 Astra using web search alone at the highest reasoning effort. On the audited set of target passages, it retrieved up to 54% more relevant passages from the correct court opinions, when comparing the systems at the same reasoning effort.

The result is a stronger research foundation for advising on a deal, assessing a dispute, or developing a legal strategy, with reliable authorities the lawyer can examine for herself.

Weighted rubric score (chart data)
| Model                       | Answers | Cost usd | Reasoning setting | Simulated latency seconds | Weighted rubric score pct |
| --------------------------- | ------- | -------- | ----------------- | ------------------------- | ------------------------- |
| GPT-6 Astra with web search | 600     | 0.9547   | 4                 | 69.8006                   | 79.8022                   |
| GPT-6 Astra with web search | 600     | 1.8318   | 10                | 111.3973                  | 82.4598                   |
| GPT-6 Astra with web search | 600     | 2.9761   | 24                | 181.3627                  | 83.5956                   |
| GPT-6 Astra with web search | 600     | 3.3834   | 64                | 211.1968                  | 84.0722                   |
| GPT-6 Astra with web search | 600     | 4.8607   | 832               | 333.4744                  | 83.4029                   |
| Astra for Law               | 600     | 1.3082   | 4                 | 78.4554                   | 86.7475                   |
| Astra for Law               | 600     | 2.4551   | 10                | 122.7027                  | 88.3725                   |
| Astra for Law               | 600     | 3.4887   | 24                | 180.8963                  | 89.6146                   |
| Astra for Law               | 600     | 3.9559   | 64                | 211.6925                  | 89.8576                   |
| Astra for Law               | 600     | 5.6684   | 832               | 388.2566                  | 89.9757                   |

All-pass rate — Legal research answer quality by model cost or simulated latency and reasoning setting (chart data)
| Model                       | All pass rate pct | Answers | Cost usd | Reasoning setting | Simulated latency seconds |
| --------------------------- | ----------------- | ------- | -------- | ----------------- | ------------------------- |
| GPT-6 Astra with web search | 30                | 600     | 0.9547   | 4                 | 69.8006                   |
| GPT-6 Astra with web search | 35.3333           | 600     | 1.8318   | 10                | 111.3973                  |
| GPT-6 Astra with web search | 39.3333           | 600     | 2.9761   | 24                | 181.3627                  |
| GPT-6 Astra with web search | 39.5              | 600     | 3.3834   | 64                | 211.1968                  |
| GPT-6 Astra with web search | 38.6667           | 600     | 4.8607   | 832               | 333.4744                  |
| Astra for Law               | 46.8333           | 600     | 1.3082   | 4                 | 78.4554                   |
| Astra for Law               | 51.1667           | 600     | 2.4551   | 10                | 122.7027                  |
| Astra for Law               | 52.8333           | 600     | 3.4887   | 24                | 180.8963                  |
| Astra for Law               | 53.1667           | 600     | 3.9559   | 64                | 211.6925                  |
| Astra for Law               | 54                | 600     | 5.6684   | 832               | 388.2566                  |

Astra for Law and GPT‑6 Astra’s performance on the Vals AI Legal Research Bench validation set, across reasoning effort settings.

### Improving performance on end-to-end legal workflows

Legal research is only the first step. Custom instructions for legal analysis and writing guide Astra for Law in applying that research to the client’s facts, developing arguments or deal terms, and identifying weaknesses and uncertainty. That can mean distinguishing a court’s holding from its other observations, addressing cases that weaken an argument, or explaining how a contract exception shifts risk between the parties.

For example, when prompted to identify good law with similar fact patterns, Astra for Law could both pinpoint relevant precedent and match fact patterns better than other frontier models:

Astra for Law will be initially offered to selected law firms through Trusted Access in ChatGPT and Codex, and will be coming soon to the API. It will appear in the model picker as “GPT‑6 Astra Law” and in the API as gpt-6-astra-law.

> “We were grateful to preview early versions of Astra for Law, which were built with legal use-cases in mind. Across both litigation and transactional matters, the models demonstrated impressive research depth and sensitivity to authority. Even at this early stage, they felt like a significant step toward legal-focused AI that is carefully grounded in research that is both current and comprehensive.”

John Savva, Partner, **Sullivan & Cromwell**

> “In our early testing, Astra for Law showed strength across key aspects of legal research: grounding answers in on-point authorities, citing with precision, and offering practical, advisory guidance.”

Niko Grupen, Head of Applied Research at Harvey

## Legal-grade trust and controls

Law firms need to protect client confidences and control how AI is used in their practice. We’ve created a special Trusted Access Program for eligible law firms to give lawyers and people working under their supervision access to Astra for Law for professional legal work. For eligible firms, the offering includes Zero Data Retention (ZDR) on our API, and usage of ChatGPT Enterprise is excluded from human review by default.

We are also working with **Latham & Watkins**, a leader in AI governance, to design for information permissions, ethical walls, client instructions, and firm oversight.

## Build ChatGPT around your firm’s expertise

With frontier intelligence and the right controls, firms can turn their own precedents, methods, and judgment into AI tools and workflows built to their standards.

Working with selected firms, our forward-deployed engineers have been adapting ChatGPT Enterprise with custom interfaces and integrations to proprietary data, creating tools for each firm’s workflows:

- **Sullivan & Cromwell** built an agreement analyzer that brings the firm’s negotiating playbooks and selected precedents into the review of a new deal. It helps lawyers spot risks that emerge when provisions are read together, then turns those findings into proposed redlines and draft client advice they can challenge and refine.

- **Ropes & Gray** built a deal diligence system around how its lawyers work through a data room and decide what matters to the deal. It helps them trace findings back to the source and pinpoint questions that could affect an acquisition, such as whether key customer contracts require notice or consent.

- **Cooley** built GO Public to bring its capital markets expertise into how companies prepare to go public, from drafting the IPO filing to identifying the risks that deserve management’s attention. When the deal changes, it carries that change across the filing so lawyers can review the implications together.

1 of 4

Firms can build with their own teams and partner products, with permitted sources and review processes defined for the work.

## Frontier intelligence, connected to the most trusted tools in legal

We’re proud to work with the specialist companies who are advancing legal AI. Today, we’re launching [26 partner-built plugins](https://openai.com/business/plugins/?tab=plugins-legal) that help firms go deeper with the tools and knowledge they already use. These plugins cover the practice and business of law. With iManage, a lawyer can draft a negotiation brief in ChatGPT and save it to the matter file; Intapp can surface activities that may need a time entry for review; DeepJudge can bring prior deals into a comparison. Thomson Reuters is bringing HighQ matter context into ChatGPT and previewing a forthcoming CoCounsel Legal connector.

The launch includes 9 community plugins from lawyers and legal engineers at LegalQuants, LECG, and Skills.law, with 47 custom skills that practitioners can adapt, extend, or draw inspiration from to do their work in ChatGPT. Community plugins give the people closest to the work a way to keep setting the standard for what these skills can do. We’re also making ChatGPT for Word generally available today, so lawyers can proofread, get suggested edits, and flag formatting issues in the tool they already rely on for drafting.

Our approach is open and composable: firms can use specialist products, bring their own tools and knowledge, and adapt community-built skills to their practice. Partners can keep developing their own applications and workflows, while firms choose how those capabilities fit together. Building on OpenAI should mean getting more from the ecosystem, not replacing it.

## Build with us

OpenAI is investing in law for the long term. We’ll keep advancing Astra for Law’s model, settings, tools, and instructions together, guided by rigorous evaluations and feedback from lawyers and legal technology partners. We’ll also keep improving ChatGPT so firms can build around their expertise and connect the tools they use.

Our work with **Wachtell, Lipton, Rosen & Katz** brings the firm’s litigation and corporate expertise together with our frontier research and engineering to explore how AI can support the work involved in providing sophisticated legal judgment.

The most ambitious applications will come from the people who practice law and the companies building alongside them. We’re committed to working with this ecosystem so firms can bring their own expertise, standards, and judgment to AI—and serve clients in ways only they can.

To explore early access to Astra for Law, build it into your product, or develop tools around your firm’s expertise, contact OpenAI.
