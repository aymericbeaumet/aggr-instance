---
title: Microsoft killed FoxPro in 2007. Anyway, here's FoxPro revived
link: https://foxscript.org/
source: hnrss-org-frontpage
published: 2026-09-22T21:00:30Z
updated: 2026-09-22T21:00:30Z
first_seen: 2026-09-23T00:20:14.260676283Z
authors:
- boredjohnny
summary: 'Visual FoxPro stopped at version 9 in 2007. A surprising amount of it is still running, in 32 bits, because rewriting a 20-year-old business app is how you lose the business. A customer wanted to keep milking their app for the foreseeable future, so here it is: the same language on a new runtime (Rust, compiled to wasm, checked against the real vfp9.exe), tables no longer stopped at 2 GB, the old 32-bit .fll add-ins still loading, and lambdas, JSON and an HTTP server bolted on for good measure. Reports are not done and the builds are unsigned. MIT because why not? Comments URL: https://news.ycombinator.com/item?id=49808023 Points: 136 # Comments: 106'
content: extracted
html: 2026-09-22-microsoft-killed-foxpro-in-2007-anyway-here-s-foxpro-revived.html
preview:
  file: 2026-09-22-microsoft-killed-foxpro-in-2007-anyway-here-s-foxpro-revived.preview-42464013980c.webp
  width: 256
  height: 165
  alt: 'Visual FoxPro''s Solution sample running on the Screen tab: its samples launcher form with an outline of ActiveX, Controls, Databases and Forms samples, beside a project explorer listing the project''s forms, menus, programs, class libraries, databases, free tables and reports; the status bar says Wai'
  color: '#f9f9f9'
images:
- source: https://foxscript.org/screenshots/solution.png
  original:
    file: 2026-09-22-microsoft-killed-foxpro-in-2007-anyway-here-s-foxpro-revived.image-158977e1ed1e.png
    width: 2758
    height: 1778
  variants:
  - file: 2026-09-22-microsoft-killed-foxpro-in-2007-anyway-here-s-foxpro-revived.image-e0e4ee83a84b.webp
    width: 320
    height: 206
  - file: 2026-09-22-microsoft-killed-foxpro-in-2007-anyway-here-s-foxpro-revived.image-6c25749cac7f.webp
    width: 640
    height: 413
  - file: 2026-09-22-microsoft-killed-foxpro-in-2007-anyway-here-s-foxpro-revived.image-9626b434eb2c.webp
    width: 960
    height: 619
  - file: 2026-09-22-microsoft-killed-foxpro-in-2007-anyway-here-s-foxpro-revived.image-c5a787a997a6.webp
    width: 1280
    height: 825
  - file: 2026-09-22-microsoft-killed-foxpro-in-2007-anyway-here-s-foxpro-revived.image-c9c87484bdc8.webp
    width: 1600
    height: 1031
  - file: 2026-09-22-microsoft-killed-foxpro-in-2007-anyway-here-s-foxpro-revived.image-90c34c0cf58e.webp
    width: 2758
    height: 1778
  color: '#fbfbfb'
- source: https://foxscript.org/screenshots/editor.jpg
  original:
    file: 2026-09-22-microsoft-killed-foxpro-in-2007-anyway-here-s-foxpro-revived.image-3c56071224b3.jpg
    width: 1600
    height: 1026
  color: '#fcfcfc'
- source: https://foxscript.org/screenshots/debugger.png
  original:
    file: 2026-09-22-microsoft-killed-foxpro-in-2007-anyway-here-s-foxpro-revived.image-645d4721fc31.png
    width: 2756
    height: 1776
  variants:
  - file: 2026-09-22-microsoft-killed-foxpro-in-2007-anyway-here-s-foxpro-revived.image-54b64804ce10.webp
    width: 320
    height: 206
  - file: 2026-09-22-microsoft-killed-foxpro-in-2007-anyway-here-s-foxpro-revived.image-b6a5e33c65a4.webp
    width: 640
    height: 412
  - file: 2026-09-22-microsoft-killed-foxpro-in-2007-anyway-here-s-foxpro-revived.image-e7e2861ca462.webp
    width: 960
    height: 619
  - file: 2026-09-22-microsoft-killed-foxpro-in-2007-anyway-here-s-foxpro-revived.image-1010119cb50c.webp
    width: 1280
    height: 825
  - file: 2026-09-22-microsoft-killed-foxpro-in-2007-anyway-here-s-foxpro-revived.image-0d2250377db4.webp
    width: 1600
    height: 1031
  - file: 2026-09-22-microsoft-killed-foxpro-in-2007-anyway-here-s-foxpro-revived.image-dd13886c55a9.webp
    width: 2756
    height: 1776
  color: '#fcfcfc'
- source: https://foxscript.org/screenshots/browser.jpg
  original:
    file: 2026-09-22-microsoft-killed-foxpro-in-2007-anyway-here-s-foxpro-revived.image-4cf6d15e36f7.jpg
    width: 1600
    height: 1036
  color: '#fcfcfc'
- source: https://foxscript.org/screenshots/http-server.jpg
  original:
    file: 2026-09-22-microsoft-killed-foxpro-in-2007-anyway-here-s-foxpro-revived.image-61068eec19cf.jpg
    width: 1600
    height: 947
  color: '#fdfdfd'
---

## Your FoxPro applications, at home on a modern machine.

FoxDev Studio opens the projects, forms and tables you already have and runs them the way you remember: **no rewrite, no conversion, no export step**.

## It picks up where you left off

Point it at a folder you have not opened in years, and what is there is what you get.

### Your project, as it is

Projects, forms, class libraries, menus and reports open straight from the files you already have. Nothing is migrated first, and nothing is left behind.

### The designers you know

Forms, classes, menus and reports are edited where you expect them to be, alongside the project manager, the Command Window and a debugger that stops on your line.

### Your data stays your data

Tables, indexes, memos and databases are read and written in place. What sits on disk afterwards is the same kind of file it was before.

### Whatever it talked to, it still talks to

The system calls, the automation objects and the old add-in libraries your application leans on keep working, so the parts nobody wants to touch stay untouched.

## What it looks like

The IDE, on Visual FoxPro's own sample projects. Every picture is a real session.

[![Visual FoxPro's Solution sample running on the Screen tab: its samples launcher form with an outline of ActiveX, Controls, Databases and Forms samples, beside a project explorer listing the project's forms, menus, programs, class libraries, databases, free tables and reports; the status bar says Waiting for events.](https://foxscript.org/screenshots/solution.png)Visual FoxPro's own Solution sample, imported and running: its launcher form on the Screen tab, the project's 123 forms, 7 menus and 11 class libraries in the explorer, and the runtime waiting for events.](https://foxscript.org/screenshots/solution.png)\
[![The program editor with a FormsUI sample open, the linter marking a syntax error inline on a FOR line, and the Output and Command windows below.](https://foxscript.org/screenshots/editor.jpg)The editor lints through the runtime's own compiler, so what it underlines is what would fail.](https://foxscript.org/screenshots/editor.jpg)\
[![The debugger stopped on line 26 of the Solution sample's main program, one step past a breakpoint on line 25: the current line highlighted, and below it the Output window, the call stack, the locals, a watch box and the breakpoints list.](https://foxscript.org/screenshots/debugger.png)Stopped one step past a breakpoint in the Solution sample's main program: call stack, locals, watches, and the line it is on.](https://foxscript.org/screenshots/debugger.png)\
[![The table browser showing the 17 records of a database container, with its memo fields as links, beside a project explorer listing forms, menus, programs, class libraries and databases.](https://foxscript.org/screenshots/browser.jpg)A database container in the table browser: 8 fields, 17 records, memos a click away.](https://foxscript.org/screenshots/browser.jpg)\
[![A FoxScript program registering HTTP routes with lambdas, calling a 32-bit encryption library from inside one, and the Output window reporting that the server is listening on port 8080.](https://foxscript.org/screenshots/http-server.jpg)FoxScript: lambdas answering HTTP routes, a 32-bit library called from inside one, listening on 8080.](https://foxscript.org/screenshots/http-server.jpg)

## Built to match, not to approximate

Small differences are what break an old application: a number printed a column too wide, an event arriving a moment late, an error with the wrong number on it. So behaviour here is **settled by asking Visual FoxPro itself** and matching its answer, rather than by reading a reference page and hoping.

What comes out of that is a runtime written from scratch: quick to start, self-contained, and straightforward about the corners it has not reached yet.

**1,722** elements of the Visual FoxPro 9 language reference known to the runtime

**1,534** of them exercised by a test that compares the answer with the product's

**3** names it has not met yet; everything else runs, is ignored on purpose, or is refused by name

## What Visual FoxPro 10 would have been

Visual FoxPro stopped at version 9, and at 32 bits. This is the same language, rebuilt on a foundation that has not been frozen since 2007. Four pieces are what make that possible.

**Where a call goes** / and where the 32-bit world stops

64-bit, end to end

### The ceiling that came with 32 bits is gone

Visual FoxPro is a 32-bit program, and that decides more than it appears to. It is why a table stops at two gigabytes, why a memo file stops at two gigabytes, and why a big report runs out of memory on a machine with plenty to spare. The limits are signed 32-bit numbers buried in the file handling, not a licensing decision anybody made.

FoxDev Studio is **64-bit throughout**. Every file offset is 64-bit and a table is never read into memory at all, so the same `.dbf` that used to stop dead carries on into the **hundreds of gigabytes**. One thing to know before you lean on it: a table grown past two gigabytes will not open in Visual FoxPro again. If you still work in both, that is a one-way door.

**The offset** / and what it reaches

The virtual machine

### A compiler, and a machine built to run what it makes

Visual FoxPro compiled your program to p-code and shipped a runtime to execute it. This is the same arrangement, made again: a compiler and a bytecode interpreter written in Rust and compiled to **WebAssembly**, so one machine runs your code wherever the application runs. The editor checks what you type through that very compiler, so what it underlines and what the runtime refuses cannot drift apart.

A running program is a fiber. When it needs something from the world outside (a message box, a modal form, the next record) it does not call out and block; it yields, the work is done while the machine is off the stack, and the answer is handed back. That is why `MESSAGEBOX()` stops your program without freezing the window behind it, why `READ EVENTS` waits without spinning, and why`SetFocus` can fire `GotFocus`, and `Init` can run while a form is still being built, in the order FoxPro always did it.

[How the machine works](https://foxscript.org/docs/vm) and [what it runs](https://foxscript.org/docs/bytecode).

The screen

### The form you see is the object tree, not a picture of it

A running form is a live tree of objects with the properties you would expect, and the interface is drawn straight from that tree by React. Each object watches only itself, so `THISFORM.lblGreeting.Caption = cMsg` repaints one label rather than the whole form. On a dense screen that is the difference between instant and sluggish.

The same tree is what the designer edits, one step earlier. There is no second model kept in step with the first, which is the usual place a form and its designer start telling different stories.

The 32-bit bridge

### Your old libraries still load, though nothing here is 32-bit

An `.fll` is a 32-bit image, and every process in a 64-bit application is 64-bit, so nothing inside the application itself could ever open one. Rather than tell you it is impossible, `SET LIBRARY TO` starts a **small 32-bit process whose only job is to hold your library**, and the runtime talks to it. The calls are synchronous, because a program may call into a library halfway through an expression, and an answer that arrived later would not be an answer. It is measured against real libraries: the encryption library, FoxTools, and libraries built from Microsoft's own API samples.

Nothing 64-bit needs the bridge: `DECLARE ... DLL` reaches a modern library in the same process, and automation objects are reached the way they always were. The old road stays open; it just is not the only one any more.

## FoxScript: the same language, with more room

Everything you have written still means what it always meant. FoxScript only adds on top: a block you can hand to something else to run later, and a way to answer a web request from the code that already knows your business.

**server.prg** / FoxScript

Every line of that runs on the same runtime your forms do. The queries, the cursor and the library call are ordinary FoxPro; the lambda and the server are what FoxScript adds: no second language, no service to stand up beside it.[The keywords](https://foxscript.org/docs/foxscript) and [the HTTP API](https://foxscript.org/docs/http-api)are each written down in full.

## Download

The nightly is rebuilt from every push to main and published as a pre-release on GitHub. Unsigned, so the first launch asks you to confirm.

[**Windows** The installer (`.exe`) and the runtime (`.zip`), x64.](https://github.com/FoxDevCommunity/FoxDevStudio/releases/tag/nightly)\
[**Ubuntu and other Linux** An AppImage and a `.deb`, x64.](https://github.com/FoxDevCommunity/FoxDevStudio/releases/tag/nightly)\
[**Source, on GitHub** FoxDevCommunity/FoxDevStudio: the IDE, the VM, this site, and every release.](https://github.com/FoxDevCommunity/FoxDevStudio)

## Read the documentation

Each part of the product is written down, including the parts that are not there yet.

[**Overview** What FoxDev Studio is, and where each part of it is described.](https://foxscript.org/docs)\
[**Getting started** Install the IDE, open a project, run a form, ship an executable.](https://foxscript.org/docs/getting-started)\
[**Required tooling** What you need to run the IDE, and what you need to build it from source.](https://foxscript.org/docs/tooling)\
[**The virtual machine** Fibers, host requests, and why the VM never blocks.](https://foxscript.org/docs/vm)\
[**Bytecode** The module format, how a program compiles, and what a frame holds.](https://foxscript.org/docs/bytecode)\
[**Instruction reference** Every instruction with its operands and stack effect, generated from the VM source.](https://foxscript.org/docs/instructions)\
[**The new keywords** LAMBDA, ENDLAMBDA, the FoxScript namespace and two new value types.](https://foxscript.org/docs/foxscript)\
[**The HTTP API** FoxScript.Http, the Node side of it, and why it is shaped this way.](https://foxscript.org/docs/http-api)

## What is coming

Work already mapped out, roughly in the order it is being built.

**The last of the language** A short list of corners is still open, each one checked against the original as it closes.

**Reports** The report designer and the report engine, band by band, measured the same way the rest was.

**Tables past two gigabytes, as a format of their own** The 64-bit offsets are there today; a container that is honest about the DBF header's limits comes next.

**An installer for what you ship** Your application handed to the people who use it as a single thing they can run, with nothing to set up first.
