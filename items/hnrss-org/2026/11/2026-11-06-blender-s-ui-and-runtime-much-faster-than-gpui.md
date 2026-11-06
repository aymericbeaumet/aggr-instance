---
title: 'Blender''s UI and Runtime: Much Faster Than GPUI'
link: https://github.com/ppmpreetham/BUI
source: hnrss-org
published: 2026-11-06T07:40:35Z
updated: 2026-11-06T07:40:35Z
first_seen: 2026-11-06T11:27:16.888185679Z
authors:
- ppmpreetham
content: extracted
html: 2026-11-06-blender-s-ui-and-runtime-much-faster-than-gpui.html
remote_preview:
  url: https://opengraph.githubassets.com/22fd4edd1c2fe482e2ad8356b04d4aaa9c8e314740fb26ebea68a85274e1b5e0/ppmpreetham/BUI
  alt: Blender's GPU Accelerated UI, Decoupled from it's original codebase. Supports Rust, Python, JS, React, C & C++. Twice as good as GPUI - ppmpreetham/BUI
---

![BUI Logo](https://raw.githubusercontent.com/ppmpreetham/BUI/main/public/Logo.png)

**Blender's user interface, as a library you can use from Python, Rust, JavaScript (React, Svelte), C, C++ and Dear ImGui.**

People spent decades building and optimizing Blender's GUI. It's battery and memory efficient, and super duper fast. We decoupled it, to give everyone a chance to build their own products with it. It supports multiple languages and frameworks, so you won't miss a beat!

[![PyPI](https://camo.githubusercontent.com/908fdc08319d3b69122931ba4639590e193b497e023917e9622191f2071ba4ad/68747470733a2f2f696d672e736869656c64732e696f2f707970692f762f626c656e64657275693f6c6162656c3d70797069253230626c656e6465727569)](https://pypi.org/project/blenderui/) [![Python](https://camo.githubusercontent.com/128768a4f09ec073ab7df23dd275907aa43a2f415714a59ca10c22a58aa3c93d/68747470733a2f2f696d672e736869656c64732e696f2f707970692f707976657273696f6e732f626c656e6465727569)](https://pypi.org/project/blenderui/) [![crates.io](https://camo.githubusercontent.com/a3a1be0ad3323a1dc977961d5bc59155d93d6885b155a292c384b22096c00496/68747470733a2f2f696d672e736869656c64732e696f2f6372617465732f762f626c656e64657275693f6c6162656c3d6372617465732e696f253230626c656e6465727569)](https://crates.io/crates/blenderui) [![@blender-ui/react](https://camo.githubusercontent.com/1a9c735a548a2f39ecde5210fda6bffb8a90c9f4db75fa859769a6c6adec9064/68747470733a2f2f696d672e736869656c64732e696f2f6e706d2f762f40626c656e6465722d75692f72656163743f6c6162656c3d253430626c656e6465722d75692532467265616374)](https://www.npmjs.com/package/@blender-ui/react) [![Wheels](https://github.com/ppmpreetham/BUI/actions/workflows/wheels.yml/badge.svg)](https://github.com/ppmpreetham/BUI/actions/workflows/wheels.yml) [![License: GPL-2.0-or-later](https://camo.githubusercontent.com/cd781d0568c328bdea35f284d33e2b1e96675ef60910926c1dd75d5a4d46de66/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f6c6963656e73652d47504c2d2d322e302d2d6f722d2d6c617465722d626c7565)](https://github.com/ppmpreetham/BUI#43-license-and-credits) [![Platforms](https://camo.githubusercontent.com/8d97c48292430107ef43a7c3cd20c4cea0d683c37f67ba8e30d37994ef10be88/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f706c6174666f726d732d57696e646f77732532302537432532304c696e75782532302537432532306d61634f532d6c6967687467726579)](https://camo.githubusercontent.com/8d97c48292430107ef43a7c3cd20c4cea0d683c37f67ba8e30d37994ef10be88/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f706c6174666f726d732d57696e646f77732532302537432532304c696e75782532302537432532306d61634f532d6c6967687467726579) [![Discord](https://camo.githubusercontent.com/85a0af93c0f1006e0d11ac9fe29ed37b57cf27812f7a3027b5902d6e6cf4f328/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f636861742d446973636f72642d3538363546323f6c6f676f3d646973636f7264266c6f676f436f6c6f723d7768697465)](https://discord.gg/MWXZfdyM3D)

**Website and full documentation: [bui.preetham.top](https://bui.preetham.top)** · **Chat: [Discord](https://discord.gg/MWXZfdyM3D)**

[![BUI's Blender components: panels, number sliders, vector fields, checkboxes and a color swatch](https://raw.githubusercontent.com/ppmpreetham/BUI/main/docs/images/components.png)](https://raw.githubusercontent.com/ppmpreetham/BUI/main/docs/images/components.png) [![An unmodified tkinter program running on BUI](https://raw.githubusercontent.com/ppmpreetham/BUI/main/docs/images/tkinter_items.png)](https://raw.githubusercontent.com/ppmpreetham/BUI/main/docs/images/tkinter_items.png)

This README is a small book. Read it front to back for the whole story, or jump to the chapter you need. Every chapter links to a longer guide in [`docs/`](https://github.com/ppmpreetham/BUI/blob/main/docs) or on the [website](https://bui.preetham.top) when there's more to say.

## Contents

**[Part I: Welcome](https://github.com/ppmpreetham/BUI#part-i-welcome)**

1. [What BUI is](https://github.com/ppmpreetham/BUI#1-what-bui-is)
2. [Why BUI exists](https://github.com/ppmpreetham/BUI#2-why-bui-exists)
3. [Who it's for](https://github.com/ppmpreetham/BUI#3-who-its-for)
4. [The many ways in](https://github.com/ppmpreetham/BUI#4-the-many-ways-in)
5. [Features at a glance](https://github.com/ppmpreetham/BUI#5-features-at-a-glance)
6. [A gallery](https://github.com/ppmpreetham/BUI#6-a-gallery)

**[Part II: Getting started](https://github.com/ppmpreetham/BUI#part-ii-getting-started)**

7.  [Install](https://github.com/ppmpreetham/BUI#7-install)
8.  [First app: tkinter](https://github.com/ppmpreetham/BUI#8-first-app-tkinter)
9.  [First app: Blender components](https://github.com/ppmpreetham/BUI#9-first-app-blender-components)
10. [First app: Qt syntax](https://github.com/ppmpreetham/BUI#10-first-app-qt-syntax)
11. [First app: Rust, in rsx and gpui builders](https://github.com/ppmpreetham/BUI#11-first-app-rust-in-rsx-and-gpui-builders)
12. [First app: JavaScript and React](https://github.com/ppmpreetham/BUI#12-first-app-javascript-and-react)
13. [First app: Dear ImGui](https://github.com/ppmpreetham/BUI#13-first-app-dear-imgui)
14. [First app: C](https://github.com/ppmpreetham/BUI#14-first-app-c)

**[Part III: Concepts](https://github.com/ppmpreetham/BUI#part-iii-concepts)**

15. [How it works](https://github.com/ppmpreetham/BUI#15-how-it-works)
16. [Blender's widgets and interactions](https://github.com/ppmpreetham/BUI#16-blenders-widgets-and-interactions)
17. [Three looks: HTML, styled and Blender components](https://github.com/ppmpreetham/BUI#17-three-looks-html-styled-and-blender-components)
18. [Rendering on the CPU and the GPU](https://github.com/ppmpreetham/BUI#18-rendering-on-the-cpu-and-the-gpu)
19. [Damage rendering](https://github.com/ppmpreetham/BUI#19-damage-rendering)
20. [Window hosts](https://github.com/ppmpreetham/BUI#20-window-hosts)
21. [Text and fonts](https://github.com/ppmpreetham/BUI#21-text-and-fonts)
22. [Themes: dark and light](https://github.com/ppmpreetham/BUI#22-themes-dark-and-light)

**[Part IV: Guides by language](https://github.com/ppmpreetham/BUI#part-iv-guides-by-language)**

23. [Python](https://github.com/ppmpreetham/BUI#23-python)
24. [Rust](https://github.com/ppmpreetham/BUI#24-rust)
25. [JavaScript and React](https://github.com/ppmpreetham/BUI#25-javascript-and-react)
26. [Dear ImGui](https://github.com/ppmpreetham/BUI#26-dear-imgui)
27. [C and embedding](https://github.com/ppmpreetham/BUI#27-c-and-embedding)
28. [Where the docs live](https://github.com/ppmpreetham/BUI#28-where-the-docs-live)

**[Part V: Performance](https://github.com/ppmpreetham/BUI#part-v-performance)**

29. [Before you read the numbers](https://github.com/ppmpreetham/BUI#29-before-you-read-the-numbers)
30. [Start time and battery](https://github.com/ppmpreetham/BUI#30-start-time-and-battery)
31. [Frame times](https://github.com/ppmpreetham/BUI#31-frame-times)
32. [JavaScript and React: startup and animation](https://github.com/ppmpreetham/BUI#32-javascript-and-react-startup-and-animation)
33. [Run the benchmarks yourself](https://github.com/ppmpreetham/BUI#33-run-the-benchmarks-yourself)

**[Part VI: Reference](https://github.com/ppmpreetham/BUI#part-vi-reference)**

34. [Platforms](https://github.com/ppmpreetham/BUI#34-platforms)
35. [Environment variables](https://github.com/ppmpreetham/BUI#35-environment-variables)
36. [Building from source and CMake options](https://github.com/ppmpreetham/BUI#36-building-from-source-and-cmake-options)
37. [Coverage](https://github.com/ppmpreetham/BUI#37-coverage)
38. [Repository layout](https://github.com/ppmpreetham/BUI#38-repository-layout)
39. [Examples](https://github.com/ppmpreetham/BUI#39-examples)

**[Part VII: The project](https://github.com/ppmpreetham/BUI#part-vii-the-project)**

40. [Status and roadmap](https://github.com/ppmpreetham/BUI#40-status-and-roadmap)
41. [FAQ](https://github.com/ppmpreetham/BUI#41-faq)
42. [Contributing and publishing](https://github.com/ppmpreetham/BUI#42-contributing-and-publishing)
43. [License and credits](https://github.com/ppmpreetham/BUI#43-license-and-credits)

* * *

## Part I: Welcome

## 1\. What BUI is

BUI takes the user interface of [Blender](https://www.blender.org) (its widget drawing, its interaction rules and its look) and turns it into a standalone UI library with a small C core. Your code builds the widget tree and handles callbacks. Layout, input, drawing and presenting all happen in fast native code, on the CPU or the GPU.

At the heart of BUI is the **kit**: a small runtime written in C. The kit holds every widget in one tree, lays them out, handles the mouse and keyboard, and draws them the way Blender does. The drawing code is a C port of Blender's own widget code, and the interaction follows Blender's rules. On top of the kit sit **bindings**: thin layers that let Python, Rust, JavaScript, C++ and Dear ImGui talk to it.

What BUI is not:

- **Not Blender.** It contains the parts of Blender's UI code it needs and builds on its own. You don't need Blender installed, and you don't need to know Blender to use it.
- **Not a browser.** The Rust and JavaScript sides use HTML and CSS as a *UI language*, but there's no browser engine, no webview and no network.
- **Not a re-imagining.** The widget drawing is ported from Blender's `interface_widgets.cc`, and the Dear ImGui support runs the real Dear ImGui.

## 2\. Why BUI exists

Blender's interface is famous for how it *feels*. You press a number field and drag: the cursor vanishes, the value glides under your hand, and you never hit the edge of the screen. You type `2*pi` into a field and it just works. You type "ad cu" into a menu and it finds "Add Cube". Millions of artists have these habits in their hands.

That interface is also fast and frugal. It redraws only what changed, sleeps when nothing does, and starts in milliseconds. BUI exists so that any app, in any language, can have both: Blender's feel, and Blender's efficiency.

## 3\. Who it's for

- **You have a tkinter app** and wish it looked less like 1998. Run it on BUI, unchanged.
- **You're building a tool for artists** who already know Blender. Give them the panels, sliders and shortcuts their hands expect.
- **You write Rust or React** and want a native desktop UI without shipping a browser.
- **You use Dear ImGui** and want it to look and feel like Blender, with one build flag.
- **You're curious** how a UI toolkit works. The source is readable, and the [concepts](https://github.com/ppmpreetham/BUI#part-iii-concepts) are explained in plain words.

## 4\. The many ways in

You can drive BUI many ways, and all of them share the same C runtime, so they all get the same widgets and interactions:

| You write                                                                                                                                                  | You get                                                                                                                                         | Status                |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- | --------------------- |
| **tkinter** code, even existing programs, unchanged                                                                                                        | Blender-styled apps; every tkinter and ttk API                                                                                                  | Complete              |
| **Qt-style** Python (`QWidget`, layouts, `clicked.connect`)                                                                                                | The common subset of Qt widgets                                                                                                                 | Partial               |
| **Blender components** (`Panel`, `NumberSlider`, `PieMenu`, the node editor ...)                                                                           | 45 native Blender widgets and editors, from Python; ten of them also from Rust and JavaScript                                                   | Complete              |
| **Rust**: HTML + CSS + Tailwind, HTML-like markup (`rsx!` / `view!`), hooks, or [gpui](https://www.gpui.rs) / [zopra](https://zopra.preetham.top) syntax | A web-like framework: HTML that looks like a browser's, Blender's look with `styled`, and Blender components (`<NumberSlider>`, `<Button>` ...) | Working               |
| **JavaScript / React / Svelte** on Node.js or Bun: a DOM-like API, JSX or Svelte components                                                                | The same engine and components in native windows, over napi-rs; React's hooks backed by signals; Svelte 5 components                            | Working               |
| **Dear ImGui** from C++, C, Python or Rust                                                                                                                 | The real Dear ImGui on BUI windows in Blender's theme, or a build where ImGui's widgets *are* Blender's widgets                                 | Working               |
| **C** (`BUI_kit.h`)                                                                                                                                        | The kit itself: nodes, widgets, windows                                                                                                         | Working               |
| **MyGo**                                                                                                                                                   | MyGo                                                                                                                                            | Not yet (coming soon) |

Not sure which to pick? If you already have code, use the row that matches it. If you're starting fresh in Python, the Blender components are the friendliest.

## 5\. Features at a glance

### Blender's widgets and interactions

- Number fields you **drag with a hidden, continuous cursor**: Shift for precision, Ctrl to snap.
- **Math in number fields**, with Blender's own expression evaluator: `2*pi`, `min(4, 3)`, `round(1.7)`, `x if c else y`.
- Ctrl+Wheel steps values, Ctrl+C / Ctrl+V copy values, Backspace resets.
- **Drag across checkboxes** to set them all.
- Press-drag-release menus, pie menus, and **Blender's fuzzy menu search**: "ad cu" finds "Add Cube", typos are forgiven, recent picks rank first.
- **Ctrl+Z / Ctrl+Shift+Z** on every UI edit, and inside a text field while you type; Tab between fields.
- Blender's float display precision, multi-field vertical drags, and panels that collapse with Blender's animation.
- The HSV color wheel, color ramps, curves, trees, lists, text areas, canvases, a node editor and Blender's File Browser.
- **Blender's Dark and Light themes**, switchable at run time.

### Python, four ways

- **tkinter**: all 7,846 public names of tkinter and ttk. Nine real programs behave identically on the standard library and on BUI.
- **Qt syntax**: widgets, layouts, signals, `QPainter`, menus and timers.
- **Components**: 45 Blender widgets, editors and templates.
- **Dear ImGui**: `from blenderui import imgui`, pyimgui-style.
- One `pip install`, no Qt or Tk widgets drawing anything.

### Rust: the web platform, natively

- **All 113 HTML elements**, drawn like a browser draws them; `styled` gives any subtree Blender's look, and **Blender components** (`<NumberSlider>`, `<Button>`, `<ColorSwatch>` ...) are real Blender widgets.
- **Modern CSS**: flexbox, grid with named areas, tables, `calc()` and every math function, `var()`, `oklch()` and `color-mix()`, `:has()`, nesting, `@layer`, `@media` ranges, `@container`, `::before`/`::after`, counters.
- **Effects and motion**: `@keyframes` animations and transitions, filters, `backdrop-filter`, blend modes, `clip-path`, masks, 2D transforms, `border-image`.
- **Tailwind CSS v4**: `class="md:hover:bg-sky-500/50"` or bare attributes (`<div p-4 bg-[#4772b3]>`), with **"did you mean" compile errors**.
- **`rsx!` / `view!`** HTML-like markup, **`#[component]`**, and **hooks** in zopra, React or Leptos style (`use_state`, `signal`, `Memo`, `Effect`, context).
- **gpui-compatible syntax** too: `div().flex().gap_2()`, `Render`, `cx.listener`.
- On [crates.io](https://crates.io/crates/blenderui): `cargo add blenderui`.

### Fast by design

- **Damage rendering**: an idle UI costs 0% CPU, and a hover redraws only the widget it touches.
- **Opt-in GPU renderer** (Direct3D 11 on Windows, OpenGL 3.3 on Linux), modeled on Blender's instanced widget drawing. It draws the same pixels as the CPU raster.
- In the benchmark, **2–4× less CPU per frame than gpui-fast**, with **less RAM** and a **\~5 ms first frame**.
- **Blender's text**: Inter (Blender's UI font) is embedded and rendered with FreeType the way Blender renders it, so text looks the same on every platform. On the GPU, glyphs are drawn from their outlines with [Slug](https://github.com/EricLengyel/Slug), falling back to a per-glyph atlas like Blender's.

### Images

- **One decoder set for every language**, in C: PNG, GIF, BMP, TGA, QOI ([Wuffs](https://github.com/google/wuffs), memory-safe), JPEG (libjpeg-turbo), WebP including animated (libwebp), Radiance HDR, PNM and SVG. The design is Blender's imbuf: one image type and a format table.
- **Decoded at the size it is drawn, off the UI thread**: JPEG thumbnails use DCT scaling (a 4K photo becomes a 256 px thumbnail in ~12 ms), visible images first, scrolled-away requests cancelled, and only the image's rectangle redraws when it arrives. The page never jumps: sizes come from the file header.
- **One memory-budgeted cache** (256 MiB by default). Scrolling 1,000 photo thumbnails: **2.9 ms p99 frames and 100 MB**, against 96 ms and 2.5 GB when decoding on the UI thread; **0 allocations** in a settled frame, every block freed at exit. [Images](https://bui.preetham.top/docs/concepts/images/).
- **Animated GIF and WebP play** everywhere an image shows, decoded one frame at a time at the size drawn (two frames held per animation), each frame for its own delay, only the image's rectangle repainting. Off screen, scrolled away or in a hidden window they pause: 200 animations at once take **0.76 ms p50 / 1.5 ms p99** render frames, and **nothing at all** once off screen. `frame` holds one frame; reduced motion holds them all.
- **Large images on the GPU**: images over 2047 px get a texture of their own on Direct3D 11 and OpenGL (tiled only past the device limit), freed with the image.
- **Video**: `<video>` in every language, decoded by the OS (Media Foundation with DXVA, GStreamer, AVFoundation; loaded at run time) at the size drawn, frames in one GPU texture per player updated in place, YCbCr converted in the shader, and **nothing decoded off screen**. 1080p plays at **0.99 ms** render frames and **9 %** of a core on the GPU renderer, five at once at 28 %, with **0 allocations per frame**. Play, pause, seek, rate, loop, poster, state events, and Blender-style controls. [Video](https://bui.preetham.top/docs/concepts/video/).
- `<img>`, CSS `background-image`, `object-fit`, Blender `Image` components, tkinter `PhotoImage` (now also JPEG, WebP, BMP, TGA, QOI, HDR, SVG), canvas items, data-table image columns, chat-list media and Dear ImGui textures (animated ones too).

### JavaScript and React

- **React 19.3** renders to BUI: HTML host components, CSS and Tailwind, events, Suspense, transitions, actions, and Blender components (`<NumberSlider>`, `<Button>` ...). BUI draws them, on the CPU or the GPU; there's no browser and no webview.
- **React's hooks, by React's names**, holding signals: pass one into JSX (`{count}`) and a change updates that one node without re-rendering; read it while rendering (`count.value`) and the component re-renders, as with React. A conformance suite checks each hook against React's own.
- **Svelte 5** (`@blender-ui/svelte`): components with runes, snippets, bindings and scoped styles, mounted into native windows; Svelte's compiled code runs on BUI's DOM, so a `$state` change updates the one node that shows it.
- **`@blender-ui/native`**: a DOM-like API (`createElement`, `appendChild`, `addEventListener`) with the same signals, for apps without a framework, and the shared layer the React and Svelte bindings are thin adapters over.
- **Node.js and Bun**, through a napi-rs addon, with headless mode for tests.

### Small standalone apps

- **`bui build app.jsx`** produces one executable of **6.3 to 6.9 MB** (by the features it uses): BUI with an embedded QuickJS engine.
- **First frame in ~70 ms**, at **19 MB** of RAM, with nothing to install. There's no browser and no webview.
- **`--bun`** builds a Bun executable instead, with Node's APIs and a JIT.
- Builds take under a second: the app's bundle is appended to a prebuilt runtime.
- **Pay for what you use**: one feature table (`features.json`) drives CMake options, cargo features and `bui build`, which links the smallest runtime an app needs: **6.3 MB** for hello world, 6.9 MB with images or video. [Binary size](https://bui.preetham.top/docs/reference/binary-size/).

### Dear ImGui

- **The real Dear ImGui 1.93**: every function of `imgui.h` works, including `ShowDemoWindow()`.
- **Four languages**: C++ (drop-in `imgui.h`), C (`cimgui.h`, all 399 functions), Python and Rust (generated from `imgui.h`).
- **Blender's look**: Blender's theme colors and metrics with Inter; or, with `BUI_IMGUI_BLENDER_WIDGETS=ON`, Blender's actual widgets and interaction, pixel-identical to BUI's own.
- **Idle costs nothing**: frames run only on input and while something animates.

### Native everywhere

- **Native windows** on Windows (Win32) and Linux (X11, Wayland through XWayland), and an experimental Cocoa host for macOS ported from Blender's GHOST.
- **HiDPI**: per-monitor DPI on Windows, `Xft.dpi` on Linux, Retina on macOS.
- **Headless mode** for tests and screenshots, with a virtual clock that steps animations exactly.
- On Linux, libX11 and libGL are loaded at run time, so BUI imports on servers without them.

## 6\. A gallery

Every screenshot here is a real BUI window, rendered off-screen. Where an image comes in a dark and a light version, GitHub shows the one that matches your theme.

![A Geometry Nodes editor in BUI: seven nodes with typed sockets, colored links and header menus](https://raw.githubusercontent.com/ppmpreetham/BUI/main/website/public/images/shots/feature-node-editor-dark.png) \
The node editor, from Python (`examples/site/geometry_nodes.py`)

![A Shading pie menu open over a node editor](https://raw.githubusercontent.com/ppmpreetham/BUI/main/website/public/images/shots/feature-pie-menu-dark.png) ![The File menu with exp typed: Blender's fuzzy search through submenus](https://raw.githubusercontent.com/ppmpreetham/BUI/main/website/public/images/shots/feature-menu-search-dark.png) \
A pie menu, and Blender's menu search (type in any open menu)

![Blender's color picker: HSV wheel, value strip, HSV sliders, hex field and eyedropper](https://raw.githubusercontent.com/ppmpreetham/BUI/main/website/public/images/shots/feature-color-picker-dark.png) ![A tooltip on a Denoise dropdown](https://raw.githubusercontent.com/ppmpreetham/BUI/main/website/public/images/shots/feature-tooltip-dark.png) \
The color picker, and a tooltip

![CSS effects: filters, clip-path shapes, transforms, blend modes and backdrop-filter on a gradient landscape](https://raw.githubusercontent.com/ppmpreetham/BUI/main/website/public/images/shots/feature-html-effects-dark.png) \
CSS effects in Rust: filters, `clip-path`, transforms, blend modes, `backdrop-filter` (`site_effects.rs`)

![A material library laid out with CSS grid: auto-fill columns, spans and dense packing](https://raw.githubusercontent.com/ppmpreetham/BUI/main/website/public/images/shots/feature-css-grid-dark.png) \
CSS grid with `repeat(auto-fill, minmax())`, spans and `grid-auto-flow: dense` (`site_grid.rs`)

![Three moments of CSS keyframe animations: a spinner, a progress sweep, a bouncing ball and a fading toast](https://raw.githubusercontent.com/ppmpreetham/BUI/main/website/public/images/shots/feature-animation-dark.png) \
CSS `@keyframes` at 0.25 s, 0.6 s and 1.0 s (`site_animation.rs`)

**More screenshots**

[![The Rust showcase: a stylesheet, grid cards, Blender form widgets, a table and lists](https://raw.githubusercontent.com/ppmpreetham/BUI/main/docs/images/rust-showcase.png)](https://raw.githubusercontent.com/ppmpreetham/BUI/main/docs/images/rust-showcase.png) \
`examples/showcase.rs`: HTML + CSS in Rust

[![Dear ImGui's demo in Blender's theme, with ImGui's own widgets](https://raw.githubusercontent.com/ppmpreetham/BUI/main/docs/images/imgui-stock.png)](https://raw.githubusercontent.com/ppmpreetham/BUI/main/docs/images/imgui-stock.png) \
Dear ImGui's demo with its own widgets, in Blender's theme

[![Qt syntax on BUI](https://raw.githubusercontent.com/ppmpreetham/BUI/main/docs/images/qt.png)](https://raw.githubusercontent.com/ppmpreetham/BUI/main/docs/images/qt.png) \
Qt syntax

[![tkinter Text with tags](https://raw.githubusercontent.com/ppmpreetham/BUI/main/docs/images/tkinter_notes.png)](https://raw.githubusercontent.com/ppmpreetham/BUI/main/docs/images/tkinter_notes.png) [![tkinter Canvas](https://raw.githubusercontent.com/ppmpreetham/BUI/main/docs/images/tkinter_canvas.png)](https://raw.githubusercontent.com/ppmpreetham/BUI/main/docs/images/tkinter_canvas.png) [![tkinter messagebox](https://raw.githubusercontent.com/ppmpreetham/BUI/main/docs/images/tkinter_messagebox.png)](https://raw.githubusercontent.com/ppmpreetham/BUI/main/docs/images/tkinter_messagebox.png) \
tkinter programs: text with tags, canvas, dialogs

* * *

## Part II: Getting started

## 7\. Install

**Python.** You need CPython 3.11 or newer. Wheels are published for Windows, Linux and macOS, so there's nothing to compile. One package gives you all four Python APIs (tkinter syntax, Qt syntax, the Blender components and Dear ImGui):

```shell
pip install blenderui
python -c "import blenderui; print(blenderui.__version__)"
```

Import it as `blenderui`. You'll also see `import bui` in older examples: it's the same code under its original name, but an unrelated project owns `bui` on PyPI, so prefer `blenderui` in anything you publish.

**Rust.** The crate is [`blenderui` on crates.io](https://crates.io/crates/blenderui):

```shell
cargo add blenderui
```

The first build compiles BUI's C kit for you, so you need Rust 1.88 or newer, CMake and a C compiler (on Windows, the Visual Studio Build Tools for the default MSVC target), and on Linux the X11 headers (`libx11-dev` or `libX11-devel`; no GL headers are needed).

**JavaScript.** You need Node.js 18 or newer, or Bun:

```shell
npm install @blender-ui/react react     # or: bun add @blender-ui/react react
npm install @blender-ui/svelte svelte   # Svelte 5
npm install @blender-ui/native          # the DOM-like API, without React
```

[![@blender-ui/react](https://camo.githubusercontent.com/1a9c735a548a2f39ecde5210fda6bffb8a90c9f4db75fa859769a6c6adec9064/68747470733a2f2f696d672e736869656c64732e696f2f6e706d2f762f40626c656e6465722d75692f72656163743f6c6162656c3d253430626c656e6465722d75692532467265616374)](https://www.npmjs.com/package/@blender-ui/react) [![@blender-ui/native](https://camo.githubusercontent.com/cfb94e39f3cad42164161a92b63dbcec1108fde260852102763693f772f0e82b/68747470733a2f2f696d672e736869656c64732e696f2f6e706d2f762f40626c656e6465722d75692f6e61746976653f6c6162656c3d253430626c656e6465722d75692532466e6174697665)](https://www.npmjs.com/package/@blender-ui/native)

The npm packages are prebuilt for **Windows x64**. On **Linux**, build the addon from a clone (`npm install` then `npm run build` in `bindings/`, with Rust, CMake and a C compiler); windows are native X11 windows there. On **macOS**, JavaScript runs headless for now (see [Platforms](https://github.com/ppmpreetham/BUI#34-platforms)).

**C and C++.** Build from source with CMake 3.20+ and a C11 / C++20 compiler (MSVC, GCC or MinGW-w64, or Clang); on Linux, also the X11 headers:

```shell
git clone https://github.com/ppmpreetham/BUI && cd BUI
cmake -S . -B build
cmake --build build -j 2
ctest --test-dir build            # optional: run the test suites
```

In your own CMake project, add BUI as a subdirectory and link `bui::kit` (or `bui::imgui` for Dear ImGui):

```cmake
add_subdirectory(BUI)
target_link_libraries(myapp PRIVATE bui::kit)
```

**Dear ImGui.** Python gets it with `pip install blenderui` (`import blenderui.imgui`). C++ links `bui::imgui`, C links `bui_cimgui`. Rust uses the [`blenderui-imgui`](https://crates.io/crates/blenderui-imgui) crate:

```shell
cargo add blenderui-imgui
```

**If something goes wrong:**

| Symptom                  | Fix                                                                                                                                                                 |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `ImportError: buipy`     | You're running from a source checkout without a build. Build first (`cmake --build build`), or set `BUIPY_BUILD_DIR`. The extension must match your Python version. |
| The window never appears | Start the event loop: `mainloop()`, `app.exec()` or `App.run()`.                                                                                                    |
| Blurry or tiny UI        | Set `BUI_UI_SCALE`, for example `BUI_UI_SCALE=1.25`.                                                                                                                |
| `gpu=True` does nothing  | Run with `BUI_GPU_DEBUG=1` to see why the GPU device didn't start. Without Direct3D 11 or OpenGL 3.3, BUI stays on the CPU renderer, which draws the same pixels.   |

More: [docs/getting-started.md](https://github.com/ppmpreetham/BUI/blob/main/docs/getting-started.md) and [Install BUI](https://bui.preetham.top/docs/start/installation/) on the website.

## 8\. First app: tkinter

Run any tkinter program on BUI **without changing a line**:

```shell
blenderui-tk my_app.py          # same as: python -m bui.tksyntax my_app.py
```

`blenderui-tk` makes `import tkinter` return BUI's implementation for that run. Or opt in from code:

```python
import blenderui.tksyntax as tk
from blenderui.tksyntax import ttk, messagebox

root = tk.Tk()
name = tk.StringVar(value="Cube")
ttk.Entry(root, textvariable=name).pack(fill="x", padx=8, pady=8)
ttk.Button(root, text="Greet",
           command=lambda: messagebox.showinfo("Hi", "Hello " + name.get())).pack()
root.mainloop()
```

![An unchanged tkinter program: a glTF export dialog with entries, checkbuttons, radio buttons, a spinbox, an option menu, a scale and a listbox](https://raw.githubusercontent.com/ppmpreetham/BUI/main/website/public/images/shots/python-tkinter-dark.png) ![An unchanged ttk program: a render-farm queue with a combobox, notebook, treeview and progress bar](https://raw.githubusercontent.com/ppmpreetham/BUI/main/website/public/images/shots/python-ttk-dark.png) \
Unchanged tkinter and ttk programs (`examples/site/tk_export.py`, `ttk_queue.py`)

Number fields (`Spinbox`, `Scale`) scrub like Blender's, Ctrl+Z / Ctrl+Shift+Z undo edits, and `filedialog` opens Blender's File Browser. Guide: [docs/tkinter.md](https://github.com/ppmpreetham/BUI/blob/main/docs/tkinter.md).

## 9\. First app: Blender components

Build with Blender's own components:

```python
from blenderui.components import App, Panel, NumberSlider, VectorField, Checkbox, ColorSwatch, Button

App("Object", (420, 520),
    Panel("Transform",
          VectorField("Location", (0.0, 1.25, 0.0)),
          NumberSlider("Scale", 1.0, min=0, max=10, on_change=print)),
    Panel("Shading",
          Checkbox("Shadows", True),
          ColorSwatch("Color", (0.8, 0.3, 0.1, 1.0))),
    Button("Apply", on_click=lambda: print("apply")),
).run()
```

![A Render Properties editor built from Blender components: menus, side tabs, panels with header checkboxes, number fields, sliders, dropdowns and a color swatch](https://raw.githubusercontent.com/ppmpreetham/BUI/main/website/public/images/shots/python-components-dark.png) \
A Render Properties editor from the components (`examples/site/render_props.py`)

There are 45 components: layout, values, buttons, views, menus and pie menus, the color picker, the color ramp, curve and node editors, areas and templates. Reference: [docs/components.md](https://github.com/ppmpreetham/BUI/blob/main/docs/components.md).

## 10\. First app: Qt syntax

Write Qt-style Python. The import is the only line that differs from PySide or PyQt, and neither is needed:

```python
import sys
from blenderui.qtsyntax import (QApplication, QWidget, QVBoxLayout, QFormLayout,
                                QLineEdit, QComboBox, QSlider, QProgressBar,
                                QPushButton, QLabel, Qt)

app = QApplication(sys.argv)
window = QWidget()
window.setWindowTitle("Export")
layout = QVBoxLayout(window)

form = QFormLayout()
kind = QComboBox()
kind.addItems(["glTF", "FBX", "OBJ"])
quality = QSlider(Qt.Horizontal)
quality.setRange(0, 100)
form.addRow("Format:", kind)
form.addRow("Quality:", quality)
layout.addLayout(form)

bar = QProgressBar()
quality.valueChanged.connect(bar.setValue)
layout.addWidget(bar)

status = QLabel("Ready.")
go = QPushButton("Export")
go.clicked.connect(lambda: status.setText("Exporting as " + kind.currentText()))
layout.addWidget(status)
layout.addWidget(go)

window.show()
sys.exit(app.exec())
```

![A material editor in Qt syntax: a list, line edit, group box with a form layout, combo box, slider with spin box, checkbox and progress bar](https://raw.githubusercontent.com/ppmpreetham/BUI/main/website/public/images/shots/python-qt-dark.png) \
Qt syntax (`examples/site/qt_material.py`)

Qt syntax is **partial** (about 24% of Qt's method names on the main widget classes); anything missing raises `AttributeError`. Guide: [docs/qt.md](https://github.com/ppmpreetham/BUI/blob/main/docs/qt.md).

## 11\. First app: Rust, in rsx and gpui builders

BUI's Rust crate has **two first-class syntaxes**, and they draw the same window:

- **`rsx!`** is HTML-like markup inside a macro, with `#[component]` functions and hooks for state.
- **gpui builders** are plain method chains: `div().flex().gap_3().child(...)`. State lives in your view struct, and `cx.listener` updates it. gpui code compiles against BUI too.

**In `rsx!`** (this is [`bindings/rust/blenderui/examples/hello.rs`](https://github.com/ppmpreetham/BUI/blob/main/bindings/rust/blenderui/examples/hello.rs); run it with `cargo run --release --example hello` in `bindings/rust`):

```rust
use blenderui::prelude::*;

#[component]
fn counter(label: &str) {
    let (count, set_count) = use_state(0);
    rsx! {
        <div flex items-center gap-3 p-4 rounded-xl bg-zinc-800>
            <span text-zinc-300>{label.to_string()} ": " {count().to_string()}</span>
            <div px-3 py-1 rounded-md bg-sky-700 hover:bg-sky-500 onClick={move || set_count(count() + 1)}>
                "Add one"
            </div>
        </div>
    }
}

struct Hello;

impl Render for Hello {
    fn render(&mut self, _: &mut Window, _: &mut Context<Self>) -> impl IntoElement {
        rsx! {
            <body m-0 p-6 min-h-screen flex flex-col gap-4 bg-zinc-900 text-white>
                <h1 text-2xl font-bold>"Hello, BUI"</h1>
                <counter label="Clicks" />
                <NumberSlider min={0.0} max={10.0} value="5" />   // a real Blender slider
            </body>
        }
    }
}

fn main() {
    Application::new().run(|cx| {
        cx.open_window(WindowOptions::default(), |_, cx| cx.new(|_| Hello)).unwrap();
    });
}
```

**In gpui builders**, the same window:

```rust
use blenderui::prelude::*;

struct Hello { count: i32 }

impl Render for Hello {
    fn render(&mut self, _: &mut Window, cx: &mut Context<Self>) -> impl IntoElement {
        let counter = div().flex().items_center().gap_3().p_4().rounded_xl().bg(rgb(0x27272a))
            .child(span().text_color(rgb(0xd4d4d8)).child(format!("Clicks: {}", self.count)))
            .child(div().px_3().py_1().rounded_md().bg(rgb(0x0369a1))
                .hover(|s| s.bg(rgb(0x0ea5e9))).cursor_pointer()
                .child("Add one")
                .on_click(cx.listener(|this, _, _, cx| {
                    this.count += 1;
                    cx.notify();
                })));
        body().m_0().p_6().css("min-height", "100vh").flex().flex_col().gap_4()
            .bg(rgb(0x18181b)).text_color(white())
            .child(h1().text_2xl().font_bold().child("Hello, BUI"))
            .child(counter)
            .child(NumberSlider().min(0.0).max(10.0).value("5"))   // a real Blender slider
    }
}

fn main() {
    Application::new().run(|cx| {
        cx.open_window(WindowOptions::default(), |_, cx| cx.new(|_| Hello { count: 0 })).unwrap();
    });
}
```

[![The Rust hello example: a heading, a counter component and a Blender slider](https://raw.githubusercontent.com/ppmpreetham/BUI/main/docs/images/rust-hello.png)](https://raw.githubusercontent.com/ppmpreetham/BUI/main/docs/images/rust-hello.png)

Two small things that trip people up in `rsx!`:

- Text children are text (`&'static str`, `String`, `SharedString`) or elements. Numbers and borrowed `&str` props aren't, so write `{count().to_string()}` and `{label.to_string()}`.
- A component's body is a normal block that ends in markup, usually an `rsx!`.

Here's a bigger example, a render-farm dashboard, written once in each syntax. The two programs draw identical pixels:

![A render-farm dashboard: a CSS grid of stat cards, job rows with Blender progress bars, a node list, a dropdown, button and checkbox](https://raw.githubusercontent.com/ppmpreetham/BUI/main/website/public/images/shots/rust-rsx-dashboard-dark.png) \
`site_dashboard.rs` (`rsx!`) and `site_dashboard_builder.rs` (gpui builders)

Guide: [docs/rust.md](https://github.com/ppmpreetham/BUI/blob/main/docs/rust.md), and the [Rust tutorial](https://bui.preetham.top/docs/tutorials/first-app-rust/) on the website.

## 12\. First app: JavaScript and React

```js
/** @jsxImportSource @blender-ui/react */
import { render, useState, NumberSlider } from "@blender-ui/react"

function Counter() {
  const [count, setCount] = useState(0) // React's hook, holding a signal
  return (
    <main styled className="p-6 flex flex-col gap-3">
      {" "}
      {/* `styled`: Blender's look */}
      <span>Clicks: {count}</span>
      <button onClick={() => setCount((c) => c + 1)}>Add one</button>
      <NumberSlider min={0} max={10} /> {/* a Blender component */}
    </main>
  )
}

render(<Counter />, { title: "Counter", width: 360, height: 200, gpu: true })
```

```shell
bun app.jsx                    # run it (Bun reads JSX directly)
npx bui build app.jsx          # or ship it: app.exe, one 7 MB file
```

Without React, `@blender-ui/native` gives you a DOM-like API:

```js
const { App, signal } = require("@blender-ui/native")

const app = new App({ gpu: true })
const win = app.window({ title: "Hello", width: 360, height: 200 })
win.body.className = "m-0 p-6 flex flex-col gap-3 bg-[#2b2b2b] text-[#e6e6e6]"

const clicks = signal(0)
const label = win.createElement("span")
label.textContent = clicks // follows the signal
const button = win.createElement("button")
button.textContent = "Add one"
button.addEventListener("click", () => clicks.value++)
win.body.appendChild(label)
win.body.appendChild(button)
```

![A task list in React on BUI: a Blender text field, button, checkboxes and a progress bar](https://raw.githubusercontent.com/ppmpreetham/BUI/main/website/public/images/shots/js-react-tasks-dark.png) \
A task list in React (`examples/site/js/todo_react.js`); the DOM-like version draws the same

Node.js 18+ or Bun. Prebuilt for Windows x64; on Linux, build the addon from source (`npm run build` in `bindings/node`) and windows are native. Guide: [docs/javascript.md](https://github.com/ppmpreetham/BUI/blob/main/docs/javascript.md).

## 13\. First app: Dear ImGui

The real Dear ImGui 1.93 runs on BUI: BUI's windows, input, CPU/GPU renderer, Blender's theme and the Inter font. Existing ImGui code compiles unchanged.

**C++:**

```c++
#include "imgui.h"
#include "imgui_impl_bui.h"     // see examples/imgui_demo.cpp

static void frame(void*) {
    ImGui::ShowDemoWindow();
    ImGui::Begin("Hello");
    static float v = 0.5f;
    ImGui::SliderFloat("Value", &v, 0.0f, 1.0f);
    ImGui::End();
}

int main() {
    ImGui::CreateContext();
    ImGui_ImplBUI_App* app = ImGui_ImplBUI_CreateApp("Demo", 1280, 800, frame, nullptr);
    ImGui_ImplBUI_RunApp(app);
    ImGui_ImplBUI_DestroyApp(app);
    ImGui::DestroyContext();
}
```

**Python:**

```python
import blenderui.imgui as imgui        # C: cimgui.h  ·  Rust: the blenderui-imgui crate

state = {"value": 0.5}

def frame():
    imgui.show_demo_window()
    imgui.begin("Hello")
    _, state["value"] = imgui.slider_float("Value", state["value"], 0.0, 1.0)
    imgui.end()

imgui.run(frame, title="Hello", size=(1280, 800))
```

**C** (`cimgui.h`, cimgui conventions: `ImGui_Button`, `ImGui_ButtonEx`, and the backend in `cimgui_impl_bui.h`) and **Rust** (`blenderui-imgui`) work the same way:

```rust
use blenderui_imgui as imgui;

fn main() {
    let mut value = 0.5f32;
    imgui::run("Hello", [1280, 800], move || {
        imgui::show_demo_window();
        imgui::begin("Hello");
        imgui::slider_float("Value", &mut value, 0.0, 1.0);
        imgui::end();
    });
}
```

Build with `-DBUI_IMGUI_BLENDER_WIDGETS=ON` (or the Rust feature `blender-widgets`) and ImGui's widgets become Blender's own: `DragFloat` is Blender's drag-to-scrub number field (Shift for precision, Ctrl to snap), `SliderFloat` Blender's number slider, checkboxes drag-toggle, menus and combos get Blender's menus. The rest of the ImGui API is unchanged. (The PyPI wheel is built without it, so from Python you get ImGui's own widgets in Blender's theme unless you build from source.)

[![Dear ImGui's demo window drawn with Blender's widgets](https://raw.githubusercontent.com/ppmpreetham/BUI/main/docs/images/imgui-blender.png)](https://raw.githubusercontent.com/ppmpreetham/BUI/main/docs/images/imgui-blender.png) \
Dear ImGui's own demo with `BUI_IMGUI_BLENDER_WIDGETS=ON`: number fields, sliders, color rows and panel headers are Blender's

![A scene inspector in Dear ImGui with ImGui's own widgets and colors](https://raw.githubusercontent.com/ppmpreetham/BUI/main/website/public/images/shots/imgui-cpp-stock-dark.png) ![The same inspector with Blender widgets and Blender's style](https://raw.githubusercontent.com/ppmpreetham/BUI/main/website/public/images/shots/imgui-cpp-blender-dark.png) \
The same ImGui code: ImGui's widgets (left) and the Blender-widgets build (right) (`examples/site/imgui_inspector.cpp`)

Guide, coverage and the stock look: [docs/imgui.md](https://github.com/ppmpreetham/BUI/blob/main/docs/imgui.md).

## 14\. First app: C

The kit's API is one header, `kit/include/BUI_kit.h`, and it doubles as the reference: every function has a one-line comment. This is a shortened [`examples/kit_hello.c`](https://github.com/ppmpreetham/BUI/blob/main/examples/kit_hello.c): a slider and a button in a native window.

```c
#include <stdio.h>
#include "BUI_kit.h"

static void on_event(void *data, BKNode *node, int kind, void *ref, double a, double b)
{
  if (kind == BK_EV_CLICK) printf("clicked %s\n", BK_get_str(node, "text"));
  else if (kind == BK_EV_CHANGE) printf("%s = %g\n", BK_get_str(node, "label"), BK_get_num(node, "value"));
}

int main(void)
{
  BKNode *root = BK_node_new("frame");          /* everything is a node */
  BK_set_num(root, "padding", 8);               /* configured by property name */

  BKNode *size = BK_node_new("slider");
  BK_set_str(size, "label", "Size");
  BK_set_num(size, "max", 10);
  BK_set_num(size, "value", 2.5);
  BK_node_add(root, size, -1);
  BK_geom_set_manager(size, BK_GEOM_PACK);      /* Tk's packer, in C */
  BK_geom_set(size, "fill", 1);

  BKNode *apply = BK_node_new("button");
  BK_set_str(apply, "text", "Apply");
  BK_node_add(root, apply, -1);
  BK_geom_set_manager(apply, BK_GEOM_PACK);

  BKContext *ctx = BK_context_new(root);        /* input, focus, popups, undo */
  BKHost host = {0};
  host.emit = on_event;                         /* the window fills in the rest */
  BK_context_set_host(ctx, &host);

  if (BK_window_supported()) {                  /* Win32, X11 or Cocoa */
    BKWindow *win = BK_window_new(ctx, "BUI from C", 320, 120);
    BK_window_show(win);
    BK_window_run();
    BK_window_free(win);
  }
  BK_context_free(ctx);
  BK_node_free(root);
  return 0;
}
```

```shell
cmake -S . -B build -DBUI_BUILD_EXAMPLES=ON
cmake --build build --target bui_kit_hello
```

![A modifier stack built with the C kit: panels, enum rows, number fields, dropdowns, a slider, checkboxes and buttons](https://raw.githubusercontent.com/ppmpreetham/BUI/main/website/public/images/shots/c-kit-app-dark.png) \
A bigger C program: a modifier stack (`examples/site/kit_modifiers.c`)

Without a window system, render into a raster instead (`BK_render(ctx, raster, w, h, full)`), which is also how tests and screenshots work. Guide: [docs/c-api.md](https://github.com/ppmpreetham/BUI/blob/main/docs/c-api.md).

* * *

## Part III: Concepts

## 15\. How it works

```
 your code:   tkinter   Qt   components   Dear ImGui (C++/C/Python/Rust)   Rust: rsx! / HTML / CSS / Tailwind / hooks   JS / React
                  │      │        │                │                                  │                                    │
                  └──── Python: options, signals    │ ImGui core + BUI backend          └── style cascade + taffy layout ◄────┘
                                  │                │                                  │
 C kit (kit/):   node tree · geometry · input & interaction · Blender widget drawing · undo · animation · themes · Blender search/expr
                                         │
 renderer:       damage-driven software raster (parallel)   or   GPU (Direct3D 11 / OpenGL 3.3, instanced, opt-in)
                                         │
 window:         native Win32 / X11 / Cocoa window   ·   stdlib Tk window   ·   headless (tests, screenshots)
```

1. **Your code**, in any language above, creates widgets and says what happens when they change.
2. **The kit** keeps them in one tree, lays them out and handles input, following Blender's rules.
3. **The renderer** draws them, on the CPU by default or on the GPU if you ask.
4. **Damage rendering** keeps it cheap: a change redraws only the rectangle it touched, and an idle window uses no CPU at all.

What's under the hood:

- **Blender's real UI code.** Widget drawing is a C port of Blender's `interface_widgets.cc` (`round_box_edges`, `widgetbase_draw`, the shape tables), and interaction follows `interface_handlers.cc`. Color math, rects, text cursors, string search and expression evaluation are Blender's `blenlib`, copied in. A few interface files (string search, text-edit undo, float precision) are copied into `kit/intern/blender/` and edited for the kit. The X11 and Cocoa hosts follow Blender's GHOST; the Win32 host is BUI's own.
- **One node tree.** Every widget is one kit node. The C kit owns layout, hit testing, hover, focus, drags, menus, tooltips, undo and animation.
- **Damage rendering.** A change marks only its widget's rectangle, and only changed tiles reach the screen.
- **Thin bindings.** Python, Rust and JavaScript keep only what their syntax layer needs.

More: [ARCHITECTURE.md](https://github.com/ppmpreetham/BUI/blob/main/ARCHITECTURE.md) and [How it works](https://bui.preetham.top/docs/concepts/how-it-works/).

## 16\. Blender's widgets and interactions

These rules live in the C kit, so they work the same everywhere Blender widgets are drawn: the Python components, tkinter and Qt syntax, the Rust and JavaScript Blender components and form controls inside `styled`, ImGui's Blender-widgets build, and every C kit widget.

- **Drag to scrub.** Press a number field and drag sideways. The cursor hides and wraps, so you never hit the screen edge, and comes back where the drag started. A float moves one click step per pixel; Shift moves at a tenth of the speed; Esc cancels and restores the value.
- **Ctrl snaps** with Blender's rules: unbounded floats to whole units, angles to 10°, sliders to 0.1, 1 or 10 depending on their range, integers to multiples of 10.
- **Click to type**, and type **math**: Blender's expression evaluator (`expr_pylike_eval.cc`) turns `2*pi`, `90/4`, `min(4, 3)` or `x if c else y` into numbers. This works in Blender number fields and sliders; plain text fields keep what you type as text.
- **Shortcuts on any value:** Backspace resets to the default, `-` negates, Ctrl+Wheel steps, Ctrl+C / Ctrl+V copy and paste, right-click opens Copy, Paste and Reset.
- **Several fields at once.** Drag *down* over a column of number fields to collect them, then sideways to change them together. Drag across checkboxes (or panel headers) to set them all in one stroke.
- **Panels** collapse with Blender's animation; Ctrl+click opens one and closes the others; drag the grip to reorder.
- **Menus** work press-drag-release or click; hover switches menu-bar menus; **pie menus** open around the pointer.
- **Menu search** is Blender's string search: start typing in any open menu. It matches word initials ("ad cu" finds "Add Cube"), forgives typos, and ranks your recent picks first.
- **Undo everywhere.** Ctrl+Z and Ctrl+Shift+Z work on every UI edit, and inside a text field while you type. A burst of typing or a multi-field drag is one step.
- **Float precision** follows Blender: small values show the decimals they need.
- **Tooltips** appear after Blender's half-second delay.

Coming soon: typing values with units ("2 m"), Blender's search-as-you-type popups, and widget drag and drop. More: [Blender's widgets](https://bui.preetham.top/docs/concepts/blender-widgets/).

## 17\. Three looks: HTML, styled and Blender components

In Rust and JavaScript you pick the look per element:

| You write                                                              | You get                                                                                                |
| ---------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------ |
| Plain HTML: `<button>`, `<input type="range">`, `<select>` ...         | What a browser draws: black text on white and Chrome's form controls, in the Inter UI font             |
| `styled` on any element: `<div styled>`, `div().styled()`              | Blender's look for it and everything inside: Blender's theme (dark or light), UI font, Blender widgets |
| **Blender components** (capitalized): `<NumberSlider>`, `<Button>` ... | Blender's widgets and interaction everywhere (drag to scrub, Ctrl+Z, Blender's color picker)           |

The components are `Button`, `Checkbox`, `Radio`, `NumberField`, `NumberSlider`, `TextField`, `TextArea`, `ColorSwatch`, `Dropdown` and `ProgressBar`. Each one is the HTML element it stands for, so every attribute, style and event works on it:

```rust
rsx! {
    <main class="p-4 flex gap-2">                          // browser look
        <input type="range" />                             // Chrome's slider
        <NumberSlider min={0.0} max={10.0} value="2.5" />  // Blender's slider
        <div styled><button>"Apply"</button></div>         // Blender's look inside
    </main>
}
div().child(Checkbox().checked(true)).child(Button().child("Apply"))   // gpui syntax
```

```js
import { render, NumberSlider, Button } from "@blender-ui/react"
// <NumberSlider min={0} max={10} onChange={...} />, <Button onClick={...}>Apply</Button>
// DOM API: win.createElement("NumberSlider")
```

[![Form controls as plain HTML, inside styled, and as Blender components](https://raw.githubusercontent.com/ppmpreetham/BUI/main/docs/images/rust-forms.png)](https://raw.githubusercontent.com/ppmpreetham/BUI/main/docs/images/rust-forms.png) \
`cargo run --example forms`: plain HTML, inside `styled`, and the Blender components

![A form and a table in plain HTML, with a browser's default look](https://raw.githubusercontent.com/ppmpreetham/BUI/main/website/public/images/shots/rust-html-plain-dark.png) ![The same markup inside styled: Blender's theme and widgets](https://raw.githubusercontent.com/ppmpreetham/BUI/main/website/public/images/shots/rust-html-styled-dark.png) \
The same markup, plain (left) and inside `styled` (right). Plain HTML looks the same in both themes.

![Camera properties built from Blender components in Rust: number fields, sliders, a dropdown, checkboxes, a text field, a color swatch, a progress bar and buttons](https://raw.githubusercontent.com/ppmpreetham/BUI/main/website/public/images/shots/rust-components-dark.png) \
Blender components from Rust (`site_components.rs`)

Python's [Blender components](https://github.com/ppmpreetham/BUI/blob/main/docs/components.md) (`Panel`, `PieMenu`, the node editor ...) are a separate, larger set.

## 18\. Rendering on the CPU and the GPU

BUI has two renderers, and they draw **the same pixels**.

- **The software raster** (`gpu/intern/gpu_raster.c`) is the default. It draws anti-aliased shapes with signed-distance coverage, like Blender's widget shaders, into one frame buffer, compares it with the last frame in 64×32 tiles, and reports only the tiles that changed. Big canvases draw in parallel bands on Windows (`BUI_RASTER_THREADS`). It needs nothing from your graphics card and uses the least memory.
- **The GPU renderer** is opt-in: **Direct3D 11 on Windows** (`gpu/intern/gpu_d3d11.c`) and **OpenGL 3.3 core on Linux** (`gpu/intern/gpu_gl.c`). It's modeled on Blender's instanced widget drawing: each primitive is one 128-byte instance of a single uber-shader, so a frame is a few instanced draw calls. Text and images are cached in atlases. Frames render into a persistent target, so only damaged regions redraw; on Windows, a flip-model swap chain presents dirty rectangles. On Linux, `libGL`, `libEGL` and `libX11` are loaded at run time (GLX for windows, EGL for offscreen), so no GL headers are needed, and BUI falls back to the CPU where OpenGL is missing.

The shaders evaluate the software raster's formulas: the parity test finds 10 of 172,800 pixels that differ by more than 3/255, on both backends.

Turn the GPU on:

| Where                   | How                                                                                |
| ----------------------- | ---------------------------------------------------------------------------------- |
| Python                  | `bui.set_gpu(True)` before creating windows; tkinter syntax also `tk.Tk(gpu=True)` |
| Rust                    | `Application::new().gpu(true)`, or `WindowOptions { gpu: Some(true), .. }`         |
| JavaScript              | `new App({ gpu: true })`, or React's `render(..., { gpu: true })`                  |
| C                       | `BUI_gpu_set_default(1)`, or `BK_window_set_gpu(win, 1)`                           |
| Any app, no code change | `BUI_GPU=1`                                                                        |

`BUI_GPU=1` overrides the app-wide setting in code; `BUI_GPU=0` turns the app-wide defaults off, but a window that asks for the GPU itself still gets it. Where no device can start, BUI quietly stays on the CPU (`BUI_GPU_DEBUG=1` says why).

**Which one?** A tool window, form or settings panel: the CPU (lightest on memory, idles at zero). A large or HiDPI window, lots of animation, thousands of items: the GPU (about half the CPU per frame, or less, for about 10 MB more RAM plus GPU memory). Servers and tests: the CPU. macOS: the CPU, for now (a GPU renderer there is coming soon).

More: [Rendering](https://bui.preetham.top/docs/concepts/rendering/).

## 19\. Damage rendering

Leave a BUI window open and walk away: it uses no CPU at all. Hover a button, and BUI redraws that button, not the window.

1. A change marks only its widget's rectangle as dirty (popups get a margin for their shadow).
2. Dirty rectangles merge (up to 24 per window).
3. If nothing is dirty, the loop doesn't render; it sleeps until the next input or timer.
4. Only the dirty regions are redrawn, clipped to their rectangles.
5. The raster compares the new pixels with the last frame in 64×32 tiles; a redraw that changed nothing reports nothing.
6. Only damaged tiles reach the screen (Win32 copies them, X11 sends each rectangle with `XPutImage`, Cocoa invalidates them, the GPU presents dirty rectangles).

On the full component demo, that's about **0.7 ms** for a complete frame and about **0.05 ms** for a hover or number-scrub frame. Animations run only while something moves, paced to the display. More: [Damage rendering](https://bui.preetham.top/docs/concepts/damage-rendering/).

## 20\. Window hosts

A **window host** puts a kit context in a top-level window and feeds it input.

| Host                             | Where                           | Notes                                                                                                                                                                                                                                                                        |
| -------------------------------- | ------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Win32** (`kit_window_win32.c`) | Windows                         | BUI's own host. Per-monitor DPI, vsync-paced animation, clipboard, cursors, window-manager features.                                                                                                                                                                         |
| **X11** (`kit_window_x11.c`)     | Linux; Wayland through XWayland | Input mapping after Blender's GHOST. `Xft.dpi` scaling. libX11 is loaded when the first window opens.                                                                                                                                                                        |
| **Cocoa** (`kit_window_cocoa.m`) | macOS                           | **Experimental.** Ported from Blender's GHOST Cocoa code, with Retina scaling. It's checked by CI and has been tried by hand on one Apple Silicon Mac so far; testing notes in [docs/macos-testing.md](https://github.com/ppmpreetham/BUI/blob/main/docs/macos-testing.md). |
| **Tk**                           | Python, anywhere                | A stdlib `tkinter` window that shows the kit's frame buffer. `BUI_HOST=tk` forces it; it's also the fallback where no native host works.                                                                                                                                     |
| **Headless**                     | Everywhere                      | No window: render into a raster for tests and screenshots, with a virtual clock that steps animations exactly.                                                                                                                                                               |

`BUI_UI_SCALE=1.5` fixes the UI scale instead of following the display (1 is Blender's default, 20 px widgets). More: [Window hosts](https://bui.preetham.top/docs/concepts/window-hosts/).

## 21\. Text and fonts

Your app's text looks the same on Windows, Linux and macOS, and it looks like Blender's:

- **Inter**, Blender's UI font, is embedded (the variable version, with optical-size and weight axes). UI text is 11 pt at scale 1.
- **Rendered like Blender's BLF**: BUI vendors FreeType 2.13.3 and renders with light hinting, Blender's gamma-1.43 coverage table, Inter's optical size, a real bold instance, no kerning (as in Blender), and Blender's text shadows. With `BUI_TEXT_FREETYPE=OFF`, stb\_truetype rasterizes instead.
- **On the GPU**, glyphs are drawn from their outlines with [Slug](https://github.com/EricLengyel/Slug) by default, or from a per-glyph atlas like Blender's (`BUI_SLUG=0`, `bui.set_slug(False)`, `.slug(false)`, `BUI_gpu_set_slug(0)`).
- **Other fonts**: `BUI_FONT=path.ttf` replaces Inter, `BUI_FONT_DIR` adds a folder to search first, `blenderui::add_font` (Rust) and `BUI_text_add_font` (C) register faces from memory, and CSS `@font-face` loads local files.
- **Windows' GDI text** is opt-in (`-DBUI_TEXT_GDI=ON`, Rust feature `gdi-text`): ClearType-style hinting and system fonts, without Slug.

Limits worth knowing: no per-glyph font fallback yet (missing characters show as boxes), no IME, right-to-left text is reordered but not shaped, no OpenType features. Font fallback and IME are coming soon. More: [Text and fonts](https://bui.preetham.top/docs/concepts/text-and-fonts/).

## 22\. Themes: dark and light

BUI looks like Blender because it uses Blender's theme. It ships two: Blender's **Dark** theme (the default) and its **Light** preset. Both are generated by `tools/gen_theme.py` from Blender's own `Blender_Dark.xml` and `Blender_Light.xml` presets, over Blender 5.x's built-in defaults. Switch at run time, and every open window relayouts and redraws:

| Where                   | Light theme                                                                                                 |
| ----------------------- | ----------------------------------------------------------------------------------------------------------- |
| C                       | `BK_set_theme("light")`; `BK_theme_name()`, `BK_theme_color("region_bg", rgba)`                             |
| Python                  | `blenderui.set_theme("light")` (also switches the Dear ImGui extension); `theme_name()`, `theme_color(key)` |
| Rust                    | `blenderui::set_theme("light")`, or `Application::new().theme("light")`; `theme_name()`, `theme_color(key)` |
| JavaScript              | `new App({ theme: "light" })`, `app.setTheme("light")`, or React's `render(..., { theme: "light" })`        |
| Dear ImGui              | Its Blender style follows the kit's theme (pick it before the backend starts)                               |
| Any app, no code change | `BUI_THEME=light` (or `dark`)                                                                               |

Widgets, panels, the region background, the `styled` HTML sheet and ImGui's Blender style all follow the theme. Plain HTML keeps a browser's look either way. Inside each theme you can still color things yourself: `bg` / `fg` in tkinter, `setStyleSheet` in Qt syntax, `color=` on components, full CSS on HTML in Rust and JavaScript, ImGui's own style colors.

BUI also includes Blender's icons: all 781 of Blender's SVG icons, rasterized with Blender's NanoSVG code so they're sharp at any scale. Use them by Blender's name: `Button("Add", icon="ADD")`, `EnumRow(..., icons=[...])`.

Coming soon: loading any Blender theme file, beyond Dark and Light. More: [Theming](https://bui.preetham.top/docs/concepts/theming/).

* * *

## Part IV: Guides by language

## 23\. Python

`pip install blenderui` gives you four APIs in one package:

- **tkinter syntax** (`blenderui.tksyntax`): every public tkinter and ttk name (7,846 of 7,846). Run existing programs with `blenderui-tk app.py`, or call `blenderui.tksyntax.install()` first. `messagebox` and `simpledialog` are Blender-styled, `colorchooser` is Blender's color picker, and `filedialog` is Blender's File Browser (`BUI_FILEDIALOG=native` for the OS dialogs). Look-only Tk options (`relief`, `ttk.Style` ...) are accepted, but the look comes from Blender's theme. Guide: [docs/tkinter.md](https://github.com/ppmpreetham/BUI/blob/main/docs/tkinter.md).
- **Qt syntax** (`blenderui.qtsyntax`): widgets, layouts (box, grid, form, stacked), signals, `QPainter`, menus, timers and animations, `QFileDialog` as Blender's File Browser. Partial: model/view classes, docks, toolbars, splitters and Qt stylesheets are missing. Guide: [docs/qt.md](https://github.com/ppmpreetham/BUI/blob/main/docs/qt.md).
- **Blender components** (`blenderui.components`): 45 components, from `NumberSlider` to `NodeEditor`. Reference: [docs/components.md](https://github.com/ppmpreetham/BUI/blob/main/docs/components.md).
- **Dear ImGui** (`blenderui.imgui`), pyimgui-style. Guide: [docs/imgui.md](https://github.com/ppmpreetham/BUI/blob/main/docs/imgui.md#python).

You can mix them in one process: windows, timers and the event loop are shared (`bui.host`). Every interaction runs in C, so tests can drive widgets headless. On the website: [tkinter](https://bui.preetham.top/docs/python/tkinter/), [Qt syntax](https://bui.preetham.top/docs/python/qt-syntax/), [components](https://bui.preetham.top/docs/python/components/), [headless testing](https://bui.preetham.top/docs/python/headless-testing/).

## 24\. Rust

The `blenderui` crate is a web-like framework drawn by BUI: every HTML element, CSS, Tailwind, HTML-like markup (`rsx!` / `view!`) or gpui-style builders, components and hooks. A taste:

```rust
rsx! {
    // Tailwind as attributes, any variant, arbitrary values; typos fail to compile
    <button px-3 py-1 rounded-md bg-zinc-800 hover:bg-sky-600 md:dark:hover:bg-sky-400
            active:style="transform: scale(0.97)" onClick={move || save()}>
        "Save"
    </button>

    // control flow right in the markup
    <ul>{for item in items { <li class="py-1">{item}</li> }}</ul>
    {if dirty { <p text-amber-400>"Unsaved changes"</p> }}

    // real CSS, evaluated by the engine
    <div style="width: calc(100% - 2 * var(--gap)); background: color-mix(in oklch, var(--brand) 60%, white)" />
}
```

```
error: rsx!: `bg-sky-50O` is not a Tailwind utility; did you mean `bg-sky-500` or `bg-sky-50` or `bg-sky-100`?
```

What's in the box:

- **Layout** by [taffy](https://github.com/DioxusLabs/taffy): block flow and wrapping text, flexbox and grid (every property, including `grid-template-areas` and named lines), tables, `relative` / `absolute` / `fixed` positioning (fixed boxes are placed against the window), scrolling, and bidirectional text.
- **CSS**: selectors level 4, nesting, `@layer` / `@media` / `@supports` / `@scope` / `@container` / `@font-face` / `@property`, `var()` and `env()`, every math function, CSS Color 4 and 5.
- **Effects and animations**: `@keyframes` animations and transitions (every longhand, `cubic-bezier()`, `steps()`, `linear()`, interruptible transitions), `filter` and `backdrop-filter`, `mix-blend-mode`, `clip-path`, masks, 2D transforms, `border-image`. Frames render only while something moves, and headless windows step a virtual clock (`App::advance_clock`).
- **Tailwind CSS v4**, compiled on first use, with compile-time checks in `rsx!` and `tw!`.
- **Components and hooks** in zopra, React or Leptos style, with optional props and keys.
- **Blender components**, `styled`, and the [theme](https://github.com/ppmpreetham/BUI#22-themes-dark-and-light).
- **Headless testing**: `Application::headless()`, screenshots, simulated input.

Not yet: async resources (`use_resource`), generic components, 3D transforms, OpenType features, multi-column layout. Guide: [docs/rust.md](https://github.com/ppmpreetham/BUI/blob/main/docs/rust.md), crate README: [bindings/rust/blenderui](https://github.com/ppmpreetham/BUI/blob/main/bindings/rust/blenderui/README.md), and the website's [Rust section](https://bui.preetham.top/docs/rust/overview/) (including [effects and animations](https://bui.preetham.top/docs/rust/effects-and-animations/)).

## 25\. JavaScript and React

- **`@blender-ui/react`**: a React 19.3 renderer. HTML host components, CSS and Tailwind, events, Suspense, transitions, actions, and the Blender components. Hooks keep React's names but hold signals: pass one into JSX and a change updates that node without re-rendering. README: [bindings/react](https://github.com/ppmpreetham/BUI/blob/main/bindings/react/README.md).
- **`@blender-ui/svelte`**: Svelte 5 components (runes, snippets, bindings, keyed blocks, context, scoped `<style>`) and the Blender components, on Node.js (`--import @blender-ui/svelte/register`), Bun and `bui build`. README: [bindings/svelte](https://github.com/ppmpreetham/BUI/blob/main/bindings/svelte/README.md).
- **`@blender-ui/native`**: a DOM-like API (`createElement`, `appendChild`, `addEventListener`, `className` with Tailwind, `style`) with signals, `memo`, `effect` and `batch`. README: [bindings/node](https://github.com/ppmpreetham/BUI/blob/main/bindings/node/README.md).
- **How it's fast**: BUI renders on its own UI thread, so it never blocks JavaScript; each tick sends one batch of changes; styles are parsed once.
- **Standalone apps**: `npx bui build app.jsx` makes a ~7 MB executable with an embedded QuickJS engine; `--bun` makes a Bun executable instead; `npx bui run app.jsx` runs on QuickJS without building.
- **Tests**: `new App({ headless: true })` renders off-screen, and `win.click(x, y)` simulates input.

Limits: npm packages are prebuilt for Windows x64 only; events reach JavaScript after BUI handled them (no native `preventDefault`), and `event.target` is the deepest *listening* element. Guide: [docs/javascript.md](https://github.com/ppmpreetham/BUI/blob/main/docs/javascript.md); website: [React](https://bui.preetham.top/docs/javascript/react/), [native API](https://bui.preetham.top/docs/javascript/native-api/), [signals](https://bui.preetham.top/docs/javascript/signals/), [standalone executables](https://bui.preetham.top/docs/javascript/standalone-executables/).

## 26\. Dear ImGui

| Language | What you use                               | Built by                                |
| -------- | ------------------------------------------ | --------------------------------------- |
| C++      | `imgui.h` + `imgui_impl_bui.h`             | CMake target `bui_imgui` (`bui::imgui`) |
| C        | `cimgui.h` + `cimgui_impl_bui.h`           | CMake target `bui_cimgui`               |
| Python   | `import blenderui.imgui as imgui`          | The `_buiimgui` extension, in the wheel |
| Rust     | the `blenderui-imgui` crate (on crates.io) | Cargo                                   |

The C, Python and Rust bindings are generated from `imgui.h`. ImGui is idle-free on BUI: frames run only on input and while something animates. The Blender-widgets build (`BUI_IMGUI_BLENDER_WIDGETS=ON`, Rust feature `blender-widgets`) swaps in Blender's widgets, and can be switched at run time (`ImGui_ImplBUI_SetBlenderWidgets`, `imgui.set_blender_widgets`). In that build, window title bars, tooltips, plain popups, tables and the color picker keep ImGui's drawing.

Guides: [docs/imgui.md](https://github.com/ppmpreetham/BUI/blob/main/docs/imgui.md), [bindings/imgui/README.md](https://github.com/ppmpreetham/BUI/blob/main/bindings/imgui/README.md) (the backend and the Blender style), [bindings/imgui/blender/README.md](https://github.com/ppmpreetham/BUI/blob/main/bindings/imgui/blender/README.md) (the widget mapping), and the website's [Dear ImGui section](https://bui.preetham.top/docs/imgui/cpp/).

## 27\. C and embedding

The kit's concepts: **nodes** made by type name (`frame`, `slider`, `button`, `panel`, `tree`, `canvas`, `filebrowser` ...), **properties** set by name, Tk's **geometry managers** (pack, grid, place) in C, a **context** that owns input, popups, focus and undo, a **host** that receives events, and **windows**. A `custom` node draws and handles input itself; Dear ImGui runs on BUI that way. Guide: [docs/c-api.md](https://github.com/ppmpreetham/BUI/blob/main/docs/c-api.md).

To embed BUI in your own renderer, render the kit into a `BuiRaster` and blit it (as `bui.qt.BuiCanvas` does inside Qt). A separate, lower-level layer, **render sinks** (`api/bui_c_api.h`), hands the triangles, lines and text of the immediate-mode GPU seam to your renderer; the Qt/QPainter backend and the SVG recorder use it. Guide: [INTEGRATION.md](https://github.com/ppmpreetham/BUI/blob/main/INTEGRATION.md).

## 28\. Where the docs live

| Guide                                                                                            | What's inside                                                                                                                        |
| ------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------ |
| [Website](https://bui.preetham.top)                                                             | Everything below, as tutorials, concepts and references, with screenshots in dark and light                                          |
| [Getting started](https://github.com/ppmpreetham/BUI/blob/main/docs/getting-started.md)         | Building, environment variables, window hosts, GPU rendering, troubleshooting                                                        |
| [tkinter](https://github.com/ppmpreetham/BUI/blob/main/docs/tkinter.md)                         | Running tkinter apps on BUI, behavior notes, limitations                                                                             |
| [Qt syntax](https://github.com/ppmpreetham/BUI/blob/main/docs/qt.md)                            | Classes, signals, layouts, painting, coverage                                                                                        |
| [Components](https://github.com/ppmpreetham/BUI/blob/main/docs/components.md)                   | The `bui.components` reference                                                                                                       |
| [Rust](https://github.com/ppmpreetham/BUI/blob/main/docs/rust.md)                               | `rsx!`, hooks, HTML, CSS and Tailwind coverage, effects, animations, events, headless tests                                          |
| [JavaScript, React and Svelte](https://github.com/ppmpreetham/BUI/blob/main/docs/javascript.md) | `@blender-ui/native`, `@blender-ui/react`, `@blender-ui/svelte`, adding a framework, how changes cross to the UI thread, performance |
| [Dear ImGui](https://github.com/ppmpreetham/BUI/blob/main/docs/imgui.md)                        | Dear ImGui on BUI from C++, C, Python and Rust; the Blender-widgets build; coverage                                                  |
| [C API](https://github.com/ppmpreetham/BUI/blob/main/docs/c-api.md)                             | The C kit (`BUI_kit.h`)                                                                                                              |
| [Integration](https://github.com/ppmpreetham/BUI/blob/main/INTEGRATION.md)                      | Embedding BUI drawing in C, C++, Qt or PySide hosts                                                                                  |
| [Architecture](https://github.com/ppmpreetham/BUI/blob/main/ARCHITECTURE.md)                    | Modules and what was taken from Blender                                                                                              |
| [Syntax coverage](https://github.com/ppmpreetham/BUI/blob/main/SYNTAX_COVERAGE.md)              | The Qt and tkinter surface, exactly                                                                                                  |
| [macOS testing](https://github.com/ppmpreetham/BUI/blob/main/docs/macos-testing.md)             | How to try the experimental Cocoa host on a Mac                                                                                      |
| [Publishing](https://github.com/ppmpreetham/BUI/blob/main/docs/publishing.md)                   | Releasing to PyPI and crates.io                                                                                                      |
| [Changelog](https://github.com/ppmpreetham/BUI/blob/main/CHANGELOG.md)                          | What changed                                                                                                                         |

* * *

## Part V: Performance

## 29\. Before you read the numbers

These numbers were measured on a Ryzen 7 5800U laptop (Windows 11, on AC power) **before the latest features landed**: Dear ImGui, the Linux GPU renderer, the light theme, and the Blender search and expression code. They show the shape of things, not a promise for your machine. Rerun them with the tools in [Run the benchmarks yourself](https://github.com/ppmpreetham/BUI#33-run-the-benchmarks-yourself).

## 30\. Start time and battery

The same small settings window (a label, text field, checkbox, slider and button) written each of the five ways, from [`bench/apps`](https://github.com/ppmpreetham/BUI/blob/main/bench/apps), on BUI's CPU renderer (the default) and its GPU renderer (`BUI_GPU=1`). The last rows are one gpui-syntax source file, [`settings.rs`](https://github.com/ppmpreetham/BUI/blob/main/bindings/rust/bench/common/settings.rs), compiled unchanged against blenderui, Zed's [gpui](https://crates.io/crates/gpui) and [gpui-fast](https://github.com/longbridge/gpui-fast). gpui has no form widgets, so there the controls are styled divs.

Median of 10 launches, from process start until the first frame is on screen, with Python 3.14 and Node 22:

|                               | Renderer | Start time | CPU used to start | Memory (idle) | Idle CPU per minute |
| ----------------------------- | -------- | ---------- | ----------------- | ------------- | ------------------- |
| Rust (HTML + CSS)             | CPU      | **38 ms**  | **27 ms**         | **13 MB**     | **0 ms**            |
|                               | GPU      | 42 ms      | 61 ms             | 31 MB         | 1.8 ms              |
| JavaScript (React, Node.js)   | CPU      | 94 ms      | 100 ms            | 48 MB         | 68 ms               |
|                               | GPU      | 99 ms      | 131 ms            | 60 MB         | 71 ms               |
| Blender components (Python)   | CPU      | 112 ms     | 86 ms             | 23 MB         | 0 ms                |
|                               | GPU      | 116 ms     | 125 ms            | 32 MB         | 1.7 ms              |
| tkinter syntax (Python)       | CPU      | 139 ms     | 112 ms            | 25 MB         | 0.2 ms              |
|                               | GPU      | 159 ms     | 157 ms            | 43 MB         | 2.4 ms              |
| Qt syntax (Python)            | CPU      | 156 ms     | 123 ms            | 27 MB         | 0 ms                |
|                               | GPU      | 161 ms     | 160 ms            | 36 MB         | 1.2 ms              |
| **Same gpui code:** blenderui | CPU      | **40 ms**  | **27 ms**         | **15 MB**     | **0 ms**            |
| blenderui                     | GPU      | 43 ms      | 60 ms             | 31 MB         | 2.1 ms              |
| gpui                          | GPU      | 231 ms     | 226 ms            | 29 MB         | 743 ms              |
| gpui-fast                     | GPU      | 226 ms     | 217 ms            | 31 MB         | 2.5 ms              |

On the same code, blenderui shows its first frame about 5× sooner than gpui and gpui-fast, spends about 8× less CPU getting there, and on the CPU renderer uses half the memory. Idle, gpui keeps about 1.2 % of a core busy; blenderui uses none.

Laptops can't report energy per app, so battery cost is measured by the CPU time an app causes: the CPU it spends starting up, and the CPU it uses over 20 seconds while the window sits idle (what drains a battery while an app is open). BUI redraws only when something changes and otherwise sleeps, so idle use is near zero on either renderer. The GPU renderer starts a little slower and uses more memory (the GPU device), and pays off on large, animated scenes (below). Node's event loop accounts for the JavaScript idle figure, Python's times include starting the interpreter, and memory doesn't count GPU memory.

## 31\. Frame times

Python and C, on the full component demo (520×900):

| Frame                                     | Time      |
| ----------------------------------------- | --------- |
| Complete frame (everything redrawn)       | \~0.7 ms  |
| Hover or number-scrub frame (damage only) | \~0.05 ms |
| Native window: input event → on screen    | \~0.06 ms |

Dear ImGui's demo window with every section open (27k triangles, 1280×800) is a full software redraw of about 5–8 ms.

Rust, against Zed's [gpui](https://crates.io/crates/gpui) and the faster fork [gpui-fast](https://github.com/longbridge/gpui-fast). The scene is 3000 animated cells, and all apps ran in the same session (GPU rows on Direct3D 11):

|               | blenderui (GPU) | blenderui (CPU) | gpui-fast     | gpui          |
| ------------- | --------------- | --------------- | ------------- | ------------- |
| FPS           | **59.9**        | 57.6            | 31.5          | 19.9          |
| CPU per frame | **11.6 ms**     | 22.0 ms         | 31.9 ms + GPU | 50.5 ms + GPU |
| RAM           | 40 MB           | **30 MB**       | 65 MB         | 77 MB         |
| First frame   | **\~5 ms**      | **\~5 ms**      | \~200 ms      | \~220 ms      |
| Idle CPU      | **0 %**         | 0–0.9 %         | 0–0.4 %       | 0.9–1.4 %     |

Full results, the GPU renderer, and the method

|                                       | blenderui (GPU)     | blenderui (CPU) | gpui-fast           | gpui                |
| ------------------------------------- | ------------------- | --------------- | ------------------- | ------------------- |
| FPS, 1000 animated cells              | **60**              | **60**          | 49.2                | 36.4                |
| CPU per frame, 1000 / 100 cells       | **4.7 / 1.2 ms**    | 10.2 / 4.6 ms   | 20.5 / 2.7 ms + GPU | 27.7 / 4.7 ms + GPU |
| Uncapped FPS, 100 / 1000 / 3000 cells | **1491 / 309 / 94** | 649 / 202 / 76  | -                   | -                   |
| RAM, 1000 / 100 cells                 | 35 / 28 MB          | **24 / 21 MB**  | 45 / 47 MB          | 47 / 40 MB          |
| GPU memory                            | 19–22 MB            | **none**        | 19–24 MB            | 16–21 MB            |

The GPU renderer is off by default. Turn it on with `Application::new().gpu(true)`, `tk.Tk(gpu=True)`, `bui.set_gpu(True)` or `BUI_GPU=1`:

- Every primitive is one instance of a single shader, so a frame is a few instanced draw calls (Direct3D 11 on Windows, OpenGL 3.3 on Linux).
- Text and images are cached in atlases.
- Frames render into a persistent target, so only damaged regions redraw.
- On Windows, presentation uses a flip-model swap chain with dirty rects.

Its shaders evaluate the software raster's formulas: the parity test finds 10 of 172,800 pixels that differ by more than 3/255, on both backends.

gpui-fast's heavy-scene frame rate varies between sessions on this laptop (30–60 fps), so compare within one session. Method, raw data and how to rerun: [bindings/rust/bench/README.md](https://github.com/ppmpreetham/BUI/blob/main/bindings/rust/bench/README.md).

## 32\. JavaScript and React: startup and animation

**Startup.** These launch a small React app (the counter above) that quits once its first frame is on screen. Each figure is the median of 12 launches after a warm-up launch (`node bench/startup.js`).

|                                         | First frame | Launch to exit | RAM at first frame | What you ship   |
| --------------------------------------- | ----------- | -------------- | ------------------ | --------------- |
| **QuickJS executable** (`bui build`)    | **68 ms**   | **104 ms**     | **19 MB**          | **7.3 MB**      |
| Bun executable (`bui build --bun`)      | 79 ms       | 147 ms         | 35 MB              | 114 MB          |
| Bun (`bun app.js`)                      | 80 ms       | 152 ms         | 35 MB              | Bun + your app  |
| Node.js (`node app.js`)                 | 108 ms      | 187 ms         | 45 MB              | Node + your app |
| Rust (for reference: the benchmark app) | 5 ms        | \~92 ms        |                    | 3.5 MB          |

First frame is measured from the runtime's start. Launch to exit is wall time measured from outside, including process creation and teardown.

**Animation.** The same scene as the Rust table above, in `one` mode: one cell changes per frame, GPU renderer, medians of 3 runs. CPU is the whole process, counted with the cycle counter as for Rust.

- **Plain JS** uses `@blender-ui/native`.
- **React with signals** passes the changing values as signals, so the grid renders once.
- **React with state** re-renders the grid every frame.
- **React's production build** is used throughout.

| 3,000 cells                                               | FPS                | CPU per frame         | RAM                 |
| --------------------------------------------------------- | ------------------ | --------------------- | ------------------- |
| Rust                                                      | 59.6               | **12.0 ms**           | **43 MB**           |
| QuickJS: plain JS / React with signals / React with state | 59.1 / 58.7 / 19.7 | 16.3 / 14.9 / 51.0 ms | **56 / 61 / 67 MB** |
| Bun 1.4: plain JS / React with signals / React with state | 58.8 / 58.4 / 55.8 | 13.1 / 13.3 / 20.8 ms | 80 / 85 / 118 MB    |
| Node 22: plain JS / React with signals / React with state | 55.8 / 56.7 / 54.8 | 14.4 / 14.0 / 18.9 ms | 88 / 93 / 121 MB    |

| 1,000 cells                                               | FPS            | CPU per frame       | RAM                 |
| --------------------------------------------------------- | -------------- | ------------------- | ------------------- |
| Rust                                                      | 60             | **4.8 ms**          | **32 MB**           |
| QuickJS: plain JS / React with signals / React with state | 60 / 60 / 56.8 | 6.0 / 5.6 / 17.8 ms | **38 / 40 / 43 MB** |
| Bun: plain JS / React with signals / React with state     | 60 / 60 / 60   | 5.2 / 5.2 / 7.5 ms  | 58 / 63 / 81 MB     |
| Node: plain JS / React with signals / React with state    | 60 / 60 / 60   | 5.6 / 5.6 / 6.9 ms  | 68 / 71 / 97 MB     |

What this means:

- **JavaScript costs little over Rust:** 0.4–2.4 ms per frame, for sending changes to BUI's UI thread.
- **Signals make React as cheap as the plain API:** the component doesn't re-render.
- **QuickJS** starts fastest and uses the least memory, from a 7 MB file. It has no JIT, so re-rendering a 3,000-element list every frame drops it to 20 FPS. Keep big lists on signals, or build with `--bun`.

**React and Svelte, side by side** (one session, runs interleaved, medians of 3; `--svelte` in the commands below):

| 3,000 cells, one session (Node 26 / Bun 1.4 / QuickJS) | CPU per frame         | RAM              | JavaScript per update |
| ------------------------------------------------------ | --------------------- | ---------------- | --------------------- |
| Plain JS                                               | 37.0 / 37.4 / 38.6 ms | 92 / 81 / 59 MB  | 14 / 13 / 28 µs       |
| React with signals                                     | 37.6 / 38.4 / 37.8 ms | 100 / 87 / 60 MB | 23 / 17 / 40 µs       |
| Svelte with signals (as values)                        | 39.0 / 36.8 / 38.5 ms | 116 / 97 / 75 MB | 17 / 19 / 36 µs       |
| Svelte with runes                                      | 39.4 / 35.9 / 37.0 ms | 117 / 98 / 75 MB | 65 / 76 / 790 µs      |
| React with state                                       | 38.1 ms on Node       | 166 MB on Node   | 5.2 / 8.6 / 76 ms     |

| First frame, median of 12        | React  | Svelte | Svelte before | RAM (React / Svelte) |
| -------------------------------- | ------ | ------ | ------------- | -------------------- |
| QuickJS executable (`bui build`) | 93 ms  | 89 ms  | 81 ms         | 20.4 / 20.4 MB       |
| Bun executable (`--bun`)         | 106 ms | 90 ms  | 94 ms         | 37.8 / 38.8 MB       |
| Bun (`--preload …/register`)     | 107 ms | 121 ms | 203 ms        | 36.5 / 40.2 MB       |
| Node.js (`--import …/register`)  | 125 ms | 135 ms | 393 ms        | 52.1 / 55.4 MB       |

- **Every variant ran at 26–29 FPS** in this session (the machine, not the code), so CPU per frame is mostly BUI's own rendering and differs within noise. "JavaScript per update" is the cost of one update alone: 500 back to back, each applied by the framework and sent to the UI thread.
- **Svelte with signals costs what React with signals does:** a BUI signal passed as a value (`{label}`, `style={style}`) binds its node, so an update skips Svelte. Before this release's work it went through Svelte as a store: 86–87 µs on Node.js and Bun, 793 µs on QuickJS.
- **Svelte with runes** pays Svelte's flush, which walks the component's effects (3,000 here): 65–76 µs, 790 µs on QuickJS (85 / 78 / 790 before).
- **Memory:** Svelte keeps 10–17 MB more than React with signals (6 MB less than before on Node). A heap snapshot puts the rest in Svelte's own effects, closures and each-block items, about 1.9 KB per cell; BUI's nodes are the same for both.
- **Startup:** `register` serves Svelte's runtime bundled (once, with Bun, into `node_modules/.cache`), which brought Node.js from 393 to 135 ms and Bun from 203 to 121 ms. React and Svelte ran one after the other here.
- **React with state** on Bun and QuickJS: the 500 back-to-back re-renders overran the warm-up, so only its per-update cost is valid from this session. Bun also sometimes crashes when a React app quits (after its first frame), a bug that predates this work.

Method, the memory breakdown and how to rerun: [docs/javascript.md](https://github.com/ppmpreetham/BUI/blob/main/docs/javascript.md#performance).

## 33\. Run the benchmarks yourself

Every number above comes from a tool in this repo. Build in release mode, close other heavy apps, and plug the laptop in. Run one benchmark (and one build) at a time.

**Start time and battery: the five ways and gpui** (`bench/`, Windows). The same settings window written each way, on the CPU and the GPU renderer, plus one gpui-syntax file compiled for blenderui, gpui and gpui-fast. It reports start time, CPU used to start, memory and idle CPU, and writes `bench/results.json`.

```shell
cmake -S . -B build && cmake --build build --config Release --target buipy  # Python extension
cd bindings/node && npm run build && cd ../..                                # Node addon
cd bindings/rust && cargo build --release --example bench_app && cd ../..    # Rust app
cd bindings/rust/bench && cargo build --release --bins && cd ../../..        # gpui comparison
py -3 bench/run.py 10 20        # launches per app, seconds of idle measurement
```

**Frame cost: 100, 1,000 and 3,000 animated cells, against gpui and gpui-fast** (`bindings/rust/bench/`, Windows). A 1000×800 window filled with N rounded, numbered cells, in three modes:

- **anim:** every cell changes every frame, the worst case.
- **one:** one cell changes per frame, like a hover or a ticking value.
- **idle:** nothing changes.

Each app measures its own CPU cycles, frames and memory, and `run.py` adds GPU utilisation and GPU memory from Windows' performance counters.

```shell
cd bindings/rust/bench
set GPUI_FXC_PATH=C:\Program Files (x86)\Windows Kits\10\bin\10.0.19041.0\x64\fxc.exe   # gpui needs fxc.exe
cargo build --release
py run.py --reps 3                                   # all apps, 100 / 1000 / 3000 cells, about 12 minutes
py run.py --cells 3000 --apps blenderui,blenderui-gpu --reps 1
target\release\bui-bench --cells 3000 --mode anim   # one run by hand (--mode one / idle, --uncapped)
set BUI_GPU=1 && target\release\bui-bench --cells 3000 --mode anim   # on the GPU renderer
```

Method and full results: [bindings/rust/bench/README.md](https://github.com/ppmpreetham/BUI/blob/main/bindings/rust/bench/README.md).

**JavaScript: the same cells, and startup** (`bindings/node/bench/`):

```shell
cd bindings/node
node bench/cells.js 3000                       # DOM-like API, one cell changing per frame
node bench/cells.js 3000 --react               # React re-rendering the grid
node bench/cells.js 3000 --react --signals     # React with signals: no re-render
node --import ../svelte/register.js bench/cells.js 3000 --svelte   # Svelte, runes
node bench/cells.js 3000 --gpu                 # on the GPU renderer
bun bench/cells.js 3000                        # on Bun
node cli.js run bench/cells.js -- 3000         # on the 5 MB QuickJS runtime
node bench/startup.js                          # launch to first frame: Node, Bun and both executables
node bench/startup.js --svelte                 # the same for the Svelte app
```

**tkinter: canvas frame cost** (headless, any platform):

```shell
py bindings/python/bench/tk_canvas_bench.py 3000 all    # move 3,000 canvas items every frame
py bindings/python/bench/tk_canvas_bench.py 3000 one    # move one
```

**Inside any app:** run it with `BUI_PROFILE=1` to print per-phase frame times (layout, paint, raster, present) every 60 frames; any value turns it on. Use `BUI_GPU=1` / `BUI_GPU=0` to pick the renderer. More: [Profiling](https://bui.preetham.top/docs/performance/profiling/).

* * *

## Part VI: Reference

## 34\. Platforms

|                              | Windows                          | Linux                                                                    | macOS                        |
| ---------------------------- | -------------------------------- | ------------------------------------------------------------------------ | ---------------------------- |
| Window                       | Native (Win32)                   | Native (X11; Wayland through XWayland)                                   | Native (Cocoa), experimental |
| Python (`pip install`)       | Works: wheels                    | Works: wheels (native X11 host)                                          | Works: wheels                |
| Rust (`cargo add blenderui`) | Works                            | Works                                                                    | Experimental                 |
| JavaScript / React           | Works: prebuilt on npm           | Works: build the addon from source (`npm run build`); native X11 windows | Headless (see below)         |
| C, C++, Dear ImGui           | Works                            | Works                                                                    | Experimental                 |
| Renderer                     | CPU raster, or GPU (Direct3D 11) | CPU raster, or GPU (OpenGL 3.3)                                          | CPU raster                   |
| HiDPI                        | Per-monitor DPI                  | `Xft.dpi`                                                                | Retina (backing scale)       |

**Works** means it works and is tested. **Experimental** means it's written but has had little hands-on testing on real hardware (macOS: one Apple Silicon Mac so far, plus CI).

**macOS.** The macOS window host is new: it's a port of Blender's Cocoa code (GHOST). So far it has been checked by CI and tried by hand on one Apple Silicon Mac by [Krish Kalaria](https://github.com/krishkalaria12), who reported issue #3 and fixed it in pull request #4. If you have another one, please try it and tell us what happens: [docs/macos-testing.md](https://github.com/ppmpreetham/BUI/blob/main/docs/macos-testing.md). Python can still use the stdlib Tk window there (`BUI_HOST=tk`; this needs Python's `tkinter`, for example from python.org or `brew install python-tk`). Node runs BUI on its own UI thread and macOS only allows windows on the main thread, so JavaScript stays headless on macOS for now.

**Linux.** libX11 and libGL are loaded when the first window opens, so BUI also imports and renders headless on servers and in containers without them, and falls back to the CPU raster where OpenGL is missing. Building from source needs the X11 headers (`libx11-dev` / `libX11-devel`); no GL headers are needed. The C, Python, window and GPU parity tests pass on Linux, on Mesa's software OpenGL and on an NVIDIA GPU.

**Packages:**

| Package                                                                | Platforms                                                           | Notes                                                                           |
| ---------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| `blenderui` on PyPI                                                    | Windows x86\_64, Linux x86\_64 (manylinux), macOS arm64 and x86\_64 | CPython 3.11 to 3.14. No musllinux or 32-bit wheels.                            |
| `blenderui` on crates.io                                               | Any platform with CMake and a C compiler                            | Rust 1.88+. `blenderui-imgui` (Dear ImGui) is on crates.io too.                 |
| `@blender-ui/native`, `@blender-ui/react`, `@blender-ui/svelte` on npm | Windows x64                                                         | Node.js 18+ (22.15+ for Svelte) or Bun. Elsewhere, build the addon from source. |
| C and C++ (CMake)                                                      | Windows, Linux, macOS                                               | CMake 3.20+, a C11 and C++20 compiler.                                          |

## 35\. Environment variables

Most are read by the C library, so they work the same from every language. On/off variables look at the first character: `1`, `y`, `Y`, `t` or `T` is on. Where a variable and an app-wide API call set the same thing, the variable wins.

| Variable                         | What it does                                                                                                                                                                                                                 |
| -------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `BUI_GPU`                        | `1` draws native windows on the GPU (Direct3D 11 / OpenGL 3.3). Overrides `bui.set_gpu()`, `Application::gpu()`, `BUI_gpu_set_default()`. `0` turns those defaults off; a window that asks for the GPU itself still gets it. |
| `BUI_SLUG`                       | GPU text: `1` from glyph outlines (Slug, the default), `0` from a per-glyph atlas.                                                                                                                                           |
| `BUI_GPU_DEBUG`                  | `1` prints why the GPU renderer couldn't start.                                                                                                                                                                              |
| `BUI_GPU_WARP`                   | `1` (Windows) creates the Direct3D 11 device on WARP, the software rasterizer. Handy in VMs and CI.                                                                                                                          |
| `BUI_GPU_MAX_TEXTURE`            | Testing: the largest texture side the GPU renderers give one image (at least 64), so images over it are tiled as on devices with a small limit.                                                                              |
| `BUI_GPU_EGL`                    | `default` or `surfaceless` (Linux): the EGL platform for offscreen GPU rendering.                                                                                                                                            |
| `BUI_RASTER_THREADS`             | 1 to 8 (Windows): the most threads the software raster uses for big canvases.                                                                                                                                                |
| `BUI_PROFILE`                    | Prints frame times every 60 frames. **Any** value turns it on, even `0`.                                                                                                                                                     |
| `BUI_THEME`                      | `dark` (default) or `light`: Blender's theme, when the app doesn't set one.                                                                                                                                                  |
| `BUI_UI_SCALE`                   | A fixed UI scale (`1.25`, `1.5` ...) instead of the display's. Clamped to 0.5–4.                                                                                                                                             |
| `BUI_HOST`                       | `tk` (Python): use the stdlib Tk window even where a native host exists.                                                                                                                                                     |
| `BUI_MAC_TK`                     | `0` (Python on macOS): skip starting a hidden Tk root before the first native window.                                                                                                                                        |
| `BUI_FONT`, `BUI_FONT_DIR`       | A font file to replace Inter, and an extra font folder to search first.                                                                                                                                                      |
| `BUI_FILEDIALOG`                 | `native` (or `system`, `tk`), Python: file dialogs use the OS's instead of Blender's File Browser.                                                                                                                           |
| `BUI_BOOKMARKS`                  | The File Browser's bookmarks file.                                                                                                                                                                                           |
| `BUIPY_BUILD_DIR`                | Running Python from a source checkout: the folder holding the built `buipy` extension.                                                                                                                                       |
| `BUI_NODE_ADDON`                 | `@blender-ui/native`: load this `.node` addon instead of the bundled one.                                                                                                                                                    |
| `BUI_HEADLESS`                   | `1`: `bui build` apps and `bui run` render off-screen (same as `--headless`).                                                                                                                                                |
| `BUI_SOURCE_DIR`, `BUI_TEXT_GDI` | Build time, Rust: build the C library from this checkout; use GDI text.                                                                                                                                                      |

Every variable, with defaults and where it's read: [Environment variables](https://bui.preetham.top/docs/reference/environment-variables/) and [docs/getting-started.md](https://github.com/ppmpreetham/BUI/blob/main/docs/getting-started.md#environment-variables).

## 36\. Building from source and CMake options

You need **CMake 3.20+**, a **C11 / C++20 compiler** (MSVC, GCC/MinGW or Clang) and **Python 3.11+** with headers. On Linux, also the X11 headers.

```shell
git clone https://github.com/ppmpreetham/BUI && cd BUI
cmake -S . -B build && cmake --build build -j 2
ctest --test-dir build                      # C, C++, ImGui and Python test suites

export PYTHONPATH=$PWD/bindings/python      # Windows: set PYTHONPATH=%CD%\bindings\python
python examples/components_demo.py

cd bindings/rust && cargo test              # Rust: CSS, Tailwind, rsx!, hooks, effects, headless windows
```

| Option                      | Default                  | What it does                                                                                        |
| --------------------------- | ------------------------ | --------------------------------------------------------------------------------------------------- |
| `BUI_BUILD_TESTS`           | `ON`                     | C test programs and the `ctest` suites                                                              |
| `BUI_BUILD_EXAMPLES`        | `OFF`                    | The C examples (`bui_kit_hello`, `bui_svg_demo`, the site screenshot programs)                      |
| `BUI_BUILD_PYTHON_BINDINGS` | `ON`                     | The `buipy` extension (and the ImGui extension); skipped without Python's dev files                 |
| `BUI_BUILD_IMGUI`           | `ON` with a C++ compiler | Dear ImGui on BUI, its C API, demo and test                                                         |
| `BUI_IMGUI_BLENDER_WIDGETS` | `OFF`                    | Dear ImGui with Blender's widgets                                                                   |
| `BUI_BUILD_QT_BACKEND`      | `OFF`                    | The C++ Qt/QPainter render-sink backend (needs Qt 5 or 6)                                           |
| `BUI_BUILD_UI`              | `OFF`                    | Also compile Blender's original `ui/` sources (reference only)                                      |
| `BUI_TEXT_FREETYPE`         | `ON`                     | Render glyphs with FreeType like Blender's BLF; `OFF` uses stb\_truetype                            |
| `BUI_TEXT_GDI`              | `OFF`                    | Windows' GDI text and system fonts                                                                  |
| `BUI_SLUG`                  | `ON`                     | Slug as the GPU's default text method (switchable at run time)                                      |
| `BUI_IMAGE_SVG`             | `ON`                     | SVG images through the kit's nanosvg (no extra code: Blender's icons already use it)                |
| `BUI_WARNINGS_AS_ERRORS`    | `OFF`                    | GCC and Clang only: when `OFF` the build adds `-Wno-error`; it never adds `-Werror` or `/WX` itself |

Rebuild the docs screenshots with `python tools/gen_doc_images.py` (and the website's dark and light shots with `--site`). More: [CMake options](https://bui.preetham.top/docs/reference/cmake-options/).

## 37\. Coverage

| API                               | Coverage                                                                                                                      | Status   |
| --------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- | -------- |
| tkinter and ttk                   | **7,846 of 7,846** public names; nine real programs behave identically                                                        | Complete |
| Qt syntax                         | **About 220 of 920** public method names on 25 core widget classes (~24%)                                                     | Partial  |
| Blender components (Python)       | **45** components (10 of them also in Rust and JavaScript)                                                                    | Complete |
| HTML elements (Rust, JavaScript)  | **113 of 113**                                                                                                                | Complete |
| CSS properties (Rust, JavaScript) | **261 of 745** W3C properties render, **129** more parse, **355** not recognized (mostly drafts, speech, paged media and SVG) | Partial  |
| CSS 2.1 screen properties         | **90 of 90**                                                                                                                  | Complete |
| Flexbox / Grid                    | **12 of 12** / **23 of 23**                                                                                                   | Complete |
| Dear ImGui, C                     | **399 of 399** functions                                                                                                      | Complete |
| Dear ImGui, Python and safe Rust  | **366 of 399** functions (Rust FFI: 385)                                                                                      | Partial  |
| Image formats (every language)    | PNG, JPEG, WebP, GIF, BMP, TGA, QOI, HDR, PNM, SVG; not yet AVIF, EXR, TIFF, JPEG XL, HEIC                                    | Partial  |

How each was counted: [Coverage](https://bui.preetham.top/docs/reference/coverage/), [SYNTAX\_COVERAGE.md](https://github.com/ppmpreetham/BUI/blob/main/SYNTAX_COVERAGE.md), [docs/rust.md](https://github.com/ppmpreetham/BUI/blob/main/docs/rust.md#coverage), [docs/imgui.md](https://github.com/ppmpreetham/BUI/blob/main/docs/imgui.md#coverage).

## 38\. Repository layout

| Path                                                    | Contents                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| ------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `kit/`                                                  | The C runtime: nodes, input, widget drawing (the `interface_widgets.cc` port), menus, geometry managers, canvas, the File Browser, themes (`kit_theme_presets.c`), the Win32, X11 and Cocoa window hosts; `kit/intern/image/` (images: decoders, cache, worker pool; third-party decoders in `kit/extern/`); `kit/intern/blender/` holds Blender files copied in (string search, text-edit undo, float precision). API: `kit/include/BUI_kit.h` |
| `gpu/`                                                  | Software raster, GPU renderer on Direct3D 11 (`gpu_d3d11.c`, `shaders/`) and OpenGL 3.3 (`gpu_gl.c`), textured meshes, Slug text, text engine (FreeType or stb\_truetype + embedded Inter, or GDI)                                                                                                                                                                                                                                              |
| `core/`, `types/`                                       | Blender's `blenlib` subset (including `string_search.cc` and `expr_pylike_eval.cc`) and data types                                                                                                                                                                                                                                                                                                                                              |
| `ui/`, `compat/`                                        | Blender's original interface sources, kept for reference (not compiled by default; `kit/` re-ports them in C), and the seams replacing Blender's application APIs                                                                                                                                                                                                                                                                               |
| `api/`                                                  | Stable C ABI for render-sink hosts                                                                                                                                                                                                                                                                                                                                                                                                              |
| `bindings/python/`                                      | The `buipy` extension and the `bui` / `blenderui` packages (tkinter, Qt, components, ImGui)                                                                                                                                                                                                                                                                                                                                                     |
| `bindings/rust/`                                        | `blenderui`, `blenderui-sys`, `blenderui-macros`, `blenderui-tailwind`, `blenderui-imgui`, `blenderui-jsview`, `blenderui-node`, `blenderui-qjs`, and the benchmark                                                                                                                                                                                                                                                                             |
| `bindings/node/`, `bindings/react/`, `bindings/svelte/` | `@blender-ui/native`, `@blender-ui/react` and `@blender-ui/svelte`                                                                                                                                                                                                                                                                                                                                                                              |
| `bindings/imgui/`                                       | Dear ImGui (vendored), the BUI backend, Blender style, the C API and its generator, the Blender-widgets build                                                                                                                                                                                                                                                                                                                                   |
| `backends/qt/`                                          | C++ Qt/QPainter render-sink backend                                                                                                                                                                                                                                                                                                                                                                                                             |
| `bench/`                                                | The start time and battery benchmark; `bench/images/` (decoders, thumbnail grid)                                                                                                                                                                                                                                                                                                                                                                |
| `examples/`, `tools/`                                   | Demos, the website's screenshot programs (`examples/site/`), the icon and theme generators, the docs screenshot generator                                                                                                                                                                                                                                                                                                                       |
| `website/`                                              | The documentation website, [bui.preetham.top](https://bui.preetham.top)                                                                                                                                                                                                                                                                                                                                                                        |

## 39\. Examples

| File                                                                                                                                                                                                                                                                                                                                                                    | Shows                                                                                                                                                                                                                                                           |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [`examples/tk_unchanged_app.py`](https://github.com/ppmpreetham/BUI/blob/main/examples/tk_unchanged_app.py)                                                                                                                                                                                                                                                            | A plain tkinter app: menus, notebook, form, table, text, canvas                                                                                                                                                                                                 |
| [`examples/components_demo.py`](https://github.com/ppmpreetham/BUI/blob/main/examples/components_demo.py)                                                                                                                                                                                                                                                              | Every Blender component in a properties-editor layout                                                                                                                                                                                                           |
| [`examples/qt_syntax_demo.py`](https://github.com/ppmpreetham/BUI/blob/main/examples/qt_syntax_demo.py)                                                                                                                                                                                                                                                                | Qt syntax: layouts, form widgets, signals                                                                                                                                                                                                                       |
| [`examples/tk_syntax_demo.py`](https://github.com/ppmpreetham/BUI/blob/main/examples/tk_syntax_demo.py)                                                                                                                                                                                                                                                                | tkinter syntax laid out like a Blender properties editor                                                                                                                                                                                                        |
| [`examples/kit_hello.c`](https://github.com/ppmpreetham/BUI/blob/main/examples/kit_hello.c)                                                                                                                                                                                                                                                                            | The C kit: a slider and a button in a native window                                                                                                                                                                                                             |
| [`examples/imgui_demo.cpp`](https://github.com/ppmpreetham/BUI/blob/main/examples/imgui_demo.cpp)                                                                                                                                                                                                                                                                      | Dear ImGui's demo window on BUI, from C++                                                                                                                                                                                                                       |
| [`examples/imgui_demo_c.c`](https://github.com/ppmpreetham/BUI/blob/main/examples/imgui_demo_c.c)                                                                                                                                                                                                                                                                      | The same from C (`cimgui.h`)                                                                                                                                                                                                                                    |
| [`examples/imgui_blender_compare.cpp`](https://github.com/ppmpreetham/BUI/blob/main/examples/imgui_blender_compare.cpp)                                                                                                                                                                                                                                                | ImGui widgets next to BUI's native Blender widgets                                                                                                                                                                                                              |
| [`examples/site/`](https://github.com/ppmpreetham/BUI/blob/main/examples/site)                                                                                                                                                                                                                                                                                         | The website's screenshot programs: a Render Properties editor and a node editor (Python components), tkinter and ttk dialogs, a Qt material editor, an ImGui inspector (C++ and Python), a C modifier stack, and a JavaScript task list (DOM, React and Svelte) |
| [`hello.rs`](https://github.com/ppmpreetham/BUI/blob/main/bindings/rust/blenderui/examples/hello.rs)                                                                                                                                                                                                                                                                   | Rust: a component, hooks, Tailwind attributes and a Blender slider                                                                                                                                                                                              |
| [`showcase.rs`](https://github.com/ppmpreetham/BUI/blob/main/bindings/rust/blenderui/examples/showcase.rs)                                                                                                                                                                                                                                                             | Rust: HTML, a stylesheet, flex/grid, tables, Blender form widgets                                                                                                                                                                                               |
| [`forms.rs`](https://github.com/ppmpreetham/BUI/blob/main/bindings/rust/blenderui/examples/forms.rs)                                                                                                                                                                                                                                                                   | Rust: form controls as plain HTML, inside `styled`, and as Blender components                                                                                                                                                                                   |
| [`site_dashboard.rs`](https://github.com/ppmpreetham/BUI/blob/main/bindings/rust/blenderui/examples/site_dashboard.rs), [`site_dashboard_builder.rs`](https://github.com/ppmpreetham/BUI/blob/main/bindings/rust/blenderui/examples/site_dashboard_builder.rs)                                                                                                        | Rust: the same dashboard in `rsx!` and in gpui builders                                                                                                                                                                                                         |
| [`site_effects.rs`](https://github.com/ppmpreetham/BUI/blob/main/bindings/rust/blenderui/examples/site_effects.rs), [`site_grid.rs`](https://github.com/ppmpreetham/BUI/blob/main/bindings/rust/blenderui/examples/site_grid.rs), [`site_animation.rs`](https://github.com/ppmpreetham/BUI/blob/main/bindings/rust/blenderui/examples/site_animation.rs)             | Rust: CSS effects, grid and `@keyframes`                                                                                                                                                                                                                        |
| [`site_plain_html.rs`](https://github.com/ppmpreetham/BUI/blob/main/bindings/rust/blenderui/examples/site_plain_html.rs), [`site_styled.rs`](https://github.com/ppmpreetham/BUI/blob/main/bindings/rust/blenderui/examples/site_styled.rs), [`site_components.rs`](https://github.com/ppmpreetham/BUI/blob/main/bindings/rust/blenderui/examples/site_components.rs) | Rust: the three looks                                                                                                                                                                                                                                           |
| [`blenderui-imgui` demo](https://github.com/ppmpreetham/BUI/blob/main/bindings/rust/blenderui-imgui/examples/demo.rs)                                                                                                                                                                                                                                                  | Dear ImGui from Rust (`cargo run -p blenderui-imgui --example demo`)                                                                                                                                                                                            |
| [`bindings/node/examples/counter.js`](https://github.com/ppmpreetham/BUI/blob/main/bindings/node/examples/counter.js)                                                                                                                                                                                                                                                  | JavaScript: the DOM-like API                                                                                                                                                                                                                                    |
| [`bindings/react/examples/counter.jsx`](https://github.com/ppmpreetham/BUI/blob/main/bindings/react/examples/counter.jsx)                                                                                                                                                                                                                                              | React in JSX (`bun examples/counter.jsx`)                                                                                                                                                                                                                       |
| [`bindings/svelte/examples/Counter.svelte`](https://github.com/ppmpreetham/BUI/blob/main/bindings/svelte/examples/Counter.svelte)                                                                                                                                                                                                                                      | Svelte 5 (`node --import ./bindings/svelte/register.js bindings/svelte/examples/counter.js`)                                                                                                                                                                    |

* * *

## Part VII: The project

## 40\. Status and roadmap

**Done**

| Area               | What works                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| C runtime (`kit/`) | Nodes, Tk-style geometry managers, input, Blender widget drawing, damage rendering, undo, animation, text area, canvas, scroll views, Blender's string search, expression evaluator, text-edit undo and float precision, Blender's Dark and Light themes                                                                                                                                                                                                                                                                                                     |
| Native windows     | Windows (Win32) and Linux (X11; Wayland through XWayland), in C: DPI-aware, frame-paced animation, clipboard, cursors, window-manager features                                                                                                                                                                                                                                                                                                                                                                                                               |
| tkinter syntax     | **Complete**: every public tkinter and ttk name (7,846 of 7,846)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| Components         | 45 Blender components, including the color ramp, curve and node editors                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| Rust               | HTML (113/113 elements), Flexbox 12/12, Grid 23/23, CSS 2.1 90/90, math functions, `var()`, CSS Color 4/5, Selectors 4, `@layer` / `@media` / `@supports` / `@scope` / `@container` / `@font-face` / `@property`, `@keyframes` animations and transitions, filters / blend modes / masks / `clip-path` / 2D transforms / `border-image`, bidi text, background images, `object-fit`, Tailwind v4, `rsx!`, `#[component]` with optional props and `key`, hooks, Blender components, browser-look form controls, headless testing                              |
| Dear ImGui         | The real Dear ImGui 1.93 on BUI (CPU, Direct3D 11, OpenGL), Blender theme and Inter; C++ / C (399/399 functions) / Python and Rust (366–385/399); the Blender-widgets build                                                                                                                                                                                                                                                                                                                                                                                  |
| Rendering          | Software raster (parallel, damage-tracked), GPU renderer (Direct3D 11 on Windows, OpenGL 3.3 on Linux), textured meshes, portable text engine (FreeType, as in Blender)                                                                                                                                                                                                                                                                                                                                                                                      |
| JavaScript / React | `@blender-ui/native`: a DOM-like API over napi-rs, with its own UI thread, only changes crossing the boundary, vsync-paced `requestAnimationFrame` and signals. `@blender-ui/react`: a React 19.3 renderer whose hooks (`useState`, `useMemo`, `useEffect` ...) hold signals that update nodes without re-renders. Both on Node.js and Bun, with headless testing. `bui build` makes standalone executables: about 7 MB with the embedded QuickJS runtime, or Bun's with `--bun` ([guide](https://github.com/ppmpreetham/BUI/blob/main/docs/javascript.md)) |
| Packaging          | [`blenderui` on PyPI](https://pypi.org/project/blenderui/), with a CI workflow for wheels on all platforms (Linux wheels with the native X11 host); [`blenderui` on crates.io](https://crates.io/crates/blenderui); [`@blender-ui/react`](https://www.npmjs.com/package/@blender-ui/react) and [`@blender-ui/native`](https://www.npmjs.com/package/@blender-ui/native) on npm                                                                                                                                                                           |
| Docs               | The website, [bui.preetham.top](https://bui.preetham.top)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |

**Partial or in progress**

| Area                                                   | What's missing                                                                                                                                                                                                                                                                                       |
| ------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Qt syntax (~24%)                                       | Model/view classes, docks, toolbars, splitters, Qt stylesheets ([details](https://github.com/ppmpreetham/BUI/blob/main/docs/qt.md#coverage))                                                                                                                                                        |
| CSS (261 of 745 W3C properties render, 129 more parse) | 3D transforms, OpenType font features, multi-column layout; hit testing ignores 2D transforms. Most of the remaining W3C list is drafts, speech, print and SVG ([breakdown](https://github.com/ppmpreetham/BUI/blob/main/docs/rust.md#coverage))                                                    |
| Text                                                   | No text wrapping around floats; right-to-left text is reordered (bidi) but not shaped (Arabic joining); no OpenType features; no per-glyph font fallback yet (characters missing from the font show as boxes); no IME                                                                                |
| Rust components                                        | No async resources (`use_resource`); no generic components                                                                                                                                                                                                                                           |
| Dear ImGui                                             | Python and safe Rust skip the `va_list`, `void*` and C-callback functions (reachable from C and the Rust FFI); with Blender widgets, window title bars, tooltips, plain popups, tables and the color picker keep ImGui's drawing                                                                     |
| JavaScript / React packages                            | npm packages are prebuilt for Windows x64 only (on Linux, build the addon from source); events reach JavaScript after BUI handled them (no native `preventDefault`), and `event.target` is the deepest *listening* element                                                                           |
| macOS                                                  | **Experimental.** The native Cocoa window host is written, checked by CI and tried by hand on one Apple Silicon Mac ([how to help](https://github.com/ppmpreetham/BUI/blob/main/docs/macos-testing.md)); JavaScript is headless there (Node's UI thread isn't the main thread); no GPU renderer yet |
| Themes                                                 | Dark and Light work; loading other Blender theme files doesn't yet                                                                                                                                                                                                                                   |

**Planned (coming soon)**

| Target                               | Plan                                                                                                                                                   |
| ------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **WebAssembly**                      | The kit in the browser: the raster blits to `<canvas>`, DOM events feed the kit; `@blender-ui/react` and the DOM-like API on top, unchanged            |
| **egui**                             | BUI panels inside egui, plus a Blender-styled theme                                                                                                    |
| **More from Blender's source**       | Per-glyph font fallback (BLF), IME, search-as-you-type popups, units in number fields, widget drag and drop                                            |
| **macOS GPU and JavaScript windows** | The GPU renderer on Metal (or the OpenGL backend), and Node windows on the main thread                                                                 |
| **More GPU backends**                | Vulkan / WebGPU drawing the same instance stream as Direct3D 11 and OpenGL                                                                             |
| **Qt parity**                        | Measure against PySide6, run the same programs on Qt and BUI, fill in the missing classes, then `install()` so `import PySide6` programs run unchanged |
| **More languages**                   | MyGo, C#, Go, Zig, Swift, Kotlin and Lua over the same C header; Godot, Unity and Bevy; Flutter                                                        |

Our roadmap has no dates on it. Like a good render, it's done when the noise goes away.

## 41\. FAQ

**Do I need Blender installed?**

No. BUI contains the parts of Blender's UI code it needs and builds on its own.

**Is it really my unchanged tkinter app?**

Yes. `blenderui-tk app.py` makes `import tkinter` return BUI's implementation, which covers every public tkinter and ttk name. Nine real programs are tested to behave identically on the standard library and on BUI. See [the limitations](https://github.com/ppmpreetham/BUI/blob/main/docs/tkinter.md#limitations) for the few differences.

**Is it a web browser in Rust?**

No. There's no browser engine and no network. BUI takes HTML elements and CSS as a *UI language*: they are laid out with [taffy](https://github.com/DioxusLabs/taffy) and drawn by BUI's engine, with Blender's widgets where you ask for them, in a native window. JavaScript is optional: the Rust crate needs none, and the JavaScript packages drive the same engine.

**Is the Dear ImGui support a re-implementation?**

No. It's the real Dear ImGui (vendored, MIT), running on a BUI backend, so every function behaves as upstream. The optional Blender-widgets build swaps only how ImGui's widgets are drawn and how they react to the mouse, by compiling an edited copy of `imgui_widgets.cpp`.

**How is it different from Electron, Electrobun or GPUIX?**

- **Electron** ships Chromium with every app: on the order of 100 MB or more.
- **Electrobun** runs on Bun and renders in the system's webview. It's small to download, but the UI is still a browser page.
- **GPUIX**, like BUI, renders React natively: React drives Zed's GPUI.

BUI also renders natively, with Blender's widgets, a much larger part of CSS, Tailwind, and hooks backed by signals. `bui build` makes a 5 MB executable that reaches its first frame in about 70 ms.

**Do the macros cost performance?**

`rsx!`, `#[component]`, `css!` and `tw!` expand at compile time into the same builder calls you would write by hand. Tailwind classes are compiled to CSS once, on first use. Hooks are a few nanoseconds per read or write.

**Can I change the colors?**

Yes. Switch between Blender's Dark and Light themes at run time ([Themes](https://github.com/ppmpreetham/BUI#22-themes-dark-and-light)), color individual widgets in every API, and use full CSS on plain HTML elements in Rust and JavaScript. Loading any other Blender theme file is coming soon.

**Does it run on my Mac?**

Probably, but nobody has checked yet: the Cocoa host is written and experimental. Python can always fall back to the Tk window (`BUI_HOST=tk`). If you try it, the notes in [docs/macos-testing.md](https://github.com/ppmpreetham/BUI/blob/main/docs/macos-testing.md) say what to report.

**Is it ready for production?**

Much of it is complete and tested (the C kit, tkinter, the components, Rust, Dear ImGui, JavaScript), and some parts are partial or experimental (Qt syntax, parts of CSS and text, macOS). [Status and roadmap](https://github.com/ppmpreetham/BUI#40-status-and-roadmap) shows which is which. Pin your version, and test on the platforms you ship.

**Who wrote it?**

The repository is built entirely using LLMs, with no human-written code. The drawing and interaction code is ported from Blender's sources, and the behavior is checked by tests: C, Python, Rust and JavaScript suites, the tkinter side-by-side programs and the GPU parity test.

**Why GPL?**

BUI is derived from Blender's source code, which is GPL-2.0-or-later, so BUI is too.

**Can I use BUI in a commercial app?**

The GPL allows commercial use. What it asks is that when you distribute a program that includes BUI, you distribute it under the GPL too, with its source code available. If that doesn't fit your project, talk to a lawyer before you ship. (This is a summary, not legal advice.)

## Star History

[![Star History Chart](https://camo.githubusercontent.com/6a35c3da354c3afe256cff36a946c93f280738a6f612fca76f2dcada90f1df71/68747470733a2f2f6170692e737461722d686973746f72792e636f6d2f63686172743f7265706f733d70706d707265657468616d2f62756926747970653d64617465266c6567656e643d626f74746f6d2d7269676874)](https://www.star-history.com/?repos=ppmpreetham%2Fbui&type=timeline&legend=bottom-right)

## 42\. Contributing and publishing

Contributions are welcome: bug reports, missing tkinter/Qt behavior, CSS features, platform hosts and docs. If you have a Mac, running the Cocoa host and reporting what happens is one of the most useful things you can do right now. Read [CONTRIBUTING.md](https://github.com/ppmpreetham/BUI/blob/main/CONTRIBUTING.md) first, and please follow the [Code of Conduct](https://github.com/ppmpreetham/BUI/blob/main/CODE_OF_CONDUCT.md). Report security issues privately, as [SECURITY.md](https://github.com/ppmpreetham/BUI/blob/main/SECURITY.md) describes.

Releases go to PyPI (`blenderui`, wheels built by CI on a `v*` tag), crates.io (the eight Rust crates, in dependency order) and npm (`@blender-ui/native`, `@blender-ui/react`). The steps are in [docs/publishing.md](https://github.com/ppmpreetham/BUI/blob/main/docs/publishing.md).

## 43\. License and credits

**License.** BUI is derived from Blender sources and is licensed **GPL-2.0-or-later**. The SPDX header is kept in every file. Third-party parts keep their own licenses:

| Part                                                                                    | What it does in BUI                                                                                             | License                                   |
| --------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- | ----------------------------------------- |
| [Blender](https://www.blender.org) sources                                             | Widget drawing, interaction rules, theme, icons, `blenlib`, GHOST window code, the File Browser and node editor | GPL-2.0-or-later                          |
| [Dear ImGui](https://github.com/ocornut/imgui) by Omar Cornut                          | The immediate-mode GUI that runs on BUI                                                                         | MIT                                       |
| [Inter](https://rsms.me/inter) by Rasmus Andersson                                     | The embedded UI font, as in Blender (`gpu/extern/Inter-LICENSE.txt`)                                            | SIL Open Font License 1.1                 |
| [FreeType](https://freetype.org) 2.13.3                                                | Glyph loading, hinting and rendering, like Blender's BLF                                                        | FreeType License or GPLv2 (dual-licensed) |
| [stb\_truetype](https://github.com/nothings/stb) by Sean Barrett                       | Font metrics and character maps, and the fallback rasterizer                                                    | Public domain                             |
| [NanoSVG](https://github.com/memononen/nanosvg) by Mikko Mononen, as edited by Blender | Rasterizing Blender's SVG icons                                                                                 | Zlib                                      |
| [Slug](https://github.com/EricLengyel/Slug) by Eric Lengyel                            | The GPU glyph coverage algorithm (shader code "Copyright 2017 by Eric Lengyel")                                 | MIT                                       |
| [Tailwind CSS](https://tailwindcss.com)                                                | The v4 default theme values behind the Tailwind support                                                         | MIT                                       |

The Rust and JavaScript bindings also depend on crates and packages such as taffy, napi-rs, React and QuickJS, each under its own license.

**Acknowledgments.** Thank you to:

- [Blender](https://www.blender.org) and its community: the UI this project is built from, including its GHOST window code and blenlib.
- [Dear ImGui](https://github.com/ocornut/imgui) by Omar Cornut: the immediate-mode GUI that runs on BUI.
- [taffy](https://github.com/DioxusLabs/taffy): flexbox, grid and block layout for the Rust crate.
- [Tailwind CSS](https://tailwindcss.com): the v4 theme behind the Tailwind support.
- [FreeType](https://freetype.org): glyph rendering, as in Blender.
- [stb\_truetype](https://github.com/nothings/stb): font metrics and the fallback rasterizer.
- [NanoSVG](https://github.com/memononen/nanosvg) by Mikko Mononen: the icon rasterizer, via Blender's edited copy.
- [Inter](https://rsms.me/inter) by Rasmus Andersson: the embedded UI font, as in Blender.
- [Slug](https://github.com/EricLengyel/Slug) by Eric Lengyel: the GPU glyph coverage algorithm.
- [gpui](https://github.com/zed-industries/zed), [zopra](https://github.com/ppmpreetham/zopra) and [Leptos](https://leptos.dev): the APIs the Rust syntax follows.

BUI is Blender's UI, minus the default cube. You're free to add your own.
