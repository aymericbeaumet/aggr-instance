---
title: 'Leanstral 1.5: Proof Abundance for All'
link: https://mistral.ai/news/leanstral-1-5/
source: mistral-ai-news
published: 2026-07-02T13:55:54Z
updated: 2026-07-02T13:55:54Z
first_seen: 2026-09-19T21:30:23.395188495Z
content: extracted
html: 2026-07-02-leanstral-1-5-proof-abundance-for-all.html
preview:
  file: 2026-07-02-leanstral-1-5-proof-abundance-for-all.preview-f13b786ff79e.webp
  width: 256
  height: 134
  color: '#ef997d'
images:
- source: https://mistral.ai/cms-media/api/media/file/OG-mistral-main_1x.jpg
  original:
    file: 2026-07-02-leanstral-1-5-proof-abundance-for-all.image-28135271f04d.jpg
    width: 1200
    height: 630
  color: '#fefefe'
- source: https://mistral.ai/_astro/Leanstral-charts_Z1KXfze.webp?dpl=6aad049eaf4c2d00095b91e5
  original:
    file: 2026-07-02-leanstral-1-5-proof-abundance-for-all.image-4df42fde75da.webp
    width: 1920
    height: 1568
  variants:
  - file: 2026-07-02-leanstral-1-5-proof-abundance-for-all.image-0e8ebceb5cd9.webp
    width: 320
    height: 261
  - file: 2026-07-02-leanstral-1-5-proof-abundance-for-all.image-346c2596c558.webp
    width: 640
    height: 523
  - file: 2026-07-02-leanstral-1-5-proof-abundance-for-all.image-6ffc7d51642e.webp
    width: 960
    height: 784
  color: '#f9faf7'
- source: https://mistral.ai/_astro/Multiturn%20Lean%20verifier%20flow_Z2kh6w9.webp?dpl=6aad049eaf4c2d00095b91e5
  original:
    file: 2026-07-02-leanstral-1-5-proof-abundance-for-all.image-e7f48bc1ac58.webp
    width: 1920
    height: 1181
  variants:
  - file: 2026-07-02-leanstral-1-5-proof-abundance-for-all.image-c4f32e786404.webp
    width: 320
    height: 197
  - file: 2026-07-02-leanstral-1-5-proof-abundance-for-all.image-276370cc20ef.webp
    width: 640
    height: 394
  - file: 2026-07-02-leanstral-1-5-proof-abundance-for-all.image-a89b579ec3c5.webp
    width: 960
    height: 591
  color: '#272636'
- source: https://mistral.ai/_astro/Agentic%20Prover%20Figure%20Recreation_19csvW.webp?dpl=6aad049eaf4c2d00095b91e5
  original:
    file: 2026-07-02-leanstral-1-5-proof-abundance-for-all.image-a15643505dfa.webp
    width: 1920
    height: 1181
  variants:
  - file: 2026-07-02-leanstral-1-5-proof-abundance-for-all.image-b58aecc2411e.webp
    width: 320
    height: 197
  - file: 2026-07-02-leanstral-1-5-proof-abundance-for-all.image-aca7858a875a.webp
    width: 640
    height: 394
  - file: 2026-07-02-leanstral-1-5-proof-abundance-for-all.image-e362ca668aea.webp
    width: 960
    height: 591
  color: '#262636'
- source: https://mistral.ai/_astro/putnambench_test_time_scaling_NUlqb.webp?dpl=6aad049eaf4c2d00095b91e5
  original:
    file: 2026-07-02-leanstral-1-5-proof-abundance-for-all.image-817c525e4dec.webp
    width: 1920
    height: 1112
  variants:
  - file: 2026-07-02-leanstral-1-5-proof-abundance-for-all.image-748825e07993.webp
    width: 320
    height: 185
  - file: 2026-07-02-leanstral-1-5-proof-abundance-for-all.image-3956b6f9a3d4.webp
    width: 640
    height: 371
  - file: 2026-07-02-leanstral-1-5-proof-abundance-for-all.image-9c5c27771f89.webp
    width: 960
    height: 556
  color: '#fdfdfd'
- source: https://mistral.ai/_astro/chart-2_ZSNQjN.webp?dpl=6aad049eaf4c2d00095b91e5
  original:
    file: 2026-07-02-leanstral-1-5-proof-abundance-for-all.image-98a8f9a1b9ec.webp
    width: 1920
    height: 1294
  variants:
  - file: 2026-07-02-leanstral-1-5-proof-abundance-for-all.image-d24104be5d76.webp
    width: 320
    height: 216
  - file: 2026-07-02-leanstral-1-5-proof-abundance-for-all.image-b65466fd48f3.webp
    width: 640
    height: 431
  color: '#fafafa'
---

Thinking

Summary

Leanstral 1.5, a free Apache-2.0 licensed model with 6B active parameters, delivers a major performance upgrade in formal verification, saturating miniF2F, solving 587/672 PutnamBench problems, and achieving state-of-the-art results on FATE-H (87%) and FATE-X (34%). Trained through mid-training, supervised fine-tuning, and reinforcement learning with CISPO, it excels in agentic proof engineering and real-world code verification, uncovering 5 previously unknown bugs across 57 repositories tested. Fully open-sourced and available via Hugging Face and a free API, Leanstral 1.5 is now accessible for practical proof engineering in Lean 4.

Since its launch, Leanstral has offered an open, practical approach to proof engineering in [Lean 4](https://leanprover.github.io/). Today, we are releasing **Leanstral 1.5**, a free Apache-2.0 licensed model with 119B total and only 6B active parameters, delivering a performance upgrade that makes formal verification more powerful and accessible than ever.

Leanstral 1.5 **saturates miniF2F,** solves **587/672 PutnamBench problems,** and achieves a new state-of-the-art of **%87 on FATE-H** and **34% on FATE-X**. Beyond benchmarks, it verifies complex code properties and **uncovers previously unknown bugs in open-source repositories**—proving that rigorous formal methods can be both effective and practical for real-world use.

![](https://mistral.ai/_astro/Leanstral-charts_Z1KXfze.webp?dpl=6aad049eaf4c2d00095b91e5)

## **Training Leanstral**

Leanstral 1.5 goes through a three-stage process: mid-training, supervised fine-tuning, and reinforcement learning with CISPO. Leanstral 1.5 leverages extensive training on two RL environments:

In the **multiturn environment**, the model is given a theorem statement and must either prove or disprove it. The model submits a proof, receives Lean compiler feedback, and refines its approach with each attempt. If the proof compiles it succeeds; otherwise the loop continues until the model either solves the problem or exhausts its budget.

![](https://mistral.ai/_astro/Multiturn%20Lean%20verifier%20flow_Z2kh6w9.webp?dpl=6aad049eaf4c2d00095b91e5)

In the **code agent environment**, Leanstral operates like a developer in a raw filesystem: it edits files, runs bash commands, and uses the Lean language server to inspect goals, errors, and type information in real time. This allows it to tackle long-horizon tasks like completing partial proofs in a repository, building auxiliary lemmas, and persisting through multiple rounds of context compaction. The model learns to navigate the full proof-engineering workflow and is finally verified by [our fork of SafeVerify](https://github.com/mistralai/LeanstralSafeVerify) for correctness given a list of target theorems.

![](https://mistral.ai/_astro/Agentic%20Prover%20Figure%20Recreation_19csvW.webp?dpl=6aad049eaf4c2d00095b91e5)

## **Evaluation**

We evaluate Leanstral on the following benchmarks:

- **miniF2F** is a cross-system benchmark for formal mathematics, ranging from elementary problems to IMO-level challenges, testing diverse proof abilities across algebra, combinatorics, and number theory.

- **PutnamBench** consists of 672 problems from the Putnam Mathematical Competition, requiring deep reasoning and long proof chains to solve challenging mathematical problems.

- **FATE-H and FATE-X** are abstract algebra benchmarks for graduate and PhD-level problems, respectively, testing advanced reasoning in areas like group theory, ring theory, and module theory.

- **FLTEval** is based on real pull requests from the Fermat’s Last Theorem repository, testing practical proof engineering with real-world complexity.

We saturate miniF2F completely, reaching 100% on both the validation and test sets. On PutnamBench and FATE-H/X, we compare Leanstral 1.5 against Goedel-Architect without natural-language guidance, Seed-Prover 1.5 at its high setting, and AxProverBase. Leanstral reaches a new state-of-the-art on FATE-H/X, solving 87 and 34 problems respectively. On PutnamBench, it edges out Seed-Prover 1.5 high by 7 problems at far lower cost: about $4 per problem, against an estimated $300 or more for Seed-Prover, whose high setting runs with a budget of 10 H20-days per problem. The only provers ranked higher operate under different conditions—some receive natural-language proof guidance, others cost far more to run, like Aleph Prover at $54–68 per problem.

Leanstral 1.5 shows the strongest test-time scaling we have seen from a formal-reasoning model. The figure below tracks Pass@8 on PutnamBench as we raise the token budget per attempt from 25k to 4M: performance climbs smoothly and monotonically the whole way, from 44 problems solved at 50k to 244 at 200k, 493 at 1M, and 587 at 4M. Rather than giving up when a proof runs long, Leanstral keeps reasoning, editing files, and revising across millions of tokens, turning that budget directly into solved problems—the same behavior behind the AVL-tree proof below, which ran for over 2.7 million tokens across 22 compactions.

![](https://mistral.ai/_astro/putnambench_test_time_scaling_NUlqb.webp?dpl=6aad049eaf4c2d00095b91e5)

With this release, we also fully open source [FLTEval](https://github.com/mistralai/FLTEval). Leanstral 1.5 lifts pass@1 on the benchmark from 21.9 to 28.9 and pass@8 from 31.9 to 43.2, surpassing Opus 4.6's 39.6 at one-seventh the cost. It also widens its lead over open-source models 3–10× larger, as shown in the figure below.

![](https://mistral.ai/_astro/chart-2_ZSNQjN.webp?dpl=6aad049eaf4c2d00095b91e5)

## **Code Verification Case Studies**

While being primarily trained for mathematics, Leanstral 1.5 exhibits strong abilities in code verification. We present 2 critical case studies to demonstrate its impact.

### **AVL Trees: Proving Time Complexity**

AVL trees are self-balancing binary search trees that maintain O(log n) height through rebalancing during insertions and deletions. Leanstral 1.5 proved these time complexity guarantees for a real implementation—a task that required structural induction to mirror the tree’s recursive structure, careful handling of monadic time tracking, and exhaustive case analysis for rebalancing paths. Over 2.7 million tokens and 22 compactions, Leanstral systematically unfolded each layer of the TimeM monad, exposing the underlying computations despite their interleaving with control flow. It established an almost tight bound of 48 steps per height unit plus a constant for insertion, then connected height to tree size via a logarithmic relationship, delivering complete, verified proofs that insertion and deletion are indeed O(log n).

### **Bug Discovery: Finding Hidden Flaws**

To test Leanstral’s bug-catching abilities, we built an automated pipeline: Aeneas translates Rust code to Lean, while Leanstral infers the user intent and generates correctness properties from the code. Leanstral then attempts to prove each property in four attempts. If they all fail, it tries to prove the negation instead, also with four attempts. Across 57 tested repositories, this process flagged 47 violated properties, with 11 pointing to genuine bugs—5 of them previously unreported on GitHub.

One such bug was in the sign function for zigzag decoding of the [datrs/varinteger](https://github.com/datrs/varinteger) library. On input Std.U64.MAX, the expression (value + 1) overflowed, causing crashes in debug mode and silent corruption in release mode—an edge case that testing and fuzzing would typically miss. Leanstral’s pipeline caught it automatically, demonstrating that formal verification can already be applied to real-world codebases and find bugs that some traditional methods overlook.

## **Get Started**

Leanstral 1.5 has a **Apache-2.0** license. The weights can be found [on Huggingface](https://huggingface.co/mistralai/Leanstral-1.5-119B-A6B), while also being available now [as a free API endpoint](https://docs.mistral.ai/models/model-cards/leanstral-1-5) as `leanstral-1-5`. We recommend using it in Mistral Vibe. To begin your journey, grab an API Key, and:

**1\. Set up Mistral Vibe**

```bash
uv tool install mistral-vibeuv tool update mistral-vibevibe --setup
```

**2\. Install Leanstral 1.5**

```bash
/leanstallexit
```

**3\. Launch the agent**

```bash
vibe --agent lean
```

**4\. Install Lean LSP MCP (Optional)**

It is highly recommended to install [Lean LSP MCP](https://github.com/oOo0oOo/lean-lsp-mcp) by adding the following to your `~/.vibe/config.toml`

```python
[[mcp_servers]]name = "lean-lsp"transport = "stdio"command = "uvx"args = ["lean-lsp-mcp"]tool_timeout_sec = 600
```

If there are no existing MCP servers, you may have to remove `mcp_servers = []`.

**5\. Start proving**

Ask Leanstral to tackle a theorem, debug a proof, or contribute to a repository. It’s that simple.
