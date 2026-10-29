---
title: 'Grace Blackwell needs a new scheduler: 27% more usable GPUs from the same racks'
link: https://skypilot.ai/blog/grace-blackwell-needs-a-new-scheduler
source: hnrss-org
published: 2026-10-29T15:45:05Z
updated: 2026-10-29T15:45:05Z
first_seen: 2026-10-29T20:30:27.834895129Z
authors:
- wondersky
content: extracted
html: 2026-10-29-grace-blackwell-needs-a-new-scheduler-27-more-usable-gpus.html
remote_preview:
  url: https://cdn.sanity.io/images/9qcljuzt/production/3cb7da5832bff00b2dd5c3fd21fc742b3e0a09ae-3840x2160.png?rect=0,77,3840,2006&w=1200&h=627&fit=crop&auto=format
  alt: Grace Blackwell Needs a New Scheduler, shown next to an NVL72 rack
---

Not all high-speed interconnects are created equal, and using the wrong one can be quite costly: running the same distributed training job over InfiniBand rather than NVLink can [degrade performance by 43%](https://nanotron-ultrascale-playbook.static.hf.space/index.html#:~:text=which%20typically%20sees%20around,scenarios).

Historically, NVLink was limited to communication within a single node, meaning that multi-node AI workloads were forced to communicate over InfiniBand or a cloud equivalent (EFA on AWS, GPUDirect-TCPX on GCP). NVIDIA’s NVL72 systems remove this limitation by connecting all 72 GPUs in a rack with a unified NVLink fabric, so GPUs on different nodes can exchange data with an order of magnitude more bandwidth.

![Infiniband](https://cdn.sanity.io/images/9qcljuzt/production/ccfaea9718e3d712a67506090f439426775972b7-1080x1080.svg?auto=format&h=3840&w=3840&fit=min)

However, maximizing the benefits of NVL72 systems is highly dependent on workload placement: a multi-node job spread across racks is still bottlenecked by InfiniBand. Preserving rack-locality gets harder as hardware failures take nodes offline and jobs finishing at different times scatter free capacity across racks.

This blog covers the communication bottleneck in large-scale training and inference, the NVL72 rack-scale fabric, and slice sizing for resilience to GPU failures, as well as how the [SkyPilot Platform](https://skypilot.ai/platform) keeps multi-node jobs rack-local through topology-aware placement, in-place recovery from hardware failures, and automatic defragmentation of shared clusters.

Frontier training jobs run across thousands of GPUs. Doing so requires answering two questions:

1. How do you shard your model across GPUs to ensure that it can actually run?
2. Given your compute budget, how many copies of the sharded model can you run to maximize training throughput?

The first question is typically answered with a combination of:

- Tensor Parallelism (TP): splitting a layer’s weight matrices across GPUs
- Expert Parallelism (EP): placing a mixture-of-experts (MoE) layer’s experts on different GPUs
- Pipeline Parallelism (PP): splitting a model layer-wise across GPUs

The answer to the second question sets the degree of Data Parallelism (DP): the number of copies of the sharded model that train in parallel, each on a different slice of the training data.

Of these parallelism strategies, tensor parallelism’s all-reduce and expert parallelism’s all-to-all communication patterns sit on the critical path during training, making them [the most important to keep on NVLink](https://arxiv.org/abs/2603.07685) (§9.1.2).

### Tensor Parallelism

Tensor parallelism splits the weight matrices inside each layer across GPUs, so every GPU only stores and computes a slice of each layer.

Transformer layers are made up of two blocks: self-attention and a multi-layer perceptron (MLP). At its core, each block is two matrix multiplications (GEMMs) in a row. [Megatron-LM](https://arxiv.org/abs/1909.08053) (§3) showed that by partitioning a block’s first GEMM column-wise and its second GEMM row-wise, the GPUs only need to synchronize via all-reduce once per block, after the second GEMM, instead of after each one.

![Animated diagram: tensor parallelism splits A column-wise and B row-wise across two GPUs, and one all-reduce sums the partial z results](https://cdn.sanity.io/images/9qcljuzt/production/57989e427e99a44011eac3fcd2758eb912f7c4b4-1200x400.svg?auto=format&h=1280&w=3840&fit=min)

**Figure 1.** Tensor parallelism across two GPUs. A is split column-wise, so each GPU computes its own slice of y without communicating. B is split row-wise to match, so each GPU produces a partial z, and one all-reduce sums the partial results.

Even with this optimization, tensor parallelism still requires two all-reduces per transformer layer during the forward pass. [These all-reduce steps sit on the critical path](https://nanotron-ultrascale-playbook.static.hf.space/index.html#:~:text=The%20inability%20to%20fully%20hide,directly%20adds%20to%20the%20critical%20path) during training because they cannot be fully overlapped with computation. As a result, tensor parallelism is typically kept on NVLink: Hugging Face measured a 43% drop in training throughput when it moved off NVLink onto InfiniBand.

### Expert Parallelism

In a mixture-of-experts (MoE) layer, the MLP block is replaced by many smaller MLPs, called experts, and a router that sends each token to a small subset of experts. Expert parallelism splits MoE layers such that these small, independent experts are placed on different GPUs.

Under expert parallelism, each MoE layer requires two all-to-all communication steps during the forward pass, dispatch and combine. A router selects the top-*k* experts for each token and dispatches tokens to the GPUs that have those experts in memory. Each GPU then computes its local experts on the tokens it received via a single grouped matrix multiplication. Finally, the expert outputs are sent back to each token's original GPU and combined.

![Expert parallelism diagram: an all-to-all dispatches tokens to experts on different GPUs, and a second all-to-all combines the results](https://cdn.sanity.io/images/9qcljuzt/production/0fdb50fb0732072a16f453ecd1a787bfd9bb9e9b-2008x950.png?auto=format&h=1817&w=3840&fit=min)

**Figure 2.** Expert parallelism distributes experts across GPUs. The all-to-all communication dispatches tokens to their assigned experts and combines results. (Source: [NVIDIA](https://arxiv.org/abs/2603.07685), Figure 4.)

As models add more MoE layers and route each token to more experts, the volume of all-to-all traffic grows, introducing a communication bottleneck.

Training an MoE model with 64-way expert parallelism spreads each MoE layer’s experts across 64 GPUs. On standard 8-GPU servers, that moves the all-to-all traffic off high-bandwidth NVLink and through much narrower InfiniBand interconnects where unoptimized communication can consume [up to 60% of training time](https://arxiv.org/abs/2603.07685) (§1.2). On NVL72 systems, all 64 GPUs stay within a single NVLink domain, [effectively eliminating the communication bottleneck](https://arxiv.org/abs/2603.07685) (§9.2.2).

The parallelism techniques discussed above are not unique to training. Beyond enabling inference engines to serve models that are too large for a single GPU, model parallelism can also improve throughput. Splitting a model's weights across more GPUs frees up memory for the KV cache, so the engine can process [more concurrent requests or support larger context windows](https://vllm.ai/blog/2026-05-14-elastic-expert-parallelism#:~:text=WideEP%20deployments,very%20long%20contexts). In some cases, the freed memory grows faster than the number of GPUs: in one [vLLM](https://vllm.ai/blog/distributed-inference#:~:text=we%20are%20able%20to%20increase%20the%20amount%20of%20KV%20Cache%20blocks%20by%2013.9x) benchmark, splitting a model tensor-parallel across two GPUs increased the amount of KV cache blocks by 13.9x and delivered 3.9x more throughput.

![Animated diagram: Llama-3.1-70B weights fill most of one H100, and sharding them across two H100s frees memory for 13.9x more KV cache blocks](https://cdn.sanity.io/images/9qcljuzt/production/562f38685bdd27a669651d0bb017fa6fc9b4d859-950x384.svg?auto=format&h=1552&w=3840&fit=min)

**Figure 3.** GPU memory breakdown for Llama-3.1-70B on one vs. two H100s. (Data: [vLLM](https://vllm.ai/blog/distributed-inference).)

Expert parallelism follows the same logic. With a low degree of expert parallelism, each GPU holds many experts, leaving less room for the KV cache. WideEP, which refers to expert-parallel configurations spanning multiple nodes, significantly reduces the number of experts per GPU. The trade-off is network traffic: every all-to-all now spans more GPUs. On standard 8-GPU servers, an expert-parallel group can span at most eight GPUs before it has to communicate over InfiniBand, where [per-GPU bandwidth drops 18x](https://inferencex.semianalysis.com/blog/gb200-nvl72-vs-b200-disagg-deepseek-r1-fp4-dynamo-trt#:~:text=once%20decode%20EP%20needs%20more%20than%208%20ranks,50%20GB%2Fs%2C%2018x). NVL72 removes this limit by keeping all 72 GPUs in a rack on NVLink. When serving DeepSeek-R1 at interactive speeds, GB200 NVL72 with 32-way expert parallelism delivers [up to 4.4x higher throughput per GPU](https://inferencex.semianalysis.com/blog/gb200-nvl72-vs-b200-disagg-deepseek-r1-fp4-dynamo-trt#:~:text=delivers%20up%20to%204.39x%20throughput%20per%20GPU) than 8-GPU B200 servers, which are capped at 8-way.

NVL72 extends NVLink from a single node to the entire rack, so the whole rack acts like a [gigantic accelerator with shared memory](https://docs.nvidia.com/multi-node-nvlink-systems/imex-guide/overview.html#:~:text=With%20an%20NVLink%20Network,gigantic%20accelerator%20with%20shared%20memory).

![The NVLink hierarchy in an NVL72 rack: the NVLink domain, NVLink partitions, and IMEX domains](https://cdn.sanity.io/images/9qcljuzt/production/605f11a79bef3258b952d99cbf8ec3eac74ff08a-1504x1020.png?auto=format&h=2604&w=3840&fit=min)

**Figure 4.** The NVLink hierarchy within an NVL72 rack. (Source: [NVIDIA](https://developer.nvidia.com/blog/running-ai-workloads-on-rack-scale-supercomputers-from-hardware-to-topology-aware-scheduling/), Figure 1.)

Every GPU on the NVL72 rack has 18 NVLink ports each delivering 100 GB/s of bidirectional bandwidth, which sums up to a total of [1.8 TB/s for each GPU](https://developer.nvidia.com/blog/nvidia-gb200-nvl72-delivers-trillion-parameter-llm-training-and-real-time-inference/#:~:text=Each%20NVLink%20switch%20tray%20delivers,bidirectional%20throughput%20per%20GPU). Each NVLink port on a given GPU maps to one of 18 NVSwitch chips that power the rack's fabric backplane. Because each NVSwitch chip accepts 72 NVLink lines, one from each of the GPUs, the aggregate internal bandwidth within the rack is 130 TB/s.

![NVL72 topology: every GPU connects one NVLink port to each of the rack's 18 NVSwitch chips](https://cdn.sanity.io/images/9qcljuzt/production/38a06893c172492ee7ec3f125530b4d902316f7f-938x341.png?auto=format&h=1396&w=3840&fit=min)

**Figure 5.** Each GPU connects one NVLink port to each of the rack's 18 NVSwitch chips. (Source: [NADDOD](https://www.naddod.com/blog/nvidia-gb200-interconnect-architecture-analysis-nvlink-infiniband-and-future-trends).)

This network topology ensures that the fabric is fully non-blocking, meaning that [any GPU in the rack can transmit data to any other GPU](https://www.nvidia.com/en-us/data-center/nvlink/#:~:text=The%20NVIDIA%20NVLink%20Switch%20chips%20connect%20multiple%20NVLinks,across%20the%20entire%20rack) with uniform latency, making it ideal for large-scale distributed AI workloads.

![An NVL72 rack next to its non-blocking NVSwitch fabric, which connects every GPU in the rack](https://cdn.sanity.io/images/9qcljuzt/production/85845fc9533a08a24492246643762168dc1cd8d8-1354x866.png?auto=format&h=2456&w=3840&fit=min)

**Figure 6.** The NVSwitch fabric is fully non-blocking. Any GPU can communicate with any other GPU in the rack at full bandwidth with the same latency. (Source: [NVIDIA](https://developer.nvidia.com/blog/enabling-multi-node-nvlink-on-kubernetes-for-gb200-and-beyond/), Figure 1)

With NVL72 systems, it is tempting to allocate entire 18-node racks to workloads to maximize cluster utilization. In practice, teams actually get more usable capacity out of their cluster by leaving a couple of spare nodes in each rack.

GPUs are unreliable. On its gigawatt-scale supercluster, [Meta](https://engineering.fb.com/2026/10/19/data-center-engineering/inside-prometheus-meta-gigawatt-scale-supercluster/#:~:text=overall%20wall%2Dclock%20mean%20time,47k%20GPU%20hours) measured a mean time between interruptions (MTBI) of roughly 490 days per 4-GPU node after putting significant engineering effort into improving reliability. NVL72 clusters make the 72-GPU rack the atomic unit of compute, which increases the fault blast radius by 18x compared to the 4-GPU nodes that make up the rack. The MTBI for the rack is given by:

Based on the formula above, a rack would be interrupted roughly every 490 / 18 ≈ 27 days. If jobs are allocated entire 18-node racks, each of those interruptions makes the entire rack unschedulable until the failed node is repaired. On a 128-rack cluster, this means that a rack would become unusable for full-rack jobs roughly every 5 hours.

Reserving two nodes per rack as spares changes this. When a node fails, a spare in the same rack replaces it, so a single node failure no longer takes the entire rack out of commission. The cost is that two nodes per rack sit idle while everything is healthy.

To quantify this trade-off, [NVIDIA](https://developer.nvidia.com/blog/achieving-peak-system-and-workload-efficiency-on-nvidia-gb200-nvl72-with-slurm-block-scheduling/#what_is_the_impact_of_segment_size_on_node_availability) modeled how different slice sizes affect usable capacity on a 128-rack (2,304-node) cluster as the share of unavailable nodes grows.

![Chart of usable capacity of a 128-rack cluster versus node unavailability for 8, 9, 16 and 18-node slices](https://cdn.sanity.io/images/9qcljuzt/production/e89aa6d3260938cd4d484af409610288c513afd7-1753x1105.png?auto=format&h=2421&w=3840&fit=min)

**Figure 7.** Usable capacity of a 128-rack cluster for different slice sizes as node unavailability increases. Each slice must fit within a single rack. (Adapted from [NVIDIA](https://developer.nvidia.com/blog/achieving-peak-system-and-workload-efficiency-on-nvidia-gb200-nvl72-with-slurm-block-scheduling/), Figure 6.)

At SkyPilot, we manage tens of thousands of our customers’ GPUs, and on production NVL72 clusters we have measured unavailability rates of around 2%. At that rate, NVIDIA’s model shows that 16-node slices provide 27% more usable capacity than 18-node slices (2,037 vs. 1,602 nodes). They also beat 9-node slices by 4%, even though two 9-node slices fill a rack exactly: with 9-node slices, every failure costs half a rack, while with 16-node slices, the first two failures in a rack are fully absorbed. Compared with 8-node slices, 16-node slices give each job twice as many GPUs per NVLink domain while giving up less than 1% of usable capacity until node unavailability exceeds about 3.3%.

Sixteen nodes is also a natural size for distributed AI workloads: 64 GPUs is a power of two, which [matches how parallelism groups are typically sized](https://engineering.fb.com/2026/10/19/data-center-engineering/inside-prometheus-meta-gigawatt-scale-supercluster/#:~:text=Training%20parallelism%20maps%20most%20efficiently,in%2Drack%20spares).

So far, we have covered why the most communication-heavy parallelism techniques belong on NVLink, how NVL72 extends NVLink from a single node to the whole rack, and why 16-node slices with two spare nodes per rack get the most usable capacity out of production clusters. SkyPilot puts this into practice by placing each slice on a single rack, recovering from hardware failures in-place, and keeping whole racks free as jobs come and go.

### Schedulers need to understand the hardware

Choosing the right slice size is only the first step. How do researchers ensure that each slice of a multi-node training run actually lands on a single rack?

Naive schedulers split multi-node jobs across racks, forcing workers to communicate over [InfiniBand (400 Gb/s per direction) instead of NVLink (1.8 TB/s bidirectional)](https://developer.nvidia.com/blog/achieving-peak-system-and-workload-efficiency-on-nvidia-gb200-nvl72-with-slurm-block-scheduling/#:~:text=All%20communications%20within%20the%20rack,through%20InfiniBand%20or%20Ethernet):

![Animated diagram: a naive scheduler places hero-run on free nodes in two racks, so its traffic crosses InfiniBand](https://cdn.sanity.io/images/9qcljuzt/production/14d403190b5169198135aa2a7271cb4266fa0792-1300x382.svg?auto=format&h=1128&w=3840&fit=min)

**Figure 8.** Naive placement splits a multi-node job across racks.

SkyPilot’s Topology-Aware Scheduler (TAS) ensures that workers are colocated on the same rack:

![Animated diagram: SkyPilot's Topology-Aware Scheduler places all of hero-run's nodes on a single rack](https://cdn.sanity.io/images/9qcljuzt/production/7a31a0b77c0ee4805db819f9913aeeaea105a331-1300x382.svg?auto=format&h=1128&w=3840&fit=min)

**Figure 9.** SkyPilot's Topology-Aware Scheduler keeps the job's workers on a single rack.

For multi-rack workloads, TAS splits the job into 16-node (configurable) slices, and places them each on their own rack with two spare nodes. Then, when **SkyPilot’s GPU Manager** detects GPU failures, **SkyPilot Frontier Trainer** can hot swap the faulty node with one of the reserved healthy nodes while the remaining nodes continue running without interruption. If there are no remaining spares in the same rack, Frontier Trainer will reschedule the slice on the faulty rack to a healthy rack:

![Animated diagram: hero-run runs as two 16-node slices with two spare nodes per rack; failed nodes are swapped for spares, then the slice moves to an idle rack](https://cdn.sanity.io/images/9qcljuzt/production/b047ba77904733fcae9bade5e965e42af589d647-1300x534.svg?auto=format&h=1577&w=3840&fit=min)

**Figure 10.** Slice-based scheduling holds two healthy spare nodes in each rack. When GPU Manager detects a failed node, Frontier Trainer swaps the faulty node for a spare in the same rack.

To quantify the impact of slice-based scheduling, [NVIDIA’s GR00T team compared the effect of two different scheduling policies](https://www.youtube.com/watch?v=4AmKOKbieus&t=910s) during pre-training of GR00T N1.7, a multimodal foundation model for humanoid robots, on 512 GB300 GPUs across 128 nodes. The first run used a naive scheduler that placed nodes across an arbitrary number of racks. The second run was scheduled with a rack-aware, slice-based policy like the one in Figure 10, which placed it on exactly 8 racks. Rack-aware placement raised throughput by 14.4%, cutting each iteration from 1.59 s to 1.39 s.

On a 10,000-GPU cluster, the same speedup saves roughly $16.7K per training hour. That’s about $400K saved for every training day, just by making better scheduling decisions.

### Fragmentation blocks rack-scale jobs

So far we have explored how topology-aware scheduling works in isolation. In practice, large clusters are typically shared: heterogeneous workloads such as pre-training runs, RL rollouts, and batch inference all compete for the same resources. TAS bin packs workloads within the same rack at scheduling time, but jobs finish at different times, leaving free nodes scattered across racks. This is fragmentation: the cluster has enough free GPUs to run a rack-scale job, but because they’re spread across racks, the job can’t be scheduled.

![Animated diagram: free nodes are split across two racks, so the 16-node hero-run waits in the queue](https://cdn.sanity.io/images/9qcljuzt/production/280bbc546f82a3c4976d863e35b5434272c636a0-1300x382.svg?auto=format&h=1128&w=3840&fit=min)

**Figure 11.** Jobs finish at different times, leaving free nodes split across racks. With 16 nodes free but split between the two racks, the 16-node hero-run cannot be placed and waits.

**SkyPilot’s Automatic Defragmentation** (Auto Defrag) is built to address this. When a job is blocked by topology rather than capacity, Auto Defrag finds the least disruptive set of workloads to preempt to free up an entire rack. Those workloads are rescheduled on other racks, allowing the blocked job to be scheduled.

![Animated diagram: Auto Defrag moves sft-d to rack-a, freeing rack-b for the 16-node hero-run](https://cdn.sanity.io/images/9qcljuzt/production/b2ff9f3f2754c09354a633c503aa9fa5d4975920-1300x382.svg?auto=format&h=1128&w=3840&fit=min)

**Figure 12.** Auto Defrag preempts sft-d and reschedules it in rack-a, where it resumes from checkpoint. This frees up rack-b, so hero-run can be scheduled as a single 16-node slice.

Auto Defrag only preempts workloads that restart safely, such as managed jobs that resume from a checkpoint. Interactive development environments are left alone unless an admin explicitly opts them in.

Large-scale AI workloads are distributed by default, and their most network-intensive communication patterns sit on the critical path. Rack-scale systems like NVL72 aim to address this bottleneck in hardware, but the hardware alone doesn't guarantee the speedup. These systems require rack-aware placement, resilience to hardware failures, and dynamic rebalancing as jobs come and go. The SkyPilot Platform is purpose-built to close that gap: Topology-Aware Scheduling keeps multi-node jobs on NVLink, Frontier Trainer swaps in a healthy spare when GPU Manager detects hardware failures, and Automatic Defragmentation keeps shared racks open for large jobs. If you're wrangling NVL72 clusters, we'd love to [chat](https://skypilot.ai/demo).
