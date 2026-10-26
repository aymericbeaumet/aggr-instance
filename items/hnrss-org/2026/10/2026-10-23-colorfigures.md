---
title: Colorfigures
link: https://april-zhu.com/colorfigures
source: hnrss-org
published: 2026-10-23T14:22:23Z
updated: 2026-10-23T14:22:23Z
first_seen: 2026-10-26T19:53:35.807667070Z
authors:
- the-mitr
content: extracted
html: 2026-10-23-colorfigures.html
remote_preview:
  url: https://april-zhu.com/colorfigures/og-2.jpg
---

Describe an everyday object. AI draws it hand-inked and isometric, with buttons that actually work. Turn it all the way round and see inside.

`mkdir -p ~/.claude/skills && curl -fsSL https://april-zhu.com/colorfigures/colorfigures.tar.gz | tar -xz -C ~/.claude/skills`

Paste in Terminal for Claude Code. Using Claude.ai or Windows? [Other ways to install](https://april-zhu.com/colorfigures#install).

## Turn it round. See inside.

Every figure turns all the way round: drag it. See-through shows the real machine at work inside, and Labels name each part, lighting up while it runs.

[Try all 26 in the demo →](https://april-zhu.com/colorfigures-demo)

## One sentence in, a working object out

Each of these came from the prompt above it, in a fresh install of the skill. No code was written by hand.

Youan electric kettle that boils, then clicks itself off when done

→

[Kettle *Open ↗*](https://april-zhu.com/colorfigures/examples/kettle.html)

## More made with the skill

Hover to see one move. Open any of them and press things.

[See all figures in the demo →](https://april-zhu.com/colorfigures-demo)

## How to install

Pick your AI. Three steps, about a minute.

1. **1**

   ### Install

   Open Terminal (on a Mac: ⌘ Space, type Terminal), paste this and press Return.

   `mkdir -p ~/.claude/skills && curl -fsSL https://april-zhu.com/colorfigures/colorfigures.tar.gz | tar -xz -C ~/.claude/skills`

2. **2**

   ### Restart Claude Code

   Quit and reopen it so it picks up the new skill.

3. **3**

   ### Ask for a figure

   Describe an object in plain words, or start with /colorfigures. It saves an .html file; open it in your browser and press things.

## Try these prompts

Copy one, paste it into your AI, open the file it makes.

a gumball machine where I can put in a coin and turn the crank

a record player: put on a record, drop the needle and it plays

a kettle that boils and clicks off by itself — show how it knows when to stop

how does a toaster know when to pop? make it a Colorfigure

a bike pump filling a tyre, with a see-through valve

a parcel locker wall: type a code and the right door pops open

Need custom figures for your product or a commercial project?

## Questions

What is a skill?

A small folder of instructions and templates that teaches an AI how to do one job well. It isn't an app and doesn't run by itself; the AI reads it when you ask for something it covers.

The AI isn't using it

Restart the app (Claude Code, Codex or Gemini CLI), then mention it by name: "use the colorfigures skill to draw a gumball machine". To check it's installed, run `ls ~/.claude/skills/colorfigures` (or `~/.codex/skills`, `~/.gemini/skills`); you should see SKILL.md, README.md, LICENSE, assets and scripts.

What kinds of objects work best?

Everyday machines and devices with parts you can press: appliances, vending and arcade machines, toys, signs, kiosks. Every figure turns all the way round, and machines get a See-through switch that shows the real working parts inside, labelled and moving with the buttons. Soft, organic things (people, animals, plants) don't suit the style.

How long does a figure take, and how many tokens?

About 15 to 30 minutes and roughly 200,000 to 400,000 tokens per figure in Claude Code. Most of that goes on modelling the object from every side and building its see-through from how the real machine works; then the AI checks it from several angles, in see-through and on a phone, and fixes what it finds. On a fresh test run a parking meter with a see-through of about 20 labelled parts took around 15 minutes and 230,000 tokens; a dense machine or a scene with several objects takes longer. Changes you ask for afterwards in the same chat are much quicker. On a Claude plan this counts toward your usage; on the API it's billed per token.

Can I change the colours?

Yes. Every figure has body-colour swatches (tomato, mustard, mint, cobalt, blush) and a hand-drawn wobble slider. You can also ask the AI for a particular colour.

Is it safe to install?

It's eight small plain-text files (about 220 KB): the instructions, the drawing engine, three example figures, a build script, a readme and a licence. Installing only copies them; nothing runs, and there are no packages or dependencies. Each figure the AI makes is a single HTML file that runs in your browser like any web page, with no tracking, accounts or outside requests. Want to look first? Download the .zip and open SKILL.md in any text editor.

Can I use it for commercial work?

Yes. The skill is MIT licensed, and the figures you make with it are yours to use, including in paid and client work. For custom figures made for your product, [email wjzhu.423@gmail.com](mailto:wjzhu.423@gmail.com?subject=Custom%20Colorfigures).

Do I need to code?

No. Describe the object in plain words. If you want changes, say so in the same chat: "make the door open further", "add a coin return".
