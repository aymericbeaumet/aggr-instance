---
title: Amp Is Now Available To Everyone — Here's How I Use It
link: https://ampcode.com/how-i-use-amp
source: ampcode-com
published: 2025-05-15T00:00:00Z
updated: 2025-05-15T00:00:00Z
first_seen: 2026-10-04T12:50:30.804479567Z
summary: As of today, Amp is available to everyone. No more waitlist. Thorsten used the occasion of Amp now being available to everyone to write down how he uses it and how it changed programming for him.
content: extracted
html: 2025-05-15-amp-is-now-available-to-everyone-here-s-how-i-use-it.html
---

Thorsten Ball//May 15, 2025

Here is how I use it.

As of today, Amp, the frontier agent, is available to everyone. The waitlist is gone — go, go and sign up and use it!

For the past 10 weeks, Amp has been the main tool with which I develop software and — putting yet another exclamation mark behind the fact that the tools we use end up changing us — it has fundamentally changed how I develop software.

So I thought I’d use the occasion to write up how I personally use Amp.

## In VS Code

You can use Amp in Visual Studio Code as an extension or as a CLI. I mainly use it in VS Code, constantly open on the right sidebar, constantly mashing ⌘I and ⌘L.

If you’d told me four months ago that I would use VS Code as my main editor, I wouldn’t have believed you. But here we are. The only explanation I have is this: which text editor I use just doesn’t feel that important to me anymore. I don’t have exact numbers, but my guess is that Amp now writes 70-80% of the code I commit.

That’s right — I barely write code by hand anymore. If I type code, it counts.

## Paint-by-numbers programming

I think of coding with agents as paint-by-numbers programming: I put in the numbers and the lines and the agent then goes and puts in the colors.

The agent doesn’t make architectural decisions for me, it doesn’t write critical code without close supervision, it doesn’t introduce a completely new structure to the codebase. That’s what I do. But once I know what that should look like, I put everything I know — architecture, possible edge cases, constraints, which tests to add and extend and run — into a prompt and send the agent on its way.

## [Small Threads](https://ampcode.com/how-i-use-amp#small-threads)

In general — and I’ll give you some exceptions soon — I’ll try to keep the threads, the conversations I have with the model, small.

Right now, the model we use under the hood is Claude Sonnet 4. With Claude 3.7 Sonnet (which we used previously), and I expect for Claude Sonnet 4 as well, after the context window reaches 100k tokens, things start to feel blurry, imprecise. When the context window gets too large, Claude starts to forget instructions from the first prompt, or it goes into what the scientists call a “doom loop” — that’s when it tries to fix and fix and fix the same test over and over again, without much success.

Sometimes I [only ask it to remove debug statements from staged code](https://ampcode.com/threads/T-66beb0de-7f02-4241-a25e-50c0dc811788). Other times I want it to implement a small-ish feature that I know won’t touch more than a handful of files, such as [adding an RSS feed to this website](https://ampcode.com/threads/T-4e6a3890-fc01-416b-acc9-40fa6da60028). Or I want it to [simplify the design of a single UI component](https://ampcode.com/threads/T-c8299300-faeb-4de2-95ef-86ef3275d846).

Again, *generally speaking*, I think a lot of the problems that people who are new to working with agents run into can be traced back to them not starting new threads often enough.

## Add new features

That’s probably the classic use case: ask the agent to implement a new feature, or change an existing one.

I’ve been asked the following multiple times in the past few weeks: now, with agents, do you think we won’t have to learn anything about software anymore?

And I say, no, *no*, when I write a prompt for the agent, *everything* I know about developing software comes together: what I think about the architecture, where I suspect the traps are, where I know the relevant code to be, how I think the refactoring should go, what a good test would be, what a necessary trade-off is, how to present it to the user.

I don’t write prompts like this:

```text
Build a batch tool into the agent
```

But instead something like this:

```text
I need your help implementing this: https://raw.githubusercontent.com/anthropics/anthropic-cookbook/refs/heads/main/tool_use/parallel_tools_claude_3_7_sonnet.ipynb

We have tools defined in core/src/tools/tools.ts (and the other files) and now I want to implement this batch tool. I imagine that it'll be a bit tricky to figure out the types, so I want you to deeply think about that and find a practical solution that doesn't lead to a lot of complicated types. We should start really simple and then evolve what we have.
```

Or something like this:

```text
A user ran into this bug that you see in the screenshot. The problem seems to be in core/src/threads/thread-worker.ts and core/src/inference/backends/anthropic.ts

It looks like we need to make sure that we don't send thinking blocks along when we error.

I know that we already have some logic for thinking blocks, but I want you to analyze how we handle thinking blocks so far.
```

And then I follow up with:

```text
What's our current logic for removing thinking blocks? Don't we remove them somewhere in core/src/threads/thread-worker.ts or core/src/threads/thread-delta.ts ?
```

And then, finally, I say:

```text
Okay, make that change!
```

## Let it screenshot!

This is, by far, one of my favorite things when working with an agent: giving them visual feedback about their work. And, guess what, they can make screenshots all on their own, when you just give them a URL to open.

In our Amp codebase, we have a storybook for UI components. I always have it running at `https://localhost:7002/storybook` and when I open that URL I can see most of our components neatly lined up, presented in the different states they can be in.

So if I want the agent to change one of the UI components, I tell it to go look at the storybook — which it can once you add the Playwright MCP server in the settings — and check its work by taking screenshots.

Let me give you an example. A few days ago I wanted the agent to update an existing story in our storybook. This was my prompt:

![Prompt to check storybook](https://ampcode.com/how-i-use-amp/screenshot_1.png)

After submitting, the agent went off and changed the storybook to include the changes. Here’s how it confirmed that the changes it made worked:

![Agent using screenshots](https://ampcode.com/how-i-use-amp/screenshot_2.png)

“The file changes are now showing up correctly!” it exclaims after taking a screenshot — what a time to be alive, right?

But it gets better. I think the screenshots-as-feedback work especially well when the changes the agent made did *not* work. Because on the screenshot, or in the browser’s console, it then sees errors and tries again until it works. Try it, it’s magical.

## Run the build and fix the errors

Sometimes my prompt is as simple as this:

```text
Run the build and fix all the errors
```

Since what “the build” is, is documented in the [`AGENTS.md`](https://ampcode.com/docs/customize/agents-md), the agent will just run the command and then fix the errors.

## Review code

Very often I ask the agent to do the following:

```text
Run `git diff` to see the code someone else wrote. Review it thoroughly and give me a report
```

Of course that “someone else” is no other than — drumroll — the agent itself! But it doesn’t know that, does it? So it goes and runs `git diff` and says that the code looks good, or clean, or that it has some bugs. If so, I ask it to fix one of the bugs after considering whether the analysis is correct.

## Clean up code

Say I had the agent write hundreds of lines of code and in the middle of it I asked it to add some debug statements so that we can figure out why it doesn’t quite work the way we want it to.

I often don’t know where it added those debug statements and I don’t have any of the files open. So once I know the feature works and I’m ready to commit, I open a new thread and ask another incantation of the agent to remove the debug statements:

```text
Run `git diff` to see what has been changed then remove the debug statements
```

## Paste screenshots

I do love screenshots, and so does Amp:

![Screenshot of a conversation into which I pasted a screenshot](https://ampcode.com/how-i-use-amp/paste_screenshot_1.png)

That’s right: you can paste screenshots using ⌘-v/ctrl-v and the agent will then “read” them.

I use this constantly and have pasted screenshots of Slack messages in which someone reported a bug, error messages that I can’t neatly copy & paste (or that I’m too lazy to copy), bugs in the UI.

It works best with text, because these models are really good at “reading” text in screenshots, but I’ve also previously asked the agent to “flip the two buttons that are marked in this screenshot” and it figured it out.

In any case: it’s a lot of fun.

## Explain code with diagrams

Amp can sketch diagrams in replies. That’s very handy when you want to figure out how some code works.

I mean, come on, look at [this](https://ampcode.com/threads/T-bf8de4e5-e03f-43aa-850e-2071d93feda6):

![Diagram the agent generated](https://ampcode.com/how-i-use-amp/diagram.png)

All it took was this prompt:

```text
Walk me through the code in this branch (compared to `main`) and explain to me how the autocompletions are hooked up into vscode and basically walk me through the lifecycle of an autocompletion via the code
```

Not the most elegant phrasing, is it? And I didn’t even say it should create a diagram, but in this case it magically did. (In other cases I specifically tell it to use a diagram when I know that’s what I want.)

## Read commits

A single git commit contains an incredible amount of meta information: who made the change, how they described the change, what files were changed together, paths of the files, partly what’s in the files.

Fred Brooks wrote in The Mythical Man Month:

> Show me your flowchart and conceal your tables, and I shall continue to be mystified. Show me your tables, and I won’t usually need your flowchart; it’ll be obvious.

Now I’ll say this: show me a commit and I shall know enough to build something similar.

I often use the fact that raw commits contain so much information to inject relevant context into a prompt by simply asking the agent to look at a specific commit before doing anything.

Here’s an example:

```text
This test server/src/lib/components/diffs/diff-file-tree.test.ts has recently been broken by this commit: 3ec95344d5d5a55ab2342d5daa53f3c3155391dd

Run

    pnpm -C server test --run diff-file-tree

to see the test fail.

Then examine the commit.

Then tell us how to fix the test.
```

Or I ask the agent to find a commit for me and then read it:

```text
Look at the git history of core/src/tools/builtin/filesystem/edit_file.common.ts

At some point I removed the vscode implementation in that file.

Find that vscode implementation and explain to me how we did file reloading after edit in it.
```

After that, we had all the relevant context needed to make changes to the implementation.

## Search code

Sometimes I just want to find out where some code lives in our codebase and not have the agent change anything.

In that case, I just tell it to:

```text
Find the code that ensures unauthenticated users can view the /notes/how-to-build-an-agent page too
```

Or, here, from a few weeks ago:

```text
Where in the codebase do we define the database default so that new users have 0 invites? I think it's a database migration or something. You need to look in `server`
```

I know I could come up with some keywords to search for, but the agent’s usually faster. And if I then want to change something, guess what? Everything’s in context already.

## Share threads with colleagues

It is *very* handy that you can share threads with colleagues or the public.

For knowledge sharing, of course, or to explain how you & your agent built something, but also to, you know, brag when the agent got it right on first try. I mean, [look at this](https://ampcode.com/threads/T-4e6a3890-fc01-416b-acc9-40fa6da60028):

![Screenshot of thread in which it implemented an RSS feed](https://ampcode.com/how-i-use-amp/share_threads_1.png)

## Tell it what I want

Very often when people struggle with the agent, the problem can be boiled down to this:

> I thought the agent would do this, but it didn’t, why?

Well, impressive or not, agents aren’t all-knowing. They might know a thing or two about the world, but they don’t know what you want if you didn’t tell them.

So instead of thinking “I wish the agent would’ve just used command `super-build --dry-run`, but it didn’t”, just tell it to!

Instead of a vague

```text
Can you figure out who wrote this component?
```

Tell it what you want it to do:

```text
Use git blame to tell me who wrote this component
```

## Build one to throw one away

With agents, it’s now much more feasible (or at least: less of a pain) to build one to throw one away. There’s no longer the feeling of sunken cost telling you “but it’s actually not that bad, is it? should we really throw it all away?”

Instead, you can tell the agent to implement it, wait five minutes, look at the code, and then decide to keep it or throw it away.

I’ve done this many times. Very often the most important thing I learned is how I do *not* want to build the feature. Or I learned that I didn’t know what I wanted at all.

## Use the git staging area

I don’t think that in the previous 10 years I’ve used the git staging area as much as in the last 10 weeks. Turns out it’s very handy. Who would’ve thought?

But why?

Instead of having checkpoints and an apply/reject model of interacting with the agent (which [we don’t](https://ampcode.com/fif#restore-checkpoints) believe is still the right approach) we just let the agent do its thing. The safety net is always version control — and that’s a safety net we never want to mess with.

That in turn means you can leverage git as much as possible: ask the agent to do something, see that it’s good, stage it, then ask for something else, see that it isn’t good, and wipe the unstaged changes.

## Write SQL

Having an agent that connects to your database is very, very close to the joy you feel when it takes screenshots and iterates on UI components.

Here’s how to do it.

First, tell the agent to use `psql` (or any other CLI utility) or the tools provided by the [`postgres`](https://github.com/modelcontextprotocol/servers/tree/main/src/postgres) MCP server (or any other MCP server for your database) to connect to your DB.

Then, ask it things like this:

```text
Update my user account (email starts with thorsten) to have unlimited invites
```

Or:

```text
Return me a list of users with the most number of threads, sorted by number of threads
```

The agent will then do everything it can to return you that list: figure out the schema of the database, try this query, try that query.

Look, [here](https://ampcode.com/threads/T-f810ef79-ba0e-4338-87c6-dbbb9085400a) I wanted to change my local development database. It didn’t know what the schema is, so it tried to figure that out first, which it did — by running four commands in parallel:

![Screenshot of thread in which agent uses psql](https://ampcode.com/how-i-use-amp/sql.png)

It’s glorious.

## It’s a different, still strange way to write code

Let the agent do this, let the agent do that, screenshots here, screenshots there, barely any typing — yes, it does sound weird, doesn’t it?

As someone who wore t-shirts with text editor logos on them, let me be the one to say: it feels strange to program like this.

Quinn and I have talked about this feeling in nearly every episode of the [podcast](https://ampcode.com/podcast). We have both been programming for a long time and now we’re relearning how to do it through an agent and it just feels strange.

Because it is *strange*. It’s new. It takes some getting used to. You need to learn how to do it. My bet is that for at least the next six to twelve months, you’ll still have to learn how to prompt well.

But here’s another bet: once you’ve seen the agent do things you haven’t thought were possible yet, that’s when it shifts from “I’m not sure I like this…” to “okay, wow, what else can I do with this, I bet it can also—”

That’s when it shifts from strange to exhilarating.

That’s when you realize that when programming with agents, to [quote Mary Rose Cook here](https://maryrosecook.com/blog/post/become-an-ai-augmented-engineer), “each move is bigger than a step. You lift off the ground. It requires more forethought, but, because you make more progress with each move, it feels like flying.”
