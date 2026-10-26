---
title: Overgrown, when plants invade your abandoned tabs
link: https://charliegerard.dev/blog/overgrown-extension/
source: hnrss-org
published: 2026-10-22T15:52:21Z
updated: 2026-10-22T15:52:21Z
first_seen: 2026-10-26T01:35:28.974992561Z
authors:
- speckx
content: extracted
html: 2026-10-22-overgrown-when-plants-invade-your-abandoned-tabs.html
remote_preview:
  url: https://res.cloudinary.com/devdevcharlie/image/upload/v1792518914/overgrown-thumbnail_q5tzrs.png
---

I was doing a little creative exercise the other day around the word "Overgrowth". The first thing that came to mind was an apocalyptic scene, something like The Walking Dead or The Last of Us, where plants tend to overgrow abandoned buildings.

I then wanted to apply this idea to the digital world, and the word "abandoned" made me think of my browser tabs... What if my tabs were slowly taken over by plants the longer I left them unread? What if I had to clear away these invasive plants to access my tabs again? A bit like trying to open a door that's been overgrown...

So I built this fun little Chrome extension that does just that. It won't necessarily make me read my tabs more but it's ok.

## Demo

![Overgrown demo](https://charliegerard.dev/overgrown-extension-demo.gif)

[Link to the repository](https://github.com/charliegerard/Overgrown)

## The idea

- Every tab has a neglect clock. It runs while the tab is out of view and stops while you're looking at it.
- The longer a tab is neglected, the more plants grow over it, from the edges towards the middle.
- Once a tab is more than 5% overgrown, the page behind the vines is walled off: you can scroll, but you can't click, select or drag anything.
- To free it, you have to slash through the vines by dragging your mouse across them. Get it down to 5% and the last bits fall off. If you abandon your tab once more, the jungle starts growing again.
- Just for fun, you can pick your weapon of choice between the rusty machete, the chainsaw and the flamethrower.
- You can also change how long it takes for your tabs to be fully overgrown.

Originally, I was thinking about blocking users from closing their overgrown tabs until they slash all plants, but I thought it would be too annoying... 😅

## The extension

The code is organised in 3 main parts: the background logic, the content script to display the plants and animation on the tabs and the popup folder that handles the UI of the extension itself.

### Background worker

The background.js file starts the jungle on each open tab and forgets it when it closes.

### The popup

This folder handles the UI for the extension with some small components for the time setting, the weapon choice, the list of tabs and their current stat.

### The overlay

The `content` folder contains the SVGs for the plants and the weapons, the logic to position and grow the clusters, the `neglect clock` and the sounds.

* * *

I've been wanting to get back into building things for a while now and this project felt like a small, fun way to start. I'm writing this on a new computer so I currently "only" have 22 tabs open but 14 of them are already completely overgrown... 😂 It might not make me read them more but slashing through the vines makes coming back to them more fun! 🪚🌿
