---
title: 'vLLM V0 to V1: Correctness Before Corrections in RL'
link: https://huggingface.co/blog/ServiceNow-AI/correctness-before-corrections
source: huggingface-co-blog
published: 2026-05-06T19:06:55Z
updated: 2026-05-06T19:06:55Z
first_seen: 2026-09-19T21:30:23.395188495Z
content: extracted
html: 2026-05-06-vllm-v0-to-v1-correctness-before-corrections-in-rl.html
preview:
  file: 2026-05-06-vllm-v0-to-v1-correctness-before-corrections-in-rl.preview-5a99d49329dd.webp
  width: 256
  height: 138
  color: '#f3f3f3'
images:
- source: https://cdn-thumbnails.huggingface.co/social-thumbnails/blog/ServiceNow-AI/correctness-before-corrections.png
  original:
    file: 2026-05-06-vllm-v0-to-v1-correctness-before-corrections-in-rl.image-e7eddc08ce58.png
    width: 1200
    height: 648
  variants:
  - file: 2026-05-06-vllm-v0-to-v1-correctness-before-corrections-in-rl.image-857c06c261c2.webp
    width: 320
    height: 173
  - file: 2026-05-06-vllm-v0-to-v1-correctness-before-corrections-in-rl.image-1f56a72570f3.webp
    width: 640
    height: 346
  - file: 2026-05-06-vllm-v0-to-v1-correctness-before-corrections-in-rl.image-94df9986980e.webp
    width: 960
    height: 518
  - file: 2026-05-06-vllm-v0-to-v1-correctness-before-corrections-in-rl.image-dea783834db5.webp
    width: 1200
    height: 648
  color: '#fefefe'
- source: https://cdn-uploads.huggingface.co/production/uploads/61f750055596aa721ce68efe/2Q2pkNtiYPrlLVLoH51dO.png
  original:
    file: 2026-05-06-vllm-v0-to-v1-correctness-before-corrections-in-rl.image-627b3530b256.png
    width: 3600
    height: 919
  variants:
  - file: 2026-05-06-vllm-v0-to-v1-correctness-before-corrections-in-rl.image-da3f7dd6ce55.webp
    width: 320
    height: 82
  - file: 2026-05-06-vllm-v0-to-v1-correctness-before-corrections-in-rl.image-e3de002622b0.webp
    width: 640
    height: 163
  - file: 2026-05-06-vllm-v0-to-v1-correctness-before-corrections-in-rl.image-e25470dab9a7.webp
    width: 960
    height: 245
  - file: 2026-05-06-vllm-v0-to-v1-correctness-before-corrections-in-rl.image-9450ba85127b.webp
    width: 1280
    height: 327
  - file: 2026-05-06-vllm-v0-to-v1-correctness-before-corrections-in-rl.image-a63475fdbbc1.webp
    width: 1600
    height: 408
  - file: 2026-05-06-vllm-v0-to-v1-correctness-before-corrections-in-rl.image-edf92cfe80c8.webp
    width: 3600
    height: 919
  color: '#fcfcfc'
- source: https://cdn-uploads.huggingface.co/production/uploads/61f750055596aa721ce68efe/YhRfKIEKndf4LGG9g7Ck9.png
  original:
    file: 2026-05-06-vllm-v0-to-v1-correctness-before-corrections-in-rl.image-3f4941a3d6f2.png
    width: 2400
    height: 919
  variants:
  - file: 2026-05-06-vllm-v0-to-v1-correctness-before-corrections-in-rl.image-b20c6ab4dfc4.webp
    width: 320
    height: 123
  - file: 2026-05-06-vllm-v0-to-v1-correctness-before-corrections-in-rl.image-36410a443b41.webp
    width: 640
    height: 245
  - file: 2026-05-06-vllm-v0-to-v1-correctness-before-corrections-in-rl.image-6013d00f50ba.webp
    width: 960
    height: 368
  - file: 2026-05-06-vllm-v0-to-v1-correctness-before-corrections-in-rl.image-05f3efcc988b.webp
    width: 1280
    height: 490
  - file: 2026-05-06-vllm-v0-to-v1-correctness-before-corrections-in-rl.image-06031d4568c1.webp
    width: 1600
    height: 613
  - file: 2026-05-06-vllm-v0-to-v1-correctness-before-corrections-in-rl.image-0862a1cf658f.webp
    width: 2400
    height: 919
  color: '#fdfdfd'
- source: https://cdn-uploads.huggingface.co/production/uploads/61f750055596aa721ce68efe/6fQ4PVHFb_bbYMzducj1o.png
  original:
    file: 2026-05-06-vllm-v0-to-v1-correctness-before-corrections-in-rl.image-5f0ed27ef3d6.png
    width: 3600
    height: 919
  variants:
  - file: 2026-05-06-vllm-v0-to-v1-correctness-before-corrections-in-rl.image-7a7e8fc57de5.webp
    width: 320
    height: 82
  - file: 2026-05-06-vllm-v0-to-v1-correctness-before-corrections-in-rl.image-097ed44d1897.webp
    width: 640
    height: 163
  - file: 2026-05-06-vllm-v0-to-v1-correctness-before-corrections-in-rl.image-b994f4f424ec.webp
    width: 960
    height: 245
  - file: 2026-05-06-vllm-v0-to-v1-correctness-before-corrections-in-rl.image-15cf618d58a9.webp
    width: 1280
    height: 327
  - file: 2026-05-06-vllm-v0-to-v1-correctness-before-corrections-in-rl.image-6feeb3c9535a.webp
    width: 1600
    height: 408
  - file: 2026-05-06-vllm-v0-to-v1-correctness-before-corrections-in-rl.image-080cce2d0e68.webp
    width: 3600
    height: 919
  color: '#fcfcfc'
- source: https://cdn-uploads.huggingface.co/production/uploads/61f750055596aa721ce68efe/K0PQyxwunQUipAXv9Iy1n.png
  original:
    file: 2026-05-06-vllm-v0-to-v1-correctness-before-corrections-in-rl.image-d404bad498d1.png
    width: 1800
    height: 919
  variants:
  - file: 2026-05-06-vllm-v0-to-v1-correctness-before-corrections-in-rl.image-4964368cb951.webp
    width: 320
    height: 163
  - file: 2026-05-06-vllm-v0-to-v1-correctness-before-corrections-in-rl.image-1375e8d8f733.webp
    width: 640
    height: 327
  - file: 2026-05-06-vllm-v0-to-v1-correctness-before-corrections-in-rl.image-22a28b86a879.webp
    width: 960
    height: 490
  - file: 2026-05-06-vllm-v0-to-v1-correctness-before-corrections-in-rl.image-74f954308e40.webp
    width: 1280
    height: 654
  - file: 2026-05-06-vllm-v0-to-v1-correctness-before-corrections-in-rl.image-6465321a28f3.webp
    width: 1800
    height: 919
  color: '#fdfdfd'
- source: https://cdn-uploads.huggingface.co/production/uploads/680ba1729f7688275d2ce0f4/XWXONkgJtznG09PVLFCAl.png
  original:
    file: 2026-05-06-vllm-v0-to-v1-correctness-before-corrections-in-rl.image-634797769155.png
    width: 1800
    height: 919
  variants:
  - file: 2026-05-06-vllm-v0-to-v1-correctness-before-corrections-in-rl.image-4757fca71efb.webp
    width: 320
    height: 163
  - file: 2026-05-06-vllm-v0-to-v1-correctness-before-corrections-in-rl.image-1c6b2e341de6.webp
    width: 640
    height: 327
  - file: 2026-05-06-vllm-v0-to-v1-correctness-before-corrections-in-rl.image-115e0123ec0e.webp
    width: 960
    height: 490
  - file: 2026-05-06-vllm-v0-to-v1-correctness-before-corrections-in-rl.image-8fb6a433e2d9.webp
    width: 1280
    height: 654
  - file: 2026-05-06-vllm-v0-to-v1-correctness-before-corrections-in-rl.image-a95ee6bd53cc.webp
    width: 1800
    height: 919
  color: '#fdfdfd'
- source: https://cdn-uploads.huggingface.co/production/uploads/680ba1729f7688275d2ce0f4/oCuXYiTLSl6-jPJLBzWoU.png
  original:
    file: 2026-05-06-vllm-v0-to-v1-correctness-before-corrections-in-rl.image-9f16a0a259eb.png
    width: 1800
    height: 919
  variants:
  - file: 2026-05-06-vllm-v0-to-v1-correctness-before-corrections-in-rl.image-602372c57703.webp
    width: 320
    height: 163
  - file: 2026-05-06-vllm-v0-to-v1-correctness-before-corrections-in-rl.image-bf8085b2f7fa.webp
    width: 640
    height: 327
  - file: 2026-05-06-vllm-v0-to-v1-correctness-before-corrections-in-rl.image-5ed3f740143c.webp
    width: 960
    height: 490
  - file: 2026-05-06-vllm-v0-to-v1-correctness-before-corrections-in-rl.image-ac01ea5638d0.webp
    width: 1280
    height: 654
  - file: 2026-05-06-vllm-v0-to-v1-correctness-before-corrections-in-rl.image-b528ed0b5972.webp
    width: 1600
    height: 817
  - file: 2026-05-06-vllm-v0-to-v1-correctness-before-corrections-in-rl.image-9028b5bb5ec1.webp
    width: 1800
    height: 919
  color: '#fdfdfd'
---

[PipelineRL](https://huggingface.co/blog/ServiceNow-AI/github.com/ServiceNow/PipelineRL/) uses vLLM as the inference engine for rollout generation. The inference engine samples tokens and returns token logprobs; the trainer uses those logprobs to compute policy ratios, KL, clip rate, entropy, and reward. Any discrepancy in how those logprobs are computed can change the training dynamics. This is the train-inference mismatch we needed to eliminate during the vLLM V0 to V1 migration.

**TL;DR.** vLLM V1 matched our vLLM V0 reference after we fixed four things: processed rollout logprobs, V1-specific runtime defaults, the inflight weight-update path, and the fp32 `lm_head` used for the final projection. We fixed the backend behavior before changing the RL objective.

The reference run used vLLM `0.8.5`; the V1 runs used vLLM `0.18.1`. Figure 1 shows the final result. The red run is the initial V1 attempt, and the green run is the final V1 run after the fixes described below.

![](https://cdn-uploads.huggingface.co/production/uploads/61f750055596aa721ce68efe/2Q2pkNtiYPrlLVLoH51dO.png)

Figure 1. Trainer-side metrics for the vLLM V0 reference (blue), the initial vLLM V1 attempt (red), and the final vLLM V1 run after our fixes (green), including the fp32 `lm_head`. The final V1 run returns close to the V0 trajectory across clip rate, KL, entropy, and reward.

## [](https://huggingface.co/blog/ServiceNow-AI/correctness-before-corrections#migration-objective) Migration Objective

vLLM V1 is a substantial rewrite of the V0 engine. Our migration target was therefore deliberately narrow:

1. verify that V1 returned rollout logprobs in the form the trainer expected
2. rerun the same workload against the V0 reference
3. evaluate objective-level changes only after backend parity was restored

The first visible symptoms appeared in:

- `clamp_log_ratio_new_old_indicator`
- `kl_new_old`
- `entropy`
- `reward`

Those metrics came from a GSPO training run, the objective used for this experiment. The same class of mismatch can surface in PPO, GRPO, or any online RL system that treats rollout-side logprobs as part of the optimization target.

The initial V1 run showed the problem clearly. The trainer-side logprobs and reward moved away from the V0 reference early in training.

![](https://cdn-uploads.huggingface.co/production/uploads/61f750055596aa721ce68efe/YhRfKIEKndf4LGG9g7Ck9.png)

Figure 2. Current-policy logprobs computed by the trainer during updates (left) and reward (right). The initial vLLM V1 run (red) separates from the vLLM V0 reference (blue).

The same pattern appears in the trainer metrics. Clip rate is the easiest signal to read in the initial comparison.

![](https://cdn-uploads.huggingface.co/production/uploads/61f750055596aa721ce68efe/6fQ4PVHFb_bbYMzducj1o.png)

Figure 3. Trainer-side metrics for the vLLM V0 reference (blue) and the initial vLLM V1 attempt (red). Clip rate tracks the rollout/trainer policy gap; entropy and reward show how that gap propagates into training.

## [](https://huggingface.co/blog/ServiceNow-AI/correctness-before-corrections#failure-modes) Failure Modes

We separated the possible causes into three layers:

1. **Semantic mismatch**: the backend returns logprobs with different meaning relative to what the trainer expects.
2. **Inference-path mismatch**: the backend uses different runtime defaults for caching, scheduling, or request handling, so the same prompts follow a different execution path.
3. **Objective mismatch**: the RL objective needs correction for the amount of staleness or backend mismatch that remains.

We initially suspected the third category too early. The useful diagnosis came from treating the first two as backend behavior problems and ruling them out first.

## [](https://huggingface.co/blog/ServiceNow-AI/correctness-before-corrections#v1-backend-fixes) V1 Backend Fixes

### [](https://huggingface.co/blog/ServiceNow-AI/correctness-before-corrections#logprob-semantics) Logprob Semantics

The first issue was semantic. vLLM V1 returns logprobs from the raw model outputs by default, before logits post-processing such as temperature scaling, penalties, and top-k/top-p filtering. PipelineRL expected logprobs from the processed distribution used by the sampler.

The required setting was:

- `logprobs-mode=processed_logprobs`

This removed the obvious mean offset in rollout logprobs. The training curves still showed a gap relative to the known-good reference, so the next issue had to be in the inference path.

The policy-ratio plot shows this directly. Once `processed_logprobs` is on for V1, the mean policy ratio stays centered extremely close to `1.0` across all three runs. That establishes the mean-bias fix. The remaining mismatch shows up in clip rate, KL, entropy, and downstream training behavior.

![](https://cdn-uploads.huggingface.co/production/uploads/61f750055596aa721ce68efe/K0PQyxwunQUipAXv9Iy1n.png)

Figure 4. Per-step deviation of the rollout/trainer policy ratio from 1.0, scaled by 10,000, for the vLLM V0 reference (blue), the initial vLLM V1 run (red), and the corrected vLLM V1 run (green).

### [](https://huggingface.co/blog/ServiceNow-AI/correctness-before-corrections#runtime-defaults) Runtime Defaults

The early V1 run mixed the engine version with V1 runtime defaults:

- prefix caching, left unset in the early run so the vLLM `0.18.1` default applied
- async scheduling, left unset in the early run so the vLLM `0.18.1` default applied
- an ad-hoc `disable-cascade-attn` override that was set through launch-time kwarg passthrough and sits outside the parity recipe in committed config

For the parity run, we made these choices explicit:

```
vllm_config:
  use_v1: true
  vllm_kwargs:
    logprobs-mode: processed_logprobs
    enable-prefix-caching: false
    async-scheduling: false
```

Prefix caching deserves a separate note. It is normally a correctness-preserving inference optimization for a fixed model state. In this online RL setup, it was a V1-only difference in cache lifetime and reuse relative to the V0 reference path. The actor was also handling repeated prefixes, concurrent requests, async scheduling, and inflight weight updates.

A prefix-cache hit can reuse state computed before a weight update when the cache policy ignores the weight-update boundary. Disabling prefix caching removed one V1-only degree of freedom from the parity comparison.

### [](https://huggingface.co/blog/ServiceNow-AI/correctness-before-corrections#inflight-weight-updates) Inflight Weight Updates

Weight synchronization also had to match the online-RL update model. One option was to make V1 stricter than V0 by draining requests and clearing caches at every update. That would answer a separate question. We first needed to verify that V1 could match the existing V0 behavior.

What V0 effectively did was closer to:

- block execution at an engine boundary
- load the new weights
- resume without an explicit cached-state invalidation

The nearest V1 analogue was:

```
await engine.pause_generation(mode="keep", clear_cache=False)
await engine_client.collective_rpc_async(
    "receive_weight_update",
    args=(request.model_dump_json(),),
)
await engine.resume_generation()
```

Two details matter:

- `mode="keep"` matches the old inflight update model more closely than `wait` or `abort`
- `clear_cache=False` matches the V0 wrapper behavior, which left cached state intact on update

Lag was a useful runtime diagnostic. The initial V1 path carries more persistent lag later in training than the corrected V1 run.

![](https://cdn-uploads.huggingface.co/production/uploads/680ba1729f7688275d2ce0f4/XWXONkgJtznG09PVLFCAl.png)

Figure 5. Number of steps the weights in the rollout server are behind the trainer policy, for the vLLM V0 reference (blue), the initial vLLM V1 run (red), and the corrected vLLM V1 run (green).

## [](https://huggingface.co/blog/ServiceNow-AI/correctness-before-corrections#the-remaining-gap-fp32-lm_head) The Remaining Gap: fp32 lm\_head

The V1 backend fixes above removed the obvious migration issues, but final parity still required matching the numerical path used to compute logits. The trainer used an fp32 `lm_head` for the final projection. The rollout backend had to match that behavior.

A closely related issue appears in the [MiniMax-M1 technical report](https://arxiv.org/abs/2506.13585): their RL run showed a training/inference token-probability mismatch that they traced to the LM output head and fixed by computing the head in fp32.

This matters because the RL update consumes token logprobs directly. Small changes in logits can become visible in policy ratios, KL, and clipping. The final projection precision is therefore part of the correctness surface for online RL. The [ScaleRL paper](https://arxiv.org/abs/2510.13786) later includes fp32 logits/head computation as part of its RL recipe and ablates it as a useful design choice for large-scale RL.

With the fp32 `lm_head` path included, reward gives a compact view of the final parity result. In Figure 6, the final V1 run tracks the V0 reference; the initial V1 attempt produces a clearly different reward curve.

![](https://cdn-uploads.huggingface.co/production/uploads/680ba1729f7688275d2ce0f4/oCuXYiTLSl6-jPJLBzWoU.png)

Figure 6. Reward for the vLLM V0 reference (blue), the initial vLLM V1 attempt (red), and the final vLLM V1 run with the fp32 `lm_head` path (green). With the fp32 head included, the final V1 run tracks the V0 reference.

## [](https://huggingface.co/blog/ServiceNow-AI/correctness-before-corrections#ablations) Ablations

The negative results are important because they rule out common explanations.

- **`processed_logprobs` alone**: fixed the semantic logprob bug; the training mismatch remained.
- **Batch invariance**: the mismatch remained in a separate test, with higher lag, higher clip rate, and NCCL complications.
- **Treating the first V1 run as a fair baseline**: the first V1 run had multiple V1-only defaults enabled, so it was a confounded migration comparison.

## [](https://huggingface.co/blog/ServiceNow-AI/correctness-before-corrections#why-we-fixed-backend-correctness-first) Why We Fixed Backend Correctness First

Objective-side corrections such as truncated importance sampling, importance-ratio reweighting, and related methods are useful tools. If rollouts are intentionally stale, generated asynchronously, or produced by a backend where equivalence to the trainer-side policy is unavailable, then some form of correction is often the right thing to add.

The first problem here was inference correctness. After moving to V1, the rollout backend returned logprobs and runtime behavior that broke the trainer assumption. Adding an objective-side correction at that point would have mixed two questions:

- is the inference backend producing the right logprobs?
- given correct logprobs, does the objective still need an off-policy or async correction?

Those questions need to be separated. Otherwise an objective-side correction can compensate for broken inference-backend behavior, which makes the training curve harder to interpret.

The current objective can still improve. After inference parity is restored, the next improvement is the usual async/off-policy cleanup:

- keep explicit behavior-policy logprobs from rollout time
- recompute trainer-side old-policy logprobs at optimization time
- separate backend mismatch correction from the policy-update ratio
- track diagnostics like ESS for the correction term alongside aggregate trainer metrics

The main lesson from this migration is narrower: fix backend correctness first, then add corrections for the mismatch that remains.
