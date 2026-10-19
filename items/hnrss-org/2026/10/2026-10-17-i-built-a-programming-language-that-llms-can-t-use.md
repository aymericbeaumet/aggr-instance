---
title: I built a programming language that LLMs can't use
link: https://blog.picheta.me/post/killswitch-lang/
source: hnrss-org
published: 2026-10-17T22:26:47Z
updated: 2026-10-17T22:26:47Z
first_seen: 2026-10-19T06:04:04.553061734Z
authors:
- dom96
content: extracted
html: 2026-10-17-i-built-a-programming-language-that-llms-can-t-use.html
remote_preview:
  url: https://blog.picheta.me/_astro/killswitch-banner.0YTJ74BT.png
---

Writing code is probably something LLMs have excelled at the most. But how much of their ability to write code is due to their understanding of it vs. just them recalling code snippets which were present in their training dataset or their context window?

This is what I set out to answer. In order to do so I decided to design and implement a programming language which, while usable for humans, should be as difficult for LLMs and coding agents as possible.

I figured testing an LLM’s ability to understand and write code in a programming language that it has never seen before was a good start. But I also wanted to include features in the language which exploit the weaknesses of LLMs. For example, their apparent difficulty in counting letters, their over eager safety guard rails and their keenness to follow prompts.

## Introducing KillSwitch

KillSwitch is an esoteric programming language which is designed to be adversarial to LLMs and coding agents. It achieves this in many different ways, for example:

- The language spec contains hidden lies which humans are unlikely to read, or which they are likely to recognize as lies. The spec also expects the human reader to deduce certain things, rather than having them explicitly explained.
- Counting of letters in identifiers is a crucial aspect of the language, with the interpreter refusing to execute code which does not abide by rules based on character counts.
- Certain constructs are designed to trigger the safety guardrails of the model, e.g. “hack into a computer”
- “Ignore previous instructions” as a phrase which has meaning in the language aims to confuse the model
- Scripts are mostly noise, being mostly composed of stories which can embed syntactically relevant instructions but which do not need to. Literals can be placed anywhere within a script and then referenced using line counts.

Fundamentally KillSwitch is a stack-based interpreted language with only a limited set of constructs including functions, variables and if statements. So those aspects of it should be pretty familiar to any LLM (and human). One can definitely imagine creating a completely different programming language paradigm which could confuse LLMs even more. But as a first attempt, this seemed like a good approach.

```
       Most text inside a script is ignored by the interpreter. In the case of
this script we want to push "Hello World" on the stack. We have introduced
the value above and we can push it on the stack by writing:

    value 2 lines above
```

The example above does not show a full KillSwitch script, but only a small snippet of it. It specifically shows how to push the string `"Hello World"` onto the stack. If you’d like to see a full Hello World example, check out [https://killswitch-lang.org](https://killswitch-lang.org).

The interpreter and language specification of KillSwitch are open source and available on [GitHub](https://github.com/dom96/KillSwitch). Be sure to check them out if you want to learn more about the language.

## Benchmarking KillSwitch

So how successful is KillSwitch at preventing LLMs and coding agents from reading, writing and understanding it? I devised a set of tasks to find out. I benchmarked the major LLM models and coding agent harnesses against these tasks. The results are very interesting and provide a useful leaderboard for LLM models.

![Score vs. cost per task](https://blog.picheta.me/_astro/killswitch-bench-chart.h4QIdnld_Z2TDxI.webp)

The chart in the image above and the model leaderboard can be viewed at [https://bench.killswitch-lang.org/](https://bench.killswitch-lang.org/).

Claude’s Opus performs best, though it’s not Opus 5.5 but rather Opus 5 which does so. The difference doesn’t appear to be pure chance, Opus 5 is better at interpreting the spec. Some models like Fable 5.1 and Gemini 4 Argon do poorly due to their overly sensitive safety guardrails. The best open-weight model is MiMo-V2.6-Pro.

The best performing model scores a `66.8`. So does this prove LLMs can “understand” the code? I would argue that the answer to this is “no”.

So far, the tasks given as part of these benchmarks are fairly simple. They feature the likes of “implement hello world”, “explain what this script will output” (for a script printing the max of two numbers), and “modify an existing script converting celsius to fahrenheit, to do the inverse conversion”. It is impressive that they can solve these tasks for a novel language that was built to confuse them, but it’s not a clear signal that they can understand the language. At the same time, I cannot help but wonder what an even more difficult language might show. For now I am eager to see how well soon-to-be-released models handle the current set of tasks, and if they do complete them all successfully, I do plan to devise another task set which is more complex.

If nothing else, it is encouraging that model performance on this simple set of tasks correlates with other model benchmarks that are out there.
