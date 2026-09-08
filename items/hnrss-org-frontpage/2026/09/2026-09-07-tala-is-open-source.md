---
title: TALA Is Open-Source
link: https://d2lang.com/blog/tala-is-open-source/
source: hnrss-org-frontpage
published: 2026-09-07T23:37:42Z
updated: 2026-09-07T23:37:42Z
first_seen: 2026-09-08T10:17:10.166315Z
authors:
- alixanderwang
summary: 'Article URL: https://d2lang.com/blog/tala-is-open-source/ Comments URL: https://news.ycombinator.com/item?id=49604150 Points: 226 # Comments: 15'
content: extracted
html: 2026-09-07-tala-is-open-source.html
preview:
  file: 2026-09-07-tala-is-open-source.preview-eb41b2ec711a.webp
  width: 256
  height: 128
  color: '#dee3f5'
images:
- source: https://d2lang.com/img/opengraph.png
  original:
    file: 2026-09-07-tala-is-open-source.image-fdaacfc5342a.png
    width: 874
    height: 437
  variants:
  - file: 2026-09-07-tala-is-open-source.image-540515ee6cfe.webp
    width: 48
    height: 24
  color: '#e8ebfe'
---

Following up on the announcement [here](https://d2lang.com/blog/d2-non-profit/), TALA (Terrastruct's AutoLayout Algorithm) is now open-source under the same license as D2 (MPL-2.0).

TALA is a novel autolayout algorithm designed with software architecture diagrams in mind. This means it's primarily an orthogonal layout engine, which more closely matches what you might find on whiteboards, rather than the DAG-based ones that grow in one direction. It blends ideas from different graph-drawing research papers (cited in source code) along with original techniques to achieve aesthetic diagrams. It considers multiple objectives of "aesthetic", including symmetry, median distance, flow, clustering of like nodes, and much more.

I'll keep the text short and lead with examples.

- The first batch compares diagrams rendered with TALA with the other two layout algorithms D2 comes with -- Dagre and ELK. These are not hand-selected, I just found public d2 files from around GitHub. So for some, you may very well prefer the not-TALA layout.
- The second batch demonstrates a unique property of TALA, which is that node positions and sizes can be customized, e.g. locking in the coordinates. This lends itself especially well to agentic use cases, where models can draw in 2D space well, but TALA still takes care of routing, which models still struggle with. I had AI generate these.
- The third batch demonstrates TALA's capability to support a hybrid of some nodes specifying coordinates and some left to the layout engine. You might have a specific shape of a collection of nodes in mind, which you can specify with coordinates, and TALA can take care of the rest. Again, AI generated.

Please also note that TALA is not without tradeoffs.

- It has randomness in the algorithm. It finds the best layout by using a default of 3 seeds and choosing the one scored the best. Given the same seeds and same input, it'll produce the same diagram. But let's say you just add one more node. The diagram could look completely different. In Dagre and ELK, it looks mostly the same as prior, with the extra node accommodated for. This is sometimes desirable.
- It doesn't do DAGs as well. I often find myself preferring Dagre or ELK when I want a long flowing graph.
- It can take longer to run for larger diagrams -- scaling nonlinearly. For a benchmark of TALA's runtime performance compared to others, see [https://github.com/d2lang/d2-benchmarks](https://github.com/d2lang/d2-benchmarks).

TALA comes bundled into D2 v0.9.0, so just install and specify with `--layout=tala` to try it out! Or head on over to [https://play.d2lang.com](https://play.d2lang.com/?layout=tala&script=7Jjfb-JGEMff_VeMLs8YWJOGbqVK6fWqVsq1dyRVXipFY--AVzG7vt11I3rif6_8C2xwCknjpJV4ifES5vPd-eUdn8G1zkxEHGLnUsuHw4V0cRb6kV4OFzpBtRh8ySijYfk3THQ4ZHjxbThFjIJvCCO8OJ_O55P5-WTM5lE4mZ6HI3YhxPl0KJe4IDucJ_phMAp8wbwzuJIRKUscPv5y8x1YIph9uLy6u_1tdvWjvxS-dwaXAlOHTmrF4SaWRgxSNG4FMtLKgqE0wYgEPEgXQ5jJxA2kggRDSkiAjTEl63uekIai0oiRi9h5XmbJjMYcfi-u8NWD8r85pGSsVvm9WyXEi68AllniZJrfO5NRsRTqRDRu51q5gZV_EQc29QDW3rrCsArD-sUEFSboFzOpMJPeMJFWDqUic5caLbKIzF0eqmsyf8qIqmjthXSHfRDkATi096Nx_YvWzzvF71lt2Q1YsbZuWWd9WS9qkMPn_AJXMjRoVt2of7LKpvWaTBIOsVa0EvRQoYpLHQMOn6pPG8s7oA5U1xa2uE-Y4gpvY5m2vhik6BwZxWFhUKqGdSPIDAwKmVkO0xaDw1IrXS2tOyIxhsH3pdf87ZY-qGIFbtDed3sPlVyiI3F0oNi0A87eAv5IKbFtKbFTKZ1K6VRKR5WSQIchWuIwk2rxh_ohm8_JNB-C0SqRSpA5vn6qMLYjlUhF1tsLRiFCu5jMnXXaEId37-DrLkunGEm34jCqnWZISMthll9q1-zpfVKmB6yx1kqWLd2_aIfOYBhKt_zCYYZhqN3Hz20tRfheSsiGqtBZDr9e3lz3SHt0263-m59Xs2V1lLnV5n5z7jy131dqv3UIOLyvPv3v2m_Z9-qN5J2wjvzPqETSa_d9hM2OYD_vyLItGbYpmdOJZWM96NX6pNdyH41PBX98wY_YyVtP8Nb4LRvkaPy8FvlCdLZPD96UPumdvva8szx5VflIqN_t5Qo63yT5dUa8N4SuoaulqkPT8S_8DrBZj-zgMTbrf9-TA-ye9t3t6faImCvbTnDX-QDVvwjWIaIxwfnboei_IqicFbsGyx1kY-QJ_g2xnNCeCawD2k67xqTlt8-sHH4iF8Uv6-dDIpi_-1zoRUZXXh2SxF5RUp5ZT9FzRErsQruTZDfb3kLE3wMA&), which runs 100% client-side. I especially look forward to the improvements that being open-source brings, and can't wait to see what improvements and ideas are submitted by the community.

Special thanks to Gavin Nishizawa for substantial broad contributions across TALA, and Júlio César Batista for his work on hierarchy algorithms and more. It was so fun getting to work on such interesting stuff with you guys.

## Batch 1: Comparisons [​](https://d2lang.com/blog/tala-is-open-source/#batch-1-comparisons "Direct link to Batch 1: Comparisons")

### Fulcro RAD architecture [​](https://d2lang.com/blog/tala-is-open-source/#fulcro-rad-architecture "Direct link to Fulcro RAD architecture")

Select a diagram to enlarge it. Each layout is scaled to fit its panel.

### Mocha secure-enclave SoC [​](https://d2lang.com/blog/tala-is-open-source/#mocha-secure-enclave-soc "Direct link to Mocha secure-enclave SoC")

Select a diagram to enlarge it. Each layout is scaled to fit its panel.

### Jupyter on AWS EKS [​](https://d2lang.com/blog/tala-is-open-source/#jupyter-on-aws-eks "Direct link to Jupyter on AWS EKS")

Select a diagram to enlarge it. Each layout is scaled to fit its panel.

### Lion Reader frontend data flow [​](https://d2lang.com/blog/tala-is-open-source/#lion-reader-frontend-data-flow "Direct link to Lion Reader frontend data flow")

Select a diagram to enlarge it. Each layout is scaled to fit its panel.

### ROSS rotor-dynamics workflow [​](https://d2lang.com/blog/tala-is-open-source/#ross-rotor-dynamics-workflow "Direct link to ROSS rotor-dynamics workflow")

Select a diagram to enlarge it. Each layout is scaled to fit its panel.

### Go Queue worker architecture [​](https://d2lang.com/blog/tala-is-open-source/#go-queue-worker-architecture "Direct link to Go Queue worker architecture")

Select a diagram to enlarge it. Each layout is scaled to fit its panel.

### Ouroboros Leios simulator [​](https://d2lang.com/blog/tala-is-open-source/#ouroboros-leios-simulator "Direct link to Ouroboros Leios simulator")

Select a diagram to enlarge it. Each layout is scaled to fit its panel.

## Batch 2: Custom positioning [​](https://d2lang.com/blog/tala-is-open-source/#positioning-with-top-and-left "Direct link to Batch 2: Custom positioning")

### Signal House [​](https://d2lang.com/blog/tala-is-open-source/#signal-house "Direct link to Signal House")

TALA · positioned with top / left

[![Signal House](https://d2lang.com/blog/tala-layouts/positioning/signal-house/tala.svg)](https://d2lang.com/blog/tala-layouts/positioning/signal-house/tala.svg)

View D2 source

### Atlas / Data platform [​](https://d2lang.com/blog/tala-is-open-source/#atlas--data-platform "Direct link to Atlas / Data platform")

TALA · positioned with top / left

[![Atlas / Data platform](https://d2lang.com/blog/tala-layouts/positioning/atlas-data-platform/tala.svg)](https://d2lang.com/blog/tala-layouts/positioning/atlas-data-platform/tala.svg)

View D2 source

### Night shift / Mission control [​](https://d2lang.com/blog/tala-is-open-source/#night-shift--mission-control "Direct link to Night shift / Mission control")

TALA · positioned with top / left

[![Night shift / Mission control](https://d2lang.com/blog/tala-layouts/positioning/night-shift/tala.svg)](https://d2lang.com/blog/tala-layouts/positioning/night-shift/tala.svg)

View D2 source

### Friday deploy: the escape room [​](https://d2lang.com/blog/tala-is-open-source/#friday-deploy-the-escape-room "Direct link to Friday deploy: the escape room")

TALA · positioned with top / left

[![Friday deploy: the escape room](https://d2lang.com/blog/tala-layouts/positioning/friday-deploy/tala.svg)](https://d2lang.com/blog/tala-layouts/positioning/friday-deploy/tala.svg)

View D2 source

### The Internet is a jellyfish [​](https://d2lang.com/blog/tala-is-open-source/#the-internet-is-a-jellyfish "Direct link to The Internet is a jellyfish")

TALA · positioned with top / left

[![The Internet is a jellyfish](https://d2lang.com/blog/tala-layouts/positioning/internet-jellyfish/tala.svg)](https://d2lang.com/blog/tala-layouts/positioning/internet-jellyfish/tala.svg)

View D2 source

### Orbital coffee logistics [​](https://d2lang.com/blog/tala-is-open-source/#orbital-coffee-logistics "Direct link to Orbital coffee logistics")

TALA · positioned with top / left

[![Orbital coffee logistics](https://d2lang.com/blog/tala-layouts/positioning/orbital-coffee/tala.svg)](https://d2lang.com/blog/tala-layouts/positioning/orbital-coffee/tala.svg)

View D2 source

### Cloud Conservatory [​](https://d2lang.com/blog/tala-is-open-source/#cloud-conservatory "Direct link to Cloud Conservatory")

TALA · positioned with top / left

[![Cloud Conservatory](https://d2lang.com/blog/tala-layouts/positioning/cloud-conservatory/tala.svg)](https://d2lang.com/blog/tala-layouts/positioning/cloud-conservatory/tala.svg)

View D2 source

### Velvet Rope [​](https://d2lang.com/blog/tala-is-open-source/#velvet-rope "Direct link to Velvet Rope")

TALA · positioned with top / left

[![Velvet Rope](https://d2lang.com/blog/tala-layouts/positioning/velvet-rope/tala.svg)](https://d2lang.com/blog/tala-layouts/positioning/velvet-rope/tala.svg)

View D2 source

### Synthwave City [​](https://d2lang.com/blog/tala-is-open-source/#synthwave-city "Direct link to Synthwave City")

TALA · positioned with top / left

[![Synthwave City](https://d2lang.com/blog/tala-layouts/positioning/synthwave-city/tala.svg)](https://d2lang.com/blog/tala-layouts/positioning/synthwave-city/tala.svg)

View D2 source

## Batch 3: Partial positioning [​](https://d2lang.com/blog/tala-is-open-source/#partial-positioning "Direct link to Batch 3: Partial positioning")

### The Printing Room [​](https://d2lang.com/blog/tala-is-open-source/#the-printing-room "Direct link to The Printing Room")

TALA · 4 pinned nodes / 10 automatic nodes

[![The Printing Room](https://d2lang.com/blog/tala-layouts/positioning/printing-room/tala.svg)](https://d2lang.com/blog/tala-layouts/positioning/printing-room/tala.svg)

**Pinned:** The four CMYK stations share a fixed top coordinate and equally spaced left coordinates so the print sequence retains its mechanical alignment.

**Automatic:** TALA positions the feeder, camera, registration controller, dryer, prepress and finishing steps; none has top or left.

See the positioning declarations

View D2 source

### MULE / Utility Rover [​](https://d2lang.com/blog/tala-is-open-source/#mule--utility-rover "Direct link to MULE / Utility Rover")

TALA · 4 pinned nodes / 11 automatic nodes

[![MULE / Utility Rover](https://d2lang.com/blog/tala-layouts/positioning/robot-chassis/tala.svg)](https://d2lang.com/blog/tala-layouts/positioning/robot-chassis/tala.svg)

**Pinned:** The four motor assemblies are fixed at the front and rear corners of the chassis rectangle.

**Automatic:** TALA places every controller, sensor, power and safety node between or around those corners and arranges the unpositioned fleet container.

See the positioning declarations

View D2 source

Sources and rendering details

The seven layout comparisons use public project diagrams from D2's [real-world fixtures](https://github.com/d2lang/d2/pull/2871). Each comparison uses the same D2 source and the same compiler build, changing only the layout engine. Source styles, themes, and explicit grid constraints are preserved. Some fixture icons were already replaced with built-in shapes.

SVGs are scaled independently to fit each panel. Use **Open SVG** to inspect labels and connections at a larger size.

[Source provenance and licenses](https://d2lang.com/blog/tala-layouts/REAL_WORLD.md) ·

[Render settings and revisions](https://d2lang.com/blog/tala-layouts/render-manifest.json)

The eleven positioning examples are original, fictional compositions rendered with TALA using the same public D2 build. Icon downloads include the full source and local vector assets. The two partial-positioning examples apply top/left only to their listed pinned nodes; all other nodes and every container are automatically placed.

[Positioning render settings](https://d2lang.com/blog/tala-layouts/positioning/render-manifest.json)

.
