---
title: Model Evaluation
link: https://ampcode.com/news/model-evaluation
source: ampcode-com
published: 2025-08-12T00:00:00Z
updated: 2025-08-12T00:00:00Z
first_seen: 2026-10-04T12:50:30.804479567Z
summary: 'A lot of models have been released in the last few weeks: Kimi K2, Qwen3-Coder, GLM-4.5, gpt-oss, Claude Opus 4.1, GPT-5, diffusion models, and there''s no end in sight. We''ve been trying a lot of them, to see whether they can make Amp better. In Amp, a new model isn''t just another entry in a model selection dropdown menu. It''s part of a whole in which many different models have different jobs to do and for each job we want to use the best model, regardless of cost or deployment concerns. So when a new model comes along, we ask: Is it a frontier model that can replace Claude Sonnet 4 fully or even partially as the main agent model? Is it a good subagent model that spikes in certain areas? (Like how the oracle uses a different model, originally o3.) Is it a fast utility model that we can use, for example, for search, summarization, or other similar tasks? With this post, we want to show you a week in the life of the Amp team as we evaluate new models. Impressions, ideas, tips — we''ll share what we discover. We''ll continuously update it with notes from Thorsten, Camden, Quinn, Beyang, and other Amp team members. GPT-5 One Week Later (latest note) GPT-5 is pretty good after a few days of use GPT-5, Two First Impressions Elbow Grease Try GPT-5 Try Claude Opus 4.1 Good Forward-Looking Evals Last updated: LAST-UPDATED(2025-08-15T16:02:00Z) GPT-5 One Week Later From Camden Cheek, Fri Aug 15, LAST-UPDATED(2025-08-15T16:02:00Z) After a week, I''m cooling on the idea of GPT-5 as primary agentic driver in Amp. There''s a lot to like about GPT-5: Price is very competitive. However, that''s somewhat balanced by the fact that it takes more tokens to complete a task on average. Behavior is extremely steerable. It follows instructions well, almost to a fault sometimes. I''ve had to soften the language in my prompting to avoid it overindexing on small style notes. Its default straightforward, terse, and direct style is a breath of fresh air compared to Sonnet''s effusiveness. Reasoning-heavy tasks like debugging or code review are really quite good. The rate of false positive "I found the problem!" responses is low, and feedback it gives is often actually useful. However, I still find myself switching back to Sonnet 4 when I just want to get stuff done. For one, GPT-5 feels slow. Even at low reasoning effort, the time spent thinking adds up and is enough that I get distracted from my task. This gets much worse at higher reasoning levels. But on top of that, GPT-5 really struggles at deciding when it''s fetched enough context. It often seems to get stuck in "research loops" where it keeps reading files until it is absolutely sure it has all the information it needs. Tool use has some rough edges as well. Tool selection isn''t as good as I''d hoped, and I''ve often had to spell out what tools to use in the system prompt for common workflows rather than relying on the model to pick the most appropriate strategy. It also has a habit of generating invalid JSON for tool args, causing gibberish to show up in tool calls or getting into generation loops that consume the entire output token limit. This looks really broken when it happens, and it gets expensive too. Finally, GPT-5 is not nearly as persistent as Sonnet in Amp. As much as I prompt it to keep going until it solves the user''s request, it still has a tendency to just give up, kicking control back to the user or asking for input/permission to continue. This is frustrating when trying to use it for longer-running tasks, and it means I feel like I have to babysit it while it''s working. GPT-5 is pretty good after a few days of use From Beyang, Mon Aug 11, LAST-UPDATED(2025-08-11T17:50:04Z) After using GPT-5 more frequently in Amp for a few days, it''s all-in-all a very solid model. Here are the most salient warts: It likes using a lot of tools. This can make things take longer due to excessive tool calls. It prefers to use natural language search queries to search subagent, which makes the search subagent take longer (though this is way better than it was initially as we''ve adjusted our model scaffolding). It doesn''t automatically emit pretty markdown, which can make responses a little harder to read and scan. It seems to prefer to edit files by rewriting the whole file. This can result in irrelevant--but sometimes substantive--changes to other parts of the file. The biggest plus is that it seems very instructable. So far, it''s decent at the whole general coding agent thing, but much more instructable and responsive to specific direction. GPT-5, Two First Impressions From Thorsten Ball, Fri Aug 8, LAST-UPDATED(2025-08-08T16:30:00Z) This morning I took GPT-5 out for a proper spin, not just testing it, but actually putting it to use, trying to fix a bug in the Amp CLI. This time I did something I usually don''t do: I used voice dictation and ended up with a long, rambly prompt that contained a lot of redundant information. To my (and everyone''s who was in hearing distance here in the office) surprise, GPT-5 fixed the bug in a single turn. Flawlessly. I committed the code just as GPT-5 wrote it. And this was a gnarly bug, something that requires knowing how our data structures work, how they''re used in the CLI, and what can lead to flicker. GPT-5 fixed it and it even fixed a something I didn''t even think of. I''m still holding out on any verdicts (maybe using voice dictation and the prompt did more than the difference between Sonnet 4 and GPT-5?), but this was impressive. Elbow Grease From Camden Cheek, Fri Aug 8, LAST-UPDATED(2025-08-08T16:02:00Z) GPT-5 seems to keep up with Sonnet pretty well for coding stuff. But it''s gonna take a lot of work to make it lovable. It still does dumb stuff, it still gives weird responses, it has weird communication patterns. All that distracts from how good it is from coding, which is quite good. More elbow grease needed, you don''t just drop in a model. Try GPT-5 From Beyang, Thur Aug 7, LAST-UPDATED(2025-08-07T18:00:00Z) You can try GPT-5 in Amp now. In the CLI, run amp --try-gpt5 In the editor extension, set the following setting: "amp.gpt5": true, (Update: These options to try GPT-5 as the main model have been removed. Amp now uses GPT-5 as the oracle.) See the official announcement for more details. We have been impressed with GPT-5''s capabilities. It excels at tool use and code generation at noticeably fast inference speeds. It is also quite steerable. Within Amp, it generally offers concise and direct technical explanations. It is great at multi-step reasoning and writing code across long agentic chains. We have tuned Amp''s tool set and feedback loops to work well when this model is enabled. We expect to discover more over time as Amp users engage with GPT-5 and will continue to invest in improvements with user and customer feedback over the coming days and weeks. Try Claude Opus 4.1 From Quinn, Tue Aug 5, LAST-UPDATED(2025-08-05T21:05:00Z) You can try Claude Opus 4.1 in Amp now with the following hidden setting in your editor or CLI: "amp.experimental.agentMode": "opus4.1", Opus is more expensive, and we pass the cost through. We still recommend, and I personally still prefer, Claude Sonnet 4 (the default in Amp). Good Forward-Looking Evals From Beyang, Wed Aug 6, LAST-UPDATED(2025-08-06T02:01:00Z) Some thoughts on evals: Everyone wants to treat evals as some magical oracle for what "good" is, because this seems scientific. but really this is scientism, because it''s impossible to fully capture product experience in evals and whatever evals you do construct are backward looking. Ergo, evals are primarily useful as unit tests and regression tests, to ensure a new model doesn''t regress some existing behavior you''d like to preserve. Things that make good forward-looking evals: Actually using it and posting public threads that seem particularly awesome (very qualitative). The Simon Willison "pelican riding a bicycle" test that is eye-catchingly simple to grok and verify (but with all the usual caveats that this doesn''t necessarily generalize)...and even that eval I suspect is quite gameable. Over time, I suspect that certain natural strengths for each model will emerge and become widely accepted. We figure out what these strengths and then have the product lean into these so that we guide the user into the "sweet spot" of each model.'
content: extracted
html: 2025-08-12-model-evaluation.html
---

A lot of models have been released in the last few weeks: Kimi K2, Qwen3-Coder, GLM-4.5, gpt-oss, Claude Opus 4.1, GPT-5, diffusion models, and there's no end in sight.

We've been trying a lot of them, to see whether they can make Amp better.

In Amp, a new model isn't just another entry in a model selection dropdown menu. It's part of a whole in which many different models have different jobs to do and for each job we want to use the best model, regardless of cost or deployment concerns. So when a new model comes along, we ask:

- Is it a frontier model that can replace Claude Sonnet 4 fully or even partially as the main agent model?
- Is it a good [subagent](https://ampcode.com/manual#subagents) model that spikes in certain areas? (Like how [the oracle](https://ampcode.com/manual#oracle) uses a different model, originally o3.)
- Is it a fast utility model that we can use, for example, for search, summarization, or other similar tasks?

With this post, we want to show you a week in the life of the Amp team as we evaluate new models. Impressions, ideas, tips — we'll share what we discover.

We'll continuously update it with notes from [Thorsten](https://x.com/thorstenball), [Camden](https://github.com/camdencheek), [Quinn](https://x.com/sqs), [Beyang](https://x.com/beyang), and other Amp team members.

- [GPT-5 One Week Later](https://ampcode.com/news/model-evaluation#gpt5-one-week-later) *(latest note)*
- [GPT-5 is pretty good after a few days of use](https://ampcode.com/news/model-evaluation#gpt5-pretty-good)
- [GPT-5, Two First Impressions](https://ampcode.com/news/model-evaluation#gpt5-two-first-impressions)
- [Elbow Grease](https://ampcode.com/news/model-evaluation#gpt5-elbow-grease)
- [Try GPT-5](https://ampcode.com/news/model-evaluation#gpt5)
- [Try Claude Opus 4.1](https://ampcode.com/news/model-evaluation#opus)
- [Good Forward-Looking Evals](https://ampcode.com/news/model-evaluation#forward-looking-evals)

*Last updated: 1y ago*

### [GPT-5 One Week Later](https://ampcode.com/news/model-evaluation#gpt5-one-week-later)

*From [Camden Cheek](https://x.com/camden_cheek), Fri Aug 15, 1y ago*

After a week, I'm cooling on the idea of GPT-5 as primary agentic driver in Amp.

There's a lot to like about GPT-5:

- Price is very competitive. However, that's somewhat balanced by the fact that it takes more tokens to complete a task on average.
- Behavior is extremely steerable. It follows instructions well, almost to a fault sometimes. I've had to soften the language in my prompting to avoid it overindexing on small style notes.
- Its default straightforward, terse, and direct style is a breath of fresh air compared to Sonnet's effusiveness.
- Reasoning-heavy tasks like debugging or code review are really quite good. The rate of false positive "I found the problem!" responses is low, and feedback it gives is often actually useful.

However, I still find myself switching back to Sonnet 4 when I just want to get stuff done.

For one, GPT-5 feels slow. Even at low reasoning effort, the time spent thinking adds up and is enough that I get distracted from my task. This gets much worse at higher reasoning levels. But on top of that, GPT-5 really struggles at deciding when it's fetched enough context. It often seems to get stuck in "research loops" where it keeps reading files until it is absolutely sure it has all the information it needs.

Tool use has some rough edges as well. Tool selection isn't as good as I'd hoped, and I've often had to spell out what tools to use in the system prompt for common workflows rather than relying on the model to pick the most appropriate strategy. It also has a habit of generating invalid JSON for tool args, causing gibberish to show up in tool calls or getting into generation loops that consume the entire output token limit. This looks *really* broken when it happens, and it gets expensive too.

Finally, GPT-5 is not nearly as persistent as Sonnet in Amp. As much as I prompt it to keep going until it solves the user's request, it still has a tendency to just *give up*, kicking control back to the user or asking for input/permission to continue. This is frustrating when trying to use it for longer-running tasks, and it means I feel like I have to babysit it while it's working.

### [GPT-5 is pretty good after a few days of use](https://ampcode.com/news/model-evaluation#gpt5-pretty-good)

*From [Beyang](https://x.com/beyang), Mon Aug 11, 1y ago*

After using GPT-5 more frequently in Amp for a few days, it's all-in-all a very solid model. Here are the most salient warts:

- It likes using a lot of tools. This can make things take longer due to excessive tool calls.
- It prefers to use natural language search queries to search subagent, which makes the search subagent take longer (though this is way better than it was initially as we've adjusted our model scaffolding).
- It doesn't automatically emit pretty markdown, which can make responses a little harder to read and scan.
- It seems to prefer to edit files by rewriting the whole file. This can result in irrelevant--but sometimes substantive--changes to other parts of the file.

The biggest plus is that **it seems very instructable**. So far, it's decent at the whole general coding agent thing, but much more instructable and responsive to specific direction.

### [GPT-5, Two First Impressions](https://ampcode.com/news/model-evaluation#gpt5-two-first-impressions)

*From [Thorsten Ball](https://x.com/thorstenball), Fri Aug 8, 1y ago*

This morning I took GPT-5 out for a proper spin, not just testing it, but actually putting it to use, trying to fix a bug in the Amp CLI. This time I did something I usually don't do: I used voice dictation and ended up with a long, rambly prompt that contained a lot of redundant information.

To my (and everyone's who was in hearing distance here in the office) surprise, GPT-5 fixed the bug in a single turn. Flawlessly. I committed the code just as GPT-5 wrote it.

And this was a gnarly bug, something that requires knowing how our data structures work, how they're used in the CLI, and what can lead to flicker. GPT-5 fixed it *and* it even fixed a something I didn't even think of.

I'm still holding out on any verdicts (maybe using voice dictation and the prompt did more than the difference between Sonnet 4 and GPT-5?), but this was impressive.

### [Elbow Grease](https://ampcode.com/news/model-evaluation#gpt5-elbow-grease)

*From [Camden Cheek](https://x.com/camden_cheek), Fri Aug 8, 1y ago*

GPT-5 seems to keep up with Sonnet pretty well for coding stuff. But it's gonna take a lot of work to make it lovable. It still does dumb stuff, it still gives weird responses, it has weird communication patterns. All that distracts from how good it is from coding, which is quite good. More elbow grease needed, you don't just drop in a model.

### [Try GPT-5](https://ampcode.com/news/model-evaluation#gpt5)

*From [Beyang](https://x.com/beyang), Thur Aug 7, 1y ago*

You can try GPT-5 in Amp now.

In the CLI, run

```
amp --try-gpt5
```

In the editor extension, set the following setting:

```
"amp.gpt5": true,
```

*(Update: These options to try GPT-5 as the main model have been removed. Amp now uses [GPT-5 as the oracle](https://ampcode.com/news/gpt-5-oracle).)*

See the [official announcement](https://ampcode.com/news/gpt-5) for more details.

We have been impressed with GPT-5's capabilities. It excels at tool use and code generation at noticeably fast inference speeds. It is also quite steerable. Within Amp, it generally offers concise and direct technical explanations. It is great at multi-step reasoning and writing code across long agentic chains. We have tuned Amp's tool set and feedback loops to work well when this model is enabled. We expect to discover more over time as Amp users engage with GPT-5 and will continue to invest in improvements with user and customer feedback over the coming days and weeks.

### [Try Claude Opus 4.1](https://ampcode.com/news/model-evaluation#opus)

*From [Quinn](https://x.com/sqs), Tue Aug 5, 1y ago*

You can try [Claude Opus 4.1](https://www.anthropic.com/news/claude-opus-4-1) in Amp now with the following hidden [setting](https://ampcode.com/manual#configuration) in your editor or CLI:

```json
"amp.experimental.agentMode": "opus4.1",
```

Opus is more expensive, and we pass the cost through. We still recommend, and I personally still prefer, Claude Sonnet 4 (the default in Amp).

### [Good Forward-Looking Evals](https://ampcode.com/news/model-evaluation#forward-looking-evals)

*From [Beyang](https://x.com/beyang), Wed Aug 6, 1y ago*

Some thoughts on evals:

- Everyone wants to treat evals as some magical oracle for what "good" is, because this seems scientific. but really this is scientism, because it's impossible to fully capture product experience in evals and whatever evals you do construct are backward looking.
- Ergo, evals are primarily useful as unit tests and regression tests, to ensure a new model doesn't regress some existing behavior you'd like to preserve.
- Things that make good forward-looking evals:
  - Actually using it and posting [public threads](https://ampcode.com/manual#thread-sharing) that seem particularly awesome (very qualitative).
  - The [Simon Willison](https://simonwillison.net/) "pelican riding a bicycle" test that is eye-catchingly simple to grok and verify (but with all the usual caveats that this doesn't necessarily generalize)...and even that eval I suspect is quite gameable.

Over time, I suspect that certain natural strengths for each model will emerge and become widely accepted. We figure out what these strengths and then have the product lean into these so that we guide the user into the "sweet spot" of each model.
