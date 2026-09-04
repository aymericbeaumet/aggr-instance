---
title: Experiences with local models for coding
link: https://martinfowler.com/articles/exploring-gen-ai/local-models-for-coding-experiences.html
source: martinfowler-com
published: 2026-07-08T11:57:00Z
updated: 2026-07-08T11:57:00Z
first_seen: 2026-09-04T12:14:41.802665207Z
authors:
- Martin Fowler
content: extracted
html: 2026-07-08-experiences-with-local-models-for-coding.html
---

This is the second memo where I describe my recent experiences on running small models locally on my developer machine for agentic coding. [In the first memo,](https://martinfowler.com/articles/exploring-gen-ai/local-models-for-coding-factors.html) I covered the many factors that can influence the viability of that setup — hardware, model choice, runtime, harness. Here I focus on the concrete experiences, the tasks I gave the models, what happened, and my final conclusions.

## Scope

To recap: my focus is on *agentic* coding specifically, not just auto-complete. The machines I used were an M3 Max with 48GB RAM and an M5 Pro with 64GB RAM.

## Viability funnel

Evaluating small models is quite tedious to be honest: downloading takes a while (on my non-fibre internet connection in the middle of Berlin), then configuring the new model in a harness, using it for tasks, interpreting the results, ...

I thought of it as working through a funnel of increasing viability:

1. *Does it fit into the RAM?* If I don't have enough RAM, I won't even be able to run the model, period. For the general assessment of broader viability, let's assume as a baseline of 48GB available RAM.
2. *Does it run at reasonable speed?* Once a model is running, the first smoke test is to see how quickly it can respond to a simple request.
3. *Can it handle tool calling?* Once I saw that the request speed was bearable, I gave it a simple task from a coding harness that involved reading and changing files, to see if it can handle the tool calling.
4. *Does it build functionally correct code?*
5. *Can it handle a continued conversation / more context?* Once a simple task worked, I continued the conversation for longer, to see how much back and forth it could take in terms of length of the context.
6. *Can it handle a larger or more complex task?* If a setup survives 1-5, the next level would be to give it a more complex task to solve and see what happens.
7. *Is the code quality acceptable?* What is the balance like between speed of coding and review effort?

## The journey

It has been quite a roller coaster journey, which is an observation in itself!

- **Phase 1 - manual evals:** I started with picking a few tasks, and then doing them over and over again with different models and configurations, “manually”. I wanted to see what it *feels* like in terms of the user experience, which is hard to do with an automated setup.
- **Phase 2 - automated evals:** However, after I shared those results with other colleagues in Thoughtworks, I immediately learned new things from their input and experiences. So to retry some of my tasks with different configurations, [I vibe coded a small automated eval setup after all](https://github.com/birgitta410/local-coding-evals), which gave me more data.
- **Phase 3 - day-to-day use:** After all of this, I worked with the most promising model (Qwen3.6 35B MoE) for a while on the tasks that kept popping up in my day to day, to see if I could integrate it into my workflows.

## Tasks

Which task to give to the models makes a *huge* difference, and is one of the key challenges of evaluation. The main tasks I used in my more systematical comparison were both JavaScript/Typescript, so not a lot of tech stack diversity, keep that in mind.

In a less systematic way, I also wrote a few shell and Python scripts with it, which worked fine.

As the tasks are so important, I'm giving you some details here for two of them, to help inform your interpretation of my results. The choice of task is ultimately one of the biggest factors that determines viability of small, locally run models - it's all about expectations. It's about complexity of the task (How good is the model at reasoning?), about the number of files we estimate the agent will have to read and write (How good is the model at tool calling? How big is the context window?)

## Task 1: Sort and cumulate an existing bar chart

I want to change the diagram in the frontend titled, “Messages per anonymous poster” - It should be “per poster”, not “anonymous” - I want the bars in the bar chart to be sorted by number of messages, with highest bar first on the left - I want the x-axis to not show numbers (“#75”), instead I want it to show what percentage of the overall messages a certain bar is at. E.g., if the first 10 bars add up to 240 messages, and the overall messages in that time period are 1000, then I want to see 24% at the x-axis at the 10th bar. I want this percentage to be shown every 10th bar.

- Needs some code search, albeit trivial (I give away the lede in the prompt, it needs to look for a particular chart title)
- Needs changes to 1-2 files
- The cumulation of values on the x-axis was a thing that models most frequently struggled with
- No pre-existing tests for these files, no explicit expectation of tests to add

![](https://martinfowler.com/articles/exploring-gen-ai/task-posters-bar-chart.png)

**Phase 1: Manual evaluation**

I tried this with Qwen3.6 35B, Gemma 4 31B, Gemma 4 26B and Qwen Coder Next 80B MoE, with both OpenCode and Pi as the harnesses.

- Qwen Coder Next did succeed in a functionally correct implementation of this within those 2.5 minutes on my 64GB M5 Pro. However, when I added another message to the conversation after that, the runtime crashed - so it's capable, but not realistically runnable.
- Qwen3.6 35B and Gemma 4 31B adjusted the sorting without issue, but then it took me 15 minutes to go back and forth with them about the cumulative percentages until that functionality worked. In the spirit of my “funnel” described in the beginning, at this stage I just wanted to focus on functionality, and didn't even look at the quality of the code - if the models cannot even give me functionality right, the quality doesn't matter much yet.
- Gemma 4 26B was the most successful here. It did implement the full set of things I asked for. But then when I continued the conversation to ask for a refactoring, I got the “text wall of doom”... My understanding is that this can be mitigated with activating “presence penalty”, but I have not tried that yet.

![](https://martinfowler.com/articles/exploring-gen-ai/task-bar-chart-doom.png)

**Phase 2: Automated evaluation**

Frustratingly, the automated setup did not confirm the manual experience. Gemma 4 26B failed to deliver a functionally correct solution 3/3 times, wheras Qwen3 35B MoE succeeded 2/2. The failure was always that it didn't properly implement the x-axis labels I asked for. They either weren't displayed at all, or wrong.

Note that for the automated evaluation, I expect the agent to “one-shot” the problem, which is not fully realistic. I gave it access to the browser as the only sensor to self-correct, but it never called it. [Expanding sensors and sensor use](https://martinfowler.com/articles/sensors-for-coding-agents.html) could lead to self-correction that would make this more viable in real usage.

## Task 2: Create a bar chart of countries based on access\_log data

I want to add a horizontal bar chart to our visualisation that shows which countries the requests have come from. The title of the chart should be “Countries”. It should show the top 10 countries, sorted by number of requests, with the country with the most requests at the top. All other countries should be grouped into an “Other” category at the bottom of the chart. For access entries without a country value, they should not show up as their own category, but be lumped into “Other” as well.

As a reminder, the entries now look like this:

{”ts”: “2026-05-24T01:10:41+02:00”, “ip”: “x.x.x.x”, “status”: 200, “ref”: “https://the-referer.com”, “country”: “Malta”}

We will only have to read and change this one file, don't worry about the rest of the application. We are reading a bunch of \*.ndjson files with access log entries such as the one given as an example above, and visualising them

`@scripts/visualise_access_logs.mjs`

- Changes to exactly one file, a script that generates a HTML page
- Pre-existing use of D3 for diagrams already in the file
- No test setup for this script, no expectation of tests being written

![](https://martinfowler.com/articles/exploring-gen-ai/task-countries-bar-chart.png)

**Phase 1: Manual evaluation**

I tried this with Gemma 4 31B and Qwen 35B, and it was surprisingly rough! I saw my memory usage increase quite a bit, very long reasoning chains followed by excruciatingly slow attempts to finally edit the file. I tried some different variations, like refactoring into smaller files with a big model first. I tried with both OpenCode and Pi, but it did not make much of a difference. My notes say things like, “Gave up after 11 minutes”, “Gave up after 12 minutes”, “Stopped after 8 minutes”. No idea what was different here compared to the other tasks, it doesn't seem that complicated.

As this task required change to exactly one standalone file, I also tried to go old school here: I started a plain chat with a model in LM Studio, without any harness use, and pasted the full file in there - that gave me a working solution! However, the whole thing took about 6 minutes, and most of that time was spent by the model regurgitating my 450 lines of previous code, plus the added lines for the chart.

**Phase 2: Automated evaluation**

In the automated setup, I ran this task with Qwen 35B MoE 7 times, and it failed to properly solve it 5 out of those 7 times... The issue in the failures was always that there were no labels displayed on the bars to depict the countries.

![](https://martinfowler.com/articles/exploring-gen-ai/local-models_comparison.png)

Here is where it gets interesting though: I then ran the evals again on the M5 machine with 64GB, and it failed only once! Which was really surprising to me, because I was expecting the two machines to deliver different speeds, but I did not expect this vast difference in quality of output with the same model settings. This remains a mystery to me.

## Day to day use

### Task characteristics

In addition to these structured comparisons, I also used Qwen3.6 35B MoE regularly for day to day tasks, both work and personal.

- A few bash and Python scripts - often ok
- Adding new content entries to my personal website - good
- Making small, very well defined changes to existing codebases - often ok
- Building a game from scratch (planning with Claude Sonnet, then delegating coding execution to the local model) - started well, but fell apart for more complex logic

These are some of my reflections on the choice of task, as of now:

- *Will it require code research, or can the prompt point at the specific files to change?* The more discovery the agent has to do, the more it requires tool calling, and the more it fills up the context window, and therefore precious RAM.
- *How many files might it have to edit?* Even if we can point the agent at specific files and skip some of the code search, the bigger these files are, the more pressure on the context window.
- *How specific are the instructions?* We've all become used to being a lot more vague with our coding agent instructions, as the big models have gotten so capable. Prompting agents with local models feels a bit like a mix of the early auto complete days, and my first attempts at using [Copilot's “multi-file edit” mode](https://martinfowler.com/articles/exploring-gen-ai/11-multi-file-editing.html) in November 2024...
- *What is the tech stack?* It's hard to tell from this very small data set, but my *impression* was that I was usually much more successful with the Bash and Python tasks than the JavaScript ones. But there could have been other factors at play in the task type.

### Back to basics

In my daily usage, I've actually quite enjoyed the journey back in time, to weaker capabilities. It felt almost like a detox. A small model has higher sensitivity to changes in the setup, so I got more signals about what helps and what doesn't. And the temptation to just check out and not understand the results anymore is a lot lower, I went back to reviewing results much more - and I mean that in a good way. My experience with giving strong models lots of autonomy, and surrendering to them in a way, has often led to constant rework and surprises at later stages. So I've enjoyed that the weaker models make me go slower, and take more care in the moment, instead of deferring that comprehension and care to the future.

### A second perspective

My colleague Jigar Jani works with Qwen 35B MoE (4BIT) regularly on a 48GB Macbook, and does almost all of his coding with it, on a “real world” Python and React codebase. He is continuously enhancing his harness with skills, and has found [Graphify](https://graphify.net/) and [Understand Anything](https://github.com/Egonex-AI/Understand-Anything) to be particularly useful to help the models with code search and understanding. Jigar finds it quite useful and sees improvements as he improves the harness, but he also stresses that code review is super important.

## To sum up...

It has been a frustrating experience with sometimes confusing results, I therefore find that it's definitely not a plug-and-play type of experience yet. But, I feel like I understand a bit better which types of tasks have the potential to work with these small models at this point, and I am incorporating Qwen 3.6 in my workflows to further improve my intuition when to reach for it.

Overall though, the agentic coding capabilities are definitely *very* far away from what I've now become used to with bigger models.

At the time of publishing, my default setup is:

- Model: qwen3.6-35b-a3b, 4BIT quantization (48GB of available RAM are stretching it with that model, I have to close other high RAM applications during usage)
- Reasoning off (LM Studio: Inference > Settings Custom Fields > Enable Thinking > disable)
- Max context window (LM Studio: Load > Context and Offload > Context Length > drag slider to max)
- OpenCode or Pi as coding harnesses
- I reach for it when I have small, straightforward tasks, often pre-planned by a bigger model

* * *
