---
title: Formal methods with Hillel Wayne
link: https://newsletter.pragmaticengineer.com/p/formal-methods-with-hillel-wayne
source: newsletter-pragmaticengineer-com
published: 2026-07-29T16:22:31Z
updated: 2026-07-29T16:22:31Z
first_seen: 2026-09-19T21:30:23.395188495Z
authors:
- Gergely Orosz
summary: Hillel Wayne explains why formal methods like TLA+ matter, how they help build reliable software, and whether AI will finally bring formal verification into the mainstream.
content: extracted
html: 2026-07-29-formal-methods-with-hillel-wayne.html
preview:
  file: 2026-07-29-formal-methods-with-hillel-wayne.preview-9bb6dc497874.webp
  width: 256
  height: 144
  color: '#c5c0be'
images:
- source: https://substackcdn.com/image/youtube/w_728,c_limit/KSkcgIYQy0U
  original:
    file: 2026-07-29-formal-methods-with-hillel-wayne.image-b1d6bcf51d9e.jpg
    width: 728
    height: 410
  color: '#f0f0f0'
- source: https://substackcdn.com/image/fetch/$s_!YCsL!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F39bdfbb4-fb73-41d8-83cb-0ca4832aed49_1280x80.png
  original:
    file: 2026-07-29-formal-methods-with-hillel-wayne.image-1286b880690d.jpg
    width: 1280
    height: 80
  color: '#fdfdfd'
extra:
  audio_url: https://api.substack.com/feed/podcast/208490793/39e73f549d0e5b122bbc9dfe29a9b11a.mp3
---

**Listen and watch now on [YouTube](https://youtu.be/KSkcgIYQy0U) , [Apple](https://podcasts.apple.com/us/podcast/formal-methods-with-hillel-wayne/id1769051199?i=1000778935586) and [Spotify](https://open.spotify.com/episode/7pF97J4VB96PpLJdPShK7W) .** See the episode transcript at the top of this page, and timestamps for the episode at the bottom.

[![](https://substackcdn.com/image/fetch/$s_!YCsL!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F39bdfbb4-fb73-41d8-83cb-0ca4832aed49_1280x80.png)](https://substackcdn.com/image/fetch/$s_!YCsL!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F39bdfbb4-fb73-41d8-83cb-0ca4832aed49_1280x80.png)

**• [Antithesis](https://antithesis.com/pragmatic)** — Turbocharge testing of your systems by running your whole system under aggressive fault injection. There’s good reason teams like Jane Street, Fly.io, and the etcd community rely on Antithesis. [Learn more.](https://antithesis.com/pragmatic)

**• [WorkOS](https://workos.com/)** – make your app and agents Enterprise Ready, with SSO, SCIM, RBAC, and more. [Get started](https://workos.com/) .

**• [turbopuffer](https://turbopuffer.com/pragmatic)** – A vector and full-text search engine built on object storage. It’s fast, cheap, and extremely scalable. I met their team in San Francisco, and am a fan of their “hardcore and whimsical” engineering culture, and how pragmatic their engineering philosophy is. [Check them out.](https://turbopuffer.com/pragmatic)

There’s a popular theory that AI will finally make formal verification mainstream because mathematical proof of correctness will be needed when machines write most or all of the code. But will this happen? Today, I’m talking with one of the best people to tackle the prediction. Hillel Wayne is a formal methods consultant, educator, and author (his most recent book being [Logic for Programmers](https://logicforprogrammers.com) ), who’s deeply interested in software history.

In this episode of The Pragmatic Engineer podcast, I sit down with Hillel to compare software engineering with traditional engineering, discuss where formal methods fit into modern software development, and we explore why they are essential for some of the world’s most complex systems. We cover the formal specification language, TLA+, walk through several formal verification tools, examine why distributed systems are so difficult to reason about, and look into whether AI will make formal methods accessible to more engineering teams.

**1\. Are we “real” engineers? After thorough research, Hillel has an answer.** For [The Crossover Project](https://www.hillelwayne.com/tags/crossover-project/) , Hillel interviewed ~20 people in different fields of traditional engineering and software engineering, and found plenty of similarities and differences. He concluded that the rigor needed in software engineering means we earn the right to the title of “engineer.”

**2\. Version control is unique to software engineering.** Other fields of engineering have change management, but “traditional” engineers wish the concept of version control in software engineering existed in their fields because it’s far more sophisticated.

**3\. TLA+ is a formal specification language created by Leslie Lamport for designing and verifying systems.** Lamport is a mathematician and creator of LaTeX, who wanted to create a language for modeling complex systems. The language represents the state machine of the system and every possible state it can transition to. From the initial state, the system enumerates to get to every reachable state and checks whether properties defined upfront apply to those states. In this episode, Hillel walks us through a demo with TLA+.

**4\. Amazon used TLA+ to find a bug almost impossible to locate without formal methods.** In the paper [How AWS uses formal methods](https://cacm.acm.org/research/how-amazon-web-services-uses-formal-methods/) , the AWS team shared that they’d found a complicated bug for which the shortest error trace to exhibit was 35 steps (!!). The bug passed unnoticed through extensive design review, code reviews, and testing. AWS concluded they wouldn’t have uncovered it if they’d stuck to conventional testing approaches.

**5\. Lack of practice makes most engineers bad at dealing with concurrency problems and race conditions.** When a system has a race condition due to your code, you usually don’t find out until a few months later – if ever! In contrast, a system modeled in TLA+ can tell you about race conditions as soon as the tool is run, making it a fast feedback loop.

**6\. Why not use formal verification for everything, then?** It’s because specs in the real world are a nightmare to write. Even a simple problem like “find the file in a directory that has the most lines” gets complicated when modeled with formal methods. We would have to answer questions like: ‘do we look at ASCII or UTF-8 new line characters, what about unreadable files, and [Symlinks](https://en.wikipedia.org/wiki/Symbolic_link) ?’ Without formal methods, we can write a simple verification that is right in 99%+ of cases. Formal methods require a lot of extra effort for the less than 1% of exotic use cases!

**7\. Hillel recommends most engineers adopt property-based testing, and stop there.** Property-based tests mean defining properties which the test then throws thousands of inputs at, in order to stress test a system. Hillel is convinced that formal methods are a niche tool for most engineers, whereas property-based testing is the most practical approach for building robust software with this lightweight formal method.

**8\. AI won’t make formal verification mainstream, but will increase its use.** As Hillel says, “AI bringing formal verification up from maybe 0.1% to 0.3% across the industry would still be huge!” He also finds that people who succeed at using AI to generate formal specs are often formal verification experts.

**9\. Hillel worries about the time-of-check vs time-of-use bug.** It makes Hillel want to pull his hair out when he sees a time gap between the time of checking something (e.g., whether a bank account contains sufficient funds for withdrawals), and the action itself (e.g., withdrawing money). This category of bug is hard to defend against and can cause annoying issues in real-world systems.

**10\. Hillel worries less about job losses from AI and more about software becoming an “ordinary” job.** Revisiting his [2025 predictions of the impact of AI](https://www.linkedin.com/posts/hillel-wayne_the-following-can-all-be-true-1-vibe-coders-activity-7341040574319206401-F0C6/) on the tech industry, one of Hillel’s concerns is that software engineering in the future will be lower-paid and lower-prestige than today. At present, the range of software careers available is pretty magical, especially compared to “traditional” engineering roles. But will this last?

**11\. One of Hillel’s coolest projects: verifying train transponders.** Beyond databases and distributed systems, he has also formally verified device firmware. One cool project was working on the electric beacons between rail tracks that pass traffic information to the control system. He found a really odd bug in one transponder system, and fixing it made the real-world system more reliable and safe.

**12\. One thing that software engineering could take from “traditional” engineering: books on “the fundamentals” which every professional in the field should know.** One of Hillel’s favorite books is [The First Snap-Fit Handbook](https://www.amazon.com/First-Snap-Fit-Handbook-Creating-Attachments/dp/1569902798) , a nearly 500-page tome on those little clips that hold battery covers in place. He observes that while most industries have copious documentation for the most mundane topics, within software engineering there’s not even a book on how to version an API! We could learn from other fields about the value of documenting our own craft.

**13: The “materials” in software engineering are freakishly consistent.** All other engineering professions have to worry about the consistency of their materials; for example, electrical engineers work with resistors that offer resistance within 20% of 100 ohms across a thousand units, and only when operated within a given temperature range. In contrast, a program runs identically on any given computer in software engineering. Hillel argues that the variability we deal with in software, like versions, APIs, bugs with integrations, etc, are largely battles of our own making.

• [How to debug large, distributed systems: Antithesis](https://newsletter.pragmaticengineer.com/p/antithesis)

• [How AWS S3 is built](https://newsletter.pragmaticengineer.com/p/how-aws-s3-is-built)

• [Paying down tech debt](https://newsletter.pragmaticengineer.com/p/paying-down-tech-debt)

• [How Big Tech does quality assurance (QA)](https://newsletter.pragmaticengineer.com/p/how-big-tech-does-qa)

• [Bug management that works](https://newsletter.pragmaticengineer.com/p/bug-management-that-works-part-1)

• [Resiliency in distributed systems](https://newsletter.pragmaticengineer.com/p/resiliency-in-distributed-systems)

[00:00](https://www.youtube.com/watch?v=KSkcgIYQy0U) Intro

[04:32](https://www.youtube.com/watch?v=KSkcgIYQy0U&t=272s) The Crossover Project

[11:37](https://www.youtube.com/watch?v=KSkcgIYQy0U&t=697s) What software engineering does better

[15:30](https://www.youtube.com/watch?v=KSkcgIYQy0U&t=930s) What traditional engineering does better

[18:17](https://www.youtube.com/watch?v=KSkcgIYQy0U&t=1097s) Formal methods

[29:32](https://www.youtube.com/watch?v=KSkcgIYQy0U&t=1772s) TLA+: what it is and demo

[36:58](https://www.youtube.com/watch?v=KSkcgIYQy0U&t=2218s) TLA+ at Amazon

[38:10](https://www.youtube.com/watch?v=KSkcgIYQy0U&t=2290s) Ways distributed systems break

[41:03](https://www.youtube.com/watch?v=KSkcgIYQy0U&t=2463s) Formal methods and systems thinking

[46:20](https://www.youtube.com/watch?v=KSkcgIYQy0U&t=2780s) The value of learning math

[50:23](https://www.youtube.com/watch?v=KSkcgIYQy0U&t=3023s) What TLA+ is good for and isn’t

[52:50](https://www.youtube.com/watch?v=KSkcgIYQy0U&t=3170s) Alloy: a declarative language for software modeling

[58:53](https://www.youtube.com/watch?v=KSkcgIYQy0U&t=3533s) Other formal methods tools

[1:01:24](https://www.youtube.com/watch?v=KSkcgIYQy0U&t=3684s) Property-based testing

[1:05:31](https://www.youtube.com/watch?v=KSkcgIYQy0U&t=3931s) AI and the need for formal verification

[1:12:29](https://www.youtube.com/watch?v=KSkcgIYQy0U&t=4349s) Logic for programmers

[1:14:35](https://www.youtube.com/watch?v=KSkcgIYQy0U&t=4475s) Hillel’s 2025 prediction on AI’s impact

[1:21:30](https://www.youtube.com/watch?v=KSkcgIYQy0U&t=4890s) Book recommendation

**Where to find Hillel Wayne:**

• LinkedIn: [linkedin.com/in/hillel-wayne](http://linkedin.com/in/hillel-wayne)

• Newsletter: [https://buttondown.com/hillelwayne](https://buttondown.com/hillelwayne)

• Website: [https://www.hillelwayne.com](https://www.hillelwayne.com)

**Mentions during the episode:**

• The Crossover Project: [https://www.hillelwayne.com/tags/crossover-project](https://www.hillelwayne.com/tags/crossover-project)

• Blog Series: Real Software Engineering: [https://vanderburg.org/blog/series/real-software-engineering](https://vanderburg.org/blog/series/real-software-engineering/)

• Software Art Thou: Glenn Vanderburg — Real Software Engineering:

• New Austrian tunneling method: [https://en.wikipedia.org/wiki/New\_Austrian\_tunneling\_method](https://en.wikipedia.org/wiki/New_Austrian_tunneling_method)

• *The Design of Everyday Things*: [https://www.amazon.com/dp/0465050654](https://www.amazon.com/dp/0465050654)

• *The First Snap-Fit Handbook: Creating Attachments for Plastics Parts*: [https://www.amazon.com/dp/1569902798](https://www.amazon.com/dp/1569902798)

• NuSMV: [https://nusmv.fbk.eu/](https://nusmv.fbk.eu/)

• TLA+: [https://github.com/tlaplus](https://github.com/tlaplus)

• Use of Formal Methods at Amazon Web Services: [https://lamport.azurewebsites.net/tla/formal-methods-amazon.pdf](https://lamport.azurewebsites.net/tla/formal-methods-amazon.pdf)

• Common Sense Computing: From the Society of Mind to Digital Intuition and beyond: [https://link.springer.com/chapter/10.1007/978-3-642-04391-8\_33](https://link.springer.com/chapter/10.1007/978-3-642-04391-8_33)

• Alloy: [https://alloytools.org](https://alloytools.org)

• Time-of-check to time-of-use: [https://en.wikipedia.org/wiki/Time-of-check\_to\_time-of-use](https://en.wikipedia.org/wiki/Time-of-check_to_time-of-use)

• P: Formal Modeling and Analysis of Distributed Systems: [https://github.com/p-org/P](https://github.com/p-org/P)

• Quint: [https://quint.sh](https://quint.sh)

• PRISM: [https://www.prismmodelchecker.org](https://www.prismmodelchecker.org)

• NuSMV: a new symbolic model checker: [https://nusmv.fbk.eu](https://nusmv.fbk.eu)

• I formally modeled Dreidel for no good reason: [https://buttondown.com/hillelwayne/archive/i-formally-modeled-dreidel-for-no-good-reason](https://buttondown.com/hillelwayne/archive/i-formally-modeled-dreidel-for-no-good-reason)

• Formally modeling dreidel, the sequel: [https://buttondown.com/hillelwayne/archive/formally-modeling-dreidel-the-sequel](https://buttondown.com/hillelwayne/archive/formally-modeling-dreidel-the-sequel)

• Event-B: [https://eventb-soton.github.io/en-us](https://eventb-soton.github.io/en-us)

• MCRL2: [https://mcrl2.org/web/index.html](https://mcrl2.org/web/index.html)

• KeYmaera X: [https://keymaerax.org](https://keymaerax.org)

• Dafny: [https://dafny.org](https://dafny.org)

• JML: [https://www.openjml.org](https://www.openjml.org)

• Frama-C: [https://frama-c.com](https://frama-c.com)

• Ada SPARK: [https://www.adacore.com/languages/spark](https://www.adacore.com/languages/spark)

• The Coming AI Revolution in Distributed Systems: [https://zfhuang99.github.io/github%20copilot/formal%20verification/tla+/2025/05/24/ai-revolution-in-distributed-systems.html](https://zfhuang99.github.io/github%20copilot/formal%20verification/tla+/2025/05/24/ai-revolution-in-distributed-systems.html)

• CRAQ.tla: TLA+ specification of CRAQ (lamport-agent): [https://github.com/zfhuang99/lamport-agent/blob/main/spec/CRAQ/CRAQ.tla](https://github.com/zfhuang99/lamport-agent/blob/main/spec/CRAQ/CRAQ.tla)

• My EuroSys 2026 paper is obsolete: [https://claudiacauli.com/2026/03/08/my-eurosys-2026-paper-is-obsolete](https://claudiacauli.com/2026/03/08/my-eurosys-2026-paper-is-obsolete)

• Situated Software — Clay Shirky (2004): [https://gwern.net/doc/technology/2004-03-30-shirky-situatedsoftware.html](https://gwern.net/doc/technology/2004-03-30-shirky-situatedsoftware.html)

• Lamport Agent - AI-assisted Formal Specification: [https://zfhuang99.github.io/github%20copilot/formal%20verification/tla+/2025/11/14/lamport-agent.html](https://zfhuang99.github.io/github%20copilot/formal%20verification/tla+/2025/11/14/lamport-agent.html)

• LLMs are bad at vibing specifications: [https://buttondown.com/hillelwayne/archive/llms-are-bad-at-vibing-specifications](https://buttondown.com/hillelwayne/archive/llms-are-bad-at-vibing-specifications/)

• *Logic for Programmers*: [https://logicforprogrammers.com](https://logicforprogrammers.com)

• Engineering a Safer World: Systems Thinking Applied to Safety: [https://www.amazon.com/dp/0262533693](https://www.amazon.com/dp/0262533693)

• The following can all be true: [https://www.linkedin.com/posts/hillel-wayne\_the-following-can-all-be-true-1-vibe-coders-share-7341040573711073281-3V8C](https://www.linkedin.com/posts/hillel-wayne_the-following-can-all-be-true-1-vibe-coders-share-7341040573711073281-3V8C)

• The third golden age of software engineering – thanks to AI, with Grady Booch: [https://newsletter.pragmaticengineer.com/p/the-third-golden-age-of-software](https://newsletter.pragmaticengineer.com/p/the-third-golden-age-of-software)

• *Data and Reality: A Timeless Perspective on Perceiving and Managing Information in Our Imprecise World*: [https://www.amazon.com/dp/1935504215](https://www.amazon.com/dp/1935504215)

• Debugging: The 9 Indispensable Rules for Finding Even the Most Elusive Software and Hardware Problems: [https://www.amazon.com/Debugging-Indispensable-Software-Hardware-Problems/dp/0814474578](https://www.amazon.com/Debugging-Indispensable-Software-Hardware-Problems/dp/0814474578)

—

Production and marketing by [Pen Name](https://penname.co/) .
