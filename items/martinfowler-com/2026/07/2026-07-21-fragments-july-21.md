---
title: 'Fragments: July 21'
link: https://martinfowler.com/fragments/2026-07-21.html
source: martinfowler-com
published: 2026-07-21T13:13:00Z
updated: 2026-07-21T13:13:00Z
first_seen: 2026-09-04T12:14:41.802665207Z
authors:
- Martin Fowler
content: extracted
html: 2026-07-21-fragments-july-21.html
---

With this post, I’ll wrap up my notes from the second [Future of Software Development Retreat](https://martinfowler.com/bliki/FutureOfSoftwareDevelopment.html). But before I do, I should note that the [full Thoughtworks report on the retreat is now available](https://www.thoughtworks.com/content/dam/thoughtworks/documents/report/tw_future_of_software_engineering_europe_2026.pdf). They have five headline findings:

> - Code generation is no longer the bottleneck — verification is.
> - ‘Harness engineering’ is emerging as a distinct, ownable discipline.
> - Organizations are colliding with a real apprenticeship crisis.
> - The executive/engineer expectation gap is a bigger risk than any technical limitation.
> - Legacy modernization is the clearest, most defensible near-term value pool.

 ❄                ❄

A session convened around the mismatch of views about using LLMs between engineers using it and the C-suite and boards that were calling for it. The concern is that boards are looking at promised productivity gains, and not concerned enough about the risks, particularly about security.

This was illustrated by one tale of a company that used ML-trained software to optimize the replacement of air filters on their field equipment. They were pleased to see that they were able to change the air filters less frequently, saving them $50 million. But the problem was the ML models were trained on equipment used in the desert, while their equipment was used in the arctic. Air filters in the desert deal with dust, but in the arctic the thing to remove is mosquitoes. There’s an important difference here, mosquitoes rot, and enough decaying mosquitoes is a serious fire risk. Fires from such dead mosquitoes around infrequently replaced air filters cost the company $100 *billion*.

Now such a tale could told of many situations without AI in the mix. Plenty of human situations have gone wrong when solutions are applied in a new context (which is why context is such a key word among pattern-writers). But the tale does remind us to be wary of an AI’s suggestions, and to always think of how to build sensors to provide rapid feedback.

Engineers particularly worry about the risks when citizen developers start [vibe coding](https://martinfowler.com/bliki/VibeCoding.html). In many ways, of course, this isn’t new. I.T. folks often worry about how many important business decisions are based on spreadsheets, that are built with little control, testing, or assessment of data quality. Vibe-coding amplifies these concerns, so companies need [a range of controls to guard against security breaches](https://martinfowler.com/articles/vibesec-reckoning.html). Some folks have made a point of raising issues at board level, running threat modeling session with board members to introduce them to the risks. Vibe-coded applications need to be put in separate infrastructure, which deterministic controls over data access to tame the [lethal trifecta](https://martinfowler.com/articles/agentic-ai-security.html#lethal-trifecta). One company encouraged widespread vibe-coding from citizen developers but recoiled from the problems of the huge shadow IT that emerged - they are now looking to build a platform to help control this work without stifling the useful tools that were produced.

Part of the problem here may be simple experience with LLMs. Many in management find LLMs do a decent job of preparing management reports. Or summarizing management reports prepared by other LLMs. Given this they naturally think LLMs must do a decent job of programming too. My anti-management self has to mention [Kelsey Hightower’s observation:](https://bsky.app/profile/kelseyhightower.com/post/3mptigbdzjk2k)

> The less busy work you have the less appealing these Al tools are

One possible antidote to this: get the legal department involved. They see LLMs doing a poor job, and appreciate the risks involved.

 ❄                ❄

Most folks I talk to, both at the retreat and outside, recognize we are in some form of bubble. Technological advances like this almost always come with economic bubbles, and in the future we will all look back at this, and shake our heads saying we knew there was so much froth. But while it’s easy to see that there is a bubble, it’s hard to see how long it will run or what will emerge after the pop. After all the dotcom bubble was clearly recognized as such… in 1995. We can happily point at those companies that failed (Webvan, pets.com) but need to then acknowledge those that survived (Amazon).

Most of those at the retreat were old enough to have lived through the dotcom bubble and crash, but one such grey-hair pointed out an interesting difference. Back then we were excited about what the future would bring, and we saw lots of new things being built. There’s much less of that, this time around. [Most people are wary](https://poll.qu.edu/poll-release?releaseid=3955) of what the AI bubble is creating. Partly this may stem from the reality that followed the dotcom hope. Social media may be everywhere, but do we think it’s actually improved our lives that much, even if (especially if?) we use so much of it?

We hear so much about the incredibly productive things we can do with [agentic programming](https://martinfowler.com/bliki/AgenticProgramming.html), but has anyone noticed a flood of wonderful applications built with it? Or have we noticed a significant improvement in common applications from the big AI boosters such as Google or Microsoft?

This may be another factor in the board-vs-engineer divide. Most of what’s driving adoption of AI at the moment is cost-cutting, and it mostly the boards that get excited by cost-cutting. Perhaps the [increasing concerns about token costs](https://martinfowler.com/fragments/2026-07-06.html#token-costs) will temper the eagerness.

 ❄                ❄

Folks are finding LLMs helpful in operations: with a good event stream from observability tools, an agent finds anomalies much faster. One of the problems with citizen-developer apps, is that they often don’t provide good observability, since the citizen-developers don’t think to ask for it. The agents ability to look at the event stream does pose governance questions, as often such event streams contain a lot of sensitive information.

Reinforcing what I’d heard in Utah, more people agreed that LLMs are valuable for operations folks to help them understand what the code does. Cross matching code and event traces helps them assist humans to find what happened when things go wrong. Agents are particularly handy with repeated incidents, as they can collate lots of information from different cases and present it to the human teams.

Getting agents to auto-remediate moves us to the next level of capabilities and concerns. It’s vital that agents carefully document all their actions when they do fixes. We also need to ensure there is feedback to the development team so they can learn. Agents don’t learn, the best they can do is update the context.

There was a sense that many people over-estimate the capability of agents to deal with incidents. Such people think of incident resolution as a simple, linear process. But it’s rarely that, instead there’s a lot of surprises and adaptation needed. Humans are good with that, but LLMs are not.

One of the perils of agent-developed code is their habit of inserting features that were never asked for. One team spent three days trying to figure out such an unrequested feature, trying to figure out who had requested it and if anyone wanted to keep it.

 ❄                ❄                ❄                ❄                ❄

A group of law professors carried an interesting experiment to judge [how well an LLM can provide short answers to student questions](https://law.stanford.edu/publications/law-professors-prefer-ai-over-peer-answers/). They created a batch of forty questions in contract law and asked the professors, plus a couple of LLMs, to provide answers. To evaluate the LLM answers they showed professors pairs of answers - one human, one LLM - and asked them which response they would prefer to deliver to a student.

> Professors rated LLMs far higher than their peers (average win rate = 75.33%), with models performing similarly to the best instructor. LLM responses were also rarely flagged as harmful (3.53%, vs 12.06% for professors).

This reminds me of the distinction I mentioned in a recent fragment between [interactional and contributory expertise](https://backofmind.substack.com/p/faking-versus-making).

 ❄                ❄                ❄                ❄                ❄

A few days ago Unmesh Joshi published an article here about his experiences [using DSLs to enable more reliable use of LLMs](https://martinfowler.com/articles/llm-and-dsls.html). Responses to this included a pointer to an article by Spender Nelson that [related similar impressions](https://blog.firetiger.com/custom-programming-languages-make-agents-really-really-smart/).

> DSLs like this hit a lot of sweet spots for LLMs. You can make them extremely token efficient, and enforce hard security boundaries. You can translate high-level LLM intent into a ton of deterministic code, ensuring good behavior and guardrails at the (custom) compiler level.
>
> And Large Language Models are very good at learning and working with DSLs. Maybe this shouldn’t come as a surprise; they are language models after all. A small bit of documentation generally is enough to set them off and running, and reasonable error messages let them course-correct even when they go wrong.

He describes a couple of examples from their use: a query language for data lakes that takes into account security and authorization issues, and a little expression language to make it easier to create safe SQL where clauses.

One of the biggest barriers to using DSLs, particularly [external DSLs](https://martinfowler.com/dsl.html), is building a parser and tooling. LLMs make this much easier. That said, my sense is that it’s the semantic model that underpins the DSL is what really matters, and the DSL is one projection of that model. LLMs may help us explore other ways to project that model in interesting ways.

 ❄                ❄                ❄                ❄                ❄

In recent weeks I’ve been noticing the stench of LLM-speak more and more. It’s not just the common tells, it’s a sense of LLM miasma that pervades the prose. I’ve noticed it’s increasingly eliciting a visceral reaction, after a couple of paragraphs I just want to dismiss the entire article out of hand. For some of these, it was necessary for me to hold my nose and wade through the whole text, but it was with an intellectual nausea which obscured the content, even increasing my desire to indulge in such an awful distraction as checking social media.

I wonder - is this just me that’s reacting so negatively to LLM-speak? Or do other people have a reaction that leads them to toss aside any prose that sets off their LLM-alarm?

One indicator that it’s not just me is this post from Jason Koebler that I highlighted a couple of months ago, where he [observed how AI was breaking his brain](https://www.404media.co/your-ai-use-is-breaking-my-brain/):

> People think things that are fake are real, things that are real are fake. Much has been written about “AI psychosis,” the nonspecific, nonscientific diagnosis given to people who have lost themselves to AI. Less has been said about the cognitive load of what other people’s AI use is doing to the rest of us, and the insidious nature of having to navigate an internet and a world where lazy AI has infiltrated everything. Our brains are now performing untold numbers of calculations per day: Is this AI? Do I care if it’s AI? Why does this sound or look or read so weird? Does this person just write like this? Is this a person at all?

A while ago, I was thinking that it was reasonable for folks who aren’t as committed to writing as I am to use an AI to help polish their prose. Now I’m turning to encouraging writers to reject it. That pervasive LLM-voice is just so common now, my sense is that it discredits the writing even before the reader has a chance to try to understand what is being said. I don’t think it’s good enough to ask the LLM to write a first draft and then tweak it. I’m not sure writers can edit the LLM-ness out of prose once it’s in there. I even worry about asking an LLM to suggest improvements, I think it’s just too easy to accept an LLM’s suggestions, and in the process trigger your readers’ LLM-antibodies.

Of course like most problems, it’s also an opportunity. Those who can get a distinctive human voice will get more visibility and credibility. But the question remains of how we can coach people to let out their true personality into their writing. Academic and corporate writing both tended to stifle engaging prose, LLMs are good amplifiers, and they will amplify this stifling. This is an even greater challenge for those for whom English is their second language (or indeed for many of my colleagues, their third or fourth). It’s too easy for me to neglect to think about a difficulty that I’ve never been able to face.

The most immediate advice I can give something I learned many years ago and shared last year - [Say Your Writing](https://martinfowler.com/bliki/SayYourWriting.html).

> Once you’ve got a reasonable draft, read it out loud. By doing this you’ll find bits that don’t sound right, and need to fix.

I always suggested this to help people get past sluggish prose, especially if they had spent too much time around academic or corporate writing. But now I think the need to Say Your Writing is even more important, in order to combat the insidious impact of AI. For most people, their speech patterns get closer to their real self, so verbalizing writing is the way to fight those forces that try to smooth away a writer’s individuality.
