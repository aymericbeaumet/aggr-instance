---
title: GPT-6.2 Astra is the first model to beat Googlers' code golf records
link: https://bjin.github.io/go-codegolf-benchmark/#leaderboard
source: hnrss-org
published: 2026-11-22T16:44:48Z
updated: 2026-11-22T16:44:48Z
first_seen: 2026-11-26T02:47:10.395377406Z
authors:
- bjin
summary: 'Back in 2015, when I was still at Google, there was a small group of code-golf enthusiasts spending hours trying to save just one byte on their solutions for weekly code-golf problems. No matter how hard you tried, there always seemed to be another Googler who found a shorter solution. And once a problem entered the challenge phase and every solution became public, it was always astonishing to read how brilliant other people''s solutions were. 11 years later, LLMs are evolving fast. A recent OpenAI post ("Perplexity and Intelligence", https://openai.com/index/perplexity-and-intelligence/) described researchers watching a model solve open math problems one after another in a Slack thread. I started to wonder: how would frontier models compete with the records that a small group of brilliant minds created with hours of effort each week? So I built this benchmark. Frontier models do find a lot of new approaches (and very brilliant ones) to solve these problems. For example, in two problems ("Surviving Queens" and "The Rolling Dice"), frontier models use complex numbers to solve them in Python and Ruby (both languages have built-in complex number support). And for the "(-5)-Smooth Numbers" problem, where you have to write code to list the first n positive integers of the form (-2)^x⋅(-3)^y⋅(-5)^z, most solutions build them with nested loops or by growing a set, then sort. GPT-6.2 Astra gets them all from powers of 4, and earns a sole best with this brilliant solution: g=lambda n:sorted(4**i%(2**999-5)%(2**49-3)for i in range(7000))[:n] There is a dedicated section for this: https://bjin.github.io/go-codegolf-benchmark/#why/finding-a-... It''s nice to see GPT-6.2 Astra become the first frontier model to beat the human records in code golfing. Along with other frontier models, it finds a lot of interesting solutions that Googlers back in 2015 didn''t. On the other hand, I also believe code golfing can be a good benchmark (not a comprehensive one, of course) for frontier models. Code golfing rewards thinking differently, because a shorter solution usually comes from looking at the problem from a different angle. It also rewards running experiments, not just thinking: writing a search program to find a shorter expression, or exploring lesser-known features and functions in the target compiler, interpreter, or library. Human code golfers do this a lot, and no public training data tells a frontier model how to do so. Comments URL: https://news.ycombinator.com/item?id=50601280 Points: 258 # Comments: 76'
content: feed
html: 2026-11-22-gpt-6-2-astra-is-the-first-model-to-beat-googlers-code-golf.html
---

Back in 2015, when I was still at Google, there was a small group of code-golf enthusiasts spending hours trying to save just one byte on their solutions for weekly code-golf problems. No matter how hard you tried, there always seemed to be another Googler who found a shorter solution. And once a problem entered the challenge phase and every solution became public, it was always astonishing to read how brilliant other people's solutions were.

11 years later, LLMs are evolving fast. A recent OpenAI post ("Perplexity and Intelligence", [https://openai.com/index/perplexity-and-intelligence/](https://openai.com/index/perplexity-and-intelligence/)) described researchers watching a model solve open math problems one after another in a Slack thread. I started to wonder: how would frontier models compete with the records that a small group of brilliant minds created with hours of effort each week?

So I built this benchmark. Frontier models do find a lot of new approaches (and very brilliant ones) to solve these problems. For example, in two problems ("Surviving Queens" and "The Rolling Dice"), frontier models use complex numbers to solve them in Python and Ruby (both languages have built-in complex number support). And for the "(-5)-Smooth Numbers" problem, where you have to write code to list the first n positive integers of the form (-2)^x⋅(-3)^y⋅(-5)^z, most solutions build them with nested loops or by growing a set, then sort. GPT-6.2 Astra gets them all from powers of 4, and earns a sole best with this brilliant solution:

```
    g=lambda n:sorted(4**i%(2**999-5)%(2**49-3)for i in range(7000))[:n]
```

There is a dedicated section for this: [https://bjin.github.io/go-codegolf-benchmark/#why/finding-a-...](https://bjin.github.io/go-codegolf-benchmark/#why/finding-a-different-approach)

It's nice to see GPT-6.2 Astra become the first frontier model to beat the human records in code golfing. Along with other frontier models, it finds a lot of interesting solutions that Googlers back in 2015 didn't. On the other hand, I also believe code golfing can be a good benchmark (not a comprehensive one, of course) for frontier models.

Code golfing rewards thinking differently, because a shorter solution usually comes from looking at the problem from a different angle. It also rewards running experiments, not just thinking: writing a search program to find a shorter expression, or exploring lesser-known features and functions in the target compiler, interpreter, or library. Human code golfers do this a lot, and no public training data tells a frontier model how to do so.

* * *

Comments URL: [https://news.ycombinator.com/item?id=50601280](https://news.ycombinator.com/item?id=50601280)

Points: 258

\# Comments: 76
