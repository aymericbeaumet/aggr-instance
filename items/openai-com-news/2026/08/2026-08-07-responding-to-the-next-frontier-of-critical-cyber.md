---
title: Responding to the next frontier of critical cyber capabilities
link: https://openai.com/index/responding-next-frontier-critical-cyber-capabilities
source: openai-com-news
published: 2026-08-07T15:20:00Z
updated: 2026-08-07T15:20:00Z
first_seen: 2026-09-07T17:03:44.343711062Z
labels:
- security
summary: OpenAI is sharing preliminary cybersecurity evaluations for Astra and the steps we’re taking to strengthen safeguards and security controls.
content: extracted
html: 2026-08-07-responding-to-the-next-frontier-of-critical-cyber.html
preview:
  file: 2026-08-07-responding-to-the-next-frontier-of-critical-cyber.preview-5214d799d1f5.webp
  width: 256
  height: 144
  alt: Blue and lime gradient social image with the text “The next frontier of critical cyber capabilities.”
  color: '#afd9ae'
images:
- source: https://images.ctfassets.net/kftzwdyauwt9/2OqBX7eh31rmzCVTZrZgb6/8c852b5a1f787c1fea8ac9de5a33d2db/Frame.png?w=1600&h=900&fit=fill
  original:
    file: 2026-08-07-responding-to-the-next-frontier-of-critical-cyber.image-f52a502b77c5.png
    width: 1600
    height: 900
  variants:
  - file: 2026-08-07-responding-to-the-next-frontier-of-critical-cyber.image-1ce56e576732.webp
    width: 320
    height: 180
  - file: 2026-08-07-responding-to-the-next-frontier-of-critical-cyber.image-5a9f9bd0d879.webp
    width: 640
    height: 360
  - file: 2026-08-07-responding-to-the-next-frontier-of-critical-cyber.image-069b6aa73102.webp
    width: 960
    height: 540
  - file: 2026-08-07-responding-to-the-next-frontier-of-critical-cyber.image-a0f068f1322e.webp
    width: 1280
    height: 720
  - file: 2026-08-07-responding-to-the-next-frontier-of-critical-cyber.image-d61adfb2cced.webp
    width: 1600
    height: 900
  color: '#a6daf7'
---

Cybersecurity is rapidly changing as models become more capable in ways that can both strengthen cyberdefenses and enable attacks at unprecedented speed and scale.

Our latest internal evaluations of Astra, one of our upcoming models, over the past few days indicate significant advancements in agentic coding and cybersecurity. These results, in addition to expert assessments, have led us to conclude last night that we cannot rule out critical cyber capabilities under our [Preparedness Framework](https://cdn.openai.com/pdf/18a02b5d-6b67-4cec-ab64-68cdfbddebcd/preparedness-framework-v2.pdf) .

We are sharing this because we believe it’s important to be transparent with the public and the safety and security communities about this potential shift in capabilities.

We first published our Preparedness Framework in December 2023, well before models approached biological, chemical, cybersecurity, and AI self-improvement capabilities at this level. We created it to give us a guide for identifying progress in capability and then planning what our company would do as those capabilities emerge. Previous models, including GPT‑5.6‑Sol, have been evaluated for frontier cyber capabilities and assessed at the High (rather than Critical) threshold.

## Measuring critical cybersecurity capabilities

Under our Preparedness Framework, a model reaches the Critical cybersecurity threshold if it can identify and develop functional zero-day exploits of all severity levels in many hardened real-world critical systems without human intervention, or can devise and execute end-to-end novel strategies for cyberattacks against hardened targets given only a high level desired goal.

While we continue to benchmark and assess this model, our preliminary evaluations indicate strong enough performance that we cannot rule out Critical capability level at this time. Astra is an upcoming model, and was not involved in exploiting Hugging Face.

## Steps we are taking

Accordingly, we have scaled up robustness testing of our safeguards and security controls so that they are appropriate for a deployment of these capabilities. Internally, we have also taken the following steps so that further development of this model happens safely and securely:

- We are implementing stricter security controls for higher-capability models and associated activities, including isolated testing environments, restricted network and tool access, enhanced model weight protections and encryption, additional monitoring and detection capabilities, and sandboxed execution.

- We are pausing internal activities involving Astra that do not yet meet these strengthened security control requirements.

- We have implemented universal monitoring for risky actions and misalignment across all agentic applications of Astra, including training and evaluation. Monitors evaluate the model's Chain of Thought and trigger a security response to review and interrupt high risk activity.

- We will work with relevant government agencies and select AI safety organizations to test the capabilities for this model.

- We will be providing recommended security controls to third-party testing partners for running higher risk evaluations and workloads safely.

The framework has already guided us through other capability transitions. In June 2025, as our models approached the high capability threshold for biology under the Preparedness Framework, we [outlined the steps we were taking](https://openai.com/index/preparing-for-future-ai-capabilities-in-biology/) to strengthen safeguards, expand testing, work with external experts, and deploy additional security controls. We are applying the same principle here.

We believe advanced cyber-capable models should help [defenders](https://openai.com/daybreak/) identify and address vulnerabilities before attackers do. We’re committed to working alongside governments, safety institutes, and civil society to ensure that the frontier capabilities of models like Astra, and those that follow, are deployed responsibly and broadly for the benefit of all humanity.
