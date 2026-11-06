---
title: Egyptian and Nigerian English join the Open ASR Leaderboard
link: https://huggingface.co/blog/open-asr-leaderboard-liva
source: huggingface-co
published: 2026-11-06T00:00:00Z
updated: 2026-11-06T00:00:00Z
first_seen: 2026-11-06T11:27:16.888185679Z
content: extracted
html: 2026-11-06-egyptian-and-nigerian-english-join-the-open-asr-leaderboard.html
remote_preview:
  url: https://huggingface.co/blog/assets/open-asr-leaderboard-liva/thumbnail.png
---

[Back to Articles](https://huggingface.co/blog)

[![Eric Bezzam's avatar](https://cdn-avatars.huggingface.co/v1/production/uploads/6384db7fb2906edaf835a91d/MOTXxaOmjlTZ8wONYifnD.jpeg)](https://huggingface.co/bezzam)

[![Ashley Mo's avatar](https://huggingface.co/avatars/d380b48d9b44431a5376b465daf057fb.svg)](https://huggingface.co/ashleymo2025)

[![Aoi Otani's avatar](https://cdn-avatars.huggingface.co/v1/production/uploads/64d5eff05900b6d111c9e1ad/hGnt9yx6LyQP5a2MBpXGg.jpeg)](https://huggingface.co/aonorii)

A few months ago, we added private data to the [Open ASR Leaderboard](https://huggingface.co/spaces/hf-audio/open_asr_leaderboard): 11 splits from Appen and DataOceanAI covering scripted and conversational speech from five accents ([blog](https://huggingface.co/blog/open-asr-leaderboard-private-data)). The community response was great, with many **highlighting the importance of held-out sets for trustworthy evaluations**. Since then, we added one more private split for Indian-accented English with Voice Arena ([blog](https://huggingface.co/blog/open-asr-leaderboard-global-south)).

We’re excited to announce a new collaboration with [Liva AI](https://huggingface.co/liva-ai) to add even more private data with two new accents: **Egyptian** 🇪🇬 and **Nigerian** 🇳🇬.

| Dataset                   | Accent   | Duration \[h\] | Style                       | Transcription                         |
| ------------------------- | -------- | -------------- | --------------------------- | ------------------------------------- |
| Liva AI Conversational EG | Egyptian | 1.34           | Conversational, spontaneous | Punctuated, cased, with disfluencies. |
| Liva AI Conversational NG | Nigerian | 1.55           | Conversational, spontaneous | Punctuated, cased, with disfluencies. |

This is part of our continued effort to improve representation in ASR evals (more accents and more languages) and to improve trust (new private data to minimize benchmaxxing, and new data providers). We’re excited to see the community continuing to push ASR performance for the world 🤗

## Richer accent breakdown in the Private tab

With these new accents, we can now present a richer breakdown of accents, based on [Kachru’s Three Circles](https://en.wikipedia.org/wiki/World_Englishes#Kachru%27s_Three_Circles_of_English) (kudos to Hume AI for this idea in their [leaderboard](https://huggingface.co/spaces/HumeAI/asr-leaderboard)). The Open ASR Leaderboard will now report:

1. US vs. non-US (*UK, Australia, Canada, India, Nigeria, Egypt*)
2. Inner Circle (*US, UK, Australia, Canada*) vs. Outer & Expanding Circles (*India, Nigeria, Egypt*)

As more accents join, we’ll split the Outer and Expanding Circles.

![thumbnail](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/open_asr_leaderboard/private_data_accent_breakdown.png)

## About Liva AI

Liva AI collects multimodal interaction data through consumer products used by real people. Because Liva owns both the collection environment and distribution, it can design products around specific model gaps across domains, industries, languages, modalities, and interaction types. This gives it access to natural, high-context behavior that emerges through real product use, rather than artificially elicited or task-based interactions.
