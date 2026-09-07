---
title: How Meta’s AI Models Are Powering the First Wave of Genesis Mission Projects
link: https://ai.meta.com/blog/genesis-mission-lawrence-berkeley-national-laboratory-segment-anything-dino/
source: ai-meta-com-blog
published: 2026-07-21T00:00:00Z
first_seen: 2026-09-07T17:03:44.343711062Z
content: extracted
html: 2026-07-21-how-meta-s-ai-models-are-powering-the-first-wave-of-genesis.html
preview:
  file: 2026-07-21-how-meta-s-ai-models-are-powering-the-first-wave-of-genesis.preview-ad36e6a55a5e.webp
  width: 256
  height: 144
  color: '#2c4c59'
images:
- source: https://scontent-sjc3-1.xx.fbcdn.net/v/t39.2365-6/752631850_918439901272497_2117962010845605116_n.png?_nc_cat=103&ccb=1-7&_nc_sid=e280be&_nc_ohc=VIonh25JfqgQ7kNvwFIgdis&_nc_oc=AdrCZSVcb8M_HR69oqZuzzBYN-ERAyv1jbv6cOp4mrNdW3LjMqe10MDswFn5IP4lcZ8&_nc_zt=14&_nc_ht=scontent-sjc3-1.xx&_nc_gid=cFlEqjoD0LlLCuWpKUWbHA&_nc_ss=7b20f&oh=00_AQJFBi7x8VPZdi03Bjti60XlOOJSVsyiNlbw11bDJzFRFg&oe=6AB9383E
  original:
    file: 2026-07-21-how-meta-s-ai-models-are-powering-the-first-wave-of-genesis.image-2bf4ccbeb7b1.png
    width: 1920
    height: 1080
  variants:
  - file: 2026-07-21-how-meta-s-ai-models-are-powering-the-first-wave-of-genesis.image-a3318d32730b.webp
    width: 48
    height: 27
  - file: 2026-07-21-how-meta-s-ai-models-are-powering-the-first-wave-of-genesis.image-bf7b660ea857.webp
    width: 320
    height: 180
  - file: 2026-07-21-how-meta-s-ai-models-are-powering-the-first-wave-of-genesis.image-3a4b76a98342.webp
    width: 640
    height: 360
  - file: 2026-07-21-how-meta-s-ai-models-are-powering-the-first-wave-of-genesis.image-537814eef1cf.webp
    width: 960
    height: 540
  - file: 2026-07-21-how-meta-s-ai-models-are-powering-the-first-wave-of-genesis.image-6f2c31081ef9.webp
    width: 1280
    height: 720
  - file: 2026-07-21-how-meta-s-ai-models-are-powering-the-first-wave-of-genesis.image-c7c3c466e150.webp
    width: 1600
    height: 900
  - file: 2026-07-21-how-meta-s-ai-models-are-powering-the-first-wave-of-genesis.image-d6ce39106b7a.webp
    width: 1920
    height: 1080
  color: '#030d15'
- source: https://scontent-sjc3-1.xx.fbcdn.net/v/t39.2365-6/752648857_903078942844172_5326165404058647623_n.png?_nc_cat=103&ccb=1-7&_nc_sid=e280be&_nc_ohc=Ig318ZPJHzcQ7kNvwFDR3m-&_nc_oc=AdocI8dfPHYkIpDVS8azRXBQBpWY40eTVpmaymCVKk6G3x0SeKO9N3uMOp60puEzvew&_nc_zt=14&_nc_ht=scontent-sjc3-1.xx&_nc_gid=cFlEqjoD0LlLCuWpKUWbHA&_nc_ss=7b20f&oh=00_AQLHBOH5OSe1jgSFcDz_0UmKTnBXo3IMRM3VsuhEuObtlA&oe=6AB91979
  original:
    file: 2026-07-21-how-meta-s-ai-models-are-powering-the-first-wave-of-genesis.image-eb1dc5644d42.jpg
    width: 406
    height: 379
  color: '#2a3748'
---

[Lawrence Berkeley National Laboratory](https://www.lbl.gov/) — one of the [US Department of Energy's](https://www.energy.gov/) premier research laboratories, known for Nobel Prize-winning work in physics, chemistry, and materials science — operates some of the most advanced scientific facilities on the planet. Among them is the [Advanced Light Source](https://als.lbl.gov/) (ALS), a football field-sized facility that produces intensely bright beams of X-ray light, allowing researchers to study materials from the atomic and molecular scale all the way to plants. The ALS's instruments, known as beamlines, generate enormous quantities of data — and as recent facility upgrades have dramatically increased their resolution and speed, the volume of data has exploded beyond what scientists can keep up with.

The numbers are staggering: The DOE's light and neutron source facilities now produce tens of petabytes of data annually — that's millions of gigabytes, roughly equivalent to streaming 2 million hours of HD video. This backlog didn't always exist. Upgraded detectors, which have gone from capturing a single image every six seconds to 100,000 images per second, mean these facilities now generate orders of magnitude more data than they did a decade ago, and traditional manual analysis simply can't keep pace.

The problem goes beyond volume: domain experts are scarce and overwhelmed, and modern in-situ experiments — where scientists observe dynamic processes like chemical reactions or material failures as they occur — demand real-time interpretation that no human team can deliver manually.

Much of the analysis challenge comes down to one task: segmentation — the process of identifying and drawing precise boundaries around distinct structures within an image. In computer vision, segmentation is what enables everything from medical scans that distinguish tumors from healthy tissue to autonomous vehicles that separate pedestrians from pavement. In scientific research, segmentation is what transforms a raw X-ray image from a wall of grayscale pixels into a labeled map of meaningful structures — cell walls, mineral grains, semiconductor layers — that researchers can quantify and compare across experiments.

## SYNAPS-I and the Genesis Mission

In late 2025, the White House launched [The Genesis Mission](https://www.energy.gov/undersecretaryforscience/genesis-mission/genesis-mission), a sweeping national initiative to accelerate scientific discovery and technological leadership using advanced artificial intelligence, led by DOE. SYNAPS-I (SYnergistic Neutron And Photon Science – Intelligence) is one of its flagship projects: a multi-lab initiative led by Berkeley Lab, in partnership with [Argonne](https://www.anl.gov/), [Brookhaven](https://www.bnl.gov/world/), [Oak Ridge](https://www.ornl.gov/), and [SLAC](https://www6.slac.stanford.edu/) National Laboratories, aimed at transforming data analysis across X-ray and neutron science from a months-long bottleneck into a real-time discovery engine, with scientific imaging as a major target. Nowhere is that bottleneck more acute than in image segmentation, where extracting meaningful structures from experimental data can consume weeks of expert effort per dataset.

At the heart of SYNAPS-I's segmentation pipeline are two open-source foundation models released by Meta: [Segment Anything Model 3](https://ai.meta.com/research/sam3/) (SAM 3) and [DINOv3](https://ai.meta.com/research/dinov3/).

## How SAM and DINO Transform Scientific Imaging

DINOv3 is a self-supervised vision model, meaning it learns visual patterns from raw images without requiring humans to label them first. It excels at understanding what different structures in an image represent and where they are located. SAM takes that understanding a step further, drawing precise boundaries around individual objects in an image — much like a scientist carefully outlining structures by hand, but in seconds rather than hours.

Together, the two models form a complementary pipeline: SAM delivers precise, pixel-level boundaries, while DINO provides global context to identify each structure and its place within the sample. The SYNAPS-I team fine-tuned both models on scientific imaging data collected at DOE beamlines, then deployed them across 300 A100 GPUs — the high-performance computing chips that power today's most advanced AI systems — at national supercomputing facilities such as [NERSC](https://www.nersc.gov/). The result: a fully reconstructed, semantically labeled 3D volume delivered back to the scientist physically standing at the beamline instrument, ready for interpretation while the experiment is still running. Total turnaround: approximately 15 minutes.

The SYNAPS-I team demonstrated this pipeline on a pressing agricultural challenge — understanding how grapevines respond to drought at the cellular level. Using micro-CT scans collected at the Advanced Light Source, the pipeline reconstructs 3D volumes of vine stems and automatically identifies xylem vessels — the microscopic tubes responsible for water transport within the plant. By tracking how these vessels change as drought progresses, researchers gain insights that could inform the development of drought-resilient crops, and provide solutions for agricultural resilience into the future.

![](https://scontent-sjc3-1.xx.fbcdn.net/v/t39.2365-6/752648857_903078942844172_5326165404058647623_n.png?_nc_cat=103&ccb=1-7&_nc_sid=e280be&_nc_ohc=Ig318ZPJHzcQ7kNvwFDR3m-&_nc_oc=AdocI8dfPHYkIpDVS8azRXBQBpWY40eTVpmaymCVKk6G3x0SeKO9N3uMOp60puEzvew&_nc_zt=14&_nc_ht=scontent-sjc3-1.xx&_nc_gid=cFlEqjoD0LlLCuWpKUWbHA&_nc_ss=7b20f&oh=00_AQLHBOH5OSe1jgSFcDz_0UmKTnBXo3IMRM3VsuhEuObtlA&oe=6AB91979)

Micro-CT scan of a grapevine stem, segmented by SYNAPS-I. Cyan: hydrated xylem vessels; dark purple: dry.

What previously required a month of expert annotation per time step now takes 15 minutes, enabling scientists to study dynamic biological processes at the speed of data acquisition itself.

## Why Open Source Matters

National laboratories keep prepublication research data and AI models on government infrastructure, not external cloud services. This work must be managed on secure platforms while in progress. Meta's open source approach makes this possible. The SYNAPS-I team can download, fine-tune, and deploy SAM and DINO within their own secure computing environments, adapting models originally trained on natural images to scientific domains they were never designed for.

With 60 researchers across five national labs, SYNAPS-I is building toward a future where user facilities operate as intelligent discovery platforms — where AI doesn't just process data faster, but helps scientists generate hypotheses, recommends next experiments, and transfers knowledge across facilities so that a breakthrough at one beamline benefits researchers at all of them. As Genesis scales from seed projects to full programs, that open source foundation is poised to accelerate discovery across an expanding set of national priorities.

At the recent Trillion Parameter Consortium, DOE Under Secretary Dario Gil referred to the promise of this effort.

"By seamlessly combining AI, advanced computing, and experimental systems, SYNAPS-I analyzes data as it's produced and guides experiments in real time, replacing slow manual steps with adaptive, automated decision-making," he said. "This compresses discovery time from days to moments and establishes a continuous, self-improving model of science that will be essential to realizing the full potential of the Genesis Mission."
