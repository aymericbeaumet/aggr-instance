---
title: Declarative Dragging
link: https://declarative-dragging.github.io/
source: lobste-rs
published: 2026-11-05T00:35:09Z
updated: 2026-11-05T00:35:09Z
first_seen: 2026-11-05T17:57:26.867095589Z
authors:
- declarative-dragging.github.io via edwintorok
labels:
- design
- graphics
- plt
summary: Comments
content: extracted
html: 2026-11-05-declarative-dragging.html
remote_preview:
  url: https://declarative-dragging.github.io/link-preview.png
  alt: 'How declarative dragging works: a render function maps model state to a view; when a drag starts, possible target states are rendered, and where the dragged element lands in each determines the drop zones.'
---

★ [Best Paper Award](https://uist.acm.org/2026/awards/#best-paper), UIST 2026

What if making a diagram draggable was as simple as specifying what it can become?

Drag-and-drop makes the digital graspable. But designing bespoke drag interactions involves a lot of fiddly code to handle geometry, feedback, and animations. As a result, many people, from educators to game designers, mathematicians to creative tool builders, often don't bother.

**Declarative dragging** inverts the problem. Rather than programming all that visual behavior directly, you simply say, for each element you want to make draggable, which future states of your application dragging it should lead to. Our system speculatively renders those states to work out where things can go, then combines these possible worlds into a smooth, responsive drag-and-drop interaction.

Our goal with this project is to let people keep working within the semantic worlds they inhabit, while helping them turn the rules of their worlds into rich tactile interactions in ours.

[![Title card: Declarative Dragging, with illustrations labelled Model-Driven and Composable.](https://declarative-dragging.github.io/video-poster.webp)](https://www.youtube.com/watch?v=l7v-UwpsEbk)

A five-minute video introducing declarative dragging.

## How it works

D2 is *model-driven*: An interface author makes an element draggable by specifying which future model states should be accessible by dragging it. An engine speculatively renders these states to view trees, extracts the position of the dragged element in each of these renderings, and uses these to synthesize a drag-and-drop interaction, all without detailed work required by the interface author.

![Diagram: a model state is rendered to a view; when a drag starts, possible target states are rendered speculatively, and where the dragged element lands in each becomes a drop zone for an animated drag.](https://declarative-dragging.github.io/how-it-works-four.webp)\
 Dragology is a library implementing “declarative dragging”: a new way for interface developers to make higher-quality drag-and-drop interactions with less work. Dragology enables model-driven dragging.

To provide greater expressive range and customizability, D2 is *composable*: simple model-driven drag-and-drop behaviors can be combined to create more complex ones, and modifiers can layer features like floating, snapping, and transitions. For more details, see [the paper](https://andrewblinn.com/papers/D2.pdf).

## Dragology demos

We implemented D2 in a library called [Dragology](https://github.com/declarative-dragging/dragology), built for SVG interfaces on the web.

[algebra](https://joshuahhh.com/dragology/#/demos/animate-algebra-tree) [twisted trees](https://joshuahhh.com/dragology/#/demos/twisted-trees) [bézier curve](https://joshuahhh.com/dragology/#/demos/bezier) [braid](https://joshuahhh.com/dragology/#/demos/braid-old) [card piles](https://joshuahhh.com/dragology/#/demos/card-piles) [face](https://joshuahhh.com/dragology/#/demos/face-creator) [trominoes](https://joshuahhh.com/dragology/#/demos/tromino) [sprouting tree](https://joshuahhh.com/dragology/#/demos/sprouting-tree)

See our [demo gallery](https://joshuahhh.com/dragology/#/demos) for many more interactives built with Dragology.

## Where it came from

> Back in 2022, I had a prototype of a game aimed at making math more tangible, a long-running obsession of mine. It had a lot going for it, but the fundamental mechanic, based on selecting transformations from a menu, lacked the juicy tactility I craved. I eventually came up with a primitive version of what would become model-driven dragging, but implementing it seemed daunting, so it sat on my whiteboard for the next two years, haunting me every time I went to make a sandwich. — Andrew

![A whiteboard titled 'nool' covered in small algebraic expression trees, with arrows showing how each rewrite rule would be dragged, a section labelled CONFLICT, and a row labelled DISAMBIGUATION STAGE listing choice tree, overlap-click, default-drag and split-drag.](https://declarative-dragging.github.io/whiteboard-2023-06.webp)\
Andrew's whiteboard in June 2023, sketching a basic version of model-driven dragging for tree rewriting.

> On a really nice walk in Seattle back in 2023, Andrew shared his draggy ideas with me. I kept thinking about them over the years, but didn't do anything about it. And then last year, it all came together. My buddy Elliot was playing with order-preserving maps between trees. I came up with a fun way to draw these structures, but I realized I didn't just want to draw them – I wanted to reach in and pull them around, and feel out their dynamics (what became the twisted trees demo above). That felt pretty tough, until I remembered Andrew's ideas. I got working on a library that, with Andrew's help, grew into declarative dragging. — Josh

![A map of a walk through Capitol Hill in Seattle, its route in orange, with Josh's drawings of order-preserving maps between small trees laid over it: a hand-drawn lattice of trees with strands running between them, a computer-drawn version of the same lattice, and single trees cut from them.](https://declarative-dragging.github.io/walk/walk-figure.webp)\
Our walk in Seattle in 2023, and Josh's later drawings of order-preserving maps between trees.

## Since the paper

Josh has been working on a variety of new, more intricate mathematical explorables. We've found Dragology particularly suited to interfaces that combine parallel representations, where you see the same system from different perspectives. In the permutation lab below, interacting with any one representation fluidly updates the others, even halfway through a drag, showing their relationships in real time.

[](https://joshuahhh.com/dragology/#/demos/permutation-lab)\
 Parallel representations for permutations, updating in tandem. [Permutation lab](https://joshuahhh.com/dragology/#/demos/permutation-lab)

Andrew has been working to bring declarative dragging to existing applications, including his research on user interfaces for programming languages, as well as restarting work on the math game concept that kicked it all off.

[](https://hazel.org/)\
 [Hazel](https://hazel.org/) is an academic programming environment where every edit keeps your code meaningful. Using declarative dragging, refactorings become tangible and discoverable.

[](https://andrewblinn.com/supernool/)\
 Not long after submitting the paper, Andrew began working on his [math game](https://andrewblinn.com/supernool/) again, adapting declarative dragging to the gamepad and moving off the 2D canvas into a 3D world.

## Cite

```
@inproceedings{horowitz2026declarative,
  title     = {Declarative Dragging},
  author    = {Horowitz, Joshua and Blinn, Andrew and Heer, Jeffrey},
  booktitle = {Proceedings of the 39th Annual ACM Symposium on
               User Interface Software and Technology (UIST '26)},
  year      = {2026}
}
```
