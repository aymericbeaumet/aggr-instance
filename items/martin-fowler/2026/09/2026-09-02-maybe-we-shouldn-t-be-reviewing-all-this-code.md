---
title: Maybe We Shouldn't Be Reviewing All This Code
link: https://martinfowler.com/rachels-ramblings/code-review.html
source: martin-fowler
published: 2026-09-02T13:32:00Z
updated: 2026-09-02T13:32:00Z
first_seen: 2026-09-04T09:21:49.209340827Z
authors:
- Rachel Laycock
labels:
- rachels-ramblings
content: extracted
html: 2026-09-02-maybe-we-shouldn-t-be-reviewing-all-this-code.html
---

**TL;DR**\
*Or, perhaps the problem isn't that AI has broken code review, maybe it’s that we've been using code review to solve the wrong problems*

I was on a panel recently with Brian Houck from DX at Code Remix, hosted by Moderne. It was one of the more interesting panels I’ve done, largely because we disagreed. As my colleague Martin Fowler says, panels are much more interesting when people disagree and both sides have a good argument. Brian and I definitely did.

Brian has since written a thoughtful piece called [*What are code reviews even for?*](https://newsletter.getdx.com/p/what-are-code-reviews-even-for) He is clearly passionate about his position, and I am passionate enough about mine that I’m writing this response. To be clear, I think we mostly want the same things. I just don’t think code review is the best way to get them. Brian is lovely, by the way, and encouraged me to write this. But I’d be lying if I said I didn’t want you to think I’m right by the end :)

So what were we disagreeing about?

AI is producing more code than humans can realistically review. Brian cites some pretty striking numbers: at Meta, significant lines of code per human-landed diff reportedly increased 106% in a year, while DX’s own data shows median pull request size increasing 64%.

His concern, which I share, is that simply automating code review away risks losing all the other things we use it for. Code review isn’t just about finding bugs. It’s how teams share knowledge, teach junior engineers, build collective ownership and spread architectural understanding.

My question is: **why are we waiting until code review to do all of those things?**

I’ve never particularly liked pull requests as the centre of the software development process. Not because engineers shouldn’t look at each other’s code, but because I’ve always struggled with the idea that we should build something, finish it, package it up, throw it over to somebody else and *then* have the important conversation about whether we built the right thing in the right way.

And don’t even get me started on merge conflicts. I’ve lost too many hours of my life.

## **Shift the judgment left**

One of the principles I learned very early at Thoughtworks was to shorten feedback loops. If feedback is valuable, don’t remove it. Move it closer to the decision it is informing.

Take the things we say code review gives us.

If we want to **explore alternative solutions**, I’d rather do that before implementing one of them.

If we want **knowledge transfer**, pair. Sitting next to someone, physically or virtually, while they reason through a problem teaches you far more than reading their completed solution afterwards.

If we want **junior engineers to learn how experienced engineers think**, let them work with experienced engineers while they’re thinking. Pairing comes to mind again here, but teams could also do design sessions collectively with a whiteboard before they write (or instruct the agent to write) anything.

If we want **collective ownership**, organise teams so people actually build and operate software collectively rather than relying on a pull request to tell everyone what somebody else has already built. For this again use pairing, mob programming, or team design sessions around whiteboard.

If we want **architectural alignment**, design together (I won’t repeat myself about pairing and team design sessions, oh wait…) and then encode the important constraints as fitness functions.

And if we’re reviewing code for formatting, linting, known security problems or things that can be deterministically tested, automate them. We really shouldn’t still be arguing about whitespace in 2026.

Pair programming, trunk-based development, automated testing, static analysis, fitness functions and security scanning all move feedback earlier. Increasingly, agents can participate in those loops too, challenging designs, testing assumptions and continuously verifying what is being built, but the real thinking is coming from experienced humans and if we want that experience to benefit the whole team then we have to act like one much earlier than code review.

## **Review by exception**

None of this means nobody ever reviews code. There are absolutely changes where I want another experienced human looking. An example would be a fundamental architectural change. Assuming we did a design session as a wider team, we might want to review the code as a team or agree it was implemented right, or discuss if we want to change anything. Other examples could be something crossing a sensitive security boundary, a change with a huge blast radius, an unfamiliar part of a critical system or simply something where the team says, “I’m not confident about this.”

Those are exactly the places where human judgment is valuable, but that’s very different from requiring a human to inspect every change because that’s the ceremony we’ve historically used to create confidence.

And we know now it’s not viable to continue down this path, hence why code review keeps coming up as an issue or a blocker. If an agent can produce ten times the code but every line eventually queues up waiting for a senior engineer to inspect it, we haven’t created a ten-times engineering organisation, we’ve created a big backlog and a new bottleneck.

And I don’t think the answer is an AI agent pretending to be the human reviewer so we can preserve exactly the same process at higher speed. That’s automating the ceremony rather than questioning why the ceremony exists.

There is one thing I do worry about in Brian’s argument, though. He talks about teams accumulating cognitive and intent debt: software grows while the humans responsible for it understand less and less about why it works the way it does. I think that’s a very real problem. I just don’t think mandatory pull requests are a particularly strong defence against it.

If agents are going to produce substantially more of the implementation, we need to be much more deliberate about maintaining human understanding through collaborative design, pairing, good boundaries, executable architecture, shared operational responsibility and probably some practices we haven’t invented yet.

**We need engineers to understand systems, not diffs.**

Perhaps that’s what AI is exposing. We’ve spent years loading an extraordinary number of responsibilities onto the humble code review: quality gate, security check, architecture review, mentoring mechanism, knowledge-sharing system, ownership model.

It worked, sort of, while humans could only produce code so quickly. That constraint is disappearing. So perhaps the question isn’t how we get the code reviewed faster. Perhaps it’s why we’re waiting until code review to have all the important conversations in the first place.
