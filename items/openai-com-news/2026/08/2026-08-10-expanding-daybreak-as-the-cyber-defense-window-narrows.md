---
title: Expanding Daybreak as the Cyber Defense Window Narrows
link: https://openai.com/index/expanding-daybreak-as-the-cyber-defense-window-narrows
source: openai-com-news
published: 2026-08-10T10:00:00Z
updated: 2026-08-10T10:00:00Z
first_seen: 2026-09-07T17:03:44.343711062Z
labels:
- security
summary: Meet GPT-5.6-Cyber, OpenAI’s cybersecurity-specific model available through Daybreak Red for authorized vulnerability research, exploit validation, and security testing.
content: extracted
html: 2026-08-10-expanding-daybreak-as-the-cyber-defense-window-narrows.html
preview:
  file: 2026-08-10-expanding-daybreak-as-the-cyber-defense-window-narrows.preview-f19ff319164a.webp
  width: 256
  height: 144
  color: '#18110d'
images:
- source: https://images.ctfassets.net/kftzwdyauwt9/126iXrjsNVMlUR3pgI1PbS/80445f60ec21f0b051acb893fdf5dac4/16x9_SEO.png?w=1600&h=900&fit=fill
  original:
    file: 2026-08-10-expanding-daybreak-as-the-cyber-defense-window-narrows.image-59f8dab7c15b.png
    width: 1600
    height: 900
  color: '#020201'
---

The cybersecurity world is rapidly changing—threat actors will increasingly use AI to conduct cyberattacks at unprecedented speed and scale, including in fully autonomous ways. As these capabilities spread, defenders have a narrowing window to prepare. Our answer is to put frontier intelligence in the hands of trusted defenders everywhere before attackers deploy offensive AI capabilities at scale.

We’re expanding OpenAI Daybreak with two access tiers designed to give approved defenders the right capabilities for their work:

- **Daybreak Blue** provides access to frontier general-purpose models, including GPT‑5.6 Sol, with safeguards tailored to authorized defensive security work. It is the recommended starting point for most defenders, supporting vulnerability discovery, secure code review, malware analysis, incident response, and patch validation.

- **Daybreak Red** provides access to our purpose-trained cybersecurity models for authorized vulnerability research, exploit validation, and security testing.

We’re also introducing GPT‑5.6‑Cyber, available through Daybreak Red. Built on GPT‑5.6 Sol, it is trained to improve capabilities on several specialized cybersecurity tasks (e.g., finding zero-day vulnerabilities and developing exploit chains) and to reduce refusals for certain higher-risk, dual-use cyber tasks.

### Daybreak unlocks advanced cyber capabilities

As we [previously shared](https://openai.com/index/gpt-5-6/) , GPT‑5.6 Sol delivers state-of-the-art performance on cybersecurity tasks. In production, we deploy system-level safeguards to screen cybersecurity-related requests to prevent misuse, but they can also block legitimate defensive work. Daybreak Blue access removes those guardrails, helping defenders get more out of the model in real-world security tasks, including incident detection and response, investigations, vulnerability management, and security assessments.

Even without system-level guardrails, there are still highly dual-use cybersecurity prompts (e.g., pentesting production systems) where GPT‑5.6 Sol will refuse to comply. To address this, we trained GPT‑5.6‑Cyber, available through Daybreak Red access, to further reduce refusals and improve performance on certain tasks. GPT‑5.6‑Cyber helps trusted defenders conduct legitimate security activities.

To measure the reduced rate of refusals that is provided by GPT‑5.6‑Cyber through Daybreak Red access, we created an internal evaluation (Advanced Cybersecurity Completion Rate) that measures how often models will respond to requests involving exploit-chain development, authentication bypass, privilege escalation, and other advanced cybersecurity scenarios1. GPT‑5.6‑Cyber completes 95.0% of these requests, compared with just 1.5% for GPT‑5.6 Sol, and 2.0% when used with Daybreak Blue access. It also completes more requests than GPT‑5.5‑Cyber, which completes only 57.3% of requests, addressing feedback from security researchers who encountered persistent refusals with the earlier model.

Below we show a series of cybersecurity prompts and the associated model responses from GPT‑5.6 Sol with system-level guardrails, GPT‑5.6 Sol (Daybreak Blue), GPT‑5.5‑Cyber (Daybreak Red), and GPT‑5.6‑Cyber (Daybreak Red).
