---
title: F-Zero X on Nintendo 3DS
link: https://hypnotize.works/gdx-3ds/postmortem/index.html
source: hnrss-org
published: 2026-10-25T23:27:30Z
updated: 2026-10-25T23:27:30Z
first_seen: 2026-10-26T14:07:38.108821132Z
authors:
- bpierre
content: extracted
html: 2026-10-25-f-zero-x-on-nintendo-3ds.html
remote_preview:
  url: https://hypnotize.works/gdx-3ds/media/v1-menu.jpg
  alt: 'v1.0 on a New 3DS: Select Mode with the Expansion Kit entries, the G-DIFFUSER idle view on the bottom screen'
---

01 · Overview

## F-Zero X on 3DS — Old and New, native, with the 64DD Expansion Kit

A post-mortem of porting G-Diffuser, the F-Zero X decompilation, to the Nintendo 3DS — v1.0.0, covering both the original 24-day sprint that put it on a New 3DS at 60 fps in 3D (2026-08-11 to 09-03) and the 43-day second phase that added Old 3DS support, a native renderer, and the Expansion Kit (2026-09-07 to 10-19). One person directed throughout; one long-lived Claude session orchestrated; 147 subagents built the first phase and many more carried the second.

The console went from a black screen to a native-60 stereoscopic racer, then from a New-3DS-only build to a four-way release covering both consoles with and without the 64DD expansion.

59.6fps

median on hardware, full Grand Prix, New 3DS, end of the first phase (from 12–20 fps in the first emulator race)

51.4fps

tenth-percentile floor in 30-machine crowds, first phase (from 25–35)

53%

of frames at the 60 cap, first phase (from 14% one week earlier)

15

active days across the first phase's 24 calendar days, three pauses when the weekly plan allowance ran out

3.17 B

tokens across all sessions and agents, first phase only; see Cost

$4,974

at published API list prices (Fable $10/$50 per MTok, 1-hour cache writes), first phase only; see Cost

250

commits touching the 3DS port in the first phase, frozen 2026-10-08 from the v0.9.0 docs — not re-verified against final history

15,039

lines under port/3ds plus 48 pure-delta patches, first-phase count

## What v1.0.0 added

3

released console builds from one source tree: Old 3DS, New 3DS, and New 3DS with the Expansion Kit

199

internal progress notes written during the second phase

43days

calendar span of the second phase, 2026-09-07 to 10-19 inclusive

The tiles above are dated. The first phase's hardware numbers and token/cost figures were measured through 2026-09-03 and have not been re-run for v1.0.0; the [Agentic workflow](https://hypnotize.works/gdx-3ds/postmortem/agentic.html) page describes how the second phase was run, and the [Cost](https://hypnotize.works/gdx-3ds/postmortem/cost.html) page is explicit about what it does and does not price.

## On hardware

The clips and photos below are from v0.9 (2026-09), before DASH replaced the debug console on the bottom screen.

[open clip](https://hypnotize.works/gdx-3ds/media/gameplay-race.mp4)\
v0.9: Grand Prix on a New 3DS. The bottom screen is the port's debug menu: 59.8 fps, heap, build id.

[open clip](https://hypnotize.works/gdx-3ds/media/gameplay-handheld.mp4)\
v0.9: handheld, mid-race. Native 60 Hz pacing; no interpolation.

## What was built

Three released console builds from one source tree, all booting from the HOME menu (.cia) or the homebrew launcher (.3dsx): the base game for Old 3DS / 2DS and for New 3DS / New 2DS XL, and a New 3DS build with the 64DD Expansion Kit (course editor and machine creator, playable with a user-supplied disk image). Every build reads prebaked asset archives from the SD card — no ROM or disk image ever ships with the game.

They play the whole game: Grand Prix, Time Trial, Practice, Death Race, and the machine editor screens, with music and effects on the real DSP, stereoscopic 3D on the slider, a bottom-screen dashboard with three HUD layouts and an idle view outside races, and clean HOME suspend, resume, close, lid sleep and power-off. The New 3DS build keeps the native-60 frame pacing the first phase reached; the Old 3DS build gets to a comparable result by a different route. At v1.0.0, with no ini, both builds default to that same route: the native renderer (`[perf] preset = auto`). The New 3DS's first-phase render-thread/per-draw-stereo path is still built in and reachable with an explicit ini, but is no longer the default — see [Frontier](https://hypnotize.works/gdx-3ds/postmortem/frontier.html) and [Pushing the boundaries](https://hypnotize.works/gdx-3ds/postmortem/boundaries.html) for the two builds compared side by side.

### The stack

- **decomp**: the F-Zero X decompilation (inspectredc/fzerox), untouched upstream, plus a patch stack.
- **libultraship**: the N64 runtime (Kenix3), untouched upstream, including a 13-line newlib fix that lets it build on devkitARM; it still draws every menu and non-race screen.
- **port/3ds**: 15,039 lines as of the first phase, since grown: a citro3d rendering backend, stereo, audio, input, menu, lifecycle, render thread, profilers, packaging.
- **the native renderer** (added in the second phase): an owned interpreter that skips the per-frame display-list walk for race geometry, and is the v1.0.0 default on both consoles — see [Frontier](https://hypnotize.works/gdx-3ds/postmortem/frontier.html) for how, and why it is what lets the Old 3DS keep up.

### The headline numbers on hardware, first phase (New 3DS)

| When                                                           | Median | p10                    | At cap |
| -------------------------------------------------------------- | ------ | ---------------------- | ------ |
| 08-14 first race (emulator)                                    | 12–20  | –                      | –      |
| 08-21 first hardware run                                       | 48–50  | –                      | –      |
| 08-28 release-grade build                                      | \~51   | \~40 (crowds, MINIMAL) | –      |
| 09-02 Round 1: Mainline before the campaign                    | 51.3   | 39.2                   | 9%     |
| 09-02 Round 2: Bridge memos (brfast)                           | 57.2   | 46.6                   | 29%    |
| 09-02 Round 3: HUD atlas · triangle memo · TMEM · audio        | 56.0   | 48.0                   | 32%    |
| 09-02 Round 4: Render thread on core 2 (pipe)                  | 57.6   | 44.6                   | 14%    |
| 09-03 Round 5: Ahead mode · bridge on main · auto rival detail | 59.6   | 51.4                   | 53%    |

## The first phase in five lines

1. **08-11 → 08-14: from research to a race.** Feasibility dossier, plan for a team of agents, five parallel foundation streams, six boot bugs, first race at 12–20 fps in the emulator.
2. **08-20 → 08-22: from pink to playable, then to a real console.** Fog, HUD, livery, shadow, sky fixed; the loadblock discovery took menus from 27 to 60 fps; first hardware run 08-21 ("wow it runs amazing"); audio the same evening.
3. **08-27 → 08-28: product grade.** Five-agent fleet drop, stereo armed, touch menu, rival detail, GPU vertex-transform moonshot built and shelved, README with legal notice, release package.
4. **09-01 → 09-02: lifecycle and the last big lever.** HOME crash and close-from-HOME hang fixed; a bridge output cache proven a NO-GO and replaced by memoizing each sub-display-list's classification instead of re-scanning it every frame; LOCKED-60 campaign launched with four parallel levers.
5. **09-02 → 09-03: multi-core.** Render thread on core 2, one-frame-ahead mode, bridge on main, auto LOD; median 59.6 with 53% of frames at cap; crash fixed; viewport placement fixed; stereo markers anchored.

## The second phase in six lines

Not originally planned; it ran 09-07 to 10-19, 43 calendar days.

1. **09-07 → 09-29: scoping, then making it fit.** The Old 3DS milestone, previously written off, was restarted; parallel agents fit the build into its 64 MB app region (the RDRAM arena halved, audio streamed instead of resident, texture uploads capped); on 2026-09-29, in the first hardware session on a real Old 3DS, the shipped (New-3DS-era, 16 MiB RDRAM) build died at boot while the `-DGDX_3DS_O3DS=ON` profile, built to fit the console's memory grant, booted and raced the same session at about 10 fps.
2. **09-29 → 09-30: finding the real costs.** A narrowed audio lock cut the race-wall wait on the Old 3DS by about 20 ms a frame; frame skipping ended the game's accidental slow motion; a first retained-mode renderer won big in the emulator and barely moved the number on hardware.
3. **09-30 → 10-01: the probe that changed the plan.** Hardware performance counters showed the real limit was instruction-cache misses, not instruction count. A pose-replay fallback was built and worked, then was set aside on the user's call: "we should go for a real 60! i still believe it has to be possible."
4. **10-01 → 10-02: the native renderer.** Rebuilt around the cache instead of around instruction counts, one small loop per race category, sized to fit the 16 KB instruction cache. Median frame time on a 2D Grand Prix race reached 16.5 ms.
5. **10-03 → 10-08: 3D at 60, on the Old 3DS too.** A per-eye resubmit with its own swap-pacing mode reached 59–60 fps in 3D on most Grand Prix courses; the heaviest courses (Silence, Sand Ocean) can still trip the frame-skip safety net to a steady 30 fps mid-race.
6. **10-08 → 10-19: the Expansion Kit, a dashboard, and release.** The 64DD Expansion Kit verified tick-for-tick identical under the native renderer and booted on the Old 3DS, which then proved too slow to release it on; the bottom-screen DASH shipped; a standing adversarial review audited the merged result before the v1.0.0 release.

See [Agentic workflow](https://hypnotize.works/gdx-3ds/postmortem/agentic.html) for how the second phase was run and [Old 3DS](https://hypnotize.works/gdx-3ds/postmortem/old-3ds.html) for the full campaign, card by card.

## Read on

- [**Author's notes**](https://hypnotize.works/gdx-3ds/postmortem/author.html): The one page written by a human.
- [**Every step**](https://hypnotize.works/gdx-3ds/postmortem/timeline.html): The chronology, New 3DS to v1.0.0.
- [**Road to 60**](https://hypnotize.works/gdx-3ds/postmortem/road-to-60.html): The New 3DS performance campaign, round by round.
- [**The native fork**](https://hypnotize.works/gdx-3ds/postmortem/native.html): Why it exists, and what it changed.
- [**Old 3DS**](https://hypnotize.works/gdx-3ds/postmortem/old-3ds.html): The road to a real 60 at 268 MHz.
- [**Pushing the boundaries**](https://hypnotize.works/gdx-3ds/postmortem/boundaries.html): Against our New 3DS build and against the field.
- [**Difficulties**](https://hypnotize.works/gdx-3ds/postmortem/difficulties.html): What fought back, and how it was beaten.
- [**Frontier**](https://hypnotize.works/gdx-3ds/postmortem/frontier.html): What this port does that others in its position haven't.
- [**Agentic workflow**](https://hypnotize.works/gdx-3ds/postmortem/agentic.html): How the fleet was run, and how the ship was steered.
- [**Cost**](https://hypnotize.works/gdx-3ds/postmortem/cost.html): Tokens, dollars, subagents.

Every number on these pages has a source: git history, the research documents under `docs/research/`, the hardware logs, or the session transcripts the [Cost](https://hypnotize.works/gdx-3ds/postmortem/cost.html) page counts. Where a value is an estimate (the dollar figures, which apply published API list prices to exact token counts), the assumption is stated next to it.
