---
title: Factoring RSA-260
link: https://cognition.com/blog/factoring-rsa-260
source: cognition-com-blog
published: 2026-09-09T17:00:00Z
first_seen: 2026-09-09T21:55:36.456408028Z
content: extracted
html: 2026-09-09-factoring-rsa-260.html
preview:
  file: 2026-09-09-factoring-rsa-260.preview-d713da6fe652.webp
  width: 256
  height: 134
  alt: Factoring RSA-260
  color: '#d6d4d5'
images:
- source: https://cognition.com/opengraph-image.jpg
  original:
    file: 2026-09-09-factoring-rsa-260.image-fc6d350cc75a.jpg
    width: 1200
    height: 630
  variants:
  - file: 2026-09-09-factoring-rsa-260.image-e9be4909f847.webp
    width: 48
    height: 25
  color: '#e9e8e8'
---

By Eric Lu 09.09.26

Over the past few weeks, the Cognition research team and I have been optimizing our job scheduler to better use disaggregated compute. As a proof of concept, and because I’ve enjoyed factoring numbers as a hobby for the past ten years or so, I drove a bevy of Devins to obtain a factorization of [RSA-260](https://en.wikipedia.org/wiki/RSA_numbers#RSA-260). In order to do this, my Devins built the world's highest-performance GPU lattice siever, which enables factoring numbers at 10x lower cost than the previous public state of the art. Here is the factorization:

```
22112825529529666435281085255026230927612089502470015394413748319128822941402001986512729726569746599085900330031400051170742204560859276357953757185954298838958709229238491006703034124620545784566413664540684214361293017694020846391065875914794251435144458199
= 4397328654844826923795068102505872571721883526553349659561256924505973939597593482272505698004801207988043088656411102133523080581
× 5028695206842569864686141618253083416610081090075366674776775706538324961364412200138116378509733307971876652984898985905923678379
```

RSA-260 (a 260 digit number) sets a new record for the largest publicly solved [RSA Factoring Challenge](https://en.wikipedia.org/wiki/RSA_Factoring_Challenge) problem, which benchmarks the feasibility of breaking the [RSA cryptosystem](https://en.wikipedia.org/wiki/RSA_cryptosystem). The previous record, RSA-250, was [set](https://arxiv.org/abs/2006.06197) in February 2020. For reference, state-of-the-art RSA public keys contain 2048 bit (~617 digit) factoring problems, while 1024-bit (~309 digit) RSA was deprecated in 2013.

Below I’ll give some details about how this was accomplished, but here are two important takeaways:

- Hyperscalers or frontier AI labs could likely factor RSA-1024 numbers at a cost on the order of $30 million per number — and, with a bit more optimization, likely substantially less. On the other hand, RSA-2048 remains roughly a billion times harder than RSA-1024 and does not appear to be meaningfully affected by this work.

- Devin is a sufficiently powerful software engineer to solve a challenging problem at the intersection of computational number theory and GPU performance engineering. My role was primarily to set priorities, establish benchmarks, and recognize when work was going off-track. Devin otherwise autonomously handled measurements, cluster operations, and optimization end-to-end. This substituted for what would likely have been a multi-month effort by a team of highly specialized domain experts.

In conclusion, the barrier to entry for cryptanalytic work, other computational mathematics more broadly, and likely most large-scale scientific computing research, is far lower than it used to be. Exciting work beckons anywhere programming can be used to solve a research problem; I encourage all to be ambitious and explore what autonomous software engineering agents can do when applied to these fields!

## [How did this happen?](https://cognition.com/blog/factoring-rsa-260#how-did-this-happen)

Contrary to some [circulating](https://x.com/_seanyneutron/status/2095391516504244480) [claims](http://x.com/penlume/status/2095429389660164463), I did not factor RSA-260 by guessing and checking 130-digit prime numbers by hand. Cognition also has not yet built a multi-thousand-qubit quantum computer. RSA-260 was factored by a new implementation of the [general number field sieve](https://en.wikipedia.org/wiki/General_number_field_sieve) (GNFS) for GPUs, prepared and run using Devin. GNFS is the most efficient algorithm known for (most) numbers above roughly 100 digits in size and was used in [previous record-breaking RSA number factorizations](https://arxiv.org/abs/2006.06197).

The implementation was a significantly modified [CADO-NFS](https://cado-nfs.gitlabpages.inria.fr/). I report essentially no algorithmic advancements — implementing lattice sieving and sparse linear system solving on GPUs required only “good old performance engineering” to take advantage of the preposterous memory systems of the GPU.

## [Cost estimates](https://cognition.com/blog/factoring-rsa-260#cost-estimates)

In total, I estimate that this factorization cost about 4,900 GPU-days, or 13.5 GPU-years, which is about $400k at current market prices. In more detail, modern GNFS implementations consist of a few stages run sequentially: polynomial selection, lattice sieving, and linear system solving. The time breakdown was:

- 643 GPU-days in polynomial selection (this is anomalously high basically due to operator incompetence)

- 3,813 GPU-days sieving

- 467 GPU-days linear system solving (of which about 7% failed to make progress due to crashes or preemption by more important work)

I did this as a side project using a single-digit percentage of our cluster, in the course of optimizing our job scheduler to improve allocations to use disaggregated compute. What does this mean for larger RSA instances?

RSA-1024 is equivalent to 309 digits; according to standard GNFS scaling this is merely 78x more computation than RSA-260. I estimate the cost of factoring RSA-1024 at market GPU prices to be roughly $30M, which can [trade off](https://eprint.iacr.org/2015/1000) against wall clock time. I know for a fact that the current implementation remains significantly suboptimal; I would not be surprised if moderate further work could reduce the cost of factoring RSA-1024 by another multiple of 2.

Of course, **the fact that RSA-1024 is insecure is not news.** There was speculation that the NSA might have the capability to do RSA-1024 economically as early as the mid-2000s (see e.g. [TWIRL](https://link.springer.com/chapter/10.1007/978-3-540-45146-4_1) or the [Bernstein matrix machine](https://research.tue.nl/en/publications/analysis-of-bernsteins-factorization-circuit/)). Instead, as we describe below, the main developments are (1) a potentially lower cost (in dollars and time) for the factorization, (2) potentially more parties capable of performing the factorizations (you just need enough GPUs rather than making specialized hardware), and (3) **the relative ease with which non-cryptographers can now work on speeding up factoring**.

Finally, I emphasize that the efficiency gains have little impact on the feasibility of factoring RSA-2048-sized numbers with GNFS.

## [Factoring on spare compute](https://cognition.com/blog/factoring-rsa-260#factoring-on-spare-compute)

The factorization ran at no marginal cost on spare or fragmented compute that couldn’t be used for other purposes. Why does this compute exist?

The clusters we use for LLM training and inference contain [NVL72](https://www.nvidia.com/en-us/data-center/gb300-nvl72/) racks, each nominally comprising 18 computers interconnected by fast NVLink. LLM workloads use groups of computers within single racks in order to take advantage of this fast interconnect. The job scheduler must solve a constrained optimization problem to pack workloads into racks. In this global allocation, some racks may end up with an idle node or two; sometimes jobs request spare nodes to accommodate failover, or jobs might need even numbers of computers on a rack that has only 17. For us, these inefficiencies amount to a single-digit percentage of overall compute.

To make use of this spare compute, as a first step, I rigged our job scheduler to fill single-node jobs around other workloads at bottom priority. However, we also lacked a consistent source of readily preemptible single-node workloads. Naturally, at this point I thought of lattice sieving, which is a perfect fit for this situation.

[Lattice sieving](https://en.wikipedia.org/wiki/Lattice_sieving) is embarrassingly parallel over billions of small work units, can make progress using single nodes at a time, and is safe to preempt instantly. It is also the most computationally expensive part of GNFS, so getting sieving done is a lot of progress toward a factorization. However, all prior public GNFS factorization records used only CPUs for lattice sieving; indeed, due to challenges of efficiently implementing lattice sieving on GPUs, for a long time it was not clear that GPU lattice sieving could be more cost-effective overall.

In short, all that I was missing was a sufficiently high-performance GPU lattice siever that could accept the parameters needed to factor RSA-260. So, what did I do? Ask Devin.

## [Using Devin to optimize GNFS](https://cognition.com/blog/factoring-rsa-260#using-devin-to-optimize-gnfs)

At August 13th 0:11:58 Pacific time I aimed Devin at producing a drop-in replacement for `las`, the CPU lattice siever of CADO-NFS. Here is the prompt I used:

> CADO-NFS is FOSS software for performing GNFS. I'd like you to develop a fast GPU lattice siever. This has historically been difficult with present techniques because optimizations for CPU lattice sieving employ a lot of conditional branching and complex memory access patterns, and the intersection of GPU kernel-writing experts and number field theory experts is quite small. However, the higher total memory bandwidth available in a GPU promises a higher performance ceiling. And that intersection now contains you. You have Modal access keys available that I authorize you to use to spin up a single GPU box for performance testing. Obtain the CUDA toolchain etc. and build locally; use GPU only for performance testing and shut it down between measurements. The resultant GPU lattice siever should be a drop-in replacement for the siever step of CADO-NFS. Iterate until you exceed the performance of the CPU lattice siever.

Two hours later I added that `glas` (GPU `las`, of course) should be able to handle the parameters used for RSA-250. Then I went to bed. I woke up to find that, after another 7 hours of iteration, Devin had succeeded.

Over the subsequent week I drove Devin to optimize lattice sieving, then the rest of the GNFS pipeline. First, I will give a high-level description of our GNFS optimizations, and then I will describe the optimization workflow.

### GNFS optimizations (high-level)

At a high level, GNFS consists of a number of stages run sequentially: polynomial selection, lattice sieving, and linear system solving.

Polynomial selection fixes the number field over which the algorithm is run. The choice of polynomial controls a constant-factor speedup on the lattice sieving step. It’s therefore typically worth spending a fixed fraction (~5%) of overall sieving compute finding a “good” polynomial. There was little technical innovation here; we adapted CADO’s stage-1 polynomial selection to GPUs (resulting in `gps1`), using also some kernel equipment from [msieve](https://sourceforge.net/projects/msieve/)’s well-optimized stage-1 polynomial selection. Polynomial selection is more or less embarrassingly parallel.

The bulk of the GNFS computational effort is in lattice sieving. The goal of lattice sieving is to produce lots (8.3 billion in our case) of sparse linear relations over GF(2), where here (suppressing some details) the vector entries represent the parities of prime exponents in the prime factorization of a smooth number. Lattice sieving is also embarrassingly parallel over work items called “special q”s, but each work item requires performing a large number of read/writes at (for our purposes) [pseudorandom](https://www.hyperelliptic.org/tanja/SHARCS/talks/FrankeKleinjung.pdf) locations in a large array; **handling this was the primary technical challenge in optimizing sieving**.

Ultimately, we pack these relations into a large matrix over GF(2) and use linear system solving to find a linear dependence. At this scale, linear system solving is typically distributed and requires a lot of communication bandwidth; this is available in spades over Infiniband and NVLink. The [block Wiedemann](https://en.wikipedia.org/wiki/Block_Wiedemann_algorithm) algorithm permits relaxing the communication constraint somewhat, and the particular implementation in CADO-NFS is also susceptible to optimization and running on GPUs, which we undertook. Solutions can be processed into congruences of squares modulo N, which yields the factorization.

Overall, Devin substantially modified almost every piece, including modifying a couple of interfaces:

- A GPU-adapted version of CADO-NFS’s stage-1 polyselect, with components from msieve

- A GPU-optimized lattice siever based on `las`

- An optimized CADO head to handle the workunit volume

- Parallelized and optimized dup/purge and fused merge/replay programs

- A new GPU-optimized block Wiedemann implementation

- GPU-accelerated sqrt

- Associated run scripts

The only untouched programs in the CADO-NFS pipeline were: `cado-nfs.py` itself, `polyselect_ropt` (stage-2 polynomial selection), `makefb`, and `dup1`. We even had to optimize `fake_rels` to get reasonable performance for synthetic inputs of the needed sizes.

N.B. I understand these components only in the same way a mid-level car hobbyist might understand car components: their approximate role in the overall system, their effects on performance, some tradeoffs of making changes, and what good operation and usage looks like — but not the underlying physics, nor how to fabricate the component from raw materials. That’s to say that I do not understand much of the underlying mathematics. I can tell you that polynomial selection provides a constant factor on lattice sieving yield; I cannot tell you how the polynomial is used in the siever. In this field I am far from an expert!

### The optimization workflow

I drove several parallel Devins. Across the 3-week duration of the project, I had an average of 3 and a maximum of 18 concurrent Devin sessions running. These were roughly of two types:

- iteratively optimizing an individual component — designing and running experiments, interpreting results, selecting target improvements, implementing them, and repeating

- managing end-to-end factorization runs on the cluster — exercising the whole pipeline and supporting scripts, revealing the most important inefficiencies (errors, utilization, performance) for further optimization

In particular, Devin handled substantial portions of:

- choosing and tuning parameters for sieving and linear system solving (though using benchmarking equipment that I had to help bootstrap)

- generating/optimizing polynomial selections (again, with varying degrees of difficulty)

- running the iterative optimization loop

- debugging (naturally)

- setting up and optimizing the processing scripts

- orchestrating large-scale computation on the cluster

I interacted with this optimization loop once every couple of hours, alternating with talking to other Devin sessions for my regular work.

Devin Cloud was especially convenient for this workflow: sessions are persistent and independent from my computer, parallelization is straightforward, sandboxing prevents interference between independent workstreams, and sessions can run autonomously for weeks. We already knew that Devin was effective at GPU programming, experimentation, and cluster management for ML-relevant workloads, but lattice sieving and GNFS are fairly different; I found Devin’s ability to generalize here quite impressive.

### Running up the scaling ladder

This workflow permitted an incredible rate of progress that I (and even Devin itself) could barely keep up with. For the first five days we ran up a scaling ladder. By the end of this, factoring a 190-digit number took the same amount of time as a 157-digit number had at the start — a little over 3 hours.

| Target                                                            | Start (UTC)         | Finish (UTC)        | Wall time                                                 |
| ----------------------------------------------------------------- | ------------------- | ------------------- | --------------------------------------------------------- |
| C155 (term in aliquot sequence 171648)                            | 2026-08-14 21:51:50 | 2026-08-15 00:04:58 | 7,988 s (including a 1,412 s gap from head crash testing) |
| C157 (term in aliquot sequence 171648)                            | 2026-08-14 13:39:58 | 2026-08-14 16:51:24 | 11,486 s                                                  |
| C173 (term in aliquot sequence 9708)                              | 2026-08-14 22:01:02 | 2026-08-15 09:18:48 | 40,666 s                                                  |
| C175 (near-repdigit)                                              | 2026-08-16 00:32:51 | 2026-08-16 08:56:50 | 30,239 s                                                  |
| C190 (near-repdigit)                                              | 2026-08-17 02:17:16 | 2026-08-17 07:34:56 | 19,060 s                                                  |
| C201 (odd perfect number roadblock)                               | 2026-08-17 09:42:44 | 2026-08-18 01:33:59 | 57,075 s                                                  |
| C311 = (179^139 - 1)/178                                          | 2026-08-18 06:52:44 | 2026-08-18 21:11:46 | 51,542 s                                                  |
| C190 (just to startle a Devin who randomly generated a semiprime) | 2026-08-18 22:12:28 | 2026-08-19 01:23:11 | 11,443 s                                                  |
| C344 = (11^331 - 1)/10 (odd perfect number roadblock)             | 2026-08-31 10:44:54 | 2026-09-01 23:40:44 | 132,950 s                                                 |
| RSA-260                                                           | 2026-08-18 12:14:19 | 2026-09-03 01:48:57 | 1,344,878 s                                               |

After factoring the C311 (which was roughly equivalent in difficulty to GNFS on a 216-digit number), it would have been safer to proceed by factoring a 230-digit equivalent difficulty number in order to cross the remaining couple of orders of magnitude. But I estimated that RSA-260 could be done in under a month and therefore had already begun polynomial selection, even though I knew the linear algebra remained significantly underoptimized.

## [What did Devin still need me (or other humans) for?](https://cognition.com/blog/factoring-rsa-260#what-did-devin-still-need-me-or-other-humans-for)

Devin successfully optimized and executed a complex computational number theory pipeline at a scale that previously required considerable human expertise. But I cannot claim that Devin iterated autonomously on the entire end-to-end pipeline. It is interesting to consider what he needed me for.

Somehow, the answer seems to be: still a lot. In retrospect, Devin claims that I sent 82,702 words (502,887 characters) in 3,328 messages across 192 sessions out of 233 used for factoring (totaling 14,450 ACUs). Of these, Devins themselves started 101 child sessions, and 36 received no intervention from me at all.

Devin needed me for executive function, talking through and sanity-checking what it was doing, to wit:

- setting a hierarchy of goals and keeping Devin properly scoped

- recognizing when Devin was doing something unproductive and redirecting (“you don’t need to take that measurement”)

- recognizing repeated inefficiencies in Devin’s workflows (“you can amortize this setup work”)

- pointing out untried directions (“make sure the GPU never blocks on the CPU”, “can you use NVLink SHARP here?”)

- organizing experimental frameworks and results (“do measurements this way and find previous results here, don’t cook up new incomparable approaches every time”)

- catching when Devin prematurely gave up on a direction

While in retrospect I appear to have supplied a couple of specific technical insights, my biggest contribution was probably to handhold the creation of a unified set of measured results, benchmarks, and performance estimators, which evidently were not otherwise going to self-assemble. This enabled Devin to more easily run and understand measurements, and me to understand the extent of progress and where more improvements were needed.

Taking a step back, I also want to emphasize that **this effort would not have been possible without CADO-NFS.** CADO-NFS is an advanced, robust, open source implementation of GNFS that was used in many previous records and also by the factoring hobbyist community. It provided all the relevant techniques, the pipeline stages and their interfaces, and a reference CPU implementation. Using those, I could have the Devin swarm optimize programs roughly independently and compare the outputs with the originals. Like in many other cases, I believe that the human-engineered decomposition of the problem was essential to enabling the agents to make progress.

In fact, I noticed that the further the codebase got from upstream CADO-NFS, the more confused the agents became. This could be attributed to accumulating complexity, but I wonder whether CADO-NFS’s presence in pre-training is also relevant here.

This concludes the technical part of the blog post (except for the appendix, which has far more details).

## [Assorted reflections](https://cognition.com/blog/factoring-rsa-260#assorted-reflections)

What has happened here is strange to me. My name is on the result, but it’s not clear to me how to apportion credit among myself, Devin, the hardware, and the world at large.

For programming tasks, I think of current models and agents somewhat like a sewing machine or a loom. I push it along in some way; it evidently could not happen without me, but neither am I throwing the shuttle by hand. It is hard to say exactly what my role was, although I am confident that it was not nothing. In some glib sense, doing this with Devin was not that different from what would have happened otherwise: I interacted with a physical system much larger than me whose precise workings are obscure to me, energy was dissipated, and the factors resulted.

On the other hand, something clearly new is happening. About three weeks elapsed from first prompt to RSA-260 factors found. The rate evinces a capability overhang whose extent we are only beginning to explore. I think we should not shy away from this exploration. In particular, if a problem can be solved via “just programming”, it seems worthwhile to attempt this immediately.

In the past few days, we’ve also seen some impressive claimed results in mathematics, including a claimed solution to one of the Millennium Prize Problems. I share some concerns about a loss of human understanding. I did not learn as much about NFS or GPU programming as I could have expected to had I done this on my own, though likely more than I would have if I were not using Devin at all. It seems important to figure out how to globally allocate resources to maintain human understanding even when technology enables or incentivizes us to forgo this. At the same time, these tools may allow us to choose where deeper understanding is most valuable, explore further than any individual could previously reach, and expand the set of people who can make meaningful contributions. I see this work as an example in both directions.

## [Acknowledgments](https://cognition.com/blog/factoring-rsa-260#acknowledgments)

Thanks to Alex Lombardi for mathematical consultation and much editing.

Thanks to my employer Cognition; this would not have been possible without Cognition compute. Thanks especially to the Cognition research team for tolerating the endlessly respawning `glas` jobs. Thanks also to friends and especially my wife Jiwon Joung for supporting this hobby project.

I’d like to dedicate this to the whole online factoring community — GIMPS, mersenneforum, FactorDB, GPU to 72, and the rest — for sparking my interest here in the first place and keeping the chase alive.

## [References](https://cognition.com/blog/factoring-rsa-260#references)

1.  \[1\] F. Boudot, P. Gaudry, A. Guillevic, N. Heninger, E. Thomé, and P. Zimmermann, "Comparing the Difficulty of Factorization and Discrete Logarithm: A 240-Digit Experiment," *Advances in Cryptology — CRYPTO 2020*, LNCS 12171, pp. 62–91, 2020. [arxiv.org/abs/2006.06197](https://arxiv.org/abs/2006.06197)
2.  \[2\] The CADO-NFS Development Team, "CADO-NFS, An Implementation of the Number Field Sieve Algorithm," software project, accessed September 9, 2026. [cado-nfs.gitlabpages.inria.fr](https://cado-nfs.gitlabpages.inria.fr/)
3.  \[3\] N. David and P. Zimmermann, "A New Ranking Function for Polynomial Selection in the Number Field Sieve," *75 Years of Mathematics of Computation*, Contemporary Mathematics 754, pp. 315–325, 2020. [inria.hal.science/hal-02151093v4/document](https://inria.hal.science/hal-02151093v4/document)
4.  \[4\] J. Franke and T. Kleinjung, "Continued Fractions and Lattice Sieving," *Special-Purpose Hardware for Attacking Cryptographic Systems — SHARCS 2005*, 2005. [hyperelliptic.org/tanja/SHARCS/talks/FrankeKleinjung.pdf](https://www.hyperelliptic.org/tanja/SHARCS/talks/FrankeKleinjung.pdf)
5.  \[5\] A. K. Lenstra, A. Shamir, J. Tomlinson, and E. Tromer, "Analysis of Bernstein’s Factorization Circuit," *Advances in Cryptology — ASIACRYPT 2002*, LNCS 2501, pp. 1–26, 2002. [research.tue.nl/en/publications/analysis-of-bernsteins-factorization-circuit](https://research.tue.nl/en/publications/analysis-of-bernsteins-factorization-circuit/)
6.  \[6\] J. Papadopoulos and contributors, "Msieve," integer-factorization software, accessed September 9, 2026. [sourceforge.net/projects/msieve](https://sourceforge.net/projects/msieve/)
7.  \[7\] A. Shamir and E. Tromer, "Factoring Large Numbers with the TWIRL Device," *Advances in Cryptology — CRYPTO 2003*, LNCS 2729, pp. 1–26, 2003. [link.springer.com/chapter/10.1007/978-3-540-45146-4\_1](https://link.springer.com/chapter/10.1007/978-3-540-45146-4_1)
8.  \[8\] M. Tervooren and contributors, "FactorDB," online factorization database, accessed September 9, 2026. [factordb.com](https://factordb.com/)
9.  \[9\] L. Valenta, S. Cohney, A. Liao, J. Fried, S. Bodduluri, and N. Heninger, "Factoring as a Service," *Financial Cryptography and Data Security — FC 2016*, LNCS 9603, pp. 321–338, 2016. [eprint.iacr.org/2015/1000](https://eprint.iacr.org/2015/1000)
10. \[10\] D. H. Wiedemann, "Solving sparse linear equations over finite fields," *IEEE Transactions on Information Theory*, vol. 32, no. 1, pp. 54–62, January 1986. [doi.org/10.1109/TIT.1986.1057137](https://doi.org/10.1109/TIT.1986.1057137)

## [Appendix 1: Cost estimates to factor other RSA numbers](https://cognition.com/blog/factoring-rsa-260#appendix-1-estimated-cost-of-factoring-other-rsa-numbers)

| #        | vs. RSA-260 | CPU core-years | CPU cost ($)  | GPU-years     | GPU cost ($)  |
| -------- | ----------- | -------------- | ------------- | ------------- | ------------- |
| RSA-230  | 0.053×      | 372\*          | 260k\*        | 0.72\*        | 21.9k\*       |
| RSA-240  | 0.142×      | 1,000          | 701k\*        | 1.9\*         | 58.8k\*       |
| RSA-250  | 0.385×      | 2,700          | 1.89M\*       | 5.2\*         | 159k\*        |
| RSA-260  | 1×          | 7,010\*        | 4.91M\*       | 13.5          | 414k\*        |
| RSA-1024 | 77.9×       | 546,000\*      | 383M\*        | 1,050\*       | 32.3M\*       |
| RSA-2048 | 91.2B×      | 6.39 × 10¹⁴\*  | 4.48 × 10¹⁷\* | 1.23 × 10¹²\* | 3.77 × 10¹⁶\* |

\* = estimated. Costs assume $0.08/CPU core-hour, $3.50/GPU-hour. CPU estimates use GNFS scaling from RSA-250 and GPU estimates from RSA-260.

## [Appendix 2: Details of the RSA-260 run](https://cognition.com/blog/factoring-rsa-260#appendix-2-details-of-the-rsa-260-run)

### Polynomial selection

Polynomial selection began on 2026-08-18 at 12:14:19 UTC. The search was irregular and a bit ad-hoc; it covered several runs over different admin/admax ranges, prime bounds P, and incr values. Search parameters shifted as benchmark results were produced, and I wasted much search time on unproductive ranges due to incorrect benchmark interpretation. A significant suboptimality of this run was spending too much time in polynomial selection; we could have searched the intended range in 1 day rather than 2.5, and it probably would have been worth it to spend only 12 hours searching less than the intended range.

The four stage-1 runs that produced every trial-sieved polynomial used 15,424 GPU-hours or 643 GPU-days. Their parameters are below:

| #     | P   | incr      | ad range         | GPU-hours | Polynomials | Root-optimized |
| ----- | --- | --------- | ---------------- | --------- | ----------- | -------------- |
| 1     | 2e7 | 110880    | 1.1e5 to 7.67e11 | 743.6     | 5,545,873   | 11,468         |
| 2     | 3e7 | 465585120 | 4.7e8 to 1.55e14 | 73.0      | 323,979     | 4,602          |
| 3     | 2e7 | 110880    | 1.1e5 to 1.87e12 | 4,352.9   | 29,976,690  | 399            |
| 4     | 3e7 | 110880    | 1.1e5 to 3.14e12 | 10,254.1  | 47,711,181  | 1,864          |
| Total |     |           |                  | 15,423.6  | 83,557,723  | 18,333         |

We trial sieved a total of 22 distinct polynomials, which can be found [attached here](https://cognition.com/documents/c260-polys.tar.gz). We tested the top 5 by MurphyE among those we had available as of 2026-08-19 10:53, which all came from the first two runs, then a further 17 selected as a union of the top 10 by each of four scores MurphyE, CADO’s E, E'sigma, and E'chi2 (from [David and Zimmermann](https://inria.hal.science/hal-02151093v4/document)) as of 2026-08-21 06:08:18, which came from runs 3 and 4.

After trial sieving, the polynomial we went with was:

```
Y0: -221673351566952308029695237213052836736183
Y1: 5766034074997040571677
c0: 4438326758963496161172848385157253702543453653246272
c1: -2760724998540198898516614911500562788411825980
c2: -3288611114230578563553198296458642435160
c3: 950383683194810225243935581823335
c4: 541831494549130032021283293
c5: -32669802676467106300
c6: -1863645537600
skew: 3226459.164
```

Some properties of this polynomial: degree 6, skew 3,226,459, MurphyE 6.633e-10 (Bf=2.749e11, Bg=1.374e11, area=8.59e18), alpha -9.57 (projective -2.38), side-1 lognorm 73.31, 6 real roots. (c260-r1 in the attachment).

The second-best trial-sieved polynomial (c260-r2) had MurphyE 6.215e-10 and gave 1-2% lower yield. The best of the first five by MurphyE (c260-p1) had 5.674e-10 and gave 13-16% lower yield.

### Sieving

Sieving began on 2026-08-22 at 10:01:12 UTC and reached target at 2026-08-30 07:26:15 UTC; the final workunits were uploaded 2026-08-30 at 07:32:17 UTC. Sieving wallclock time was 189.5 hours = 7.9 days. Sieving parameters were lpb0/lpb1 = 36/37 (candidate `lpb3637`), lim0 = lim1 = 2^31, mfb0/mfb1 = 72/111, ncurves 50/35, A = 33, sqside = 1. Special q range was q = 1.0e9 to about q = 3.91e10 (highest workunit ended at 39,091,320,000) for a total of 632,249 workunits. We used a special q range of 60,000 per workunit so they would take about 10 minutes.

Sieving was relatively drama-free. We upgraded the siever twice during this process, gaining 14-17% performance vs. the start of sieving. Measuring end-to-end workunit times as the median interval between consecutive uploads from one GPU, our GB200s went from 585 s to 486s, GB300s from 586s to 504s, and B200s from 628s to 541s. Overall sieving time was 3,813 GPU-days.

Here’s a chart showing sieving progress over time.

#### RSA-260 sieving progress

##### Workunits marked OK

Marked OK

target: 13.85B raw

##### Raw and unique relations

Raw, model Unique, model Raw, measured Unique, measured

Starting q = 1,000,000,000.

##### Yield per special-q: measured and model

Raw, model Unique, model Raw, measured Unique, measured

Ultimately, we obtained 13,849,985,589 raw relations for filtering (shy of the 13.85B target because four files were truncated after running out of disk space), which filtered down to 8,298,749,059 unique relations (40.1% duplicates), and 3,991,449 free relations.

### Linear algebra

Dedup/purge/merge/filter took 4 hours end-to-end (after a run crashed 2 hours in) on 176 threads of a 192-vCPU node and produced a 656,182,601 x 656,182,189 matrix with 98,431,741,898 nonzeros at density 150 per row. We gave some consideration to optimal merging and changed the target density over the course of sieving, but ultimately landed close to 150.

We optimized the linear algebra while sieving proceeded. Optimization was turbulent because we simultaneously co-optimized matrix params, solver layout, and code targeting speculative final matrix properties. For representative test inputs, we eventually converged on a few matrices formed using optimized `fake_rels`. This fed back into sieving: changes to linear algebra performance (and departure from trial sieving yield estimates) nudged the relation target a couple of times. It is unclear to me how far we were from optimal overall, but I halted optimization when a 48-hour linear algebra run seemed within reach.

Linear algebra began on 2026-08-30 15:45:10 UTC. Dispatch/prep/secure took 4.4 hours. `krylov` ran 2,564,096 iterations on each of two width-256 sequences (m = n = 512), taking 0.045 to 0.102 s per iteration depending on available compute. We saved checkpoints every 8,192 iterations and kept every fourth one to enable parallelizing `mksol`; we deleted the others after verification. `krylov` was done at 2026-09-02 02:38:14 UTC. `lingen` took 3.5 hours on a single GB200 node with 4 GPUs; multiple attempts were pre-empted so this phase took 7 hours overall. We ran 40 `mksol` ranges across two clusters; each ran on a 2x2 grid (16 GPUs) and took between 82 and 87 minutes. Gather took 9m04s on a 2x2 grid and wrote 64 kernel vectors.

To confirm the correctness of the optimized implementation, which had not been tested on any smaller scale before starting the run, we factored (11^331 - 1)/10 (C344) end-to-end while `krylov` ran. As another check, in parallel with `krylov`, we also ran CADO’s `bwccheck` over every V checkpoint pair. CADO’s normal in-`krylov` check went off about 14 hours into the run; we re-ran that interval and continued the run correctly.

I initially intended the linear algebra configuration to be m = n = 512 and two width-256 sequences on 16 GB200 nodes in a 4x4 MPI grid (64 GPUs each). I had planned on using dedicated cliques, but could not due to the requirements of higher-priority workloads, so proceeded with scattered compute as before. I expected some loss of performance due to running across InfiniBand, but the implementation does not fully utilize NVLink, so performance degradation was not too severe. Unlike sieving, linear algebra requires all of its workers to remain up. Consequently it was fatally preempted frequently and we had to develop a somewhat complex placement script to constantly fit the best shape possible to the available compute.

Here’s a chart showing allocations and progress over time:

#### RSA-260 linear algebra · August 30–September 2, 2026

Cluster 1 Cluster 2

##### Krylov sequence 0..256: nodes, MPI grid and NVLink domains

Hover or focus an allocation to inspect its node count, grid, domains, and time range.

##### Krylov sequence 256..512: nodes, MPI grid and NVLink domains

Hover or focus an allocation to inspect its node count, grid, domains, and time range.

##### Recorded Krylov checkpoints

Sequence 0..256 Sequence 256..512

Dots use recorded checkpoint write times. Final iteration: 2,564,096 for both sequences.

##### Other steps: nodes allocated by cluster

Cluster 1 Cluster 2

### Characters and square root

Normally this step requires relatively inconsiderable computational effort, but in the RSA-260 run took 12 hours end-to-end after running into some trouble. Due to the size of the rational product (1.76e11 bits), `sqrt` overflowed the `mpz_t` limb counter and aborted. Devin rebuilt `sqrt` three times (first using GMP’s `mpn` functions and then also parallel Karatsuba on CPU); the version that ultimately produced the factors used GPU-accelerated NTT multiplication and finished in 88 minutes. Factors were produced at 2026-09-03 01:48:57 UTC.

## [Appendix 3: All numbers factored](https://cognition.com/blog/factoring-rsa-260#appendix-3-other-numbers-factorized)

† = timestamp evaluated by file modification time

### 1\. C155, term in aliquot sequence 171648

```
35897832874755680820449985423390320593468589158596246321041362108197306669238431228407629716984730854707528618438239056757108766703839883694565332020649303
= 22021946164808393 (p17)
× 262417319699457621175161752791477017206038853130734538645633052531 (p66)
× 6211836830541364900526909822006571137609795674665615556271096661561407141 (p73)
```

[FactorDB](https://factordb.com/index.php?id=1100000009084066855)

| stage      | start (UTC)          | finish (UTC)         | wall time (s) | GPU-days | output                                                                                              |
| ---------- | -------------------- | -------------------- | ------------- | -------- | --------------------------------------------------------------------------------------------------- |
| polyselect | 2026-08-14 21:51:50  | 2026-08-14 21:57:40  | 350           | 0.29     | 375,600 candidates; 160,578 size-optimized; 72 root-optimized; 67 kept; selected MurphyE 3.228e-07  |
| sieve      | 2026-08-14 21:58:37  | 2026-08-14 22:04:16  | 339           | 0.42     | raw 164,161,253; unique 118,353,582; free 875,191                                                   |
| filtering  | 2026-08-14 22:04:16  | 2026-08-14 22:26:10  | 1,314         | 0.12     | purge 16,040,431x16,040,251, excess 180; merge 3,664,154 rows, 545,411,699 nonzeros (density 148.9) |
| krylov     | 2026-08-14 22:26:44† | 2026-08-14 23:02:59† | 2,175         | 0.07     | 116,000 iterations                                                                                  |
| lingen     | 2026-08-14 23:02:59† | 2026-08-14 23:06:57† | 238           | 0.02     | generator length 56,839                                                                             |
| mksol      | 2026-08-14 23:06:57† | 2026-08-14 23:22:07† | 910           | 0.08     | 15 partial-solution files                                                                           |
| gather     | 2026-08-14 23:22:07† | 2026-08-14 23:22:22  | 15            | 0.00     | 64 kernel vectors                                                                                   |
| characters | 2026-08-14 23:22:22  | 2026-08-14 23:45:56  | 1,414         | 0.13     | 28 non-zero dependencies                                                                            |
| sqrt       | 2026-08-14 23:45:56  | 2026-08-15 00:04:58  | 1,142         | 0.11     | \~7.88M pairs per dep; factors from 4th dep: p17 x p66 x p73                                        |
| total      | 2026-08-14 21:51:50  | 2026-08-15 00:04:58  | 7,988         | 1.2      |                                                                                                     |

#### Parameters

- polyselect: degree 5; P 390000; incr 420; admin 4620; admax 180000; adrange 12600; nq 156250; nrkeep 72; sopteffort 4; ropteffort 25; threads 22; GPU stage 1 (`polyselect-gps1`)

- sieve: I 14; sqside not set; lpb 31/31; lim 33000000/40000000; mfb 60/60; lambda 1.94/1.95; ncurves 15/15; qmin 5200000; qrange 50000; special q 5200000 to 29050000; rels\_wanted 164000000; las.threads 22; wutimeout 900

- filter+merge: target\_density 148.0; purge.keep 180; required\_excess 0.04; dup1, dup2 and merge settings not set

- linear algebra: krylov: m=64, n=64; one sequence 0-64; thr=2x4, no MPI, one B200 node; mm\_impl=cuda; interleaving=0; nullspace=left; checkpoints every 4,000 iterations. lingen: thr=2x4; mksol: 15 ranges of 4,000 iterations; gather: thr=2x4, one B200 node

- sqrt: nchar 50; sqrt.threads 1; CPU `sqrt`, dependencies in order

#### Polynomial

```
n: 35897832874755680820449985423390320593468589158596246321041362108197306669238431228407629716984730854707528618438239056757108766703839883694565332020649303
skew: 1079717.52
c0: 311415128652871829359026871832488890
c1: 6038213931379356671126098734089
c2: -52755893829252005178014
c3: -3979443941439656428
c4: -636741235170
c5: 963900
Y0: -183822426494573757202526169411
Y1: 799614469853922336864107
# MurphyE (Bf=2.147e+09,Bg=2.147e+09,area=6.979e+14) = 3.228e-07
# f(x) = 963900*x^5-636741235170*x^4-3979443941439656428*x^3-52755893829252005178014*x^2+6038213931379356671126098734089*x+311415128652871829359026871832488890
# g(x) = 799614469853922336864107*x-183822426494573757202526169411
```

### 2\. C157, term in aliquot sequence 171648

```
9094117661604772474513996307258881217012042571399291054272697101019113898735915143042712474990468911795356432593098151723862961657529605023713028245732813881
= 1373873899497528055698752141917005095037184057 (p46)
× 6619324863024763414167515910344780329208808737133855530598807493174878587238190154476062604506187361704747820033 (p112)
```

[FactorDB](https://factordb.com/index.php?id=1100000002703751305)

| stage      | start (UTC)          | finish (UTC)         | wall time (s) | GPU-days | output                                                                                              |
| ---------- | -------------------- | -------------------- | ------------- | -------- | --------------------------------------------------------------------------------------------------- |
| polyselect | 2026-08-14 13:39:58  | 2026-08-14 13:46:01  | 363           | 0.03     | 490,548 candidates; 210,537 size-optimized; 72 root-optimized; 59 kept; selected MurphyE 2.364e-07  |
| sieve      | 2026-08-14 13:46:59  | 2026-08-14 14:51:36  | 3,877         | —        | raw 170,017,806; unique 120,860,691; free 875,427                                                   |
| filtering  | 2026-08-14 14:51:36  | 2026-08-14 15:16:05  | 1,469         | 0.14     | purge 20,378,554x20,378,374, excess 180; merge 4,586,690 rows, 691,435,635 nonzeros (density 150.7) |
| krylov     | 2026-08-14 15:16:48† | 2026-08-14 15:36:31† | 1,183         | 0.11     | 145,000 iterations                                                                                  |
| lingen     | 2026-08-14 15:36:31† | 2026-08-14 15:42:08† | 337           | 0.03     | generator length 71,513                                                                             |
| mksol      | 2026-08-14 15:42:08† | 2026-08-14 16:05:54† | 1,426         | 0.13     | 15 partial-solution files                                                                           |
| gather     | 2026-08-14 16:05:54† | 2026-08-14 16:06:10  | 16            | 0.00     | 64 kernel vectors                                                                                   |
| characters | 2026-08-14 16:06:10  | 2026-08-14 16:34:47  | 1,717         | 0.16     | 26 non-zero dependencies                                                                            |
| sqrt       | 2026-08-14 16:34:47  | 2026-08-14 16:51:24  | 997           | 0.09     | \~10.13M pairs per dep; factors from 3rd dep: p46 x p112                                            |
| total      | 2026-08-14 13:39:58  | 2026-08-14 16:51:24  | 11,486        | \~0.70   |                                                                                                     |

#### Parameters

- polyselect: degree 5; P 480000; incr 420; admin 3360; admax 240000; adrange 12600; nq 156250; nrkeep 72; sopteffort 4; ropteffort 25; threads 22; GPU stage 1 (`polyselect-gps1`)

- sieve: I 14; sqside not set; lpb 31/31; lim 36000000/45000000; mfb 60/61; lambda 1.95/1.98; ncurves 15/15; qmin 7000000; qrange 10000; special q 7000000 to 34790000; rels\_wanted 170000000; las.threads 22; wutimeout not set

- filter+merge: target\_density 150.0; purge.keep 180; required\_excess 0.04; dup1, dup2 and merge settings not set

- linear algebra: krylov: m=64, n=64; one sequence 0-64; thr=2x4, no MPI, one B200 node; mm\_impl=cuda; interleaving=0; nullspace=left; checkpoints every 5,000 iterations. lingen: thr=2x4; mksol: 15 ranges of 5,000 iterations; gather: thr=2x4, one B200 node

- sqrt: nchar 50; sqrt.threads 1; CPU `sqrt`, dependencies in order

#### Polynomial

```
n: 9094117661604772474513996307258881217012042571399291054272697101019113898735915143042712474990468911795356432593098151723862961657529605023713028245732813881
skew: 4028941.249
c0: -30929648823185396741795692684080726360
c1: 1984799888640434257894945231826
c2: 32825749084108570009393021
c3: 197070756762860649
c4: -1019587382066
c5: 27720
Y0: -4731635427901989535771954621627
Y1: 94637504817809596577371
# MurphyE (Bf=2.147e+09,Bg=2.147e+09,area=9.395e+14) = 2.364e-07
# f(x) = 27720*x^5-1019587382066*x^4+197070756762860649*x^3+32825749084108570009393021*x^2+1984799888640434257894945231826*x-30929648823185396741795692684080726360
# g(x) = 94637504817809596577371*x-4731635427901989535771954621627
```

### 3\. C173, term in aliquot sequence 9708

```
16822729250245162197210786340073520345302701126309345909868690809883178067955616113069491365481002999485261589865849989569081865831418044294861652263678072140784922937770471
= 3676483960011861552959023833205172827558807236795952573316927262433 (p67)
× 4575765713442929453137801139482723088869543413891048184845232501463745899574413667558326370545261994463687 (p106)
```

[FactorDB](https://factordb.com/index.php?id=1100000000853023041)

| stage      | start (UTC)          | finish (UTC)         | wall time (s) | GPU-days | output                                                                                                 |
| ---------- | -------------------- | -------------------- | ------------- | -------- | ------------------------------------------------------------------------------------------------------ |
| polyselect | 2026-08-14 22:01:02  | 2026-08-14 22:10:39  | 577           | 0.05     | 1,435,616 candidates; 508,830 size-optimized; 200 root-optimized; 112 kept; selected MurphyE 1.966e-08 |
| sieve      | 2026-08-14 22:11:26  | 2026-08-15 00:33:47  | 8,541         | —        | raw 163,518,774; unique 121,605,989; free 453,452                                                      |
| filtering  | 2026-08-15 00:33:47  | 2026-08-15 00:51:43  | 1,076         | 0.10     | purge 59,123,902x59,123,742, excess 160; merge 14,262,604 rows, 2,427,731,780 nonzeros (density 170.2) |
| krylov     | 2026-08-15 00:54:13† | 2026-08-15 03:05:32† | 7,879         | 0.73     | 446,464 iterations                                                                                     |
| lingen     | 2026-08-15 03:05:32† | 2026-08-15 03:25:03† | 1,171         | 0.11     | generator length 222,852                                                                               |
| mksol      | 2026-08-15 03:25:03† | 2026-08-15 06:56:17† | 12,674        | 1.2      | 218 partial-solution files                                                                             |
| gather     | 2026-08-15 06:56:17† | 2026-08-15 06:57:30  | 73            | 0.01     | 64 kernel vectors                                                                                      |
| characters | 2026-08-15 06:57:30  | 2026-08-15 08:18:55  | 4,885         | 0.45     | 26 non-zero dependencies                                                                               |
| sqrt       | 2026-08-15 08:18:55  | 2026-08-15 09:18:48  | 3,593         | 0.33     | \~29.56M pairs per dep; factors from 3rd dep: p67 x p106                                               |
| total      | 2026-08-14 22:01:02  | 2026-08-15 09:18:48  | 40,666        | \~3.0    |                                                                                                        |

#### Parameters

- polyselect: degree 5; P 500000; incr 60; admin not set; admax 5000000; adrange 12600 (ad 12600 to 5000000 searched); nq 3125; nrkeep 200; sopteffort and ropteffort not set; threads 22; GPU stage 1 (`polyselect-gps1`)

- sieve: I 14; sqside not set; lpb 30/31; lim 48100000/67600000; mfb 60/90; lambda not set; ncurves 15/8; qmin 39200000; qrange 50000; special q 39200000 to 230900000; rels\_wanted not set; las.threads 22; wutimeout not set

- filter+merge: target\_density 170.0; purge.keep 160; required\_excess not set; dup1, dup2 and merge settings not set

- linear algebra: krylov: m=64, n=64; one sequence 0-64; thr=2x4, no MPI, one B200 node; mm\_impl=cuda; nullspace=left; checkpoints every 1,024 iterations. lingen: thr=2x4; mksol: 218 ranges of 1,024 iterations; gather: thr=2x4, one B200 node

- sqrt: nchar 50; sqrt.threads 1; CPU `sqrt`, dependencies in order

#### Polynomial

```
n: 16822729250245162197210786340073520345302701126309345909868690809883178067955616113069491365481002999485261589865849989569081865831418044294861652263678072140784922937770471
skew: 7539323.115
c0: 5507904840044862958567101790419123351248
c1: 11075662115103401666716027762474064
c2: 2069703995428638025176582672
c3: -460426871814604581272
c4: -10802939281763
c5: 1414980
Y0: -1640700354148067422687336044906519
Y1: 101559148972858506719
# MurphyE (Bf=2.147e+09,Bg=1.074e+09,area=5.261e+15) = 1.966e-08
# f(x) = 1414980*x^5-10802939281763*x^4-460426871814604581272*x^3+2069703995428638025176582672*x^2+11075662115103401666716027762474064*x+5507904840044862958567101790419123351248
# g(x) = 101559148972858506719*x-1640700354148067422687336044906519
```

### 4\. C175, Kamada near-repdigit table

```
5470451651282352577428602590986639243300312582317119394917414850088270252125425873202922100945580574919292893135207214884038940172525025590402945033754680230815866783742314989
= 26545519698456167532008255537577644047411055430442133 (p53)
× 206078152299293756900604204800302055899369808326285212990616910104280090482314900582700376408434332258183140974789810506233 (p123)
```

[FactorDB](https://factordb.com/index.php?id=1100000001294505256)

| stage      | start (UTC)          | finish (UTC)         | wall time (s) | GPU-days | output                                                                                                 |
| ---------- | -------------------- | -------------------- | ------------- | -------- | ------------------------------------------------------------------------------------------------------ |
| polyselect | 2026-08-16 00:32:51  | 2026-08-16 00:36:47  | 236           | 0.32     | 312,940 candidates; 114,948 size-optimized; 200 root-optimized; 67 kept; selected MurphyE 1.573e-08    |
| sieve      | 2026-08-16 00:37:33  | 2026-08-16 01:45:06  | 4,053         | 6.5      | raw 171,492,612; unique 123,419,402; free 453,705                                                      |
| filtering  | 2026-08-16 01:45:06  | 2026-08-16 02:03:40  | 1,114         | 0.10     | purge 64,040,726x64,040,566, excess 160; merge 15,667,159 rows, 2,669,749,178 nonzeros (density 170.4) |
| krylov     | 2026-08-16 02:06:26† | 2026-08-16 04:50:17† | 9,831         | 0.91     | 490,496 iterations                                                                                     |
| lingen     | 2026-08-16 04:50:17† | 2026-08-16 05:11:34† | 1,277         | 0.12     | generator length 244,798                                                                               |
| mksol      | 2026-08-16 05:11:34† | 2026-08-16 06:48:24† | 5,810         | 0.54     | 240 partial-solution files                                                                             |
| gather     | 2026-08-16 06:48:24† | 2026-08-16 06:49:43  | 79            | 0.01     | 64 kernel vectors                                                                                      |
| characters | 2026-08-16 06:49:43  | 2026-08-16 08:17:53  | 5,290         | 0.49     | 25 non-zero dependencies                                                                               |
| sqrt       | 2026-08-16 08:17:53  | 2026-08-16 08:56:50  | 2,337         | 0.22     | \~32.02M pairs per dep; factors from 2nd dep: p53 x p123                                               |
| total      | 2026-08-16 00:32:51  | 2026-08-16 08:56:50  | 30,239        | 9.2      |                                                                                                        |

#### Parameters

- polyselect: degree 5; P 1200000; incr 60; admin not set; admax 1250000; adrange 12600 (ad 12600 to 1250000 searched); nq 3125; nrkeep 200; threads 22; GPU stage 1 (`polyselect-gps1`)

- sieve: I 14; sqside not set; lpb 30/31; lim 48100000/67600000; mfb 60/90; ncurves 15/8; qmin 39200000; qrange 50000; special q 39200000 to 281800000; rels\_wanted 169766062; las.threads 22; wutimeout 900

- filter+merge: target\_density 170.0; purge.keep 160; required\_excess not set; dup1, dup2 and merge settings not set

- linear algebra: krylov: m=64, n=64; one sequence 0-64; thr=2x4, no MPI, one B200 node; mm\_impl=cuda; nullspace=left; checkpoints every 1,024 iterations. lingen: thr=2x4; mksol: 240 ranges of 1,024 iterations; gather: thr=2x4, one B200 node

- sqrt: nchar 50; sqrt.threads 8; CPU `sqrt`, dependencies in order

#### Polynomial

```
n: 5470451651282352577428602590986639243300312582317119394917414850088270252125425873202922100945580574919292893135207214884038940172525025590402945033754680230815866783742314989
skew: 6069782.59
c0: -67733679288880239798108684386132972016894
c1: 866375916432470715190517437912097
c2: 28292826923946390520259902850
c3: 257301599183566358927
c4: -398514986671596
c5: 5775840
Y0: -5635239997340677903984973751823056
Y1: 527256926047937086559
# MurphyE (Bf=2.147e+09,Bg=1.074e+09,area=5.261e+15) = 1.573e-08
# f(x) = 5775840*x^5-398514986671596*x^4+257301599183566358927*x^3+28292826923946390520259902850*x^2+866375916432470715190517437912097*x-67733679288880239798108684386132972016894
# g(x) = 527256926047937086559*x-5635239997340677903984973751823056
```

### 5\. C190, cofactor of 10^294 \* 106 - 1

```
2803449180324637751696097116917035415510283366000770209964656143125021698552328456230457085999381765506227676072887672270352769749338084027093323803706408761157245199388770494277027275902323
= 3031232029603751020573994806456966726044658734818549537566836351886379240410044896085021901 (p91)
× 924854697016087706590402161140219834859056267403120024108011627126585093503955645619014094737058623 (p99)
```

[FactorDB](https://factordb.com/index.php?id=1100000001288793722)

| stage      | start (UTC)          | finish (UTC)         | wall time (s) | GPU-days | output                                                                                                 |
| ---------- | -------------------- | -------------------- | ------------- | -------- | ------------------------------------------------------------------------------------------------------ |
| polyselect | 2026-08-17 02:17:16  | 2026-08-17 02:22:12  | 296           | 0.37     | 317,273 candidates; 108,110 size-optimized; 200 root-optimized; 35 kept; selected MurphyE 6.238e-09    |
| sieve      | 2026-08-17 02:25:10  | 2026-08-17 03:23:05  | 3,475         | 15       | raw 637,490,768; unique 558,502,707; free 1,693,435                                                    |
| filtering  | 2026-08-17 03:23:05  | 2026-08-17 05:30:38  | 7,653         | 0.71     | purge 95,381,750x95,381,590, excess 160; merge 23,489,464 rows, 4,020,264,141 nonzeros (density 171.2) |
| krylov     | 2026-08-17 05:31:52† | 2026-08-17 06:22:34† | 3,042         | 2.5      | 550,912 iterations                                                                                     |
| lingen     | 2026-08-17 06:22:34† | 2026-08-17 06:33:29† | 655           | 0.55     | generator length 367,013                                                                               |
| mksol      | 2026-08-17 06:33:29† | 2026-08-17 07:12:38† | 2,349         | 2.0      | 359 partial-solution files                                                                             |
| gather     | 2026-08-17 07:12:38† | 2026-08-17 07:14:44  | 126           | 0.11     | 64 kernel vectors                                                                                      |
| characters | 2026-08-17 07:15:08  | 2026-08-17 07:16:31  | 83            | 0.01     | 27 non-zero dependencies                                                                               |
| sqrt       | 2026-08-17 07:16:31  | 2026-08-17 07:34:56  | 1,105         | 0.10     | \~47.69M pairs per dep; factors from 2nd dep: p91 x p99                                                |
| total      | 2026-08-17 02:17:16  | 2026-08-17 07:34:56  | 19,060        | 22       |                                                                                                        |

#### Parameters

- polyselect: degree 5; P 1200000; incr 60; admin 0; admax 1250000; adrange 12600; nq 3125; nrkeep 200; threads 22; GPU stage 1 (`polyselect-gps1`)

- sieve: I 16; sqside not set; lpb 32/33; lim 340600000/162343750; mfb 85/96; ncurves 13/14; qmin 101875000; qrange 35000; special q 101875000 to 256235000; rels\_wanted 637470715; las.threads 22; wutimeout 5400

- filter+merge: target\_density 170.0; purge.keep 160; required\_excess not set; dup1, dup2 and merge settings not set

- linear algebra: krylov: `bwc.pl :complete`; m=128, n=64; one sequence 0-64; mpi=4x2, thr=1x1, 8 B200 nodes; mm\_impl=cuda; comm\_impl=nccl; nullspace=left; interleaving=0; checkpoints every 1,024 iterations; krylov\_jobs=1. lingen: mpi=1x1, thr=8x8; mksol: 359 ranges of 1,024 iterations, mpi=4x2; gather: mpi=4x2, thr=1x1, 8 B200 nodes

- sqrt: nchar 50; sqrt.threads 8; CPU `sqrt`, dependencies in order

#### Polynomial

```
n: 2803449180324637751696097116917035415510283366000770209964656143125021698552328456230457085999381765506227676072887672270352769749338084027093323803706408761157245199388770494277027275902323
skew: 21435826.965
c0: -228821101739770306666174403224295131380205560
c1: 100892241193110033273901788248298093162
c2: -5021540351247338802317304532661
c3: -473581781581809751070461
c4: -6741589944834600
c5: 184579200
Y0: -5121528411741386878578269803021899968
Y1: 5880053293624743921847
# MurphyE (Bf=8.590e+09,Bg=4.295e+09,area=2.188e+17) = 6.238e-09
# f(x) = 184579200*x^5-6741589944834600*x^4-473581781581809751070461*x^3-5021540351247338802317304532661*x^2+100892241193110033273901788248298093162*x-228821101739770306666174403224295131380205560
# g(x) = 5880053293624743921847*x-5121528411741386878578269803021899968
```

### 6\. C190, random semiprime

```
1915702666254754589302138082953064030136985233543617546393016850305362357282661012267312792578128353496405760484753165337002709486896975439203074537923793503963218174251187680660285478774247
= 41698471563777460657883793234606994934029658617745521302475971276954056473612781956799433416449 (p95)
× 45941795811980866357439455815073930815170459048464456198275523982412122564353572361237287120103 (p95)
```

[FactorDB](https://factordb.com/index.php?id=1100000009143796299)

| stage      | start (UTC)          | finish (UTC)         | wall time (s) | GPU-days | output                                                                                                 |
| ---------- | -------------------- | -------------------- | ------------- | -------- | ------------------------------------------------------------------------------------------------------ |
| polyselect | 2026-08-18 22:12:28  | 2026-08-18 22:17:29  | 301           | 0.47     | 317,811 candidates; 108,296 size-optimized; 200 root-optimized; 43 kept; selected MurphyE 2.236e-09    |
| sieve      | 2026-08-18 22:19:01  | 2026-08-18 22:56:56  | 2,275         | 7.0      | raw 328,861,343; unique 274,590,255; free 876,098                                                      |
| filtering  | 2026-08-18 22:56:56  | 2026-08-18 23:42:32  | 2,736         | 0.25     | purge 90,508,701x90,508,541, excess 160; merge 24,119,567 rows, 3,642,554,551 nonzeros (density 151.0) |
| krylov     | 2026-08-18 23:43:43† | 2026-08-19 00:24:56† | 2,473         | 2.1      | 573,440 iterations                                                                                     |
| lingen     | 2026-08-19 00:24:56† | 2026-08-19 00:36:01† | 665           | 0.55     | generator length 376,867                                                                               |
| mksol      | 2026-08-19 00:36:01† | 2026-08-19 01:04:14† | 1,693         | 1.4      | 47 partial-solution files                                                                              |
| gather     | 2026-08-19 01:04:14† | 2026-08-19 01:05:08  | 54            | 0.05     | 64 kernel vectors                                                                                      |
| characters | 2026-08-19 01:05:32  | 2026-08-19 01:06:49  | 77            | 0.01     | 27 non-zero dependencies                                                                               |
| sqrt       | 2026-08-19 01:06:49  | 2026-08-19 01:23:11  | 982           | 0.09     | \~45.25M pairs per dep; factors from 2nd dep: p95 x p95                                                |
| total      | 2026-08-18 22:12:28  | 2026-08-19 01:23:11  | 11,443        | 12       |                                                                                                        |

#### Parameters

- polyselect: degree 5; P 1200000; incr 60; admin 0; admax 1250000; adrange 12600; nq 3125; nrkeep 200; threads 22; GPU stage 1 (`polyselect-gps1`)

- sieve: I 16; sqside not set; lpb 31/32; lim 340600000/162343750; mfb 62/93; ncurves 13/14; qmin 101875000; qrange 45000; special q 101875000 to 255870000; rels\_wanted 328805148; las.threads 22; wutimeout 4800

- filter+merge: target\_density 150.0; purge.keep 160; required\_excess not set; dup1 nshards 16, outfmt .zst; dup2 concurrent; purge and merge gzip off

- linear algebra: krylov: `bwc.pl :complete`; m=128, n=64; one sequence 0-64; mpi=4x2, thr=1x1, 8 B200 nodes; mm\_impl=cuda; comm\_impl=nccl; simd=64; nullspace=left; checkpoints every 8,192 iterations; krylov\_jobs=1. lingen: mpi=1x1, thr=8x8; mksol: 47 ranges of 8,192 iterations, mpi=4x2; gather: mpi=4x2, thr=1x1, 8 B200 nodes

- sqrt: nchar 50; sqrt.threads 8; CPU `sqrt`, dependencies in order

#### Polynomial

```
n: 1915702666254754589302138082953064030136985233543617546393016850305362357282661012267312792578128353496405760484753165337002709486896975439203074537923793503963218174251187680660285478774247
skew: 69471962.165
c0: -8851052641249209046858885105626001014879976840
c1: 137284591523111942762317014521594293102
c2: 15711746557944118993342136575101
c3: -71479708151186561135983
c4: -2037361818341234
c5: 1484040
Y0: -4812639678831248968259250032120433579
Y1: 34259808457928473708459
# MurphyE (Bf=4.295e+09,Bg=2.147e+09,area=2.188e+17) = 2.236e-09
# f(x) = 1484040*x^5-2037361818341234*x^4-71479708151186561135983*x^3+15711746557944118993342136575101*x^2+137284591523111942762317014521594293102*x-8851052641249209046858885105626001014879976840
# g(x) = 34259808457928473708459*x-4812639678831248968259250032120433579
```

### 7\. C201, odd perfect number roadblock

```
930979809278937791072509028975014732977616551736470955539887227350102699241200870603629468473257201294317067671301934475214965923913736810157920028794236692842602151220424895418579020332545717523397343
= 192025012390228559221585986932909773176779292159023659696284779527 (p66)
× 4848221581608458738148347619143924968930459331699831038700672259359631705871477758619201973885803963022228656479922682484475622839880809 (p136)
```

[FactorDB](https://factordb.com/index.php?id=1100000000685451674)

| stage      | start (UTC)          | finish (UTC)         | wall time (s) | GPU-days | output                                                                                                   |
| ---------- | -------------------- | -------------------- | ------------- | -------- | -------------------------------------------------------------------------------------------------------- |
| polyselect | 2026-08-17 09:42:44  | 2026-08-17 09:54:47  | 723           | 0.07     | 317,323 candidates; 106,047 size-optimized; 200 root-optimized; 51 kept; selected MurphyE 2.581e-09      |
| sieve      | 2026-08-17 16:43:15  | 2026-08-17 18:21:37  | 5,902         | 43       | raw 637,475,661; unique 466,787,872; free 1,694,062                                                      |
| filtering  | 2026-08-17 18:21:37  | 2026-08-17 19:42:58  | 4,881         | 0.45     | purge 180,157,710x180,157,550, excess 160; merge 45,859,426 rows, 7,803,257,911 nonzeros (density 170.2) |
| krylov     | 2026-08-17 19:44:49† | 2026-08-17 22:35:07† | 10,218        | —        | 1,081,344 iterations                                                                                     |
| lingen     | 2026-08-17 22:35:07† | 2026-08-17 22:56:36† | 1,289         | —        | generator length 716,552                                                                                 |
| mksol      | 2026-08-17 22:56:36† | 2026-08-18 00:48:23† | 6,707         | —        | 88 partial-solution files                                                                                |
| gather     | 2026-08-18 00:48:23† | 2026-08-18 00:53:22† | 299           | —        | 64 kernel vectors                                                                                        |
| characters | 2026-08-18 00:53:30  | 2026-08-18 00:56:15  | 165           | 0.02     | 25 non-zero dependencies                                                                                 |
| sqrt       | 2026-08-18 00:56:15  | 2026-08-18 01:33:59  | 2,264         | 0.21     | \~90.07M pairs per dep; factors from 2nd dep: p66 x p136                                                 |
| total      | 2026-08-17 09:42:44  | 2026-08-18 01:33:59  | 57,075        | \~43     |                                                                                                          |

#### Parameters

- polyselect: degree 5; P 1200000; incr 60; admin 0; admax 1250000; adrange 12600; nq 3125; nrkeep 200; threads 22; GPU stage 1 (`polyselect-gps1`)

- sieve: I 16; sqside not set; lpb 32/33; lim 130000000/100000000; mfb 85/96; ncurves 13/13; qmin 309440000; qrange 40000; special q 309440000 to 812920000; imported relations for q 50000000 to 309440000; rels\_wanted 637470715; las.threads 22; wutimeout 5400

- filter+merge: target\_density 170.0; purge.keep 160; required\_excess not set; dup1 nshards 16, outfmt .zst; dup2 concurrent; purge and merge gzip off

- linear algebra: krylov: `bwc.pl :complete`; m=128, n=64; one sequence 0-64; mpi=4x3, thr=1x1, 12 B200 nodes; mm\_impl=cuda; comm\_impl=nccl; nullspace=left; checkpoints every 8,192 iterations; krylov\_jobs=1. lingen: mpi=1x1, thr=8x8; mksol: ranges of 8,192 iterations; gather: mpi=4x4, 16 GB200 nodes

- sqrt: nchar 50; sqrt.threads 8; CPU `sqrt`, dependencies in order

#### Polynomial

```
n: 930979809278937791072509028975014732977616551736470955539887227350102699241200870603629468473257201294317067671301934475214965923913736810157920028794236692842602151220424895418579020332545717523397343
skew: 141639179.53
c0: 4920071598665344600849898275027200375673687141584
c1: -17473086803075379177941148842054370989064
c2: -1895700168885924627964679424329691
c3: -5093363513389771937188279
c4: 65012146697554240
c5: 4586160
Y0: -959202297370196215605054183736005106510
Y1: 22769097491370535613
# MurphyE (Bf=8.590e+09,Bg=4.295e+09,area=1.074e+17) = 2.581e-09
# f(x) = 4586160*x^5+65012146697554240*x^4-5093363513389771937188279*x^3-1895700168885924627964679424329691*x^2-17473086803075379177941148842054370989064*x+4920071598665344600849898275027200375673687141584
# g(x) = 22769097491370535613*x-959202297370196215605054183736005106510
```

### 8\. RSA-260

```
22112825529529666435281085255026230927612089502470015394413748319128822941402001986512729726569746599085900330031400051170742204560859276357953757185954298838958709229238491006703034124620545784566413664540684214361293017694020846391065875914794251435144458199
= 4397328654844826923795068102505872571721883526553349659561256924505973939597593482272505698004801207988043088656411102133523080581 (p130)
× 5028695206842569864686141618253083416610081090075366674776775706538324961364412200138116378509733307971876652984898985905923678379 (p130)
```

[FactorDB](https://factordb.com/index.php?id=1100000000104374167)

| stage      | start (UTC)         | finish (UTC)        | wall time (s) | GPU-days | output                                                                                                                |
| ---------- | ------------------- | ------------------- | ------------- | -------- | --------------------------------------------------------------------------------------------------------------------- |
| polyselect | 2026-08-18 12:14:19 | 2026-08-21 06:08:18 | 237,239       | 643      | see text; selected MurphyE 6.633e-10                                                                                  |
| sieve      | 2026-08-22 10:01:12 | 2026-08-30 07:26:15 | 681,903       | 3,813    | raw entering filtering 13,849,985,589; unique 8,298,749,059; free 3,991,449                                           |
| filtering  | 2026-08-30 07:26:15 | 2026-08-30 13:23:05 | 21,410        | 0        | purge 2,238,133,184x2,238,133,024, excess 160; merge 656,182,601x656,182,189, 98,431,741,898 nonzeros (density 150.0) |
| prep       | 2026-08-30 15:45:10 | 2026-08-30 20:09:07 | 15,837        | 7.9      | dispatch, prep and secure                                                                                             |
| krylov     | 2026-08-30 20:11:37 | 2026-09-02 02:38:14 | 195,997       | 405      | 2,564,096 iterations per sequence                                                                                     |
| lingen     | 2026-09-02 02:40:30 | 2026-09-02 09:39:10 | 25,120        | 1.1      | generator length 1,281,607                                                                                            |
| mksol      | 2026-09-02 09:48:55 | 2026-09-02 12:25:16 | 9,381         | 53       | 40 partial-solution files                                                                                             |
| gather     | 2026-09-02 12:30:12 | 2026-09-02 12:39:16 | 544           | 0.11     | 64 kernel vectors                                                                                                     |
| characters | 2026-09-02 13:43:02 | 2026-09-02 14:34:41 | 3,099         | —        | 26 non-zero dependencies                                                                                              |
| sqrt       | 2026-09-02 14:34:41 | 2026-09-03 01:48:57 | 40,456        | —        | 1,119,082,750 pairs per dep; first factor from dep 12: p130 x p130                                                    |
| total      | 2026-08-18 12:14:19 | 2026-09-03 01:48:57 | 1,344,878     | \~4,923  |                                                                                                                       |

#### Parameters

- polyselect: GPU stage 1 (`polyselect-gps1`) on B200, GB200, and GB300 plus CADO polyselect; degree 6; P 2e7 and 3e7; incr 110880 and 465585120; ad 0 to above 1.04e16

- sieve: A 33; sqside 1; lpb 36/37; lim 2147483648/2147483648; mfb 72/111; ncurves 50/35; fill\_bands 2; host\_mr 1; qmin 1000000000; qrange 60000; special q 1000000000 to 39091320000; rels\_wanted 13460000000 initially, then 13850000000; las.threads 22; siqs.threads 2; wutimeout 7200

- filter+merge: target\_density 150.0; purge.keep 160; required\_excess not set; dup1 nshards 16, nslices\_log 4, outfmt .zst; dup2 concurrent; purge and merge gzip off

- linear algebra: krylov: m=512, n=512; two sequences of width 256; simd=256; nullspace=left; mm\_impl=cuda; comm\_impl=nccl; checkpoints every 8,192 iterations; checkpoints retained every 32,768 iterations for mksol; grids 4x2, 4x4, 8x4 and 16x4 of GB300 and GB200 nodes. lingen: a GB200 node; mksol: 40 ranges of 32,768 iterations, mpi=2x2, GB300 and GB200 nodes; gather: mpi=2x2, 4 GB200 nodes

- sqrt: nchar 50 on one B200 node; `-ab` pass on CPU; GPU `sqrt`, one dependency per node, dependencies 8 to 14

#### Polynomial

```
n: 22112825529529666435281085255026230927612089502470015394413748319128822941402001986512729726569746599085900330031400051170742204560859276357953757185954298838958709229238491006703034124620545784566413664540684214361293017694020846391065875914794251435144458199
skew: 3226459.164
c0: 4438326758963496161172848385157253702543453653246272
c1: -2760724998540198898516614911500562788411825980
c2: -3288611114230578563553198296458642435160
c3: 950383683194810225243935581823335
c4: 541831494549130032021283293
c5: -32669802676467106300
c6: -1863645537600
Y0: -221673351566952308029695237213052836736183
Y1: 5766034074997040571677
# MurphyE (Bf=2.749e+11,Bg=1.374e+11,area=8.590e+18) = 6.633e-10
# f(x) = -1863645537600*x^6-32669802676467106300*x^5+541831494549130032021283293*x^4+950383683194810225243935581823335*x^3-3288611114230578563553198296458642435160*x^2-2760724998540198898516614911500562788411825980*x+4438326758963496161172848385157253702543453653246272
# g(x) = 5766034074997040571677*x-221673351566952308029695237213052836736183
```

### 9\. C311 = (179^139 - 1)/178

```
78732003642300039104997556853283781324722722514871172820596350395008499128466382307395809435579611623474564094986667529892960864716597384448096503729214198851662899485730276355438419349226871332426916221394784977161223247509417343613224455688485860012856805177174377754441936853855717590652007641081003553555981
= 113099312568209673935042334303729415651026293476381594877312955562280961045355175671136840242009011468329 (p105)
× 696131584308411518623832257550002804001621961634551342461843289253649103892820497050633605105814786732064892829393341608140504002525639124360548840359891492626081616809277191319539440045658855879311157144389 (p207)
```

[FactorDB](https://factordb.com/index.php?id=1100000000007561365)

| stage      | start (UTC)          | finish (UTC)         | wall time (s) | GPU-days | output                                                                                                          |
| ---------- | -------------------- | -------------------- | ------------- | -------- | --------------------------------------------------------------------------------------------------------------- |
| sieve      | 2026-08-18 06:52:44  | 2026-08-18 10:39:42  | 13,618        | 63       | raw 1,237,126,812; unique 1,047,171,593; free 32,797,398                                                        |
| filtering  | 2026-08-18 10:39:42  | 2026-08-18 13:41:03  | 10,881        | 1.0      | purge 241,446,032x241,445,872, excess 160; merge 65,393,394x65,393,182, 11,172,358,980 nonzeros (density 170.8) |
| krylov     | 2026-08-18 13:42:47† | 2026-08-18 17:00:47† | 11,880        | 19       | 1,540,096 iterations                                                                                            |
| lingen     | 2026-08-18 17:00:47† | 2026-08-18 17:32:57† | 1,930         | 3.0      | generator length 1,021,770                                                                                      |
| mksol      | 2026-08-18 17:32:57† | 2026-08-18 20:24:26† | 10,289        | 16       | 125 partial-solution files                                                                                      |
| gather     | 2026-08-18 20:24:26† | 2026-08-18 20:26:31  | 125           | 0.20     | 64 kernel vectors                                                                                               |
| characters | 2026-08-18 20:26:35  | 2026-08-18 20:30:32  | 237           | 0.02     | 30 non-zero dependencies                                                                                        |
| sqrt       | 2026-08-18 20:30:32  | 2026-08-18 21:11:46  | 2,474         | 0.23     | \~120.72M pairs per dep; factors from 2nd dep: p105 x p207                                                      |
| total      | 2026-08-18 06:52:44  | 2026-08-18 21:11:46  | 51,542        | 102      |                                                                                                                 |

#### Parameters

- polynomial: f = 179 x^6 - 1, g = x - m, m = 179^23, skew 0.4213, MurphyE 8.836e-16 (179^139 - 1 = 178 N, 139 = 6\*23 + 1)

- alternatives: none recorded

- sieve: A 32; sqside 0; lpb 33/34; lim 500000000/800000000; mfb 66/99; ncurves 21/26; qmin 250000000; qrange 100000; special q 250000000 to 1184000000; rels\_wanted 1237056574; las.threads 22; wutimeout 6000

- filter+merge: target\_density 170.0; purge.keep 160; required\_excess not set; dup1 nshards 16, outfmt .zst; dup2 concurrent; purge and merge gzip off

- linear algebra: krylov: `bwc.pl :complete`; m=128, n=64; one sequence 0-64; mpi=4x4, thr=1x1, 16 B200 nodes; mm\_impl=cuda; comm\_impl=nccl; simd=64; nullspace=left; checkpoints every 8,192 iterations; krylov\_jobs=1. lingen: mpi=1x1, thr=8x8; mksol: 125 ranges of 8,192 iterations, mpi=4x4; gather: mpi=4x4, thr=1x1, 16 B200 nodes

- sqrt: nchar 50; sqrt.threads 8; CPU `sqrt`, dependencies in order

### 10\. C344 = (11^331 - 1)/10

```
50231805376049596631820685866210796229177935753938552911186809956414816600066561080962695239770426026723364698292328197470387911075676202539623423774798834486148386204131623950780297626046160114576438460656977450890504071673236180892236065865431442427746499804001930166673809250928496230908288781474418503703449380725550307006218226204967040981
= 4430846974881282458576305306275299267803558513190420498353196875372229299069071572187853350349351721816194984586423957772963949687017317 (p136)
× 11336840486890314794032557608959863028316193728266529081837586701486166437930282744836829943727770582091213875616984397275548049092194046001698425309465943848550352458828194439770494240269696008098233721116593 (p209)
```

[FactorDB](https://factordb.com/index.php?id=1000000000043590355)

| stage      | start (UTC)         | finish (UTC)        | wall time (s) | GPU-days | output                                                                                                            |
| ---------- | ------------------- | ------------------- | ------------- | -------- | ----------------------------------------------------------------------------------------------------------------- |
| sieve      | 2026-08-31 10:44:54 | 2026-09-01 00:54:48 | 50,994        | 148      | raw 1,632,935,487; unique 1,215,709,840; free 63,572,584                                                          |
| filtering  | 2026-09-01 00:54:48 | 2026-09-01 01:32:55 | 2,287         | 0.21     | purge 691,598,231x691,598,071, excess 160; merge 183,979,604x183,979,412, 31,557,886,647 nonzeros (density 171.5) |
| prep       | 2026-09-01 02:54:22 | 2026-09-01 04:04:05 | 4,183         | 0.80     | dispatch, prep and secure                                                                                         |
| krylov     | 2026-09-01 04:12:09 | 2026-09-01 20:32:53 | 58,844        | 16       | 718,848 iterations per sequence                                                                                   |
| lingen     | 2026-09-01 20:45:09 | 2026-09-01 21:39:39 | 3,270         | 0.15     | generator length 359,296                                                                                          |
| mksol      | 2026-09-01 21:46:45 | 2026-09-01 22:08:16 | 1,291         | 1.7      | 8 partial-solution files                                                                                          |
| gather     | 2026-09-01 22:12:43 | 2026-09-01 22:15:09 | 146           | 0.04     | 64 kernel vectors                                                                                                 |
| characters | 2026-09-01 22:17:35 | 2026-09-01 22:28:05 | 630           | 0        | 29 non-zero dependencies                                                                                          |
| sqrt       | 2026-09-01 22:28:05 | 2026-09-01 23:40:44 | 4,359         | 0        | \~345.8M pairs per dep; first factor from dep 20: p136 x p209                                                     |
| total      | 2026-08-31 10:44:54 | 2026-09-01 23:40:44 | 132,950       | 167      |                                                                                                                   |

#### Parameters

- polynomial: f = 11 x^6 - 1, g = x - m, m = 11^55, skew 0.6706, MurphyE 7.317e-17 (331 = 6\*55 + 1)

- alternatives: septic f = x^7 - 11^5, m = 11^48, and sextic f = x^6 - 11^5, m = 11^56, rejected on norm estimates, not by trial sieving

- sieve: A 32; sqside 0; lpb 34/34; lim 1073741824/1073741824; mfb 68/102; ncurves 25/35; qmin 300000000; qrange 100000; special q 300000000 to 3532000000; rels\_wanted 1632914677; las.threads 22; wutimeout 6000

- filter+merge: target\_density 170.0; purge.keep 160; required\_excess not set; dup1 nshards 16, nslices\_log 4, outfmt .zst; dup2 concurrent; purge and merge gzip off

- linear algebra: krylov: m=512, n=512; simd=256; two sequences of width 256; mpi=2x2, 4 GB200 nodes per sequence; mm\_impl=cuda; comm\_impl=nccl; nullspace=left; checkpoints every 1,024 iterations; checkpoints retained every 8,192 iterations for mksol. lingen: one GB200 node; mksol: 8 jobs, mpi=2x2, ranges cut at multiples of 8,192 iterations; gather: one GB200 node

- sqrt: nchar 50, 176 threads, and `-ab` pass on the CPU head; sqrt.threads 8; CPU `sqrt`, one dependency per node

### 11\. C337, cofactor of 2^1207 - 1

```
7121450524338129034228935888406290342440924878292924475154549706165967683018106989365212535726088820424187252154592086367126609115652316059905387197810171357101484623269226530219357641629634637632902818929376633641165995708421341209033069328278856607776384578328080846029713646218471064646270016968312691054210973840071649700163924918271
= 2135456634416723262926946022637259241391053741075352614983924508336321947951381676798676767 (p91)
× 3334860755101812561067390658542870185942902307642912877405776237023857839224225092829417050945007343491020348357930255766175507591130368636694557769027206776756787240691456180112351039576773125118453259495249395384279121506465565518011175872090913 (p247)
```

[FactorDB](https://factordb.com/index.php?id=1100000000002356256)

| stage      | start (UTC)         | finish (UTC)        | wall time (s) | GPU-days | output                                                                                                            |
| ---------- | ------------------- | ------------------- | ------------- | -------- | ----------------------------------------------------------------------------------------------------------------- |
| sieve      | 2026-09-01 03:25:02 | 2026-09-02 17:36:23 | 137,481       | 602      | raw 4,000,022,905; unique 2,809,594,239; free 123,347,484                                                         |
| filtering  | 2026-09-02 17:36:23 | 2026-09-02 20:33:23 | 10,620        | 0        | purge 875,233,659x875,233,499, excess 160; merge 262,840,014x262,839,822, 39,761,486,522 nonzeros (density 151.3) |
| prep       | 2026-09-02 21:16:54 | 2026-09-02 21:34:13 | 1,039         | 0.73     | dispatch, prep and secure                                                                                         |
| krylov     | 2026-09-02 21:36:37 | 2026-09-03 15:33:00 | 64,583        | 50       | 1,027,072 iterations per sequence                                                                                 |
| lingen     | 2026-09-03 15:36:32 | 2026-09-03 16:56:57 | 4,825         | 0.23     | generator length 513,361                                                                                          |
| mksol      | 2026-09-03 19:20:05 | 2026-09-03 23:01:42 | 13,297        | 5.2      | 32 partial-solution files                                                                                         |
| gather     | 2026-09-03 23:05:38 | 2026-09-03 23:09:27 | 229           | 0.05     | 64 kernel vectors                                                                                                 |
| characters | 2026-09-03 23:23:22 | 2026-09-03 23:36:26 | 784           | 0        | 29 non-zero dependencies                                                                                          |
| sqrt       | 2026-09-03 23:36:26 | 2026-09-04 00:16:08 | 2,382         | 0.70     | \~437.6M pairs per dep; first factor from dep 1: p91 x p247                                                       |
| total      | 2026-09-01 03:25:02 | 2026-09-04 00:16:08 | 247,866       | 658      |                                                                                                                   |

#### Parameters

- polynomial: f = 2 x^6 - 1, g = x - m, m = 2^201, skew 0.891, MurphyE 1.173e-17 (1207 = 6\*201 + 1)

- rejected by trial sieving, relations per 5,000-q window relative to the sextic at lpb 37/37, q = 2e9 / 8e9 / 32e9 / 128e9: octic x^8 - 2, m = 2^151: -55% / -57% / -66% / -71%; septic 8 x^7 - 1, m = 2^172: -4% / -17% / -22% / -32%; quintic 4 x^5 - 1, m = 2^241: -62% / -54% / -48% / -39%

- sieve: A 33; sqside 0; lpb 35/35; lim 2147483648/2147483648; mfb 105/70; ncurves 35/50; cofac\_gpu 1; ecm\_curves 24; qmin 1000000000; qrange 60000; special q 1000000000 to 7206840000; rels\_wanted 4000000000; las.threads 22; wutimeout 7200

- filter+merge: target\_density 150.0; purge.keep 160; required\_excess not set; dup1 nshards 16, nslices\_log 4, outfmt .zst; dup2 concurrent; purge and merge gzip off

- linear algebra: krylov: m=512, n=512; simd=128; four sequences of width 128; mpi=4x2, 8 GB200 nodes per sequence; mm\_impl=cuda; comm\_impl=nccl; nullspace=left; checkpoints every 1,024 iterations; checkpoints retained every 8,192 iterations for mksol. lingen: one GB200 node; mksol: 32 ranges of 16,384 iterations, mpi=2x2; gather: mpi=2x2, thr=1x1, 4 GB200 nodes

- sqrt: nchar 50 and `-ab` pass on the CPU head; GPU `sqrt` (`-side0 -side1 -gcd`), one dependency per node, 8 jobs

### 12\. C385, 2^1277 - 1 — a new record for SNFS

```
2601983048666099770481310081841021384653815561816676201329778087600902014918340074503059860433081046210605403488570251947845891562080866227034976651419330190731032377347305086443295837415395887618239855136922452802923419286887119716740625346109565072933087221327790207134604146257063901166556207972729700461767055550785130256674608872183239507219512717434046725178680177638925792182271
= 1724716499241899864602425389894492014760761058306262762892810886087012338823944627660237293943404138481321 (p106)
× 174090709548829862673444876509956283267433847880436775401923086006338309086502338849028139248274886760517707263111447 (p117)
× 8665849643335295674769105294571409283548491149467547745358388419546365219034102218289049713968532434665290528265538637566847289906584634645620272395224026405923633 (p163)
```

[FactorDB](https://factordb.com/index.php?id=1000000000000001277)

| stage      | start (UTC)         | finish (UTC)        | wall time (s) | GPU-days | output                                                                                                                                    |
| ---------- | ------------------- | ------------------- | ------------- | -------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| sieve      | 2026-09-03 04:42:11 | 2026-09-06 12:55:01 | 288,770       | 2,063    | raw 5,900,009,161; unique 3,661,079,985; free 123,347,484                                                                                 |
| filtering  | 2026-09-06 12:55:01 | 2026-09-06 14:38:04 | 6,183         | 0        | purge 1,988,203,513x1,988,203,353, excess 160; merge 579,167,442x579,167,250 (+32 dense columns), 87,097,397,544 nonzeros (density 150.4) |
| prep       | 2026-09-06 16:11:05 | 2026-09-06 16:32:18 | 1,273         | 0.73     | dispatch, prep and secure                                                                                                                 |
| krylov     | 2026-09-06 16:35:06 | 2026-09-08 02:36:22 | 122,476       | 324      | 2,263,040 iterations per sequence                                                                                                         |
| lingen     | 2026-09-08 02:51:12 | 2026-09-08 06:09:59 | 11,927        | 0.56     | generator length 1,131,188                                                                                                                |
| mksol      | 2026-09-08 06:12:12 | 2026-09-08 09:02:08 | 10,196        | 33       | 139 partial-solution files                                                                                                                |
| gather     | 2026-09-08 09:06:22 | 2026-09-08 09:20:37 | 855           | 0.34     | 64 kernel vectors                                                                                                                         |
| characters | 2026-09-08 09:23:15 | 2026-09-08 09:51:15 | 1,680         | 0        | 30 non-zero dependencies                                                                                                                  |
| sqrt       | 2026-09-08 09:51:15 | 2026-09-08 11:08:57 | 4,662         | 1.4      | 29,822,827,954 pairs in total (994,055,572 to 994,127,264 per dep); first factor from dep 5: p106 x p117 x p163                           |
| total      | 2026-09-03 04:42:11 | 2026-09-08 11:08:57 | 455,206       | 2,423    |                                                                                                                                           |

#### Parameters

- polynomial: f = x^6 - 2, g = x - m, m = 2^213, skew 1.1225, MurphyE 1.6164e-18 (1277 = 6\*213 - 1)

- rejected by trial sieving, relations per 5,000-q window relative to the sextic at lpb 37/37, q = 2e9 / 8e9 / 32e9 / 128e9: octic x^8 - 8, m = 2^160: -44% / -47% / -55% / -66%; septic 8 x^7 - 1, m = 2^182: +3% / -8% / -14% / -24%; quintic 4 x^5 - 1, m = 2^255: -73% / -70% / -63% / -54%

- sieve: A 33; sqside 0; lpb 36/35; lim 2147483648/2147483648; mfb 108/70; ncurves 35/50; cofac\_gpu 1; ecm\_curves 24; qmin 1000000000; qrange 60000; special q 1000000000 to 23627100000; rels\_wanted 5900000000; las.threads 22; wutimeout 7200

- filter+merge: target\_density 150.0; purge.keep 160; required\_excess 0.0; dup1 nshards 16, nslices\_log 4, outfmt .zst; dup2 concurrent; merge skip 32; purge and merge gzip off; 176 threads

- linear algebra: krylov: m=512, n=512; simd=128; four sequences of width 128; grids 7x1 to 16x4 of GB200 and GB300 nodes; mm\_impl=cuda; comm\_impl=nccl; nullspace=left; checkpoints every 1,024 iterations; checkpoints retained every 8,192 iterations for mksol. lingen: mpi=1x1, one GB300 node; dispatch: mpi=4x2, 8 GB300 nodes; mksol: solutions=0-64, mpi=4x2, 8 GB300 nodes per job; gather: mpi=4x2, 8 GB300 nodes

- sqrt: nchar 50, 176 threads, and `-ab` pass (`-t 8`) on the CPU head; GPU `sqrt` (`-side0 -side1 -gcd`), one dependency per node, 8 jobs for dependencies 0 to 7
