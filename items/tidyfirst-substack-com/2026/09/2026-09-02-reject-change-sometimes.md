---
title: Reject Change, Sometimes
link: https://newsletter.kentbeck.com/p/reject-change-sometimes
source: tidyfirst-substack-com
published: 2026-09-02T13:29:36Z
updated: 2026-09-02T13:29:36Z
first_seen: 2026-11-14T02:15:15.124110058Z
authors:
- Kent Beck
summary: Volatility, Shannon's Demon, & Free Money
content: extracted
html: 2026-09-02-reject-change-sometimes.html
remote_preview:
  url: https://substackcdn.com/image/fetch/$s_!glPN!,f_auto,q_auto:best,fl_progressive:steep/https%3A%2F%2Ftidyfirst.substack.com%2Ftwitter%2Fsubscribe-card.jpg%3Fv%3D1077263058%26version%3D9
---

Volatility, Shannon's Demon, & Free Money

An ancient exchange gave each trader sitting at their oak desks two options for investment:

- A strongbox for holding coins.

- A coin flipper that, given some number of coins, would either return twice that many coins or half.

Each day each trader got to choose which investment to make.

Prudence ignored the coin flipper. Sat at their desk all day reading newspapers, caution relieving stress but costing opportunity.

Reckless ignored the strongbox. Making the bet & watching it (sometimes) pay off made trading worthwhile. Reckless’ stack of coins went up & down like a yo-yo (even though yo-yos hadn’t been invented yet).

SD (Shannon’s Demon), sitting between, was curiously busy. After every flip they would rebalance their coins—half in the strongbox & half going into the flipper.

After a while a curious thing happened. Prudence’s stack of coins stayed exactly the same. Reckless’ stack grew & shrank & grew & shrank, half the time up, half the time down. But SD’s stack grew over time. Same investments. Different outcomes. What’s going on?

In discussing long-volatility software development, so far I’ve made it sound like we always want to be long volatility (“embrace change”, anyone?) Alert reader & good friend Kunal Bhalla pointed out the exception, when & how to go short volatility. He also introduced me to a powerful metaphor for building intuition around volatility—Shannon’s Demon. (I love these kinds of intuition sharpeners—see also the [multi-armed bandit](https://en.wikipedia.org/wiki/Multi-armed_bandit).)

My first goal is building an intuition to how SD’s strategy works to create value. Then we’ll apply it to software product development.

Imagine a 2-day sequence of one win & one loss. Reckless will end up exactly where they started. SD, though, will be up. After the win SD will “bank” some of the winnings so the second day’s loss will be smaller than Reckless’. If Reckless is betting 100 coins, they will end up with 100 coins—(100 \* 2 / 2). SD will end up with 50 + (50 \* 2) = 150 after the first day and then 75 (the rebalance) + (75 / 2) = 112.5.

Switch the days, lose then win. Reckless will be exactly the same, (100 / 2 \* 2). SD will have 50 + (50 / 2) = 75 and then 37.5 + (37.5 \* 2) = 112.5.

Here are all four paths through the outcomes.

![](https://substackcdn.com/image/fetch/$s_!ojQ5!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F664d54b4-1632-4e17-8448-02e34c042838_661x168.png)

Reckless ends up with big wins & big losses but usually ends up around flat. SD gives up upside, avoids downside, & generally wins. Reckless hopes the flipper runs hot. SD hopes the flipper just keeps flipping.

The default strategy for dealing with a coin flipper like this (double or half) is to continually rebalance. Extract is the part of software product development most like this. You make a change, maybe you gain some customers or some revenue maybe you lose some.

In such an environment, the SD product strategy makes the most sense. Protect the revenue stream. Take some growth bets. Lower costs. Keep changes reversible as much as possible.

What if we change the flipper so it pays triple on a win & only loses a third on a loss?

![](https://substackcdn.com/image/fetch/$s_!PkM1!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F62f598d1-45e5-4b61-bec0-c685ec407b5c_648x154.png)

The criteria for being “Reckless” is more complicated than what you win versus what you lose. Take the pairs of outcomes—win then lose & lose then win. In the previous payoff scheme, this resulted in no profit. With the less-balanced flipper we see a superior return by going all in on the flipper. Only in the case where we have a string of losses do we come out worse, & who cares about that?

- We’ll move on to the next idea.

- Besides, the potential upside is so big that we move on to another whole game.

The +300%/-33% flipper looks like the Explore payoff (actual numbers chosen at your discretion). So in Explore we go all in on the flipper. Reckless may be reckless, but they aren’t irrational.

Note how we aren’t making an ROI-based decision. Should we play this flipper/strongbox combination? Does it provide a positive expected value? That’s the not the interesting question. The interesting question is *how* we should play it.

Expand is where the model needs to get richer. In Expand we have 3 outcomes each “turn”:

- Succeed at overcoming the next growth bottleneck. (The winning outcome.)

- Die. Lose everything. (The losing outcome. It *really* costs you because it erases all possible future gains.)

- Switch to Extract. (This one is new.)

Where Explore is pure Reckless & Extract is pure Shannon’s Demon, Expand offers a new strategy for creating value:—engineering & operational investment to reduce the probability of death. Could be performance tuning, securing future resources, even things like improving backup & recovery procedures. Create value by:

- Reducing the chance of death, or

- Increasing the chance of getting over the next growth hurdle.

During Expand demand is pulling customers/usage/revenue. Pushing increases risk. Reducing friction sustains growth longer. And all of this is taking place inside a system only vaguely seen & understood. Some day the dials & levers will come into focus. Then you can put Shannon’s Demon in charge.

I’m a programmer. I understand the world so I can program. I program so I can understand the world. Here is [a little simulator](https://github.com/KentBeck/ShannonsDemon.git) I created & played with to help me gain intuition about the message of Shannon’s Demon.

Thanks again to Kunal Bhalla for the reference. One of the luxuries of my career is that really smart people are willing to talk with me.

Play, friends! It’s a way to understand.

**Most teams don’t have a strategy problem. They have an adaptation problem.**\
Your plan was never going to survive contact with reality. The question is whether your organization bends or breaks when it doesn’t.

I help teams bend. *Adapt to Thrive.*

Booking a handful of **custom talks** and **advisory engagements** now. I interview your people, measure your real software flows, and hand you the truth plus what to do about it.

Curious whether it fits? [Tell me about your team.](https://kentbeck.com/kent-beck-sponsorship#inquire)
