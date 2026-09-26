---
title: Asana cleared 5 years of engineering work in 2 weeks with Codex
link: https://openai.com/index/asana
source: openai-com-news
published: 2026-08-18T07:00:00Z
updated: 2026-08-18T07:00:00Z
first_seen: 2026-09-07T17:03:44.343711062Z
summary: Asana used OpenAI Codex to replace an outdated testing system in two weeks, completing work expected to take five years for about $12K.
content: extracted
html: 2026-08-18-asana-cleared-5-years-of-engineering-work-in-2-weeks-with.html
preview:
  file: 2026-08-18-asana-cleared-5-years-of-engineering-work-in-2-weeks-with.preview-b423635c4331.webp
  width: 256
  height: 144
  alt: OpenAI and Asana logos beside a red textured center image.
  color: '#facdca'
images:
- source: https://images.ctfassets.net/kftzwdyauwt9/QuPTOlw5BZCStCK9oVFW2/d80ca15c2876853ba7789109f075e8f5/oai_asana_SEO.real.png?w=1600&h=900&fit=fill
  original:
    file: 2026-08-18-asana-cleared-5-years-of-engineering-work-in-2-weeks-with.image-c80f0387535a.png
    width: 1600
    height: 900
  variants:
  - file: 2026-08-18-asana-cleared-5-years-of-engineering-work-in-2-weeks-with.image-9567380f14b3.webp
    width: 320
    height: 180
  - file: 2026-08-18-asana-cleared-5-years-of-engineering-work-in-2-weeks-with.image-08bd27c88ae4.webp
    width: 640
    height: 360
  - file: 2026-08-18-asana-cleared-5-years-of-engineering-work-in-2-weeks-with.image-3f1ff8e72026.webp
    width: 960
    height: 540
  - file: 2026-08-18-asana-cleared-5-years-of-engineering-work-in-2-weeks-with.image-37cc8d1b2c60.webp
    width: 1280
    height: 720
  - file: 2026-08-18-asana-cleared-5-years-of-engineering-work-in-2-weeks-with.image-95411a093e4c.webp
    width: 1600
    height: 900
  color: '#fefefe'
---

In order to modernize its frontend stack, Asana needed to finish a long-running code migration that was still years from completion. At its prior pace—with only a few engineers working on the migration incrementally alongside other priorities—Asana projected that it would take roughly five more years to complete. With Codex, Asana completed the project in about two weeks.

Asana helps teams coordinate work. Its platform uses AI agents and automations to help customers manage, track, and execute that work. Asana brings the same approach to its engineering organization, where people use OpenAI Codex, powered by frontier models, to tackle large codebase changes, then review and approve each proposed change.

For this project, [Codex helped Asana’s engineers remove Enzyme](https://asana.com/inside-asana/migrating-off-enzyme-2-weeks) , an outdated testing system that had made the company’s code harder to upgrade. Model and infrastructure costs came to about $12,000.

> “Not every years-long project will collapse into weeks. But agents can give engineers more room for craft—and make once-impossible work worth attempting.”

—Amritansh Raghav, Chief Technology Officer, Asana

Their old testing tool, Enzyme, had fallen out of active maintenance and was becoming a blocker to modernizing Asana’s frontend stack. Asana used Codex, powered by frontier models, to do the work. From a five-sentence prompt, up to four coding agents worked in parallel, each in a separate copy of the codebase. An engineer checked progress twice a day and reviewed every proposed change. Simpler instructions worked better than a more elaborate setup.

After 1.5 weeks of engineering effort spread across two calendar weeks, Enzyme was fully removed. The work also delivered benefits beyond the framework migration itself, improving test coverage, fixing poor-quality tests, and cleaning up legacy testing infrastructure. Model and infrastructure costs totaled about $12K. Asana estimates that completing that same scope through manual engineering work would have represented roughly $6M in fully loaded engineering effort. The experience changed which long-running software projects the company believes are practical to take on.

With this migration complete, Asana can test agents on other migrations, rewrites, and performance problems it once assumed would take years. The team hopes this will give engineers more room to focus on craft while people continue to review the work.
