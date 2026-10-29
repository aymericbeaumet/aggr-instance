---
title: Why are good developers so wary of agentic development?
link: https://martinfowler.com/rachels-ramblings/good-devs-wary.html
source: martinfowler-com
published: 2026-10-29T13:50:00Z
updated: 2026-10-29T13:50:00Z
first_seen: 2026-10-29T15:30:03.869733718Z
authors:
- Rachel Laycock
labels:
- rachels-ramblings
content: extracted
html: 2026-10-29-why-are-good-developers-so-wary-of-agentic-development.html
remote_preview:
  url: https://martinfowler.com/rachels-ramblings/card.png
---

**TL;DR**\
*Good developers’ caution about agentic development comes from knowing that built isn’t the same as working in production.*

I’ve been surprised by how many excellent developers have been hesitant to go all in on coding agents. These are people who usually try new ways of working before anyone else and in many cases invent new tools and practices that support building the application well. So why have we had so much reluctance?

The easy (lazy?) answer is that they love writing code, or they’re worried about their jobs. I’m sure that’s true for some people, some of the time. And I know it is true for some of the developers I talk to but I always felt there was something else, another fear hiding behind those obvious ones. When we put together the [Technology Radar](https://www.thoughtworks.com/en-us/radar) every six months I try to block out a chunk of time for us to do an open space. In September, a discussion we had on agentic development, which pushed it to its limits, finally helped me see clearly what that fear is.

## Building it right

Thoughtworkers care deeply about *building the right thing and building it right*. It’s pretty much our default way of working and thinking and it’s where many of the practices and technologies we invented and championed came from. We want software that is useful and helpful, can be changed next year, and doesn’t fall over in production. Our developers adopted, championed or invented some of the first versions of continuous integration, pairing, automated testing, continuous delivery, microservices and many others early because they helped us build the software right.

Early coding agents made it very easy to produce code, including code we wouldn’t have wanted anywhere near production, written by people who have not learned about the chasm between building something and making it run in production safely. The tools have improved enormously, but experienced developers still have questions. Are we confident this is the right behaviour? Does it work as expected? Are these the right boundaries? Will we be able to evolve it? Does it abide by the principles and practices we learned the hard way to give us confidence it will meet the needs of the customer and stay up in production? What happens when something fails? Will another team be able to live with it? And so on.

Underneath all of those sits a bigger question. **How much can we trust a system when nobody has experience of building it this way?** Let me state that another way. Nearly every system we have in production now was built the old way, by hand, by people we could trust (hopefully). Everything we build now is the new way and we have not lived with that yet.

After more than 25 years in technology, I’ve come to value the judgment people develop through experience. I regularly put similar questions to models and to senior technologists at Thoughtworks. For the difficult questions, I still get more useful answers from the humans or maybe more insightful with very specific experience that helps me better understand the point. They challenge the premise, recognise a pattern from something that went wrong ten years ago, or spot the organisational problem hiding inside the technical one. I can now access an extraordinary amount of information. I still need people who can help me think through what matters, I still need wisdom and insight. That type judgment is exactly what hesitant experienced developers are trying to protect.

And I want to be clear: they are not hesitant to use a coding assistant, they are hesitant to go all in and let the agents do all the execution. They are hesitant to take their hand off the wheel because they don’t have full confidence the agents have the right guardrails in place to run off on their own.

## A week at the whiteboard

Recently, Giles Alexander, our Regional CTO for Europe, brought senior engineers, infrastructure consultants and product people together for a week of working in exactly this way. No code written by anyone. Everything built by agents. The point wasn’t to prove that an airline application could be built in a week. (Funnily enough they got 80% done and we all know what that means… you can’t make stuff up!) The purpose of the experiment was to learn what happens to the team’s work when agents do all of the coding. You can read some of what they learned [here](https://martinfowler.com/articles/exploring-gen-ai/an-accidental-blackboard.html).

What struck me in our conversations was how much time they spent at the whiteboard. Working out the domain. Drawing boundaries. Deciding what should happen next and making plans the agents could act on. Meanwhile, the agents kept producing code beyond the team’s working day.

The humans were also designing the feedback around that work: tests, contracts, architecture checks and a delivery pipeline. They were working out how to give agents enough freedom to make progress while keeping confidence in the result. Most of the work at the keyboard was directing agents to put this learning into the harness, so the building and testing agents got it right on the next run and they could have more confidence in what they built. Interestingly in the end the product person committed more feature code than anyone else on the team.

When I asked them to estimate they said they spend about ⅔ of their time at the whiteboard and ⅓ on the harness, which could give us some clue as to how developers will work in the future, but before you get ahead of yourself redesigning your team, that’s not the end of the story…

## The application ran. The build was red.

The application ran, but the build was frequently red. It worked on their machine - again you couldn’t make this up if you tried! A constant red build gives me the ick. A red build should mean something: stop, investigate, fix it. If it’s red so often that people stop paying attention, how do we spot the next real failure? And if we can’t trust the checks, a green build stops being reassuring too. That’s exactly the uncertainty [continuous integration](https://martinfowler.com/articles/continuousIntegration.html) was supposed to remove, and we’ve been advocating it for over 25 years.

We’re still working through how to address it. Remember this was an experiment to push the very edge of agentic development. Were too many agents working in parallel and stepping on each other’s changes? Was the system not yet modular enough for agents to work on separate parts? Or were changes simply arriving faster than the checks could finish? Or was it a combination of all of these and more? Either way we definitely need to rethink how we will solve the last mile challenges. I can see continuous delivery/DevOps as a movement having a resurgence and a rethink in the agentic era in the same way the rest of the development life cycle is being upended. And it isn’t just our experiment. [DORA’s 2025](https://cloud.google.com/blog/products/ai-machine-learning/announcing-the-2025-dora-report?utm_source=chatgpt.com) research found AI adoption is now linked to higher delivery throughput, but to lower delivery stability. Exactly what continuous delivery and DevOps were trying to solve for!

All of these challenges prompted some good arguments internally. What role should test-driven development play when you aren’t reading every line and the agent created the tests alongside the code? Where would acceptance tests, contract tests and checks at system boundaries give us more confidence without being too brittle? How much design should we do before setting an agent loose, given how quickly we can revise an approach? Is there an appropriate point we will know we have gone too far? One of the biggest puzzles in all this is the size of the batch. I have heard many teams are moving from Stories to Epics. Great for speed of development but can your CI pipeline handle that size unit of work at speed and in parallel?

The experiment team didn’t settle all of this in a week or even two (they ran it twice with different teams). I wouldn’t trust anyone who claimed they had. But this is exactly the kind of work I want experienced developers involved in: experimenting, questioning and helping us discover which practices hold up and where we need some new ones.

## Off the groomed slopes

After coming away from this conversation with some of the senior developers involved I kept thinking about learning to ski. Kids will often head straight down the hill without fear, kind of like a new builder who has never put software into production and has never fixed an on-call issue at 3am (something I will never forget, right up there with every production issue I personally caused and the resultant learnings from those, which are burned in my brain forever). When I first tried to ski as an adult, I was thinking about what I might break. I wanted to ski, it looked so fun and freeing, but I was old enough to see the consequences and I didn’t fancy hobbling around London with a broken limb or worse. My biggest issue was I hadn’t yet learned how to turn or stop effectively and without that I was a runaway train on a ski slope.

When I shared a draft of this post with Martin, who also learned to ski late, he pointed out where the comparison breaks down (like all analogies I like to push them to the edge, pun intended). On a ski slope you’ve watched countless people reach the bottom with no broken bones. The instructors have taught thousands of people before you on the same slope. They know which skills matter and how to teach them. Agentic development right now is more like a backcountry trip across unknown territory. We don’t yet know which skills will matter most. Not many people, if any, have finished the trip safely and the feedback on how to navigate it is slow or non-existent. As Martin put it, we won’t know whether a heavily agent-built system works until it has run in production for a while, survived a few rounds of change, and not sent all our planes to Aberystwyth. **Built is not the same as working in production.** Or as Charity Majors [puts it](https://x.com/mipsytipsy/status/1822877010894758096), “staging is just a glorified laptop. Only prod is prod”. Which is why when someone says “it works on my machine” it’s usually intended as a joke. Or at least I always laugh.

That solidified a nagging discomfort I have had with leaders, technical or non-technical, claiming devs aren’t curious, or are laggards slow to adopt, and putting that down to easy-to-point-at fears like missing writing code (I do too) and fearing for our jobs (who isn’t right now?). Our best developers aren’t nervous beginners on a nursery slope that everyone else has already mastered. They’re looking at unmarked terrain and asking sensible questions about the route. Telling them to be less cautious misses the point. They need to be on the team that maps it.

Of course, experience can also make us too attached to familiar practices. Some of what we learned on the groomed slopes won’t hold out there, and we need to be willing to test our assumptions.

## A challenge for both sides

So there’s a challenge here for both leaders and developers. Leaders need to listen carefully enough to tell useful concerns from reluctance to change, and to create real room for experimentation. That means time, permission to get things wrong, and not judging success by how much code the agents produced. Developers need to bring their experience into those experiments and be prepared to find that some of their assumptions no longer hold.

Some familiar practices will become more important. Others will need a different shape. Nobody is going to hand us a map first.

If you care deeply about building the right thing and building it right, your judgment is exactly what this new way of working needs. Bring the caution. Then let’s set off into the snow.
