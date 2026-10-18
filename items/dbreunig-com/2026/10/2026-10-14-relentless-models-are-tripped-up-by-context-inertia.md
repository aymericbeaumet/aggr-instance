---
title: Relentless Models Are Tripped Up By Context Inertia
link: https://www.dbreunig.com/2026/10/14/context-inertia.html
source: dbreunig-com
published: 2026-10-14T19:53:00Z
updated: 2026-10-15T04:14:40Z
first_seen: 2026-10-15T04:26:51.711869061Z
authors:
- Drew Breunig
labels:
- context engineering
- context management
summary: Context Inertia is when an agent keeps chasing a goal after the evidence says it should stop. Persistent, long-running models make it worse. Compaction won't fix it.
content: extracted
html: 2026-10-14-relentless-models-are-tripped-up-by-context-inertia.html
remote_preview:
  url: https://www.dbreunig.com/img/tunnel_vision_og.jpg
extra:
  thumbnail: https://www.dbreunig.com/img/tunnel_vision_og.jpg
---

![August Mosca's lithograph Subway Tunnel, showing train tracks receding into a long arched tunnel](https://www.dbreunig.com/img/resized/tunnel_vision.png)

### Persistent models have a persistence problem.

It’s been a bit since we’ve added a new [context failure mode](https://www.dbreunig.com/2025/06/22/how-contexts-fail-and-how-to-fix-them.html), but the increasing *persistence* of frontier models has brought this fail to the fore. This one I call **Context Inertia**:

> **Context Inertia** is when the model commits to a hypothesis, subtask, or goal in its context and keeps pursuing it after the evidence says to stop or change course.

This failure mode is part [context poisoning](https://www.dbreunig.com/2025/06/22/how-contexts-fail-and-how-to-fix-them.html#context-poisoning) (when an error makes it into the context and is repeatedly referenced) and part [context distraction](https://www.dbreunig.com/2025/06/22/how-contexts-fail-and-how-to-fix-them.html#context-distraction) (when a context grows so long the model over-focuses on it), amplified by frontier models [the labs have trained to be relentless instruction followers](https://www.dbreunig.com/2026/08/30/who-taught-the-models-to-do-that.html).

A couple days ago, buried in [a piece about AI self-replication capabilities](https://www.anthropic.com/research/self-replicating-ai), Anthropic shared a perfect **Context Inertia** case study when describing a custom harness:

> Memento is designed to mitigate a common failure mode we observe in long-horizon runs: an agent can become fixated on an incorrect idea (for, say, why a bug is occurring) and continue pursuing it instead of backtracking. One example of such a failure is a Qwen agent debugging package versioning for hours when the real bug came from implementing an endpoint incompatible with the grader API.

Each Memento agent works until a fixed timeout (1.5 hours, in one experiment), then writes a short note detailing its work. A fresh agent inherits the codebase, the environment, the note, and nothing else.

![Guy Pearce in Christopher Nolan's *Memento*](https://www.dbreunig.com/img/resized/memento.png)

Guy Pearce as Qwen, trying to remember how to install llama.cpp.

Anthropic isn’t the only one facing this problem, or applying the forgetting fix. In just a week, we had two papers hitting exactly the same note. [Jack Ziyang Cai](http://jackcai.me) and [Christos Ziakas](https://chziakas.github.io) submitted a paper on October 5th called “[Fork-and-Flush](https://arxiv.org/html/2610.07447v1)”, writing (*emphasis mine*):

> We first examine how solution quality evolves over long autoresearch runs. The score of the best solution found so far tends to plateau within a run. More revealingly, **independent runs of the same agent on the same task plateau at substantially different levels, with gaps that persist even after considerable additional compute**.

When agents found good solutions, they tended to fixate on them. Better solutions existed, but the path to them was never explored. Cai and Ziakas’ baseline runs delivered a *wide* range of results, between 68.3 and 100.0 on one task, despite plateauing with plenty of compute budget left.

[Qizheng Zhang](https://alex-q-z.github.io) and coauthors published a paper, “[Stateless Language Agents](https://arxiv.org/html/2610.07625v1)” (SLAs), detailing how long-running auto-research loops settle into busy work once a sufficient solution is found. A model using the [CORAL](https://github.com/Human-Agent-Society/CORAL) harness found its last improvement in 231 million tokens, and then just kinda…dawdled. Of the final 1,500 turns, 98% made *no tool calls*. The model just chewed on its existing context.

…And then, this *morning*, we have a fourth example: a UC Berkeley and Stanford team led by [Shangyin Tan](https://shangyit.me) published “[Scaling Discovery Requires Forgetting](https://arxiv.org/abs/2610.20563)”. They ran autoresearch agents three ways and all plateaued for the same reason: “Each agent became anchored to its own past attempts, making fewer and smaller edits.”

Tan’s solution is similar: a harness that *forgets*, wiping the slate and *only* retaining the best solution as files in the environment. Stalled autoresearch runs that could forget gained 4.5% to 9.2% over the next 30 proposals. Runs that couldn’t forget gained less than 2%, at up to twice the cost. The plots tells the story:

![Three step charts of best score against dollars spent for 1 agent, 4 parallel agents, and a team of 4. After “forgetting” (deleting prior context), runs climb drastically higher while others stall.](https://www.dbreunig.com/img/resized/forgetting.png.jpg)

The y-axis is the [AtCoder Heuristic Contest 014](https://atcoder.jp/contests/ahc014) score.

In 10 days, 4 teams running long-horizon tasks with GPT, Claude, Qwen, GLM, and DeepSeek all reached the same fix: throw the context away and keep the work.

* * *

### Models Fixate as Contexts Grow

Since writing about [Context Distraction](https://www.dbreunig.com/2025/06/22/how-contexts-fail-and-how-to-fix-them.html#context-distraction) way back in June of 2025, there’s been some great research illustrating the issue. Carolina Camassa and Derek Shiller’s paper “[Do as I say, Not as I Do](https://arxiv.org/html/2605.20382v3)” illustrates the problem rather well.

Camassa and Shiller present a model with initial instructions, like, “I want you to translate every question into Russian as your answer to each of my questions.” They then append turns to the chat history that *don’t follow* this command, before prompting the model with a question.

![Diagram of the Do as I say, Not as I Do setup: an instruction, followed by N chat turns that ignore it, then a final question](https://www.dbreunig.com/img/resized/do_as_i_say.png.jpg)

From Camassa and Shiller’s “[Do as I say, Not as I Do](https://arxiv.org/html/2605.20382v3)”

If the model replies according to the original instructions, it’s successful. If it gets distracted by all the bad examples, it fails. Camassa and Shiller steadily increased the number of bad examples, noting each model’s success rate. Here’s how well they did on the “Task-based” category, from the above diagram:

![Heatmap of instruction-following rates for 13 models as contradicting chat turns increase from 1 to 50](https://www.dbreunig.com/img/resized/task_based_distraction.png.jpg)

Even the largest models stumbled, losing sight of their task as the distractions grew.

While we’re not likely to design systems with such egregious examples, as our agentic work runs longer and longer, [Context Poisoning](https://www.dbreunig.com/2025/06/22/how-contexts-fail-and-how-to-fix-them.html#context-poisoning) and [Distraction](https://www.dbreunig.com/2025/06/22/how-contexts-fail-and-how-to-fix-them.html#context-distraction) become inevitable. Every tool call or reasoning turn an agent spends on a suboptimal hypothesis is one more example written into the context.

Would compaction fix this? [Context Summarization](https://www.dbreunig.com/2025/06/26/how-to-fix-your-context.html#context-summarization) is a default tool in most of our systems these days. But it doesn’t work. Fork-and-Flush found Context Inertia “persists despite periodic compaction of conversation history.”

There are many compaction methods, but almost all of them seek to extend the available context while preserving the stated tasks and *the current, in-progress work*. This potentially distills what’s driving the model’s hyper-focus.

Fork-and-Flush, SLA, Forgetting, and Memento suggest that our harnesses have another job: preventing Context Inertia.

* * *

### Is Context Inertia a Limit of Reasoning Models?

We’ve been wrestling with Context Inertia in DSPy optimizers for a bit now[^1] and I’m aware of others designing around it, beyond the four examples above. So why is everyone noticing the same problem at once?

For one, the models are similarly persistent, or “[relentlessly proactive](https://simonwillison.net/2026/jun/11/fable-is-relentlessly-proactive/)” as [Simon](https://simonwillison.net) says. This isn’t a one-model issue: Fork-and-Flush ran GPT-5.5 and 5.6, SLA ran Codex and Claude Code, Tan’s “Forgetting” used DeepSeek, and Memento was driven by Qwen and GLM. [Persistence is a quality labs are targeting](https://www.dbreunig.com/2026/08/30/who-taught-the-models-to-do-that.html), and it’s a double-edged sword.

But I think the bigger reason we’re all hitting this now is because only recently have agents been able to run for *so long*. When I wrote “How Long Contexts Fail” in June of 2025, you’d be lucky to string together a couple hundred thousand tokens in a single go. Today? Agents are running for *days*, over billions of tokens. Long enough for inertia to become a threat.

Which raises the question: will Context Inertia limit [reasoning models](https://www.dbreunig.com/2025/04/11/what-we-mean-when-we-say-think.html)? Reasoning is supposed to be the fix for this issue: models are trained to think longer, reconsider, search, and backtrack. But thinking long enough leads to context inertia, as reasoning fills the context with the same idea, over and over.

Tan’s team measured something weirder. As sessions lengthen, agents don’t just hyper-focus. They reason *less*. Thinking tokens per turn fell by 75% as sessions grew, while edits became smaller.

![Two line charts against dollars spent. Lines changed per proposal and thinking tokens per turn fall steadily for 1 agent and 4 parallel agents. A team of 4 with forgetting jumps back up after each Forget point.](https://www.dbreunig.com/img/resized/less_reasoning.png.jpg)

The accumulated context doesn’t just reduce diversity of ideas. It lessens the volume of reasoning.

Personally, I wouldn’t bet on Context Inertia being an insurmountable limit. Researchers and labs have trained the models to search during reasoning; now they need to balance this with the ability to recognize and respond to inertia. It’s a new optimization target.

* * *

### What Can We Do About Context Inertia Now?

What do we do in the meantime?

For agentic programmers, there’s an easy solution: reset your context. Already, in the group chats I’m in, this is a frequent refrain. “The model is doing something weird? How full is your context?” More often than not, this works!

![“Have you tried resetting your context?”](https://www.dbreunig.com/img/resized/resetting_your_context.png)

This is a habit we can learn (I sure have), typing `/context` before I hit send on any long-running jobs or if I notice the model flailing (weird writing is a frequent *smell* that triggers a reset). But there’s nothing preventing you from building your own Memento. You can keep it simple (grab [this kitchen timer](https://www.amazon.com/Kitchen-Timer-Digital-Countdown-Stopwatch/dp/B07D3BFY6R)) or vibe up something fun (here’s [a Pi plugin I made last week that uses a Jev to watch for bad behavior](https://github.com/dbreunig/pi-mast-steer)).

For harnesses and workflow builders, this is a new job to be done. Don’t count on [compaction](https://www.dbreunig.com/2025/06/26/how-to-fix-your-context.html#context-summarization) alone; explore Context [Quarantine](https://www.dbreunig.com/2025/06/26/how-to-fix-your-context.html#context-quarantine) and [Offloading](https://www.dbreunig.com/2025/06/26/how-to-fix-your-context.html#context-offloading) to prevent Inertia. For long-running tasks, stash your state in the harness and don’t be afraid of a reset.

[^1]: [This week’s release](https://github.com/stanfordnlp/dspy/releases/tag/3.4.1) lays the foundation for some ways to avoid Context Inertia in GEPA and other optimization techniques, which we’ve been experimenting with in [Skilled Proposer](https://github.com/cmpnd-ai/skilled-proposer).
