---
title: Simple circuits can create complex behaviors
link: https://aatishb.com/myriad/lets-build-a-brain/
source: hnrss-org
published: 2026-10-27T16:01:06Z
updated: 2026-10-27T16:01:06Z
first_seen: 2026-10-31T19:30:57.182345052Z
authors:
- aatish
content: extracted
html: 2026-10-27-simple-circuits-can-create-complex-behaviors.html
remote_preview:
  url: https://aatishb.com/myriad/promo_behaviors.jpeg
extra:
  content:interactive: true
---

**The world is full of surprising behavior:** Beavers building dams, songbirds singing, spiders building webs, and you reading this article.

All of these behaviors are orchestrated by neurons firing in the brain. But how exactly do collections of neurons create these complex behaviors?

This is a huge question that researchers are still working on. But over the last half century, neuroscientists have solved some pieces of this puzzle.

In this article, we’ll build a tiny creature called a spark, and we’ll give it a very simple brain. As you’ll see, even this tiny brain can create rich and unexpected behaviors.

### Light eaters

Suppose this spark feeds on light. Its goal in life is to reach the light.

The light-hungry spark needs a way to sense the world. So we’ll give it a rudimentary eye — a single neuron that detects brightness and converts it into electrical pulses.

It also needs a way to move. So we’ll give it another neuron that can wiggle a flipper.

By wiring the first **sensory neuron** to the second **motor neuron**, we’ve just assembled our first neural circuit:

### The anatomy of a spark

With this simple circuit, the spark wiggles its flipper in proportion to the amount of light.

Can it find the light?

If the sparks happen to face the light, they’re drawn towards it. But most of the time, they miss.

Over time, their paths look like this:

Only a few paths cross the light. Clearly, we can do a lot better…

### Adding an eye

Here’s the problem: While the spark can speed up, it can’t steer. With one eye and one flipper, it only moves in one direction. But we need it to turn.

What if we give it another eye and flipper?

With two eyes, the spark can now sense whether the light is to its left or right. Now, it moves like this:

Hmm… that didn’t work. Instead of steering towards the light, it steered *away* from it!

If you didn’t know how their brains work, you might think that they **fear** light.

Why do they avoid the light? Well, if the light were to the spark’s left, then its left eye would sense more brightness, which means that its left flipper would wiggle faster, and this would cause it to turn right, away from the light. Conversely, if the light were to its right, then it would veer left.

### A crossed connection

But there’s a clever way to make this spark steer towards the light instead.

All we have to do is cross its connections. That is, we wire its left eye to its right flipper, and its right eye to its left flipper.

Now, it moves like this:

With this simple wiring change, it can finally steer towards the light.

But if you watch these sparks for a while, you’ll notice that there’s a problem.

While they can reach the light, they don’t slow down near it. Instead, they charge towards it like comets plunging towards the sun.

If you didn’t know how their brains work, now you might think that they’re **attacking** the light.

### Becoming inhibited

Why do they attack the light? The problem lies in the *type* of connection that we used.

To see why, we need to go back to our original spark. Remember how its flipper moved faster when the light got brighter? That’s because its eye and flipper were connected with an [excitatory connection](https://en.wikipedia.org/wiki/Excitatory_postsynaptic_potential) — where more input leads to more output.

If the light were a food source, then this spark would accelerate towards it, but it wouldn’t stop to enjoy its lunch.

Instead, we want the spark to slow down as it approaches the light. That is, we want its motion to be *inhibited* by light.

We can solve this by swapping out the excitatory connection for an [inhibitory connection](https://en.wikipedia.org/wiki/Inhibitory_postsynaptic_potential) — one where more input leads to *less* output.

With the original excitatory connection, the spark overshoots the light. With the inhibitory one, it slows down and gently comes to a stop.

### Finding the light

Now let’s bring back the original two-eyed spark, but this time with inhibitory connections between its eyes and flippers.

Now, the flipper closer to the light moves slower, while the flipper on the opposite side moves faster, and this makes the spark turn towards the light.

Click to move the light around.

Drag the light to move it around.

Hooray! It can finally steer to the light and come to a stop. You can move the light in the graphic above (or the one below) to watch the sparks react.

Click to move the light around.

Drag the light to move it around.

They move as if they’re magnetically attracted to the light. If you didn’t know how their brains work, now you might think that they **love** light.

We’ve now seen four very different behaviors, each achieved by making small changes to the neural circuitry of our virtual creatures.

Note: We’ve added a small amount of noise to the simulation to make the paths look more organic.

If you saw creatures behaving like this in the wild, you might attribute some rudimentary intelligence to them.

But we’ve just shown you how this apparent intelligence arises from the bottom up, with just four neurons and two connections. There’s no magic here. In this view, **behavior emerges from neural circuits.**

And if a brain with just four neurons can do all this, imagine what your brain can achieve with nearly 100 billion neurons.

### Adding variation

We can take this one step further.

In each of the simulations so far, the sparks all had identical brains. But biology is full of variation: No two brains are the same.

If we allow the connections between neurons to vary in strength, then each spark can react differently to the light.

They now have different preferences: The yellower sparks seem to love light more, while the redder sparks keep a cautious distance.

Click to move the light around.

Drag the light to move it around.

Just as a thermostat sets a preferred temperature, their brain wiring now determines their preference for light.

If you squint very hard at this, you might be able to see the rudiments of how *our* brains encode preferences, such as a love for spicy food or an aversion to smelly cheese.

Of course, biology is far more complex than these simplified sparks. We intentionally gave them very simple brains, so that we can understand how every part works.

Their brains are also frozen in time. In reality, brains can learn: They alter their wiring to gain new skills such as learning to walk, ride a bicycle, or play a piano.

In the next article, we’ll watch this learning process in action, and see how these sparks rewire their brains to find the light.
