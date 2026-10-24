---
title: 'Show HN: Gemini 4 Argon plays StarCraft'
link: https://starskirmish.com/bench/v0-2/
source: hnrss-org
published: 2026-10-16T14:18:44Z
updated: 2026-10-16T14:18:44Z
first_seen: 2026-10-16T18:46:36.999527378Z
authors:
- __cayenne__
summary: 'I''ve refreshed StarSkirmish Bench, a benchmark where LLMs play StarCraft BW by writing code, with results from the flagship models of OpenAI, Anthropic, Google, and Muse Comments URL: https://news.ycombinator.com/item?id=50118688 Points: 122 # Comments: 57'
content: extracted
html: 2026-10-16-show-hn-gemini-4-argon-plays-starcraft.html
---

## TL;DR

- StarSkirmish is a benchmark where LLMs write bots for StarCraft: Brood War
- Each LLM gets one hour to write a Protoss bot in C++, which is then evaluated against other LLMs' bots and established human written bots
- StarSkirmish tests long horizon reasoning and agentic coding as LLMs must incorporate learnings from practice games to improve their StarCraft bot

The StarSkirmish Bench evaluates how strong of a StarCraft bot can each LLM write given 1 hour of wall clock time. v0.2 updates [StarSkirmish Bench v0.1](https://starskirmish.com/bench/v0-1/) with flagship models from OpenAI, Anthropic, Google and Meta, each running in its lab's 1st party CLI harness.

An LLM’s score reflects its win rate against a roster of competitive human written bots and demo bots.

### StarSkirmish Bench v0.2

1 hour runs, in its 1st party CLI harness. Scores are average win rates (0–100) of each model's 5 runs against a roster of competitive human written bots.

Gemini 4 Argon runs at `high`, the highest reasoning effort available to it; every other model runs at `max`.

GPT-6 Astra is the top scoring StarSkirmish Bench v0.2 LLM, a clear step ahead of GPT-6.1 Sol in second. Gemini 4 Argon, Claude Opus 5.5 and Claude Fable 5.1 are functionally tied behind them, all scoring between 37 and 39.

### StarSkirmish Bench vs. Cost

StarSkirmish Bench score against the average API cost of one 1 hour run. Cost is on a log scale.

Muse Wonder Full's pricing is based on Muse Wonder's API pricing.

GPT-6.1 Sol offers uniquely good value: it is the only LLM in the most attractive quadrant, scoring second at just over a tenth of GPT-6 Astra's cost per run.

## [No Signs of Saturating](https://starskirmish.com/bench/v0-2/#no-signs-of-saturating)

The strongest human written bots are still well ahead of every model. Against Stardust, PurpleWave and BananaBrain, the best model, GPT-6 Astra, wins only 7% of its games.

### Win rates vs the top human written bots

Average win rate of each model's 5 bots against top human written bots Stardust, PurpleWave, and BananaBrain.

1 hour reasoning limits are a factor, but these matches are played on three 2 player maps. Real human written bot tournament conditions are significantly harder.

## [New AI Model Highlights](https://starskirmish.com/bench/v0-2/#new-ai-model-highlights)

Google and Meta's new flagships replace v0.1's Gemini 3.8 Flash and Muse Spark 1.3: Gemini 4 Argon places 3rd, and Muse Wonder Full 7th. GPT-6 Astra and Claude Opus 5.5 were covered in [v0.1](https://starskirmish.com/bench/v0-1/#model-breakdown).

## [AI Model Behaviors](https://starskirmish.com/bench/v0-2/#ai-model-behaviors)

Each run's agent trace records how it spent its hour. The strongest predictor of a model's score is how much it practiced against the 7 strongest bots on the roster.

### Win Rates vs. Practice Against Strong Bots

StarSkirmish Bench score against the number of practice games a model's median run played against the 7 strongest roster bots during the hour. The dashed line is a linear fit across the 9 models.

GPT-6 Astra, GPT-6.1 Sol and Gemini 4 Argon start playing these bots early in the hour, while their own bots still lose to them. Muse Wonder Full, Claude Haiku 5.5 and GPT-6 Luna rarely or never do. Claude Fable 5.1 is the outlier: it scores 37 while practicing almost entirely against weaker bots.

### Run-to-Run Variance

Win rates of each model's 5 runs. Each dot is one run's final bot; the line spans the lowest to the highest run, the bar marks their mean (the model's Bench score) and the band one standard deviation (σ).

Results also vary between runs of the same model. Gemini 4 Argon varies far more than the other labs' flagships: its best and worst runs are 18 points apart, against 9 for GPT-6 Astra and 8 for Claude Opus 5.5.

## [Implementation Details](https://starskirmish.com/bench/v0-2/#implementation-details)

## [Objective](https://starskirmish.com/bench/v0-2/#objective)

Each LLM writes a Protoss bot in C++ against [BWAPI 4.4.0](https://github.com/bwapi/bwapi), played on [OpenBW](https://github.com/OpenBW/openbw). Games are Protoss versus Protoss on three ladder maps: Heartbreak Ridge, Benzene and Destination. A game ends when one side's buildings are all destroyed. Games that reach the 60 minute cap (86,400 frames) are decided as on the [BASIL](https://www.basil-ladder.net/ranking.html) ladder: the higher kills + razings score wins.

The LLM starts from a skeleton bot that only mines, with the source code of the three demo bots and two standard bot libraries: FAP, a combat simulator, and BWEM, a map analyzer. Whatever is in its bot folder when the hour ends is its final bot, so it has to keep the bot compiling.

The LLM has tools to compile its bot, play practice games, and read each game's summary, transcript and its bot's own log. Practice opponents are grouped into tiers: D (the three demo bots), C and B (three mid-ranked human written bots each), A (BananaBrain, Locutus) and S (Stardust, PurpleWave).

## [Tournament Setup](https://starskirmish.com/bench/v0-2/#tournament-setup)

LLM bots

Opponents

Games

Games / pair

Each model's 5 final bots play each of the 13 roster bots, the 10 competitive human written bots and the 3 demo bots, 6 times on each map, on fresh seeds. Unlike v0.1, the LLM bots don't play each other.

## [Harness and Reasoning](https://starskirmish.com/bench/v0-2/#v01-vs-v02)

[StarSkirmish Bench v0.1](https://starskirmish.com/bench/v0-1/) ran every model in the same harness at 'high' reasoning. v0.2 runs each model in its lab's 1st party CLI harness at the highest reasoning setting it offers.

### Harness

v0.1 ran every model in one neutral 3rd party harness; v0.2 runs each lab's models in its 1st party CLI harness. Both versions give each model 1 hour.

v0.1

v0.2

![](https://starskirmish.com/bench/labs/claude.svg)Claude Code*Claude Opus 5.5, Fable 5.1, Sonnet 5.5, Haiku 5.5*

![](https://starskirmish.com/bench/labs/openai.svg)Codex*GPT-6 Astra, GPT-6.1 Sol, GPT-6 Luna*

![](https://starskirmish.com/bench/labs/gemini.svg)Antigravity CLI*Gemini 4 Argon*

![](https://starskirmish.com/bench/labs/meta.svg)Muse Code*Muse Wonder Full*

CLI versions: Claude Code 2.1.296, Codex 0.162.1, Antigravity CLI 1.3.5 and Muse Code 1.4.7.

### Reasoning Effort

v0.1 ran every model at high reasoning effort; v0.2 runs them at max.

v0.1

*Low Medium High XHigh Max*

High

v0.2

*Low Medium High XHigh Max*

Max

Gemini 4 Argon runs at `high`, the highest effort supported.

Every v0.2 harness compacts the model's context at about 250k tokens. When a conversation nears that point, the CLI condenses it and the model carries on. Codex, OpenAI's CLI, for example, gives the GPT models its default 272k token window and triggers compression once 90% filled.

The CLIs' web search and web fetch tools are turned off, and the agent has no other internet access. The CLI is installed before the clock starts, and the hour counts from when the agent starts. There is no submit tool: the harness collects the bot when the time runs out.

## [Scoring](https://starskirmish.com/bench/v0-2/#scoring)

v0.2 replaces v0.1's Elo ratings with plain win rates. A bot's score is its win rate over its tournament games, and a model's StarSkirmish Bench score is the average of its 5 bots' scores, from 0 (every game lost) to 100 (every game won).

Because every bot plays the same fixed roster, its games don't depend on the other LLM bots in the field. They all run in parallel, and a new model's bots can be scored without replaying anyone else's. A fixed roster also keeps the bar the same over time, so scores from different model generations are more directly comparable than Elo ratings fitted within one field.

## [Benchmark Correlations](https://starskirmish.com/bench/v0-2/#correlations)

StarSkirmish no longer tracks the most widely reported evals. It barely moves with the Artificial Analysis Intelligence Index (ρ 0.26), where GPT-6 Astra, the top model here, only ties for 4th of these 9 models. It has also drifted away from Terminal-Bench 4.0, which it tracked closely in v0.1. Claude Sonnet 5.5 and Muse Wonder Full both rank near the top of Terminal-Bench 4.0 but near the bottom here.

### Benchmark Correlation

Each model's StarSkirmish Bench score against its Vals IOI and WeirdML v3 scores, with the Spearman ρ between them. The dashed line is the best straight-line fit.

WeirdML v3 scores 7 of the 9 models: Gemini 4 Argon and Muse Wonder Full have not been included at time of writing.

StarSkirmish still tracks Vals IOI, olympiad programming problems solved in C++, and WeirdML, an independent benchmark of unusual machine learning coding tasks. One reading is that the most heavily benchmarked evals have become targets that models are tuned for, while less targeted tests of long horizon, difficult coding tasks still move together.
