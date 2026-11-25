---
title: Bypassing iOS Safari's 60 FPS Limit to Build Web Applications That Feel Native
link: https://rida.dev/blog/bypassing-ios-safari-60-fps-limit
source: hnrss-org
published: 2026-11-25T03:04:37Z
updated: 2026-11-25T03:04:37Z
first_seen: 2026-11-25T10:22:41.845233465Z
authors:
- vinhnx
content: extracted
html: 2026-11-25-bypassing-ios-safari-s-60-fps-limit-to-build-web.html
remote_preview:
  url: https://rida.dev/og/blog/bypassing-ios-safari-60-fps-limit.png
  alt: Bypassing iOS Safari’s 60 FPS Limit to Build Web Applications That Feel Native
---

Rida F'kih · November 20, 2026 · 10 min read

[Devon Govett](https://x.com/devongovett?utm_source=rida.dev&utm_medium=referral&utm_campaign=bypassing-ios-safari-60-fps-limit) of React Aria posted [“Scrolling is All You Need: Building a Swipeable Sheet”](https://react-aria.adobe.com/blog/sheet?utm_source=rida.dev&utm_medium=referral&utm_campaign=bypassing-ios-safari-60-fps-limit) back in October, and in it he talked about his team developing a sheet component that cleverly leverages the physics and smooth interactions provided by native browsers to provide the illusion of a native interaction.

More recently, [Jordan Walke](https://x.com/jordwalke?utm_source=rida.dev&utm_medium=referral&utm_campaign=bypassing-ios-safari-60-fps-limit), creator of React, posted [a video of an experiment](https://x.com/jordwalke/status/2122804602143924397?utm_source=rida.dev&utm_medium=referral&utm_campaign=bypassing-ios-safari-60-fps-limit) on X showing a convincing native-like mobile interaction using DOM & CSS. It re-established a long-lived itch that I’ve had, and prompted [this response](https://x.com/ridafkih/status/2122834693611819451?utm_source=rida.dev&utm_medium=referral&utm_campaign=bypassing-ios-safari-60-fps-limit) from me. It’s something I could never justify building, however. Anything I built using this method would be pegged to 60 FPS.

## Why the iOS Safari Experience Sucks

What the video Jordan Walke posted doesn’t show is that, unless you flip the “Prefer Page Rendering Updates near 60fps” flag buried deep within the Safari developer settings on iOS, WebKit limits requestAnimationFrame to just 60 Hz on a 120 Hz screen, and on a regular page, touch and pointer events are held to that same rate. This is what makes even the best mobile websites feel irritating to use.

How can I make this experience suck less without resorting to flags? Let’s call back to the React Aria piece. The reason it feels so nice is that scrolling is the responsibility of the compositor. It runs in its own thread. It moves, clips and fades without a JavaScript round-trip.

Finger to digitizer, digitizer to compositor, compositor to GPU. This will become the basis of our interaction.

## High Refresh Rate Interactions

Behold, two grids. They both move a square when you drag. The one on the left uses JavaScript events and sets the `transform` attribute in CSS, while the one on the right simply places a square in a scrollable area.

Try dragging this

Try dragging this

If you’re on iOS Safari, you’ll immediately be able to tell which is which because one refreshes at most 60 times a second in a renderer pass, while the other uses the compositor.

In order to leverage the power of the scroll area, we need to find a way to read the scroll position outside of the renderer. If we can do that, we can treat it like a trackpad, or use it to “extract” native bounce, physics, momentum, etc., and apply them directly from the compositor to our animations.

- Want a 1:1 trackpad experience? We just measure only when the finger is pressed.
- Want a true trackball with native momentum? We just make it an infinitely scrollable region.
- Want native bounce? We configure over-scroll padding and set the bounds of the container.
- Want a dial with notches? Snap points!

The point is, it’d be crazy flexible if we could just find a way to pull the values out of it frequently enough.

## Creating “The Controller”

I measured scroll events updating 5 to 26 times per second during momentum scroll on an iPhone Duo. The best-case scenario is during slow, deliberate scrolls where it reaches almost 60 updates a second. Useless.

Instead, we’ll be aiming to read `scrollTop` and `scrollLeft` synchronously.

First, I tried having a `MessageChannel` call itself recursively, which worked: but it produced 50,000 reads a second with just 80 distinct values. That’s a lot of waste and heat since it pins a CPU core.

Next failure was trying to use `Atomics` to wake at the screen’s refresh rate, which worked and similarly extracted 80 distinct positions every second, but the drawbacks were too great. Since it needs a SharedArrayBuffer, we’re forced to use COOP/COEP headers which break cross-origin assets on our website.

What finally worked was using a `MessageChannel` like before, throttling the next measurement to the next tick using a `setTimeout` according to the refresh rate of the device, phase-locked to vsync using `requestAnimationFrame` timestamps. I was nervous about this approach, but as you’ll see later, it panned out well. The only remaining drawback specific to this solution was easy to work around. iOS rounds `performance.now()` to the millisecond, so I derived the minimum time between draws from the refresh interval.

Each solution got us 80 position reads per second. Two-thirds towards our goal of 120 Hz input.

Safari’s UI process sends the web process one scroll position at a time, and waits for an acknowledgement from the web process before sending another. Anything that arrives in the meantime is overwritten. This was costing us dropped positions. Digging through WebKit yielded what we needed to get around this. It turns out that on every tick where the page has a rendering update queued, Safari flushes the position without waiting. An empty `requestAnimationFrame` loop is all we need to do that.

This bought us ~12.5% more reads, and brought us to 90 unique position reads per second.

I sort of assumed I had hit the limit of the aforementioned IPC, but it turns out that ProMotion was just deciding that there’s no need to render at a refresh rate higher than 90 Hz.

Squeezing out the last 30 Hz was trickier. A GPU-accelerated CSS animation lets the screen go to 120 Hz, but only while something is scrolling. So a tiny invisible scroller smooth-scrolls in the background, ready to boost us to the full ProMotion refresh rate whenever we need it.

Of course, we don’t want the display pegging to 120 Hz the entire time, so we reserve all the tricks we used to get there for times we expect animations to occur: touch events, scroll-derived animations, etc.

If the interaction you’re building doesn’t need momentum, rubber banding, or snapping, you can forego them entirely and just use a passive `touchmove` listener on the controller which fires at the full 120 Hz.

Just like that, we’re receiving updates from the controller at a full 120 Hz! Drag the grid below to see.

Position

0, 0

Updates

0/s

Unique values

0/s

Speed

0 pt/s

Try dragging this

One issue remains. `scrollTop` and `scrollLeft` are rounded to the nearest integer even though iOS scrolls with sub-point precision. Easy fix, thankfully. We just scale the scrollable surface up by the device pixel ratio, then its container back down by its reciprocal.

If you want to use the viewport itself as a controller, you *can* choose to invasively set the `initial-scale` in the same way, but this comes with some caveats. You’ll have to scale units by the DPR, disable text autosizing, and manually scale safe areas. You’ll also get tiny text-selection indicators. All trade-offs, but by no means required.

Position

0.00, 0.00

Updates

0/s

Unique values

0/s

Speed

0 pt/s

Try dragging this

From here on out, I’ll refer to these scrolling grids as “controllers”, because they act exactly like one. All we have to do is make one invisible and lay it over where we want to read high-frequency input.

Note that the page itself can also be a controller, but Safari will throttle reported scroll positions to 10 Hz during the URL bar resize that happens when you change scroll direction. WebKit skips this if we give the page a scroll-driven animation that animates a non-GPU-accelerated attribute. I went with transitioning between two transparent colours for this page.

## Breaking the 60 FPS Render Barrier

Now we get high-frequency input, but the DOM still only updates 60 times a second, so if we tried to leverage it as it stands, we’d just be updating a value twice for every frame that’s drawn. The user would get no benefit.

The vast majority of dragging interactions are already covered by our controllers, and predefined animations are covered by CSS animations. But what about more complicated interactions? What about computation-derived layouts? What about shaders?

Ask your search engine or AI of choice today “How to render canvas content on iOS at 120 Hz without feature flag?” and it will respond that this is a fundamental impossibility in iOS Safari. Even WebKit’s own [bug tracker](https://www2.webkit.org/show_bug.cgi?id=294338&utm_source=rida.dev&utm_medium=referral&utm_campaign=bypassing-ios-safari-60-fps-limit) is full of people asking for it. Ink & Switch, game-engine makers. People even cite Capacitor & Tauri as resorting to private APIs.

Asking Google yields the following preamble.

> There is no public web API or standard JavaScript workaround to force HTML5 \<canvas> (requestAnimationFrame) to run at 120 Hz on iOS Safari without changing the system feature flag.

Fable 5.2 shares the same sentiment.

> Short answer: you can't, for JavaScript-drawn canvas frames. On iOS, WebKit caps page rendering updates (which is when canvas content is presented) at 60 Hz unless the user turns off the flag. No web API overrides it. \[…\] Timer-based loops (setTimeout, MessageChannel) won't help, in my understanding: you can draw more often, but the result is still presented at the 60 Hz rendering update. Low Power Mode throttles further.

Well, as of today… you can!

At first, I tunnel-visioned on scroll. I tried breaking workers, pre-rendering permutation “flip-books,” and even live-generating video. It was all too laggy, jagged, or required so much RAM the WebKit process just crashed.

Seven attempts later, and a whole lot of agents digging through WebKit’s source, I landed on this.

In WebKit on iOS, if we create a `canvas` element and call `transferControlToOffscreen`, we get an `OffscreenCanvas` back. The original canvas element becomes a placeholder, and WebKit (following the HTML specification’s recommendation) skips waiting for the next rendering pass when an `OffscreenCanvas` pushes a frame.

Unlike earlier attempts, without workarounds this is easy on the battery and only uses as much memory as is needed to support a higher-frequency backing store.

Jackpot. We now have 120 FPS rendering on iOS WebKit canvases.

OffscreenCanvas

Canvas

Controller

Try dragging this

## Putting it All Together

Now we can take any input at 120 Hz, and render an output at that very same frame rate. The first thing I created for this was a small UI clone of [Ando](https://ando.so/?utm_source=rida.dev&utm_medium=referral&utm_campaign=bypassing-ios-safari-60-fps-limit)’s mobile application fitted with a Liquid Glass tab bar, buttons, and buttery smooth navigation interactions. Once installed as a PWA, I kept having to double-take to check I wasn’t in the native application.

## Applying Our Learnings

This page is fitted with two custom “Liquid Glass” elements in the viewport. Each of them renders at the full device refresh rate. We have to be very deliberate in order to do this in a way that is performant and convincing.

When the page first loads, we take a snapshot and load it into a WebGL texture. This texture is used as the foundation for a composite that is then fed into our shader.

WebGL Composite Texture + Glass

Drag me!

As the page scrolls, we then sample the region beneath each component, which is all we need to produce our effect. Building it this way means that no matter *how* the content below it is built, it can decide the best way to keep the texture live.

On this page, components using `requestAnimationFrame` for illustrative reasons update the texture from the main thread. Components using `OffscreenCanvas` post their frames directly from their workers to the worker managing the texture. This keeps us off the main thread and lets us get shaders refreshing at 120 Hz.

After all that, the glass just samples a small region around where it would land on the composite texture and computes the frame.

## Building a Tab Bar Component

### Building the Structure

Let’s build our Liquid Glass tab bar. We’ll start out by just drawing the underlying structure.

Structure

### Layout of Controllers

We’ll break this down into one controller for each button. Each controller sends us timestamped events: which controller is being used, and how much it has been dragged. We keep a short history of those samples, so we know when a press has moved far enough to count as a drag, and how fast the finger is moving. Nothing else. Everything else is derived from that.

Controllers

Tap and drag me!

### Building the State Machine

We now build a state machine around this. Every frame, the new events go in, and a frame comes out: where the lens is, how far it has lifted and stretched, and how visible the pill is.

- If a controller is being touched we then magnify & lift the lens.
- We know where to move the lens based on the controller’s offset since the touch began.
- If the user pulls past the bounds of the tab bar, we stretch the bar towards their finger.
- Depending on the offset, when the user stops touching the controller, we settle the lens on the nearest tab.

State machine

Tap and drag me!

### Adding razzle-dazzle

Finally, we apply some shaders to get a convincing Liquid Glass effect.

Shaders

Tap and drag me!

## Performance

I built a benchmark environment and recorded an iPhone 18 Pro at 240 FPS, slowing it down to an eighth to benchmark the difference. The two oscillating squares measure inputs and renders.

Controller Pointer Dropped

Try dragging the chart

Over three seconds of dragging, the controller version averaged 118 FPS while the pointer events version held a steady 60. Even in Low Power Mode, which traditionally limits you to 30, this approach gets you 60.

## Wrapping Up

Liquid Glass was the target as an overblown example, but this unlocks a lot. For the first time, you can take input and render output at true device-native speeds on iOS Safari. Micro-interactions feel native and delightful, PWAs which ignored the aforementioned feature flag finally have a way to render at 120 Hz, and look at all the fun we had along the way!

## Disclaimers

Would I rebuild my entire application around this? No. Fact remains it’s completely within Apple’s rights and would not violate the HTML specification if they just decided to make `OffscreenCanvas`’ placeholder wait for the main thread rendering pass. They even have a strong track record of “party pooping” as was done shortly after [Lochie Axon](https://x.com/lochieaxon?utm_source=rida.dev&utm_medium=referral&utm_campaign=bypassing-ios-safari-60-fps-limit) unveiled the `web-haptics` library. I’d much prefer they make these findings moot by disabling the feature flag and natively supporting ProMotion for websites, but Apple is Apple for a reason after all.
