---
title: 'Show HN: A working Nintendo in ~1GB of Qwen weights'
link: https://github.com/dbreunig/nesformer
source: hnrss-org
published: 2026-10-30T16:40:22Z
updated: 2026-10-30T16:40:22Z
first_seen: 2026-10-30T22:55:36.634700404Z
authors:
- dbreunig
content: extracted
html: 2026-10-30-show-hn-a-working-nintendo-in-1gb-of-qwen-weights.html
remote_preview:
  url: https://opengraph.githubassets.com/9db3963b025cbda16f96455ae5a189f29469d306aafae65a32db24bf00bfcce4/dbreunig/nesformer
  alt: An NES CPU compiled into the weights of a stock Qwen3.5 model - dbreunig/nesformer
---

**An NES CPU compiled into the weights of a stock Qwen3.5 model.**

The NES's 6502 CPU (the core of its 2A03 chip), its 2 KB of work RAM, and a cartridge's program ROM run inside a standard `Qwen3_5ForCausalLM` checkpoint. Nobody trained these weights; a compiler computed every one. When you play a game, the model predicts the CPU's execution log one token at a time. A small host program plays the rest of the console around it: it draws the picture, reads the controller, and fires the vblank interrupt.

nesformer builds on [Qwenputer](https://github.com/divagr18/qwenputer) by [Divyansh Agrawal](https://divagr.com/blog/qwenputer), which compiled a CHIP-8 and then a RISC-V CPU into Qwen3.5-4B. It reuses Qwenputer's compiler and its central trick: **the model's Gated DeltaNet state is the machine's memory.**

| Super Mario Bros. (World 1-1)                                                                                                                                                       | Full Game by Doug Fraker (title screen and first level)                                                                                                                               |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [![Super Mario Bros., played by the weights](https://github.com/dbreunig/nesformer/raw/main/examples/demo.gif)](https://github.com/dbreunig/nesformer/blob/main/examples/demo.gif) | [![Full Game, played by the weights](https://github.com/dbreunig/nesformer/raw/main/examples/full_game.gif)](https://github.com/dbreunig/nesformer/blob/main/examples/full_game.gif) |

Every CPU token in these runs came from the model. For the bundled test games, every frame also matches a conventional 6502 emulator's output exactly (see [How it was checked](https://github.com/dbreunig/nesformer#how-it-was-checked)).

## Quick start

### 1\. Folder layout

Unzip `nesformer.zip` and `nesformer-weights.zip` into the same folder. The weights go *next to* the `nesformer/` package, not inside it:

```
nesformer/              <- run commands from here
├── nesformer/          <- the Python package
├── chipformer/         <- Qwenputer's compiler
├── roms/               <- test ROMs (put your own here too)
├── examples/           <- sample GIFs and frame sheets
├── tests/
├── LICENSE
├── LICENSE.chipformer
├── README.md
├── pyproject.toml
└── weights/            <- from nesformer-weights.zip
    ├── config.json
    ├── generation_config.json
    ├── model.safetensors
    ├── nesformer.json
    └── SHA256SUMS
```

The CLI looks for `weights/` beside the `nesformer/` package. To keep the weights elsewhere, pass `--weights /path/to/weights` to `run`, `check` or `trace`, or give the path to `info`.

To confirm the weights unzipped correctly, run `shasum -a 256 -c weights/SHA256SUMS` from the top folder.

### 2\. Install

You need Python 3.11 or newer.

```shell
python3 -m venv .venv
source .venv/bin/activate          # Windows: .venv\Scripts\activate
pip install numpy scipy numba pillow
```

### 3\. Play a game

```shell
python -m nesformer run roms/thwaite.nes --frames 600 --show \
  --buttons "start@90-95+200-205,a@230-233+250-253+270-273+290-293+310-313+330-333+350-353+370-373+390-393+410-413+430-433+450-453+470-473+490-493+510-513+530-533+550-553+570-573+590-593"
```

`--show` draws each frame in your terminal as the model generates it. When the run ends, the CLI saves an animated GIF named after the ROM (here `thwaite.gif`) in the current folder.

Expect it to be slow. The first run spends about 20 seconds compiling the engine with numba. After that, a frame takes 1–6 seconds, so 600 frames (10 seconds of NES time) take around 25 minutes.

## Playing your own ROMs

Point `run` at any `.nes` file that uses the NROM board (iNES mapper 0), such as Super Mario Bros., Donkey Kong or Excitebike. You supply the ROM; nesformer only ships the three freely licensed test ROMs in `roms/`.

```shell
python -m nesformer run roms/smb.nes --frames 600 --show \
  --buttons "start@60-65,right@200-600,a@250-265+300-315+360-375+420-435+480-495+540-555"
```

That script starts the game, then walks Mario right and jumps about once a second. The CLI loads the ROM into the model's state at startup, so the same weights run any NROM game without a rebuild. Games on other boards stop with a message: see [Limitations](https://github.com/dbreunig/nesformer#limitations).

## Controller scripts

`--buttons` takes a comma-separated list of entries. Each entry names one or more buttons joined by `+`, then `@`, then one or more frame ranges joined by `+`:

```
start@60-65                 press Start during frames 60 to 65
right@200-600               hold Right from frame 200 to frame 600
a@250-265+300-315           press A twice
right+b@200-600             hold Right and B together (run, in Super Mario Bros.)
```

The buttons are `a`, `b`, `select`, `start`, `up`, `down`, `left` and `right`. Frames count from power-on at 60 per second, so frame 600 is 10 seconds in. Games ignore input on screens where you can't move, so holding a button a little early does no harm. In Super Mario Bros., a longer A press makes a higher jump.

## Commands

| Command                         | What it does                                                                     |
| ------------------------------- | -------------------------------------------------------------------------------- |
| `python -m nesformer run ROM`   | Play a ROM on the weights and save a GIF                                         |
| `python -m nesformer check ROM` | Run the weights and the reference CPU side by side; stop at the first difference |
| `python -m nesformer trace ROM` | Print the model's token stream (default 40 tokens)                               |
| `python -m nesformer build`     | Compile the circuit and write `weights/` (about 10 seconds)                      |
| `python -m nesformer info`      | Describe the checkpoint                                                          |

Options for `run`:

| Option             | Default  | Meaning                                                                                             |
| ------------------ | -------- | --------------------------------------------------------------------------------------------------- |
| `--frames N`       | 120      | Stop after N frames                                                                                 |
| `--buttons SCRIPT` | none     | Controller input (see above)                                                                        |
| `--show`           | off      | Draw frames in the terminal (needs 24-bit color, e.g. iTerm2 or Windows Terminal)                   |
| `--gif PATH`       | ROM name | Where to save the animated GIF                                                                      |
| `--png DIR`        | off      | Save each kept frame as a PNG in DIR. With `--png` alone, no GIF is saved. Add `--gif` to get both. |
| `--every N`        | 1        | Keep every N-th frame                                                                               |
| `--scale N`        | 2        | Enlarge saved frames N times                                                                        |
| `--engine`         | `fast`   | `fast` (numba), `numpy`, or `hf` (stock transformers; needs torch, ~10 tokens/s)                    |
| `--no-skip`        | off      | Run every iteration of idle wait loops (slower; identical frames)                                   |
| `--max-tokens N`   | none     | Stop after N tokens                                                                                 |
| `--trace FILE`     | none     | Save the token ids                                                                                  |

`run`, `check` and `trace` all accept `--weights`, `--engine`, `--buttons` and `--no-skip`. `check --tokens N` sets how far to compare (default 50,000). `trace --tokens N` sets how many tokens to print (default 40). `build --out DIR` writes the checkpoint somewhere other than `weights/`.

## How it works

### What's in the weights, and what isn't

| In the model (weights + DeltaNet state)                                                         | In the host (`nesformer/host.py`)                       |
| ----------------------------------------------------------------------------------------------- | ------------------------------------------------------- |
| The 6502 core: all 151 official opcodes, every addressing mode, flags, stack, BRK and NMI entry | The PPU: a scanline renderer and the palette            |
| 2 KB of work RAM                                                                                | Controllers; APU registers (written, otherwise ignored) |
| 32 KB of PRG-ROM, stored twice: one copy for instruction fetch, one for data reads              | NMI timing at vblank, injected as an `NMI` token        |
| OAM DMA reads (the 256 bytes appear as `DM` tokens)                                             | Answers to I/O reads, injected as `IN` tokens           |

This split follows Qwenputer's CHIP-8 build, whose host injects timer ticks and draws the display from the trace. The host never sees the CPU's registers or RAM except through the trace.

The memory trick works like this. Each DeltaNet head keeps a 128 × 128 state matrix that the delta rule updates at every token. With the gate held open and a one-hot key, an update overwrites exactly one row of that matrix, and a one-hot query reads it back. So every row is a memory cell, and the cartridge, RAM and registers all live in rows.

### The checkpoint

`weights/` is an ordinary Hugging Face folder. It loads with `Qwen3_5ForCausalLM.from_pretrained` and needs no custom code.

|              |                                                                                                                            |
| ------------ | -------------------------------------------------------------------------------------------------------------------------- |
| Architecture | `Qwen3_5ForCausalLM`, with Qwen3.5-4B's DeltaNet head shapes (16 key heads, 32 value heads, dim 128) and MLP width (9,216) |
| Layers       | 7 Gated DeltaNet layers (the circuit), plus 1 all-zero attention layer that Hugging Face's cache needs                     |
| Hidden size  | 768                                                                                                                        |
| Vocabulary   | 263,808 tokens                                                                                                             |
| Parameters   | 466.6M in bf16 (933 MB), of which **9.3M are nonzero**, so the weights zip to 3.3 MB                                       |

`nesformer.json` records which layer and head holds each part of the machine. A run writes the cartridge, registers and RAM into those DeltaNet states, then decodes greedily.

### Layer plan

| Layer | Attention (state)                                                                          | MLP                                                                            |
| ----- | ------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------ |
| 1     | PC anchor; instruction counters; A, X, Y, SP; flags N V Z C I D; pointer latches; NMI flag | counter thermometers; SP ± 1; LO + Y                                           |
| 2     | —                                                                                          | PC = anchor + bytes executed since; PC + 1, PC + 2                             |
| 3     | PRG fetch: opcode and two operand bytes at the PC                                          | opcode decode; operand bits; indexed sums; branch test and target              |
| 4     | —                                                                                          | effective address, by addressing mode and record stage; the byte a push stores |
| 5     | RAM and PRG data reads at the effective address                                            | operand; ADC/SBC adder, compares, BIT; address decode                          |
| 6     | —                                                                                          | result byte, carry, overflow, flags, jump targets                              |
| 7     | —                                                                                          | the next token                                                                 |

### The trace

Each instruction becomes a record, and the record's last token commits it. As in Qwenputer's RISC-V core, the PC is implied rather than written: each final token carries its instruction's length, three DeltaNet counters tally the instructions of each length since the last jump, and the PC is the jump target plus that offset. Excerpts from Full Game:

```
SEI            P(000100 px0 L1)
LDX #$40       X($40 c0 v0 px0 L2)
STX $4017      IOW($4017=$40 L3)
TXS            S($FF L1)
BIT $2002      IOR($2002) IN($00) P(000110 px0 L3)       <- the host injects IN
BPL $8017      J($8017)
JSR $8279      PSH($FE) PSH($FD) J($8279)                 <- the return address is pushed in-circuit
RTS            PUL($FC) LO($7B) PUL($FD) J($827C)
STA ($72),Y    LO($08) HI($03) ST($00 a7=0 L2)            <- pointer bytes latched, then the store
STA $4014      IOW($4014=$02 L0) DM(0:$FF) … DM(255:$00) NT(L3)   <- OAM DMA
```

Most instructions take a single token; real game code averages 1.1–1.3 tokens per instruction. `nesformer/spec.py` documents every token family.

## How it was checked

- **The reference CPU** (`nesformer/cpu.py`) matches nestest's golden log for 3,348 instructions, and the py65 emulator on 5.7 million instructions of game code.

- **The exact circuit**, run in Qwenputer's evaluator with its bf16 exactness checks, predicts every reference token: all of nestest plus 283,610 positions of two games' boot sequences.

- **The checkpoint in stock transformers** (bf16, CPU) predicts 2,651 positions of nestest and Full Game with no mismatches.

- **The weights, free-running**, generate traces identical to the reference CPU's:

  | Run                        | Tokens    | Frames | NES time | Result                                 |
  | -------------------------- | --------- | ------ | -------- | -------------------------------------- |
  | nestest (official opcodes) | 4,586     | —      | —        | identical, ending in the expected HALT |
  | Thwaite, `check`           | 700,000   | 271    | 4.5 s    | identical trace and frames             |
  | Thwaite, `run`             | 1,916,527 | 600    | 10.0 s   | all 300 saved frames match             |
  | Full Game, `run`           | 2,645,885 | 420    | 7.0 s    | all 210 saved frames match             |

- **The idle-loop skip** changes nothing visible: frames and I/O writes, cycle timestamps included, match a run without it.

## Speed

The default engine computes Qwen3.5's forward pass from `model.safetensors`, compiled with numba and skipping the all-zero blocks. It ran at about 1,650 tokens/s on one CPU core of the machine it was built on. The plain numpy engine does about 210, and stock transformers about 10. A real NES would need about 700,000 tokens per second.

Games spend most of each frame in a loop that waits for vblank. When the host sees a loop body repeat exactly, with no RAM or I/O writes, it advances the clock to just before the next PPU event, and the remaining iterations still run. That cuts the token count by 2.6× to 4.2×. Overall, nesformer runs 150–290× slower than the real console.

## Limitations

- **NROM cartridges only (iNES mapper 0).** The CPU can address only 32 KB of program ROM at a time. Bigger games carry a bank-switching chip (a *mapper*, such as MMC1 or MMC3) that swaps which part of the ROM the CPU sees. nesformer's ROM reads happen inside the model, from a fixed 32 KB table, so it can't follow a bank switch yet. Supporting a mapper means storing the whole ROM in the state, keeping the bank registers there too, and adding one stage that turns each CPU address into a ROM address.
- **Official opcodes only.** An unofficial opcode emits `HALT`.
- **Code must run from the cartridge ROM.** A jump into RAM emits `HALT`.
- **No IRQs** (APU frame IRQ, DMC). BRK and NMI work.
- Read-modify-write instructions on I/O registers don't produce an I/O write, and $4020–$7FFF reads as 0 (no PRG-RAM).
- The PPU and APU aren't in the weights. The PPU renders one scanline at a time, so mid-scanline effects aren't reproduced. Sprite-0 hit and sprite overflow are approximate, and there's no sound.

## Tests

```shell
pip install pytest py65
python -m pytest -q tests
```

The tests take a few minutes. They check the reference CPU against nestest's log, the circuit against the reference, the weights free-running against the reference, and the idle-loop skip.

## Running the checkpoint in stock transformers

```shell
pip install "torch>=2.6" "transformers>=5.22"
python -m nesformer run roms/thwaite.nes --frames 3 --engine hf
```

`nesformer/hfcheck.py` shows the whole procedure: load the checkpoint, decode one padding token, write the machine's state into the cache's DeltaNet states, then feed tokens one at a time.

## Files

| Path                                                    | Contents                                                                                      |
| ------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| `nesformer/spec.py`                                     | token families and records                                                                    |
| `nesformer/cpu.py`                                      | the reference 6502 that writes the trace                                                      |
| `nesformer/host.py`                                     | everything around the CPU: PPU, controllers, NMI timing, cycle counting, idle skip            |
| `nesformer/core.py`                                     | the circuit and the state snapshot                                                            |
| `nesformer/weights.py`                                  | the emitter that turns the circuit into a `Qwen3_5ForCausalLM` checkpoint                     |
| `nesformer/engine.py`, `fastengine.py`                  | run the weights (numpy / numba)                                                               |
| `nesformer/hfcheck.py`                                  | run the weights in stock transformers                                                         |
| `nesformer/run.py`, `check.py`, `cli.py`, `__main__.py` | drivers, the teacher-forced check, the CLI                                                    |
| `chipformer/`                                           | Qwenputer's q35 compiler (`q35/` emitter and primitives, `dsl/` graph)                        |
| `weights/`                                              | the compiled checkpoint and its SHA-256 checksum                                              |
| `roms/`                                                 | test ROMs; `roms/SOURCES.md` lists their authors and licenses                                 |
| `examples/`                                             | GIFs and frame sheets from the runs; `demo.gif` is the Super Mario Bros. run shown at the top |
| `tests/test_nesformer.py`                               | the test suite                                                                                |

## Credits

- [Qwenputer](https://github.com/divagr18/qwenputer) and its `chipformer` compiler: MIT, © 2026 Divyansh Agrawal (`LICENSE.chipformer`). That license also covers the code adapted from Qwenputer in `nesformer/core.py` and `nesformer/weights.py`. Its last paragraph refers to Qwenputer's CHIP-8 ROMs, which this repository does not include.
- Test ROMs: nestest by Kevin Horton; Full Game by Doug Fraker (MIT); Thwaite by Damian Yerrick (GPL-3.0, source at [pinobatch/thwaite-nes](https://github.com/pinobatch/thwaite-nes)).

## License

nesformer is released under the MIT License (see `LICENSE`). The ROMs in `roms/` keep their own licenses, listed in `roms/SOURCES.md`.
