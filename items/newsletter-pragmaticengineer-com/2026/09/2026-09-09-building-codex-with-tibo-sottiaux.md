---
title: Building Codex with Tibo Sottiaux
link: https://newsletter.pragmaticengineer.com/p/building-codex-with-tibo-sottiaux
source: newsletter-pragmaticengineer-com
published: 2026-09-09T15:57:24Z
updated: 2026-09-09T15:57:24Z
first_seen: 2026-09-19T21:30:23.395188495Z
authors:
- Gergely Orosz
summary: OpenAI’s Tibo Sottiaux shares how Codex was built and how it’s reshaping software development.
content: extracted
html: 2026-09-09-building-codex-with-tibo-sottiaux.html
preview:
  file: 2026-09-09-building-codex-with-tibo-sottiaux.preview-8f9249f14a2e.webp
  width: 256
  height: 144
  color: '#bfbcbb'
images:
- source: https://substackcdn.com/image/youtube/w_728,c_limit/sLSTM9znQNs
  original:
    file: 2026-09-09-building-codex-with-tibo-sottiaux.image-2e82e0a79c7c.jpg
    width: 728
    height: 410
  color: '#f0f0f0'
- source: https://substackcdn.com/image/fetch/$s_!AlJu!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8e3d3c6f-6a37-411e-aa1a-dfcefc979af3_1600x140.png
  original:
    file: 2026-09-09-building-codex-with-tibo-sottiaux.image-a3765fe2c5ab.jpg
    width: 1456
    height: 127
  color: '#fdfdfd'
- source: https://substackcdn.com/image/fetch/$s_!x-Vz!,w_120,h_120,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fpbs.substack.com%2Fprofile_images%2F2093807917833281537%2F2yBgpwVV.jpg
  original:
    file: 2026-09-09-building-codex-with-tibo-sottiaux.image-5521472a8faa.jpg
    width: 120
    height: 120
  color: '#150216'
extra:
  audio_url: https://api.substack.com/feed/podcast/214748970/1fe61a458d29a2d32243be0f3366b6d3.mp3
---

**Listen and watch now on [YouTube](https://youtu.be/sLSTM9znQNs) , [Apple](https://podcasts.apple.com/us/podcast/the-pragmatic-engineer/id1769051199) , and [Spotify](https://open.spotify.com/show/2Bho9xCbOQMWMJ7UKmqCzD) .** See the episode transcript at the top of this page, and timestamps for the episode at the bottom.

[![](https://substackcdn.com/image/fetch/$s_!AlJu!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8e3d3c6f-6a37-411e-aa1a-dfcefc979af3_1600x140.png)](https://substackcdn.com/image/fetch/$s_!AlJu!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8e3d3c6f-6a37-411e-aa1a-dfcefc979af3_1600x140.png)

**• [turbopuffer](https://turbopuffer.com/pragmatic)** – a vector and full-text search engine built on object storage. It’s fast, cheap, and extremely scalable. The teams building the smartest AI products out there — Anthropic, Cognition, Notion, Harvey — all run on turbopuffer. [Check it out](https://turbopuffer.com/pragmatic)

**• [Antithesis](https://antithesis.com/pragmatic)** – use AI agents to work on critical systems without worrying about correctness. Antithesis runs your complete system in a hostile environment, analyzes its behavior, and reproduces every issue perfectly. [Learn more](https://antithesis.com/pragmatic)

**• [Entire](https://entire.io/pragmatic)** – Git hosting, rebuilt for the agentic era. Entire hosts your code in-region, and is up to 89x faster than any other competitor. Mirror from GitHub [with a single click](https://entire.io/pragmatic) – I’ve already done so.

[Tibo Sottiaux](https://www.linkedin.com/in/thibault-sottiaux-27195366) is one of the engineers who created Codex, and today, he heads up the Core Products & Platform org at OpenAI which also includes Codex. He’s also one of the most public faces of Codex due to his frequent – and generous – usage reset announcements, like this one [yesterday](https://x.com/thsottiaux/status/2097174560412246215?s=20) .

In this episode of the Pragmatic Engineer Podcast, Tibo and I discuss how Codex was built and continues to be iterated upon. We explore why the Codex CLI is written in Rust and was released as open source, how the harness and models have evolved, and why Codex supports models from multiple providers.

Tibo also shares details about how the OpenAI team uses Codex throughout the software development lifecycle, including code reviews, maintenance, and system rearchitecture. We look into how AI is lowering the cost of changing code – and some interesting side effects of this – the merger of ChatGPT and Codex, and also how Tibo uses the tools in his own work.

**1\. A shock cancellation proved to be an important lesson.** While at Google in London, Tibo spent two enjoyable years working on a project in the Ads organization – right up until a VP flew in from California and abruptly cancelled the whole thing. Tibo was shocked: there were hundreds of users and he was having a great time solving engineering challenges. Reflecting later, he realized he’d been blind to the fact that the product had no product-market fit and the feedback loop from users was poor. Tibo also learned that just because a product manager says a project is going well, doesn’t mean it is! That experience means he now always questions the impact of his work, and the importance of projects to which he contributes. *Looking at Tibo’s career since, it seems like a well-timed lesson!*

**2\. Google had a “ChatGPT-like” project a year before OpenAI.** At the start of the decade, DeepMind was largely focused on games and reinforcement learning, but some members, including Tibo, reckoned that scaling language models might lead to artificial general intelligence (AGI). Their “chat with an LLM” project spread like wildfire internally, but for some reason the product was never launched. Who knows what would have happened if Google had beaten OpenAI to the public launch of AI chatbots in 2022.

**3\. Tibo was drawn to OpenAI because only 20 people worked on ChatGPT.** Tibo had a great run at Google, but missed working somewhere where Research and Product collaborated more closely than at the search giant. When he learned in 2023 that ChatGPT was built and maintained by around 20 engineers – despite its massive popularity – he was very surprised and wanted to join.

**4\. Codex is built using Rust, even though the AI models were much better at writing Python and TypeScript at the time.** The Codex team had a vision of Codex instances running on millions of cloud machines, which meant performance, security, and engineering for efficiency and scale were the first design principles. This led to Rust, despite AI models then not being the strongest at writing Rust. The team’s decision to choose a performant language upfront and avoid a rewrite later reminds me of Casey Muratori’s point about the [need to architect for performance, upfront.](https://newsletter.pragmaticengineer.com/i/212722114/takeaways-from-the-conversation-with-casey)

**5\. Codex is open source, which has upsides and downsides.** Codex’s biggest competitor, Claude Code, is closed source, so I find it inspiring that the Codex team chose the open source path. Tibo says the upsides are trust and the community of contributors who are an energizing influence. A less discussed downside of open source is that the Codex team’s work sometimes gets copied and released in other tools before Codex. Tibo told me this stings, but it’s the price of working in the open.

**6\. Open source is a big reason why Codex supports working with other AI models.** Claude Code can only be used with Anthropic’s models, whereas Codex is usable with any model, not just OpenAI’s. Being open source means that even if Codex were locked down to a model, anyone could still fork the harness and change a few lines of code to support a different model. Tibo believes in winning by letting users use the best models; the Codex team themselves also try other models in the same harness. *Personally, I appreciate this approach of encouraging competition from a frontier lab!*

**7\. Cloud development environments (CDEs) never took off outside of Big Tech because of setup costs – but AI agents could change this.** Tibo predicts a resurgence of fully cloud-orchestrated machines, where agents like Codex can configure and stay in sync with your local machine setup.

**8\. The Codex harness is always slightly ahead of OpenAI’s latest model.** The harness provides the model with crutches: guardrails, safety, efficiency, steerability, and the developer message injected into context at the start of each turn. As models improve, some “crutches” are discarded and the harness shrinks. This has been the development cycle between Codex and OpenAI’s new models to date.

**9\. Codex “knows” a staggering amount because it’s plugged into pretty much every OpenAI system.** I asked Tibo what pointers he’d give a new joiner on the Codex team. His answer: “have you asked Codex?” At OpenAI, it’s plugged into Slack, every document, and all code, by default. New starters are surprised about being able to ask it anything, including who’s working on something, or why a decision was made. The team purposely work in public channels and open documents with broad permissions.

**10\. Correctness checks and security reviews will be automated with AI.** With code review changing under AI’s influence, Tibo believes that discussion about the intent of code doesn’t have to happen inside a code review, which is mostly about correctness, information exchange, and providing a forcing function for conversations that should’ve happened sooner. With AI code review, conversations about what the system should do still matter, and are probably best had before the code is written. This echoes the theme of this week’s Tuesday article about [what is happening with code reviews](https://newsletter.pragmaticengineer.com/p/what-is-happening-with-code-reviews) .

**11\. Maintaining and re-architecting code is becoming very cheap with agents, especially time-wise.** Maintenance tasks like dependency upgrades can be handled by a model blasting through the codebase within a couple of hours. Meanwhile, re-architecting for new tradeoffs that used to take years can now take days, at most. Tibo adds a caveat: quality code, good abstractions, and good test suites greatly affect how easy – or not – a codebase change is to make.

**12\. Being “in the zone” is history; Tibo sees code as a tool for solving problems.** During the podcast recording, we bonded over remembering the “good old days” of pulling late nighters and staying “in the zone” to solve difficult problems with code. These days, Tibo has adapted, like most people at OpenAI. He still opens an editor and writes a little code because it feels nice, but says that an upside of AI agents is being able to gather more data faster – meaning there’s less need for the lengthy coding sessions of yore. Instead of making gut calls, he can fire off an agent and get the data within a minute to make much better decisions with.

**•** [How Codex is built](https://newsletter.pragmaticengineer.com/p/how-codex-is-built)

**•** [How Claude Code is built](https://newsletter.pragmaticengineer.com/p/how-claude-code-is-built)

**•** [How Cursor was built](https://newsletter.pragmaticengineer.com/p/cursor)

**•** [What is “loop engineering?”](https://newsletter.pragmaticengineer.com/p/what-is-loop-engineering)

**•** [How Uber uses AI for development: inside look](https://newsletter.pragmaticengineer.com/p/how-uber-uses-ai-for-development)

**•** [Why Ramp built its own in-house coding agent, Inspect](https://newsletter.pragmaticengineer.com/p/why-ramp-built-inspect)

**•** [“I ship code I don’t read”:](https://newsletter.pragmaticengineer.com/p/the-creator-of-clawd-i-ship-code) with Peter Steinberger, the creator of OpenClaw

[00:00](https://www.youtube.com/watch?v=sLSTM9znQNs) Intro

[07:21](https://www.youtube.com/watch?v=sLSTM9znQNs&t=441s) Working at Google

[12:41](https://www.youtube.com/watch?v=sLSTM9znQNs&t=761s) What drew Tibo to OpenAI

[15:19](https://www.youtube.com/watch?v=sLSTM9znQNs&t=919s) The early days of Codex

[18:20](https://www.youtube.com/watch?v=sLSTM9znQNs&t=1100s) Why Codex was built in Rust

[21:15](https://www.youtube.com/watch?v=sLSTM9znQNs&t=1275s) Why Codex is open source

[25:50](https://www.youtube.com/watch?v=sLSTM9znQNs&t=1550s) Codex plays nice with other models: why?

[32:09](https://www.youtube.com/watch?v=sLSTM9znQNs&t=1929s) How the harness works

[36:44](https://www.youtube.com/watch?v=sLSTM9znQNs&t=2204s) Harness and model improvements

[41:19](https://www.youtube.com/watch?v=sLSTM9znQNs&t=2479s) The SDLC behind Codex

[46:39](https://www.youtube.com/watch?v=sLSTM9znQNs&t=2799s) Code reviews at Codex

[52:09](https://www.youtube.com/watch?v=sLSTM9znQNs&t=3129s) Maintenance and architecture

[56:43](https://www.youtube.com/watch?v=sLSTM9znQNs&t=3403s) How AI tools expand what engineers can do

[1:02:30](https://www.youtube.com/watch?v=sLSTM9znQNs&t=3750s) The Merge: ChatGPT + Codex

[1:07:16](https://www.youtube.com/watch?v=sLSTM9znQNs&t=4036s) How Tibo uses Codex and ChatGPT

[1:10:44](https://www.youtube.com/watch?v=sLSTM9znQNs&t=4244s) Advice for engineers who want to work in AI

**Where to find Tibo Sottiaux:**

• X: [https://x.com/thsottiaux](https://x.com/thsottiaux)

• LinkedIn: [https://www.linkedin.com/in/thibault-sottiaux-27195366](https://www.linkedin.com/in/thibault-sottiaux-27195366)

**Mentions during the episode:**

• How Codex is built: [https://newsletter.pragmaticengineer.com/p/how-codex-is-built](https://newsletter.pragmaticengineer.com/p/how-codex-is-built)

• Slow down to speed up: so much has changed in 6 months’ time: [https://newsletter.pragmaticengineer.com/p/slow-down-to-speed-up](https://newsletter.pragmaticengineer.com/p/slow-down-to-speed-up)

• N-Side: [https://www.n-side.com](https://www.n-side.com)

• Google DeepMind: [https://deepmind.google](https://deepmind.google)

• AlphaFold: [https://deepmind.google/science/alphafold](https://deepmind.google/science/alphafold)

• Tibo’s reply on X about Google’s canceled bot, LMChat:

[![X avatar for @thsottiaux](https://substackcdn.com/image/fetch/$s_!x-Vz!,w_120,h_120,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fpbs.substack.com%2Fprofile_images%2F2093807917833281537%2F2yBgpwVV.jpg)

Tibo @thsottiaux

@\_chenglou I was part of that team. Basically ChatGPT one year before it came out. Called LMChat and then another codename. Google was too nervous to release it and DeepMind was blocked from shipping products that could disrupt Google. I think about this a lot.

4:53 PM · Aug 1, 2026 · 1.97M Views

250 Replies · 566 Reposts · 12.6K Likes](https://x.com/thsottiaux/status/2083596911060324570)

• Greg Brockman on X: [https://x.com/gdb](https://x.com/gdb)

• Sam Altman on X: [https://x.com/sama](https://x.com/sama)

• Python: [https://www.python.org](https://www.python.org)

• Rust: [https://rust-lang.org](https://rust-lang.org)

• The creator of Clawd: “I ship code I don’t read”: [https://newsletter.pragmaticengineer.com/p/the-creator-of-clawd-i-ship-code](https://newsletter.pragmaticengineer.com/p/the-creator-of-clawd-i-ship-code)

• Using Goals in Codex: [https://developers.openai.com/cookbook/examples/codex/using\_goals\_in\_codex](https://developers.openai.com/cookbook/examples/codex/using_goals_in_codex)

• ChatGPT is now a partner for your most ambitious work: [https://openai.com/index/chatgpt-for-your-most-ambitious-work](https://openai.com/index/chatgpt-for-your-most-ambitious-work)

• ChatGPT Work: [https://chatgpt.com/work](https://chatgpt.com/work)

—

Production and marketing by [Pen Name](https://penname.co/) .
