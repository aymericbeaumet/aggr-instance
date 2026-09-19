---
title: Mapping global methane emissions from space with deep learning
link: https://blog.google/innovation-and-ai/models-and-research/google-research/mapping-global-methane-emissions-from-space/
source: blog-google-innovation-and-ai-technology-ai
first_seen: 2026-09-19T21:30:23.395188495Z
summary: The work we're doing to make AI helpful for everyone.
content: extracted
html: 2026-09-19-mapping-global-methane-emissions-from-space-with-deep.html
preview:
  file: 2026-09-19-mapping-global-methane-emissions-from-space-with-deep.preview-b7ebc4d8b453.webp
  width: 256
  height: 144
  alt: B-roll showing diverse environments and people, including a teacher and students in a classroom and a patient with a doctor
  color: '#4e4e4d'
images:
- source: https://storage.googleapis.com/gweb-uniblog-publish-prod/images/SocialShare_hD2gcAw.width-1000.format-webp.webp
  original:
    file: 2026-09-19-mapping-global-methane-emissions-from-space-with-deep.image-7d57f15b6f47.webp
    width: 1000
    height: 562
  color: '#0b0608'
- source: https://storage.googleapis.com/gweb-uniblog-publish-prod/images/Map_Global_Methane_social.width-2000.format-webp.webp
  original:
    file: 2026-09-19-mapping-global-methane-emissions-from-space-with-deep.image-62b03c17280a.webp
    width: 2000
    height: 1125
  variants:
  - file: 2026-09-19-mapping-global-methane-emissions-from-space-with-deep.image-447167d83b3b.webp
    width: 320
    height: 180
  - file: 2026-09-19-mapping-global-methane-emissions-from-space-with-deep.image-dc0809e72ea5.webp
    width: 640
    height: 360
  color: '#4a5643'
---

In a new study published in [PNAS](https://www.pnas.org/doi/10.1073/pnas.2612145123), Google and NASA’s Jet Propulsion Laboratory (JPL) introduced MAPL-EMIT, an AI model that tracks methane emissions globally from space using NASA’s [EMIT](https://www.jpl.nasa.gov/missions/emit-earth-surface-mineral-dust-source-investigation/) instrument.

Methane is a potent greenhouse gas. Over a 100-year timeframe, its warming potential is 30 times greater than that of carbon dioxide. MAPL-EMIT tackles a critical bottleneck in methane detection. Trained on 3.6 million physics-simulated methane plumes (clouds of methane gas released into the atmosphere), it cuts through complex, noisy terrain to detect 50% more plumes than human experts and identifies more than 23,000 additional plumes globally, including 24 out of 25 of the world’s largest-emitting landfills. By making methane sources easier to find at scale, MAPL-EMIT enables faster, more targeted climate mitigation.

Google has released the global plume database on [Earth Engine](https://developers.google.com/earth-engine/datasets/catalog/projects_nature-trace_assets_ghg_emit_mapl_emit_plumes_v1_0) alongside an [Earth Engine app](https://nature-trace.projects.earthengine.app/view/mapl-emit) to visualize the data. Open-source models are available on [Kaggle](https://www.kaggle.com/models/vishalbatchu/emit-methane-plume-detection-and-quantification/) and inference tools are on [GitHub](https://github.com/google-research/mapl) to support researchers, policymakers, and operators. Read more on the [Google Research blog](https://research.google/blog/mapping-global-methane-emissions-from-space-with-deep-learning/).

![A six-panel satellite graphic showing detected methane plumes and emission heatmaps in California (USA), Turkmenistan, Delhi (India), Sao Paulo (Brazil), Katowice (Poland) and Shanxi (China).](https://storage.googleapis.com/gweb-uniblog-publish-prod/images/Map_Global_Methane_social.width-2000.format-webp.webp)
