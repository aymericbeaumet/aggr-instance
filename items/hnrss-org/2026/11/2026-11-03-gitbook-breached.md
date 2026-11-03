---
title: Gitbook Breached
link: https://news.ycombinator.com/item?id=50354336
source: hnrss-org
published: 2026-11-03T16:45:49Z
updated: 2026-11-03T16:45:49Z
first_seen: 2026-11-03T20:48:58.593592327Z
authors:
- punchmesan
summary: 'We opened a ticket with support about Gitbook availability issues and got this in reply. We have yet to see any official customer communication or breach notification. Apologies for the formatting, did the best I could as it''s a mobile paste. Update: GitBook security incident update Hey <$name>, The GitBook app, the GitBook API, and published docs are available again. A few services may still be degraded as we finish restoring everything to full performance. *What happened* GitBook has been the target of a sophisticated, highly targeted attack using AI agents. At 19:00 CET, November 2, the attacker gained unauthorized access to some of our infrastructure credentials. We detected the attack immediately and acted swiftly to block further impact. To make sure our infrastructure was fully secure, we then took the GitBook app and authenticated docs sites offline. We kept publicly published content live for as long as possible, using cached data. *What we did immediately* * Blocked the attacker and closed the vulnerabilities involved * Replaced every credential and secret across our infrastructure * Brought services back only once we were confident they were secure * Invalidated every customer’s API tokens on our platform *Your data* Our investigation shows that the attacker may have accessed the following: * Git Sync secrets. * GitBook API tokens. * Third-party API tokens used to connect external tools to GitBook. * Questions asked to GitBook Assistant by your site visitors. At this stage, we have found no evidence that any of your content in GitBook was modified. *What you need to do now* * Rotate any existing GitBook API tokens. We revoked all personal API tokens during the incident. * Rotate any third-party API tokens or secrets you use to connect other tools to GitBook. * If you have published integrations that use secrets, rotate those secrets. * If you use the GitBook MCP server, re-authenticate it. Please contact your GitBook organization’s admin to assist with this process. *What we are doing next* * Monitoring the situation in case of further attacks. * Introducing further security measures to our platform. * Writing a full post mortem that will be published soon with more detailed information about the incident. * If you have any further questions, please reach out to your account manager or our support team. You can also check status.gitbook.com for updates Thank you for your patience, and we apologize again for the disruption The GitBook team Comments URL: https://news.ycombinator.com/item?id=50354336 Points: 113 # Comments: 28'
content: extracted
html: 2026-11-03-gitbook-breached.html
---

We opened a ticket with support about Gitbook availability issues and got this in reply. We have yet to see any official customer communication or breach notification. Apologies for the formatting, did the best I could as it's a mobile paste.

Update:

GitBook security incident update Hey <$name>, The GitBook app, the GitBook API, and published docs are available again. A few services may still be degraded as we finish restoring everything to full performance.

\*What happened\* GitBook has been the target of a sophisticated, highly targeted attack using AI agents. At 19:00 CET, November 2, the attacker gained unauthorized access to some of our infrastructure credentials. We detected the attack immediately and acted swiftly to block further impact. To make sure our infrastructure was fully secure, we then took the GitBook app and authenticated docs sites offline. We kept publicly published content live for as long as possible, using cached data.

\*What we did immediately\* \* Blocked the attacker and closed the vulnerabilities involved \* Replaced every credential and secret across our infrastructure \* Brought services back only once we were confident they were secure \* Invalidated every customer’s API tokens on our platform

\*Your data\* Our investigation shows that the attacker may have accessed the following: \* Git Sync secrets. \* GitBook API tokens. \* Third-party API tokens used to connect external tools to GitBook. \* Questions asked to GitBook Assistant by your site visitors.

At this stage, we have found no evidence that any of your content in GitBook was modified.

\*What you need to do now\* \* Rotate any existing GitBook API tokens. We revoked all personal API tokens during the incident. \* Rotate any third-party API tokens or secrets you use to connect other tools to GitBook. \* If you have published integrations that use secrets, rotate those secrets. \* If you use the GitBook MCP server, re-authenticate it. Please contact your GitBook organization’s admin to assist with this process.

\*What we are doing next\* \* Monitoring the situation in case of further attacks. \* Introducing further security measures to our platform. \* Writing a full post mortem that will be published soon with more detailed information about the incident. \* If you have any further questions, please reach out to your account manager or our support team. You can also check status.gitbook.com for updates

Thank you for your patience, and we apologize again for the disruption The GitBook team
