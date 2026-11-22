---
title: Priorities and principles for effective third party assessments
link: https://openai.com/index/priorities-principles-third-party-assessments
source: openai-com
published: 2026-09-22T00:00:00Z
updated: 2026-09-22T00:00:00Z
first_seen: 2026-10-19T01:26:53.998049939Z
labels:
- safety
summary: OpenAI outlines priorities and principles for rigorous, secure, and independent third-party AI safety assessments of frontier models and safeguards.
content: extracted
html: 2026-09-22-priorities-and-principles-for-effective-third-party.html
---

Frontier AI labs carry an immense responsibility in training, evaluating, and deploying models safely. Third party assessments are a critical part of balancing that responsibility, expanding opportunities for input on AI safety, keeping the world informed, and keeping labs accountable to clear and independently supported safety claims.

As part of our efforts to [pace the frontier](https://openai.com/index/an-alien-mind/), OpenAI is committed to supporting independent assessments with deep levels of access across training, evaluation, and deployment. That access should enable assessors to challenge our assumptions, identify risks we may have missed, and reach their own conclusions about the effectiveness of our safeguards.

We have long worked with [third party assessors](https://openai.com/index/strengthening-safety-with-external-testing/) at various stages of the model development and deployment process. We have also incorporated third party assessments into our [Preparedness Framework](https://openai.com/index/updating-our-preparedness-framework/) practices and [supported organizations and legislation](https://openai.com/index/frontier-safety-blueprint/) that advocate for a more rigorous and accountable process. Throughout these engagements, we have provided deep forms of access, including information about our technical safeguards, visible chain of thought access, and unprecedented levels of confidential data and internal deployment access for incident response and monitor red teaming. The priorities and principles shared here focus on our engagement with independent assessment organizations in the private and non-profit sector on technical safety assessments. They complement our work with governments on testing and evaluation, where distinct roles and responsibilities may call for different approaches.

Making these assessments effective requires strong independence mechanisms, scientific rigor, robust security practices, and clear responsibilities. Labs have a responsibility to enable meaningful scrutiny while protecting sensitive information. Labs and independent assessors share the responsibility for getting this right, and should be operating with [shared international standards for safety and security practices](https://openai.com/index/building-standards-next-phase-ai/). Below, we propose four priority areas for deeper assessment, alongside principles for rigorous, secure, and independent work.

## Priority areas for assessment

Third party assessments are most useful when they address specific, consequential questions: Does the evidence support a lab’s safety case and safety claims? Do evaluations adequately test the risks they are intended to measure? Do safeguards work under realistic conditions?

The assessments described here are intended to take different forms depending on the safety questions being examined. We expect to support multiple assessments in parallel and over different periods of time, with some lasting weeks and others several months. While third-party assessments can also be part of pre-deployment work and may inform deployment decisions, the work described here is generally longer-term and launch-agnostic—focused on examining particular safety claims in depth over time.

Throughout our priority areas and principles, we refer to safety claims and safety cases. What we mean by these terms is the following:

We propose four priority areas for deeper assessment, alongside principles for rigorous, secure, and independent work.

1. **Independent assessment of safety cases, spanning training, evaluation, internal deployment and external deployment.**

   Assessment of safety cases requires expertise in alignment, control methods such as monitoring, cybersecurity, biological and chemical misuse and red teaming. Safety cases consist of claims including training, capability evaluations, and safeguards, which can be assessed as a whole or in parts (see priorities 2 and 3 below). Multiple assessors will likely need to examine different parts of the cases, drawing on their respective expertise. Together, their assessments should answer questions such as:

   1. Is the evidence for safety cases for training, evaluation, and deployments substantiated? Were the conditions of the safety case followed during training, evaluation, and deployment?

   2. Do our safety cases cover the most urgent risks identified in the course of an assessment? Do the safety claims support the overall safety case? Are there any gaps or areas for improvement?

   3. Are effective methods used to identify and reduce incentives in training that could reward deception, reward hacking, destructive actions, or circumventing restrictions?
2. **Assessment of critical safeguards, across internal and external deployments**

   Our safeguard stack is always evolving to meet the changing capabilities and landscape. Our safeguards span training, internal deployment, and external deployment. They currently include model-level safeguards, enforcement safeguards, and security safeguards, as well as misalignment monitors—covering a wide variety of risks (such as loss of control, and misuse in cyber, biological and chemical risks). Technical partnerships can identify weaknesses in safeguards now while improving assessment methods and accelerating standards development, providing a stronger technical basis for future public policies to pace the frontier—particularly for internal deployments, where safety and security standards are still nascent.

   Independent assessments should examine how well these safeguards work and where they may fall short, such as:

   1. Using “grey box” access, are safeguards robust to adversarial testing (jailbreaks) and do they sufficiently protect against capability uplift in high risk domains (e.g, cyber, bio)? Does our adversarial testing and red teaming cover the most important risks?

   2. In authorized testing under realistic operating conditions, how do agents interact with cyber defenses such as access controls, sandboxing, and detection and response systems? Which defenses prevent, detect, or contain harmful actions, and where do they fail?

   3. Do our misalignment monitors have any critical gaps that could lead to loss of control or severe misalignment, for both internal and external deployments? How reliable is chain-of-thought monitoring as a source of evidence for safety or alignment as model capabilities improve?

   4. Is appropriate monitoring implemented across all relevant training, evaluations, and deployment, in a way that cannot easily be disabled?

   5. Are safeguards implemented commensurate with the capabilities?
3. **Assessment of capability evaluations that cover Preparedness risk categories (Chemical and Biological Risks, Cybersecurity, AI Self-Improvement) and alignment evaluations for misalignment risks**

   Our Preparedness Framework requires evaluations to assess key frontier risk areas. As thresholds are surpassed and evaluations saturate, it is important to consistently refresh and ensure coverage and quality of evaluations that assess capabilities in Preparedness risk areas, and alignment evaluations that seek to assess severe misalignment risks.

   Relevant questions include:

   1. Do evaluations that assess Preparedness risks adequately cover the Preparedness risk threshold definition? Are thresholds set correctly for these evaluations?

   2. Are evaluations updated when models consistently achieve the highest scores, and do the new tests meaningfully measure more advanced capabilities?

   3. Do our alignment evaluations adequately cover severe misalignment risks, and what important behaviors or conditions might they miss?
4. **Independent investigation of critical misalignment incidents**

   Independent investigation can be valuable across a range of critical AI safety incidents. Here, we focus on model misalignment, including models acting without authorization or evading oversight, which can reveal weaknesses in alignment methods and safeguards even without intentional misuse.

   In select circumstances, as with the OpenAI Hugging Face incident, it can be beneficial to bring in an independent third party for independent [misalignment incident investigations](https://openai.com/index/model-misalignment-reporting-framework/). It is critical that third parties involved in incident investigation have the requisite expertise for investigation, including where applicable: cyber forensics expertise, alignment expertise, large-scale chain of thought analysis, and the staff and resources available to conduct investigation in a timely manner. Incident response may involve access to sensitive internal data and third party data, and as a result some details may be sensitive to publish or access. Findings from independent investigations can also inform a model’s safety case by providing evidence to assess claims about its alignment and the effectiveness of measures taken to remediate previously observed misalignment.

   In the context of misalignment incidents, we prioritize independent assessments on:

   1. What model behavior or misalignment issues occurred in the course of the incident, and what are the primary contributing factors?

   2. Would safeguards and remediation effectively mitigate similar incidents in the future?

## Principles for effective assessments

- **Clearly scoped and mutually agreed upon claims for assessment:** Assessments should begin with a mutually agreed-upon scope, followed by clearly defined safety claims that are pre-registered before assessment activities begin. Third parties and labs should clarify whether they are claims that the company being evaluated wants to put forward for assessment, or claims that the third party assessor is aiming to assess independently and the lab agrees to be assessed against. There are many reasons some claims may be out of scope, including infeasibility for third parties to access data, insufficient expertise from the assessor, or reasonable time constraints when an assessment is urgent. Labs and assessors should establish a process for considering significant risks identified outside the original scope, including whether further investigation is warranted. Conclusions should make clear what was and was not assessed as it relates to the mutually agreed upon scope.

- **Proportionate access:** Assessors should have proportionate access to assess the agreed upon claims where possible within the bounds of legal, security, and IP constraints. Where direct access is impractical or not possible, assessors can work with a designated representative at a company or explore indirect or privacy preserving access mechanisms to protect underlying information.

- **Transparent methodology and standards:** Assessors should explain their methods, assessment criteria, and uncertainties, drawing on established standards where available. Where standards do not yet exist, they should clearly justify the criteria they use. Reports should distinguish direct findings from interpretation, explain uncertainty, and make clear which conclusions the evidence supports.

- **Expertise and independence:** Assessors should demonstrate relevant technical expertise and identify, disclose, and address organizational and individual conflicts of interest, including financial incentives, relationships with developers, and prior involvement in the work being assessed. Safeguards should be designed to ensure that commercial pressures and compensation arrangements do not influence findings, and may include recusal or appropriate exclusion periods.

- **Security and confidentiality:** Assessors should demonstrate information-security practices and enforceable confidentiality protections covering their personnel, proportionate to the sensitivity of the systems and information accessed. These protections should cover intellectual property, sensitive information, and assessment records. Where assessors cannot meet security requirements in their own environments, or the data being accessed is especially sensitive, access on company-managed devices or premises may be appropriate.

- **Actionable findings and time to remediate:** Assessments should identify specific gaps and provide enough detail for labs to address them. Where appropriate, labs should have a reasonable period to remediate issues before publication. Where relevant, reports should also explain lessons for agent developers, deployers, and defenders, with practical recommendations for implementation.

- **Responsible publication practices:** Assessment reports should be grounded in evidence and shared as openly as possible while protecting sensitive and confidential information. Where full public disclosure is not possible, confidential reporting to appropriate oversight bodies (such as governance bodies or boards of companies) can support accountability. Principled redaction protections and policies, as well as clear expectations about feedback and correction of inaccuracies, should be in place to preserve the balance of independence and confidentiality. Assessors should maintain editorial independence, while ensuring that confidentiality and IP protections are maintained. Assessors should adopt clear redaction policies that allow for labs to request redactions of sensitive information, while assessors can note where substantive redactions have been made and the impact on their assessment report.

## The road ahead

We are committed to supporting independent assessors and establishing clearer, shared international standards—both through future laws and private governance institutions—for effective third party assessments. While the independent evaluation ecosystem is still growing, we will help it grow by supporting and working with a diverse community of independent assessors with deep expertise across frontier safety questions. No one third party can or should comprehensively cover urgent frontier safety questions. We will move deliberately and with intention to grow our capacity and enable the growing third party ecosystem to align on the best practices and principles. We are in conversation with multiple third parties about proposals that align with the priority areas above.
