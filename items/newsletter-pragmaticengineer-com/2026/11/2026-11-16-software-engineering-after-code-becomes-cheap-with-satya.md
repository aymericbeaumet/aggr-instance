---
title: Software engineering after code becomes cheap with Satya Nadella
link: https://newsletter.pragmaticengineer.com/p/software-engineering-after-code-becomes
source: newsletter-pragmaticengineer-com
published: 2026-11-16T13:33:48Z
updated: 2026-11-16T13:33:48Z
first_seen: 2026-11-16T17:21:52.095799575Z
authors:
- Gergely Orosz
summary: Microsoft CEO Satya Nadella on AI’s impact on software engineering, code verification, and engineers’ careers.
content: extracted
html: 2026-11-16-software-engineering-after-code-becomes-cheap-with-satya.html
remote_preview:
  url: https://substackcdn.com/image/youtube/w_728,c_limit/0NE89Nzfk20
extra:
  audio_type: audio/mpeg
  audio_url: https://api.substack.com/feed/podcast/225062889/9e49975a33efeb1a48ac6716f97a6c3d.mp3
  duration_seconds: 3760
---

**Listen and watch now on [YouTube](https://youtu.be/0NE89Nzfk20), [Apple](https://podcasts.apple.com/us/podcast/the-pragmatic-engineer/id1769051199), and [Spotify](https://open.spotify.com/show/2Bho9xCbOQMWMJ7UKmqCzD).** See the episode transcript at the top of this page, and timestamps for the episode at the bottom.

![](https://substackcdn.com/image/fetch/$s_!PDDS!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa3e14da4-b493-4b54-b18d-0b618a4a59f4_1600x140.png)

• **[turbopuffer](http://turbopuffer.com/pragmatic)** – a ridiculously scalable, fast and cheap hybrid search engine, built on top of object storage, used by cutting-edge companies like Anthropic, Linear, Ramp and many others. Many companies that switch to turbopuffer are cutting their search bills by 50% or more and getting much faster results. [Check it out](http://turbopuffer.com/pragmatic). *Also, check out the new and improved [The Pragmatic Engineer search I built](http://pragmaticengineer.com/search) on top of turbopuffer!*

• **[Linear](https://linear.app/pragmatic)** – one of the big challenges for us devs is the sheer amount of code to review. Linear Diffs is a new feature for reviewing PRs inside of Linear, with all the context in one place. It also supports stacked PRs. [Check it out](https://linear.app/pragmatic)

• **[WorkOS](https://workos.com/)** – WorkOS built Airlock, the authorization layer for AI agents. It evaluates every request against the agent’s intent + your rules, and either allows it, denies it, or routes it to a human. Works with Claude Code, Codex & MCP gateways. [Learn more](http://workos.com/airlock)

What does software engineering look like when generating the code becomes cheap and verifying it is the hard part?

There can be few people in tech more qualified to tackle this hot topic than Satya Nadella. Microsoft’s CEO started his career as a software engineer – a rare thing among Big Tech CEOs. Satya joins me to discuss how building software is changing at Microsoft, and what it means for us, software engineers.

We start with Microsoft’s embrace of open source from the mid-2010s and the lessons learned from building VS Code in the open. I ask how Microsoft went from holding an early lead with GitHub Copilot to having to catch up with Cursor and Claude Code, and what it reveals about building products around rapidly improving models.

Satya also explains what a “software factory” means to him, why specifications and verification are becoming more important, and how teams that work on products like the Windows operating system are adapting. We also touch on Microsoft’s voluntary exit offers, AI-related anxiety around engineering careers, and hear Nadella caution against drawing long-term conclusions from the slowdown in junior hiring.

We also explore open and local models, why companies need to control their own AI infrastructure, and how to build safeguards around increasingly capable agents. Satya also has some advice for engineers who want to excel during this period of change – and why he would like to spend more time on building.

*Sidenote: we recorded this episode in Redmond just a few hours before Microsoft’s day-long Outlook outage, so there was no discussion of it. To the people [asking](https://x.com/kanateven/status/2121097939913179572?a=PYx_t9nE1mtrVWPvGWE&s=20): no, I did not pull out any cables during my visit that might have kicked out that outage :) However, I am already talking with the team inside Microsoft on-point for this outage, and expect a more detailed follow-up in the newsletter on what went wrong and what we can learn from it.*

[www.youtube.com](https://www.youtube.com/watch?v=0NE89Nzfk20)

I appreciated how grounded and thoughtful Satya was – and how pragmatic he is in suggesting to *verify* anything and everything related with LLMs, and not blindly trust anything or anyone (including vendors or AI models!) 12 of the most interesting takeaways I noted:

**1\. A new form of software engineering: teams building software factories to improve their work**. Satya cites the Azure networking team which manages 500+ fiber network operators and recently built an agentic system to better manage emails and processes. Now, they operate much more efficiently as a result. Satya sees this as the reality of “AI transformation”: brownfield work on building software factories to improve how teams operate, all while continuing the day-to-day work.

**2\. GitHub Copilot fell behind Claude Code because the IDE faded out in just six months**. Dev tools are in Microsoft’s DNA, but it was completely unprepared for Claude Code retiring the concept of the IDE in favor of the command line. Satya admits this move by Anthropic caught them off guard, and it taught him that when a non-linear model jump occurs, the form factor of using that model can change fast. He also admits the IDE “faded away” over the course of just six months, and Microsoft’s stronghold in dev tools had been the IDE, with VS Code. *It sounds like Microsoft learned a lesson here and is now more nimble with dev tools, and less attached to past form factors.*

**3\. “Own the intelligence”: avoid lock-in & treat agents as an insider risk**. Satya cautions companies to avoid lock-in with any single model provider by ensuring that the harness, memory, and model are not coupled, so that they can be changed with ease. He also thinks more companies should consider using open models as a way to protect their core intellectual property (IP) from leaking out, and build private evals to decide what models to use and upgrade to. He also says that models are intelligent but unreliable, so it’s best to treat them as “[insider risks](https://x.com/satyanadella/article/2108931348857827686)” and build a separate system to verify that agentic systems work as expected.

**4\. Microsoft is betting on “hybrid intelligence**.” Satya questions why we rely on the cloud for AI infrastructure when last year’s frontier-equivalent model can already run on a laptop, while the frontier-equivalent model from as recently as the previous quarter can run on desktops. He predicts the concept of “hybrid intelligence” will gain traction, of having local AI models running on laptops and desktops, and a model router deciding when to use cheap, “unmetered” local models, and when to route to a more capable, expensive one in the cloud. Microsoft is building infra to support such routing with its Microsoft AI (MAI) models and [Surface Laptop Ultra](https://www.microsoft.com/en-us/surface/devices/surface-laptop-ultra) lineups.

**5\. Code generation has become cheap, but verification is still hard**. Satya reveals he’s been thinking a lot lately about how challenging it is to verify the correctness of code generated today. Models doing a “critique” on generated code is one solution, but it involves model alignment issues, such as a model achieving its goal in undesirable ways; for example, making tests pass by deleting failing tests. Satya also thinks we’ll start to build more verification loops based on production traces, in a kind of “recursive self-improvement” (RSI). But for this, models need to be able to verify fixes, meaning we return to the verification problem! *We didn’t arrive at any definite solutions for this during the conversation, which tells me this is a problem space that will keep many of us engineers busy!*

**6\. What’s with those voluntary exit offers**? I asked Satya directly why Microsoft recently made voluntary exit offers to most of its software engineers for the first time in the company’s history. I also wanted to know if it could mean layoffs in the future. Unsurprisingly, his reply was diplomatic, and my more direct interpretation is this:

- Large transformations/reorgs are likely because teams will change in size (many getting smaller) and function, with many new AI-related teams being formed.

- There’s high demand internally for new, AI-related roles like forward-deployed engineers.

- A voluntary exit offer can motivate engineers who feel “stuck” on their current team (for example, if there’s less opportunity to do AI-related work) to take interviews elsewhere, and, with an offer in hand, depart with a smaller “bonus.” Satya admits that even inside Microsoft, not all teams got the memo about becoming more AI-native. *Those affected can accept a voluntary exit offer until December, and it’s safe to assume many are indeed interviewing at other places!*

- There is a lot of change ahead. *My take is that any “pre-AI” company will have to restructure itself, not just Microsoft. At least Satya is candid about it.*

**7\. The cause of the junior hiring slowdown is that companies are figuring out the new way to work**. I asked if Microsoft is hiring new grads and juniors, and Satya admits there’s a slowdown there (and at many places). He identifies a process that’s currently taking place as the reason why:

- There’s an internal reorg happening on team sizes and roles

- Teams are figuring out how to build “software factories” and how to balance human cost/effort and token budgets

- When all this is figured out, *then* junior recruitment will pick back up

At Microsoft, most teams are busy coming up with new ways of working, so it’s not a good time to bring in new hires. Satya expects this “reorg” phase to be temporary, after which hiring will pick up.

**8\. Microsoft has started a “preceptorship” program for AI-accelerated development**. In collaboration with Carnegie Mellon University, Microsoft launched the [PRAISE program](https://commandline.microsoft.com/praise-preceptor-ai-accelerated-software-engineering-cmu/) (Preceptorship for AI-accelerated Software Engineering). It’s actually a concept borrowed from nursing, where new nurses are paired with experienced ones to develop real-world experience and confidence. Preceptors are experienced software engineers who coach “preceptees” on using AI tools thoughtfully, while building strong systems thinking, taste, and judgement. *This is the kind of program I wish more companies would consider partnering with educational institutions on; perhaps this one will inspire other engineering leaders.*

**9\. The next big engineering challenge: building “software factories” within teams and companies**. Satya believes every company needs to control their AI infra – the harness, context, memory, model – and that verification of the generated code is a hard, currently unsolved, problem. Inside Microsoft, he sees lots of teams building their own “software factories” to improve team and org operations. As we talked, it became evident that building software factories looks set to be a widespread engineering problem across the industry because it’s a novel challenge without many existing “blueprints”. It will be slightly different for every team and company. There’s going to be lots of work in this space!

**10\. “Migrating” to AI is quite different from migrating to a new technology**. We talk about how “migrating” to become “AI-native” feels similar to a brownfield project introducing a new technology, such as moving over from HTML+JavaScript to React. Satya also perceives some differences:

- All tools are changing, not just a single technology: the runtime, the framework, and the concept of the IDE

- When a new model launches with new capabilities, it can invalidate existing setups, just as models like Opus 4.5-and-above have invalidated IDEs.

**11\. Satya is very enthusiastic about decision models**. Satya loves how decision models like ‘Jev’ and Microsoft’s ‘Decision-1’ are a kind of new primitive. He says: “These decision models are kind of like Excel macros. What happened to spreadsheets with macros can happen with code with intelligence embedded as a primitive. This primitive has a distribution, it doesn’t need to be generated again and again, it’s token-efficient. I think that this is one of those places where software engineering remains very much alive. Decision models are like the stored procedure of the intelligence era.”

**12\. Satya’s advice for winning with AI: stop reading & listening about it, and start using it more**. He says:

> “It’s ironic on a podcast to say, but it’s “do more, learn more, and be hands-on.” Experience it firsthand. If I could change the ratio of my “talk” to “do,” I would. This is the time. If there’s one piece of advice – whether you’re mid-career, a CEO, or a fresh college grad – it’s that the more you can take the time to learn, to be in teams that are doing it, then the better that is versus anything else I can think of.”

• [How will AI change operating systems? A deepdive into Windows](https://newsletter.pragmaticengineer.com/p/windows-and-ai)

• [A pragmatic guide to LLM evals for devs](https://newsletter.pragmaticengineer.com/p/evals)

• [What is “loop engineering?”](https://newsletter.pragmaticengineer.com/p/what-is-loop-engineering)

• [What is inference engineering? A deepdive](https://newsletter.pragmaticengineer.com/p/what-is-inference-engineering)

• [What are Forward Deployed Engineers, and why are they so in demand?](https://newsletter.pragmaticengineer.com/p/forward-deployed-engineers)

• [What is happening with code reviews?](https://newsletter.pragmaticengineer.com/p/what-is-happening-with-code-reviews)

• [Building MCP servers in the real world](https://newsletter.pragmaticengineer.com/p/mcp-deepdive)

• [Microsoft is dogfooding AI dev tools’ future](https://newsletter.pragmaticengineer.com/p/microsoft-ai-dev-tools)

[00:00](https://www.youtube.com/watch?v=0NE89Nzfk20) Intro

[02:12](https://www.youtube.com/watch?v=0NE89Nzfk20&t=132s) Microsoft’s open-source shift

[04:15](https://www.youtube.com/watch?v=0NE89Nzfk20&t=255s) Building VS Code in the open

[05:55](https://www.youtube.com/watch?v=0NE89Nzfk20&t=355s) Copilot, Cursor, and Claude Code

[09:52](https://www.youtube.com/watch?v=0NE89Nzfk20&t=592s) Decision engines

[12:32](https://www.youtube.com/watch?v=0NE89Nzfk20&t=752s) Verifying AI-generated code

[15:39](https://www.youtube.com/watch?v=0NE89Nzfk20&t=939s) The software factory

[20:45](https://www.youtube.com/watch?v=0NE89Nzfk20&t=1245s) Bringing AI to existing products

[24:06](https://www.youtube.com/watch?v=0NE89Nzfk20&t=1446s) Why adopting AI is different to adopting React

[26:29](https://www.youtube.com/watch?v=0NE89Nzfk20&t=1589s) The changing role of software engineers

[29:25](https://www.youtube.com/watch?v=0NE89Nzfk20&t=1765s&pp=0gcJCWMAwfN6Pr3D) Microsoft’s voluntary exit offers

[34:05](https://www.youtube.com/watch?v=0NE89Nzfk20&t=2045s) AI anxiety and changing roles

[36:35](https://www.youtube.com/watch?v=0NE89Nzfk20&t=2195s) Junior hiring

[42:36](https://www.youtube.com/watch?v=0NE89Nzfk20&t=2556s) Open models

[45:39](https://www.youtube.com/watch?v=0NE89Nzfk20&t=2739s) Owning your AI infrastructure

[51:21](https://www.youtube.com/watch?v=0NE89Nzfk20&t=3081s) Building trust in AI agents

[53:48](https://www.youtube.com/watch?v=0NE89Nzfk20&t=3228s) Local models and hybrid intelligence

[56:39](https://www.youtube.com/watch?v=0NE89Nzfk20&t=3399s) Microsoft’s model strategy

[59:26](https://www.youtube.com/watch?v=0NE89Nzfk20&t=3566s) Advice for engineers

[1:01:21](https://www.youtube.com/watch?v=0NE89Nzfk20&t=3681s) Closing takeaways

**Where to find Satya Nadella:**

• X:[https://x.com/satyanadella](https://x.com/satyanadella)

• LinkedIn:[https://www.linkedin.com/in/satyanadella](https://www.linkedin.com/in/satyanadella)

• Blog (sn scratchpad): [https://snscratchpad.com/](https://snscratchpad.com/)

**Mentions during the episode:**

• Visual Studio Code: [https://code.visualstudio.com/](https://code.visualstudio.com/)

• Visual Studio: [https://visualstudio.microsoft.com/](https://visualstudio.microsoft.com/)

• TypeScript: [https://www.typescriptlang.org/](https://www.typescriptlang.org/)

• Git: [https://git-scm.com/](https://git-scm.com/)

• Microsoft’s acquisition of GitHub:[https://blogs.microsoft.com/blog/2018/10/26/microsoft-completes-github-acquisition](https://blogs.microsoft.com/blog/2018/10/26/microsoft-completes-github-acquisition/?utm_source=chatgpt.com)

• SQL Server on Linux:[https://learn.microsoft.com/en-us/sql/linux/sql-server-linux-overview](https://learn.microsoft.com/en-us/sql/linux/sql-server-linux-overview)

• GitHub Copilot:[https://github.com/features/copilot](https://github.com/features/copilot?utm_source=chatgpt.com)

• Cursor: [https://cursor.com/](https://cursor.com/)

• Claude Code:[https://claude.com/product/claude-code](https://claude.com/product/claude-code?utm_source=chatgpt.com)

• Introducing System One Models & Jev, by Diogo Almeida:[https://typesafe.ai/blog/introducing-system-one-models-and-jev](https://typesafe.ai/blog/introducing-system-one-models-and-jev)

• Introducing Microsoft-Decision-1, by Achint Srivastava:[https://commandline.microsoft.com/microsoft-decision-1-model-foundry/](https://commandline.microsoft.com/microsoft-decision-1-model-foundry/?utm_source=chatgpt.com)

• Nine engineers, 1,211 pull requests in a day: Lessons from building Copilot Cowork, by Bas Brekelmans:[https://commandline.microsoft.com/how-we-built-copilot-cowork-python-to-rust-migration/](https://commandline.microsoft.com/how-we-built-copilot-cowork-python-to-rust-migration/?utm_source=chatgpt.com)

• Migrating the GitHub Copilot runtime to Rust, using Copilot:[https://github.blog/ai-and-ml/generative-ai/migrating-the-github-copilot-runtime-to-rust-using-copilot/](https://github.blog/ai-and-ml/generative-ai/migrating-the-github-copilot-runtime-to-rust-using-copilot/?utm_source=chatgpt.com)

• Rust: [https://rust-lang.org/](https://rust-lang.org/)

• PRAISE: What will new developers do when AI writes the code?:[https://commandline.microsoft.com/praise-preceptor-ai-accelerated-software-engineering-cmu/](https://commandline.microsoft.com/praise-preceptor-ai-accelerated-software-engineering-cmu/?utm_source=chatgpt.com)

• Microsoft and CMU Team Up to Train Early Career Software Engineers: [https://s3d.cmu.edu/news/2026/1015-microsoft-cmu-praise.html](https://s3d.cmu.edu/news/2026/1015-microsoft-cmu-praise.html)

• Amanda Silver:[https://azure.microsoft.com/en-us/blog/author/amanda-silver/](https://azure.microsoft.com/en-us/blog/author/amanda-silver/?utm_source=chatgpt.com)

• Diogo Almeida:[https://typesafe.ai/team](https://typesafe.ai/team)

• From Copilots to Coworkers: How AI Agents Are Transforming Azure Networking Operations:[https://techcommunity.microsoft.com/blog/appsonazureblog/from-copilots-to-coworkers-how-ai-agents-are-transforming-azure-networking-opera/4519850](https://techcommunity.microsoft.com/blog/appsonazureblog/from-copilots-to-coworkers-how-ai-agents-are-transforming-azure-networking-opera/4519850)

• React: [https://react.dev/](https://react.dev/)

• Microsoft Foundry:[https://azure.microsoft.com/en-us/products/ai-foundry](https://azure.microsoft.com/en-us/products/ai-foundry?utm_source=chatgpt.com)

• Introducing the Microsoft Copilot harness in Foundry Agent Service:[https://devblogs.microsoft.com/foundry/microsoft-copilot-harness-in-foundry/](https://devblogs.microsoft.com/foundry/microsoft-copilot-harness-in-foundry/?utm_source=chatgpt.com)

• Pi: open source coding agent and agent toolkit: [https://github.com/earendil-works/pi](https://github.com/earendil-works/pi)

• Mark Russinovich:[https://markrussinovich.com/about-mark/](https://markrussinovich.com/about-mark/?utm_source=chatgpt.com)

• Scott Hanselman: [https://www.hanselman.com/](https://www.hanselman.com/)

• Dave Cutler:[https://news.microsoft.com/features/the-engineers-engineer-computer-industry-luminaries-salute-dave-cutlers-five-decade-long-quest-for-quality/](https://news.microsoft.com/features/the-engineers-engineer-computer-industry-luminaries-salute-dave-cutlers-five-decade-long-quest-for-quality/)

• The Use of Knowledge in Society, by Friedrich A. Hayek:[https://www.econlib.org/library/Essays/hykKnw.html](https://www.econlib.org/library/Essays/hykKnw.html?utm_source=chatgpt.com)

• Microsoft Ignite: [https://ignite.microsoft.com/](https://ignite.microsoft.com/)

• Surface Laptop Ultra:[https://www.microsoft.com/en-us/surface/devices/surface-laptop-ultra](https://www.microsoft.com/en-us/surface/devices/surface-laptop-ultra?utm_source=chatgpt.com)

• Building Windows for hybrid intelligence:[https://blogs.windows.com/windowsexperience/2026/10/07/building-windows-for-hybrid-intelligence/](https://blogs.windows.com/windowsexperience/2026/10/07/building-windows-for-hybrid-intelligence/?utm_source=chatgpt.com)

• GitHub Copilot Fusion: model routing and orchestration:[https://github.blog/changelog/2026-10-28-route-and-orchestrate-models-with-github-copilot-fusion/](https://github.blog/changelog/2026-10-28-route-and-orchestrate-models-with-github-copilot-fusion/)

• MAI Code 1.1 Flash:[https://microsoft.ai/news/mai-code-1-1-flash-br-better-faster-at-a-quarter-of-the-cost/](https://microsoft.ai/news/mai-code-1-1-flash-br-better-faster-at-a-quarter-of-the-cost/)

• Bringing local models and sandboxed tools to Windows and GitHub Copilot:[https://commandline.microsoft.com/local-models-sandboxed-tools-github-windows/](https://commandline.microsoft.com/local-models-sandboxed-tools-github-windows/?utm_source=chatgpt.com)

• MAI-Base-2: Frontier Pre-training at Microsoft AI:[https://microsoft.ai/news/frontier-pre-training-at-microsoft-ai/](https://microsoft.ai/news/frontier-pre-training-at-microsoft-ai/)

• Erich Gamma:[https://github.com/egamma](https://github.com/egamma?utm_source=chatgpt.com)

• Achint Srivastava:[https://commandline.microsoft.com/author/achint-srivastava/](https://commandline.microsoft.com/author/achint-srivastava/?utm_source=chatgpt.com)

• Bas Brekelmans: [https://commandline.microsoft.com/author/bas-brekelmans/](https://commandline.microsoft.com/author/bas-brekelmans/)

—

Production and marketing by [Pen Name](https://penname.co/).
