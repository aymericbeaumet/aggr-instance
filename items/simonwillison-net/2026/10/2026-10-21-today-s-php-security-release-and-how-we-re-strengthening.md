---
title: Today's PHP Security Release, and How We're Strengthening PHP Security
link: https://simonwillison.net/2026/Oct/21/php-security-release/
source: simonwillison-net
published: 2026-10-21T18:41:53Z
updated: 2026-10-21T18:41:53Z
first_seen: 2026-10-21T19:14:42.600331207Z
labels:
- ai
- ai-security-research
- open-source
- php
- security
summary: 'Today''s PHP Security Release, and How We''re Strengthening PHP Security If you''re running anything on PHP it''s vital you upgrade - today''s updates (across the entire PHP 8.x line) address two remote code execution holes, and there have been exploits circulating since last night, thanks to a leak in the coordinated disclosure process. This post from the PHP Foundation explain what went so wrong: On Tuesday, October 20th, at around 15:00 UTC, the two CVE IDs began circulating on online forums and social media. They came with a wrong release time and a wrong CVSS score of 10.0 (the actual score is 9.2). No technical details leaked. In response, the PHP project brought its pre-announcement forward and published it at 19:30 UTC. It also moved the release from October 22nd, 14:00 UTC, to October 21st, 14:00 UTC [...] As a result of the leak and pre-announcement around 20:00 UTC, Calif, one of the security firms that had reported the issues mistakenly published a video. It showed their proof of concept working against a number of targets and revealed a few additional, though limited, details about the vulnerabilities. Copies of the deleted video started circulating on social media soon after. The video contributed to accelerated development of various PoCs (exploitation proof of concepts), which then increased circulation on October 21st. This article from the team that discovered the original vulnerability includes a more detailed timeline of the original intended responsible disclosure: August 19, 2026: Wiz Research discovers the first PHP vulnerability using Wiz Atlas, Gemini 4 Argon, and GPT-5.6 Sol. August 20, 2026: Unauthenticated RCE is confirmed against Drupal. August 21, 2026: Unauthenticated RCE is confirmed against WordPress. August 24, 2026: Wiz Research reports the vulnerabilities to the PHP security team. September 21, 2026: PHP acknowledges the reports and begins developing a coordinated fix. October 21, 2026: Patched PHP releases become available. October 21, 2026: Public disclosure. Via Hacker News Tags: open-source, php, security, ai, ai-security-research'
content: extracted
html: 2026-10-21-today-s-php-security-release-and-how-we-re-strengthening.html
---

**[Today's PHP Security Release, and How We're Strengthening PHP Security](https://thephp.foundation/blog/2026/10/21/todays-php-security-release-and-how-were-strengthening-php-security/)** ([via](https://news.ycombinator.com/item?id=50182987 "Hacker News")) If you're running anything on PHP it's vital you upgrade - today's updates (across the entire PHP 8.x line) address two remote code execution holes, and there have been exploits circulating since last night, thanks to a leak in the coordinated disclosure process.

This post from the PHP Foundation explain what went so wrong:

> On Tuesday, October 20th, at around 15:00 UTC, the two CVE IDs began circulating on online forums and social media. They came with a wrong release time and a wrong CVSS score of 10.0 (the actual score is 9.2). No technical details leaked. In response, the PHP project brought its pre-announcement forward and [published it at 19:30 UTC](https://www.php.net/archive/2026.php#2026-10-20-1). It also moved the release from October 22nd, 14:00 UTC, to October 21st, 14:00 UTC \[...\]
>
> As a result of the leak and pre-announcement around 20:00 UTC, Calif, one of the security firms that had reported the issues mistakenly published a video. It showed their proof of concept working against a number of targets and revealed a few additional, though limited, details about the vulnerabilities. Copies of the deleted video started circulating on social media soon after. The video contributed to accelerated development of various PoCs (exploitation proof of concepts), which then increased circulation on October 21st.

[This article](https://www.wiz.io/blog/php2shell-php-rce) from the team that discovered the original vulnerability includes a more detailed timeline of the original intended responsible disclosure:

> - **August 19, 2026:** Wiz Research discovers the first PHP vulnerability using Wiz Atlas, Gemini 4 Argon, and GPT-5.6 Sol.
> - **August 20, 2026:** Unauthenticated RCE is confirmed against Drupal.
> - **August 21, 2026:** Unauthenticated RCE is confirmed against WordPress.
> - **August 24, 2026:** Wiz Research reports the vulnerabilities to the PHP security team.
> - **September 21, 2026:** PHP acknowledges the reports and begins developing a coordinated fix.
> - **October 21, 2026:** Patched PHP releases become available.
> - **October 21, 2026:** Public disclosure.
