---
title: 'Antimony: CAD Software Evolved from Lisp Machines and Not Drafting Tables (2015)'
link: https://www.mattkeeter.com/projects/antimony/3/
source: hnrss-org
published: 2026-11-02T08:29:40Z
updated: 2026-11-02T08:29:40Z
first_seen: 2026-11-04T00:52:13.198927099Z
authors:
- pmkary
content: extracted
html: 2026-11-02-antimony-cad-software-evolved-from-lisp-machines-and-not.html
remote_preview:
  url: https://www.mattkeeter.com/projects/antimony/3/images/graph.png
  alt: Graph example
---

## about

Antimony is a computer-aided design (CAD) tool from a parallel universe in which CAD software evolved from Lisp machines rather than drafting tables.

## demo

[player.vimeo.com](https://player.vimeo.com/video/125111378)

## foundations

Antimony is built on three mostly-orthogonal axes:

- A framework for tracking information flow through directed acyclic graphs
- A geometry engine for doing [CSG](http://en.wikipedia.org/wiki/Constructive_solid_geometry)
- A standard library of shapes and transforms

### Graph engine

Solid modeling in Antimony is done by connecting nodes in a graph:\
 ![Graph example](https://www.mattkeeter.com/projects/antimony/3/images/graph.png)

Each node is defined by a customizable script. Antimony uses Python 3.x plus a few extra magic keywords.

Here's what the circle node above looks like:

```python
import fab.shapes

# Define a set of input ports
# (and inject variables into the local namespace)
input('x', float)
input('y', float)
input('r', float)

# Call the 'circle' function in the standard shapes library
# using the variables that came in from the input ports
c = fab.shapes.circle(x, y, r)

# Create an output port that produces the circle shape
output('shape', c)
```

Nodes usually represent a primitive shape or transform. Links are tracked and changes are automatically propagated downstream.

![Change propagation](https://www.mattkeeter.com/projects/antimony/3/images/tracking.gif)

The graph engine scales up to complex multi-part assembles.

![Kelvin](https://www.mattkeeter.com/projects/antimony/3/images/big.png)

![Kelvin](https://www.mattkeeter.com/projects/antimony/3/images/bigger.png)

### Geometry engine

Antimony's geometry engine uses [functional representations](http://en.wikipedia.org/wiki/Function_representation) for solid modeling. This representation is particularly suitable for modeling with boolean operations (union / intersection / difference).

![Boolean example](https://www.mattkeeter.com/projects/antimony/3/images/csg.png)

In the application, the geometry engine renders shapes as shaded bitmaps. They're then blitted to a 3D viewport.

![Application screenshot](https://www.mattkeeter.com/projects/antimony/3/images/app.png)

Antimony can export heightmaps for 2.5D processes and `.stl` files for 3D manufacturing. The `.stl` export includes feature detection to keep corners and edges sharp.

![Mesh](https://www.mattkeeter.com/projects/antimony/3/images/export.png)

### Standard library

Antimony's standard library defines many [shapes and transforms](https://github.com/mkeeter/antimony/blob/develop/py/fab/shapes.py), from basic (rotate, scale, boolean operations) to unusual (attract, repel, bend).

These shapes are used in node definitions, which typically add input and output ports and can optionally define UI features.

Here's an example of a node that defines UI features:

```python
import fab

title('Rectangle')

input('xmin', float)
input('ymin', float)
input('xmax', float, 1)
input('ymax', float, 1)

output('shape', fab.shapes.rectangle(xmin, xmax, ymin, ymax))

# UI
sb.ui.wireframe([(xmin, ymin, 0), (xmax, ymin, 0),
                 (xmax, ymax, 0), (xmin, ymax, 0)], close=True)
sb.ui.point(xmin, ymin, 0)
sb.ui.point(xmax, ymax, 0)
```

In the 3D viewport, the UI features defined at the bottom of the script are drawn over the rendering of the rectangle.

These UI features are hooked back into the graph: dragging the points at the rectangle's corners pushes changes to the graph node.

![UI dragging](https://www.mattkeeter.com/projects/antimony/3/images/dragging.gif)

**As an aside**: most users will be satisfied with the built-in shapes, but it's also possible to define custom shapes.

Shapes are represented with a prefix string syntax that's easy to parse but a bit cryptic to decipher. Here's the definition for the circle shape used above:

```python
def circle(x0, y0, r):
    return Shape(
            '-r+q-Xf%gq-Yf%gf%g' % (x0, y0, r), # math string
            x0 - r, y0 - r, x0 + r, y0 + r)     # bounds
```

Unpacking and hierarchically indenting the prefix string gives

```python
-
  r
    +
      q
        -
          X
          f%g
      q
        -
          Y
          f%g
  f%g
% (x, y, r)
```

This encodes a distance field representation of a 2D circle:

```python
f(x,y) = sqrt(pow(x - x0, 2) + pow(y - y0, 2)) - r
```

where `x0`, `y0`, and `r` are the circle's center and radius (specified when `circle` is called).

The exact mapping from prefix strings to infix functions is left as an exercise for the reader (or can be looked up in Appendix A of [my thesis](https://www.mattkeeter.com/research/thesis.pdf)).

## download

A prebuilt Mac application can be downloaded [from the Github releases page](https://github.com/mkeeter/antimony/releases).

Antimony's source is [available on Github](https://github.com/mkeeter/antimony), along with instructions for [building it](https://github.com/mkeeter/antimony/blob/master/BUILDING.md) on Mac and Linux.

## development

Antimony is a long-running project under [active development](https://github.com/mkeeter/antimony/graphs/contributors).

![Development](https://www.mattkeeter.com/projects/antimony/3/images/development.png)

It's at a beta level of stability: solid, but not recommended for mission-critical use.

If you're interested in contributing, there's a pretty low bar for entry. Given basic Python knowledge, you can get started making custom nodes (like this [HSB color node](https://github.com/mkeeter/antimony/blob/develop/py/nodes/Color/set_color_hsb.node) contributed by [RobotGrrl](https://twitter.com/RobotGrrl) or the [plethora of nodes](https://github.com/mkeeter/antimony/commits?author=neilger) contributed by [Neil](http://ng.cba.mit.edu/)).

For more ambitious folks with C++ experience, there are a whole host of improvements that could be made, from speeding up rendering to cleaning up the UI and optimizing graph evaluation. Send me a note if this piques your interest.

## history

Antimony grew out of my work on powerful personal-scale CAD/CAM workflows at the [MIT Center for Bits and Atoms](http://cba.mit.edu).

It's the latest tool in a line of software that included [cad\_ui](https://www.mattkeeter.com/projects/solver) and [kokopelli](https://www.mattkeeter.com/projects/kokopelli), and draws inspiration from [fabserver](http://kokompe.cba.mit.edu).

For a historical perspective, there are a set of earlier Antimony writeups:

- [Early 2014](https://www.mattkeeter.com/projects/antimony/1) (developed as a demo for the World Economic Forum)
- [Fall 2014](https://www.mattkeeter.com/projects/antimony/2) (updated for use in How to Make Almost Anything)

This update coincides with the 2015 edition of [Fab Academy](http://fabacademy.org/); use [this search query](https://www.google.com/search?q=site%3Ahttp%3A%2F%2Ffabacademy.org%2Farchives%2F2015%2F%20antimony&rct=j) to see how people are using it.

## press & discussion

- [Hackaday write-up](http://hackaday.com/2015/05/29/otherworldy-cad-software-hails-from-a-parallel-universe/)
- [Interview with Libre Graphics World](https://librearts.org/2015/05/introducing-antimony-node-based-cad-application/)
- [Hacker News discussion](https://news.ycombinator.com/item?id=9623609)
