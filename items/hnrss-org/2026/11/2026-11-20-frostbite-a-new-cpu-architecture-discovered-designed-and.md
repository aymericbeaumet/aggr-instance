---
title: 'Frostbite: A new CPU architecture discovered, designed and verified by AI'
link: https://architectlabs.com/blog/frostbite
source: hnrss-org
published: 2026-11-20T10:01:41Z
updated: 2026-11-20T10:01:41Z
first_seen: 2026-11-20T16:02:15.998933044Z
authors:
- shioyama
content: extracted
html: 2026-11-20-frostbite-a-new-cpu-architecture-discovered-designed-and.html
remote_preview:
  url: https://main--gleeful-begonia-6bb5f3.netlify.app/_next/static/media/frostbite-cover.3n9ytugnquhlr.jpg
  alt: 'Introducing Frostbite: A new CPU architecture discovered, designed, and verified end-to-end by AI'
---

## Abstract

In our previous post, [Redwood](https://architectlabs.com/blog/redwood), we showed that given an architecture, our AI systems could implement, verify, and deploy an inference accelerator in record time. However, similar to other scientific domains, we believe that AI systems, given the correct freedom and guardrails, can co-design software and hardware together, making optimizations across the stack and opening a design space so large it was previously inaccessible. We set out to answer the question: “Could our AI systems & tools lead to a novel hardware discovery that radically outperforms human-made designs?”

Today, we're introducing Frostbite, a general-purpose CPU with a new instruction set built from the ground up. Frostbite exploits the properties of interaction nets, namely that every step is local, every dependency is explicit, and every value has exactly one owner, to reach a step-function gain in CPU performance that has long stagnated. On a set of 25 initially chosen workloads, including 15 held out until the final evaluation, we achieved a 4.59x performance-per-watt multiple when compared to a modern, silicon-proven superscalar (dual-issue, in-order) CPU core.

Our AI systems autonomously iterated over the compiler, ISA, architecture, microarchitecture, RTL, verification, and final GDS. When we took a closer look at the discovered architectural primitives, we were surprised to find novel hardware implementations and runtime features that would have otherwise been difficult to architect. In spite of stagnation in CPU core improvement, we were able to take a workload concept all the way to a final GDS, and prove a step-function increase in perf/W and perf/area. We believe that as AI systems become more capable, every class of chip we know today, from datacenter-grade CPUs, to AI accelerators, to networking switches and NICs, will go through similar fundamental architectural shifts.

## Introduction to interaction nets

Mainstream programming languages, when compiled for general-purpose CPUs, hide the dependencies between the parts of a program. Any piece of code may read or write any variable through a shared, mutable state, and so modern CPU hardware assumes the worst case and rediscovers, at runtime, which instructions actually depend on each other. Moreover, single-core CPU performance has largely stalled since the early 2000s, pushing people towards large multicore CPUs such as Intel Xeon, AMD Zen, and NVIDIA Vera. This forces CPUs to maintain the illusion that every core sees the same memory, and keeping that illusion consistent and performant requires deep cache hierarchies and coherence protocols that cost area and power, and scale poorly.

Interaction nets[^1] take a different approach. A program is a graph of nodes, and computation happens by rewriting pairs of nodes that are connected through their ports. Every rewrite only touches a small, local part of the graph. Because each node has exactly one principal port, no two of these active pairs ever overlap, so they can be rewritten in any order, or all at once, and the result is always the same. Interaction nets are also linear: every value is used exactly once, so there is no shared mutable state for the hardware to guard. Languages such as Vine[^2]compile ordinary-looking programs directly to interaction nets. For an excellent introduction to interaction nets and why they suit hardware, we highly recommend Tendrils' “On Interaction Nets.” [^3]

Fig. 1. Five queens, two ways. On the left, a C++ program walks the search tree one step at a time. On the right, the same search as an interaction net explores many branches at once: 3,183 interactions with a critical path of only 262 steps.

Fig. 1 shows the difference on a small search problem, placing five queens on a five-by-five board. The sequential program visits one candidate at a time. The interaction net exposes all of its independent work: on average, about twelve interactions are ready to run at every step. Nothing about the program was rewritten to get this parallelism; the dependencies were simply never thrown away.

In theory, this should have two large advantages. The first is multithreading for free: rather than varying levels of coarse and fine-grained parallelism, parallelism comes straight from the program. No threads, locks, or race conditions, and none of the out-of-order machinery a conventional core uses to dig it back out. The second is that, since every value has one owner and every step is local, data can live in small local memories next to where it is used. That removes much of the area and power a conventional chip spends on large caching subsystems, and much of the system-level complexity of keeping those caches coherent.

## Process

Frostbite was designed with zero architectural priors. We followed the process below, riding multiple agent swarms along the way.

![Timeline of the Frostbite program showing key events, stages from architecture exploration through alignment and iteration, and the agent worktrees created in each stage.](https://architectlabs.com/_next/static/media/process-timeline.35kiqsqt77ga9.png)

Fig. 2. The Frostbite program from first proposal to ablations: key events, the stages of work, and the worktrees the agents created, with each branch drawn in its phase's color.

### Architecture exploration and selection

To begin architectural exploration, we had to establish the set of workloads Frostbite should do exceedingly well in. We decided to look at different ways a program could expose its work: many independent pieces at once, rounds of parallel work, long sequential chains, data-dependent branches, and recursion. Under each category, we created a search set and a held-out eval set to evaluate the final Frostbite on, ensuring the design generalized to programs it was never tuned on. The kernels for FFT, radixsort, and LU follow the implementation in the [SPLASH-2](https://github.com/liuyix/splash2_benchmark) benchmark, although 64 times smaller, because at regular sizes they are not suitable for exploratory work.

Later, we added five more practical workloads to the held-out set: a Kalman filter, PCA, finite element analysis, an LU solve, and LZ4 decompression.

![Table of 25 workloads in five groups, each split into search set and eval set. Parallel: gemm, chacha20 and spmv in the search set; conv2d, radixsort and pca in the eval set. Staged: fft and binomial in the search set; lcs, bfs, emeans, lu solve and fea in the eval set. Sequential: biquad and sha256 in the search set; huffman and kalman in the eval set. Branching: json in the search set; randomforest and lz4 in the eval set. Recursive: mergesort and nqueens in the search set; quicksort, kdtree and fib in the eval set.](https://architectlabs.com/_next/static/media/workload-suite.0d2l3bzeh4gp9.png)

Fig. 3. All 25 workloads, grouped by how each program exposes its work, and split into the search set the agents tuned on and the held-out eval set, with the size each was run at.

Agents synthesized research papers and existing benchmarks, and built Python models to trade off high-level architectural ideas against each other, before narrowing to a single architectural concept. The output of this exploration was a 141-page PDF covering the proposed programming model, the runtime, and the high-level blocks of the chip. This was nowhere near the final architecture, but rather a starting point where all the prior findings, high-level requirements, constraints, experiments, and runtime traits were described. Once the first architectural iteration completed (first RTL generation), this was deleted, since many of the numbers and estimates were well off what was physically possible.

### Formal contracts and system modeling

Once the architecture candidate was selected, our system moved on to building the full software model of the chip. Building software models is essential when exploring an architectural space, as they allow fast iteration between the workloads, the ISA, and the abstracted view of the hardware. Whether we build a CPU, GPU, NPU, or networking switch, mastering this iteration loop allows the fastest exploration of the design space within a finite time. To do this repeatedly, we designed a new in-house specification language, ASL, and wrote Frostbite's system model “Arcmodel” in it. ASL supports machine-checkable translations and refinements between RTL, system models, UVM, SVA, and more, all against a formally backed specification. Its power and performance estimates are accurate to within 1–5%, it runs roughly 200x faster than RTL simulation, and it is formally verifiable through “contracts.”

In every silicon program to date, human architects have written many English PDFs before handing off to an implementation team and a verification team. However, natural language cannot capture every detail an implementation needs precisely and unambiguously, and it cannot guarantee that every agent hallucination is caught while the ASIC is being designed and verified. Natural language was never built for this, and in a world where AI writes the specification, we opt for a formal, machine-checkable specification language instead.

ASL can formally describe both high-level, protocol-like behavior, such as out-of-order handling, CSR programming, memory ordering, and coherency, and low-level details, such as pipelines, arbitration mechanisms, and compute transfer functions. This lets the model leave the specification open-ended where that is useful and constrain it exactly where a precise behavior is required. ASL also translates natively to Lean,[^4] which we use for all theorem proving when checking AI outputs across RTL, UVM, C, SVA, and more. We will discuss ASL in more detail in a future post.

Throughout the entire development of Frostbite, our AI iterates on the Frostbite ISA, Arcmodel, Design, and Verification tightly together, maintaining the ASL Contract as the source of truth. We've found that having a single, provable source of truth prevents a large class of bugs, silly mistakes, and reward-hacking instances when deploying large agent swarms. Ultimately, this common language allows the models to work across the stack nimbly, reflecting the changes propagated from the workload down to the final hardware. By the end, our Frostbite ASL lowered down to 800k+ lines of Lean.

![A web of 31 clusters, one per block of the chip, each made of the signals, events, state and clauses its Lean contract names, linked by blue curves where blocks share signals; below, the frame manager's contract drawn out, with its signals on the left feeding events, state and 49 clauses on the right.](https://architectlabs.com/_next/static/media/lean-web.2cbgtumsblhy-.png)

Fig. 4. The Lean contracts of all 31 blocks. Each block is a cluster of what its contract names, joined by what reads what and, in blue, by the signals blocks share. Below, we zoom into the Frame Manager (FRM) block.

### RTL and alignment

Because ASL is a formally backed specification, designing and verifying the RTL derived from it is the fastest part of the entire Frostbite process. Arcmodel, however, is not physically aware, so its power and performance estimates at first diverge from the true numbers of the timing-converged, synthesis-clean RTL, in the worst case by almost 250%.

Before continuing the ASL autoresearch, one of our systems recalibrates Arcmodel against the RTL. This is a fully autonomous pipeline. It first identifies the power hotspots in the RTL and traces them down to key counters in the formal specification, such as SRAM reads and writes, operation counts, flits sent and received, credit returns, and frame misses. The model may then write any scalar Python function over those counters to predict power, and it repeats this process until the RTL and Arcmodel agree to within 5%.

We find this style of non-gradient learning extremely powerful. Once this first physical alignment has been applied, later iterations are much faster. These are, of course, still pre-silicon power numbers, and over multiple tapeout loops, ASL models can be correlated to true post-silicon, production power numbers.

![Chart of model energy versus RTL energy prediction error over time, showing the Python model at 40–130% error and Arcmodel falling from 250% worst-case error after the first power fit to under 6% worst-case and about 1.5% median after searching.](https://architectlabs.com/_next/static/media/model-alignment.1w2vgrexnx2a5.png)

Fig. 5. Error between modeled energy and RTL energy over the program. The Python models stayed far from the RTL; after the first power fit, Arcmodel was realigned and searched until its error fell into the 1–5% band.

By aligning the Arcmodel and RTL so closely, the continued swarms were able to explore and exploit much faster. The full Arcmodel chip simulations are much faster than the RTL (660x faster), which led to many cross-stack optimizations. For instance, our AI systems discovered very fine-grained clock gating tricks, and adjusted the software runtime accordingly to accommodate them in a matter of hours. Such a cross-stack optimization is near-impossible to execute on in a traditional ASIC flow.

### Contracts, Arcmodel, RTL, and power, iterated together

Once Arcmodel and the RTL agreed, the original Python models and the architecture PDF were deleted, and the ASL contracts became the only specification of the chip. From then on, contracts, Arcmodel, RTL, and power moved forward together. Swarms of agents proposed architecture revisions in parallel worktrees; each change was checked against the contracts, modeled in Arcmodel, implemented in RTL, and measured for power, and then either kept or dropped. Across four searches and a final round of ablations, the swarm tried 58 ideas and kept 37 of them, from clock gates on a running tile to smaller L3 tables and one shared expander.

Fig. 6. From the first release to the end of the program, drawn out: each search's ideas, the worktrees they became, and the agents working on them, with every idea kept or dropped.

## Architectural novelty

The architecture in full detail would warrant its own post. Instead, we choose to focus here on the features that we considered either novel or creative and highlight the capability of our systems to push the frontiers of hardware design.

Programs for Frostbite are written in Vine,[^2] which is a fully fledged programming language with syntax similar to Rust. A Vine program is compiled to *Ivy*, an intermediate representation (IR) that encodes the interaction net structure. The optimizer also tries to apply as many of the graph reduction rules as it can, so the final *Ivy net*, as it is called, has a (mostly) fixed shape, composed of three primitives: data, computation nodes, and control nodes. However, the runtime of this net is still *very* dynamic, because unlike the typical computation graph, where the parallelism is encoded in the structure and pipelined over time, the parallelism has to be derived at runtime from the *state* and pipelined *spatially*. The initial naïve implementations were slow, so our agents had to get creative.

The generated ISA follows from this set of constraints. A lot of it is expected: wide bundles, hardware loop counters, and streams. We found the dynamic features more interesting, particularly operations to take, peek, and copy values from other parts of the net that would be running in parallel. The tiles then come alive, communicating across space.

Fig. 7. Code-to-hardware on Frostbite. The first call populates the chip, and then the calls it makes follow it, reusing the same nodes in a recursive fashion, until every tile is firing at once. The recursion is spread across space.

### Emergent property: stealing work

Because the parallelism is spread over space, there is no central scheduler. Instead, we saw the agents design these elegant rules for propagating the work. A tile follows three rules: it runs its own work first, meaning work in its input ports. If those are empty, it takes work from a neighboring tile that has work to spare. If no such tile is found, it can just go inactive and save power.

The agents demonstrated remarkable sophistication in how they implemented this. Below, we have the hardware for a single Frostbite tile:

![Block diagram of one Frostbite tile: the front end, the engine, the memory and the system blocks, with the runtime (frame manager, scheduler, work queue, outlet) and the network interface highlighted, and the router, static switch, quad divider and configuration bus outside the tile.](https://architectlabs.com/_next/static/media/tile-blocks-plain.1gnqiesx7j0kk.png)

Fig. 8. A Frostbite tile. The bounding boxes demonstrate how to think about the domains of the blocks.

A lot of them should be familiar. We are interested in the RUNTIME domain that is responsible for the work-stealing behavior described above.

The compiler breaks code into short sections that run uninterrupted once their inputs are ready. These are run in the **engine** and are mostly organized like traditional computation through the **front end**. Whenever a loop or branch splits into parallel pieces whose sizes aren't known ahead of time, the running section parks whatever comes after into a small record in local memory, the **frame manager**. It then drops the new tasks onto a hardware queue called the **work queue** and picks up the newest one itself. Idle neighbors are then free to pull the remaining tasks over the network. As tasks finish anywhere on the chip in any order, their return packets tick down the parked record's counter and automatically wake the next section the instant the last piece arrives.

This is what it looks like in hardware:

Fig. 9. How tiles share a workload.

We can think of each “task” as the current board, one level down, with one more queen. Tile 0 has to queue three tasks for all the possible queen configurations before it can finish the present board state. It's free, so it takes the newest task (from itself) and runs it. Meanwhile, its neighbors have gotten the other ones, except for Tile 5, which goes back to sleep.

This behavior was never specified by any human. We can directly trace it back to necessity on specific regressions and hill-climbing performance metrics.

## Results

We synthesized Frostbite on a 7 nm process, reaching 800 MHz timing at synthesis and 500 MHz in the final GDS. Each Frostbite quad (a two-by-two grid of tiles) paired with four L3 nodes is 0.792 mm². We benchmark one Quad+L3 cluster against a single core of a modern superscalar (dual-issue, in-order) CPU, which we refer to as TCPU (“Traditional CPU”). This TCPU is 0.895 mm² at the same process.

![Layout of the Frostbite chip, a four-by-four array of identical tiles with memory macros in pink. The top-left two-by-two tiles are outlined and labelled Frostbite quad, and one block on the left edge is outlined and labelled L3 node.](https://architectlabs.com/_next/static/media/die-shot-quad.2t9948bw50w5a.jpg)

Fig. 10. Frostbite layout, four by four tiles, on a 7 nm process, with one Frostbite quad (two by two tiles) and one L3 node outlined.

We benchmarked on a wide set of workloads, from dense, parallel kernels such as gemm and conv2d, to irregular, branching programs such as JSON parsing, Huffman coding, k-d trees, and recursive Fibonacci. A search set of workloads drove all architectural optimization, ISA design, RTL, and verification, and the chip was evaluated on both the 10 search-set workloads and the 15 held out, including the five practical workloads added later (Fig. 3).

For a fair comparison, we chose to first measure “single-core” performance by benchmarking one Frostbite quad against the TCPU, since they have similar area. Across all the workloads, we average an 8.77x performance-per-area, and across all 25 workloads, search set and held out together, Frostbite reaches a geometric mean of 4.59x the TCPU's performance-per-watt: 6.02x on the 10 search-set workloads, and 3.83x on the 15 held-out workloads it was never tuned on. As expected, the architecture does most strongly on highly parallelizable tasks, but it also outperforms the TCPU on branching, dynamic code.

![Bar chart of the Frostbite quad's performance-per-watt relative to the TCPU across 25 workloads, search set in blue and the held-out eval set in light blue, from 23.12x on conv2d (eval set) down to 1.15x on fib (eval set), with dashed lines at the geometric means: 3.83x on the eval set, 4.59x across all 25, and 6.02x on the search set.](https://architectlabs.com/_next/static/media/perf-per-watt-heldout.26hhprjnn8kyi.png)

Fig. 11. Performance-per-watt of one Frostbite quad relative to the TCPU's best build, across 25 workloads: the 10 in the search set, and the 15 in the held-out eval set. Dashed lines mark the geometric mean of the eval set, of all 25, and of the search set.

![Stacked bars of energy share: TCPU 57% memory, 27% fetch, decode and issue, 13% computing; Frostbite quad 38% memory, 6% fetch, decode and issue, 16% computing, 26% on-chip network and scheduler, 14% other; four-by-four Frostbite 40% memory, 7% fetch, decode and issue, 18% computing, 23% on-chip network and scheduler, 11% other.](https://architectlabs.com/_next/static/media/energy-breakdown.1vhkm_oxxzwwv.png)

Fig. 12. Share of each chip's energy by function, excluding clock distribution, for the TCPU, one Frostbite quad, and the full four-by-four Frostbite. Mean over 20 workloads.

The energy breakdown shows where the savings come from. Leaving out clock distribution (49% of TCPU energy, 41% of a Frostbite quad's), the TCPU spends 57% of its energy on memory and 27% on fetching, decoding, and issuing instructions. Frostbite cuts memory to 38% and fetch, decode, and issue to just 6%. In exchange, it spends 26% on its on-chip network and scheduler, the cost of sharing work and moving messages between tiles. What's more exciting, however, is that as we scale the chip to a four-by-four array (four quads), the power distribution is relatively the same. In a general-purpose multi-core CPU arrangement, scaling the number of cores requires adding heavy coherency mechanisms and large caches, whereas when running interaction nets, the same data locality rules always apply, and more cores do not increase system complexity. This is illustrated below. In the coming months, we'll be benchmarking Frostbite mesh-like configurations against multi-core “TCPU” arrangements with large caches and larger, wider, out-of-order CPUs, over a larger set of industry benchmarks such as SPEC and PARSEC.

![Schematic floorplans side by side: a TCPU with eight cores, each with its own L1 and L2, around one large shared L3; and Frostbite, an eight-by-eight grid of tiles, each with a small memory block of its own.](https://architectlabs.com/_next/static/media/multicore-floorplan.0u50uc-8yhajl.png)

Fig. 13. Where the memory sits. A multi-core TCPU shares a deep hierarchy of L1, L2, and L3 caches across its cores; Frostbite gives every tile its own small memory. Schematic, not to scale.

## What comes next

While interaction nets have been around for more than 35 years, their production value has never been fully clear. Historically, there has always been a risk of exploring out-of-distribution architectures, for the time, investment, and labor required to come to conviction. We believe AI fundamentally changes these economics. In the search for higher performance and lower power, extreme co-design of workloads, ISA, and hardware will continue to be demanded. With large agent swarms, sophisticated search-space techniques, precise guardrails, formal verification, and faster EDA tools, AI systems will be able to discover and build novel accelerators at a quality that exceeds human cognition. While there is much more to do, we feel it is the right time to share with the world what the future of autonomous chip design could look like. We plan to continue scaling development on Frostbite, including but not limited to:

- Deliver a software playground for anybody to run a program on Frostbite, hosted on FPGAs.
- Build a preliminary operating system capable of managing hardware resources, multiple programs, virtualization, and more.
- Scale swarms to bring Frostbite to higher performance-per-watt, and benchmark it on a much larger set of workloads against a broader set of CPUs.
- Release the RTL and ISA for Frostbite.

We are having a lot of fun!

## Notes

For legal reasons, we cannot disclose the exact CPU.

[^1]: 1.Lafont, Y. (1990). “Interaction Nets.” Proceedings of the 17th ACM SIGPLAN-SIGACT Symposium on Principles of Programming Languages (POPL ’90), 95–108. [doi:10.1145/96709.96718](https://doi.org/10.1145/96709.96718)

[^2]: 2.The Vine programming language. [vine.dev](https://vine.dev)

[^3]: 3.Tendrils (2026). “On Interaction Nets.” [tendrils.co/blog/2026-04-26/on-inets](https://tendrils.co/blog/2026-04-26/on-inets)

[^4]: 4.de Moura, L., and Ullrich, S. (2021). “The Lean 4 Theorem Prover and Programming Language.” Automated Deduction – CADE 28, LNCS 12699, 625–635. [doi:10.1007/978-3-030-79876-5\_37](https://doi.org/10.1007/978-3-030-79876-5_37)
