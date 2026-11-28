---
title: Our first phase of embedded assessments
link: https://openai.com/index/our-first-phase-of-embedded-assessments
source: openai-com
published: 2026-11-13T08:00:00Z
updated: 2026-11-13T08:00:00Z
first_seen: 2026-11-16T17:21:52.095799575Z
labels:
- safety
summary: OpenAI begins embedded assessments with Apollo Research, METR, and Transluce, and Goodfire to independently evaluate model alignment, safeguards, and monitoring systems.
content: extracted
html: 2026-11-13-our-first-phase-of-embedded-assessments.html
---

We have signed agreements with four independent assessors—Apollo Research, METR, Transluce, and Goodfire—for our first phase of embedded assessments. As AI capabilities advance rapidly and the potential risks grow, public reporting from independent assessments can give people a clearer view of how labs are evaluating and managing those risks.

Embedded assessments give independent experts deep access to a lab’s internal models and systems to assess model alignment and the effectiveness of safeguards over an extended period of time, especially where relevant to our safety cases. Based on what each evaluation needs, assessors receive access to some of the same internal resources our researchers use, including model checkpoints, source code, training and evaluation data, monitoring systems, technical documentation, and discussions with the researchers and engineers responsible for the work. This access gives assessors the evidence and context to pursue independent evaluations and uncover failures we may not have anticipated.

Our initial assessments focus on alignment and safeguards for models during training and internal use, complementing our [safety cases](https://openai.com/index/towards-safety-cases-for-frontier-ai-training/). They build on our [priorities and principles for third party assessments](https://openai.com/index/priorities-principles-third-party-assessments/) and years of work with [third party evaluators](https://openai.com/index/strengthening-safety-with-external-testing/), including a recent pilot assessment of [our internal monitoring systems](https://metr.org/blog/2026-10-22-red-teaming-openai-agent-monitoring/) with deep internal access and an investigation of [the Hugging Face incident](https://metr.org/blog/2026-08-26-openai-hugging-face-incident-investigation/). We’ll use what we learn to deepen our work with independent experts in the coming months. These efforts also support our commitments under the White House Accord on Super Intelligence, where we agreed to independent assessments of our safety controls. These collaborations can contribute to stronger shared standards for independent assessment and public transparency across the industry.

## Our pilot embedded assessors

### Apollo Research

In this pilot assessment, [Apollo Research](https://www.apolloresearch.ai/) will assess whether models could covertly work against their developers’ intentions and whether our safeguards would detect such behavior. Drawing on evidence from training, safety testing, and internal use. Apollo will assess four claims in two stages:

1. Training did not incentivize subversion.

2. Strong evaluations and red teaming do not elicit subversion.

3. Models have not attempted subversion during internal use.

4. Reasoning behind subversion would be detectable.

For the first stage of Apollo’s assessment, we’ll provide access to selected public and unreleased models and training checkpoints, relevant training data and evaluation results, training intervention records, monitoring systems and flagged behaviors, and researchers who can provide context, alongside the tools and compute needed to conduct independent testing.

### Goodfire

[Goodfire](https://www.goodfire.com/) will embed interpretability researchers and use the Goodfire platform to analyze model internals. The main objective of their assessment is training run monitoring, debugging, and alignment. The primary audience for their assessments are internal OpenAI researchers, who will use their findings to make safety improvements. Goodfire will also publish learnings and findings as the collaboration progresses.

### METR

We intend to conduct two assessments with [METR](https://metr.org/), focused on model alignment and monitoring:

- **Alignment assessment:** For the first phase, METR will assess misaligned behavior in our largest reinforcement learning run to date, including failures that our existing tests and monitors may have missed. Within the selected run and agreed access, resources, and safety arrangements, METR would independently set assessment priorities and iterate on searches, analyses, and elicitation experiments. Access will include sampling relevant checkpoints, training and eval rollouts, and misalignment monitoring records for this run. For this first phase, the assessment will focus on our largest frontier RL run.[1](https://openai.com/index/our-first-phase-of-embedded-assessments#citation-bottom-1)

- **Monitor assessment:** We recently worked with METR to conduct a monitor stress testing exercise [2](https://openai.com/index/our-first-phase-of-embedded-assessments#citation-bottom-2). We have preliminarily agreed to conduct another [monitor assessment](https://metr.org/blog/2026-10-14-control-assessment-proposal/) which we intend to be broader, but this is pending mutual agreement on the assessment scope.

### Transluce

[Transluce](https://transluce.org/) will conduct an initial assessment of our post-training alignment evaluations, examining their quality and coverage. For the initial assessment, we will provide Transluce with access to selected public and unreleased models, our post-training alignment evaluations and relevant results, supporting model behavior data and safety case documentation, and researchers who can provide context, alongside the tools and compute needed to conduct independent testing.

OpenAI and Transluce also share the intent to conduct a follow-on assessment of approximately eight weeks in early 2027, with broader access to additional models, deeper access to our alignment evaluations, and information on how evaluation results inform model release decisions.

## Protocols for embedded assessments

We’ve developed protocols and processes with input from the assessors to balance deep access with security, confidentiality, oversight, and the independence and integrity of assessments. We are sharing them to shed light on how we will work with embedded assessors, and to advance discussion about norms for embedded assessments, which have yet to be standardized. We expect that these will need to evolve as we learn more about how to best conduct these assessments. Especially in the early stages of embedded assessments, we expect technical access challenges, space constraints, evolving security protocols, and other unexpected issues to arise. We hope to work through these challenges collaboratively with our embedded assessors and share lessons learned.

### Scoping and iteration

Each engagement begins with an agreed scope and the access, resources, and safety arrangements needed to conduct a rigorous assessment. Some assessments test specific safety claims; others give assessors room to discover unexpected failure modes within a defined system or training run. In both cases, assessors need the freedom to follow evidence, refine their methods, and propose further assessments when findings warrant it. We also have a process for addressing additional access needs as assessments develop. When direct access could expose our most sensitive IP, such as model weights or architecture, we will consider alternatives, including pairing assessors with OpenAI researchers to run analyses.

### A secure, on-site environment

Assessors who need access to sensitive confidential information will work on site at OpenAI offices and be provided OpenAI devices to conduct their work. The primary purpose is to protect the security of the information they access. Working onsite also gives assessors the opportunity to engage with dedicated staff who can help them navigate their access and provide context for their assessments. We also expect our embedded assessors to follow at minimum [3](https://openai.com/index/our-first-phase-of-embedded-assessments#citation-bottom-3) the same security standards as OpenAI employees when running experiments and evaluations.

### Oversight and dispute resolution

Our Safety and Security Committee (SSC), an independent oversight committee of the OpenAI Foundation’s Board, will have visibility into the embedded assessment and the ability to exercise their oversight responsibilities before, during, and at their conclusion. They will receive unredacted reports, and both the assessors and the SSC will be able to speak with each other during the assessment, including privately. Disagreements among OpenAI and any evaluator will be escalated to the Safety Advisory Group and OpenAI executives, who will have a specified time period to resolve them, and the SSC will be notified of any such disagreements promptly when escalated. If a disagreement remains, the matter will be referred to the SSC for resolution.

### Employee discussions and attestation

Discussions with employees help assessors understand internal information and the context behind the work. OpenAI will authorize and provide guidance to staff members who can speak to assessors, and assessors may request additional staff to speak with as needed. To encourage open, candid conversations and protect the privacy of our staff, these discussions are off the record, meaning that they can inform the assessments but cannot be published or attributed to employees. Through a separate attestation pathway, assessors can request OpenAI-attested statements verifying, correcting, or clarifying something they learned in conversation with employees. These statements are not attributed to individuals or intended to characterize individual views. Assessors may publish statements that OpenAI attests to, or disclose that OpenAI declined to attest to a statement.

### Conflict disclosures & cool off periods

Managing conflicts of interest clearly and transparently—including those involving personal relationships, financial interests [4](https://openai.com/index/our-first-phase-of-embedded-assessments#citation-bottom-4), and other issues—is important to the integrity and independence of evaluations. Cool-off periods [5](https://openai.com/index/our-first-phase-of-embedded-assessments#citation-bottom-5) also need to be designed carefully, so assessors can work responsibly across labs while handling competitive information sensitively. We are working with our assessors to develop clear processes for both conflicts and cool-off periods, including allowing for flexibility in mitigations and transparency in decisions to request recusal.

### Publication and responsible disclosure periods

Assessors maintain independence in publishing their assessments, but we will retain the right to redact confidential information, consistent with our previous third-party engagements. We may also request responsible disclosure delays to allow time to act on findings that could pose security or safety risks and remediate issues that remain.

Public reporting will help people understand the findings and their limitations, while more detailed confidential reports give oversight bodies the evidence they need to act. We want these assessments to improve how we identify and address risks, and give the public a clearer basis for judging our safety work.

## Looking ahead

These assessments bring an independent perspective to our ongoing alignment and safety work. Public reporting on their findings will make that work more transparent and provide greater assurance about how risks are being identified and addressed. We expect this first phase of assessments to lead to longer-term, consistent embedded engagements over time that can continuously provide third party safety signals and recommended improvements on our most load bearing safety claims and safety cases.

We’ll continue working with multiple independent third parties to help build a healthy assessment ecosystem with diverse expertise and approaches. Some investigations need sustained, on-site work with sensitive systems; others primarily evaluate deployed or soon to be deployed models or review public documentation.

In the coming months, we’ll support external assessments across a broader range of safety questions. We are also exploring future embedded assessments that would examine compliance with our safety frameworks, as well as our organizational culture and processes. We are committed to expanding this work and helping build the capacity and shared standards needed to sustain it.
