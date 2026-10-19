---
title: Plasma 6.8 released (KDE's 30th birthday)
link: https://kde.org/announcements/plasma/6/6.8.0/
source: lobste-rs
published: 2026-10-14T11:29:51Z
updated: 2026-10-14T11:29:51Z
first_seen: 2026-10-15T04:26:51.711869061Z
authors:
- kde.org via joshsharp
labels:
- linux
- release
summary: Comments
content: extracted
html: 2026-10-14-plasma-6-8-released-kde-s-30th-birthday.html
---

## Celebrating thirty years of KDE

Plasma 6.8 is released on a very special day: KDE’s 30th birthday. Today we celebrate three decades of the KDE community providing the world’s greatest free and open-source software.

Plasma 6.8 is no exception, offering enhancements across much of the user experience and beneath the surface.

6.8 is all about launching Plasma into the future: adding changes you’ll notice right away, combined with major but subtle improvements designed to set the stage for decades of service to come.

Dive in and find out what the desktop environment of tomorrow looks like!

## Improvements you can use today

### Better accessibility

Part of our continued effort to bring digital control to everyone, a new “dwell clicker” feature can send a mouse click when the pointer stops moving. Designed especially for users with mobility issues, an external app is no longer required to use your system in this way; now it’s built right in!

### Serving more users worldwide

Plasma 6.8 gains support for the [Bikram Sambat](https://en.wikipedia.org/wiki/Vikram_Samvat) calendar in the Digital Clock, and the [Amharic](https://en.wikipedia.org/wiki/Amharic) language in the on-screen keyboard. These are just two more additions of the dozens incorporated into Plasma over recent versions in our effort to make Plasma a truly universal desktop.

![Bikram Sambat calendar](https://kde.org/announcements/plasma/6/6.8.0/10-bikram-sambat-calendar.webp) Bikram Sambat calendar

![Amharic on-screen keyboard](https://kde.org/announcements/plasma/6/6.8.0/11-amharic-on-screen-keyboard.webp) Amharic on-screen keyboard

### System monitoring

Gain an extreme level of control over your system with Plasma’s System Monitor. In Plasma 6.8, you can assign compute-intensive tasks to only the CPUs of your choosing from directly within the monitor, and keep your machine running smoothly.

![System Monitor app offering the possibility to change the CPU affinity for the plasmashell process](https://kde.org/announcements/plasma/6/6.8.0/20-systemmonitor-affinity.webp)

You can also monitor your VRAM more easily, as it now shows the load as an easy-to-read percentage.

### Remote desktop

We’ve invested a great deal of time into polishing the experience provided by Plasma’s built-in remote desktop server. In Plasma 6.8, you’ll be able to enjoy the following improvements — with more in the pipeline:

- **Much better performance and quality** that prioritizes latency and responsiveness by adapting to slow networks and rapidly-updating screen content.
- **Exclusive mode** optionally locks the server’s screen and provides the client with a virtual screen that exactly matches its window size.
- **Bi-directional clipboard synchronization** makes it easier to exchange content between the server and client.

![Plasma’s Remote Desktop settings and the KRDC application in use](https://kde.org/announcements/plasma/6/6.8.0/23-rdp.webp)

### Screen casting and recording

Spectacle can now optionally include both system audio and microphone input in its screen recordings, making it possible to narrate a recording or accurately record everything that’s going on.

Plasma’s screen sharing portal dialog lets you create a virtual screen to share, instead of an existing display. In Plasma 6.8, you can now specify the resolution of the virtual screen instead of being limited to only 1920 × 1080.

![The screen sharing portal window showing the ability to select a custom resolution for the virtual display](https://kde.org/announcements/plasma/6/6.8.0/25-screencast-portal-resolution.webp)

In addition, the quality of screen recordings and screencasts has been improved.

### Nicer-looking third-party app windows

We’ve added support for window rules that add desktop shadows, outlines, and rounded corners to client-side-decorated windows that can’t provide these visual niceties themselves. Plasma 6.8 ships with one such rule for Steam, making it feel more at home in Plasma.

### Window management

We’ve added new global shortcuts to help power users manage their open windows with the Task Manager widget. By pressing Meta + Ctrl + Page Up or Page Down, you can cycle between the previous and next tasks. By also holding Shift, you can move the active task backwards and forwards.

## Setting the stage for decades to come

### Theming

Four months ago, Plasma 6.7 included an alpha release of the new Union theming system, demonstrating its ability to style Qt Quick applications.

Plasma 6.8 builds on this with a beta release of Union, including not only refinements for Qt Quick theming, but also the ability to style Qt Widgets applications! This includes KDE’s more traditional desktop applications, such as Dolphin, Kate, and Konsole.

![](https://kde.org/announcements/plasma/6/6.8.0/40-dolphin-union.webp)\
Dolphin themed with Union’s Breeze style. Can you tell the difference?

While Union is not quite ready to be used by default, Plasma 6.8 brings it ever closer to being a single source of truth for styling all of KDE’s products, and more. To learn more about the current state of Union, read the blog posts from [Arjen](https://quantumproductions.info/articles/2026-08/union-summer-2026-update) and [Akseli](https://akselmo.dev/posts/union-qtwidgets/).

### Wayland

30 years is an impressive milestone for a community like KDE. But even older is the venerable [X Window System](https://en.wikipedia.org/wiki/X_Window_System) (or “X11”), first released in 1984! After 42 years of faithful service, support for starting X11 sessions in Plasma is being retired, in conjunction with full adoption of the newer [Wayland](https://en.wikipedia.org/wiki/Wayland_%28protocol%29) system.

Compared to X11, Wayland offers better support for modern screens and graphics cards, and is actively developed.

The small number of individual apps that still require X11 will continue to work via the “XWayland” compatibility layer.

KDE applications will continue to support X11 for the foreseeable future.

### Performance, colors, and graphics

As part of the backend improvements included in Plasma 6.8, we’ve made changes to help Plasma load faster and use less memory. Discover’s update notifier now uses less memory, too.

With KWin, NVIDIA GPUs now use triple buffering by default, and all users will have better-looking HDR when using an ICC color profile. We’ve also added support for the HLG transfer function, which means that certain media and applications that did not previously work in HDR are now working as expected.

## But that’s not all…

Plasma 6.8 is chock-full of smaller changes, including:

- A new `kscreenctl` tool makes managing displays from the command-line easier and also offers the potential for scripting and automation.
- Kup, the KDE backup tool, has been polished up and is now released as a part of Plasma.
- We have added EAP-TTLS authentication support for StrongSwan VPNs.
- You can enable and disable auto-starting apps without having to delete and recreate their entries.
- Use the arrow keys to move screens pixel-by-pixel in *Display Configuration*.
- The system shows a warning notification when trying to print using an unavailable printer.
- Copying secrets such as passwords to the clipboard now clarifies why they are not included in history.
- Emojis in the *Emoji Selector* can now be shown at different sizes.
- …and countless bug fixes and small tweaks to make Plasma better than ever!

To see a high-level overview of all user-facing changes, [see the Plasma 6.8 wiki page](https://community.kde.org/Plasma/Plasma_6#Plasma_6.8).

To see the full list of *all* changes, read the [complete changelog](https://kde.org/announcements/changelogs/plasma/6/6.7.5-6.8.0/).

## Feedback

Discuss KDE on the [KDE Forums](https://discuss.kde.org/).

You can provide feedback direct to the developers via the [#Plasma Matrix channel](https://go.kde.org/matrix/#/%23plasma:kde.org), [Plasma-devel mailing list](https://mail.kde.org/mailman/listinfo/plasma-devel) or report issues via [Bugzilla](https://bugs.kde.org/enter_bug.cgi?product=plasmashell&amp;format=guided). If you like what the team is doing, please let them know!

Your feedback is greatly appreciated.

## Supporting KDE

KDE is a [Free Software](https://www.gnu.org/philosophy/free-sw.html) community that exists and grows only because of the help of many volunteers that donate their time and effort. KDE is always looking for new volunteers and contributions, whether it is help with coding, bug fixing or reporting, writing documentation, translations, promotion, money, etc. All contributions are gratefully appreciated and eagerly accepted. Please read through the [Supporting KDE page](https://kde.org/community/donations/) for further information or become a KDE e.V. supporting member through our [Join the Game](https://kde.org/community/donations/) initiative.

## About KDE

KDE is an international technology team that creates free and open source software for desktop and portable computing. Among KDE’s products are a modern desktop system for Linux and UNIX platforms, comprehensive office productivity and groupware suites and hundreds of software titles in many categories including Internet and web applications, multimedia, entertainment, educational, graphics and software development. KDE software is translated into more than 60 languages and is built with ease of use and modern accessibility principles in mind. KDE’s full-featured applications run natively on Linux, BSD, Windows, Haiku, and macOS.

* * *

*Trademark Notices.*

KDE® and the K Desktop Environment® logo are registered trademarks of KDE e.V..\
Linux is a registered trademark of Linus Torvalds. UNIX is a registered trademark of The Open Group in the United States and other countries.\
All other trademarks and copyrights referred to in this announcement are the property of their respective owners.

* * *

#### Press Contacts

For more information send us an email: [press@kde.org](mailto:press@kde.org)
