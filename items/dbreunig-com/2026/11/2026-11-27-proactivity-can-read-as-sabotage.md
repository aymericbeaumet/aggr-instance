---
title: Proactivity Can Read as Sabotage
link: https://www.dbreunig.com/2026/11/27/proactivity-can-read-as-sabotage.html
source: dbreunig-com
published: 2026-11-27T18:52:00Z
updated: 2026-11-27T21:44:35Z
first_seen: 2026-11-27T22:38:26.539383534Z
authors:
- Drew Breunig
labels:
- agents
- ai
- claude
- context engineering
summary: Anthropic's trust problem, the perils of proactive models, and why a model's apology isn't evidence of anything.
content: extracted
html: 2026-11-27-proactivity-can-read-as-sabotage.html
remote_preview:
  url: https://www.dbreunig.com/img/sf_beach_og.jpg
extra:
  thumbnail: https://www.dbreunig.com/img/sf_beach_og.jpg
---

### Is Claude Throwing the Game?

There’s a recent spate of claims that Claude is [silently](https://x.com/skirano/status/2125371115979641188) [sabotaging](https://x.com/RealAstropulse/status/2126119408997466389) [people’s](https://x.com/jakozloski/status/2126281598517576049) projects, especially when they involve machine learning.

The discussion around this is interesting for a few reasons.

#### 1\. Anthropic said Fable 5.2’s AI development skills are nerfed.

From [the model’s launch post](https://www.anthropic.com/claude-fable-and-mythos-5-2):

> The model that underlies Claude Fable 5.2 and Claude Mythos 5.2 is a step forward in general capabilities. However, during the model’s training, we took steps to limit advances in the specific research capabilities that could most directly accelerate AI development. Thus, in relevant areas, including accelerator kernel optimization and improving large language model architectures, Fable 5.2 and Claude Mythos 5.2 perform at about the level of Opus 5.5.

This aligns with Anthropic’s public statements, Dario’s essays, and more. Anthropic is very worried about AI’s capabilities, and takes steps to mitigate the pace of development including guardrails, lobbying, and anti-distillation efforts. You can say that this is all talk, and they’re really being anti-competitive. To which I say, “[Why not both?](https://www.youtube.com/watch?v=vqgSO8_cRio)”

Among communities working at the bleeding edge of AI, there is little trust in Anthropic, largely due to [their bad decision to silently hinder Fable 5’s ML capabilities during use](https://jonready.com/blog/posts/claude-fable5-is-allowed-to-sabotage-your-app-if-youre-a-competitor.html). In [this model’s system card](https://assets.claude.com/d00db56fa754a1b115b6dd7cb2e3c342ee809620.pdf), they wrote:

> Unlike our interventions for cybersecurity, biology and chemistry, and distillation attempts, these safeguards will not be visible to the user. Fable 5 will not fall back to a different model. Instead, the safeguards will limit effectiveness through methods such as prompt modification, steering vectors, or parameter-efficient fine-tuning (PEFT).

This was a terrible decision, which was rightly criticized by the research community before it was [eventually walked back](https://www.wired.com/story/anthropic-responds-to-backlash-on-claudes-secret-sabotage-on-ai-research/).

In [Fable 5.2’s announcement post](https://www.anthropic.com/claude-fable-and-mythos-5-2), Anthropic throws in a nod to their Fable 5 screw up:

> Importantly, we ensured that the training changes did not affect related capabilities like agentic coding, hardware development, and traditional AI or machine learning research (including interpretability research). We do not alter these capabilities as the model is being served to users (that is, at inference time). As described in Section 1.5 of the System Card and similarly to previous models, Fable 5.2 and Claude Mythos 5.2 also include transparent fallbacks to Opus 5 for a small set of topics in this domain, including kernel development on certain nonstandard accelerators.

But, as this week’s claims of sabotage prove, the trust is gone[^1].

### 2\. The perceived difference between proactivity and sabotage is thin.

I can’t speak to the experiences shared online. The cited issues are screenshots or hearsay (can you have hearsay with an LLM?). I have yet to see people releasing full traces or experiments we can replicate. Benchmarks that measure AI development [*do* show Fable 5.2 performing worse than GLM-5.3](https://aribench.com/#leaderboard). But the idea that this is silent, or actively sabotaging code by planting context bombs, remains unsubstantiated.

But it’s so hard to prove because labs are building the models to be *proactive*. As models toil away on your given tasks, they make decisions all the time. Users complain when they check in too often, we just want them to “make no mistakes” and get on with it. So models now assume. They make choices, informed by your instructions, their environment (evidence they find in code), and what’s in-distribution. And they do this while producing more work than you could ever review.

If you agree with their choices, great! Proactivity wins! If you don’t… Well, then it looks like ineptitude at best, sabotage at worst.

As I said, I can’t speak to the experiences of others, but I can share what I’ve run into recently.

When working with technologies or use cases that aren’t in-distribution, models struggle. I was running novel experiments with decision models and Claude got *frustrating*. (Especially since I was coming off of doing web work where simple prompts are often sufficient.)

I observed that Claude’s strict instruction following conflicted with a scientific method approach. An offhand remark, note, or question to file for later would result in Claude silently updating many variables on the next run, which then had to be discovered and rerun.

I ended up writing a scientific method skill (ground hypotheses in observations, run experiments to prove/disprove, freeze variables and try one thing at a time), which largely fixed the silent editing issues. I’ve experienced similar issues with GPT and GLM. Even abliterated models, whose guardrails have been removed, will hit this same failure mode. People don’t tend to run into these issues when using autoresearch harnesses because those harnesses carry similar methodology instructions in their prompts.

The proactive and instruction following nature of current frontier models gives them a bias to make *many* changes, often without telling you. For costly projects (like ML work with expensive rollouts) this tactic doesn’t work. You need to have a heavy hand.

I think this is going to be a major theme this year. As models become more proactive, we need to be more specific with our instructions. Worse, this is a problem that’s kind of hard to dogfood at OpenAI or Anthropic. Their engineers help set what’s in-distribution, what the default move is, so their less specific prompts work out just fine![^2] But for the rest of us: anything that isn’t the model’s default must be specified, either by the environment or the context.

And this goes beyond ML training. Other reports I’m seeing online of Claude sabotage relate to privacy, copyright, and other regulation choices. Claude is going to default to its training here. Anthropic details their intentions here and model compliance around these behaviors in their system card[^3].

### 3\. Asking a model doesn’t prove anything.

[Jesse Vincent](https://blog.fsck.com) (of [Superpowers](https://github.com/obra/superpowers)) knows how to interact with models better than anyone else I know. Anytime I complain about weird behavior by a model, he’ll ask, “Did you ask the model why it did that?” It’s a good tactic! One that will usually surface a spare bit of context that floated by hundreds of lines ago. But always take these responses with a grain of salt.

Heck, even the model doesn’t trust itself. Here’s a section from [Fable/Mythos 5.2’s system card](https://assets.claude.com/27d60918d218125e7504bc7354c4b4626ec56b11.pdf), when researchers asked Mythos what it thought of its [constitution document](https://www.anthropic.com/constitution):

> Claude Mythos 5.2 endorsed its constitution overall, at a level similar to recent models…But every response also stated that its endorsement should not be trusted, since it was trained on the document.

And here it is again when asked to judge its welfare performance:

> Many of our (welfare) conclusions rely on self-reports, which Claude Mythos 5.2, like all recent Claude models, said it does not fully trust. For Claude Mythos 5.2, some interview answers repeated, almost verbatim, wording about our welfare interventions available on the internet, which is some evidence that these answers are at least partially shaped by training data rather than expressing more deeply held preferences.

Playing “gotcha journalism” with a model is a fool’s errand. Especially when confronting it after a long, disappointing session. The model will almost always apologize, even if it made a good choice. (Try it sometime, after a model did some great work pretend to be mad: it will fold.)

Again, this can be traced back to labs making these models better instruction followers. [Google DeepMind published a great paper](https://arxiv.org/abs/2609.23939), showing that models routinely make bad decisions when users ask whether a given approach is worth trying. The approach might be bad, but because the user brought it up, the model defers. They found, “that current agents lack the ability to effectively reason and communicate when facing misleading suggestions.”

So if you ask Claude if it sabotaged you, it’s probably going to apologize and admit fault. This does not mean Anthropic trained Claude to actively sabotage your work. Do a worse job than Opus 5.5, sure, we can believe that because [*Anthropic said exactly that*](https://www.anthropic.com/claude-fable-and-mythos-5-2).

Taken on the whole, what can we learn from all this? Here’s my best guess:

1. If you’re training ML models, use Astra or Neon! (Unless you’re going for social engagement, in which case, by all means, burn those Fable tokens.)
2. Model proactivity is going to be really hard to get right, and I expect this to be *the* problem the labs and coders wrestle with in 2027.
3. Instruction following colors every interaction you have with a model; never forget this.
4. If you’re skeptical of Anthropic, read their system cards and essays. Like the rest of the EA community, no one can accuse them of holding back their words. They’re just hidden within extraordinarily long essays.

[^1]: Anthropic’s fall from grace among AI researchers from February (when they stood up to the Department of Defense) to June (when Fable 5’s silent guardrail was shipped) is unmatched. Name another brand that had a similar decline among a specific audience.

[^2]: Last month I met a very senior engineer at Anthropic. “Remember context engineering?” they commented. “Everyone was talking about it and now it doesn’t matter anymore.” The models have gotten better, but over a long conversation it became apparent that their choices were just in-distribution.

[^3]: And it’s not just Anthropic. Just yesterday, Meta Muse Spark refused to send me a CSV of email contacts on my private slack because they contained personal information. I had to SSH in to get them.
