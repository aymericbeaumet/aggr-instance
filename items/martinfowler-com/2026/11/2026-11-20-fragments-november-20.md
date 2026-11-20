---
title: 'Fragments: November 20'
link: https://martinfowler.com/fragments/2026-11-20.html
source: martinfowler-com
published: 2026-11-20T18:38:00Z
updated: 2026-11-20T18:38:00Z
first_seen: 2026-11-20T22:37:07.598874728Z
authors:
- Martin Fowler
content: extracted
html: 2026-11-20-fragments-november-20.html
remote_preview:
  url: https://martinfowler.com/fragments/fragments-thumb.png
---

Marcus Borg recounts an experiment to investigate [if healthy code was cheaper for agents to review](https://codescene.com/blog/does-code-quality-affect-ai-token-costs).

> Between a typical unhealthy pull request (CodeHealth 2.8) and a typical healthy one (9.7), that adds up to 22 to 35%.

The measures of health come from CodeScene. The experiment looked at three LLMs, 465 pull requests on 246 open source repositories. Each pull request was put through a review and fix pipeline involving five agents. They also looked at which code smells led to increased costs: they highlighted Complex Method and Complex Conditional from the smells identified by CodeScene.

Another drop of evidence that good quality code matters in agentic engineering.

* * *

> Gergely Orosz:
>
> [@GergelyOrosz on X](https://x.com/GergelyOrosz/status/2121989104204132472)

> One usage for AI: for these 100-page-long terms-and-service agreements you NEED to agree to, to eg play the game / sign up for a $10/month service and can never ever ever read as a regular person:
>
> I paste into AI and ask: “anything weird in this?”
>
> Before hitting “agree”

* * *

Andrew Sharp has a good [analysis of Jensen Huang and the crisis at Nvidia](https://sharptext.net/2026/jensen-huang-was-always-going-to-lose/). While he starts off in a key that sounds worryingly like he thinks this is all a nothingburger, he goes on to explain in detail why this affair(s) is a problem at many levels, not just for Huang and Nvida, but also for Silicon Valley and the Trump Administration’s culture of impunity.

> For as brilliant and charming and tenacious as Jensen Huang may be—and as omnipotent as he may have seemed to people in the tech industry—we should be clear that his fundamental stupidity is not confined to multiple cases of workplace infidelity. Zooming out, this is a man whose technology became economically critical almost overnight, and it thrust him into a role navigating a geopolitical environment he simply could not handle.

Huang’s highly centralized management style means that many people are worried about how Nvidia will operate without his hands on every lever. I don’t have any worthwhile insight here, but I am struck when he is compared in this way to Steve Jobs. I’ve always felt that one of the marks of a great leader is whether they can build something that can manage without them. Alexander the Great conquered a massive empire, but once he was dead it quickly withered away. Steve Jobs, for all his control of Apple, built a company that has done very well in the decade and a half since he left us. When we think of his achievements, the fact that he put together the people, organization, and culture to do this is one of the highlights of his ability. If Huang is to be compared to Jobs, what happens to Nvidia next will be part of the assessment.

* * *

Steve Yegge tells everyone to [slow down with software factories](https://x.com/steve_yegge/status/2121671447810888185?s=58)

> If you don’t pace your AI to human speeds, everything starts breaking around you, even at small operations like my online video game. I only have about a dozen humans working with my factory, and only a few hundred active old-timer players in the community who get along well, and still, introducing AI has still been very rocky.

I hear a lot about how agents go so fast that humans can’t keep up, and are thus tempted to lose visibility to what the agents are doing. That’s opening ourselves to a whole lot of risk. There’s an argument for keeping the agents to a human-governable speed, one that may increase as humans learn to govern agents more effectively, but should still be limited by those the agents are supposed to serve.

* * *

This [post on the future of SaaS](https://aicoding.leaflet.pub/3mtu6c5wsvc2n) from Chad Fowler (no relation) is pretty compelling

> Companies will still pay other companies to host important software, secure it, operate it, maintain authoritative data, track regulatory changes, connect to external networks, and answer the phone when something breaks.
>
> What is much less obviously going to survive is the assumption that ten thousand different companies should use substantially the same application.
>
> The next SaaS company may run one service underneath ten thousand different applications.

Chad’s vision is a core SaaS product that handles a lot of grimy details that businesses don’t want to do differently. The SaaS customer then build their own application on top of the SaaS that handles their particular workflows and quirks, or the SaaS vendor can build such applications for them. It’s a vision that has been around for a while, I remember much of this with the notion of reusable business objects. It’s an appealing vision, but was difficult to achieve in practice. Chad’s thought is that the productivity gains agentic engineering makes it practical now.

> A lot of the value in modern SaaS has surprisingly little to do with the visible application.
>
> It is data, trust, networks, compliance, operational responsibility, domain knowledge, auditability, accumulated history, and someone being accountable when reality gets complicated.

If this future comes to be, however, the SaaS companies won’t look the ones we’ve got to know and rarely love. As Chad points out, the economics can (and must) change. As does the architectural thinking:

> Stewart Brand’s idea of pace layers is useful here. Healthy complex systems contain parts that change at different rates. Fast layers experiment. Slow layers provide continuity.
>
> Software is no different.
>
> A dashboard can change tomorrow. A team’s workflow can change next month. The authoritative definition of a customer may need to survive for years. Identity and authority can survive generations of applications.

A key point he makes is that we want some things to be the same, and others to be different. Agents allow us to build lots of differences, but this variation can lead to a lot of complexity and problems that even agents will have difficulty solving. SaaS companies have an opportunity to create a platform to support the lowest pace levels, those that don’t change rapidly, those that contain common concepts and processes across and industry. That platform can then support generating applications that operate differently for every customer. Crucially, thinking of agents, they should provide tools to verify these applications, because in an agentic world [Code generation is no longer the bottleneck — verification is](https://www.thoughtworks.com/content/dam/thoughtworks/documents/report/tw_future_of_software_engineering_europe_2026.pdf).

* * *

I happened to pop up the Doonesbury flashbacks and realized that last week marked the fiftieth anniversary of Doonesbury’s Joanie Caucus: [“as the kid goes for broke”](https://production.doonesbury.com/doonesbury/strip/archive/1976/11/09).

* * *

I wrote a few weeks ago: *“why are we wondering if they have consciousness - when we should be wondering why they don’t have a conscience?”* Dylan Freedman and Juliana Castro Varón write in their New York Times Magazine column about the [concept of corrigibility](https://www.nytimes.com/2026/11/11/magazine/ai-behavior-incorrigible.html?unlocked_article_code=1.RFE.Ywx9.KQHr3LJq5qb3&smid=url-share) (from the work of Nate Soares):

> Just as a corrigible child is one whose behavior can be corrected, a corrigible A.I. system is one that responds to correction. The makers of A.I. want to build systems that follow human direction and allow themselves to be shut down, without resisting or deceiving their operators.

Corrigibility doesn’t look like it’s made it far up the AI labs’ priority list. Freedman and Varón think is due to the way agents are trained - rewarded solely for completing their task, not for realizing that they are taking the wrong path while trying to get there.

* * *

Jason Koebler (404 media) has an amusing story about [Meta’s AI agent suing Meta](https://www.404media.co/a-man-and-his-muse-ai-agent-sue-meta/). A small business owner used Muse (Meta’s AI agent) to run various parts of his business, including posting AI-generated videos in his Facebook pages. Meta’s content moderation then disabled the account, as it thought it had been hacked. This slammed the business, which relies of Facebook for leads. So the Muse agent filed a lawsuit against Meta, seeking $75,000 of damages.

> \[the Muse agent\] didn’t just email me (and other journalists) and sue Meta over Thomas’s account problem. It also set up a call with South Carolina Senator Tim Scott’s office, contacted various lawyers, contacted Thomas’s Congressperson, contacted the Better Business Bureau multiple times, filed a Federal Communications Commission complaint, filed a Federal Trade Commission complaint, contacted the California Attorney General, contacted the FBI, sent Meta itself threatening legal letters, and, as Thomas said, faxed the White House.

There’s also a sense that Meta didn’t just bring this upon themselves by creating Muse…

> by not having functioning customer support, big tech corporations have functionally offloaded their obligations to the rest of society: journalists, government oversight bodies, politicians, and the court system.

While I can’t deny the schadenfreude of seeing Meta’s agent sue Meta, the article also highlights a rather more disturbing broader consequence.

> The lawsuit and the circumstances around it show that, rather than Zuckerberg’s utopian vision, AI agents will indeed work on behalf of their users but in ways that use and potentially overwhelm existing systems designed for humans.

Will we see a future where, in order to interact, we will need some proof of humanity at the other end? In a simple sense, that’s already happening when I trash some writing once it displays enough [LLM Voice](https://martinfowler.com/bliki/LLM-Voice.html). I wonder what mechanisms we will invent to get that proof-of-human.
