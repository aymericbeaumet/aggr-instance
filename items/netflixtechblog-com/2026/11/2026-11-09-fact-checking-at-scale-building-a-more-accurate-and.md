---
title: 'Fact-Checking at Scale: Building a More Accurate and Efficient Synopsis Factuality System'
link: https://netflixtechblog.com/fact-checking-at-scale-building-a-more-accurate-and-efficient-synopsis-factuality-system-d7d85859d808?source=rss----2615bd06b42e---4
source: netflixtechblog-com
published: 2026-11-09T17:01:02Z
updated: 2026-11-09T17:01:02.202Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Netflix Technology Blog
content: feed
html: 2026-11-09-fact-checking-at-scale-building-a-more-accurate-and.html
remote_preview:
  url: https://cdn-images-1.medium.com/max/466/1*zkhDdQjtUryvyXdFAoMJoQ.png
---

*By* [*Shreya S Ramanujam*](https://www.linkedin.com/in/shreya-s-ramanujam)*,* [*Yongce Li*](https://www.linkedin.com/in/yongceli)*,* [*Afshine Amidi*](https://www.linkedin.com/in/afshineamidi)*,* [*Sina Ghiassian*](https://www.linkedin.com/in/sina-ghiassian-23452b61)*,* [*Benjamin Klein*](https://www.linkedin.com/in/benjamin-klein-usa)*.*

### Introduction

Picture this — you’re home after a long day at work and can’t wait to catch up on Bridgerton. You log on to Netflix, search for “Bridgerton” and land on this screen:

![](https://cdn-images-1.medium.com/max/466/1*zkhDdQjtUryvyXdFAoMJoQ.png)

If this sounds familiar, you already know what a synopsis is — it’s a short glimpse into what a viewer can expect to see in a show. We see two types of synopses here: a *series-level* synopsis describing the overarching plot, and multiple *episode-level* synopses describing each episode.

A synopsis plays a very important role for us — it helps viewers decide whether a title is worth watching or not. Every synopsis needs to strike a balance: sharing *just* enough to give viewers a sense of what to expect, while leaving enough intrigue to draw them in. But of course, we can’t just write *anything*.

> *“In the fog-drenched streets of Victorian London, Detective Daphne Bridgerton races to unmask a serial killer known only as the elusive Lady Whistledown.”*

Intriguing? Definitely. But it is most certainly *not* the premise of Bridgerton. Which leads us to the first (and most important) rule of writing a good synopsis: it must be *factual*. At Netflix’s scale, with thousands of titles, multiple episodes per season and multiple languages, fact-checking all of these by hand simply doesn’t scale. This is where our factuality service comes in: an automated way to judge whether a synopsis is factual or not.

Our current factuality checker feeds a reasoning model everything we know about a title (like metadata and text assets) and asks it to judge synopsis factuality according to a rigorous rubric. There are two main limitations of this approach that we want to optimize:

1. **High cost of long-context reasoning calls:** Each decision requires expensive, long-context model calls to reasoning models, which doesn’t scale well when we want to check multiple titles, episodes, languages, and style variants.
2. **Misaligned rationales:** The model’s explanations often diverge from our annotators’ reasoning, which makes its factuality verdicts less reliable. We want these rationales to be better aligned so that (1) the model can consistently flag nuanced errors, and (2) its explanations can be used downstream to automatically correct issues in the synopsis.

Our new approach tackles both problems at once. On the cost side, it delivers a **52.4% reduction** per synopsis. On the quality side, it significantly improves both verdict accuracy and reasoning alignment: **Recall–Fail** (*“Of all truly non-factual synopses, how many does the system correctly reject?”*) rises from **0.77 to 0.86**, and the new factuality service’s explanations now show ~**82% overlap** with human rationales (up from ~**69%**). In other words, it’s not just cheaper and more accurate — it’s also much better aligned with how our expert annotators *actually* judge synopses.

### So, what *exactly* does factuality mean?

Let’s go back to the Bridgerton example. Say you see this synopsis about Bridgerton Season 1:

> **Synopsis A:** *“Bridgerton follows Daphne Bridgerton, the eldest daughter of the powerful Bridgerton family as she makes her debut onto Regency London’s competitive marriage market.”*

It’s easy to judge that this synopsis is factually correct — no wrong facts, and it gives the reader a clear, accurate glimpse of what to expect from the show.

Let’s take another example:

> **Synopsis B:** *“Set in modern-day London, this gritty crime drama follows Detective Daphne Bridgerton as she hunts a serial killer while hiding her own criminal past.”*

Another easy one — the plot is so blatantly off the mark that it is definitely *not* factual.

But what about this?

> **Synopsis C:** *“Bridgerton follows the mysterious, sharp-tongued Lady Whistledown as she exposes the secrets of London’s high society through her scandalous gossip sheet.”*

*Technically*, every statement here is true — Lady Whistledown does indeed publish a gossip sheet, and if there’s one thing she is known for, it’s her scathing exposés. So then, is this a factual synopsis?

Not quite! While technically true, it paints a misleading picture of the show. A new viewer might reasonably expect it to revolve around Lady Whistledown, rather than Daphne navigating the marriage market. This, too, is a type of non-factuality we want to catch.

Lastly, here’s the trickiest of all:

> **Synopsis D:** *“When Daphne Bridgerton makes her society debut, she catches the eye of the aloof Duke of Hastings, and the two begin a passionate love affair that sets the whole ton abuzz.”*

Again, every statement is true, but the phrasing is misleading. A new viewer might assume they fall in love immediately, missing the fact that their relationship begins as a fake, mutually beneficial arrangement. This is another type of nuanced error our system should flag.

So now that we have an idea of the kinds of errors we care about, the real question is: how do we build a system that is able to catch both glaring as well as subtle factuality violations?

### Current Service and Its Limitations

Our current factuality pipeline is a straightforward, holistic LLM call. We feed a reasoning model everything we know about a title (text assets, metadata, and a detailed factuality rubric) and ask it to judge the factuality of a synopsis. We repeat the call several times to reduce output randomness, and if *any* run flags the synopsis as non-factual, it fails. This approach has two main limitations:

1. **High cost:** Each judgment requires multiple (five, to be exact) calls to a large reasoning model over extremely long contexts (100k+ tokens), which does not scale well.
2. **Misaligned reasoning:** The model can fail synopses for nitpicky reasons (e.g., “This is a show about wonderful magic” gets failed because the characters never *literally* call the magic “wonderful”) while overlooking nuanced errors by convincing itself they “don’t really make the synopsis false.” This misalignment makes verdicts less reliable and rationales less useful for downstream correction.

### Optimizing the Holistic Pipeline

To understand exactly where we should optimize the current holistic pipeline, we study the current pipeline both qualitatively and quantitatively on a custom factuality benchmark that we built. This benchmark comprises synopses hand-labeled by human annotators as factual or non-factual, along with their rationales. This analysis reveals several key insights:

### Insight 1: Do we really need five holistic calls?

Let’s think back to why we added multiple holistic calls in the first place: to reduce sampling noise. In practice, though, we see an interesting dichotomy:

- **Blatantly true / false synopses (Bridgerton A, B):** All runs agree on the verdict with near-identical reasoning.
- **Nuanced errors (Bridgerton C, D):** The model is much more likely to disagree with itself — some runs catch the error, some don’t. This is exactly where additional sampling helps.

This suggests a simple optimization: A and B don’t really need all 5 calls. If we can somehow identify these “easy” synopses upfront, we can cut down on the number of calls for them, while still giving harder cases like C and D the extra calls they need.

The easiest way to do this is to ask the model to output its *confidence* alongside its verdict and rationale. We can then separate “easy” (high confidence) synopses from “hard” (low confidence) ones and only send the hard cases through additional checks. Think of it like airport security: most bags go straight through, but those that trigger the alarm (low confidence) get extra screening.

Implementing this easy fix makes our pipeline go from the current holistic model to a new “confidence gated” holistic model:

![](https://cdn-images-1.medium.com/max/941/1*gnniKSbeJxGNHjrZTRT96A.png)

### Insight 2: Five identical judges calls give us no new information

With confidence gating, our pipeline only escalates harder synopses to the full five holistic calls. But extra calls only help with *sampling noise*, not systematic reasoning errors: if every run follows the same flawed rationale, they’ll all reach the same wrong verdict, rendering the extra calls useless. How can we ensure that each call counts?

**Idea: Diversify the judges and make them specialized**

Instead of running five holistic judges with the same generic factuality prompt, we introduce two specialized judges that individually target the two aspects of factuality we care about:

- **Literal Judge**: Is the synopsis *literally* true?
- **Implicature Judge:** Does this synopsis give a false picture to a new reader who has not watched this show?

Explicitly searching for specific types of errors helps us better flag both literal and subtler implicature errors. In practice, we observe that this works better than repeatedly querying a single holistic judge, hoping that sampling stochasticity will surface nuanced errors.

### Insight 3: The rationale reveals mistakes the verdict hid

At this point, we have a pretty good pipeline, but we’re still underusing one of the richest signals available to us: the model’s rationale behind its verdict. For nuanced errors, we often see the model identify a point of contention, then brush it aside based on a faulty assumption or because it deems the issue too minor to affect the verdict. Can we use these shaky spots in the rationale to catch (and correct) errors the verdict misses?

**Idea: Secondary atomic-claim checking via a router**:

Since our literal and implicature judges are already strong reasoning models, we ask them to additionally *critically analyse their own reasoning and surface shaky spots* — any doubts, concessions, or assumptions that might need rechecking. These include cases where the model:

- Makes an assumption.
- Raises a doubt that the available evidence can’t resolve (e.g., an external critic quote).
- Notices a contradiction but dismisses it as insufficient to change the verdict.

We pass all concessions from both judges to a router model, which:

1. Reframes them as self-contained, verifiable atomic claims, each with a *materiality* field specifying the evidence needed to support or refute it.
2. Routes each claim to the appropriate secondary Stage-2 judge: a **web judge** for external facts (e.g., citations, media quotes, or prior work), or a **source-material recheck** focused on the reframed atomic claim. By pinpointing the exact point of contention, the recheck is more targeted, making it easier to mark the claim as true or false.

### Putting It All Together: Proposed Cascade Factuality Service

Our findings lead to three key improvements: specialized judges, confidence-gated escalation, and targeted rechecking of shaky reasoning using a secondary stage when escalated.

We implement these improvements as a tiered system shown in the figure below: every synopsis first passes through both the literal and the implicature judges. High-confidence verdicts are retained, while low-confidence cases escalate to the router and Stage-2 judges for further checking. Stage 2 can overturn the initial verdict in two cases:

- **FAIL → PASS:** All concessions, including the original failure reasons, are supported by the secondary check.
- **PASS → FAIL:** At least one concession is contradicted.

![](https://cdn-images-1.medium.com/max/1024/1*9S504rMn84FciiQHq_VT8g.png)

### Results

Based on our discussions from above, we evaluate three different pipelines:

![](https://cdn-images-1.medium.com/max/1024/1*6Ul8fCgLcHAe3tYKu3lXMA.png)

### Evaluation: Datasets and Metrics

We evaluate these systems on two datasets: show-level and episode-level synopses. Each datapoint includes a synopsis, a human gold label (factual / not factual), and the annotator’s rationale. For episode-level synopses, we additionally require that the synopsis be specific to that *particular* episode, not just a generic synopsis of the overall show.

We focus on two main axes: Quality and Cost. The quality metrics are listed below:

![](https://cdn-images-1.medium.com/max/1024/1*kITf9rBXCmfwTk8nPxPDCQ.png)

Recall-FAIL is our primary metric: higher recall means fewer Gold FAILs are missed. We also track Precision-PASS, which measures how reliably PASS verdicts identify Gold PASSes, and alignment with human rationales.

On the cost side, we want to reduce both the average number of LLM calls per synopsis and the end-to-end cost of producing a final verdict. Latency is not our primary optimization target, but we do want to ensure the median (P50) latency does not blow up to impractical judgement times.

### Evaluation Results

The best numbers per metric per dataset are shown in bold.

![](https://cdn-images-1.medium.com/max/1024/1*dyTgWrmDFFFZkXgeAw_Fdg.png)

**Note**: We compared the cascade against two holistic ablations — increased reasoning effort and a smaller, cheaper model — to test if simple ablations could achieve better performance. For episode-level synopses, we also added a full-show summary to the prompt to provide context from previous episodes; we observe summaries substantially improve performance when compared to non-summary context variants, so we report only summary-based variants.

Our takeaways from this experiment are:

- **Cascade improves quality.** Our primary metric, Recall-FAIL improves on both show and episode level synopses, with a limited impact on Recall-PASS. In other words, we now catch more Gold FAIL synopses without sacrificing the reliability of our PASS verdicts.
- **Cascade is cheaper**. Cascade cuts total cost by **52.4**% on the show-level dataset. Cascade (with summary) is also **15%** cheaper than the current (no summary) holistic pipeline on episodic dataset.
- **Cascade’s P50 latency increases with respect to the holistic baseline.** Even so, this remains within what we consider a reasonable range for factuality judgment.
- **Simple tweaks aren’t enough.** Increasing reasoning effort makes the factuality checker too lenient, while using a smaller model makes it too strict — neither addresses the underlying problems of the holistic baseline.
- **Summary context matters for episodic tasks.** Without a full-show summary, the model tends to over-fail because it lacks the context to determine whether a detail from the episode summary fits the broader storyline.

### Alignment with Human Rationale

So far, we’ve established that the cascade produces better verdicts: it catches more non-factual synopses and produces more reliable PASS decisions. But are these verdicts correct for the *right reasons*?

To measure this, we use an LLM-as-a-Judge to compare each pipeline’s reasoning against the human rationale on Gold-FAIL (GF) synopses. The judge assigns a Reasoning Similarity Score (RSS) of 0 (dissimilar), 0.5 (partially similar), or 1 (very similar) based on rationale overlap.

![](https://cdn-images-1.medium.com/max/998/1*i7wNx5sDllks5IpPFmqBiQ.png)

We observe that cascade is *significantly* better aligned with human rationale in all cases. This is a big gain — not only is the cascade giving us more accurate factuality verdicts, but it also identifies the *same* points of contention in the synopsis as a specialized human annotator.

### Conclusion + Future Work

Overall, our proposed cascade pipeline improves both cost and quality of the existing factuality service:

- On the **cost** side, confidence gating lets us avoid unnecessary calls, reducing the average cost of factuality evaluation.
- On the **quality** side, two diverse judges allow us to collect complementary informative verdicts, while the Stage-2 judges recheck and correct the assumptions behind uncertain decisions. This results in higher verdict accuracy and substantially better alignment between model and expert annotator reasoning.

The cascade gives us a path toward a factuality system that is not just more accurate, but also *more interpretable* and *self-correcting*. Some future directions to consider would be:

- Adding multimodal inputs to help verify claims that text assets alone cannot confirm.
- Use the rationale from factuality checking to correct the faulty synopsis.

![](https://medium.com/_/stat?event=post.clientViewed&referrerSource=full_rss&postId=d7d85859d808)

* * *

[Fact-Checking at Scale: Building a More Accurate and Efficient Synopsis Factuality System](https://netflixtechblog.com/fact-checking-at-scale-building-a-more-accurate-and-efficient-synopsis-factuality-system-d7d85859d808) was originally published in [Netflix TechBlog](https://netflixtechblog.com) on Medium, where people are continuing the conversation by highlighting and responding to this story.
