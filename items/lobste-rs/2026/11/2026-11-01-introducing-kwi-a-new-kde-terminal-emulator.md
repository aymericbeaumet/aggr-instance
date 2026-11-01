---
title: Introducing Kwi, a new KDE terminal emulator
link: https://blogs.kde.org/2026/11/01/introducing-kwi/
source: lobste-rs
published: 2026-11-01T02:56:29Z
updated: 2026-11-01T02:56:29Z
first_seen: 2026-11-01T20:46:55.937905179Z
authors:
- blogs.kde.org via cesarandreu
labels:
- linux
summary: Comments
content: extracted
html: 2026-11-01-introducing-kwi-a-new-kde-terminal-emulator.html
remote_preview:
  url: https://eikehein.com/kde/kwi_screenshot_2026-11_01.png
  alt: A basic screenshot of Kwi
---

I've started work on a new KDE application. It's called **Kwi** (pronounced *kwee*).

**Kwi** is designed to provide users of KDE Plasma and other free desktops with a modern and fast terminal, built on a robust tech stack and following the latest KDE HIG and UI conventions.

Here's a screenshot:

[![A basic screenshot of Kwi](https://eikehein.com/kde/kwi_screenshot_2026-11_01.png)](https://eikehein.com/kde/kwi_screenshot_2026-11_01.png)

In some sense, the product vision for **Kwi** is to be kind of unexciting: Just what you want and expect out of a KDE Plasma terminal. Of course, given the powerful nature users generally expect from KDE software, this does not make the project lacking in ambition. :-)

To achieve things, **Kwi** builds on **libghostty-vt** from the excellent [Ghostty](https://ghostty.org/) project and familiar, mature KDE Frameworks such as [KPty](https://api.kde.org/kpty-index.html). It has a very speedy GPU-accelerated (Vulkan or OpenGL) renderer powered by the Qt Scene Graph. Its user interface is built with the latest and greatest by [Kirigami](https://develop.kde.org/frameworks/kirigami/), making it blend in very well among more recent KDE applications and the Plasma desktop.

In keeping with the theme, **Kwi's** icon/logo is a cute little terminal ghost. This is also how it came to bear its name: *kwisin*, or more typically *gwisin*, is a romanized form of *귀신* - Korean for ghost. It's pronounced a little bit like *kwee-shin*.

![The Kwi logo and application icon](https://eikehein.com/kde/kwi_logo_2026-11_01.png)

### Things Kwi has to offer

Compared to the venerable and mature **Konsole**, Kwi is young and new. At the moment it offers a more streamlined feature set than Konsole, carefully realized using KDE's latest HIG patterns. The way forward will be to build on this slowly and carefully, using everything we've learned as a community about running mature design processes over the last decade or so (hello, VDG!). It's a chance to canvass the scene, take stock of how other terminals implement more recent innovations in this space, and compete with a KDE spin on things.

Konsole is very intertwined with the older of the two user interface tech stacks in Qt, *QWidget*. QWidget typically renders using the CPU, and so does Konsole. Newer KDE software (including Plasma itself) typically uses [Qt Quick](https://doc.qt.io/qt-6/qtquick-index.html) and benefits from GPU acceleration via Vulkan or OpenGL instead. This is what Kwi does as well. For certain terminal applications the performance gain (3x-35x faster than Konsole depending on what's being benchmarked) can make an actual big difference in the user experience, but generally it should also help reduce battery usage.

Kwi is built on [libghostty-vt](https://libghostty.tip.ghostty.org/index.html), a project that describes itself as an ecosystem play - provide a headless lib that takes care of all the gnarly terminal parsing and state, enable downstream projects and become sort of a rising tide that lifts all boats. It's starting to really deliver on this promise lately, outpacing other terminal implementations on support for modern TUI features and not hoarding all this good only for itself. While the library will only make an ABI/API stable v1.0 promise at some point in the next 4-6 months, it's already slowly being adopted by big and popular projects, including Neovim, Ghostel and many more. Here's a [list](https://github.com/Uzaaft/awesome-libghostty).

Kwi marries the library to the KDE/Qt tech stack, which allows the project to focus mostly on desktop integration and rendering performance, two areas where KDE's software stack is generally second to none. The combination of the two offers some improvements over Konsole already, for example:

- General
  - UI lift-up to the latest KDE HIG
  - Automatic sync to the system color scheme
  - 3x–35x faster overall performance, depending on the benchmark
    - E.g. 870 fps in [DOOM-fire-zig](https://github.com/const-void/DOOM-fire-zig) on a test system, vs. 23 fps in Konsole
- Rendering
  - GPU-accelerated rendering via Vulkan or OpenGL
  - Better support for using the KDE Plasma system color scheme, including live changes
  - Improved support for correct handling of Wayland fractional scaling
  - Improved terminal text rendering
    - Improved rendering of text decorations (e.g. z-sorting for complex underline styles)
    - Improved support for ligatures, especially when they interact with coloration and selection
    - Support for mode-2027 grapheme widths, Nerd Font icon scaling, etc.
    - Improved support for Powerline and geometry symbols
    - Support for Unicode placeholder images
- TUI app features, desktop integrations for terminal features, etc.
  - Improved accessibility in the terminal
    - Much improved text extraction for screen readers
    - Better mouse/hitscan/query support for screen readers
    - Better support for accessing actions from accessibility tools
  - Support for CSI 996/mode 2031 color scheme queries and updates
  - Improved, more complete support for modern TUI protocols
    - More complete, significantly faster support for the Kitty image protocol
      - Support for parent-relative image placement
    - Support for the Kitty animation protocol
    - Support for the Kitty mouse cursor shape protocol
    - Support for the Kitty/OSC 5522 and OSC 1337 clipboard protocols
    - More complete support for the OSC 9;4 progress protocol, including error and indeterminate states, etc.
    - Improved support for the Kitty keyboard protocol
- Architecture
  - Support for embedding into Qt Quick/Kirigami apps
  - More fine-grained threading architecture, also improving performance
  - Additional optimizations for running efficiently when containerized (e.g. as a Flatpak)

And many more, deeper down in the weeds.

### Why make this, continued - a little bit about architecture

Why make this? First of all, to deliver the marked advantages detailed above! But there are also a few deeper motivations at play.

There's a gap we currently have in our developer offerings: Konsole, being strongly tied into the older QWidget tech stack, offers an embeddable component used in various QWidget-based KDE apps, for example Dolphin, Kate and Yakuake (the Kwi project offers a drop-in replacement solution for these apps). It doesn't offer anything similar to KDE apps written in Qt Quick/Kirigami, however, nor can it easily. This is a (minor) factor increasing the friction to move these KDE apps to the new tech stack. It also leaves new Qt Quick/Kirigami apps without a well-realized KDE terminal component to use.

Kwi installs a public Qt Quick/QML module that can be imported and used by other KDE apps, closing this gap.

An aside to this is an architectural improvement over Konsole: Anyone who's ever made an app using the Konsole KPart component (I made and maintained the KDE 4 version of [Yakuake](https://apps.kde.org/yakuake/) for many years) knows that the Konsole main application famously isn't a consumer of its own KPart. This leads to behavioral differences and some odd limitations between Konsole and alternative UIs like Yakuake. It's also a big reason why Konsole has very complex and sometimes confusing settings UI, with options placed seemingly at random in either a primary *Settings* dialog or within an *Edit Current Profile* dialog - it's because only the latter can be opened from within apps hosting the KPart. This is addressed in Kwi as well: The Kwi application is built around the same public QML module it offers for use by others. The settings accessible from the KPart are the same as from the application.

The improvements list in the earlier section also documents a pattern: In principle, Konsole is well-maintained and has also kept pace with modern terminal features. Often, however, the implementations of newer features are neither complete nor thorough, due to the high amount of effort involved in making changes to the old codebase and the highly terminal-specific expertise required. For example, Konsole does have support for Kitty images, but in practice it's just not fast or complete enough to be very useful, usually resulting in odd placement or low-res image display. The Kwi/libghostty split could be just the right one to pool and access the right resources where they are available, and increase velocity for the terminal ecosystem overall.

### Future Outlook

There are a few more ideas further away that involve Kwi as a technology in some fashion.

Linux distributions are currently moving away from the in-kernel [virtual terminal](https://en.wikipedia.org/wiki/Virtual_console) to better userspace solutions, generally [Kmscon](https://en.wikipedia.org/wiki/Kmscon). This is overall a good thing, but from a KDE perspective, still leaves some potential for further seamless product integration on the table.

Kwi will allow us to experiment with building a Kmscon alternative built on the KDE tech stack, offering for the first time a VT with full accessibility and IME support and behavioral parity for the complete Linux user interface. Only time will tell if this works out, though, as it has been in no way attempted just yet!

Another challenge the project will have to work on is a more seamless transition path for Konsole users. Kwi already supports Konsole color scheme files. An open problem, though, is that QWidget-based apps embedding the Konsole KPart need a one-line code change to request the Kwi KPart instead. There an elegant solution needs to be found to express a user preference, ideally somewhere central.

### Current Status & How To Try Kwi

Kwi is well underway and has seen limited testing by a small circle of friends, but it's also *so very* hot off the presses there's not even a tagged v0.1.0 yet. As such, please greatly temper your expectations and expect obvious and immediate bugs :-). It is in no way a match for the 29 years of continuous development work on Konsole, used by millions every day.

Please also don't mistake Kwi as some kind of announcement of the future KDE direction. For now, Kwi is an experiment and a proposal, only promising to develop an application you may choose to, and will hopefully like to, use. It's an *option*. The rest is a conversation to be had, and only time will tell.

That said, I greatly appreciate your help and feedback. If you find a bug, you can file it on [KDE's Bugzilla](https://bugs.kde.org/), against the *kwi* application.

If you would like to take Kwi for a spin today, [check out its code repository on KDE Invent](https://invent.kde.org/utilities/kwi).

Building Kwi can be difficult today, as it has very fresh dependencies that are likely still on their way into your distro/OS of choice at the moment. Thankfully, the KDE Invent CI/CD is also producing Flatpak builds and keeps a Flatpak repo of nightly builds you can install. Both are linked from the repo README. (Be aware, however, that also the Flatpak runtime can provide too-old versions of things for a while still, in particular affecting Union theming at the moment. Update: Nate has already [ninja-fixed](https://invent.kde.org/utilities/kwi/-/merge_requests/1) this, many thanks!)

On to lots of work ahead!
