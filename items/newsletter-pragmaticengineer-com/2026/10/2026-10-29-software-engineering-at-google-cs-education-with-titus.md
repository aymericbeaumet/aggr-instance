---
title: Software engineering at Google & CS education with Titus Winters
link: https://newsletter.pragmaticengineer.com/p/software-engineering-at-google-and
source: newsletter-pragmaticengineer-com
published: 2026-10-29T13:37:13Z
updated: 2026-10-29T13:37:13Z
first_seen: 2026-10-29T15:30:03.869733718Z
authors:
- Gergely Orosz
summary: Titus Winters shares lessons from engineering at Google’s scale and explores how AI is reshaping developer productivity and computer science education.
content: extracted
html: 2026-10-29-software-engineering-at-google-cs-education-with-titus.html
remote_preview:
  url: https://substackcdn.com/image/youtube/w_728,c_limit/mQu0zN4IvSI
extra:
  audio_type: audio/mpeg
  audio_url: https://api.substack.com/feed/podcast/222246289/6178354a121297edeffdeee4a9c630b8.mp3
  duration_seconds: 6012
---

*Before we start: I’m in SF this week and next, hosting these free-to-attend events (registration required). If you’re in the area, hopefully I'll see you at one of them:*

- **[The Pragmatic Engineer Unplugged](https://luma.com/pragmatic-engineer-unplugged)**: Wednesday (4 Nov), 5pm-7:30pm. With Diogo Almeida (creator of Jev & co-inventor of ChatGPT), and Bryan Cantrill (cofounder and CTO of Oxide). *We still have space left, [RSVP here](https://luma.com/pragmatic-engineer-unplugged).*

- **[System Update](https://luma.com/systemupdate)**: Thursday (5 Nov) 5pm-8pm. *We have some last spots left, [RSVP here](https://luma.com/systemupdate).*

**Listen and watch now on [YouTube](https://youtu.be/mQu0zN4IvSI), [Apple](https://podcasts.apple.com/us/podcast/the-pragmatic-engineer/id1769051199), and [Spotify](https://open.spotify.com/show/2Bho9xCbOQMWMJ7UKmqCzD).** See the episode transcript at the top of this page, and timestamps for the episode at the bottom.

![](https://substackcdn.com/image/fetch/$s_!Qhi0!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1228ff6a-514b-4643-9360-02cda14dc5ac_800x70.png)

• **[turbopuffer](http://turbopuffer.com/pragmatic)** – The tpuf eng team is completely redesigning their storage architecture from first principles. And they’re documenting *all* of it! They made epic progress, take a look: [turbopuffer.com/v3](http://turbopuffer.com/v3)

• **[Linear](https://linear.app/pragmatic)** – one of the big challenges for us devs is the sheer amount of code to review. Linear Diffs is a new feature to review PRs inside of Linear, with all the context in one place. It also supports stacked PRs. [Check it out](https://linear.app/pragmatic)

• **[WorkOS](https://workos.com/)** – WorkOS built Airlock, the authorization layer for AI agents. It evaluates every request against the agent’s intent + your rules and either allows it, denies it, or routes it to a human. Works with Claude Code, Codex & MCP gateways. [Give it a spin](http://workos.com/airlock)

An engineer who helped create today’s software engineering curriculum now says to burn down almost all of it. What’s the cause of this contradiction? We find out from the lead author of *[Software Engineering at Google](https://www.oreilly.com/library/view/software-engineering-at/9781492082781)*.

[Titus Winters](https://www.linkedin.com/in/tituswinters/) spent more than a decade at Google, where he led its readability program and built [Abseil](https://abseil.io/), Google’s open-sourced foundational C++ libraries. He is currently a Senior Principal Scientist at Adobe, where he focuses on developer experience and productivity.

In this episode of *The Pragmatic Engineer podcast*, Titus and I discuss how his team tackled a massive refactoring project across Google’s 250-million-line C++ codebase, why Google made testing mandatory, and his work on updating the ACM/IEEE undergraduate computer science curriculum guidelines.

We also explore why traditional productivity metrics miss some of AI’s biggest benefits, what needs to change in CS education, and how new engineers can stand out.

[www.youtube.com](https://www.youtube.com/watch?v=mQu0zN4IvSI)

Titus is a deep thinker and has worked on some very interesting projects. Here are 12 things that stick out to me from our chat.

**1\. Was refactoring 250M+ lines of C++ code the largest refactoring task of all time**? This happened at Google in 2010, a few months after Titus joined the search giant. An internal email asked for volunteers to help clean up old C++ code, some of it written by the very first Googlers.

A team of a dozen people was assembled. Google had close to 250M lines of code at the time, edited by 10,000 engineers. Titus recalled the cleanup: “We were replacing the foundation of the Empire State Building. We were not allowed to close the building or block traffic. Oh, and we discovered nuclear waste in the basement.”

**2\. The refactoring was done with advanced language parsing techniques, long before LLMs existed**. Back in 2010, there were no LLMs for this giant refactor. Here’s what the team did:

- **Parse**: ran a MapReduce over the whole codebase. The mapper parsed every build target into an abstract syntax tree

- **Match**: match exact call patterns in the tree

- **Code generation:** generated edits to be made by the machine

- **Coexist**: make the old API and new API work side-by-side

- **Shard**: once the new API is ready to go, split the edits by directory, and send them out in batches

- **Block the old**: make sure nobody copied the old pattern back into the code

- **Delete**: remove the old API

The [clang-format toolkit](https://clang.llvm.org/docs/ClangFormat.html) exists because Titus and his team needed it for this refactor.

**3\. Google made automated tests mandatory after Search could no longer ship regularly**. Around 2007, Search, the flagship product, was unable to ship at a monthly cadence any more, and the best they could ship to production was quarterly.

Almost every change at Google touched Search’s code, even things like a change to Gmail. To clamp down, Google, as an org, mandated that every change was to be accompanied by a test. Over time, Search returned to its former higher cadence.

**4\. Google’s readability program might have been the largest educational program ever in tech**. To make a code change at Google, two people need to sign off that change through a code review:

- An “owner” of the file/project: someone on the team with domain knowledge

- A “readability” reviewer: an engineer certified to be a “readability” expert in the given language that the change was made in

Google’s first engineer, Craig Silverstein, walked each new dev hire through their first change. Java teams turned this approach into mentoring, where experienced reviewers were assigned to review all changes in a round-robin fashion. And soon enough, a formalized readability program was built for all languages. When Titus left Google three months ago, 10,000+ engineers were enrolled in readability at any given time. Has there ever been a bigger engineering “classroom” anywhere else?

**5\. Fear is not an acceptable basis for engineering**. Titus believes your codebase is unsustainable if it’s not possible to make a valuable change safely. There shouldn’t be fear about making a change to a codebase. Having the right “security nets” that catch errors is strategically very valuable because it enables teams to make changes without fear.

**6\. With AI, outages seem to be going up**. Titus talks to engineers at other companies and it’s the same picture: code output is up hugely versus a year ago, and outages are also up about 30%, while the number of features shipped is static. It seems like treading water. From that angle, it’s not positive for AI; based on outages alone, the technology could be a negative.

**7\. On the other hand, AI makes ambitious projects possible that weren’t realistic without it**. Titus is optimistic about AI as a net productivity boost, and reckons people are caring about the wrong things. He sees the real value of AI as not in repeating the same things as before at higher speed. Instead, it’s about doing things you *never* thought of before: projects so ambitious that they didn’t used to make sense!

**8\. Burn down the US undergrad CS curriculum (most of it**). Titus was the only working engineer on the steering committee for the ACM/IEEE/AAAI CS curriculum guidelines in 2023, and believes the curriculum is hopelessly out of date. He accuses it of not having altered substantially since around 2000, while the industry has. Testing and verification are cited as mattering a lot more for new joiners, but they are absent from the curriculum.

Unless major changes are made to make it relevant, computer science enrollment could fall and software engineering programs that focus on practical dev skills will be attractive to students – this is Titus’s forecast.

**9\. “Middle group” of students struggle most with AI**. Professors in CS programs tell Titus that students divide into three groups:

- The best are better than ever and take full advantage of a patient, knowledgeable tutor, aka the AI

- The middle: professors say that everyone below the “best” have collapsed: students below a B grade tend to not understand code at all

- The weakest: they can catch up to the middle of the pack easier – mostly by producing slop with AI, though.

*When I did a guest lecture at my alma mater, professors told me similar observations – how the “middle of the pack students,” by grades, have disappointingly low understanding of the subject, compared to former years.*

**10\. Grading students’ prompts and tests is better than focusing on their code**. Titus would focus on the thinking processes students put into building and validating software, not just the output.

**11\. “Remember Andrew’s axe” – advice for junior engineers**. Andrew chops down a tree with an axe. Or did Andrew’s axe chop down the tree? Titus says Andrew is the one who did it, and the axe is only a tool for his will. Similarly with AI: it’s a tool for you to use, and anthropomorphizing this tool takes us out of that mindset.

**12\. To succeed, don’t copy what everyone else is doing**! As our time came to an end, I asked Titus what he’d say to sophomore students. It’s a segment that did not make it into the podcast, but I’m adding it here!

> “Most of my success in life has come from not doing the thing that everyone else is doing. It is really incredibly hard to stand out if you are just listening to what is a hot topic and chasing the same shiny thing that literally everyone else is doing.
>
> My primary advice \[is to\] find something that you love that is not just the same thing that you assume is on everybody’s resume, and do a lot of that. Find a passion project, go really deep on it.
>
> Like, ‘how do I use computing and AI vibe coded new generation techniques?’ ‘How do I solve this problem that matters a lot to me?’ ‘Or how do I express myself through a game or art?’ The more that you do \[something\] that’s uniquely *you*, the more that you *stand out,* and the more that you do, the more that you *learn*.”

• [The state of the tech industry in 2026](https://newsletter.pragmaticengineer.com/p/the-state-of-the-tech-industry-in)

• [Inside Google’s Engineering Culture](https://newsletter.pragmaticengineer.com/p/google)

• [Slow down to speed up when working with AI agents](https://newsletter.pragmaticengineer.com/p/ideas-slow-down-to-speed-up-when)

• [Measuring developer productivity? A response to McKinsey](https://newsletter.pragmaticengineer.com/p/measuring-developer-productivity)

• [How tech companies measure the impact of AI on software development](https://newsletter.pragmaticengineer.com/p/how-tech-companies-measure-the-impact-of-ai)

• [What is happening with code reviews?](https://newsletter.pragmaticengineer.com/p/what-is-happening-with-code-reviews)

• [When AI writes almost all code, what happens to software engineering?](https://newsletter.pragmaticengineer.com/p/when-ai-writes-almost-all-code-what)

• [Developer productivity with Dr. Nicole Forsgren (creator of DORA, co-creator of SPACE)](https://newsletter.pragmaticengineer.com/p/developer-productivity-with-dr-nicole)

• [Measuring the impact of AI on software engineering – with Laura Tacho](https://newsletter.pragmaticengineer.com/p/measuring-the-impact-of-ai-on-software)

[00:00](https://www.youtube.com/watch?v=mQu0zN4IvSI) Intro

[07:01](https://www.youtube.com/watch?v=mQu0zN4IvSI&t=421s) Getting hired by Google

[09:48](https://www.youtube.com/watch?v=mQu0zN4IvSI&t=588s) Finding his niche at Google

[14:32](https://www.youtube.com/watch?v=mQu0zN4IvSI&t=872s) Why Google made testing mandatory

[17:45](https://www.youtube.com/watch?v=mQu0zN4IvSI&t=1065s) Why new engineers fear making mistakes

[20:40](https://www.youtube.com/watch?v=mQu0zN4IvSI&t=1240s) Abseil: C++ foundations at Google

[23:09](https://www.youtube.com/watch?v=mQu0zN4IvSI&t=1389s) Google’s massive C++ refactoring

[30:48](https://www.youtube.com/watch?v=mQu0zN4IvSI&t=1848s) AI for rewrites

[39:07](https://www.youtube.com/watch?v=mQu0zN4IvSI&t=2347s&pp=0gcJCWMAwfN6Pr3D) Software Engineering at Google: the book

[43:10](https://www.youtube.com/watch?v=mQu0zN4IvSI&t=2590s) Google’s readability reviews

[47:18](https://www.youtube.com/watch?v=mQu0zN4IvSI&t=2838s) Why teams still matter

[52:41](https://www.youtube.com/watch?v=mQu0zN4IvSI&t=3161s) Psychological safety

[58:17](https://www.youtube.com/watch?v=mQu0zN4IvSI&t=3497s) Layoffs and culture

[1:02:11](https://www.youtube.com/watch?v=mQu0zN4IvSI&t=3731s) Measuring developer productivity

[1:11:17](https://www.youtube.com/watch?v=mQu0zN4IvSI&t=4277s) Rethinking CS education

[1:22:56](https://www.youtube.com/watch?v=mQu0zN4IvSI&t=4976s) What new grads should learn

[1:32:08](https://www.youtube.com/watch?v=mQu0zN4IvSI&t=5528s) Titus’s ideal software engineering curriculum

[1:36:27](https://www.youtube.com/watch?v=mQu0zN4IvSI&t=5787s&pp=0gcJCWMAwfN6Pr3D) Advice for breaking into tech

[1:38:35](https://www.youtube.com/watch?v=mQu0zN4IvSI&t=5915s) Book recommendation

**Where to find Titus Winters:**

• X: [https://x.com/TitusWinters](https://x.com/TitusWinters)

• LinkedIn: [https://www.linkedin.com/in/tituswinters](https://www.linkedin.com/in/tituswinters)

• GitHub: [https://github.com/tituswinters](https://github.com/tituswinters)

**Mentions during the episode:**

• QBasic:[https://en.wikipedia.org/wiki/QBasic](https://en.wikipedia.org/wiki/QBasic)

• Harvey Mudd College: [https://www.hmc.edu](https://www.hmc.edu)

• UC Riverside: [https://ucr.edu](https://ucr.edu)

• Universal Plug and Play (UPnP):[https://en.wikipedia.org/wiki/Universal_Plug_and_Play](https://en.wikipedia.org/wiki/Universal_Plug_and_Play)

• Remote procedure call (RPC):[https://en.wikipedia.org/wiki/Remote_procedure_call](https://en.wikipedia.org/wiki/Remote_procedure_call)

• Software Engineering at Google by Titus Winters, Tom Manshreck, and Hyrum Wright:[O’Reilly](https://www.oreilly.com/library/view/software-engineering-at/9781492082781/) ·[Free online edition](https://abseil.io/resources/swe-book)

• Abseil:[Website](https://abseil.io) ·[GitHub](https://github.com/abseil/abseil-cpp)

• The Pulse: What can we learn from Bun’s rapid Rust rewrite with AI?:[https://newsletter.pragmaticengineer.com/p/the-pulse-what-can-we-learn-from](https://newsletter.pragmaticengineer.com/p/the-pulse-what-can-we-learn-from)

• Bun: lessons from disrupting a tech ecosystem:[https://blog.pragmaticengineer.com/bun-lessons-from-disrupting](https://blog.pragmaticengineer.com/bun-lessons-from-disrupting)

• Clang: [https://clang.llvm.org](https://clang.llvm.org)

• Abstract syntax tree (AST):[https://en.wikipedia.org/wiki/Abstract_syntax_tree](https://en.wikipedia.org/wiki/Abstract_syntax_tree)

• MapReduce:[https://en.wikipedia.org/wiki/MapReduce](https://en.wikipedia.org/wiki/MapReduce)

• ClangFormat:[https://clang.llvm.org/docs/ClangFormat.html](https://clang.llvm.org/docs/ClangFormat.html)

• OpenRewrite:[Documentation](https://docs.openrewrite.org) ·[GitHub](https://github.com/openrewrite/rewrite)

• BrontoSource: [https://www.brontosource.dev/](https://www.brontosource.dev/)

• ZTA: Zero Token Architecture – Kelsey Hightower | PlatformCon 2026:

[www.youtube.com](https://www.youtube.com/watch?v=A7WFt2JQ5sg)

• Kubernetes and retiring at the top with Kelsey Hightower:[https://newsletter.pragmaticengineer.com/p/kubernetes-and-retiring-at-the-top](https://newsletter.pragmaticengineer.com/p/kubernetes-and-retiring-at-the-top)

• Google SRE books:[https://sre.google/books/](https://sre.google/books/)

• Hyrum Wright’s website: [https://www.hyrumwright.org](https://www.hyrumwright.org)

• Tom Manshreck on O’Reilly:[https://oreilly.com/pub/au/7955](https://oreilly.com/pub/au/7955)

• Craig Silverstein:[https://en.wikipedia.org/wiki/Craig_Silverstein](https://en.wikipedia.org/wiki/Craig_Silverstein)

• Building Claude Code with Boris Cherny:[https://newsletter.pragmaticengineer.com/p/building-claude-code-with-boris-cherny](https://newsletter.pragmaticengineer.com/p/building-claude-code-with-boris-cherny)

• DORA: [https://dora.dev](https://dora.dev)

• Blameless postmortem culture (Google SRE book):[https://sre.google/sre-book/postmortem-culture/](https://sre.google/sre-book/postmortem-culture/)

• Dr. Cat Hicks’ website: [https://www.drcathicks.com](https://www.drcathicks.com)

• The Psychology of Software Teams by Cat Hicks:[https://www.amazon.com/dp/B0GQTCL1J4](https://www.amazon.com/dp/B0GQTCL1J4)

• Platform Engineering at Google:[https://www.oreilly.com/library/view/platform-engineering-at/9781098169428/](https://www.oreilly.com/library/view/platform-engineering-at/9781098169428/)

• The Mythical Man-Month:[https://www.oreilly.com/library/view/mythical-man-month-the/0201835959/](https://www.oreilly.com/library/view/mythical-man-month-the/0201835959/)

• Prisoner’s dilemma (iterated):[https://en.wikipedia.org/wiki/Prisoner%27s_dilemma](https://en.wikipedia.org/wiki/Prisoner%27s_dilemma)

• Adobe Names Anil Chakravarthy as CEO, Replacing Shantanu Narayen:[https://daringfireball.net/linked/2026/09/04/adobe-new-ceo](https://daringfireball.net/linked/2026/09/04/adobe-new-ceo)

• Yes, you can measure software developer productivity (McKinsey):[https://www.mckinsey.com/capabilities/people-and-organization/our-insights/yes-you-can-measure-software-developer-productivity](https://www.mckinsey.com/capabilities/people-and-organization/our-insights/yes-you-can-measure-software-developer-productivity)

• Measuring developer productivity? A response to McKinsey: • [https://newsletter.pragmaticengineer.com/p/measuring-developer-productivity](https://newsletter.pragmaticengineer.com/p/measuring-developer-productivity)

• Measuring developer productivity? A response to McKinsey, Part 2:[https://newsletter.pragmaticengineer.com/p/measuring-developer-productivity-part-2](https://newsletter.pragmaticengineer.com/p/measuring-developer-productivity-part-2)

• How Kent Beck shapes the software engineering industry:[YouTube](https://www.youtube.com/watch?v=ddHQQtjIOpw) ·[Newsletter](https://newsletter.pragmaticengineer.com/p/how-kent-beck-shapes-the-software)

TDD, AI agents and coding with Kent Beck:[https://newsletter.pragmaticengineer.com/p/tdd-ai-agents-and-coding-with-kent](https://newsletter.pragmaticengineer.com/p/tdd-ai-agents-and-coding-with-kent)

• Wisdom of the crowd:[https://en.wikipedia.org/wiki/Wisdom_of_the_crowd](https://en.wikipedia.org/wiki/Wisdom_of_the_crowd)

• Develop, Deploy, Operate: A holistic model for understanding the costs and value of software development:[https://queue.acm.org/doi/full/10.1145/3733703](https://queue.acm.org/doi/full/10.1145/3733703)

• CS2023: ACM/IEEE-CS/AAAI Computer Science Curricula: [https://csed.acm.org/](https://csed.acm.org/)

• Distributed databases with Peter Mattis:[https://newsletter.pragmaticengineer.com/p/distributed-databases-with-peter](https://newsletter.pragmaticengineer.com/p/distributed-databases-with-peter)

• Cockroach Labs: [https://www.cockroachlabs.com](https://www.cockroachlabs.com)

• GIMP: [https://www.gimp.org](https://www.gimp.org)

• GTK: [https://www.gtk.org](https://www.gtk.org)

• LeetCode: [https://leetcode.com](https://leetcode.com)

• Outliers: The Story of Success by Malcolm Gladwell (the 10,000-hour rule):[Amazon](https://www.amazon.com/dp/0316017930) ·[Wikipedia](https://en.wikipedia.org/wiki/Outliers_\(book\))

• University of Waterloo co-operative education:[https://uwaterloo.ca/co-operative-education/](https://uwaterloo.ca/co-operative-education/)

• A computer can never be held accountable (the 1979 IBM training slide):[https://simonwillison.net/2025/Feb/3/a-computer-can-never-be-held-accountable/](https://simonwillison.net/2025/Feb/3/a-computer-can-never-be-held-accountable/)

• Computing Education When Writing Code Is No Longer the Challenge:[Communications of the ACM](https://cacm.acm.org/opinion/computing-education-when-writing-code-is-no-longer-the-challenge/) ·[ACM Digital Library](https://dl.acm.org/doi/10.1145/3808702)

—

Production and marketing by [Pen Name](https://penname.co/).
