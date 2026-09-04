---
title: Making Fable Cheaper Than Opus
link: https://cognition.com/blog/making-fable-cheaper-than-opus
source: cognition-com-blog
published: 2026-07-13T17:00:00Z
first_seen: 2026-09-04T12:14:41.802665207Z
content: extracted
html: 2026-07-13-making-fable-cheaper-than-opus.html
---

By Joon Hee Lee07.13.26

We replaced Opus 4.8 with Fable 5, and Devin’s bill went down.

Fable 5 costs twice as much per token as Opus 4.8. But when we ran both models on FrontierCode 1.1 using our new Fusion architecture, Fable cost less. Unsurprisingly, it also scored higher. This post explains why, and what that means for pricing agentic work.

## [Introduction](https://cognition.com/blog/making-fable-cheaper-than-opus#introduction)

Everyone who runs coding agents knows that stronger models give you better results, but you eat the cost.

When we introduced [Devin Fusion](https://cognition.com/blog/devin-fusion), we showed a way out: keep a frontier model in charge, let it delegate to a cheaper and faster sidekick, and you get frontier-level performance at 60% lower cost.

But once the lead model delegates most of the work, does its price-per-token still dominate the bill? Fable 5 costs 2x more per token than Opus 4.8, so a Fable-led agent should cost more. To find out, we ran 3,000 evaluation sessions on [FrontierCode 1.1](https://cognition.com/blog/frontier-code-1.1) across four configurations: Fable and Opus in the lead seat, each with and without the same cheap sidekick.

The pure runs behave exactly as intuition would suggest: Fable outscores Opus (60.8 vs 55.4) and costs more. Better model, bigger bill.

The sidekick-enabled runs are where things get interesting.

#### FrontierCode 1.1 ExtendedScore vs Cost

[See how models rank on the FrontierCode leaderboard](https://cognition.com/frontiercode)

Given the same sidekick, the cost ordering reverses: **Fable + Sidekick costs less than Opus + Sidekick ($1.86 vs. $2.04), while scoring higher (60.7 vs. 54.6).** Compared with pure Fable, Fable + Sidekick cuts cost by 54% while leaving the score nearly unchanged.

| Config                       | Score | Cost/run (mean) |
| ---------------------------- | ----- | --------------- |
| Fable 5 (low) + Sidekick     | 60.7  | **$1.86**       |
| Opus 4.8 (medium) + Sidekick | 54.6  | $2.04           |
| Fable 5 (low)                | 60.8  | $4.03           |
| Opus 4.8 (medium)            | 55.4  | $3.06           |

The 2x per-token premium turns out to be the wrong number to look at. The cost of an agent is dominated by how many turns the lead model takes, how much context it drags along, and above all, what it decides *not* to do itself. The difference comes down to management style: **Opus behaves like a micromanager with an intern; Fable is a manager with a capable engineer.**

## [The setup](https://cognition.com/blog/making-fable-cheaper-than-opus#setup)

A quick refresher on how Fusion’s sidekick architecture works. The lead agent owns the session: it talks to the user, plans, reviews the work, and commits. It also has a persistent `sidekick` subagent for delegating tasks. The lead writes a handoff brief in plain language, and the subagent, powered by a much cheaper model, carries it out in its own context and reports back. The lead reviews the result and decides what happens next.

To find out where the cost goes, we did two things. First, we parsed every LLM call across all 3,000 sessions: which model was speaking, what tool it called, how many tokens it read and wrote, and what each call cost. Second, we picked 40 tasks for a closer look: the ones where Fable was dramatically cheaper, the ones where Opus was, and another random sample from the middle. For each one, we analyzed the Fable-led run against the Opus-led run side by side, examining the trajectories and observing where the dollars went.

## [Cost of an agent](https://cognition.com/blog/making-fable-cheaper-than-opus#cost-of-an-agent)

Here's how cost splits between the lead and the sidekick in our experiment:

|                  | Lead $ | Sidekick $ | Total $/run | Lead turns/run | Lead input tokens (cumulative) |
| ---------------- | ------ | ---------- | ----------- | -------------- | ------------------------------ |
| Fable + Sidekick | $1.28  | $0.58      | $1.86       | 11.5           | 545k tok                       |
| Opus + Sidekick  | $1.73  | $0.31      | $2.04       | 26.5           | 1,679k tok                     |

Fable spends more on its sidekick than Opus does — $0.27 more per run. But it spends $0.45 less on *itself*. Fable's lead takes 11.5 turns per run to Opus's 26.5, writes a third of the output tokens (6.1k vs 19.0k), and consumes a third of the input tokens. Fable is significantly more expensive per token, but wins on context management and turn count.

Fable’s token savings arise from avoiding work outright. Interestingly, **in 81% of Fable-led runs, the lead never makes a single code edit**. For Opus, that’s true for only 24% of the runs. In 13% of the Fable-led runs, the lead never even reads a repo file itself.

## [A micromanager with an intern vs a manager with an engineer](https://cognition.com/blog/making-fable-cheaper-than-opus#management-styles)

Here's what makes the gap interesting: **both leads delegate the same number of times**, about 3 handoffs per run. The per-call logs debunk the easy explanation that Fable simply delegates more. What differs is *when* and *what* they delegate. Fable’s first handoff comes early. Opus often delegates late, after a long stretch of solo exploration and implementation; by then, the design decisions are made, the important files are in its context, and the expensive work is done.

#### A manager with an engineer vs a micromanager with an intern

The same task, the same sidekick. What differs is how much the lead trusts its sidekick: when it delegates, and what it keeps for itself.

##### Fable 5 + Sidekick

lead (Fable 5)sidekick

User task

Explores repo, maps OIDC SSO, reports paths + snippets

Reviews report, writes spec-quality brief

Implements + tests + fixes lint

Reviews diff, gives feedback

Simplifies the change, re-runs tests

Final review · commit

##### Opus 4.8 + Sidekick

lead (Opus 4.8)sidekick

User task

Quick repo survey

Re-reads 12 files the sidekick already summarized (7 actions)

Design decisions

Implements + debugs by hand (11 actions)

Types out remaining files dictated by the lead

Pulls files back into context, corrective edits

Runs the test suite

Commit

Both leads delegate 3 times. Fable's first handoff comes early; Opus delegates the mechanical tail after the expensive work is done.

A typical Fable-led run takes a few reconnaissance actions on the repo, then writes one spec-quality brief delegating the entire implement + test + lint loop. Then one `git show` to review the diff, and a commit.

A typical Opus-led run goes through 20–45 turns of solo exploration, design, and implementation, and one late handoff for the mechanical tail.

Sometimes Fable's *first action* in a session is a handoff. On the same task, the two leads opened like this:

FABLEaction #1 — sidekick handoff

"Explore the repo to map out how OIDC SSO is implemented... Don't change anything yet; just report back with file paths and relevant snippets."

OPUSsame task — after receiving the same kind of sidekick report

"Let me read the key files myself to make the design decisions."

\[proceeds to re-read ~12 files the sidekick had already summarized\]

The obvious fix is to make Opus delegate more exploration, but forcing that behavior tends to decrease performance. Knowing when an investigation is safe to hand off and when it’s something you need to do yourself is itself a judgment call. A model coerced into delegating doesn’t gain that judgment; it just delegates the wrong things.

Each model’s management style also reveals itself in the handoff briefs themselves. When Opus delegates implementation, it dictates, while Fable writes a design doc:

FABLE— implementation handoff (hashing task)

"operator() must be O(1) in pointer length: NO full token scan...

\[algorithm, constraints, test matrix\]

...Report back with the full diff and test results BEFORE committing."

OPUS— implementation handoff (changesets migration)

"Overwrite .changeset/config.json with exactly: {...}"

\[full file contents dictated inline; complete code for every file\]

Delegation doesn't just move cost around; it also changes the quality of the work. The hashing task above is a stark example. The task spec required a hash function to be O(1) in the length of the pointer. Opus implemented it by hand and never wrote that requirement down anywhere. Somewhere along the way it forgot the constraint and shipped a linear-time implementation, which scored 25. In contrast, Fable delegated using high-level constraints. Its brief said, "operator() must be O(1) in pointer length: NO full token scan." The sidekick implemented this successfully for a score of 94.

We found that this pattern generalizes across tasks. Fable’s handoffs enumerated constraints, edge cases, and a definition of “done”, saving itself effort while enabling the sidekick to cheaply and correctly complete the implementation.

## [After the handoff](https://cognition.com/blog/making-fable-cheaper-than-opus#after-the-handoff)

The other half is what the lead agent does with the work that comes back from the sidekick. Both leads often run the same cheap check: two or three `git diff`/`git show` calls. But Opus doesn't stop there. It pulls the sidekick's files back into its own context 2x more often and makes 4x more corrective edits at lead prices. In the extreme case, it reverted the sidekick's work and rewrote it by hand:

FABLE— reviewing the sidekick's diff, finds it over-engineered

"Try simpler alternatives in this order and keep the first that passes."

\[issues a second handoff; the sidekick fixes it\]

OPUS— same complaint, same task

"This is over-engineered... ugly cast."

\[reverts the change; reimplements it itself at lead prices\]

Opus’s distrust doesn’t increase correctness, either. In some eval tasks, Fable's single diff review caught real sidekick bugs, and opted for another cheap handoff, instead of the lead-level rewrite Opus so often reaches for.

## [When delegation doesn’t help](https://cognition.com/blog/making-fable-cheaper-than-opus#when-delegation-doesnt-help)

Fable’s delegation strategy is not universally useful; it fails when the task does not have delegable components. The following kinds of tasks seemed hard to decompose:

- Short tasks that include only a handful of lead-model turns with nothing to delegate between deciding and shipping.

- Serial debugging tasks where the root-cause hunt is one long chain of judgments. Here, the accumulated context *is* the work.

Notably, on these tasks Fable barely delegates at all. The same judgment that writes a good brief also knows when not to write one. But when a task offers nothing worth handing off, delegation has no leverage over cost.

In production, Fusion handles this at another layer: delegation controls what work stays with the expensive model, while routing decides whether the expensive model is involved at all.

## [Closing](https://cognition.com/blog/making-fable-cheaper-than-opus#closing)

We started this experiment expecting to measure how much Fable’s 2x premium would increase cost. We were surprised to find that Fable’s effective delegation actually *decreased* cost overall. It specified constraints and outcomes instead of spelling out the implementation, gave feedback instead of making fixes itself, and in most cases never touched the code at all. These are the habits of a good manager.

As sidekick models get cheaper and better, more of the work can be handed to them. What will remain worth frontier prices is judgment: what to build, what to constrain, and who should write it.
