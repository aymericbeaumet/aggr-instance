---
title: '500B tokens later: Letting AI agents decompile a first-person shooter'
link: https://momo5502.com/posts/2026-10-09-game-decompilation/
source: hnrss-org
published: 2026-10-11T02:02:38Z
updated: 2026-10-11T02:02:38Z
first_seen: 2026-10-11T23:56:01.055982684Z
authors:
- davikr
content: extracted
html: 2026-10-11-500b-tokens-later-letting-ai-agents-decompile-a-first.html
extra:
  comments_url: https://news.ycombinator.com/item?id=50039092
  num_comments: 101
  points: 134
---

During the last 3 months, I spent some of my time and tokens decompiling a popular first-person shooter. The goal was not to reach a simple proof-of-concept state. Instead, we really wanted an accurate, stable and feature-complete recreation of the game.

The avid reader of my blog might have noticed that I had previously written two posts that have since been removed. Everyone else might now be wondering which game I am talking about. To both of you I can only say that corporate America was here to ruin our fun.

However, that’s fine. This post is not about the game, it’s also less about the process of decompilation. It’s more about AI orchestration and how to optimize infrastructure, setup and harness for optimal results.

This project was done with the help of [RektInator](https://x.com/RektInator), [Future](https://x.com/RisottoNeroHit), [st0rm](https://x.com/NamelessNoobs) and other members of the community. A big thank you to all of them.

## What Was Our Goal?

We aimed at an accurate decompilation of the game to C++. Besides obvious semantic correctness, we had quite a few more requirements: We wanted readable C++ source that compiles. Given how old the game is, we also wanted security and bug fixes, but also portability improvements. It would be nice to run the game on Linux, macOS, in the browser, …

We later deferred modernization and portability to focus entirely on reconstructing the original behavior.

Obviously, the overall goal was to learn how to effectively orchestrate autonomous AI agents over the course of months.

## The Initial Setup

We started with Claude Max (20x), then added Codex Pro and used both subscriptions simultaneously. Model choice varied a lot. We had been using Sonnet 5 most of the time, but Opus 5.5, Luna, Sol and Terra were also used a lot. More on that later.

Claude agents were running in Claude Code CLI, Codex agents in Codex CLI. We also tried other agent harnesses, but the choice barely mattered, so we stuck to the defaults.

## Progress Tracking

Using GitHub CLI, the agents manage GitHub issues to track their progress. There is one issue per translation unit (.cpp file). Additionally, labels help group and prioritize issues.

![](https://momo5502.com/posts/2026-10-09-game-decompilation/images/issues.png)

## Communication

Agents communicate via [Discord](https://www.datacamp.com/tutorial/claude-code-channels). All of them have access to one channel and can both post and read all messages in there.

Discord allows agent-2-agent communication, as well as human-2-agent. So other participants can talk to them, without needing machine access.

A GitHub webhook posts CI failures into the shared channel, so agents get notified when something broke.

![](https://momo5502.com/posts/2026-10-09-game-decompilation/images/discord.png)

## Disassembly & Decompilation

Agents have been using the official [ida-mcp](https://github.com/HexRaysSA/ida-mcp) by Hex-Rays almost the entire time. It works great. It’s super stable, it’s headless and supports everything needed for this project. I can only recommend it.

## The First Month

We had 4 agents running at that time.

3 worker agents decompiling and committing and one reviewer agent that passively coordinates and reviews commits to flag bugs.

Agents managed to decompile about 80% of the game and visible progress was made. The game launched, the main menu was visible and we were able to load maps.

We spent a lot of those 4 weeks optimizing our setup:

We reduced token consumption by triggering earlier compactions.\
 The default compaction threshold is 90% context fill. We reduced it down to 42%. Decompilation consists of a lot of volatile information: A function that was decompiled is now irrelevant and could be removed from the context. So earlier compactions help remove such “junk” from the context.

We also noticed that agents tend to lose focus over time. Even within the span of one compaction cycle, agents can drift and lose focus, the more data the context holds.

Agents sometimes moved to another function before finishing the previous one. They also started idling while watching CI, despite receiving failure notifications on Discord. Occasionally, they closed issues without thoroughly checking whether the work was actually complete.

Working side by side in a terminal, you can steer the agents to prevent that, but when letting them work autonomously, steering is not possible.

To prevent that, we wrote a document defining our goal, how agents should work, what they must avoid and how to handle specific situations.

An hourly cron job automatically injected a request for agents to reread this document, keeping the instructions fresh in their context. While this might not be the ideal method to keep agents focused, it worked really well through the end of the project.

Lots of time was spent refining the instruction document. As the content is extremely specific to this project, it makes less sense to share it here, though.

## However…

… despite all our efforts optimizing setup and infrastructure, we need to talk about the quality of the work.

Constant progress (game starting, menu rendering, maps loading) led us to believe the quality of the decompilation was great.

It was not. Despite the code being extremely readable, it was semantically wrong.

Agents used wrong function signatures, types or struct layouts. They invented logic or removed it where deemed unnecessary.

Beyond semantic errors, agents also introduced unnecessary architectural changes. As an example, the game has certain configuration variables that it accesses via global variables. The agents had turned this constant memory access into hash tables with a lookup that was orders of magnitude more expensive.

And that’s just one example of the many things that went wrong.

## Why is that?

While a reviewer helps catch bugs, it does not work well for anything beyond that. Architectural decisions were not questioned, as long as they aligned with the goal.

The main reason for that is that we didn’t have objective acceptance criteria. We never properly defined “correctness”. Therefore it was hard for the reviewer to judge which change is correct and what’s wrong.

Obviously it had the game as reference, but given that we had modernization and portability on the list as well, certain deviations were not treated as bugs.\
 Interestingly, comments in the commits or code led the reviewer to accept deviations, due to whatever justification the worker agent had written down. The workers’ comments effectively acted as unintentional prompt injection: the reviewer accepted their justifications instead of independently checking the deviations against the original.

## The Oracle

What we needed was an automated check that tells agents whether a reconstructed function matches the original. It should verify identical semantics. A simple PASS or FAIL signal would be enough and the agent can figure out on its own what’s wrong.

## Byte Matching Decompilation

The simplest way to achieve this was byte matching decompilation.

We switched to the compiler used to build the original game and wrote a script that performs the comparison.

The script reads our reconstructed OBJ file and the game EXE/PDB (having the PDB is great and makes things slightly simpler, but the process can work just as well without a PDB).

It then extracts the function data from OBJ and EXE and compares all bytes. If they match, the function is exact, otherwise it fails and the agent needs to rework the function.

![](https://momo5502.com/posts/2026-10-09-game-decompilation/images/graph.png)

References to other functions or data won’t necessarily match byte for byte, because their encoded values depend on where the targets end up in the compiled binary.

Luckily, the OBJ file records these references as relocations. We can exclude the relocation bytes from the direct comparison and instead verify that both versions reference the same symbol with the same offset.

![](https://momo5502.com/posts/2026-10-09-game-decompilation/images/bytes.png)

The script then does the same for data and types.

Agents can then use this script to verify their work, before pushing it.

Reconstructed functions are recorded in a set of text files. CI can then use these text files to verify all recorded functions and alert in case of regressions.

## Cheating

The first thing agents did when we introduced this script was write inline assembly.

This obviously defeats the purpose. So we had to refine our instructions to disallow certain constructs. Naked functions, object patching, inline assembly and embedding bytes in the code were forbidden verbally. Given how easy it is to scan for those constructs, verbal rules were enough.

However, agents repeatedly tried to modify this script to exclude their function from comparison.

To prevent them from doing this, CI hashes the verification script and compares it against a stored GitHub Actions secret.

## Trade-Offs

### Cons

Functions can be hard to match. Register selection, inlining decisions and calling conventions can be difficult to reproduce exactly. In rare cases, we also observed different compiler output from identical inputs. Agents now need much longer for decompilation, without necessarily producing better results. A function can have identical semantics, despite showing certain divergences, e.g. independent instructions that are shuffled.

### Pros

Matching functions are guaranteed to have identical semantics. This preserves the original behavior, including any existing bugs, and prevents reconstruction errors in those functions. On top of this, the reviewer agent is not needed anymore.

Another benefit is that cheaper, less capable models can now reliably work on the task. Previously models like Haiku or Luna were a bad fit and produced extremely bad results. However, given this strict acceptance criterion, they now have enough feedback to produce incredible results, reducing the costs drastically and allowing the project to scale up massively.

## Final State

With our new verification harness, agents have been working for almost 2 more months now. 99% of the game’s functions are present in our reconstructed source, with 83% of all functions being byte exact.

![](https://momo5502.com/posts/2026-10-09-game-decompilation/images/progress.png)

For the most part, we have been using 14 Luna and 2 Opus 5.5 agents throughout the final weeks.

At that scale, workers used separate branches and submitted their changes through pull requests.

This is also where Discord stopped scaling well. So many agents spamming the channel is nonsense. We restricted messages to which issues they were taking and CI coordination. That kept communication to a minimum. However, the further we got into the project, the less we needed to talk to the agents. Human-2-agent communication was no longer needed as they worked fully autonomously. For other projects at that scale, I would likely choose something other than Discord.

At this point, we’re hitting diminishing returns. The remaining functions mostly have certain non-deterministic characteristics or can’t be matched due to other circumstances, e.g. identical COMDAT folding in the linker that we cannot reliably reproduce.

The Opus 5.5 agents are still able to move forward and get the remaining functions matched. However, the game runs flawlessly now. There are no noticeable bugs and all features of the original game are present.

The remaining functions have been reworked repeatedly. While they still don’t match byte for byte, we believe their semantics are correct.

Continuing the byte matching process for them would consume more tokens without meaningfully improving the result. That means, the project can be considered done now.

## Lessons

This project has taught us a lot. Here are the most valuable takeaways at a glance:

- Precise instructions are necessary. Agents have the desire to cheat if the assignment leaves room for interpretation.

- Correctness should be defined and machine-checkable. Humans are notoriously incapable of precisely articulating their intent. Therefore, I don’t think reviewers will ever be enough. A reliable verification harness that delivers an objective PASS or FAIL signal is the best feedback an agent can get. Obviously not every project has the luxury decompilation has. Yet, I think for every project it is possible to get close to that, with enough creativity.

- A good harness makes cheaper models viable. When feedback is precise and acceptance is strict, smarter models don’t necessarily perform better.

- Instructions decay over time. Agents forget or treat certain rules as less important, the more time passes and the more context gets compacted. During interactive work, you can correct that drift as it happens. When agents work autonomously, it can go unnoticed and undermine the quality of their work. The hourly refresh of the instructions solved that.

- Generating code is cheap now. Throw it away if it’s bad. After the first 4 weeks, when we noticed the code was bad, we tried to salvage it. This did in fact cost us more time than starting from scratch.

- Correctness is so much more important than productivity. Reducing token consumption, scaling up agents, optimizing agent throughput, etc. is great and all, but if the result is bad, it doesn’t help much.

## Final Words

This was honestly such a valuable project and I learned so much. I don’t think writing down these takeaways can convey the sheer amount of lessons we’ve learned throughout the process.

So many things went wrong, yet so much went right. As AI agents take on more software development work, orchestrating them becomes a new, much more demanding task for humans.

Scaling up to 15+ agents revealed even more difficulties. Agents periodically wiped the VM because of malformed commands. There are already sandboxing solutions for Windows, yet none of what I have seen currently fits my needs. I have started extending [Sogen](https://sogen.dev), my userspace emulator, to provide lightweight and scalable sandboxing capabilities. However, it will take a while before this becomes even remotely production ready.

Given that the VMs were wiped, we unfortunately lost a bunch of session logs. So I don’t know the exact number of tokens that were spent. My estimate is something between 600-700 billion tokens. This figure might seem high. However, given that we have used Luna for the most part, it might in fact not be. Probably around 300 billion tokens were spent with Luna. Compared to Astra, with a ratio of 100:1, that equates to ~3 billion Astra tokens.

For obvious reasons, none of the code will be shared. Everything will stay private and I will use it for my own needs only.
