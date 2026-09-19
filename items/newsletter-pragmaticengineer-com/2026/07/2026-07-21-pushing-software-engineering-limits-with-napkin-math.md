---
title: Pushing software engineering limits with “napkin math”
link: https://newsletter.pragmaticengineer.com/p/pushing-software-engineering-limits
source: newsletter-pragmaticengineer-com
published: 2026-07-21T16:52:07Z
updated: 2026-07-21T16:52:07Z
first_seen: 2026-09-19T21:30:23.395188495Z
authors:
- Gergely Orosz
summary: Turbopuffer cofounder Simon Eskildsen on the benefits of longer tenure, using first principles to build durable software – and why founders should be cautious when raising VC money
content: extracted
html: 2026-07-21-pushing-software-engineering-limits-with-napkin-math.html
preview:
  file: 2026-07-21-pushing-software-engineering-limits-with-napkin-math.preview-c882998f2603.webp
  width: 256
  height: 144
  color: '#3a3839'
images:
- source: https://substackcdn.com/image/youtube/w_728,c_limit/h8TBLKyo7Rs
  original:
    file: 2026-07-21-pushing-software-engineering-limits-with-napkin-math.image-f66f280d4638.jpg
    width: 728
    height: 410
  color: '#2d2d2d'
- source: https://substackcdn.com/image/fetch/$s_!Gp-r!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa7d73bb2-e470-41b1-afe7-059ddbd78d55_1294x916.png
  original:
    file: 2026-07-21-pushing-software-engineering-limits-with-napkin-math.image-24f1e827d216.jpg
    width: 1294
    height: 916
  color: '#220926'
- source: https://substackcdn.com/image/fetch/$s_!9au0!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9fdba208-a817-42fe-b4a0-f6ba813d8541_512x135.png
  original:
    file: 2026-07-21-pushing-software-engineering-limits-with-napkin-math.image-1d971a797949.jpg
    width: 512
    height: 135
  color: '#a6a6a6'
- source: https://i.ytimg.com/vi/h8TBLKyo7Rs/hqdefault.jpg
  original:
    file: 2026-07-21-pushing-software-engineering-limits-with-napkin-math.image-20e9d0b63279.jpg
    width: 480
    height: 360
  color: '#010101'
---

*Hi, this is Gergely with the monthly, free issue of the Pragmatic Engineer Newsletter. In every issue, I cover Big Tech and startups through the lens of senior engineers and engineering leaders. If you’ve been forwarded this email, you can [subscribe here](https://newsletter.pragmaticengineer.com/about?ref=blog.pragmaticengineer.com) .*

After I recently interviewed Simon Eskildsen, co-founder and CEO of turbopuffer, on the main stage at the AI Engineer’s World Fair, many people at the event told me they found him relatable and inspiring for his choices to stick with one company for close to a decade, his belief in the power of “napkin math” to reveal why products run slowly or cost too much, and for his insight about how too much of VC funding is about ego, not business needs.

This article contains the most interesting parts from that conversation; in particular, the concept of “napkin math” – doing quick calculations to get rough answers – as a way to challenge existing systems to improve. The full 55-minute-long video of the discussion at the AI Engineer’s World Fair is available to watch:

[Watch the full interview](https://youtu.be/h8TBLKyo7Rs)

Today, we cover:

1. **Algorithmic programming speedrun.** While in high school, Simon competed in the International Olympiad for Informatics (IOI) which pushed him to learn about writing correct programs that are fast and memory-efficient, and more.

2. **Eight years of infra at Shopify.** There are many upsides to longer tenure: Simon learned infrastructure concepts, dug deep into databases running across regions, and learned to write software that ages well.

3. **“Napkin math” as a superpower.** Simon became obsessed with finding the theoretical limits of compute operations, such as sending over data and reading bytes. This held him in good stead at Shopify, and then at his startup.

4. **Origins of turbopuffer**. When ChatGPT took off, context windows were small, and so stuffing them with the right information was key for AI applications. Fast search was needed, but the search solutions were surprisingly expensive. Using “napkin math”, Simon discovered they were *far* more expensive than necessary.

5. **A new product without VC funding & Cursor as customer no.1.** After raising $8M in seed funding, Cursor rolled the dice on the new turbopuffer team after Simon helped with their search & database needs.

6. **Reasons to raise venture capital**. Fund R&D, fund growth, stroke founders’ egos, and more.

*Disclaimer: turbopuffer is a season sponsor [of the podcast](https://pragmaticpodcast.com/) , but as with all our deepdives, this article is independent of podcast sponsorships.*

A self-taught professional, Simon skipped college to work at Shopify and spent nearly a decade there building a variety of systems. His interest in computers started after initially getting into building websites aged 12. Growing up in Denmark, he dabbled in HTML with tools like Microsoft FrontPage and Adobe’s Dreamweaver.

While still a teenager, Simon “hit a wall” by exhausting the Danish-language part of the internet for learning programming, and got into the World of Warcraft MMO game, which helped him acquire English.

After discovering the International Olympiad for Informatics (IOI), he decided to enter, despite a very competitive, multi-stage selection process open to all Danish high schools. IOI problems are pretty similar to Leetcode problems: algorithmic challenges that value correctness, speed, and memory usage.

Simon cleared the online qualification round and was invited to the Danish Nationals. This was more than a competition: a weekend-long bootcamp to teach participants more advanced programming techniques such as recursion (which Simon already knew), the [divide-and-conquer algorithm](https://en.wikipedia.org/wiki/Divide-and-conquer_algorithm) , and [dynamic programming](https://en.wikipedia.org/wiki/Dynamic_programming) ; one of the more tricky concepts to master for algorithmic programming.

I tip my hat to the organizers for creating a challenging bootcamp and offering the opportunity for people like Simon, who wasn’t aware of what “NP complete” meant when he entered. [He recalls](https://sirupsen.com/my-journey-to-the-international-olympiad-in-informatics) :

> “The routine was that every four hours we’d be introduced to a new “programming concept”, and receive ~2-6 tasks where this, combined with previously introduced concepts, had to be applied. All the solutions had to be submitted to the same site I submitted my qualification solutions to, as it was all part of the final evaluation. The tasks were incredibly challenging, like nothing I had ever tried before.
>
> Sometimes in extreme desperation combined with tiredness from the trip, I’d think about taking the next train home. This feeling would disappear with the utter joy and confidence that arose whenever I would finally solve a task, and creep back once again when I found myself still struggling after an hour on a new problem. But this kept me going. By Saturday afternoon, I had almost managed to get up to speed with the others, and was doing the same tasks as them.”

Ultimately, Simon claimed one of six spots in the Danish national team, making it through to the regional finals.

**Aware of how little he knew about programming, Simon doubled down to catch up.** He realized that most other participants were better prepared and that he had to do something to survive the next qualifying round. So, he got to work:

[![](https://substackcdn.com/image/fetch/$s_!Gp-r!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa7d73bb2-e470-41b1-afe7-059ddbd78d55_1294x916.png)](https://substackcdn.com/image/fetch/$s_!Gp-r!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa7d73bb2-e470-41b1-afe7-059ddbd78d55_1294x916.png)

Simon’s desk with Donald Knuth’s “The Art of Computer Programming,” and the training week he created

As Simon recalled:

> “I armed myself with a borrowed copy of “The Art of Computer Programming”, worked through the exercises, read up on common algorithms on Wikipedia, completed tasks on USACO, and memorized the critical parts of my Vim config for the competition computers. I managed to create quite an intense training weekend for myself.”

The regional finals were even more challenging than the Nationals, and participants struggled to write performant solutions to the problems. Despite putting in the effort, Simon was pretty sure he was out of the competition, [and recorded](https://sirupsen.com/my-journey-to-the-international-olympiad-in-informatics) his learnings:

> “\[From the programming competition\] I learned that you must avoid digging holes. Repeatedly, I found myself so fixated on getting a particular idea to work that I’d get absolutely nowhere. Sometimes, you have to bite the bullet, delete your program, find a new sheet of paper, and start from scratch. A good case of this is when you start working around a general solution to solve specific edge-cases. I learned that there is almost always a simple way to solve a problem without explicitly handling edge-cases. If there are two edge-cases, there’s almost certainly two more. The simple solution will handle edge-cases automatically – even those you might not have considered.”

But as it turned out, two months later he was told he had been selected to represent Denmark in the final phase of the competition. Simon continued to push himself out of the comfort zone of web development, using HTML, a bit of PHP, and getting into algorithmic programming.

**Another decision that would later bear fruit career-wise was starting a blog while at high school – though he didn’t know it at the time.** In 2010, Simon launched [his English-language blog](https://sirupsen.com/) but posted only one or two articles per year; mostly short ones describing problems he’d solved:

- [A five-line script](https://sirupsen.com/a-simple-imgur-bash-screenshot-utility) to take screenshots and upload them to Imgur, all from the command line (2010)

- [Setting up a Ruby HTTP server](https://sirupsen.com/setting-up-unicorn-with-nginx) on NGINX using the library [Unicorn](https://tomayko.com/blog/2009/unicorn-is-unix) (2010)

- [Why Simon favors SQL](https://sirupsen.com/stop-relying-on-your-orm-and-learn-sql) over ORMs: Object-Relational Mappings (ORMs) are frameworks to abstract SQL, these were very popular around that time (2011)

Two posts he published would go on to have an impact on his life. One was about [his IOI experience and learnings](https://sirupsen.com/my-journey-to-the-international-olympiad-in-informatics) . The other only came about after he suffered the disaster of fatally dropping his iPhone during his final year in high school.

With his smartphone dead, Simon was forced to switch to an old-school Nokia “dumb” phone. He wrote a short article about the experience titled [Why I’m glad my iPhone broke](https://sirupsen.com/iphone) , which [went viral on Hacker News](https://news.ycombinator.com/item?id=5109159) , making it to the front page of the site with many comments.

It seems that someone at Shopify, in Canada, read the article and others by him – including the summary of his impressive performance at the International Olympiad for Informatics – as a recruiter from Shopify flew Simon out to interview in Ottawa, Canada, where he was offered a software engineering position at the company.

**In his new job on the infrastructure team, Simon made notes about things he didn’t understand and read up on new concepts.** As he told me:

> “When I started at Shopify, I was insecure about having not studied computer science and my biggest exposure to programming had been the IOI. If nothing else, preparing for the IOI taught me that you can sit down, read a paper, and figure it out if you spend enough time on it. So, I did that repeatedly.
>
> In my first year at Shopify, every time I heard something I didn’t know, I noted it down on a piece of paper. Then, that evening, I would read up about it.
>
> For example, if someone at work mentioned TCP, I assumed that surely they would know *exactly* what’s in the three-way handshake and how TLS is layered on top. And I also assumed they’d looked at Wireshark and all of that. I don’t think that’s true, but that’s what I thought at the time! So, I dug deep into everything I encountered.”

Working in infrastructure meant solving interesting engineering problems at a time when Shopify was growing 120-140% in load year-on-year; Simon was exposed to problem domains like:

- **Ruby on Rails and databases:** Shopify was already one of the world’s largest Ruby on Rails monolith applications, and working on infrastructure meant being close to the database layer

- **Sharding and cutting over:** as Simon’s manager used to say, “you cannot cache writes”, so Shopify had to move from running on a single group of machines to a shard (partitioned) setup of machines. The team did the cut-over (moving to the shard) just a week before Black Friday, the busiest time of the year.

- **Multi-data centers:** expanding database footprint from one data center (DC) to multiple DCs

- **Splitting up key services:** Shopify had a 128GB machine (massive for the time) running Redis as a key-value store. Nobody dared touch it until one day the service went down. Then, the team split responsibilities up into separate services.

**Simon built a framework to simulate networking conditions called [toxiproxy](https://github.com/shopify/toxiproxy) .** The idea came to him while attempting to write a more thorough, systems-level test to see how Shopify’s application held up during partial outages. He created a matrix of Shopify’s services, and wanted to write a complete test suite for this matrix, to see if the system was resilient enough to handle some parts of the system being down. For example:

[![](https://substackcdn.com/image/fetch/$s_!9au0!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9fdba208-a817-42fe-b4a0-f6ba813d8541_512x135.png)](https://substackcdn.com/image/fetch/$s_!9au0!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9fdba208-a817-42fe-b4a0-f6ba813d8541_512x135.png)

*An example of a test case Simon wanted to run*

As Simon [wrote](https://shopify.engineering/building-and-testing-resilient-ruby-on-rails-applications) about this problem at the time:

> “Having tests for the matrix was a must; otherwise we couldn’t guarantee the state of the matrix wouldn’t degrade over time. Since the tools mentioned previously require root access, we investigated proxies to simulate latency and downtime at the TCP level, but didn’t find one that suited our needs. We needed an online API to edit proxies and to support deterministic latencies, which made it suitable for integration testing.”

This was the inspiration that led to toxiproxy. As Simon told me on stage:

> “Toxiproxy is a proxy that sits in between the application and the databases. With the proxy in place, you could do things like make an API call to the proxy, instructing it to simulate taking the database down, or making it slow.
>
> Over time, we added a bunch of other ways to inject failures. With this proxy, we did not have to mock low-level drivers, but we could test failure handling really well.”

Toxiproxy was [open sourced](https://github.com/shopify/toxiproxy) in 2014, and apparently still runs in Shopify’s CI system 12 years later, to Simon’s knowledge!

**The biggest benefit of a long tenure at a company was learning to write software that ages well.** Simon told me that this was the lesson that made it so worthwhile, and how often the simple solution someone put together in a week or two outlasted the big, multi-team RFC-driven solutions. That was a lesson he still uses today.

While at Shopify, Simon became interested in figuring out the theoretical limits of certain computer operations. For example, how much bandwidth does DRAM have for memory transfer? How long does a round-trip operation to AWS S3 take, and how much does it cost? What does a gigabyte of memory cost?

He sought the actual numbers, wrote a script to collect them, and created a table with the data:

**Simon started to memorize key numbers with flash cards.** He wanted to be able to instantly recall all important numbers. As he told me:

> “Napkin math was essentially just this table that I maintain on GitHub. There’s probably like 50 of these numbers and then a script that generates them all.
>
> For example, what does a gigabyte of memory cost? $2. What does a gigabyte of S3 cost? Two cents. What does a gigabyte of this cost? 10 cents. What does it cost on spot? What does it cost on a three-year commit? I had a massive table, then I created flashcards for almost every single cell so I know all these numbers.
>
> This was a project I started taking on at Shopify because I would review projects and these numbers would be helpful.”

**Napkin math enabled him to challenge design decisions based on benchmarks that had issues.** As he told me:

> “When reviewing a project, a product team would tell me that they chose Database A over Database B because they benchmarked both, and Database A was better. I hate benchmarks because making design decisions based on benchmarks is not a satisfying answer to me.
>
> For example, you’re saying that with Database A, as per the benchmark, it takes 10 seconds to do this one thing. But it should take 10 milliseconds if you do the napkin math. Say, it’s a search query. Okay, you’re searching for three terms then. Each term has this many documents that match it. That’s this many megabytes. We intersect this many lists. You have DRAM bandwidth on multiple cores, at 100 GB/sec. So if you do the math, it should be 10 milliseconds.
>
> So, now you’re telling me that the benchmark for the same thing takes 10 seconds, which means one of us is wrong! Either there’s a gap in my understanding – which is possible! – or the benchmark measures the wrong thing.
>
> Often, it would be things like the person doing the benchmark and not realizing that the query would be a distributed one, running across a hundred different nodes. And in this case, of course the p99 is going to be very high!”

**Simon went even deeper into “napkin math” after leaving Shopify.** He [investigated](https://sirupsen.com/napkin/problem-10-mysql-transactions-per-second) whether MySQL’s maximum transactions per second is equivalent to fsyncs per second (the number of file writes per second a system can handle). He discovered that MySQL can handle more writes than the operating system could sync to the file, which was a surprise:

> “It takes ~3 seconds to perform 16,000 insertions, or ~5,300 insertions per second. This is 5x more than the 1,000 fsyncs per second our napkin math told us would be the theoretical maximum transactional throughput!
>
> Typically, with napkin math we aim for being within an order of magnitude, which we are. But when I do napkin math, it usually establishes a lower bound for the system, i.e., from first-principles, how fast could this system perform in ideal circumstances?
>
> **Rarely is the system 5x faster than the napkin math says.** When we identify a significant-looking gap between the real-life performance and the expected performance, I call it the “first-principle gap.” This is where curiosity sets in. It typically means there’s (1) an opportunity to improve the system, or (2) a flaw in our model of the system. In this case, only (2) makes sense, because the system is faster than we predicted.”

Simon [started to investigate](https://sirupsen.com/napkin/problem-10-mysql-transactions-per-second) , and learned that MySQL does grouping of transactions (not doing an fsync for every write), and it also does smart merging of multiple fsync operations that would be processed in parallel, effectively doing a “group commit” to further improve performance. This is a learning he’d later use: grouping writes together on top of S3, to reduce latency and improve cost.

Personally, I find it amazing how much you can learn with some measurements and by asking questions about how a system achieves results that go against what the “napkin math” suggested should be its limit!

“Napkin math” also played a part in the creation of turbopuffer. Simon told me that three things had to combine for the project to come to life.

**1\. Search was a difficult project at Shopify.** Building search was one of Simon’s final projects at the company, and he did not have a good time with it. He used a popular database vendor but struggled to have it perform napkin math. The query plans were not exposed easily by the database, so he could not figure out what was missing and why it wasn’t performing the napkin math without reading massive amounts of source code. Also, the search infra was difficult to operate.

**2\. Napkin math became a surprisingly efficient reasoning tool.** As a toolkit, napkin math gave Simon a way to reason about what could be achievable with a machine, if utilized perfectly.

**3\. His friends’ startups needed fast search badly because of AI.** In early 2023, Simon helped a few friends with infra at their startups, many of whom had the same problem: LLM context windows were very small (4-8 KB), and they needed to fill them with the right parts of documents, but this demanded very fast search. One startup budgeted that their new search vendor would cost $30K/month, while the existing infra bill was only $5K/month! For a bootstrapped Canadian company, the cost was too high, so they didn’t ship the AI feature.

Simon could not stop thinking about why search was so expensive, and why it didn’t line up with what the “napkin math” predicted the cost should be. Eventually, he laid out a basic architecture to make it fast and cheap:

1. Store the data to search in AWS S3

2. Do some clustering and organize the files

3. Get latency down. S3 is cheap and reliable, but has high latency

Driven by curiosity about how that could work, he set out to build it. As he told me:

> “I just became fully obsessed that summer \[of 2023\] with building it. The first version was the simplest possible thing. I’m a very pragmatic person, so I didn’t get buried in detail. I barely read the literature on log-structured merge ( [LSM](https://en.wikipedia.org/wiki/Log-structured_merge-tree) ).
>
> The simplest way to do this is to run a clustering algorithm on the vectors. You get the clusters, and then put the clusters in files. The files are called ‘cluster\_01’, ‘cluster\_02’, ‘cluster\_03.’ Then you have a file called centroids of the clusters. Then, you search by downloading centroids, looking into them, then downloading the closest clusters.
>
> There were a few optimizations around merging clusters that were adjacent in files, just to control costs and boost performance. But this was the core of it.”

What about performance? Simon:

> “For the first version, I didn’t even implement a dedicated caching layer. I just put the reverse proxy (NGINX) in front of S3, and that was it! The performance improvement came from caching all of the S3 objects. It was the simplest possible layer at the NGINX level.”

I first heard of turbopuffer last year, when I interviewed Cursor cofounder Sualeh Asif about how they built the AI coding harness. From our previously published [Cursor deepdive](https://newsletter.pragmaticengineer.com/p/cursor) :

> “The ongoing need to re-shard as usage grew became error-prone and frustrating. Sualeh told me the biggest lesson the team learned was to avoid sharding where possible in future. So, they looked for a vector database that could support multi-tenancy without requiring manual sharding.
>
> Turbopuffer was a startup that promised this, so Cursor tried it and migrated a good part of their vector search use cases over.”

**So, how did a billion-dollar startup come to bet on a tiny, unproven infra product with zero customers?** There were a few factors:

First, Cursor was not *yet* a billion-dollar company in the fall of 2023, but a relatively fresh startup. The company raised $8M in seed funding in the same month. In 2024, the company was valued at $400M, and then its valuation surged to $29.3B. It was sold to SpaceX for $60B this year.

Secondly, it involved a tweet Simon posted after having had enough of building all summer, and wanting to test the waters. As he told me:

> “I was so sick of working on \[turbopuffer\]. I’d been working on this all summer and didn’t know if anyone cared. I only wanted to work on it if anyone cared. So, I decided to put it on Twitter.
>
> At that point, I had a single [TMUX](https://groups.google.com/g/gce-discussion/c/eNl7L3_Y6Mg) instance running on an 8-core node somewhere in GCP. I was thinking: “if someone goes to prod, I’ll set it up properly on multiple nodes.
>
> **But first, let’s see if anyone cares.**
>
> Anyone who’s worked in the internals of databases would’ve had too much pride to ship anything like that; I was just releasing it like a SaaS project. Why can’t you work on a database like it’s SaaS?”

So, he put out this confident-sounding tweet:

Simon had the confidence to launch his product because he knew it was rock-solid and scalable:

> “I knew turbopuffer was reliable. It upheld its invariants. For example, you shut down all the VMs and no data is lost. All the writes are committed directly to blob storage.”

**Cursor reached out.** The company was an eight-person team at the time, fresh from seed funding and growing fast, but with a search problem that they were looking to partner with a startup on. Simon recounts:

> “Knowing Cursor’s founders now, I’m sure they must have sat at the dinner table one day and were like: ‘the unit economics of what we have right now, where all the vectors are in DRAM, are not working.’
>
> They probably were asking why someone had not built a solution where you can put all the vectors from the codebase into S3 (to make it cheap), and move the part of the codebase used in memory (to make it fast). Then, everything sits in blob stores and you just hot load it all into cache. When you open the codebase, after a few seconds it’s all in RAM and the queries are as fast as everything else.
>
> Aman (one of the cofounders) was already talking about using S3 as a key-value cache, which at the time, barely anyone was thinking of for unit economics.”

**Simon knew nothing about B2B sales at that point, and was not trying to “sell” anything to Cursor.** He exchanged a few emails with the Cursor team about their use case of searching many small local codebases. He wanted to help Cursor with the unit economics, while also proving that turbopuffer works. So, he hopped on a flight from Canada to San Francisco, arrived at Cursor’s office and got down to debugging their database provider:

> “When I showed up at Cursor’s office, they were having some Postgres problems. I asked, ‘do you guys have [pganalyze](https://pganalyze.com/) ?’ And they didn’t, so I was like, ‘Okay, let’s get that going. Let’s look at it. “
>
> And the problem was the same thing as it always is with Postgres: [autovacuum](https://www.postgresql.org/docs/current/runtime-config-vacuum.html) hadn’t run enough. And so they had all of these going to [heap](https://medium.com/@Amir_M4A/how-postgresql-stores-data-heaps-blocks-and-ctids-explained-f66d052cf87d) , when they should be doing index scans, etc. So, we were talking about all of that.
>
> **I was just helping them: my “database genes” just kicked in.** I think this built enough trust with them to believe that if I know enough to help them with their database, maybe I also know how to build one.”

It was around when turbopuffer’s other cofounder, [Justine Li](https://justine.li/) , joined that the work with Cursor kicked off. Simon describes Justine as the best engineer he ever worked with at Shopify, and together, they made more performance optimizations. Cursor migrated their local code-base search over to this brand new product.

**Simon promised to reduce Cursor’s bill by 95%, and delivered – thanks to napkin math!** Cursor was spending around $80K/month on indexing and search; with turbopuffer in place, this dropped to $4K/month. Simon was confident about making this prediction thanks to napkin math! He did the calculations based on the fundamentals; only counting resource usage that Cursor was using, and not taking a margin for turbopuffer’s operating cost at the time.

After securing Cursor as the first customer, Simon was not convinced that turbopuffer should raise VC money. He explained his thinking:

> “**I understood that if you take venture capital, no matter how many smiles there are in the room, everyone’s expecting to earn a big return** on some timeline that makes sense to everyone involved. And ‘everyone involved’ are pension funds in Canada.
>
> But at the time, I did not know if turbopuffer could be a billion dollar company. It felt like a very niche kind of search engine. And that was completely fine with me!
>
> So, I just looked at what we invoiced and I looked at my GCP bill. And I was making this equation that:
>
> Customer
>
> *Invoices >= GCP bill \[the turbopuffer costs\]*Justine and I were going to optimize the system until these numbers were roughly equal. And if we could get some other workloads over to turbopuffer, we could then start paying ourselves.
>
> But either way, I didn’t know if I could even go and raise a bunch of money. I didn’t have any relationships. I was an outsider who grew up in Aarhus, Denmark, and moved to Canada.”

A few months later, turbopuffer ended up raising a total of $700K, just to be able to hire two engineers until the end of the year on *real* salaries. Simon found that most VCs did not take them seriously for wanting to raise *too little money,* and assumed this signified a lack of ambition! Two years later, with less than $1M in initial funding raised, turbopuffer crossed $100M in annual run rate. So, there evidently was ambition!

Since our conversation, Simon has reflected on the topic of VC capital and built a mental model of six reasons that justify raising external funding:

**Reason #1: Research and development.** This is what turbopuffer raised for: they needed the money to hire two more engineers until the end of the year, in order to build out more of the platform. Then, they would either get more customers and generate enough revenue to not need funding, or shut down the project if it failed to gain traction.

**Reason #2: Growth.** When you’ve built something and want to tell the world about it, often, you have to spend money on doing that.

**Reason #3: Massaging founders’ egos.** Simon:

> “This is a very, very popular reason! You see big numbers, you get lots of press. But I think it’s a really dangerous reason to raise money.
>
> I wish this reason was talked about more, because you are diluting all of your employees when you do it. And for some people, it can become a status game. It’s not what we are about: we’re here to build a big business together.”

**Reason #4: Employee rewards.** A startup is a long journey, and everyone wants to work with the best people. But there are necessarily few of these folks, so you want to reward the standout ones by raising money so that employees can sell their shares. This was why, in December 2025, turbopuffer raised a round where they let employees sell some of their equity: it meant they would not have to wait for a future financial event like an IPO.

**Reason #5: Strategic partnerships.** It might be the case that raising from a VC whose network of connections is key to your business succeeding, or that taking funding from a strategic investor gives you access to their platform or services.

**Reason #6: Mergers & acquisitions (M&A).** Purchase another company with the funding in order to expand the business.

Simon encourages founders to be honest about why they raise capital funding – and to watch out if the reason is egocentric!

Watch [the full interview here](https://www.youtube.com/watch?v=h8TBLKyo7Rs) .

**It’s inspiring to see how far it’s possible to get with “napkin math” and by understanding the bottom layers.** It didn’t sit right with Simon to make decisions about vendors based on hastily written benchmarks that might measure the wrong thing. By understanding the constraints of data transfer latency and storage cost, he found a way to estimate the theoretical lower bounds of the system.

This helped lead to more informed design decisions at Shopify, and showed him there was an opportunity to build a faster, cheaper search product than the status quo.

**Getting “lucky” in business requires a lot of skill, and in-person greatly helps with first impressions.** Turbopuffer’s first customer being Cursor sounds almost too good to be true. But the account from [Cursor cofounder Sualeh Asif](https://newsletter.pragmaticengineer.com/i/165641889/sharding-headaches) – and now from Simon – reveals the ingredients in more detail:

- Spotting a new business need: AI-native startups like Cursor saw their search bills explode, but needed search functionality to offer usable AI products

- Offering a product with a magnitude of lower pricing: what people made suddenly pay attention to turbopuffer was the promise of not “just 20-50%” cost savings, but a seemingly radical, 90%+ reduction in costs. When you’re late to enter a market (like search), you need major differentiation, then deliver on it! This is also what Cursor’s attention.

- Building trust before a sale: Simon helped the Cursor team fix their existing database before discussing using his product

- In-person impressions: if Simon had not flown to San Francisco to meet the Cursor team in person, would they have taken a bet on turbopuffer?

- Launching at the earliest opportunity: none of this would have happened if Simon did not announce the first version of the product when he knew it could work, but was still in a pretty unpolished state!

**It’s always helpful to be aware of the dynamic of venture funding – some of which are rarely mentioned.** Simon knew investors expect returns and growth on timelines which they set. Raising money is helpful in many situations, but problems arise from egotistical reasoning, and Simon believes too many founders prioritize them – knowingly or not.

No posts
