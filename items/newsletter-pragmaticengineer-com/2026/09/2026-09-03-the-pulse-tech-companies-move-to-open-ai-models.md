---
title: 'The Pulse: tech companies move to open AI models'
link: https://newsletter.pragmaticengineer.com/p/the-pulse-tech-companies-move-to
source: newsletter-pragmaticengineer-com
published: 2026-09-03T17:00:14Z
updated: 2026-09-03T17:00:14Z
first_seen: 2026-09-19T21:30:23.395188495Z
authors:
- Gergely Orosz
summary: 'Cost-saving efforts reveal that moving simpler workloads to open AI models is the easiest way to save ~50% on AI bills. Also: automated software maintenance experience, and more'
content: extracted
html: 2026-09-03-the-pulse-tech-companies-move-to-open-ai-models.html
preview:
  file: 2026-09-03-the-pulse-tech-companies-move-to-open-ai-models.preview-b43a8559e3be.webp
  width: 256
  height: 143
  color: '#f5f5f6'
images:
- source: https://substackcdn.com/image/fetch/$s_!l-d4!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F534b20ec-8678-40d4-9248-2d5751979703_2048x1145.png
  original:
    file: 2026-09-03-the-pulse-tech-companies-move-to-open-ai-models.image-c97569f269f1.png
    width: 2048
    height: 1145
  variants:
  - file: 2026-09-03-the-pulse-tech-companies-move-to-open-ai-models.image-05c045abcce6.webp
    width: 320
    height: 179
  - file: 2026-09-03-the-pulse-tech-companies-move-to-open-ai-models.image-f33294f81a5f.webp
    width: 640
    height: 358
  - file: 2026-09-03-the-pulse-tech-companies-move-to-open-ai-models.image-2fcfff37e81d.webp
    width: 960
    height: 537
  - file: 2026-09-03-the-pulse-tech-companies-move-to-open-ai-models.image-b24c53befa9b.webp
    width: 1280
    height: 716
  - file: 2026-09-03-the-pulse-tech-companies-move-to-open-ai-models.image-1ac9c4979aeb.webp
    width: 1600
    height: 895
  - file: 2026-09-03-the-pulse-tech-companies-move-to-open-ai-models.image-4ce4f0178937.webp
    width: 2048
    height: 1145
  color: '#fdfdfd'
---

*The Pulse is a series covering events, insights, and trends within Big Tech and startups.*

Today, we cover:

1. **New trend: tech companies moving to open models.** Uber, Pinterest, Stripe, Coinbase, Ramp, and AT&T are making large savings on their AI bills by dropping proprietary models and using smart model routing.

2. **Automatic software maintenance experiments by Linear and Anthropic.** Both startups are experimenting with how far they can push AI agents to automatically fix bugs and remove tech debt. It’s working better than anyone might’ve expected in the recent past, but not producing code that can be merged without review.

3. **Frontier AI lab wars: OpenAI pulls models from SpaceX / Cursor.** With SpaceX now a frontier model and rival to OpenAI and Anthropic, OpenAI has pulled its GPT models from Cursor. This isn’t an option for Anthropic which is dependent on the SpaceX compute they rent to serve Claude.

4. **HR tech startup’s one-dev-per-project approach.** A full-remote HR startup with 70 engineers has a single engineer run each project, and says the approach works well. Will this approach be adopted elsewhere, especially at other full-remote startups?

5. **Industry Pulse.** Meta moved over to Slack for better agent interoperability, layoffs at Uber and PagerDuty, Anthropic upsets users by calling a rate limit decrease an “increase”, token usage explodes on OpenRouter, AI drives surging demand for Apple’s Mac Mini & Mac Studio, and more.

*Update: a week after publishing this article, Ara Krahzian at Ramp [has confirmed](https://x.com/arakharazian/status/2097706961584140645?s=20) that AI spend in August, has, indeed, declined at the top 1% of businesses by 10%, based on Ramp data. I’d wager those companies are not spending fewer tokens, but they are optimizing cost, in ways outlined below.*

In May, I [covered an emerging trend](https://newsletter.pragmaticengineer.com/p/the-pulse-a-trend-of-trying-to-cut) of companies *wanting* to cut back their AI spending, starting with engineering departments. Different approaches were being tried:
