---
title: Pele – Mermaid diagrams as themeable SVG
link: https://pele.run/
source: hnrss-org
published: 2026-10-17T15:55:09Z
updated: 2026-10-17T15:55:09Z
first_seen: 2026-10-18T06:32:49.699637230Z
authors:
- dchest
content: extracted
html: 2026-10-17-pele-mermaid-diagrams-as-themeable-svg.html
extra:
  comments_url: https://news.ycombinator.com/item?id=50132193
  num_comments: 31
  points: 368
---

## How it works

Write Mermaid syntax, Pele renders it to SVG. Try editing the example below.

Mermaid

```mermaid
1flowchart TD2  A[Mermaid text] --> B{Flowchart?}3  B -- Yes --> C[Pele]4  B -- No --> D[Mermaid]5  C --> E([SVG])6  D --> E
```

Use CSS variables to set colors and fonts. Color changes apply without re-rendering.

CSS

```css
1.pele {2  --pele-bg: #fffcf0;3  --pele-fg: #100f0f;4  --pele-muted: #6f6e69;5  --pele-line: #100f0f;6  --pele-surface: #f2f0e5;7  --pele-surface-alt: #e6e4d9;8  --pele-border: #cecdc3;9}
```

Pele

## Why Pele?

Zero dependencies, 6× smaller, and up to 500× faster than Mermaid.js. Pele supports every diagram type built into Mermaid 12.1.0.

| Pele                | Mermaid.js        |                     |
| ------------------- | ----------------- | ------------------- |
| Bundle size gzip    | 230 kB            | 1.6 MB              |
| Render time         | 0.05–0.3 ms       | 6–82 ms             |
| Direct dependencies | 0                 | 23                  |
| Small screens       | Adapts the layout | Scales the diagram  |
| Theming             | CSS variables     | Theme configuration |

## Pele in your app

Add Pele to your app using the [API](https://pele.run/api).

```shell
1npm install pele
```

Pass Mermaid syntax to `render()` and insert the returned SVG into your page.

```ts
1import { render } from 'pele';23const { svg } = render(`flowchart LR4  A[Mermaid text] --> B[SVG]`);56document.querySelector('#diagram').innerHTML = svg;
```

## Explore
