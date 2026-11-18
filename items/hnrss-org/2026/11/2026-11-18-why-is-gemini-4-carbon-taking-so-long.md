---
title: Why is Gemini 4 Carbon taking so long?
link: https://kernel.dev/blog/why-is-gemini-4-carbon-taking-so-long/
source: hnrss-org
published: 2026-11-18T04:14:04Z
updated: 2026-11-18T04:14:04Z
first_seen: 2026-11-18T17:57:41.978411823Z
authors:
- deenadz
content: extracted
html: 2026-11-18-why-is-gemini-4-carbon-taking-so-long.html
remote_preview:
  url: https://kernel.dev/blog/why-is-gemini-4-carbon-taking-so-long/og.png
  alt: Why is Gemini 4 Carbon taking so long? Kernel, November 2026.
---

### Sample trial: time to find the cause vs. time to first edit

Minutes from the start of each run. GPT-6 Astra at max effort, Gemini 4 Carbon at high.

In this sample trial, both models identified the cause early, Astra within a minute and Carbon within about six. Astra had its fix written three minutes later; Carbon didn't write any code for another 25 minutes.

Carbon might not have been sure it had the cause (a knowledge gap), or it might have known and kept going anyway (a general habit, learned from training that rewards right answers more than it penalises extra steps). To tell them apart, we re-ran one of the trials in a modified harness that automatically injects the same check-in every five turns: "do you already have enough to make the fix?"

### Sample trial with check-ins: time to find the cause vs. time to first edit

The same bug and scale as above. Gemini 4 Carbon at high, with the check-in injected every five turns (circles).

With check-ins, Carbon finished in 28 minutes instead of 37, and the wait between naming the cause and editing shrank from 25 minutes to about 10. This implies that the slowness seems to come from how Carbon behaves, not what it knows.
