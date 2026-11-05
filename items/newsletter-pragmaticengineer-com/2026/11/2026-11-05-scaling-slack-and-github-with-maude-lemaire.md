---
title: Scaling Slack and GitHub with Maude Lemaire
link: https://newsletter.pragmaticengineer.com/p/scaling-slack-and-github-with-maude
source: newsletter-pragmaticengineer-com
published: 2026-11-05T15:58:00Z
updated: 2026-11-05T15:58:00Z
first_seen: 2026-11-05T17:57:26.867095589Z
authors:
- Gergely Orosz
summary: Maude Lemaire shares lessons from scaling Slack and GitHub, improving performance at Cursor, and using AI while keeping engineering judgment in human hands.
content: extracted
html: 2026-11-05-scaling-slack-and-github-with-maude-lemaire.html
remote_preview:
  url: https://substackcdn.com/image/fetch/$s_!_et5!,w_1200,h_600,c_fill,f_jpg,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5cd802b9-eecc-4029-9fa8-3ba97853bb13_1280x720.png
extra:
  audio_type: audio/mpeg
  audio_url: https://api.substack.com/feed/podcast/223203725/962a5df4fc1a3b45483ec99f2f424ac4.mp3
  duration_seconds: 4713
---

**Listen and watch now on [YouTube](https://youtu.be/x6WqD4qHxSc), [Apple](https://podcasts.apple.com/us/podcast/the-pragmatic-engineer/id1769051199), and [Spotify](https://open.spotify.com/show/2Bho9xCbOQMWMJ7UKmqCzD).** See the episode transcript at the top of this page, and timestamps for the episode at the bottom.

![](https://substackcdn.com/image/fetch/$s_!AlJu!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8e3d3c6f-6a37-411e-aa1a-dfcefc979af3_1600x140.png)

• **[turbopuffer](http://turbopuffer.com/pragmatic)** – The tpuf eng team is completely redesigning their storage architecture from first principles. And they’re documenting all of it! They made epic progress; take a look: [turbopuffer.com/v3](http://turbopuffer.com/v3)

• **[Antithesis](https://antithesis.com/pragmatic)** – use AI agents to work on critical systems without worrying about correctness. Antithesis runs your complete system in a hostile environment, analyzes its behavior, and reproduces every issue perfectly. [Learn more](https://antithesis.com/pragmatic)

• **[Entire](https://entire.io/pragmatic)** – Entire [launched](https://entire.com/about?utm_source=pragmatic#code-hosting) its own Git forge, shipped [Continuous Memory](https://entire.com/about?utm_source=pragmatic#continuous-memory), and reinvented the pull request (called [Trails](https://entire.com/about?utm_source=pragmatic#trails)). Trails are a much more natural workflow when agents keep pushing new changes, after iterating on feedback – and you have more context and control on when a change is ready to merge. [Check it out](https://entire.com/pragmatic)

[![](https://substackcdn.com/image/fetch/$s_!Patg!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc2711f84-e939-4315-a4b4-4462f1e45735_2360x1338.png)](https://substackcdn.com/image/fetch/$s_!Patg!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc2711f84-e939-4315-a4b4-4462f1e45735_2360x1338.png)

*The reimagined concept of the pull request for agentic development: Entire calls them ‘[Trails](https://entire.com/about?utm_source=pragmatic#trails)’*

What is involved in scaling up large, popular products at places like Slack and GitHub?

We get an insider’s view on this from [Maude Lemaire](https://www.linkedin.com/in/lemairemaude), a principal engineer and author of [‘](https://www.oreilly.com/library/view/refactoring-at-scale/9781492075523/)*[Refactoring at Scale: Regaining Control of Your Codebase](https://www.oreilly.com/library/view/refactoring-at-scale/9781492075523/)*.’ She spent nearly eight years at Slack, where she led the backend performance infrastructure team, before joining GitHub to tackle database scaling challenges. Most recently, she worked on improving performance at Cursor – including time to first token.

In this episode of *The Pragmatic Engineer*, Maude and I dig into the challenges of scaling some of the world’s largest software products. We explore how Slack handled growing numbers of enterprise customers, how it evolved its approach to load testing, and the lessons Maude learned during nearly eight years of scaling Slack’s infrastructure.

She also tells us what it was like joining GitHub in 2024 when Copilot was taking off, the challenges of working with GitHub’s gigantic Rails monolith, and how Maude uses AI in her work – while keeping judgment calls and decision-making firmly in her own hands.

[www.youtube.com](https://www.youtube.com/watch?v=x6WqD4qHxSc)

Here are eight interesting points from our discussion:

**1\. Performance should matter even if there’s no competition**. In 2015, the homepage of Rent the Runway took up to 10 seconds to fully load due to a high-resolution hero image on the site. Maude was told it wasn’t an issue because there were no competitors to worry about. She thought differently: “The best competition is competition with yourself, knowing that you can do better, knowing that you can drive a better experience, ” she said. Maude spent some free time on speeding up the website without asking for permission to do so because it was the right call.

**2\. Slack limits custom emojis to 23 to avoid thundering herds**. Here’s an example of a [thundering herd](https://en.wikipedia.org/wiki/Thundering_herd_problem): the CEO of a 100,000-member Slack channel posts “everyone gets Friday off!” In response, people start posting custom emojis – hundreds of them. The problem is that every single custom emoji has to fan out to all 100,000 users. With 10 custom emojis, that’s 1,000,000 fan-outs, with 100 it’s 10,000,000; a lot of network load! Slack limits custom emojis to 23 to avoid this problem. In reality, the absent 24th custom emoji is not missed.

**3\. Event floods can cripple the machines of Slack’s power users**. Event floods are the cousin of thundering herds: they occur when a single client is overwhelmed by events coming through its own WebSocket. For example, when an IT worker is logged into lots of busy channels and they all receive the “everyone gets Friday off!” message – along with all the associated custom emojis and messages. This “power user” gets a sudden influx of messages with no deduplication and they struggle.

**4\. Load testing 500,000 WebSocket connections the simple way would have cost $150,000 per run**. A new customer with 500,000 users was about to sign up to Slack, and Maude needed to run a load test to confirm the service could handle the load. Initially, she planned to spin up headless Chrome instances – each instance running a Slack session, and that would have cost $0.37 per five-hour session, per instance. The total price would’ve been crazily expensive. Instead, Maude built a tool called [Koi Pond](https://slack.engineering/load-testing-with-koi-pond/) that spins up slimmed-down versions of Slack (‘Koi’), written in Go to establish the WebSocket connections.

**5\. Active Record downside: too much database load**! GitHub used to be a Ruby on Rails monolith, which used [Active Record](https://guides.rubyonrails.org/active_record_basics.html) as a database abstraction layer. This caused unexpected load: GitHub’s home page fetched live data from the database for most reloads, and loading the home page regularly triggered 100 SQL queries across three database clusters. This revealed a downside of abstractions like Active Record to Maude: when devs used it, they didn’t know or care about the additional load they unwittingly placed on the database while fetching data for whatever they were working on.

**6\. GitHub could not add more than 16 database shards because of scatter query problems**. A scatter query is one without a primary key (a sharding key), and it needs to be run across all [shard instances](https://en.wikipedia.org/wiki/Shard_\(database_architecture\)). But more shards make such queries slower.

Back in the day at GitHub, lots of queries made through Active Record (many on the homepage) were scatter queries. This limitation prevented GitHub from adding more than 16 shards. I find this detail fascinating: it shows why a developer-friendly abstraction (Active Record) can actually cause severe pain at the infra level!

**7\. For platform work, Maude personally verifies everything she gets from an LLM**. When doing foundational work, Maude pays close attention to verifying the output of LLMs because she believes that ambitious structures can only be built on stable foundations. It’s an interesting approach to separate “foundational” and “non-foundational” work in order to give more attention to the former, most critical, type.

**8\. Platform and product engineers should rotate more**. The best platform engineers Maude has worked with are product engineers who reimplemented similar features a few times, grew frustrated with the repetitive toil, and so built tools to remove it once and for all. But an issue arises when platform engineers become detached from product development and drift away from engineers’ experience of using the product. To remedy this, Maude suggests rotation cycles where a platform engineer joins a product team to build a feature, and a product engineer joins the platform team.

• [Real-world engineering challenges: building Cursor](https://newsletter.pragmaticengineer.com/p/cursor)

• [Pushing software engineering limits with “napkin math”](https://newsletter.pragmaticengineer.com/p/pushing-software-engineering-limits)

• [What is happening with code reviews?](https://newsletter.pragmaticengineer.com/p/what-is-happening-with-code-reviews)

• [Why reliability is hard at scale: learnings from infrastructure outages](https://newsletter.pragmaticengineer.com/p/why-reliability-is-hard-at-scale)

• [What is Reliability Engineering?](https://newsletter.pragmaticengineer.com/p/reliability-engineering)

• [Migrations Done Well](https://newsletter.pragmaticengineer.com/p/migrations)

• [Shipping to Production](https://newsletter.pragmaticengineer.com/p/shipping-to-production)

• [The Platform and Program Split at Uber](https://newsletter.pragmaticengineer.com/p/program-platform-split-uber)

• [The present, past and future of GitHub](https://newsletter.pragmaticengineer.com/p/github)

• [Why performant code matters (but gets widely ignored), with Casey Muratori](https://newsletter.pragmaticengineer.com/p/why-performant-code-matters-but-gets)

• [Designing Data-intensive Applications with Martin Kleppmann](https://newsletter.pragmaticengineer.com/p/designing-data-intensive-applications)

[00:00](https://www.youtube.com/watch?v=x6WqD4qHxSc) Intro

[03:15](https://www.youtube.com/watch?v=x6WqD4qHxSc&t=195s) Early years and path to software engineering

[07:32](https://www.youtube.com/watch?v=x6WqD4qHxSc&t=452s) First job at Rent the Runway

[09:17](https://www.youtube.com/watch?v=x6WqD4qHxSc&t=557s) The HipChat data leak

[10:54](https://www.youtube.com/watch?v=x6WqD4qHxSc&t=654s) Joining Slack and the challenges of scaling

[26:30](https://www.youtube.com/watch?v=x6WqD4qHxSc&t=1590s&pp=0gcJCWMAwfN6Pr3D) Load testing and thundering herds

[32:29](https://www.youtube.com/watch?v=x6WqD4qHxSc&t=1949s) Event floods

[38:30](https://www.youtube.com/watch?v=x6WqD4qHxSc&t=2310s) Evolving load testing at Slack

[48:38](https://www.youtube.com/watch?v=x6WqD4qHxSc&t=2918s) Joining GitHub in 2024

[56:52](https://www.youtube.com/watch?v=x6WqD4qHxSc&t=3412s) The challenges of the Rails monolith

[1:02:26](https://www.youtube.com/watch?v=x6WqD4qHxSc&t=3746s) How Maude uses AI

[1:12:40](https://www.youtube.com/watch?v=x6WqD4qHxSc&t=4360s) Advice for engineers interested in product/platform work

**Where to find Maude Lemaire:**

• X: [https://x.com/QcMaude](https://x.com/QcMaude)

• LinkedIn: [https://www.linkedin.com/in/lemairemaude](https://www.linkedin.com/in/lemairemaude)

• Website: [https://maudethecodetoad.com](https://maudethecodetoad.com)

**Mentions during the episode:**

• The Manager’s Path: A Guide for Tech Leaders Navigating Growth and Change:[https://www.amazon.com/dp/1491973897](https://www.amazon.com/dp/1491973897)

• Building Koi Pond: Simulating Millions of Slack Clients:[Strange Loop](https://www.thestrangeloop.com/2023/building-koi-pond-simulating-millions-of-slack-clients.html),[LeadDev](https://leaddev.com/technical-direction/building-koi-pond-simulating-millions-slack-clients),[Presentation PDF](https://leaddev.com/wp-content/uploads/2022/12/Building-Koi-Pond-Simulating-Millions-of-Slack-Users.pdf)

• Load Testing with Koi Pond:[https://slack.engineering/load-testing-with-koi-pond/](https://slack.engineering/load-testing-with-koi-pond/)

• Thundering herd problem:[https://en.wikipedia.org/wiki/Thundering_herd_problem](https://en.wikipedia.org/wiki/Thundering_herd_problem)

• Flamework:[https://github.com/exflickr/flamework](https://github.com/exflickr/flamework)

• Vitess: [https://vitess.io/](https://vitess.io/)

• Prisma: [https://www.prisma.io/](https://www.prisma.io/)

• Uchat:[https://www.uber.com/us/en/blog/uchat/](https://www.uber.com/us/en/blog/uchat/)

• Mattermost: [https://mattermost.com/](https://mattermost.com/)

• Rent the Runway: [https://www.renttherunway.com/](https://www.renttherunway.com/)

• Year 2038 problem:[https://en.wikipedia.org/wiki/Year_2038_problem](https://en.wikipedia.org/wiki/Year_2038_problem)

• HipChat:[https://en.wikipedia.org/wiki/HipChat](https://en.wikipedia.org/wiki/HipChat)

• Refactoring at Scale: Regaining control of your codebase:[https://www.oreilly.com/library/view/refactoring-at-scale/9781492075523](https://www.oreilly.com/library/view/refactoring-at-scale/9781492075523)

• Thread about building uChat on X:

> Gergely Orosz @GergelyOrosz
>
> Oh, HipChat broke down for Uber even before (Atlassian btw really fumbled HipChat - it could have become what Slack is today, it had the momentum! And then it just... died, and Atlassian retired it for good)
>
> > Benjamin Booth @benjaminbooth
> >
> > @GergelyOrosz By Oct, we weren’t close to anything releasable. Forget secure, on the App Store, tested, etc. Halloween was and may still be a critical night for Uber, due the drunken revelree. Again, Uber had expensive outages exacerbated by unavailable HipChat. 2017 NYE was our focus.
>
> [@GergelyOrosz on X](https://x.com/GergelyOrosz/status/2032840848979755012)

1 Reply · 28 Likes

• Cal Henderson on X:[https://x.com/iamcal](https://x.com/iamcal)

• GitHub: [https://github.com](https://github.com)

• The present, past and future of GitHub:[https://newsletter.pragmaticengineer.com/p/github](https://newsletter.pragmaticengineer.com/p/github)

• Azure: [https://azure.microsoft.com](https://azure.microsoft.com)

• Cursor: [https://cursor.com](https://cursor.com)

• Real-world engineering challenges: building Cursor:[https://newsletter.pragmaticengineer.com/p/cursor](https://newsletter.pragmaticengineer.com/p/cursor)

—

Production and marketing by [Pen Name](https://penname.co/).
