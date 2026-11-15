---
title: A visual explanation of reinforcement learning
link: https://aatishb.com/myriad/how-brains-learn/
source: hnrss-org
published: 2026-11-14T13:31:06Z
updated: 2026-11-14T13:31:06Z
first_seen: 2026-11-15T00:13:30.261415141Z
authors:
- aatish
content: extracted
html: 2026-11-14-a-visual-explanation-of-reinforcement-learning.html
remote_preview:
  url: https://aatishb.com/myriad/promo_learning.jpeg
extra:
  content:interactive: true
---

**How does your brain help you learn a new skill?**

To try to answer this question, we’ll build a digital being with a simple brain, and watch it learn a new behavior.

[In our previous article](https://aatishb.com/myriad/lets-build-a-brain/), we created a creature called a spark that could steer towards light. And we saw how small changes to its neural circuitry gave rise to very different behaviors.

Neural\
circuit

Back then, we’d wired these circuits into the spark’s brain by hand. But now, we want it to learn the same behaviors *by itself*.

After all, you didn’t come into this world knowing how to walk, talk or read. While some of your abilities, such as breathing or swallowing, are wired by evolution, you gained others through an arduous process of trial and error.

We want our spark to do the same. To give you an intuition for this learning process, we’re going to hand over the controls to you. **We’d like** ***you*** **to wire the spark’s brain.**

### You wire its brain

But first, we need to tell you what you’ll be wiring. The spark’s brain has just four neurons — two neurons serving as its eyes, and two neurons that wiggle its flippers.

There are **straight connections** between neurons on the same side of its body, **crossed connections** between neurons on opposite sides, and **drive**, which controls the spark’s speed when there’s no light.

straight

crossed

drive

So its brain wiring boils down to just three numbers — the strength of each kind of connection, and the drive.

Now let’s set this spark loose. Use the sliders below to wire its brain as it moves. We’ll keep score.

Your goal: Make the spark love the light by **getting a score above 0.5.**

### Can you make the spark spend time inside the light?

Neural\
circuit

#### Score

Target 0.11

Tries

straight

crossed

drive

The score represents the average amount of light the spark encounters over 100 trials with these settings.

Finding the right configuration takes a lot of trial and error. (If you get stuck, click the hint button above for a nudge in the right direction.)

Let’s visualize what you just did. To guide the spark to the light, you tweaked three numbers. We can represent these numbers as the three axes of a cube.

The payoff of this picture is that we can visualize your spark’s learning trajectory as a path within this cube.

To guide the spark, you probably nudged a slider one way, saw whether this made things better or worse, and then used this feedback to decide your next move.

Let’s distill this into a **learning algorithm** — a step-by-step procedure by which the spark can rewire its brain through practice.

Here’s how it’ll work: The spark gets many tries to steer towards the light. Before every trial, its brain wiring will change a little at random, just like you wiggled the sliders to and fro.

If the trial goes better than previous ones, then the spark gets a reward and its wiring shifts in the direction of this random change. And if the trial goes worse, the spark gets a punishment, and its wiring shifts the other way.

This isn’t that different from how you learned how to walk or ride a bike. Falling down was your punishment, and the thrill of balancing was your reward.

Can this simple algorithm help the spark find the light?

### A spark learns by itself

In the beginning, the spark is a novice, with a brain that’s wired together at random. Unsurprisingly, it doesn’t get very far.

### Before learning: Stuck

Click to move the light around.

Drag the light to move it around.

The baby spark can spin in place, but it can’t yet move around. But it’s about to get better…

### After 50 tries: Meandering

Click to move the light around.

Drag the light to move it around.

After 50 tries of this learning algorithm, our infant spark has taken its first steps. And while it won’t break any speed records, it’s now on the move…

### After 250 tries: Avoiding

Click to move the light around.

Drag the light to move it around.

After 250 tries, our juvenile spark has picked up speed. But it veers away from the light at the last moment.

### After 500 tries: Charging

Click to move the light around.

Drag the light to move it around.

After 500 tries, our adolescent spark boldly charges towards the light. But it hasn’t yet figured out how to slow down or stop.

### After 2,000 tries: Expert

Click to move the light around.

Drag the light to move it around.

After 2,000 tries, our spark is now an adult — an expert at seeking and following the light. It clings to the light and can turn on a dime. (Try moving the light to see the spark follow it.)

Like before, we can visualize this learning process as a path.

#### The spark’s learning path after 0 tries...

This isn’t a path through physical space. Instead, it’s a path through **the space of possible brains**. It shows how the spark’s brain changed as it learned.

Other novice sparks would start from different points, and they would each trace out their own learning trajectory.

#### Many sparks’ learning paths after 0 tries...

This tangled web illustrates the learning paths of hundreds of novice sparks, each starting with a differently wired brain. As if by magic, all of these threads converge to the point marked ♥︎.

The paths aren’t straight lines: Learning isn’t linear. There are setbacks, wrong turns and mistakes along the way. And it isn’t necessarily fast. Some of the learners took thousands of tries to reach their goal.

You’ll notice that the paths are all jittery. That’s because every step involves randomness. We can smooth out this jitter by averaging over many trajectories at every point, revealing a map of the flow of learning.

### The sparks learn to love

It’s worth taking a moment to appreciate how surprising this is — given enough tries, the same learning process turned every novice into an expert light seeker.

Of the myriad possible arrangements of this spark’s brain, only a tiny fraction generate the behavior that we want. There are far more ways to fail than to succeed.

And yet, you’ve just witnessed a small miracle. The spark navigated this labyrinth of dead ends and rewired its brain to find the light, guided by nothing but reward and punishment.

### Learning new tricks

Learning doesn’t stop once you become an adult. We learn throughout our lives and adapt to changing circumstances.

Our sparks can do this too. For example, what if the light represents a harmful chemical or a dangerous predator instead of a food source?

The spark is now rewarded for **avoiding the light** rather than for seeking it, and this leads to new behaviors:

### The sparks learn to flee or freeze

If the light is harmful, then **LOVE** becomes the worst strategy, and the sparks learn to **FLEE** (escape as fast as possible) or to **FREEZE** (play dead).

If instead the spark is rewarded for approaching the light with high speed, then it learns to charge towards the light, i.e. to **FIGHT**.

### The sparks learn to fight

Much like a child learns from its environment, the spark can learn very different behaviors — fleeing, freezing, fighting or following the light — depending on how it’s rewarded.

### A spark is shaped by its environment

Neural\
circuit

· Spark is shown moving at two-thirds speed.

You’ve now seen how a novice spark can rewire its brain and learn new behaviors through a process known as reinforcement learning.

Biological brains also learn through reinforcement. For example, baby songbirds learn to sing by babbling. They [try out random variations](https://pmc.ncbi.nlm.nih.gov/articles/PMC1069649/) of their song. If a variation gets closer to their memory of an adult’s song, they [get a burst of dopamine](https://www.nature.com/articles/s41586-025-08729-1) (and if it gets further away, the dopamine dips). Guided by these rewards and punishments, they can recreate the song from memory, even [with no teacher present](https://www.sciencedirect.com/science/article/abs/pii/S0003347205808838)!

Now, we do have to come clean about something. This spark’s brain had just four neurons and three numbers that determined how these neurons were connected. This simple structure allowed us to watch its brain learn in three dimensions.

Your brain has around 86 billion neurons and over a hundred trillion connections. The space of possible ways in which it could be wired is therefore unimaginably vast.

But while the details are much more complicated, the underlying idea is the same: Learning is how you navigate this vast internal space. And feedback is the compass that points the way.

## Learn more

Thanks for reading chapter two of [Myriad](https://aatishb.com/myriad/), a series exploring how minds emerge from matter. Myriad is a collaboration between [Arkarup Banerjee](https://www.arkabanerjeelab.com/team), a neuroscientist who studies behavior, and [Aatish Bhatia](https://aatishb.com/), a physicist turned science journalist.

#### How the spark moves

The sparks in this article are inspired by the work of the neuroscientist [Valentino Braitenberg](https://en.wikipedia.org/wiki/Valentino_Braitenberg). We implemented their neural circuitry as follows:

$\begin{aligned}v_L &= \sigma(\textcolor{#7ee081}{w_s}\, b_L + \textcolor{#c49bff}{w_c}\, b_R + \textcolor{#ff8fb8}{d})\\ v_R &= \sigma(\textcolor{#c49bff}{w_c}\, b_L + \textcolor{#7ee081}{w_s}\, b_R + \textcolor{#ff8fb8}{d})\end{aligned}$

where $v_L$ and $v_R$ are the left and right flipper speeds, $b_L$ and $b_R$ are the brightness levels at the left and right eyes, $\textcolor{#7ee081}{w_s}$ and $\textcolor{#c49bff}{w_c}$ are the straight and crossed connection strengths between the eyes and the flippers, and $\textcolor{#ff8fb8}{d}$ is a drive that sets the spark’s speed when there is no light. $\sigma(x)$ is the [ReLU function](https://www.geeksforgeeks.org/deep-learning/relu-activation-function-in-deep-learning/). The spark’s forward speed is the average of the two flipper speeds, and its turning speed is proportional to their difference.

#### How the spark learns

There are three parameters in our simplified setup: $\textcolor{#7ee081}{w_s}\textrm{,}$ $\textcolor{#c49bff}{w_c}$ and $\textcolor{#ff8fb8}{d}\textrm{.}$ The spark learns values for these parameters using a version of the [REINFORCE algorithm](https://link.springer.com/article/10.1007/BF00992696) published by Ronald Williams in 1992.

Each trial, the parameters were wiggled by a small random amount and the spark moved for 500 time steps, starting from a random point on the edge of the arena. Its reward was the average brightness along its path.

This reward was then compared to a running average of recent rewards. If the spark did better than usual, its parameters were nudged in the direction of this random change. If it did worse, its parameters were nudged in the opposite direction. The size of the nudge was proportional to how much better (or worse) it did:

$\small \underset{\mathclap{\htmlClass{sans}{\text{change}}}}{\Delta W\vphantom{(\overline{R})}}\hspace{0.3em}=\hspace{0.5em}\underset{\mathclap{\htmlClass{sans}{\color{#66b3ff}\text{step size}}}}{\textcolor{#66b3ff}{\alpha}\vphantom{(\overline{R})}}\hspace{2.1em}\underset{\mathclap{\htmlClass{sans}{\color{#ff9b32}\text{wiggle}}}}{\textcolor{#ff9b32}{\varepsilon}\vphantom{(\overline{R})}}\hspace{1.2em}\underset{\mathclap{\htmlClass{sans}{\color{#ffc832}\text{improvement}}}}{\textcolor{#ffc832}{(R - \overline{R})}\vphantom{(\overline{R})}}$

Here $\Delta W$ is the change in the three parameters, $\textcolor{#66b3ff}{\alpha}$ is the learning rate, $\textcolor{#ff9b32}{\varepsilon}$ is the random wiggle, and $\textcolor{#ffc832}{R - \overline{R}}$ is how much better or worse than usual the spark did. We repeated this thousands of times.

[Williams’ original approach](https://link.springer.com/article/10.1007/BF00992696) adds randomness to each neuron’s output. We add it to the weights and biases instead, an approach known as [weight perturbation](https://link.aps.org/doi/10.1103/PhysRevX.13.021006). A similar idea, called [evolution strategies](https://openai.com/index/evolution-strategies/), was used by OpenAI in 2017 to teach AI models to play Atari video games.

Sign up below for more rigorously researched visual explanations of neuroscience, and share this article with others who might enjoy it.
