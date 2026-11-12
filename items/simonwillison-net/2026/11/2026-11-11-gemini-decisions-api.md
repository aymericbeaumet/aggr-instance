---
title: Gemini Decisions API
link: https://simonwillison.net/2026/Nov/11/gemini-decisions-api/
source: simonwillison-net
published: 2026-11-11T22:03:25Z
updated: 2026-11-11T22:03:25Z
first_seen: 2026-11-12T02:51:23.435329786Z
labels:
- ai
- decision-models
- gemini
- generative-ai
- jev
- llm-release
summary: 'Gemini Decisions API Google join Jev and OpenAI in the decision model category with "Gemini Decision 1". This new model API is almost identical to Jev: it supports the same three types of questions (yes/no, choice, and rubric-scoring) and even reuses Jev''s term "Noul" for a yes/no question. It''s priced the same as the GPT-6 Luna decision model: 10 cents per million input tokens, and output is free. Jev remains the cheapest of the three, at 4.2 cents per million. Jev is text (and JSON) only. OpenAI can handle text and images. Gemini Decision 1 is fully multi-model: it covers text, images, audio, video, and PDFs. I tested it against video by passing these questions to it along with my Yonkers stomping robot video file: dragon: Does this video have a dragon in it? robot: Does this video have a robot in it? france: Is this video set in France? usa: Is this video set in the USA? Jev and OpenAI both return 1.0 or 0.0 for yes/no questions, but Gemini insists on being a little less confident than that: { "dragon": { "type": "yes_no", "probability": 0.04976021841520312 }, "robot": { "type": "yes_no", "probability": 0.9234055387792485 }, "france": { "type": "yes_no", "probability": 0.08329171353166241 }, "usa": { "type": "yes_no", "probability": 0.9230459995157706 } } Via Hacker News Tags: ai, generative-ai, gemini, llm-release, jev, decision-models'
content: extracted
html: 2026-11-11-gemini-decisions-api.html
---

**[Gemini Decisions API](https://blog.google/innovation-and-ai/technology/developers-tools/gemini-decisions-api/)** ([via](https://news.ycombinator.com/item?id=50456791 "Hacker News")) Google join [Jev](https://simonwillison.net/2026/Sep/21/jev/) and [OpenAI](https://simonwillison.net/2026/Oct/6/llm-openai-decisions/) in the decision model category with "Gemini Decision 1". This new model API is almost identical to Jev: it supports the same three types of questions (yes/no, choice, and rubric-scoring) and even reuses Jev's term "Noul" for a yes/no question.

It's priced the same as the GPT-6 Luna decision model: 10 cents per million input tokens, and output is free. Jev remains the cheapest of the three, at 4.2 cents per million.

Jev is text (and JSON) only. OpenAI can handle text and images. Gemini Decision 1 is fully multi-model: it covers text, images, audio, video, and PDFs.

I tested it against video by passing these questions to it along with my [Yonkers stomping robot](https://simonwillison.net/2026/Nov/10/yonkers-rises/) video file:

```
dragon: Does this video have a dragon in it?
robot: Does this video have a robot in it?
france: Is this video set in France?
usa: Is this video set in the USA?
```

Jev and OpenAI both return 1.0 or 0.0 for yes/no questions, but Gemini insists on being a little less confident than that:

```json
{
  "dragon": {
    "type": "yes_no",
    "probability": 0.04976021841520312
  },
  "robot": {
    "type": "yes_no",
    "probability": 0.9234055387792485
  },
  "france": {
    "type": "yes_no",
    "probability": 0.08329171353166241
  },
  "usa": {
    "type": "yes_no",
    "probability": 0.9230459995157706
  }
}
```
