---
title: 'Faster than earcut: triangulating 3.7M map polygons in 489 ms'
link: https://globus.software/blog/2026/10/16/cutting-map-polygons/
source: hnrss-org
published: 2026-10-22T09:31:18Z
updated: 2026-10-22T09:31:18Z
first_seen: 2026-10-22T17:33:41.202070908Z
authors:
- molind
labels:
- glmap
- glmap,rendering,triangulation
- rendering
- triangulation
content: extracted
html: 2026-10-22-faster-than-earcut-triangulating-3-7m-map-polygons-in-489-ms.html
remote_preview:
  url: https://globus.software/img/social-card.webp
extra:
  content:interactive: true
---

We have published the polygon triangulator of our map engine as a small library, [earstrip](https://github.com/GLMap/earstrip), under the ISC license, the same one [earcut](https://github.com/mapbox/earcut) uses. It is one C++ header, and a WebAssembly build on [npm](https://www.npmjs.com/package/earstrip) that takes the same arguments as earcut. In C++ it is 14% faster than earcut.hpp, in the browser 1.4 to 2.1 times as fast as earcut.js, and the [comparison](https://globus.software/blog/2026/10/16/cutting-map-polygons/#in-the-browser) is near the end. This post is about how we got there.

A GPU fills triangles and nothing else. A map, on the other hand, is mostly polygons: buildings, parks, parking lots, the strip of grass between two tram tracks. Here are a few blocks of Śródmieście in Warsaw the way our renderer reads them from a map file, :

Before any of this gets a fill, every outline has to be cut into . Hover over a polygon, or tap it, to see what it turns into. These blocks hold 331 polygons with 5,151 vertices, and they become 4,514 triangles.

Zoom level 13 of our offline map of the Masovian Voivodeship, the region around Warsaw, has 3,740,425 polygons. On one core of an M4 Pro, the constrained Delaunay triangulator in our engine needs 2.93 seconds to cut all of them. This October I replaced it with ear clipping. To write this post I rebuilt the ear clipper one idea at a time, one commit per idea, and timed every commit on those same 3.7 million polygons, so each section below ends with a number.

## Delaunay

A triangulation of a set of points is Delaunay when no point lies inside the circle through the three corners of any triangle. Two triangles that share an edge are enough to see how the rule works. Drag the point D:

Smallest angle 45°; with BD instead it would be 37°

While D stays outside the dashed circle through A, B and C, the shared edge is AC. and the edge flips to BD; and it flips back. Each flip is a local repair, and a triangulation where no edge wants to flip is Delaunay. Of all the ways to triangulate the same points, it has the largest smallest angle. The orange arc marks the smallest angle of the two triangles, and the line under the figure shows what the other diagonal would give: wherever you put D, the edge the circle rule picks wins.

"Constrained" means that the polygon's own edges must stay edges of the triangulation, even where the circle rule would flip them away. Our triangulator inserts the vertices one at a time. For each new point it walks across the triangles built so far until it finds the one the point falls into, splits it, and flips edges around the new point until the circle rule holds again. It also forces the polygon's edges in, and at the end it keeps only the triangles that lie inside the polygon and outside its holes.

Well-shaped triangles matter when you interpolate something across them or run a simulation on them. A flat fill doesn't care: a GPU fills a needle-thin triangle as correctly as a fat one. And most of the work is spent on polygons that hardly need any. Almost half of the Masovian polygons, 1.8 million, have four vertices or fewer. A triangle needs no cutting at all and a convex quad is two triangles, yet the Delaunay code spends 419 ms on them, building each triangulation from scratch.

## Ears

An ear of a polygon is a corner you can cut off with one straight cut. Take three consecutive vertices a, b and c. If the corner at b is convex and no other part of the polygon reaches into the triangle abc, that triangle is an ear. Cut it off, and what remains is a polygon with one vertex fewer. In 1975 Gary Meisters proved that every simple polygon with more than three vertices has at least two ears that don't overlap. So there is always another ear to cut, and a polygon with n vertices falls apart into n − 2 triangles.

Here is [one of the buildings](https://www.openstreetmap.org/way/258988073) from the blocks above. Move the slider to cut its ears one at a time, or :

0 / 25

The green triangle is the next ear to go. The algorithm needs one test, "is this corner an ear?", and the simplest version of it checks the corner and then every other vertex of the boundary. `cross` is the 2×2 determinant found in every geometry codebase; it is positive when c lies to the left of the line from a to b, so for a boundary that runs counter-clockwise a positive `cross(a, b, c)` means a convex corner:

```cpp
static double cross(double ax, double ay, double bx,
                    double by, double cx, double cy)
{
    return (bx - ax) * (cy - ay) - (by - ay) * (cx - ax);
}

bool isEar(uint32_t v) const
{
    const Node &b = _nodes[v];
    const Node &a = _nodes[b.prev], &c = _nodes[b.next];
    if (cross(a, b, c) <= 0)
        return false; // turns the wrong way, or not at all
    for (uint32_t w = c.next; w != b.prev;
         w = _nodes[w].next) {
        const Node &p = _nodes[w];
        if (cross(a.x, a.y, b.x, b.y, p.x, p.y) < 0 ||
            cross(b.x, b.y, c.x, c.y, p.x, p.y) < 0 ||
            cross(c.x, c.y, a.x, a.y, p.x, p.y) < 0)
            continue; // outside the triangle
        if (!samePos(p, a) && !samePos(p, b) &&
            !samePos(p, c))
            return false;
    }
    return true;
}
```

A point on an edge of the triangle counts as inside: if the ear touched the rest of the boundary, cutting it off would leave a polygon that touches itself. The one exception is a point at exactly the position of a, b or c, which happens where an outline meets itself at a single vertex. Holes make that common.

## Holes

Ear clipping works on one closed boundary, and 11,345 of the Masovian polygons have holes, such as courtyards and clearings in forests. The way to deal with them is to cut each hole open and stitch it to the outer ring with a bridge, an edge from a vertex of the hole to a vertex of the boundary, walked once in each direction. The boundary then runs around the outside, crosses the bridge, goes around the hole the other way and comes back. There is no area between the two copies of the bridge, so the polygon is the same, but now it is one ring.

The bridge must not cross anything. David Eberly's [write-up of ear clipping](https://www.geometrictools.com/Documentation/TriangulationByEarClipping.pdf) has a neat way to find one. Take the rightmost vertex of the hole, m, and cast a ray to the right. The first edge the ray hits, at I, has an end P further to the right. If no boundary vertex lies inside the triangle m, I, P, the bridge m–P is safe. You can drag the hole around:

The tip of the dent lies in the triangle m, I, P. A bridge to P would cross the boundary, so it goes to the dent.

the triangle is empty and the bridge goes to P. the tip of the dent sits inside the triangle, and the bridge goes to the dent instead: of the dents in the triangle, Eberly takes the one closest in angle to the ray. Holes are bridged rightmost first, so a hole further left can bridge to one that is already part of the boundary.

Map data adds two cases of its own. Our coordinates are integers on a tile grid, so a ray along a grid line runs all the time; the crossing test passes over such a vertex, and my first version got exactly this case wrong. And mappers often draw a courtyard that at one point. Such a hole needs no bridge: the two rings are joined at the shared vertex.

## The naive clipper (−1,812 ms)

With holes bridged, the first complete version walks around the boundary, tests corners with the `isEar` above, cuts every ear it finds and writes each triangle as three indices. On the Masovian polygons it takes 1.12 seconds, 2.6 times faster than Delaunay. The chart splits each time by polygon size; hover over a part of a bar to see its number, or click a row:

Delaunay 2,931 ms

1\. Naive ears 1,119 ms

2\. Only dents 882 ms

3\. Convex shortcut 880 ms

4\. Zig-zag strips 762 ms

5\. Buckets 697 ms

6\. No branches 519 ms

7\. Marked hits 489 ms

up to 4 vertices 5–8 vertices 9–32 vertices 33–256 vertices 257 and more vertices

is shorter than in every size class but the last. The 1,123 largest polygons, with 257 vertices or more, take 199 ms to clip and 90 ms with Delaunay. Each ear test there walks hundreds of vertices, and a polygon needs hundreds of tests, so the cost grows with the square of its size. The rest of this post is about making the ear test cheaper and needed less often.

## Test only the dents (−237 ms)

Look at which vertices can actually stop a convex corner from being an ear. Say some vertices lie inside its triangle, and take the one closest to the corner itself. Its two edges can't cross the sides of the triangle, so both run back, away from the corner, and the polygon wraps around that vertex from the corner's side. That makes it a reflex vertex, a dent. So if anything is inside the triangle, a dent is inside it too, and the test only has to look at the dents.

The engine keeps a list of them. A dent leaves the list when cutting its neighbour makes it convex, and a convex corner never turns into a dent, so the list only shrinks. The test becomes:

```cpp
bool isEar(uint32_t v) const
{
    const Node &b = _nodes[v];
    if (b.reflex)
        return false;
    const Node &a = _nodes[b.prev], &c = _nodes[b.next];
    for (uint32_t i = 0; i < _live; ++i) {
        const Item &p = _items[i]; // a dent
        if (p.node == b.prev || p.node == b.next)
            continue;
        if (cross(a.x, a.y, b.x, b.y, p.x, p.y) < 0 ||
            cross(b.x, b.y, c.x, c.y, p.x, p.y) < 0 ||
            cross(c.x, c.y, a.x, a.y, p.x, p.y) < 0)
            continue;
        if ((p.x != a.x || p.y != a.y) &&
            (p.x != b.x || p.y != b.y) &&
            (p.x != c.x || p.y != c.y))
            return false;
    }
    return true;
}
```

The 28.1 million ear tests now look at 185 million points instead of 353 million, and the total drops to . That is less than you might expect, because building outlines have a lot of dents. The building with the arch above has 15 of them among its 27 vertices. And the smallest polygons get slower, from 34 to 54 ms: for a triangle or a quad, sorting the corners into dents and the rest costs more than the walk it saves.

## Skip tests on convex polygons (−2 ms)

A polygon without dents is convex, and every corner of a convex polygon is an ear, so it can be cut without a single test. Checking for that needs care. "Every corner turns left" is not enough:

Corners turning left: 5 of 5. Changes of x direction: 4. Not convex.

The turns left at all five corners, because it goes around its centre twice. The engine adds one more condition. Walking around a convex ring, the edges go right along the bottom and left along the top, so their x direction changes exactly twice, at the leftmost and the rightmost point. The passes; the star changes direction four times, at the circled corners, and goes to the ordinary ear tests. Both counts come out of the same loop that marks the dents, so the check costs almost nothing. Triangles and quads get their own shortcut before that loop.

The shortcut removes 4.4 million of the 28.1 million ear tests, but those were the cheapest ones: a ring without dents has nothing to scan. On its own it barely moves the total, , though the polygons with four vertices or fewer win back part of what the dents cost them, from 54 to 48 ms. It matters more in the next step, where a convex ring becomes a single strip.

## Write strips (−118 ms)

So far every triangle costs three indices. A GPU can also draw a triangle strip, where each new index adds a triangle made of it and the two before. A convex ring makes one strip if you zig-zag across it:

`0 1 7 2 6 3 5 4`

The ear clipper can write strips too. Cutting an ear makes the corners of its two neighbours sharper, so they are good candidates for the next ear, and cutting one of them extends the strip by a single index. The engine tries them first and keeps alternating between the two ends while the ears last. In its code the neighbours of the cut vertex are a and b:

```cpp
// The next ear alternates between the ends of the cut.
while (true) {
    if (sideA) {
        const uint32_t z = n[a].prev;
        _strips.push_back(n[z].index);
        unlink(a);
        a = z;
    } else {
        const uint32_t z = n[b].next;
        _strips.push_back(n[z].index);
        unlink(b);
        b = z;
    }
    count--;
    sideA = !sideA;
    if (count < 3 || !isEar(sideA ? a : b))
        break;
}
```

When the next corner is not an ear, the strip ends and the walk goes on from where it stopped. The 21.5 million triangles now take 35.8 million indices instead of 64.5 million. The ear tests don't get fewer: the naive walk already went on at the next corner after a cut, and now every strip also ends on a test that fails, 25.0 million instead of 23.7 million. Still, the total falls to , and the largest polygons drop from 146 to 103 ms.

## Sort the dents into buckets (−65 ms)

A forest or a lake can have thousands of vertices and hundreds of dents, and each ear test still reads every one of them. When a polygon has more than 32 dents, the engine sorts them into buckets along the longer side of their bounding box, about two dents per bucket. An ear test looks only at the buckets that its triangle spans. As dents turn convex and leave, the index is rebuilt once half of it is gone.

Points scanned fall from 181 million to 101 million, almost all of it on the large polygons: the polygons with 257 vertices or more go from 103 to 57 ms. Total: .

## Count without branches (−178 ms)

The loop in the dent test exits at the first dent inside the triangle. Most of the time there is none: the test reads all the dents and says yes. An early exit then buys nothing, and the three comparisons per point are branches the CPU has to predict. The engine first runs a pass that only counts dents inside the triangle, with no branches at all:

```cpp
const double abx = b.x - a.x, aby = b.y - a.y;
const double bcx = c.x - b.x, bcy = c.y - b.y;
const double cax = a.x - c.x, cay = a.y - c.y;
uint32_t hits = 0;
for (uint32_t i = begin; i < end; ++i) {
    const Item &p = items[i];
    const double side = std::min(
        std::min(abx * (p.y - a.y) - aby * (p.x - a.x),
                 bcx * (p.y - b.y) - bcy * (p.x - b.x)),
        cax * (p.y - c.y) - cay * (p.x - c.x));
    hits += (uint32_t)(side >= 0) & (uint32_t)(p.node != ia)
          & (uint32_t)(p.node != ic);
}
if (hits == 0)
    return true;
```

The smallest of the three determinants is non-negative exactly when the point is inside the closed triangle. Only when the count is not zero does the exact loop from before run, to rule out dents that sit on a corner. The bucket version does the same with a bounding-box test, 64 points at a time into a bit mask. The total falls to , and the middle sizes gain the most: polygons with 33 to 256 vertices go from 327 to 216 ms.

## Test only the marked dents (−30 ms)

When the count is not zero, the exact loop reads all the dents again, although the first pass has already seen which ones were inside. The bucket version keeps that in its bit mask. The short list, up to 32 dents, can do the same: the first pass sets one bit per hit instead of counting, and the exact test looks only at those dents:

```cpp
    hits |= ((uint32_t)(side >= 0) & (uint32_t)(p.node != ia)
             & (uint32_t)(p.node != ic)) << (i - begin);
}
while (hits) {
    const Item &p = items[begin + lowestBit(hits)];
    hits &= hits - 1;
    // the exact test from before
}
```

Points read fall from 128 million to 120 million. This is the last step, and it is the code of earstrip as published: , 6.0 times less than Delaunay on the same polygons.

## When there are no ears

Meisters' theorem is about simple polygons. Map data is rounded to a tile grid, and rounding can break a simple polygon. Here is the outline of [Warszawa Śródmieście](https://www.openstreetmap.org/way/28010962), a railway station less than a kilometre from the blocks above, rounded to grids of different size. The orange rings mark vertices that land on the same point as their neighbour:

Grid step 73 cm. 68 of 154 vertices land on their neighbour. 84 triangles.

per tile side, the grid our map files use, a grid step is 18 cm. Even there 31 of the 154 vertices merge with a neighbour, because mappers often place points a few centimetres apart, and the outline survives. nearly half of the vertices merge, and the clipper still cuts it. the rounded outline touches itself in a way that leaves no ear at all, and at 10 bits it happens to work again.

When the clipper walks all the way around the boundary without finding an ear, it drops the corners that have no area and walks once more. If that fails too, it gives up, and the engine triangulates that polygon with the Delaunay code, which still exists for this. In the Masovian data 76 polygons end up there. They come from a map built before the last change on the other end: our map generator now rounds every polygon to the final grid with GEOS's snap rounding and checks that ear clipping can cut it. In a fresh build of the Masovian map that repaired 273 polygons, and ear clipping can cut every polygon in the file.

Delaunay has one more job. Every hole needs its own bridge, and finding one scans the whole boundary, so a polygon with thousands of holes pays for thousands of scans. Our world map has a polygon with 11,675 holes: ear clipping takes 606 ms on it, Delaunay 17 ms. The engine now sends every polygon with 128 holes or more to Delaunay. In the Masovian data that is two polygons.

The library keeps both of these fallbacks inside. earstrip covers a polygon with a constrained Delaunay triangulation of its ring edges when the ears run out, when the triangles they leave cover the wrong area, and when it has more than 128 rings; `run()` in C++ and `lastStatus()` in JavaScript say which way a polygon went. On the 56,472 polygons of our world map tiles, with up to 11,675 holes in one, earstrip takes 459 ms and earcut.hpp 2,892 ms. On the zoom 13 polygons that the ears can't cut exactly, earcut's triangles differ from an even-odd fill of the rings on 31% of the pixels on average, earstrip's on 0.15%.

## In the browser

Before the new clipper went into the app, I compared it with [earcut](https://github.com/mapbox/earcut), the ear clipper behind MapLibre and Mapbox GL, in its C++ version earcut.hpp 3.2.4. Natively the two are close, and that could have been the end of it. But we are working on a web renderer that runs the same C++ engine compiled to WebAssembly, so I went further and compared the browser side too: both clippers compiled to WebAssembly, and earcut.js 3.2.4, the JavaScript original that web maps run today. For that I took the clipper out of the engine into earstrip. On all 5.8 million polygons of our test sets it writes exactly the engine's strips, and its repository has these polygons and the code behind every number in this section. Its native benchmark is the one behind the chart above, and the native row below is the last step run against earcut.hpp. Every WebAssembly build gives exactly the same triangles as its native version on all 4.4 million polygons of zoom levels 13 and 9. Milliseconds for the 3.74 million polygons of zoom 13, best of 7, one thread, V8 in Node and JavaScriptCore in Bun:

|                                                               | earstrip  | earcut        |
| ------------------------------------------------------------- | --------- | ------------- |
| Native C++                                                    | 489       | 557           |
| WebAssembly, loop inside the module, V8 / JSC                 | 601 / 566 | 930 / 843     |
| WebAssembly, one call from JS per polygon, V8 / JSC           | 688 / 688 | 1,007 / 950   |
| npm package, one call per polygon, V8 / JSC                   | 766 / 807 | 1,103 / 1,290 |
| npm package, direct calls, all polygons in one call, V8 / JSC | 635 / 619 |               |

Natively earcut.hpp is 14% slower than earstrip. Compiled to WebAssembly and run in V8, earstrip loses 23% and earcut.hpp loses 67%, almost all of it in its main loop that searches for ears. It isn't the allocator: builds with mimalloc and emmalloc give the same time. I haven't dug further. In V8, earcut.hpp in WebAssembly ends up only 19% faster than plain earcut.js, and 10% once each polygon is a separate call from JavaScript.

The package rows are what a web page gets, and the earcut column there is earcut.js. The call that replaces earcut takes the coordinates as JavaScript numbers and returns a new array of triangle indices every time: 1.4 times as fast as earcut.js in V8 and 1.6 times in JavaScriptCore. About half of what it loses against the loop inside the module goes into copying the coordinates in and turning the strips into a list, the rest into calling into WebAssembly once per polygon. The direct calls skip all of that: the polygons go straight into the module's memory and come back as one index buffer of strips for WebGL 2 or WebGPU, 1.7 and 2.1 times as fast as earcut.js.

Some of the ideas above are in earcut too. It also relies on the fact that only a dent can block an ear, but it checks that last, for every vertex that passes the bounding box and triangle tests, instead of keeping a list of dents. Above 80 vertices it links them in z-order and scans only the stretch of that curve the triangle's bounding box covers, which does the job of the buckets. What it doesn't have are the shortcut for convex polygons, the strips and the count without branches: it writes three indices per triangle and stops at the first point inside.

earstrip is on [GitHub](https://github.com/GLMap/earstrip) and [npm](https://www.npmjs.com/package/earstrip), with the polygons from this post and the code to time it against earcut on your own machine.
