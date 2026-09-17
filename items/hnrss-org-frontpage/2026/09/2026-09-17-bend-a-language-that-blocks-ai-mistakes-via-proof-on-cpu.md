---
title: Bend – A language that blocks AI mistakes via proof, on CPU and GPU
link: https://bend-lang.com/
source: hnrss-org-frontpage
published: 2026-09-17T20:36:13Z
updated: 2026-09-17T20:36:13Z
first_seen: 2026-09-17T23:03:54.779334643Z
authors:
- nicolas-siplis
summary: 'Article URL: https://bend-lang.com/ Comments URL: https://news.ycombinator.com/item?id=49746163 Points: 186 # Comments: 100'
content: extracted
html: 2026-09-17-bend-a-language-that-blocks-ai-mistakes-via-proof-on-cpu.html
---

[\~/bend](https://bend-lang.com)

[](https://github.com/bendlang/bend "github")\
[](https://discord.bend-lang.com "discord")\
[](https://x.com/bendlang "x")\
[](https://www.reddit.com/r/bendlang/ "reddit")\
[hub](https://hub.bend-lang.com)\
[bender](https://bend-lang.com/bender)\
[get Bend](https://bend-lang.com/#get)

[](https://bend-lang.com/#!)

[×](https://bend-lang.com/#! "close")

### 1.Install

```
curl -fsSL https://bend-lang.com/install.sh | sh
```

### 2.Add this to your AGENTS.md

```
When using Bend:
- run `bend guide` to learn it
- use `LAWS.bend` to keep important rules
- run `bend PROOF.bend` before committing
- parallelize the code whenever possible
```

### 3.Enjoy bug-free, fast vibe-coded apps!

a **fast** language that **blocks AI mistakes** via **proof**

**C** speed · **CUDA** parallelism · **Lean** proofs · **Python** syntax

In the post-AGI economy, humans will eventually stop writing and reading code, but we still need an ambiguity-free way to tell the AIs building the world around us what we want done.

With **laws**, our intents can be much more precise than natural language. With **proofs**, we can verify that the AI implemented our prompts correctly. And a **fast compiler** runs it at speed.

That's Bend - and nothing else.

## 1.Bend runs **FAST**.

Bend compiles to native code. On one core, it runs nearly as fast as C. The same binary also runs on sixteen cores, or on the GPU, running up to a hundred times faster than one core.

Apple M4 Max · lower is better

## 2.Bend compiles **FAST**.

Bend's type checker is a proof checker, as in Lean and Rocq. Those can take minutes on a mid-sized codebase. Bend takes a second at most, so an AI agent can check after every change.

Apple M4 Max · lower is better

## 3.Bend is **PARALLEL**.

No threads, no locks, no kernels to write. Split the work in two, and Bend spreads the calls over every core it can find, then joins them back. Now watch pow2 run on 4,096 GPU cores:

pow2.bend running on the **GPU**

## 4.Bend **BLOCKS** mistakes - with proof

How can you **trust** code you never read? By demanding a **proof**. LAWS.bend is where you declare laws. From then on, no AI can ship one line that breaks them, ever. Watch it guard a game:

**Law**: winning is **impossible**

So far, it works!

**New feature:**

“Claude, make the board wrap around”

**Without LAWS.bend:**

Laws broken. AI mistake: **merged**.

**With LAWS.bend:**

Laws intact. AI mistake: **blocked**!

Without LAWS.bend, the bug went live. With LAWS.bend, the AI had to retry until it built a wall and proved the law holds. Merging a bug is mathematically impossible: it is a *theorem*.

LAWS.bend

```
# LAW: no move sequence leads to victory.
law you_cant_win:
  for moves: List<Move>            # any sequence of moves
  board = replay(start(), moves)   # replayed from the start
  is_won(board) == False{}         # never leads to victory
```

PROOF.bend

```
# PROOF: you_cant_win holds.
def Laws.you_cant_win(moves):
  # ... written by the AI
```

`LAWS.bend` is `AGENTS.md` backed by **proof**. “Make no mistakes” is now *type-checked*.

[Skeptical? Try breaking the game.](https://bend-lang.com/#lab)

## 5.Get started.

### 5.1.Install

```
curl -fsSL https://bend-lang.com/install.sh | sh
```

### 5.2.Tell your agent to use Bend

Add this to your `AGENTS.md`:

```
When using Bend:
- run `bend guide` to learn it
- use `LAWS.bend` to keep important rules
- run `bend PROOF.bend` before committing
- parallelize the code whenever possible
```

Then, just say: "use Bend"!

### 5.3.Enjoy bug-free, fast vibe-coded apps!

Hints: ask it to write **laws** for whatever should never break, and to **parallelize** everything you want running fast. Bend is young: if anything goes wrong, ask it to open an issue. Bend works best on the back-end, on Linux and on macOS. Enjoy! <3

## 6.References.

Guide: [GUIDE.md](https://github.com/bendlang/bend/blob/main/guide/GUIDE.md) is the whole language; `bend guide` prints it. Paper: [BendTT](https://github.com/bendlang/bend/blob/main/paper/BendTT.pdf), an affine dependent type theory, Bend's core. Paper: [BendRT](https://github.com/bendlang/bend/blob/main/paper/BendRT.pdf), a parallel runtime for CPUs and GPUs, the VM.

Bend is still evolving. Expect bugs, and please [report them](https://github.com/bendlang/bend/issues).
