---
title: 'WorldTUI: I trained a 360M model to pretend to be a terminal'
link: https://simedw.com/2026/10/21/world-model-terminal/
source: hnrss-org
published: 2026-10-22T02:18:41Z
updated: 2026-10-22T02:18:41Z
first_seen: 2026-10-22T17:33:41.202070908Z
authors:
- alphax314
content: extracted
html: 2026-10-22-worldtui-i-trained-a-360m-model-to-pretend-to-be-a-terminal.html
remote_preview:
  url: https://simedw.com/2026/10/21/world-model-terminal/images/worldtui-cowsay.jpg
  alt: 'WorldTUI: I trained a 360M model to pretend to be a terminal'
---

TL;DR: I fine-tuned a 360M-parameter language model to act as a terminal. There's no shell or filesystem behind it: every keypress goes in, and the model predicts how the screen changes. It can create files, run vim and tmux, play snake, and usually remembers what you wrote a few commands later. The biggest wins were teaching it to repair its own mistakes and decoding screen rows in parallel. [Try it in your browser (208 MB)](https://simedw.com/projects/worldtui) and type `pythoon` to see that it's not a real shell.

The demo is designed for a desktop browser with WebGPU and a physical keyboard. Chrome or Edge is the best place to try it. It should work on a modern phone, but painfully slowly.

When I first saw [Genie 3](https://deepmind.google/models/genie/) last year, I started imagining a future where applications aren't written in code but learned. Instead of a game engine rendering the scene, a model predicts the next most likely frame from the user's inputs.

Naturally I wanted to train my own world model. But I don't have a massive GPU cluster or a pile of video data, so I scaled down my ambition: instead of a game world, a terminal.

A terminal turned out to be a good fit. It's just a grid of characters rather than pixels, and I can generate as much perfectly labelled training data as I want by running real shells. A pretrained language model also already has a pretty good idea of what `ls` or `cat` should print.

What I really wanted to know was whether it could maintain a consistent world, or just generate convincing terminal output.

The final model is based on Qwen2.5-Coder-0.5B, fine-tuned on terminal sessions recorded from real programs running inside Docker. [Jump to example videos](https://simedw.com/2026/10/21/world-model-terminal/#what-it-can-do)

## How it sees a terminal

Standard LLMs take a prompt and then append tokens to the end, one at a time. That doesn't map well to a terminal. A single keypress can do something on its own, like `esc` in vim, and output doesn't always go to the bottom: `clear` wipes the screen and sends the cursor back to the top. So instead of predicting a stream of text, I need to predict changes to the screen.

I treat the terminal as a grid of characters, one per cell.[^1] The model doesn't see the grid directly, but a sequence of tokens describing its contents and the changes made to it.

### A session

Every session starts with a snapshot: the screen size, its contents and where the cursor is.

The size is part of the input, not fixed at 32×96. In the demo, you can set the rows and columns when creating a custom starting screen.

```
BOS SCREEN_START 32 96
ROW_0 0 'simedw@laptop:~$ '
CURSOR 0 17 1
SCREEN_END
```

This also lets me start partway through an application. Give it a bash prompt, vim buffer or a game of snake, and the model tries to continue from there. (It does better from bash.)

After that, each keypress goes in as the raw bytes a real terminal would send. Letters are themselves, Enter is `\r`, Escape is `0x1b` and ← is `ESC [ D` and so on.

```
INPUT_START 'l' INPUT_END
```

The model answers with which rows to rewrite and where the cursor ends up:

```
OUTPUT_START
ROW_0 17 'l'
CURSOR 0 18 1
OUTPUT_END
```

In words: on row 0, starting at column 17, write `l`, then move the cursor to row 0, column 18. The last `1` means the cursor is visible, since full-screen apps sometimes hide it.

Nothing gets cleared first. New characters are simply drawn over the old ones.

I didn't need new tokens for characters. Qwen already has tokens for all 256 byte values, which I reuse for characters, coordinates and other small numbers. So a coordinate like 17 takes one token, not two.

Time is an input too. If nothing is pressed, the model gets a WAIT token with a duration instead of a key, which is how snake keeps moving on its own.

### Copying rows

When you reach the bottom of the terminal window and press enter, every line has to move up by one. I could just ask the model to output every row again in its new place, but that's around 3,000 tokens for a single keypress.

So I added a `COPY` operation instead:

```
ROW_0 COPY 1
ROW_1 COPY 2  
ROW_2 COPY 3
...
```

Each operation copies a row from the previous screen. In this example, the old row 1 becomes row 0, row 2 becomes row 1, and so on.

A scroll now costs three tokens per row instead of roughly 96.

The same trick covers clearing the screen, inserting and deleting lines in vim, and navigating man pages. You can also copy a row and then overwrite part of it.

Altogether, the model uses a tiny vocabulary of 333 tokens: 256 byte tokens, 64 row tokens and a handful of special operations. Decoding is grammar-constrained, so every prediction is a valid screen update.

That tiny vocabulary comes back later, when I squeeze the model into a browser.

## Generating the training data

I initially looked for existing datasets of terminal recordings, including asciinema, but couldn't find one with the precise keystroke-to-screen alignment I needed.

Luckily it wasn't too hard to create my own.

The basic idea was to generate believable terminal sessions by running real commands inside Docker containers, recording every keypress and the screen after each one.

The synthetic recipe was:

1. Randomize usernames, hostnames, prompt styles, terminal sizes.
2. Randomize directories and files based on a fictional persona (developer, sysadmin, etc.).
3. Pick 6–24 weighted activities (modify files, vim, python, tmux, etc.).
4. Track state, so sessions mostly open files that exist.
5. Type like a person: one keypress at a time, with tab completion etc. I set the typo rate to 1.2%, which is generous to me.
6. 35% of sessions start partway through, forcing the model to learn to work from any state.

The terminal size was mostly 32×96, with some 24×80 and 40×120, so the model doesn't overfit to one size.

I kept expanding the dataset as the model became more capable, and finally landed on 336k episodes, which corresponds to roughly 2B tokens. For comparison, the base model saw around 18T tokens during training.

## Training

I used Qwen2.5-Coder-0.5B as the starting point. I needed a small model to get close to realtime performance. Being a coding model, it had also already seen plenty of CLI usage.

I started off by doing straightforward supervised fine-tuning: give a screen history and a keypress, predict the screen delta produced by the real terminal.

Ordinary typing was learned quickly: a letter appears, backspace removes it, the cursor moves.

In longer sessions, though, errors started to compound: a mistake in a filename propagated through the rest of the session.

### Learning from its own mistakes

To address this I took a page from DAgger, where a model learns from the states it visits rather than just the ideal demonstrations.[^2]

For each training example:

1. Pick a point in a recorded session, usually close to the command that produces the output.
2. Let the model predict the next 1-4 events.
3. Compare the resulting screen with the real recorded screen.
4. If the difference is small enough, construct a correction that repairs the model's screen on the next keypress.
5. Continue training on the corrected frame and a few subsequent real events.

The delta takes the model's incorrect screen to the correct next screen. In essence, it learns to "repair" its own mistakes.

There's one important restriction: the model must have enough information to infer the correction. Any words introduced by a correction must have appeared on an earlier real screen or in the user's typing, not just in the model's own predictions.

For example, if the model misspells a filename that appeared earlier, I can teach it to fix that. But if it lists a filename it has never seen, I can't expect it to get it right.

Training on those examples would actually reward hallucinations, which I really don't need more of.

About a third of the sampled sessions produced useful correction examples. I repeated this in three rounds, each time collecting mistakes from the latest model.

Self-correction was one of the more effective training changes. Along with some additional training data, it improved PresetBench (defined below) from 24/47 to 38/47 at int4.

In total, training the model and its later refinements took roughly 75 GPU-hours on an RTX 4090.

### Testing

I use three tests. **ShellBench** has 106 commands across 24 handwritten bash sessions, scored by exact output. **PresetBench** has 47 scripted cases drawn from failures I hit in the demo. Neither test appears in the training data.

The **memory test** has 32 worlds. Each writes two random values to two files, then reads them back in separate sessions. A world passes only if both readbacks are exact, giving 64 readbacks overall. The names and values never appear in training.[^3]

How much does fine-tuning help?

| Model[^4]                     | ShellBench /106 | File readbacks /13 |
| ---------------------------- | --------------- | ------------------ |
| Qwen2.5-Coder-0.5B, prompted | 52              | 2                  |
| Qwen3-0.6B, prompted         | 60              | 3                  |
| WorldTUI                     | 80              | 9                  |

The biggest difference was memory: WorldTUI got 9/13 readbacks right, compared with 2/13 and 3/13 for the prompted models.

It's still far from a reliable filesystem. It handles familiar commands fairly well, but combining them in new ways can quickly break things. For example, after `echo green >> colors`, running `cat colors` often doesn't show the appended line.

## Making it fast

For a while I stayed away from autoregressive models altogether. A full redraw is around 3,000 tokens, and generating those one at a time seemed hopeless for something that's supposed to feel like a terminal.

But they just performed so much better than the alternatives I tried, so I gave in. With one token at a time, typing felt fine at ~38 ms per key, but anything that redraws the whole screen (`vim test.txt`, `man`, `clear`) took up to 12 s on my RTX 4090, exactly as feared. Plenty of time to reconsider whether I needed the manual. And I wanted this to run on my Mac, in near real-time.

### Rows in parallel

What I'd missed was that most rows in a frame don't depend on each other, so there's no reason to generate them one after another. I run one batch element per row. Each gets the same previous history (including KV cache) but generates only its own update.

That changes the decoding cost from the sum of all rows, up to 3,000 steps, to the length of the longest row, roughly 100 steps.

The cursor then becomes its own step at the end that can see the whole set of row updates.

This required a matching attention mask during training, but the speed improvement was worth it. Large-frame p95 latency dropped from 6.7 s to 0.87 s.

Large-frame p95 latency (RTX 4090) ↓

Sequential

6.7 s

Parallel rows

0.87 s

### Rows that can't talk to each other

This might sound too good to be true, and it was. The issue is that since rows are independent, they can't coordinate, which led to `vim` opening on row 3 instead of row 0, or several rows adding food for snake at once.

The solution was speculative decoding.

Normally speculative decoding uses a small model to draft tokens and a larger model to verify them. Thanks to causal attention, several tokens can be verified in a single forward pass.

Here I use the same model for both: the parallel rows produce a draft, which the model then verifies sequentially.

The verifier keeps tokens up to the first disagreement. From there it generates its own correction and re-proposes the remaining draft.

In the browser on my M4 Mac, typing a single character takes about 60ms. A larger update like `man test` takes 2.5s sequentially, or 0.9s with parallel drafting and verification.[^5]

## Making it smaller

I started with Qwen2.5-Coder-0.5B, which has roughly 494M parameters. I really wanted a web demo, so I needed to shrink it. The embedding table for its 151,936-token vocabulary is 136M parameters. I only need my 333 tokens,[^6] so I can drop almost all of it, which takes the model from 494M to about 360M parameters (990MB → 716MB).

I then quantized it to int4, which brought it to 208 MB.

The browser demo uses a custom JavaScript implementation of the Qwen decoder, with compute shaders written in WGSL and run through WebGPU. The shaders unpack the 4-bit weights during inference. Activations and the attention cache use 32-bit floats. All inference runs locally in the tab.

### Distilling

Distilling it into fewer layers taught me something about which layers memory depends on. The teacher was an earlier checkpoint, which scored 78/106 on ShellBench. Removing just layer 7 without retraining barely changed next-token accuracy: 99.3% fell to 98.9%. But exact memory readbacks fell from 64/64 to 0/64. These tests fed in the true history, so the failure wasn't caused by errors compounding on the model's own output.

My first distilled student kept every even-numbered layer, but used the final layer, 23, instead of 22. It still wrote plausible terminal output, but returned the right value in only 31/64 memory readbacks. It always printed one of the two values it had written, but picked the wrong one about half the time. That's chance, and it hadn't recovered after 480M training tokens.

That student had dropped layers 1, 7, 15 and 22. A 16-layer student that kept those four passed all 32 memory test worlds, with 64/64 exact readbacks. It still passed all 32 worlds at 4-bit, at about 138MB. There was a cost elsewhere, though: it got 65 ShellBench commands right against the teacher's 78. So a student can look fine and have quietly lost its memory.

In the end I gave up on distilling it.

## What it can do

### Memory

This was the part I was most interested in. Generating a convincing directory listing is easy. What I wanted was to create a file, edit it, wander off, and come back to find it unchanged.

The first time I started tmux, opened a file in vim, changed it, saved it and then in a different tmux window read the updated contents with `cat`, I felt like Dr. Frankenstein: it's alive!

### Other applications

It was very important to me that this should work beyond the bash prompt, so I trained on vim, python, man pages, snake and even tmux.

In vim, it handles the mode switch on `esc` and lets me save, quit or go back into insert mode.

Probably the most compute-intensive snake in the world.

What really blew my mind was tmux: I can create multiple panes, do various things in each and jump between them. It's not perfect, but it's a small model.

It even manages environment variables and variable expansion.

### Python

Since the base model is a coding model, I was curious how it would handle the Python REPL. It makes a decent attempt: small additions are usually right, and list comprehensions return a list, though often with the wrong contents. It's also slightly forgiving of typos: `pythoon` launches Python, but `ython` doesn't.

## What doesn't work

I didn't allow the Docker containers internet access, so tools like lynx are fairly boring. Python can handle very simple operations, but anything harder produces answers that remind me of GPT-3-era hallucinations.

Sometimes, when I create nested directories, it gets lost and all of a sudden I'm in a different path. There's clearly a lot of room to improve consistency.

In my testing, the demo ran about 20× slower in Firefox than in Chrome. It uses WebGPU in both. I haven't managed to establish the cause.

As always with projects like this, I tried to solve it myself first and only then looked at how others had approached it.

Back in 2022, people were already [prompting ChatGPT to act as a Linux terminal](https://www.engraved.blog/building-a-virtual-machine-inside/). More recently, [GameNGen](https://gamengen.github.io/) simulated Doom using diffusion, and [NeuralOS](https://neural-os.com/) tackled a desktop GUI.

WorldTUI is closer to the original ChatGPT terminal trick, except that it's trained specifically on keypresses and screen updates, including full-screen applications like vim and tmux.

## Summary

I had a blast building this. Is it useful? Not really. But for a 360M-parameter model running in a browser tab, it's far better than I expected.

What interests me most is that it manages to maintain a somewhat consistent world without implementing any of the rules underneath.

It's obviously a pretty weak world model, and I'd love to see how far this approach could go with better consistency and a different way of generating screen updates, perhaps using diffusion instead of autoregressive decoding.

One experiment I want to try next is training on thousands of randomly generated applications, each with its own controls and rules. The idea is to see whether the model can learn to predict the behaviour of an application it has never encountered before, given enough contextual clues.

[Try it here](https://simedw.com/projects/worldtui). It's a 208 MB download, runs entirely locally, and works best in Chrome. Try to break it, and let me know what you find.

I'll post the weights soon and add the download link here.

[^1]: It's monochrome: only characters, no colours or text styles. Unicode characters, including tmux's box-drawing borders, are mapped to ASCII before encoding, so those borders don't need multibyte character tokens.

[^2]: Not too dissimilar to the scheduled sampling in my [midi post](https://simedw.com/2026/08/20/midi-autocomplete/).

[^3]: These are short-gap tests, with 0–1 intervening commands. ShellBench also tests persistence across 18 commands.

[^4]: Both prompted models got three worked examples each. They predict whole command outputs rather than individual screen updates, so it's not quite the same task.

[^5]: Speculative decoding gives a much bigger speedup on a 4090 than on my M4. My suspicion is that the bottleneck shifts between compute and memory bandwidth, but I haven't investigated this properly.

[^6]: Padded to 384 to please the GPUs.
