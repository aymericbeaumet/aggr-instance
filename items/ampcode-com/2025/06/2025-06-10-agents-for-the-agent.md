---
title: Agents for the Agent
link: https://ampcode.com/agents-for-the-agent
source: ampcode-com
published: 2025-06-10T00:00:00Z
updated: 2025-06-10T00:00:00Z
first_seen: 2026-10-04T12:50:30.804479567Z
summary: Amp now has subagents. Thorsten wrote a post about subagents, what they even are, and how they make us wonder whether everything is changing again.
content: extracted
html: 2025-06-10-agents-for-the-agent.html
---

Thorsten Ball//June 10, 2025

Everything is changing... again?

Amp now has subagents. Here’s what they look like in action:

![Agent spawning multiple subagents](https://ampcode.com/agents-for-the-agent/subagents.png)

But chances are that the first sentence and even that screenshot didn’t cause your facial expression to change.

After all: who even knows what a subagent is? So let’s start there.

What’s a subagent? If you’ve read [How to Build an Agent](https://ampcode.com/notes/how-to-build-an-agent) you know that we define an agent as “an LLM with access to tools, giving it the ability to modify something outside the context window.”

Subagents are tools, too. They are agents that can be started by the main agent, the one you’re interacting with when you use Amp.

Just like the main agent can invoke tools to edit files or run terminal commands, it can now also decide to spawn another agent when it thinks (“thinks” in airquotes) that’ll help. When it does, it passes a prompt to the spawning subagent and tells it what it should do. Just like we do, when we spawn an agent.

## It ain’t new

Okay, that first sentence wasn’t entirely truthful. You might even say it was a lie, because subagents aren’t new to Amp. We’ve had one from the start and you very likely have seen it being invoked too: the search agent.

It’s an agent that has access to read-only tools and that’s prompted to search your codebase. If the main agent has to answer a question such as “where in the codebase is the auth logic?”, it can spawn the search agent and ask it to “Find out where the auth logic is”.

The search agent will then go, search, find the auth logic, and return what it found back up to the main agent.

Like this:

![Agent spawning a search agent subagents](https://ampcode.com/agents-for-the-agent/search-agent.png)

Over the past few months, we’ve also experimented with other types of subagents. Each type would have a different system prompt or a different set of tools, but they never turned out to be useful. Either their job was already covered by the search agent and it wasn’t even clear to us humans when to use one subagent over the other, or, as was often the case, the model behind the main agent — Claude 3.7 Sonnet, at the time — simply didn’t invoke them and instead did the work on its own.

Then Claude Sonnet 4 came along.

## A model that yearns for subagents

Once we had access to Claude Sonnet 4, we quickly added subagents back to Amp to see whether things were different with this new model and… yes indeed, things are different.

Turns out that Sonnet 4 really likes delegating work to subagents and invokes them whenever it can spot clearly defined tasks.

Following our mantra of giving the models what they need, we barely had a choice: Sonnet 4 wants subagents and subagents it shall have.

So we added subagents to Amp. This time they were *generic*; mini-Amps if you will; subagents that can basically do anything the main agent can do. That makes them more powerful than any other subagent we’ve had before. Instead of only being able to read the codebase, these new subagents now have access to tools to *write* to it and run terminal commands too.

They are now available in Amp and over the last few weeks, while working with them, we couldn’t help but wonder: wait, is everything changing… again?

## Everything is changing… again?

It’s a question you have to ask yourself when [Geoff Huntley writes](https://x.com/geoffreyhuntley/status/1931199073316688224) the following:

> One of the things that was holding me back from doing it was the context window management, because it required a high level of skill. But because of sub-agents, that skill level has now been commoditised and made easy for anyone to do, which means classes of inception are now possible.

What he’s referring to are two magical properties that make subagents different from any other tool in the agent’s toolbox.

For one: they are agents. They can make their own decisions (again: airquotes) and autonomously attempt to solve problems and accomplish tasks. Instead of, say, using a terminal-command tool to “run a command”, with a subagent you can tell it to “run this command 5 times and report back to me how the output changed with each invocation”.

That’s already very fascinating (I mean: who doesn’t like recursion?), but what’s even more interesting is the second, less obvious property of subagents: they each have their own context window.

That’s what Geoff is referring to in his post and what also makes us scratch our head and wonder if things will change.

With a single agent, you previously had to pay attention to how many tokens are used in its context window.

There are only so many tokens to go around and if you send the agent off on longer tasks and it runs into a compiler error that requires a few attempts to fix, those tokens are gone — spent on fixing that single error — and not available for the longer task anymore.

That’s different now.

With subagents, instead of spending its own tokens, the agent can now spawn a subagent to fix the error. The subagent in turn will have a completely fresh context window and once it’s done fixing the error, no matter how many attempts it took, only a tiny fraction of the main agents tokens (just enough to spawn the subagent and send a prompt along) have been used.

I see your facial expression change, your eyebrows going up, your chin dropping, because you’re about to let out a I-get-it-now-”ohhh”, but wait, it gets better: the main agent can spawn *multiple subagents*, in parallel!

That’s right. Instead of having the main agent come up with a plan or a list of tasks or TODOs and working them off, one by one, and possibly falling into a token-sucking rabbit hole along the way, the main agent can now come up with a plan, but then delegate the work to subagents.

What does that mean for how we interact with agents? Is the meta of having plan files outdated? Is careful conversation management a thing of the past if now a single conversation with one main agent can potentially contain tens and tens of other conversations with subagents? Can the agent now go on for much longer, solving more complex tasks, because it doesn’t have to concern itself with the details of editing single files anymore?

We don’t know and I bet no one knows. It’s time to explore and find out.

The best way to do that is to be very explicit about it and tell the model to “use a subagent for this task” or “use a subagent implementing this in each of these files”. We purposefully haven’t cranked up the subagent-nudging in the system prompt, because we still need to figure out how to best integrate them into workflows.

But what I already know is this: it’s an exciting time.
