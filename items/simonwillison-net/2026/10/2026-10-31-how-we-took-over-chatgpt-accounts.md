---
title: How We Took Over ChatGPT Accounts
link: https://simonwillison.net/2026/Oct/31/how-we-took-over-chatgpt-accounts/
source: simonwillison-net
published: 2026-10-31T23:15:22Z
updated: 2026-10-31T23:15:22Z
first_seen: 2026-11-01T02:06:10.004030692Z
labels:
- chatgpt
- content-security-policy
- iframes
- javascript
- openai
- sandboxing
- security
- xss
summary: 'How We Took Over ChatGPT Accounts Parameter Labs describe an exploit they found against ChatGPT''s implementation of the artifacts pattern (OpenAI used to call this "Canvas" but it looks like the latest revision is called HTML previews), where interactive custom HTML apps are displayed in a sandboxed iframe. It''s important that HTML in these stays tightly sandboxed to avoid untrusted code interacting with OpenAI APIs and accessing private data. Parameter found quite a complex attack chain. OpenAI use the MathLive JavaScript library as part of their graph support, and that library includes its own virtual keyboard, which can be triggered in the parent page via postMessage() from the sandboxed iframe. That message receiving code uses innerHTML, so anything that sends a message can add custom HTML to the parent - and there was no mechanism to prevent other sandboxed frames, including HTML preview frames, from calling that method. This meant untrusted code in an iframe could insert HTML, and while innerHTML doesn''t directly execute script elements it could be used to inject a new iframe without sandbox restrictions. OpenAI’s CSP rules blocked inline scripts entirely, and only allowed script loading from a handful of domains, but one of those was YouTube - and the YouTube oEmbed endpoint could be used to reflect JavaScript back in a way that executed it. Once the attack had JavaScript execution on ChatGPT, it was trivial to fetch private user data from APIs and exfiltrate it via an image reference. The end result: you could create a ChatGPT share URL which, when visited by another signed-in user, could exfiltrate their private data or take actions on their behalf. OpenAI closed the hole and paid out a $5,000 bug bounty, which I think was a bit cheap for a vulnerability this serious. Via Hacker News Tags: iframes, javascript, sandboxing, security, xss, openai, chatgpt, content-security-policy'
content: extracted
html: 2026-10-31-how-we-took-over-chatgpt-accounts.html
---

**[How We Took Over ChatGPT Accounts](https://www.parameter.ai/labs/we-found-a-gap-at-openai)** ([via](https://news.ycombinator.com/item?id=50314699 "Hacker News")) Parameter Labs describe an exploit they found against ChatGPT's implementation of the artifacts pattern (OpenAI used to call this "Canvas" but it looks like the latest revision is called [HTML previews](https://help.openai.com/en/articles/20001246-working-with-writing-blocks-and-code-blocks-in-chatgpt)), where interactive custom HTML apps are displayed in a sandboxed iframe.

It's important that HTML in these stays tightly sandboxed to avoid untrusted code interacting with OpenAI APIs and accessing private data.

Parameter found quite a complex attack chain. OpenAI use the MathLive JavaScript library as part of their graph support, and that library includes [its own virtual keyboard](https://mathlive.io/mathfield/virtual-keyboard/), which can be triggered in the parent page via `postMessage()` from the sandboxed iframe.

That message receiving code uses `innerHTML`, so anything that sends a message can add custom HTML to the parent - and there was no mechanism to prevent other sandboxed frames, including HTML preview frames, from calling that method.

This meant untrusted code in an iframe could insert HTML, and while `innerHTML` doesn't directly execute script elements it could be used to inject a new iframe *without* sandbox restrictions.

OpenAI’s CSP rules blocked inline scripts entirely, and only allowed script loading from a handful of domains, but one of those was YouTube - and the YouTube oEmbed endpoint could be used to reflect JavaScript back in a way that executed it.

Once the attack had JavaScript execution on ChatGPT, it was trivial to fetch private user data from APIs and exfiltrate it via an image reference.

The end result: you could create a ChatGPT share URL which, when visited by another signed-in user, could exfiltrate their private data or take actions on their behalf.

OpenAI closed the hole and paid out a $5,000 bug bounty, which I think was a bit cheap for a vulnerability this serious.
