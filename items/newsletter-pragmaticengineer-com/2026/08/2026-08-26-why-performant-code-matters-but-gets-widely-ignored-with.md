---
title: Why performant code matters (but gets widely ignored), with Casey Muratori
link: https://newsletter.pragmaticengineer.com/p/why-performant-code-matters-but-gets
source: newsletter-pragmaticengineer-com
published: 2026-08-26T15:59:59Z
updated: 2026-08-26T15:59:59Z
first_seen: 2026-09-19T21:30:23.395188495Z
authors:
- Gergely Orosz
summary: Casey Muratori explains why software performance matters, how developers can write faster code, and why he challenges conventional engineering practices.
content: extracted
html: 2026-08-26-why-performant-code-matters-but-gets-widely-ignored-with.html
preview:
  file: 2026-08-26-why-performant-code-matters-but-gets-widely-ignored-with.preview-552c149a7ff4.webp
  width: 256
  height: 144
  color: '#d7cac9'
images:
- source: https://substackcdn.com/image/youtube/w_728,c_limit/8xBJPa_480Q
  original:
    file: 2026-08-26-why-performant-code-matters-but-gets-widely-ignored-with.image-51a4ca42cb10.jpg
    width: 728
    height: 410
  color: '#f0f0f0'
- source: https://substackcdn.com/image/fetch/$s_!T84b!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F31fb1727-eeae-4f63-8c95-81d589e3c3f3_1600x140.png
  original:
    file: 2026-08-26-why-performant-code-matters-but-gets-widely-ignored-with.image-2d7b09d90366.jpg
    width: 1456
    height: 127
  color: '#fdfdfd'
- source: https://substackcdn.com/image/fetch/$s_!zFxq!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9c4b0a6c-f810-445a-bdc5-e298db97f7a0_1420x1180.png
  original:
    file: 2026-08-26-why-performant-code-matters-but-gets-widely-ignored-with.image-9be880c61dcc.jpg
    width: 1420
    height: 1180
  color: '#f3f2f0'
extra:
  audio_url: https://api.substack.com/feed/podcast/212722114/09d691a0ab1c3abac2fb91ebdd192de3.mp3
---

**Listen and watch now on [YouTube](https://youtu.be/8xBJPa_480Q) , [Apple](https://podcasts.apple.com/us/podcast/the-pragmatic-engineer/id1769051199) , and [Spotify](https://open.spotify.com/show/2Bho9xCbOQMWMJ7UKmqCzD) .** See the episode transcript at the top of this page, and timestamps for the episode at the bottom.

[![](https://substackcdn.com/image/fetch/$s_!T84b!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F31fb1727-eeae-4f63-8c95-81d589e3c3f3_1600x140.png)](https://substackcdn.com/image/fetch/$s_!T84b!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F31fb1727-eeae-4f63-8c95-81d589e3c3f3_1600x140.png)

• **[Antithesis](https://antithesis.com/pragmatic)** – turbocharge testing of your systems by running your whole system under aggressive fault injection. There’s good reason teams like Jane Street, Fly.io, and the etcd community rely on Antithesis. [Learn more.](https://antithesis.com/pragmatic)

• **[Sentry](https://sentry.io/pragmatic)** – application monitoring software built by developers, for developers. [Sentry’s Seer AI](https://sentry.io/product/seer/?utm_source=pragmatic&utm_medium=paid-community&utm_campaign=ai-fy27q1-seerlaunch&utm_content=recording-coreread-freetrial&code=pragmatic) agent is one of their new, neat tools, which I’ve used as a way to quickly fix errors on my backend. [Check out Sentry.](https://sentry.io/pragmatic)

• **[turbopuffer](https://turbopuffer.com/pragmatic)** – A vector and full-text search engine built on object storage. It’s fast, cheap, and extremely scalable. I met their team in San Francisco, and am a fan of their “hardcore and whimsical” engineering culture, and how pragmatic their engineering philosophy is. [Check them out.](https://turbopuffer.com/pragmatic)

There can be few people around who care about software performance more than today’s pod guest, [Casey Muratori](https://x.com/cmuratori) . He’s a programmer and videogame developer, founder of [Molly Rocket](https://mollyrocket.com) , and creator of [Handmade Hero](https://hero.handmade.network/) – a long-running series about building a game from scratch. He also evangelizes about [performance](https://www.computerenhance.com/p/welcome-to-the-performance-aware) on his Substack, [Computer, Enhance](https://www.computerenhance.com/) .

We got to know each other about three years ago, first via messages, including this one from Casey:

> “Why does the industry zeitgeist place so little emphasis on software performance when there seems to be overwhelming evidence that performance is critical to their bottom line?
>
> Like you, I run a Substack for professional programmers, but I focus exclusively on software performance. Although we are quite large by Substack standards, so a certain subset of programmers must believe performance is important, I nonetheless hear lots of dismissive excuses when I post on social media. This happens so frequently, I devoted an entire article to cataloging the extensive pro-performance evidence we already have from the world’s leading software companies: [Performance Excuses Debunked](https://www.computerenhance.com/p/performance-excuses-debunked) .
>
> **Strangely, nobody has a rebuttal to why performance is important**. When I point people to this, they actually tend to agree. But the prevailing attitude nonetheless stays the same.”

I’m delighted we finally have Casey on the podcast because it’s overdue! In this episode, we discuss why software performance matters, why it’s overlooked, and how developers can get better at writing performant code. We explore why performance should be considered during design, the value of learning to read assembly & understanding how CPUs work, Casey’s critique of ‘clean code’, and why he believes testing shouldn’t drive software design.

We touch on how videogame development has changed, and influential game engines. Casey also tells us why he prefers to write code by hand, not with AI, and more.

**1\. DirectX might not exist without an “unauthorized” internal Microsoft project.** DirectX is a very popular Microsoft library that standardized rendering on top of GPUs, used mostly for games. Casey tells how Chris Hecker built a library for fast on-screen rendering at Microsoft called WinG, which was never authorized; it was a total “ [Skunk Works](https://en.wikipedia.org/wiki/Skunk_Works) ” project. DirectX’s roots go back to WinG, which its three founders were testers on.

**2\. Is performance starting to matter to businesses?** Enterprise software buyers care mainly about cost, compliance, and capabilities – but not performance. Even so, there are some products gaining major popularity and market share due to their performance, such as [File Pilot](https://filepilot.tech/) (next-gen file explorer) and the [Blick video editor](https://blickeditor.com/) . Is the tide turning?

**3\. Profiler-driven performance optimization is the wrong way to optimize.** The standard way of optimizing is to profile the application, tweak hotspots, then check if the stats have improved. But this only finds a local minimum; Casey says every engineer he’s worked with who was a great “optimizer” began by establishing what the hardware could theoretically do, and then did not stop until they’d closed the gap to that performance level.

**4\. If you care about performance, learn to read assembly (no need to write it).** There are about 20-30 instructions you need to learn to be able to read basic assembly. For example, here’s a program that calculates the value of 5 + 3 - 1 (which is 7), then prints it out:

[![](https://substackcdn.com/image/fetch/$s_!zFxq!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9c4b0a6c-f810-445a-bdc5-e298db97f7a0_1420x1180.png)](https://substackcdn.com/image/fetch/$s_!zFxq!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9c4b0a6c-f810-445a-bdc5-e298db97f7a0_1420x1180.png)

*An assembly program calculates 5+3-1 (the first 3 lines after \_start), then prints the result to stdout*

**5\. Take a grain of salt with conventional wisdom that premature optimization is the “root of all evil”.** Many devs use it as an excuse to delay performance optimization, but Casey says that not optimizing in time could mean that only performance hotspots can be fixed later, and not the architectural issues that create poor performance. Architect your system to be performant, or you’ll have trouble solving problems without a rewrite!

**6\. Only three things are needed to understand how CPUs work.** Casey believes that knowing them means you’ll be able to tell from any CPU announcement roughly how well it performs. Those three pillars of understanding:

- How data moves in and out: load/store units and L1–L3 caches

- How instructions flow through the pipes: branch prediction, i-cache

- Execution unit scheduling: raw throughput per operation type

**7\. Why are game studios so secretive?** Before licensable videogame engines existed, the game engine was a studio’s “core” intellectual property (IP), and every studio built rendering, pathfinding, and other tools from scratch. This is how Blizzard rolled Warcraft 1’s engine into Warcraft 2. Any competitor making a rival game had to start from scratch, which was a reason for game studios to closely guard the secrets of how their own game engines worked.

**8\. The games industry already had its “AI moment” – and it wasn’t pretty.** When game engines became licensable, pretty much any developer could build and publish a game with the likes of Unity and Unreal, on a platform like Steam.

Initially, this change empowered new devs to build interesting games. But soon enough, the market was flooded with tens of thousands of releases per year, which destroyed organic discovery. Without a marketing strategy, the chances of a game gaining traction today are basically zero, says Casey.

**9\. Old games don’t look dated anymore, and that’s a problem.** For decades, graphics were a vital barometer for showing how videogames improved over time; a new release in 1995 was guaranteed to be visually superior to one from 1990. But a new game in 2026 likely doesn’t look much different from one that’s nine years old, and new releases face ongoing competition from older games.

**10\. Casey’s problem with test-driven development is the “test” bit.** Casey believes tests should be a cost/benefit decision, and not put in place by default. For some projects, doing tests upfront – or doing any tests at all, in some cases – is simply a bad choice.

**11\. One trait of almost every great engineer: refusing to accept programming wisdom untested in the real world.** As Casey puts it:

> “I find there’s a lot of received programming wisdom that’s just nonsense. Clearly, no one’s ever tested it. In order for something to be received wisdom, you should have to at least demonstrate concrete upsides, but often this cannot be done. I would say focusing on what actually works in practice is a huge plus.”

**12\. No AI in Casey’s upcoming game.** He acknowledges that many developers will disagree, but insists there’s nothing wrong with being outside of mainstream tastes, just like some people chose handmade furniture over the flatpack kind. His reasoning for omitting AI is straightforward:

> “I want to program things in a game because I want to program them. If I only wanted output, I’d just get the Unreal Engine.”

• [Pushing software engineering limits with “napkin math”](https://newsletter.pragmaticengineer.com/p/pushing-software-engineering-limits)withSimon Eskildsen

• [How Games Typically Get Built](https://newsletter.pragmaticengineer.com/p/how-games-typically-get-built) : prototyping, game engines, and a different type of QA

• [Game Development Basics](https://newsletter.pragmaticengineer.com/p/game-development-basics) : deepdive on how game studios differ from standard software teams

• [Inside Linear’s Engineering Culture](https://newsletter.pragmaticengineer.com/p/linear) : building a performant product with a tiny team

• [Building a best-selling game with a tiny team](https://newsletter.pragmaticengineer.com/p/thronefall) – with Jonas Tyroller. A two-person team built a game that sold 1M+ copies

**More on premature optimization:** read or watch Casey’s extended take on “premature optimization is the root of all evil”:

[00:00](https://www.youtube.com/watch?v=8xBJPa_480Q) Intro

[05:17](https://www.youtube.com/watch?v=8xBJPa_480Q&t=317s) Games at Microsoft

[12:52](https://www.youtube.com/watch?v=8xBJPa_480Q&t=772s) Building games

[16:00](https://www.youtube.com/watch?v=8xBJPa_480Q&t=960s) Why performance matters

[27:12](https://www.youtube.com/watch?v=8xBJPa_480Q&t=1632s) Why you should learn to read assembly

[30:36](https://www.youtube.com/watch?v=8xBJPa_480Q&t=1836s) Designing for optimization

[42:51](https://www.youtube.com/watch?v=8xBJPa_480Q&t=2571s) How to get better at writing performant software

[49:04](https://www.youtube.com/watch?v=8xBJPa_480Q&t=2944s) Understanding how the CPU works

[55:53](https://www.youtube.com/watch?v=8xBJPa_480Q&t=3353s) Building games then and now

[1:05:56](https://www.youtube.com/watch?v=8xBJPa_480Q&t=3956s) How game engines changed building games

[1:10:48](https://www.youtube.com/watch?v=8xBJPa_480Q&t=4248s) Why new games compete with old games

[1:13:25](https://www.youtube.com/watch?v=8xBJPa_480Q&t=4405s) GTA 6: why is it taking so long?

[1:16:59](https://www.youtube.com/watch?v=8xBJPa_480Q&t=4619s) Casey’s critique of clean code

[1:21:48](https://www.youtube.com/watch?v=8xBJPa_480Q&t=4908s) Casey’s take on TDD

[1:24:30](https://www.youtube.com/watch?v=8xBJPa_480Q&t=5070s) What is good code?

[1:27:32](https://www.youtube.com/watch?v=8xBJPa_480Q&t=5252s) What makes a good software engineer?

[1:33:56](https://www.youtube.com/watch?v=8xBJPa_480Q&t=5636s) Why Casey doesn’t code with AI

[1:39:01](https://www.youtube.com/watch?v=8xBJPa_480Q&t=5941s) AI’s impact on the game industry

[1:44:43](https://www.youtube.com/watch?v=8xBJPa_480Q&t=6283s) AI and burnout

[1:50:21](https://www.youtube.com/watch?v=8xBJPa_480Q&t=6621s) Why you should read papers

**Where to find Casey Muratori:**

• X: [https://x.com/cmuratori](https://x.com/cmuratori)

• Website:

• Substack: [https://substack.com/@cmuratori](https://substack.com/@cmuratori)

**Mentions during the episode:**

• Digital Equipment Corporation: [https://en.wikipedia.org/wiki/Digital\_Equipment\_Corporation](https://en.wikipedia.org/wiki/Digital_Equipment_Corporation)

• VAX 9000: [https://en.wikipedia.org/wiki/VAX\_9000](https://en.wikipedia.org/wiki/VAX_9000)

• Intel: [https://www.intel.com](https://www.intel.com)

• Chris Hecker’s website: [https://www.chrishecker.com/Homepage](https://www.chrishecker.com/Homepage)

• Doom: [https://en.wikipedia.org/wiki/Doom\_(franchise)](https://en.wikipedia.org/wiki/Doom_\(franchise\))

• Wolfenstein 3D: [https://en.wikipedia.org/wiki/Wolfenstein\_3D](https://en.wikipedia.org/wiki/Wolfenstein_3D)

• WinG: [https://en.wikipedia.org/wiki/WinG](https://en.wikipedia.org/wiki/WinG)

• Ron Gilbert: [https://en.wikipedia.org/wiki/Ron\_Gilbert](https://en.wikipedia.org/wiki/Ron_Gilbert)

• Humongous Entertainment: [https://en.wikipedia.org/wiki/Humongous\_Entertainment](https://en.wikipedia.org/wiki/Humongous_Entertainment)

• The Secret of Monkey Island: [https://en.wikipedia.org/wiki/The\_Secret\_of\_Monkey\_Island](https://en.wikipedia.org/wiki/The_Secret_of_Monkey_Island)

• DirectX: [https://en.wikipedia.org/wiki/DirectX](https://en.wikipedia.org/wiki/DirectX)

• Todd Laney on Tumblr: [https://toddla.tumblr.com](https://toddla.tumblr.com)

• Craig Eisler on LinkedIn: [linkedin.com/in/craigeisler](http://linkedin.com/in/craigeisler)

• Eric Engstrom: [https://en.wikipedia.org/wiki/Eric\_Engstrom](https://en.wikipedia.org/wiki/Eric_Engstrom)

• Dungeon Siege: [https://en.wikipedia.org/wiki/Dungeon\_Siege](https://en.wikipedia.org/wiki/Dungeon_Siege)

• RAD Game Tools: https://www.radgametools.com

• Alex St. John: [https://en.wikipedia.org/wiki/Alex\_St.\_John](https://en.wikipedia.org/wiki/Alex_St._John)

• Molly Rocket: [https://mollyrocket.com](https://mollyrocket.com)

• File Pilot: [https://filepilot.tech](https://filepilot.tech)

• Bun: [https://bun.com](https://bun.com)

• npm: [https://www.npmjs.com](https://www.npmjs.com)

• Napkin math: [https://github.com/sirupsen/napkin-math](https://github.com/sirupsen/napkin-math)

• Fortnite: [https://www.fortnite.com](https://www.fortnite.com)

• Minecraft: [https://www.minecraft.net](https://www.minecraft.net)

• Roblox: [https://www.roblox.com](https://www.roblox.com)

• GTA online: [https://www.rockstargames.com/gta-online](https://www.rockstargames.com/gta-online)

• Unreal Engine: [https://www.unrealengine.com](https://www.unrealengine.com)

• Ken Silverman’s website: [https://advsys.net/ken](https://advsys.net/ken)

• id software: [https://www.idsoftware.com](https://www.idsoftware.com)

• Bullfrog Productions: [https://en.wikipedia.org/wiki/Bullfrog\_Productions](https://en.wikipedia.org/wiki/Bullfrog_Productions)

• Thief: The Dark Project: [https://en.wikipedia.org/wiki/Thief:\_The\_Dark\_Project](https://en.wikipedia.org/wiki/Thief:_The_Dark_Project)

• Death Rally: [https://en.wikipedia.org/wiki/Death\_Rally](https://en.wikipedia.org/wiki/Death_Rally)

• Grand Theft Auto V: [https://www.rockstargames.com/gta-v](https://www.rockstargames.com/gta-v)

• “Clean” Code, Horrible Performance:

• TDD, AI agents and coding with Kent Beck: [https://newsletter.pragmaticengineer.com/p/tdd-ai-agents-and-coding-with-kent](https://newsletter.pragmaticengineer.com/p/tdd-ai-agents-and-coding-with-kent)

• Python, Go, Rust, TypeScript and AI with Armin Ronacher: [https://newsletter.pragmaticengineer.com/p/python-go-rust-typescript-and-ai](https://newsletter.pragmaticengineer.com/p/python-go-rust-typescript-and-ai)

—

Production and marketing by [Pen Name](https://penname.co/) .
