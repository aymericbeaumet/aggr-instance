---
title: Chinese frontier AI models carry the fingerprint of export-controlled GPUs
link: https://unemploymentlabs.com/checkpoint-forensics
source: hnrss-org
published: 2026-11-12T21:56:11Z
updated: 2026-11-12T21:56:11Z
first_seen: 2026-11-13T00:08:02.707796831Z
authors:
- firloop
content: extracted
html: 2026-11-12-chinese-frontier-ai-models-carry-the-fingerprint-of-export.html
remote_preview:
  url: https://unemploymentlabs.com/checkpoint-forensics-og.png?v=4
  alt: Table of ten models from DeepSeek, Moonshot AI, MiniMax and Ant Group. Each lab’s earlier model was created on NVIDIA Hopper and its newest models on NVIDIA Blackwell.
---

[Cite this report](https://unemploymentlabs.com/checkpoint-forensics#cite)

## Executive Summary

**Our forensic analysis of publicly released AI models finds evidence that DeepSeek, Moonshot AI, MiniMax and Ant Group have gained access to NVIDIA’s Blackwell GPUs, despite restrictions on exporting these chips to China, and are using that hardware to create their AI models.** Six models released by these companies since April 2026 contain numerical patterns matching Blackwell hardware, whereas earlier models from the same companies match NVIDIA’s older Hopper generation. The findings suggest that several leading Chinese AI developers have begun using Blackwell to train their newest models. U.S. officials and press reports had already alleged that DeepSeek and Moonshot AI were using Blackwell chips, without publishing technical evidence.

The evidence comes entirely from the models’ own published weights, rather than from leaks or insider accounts. We recover traces of the models’ initial values, and then search for the hardware configurations and initial configurations that reproduce them; for DeepSeek-V4-Flash and Kimi K3, we also confirmed the match on physical Blackwell chips. The fingerprint identifies how the weights were created and supports, without proving, the inference that Blackwell also performed the training. Specially configured AMD hardware, which is also restricted from export to China, is the only alternative we have identified; we have excluded it for DeepSeek-V4-Flash, and consider it very unlikely for the other five models. Notably, our findings provide technical evidence of access to advanced computing hardware, but do not establish where the chips were located, how access was obtained, or whether export controls were violated.

**Table 1. Hardware fingerprints in successive models from DeepSeek, Moonshot AI, MiniMax and Ant Group** [Link ↗](https://unemploymentlabs.com/checkpoint-forensics#table-1)

| Lab         | Model             | Released | Chip the model was started on      |
| ----------- | ----------------- | -------- | ---------------------------------- |
| DeepSeek    | DeepSeek-V3       | Dec 2024 | NVIDIA Hopper (lab disclosed H800) |
|             | DeepSeek-V4-Flash | Apr 2026 | • NVIDIA Blackwell B200/B300       |
|             | DeepSeek-V4-Pro   | Apr 2026 | • NVIDIA Blackwell GB200/GB300     |
| Moonshot AI | Kimi K2           | Jul 2025 | NVIDIA Hopper (lab disclosed H800) |
|             | Kimi K2.5         | Jan 2026 | NVIDIA Hopper (lab disclosed H800) |
|             | Kimi K3           | Jul 2026 | • NVIDIA Blackwell GB200/GB300     |
|             | Kimi K3.1-Pro     | Nov 2026 | • NVIDIA Blackwell GB200/GB300     |
| MiniMax     | MiniMax-M2.7      | Apr 2026 | NVIDIA Hopper                      |
|             | MiniMax-M3        | Jun 2026 | • NVIDIA Blackwell B200/B300       |
| Ant Group   | Ling-3.0-flash    | Aug 2026 | NVIDIA Hopper                      |
|             | Ling-3.1-flash    | Oct 2026 | • NVIDIA Blackwell GB200/GB300     |

*• Requires a U.S. license for export to China; none have been reported.*

*A Hopper fingerprint cannot distinguish an H800 from an H100 SXM or H200; the H800 identifications above are lab disclosures, not findings of this method.*

[Full list of models in Appendix 2 ↓](https://unemploymentlabs.com/checkpoint-forensics#appendix-2)

## Main Findings

1. **Six models from four Chinese labs read as Blackwell-trained.** DeepSeek-V4-Flash and MiniMax-M3 match NVIDIA’s Blackwell-class B200 or B300 chips, which have 148 processor cores (called *streaming multiprocessors*, or SMs). DeepSeek-V4-Pro, Kimi K3, Kimi K3.1-Pro and Ling-3.1-flash match the GB200 or GB300 chips, also Blackwell-class, which have 152 processor cores. Kimi K3.1-Pro starts from the same seed as K3, and may be a checkpoint of K3 with further post-training.
2. **Chinese labs started training on Blackwell relatively recently.** DeepSeek’s models read as Hopper-trained through DeepSeek-V3 (released in December 2024), and as Blackwell from DeepSeek V4 (released in April 2026). Moonshot’s models read as Hopper through Kimi K2.5 (January 2026) and as Blackwell from K3 (July 2026). One Moonshot model sits in between: Kimi K2.7-Code, posted six weeks before K3, is built on the Hopper-era weights but contains a small part that matches the B200 or B300. MiniMax’s models read as Hopper through M2.7 (April 2026) and as Blackwell from M3 (June 2026). Ant Group’s Ling-3.0-flash (August 2026) reads as Hopper, while its Ling-3.1-flash (October 2026) reads as Blackwell. The Hopper readings do not identify a specific chip: H800s, H100 SXMs and H200s share the same fingerprint.
3. **Not every Chinese AI lab has switched to Blackwell.** The newest models we could read from Alibaba (Qwen), Zhipu (GLM), Tencent, Xiaomi and StepFun all read as Hopper-trained. Meituan says its LongCat-2.0 was trained entirely on “AI ASIC superpods,” and we found no NVIDIA fingerprint in the model weights it’s released.
4. **The method gets known answers right.** For 24 specific models, the developer has publicly said what hardware was used. In every case, our method agrees with the public announcement. NVIDIA’s own Nemotron-3 Ultra, whose technical report names the GB200 for one phase of its training, reads as 152 SMs, the same as Kimi K3.
5. **The matches are far stronger than chance would explain.** Even unrelated numbers can appear to match when enough combinations are tried. Across the roughly 6 trillion seed-and-layout combinations in our search, the strongest chance match would be expected to reach about 8 sigma—a measure of how far a result stands above ordinary random variation. The strongest evidence for each of the six models ranges from 49 to 1,385 sigma. Even the weakest individual test we report reaches 20 sigma.
6. **The training runs were data-center scale.** From each model’s size and the amount of data it was trained on, we estimate that each run needed roughly 500 to 3,000 Blackwell GPUs for two to three months (Table 2), assuming each model was trained on the hardware that initialized it. The same arithmetic applied to DeepSeek-V3 gives 2,000 to 2,500 chips; DeepSeek disclosed 2,048. At roughly $70,000 per GPU—the cost implied by a Wall Street Journal report that about 36,000 Blackwell chips being installed in Malaysia for ByteDance’s use would cost more than $2.5 billion—the largest of these clusters would cost $150 million to $200 million, and the five together more than half a billion dollars. These figures cover only the final training runs; the labs’ total spending on export-controlled GPUs is likely in the billions of dollars.

**Table 2. Rough scale of each training run** [Link ↗](https://unemploymentlabs.com/checkpoint-forensics#table-2)

| Model             | Blackwell GPUs, best estimate | Range       |
| ----------------- | ----------------------------- | ----------- |
| DeepSeek-V4-Flash | 1,000–1,500                   | 250–4,500   |
| DeepSeek-V4-Pro   | 2,000–2,500                   | 750–6,000   |
| Kimi K3           | 2,000–3,000                   | 1,000–8,000 |
| MiniMax-M3        | 500–600                       | 200–2,000   |
| Ling-3.1-flash    | 1,100–1,650                   | 400–6,500   |

*Training compute (about 6 × active parameters × training tokens) divided by chip throughput at 35% utilization, over a run of two to three months (six to eight weeks for DeepSeek-V4-Flash). DeepSeek disclosed its token counts; for the other models, we estimated them from the weights.*

## Our investigation

A model’s *weights*, also called *parameters*, are the billions of adjustable numbers that determine how it processes information and produces answers. Training repeatedly adjusts these numbers; but before training can begin, they need initial values. In the software examined here, those values come from a pseudorandom number generation (PRNG) algorithm called Philox: Philox produces seemingly random numbers in a deterministic way. The starting input for Philox is a number called the “seed.” Given the same seed and settings, Philox produces the same numbers every time. This lets engineers repeat an experiment from the same starting point.

To fill billions of weights quickly, PyTorch, the software framework used in the development of most major AI models, divides the work that Philox does among many parallel workers on the GPU, called *threads*. (It does this through cuRAND, NVIDIA’s library for random number generation.) Each thread generates its own sequence of numbers from the seed and fills a particular set of positions. The number of threads then determines how those positions are shared out. On the NVIDIA chips examined here, a sufficiently large initialization uses the maximum of 2,048 threads for each processing unit, called a *streaming multiprocessor* (SM). A chip with more SMs therefore assigns the same job to more workers, changing where their numbers go.

**Figure 1** illustrates how this works on a simplified GPU with six SMs and eight threads per SM, giving 48 threads in total. Imagine the weights as a long row of empty boxes, wrapped into a grid to fit on the page. The first thread fills the first box, the second fills the second box, and so on through all 48 threads. The first thread’s next number then goes into box 49, its next into box 97, and so forth. The black outlines follow this one thread through the grid: the first thread fills every 48th position. The colors identify which processing unit supplies each set of values, while the brightness in the right-hand panel represents the value itself.

![Figure 1](https://unemploymentlabs.com/checkpoint-forensics/img/fig1.png)\
Figure 1. How a GPU fills the weights. This simplified example uses six processing units and eight threads per unit, for 48 threads in total. The outlined cells belong to thread 0, which fills every 48th position. Real chips use many more threads, but the placement rule is the same. [Link ↗](https://unemploymentlabs.com/checkpoint-forensics#figure-1)

Now keep the seed fixed, but use a GPU with five SMs instead of six. In our simplified example, that means 40 threads rather than 48. The first thread generates exactly the same sequence, but now puts its numbers in boxes 1, 41, 81, and so on, rather than boxes 1, 49, 97. **Figure 2** follows these numbers as their positions change. Across the grid, only 6 percent of the values match the original arrangement. The seed is right, but the worker count is wrong. **That is what makes the arrangement of model weights a clue to the chip used to create it.**

![Figure 2](https://unemploymentlabs.com/checkpoint-forensics/img/fig2.png)\
Figure 2. The same seed on a different GPU. With five processing units and 40 threads in this toy example, thread 0 generates the same sequence but fills every 40th position instead of every 48th. Only 6% of the displayed matrix matches the original arrangement. [Link ↗](https://unemploymentlabs.com/checkpoint-forensics#figure-2)

Reproducing the weights therefore requires two things to be right at once. The seed must generate the right numbers, and the thread count must put them in the right places. A correct seed with the wrong thread count mostly gives the right numbers in the wrong positions; a correct thread count with the wrong seed gives different numbers. **Figure 3** shows why *only the correct combination* recreates the original arrangement.

![Figure 3](https://unemploymentlabs.com/checkpoint-forensics/img/fig3.png)\
Figure 3. Both the seed and the layout must match. The original matrix is reproduced only when the seed and thread count are both correct. Changing either one destroys most of the match. The simplified chips represent a Blackwell B300 and a Hopper H100. Faded cells differ from the published weights. The numbered cells show where thread 0’s numbers land, and the strip beneath each panel shows the numbers themselves. [Link ↗](https://unemploymentlabs.com/checkpoint-forensics#figure-3)

On real chips, the thread counts are much larger, but the principle is the same. In the NVIDIA configurations examined here, the Hopper H100 SXM, H200 and H800 have 132 SMs and use 270,336 threads; the Blackwell B200/B300 have 148 SMs and use 303,104 threads; and the Blackwell GB200/GB300 have 152 SMs and use 311,296 threads. Recovering the thread count therefore tells us which of these hardware groups matches the starting arrangement, although it cannot distinguish chips within a group. A Hopper match, for example, cannot tell an H800 from an H100 or H200.

The question is whether enough of that arrangement survives training for us to recover it. Some weights, such as those reserved for words or symbols never encountered during training, can remain untouched and retain their starting values almost exactly. We can compare these preserved values directly with the starting values generated by a candidate seed and thread count. **Figure 4** shows such a comparison: DeepSeek-V3’s preserved weights match the Hopper arrangement, while DeepSeek-V4-Flash’s match the Blackwell arrangement.

![Figure 4](https://unemploymentlabs.com/checkpoint-forensics/img/fig4.png)\
Figure 4. Preserved starting values reveal the layout. Each dot compares a published weight with the value regenerated from the seed. A diagonal line means the values match. DeepSeek-V3 matches the Hopper layout, while V4-Flash matches the Blackwell layout. The panels show 4,096 values from four preserved rows per model. [Link ↗](https://unemploymentlabs.com/checkpoint-forensics#figure-4)

Even weights that training has changed can retain a faint trace of their starting values. Each final weight is its starting value plus all the adjustments made during training. If those adjustments do not completely cancel or overwhelm the starting differences, some of those differences survive. For example, two weights that begin at 0.2 and 0.8 and each increase by about 0.1 will still be ordered the same way afterward. Real adjustments vary much more, so one weight tells us little. But across hundreds of thousands of weights, we can test whether higher starting values still tend to correspond to higher final values. **Figure 5** shows this weak but measurable relationship. It is a finding in these weights, not a property guaranteed to survive every training run.

![Figure 5](https://unemploymentlabs.com/checkpoint-forensics/img/fig5.png)\
Figure 5. Trained weights retain a weaker trace. The left and middle panels average 745,472 weights per layer across 42 layers of DeepSeek-V4-Flash. Under the Blackwell layout, higher starting values still predict slightly higher final values (r = 0.066, 57 sigma); under Hopper, the relationship disappears. The right panel shows the weaker evidence from individual layers. [Link ↗](https://unemploymentlabs.com/checkpoint-forensics#figure-5)

These surviving traces let us work backward. Suppose, in an invented example using Figure 1’s layout, seed 7 makes the first thread produce 0.2, −0.5 and 0.8. With 48 threads, those values go into positions 1, 49 and 97; with 40 threads, they go into positions 1, 41 and 81. If the published weights preserve the first pattern, we can try candidate seeds and thread counts until we recreate it. In the actual investigation, we compare thousands of values in their corresponding positions. Untouched weights can match exactly; trained weights may show only the weaker resemblance described above. In either case, we look for the combination that best reproduces the surviving pattern.

We then test that combination on weights we kept out of the search. Having chosen a seed and thread count using one set of weights, we use them to predict the starting values of a different set, without changing either setting to improve the fit. A match on these previously unused weights is stronger evidence than a match confined to the weights that helped us choose the combination. Figure 6 shows the comparisons: DeepSeek-V3 matches the initialization layout of Hopper H100 SXM, H200 or H800 chips; DeepSeek-V4-Flash matches Blackwell B200/B300; and Kimi K3 matches Blackwell GB200/GB300.

![Figure 6](https://unemploymentlabs.com/checkpoint-forensics/img/fig6.png)\
Figure 6. Comparing candidate layouts. Each model has one clear peak: DeepSeek-V3 at 132 SMs, V4-Flash at 148, and Kimi K3 at 152. The other tested layouts score near zero. [Link ↗](https://unemploymentlabs.com/checkpoint-forensics#figure-6)

In our investigations, we found patterns strongly suggestive of initialization on Blackwell chips in six models from four Chinese companies: DeepSeek-V4-Flash and DeepSeek-V4-Pro from DeepSeek, Kimi K3 and Kimi K3.1-Pro from Moonshot AI, MiniMax-M3 from MiniMax, and Ling-3.1-flash from Ant Group. V4-Flash and MiniMax-M3 match the 148-SM layout; the other four match the 152-SM layout. These are fingerprints of the starting weights, which support, but do not by themselves prove, the use of Blackwell chips for training AI models.

## Limits of our investigation

Notably, the forensic method we used to identify the hardware on which these models were initialized only works for certain models. It needs published weights, so closed models cannot be checked. It also needs a recognizable, hardware-dependent pattern from the software that generated the starting weights. PyTorch on the GPUs examined here leaves such a pattern; Baidu’s PaddlePaddle framework, which was used to train Baidu’s ERNIE-5.5 model, does not. We were able to recover the seed for ERNIE-5.5 (2760), but could not identify which hardware was used to initialize it. Likewise, weights initialized on an ordinary processor rather than a GPU leave no GPU fingerprint; several of Alibaba’s Qwen3 models were initialized this way, and thus leave no trace of which GPUs were used to train them. We also found no fingerprint in Meituan’s LongCat-2.0, which the company says was trained on “AI ASIC superpods,” or in DeepSeek-V4.1-Flash, despite an exhaustive search.

The method identifies the hardware that generated the starting weights, not every chip that later helped train the model. When a training run creates its weights on the same GPUs that will train it, the fingerprint points to both. But a lab could generate the weights on one system and move them to another, or continue training an existing model on new hardware. The weights alone cannot rule out those possibilities. A mixed cluster introduces the same limitation. If NVIDIA chips generated some weights and Huawei chips later helped train them, the NVIDIA pattern could survive without revealing Huawei’s participation. Different NVIDIA generations could leave different patterns in the parts they initialized, if those traces survived. In Kimi K3, at least 64 separately seeded processes each left the same 152-SM layout. A Blackwell-compatible pattern therefore supports its use at initialization, subject to the AMD alternative below; it does not establish that the entire training run used Blackwell. A Hopper pattern likewise does not rule out later training on Blackwell.

The weights also cannot reveal who owned the chips or where they were located. Blackwell systems can be rented from data centers outside China. The evidence concerns access to the hardware; by itself, it does not establish that chips were smuggled into China or that an export-control violation occurred.

Could a different chip produce one of these Blackwell-compatible patterns? Among the NVIDIA configurations tested here, the older chips cannot: software can use fewer of a chip’s processing units, but cannot give it more than it physically has. Under the standard PyTorch behavior examined here, they cannot produce the 148- or 152-SM layouts. The China-market Blackwell graphics cards use a different number of threads per SM and do not produce either total.

AMD presents a different possibility. An MI300X normally uses 304 processing units, but it can be divided into two groups of 152 or configured to use only 148. Its random-number software can also generate the same underlying sequences as NVIDIA’s. Such configurations would be very unusual, but we confirmed on a physical MI300X that, limited to 152 or 148 of its units, it reproduces the layouts we associate with Blackwell, so the layout alone does not settle the question.

To distinguish them, we looked for small numerical differences in the values themselves. AMD and NVIDIA perform one calculation in the number-generating process slightly differently. On a physical MI300X, DeepSeek-V4-Flash’s preserved weights point to NVIDIA: in the 377 values where the two vendors’ results differ, the published weight matches NVIDIA’s in 363 and AMD’s in none. The same test could not settle the other five models, but we consider the AMD explanation very unlikely for them. In its normal configuration, an MI300X produces neither layout; a lab would have had to configure an entire cluster to use the same number of processing units as a Blackwell chip. Other public evidence also points to Blackwell: U.S. officials and press reports have alleged that DeepSeek and Moonshot AI were using Blackwell chips, and Ant Group’s public code includes benchmark results generated on a GB300.

## Verifying our findings

To reproduce the result that linked DeepSeek-V3 to Hopper and DeepSeek-V4-Flash to Blackwell, we ran unmodified PyTorch on a Hopper-class NVIDIA H100 and a Blackwell-class NVIDIA B300. We had identified DeepSeek’s seed number as 953049583, and found that it was used in the initialization of both DeepSeek-V3 and DeepSeek-V4-Flash. Using that seed, **the H100 reproduced the preserved starting values in DeepSeek-V3, but not V4-Flash; the B300 reproduced the preserved values in V4-Flash, but not V3.** On the B300, unmodified PyTorch reproduced 461 of the 469 selected rows in DeepSeek-V4-Flash with correlations above 0.99, including 445 entire rows that matched bit for bit. Eight selected rows were partly changed by training. On the H100, it reproduced all 57,344 preserved values in DeepSeek-V3 bit for bit. Neither chip reproduced the other model’s preserved rows. Changing the seed by one reproduced no matching rows on either machine. On a rented GB200, unmodified PyTorch likewise reproduced all 13 of Kimi K3’s preserved rows using Moonshot’s seed, 20256586; changing the seed by one reproduced none.

Our forensic method also gets the right answer when the answer is already known. There are 24 cases where a developer has publicly named the hardware behind a specific model; our forensic method agrees with every one. Meta’s Llama 3.1 models, trained on H100s, read as 132; eight of EleutherAI’s Pythia models, trained on A100s, read as 108. NVIDIA’s own Nemotron-3 Ultra, whose technical report names the GB200 for one phase of training, reads as 152, the same as Kimi K3. DeepSeek and Moonshot have reported H800 use for earlier models. Our Hopper readings are consistent with those disclosures, but would also be consistent with H100 SXM or H200 hardware; they do not independently verify H800 use.

When we compare the published weights with the numbers generated by a candidate seed and chip layout, some resemblance is inevitable even if the candidate is wrong. How much resemblance counts as evidence depends on how many weights we compare: agreement across a handful of numbers could easily be accidental, while agreement across thousands is much harder to explain by chance. Our statistical model accounts for the number of weights compared and measures how far the observed match exceeds the resemblance expected by chance. We express that distance in standard deviations, or *sigma*. A higher score means stronger evidence against an accidental match, provided the statistical model correctly describes the random variation in the data.

We must also account for how many candidates we tried. Searching roughly six trillion combinations of seed and layout gives us trillions of opportunities to find a lucky match. Under a normal-distribution model, a positive match of five sigma or more occurs by chance about once in 3.5 million comparisons. That would be striking in a single test, but unsurprising somewhere in a search as large as ours. We therefore use approximately eight sigma as our benchmark for assessing accidental matches across the full search.

**Our findings exceed that benchmark by a wide margin.** We first select a seed and layout using one set of weights, and then test that same combination on a separate set that played no part in choosing it. This checks whether the apparent fingerprint extends beyond the numbers that led us to it. The strongest evidence for each of the six models ranges from 49 to 1,385 sigma; even the weakest individual test reported reaches 20 sigma. Under our statistical model, these results are far too strong to attribute to accidental resemblance. They establish evidence of a shared initialization pattern; identifying precisely which computing hardware produced that pattern, and whether it also performed the training, requires the separate arguments discussed above.

## Conclusion

Based on our forensic investigation, we believe that **we have strong evidence that six Chinese AI models were initialized on export-controlled NVIDIA hardware, and that for DeepSeek-V4-Flash the evidence is conclusive**; we believe that this initialization also strongly suggests that they trained on the same export-controlled hardware. While this leaves unanswered other questions—such as the circumstances through which the companies that developed these models acquired export-controlled NVIDIA systems—we believe that this fact deserves urgent attention from the public.

Appendix 1: Each Chinese lab’s hardware over time Hardware fingerprints across successive models from each lab.

DeepSeek reuses one seed across model generations, and Moonshot uses one rule to derive its seeds, so we can follow each lab’s hardware from model to model. Dates are when the weights were posted.

| Model                                           | SMs                     | Hardware class                                    | Export to China                 | Evidence                                             |
| ----------------------------------------------- | ----------------------- | ------------------------------------------------- | ------------------------------- | ---------------------------------------------------- |
| DeepSeek                                        |                         |                                                   |                                 |                                                      |
| V2-Lite, MoE-16B                                | 108                     | A100 / A800 (Ampere)                              | A800 legal until Nov 2023       | Exact copy                                           |
| LLM 7B/67B, Coder, V2, V3 (Dec 2024), V3.2      | 132                     | H100 / H800 / H200 (Hopper)                       | Chip not identified             | Exact copy; V3 239σ                                  |
| V4-Flash (Apr 2026)                             | 148                     | B200 / B300 (Blackwell)                           | License required; none reported | Exact copy, 1,385σ                                   |
| V4-Pro (Apr 2026)                               | 152                     | GB200 / GB300 (Grace-Blackwell)                   | License required; none reported | Partial survival, 49σ                                |
| Moonshot AI                                     |                         |                                                   |                                 |                                                      |
| Kimi K2 (Jul 2025)                              | 132                     | H100 / H800 / H200 (Hopper)                       | Chip not identified             | Partial survival, 292σ                               |
| Moonlight-16B; Kimi-Linear (Oct 2025)           | 132                     | H100 / H800 / H200 (Hopper)                       | Chip not identified             | Exact copy; Kimi-Linear 763σ                         |
| Kimi K2.5 (Jan 2026)                            | 132                     | H100 / H800 / H200 (Hopper)                       | Chip not identified             | Untrained rows, r 0.9975, 255σ                       |
| Kimi K2.7-Code (Jun 2026)                       | 132, plus a 148-SM part | Hopper, plus a second source matching B200 / B300 | Mixed                           | 148 part: 254 rows, 246σ after removing the 132 part |
| Kimi K3 (Jul 2026), K3.1-Pro (Nov 2026)         | 152                     | GB200 / GB300 (Grace-Blackwell)                   | License required; none reported | Exact copy, 305σ each                                |
| MiniMax                                         |                         |                                                   |                                 |                                                      |
| Text-01 (Jan 2025), M1, M2 to M2.7 (Apr 2026)   | 132                     | H100 / H800 / H200 (Hopper)                       | Chip not identified             | Untrained rows; one of 2,048 grids                   |
| M3 (Jun 2026), M3.1-Flash (Oct 2026)            | 148                     | B200 / B300 (Blackwell)                           | License required; none reported | Exact copy, 323σ                                     |
| Ant Group                                       |                         |                                                   |                                 |                                                      |
| Ling-1T† (Oct 2025), Ling-3.0-flash (Aug 2026)  | 132                     | H100 / H800 / H200 (Hopper)                       | Chip not identified             | Untrained rows, r 0.98; 345σ, 193σ                   |
| Ling-3.1-flash (Oct 2026)                       | 152                     | GB200 / GB300 (Grace-Blackwell)                   | License required; none reported | Untrained rows, r 0.98, 244σ                         |
| Other labs                                      |                         |                                                   |                                 |                                                      |
| GLM-5.3 (Zhipu), Qwen3.8 (Alibaba)              | 132                     | H100 / H800 / H200 (Hopper)                       | Chip not identified             | Partial survival, 46σ, 478σ                          |
| Hy4-preview (Tencent), Step-5-Preview (StepFun) | 132                     | H100 / H800 / H200 (Hopper)                       | Chip not identified             | Exact copy, 256σ each                                |
| MiMo-V2.5-Pro (Xiaomi), Qwen3.5-397B (Alibaba)  | 132                     | H100 / H800 / H200 (Hopper)                       | Chip not identified             | Partial survival, 41σ, 323σ                          |

*How to read the evidence column. Exact copy: weights that training never updated reproduce the generated values with a correlation (r) above 0.999. Untrained rows: the same, for models whose starting values were clipped or rounded, which lowers r. Partial survival: trained weights keep part of their starting values. Figures from grid scans use cells held out of the search. Chance could not reach 8σ even after searching every 32-bit seed and every grid; the lowest of the six Blackwell results, on V4-Flash’s trained attention weights, is 20σ.*

*† Result from one analysis pass, not yet re-run.*

*Kimi K2.7-Code. Its 254 reserved rows are a blend of two sources: about 80% are the Hopper-layout values inherited from K2.5, and about 20% are a new draw that matches the 148-SM layout. The blend is the same in every row, which points to a merged checkpoint rather than mixed hardware; K2.7-Code is not counted among the six Blackwell models.*

*Public code. Ant Group’s cuLA repository includes benchmark results generated on a GB300 on March 31, 2026. In June 2025 a DeepSeek engineer wrote on GitHub that “we don’t have SM100 GPUs” (SM100 is Blackwell).*

Appendix 2: Every model identified Complete model catalogue, disclosed hardware, and evidence notes.

Our readings are consistent with all 24 cases where a developer has publicly stated the hardware for a specific model: 21 from our original survey, plus Kimi K2.5 (H800), Step-3.5-Flash (H800) and ZAYA1 (AMD MI300X). Eleven of these are step-zero checkpoints, and related models are not independent, so this is a consistency check rather than a measured accuracy rate. “None found” means a search of the model’s card, paper and repository history turned up no statement about hardware.

| Model                                      | Signature             | Developer   | Publicly disclosed hardware    | Agrees       | Export status (China)      |
| ------------------------------------------ | --------------------- | ----------- | ------------------------------ | ------------ | -------------------------- |
| China                                      |                       |             |                                |              |                            |
| DeepSeek-V4-Flash, -Base                   | 148: B200/B300        | DeepSeek    | Not disclosed                  | —            | License required²          |
| DeepSeek-V4-Pro-Base                       | 152: GB200/GB300      | DeepSeek    | Not disclosed                  | —            | License required²          |
| Kimi K3; K3.1-Pro (same seed)              | 152: GB200/GB300      | Moonshot AI | Not disclosed                  | —            | License required²          |
| Ling-3.1-flash                             | 152: GB200/GB300      | Ant Group   | None found                     | —            | License required²          |
| MiniMax-M3; M3.1-Flash (same init)         | 148: B200/B300        | MiniMax     | None found                     | —            | License required²          |
| Kimi K2.7-Code                             | 132, plus a 148 part⁵ | Moonshot AI | None found                     | —            | Unresolved⁴                |
| Kimi K2.5; K2.6 (same init)                | 132: Hopper           | Moonshot AI | H800                           | Yes          | Unresolved⁴                |
| Kimi-Linear-48B                            | 132: Hopper           | Moonshot AI | None found                     | —            | Unresolved⁴                |
| Kimi K2-Base                               | 132: Hopper           | Moonshot AI | H800                           | Yes          | Unresolved⁴                |
| Moonlight-16B-A3B                          | 132: Hopper           | Moonshot AI | Not disclosed                  | —            | Unresolved⁴                |
| DeepSeek-V3-Base; V3.2 (same init)         | 132: Hopper           | DeepSeek    | H800                           | Yes          | Unresolved⁴                |
| DeepSeek-V2                                | 132: Hopper           | DeepSeek    | H800 (report-wide)             | Consistent¹  | Compliant variant existed³ |
| DeepSeek-Coder-1.3B                        | 132: Hopper           | DeepSeek    | A100 and H800 (report-wide)    | Consistent¹  | Compliant variant existed³ |
| DeepSeek-LLM-7B, 67B                       | 132: Hopper           | DeepSeek    | Not disclosed for original run | —            | Compliant variant existed³ |
| DeepSeek-MoE-16B                           | 108: A100/A800        | DeepSeek    | A100 or H800 (report-wide)     | Consistent¹  | Compliant variant existed³ |
| DeepSeek-V2-Lite                           | 108: A100/A800        | DeepSeek    | H800 (report-wide)             | Shared init¹ | Compliant variant existed³ |
| Ling-3.0-flash                             | 132: Hopper           | Ant Group   | None found                     | —            | Unresolved⁴                |
| Ling-1T; Ling-2.6-1T (same init)†          | 132: Hopper           | Ant Group   | Hopper (report-wide)           | Consistent¹  | Unresolved⁴                |
| GLM-5, 5.1, 5.2, 5.3 (one init)            | 132: Hopper           | Zhipu AI    | Not disclosed                  | —            | Unresolved⁴                |
| GLM-5.3-Flash                              | 132: Hopper           | Zhipu AI    | None found                     | —            | Unresolved⁴                |
| GLM-4.5-Base; GLM-4.7 (same init)†         | 132: Hopper           | Zhipu AI    | None found                     | —            | Unresolved⁴                |
| Qwen3.8-2.4T-A95B                          | 132: Hopper           | Alibaba     | Not disclosed                  | —            | Unresolved⁴                |
| Qwen3.5-397B-A17B                          | 132: Hopper           | Alibaba     | None found                     | —            | Unresolved⁴                |
| Hy3-preview; Hy3 (same init)               | 132: Hopper           | Tencent     | None found                     | —            | Unresolved⁴                |
| Hy4-preview                                | 132: Hopper           | Tencent     | None found                     | —            | Unresolved⁴                |
| MiMo-V2-Flash; MiMo-V2.5-Pro               | 132: Hopper           | Xiaomi      | None found                     | —            | Unresolved⁴                |
| Step-5-Preview                             | 132: Hopper           | StepFun     | None found                     | —            | Unresolved⁴                |
| Step-3.5-Flash; Step-3.7-Flash (same init) | 132: Hopper           | StepFun     | H800 (Step-3.5-Flash)          | Yes          | Unresolved⁴                |
| MiniMax-M2, M2.1, M2.5, M2.7 (one init)    | 132: Hopper           | MiniMax     | None found                     | —            | Unresolved⁴                |
| MiniMax-Text-01, VL-01; M1 (one init)      | 132: Hopper           | MiniMax     | H800 (report-wide)             | Consistent¹  | Unresolved⁴                |
| Seed-OSS-36B-Base                          | 108: A100/A800        | ByteDance   | None found                     | —            | Compliant variant existed³ |
| Yi-34B                                     | 108: A100/A800        | 01.AI       | Not disclosed                  | —            | Compliant variant existed³ |
| United States                              |                       |             |                                |              |                            |
| Llama 3.1 8B, 70B, 405B                    | 132: Hopper           | Meta        | H100                           | Yes          | n/a                        |
| Nemotron-3-Ultra-550B                      | 152: GB200/GB300      | NVIDIA      | GB200 (one phase)              | Consistent   | n/a                        |
| Nemotron-4-340B-Base                       | 132: Hopper           | NVIDIA      | H100 SXM5                      | Yes          | n/a                        |
| ZAYA1-74B-preview                          | 304 CU: AMD MI300X    | Zyphra      | AMD (MI300X per its blog)      | Yes          | n/a                        |
| OLMo-2 32B, Olmo-3 32B                     | 132: Hopper           | AI2         | H100                           | Yes          | n/a                        |
| OLMo-7B                                    | 108: A100             | AI2         | A100                           | Yes          | n/a                        |
| OLMo-7B-Twin-2T                            | 110 CU: AMD MI250X    | AI2         | AMD MI250X                     | Yes          | n/a                        |
| OLMo-7B-0424, OLMo-1B-0724                 | 132: Hopper           | AI2         | Not disclosed                  | —            | n/a                        |
| Pythia 70M–12B (8 models)                  | 108: A100             | EleutherAI  | A100                           | Yes (all 8)  | n/a                        |
| GPT-NeoX-20B                               | 108: A100             | EleutherAI  | A100                           | Yes          | n/a                        |
| Europe                                     |                       |             |                                |              |                            |
| Apertus-70B                                | 132: Hopper           | Swiss AI    | GH200 (Hopper, 132 SMs)        | Yes          | n/a                        |
| Mixtral-8x7B                               | 132: Hopper           | Mistral AI  | Not disclosed                  | —            | n/a                        |
| BLOOM-176B (partial signature)             | 108: A100             | BigScience  | A100                           | Yes          | n/a                        |

¹ Report-wide disclosure. DeepSeek-V2 and V2-Lite share one “H800 cluster” sentence, MoE-16B says “A100 or H800,” and Coder says “A100 and H800.” MiniMax’s MiniMax-01 paper describes a cluster of H800s, and Ant Group’s Ling 2.0 report says its training ran on “Hopper GPUs.” These are consistent with our readings but are not independent tests. V2-Lite reuses MoE-16B’s initialization: their preserved rows are byte-identical.

² License required. The B200, B300, GB200 and GB300 have needed a U.S. export license for China since before they shipped, and none have been reported. The AMD MI300X is also controlled.

³ Compliant variant existed. The A100/H100 class was controlled from October 2022, but the A800 and H800, which have the same SM counts, could be exported until October 2023.

⁴ Unresolved Hopper hardware. These models were first published from December 2024 onward and show a 132-SM fingerprint compatible with H800, H100 SXM or H200 hardware. DeepSeek and Moonshot have reported H800 use for some models, but the fingerprint cannot distinguish these chips. A model’s release date does not establish when or where its hardware was acquired, and this reading alone does not establish export-control compliance.

⁵ 148 part. See the note on Kimi K2.7-Code under the results table.

† Reported by one analysis pass and not yet independently re-run. In every other row added for this brief, the first model named was re-run independently; models listed as sharing its initialization were not.

These flags describe the hardware class under the U.S. export rules of October 2022 and October 2023. They are not a finding that any law was broken.

No GPU signature recovered. A blank says nothing about a model’s hardware. We recovered no signature from DeepSeek-V4.1-Flash (after an exhaustive search), Qwen3.5-122B, Qwen3.8-Flash-Next, Qwen3-0.6B and 1.7B, gpt-oss-120b and Grok-1. Qwen3-235B, Qwen3-32B and Qwen3.5-9B were initialized on the CPU, which leaves no GPU layout. Baidu’s ERNIE-5.5 uses PaddlePaddle, whose generator carries no chip layout (we recovered its seed, 2760); ERNIE-4.5 gave no result. Meituan’s LongCat-2.0 gave no result. Its model card says training ran “entirely on AI ASIC superpods,” and a since-deleted fine-tuning configuration sets device\_arch to 910 with NPU and HCCL options, which we read as Huawei Ascend 910. Intern-S2 is built on Alibaba’s Qwen3.5-397B and carries its initialization. DBRX and Llama-4-Scout could not be downloaded.

Availability. The reproduction scripts, row fixtures and test outputs are available to reviewers on request.

Contact: Will DePue, will@unemploymentlabs.com.

## Sources

- The Information, Dec. 10, 2025, on DeepSeek and Blackwell ([via Bloomberg](https://news.bgov.com/tech-and-telecom-law/deepseek-has-used-banned-nvidia-chips-the-information-says-2)).
- Reuters, Feb. 23, 2026, [“China’s DeepSeek trained AI model on Nvidia’s best chip despite US ban, official says”](https://whbl.com/2026/02/23/exclusive-chinas-deepseek-trained-ai-model-on-nvidias-best-chip-despite-us-ban-official-says/) (syndicated copy).
- Michael Kratsios, White House OSTP director, July 22, 2026, on Moonshot AI ([via Bloomberg](https://news.bgov.com/artificial-intelligence/white-house-official-says-moonshot-accessed-banned-nvidia-chips)).
- The Wall Street Journal, March 2026, on Blackwell chips in Malaysia for ByteDance’s use ([via Reuters](https://www.thestandard.com.hk/finance/article/326579/)).
- DeepSeek engineer’s GitHub comment, June 12, 2025: [DeepGEMM pull request 112](https://github.com/deepseek-ai/DeepGEMM/pull/112).
- Ant Group’s GB300 benchmark results, March 31, 2026: [cuLA, BENCHMARK\_GB300.md](https://github.com/inclusionAI/cuLA/blob/main/BENCHMARK_GB300.md).
- U.S. export licensing rule: [15 CFR 742.6](https://www.ecfr.gov/current/title-15/subtitle-B/chapter-VII/subchapter-C/part-742/section-742.6).
- Disclosed hardware: [DeepSeek-V3 Technical Report](https://arxiv.org/abs/2412.19437); [Kimi K2 paper](https://arxiv.org/abs/2507.20534); [Nemotron-3 Ultra Technical Report](https://research.nvidia.com/labs/nemotron/files/NVIDIA-Nemotron-3-Ultra-Technical-Report.pdf).
- PyTorch code that sets the thread layout: [DistributionTemplates.h](https://github.com/pytorch/pytorch/blob/main/aten/src/ATen/native/cuda/DistributionTemplates.h).
- Model weights: [DeepSeek-V3](https://huggingface.co/deepseek-ai/DeepSeek-V3-Base), [V4-Flash](https://huggingface.co/deepseek-ai/DeepSeek-V4-Flash), [V4-Pro](https://huggingface.co/deepseek-ai/DeepSeek-V4-Pro), [Kimi K3](https://huggingface.co/moonshotai/Kimi-K3), [K3.1-Pro](https://huggingface.co/moonshotai/Kimi-K3.1-Pro), [MiniMax-M3](https://huggingface.co/MiniMaxAI/MiniMax-M3), [Ling-3.1-flash](https://huggingface.co/inclusionAI/Ling-3.1-flash).

## How to cite this report

Suggested citation:

Will DePue, “Top Chinese Frontier AI Models Carry the Fingerprint of Export-Controlled NVIDIA Blackwell GPUs,” Unemployment Labs, November 12, 2026, https://unemploymentlabs.com/checkpoint-forensics.

BibTeX:

```
@techreport{depue2026blackwell,
  author      = {DePue, Will},
  title       = {{Top Chinese Frontier AI Models Carry the Fingerprint of Export-Controlled NVIDIA Blackwell GPUs}},
  institution = {Unemployment Labs},
  year        = {2026},
  month       = nov,
  url         = {https://unemploymentlabs.com/checkpoint-forensics}
}
```

In news coverage, please attribute the findings to Will DePue and link to this page.
