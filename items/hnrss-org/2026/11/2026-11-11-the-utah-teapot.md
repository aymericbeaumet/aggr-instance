---
title: The Utah Teapot
link: https://graphics.cs.utah.edu/teapot/
source: hnrss-org
published: 2026-11-11T19:45:10Z
updated: 2026-11-11T19:45:10Z
first_seen: 2026-11-12T10:54:03.804437502Z
authors:
- cobbal
summary: 'https://www.youtube.com/watch?v=k-23LgrFcvs Comments URL: https://news.ycombinator.com/item?id=50457618 Points: 146 # Comments: 34'
content: extracted
html: 2026-11-11-the-utah-teapot.html
remote_preview:
  url: https://graphics.cs.utah.edu/teapot/textures/teapot-cy.jpg
extra:
  content:interactive: true
---

Model Type: Curves (Coming Soon) Patches (Coming Soon) Triangle mesh Quad-dominant mesh Triangulate Tips Eliminate duplicate vertices Duplicate vertices with\
different normals Duplicate vertices with\
different texture coordinates Orientation: y-up z-up

 Wireframe Show Interior Resolution: \
 Version: 2026 - Cem Yuksel 2006 - Russ Fish 1992 - Hank Driskill\* 1987 - Frank Crow 1976 - Jim Blinn 1975 - Martin Newell Handle Spout Lid Body

* * *

 Show Custom Settings

 Blinn Scale Circular Trimmed Chamfer Curvature\
Continuous

Bottom: Round Flat Cap Open

Interior Version: Cem Yuksel Russ Fish Hank Driskill\* None

Spout Cap: Outer Trim Outer Inner None Lid Cap

Texture Coordinates: Patch Coordinates Patch Coordinates with Offsets Injective Layout Bijective Layout Show Texture Layout

 Match Edge Normals Symmetric Triangulation Flipped Triangulation

The **Utah Teapot** has been the symbol of computer graphics. It was one of the first computer graphics models created using [Bézier patches](https://en.wikipedia.org/wiki/B%C3%A9zier_surface). This is the official Utah Teapot page, where you can you can visualize and download any of these historical versions or customize it, using the settings above.

**Terms of Use and Attribution:** *The Utah Teapot model files are freely available for any use, including commercial use. To preserve the historical identity and provenance of the model, users are requested to identify the model, and any substantially derived version of it, as a **Utah Teapot** (or **Modified Utah Teapot**) and to acknowledge its origin as the historic Utah Teapot developed at the University of Utah. This request is intended solely to preserve the historical identity of the Utah Teapot and is not intended to restrict use of the model. The model files are provided “as is,” without warranty of any kind. The provider of these files shall not be liable for any claims, damages, or other liability arising from their use.*

The Utah Teapot was originally created by Martin Newell and went through a number of modifications over the years. It quickly became famous within the graphics community after making its first appearance at SIGGRAPH in 1976. Its extensive use in graphics prompted Arvo and Kirk \[1987\] to name it the “Teapotahedron” and jokingly call it “the sixth [Platonic solid](https://en.wikipedia.org/wiki/Platonic_solid)”. It remains one of the standard models for evaluating new computer graphics algorithms and frequently appears in technical papers presented at SIGGRAPH and other graphics conferences. Utah Teapot has also appears in art installations, movies, video games, and a wide range of other graphics applications. Versions of the Utah Teapot model are included in numerous rendering and modeling programs and graphics APIs, among them 3ds Max, Direct3D, Houdini, Modo, OpenGL, POV-Ray, Quartz Composer, and RenderMan. See the [Wikipedia page on the Utah Teapot](https://en.wikipedia.org/wiki/Utah_teapot).

[www.youtube.com](https://www.youtube.com/watch?v=k-23LgrFcvs)

[Martin Newell](https://en.wikipedia.org/wiki/Martin_Newell_\(computer_scientist\)), [Jim Blinn](https://www.jimblinn.com/), and [Cem Yuksel](https://www.cemyuksel.com/) tell the story of the [Utah Teapot](https://graphics.cs.utah.edu/teapot/) at SIGGRAPH 2026, commemorating the 50th anniversary of the Utah Teapot's first appearance at SIGGRAPH in 1976. The extended abstract of this talk includes additional information: “[The Return of the Utah Teapot](https://doi.org/10.1145/3799818.3816116)” ([PDF](https://graphics.cs.utah.edu/teapot/The_Return_of_the_Utah_Teapot.pdf)) by Cem Yuksel, Jim Blinn, and Martin Newell.

## Utah Teapot Versions

![](https://graphics.cs.utah.edu/teapot/images/utah_teapot_evolution.svg)

The Utah Teapot has a number of versions that were developed and released over the years by different people from the University of Utah, as summarized below. [Version 1975](https://graphics.cs.utah.edu/teapot/#v1975) is how [Martin Newell](https://en.wikipedia.org/wiki/Martin_Newell_\(computer_scientist\)) modeled it. [Version 1976](https://graphics.cs.utah.edu/teapot/#1976) is the scaled down version by [Jim Blinn](https://www.jimblinn.com/). [Version 1987](https://graphics.cs.utah.edu/teapot/#v1987) by [Frank Crow](https://en.wikipedia.org/wiki/Franklin_C._Crow) adds the round bottom. [Version 1992](https://graphics.cs.utah.edu/teapot/#v1992) is the first attempt by [Hank Driskill](https://www.linkedin.com/in/hank-driskill-1a7140165/) for turning the Teapot into a solid object. [Version 2006](https://graphics.cs.utah.edu/teapot/#v2006) by [Russ Fish](https://www.linkedin.com/in/russ-fish-b1113b/) improves the interior shape. Finally, [Version 2026](https://graphics.cs.utah.edu/teapot/#v2026) by [Cem Yuksel](https://www.cemyuksel.com) further improves the interior, adds chamfers where the handle and the spout connect to the body, and makes the shape of the Teapot [curvature-continuous](https://graphics.cs.utah.edu/research/projects/continuity-enhancement/) to achieve reflection continuity.

![](https://graphics.cs.utah.edu/teapot/images/utah_teapot1975.svg)

### Version 1975 - Martin Newell

This is the original version created by [Martin Newell](https://en.wikipedia.org/wiki/Martin_Newell_\(computer_scientist\)), while he was a PhD student at the University of Utah, by approximating the proportions of a physical teapot (Melitta by Friesland Porzellan). It is formed by 28 cubic Bézier patches, which he first [drew the control points on an engineering paper](https://archive.org/details/newell_teapot_drawing/). It is one of the first computer graphics object modeled using a set of nonlinear surfaces, rather than a collection of polygons. Notably, Martin Newell's version is taller than the commonly known shape of the famous Utah Teapot, and it has no bottom. This is the model that appeared in his dissertation, titled “[The utilization of procedure models in digital image synthesis](https://collections.lib.utah.edu/ark:/87278/s651906h)” in 1975, along with the rest of the tea set he modeled, though he prepared these models earlier, sometime in 1974.

![](https://graphics.cs.utah.edu/teapot/images/newell-teapot-drawing-01-small.webp)![](https://graphics.cs.utah.edu/teapot/images/newell-teaset-640-471.webp)

Martin Newell's drawing of the Teapot and the tea set image from his dissertation.

Here is [a YouTube video](https://www.youtube.com/watch?v=ui3R1Dlmsk0&t=3182), where Martin Newell, at an event celebrating the 50th anniversary of [Computer Science department at the University of Utah](https://www.cs.utah.edu/), talking about the Teapot model.

![](https://graphics.cs.utah.edu/teapot/images/utah_teapot1976.svg)

### Version 1976 - Jim Blinn

[Jim Blinn](https://www.jimblinn.com/), while he was a PhD student at the University of Utah, scaled Martin Newell's teapot model by 3/4 during a demo. He liked its new shape and kept it that way. In fact, everyone liked this shorter version, including Martin Newell. Since 1976, this version of the teapot appeared in numerous publications, turning it into a symbol of computer graphics.

Utah Teapot was first introduced to the computer graphics community at SIGGRAPH 1976 in an article by Jim Blinn and Martin Newell, titled “[Texture and Reflection in Computer Generated Images](https://doi.org/10.1145/360349.360353)” ([PDF](<https://graphics.cs.utah.edu/teapot/Texture and Reflection in Computer Generated Images.pdf>)). It was also published in the Communications of the ACM journal and an image from this article, featuring the Utah Teapot with a texture mapped to its patches, was used as the cover of the October 1976 issue.

Jim Blinn explains the components of the teapot and why it was squished in an article titled “[What, Teapots Again?](https://doi.org/10.1109/MCG.1987.277080)” ([PDF](<https://graphics.cs.utah.edu/teapot/What Teapots Again.pdf>)), published in IEEE Computer Graphics & Applications in September 1987. There, he also suggests adding a flat bottom cap.

![](https://graphics.cs.utah.edu/teapot/images/cacm-cover-oct1976.jpg) ![](https://graphics.cs.utah.edu/teapot/images/blinn-teapot-fig9a-orig.webp) ![](https://graphics.cs.utah.edu/teapot/images/blinn-teapot-fig9b-orig.webp)

The cover of the CACM journal in October 1976, featuring one of the images from Blinn and Newell's paper at SIGGRAPH 1976, and two other images from the same paper with the original pixels computed on PDP-11 (after tone mapping) by Jim Blinn.

Here is [a YouTube video](https://www.youtube.com/watch?v=VktqJ5I9aKY&t=3942s), where Jim Blinn, at a reception honoring [Ivan Sutherland](https://en.wikipedia.org/wiki/Ivan_Sutherland) at SIGGRAPH 2018, explaining why the Utah Teapot model is squished.

![](https://graphics.cs.utah.edu/teapot/images/utah_teapot1987.svg)

### Version 1987 - Frank Crow

The most common version of Utah Teapot that appears in numerous graphics applications is the one released by [Frank Crow](https://en.wikipedia.org/wiki/Franklin_C._Crow), who was another PhD graduate of the University of Utah, in an article titled “[The Origins of the Teapot](https://doi.org/10.1109/MCG.1987.277023)” ([PDF](<https://graphics.cs.utah.edu/teapot/The Origins of the Teapot.pdf>)), published in IEEE Computer Graphics & Applications in January 1987. This article discusses the important role that the Utah Teapot model played in the early days of computer graphics and includes the control points of the scaled model by Jim Blinn. Except that all control points are shifted up just a bit to make room for a round bottom surface, defined by 4 additional Bézier patches. This brings the total number of patches to 32 and the total number of control points to 512.

![](https://graphics.cs.utah.edu/teapot/images/crow-patch-boundaries.webp) ![](https://graphics.cs.utah.edu/teapot/images/crow-control-points.webp)

Crow's figure showing the patch boundaries and the control polygon of the Teapot without its body, but with the bottom that he added.

![](https://graphics.cs.utah.edu/teapot/images/utah_teapot1992.svg)

### Version 1992 - Hank Driskill

The handle and the spout patches of the original Utah Teapot model intersect with its body. This was necessary, as it was modeled using cubic Bézier patches. This was also a useful feature for testing various hidden surface removal algorithms that were being developed at the time, and their abilities in rendering intersecting surfaces. Also, because cubic Bézier patches cannot form perfectly circular shapes, the teapot's body was not perfectly round. Perhaps more importantly, it did not contain an interior and a rather visible gap around the lid.

In 1992, [Hank Driskill](https://www.linkedin.com/in/hank-driskill-1a7140165/), another PhD student at the University of Utah, addressed all these problems. He used the [Alpha\_1 software](https://www-old.cs.utah.edu/gdc/projects/alpha1/), which was being developed by Professors [Elaine Cohen](http://www.cs.utah.edu/~cohen) and [Rich Riesenfeld](http://www.cs.utah.edu/~rfr) and their students/collaborators, and he prepared a trimmed NURBS model of the Utah Teapot, connecting its perfectly round body to its handle and spout, and a new interior surface. Its bottom was flat, as suggested by Jim Blinn, instead of the round bottom that was previously added by Frank Crow. Nonetheless, Hank Driskill's design, titled “Utah, the Next Generation,” featuring this new Utah Teapot (with warp nacelles), won the SIGGRAPH 1992 T-Shirt Contest.

\*Hank Driskill's version used a thick curve to cut a path through the spout. Here, we use the spout interior designed by Russ Fish instead.

The interior of this version was defined by revolving B-spline curves, resulting in 86 additional patches, so this version of the Utah Teapot has 118 patches in total.

![](https://graphics.cs.utah.edu/teapot/images/driskill-teapot1-fixed.jpg) ![](https://graphics.cs.utah.edu/teapot/images/driskill-teapot2-fixed.jpg) ![](https://graphics.cs.utah.edu/teapot/images/driskill-tshirt-fixed.webp)

Driskill's Teapot images with his new model and warp nacelles, and the award-winning t-shirt at SIGGRAPH 1992.

![](https://graphics.cs.utah.edu/teapot/images/utah_teapot2006.svg)

### Version 2006 - Russ Fish

The version by Hank Driskill was later improved by [Russ Fish](https://www.linkedin.com/in/russ-fish-b1113b/), a co-founder and technical lead of the [Alpha\_1](https://www-old.cs.utah.edu/gdc/projects/alpha1/) project at the University of Utah. Russ Fish modeled a new interior with a hollow knob for the lid and presented his “third-generation” teapot at SIGGRAPH 2006 with the title “[Teapot Subdivision](https://doi.org/10.1145/1180098.1180135).” He also released and [documented](https://www-old.cs.utah.edu/gdc/projects/alpha1/help/man/html/model_repo/model_teapot/model_teapot.html) this new and earlier versions of the Utah Teapot. In particular, his solution for generating the interior of the spout is elegant and closely follows the exterior contours prepared by Martin Newell. Similar to Hank Driskill's version, this one also has a flat bottom, rather than the round bottom of Frank Crow's version.

![](https://graphics.cs.utah.edu/teapot/images/teapot1998-Russ-Fish.jpg)

The "Teapot Subdivision" image generated by Russ Fish.

This version forms even more patches to model the interior, resulting in 134 patches in total.

![](https://graphics.cs.utah.edu/teapot/images/utah_teapot2026.svg)

### Version 2026 - Cem Yuksel

The latest version of the Utah Teapot is prepared by Prof. [Cem Yuksel](http://www.cemyuksel.com) of the University of Utah. This version uses much fewer patches for modeling the Teapot's interior and more closely follows the contours of its exterior. It contains a solid knob for the lid. The spout interior is also slightly modified to ensure that the surface normals are continuous where the tip of the spout meets the interior surface.

The new interior design works with both the round bottom of Frank Crow's model and the flat bottom used by Hank Driskill and Russ Fish. The official version has a round bottom, so that it matches the most commonly used form of the Utah Teapot today.

In addition, this latest version also includes two types of modifications to the exterior surface of the Utah Teapot:

- Its original spline models are modified using a new continuity enhancement method to ensure curvature continuity. This ensures that the reflections on the teapot appear smooth, without the sharp changes that appear in the earlier versions. See the SIGGRAPH 2026 paper on this topic by Cem Yuksel, titled “[Continuity-Enhancing Degree Elevation and Splits](https://graphics.cs.utah.edu/research/projects/continuity-enhancement/)” for more information.

- It also includes small chamfers where the spout and the handle meet the Teapot's body, ensuring that the teapot's surface normal is continuous over the exterior surface. The actual physical teapot that Martin Newell used as a reference has much larger chamfers, but adding such larger chamfers would deviate too much from the historical shape of the digital model.

The lightweight interior model of this version only needs 30 additional patches, so the resulting teapot has 62 patches in total.

Cem Yuksel also added texture mapping layouts that can be applied all versions of the Utah Teapot to achieve a bijective or a lower-distortion injective mapping.

## Custom Settings

- **Blinn Scale:** Apply Jim Blinn's scaling factor of 0.75 that gives the Utah Teapot is popular proportions.
- **Circular:** The teapot body and the lid get a perfectly circular shape, instead of the cubic Bézier approximation of the earlier versions.
- **Trimmed:** The handle and spout get properly attached to the body by trimming the polygons forming the body, the handle, and the spout.
- **Chamfer:** Forms a small chamfer around the handle and the spout where they meet the body, making the outer surface of the teapot smooth, such that the surface normal is continuous. This only works if the teapot is **trimmed**.
- **Curvature Continuous:** The shape of the teapot is slightly modified to ensure curvature continuity. This avoids sharp reflection discontinuities on the teapot's surface.
- **Bottom:** The bottom of the teapot can be **round**, as introduced by [Frank Crow](https://graphics.cs.utah.edu/teapot/#v1987), **flat**, a **cap** that also forms a flat shape with with minimal polygon count, or **open**, as in the original versions.
- **Interior:** Utah Teapot has three official interior designs, introduced by [Hank Driskill](https://graphics.cs.utah.edu/teapot/#v1992), [Russ Fish](https://graphics.cs.utah.edu/teapot/#v2006), and [Cem Yuksel](https://graphics.cs.utah.edu/teapot/#v2026). The first two assume a **flat bottom**. The latest version of the interior by [Cem Yuksel](https://graphics.cs.utah.edu/teapot/#v2026) can work with either a **round** or a **flat bottom**. Note that the interior version of [Hank Driskill\*](https://graphics.cs.utah.edu/teapot/#v1992) here uses the spout interior of [Russ Fish](https://graphics.cs.utah.edu/teapot/#v2006), instead of the original spout interior prepared by [Hank Driskill](https://graphics.cs.utah.edu/teapot/#v1992).
- **Spout Cap:** Utah Teapot without an interior has a hole at the tip of the spout. This option allows putting a cap to cover that hole. This cap can be placed at the **inner** border of the tip or on top of its **outer** surface, where the tip is the highest. The **outer trim** option places an outer cap and also trims the inner portion of the original tip surface, such that the tip smoothly connects to the cap.
- **Lid Cap:** Utah Teapot without an interior has a gap between the lid and the body. This option allows putting a cap to cover this gap. If the teapot is generated without a lid, the lid cap covers the entire upper part of the body.
- **Texture Coordinates:** For texture mapping, traditionally Utah Teapot uses the **patch coordinates** that vary between 0 and 1 within each patch. In this form, each patch of the teapot is mapped over the same texture area and all patches overlap in texture space. To remedy this, and to allow independently texturing different parts of the teapot, we provide three other alternatives:
  - **Patch Coordinates with Offsets:** A different integer offset is added to the patch coordinates of each patch such that the patch maps to a different portion of the texture space. When using tiled textures, the result is the same as using the **patch coordinates** option, but the integer offset allows identifying different patches of the teapot and handling them differently. The visualization here assigns a different color to each patch, using its integer offset.
  - **Injective Layout:** This option uses a carefully-designed layout to map each patch of the teapot to a different part of the texture space:
    - If there is no teapot interior, the body and its bottom use the bottom half of the texture space. The top half is split into four equal portions horizontally to map the handle, the knob of the lid, the remainder of the lid, and the spout. The lid portions are flattened as disks. ![](https://graphics.cs.utah.edu/teapot/images/texc-injective-nointerior-preview.svg)
    - If there is an interior, the texture space is split into four rows. The two bottom rows of the texture space is reserved for the body (one for the interior and and one for the exterior), except for the bottom portions. The bottom portions are flattened as disks and mapped into the top two squares of the first row. The third square is used for the interior of the lid, and the last square is used for the interior of the spout. The second row of the texture space is the same as the top half of version without an interior. ![](https://graphics.cs.utah.edu/teapot/images/texc-injective-withinterior-preview.svg)
  - **Bijective Layout:** This option is similar to the injective layout. The main difference is that the disk-shaped mapping used for some parts in the injective layout is replaced with an alternative mapping that covers the texture area.
    - If there is no teapot interior, the body and its bottom use the bottom half of the texture space. Again, the top half is split into four equal portions, where the first portion hold the handle and the last portion is for the spout. The lid uses the two middle portions, where the lid knob is mapped to the upper part. ![](https://graphics.cs.utah.edu/teapot/images/texc-bijective-nointerior-preview.svg)
    - If there is an interior, the texture space layout changes, similar to the injective layout. The main difference is how the disk-shaped components are mapped to cover square areas. This leads to more distortion, but a better coverage of the texture space. ![](https://graphics.cs.utah.edu/teapot/images/texc-bijective-withinterior-preview.svg)
- **Show Texture Layout:** This option shows a checkerboard texture with markings (shown below) over the teapot surface to indicate how the surface is mapped to the texture space. ![](https://graphics.cs.utah.edu/teapot/textures/grid.png)
- **Match Edge Normals:** The teapot surface has sharp edges where the lid and body connect to their interior parts. If this option is checked, the surface normal along those edges use the average of the two surfaces. This way, the surface normal over the entire teapot becomes continuous.
- **Symmetric Triangulation:** The triangulation of the teapot on its left side mirrors the triangulation of the right side, such that the triangulation on the two sides of the teapot is symmetrical.
- **Flipped Triangulation:** The triangulation of the teapot faces gets flipped.
