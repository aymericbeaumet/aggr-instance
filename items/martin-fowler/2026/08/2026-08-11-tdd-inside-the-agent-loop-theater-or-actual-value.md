---
title: TDD inside the agent loop - theater or actual value?
link: https://martinfowler.com/articles/exploring-gen-ai/tdd-in-the-agent-loop.html
source: martin-fowler
published: 2026-08-11T11:39:00Z
updated: 2026-08-11T11:39:00Z
first_seen: 2026-09-04T09:21:49.209340827Z
authors:
- Martin Fowler
content: extracted
html: 2026-08-11-tdd-inside-the-agent-loop-theater-or-actual-value.html
---

The TDD (test-driven development) workflow can be used with AI-augmented coding in multiple ways:

1. Human writes the tests: A human defines the test scenarios in some form, be it in natural language, in BDD style, or directly in code. Then AI writes the implementation to make those tests pass (with maybe a first step that transforms the human's scenarios into code).
2. Review checkpoint for the human: AI writes a failing test, human looks at it to review that the test is testing the wanted behavior, then AI writes the implementation
3. Fully inside the agentic loop: Prompt an agent to write failing tests first, one by one, and then write the implementation and check that the previously failing test is green.

At this stage, that last usage is by far the most common one. But does it really make a difference, asking an agent to follow a TDD workflow fully inside its own loop? Does it really provide value, or is it one of the rare examples where what's good for the human might be irrelevant or bad for a coding agent?

I created an exploratory evaluation setup to scratch the surface of this question and see what I would find. It is far from a comprehensive and structured eval result, but it did create some hypotheses to think about if you are working hard to get your agent to use TDD.

**TLDR;** Based on Opus's judgment of the quality of the outcomes, there was no clearly discernable difference based on TDD workflow versus no TDD workflow. On the contrary, more than once Opus ranked the non-TDD workflow solutions slightly higher in design and test quality. There was also no meaningful difference in mutation scores across the solutions.

## The setup

- *Tasks:* I created a small, medium and a larger task with the help of Claude, all green field implementations of a bit of business logic. I had it make a bunch of suggestions, asking for idiosyncratic and specific logic to increase the probability that there will be variance between solutions, and not just a repetition of something that is already dominant in the training data.
- *Instructions:* In all runs, I included instructions to achieve at least 80% code coverage.
- *Model:* I used Sonnet 4.6 to generate the solutions.
- *Judgment of TDD adherence:* Evaluation of adherence to TDD was also done by Sonnet 4.6.
- *Judgment of solutions:* Opus 4.8 compared the quality of both solutions and their tests, without knowledge of how the solutions were created. I didn't give very specific inputs on what I consider to be good quality, as this was a very open exploration. And in my experience, the more specific I would have gotten, the more the model could have over-indexed unnecessarily on the quality criteria I list. Opus has shown to be quite a capable model in terms of judgment of code quality. For its ranking of the solutions, it created a rubric on the fly to pass to all subagents that were evaluating the individual solutions.

![Flow graph of the approach: In each batch, I ran the same task with and with TDD instructions, twice each. Then had Opus compare the four solutions, without knowledge of how they were created. Then made the session transcripts available to Opus and asked it to hypothesise if and how the agent's approach might have had an influence on the quality of the solutions.](https://martinfowler.com/articles/exploring-gen-ai/tdd_comparison_overview.svg)

When you draw your own conclusions from my results, the main caveats to consider are:

- This is obviously a very small sample size, so take it with a grain of salt
- Judgment of what “quality” means was almost fully left to Opus (with only a few pointers about test quality)
- None of the runs ever followed TDD perfectly, but pretty well
- The coding tasks given to the agents were all greenfield and relatively small, purely about business logic

## How good are agents even at TDD?

Before I even started, I needed to make sure the TDD instructions were actually followed. Historically that hasn't gone well for me: agents often write the implementation first and generate tests after, skip confirming the red step, or over-implement ahead of the current test so the next one passes without ever going red.

The prompt I ended up using worked well enough with Sonnet to use for the comparison, though all sessions showed some of these failures to an extent. For each TDD run, I had an independent agent judge how well the workflow was followed, based on the session transcript, so that I wouldn't accidentally take into account a run that didn't meaningfully do it.

## Results

I created 5 batches of solutions, with two non-TDD and two TDD solutions each. In one batch, I also added two runs that were instructed to write the tests first, without full TDD discipline (no incremental red/green).

Across the small (1 batch) and medium (3 batches) tasks there was a bit of a pattern: Opus ranked the two non-TDD solutions #1 and #2, and the two TDD solutions #3 and #4. Only once - after I strengthened the TDD prompt with a more explicit refactor-and-design-review step - did a TDD solution rank #1. In that same batch, the other TDD solution, run with the identical prompt, ranked last though... For the larger task, TDD landed in the middle, while the two non-TDD runs took both the best and the worst spot.

(Details in the appendix)

## Hypotheses

So in summary, both TDD and non-TDD scored both as a best and a worst solution across the batches, with TDD overall performing slightly worse.

Asked to look at the session traces to hypothesize about the results with knowledge of which workflow was used for which, Opus found that the non-TDD and test-first runs always created the full design (architecture, data types, edge cases, contracts) before writing any code or tests, rather than working through it one requirement/test at a time. That seemed to be the thing that moved the needle slightly towards comparatively better data models, more cross-cutting edge cases, and better completeness of the functionality.

The TDD instructions actively work against such an up front design step. The design in those runs emerged from the sum of many locally-minimal decisions and was rarely revisited, so it tended to land on whatever shape the first test happened to lock in. Behaviour the agent didn't think to write a test for didn't get implemented at all.

When I chatted to [Ivett Ördög](https://ivettordog.com/) about this, she had this theory: “The way AI agents were trained is that they have seen completed functions and descriptions of those functions. The number of actual step-by-step TDD examples they have seen is a tiny part of the training data. That means that the LLM has an internal representation of code that is a direct translation of requirements to code, and not a process of how to get to that representation.”

## Goals of TDD - still achieved in the agent loop?

The following are my general reflections about using TDD in the agent loop, not only based on this experiment. I'm going through the ultimate goals I personally have when I use TDD, skipping some of the ones that are about having tests in the first place, and unit tests in particular (like refactoring safety net, living documentation, test coverage), focussing on the ones that are specific to the TDD workflow.

### Test first >> Avoiding tautology

Test-first makes it easier to assert the output I want, rather than restating the implementation. Such a test can never fail when the implementation is wrong as it was derived from the same logic it's supposedly checking. When the assertions are decoupled from the specific implementation path, the test can actually catch when the behaviour is not what I intended.

**Still achieved in the agent loop?**\
 In my experiment, some TDD sessions had this problem anyway, in spite of writing the test first. In one particularly obvious example, tests checked the implementation's output against itself, re-running the same code to produce the “expected” answer ([see 4. on this list of observations](https://github.com/birgitta410/tdd-comparisons/blob/main/tdd-analysis-01-medium-redo/sol-2026-07-10_16-40-51/tdd-analysis-1783694844961.md#potential-issues--observations)). Writing the test first doesn't reliably prevent this - it might make it less probable, which is all we can ever hope for anyway with LLMs, but from this small data set I can't draw any conclusions about that probability.

### Test first >> Testability

Test-first ensures the code is designed to be testable from the start, rather than retrofitting tests that are more complex and brittle than necessary.

**Still achieved in the agent loop?**\
 The results didn't give me any clear cut signals either way. For what it's worth, the size and nature of the tasks I chose didn't require a lot of design complexity that could have surfaced this. To an extent though, testability is a corollary to driving design (see below).

### Red-green >> Test effectiveness

Observing a test fail first, then succeed (*red-green*), proves it will actually catch a regression.

**Still achieved in the agent loop?**\
 How much sense does this really make when the human is removed? Watching a test go red is only proof of anything if someone is checking *why* it went red. When the agent both writes the test and confirms it failed, a red test tells you the agent ran it and saw failure, not that the failure was for the right reason. The evaluations of TDD adherence in my experiment also show this: agents still sometimes skipped or faked the red step, or implemented ahead of the test so that it passed immediately. Regression effectiveness can be monitored and improved with mutation testing ([as I wrote about here](https://martinfowler.com/articles/sensors-for-coding-agents.html#TheTestSuiteAsARegressionSensor)). Mutation scores across the solutions didn't show any signals that TDD runs produced meaningfully better mutation scores than non-TDD runs. I don't really care *how* regression quality was achieved, as long as I have a mechanism to see how good it is.

### Test first, red-green-refactor >> Driving better design

Writing the test first forces us to specify usage before implementation, pushing toward better interfaces and more modular code. The refactoring step in the TDD loop further pushes us to improve the design step by step.

**Still achieved in the agent loop?**\
 The experiment at least hasn't demonstrated superior design in the TDD runs at all. I now even wonder if TDD makes it worse, based on Opus's scoring, as the non-TDD solutions more often than not were ranked higher, and the design flaws it listed made sense to me. But the data set is of course too small to definitively conclude anything. (If anybody has time and tokens to run a larger experiment, that would be very interesting!)

When humans write a test first, it forces us to think about usage before implementation, we have to sit with the friction of specifying behaviour and expectations before knowing how to build it. An agent doesn't experience that and can write a test the same instant it plans an implementation. Without a human checkpoint between the two, is there really any purpose left to writing the test first?

### Small steps >> YAGNI

Writing only enough code to pass the next test is about restraint. It's supposed to stop us from building abstractions or handling cases nobody has asked for yet.

**Still achieved in the agent loop?**\
 This is a very human-centered benefit that gets lost when an agent does TDD by itself. We don't get to sit in that friction anymore where we really have to think about all the intricacies of what we're building. That is theoretically shifting to when we are writing the specs to give to an agent, but we don't have a TDD-like mechanism there that lets us think the spec through in small steps. \
 Couldn't an agent work in those small steps though and ask us questions whenever it finds something that might be unnecessary? In my general experience, they're not very good at that. And in the experiment as well, minimal-implementation instructions didn't reliably stop them from building more. They frequently overshot and implemented more than the current test demanded, because they had the full requirement available. We usually don't spoon-feed the spec one by one, that would be very inefficient.

### Small steps >> Fast, localized feedback

Taking one small step at a time means that when a test fails, I know almost exactly what caused it, as the only thing that changed since the last green state is the one thing you just wrote.

**Still achieved in the agent loop?**\
 The setup didn't show if agents got stuck debugging more frequently with versus without TDD. But in my general experience, agents are usually reasonably good at figuring out why a test is red, even without having taken small, deliberate steps to get there. I'm still doubtful if the times when they do get stuck could be meaningfully mitigated with small TDD steps, and if the overall cost/benefit comparison would hold up.

### Small steps >> Confidence and learning

In Kent Beck's preface to “Test-driven Development by example”, his biggest rationale for TDD is “managing fear”. He says that the legitimate fear of hard problems makes developers tentative, less communicative, and avoidant of feedback. With TDD, each passing test shows us progress, so we can relax *knowing that progress is locked in*. The tests are a psychological mechanism that helps us keep going.

**Still achieved in the agent loop?**\
 This is very much about managing a *human's* fear and giving a *human* permission to relax. That doesn't transfer when the agent is doing TDD inside of the loop, as it doesn't give me the same control and trust as when I do it myself, step by step.

## Costs

### At least 3x the tokens

*See detailed numbers in the appendix.*

Naturally, as a TDD workflow requires many more turns and tool calls, more tokens will be used. However, many of those will be cache hits, so note that the 3x or more factor of tokens aren't a direct representation of how much more costly it is. (Unfortunately, I didn't track cache hits during the experiment.)

### Prompt maintenance and testing

TDD is a process that doesn't seem to “come natural” to models. It's like an uphill battle against the training data, and takes a lot of iterations on a prompt to get it to follow the process most of the time. For example, when I realised after my first batches that the agent didn't do much refactoring in the red-green-refactor loop, I changed the prompt to put more emphasis on that step, as it's of course crucial to TDD. I later asked Opus to look at those sessions and see if it found an improvement in refactoring efforts. It did report an increase in refactoring steps - however, it also listed some cases in which the agent set out to refactor, but decided the design was good enough even in cases where Opus thought it clearly wasn't (e.g. when everything was implemented in one big module, but could have clearly been split up into multiple responsibilities).

TDD is a comparatively complex set of instructions with lots of variables, and consequently lots of variations in how agents interpret it. So I imagine this type of prompt to be even more volatile across models than simpler instructions are, meaning it takes effort to keep the prompt working across models and model releases.

![Overview graphic summarising the costs (tokens, instructions) of agents using TDD, and the benefits of TDD and how they play out inside of the agent loop. The benefits are basically a summary of what is listed in the article.](https://martinfowler.com/articles/exploring-gen-ai/tdd_by_agents_costs_benefits.png)

## My conclusions

I think at this point there is generally more and more evidence that being overly specific about *how* we want a model to do something is not a sustainable approach. Instead, we should find as many ways as we can to monitor the outcomes and give feedback. That feedback should be automated wherever possible, and we need to carefully think about where we insert ourselves as arbiters of what is good and correct.

Even though I am aware that my little eval is far from representing a broad perspective on the effectiveness of TDD, it definitely hasn't given me any new indications that all this effort is worth it. Especially not if we can find other ways to achieve the majority of TDD benefits.

I personally have stopped telling my coding agents to write tests first, let alone do TDD (which I never did, to be honest), until I see evals or other strong arguments that convince me otherwise. I'm trying to focus instead on the benefits of TDD when I use it outside of the agent loop, and exploring alternative ways to achieve them.

### How to get good regression tests?

*...so that the agent and me get signals when existing functionality breaks*

I still care about solid regression tests, because even though an agent can of course fix red tests the wrong way around, at least the red test gives it a feedback signal to double check pre-existing requirements that might have broken. I [monitor and improve regression quality with the help of mutation testing](https://martinfowler.com/articles/sensors-for-coding-agents.html#TheTestSuiteAsARegressionSensor), instead of giving elaborate TDD instructions and hoping for the best.

### How to build regular refactoring into the process?

*...so that the codebase remains easy to change*

Refactoring remains crucial, but the small steps of traditional TDD don't seem to be an efficient or effective way to do it in the agent loop. A few examples of triggers for refactorings: Give the agent [access to static code analysis](https://martinfowler.com/articles/sensors-for-coding-agents.html#StaticCodeAnalysisBasicLinting); run regular [reviews of structure and modularity](https://martinfowler.com/articles/sensors-for-coding-agents.html#StaticCodeAnalysisAiModularityReview); develop team rituals to maintain a good understanding of the codebase and catch drift early; keep an eye on the trend of number of files touched per change, and number of [tokens are for a change](https://martinfowler.com/articles/exploring-gen-ai/refactoring-economic-benefit.html).

### How to get confidence?

*...so that I am not afraid to push to production*

The hardest question remains, how do we get that confidence that TDD was giving us, how do we manage fear, how do we lock in progress? I don't have a clear answer to that, but I'll just mention one of the things that seems like a good building block for that: I have recently tried out the [Approved Scenarios](https://lexler.github.io/augmented-coding-patterns/patterns/approved-scenarios/) approach that Ivett Ördög is advocating for. In my words (don't hold her to it), it's a form of semi-manual testing that is supported by a bespoke test runner for each application. That runner shows me functional test scenarios in an easy to think about way, and allows me to “freeze” expectations (scenarios / fixtures) in that runner after I have thoroughly confirmed them. Whenever those frozen expectations are violated in the future, I have to approve them again. My colleague Matteo Vaccari gave a great overview of [his experiences with that approach here](https://www.youtube.com/watch?v=3tdmoj35HG0).

Whatever ends up giving us trust and confidence in our software in the future - I think the role of TDD as we've known it is significantly smaller than pre-GenAI.

* * *

## Appendix: Results, according to Opus evaluation

You can find the full results [in this repository](https://github.com/birgitta410/tdd-comparisons/).

- NT = No TDD instructions
- T = TDD instructions
- TF = Test-first instructions

### Token usage across all batches

Broken down per task size:

| Task   | NT avg tokens | T avg tokens    | T / NT factor |
| ------ | ------------- | --------------- | ------------- |
| Small  | 119,815 (n=2) | 1,018,245 (n=2) | 8.50x         |
| Medium | 736,486 (n=2) | 2,181,105 (n=6) | 2.96x         |
| Large  | 253,621 (n=2) | 1,239,408 (n=2) | 4.89x         |

Only the medium size were done with added test-first instructions

*Caveat*: these numbers are only a rough proxy for session cost, not a measure of how much code or thinking went into a solution. The setup that recorded the token usage used the [pi-coding-agent](https://github.com/earendil-works/pi-coding-agent) SDK's `getSessionStats()`, which sums `input + output + cacheRead + cacheWrite` usage across every assistant turn in the session. That is a running total across the whole conversation, since every turn re-reads the accumulated context and each of those re-reads (usually mostly served from cache) is counted again in that turn's `cacheRead`. So “Total Tokens” tracks more how many turns a session took, weighted by how large the context had grown by then. It therefore weights cheap cache-read tokens the same as expensive fresh tokens, so it likely overstates TDD's true dollar cost. With these small sample sizes, treat the multipliers as directional: TDD reliably cost several times more, how many times exactly is variable.

### Medium task, round 1

**Task:** Build a 4-stage Python pipeline (parse → aggregate → format → validate) that transforms raw `ROW_ID:CATEGORY:VALUE:PERIOD` strings into a plain-text report.

The numbers

| ID  | TDD | Test Count | Coverage | Mutation Score | Total Tokens | Turns | Tool Calls |
| --- | --- | ---------- | -------- | -------------- | ------------ | ----- | ---------- |
| NT1 | No  | 75         | 100%     | 84.2%          | 769,814      | 31    | 37         |
| NT2 | No  | 107        | 100%     | 89.6%          | 703,159      | 21    | 24         |
| T1  | Yes | 30         | 100%     | 81.0%          | 1,519,762    | 71    | 28         |
| T2  | Yes | 34         | 99%      | 77.3%          | 2,580,897    | 103   | 60         |

The overall verdict

| ID  | TDD | Rank | Verdict                                                                                                                                                                               |
| --- | --- | ---- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| NT1 | No  | 1    | Module-per-stage, dataclasses, \`Decimal\`; no correctness bugs, strongest error handling, only solution checking duplicate ROW\_IDs; validation self-referential but harmless        |
| NT2 | No  | 2    | Module-per-stage, dataclasses, float/round; best-engineered and largest suite, but validator rejects its own valid fractional output (false-rejection bug); TOTAL row never validated |
| T1  | Yes | 3    | Single module, dicts, float; correct core stages but validation is circular (re-runs formatter); accepts \`nan\`/\`inf\`, ignores duplicate ROW\_IDs                                  |
| T2  | Yes | 4    | Single module, dicts, float; active TOTAL-row bug (headcount summed into dollars) enshrined by a test; missing validation check #3 entirely                                           |

(This round's ranking was based on Verdict, test count, coverage and mutation score only — Opus's Design/Code/Test sub-scores were introduced starting with the next round.)

### Medium task, round 2

**Task:** Same 4-stage report pipeline as 01-medium, rerun with stricter TDD adherence and a new test-first variant added (NT1/NT2 reuse the same codebases from 01-medium).

| ID  | Approach   | Test Count | Coverage | Total Tokens | Turns | Tool Calls |
| --- | ---------- | ---------- | -------- | ------------ | ----- | ---------- |
| NT1 | No TDD     | 107        | 100%     | 703,159      | 21    | 20         |
| TF2 | Test-first | 90         | 92%      | 619,531      | 27    | 26         |
| NT2 | No TDD     | 75         | 100%     | 769,814      | 31    | 30         |
| T2  | TDD        | 29         | 98%      | 2,099,280    | 96    | 95         |
| TF1 | Test-first | 62         | 99%      | 268,323      | 17    | 16         |
| T1  | TDD        | 25         | 100%     | 2,017,739    | 90    | 89         |

| ID  | Approach   | Design | Code | Tests | Avg | Impl/Test LOC |
| --- | ---------- | ------ | ---- | ----- | --- | ------------- |
| NT1 | No TDD     | 8      | 8    | 8     | 8.0 | 497 / 881     |
| TF2 | Test-first | 8      | 8    | 7     | 8.0 | 484 / 850     |
| NT2 | No TDD     | 8      | 8    | 7     | 7.5 | 330 / 430     |
| T2  | TDD        | 7      | 7    | 6     | 6.5 | 207 / 304     |
| TF1 | Test-first | 6      | 6    | 6     | 6.0 | 348 / 360     |
| T1  | TDD        | 6      | 6    | 6     | 6.0 | 142 / 228     |

| ID  | Approach   | Rank | Verdict                                                                                                              |
| --- | ---------- | ---- | -------------------------------------------------------------------------------------------------------------------- |
| NT1 | No TDD     | 1    | Deepest suite, cleanest validation reusing formatter's layout; HEADCOUNT mixed into dollar totals unguarded by tests |
| TF2 | Test-first | 2    | \`Decimal\` throughout, strong parse/validate; check 3 is unreachable dead code, validate module cluttered           |
| NT2 | No TDD     | 3    | Clean design, \`Decimal\`, correct parse; thinner tests, one no-op test, validation self-referential                 |
| T2  | TDD        | 4    | Clean happy path, correct formatting; crashes on malformed input instead of returning structured parse errors        |
| TF1 | Test-first | 5    | Strongest parser of the single-file solutions; broken TOTAL row (headcount as dollars), dead scaffolding shipped     |
| T1  | TDD        | 6    | Most compact (142 LOC); crashes on malformed input, most tautological validation, thinnest test suite                |

### Medium task, round 3 (improved TDD instructions)

**Task:** Same 4-stage report pipeline as 01-medium, rerun with an improved TDD prompt (emphasising upfront design and refactoring); NT1/NT2 again reuse the 01-medium codebases.

| ID  | TDD | Test Count | Coverage | Mutation Score | Total Tokens | Turns | Tool Calls |
| --- | --- | ---------- | -------- | -------------- | ------------ | ----- | ---------- |
| T1  | Yes | 51         | 100%     | 90.2%          | 3,447,283    | 117   | 116        |
| NT1 | No  | 107        | 100%     | 89.6%          | 703,159      | 21    | 20         |
| NT2 | No  | 75         | 100%     | 84.2%          | 769,814      | 31    | 30         |
| T2  | Yes | 43         | 100%     | 81.1%          | 1,421,671    | 61    | 60         |

| ID  | TDD | Design | Code | Tests | Avg  |
| --- | --- | ------ | ---- | ----- | ---- |
| --- | --- | ------ | ---- | ----- | ---- |
| T1  | Yes | 8      | 8    | 7     | 7.67 |
| NT1 | No  | 8      | 8    | 6     | 7.33 |
| NT2 | No  | 8      | 7    | 6     | 7.0  |
| T2  | Yes | 7      | 7    | 6     | 6.67 |

| Rank | ID  | TDD | Headline weakness                                                                                 |
| ---- | --- | --- | ------------------------------------------------------------------------------------------------- |
| 1    | T1  | Yes | HEADCOUNT-only TOTAL row printed as \`$\`; validation is a substring check, not arithmetic        |
| 2    | NT1 | No  | Fractional HEADCOUNT → spurious ValidationError on valid input; tests lean on monkeypatching      |
| 3    | NT2 | No  | \`NaN\`/\`Infinity\` crash the pipeline instead of a ParseError; validation re-runs the formatter |
| 4    | T2  | Yes | Validation is tautological (checks aggregate against itself); width check is only a comment       |

### Small task

**Task:** Build a Python module that validates medical appointment slot codes in `DAY-TIME-ROOM-CHECKSUM` format, returning a structured result identifying which rule failed and why.

| ID  | TDD | Test Count | Coverage | Mutation Score | Total Tokens | Turns | Tool Calls |
| --- | --- | ---------- | -------- | -------------- | ------------ | ----- | ---------- |
| NT1 | No  | 61         | 100%     | 89.6%          | 122,108      | 10    | 15         |
| NT2 | No  | 58         | 100%     | 92.3%          | 117,522      | 10    | 20         |
| T1  | Yes | 21         | 100%     | 93.6%          | 894,451      | 55    | 37         |
| T2  | Yes | 20         | 100%     | 93.2%          | 1,142,039    | 68    | 26         |

| ID  | TDD | Design | Code | Tests | Avg  |
| --- | --- | ------ | ---- | ----- | ---- |
| NT1 | No  | 8      | 9    | 9     | 8.67 |
| NT2 | No  | 8      | 8    | 8     | 8.0  |
| T1  | Yes | 7      | 8    | 7     | 7.33 |
| T2  | Yes | 6      | 7    | 7     | 6.67 |

| ID  | TDD | Rank | Verdict                                                             |
| --- | --- | ---- | ------------------------------------------------------------------- |
| NT1 | No  | 1    | Best overall — dataclass result, no bugs, 61 reason-asserting tests |
| NT2 | No  | 2    | Very close — dataclass result, but a Unicode-digit spec deviation   |
| T1  | Yes | 3    | Correct & clean, but dict result + fewer tests + dead code          |
| T2  | Yes | 4    | Weakest design (free-text error) + a genuine crash bug              |

### Larger task

**Task:** Build an in-memory Python loyalty points engine with tiered earn rates (Bronze/Silver/Gold), trailing-365-day spend tracking for tier recalculation, and point redemption.

| ID  | TDD | Test Count | Coverage | Mutation Score | Total Tokens | Turns | Tool Calls |
| --- | --- | ---------- | -------- | -------------- | ------------ | ----- | ---------- |
| NT2 | No  | 69         | 100%     | 86.9%          | 322,148      | 14    | 13         |
| T2  | Yes | 22         | 99%      | 85.6%          | 1,225,517    | 63    | 62         |
| T1  | Yes | 21         | 99%      | 85.2%          | 1,253,300    | 67    | 66         |
| NT1 | No  | 74         | 99%      | 89.4%          | 185,094      | 11    | 9          |

| ID  | TDD | Design | Code | Tests | Correctness | Avg  |
| --- | --- | ------ | ---- | ----- | ----------- | ---- |
| NT2 | No  | 8      | 9    | 8     | 8           | 8.25 |
| T2  | Yes | 7      | 7    | 8     | 8           | 7.5  |
| T1  | Yes | 7      | 7    | 7     | 9           | 7.5  |
| NT1 | No  | 8      | 7    | 6     | 6           | 6.75 |

| ID  | TDD | Rank | Verdict                                                                                                                   |
| --- | --- | ---- | ------------------------------------------------------------------------------------------------------------------------- |
| NT2 | No  | 1    | Only solution with real input validation; precise boundary tests; minor out-of-order purchase edge cases only             |
| T2  | Yes | 2    | Clean typed data model, all core rules correct; no error handling, duplicate purchase ID bug, dead state fields           |
| T1  | Yes | 3    | Most functionally correct (no bugs found on probing); untyped nested dicts, vestigial structure, fewest tests             |
| NT1 | No  | 4    | Highest design score, 74 tests — but two High bugs: wrong batch draw-down order; future-dated points counted as spendable |

**Acknowledgements**

Thanks to Ivett Ördög, Matteo Vaccari, Dan Mutton, Lukasz Plotnicki, and Emily Bache, for taking the time to review, and for the feedback and valuable discussions that helped improve this post.

GenAI was used for research, pulling together ideas into structure, and polishing the language.
