---
title: Building resilient systems with Sam Newman
link: https://newsletter.pragmaticengineer.com/p/building-resilient-systems-with-sam
source: newsletter-pragmaticengineer-com
published: 2026-10-07T17:19:58Z
updated: 2026-10-07T17:19:58Z
first_seen: 2026-10-07T19:30:21.779631669Z
authors:
- Gergely Orosz
summary: Sam Newman joins me to discuss when to use microservices, how to build resilient distributed systems, and how AI is changing software development.
content: extracted
html: 2026-10-07-building-resilient-systems-with-sam-newman.html
remote_preview:
  url: https://substackcdn.com/image/fetch/$s_!AlJu!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8e3d3c6f-6a37-411e-aa1a-dfcefc979af3_1600x140.png
extra:
  audio_type: audio/mpeg
  audio_url: https://api.substack.com/feed/podcast/218980052/4ba6e182abf3183ae7500e722b77f40d.mp3
  duration_seconds: 7103
---

**Listen and watch now on [YouTube](https://youtu.be/a0sZV0qIbI0), [Apple](https://podcasts.apple.com/us/podcast/the-pragmatic-engineer/id1769051199), and [Spotify](https://open.spotify.com/show/2Bho9xCbOQMWMJ7UKmqCzD).** See the episode transcript at the top of this page, and timestamps for the episode at the bottom.

![](https://substackcdn.com/image/fetch/$s_!AlJu!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8e3d3c6f-6a37-411e-aa1a-dfcefc979af3_1600x140.png)

• **[turbopuffer](http://turbopuffer.com/pragmatic)** – vector and full-text search built on object storage: fast, 10x cheaper, and extremely scalable. Under the hood, the team are rewriting the tpuf storage engine. Follow along until it hits production at [turbopuffer.com/v3](http://turbopuffer.com/v3)

• **[Get a free introduction to property-based testing](https://pages.antithesis.com/webinar-property-based-testing-pragmatic?utm_medium=Email&utm_campaign=pragmatic_2026&utm_source=pragmatic&utm_content=pbt_20261001)**: through a live, online webinar hosted by Hillel Wayne (a recent [podcast guest](https://newsletter.pragmaticengineer.com/p/formal-methods-with-hillel-wayne)!), author of Logic for Programmers and Developer Educator at Antithesis. What if you only needed a single test to harden your system? [RSVP here](https://pages.antithesis.com/webinar-property-based-testing-pragmatic?utm_medium=Email&utm_campaign=pragmatic_2026&utm_source=pragmatic&utm_content=pbt_20261001).

• **[Entire](https://entire.io/pragmatic)** – Git hosting, rebuilt for the agentic era. Entire hosts your code in-region, and is up to 89x faster than any other competitor. Mirror from GitHub [with a single click](https://entire.io/pragmatic) – I’ve already done so.

Sam Newman wrote one of the most-read books about microservices (“[Building Microservices](https://samnewman.io/books/building_microservices/)”), but he calls them an architecture of “last resort.”

In this episode of the Pragmatic Engineer podcast, Sam explains his thinking on this, and he’s certainly well placed to do so; he was in the room when the term “microservices” was coined. We discuss what teams get wrong when adopting microservices, why independent deployment matters, and how microservices can help teams work more autonomously.

We also peek into his new book, ‘**[Building Resilient Distributed Systems](https://www.oreilly.com/library/view/building-resilient-distributed/9781098163532/)**’, and Sam explains his three rules for distributed systems, why observability is essential, and also why it’s vital to take business context into account when deciding whether to fail open or fail closed on errors.

We also explore how AI is changing software development, from specs versus code as a source of truth to cognitive debt and cognitive surrender. Sam shares how modular architecture can help teams experiment with AI while maintaining understanding of the systems being built.

[www.youtube.com](https://www.youtube.com/watch?v=a0sZV0qIbI0)

**1\. In the room when “microservices” was coined**. In the early years of the 2010s, James Lewis – Sam’s colleague at Thoughtworks – kept running into companies building service-oriented architectures where each service could be replaced quickly. At an architecture symposium in the Lake District, England, James pitched a name for this trend: “micro apps,” and in a room of about 10 people (including Sam), someone else suggested “microservices.” Lewis and Martin Fowler then wrote the first article on the topic in March 2014, ‘[Microservices: a definition of this new architectural term](https://martinfowler.com/articles/microservices.html)’, and Sam published the book [Building Microservices](https://samnewman.io/books/building_microservices/) the following year.

**2\. Sam spent 18 months teaching Googlers how to write good automated tests**. In 2007, Sam was embedded at Yahoo and Google as a ThoughtWorks consultant, alongside people from Pivotal Labs. Their job was to teach engineers how to write automated tests, and code that’s testable. He recalls that some Google devs did not believe it was possible to write automated functional tests for websites, until Sam and his colleagues showed them how Selenium works!

**3\. Sam spent 10+ years at a large company, three years at startups, and has been independent for more than a decade.** After 13 years at Thoughtworks, he quit to avoid becoming “institutionalized.” He recalls thinking he could hang around at Thoughtworks and grow bitter. “So I’ll just go now.” He then worked at three different startups before going independent, which he’s now done for 15 years. The biggest challenge of being your own boss? He says: “The problem of working for yourself is that the boss is frequently an a\*\*\*\*\*e. So, current Sam frequently is cursing past Sam for the commitments he made!”

**4\. So, what’s a “microservice**?” Sam gives two definitions: a clear one, and a softer one:

1. **Independently deployable service**. The “clear” definition: as long as, upon a change to a system, it can be independently deployed – without any other services needing to be deployed –, you can call it a microservice.

2. **Clear boundaries around the business domain.** The “softer” definition: services split by business function (not by technical layer) can also be called microservices.

**5\. Three rules of distributed systems**: Sam sometimes struggled to recall the [eight fallacies of distributed computing](https://en.wikipedia.org/wiki/Fallacies_of_distributed_computing), and so he boiled them down to the three that cause the most problems:

1\. Information takes time to travel. You cannot send information instantaneously from point A to point B. This is basically the “speed of light” constraint.

2\. Sometimes, the thing you want to talk to isn’t there. That is, resources can and do go offline.

3\. Resources are not infinite; you will eventually run out of things like CPU, memory, storage, and networking bandwidth.

In Sam’s experience, most distributed systems outages are caused by #3; resource pools running out.

**6\. Dealing with idempotency: keys or fingerprints**? Idempotency is performing the same operation multiple times and it having the same effect as if done once only. For example, if you repeatedly hit the “down” button for an elevator, nothing more happens than what happened upon that first press: multiple button smashes don’t bring more elevators your way. Payments are a classic example where idempotency is needed: when you retry or refresh a payments page, you should not be charged twice.

There are two common ways to deal with idempotency:

- **Idempotency keys**: a client sends a unique identifier (usually a UUID) that is the idempotency key. The server makes sure operations with the same idempotency key are executed only once. It’s clean to build but hard to retrofit, which involves changing both the client and server.

- **Fingerprints:** the client sends nothing, but the server creates a “fingerprint” of the request by hashing relevant fields. It’s easy to retrofit later (only need to change the server), however, legitimate requests can be rejected as duplicates, and duplicate requests could still happen. Sam describes a Danish payment system he worked on that rejected purchases of the same value within a 5-minute period while it did the fingerprinting.

**7\. Four concepts of resiliency:** Sam frequently references the four outcomes for military systems, explained [by David Woods](https://nps.edu/web/eag/practical-applications-of-woods-four-resilience-concepts):

- **Robustness**: systems continue to function as intended, for example, when Kubernetes replaces a failing pod. A downside of robustness is that it can add complexity, and more complex systems have more ways to fail.

- **Rebound**: how quickly the system recovers when something goes wrong.

- **Extensibility**: dealing with surprises and being able to stretch the system’s support to deal with new needs

- **Adaptability**: the system changing to support new functions. Resilient systems are adapted, even between incidents.

**8\. What if code is a side effect of collaboration**? When Sam was at Thoughtworks, some clients refused to do pair programming as a practice because they could only see one developer typing and the other doing nothing, which is a waste. Martin Fowler’s dictum that “programming is not typing” helped Sam to explain to clients that typing out code is an output, but not an outcome.

Sam conceives programming as building a system collectively, and code is one of the mechanisms delivering the system. Now that with LLMs we no longer spend most time typing out code, we’ll discover the next bottleneck in creating good software. Could it be delayed feedback cycles?

**9\. Hedging the vendor options is sensible when integrating AI into systems**. It’s hard to tell which AI companies have long-term, sustainable business models and which don’t, so it’s the responsible thing when designing systems to hedge the providers. Aim to be multi-vendor, multi-model in your choices, so you can easily change the vendor or model layer. Also, consider if you can swap out LLM-powered functionality for deterministic code that runs faster and cheaper.

**10\. We must resist “cognitive surrender” to AI**. Sam believes that more AI usage is leading to less critical thinking – or could we be using it wrong? He says: “The original pitch of AI was it was going to free us from drudgery. We’d have less toil, and we can focus more on critical thinking and smarts. However, much of our current use of AI is not freeing us up from critical thinking! It’s causing *more* context switching and leaving less time for thinking. People are working longer hours; we’ve got studies that show this.

But \[longer hours and more context switching\] is not necessarily AI’s fault. It’s just how we’re using it.”

Putting in the effort often leads to better results. For example, when writing Building Resilient Distributed Systems, Sam used NotebookLM for research, but he manually clicked through each link it surfaced, and checked whether it was information he wanted to reference.

**11\. Starting with modules encourages better software architecture. Also: production is truth**. I asked Sam for advice on what to do to get better at designing systems:

- Start by thinking carefully about module boundaries and the connections between them

- When you use AI, allow it to roam freely, but only *inside* the module structure that you have designed!

**12\. Much of the tech world might still be naive about what an LLM is**. As Sam said:

> “I think the tech world in general is quite naive about AI and fundamentally misunderstands what an LLM is. For example: why did the LLM delete my database? Well, because it has no concept of causality. They have no concept that if I do A, B happens.
>
> LLMs are not world models. World models have been around for a long time. They predate LLMs. So if you had the appropriate model and the appropriate inputs, world models could at least get to the point of ‘if I do X, then Y happens and I’ve been told not to do Y.’
>
> **I think we expect LLMs to do more than they actually can do, because they seem so smart.** This leads us to overestimate their capabilities a bit too much. But the nature of LLMs is why all the guardrails around LLMs are really not going to be the right long-term solution.”

Sam’s comments feel very timely, given Opus 5.5 [just formatted the hard drive of a dev](https://x.com/PerceptualPeak/status/2107621483392446572?s=20) who ran the model with the —dangerously-skip-permissions tag, and was surprised to see all of his data gone.

[![Image](https://substackcdn.com/image/fetch/$s_!U5wh!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fee80d977-426c-419f-9461-e7e8ee11f3eb_1806x570.png "Image")](https://substackcdn.com/image/fetch/$s_!U5wh!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fee80d977-426c-419f-9461-e7e8ee11f3eb_1806x570.png)

Opus 5.5 decided it would be a good idea to format the C: drive. Sam warned about this kind of outcome due to the nature of LLMs. Screenshot source: [X](https://x.com/PerceptualPeak/status/2107621483392446572?s=20)

**Finally, recommended reading to get better at software architecture.** Sam’s recommendations:

- [Fundamentals of Software Architecture](https://www.amazon.com/dp/1492043451) by Neal Ford and Mark Richards.

- [Balanced Coupling](https://coupling.dev/) by Vlad Khononov

- [On the criteria to be used in decomposing systems into modules](https://dl.acm.org/doi/10.1145/361598.361623): a paper by David Parnas from the 1970s original papers from the early 1970s, such as

- [An architecturally-evident coding style: making your design visible](https://dl.acm.org/doi/10.1145/1869542.1869627): a paper by George Fairbanks, published in 2010. It’s an approach that allows the code to hint at the system’s architecture

• [Scaling Uber with Thuan Pham (Uber’s first CTO)](https://newsletter.pragmaticengineer.com/p/scaling-uber-with-thuan-pham-ubers)

• [What is good software architecture at Netflix?](https://newsletter.pragmaticengineer.com/p/what-is-good-software-architecture)

• [The past and future of modern backend practices](https://newsletter.pragmaticengineer.com/p/the-past-and-future-of-backend-practices)

• [What is reliability engineering, and the history of SRE](https://newsletter.pragmaticengineer.com/p/reliability-engineering)

• [How to debug large, distributed systems: Antithesis](https://newsletter.pragmaticengineer.com/p/antithesis)

• [Designing Data-intensive Applications with Martin Kleppmann](https://newsletter.pragmaticengineer.com/p/designing-data-intensive-applications)

• [Building Bluesky: a Distributed Social Network](https://newsletter.pragmaticengineer.com/p/bluesky)

[00:00](https://www.youtube.com/watch?v=a0sZV0qIbI0) Intro

[03:16](https://www.youtube.com/watch?v=a0sZV0qIbI0&t=196s) Sam’s path into tech

[09:32](https://www.youtube.com/watch?v=a0sZV0qIbI0&t=572s) Thoughtworks

[20:55](https://www.youtube.com/watch?v=a0sZV0qIbI0&t=1255s) The rise of microservices

[33:37](https://www.youtube.com/watch?v=a0sZV0qIbI0&t=2017s) Are specs becoming more important than code?

[45:22](https://www.youtube.com/watch?v=a0sZV0qIbI0&t=2722s) Building Resilient Distributed Systems (Sam’s new book)

[52:35](https://www.youtube.com/watch?v=a0sZV0qIbI0&t=3155s) Three rules of distributed systems

[56:16](https://www.youtube.com/watch?v=a0sZV0qIbI0&t=3376s) Observability

[1:02:20](https://www.youtube.com/watch?v=a0sZV0qIbI0&t=3740s) Resilience tradeoffs

[1:07:54](https://www.youtube.com/watch?v=a0sZV0qIbI0&t=4074s) Idempotency

[1:15:58](https://www.youtube.com/watch?v=a0sZV0qIbI0&t=4558s) Thundering herds

[1:21:03](https://www.youtube.com/watch?v=a0sZV0qIbI0&t=4863s) Business context and resilience decisions

[1:25:51](https://www.youtube.com/watch?v=a0sZV0qIbI0&t=5151s) Resilience engineering: four concepts

[1:32:42](https://www.youtube.com/watch?v=a0sZV0qIbI0&t=5562s) AI and resilience

[1:36:26](https://www.youtube.com/watch?v=a0sZV0qIbI0&t=5786s) AI’s limitations and where to use it

[1:40:06](https://www.youtube.com/watch?v=a0sZV0qIbI0&t=6006s) Cognitive debt and cognitive surrender

[1:45:02](https://www.youtube.com/watch?v=a0sZV0qIbI0&t=6302s) Modular architecture and AI software factories

[1:51:53](https://www.youtube.com/watch?v=a0sZV0qIbI0&t=6713s) Resources for learning software architecture

[1:55:30](https://www.youtube.com/watch?v=a0sZV0qIbI0&t=6930s) Where to find Sam

**Where to find Sam Newman:**

• X: [https://x.com/samnewman](https://x.com/samnewman)

• LinkedIn: [https://www.linkedin.com/in/samnewman](https://www.linkedin.com/in/samnewman)

• Website: [https://samnewman.io](https://samnewman.io)

**Mentions during the episode:**

[Uptime Labs](https://www.uptimelabs.io/): incident response training, backed by science. In the episode, Sam said this about them:

> “I’d like to give a shout out to Uptime Labs who are based here partly here in London. They do excellent instant training that you can do online in a web browser. It’s fantastic. They are really great.”

• Acorn Electron: [https://en.wikipedia.org/wiki/Acorn_Electron](https://en.wikipedia.org/wiki/Acorn_Electron)

• Commodore 64: [https://en.wikipedia.org/wiki/Commodore_64](https://en.wikipedia.org/wiki/Commodore_64)

• Alstom: [https://www.alstom.com](https://www.alstom.com)

• Fortran: [https://en.wikipedia.org/wiki/Fortran](https://en.wikipedia.org/wiki/Fortran)

• Thoughtworks: [https://www.thoughtworks.com](https://www.thoughtworks.com)

• TDD, AI agents and coding with Kent Beck: [https://newsletter.pragmaticengineer.com/p/tdd-ai-agents-and-coding-with-kent](https://newsletter.pragmaticengineer.com/p/tdd-ai-agents-and-coding-with-kent)

• How Kent Beck shapes the software engineering industry: [https://newsletter.pragmaticengineer.com/p/how-kent-beck-shapes-the-software](https://newsletter.pragmaticengineer.com/p/how-kent-beck-shapes-the-software)

• Extreme programming: [https://martinfowler.com/bliki/ExtremeProgramming.html](https://martinfowler.com/bliki/ExtremeProgramming.html)

• Growing Object-Oriented Software Guided by Tests: [https://growing-object-oriented-software.com/](https://growing-object-oriented-software.com/)

• Dave Farley’s website: [https://www.davefarley.net](https://www.davefarley.net)

• Jez Humble on X: [https://x.com/jezhumble](https://x.com/jezhumble)

• Continuous Delivery: [https://continuousdelivery.com](https://continuousdelivery.com)

• Mike Bland’s publications: [https://mike-bland.com/publications](https://mike-bland.com/publications)

• How AI will change software engineering – with Martin Fowler: [https://newsletter.pragmaticengineer.com/p/martin-fowler](https://newsletter.pragmaticengineer.com/p/martin-fowler)

• James Lewis on LinkedIn: [https://www.linkedin.com/in/james-lewis-microservices](https://www.linkedin.com/in/james-lewis-microservices)

• Building Microservices: Designing Fine-Grained Systems: [https://www.amazon.com/dp/1492034029](https://www.amazon.com/dp/1492034029)

• Microservices Guide: [https://martinfowler.com/microservices](https://martinfowler.com/microservices)

• Ben Christensen on LinkedIn: [https://www.linkedin.com/in/benjchristensen](https://www.linkedin.com/in/benjchristensen)

• Scaling Uber with Thuan Pham (Uber’s first CTO): [https://newsletter.pragmaticengineer.com/p/scaling-uber-with-thuan-pham-ubers](https://newsletter.pragmaticengineer.com/p/scaling-uber-with-thuan-pham-ubers)

• Stop being skeptical about AI for development with Charity Majors: [https://newsletter.pragmaticengineer.com/p/stop-being-skeptical-about-ai-for](https://newsletter.pragmaticengineer.com/p/stop-being-skeptical-about-ai-for)

• Understanding Spec-Driven-Development: Kiro, spec-kit, and Tessl: [https://martinfowler.com/articles/exploring-gen-ai/sdd-3-tools.html](https://martinfowler.com/articles/exploring-gen-ai/sdd-3-tools.html)

• Chris Ford on LinkedIn: [https://www.linkedin.com/in/ctford](https://www.linkedin.com/in/ctford)

• Beyond Vibe Coding with Addy Osmani: [https://newsletter.pragmaticengineer.com/p/beyond-vibe-coding-with-addy-osmani](https://newsletter.pragmaticengineer.com/p/beyond-vibe-coding-with-addy-osmani)

• Building Resilient Distributed Systems: [https://samnewman.io/books/building-resilient-distributed-systems](https://samnewman.io/books/building-resilient-distributed-systems)

• Honeycomb: [https://www.honeycomb.io](https://www.honeycomb.io)

• Monzo: [https://monzo.com](https://monzo.com)

• How Generative and Agentic AI Shift Concern from Technical Debt to Cognitive Debt: [https://margaretstorey.com/blog/2026/02/09/cognitive-debt](https://margaretstorey.com/blog/2026/02/09/cognitive-debt)

• Fundamentals of Software Architecture: A Modern Engineering Approach: [https://www.amazon.com/dp/1098175514](https://www.amazon.com/dp/1098175514)

• Balancing Coupling in Software Design: Universal Design Principles for Architecting Modular Software Systems: [https://www.amazon.com/Balancing-Coupling-Software-Design-Addison-wesley/dp/0137353480](https://www.amazon.com/Balancing-Coupling-Software-Design-Addison-wesley/dp/0137353480)

• Learning Domain-Driven Design: Aligning Software Architecture and Business Strategy: [https://www.amazon.com/Learning-Domain-Driven-Design-Vlad-Khononov-ebook/dp/B09J2CMJZY](https://www.amazon.com/Learning-Domain-Driven-Design-Vlad-Khononov-ebook/dp/B09J2CMJZY)

• Software Fundamentals: Collected Papers by David L. Parnas: [https://www.abebooks.com/9780201703696/Software-Fundamentals-Collected-Papers-David-0201703696/plp](https://www.abebooks.com/9780201703696/Software-Fundamentals-Collected-Papers-David-0201703696/plp)

• Just Enough Software Architecture: [https://www.georgefairbanks.com/book](https://www.georgefairbanks.com/book)

• Modern Software Engineering: [https://www.youtube.com/c/ContinuousDelivery](https://www.youtube.com/c/ContinuousDelivery)

—

Production and marketing by [Pen Name](https://penname.co/).
