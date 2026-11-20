---
title: Finite-time blow-up for the Navier–Stokes equations
link: https://github.com/tristanbuckmaster/navier-stokes/
source: hnrss-org
published: 2026-11-20T15:34:31Z
updated: 2026-11-20T15:34:31Z
first_seen: 2026-11-20T22:37:07.598874728Z
authors:
- trotro
content: extracted
html: 2026-11-20-finite-time-blow-up-for-the-navier-stokes-equations.html
remote_preview:
  url: https://opengraph.githubassets.com/2f06163b926c525f510018d2066492179522b14b43d812992656d1bc070af513/tristanbuckmaster/navier-stokes
  alt: Contribute to tristanbuckmaster/navier-stokes development by creating an account on GitHub.
---

This repository holds the paper *Finite-time blow-up for the Navier–Stokes equations* by L. Alpöge and T. Buckmaster and the Lean 4 formalization that accompanies it.

## Contents

- `paper/finite-time-blow-up-navier-stokes.pdf`: the paper (79 pages, version of 20 November 2026).
- `lean/`: the Lean package (Lean v4.33.1 with Mathlib, at the revision pinned in `lean/lakefile.toml`).
  - `lean/README.md` explains how the formal statements relate to Theorem 1.1, Theorem 7.1, Corollary 7.2 and Appendix B of the paper, and where the formal proofs differ from the proofs of the paper.
  - `lean/NOTICE` says in words what each statement file contains and what is proved.

## The Lean package

The statement files at the top of `lean/` give the definitions and state the theorems with `sorry` in place of their proofs; `Solution.lean` proves them from the library `lean/Breakdown/`. Each `comparator*.json` pairs a statement file with `Solution.lean`, names the theorems whose statements must agree, and permits only the axioms `propext`, `Quot.sound` and `Classical.choice`.

To build, from `lean/`:

```shell
lake exe cache get
```

```shell
lake build
```

The library is large (about 2,270 files and 600,000 lines), and a full build needs a great deal of memory and time.

## What is and is not here

The formalization contains no data. The certificate whose hash the paper gives is not in this repository; the paper says that it will be released. That the certificate satisfies the conditions of Appendix B is verified by a rigorous computation, involving interval arithmetic (128 bits of precision, with the Arb library through FLINT), which is not part of the formalization and is not in this repository either.

The source of the paper is not in this repository.

## State of the Lean text

The package is the one of the paper's working repository, with one patch of 20 November 2026 applied to the cell-layer statements and to the library proofs that depend on them: the weight of one bound becomes $(|m|+\\tfrac12)^2$ in place of $m^2+\\tfrac14$, one constant of a cell may be zero, and the set of the harmonics of the model in a cell is restricted to those that do not vanish there. Appendix B of the paper states the conditions in this patched form. The patched package was not rebuilt when this repository was assembled.
