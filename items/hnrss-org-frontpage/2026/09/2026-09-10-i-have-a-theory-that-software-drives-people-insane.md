---
title: I have a theory that software drives people insane
link: https://graybeard.ing/software-drives-people-insane/
source: hnrss-org-frontpage
published: 2026-09-10T16:13:42Z
updated: 2026-09-10T16:13:42Z
first_seen: 2026-09-10T20:00:28.523525536Z
authors:
- rglover
summary: 'Article URL: https://graybeard.ing/software-drives-people-insane/ Comments URL: https://news.ycombinator.com/item?id=49646181 Points: 246 # Comments: 97'
content: extracted
html: 2026-09-10-i-have-a-theory-that-software-drives-people-insane.html
preview:
  file: 2026-09-10-i-have-a-theory-that-software-drives-people-insane.preview-ab4a2211cb1a.webp
  width: 256
  height: 256
  color: '#fbfbfb'
images:
- source: https://graybeard.ing/static/og-image.png
  original:
    file: 2026-09-10-i-have-a-theory-that-software-drives-people-insane.image-4f0fffc0914b.png
    width: 1200
    height: 1200
  variants:
  - file: 2026-09-10-i-have-a-theory-that-software-drives-people-insane.image-921ef03576ce.webp
    width: 48
    height: 48
  - file: 2026-09-10-i-have-a-theory-that-software-drives-people-insane.image-62a1f396a800.webp
    width: 320
    height: 320
  - file: 2026-09-10-i-have-a-theory-that-software-drives-people-insane.image-70d4812dff3d.webp
    width: 640
    height: 640
  - file: 2026-09-10-i-have-a-theory-that-software-drives-people-insane.image-d5e7b3801e96.webp
    width: 960
    height: 960
  - file: 2026-09-10-i-have-a-theory-that-software-drives-people-insane.image-d135e20925e6.webp
    width: 1200
    height: 1200
  color: '#fefefe'
---

*10 Sep, 2026*

I have a little pet theory that software drives people insane.

Not in the "wash your hands every thirty minutes like Howard Hughes" kind of way, but more so in how the conditions surrounding software seem remarkably effective at making otherwise normal people lose their sense of proportion.

I've watched this happen enough times from enough angles that I don't think it's entirely a personality problem.

Software combines speed, money, complexity, abstraction, and almost unlimited freedom to change your mind. On their own, those things are perfectly manageable, but put them together and you start to get some really bizarre side-effects.

Most software, when you strip away the branding and architecture diagrams, is remarkably boring. A form here, an API endpoint there, sprinkle in some permissions, calculations, workflows, and a database somewhere. Maybe a queue if you need to scale something (or you're feeling adventurous).

But, harsh as it may be: most software is still just a glorified spreadsheet.

And yet somehow the process of producing this stuff can turn perfectly ordinary adults into b-tier Bond villains. The project is never moving fast enough, the plan has to be ever-flexible to sudden change, and every *new* feature is the one that's going to be *the one* (but certainly not that one that was supposed to be *the one* during the previous sprint).

There is always some new concern about whether it will work, whether it will scale, whether we're moving fast enough, or whether we should be doing something else entirely.

The weird thing about software is that many of these ideas are *technically* possible. That's part of the problem: there's very little natural friction between an idea and its implementation. If you're building a house and somebody decides halfway through framing that the kitchen should be on the opposite side of the building, everyone immediately understands that decision has a cost.

Boards have been cut, plumbing has been run, and people have to tear already-fixed things apart. The cost is physical enough that nobody can pretend it doesn't exist. With software, that cost hides inside people's heads and inside systems that are, typically, already difficult to reason about.

Moving the kitchen in software might look like a "simple fix." The work is still costly, but the actual expense accumulates quietly. Somewhere between context switching, regression risk, and architectural erosion we find lost momentum, forgotten assumptions, and endless meetings to "get aligned."

No matter the actual complexity, it's easy to pretend the change was free (as in beer) because there isn't any visible dust or scraps. The problem is made worse by the fact that changing software really *can* be cheap...sometimes.

A useful adjustment might genuinely take an hour, while another equally simple-looking request can ripple through an entire system causing failures. That opaqueness creates a dangerous habit where every "cool idea" that's "super quick" gets added to the roadmap, often with urgency.

Someone has an idea in a meeting and there is often very little resistance between the idea and reality. Could we make this screen work differently? Could we change the business model, chase another customer segment, introduce another workflow, or build our own event system?

The answer is almost always some variation of "sure, we could."

Eventually, "could" becomes "should," and "should" becomes "why isn't it done yet?" This is where software starts doing something strange to people's brains. Everything becomes urgent because everything can move quickly, and every decision starts feeling strategic because the theoretical upside can be enormous.

Every technical choice also becomes ideological because there are dozens of plausible ways to solve the same problem. Every slowdown begins to look like a crisis because somewhere, somebody else is (supposedly) moving faster. The industry has very few natural mechanisms that tell people when enough is enough.

There isn't really an obvious definition of "done" in software (I'll reluctantly share [a clip from The Social Network](https://www.youtube.com/watch?v=1yGQV6JZKtk) that illustrates this attitude well).

A carpenter eventually puts down the hammer because the cabinet exists, but software can always be improved. The button *could* be better, the query *could* be faster, the abstractions *could* be cleaner, the onboarding *could* convert more people, and the infrastructure *could* scale further. The product could expand into an adjacent market, the pricing could change, or the entire company could decide it has discovered a more lucrative direction.

The point: if you want there to be, there's always another lever within reach.

I think this is one reason software organizations become so neurotic: they are surrounded by levers, and people who are surrounded by levers eventually start pulling them.

Sometimes they pull them because something is genuinely wrong. Other times they pull them because they're scared, because the board wants growth, because a competitor shipped something, because the numbers were flat this month, or because nobody knows what else to do.

A founder changing direction every few days gets mythologized as "responding to the market." A manager constantly pressuring people to move faster is considered execution focused, while an engineer introducing several new infrastructure components may be praised for thinking about scale. A product team rebuilding a working interface because conversion dropped slightly is iterating, and a company abandoning its original identity because another category is suddenly fashionable is pivoting.

Some of this language exists because the underlying motivation is legitimate. There are times when you *should* move quickly, times when you *should* pivot, and times when the architecture really *does* need to change. What makes software dangerous is how easy it is to confuse the existence of a take-able action with the need to actually take it.

And if this wasn't bad enough, money just pours gasoline on the whole thing.

There are very few industries where a small group of people can sit in a room, type for a few years (or now, steer agents), and plausibly produce something worth hundreds of millions of dollars. That possibility changes the emotional weight of what is traditionally mundane work (to anyone who isn't a nerd).

You can watch sensible people argue for an hour over a button because, somewhere deep inside the conversation, the button has become connected to a future pile of money.

Once that happens, ordinary judgment is thrown to the wolves. The work stops being about whether the button is tied to something useful and starts carrying the weight of everything people hope the company might become.

Complexity rushes in to fill the same gap. Software is unusually good at making complexity feel important, because complicated systems make ordinary problems seem more serious.

A boring application that stores records in a database and lets people edit them doesn't sound especially impressive, while a distributed event-driven platform with a service mesh and realtime synchronization layer sounds like you're building NORAD.

Sometimes you really do need the complicated thing. Most of the time, you probably don't. But complicated systems provide psychological rewards that simple systems don't. They give people things to design, debate, own, optimize, rewrite, diagram, benchmark, and talk about.

Complexity creates work and work creates an air of importance. Importance creates status, and before long the system exists partly to support the organization that exists partly to support the system. The whole thing becomes self-reinforcing in a way that is surprisingly hard to notice from the inside.

I suspect there is also a deeper attraction underneath all of this. Software gives us an unusual amount of control, because code is one of the few places where you can describe what you want with enough precision and have a machine reliably follow those instructions. The rest of reality is much less cooperative.

Reality is messy and stubborn, while software gives us the impression that messiness—technically or conceptually—is a problem waiting to be debugged.

So we start trying to debug everything around the software too. Growth is slow, so we change the funnel; customers are confused, so we redesign the product; development is slow, so we change the process; the process is slow, so we change the tools. The company is struggling, so we reorganize it, and if it is still struggling (and dependent on venture capital), maybe we pivot.

There is always another variable to manipulate.

Eventually the company itself starts being treated like software: permanently mutable, permanently unfinished, and permanently one refactor away from working properly. This is where the madness really settles in, because *nobody can leave anything alone*.

Leaving things alone is an underrated engineering skill. There is a point in your career where you begin to understand that a surprising amount of good work comes from refusing to touch things that are already doing their job. The database does not always need to be replaced, the framework is often fine, and the onboarding does not need another redesign this week.

The architecture does not need to anticipate a billion users, and the roadmap does not need to change because somebody got excited by a tweet. The product does not need to become a platform, and the company does not need to rediscover its identity every quarter. Sometimes the thing just needs to sit there and work.

Customers often need time to find a product, engineers need time to understand a system, and businesses need time to become businesses. None of this sounds particularly profound, but [software culture is remarkably hostile to patience](https://graybearding.bearblog.dev/the-religion-of-speed/). Patience looks suspiciously like inactivity, and inactivity is difficult to justify in an industry obsessed with velocity.

So we create activity instead. We ship, iterate, optimize, pivot, replatform, rethink, and reinvent until the original problem is barely visible under everything we've layered on top of it. Then, a few years later, somebody quietly proposes rebuilding the original simple thing (often pointing to their own self-righteous wisdom as the guiding light that made that insight possible).

I don't think the answer is moving slowly for the sake of moving slowly. That would just be another ideology, and software already has enough of those (god help us). The answer is *proportion*.

It means understanding that not every problem is existential, not every idea belongs on the roadmap, and not every abstraction deserves to exist. It means accepting that not every slowdown requires intervention, not every competitor matters, and not every piece of software needs to become a platform. It also means remembering that not every company needs to seek World Domination.

I'll reiterate: most software is still, despite all this ornament, a glorified spreadsheet. That isn't intended as an insult. Spreadsheets are useful and (actually) useful software is enough.

But maybe we'd all be a little saner if we remembered what we're actually doing: building tools to make our lives and the lives of others easier. Not waste away in Margaritaville tinkering with shit that doesn't need tinkering with.
