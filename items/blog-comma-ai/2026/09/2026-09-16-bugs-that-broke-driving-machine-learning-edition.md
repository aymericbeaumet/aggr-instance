---
title: 'Bugs that broke driving: Machine Learning edition'
link: https://blog.comma.ai/ml-bugs/
source: blog-comma-ai
published: 2026-09-16T20:00:00Z
updated: 2026-09-16T20:00:00Z
first_seen: 2026-09-19T21:30:23.395188495Z
authors:
- Yassine
summary: “We have solved self driving, we just need to fix the bugs.” Rumor has it that the Linux kernel averages around 0.5 bugs per 1,000 lines of code. Some bugs are easy to avoid or catch with tests and metrics. Others are very good at hiding. This is a sequel to Bugs that …
content: extracted
html: 2026-09-16-bugs-that-broke-driving-machine-learning-edition.html
preview:
  file: 2026-09-16-bugs-that-broke-driving-machine-learning-edition.preview-b6841429a539.webp
  width: 256
  height: 139
  color: '#9fa26f'
images:
- source: https://blog.comma.ai/img/bug_stories_ml/dit_bugs_grid.jpg
  original:
    file: 2026-09-16-bugs-that-broke-driving-machine-learning-edition.image-e77faba824f1.jpg
    width: 3072
    height: 1672
  color: '#e7e6e6'
- source: https://blog.comma.ai/img/bug_stories_ml/output_precision.png
  original:
    file: 2026-09-16-bugs-that-broke-driving-machine-learning-edition.image-abed12e78c04.png
    width: 1200
    height: 525
  variants:
  - file: 2026-09-16-bugs-that-broke-driving-machine-learning-edition.image-a04b298ecce2.webp
    width: 320
    height: 140
  - file: 2026-09-16-bugs-that-broke-driving-machine-learning-edition.image-7bdc29ef9346.webp
    width: 640
    height: 280
  - file: 2026-09-16-bugs-that-broke-driving-machine-learning-edition.image-d9288438953a.webp
    width: 960
    height: 420
  - file: 2026-09-16-bugs-that-broke-driving-machine-learning-edition.image-09170bd3fe1e.webp
    width: 1200
    height: 525
  color: '#fdfdfd'
- source: https://blog.comma.ai/img/bug_stories_ml/ddp_worldmodel_diffusion_loss.png
  original:
    file: 2026-09-16-bugs-that-broke-driving-machine-learning-edition.image-b5f3bb181111.png
    width: 1176
    height: 790
  color: '#fdfdfd'
- source: https://blog.comma.ai/img/bug_stories_ml/ddp_worldmodel_plan_loss.png
  original:
    file: 2026-09-16-bugs-that-broke-driving-machine-learning-edition.image-2269d95f0de2.png
    width: 1176
    height: 790
  variants:
  - file: 2026-09-16-bugs-that-broke-driving-machine-learning-edition.image-4ea448781234.webp
    width: 320
    height: 215
  - file: 2026-09-16-bugs-that-broke-driving-machine-learning-edition.image-16faa5c60e02.webp
    width: 640
    height: 430
  - file: 2026-09-16-bugs-that-broke-driving-machine-learning-edition.image-42bb671650b4.webp
    width: 1176
    height: 790
  color: '#fdfdfd'
- source: https://blog.comma.ai/img/bug_stories_ml/allnorm_loss.png
  original:
    file: 2026-09-16-bugs-that-broke-driving-machine-learning-edition.image-5398048280f6.png
    width: 1176
    height: 790
  color: '#fdfdfd'
---

“We have solved self driving, we just need to fix the bugs.”

Rumor has it that the Linux kernel averages around 0.5 bugs per 1,000 lines of code. Some bugs are easy to avoid or catch with tests and metrics. Others are very good at hiding.

This is a sequel to [Bugs that broke driving](https://blog.comma.ai/driving-bugs/) with more Machine Learning related bugs.

Some of these bugs affected correctness, others slowed things down or used up extra memory. They all made training or inference worse.

Looking back at these bugs helps us understand where our code tends to break and how to catch problems earlier.

## Output layers need to be run in high precision [Permalink](https://blog.comma.ai/ml-bugs/#output-layers-need-to-be-run-in-high-precision "Permalink")

*September 2026.*

The world model is a diffusion transformer (DiT) which predicts the next frame and an “expert” driving plan.

During inference, the model runs in mixed precision `bf16_fp8_nvfp4`:

- Transformer MLP linears use NVFP4 (most FLOPs)
- Attention projections, cached attention Q/K/V, and the KV cache use FP8 (saves VRAM)
- Other layers use BF16 (not too many FLOPs)
- Norm reductions, attention scores, and sinusoidal timestep features use FP32 (standard safe mixed precision)

In particular, the plan output layers ran in BF16. Around 30 m/s, BF16 can only represent speeds in steps of 0.125 m/s. The acceleration calculation from the plans subtracts nearby speed predictions and divides by a short time interval , magnifying the rounding errors:

[![Acceleration from BF16 and FP32 plan outputs](https://blog.comma.ai/img/bug_stories_ml/output_precision.png)](https://blog.comma.ai/img/bug_stories_ml/output_precision.png "Open image at full size")

The fix is to promote the plan head and its inputs to FP32 *before* running it, Casting the finished plan to FP32 just gives the staircase more decimal places.

```diff
+plan_head.float()
-plan = plan_head(features).float()
+plan = plan_head(features.float())
```

## ConvNeXt FP16 [Permalink](https://blog.comma.ai/ml-bugs/#convnext-fp16 "Permalink")

*August 2026.*

The driving model’s vision encoder is a ConvNeXt, with a learned multiplier on each residual branch: `x + gamma * f(x)`. At runtime, it runs in FP16.

The [pretrained checkpoint we used](https://huggingface.co/timm/convnext_xxlarge.clip_laion2b_soup) we pick had a value of almost 54 at `stages[2].blocks[29]`. After fine-tuning, some inputs push the residual past FP16’s 65,504 limit leading to `inf`.

The annoying thing is that this only triggered in some seemingly random cases, and didn’t happen in the various checks we run during model exporting to FP16.

The workaround was to divide that stage’s residual stream by four and carry the scale through its biases, multipliers, and LayerNorm epsilons:

```diff
 stage = encoder.stages[2]
+stage.downsample[1].weight.div_(4)
+stage.downsample[1].bias.div_(4)
+for block in stage.blocks:
+    block.conv_dw.bias.div_(4)
+    block.gamma.div_(4)
+    block.norm.eps /= 16
+encoder.stages[3].downsample[0].eps /= 16
```

## Missing desire targets [Permalink](https://blog.comma.ai/ml-bugs/#missing-desire-targets "Permalink")

*July 2026.*

The driving model predicts “driving desire”, like turning or changing lanes.

For a while, some training data had no desire targets: we use NaNs to distinguish missing targets, and our loss helpers normally handle this by masking those targets out. But the desire loss took an `argmax` first, and did not have the NaN handling.

```python
>>> import torch
>>> import torch.nn.functional as F
>>> target = torch.full((1, 8), torch.nan)
>>> target.argmax(-1)
tensor([0])
>>> F.cross_entropy(torch.zeros(1, 8), target.argmax(-1))
tensor(2.0794)
```

All those NaNs became class `0`, “no desire”, with a perfectly normal loss.

The fix replaces NaNs with zeros while keeping a mask, then uses that mask to ignore missing targets:

```python
def num_from_nan(target):
    valid = ~torch.isnan(target)
    return target.masked_fill(~valid, 0).detach(), valid
```

```diff
+target, valid = num_from_nan(target)
 loss = F.cross_entropy(logits, target.argmax(-1), reduction="none")
+loss = valid.all(-1) * loss
```

## Redundant biases drifted apart across distributed ranks [Permalink](https://blog.comma.ai/ml-bugs/#redundant-biases-drifted-apart-across-distributed-ranks "Permalink")

*April 2026.*

The driving model has many `nn.Linear` layers. During training, we use `torch.compile`, BF16 autocast and Distributed Data Parallel (DDP) as “acceleration” recipes.

Sometimes, harmless redundant biases are written in the architecture:

```python
a = nn.Linear(8, 8192)
b = nn.Linear(4, 8192)
y = a(x) + b(x[:, :4])
```

Note that the 8/4 is just illustrative, and keeps the weight gradients different while the bias gradients are identical.

`a.bias` and `b.bias` will be merged by the compiler into one bias, so they get the same gradient. The compiler went one step further: it made both `.grad` tensors share the same memory.

DDP synchronized them correctly, then `clip_grad_norm_` used its foreach implementation to scale that shared buffer from two CUDA blocks at once.

Depending on timing, an entry could be multiplied by the clipping coefficient once or twice, giving each rank a different update.

In one recorded run, rank 0 clipped all 8,192 entries once; rank 1 clipped 384 of them twice. The drift can reach a magnitude of 0.0056 between ranks after only 50 SGD steps, while the weight matrices stayed identical.

Both biases receive the same faulty update within a rank, their sum drifts too, which can change the model’s outputs. DDP synchronizes gradients during training, so it doesn’t repair those parameter differences: the GPUs are now training different copies of the model.

We worked around it by removing the redundant biases.

This was fixed in [PyTorch 2.13.0](https://github.com/pytorch/pytorch/blob/v2.13.0/torch/_inductor/fx_passes/joint_graph.py#L801-L804) as a side effect of a [compiler optimization change](https://github.com/pytorch/pytorch/commit/6f94c6e16e1bf562f8f5514953cf9b8d47d07cdd).

It preserved `FP32 → BF16 → FP32` cast chains so smaller intermediate tensors could still be materialized: in our backward graph, that happened to keep the two bias gradients in separate FP32 buffers.

Note: The foreach kernel itself still assumes the gradient buffers don’t overlap…

## DDP random seed bug [Permalink](https://blog.comma.ai/ml-bugs/#ddp-random-seed-bug "Permalink")

*August 2026.*

[Torchtitan](https://github.com/pytorch/torchtitan) uses the same seed across DDP (replicate), this is correct because weight initialization should match across replicate ranks. But torchtitan then keeps that rank identical across during for training.

With ordinary tensors (not DTensors), different ranks could get identical dropout masks and diffusion noise.

The fix is to reseed each data-parallel rank after initialization (essentially):

```diff
 torch.manual_seed(seed)
 model.init_weights()
+torch.manual_seed(seed + dp_rank)
 train(model)
```

Flux [does this after initialization](https://github.com/pytorch/torchtitan/blob/68c97b0c54b0d853fefefab4ecf9e41e6a82a6b7/torchtitan/models/flux/trainer.py#L38-L50), setting `distinct_seed_mesh_dims=["cp", "dp_shard", "dp_replicate"]`.

[![Diffusion loss: reference run and adjusted causal run](https://blog.comma.ai/img/bug_stories_ml/ddp_worldmodel_diffusion_loss.png)](https://blog.comma.ai/img/bug_stories_ml/ddp_worldmodel_diffusion_loss.png "Open diffusion loss plot at full size") [![Plan loss: reference run and adjusted causal run](https://blog.comma.ai/img/bug_stories_ml/ddp_worldmodel_plan_loss.png)](https://blog.comma.ai/img/bug_stories_ml/ddp_worldmodel_plan_loss.png "Open plan loss plot at full size")

Fixing the ddp seed bug produces better world models overall and less noisy loss curves given that the diffusion noise samples are not the same across all ranks.

## AllNorm [Permalink](https://blog.comma.ai/ml-bugs/#allnorm "Permalink")

*June 2021 to April 2026.*

[![BatchNorm and AllNorm training and validation loss](https://blog.comma.ai/img/bug_stories_ml/allnorm_loss.png)](https://blog.comma.ai/img/bug_stories_ml/allnorm_loss.png "Open loss plot at full size")

Training and validation loss for models trained with BatchNorm and AllNorm.

Normalization layers are widely used in Machine Learning. BatchNorm normalizes each channel using the mean and variance across the batch and spatial dimensions. During training, it uses the current batch statistics and updates running averages. During inference, it uses those running averages.

The driving models are multi-task learners, with some tasks being more sparse than others, for example: hard brake events.

One failure mode we encountered was when a channel specializes in a rare feature. Through batches without that feature, the channel stays quiet and its BatchNorm running variance shrinks.

When validation activates that channel again, it gets divided by a stored standard deviation that is too small, blowing up the outputs. Training can still look fine because its statistics come from the current batch: when the feature returns, the denominator grows with it.

LayerNorm fixes this, but needs per input statistics at inference. On device, we want normalization folded into convolutions. Recalibrating BatchNorm running statistics offline also fixes this, but adds overhead and another training pass. Same for lowering momentum which needed extra tuning.

The best fix we came up with was pooling the whole batch, channels, and spatial dimensions into one set of statistics. We called this AllNorm.

```python
class AllNorm(nn.Module):
    def __init__(self):
        super().__init__()
        self.bn = nn.BatchNorm1d(1)

    def forward(self, x):
        return self.bn(x.reshape(-1, 1)).reshape_as(x)
```

All channels share statistics, making it robust to this failure, and keeping the ability to fold the statistics into the convolutions.

Note: With the extra compute that the [chestnut](https://blog.comma.ai/chestnut/) gives, we can now afford using LayerNorm and not deal with this!

## Gigashuffle’s reusable CPU buffers [Permalink](https://blog.comma.ai/ml-bugs/#gigashuffles-reusable-cpu-buffers "Permalink")

*July 2026.*

[Gigashuffle](https://github.com/commaai/gigashuffle) returns tensors backed by reusable CPU buffers. Each reader allocates these once and fills them again for every batch:

```python
# Once per reader:
reader_buffer[i][key] = torch.empty(shape, dtype=dtype).share_memory_()

# For every batch:
torch.index_select(shuffle_buffer[i][key], 0, indices,
                   out=reader_buffer[i][key])
```

Normally, each batch is copied to the GPU before the next read. Our gradient accumulation loop collected several microbatches first, so later reads overwrote batches already in the list.

One way to fix this is to clone both inputs and targets before fetching the next microbatch:

```diff
 for _ in range(grad_accum):
     inputs, targets = next(loader)
+    inputs = {k: v.clone() for k, v in inputs.items()}
+    targets = {k: v.clone() for k, v in targets.items()}
     microbatches.append((inputs, targets))
```

## FSDP2 init bugs [Permalink](https://blog.comma.ai/ml-bugs/#fsdp2-init-bugs "Permalink")

*April 2025.*

With FSDP2, we construct the model on `meta`, shard it, then use `to_empty()` to allocate the weights. Initialization has to run after allocation: not defining an init function for a parameter initializes it to the arbitrary memory value it received during allocation.

```python
>>> import torch
>>> with torch.device("meta"):
...     model = torch.nn.Module()
...     model.weight = torch.nn.Parameter(torch.ones(4))
>>> _ = model.to_empty(device="cpu")
>>> model.weight.detach()
tensor([ 3.7189e-38,  0.0000e+00, -6.3894e-37,  4.5160e-41])
```

No clear fix here but to be paranoid about not forgetting init methods!

## NCCL timeout increase didn’t do anything [Permalink](https://blog.comma.ai/ml-bugs/#nccl-timeout-increase-didnt-do-anything "Permalink")

*July 2026.*

Our RL training runs collect rollouts from the cluster. If the cluster is busy, some ranks can wait for data longer than PyTorch’s default NCCL timeout of [10 minutes](https://docs.pytorch.org/docs/2.14/distributed.html#torch.distributed.init_process_group).

To work around this, we added a barrier with a timeout of one hour before each training step, thinking we have fixed it. But that timeout was passed around, only to never be used:

```python
opts = BarrierOptions()
opts.timeout = timeout
opts.asyncOp = async_op
work = group.barrier(opts=opts)
```

The C++ dispatcher passed it along, but NCCL implemented the barrier as an all-reduce and only forwarded `asyncOp`:

```cpp
// C++ dispatcher
BarrierOptions opts;
opts.timeout = std::chrono::milliseconds(timeout);
opts.asyncOp = asyncOp;
backend->barrier(opts);

// ProcessGroupNCCL::barrier
AllreduceOptions arOpts;
arOpts.asyncOp = opts.asyncOp;
auto work = allreduce_impl(barrierTensor, "nccl:all_reduce_barrier", arOpts);
// opts.timeout goes nowhere.
```

That all-reduce got its deadline from a different options object: the process group’s configuration. The watchdog checked that deadline; our `barrier(timeout=...)` argument never entered the calculation:

```cpp
// Creating the collective's work: options_ belongs to the process group.
assignTimeoutToWork(work, options_);

// Inside assignTimeoutToWork(work, option), omitting timeout extensions:
work->opTimeout_ = option->timeout;

// Watchdog calls work.checkTimeout() with no timeout override:
if (timeElapsed >= opTimeout_) {
  // Report a collective timeout.
}
```

The fix was to give the wait for data its own Gloo group, with the timeout set on that group:

```diff
+# Once at startup, on every rank:
+ready = dist.new_group(backend="gloo", timeout=timedelta(hours=1))

 # Before each training step:
-dist.barrier(timeout=timedelta(hours=1))
+dist.barrier(group=ready)
```

## Memory leak with AdamW’s foreach updates on DTensors [Permalink](https://blog.comma.ai/ml-bugs/#memory-leak-with-adamws-foreach-updates-on-dtensors "Permalink")

*March 2026.*

AdamW batches updates into calls like `torch._foreach_mul_(params, 1 - lr * weight_decay)`. FSDP2 exposes its parameters as DTensors, so those calls go through DTensor’s dispatcher to work out how the operation applies to the shards. For foreach’s tensor lists, the dispatcher built a fresh `OpSchema` on every call, then constructed a cache key from the tensors’ shape and sharding metadata.

In the C++ key builder, `PyTuple_Pack` increments its arguments’ reference counts, but `.release()` stopped the C++ wrapper from decrementing its own reference afterward. That left an extra reference to the argument tuple on every call, so the tuples and their metadata kept piling up in CPU RAM.

The [fix](https://github.com/pytorch/pytorch/pull/176010) was to simply not release:

```diff
 // torch/csrc/autograd/python_variable.cpp
 // Inside DTensor_OpSchema_recompute_comparison_key_impl (simplified):
 py::tuple args_to_hash_tup(args_to_hash.size());
 // ... fill the tuple with argument metadata ...
 comparison_key = PyTuple_Pack(
     2, op,
-    args_to_hash_tup.release().ptr());
+    args_to_hash_tup.ptr());
```

The bug affected PyTorch 2.10.0 through 2.12.1; the same fix shipped via [a later PR](https://github.com/pytorch/pytorch/pull/181792) in 2.13.0.

## CUDA cache allocation [Permalink](https://blog.comma.ai/ml-bugs/#cuda-cache-allocation "Permalink")

*February 2026.*

When doing inference in our distributed cluster, we use an inference server with dynamic batching: [triton inference server](https://github.com/triton-inference-server). We usually benchmark at the largest batch size in isolation, and assume this will be the max VRAM the model will use. But dynamic batching reserved more VRAM, and we didn’t know why.

We traced it to how PyTorch caches freed GPU allocations, so the order of batch sizes matters. With its default native allocator, this takes just one tensor at a time:

```python
import torch

def reserved_after(batch_sizes):
    torch.cuda.empty_cache()  # Reset between experiments.
    for bs in batch_sizes:
        x = torch.empty(bs, 1024, 1024, device="cuda", dtype=torch.float32)
        del x
    return torch.cuda.memory_reserved() // 2**20

print(reserved_after([3, 4]))     # 28 MiB
print(reserved_after([4, 3, 4]))  # 16 MiB
```

Batch size 3 leaves a cached 12 MiB block, batch size 4 needs 16 MiB, so the allocator keeps both. Starting with 4 lets batch 3 use 12 MiB of the larger 16 MiB block.

We flipped the warmup order to start with the largest batch:

```diff
-for bs in range(1, max_batch_size + 1):
+for bs in range(max_batch_size, 0, -1):
     warmup(bs)
```

## CUDA graphs don’t capture Adam’s CPU work [Permalink](https://blog.comma.ai/ml-bugs/#cuda-graphs-dont-capture-adams-cpu-work "Permalink")

*June 2022.*

When training with CUDA graphs, we used to capture the whole training step, optimizer step included. CUDA graphs replay only GPU work, so CPU code inside the capture runs just once!

By default, PyTorch’s eager Adam increments its step counter and computes its bias corrections, `1 - beta1**step` and `1 - beta2**step`, on the CPU:

```python
# torch/optim/adam.py — Adam._init_group() (simplified)
state = self.state[p]
if len(state) == 0:  # Initialize this parameter's optimizer state once.
    state["step"] = (
        torch.zeros((), device=p.device)
        if group["capturable"] or group["fused"]
        else torch.tensor(0.0, device="cpu") # cpu?!?!
    )
```

Back then, replay kept updating the GPU moment estimates and weights with those frozen corrections, silently reusing the value of `step` from the capture-time value. We avoided this by running the optimizer outside the graph.

PyTorch 1.12.0 later added `Adam(..., capturable=True)`, putting the step counter and bias-correction math on the GPU so replay advances them too. Today, manual capture with `capturable=False` [raises an error](https://github.com/pytorch/pytorch/blob/v2.14.0/torch/optim/optimizer.py#L440-L449).

## Looking back [Permalink](https://blog.comma.ai/ml-bugs/#looking-back "Permalink")

The recurring themes were precision, leaked or reused memory, and distributed training. Some bugs were hard to spot but preventable with better code practices. Others became obvious once we visualized model outputs during inference or inspected model state during training.

Come fix the bugs that are still hiding ([https://comma.ai/jobs)](https://comma.ai/jobs\)).

Yassine.
