---
title: Stop being skeptical about AI for development with Charity Majors
link: https://newsletter.pragmaticengineer.com/p/stop-being-skeptical-about-ai-for
source: newsletter-pragmaticengineer-com
published: 2026-08-12T16:45:07Z
updated: 2026-08-12T16:45:07Z
first_seen: 2026-09-19T21:30:23.395188495Z
authors:
- Gergely Orosz
summary: In 2025, it was rational to be skeptical about AI. In 2026, it's not, anymore. With Charity Majors, CTO and co-founder of Honeycomb.
content: extracted
html: 2026-08-12-stop-being-skeptical-about-ai-for-development-with-charity.html
preview:
  file: 2026-08-12-stop-being-skeptical-about-ai-for-development-with-charity.preview-fa3dcec6d1d2.webp
  width: 256
  height: 128
  color: '#c2bdc0'
images:
- source: https://substackcdn.com/image/fetch/$s_!FF3K!,w_1200,h_600,c_fill,f_jpg,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F72b39474-2eb9-4763-803c-26bbc46bf87f_1280x720.png
  original:
    file: 2026-08-12-stop-being-skeptical-about-ai-for-development-with-charity.image-cd3603371e25.jpg
    width: 1200
    height: 600
  color: '#f0f0f0'
- source: https://substackcdn.com/image/fetch/$s_!TArv!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6bda57d7-998f-464d-bb13-7b961d6b3598_1600x140.png
  original:
    file: 2026-08-12-stop-being-skeptical-about-ai-for-development-with-charity.image-7e870021d623.jpg
    width: 1456
    height: 127
  color: '#fdfdfd'
extra:
  audio_url: https://api.substack.com/feed/podcast/210687046/c0fb00207dd48d417228414f62510f3f.mp3
---

**Listen and watch now on [YouTube](https://youtu.be/HC8T1OlgYi0) , [Apple](https://podcasts.apple.com/us/podcast/the-pragmatic-engineer/id1769051199) , and [Spotify](https://open.spotify.com/show/2Bho9xCbOQMWMJ7UKmqCzD) .** See the episode transcript at the top of this page, and timestamps for the episode at the bottom.

[![](https://substackcdn.com/image/fetch/$s_!TArv!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6bda57d7-998f-464d-bb13-7b961d6b3598_1600x140.png)](https://substackcdn.com/image/fetch/$s_!TArv!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6bda57d7-998f-464d-bb13-7b961d6b3598_1600x140.png)

**• [Antithesis](https://antithesis.com/pragmatic)** – turbocharge testing of your systems by running your whole system under aggressive fault injection. There’s good reason teams like Jane Street, Fly.io, and the etcd community rely on Antithesis. [Learn more.](https://antithesis.com/pragmatic)

• **[Buildkite](http://buildkite.com/pragmatic) –** the CI platform trusted by OpenAI, Anthropic, Cursor, Meta, Uber, NVIDIA , Airbnb and many more. When CI volume becomes an architecture problem, you deserve better CI. Engineered to reliably manage whatever your coding agents throw at the build queue: today, next year, and beyond. [Learn more.](https://buildkite.com/pragmatic)

**• [WorkOS](https://workos.com/)** – make your app and agents Enterprise Ready, with SSO, SCIM, RBAC, and more. [Get started](https://workos.com/) .

In 2025, it was rational to be skeptical about AI, but in 2026 it’s clear that AI is changing all of the industry, and there’s less and less place for skepticism. This take is from one of my favorite voices in software reliability and observability: Charity Majors, CTO and cofounder of Honeycomb, co-author of [Observability Engineering](https://www.oreilly.com/library/view/observability-engineering-2nd/9781098179915/) . (Note: the [second edition of Observability Engineering](https://www.oreilly.com/library/view/observability-engineering-2nd/9781098179915/) is out, and it’s pretty much a full rewrite of the book, I recommend grabbing it if you’re building reliable systems)

In this episode, I sat down with Charity to discuss how her thinking on AI has evolved, why she believes it is becoming a foundational part of software engineering, and what that means for how teams build, review, and ship software.

We explore how AI is changing the economics of code generation, why reliability and verification are increasingly the bottlenecks, and why the rise of non-deterministic systems requires more engineering discipline. Charity shares her views on code reviews, observability, DevOps, leadership, and why both AI skeptics and enthusiasts are getting important things right.

Here are 13 parts I found especially interesting, talking with Charity:

**1\. In March 2025, Charity told the audience at SREcon to try vibe coding, and back then, the response was grumbling.** Charity’s point was that people who are skeptical of AI should still learn to use it, because you can complain better if you’ve learned it. At this time, Charity still saw AI having a bigger impact than a new programming language, but was skeptical that it would have a generational impact.​

**2\. Charity’s turning point in seeing AI as a generational change was in November 2025.** This was due to Opus 4.5, but Charity argues that the coding harness (Claude Code) made the bigger difference. Because thanks to Claude Code, harnesses went from being more of a shell script to serious infrastructure.​

**3\. The impact of AI on the industry in 2025 was similar to the impact of the cloud in 2010.** Looking back, Charity is comfortable saying this: in 2010, it became clear that cloud computing was certainly going mainstream and would change the infra-layer. After 2025, it’s also clear that AI will have a similar impact on the infrastructure of building software.

**4\. Engineers who were skeptical of AI up to 2025: they had good reason to be so.** This was because we’ve seen plenty of technologies and innovations in the past that all promised to transform the software industry, but later fell short. Examples include COBOL (a technology promising that programmers would no longer be needed to create software), neural nets, no-code and low-code tools.​

**5\. The question engineers need to answer: what would it take for you to be fully comfortable shipping code you have not read?** Charity believes it is a “when” and not an “if” that professional software engineers will ship code they never looked at – and thus do not understand – to production. Engineering is building the systems that validate this code, and allow shipping with full confidence.

**6\. AI could have the software industry go through the “pets” to “cattle” change that compute infra went through in the 2010s.** Up to now, writing software from scratch was far more expensive than editing existing software. But now, generating hundreds of variants of a function can be done faster than how long it would take you to hand-write it once.

Charity believes that we might be at the beginning of the transition from “pets” to “cattle” that happened at the hardware infrastructure layer. Before the 2010s, configuring and repairing individual servers was commonly done. But with tools like Terraform and Kubernetes, individual servers having issues are no longer fixed up: they are re-created instead. Charity thinks the same might happen with code, sooner rather than later. When there’s an issue with the code, generate new code that solves it, and is verifyably correct.​

**7\. Her contrarian take: code review is overrated, and the least valuable part of what humans add to software engineering.** Charity says that humans are good at conversations and deciding what to build, not reading code to check for correctness, syntax and bugs.​

**8\. Charity’s verdict of 20 years of DevOps: it failed.** The DevOps feedback was about trying to create a feedback loop that connected people writing the code to the code running in production. She thinks that the “ops people: learn to code!” wave worked, but the “software engineers: understand your code in production” failed, to this day.

**9\. Non-deterministic systems require more engineering discipline versus before.** With code written by AI, we’re reducing the trust in the code (because we no longer wrote it), so we need to increase trust at the other part of the development process. Specifically, at validation: with things like tests, evals, and conformance testing.​

**10\. Charity’s career advice for engineering directors: run towards the waves, and get AI on your resume, immediately.** It’s an anxious time to work in tech, thanks to all the change, driven by AI. Charity reminds us that anxiety and excitement are physiologically almost the same, but the difference is agency. When you have no agency, you’re more likely to get anxious, and when you do, you’re more likely to get excited.

So her advice to anxious engineering directors: consider going back to IC work, where you’ll have far more agency. IC work is well-respected, getting back to it has never been easier, but the window to do so is closing. As she put it:​

> “The next time you’ll have a job interview, you’ll be filtered out if you don’t have AI experience.”​

**11\. On AI fatigue: take back control with small acts!** We talked about various types of AI fatigue: reviewing AI slop, getting tired of the AI hype, and getting worn down by “doom trolling” by AI CEOs. Charity finds small acts of taking control back in your work from AI tools help. For example, none of the Honeycomb team uses AI on Wednesdays.​

**12\. Charity would like to see both the “AI-pilled” and the “anti-AI” camps tell the stories better.** As she put it:

“There are some really incredible things happening in software right now, for example, with rewrites and with automating away toil. Not a single person that I’ve talked to would give up using AI.

But half of the people are seeing the *wins,* and they’re not connecting it to the *cost*, which makes them think that their coworkers are just afraid of getting automated out of existence.

So that’s my beg to everyone who listens to this: tell the whole story! Talk about the costs as well. We’re all in it together.”

**13\. Charity’s rule on AI writing: do not send any message/email to a human that you yourself have not read in full.** She also says that it would take them longer to read whatever you send than it took you to produce it: it’s probably slop!

**•** [Shipping to production](https://newsletter.pragmaticengineer.com/p/shipping-to-production)

**•** [Deepdive: How 10 tech companies choose the next generation of dev tools](https://newsletter.pragmaticengineer.com/p/measuring-ai-dev-tools)

**•** [Why is Meta destroying its engineering organization?](https://newsletter.pragmaticengineer.com/p/why-is-meta-destroying-its-engineering)

**•** [When AI writes almost all code, what happens to software engineering?](https://newsletter.pragmaticengineer.com/p/when-ai-writes-almost-all-code-what)

**•** [Are AI agents actually slowing us down?](https://newsletter.pragmaticengineer.com/p/are-ai-agents-actually-slowing-us)

**•** [Observability: the present and future](https://newsletter.pragmaticengineer.com/p/observability-the-present-and-future) , with Charity Majors

**•** [The third golden age of software engineering – thanks to AI](https://newsletter.pragmaticengineer.com/p/the-third-golden-age-of-software) , with Grady Booch

[00:00](https://www.youtube.com/watch?v=HC8T1OlgYi0) Intro

[02:56](https://www.youtube.com/watch?v=HC8T1OlgYi0&t=176s) How Parse led to Honeycomb

[06:00](https://www.youtube.com/watch?v=HC8T1OlgYi0&t=360s) The limits of individual productivity metrics

[09:08](https://www.youtube.com/watch?v=HC8T1OlgYi0&t=548s) How Charity’s perspective on AI has evolved

[13:50](https://www.youtube.com/watch?v=HC8T1OlgYi0&t=830s) Rewriting code vs. editing code

[19:20](https://www.youtube.com/watch?v=HC8T1OlgYi0&t=1160s) Production as a stage of development

[22:14](https://www.youtube.com/watch?v=HC8T1OlgYi0&t=1334s) Code reviews

[26:56](https://www.youtube.com/watch?v=HC8T1OlgYi0&t=1616s) Non-deterministic systems

[31:11](https://www.youtube.com/watch?v=HC8T1OlgYi0&t=1871s) Sensible uses of AI

[37:41](https://www.youtube.com/watch?v=HC8T1OlgYi0&t=2261s) The two AI camps

[44:40](https://www.youtube.com/watch?v=HC8T1OlgYi0&t=2680s) Why AI works so well for building software

[49:42](https://www.youtube.com/watch?v=HC8T1OlgYi0&t=2982s) DevOps

[55:13](https://www.youtube.com/watch?v=HC8T1OlgYi0&t=3313s) Modern observability

[1:00:40](https://www.youtube.com/watch?v=HC8T1OlgYi0&t=3640s) Handling context overload

[1:01:56](https://www.youtube.com/watch?v=HC8T1OlgYi0&t=3716s) What’s new in Observability Engineering’s 2nd edition

[1:07:45](https://www.youtube.com/watch?v=HC8T1OlgYi0&t=4065s) What effective leadership looks like

[1:10:25](https://www.youtube.com/watch?v=HC8T1OlgYi0&t=4225s) Engineering management: what is changing?

[1:16:31](https://www.youtube.com/watch?v=HC8T1OlgYi0&t=4591s) Junior engineers

[1:18:01](https://www.youtube.com/watch?v=HC8T1OlgYi0&t=4681s) AI fatigue

[1:21:39](https://www.youtube.com/watch?v=HC8T1OlgYi0&t=4899s) Book recommendations

**Where to find Charity Majors:**

• X: [https://x.com/mipsytipsy](https://x.com/mipsytipsy)

• LinkedIn: [https://www.linkedin.com/in/charity-majors](https://www.linkedin.com/in/charity-majors)

• Website:

**Mentions during the episode:**

• Observability Engineering, 2nd Edition: [https://www.oreilly.com/library/view/observability-engineering-2nd/9781098179915](https://www.oreilly.com/library/view/observability-engineering-2nd/9781098179915)

• Honeycomb: [https://www.honeycomb.io](https://www.honeycomb.io)

• Linden Lab: [https://lindenlab.com](https://lindenlab.com)

• Second Life: [https://secondlife.com](https://secondlife.com)

• Parse: [https://en.wikipedia.org/wiki/Parse,\_Inc](https://en.wikipedia.org/wiki/Parse,_Inc) .

• Scuba: [https://research.facebook.com/publications/scuba-diving-into-data-at-facebook](https://research.facebook.com/publications/scuba-diving-into-data-at-facebook)

• Can You Really Measure Individual Developer Productivity? - Ask the EM: [https://blog.pragmaticengineer.com/can-you-measure-developer-productivity](https://blog.pragmaticengineer.com/can-you-measure-developer-productivity)

• Let’s Talk Agentic Development: Spotify x Anthropic Live: [https://engineering.atspotify.com/2026/4/anthropic-agentic-development](https://engineering.atspotify.com/2026/4/anthropic-agentic-development)

• Questionable Advice: Can Engineering Productivity Be Measured?:

• 2025 was for AI what 2010 was for cloud:

• AI demands more engineering discipline. Not less:

• The Phoenix Architecture: [https://aicoding.leaflet.pub](https://aicoding.leaflet.pub)

• The third golden age of software engineering – thanks to AI, with Grady Booch: [https://newsletter.pragmaticengineer.com/p/the-third-golden-age-of-software](https://newsletter.pragmaticengineer.com/p/the-third-golden-age-of-software)

• Software architecture with Grady Booch: [https://newsletter.pragmaticengineer.com/p/software-architecture-with-grady-booch](https://newsletter.pragmaticengineer.com/p/software-architecture-with-grady-booch)

• TypeScript, C# and Turbo Pascal with Anders Hejlsberg: [https://newsletter.pragmaticengineer.com/p/typescript-c-and-turbo-pascal-with](https://newsletter.pragmaticengineer.com/p/typescript-c-and-turbo-pascal-with)

• David Poll on LinkedIn: [https://www.linkedin.com/in/depoll](https://www.linkedin.com/in/depoll)

• Intercom: [https://www.intercom.com](https://www.intercom.com)

• AI is approving our pull requests: Here’s how we made it safe: [https://www.intercom.com/blog/ai-is-approving-our-pull-requests-heres-how-we-made-it-safe](https://www.intercom.com/blog/ai-is-approving-our-pull-requests-heres-how-we-made-it-safe/)

• How AI will change software engineering – with Martin Fowler: [https://newsletter.pragmaticengineer.com/p/martin-fowler](https://newsletter.pragmaticengineer.com/p/martin-fowler)

• HackerRank open sourced its ATS. My resume scored 90/100. Oh wait 74. No – 88: [https://news.ycombinator.com/item?id=48713832](https://news.ycombinator.com/item?id=48713832)

• AI enthusiasts are in a race against time, AI skeptics are in a race against entropy:

• Ep. #89, Software is the Killer App with Bryan Cantrill of 0xide Computer: [https://www.honeycomb.io/resources/podcasts/ep-89-bryan-cantrill-software-is-the-killer-app](https://www.honeycomb.io/resources/podcasts/ep-89-bryan-cantrill-software-is-the-killer-app)

• Eric Riddoch’s post on LinkedIn: [https://www.linkedin.com/posts/eric-riddoch\_the-observability-engineering-book-has-share-7475807056285814785-pw4J](https://www.linkedin.com/posts/eric-riddoch_the-observability-engineering-book-has-share-7475807056285814785-pw4J)

• Why traditional observability misses AI agent failure: [https://www.dataiku.com/blog/traditional-observability-misses-ai-agent-failure](https://www.dataiku.com/blog/traditional-observability-misses-ai-agent-failure)

• Charity’s LinkedIn post on effective leaders: [https://www.linkedin.com/posts/charity-majors\_the-most-effective-leaders-are-kind-caring-share-7477160924928233472-qcLw](https://www.linkedin.com/posts/charity-majors_the-most-effective-leaders-are-kind-caring-share-7477160924928233472-qcLw/)

• Catastrophe Ethics: How to Choose Well in a World of Tough Choices: [https://www.amazon.com/dp/0593471970](https://www.amazon.com/dp/0593471970)

• More Everything Forever: AI Overlords, Space Empires, and Silicon Valley’s Crusade to Control the Fate of Humanity: [https://www.amazon.com/More-Everything-Forever-Overlords-Humanity/dp/1541619595](https://www.amazon.com/More-Everything-Forever-Overlords-Humanity/dp/1541619595)

—

Production and marketing by [Pen Name](https://penname.co/) .
