---
title: A Topological Picture Book, Rendered
link: https://e-infinity.space/picture-book/
source: hnrss-org-frontpage
published: 2026-09-08T22:28:14Z
updated: 2026-09-08T22:28:14Z
first_seen: 2026-09-09T11:49:40.830353229Z
authors:
- mathgenius
summary: 'Article URL: https://e-infinity.space/picture-book/ Comments URL: https://news.ycombinator.com/item?id=49618050 Points: 116 # Comments: 12'
content: extracted
html: 2026-09-08-a-topological-picture-book-rendered.html
---

Hand-hatched surfaces after the mid-century manner of Francis, Apéry, Hilbert–Cohn-Vossen. Drag to turn the model freely (shift-drag to roll it); scroll to approach. On a touch screen, one finger turns, two fingers pinch to approach and twist to roll.

Sources & method

The drawing is made of strokes, not pixels. Silhouettes are the zero set of n·v on the mesh (found with interpolated normals, so they chain into smooth curves), boundaries and the double curve of an immersion are added as further chains, and all of them are drawn as tapered ribbons with a broad-nib pen model, weight that grows on the shadow side and toward the viewer, and coherent hand wobble. Visibility is settled on the GPU: a hidden-line pass draws the occluded parts dashed, and a one-sided paper halo under each near contour cuts the lines behind it. Hatching is a set of streamlines of the principal-curvature line field, traced at build time in three nested densities (the second along the other principal direction, for cross-hatching); tone selects which family is inked and where each stroke feathers out. Highlights stay bare paper. The principal directions are ordered by signed curvature, not magnitude, so the two families stay continuous across the loci where κ₁ = −κ₂. Contour chains are lightly smoothed before inking. Strokes overshoot their ends a little in the manner of sketchy line rendering; the ink pooling at stroke starts and the ragged bleed into the paper grain are hand-tuned effects of this page (a widened ribbon whose fringe is gated by a fibre-like noise), not taken from a paper. Labels are hand-lettered, pinned to points of the surface with a leader line, and dimmed when their point is hidden.

G. K. Francis, *A Topological Picture Book*, Springer, 1987 — the style target: contour drawing with cusps, double curves, hidden lines, and sparing hatched bands.

P. Bénard, A. Hertzmann, “Line Drawings from 3D Models: A Tutorial,” *Foundations and Trends in Computer Graphics and Vision* 11(1–2), 2019 — the contour pipeline: smooth silhouettes as n·v = 0, chaining, visibility, stylization.

A. Hertzmann, “Introduction to 3D Non-Photorealistic Rendering: Silhouettes and Outlines,” *SIGGRAPH 99 Course Notes* — silhouettes from interpolated vertex normals (marching-triangles on n·v).

A. Hertzmann, D. Zorin, “Illustrating Smooth Surfaces,” *SIGGRAPH 2000*, pp. 517–526 — hatching along principal curvature directions, cross-hatching only in dark regions, blank highlights, undercuts.

B. Jobard, W. Lefer, “Creating Evenly-Spaced Streamlines of Arbitrary Density,” *Visualization in Scientific Computing*, 1997 — the separation-distance rule used to trace the hatch streamlines.

A. Appel, F. J. Rohlf, A. J. Stein, “The Haloed Line Effect for Hidden Line Elimination,” *SIGGRAPH 1979* — the paper haloes at line crossings.

J. D. Northrup, L. Markosian, “Artistic Silhouettes: A Hybrid Approach,” *NPAR 2000* — chaining silhouette segments and rendering them as stylized strokes with tapering and width variation.

T. Strothotte, B. Preim, A. Raab, J. Schumann, D. R. Forsey, “How to Render Frames and Influence People,” *Computer Graphics Forum* 13(3) (Eurographics 1994) — sketch-like line rendering: lines that overshoot their endpoints and wiggle, drawn with a pen model whose width varies along the stroke.

M. P. Salisbury, S. E. Anderson, R. Barzel, D. H. Salesin, “Interactive Pen-and-Ink Illustration,” *SIGGRAPH 1994*, pp. 101–108 — stroke textures and the placement of hand-character strokes to reach a target tone.

E. Praun, H. Hoppe, M. Webb, A. Finkelstein, “Real-Time Hatching,” *SIGGRAPH 2001* — nested tone levels of hatching; here realized with object-space strokes so the hatching never swims.

G. Winkenbach, D. H. Salesin, “Computer-Generated Pen-and-Ink Illustration,” *SIGGRAPH 1994*, pp. 91–100 — tone by stroke density and thickness; stroke textures.

G. Elber, “Line Art Rendering via a Coverage of Isoparametric Curves,” *IEEE TVCG* 1(3), 1995 — hatching along isoparametric curves (the parameter-line stripes mode).

T. Saito, T. Takahashi, “Comprehensible Rendering of 3-D Shapes,” *SIGGRAPH 1990*, pp. 197–206 — edge extraction from normal and depth buffers (the optional Sobel edge filter).

W. E. Lorensen, H. E. Cline, “Marching Cubes,” *SIGGRAPH 1987*; A. Doi, A. Koide, “An Efficient Method of Triangulating Equi-Valued Surfaces by Using Tetrahedral Cells,” *IEICE Trans.* E74(1), 1991 — the implicit surfaces are polygonized by the tetrahedral variant.

T. Möller, B. Trumbore, “Fast, Minimum Storage Ray-Triangle Intersection,” *J. Graphics Tools* 2(1), 1997 — the segment–triangle test behind the double-curve computation.

R. Kusner, “Conformal Geometry and Complete Minimal Surfaces,” *Bull. Amer. Math. Soc.* 17(2), 1987 — source of the Bryant–Kusner parametrization used for Boy’s surface (checked here numerically: antipodal boundary gluing and threefold symmetry hold to machine precision). The general-p form used by kusner() has denominator w2p + κpwp − 1 with κp = 2√(2p−1)/(p−1) and prefactor p/(p−1); the constant was fixed by checking numerically that the pre-inversion surface is minimal (a circulating p = 2 version with √3 in place of 2√3 is not). See also F. Apéry, *Models of the Real Projective Plane*, Vieweg, 1987, whose Cartesian family (as tabulated on R. Ferréol’s *mathcurve.com*, “Morin surface”) gives the Morin preset and, with n = 3, a second model of Boy’s surface.

Related reading: D. DeCarlo et al., “Suggestive Contours for Conveying Shape,” SIGGRAPH 2003; R. Kalnins et al., “WYSIWYG NPR,” SIGGRAPH 2002.

Notation

Formulas are JavaScript expressions; ^ is accepted for powers. Available: sin cos tan asin acos atan atan2 sinh cosh tanh exp log sqrt cbrt abs sign pow min max floor hypot pi tau e sq(x). Range boxes accept expressions too (2\*pi). The helper boy(u,v) returns the Bryant–Kusner immersion of ℝℙ² as \[x,y,z\] (u = radius in \[0,1\], v = angle). torusknot(u,v,p,q,R,r,a) returns the tube of radius a about the (p,q) torus knot on the torus of radii R, r (defaults 2, 3, 2.2, 1, 0.42); u runs once along the knot, v around the tube. apery(u,v,n,k) is Apéry’s Cartesian family with u ∈ \[−π/2, π/2\]: n = 2, k = 1 is Morin’s surface (v ∈ \[0, 2π\]); n = 3, k = 1 is Boy’s surface (v ∈ \[0, π\]). kusner(u,v,p,d) is the Kusner–Bryant family, w = tan(πu/4) eiv: u ∈ \[0, 2\] is the whole sphere (p = 2 is Morin’s surface), u ∈ \[0, 1\] covers ℝℙ² once for odd p (p = 3 is boy); d shifts the centre of inversion (default −½).
