---
title: Bye Bye, Dial
link: https://ampcode.com/news/bye-bye-dial
source: ampcode-com
published: 2026-11-12T00:00:00Z
updated: 2026-11-12T00:00:00Z
first_seen: 2026-11-12T18:49:11.379952736Z
summary: 'Today we''re saying goodbye to the Dial and its four modes: low, medium, high, and ultra. A model selector takes its place: You can now pick the model and its effort for each thread. And you can now also switch both in the middle of a thread. The model selector now also shows you which of your subscriptions or API keys will be billed for each model, or whether it uses Amp credits. If you don''t want to pick the model yourself, use the Staff Pick, which is the model we would choose. Yes, We Changed Our Mind In January we explained why Amp does not have a model selector. In July we then shipped the Dial so you could focus on how hard a task is instead of thinking about model names and version numbers. And it worked: when we changed the default model behind the Dial, nobody complained. But then we added the ability to Amp to bring your own subscriptions and keys, and Amp became free when you do. That changed what the selected model does. Not only is it what produces the tokens, but it also started to decide who gets billed. medium on the Dial could use your Claude plan, or your ChatGPT plan, or Amp credits, and it became very hard to tell who''s paying for which model when. And that''s not to speak of all the settings we had to add to the Dial to make it work in every combination. So we decided to stop hiding the model and now have a model selector. Now, when you pick it yourself, you know where the tokens come from and who''s paying. And yet, it''s still not just a model you pick. An agent is more than a model. You pick the model, and we build the harness around it: each model in the selector gets a system prompt and tools optimized for it, and both switch when you switch models in a thread. Switch Models in a Thread With the Dial, a thread kept the mode (and model) it started with. We did that because a model became confused when it continued a conversation that another model had started. But now models are good enough to recover from that. Now you can change the model mid-thread by opening the selector and making the change. After your next message the new model will be used and the transcript will show a record: "Switched to GPT-6.1 Sol · High". But some things haven''t changed. A model switch still has a price. Prompt caches belong to one model, so the new model reads the whole thread uncached, and the first message after a switch costs more. Threads that run an agent from a plugin keep that agent. Staff Pick New threads start on Staff Pick by default. (Except if you had previously configured the Dial to have a default mode.) Staff Pick is the model we would pick ourselves, which is Claude Opus 5.5 today. We keep benchmarking models, and we''ll change the pick when another model does better. In Settings → Model Selector, under New Threads Start On, you can also start new threads on the model you picked last, or on one fixed model you choose. What Happens to Your Dial Until today everyone used the Dial, so this applies to you. New threads start on Staff Pick, whichever mode you used last. Existing threads keep working. A thread you started in high continues on the model it ran. Dial settings don''t carry over. If you changed the model behind a mode in Tune Modes, pick that model in the selector now. If you put your own agents on the Dial in Build Dial, you find them under Other Models. If you never changed those settings, these are the models your modes ran. The first three are at the top of the selector, and Claude Fable 5.2 is under Other Models: low: GLM-5.3-Flash medium: Claude Opus 5.5 high: GPT-6 Astra ultra: Claude Fable 5.2 Pin your models in Settings → Model Selector, or leave the selector on Staff Pick. Read the Model Selector docs for everything the selector and its settings do.'
content: extracted
html: 2026-11-12-bye-bye-dial.html
remote_preview:
  url: https://ampcode.com/og?_template=news&_version=7&title=Bye+Bye%2C+Dial&date=November+12%2C+2026&tagline=We+said+Amp+does+not+have+a+model+selector.+Now+it+has+one%2C+and+the+Dial+is+gone.&backgroundImage=https%3A%2F%2Fstatic.ampcode.com%2Fnews%2Fbye-bye-dial-cover-wide.avif&sig=78f4e3e8a79c6c9f41b75c4b1a8453f4f19ce7fdeb2402801e2daf01cfec4c0f
---

Today we're saying goodbye to [the Dial](https://ampcode.com/news/the-dial) and its four modes: `low`, `medium`, `high`, and `ultra`.

A model selector takes its place:

![The model selector open below the prompt box. Opus 5.5 is at the top with a Staff Pick tag, then GPT-6 Astra, GLM-5.3-Flash, and GPT-6.1 Sol. An icon on each row shows who bills you: Claude, ChatGPT, or Amp. A card next to the list describes Claude Opus 5.5 and says it is served by your personal Claude subscription.](https://static.ampcode.com/news/bye-bye-dial-selector-light.avif) ![The model selector open below the prompt box. Opus 5.5 is at the top with a Staff Pick tag, then GPT-6 Astra, GLM-5.3-Flash, and GPT-6.1 Sol. An icon on each row shows who bills you: Claude, ChatGPT, or Amp. A card next to the list describes Claude Opus 5.5 and says it is served by your personal Claude subscription.](https://static.ampcode.com/news/bye-bye-dial-selector-dark.avif)

You can now pick the model and its effort for each thread. And you can now also switch both in the middle of a thread.

The model selector now also shows you which of your subscriptions or API keys will be billed for each model, or whether it uses Amp credits.

If you don't want to pick the model yourself, use the Staff Pick, which is the model we would choose.

## Yes, We Changed Our Mind

In January we explained why [Amp does not have a model selector](https://ampcode.com/model-selector). In July we then [shipped the Dial](https://ampcode.com/news/the-dial) so you could focus on how hard a task is instead of thinking about model names and version numbers. And it worked: when we changed the default model behind the Dial, [nobody complained](https://ampcode.com/news/who-cares-about-the-model).

But then we added the ability to Amp to bring your own subscriptions and keys, and [Amp became free](https://ampcode.com/news/free-agent) when you do. That changed what the selected model does. Not only is it what produces the tokens, but it also started to decide who gets billed. `medium` on the Dial could use your Claude plan, or your ChatGPT plan, or Amp credits, and it became very hard to tell who's paying for which model when.

And that's not to speak of all the settings we had to add to the Dial to make it work in every combination.

So we decided to stop hiding the model and now have a model selector. Now, when you pick it yourself, you know where the tokens come from and who's paying.

And yet, it's still not just a model you pick. An agent is more than a model. You pick the model, and we build the harness around it: each model in the selector gets a system prompt and tools optimized for it, and both switch when you switch models in a thread.

## Switch Models in a Thread

With the Dial, a thread kept the mode (and model) it started with. We did that because a model became confused when it continued a conversation that another model had started. But now models are good enough to recover from that.

Now you can change the model mid-thread by opening the selector and making the change. After your next message the new model will be used and the transcript will show a record: "Switched to GPT-6.1 Sol · High".

But some things haven't changed. A model switch still has a price. Prompt caches belong to one model, so the new model reads the whole thread uncached, and the first message after a switch costs more.

Threads that run an agent from a plugin keep that agent.

## Staff Pick

New threads start on Staff Pick by default. (Except if you had previously configured the Dial to have a default mode.)

Staff Pick is the model we would pick ourselves, which is Claude Opus 5.5 today. We keep benchmarking models, and we'll change the pick when another model does better.

In [Settings → Model Selector](https://ampcode.com/settings/model-selector), under **New Threads Start On**, you can also start new threads on the model you picked last, or on one fixed model you choose.

## What Happens to Your Dial

Until today everyone used the Dial, so this applies to you.

**New threads** start on Staff Pick, whichever mode you used last.

**Existing threads** keep working. A thread you started in `high` continues on the model it ran.

**Dial settings** don't carry over. If you changed the model behind a mode in Tune Modes, pick that model in the selector now. If you put your own agents on the Dial in Build Dial, you find them under **Other Models**.

If you never changed those settings, these are the models your modes ran. The first three are at the top of the selector, and Claude Fable 5.2 is under **Other Models**:

- `low`: GLM-5.3-Flash
- `medium`: Claude Opus 5.5
- `high`: GPT-6 Astra
- `ultra`: Claude Fable 5.2

Pin your models in [Settings → Model Selector](https://ampcode.com/settings/model-selector), or leave the selector on Staff Pick.

Read the [Model Selector docs](https://ampcode.com/docs/model-selector) for everything the selector and its settings do.
