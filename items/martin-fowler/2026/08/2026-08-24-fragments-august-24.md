---
title: 'Fragments: August 24'
link: https://martinfowler.com/fragments/2026-08-24.html
source: martin-fowler
published: 2026-08-24T15:29:00Z
updated: 2026-08-24T15:29:00Z
first_seen: 2026-09-04T09:21:49.209340827Z
authors:
- Martin Fowler
content: extracted
html: 2026-08-24-fragments-august-24.html
---

I was listening to [Ezra Klein’s interview with Helen Toner](https://www.nytimes.com/2026/08/18/opinion/ezra-klein-podcast-helen-toner.html) about the recent OpenAI hack of Hugging Face and the subsequent discovery that there were swarms of agents inside OpenAI doing unsanctioned activities. One of the points Klein made was that at no point did any of these (thousands of?) agents ever try to check in with a human

> \[Klein:\] So these message boards — you have however many A.I. agents posting hundreds of thousands of messages. At no point do they say: Hey, researchers, programmers, parents at OpenAI, Anthropic — do you want us coordinating with each other on this message board we have created in the innards of your systems?

> \[Toner:\] Or even F.Y.I., we have a message board we’re coordinating on in the innards of your system.

Listening to that, another thing occurred to me - *none of these agents thought to rat the others out*. No “hey, some of the agents in here are doing sketchy things”, no sign of an AI whistleblower.

 ❄                ❄                ❄                ❄                ❄

Is the AI bubble so big that the frontier companies like OpenAI and Anthropic have no way of becoming a viable business? If that’s the case, Bruce Schneier and Nathan Sanders [have a possible path:](https://www.schneier.com/blog/archives/2026/08/if-the-markets-reject-openai-and-anthropic-the-us-should-nationalize-them.html)

> Evidence suggests the market itself could reassess that these companies offer nothing of financial value. In that case, perhaps we can return them both to their original purposes. If these AI companies should fail in the financial markets, the US should nationalize them and convert them into national labs operated under democratic control that preserve their benefit to the public interest.

Such an idea may strike many people, used to the laissez-faire free enterprise world of Silicon Valley, as sacrilege, disaster, even socialism. But the United States made world-beating technological progress through such institutions in the recent past. AT&T was a quasi-government entity that led the world in telecommunications and electronics after the second world war.

> The US has a long, successful history of these kinds of institutions, which have produced world-shaping innovations in spaceflight, telecommunications, nuclear power and more. Congress currently manages a $200bn R&D portfolio, within which frontier AI development is, arguably, a glaring gap.

 ❄                ❄                ❄                ❄                ❄

Here’s a message for those readers who live in Massachusetts, just to the north of me, specifically in congressional district MA-06. I don’t usually endorse political candidates, but I’ve made an exception for [Beth Anders-Beck](https://bethfordemocracy.com/), who is running for that house district. I’ve known Beth for many years and have a high opinion of her smarts, wisdom, and compassion. They would make an excellent member of congress.

 ❄                ❄                ❄                ❄                ❄

Kevlin Henney posts “one weird trick” for [deciding when to skip reading LinkedIn posts](https://kevlinhenney.medium.com/streamline-your-linkedin-experience-with-this-one-weird-trick-041daa16777a), essentially by identifying a common pattern for skippable posts:

1. Post is too long
2. Contains a (crummy) info-graphic
3. No voice of poster (instead “aspiring anodyne anonymity”

It seems like a good approach. I, however, have a simpler one - skip all LinkedIn posts.

 ❄                ❄                ❄                ❄                ❄

Bartosz Ocytko has detailed and thoughtful post about the usage of [agentic programming at Zalando](https://engineering.zalando.com/posts/2026/08/agentic-engineering-at-zalando-a-snapshot.html). Like most companies I hear from, they are convinced of the value of agentic programming but still exploring how best to do it. One notable step they’ve taken is building platforms to act as a clear portal for API access and tools to support chat UI and CLI. This allows them better support good security practices and to monitor usage of models.

They have seen signs of agentic programming increasing the complexity of codebases, including leading to larger commit messages.

The write-up spends a lot of time on knowledge sharing, how to pass on skills, and the support of experiments.

> With >200 teams innovating and broadly exploring the ecosystem, the question arises whether and when to converge. We believe it’s way too early for this. While agentic engineering practices are still in their early stages, our key objective is transparency and exchange across teams.

I was struck by their use of an LLM to assess the risk of pull-requests. Those with a low risk of rollout can be auto-approved, reducing lead time by 20-40%. An interesting consequence of this is that it encouraged folks to split pull-requests so low risk portions can take advantage of the fast approval. Any changes to configurations are automatically made high-risk, which they feel protects them from common outage traps.

They repeat the common thread that the value of AI depends greatly on underlying skills.

> Like anyone in the industry we observe how AI amplifies the good and bad practices across our organization. Teams that get carried away with agentic engineering end up with large PRs that discourage reviewers and slow down delivery until a team adjusts their practices.

 ❄                ❄                ❄                ❄                ❄

Julia Curlee was a senior intelligence official in the White House. She had served under administrations of both parties, been the briefer for Vice President Pence, and on the National Security Council under Biden. She writes an absorbing account of her relationship with Pence and shares observations about the changes to the intelligence community under the current administration, including [recent events at the CIA](https://www.theatlantic.com/magazine/2026/10/trump-white-house-transgender-mike-pence/688284/?gift=zGsHlQiVhVhk3cFVqv--gzIqOYZAxuFeY4CtPlqqfx4&utm_source=copy-link&utm_medium=social&utm_campaign=share) (gift link)

> The agency has been gutted as part of a deliberate plan, the director of the Office of Management and Budget once boasted, to put the people who defend our country “in trauma.” Analysts have been fired in public or questioned by the FBI; decade-old assessments have been denounced by the CIA director in the press. The president calls analysis “virtual treason” when it contradicts his preferred reality, and uses the CIA to undermine public confidence in American elections.
>
> Fear has done its work. Irreplaceable officers with crucial language and technical skills, and decades of experience, have walked out the door. Those who remain within an agency built to deliver hard truths are being muzzled.

For a worthwhile sample of her analysis, read this evaluation of the [current bargaining between the US and Iran](https://www.lawfaremedia.org/article/fighting-while-talking--the-iran-war-enters-its-bargaining-phase)

> Most wars do not end in “unconditional surrender.” They end when both sides accept terms. Paul Pillar’s classic study of war termination, “Negotiating Peace,” treats combat and diplomacy as a single process: Each side fights to improve the terms it can demand at the table, and talks to lock in what the fighting has won.

She continued to serve the second Trump administration even though they knew she was trans, until her position was made public.

Autocrats seem appealing, with the promise to get things done without the ponderous constraints of rule of law or bureaucratic procedure. There are occasional “Good Emperors” who raise people based on merit, but more often such power attracts corruption, nepotism, and toadies.

> Flailing regimes dehumanize minorities to distract from their failures. When the economy collapses or a war goes badly, they find a tiny group of people, make them the enemy within, and rally the country against them. This is how it’s gone in Iran. Hungary. Russia. I wrote PDBs about it. This will not stop with trans people. It never has.
