---
title: LLM assisted reverse engineering
link: https://www.slowchronicle.com/posts/llm-assisted-reverse-engineering
source: hnrss-org
published: 2026-11-18T22:29:13Z
updated: 2026-11-18T22:29:13Z
first_seen: 2026-11-19T07:50:29.493483202Z
authors:
- ioma8
labels:
- ai
- ghidra
- reverse engineering
- rust
content: extracted
html: 2026-11-18-llm-assisted-reverse-engineering.html
remote_preview:
  url: https://www.slowchronicle.com/og-default.png
  alt: LLM assisted reverse engineering cover
---

I have been hacking both hardware and software for 2 decades. Previously manually. But in 2026 with the help of AIs the proccess is greatly sped up. Below I will describe my proccess briefly or how I do it.

## Set Up

Tools which I mostly use are these:

| TOOL                                   | use                                                                                                                  |
| -------------------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| DeepSeek Harness + Deepseek v4.1 Flash | Thats the AI I am using. Occasionaly I use Codex with Luna or Terra but Deepseek is super performant, fast and cheap |
| Ghidra + MCP bridge                    | The best in the market, free, disassembler and decompiler. With the MCP bridge agents can use it very well           |
| radare2 and kuna                       | CLI disassemblers and decompilers. Not as good as Ghidra but much faster and possibly better usable for AIs          |

## Process

What I basically do nowadays is just point the agent at the binary to be analysed and use my set of dedicated agent skills. Rest usually takes care of itself. But putting the skills besides, the flow is basically this.

1. Find binary type, platform, build etc - based on this using appropriate decompiler (note - current AIs do fairly well with just disassembly . ie pure x86 or ARM instructions).
2. Command agent to document all findings along the way to MD files. This serves several purposes. They become notes for your own understanding. They help the agent not lose the track after compaction / handoff to different agent. And last but not least, it helps the agent work more effectively, not get lost in circles etc. More about this later.
3. Ask the agent to analyse basic structure. Main functions, main branching, imported functions etc of given binary. By binary I mean whatever. Desktop executable, website or firmware image.

Once I have this basic overview I can usually proceed any way I need to do modifications to the original binary or build mods or just help me understand it better.

On some occasions some things are not easily resolvable by simple disassembly or decompilation. In those cases you can do:

- Let the agent debug given binary and set breakpoints on specific parts to help it figure out given inner workings.
- Use logging. Either by somehow enabling it in the original binary or by making a probe (and this can mean many things) that injects to the original binary and records you via logs (to file) whatever is needed to resolve given feature.
- Give agent outside access - ie computer use for desktop apps, or api controllable emulator for running and controlling given firmware.
- Ask agent to try different approach. But you must be specific. Ie when it gets stuck in disasembly by following xrefs, ask it to try to follow strings in the binary instead.
- Read the entire LLM conversation and thinking. Current LLMs are smart but they still sometimes get to a narrow approach. By reading the traces you can often instinctively figure the agent it going into a blind path and give it suggestion as to how it might be approached better. That however skill of your own to have.

## SKILLS

Here are some of my major skills I use frequently.

### Matt Pottock - Grill me

It gives you several questions when your input might be valuable. I usually use it with this prompt:

text

```text
Use skill /grill-me to ask me up to 5 questions
whose answers are crucial for your further progress.
Ask only one question at a time.
```

[mattpocock skills Shell Skills for Real Engineers. Straight from my .agents directory. Open repo Opens in a new tab.](https://github.com/mattpocock/skills)

### Ghidra / other RE tooling skill

It helps a ton to have a skill where your specific reverse engineering tooling is described. Ie to use radare2 or kuna. Or description of how to use the MCP Ghidra bridge and for what. Without such a skill, agents often give up too quickly getting lost, not knowing how to do the actual disassembly / decompilation.

[ioma8 ghidra-decompile-skill Java Agent skill + 22 Ghidra scripts for fast targeted decompilation, and the fix for GhidraMCP extension/Ghidra version mismatches. Open repo Opens in a new tab.](https://github.com/ioma8/ghidra-decompile-skill)

### Hypothesis debugging

That is also my own skill. Its related to how I wrote earlier how important it is to make the agent document everything into MD files. This skill forces agent who is getting stuck to follow this loop:

1. Make a hypothesis of why / how something works. Write it down into CSV table of hypotheses, saved on disc.
2. Design experiment (probe, logging, ...) which will definitively prove or disprove this hypothesis.
3. Record the result.
4. Repeat.

This is variation of scientific method and it significantly helps agents when they are getting stuck on some problem. Stops it going in circles. By forcing it to do definitive experiments. No endless guessing and trying random things. This one is crucial in my opinion. I made it public too, so other can benefit from it.

[ioma8 hypothesis-debugging Agent skill for hypothesis-driven debugging — one testable hypothesis at a time, logged and validated in a CSV loop. Open repo Opens in a new tab.](https://github.com/ioma8/hypothesis-debugging)

## Implementation of hacks and probes

For implementing anything I basically go for 2 toolings.

1. Python but with UV tooling. UV tooling allows to make custom `envs` super quickly and portably. Also allows to make single-file, inline dependencies scripts. Python is great for prototyping or quickly making probes or even decompilation helpers.
2. Rust for actual implementation of given hack. Whenever it is possible I am using rust for this. It has many reasons but he two main are:

- it is very elegant language for me personally to work with and
- it provides brutally good feedback loops to the agent when it iterates - the compiler is very strict and points the agent immediately to any possible memory or syntax issues he might be heading to.\
   Of course rust has also its downsides, like slower compile times, but overal its mature language with plethora of build targets (I have implemented in it webs, desktop apps, hacks / trainers but even firmwares for various embedded languages).

## My hacking successes

With the AI assisted reverse engineering I was able to achieve such crazy reverse engineering succeses, in the last 6 months, which would take me years each on its own, if done by hands.

- [Reverse engineered and hacked](https://github.com/ioma8/neo-re) Alphasmart NEO typewriting device
- Completely mapped communication protocols of many devices like EmWave [biofeedback device](https://github.com/ioma8/emwave-utils) or both my Sony Alpha mirrorless cameras. Allowing me to make custom desktop and mobile apps for file transfer, wireless control and more. I even managed to make this way a wireless webcam from my A7III mirrorless camera.
- Hacked into and developed custom firmwares for musical synthesizers:
  - Arturia MiniFreak
  - [Teenage Engineering OP-Z](https://github.com/ioma8/opz-firmware-notes)
  - [Akai MPC Sample](https://github.com/ioma8/mpc-minimod)
- Made [custom firmware](https://github.com/ioma8/cool) for Xteink X4 ebook reader
- [Fixed driver](https://github.com/ioma8/hp-p2055dn-macos-driver) for old HP printer for MacOs
- Cracked tons of license-locked abandonware softwares from my youth
- Developed custom lightweight Evernote client
- And many more

Sometimes its a thin boundary between reverse engineering and software development. Sometimes you just decipher the proprietary save file format of the ugly paid app and then move into the clean software development phase of building your new, totally unique editor, to work with these files. (yes I made custom Photoshop too and its not the ArtCraft suite - I did it about a year before them, just for me).

## Final thoughts

Reverse engineering and hacking into old softwares or devices has always been a hobby for me. I understand assembly, I understand hardware. But it was long and tedious process.

With the cheap accesibility to smart LLM tools in these days, this hobby took dramatic turn for me. I do not miss manually analyzing the assembly instructions. But I love quickly getting the results. Yes my long experience helps me steer the agents the correct way. But that is also fun. And I am learning a ton of things along the way.

I wish everyone would try to do some hacks of his own. For me the hobby is driven by curiosity. And LLMs didn't take that away. They gave me turbo boost to my wonderings and explorations. Let's free our minds!
