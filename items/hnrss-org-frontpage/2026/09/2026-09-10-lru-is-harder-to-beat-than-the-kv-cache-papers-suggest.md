---
title: LRU is harder to beat than the KV-cache papers suggest
link: https://github.com/gauravapiscean/agentic-kv-cache
source: hnrss-org-frontpage
published: 2026-09-10T13:39:11Z
updated: 2026-09-10T13:39:11Z
first_seen: 2026-09-13T01:23:10.731749865Z
authors:
- gauravapiscean
summary: 'Article URL: https://github.com/gauravapiscean/agentic-kv-cache Comments URL: https://news.ycombinator.com/item?id=49643543 Points: 100 # Comments: 47'
content: extracted
html: 2026-09-10-lru-is-harder-to-beat-than-the-kv-cache-papers-suggest.html
preview:
  file: 2026-09-10-lru-is-harder-to-beat-than-the-kv-cache-papers-suggest.preview-ce714cc28684.webp
  width: 256
  height: 128
  alt: Reproducing agentic KV-cache policy claims on real traces. 68k requests from 393 Claude Code sessions. LRU is harder to beat than the papers suggest. - gauravapiscean/agentic-kv-cache
  color: '#eff1f3'
images:
- source: https://opengraph.githubassets.com/aa48b48bbd1e0d559cda23873eb6c65c8e06b965e9b834ffff41b60f0f3165a1/gauravapiscean/agentic-kv-cache
  original:
    file: 2026-09-10-lru-is-harder-to-beat-than-the-kv-cache-papers-suggest.image-b040f4b0e39c.png
    width: 1200
    height: 600
  variants:
  - file: 2026-09-10-lru-is-harder-to-beat-than-the-kv-cache-papers-suggest.image-9c5c9c716c0d.webp
    width: 48
    height: 24
  color: '#fdfdfd'
---

I replayed **68,266 requests from 393 real Claude Code sessions** and **23,608 Mooncake requests** through a prefix-cache simulator, tried to beat the production baseline three different ways, and failed. The interesting part is why: under capacity pressure, most recomputation comes from tool-calling loops *seconds* apart, not from sessions idling past a TTL — and the TTL never fires at all.

Everything here reproduces from a cold checkout with `make setup data repro`.

* * *

## Contents

[](https://github.com/gauravapiscean/agentic-kv-cache#contents)

- [What I built](https://github.com/gauravapiscean/agentic-kv-cache#what-i-built)
- [Validation: reproducing Mooncake's published curve](https://github.com/gauravapiscean/agentic-kv-cache#validation-reproducing-mooncakes-published-curve)
- [1\. Agent sessions are idle more than published](https://github.com/gauravapiscean/agentic-kv-cache#1-agent-sessions-are-idle-more-than-published)
- [2\. Under capacity pressure, the waste isn't where I expected](https://github.com/gauravapiscean/agentic-kv-cache#2-under-capacity-pressure-the-waste-isnt-where-i-expected)
- [3\. The 5-minute TTL never fired under capacity pressure](https://github.com/gauravapiscean/agentic-kv-cache#3-the-5-minute-ttl-never-fired-under-capacity-pressure)
- [4\. Three ways to beat LRU, three failures](https://github.com/gauravapiscean/agentic-kv-cache#4-three-ways-to-beat-lru-three-failures)
- [5\. The harness bug that makes Belady lose to LRU](https://github.com/gauravapiscean/agentic-kv-cache#5-the-harness-bug-that-makes-belady-lose-to-lru)
- [What I think this means](https://github.com/gauravapiscean/agentic-kv-cache#what-i-think-this-means)
- [Limitations](https://github.com/gauravapiscean/agentic-kv-cache#limitations)
- [Reproduce it](https://github.com/gauravapiscean/agentic-kv-cache#reproduce-it)

* * *

Cross-request KV prefix caching is the largest practical lever in agentic LLM serving. It's why your coding agent's fiftieth turn costs a fraction of its first. Every serving stack has one — vLLM's automatic prefix caching, SGLang's RadixAttention, LMCache, Mooncake Store — and all of them evict with LRU by default. (SGLang also ships LFU, SLRU, Priority and others behind `--radix-eviction-policy`; LRU is the shipped default.)

There's a large, fast-growing literature arguing LRU is the wrong policy for agentic workloads, because agent sessions go idle and LRU can't tell a paused session from a dead one. The argument is intuitive. I believed it, and built a simulator to exploit it.

It didn't work, and why it didn't work turned out to be more interesting than the policy would have been.

## What I built

[](https://github.com/gauravapiscean/agentic-kv-cache#what-i-built)

A block-granular, discrete-event simulator of a cross-request prefix cache. Three properties that matter, and that quick implementations tend to get wrong:

**Hits are prefix-contiguous.** A hit is the *longest resident prefix* of the block chain, not a set intersection. Miss one block at depth 3 and everything after it is unusable even if it's still resident.

**The radix structure constrains eviction.** A block with resident children isn't evictable. So the baseline is LRU *over radix leaves*, which is what SGLang and vLLM actually implement. Beating naive flat LRU would be a strawman.

**The in-flight chain must be pinned.** See [finding 5](https://github.com/gauravapiscean/agentic-kv-cache#5-the-harness-bug-that-makes-belady-lose-to-lru).

Traces are real, not synthetic:

| trace                                       | requests                                   | block size | hash scope    | source                                                                                       |
| ------------------------------------------- | ------------------------------------------ | ---------- | ------------- | -------------------------------------------------------------------------------------------- |
| **SemiAnalysis AgentX**                     | 68,266 across **393 Claude Code sessions** | 64 tok     | session-local | [HF](https://huggingface.co/datasets/semianalysisai/cc-traces-weka-062126-256k) (Apache-2.0) |
| **Mooncake** `mooncake_trace` / `toolagent` | 23,608                                     | 512 tok    | **global**    | [GitHub](https://github.com/kvcache-ai/Mooncake) (Apache-2.0)                                |
| **Mooncake** `conversation`                 | 12,031                                     | 512 tok    | global        | same                                                                                         |

## Validation: reproducing Mooncake's published curve

[](https://github.com/gauravapiscean/agentic-kv-cache#validation-reproducing-mooncakes-published-curve)

Before trusting anything, I reproduced Mooncake's published hit-rate-vs-capacity table on Mooncake's own released trace, with their stated policy.

| cache (blocks)                | 1k    | 10k   | 30k   | 50k   | 100k  | ∞     |
| ----------------------------- | ----- | ----- | ----- | ----- | ----- | ----- |
| **published (LRU)**           | 0.30  | 0.40  | 0.48  | 0.50  | 0.51  | 0.51  |
| **measured (radix-leaf LRU)** | 0.341 | 0.460 | 0.537 | 0.551 | 0.552 | 0.553 |
| **measured (flat block LRU)** | 0.340 | 0.460 | 0.537 | 0.551 | 0.552 | 0.553 |

The shape reproduces exactly, including the saturation point they describe in prose ("1,000 to 50,000 blocks boosts the cache hit ratio from 30% to 50%; further capacity increases show minimal improvement").

**There is a systematic +4–6pp offset I could not explain.** I tested five metric definitions — block denominator, token denominator, dropping the partial tail block, per-request averaging — and none closes it. The infinite-cache case is policy-free, a pure property of the trace, so the discrepancy is definitional or a trace-version mismatch, not a replay bug.

Publishing it unresolved rather than tuning until it matches. **If you know why, please open an issue.**

> **Incidental finding:** flat block LRU and radix-leaf-restricted LRU differ by **0.02pp** on this workload. The leaf restriction both major engines implement buys essentially nothing here.

Reproduce: `make validate`

## 1\. Agent sessions are idle more than published

[](https://github.com/gauravapiscean/agentic-kv-cache#1-agent-sessions-are-idle-more-than-published)

```
sessions=393  requests=68266

session span (h):   p50=1.84  p90=28.36  max=254.8
inter-req gap (s):  p50=2.1   p90=51.1   p99=3426.3   max=491922   (5.7 days)
   gaps >   60s: 9.5%
   gaps >  300s: 3.3%
   gaps > 3600s: 1.0%
input tokens:       p50=88768  p90=204288  max=255808
output tokens:      p50=376    p90=1845
requests/session:   p50=70     max=3551

DUTY CYCLE (fraction of wall-clock actually executing):
   p25=3.4%   p50=13.9%   p75=33.9%
   sessions executing <50% of lifetime: 85.5%
```

The most-cited characterization of agentic serving reports a **20%** median duty cycle and **70%** of sessions below 50%. On this independent trace it's **13.9%** and **85.5%** — the premise is *more* extreme than published, not less.

Note the shape: gaps are bimodal. A median of **2.1 seconds** (tight tool loops) with a heavy tail out to days.

Reproduce: `make characterize`

## 2\. Under capacity pressure, the waste isn't where I expected

[](https://github.com/gauravapiscean/agentic-kv-cache#2-under-capacity-pressure-the-waste-isnt-where-i-expected)

This is the finding that changed my mind.

AgentSysBench ([arXiv:2608.15127](https://arxiv.org/abs/2608.15127)) reports that "cache evictions contribute 55.9% of the total cache-create tokens and account for **31.5% of aggregate monetary cost**," driven by a 5-minute provider TTL colliding with 1–10 minute idle gaps. That motivated my entire approach.

So before optimizing for it, I measured where recompute comes from — policy-independently. Replay the trace, and bucket every request's recomputed tokens by the idle gap that preceded it:

| gap before request | requests | share of all recompute tokens |
| ------------------ | -------- | ----------------------------- |
| **<10 s**          | 10,069   | **33.1%**                     |
| 10–60 s            | 912      | 7.0%                          |
| 1–5 min            | 701      | 20.5%                         |
| 5–30 min           | 236      | 8.6%                          |
| 30–60 min          | 50       | 3.0%                          |
| \>1 h              | 123      | 5.8%                          |

**Requests arriving after a gap longer than 5 minutes account for 17.5% of recompute. Requests arriving within 10 seconds account for 33.1%.**

The dominant source of cache misses here is **tight two-second tool loops whose 88k-token working sets exceed cache capacity** — a *capacity* problem, not a liveness-prediction problem. With a p50 gap of 2.1 seconds, almost every session is "about to return," so a liveness estimator has essentially nothing to discriminate on.

> ### ⚠️ This does not contradict the 31.5% figure — read this before citing either
>
> [](https://github.com/gauravapiscean/agentic-kv-cache#%EF%B8%8F-this-does-not-contradict-the-315-figure--read-this-before-citing-either)
>
> The two numbers measure different things in different regimes, and I initially framed this as a contradiction. It isn't.
>
> |             | AgentSysBench                                                                                                    | this repo                                                             |
> | ----------- | ---------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------- |
> | numerator   | eviction-caused cache-create tokens, priced at $6.25/M                                                           | recomputed prefill tokens after a >5min gap                           |
> | denominator | **total bill** (incl. cache reads and output tokens)                                                             | **all recompute tokens**                                              |
> | regime      | **TTL-bound** — a provider cache where per-customer capacity is effectively unlimited and entries die on a timer | **capacity-bound** — 40,000 blocks against a ~10.7M-token working set |
>
> In a TTL-bound cache, essentially all evictions are gap-driven by construction. My setup never enters that regime — which [finding 3](https://github.com/gauravapiscean/agentic-kv-cache#3-the-5-minute-ttl-never-fired-under-capacity-pressure) demonstrates directly, since `TTL-300s` was byte-identical to LRU-leaf in every run.
>
> **Both results can be entirely correct.** The claim here is narrower and it is this: *when capacity binds, it dominates the TTL, and the recompute it causes looks nothing like the idle-session story.* If you are provisioning cache capacity, that changes what you optimise. If you are reasoning about provider TTLs, the 31.5% figure is the relevant one, not this.

Reproduce: `make gap`

## 3\. The 5-minute TTL never fired under capacity pressure

[](https://github.com/gauravapiscean/agentic-kv-cache#3-the-5-minute-ttl-never-fired-under-capacity-pressure)

`TTL-300s` produced **byte-identical results to LRU-leaf in every single run.**

LRU always evicted before the timer expired, so the TTL never became the binding constraint at any cache size I tested. This is also the cleanest evidence that these runs sit in a capacity-bound regime rather than the TTL-bound one a provider cache operates in.

## 4\. Three ways to beat LRU, three failures

[](https://github.com/gauravapiscean/agentic-kv-cache#4-three-ways-to-beat-lru-three-failures)

I implemented a policy with three separable, independently ablatable components:

- **H** — hazard-based `P(session returns)` replacing recency. Online Bayesian estimator over observed inter-turn gaps and continuation rates. No oracle: it only ever sees completed observations.
- **C** — physically-modelled recompute cost. Prefill cost at position *i* is a linear term plus an attention term proportional to *i*, so recomputing the tail of a 100k-token chain is far more expensive per byte than it looks.
- **G** — coherent session-granularity eviction. Instead of taking the *N* globally-oldest leaves (which may truncate 50 different chains), sacrifice one session's private tail.

Hit rate, 40 AgentX sessions, 4,751 requests:

| cache (blocks) | LRU-leaf   | TTL-300s | LFU-leaf | +H     | +HC    | +HCG   |
| -------------- | ---------- | -------- | -------- | ------ | ------ | ------ |
| 8,000          | **83.48%** | 83.48%   | 63.61%   | 82.89% | 71.77% | 68.63% |
| 20,000         | **93.92%** | 93.92%   | 69.96%   | 93.61% | 84.86% | 78.89% |
| 50,000         | **95.76%** | 95.76%   | 79.58%   | 95.68% | 94.45% | 91.40% |

Effective recompute cost versus LRU-leaf (negative is worse):

| cache  | LFU-leaf | +H    | +HC    | +HCG    |
| ------ | -------- | ----- | ------ | ------- |
| 8,000  | −129.7%  | −3.2% | −38.9% | −81.0%  |
| 20,000 | −434.3%  | −4.5% | −90.4% | −207.8% |
| 50,000 | −447.1%  | −1.0% | −15.4% | −66.8%  |

Monotone negative. Every component made it worse, and the one I was most confident in — coherent eviction — was the worst.

Given [finding 2](https://github.com/gauravapiscean/agentic-kv-cache#2-the-waste-isnt-where-everyone-is-looking), this is exactly what should have happened. I was optimizing for a signal carrying 17.5% of the waste, using a predictor that can't discriminate at a 2.1-second median gap.

Reproduce: `make ablation`

## 5\. The harness bug that makes Belady lose to LRU

[](https://github.com/gauravapiscean/agentic-kv-cache#5-the-harness-bug-that-makes-belady-lose-to-lru)

In my first run, Belady — an *offline oracle* — lost to LRU. That's not a result, that's a broken harness, and it's worth publishing because I expect it to be common.

The cause: inserting a long chain into a near-full cache lets a policy **evict the very prefix it is currently building.** LRU is accidentally immune because just-inserted blocks have the newest timestamp. Every non-recency policy cannibalises itself. Real engines prevent this with refcount pins; a from-scratch simulator usually doesn't.

**If you build one of these, make your first test "does Belady beat LRU?" If it doesn't, you have this bug, and every policy comparison you run will be silently wrong in LRU's favour.**

Two other implementation notes:

- Only the *deepest* hit block can ever be a leaf, so `touch()` need only update that one block. An O(chain length) walk becomes O(1) — which matters at AgentX's 1,387-block median.
- Score eviction candidates by sampling *k* least-recently-used leaves rather than scanning the cache. This is what production caches do anyway, so it's realism, not a shortcut.

## What I think this means

[](https://github.com/gauravapiscean/agentic-kv-cache#what-i-think-this-means)

**Which constraint binds determines what you should optimise, and the two regimes want opposite things.** If your cache is TTL-bound, liveness prediction and retention policy are the levers, and the published eviction-cost work applies directly. If it's capacity-bound — which is where these runs sit — the question isn't "will this session come back?" but "how do I fit 88k-token working sets for N concurrent sessions in tight tool loops?" That points at compression, tiering, admission control and working-set-aware scheduling instead, and liveness prediction has essentially nothing to work with at a 2.1s median gap.

I went in assuming the liveness framing and it cost me three failed policies. Establishing which regime you're in first would have saved all of it.

**LRU-leaf is a stronger baseline than the literature treats it as.** I couldn't beat it with three independent mechanisms on real traces. Meanwhile several published alternatives are evaluated against degraded ports of their competitors — two separate papers benchmark against Continuum with its adaptive TTL replaced by a fixed 2s or 0.3s pin, which disables the thing that makes it work. This null result suggests those margins are softer than they read.

**Validate against a published curve before trusting your own numbers.** Doing that surfaced a discrepancy I still can't explain, and it's the only reason I trust anything else here.

## Limitations

[](https://github.com/gauravapiscean/agentic-kv-cache#limitations)

- **These runs are capacity-bound, not TTL-bound.** 40,000 blocks against a ~10.7M-token working set. A provider cache like Anthropic's is the opposite: per-customer capacity is effectively unlimited and entries die on a 5-minute timer. Findings 2 and 3 characterise the capacity-bound regime and say nothing about the TTL-bound one.
- **This is simulation.** It models cache policy faithfully and GPU execution not at all. Valid for "what should I keep in cache"; **not** valid for throughput, latency, or SLO attainment.
- **AgentX block hashes are session-local**, so they're namespaced per session. That models *zero* cross-session sharing — conservative, but it means shared system prompts across users are invisible here. Mooncake's hashes are global but its trace is one dense hour with no idle structure.
- **AgentX session arrival times are synthesised** (uniform over a window), because the trace stores session-relative timestamps only.
- **393 sessions and one hour of Mooncake is not the world.**
- **I am not claiming the liveness literature is wrong.** I'm claiming that in a capacity-bound cache the lever it targets has little to work with, and that establishing which regime you're in should come before choosing a policy.

## Reproduce it

[](https://github.com/gauravapiscean/agentic-kv-cache#reproduce-it)

```
git clone https://github.com/<you>/agentic-kv-cache && cd agentic-kv-cache
make setup      # venv
make data       # ~1.1 GB of traces (Apache-2.0), then flattens AgentX to a pickle
make repro      # all four experiments, writes results/
```

Individually:

```
make validate      # Mooncake reproduction        -> results/01_validate.txt
make characterize  # AgentX duty cycle and gaps   -> results/02_characterize.txt
make gap           # recompute by idle gap        -> results/03_gap.txt
make ablation      # policy ablation              -> results/04_ablation.txt
```

The simulator is pure stdlib Python; `numpy` is only used by helper scripts. Committed outputs in [`results/`](https://github.com/gauravapiscean/agentic-kv-cache/blob/main/results) let you check the tables without downloading anything.

## Open questions

[](https://github.com/gauravapiscean/agentic-kv-cache#open-questions)

If you can answer any of these, please open an issue — I'd genuinely like to know:

1. **Why the +4–6pp Mooncake offset?** Policy-free at infinite cache, so it should be explicable by metric definition alone, and five definitions don't close it.
2. **Is there a workload where liveness-aware eviction beats radix-leaf LRU?** Plausibly one with much longer median gaps than 2.1s — human-in-the-loop approval flows, perhaps.
3. **Does the 33%-from-sub-10-second-gaps result hold on other agentic traces?** If it does, a good chunk of this subfield is aimed at the wrong term.

## Credits

[](https://github.com/gauravapiscean/agentic-kv-cache#credits)

Traces: [Mooncake](https://github.com/kvcache-ai/Mooncake) (Moonshot AI, FAST'25) and the [AgentX corpus](https://huggingface.co/datasets/semianalysisai) (SemiAnalysis), both Apache-2.0. This work is independent of and unaffiliated with either.

MIT licensed.
