---
title: GPT-5.6 Sol can beat Factorio on peaceful mode
link: https://ryanmadden.net/gpt-56-beats-factorio/
source: hnrss-org
published: 2026-10-31T18:50:34Z
updated: 2026-10-31T18:50:34Z
first_seen: 2026-10-31T22:46:45.557065792Z
authors:
- ryanmadden
content: extracted
html: 2026-10-31-gpt-5-6-sol-can-beat-factorio-on-peaceful-mode.html
remote_preview:
  url: https://ryanmadden.net/img/factorio_rocket_launch_social.jpg
---

Edit: Woah, this took off! If you want more details you can subscribe at the bottom of the post or [on YouTube](https://www.youtube.com/watch?v=kLuSDUzKYZs). I’m working on a more detailed highlights/commentary post and video.

*Previously: [We got Claude Code playing Factorio](https://ryanmadden.net/claude-plays-factorio/), [Towards beating Factorio autonomously](https://ryanmadden.net/towards-automating-factorio/), and [Faketorio: an agent testbed](https://ryanmadden.net/faketorio-an-agent-testbed/)*

I got GPT-5.6 Sol, running in Codex with low thinking level, to complete a peaceful (i.e. no biters) Factorio run in 13.5 hours of game time:

![](https://ryanmadden.net/img/factorio_rocket_launch.webp)

A timelapse of the full run is available [on YouTube](https://www.youtube.com/watch?v=kLuSDUzKYZs).

Similar to my [previous post](https://ryanmadden.net/claude-plays-factorio/), this run used a game manager to allow Sol/Codex to interact with the game via a small library (though in this run it was Python rather than Bash). The library allows the model to play the game programmatically while enforcing the same restrictions that human players face when using the GUI (e.g. reach of the player, fog of war).

Observing the run was fascinating: Sol exceeded my expectations in some areas while also showing clear weaknesses in others. I have the entire run recorded and am working on a commentary video, but in the meantime here’s a small timelapsed segment showing some parts of the factory:

Besides the screencast, my run manager also captured production statistics throughout the game. Here’s a chart of total resource production, annotated with science research milestones:

![](https://ryanmadden.net/img/factorio_production_stats.png)

I had planned to terminate the run after blue science research, but Sol’s surprising effectiveness convinced me to keep it going. Unfortunately, the scaling issues I thought would occur in the blue science stage appeared later and slowed Sol’s relative progress significantly.

The rocket launch was only achieved through significant ‘manual’ effort on Sol’s part as it papered over its late-game failures to connect certain production systems by repeatedly ferrying supplies between areas of the factory.

Still, I was pleased with the improvement over previous attempts with GPT-5.5 which suffered from the model’s failures to use transport belts at all.

I’m optimistic about autonomous Factorio play as a medium for building intuition about the nature of model capabilities. In many ways playing Factorio is analogous to programming but with the benefit of being much more legible to casual observers, making it convenient for grounding discussion of model behaviors.

I look forward to testing other models and thinking levels, as well as non-peaceful runs. Stay tuned!
