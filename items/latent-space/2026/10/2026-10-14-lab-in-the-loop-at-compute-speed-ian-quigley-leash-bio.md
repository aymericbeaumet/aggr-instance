---
title: Lab-in-the-Loop at Compute Speed — Ian Quigley, Leash Bio
link: https://www.latent.space/p/lab-in-the-loop-at-compute-speed
source: latent-space
published: 2026-10-14T20:35:09Z
updated: 2026-10-14T20:35:09Z
first_seen: 2026-10-14T21:35:38.678109349Z
authors:
- Brandon Anderson
summary: How Leash Bio tamed DELs, the assay that traumatizes everyone, and used the data to catch cheating models. And why that same data is perfect for AI agents learning to think like chemists.
content: feed
html: 2026-10-14-lab-in-the-loop-at-compute-speed-ian-quigley-leash-bio.html
extra:
  audio_type: audio/mpeg
  audio_url: https://api.substack.com/feed/podcast/220316507/088d386ca7b830a53c64e1cd2678e427.mp3
---

A few years ago I stumbled upon [Leash Bio’s](https://www.leash.bio/) [Substack](https://leashbio.substack.com/), and was hooked. Be it their [Kaggle competition](https://leashbio.substack.com/p/introducing-belka) that showed that [no, we really haven’t solved protein-ligand binding](https://leashbio.substack.com/p/belka-results-suggest-computers-can), or their crazy discovery that knowing [a molecule’s author is perhaps all you need to know to predict how good it is](https://leashbio.substack.com/p/ai-for-chemistry-in-2025-is-like). Their substack — or at least [Owl Posting](https://www.owlposting.com/)’s [1](https://www.latent.space/p/lab-in-the-loop-at-compute-speed#footnote-1) [deep dive](https://www.owlposting.com/p/an-ml-drug-discovery-startup-trying) on them — should be required reading for any scientist trying to do chemistry with AI.

So I was super excited to finally be able to sit down with [Ian Quigley](https://www.linkedin.com/in/iquigley/) from Leash Bio, and pick his brain. Leash specializes in [DNA Encoded Libraries](https://en.wikipedia.org/wiki/DNA-encoded_chemical_library) (aka “DELs”), a way of collecting ridiculously high-throughput interaction data between proteins and small molecules.[2](https://www.latent.space/p/lab-in-the-loop-at-compute-speed#footnote-2) Before I discovered Leash, I had heard nothing but horror stories from DEL users. And yet, Ian and friends somehow found a way to tame the beast, extracting clean and reproducible data that is just perfect for large scale modeling. Without teasing too much, the answer comes down to epistemic rigor, and an incredible conviction that you just have to Do The Thing.

It gets even better! Near the end of this episode, Ian teases something his team was just playing with when we recorded: an AI playground for agents to do verifiable scientific reasoning. They discovered the data they had spent the last few years collecting could provide tens of thousands of “lab-in-the-loop” simulations, just what an agent needs for scientific RL. Two months later, it’s matured into Minos, and today they’re releasing a full [preprint](https://github.com/Leash-Labs/minos-paper/blob/trunk/Minos_Leash.pdf), [blog](https://leashbio.substack.com/p/making-reality-replayable), and [open dataset](https://strelka.leash.bio/). We’ll be releasing a lightning podcast in a few days with an update. Stay tuned!

We had a blast with Ian, probably one of our most fun episodes to date. For both complete chemistry novices and expert AI x Bio practitioners, we think you’ll get something out of this.

Enjoy!

[www.youtube.com](https://www.youtube.com/watch?v=hQSBJVreLms)

We covered:

- Why DELs traumatize everyone who touches them, and how Leash puts dozens of barcodes on each molecule to catch proteins that bind the DNA instead of the drug.

- Ian’s rule that biology only scales two ways: taking pictures and sequencing DNA.

- The [Kaggle competition](https://www.kaggle.com/competitions/leash-BELKA): about 4B measurements from a basement and about 2,000 teams, with nobody beating chance on an unseen chemical library. Also why Leash quietly feared co-folding would make them obsolete.

- [Clever Hans](https://en.wikipedia.org/wiki/Clever_Hans) in [ChEMBL](https://www.ebi.ac.uk/chembl/): models can guess a molecule’s chemist from its structure, and “who made it” predicts activity about as well as the structure does.

- [Hermes](https://leashbio.substack.com/p/we-now-know-what-it-will-take), trained only on Leash’s own data, keeping pace with [Boltz](https://github.com/jwohlwend/boltz) on public benchmarks. Plus the BRD4 molecule Boltz had already seen in the PDB.

- [Breaking the pocket](https://leashbio.substack.com/p/taking-away-the-protein-cheat-sheet): engineering protein mutants at scale after catching Hermes memorizing “nicknames” for proteins instead of binding sites.

- [Lumacaftor](https://en.wikipedia.org/wiki/Lumacaftor)-like hits as an accidental detector for misfolded proteins.

- The [agent sandbox](https://github.com/Leash-Labs/minos-paper/blob/trunk/Minos_Leash.pdf): LLMs that reason about SAR in plain English, hunting for the best binders among 6.5M molecules on a budget of 100.

- The [Recursion](https://www.recursion.com/) mafia, and why its alumni companies all rhyme.

- The [Kosuri paradox](https://x.com/srikosuri/status/2035866164325437894): if you can collect the data, why do you need the model?

- Going from a DEL hit to good mouse PKPD in six months on an inflammation program, and the case for pills over antibodies on cytokine targets.

- What stops a better-funded lab from outscaling them, and Ian’s by-fiat wish for massively parallel purification.

[1](https://www.latent.space/p/lab-in-the-loop-at-compute-speed#footnote-anchor-1)

The hands-down best bio blogger on the internet.

[2](https://www.latent.space/p/lab-in-the-loop-at-compute-speed#footnote-anchor-2)

As always, the secret here is to map your problem to a combinatorial library and a readout using Next Generation Sequencing. We talk about this in depth in the podcast!
