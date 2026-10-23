---
title: AI Is Getting Good at Math. Here’s Why.
link: https://latent.space/p/ai-is-getting-good-at-math/
source: latent-space
published: 2026-10-23T15:19:48Z
updated: 2026-10-23T15:19:48Z
first_seen: 2026-10-23T18:40:08.262074691Z
authors:
- Ankit Maloo
summary: AI models are solving more mathematical problems. A pattern is emerging in their successes that could help us predict which problems they’ll solve next.
content: extracted
html: 2026-10-23-ai-is-getting-good-at-math-here-s-why.html
remote_preview:
  url: https://beehiiv-images-production.s3.amazonaws.com/uploads/asset/file/bd3e76aa-5e3c-4cbf-b175-c20919ca2b9d/Math_Feature_Image-selection3.png?t=1792768487
extra:
  thumbnail: https://beehiiv-images-production.s3.amazonaws.com/uploads/asset/file/bd3e76aa-5e3c-4cbf-b175-c20919ca2b9d/Math_Feature_Image-selection3.png?t=1792768487
---

AI models are solving more mathematical problems. A pattern is emerging in their successes that could help us predict which problems they’ll solve next.

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/856c7ee4-cdf5-4da4-8c1a-6104620492aa/image.png?t=1792756171)

The question that interests me is whether we can predict what kinds of problems these models will solve. **What makes a problem tractable for an LLM, and what do these solutions have in common?** If AI solves problems differently from humans, identifying the difference should help us decide where to point it. The unit-distance disproof has the richest commentary, so let’s start there.

## Problem complexity

Here is a simple mental model. To solve any given problem, you need a sequence of hints or insights with work between them. This maps surprisingly well to simple everyday problems and to complex ideas like General Relativity[^1] (see Bonus). The sequence looks something like this:

Insight -> time / effort -> insight -> time/effort -> solution.

An expert would need fewer hints than a beginner would, as they would have more background. In his commentary on unit-distance proof, WT Gowers uses a similar idea: **the difficulty of the proof is the shortest sequence of hints that would let experts reconstruct it.** He calls it Kolmogorov complexity[^2] modulo experts. For the unit-distance problem, his hint sequence had three parts:

1. Look for a counterexample.

2. Take the best known construction and generalize it.

3. Try a sequence of number fields of increasing degree, but work with prime ideals of bounded norm.

Gowers’ proposed formula for complexity is 2^k + t, where k is the length of the shortest hint sequence and t is the work required to develop it. It is the sort of formula that looks rigorous right up until you try to put a number into it, which is more or less the point. **Complexity grows exponentially with the number of insights you need to solve a problem.**

Most problem solving is the work between these insights. You have a eureka moment, then spend months developing the hint until another conceptual jump becomes possible. For an expert, time spent is a useful proxy for this work, hence t. Hand someone the first unit-distance hint and they still have to do enough work to reach the generalized construction.

Useful, but post hoc. You can’t look at the problem and predict it would need x number of hints. So, a problem’s age, the number of tries, and the prize attached to it are common proxies to ascribe difficulty.

### Characteristics of a given hint

A typical insight changes the formulation of the problem. **The representation and understanding of the problem change**. Two things decide how much a hint is worth:

- How unlikely it looked before, against how obvious it looks after supplying the hint → This is harder than people realize. In unit distance, many experts did not consider the counterexample as likely.

- Where it belongs in the sequence: hints are ordered. A third hint is useful only after the work on the first two have made enough progress for it to be actionable. This defeats the naive brute force idea of listing a thousand mathematical techniques and trying every combination[^3] . Hints sequence traverses a tree. Initial insights change the problem and reveal the choices that follow.

### Humans solve problems breadth first

Combine k and t and you get **the human search pattern**. Generate candidate directions, explore them shallowly, abandon most, and commit to a few. A promising direction is abandoned if it does not give enough encouragement early[^4] . This is a human constraint: Developing an idea can take months. Before committing, a researcher needs reasons to believe it will lead somewhere: conceptual promise, early results, or experience with similar problems. The longer it takes to find out, the higher the bar for trying the idea.

Most research software exists to shrink this time by automating the tedious, mechanical, repeatable work. AI shrinks it far enough that the search pattern itself flips, and everything downstream follows from that.

## AI and complexity

First, t. The model’s equivalent of effort is compute and tokens, spent on one trajectory (test time compute) or spread across parallel agents (parallel test time compute). Pretraining also supplies knowledge that a human researcher would have to locate or reconstruct. No individual expert is aware of every previous development, so occasionally they end up redoing things already done.

The bigger difference is how models search:

### LLMs are depth first

Give the model a single prompt and it will give you *a* solution. Maybe correct, perhaps not. Ask for a proof and it will give you something that approaches a good proof. CoT for these models runs in a greedy [depth first](https://ankitmaloo.com/fable/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) manner too. And the process scales. Give parallel agents ten approaches and you get ten attempts developed toward a conclusion. Maybe all distinct, or some overlapping. Each either hits a roadblock or produces an argument that looks right. **Left alone, this gets you beautiful false proofs.**

This seems like a waste of compute and resources, until you add **an automated verifier**[^5] that can prune the completed trajectories and identify the correct ones. With this checker, it is now rational to spend enormous compute on uncertain ideas. You find out whether a given idea works or not within minutes. The catch is that the verifier can only judge finished arguments. It cannot tell you whether a half-developed idea is going anywhere.

### The contrast

How good does a hint need to be if you can find out whether it works in five minutes? For humans, the quality bar is high because it could take months of work to pursue a hint. For a model, the bar nearly disappears. The hint can be ordinary, implausible, or barely formed. Following it establishes whether it leads anywhere. **LLMs enable us to exchange time for compute, and can help us pursue more problems in a shorter timespan**.

### Failed attempts are hints for LLMs

The model doesn't need a definitive direction to start. Pretraining already gives it priors about the problem's history and past attempts. Every wrong solution can go back to the model as a negative hint for the next iteration. The system accumulates instructions about what has already failed and why. Roughly, a harness looks like this:

- **Drafter:** writes the prompts given to the solver model. The prompts are based on model priors, mitigating errors from previous runs, and accounting for all the promising paths that went nowhere (these are negative hints).

- **Solver:** Takes the prompts and comes up with one or many candidate solutions. It can run multiple subagents and pursue different paths using parallel test-time compute.

- **Verifier / Checker:** Checks whether the argument is valid and establishes the requested result.

For example, consider [this](https://cdn.openai.com/pdf/04d1d1e4-bc75-476a-97cf-49055cd98d31/cdc_prompt.pdf?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) CDC prompt:

*Assume for purposes of this task that a complete affirmative proof exists*. → ***This prevents the model from treating the problem’s open status, learned during pretraining, as a reason to stop.***

*In particular, proofs for special graph classes, constructions of cycle covers with some edges covered other than twice, bounded-length or prescribed-cycle variants, reductions to another unproved conjecture, computational verification through any fixed graph size, and candidate counterexamples without a complete nonexistence certificate are insufficient* → ***This rules out a catalogue of substitutes for solving the actual problem.***

*Use adversarial agents throughout: every candidate proof must be checked for exact-two multiplicity, repeated-edge closed trails masquerading as cycles, parallel-edge 2-cycles, disconnected graphs, cutvertices, bridges introduced by reductions, and circular use of an equivalent CDC statement* → ***The catalogued failure modes, promoted to standing checks.***

Plus one line against learned laziness[^6] : *Do not return merely because current approaches fail or agents report theorem-strength gaps*

OpenAI likely ran multiple rounds of attempts, the drafter took the errors and failed ideas, and steered the next iteration away from those. Every one of these lines is a failed path, written up as an instruction.

## Reconstructing Unit-Distance Proof and Tsimerman’s work

Let us read Gowers' three hints back through the harness lens:

**Hint 1: Look for a counterexample**: This is genuinely hard, because the community and model’s pretraining priors both say the conjecture is true. The drafter most plausibly arrived at it because the solvers kept failing while pursuing a proof. **The framing stops the model from treating the accepted view as the direction it must pursue**. Every failed attempt makes a counterexample a little less unlikely.

**Hint 2: Take the best known construction and generalize it.** Closest to a community prior; this is a standard move. The substantial work is developing the generalization far enough to discover what it permits. This is the large-t step. But notice, this is a generalization of a counterexample, which almost nobody was looking for.

**Hint 3: Try number fields of increasing degree.** Standard machinery once the first two hints have built the space in which it applies.

The order matters. Start at hint three and you don't even know the exercise is disproof. Skip the generalized construction and hint three has nothing to act on. And even with all three, effort remains enormous. Counterexample hint is obvious in hindsight, which makes you wonder why nobody tried it. In fact, some did.

**Jacob Tsimerman, recently hired by OpenAI**, worked on the counterexample. He knew the number theory. Even considered increasing degrees. And he stopped. Looking for a counterexample was the first insight. It was still only the beginning. What stopped him was not the lack of skill. It was the aptly sane feeling of a man several weeks into a swamp with no evidence that it has another side. His own words:

> “A very scary dynamic, hard to think through the analytic regimes and retain guiding intuition, consumes much time and frequently doesn't work out.”

Jacob Tsimerman, OpenAI mathematician

Psychology found this human side of problem-solving decades ago: Metcalfe and Wiebe asked solvers every fifteen seconds how close they felt to a solution, and on ordinary problems[^7] their reported sense of progress climbed. On insight problems, it stayed flat until they found the answer. They reported no feeling of progress to sustain the search. **Insight problems have no gradient**. You don’t know how close you are until you solve it.

So the expert question is twofold: does anyone enter the branch at all (the hint), and having entered, do they stay long enough for ordinary competence to finish the job? The costs of staying are working memory, morale, and career. And staying on a path every known expert considers wrong compounds all three. For an LLM, this is a huge advantage. From Arul Shankar:

*“The model went through ideas pretty quickly, but when it reached the crucial idea (in the paragraph starting with "Suppose optimistically that..."), it honed in on the proof quite methodically.”*

It compresses to two questions: how many insights does a problem need, and how much work sits between them before you can tell whether a direction is worth it? Tsimerman puts the AI side of it:

*“This may indicate one way that AI systems have an edge: it’s not just that they can try all known methods, but they can play for longer and in more treacherous waters than mathematicians without getting overwhelmed. Of course this is not yet robustly true, but this may be a foreshadowing event.”*

## Other proofs

**OpenAI's proof of Navier-Stokes** demonstrates the same process. Based on a cursory reading, it takes to completion the work done by Córdoba and Martínez-Zoroa. The proof runs 166 pages with many corrections and a lot of complex math, but in terms of conceptual jumps, it does not offer as many.[^8]  **Anthropic's work on Fermat and Riemann** comes with rich commentary, and it shows the same skeleton: hundreds of failed ideas, multiple agents under one orchestrator, scripts and checkers everywhere. And a small number of conceptual leaps carrying the load.

### What changed in models this year?

Three specific capabilities stack up. And they help drive the proofs.

- **Models are now good at chaining primitives.** Recent training objectives have focused on creating artifacts at step 1 that can be used at step n of a long horizon task. => This lowers the t.

- **Self verification**: Models can now catch steps that are wrong (not steps that are valid syntactically but lead to nowhere) within a trajectory before a human or even a verifier sees the output. OpenAI specifically trained models on math verification, with real-world examples at scale and a verification layer that identifies mistakes in proofs. => This makes pruning within a trajectory cheaper.

- **Orchestration.** Models can command subagents, use harness capabilities like memory for steering and eliminating the wrong paths, and coordination between agents. => This is how negative hints build up.

### Branching factor

Revisiting Gowers’ formula. He notes the formula leaves out one thing: **the time it takes to explore the tree of potential hints.** Call the number of plausible next hints at each step the branching factor, b. With b plausible hints at each step, k hints means b^k paths. For humans, taste and judgement keep the effective b small and most branches are discarded by default.[^9]

AI has to pursue almost every plausible branch at every intermediate step, but it can be done quickly. This predicts that models would do well where b^k x compute cost (where k is the number of hints) is small. This answers the question we started with. Cybersecurity seems complex, but it fell first, because the branching factor at intermediate steps[^10] is about 1-2.

Professionals need a lot of skill, but **what the solutions look like are known in advance.** This also explains why looking for a counterexample was so powerful. *That collapsed the solution space and b*. The same pattern is reflected in a community effort to direct agents toward finding counterexamples to all open problems.

The verifier prunes completed branches. The drafter prunes future branches, narrowing the search space by telling the solver which paths already failed. When in the middle, you have no pruning signal. Self verification catches steps that are wrong, not the ones which are syntactically correct but lead to nowhere. There is no gradient, and you have to explore all branches. Tsimerman paid for it in morale, AI companies would pay in compute. **Labs would pick problems with narrow solution spaces to show their models work.**[^11]

One intuition to draw from this: expect AI models to be very good at taking ideas to completion **when the conceptual leaps have already been made and what remains is tedious, complicated math.** That is, where k and b are not prohibitively large. Many of the other proofs published recently fit this. Navier-Stokes' proof finishes Córdoba and Martínez-Zoroa's program and even that took 10,000 agents, with a total of 300B tokens used in 88 hours.

If humans keep an edge anywhere, Gowers [expects](https://gowers.wordpress.com/2026/08/12/what-sort-of-maths-are-llms-good-at/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) it to be on problems "where the search tree is deep and has a large amount of branching, so that without rigorous pruning a search is not feasible even for a computer." In other words, high b and/or high k **where human intuition still matters**.

## An interesting divergence

Humans choose their approach carefully and then spend months on it. An approach that fails is insightful precisely because someone was convinced enough to spend months on it. Same goes for an approach an expert decidedly rejected. The rejection comes with their judgement and insights. Either way, **following an idea changes our understanding of the problem.** And even a dead end can leave experts with useful tools to think with.

With AI, a solution arrives because the model takes every plausible path to completion. Failures are cheap. Most of them are naive, some might be interesting. **The information in a failure scales with the conviction behind it.** Though we rarely get to see them anyway, let alone understand why a certain attempt failed. The CDC prompt is probably the closest we get, a list of all failures passed in the prompt itself.

This divergence helps create a good kind of complement. **Humans are good at shrinking branches. Models are good at shrinking the time it takes to prune one branch.** Experts can ask a model to explore an idea end to end before they would decide if it’s worth three months. Problems that need a lot of insights, i.e. large k, stay hard for both. The expert can narrow the scope using the model's failed attempts *and* their own process of getting to conviction.

Still, a lot of open problems are posed because solving them improves our understanding of the domain. Some of that comes from pursuing the problem directly, and some from knowing why high conviction approaches failed. A model can eventually generate the proof.[^12] Much of the insight in the failures. The complement only works if we get to see them.

Terence Tao [describes](https://terrytao.wordpress.com/2026/09/11/a-severe-misalignment-of-ai-in-mathematics?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) solving problems as **a proxy for the deeper purpose of mathematics: developing our understanding.** A result or a proof is a community event that gets people to explain new ideas, connect them to existing ones, and move our collective intuition forward. Announcing just the final solution is mistaking the destination for the journey.

## Bonus: Can LLMs ever come up with the General Theory of Relativity?

People have suggested ([Demis](https://www.youtube.com/watch?v=JNyuX1zoOgU&t=2230s&utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv), [again](https://www.youtube.com/watch?v=6rwIcNjRQU8&utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv), [Zahavy](https://www.tomzahavy.com/files/llms-cant-jump.pdf?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv), [Adam Brown](https://www.youtube.com/shorts/y9eGsI2C9iY?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv)) that one test for AI crossing the threshold would be to train a model on data up to 1901 and see if it comes up with general relativity. I now think, if researchers [goodhart](https://en.wikipedia.org/wiki/Goodhart%27s_law?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) on this, it will be quick. The leap doesn't need many hints.

The first working hint is to treat gravity as geometry, and that narrows the solution space sufficiently. The math after that is extremely hard, which is exactly where a model would excel. The hint is extremely unintuitive, and I think a CoT would land on it only after exhausting everything else. Try gravity as a force, as a field in flat space, as a tweak to Newton's inverse-square law. Each attempt fails against something already known by 1901, most famously Mercury's perihelion.

Every failure makes geometry a little more likely. It might come up with hundreds of plausible theories, but we will verify this one and be like, "yeah, it did it."

[^1]: Einstein had what he called the happiest thought of his life, that a man falling off a roof would not feel his own weight, in 1907. He then needed eight more years, and a good deal of help from a friend who actually knew the geometry, to turn it into equations. That is what the arrow between two insights looks like. General Relativity required two to three main insights. First was, treat gravity like geometry. Einstein sat with it for years, took that forward, and then got to the final theory in about eight years. The problem was, at the time everyone believed Newton’s laws to be sacrosanct, so to say Newton got it wrong took enormous conviction and belief. C.P. Snow once said, special relativity "was an idea waiting to happen. But the General Theory was something else altogether." That is the difference between an uninspiring and a very surprising hint.

[^2]: Kolmogorov complexity asks for the shortest program that produces a given output; here, the expert supplies the background knowledge.

[^3]: This is more permutations than there are atoms in the universe, and considerably more than there are graduate students willing to slog it out.

[^4]: This is also why different human cultures put a lot of emphasis on persistence. Tsimerman could have solved this problem had he persisted a bit more. No shade, but later in the essay, he explains how he felt being so close. Aside: at three months an idea, a researcher gets perhaps twenty serious bets in the most productive decade of a life, and most people would quite like one of them to come good.

[^5]: I have this theory that infinite monkeys on infinite typewriters can type out Shakespeare and a host of other novels, plus some original work. But, to know if they did, you need a verifier. Which would also likely reject all the original work they would have done alongside. Douglas Adams had a line on this: *"Ford, there's an infinite number of monkeys outside who want to talk to us about this script for Hamlet they've worked out."*

[^6]: It is a strange thing to have to write to a machine, essentially a note saying "no, really, keep going", and stranger still that it works.

[^7]: I don’t think asking every 15 seconds worked. I would have said no even if I felt I was close.

[^8]: This is obviously an early conclusion, but I don't see a rich commentary about the conceptual jumps.

[^9]: Like many things, taste and judgement come from priors. And priors can be wrong. As we see in the unit distance counterexample itself. In Gowers’ own account, "it never occurred to me to try to disprove it." This prior pushed the right branch towards low probability, and many experts did not pick that path.

[^10]: An exploit chain already exists in the literature. Model just has to fill the slots.

[^11]: Or who knows, they might announce a model that [spent](https://www.anthropic.com/research/riemann-zeta?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) forty bazillion dollars discovering that a problem was hard and there are so many ways not to solve it and instead settle for a different result.

[^12]: Douglas Adams predicted this one already. *"I checked it very thoroughly," said the computer, "and that quite definitely is the answer. I think the problem, to be quite honest with you, is that you've never actually known what the question is."*
