---
title: Introducing Devin Security Swarm
link: https://cognition.com/blog/introducing-devin-security-swarm
source: cognition-com-blog
published: 2026-07-01T17:00:00Z
first_seen: 2026-09-04T12:14:41.802665207Z
content: extracted
html: 2026-07-01-introducing-devin-security-swarm.html
---

By The Cognition Team07.01.26

As AI code production accelerates, security teams are facing a growing pile of findings they can't act on. Some teams are seeing 10–100x more security findings—and many are false positives.

Security teams are stuck with tools that haven't caught up. Scanners offer coverage at scale but miss critical exploits because they can't reason about business logic and discover chained vulnerability attacks. AI security tools often struggle to reason effectively about the whole codebase. None of them can validate which findings are actually exploitable or write a fix.

Devin Security Swarm brings engineering capabilities to security teams so they can ship fixes themselves. It finds vulnerabilities across the codebase, validates that they are exploitable at runtime, and ships remediation PRs. Security Swarm finds more verified vulnerabilities at 30% lower cost than the nearest comparable alternative.

## How it works

A swarm of parallel agents investigate segments of the codebase. Each agent reasons across files—catching business logic flaws, chained auth bypasses, and cross-service exploit paths. Devin then composes individual findings into full attack paths and reproduces each one in an isolated sandbox to confirm exploitability at runtime.

What reaches the security team is a set of confirmed-exploitable vulnerabilities with attack paths and steps to reproduce. After Devin confirms a vulnerability, it writes the patch and opens a PR for review.

For the full technical breakdown, read our blog post on the [Agentic MapReduce architecture](https://devin.ai/blog/agentic-map-reduce).

## Performance

We evaluated Devin Security Swarm on a benchmark of 50 real-world vulnerabilities, each tied to a published GitHub Security Advisory (GHSA) across repositories in Go, Python, JavaScript, Rust, Ruby, C#, Java, Swift, PHP, Elixir, Erlang, C, Kotlin, and Dart.

HarnessRecall$/Run

Devin Security

72%

$90.23

Claude Security

68%

$131.87

Codex Security

48%

$118.20

Cursor Security

26%

$4.60

Only Devin found three critical vulnerabilities that other tools missed: a PHP sandbox bypass via template injection, an argument injection through metadata value parsing, and an overly broad deserialization surface in Spring Kafka.

[Read more about our evaluation methodology here.](https://devin.ai/blog/security-swarm-eval)

## Scan profiles

Devin can generate scan profiles directly from your existing threat model documentation, tailor them to specific attacker personas, and apply them across your entire organization without per-repo configuration or CI setup. Batch size is configurable per profile, giving you direct control over depth and cost.

Scans run on a daily, weekly, or custom schedule. The first full scan establishes a baseline across your codebase. Subsequent scans process only code that changed since the last run, so cost decreases over time.

## How to get started

Devin Security Swarm is already used by major companies for regular security scanning, and it's available starting today.

For enterprises that want more guidance, the Devin Security Vulnerability Remediation Program is a six-week engagement to help organizations reduce their vulnerability backlog and set up ongoing remediation. Cognition's forward-deployed engineering team embeds with yours. First, Devin burns down your CVE backlog. After that, Security Swarm is set up to continuously find and fix vulnerabilities.

[Talk to sales](https://cognition.com/demo)
