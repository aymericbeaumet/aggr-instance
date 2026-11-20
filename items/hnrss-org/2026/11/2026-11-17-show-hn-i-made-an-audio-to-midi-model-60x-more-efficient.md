---
title: 'Show HN: I made an audio-to-MIDI model 60x more efficient'
link: https://kobi.music/research/dettato
source: hnrss-org
published: 2026-11-17T01:03:05Z
updated: 2026-11-17T01:03:05Z
first_seen: 2026-11-20T22:37:07.598874728Z
authors:
- hidude562
summary: 'I released dettato, my open weight audio to midi transcription model. It beats muscriptor 1.3b on every set and is roughly 60x smaller, without training on copyrighted recordings. Feel free to ask questions Comments URL: https://news.ycombinator.com/item?id=50526023 Points: 212 # Comments: 46'
content: extracted
html: 2026-11-17-show-hn-i-made-an-audio-to-midi-model-60x-more-efficient.html
remote_preview:
  url: https://kobi.music/og.png
  alt: 'kobimusic: sheet music from anything, music transcription research'
extra:
  content:interactive: true
---

![A low-poly pink bird, drawn ten times in a loop, listening](https://kobi.music/media/dettato/title.svg)

we're introducing Dettato, a SOTA audio to MIDI transcription model that significantly beats MuScriptor 1.3B - the next best open model - and scores a bit higher than mirelo's closed source model 'Mirelo A2M v1.1 Pro' on their published benchmarks. all at just 20m parameters - roughly 65x smaller than muscriptor. Additionally, in training the model, we trained on absolutely no copyrighted recordings. the model is [open source](https://github.com/kobimusic/dettato) and you can use its outputs for commercial purposes.

the composite, and the measures it is the mean of, for each system
| system | composite | onsets | frames | offsets | drums         | parameters |
| ------ | --------- | ------ | ------ | ------- | ------------- | ---------- |
| 60.8   | 70.8      | 79.1   | 40.9   | 52.5    | 20M           |            |
| 56.1   | 67.1      | 75.7   | 40.1   | 41.4    | not published |            |
| 47.4   | 57.0      | 71.2   | 31.4   | 30.1    | 1.3B          |            |
| 45.7   | 58.2      | 63.9   | 30.5   | 30.0    | 46M           |            |

sets used: PHENICX, RWC classical, RWC royalty-free

F1 per set: the mean of onset, frame and offset

F1 per set: the mean of onset, frame and offset
| set  | dettato      | mirelo A2M v1.1 pro | muscriptor 1.3B | yourmt3+ |
| ---- | ------------ | ------------------- | --------------- | -------- |
| 91.3 | not reported | 54.7                | 57.9            |          |
| 60.8 | not reported | 35.5                | 28.0            |          |
| 64.8 | 60.9         | 54.4                | 44.8            |          |
| 54.0 | not reported | 44.3                | 36.1            |          |
| 64.4 | 65.8         | 58.4                | 62.4            |          |
| 48.7 | not reported | 42.6                | 35.6            |          |
| 55.7 | not reported | 52.0                | 47.1            |          |
| 61.6 | 56.2         | 46.8                | 45.4            |          |
| 68.4 | not reported | 51.1                | not reported    |          |

disclaimer with the benchmarks: it's difficult to gauge the true accuracy of the model for your use-case without trying it yourself. one thing my model performs poor on for instance, is splitting instruments correctly. In addition, this just spits out performance MIDI with a beat aligned. If you're looking for something that you can actually play(e.g sheet music), i'd reccommend [songscription](https://www.songscription.ai) or [klangio](https://klang.io) - both are excellent tools by great companies that push the envelope of what's possible in this field forward.

0:00 / 0:00 recording dettato

sousa, the stars and stripes forever: “the president's own” united states marine band, from the complete marches of john philip sousa, vol. 3. a us government recording, public domain. [download the MIDI](https://kobi.music/media/dettato/samples/stars-and-stripes-forever.mid)

upload your own example (or pick a demo below)

the driving goal of kobimusic: to help musicians, not replace them. a part of that goal means following copyright law. we trained dettato on a mix of synthetic instrument recordings and CC solo instruments mixed in using the URMP dataset. the rest of the instruments morph together ~20 soundfonts, using the ['soundfont babies'](https://kobi.music/research/making-soundfont-babies) technique i published. no copyrighted recordings were used.

I began working on dettato about 10 months ago, but scrapped what I had in about April. The reason why was that I treated AMT as a data problem - but in reality, the bottleneck is architecture. There are a lot of parellels that you can draw for OMR (first model i made) and AMT. both traditionally were tackled early on using algorithms. later, in both fields they plugged in giant LLM components. These models would naturally become overfit to synthetic data, since globally the problems of AMT and OMR become different than if you treat it locally. But, for both you can reframe it as a local problem. For OMR, using a detector model. For AMT, a pitch-lane model.

overall, i hope both open and closed weight music retrieval companies can learn from this. The constraint of not using copyrighted recordings doesn't hold you back, but actually encourages innovation, and it's likely you'll end up with a better model in the end.
