---
title: Growing Trees
link: https://brashandplucky.com/2026/10/15/growing-trees.html
source: hnrss-org
published: 2026-10-23T14:07:42Z
updated: 2026-10-23T14:07:42Z
first_seen: 2026-10-26T14:07:38.108821132Z
authors:
- ibobev
content: extracted
html: 2026-10-23-growing-trees.html
remote_preview:
  url: https://brashandplucky.com/thumbnails/2026/chemaguerra-growing-trees.png
---

![Procedurally grown linden with foliage](https://brashandplucky.com/uploads/2026/growing-trees/chemaguerra-growing-trees.png)\
A procedurally grown linden. The branching structure comes from the simulation described below.

I’ve been playing with procedural tree growth recently. The method I am using is based on [Self-organizing tree models for image synthesis](https://www.algorithmicbotany.org/papers/selforg.sig2009.html), by Palubicki et al. (SIGGRAPH 2009).

The idea is beautifully simple: buds compete for light, the tree distributes a growth resource among them, and the successful ones produce new shoots. Those shoots change the light available for the next round. The crown shape emerges from this feedback loop.

Here is a quick breakdown, with some visualizations from my implementation.

## One season at a time

The basic building block is a *metamer*: an internode (a short piece of stem), a leaf, and the buds at its upper node. A terminal bud continues an axis, and axillary buds may start lateral branches.

Each iteration represents one growing season:

1. Measure the light available to every bud.
2. Sum that light from the tips down to the root.
3. Distribute a proportional growth resource back up the tree.
4. Grow shoots from the buds that receive enough resource.
5. Shed poorly lit branches and age dormant buds.

The same seed growing from season 2 through 30. The camera and scale stay fixed.

## Where is the light?

One way to approximate light availability is a voxel grid. Each bud adds shadow to a widening pyramid of voxels below it, with its contribution decaying with depth. Contributions from all buds add up, so the interior naturally becomes darker as the tree grows.

Each bud reads two things from this grid: an available-light value \\(Q\\), and a direction \\(\\mathbf{V}\\) toward the least shaded neighboring voxel inside its perception cone.

![Vertical slice through the tree's shadow grid](https://brashandplucky.com/uploads/2026/growing-trees/chemaguerra-tree-shadow-grid.svg) ![Tree buds colored by available light](https://brashandplucky.com/uploads/2026/growing-trees/chemaguerra-tree-bud-light.svg)\
Left: a vertical slice through the shadow grid. Right: light per bud. Cyan buds are well exposed. Orange ones are shaded.

The paper also supports *space colonization*: fill an envelope with marker points, let buds compete for nearby markers, and remove markers as growth occupies their space. Same growth machinery, a different way to decide where growth is possible.

## Who gets to grow?

Light values are first summed down the branching structure. At the base, the total \\(Q\_{base}\\) becomes a growth resource \\(v=\\alpha Q\_{base}\\). This resource travels back up, splitting at every branching point.

![Light summed from the tree tips to its base](https://brashandplucky.com/uploads/2026/growing-trees/chemaguerra-tree-light-sum.svg) ![Growth resource distributed back toward the buds](https://brashandplucky.com/uploads/2026/growing-trees/chemaguerra-tree-resource-allocation.svg)\
Left: accumulated light flowing down. Right: resource flowing up. Orange dots mark buds that receive enough to grow.

In the extended Borchert-Honda model, the main continuation receives:

\\\[v\_m=v\\frac{\\lambda Q\_m}{\\lambda Q\_m+(1-\\lambda)Q\_l}\\\]

The lateral side receives the remainder. At \\(\\lambda=0.5\\), resource follows light. Larger values favor the main axis. Smaller values favor the laterals.

This little bias has a surprisingly large effect:

![Broad crown grown with lambda 0.46](https://brashandplucky.com/uploads/2026/growing-trees/chemaguerra-tree-lambda-046.png) ![Narrow crown grown with lambda 0.56](https://brashandplucky.com/uploads/2026/growing-trees/chemaguerra-tree-lambda-056.png)\
Left: lambda=0.46, a spreading crown. Right: lambda=0.56, a dominant leader.

A bud receiving less than one unit stays dormant. Otherwise it produces a shoot of \\(\\lfloor v\\rfloor\\) metamers, subject to a configured cap. New metamers receive their own buds, and the process repeats. Magic!

## Which way?

Each new segment follows a normalized weighted sum of its previous direction, the light-seeking direction \\(\\mathbf{V}\\), an upward or downward *tropism*, and some noise. A horizontal component can make lateral branches spread into flat tiers.

Bud placement around the stem (*phyllotaxis*) provides another important control. Spiral arrangements, opposite pairs, and annual whorls produce very different branching patterns. The frame is parallel-transported along the shoot so the arrangement stays coherent as it bends.

![Six arrangements of buds around a shoot](https://brashandplucky.com/uploads/2026/growing-trees/chemaguerra-tree-phyllotaxis.svg)\
Six phyllotaxis patterns, seen down the shoot axis. Numbers identify consecutive nodes.

## Dropping branches and adding girth

With the shadow model, a lateral branch is shed when the light collected by its subtree, divided by its metamer count, falls below a threshold. Young branches get a grace period. Otherwise the inner crown would keep accumulating starved twigs.

![Dense branching structure with shedding disabled](https://brashandplucky.com/uploads/2026/growing-trees/chemaguerra-tree-shedding-off.svg) ![More open branching structure with shedding enabled](https://brashandplucky.com/uploads/2026/growing-trees/chemaguerra-tree-shedding-on.svg)\
Same seed, without shedding (left) and with it (right). Self-pruning changes the light available for subsequent growth too.

Branch thickness comes afterwards, using a *pipe model*:

\\\[r\_{parent}^{n}=\\sum\_i r\_i^{n}\\\]

Start with a fixed radius at the tips and accumulate toward the root. For \\(n=2\\), cross-sectional area is preserved at junctions. Larger exponents produce slenderer trunks. Shed branches still contribute to the girth below them. Losing a branch does not make the trunk shrink.

Width and passive sag are calculated from the grown skeleton and do not feed back into growth.

## From branches to foliage

The skeleton becomes a bark mesh by sweeping tubes along its axes. For foliage I use *twig spray cards*: each card depicts a small twig carrying several leaves, using variants from a procedurally drawn atlas.

![Procedural atlas containing four leafy twig sprays](https://brashandplucky.com/uploads/2026/growing-trees/chemaguerra-tree-leaf-atlas.png) ![Foliage card geometry shown in wireframe](https://brashandplucky.com/uploads/2026/growing-trees/chemaguerra-tree-spray-cards.png)\
The twig atlas and the cards that carry it. Each card replaces a whole clump of leaves.

Blending the card normals toward an ellipsoid around the crown helps the foliage shade as a volume. Lower detail levels retain fewer, larger cards and simplify the bark. At a distance, baked directional impostors take over.

*Bonus:* The grown tree can also be baked into depth-field views for forest rendering. The image below uses many instances of one baked linden, with size variation. The forest representation and lighting deserve a separate post, though.

![A forest rendered from instances of one baked linden](https://brashandplucky.com/uploads/2026/growing-trees/chemaguerra-tree-forest.png)\
One grown tree, instanced as a forest using baked depth-field views.
