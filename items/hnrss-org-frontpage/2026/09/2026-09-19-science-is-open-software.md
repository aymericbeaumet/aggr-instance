---
title: Science Is Open Software
link: https://jepedersen.dk/blog/202505_research/
source: hnrss-org-frontpage
published: 2026-09-19T02:21:35Z
updated: 2026-09-19T02:21:35Z
first_seen: 2026-09-19T13:22:28.237536491Z
authors:
- jegp
summary: 'Article URL: https://jepedersen.dk/blog/202505_research/ Comments URL: https://news.ycombinator.com/item?id=49762687 Points: 109 # Comments: 41'
content: extracted
html: 2026-09-19-science-is-open-software.html
---

**TL;DR** I claim that modern science is synonymous with open source software. This post explains why, why it matters, and what you can (and should) do next.

* * *

> Why do you care about (open source) software? - Everyone

I spend a *lot* of my time working on software. I have been asked why software matters more times than I can remember. Software is, people say, not science. It’s a time sink, something to rush past in the pursuit of what really matters: results (and papers if you’re in academia). Publish or perish.

Well. I think software matters. In fact, I think open source software *is* science. Or, at least computational science. And this post tells you why. Why we as scientists must insist on the scientific method and why that means working on open and reproducible software. This post is not easy to write. It challenges many of the current trends in academia, but it is an important move *towards better science* that doesn’t turn us all *[insane](https://en.wikipedia.org/wiki/Insanity#Insult)*.

## What is science?

If you look up science on Wikipedia, here’s what hits you:

> Science is a systematic discipline that builds and organises knowledge in the form of testable hypotheses and predictions about the universe. - [Wikipedia](https://en.wikipedia.org/wiki/Science)

Now, [go and grab a random arXiv paper](https://jepedersen.dk/arxiv.html). It clearly contains “knowledge” of some sort. But, does the paper contribute predictions that are testable and can by systematically organized? **Can you test it?** **Can you systematize it?**

The answer is never a flat no, but it’s hard. You rarely have direct access to that knowledge.

### The good explanation - inner models

> If the organism carries a 'small-scale model' of external reality and of its own possible actions within its head, it is able to try out various alternatives, ... and in every way to react in a much fuller, safer, and more competent manner to the emergencies which face it.

In his *excellent* book [The Nature of Explanation](https://en.wikipedia.org/wiki/Kenneth_Craik#CITEREFCraik1943), [Kenneth James Williams Craik](https://en.wikipedia.org/wiki/Kenneth_Craik) posits that we use small simulations of reality to explain and predict the world outside.

This point seems obvious today, but it highlights the goal of pursuing science in the first place: you, as an acting entity, improves your **inner model** to the point that you can make *better* predictions than before. The **inner model** here is critical: if the arXiv paper does not help their readers predict the world, it is not science. This is why computational reproducibility matters–software is how we encode and share predictive models.

## What is reproducibility?

Recall that according to Wikipedia, it is *not* enough to demonstrate results alone. Results have to be (1) systematic and they have to be (2) testable.

It is entirely possible that the given paper is too hard to understand or unaccessible to the audience for other reasons. That does not mean that there are no scientific insights to find—readers may find ways to systematize them on their second or third reading. No, it means that *you specifically* cannot take the idea as your own, test it, and use it to improve your world model.

Reproducibility, in this context, is not only the duplication of results. It is the ability to take the scientific idea, embed it into your own inner model, adapt it, and build upon it—or discard it because it reduces predictabilitly.

If an idea is not reproducible, the findings cannot be expanded. And are, therefore, useless.

This becomes clear if we do a quick thought-experiment where we replace “software model” with “mathematical model”. Just as we wouldn’t accept a physics paper that said our equations predict X but we won’t show the math, we shouldn’t accept (computational) science that hides its methods.

> How many fields have been held back, and how many people have had their careers disrupted, because of a buggy program? - [Greg Wilson](https://www.nature.com/articles/467775a)

[Software is ubiquitous in modern science](https://www.nature.com/articles/s43588-024-00651-2). Anything from CoVid models to search algorithms to lab protocols are build on software built by other people. Researchers are busy people. They don’t bother to look through all software dependencies to verify correctness, understand implementation details, or check for potential errors that could invalidate results.

From that follows that the **scientific results depend on the software**. If the software is wrong, the science is wrong. (Software bugs already cause numerous retractions, such as [here](http://blogs.sciencemag.org/pipeline/archives/2007/02/27/wrong_but_still_convincing), [here](https://retractionwatch.com/2022/03/03/nasa-researchers-retract-nature-paper-on-climate-change-and-evapotranspiration/), [here](https://retractionwatch.com/2016/05/19/software-glitch-not-intentional-manipulation-sunk-immunology-paper/), and [several places here](https://www.science.org/content/article/how-avoid-stigma-retracted-paper-dont-call-it-retraction)).

And that is well and good, because at some point we *have* to trust and rely on other’s work. For that to happen, it (software) needs to be reliable.

## Why open source?

We found that software needs to be

1. **Reproducible**, meaning executable, as well as modifiable, and
2. **Reliable**, meaning that the results are consistently trustworthy

Modifiability is important for science for the same reason that equations are important for scientific predictions. Reliability is crucial because we want *systematic* improvement of our knowledge, not flaky and partial results that only work occasionally.

This is what open source software gives us. We can change code and retrofit it to suit our needs (just think about [Hugging Face models](https://huggingface.co/)) and we can iterate upon it to continue to improve it. It already *[generates trillions in value](https://www.hbs.edu/faculty/Pages/item.aspx?num=65230)* and there is room for much, much more.

Of course, open source software is not a perfect cure. There are IP and security concerns, bugs can still occur, and stability can be a problem. But at least the imperfections are on *public record*. They can be amended and improved, just like our scientific understanding. From that perspective, one can claim that open source software *is* the scientific method—just in simulation.

## A vision for future science

If we accept these premises we can ask: what would truly open (computational) science look like?

**Every result is instantly reproducible.** When you read a paper claiming that a new drug reduces symptoms by 30%, you click a link and watch the exact analysis run in your browser. The data processing, statistical tests, and visualizations execute in seconds using the same environment the authors used—preserved perfectly through reproducible containers.

**Scientific software evolves like Wikipedia.** Climate models aren’t developed in isolation by single labs, but maintained by global communities. When a researcher in Kenya discovers a bug in atmospheric turbulence calculations, the fix propagates instantly to climate simulations worldwide. Models improve continuously rather than languishing in academic silos.

**The pace of discovery accelerates.** Instead of each researcher building from scratch, we stand on shoulders of giants whose work is not just readable, but runnable and modifiable. Scientific progress compounds at an unprecedented rate.

**Trust in science strengthens.** When climate models, economic forecasts, and medical recommendations are built on transparent, auditable code, public confidence grows. Science communication improves because the models themselves become part of the conversation—not just their conclusions.

This isn’t utopian fantasy. Every piece already exists—open source communities, reproducible environments, collaborative development platforms. We just need to shape them into a coherent vision for how science should work in the digital age.

The question isn’t whether this future is possible. The question is: how quickly can we build it?

## What now?

I posit that open source software is a necessary condition if we are [to science](https://en.wiktionary.org/wiki/science#Verb) in a computerized world. Software is executable mathematical models that we should prioritize much higher.

We still have work to do and this is how you can help:

- **Share and document your code**
  - Papers without code is less scientific because it is harder to build on the insights. In the ideal world any claim should be backed up by *reproducible* code. Always use code from day 1 and always share it.
- **Write stable code, use [NixOS](https://nixos.org/)**
  - Code should be reliable and work in perpetuity. That means making sure dependencies and environments are kept constant. The best way to do that is to use *reproducible environments*. NixOS is quickly becomming the biggest and best tool there is. It will guarantee that your code will run *exactly the same way*, even 100 years in the future. Docker, Conda, and similar tools are better, but NixOS gives more comprehensive guarantees.
- **Build on existing tools instead of creating your own**
  - For the common scientific knowledgebase to improve, we need cross-platform tools. This is particularly true for small fields such as neuromorphics, where [a recent Nature paper pointed out that open source software is key to scaling](https://www.nature.com/articles/s41586-024-08253-8). Go check out the [Open Neuromorphic software guide](https://open-neuromorphic.org/neuromorphic-computing/software/) and see if you can’t find libraries close to your work.
- **Promote academics that work on software**
  - Given the huge importance of code, Academic promotions should value software contributions

The scientific revolution succeeded because it insisted on transparency, reproducibility, and constant scrutiny. The open source movement embodies these same principles for software, but there is much more work to be done.

Will you help make software scientific?
