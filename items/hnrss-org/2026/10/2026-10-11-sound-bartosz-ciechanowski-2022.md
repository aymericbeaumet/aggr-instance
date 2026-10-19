---
title: Sound – Bartosz Ciechanowski (2022)
link: https://ciechanow.ski/sound/
source: hnrss-org
published: 2026-10-11T16:46:43Z
updated: 2026-10-11T16:46:43Z
first_seen: 2026-10-14T13:03:36.501235501Z
authors:
- dverlaeckt80
content: extracted
html: 2026-10-11-sound-bartosz-ciechanowski-2022.html
extra:
  comments_url: https://news.ycombinator.com/item?id=50045147
  num_comments: 21
  points: 158
---

Invisible and relentless, sound is seemingly just there, traveling through our surroundings to carry beautiful music or annoying noises. In this article I’ll explain what sound is, how it’s created and propagated.

Throughout this presentation you will be hearing different sounds, which you will often play yourself on little keyboards like the one below. You can either click its keys with your mouse or use WER keys on your computer keyboard, but before you do so make sure your system volume is at a reasonable level: You can press its keys with your fingers, but before you do so make sure your system volume is at a reasonable level. You may also need to unmute your device:

We’ll eventually understand how these sounds are created and how they get to your ears, but we have to start by talking about the medium that is most commonly associated with sound – air.

## Air [![](https://ciechanow.ski/images/anchor.png)](https://ciechanow.ski/sound/#air)

You’re probably aware that at microscopic level air consists of individual, tiny particles. Let’s look at the behavior of those molecules up close. In the demonstration below, I parceled out a tiny cube of air – notice how the molecules freely enter and leave this space. You can drag the cube around to change the viewing angle and you can also control the speed of the flow of time with the slider:

I need to note that this visualization simplifies things a little. The air consists primarily of nitrogen and oxygen – their particles contain two atoms bound together, but for simplicity I’m drawing all molecules as simple spheres.

Each particle of air travels through space with some velocity in a more or less random direction. While some molecules are faster, and some are slower, at room temperature the average velocity of a particle is a staggering 1510 ft/s 460 m/s so to make the particles visible I’m showing their motion *significantly* slowed down. As a side note, I’m using imperial metric units here, but you can [switch](https://ciechanow.ski/sound/#) to the metric the imperial system if you prefer.

With all the commotion in the previous simulation it may have been hard to see that the air particles collide with each other. In the demonstration below, I’m flashing the particles **red** and making them a little bigger right after they collide. To make these events easier to see you can control the speed of time with the slider:

Naturally, those particles can also collide with other things in the environment. In the simulation below I put up walls around that parcel of air. While at this scale we could also see the individual atoms constituting the walls of the box, I’m not showing them here to make things clearer. The particles of air are bouncing off these sides and every time that happens I’m drawing a little **blue glow** on the wall:

When an air particle bounces off, it imparts a small force on the wall. Although a single collision doesn’t do that much, with enough particles present the walls experience a constant barrage of collisions which exert expanding *pressure* on the container. The magnitude of that pressure is the sum of all these forces of impacts divided by the area of the box. Intuitively, for a box of a fixed size, the more particles there are, the heavier they are, and the faster they move, the higher the pressure.

Let’s see what happens if we resize this box a little while keeping the number of particles constant. In the demonstration below, you can drag the slider to change the **length** of the box, the plot in the bottom tracks the number of collisions *per area* of the box in a unit of time:

The number of collisions per area, and thus the pressure, increases as we [squeeze](https://ciechanow.ski/sound/#) particles closer together, and it decreases as we [let them loose](https://ciechanow.ski/sound/#). Remember that we’re observing a significantly slowed down simulation. Because of the speeds involved here, I’m only letting you resize the box at a limited pace. That limitation prevents the walls from moving at enormous speeds, which would create some truly extreme pressure conditions.

It may not be immediately clear, but the experience of pressure is not limited to the walls of the box we built. Molecules in any *open* region of gas also experience compressing forces exerted by collisions with molecules from the neighboring regions. Those collisions aren’t as neatly aligned in space, but in aggregate it makes no difference if particles are bouncing off a wall or off particles from the neighboring area – the pressure is still there.

All the collisions between particles also have other consequences, but to look at them I need to adjust the scale at which we’ll be looking at – you can do this with a slider below:

The **small cube of air** from the previous demonstration is now just part of a **larger volume**. While this **large cube** is 20 times larger in each dimension, its sides measure only 1 micrometer – a few times smaller than a width of spider silk. Unfortunately, it would be both chaotic to display and taxing to correctly simulate the over 26 million air particles that one could find in this area. Instead, I’ll just visualize the motion of a single molecule as it travels through this space. In the demonstration below, I enlarged that particle to make it visible, you can also control the flow of time with the slider:

Due to collisions with other molecules, it may take a while for this particle to travel a significant distance and get out of this larger volume. You probably experienced this yourself – when someone sprays perfume in a distant part of a room the scent doesn’t instantly get to you. It’s only after some lucky aromatic particles manage to bounce into the vicinity of your nose that you get to experience the smell. If it wasn’t for the collisions with other molecules, those fragrant particles could travel the distance to your nose in a fraction of a second.

The important point is that, while the air particles move very fast, the collisions with other particles keep them mostly contained inside their local area. Over a longer period of time they will eventually all mix up, but at the short timeframes we’ll be interested in here, each reasonably sized parcel of air contains mostly the same particles.

All the microscopic behavior of individual particles we’ve observed ends up contributing to properties of air at larger scales. From this point on, we’ll move past looking at individual molecules and instead we’ll focus on regions of air that more closely match the sizes we’re used to. Those particles are still there, and they’re what actually end up distributing changes in gases, but we need to aggregate a huge collection of the molecules to end up with more discernible motions.

Let’s witness some of this large scale behavior when a section of air in a tube gets pushed on by a moving **plate**. In the demonstration below, I divided the air next to that **circular plate** into individual slices, which I’m drawing with thin lines. Although these boundaries are imaginary, they let us see what happens to different sections of air as you move the **plate** around with the slider:

Notice that the individual parcels of air move only a little from their initial position, but the *disturbance* caused by the motion of the **plate** seems to propagate through them with some speed. Moreover, that disturbance moves away from the **plate** regardless of the direction of the plate’s motion.

If you recall our experiment with a resized cube of air having more or less available space, you’ll notice that a very similar situation happens here inside each slice – this tells us that we’re witnessing some changes in pressure.

Let’s look at this situation from the side, which will make it easier to see how these pressure changes propagate. In the following demonstration I’m coloring each slice of air **red** when it’s compressed and the **pressure increases**, and I’m coloring it **blue** when it’s expanded and the **pressure decreases**:

When the **plate** moves to the right, it compresses the air particles in its vicinity, which **increases** the pressure in this region. Those particles then push and expand into particles in the neighboring slice of air, which then push onto the next slice and so on. All of this results in an area of **higher pressure** propagating away from the plate.

On the flip side, when the plate moves to the left, it creates an area of depletion, or *rarefaction*, which the neighboring slice of air rushes to fill in. This then **reduces the pressure** in the vicinity of the next slice and so on. While the slices move towards the receding **plate**, the pressure *disturbance* still moves *away* from the plate.

What we’ve just seen more clearly were *pressure waves* propagating through the air. When those pressure waves reach our ears we perceive them as *sound*. By moving the **plate** we’ve effectively created a very crude speaker that emits sounds as its moving element changes position.

While this simple demonstration helps to build an intuitive understanding of the propagation of pressure waves, the relation between the vibration of the object and the pressure disturbances it generates is much more complicated. We’ll soon explore some of those details, but for now we’ll focus on the relation between the movement of that vibrating element and the sound it produces.

## Making Sounds [![](https://ciechanow.ski/images/anchor.png)](https://ciechanow.ski/sound/#making-sounds)

Let’s begin by introducing a simple musical instrument. By pressing a key down, you can request the **plate** to rapidly change its position. When you let that key go, the plate will move back by the same amount. Notice that each of **the** **three** **keys** moves the plate by a different amount, with **red marker** showing the total offset:

Despite its simplicity, this device lets us observe a few things. Firstly, a sudden jump of a **plate** creates a popping sound. It also doesn’t matter if the object has jumped forwards or backwards – the effect sounds the same.

Secondly, the loudness of that pop depends on the magnitude of the jump – the larger the jump the louder the pop.

Finally, this plate can jump forwards or backwards from any position – you can verify this by holding one of the keys and then pressing some other key. That pop sounds exactly the same as the sound created by a standalone key press.

Three pops of different loudness is not much to create music with as this only lets us play some simple beats. You may have tried pressing the buttons as fast as possible to create more upbeat tunes, but there are limits to how fast human fingers can press buttons.

Thankfully, we can ask the device you’re using to repeat the pops at a certain rate. In the demonstration below, you can drag the first slider to change **how many times per second** the **plate** should jump forwards and backwards. You can also control **how far the plate moves** with each jump using the second slider:

To describe the **frequency** of vibration, I’m using the unit of [hertz](https://en.wikipedia.org/wiki/Hertz), often abbreviated as **Hz** – it describes how many *full* cycles of motion happen within a single second. For example, a [frequency of 2 Hz](https://ciechanow.ski/sound/#) means that two full cycles happened in a second which means that the plate moved back and forth *and* back and forth again.

While at lower frequencies these patterns sound like a rapid series of pops, at higher frequencies these sounds blend together into a discernible note. The most important observation we can make here is that the higher the frequency with which the plate vibrates, the higher the perceived [*pitch*](https://en.wikipedia.org/wiki/Pitch_\(music\)) of the resulting sound is.

You’ve probably observed that at lower rates you could follow the motion of the vibrating **plate**, but as the frequency increases, it becomes impossible to reliably track its movement. Unfortunately, we’re hitting the limits of frequencies we can easily display on a screen and perceive with our eyes, so from this point on I’ll significantly slow down the motion of the **plate**. The *patterns* of motion will still be correct, they’ll just happen at a much slower pace so that we can actually see what’s going on.

With that in mind, let’s make our music synthesizer create those very frequent jumps of the **plate**. We’ll assign a different frequency to each key – you can see them displayed above the keys. I’m also **drawing** **three** **plots** corresponding **to** **each** **key**, and an **additional plot** showing the *cumulative* effect of the individual motions.

These plots, or *waveforms*, represent the motion of the plate as requested by the keys. As the plots pass through the center line they reflect the current position of the plate, but remember that the motions we see here are a few hundred times slower than the actual ones:

Notice that when you press two keys at the same time you can still hear both sounds playing through – we’ve already seen that the final **plate** movement is just the sum of individual motions that each key requests.

You may also have realized that I’ve shifted the center line of the plate. What we consider a base position of that object is ultimately arbitrary. The keys simply request the plate to move forward or backward and they don’t care from where it’s starting, but having the object oscillate around a well defined origin will be more convenient.

If you look closely at the plots when two keys are pressed, you may have also noticed that there are some areas where one of the keys wants the **plate** to move forward, while the other wants it to move back. As a result, the **plate** stays at the same position.

What may be a little surprising is that there is nothing magical about the sound emitted by the **plate** when two keys are pressed – the emitted sound depends purely on the final movement of the plate. In the demonstration below, I made the **third key** emit the same pattern of motion as the combination of the **first** **two**:

The sound emitted when the **third key** is pressed is exactly the same as the sound emitted when the **first** **two** keys are pressed together. This is a very important observation, as it lets us explore some other patterns of motion that the keys could induce.

We’ll expand on this idea by changing the way in which the **plate** vibrates. Instead of having it rapidly jump back and forth, we can request the **plate** to move with constant velocity, following a triangular pattern. Let’s look at that pattern up close. Like other periodic shapes, a triangular waveform can be characterized by its **amplitude**, **frequency**, and **phase**:

In these visual examples the **amplitude** controls the height of the waveform. The **frequency** controls the width of the repeating pattern, which affects how many repetitions of the up-and-down triangles can fit inside a single second. The **phase** just shifts the shape around to change what we consider the beginning of that pattern.

We’ve already seen how the **amplitude** of the plate’s motion affects the perceived loudness, and this triangular motion is no different. In the demonstration below, all three keys move the plate in the same pattern, only their **amplitude** is different:

Similarly to the original rectangular pattern, we perceive different frequencies of this triangular motion as different pitches. In the following demonstration, each key has a different frequency as marked by the display in the upper part of the keyboard. The higher the frequency the higher the perceived pitch:

With these more complicated triangular shapes in place it may be a bit harder to understand how all these waveforms add up to form the **final plot**. Let’s visualize this process step-by-step using a few examples that you can select in the demonstration below. The slider tracks the process of the addition of four different triangular shapes of different **amplitude**:

Notice that each waveform expresses some shift from the current position. This offset can be **positive** or **negative** – I’m visualizing it using small arrows. To add the waveforms we just need to apply these arrows to the existing shape, which we can do by placing the arrows on that shape and then deforming the shape as the arrows demand.

The yellow bars in the bottom part symbolize the **amplitude** of the specific triangular waveforms at different frequencies. Notice that it’s completely fine for an amplitude to be zero – that element just won’t participate in the creation of the final cumulative waveform.

The last parameter of any periodic function is **phase**. On its own, **phase** doesn’t do much for sound, but it may give us some trouble when a phase-shifted shape is combined with another shape of the same frequency. In the demonstration below, all three keys of our synthesizer vibrate at the same frequency and amplitude, but each one is shifted in **phase** by a quarter of a cycle:

Individually, each of the three keys sounds exactly the same, but once we combine the keys the trouble begins. When the **middle key** is combined with the **left** or the **right** key, the resulting sound is slightly louder. However, when the **first** and **last** keys combine, their waveforms perfectly cancel each other out causing no plate movement – no sound gets emitted! We’re experiencing a destructive interference where the peaks of one waveform end up getting cancelled out by the valleys of the other waveform.

Triangular functions were a little more intricate than simple jumps, but nothing prevents us from using completely arbitrary motions. In the top part of the demonstration below, you can **draw your own shape** of the waveform that defines the vibration of the plate. I then repeat this pattern at frequency prescribed by each keyboard key, letting you hear the result through your speakers:

After playing with different shapes you may have noticed that while they have a different character, they all share a very similar pitch. Different shapes end up creating different [*timbre*](https://en.wikipedia.org/wiki/Timbre) which is a property of sound that lets us distinguish different instruments playing the same note.

So far we’ve been building waveforms of increasing complexity, but it’s time we reversed this direction by discussing sinusoidal waves, which form the most fundamental periodic functions.

## Pure Tones [![](https://ciechanow.ski/images/anchor.png)](https://ciechanow.ski/sound/#pure-tones)

Let’s try to create a sine wave from scratch. In the demonstration below, you can witness a rotating circle with a small red point marked on it. If we track the vertical position of that point over time we’ll draw a perfect sine wave. While we can assign **amplitude**, **frequency**, and **phase** to any other periodic function, those properties beautifully correspond to radius, speed, and initial angle of the rotating circle that generates this wave:

Let’s see how a sinusoidal vibration of a **plate** sounds. In the demonstration below, you can experience the whole range of sine waves with different **frequencies** and **amplitudes** playing through your speakers. Some of the tones are expected to be imperceptible, so do not adjust your sound volume until you reach the middle **frequencies**:

You’ve probably noticed that low **frequencies** start inaudible, and they increase in loudness as the **frequency** increases. However, as the **frequencies** go toward the higher end the sound fades away into silence. Although your speaker and audio settings may to some extent contribute to this effect, it’s otherwise perfectly natural – humans can hear in a limited range of frequencies and the perceived loudness changes with frequency.

In the physical world, these pure sine tones are easily generated by striking different [tuning forks](https://en.wikipedia.org/wiki/Tuning_fork), but on this website we have to resolve to making our synthesizer play some of these notes at different **frequencies**:

Once again, as each key requests the plate to vibrate in a certain way, its **final motion** is the sum of **those** **individual** **requests**. That addition can sometimes create unexpected effects when the frequencies of played sounds are close to each other, which you can experience in the demonstration below. Notice the frequencies of sounds as shown above the keyboard keys:

When you press two keys at the same time you’ll hear the volume of sound increasing and decreasing in a phenomena known as [*beats*](https://en.wikipedia.org/wiki/Beat_\(acoustics\)). It’s not your ears misleading you – it’s a real effect caused by the two original frequencies interfering with each other, which you can observe on the plots. This effect can happen with all periodic signals and you can sometimes hear it when sitting in an airplane – the rotation speeds of two engines may not be perfectly synchronized and you can hear the variation in slowly changing volume of the resulting sound.

Similarly to triangular waves, we can fairly easily visualize how the addition of multiple sine waves with different amplitudes, frequencies, and phases creates more complex shapes. In the demonstration below, I’m adding a bunch of these sine waves. You can use the control below to choose the shape we’re creating:

Observe that I’m also using a “special” sine wave with frequency of 0 Hz – it reflects a constant value that is useful to represent waveforms that are, on average, not centered around the baseline. Looking at the construction of these waveforms, we can easily reason that the final shape is actually created by 6 different sine waves with varied amplitude, frequency, and phase.

Instead of adding these sinusoidal basic blocks to create a complicated function, we can, perhaps surprisingly, invert this process and *decompose* any repeating waveform into its constituent sine waves. Let’s see this in action. In the demonstration below, you can **draw the desired shape** in the top part and I’ll do my best to express it using the sum of 64 sine waves with increasing frequencies. For clarity, I’ll skip the animations of green and red arrows being added:

In the bottom part I’m once again showing the amplitudes of individual sine waves creating the final shape, but since we have so many of them I just pack their **amplitude bars** next to each other to create a cumulative plot of amplitudes.

Depending on how the original signal is expressed, this method of decomposing periodic functions into sines is known as [*Fourier series*](https://en.wikipedia.org/wiki/Fourier_series) or [*Fourier transform*](https://en.wikipedia.org/wiki/Discrete_Fourier_transform). It lets us deconstruct a waveform expressed as a function of time into a waveform expressed as a collection of individual frequencies that are contained in that waveform.

We can look at any repeating pattern either as a collection of individual values over time *or* as a collection of distinct sine waves that add up to the same pattern. Both views represent the same shape, they just present it in a different way.

In general, Fourier transform provides us with both amplitude *and* phase of each contributing sine wave. As we’ve seen, however, human hearing is almost completely agnostic to phase shifts so amplitudes typically provide enough information to express the original shape.

Let me bring in the little waveform decomposer once more, this time I’ll fill it with a square function corresponding to the simple back-and-forth jumps that our plate did initially:

As you [drag the slider to the end](https://ciechanow.ski/sound/#) you’ll notice that the decomposition of a waveform that rapidly changes values ends up creating ripples in the recreated function in an effect known as [*Gibbs phenomenon*](https://en.wikipedia.org/wiki/Gibbs_phenomenon). Notice, however, that as we add more sines of high frequency the width of that ripply region decreases.

More importantly, this [square pattern](https://ciechanow.ski/sound/#) that repeats once per second contains in it many sine waves of much higher frequencies. This explains why we were able to hear a plate jumping back and forth once per second – while we can’t hear many of those lower frequencies, we *can* hear the higher pure tones. A [sine wave at 1Hz frequency](https://ciechanow.ski/sound/#) contains only a 1Hz signal that we *can’t* hear.

Let’s look at the frequency spectrum of the things in your environment. In the demonstration below, you can press the button to start using your microphone. I’ll then draw a plot of different frequencies and their amplitudes as heard by the device:

I recommend whistling, or singing *“aaaaaa”* using lower and higher voice to see different frequencies appear. If you have any musical instruments handy or applications that can recreate their sounds, the resulting diagrams will in many cases also show interesting distributions. More complicated sounds like speech or music will rapidly change their shapes, which reflects their dynamic nature.

When singing or playing your simple notes into the microphone you may have noticed repeating patterns of frequencies – a certain base frequency gets a strong sibling at double, triple, and higher multiples of that initial frequency. These repeating patterns can happen for different reasons, and in the next section I’ll explain how they’re created by some of the simplest vibrating objects.

## Masses and Springs [![](https://ciechanow.ski/images/anchor.png)](https://ciechanow.ski/sound/#masses-and-springs)

To explore these vibrations, we’ll start by building a simple system consisting of a **small mass** attached to **two springs**. In the demonstration you can move that **mass** up and down with the slider:

At the small amplitudes of motion we’re considering here, the system oscillates sinusoidally with a certain natural frequency that depends on the mass of the **object** and the stiffness of the **springs**. The friction between moving parts and the air resistance causes the oscillations to die out after a while.

You may have realized that in this simulation the **mass** moves only up and down, but in principle it could also move left and right. However, these horizontal motions won’t be of much use to us, so we’ll focus purely on the vertical movements.

Let’s add another **mass** and **spring** to this system:

Even with this simple setup the behavior of this system can get fairly complicated, including some [unusual patterns](https://ciechanow.ski/sound/#) of motion, but all of these scenarios can actually be decomposed into the sum of two much simpler motions.

In the **first motion** the **masses** move together in the same direction, and in the **other motion** the **masses** move in the *opposite* directions. In the demonstration below, you no longer control the position of the **masses**, but instead the *amplitudes* of those **two** **separate** modes:

With careful arrangements, you can recreate the [interesting motion patterns](https://ciechanow.ski/sound/#) that this system can create. Notice that the [second mode](https://ciechanow.ski/sound/#) oscillates faster than the [first mode](https://ciechanow.ski/sound/#). This should make intuitive sense since the central **spring** is getting stretched, which adds an additional pulling force to the system.

Any pattern of vertical motion of those two identical **masses** and three **springs** can be decomposed into the two [*normal modes*](https://en.wikipedia.org/wiki/Normal_mode) that we’ve just looked at. That idea extends to even more complicated systems. In the demonstration below, you can see the five normal modes of a system consisting of five **masses** and six **springs**:

As we increase the number of masses and springs to the extreme, each segment starts to resemble a tiny section of a string that has some mass and elasticity. Conceptually, a simple string can be thought of as a system of a huge amount of very small spring-and-mass segments – you can see one in the following demonstration. Let’s look at the first few normal modes of this system:

Since the endpoints of the string can’t move, only certain wave patterns are allowed – no wave shapes that would cause the ends to wiggle up and down can be expressed with this setup. The frequency of the [first mode](https://ciechanow.ski/sound/#) is known as the *fundamental frequency*. Each higher mode has a frequency that is an integer multiple of that base frequency – for instance the [third mode](https://ciechanow.ski/sound/#) has three times as high frequency as the fundamental mode. These modes of vibration are known as [*harmonics*](https://en.wikipedia.org/wiki/Harmonic).

Similarly to the more primitive systems, any motion of a string tied to two ends can be decomposed into the sum of these individual motions, each with a different amplitude. These concepts are very similar to the Fourier transform operations we did for the waveform shapes, but this time they’re applied to the shape of a string.

What this all ultimately means is that the motion of a vibrating string is the sum of the individual harmonic modes, each with an arbitrary amplitude, and a prescribed frequency that is a multiple of the fundamental frequency. Let’s see and hear how this manifests in practice. In the demonstration below, you can pluck the string by clicking and dragging over with your mouse with your finger:

You may have noticed that the place at which you pluck the string changes the sound it produces – it’s just a result of different ratios of harmonics created by the shape of the plucked string right before it’s released. The extent to which this string can be pulled is quite exaggerated here making it look a little unrealistic, but a [slow-motion capture](https://youtu.be/LNNQvG0jWtw?t=15) of a guitar string reveals a very similar behavior.

The fundamental frequency of vibration of a taut string depends on its length, weight, and tension. The shorter the length, the higher the frequency – this is the main reason why a [violin](https://en.wikipedia.org/wiki/Violin) has a much higher pitch than a [double bass](https://en.wikipedia.org/wiki/Double_bass).

More importantly, by placing a finger on a string and thus changing its effective length the performer can play different notes using a limited set of strings. In the demonstration below, you can hear the variation in pitch as you drag the slider to change the location of the **finger** holding down the string:

The thicker the string the more inertia it has and the more difficult it is to move it around, so the frequency of the sound *decreases* as string gets heavier, or, more specifically, as its [*linear density*](https://en.wikipedia.org/wiki/Linear_density) increases. By using strings of different **thickness** or material, musical instruments can achieve different pitches on strings of roughly the same length – you can play with that parameter in the demonstration below:

Finally, the more taut the string is, the faster it snaps back, so a higher tension increases the frequency – by turning the [tuning pegs](https://en.wikipedia.org/wiki/Machine_head) of a string instrument one can tweak the fundamental frequency of each string.

On its own, a vibrating string doesn’t move much air and it’s not very loud. To make the sound more pronounced, the motion of the string is transferred to a larger surface to increase the vibrating area and the volume of emitted sound. The shape and materials used in the construction of the body of an instrument strongly contribute to different sound frequencies getting amplified or absorbed.

Our models of springs and masses also extends to other vibrating elements. Membranes of drums form a two dimensional grid of vibrating elements. Tightly packed particles of solids form a three dimensional lattice – its elements also elastically interact with each other, which at larger scales causes cymbals, gongs, and bells to [vibrate](https://www.youtube.com/watch?v=zVPOL3Wwqbc) with various natural frequencies that depend on the shape and construction of these instruments.

Some other instruments like flutes or organs rely on a column of air rapidly traveling back and forth through their pipe-like bodies, forming yet another way to make the surrounding air vibrate at certain rates. When we sing, the vocal cords rhythmically open and close creating puffs of air – those also induce pressure disturbances that then travel around.

In the final part of this article we’ll explore how pressure waves get emitted from vibrating objects and how they end up spreading through the air and interacting with the environment.

## Pressure Waves [![](https://ciechanow.ski/images/anchor.png)](https://ciechanow.ski/sound/#pressure-waves)

Let me bring back the vibrating **plate** inside a tube that we could control directly with a slider:

While easy to understand, the contained nature of this model makes it a little simplified as it doesn’t match the behavior of typical instruments or speakers. To explore more realistic scenarios of sound propagation, we’ll firstly use a simple sphere that can inflate and deflate to quickly change its radius – you can think of it as some sort of water-filled balloon that we can rapidly add or remove liquid from. In the demonstration below, you can see an example of that simple pulsing sphere with the slider controlling the **frequency** of its vibration:

Let’s look at the pressure near the surface of this sphere up close using our method of slicing the air, this time into thin shells surrounding this sphere. At high frequencies of vibration the behavior of the air surrounding the sphere resembles that of the simple plate in a tube, which you can experience in the following demonstration. The slider controls the speed of the *animation*, which lets you see the details, but the sphere vibrates with the same frequency at all times:

As this sphere finishes expanding, it creates an area of **higher pressure** near its vicinity. However, at lower frequencies of vibration the situation is a bit different – you can see this in the simulation below. Notice that when the sphere finishes expanding, the **pressure decreases**. I’m exaggerating the size changes of the sphere to make it more visible:

What we’re witnessing here is the air’s inertia. The initial expansion of the sphere accelerates the neighboring air. When the sphere stops growing, that air still has some velocity so it moves away from the sphere, creating the local **rarefaction**. Similarly, as the sphere shrinks, the surrounding air starts getting pulled towards it, and it continues to **bunch up** even after the sphere reaches its smallest size.

The analytical derivation of this behavior is [quite complicated](https://asa.scitation.org/doi/pdf/10.1121/2.0001259), but ultimately the range of frequencies at which these different behaviors occur depends on the size and shape of the vibrating object.

A sphere forms only a simple model of a speaker – a more realistic example would be a vibrating **circular plate** surrounded by a very large wall:

Similarly to a sphere, the relation between the position of this **plate** and the resulting pressure also depends on **frequency** of the sound. However, the sphere was perfectly symmetrical, which made the intensity of the created pressure waves independent of direction. For a vibrating **plate** some of those symmetries are lost and the intensity of the pressure disturbances also depends on the direction relative to the source:

As you [increase](https://ciechanow.ski/sound/#) the **frequency** of the sound, the pressure waves focus in the frontal direction. Most commonly, lower frequencies emitted by speakers are much more widespread than higher frequencies. Notice that I also stopped drawing the thin lines separating individual sections of air – we’ll now more realistically assume that the pressure changes without any discrete steps.

As we’ve seen, the distribution of pressure that a vibrating object creates can get a little complicated, but, thankfully, at a reasonable distance, a steady sinusoidal oscillation of the source creates a steady sinusoidal variation in pressure that travels through the air. You can see the “side” view of that pressure variation on the dotted line in the bottom part of the following simulation:

This means that everything related to sinusoidal vibrations of plates like the amplitude, frequency, and phase of their motion also applies to the pressure waves themselves.

While these colorful diagrams made it easier to see how pressure changes propagate through air, they’re unfortunately a little deceptive. Firstly, I’m slowing things down significantly to make the propagation of waves visible, otherwise they’d just zip across your screen in a fraction of a second. At room temperatures the speed of sound in air is around 1125 ft/s or 767 mph 343 m/s or 1,235 km/h.

Moreover, the actual pressure variation in sound waves is very small. When a person speaks at a normal volume the pressure increases and decreases by around 0.02 pascals or around 0.00002% of the atmospheric pressure. The **red** and **blue** colors make it look like pressure is changing a lot, but in practice these variations are minuscule – it’s quite astonishing that our ears pick up differences that small.

I also need to note that the pressure simulations we’ve just seen were fairly simplified. The behavior of a typical speaker is more complicated and the way it impacts the surrounding air depends on the speaker’s size, shape, and overall construction. All of these intricate behaviors may obscure some other phenomena that we’ll discuss, so for the following examples I’ll simplify things a little by using a basic, omnidirectional source of sound, similar to the expanding sphere we’ve used.

In the demonstration below, the **blue speaker icon** symbolizes that source and the **yellow ear icon** shows the location at which you can hear things – you can drag the latter around to change where **you** are:

You may have noticed a few interesting phenomena. Firstly, as **you** move away from the **source** it may take some time for the sound to get to you. This effect is just a consequence of the finite speed of sound. While at small distances we don’t notice that delay, it’s easily observed when a lightning bolt strikes and the sound gets to us with a perceptible lag. In this demonstration the distances are also quite large which you can see on the scale in the bottom left corner.

Secondly, the loudness of the sound we hear depends on the distance from the source. With a simple source like this the pressure waves travel away radially. While the waves look like circles in this two dimensional demonstration, in the physical world they form expanding spheres. The amplitude of pressure **p** of these waves is *inversely* proportional to the distance **r** from the source:

p ~ 1 / r

This effectively means that when we double the distance from the source the amplitude of the sound pressure wave gets halved. As we’ve seen a few times with vibrating plates, a change in amplitude of their motion, and thus the change of amplitude of the generated pressure waves, affects how loud a sound sounds.

The difference in magnitude of those pressure variations between loud and quiet sounds can get very large – the amplitude of sound pressure waves from a roaring jet engine is around 10000 times larger than that of a faint whisper. Comparing values using numbers that large would be inconvenient and vague terms like “faint whisper” aren’t particularly well-defined.

To solve these problems the concept of [*sound pressure level*](https://en.wikipedia.org/wiki/Sound_pressure#Sound_pressure_level) or **SPL** gets introduced:

SPL = 20×log10 p / p0   dB

Let’s unpack this equation. For reference pressure **p0** the commonly used value is 20 µPa, or 20 micropascals, since it is considered to be the threshold of human hearing, while **p** is the average *measured* pressure for which we’re calculating the **SPL**.

The **logarithm** compacts large numbers into much more manageable values. Speaking very roughly, this logarithm is related to the number of digits in the decimal representation of a number – making a number 10 times as large, or, equivalently, adding one more 0 digit to it, increases the value of a **base-10 logarithm** by 1.

What we’re calculating here is the **logarithm** of the *ratio* of the measured pressure **p** to the reference pressure **p0**. All of that is scaled by number **20**, which gives us the result in units of [*decibels*](https://en.wikipedia.org/wiki/Decibel), often abbreviated as **dB**.

Let’s see how this equation can be used in practice. In the demonstration below, you can change the value of **pressure** and see how it affects the computed value of **sound pressure level**:

SPL = 20×log10 2000 µPa / 20 µPa = 40  dB

Recall that doubling distance halves the pressure, which effectively means that every doubling of distances reduces **sound pressure level** by 6 decibels. On the flip side, using two speakers instead of one also increases the **SPL** by just 6 decibels. Note that these equations can also be used to compare the **SPL** of any two sources – for example, if two sounds differ by 20 dB then the amplitude of one is 10 times larger than that of the other.

The perceived loudness of sound very strongly depends on the **sound pressure level**, however, that loudness also [depends on the frequency](https://en.wikipedia.org/wiki/Loudness#/media/File:Lindos1.svg) of the sound – you’ve experienced this when tinkering with the plate moving in a sinusoidal motion. To account for this, the value of pressure amplitude at different frequencies is often [weighted](https://en.wikipedia.org/wiki/A-weighting) so that the decibel values match human perception more closely.

It’s also worth mentioning another quantity related to the “amount” of sound – the sound *intensity* **I** which measures the *power* of the sound per area. The power of any wave depends on the *square* of its amplitude, so for simple spherical waves the sound intensity is proportional to the inverse *square* of the distance in, already [mentioned on this blog](https://ciechanow.ski/lights-and-shadows/#lns_inv_square), inverse square law:

I ~ 1 / r2

Similarly to pressure, one can also define a [sound *intensity* level](https://en.wikipedia.org/wiki/Sound_intensity#Sound_intensity_level), which expresses the ratio of intensity to a reference intensity.

Another aspect affecting the propagation of sound is [acoustic attenuation](https://en.wikipedia.org/wiki/Acoustic_attenuation) caused by e.g. viscosity of air and thermal conduction between its molecules. These interactions cause additional decrease in amplitude of the pressure waves as they move away from the source. Attenuation depends on temperature and humidity of air and it also varies with the frequency of the sound. These effects are fairly complicated so I won’t account for them here.

The last demonstration had one final hidden feature that you may have experienced when simultaneously pressing the keyboard keys *and* changing your **hearing position** relative to the **speaker**. This effect is more commonly experienced when it’s the **source** of the sound that’s moving relative to the **receiver**, like in the demonstration below, where you can control the **velocity** of the **speaker** using the slider:

Notice that when the **speaker** moves toward the **receiver** the pitch increases, and when it moves away from the **receiver** the pitch decreases in the phenomenon known as [Doppler effect](https://en.wikipedia.org/wiki/Doppler_effect) – you’ve probably experienced this in real life when an emergency vehicle with blazing sirens or a motorcycle with a loud engine was passing by.

This effect is easiest to understand by looking at the individual **peaks** of the sound waves. In the demonstration below, you can control both the **flow of time** and the **velocity** of the **speaker**. The plot at the bottom tracks when the **peaks** of the sound wave are **emitted**, and when they’re **heard** at the center of their respective icons:

The **speaker** emits the **peaks** at a certain fixed cadence, which depends on the sound frequency. When a [first peak is emitted](https://ciechanow.ski/sound/#), it will take some time for it to get to the **receiver**. However, as the **speaker** moves towards the **listener**, the [next peak](https://ciechanow.ski/sound/#) will be emitted from a closer distance than the previous one, and it will take it *less* time to get to the **receiver** – the **receiver** will witness the peaks coming in at faster rate, or higher frequency, which we perceive as higher pitch.

When the **speaker** moves away from the **receiver**, the situation is reversed – each new peak has to travel a bit longer distance to get to the **listener**, making the peaks more spread out for the **receiver** causing the frequency and the pitch to drop.

You can see both of those effects in the plot in the bottom part of the demonstration – the **yellow bars** get bunched up or loosened up relative to the **blue bars**, which implies increased or decreased frequency of received sound.

The **speaker** is completely unaware of what’s going on – it just keeps emitting the sounds at the same frequency. However, as the **speed** of the **sound emitter** increases, the circles corresponding to the **peaks** of the waves get bunched up more and more creating more extreme pressure conditions. When the sound-emitting object surpasses the speed of sound it will create a [sonic boom](https://en.wikipedia.org/wiki/Sonic_boom).

Although relative motion created interesting effects, even static conditions may end up forming unexpected phenomena. In the demonstration below, the pure notes played on the keyboard are emitted by **two speakers** at the same time. You can drag **your position** around to see how the interaction of pressure waves affects what you hear:

Depending on the location of the **receiver** the sound level increases and decreases, which we can also see in the diminished intensity of the pressure variations in those areas. This effect is caused by destructive interference of pressure waves from the two speakers – **peaks** from one speaker cancel out **valleys** from the other. As long as you have access to a two speaker system you can actually try to [replicate that experiment](https://www.youtube.com/watch?v=b87QZtYKmqo).

Fortunately, different frequencies played by different keys end up having dead spots in different places, too. Since the vast majority of the typical sounds we hear consists of a multitude of different frequencies that constantly change, we don’t experience this loudness variation in more practical scenarios.

It’s important to note that the interference doesn’t permanently destroy the waves as the disturbances merely pass through each other. This should make intuitive sense – when a few people are simultaneously speaking in the same room their speech doesn’t become garbled or faded and we can still understand what they say.

Quite conveniently, all of our experiments so far happened in a vast open space unencumbered by any obstacles. Let’s see what happens if we put a wall in our testing environment:

When you emit a short pulse of sound while [standing far enough](https://ciechanow.ski/sound/#) from the wall, you’ll hear the originally emitted sound with some delay in a familiar effect known as *echo*. Notice the distances involved here as shown by the scale in the bottom left corner. When standing [close to the wall](https://ciechanow.ski/sound/#), the delay between the original and reflected sound gets very short – when it’s smaller than around 50 ms, our auditory system doesn’t discern these two separate sounds.

You may have noticed that the reflected wave is quieter. While the increased travel distance accounts for some of that reduction, it’s also the wall itself that partially absorbs the energy of the pressure disturbances.

For the last demonstration in this article we’ll move to a completely enclosed space allowing the sound waves to bounce back and forth from the walls. You can change the size of this room using the slider and witness the reflection of sound off the walls:

Even after you stop pressing the keys it may take a while for the sound to die out, because the waves keep reflecting off the walls, losing only some portion of their energy with each bounce. You may have heard this effect known as [*reverb*](https://en.wikipedia.org/wiki/Reverberation) in an empty room devoid of any furniture, drapes, or clothes that absorb the pressure waves and heavily attenuate the reflection of sound.

## Further Reading [![](https://ciechanow.ski/images/anchor.png)](https://ciechanow.ski/sound/#further-reading)

[The Science of Sound](https://www.pearson.com/en-us/subject-catalog/p/science-of-sound-the/P200000006977/9780805385656) is an entry level, yet very thorough book dedicated to many topics related to sound. The three authors use a minimal amount of math and focus more on higher level descriptions of different phenomena, including perception of sound, musical scales, and audio hardware. The only drawback is the price, but thankfully one can easily find cheaper used copies online.

For further exploration of Fourier Transform and signal processing I highly recommend [The Scientist and Engineer’s Guide to Digital Signal Processing](https://www.dspguide.com) by Steven W. Smith. I personally really like the author’s style of patiently explaining things step by step with multiple examples. On top of that, the entire book is available online for free on the author’s website.

## Final Words [![](https://ciechanow.ski/images/anchor.png)](https://ciechanow.ski/sound/#final-words)

Sound waves aren’t limited to gases – they spread through liquids and solids too. However, in daily life almost everything we hear are just minuscule pressure variations of air, which ultimately are just changes in the number and intensity of collisions of individual air particles.

I find it fascinating that the most irritating noises and the most inspiring music are driven by the same phenomena – it’s only the underlying shapes and magnitudes of their pressure waves that make them sound so distinctively different.
