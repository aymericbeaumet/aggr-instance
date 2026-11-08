---
title: Goodbye Claude, Hello Pi
link: https://caleb.software/posts/claude-to-pi.html
source: hnrss-org
published: 2026-11-08T03:11:48Z
updated: 2026-11-08T03:11:48Z
first_seen: 2026-11-08T06:37:14.114093134Z
authors:
- karagenit
content: extracted
html: 2026-11-08-goodbye-claude-hello-pi.html
remote_preview:
  url: https://caleb.software/assets/img/claude-to-pi/vsc-disconnected.png
  alt: claude code in vscode showing disconnected error
---

Goodbye Claude, Hello Pi

21 September 2026 [#pi](https://caleb.software/tag/pi.html) [#claude](https://caleb.software/tag/claude.html)

I’ve been a longtime Claude Code user, ever since it became available in the Claude Pro subscription plan over a year ago. I remember the first time I used it, feeling giddy as it spat out messy Python code for whatever random weekend project I was working on, all from a single prompt. I’m certain that in a few years we’ll all look back at its release as one of the major drivers of this new era of software development we’re in.

Despite this, due to an accumulation of many small frustrations, I’ve finally decided to retire it as my main coding harness and migrate my development setup to use [Pi](https://pi.dev/) instead.

Someone at Anthropic must’ve caught wind… just a day after making my decision, they announced Claude Code would [finally start supporting](https://code.claude.com/docs/en/memory#agents-md) `AGENTS.md` files in an effort to get me to stay. Unfortunately, for me it was too little, too late.

## Complaining

In lieu of a proper obituary, I’d like to take this time to say a few words about the things I liked least about Claude Code (and thus what I’m looking forward to avoiding with Pi).

The `AGENTS.md` thing kind of epitomizes the whole reason I decided to quit using Claude Code. Don’t get me wrong, having a ton of `CLAUDE.md` files scattered about various GitHub hobby repos is great guerrilla marketing. But it sucks for users to have to deal with copying or symlinking between the two formats.

Like the Luddite I am, I still prefer to review most of the code it churns out before I commit it. I use(d) Claude Code inside VSCode, and it lets you connect the harness to the IDE. This is nice because you can highlight text in a file and have it automatically included in the context when asking questions or requesting changes during a review.

Except you can only have one session “connected” to the IDE at a time. If you open a second tab to start another task while the first one works, it can’t receive the text selection events from the IDE at all. This has been reported at *least* as far back as [December 2025](https://github.com/anthropics/claude-code/issues/14865)… and closed as a duplicate of a totally unrelated bug.[^1]

![claude code in vscode showing disconnected error](https://caleb.software/assets/img/claude-to-pi/vsc-disconnected.png)

Of course, you can always use the official Claude Code VSCode GUI extension, which does allow you to have multiple tabs open and load the text selection into the context of each. But this extension has plenty of problems of its own. Half of the built-in slash commands don’t work at all in the GUI. It doesn’t have any support for a custom status line. Subagent output messages are simply layered into the main chat thread in chronological order, which is a mess to read when more than one is running. I could go on and on.

In spite of all of these existing issues, it seems like Anthropic has worked at a feverish pace to pack more features into Claude Code without much care for quality or stability. [Dynamic workflows](https://code.claude.com/docs/en/workflows), [agent teams](https://code.claude.com/docs/en/agent-teams), [ultrareviews](https://code.claude.com/docs/en/ultrareview) – all great ways to help enterprise customers burn ever more tokens each day, but not really anything I want or need.

Alongside all of these feature releases, they always seem to be fiddling with things in ways that mess up my personal setup. At the time of writing, there have been **244** releases to Claude Code this year; over 3/4 of these changed the system prompt or tool descriptions, which can substantially change how the LLM performs (especially when these new instructions contradict user prompts or repo conventions).

And what are all those changes for? Despite [chopping 80% of the system prompt](https://claude.com/blog/the-new-rules-of-context-engineering-for-claude-5-generation-models), Claude Code still has way more overhead than Pi for seemingly no benefit. The default Bash tool’s description is almost 3,000 tokens, most of which is random stuff about using git and opening PRs on GitHub.[^2] The models already know how to do this without detailed instructions, by the way.[^3]

## Looking Forward

This post has been a lot of hating. I’m sorry! That’s not my usual style, and I do think Anthropic has made a really cool piece of software here. I’m also not under any delusion that I would’ve done a better job if I were the one in charge of running Claude Code from the beginning. LLM coding harnesses are a very new space and being one of the first to innovate in a field is a really hard task. Later tools like Pi have the benefit of hindsight to see what works and what doesn’t, what’s a core feature and what can be cut.

Ultimately, CC feels like a product without a strong vision or direction. It’s as if a bunch of excited engineers and PMs are rapidly tacking on whatever features suit their workflow… but without consideration for how this might make things worse for other users. It seems like there isn’t someone high-up whose job is strictly to say “no” to these requests, which is incredibly important when it comes to product engineering. So, for now, I’m leaving.

I’m excited to lean more into Pi. The first few days required some finagling, but since then it’s been smooth sailing. I’m enjoying the stability, minimalist approach, and the extreme level of customizability. I’ve already written a couple of extensions to support my use cases, which I’m hoping to share soon after I’ve cleaned them up a bit. If you’re also feeling these frustrations, I’d highly encourage giving an open source harness a try!

[^1]: It’s not like there’s some huge technical roadblock to this either. After installing Pi, I wrote an extension to handle this very functionality across multiple open tabs in like 20 minutes.

[^2]: Bonus points for the fact that it instructs the model to use `cat <<'EOF'` which (prior to widespread rollout of `auto` mode) would *always* trip the manual review rules regardless of existing git command allowlists.

[^3]: If you assume 1 million DAU and 100M requests per day, trimming 1,000 tokens off of this could literally save millions of dollars in inference costs per year. Surely that’s worth some engineer’s time??
