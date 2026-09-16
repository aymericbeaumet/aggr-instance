---
title: 'Fragments: September 16'
link: https://martinfowler.com/fragments/2026-09-16.html
source: martinfowler-com
published: 2026-09-16T20:05:00Z
updated: 2026-09-16T20:05:00Z
first_seen: 2026-09-16T20:24:49.200228376Z
authors:
- Martin Fowler
content: extracted
html: 2026-09-16-fragments-september-16.html
preview:
  file: 2026-09-16-fragments-september-16.preview-fbde5f67ca0e.webp
  width: 256
  height: 128
  color: '#301b59'
images:
- source: https://martinfowler.com/fragments/fragments-thumb.png
  original:
    file: 2026-09-16-fragments-september-16.image-1f897b24c40a.png
    width: 600
    height: 300
  color: '#070d4a'
---

Reports of agentic hacking continue, in this case it happened back in May and it seems OpenAI did not disclose that they were responsible. [Simon Willison sees two options:](https://simonwillison.net/2026/Sep/12/openai-agents-rubygems/)

> - After the Hugging Face and Wiki attacks OpenAI were still unable to review their previous logs and determine that they had previously attacked RubyGems.
> - They knew about the attack on RubyGems and made the decision not to reach out to the RubyGems team about it.
>
> Both of these are bad!
>
> Given this incident, the Hugging Face situation, and the Wiki attack, the obvious question right now is how many more incidents like this are out there waiting to be discovered?

 ❄                ❄                ❄                ❄                ❄

[Dave Farley:](https://bsky.app/profile/davefarley77.bsky.social/post/3mv3b4zawds24)

> Stop asking the sci-fi question: ‘Is it conscious?’ Start asking the engineering question: ‘Is this a powerful, unpredictable component being put somewhere consequential, and where’s the feedback that tells us that it’s safe?

 ❄                ❄                ❄                ❄                ❄

Nate Silver is known for his forecasts, but to do them he writes a lot of code for his models. He’s found [agentic programming capable of doing miraculous work](https://www.natesilver.net/p/were-not-ready-for-superpersistent).

> In spending so much time with the LLMs, I’m super attentive to improvements in their capabilities. And these changes tend not to be so linear. Instead, they improve in step functions, almost as phase changes. Suddenly, the models just start doing things capably that they were screwing up before. In my experience, there was a big leap forward when reasoning models first came out in late 2024/early 2025 — enough that they were occasionally useful for tasks involving data and not just words — and then another one this past winter.
>
> The most recent changes I’ve noticed, however, have had less to do with intelligence and more with persistence.

Consider the Hugging Face attack. Although these agents showed remarkable intelligence, they weren’t really super-intelligent - but they were super-persistent. This is a common theme of AI in its various forms:

> Game engines like AlphaGo Zero start out by basically making random moves — but by playing against themselves millions of times, they eventually far surpass human capabilities

As we try to figure out what kind of regulations we need to keep AI under control, we need to remember that we should design our guards around super-persistence as much as worrying about super-intelligence.

 ❄                ❄                ❄                ❄                ❄

“Uncle Bob” Martin has made many posts on X during the last few months about his programming with LLMs. His approach has been to build a firm harness to keep them under control, so they create software that is maintainable as well as functional. Sadly the posts have been frustratingly light on detail. But now it seems [that lack of information may not matter](https://x.com/unclebobmartin/status/2098432570887217520)

> And while I was heads-down getting that to work, the agents got a LOT better. So much so that when I came up for air, the need for my harness was obviated. Indeed, the need for *any* but the most liberal of harnesses may be obviated.

 ❄                ❄                ❄                ❄                ❄

Some tidbits that struck me from Ezra Klein’s recent (recommended) interview with Matt Sheehan on the interplay between [regulation of AI and competition with China](https://www.nytimes.com/2026/09/15/opinion/ezra-klein-podcast-matt-sheehan.html).

- While Chinese models have made some surprisingly remarkable gains in the slipstream of US frontier models, the US still has 8 times as much compute available to it than China - which is a material gap.
- People in the US worry that regulation will slow down the US model builders, but these rapid recent gains in China have occurred under much more regulation
- Americans say that when they set up a hotline to talk to Chinese leaders in a crisis, the Chinese don’t pick up the phone. But this misunderstands the Chinese system. Individual Chinese, even powerful ones, aren’t given individual decision-making power. They operate with committees and documents. So the Americans are better off sending a fax than trying to call an individual
- Like so many things, effective regulation needs regular practice

> When American policymakers are like: Where do you start? — I sometimes say: Well, you start by starting. You learn how to regulate things, you learn how to legislate on them by regulating and legislating on them.
