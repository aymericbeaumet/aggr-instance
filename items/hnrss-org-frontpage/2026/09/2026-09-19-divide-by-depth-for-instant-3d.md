---
title: Divide by depth for instant 3D
link: https://gabrieloc.com/2026/09/15/perspective.html
source: hnrss-org-frontpage
published: 2026-09-19T19:43:13Z
updated: 2026-09-19T19:43:13Z
first_seen: 2026-09-22T04:43:15.388238719Z
authors:
- gabrieloc
summary: 'Article URL: https://gabrieloc.com/2026/09/15/perspective.html Comments URL: https://news.ycombinator.com/item?id=49769561 Points: 105 # Comments: 18'
content: extracted
html: 2026-09-19-divide-by-depth-for-instant-3d.html
preview:
  file: 2026-09-19-divide-by-depth-for-instant-3d.preview-392404ffb039.webp
  width: 256
  height: 194
  color: '#0a0a11'
images:
- source: https://gabrieloc.com/assets/perspective/preview.gif
  original:
    file: 2026-09-19-divide-by-depth-for-instant-3d.image-4fb12647c0be.gif
    width: 624
    height: 474
  color: '#020108'
extra:
  content:interactive: true
---

When I first started working on games, I used high level frameworks that would give you a `Camera` that just kind of worked. But as I learned more and wanted to apply more creative techniques, I struggled to even have the vocabulary to search for what I wanted to do, and only after writing lower level graphics code did I understand cameras are based around kind of really simple math.

In the excellent [One Formula That Demystifies 3D Graphics](https://www.youtube.com/watch?v=qjWkNZ0SXfo) (via [@tsoding](https://twitter.com/tsoding)), we’re presented with the following:

```
(x, y, z)
x' = x/z
y' = y/z
```

In essence, if we say \\(y\\) is up and \\(z\\) is forward, 3D coordinates \\((x, y, z)\\) can be projected into 2D coordinates \\((x', y')\\) by dividing \\(x\\) and \\(y\\) by \\(z\\). For example, if we have a series of 3D points that only vary in depth, as depth increases, their projected positions get closer to vanishing point \\((0,0)\\):

\\\[\\begin{array}{c|c} (x,y,z) & (x',y') \\\\ \\hline (2,1,2) & (1,0.5) \\\\ (2,1,4) & (0.5,0.25) \\\\ (2,1,8) & (0.25,0.125) \\end{array}\\\]

This can be demonstrated by a ball that moves and scales with depth as it orbits the camera’s up axis, offset along the \\(z\\) axis by `forward`:

Using the same principle, we can even draw more sophisticated “geometry” in the same way!

Obviously, these are very constrained and naive examples that are largely impractical in all but the simplest scenarios. When we do any kind of 3D work, we typically need to involve things like the direction the camera’s pointing in, it’s position, it’s field of view, etc. While we *could* in theory hack those shaders to support these features, there is a way that’s less effort and more practical. It’s called the *perspective projection matrix*, and it’s the magic behind the mighty `Camera`.

## Perspective Projection

There are a few ways to construct a perspective projection matrix, depending on your use case. Computer vision and graphics for example, have slightly different conventions for layout, which is why it’s hard to point to a [single Wikipedia article](https://en.wikipedia.org/wiki/3D_projection) and expect a universal form. That being said, in triangle-based graphics, a common convention that’s often followed involves parameterization of *field of view*, *aspect ratio*, and *near and far clipping planes*. These values are all very important because they also double as a way to easily know what’s in frame and what isn’t, which lets us render our scenes in performant ways, such as through culling off-screen geometry and rendering only what’s visible.

While this varies across different coordinate conventions ([little consensus around which axes correspond to up, right, and forward](https://x.com/FreyaHolmer/status/1325556229410861056)), the general structure is mostly consistent:

\\\[P = \\begin{bmatrix} \\frac{f}{a} & 0 & 0 & 0 \\\\ 0 & f & 0 & 0 \\\\ 0 & 0 & A & B \\\\ 0 & 0 & 1 & 0 \\end{bmatrix}\\\]

This describes a camera with focal scale \\(f\\) (derived from vertical fov angle \\(\\theta\\)) and aspect ratio \\(a\\):

\\\[f = \\frac{1}{\\tan(\\theta/2)},\\quad a = \\frac{width}{height}\\\]

Where \\(A\\) and \\(B\\) represent depth mapping, derived from near and far clipping planes \\(n\\) and \\(F\\) respectively:

\\\[A = \\frac{F+n}{F-n}, \\quad B = -\\frac{2Fn}{F-n}\\\]

So how does this connect to the depth division trick from earlier? Turns out that’s a special form of the perspective projection matrix.

Given a point somewhere in 3D space, we first describe its position relative to the camera. This takes us from *world space*, where coordinates are shared by the whole scene, into *view space*, where the camera sits at the origin. We then multiply by the perspective projection matrix, producing *clip-space* coordinates that are ready for the perspective division.

\\\[\\begin{bmatrix} \\frac{f}{a} & 0 & 0 & 0 \\\\ 0 & f & 0 & 0 \\\\ 0 & 0 & A & B \\\\ 0 & 0 & 1 & 0 \\end{bmatrix} \\begin{bmatrix} x \\\\ y \\\\ z \\\\ 1 \\end{bmatrix} = \\begin{bmatrix} \\frac{f}{a}x + 0y + 0z + 0(1) \\\\ 0x + fy + 0z + 0(1) \\\\ 0x + 0y + Az + B(1) \\\\ 0x + 0y + z + 0(1) \\end{bmatrix} = \\begin{bmatrix} \\frac{f}{a}x \\\\ fy \\\\ Az + B \\\\ z \\end{bmatrix}\\\]

After dividing the first three clip-space components by \\(w\\), the coordinates no longer describe distances in the scene. Instead, they describe where the point falls inside the camera’s visible bounds. These are *normalized device coordinates*, usually shortened to NDC, which later allow points to finally be mapped to the screen’s output resolution.

\\\[x\_{ndc} = \\frac{\\frac{f}{a}x}{z}, \\qquad y\_{ndc} = \\frac{fy}{z}\\\]

In the earlier example, both focal scale \\(f\\) and aspect ratio \\(a\\) are omitted. Substituting them both with 1 gives us exactly the original depth division trick:

\\\[x\_{ndc} = \\frac{\\frac{1}{1}x}{z} = \\frac{x}{z}, \\qquad y\_{ndc} = \\frac{1y}{z} = \\frac{y}{z}\\\]

While the depth division trick works for camera-relative points in simple scenarios, it’s really just one step in the full graphics pipeline. Now that all of those terms have names, the whole journey looks like this:

\\\[\\mathrm{world} \\rightarrow \\mathrm{view} \\rightarrow \\mathrm{clip} \\xrightarrow{\\,/w\\,} \\mathrm{NDC} \\rightarrow \\mathrm{pixels} \\rightarrow \\mathrm{rasterization}\\\]

## Tying it all together

If there’s one thing to take away from this writeup, it’s that `Camera`s are just doing a few simple transformations, and if you understand what each of those transformations are for, you can pick and choose which parts to implement yourself based off your own needs and constraints. Sometimes you need the entire pipeline, and sometimes you need that one depth division.
