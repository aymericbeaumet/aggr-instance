---
title: Select Styling Specimens
link: https://simonwillison.net/2026/Nov/22/select-styling/
source: simonwillison-net
published: 2026-11-22T19:45:06Z
updated: 2026-11-22T19:45:06Z
first_seen: 2026-11-23T01:33:27.333975467Z
labels:
- css
- html
- tools
summary: 'Tool: Select Styling Specimens I got frustrated at how HTML <select> boxes default to looking different in different browsers, and are (I think) quite ugly in Chrome: So I had Claude Fable 5.2 build me this page showing all of the ways they can be styled: Build an HTML page that shows all of the ways a select box can be styled, I plan to take screenshots in every desktop and mobile browser After some experimentation I''ve decided my favorite CSS for select boxes is this: select { appearance: none; color: #1f2933; padding: 0.5em 2.25em 0.5em 0.75em; border: 1px solid #c4cbd3; border-radius: 6px; background: #fff url("data:image/svg+xml,%3Csvg xmlns=''http://www.w3.org/2000/svg'' viewBox=''0 0 12 8''%3E%3Cpath d=''M1 1.5l5 5 5-5'' fill=''none'' stroke=''%23667085'' stroke-width=''1.5''/%3E%3C/svg%3E") no-repeat right 0.75em center / 0.75em; } Here''s what that looks like in the three browsers: Tags: tools, css, html'
content: extracted
html: 2026-11-22-select-styling-specimens.html
remote_preview:
  url: https://static.simonwillison.net/static/2026/select-firefox-safari-chrome.webp
  alt: The same select box in Chrome, Safari, and Firefox, Chrome is white with a black border. Safari is grey with no border. Firefox is gray with a black border. They are all slightly different sizes.
---

I got frustrated at how HTML `<select>` boxes default to looking different in different browsers, and are (I think) quite ugly in Chrome:

![The same select box in Chrome, Safari, and Firefox, Chrome is white with a black border. Safari is grey with no border. Firefox is gray with a black border. They are all slightly different sizes.](https://static.simonwillison.net/static/2026/select-firefox-safari-chrome.webp)

So I had Claude Fable 5.2 [build](https://claude.ai/share/a82c181c-0ea6-4fec-a659-8de653cb3e7b) me [this page](https://tools.simonwillison.net/select-styling) showing all of the ways they can be styled:

> `Build an HTML page that shows all of the ways a select box can be styled, I plan to take screenshots in every desktop and mobile browser`

After some experimentation I've decided my favorite CSS for select boxes is this:

```css
select {
  appearance: none;
  color: #1f2933;
  padding: 0.5em 2.25em 0.5em 0.75em;
  border: 1px solid #c4cbd3;
  border-radius: 6px;
  background: #fff url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 12 8'%3E%3Cpath d='M1 1.5l5 5 5-5' fill='none' stroke='%23667085' stroke-width='1.5'/%3E%3C/svg%3E") no-repeat right 0.75em center / 0.75em;
}
```

Here's what that looks like in the three browsers:

![Now the select box is the same in all three browsers - it has a rounded corner grey border and a grey downward facing arrow in each one.](https://static.simonwillison.net/static/2026/select-styled-firefox-safari-chrome.webp)
