---
title: eMudhra Intermediate CA Compromise
link: https://simonwillison.net/2026/Oct/20/emudhra-certificates/
source: simonwillison-net
published: 2026-10-20T23:50:25Z
updated: 2026-10-20T23:50:25Z
first_seen: 2026-10-21T00:05:10.170627791Z
labels:
- encryption
- https
- pip
- python
- security
- uv
summary: 'eMudhra Intermediate CA Compromise This is very, very bad news: The private keys of eight eMudhra intermediate CA certificates have been compromised. They chain to roots trusted by the Mozilla, Microsoft, Apple and Chrome root programs and by many downstream trust stores. Any software that bundles a list of trusted certificates needs to be updated. This site is tracking software updates via this GitHub repository - Linux distros need to update their ca-certificates packages. In Python world uv 0.13.4 and pip 26.2.2 and certifi 2026.10.19 have been released with upgraded certificate lists. Tags: encryption, https, pip, python, security, uv'
content: extracted
html: 2026-10-20-emudhra-intermediate-ca-compromise.html
---

**[eMudhra Intermediate CA Compromise](https://emudhra.we-have-a-problem.com)**. This is very, very bad news:

> The private keys of eight eMudhra intermediate CA certificates have been compromised. They chain to roots trusted by the Mozilla, Microsoft, Apple and Chrome root programs and by many downstream trust stores.

Any software that bundles a list of trusted certificates needs to be updated. This site is tracking software updates [via this GitHub repository](https://github.com/galadran/we-have-a-problem.com) - Linux distros need to update their `ca-certificates` packages.

In Python world [uv 0.13.4](https://github.com/astral-sh/uv/releases/tag/0.13.4) and [pip 26.2.2](https://github.com/pypa/pip/releases/tag/26.2.2) and [certifi 2026.10.19](https://github.com/certifi/python-certifi/releases/tag/2026.10.19) have been released with upgraded certificate lists.
