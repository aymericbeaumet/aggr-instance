---
title: GPT-6.1 Sol helps CodeRabbit strengthen security across complex codebases
link: https://openai.com/index/gpt-6-1-sol-coderabbit-code-security
source: openai-com
published: 2026-10-26T13:45:00Z
updated: 2026-10-26T13:45:00Z
first_seen: 2026-10-26T23:45:44.074426996Z
labels:
- security
summary: CodeRabbit uses GPT-6.1 Sol to investigate security risks across complex codebases. Its security agent helps developers review findings and proposed fixes.
content: extracted
html: 2026-10-26-gpt-6-1-sol-helps-coderabbit-strengthen-security-across.html
---

As AI accelerates software development, engineering and application-security teams need ways to investigate code changes and address vulnerabilities while keeping people in control. CodeRabbit is using [GPT‑6.1 Sol](https://openai.com/index/introducing-gpt-6-1-sol/) as the primary model in its security agent to extend review beyond individual changes and help developers understand risks across complex codebases.

A pull request can affect shared libraries, dependencies and safeguards elsewhere in an application. GPT‑6.1 Sol helps CodeRabbit connect behavior across files and components to surface potential security issues. CodeRabbit’s review is independent of the agent that wrote the code and complements established scanners. Its system also uses other frontier models and security tooling.

During pull-request review, Sol helps analyze changes as developers work. In Deep Scan, it supports broader investigation across repositories, including dependencies and how components interact. CodeRabbit Secure extends that investigation to linked repositories, such as a shared authentication library, to help teams understand how a weakness could affect the wider system.

CodeRabbit reports using GPT‑6.1 Sol to flag security-related issues across thousands of repositories and organizations. These findings feed into the pull-request workflows developers already use to review and improve their code.

CodeRabbit Secure connects investigation to remediation. For user-selected findings, it can attempt exploit verification in an isolated sandbox without testing production systems. For supported issues, it can draft a pull request with a proposed fix for a person to review and approve.

This work shows how frontier reasoning can support software security: understanding risks beyond an individual change, helping validate findings and turning them into proposed fixes. Human reviewers remain responsible for deciding which changes to ship.

Learn more about GPT‑6.1 Sol [here](https://openai.com/index/introducing-gpt-6-1-sol/). Learn more about CodeRabbit Secure [here](https://www.coderabbit.ai/security).
