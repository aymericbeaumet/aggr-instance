---
title: Inline rendering with document​.currentScript
link: https://simonwillison.net/2026/Oct/17/document-currentscript/
source: simonwillison-net
published: 2026-10-17T21:11:12Z
updated: 2026-10-17T21:11:12Z
first_seen: 2026-10-18T06:32:49.699637230Z
labels:
- javascript
summary: 'Inline rendering with document​.currentScript Lean Rada describes a neat trick that takes advantage of the document.currentScript property. You can use that to drop in a tiny inline <script> element that replaces itself when the page loads: The year is <script> document.currentScript.replaceWith( new Date().getFullYear())</script> If you define a shortcut function like this: $=(...n)=>document.currentScript.replaceWith(...n) You get a tiny template language: The year is <script>$(new Date().getFullYear())</script> Tags: javascript'
content: feed
html: 2026-10-17-inline-rendering-with-document-currentscript.html
---

**[Inline rendering with document.currentScript](https://leanrada.com/notes/inline-rendering-currentscript/)**

Lean Rada describes a neat trick that takes advantage of the [document.currentScript](https://developer.mozilla.org/en-US/docs/Web/API/Document/currentScript) property. You can use that to drop in a tiny inline `<script>` element that replaces itself when the page loads:

```
The year is <script>
document.currentScript.replaceWith(
  new Date().getFullYear())</script>
```

If you define a shortcut function like this:

```
$=(...n)=>document.currentScript.replaceWith(...n)
```

You get a tiny template language:

```
The year is <script>$(new Date().getFullYear())</script>
```

Tags: [javascript](https://simonwillison.net/tags/javascript)
