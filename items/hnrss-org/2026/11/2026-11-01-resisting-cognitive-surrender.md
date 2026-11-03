---
title: Resisting Cognitive Surrender
link: https://knlb.dev/essays/surrender
source: hnrss-org
published: 2026-11-01T06:13:30Z
updated: 2026-11-01T06:13:30Z
first_seen: 2026-11-03T14:36:14.641821525Z
authors:
- knlb
content: extracted
html: 2026-11-01-resisting-cognitive-surrender.html
remote_preview:
  url: https://knlb.dev/essays/.surrender_assets/image-4147323edcec4900b374ba7a6e8d4cbb.png
  alt: Managing complexity
---

Working Notes: a commonplace notebook for recording & exploring ideas. \
 [Home](https://knlb.dev/). [Site Map](https://knlb.dev/sitemap.html). [Subscribe](https://buttondown.com/knlb). More at [expLog](https://explog.in). — [Kunal](https://knlb.dev/now.html)

This particular note is around exploring *cognitive surrender*: uncritically accepting whatever agents output without actually *thinking*, exploring or considering alternatives. Working this way is extremely fast, not particularly taxing, and unsurprisingly seductive.

And yet I find myself repeatedly questioning if this is actually a good way to apply agents: often enough I find myself making suboptimal decisions, accruing technical debt and *reducing* my familiarity with the system I’m working on instead of improving it.

As we collectively rethink how we work I’ve been finding it useful to read and share other people’s workflows, and consciously write about and examine my own. This particular post focuses on finding the right balance of *trust* while working with agents.

![managing complexity](https://knlb.dev/essays/.surrender_assets/image-49a328ff5b6f473aaf09f213ccc98d46.svg)

## Cognitive surrender is seductive

[One of the virtues](https://thethreevirtues.com/) of being a great programmer is to be *lazy*. After more than a decade of training myself to look for the shortest and simplest path it’s rather attractive to be able to simply hand over the problem. If I can spin up a software factory, why wouldn’t I?

There’s also a lot of peer pressure (or game theory) at work: if any of my colleagues have seemingly surrendered, then I’m only going to look worse if I spend too much time trying to do silly things like *understanding* what I’m working on. And, *of course*, while everyone else is a slop-cannon generating code faster than they can read it I can build a meticulously designed software factory manufacturing perfect designs. The sheer *volume* of work also appears remarkably productive – we’ve all talked about just how many more PRs we ship every day.

This tends to get compounded by the agents’ expressed confidence & conviction. Perhaps an inadvertent consequence of RLHF, but models tend to be astonishingly confident in what they recommend (and perhaps even more *blasé* while admitting they made a mistake). This makes it too easy to take their answers at face value, with whatever [social programming](https://www.sussex.ac.uk/broadcast/read/38316) I seem to have evolved with.

OSS projects – particularly those facing time pressure – seem to accrue tremendous complexity quickly, to the point that it feels like the *only* way to interact with a project in any sane time frame is by heavily relying on agents to navigate and modify and test the code. This is also a self-reinforcing feedback loop: broken windows multiply rapidly with agents on the loose. [^1]

Technical debt and incidental complexity have a fairly attenuated feedback loop: complexity introduced by slop doesn’t bite immediately; it’s far more subtle and compounds over time when I’m not paying attention.

Beyond all of that, I have to mention the hype cycle. We all should be skipping Netflix to watch talks by Anthropic engineers on making loops (or the agentic workflow *du jour*). [^2]

## Why *not* surrender?

It’s not entirely obvious that it’s a *bad* idea to surrender: if it’s *easier* and *better*, then why not take that path? So far, my personal answer is that I need to take the easier path *consciously* and intentionally, with enough effort to mitigate downsides.

Part of what makes me deeply uncomfortable is suffering from a form of Gell-Mann amnesia while working with agents. When I have expertise and the overall design direction I can generally simplify the output significantly, controlling for complexity. When I don’t they seem to be the smartest engineers in existence – far beyond the more squishy, organic variety – and all I can do is follow in awe. And then the agent gets stuck creating a directory and I have to reset all my assumptions.

For the current state of the art (October 2026) I generally find maximal use of agents leads to a significant explosion in complexity: something that mostly works, with extremely surprising edge cases and bugs. These are generally the projects with hundreds of thousands of lines of code shipped rapidly, breaking the scale supported by most software development tools, so you also need to revisit those – and this compounds rapidly.

I have a strong aversion to incidental complexity, and generally refuse to build, maintain or extend it: it’s fine for a proof-of-concept, but it must end there. For systems that need to keep working, steering the design aggressively towards simplicity and modularity is what lets me build something I can actually operate (with and without agents).

As models continue to improve rapidly, I expect the previous argument to go away as the models learn and can express a broader sense of engineering taste, good design and quality. The other reason – which I unfortunately expect to persist – is that my personal understanding and mechanical sympathy for systems I maintain erode extremely rapidly, or never get a chance to develop.

In the early days when I would build things by hand I’d be able to quickly diagnose any bug or misbehavior based on my familiarity with building and maintaining the code: the more I rely on agents, the more my only option is to spin up one more session and hope; and the automated fix then further reduces my understanding in a self-reinforcing loop. Without context, I no longer have much meaningful judgement and can’t engage – or handle it in context.

[The Ironies of Automation](https://en.wikipedia.org/wiki/Ironies_of_Automation) (1983) is referred to often in articles similar to this one. I’d recommend directly reading the paper if you haven’t: it’s fully relevant to the decisions we have to make today. The paper warns that it becomes even *harder* to deal with tasks an agent fails at because we never manage to get any practice with the easier tasks; nor do we get a chance to internalize what a system *feels* like under standard operating circumstances.

![The Ironies of Automation](https://knlb.dev/essays/.surrender_assets/image-fb9c2dd38f304c90a0782e76f9f1ebb2.png)

At the end of the day, *I’m* responsible and accountable for what I build, *not* the agents. Surrendering agency and *not thinking* basically makes me accountable without any meaningful ownership. There’s also the small matter of pride in the quality of my work.[^3]

## Solutions

The sombre opening shouldn’t lead you to believe that I’m not excited about the sheer potential unlocked by AI, and the opportunities to build things that I would never have been able to without it. It’s more about resetting expectations, explicitly working through the *tradeoffs* involved and choosing which ones are acceptable. And finding ways to maintain the quality and craftsmanship I care about.

### There are still tradeoffs

The first step for me has been to internalize that I can’t *truly* have my cake and eat it: I can go terribly, inhumanly fast at building things but I need to accept that there’s no way to retain the mechanical sympathy, depth and confidence I used to have without putting in the effort. Time that used to be only counted as *building* a system also involved *comprehension*; agents can drive the time to build to zero but the next bottleneck – comprehension – hasn’t moved much so far.

The hype cycle occasionally can make it hard to communicate this explicitly: a lot of the communication around agentic loops tends to assume that there are no downsides whatsoever.

### Choose your vibe limit & triage

Several months ago I defined my *vibe limit* as the *complexity of the change I can make with sufficient confidence* using agents. I like the risk/complexity curve enough to post it again:

![Vibe Limits](https://knlb.dev/essays/djn/vibelimits2.svg#inv)

The next step is figuring out where I land: there are several projects where *good enough* is fine, or the problem is sufficiently sandboxed that I’m happy to live with whatever the agents generated. On the other hand, if the system is important, fragile, can’t be sanity checked easily or isn’t particularly well isolated I need to directly spend *my* time.

Aggressive prioritization, triage, and constantly recalibrating myself become really important in this ecosystem: I should explicitly choose where to surrender agency and where to spend my time (not unlike leading a team, except the pace is *much* faster). Constantly reviewing my predictions and the outcomes helps a lot in recalibrating, particularly with new model drops.

### Watch out for automation bias

I also find myself needing to be much more mindful while working. A warning sign I watch out for these days is if I find myself repeatedly tapping “yes, continue” in a session. At that point either I should figure out what it would take to completely delegate the problem to Claude instead of trying to micro-manage the agent – or realize that I *stopped thinking* and I need to check the decisions being made.

Completely delegating the problem generally means building a better sandbox or feedback loop, or figuring out ways to be able to sanity check much more easily before shipping to production. Alternatively, if I need to engage much more deeply I’ll use some of the other tactics described below.

### Force yourself into the loop

The premise of augmenting myself by [working in the loop with powerful thinking machines](https://thinkingmachines.ai/blog/the-future-worth-building-is-human/) was a large part of my decision to join my current company. This generally comes down to directions you can give the model to reduce the size of the steps it takes, or explicitly choosing to drive instead of passively observing.

For projects I deeply care about, *choosing to drive* instead of letting the model drive is one way to intervene in every single decision. It’s much slower to type things out, but it does force me to engage with every decision and gives me an opportunity to override it. In fact, for this post I’m using a Codex session to edit: help me tighten the post, point out the small typos, punctuation errors, or half-finished sentences that tend to embarrass me after I explicitly share a post publicly – but *I make all changes by hand*.

In the `AGENTS.md` for the notebook:

> Never edit files directly, always wait for the author to request or confirm changes, or let him make the edits directly.

This is reminiscent of how I used to work with Stack Overflow: never copy-paste code; instead write it out explicitly while cross-checking with documentation. Either I’d learn and internalize something new or I’d realize the Stack Overflow answer was suboptimal or outdated.

### Force comprehensibility

We don’t need to limit ourselves to existing interfaces anymore: with agents it’s also possible to have them build a tremendous amount of scaffolding to support comprehension and make changes in ways that are digestible.

Often enough for larger, complex stacks I have the agents build me a custom view for the Pull Request with *4* columns: the standard 2 for the diff, a 3rd column with running commentary from the agent on *why* it made a particular change and finally the 4th column as a place for me to add comments and directions.

Getting the agents to make stacks of changes with the explicit direction to [“Make the change easy, then make the easy change”](https://x.com/KentBeck/status/250733358307500032?lang=en) works really well. I used to spend a lot of time trying to make my diffs easily reviewable to respect the reviewers’ time – now I can ask the agents to do the same for me.

It’s also very valuable to dive into actual execution with a debugger (or have an agent manipulate the debugger on your behalf) till you get a deep sense of how things are flowing through the program, with the confidence that this is the real execution path.

Another example is to make harnesses explain what they’re doing: I’ve built several variations of command-line helpers that transparently fit into my shell sessions ([x](https://knlb.dev/essays/djn#x-exec) being the last public version). In my most recent iteration, I explicitly ask the model to explain any obscure flags and commands used so that I can learn as I go.

### When you *do* decide to delegate

Sometimes, it does make sense to hand over the problem to an agent: it could be too big, not valuable enough, time could be too short, or you just want to get a first prototype out to explore a problem end-to-end.

This one is probably an essay by itself: adapting old-school best practices when you have infinitely fast stochastic intelligence on top. The idea here is to reduce the risk in making changes, flattening the vibe-limit curve and making it possible to make bigger changes safely.

Beyond giving the agent some means to verify itself against the goal you’re aiming for, I find it very useful to:

- **Observability**: Invest heavily in observability, both black-box and white-box, and make sure to keep a direct handle on the metrics instead of solely relying on the agents’ view. As a habit I prefer to triangulate onto a behavior with multiple sources of metrics, and that becomes even more important with systems rapidly becoming opaque.
- **Non-functional requirements**: Don’t forget to observe resource utilization and other non-functional requirements: these can be canaries for unexpected behavior.
- **Deterministic validation**: Build mechanical straitjackets for the model: have held-out pieces of code or deterministic tools for validation and observations as a hold-out against the model misunderstanding or glitching.
- **Shadow Traffic**: Make it *possible* and *cheap* to run a deployed candidate against shadow traffic with no side effects: keep it as similar to production as possible, and this can be the final gate to prevent surprises.
- **Style guides**: Give the model a style guide based on your existing code and work. I’ve found Claude’s output far more to my taste after having the models generate a style guide based on all the code and blog posts I’ve written in the past.
- **Fast deploys**: An often maligned motto at Facebook used to be *Move Fast & Break Things*: that, too, is an intentional tradeoff to make. If you can’t be sure of quality, make it very easy to deploy code as well – so changes can be shipped and reverted extremely rapidly.
- **Rethink what’s possible**: It’s surprisingly easy to write and rewrite systems from scratch rapidly as you learn. Build to throw, evaluate, and repeat to get something knocked into great shape – I’ve built projects in multiple iterations, giving the agent the previous iteration and its limitations as initial context and constantly finding myself able to deeply increase the quality. This doesn’t work as well when you’re punishing your code reviewers though, so make sure to align with your team, or simply don’t get the first several iterations reviewed.

## Good design is a force multiplier

In *Notes on the Synthesis of Form* Christopher Alexander has a wonderful example demonstrating the value of decomposition – or building a lightly coupled, cohesive system to be able to succeed quickly.

> No complex adaptive system will succeed in adapting in a reasonable amount of time unless the adaptation can proceed subsystem by subsystem, each subsystem relatively independent of the others.

![Screenshot 2026-11-01 001757](https://knlb.dev/essays/.surrender_assets/image-3bba85ffe42f40e6856194e11094165d.png)

Whether agents or humans are doing the work, a good design makes the system much easier to understand, iterate on, validate and extend. As you learn the limitations of the design, you also now have the ability to redo it much more rapidly than ever before. Regardless of how much depth you decide to maintain in your projects, I’d make sure the broader structure continues to make sense.

## Additional reading

There are several interesting articles, talks and books that are relevant:

- [Addy Osmani](https://addyosmani.com/blog/cognitive-surrender/) with several additional techniques
- [Guy Levi](https://medium.com/@guylevi.57/avoiding-cognitive-surrender-e29f8a487192) recommending leaning into critical thinking
- [Cory Doctorow](https://craphound.com/news/2026/06/23/the-reverse-centaurs-guide-to-life-after-ai-launch-at-keplers-books-with-angie-coiro/) talking about handling being forced to use AI
- [Stuart Halloway](https://www.youtube.com/watch?v=7yeH1DqK984) emphasizing that AI is automation, and the importance of friction to build *metis*.
- [Elizabeth, SigNoz](https://newsletter.signoz.io/p/ai-isnt-replacing-sres-its-deskilling) on skills atrophying.
- [Ricard Solé et al.](https://arxiv.org/abs/2609.03344) on LLMs as a cognitive virus, modeling the interaction and consequences of cognitive offloading.

I’m curious if you have any other recommendations around effectively leveraging agents without losing context – I’d love to hear about additional experiments to try or workflows to adopt! Particularly if you’ve successfully recovered projects that collapsed into slop. Discuss on [Hacker News](https://news.ycombinator.com/item?id=50323123).

[^1]: I have constantly been expecting more software to fail under the weight of its own complexity. One of my hot takes is that this hasn’t happened at the scale and magnitude I expect yet because agents became smarter fast enough to keep up with the debt introduced by the previous generation.

[^2]: You’ll pry my weekly indulgence in *The Great British Baking Show* from my cold, dead hands.

[^3]: You could argue that this is a very “Individual Contributor” approach to building things, and that managers deal with this all the time. I’ll quote Andrew Grove on management: “delegation without follow-through is abdication” – you still need to engage to get the outcome you need.
