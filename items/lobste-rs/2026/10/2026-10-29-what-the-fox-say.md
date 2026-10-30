---
title: What the fox say?
link: https://entropicthoughts.com/what-the-fox-say
source: lobste-rs
published: 2026-10-29T06:42:40Z
updated: 2026-10-29T06:42:40Z
first_seen: 2026-10-30T13:27:01.845275939Z
authors:
- entropicthoughts.com by kqr
labels:
- games
summary: Comments
content: extracted
html: 2026-10-29-what-the-fox-say.html
remote_preview:
  url: https://entropicthoughts.com/image/what-the-fox-say_01.png
  alt: what-the-fox-say_01.png
---

There is an old Norse board game, which in Sweden is played under the name *Starve the Fox*, or just *the Fox Game*. According to Wikipedia, *Fox and Geese* is a simplified version of it played in English speaking countries.

In *Starve the Fox*, one player plays as the foxes, and one player plays as the sheep. The rules are asymmetric, and obviously the game is almost always won by the fox player …

… or is it? Let’s find out.

## [Game rules](https://entropicthoughts.com/what-the-fox-say#game-rules)

This is what the board looks like in its starting position.

![what-the-fox-say_01.png](https://entropicthoughts.com/image/what-the-fox-say_01.png)

The orange circles with the letter F in them are the two pieces the fox player starts with. The white circles with the letter S in them are the 20 pieces the sheep player starts with. Green circles represent empty locations pieces can move into. The fox and the sheep players take turns moving one piece of their own. Pieces can only move one step at a time, along the lines on the board, and cannot pass through each other or occupy the same location.

After that, though, rules are asymmetric. First, the rules for the sheep player:

Goal

Fill the pen, which is the top-centre 3×3 square the fox starts in. The sheep player wins when nine of its pieces to occupy the box bounded by corners at `c5` and `e7`.

Movement

Not backwards. In other words, moves that reduce the rank of the sheep are not legal.

Attack

If the sheep player manages to trap a fox such that it cannot move at the end of the turn, that fox is removed from the board. This gives the sheep an alternate victory condition: if they manage to remove both foxes, they can no longer prevent the sheep from filling the pen, so the sheep win right away.

The fox player moves first at the start of the game.1 1 Though my analysis indicates that the first-move advantage is very small in this game, so it probably doesn’t matter too much who wins. Its rules are:

Goal

Prevent the sheep from filling the pen.

Movement

Any direction.

Attack

The fox can eat sheep by jumping over them. If there is an open location two steps away in a straight line from the fox, and a sheep on the location in between (i.e. this type of configuration: `F—S—O`), then the fox can move to the open location two steps away, and the sheep in between is removed. When fewer than nine sheep remain, they can no longer fill the pen and the fox wins.

It doesn’t take much to realise that this is a *loopy* game, meaning it can get into positions where the best moves for both players are to shuffle their pieces back and forth, repeating previous positions over and over and not making any progress. I have only heard rules explained in the presence of children, and they don’t specify what happens in such situations. In the analysis below, I’ve considered the game a draw after a stretch of 40 moves with no progress. Progress is easily measured in terms of sheep rank, since sheep cannot go backward.

## [The foxes are assumed to be overpowered](https://entropicthoughts.com/what-the-fox-say#the-foxes-are-assumed-to-be-overpowered)

I don’t remember playing this game when I was young, but it was a favourite of my wife’s late grandfather, and that half of her family knows it well. It is common wisdom in her family that the fox player needs to blunder badly for the sheep to win. Her entire family believes the game is heavily tilted in favour of the fox. I have played the game a few times, and indeed it has been easier for me to win when playing as fox.

I have an unhealthy relationship with strategy games like these. I simply *must* put them through computer analysis to figure them out. I don’t know why. It’s just something that happens. I used to write tree search algorithms manually, but I have realised that [OpenSpiel provides a generic tree search supporting most of the things](https://openspiel.readthedocs.io/en/latest/intro.html) I’ve ever found myself needing to support, so I can write the game rules in Python and hook into OpenSpiel to analyse it.

Well, consider the rules above. How often do you think the fox wins, between two equal players? Write down your guess, and you’ll learn how good your sense of this is.

## [A somewhat strong computer opponent](https://entropicthoughts.com/what-the-fox-say#a-somewhat-strong-computer-opponent)

The first stop for a problem like this is plain Monte Carlo tree search (mcts) with random rollouts to the end for leaf scoring. In un-optimised Python, this managed to perform about 1,000 search iterations per second when playing *Starve the Fox*. I gave this algorithm a thinking budget of just under a second, and played against it, both as the fox player and the sheep player. In those games, it seemed clear that the fox would win most of the time.2 2 Unless the sheep forced a draw, which they can do as long as there’s enough of them, thanks to the loopy nature of the game. The algorithm playing itself got a similar result: around 85 % of games were won by the foxen.

With so much corroborating evidence, I was close to calling the experiment finished. I didn’t expect deeper search to improve things by much, because the branching factor is relatively low, in part thanks to the constraints on sheep movement.

But then just to be sure, I tried one last game. I played as fox, and the computer played sheep with a search budget of 8,000 iterations. Although it still didn’t play particularly well, it managed to exploit two of my blunders, and actually get into a winning position. With the sheep! That caught my interest, and it meant the experiment wasn’t over at all, but might actually need deeper search to more accurately reflect the strategy possible.

## [What this says about the game](https://entropicthoughts.com/what-the-fox-say#what-this-says-about-the-game)

I didn’t realise it right away, but what happened in that high search budget game was that we learned something about the nature of *Starve the Fox*.

The sheep side requires deeper thinking than the fox side to play well. Since the sheep, in isolated conflicts with foxes, are so underpowered, *they must move the mass of the entire flock in coordination to beat the foxes*, and with one movement per turn, that requires strategising over many turns.3 3 In fact, the sheep player isn’t really moving sheep as much as they’re moving the holes in the flock to make sure the holes stay away from the sheep closest to the foxen. This is a *fantastic* property of the game, because it (a) is thematically smashing, and (b) means a child can play an adult and both still have fun; the child gets to play the fox side and the adult just has to think harder.

## [Speeding up the computer opponent](https://entropicthoughts.com/what-the-fox-say#speeding-up-the-computer-opponent)

However, I didn’t want to wait for 16,000 iterations or whatever’s needed for the plain mcts to play a very strong game. One of the more expensive parts of that search algorithm are the random rollouts to score leaves in the search tree. A trick to make leaf scoring faster is to cut off the rollouts well before the game ends4 4 The depth at which to truncate rollouts depends on both the complexity of the game and the quality of the rollout policy. For completely random rollouts, it may even be desirable to cut them off at depth zero – i.e. not perform any random moves at all, because they make the scored state less representative of the end state than the earlier leaf node was. In the case of *Starve the Fox*, stopping the rollout after 8 plies seemed to yield the best results with the first draft of the heuristic function, but it’s possible with a stronger heuristic function shorter rollouts would be even better, because they buy time for more tree search at the cost of not evaluating as far into the future., and then use a heuristic function to score the positions in games that haven’t yet terminated when the rollout is truncated. The heuristic value is scaled to be smaller in magnitude than the score given to a terminated game, so that the tree search will prioritise the win/loss signal over the heuristic when it can see both.

But this means a heuristic function is needed. A primitive one might be based on material, like “number of sheep alive” or based on progress, like “total rank gained by sheep”. A more sophisticated one might include both of those variables, and more. If we didn’t have computers, we’d probably be best off by picking out [the top three variables and weighing them equally](https://entropicthoughts.com/arithmetic-models-better-than-you-think).

But we have computers, so we can do better. What we do is play the weak tree search5 5 Search budget of maybe 0.5 seconds per ply, scoring leaves by random rollouts to the end. against itself a bunch of times. From each of those games we sample one random position6 6 Only one position to avoid having to deal with the strong correlation between consecutive positions in the same game., record how the game ended, and then train a regression model to predict the game outcome based on the game state in those sampled positions. The weights we get out of the regression minimise square loss, which is probably what we want for a heuristic function.

Once we have those weights, we can create a new generation of the tree search using the same search budget, but with truncated rollouts and applying the learned heuristic function. This gets to play against the previous generation to confirm it is indeed better, and if it is, it plays itself a bunch of times again, and we sample new positions, and fit the regression again. Since it’s playing better than the previous generation, it will find ways to win from positions where the previous generation did not. Thus, the heuristic function’s prediction of outcome – assuming skilled play on both sides – will become more accurate.

We repeat that a few times, and what we get is a decently strong opponent, even at short thinking time budgets. Well, at least it reliably sweeps the floor with me, regardless of which side it plays.

## [The learned evaluation function](https://entropicthoughts.com/what-the-fox-say#the-learned-evaluation-function)

Maybe the trained weights tells us something about what good positions look like, which might help us get better at the game, too. Since we haven’t seen the link function7 7 It’s a hyperbolic tangent to map the raw score (real number, infinite support) onto the range \[−1, +1\] indicating “sure loss” and “sure win” respectively, with a draw having value zero. But we’d also need the bias term (roughly +1) to convert it into the final value, which isn’t really a probability but almost., we can’t do anything with the absolute values of these weights, but their relative sizes are more interesting anyway.

- *Number of sheep eaten so far*: favours the fox with +0.036 per sheep
- *Number of sheep that can be eaten, fox to move*: favours the fox with +0.096 per sheep
- *Number of sheep that can be eaten, sheep to move*: favours the fox with +0.035 per sheep
- *Number of candidate moves of the fox player*: favours the fox with +0.021 per move
- *Number of ranks gained by sheep in total*: favours the sheep with +0.0034 per rank
- *Number of sheep in the pen*: favours the sheep with +0.017 per sheep

Although it’s tempting, it would be improper to interpret these right away, because the heuristic function trained here includes one more thing. It’s also trained weights for specific locations on the board, which answers the question of how valuable it is for a player to occupy specific positions on the board, in general.

The values of the board positions absorb some of the coefficients from the variables above. For example, the list above indicates the value of a sheep in the pen as 0.017, but the least valuable pen position has value 0.11, compared to the average outside-pen position with value 0.09. Thus, the value of a sheep in the pen (compared to having it outside the pen) is never less than 0.11−0.09+0.017=0.04. On average it is 0.08, which is much higher than the list above would make us believe.

Here are the rest of the board values for the sheep player.

```
7               0.21  0.13  0.21              | 0.18
6               0.11  0.16  0.11              | 0.13
5   0.08  0.16  0.14  0.13  0.14  0.16  0.08  | 0.13
4   0.05  0.12  0.10  0.13  0.10  0.12  0.05  | 0.10
3   0.12  0.11  0.10  0.13  0.10  0.11  0.12  | 0.11
2               0.02  0.05  0.02              | 0.03
1               0.06  0.12  0.06              | 0.08
     a     b     c     d     e     f     g    | mean
```

This tells us that a sheep in the `c7` position is worth 0.23, with most of the value coming from the position itself, and only a small amount coming from the general value of the *number of sheep in the pen* variable. Here are some other observations:

- All positions in rank 2 are bad, because sheep there are exposed to being eaten, and not contributing to the goal either.
- Positions along the edges but not in the corner, like `b3`, `c3`, `c6`, and `d7` are generally worse than their corner neighbours, because of the risk of being eaten.
- A position like `a5` is bad because a sheep stuck in there only has one move, and it is blocked by a fox at `c5`. I suppose `a4` is bad for similar reasons: it doesn’t leave the sheep much choice about where to go. (That said, `a5` has one redeeming quality: it protects a sheep at the more valuable `b5` position from being eaten.)
- Other than ranks 2 and 4, which both are outside the pen and have a heightened risk of being eaten, the value of the ranks increases as they get closer to the pen.
- I am surprised that the `d6` cell is so valuable for sheep! I don’t know why. Possibly related to mobility.

These position values also help us interpret the macro-scale variables from before:

- The average value of a sheep gaining a rank is 0.02 favouring the sheep.
- The average value of having a sheep in the pen (compared to outside) is 0.08 favouring the sheep.
- The average value of starting a turn being able to eat a sheep is 0.20 favouring the fox.
- The average value of an eaten sheep is 0.15 favouring the fox.

The value of an eaten sheep is a nice, intuitive scale for these values. We can express all the others in terms of *sheep value*, similar to how in chess they measure things in pawn value. Under that conversion, the sheep player needs to gain a total of nine ranks to offset the loss of a sheep.

It may also seem like it is a good trade (net sheep value +0.07) to sacrifice a sheep to get two others into the pen. However, intentionally sacrificing a sheep means putting it into a position where the fox is able to eat it, which is worse than just having lost a sheep. The trade actually has net sheep value −0.27. I don’t know why an imminent feeding is worse for the sheep than an eaten sheep, but I suspect it’s because it allows the fox to jump two positions, and thus move quicker across the board.8 8 And logic would have us conclude that this ability to move two places (in isolation, separate from any eating that would happen as a side effect) is, on average, worth a whopping 0.2 sheep value!

Enough about sheep. The individual-position values for the fox player are

```
7               0.30  0.30  0.30              | 0.30
6               0.29  0.30  0.29              | 0.29
5   0.33  0.41  0.37  0.36  0.37  0.41  0.33  | 0.37
4   0.34  0.37  0.39  0.37  0.39  0.37  0.34  | 0.37
3   0.34  0.37  0.34  0.35  0.34  0.37  0.34  | 0.35
2               0.32  0.31  0.32              | 0.32
1               0.33  0.29  0.33              | 0.32
     a     b     c     d     e     f     g    | mean
```

These position values are flatter across the board, with an average corresponding to 2.3 sheep value – each living fox is worth just over two living sheep. There is some advantage for the fox to be out in the middle of the field, in ranks 4 and 5. The fox does not want to find itself inside the pen, because then sheep in the field get the opportunity to rearrange themselves with impunity. The foxes should, all else equal, stick around in the middle 5×2 rectangle bounded by `b4` and `f5`.

It seems that `b5` (and its mirror image `f5`) are some of the most valuable points for both fox and sheep and I’m not sure why.

## [Getting good with puzzles](https://entropicthoughts.com/what-the-fox-say#getting-good-with-puzzles)

Let’s say we still don’t feel like we’ve learned enough, and want to improve even more at the game. We have game records from when the algorithm played itself, where all kinds of interesting situations have shown up. What we can do is create *Starve the Fox* puzzles. These would be used like tsumego or chess puzzles: they plop the player down in an interesting situation with a given objective, and then the player must figure out the one next move that achieves the objective. Here are some examples of situations that make for good puzzles:

- Sheep to play: trap a fox within N turns.
- Fox to play: survive a forced trap for N turns.
- Fox to play: eat a sheep without getting trapped within N turns.
- Sheep to play: prevent a sheep loss for N turns.
- Sheep to play: fill the pen within N turns.
- Fox to play: prevent the pen from being filled for N turns.

The parameter N controls both the difficulty of the puzzle9 9 And I have to admit I have failed some where N=1, i.e. I just needed to spot the direct capture or whatever the case may be … and the compute cost to find the right situations in the self-play records. A cheap filter can quickly eliminate most positions as uninteresting, but then many of the puzzle types require a complete search to depth N before we know that the situation would make for a proper puzzle. For example, “eat a sheep without getting trapped” means we first have to find a situation in which a sheep can be eaten, and then also make sure there is no follow-up trap within the same N turns. Such a follow-up would be a refutation to the solution: the sheep wouldn’t be lost, it would merely be sacrificed to trap the fox.

I only tried generating puzzles with N lower than four, but that alone gave some 2000 puzzles. If I worked through all of those, I’m sure I would end up playing a much meaner sheep game. Alas, of all the things I could get skilled at, this is very low on my not-so-well prioritised list.

But in case someone else is interested in spending time to get good at *Starving the Fox*, [here’s a decent set of puzzles](https://xkqr.org/fox-game-puzzles/) to get you started.

## [Language models and me](https://entropicthoughts.com/what-the-fox-say#language-models-and-me)

As a meta-reflection, this is the sort of project I would not have attempted before the developments we’ve seen to language model harnesses in the past year or so. Even if the language models themselves have not gotten better, the harnesses have gotten better at pushing them into actively producing feedback, which in turn lets them generate code satisfying specifications more reliably.

But satisfying specifications doesn’t mean good code. It’s generated over 3,000 lines of Python implementation, 1,000 lines of tests, and 500 lines of html, all in the dense code style modern language models tend to generate. It’d expand to a total of over 6,000 lines if formatted like a human. The code is unnecessarily complex and rigid. If I cared, I could refactor it to much higher quality, shrinking it in size maybe to around 4500 lines, which seems appropriate for this size of functionality when the code is well-written. That’d be a [complexity reduction of 25 %](https://entropicthoughts.com/llms-and-almost-good-code).

But for this? I don’t care about that. I don’t care about the code. I would not be sad if this code disappeared tomorrow. I would also never end up manually writing 4,500 lines of code to learn more about an old Nordic board game. I like programming, but I have much more interesting programming projects on my plate.

## [The win chance for the fox](https://entropicthoughts.com/what-the-fox-say#the-win-chance-for-the-fox)

Oh, I almost forgot. Under these rules, with a strong-ish search, the sheep win about 50 % of the time. When they cannot win, they can usually force a draw, which appens 35 % of the time. The fox wins 15 % of the time.

This is not a solution – we don’t know if perfect play means the sheep always win, or the fox always wins, but the fox is certainly not so overpowered as my wife’s family assumes.
