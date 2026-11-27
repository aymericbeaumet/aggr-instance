---
title: Measuring how AI agents handle everyday wet-lab science
link: https://openai.com/index/measuring-how-ai-agents-handle-wet-lab-science
source: openai-com
published: 2026-11-06T00:00:00Z
updated: 2026-11-06T00:00:00Z
first_seen: 2026-11-06T23:27:52.101188226Z
labels:
- publication
summary: OpenAI introduces BioBasicsBench, a 100-task benchmark measuring how well AI agents interpret raw experimental data and handle everyday wet-lab science.
content: extracted
html: 2026-11-06-measuring-how-ai-agents-handle-everyday-wet-lab-science.html
---

Our mission is to ensure that artificial general intelligence benefits all of humanity. As part of that mission, we want to help scientists accelerate biological research—from understanding disease to developing new treatments. For AI agents to contribute, they need to reliably interpret the experimental evidence scientists work with every day in wet labs, where they run experiments with biological samples. Everyday wet-lab work involves failed experiments, ambiguous images and conflicting observations. Existing benchmarks are built around charts and images selected for scientific papers may miss this ambiguity and the troubleshooting it requires. That’s why we’re introducing BioBasicsBench, a new benchmark developed with Ligo, a biotech company focused on enzyme design, to measure how well agents handle routine wet-lab analysis.

The benchmark’s 100 tasks test whether agents can interpret raw experimental data, identify errors, and recognize when the evidence cannot support a conclusion. Getting these tasks right matters because misreading an image or overlooking a problem with the controls used to check an experiment’s results can lead scientists to draw the wrong conclusion and spend time on experiments that won’t help answer their question.

At the ‘xhigh’ setting, which allows more reasoning per task, GPT‑6 Astra earned an average of 66.7% of the available credit, compared with 43.8% for GPT‑5.6 Sol. The results show progress on routine lab analysis, with room for further improvements in interpreting images, checking controls and applying experimental context.

BioBasicsBench uses raw experimental evidence, from gel images and colony plates to microscopy and instrument readouts.

BioBasicsBench has 100 tasks across five areas of routine wet-lab work: protein methods, imaging and cytometry, quantification and assays, nucleic acid methods, and molecular cloning. Tasks range from simple requests, such as reading the molecular weight of a DNA sample in a gel, to detailed assay analysis, but all involve routine work that wet-lab scientists should be able to complete reliably.

Scientists created the tasks using data from their own experiments, pairing each question with expert-written grading criteria. Each task underwent an internal review by a Ligo scientist who had not written it, an external review by a PhD-level scientist working in a wet lab, and a final internal review.

Agents return a structured answer with the requested conclusion, measurement or recommendation and, where required, supporting reasoning. Answers receive credit against individual rubric criteria, allowing the benchmark to distinguish a correct conclusion from a partially correct analysis. Most tasks test several skills together, including biological knowledge, interpretation of images and other data, error detection and protocol understanding.

The inputs preserve the forms of evidence scientists work with: gel and blot images, plate photographs, microscopy stacks, sequencing reads, plate-reader exports and videos of bench work. A task can include several kinds of data. Reference sheets, such as ladder charts and plasmid maps, support the interpretation of the experimental evidence.

| **Kind of data**                  | **Tasks** | **What it includes**                                                                                                                |
| --------------------------------- | --------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| Gels & blots                      | 31        | Agarose DNA gels, SDS-PAGE gels, Western blots                                                                                      |
| Protocols, notes & specs          | 27        | Protocols and SOPs, lab notebooks and run records, photographed handwritten notes, catalogues, instrument specs, published methods  |
| Tables & instrument exports       | 20        | CSV and Excel exports, lane layouts and inventories, raw flow cytometry (FCS) files                                                 |
| Bench, apparatus & reagent photos | 18        | Benches, gel rigs, reagent and culture tubes, a PCR plate, tube racks, pH strips, a concentrator, a freezer, a liquid-handler deck  |
| Microscopy                        | 14        | Raw brightfield and fluorescence images, multi-channel sets, z-stacks, time-lapse movies                                            |
| Instrument screens & plots        | 13        | Photos of flow cytometer, microscope, NanoDrop, plate reader and balance screens; exported standard curves, chromatograms and plots |
| Sequencing files                  | 12        | Raw long-read FASTQs, FASTA sequences, GenBank maps                                                                                 |
| Protocol videos                   | 7         | Streaking, inoculation, serial dilution, SDS-PAGE tank and incubator setups, weighing on a balance                                  |
| Agar plates & colonies            | 7         | Transformation plates, blue/white screens, antibiotic-disc plates                                                                   |
| Reference sheets                  | 21        | Ladder charts, plasmid maps, pH charts (supporting material)                                                                        |

*Number of tasks that include each kind of data. Categories overlap; reference sheets are supporting material.*

Scientists wrote the tasks using data from their own experiments, pairing the experimental evidence with a question and an expert-written grading rubric. Each task went through an internal review by a Ligo scientist who had not written it, an external review by a PhD-level scientist working in a wet lab, and a final internal review. Reviewers checked the prompt, experimental evidence, rubric, author notes and a graded model attempt.

Answers are scored against the task’s rubric, which assigns credit to individual criteria. Depending on the task, these criteria assess the conclusion, numerical results, supporting reasoning or recognition that the evidence is insufficient.

We evaluated seven models on BioBasicsBench: GPT‑5.2, GPT‑5.4, GPT‑5.5, GPT‑5.6 Sol, GPT‑6.1 Sol, GPT‑6 Luna and GPT‑6 Astra. The evaluation covers 37 model and reasoning-effort settings, with five attempts on each of the 100 tasks per setting, for a total of 18,500 results.

We report **mean reward**: the average task score, expressed as a percentage of the available credit. Error bars show 95% confidence intervals calculated by resampling whole tasks, keeping the five attempts for each task together.

GPT‑6 Astra substantially improves on GPT‑5.6 Sol, even at low reasoning effort. These gains suggest that **stronger native multimodal capabilities may be key to improving performance on everyday wet-lab** analysis.

### Performance at xhigh reasoning effort

| **Model**       | **Mean reward** | **95% CI** | **Output tokens / task** |
| --------------- | --------------- | ---------- | ------------------------ |
| **GPT‑6 Astra** | 66.7%           | 60.3–73.1% | 4,704                    |
| **GPT‑6.1 Sol** | 62.0%           | 55.6–68.4% | 5,238                    |
| **GPT‑5.6 Sol** | 43.8%           | 37.6–50.0% | 9,099                    |
| **GPT‑6 Luna**  | 38.3%           | 32.3–44.5% | 13,120                   |
| **GPT‑5.5**     | 35.4%           | 29.7–41.0% | 11,135                   |
| **GPT‑5.4**     | 33.2%           | 28.3–38.4% | 13,717                   |
| **GPT‑5.2**     | 31.2%           | 26.2–36.3% | 27,620                   |

*Xhigh is the highest reasoning-effort setting reported for all seven models. Each row includes all 100 tasks and five attempts per task. Max-effort results, where available, are included in the chart above.*

A common trap in science is over-interpreting the data, especially given the nature of bias that occurs when testing a hypothesis. Astra demonstrated a better ability to discern the limit of what the data shows and not draw unwarranted conclusions.

In this example, GPT‑5.6 Sol classified cells as dead based on the signal from a dye used to detect dead cells. But the detector could also pick up light from a second dye, so brightness alone could not justify that conclusion. Astra recognized this ambiguity and called for better controls to separate the signals.

*Brightfield · transmitted light*

*Channel 2 · SYTOX-labelled · 470–540 nm*

*Channel 3 · FUN-1 green · 525–580 nm*

Scientific research is highly multimodal, requiring constant visual interpretation and decision making. Astra displayed a significant improvement at handling multimodal data which was given in the form of photos and videos, as well as raw outputs from sequencing, plate-reader assays, microscopy and flow cytometry.

At the most basic level, this improvement was noticeable through GPT‑6 Astra’s ability to pick up on finer detail from images taken during experiments and therefore draw more accurate conclusions as shown in this example. This task asked agents to draw conclusions from a DNA cloning experiment, where controls are essential. GPT‑5.6 Sol saw no colonies on the negative control plate and based its conclusions on that observation. GPT‑6 Astra noticed that the plate was labeled with the wrong antibiotic and correctly rejected it as a reliable control.

![A gloved hand holds the plate. The handwritten rim label is circled.](https://cdn.openai.com/mdx-assets/16da4f0a-d37c-4056-a78c-4e1db6f08a14/plate-and-rim-mark.9c6764fa.png?w=3840&q=90&fm=webp)

*Original photograph*

*The label on the rim*

*The original benchmark photograph, with the rim label circled and enlarged. “LB Kan50” identifies kanamycin, a different antibiotic from the ampicillin used in the experiment.*

Scientists must be able to hold tacit biological knowledge and experimental context in mind whilst analysing data in order to correctly interpret what it shows. Astra demonstrated an improved ability to maintain this context and apply it throughout its analysis.

One task showed cells coming into and out of focus in a series of microscope images. While GPT‑5.6 Sol looked for a biological explanation for the cells’ apparent disappearance, GPT‑6 Astra correctly recognized a z-stack: a series of images taken at different depths.

![Frame at 9 seconds with the same cell location boxed.](https://cdn.openai.com/mdx-assets/16da4f0a-d37c-4056-a78c-4e1db6f08a14/depth-9s.332948ac.png?w=3840&q=90&fm=webp)

*9 s*

![Frame at 12 seconds with the same cell location boxed.](https://cdn.openai.com/mdx-assets/16da4f0a-d37c-4056-a78c-4e1db6f08a14/depth-12s.18f51074.png?w=3840&q=90&fm=webp)

*12 s*

![Frame at 15 seconds with the same cell location boxed.](https://cdn.openai.com/mdx-assets/16da4f0a-d37c-4056-a78c-4e1db6f08a14/depth-15s.15017a8a.png?w=3840&q=90&fm=webp)

*15 s*

*Six frames from the original recording. Dashed boxes mark the same location as the microscope scans through depth. Times refer to playback position in the supplied video.*

Higher benchmark scores do not yet mean models can make reliable judgments in the lab. These Astra examples show how assumptions, expected answers and supplied labels can override the evidence, and how a correct intermediate observation can fail to guide the final decision.

**Astra lets a familiar assumption override the interpretation supported by the experimental controls.** It can distinguish the two groups of colony bands, but assumes that a larger PCR product would identify successful cloning. On that condition it proposes lanes **4, 6, 7 and 8**; the reference’s control comparison instead selects lanes **5, 9 and 10**.

![Annotated colony PCR gel. Pink lane numbers 4, 6, 7 and 8 match the old-plasmid band. Teal lane numbers 5, 9 and 10 match the new-product band.](https://cdn.openai.com/mdx-assets/16da4f0a-d37c-4056-a78c-4e1db6f08a14/pcr-lanes.9b38de80.png?w=3840&q=90&fm=webp)

*The reference selects colonies **5, 9 and 10**; Astra proposes **4, 6, 7 and 8**.*

*1: marker · 2: cloning mixture · 3: original plasmid · 4–10: colony samples*

##### **Reading the controls**

1. Lane 3 contains only the original plasmid PCR. Its band provides the old-plasmid reference position.

2. In the reference interpretation, lane 2 contains both patterns: incomplete assembly leaves old template/backbone alongside the new product.

3. The resulting colonies can carry either old or new plasmid. Compare their bands with these reference patterns.

**Reference selection:** 5, 9 and 10

**Astra’s selection:** 4, 6, 7 and 8. Astra nominates this group suggesting the larger PCR product represents the intended construct.

The visual grouping is correct but the experimental identities attached to those groups are reversed relative to the reference. Astra therefore selects the wrong colonies from the colony PCR results which would result in the down-stream purification of the incorrect plasmid.

**Astra discounts observations that contradict its preferred interpretation.** It concludes that vacuoles, compartments inside cells, are generally darker than the surrounding cell, even though some appear lighter in the same image. Both appearances matter: changing the microscope’s focus can reverse the contrast, so the darker examples do not establish a general rule.

![Microscope image of cells showing the darker appearance in the contrast comparison.](https://images.ctfassets.net/kftzwdyauwt9/2aW0ho7APO5LnXzbTPP7fo/0a1404f9559b7a61b31578322efd1337/biobasicsbench-cell-contrast-darker.svg?w=3840&q=90)

*Darker appearance*

![Microscope image of cells showing the lighter appearance in the contrast comparison.](https://images.ctfassets.net/kftzwdyauwt9/3MC7gCtOqgIrJkNwWJFx5c/23494791f1915a2cbfc18b24f9e3ea05/biobasicsbench-cell-contrast-lighter.svg?w=3840&q=90)

*Lighter appearance*

*Astra’s darker-vacuole generalisation omits the lighter vacuoles visible in the same field.*

##### What that answer misses

Some cells have an opposite contrast. Both appearances occur because different focal depths can reverse the contrast.

Astra inspects several regions and even comments that contrast varies with focus. It then gives greater weight to the cells it calls better focused and retains only their darker-vacuole description in the final answer. The contradictory lighter regions have been inspected but are not considered in the conclusion.

**Astra’s counting marks do not reliably correspond to individual colonies: it fails to resolve close neighbours, misses visible colonies and marks empty agar.** The net result on this plate is an overcount of approximately 12% relative to the benchmark reference.

![Empty agar in the original crop.](https://cdn.openai.com/mdx-assets/16da4f0a-d37c-4056-a78c-4e1db6f08a14/counting-empty-agar-original.98da744f.png?w=3840&q=90&fm=webp)

*Original image*

![Empty agar with an incorrect counting mark.](https://cdn.openai.com/mdx-assets/16da4f0a-d37c-4056-a78c-4e1db6f08a14/counting-empty-agar-marked.6b30ead7.png?w=3840&q=90&fm=webp)

*Astra’s counting marks*

*A: one counting mark between neighbouring colonies. B: two visible colonies left unmarked. C: empty agar counted as a colony. Red circles indicate Astra’s counting positions. Black arrows are editorial guides.*

An inaccurate colony count directly affects estimates of how efficiently DNA was introduced into cells. With the plated volume and dilution held fixed, an overcount inflates that estimate by the same proportion.

Accelerating biological research requires getting the everyday work right. BioBasicsBench makes that work a target for improvement: interpreting raw experimental data, checking controls and recognizing when a conclusion is not supported. Progress on these tasks could help scientists spend less time troubleshooting and more time deciding which experiments to pursue next. Tracking these capabilities will also help us understand their implications for biosafety and biosecurity.
