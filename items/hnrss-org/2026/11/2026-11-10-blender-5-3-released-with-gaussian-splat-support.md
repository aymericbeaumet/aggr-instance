---
title: Blender 5.3 released with Gaussian Splat support
link: https://www.blender.org/download/releases/5-3/
source: hnrss-org
published: 2026-11-10T16:15:43Z
updated: 2026-11-10T16:15:43Z
first_seen: 2026-11-10T19:38:28.320395710Z
authors:
- amlib
content: extracted
html: 2026-11-10-blender-5-3-released-with-gaussian-splat-support.html
remote_preview:
  url: https://www.blender.org/wp-content/uploads/2026/07/splash.webp
  alt: Gaussian splats, DLSS denoising, light dispersion, EEVEE fisheye, Outliner sorting, ripple video editing, and so much more.
---

Gaussian splats, DLSS denoising, light dispersion, EEVEE fisheye, Outliner sorting, ripple video editing, and so much more.

## Watch the **Recap** Video

[What&apos;s New in Blender 5.3! Official Overview](https://www.youtube.com/watch?v=pvUy69I_Xuk)

New features overview by Jonathan Lampel and Wayne Dixon from [CGCookie](https://www.youtube.com/@cg_cookie).

![4D Gaussian Splatting - Nunchuck](https://www.blender.org/wp-content/uploads/2025/11/gaussian_splatting-nunchuck.jpg)

## **Make a Splat**

Welcome native **3D Gaussian Splats** support to Blender 5.3!

Import them as point clouds, experiment with Geometry Nodes, preview them in Workbench, and render them natively with EEVEE or Cycles.

## **Gaussian** Magic

3D Gaussian Splats are commonly used to reconstruct 3D scenes from photos or videos. They consist of many small, soft, transparent blobs that combine to represent complex geometry and shading.

Blender can now import Gaussian Splats from [PLY](https://docs.blender.org/manual/en/5.3/modeling/geometry_nodes/input/import/ply.html), [SPZ](https://docs.blender.org/manual/en/5.3/modeling/geometry_nodes/input/import/spz.html), and [glTF](https://docs.blender.org/manual/en/5.3/addons/scene_gltf2.html) files, store them as point clouds, process them with Geometry Nodes, and render them in EEVEE, Cycles, and Workbench (solid mode).

PLY file imported as gaussian splats, featuring DOGWALK by Blender Studio.

![Point Cloud Gaussian Splat settings](https://www.blender.org/wp-content/uploads/2026/11/gaussian_splat_settings.webp)

## **Point** of View

Gaussian splats are represented as point clouds, using a new type to tell them apart and with special attributes:

- **scale**: A vector represneting the size of the splat.
- **rotation**: A quaternion to control the rotation of the splat.
- **radiance:base**: 4D Vector as base (DC) radiance of the splats, followed by opacity.
- **radiance:sh\_{index}**: Coefficients of the spherical harmonics bands >= 1. The order is `<L=1 M=-1>`, `<L=1 M=0>`, `<L=1 M=1>`, `<L=2 M=-2>` … where `L` is the band index and `M` is the basis function.

## **Radiant** Future

By default, a 3D Gaussian Splat is rendered using an emissive shader. The radiance attribute provides access to the resolved radiance of the splat at the current shading point.

There is rudimentary support for making the splat relightable by replacing the Emission node with a hard surface BSDF such as Diffuse BSDF.

Performance is still being optimized. Rendering may produce inaccuracies due to differences between the sRGB space used by most Gaussian Splat authoring tools and Blender’s linear color space, particularly in areas with low opacity and high radiance.

![Example of relit Gaussian Splat](https://www.blender.org/wp-content/uploads/2026/11/gaussian_splat_diffuse.webp)\
Simple example of relit Gaussian Splat.

## More About Shading

##### CYCLES

## Divide and **Shine**

The Principled BSDF now supports **Dispersion**!

Turn ordinary glass and diamonds into beautiful little rainbows by splitting white light into its colorful components.

[![](https://www.blender.org/wp-content/uploads/2026/11/cycles_dispersion_christopher3d.avif)](https://www.youtube.com/watch?v=Q9irGPAcUDE)\
Cycles Dispersion overview on YouTube by [Chris Tyler](https://www.youtube.com/watch?v=Q9irGPAcUDE)

[Butterflies specter shader](https://www.youtube.com/shorts/bMrliRB56eM) by [Cartesian Caramel](https://www.youtube.com/@CartesianCaramel)

##### CYCLES

A diamond\
in the **Rough**
---

With Dispersion in the **Transmission** panel, the Principled BSDF can now reproduce the characteristic rainbow effect seen in glass and gemstones. This corresponds to OpenPBR v1.1.1.

**Roughness anisotropy** is also now supported by the Glass BSDF and the transmission component of the Principled BSDF. It behaves just like anisotropy for specular reflections.

![](https://www.blender.org/wp-content/uploads/2026/11/nvidia_dlss_bg-scaled.webp)

##### CYCLES

## Gone with **Denoise**

Blender 5.3 integrates **DLSS 4.5 Ray Reconstruction** as a new denoiser, delivering cleaner images faster by upscaling and reusing samples from previous frames.

Think Cycles looks with EEVEE responsiveness.

[Blender DLSS Ray Reconstruction - NVIDIA Studio Computex 2026](https://www.youtube.com/watch?v=XYvUsBFkJMA)

![Find DLSS denoising in the Denoise panel under Sampling.](https://www.blender.org/wp-content/uploads/2026/11/cycles_dlss_setting.webp)

NVIDIA DLSS brings near-final render quality to the viewport, making lighting and look development faster and more responsive while virtually eliminating flickering between frames during animation playback.

Find it in the Denoise panel under Render properties, Sampling.

**DLSS denoising requires NVIDIA driver version 620 or newer.**

![NVIDIA RTX Spark](https://www.blender.org/wp-content/uploads/2026/11/nvidia_rtx_spark_laptops-2048x616.webp)

Ready to **Spark**

Blender 5.3 brings native support for **NVIDIA RTX Spark** devices running Windows 11, including memory optimizations.

##### EEVEE

## Lights Meet **Material Nodes**

**Material Lighting nodes** allow authoring how the Material reacts to Lights.

This can, for example, be used for toon and stylized shading, but also for complex physical effects like iridescence.

![](https://www.blender.org/wp-content/uploads/2026/11/material_lighting_nodes.webp)\
All the nodes you need to adjust lighting to your liking.

## **Lighting** to Your Liking

Per-light data, evaluation, and shadow information are now available in the Shader Editor, giving you more control over how materials respond to individual scene lights.

Four new nodes and two that received a “Light” mode/space:

- **[Light Accumulation](https://docs.blender.org/manual/en/5.3/render/shader_nodes/shader/light_accumulation.html)**: Evaluates its inputs for each light in the scene and accumulates the results.
- **[Light Info](https://docs.blender.org/manual/en/5.3/render/shader_nodes/input/light_info.html#bpy-types-shadernodelightinfo)**: Exposes light data properties (Color, Power, and Position).
- **[Light Evaluation](https://docs.blender.org/manual/en/5.3/render/shader_nodes/input/light_evaluation.html#bpy-types-shadernodelightevaluation)**: Evaluates position-dependent light properties (Diffuse/Specular Term, Attenuation Mask, Direction, and Distance).
- **[Shadow Raycast](https://docs.blender.org/manual/en/5.3/render/shader_nodes/input/shadow_raycast.html#bpy-types-shadernodeshadowraycast)**: Outputs a shadow mask value for the light being shaded.
- [**Attribute** (Light Mode)](https://docs.blender.org/manual/en/5.3/render/shader_nodes/input/attribute.html#bpy-types-shadernodeattribute): Queries custom properties from the Light Object or Light Data being evaluated.
- [**Vector Transform** (Light Space)](https://docs.blender.org/manual/en/5.3/render/shader_nodes/utilities/vector/transform.html#bpy-types-shadernodevectortransform): Transforms coordinates to and from the local space of the light being evaluated.

## A **Wider** Perspective

EEVEE can now render panoramic cameras, supporting all types of projections, from equirectangular and cubemap formats to various fisheye models and mirror ball.

EEVEE fisheye matching Cycles 1:1

The current implementation has a few limitations: depth of field, raytracing, and volumetric temporal reprojection in the viewport are not yet supported.

## More in **EEVEE & Viewport**

![](https://www.blender.org/wp-content/uploads/2025/07/sprite_fright_motacilla_flavai-scaled.webp)

##### COMPOSITOR

## Side **Effects**

You can now stack **Effects** on your scene just like you would modifiers or constraints.

Applying compositing effects has never been easier.

Scene Effects stack featuring [Storm](https://studio.blender.org/characters/storm/v1/) by Blender Studio

![](https://www.blender.org/wp-content/uploads/2026/11/compositor_scene_stack_bloom.webp)\
New **Compositor & Effects** tab in the Properties editor.

## One-click, **Bloom**!

Choose from built-in effects such as [Bloom](https://docs.blender.org/manual/en/5.3/compositing/types/camera_lens_effects/bloom.html), [Chromatic Aberration](https://docs.blender.org/manual/en/5.3/compositing/types/camera_lens_effects/chromatic_aberration.html), [Film Grain](https://docs.blender.org/manual/en/5.3/compositing/types/camera_lens_effects/film_grain.html), and [Vignette](https://docs.blender.org/manual/en/5.3/compositing/types/camera_lens_effects/vignette.html), as well as effects from online asset libraries or your own custom compositor node groups.

Internally, each effect is its own node group, with its output passed on to the next effect.

You can further customize effects at any time in the Compositor editor.

![](https://www.blender.org/wp-content/uploads/2026/11/compositor_new_nodes_blender_53-scaled.webp)

##### COMPOSITOR

## **Bundle** Up

**17 Geometry Nodes** are now supported in the Compositor, bringing a range of new capabilities across **[bundles](https://docs.blender.org/manual/en/5.3/compositing/types/utilities/bundle/index.html)**, math and logic, and utility operations.

This includes bundle support with **Get Bundle Item**, **Store Bundle Item**, **Combine Bundle**, **Typed Bundle**, and **Separate Bundle**.

Math and logic nodes such as **Boolean Math**, **Compare**, **Float to Integer**, **Integer Math**, **Matrix SVD**, **Combine Transform**, and **Separate Transform**. Plus utility nodes including **Hash Value**, **Random Value**, **Is Viewport**, **Import Text**, and **Bone Info**.

- Blender 5.2
- Blender 5.3

- CPU

- NVIDIA GPU

- Apple GPU

- Relative time to process Anisotropic Kuwahara filter. Lower is better.

## More in **Compositor**

![Blender 5.3 Splash by Blender Studio](https://www.blender.org/wp-content/uploads/2026/07/splash-overgrown.webp)

## Study the **artwork**

Blender splash artwork source files are available for you to play with!

![Blender 5.3 Splash by Blender Studio](https://www.blender.org/wp-content/uploads/2026/07/splash.webp)\
OVERGROWN, by Blender Studio

![File Project menu](https://www.blender.org/wp-content/uploads/2026/11/projects_menu_file.webp)\
The new **File** → **Project** menu, to create, open and access projects.

##### CORE

## Blender, meet **projects**.

Until now, Blender’s preferences (such as asset libraries) were shared across all projects, and it was up to users to keep related files and resources organized.

Blender now supports projects, giving you a way to group related files, share resources, and manage settings across an entire project, whether you’re working alone or as part of a team.

Everything\
Everywhere\
All In a Project
---

Projects support custom variables that can be used throughout a project, such as a character’s name or a frequently used file path.

Per-project asset libraries and color management configurations are also supported. You can even set up a custom splash screen for your project, providing quick access to its files and other recent projects.

Add-ons and other extensions per-project will follow in future Blender versions.

Projects are entirely optional, most of the time you’ll probably work without one.

![Project Splash](https://www.blender.org/wp-content/uploads/2026/11/projects_splash_blender53-1.webp)\
Project-specific splash and color management configuration.

![Charge Open Movie](https://www.blender.org/wp-content/uploads/2023/02/background_charge_workbench.jpg)

##### ASSETS

## Asset Libraries ♥ Extensions

Asset libraries can now be shared as extensions! Just like add-ons or themes.

Browse hundreds of public domain assets available on the Extensions platform via the **Get Extensions** tab and install them directly from within Blender.

Create and share your own asset library extension. Optionally, access can be controlled via [authentication tokens](https://docs.blender.org/manual/en/5.3/files/asset_libraries/remote_asset_libraries.html#asset-library-access-token).

![Extension Asset Libraries](https://www.blender.org/wp-content/uploads/2026/11/get_extensions_asset_libraries_asset-2027x2048.webp)\
A selection of public domain assets by the popular library Poly Haven is now available from within Blender. No add-ons required.

##### GREASE PENCIL

## Drawn to **Select**

Draw Mode now has its own selection tools: **Tweak**, **Lasso**, **Box**, and **Circle**. Select strokes and adjust them right where you draw.

Move, rotate, scale, duplicate, copy, paste, and delete strokes using the usual shortcuts, or access all these actions from the new Stroke menu.

## **Carve** Diem

Meet the new **Carver** tool in Draw Mode.

It cuts shapes using proper **Boolean** subtraction, creating clean cuts in strokes and fills based on a lasso selection. Holes are created automatically when needed.

When a shape has both a stroke and a fill, carving a hole into it also creates a stroke around the hole. Fills that are split apart become separate shapes, making them easy to sculpt further.

The new Carver tool uses booleans to cut through strokes and fills.

Fill with color multiple regions at once using the new Fill tool in Blender 5.3.

## One Tool to **Fill** Them All

The Fill tool can now fill multiple regions at once.

Simply draw a path across multiple regions to fill them all as a single shape, with no gaps or borders in between.

## **Ramp** It Up

Gradient fills now use a ColorRamp input instead of being limited to just two colors.

![](https://www.blender.org/wp-content/uploads/2026/11/greasepencil_gradient_53.webp)

Assign **Flip View** a shortcut or add it to Quick Favorites for easy access.

## **Flip** Out

Mirror the camera view horizontally with the new **Flip View** operator to evaluate the composition from a different perspective and spot asymmetry or visual imbalances.

This only affects the viewport display. It does not change the camera object’s transform or the final render, which remains unchanged. Use the operator again to restore the original orientation.

Flip View has no shortcut assigned by default, but you can assign your own through the right-click menu or add it to Quick Favorites (Q) for easy access.

##### I/O

## USD Collection Importer

Collections can now be associated to **importers**.

Similar to the [Collection Exporters](https://developer.blender.org/docs/release_notes/4.2/pipeline_assets_io/#collection-exporters) introduced in Blender 4.2 LTS, Collection Importers make it easy to re-import data with a single click, without having to navigate menus or adjust settings repeatedly.

Configure them through the **Importers** panel under **Collection** Properties. Settings are stored in .blend files, making them easy to share and preserving them across Blender sessions.

Only the USD importer is supported in Blender 5.3.

See all [**USD**, **Alembic**, **glTF**, **OpenEXR**, and other I/O](https://developer.blender.org/docs/release_notes/5.3/pipeline_io/#changelog) changes.

![](https://www.blender.org/wp-content/uploads/2026/11/collection_importer_ui-scaled.webp)

##### VIDEO SEQUENCER

## **Cut** to the chase

The Video Sequencer gets a full **ripple editing** framework, a rewritten **blade** tool, and a long list of editing, interface, and performance improvements, including initial **OpenTimelineIO** support.

Ripple,\
Shuffle,\
Overwrite
---

Welcome **Ripple** editing to Blender 5.3!

This new overlap mode automatically moves strips forward or backward as edits are made, maintaining timeline continuity without the need to manually close gaps.

**Ripple**, **Shuffle**, and **Overwrite** modes are now consistently used across editing operations. Switch between them while moving strips using R, S, and V.

![](https://www.blender.org/wp-content/uploads/2026/11/vse-ripple-options.webp)

![](https://www.blender.org/wp-content/uploads/2026/11/vse-ripple-delete.webp)

## Making Waves

Complete the ripple workflow with these handy operators:

**[Ripple Add](https://docs.blender.org/manual/en/5.3/video_editing/edit/montage/editing.html#ripple-settings)**: Hold Shift while dragging and dropping strips to insert them using ripple behavior, automatically making room in the timeline.

**[Ripple Trim](https://docs.blender.org/manual/en/5.3/video_editing/edit/montage/editing.html#ripple-trim)**: Trim selected strips at the playhead and automatically close the resulting gaps. Use Shift+Q or Shift+W to trim from either side.

**[Ripple Delete](https://docs.blender.org/manual/en/5.3/video_editing/edit/montage/editing.html#ripple-delete)**: Delete selected strips and move subsequent content back to close the gaps. Use Shift+X.

## **Cutting** Edge: Blade 2.0

Splits are now visualized as a line when using **Blade** and as a region when using **Box Blade**. Tooltips display frame values at the split location, while a real-time preview shows the frame beneath the cursor.

Press and hold C in the timeline to temporarily activate the Blade tool.\
Release it to switch back to the previously active tool.

But wait, there’s more: **Hardware decoding**, **thumbnail** support for **Scene** and other strips, **OpenTimelineIO** import and export, and much more.

##### USER INTERFACE

## Search. Sort. Scribble.

Blender 5.3 gets more than a layer of polish. Long-awaited quality of life features and improvements that make this release take a leap in usability.

## **Sorted** Out

Arrange your objects freely with **custom sorting** in the Outliner. Finally.

Switch back to alphabetical order from the header popover.

Plus improvements to filtering, drag-and-drop behavior, and hierarchy management.

Place objects anywhere you want in the Outliner.

![](https://www.blender.org/wp-content/uploads/2026/11/ui_sidebar_search.webp)\
Press Ctrl/Cmd + F to find anything in the sidebar.

## **Side** Quest

Sidebars in all editors now support **search**, making it possible to find any property or operator, even inside tabs created by add-ons.

Press Ctrl/Cmd + F while hovering the sidebar and start typing right away, or through the context menu of the tabs and panels.

To clear the search, press Alt/Option + F.

## **Mark** it **Down**

The Blender UI now supports rendering basic [Markdown](https://en.wikipedia.org/wiki/Markdown) formatting.

In node editors, write markdown directly in the new [Comment](https://docs.blender.org/manual/en/5.3/interface/controls/nodes/types/layout/index.html) node, or in regular layouts for scripts/add-ons through the [`label_markdown`](https://docs.blender.org/api/5.3/bpy.types.UILayout.html#bpy.types.UILayout.label_markdown) widget. Another handy widget for long strings is [`label_multiline`](https://docs.blender.org/api/5.3/bpy.types.UILayout.html#bpy.types.UILayout.label_multiline), it behaves just like labels but allows text to wrap.

![Markdown comment node](https://www.blender.org/wp-content/uploads/2026/11/ui_node_comment.webp)\
New **[Comment](https://docs.blender.org/manual/en/5.3/interface/controls/nodes/types/layout/comment.html)** node to annotate node trees, with Markdown support. Rendered node on the left, Edit mode on the right.

But wait: **IME support**, improved **tooltips**, **box-select** gesture, browsable node editor **context paths**, better **Attribute Viewer** overlay, **Curve** widget selection, and more!

![](https://www.blender.org/wp-content/uploads/2026/11/roadmap_bg_titl.webp)

## What’s **Next**?

Blender on tablets, texture layers, animation layers? Show your support for upcoming development projects and help shape the future of Blender.

## BUT WAIT, **THERE’S** **MORE**

Core

- [Improved **Undo**.](https://projects.blender.org/blender/blender/commit/69f5d770c4f17b1688af1f9dceb12bb84d1591af)
- [**Linking & Appending** improvements.](https://developer.blender.org/docs/release_notes/5.3/core/#linked-packed-ids)
- [See all **Core** changes.](https://developer.blender.org/docs/release_notes/5.3/core/)

Animation

- [**Copy Paste World Space**](https://developer.blender.org/docs/release_notes/5.3/animation_rigging/#highlights)
- [**Convert Rotation Mode**](https://developer.blender.org/docs/release_notes/5.3/animation_rigging/#highlights)
- [Improved **Playback**.](https://developer.blender.org/docs/release_notes/5.3/animation_rigging/#playback)
- [**Pose** changes.](https://developer.blender.org/docs/release_notes/5.3/animation_rigging/#pose)
- [**Rigify** B-Bone segments.](https://developer.blender.org/docs/release_notes/5.3/animation_rigging/#rigify)
- [Batch rename **Timeline Markers**.](https://developer.blender.org/docs/release_notes/5.3/core/#linking-appending)
- [See all **Animation & Rigging** changes.](https://developer.blender.org/docs/release_notes/5.3/animation_rigging/)

Python API

- [Access to all and selected **markers**.](https://projects.blender.org/blender/blender/commit/3048200244b8acb082c5399bd8b2aab8170035dc)
- [**OpenEXR** and **OpenTimelineIO** modules.](https://developer.blender.org/docs/release_notes/5.3/python_api/#built-in-python-modules)
- [Better **color space interoperability**.](https://developer.blender.org/docs/release_notes/5.3/python_api/#interoperability)
- [See all **Python API** changes.](https://developer.blender.org/docs/release_notes/5.3/python_api/)

Plus hundreds of bug fixes, code cleanups and refactors.\
See the [full list of changes](https://developer.blender.org/docs/release_notes/5.3/).

## CREDITS
