---
title: Dungeon Generation in the Gnomish Mines of NetHack
link: https://www.boristhebrave.com/2026/10/18/dungeon-generation-in-the-gnomish-mines-of-nethack/
source: hnrss-org
published: 2026-10-19T16:06:40Z
updated: 2026-10-19T16:06:40Z
first_seen: 2026-10-22T23:57:38.352236389Z
authors:
- ibobev
labels:
- gamedev
- level generation
- procgen
content: extracted
html: 2026-10-19-dungeon-generation-in-the-gnomish-mines-of-nethack.html
remote_preview:
  url: https://www.boristhebrave.com/wp-content/uploads/2026/10/gnomish-mines-header.png
---

Ok, this one is really best explained with a visualiser:

NetHack’s level generation has a lot of interesting things, but I wanted to focus on the [Gnomish Mines](https://nethackwiki.com/wiki/Gnomish_Mines) because of their use of Cellular Automata. This gives it an organic caves feel, while the majority of the game is spent in square rooms.

It’s very straightforward:

- Randomly pick 40% of the cells to be floor
- Apply four passes of Cellular Automata
- Fix up disconnected caves with extra corridors

Let’s dive into the cellular automata specifically. There’s only two states – floor or rock. And each pass works by counting how many floor tiles are in the 8-neighbor hood of each cell.

Pass 1: 0-2 → rock; 3-4 → unchanged, 5-8 → floor\
Pass 2: 0-4 → unchanged; 5 → rock; 6-8 → unchanged\
Pass 3: 0-2 → rock; 3-8 → unchanged.\
Pass 4: *same as pass 3*

Interestingly, pass 1 is unbuffered. Each cell is updated in place before moving to the next cell. All the rest are buffered – all cells have new computed values, then the entire grid updated at once.

Pass 1 creates floors near other floors, and creates rocks near other rocks. I’d call this a “clustering” pass, that pushes the random tiles towards wide open areas of tiles. The unbuffering is important here so that you don’t flop-flop, turning something rock→floor near something turning floor→rock.

Passes 3 and 4 are erosion routines, it’s a very common use of cellular automata. If you have 6-8 rocks nearby, then you also become a rock. That tends to smooth out corners, and delete tiny islands.

Pass 2 though… It turns cells to rock if they have exactly 5 floor neighbors (i.e. 3 rock neighbors). I’m still struggling to understand what purpose this is intended to achieve. Any ideas?
