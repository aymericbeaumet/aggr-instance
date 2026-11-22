---
title: Verification files for the Lean proof of the Riemann Hypothesis
link: https://math.mit.edu/~drew/grh/
source: hnrss-org
published: 2026-11-21T15:54:34Z
updated: 2026-11-21T15:54:34Z
first_seen: 2026-11-22T06:30:16.969889396Z
authors:
- Bluestein
content: extracted
html: 2026-11-21-verification-files-for-the-lean-proof-of-the-riemann.html
---

Announced at [alpo.ge/remark.pdf](https://alpo.ge/remark.pdf) on 20 November 2026.

Andrew Sutherland (MIT), checks run with Claude Code. Page last updated 2026-11-22 04:35 UTC. Results below are as of that time and this page will be updated as the remaining checks finish.

## Summary

The published Lean source compiles without errors, the five headline theorems depend only on the three standard axioms, every module has been replayed through the Lean kernel, two independently written kernels, nanoda and con-leche, accept the full exported proof, and lean4lean accepts every compiled module. The `--compare` modes of con-leche and of its Rust port con-ron, independent implementations of Comparator's statement matching, each confirm all five theorems against the challenge. An independent sandboxed `lake build` of the archive reproduces the export byte for byte. The `lake comparator` run and the Lean-kernel replay of the export are still in progress.

| Check                                                                                                                        | Result                       | Detail                                                                                                                                                                                                                                                                                                                                                                                               |
| ---------------------------------------------------------------------------------------------------------------------------- | ---------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| All 175,187 modules compile (Lean v4.33.1, Mathlib v4.33.1)                                                                  | pass                         | zero errors; finished 16:43:21 UTC                                                                                                                                                                                                                                                                                                                                                                   |
| `#print axioms` for the five theorems in `Main.lean`                                                                         | pass                         | `propext`, `Classical.choice`, `Quot.sound` only ([audit.log](https://math.mit.edu/~drew/grh/audit.log))                                                                                                                                                                                                                                                                                            |
| No axioms declared outside Lean core                                                                                         | pass                         | 15 axioms in the environment, all in `Init`                                                                                                                                                                                                                                                                                                                                                          |
| Mathlib files identical to the official cache                                                                                | pass                         | 8,322 Mathlib and 378 dependency `.olean` files, byte for byte                                                                                                                                                                                                                                                                                                                                       |
| Independent rebuild with `lake build Main` in a sandbox (no network, read-only source), on a separate machine                | pass                         | exit 0, zero errors; finished 18:07:21 UTC on 21 November ([timeline](https://math.mit.edu/~drew/grh/sandbox-timeline.txt), [script](https://math.mit.edu/~drew/grh/sandbox-build.sh))                                                                                                                                                                                                             |
| Export made inside that sandbox, compared with the published export                                                          | identical                    | 61,570,595,788 bytes, sha256 5ea74b91…244e; finished 19:32:56 UTC                                                                                                                                                                                                                                                                                                                                    |
| Compiled files of the sandboxed `lake build`, compared with our manifest                                                     | 175,186 of 175,187 identical | the one differing module compiles non-deterministically; see [the note below](https://math.mit.edu/~drew/grh/#manifest-path) ([its manifest](https://math.mit.edu/~drew/grh/sandbox-project-oleans.sha256))                                                                                                                                                                                        |
| `leanchecker` kernel replay of every module                                                                                  | pass                         | 175,187 of 175,187; zero rejections; finished about 18:17 UTC                                                                                                                                                                                                                                                                                                                                        |
| Challenge export equals the one published by [Deicyde/riemann-hypothesis](https://github.com/Deicyde/riemann-hypothesis) CI | match                        | sha256 bd484ac9…b2b9                                                                                                                                                                                                                                                                                                                                                                                 |
| nanoda (independent Rust kernel) on the full export                                                                          | accepts                      | exit 0 at 19:05:45 UTC; 27 min 42 s on 80 threads ([nanoda.log](https://math.mit.edu/~drew/grh/nanoda.log))                                                                                                                                                                                                                                                                                         |
| Lean default kernel and Lean paranoid kernel on the export                                                                   | running                      | single-threaded; no verdict yet                                                                                                                                                                                                                                                                                                                                                                      |
| `lake comparator` (Lean v4.36.0) end to end                                                                                  | running                      | statement comparison and axiom check already passed in one run; kernels in progress                                                                                                                                                                                                                                                                                                                  |
| con-leche `master` (`cd2eceab`) on the full export                                                                           | accepts                      | 1,468,418 declarations accepted in `--verified` mode; exit 0 at 21:19:04 UTC; 3 min 15 s on 64 threads ([log](https://math.mit.edu/~drew/grh/con-leche-master.log))                                                                                                                                                                                                                                 |
| con-leche `--compare` (branch `compare`, `5dc36dc1`): challenge against solution                                             | accepts                      | all five theorems "justified by the solution"; 51,692 challenge declarations accepted; exit 0 at 21:19:12 UTC ([log](https://math.mit.edu/~drew/grh/con-leche-compare.log))                                                                                                                                                                                                                         |
| con-ron `--compare` (`master`, `3f22699b`): challenge against solution                                                       | accepts                      | all five theorems "justified by the solution"; 1,468,418 solution and 51,692 challenge declarations accepted in `--verified` mode; exit 0 at 23:02:33 UTC on 21 November; 7 min 6 s on 64 threads ([log](https://math.mit.edu/~drew/grh/con-ron-compare.log))                                                                                                                                       |
| lean4lean (`67f63b0c`, built with Lean v4.33.1) on the compiled files, module by module                                      | pass                         | 175,187 of 175,187; finished about 21:55 UTC. 175,182 passed with default settings. Five hit lean4lean's default step limit ("deterministic timeout") on `decide +kernel` computations and passed when rerun with the limit raised ([timeouts](https://math.mit.edu/~drew/grh/lean4lean-default-fuel-timeouts.log), [reruns with flags](https://math.mit.edu/~drew/grh/lean4lean-raised-fuel.log)) |
| con-leche and con-ron as bundled with Lean v4.36.0                                                                           | declined                     | exit 2, not a rejection: those older binaries do not support one reflexive inductive type in the proof. con-leche and con-ron built from `master`, above, do. ([log](https://math.mit.edu/~drew/grh/con-leche-bundled-4.36.log), [log](https://math.mit.edu/~drew/grh/con-ron-bundled-4.36.log))                                                                                                   |
| lean4lean bundled with Lean v4.36.0                                                                                          | not applicable               | cannot read a v4.33 core library (`unknown constant 'Decidable.intro'`)                                                                                                                                                                                                                                                                                                                              |

## What was verified

Five theorems, proved in the submission's `Main.lean`. Their statements are below exactly as they appear in the challenge file, which imports only Mathlib. All the mathematical notions in them (`riemannZeta`, `DirichletCharacter`, `DirichletCharacter.LFunction`, `IsPrimitive`) are Mathlib's.

**1\. The Riemann Hypothesis**, using Mathlib's own definition (`Mathlib/NumberTheory/LSeries/RiemannZeta.lean`): every zero of ζ other than the trivial zeros −2, −4, −6, … has real part 1/2. The condition `s ≠ 1` is there because Mathlib assigns ζ a conventional value at its pole.

```
def RiemannHypothesis : Prop :=
  ∀ (s : ℂ) (_ : riemannZeta s = 0) (_ : ¬∃ n : ℕ, s = -2 * (n + 1)) (_ : s ≠ 1), s.re = 1 / 2

theorem riemannHypothesis : RiemannHypothesis
```

**2\. The same statement written out** in the submission's own words. Lean accepts the two as equal by definition.

```
theorem riemannZeta_eq_zero_re :
  ∀ s : ℂ, riemannZeta s = 0 → (¬∃ n : ℕ, s = -2 * ((n : ℂ) + 1)) → s ≠ 1 → s.re = 1 / 2
```

**3\. No zeros to the right of the critical line, for every Dirichlet L-function.** For every modulus `N ≥ 1` and every Dirichlet character χ modulo N, primitive or not, L(χ, s) ≠ 0 whenever Re s > 1/2. The one excluded case is the trivial character at `s = 1`, where the L-function has its pole.

```
theorem LFunction_ne_zero_of_half_lt_re :
  ∀ (N : ℕ) [NeZero N] (χ : DirichletCharacter ℂ N) (s : ℂ),
    1 / 2 < s.re → (χ ≠ 1 ∨ s ≠ 1) → DirichletCharacter.LFunction χ s ≠ 0
```

**4\. Zeros in the critical strip lie on the critical line, for every Dirichlet L-function.**

```
theorem LFunction_eq_zero_re :
  ∀ (N : ℕ) [NeZero N] (χ : DirichletCharacter ℂ N) (s : ℂ),
    0 < s.re → s.re < 1 → DirichletCharacter.LFunction χ s = 0 → s.re = 1 / 2
```

**5\. The same for primitive characters**, the form in which the Generalized Riemann Hypothesis for Dirichlet L-functions is usually stated.

```
theorem LFunction_eq_zero_re_of_isPrimitive :
  ∀ (N : ℕ) [NeZero N] (χ : DirichletCharacter ℂ N), χ.IsPrimitive →
    ∀ s : ℂ, 0 < s.re → s.re < 1 → DirichletCharacter.LFunction χ s = 0 → s.re = 1 / 2
```

**Scope.** "GRH" here means Dirichlet L-functions: all moduli and all characters. Dedekind zeta functions, Hecke L-functions and automorphic L-functions in general are not among the statements that were checked.

## Arithmetic consequences

As a check that does not depend on Mathlib's definition of the zeta function, the proof was combined with [kimihiro64/Robin1984](https://github.com/kimihiro64/Robin1984) (commit `acab1a3`), an independent formalization of Robin's and Lagarias's criteria on the same Lean and Mathlib versions. Importing both gives two theorems about the divisor sum, with no hypotheses and no mention of zeta ([ArithSolution.lean](https://math.mit.edu/~drew/grh/ArithSolution.lean); statements alone in [ArithChallenge.lean](https://math.mit.edu/~drew/grh/ArithChallenge.lean), which imports only Mathlib):

```
theorem robin_inequality :
    ∀ n : ℕ, 5040 < n →
      ((ArithmeticFunction.sigma 1 n : ℕ) : ℝ) <
        Real.exp Real.eulerMascheroniConstant * (n : ℝ) * Real.log (Real.log (n : ℝ))

theorem lagarias_inequality :
    ∀ n : ℕ, 0 < n →
      ((ArithmeticFunction.sigma 1 n : ℕ) : ℝ) ≤
        (harmonic n : ℝ) + Real.exp (harmonic n : ℝ) * Real.log (harmonic n : ℝ)
```

| Robin1984 on its own (its three equivalences with RH)          | Result  | Detail                                                                                                                                                                                                |
| -------------------------------------------------------------- | ------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Robin1984 builds with `lake build`                             | pass    | 4,435 jobs, exit 0                                                                                                                                                                                    |
| con-leche `master` and `--compare`                             | accepts | 69,293 declarations accepted; all three theorems justified against its own challenge ([log](https://math.mit.edu/~drew/grh/robin1984-con-leche-compare.log))                                         |
| Unmodified Comparator v4.33.0 with the repository's own config | pass    | nanoda accepts, Lean default kernel accepts, "Your solution is okay!"; exit 0 at 03:31:06 UTC ([log summary](https://math.mit.edu/~drew/grh/robin1984-comparator-v4.33.0.log))                       |
| `lake comparator` (Lean v4.36.0) from exports                  | pass    | Lean paranoid kernel, nanoda and Lean default kernel all accept; "Your solution is okay!"; exit 0 at 04:16:52 UTC ([log summary](https://math.mit.edu/~drew/grh/robin1984-lake-comparator-4.36.log)) |

| The two inequalities (RH proof combined with Robin1984)               | Result  | Detail                                                                                                                                                                                        |
| --------------------------------------------------------------------- | ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `robin_inequality` and `lagarias_inequality` compile; `#print axioms` | pass    | `propext`, `Classical.choice`, `Quot.sound` only ([audit](https://math.mit.edu/~drew/grh/arith-audit.log))                                                                                   |
| con-leche `master` on the full combined export                        | accepts | 1,472,825 declarations accepted in `--verified` mode; exit 0 at 02:29:56 UTC ([log](https://math.mit.edu/~drew/grh/arith-con-leche-master.log))                                              |
| con-leche `--compare` against `ArithChallenge`                        | accepts | both theorems "justified by the solution"; exit 0 at 02:29:06 UTC ([log](https://math.mit.edu/~drew/grh/arith-con-leche-compare.log))                                                        |
| con-ron `--compare` (`master`, `3f22699b`) against `ArithChallenge`   | accepts | both theorems "justified by the solution"; 1,472,825 solution declarations accepted; exit 0 at 23:15:58 UTC on 21 November ([log](https://math.mit.edu/~drew/grh/arith-con-ron-compare.log)) |
| nanoda on the full combined export                                    | accepts | exit 0 at 02:56:46 UTC; 33 min 46 s on 64 threads ([log](https://math.mit.edu/~drew/grh/arith-nanoda.log))                                                                                   |
| `lake comparator` (Lean v4.36.0) on the combined export               | running | statement comparison and axiom check passed; its Lean kernel stages are single-threaded on a 62 GB export and are expected to take many hours, as for the main proof                          |

The combined export is 61,752,134,938 bytes, sha256 44651dcc482a6e06e8d626994221aff638e422fded189bf856e0c31176926f0d; its challenge export has sha256 088a57c9e7454fa5279a86a825e551828ac8431694855aadd1f6e6a6eb37bb1d. These statements still rely on Mathlib's definitions of the divisor sum, Euler's constant, harmonic numbers, `exp` and `log`, and on the Robin1984 proof, which is why that proof was put through the same checks.

### The elaborated statement of the Lagarias inequality

The kernels check elaborated terms, not source text, so what was verified is the term that Lean's elaborator produces from the statement. For the Lagarias inequality that term is short enough to read in full. This is the statement with every coercion, instance and implicit argument shown, as elaborated against Mathlib alone (Lean v4.33.1, Mathlib `0df444a3`), with none of the proof loaded:

```
def lagariasStmt : Prop :=
∀ (n : Nat),
  @LT.lt Nat instLTNat (@OfNat.ofNat Nat (nat_lit 0) (instOfNatNat (nat_lit 0))) n →
    @LE.le Real Real.instLE
      (@Nat.cast Real Real.instNatCast
        (@DFunLike.coe (@ArithmeticFunction Nat (@MulZeroClass.toZero Nat Nat.instMulZeroClass)) Nat (fun x => Nat)
          (@ArithmeticFunction.instFunLikeNat Nat (@MulZeroClass.toZero Nat Nat.instMulZeroClass))
          (ArithmeticFunction.sigma (@OfNat.ofNat Nat (nat_lit 1) (instOfNatNat (nat_lit 1)))) n))
      (@HAdd.hAdd Real Real Real (@instHAdd Real Real.instAdd) (@Rat.cast Real Real.instRatCast (harmonic n))
        (@HMul.hMul Real Real Real (@instHMul Real Real.instMul)
          (Real.exp (@Rat.cast Real Real.instRatCast (harmonic n)))
          (Real.log (@Rat.cast Real Real.instRatCast (harmonic n)))))
```

It names 27 constants directly: `Nat`, `Real`, the order and arithmetic instances on them, the casts from `Nat` and `Rat` to `Real`, `ArithmeticFunction.sigma`, `harmonic`, `Real.exp` and `Real.log`. Its meaning depends on 1,801 Mathlib and core declarations in all, counting definitions with their bodies and theorems by their statements only (a proof cannot change what a definition means). The corresponding count for Robin's inequality is 2,050; the difference is mostly the definition of Euler's constant.

Files: [the script](https://math.mit.edu/~drew/grh/explicit-statements.lean), [its output](https://math.mit.edu/~drew/grh/explicit-statements.out) (both statements), and the dependency lists for [Lagarias](https://math.mit.edu/~drew/grh/lagarias-deps.txt) and [Robin](https://math.mit.edu/~drew/grh/robin-deps.txt).

## Downloads

The exported proof: output of lean4export on module `Main`. Unpacks to 61,570,595,788 bytes.

compressed sha256 61a8938ad5f5e57397c1c0e3b954916af546d9004ade01c3555744a681b05381

unpacked sha256   5ea74b912147c5c93cc2b6d70cc727cf908df2f62ac6a20480a3fa02f6be244e

The exported challenge: the five statements with `sorry` proofs, importing only Mathlib. Unpacks to 350,862,970 bytes.

unpacked sha256   bd484ac9886a96f26b1d9a7a59ebd45b7a70d102c015d4f31f9302bbdd6ab2b9

Smaller files:

- [comparator.json](https://math.mit.edu/~drew/grh/comparator.json): Comparator config naming the checkers that can process this proof
- [SHA256SUMS](https://math.mit.edu/~drew/grh/SHA256SUMS): hashes of everything on this page
- [audit.log](https://math.mit.edu/~drew/grh/audit.log) and [Audit.lean](https://math.mit.edu/~drew/grh/Audit.lean): the axiom audit and the file that produced it
- [project-oleans.sha256](https://math.mit.edu/~drew/grh/project-oleans.sha256): sha256 of each of the 175,187 compiled files (29 MB). These hashes depend on the build directory; see [the note below](https://math.mit.edu/~drew/grh/#manifest-path).
- [nanoda.log](https://math.mit.edu/~drew/grh/nanoda.log): timing and exit status of the nanoda run
- [sandbox-timeline.txt](https://math.mit.edu/~drew/grh/sandbox-timeline.txt), [sandbox-build.sh](https://math.mit.edu/~drew/grh/sandbox-build.sh), [sandbox-build-continue.sh](https://math.mit.edu/~drew/grh/sandbox-build-continue.sh) and [sandbox-project-oleans.sha256](https://math.mit.edu/~drew/grh/sandbox-project-oleans.sha256): the sandboxed `lake build`, its scripts and its per-file hashes (29 MB)
- [con-leche-master.log](https://math.mit.edu/~drew/grh/con-leche-master.log) and [con-leche-compare.log](https://math.mit.edu/~drew/grh/con-leche-compare.log): the two con-leche runs

## Check it yourself

With Lean v4.36.0 installed, nothing needs to be built. You need roughly 100 GB of RAM and 70 GB of disk.

```
zstd -d --long=30 solution.ndjson.zst      # --long=30 is required
zstd -d --long=27 challenge.ndjson.zst
sha256sum solution.ndjson challenge.ndjson  # compare with the hashes above

lake comparator --config comparator.json \
  --challenge-from-export challenge.ndjson --solution-from-export solution.ndjson
```

nanoda alone took about 28 minutes on 80 threads. The Lean kernels are single-threaded and take much longer.

The fastest independent check is con-leche built from source (Lean v4.36.0, about two minutes to build). It needs about 50 GB of RAM and took under four minutes on 64 threads:

```
git clone https://github.com/leanprover/con-leche && cd con-leche
git checkout compare        # 5dc36dc1; master (cd2eceab) for the plain check
lake build con-leche
.lake/build/bin/con-leche --compare challenge.ndjson solution.ndjson
```

## Provenance

| Item                           | Value                                                                                                                                                                                                                                              |
| ------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Source archive                 | `lean.tar.gz`, the 14 parts from alpo.ge joined; 239,275,554 bytes sha256 9631b4f020199e580ba86178cb56c6e9b09f690c2369a80dc4334aa93f49c7fc                                                                                                         |
| Source mirror                  | [Deicyde/riemann-hypothesis](https://github.com/Deicyde/riemann-hypothesis), byte-identical to the archive apart from the Comparator files it adds                                                                                                |
| Lean / Mathlib                 | Lean v4.33.1 (commit `819816b2`); Mathlib tag `v4.33.1` (`0df444a3`)                                                                                                                                                                               |
| Exporter                       | lean4export tag `v4.33.0` (`15f6055e`), built with Lean v4.33.1; target list as in Deicyde's `export-targets.txt`                                                                                                                                  |
| Judge and extra kernels        | Lean v4.36.0: `lake comparator`, `leanchecker`, `leanchecker-paranoid`, `nanoda_bin`                                                                                                                                                               |
| lean4lean                      | [digama0/lean4lean](https://github.com/digama0/lean4lean) at `67f63b0c`, built with Lean v4.33.1; run on the compiled `.olean` files, not the export                                                                                              |
| con-leche                      | [leanprover/con-leche](https://github.com/leanprover/con-leche) `master` at `cd2eceab` and branch `compare` at `5dc36dc1`, each built with Lean v4.36.0                                                                                           |
| con-ron                        | [leanprover/con-ron](https://github.com/leanprover/con-ron) `master` at `3f22699b`, the Rust port of con-leche, built with `cargo build --release`                                                                                                |
| `Main.olean`                   | sha256 9146dfb4148e24d2252e04c50742f3e49dd126dfcc5006d10bf5f873ddf796ad                                                                                                                                                                            |
| Manifest of all compiled files | sha256 375cbee2fc3fc0f5088ef2e6dc9a5ac5530924310dc62422ec65aac9dc4c1d25 specific to the build directory `/home/ubuntu/grh/src/lean`, and one file in it is not deterministic; see [the note below](https://math.mit.edu/~drew/grh/#manifest-path) |
| Build directory                | the archive's `lean/` directory was at `/home/ubuntu/grh/src/lean` on every build machine, so each source file was passed to Lean as `/home/ubuntu/grh/src/lean/src/<Module/Path>.lean`                                                            |

## Caveats

- **How the original build was done, and the rebuild that checks it.** The original build ran on cloud machines without a sandbox, and compiled each file by calling `lean` directly with the same inputs `lake` supplies (module name, package name and the lakefile's options), not through `lake build`. All the kernel checks above were run on the export from that build. We then repeated the build on a separate machine with `lake build Main` inside a bubblewrap sandbox: no network, the source tree read-only, only `.lake` writable, the archive downloaded afresh from the author's site, and Mathlib's files checked against the official cache first. The export made inside that sandbox is byte-identical to the published one, so every check on the published export applies to it.
- **The per-file `.olean` hashes depend on the build directory, and one module is not deterministic.** In Lean v4.33.1 a compiled file stores each linter message together with the absolute path of its source file. Of our 175,187 compiled files, 98,219 contain the path `/home/ubuntu/grh/src/lean/src/…`; the other 76,968, including `Main.olean`, contain no path. So `project-oleans.sha256` can be reproduced only by a build whose source tree is at `/home/ubuntu/grh/src/lean`; elsewhere, only the files of modules that emit no linter messages should match.\
   The sandboxed `lake build` was run at that same path, and 175,186 of its 175,187 files are byte-identical to our manifest. The exception is `Formalism/LocalToGlobal/SiegelLatticeTheory/CokernelGLCounts/RankPointCounts/LocModel.olean`: same size, different contents. Recompiling that one module six times on one machine with identical inputs gave one output four times and the other twice, and those are exactly the two hashes in the two manifests. The two outputs differ in 38 bytes: the name of one bound variable in `BBtop.eq_1`, an equation lemma that Lean generates automatically the first time a proof asks for it. Two adjacent theorems in the file both ask for it, Lean elaborates theorem bodies in parallel, and the lemma takes that variable's name from whichever theorem gets there first. Bound-variable names play no part in kernel checking, so the two files are logically the same. The export is not affected, and the export from the sandboxed build is byte-identical to ours: an export contains only the declarations that the five theorems depend on, and `BBtop.eq_1` is not one of them, because the proofs that caused it to be generated do not refer to it. We checked that this name does not occur in the export. Had a proof on the dependency path referred to such a lemma by name, two builds could have produced exports with different hashes, although every check would still have passed on either. Compiled single-threaded (`lean -j1`), the module gave the same output six times out of six. A [ten-line file with no imports](https://math.mit.edu/~drew/grh/nondet-repro.lean) reproduces the behaviour: on Lean v4.33.1 and v4.34.1 it compiles to two different outputs with `-j32` and to one with `-j1`; on v4.36.0 it gave one output in 40 runs. The same race, with the first caller's options in place of its names, was reported as [Lean issue #15498](https://github.com/leanprover/lean4/issues/15498) and fixed by [pull request #15499](https://github.com/leanprover/lean4/pull/15499), first released in v4.36.0. That change also removes this behaviour: the file gives two outputs on the nightly builds of 7 and 8 October 2026 and one output in 40 runs on those of 9 and 10 October and on the toolchain built from the pull request alone. Because of this module, the sha256 of the whole manifest file is not reproducible even at the same path; compare file by file.
- A passing check shows that the formal statements, as written in Lean against Mathlib's definitions, follow from the three standard axioms according to the kernels used. It relies on those kernels and on Mathlib's definitions being what they are intended to be.
