---
title: H Company's new Holo2 model takes the lead in UI Localization
link: https://huggingface.co/blog/Hcompany/introducing-holo2-235b-a22b
source: huggingface-co
published: 2026-02-03T17:40:14Z
updated: 2026-02-03T17:40:14Z
first_seen: 2026-10-04T12:50:30.804479567Z
content: extracted
html: 2026-02-03-h-company-s-new-holo2-model-takes-the-lead-in-ui.html
---

Two months since releasing our first batch of Holo2 models, H Company is back with our largest UI localization model yet: **Holo2-235B-A22B Preview**. This model achieves a new State-of-the-Art (SOTA) record of 78.5% on [Screenspot-Pro](https://gui-agent.github.io/grounding-leaderboard/) and 79.0% on OSWorld G.

Available on [Hugging Face](https://huggingface.co/Hcompany/Holo2-235B-A22B), Holo2-235B-A22B Preview is a research release focused on UI element localization.

[![benchmark_table_light (3)](https://cdn-uploads.huggingface.co/production/uploads/698224a939bac08a1b6e0488/5dezFBy4-KimAIQR5_1j1.png)](https://cdn-uploads.huggingface.co/production/uploads/698224a939bac08a1b6e0488/5dezFBy4-KimAIQR5_1j1.png)

**Agentic Localization**

High-resolution 4K interfaces are challenging for localization models. Small UI elements can be difficult to pinpoint on a large display. With agentic localization, however, Holo2 can iteratively refine its predictions, improving accuracy with each step and unlocking 10-20% relative gains across all Holo2 model sizes.

**Holo2-235B-A22B's Performance on ScreenSpot-Pro**

Holo2-235B-A22B Preview reaches 70.6% accuracy on ScreenSpot-Pro in a single step. In agent mode, it achieves 78.5% within 3 steps, setting a new state-of-the-art on the most challenging GUI grounding benchmark.

[![cost_perf_screenspot_pro_light (2)](https://cdn-uploads.huggingface.co/production/uploads/698224a939bac08a1b6e0488/nht6cib9ua4mfPcM2C1I1.png)](https://cdn-uploads.huggingface.co/production/uploads/698224a939bac08a1b6e0488/nht6cib9ua4mfPcM2C1I1.png)

**Trained with SkyPilot**

Training Holo2 models at scale requires coordinating workloads across multiple cloud providers. H Company uses [SkyPilot](https://skypilot.readthedocs.io/) as a unified interface for launching training jobs on our clusters with Kubernetes (k8s). By abstracting away infrastructure complexity, SkyPilot lets researchers focus on model development instead of managing k8s manifests or maintaining separate deployment scripts.
