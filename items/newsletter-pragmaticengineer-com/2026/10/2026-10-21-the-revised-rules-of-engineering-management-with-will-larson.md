---
title: The revised rules of engineering management with Will Larson
link: https://newsletter.pragmaticengineer.com/p/the-revised-rules-of-engineering
source: newsletter-pragmaticengineer-com
published: 2026-10-21T16:58:21Z
updated: 2026-10-21T16:58:21Z
first_seen: 2026-10-21T19:14:42.600331207Z
authors:
- Gergely Orosz
summary: Will Larson, CTO of Imprint, explores how AI and shifting industry expectations are changing engineering management, career ladders, and the path from engineer to CTO.
content: extracted
html: 2026-10-21-the-revised-rules-of-engineering-management-with-will-larson.html
remote_preview:
  url: https://substackcdn.com/image/youtube/w_728,c_limit/Prl858_AovM
extra:
  audio_type: audio/mpeg
  audio_url: https://api.substack.com/feed/podcast/221358515/21be084e9eda508a6f0476b15e6df032.mp3
  duration_seconds: 6934
---

*Before we start: I’m very excited to announce an [The Pragmatic Engineer Unplugged](https://luma.com/pragmatic-engineer-unplugged) live event: with Diogo Almedia (the creator of Jev) and Bryan Cantrill (cofounder and CTO, Oxide). 4 November (Wednesday), San Francisco, the SFJAZZ Center. It’s free to attend, but spaces are limited and [registration is required](https://luma.com/pragmatic-engineer-unplugged).*

[![](https://substackcdn.com/image/fetch/$s_!aK1D!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fee01f02b-a319-48e2-9687-2a6dc0dc9fe2_2400x2400.png)](https://substackcdn.com/image/fetch/$s_!aK1D!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fee01f02b-a319-48e2-9687-2a6dc0dc9fe2_2400x2400.png)

If you’re in SF or in the area, perhaps see you there? [RSVP here](https://luma.com/pragmatic-engineer-unplugged)

**Listen and watch now on [YouTube](https://youtu.be/Prl858_AovM), [Apple](https://podcasts.apple.com/us/podcast/the-pragmatic-engineer/id1769051199), and [Spotify](https://open.spotify.com/show/2Bho9xCbOQMWMJ7UKmqCzD).** See the episode transcript at the top of this page, and timestamps for the episode at the bottom.

![](https://substackcdn.com/image/fetch/$s_!AlJu!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8e3d3c6f-6a37-411e-aa1a-dfcefc979af3_1600x140.png)

• **[turbopuffer](http://turbopuffer.com/pragmatic)** – vector and full-text search built on object storage: fast, 10x cheaper, and extremely scalable. Under the hood, the team are rewriting the tpuf storage engine. Follow along until it hits production at [turbopuffer.com/v3](http://turbopuffer.com/v3)

**• What if you only needed a single test to harden your system?** [Get a free introduction to property-based testing](https://pages.antithesis.com/webinar-property-based-testing-pragmatic?utm_medium=Email&utm_campaign=pragmatic_2026&utm_source=pragmatic&utm_content=pbt_20261001): through a live, online webinar hosted by Hillel Wayne (a recent [podcast guest](https://newsletter.pragmaticengineer.com/p/formal-methods-with-hillel-wayne)), author of Logic for Programmers and Developer Educator at Antithesis. [RSVP here](https://pages.antithesis.com/webinar-property-based-testing-pragmatic?utm_medium=Email&utm_campaign=pragmatic_2026&utm_source=pragmatic&utm_content=pbt_20261001).

• **[Entire](https://entire.io/pragmatic)** – Git hosting, rebuilt for the agentic era. Entire hosts your code in-region, and is up to 89x faster than any other competitor. Mirror from GitHub [with a single click](https://entire.io/pragmatic) – I’ve already done so.

What’s changed in “the unwritten rules” of engineering management since the 2010s, what drove the shift to today, and what has it done to the engineer-to-CTO career path? Today’s guest is one of the best people I know who can tackle these questions.

[Will Larson](https://lethain.com/) is CTO of [Imprint](https://imprint.co/), was an engineering executive for 10+ years, and is the author of several popular books on engineering leadership, including ‘*[The Engineering Executive’s Primer](https://lethain.com/eng-execs-primer/)*,’ ‘*[Staff Engineer](https://www.amazon.com/Staff-Engineer-Leadership-beyond-management/dp/1736417916)*, *[An Elegant Puzzle](https://www.amazon.com/dp/1732265186)*,’ and ‘*[Crafting Engineering Strategy](https://craftingengstrategy.com/).’* Previously, he was also CTO at Carta and Calm, and an engineering leader at Stripe and Uber.

In this episode, we explore how engineering leadership has evolved from management practices shaped in the long post-financial-crisis ZIRP era to the altered expectations of engineers and managers today.

Is middle management an increasingly precarious career path, and might managers need to reskill and return to hands-on technical work – or even consider retirement? Will makes the case, and we hear why the traditional path to engineering executive roles is changing, as well as why taking one step backward might sometimes be the best way forward.

We discuss Will’s early career, and get his first-hand account of Digg’s disastrous relaunch, his own path to CTO, and he tells us why career ladders still matter. We also explore how AI is changing engineering roles and bringing technical decision-making back to leadership.

[www.youtube.com](https://www.youtube.com/watch?v=Prl858_AovM)

Here are some standout talking points from our conversation, during which Will recommends middle managers to go back to line management and re-skill like he has done. He didn’t ask me to, but I want to mention that he’s [hiring for several key engineering manager roles at Imprint](https://x.com/Lethain/status/2112399443840618670?s=20) to use agents to rethink how software teams work.

**1\. Will shares an inside account of how a 10 website sank into irrelevance after a v4 rewrite**. Digg was the “king” of social media sites in the late 2000s; bigger than Reddit, and one of top-10 most visited websites, globally. That all crashed to a halt in 2010 when the company embarked on a massive rewrite called Digg v4 – and it was a disaster. It was the start of Digg’s downfall.

Will worked at Digg during this rewrite, and shares how Digg was already losing traction and traffic to Facebook, and that Google changed its algorithm which caused 60-70% traffic loss. From the outside, Digg looked like it was growing, but its business prospects were shrinking fast. The company had about 120 people and was burning through money fast.

Into this entered the v4 rewrite, which was carried out to a hard deadline set by Kevin Rose, the newly-returned CEO at the time. The v4 version alienated passionate users, and had major performance and reliability problems.

Will and a small engineering team fixed the issues. The root cause was Python memory leaks introduced by devs who didn’t know Python so well. Will has shared more detail [on his blog](https://lethain.com/digg-v4/).

Even so, Will doesn’t think the rewrite killed Digg: with or without that rewrite, Digg would’ve met the same fate of being overtaken and replaced by Facebook and Reddit.

**2\. At Yahoo, a “black market” for servers popped up after the cofounder insisted on personally approving new servers**. A hilarious story of how incentives drive behavior: Yahoo cofounder, David Filo, wanted to drive accountability by teams justifying every server purchase. This created a “black” market and Will recalled that one engineer on his team had 30 servers left from a canceled project that he traded with devs who could not get approvals elsewhere, in exchange for stuff he needed. Also, servers didn’t have to be returned, so Yahoo ended up running a large number of outdated, power-hungry machines!

**3\. In the 2000s, having annual 1:1s with managers was normal**. Will worked at Yahoo in 2008-2010 and had two 1:1 sessions with his manager: one in which they asked Will about someone else’s performance, and the next one when he handed in his notice after it finished. It was pretty common for 1:1s with managers to be rare things at the time.

**4\. Too many Staff+ engineers only want to work on fun problems**. At Stripe, Will led an org of about 250 people, where lots of the most senior engineers regularly sought more “engaging” problems to work on. Will eventually found his optimal response:

> “I only have suffering to offer, but this suffering is what’s going to make the business succeed.”

However, he felt that he was seen by some as guilty of violating an unwritten rule of management by not finding enough – or interesting enough – problems for them to engage with. Today, at Staff+ levels, engineers putting business problems first is a baseline expectation.

**5\. ZIRP shaped engineering management practices in the 2010s**, Will believes. ZIRP stands for zero interest rate period – and we did a deepdive on the impact [ZIRP had on engineering management](https://newsletter.pragmaticengineer.com/p/zirp-engineering-managers). He recommends the article ‘[ZIRP explains the world](https://www.readmargins.com/p/zirp-explains-the-world)’ by Ranjan Roy. From Will:

> “Back then, your ability to hire and close and retain became the differentiator between successful companies. The less successful ones simply couldn’t retain great people.
>
> \[This dynamic on hiring and retaining\] pulled leverage back into the engineer’s hands and created a very different environment. So, companies were pitching themselves as a more relaxed environment, more focused on *you*, and offering more career development. And I think in ways that I think were often kind of self-defeating.”

**6\. “Hire more engineers” used to be the answer from many engineering leaders in the 2010s. Today, it’s not,** he says:

> “I tell my teams that I work with now that hiring is the least interesting way to solve every problem. We should almost never talk about hiring.
>
> But in the 2010s, we basically told managers they couldn’t think about technological solutions. And then what do they have? They have process solutions, they have scheduling, or they have people to hire.”

**7\. Focus on engineering wellbeing could have led to less reliable software**. During the 2010s, several companies chose to not put the highest-paid engineers oncall, and for it to be handled by others. But when engineers are oncall for the software they build, it leads to better, more reliable software. Back then, not having oncall was a way to hire and retain talent, but it also led to lower quality as engineering teams did not feel the pain of outages caused by their own software.

*Side note from Gergely: I wonder if AI agents will make a difference here. One of the worst things about oncall is getting awoken by the pager. Could we design loops that automatically mitigate incidents that we expect and are well-prepared for?*

**8\. Software engineering change is more extreme, and it’s driving away some engineers**. Will knows someone who decided to retire from tech because AI has destroyed the ability to be “in the flow.” Engineers are being more productive, but enjoy it less. By focusing on the outcomes, Will says he’s having a great time.

**9\. The tech lead manager role is now** ***very*** **in demand**. Will wrote previously about how the tech lead manager role [is a career trap](https://lethain.com/tech-lead-managers/), and I have a similar opinion in the deepdive ‘[Engineering leadership skillset overlaps](https://newsletter.pragmaticengineer.com/p/engineering-leadership-skillset-overlaps):’

Things are changing inside companies; EMs are starting to manage 12-14 people, instead of 4-8, as before, while Directors are managing more TLMs. At Imprint, some of Will’s best hires have been managers who also want to write software. The TLM role is perfect for this. In his book, he describes a great TLM as someone who has the capability, but no desire, for bureaucratic work; by default, they’re builders.

**10\. Engineering managers used to expect not to make technical decisions, but that’s outdated now**. Will recalls:

> “One of the worst parts of being an engineering manager in the 2010s was when you joined the ranks of engineering management, one of the first lessons you’d get was: ‘you need to never make a technical decision again.’ That’s literally what I was told.”

It was the same advice I received when becoming an EM: let your engineers make the technical decisions, get out of their way, and support them in growth. The tide is very much changing these days, with more tech companies expecting managers to be hands-on and in the details.

**11\. Great engineering executives (CTOs, VPEs) need to be technologists and bureaucrats**. Will believes an engineering executive has two jobs:

- Be a bureaucrat: manage incentives, create and deal with bureaucracy, and know how to navigate it. The job often includes running functions you know nothing about

- Be a technologist: know what to build, how it works, and whether a technology choice makes sense or not

Will reckons that until recently, many engineering executives from the middle-manager path were bureaucrats before they were technologists.

**12\. Blunt message for middle managers: retire or re-skill**. Will left a team of 250 at Carta to head up 25 tech people at Imprint. He purposely took a “step back” in his leadership career because he believes things are changing so radically that the engineering leadership profession that we know will not exist in three years.

Amid all this, Will says that line management can be a lot more engaging for executives these days because the tools for line management and the tools for writing software are really good. However, the tools of bureaucracy are still pretty terrible, he believes. That’s why some talented engineering folks get jaded about the work:

> “For a lot of folks, there is this sense the industry has betrayed them and that they’re owed something that they’ve worked for. And the thing is, they’re right!
>
> **They have worked incredibly hard, but the rules have changed.** Since the 2010s, we’ve forgotten that our industry is actually very volatile. We really do just get paid to do things that are valuable to the companies. There’s no entertainment orientation to why these companies employ us.”

• [The end of ZIRP and what it means for startups and the tech industry](https://newsletter.pragmaticengineer.com/p/zirp)

• [Getting an Engineering Executive Job](https://newsletter.pragmaticengineer.com/p/getting-an-engineering-executive)

• [What is a Staff+ Engineer?](https://newsletter.pragmaticengineer.com/p/what-is-a-staff-engineer)

• [Inside Stripe’s engineering culture](https://newsletter.pragmaticengineer.com/p/stripe)

• [Engineering career paths at Big Tech and startups](https://newsletter.pragmaticengineer.com/p/engineering-career-paths)

• [Staying technical as an engineering manager](https://newsletter.pragmaticengineer.com/p/staying-technical)

• [Performance calibrations at tech companies](https://newsletter.pragmaticengineer.com/p/performance-calibrations-part-2)

• [Scaling Uber with Thuan Pham (Uber’s first CTO)](https://newsletter.pragmaticengineer.com/p/scaling-uber-with-thuan-pham-ubers)

• [Hypergrowth startups: Uber and CloudKitchens](https://newsletter.pragmaticengineer.com/p/high-growth-startups-uber-and-cloudkitchens)

[00:00](https://www.youtube.com/watch?v=Prl858_AovM) Intro

[08:20](https://www.youtube.com/watch?v=Prl858_AovM&t=500s) Stories from Yahoo!

[12:47](https://www.youtube.com/watch?v=Prl858_AovM&t=767s) Digg’s v4 rewrite: the insider story

[26:02](https://www.youtube.com/watch?v=Prl858_AovM&t=1562s) Will’s path to eng leadership

[30:03](https://www.youtube.com/watch?v=Prl858_AovM&t=1803s) Uber, Stripe, Carta, and Calm

[35:11](https://www.youtube.com/watch?v=Prl858_AovM&t=2111s) What does a CTO do?

[38:09](https://www.youtube.com/watch?v=Prl858_AovM&t=2289s) Will’s role at Imprint

[42:48](https://www.youtube.com/watch?v=Prl858_AovM&t=2568s) How engineering management has evolved

[51:08](https://www.youtube.com/watch?v=Prl858_AovM&t=3068s) ZIRP’s impact on tech

[57:21](https://www.youtube.com/watch?v=Prl858_AovM&t=3441s) Career ladders

[1:01:03](https://www.youtube.com/watch?v=Prl858_AovM&t=3663s&pp=0gcJCWMAwfN6Pr3D) Eng 1 vs Eng 2

[1:06:58](https://www.youtube.com/watch?v=Prl858_AovM&t=4018s) How software engineering roles are changing

[1:16:25](https://www.youtube.com/watch?v=Prl858_AovM&t=4585s) Revised rules of engineering leadership

[1:21:57](https://www.youtube.com/watch?v=Prl858_AovM&t=4917s) The risks of becoming a bureaucrat as an EM

[1:25:57](https://www.youtube.com/watch?v=Prl858_AovM&t=5157s) How to become a CTO?

[1:31:33](https://www.youtube.com/watch?v=Prl858_AovM&t=5493s) Middle management vs. line management

[1:41:36](https://www.youtube.com/watch?v=Prl858_AovM&t=6096s&pp=0gcJCWMAwfN6Pr3D) Will’s writing

[1:52:13](https://www.youtube.com/watch?v=Prl858_AovM&t=6733s) Book recommendations

**Where to find Will Larson:**

• X: [https://x.com/Lethain](https://x.com/Lethain)

• LinkedIn: [https://www.linkedin.com/in/lethain](https://www.linkedin.com/in/lethain)

• Website: [https://lethain.com](https://lethain.com)

**Mentions during the episode:**

• Apple IIc: [https://en.wikipedia.org/wiki/Apple_IIc](https://en.wikipedia.org/wiki/Apple_IIc)

• Multi-user dungeon: [https://en.wikipedia.org/wiki/Multi-user_dungeon](https://en.wikipedia.org/wiki/Multi-user_dungeon)

• mIRC: [https://en.wikipedia.org/wiki/MIRC](https://en.wikipedia.org/wiki/MIRC)

• Jet Program: [https://jetprogramusa.org](https://jetprogramusa.org)

• Yahoo Search BOSS: [https://en.wikipedia.org/wiki/Yahoo_Search_BOSS](https://en.wikipedia.org/wiki/Yahoo_Search_BOSS)

• David Filo on LinkedIn: [https://www.linkedin.com/in/david-filo-230184169](https://www.linkedin.com/in/david-filo-230184169)

• Digg: [https://en.wikipedia.org/wiki/Digg](https://en.wikipedia.org/wiki/Digg)

• Kevin Rose’s website: [https://www.kevinrose.com](https://www.kevinrose.com)

• Mrbabyman on Instagram: [https://www.instagram.com/mrbabyman](https://www.instagram.com/mrbabyman)

• Jeremy Grosser on LinkedIn: [https://www.linkedin.com/in/synack](https://www.linkedin.com/in/synack)

• The Phoenix Project: A Novel About IT, DevOps, and Helping Your Business Win: [https://www.amazon.com/dp/0988262592](https://www.amazon.com/dp/0988262592)

• Site Reliability Engineering: How Google Runs Production Systems: [https://www.google.com/books/edition/_/iXnw0QEACAAJ?kptab=getbook](https://www.google.com/books/edition/_/iXnw0QEACAAJ?kptab=getbook)

• Stripe: [https://stripe.com](https://stripe.com)

• Carta: [https://carta.com](https://carta.com)

• Calm: [https://www.calm.com](https://www.calm.com)

• David Singleton on LinkedIn: [https://www.linkedin.com/in/davidpsingleton](https://www.linkedin.com/in/davidpsingleton)

• Imprint: [https://imprint.co](https://imprint.co)

• ZIRP explains the world:

• Linear: [https://linear.app](https://linear.app)

• The Mythical Man-Month: Essays on Software Engineering: [https://www.amazon.com/dp/B0722G5TKZ](https://www.amazon.com/dp/B0722G5TKZ)

• Revised rules of engineering leadership: [https://lethain.com/revised-rules-of-engineering-leadership](https://lethain.com/revised-rules-of-engineering-leadership)

• Parkinson’s Law: [https://en.wikipedia.org/wiki/Parkinson%27s_Law](https://en.wikipedia.org/wiki/Parkinson%27s_Law)

• Slack agents: [https://slack.com/ai-agents](https://slack.com/ai-agents)

• Jira: [https://www.atlassian.com/software/jira](https://www.atlassian.com/software/jira)

• Muse: [https://muse.ai](https://muse.ai)

• Seeing like a State: How Certain Schemes to Improve the Human Condition Have Failed: [https://www.amazon.com/dp/0300078153](https://www.amazon.com/dp/0300078153)

• Salary Negotiation: Make More Money, Be More Valued: [https://www.kalzumeus.com/2012/01/23/salary-negotiation](https://www.kalzumeus.com/2012/01/23/salary-negotiation)

• How to be a tech influencer: [https://lethain.com/tech-influencer](https://lethain.com/tech-influencer)

• No idea blog: [https://www.noidea.dog](https://www.noidea.dog)

• Being glue: [https://www.noidea.dog/glue](https://www.noidea.dog/glue)

• An Elegant Puzzle: Systems of Engineering Management: [https://www.amazon.com/dp/1732265186](https://www.amazon.com/dp/1732265186)

• Staff Engineer: Leadership beyond the management track: [https://www.amazon.com/Staff-Engineer-Leadership-beyond-management/dp/1736417916](https://www.amazon.com/Staff-Engineer-Leadership-beyond-management/dp/1736417916)

• The Engineering Executive’s Primer: [https://www.amazon.com/Engineering-Executives-Primer-Will-Larson-ebook/dp/B0CV4QGPXD](https://www.amazon.com/Engineering-Executives-Primer-Will-Larson-ebook/dp/B0CV4QGPXD)

• Crafting Engineering Strategy: How Thoughtful Decisions Solve Complex Problems: [https://www.amazon.com/Crafting-Engineering-Strategy-Thoughtful-Decisions](https://www.amazon.com/Crafting-Engineering-Strategy-Thoughtful-Decisions)

• Thinking in Systems: [https://www.amazon.com/Thinking-Systems-Donella-H-Meadows/dp/1603580557](https://www.amazon.com/Thinking-Systems-Donella-H-Meadows/dp/1603580557)

• Practical TLA+: Planning Driven Development: [https://www.amazon.com/Practical-TLA-Planning-Driven-Development-ebook/dp/B07FMHX7M2](https://www.amazon.com/Practical-TLA-Planning-Driven-Development-ebook/dp/B07FMHX7M2)

• Formal methods with Hillel Wayne: [https://newsletter.pragmaticengineer.com/p/formal-methods-with-hillel-wayne](https://newsletter.pragmaticengineer.com/p/formal-methods-with-hillel-wayne)

—

Production and marketing by [Pen Name](https://penname.co/).
