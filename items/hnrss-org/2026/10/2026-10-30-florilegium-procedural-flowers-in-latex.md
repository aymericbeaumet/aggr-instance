---
title: 'Florilegium: Procedural Flowers in LaTeX'
link: https://github.com/kevinraymond/florilegium
source: hnrss-org
published: 2026-10-30T15:48:12Z
updated: 2026-10-30T15:48:12Z
first_seen: 2026-10-30T22:55:36.634700404Z
authors:
- nanna
content: extracted
html: 2026-10-30-florilegium-procedural-flowers-in-latex.html
remote_preview:
  url: https://opengraph.githubassets.com/a420b9d213dfaccef49cd3ea6d367c692af73809e2bcf95698eda5a3dec37d44/kevinraymond/florilegium
  alt: Procedurally generated Delft-blue floral page margins for LuaLaTeX, drawn in Lua - kevinraymond/florilegium
---

## Origin

The [Reddit post](https://www.reddit.com/r/LaTeX/comments/1xixznw/how_do_you_make_these_flowers_in_latex/) asked how to make flowers like these in LaTeX. I don't know how the original was made, and this project doesn't try to reproduce it. It is one possible way, and the way I did it: hand-authored primitives with randomized parameters, painted in Lua, placed by a procedural layout whose "fill the unused space" part borrows a 1998 idea from the ornament literature. It gets a similar look.

## What it is

Procedurally generated Delft-blue floral page margins for LaTeX. No images, no imported paths, no external assets: every petal, leaf and stem is a parametric curve computed in Lua while the page is being written, painted like a watercolor, and written straight into the PDF. Every page gets its own garden.

[![six seeds](https://github.com/kevinraymond/florilegium/raw/main/gallery/seeds.gif)](https://github.com/kevinraymond/florilegium/blob/main/gallery/seeds.gif)

*Six seeds of the grown composition. Same code, different seed — and the garden fills whatever space the text leaves free.*

Two documents share one package, and a third shows what else it does:

|             | `florilegium.tex`                                                                                                                                                                     | `florilegium-grow.tex`                                                                                                                                                       |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| composition | **authored**: stems, leaves and flowers placed by hand, every parameter sampled                                                                                                       | **grown**: a plant grown to fit the page by a greedy space-filling loop                                                                                                      |
| adapts to   | nothing (tuned for A4, 68 mm margin)                                                                                                                                                  | the page layout, and the free space below the text on the last page                                                                                                          |
| preview     | [![authored](https://github.com/kevinraymond/florilegium/raw/main/gallery/preview-authored.png)](https://github.com/kevinraymond/florilegium/blob/main/gallery/preview-authored.png) | [![grown](https://github.com/kevinraymond/florilegium/raw/main/gallery/preview-grown.png)](https://github.com/kevinraymond/florilegium/blob/main/gallery/preview-grown.png) |

Both PDFs are self-describing: the body text is the write-up of how the margin is drawn.

[![last page of the grown document](https://github.com/kevinraymond/florilegium/raw/main/gallery/preview-grown-lastpage.png)](https://github.com/kevinraymond/florilegium/blob/main/gallery/preview-grown-lastpage.png)

*Last page of the grown document: the text ends early, so branches arch into the space below.*

[![detail](https://github.com/kevinraymond/florilegium/raw/main/gallery/detail.png)](https://github.com/kevinraymond/florilegium/blob/main/gallery/detail.png)

*300 dpi crop. Mottled washes with white streaks lifted out, lobed leaves with white veins, forget-me-not cymes, a pale ghost flower behind the stems.*

## Watch it grow

| as it grows                                                                                                                                                             | with the free space it fills                                                                                                                                                                                        |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [![seed 6 growing](https://github.com/kevinraymond/florilegium/raw/main/gallery/growth.gif)](https://github.com/kevinraymond/florilegium/blob/main/gallery/growth.gif) | [![seed 6 growing, with the free discs marked](https://github.com/kevinraymond/florilegium/raw/main/gallery/growth-explain.gif)](https://github.com/kevinraymond/florilegium/blob/main/gallery/growth-explain.gif) |

*Seed 6, from an empty margin to the finished page. The stems rise first, leaves open as the tips pass their nodes, and each stem ends in a flower. Then the greedy loop takes over: on the right, each dashed circle is the largest free disc it found, just before a branch grows to it. Big holes come first and slowly, small ones last and fast. The butterflies and bees fly in, the bird glides onto its twig, and the snail crawls up its stem until the last frame, which is exactly the page this seed always gives.*

The growth is recorded once, while the page is laid out: each stem is also drawn as prefixes of increasing length and each motif at a few smaller scales, replaying the same random draws. A frame then writes the pieces whose time has come, in their original painting order. `make grow-6` renders one seed this way (`build/grow-6.gif` and `.mp4`, about 45 seconds); add `EXPLAIN=1` for the dashed circles.

## The margin reads the text

[![the three pages of showcase.tex](https://github.com/kevinraymond/florilegium/raw/main/gallery/preview-showcase.png)](https://github.com/kevinraymond/florilegium/blob/main/gallery/preview-showcase.png)

*`showcase.tex`, "A year in the margin": spring on the first page, summer, autumn on the last. Each section describes the feature it shows.*

The grown margin can respond to the document around it:

- **Initials.** `\florilegiuminitial[5]{E}` sets the first letter of a paragraph in a tile five lines tall. The outline of the letter is read from an OpenType font (Latin Modern Bold by default) and painted like a petal; a vine passes over and under its strokes in turn, with leaves where they fit and flowers in the gaps, often in the letter's counters. The letter is also typeset invisibly, so the paragraph still copies as "Every".
- **Flowers at the headings.** With the option `sections`, every numbered section and subsection gets a flower in the margin beside its heading, painted in the copper of the accents, the way headings were once picked out in red.
- **A flower where the text asks.** `\florilegiumbloom` puts a flower beside the line it stands in; `\florilegiumbloom[lily]` asks for any motif of the garden. The marks are found in the page box when the page is shipped out, so this works in one run.
- **Seasons.** With the option `aging`, the garden ages from spring on the first page (mostly in bud, small pale leaves) to autumn on the last (seed heads, falling petals, a few copper leaves). It needs two runs, for the page count; `\florilegiumage{0.8}` sets an age by hand instead.

[![four initials](https://github.com/kevinraymond/florilegium/raw/main/gallery/initials.png)](https://github.com/kevinraymond/florilegium/blob/main/gallery/initials.png)

[![one seed through the seasons](https://github.com/kevinraymond/florilegium/raw/main/gallery/seasons.png)](https://github.com/kevinraymond/florilegium/blob/main/gallery/seasons.png)

*One seed in spring, early summer, late summer and autumn. The seasons change what grows, not where.*

## Visitors

[![detail with a bird and a snail](https://github.com/kevinraymond/florilegium/raw/main/gallery/detail-visitors.png)](https://github.com/kevinraymond/florilegium/blob/main/gallery/detail-visitors.png)

Most pages have visitors: a butterfly or two and bees in the open spaces, a snail crawling up one of the main stems, and on about one page in five a bird perched on a twig of its own. They come from a random stream of their own, so a page without them is exactly the page it would have been; the option `novisitors` leaves them out.

## Any ink

[![one seed in six inks](https://github.com/kevinraymond/florilegium/raw/main/gallery/inks.png)](https://github.com/kevinraymond/florilegium/blob/main/gallery/inks.png)

*The whole palette comes from one color, `ink`: Delft blue, sepia, green, crimson, violet, teal. The ramp from the darkest stroke to the palest wash is computed from that hue in OKLCH, so the light end never turns gray.*

## In PostScript

[![the two pages of postscript/florilegium.ps, seed 7](https://github.com/kevinraymond/florilegium/raw/main/gallery/preview-postscript.png)](https://github.com/kevinraymond/florilegium/blob/main/gallery/preview-postscript.png)

*`postscript/florilegium.ps`: the grown margin in pure PostScript, one self-contained file. Ghostscript, or a printer, computes every petal while it interprets the page, and the two pages of text explain how.*

The drawing core, every primitive and the growth loop are ported from `ink.lua`, `flora.lua` and `grow.lua`, procedure for procedure. It needs no TeX and no other file:

```shell
gs -dALLOWPSTRANSPARENCY -sDEVICE=pdfwrite -dSeed=7 -o garden.pdf postscript/florilegium.ps
make postscript             # the same -> build/postscript-7.pdf; SEED=12 for another garden
```

`-dSeed=N` picks the garden; without it the clock picks one, so every print is new. `-dAge=0` to `1` sets the season, and `-dNoVisitors` leaves out the butterflies, bees, snail and bird. Two pages take about 2.5 seconds. Much of the Lua maps directly: PostScript's own transformation matrix places each primitive (translate, rotate, scale), paths and Bézier curves are native, and procedures built at run time stand in for Lua's closures (a petal's half-width is a procedure handed to the outline builder). Four things don't map:

- **Layers.** The garden grows in one order but is painted back to front, so every fill and stroke is captured with `pathforall` into a list per layer, and the lists are played back at the end.
- **Transparency.** PostScript has none. Ghostscript provides it with `-dALLOWPSTRANSPARENCY`. Without that flag, or on a printer, each translucent fill is mixed with the color it is likely to land on and painted opaque. The result is flatter, but it's the same garden.
- **Randomness.** The generator is Park-Miller in 32-bit integer arithmetic, and PostScript reals are single precision, so seed 7 here is a different garden from seed 7 in LaTeX.
- **Text.** The file typesets its own two pages in Times. It knows where its headings are and where its text ends, so the copper flowers at the headings and the garden under the last line work as they do in LaTeX. It has no initials, animation or authored composition.

[![two prints of florilegium.ps, two different gardens](https://github.com/kevinraymond/florilegium/raw/main/gallery/printed.jpg)](https://github.com/kevinraymond/florilegium/blob/main/gallery/printed.jpg)

*The file sent twice to an Epson inkjet with `lp postscript/florilegium.ps`, with no seed given. The computer's print system runs the program again for each job, so each print takes its seed from the clock and grows a different garden. A printer gets the opaque version, as it has no transparency.*

## Build

LuaLaTeX, one pass, about a second a page (two passes for `showcase.tex`, whose seasons need the page count).

```shell
make                        # the three PDFs
make seed-7                 # one page from seed 7 -> build/seed-7.pdf
make gallery                # gallery/seeds.png + seeds.gif, inks.png, seasons.png (python3, Pillow, pdftoppm)
make grow-6                 # growth animation of seed 6 -> build/grow-6.gif + .mp4 (ffmpeg)
make grow-6 EXPLAIN=1       # the same with the free discs marked -> build/grow-6-explain.*
```

No TeX installed? Build the image once and run everything in it:

```shell
docker build -t florilegium-tex tools
make RUN=tools/in-docker
```

Or paste `florilegium.sty`, the six `.lua` files and one of the `.tex` files into an [Overleaf](https://www.overleaf.com) project and set the compiler to LuaLaTeX.

Use it in your own document:

```text-tex-latex
\usepackage[grown]{florilegium}      % or plain \usepackage{florilegium} for the authored one
% options for the grown one: sections (flowers at the headings), aging (two runs),
% novisitors (no butterflies, bees, snails or birds)
\definecolor{ink}{HTML}{5B2A1F}      % optional: the hue of the whole palette (this is sepia)
\definecolor{accent}{HTML}{B4662F}   % optional: the copper of the hairlines
...
\florilegiuminitial[4]{T}his paragraph starts with a decorated T.
... a flower in the margin beside this line\florilegiumbloom ...
\florilegiumage{0.8}                 % grown only: autumn from here on
\florilegiumtextend                  % grown only: at the very end of the text
```

A fixed seed instead of the page number: `lualatex "\def\florilegiumseed{7}\input{florilegium-grow.tex}"`.

Any primitive can be drawn on its own inside a TikZ picture, under the current scope's shift, rotation and scale:

```text-tex-latex
\begin{tikzpicture}
  \begin{scope}[shift={(2,2)},rotate=20]\florilegiumdraw{flower}{1.3}\end{scope}
  \begin{scope}[shift={(5,0)}]\florilegiumdraw{leaf}{3.0, {depth=0.6}}\end{scope}
  \begin{scope}[shift={(8,1)}]\florilegiumdraw{ghost}{"lily", {s=1.4}}\end{scope}
  \begin{scope}[shift={(11,1)}]\florilegiumdraw{butterfly}{1.5}\end{scope}
\end{tikzpicture}
```

## How it works

**Drawing** (`ink.lua`). All geometry is computed in Lua and written as PDF path operators into one literal node per page; TeX does no arithmetic. Primitives are drawn in a local frame under an affine transform, which is read from TikZ when a primitive is called inside a scope. Colors come from a tone ramp derived from the one document color `ink`: navy to mist at the hue of `ink`, interpolated in OKLCH so the light end stays blue instead of turning gray. Tapered strokes are two offset Béziers, not sampled ribbons.

**Primitives** (`flora.lua`). Painted in the order a watercolorist would: a flat wash, darker blotches multiplied over it, pigment pooling at the edges, streaks lifted out in white, a few fine veins, and almost no outlines. Broad petals and flowers, star lilies seen at a tilt, blossoms and florets, buds and bellflowers; lobed and entire leaves; forget-me-not cymes that curl like a scorpion's tail, branching sprigs, umbels, tendrils, seed heads; butterflies, bees, snails and a bird; copper sparkles, rosettes and the ivy-leaf fleuron whose stem becomes the head rule. Any of them can be painted as a pale ghost.

**Authored composition** (`authored.lua`). A plant grows from a root in the bottom-left corner: three stems through hand-picked control points, a fourth along the bottom of the page as the footer, leaves at chosen fractions of the stems, and every other motif on a branch that leaves its stem tangentially and arrives along the motif's axis — so every flower can be traced back to the root.

**Grown composition** (`grow.lua`), in the spirit of Wong, Zongker & Salesin, [*Computer-Generated Floral Ornament*](https://dl.acm.org/doi/10.1145/280814.280948) (SIGGRAPH 1998). The stems sway up the column as smooth noise of their height; leaves are hung at their nodes where there is room. Then, repeatedly: take the largest free disc within reach of a stem, choose a motif that fits it, and grow a branch to it from the nearest stem point; the branch becomes a stem in turn. Big holes are taken first, so flowers and lilies land early, sprays and leaves fill the middle, and the loop ends by tucking leaflets, buds and florets onto the stems. The end of the text is measured with `\pagetotal` in the same pass, so a short last page grows branches into the space below it with one compilation.

**Reading the page** (`florilegium.lua`). A heading or `\florilegiumbloom` leaves an invisible marker node in the text. When the page is shipped out, its box is walked node by node, adding up heights, depths and the glue as it was set, to find each marker's height on the page; the garden then gives those lines their flowers before any leaf is hung. An age reweights the motifs: buds and sprays of buds in spring, seed heads in autumn, when some leaves are painted with a ramp built from the accent instead of the ink.

**Initials** (`initial.lua`). The letter's outline comes from luaotfload's OpenType reader as Bézier segments. Flattened into polygons, it gives an even-odd inside test, and a grid of distances to the letter and the frame finds the largest gaps for the flowers. The vine is a Catmull-Rom curve across the tile; where it crosses the letter it goes over and under in turn, and the strands that go over are drawn again, clipped to the letter, with a thin halo of paper.

**Procedural ≠ random.** The seed is derived from the page number, so page *n* always gets garden *n*. Reproducibility is what makes the document stable enough to typeset.

See [CLAUDE.md](https://github.com/kevinraymond/florilegium/blob/main/CLAUDE.md) for a handoff-style map of the code, the tunables, and ideas not yet done.

## License

MIT.
