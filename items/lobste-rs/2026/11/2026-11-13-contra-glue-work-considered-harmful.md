---
title: Contra "Glue work considered harmful"
link: https://sunshowers.io/posts/glue-work/
source: lobste-rs
published: 2026-11-13T02:36:05Z
updated: 2026-11-13T02:36:05Z
first_seen: 2026-11-16T07:11:22.159097445Z
authors:
- sunshowers.io via nrposner
labels:
- practices
summary: Comments
content: extracted
html: 2026-11-13-contra-glue-work-considered-harmful.html
remote_preview:
  url: https://sunshowers.io/images/og-image.png
---

Sean Goedecke, [*Glue work considered harmful*](https://www.seangoedecke.com/glue-work-considered-harmful/) (emphasis here, and in the rest of the quotes, in original):

> So why doesn’t glue work get you promoted, if it’s so crucial to shipping projects? Are companies stupid? Are they deliberately leaving value on the table? No, I don’t think so. Companies don’t reward glue work because **they don’t want you prioritizing it.** And they don’t want you prioritizing it because they want you shipping features. Glue work is hard. If you’re capable of doing glue work well, they want you to use that ability to deliver projects instead of improving general efficiency.

This is an argument to settle for mediocrity. If you’re like most people who have worked in big tech, it is plainly obvious to you that they *are* leaving value on the table. (Whether it is deliberate or not is a matter of what’s in people’s hearts.) What big tech employers want is largely incoherent. They want you delivering features. They also know that somebody’s got to do the glue work to keep the company running, and they have trouble describing or capturing its value.

Here, and throughout his post, Goedecke is quite accurately describing some of the incentives presented to employees at big tech. My problem with the post is that he tells you to internalize them instead of keeping in mind that a better world is possible.

> The core problem is that you’re deciding for yourself what the company needs instead of doing your job. Isn’t it your job to make your team run smoother? No! **Your job is to execute the mission of your company’s leadership.**

If glue work is critical to the mission of a company’s leadership, how does neglecting it serve the mission?

> It is better to execute that mission at 60% efficiency than to spend all your time increasing efficiency in general (or even worse, to execute some other mission at 100% efficiency). Why? For two main reasons: first, you’re inevitably going to burn out, which will be bad for everyone; second, it’s better to let your team get used to operating at the base efficiency level of the company instead of artificially removing friction for a brief period.

There are two very different things being conflated here: saving some gas in the tank for when there are real deadlines, which is great advice, versus not trying to make your coworkers’ lives better, which is a real case of misaligned incentives.

Besides, the premise is fundamentally wrong: there is a lot of glue work that, if you give it the respect it deserves, removes friction *permanently*. I’m obviously biased here, but I think the [cargo-nextest](https://nexte.st/) project is a great example of glue work (running tests!) done so well that the problem mostly vanishes. More recently, at Oxide I wrote an agent skill called [flake-review](https://github.com/oxidecomputer/omicron/blob/main/.claude/skills/flake-review/SKILL.md), backed by a [catalog](https://github.com/oxidecomputer/omicron/blob/main/docs/flake-patterns.adoc) I wrote[^1] over a few hours. I’ve since used the skill to fix almost a dozen test flakes, making the entire team’s lives better.

> Should you never do glue work? No, you should do glue work *tactically*. That is, you should do this kind of extra work for the projects you lead — the projects whose success you’re accountable for — in order to make sure they succeed. You won’t be rewarded for the glue work specifically, but you will be rewarded for the success of the project. For other projects, you should just do your regular job.

With the most emphatic of feelings, *fuck that*. I do glue work whenever I’m the best situated person to do the glue work. The idea that you shouldn’t help your peers, simply because someone in middle management has trouble articulating the value of helping your peers, should be deeply offensive to any serious professional.

> Is this a deeply cynical take about how to succeed in office politics? I don’t actually think so. Large tech companies operate at something like 20-60% efficiency at any given time (as they get larger, they get less efficient).

A large chunk of people’s working lives is wasted on solvable problems whose value organizations don’t recognize. How can one not be cynical about this kind of unnecessary waste and suffering? It baffles me.

* * *

So, what is going on here? Any organization beyond a certain size requires [legibility](https://en.wikipedia.org/wiki/Seeing_Like_a_State) to function. The requirement for legibility pulls in all the things many people recoil at: performance reviews, OKRs, KPIs and whatnot[^2].

Some big tech firms have figured out ways of making some of that glue work more legible. For example, Meta has fairly large internal tools and developer infrastructure teams, where goals are set around measurable indicators like performance numbers. But there will always be some glue work that will resist all attempts to measure it, and to the extent that firms don’t care about it, they will suffer.

What does one do instead? Well, [getting rid of performance reviews](https://oxide.computer/blog/oxides-compensation-model-how-is-it-going) certainly helps! For the last few years of my work life at Oxide, the distinction between glue work and feature work has largely vanished; the question I’ve repeatedly asked my coworkers and manager is “how can I be helpful?”, not “what would look good on my performance review?” But I accept that larger organizations will need a greater degree of legibility. I think all the usual answers apply: don’t overhire (and certainly don’t [incentivize managers](https://www.nbcnews.com/id/wbna54928896) to hire as many people as possible), don’t require all work to be legible, and trust your employees with the rest.

[^1]: I wrote the catalog in my own words, for two reasons: to gain a deeper understanding of the kinds of test flake patterns we’ve seen in our codebase, and to not subject my peers to AI slop prose.

[^2]: Did you know that OKR and KPI mean [slightly different things](https://www.atlassian.com/blog/teamwork/okr-vs-kpi)? Before writing this post, I did not!
