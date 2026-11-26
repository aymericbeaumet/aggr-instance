---
title: Much shorter version of GRH Lean verification from Anthropic
link: https://github.com/lsernau/grh-lean
source: hnrss-org
published: 2026-11-25T21:48:10Z
updated: 2026-11-25T21:48:10Z
first_seen: 2026-11-26T02:47:10.395377406Z
authors:
- nb-t
content: extracted
html: 2026-11-25-much-shorter-version-of-grh-lean-verification-from-anthropic.html
remote_preview:
  url: https://opengraph.githubassets.com/47cc6de1b76b3d020950518d538d026836ac64d179afb8f6092ab5661fd1369e/lsernau/grh-lean
  alt: A short Lean 4 proof of the Generalized Riemann Hypothesis - lsernau/grh-lean
---

## GRH for Dirichlet L-functions, in Lean 4

Research artifact. Not maintained and not accepting contributions.

A Lean 4 proof of the Riemann Hypothesis and of the Generalized Riemann Hypothesis for Dirichlet L-functions, built on Mathlib and using only the axioms `propext`, `Classical.choice` and `Quot.sound`.

The proof is based on Levent Alpöge's, with Claude ([https://alpo.ge/remark.pdf](https://alpo.ge/remark.pdf)). This repository is a shorter version, produced in collaboration with my colleagues at Anthropic, and Claude.

## Two editions

- `grh/`: readable edition, about 300,000 lines. Start at `grh/GUIDE.md`.
- `grh-compact/`: the same five theorems, about 76,000 lines.

Each folder is its own Lake project with its own README.

## Build

Needs Lean v4.35.0 and Mathlib at commit 56a00585. In either folder:

```
lake exe cache get
lake build
lake env lean scripts/PrintAxioms.lean
```

A build takes one to two hours on 12 cores with 24 GB of memory (about 85 minutes more if the Mathlib cache is missing).

## Status

The proof has passed machine checks only, listed in each folder's README. No mathematician has read it through, and no paper comes with it.

## License

Apache 2.0, Copyright 2026 Anthropic, PBC. See `LICENSE`, and `NOTICE` and `ATTRIBUTION.md` in each folder.
