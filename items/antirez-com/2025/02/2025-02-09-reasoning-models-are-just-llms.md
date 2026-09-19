---
title: Reasoning models are just LLMs
link: http://antirez.com/news/146
source: antirez-com
published: 2025-02-09T18:19:38Z
updated: 2025-02-09T18:19:38Z
first_seen: 2026-09-19T21:30:23.395188495Z
summary: 'It’s not new, but it’s accelerating. People that used to say that LLMs were a fundamentally flawed way to reach any useful reasoning and, in general, to develop any useful tool with some degree of generality, are starting to shuffle the deck, in the hope to look less wrong. They say: “the progresses we are seeing are due to the fact that models like OpenAI o1 or DeepSeek R1 are not just LLMs”. This is false, and it is important to show their mystification as soon as possible. First, DeepSeek R1 (don’t want to talk about o1 / o3, since it’s a private thing we don’t have access to, but it’s very likely the same) is a pure decoder only autoregressive model. It’s the same next token prediction that was so strongly criticized. There isn’t, in any place of the model, any explicit symbolic reasoning or representation. Moreover, R1 Zero has similar reasoning capabilities of R1 without requiring *any* supervised fine tuning, just generating chain of thoughts, and improving it with a reward function, using reinforcement learning, was enough to learn a stronger form of reasoning. Interestingly enough, part of these capabilities were easily distilled into smaller models via SFT, which brings me to the next point. The other fundamental observation is that the S1 paper shows that you need very few examples (as little as 1000) in order for the model to start being able to build complex reasoning steps and solve non trivial mathematical problems. S1, and R1 Zero, hint that in some way in the pre-training step the models already learned the representations needed in order to perform reasoning, just with the unsupervised next word prediction training target. So it’s not just that R1 is a vanilla LLM in its fundamental structure, but also the unsupervised pre-training creates enough representations and potential that, powerful enough LLMs, with RL (and/or some minor SFT), learn to reply to complex questions the users pose (I''m referring to instruct models, an old but yet impressive capability) and to use chain of thoughts to reason about things and provide better answers. Reasoning models are just LLMs, and who said LLMs were a dead end was just wrong. Now, to be wrong happens (even if the version of being wrong, in this instance, was particularly aggressive, particularly in denial of evidences). However, trying to change the history and the terminology in order to be in the right side is, for me, unacceptable. Comments'
content: extracted
html: 2025-02-09-reasoning-models-are-just-llms.html
---

## [Reasoning models are just LLMs](https://antirez.com/news/146)

[antirez](https://antirez.com/user/antirez) 587 days ago. 108179 views.

```
It’s not new, but it’s accelerating. People that used to say that LLMs were a fundamentally flawed way to reach any useful reasoning and, in general, to develop any useful tool with some degree of generality, are starting to shuffle the deck, in the hope to look less wrong. They say: “the progresses we are seeing are due to the fact that models like OpenAI o1 or DeepSeek R1 are not just LLMs”. This is false, and it is important to show their mystification as soon as possible.

First, DeepSeek R1 (don’t want to talk about o1 / o3, since it’s a private thing we don’t have access to, but it’s very likely the same) is a pure decoder only autoregressive model. It’s the same next token prediction that was so strongly criticized. There isn’t, in any place of the model, any explicit symbolic reasoning or representation.

Moreover, R1 Zero has similar reasoning capabilities of R1 without requiring *any* supervised fine tuning, just generating chain of thoughts, and improving it with a reward function, using reinforcement learning, was enough to learn a stronger form of reasoning. Interestingly enough, part of these capabilities were easily distilled into smaller models via SFT, which brings me to the next point.

The other fundamental observation is that the S1 paper shows that you need very few examples (as little as 1000) in order for the model to start being able to build complex reasoning steps and solve non trivial mathematical problems. S1, and R1 Zero, hint that in some way in the pre-training step the models already learned the representations needed in order to perform reasoning, just with the unsupervised next word prediction training target.

So it’s not just that R1 is a vanilla LLM in its fundamental structure, but also the unsupervised pre-training creates enough representations and potential that, powerful enough LLMs, with RL (and/or some minor SFT), learn to reply to complex questions the users pose (I'm referring to instruct models, an old but yet impressive capability) and to use chain of thoughts to reason about things and provide better answers.

Reasoning models are just LLMs, and who said LLMs were a dead end was just wrong. Now, to be wrong happens (even if the version of being wrong, in this instance, was particularly aggressive, particularly in denial of evidences). However, trying to change the history and the terminology in order to be in the right side is, for me, unacceptable.
```

[blog comments powered by Disqus](https://disqus.com)
