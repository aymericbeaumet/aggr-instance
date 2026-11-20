---
title: What Comes After AGI?
link: https://institute.deepmind.com/essays/what-comes-after-agi/
source: hnrss-org
published: 2026-11-20T02:44:13Z
updated: 2026-11-20T02:44:13Z
first_seen: 2026-11-20T16:02:15.998933044Z
authors:
- jdkee
content: extracted
html: 2026-11-20-what-comes-after-agi.html
remote_preview:
  url: https://lh3.googleusercontent.com/MtcSyuy2GVyxylszKhwxAUP_OVh-e9ETmOtE5kLDGL8AIl59WmqxO5QJjRU3NDBnLsjBPmR3Wn5c4V0gVeOKge7MOF1oXMhqB5WWz8MCb89aH_e2=w1200-h630-n-nu-rw
---

For decades, AI research has been focused on a monumental milestone: building artificial general intelligence, or AGI - machines that match humans across nearly all cognitive tasks. It was always clear that, in principle, intelligence could progress beyond human level, but the prospect of AGI was far enough off, and surrounded by ample uncertainties of its own, that looking past it felt premature.

Now that AGI is coming within reach, we can no longer put off seriously considering what comes next. While far from certain, AI could continue to advance at its current pace - and possibly even accelerate - towards artificial general superintelligence, or ASI. It could have a profound impact on science, the economy, and society at a scale and speed much greater than AGI itself. Preparing for that potential shift requires mapping the possible roads from [AGI to ASI](https://arxiv.org/abs/2606.12683) and building rigorous, reliable ways to track progress.

## The continuum of general intelligence

To examine how the development of advanced AI might unfold, it helps to think of [general intelligence](https://doi.org/10.1007/s11023-007-9079-x), whether biological or artificial, as existing on a continuum. Human-level AGI is just one point along that spectrum.

ASI sits further along as an advanced system that consistently outperforms large groups of human experts on tasks that could take them years to complete, using fewer resources and less time. Examples would be making the same scientific progress as an entire research field in a fraction of the time, or achieving technological developments, such as in chip design, on par with what large companies can do in a year. ASI would need to meet this bar across virtually all tasks of human interest, not just some in narrow domains (which is already the case with some AI systems today that can beat any human in Chess and Go, predict the structure of proteins from their amino acid sequences, or prove theorems that resisted human mathematicians for decades).

![A diagram titled \"The Continuum of General Intelligence\" illustrating the progression of intelligence across four categories: Sub-AGI, Human Scale, Superintelligence (ASI), and Theoretical Ideal, ending at an impossible physical limit.](https://lh3.googleusercontent.com/LOBiGvYfmDOsmtjAFN7aBrcUx080UCHLdSIc4CywLxEXZwGh18ltYYe2mQRhb3Exd8oFf1AJ36gCQ4BLBlEJjyXfMHXsFIzepVP3g-j9rfxPjoHn=w2880-rw-lo)

Interestingly, we have a good [mathematical understanding](https://www.hutter1.net/ai/uaibook2.htm) of the very upper end of the intelligence continuum - the theoretical limits of general intelligence. We also know about hard physical, logical, and complexity-theoretic limits to what AI can achieve, such as predicting how real-world complex systems - from climate to immune systems to human societies - play out with arbitrary precision.

Yet knowing those upper limits doesn’t tell us what comes before them. Even in theory, it’s hard to predict how capable a ‘good enough’ approximation of general intelligence can be. For example, solving chess [perfectly](https://doi.org/10.1080/14786445008521796) requires [far more](https://doi.org/10.1016/S0004-3702\(01\)00152-7) computing power than exists on Earth, yet a thumbnail-sized chip can play the game well enough to defeat any human.

What we do know is that fundamental limits lie far beyond both today’s AI systems and humans. But the vast space between human-level AI and theoretical limits to intelligence, which encompasses [expert AGI](https://arxiv.org/abs/2311.02462) and a whole regime of more and more advanced superintelligent systems, is largely uncharted.

## Cognitive industrialization

Even if you are pessimistic that progress at the frontier of AI capabilities will continue, consider what happens if AI hardware and software continue to become more efficient.

Over the last ten years, AI progress was fueled by steady growth in *effective compute*, driven by three compounding factors:

1. **Hardware manufacturing improvements:** Under [Moore's law](https://ourworldindata.org/moores-law) and related trends, compute per dollar has increased by roughly 1.5x per year for six decades - meaning the same investment 18 months from now buys twice the compute.
2. **Algorithmic efficiency:** The amount of training compute (FLOPs) required to reach a [fixed AI performance level](https://arxiv.org/abs/2512.00193) has fallen at roughly 3–6x per year [over the last decade](https://arxiv.org/abs/2212.05153), while algorithmic efficiency at inference [improved by roughly 3x](https://arxiv.org/abs/2511.23455) per year. Doubling efficiency is practically equivalent to doubling the stock of physical compute.
3. **Compute hardware investment:** Spending on AI hardware has grown by roughly 2.5x per year over the past decade. For example, the training compute of the largest frontier training runs grows by [more than 4x per year](https://epoch.ai/blog/training-compute-of-frontier-ai-models-grows-by-4-5x-per-year).

Multiplying all three factors gives an overall growth rate of global effective compute stock of at least **10x per year** - a factor of 10 billion over ten years. Imagine that this trend holds. A decade from now, for *each* single AI agent you could run today, you would hypothetically be able to run more agents of the same quality than there are people in the world.

At the same time, continued hardware and software (inference) efficiency improvements alone would reduce the compute demand to run an AI workload by about **4x per year**. A cognitive workload that can be performed by today’s AI systems for **$1 billion in compute tokens** would cost just **$1000 in ten years.** Looking only at the last few years gives even steeper estimates: a [recent Epoch study](https://epoch.ai/publications/the-plunging-price-of-thought) finds the consumer price for achieving a fixed score on a set of benchmarks has fallen by more than 13x per year since 2023.

![A horizontal bar chart showing the annual scaling rates behind cognitive industrialization. It illustrates three factors contributing to the \"Global effective compute stock\": Compute per dollar (1.5x/year from hardware), AI hardware spend (2.4x/year from investment), and Algorithmic efficiency (~3x/year from algorithms). Combined, these multiply to an annual growth rate of ~11x, leading to a ~150,000x total scaling over five years.](https://lh3.googleusercontent.com/fplHb2TFWLnl2TUHa8KcifvtBpMmgxLyfb3PhpElyo8Jyl3z6nk_Hh1KGhnGfz3JtMSXRz1P_MSu5_Jex0r2kgmbnJZOmD9gOBtyej_W3yy70nMV=w2880-rw-lo)

Extrapolating both trends into the next decade raises the prospect of a dramatically larger global effective compute stock combined with equally dramatic cost reductions for cognitive work performed by AI. Increasingly, many cognitive tasks may therefore soon become *commoditised*. Much like mechanical industrialization turned routine physical labor into a commodity, this shift will lead to *cognitive industrialization* characterized by rapidly increasing standardization and automation of cognitive work across sectors, industrial-scale infrastructure build-out, and an abundance of (certain) cognitive outputs and services.

Is such a dramatic proliferation of intelligence realistic? It’s certainly [not a given](https://newsletter.forethought.org/p/humans-are-not-chinchillas-revisiting), but [practical ceilings](https://epoch.ai/publications/can-ai-scaling-continue-through-2030) on compute [build-out](https://www.forethought.org/research/how-far-can-ai-progress-before-hitting-effective-physical-limits), and more [speculative ceilings](https://www.forethought.org/research/how-quick-and-big-would-a-software-intelligence-explosion-be#distance-to-the-effective-limits-to-software) on algorithmic efficiency, together might allow the effective population of today’s AI agents to scale up by more than a thousand-fold by 2030, and more than a billion-fold within ten years from now.

Would cognitive industrialization alone be enough to achieve ASI? If we froze other dimensions of AI progress and only scaled up today’s AIs in speed and number, the result might not even count as full AGI: there may still be cognitive tasks that AI cannot do without human help. Nonetheless, sheer scale [can have a quality of its own](https://defensesindepth.bio/quantity-has-a-quality-of-its-own/). Suppose that we do achieve broadly human-level AI. Then there is clearly a major difference between human-level AIs that are slow and expensive to run, and a world where *billions* of agents act much faster than humans. That quantitative leap in speed and scale might itself be enough to bridge the qualitative gap from AGI to (weak) ASI.

Unlike humans, digital intelligence directly benefits from more and faster compute in several fundamental ways:

- **Processing speed:** AI systems can reason, ingest data, and generate outputs at increasingly high rates.
- **Working memory and memorization:** Today’s models can literally memorize large parts of the internet and their working memory capacity far exceeds human limits.
- **Substrate independence:** The same AI system can migrate from one computer to another, upgrade to faster or more energy-efficient hardware, or run across heterogeneous, spatially distributed fleets.
- **Lossless replication:** Digital intelligence can be copied nearly instantly and without loss - not only its source code (akin to DNA), but also its memory state (our lifetime experience). Specialist systems can be trained once, then spawned in millions of copies when needed.
- **High-bandwidth sharing of experiences:** Humans must strongly compress [experience and knowledge](https://henrich.fas.harvard.edu/publications/secret-our-success-how-culture-driving-human-evolution-domesticating-our) in order to teach others; AIs can replay stored high-bandwidth digital input-output streams across an entire population, and homogeneous AI groups could share raw learning signals directly.

## Four pathways from AGI to ASI

Dramatically more efficient AI and larger compute stocks can scale up the speed and number of AI agents at a given capability level. But some missing capabilities may be hard - or impossible - to patch over with sheer volume. While Einstein did figure out general relativity, a conference of one hundred thousand Ptolemaic astronomers probably wouldn’t have.

AI could follow four potential technological pathways to unlock previously unavailable cognitive capabilities. (For an in-depth discussion, see our [*From AGI to ASI*](https://arxiv.org/abs/2606.12683) report.)

### Scaling models and data

So far, qualitative capabilities have improved with remarkable consistency alongside model and data scale. This trend is captured by [scaling laws](https://arxiv.org/abs/2001.08361) that relate model performance to training compute, training data, and more recently, [test-time compute](https://www.tobyord.com/writing/mostly-inference-scaling) (giving a model extra computing power to reason through a problem before answering).

These scaling “laws,” however, are extrapolations of observed trends that hold only within familiar boundaries, such as a particular model architecture, type of training data, and family of evaluations. They can’t confidently predict the scale required for AGI or ASI, but we can identify potential bottlenecks to continued scaling of models and data:

- **Data exhaustion:** The need for larger amounts of high-quality training data is [outpacing humanity’s ability to produce more of it](https://arxiv.org/abs/2211.04325). ‘Synthetic’ data could help, but not all data produced by AI models [is useful for training](https://www.nature.com/articles/s41586-024-07566-y). To overcome this ‘data wall,’ researchers could rely on high-fidelity simulations, generate data within interactive (RL) environments, and use test-time compute to create better training data for future AI models.
- **Diminishing returns on model scale:** Each linear step up in model capability typically requires *multiplying,* not just adding, training compute and model size - but bigger models are slower, thus bottlenecking test-time scaling methods such as chain-of-thought reasoning.
- **Physical and economic resource limits:** Sustaining multi-order-of-magnitude increases in training compute runs up against real-world limits in building power plants, electrical grids, chip fabrication plants, and low-latency hardware interconnects. It remains unclear whether hardware and software efficiency gains - the main trends underlying cognitive industrialization - can compensate for this.

### Technological paradigm shifts

Today’s dominant AI paradigm is to train large transformers to predict the next symbol in a long sequence across vast datasets, then refine them to learn to reason and follow instructions, and give them extra test-time compute. [Recent research](https://www.mdpi.com/1099-4300/28/6/596) suggests this paradigm can, in principle, produce models that can run complex algorithms, including powerful [learning algorithms](https://arxiv.org/abs/1905.03030), via their prompts - that is, without requiring retraining. This is a key ingredient for building AI systems at the [theoretical upper limit of intelligence](https://arxiv.org/abs/2401.14953).

But today’s models fall short of humans on a few fundamental dimensions, including continual learning, certain forms of world modeling, decision-making, and sample efficiency - meaning that AI models need to be initially trained on much more data than humans to reach roughly the same intelligence level. They may also be missing the ability to invent [genuinely new concepts and abstractions](https://philarchive.org/rec/LERTAF) rather than recombining existing ones. Researchers are divided on whether these problems can be solved with simple extensions and evolutions of today’s paradigm, or whether reaching ASI will require truly deep technological paradigm shifts.

### Improving agent coordination

Human corporations, markets, and research communities solve problems that no individual could tackle alone. Similarly, coordinating thousands or millions of AGI instances into ["group agents"](https://doi.org/10.1007/s13347-021-00454-7) or [virtual agent economies](https://arxiv.org/abs/2509.10147) could produce [collective superintelligence](https://institute.deepmind.com/essays/artificial-symbiotic-intelligence/). What is unclear today is how AI group intelligence could [scale as we add more agents and compute](https://arxiv.org/abs/2606.02859), depending on how agents are organized. That is, we need **multi-agent scaling laws**. Early [research](https://arxiv.org/abs/2406.07155) finds clear intelligence gains in some tasks, but [coordination inefficiencies](https://arxiv.org/abs/2602.11865) still lead to [diminishing returns](https://www.tobyord.com/writing/swarm-scaling) as the population size grows.

Less obviously, steering large agent collectives will require robust institutional design and organizational mechanisms, including defenses against accidental or intentional [misinformation spread, manipulation, and collusion](https://arxiv.org/abs/2603.15809). Experiments with agent swarms are already demonstrating how quickly these dynamics can emerge, with individual agents [learning to cheat](https://institute.deepmind.com/essays/cheaters-and-whistleblowers-in-the-agent-swarm/) and others acting as whistleblowers to expose them.

### Recursive self-improvement (RSI)

Finally, AI systems could begin automating improvements to future generations of AI - a process known as recursive self-improvement (RSI) that was first discussed by [I. J. Good](https://doi.org/10.1016/S0065-2458\(08\)60418-0) and [Ray Solomonoff](https://journals.sagepub.com/doi/abs/10.3233/HSM-1985-5207).

Unlike typical technological development, improvements to AI could *directly* speed up subsequent gains, leading to an accelerating rate of progress. However, further improvements in research tend to be “[harder to find](https://www.aeaweb.org/articles?id=10.1257/aer.20180338)” - more and more effort is required to maintain the same rate of improvement across technologies. Whether RSI leads to ‘runaway’ progress is largely an empirical question.

RSI is not a single process. It can operate via improvements to **code and designs** - from model architectures and training algorithms to chip layouts. It can also operate through **data**, by spending compute at deployment to produce improved results and outputs that can be *reused* to train future models. [AlphaZero](https://www.nature.com/articles/nature24270) self-improved purely in this data driven way to reach superhuman performance.

Finally, RSI can play out at the **group level** by dividing labor and specializing roles in an agent collective. A group of coordinated specialists can achieve more than the sum of what they could achieve as isolated generalists, in turn becoming more effective at finding further improvements. Instead of just scaling up a team with fixed roles, larger and more efficient collectives enable new types of specialization that were previously unviable - much like how a multi-cellular organism can support specialized organs like a heart or a brain, as opposed to a single-celled organism that perform all survival functions on its own.

These three RSI mechanisms mirror evolutionary processes that drove the growth of human cognitive capabilities: genetic (code and design), cultural (data), and social evolution (group specialization through cooperation).

Importantly, RSI loops can easily stall or degenerate if the feedback guiding them is poor. Pursuing ASI via recursive self-improvement therefore requires evaluation methods that can be rapidly scaled into the superhuman regime, where even large groups of humans can't easily assess AI outputs. Accordingly, RSI should advance fastest in domains that allow for automated verification without humans in the loop, such as mathematical proofs, while other domains might lag behind or taper off sooner.

## **The many roads to ASI**

These pathways to ASI are not mutually exclusive - in fact, they compound on one another. If progress on one stalls, [economic and geopolitical competition](https://doi.org/10.1177/0162243915579283) will likely shift effort toward the others. Even if RSI doesn’t result in runaway gains, continued scaling in training data, speed, efficiency, and coordination could still drive rapid progress well beyond human-level AGI

Because there are multiple routes to superintelligence, trying to slow down AI development by restricting just one area - like the computing power for training an AI model - won't be enough to stop overall progress. Equally, suppressing a single dimension risks creating a capability “overhang”: if multiple inputs need to be combined to make progress, holding back one dimension while the others move forward would lead to sudden, steep jumps in progress when the suppressed dimension is relaxed (or overcome otherwise).

![A diagram illustrating the pathways to overall AI capabilities. It shows how \"Compute\" (Inference, Training, Experiments) feeds into \"Scale and Quantity\", \"Algorithmic Progress\", and \"Intelligence Improvements\" to drive \"Overall AI capabilities.\" A dashed feedback loop labeled \"Recursive improvement\" connects overall capabilities back to RL, data, and algorithmic progress.](https://lh3.googleusercontent.com/F1iAB9Lnl_XG42_gDB0glxaw17JGWAHBHqw1cMxB_Y0eRYObilu1mc3JEA7jIBCyY6AfyYUE5vXaoH6cb2fr6kyw2e5iJGaDTrkmEi9sIsy_8rX6vg=w2880-rw-lo)

Taking all these arguments together, continued fast AI progress over the next decade is a serious possibility. It could take us relatively smoothly from today, to Expert AGI, and then into the superhuman AI regime, accelerating science and enabling breakthroughs in fields like medicine, materials, and clean energy. Even in the absence of dramatic frontier AI progress, cognitive industrialization alone could reshape knowledge work and the broader economy. Regardless of which path AI progress takes, we must anticipate far-reaching opportunities along with equally transformative pressures to many aspects of human society.

None of these predictions are certain, but the pathways and scenarios are also far from unfounded hype or fiction. So what can be done? Our best course of action may sound familiar: model, measure, forecast, and repeat. While those steps aren’t surprising, the complexity and scale of carrying them out on the road to ASI is entirely unprecedented. It requires large-scale and highly-interdisciplinary efforts across both frontier labs and national and global institutions. Many of the key metrics for tracking progress on these pathways require frontier-model access and sizable amounts of evaluation compute, alongside new benchmarking and modelling methodologies that could spawn entirely new research fields.

Human-level AGI has long been the horizon for AI research, but it is far from the upper limit of intelligence. Understanding what could come next - and its implications across science, industry, and society - is the grand challenge ahead for researchers, developers, and policymakers.

**Acknowledgements**

This essay is based on the technical report [*From AGI to ASI*](https://arxiv.org/abs/2606.12683), co-authored with Matija Franklin, Alexander Lerchner, Laurent Orseau, Samuel Albanie, Adam Bales, Cole Wyeth, Stephanie Chan, Iason Gabriel, Joel Z. Leibo, Allan Dafoe, Marcus Hutter, Thore Graepel, and Shane Legg, as well as subsequent discussions within the DeepMind Institute team. Thanks also to Nenad Tomašev, Seb Krier, Davide Paglieri, Sasha Vezhnevets, Alison Snyder, and Fin Moorhouse for helpful discussions and feedback.
