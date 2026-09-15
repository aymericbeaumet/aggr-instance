---
title: 'GPT-5.6 Luna vs. GPT-6 Astra: Is a $1.20 Model Good Enough for Code Review?'
link: https://entelligence.ai/blogs/gpt-5.6-luna-vs-gpt-6-astra-is-a-1.20-model-good-enough-for-code-review
source: hnrss-org-frontpage
published: 2026-09-14T19:56:20Z
updated: 2026-09-14T19:56:20Z
first_seen: 2026-09-15T01:23:10.737129797Z
authors:
- theanonymousone
summary: 'Article URL: https://entelligence.ai/blogs/gpt-5.6-luna-vs-gpt-6-astra-is-a-1.20-model-good-enough-for-code-review Comments URL: https://news.ycombinator.com/item?id=49703003 Points: 104 # Comments: 109'
content: extracted
html: 2026-09-14-gpt-5-6-luna-vs-gpt-6-astra-is-a-1-20-model-good-enough-for.html
preview:
  file: 2026-09-14-gpt-5-6-luna-vs-gpt-6-astra-is-a-1-20-model-good-enough-for.preview-27b7bd5b0444.webp
  width: 256
  height: 134
  color: '#72736c'
images:
- source: https://framerusercontent.com/images/gPrTReKBwM5bVRyIqVcgBE4kEAk.png
  original:
    file: 2026-09-14-gpt-5-6-luna-vs-gpt-6-astra-is-a-1-20-model-good-enough-for.image-3aad781e1b02.png
    width: 1836
    height: 961
  color: '#13140c'
- source: https://framerusercontent.com/images/DOHHuAt891rBMgV4LR9A0TSso.png?width=3200&height=1290
  original:
    file: 2026-09-14-gpt-5-6-luna-vs-gpt-6-astra-is-a-1-20-model-good-enough-for.image-ba51a83a78fa.png
    width: 3200
    height: 1290
  color: '#fcfbf7'
- source: https://framerusercontent.com/images/O1IH9Y2u0ElsjcWu7cfzC7vgAw.png?width=3200&height=1920
  original:
    file: 2026-09-14-gpt-5-6-luna-vs-gpt-6-astra-is-a-1-20-model-good-enough-for.image-8beab892e733.png
    width: 3200
    height: 1920
  color: '#fdfcf8'
- source: https://framerusercontent.com/images/WOugCTuPRMuJppcheyLepXp2Q0E.png?width=3200&height=1520
  original:
    file: 2026-09-14-gpt-5-6-luna-vs-gpt-6-astra-is-a-1-20-model-good-enough-for.image-ff9e8d833199.png
    width: 3200
    height: 1520
  color: '#fdfcf9'
- source: https://framerusercontent.com/images/os5lorpdsOsJFDFbVYRNCEdM.png?width=3200&height=1800
  original:
    file: 2026-09-14-gpt-5-6-luna-vs-gpt-6-astra-is-a-1-20-model-good-enough-for.image-9f450b5c3ed0.png
    width: 3200
    height: 1800
  color: '#fdfcf8'
- source: https://framerusercontent.com/images/OW66FYCP8JJNYHRRFtdXb9rQY4.png?width=3200&height=1800
  original:
    file: 2026-09-14-gpt-5-6-luna-vs-gpt-6-astra-is-a-1-20-model-good-enough-for.image-95b6d6e799fe.png
    width: 3200
    height: 1800
  color: '#fdfcf8'
- source: https://framerusercontent.com/images/Vdqne192yOXW8G0gl9qwuhVhPeo.png?width=3200&height=1920
  original:
    file: 2026-09-14-gpt-5-6-luna-vs-gpt-6-astra-is-a-1-20-model-good-enough-for.image-90d8e3ea5e14.png
    width: 3200
    height: 1920
  color: '#fdfcf8'
---

![](https://framerusercontent.com/images/DOHHuAt891rBMgV4LR9A0TSso.png?width=3200&height=1290)

GPT-5.6 Luna costs $0.20 per million input tokens and $1.20 per million output tokens. GPT-6 Astra costs $10 and $50. On the same pull requests, one Luna review cost $0.0041 and one Astra review cost $0.113, a 28x difference.

Our [last post](https://entelligence.ai/blogs/gpt-6-astra-cost-1.6x-more-per-verified-bug-than-gpt-5.6-sol) compared Astra with GPT-5.6 Sol. This time we wanted to know what you give up if every pull request goes through the cheapest model.

## The short answer

Luna found 69 verified bugs across 50 pull requests. Astra found 92. Luna cost $0.20 for the whole run and Astra cost $5.66. Luna was wrong more often, with 24 of its 93 findings failing verification against Astra's 4 of 96, and it found 9 of the 24 security bugs where Astra found 19.

Our read: Luna is good enough for everyday correctness bugs at that price, and we wouldn't let it review authentication or permission code on its own.

## How we ran it

We reused the setup from the Astra vs Sol post so the numbers line up.

The pull requests are the 50 public benchmark PRs in the [AI-Code-Review-Evals](https://github.com/AI-Code-Review-Evals) organization, ten each from Cal.com, Sentry, Discourse, Keycloak and Grafana. Each one introduces defects against a clean base branch.

Luna and Astra got the same prompt on the same diffs. The prompt asks for correctness, security, concurrency, resource and error-handling bugs, and excludes style, naming, docs and test suggestions. Each model returned structured findings.

Verification works the same way as before. For every pull request, the findings from Astra, Sol, Luna and the public Entelligence reviewer comments go into one anonymized list. GPT-6 Astra and GPT-5.6 Sol each judge that list separately against the diff, grouping duplicates and deciding whether each issue is a real bug. An issue only counts as verified when both judges call it real. They agreed on 91% of findings, and 143 distinct bugs passed both.

Adding Luna's findings changed the pool the judges saw, so everything was judged again. Astra's verified count moved from 91 in the last post to 92 here, and Sol's from 107 to 108. Astra is also one of the two judges, which could favor it slightly. The limits section covers that.

## The results

|                               | GPT-5.6 Luna | GPT-6 Astra |
| ----------------------------- | ------------ | ----------- |
| Verified bugs                 | 69           | 92          |
| Findings raised               | 93           | 96          |
| Precision                     | 74%          | 96%         |
| Total cost, 50 PRs            | $0.20        | $5.66       |
| Cost per verified bug         | $0.0030      | $0.061      |
| Mean time per review          | 23s          | 36s         |
| Mean output tokens per review | 2,104        | 688         |

![Scatter of total cost against verified bugs on a log cost axis. Luna at $0.20 and 69 bugs, Sol at 108 bugs, Astra at $5.66 and 92 bugs](https://framerusercontent.com/images/O1IH9Y2u0ElsjcWu7cfzC7vgAw.png?width=3200&height=1920)

Luna found 75% as many verified bugs as Astra for 3.6% of the money. Per verified bug, Astra cost 20x more.

Luna wrote 3.1x as many output tokens per review as Astra and still came in far cheaper, because its output price is 42x lower. It was also faster, at 23 seconds per review against 36.

Your team would feel the precision gap first. About one Luna comment in four was wrong, while Astra was wrong 4 times in 96. Developers who already skim AI review comments will skim harder when a quarter of them are noise.

## Where Luna falls behind

Readers of the last post asked us to split results by codebase and by bug type, because an overall score can hide a model that does well on one repository and badly on another. On this data, the split shows where Luna's missing bugs come from.

![](https://framerusercontent.com/images/WOugCTuPRMuJppcheyLepXp2Q0E.png?width=3200&height=1520)

In Sentry, Discourse and Grafana, Luna came within two verified bugs of Astra. Cal.com had a wider gap, 21 to 30. Keycloak had the widest: Luna found 6 verified bugs to Astra's 14, and only 50% of its Keycloak findings held up, against 93% for Astra.

Keycloak is an identity and access management server, and most of its benchmark PRs change authentication and permission logic. The bug-class split points the same way.

![Verified bugs by class for Luna and Astra. Security shows the widest gap, 9 of 24 for Luna against 19 for Astra](https://framerusercontent.com/images/os5lorpdsOsJFDFbVYRNCEdM.png?width=3200&height=1800)

We labeled every verified bug by root cause. GPT-5.6 Sol, which isn't one of the two models compared here, labeled all 143 bugs in one pass against written definitions. The labels are committed alongside the benchmark data so anyone can check them.

On data and logic bugs, the largest group, Luna found 39 to Astra's 47. On concurrency it found 10 to 13. On security, Luna found 9 of 24 and Astra found 19.

Two of the Keycloak bugs Astra caught and Luna didn't:

- Federated recovery codes were never marked as used, so a recovery code could be used more than once.

- A global view permission overrode denials set on individual clients.

Neither looks wrong on any single line. You only see them by working out what the permission model allows after the change.

## What Luna catches that Astra misses

![Donut of all 143 verified bugs: 44 found by both, 25 only by Luna, 48 only by Astra, 26 by neither](https://framerusercontent.com/images/OW66FYCP8JJNYHRRFtdXb9rQY4.png?width=3200&height=1800)

Luna also found bugs Astra missed. Of the 143 verified bugs, 44 were found by both models, 48 only by Astra, and 25 only by Luna.

Of the 25 Luna-only bugs, 16 are data and logic bugs and 4 are concurrency bugs. In Discourse, repeating an unsubscribe request kept lowering a user's notification level. In Sentry, a concurrency bug replaced unhealthy worker threads without stopping the old ones.

Running both models on every pull request would have found 117 of the 143 verified bugs (82%) for $5.86 in total. That is Luna's $0.20 on top of Astra's $5.66, for 25 more verified bugs.

## What readers asked us to check

### Did the models just remember the fixes?

One reader pointed out that these repositories are public, and the fixes for the benchmark bugs may sit in their history. A model trained after those fixes landed could be recalling a patch it has already seen. The suggested test was to split the pull requests by date and see whether the ranking holds on changes made after each model's training cutoff.

We can't run that split on this benchmark. We pulled the commit date behind every PR, and they range from 2013 to July 25, 2025. 20 are from 2025, and none are recent enough to fall after either model's cutoff. The post-cutoff group would be empty.

The risk is smaller than it sounds, because the defects were added to these PRs for the benchmark on purpose, so the exact bug in each diff is not a commit a model could have trained on. The surrounding code is old and public, though, and a model that knows what the correct version looks like has an advantage. Testing that properly needs pull requests newer than the models, and this benchmark can't provide them.

### Do the models find the same bugs twice?

Another reader asked us to rerun some PRs with identical settings. We picked two PRs per codebase and ran each model two more times.

![Dot plot of finding counts across three runs for ten pull requests. Astra re-found 67 percent of its verified bugs in both repeat runs, Luna 47 percent](https://framerusercontent.com/images/Vdqne192yOXW8G0gl9qwuhVhPeo.png?width=3200&height=1920)

From its first run, Astra had 15 verified bugs on those ten PRs. 10 came back in both repeats and 14 in at least one. Luna also had 15. 7 came back in both repeats and 12 in at least one.

The sample is small, so treat these as rough. A model that finds a bug on one run can miss it on the next, which applies to every single-run number in this post, and Luna did it more often than Astra.

### What about bugs nobody flagged?

The third request was to track false negatives, meaning real bugs every model missed. Measuring that needs a complete list of the bugs in each PR, which the benchmark doesn't publish.

We can give a lower bound. 26 verified bugs were missed by both Luna and Astra and caught only by Sol or the Entelligence reviewer. Two of them are the Discourse security bugs from our last post: a `postMessage` origin check that used a substring match, and a remote fetch that followed redirects past a host allowlist. The true number of missed bugs is higher, because bugs no reviewer flagged never enter the pool.

## Limits of this comparison

- Apart from the ten repeated PRs, each model reviewed each PR once, and the repeat runs show that results move between runs.

- Astra is both a contestant and one of the two judges. Requiring Sol to agree reduces the bias without removing it.

- Every PR predates both models' training cutoffs, so the date split readers asked for isn't possible here.

- Both models saw the diff and nothing else. They had no repository history, call graph, or production data.

- Verified counts are a floor on the bugs present, and the benchmark has no complete bug list to measure against.

## What a diff doesn't tell the model

On this benchmark, a cheap model did well on most changes and badly on authentication and permission code. A diff alone doesn't tell the model which kind of change it is reviewing.

Knowing that a file sits on an authorization path, that a function is called from a login flow, or that a similar change caused an incident last quarter decides how carefully a change should be reviewed. [Entelligence code review](https://entelligence.ai/code-review) reviews pull requests with full-repository context and feeds production behavior back into later reviews, which is the information a diff-only model is missing.

For coding agents, [Entelligence Model Router](https://entelligence.ai/model-router) sends routine steps to cheaper models and harder steps to stronger ones. Our earlier [Terminal-Bench comparison](https://entelligence.ai/blogs/entelligence-router-solved-8-more-tasks-than-claude-opus-5-at-65-lower-cost) covers how that played out on agent tasks.

## Running this on your own code

1. Collect 30 to 50 merged pull requests from your repositories that later needed a fix.

2. Run a cheap model and an expensive model with the same prompt.

3. Verify findings with a judge that isn't one of the two models, or have both a judge and a person check a sample.

4. Split results by repository and by bug class, since an average hides the weak spots.

5. Rerun a handful of PRs to see how much the results move.

6. Compare cost per verified bug, and look separately at the classes where a miss is expensive.

## Frequently asked questions

### Is GPT-5.6 Luna good enough for code review?

For general correctness bugs, it came close to Astra on this benchmark. Luna found 39 data and logic bugs to Astra's 47 at a small fraction of the cost. For security-sensitive code it fell well behind, finding 9 of 24 security bugs to Astra's 19.

### How much cheaper is Luna than Astra per review?

On these pull requests, a Luna review cost $0.0041 and an Astra review cost $0.113, about 28x less. Per verified bug, Luna cost $0.0030 and Astra cost $0.061.

### Is Luna noisier than Astra?

Yes. 74% of Luna's findings were verified, compared with 96% of Astra's, so roughly one Luna comment in four didn't hold up.

### Should you run both models?

Running both found 117 of the 143 verified bugs for $5.86 across 50 pull requests, compared with 92 for Astra alone. Whether the extra bugs are worth the extra noise depends on how your team handles review comments.

### Can I reproduce this?

Yes. The pull requests are public, and the prompts, raw model outputs, judge verdicts, bug-class labels, repeat runs and scoring scripts are committed with this article.

## Summary

Luna found three-quarters of Astra's verified bugs for less than 4% of the cost. It fell furthest behind on authentication and permission code, where a missed bug tends to cost the most. A setup that reviews most changes cheaply and gives security-sensitive ones more scrutiny can use that trade, but it needs to know which changes are which.

[See how Entelligence reviews pull requests with full repository context](https://entelligence.ai/code-review).

*Methodology: 50 public pull requests from AI-Code-Review-Evals, reviewed by GPT-5.6 Luna and GPT-6 Astra with an identical bug-only prompt, September 2026. Findings from Luna, Astra, GPT-5.6 Sol and the public Entelligence reviewer comments were pooled per pull request and judged separately by GPT-6 Astra and GPT-5.6 Sol; a bug counts only where both judges agreed. Bug classes were labeled by GPT-5.6 Sol. Ten pull requests were reviewed three times per model. Prices are $0.20/$1.20 per million tokens for Luna and $10/$50 for Astra.*
