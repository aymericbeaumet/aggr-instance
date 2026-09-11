---
title: RTK reports token savings, but our cost benchmarks disagree
link: https://quesma.com/blog/does-rtk-make-ai-coding-cheaper/
source: hnrss-org-frontpage
published: 2026-09-11T11:15:13Z
updated: 2026-09-11T11:15:13Z
first_seen: 2026-09-11T17:43:44.917817170Z
authors:
- michalwarda
summary: 'Article URL: https://quesma.com/blog/does-rtk-make-ai-coding-cheaper/ Comments URL: https://news.ycombinator.com/item?id=49656471 Points: 108 # Comments: 56'
content: extracted
html: 2026-09-11-rtk-reports-token-savings-but-our-cost-benchmarks-disagree.html
preview:
  file: 2026-09-11-rtk-reports-token-savings-but-our-cost-benchmarks-disagree.preview-e0baf34145ba.webp
  width: 256
  height: 144
  alt: RTK reports huge token savings, but our cost benchmarks disagree
  color: '#c1b6b5'
images:
- source: https://quesma.com/_astro/rtk-terminal-bench-verdict-wide.Cx7zyGD2.png
  original:
    file: 2026-09-11-rtk-reports-token-savings-but-our-cost-benchmarks-disagree.image-6b9127343f58.png
    width: 1600
    height: 900
  color: '#f9f7f1'
- source: https://quesma.com/_astro/rtk-source-futurminds.InmKiHQt_Z1iq3Pi.webp
  original:
    file: 2026-09-11-rtk-reports-token-savings-but-our-cost-benchmarks-disagree.image-62f17f7002dc.webp
    width: 883
    height: 700
  variants:
  - file: 2026-09-11-rtk-reports-token-savings-but-our-cost-benchmarks-disagree.image-74536413f1c6.webp
    width: 48
    height: 38
  color: '#f7f7f7'
- source: https://quesma.com/_astro/rtk-source-sobalabs.CYWYKbvm_Z25vckK.webp
  original:
    file: 2026-09-11-rtk-reports-token-savings-but-our-cost-benchmarks-disagree.image-08f0c2f8117a.webp
    width: 780
    height: 365
  variants:
  - file: 2026-09-11-rtk-reports-token-savings-but-our-cost-benchmarks-disagree.image-b7a7129eb5f8.webp
    width: 48
    height: 22
  color: '#142a3b'
- source: https://quesma.com/_astro/rtk-source-computeleap.De0LOnMs_UCJy7.webp
  original:
    file: 2026-09-11-rtk-reports-token-savings-but-our-cost-benchmarks-disagree.image-66d43376a492.webp
    width: 790
    height: 300
  variants:
  - file: 2026-09-11-rtk-reports-token-savings-but-our-cost-benchmarks-disagree.image-6ca19442de2a.webp
    width: 48
    height: 18
  color: '#fdfdfd'
---

[RTK](https://github.com/rtk-ai/rtk) (Rust Token Killer) filters and compresses terminal output before the AI agent reads it. With over 79k GitHub stars today, RTK is one of the most popular tools to make AI coding cheaper.

One X post saying [RTK could cut Claude Code tokens by up to 60%](https://x.com/jasonzhou1993/status/2038215854584906078) reached 313K views.

[![FuturMinds video titled Claude Code plus RTK: Saves 90% Tokens, showing 31,000 views](https://quesma.com/_astro/rtk-source-futurminds.InmKiHQt_Z1iq3Pi.webp)](https://www.youtube.com/watch?v=CncyYt9ozAQ)\
[![Soba Labs article titled How we halved Claude Code token usage with RTK AI](https://quesma.com/_astro/rtk-source-sobalabs.CYWYKbvm_Z25vckK.webp)](https://sobalabs.ai/blog/halving-claude-code-token-usage-with-rtk/)\
[![ComputeLeap guide titled Cut Claude Code Token Costs 60–90% With rtk](https://quesma.com/_astro/rtk-source-computeleap.De0LOnMs_UCJy7.webp)](https://www.computeleap.com/blog/cut-claude-code-token-costs-rtk-guide-2026/)

Yet [JetBrains’s SkillsBench run found no savings](https://blog.jetbrains.com/ai/2026/07/rtk-claude-code-token-savings/). The [README](https://github.com/rtk-ai/rtk#how-savings-work) has a disclaimer:

> RTK cuts up to 90% of the bash output your agent reads. \[…\] it is not the same as cutting your bill by 90%.

So “less terminal output” is not the same as “cheaper AI coding”. It can help, be a no-op, or backfire (more turns or lower quality). In this post, we present our findings after several days and over $1,500 spent on tokens.

## How RTK works [](https://quesma.com/blog/does-rtk-make-ai-coding-cheaper/#how-rtk-works)

RTK can rewrite Git, test, package and file commands the agent runs through its shell tool (`Bash` in Claude Code, `bash` in OpenCode). Each rewrite returns a terser version of the same output.

For example, RTK keeps file names, sizes and permissions (`644` means `rw-r—r—`), but drops the owner and date:

```
$ ls -la /app/warriors
-rw-r--r-- 1 root root  824 Sep 13  2025 g2-clear.red
-rw-r--r-- 1 root root  487 Sep 13  2025 paper.red

$ rtk ls -la warriors/
644  g2-clear.red  824B
644  paper.red  487B
```

## Testing RTK on Terminal-Bench 2.1 [](https://quesma.com/blog/does-rtk-make-ai-coding-cheaper/#testing-rtk-on-terminal-bench-21)

RTK compresses terminal output, so we tested it on [Terminal-Bench 2.1](https://www.tbench.ai/news/terminal-bench-2-1), a benchmark with heavy terminal interaction. We stayed on 2.1 rather than the newer [3.0](https://www.tbench.ai/news/terminal-bench-3-0) and [4.0](https://www.tbench.ai/news/terminal-bench-4-0): agents pass most 2.1 tasks, while 3.0 and 4.0 are still a challenge. Cost only matters for tasks that pass.

We ran Claude Code with Fable 5.0, and OpenCode with DeepSeek V4 Pro 0813 through OpenRouter. Each task was scheduled five times without RTK and five times with it, on the same model route, platform and task-specific timeout.

After removing four Fable security tasks that got refusals, the final comparison covers 85 Fable tasks and 89 DeepSeek tasks, or 1,740 attempts.

## The first chart was promising [](https://quesma.com/blog/does-rtk-make-ai-coding-cheaper/#the-first-chart-was-promising)

With RTK, costs fell by 5% for Fable and rose by 5% for DeepSeek.

Claude Code · Fable 5.0

baseline $731 (84% pass rate)

RTK $698 (83% pass rate)

OpenCode · DeepSeek V4 Pro 0813

baseline $51 (71% pass rate)

RTK $54 (69% pass rate)

 Passed attempts Other attempts

Pass rates were lower with RTK: by 1% for Fable and 2% for DeepSeek. Both pass-rate gaps are small.

When we divided all spending, including failed attempts, by the number of passes, Fable was 3% cheaper with RTK, and DeepSeek was 7% more expensive.

Another way is to weight every task equally, because one expensive task can outweigh many cheap ones. We compared the mean of each task’s baseline attempts with the mean of its RTK attempts, then averaged those changes.

RTK cost change vs baseline

Total bill change Average change per task 95% confidence interval

On this task-level measure, Fable was 1% more expensive, with no clear difference from zero. DeepSeek’s task cost rose **17% on average**.

Accounting for failures does not change the trend. Across the 36 DeepSeek tasks where all ten attempts passed, the increase was still 18%.

## One task made the difference in the whole benchmark [](https://quesma.com/blog/does-rtk-make-ai-coding-cheaper/#one-task-made-the-difference-in-the-whole-benchmark)

Almost all of Fable’s savings with RTK came from one task: `winning-avg-corewars`. Both setups passed every attempt, but with RTK it finished in about half as many turns. Across the other tasks, the savings were less than 1%.

DeepSeek had the reverse result on that same task. Both setups passed every attempt, but RTK took more turns and cost more. Even without that task, costs remained higher with RTK.

## `rtk gain` is useless as a cost metric [](https://quesma.com/blog/does-rtk-make-ai-coding-cheaper/#rtk-gain-is-useless-as-a-cost-metric)

RTK documents [`rtk gain`](https://github.com/rtk-ai/rtk/blob/develop/docs/guide/resources/savings-explained.md#how-to-read-rtk-gain) as raw minus filtered command output in bytes, divided by 4, not a count of billed tokens.

Across 445 DeepSeek RTK attempts, RTK reported **349.2 million tokens saved**, a 89% reduction.

Large reported token savings did not mean cheaper tasks.

In `train-fasttext`, the model requested `head -1 train.txt` twice. RTK credited 120.5 million tokens saved each time by comparing those limited reads with the whole file. Those two calls accounted for **69% of the comparison’s savings counter**, although the requested commands would never have returned the whole file.

Treating `rtk gain` as money saved assumes the rest of the attempt would stay the same. RTK can change the agent’s next turns. `rtk gain` does not account for the cost of those turns.

This is where social posts go wrong: `rtk gain` counts removed output, not money saved, and it can make a more expensive attempt look optimized.

## RTK bugs can bite you [](https://quesma.com/blog/does-rtk-make-ai-coding-cheaper/#rtk-bugs-can-bite-you)

One DeepSeek `git-multibranch` attempt got stuck in a loop. The agent ran a `find` with a flag that `rtk find` 0.45.0 did not support. The plugin rewrote it to `rtk find`, which failed with “Use `find` directly”. Every retry was rewritten again. RTK [fixed this](https://github.com/rtk-ai/rtk/commit/6370e79275ef8c1063fc9b682bc36e7e63041b53) in 0.46.0, after our runs.

**339 consecutive errors**\~12 min

The agent accumulated **339 consecutive errors** before its timeout. It still passed the task, but cost **about 9× as much** as the matching baseline attempt, which also passed. One outlier attempt; the trend holds without it.

Without RTK, tool output made up about 11% of Fable’s input tokens and 40% of DeepSeek’s.

In the RTK attempts, 31% of Claude Code’s terminal calls and 51% of OpenCode’s terminal calls used RTK.

RTK rewrites only shell commands: its Claude Code hook matches the `Bash` tool and its OpenCode plugin acts on `bash` calls. Both platforms expose file reading and searching as separate `Read`, `Grep`, and `Glob` tools, which [bypass RTK](https://github.com/rtk-ai/rtk/blob/v0.45.0/README.md). About half of Claude Code’s Bash calls already limited their own output with `head`, `tail`, or `wc`.

In agentic coding, the context is cached after each turn, so later reads of terminal output mostly show up as cache reads. Those cost 1/10 of regular input tokens for Fable, and 1/30 for DeepSeek.

In DeepSeek, RTK reduced terminal-output characters by 9%, yet prompt tokens rose 9%. Uncached input fell 1% and cached input rose 9%. Model output, including reasoning, accounted for 56% of cost with RTK and 57% without it.

When the agent took more turns, task cost usually rose with it.

Claude Code · Fable 5.0

Turns, log scale

Cost ($), log scale

OpenCode · DeepSeek V4 Pro 0813

Turns, log scale

Cost ($), log scale

DeepSeek’s RTK attempts took more turns on 58 tasks, and 44 of them cost more. They took fewer turns on 28, and 23 of them cost less.

The average DeepSeek turn had 7% less input with RTK, but there were 18% more turns overall. Smaller turns did not add up to less total input.

One extra agent turn can cost more than the compression saved. It is the same [tokenflation](https://quesma.com/blog/tokenflation-when-hi-triggers-33-tool-calls) problem in another form. JetBrains saw the same pattern on SkillsBench: RTK added turns at low effort and did not lower cost at high effort.

## RTK does not make AI coding cheaper [](https://quesma.com/blog/does-rtk-make-ai-coding-cheaper/#rtk-does-not-make-ai-coding-cheaper)

On Terminal-Bench 2.1, Fable’s savings depended on one task and did not hold across tasks. We do not recommend RTK as a generic cost-saving tool.

Individual transcripts show that current frontier models already use the terminal efficiently (just ~7% of Fable’s context was terminal output). Models use techniques like `head -n` or `tail -n` themselves. RTK probably helped more with older models. Today it is a niche optimization, not a source of general savings.

*Tested with RTK 0.45.0, Claude Code 2.1.220, OpenCode 1.18.25 and Harbor 0.20. Trajectories available [on request](https://quesma.com/cdn-cgi/l/email-protection#21424e4f5540425561505444524c400f424e4c) for follow-up research. [Subscribe](https://quesma.com/blog/does-rtk-make-ai-coding-cheaper/#mce-EMAIL) for future posts, including our planned benchmark of [Headroom](https://github.com/headroomlabs-ai/headroom). Thanks to Piotr Migdał for his review and feedback.*
