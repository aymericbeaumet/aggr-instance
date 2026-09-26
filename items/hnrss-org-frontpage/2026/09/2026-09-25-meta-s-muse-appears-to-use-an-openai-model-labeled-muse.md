---
title: Meta's Muse appears to use an OpenAI model labeled muse-special
link: https://mouse.dev/blog/muse-special/
source: hnrss-org-frontpage
published: 2026-09-25T18:18:06Z
updated: 2026-09-25T18:18:06Z
first_seen: 2026-09-26T00:33:08.793159958Z
authors:
- Aeroi
summary: 'Article URL: https://mouse.dev/blog/muse-special/ Comments URL: https://news.ycombinator.com/item?id=49848095 Points: 106 # Comments: 45'
content: extracted
html: 2026-09-25-meta-s-muse-appears-to-use-an-openai-model-labeled-muse.html
preview:
  file: 2026-09-25-meta-s-muse-appears-to-use-an-openai-model-labeled-muse.preview-1b9db4918eeb.webp
  width: 256
  height: 134
  alt: Is Meta’s Muse secretly running an OpenAI model? | Mouse
  color: '#1f1f1f'
images:
- source: https://mouse.dev/og/muse-special.png
  original:
    file: 2026-09-25-meta-s-muse-appears-to-use-an-openai-model-labeled-muse.image-bf38dc67704d.png
    width: 1200
    height: 630
  variants:
  - file: 2026-09-25-meta-s-muse-appears-to-use-an-openai-model-labeled-muse.image-85db20d0ef2d.webp
    width: 320
    height: 168
  - file: 2026-09-25-meta-s-muse-appears-to-use-an-openai-model-labeled-muse.image-9eeed99d6357.webp
    width: 640
    height: 336
  - file: 2026-09-25-meta-s-muse-appears-to-use-an-openai-model-labeled-muse.image-80c603d7e610.webp
    width: 960
    height: 504
  - file: 2026-09-25-meta-s-muse-appears-to-use-an-openai-model-labeled-muse.image-fa5edb3153db.webp
    width: 1200
    height: 630
  color: '#0a0a0a'
- source: https://mouse.dev/img/blog/muse-models/hero.webp
  original:
    file: 2026-09-25-meta-s-muse-appears-to-use-an-openai-model-labeled-muse.image-6e0f1507120c.webp
    width: 900
    height: 900
  color: '#f9f8f8'
---

I found a model labeled `azure/muse-special` while Muse was building my website. So I dug deeper.

This is Part 2 of digging through the Muse filesystem after [my article](https://mouse.dev/blog/muse-runtime-export) hit the front page of Hacker News this week.

In this article I focus on a model I found in my logs called `muse-special`, and confront the question: does Muse actually use OpenAI and Claude models behind the scenes?

![A fuzzy mascot holding a sign that reads azure/muse-special](https://mouse.dev/img/blog/muse-models/hero.webp)

## One odd session

Muse records which model each agent session uses.

Nearly every session log in my VM was routed to Meta’s internal model, called Avocado.

But one subagent used a model named `azure/muse-special`.

Interesting...

Figure 1. Sessions in my VM grouped by model. Everything is Avocado except a single azure/muse-special session on September 21. Click image to enlarge.

## Following the name

This made me curious, so I searched across the repo inside Cursor and found this:

“GPT Responses model client via MAGI native Azure OpenAI lane.”

OK.

The model catalogue seems to list `azure/muse-special` then `azure/gpt-5.6-sol`.

So I searched my session transcripts...

Here I found two details that stood out:

1. The signature is tagged `gpt_responses_v1` and contains an encrypted payload starting with `gAAAAA` (which OpenAI uses).
2. Tool call IDs used `call_` followed by 24 mixed-case characters.

This was different from all the other lines that the Avocado sessions printed (`call_` followed by 32 hex characters).

Figure 2. Lines from the muse-special transcript: a call\_ ID in the OpenAI style and a gpt\_responses\_v1 signature with an encrypted gAAAAA payload. Click image to enlarge.

These little details tell me that the `muse-special` model is possibly an OpenAI model or OpenAI’s Responses API.

So is `muse-special` an alias for a GPT model served through Azure?

The files and logs don’t tell me exactly which GPT model, or why it was selected by the subagent in the first place, but let’s take a step back and explore further...

## The model catalogue

The broader model catalogue that is shipped with Muse’s agent daemon lists about 15 versions of Avocado, plus:

- Claude Opus 4.6 / 4.7 / 4.8
- Sonnet 4.6 and Haiku 4.5
- GPT-5.5 and GPT-5.6 variants via OpenAI, Azure and Codex
- Kimi K3 through Fireworks and Meta-hosted routes

Figure 3. My summary of the model IDs shipped in the hatch daemon, grouped by family. A shipped ID means the runtime can address it, not that it was used. Click image to enlarge.

## The Anthropic plumbing

The Claude support goes beyond just the model ID and includes an Anthropic client with request handling, prompt conversion and streaming parsers:

- `anthropic/request_flow.rs`
- `anthropic/convert_prompt.rs`
- `anthropic/parse_sse_stream.rs`

OK, so now we’re kind of wondering... why?

There are API key files present for Anthropic, OpenAI, etc., with access restricted to the inference-proxy service.

...But there’s also a proxy kill-switch setting in the env.

Figure 4. JARVIS\_ANTHROPIC\_BASE\_URL\_REVPROXY\_OVERRIDE=0 in the runtime env. The comment calls it a live kill switch, not stale config. Click image to enlarge.

## Why ship all of this?

Now, there are a few reasons for this, I guess.

1. The first would be that an OpenAI or Anthropic model just does a superior job at a certain task that Muse can’t fulfill right now, and they selectively route for that.
2. The second is that all these VMs are shipped with the ability to A/B test model responses, tool calls, etc. for the purpose of distillation and RL.

Distillation or RL? Maybe. Idk.

This leads us to the truth, which is that the model behind Muse is ultimately a server-side choice.

The runtime has clients for multiple providers, which gives Meta the ability to change routing without asking users.

In my case there was only a single outlier session that didn’t use the Avocado (Meta) model, but the infra is there to.

Wait, so is Meta distilling from the other frontier labs?

(Getting technical. tl;dr: No.)

With the `muse-special` model the raw reasoning is encrypted. The daemon stores it to send back to Azure on the following turn. In the binary it explicitly says that the encrypted reasoning cannot use the RL completion-server override.

So what Meta can see here is only the reply, the tool calls, and a short reasoning summary when OpenAI/Anthropic returns one. The raw chain of thought is encrypted and the RL server refuses those blobs. There is no indication that Meta copies OpenAI or Anthropic weights.

Avocado models are treated differently, however. The thinking text is written directly into the transcript, with an empty signature, and available for RL use.

So Avocado models, according to the privacy note and repo, do indicate that conversations can be used to develop AI at Meta unless you opt out. (Makes sense.)

## Closing thoughts

This is my own exploration of what has been a very cool release from Meta.

My best guess is that `muse-special` is an OpenAI model served through Azure.

Whatever you think of Meta, the talent they brought onto this project deserves credit. They took a different approach in a world full of chatbots and search bars, and the exec team’s response to my first article, which got some eyeballs, has been pretty amazing too, as has their willingness to reach out to a nobody and explain their thinking.

It’s not every day you get to look inside the filesystem of a product that could reach hundreds of millions of people.

Seeing inside a runtime cell gives us an early look at where this whole personal agent thing might be going, and it’s been really fascinating to read through all of it this week.

If you worked on Muse at all, please feel free to reach out. I’d love to learn more and perhaps contribute.

Things seem to be moving fast. Not really breaking, yet.

pete at mouse dot dev

-Pete

[@heypeterjames](https://x.com/heypeterjames)
