---
title: 'Fragments: July 13'
link: https://martinfowler.com/fragments/2026-07-13.html
source: martinfowler-com
published: 2026-07-13T12:51:00Z
updated: 2026-07-13T12:51:00Z
first_seen: 2026-09-04T12:14:41.802665207Z
authors:
- Martin Fowler
content: extracted
html: 2026-07-13-fragments-july-13.html
---

Some more of my notes from [Thoughtworks Future of Software Development Retreat](https://martinfowler.com/bliki/FutureOfSoftwareDevelopment.html).

When we had our first retreat in Utah early this year, nobody had heard of [Harness Engineering](https://martinfowler.com/articles/harness-engineering.html). This time we had a whole session on it.

When comes to the guide side of harnesses, most of the discussion is about context management. While context windows have increased is size as models get more sophisticated, that doesn’t mean that models will properly focus on the right bits. Models typically only focus attention on part of the context, and to get the best behavior, we need to manage that focus. One attendee keeps their context small, limiting the `agents.md` file to less than 200 lines

On the sensor side, we see more attention on computational sensors. Two patterns from one participant was shifting to languages with greater controls, (eg Rust rather than Python) and “leveling up” validation approaches, using more property-based testing and techniques from formal methods. One commented that while they aren’t smart enough to write specifications in a formal specification language, they are smart enough to read it and check it makes sense for their domain.

Will our attention on harnesses last long enough for our next retreat? Will the models just get so good that harnesses become unnecessary? Those with some [mechanical sympathy](https://martinfowler.com/bliki/MechanicalSympathy.html) for LLMs seem to think not - but are they overly coupled to the current state of technology? I find such speculation tends not to lead anywhere useful, I’ve not seen much success in guessing the future in the past, and with technology as radical as this, I don’t see it being any easier. So for the moment, attention to harnesses pays off. We find it reduces token usage, and also allows weaker models to be useful, supporting such things as local hosting of open-weight models.

 ❄                ❄

Which naturally segues me to a session on self-hosted models. Increasing token costs have made hosting an open-weight model more attractive, particularly due to the [decreasing time for open-weight models to catch up](https://redmonk.com/sogrady/2026/05/15/open-ai-models/) with frontier models. Cost isn’t the only factor, however, many folks find a desire to be independent of the frontier model firms to be the the driving force. After all we’ve seen the U.S. government intervene to deny access to models, increasing the desire for greater model sovereignty. Information security is also something to consider, some attendees just can’t give models necessary data for critical work. Even without that, if someone else hosts the model then their model learns rather than your model. And although recent events have increased interest, several participants worked with companies that had been self-hosting for up to a couple of years.

Is this trudging down the same path of self-hosted clouds, which led to lots of folks spending excessive funds on [half-arsed private clouds](https://martinfowler.com/articles/talk-about-platforms.html#TheHalf-arsedSuperficialPrivateCloud)? The answer hinges upon whether it ends up being simpler to host a model than a cloud, perhaps due to a simpler interaction protocol. The hard part of this may be the talent required to efficiently use the GPUs, managing an inference data center currently isn’t a widely available skill. Even self-hosted models are a cost to operate, capital costs in GPUs, ongoing costs in electricity. The physical design of a data center can affect optimal usage. There’s an opportunity here for professional services firms to help companies manage this.

Cost control also involves teaching people to pick the right model for the job. Can we teach engineers, or indeed other users, to pick a less-powerful model? This, of course, could be a job for model itself, acting as a broker, deciding which model is the best choice to tackle certain jobs.

Self-hosting may lead to a greater use of fine-tuning. Currently that’s a niche activity, but over time we could well find that models that are fine-tuned to a particular domain need less reasoning, consume less tokens, and thus are cheaper to operate. We are seeing models trained specifically to support programming.

As with any topic with this degree of uncertainty, the big win isn’t finding the right answer, but coming up with a strategy that will cope with the inevitable and unpredictable changes.

 ❄                ❄

After an event like this, many people come up to me and ask me to make some grand summing up. I hate this, because I rarely leave these kinds of event with some grand narrative. Even after mulling on it afterwards (in writing the above notes) I still usually don’t have one, and distrust one that forms, as my skepticism includes attempts to make coherent narratives of an event that’s naturally rather jumbled.

However my failings are irrelevant this time, because Kief Morris has put together such a narrative, [and it’s a convincing one](https://infrastructure-as-code.com/posts/fose-july-2026.html), even to a narrative-denier like me.

> The sessions had different titles and different casts, and on the surface they were about different problems.
>
> But they weren’t. Nearly every one of them was a different facet of the same argument. How much do we let an agent decide, and how do we stay confident in what it does?

He looks at code review, questions whether it matters, but sees that the rigor that many associate with code review shifts to other forms. He describes the disagreements about how much we should trust an agent to identify and fix production incidents. He sees that the contrast between how much leeway teams give to agents depends on the context they are operating

> Underneath all of these sessions, the operations debate, the wide-remit team, the dark-factory spectrum, the argument about who’s allowed to steer the model, people were making the same handful of choices over and over about a single thing: the unit of work they were prepared to hand to an agent. How big it is. How much of the job it covers. What you do to get it ready to hand over. How you check what comes back. What you put around the agent to keep it inside the lines. Different rooms set those differently, but they were setting the same controls.

 ❄                ❄

Sam Ruby convened a session called “Bring me a Rock”. The name evokes a particular kind of management dysfunction. The manager tells his underlings to bring him a rock, and then starts rejecting the results without explaining why (“no not that one”, “no not that one”) until eventually one rock matches the unstated expectation.

> It names a manager who substitutes serial rejection for the work of saying what they want, and makes you pay for their unfinished thinking one rock at a time.

Sam had already written why he thought with LLMs, [this changed from a slur to a defensible way to work](https://intertwingly.net/blog/2026/06/27/Bring-Me-a-Rock.html). When its a bunch of tireless machines with endless patience, that return new rocks in minutes rather than days, then an approach like this (using the [brainstorming register](https://martinfowler.com/fragments/2026-06-16.html#registers) becomes a defensible way to work.

Sam [described the discussion](https://intertwingly.net/blog/2026/07/01/What-Survived-Contact.html):

> The room pulled it somewhere narrower than I’d framed, and the narrower place was the more interesting one: not how to explore by elimination but who should even be allowed to. Product managers, increasingly people managers, are reaching for these models directly, and seasoned engineers get measurably better results from them than untrained people do — so the worry followed. If expertise is what separates a good outcome from slop, should non-engineers be steering the model at all?
>
> It’s a fair question, and I think it’s the wrong one, because it mistakes the act. When a manager reaches for an LLM instead of routing the work to the team that reports to them, they didn’t pick up a tool — they made a hire. And you don’t ask permission to manage your own team; a manager who decides a piece of work is better given to a new participant than to the existing one is doing the most ordinary thing a manager does. Framed that way, the permission question dissolves into an older, better-understood one — the one Drucker named in 1959: when the worker knows more about the specifics than the manager does, you manage by objective, not by method. The non-engineer steering an agent is exactly that manager, out-known by the thing they’re directing, and the slop the room feared is the old danger of managing by method when you should be managing by objective. The question isn’t may they hire? It’s do they know how to manage by objective? — which you can teach, hire for, and hold people to without anyone first becoming an engineer.

Sam’s article explores managing an LLM by objective, giving it a goal rather than a task. And Kief’s earlier point about the essence of the discussion still holds: how confident can we be that it’s done the right thing? We can outsource many things, but not the acceptance criteria, at some point there’s a human request and a human judgment on whether that request was properly executed. But the danger lies in important unstated objectives, unstated perhaps because they weren’t even imagined.

It’s easy to state objectives around desired functionality. Give me a an application that will examine my emails and form a todo list for today. But behind that simple statement is a thicket of unstated assumptions. We tend to assume [The Genie](https://martinfowler.com/articles/who-is-llm.html) won’t include any undesired functionality, perhaps deleting emails it thinks are unworthy of our attention. We assume it won’t let an email tell it to send private information to villain@evil.com. We have some hope here - we hear more experiences that suggest that recent models can do an excellent job of finding (and hopefully fixing) security holes. The careful precision of the machine outruns the sloppy if imaginative thinking in squishyware. Perhaps we can assume the genie can take care of some of our unstated objectives. Conformance tests (sensors) are more valuable than specifications (guides), but it’s hard to imagine all the conformance tests that are needed to say what shouldn’t happen.

Furthermore, building software is about exploration, finding out how a workflow can evolve as machines are embedded in the process. For a human to guide that process, we need some understanding of it. My sense is that model building is still important, and while I agree that the genie can take an active role in that construction, I don’t think the human can entirely outsource it. Even if the genie builds the model itself, it needs to teach us that model, because the model helps us imagine and communicate the goals, the objectives that we give to the machine.

 ❄                ❄                ❄                ❄                ❄

If you follow my feeds (which you probably do if you’re reading this), then you’ll know that Birgitta Böckeler has written a couple of memos on working with local models. She first looked the [factors that influence how viable they are for programming](https://martinfowler.com/articles/exploring-gen-ai/local-models-for-coding-factors.html), and then related some of her [recent experiences evaluating such models](https://martinfowler.com/articles/exploring-gen-ai/local-models-for-coding-experiences.html).

As a nice, if accidental, complement to these, Sebastian Raschka wrote a [detailed guide to his local model environment](https://magazine.sebastianraschka.com/p/using-local-coding-agents). Like Birgitta, he’s found the Qwen 3.6 model to be the current sweet spot for local agentic programming.

 ❄                ❄                ❄                ❄                ❄

Simon Willison shares [a useful tip to save money](https://simonwillison.net/2026/Jul/3/judgement/#atom-everything) while using the latest Anthropic Fable model

> Tell Fable to use other models for smaller tasks, applying its own judgement about which model to use.

 ❄                ❄                ❄                ❄                ❄

Josh Comeau writes a blog and online courses for developer education, primarily front-end web material. His been successful for most of this decade but has found his online courses have had [only ⅓ the sales this year](https://bsky.app/profile/joshwcomeau.com/post/3mkxyqgrp2d2t). He attributes this to AI, partly as people worry if it’s worth spending money on a job that may not have a future, but also because AI can provide personalized tutoring.

> ideally, it shouldn’t cost any money to learn stuff.
>
> But I sorta worry about how this is supposed to work, going forwards, if there’s no incentive for people to make high-quality free content.
>
> I’ve spoken to a few course creators now, and we’re all seeing the same trend. Revenue down 50%+. Fewer people engaging with our content. People switching to LLMs, which slurp up all of our work and regurgitate it, without consent or compensation.
>
> It feels pretty bleak. 😅

 ❄                ❄                ❄                ❄                ❄

John Gruber is [annoyed that Claude’s desktop app for MacOS in uses Electron](https://daringfireball.net/2026/07/claudes_criminally_bad_mac_app_is_an_inside_job).

> Electron guarantees that an app feels just as wrong on all platforms.

He has some tasty invective for the folks at Anthropic with ties to the Electron platform.

> Finding out that one guy — who is a senior Electron maintainer — has led the teams for the desktop clients for Slack, Notion, and now Claude is like discovering that it was one guy — whose family business was a distillery — who helmed the Titanic, piloted the Hindenburg, and then served as air traffic controller for Amelia Earhart.

The deeper question here is whether there should be a future for cross-platform front-ends in the world of agentic programming. There’s lots of evidence that coding agents do a great job of building the same thing in multiple languages and platform ecosystems. That should mean that the days of least-denominator cross-platform UIs are numbered - and that number is small.

 ❄                ❄                ❄                ❄                ❄

Dan Davies tries to draw [a distinction between interactional and contributory expertise](https://backofmind.substack.com/p/faking-versus-making). Contributory expertise is that held by people who are doing the work to advance a field of study, interaction expertise is held by folks that spend time talking to contributory experts, building up a decent store of knowledge themselves, but not steeped in the day-to-day of the work.

> it seems to me that there is an important distinction here, which is not any less important because the dividing line might be difficult to establish empirically, or even if that line turns out to be in a different place from where we guessed it was. As well as difficult cases where it’s not clear, I think we could also come up with cases where the distinction between interactional and contributory expertise would suddenly become very clear and important indeed – the ones where someone who was faking it got “found out”.
>
> And so the question that I think is quite important is whether there is a similar kind of distinction between the kind of expertise that it’s possible for a machine to get by industralised consumption and interaction with a much larger corpus of literature than any human being could inhale, and genuine contributory expertise that could apply to entirely new situations outside that literature.

As a human, I’d like to think I’m more of a contributor than an interactor (especially given my increasing introversion), and thus relatively safe from being forced into obsolescence by silicon. But I’m also aware that my career is devoid of any original ideas, my skill is only that of someone who is good at selecting and explaining the ideas of others. (As Brian Foote put it more memorably: “an intellectual jackal with good taste in carrion”.) But there’s skill in being a good jackal too - and we don’t really know yet where the real boundaries of the LLMs will lie.
