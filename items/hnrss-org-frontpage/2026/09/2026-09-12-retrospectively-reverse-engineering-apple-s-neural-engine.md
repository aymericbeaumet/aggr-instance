---
title: Retrospectively Reverse-Engineering Apple's Neural Engine
link: https://eiln.github.io/posts/ane.html
source: hnrss-org-frontpage
published: 2026-09-12T07:54:03Z
updated: 2026-09-12T07:54:03Z
first_seen: 2026-09-12T19:37:44.082475059Z
authors:
- zdw
summary: 'Article URL: https://eiln.github.io/posts/ane.html Comments URL: https://news.ycombinator.com/item?id=49670032 Points: 195 # Comments: 26'
content: extracted
html: 2026-09-12-retrospectively-reverse-engineering-apple-s-neural-engine.html
preview:
  file: 2026-09-12-retrospectively-reverse-engineering-apple-s-neural-engine.preview-cdd789642827.webp
  width: 201
  height: 256
  alt: ANE die layout
  color: '#536566'
images:
- source: https://eiln.github.io/posts/2026-08-10-ane-cnn/artifacts/ane-die.png
  original:
    file: 2026-09-12-retrospectively-reverse-engineering-apple-s-neural-engine.image-956211dcf56c.png
    width: 804
    height: 1022
  color: '#666668'
- source: https://eiln.github.io/posts/2026-08-10-ane-cnn/artifacts/tanh-comparison.png
  original:
    file: 2026-09-12-retrospectively-reverse-engineering-apple-s-neural-engine.image-080248428885.png
    width: 1476
    height: 1116
  variants:
  - file: 2026-09-12-retrospectively-reverse-engineering-apple-s-neural-engine.image-160044aba67f.webp
    width: 48
    height: 36
  - file: 2026-09-12-retrospectively-reverse-engineering-apple-s-neural-engine.image-f64952d1cd40.webp
    width: 320
    height: 242
  - file: 2026-09-12-retrospectively-reverse-engineering-apple-s-neural-engine.image-0acd95a382f0.webp
    width: 640
    height: 484
  - file: 2026-09-12-retrospectively-reverse-engineering-apple-s-neural-engine.image-9a0937f99412.webp
    width: 960
    height: 726
  - file: 2026-09-12-retrospectively-reverse-engineering-apple-s-neural-engine.image-697a2832ac76.webp
    width: 1476
    height: 1116
  color: '#fcfcfc'
- source: https://eiln.github.io/posts/2026-08-10-ane-cnn/artifacts/tanh-mode2-interpolation.png
  original:
    file: 2026-09-12-retrospectively-reverse-engineering-apple-s-neural-engine.image-767dc354d8c1.png
    width: 1476
    height: 1116
  variants:
  - file: 2026-09-12-retrospectively-reverse-engineering-apple-s-neural-engine.image-cde096dafdca.webp
    width: 48
    height: 36
  - file: 2026-09-12-retrospectively-reverse-engineering-apple-s-neural-engine.image-8b80d52b84e0.webp
    width: 320
    height: 242
  - file: 2026-09-12-retrospectively-reverse-engineering-apple-s-neural-engine.image-370e65de4630.webp
    width: 640
    height: 484
  - file: 2026-09-12-retrospectively-reverse-engineering-apple-s-neural-engine.image-f16da1e59c42.webp
    width: 960
    height: 726
  - file: 2026-09-12-retrospectively-reverse-engineering-apple-s-neural-engine.image-ada60fc25f56.webp
    width: 1476
    height: 1116
  color: '#fcfcfc'
- source: https://eiln.github.io/posts/2026-08-10-ane-cnn/artifacts/relu-weight-folding.png
  original:
    file: 2026-09-12-retrospectively-reverse-engineering-apple-s-neural-engine.image-18dfc8f068d0.png
    width: 1476
    height: 846
  variants:
  - file: 2026-09-12-retrospectively-reverse-engineering-apple-s-neural-engine.image-ffb013df3d71.webp
    width: 48
    height: 28
  - file: 2026-09-12-retrospectively-reverse-engineering-apple-s-neural-engine.image-23b9d58c67e3.webp
    width: 320
    height: 183
  - file: 2026-09-12-retrospectively-reverse-engineering-apple-s-neural-engine.image-a8cb3928f9d3.webp
    width: 640
    height: 367
  - file: 2026-09-12-retrospectively-reverse-engineering-apple-s-neural-engine.image-5673fbd1578d.webp
    width: 960
    height: 550
  - file: 2026-09-12-retrospectively-reverse-engineering-apple-s-neural-engine.image-4405962c1edc.webp
    width: 1280
    height: 734
  - file: 2026-09-12-retrospectively-reverse-engineering-apple-s-neural-engine.image-c0779471fb7e.webp
    width: 1476
    height: 846
  color: '#fcfcfc'
- source: https://eiln.github.io/posts/2026-08-10-ane-cnn/artifacts/convolution-equation.png
  original:
    file: 2026-09-12-retrospectively-reverse-engineering-apple-s-neural-engine.image-b5df48c7e77d.png
    width: 2582
    height: 1140
  color: '#fdfdfd'
- source: https://eiln.github.io/posts/2026-08-10-ane-cnn/artifacts/m3-memory-streaming-bandwidth.png
  original:
    file: 2026-09-12-retrospectively-reverse-engineering-apple-s-neural-engine.image-d633bb84ca5c.png
    width: 2400
    height: 1019
  variants:
  - file: 2026-09-12-retrospectively-reverse-engineering-apple-s-neural-engine.image-a44542f5bb2a.webp
    width: 48
    height: 20
  - file: 2026-09-12-retrospectively-reverse-engineering-apple-s-neural-engine.image-fe9596638239.webp
    width: 320
    height: 136
  - file: 2026-09-12-retrospectively-reverse-engineering-apple-s-neural-engine.image-8840b1dc5c9a.webp
    width: 640
    height: 272
  - file: 2026-09-12-retrospectively-reverse-engineering-apple-s-neural-engine.image-ac9ff042d8c8.webp
    width: 960
    height: 408
  - file: 2026-09-12-retrospectively-reverse-engineering-apple-s-neural-engine.image-de26698ca7a6.webp
    width: 1280
    height: 543
  - file: 2026-09-12-retrospectively-reverse-engineering-apple-s-neural-engine.image-f13fb319605c.webp
    width: 1600
    height: 679
  - file: 2026-09-12-retrospectively-reverse-engineering-apple-s-neural-engine.image-449e941a728f.webp
    width: 2400
    height: 1019
  color: '#fdfdfd'
- source: https://eiln.github.io/posts/2026-08-10-ane-cnn/artifacts/coreml-dma-throughput-sweep.png
  original:
    file: 2026-09-12-retrospectively-reverse-engineering-apple-s-neural-engine.image-3e4d3fd855b1.png
    width: 2600
    height: 1060
  variants:
  - file: 2026-09-12-retrospectively-reverse-engineering-apple-s-neural-engine.image-44e6a5b16f7d.webp
    width: 48
    height: 20
  - file: 2026-09-12-retrospectively-reverse-engineering-apple-s-neural-engine.image-e10c0499a14f.webp
    width: 320
    height: 130
  - file: 2026-09-12-retrospectively-reverse-engineering-apple-s-neural-engine.image-9e168e50c1e2.webp
    width: 640
    height: 261
  - file: 2026-09-12-retrospectively-reverse-engineering-apple-s-neural-engine.image-2856588c03f3.webp
    width: 960
    height: 391
  - file: 2026-09-12-retrospectively-reverse-engineering-apple-s-neural-engine.image-780210e81840.webp
    width: 1280
    height: 522
  - file: 2026-09-12-retrospectively-reverse-engineering-apple-s-neural-engine.image-c8b57c08e8b1.webp
    width: 1600
    height: 652
  - file: 2026-09-12-retrospectively-reverse-engineering-apple-s-neural-engine.image-1e930648005c.webp
    width: 2600
    height: 1060
  color: '#fbfcfc'
- source: https://eiln.github.io/posts/2026-08-10-ane-cnn/artifacts/kernel-tile-dma-additivity.png
  original:
    file: 2026-09-12-retrospectively-reverse-engineering-apple-s-neural-engine.image-d73214215a4c.png
    width: 1954
    height: 962
  color: '#fcfcfd'
---

Aug 10, 2026

(5089 words)

I stopped working on the [reverse-engineered Apple Neural Engine (ANE) driver](https://github.com/eiln/ane/tree/main) three years ago, upon a sad mini realization that the ANE block is just not that useful, and I could be doing more useful things, and moved onto upstreaming other, more useful, blocks. The ANE's architecture was too opinionated to build a general-purpose accelerator platform around it, and a linux driver effectively opening ANE hardware API access could not broaden the class of workloads it could do. Even macOS only regularly uses their own ANE to generate upsampled preview images in Finder.

[https://github.com/eiln/ane/tree/main](https://github.com/eiln/ane/tree/main)

![M1 die](https://eiln.github.io/posts/2026-08-10-ane-cnn/artifacts/m1-die.png) M1 die shot: https://mastodon.social/@dougall/115149886886125067

The M5 (2025)'s headline feature was "LLM performance", and they also conveniently folded the ANE cores inside the GPU cores — I knew it was coming, but it officially feels like the beginning of the end for the standalone NPU. So, in honor of the ANE’s apparent demise, we will do something even more useless: go back and reverse-engineer the ANE on the M1, finish what we started. It's been three years (fuck), and I should know more than I did when I first worked on this.

If the goal three years ago was to make the ANE useful by running ops on it; this time, it's more about mapping the full internal architecture — compute, datapath, scheduler, memory, and execution model — because those internal design decisions reveal the assumptions about ML workloads that Apple was willing to commit to silicon first in the A11 Bionic (2017), and what that says about the shift from CNN-era NPUs to today's GPUs running transformer workloads.

## 1\. Compute [](https://eiln.github.io/posts/ane.html#1-compute "Link to this section")

The 16 compute cores are probably the least interesting part of the ANE. Apple originally targeted dense image-processing CNN workloads, which consists of dense tensor reductions with predictable reuse. The M1 ANE compute core is a large parallel array of multiply-accumulate (MAC) units, but that alone says almost nothing about what workloads it was designed for and accels at.

![ANE die layout](https://eiln.github.io/posts/2026-08-10-ane-cnn/artifacts/ane-die.png)

A convolutional layer does a dot product between an activation window and learned kernel weights, and attention does a dot product between a query and key vector. A dot product is a dot product, and a MAC does just that. What specialized ANE to the 2017 CNN models is not the MAC, but dataflow surrounding the MACs: when and where MAC inputs and outputs enter, stay, move. The assumption that transformers broke, especially with autoregressive decode, was predictable reuse patterns, which the ANE exploited to architect a dataflow efficient enough to run on phones. The M5 decision confirms that ANE's compute core remained still useful for transformers, but inside a different dataflow.

Still, here's the datapath inside each of the 16 compute cores:

```text
┌────────────────────── core ─────────────────────┐
│ ┌───────── 256× MACs ─────────┐  ┌────────────┐ │
│ │ MAD ─► add ─► accumulator   │─►│ activation │ │
│ │        ▲          │         │  └────────────┘ │
│ │        └──────────┘         │                 │
│ └─────────────────────────────┘                 │
└─────────────────────────────────────────────────┘
```

* * *

### Multiply-Accumulate [](https://eiln.github.io/posts/ane.html#multiply-accumulate "Link to this section")

ANE has 16 parallel compute cores. Each compute core has 128 FP16 (or 256 INT8) parallel multiply-accumulate (MAC) lanes. Each MAC lane performs the recurrence:

\\\[ s\\leftarrow s+a\\times b \\\]

Multiply two operands \\(a\\) and \\(b\\), and then add the product to the running sum (accumulator).

Repeating the MAC operation over T cycles computes a T-term dot product:

\\\[ s\_T=s\_0 + \\sum\_{t=0}^{T-1} a\_t \\, b\_t. \\\]

A MAC lane thus performs a scalar reduction over *time*. A 16-core ANE has 2048 parallel MAC lanes,

\\\[ 128\\ \\text{lanes/core}\\times16\\ \\text{cores} = 2048\\ \\text{parallel MAC lanes} \\\]

So each cycle performs 2048 parallel reductions *spatially*, with time being the only reduction axis:

\\\[ S\_T\[q,p\] = S\_0\[q,p\] + \\sum\_{t=0}^{T-1} a\_t\[q,p\]\\,b\_t\[q\]. \\\]

An individual MAC lane does not know what dimension of the matrix or tensor it is reducing over. It's important to note that a dot product vs matrix multiplication vs convolution arises from how the operands are mapped and scheduled onto the core. The ANE core (with the exception of kernel memory, discussed later) does not encode a 4-channel CNN layer into the hardware.

* * *

Internally, the [MAC datapath](https://patents.google.com/patent/US20190340489A1/en) consists of a multiplier, adder, and a 32-bit accumulator register. Each cycle, the adder adds the fresh multiplier output with the previous sum, which then becomes the new running sum.

```text
operand a ──┐   ┌────────────┐   p[31:0]   ┌──────────────┐   s_next[31:0]  ┌─────────────┐
            ├──►│ MULTIPLIER │────────────►│ 32-BIT ADDER │────────────────►│ ACCUMULATOR │
operand b ──┘   └────────────┘             └──────▲───────┘                 └──────┬──────┘
                                                  │                                │ s[31:0]
                                                  └────────────────────────────────┘
```

This feedback path keeps the partial sum in memory local to the MAC lane, so it does not need fetched from an external memory far away, between MAC cycles.

Regarding resolution, it does fixed-point reduction with FP16 at readout. The multiplier is 16-bit, accumulated in a 32-bit register as Q16.16, then read out as FP16 via sign-extend and etc. Working in integer (hex) FP16 representation, to probe the accumulator range, build a CoreML ANE program that computes a dot product with a vector of all (1)s, so each multiplier results in a bounded v, but the running sum in the accumulator keeps growing:

\\\[ s=\\sum\_{i=0}^{255}v=256v. \\\]

| (v)        | CPU hex  | CPU value | ANE hex  | CoreML value |
| ---------- | -------- | --------- | -------- | ------------ |
| `127.9375` | `0x77ff` | `32752`   | `0x77ff` | `32752`      |
| `128`      | `0x7800` | `32768`   | `0x7c00` | `+∞`         |
| `−128`     | `0xf800` | `−32768`  | `0xf800` | `−32768`     |
| `−128.125` | `0xf801` | `−32800`  | `0xfc00` | `−∞`         |

Since 32768 is itself a valid FP16 word (0x7800), the ANE's 0x7c00 can't be FP16 output overflow, the clamp happens inside the accumulator, at \\(2^{15}\\). Thus the accumulator saturates at \\(2^{15}\\), exactly the range of a signed 32-bit fixed-point value with 16 fractional bits.

* * *

### Nonlinear Activation [](https://eiln.github.io/posts/ane.html#nonlinear-activation "Link to this section")

For a fused layer, the ANE computes:

\\\[ y = f(\\sum\_k x\_k w\_k + b) \\\]

Importantly, completed MAC sums feed directly into the post-MAC activation block, avoiding an intermediate memory round-trip. This is possible because the activation is pointwise: once a scalar reduction is complete, its activation depends only on that scalar and can be applied immediately.

To determine how the ANE implements `tanh()`, compile a CoreML model containing a single [`TANH` activation layer](https://apple.github.io/coremltools/source/coremltools.converters.mil.mil.ops.defs.html#module-coremltools.converters.mil.mil.ops.defs.iOS15.activation) and inspect the resulting compiled hardware register file (hwx). The coefficient region contains 33 consecutive FP16 words beginning at `0x4288`:

```
00004270: 3120 3001 0000 0000 0000 0000 0000 0000
00004280: 0000 0044 0000 003c 0000 f52f d633 bc35  # 0.000000 0.124329 0.244873 0.358398
00004290: 6537 7038 1539 a239 183a 793a c93a 0a3b  # 0.462158 0.554688 0.635254 0.704102 0.761719 0.809082 0.848145 0.879883
000042a0: 3e3b 673b 883b a23b b63b c63b d33b dd3b  # 0.905273 0.925293 0.941406 0.954102 0.963867 0.971680 0.978027 0.982910
000042b0: e53b eb3b ef3b f33b f63b f83b fa3b fb3b  # 0.986816 0.989746 0.991699 0.993652 0.995117 0.996094 0.997070 0.997559
000042c0: fc3b fd3b fe3b fe3b ff3b 0000 0000 0000  # 0.998047 0.998535 0.999023 0.999023 0.999512
000042d0: 003c 0300 6000 0000 0000 0000 0000 0000
```

Those 33 FP16 words match 33 IEEE LE FP16 quantized samples of \\(\\tanh(x)\\):

\\\[ T\_i=\\operatorname{round}\_{16}\\!\\left(\\tanh(i/8)\\right), \\qquad i=0,1,\\ldots,32. \\\]![Core ML tanh overlaid with double-precision tanh, followed by signed error](https://eiln.github.io/posts/2026-08-10-ane-cnn/artifacts/tanh-comparison.png)

Now switch to `RELU` activation layer:

| activation program | `NonlinearMode` | lookup coefficients |
| ------------------ | --------------- | ------------------- |
| identity           | 0               | none                |
| ReLU               | 1               | none                |
| tanh               | 2               | 33 FP16 words       |

Thus, mode 2 selects a custom 33-entry lookup table. 33 points defines 32 intervals. With \\(R=3\\), the knots are

\\\[ x\_i=\\frac{i}{8},\\qquad i=0,\\ldots,32, \\\]

covering \\(\[0,4\]\\) with spacing \\(1/8\\). The input maps into the table as \\(u=2^R|x|\\), so \\(R\\) sets the knot spacing. The resolution is smoother than its 33 bin; I suspect that adjacent entries are linearly interpolated. To test, build an impulse LUT with a single spike:

\\\[ T\_8=1,\\qquad T\_k=0\\ \\text{for }k\\ne8,\\qquad R=3. \\\]![One nonzero lookup-table entry produces two straight-line segments on the ANE](https://eiln.github.io/posts/2026-08-10-ane-cnn/artifacts/tanh-mode2-interpolation.png)

Then sweep the input across the two cells around \\(T\_8\\). The measured output forms a triangle: magnitude rises linearly from \\(0\\) at \\(|x|=7/8\\) to \\(1\\) at \\(|x|=1\\), then falls linearly to \\(0\\) at \\(|x|=9/8\\).

Thus, we know that mode 2 implements a 33-entry piecewise-linear LUT. \\(R\\) scales the input into LUT coordinates,

\\\[ u=2^R|x|, \\\]

so the knot spacing is \\(\\Delta x=2^{-R}\\). \\(\\lfloor u\\rfloor\\) and \\(\\lceil u\\rceil\\) select the adjacent entries, and \\(\\alpha=u-\\lfloor u\\rfloor\\) gives the interpolation weight between them.

* * *

### Scaling and Bias [](https://eiln.github.io/posts/ane.html#scaling-and-bias "Link to this section")

CoreML also supports a linear scaling and bias \\(ax + b\\) transform. I then suspected \\(ax + b\\) could share the linear interpolation hardware of mode 2. To confirm, construct a CoreML model with a ReLU with a constant scale and offset:

\\\[ z=4x-2,\\qquad y=\\operatorname{ReLU}\\left(\\frac{z}{2}+1\\right), \\\]

If the compiler folds the constant scale and offset into the convolution:

\\\[ W'=\\frac12W=2,\\qquad b'=\\frac12b+1=0, \\\]\\\[ y=\\operatorname{ReLU}(2x). \\\]

Decoding `model.espresso.weights` confirms exactly this folded transformation on ReLU:

```text
authored convolution:  W  = 4,  b  = -2
activation affine:     s  = 0.5, c  =  1
compiled convolution:  W' = 2,  b' =  0
```

![Core ML folds constant scale and offset into convolution weights and bias before ReLU](https://eiln.github.io/posts/2026-08-10-ane-cnn/artifacts/relu-weight-folding.png)

And the register file hexdiff shows how bias and activation are fused into the same post-MAC path at compile time:

| Probe                   | Tasks | BiasMode | PostScaleMode | NonlinearMode |
| ----------------------- | ----- | -------- | ------------- | ------------- |
| Plain convolution       | 1     | 0        | 0             | 0             |
| Explicit Core ML `Bias` | 1     | 1        | 0             | 0             |
| Bias + ReLU             | 1     | 1        | 0             | 1             |
| Bias + tanh             | 1     | 1        | 0             | 2             |

Extremely cursed idea: use nonlinear interpolation to compute an additional kernel pass, or quantize int8 into int4 weights.

## 2\. Scheduler [](https://eiln.github.io/posts/ane.html#2-scheduler "Link to this section")

The [ane driver source code](https://github.com/eiln/ane/blob/main/ane/src/ane_drv.c) is disappointingly boring. The driver never gives the ANE a `CONV`, `MATMUL`, or `RELU` opcode to run. All the neural operations have all already been compiled into a command stream of task descriptors (TDs), and the driver software simply loads the task to memory, sets the pointer to the opaque task blob via (TM\_ADDR, TM\_SIZE), and submits the staged task by ringing the doorbell (TM\_PUSH).

```c
static void ane_tm_push_tq(struct ane_device *ane, struct ane_request *req)
{
	int qid = req->qid;
	tm_write32(ane, TM_ADDR, tq_read32(ane, TQ_ADDR1(qid)));
	tm_write32(ane, TM_INFO, tq_read32(ane, TQ_SIZE1(qid)) | req->td_count);
	tm_write32(ane, TM_PUSH, TQ_PRTY_TABLE[qid] | (qid & 7) << 8); // magic
}
```

[https://github.com/eiln/ane/blob/main/ane/src/ane\_tm.c#L87](https://github.com/eiln/ane/blob/main/ane/src/ane_tm.c#L87)

The hardware then owns the submission until completion, and raises an interrupt to the ARM64 core when it's done.

```c
static void ane_tm_handle_irq(struct ane_device *ane)
{
	int line;

	line = 0;
	for (u32 n = 0; n < tm_read32(ane, TM_IRQ_EVTC(line)); n++) {
```

This (boring) command submission frontend resembles that of a GPU's, think NVIDIA's pushbuffer/PBDMA. The software submits a command stream resident in memory, and the GPU's command processor walks over command stream and dispatches the commands, without knowing what that command executes.

TM\_ADDR and TM\_INFO are global staging registers, and TM\_PUSH atomically commits that staged launch state, given that nothing happens until TM\_PUSH is written ("magic"). TM\_INFO in particular stores the total number of descriptors in the supplied stream:

```
TM_INFO[31:16] = descriptor_dwords - 1
TM_INFO[15:0]  = descriptor_count
```

TM\_INFO register naturally maps onto a hardware counter:

```verilog
if (fetch) begin
    if (word_ctr == descriptor_dwords_minus_1) begin
        word_ctr <= 0;
        desc_ctr <= desc_ctr + 1;
    end else begin
        word_ctr <= word_ctr + 1;
    end
end
```

Why the "minus 1"? Encoding `length - 1` is an RTL-friendly way to terminate a zero-based counter out of the critical path. But note how, compared to GPU commands which parse a variable-length stream of descriptors in a ringbuffer, ANE only receives the total count, indicating that descriptors are fixed-size.

### Task Queue [](https://eiln.github.io/posts/ane.html#task-queue "Link to this section")

Going one layer deeper, what's in a task queue (TQ) that the task manager selects from?

```
                    +------------------+
CPU / driver ------>|   Task Manager   |
                    |                  |
                    | schedule / fetch |
                    | / dispatch       |
                    +--------+---------+
                             |
          +------------------+------------------+
          |                  |                  |
          v                  v                  v
     +---------+        +---------+        +---------+
     | TQ 0    |  ...   | TQ 3    |  ...   | TQ 7    |
     | BAR[32] |        | BAR[32] |        | BAR[32] |
     | NID     |        | NID     |        | NID     |
     | state   |        | state   |        | state   |
     +---------+        +---------+        +---------+
```

There's 8 copies of the same register block (indexed by qid (0…7)), structured as:

```text
TQ[qid] + 0x000   STATUS
          0x010   PRIORITY
          0x014   VACANT
          0x01c   INFO

          0x020   BAR1[0..31] // task1
          0x0a0   NID1
          0x0a4   SIZE2
          0x0a8   ADDR2

          0x0ac   BAR2[0..31] // task2
          0x12c   NID2
          0x130   SIZE1
          0x134   ADDR1

next qid: +0x148
```

Each TQ holds:

- (1) Per-TQ scheduling state (status, priority, and vacancy)
- (2) Two sets of command stream descriptors, per-TQ (ADDR1/ADDR2, SIZE1/SIZE2, NID1/NID2, and 32 BARs). The two slots are certainly a ping-pong staging scheme to let one slot execute, while software modifies the other slot.

Notice how TM\_PUSH executes a task referenced in TM\_ADDR/TM\_SIZE by attaching a qid:

```c
	tm_write32(ane, TM_PUSH, TQ_PRTY_TABLE[qid] | (qid & 7) << 8); // magic
```

The natural interpretation is that the descriptor stream specifies what task to run, while the `qid` selects the launch context the descriptor runs under. The resident TQ context (BAR, NID) is much like a GPU hardware channel. Here's my driver populating a single TQ to launch it:

```c
int ane_tm_enqueue(struct ane_device *ane, struct ane_request *req)
{
	int qid = req->qid;

	tq_write32(ane, TQ_STATUS(qid), 0x1);

	for (int bdx = 0; bdx < ANE_TILE_COUNT; bdx++) {
		tq_write32(ane, TQ_BAR1(qid, bdx), req->bar[bdx]);
	}

	tq_write32(ane, TQ_SIZE1(qid), ((req->td_size >> 2) - 1) << 0x10);
	tq_write32(ane, TQ_ADDR1(qid), req->btsp_iova);
	tq_write32(ane, TQ_NID1(qid), (req->nid & 0xff) << 8 | 1);

	return 0;
}
```

[https://github.com/eiln/ane/blob/main/ane/src/ane\_tm.c#L70](https://github.com/eiln/ane/blob/main/ane/src/ane_tm.c#L70)

The only thing important here is the 32-entry BAR table (base address register). We'll get into task descriptors next, but the compiled ANE command stream only references virtual addresses by relative offsets, and BAR provides the base IOVA (IOMMU peripheral virtual address) relocation address. An ANE virtual address access needs a hard-coded BAR base offset supplied at compile time, meaning it lacks GPU-style load/store instructions that dynamically issue load/stores from virtual address.

* * *

### Task Descriptor [](https://eiln.github.io/posts/ane.html#task-descriptor "Link to this section")

The task manager walks over and executes chain of fixed-size task descriptors:

```verilog
for (int i = 0; i < td_count; i++)
    execute_task(td_block, i);
```

What's in each TD? Here's a hexdump of the TD for the simplest 1x1 convolution:

```
# M1 h13, 1x1 convolution: X[1,8,4,1] -> Y[1,3,4,1]
# TD header KernelDMASrc Common TileDMASrc L2 PE NE TileDMADst

00000000: 02000000 00000000 0000042a 00000000   # Header: EON=1 LogEvents=0x42a
00000010: 00fff86a 00000000 30009800 00000000   # Header: DebugEvents=0xfff86a SPL TSR TSE SrcLoc=1 DstLoc=1
00000020: 03025024 00000021 f401f800 00000040   # Header: RBase0=4 WBase=5 KBase0=1 ENE=3 KernelDMA: packet
00000030: 00000000 00000081 00000081 00000081   # KernelDMA.Config[0..2]: En=1 Hint=2
00000040: 00000080 00000080 00000080 00000080   # KernelDMA.Config[3..6]: En=0 Hint=2
00000050: 00000080 00000080 00000080 00000080   # KernelDMA.Config[7..10]: En=0 Hint=2
00000060: 00000080 00000080 00000080 00000080   # KernelDMA.Config[11..14]: En=0 Hint=2
00000070: 00000080 00000000 00000040 00000080   # KernelDMA.Config[15]: En=0 Hint=2; Base[0..2]=0,1,2
00000080: 00000000 00000000 00000000 00000000   # KernelDMA.Base[3..6]=0
00000090: 00000000 00000000 00000000 00000000   # KernelDMA.Base[7..10]=0
000000a0: 00000000 00000000 00000000 00000000   # KernelDMA.Base[11..14]=0
000000b0: 00000000 00000040 00000040 00000040   # KernelDMA.Base[15]=0; Size[0..2]=1
000000c0: 00000040 00000040 00000040 00000040   # KernelDMA.Size[3..6]=1
000000d0: 00000040 00000040 00000040 00000040   # KernelDMA.Size[7..10]=1
000000e0: 00000040 00000040 00000040 00000040   # KernelDMA.Size[11..14]=1
000000f0: 00000040 00000080 00000080 00000080   # KernelDMA.Size[15]=1
00000100: 00000080 00000000 00000000 00000000
00000110: 00000000 00000040 00000040 00000040
00000120: 00000040 3c000000 00040001 00000001   # Common: packet; Win=1 Hin=4
00000130: 00000022 00000008 00000003 00040001   # Common: InFmt=2 OutFmt=2 Cin=8 Cout=3 Wout=1 Hout=4
00000140: 00000001 5000a021 00002041 00010001   # Common.Conv: Kw=1 Kh=1 Sx=1 Sy=1 Groups=1
00000150: 00000004 00000000 00000000 04144405   # Common: tileH=4 ActiveNE=2 AccDB=1
00000160: 00100000 00000000 6c013800 00033881   # Common: NID=1 TileSrc: packet; enabled
00000170: 00008880 00000000 00000040 00000100   # TileSrc: base=0 row=1 plane=4
00000180: 00000800 00000800 00000000 00000000   # TileSrc: depth=32 group=32
00000190: 00000000 00000000 00000000 00000000
000001a0: 00000000 01002031 00000000 00000100   # TileSrc.Fmt: mode=1 trunc=3 mem=2 intlv=1
000001b0: 00000000 00000000 00000000 00000000
000001c0: 00000000 00000000 00000000 00000000   # TileSrc.PixelOffset[1..3]=0
000001d0: 00000000 00000000 00000000 44004800   # L2: packet
000001e0: 00000000 00500172 00000000 00000010   # L2.Source: base=0 channel=1
000001f0: 00000080 00000080 00000080 00000000   # L2.Source: row=8
00000200: 00000000 00000000 00000000 00000000
00000210: 0050017a 00000200 00000000 00000000   # L2.Result: base=0x20 channel=0 row=0
00000220: 00000000 00000000 0c008800 00000000   # PE: packet
00000230: 00000000 00000000 00000000 1000c800   # PE: zero NE: packet
00000240: 00000082 00101c00 00000000 00000000   # NE: KernelFmt=2 BinaryPoint=28
00000250: 00003c00 18017800 040000c1 00000000   # NE: PostScale=0x3c00 TileDst: packet; En=1 Base=0
00000260: 00000040 00000100 00000300 00000300   # TileDst: row=1 plane=4 depth=12 group=12
00000270: 01302031   # TileDst.Fmt: mode=1 trunc=3 mem=2 intlv=1 zpad
```

Important is that a TD is not an executable instruction stream. ANE has no ISA. TD is a sequence of "ControlDMA" (I made this name up) burst-write packets writes to the ANE's hardware configuration registers, such as input dimension, input/output address, activation function. Each ControlDMA packet consists of a 32-bit transfer word followed by N consecutive 32-bit register values:

```
31                    26 25                         2 1  0
+-----------------------+-----------------------------+----+
| register count minus 1| first register base index   | 00 |
+-----------------------+-----------------------------+----+
```

Notice the “minus 1” termination count again. ControlDMA is a flexible unidirectional DMA engine that copies N 32-bit words from IOMMU virtual DRAM into the ANE’s physical register space. For example, KernelDMASrc's packet header in TD is `0xf401f800`:

```verilog
count         = (0xf401f800 >> 26) + 1 = 62 words
register base = 0xf401f800 & 0x03fffffc = 0x1f800
```

This is not a LOAD\_WEIGHTS instruction. It's copying 0xf4 or 62 consecutive words into the KernelDMA register offset starting at 0x1f800. And those KernelDMA configuration values can tell KernelDMA where to load the weights from.

| Start byte | Section           | Information                                         |
| ---------- | ----------------- | --------------------------------------------------- |
| `0x000`    | Header            | Dependencies, chaining, and BAR selectors           |
| `0x028`    | `KernelDMASrc`    | `0xf401f800`; 16 coefficient-DMA lanes              |
| `0x124`    | `Common`          | `0x3c000000`; tensor and convolution geometry       |
| `0x168`    | `TileDMASrc`      | `0x6c013800`; activation-source DMA                 |
| `0x1dc`    | `L2`              | `0x44004800`; local source/result configuration     |
| `0x228`    | Processing engine | `0x0c008800`; PE configuration                      |
| `0x23c`    | Neural engine     | `0x1000c800`; MAC and post-processing configuration |
| `0x254`    | `TileDMADst`      | `0x18017800`; result-destination DMA                |
| `0x274`    | End               | 628 bytes total                                     |

Since each section writes to one MMIO register block, TD divides cleanly into ANE's datapath sections:

| Starting address | Size     | Block name           | What                                                                      |
| ---------------- | -------- | -------------------- | ------------------------------------------------------------------------- |
| `0x26bc00000`    | `0x4000` | Common               | Broadcast configuration selector; inferred                                |
| `0x26bc04000`    | `0x4000` | L2                   | L2 backing/register aperture                                              |
| `0x26bc08000`    | `0x4000` | PE                   | Processing-element configuration                                          |
| `0x26bc0c000`    | `0x4000` | NE / MAC             | Kernel format, MAC, bias, scaling, and nonlinear controls                 |
| `0x26bc10000`    | `0x3000` | Unknown              | Unidentified register bank                                                |
| `0x26bc13000`    | `0x4000` | Tile DMA source      | Input-tile addresses, strides, formats, and DMA controls                  |
| `0x26bc17000`    | `0x4000` | Tile DMA destination | Output-tile addresses, strides, formats, and DMA controls                 |
| `0x26bc1b000`    | `0x4000` | Unknown / tunables   | Unidentified configuration and tunable registers                          |
| `0x26bc1f000`    | `0x4000` | Kernel               | Kernel backing / kernel DMA-source aperture                               |
| `0x26bc23000`    | `0x1000` | Unknown              | Unidentified register bank                                                |
| `0x26bc24000`    | `0x1000` | Task Manager         | Task submission, execution state, events, and completion                  |
| `0x26bc25000`    | `0x1000` | Task Queues          | Eight queues containing TD stacks, NIDs, priorities, and request pointers |

A TD is effectively a serialized register-file dump of the ANE’s datapath registers. Each “ANE program” is simply the configuration for one pass through the datapath. We can configure how the fixed datapath operates (subject to the knobs it exposes), but not what operations the datapath is capable of performing, or how those operations are sequenced.

When the "magic" atomic word is written to task manager to execute a TD, roughly, the sequence of what happens:

1. ControlDMA copies TD into the configuration registers.
2. KernelDMA copies kernel W into kernel memory (KMem).
3. TileDMA copies input \\(X\\) from DRAM into L2.
4. Each MAC core reduces a row by its weights, producing one row of \\(Y\\).
5. Steps 2–3 repeat for all rows of \\(X\\).
6. Postprocessing is applied, and the completed results are stored in L2.
7. TileDMADst copies \\(Y\\) from L2 back to DRAM.

ANE is a fixed-function dataflow engine, not a GPU executing arbitrary instructions. The TD configures a domain-specific datapath. Constraining the hardware interface usually means smaller area, deterministic movement, lower latency, and less power drawn. ANE's compiler can explicitly schedule what the tensors do, but that also means the compiler must explicitly schedule what the tensors do. This is a tradeoff, but a justified one: we usually know what the model looks like at compile time. Dynamic execution is not what limits ANE. ANE's processor interface is relatively generic, and it simply launches tasks, and the tasks can describe transformers.

For example making tensor sizes fixed at compile time does not mean it can't handle variable-length tensors: for example, a growing KV cache can be traversed by looping over the size, and dispatch overhead is negligible relative to the elephant in the room here, that is, memory-streaming bandwidth. What actually shaped ANE for CNNs over transformers is memory movement.

## 3\. Memory [](https://eiln.github.io/posts/ane.html#3-memory "Link to this section")

### Roofline [](https://eiln.github.io/posts/ane.html#roofline "Link to this section")

It's always good to identify our current slowest link, so we can optimize what actually matters.

Apple’s unified memory lets the ANE access buffers from the system DRAM pool accessible by the CPU and GPU. It does not mean the ANE zero-copy streams directly out of that DRAM pool. ANE must first copy any memory into its local "ANE memory" or SRAM. Any bandwidth-limited task will thus be limited by ANE's local memory streaming throughput.

M1 ANE reports \\(11\\text{ TOP/s}\\) at \\(68\\text{ GB/s}\\) at system DRAM bandwidth. A MAC performs two operations but consumes two FP16 operands, or 4 bytes:

\\\[ \\frac{2\\text{ OP}}{4\\text{ bytes}} =0.5\\text{ OP/byte}. \\\]

If every MAC operand streamed from DRAM, sustaining \\(11\\text{ TOP/s}\\) would require streaming

\\\[ \\frac{11\\text{ TOP/s}}{0.5\\text{ OP/byte}} = 22\\text{ TB/s}. \\\]

which is over 300x times the reported \\(68\\text{ GB/s}\\) system DRAM capacity. Thus peak ANE MAC throughput could be reached by fetching from some local ANE memory reservoir, and reusing it.

Restated, the M1 ANE’s \\(11\\text{ TOP/s}\\) at \\(68\\text{ GB/s}\\) number sets the roofline ridge point:

\\\[ \\frac{11\\text{ TOP/s}}{68\\text{ GB/s}} = 162\\text{ OP/byte}. \\\]

Each byte fetched from DRAM must support, on average, at least 162 operations for DRAM bandwidth to stop being the limiter. Equivalently, the workload must provide enough on-chip reuse to achieve an arithmetic intensity of at least 162 OP/byte DRAM traffic. Below the 162:1 ratio, speeding up compute won't increase decoded token/s.

* * *

### Memory Hierarchy [](https://eiln.github.io/posts/ane.html#memory-hierarchy "Link to this section")

Even if (average) DRAM bandwidth were sufficient, ANE does not read DRAM directly for many reasons, including DRAM deterministic timing, physical routing, shared traffic, etc. If ANE's traffic competes on AXI the CPU, GPU, display, and etc, it cannot provide deterministic timing to the MACs. Also, if 16 cores consume some input tile, we do not want to initiate 16 identical DRAM transfers. "ANE local memory" would allow intermediate activation produced by one operation be consumed by the next instead of traveling to DRAM and back.

Apple had several ways to organize local memory hierarchy. The [multiply-accumulate patent](https://patents.google.com/patent/US20190340486A1/en) describes the the data buffer paths around the array.

```
                 Unified DRAM
                      │
                      ▼
┌───────────────────────────────────────────────┐
│      shared ANE L2 memory, 2 MiB              │
└────────┬───────────────┬───────────────────┬──┘
         │               │                   │
         ▼               ▼                   ▼
  ┌────────────┐   ┌────────────┐  ...  ┌────────────┐
  │   core 0   │   │   core 1   │       │   core N   │
  │ ┌────────┐ │   │ ┌────────┐ │       │ ┌────────┐ │
  │ │   L1   │ │   │ │   L1   │ │       │ │   L1   │ │
  │ └────────┘ │   │ └────────┘ │       │ └────────┘ │
  │ ┌────────┐ │   │ ┌────────┐ │       │ ┌────────┐ │
  │ │  KMem  │ │   │ │  KMem  │ │       │ │  KMem  │ │
  │ │ 64 KiB │ │   │ │ 64 KiB │ │       │ │ 64 KiB │ │
  │ └────────┘ │   │ └────────┘ │       │ └────────┘ │
  └────────────┘   └────────────┘       └────────────┘
```

- **KMem**: 16x per-core 64 KiB "L1" SRAM for kernel. Total 1 MiB.
- **L1**: 16x per-core MAC input "L1" staging area.
- **L2**: 1x shared 2 MiB L2 across all cores.

I'm not gonna pretend like I've never decompiled shit. The ANE ARM64 firmware's [task-debug routine](https://github.com/eiln/ane-notes/blob/055a4b4eee7298531147fbb9e5d5361e512b2cc6/regs/fw.c#L353-L487) (1) dumps 0x10000 bytes from KMem indices 0 through 15 (2) then dumps one separate 0x200000-byte L2 dump:

```c
_DAT_26bc30000 = 0; // core 0
uVar7 = 0;
do {
  *(undefined4 *)((long)pvVar2 + uVar7) = *(undefined4 *)(&DAT_26bc34000 + uVar7);
  bVar1 = uVar7 < 0xfffc;
  uVar7 = uVar7 + 4;
} while (bVar1);
CDebugUtility::fileWrite(this,pvVar2,0x10000,"./td_%d/kmem_%d-%d-%d_%d.bin"); // KMem #0 64 KiB

// ... repeat

_DAT_26bc30000 = 0xf; // core 15
uVar7 = 0;
do {
  *(undefined4 *)((long)pvVar2 + uVar7) = *(undefined4 *)(&DAT_26bc34000 + uVar7);
  bVar1 = uVar7 < 0xfffc;
  uVar7 = uVar7 + 4;
} while (bVar1);
CDebugUtility::fileWrite(this,pvVar2,0x10000,"./td_%d/kmem_%d-%d-%d_%d.bin"); // KMem #15 64 KiB

uVar7 = 0;
do {
  *(undefined4 *)((long)pvVar2 + uVar7) = *(undefined4 *)(&DAT_26bd00000 + uVar7);
  bVar1 = uVar7 < 0x1ffffc;
  uVar7 = uVar7 + 4;
} while (bVar1);
CDebugUtility::fileWrite(this,pvVar2,0x200000,"./td_%d/l2_%d-%d-%d.bin"); // L2 2 MiB
```

* * *

### DMA Engines [](https://eiln.github.io/posts/ane.html#dma-engines "Link to this section")

```
┌──────────┐       ┌────────────────────────────────────── ANE ──────────────────────────────────────┐
│          │       │                                                                                 │
│          │       │  ┌─────────────┐      ┌────────────────────┐                                    │
│          ├──────►│  │ Control DMA │─────►│ Hardware Registers │                                    │
│          │       │  └─────────────┘      └────────────────────┘                                    │
│          │       │                                                ┌─────────── MAC ────────────┐   │
│          │       │  ┌──────────────┐                              │  ┌──────────────────────┐  │   │
│          ├──────►│  │ KernelDmaSrc │─────────────────────────────►│  │    Kernel Memory     │  │   │
│   DRAM   │       │  └──────────────┘                              │  └──────────┬───────────┘  │   │
│          │       │                                                │             ▼              │   │
│          │       │  ┌──────────────┐       ┌────────────────┐     │  ┌──────────────────────┐  │   │
│          ├──────►│  │  TileDmaSrc  │──────►│       L2       │◄───►│  │      MAC Array       │  │   │
│          │       │  └──────────────┘       │  Tile Memory   │     │  └──────────────────────┘  │   │
│          │       │  ┌──────────────┐       │                │     └────────────────────────────┘   │
│          │◄──────┤  │  TileDmaDst  │◄──────│                │                                      │
│          │       │  └──────────────┘       └────────────────┘                                      │
│          │       │                                                                                 │
└──────────┘       └─────────────────────────────────────────────────────────────────────────────────┘
```

There are three DMA engines to/from the MACs:

- `KernelDMASrc[0..15]`: sixteen logical coefficient lanes copy weights from DRAM into the matching per-core KMem banks.
- `TileDMASrc`: copies tiles from DRAM to L2.
- `TileDMADst`: copies tiles from `L2` to DRAM.

The sixteen `KernelDMASrc` register lanes do not by themselves prove sixteen physically independent DMA front ends. A lane-scaling experiment does show that the logical lanes make concurrent progress: 32 valid tasks with 1 MiB aggregate coefficients per enabled lane take essentially the same time at one and sixteen lanes. They may still merge into shared request-generation, crossbar, cache, and DRAM arbitration.

The three tile/kernel DMA engines are chill. You supply it any src, dst, and size, and it will transfer that block of memory. However the existence of three DMA engines reveals some interesting assumptions:

- Kernel gets a dedicated path, at all. Splitting kernel vs tile is a commitment that kernel is a distinct operand class with a different lifecycle.
- KernelDMA is unidirectional load-only. They thought that weights would not be written back.
- TileDMA is bidirectional and shared. Intermediate tile outputs can be fed back without a round-trip to DRAM.
- Kernel memory is private per core (replicated 16x). They thought that weights would be reused many times per-core.
- New kernels must be loaded from DRAM, and not L2. They probably didn't think to stream large weights that exceeds 64 KiB x 16.

* * *

### Kernel L1 [](https://eiln.github.io/posts/ane.html#kernel-l1 "Link to this section")

A convolution is not a symmetric multiply-add of \\(A\\) and \\(B\\). A convolution slides the same kernel \\(w\[k\]\\) across the input:

\\\[ y\[p\]=\\sum\_k x\[p+k\]\\,w\[k\]. \\\]

Shoutout 2k2 textbook ![2k2 textbook](https://eiln.github.io/posts/2026-08-10-ane-cnn/artifacts/convolution-equation.png)

Notice how the two tap kernel \\(w\_0\\) and \\(w\_1\\) can be loaded once and then reused across the whole duration of the output:

```text
                 MAD 0      MAD 1
coefficient        w0         w1
                    ×          ×
shift 0:           a0         a1    → y0
shift 1:           a1         a2    → y1
shift 2:           a2         a3    → y2
```

So the kernel can reside in local memory, and reused, while new inputs shift in; which is what Apple's datapath patent describes ([https://patents.google.com/patent/US20190340491A1/en)](https://patents.google.com/patent/US20190340491A1/en\)).

The intended steady state is:

```
                    ┌── activation ───────┐
DRAM → L2 ──────────┤                     × → MAC
                    └── output ←──────────┘

DRAM → KMem ─────────────────── coefficient
```

If designing an ASIC to churn convolutions — where the kernel isn't something that's frequently and dynamically streamed in (ahem KV) — it's a no brainer to exploit the operand asymmetry and reduce kernel memory movement, because we've shown that the ANE is still deeply in the memory throughput-limited (162:1 ratio) region before the MACs can be saturated.

But why not allow L2 → KMem? It seems easier, even, to unify tile and kernel paths.

(1) So kernel traffic doesn't compete with tile for L2 bandwidth? I argue that L2 contention was not why Apple omitted the L2 -> KMem path: resident kmem exists at all because kernels were expected to be loaded infrequently. If kmem traffic is negligible in steady state, it could simply be given lower priority than tile L2 accesses.

(2) Because kernel L2 distribution adds complexity? Apple already distributes L2 endpoints to each core; I argue that a kernel path riding the same tile path is not that bad.

![ANE die layout](https://eiln.github.io/posts/2026-08-10-ane-cnn/artifacts/ane-die.png)

ANE's physical layout is centered around (literally) the center L2 SRAM rectangle, with (7+7) cores along each side of the rectangle, 2 cores on the top side, and shared control logic on the bottom side. The 7+7 side cores take L2 ingress horizontally from the cyan vertical trunk; but the two top cores need the same horizontal wide interface rotated and escaped vertically, which likely produces that conspicuous vertical comb in the top ingress.

Granted I am fully armchair engineering here, but adding the kmem L2 mux, I argue, really could not have been that bad. ANE decode performance would not have been as tanked if kernel fetches go back to DRAM. It makes me think that Apple simply never expected L2-resident tensors to become kernels, which was a valid assumption in 2017. And Apple also likes developing isolated modular modules, probably would've been easier to completely isolate development of the 1 MiB kmem (read-only, which would save a little area in SRAM routing) and 2 MiB tile L2.

## 4\. Is it over? [](https://eiln.github.io/posts/ane.html#4-is-it-over "Link to this section")

### DRAM Throughput [](https://eiln.github.io/posts/ane.html#dram-throughput "Link to this section")

> Is the GPU faster than the ANE?

Transformer [single-token decode](https://arxiv.org/abs/2406.15657) is the worst case for weight reuse and compute per memory ratio, because we need to stream the whole model’s worth of weights to generate one token. However, if both the ANE and GPU are read-bandwidth bound, whichever one that has higher read bandwidth will decode more token/s, regardless of peak compute capacity.

Now since ANE and GPU share the same DRAM, it's fair game: the ANE is not necessarily penalized in DRAM access compared to the GPU.

- If ANE is slower than GPU at single token decode, it's because its DMA controller cannot maintain enough parallel requests to saturate DQ.

To measure ANE vs GPU DRAM read throughput, generate a read-bandwidth-bound workload (buffers are much larger than the caches, filled with real pseudo-random data, and consumed once), and measure the execution time, and repeat for different read sizes:

\\\[ s=\\frac{\\text{change in measured execution time}} {\\text{change in payload size}} \\\]

The fitted slope answers: how much additional execution time does one extra DRAM read require? The reciprocal is the device's sustained DRAM read bandwidth.

![ANE and GPU DRAM read throughput](https://eiln.github.io/posts/2026-08-10-ane-cnn/artifacts/m3-memory-streaming-bandwidth.png)

- ANE KernelDMA: 37.99 GB/s: CoreML kernel size per execution time.
- ANE TileDMA: 59.08 GB/s: CoreML tile src size per execution time.
- GPU: 77.70 GB/s: Metal buffer reads per execution time, via a shader that reads every private uint4 once and writes a data-dependent checksum.

ANE's kernel (operand A) maxes out at 38 GB/s, and tile (operand B) at 60 GB/s. Can we issue 38 + 60 = 98 GB/s to hit M3's 100 GB/s DRAM ceiling?

![ANE execution time comparison](https://eiln.github.io/posts/2026-08-10-ane-cnn/artifacts/coreml-dma-throughput-sweep.png)

No. Experiment shows that the kernel+tile combined runtime matched the sum of the isolated runtimes. If the requests overlapped at all, then the shorter path would contribute little or no additional time, but execution time (not throughput) is strictly monotonic.

\\\[T\_{AB}=0.001+0.939T\_A+0.981T\_B\\\]![ANE kernel and tile DMA runtime comparison](https://eiln.github.io/posts/2026-08-10-ane-cnn/artifacts/kernel-tile-dma-additivity.png)

Thus, ANE's kernel and tile DMA requests are sent *serially* (one at a time), meaning ANE DRAM throughput is double-fucked:

- Both isolated kernel and tile DMA are lower than GPU's read GB/s.
- Kernel and tile DMA times are also additive.

* * *

### Unless…? [](https://eiln.github.io/posts/ane.html#unless "Link to this section")

With ANE decode pinned to the DRAM roofline, a drastic 2.5x improvement like 10 -> 25 tok/s can only come from ~2.5× higher memory streaming bandwidth.

> [Getting 50 GB/s Back Out of the ANE](https://eiln.github.io/posts/ane-dma.html)

But … what if we could add 50 GB/s of additional kernelDMA throughput ?
