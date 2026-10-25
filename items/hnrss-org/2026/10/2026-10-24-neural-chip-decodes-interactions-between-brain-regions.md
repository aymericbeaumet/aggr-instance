---
title: Neural chip decodes interactions between brain regions
link: https://actu.epfl.ch/news/neural-chip-decodes-interactions-between-brain-r-2/
source: hnrss-org
published: 2026-10-24T15:57:17Z
updated: 2026-10-24T15:57:17Z
first_seen: 2026-10-25T10:04:58.814034362Z
authors:
- giuliomagnifico
content: extracted
html: 2026-10-24-neural-chip-decodes-interactions-between-brain-regions.html
remote_preview:
  url: https://actu.epfl.ch/image/168660/1440x810.jpg
---

EPFL researchers have developed an implantable neural chip that tracks communication across brain regions, identifying cognitive and emotional states in real time.

Deep brain stimulation (DBS) is a therapeutic technique that delivers electrical pulses to specific areas of the brain via an implant to modulate neural activity. It is already widely used for movement disorders such as Parkinson’s disease, and researchers are increasingly exploring its potential for severe psychiatric illnesses including major depression, post-traumatic stress disorder, and obsessive-compulsive disorder.

However, most existing DBS systems operate in what engineers call an “open loop” manner: stimulation is delivered according to predefined settings rather than continuously adapting to the patient’s brain activity. Even among closed-loop, or adaptive systems, many rely on signals recorded from a single brain region. This is a major limitation for psychiatric disorders, where clinically relevant information may be distributed across networks of interacting brain regions.

“Our work is based on the growing body of research suggesting that psychiatric disorders arise more from disruptions in communication between brain regions than from dysfunction of any single region,” explains Mahsa Shoaran, head of the [Integrated Neurotechnologies Lab](https://www.epfl.ch/labs/inl/) in EPFL’s School of Engineering and Neuro-X Institute.

![](https://actu.epfl.ch/public/upload/fckeditorimage/82/9a/74ffa000.jpg)\
The INL team at EPFL's Geneva campus. From left: Amitabh Yadav, Jieun Joo, Mahsa Shoaran, Jinhan Liu, and Daney Alex. 2026 EPFL / Lundi13 - CC-BY-SA 4.0

Shoaran and her team have developed a low-power, implantable neural system-on-chip. Dubbed LogSync, it records brain activity and extracts biomarkers that describe the strength of brain waves (oscillations) and synchrony between signals from different brain regions. A machine-learning algorithm then analyzes these network-level biomarkers to distinguish cognitive and emotional states, and generate stimulation triggers when specific states are detected.

The work, published in the [*IEEE Journal of Solid-State Circuits*](https://ieeexplore.ieee.org/document/11685197)*,* was developed as part of a [CHF 6.6 million grant](https://actu.epfl.ch/news/a-66-million-grant-to-develop-neurostimulator-fo-6/) from the US National Institute for Mental health (NIMH) to develop implantable systems capable of measuring and controlling oscillatory brain networks for severe psychiatric illnesses, particularly mood and anxiety disorders. It builds on the lab's previous work on [brain-to-text decoding](https://actu.epfl.ch/news/an-entire-brain-machine-interface-on-a-chip/) and [closed-loop neuromodulation](https://actu.epfl.ch/news/a-neuro-chip-to-manage-brain-disorders-7/), extending these concepts to network-level biomarkers and adaptive decoding for future psychiatric therapies.

**More targeted stimulation at lower power**

The researchers evaluated LogSync using human brain recordings from previous psychiatric experiments. One dataset captured changes in cognitive engagement while performing a mental task, while another captured emotional arousal as participants viewed video clips. The algorithm distinguished these cognitive and emotional states from the neural recordings alone. In further experiments conducted in rats at the University of Minnesota, the implant tracked ongoing oscillations and generated stimulation triggers at predefined phases of brain activity.

![](https://actu.epfl.ch/public/upload/fckeditorimage/10/08/2f640b49.jpg)\
The LogSync chip, measuring approximately 4x2 mm. 2026 EPFL / Lundi13 CC-BY-SA 4.0

Shoaran says that major challenge was achieving all this functionality while working within the very limited power budget of an implantable system.

“Our entire system is co-designed, meaning the algorithms and hardware are developed together with the constraints of implant in mind. This approach helped us achieve network-level neural signal processing and decoding on a single chip, while keeping power consumption under 200 microwatts — a fraction of that typically required by many implantable neural-interface systems,” she says. Another innovation is LogSync’s use of logarithmic computations, which simplify the complex operations needed for brain-network analysis and real-time biomarker extraction and decoding.

LogSync's initial validation focused on psychiatric applications. But the capability to extract brain-network biomarkers to trigger precisely timed stimulation is broadly applicable across next-generation neurotechnology, including adaptive DBS for movement disorders, closed-loop non-invasive stimulation, and emerging focused-ultrasound interfaces.

In parallel, the laboratory is developing new machine-learning methods that learn directly from neural recordings rather than predefined biomarkers. Their recent ICML 2026 work, which uses self-supervised learning to capture neural activity across regions, channels, and time, supports broader efforts to bring intelligent neural interfaces closer to clinical applications, including through [Infera Neuro](https://infera-neuro.com/), an EPFL spin-off co-founded by Shoaran.

**Funding**

United States National Institute of Mental Health

Swiss State Secretariat for Education, Research, and Innovation (SERI)-funded ERC Starting Grant

**References**

D. Alex et al., "LogSync: A Low-Power Logarithmic SoC for Cross-Regional Synchrony Extraction and Adaptive Psychiatric State Classification," IEEE Journal of Solid-State Circuits (JSSC), Sept 2026 doi: [10.1109/JSSC.2026.3728617](https://ieeexplore.ieee.org/document/11685197).

J. Liu and M. Shoaran, “RECTOR: Masked Region-Channel-Temporal Modeling for Affective and Cognitive Representation Learning”, International Conference on Machine Learning (ICML), 2026 [https://proceedings.mlr.press/v306/liu26dt.html](https://proceedings.mlr.press/v306/liu26dt.html)

D. Alex et al., "A 32-Channel 196-μW Logarithmic SoC for Brain Network Connectivity Extraction and Adaptive Psychiatric Symptom Classification," 2025 Symposium on VLSI Technology and Circuits (VLSI Technology and Circuits), Kyoto, Japan, 2025, pp. 1-3, doi: [10.23919/VLSITechnologyandCir65189.2025.11075022](https://ieeexplore.ieee.org/document/11075022/).
