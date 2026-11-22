---
title: 'Exponential Behavior in Magic: The Gathering'
link: https://levineuwirth.org/essays/exponential-magic/
source: hnrss-org
published: 2026-11-18T14:07:09Z
updated: 2026-11-18T14:07:09Z
first_seen: 2026-11-22T06:30:16.969889396Z
authors:
- neuwirth
content: extracted
html: 2026-11-18-exponential-behavior-in-magic-the-gathering.html
remote_preview:
  url: https://levineuwirth.org/og-image.png
---

I have been fascinated with [Magic: The Gathering](https://en.wikipedia.org/wiki/Magic%3A_The_Gathering) for years, not only as a remarkably fun and strategically deep game, but also as a mathematical and computational space. The game is remarkably complex, even [Turing complete](https://arxiv.org/abs/1904.09828), with intricate keywords and a daunting amount of mechanics [a](https://levineuwirth.org/essays/exponential-magic/#fn-a), presenting an immense computational challenge for making the game run on a computer, let alone machine-playable. There are multiple existing software implementations of Magic; [Arena](https://en.wikipedia.org/wiki/Magic:_The_Gathering_Arena) is the most prevalent, developed by [Wizards](https://en.wikipedia.org/wiki/Wizards_of_the_Coast) themselves, but there are numerous issues: the card catalog is notoriously incomplete, missing cards from the majority of the game’s history; the matchmaking methodology, particularly for ranked play, is absolutely horrific [b](https://levineuwirth.org/essays/exponential-magic/#fn-b), and the performance of the game is quite poor, particularly when there is a lot on the [stack](https://mtg.wiki/page/Stack). [MTGO](https://en.wikipedia.org/wiki/Magic:_The_Gathering_Online) resolves the card catalog issue, but introduces issues of its own, including lack of adoption, a rather outdated and difficult to use interface, and so on. Meanwhile, [Forge](https://github.com/Card-Forge/forge), an independent implementation, is promising at first glance — but the AI play is quite poor [c](https://levineuwirth.org/essays/exponential-magic/#fn-c), multiplayer is quite buggy and not truly supported, the interface is unintuitive, and the performance is still not ideal, even if improved over Arena.

## Building an MTG Engine

I began theorizing methods to improve AI play of Magic as early as 2024 but did not begin any implementation work on this until mid 2026; I first evaluated Forge and then decided that it was not feasible to get Forge up to a level of performance such that it could serve as an experimentation environment for RL on Magic itself, and thus decided to implement my own rules engine, Arcana. I figured that I could also design with multiplayer in mind from day one as a nice bonus and motivator.

One of the sources of my fascination with Magic, and, indeed, one of the reasons that a truly performant and *complete* software implementation of the game has yet to be achieved, is the nature of abilities in the game. Abilities in Magic can compound exponentially, and the amount of different possible interactions that can occur when any ability is triggered leads to a situation where although there may not be an unfeasible amount of [state](https://en.wikipedia.org/wiki/State_\(computer_science\)) in principle, the amount of checks that are required to ensure that no further side effects must be put on the stack *also* grows exponentially, and as we all know, exponential behavior is an enemy of us mere mortal computer scientists. My fascination with the exponential behavior of Magic came back to bite me in an unexpected way.

## My Irritable Scute Swarm Deck

One of my favorite decks I ever devised was very simple in mechanicism and brutal in computational complexity. It revolves around a notorious card: [Scute Swarm](https://scryfall.com/card/znr/203/scute-swarm). To explain how this deck worked, we need a bit of background on the internals of Magic.

An introduction to Magic’s mechanics

The core quantity of **Magic: The Gathering** is [life](https://mtg.wiki/page/Life); it can be thought of as the player’s score, or an encapsulation of how well the player is doing. Each player begins with 20 life, and the primary way to win the game is to reduce the opponent’s life score to 0 (or below) before your own life score is reduced as such. There are different types of cards that can be used, and on any given turn, a player may use the resources granted to them by [lands](https://mtg.wiki/page/Land), the type of card that forms a basic currency in the game, to deploy whatever they wish within the game’s limitations and rules.

Perhaps the most important type of card is a [creature](https://mtg.wiki/page/Creature). These are equivalent to the prototypical card in most trading card games, like Pokemon; they can attack the opponent and defend from the opponent’s attacks. Scute Swarm is one example of a creature.

There are also [tokens](https://mtg.wiki/page/Token), which are special cards that are the result of an ability of a different card rather than a card that you play. One might use a card to earn a token, but one would not have a token in the deck that they build, and they would not ever have a token in their hand so to speak. The actual mechanics here are a bit complicated, but the big picture is that tokens come into existence as a result of some ability and not from being in the player’s hand or deck outright.

Scute Swarm is perhaps the most notorious example of a Magic card that exhibits exponential behavior. It has the ability, when a new land is deployed, to double the amount of copies of itself that exist. In practice, this is usually countered early, as Magic allows for many different ways [d](https://levineuwirth.org/essays/exponential-magic/#fn-d) to avoid the exponential being realized (which, if substantially realized, *will* lead to victory for the person who played Scute Swarm practically invariably). If you combined this, I realized, with cards that produce life-gain whenever a new creature enters the battlefield, then one would have exponentially growing life. To make this even worse, one could play an artifact that doubles the amount of tokens that are created whenever any token is created, and throw in something to persistently double the amount of life gained. Seems like quite the loophole, eh?

For the arithmetic, the sixth land matters. Below six lands a Scute makes an ordinary 1/1 Insect. With six or more when the ability resolves, it makes a copy with the same landfall ability. Give each land’s triggers time to resolve before the next land comes down and every new copy gets to help with the next batch.[e](https://levineuwirth.org/essays/exponential-magic/#fn-e)

Suppose Soul Warden and Essence Warden are both in play. Each gives you one life for each new Scute. With a life-gain doubler that’s four life per arrival, and with a token doubler each existing Scute makes two new ones. Here’s what eight land drops would buy you under those assumptions.

```python
# One Scute, two Wardens, one token doubler, one life-gain doubler.
# Six or more lands are present.
scutes = 1
life_gained = 0
for _ in range(8):
    new_scutes = 2 * scutes
    scutes += new_scutes
    life_gained += 4 * new_scutes
# 6,561 Scutes; 26,240 life gained.
```

This is totally egregious! Considering that a player begins with 20 life in Magic, and life gains are usually modest if present at all, a change of 3 orders of magnitude warrants that notoriety that Scute Swarm has. For comparison, I think (playing decks outside of this one) that, on paper cards, the highest life total I have achieved in a game is around 150.

![Figure 1. Eight land drops with two Wardens and both doublers, compared with eight resolutions of Liliana’s emblem. Each gridline marks a tenfold increase.](https://levineuwirth.org/essays/exponential-magic/figures/scute-and-life.svg)\
Figure 1. Eight land drops with two Wardens and both doublers, compared with eight resolutions of Liliana’s emblem. Each gridline marks a tenfold increase.

The Scute curve is something we can work out on paper, but Arcana doesn’t yet support scute swarm (deliberately, I might add). It was the purple curve that got me into trouble.

Now, who is this **Liliana** in the above plot?

## Liliana takes over

Planeswalkers are, in a way, the *heroes* of Magic. They usually have multiple abilities, the most expensive of which (usually positioned at the bottom as the last one) we tend to call the “ultimate” — simply put, if you are playing against an opponent with a Planeswalker, you’d best ensure they don’t trigger that ability. In some cases, the ultimate can create a special type of card [f](https://levineuwirth.org/essays/exponential-magic/#fn-f) called an [emblem](https://mtg.wiki/page/Emblem). Emblems are particularly concerning for you if your opponent has the opportunity to procure one, and particularly advantageous if you have the ability to use one, chiefly because they persist even if the Planeswalker is removed from play. An ability that an emblem provides will be in effect for the remainder of the game, typically leading to devastating and, importantly, **compounding** consequences for the opponent.

[Liliana, the Last Hope](https://scryfall.com/card/2x2/81/liliana-the-last-hope) is one such planeswalker. Her ultimate happens to create quite the dire situation not only for the opponent, but also for those who are interested in implementing Magic in software. When her ultimate ability resolves, the resulting emblem ensures that more and more zombie tokens are deployed onto the battlefield in a matter not too unlike that of Scute Swarm. Unfortunately for us as implementers, and for those who play against Liliana as losers, the amount of zombies that are deployed grows exponentially. What *is* unlike Scute Swarm is that clearing away the Zombies leaves the emblem intact and able to rebuild the army, whereas removing every Scute Swarm removes the source of its growth.

Let’s formalize what happens. At the beginning of each of your end steps (the last part of a player’s turn), the emblem triggers. When that ability resolves, it counts the Zombies you control and creates that many more, plus two. With six Zombies already on the battlefield, you get eight new ones, leaving fourteen.

Let $Z_n$ be the number of Zombies after $n$ resolutions of the emblem’s ability. Start with no Zombies ($Z_0=0$) and one emblem, and suppose nothing else adds or removes Zombies or changes how many tokens are created. Each resolution then gives us:

$$
Z_{n+1}
=\underbrace{Z_n}_{\text{already there}}
+\underbrace{(Z_n+2)}_{\text{new arrivals}}
=2Z_n+2.
$$

The first few populations are $0,2,6,14,30,62,\ldots$. After $n$ resolutions, that gives $Z_n=2^{n+1}-2$. Crucially, the army will always restart: even if every Zombie is destroyed [g](https://levineuwirth.org/essays/exponential-magic/#fn-g), the next resolution produces two more.

This forms Liliana’s purple curve in the [comparison above](https://levineuwirth.org/essays/exponential-magic/#fig-scute-growth): 510 Zombies after eight resolutions. That plot counts *growth events* along the horizontal axis: one emblem resolution for Liliana, or one land entry for Scute Swarm.

Arcana implements that counting rule directly:

```rust
let zombies = script::count_matching(state, &filter, trig.controller);
let x = 2 + zombies;

// Token definition omitted: a 2/2 black Zombie.
let mut effects = Vec::new();
for _ in 0..x {
    effects.push(Effect::CreateToken {
        controller: trig.controller,
        token: token.clone(),
    });
}
```

Here, `x` counts the *new arrivals*, and the loop produces a separate `CreateToken` effect for every one of them. The engine then has to process all those arrivals, including the rules checks and any abilities they set off. As the army grows, so does the work required to resolve the emblem’s ability.

## What one engine step costs

Let’s step back for a second. I was trying to decide which gaps in Arcana’s rules to fix next. I had 64 Pioneer decks [h](https://levineuwirth.org/essays/exponential-magic/#fn-h) and a list of 37 cards with known issues. There were 2,016 distinct pairs of decks. I played each pair twice, once with each deck going first, giving me 4,032 games in which to count how often those situations came up.

In an effort somewhat analogous to [fuzzing](https://en.wikipedia.org/wiki/Fuzzing), I scheduled that entire 4,032-game lineup another 100 times. Each run used a different random seed [i](https://levineuwirth.org/essays/exponential-magic/#fn-i), and both players chose their actions at random. That meant $100 \times 4{,}032 = 403{,}200$ additional games to see whether I could trigger a panic, crash, or stall in the engine.

All ~400,000 games are independent, with no intertwined dependencies between them. Thus, the work parallelizes nicely to HPC, and I typically run these games on a cluster rather than my own hardware. (This is especially important when I run larger trials and RL experiments using the engine, but still nice-to-have for purposes like this!) Unfortunately, when I did this on this occasion, nine jobs hit their two-hour limit. The random seed and the two decklists let me replay a single stalled game on my laptop and time its individual steps.

In that game, one player activated Liliana’s ultimate on turn 34. Once the army had grown into the thousands, one resolution of the emblem took 35 seconds on my laptop. That was a *single engine step*. Both players had let the ability resolve. Before either could make another decision, Arcana had to put every new Zombie onto the battlefield and check what its arrival triggered.

Let’s take the first of those Zombies. A creature you control might have *evolve*, an ability that can give it a +1/+1 counter when a creature with greater power or greater toughness enters under your control. The engine has to find every creature that qualifies as follows [j](https://levineuwirth.org/essays/exponential-magic/#fn-j).

```rust
let evolvers: Vec<ObjectId> = state.objects
    .objects_in_zone(Zone::Battlefield)
    .filter(|o| o.controller == ctrl && o.id != *object_id)
    .map(|o| o.id)
    .filter(|id| state.effective_keywords(*id)
        .iter().any(|k| matches!(k, KA::Evolve)))
    .filter(|id| {
        ep > state.computed_power(*id).unwrap_or(i32::MAX)
        || et > state.computed_toughness(*id).unwrap_or(i32::MAX)
    })
    .collect();
```

This code is a walk of the battlefield. For each other [permanent](https://mtg.wiki/page/Permanent) you control when one of your Zombies enters, `effective_keywords` computes its current characteristics to find out whether it has evolve. In this game, *nothing had evolve*, so the list came back empty. Then the next Zombie entered, and, most lamentably, the engine asked the same question all over again. Double the army and about twice as many Zombies enter, each scanning about twice as many objects. You can see where this is going: about four times as many checks are required to produce the same empty list.

On turns 52, 54, and 56, I counted the Zombies at the start of the turn and timed the emblem when it resolved at the end. In one laptop replay those steps took 1.72, 7.78, and 35.15 seconds.

![Figure 2. As the Zombie count roughly doubled, each emblem resolution took about four and a half times as long.](https://levineuwirth.org/essays/exponential-magic/figures/liliana-replay.svg)\
Figure 2. As the Zombie count roughly doubled, each emblem resolution took about four and a half times as long.

By turn 58, the opponent was at 31 life, facing 7,256 Zombies. At two damage apiece, sixteen unblocked Zombies would have been enough. The largest attack Arcana offered that turn used one creature.

## Why didn’t the army attack?

Here’s the view from the replay visualizer of the defending seat, just before the other player chose attackers. Don’t miss the little badge in the corner of each Zombie pile! (The visualizer groups creatures with the same name, tapped state, power, and toughness onto one tile.)

![Turn 44. The attacker has 56 Zombies across three piles. One of the 229 declarations on offer would send 56 creatures into combat.](https://levineuwirth.org/essays/exponential-magic/figures/battlefield-turn-44.png "The opponent’s Zombie piles hold 49, six, and one. The defending player is at 24 life.")\
Figure 3. Turn 44. The attacker has 56 Zombies across three piles. One of the 229 declarations on offer would send 56 creatures into combat.

![Turn 58. The largest pile alone holds 7,234 Zombies, and the defender is at 31 life. The biggest attack on the menu sends one creature.](https://levineuwirth.org/essays/exponential-magic/figures/battlefield-turn-58.png "The opponent’s three piles now hold 7,234, 21, and one, for a total of 7,256 Zombies.")\
Figure 4. Turn 58. The largest pile alone holds 7,234 Zombies, and the defender is at 31 life. The biggest attack on the menu sends one creature.

At a table, I can say “all the Zombies attack you” and turn them sideways. In fact, with paper cards, there’s essentially no chance that I, nor any reasonable human, own 7,000 printed copies of a 2/2 zombie token — so we’re probably using some other methodology, whether pencil and paper or an app on someone’s phone that acts as a counter. In Arcana, unfortunately for us, we have to be quite precise about the state and transformations applied to it: the random player picks from a list, and each entry specifies the *entire* attack. On turn 44, each eligible Zombie could do nothing, could attack the player, or could attack one of the three opposing planeswalkers. Thus, we have a total of five choices per Zombie. Even with only five Zombies, there would be $5^5 = 3{,}125$ ways to declare attackers, and as you might guess, when there are thousands of zombies… yeah, not good!

Arcana had a budget of 1,024 declarations. When the combinations exceeded that budget, the idea was to keep a few useful possibilities, encapsulating the decisions a reasonable player might make rather than providing all options. My justification for this was that if playing on paper, because as I’ve stated no one has 7,000 printed zombies, it would be incredibly pedantic to say “3,759 of my zombies are attacking you, and 3,497 are attacking planeswalker X” — no one [k](https://levineuwirth.org/essays/exponential-magic/#fn-k)would ever do this. Arcana reduces the options such that one could pass, send a single creature at any defender, or send everyone who could attack a particular defender at once. The pass was already in the list when this code ran. Look at the order of the two loops.

```rust
// Abridged control flow; declaration construction is omitted.
for (atk, defs) in &slots {
    for &def in defs {
        if out.len() >= MAX_COMBAT_ENUM { return out; }
        // Append a single-creature attack.
    }
}

// Only after the singles: append all-in attacks for each defender.
for def in distinct {
    if out.len() >= MAX_COMBAT_ENUM { return out; }
    // Append all eligible attackers against this defender.
}
```

On turn 44, the arithmetic worked out pretty nicely. Of the 58 creatures on the board for the attacker, 56 could attack. Four defenders gave those creatures $56 \times 4 = 224$ ways to attack alone. Add the pass and the four ways to send every attacker at a singular target, and you get the 229 entries in the replay. Only one of those sent the whole army at the player, so odds are that the random player would miss it.

Now, give that first loop 256 eligible attackers and the same four defenders. The single-creature attacks alone would take 1,024 slots, and passing has already taken one. That little `return out` exits the *function*, so crucially, the second loop never gets a turn.

| Turn | Creatures controlled | Declarations offered | Using multiple creatures |
| ---- | -------------------- | -------------------- | ------------------------ |
| 44   | 58                   | 229                  | 4                        |
| 48   | 229                  | 913                  | 4                        |
| 50   | 457                  | 1,024                | 0                        |
| 58   | 7,258                | 1,024                | 0                        |

By turn 50, even a perfect player choosing from this list could send at most one creature. And what did our player actually do on turn 58? It sent one 2/2 Zombie after Oko, Thief of Crowns. The defending player stayed at 31 life. Then came the end step, and Liliana’s emblem triggered again.

## Putting a bound on it

Arcana already knew how to call a draw when it saw the same settled position often enough; we had a system analogous to [threefold repetition](https://en.wikipedia.org/wiki/Threefold_repetition) in chess, though ours was more for the convenience of the hardware Arcana runs on rather than any player computational or human. The random-play test itself also had a limit of 20,000 actions per game. Each time Liliana’s army doubled, that check saw a position it had never previously seen. On turn 58 the replay got as far as action 2,325, passing priority to let the emblem resolve. Sixty seconds later, my poor laptop probe was still waiting for that one call to return, with plenty of actions left in the budget.

Recall that we had 64 decks; only four of them actually contained Liliana, the Last Hope. (Doesn’t her name take a special irony at this point? I find it wonderful.) Indeed, as one might expect, all nine jobs that timed out had one of those four decks in the game they left unfinished. For the next run, then, I made the test harness count every permanent on both sides of the battlefield. With the limit set to 500, the check in the random-play test looks like the following.

```rust
let size = battlefield_size(&state);
peak = peak.max(size);
if size > max_permanents && !matches!(yld, EngineYield::GameOver(_)) {
    let detail = format!("{size} permanents at turn {}", state.turn.turn_number);
    return out("board_cap", steps, detail, peak);
}
```

That explains the slightly odd-looking result from replaying *another* stalled game with the limit on. It stopped on turn 48 at **576** permanents, 0.16 seconds into the game! Why was this, you ask? An entire batch of tokens can arrive during one step. The harness sees the new board as soon as that step finishes, records the turn and size under its own outcome, `board_cap`, and moves on to the next game. When I count wins, losses, and draws, I can still see exactly which games the experiment cut short.

This got the experiment moving again, but there are two engine changes I’d still like to measure on this replay. One is to work out which permanents have evolve once and reuse that answer for as long as the relevant characteristics stay the same. The other is to reserve room for the all-in attacks before filling the menu with singles. I think both are necessary potential directions; the first might buy Liliana a cheaper end step, while the second would give the attacking player a chance to end the game before she gets one.

## The “Magic” in Magic: The Gathering

In a way, this was inevitable, wasn’t it? I had it coming. Saying I knew the trouble I was creating when I envisioned the Scute Swarm deck with its exponential synergies would be an understatement; in a way, I recall having this subtle feeling of foreboding when I had the idea and first put a concept together in Arena. I knew both in principle what would happen (“what number comes next in the sequence 2, 4, 8, \_\_?“) and what I wanted to see happen in effect (”just how much life can I gain?“). Years later, it seems the true meaning of this foreboding sensation has been revealed to me. Liliana has obliged me, at a somewhat greater expense.

Perhaps what I find most remarkable about Liliana, though, is how legible the instruction is on her emblem, regardless of where within that exponential growth you are. One of the chief criticisms of Magic as a game is that the instructions are often jargon heavy and difficult to interpret; indeed, one seldom sees a card (other than something like a basic land, of course) on Scryfall which *hasn’t* accumulated multiple official rulings on interpretation over the years. Yet I think that with Liliana (who herself has accumulated such interpretations, indeed, so perhaps I’m wrong) this is a nonissue. If you have 0 zombies you make 2, and if you have 7,256, you make 7,258 all the same. The amount of ink that the instructions need to describe the latter is exactly the same as how much describes the first, yet the latter can evidently be the cause of much more tumult. That’s some of the magic of *Magic* for me, in a way. I can hold a deck of 60 cards just like my Scute Swarm creation in my hand in an afternoon and years later, I’m still discovering *exactly* what it is that I’m holding.

## Code, data, and replay logs

- **The decks.** The field came from [camilonunez’s MTGTop8 dataset on Kaggle](https://www.kaggle.com/datasets/camilonunez/magic-the-gathering-top8-some-decks-and-events). The [64 tournament deck dates](https://levineuwirth.org/essays/exponential-magic/data/field-deck-dates.csv) show when these lists saw play.
- **The figures.** The [growth model](https://levineuwirth.org/essays/exponential-magic/code/growth-model.py) works through the Scute and Liliana arithmetic. The [plotting script](https://levineuwirth.org/essays/exponential-magic/code/plot-figures.py) reads the [figure data and assumptions](https://levineuwirth.org/essays/exponential-magic/data/figure-data.json).
- **The engine.** From [Arcana](https://github.com/levineuwirth/arcana) at the time of the incident, here are [Liliana’s emblem](https://levineuwirth.org/essays/exponential-magic/code/liliana-emblem.rs), the [evolve scan](https://levineuwirth.org/essays/exponential-magic/code/evolve-scan.rs), the [attack enumerator](https://levineuwirth.org/essays/exponential-magic/code/attack-list-cap.rs), and the [loop detector](https://levineuwirth.org/essays/exponential-magic/code/loop-detection.rs). The later [battlefield limit and its replay result](https://github.com/levineuwirth/arcana/commit/6b930542600f910443e11c9b91d81d63faa65fe5) are in the harness commit.
- **The replay.** The [growth log](https://levineuwirth.org/essays/exponential-magic/data/replay-growth.txt) follows seed 20261156 through turn 58. Figure 2 uses its timings, and the [second timing log](https://levineuwirth.org/essays/exponential-magic/data/replay-timing.txt) records another laptop replay of the same game. The [recorded attack choices](https://levineuwirth.org/essays/exponential-magic/data/attack-choices.csv) give the size of each menu. The [short Python example](https://levineuwirth.org/essays/exponential-magic/code/attack-menu-demo.py) reproduces the 1,024-entry cutoff.
- **The battlefield.** The [screenshot instructions](https://levineuwirth.org/essays/exponential-magic/code/battlefield-capture.txt) use a [replay patch](https://levineuwirth.org/essays/exponential-magic/code/replay-capture.patch) and a [capture script](https://levineuwirth.org/essays/exponential-magic/code/capture-battlefield.py). The [capture manifest](https://levineuwirth.org/essays/exponential-magic/data/battlefield/manifest.json) identifies the saved boards for [turn 44](https://levineuwirth.org/essays/exponential-magic/data/battlefield/turn-44.json.gz) and [turn 58](https://levineuwirth.org/essays/exponential-magic/data/battlefield/turn-58.json.gz). To find the attack on Oko, look up step 2,311 in the growth log. In the turn 58 board, attacker 738 is the Zombie and planeswalker 686 is Oko.

1.  These are split between “evergreens,” which are the ones more familiar players like myself will know, and the rest; Magic introduces at least one new keyword for every new set introduced, which can often be the source of great confusion. Luckily, the new ones are usually explained, and [Scryfall](https://scryfall.com) is only a quick search away.[↩︎](https://levineuwirth.org/essays/exponential-magic/#snref-a)

2.  It has never been proven (Arena is closed-source) but has been widely documented that, particularly at the highest levels of play, Arena almost uniformly ranks decks against other decks of similar archetypes. This infuriating situation defeats the variety in deckbuilding and strategization that makes the game fun. I quit playing Arena when I was ranked Mythic, and I lament the fact that I was not 18 yet and thus couldn’t participate in the Pro Tour qualifier that I would’ve otherwise been eligible for, as for this reason, I will never return to serious Arena play.[↩︎](https://levineuwirth.org/essays/exponential-magic/#snref-b)

3.  Even worse, in fact, than that of Arena, which seems to be deterministically coded based on a few “starter decks” that do not rotate; at least Forge’s can be played with any arbitrary deck\![↩︎](https://levineuwirth.org/essays/exponential-magic/#snref-c)

4.  Instant removal is the obvious one, but if such is not available, countering the spell while it is on the stack is another option, as is exiling the card before the sixth land comes down.[↩︎](https://levineuwirth.org/essays/exponential-magic/#snref-d)

5.  See the Scute Swarm entry in the [Zendikar Rising release notes](https://magic.wizards.com/en/news/feature/zendikar-rising-release-notes-2020-09-10).[↩︎](https://levineuwirth.org/essays/exponential-magic/#snref-e)

6.  It’s actually a [*marker*](https://mtg.wiki/page/Marker), not a [*card*](https://mtg.wiki/page/Card), but for our conversational purposes, it’s effectively a “card” as we would define it in colloquial English speech. Sorry, not sorry.[↩︎](https://levineuwirth.org/essays/exponential-magic/#snref-f)

7.  So, no, sorry, as much as I admire it as a card, [Extinction](https://scryfall.com/card/tmp/135/extinction) will *not* save you. Exponentials always win.[↩︎](https://levineuwirth.org/essays/exponential-magic/#snref-g)

8.  These came from camilonunez’s collection of MTGTop8 tournament decks on Kaggle. Of the 800 distinct Pioneer decks I checked, Arcana had every card in the 60-card maindeck for 64 of them.[↩︎](https://levineuwirth.org/essays/exponential-magic/#snref-h)

9.  In this context, for a given matchup, changing the [seed](https://en.wikipedia.org/wiki/Random_seed) can change how the decks are shuffled and which moves the players pick. Using the same seed with the same decks, seating order, and version of Arcana lets me replay those choices exactly.[↩︎](https://levineuwirth.org/essays/exponential-magic/#snref-i)

10. Two debugger samples caught Arcana computing characteristics inside a battlefield scan consistent with this code. Other trigger checks also scan the board, and those samples can’t tell us how much of the elapsed time belonged to evolve alone, so treat evolve as merely an example.[↩︎](https://levineuwirth.org/essays/exponential-magic/#snref-j)

11. At least, not anyone who actually has opponents *willing* to play them in a game of Magic: The Gathering!\![↩︎](https://levineuwirth.org/essays/exponential-magic/#snref-k)
