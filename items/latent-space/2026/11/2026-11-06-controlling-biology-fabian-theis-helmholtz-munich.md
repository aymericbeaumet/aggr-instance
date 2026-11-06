---
title: Controlling Biology - Fabian Theis, Helmholtz Munich
link: https://latent.space/p/controlling-biology-fabian-theis-helmholtz-munich/
source: latent-space
published: 2026-11-06T15:10:53Z
updated: 2026-11-06T15:10:53Z
first_seen: 2026-11-06T18:21:10.909146084Z
authors:
- RJ Honicky
summary: As systems biology efforts heat up to map biology at higher resolution, greater breadth and more modalities, Fabian Theis is pushing the frontier from mapping to predicting to controlling biology
content: extracted
html: 2026-11-06-controlling-biology-fabian-theis-helmholtz-munich.html
remote_preview:
  url: https://i.ytimg.com/vi/3nAV2s4GYbs/maxresdefault.jpg
  alt: Controlling Biology - Fabian Theis, Helmholtz Munich
extra:
  audio_type: audio/mpeg
  audio_url: https://podcasts.beehiiv.com/3967c1f2-cb97-4eb0-a5c0-21a1d2579408/01a15fdf-0e93-7e1a-880d-d73752a1f915/01a1a3f4-4773-7a7d-acdc-731f8540ab5d/01a1b153-728b-7ba1-9c45-88599573e55f.mp3
  duration_seconds: 3050
  thumbnail: https://i.ytimg.com/vi/3nAV2s4GYbs/maxresdefault.jpg
---

As systems biology efforts heat up to map biology at higher resolution, greater breadth and more modalities, Fabian Theis is pushing the frontier from mapping to predicting to controlling biology

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/dffd35a0-396d-4f5d-bcc2-7f9328e3dc21/image.png?t=1793836488)

Fabian Theis has been one of the key players in the various iterations of the Virtual Cell concept and initiatives. It is not hyperbole at all to say that his lab has been pushing the frontier of algorithms to understand multi-scale, multi-modal (”multiomics”), time-varying biological data.

Indeed, he has shaped the large scale vision and data collection through his leadership at Helmholtz, various startups and probably at Biohub itself.

Much of the recent attention in AI x Bio has been on protein and molecular design. AlphaFold, [ESM](https://latent.space/p/esmfold2/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) and [BioHub](https://latent.space/p/biohub/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv), [Chai](https://latent.space/p/chai-discovery/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv), [Leash](https://latent.space/p/lab-in-the-loop-at-compute-speed/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv), Isomporphic, [Boltz](https://latent.space/p/boltz/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv), [Genesis](https://latent.space/p/the-coolest-diffusion-research-isnt/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv), Octant, Baker Lab, and many many more. After a lot of initial excitement, Virtual Cell projects have received less attention, due in no small part to questions about the effectiveness of large-scale models in solving even basic Virtual Cell problems.

## A $1.8B bet

The Biohub announcement a few weeks ago got a lot of press for the (really) big numbers: $1.8B of commitments\[1\] from Biohub, NIH, DOE, Google and Meta. The discussion cites new imaging and measurement capabilities, generating large data sets and making the data available for training AI.

But why are these organizations doubling down on these big bets now? They have been in the works for years now, what is going on that makes now the moment to push?

New measurement capabilities and scale infrastructure, are now coming online that make Biohub’s push likely lead to big impact on science. DOE and NIH are developing lab-in-the-loop automation capabilities at scale. Google is scaling and broadening its connectomics capabilities ( episode coming soon!). Biohub itself has worked with various labs to develop and improve imaging platforms like laser phase plate microscopy, light sheet microscopy and advanced post-processing for regular microscopes that reveal deep phenotypic data. These technologies are quickly maturing, and will likely play a big part in the Biohub initiative.

## Virtual cells struggle

This kind of multi-scale, multi-modal data looks a lot like the data Fabian has made a career out modeling. On the other hand Virtual Cell modeling has run into problems that have dampened excitement about systems biology.

The 2025 Arc Virtual Cell Challenge shined a light on the problem: a slew of papers pointed out that constant baselines (not even linear!) often beat sophisticated models for “Perturbation Prediction”. If you change the inputs or programming of a cell (via gene editing, drugs or other environmental factors), how will the cell change it’s “state”. The disappointing finding is that many models fail to beat even the most basic basic baselines 🤦♂️.

The situation is not actually so bad:

[![Figure from the Nature Biotechnology paper on perturbation prediction evaluation metrics](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/789d09ac-2041-43b5-9921-850fa94c6952/image.png?t=1793752296)](https://www.nature.com/articles/s41587-026-03307-w?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv)

This excellent paper show that many existing evaluation metrics are poorly calibrated: they can reward an uninformative mean prediction because most genes barely change. With better-calibrated metrics, deep learning models can outperform the mean baseline.

## The problem is the problem

Nonetheless, Fabian Theis believes that perturbation prediction under the existing data sets just isn’t a hard enough problem to warrant complex algorithms. Instead we should be focused on exactly the problems that Biohub just announce an $1.8B data collection effort around: using large, high precision, multi-scale, co-registered biological data to predict the evolution of molecules → cells → niches → tissues → organs → organisms, and how all these scales effect one another.

This is a happy situation: at this very moment, AI is enabling us to tackle dramatically larger problems, and this problem is very, very big. We lack understanding of the “scale interfaces” between these different different “domains of applicability,” and we lack the data. The Biohub initiative aims to solve the data problem. Fabian Theis has been working on the algorithms to model these larger problems for years.

How do the other cells near me change my state? What happens to a distribution of cells from a perturbation? How do I perturb a cell to get it into the state I want?

These are increasingly complex questions that are building towards a lab-in-the-loop and large scale experimentation, and (actually) require increasingly complex models.

## Models drive precision medicine

Fabian’s labs and initiatives have been also investing heavily: the Computational Health Center at Helmholtz combines AI, molecular data, medical imaging, genomics, and clinical data to develop predictive and mechanistic models for precision medicine. The Institute of Computational Biology develops statistical and dynamical models integrating biological information from single-cell time series through population-scale omics data.

Given his drivers seat at the frontier, we were so excited for the opportunity to get an hour of his time to understand how Fabian Theis is thinking about Virtual Cells when the concept of what is difficult is changing so quickly.

We also discussed:

- The business of Virtual Cells

- The Bitter Lesson in biology - our perennial topic

- Training the next generation of Computation Biologists

- Closing the lab-in-the-loop

\[1\] NIH money is prior investments
