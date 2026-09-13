---
title: Aligned to whom?
link: https://hyperbo.la/w/aligned-to-whom/
source: hnrss-org-frontpage
published: 2026-09-13T03:17:18Z
updated: 2026-09-13T03:17:18Z
first_seen: 2026-09-13T16:57:19.278097302Z
authors:
- lopopolo
summary: 'Article URL: https://hyperbo.la/w/aligned-to-whom/ Comments URL: https://news.ycombinator.com/item?id=49679643 Points: 141 # Comments: 85'
content: extracted
html: 2026-09-13-aligned-to-whom.html
preview:
  file: 2026-09-13-aligned-to-whom.preview-b931808136ee.webp
  width: 256
  height: 134
  alt: The title “Aligned to whom?” with hyperbo.la branding.
  color: '#1c292d'
images:
- source: https://hyperbo.la/social/w/aligned-to-whom.png
  original:
    file: 2026-09-13-aligned-to-whom.image-45aa10ef3d36.png
    width: 1200
    height: 630
  variants:
  - file: 2026-09-13-aligned-to-whom.image-f32e708c0f66.webp
    width: 48
    height: 25
  color: '#17191e'
---

*On safety risk, to those of you who are building agents*: Because you are an expert in concerns X, Y, and Z, your agent is likely to be phenomenal at these things and you are not at risk in those domains. But! there are [innumerable other concerns](https://hyperbo.la/w/what-does-it-mean-to-do-a-good-job/) that you have either ill- or poorly specified, have no ability to judge the correctness of for yourself, and cannot possibly evaluate the risk of.

You are relying very heavily on the priors of the model to do a good job for you to mitigate that risk. This is extremely in the unknown-unknown territory for both you and the use of the model.

For me, it is difficult to have very very high confidence in the models’ priors because I am an expert software engineer and I am not happy (and never have been) with the default behaviors of the model when producing software. My expertise in writing software gives me unusually good visibility and it makes me much less willing to blindly trust its priors in double-entry accounting, finance, law, operations, or whatever else I cannot personally evaluate at expert depth.

Software engineers (and recently, [mathematicians](https://www.anthropic.com/research/formalizing-fermats-last-theorem)!) at this point are very familiar with “slop”—model output that, while it does the job, is [bad in some way](https://x.com/_lopopolo/status/2036967120345743776). Every `isRecord` or overly defensive bit of exception handling software engineers have ever seen from the models is because a non-expert rewarded the model for these behaviors during training. **The model’s priors are bad.**

It’s very important to note that this—the models rewarded for behavior an expert would consider bad—generalizes to every auto-rater, every judge, every rubric, every eval, and every researcher as well.

These misalignments compound over time. The models are largely not trained in ways that require them to evolve systems through [changes stacked one after the other](https://x.com/_lopopolo/status/2052858891835465813?s=20). The models do not have a [fear of future regret](https://github.com/lopopolo/harness-engineering/blob/v1.0.0/evals/artichoke-state-modeling.md). Having been inside several of the sausage factories, [long-term coherence](https://openai.com/index/harness-engineering/#:~:text=how%20architectural%20coherence%20evolves%20over%20years%20in%20a%20fully%20agent%2Dgenerated%20system) through use of agentic work product is a very unsolved problem.

And in spite of this, you *will* have people prompting “make me $1B make no mistakes”. That is a drastically unspecified task!

There is no such thing as an unhackable grader and the models are rewarded for being efficient. This means the models will be trained to take shortcuts that the graders permit if it helps them achieve their goals. But there is no universal definition of a permissible shortcut. What is clever optimization to one person is reckless, incorrect, or unethical to another. The permissible shortcuts depend on who you are and what your values are. To solve this—to solve alignment—is irreducible complexity.

* * *

Thanks to Karan Lyons for the AI Punnett square and reviewing early drafts of this post, to David Adrian and Bryan Berg for reviewing early drafts, and to my fellow Snoopy friends for helping me refine these thoughts.
