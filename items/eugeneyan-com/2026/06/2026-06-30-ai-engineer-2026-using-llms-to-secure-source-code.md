---
title: AI Engineer 2026 - Using LLMs to Secure Source Code
link: https://eugeneyan.com/speaking/aie-2026
source: eugeneyan-com
published: 2026-06-30T00:00:00Z
updated: 2026-06-30T00:00:00Z
first_seen: 2026-11-14T09:19:50.800387300Z
summary: Why now, six steps to an agentic harness for security, and the org bottlenecks that follow.
content: extracted
html: 2026-06-30-ai-engineer-2026-using-llms-to-secure-source-code.html
remote_preview:
  url: https://eugeneyan.com/assets/og_image/aie-2026.jpg
---

I spoke at the AI Engineer World’s Fair 2026 in San Francisco on using LLMs to secure source code: why now, six steps to building your own agentic harness for security, and the org bottlenecks that show up once finding vulnerabilities gets easy.

Here are the slides. The [AI Engineer talk page](https://ai.engineer/talks/imFedndyXYQ-using-llms-secure-source-code) has the recording, chapters, and transcript, and the [write-up](https://claude.com/blog/using-llms-to-secure-source-code) has the field lessons behind the talk.

![Title slide: Using LLMs to Secure Source Code, by Eugene Yan of Anthropic, at the AI Engineer World’s Fair 2026](https://eugeneyan.com/img/aie-2026-01_hu_1dc73c59c10b6ce2.webp)

![What we’ll cover: why now, how to work with models for security and the potential bottlenecks, and how to get started](https://eugeneyan.com/img/aie-2026-02_hu_d06af3f53c664c07.webp)

![Section I: Why now. Three trends: model capability, vulns found, and a shifting bottleneck](https://eugeneyan.com/img/aie-2026-03_hu_4af22d867897bbeb.webp)

![Model capability on cyber tasks is doubling every ~5 months](https://eugeneyan.com/img/aie-2026-04_hu_b6b8108842b6101f.webp)

![Mozilla found more vulns in April 2026 than in all of 2025](https://eugeneyan.com/img/aie-2026-05_hu_edbb5a04c98080c0.webp)

![Notable public vulns and exploits: Log4Shell and Heartbleed](https://eugeneyan.com/img/aie-2026-06_hu_e96b8de1b9bd344c.webp)

![The new bottleneck: from finding vulns to verifying, triaging, and patching them](https://eugeneyan.com/img/aie-2026-07_hu_5ee3aa8fc9bcea43.webp)

![Section II: Six simple steps to building your own agentic harness for security](https://eugeneyan.com/img/aie-2026-08_hu_e40918eacc2fd7ff.webp)

![Why this works now: agentic harnesses](https://eugeneyan.com/img/aie-2026-09_hu_24f549175829b39c.webp)

![The six steps: threat model and sandbox as setup, then discovery, verification, triage, and patching as a cycle](https://eugeneyan.com/img/aie-2026-10_hu_d43d19a90e4a9816.webp)

![Running example: meet order-service](https://eugeneyan.com/img/aie-2026-11_hu_f6a7ea379c278c27.webp)

![Threat model: decide what counts as a vuln before starting](https://eugeneyan.com/img/aie-2026-12_hu_567971d26bd10f4f.webp)

![Example: a threat model bootstrapped from the codebase](https://eugeneyan.com/img/aie-2026-13_hu_5885f3b24fb58f2f.webp)

![Sandbox: isolate the agent and make runs reproducible](https://eugeneyan.com/img/aie-2026-14_hu_23d56ba6523df8fa.webp)

![Example: the sandbox holds the target stack, plus a security agent probing it](https://eugeneyan.com/img/aie-2026-15_hu_c6ecc810e5b829e9.webp)

![Discovery: rich context, shorter prompts, useful tools](https://eugeneyan.com/img/aie-2026-16_hu_7ff7dd8269277ff2.webp)

![Example: spot the vulnerability](https://eugeneyan.com/img/aie-2026-17_hu_b495fc5371589bea.webp)

![Example: one vulnerability the discovery agent found, a SQL injection](https://eugeneyan.com/img/aie-2026-18_hu_8861e216683152c7.webp)

![Verification: independently confirm what’s exploitable](https://eugeneyan.com/img/aie-2026-19_hu_14a6a51a64003552.webp)

![Example: the proof of concept successfully exfiltrates user data](https://eugeneyan.com/img/aie-2026-20_hu_64f790b5a730c394.webp)

![Triage: dedupe by root cause, rank by preconditions](https://eugeneyan.com/img/aie-2026-21_hu_a1fbedd0ab75020f.webp)

![Example: compensating controls downgrade a finding from High to Low](https://eugeneyan.com/img/aie-2026-22_hu_f4ebeedbbb71b59.webp)

![Patching: fix, validate, hunt variants, close the loop](https://eugeneyan.com/img/aie-2026-23_hu_482a6d1767747e59.webp)

![Example: patch the code, then close the loop on the threat model](https://eugeneyan.com/img/aie-2026-24_hu_cd78cfae944c3599.webp)

![What each of the six steps focuses on](https://eugeneyan.com/img/aie-2026-25_hu_a863c2c17bb46b5d.webp)

![Section III: Yet another bottleneck, org processes](https://eugeneyan.com/img/aie-2026-26_hu_d4a55338e792c272.webp)

![What happens when you find one to two orders of magnitude more vulns?](https://eugeneyan.com/img/aie-2026-27_hu_942d536d2c66018c.webp)

![Organizational bottlenecks you might hit, and what to do](https://eugeneyan.com/img/aie-2026-28_hu_61601c519739ed09.webp)

![Section IV: If you only remember one thing](https://eugeneyan.com/img/aie-2026-29_hu_b7010a8c49c52a6e.webp)

![Start now, and here’s how](https://eugeneyan.com/img/aie-2026-30_hu_ad75f0bcf07a392e.webp)

## Resources

- [Claude Security](https://claude.com/product/claude-security): scans your codebase, validates findings, and suggests patches
- [Using LLMs to secure source code](https://claude.com/blog/using-llms-to-secure-source-code): field lessons from teams running this in production
- [defending-code-reference-harness](https://github.com/anthropics/defending-code-reference-harness): interactive skills and an autonomous harness
- [claude-code-security-review](https://github.com/anthropics/claude-code-security-review): a GitHub Action for AI security review on pull requests
