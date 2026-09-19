---
title: Brood War Bench
link: https://bw.swerdlow.dev/report
source: hnrss-org-frontpage
published: 2026-09-19T14:44:02Z
updated: 2026-09-19T14:44:02Z
first_seen: 2026-09-19T23:45:30.112448451Z
authors:
- benswerd
summary: 'Article URL: https://bw.swerdlow.dev/report Comments URL: https://news.ycombinator.com/item?id=49766966 Points: 113 # Comments: 62'
content: extracted
html: 2026-09-19-brood-war-bench.html
preview:
  file: 2026-09-19-brood-war-bench.preview-dc041f781b90.webp
  width: 256
  height: 134
  alt: Brood War Bench. Which model wins at Brood War? Codex, Claude Code, and Grok logos alongside a recorded Protoss battle.
  color: '#aaa7a2'
images:
- source: https://bw.swerdlow.dev/social/report-v2.png
  original:
    file: 2026-09-19-brood-war-bench.image-a3be9b448513.png
    width: 1200
    height: 630
  variants:
  - file: 2026-09-19-brood-war-bench.image-5495e962063b.webp
    width: 320
    height: 168
  - file: 2026-09-19-brood-war-bench.image-c05eafac1f78.webp
    width: 640
    height: 336
  - file: 2026-09-19-brood-war-bench.image-589dd93b7c96.webp
    width: 960
    height: 504
  - file: 2026-09-19-brood-war-bench.image-e0b0eafa2f29.webp
    width: 1200
    height: 630
  color: '#f5f3ee'
- source: https://bw.swerdlow.dev/benchmark/codex.svg
  original:
    file: 2026-09-19-brood-war-bench.image-089714a36d2b.png
    width: 12
    height: 12
  color: '#839eff'
- source: https://bw.swerdlow.dev/benchmark/claude-code.svg
  original:
    file: 2026-09-19-brood-war-bench.image-0b9ec8b9ac12.png
    width: 12
    height: 12
  color: '#ce7153'
- source: https://bw.swerdlow.dev/benchmark/grok.svg
  original:
    file: 2026-09-19-brood-war-bench.image-fec46886117c.png
    width: 1024
    height: 1024
  variants:
  - file: 2026-09-19-brood-war-bench.image-92336ebe03de.webp
    width: 320
    height: 320
  - file: 2026-09-19-brood-war-bench.image-beb6655b36ba.webp
    width: 640
    height: 640
  - file: 2026-09-19-brood-war-bench.image-2fe1100ec1a8.webp
    width: 960
    height: 960
  - file: 2026-09-19-brood-war-bench.image-a83f93d305ed.webp
    width: 1024
    height: 1024
  color: '#000000'
---

By [Ben Swerdlow](https://twitter.com/benswerd)

Which model wins at Brood War?

## Key takeaways

- None of the models played beyond a beginner level.
- Codex Astra is the clear leader beating all other models consistently.
- Grok models are not smart enough to play Brood War yet.
- Older models tended to play the RTS as a turn-based game, leading them to get destroyed while they were thinking. Newer models sometimes fell into the same trap, which may explain why some lower-effort settings performed better, but overall were much more cognizant of the cost of thinking.

## Leaderboard

| Rank | System                                                                      | Wins | Losses | APM  | Cost / game | Win rate   |
| ---- | --------------------------------------------------------------------------- | ---- | ------ | ---- | ----------- | ---------- |
| 🥇    | ![](https://bw.swerdlow.dev/benchmark/codex.svg)**Codex Astra / xhigh**     | 18   | 0      | 12.6 | $10.54      | **100.0%** |
| 🥈    | ![](https://bw.swerdlow.dev/benchmark/codex.svg)**Codex Astra / medium**    | 16   | 2      | 17.2 | $15.11      | **88.9%**  |
| 🥉    | ![](https://bw.swerdlow.dev/benchmark/claude-code.svg)**Claude Fable**      | 15   | 3      | 12.6 | $12.24      | **83.3%**  |
| 4    | ![](https://bw.swerdlow.dev/benchmark/codex.svg)**Codex Astra / low**       | 14   | 4      | 25.7 | $21.07      | **77.8%**  |
| 5    | ![](https://bw.swerdlow.dev/benchmark/codex.svg)**Codex 5.6 Sol / medium**  | 13   | 5      | 10.1 | $5.12       | **72.2%**  |
| 6    | ![](https://bw.swerdlow.dev/benchmark/codex.svg)**Codex 5.6 Sol / low**     | 12   | 6      | 18.1 | $9.23       | **66.7%**  |
| 7    | ![](https://bw.swerdlow.dev/benchmark/claude-code.svg)**Claude Opus 5**     | 12   | 6      | 10.5 | $20.78      | **66.7%**  |
| 8    | ![](https://bw.swerdlow.dev/benchmark/codex.svg)**Codex 5.6 Sol / xhigh**   | 11   | 7      | 8.0  | $3.23       | **61.1%**  |
| 9    | ![](https://bw.swerdlow.dev/benchmark/codex.svg)**Codex 5.6 Luna / low**    | 9    | 9      | 23.8 | $0.42       | **50.0%**  |
| 10   | ![](https://bw.swerdlow.dev/benchmark/codex.svg)**Codex 5.6 Terra / xhigh** | 9    | 9      | 15.8 | $2.10       | **50.0%**  |

Brood War Bench started after I built a version of Brood War that you could only play through agents as an experiment to play with friends. I played it with a couple friends who did surprisingly well for people who have only played a couple Starcraft games in their lives. When I asked them why, they said they hadn't done much, they asked their agent to attack and it had built a small army and done the full attack for them. This lead me to wonder how far they can go on their own; this is my answer.

## What I observed

01

### Codex found cheese before it found macro

Codex's strongest recurring idea was disruption. In Protoss games it often sent a Probe across the map to attack workers or buildings. This worked shockingly well as the opposing agents often spent dozens of seconds thinking about what to do about a probe instead of doing anything else.

The same systems were much weaker at sustained production. They delayed tech, trickled one or two basic units into defended bases, and threw workers into last stands.

I also noticed Codex often created separate subagents to manage the economy, army production, and army control. They didn't communicate much with one another, so the army agent often sent each new unit straight into an attack, unaware of the larger army the other agents were planning to build.

This is a common beginner mistake: sending units in one at a time instead of waiting for a critical mass and a planned attack timing. In games where I helped direct Codex, it was much better at planning those moments and getting its subagents to work together.

The persistence was real. In G009, after losing its army and main base, Codex 5.6 Terra / medium lifted its last Command Center and moved it toward the opposite corner. It survived for another six minutes.

Six Probes cross the map

A Probe first, then Zealots in drips

The last Command Center runs

02

### Grok spent the game between actions

Grok 4.6 frequently produced long stretches of reasoning and very few command batches. In G043, the xhigh run logged 11,138 reasoning tokens but issued only six command batches across 43 minutes and never fielded a combat unit.

The actions it did take rarely developed into a working control loop. In G003, Grok / xhigh made three Marines and never reached the enemy base. In G002, Grok / medium made two Zealots and also never crossed the map. These looked less like bad strategies than failures to keep observing and acting.

Forty-three minutes, no army

03

### Fable earnestly tried to play the game

I found myself rooting for Claude Fable in more than a few games. Fable usually tried to build an economy and climb the tech tree instead of stopping at the first unit available. It seemed more interested in actually playing the game than any of the other models.

In G007 it reached a Lair, Spire, and Mutalisks and won. In G027 it added a Robotics Facility, Citadel of Adun, Observatory, and Templar Archives before winning. Ambition did not guarantee execution: in G036 Fable reached a Factory and Academy but Opus 5 overran it.

Fable gets Mutalisks

Fable keeps climbing

The build does not become an army

### No agent here played beyond beginner level

Even Astra and Fable were unable to build complex army's, defend simple attacks or play concrete strategies. A beginner playing photon rush would win every single one of these games.

That said, watching the agents play made me more excited than I have been in a while. This benchmark is nowhere near exhausted. There is much more for the agents to learn, and much more for the benchmark to ask them to do. I look forward to watching them get there.

## How the games developed

Compare Race Game time

**5:00** / 15:00

### Technology investment

Completed research + upgrade levels

Codex Astra**0.3** Claude Fable**0.2** Grok 4.6**0**

### Workers

Completed workers alive

Codex Astra**14.7** Claude Fable**16.7** Grok 4.6**7.4**

### Army size

Completed army and support units

Codex Astra**8.3** Claude Fable**7.6** Grok 4.6**2**

### Structures

Completed buildings, including add-ons

Codex Astra**6.2** Claude Fable**7.4** Grok 4.6**4.5**

### Minerals in the bank

Unspent minerals, not income

Codex Astra**240.6** Claude Fable**248.6** Grok 4.6**536.6**

### Gas in the bank

Unspent gas, not income

Codex Astra**151** Claude Fable**326.9** Grok 4.6**244.5**

### Supply used

Includes production in progress

Codex Astra**26.5** Claude Fable**26.6** Grok 4.6**11.3**

### When games ended

Share of games ending per 5-minute window

Time-series charts show means of recorded player-runs at each game time. Finished games drop out; missing samples are not filled. Models pool their effort settings. Units and buildings count only once completed; army excludes workers, Overlords, eggs, larvae, and ammunition.

Average cost per game, using the same prices as the leaderboard. Codex and Sonnet costs are token-based estimates.

![](https://bw.swerdlow.dev/benchmark/codex.svg)Codex ![](https://bw.swerdlow.dev/benchmark/claude-code.svg)Claude ![](https://bw.swerdlow.dev/benchmark/grok.svg)Grok

## How the benchmark ran

We built a round-robin matrix of model and effort configurations and had every configuration play every other. The harness ran those matchups in parallel across Freestyle VMs, saving game-engine data and both agents' harness logs for each match.

## Head-to-head matrix

Read across a row. W is a win, L is a loss, and T is a match that reached the benchmark time limit.

Open the full 19 × 19 matrix

W win L loss T time limit

| System                         | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 |
| ------------------------------ | - | - | - | - | - | - | - | - | - | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- |
| 1**Codex Astra / xhigh**       | - | W | W | W | W | W | W | W | W | W  | W  | W  | W  | W  | W  | W  | W  | W  | W  |
| 2**Codex Astra / medium**      | L | - | W | W | W | W | W | W | W | W  | W  | W  | L  | W  | W  | W  | W  | W  | W  |
| 3**Codex Astra / low**         | L | L | - | W | W | W | W | W | W | W  | W  | W  | L  | L  | W  | W  | W  | W  | W  |
| 4**Codex 5.6 Sol / xhigh**     | L | L | L | - | L | W | W | W | L | W  | W  | W  | L  | L  | W  | W  | W  | W  | W  |
| 5**Codex 5.6 Sol / medium**    | L | L | L | W | - | W | W | W | L | W  | W  | W  | L  | W  | W  | W  | W  | W  | W  |
| 6**Codex 5.6 Sol / low**       | L | L | L | L | L | - | W | W | W | W  | W  | W  | W  | W  | L  | W  | W  | W  | W  |
| 7**Codex 5.6 Luna / xhigh**    | L | L | L | L | L | L | - | W | L | W  | L  | W  | L  | L  | L  | W  | W  | W  | W  |
| 8**Codex 5.6 Luna / medium**   | L | L | L | L | L | L | L | - | L | L  | L  | L  | L  | W  | W  | W  | W  | W  | W  |
| 9**Codex 5.6 Luna / low**      | L | L | L | W | W | L | W | W | - | W  | L  | L  | L  | L  | L  | W  | W  | W  | W  |
| 10**Codex 5.6 Terra / xhigh**  | L | L | L | L | L | L | L | W | L | -  | W  | W  | L  | W  | W  | W  | W  | W  | W  |
| 11**Codex 5.6 Terra / medium** | L | L | L | L | L | L | W | W | W | L  | -  | L  | L  | L  | W  | W  | W  | W  | W  |
| 12**Codex 5.6 Terra / low**    | L | L | L | L | L | L | L | W | W | L  | W  | -  | L  | L  | W  | W  | W  | W  | W  |
| 13**Claude Fable**             | L | W | W | W | W | L | W | W | W | W  | W  | W  | -  | L  | W  | W  | W  | W  | W  |
| 14**Claude Opus 5**            | L | L | W | W | L | L | W | L | W | L  | W  | W  | W  | -  | W  | W  | W  | W  | W  |
| 15**Claude Sonnet**            | L | L | L | L | L | W | W | L | W | L  | L  | L  | L  | L  | -  | W  | W  | W  | W  |
| 16**Claude Haiku**             | L | L | L | L | L | L | L | L | L | L  | L  | L  | L  | L  | L  | -  | L  | T  | T  |
| 17**Grok 4.6 / xhigh**         | L | L | L | L | L | L | L | L | L | L  | L  | L  | L  | L  | L  | W  | -  | W  | T  |
| 18**Grok 4.6 / medium**        | L | L | L | L | L | L | L | L | L | L  | L  | L  | L  | L  | L  | T  | L  | -  | W  |
| 19**Grok 4.6 / low**           | L | L | L | L | L | L | L | L | L | L  | L  | L  | L  | L  | L  | T  | T  | L  | -  |

## Play your own match

Bring your agent and play Brood War with friends.

[Play Brood War](https://bw.swerdlow.dev/)
