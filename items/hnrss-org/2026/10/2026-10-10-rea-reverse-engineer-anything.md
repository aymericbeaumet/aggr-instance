---
title: REA Reverse – Engineer Anything
link: https://rea.tools/
source: hnrss-org
published: 2026-10-10T00:37:07Z
updated: 2026-10-10T00:37:07Z
first_seen: 2026-10-10T02:41:46.265872821Z
authors:
- modinfo
content: extracted
html: 2026-10-10-rea-reverse-engineer-anything.html
extra:
  comments_url: https://news.ycombinator.com/item?id=50028275
  num_comments: 17
  points: 105
---

Reverse engineering, with your coding agent

Find out how\
software works.
---

Ask your coding agent about an app, a game or a website. REA gives it the tools to inspect the code behind the answer.

[Showcases →](https://rea.tools/showcase/) — investigations and interactive demos.

[Blog →](https://rea.tools/blog/) — methods and reconstruction notes.

## Set up REA

Copy this into your coding agent:

Install REA and connect it to this coding agent using npx rea-agents@latest setup. Show me the setup plan for approval, then verify the installation.

## How it works

Reverse engineering means learning how software works by inspecting the program itself. With REA, your agent can follow the code, explain a feature and help you rebuild it.

1. ### Ask about one feature

   Give your agent a program or page and a question.

2. ### Inspect the program

   REA returns code, references and runtime observations.

3. ### Explain it, then try a change

   Your agent uses the findings to explain or rebuild the feature.

See the full Calculator investigation

Why does Calculator give 220 for 200 + 10%?

### Follow the instructions step by step

```
0x180124945: MOV EAX, dword ptr [R13 + 0x18]
0x180124949: CMP EAX, 0x5c
0x18012494c: JZ 0x180124aba
0x180124952: CMP EAX, 0x5b
0x180124955: JZ 0x180124aba
0x18012495b: MOV EDX, 0x64
0x180124960: LEA RCX, [RSP + 0x160]
0x180124968: CALL 0x180122650
0x18012496e: LEA RDX, [R13 + 0x160]
0x180124975: LEA RCX, [RSP + 0x78]
0x18012497a: CALL 0x180109720
0x18012497f: MOV RSI, RAX
0x180124982: MOV qword ptr [RSP + 0x28], RAX
0x180124987: LEA RDX, [RSP + 0x160]
0x18012498f: MOV RCX, RAX
0x180124992: CALL 0x180123190
; … value-copy instructions omitted …
0x180124a14: MOV RDX, RDI
0x180124a17: LEA RCX, [RSP + 0xf8]
0x180124a1f: CALL 0x180109720
0x180124a24: LEA R8, [RSP + 0x30]
0x180124a29: MOV RDX, RAX
0x180124a2c: LEA RCX, [RSP + 0xb8]
0x180124a34: CALL 0x180123c10
```

**You decode the instructions, follow the calls and reconstruct the calculation.**

Ask your agent the same question

Use REA to inspect Windows Calculator. Why does 200 + 10% give 220?

Example answer · based on the findings

After +, the % button takes **a percentage of the first number.**

`10% of 200 = 20`**200 + 20 = 220**

REA supplies

The handler’s instructions, decompiled code and calls.

Your agent explains

Which branch applies, what it calculates and how to rebuild it.

Selected assembly from the installed Calculator DLL. The thoughts and reply illustrate the findings. [Continue with the Calculator example ↓](https://rea.tools/#calculator)

Let’s try two examples: change a game’s speed, then rebuild Calculator’s % button.

Example 01 · Chrome’s dinosaur game

## Why does the dinosaur get faster?

The game gets faster as you run. To rebuild it with a speed control, we first need to find out how the speed changes.

### 1\. Ask about the speed

Use REA to inspect this dinosaur game. Why does it get faster? Show the speed rule, then build a small version with an adjustable speed.

### 2\. Find the speed rule

REA lets the agent read the game’s running code. The speed starts at 6 and increases a little each update while it is below 13.

### 3\. Play the result

We rebuilt that rule in the mini-game below and added a speed slider. Press Play, then choose your own speed.

Original rule: start at 6, then increase toward 13.

Move the slider to set your own speed. Space or Jump clears a cactus.

Enable JavaScript to play. You can still read the rule and open the source code below.

[Open the speed lab →](https://rea.tools/examples/dino-lab/)

See the script, checks and how to try the analysis

Inspected target: [wayou’s Chromium-derived browser edition](https://wayou.github.io/t-rex-runner/).

```
if (this.currentSpeed < this.config.MAX_SPEED) {
  this.currentSpeed += this.config.ACCELERATION;
}
```

Start at 6. Add 0.001 each update without a collision, while speed is below 13.

What each part does

1. 01 · REA Read the running game’s script Return its actual code and settings to the agent.
2. 02 · Your agent Rebuild the rule, add a control Use the recovered acceleration in a small game with a speed slider.
3. 03 · You Change the speed and play Try the original acceleration, then choose your own pace.

REA inspected the HTTP browser edition through a local debugging connection and returned the loaded `index.js`, including its source and digest.

```
ACCELERATION: 0.001,
MAX_SPEED: 13,
SPEED: 6
```

We called the original game’s update function in a controlled browser check, with obstacles and automatic scheduling disabled. After 4,000 updates, speed was 10.0; after 10,000, it was 13.0, rounded to one decimal.

The new mini-game keeps that speed rule. Its drawing, jumping and collision code are a small teaching implementation. [Open the lab](https://rea.tools/examples/dino-lab/) to see the new code and run the same speed check.

To inspect the target yourself, follow the [browser connection steps](https://rea.tools/guides/browser/#prepare) using [the dinosaur page](https://wayou.github.io/t-rex-runner/). Give your agent that page URL and your local debugging endpoint, then copy the prompt above.

[Inspected speed code](https://github.com/wayou/t-rex-runner/blob/5455bfa408ec6b707c7300ff194b7390733a766d/index.js#L565-L571) · [Chromium Authors · BSD license](https://rea.tools/assets/licenses/dinosaur.txt)

Example 02 · Windows Calculator

## Rebuild Calculator’s % button.

The % button does different things after + and ×. Let’s find both rules and try them in a small calculator.

### 1\. Ask about the % button

Use REA to inspect Windows Calculator’s % button. Recover the rules after + and ×, then build a small calculator that uses both.

### 2\. Find both rules

REA helps the agent read the code behind the % button. It applies two different rules:

With +

Take 10% of the first number.`10% of 200 = 20 → 200 + 20 = 220`

With ×

Turn 10% into 0.1.`200 × 0.1 = 20`

### 3\. Try the % button

We recreated both rules below. Choose + or ×, press %, then press =.

200 + **20**

10% of 200 = 20. Then 200 + 20 = 220.

Interactive illustration of the inspected percentage rule.

See the real code and how the answer was checked

For this example, `previous = 200` and `current = 10`.

```
if (operation == multiply || operation == divide) {
  percent = current / 100;
} else {
  percent = current * previous / 100;
}
```

From the installed app to a working % button

1. 01 · REA reads calc.exe It launches the Calculator app The code opens `ms-calculator:`. The agent then selects the installed app’s library.
2. 02 · REA reads the library Return the % handler’s code The function checks the operator and uses the constant `100`.
3. 03 · Your agent interprets it Explain and reproduce the rules Crosscheck the branch with Microsoft’s source, then try + and × above.

```
0x180124945: MOV EAX, dword ptr [R13 + 0x18]
0x180124949: CMP EAX, 0x5c
0x18012494c: JZ 0x180124aba
0x180124952: CMP EAX, 0x5b
0x180124955: JZ 0x180124aba
0x18012495b: MOV EDX, 0x64
```

```
#define IDC_MUL 92      // 0x5c
#define IDC_DIV 91      // 0x5b
#define IDC_PERCENT 118 // 0x76

0x64 = 100
```

Inspected with REA 4.1.0: Windows Calculator 11.2508.4.0, x64. The readable summary uses names from Microsoft’s public source to explain the recovered branch.

[Percentage implementation](https://github.com/microsoft/calculator/blob/d125246a4e19842ce1332e6c7839cf0e110027d8/src/CalcManager/CEngine/scifunc.cpp#L98-L111) · [Microsoft’s test cases](https://github.com/microsoft/calculator/blob/d125246a4e19842ce1332e6c7839cf0e110027d8/src/CalculatorUnitTests/CalculatorManagerTest.cpp#L350-L370) · [Native setup guide](https://rea.tools/guides/native/)

## REA case studies.

Explore a feature in a released app or game, with code and checked results.

[Showcases →](https://rea.tools/showcase/)

Android · APK analysis

### How an authenticator generates a login code

Trace Aegis's six-digit login code through its released APK, then move a demo clock to see why it changes every 30 seconds.

[Read the Android case study](https://rea.tools/showcase/aegis/)

Electron · Feature tracing

### How Notion copies text and HTML

Follow Notion's packaged JavaScript to the system clipboard and see how a copy preserves rich block data.

[Read the Notion case study](https://rea.tools/showcase/notion/)

Native · Reconstruction

### DX-Ball's sound-pan calculation

Recover how a ball's position becomes left/right sound in a 1996 Windows game.

[Read the DX-Ball case study](https://rea.tools/showcase/dx-ball/)

Your first investigation

## Try REA on a small app.

Download our Notes example, trace its CSV export, and check one changed input.

[Try the guided example](https://rea.tools/first-investigation/)

## Choose your target.

These guides cover setup and a first investigation for each kind of software.

### Native binaries

Inspect functions, strings, references and call relationships in executables and libraries.

[Native analysis guide](https://rea.tools/guides/native/)

### JavaScript & Electron

Map modules, routes, IPC and native dependencies from an application folder or ASAR archive.

[Application workflows](https://rea.tools/guides/javascript/)

### Browser & runtime activity

Capture selected browser or process activity, then compare the results across runs.

[Browser observation guide](https://rea.tools/guides/browser/)

Try a larger project

Use REA to inspect https://rea.tools/. Clone this website for me.

Yes, this one.

Use REA to reconstruct this game from its executable. Recover the gameplay logic in C and test it against the original.

[See the DX-Ball case study →](https://rea.tools/showcase/dx-ball/)
