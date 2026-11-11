---
title: Designing for Agents — Zeno Rocha, Resend
link: https://latent.space/p/resend/
source: latent-space
published: 2026-11-10T20:05:17Z
updated: 2026-11-10T20:05:17Z
first_seen: 2026-11-11T00:34:12.222309031Z
authors:
- swyx .ai
summary: Where does answer engine optimization (AEO) end and agent experience begin? Inside Resend, from the docs that help Claude Code pick an email API to the tools it uses to send.
content: extracted
html: 2026-11-10-designing-for-agents-zeno-rocha-resend.html
remote_preview:
  url: https://i.ytimg.com/vi/bdIoODXsu8k/maxresdefault.jpg
  alt: Designing for Agents — Zeno Rocha, Resend
extra:
  audio_type: audio/mpeg
  audio_url: https://podcasts.beehiiv.com/3967c1f2-cb97-4eb0-a5c0-21a1d2579408/01a15fdf-0e93-7e1a-880d-d73752a1f915/01a1c6f7-44d3-72a3-8e65-ffcfae742084/01a1c6f7-4605-726f-ac19-31f9b1763da0.mp3
  duration_seconds: 5152
  thumbnail: https://i.ytimg.com/vi/bdIoODXsu8k/maxresdefault.jpg
---

Where does answer engine optimization (AEO) end and agent experience begin? Inside Resend, from the docs that help Claude Code pick an email API to the tools it uses to send.

Today’s podcast comes from a real life mystery: We are big fans of Cloudflare’s developer platform, and Cloudflare Email is a convenient tool especially when all your domains are already on Cloudflare.

So why do agents keep suggesting we use Resend despite having an existing “home bias” and much larger incumbent player?

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/e3cf6421-0eef-401f-b47d-3e7681c9d032/image.png?t=1794340740)

In a recent AEO study, Claude Code picked Resend first in 64 of the 102 email recommendations the researchers could extract. SendGrid came first seven times, yet showed up 55 times as an alternative. If the developer asks an agent to implement email and accepts the patch, how often does anyone get around to trying the second choice? [The study breaks down the results by model and app.](https://amplifying.ai/research/claude-code-picks/report?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) Our more recent [comprehensive AEO study](https://aeo.latent.space/entities?entity=Resend&entityId=platform%3A6de1be36afd7a16d2e4fb504&cat=email&utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) was a bit more competitive, with Postmark coming up top for most models.

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/6fcbc2e2-ce68-481d-92c4-f225aa40bfdf/image.png?t=1794340930)

Zeno Rocha, Resend’s co-founder and CEO, joined us to walk through what happens on both sides of that pull request. He says Resend had 4.3 million users and 120,000 paying customers when we recorded. He discovered one of them, Warner Bros. Discovery, by sorting through short .com domains in the customer database on a Saturday.

## The developer gets there before the model

Resend’s first useful bit of distribution was an email template. While Zeno and his team were building the sending API, developers kept telling them how painful it was to get the email itself to look right. So they released React Email. You could build a template with it and send it through SendGrid. They even documented how. By the time a developer needed a different provider, some of Resend’s code was already in the repo. [React Email](https://react.email/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv)

Now try the same journey as an agent comparing providers. Resend’s pricing page has a slider. A person can drag it to the right volume and read off the price. Add `.md` to the URL and the agent gets a table it can read in one request. That’s the kind of change I liked in our [Supabase interview](https://latent.space/p/supabase/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) too. You can ask whether a model has heard of your product. You can also open your own [pricing table](https://resend.com/pricing.md?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) and see whether it can answer a customer’s question.

## 80 tools, or two?

When we recorded, Resend’s MCP server had grown from roughly 20 tools or fewer at launch to more than 80. Zeno wants someone working in a chat client to be able to do what they can do in the dashboard. Sending an email gets the demo working. Inviting a teammate, checking usage and managing billing account for some of the long tail. The design looks especially interesting alongside [Cloudflare’s Code Mode server](https://blog.cloudflare.com/code-mode-mcp/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv), which exposes its API through two tools, `search()` and `execute()`.

Zeno describes opening the collapsed tool calls in his own sessions to see what the agent actually did. In one, the result was too large to fit in context. Reducing the tool count wouldn’t by itself make that response any smaller. For someone designing an MCP server, that’s a cheap experiment worth borrowing: expand the result of a call you make every day and see how much of it the model actually needs.

## What happens after “send”?

Getting an agent to pick the SDK still leaves the entire business of delivering the mail. Take click tracking. A provider can rewrite links through a shared tracking domain so it can count clicks. If one customer uses that domain for phishing and it gets blocklisted, links in another customer’s perfectly legitimate emails can suffer too. Zeno explains why Resend introduced custom tracking domains and why he wants to keep the free tier even though spammers can sign up for it. [Custom tracking domains](https://resend.com/blog/introducing-custom-tracking-domain?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) · [Resend’s $3M safety commitment](https://resend.com/blog/3-million-to-make-email-safer?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv)

Watch the domain setup and pricing walkthroughs if you build developer tools. Stay for the last stretch if you know Zeno from Dracula. We get into what it’s like to build a company with a family and how much of a founder’s job comes down to finding a name that other people actually use.

## Show notes

- [Zeno’s original Dracula announcement](https://x.com/zenorocha/status/395216794249486336?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) — the editor theme he launched in 2013.

- [Zeno on what Claude Code chooses](https://x.com/zenorocha/status/2027757871597822022?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) — his reading of the recommendation study and the differences between model versions.

- [Resend’s MCP server](https://resend.com/docs/mcp-server?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) — the integration behind the tool-count discussion.

- [Anthropic on writing tools for agents](https://www.anthropic.com/engineering/writing-tools-for-agents?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) — concrete approaches to returning less data when a tool result fills the context window.

- [Resend’s anti-abuse handbook](https://resend.com/handbook/success/what-is-our-philosophy-for-anti-abuse?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) — what happens when a new sender signs up and where the team intervenes.

## Chapters

[0:00](https://www.youtube.com/watch?v=bdIoODXsu8k&t=0s&utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) Cold open\
[0:59](https://www.youtube.com/watch?v=bdIoODXsu8k&t=59s&utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) From Dracula to email\
[5:02](https://www.youtube.com/watch?v=bdIoODXsu8k&t=302s&utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) When agents pick the API\
[6:54](https://www.youtube.com/watch?v=bdIoODXsu8k&t=414s&utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) Getting into the inbox\
[13:28](https://www.youtube.com/watch?v=bdIoODXsu8k&t=808s&utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) What tracking measures\
[17:05](https://www.youtube.com/watch?v=bdIoODXsu8k&t=1025s&utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) Paying for trust\
[19:44](https://www.youtube.com/watch?v=bdIoODXsu8k&t=1184s&utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) The DNS hurdle\
[23:55](https://www.youtube.com/watch?v=bdIoODXsu8k&t=1435s&utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) React Email’s wedge\
[27:01](https://www.youtube.com/watch?v=bdIoODXsu8k&t=1621s&utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) Measuring adoption\
[31:58](https://www.youtube.com/watch?v=bdIoODXsu8k&t=1918s&utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) Owning the mail stack\
[37:55](https://www.youtube.com/watch?v=bdIoODXsu8k&t=2275s&utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) Designing the MCP\
[46:29](https://www.youtube.com/watch?v=bdIoODXsu8k&t=2789s&utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) CLI, SDKs and craft\
[52:41](https://www.youtube.com/watch?v=bdIoODXsu8k&t=3161s&utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) Mail for agents\
[58:28](https://www.youtube.com/watch?v=bdIoODXsu8k&t=3508s&utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) Writing for machines\
[1:06:00](https://www.youtube.com/watch?v=bdIoODXsu8k&t=3960s&utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) Winning recommendations\
[1:12:14](https://www.youtube.com/watch?v=bdIoODXsu8k&t=4334s&utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) Enterprise and pricing\
[1:16:51](https://www.youtube.com/watch?v=bdIoODXsu8k&t=4611s&utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) Building with a family\
[1:23:26](https://www.youtube.com/watch?v=bdIoODXsu8k&t=5006s&utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) Naming a category

|                                                                                                                                       |                                                            |
| ------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------- |
| ![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/static_assets/file_attachment.png) | Resend - Zeno Rocha - Transcript.txt 94.65 KB • PLAIN File |
|                                                                                                                                       |                                                            |
