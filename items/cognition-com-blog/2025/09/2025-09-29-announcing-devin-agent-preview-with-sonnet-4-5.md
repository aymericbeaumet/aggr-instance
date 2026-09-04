---
title: Announcing Devin Agent Preview with Sonnet 4.5
link: https://cognition.com/blog/devin-agent-preview-sonnet-4-5
source: cognition-com-blog
published: 2025-09-29T00:00:00Z
first_seen: 2026-09-04T12:14:41.802665207Z
content: extracted
html: 2025-09-29-announcing-devin-agent-preview-with-sonnet-4-5.html
---

By Theodor Marcu09.29.25

\
Claude Sonnet 4.5 is Anthropic's newest and most powerful model, and we believe it represents a new generation of coding models.

> For Devin, Claude Sonnet 4.5 increased planning performance by 18% and end-to-end eval scores by 12% - the biggest jump we've seen since the release of Claude Sonnet 3.6. It excels at testing its own code, enabling Devin to run longer, handle harder tasks, and deliver production-ready code more consistently.— Scott Wu

Over the past few days, we've been testing this model internally, and what we've found goes beyond just benchmark improvements. The model exhibits fundamentally different behaviors - ways it approaches problems, manages its own work, and structures its development process - that required us to rethink how we architect Devin.

![Announcing Devin Agent Preview with Sonnet 4.5](https://cdn.sanity.io/images/2mc9cv2v/production/d46d0d337d984e92f01cb4f5e7ccec48dc64ac4b-3346x1908.png?w=1600&fit=max)

Because Devin is an agent that plans, executes, and iterates rather than just autocompleting code, we get an unusual window into what's genuinely changed. And with Sonnet 4.5, the improvements compound across our feedback loops in exciting new ways.

Claude Sonnet 4.5 is available in Devin starting today. We're excited to see what you build with it.

**Want to learn more about what makes this model different?** We've been testing Sonnet 4.5 extensively over the past few days and discovered some fascinating behaviors, from how it manages its own context window to how it creates feedback loops to verify its work.

[Read our deep dive on the unexpected challenges and opportunities we found while rebuilding Devin for this new generation of coding models](https://cognition.ai/blog/devin-sonnet-4-5-lessons-and-challenges)
