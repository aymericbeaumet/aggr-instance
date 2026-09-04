---
title: 'Fragments: May 27'
link: https://martinfowler.com/fragments/2026-05-27.html
source: martinfowler-com
published: 2026-05-27T19:40:00Z
updated: 2026-05-27T19:40:00Z
first_seen: 2026-09-04T12:14:41.802665207Z
authors:
- Martin Fowler
content: extracted
html: 2026-05-27-fragments-may-27.html
---

At the GOTO Conference in Copenhagen in 2025, [Kent Beck and I spent some time on stage](https://www.youtube.com/watch?v=ii_rLjQfjp0&list=PLEx5khR4g7PINwOsYrkwz3lTTJUYoXC53) talking and answering questions from the audience - a format I refer to as “two old geezers on a park bench”. We talk about our experiences with LLM-augmented programming (at that point - October 2025), we show our frustration that things we’ve been saying for thirty years still need to be said, we say how anything like a manifesto reunion needs to be led by a younger generation, and opine on what junior developers should be focusing on in their career.

[![](https://martinfowler.com/img/catalog/2025-goto-kent.png)](https://www.youtube.com/watch?v=ii_rLjQfjp0&list=PLEx5khR4g7PINwOsYrkwz3lTTJUYoXC53)

 ❄                ❄                ❄                ❄                ❄

Ian Johnson has written a series of posts about [restructuring a gnarly codebase](https://dev.to/tacoda/the-agent-harness-turning-ai-slop-into-shipping-software-589i)

> The story follows a real Laravel + React codebase over ~3 months and ~258 commits from a legacy monolith with no tests to a well-structured application with automated quality gates, a React SPA migration in progress, and an AI agent that reliably ships production code with minimal supervision.

The series covers the steps in decent detail, and his approach follows the kinds of steps I’d use. First get everything under the control of decent characterization tests, add static analysis, introduce the right patterns to make things flow easily.

With all of this, is his use of AI, which changed during the exercise:

> For the first two months of this project, I used Claude Code with auto-approve turned off. Every file edit, every terminal command, every change… I reviewed it before it executed. \[…\] The results were good. The code was clean. But I was doing most of the thinking and half the typing. The agent was a fancy autocomplete with better suggestions. I wasn’t getting the leverage I’d hoped for.
>
> I read an article about “on-the-loop” versus “in-the-loop” human-AI collaboration. The framing clicked immediately \[…\] I was micromanaging because I didn’t trust the agent to do the right thing. And I didn’t trust the agent because there was nothing forcing it to do the right thing.

His early steps put in tests, static analysis, and the right architectural patterns. With those in place, he could let the agent do more work.

> My role shifted from writer to curator. I don’t write most of the code anymore. I Define the patterns \[…\] Review the test specs \[…\] Review the output \[…\] Update the harness \[…\] Make strategic decisions \[…\]

He finishes the series with conclusions about how he’d generalize his experience to other circumstances.

 ❄                ❄                ❄                ❄                ❄

Back in the land of my birth, there was some notable groans when the National Health Service decided to [close nearly all of their Open Source repositories](https://shkspr.mobi/blog/2026/05/nhs-goes-to-war-against-open-source/), supposedly to the security threat of LLMs. Closing repos like this isn’t an effective counter to LLM-augmented attackers. I suspect it’s no coincidence to see GDS (Government Data Services), the highly-regarded IT enablers in the UK government [publish their position](https://www.gov.uk/guidance/ai-open-code-and-vulnerability-risk-in-the-public-sector)

> Moving code from public to private as a substitute for investment in secure-by-design delivery, ownership and remediation is a warning sign because it reduces sharing and scrutiny, can slow coordinated improvement across government and suppliers, and does not remove the underlying weaknesses in a running service.

[Terence Eden](https://shkspr.mobi/blog/2026/05/gds-weighs-in-on-the-nhss-decision-to-retreat-from-open-source/) memorably sums up his view on this:

> Within the UK’s Civil Service you occasionally hear the expression “being invited to a meeting without biscuits”. It implies a rather frosty discussion without any of the polite niceties of a normal meeting.

 ❄                ❄                ❄                ❄                ❄

I’ve seen a few cases where those developers who are most involved in working with LLMs find they are running into a problem with cognitive endurance, [Adam Tornhill has joined this group](https://adamtornhill.substack.com/p/compressed-cognition-the-hidden-cost):

> One of the big wins with agents is that they let us stay with the higher-level problem for longer. We get less sidetracked by details, dependency cleanup, and similar secondary tasks that used to break concentration.
>
> But there is a cost we are still underestimating. Agentic coding is mentally expensive.
>
> I can usually sustain the pace for a couple of hours. Then I need a break. The pace is simply too intense. And based on conversations with other engineers, I do not think I am alone in that.

He explains that working with The Genie means we are making more decisions in less time, this increase in decision density is hard on the brain.

He responds by keeping agent tasks small, automating everything he can, and accepting that he won’t know every line of code as long as he has good verification mechanisms in place.

Notably, he has not gone in the direction of doing his work with swarms of agents that he coordinates. Instead has one long-running task that he babysits and one focus task

> That last point is important given the running-twenty-agents-in-parallel hype. I cannot even think about twenty meaningful things to build, and even less so about the resulting cognitive tax of the likely interruptions. It’s exactly the wrong thing to even consider. At least for humans. (And yes, I understand sub-agents and machine parallelisation. That is not what I’m objecting to. It is the parallelisation of human attention that does not scale).

I liked that he included some thoughts about what folks can do in time outside this intense programming time. Not just “have a coffee” (although he includes that) but also about learning about the domain that the software supports.

 ❄                ❄                ❄                ❄                ❄

A couple of pithy quotes from social media

[Lorin Hochstein](https://toot.thoughtworks.com/@norootcause@hachyderm.io/116626163784445316)

> “Metaphor debt” is when all of your metaphors involve the concept of “debt” because you can’t think of any other metaphors anymore.

 ❄                ❄

[Daniel Terhorst-North](https://toot.thoughtworks.com/@tastapod@mas.to/116641666908831725)

> If a vegan crossfit fan is using Claude to write Rust, which thing do they tell you first?

 ❄                ❄                ❄                ❄                ❄

Karl Bode reacts to speakers getting booed when mentioning AI during commencement addresses. He points out that younger folks are increasingly [unhappy with the tech oligarchy and their fruits](https://karlbode.com/anger-at-ai-is-inextricably-fused-with-justified-loathing-of-the-extraction-class-deal-with-it/).

> The thing is the kids aren’t stupid. They see the field clearly. They see the difference between what’s being sold to them by tech companies, the press, and commencement speakers, and what they have repeatedly seen with their own eyes.
>
> They’ve watched tech oligarchs spend the last decade mired in scandal after scandal, hype cycle after hype cycle, steadily enshittifying everything they touch along the way.
>
> \[…\]
>
> The percentage of Gen Z that think AI’s benefits don’t counterbalance the risks now sits around fifty percent, up 11 percentage points in just the last year. Eight out of every ten believe that using AI makes the process of actual learning more difficult.

He sees young people saddled with the perception of entering a worsening world - which leads them to rage against this latest fruit of the tech oligarchy. A rage that is easy for folks like me

- with a comfortable retirement off-ramp - to properly appreciate. A rage that could have marked political and social consequences.

 ❄                ❄                ❄                ❄                ❄

Relevant to these concerns are a couple of items in last week’s Economist newspaper. The newspaper argues that historically major technological advances haven’t led to significant unemployment or drops in wages ([paywalled article](https://www.economist.com/finance-and-economics/2026/05/14/the-jobs-apocalypse-a-very-short-history)). The closest was the original industrial revolution in 19th Century Britain. There was a stagnation in wages during this period, but there was also a massive increase in population, from 4½ million to 12 million.

It also points out that we’ll probably only understand the full consequences of all this when a recession hits, as this is when most unproductive jobs tend to be flushed out of the system.

A second article ([also paywalled](https://www.economist.com/finance-and-economics/2026/05/13/is-ai-putting-graduates-out-of-work-already)) indicates that AI is having some effect on graduate hiring. They did an analysis of surveys of recent graduates, looking to see if employment varied depending on a job’s exposure to AI. The least exposed quintile of subjects saw employment rate fall by 1.5% over the last couple of years, while the most exposed quintile’s drop was 6.6%.

 ❄                ❄                ❄                ❄                ❄

[Lawfare isn’t impressed](https://www.lawfaremedia.org/article/ai-governance-by-phone-call) with the latest efforts by the US Government to regulate AI.

> On \[last\] Wednesday, the White House invited leaders of OpenAI, Google, Anthropic, Meta, and Microsoft to the Oval Office for a signing ceremony the following afternoon. President Trump was to sign an executive order on AI and cybersecurity—the administration’s most formal effort yet to establish a voluntary process for reviewing frontier models before their release. But roughly three hours before the ceremony, when some company executives were already in the air to Washington, the White House called it off.

They see the proposed regulations as mild, and including some valuable measures to harden defenses against cyber threats.

> But it’s worth underscoring the implications of postponing (if not outright canceling) this order, which, by its own terms, was about as modest a frontier-AI intervention as the federal government could put on paper: voluntary, focused on the government’s own defenses, and explicitly barred from becoming a licensing regime. The objection isn’t so much about government coercion as about the government having any settled role at all. Voluntary, in other words, isn’t the floor of frontier AI policy in this administration; it’s the ceiling.
>
> This is a questionable position given that the concerns animating this draft order will likely grow in the near future. It is also self-defeating for those who applauded the order’s delay or demise. Far from resolving the risk of government meddling in AI, killing the order just leaves in place what Ball has described as the “opaque and essentially lawless” alternative: government access happening through back channels, on terms set case by case, with no stable rules at all.

One of the problems here is a distinct lack of governmental expertise, either in AI or in software in general. Too much is being decided at the whims of the tech oligarchy, there isn’t any attempt to engage in the broader issues at hand. That’s not entirely a bad thing, trying to regulate something that’s still evolving so fast is usually a fool’s errand - but the problem here is the impact of AI is so big that there’s real danger in being too far behind.

 ❄                ❄

Which leads me to a rare thing, an endorsement of a candidate for political office. If you are voting in congressional district MA-06 (North Shore of Massachusetts), I’d seriously look at [Beth Anders-Beck](https://bethfordemocracy.com/), who is running for congress in that district. Beth has a long background in software development (including developing the notion of [Forest and Desert](https://martinfowler.com/bliki/ForestAndDesert.html)), so would introduce expertise that Congress desperately needs. I’ve known Beth for decades, and have a high opinion of their intelligence, judgment, and ability to work with others. Congress doesn’t deserve Beth, but it does need her.
