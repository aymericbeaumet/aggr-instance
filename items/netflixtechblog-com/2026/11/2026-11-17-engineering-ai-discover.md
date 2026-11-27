---
title: Engineering AI Discover
link: https://netflixtechblog.com/engineering-ai-discover-e244944cdc38?source=rss----2615bd06b42e---4
source: netflixtechblog-com
published: 2026-11-17T17:01:02Z
updated: 2026-11-17T17:01:02.131Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Netflix Technology Blog
labels:
- ai-agent
- artificial-intelligence
- harness-engineering
- llm
- recommendation-system
content: feed
html: 2026-11-17-engineering-ai-discover.html
remote_preview:
  url: https://cdn-images-1.medium.com/max/1000/1*MSAqZ2TgZxKfGifhug2wuA.gif
  alt: A visual walkthrough of the AI Discover experience launched on Netflix TV devices.
---

*The challenges and decisions behind AI Discover’s intelligence layer*

By [Varun Khaitan](https://www.linkedin.com/in/varun-khaitan/), [Peng Ren](https://www.linkedin.com/in/peng-ren-49b88369/), [Gary Tang](https://www.linkedin.com/in/thegarytang/), [Itay Teller](https://www.linkedin.com/in/itay-t-31150b192/), [Atif Faheem](https://www.linkedin.com/in/faheematif/), [Nipun Kumar](https://www.linkedin.com/in/nipunk/), [Donnie DeBoer](https://www.linkedin.com/in/donniedeboer/), [Eduardo Lundgren](https://www.linkedin.com/in/eduardolundgren/), [Matteo Rinaldi](https://www.linkedin.com/in/matteo-rinaldi/), [Raghav Bakshi](https://www.linkedin.com/in/b0kshi/)

We recently introduced [AI Discover](https://about.netflix.com/en/news/introducing-ai-discover), a new tab in our TV app where a small group of members can try new ways to find something to watch. For years, our recommendations have worked in the background, inferring what a member might enjoy from their viewing history. AI Discover moves from that silent ranking toward steerable discovery. A member picks a theme, say “Quick Watches,” and gets a row of matching titles, a short note on why they fit and a few narrower themes to try next. From there, they can keep narrowing, head somewhere new or tune what they see.

Building AI Discover raised a question many teams will recognize: how do you deliver a better member experience with generative AI on TV without sacrificing speed, reliability or safety? Our answer is a new intelligence layer, the agent harness behind AI Discover. For each request, it decides how to combine LLMs with our recommendation systems. It builds on years of work understanding what members enjoy, and on our recent work applying LLMs to recommendations and generative experiences. The layer itself plans each request, runs it across those systems, serves the result and evaluates it offline.

This post covers the challenges we designed for and the key decisions we made along the way.

![A visual walkthrough of the AI Discover experience launched on Netflix TV devices.](https://cdn-images-1.medium.com/max/1000/1*MSAqZ2TgZxKfGifhug2wuA.gif)

### Why an intelligence layer?

Every Netflix page combines recommendations with rules. We pick titles a member may enjoy, and eligibility, maturity and safety rules decide what can appear. An experience built on LLMs needs the same rules, plus code that decides when to call an LLM and how to handle its output. That code is called an agent harness, and it’s the core of our intelligence layer.

The simplest implementation hands the entire request to an LLM and lets it pick the titles, write the labels and decide what happens next. However, that leaves us relying on probabilistic output where we need governed results, because every turn in AI Discover ends on a screen in someone’s living room. At our scale, even rare failures happen many times a day. A member sees every slow call, unavailable title or off-brand line. Seven challenges shaped our design:

1. **Fitting LLMs into a product and architecture designed without them.** Traditional recommenders rank titles based on past behavior. LLMs can do more: understand what a member is asking for and plan how to respond. Our existing experience would leave most of that unused.
2. **Keeping recommendations grounded.** Every pick must be a real, available title from our changing catalog. Retrieval and rankers work in tandem with LLMs to choose titles that fit the member’s taste and intent.
3. **Working with slow, unpredictable LLMs.** LLM calls are the slowest and most expensive part of the system. Tail latencies can spike, and the output may not be what you asked for. So we use LLMs selectively, treat what they return as untrusted input and test performance and quality before anything ships.
4. **Adapting to fast-changing models, tools and prompts.** Modeling and engineering solutions keep evolving, so the product can’t depend on any single model, tool or prompt.
5. **Building for TV.** Typing on a TV is hard, so members rely on the remote, and the app should respond to every press right away on widely varying hardware. TV apps also update slowly, so changes have to ship without an app release. A typical TV request returns in moments, while multi-step generation can take several seconds.
6. **Holding a safety and reliability bar at scale.** Members always get a page, even when an LLM call fails or yields poor results. Their choices and our content standards hold on every request, and when something breaks, structured records show why.
7. **Learning from what members say alongside what we already know.** Recommendations have always learned from viewing behavior. AI Discover also lets members tell us what they want, and that has to shape what they see next.

### System Architecture

When a member picks a theme, the TV app sends the request to the *Page Server*, which builds pages for all Netflix apps and hands the request asynchronously to the intelligence layer. At a high level, this new layer has two components: a controller and an executor.

The **controller** decides what to run. It gathers the context needed for each request, including real-time signals such as the time of day. It then settles on a plan, either an established one or a new one drafted by its planner, validates it and retrieves only the member details that this plan requires from *Memory Hub*, our per-member memory store. These details may include member taste summaries generated by MemberLLM, our in-house language model for personalized recommendations, or feedback shared by the member. We talk more about Memory Hub and member taste summaries later.

A plan is a graph of approved steps, each running a capability. A capability is a named unit of work that can have multiple versions and belongs to a family such as retrieval, ranking, generation or packaging. It declares its inputs and outputs, the context it may see and what happens if it fails.

Capability declarations make a plan verifiable. Before execution, the controller validates the graph: every step is approved, every input has a producer, nothing loops and the plan ends in exactly one page. Afterward, the controller checks the result before it’s served.

The **executor** runs the plan. Steps pass typed outputs to each other through the declared contracts, so each receives exactly what it asked for. Independent steps run in parallel, each with its own deadline and appropriate fallback behaviors. Some steps are optimized LLM calls with one task each, such as writing row titles or suggesting next themes, and they act only within the validated plan. Checks before the run and contracts during it give us governed execution, where every action stays within limits set in code.

Both parts write to the request records, the traces we use offline to evaluate and improve the system.

![](https://cdn-images-1.medium.com/max/1024/1*A1hYFSHsZ3lenwBF0nBELg.jpeg)\
System Architecture Diagram

### Key Decisions

Each challenge led to one decision. Here’s what we chose and what it bought us.

#### 1\. Design the product and architecture around LLMs

We reimagined the experience around LLMs. The easier path would have been to drop them in as rankers, keep today’s latency budgets and precompute where necessary. AI Discover is built on a back-and-forth around themes a member can steer, which gives LLMs work only they can do: generating themes, explaining rows, summarizing taste and helping choose the titles that fit the member and the moment. Members pick themes with the remote, so they never have to type. On the TV, those themes and explanations get their own native UI elements. Behind the screen, LLMs can draft plans, and asynchronous serving gives multi-step agentic orchestration the time it needs.

This opens up experiences our traditional stack couldn’t support on its own.

#### 2\. Make existing systems composable

Keeping recommendations grounded meant building on the systems that already know our catalog and members. Most capabilities wrap complex, proven Netflix systems such as search, large-scale ranking and catalog metadata access. Some of them may run their own agents in the future.

Reshaping them took careful work, because agentic orchestration challenges many long-standing assumptions of distributed systems. A capability now has to work in combinations no one designed, for a caller that chooses to call it at runtime. Each one needs a clear purpose and a semantic interface, and shaping it means deciding how much to combine into a single step and how much to leave to the planner. Too fine, and the planner has more ways to go wrong; too coarse, and it loses room to compose richer plans.

In return, the planner gets building blocks it can mix in new ways, and connecting services takes far less manual wiring. Because plans can only use these blocks, even one drafted on the fly stays grounded in our catalog and member taste.

#### 3\. Plan first, then execute

To contain LLM latency and errors, we plan every request before anything runs. The familiar way to let an agent use tools is a loop: reason, call a tool, read the result, reason again. It’s flexible, but you can’t tell up front how many steps it will take or whether it has gone down a wrong path. In agent terms, ours is a plan-and-execute agent: an LLM can propose the path, but code decides whether it runs.

Plans come from two places: established plans we’ve already tested, and plans an LLM drafts on the fly. A router we control decides which one is served. Either way, the plan passes the same checks before it runs. If the planner is slow or its plan fails a check, we fall back to a tested plan.

This allows for adaptive planning. The planner can find new ways to serve a request, and we can balance cost, latency and quality for each task, from a lean single-call plan to a richer multi-step one.

![](https://cdn-images-1.medium.com/max/1024/1*d9BOSs7HRW58yFg3ZH99Tg.jpeg)

#### 4\. Stay model and capability agnostic

The best model or capability for a task today may be second-best, or overkill, in a few months, so we built the system to switch between them. We assign each task to the model or capability version that fits it. Today that’s a per-task setting we can override live. Only some steps in a request use an LLM.

That only works if switching is easy. All the LLMs we use sit behind one common interface. Before we switch, each candidate is scored on the same tasks by automated checks and LLM judges, so every swap is a measured change.

Much of the system’s behavior lives in server-side prompts and configuration, so we can swap models, tools and capabilities without new code or a TV app release.

Upgrades get cheap and experiments get easy. Moving to a better model, ranker or retrieval system is a plan or setting change, not a migration, so each component can keep improving without a large investment.

#### 5\. Make page generation asynchronous

Inference keeps getting faster, but member expectations rise just as quickly: richer responses, more context and more of the page shaped by LLMs. The line between what feels instant and what’s worth waiting for will keep shifting, so we built the asynchronous path as a first-class way of serving.

Most of a TV app is synchronous: press a button, get a page. Generative work breaks that pattern. Holding one request open for many seconds across services would tie up too many threads. So requests that involve generation are asynchronous. The press returns right away with an acknowledgement, and the TV switches to a thinking state that explains what’s being prepared. Meanwhile, the executor runs the plan in the background. When the result is ready, the server notifies the TV, which then fetches it.

Slow generation no longer slows anything else. The serving path stays healthy however long a turn takes, and every press still gets immediate feedback on screen.

![](https://cdn-images-1.medium.com/max/1024/1*E0CdsG6_dmd8jQcAKOqdxA.jpeg)\
Async Sequence Diagram

#### 6\. Enforce safety in code

Some rules must hold no matter what an LLM writes: content and maturity standards, what each step can access, and what a member has asked us not to show them. Putting those rules in a prompt makes them probabilistic, so we enforce them in code. Generated text is cleaned up before it reaches the screen. The planner sees a stripped-down request, and member details are limited to what the validated plan needs.

For example, the planner decides which inputs each step needs, such as the profile’s maturity rating for retrieval. Instead of writing parameter values directly, it uses placeholders. After the plan passes validation, the controller fills them with values from the request, matched by declared type. This also protects against prompt injection.

Each request also leaves a structured record: the plan, each step and its outcome, and which rules applied. When a page misses, we can tell whether the plan, the data, a rule or the model was at fault. The same records feed our offline evaluation and replay.

Together, these rules and records let an experiment ship at the same safety and privacy bar as the rest of the product.

#### 7\. Remember what members tell us

A taste summary is a short profile of what a member enjoys, such as favorite genres, tone and recurring themes. MemberLLM generates it ahead of time from the member’s viewing history, which keeps that LLM work off the request path. It helps us further personalize their page, and steps that write themes and explanations receive it as context, so their wording reflects the member. We’ll cover MemberLLM in more detail in an upcoming post.

In AI Discover, members can also tune their recommendations and tell us directly what they want. Those choices apply right away and carry across visits, and they give us a new signal alongside what we already know about their taste. This is a first step toward remembering more about each member, from their preferences to the habits that show up over time and the corners of the catalog they like to explore. We keep it all in Memory Hub, our persistent, low-latency memory store, next to their taste summaries, and we can add new kinds of memory as we need them. We apply these memories to later requests where they’re relevant, together with real-time context.

### The member holds the remote

In this version, members interact by selecting themes and using tuning options such as “don’t recommend.” There’s no free-text input or open-ended chat. That keeps the LLM’s job narrow by design.

- **The usual protections still apply.** Profile settings and our safety and privacy policies hold here as everywhere else.
- **Generated text was tested before launch.** We ran safety evaluations on theme labels, row titles and explanations to make sure they meet our standards.

### What’s next

It would have been faster to let an LLM do everything and stop there. The intelligence layer gave us an AI-native product, grounded building blocks, adaptive planning, quicker experimentation, asynchronous generation, governed execution and agent memory for tunable personalization. While AI Discover is its first experience, we designed it with more in mind.

We’ve organized our efforts to answer two questions:

- **How much do members want to shape their recommendations?** Whether they’d rather steer or lean back, and how much, which kinds of themes help, and how we should blend what members tell us with our existing personalization systems.
- **What tradeoffs does this technology bring?** How cost and latency hold up in production as we ramp traffic, how long members will wait for generated results, and whether errors came from the model, the harness or our existing systems. Multi-turn sessions also need different evaluation and observability than single requests.

Longer term, our goal is a system that improves itself, starting from the record each request leaves behind. Today we replay past requests against a new prompt, model or rule to see what changes, and we judge each change with automated checks, LLM judges and human evaluations. Eventually, we want the system to propose improvements, test them safely and keep only what proves better, within our privacy and data-use policies. As LLMs improve, more of the work may move into them, and the intelligence layer lets us move that line deliberately.

This is part of a bigger shift. Recommendation systems have long learned quietly from what members watch and answered with ranked rows. LLMs let them listen and respond, and members may increasingly expect that. We’ll keep exploring what that means for discovery on Netflix.

### Acknowledgements

We would like to thank Adi Suresh, Akash Katipally, Allan Johns, Becks Wood, Bhargav Mangipudi, Carrie Santiago, Chris Martin, Claire Campbell, Corey Waldin, Dana Sittler, Daneo Zhang, David Leen, Devon Bryant, Dharamendra Kumar, Dharini Chandrasekaran, Ebony Turner, Federico Roa, Gianfranco Cuoco, Guru Tahasildar, Haley Clanton, Haresh Chudgar, Ivan Provalov, James Schultz, JingXu Yang, Josie Shaw, Kamelia Aryafar, Kate Cummings, Kathleen Garrity, Kelley Robinson, Kevin Du, Kevin O’Connor, Kyla Moore, Kyle Lind, Laura Matsuura, Leah Cusella, Lingyi Liu, Lingyong Wang, Mallory Walkup, Matthew Pereira, Michael Galassi, Minsu Jang, Moumita Bhattacharya, Paran Jain, Paul Lee, Qusai Al-Rabadi, Scarlet Chen, Sergi Perez, Sriram Rao, Stephen Shaw, Sura Elamurugu, Varad Pathak, Yesu Feng, Zainab Mahar Mir, and Zhongli Cheng for their contributions to this work.

![](https://medium.com/_/stat?event=post.clientViewed&referrerSource=full_rss&postId=e244944cdc38)

* * *

[Engineering AI Discover](https://netflixtechblog.com/engineering-ai-discover-e244944cdc38) was originally published in [Netflix TechBlog](https://netflixtechblog.com) on Medium, where people are continuing the conversation by highlighting and responding to this story.
