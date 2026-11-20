---
title: Unforced Navier-Stokes Singularity and the Generalized Riemann Hypothesis
link: https://terrytao.wordpress.com/2026/11/20/unforced-euler-singularity-unforced-navier-stokes-singularity-and-the-generalized-riemann-hypothesis/
source: hnrss-org
published: 2026-11-20T17:21:36Z
updated: 2026-11-20T17:21:36Z
first_seen: 2026-11-20T22:37:07.598874728Z
authors:
- archon1410
content: extracted
html: 2026-11-20-unforced-navier-stokes-singularity-and-the-generalized.html
remote_preview:
  url: https://secure.gravatar.com/blavatar/bd4bda4207561b6998f10dec44b570f04ff4072b20f89162d525b186dfca3e49?s=200&ts=1795214036
---

Obviously these results will be reported widely in other outlets, but I will note them here, in decreasing order of how “digested” the results currently are:

- Levent Alpöge, Tristan Buckmaster, and Matei Coiculescu, with heavy AI assistance, have written a [human-readable proof of the finite time blowup for the unforced incompressible Euler equations in three dimensions](https://cims.nyu.edu/~tristanb/cc.pdf), which is [formalized in Lean here](https://github.com/mcoiculescu/EulerBlowupPublic).
- Levent Alpöge and Tristan Buckmaster, again with heavy AI usage, have generated a [proof of the finite time blowup for the unforced incompressible Navier Stokes equations in three dimensions, again with a Lean formalization](https://github.com/tristanbuckmaster/navier-stokes/). Here, the paper is technically human-readable and the general strategy of proof is broadly clear (following one of the expected paths to this result, namely to numerically build an approximate discretely self-similar solution and then use rigorous perturbation theory to convert this to an exact solution); but more work needs to be done to create a proper, professionally written proof.
- Levent Alpöge, using an internal AI model, has generated an (enormous) Lean-only proof of the Generalized Riemann Hypothesis. [This Lean Zulip thread](https://leanprover.zulipchat.com/#narrow/channel/113488-general/topic/Discussion.3A.20Alp.C3.B6ge.20Proves.20the.20Riemann.20Hypothesis) is probably the best place to follow the efforts to verify the proof and extract the strategy of proof (which, from very initial analysis, appears to use the full power of modern developments on the Langlands program and arithmetic geometry). (UPDATE: the moderators of the Lean thread are now focusing the discussion on the Lean-related aspects of the proof only.) I imagine the digestion process will take some time, though in this case at least we have a human author to communicate with to assist with this task.

See also [Buckmaster’s Mastodon post](https://mastodon.social/@tristanbuckmaster/117649035783718539) regarding the first two results.
