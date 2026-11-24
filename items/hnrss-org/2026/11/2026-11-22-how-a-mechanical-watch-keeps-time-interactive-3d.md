---
title: How a Mechanical Watch Keeps Time (Interactive 3D)
link: https://martincmartin.github.io/slidev-stem/watches/article/1/
source: hnrss-org
published: 2026-11-22T13:21:25Z
updated: 2026-11-22T13:21:25Z
first_seen: 2026-11-24T17:30:21.295672362Z
authors:
- martincmartin
content: extracted
html: 2026-11-22-how-a-mechanical-watch-keeps-time-interactive-3d.html
remote_preview:
  url: https://martincmartin.github.io/slidev-stem/watches/article/1/figures/what-everyone-should-know-about-mechanical-watches-0.png
  alt: 'The whole watch, running: its wheels, its balance wheel and its hands, inside the case.'
---

Mechanical watches have kept time for 500 years. Many of their core parts were invented even earlier for giant cathedral tower clocks before being shrunk into portable timepieces. This page explains how a modern one works. I personally think it’s fascinating that a well-calibrated, purely mechanical device can keep time with 99.99% accuracy for $100. They also changed the world and look good on your wrist.

This web page was partially inspired by Bartosz Ciechanowski’s [Mechanical Watch](https://ciechanow.ski/mechanical-watch/) page. You can think of this page as a simpler introduction or background. When you’re finished with this, that page gives more depth.

You can drag any of the 3D figures to turn them and see the parts from the side; on a phone, drag sideways.

![The whole watch, running: its wheels, its balance wheel and its
hands, inside the case.](https://martincmartin.github.io/slidev-stem/watches/article/1/figures/what-everyone-should-know-about-mechanical-watches-0.png)

## Hairspring

c. 1675

![A hairspring, seen at an angle: a flat spiral of steel ribbon. Its
outer end is held in a small block, and its inner end is on a hub at the
center. The spiral winds and unwinds.](https://martincmartin.github.io/slidev-stem/watches/article/1/figures/hairspring-0.png)

Start with the part that keeps time. In 1675 Christiaan Huygens, a Dutch scientist, invented the *hairspring*. It is a flat spiral of steel ribbon, finer than a hair. Its outer end is held still, and its inner end is free to turn. Twist the inner end and let go, and the spiral winds and unwinds, back and forth, at a steady rate. In this drawing it swings once every two seconds, and the rest of the watch follows that beat.

![A diving board over a pool. It bends, is let go, and swings up and
down past its straight shape, which a dashed line shows. The label reads:
\"Bend it away from straight, it pushes back\".](https://martincmartin.github.io/slidev-stem/watches/article/1/figures/hairspring-1.png)

Why does a spring swing? Think of a diving board. Bend it down, and it pushes back: an *elastic restoring force*. Let go, and it does not stop at straight. It overshoots, bends the other way, and comes back again: it *oscillates*. With no friction, it would oscillate forever.

A hairspring is a diving board rolled up. The board’s rest shape is a straight line; the hairspring’s is a spiral. Take hold of the slider under the next picture, and drag it to the right.

![The diving board curls up into the hairspring, at rest.](https://martincmartin.github.io/slidev-stem/watches/article/1/figures/hairspring-2.png)

“Bend” becomes “wind”, and “straight” becomes “rest”. Now give the hairspring a twist: drag the slider to wind the spiral away from its rest shape, and let go.

![The hairspring at rest. Wound away from rest and let go, it swings
back and forth.](https://martincmartin.github.io/slidev-stem/watches/article/1/figures/hairspring-3.png)

It pushes back. It overshoots and swings, just as the board did. This one move is the whole idea of the hairspring.

## Balance Wheel

c. 1364

The hairspring is only half of the timekeeper. The other half is the *balance wheel*. While most of its mass is around the outside, it is actually fixed to the spring’s inner end. The spring provides the *restoring force*. The wheel provides the *mass* and *inertia*.

![The balance wheel on the hairspring, swinging. The wheel is
marked.](https://martincmartin.github.io/slidev-stem/watches/article/1/figures/balance-wheel-3.png)

A hairspring alone would oscillate far too fast to count. The wheel slows it to a pace that the gears can count.

Mass also makes the swing steady. Push an empty playground swing, and it jerks about. Put a child on it and it swings much more gracefully. A heavy wheel carries its own momentum, so a knock to your wrist, or turning it upside down, barely changes its rhythm.

How good is it? A well-calibrated mechanical watch, even an everyday Seiko or Citizen, is about 99.99% accurate. A day has 86,400 seconds, so that watch gains or loses about nine seconds a day.

I’m not particularly handy with mechanical things. If I tried to build a table it would wobble because things would be off by a few percent. So it kind of blows my mind that, for $100, I can get a simple, everyday, purely mechanical system and tweak it to be 99.99% accurate!

## Escapement

c. 1275

A second hand that just moves back and forth isn’t very useful. We need it to turn one way only. The *escapement* turns the swing into steps.

The *escape wheel* is being turned by a mechanism we’ll explain soon, so it’s hidden on this diagram. Without something to stop it, it would just turn and turn.

![The escape wheel alone, spinning freely. A curved arrow shows the
push on it.](https://martincmartin.github.io/slidev-stem/watches/article/1/figures/escapement-1.png)

The *pallet fork* stops it.

![The balance wheel on the left, the pallet fork in the middle, and
the escape wheel on the right. A stone at the end of the fork holds a tooth
of the wheel. Labels: \"Wheel driven by hidden mechanism\" and \"Pallet blocks
wheel\".](https://martincmartin.github.io/slidev-stem/watches/article/1/figures/escapement-2.png)

Its two prongs (called *stones*) stick in the wheel’s teeth: a tooth lands on a stone, and the wheel stops. Each time the fork rocks, one stone lets a tooth go, and the other stone catches the next tooth. The tooth *escapes*, and that is where the name comes from.

What rocks the fork? The balance does. On its staff is a small *impulse jewel*, which sits in the notch at the end of the fork. It’s actually a synthetic ruby or sapphire, so it really is a jewel, a precious stone.

![The escapement at rest, labeled: the escape wheel, the pallet
fork, and the impulse jewel in the fork's notch.](https://martincmartin.github.io/slidev-stem/watches/article/1/figures/escapement-3.png)

As the balance swings through the middle, the jewel strikes the fork and flips it one way. On the swing back, it flips it the other way.

![The escapement at work. The balance swings, its jewel strikes the
fork, the fork rocks, and the escape wheel moves on one tooth. \"Tick\" and
\"tock\" show each time a tooth lands on a stone. Labels: \"Jewel strikes
pallet\", \"Wheel driven by hidden mechanism\" and \"Pallet blocks wheel\".](https://martincmartin.github.io/slidev-stem/watches/article/1/figures/escapement-4.png)

So, at each swing, the balance lets the wheel take one small step. The step ends when the next tooth lands on a stone, hard enough to make a sound. This is the tick and the tock of a watch.

## Ticks and Turns

Before we go on, something that trips people up until they think it through. How long does the second hand take to go once around the dial?

![The second hand alone, pinned on its arbor, ticking once a
second.](https://martincmartin.github.io/slidev-stem/watches/article/1/figures/ticks-and-turns-3.png)

Each tick is one second, and sixty ticks brings it all the way around. So the second hand makes a complete circle once a minute. A hand is named for what it counts, not for how long it takes to make a full 360° turn.

| Hand        | A full turn   |
| ----------- | ------------- |
| Second hand | Once a minute |
| Minute hand | Once an hour  |
| Hour hand   | Twice a day   |

## Gears

Second hand: c. 1670

The escapement gets the escape wheel turning at a precise rate. But the second hand must turn once a minute, and the escape wheel turns faster than that. To convert a faster rotation into a slower one, a watch uses gears: a small gear that meshes with a big one.

![Two gears, turning: a small gear with eight teeth drives a big
gear with sixteen. Four teeth on each gear are colored.](https://martincmartin.github.io/slidev-stem/watches/article/1/figures/gears-4.png)

The teeth on the two gears must have the same spacing, so they mesh neatly. Each tooth fits in a gap of the other gear, so the teeth go past each other one for one, and both rims move at the same speed. Four teeth on each gear are colored. Those four teeth are half of the small gear, but only a quarter of the big one. So the small gear turns twice for each turn of the big one: twice the circumference, half the speed.

![The pinion, now under the escape wheel, meshes with the big gear,
and the pallet fork is beside the escape wheel. The second hand is on the big
gear's axle, and steps around clockwise, once a minute.](https://martincmartin.github.io/slidev-stem/watches/article/1/figures/gears-6.png)

Now attach the smaller gear (called a *pinion*) to the escape wheel. The gears do not spin freely any more: they move on one tick at a time, with the escape wheel. The escapement controls the speed of the whole train of gears.

The big gear pushing the small gear (pinion) is the force turning the escape wheel. What turns the big wheel? That’s a mechanism we’ll explain soon.

In this watch, the big gear (called the *fourth wheel*) turns exactly once a minute: just what the second hand needs! So the second hand goes on the big gear’s axle. It is the first part of the watch that we can read.

## Gear Train

Minute hand: c. 1656

The second hand turns once a minute. The minute hand must turn once an hour: sixty times slower. So we could try the same trick, and have a gear that’s 60 times bigger than the small gear.

![The watch, with a gear sixty times the size of the second hand's
pinion drawn in place. The gear is far larger than the whole watch, and sticks
out of the case.](https://martincmartin.github.io/slidev-stem/watches/article/1/figures/gear-train-1.png)

But there’s a limit to how small you can manufacture the small gear, so the big gear would be huge. And it would have a lot of teeth which adds to the cost. Plus, it would turn the wrong way, counterclockwise, because every gear turns the opposite direction from the one it meshes with.

![Two more pairs of gears, between the second hand's wheel and the
center wheel.](https://martincmartin.github.io/slidev-stem/watches/article/1/figures/gear-train-2.png)

So the watch slows in two stages. The *third wheel* runs 7½ times slower, and the *center wheel* runs 8 times slower than that.

7½ times 8 is 60, so the center wheel turns once an hour.

![The minute hand on the center wheel's arbor, in the middle of the
case.](https://martincmartin.github.io/slidev-stem/watches/article/1/figures/gear-train-4.png)

The minute hand goes on the center wheel’s axle (*arbor*), in the middle of the dial. It makes a full turn once an hour.

The center wheel pushes the third wheel which pushes the fourth wheel which pushes the escape wheel. Next up, the mechanism that pushes the center wheel and powers the whole watch.

## Mainspring

c. 1430

![The bare mainspring: a long ribbon of steel, coiled around its arbor,
at rest.](https://martincmartin.github.io/slidev-stem/watches/article/1/figures/mainspring-0.png)

This is a spiral spring, like the hairspring. But instead of swinging back and forth, it does one slow unwind over a day or two. In the illustration above, it’s at rest.

Below, the outer end is held in place. Drag the slider to wind it up. The spring keeps its length, so its coils draw in toward the center.

![The mainspring on its arbor. A stud holds its outer end, and a key
turns the arbor.](https://martincmartin.github.io/slidev-stem/watches/article/1/figures/mainspring-1.png)

When wound away from its rest shape, the dashed line, it pushes back, as the hairspring does. A tiny one-way ratchet locks the axle (*arbor*) so it can only turn one way. To unwind the mainspring, let go of the outer end by pressing the button.

![The wound mainspring, its outer end held by the stud.](https://martincmartin.github.io/slidev-stem/watches/article/1/figures/mainspring-2.png)

The block is gone, and the outer end swings around until the spring lies on its rest shape again. That fling is the energy that the winding put in.

A watch harnesses that force by attaching the spring’s outer end to a toothed drum called the *barrel*, invented by Jean-Antoine Lépine around 1760.

![The mainspring in its barrel, driving the whole movement. Then six
hours pass in six seconds.](https://martincmartin.github.io/slidev-stem/watches/article/1/figures/mainspring-4.png)

As the mainspring pushes the barrel, the barrel’s teeth drive the gear train we just built. This is the “hidden mechanism” we mentioned earlier.

Because the ratchet stops the arbor from turning backwards, the spring has only one way to unwind: by its outer end turning the barrel. Press the button under the figure, and six hours pass in six seconds. The barrel turns once, the minute hand six times, and the coils move back out to the wall. When the spring is completely unwound, the watch stops.

## Hour Hand

Hour hand: c. 1350

The hour hand must turn twice a day. That’s once every twelve hours. Unlike the 60:1, a 12:1 gear is doable.

But the hour hand on that wheel’s axle (*arbor*) is off center! And it would run backward (counterclockwise): a pair of meshing gears turn opposite directions.

![The movement, with a 12:1 gear beside the center wheel. The hour
hand is on that gear's axle, far from the center of the dial.](https://martincmartin.github.io/slidev-stem/watches/article/1/figures/hour-hand-2.png)

So we use the same two-step trick we used for the minute hand. This time, 3:1 going out and 4:1 coming back. 3:1 × 4:1 = 12:1.

![The motion works, close up: the cannon pinion, the minute wheel
and its pinion, and the hour wheel. The rest of the watch is dimmed.](https://martincmartin.github.io/slidev-stem/watches/article/1/figures/hour-hand-5.png)

The hour hand goes on the hour wheel, in the middle, under the minute hand, attached to a hollow tube (pipe) that slips *over* the minute hand’s axle. It turns clockwise, twice a day.

![The finished movement. The hour hand and the minute hand share the
center of the dial; the hour hand is the shorter one. The barrel is at the
bottom.](https://martincmartin.github.io/slidev-stem/watches/article/1/figures/hour-hand-6.png)

## Recap

And that’s how a mechanical watch works!

Here’s a recap, retracing our steps in reverse, from the hour hand to the hairspring. The tour plays by itself, like a short film. The time scale in the corner says how much faster than real time each segment runs.

![A tour of the watch's train, from the motion works to the
balance wheel, one touch at a time.](https://martincmartin.github.io/slidev-stem/watches/article/1/figures/recap-0.png)

The hour hand is driven by a pair of gears totaling 12:1.
