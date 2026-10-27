---
title: The Mythical Man-Month in the Age of AI
link: https://j11g.com/the-mythical-man-month-in-the-age-of-ai
source: hnrss-org
published: 2026-10-27T06:25:37Z
updated: 2026-10-27T06:25:37Z
first_seen: 2026-10-27T10:24:00.280726925Z
authors:
- ilreb
content: extracted
html: 2026-10-27-the-mythical-man-month-in-the-age-of-ai.html
remote_preview:
  url: https://j11g.com/content/images/cern-resized.jpg
---

Last weekend I watched four people desperately trying to get a laptop to connect to a screen. I was happily observing their efforts, with zero intent to join in.

But someone nudged me: can't *you* have a look at it?

My reply: "Haha sorry no! My experience is that adding more people will not solve this problem any quicker".

I was of course -- indirectly -- referring to Brooks's Law:

> "Adding manpower to a late software project makes it later."

It was formulated by computer scientist Frederick P. Brooks Jr. in his 1975 book The Mythical Man-Month, based on his experience managing the IBM OS/360 project.

However I got to thinking. Does this still hold up in the age of AI agents and LLMs?

At first glance, you’d think no. The traditional bottlenecks -- onboarding time, endless stand-ups, and office politics -- disappear overnight. An AI agent is up to speed in milliseconds and doesn't demand a coffee break.

But when you look closer, and the law hasn't vanished. It just mutated.

The famous ["Baby Problem"](https://en.wikipedia.org/wiki/Brooks%27s_law#:~:text=Adding%20more%20people,in%20one%20month%22.) remains: nine agents still can’t deliver a baby in one month. Sequential tasks are still sequential.

Overhead hasn't disappeared; it shifted: instead of managing people, you are now managing agents. Someone still needs to define "done", review the code, catch hallucinations, and stitch the pieces together.

That someone is you.

Throw nine autonomous agents at a single codebase without crystal-clear orchestration, and they will happily rewrite each other's work in a high-speed loop.

Maybe the 2026 update to Brooks’s Law should be:

> "Adding agents to a project with unclear requirements gets you the wrong answer faster."
