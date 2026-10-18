---
title: Microsoft-Decision-1, our model for fast decision-making
link: https://commandline.microsoft.com/microsoft-decision-1-model-foundry/
source: hnrss-org
published: 2026-10-09T18:38:44Z
updated: 2026-10-09T18:38:44Z
first_seen: 2026-10-09T23:10:41.116203336Z
authors:
- lisajaloza
content: extracted
html: 2026-10-09-microsoft-decision-1-our-model-for-fast-decision-making.html
extra:
  comments_url: https://news.ycombinator.com/item?id=50024913
  num_comments: 39
  points: 112
---

Decision models are quickly emerging as an important new category in AI. Unlike LLMs, which are designed to generate text or reason through complex problems, decision models are purpose-built to deliver structured outputs that software can immediately act on. And once you understand that capability—making decisions and classifying things at very low cost with high performance—all kinds of useful tasks get unlocked.

Today we’re introducing **Microsoft-Decision-1**, our new model for fast decision-scoring, available in [Microsoft Foundry](https://ai.azure.com/catalog/models/Microsoft-Decision-1) and through [OpenRouter](https://openrouter.ai/microsoft/microsoft-decision-1). This model is designed for routing, classification, prioritization, verification, and workflow control, making it easier to incorporate decision intelligence into existing applications, agents, and workflows in a secure, trusted environment. Microsoft-Decision-1 delivers top performance in latency and quality on structured decision tasks to outperform both LLMs and other decision models.

Microsoft-Decision-1 achieved the highest accuracy in our 36-benchmark comparison, spanning nearly 150,000 questions across benchmarks kept blind from training. And in our benchmarking, it was the fastest measured: 2.5 times quicker than H2O-Lightning-4B v1.1, the runner-up, and 35 times quicker than GPT-6 Sol.

*Editor’s note: This post was updated from the original to add benchmarks for Jev on accuracy and calibration.*

## How Microsoft-Decision-1 works

To build Microsoft-Decision-1, we post trained Qwen3.5-9B for fast, single-pass decision scoring and will soon rebase it on other models, including Microsoft AI (MAI) and OpenAI. When given a fixed set of answer options, Microsoft-Decision-1 provides a calibrated probability score for each option. The model supports yes/no, multiple-choice, and rating options, as well as rubric-based grading of AI responses and agent actions, all through a simple structured API call.

To build a reliable decision model, we had to address several challenges:

### 1\. Speed

Each decision adds delay, especially when one step depends on another. For example, adding just 100 milliseconds to each of 20 sequential decisions adds two seconds to the overall workflow.

Microsoft-Decision-1 P50 latency is ~35x faster than GPT-6 Sol.

### 2\. Quality that generalizes

It’s easy to overfit a model for one benchmark or one type of decision task. We need to know whether that quality carries over to various tasks the model wasn’t trained on. That’s why we evaluated Microsoft-Decision-1 across dozens of benchmarks kept blinded from training, spanning routing, ranking, long context, multilingual and out-of-distribution tasks, reasoning, and safety. We also took several of the top public models on the [popular open leaderboard JevBench](https://benchmarkheaven.com/jev-models) and tested them across 36 additional public and private benchmarks. Microsoft-Decision-1 performed the best across these broader sets of benchmarks, demonstrating strong generalization.

### 3\. Robustness

Equivalent inputs should produce equivalent decisions. In production, states and instructions get paraphrased, option descriptions change, choices are reordered, keys change, and harmless formatting noise appears. None of those changes should materially alter the decision.

We perturb the same request in eight ways and measure how often the decision flips. Microsoft-Decision-1 changes its decision on 1.3% of perturbations on average with zero flips when option descriptions are paraphrased or when options are reversed or shuffled.

### 4\. Probability and confidence calibration

The probability itself is part of the API, not just a ranking score. Applications use confidence to decide when to act, defer, or ask for review, so a 90% prediction should be right about nine times out of 10 on representative cases.

### 5\. Safety

A decision model should recognize harmful requests without needlessly blocking harmless ones. We tested Microsoft-Decision-1 on 5,250 requests across 11 benchmarks, covering harmful content, jailbreak attempts, and prompt injection, and found that the model successfully refused harmful behavior while retaining a high degree of utility.

## Demo examples

Classification is a key use case for decision models. Check out how accurately and quickly Microsoft-Decision-1 can categorize a variety of queries compared to GPT-6 Sol:

Decision models can also be efficient for computer use scenarios. This demo shows how fast Microsoft-Decision-1 can complete the task of buying a backpack compared to GPT-6 Sol:

## How we’re testing Microsoft-Decision-1 internally

Here are some of the ways we’ve been testing Microsoft-Decision-1 internally, with a lot more to come.

### Labeling data

XBOX Research used Microsoft-Decision-1 to process more than 10,000 open-ended pieces of feedback and reviews from surveys, STEAM, and Twitter/X and sort them into a fixed set of themes established by researchers to understand what people are saying about different games, launches, streams, and more. They found Microsoft-Decision-1 to be competitive on quality with GPT-6 Sol while running over 14 times faster and 200 times less expensive.

### Quality control

The Copilot team measures the quality of chat and agentic responses. Their testing found Microsoft-Decision-1 to be competitive with GPT5.6 Luna and 100 times faster.

### Incident response

Our on-call engineers use AI to retrieve relevant knowledge to respond to live incidents across logs, ticketing systems, calls, messages, and other data sources. Microsoft-Decision-1 performed better and faster than an LLM for knowledge retrieval.

### Scientific discovery

Microsoft Discovery implements an adaptive replanning feature where an agent evaluates a previous experiment, revises its approach based on rubric grades, and repeats until it has completed its objectives. Microsoft-Decision-1 scored as 46 times more consistent than the LLM-based score at three times the speed and resulted in nearly four times the speed on adaptive replanning

Faster, more reliable planning could significantly impact outcomes for long-running scientific experiments.

Those are just a small handful of examples. There are many more potential use cases for Microsoft-Decision-1. Consider trying out the following:

- **Agent controls:** Evaluate an agent’s proposed next step and decide whether to continue, stop, retry, or hand off to a model, tool, or human.
- **Model routing:** Evaluate an incoming request and select the best model for the task based on quality, cost, and latency requirements.
- **Skill-based decisions:** Apply the rules from an agent skill to select the appropriate next action without repeatedly processing a long list of instructions.
- **Data labeling:** Assign consistent labels to social media posts, customer feedback, and other data for analysis or training.
- **AI judging:** Evaluate an AI-generated response against defined quality criteria and decide whether to accept, revise, or reject it.
- **Intent analysis:** Identify what a user is trying to accomplish and match their request to a supported intent.
- **Incident response routing:** Classify an incident by type and urgency, then route it to the appropriate team or workflow.
- **Data validation:** Check whether an input meets defined requirements and decide whether to accept it, reject it, or flag it for review.
- **Recommendations:** Select the most relevant item, offer, or next action from a set of candidates.
- **Search relevance:** Assess how well a search result matches a query and assign a relevance label or priority.
- **Content classification and filtering:** Categorize content by topic or policy and decide whether to display it, filter it, or send it for review.
- **Code scanning:** Evaluate code against defined criteria and flag potential defects or policy violations for review.
- **Safety and security screening:** Classify requests, outputs, or proposed actions by risk and decide whether to allow, block, or escalate them.
- **Computer and UI use:** Select the next interface action from a set of options based on the current screen and task.
- **Robotics:** Select among predefined robot actions based on observations, task goals, and operating constraints.
- **Scientific discovery:** Screen candidate hypotheses, compounds, or experiments against defined criteria and prioritize them for further evaluation.

## Getting started

Developers can get started with Microsoft-Decision-1 today in Microsoft Foundry here: [https://ai.azure.com/catalog/models/Microsoft-Decision-1](https://ai.azure.com/catalog/models/Microsoft-Decision-1). You can also get started through OpenRouter here: [https://openrouter.ai/microsoft/microsoft-decision-1](https://openrouter.ai/microsoft/microsoft-decision-1).

Documentation is available here: [https://learn.microsoft.com/en-us/azure/foundry/foundry-models/how-to/use-foundry-models-microsoft-decision](https://learn.microsoft.com/en-us/azure/foundry/foundry-models/how-to/use-foundry-models-microsoft-decision).

### Pricing

Input tokens cost $0.042 USD per million tokens. Output tokens are free.

## Looking ahead

Now that agentic AI is a reality, we’ve seen that cost plays a major role in how people decide to use AI. And it’s increasingly important to choose the right model for the right job. With agents taking action and making an impact in the real world, decision models have the potential to help people guide and control those agents through complex environments.

We look forward to seeing what developers build with Microsoft-Decision-1 and hearing their feedback. We’ll continue to release updates to the model, including by incorporating evaluations and data to further optimize quality, confidence, and cost.

### Appendix: Models benchmarked

Quyet-1.0-Large — [https://huggingface.co/chinhnc/Quyet-1.0-Large](https://huggingface.co/chinhnc/Quyet-1.0-Large) \
Surogate Rune 26B-A4B — [https://huggingface.co/surogate/rune-26b-a4b-GGUF](https://huggingface.co/surogate/rune-26b-a4b-GGUF) \
GPT-6 Luna Decisions — [https://developers.openai.com/api/docs/guides/decisions](https://developers.openai.com/api/docs/guides/decisions) \
deck-31B — [https://github.com/krishna-gogineni-765/deck31b](https://github.com/krishna-gogineni-765/deck31b) \
H2O-Lightning-4B — [https://huggingface.co/h2oai/h2o-lightning-4b](https://huggingface.co/h2oai/h2o-lightning-4b) \
Strands-Decider 2B — [https://github.com/strands-labs/strands-decider](https://github.com/strands-labs/strands-decider)
