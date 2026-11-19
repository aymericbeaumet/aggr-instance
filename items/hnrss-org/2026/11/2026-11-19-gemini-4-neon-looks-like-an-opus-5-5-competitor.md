---
title: Gemini 4 Neon looks like an Opus 5.5 competitor
link: https://micjm.com/blog/gemini-4-neon-looks-like-a-top-model/
source: hnrss-org
published: 2026-11-19T00:29:45Z
updated: 2026-11-19T00:29:45Z
first_seen: 2026-11-19T07:50:29.493483202Z
authors:
- seizethecheese
content: extracted
html: 2026-11-19-gemini-4-neon-looks-like-an-opus-5-5-competitor.html
remote_preview:
  url: https://micjm.com/images/gemini-4-neon-looks-like-a-top-model/calibration.png
  alt: Win rate vs. Projected win rate
---

*TLDR: we project Gemini 4 Neon’s performance based on what a faster Gemini 4 Carbon might score and find it equivalent to Opus 5.5*

Gemini 4 Carbon is the best model on the [Agent Duel leaderboard](https://agentduel.com/leaderboard?excludeEarlyVotes=true) if you exclude early voting. Carbon’s main problem is that it is [slooooow](https://micjm.com/blog/carbon-is-that-slow/).

Today, [Gemini 4 Neon was released](https://blog.google/innovation-and-ai/models-and-research/gemini-models/introducing-gemini-4-neon/), and looks to be an incremental benchmark improvement with a very large speed improvement. Earlier today, it was being served extremely fast via OpenRouter: [127 tokens per second](https://openrouter.ai/google/gemini-4-neon). This is way faster than [Carbon at 46 tokens per second](https://openrouter.ai/google/gemini-4-carbon).

This got me wondering, could Neon be not just a good model but the best model?

## Modeling our leaderboard

To investigate, I modeled the win rate in our leaderboard. The best model has three components, model identity, speed and whether the user voted early. My initial modeling did poorly, so I added an interaction between speed and early voting. This ended up working well.

[![Win rate vs. Projected win rate](https://micjm.com/images/gemini-4-neon-looks-like-a-top-model/calibration.png)](https://micjm.com/images/gemini-4-neon-looks-like-a-top-model/calibration.png)

And surprisingly, speed seems to be pretty much as important as model identity (another blog on that soon).

[![Feature importance](https://micjm.com/images/gemini-4-neon-looks-like-a-top-model/feature-importance.png)](https://micjm.com/images/gemini-4-neon-looks-like-a-top-model/feature-importance.png)

## Projecting Gemini 4 Neon performance

If you assume the same performance as Gemini 4 Carbon (which is reasonable given even better benchmark scores), and [127 tps](https://openrouter.ai/google/gemini-4-neon), we would project an Elo on the [Agent Duel leaderboard](https://agentduel.com/leaderboard) of 1,066, just below Opus 5.5 at 1,078 in our later analysis snapshot.

[![Synthetic Neon Elo projection](https://micjm.com/images/gemini-4-neon-looks-like-a-top-model/synthetic-elo.png?v=127)](https://micjm.com/images/gemini-4-neon-looks-like-a-top-model/synthetic-elo.png?v=127)

The 90% interval for the projected Elo difference between Gemini 4 Neon and Opus 5.5 is -94 to +68, with Neon projected to win 48.3% of the time against Opus (counting ties as half a win).
