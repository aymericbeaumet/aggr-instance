---
title: From Chrome DevTools to AI Engineering, with Addy Osmani
link: https://newsletter.pragmaticengineer.com/p/from-chrome-devtools-to-ai-engineering
source: newsletter-pragmaticengineer-com
published: 2026-08-19T16:53:57Z
updated: 2026-08-19T16:53:57Z
first_seen: 2026-09-19T21:30:23.395188495Z
authors:
- Gergely Orosz
summary: Addy Osmani shares lessons from 14 years at Google and how AI agents are reshaping software engineering, developer workflows, and the skills engineers need to succeed.
content: extracted
html: 2026-08-19-from-chrome-devtools-to-ai-engineering-with-addy-osmani.html
preview:
  file: 2026-08-19-from-chrome-devtools-to-ai-engineering-with-addy-osmani.preview-35793053ecd8.webp
  width: 256
  height: 128
  color: '#c7c2c1'
images:
- source: https://substackcdn.com/image/fetch/$s_!oP9R!,w_1200,h_600,c_fill,f_jpg,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb3599397-af98-4169-beb1-8342b84e65db_1280x720.png
  original:
    file: 2026-08-19-from-chrome-devtools-to-ai-engineering-with-addy-osmani.image-116da5831e80.jpg
    width: 1200
    height: 600
  color: '#f0f0f0'
- source: https://substackcdn.com/image/fetch/$s_!-jSJ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F16f7884a-f725-4cff-84be-46c6e10c1df1_1600x140.png
  original:
    file: 2026-08-19-from-chrome-devtools-to-ai-engineering-with-addy-osmani.image-d7cf6f3baebf.jpg
    width: 1456
    height: 127
  color: '#fdfdfd'
- source: https://substackcdn.com/image/fetch/$s_!CVGe!,w_120,h_120,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fpbs.substack.com%2Fprofile_images%2F1772426579156643840%2FRF8iP6Zn.jpg
  original:
    file: 2026-08-19-from-chrome-devtools-to-ai-engineering-with-addy-osmani.image-98a3c8396779.jpg
    width: 120
    height: 120
  color: '#221926'
extra:
  audio_url: https://api.substack.com/feed/podcast/211500766/5765020bc1d2746638954634cc052c95.mp3
---

**Listen and watch now on [YouTube](https://youtu.be/2fyPnxKu8ZM) , [Apple](https://podcasts.apple.com/us/podcast/the-pragmatic-engineer/id1769051199) , and [Spotify](https://open.spotify.com/show/2Bho9xCbOQMWMJ7UKmqCzD) .** See the episode transcript at the top of this page, and timestamps for the episode at the bottom.

[![](https://substackcdn.com/image/fetch/$s_!-jSJ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F16f7884a-f725-4cff-84be-46c6e10c1df1_1600x140.png)](https://substackcdn.com/image/fetch/$s_!-jSJ!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F16f7884a-f725-4cff-84be-46c6e10c1df1_1600x140.png)

• **[Antithesis](https://antithesis.com/pragmatic)** – verify your system’s correctness without human review or traditional integration tests – and avoid bugs or outages. Teams like Jane Street, Fly.io, and the etcd community use Antithesis to ship better code, faster. [Learn more.](https://antithesis.com/pragmatic)

• **[Sentry](https://sentry.io/pragmatic)** – application monitoring software built by developers, for developers. [Sentry’s Seer AI agent](https://sentry.io/product/seer/?utm_source=pragmatic&utm_medium=paid-community&utm_campaign=ai-fy27q1-seerlaunch&utm_content=recording-coreread-freetrial&code=pragmatic) is one of their new, neat tools, which I’ve used as a way to quickly fix errors on my backend. [Check out Sentry.](https://sentry.io/pragmatic)

• **[Google Cloud Run](https://cloud.google.com/run?e=48754805&utm_source=&utm_medium=&utm_campaign=the_pragmatic_engineer)** – run untrusted agent code without the security anxiety. [Cloud Run sandboxes](https://docs.cloud.google.com/run/docs/code-execution?e=48754805&utm_campaign=the_pragmatic_engineer&utm_source=&utm_medium=&utm_campaign=the_pragmatic_engineer_2) deliver hyper-isolated, ephemeral execution environments that spin up in milliseconds. Check out [Cloud Run sandboxes.](https://docs.cloud.google.com/run/docs/code-execution?e=48754805&utm_campaign=the_pragmatic_engineer&utm_source=&utm_medium=&utm_campaign=the_pragmatic_engineer_2)

[Addy Osmani](https://addyosmani.com) spent more than 14 years at Google, working on Chrome, DevTools, Core Web Vitals, and most recently, AI developer experience.

If you’ve ever opened Chrome DevTools, or optimized a page for Core Web Vitals, you’ve used software built by Addy Osmani. In this episode, I sit down with Addy and we talk about his path from building a web browser aged just 16 to becoming a director at Google. We discuss what he learned from building tools for millions of developers, Google’s engineering culture, and why he continued doing hands-on coding work as a manager. We also get into how he works with AI agents today, the risks of ‘cognitive surrender,’ his approach to ‘loop engineering,’ and why it’s good to develop skills in product management, go-to-market, and other areas.

Here are eleven interesting points from the chat with Addy:

**1\. Addy built a web browser from scratch, aged just 16.** Back then, a pain point was that Addy had to carry floppy disks to his local library to download data. To speed up browsing, he built a browser that opened multiple connections when fetching webpages.

**2\. Publishing free educational materials helped Addy land a job at Google.** A documentary about Google which he watched as a youngster made Addy want to work somewhere like it. Later, Google noticed his work in publishing educational resources about frontend and JavaScript development. The company reached out about a DevRel-and-builder role, and Addy was hired to join the Chrome team.

**3\. Chrome DevTools was an effort by Google to meet web developers in the browser**. Today, DevTools is one of the closest things Google has to an IDE (not counting Antigravity, that is), but the project started as a way to add tools to the browser to help debug web applications. As web engineers started to use more complex frameworks and build chains, DevTools added capabilities like source-map-aware debugging, hiding library code, mobile device emulation, tooling for service workers, and more.

**4\. Most developers don’t understand memory management.** Addy says this is because memory debugging tooling has not advanced in a decade, and remains a hard problem to solve. This is despite making improvements in runtime performance debugging in Chrome DevTools ( [flame graphs](https://medium.com/slalom-blog/flame-graphs-in-chrome-devtools-a-guide-for-front-end-developers-b9503ff4a4d) and [deep tracing](https://www.dtoolkits.com/perf-trace) ).

**5\. Becoming accountable on a weekly basis for a top company goal is the biggest difference in a director of engineering at a major tech company.** Addy worked his way up from engineer to Director of Engineering at Google, and I asked what the biggest change was when he made it to that level. Being on the hook and reporting regularly on a top company goal was something he found entirely new, Addy said.

**6\. A big culture shift at Google in the last two years has been VPs and SVPs coding on weekends.** Naturally, this is because AI tools make coding much easier. During his last two years at Google, it was common for these folks to talk about their weekend side projects and tools they used to build them.

**7\. A big risk of AI-assisted development is cognitive surrender.** Addy defines cognitive surrender as the erosion of your comprehension of the problems being worked on, and of your own memory of what’s going on. He recommends pushing back against this by understanding every major decision an LLM makes. Unfortunately, his former method of reading the AI’s *entire* reasoning process is no longer practical given how much output agents can generate, but you’ll still want to understand the most important decisions.

**8\. Aim for mutual amplification when using AI tools.** The aim is to do two things simultaneously:

- Help the agent improve throughout the task by having it log its decisions and key learnings

- You also improve by reviewing, understanding, and internalizing what the agent does and how you can learn from it

**9\. Addy believes software engineers will always be important because an AI model cannot be accountable.** Accountability for code and software is possible even if the accountable party didn’t write the code, as is the case in projects like Chromium, where designated engineers own parts of the codebase. They’re responsible for approving and rejecting contributions, and for shaping that part of the codebase. Addy reckons that a “what am I accountable for?” mindset will be adopted by many software engineers.

**10\. Addy is bullish about software engineering’s outlook.** Every time the profession has made it easier to create software, we’ve created exponentially more software. Addy predicts the same will happen with AI, and that the total addressable market of people building software will get much bigger.

**11\. Advice on where to invest efforts as engineers in the coming years.** In his words:

> “What we are very likely to see happen next with engineering careers (as well as product and other roles) is the unbundling of them, so that an engineer also has product sense, while a product person also has engineering sense, or UX sense.
>
> \[You should\] think about the non-engineering things if you don’t \[usually\] have the time to think about product or technical evangelism, or go-to-market approaches, or any other parts of how businesses are successful.
>
> If you can show employers that you are not just a builder, but someone that can help them as roles start to become a little bit fuzzier, then I think that you can be successful in these times. **Don’t be** ***just*** **an engineer.”**

• [What is loop engineering?](https://newsletter.pragmaticengineer.com/p/what-is-loop-engineering)

• [Inside Google’s engineering culture](https://newsletter.pragmaticengineer.com/p/google)

• [How AI-assisted coding will change software engineering: hard truths](https://newsletter.pragmaticengineer.com/p/how-ai-will-change-software-engineering)

• [Are AI agents actually slowing us down?](https://newsletter.pragmaticengineer.com/p/are-ai-agents-actually-slowing-us)

• [How Claude Code is built](https://newsletter.pragmaticengineer.com/p/how-claude-code-is-built)

• [How Codex is built](https://newsletter.pragmaticengineer.com/p/how-codex-is-built)

• [From IDEs to AI Agents](https://newsletter.pragmaticengineer.com/p/from-ides-to-ai-agents-with-steve?) with Steve Yegge

• [Google’s engineering culture](https://newsletter.pragmaticengineer.com/p/googles-engineering-culture?utm_source=chatgpt.com) : the podcast

[00:00](https://www.youtube.com/watch?v=2fyPnxKu8ZM) Intro

[02:50](https://www.youtube.com/watch?v=2fyPnxKu8ZM&t=170s) Addy’s current workflow

[05:11](https://www.youtube.com/watch?v=2fyPnxKu8ZM&t=311s) Addy’s path into tech

[15:04](https://www.youtube.com/watch?v=2fyPnxKu8ZM&t=904s) Addy’s work on jQuery

[16:44](https://www.youtube.com/watch?v=2fyPnxKu8ZM&t=1004s) TodoMVC

[21:44](https://www.youtube.com/watch?v=2fyPnxKu8ZM&t=1304s) Getting hired at Google and working on Chrome

[27:17](https://www.youtube.com/watch?v=2fyPnxKu8ZM&t=1637s) Building dev tools

[40:15](https://www.youtube.com/watch?v=2fyPnxKu8ZM&t=2415s) Core Web Vitals

[45:42](https://www.youtube.com/watch?v=2fyPnxKu8ZM&t=2742s) Google’s engineering culture

[51:03](https://www.youtube.com/watch?v=2fyPnxKu8ZM&t=3063s) Addy’s career trajectory at Google

[57:55](https://www.youtube.com/watch?v=2fyPnxKu8ZM&t=3475s) The director role at Google

[1:01:40](https://www.youtube.com/watch?v=2fyPnxKu8ZM&t=3700s) Cognitive debt and cognitive surrender

[1:03:03](https://www.youtube.com/watch?v=2fyPnxKu8ZM&t=3783s) Working with agents

[1:05:52](https://www.youtube.com/watch?v=2fyPnxKu8ZM&t=3952s) Loop engineering

[1:12:55](https://www.youtube.com/watch?v=2fyPnxKu8ZM&t=4375s) The changing role of the software engineer

[1:18:15](https://www.youtube.com/watch?v=2fyPnxKu8ZM&t=4695s) How Addy uses AI in writing

[1:27:40](https://www.youtube.com/watch?v=2fyPnxKu8ZM&t=5260s) What’s next for Addy

[1:28:47](https://www.youtube.com/watch?v=2fyPnxKu8ZM&t=5327s) Career advice

**Where to find Addy Osmani:**

• X: [https://x.com/addyosmani](https://x.com/addyosmani)

• LinkedIn: [https://www.linkedin.com/in/addyosmani](https://www.linkedin.com/in/addyosmani)

• Website: [https://addyosmani.com](https://addyosmani.com)

**Mentions during the episode:**

• Beyond Vibe Coding with Addy Osmani: [https://newsletter.pragmaticengineer.com/p/beyond-vibe-coding-with-addy-osmani](https://newsletter.pragmaticengineer.com/p/beyond-vibe-coding-with-addy-osmani)

• Borland: [https://en.wikipedia.org/wiki/Borland](https://en.wikipedia.org/wiki/Borland)

• jQuery: [https://jquery.com](https://jquery.com)

• John Resig on X: [https://x.com/jeresig](https://x.com/jeresig)

• AngularJS: [https://angularjs.org](https://angularjs.org)

• Backbone.js: [https://backbonejs.org](https://backbonejs.org)

• YUI: [https://github.com/yui/yui3](https://github.com/yui/yui3)

• Ext JS: [https://en.wikipedia.org/wiki/Ext\_JS](https://en.wikipedia.org/wiki/Ext_JS)

• Sindre Sorhus’s website: [https://sindresorhus.com](https://sindresorhus.com)

• Speedometer: [https://browserbench.org/Speedometer3.0](https://browserbench.org/Speedometer3.0)

• Next.js: [https://nextjs.org](https://nextjs.org)

• Grunt: [https://en.wikipedia.org/wiki/Grunt\_(software)](https://en.wikipedia.org/wiki/Grunt_\(software\))

• Firebug: [https://en.wikipedia.org/wiki/Firebug\_(software)](https://en.wikipedia.org/wiki/Firebug_\(software\))

• Pavel Feldman on LinkedIn: [https://www.linkedin.com/in/pavel-feldman-24b0041](https://www.linkedin.com/in/pavel-feldman-24b0041)

• Paul Irish on LinkedIn: [https://www.linkedin.com/in/paulirish](https://www.linkedin.com/in/paulirish)

• Paul Bakaus on LinkedIn: [https://www.linkedin.com/in/paulbakaus](https://www.linkedin.com/in/paulbakaus)

• Impeccable: [https://impeccable.style](https://impeccable.style)

• Visual Studio: [https://visualstudio.microsoft.com](https://visualstudio.microsoft.com)

• Yang Gao on LinkedIn: [https://www.linkedin.com/in/yang-gao-08567b51](https://www.linkedin.com/in/yang-gao-08567b51)

• Understanding Core Web Vitals and Google search results: [https://developers.google.com/search/docs/appearance/core-web-vitals](https://developers.google.com/search/docs/appearance/core-web-vitals)

• Google’s engineering culture: [https://newsletter.pragmaticengineer.com/p/googles-engineering-culture](https://newsletter.pragmaticengineer.com/p/googles-engineering-culture)

• Inside Google’s Engineering Culture: Part 1: [https://newsletter.pragmaticengineer.com/p/google](https://newsletter.pragmaticengineer.com/p/google)

• Inside Google’s Engineering Culture: the Tech Stack (Part 2): [https://newsletter.pragmaticengineer.com/p/google-part-2](https://newsletter.pragmaticengineer.com/p/google-part-2)

• Simon Hørup Eskildsen’s website: [https://sirupsen.com](https://sirupsen.com)

• Pushing software engineering limits with “napkin math”: [https://newsletter.pragmaticengineer.com/p/pushing-software-engineering-limits](https://newsletter.pragmaticengineer.com/p/pushing-software-engineering-limits)

• Loop engineering: [https://addyosmani.com/blog/loop-engineering](https://addyosmani.com/blog/loop-engineering)

• What is “loop engineering?”: [https://newsletter.pragmaticengineer.com/p/what-is-loop-engineering](https://newsletter.pragmaticengineer.com/p/what-is-loop-engineering)

• Peter Steinberger on X: [https://x.com/steipete](https://x.com/steipete)

• Boris Cherny on X: [https://x.com/bcherny](https://x.com/bcherny)

• Ryan Dahl’s post on X:

[![X avatar for @rough__sea](https://substackcdn.com/image/fetch/$s_!CVGe!,w_120,h_120,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fpbs.substack.com%2Fprofile_images%2F1772426579156643840%2FRF8iP6Zn.jpg)

Ryan Dahl @rough\_\_sea

This has been said a thousand times before, but allow me to add my own voice: the era of humans writing code is over. Disturbing for those of us who identify as SWEs, but no less true. That's not to say SWEs don't have work to do, but writing syntax directly is not it.

4:02 PM · Jan 19, 2026 · 7.33M Views

943 Replies · 2.67K Reposts · 19.8K Likes](https://x.com/rough__sea/status/2013280952370573666)

• *The Effective Software Engineer: How ICs at Every Level Can Leverage AI, Prioritize High-Value Work, and Lead Beyond Their Role*: [https://www.amazon.com/Effective-Software-Engineer-Prioritize-High-Value/dp/B0FMJ5XVSD](https://www.amazon.com/Effective-Software-Engineer-Prioritize-High-Value/dp/B0FMJ5XVSD)

• *Leading Effective Engineering Teams: Lessons for Individual Contributors and Managers from 10 Years at Google*: [https://www.amazon.com/Leading-Effective-Engineering-Teams-Contributors/dp/109814824X](https://www.amazon.com/Leading-Effective-Engineering-Teams-Contributors/dp/109814824X)

• *Beyond Vibe Coding: From Coder to AI-Era Developer*: [https://www.amazon.com/Beyond-Vibe-Coding-AI-Era-Developer/dp/B0F6S5425Y](https://www.amazon.com/Beyond-Vibe-Coding-AI-Era-Developer/dp/B0F6S5425Y)

• Michael Novati on LinkedIn: [linkedin.com/in/michaelnovati](http://linkedin.com/in/michaelnovati)

• “The Coding Machine” at Meta with Michael Novati: [https://newsletter.pragmaticengineer.com/p/the-coding-machine-at-meta](https://newsletter.pragmaticengineer.com/p/the-coding-machine-at-meta)

—

Production and marketing by [Pen Name](https://penname.co/) .
