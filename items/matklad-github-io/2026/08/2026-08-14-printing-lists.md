---
title: Printing Lists
link: https://matklad.github.io/2026/08/14/printing-lists.html
source: matklad-github-io
published: 2026-08-14T00:00:00Z
updated: 2026-08-14T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Alex Kladov
summary: 'To print a comma-separated list, a concise idiom is to optionally print the comma first, before the element:'
content: extracted
html: 2026-08-14-printing-lists.html
---

To print a comma-separated list, a concise idiom is to optionally print the comma first, before the element:

```
for (items, 0..) |item, item_index| {
    if (item_index > 0) std.debug.print(", ");
    std.debug.print("{}", .{item});
}
```
