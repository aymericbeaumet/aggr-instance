---
title: Android apps running native on macOS
link: https://github.com/kksimp/Mac.apk
source: hnrss-org
published: 2026-10-19T16:45:30Z
updated: 2026-10-19T16:45:30Z
first_seen: 2026-10-19T23:02:59.728557376Z
authors:
- flocked
content: extracted
html: 2026-10-19-android-apps-running-native-on-macos.html
remote_preview:
  url: https://opengraph.githubassets.com/331fba8705ca61b2a58d87ebd36b0e50a092158f8d722dc5ad57ce5704f4812a/kksimp/Mac.apk
  alt: 'Mac.apk: Run Android apps natively on Apple Silicon Macs - kksimp/Mac.apk'
---

**Run Android apps natively on Apple Silicon Macs. No emulator, no virtual machine, no Linux underneath.**

[![Latest release](https://camo.githubusercontent.com/2b2f2359415b28b0d5dc970bb1f3fa9e38938738a2ddd2efb023765d68cbea1e/68747470733a2f2f696d672e736869656c64732e696f2f6769746875622f762f72656c656173652f6b6b73696d702f4d61632e61706b3f6c6162656c3d6c617465737426636f6c6f723d366335636537)](https://github.com/kksimp/Mac.apk/releases/latest) [![License: AGPL-3.0](https://camo.githubusercontent.com/e75361a6644d5c0f8c3efce74ee774f25811f1c1b9a2d7c7d3a1f05a33d790a3/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f6c6963656e73652d4147504c2d2d332e302d626c7565)](https://github.com/kksimp/Mac.apk/blob/main/LICENSE) [![Commercial license available](https://camo.githubusercontent.com/a3395636ca5f49c1b88580fd14d946e857597c63987274b93a63b33eef3c4db4/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f636f6d6d65726369616c2532306c6963656e73652d617661696c61626c652d626c7565)](https://github.com/kksimp/Mac.apk/blob/main/COMMERCIAL.md) [![Signed and notarized](https://camo.githubusercontent.com/2bc7bddecb45935a01c72cc6232a0967f49316bd27501d755f6f40cb2340a8cc/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f7369676e65642d6e6f746172697a656425323062792532304170706c652d303030303030)](https://github.com/kksimp/Mac.apk/wiki/Installation) [![Sponsor](https://camo.githubusercontent.com/4b4f2b728ee51438a0ad422e2de6b7b69a0045c8b93f9aec0ae6e9258a77de4d/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f73706f6e736f722d6b6b73696d702d6561346161613f6c6f676f3d67697468756273706f6e736f7273266c6f676f436f6c6f723d7768697465)](https://github.com/sponsors/kksimp)

### 📱 [Which apps work? See the compatibility list →](https://github.com/kksimp/Mac.apk/blob/main/COMPATIBILITY.md)

[![Apps tested: 65](https://camo.githubusercontent.com/2f5943ceb5af6e8663ef767eb2990d7c05a3741f573f07cae054e64582090a42/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f617070732532307465737465642d36352d366335636537)](https://github.com/kksimp/Mac.apk/blob/main/COMPATIBILITY.md) [![Fully playable: 51](https://camo.githubusercontent.com/765ec69ba37859a239068016369bb5cd86ee5194d2819fcc54ed20ef6a2579d4/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f66756c6c79253230706c617961626c652d35312d326561343466)](https://github.com/kksimp/Mac.apk/blob/main/COMPATIBILITY.md) [![Partly works: 10](https://camo.githubusercontent.com/c4ad2ba5337174845210fdc825641261da05f5b46579c1b7a191a09c30f75a9f/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f706172746c79253230776f726b732d31302d646662333137)](https://github.com/kksimp/Mac.apk/blob/main/COMPATIBILITY.md) [![Not yet: 4](https://camo.githubusercontent.com/ef1a4c86b398657a2379a9e48317b6728e48a3492caee546ae96c0836f94c885/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f6e6f742532307965742d342d653035643434)](https://github.com/kksimp/Mac.apk/blob/main/COMPATIBILITY.md)

* * *

**Alpha.** Mac.apk is early software under active development. Some apps run beautifully, some run partially, and some do not run yet. It is stable enough to use and interesting enough to explore, and it improves with every build.

## What it is

Mac.apk runs Android apps directly on Apple Silicon. Apps launch as ordinary macOS processes, draw through the Mac's GPU, and execute on the CPU at native speed. They appear in your Dock, get their own windows with a back button next to the traffic lights, resize and rotate like any Mac window, and switch between light and dark along with your Mac.

There is no emulator here, and no virtual machine. Nothing boots Android in the background, there is no Linux kernel running underneath, and nothing is interpreted instruction-by-instruction. Mac.apk implements the Android application environment on top of macOS directly. [How it works →](https://github.com/kksimp/Mac.apk/wiki/How-Mac.apk-Works)

[![How an Android app runs on your Mac: the app code, native code and resources in an APK go through Mac.apk and come out as a real Mac app](https://github.com/kksimp/Mac.apk/raw/main/images/how-mac-apk-works-overview.png)](https://github.com/kksimp/Mac.apk/blob/main/images/how-mac-apk-works-overview.png)

## 📖 [Read the wiki →](https://github.com/kksimp/Mac.apk/wiki)

|                                 |                                                                                                                                                                                                                                                                                                                                                                               |
| ------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Getting started**             | [Installation](https://github.com/kksimp/Mac.apk/wiki/Installation)                                                                                                                                                                                                                                                                                                          |
| **Installing apps**             | [Installing apps](https://github.com/kksimp/Mac.apk/wiki/Installing-Apps) · [App stores](https://github.com/kksimp/Mac.apk/wiki/App-Stores) · [How apps are installed](https://github.com/kksimp/Mac.apk/wiki/How-Apps-Are-Installed) · [Managing apps](https://github.com/kksimp/Mac.apk/wiki/Managing-Apps)                                                             |
| **Using apps**                  | [Controls and input](https://github.com/kksimp/Mac.apk/wiki/Controls-and-Input) · [Windows and appearance](https://github.com/kksimp/Mac.apk/wiki/Windows-and-Appearance) · [Files and privacy](https://github.com/kksimp/Mac.apk/wiki/Files-and-Privacy) · [Google accounts and Play Services](https://github.com/kksimp/Mac.apk/wiki/Google-Accounts-and-Play-Services) |
| **Flags and advanced settings** | [Flags](https://github.com/kksimp/Mac.apk/wiki/Flags) · [Flags reference](https://github.com/kksimp/Mac.apk/wiki/Flags-Reference) · [Developer menu](https://github.com/kksimp/Mac.apk/wiki/Developer-Menu)                                                                                                                                                                |
| **Help**                        | [Known issues](https://github.com/kksimp/Mac.apk/wiki/Known-Issues) · [Troubleshooting](https://github.com/kksimp/Mac.apk/wiki/Troubleshooting) · [Reporting problems](https://github.com/kksimp/Mac.apk/wiki/Reporting-Problems)                                                                                                                                          |
| **About Mac.apk**               | [How Mac.apk works](https://github.com/kksimp/Mac.apk/wiki/How-Mac.apk-Works) · [Design decisions](https://github.com/kksimp/Mac.apk/wiki/Design-Decisions) · [For developers and publishers](https://github.com/kksimp/Mac.apk/wiki/For-Developers-and-Publishers)                                                                                                        |

## System requirements

|                                 |                                                                                    |
| ------------------------------- | ---------------------------------------------------------------------------------- |
| **macOS 26.4 (Tahoe) or newer** | Required. macOS 27 works from v1.0.1702                                            |
| **Apple Silicon**               | Any Apple Silicon Mac, including MacBook Neo. Intel Macs are not supported         |
| **Disk**                        | \~280 MB for Mac.apk. Each installed app takes roughly 1.5 to 3 times its APK size |

## Quick start

1. Download the disk image from the [Releases page](https://github.com/kksimp/Mac.apk/releases/latest) and drag **Mac.apk** into **Applications**. It is signed and notarized by Apple, so it opens normally.
2. Open Mac.apk and click **Install** next to **Runtime:** in the top left.
3. Open **Settings** (the gear), find **File types**, click **Make Default**, and approve the macOS prompt.
4. Double-click any `.apk` (or `.apkm`, `.xapk`, `.apks`, `.apkx`), or drag it onto the Mac.apk window.

[![Installing an Android app in four steps: pick an app, confirm, it becomes a Mac app, first launch](https://github.com/kksimp/Mac.apk/raw/main/images/installing-an-app.png)](https://github.com/kksimp/Mac.apk/blob/main/images/installing-an-app.png)

**The first launch of any app is slow** (up to a minute for a large one) while Mac.apk prepares it. Later launches are fast. Full details, updating, and the app stores are in the [Installation](https://github.com/kksimp/Mac.apk/wiki/Installation) and [App stores](https://github.com/kksimp/Mac.apk/wiki/App-Stores) pages.

Warning

An Android app's saved data lives inside its own `.app`. Dragging the app to the Trash deletes its saves; uninstall from the Mac.apk control panel instead. [More →](https://github.com/kksimp/Mac.apk/wiki/Managing-Apps)

## Known issues in v1.0.2237

See [Known issues](https://github.com/kksimp/Mac.apk/wiki/Known-Issues) for this release, and the [compatibility list](https://github.com/kksimp/Mac.apk/blob/main/COMPATIBILITY.md) for every app tested by hand.

## Reporting problems

Send the Mac.apk version, your macOS version, the app and where you got it, what happened, and the app's log from `~/Library/Logs/macapk/` to [Kaleb@voltare.us](mailto:Kaleb@voltare.us) or [open an issue](https://github.com/kksimp/Mac.apk/issues/new). Look the log over before posting it publicly. [What to include →](https://github.com/kksimp/Mac.apk/wiki/Reporting-Problems)

## Source code

Everything that makes Mac.apk work is in this repository: the Android framework reimplementation, the native runtime, the arm64 loader and Bionic shim, the ARM32 recompiler and the macOS app. [BUILDING.md](https://github.com/kksimp/Mac.apk/blob/main/BUILDING.md) explains how to build it, and [CONTRIBUTING.md](https://github.com/kksimp/Mac.apk/blob/main/CONTRIBUTING.md) how to send a fix. App reports are just as useful as code.

## Developers and publishers

Mac.apk serves no ads and supports no in-app purchases, by design. If you publish an app that runs on Mac.apk and want to talk about a partnership, or would rather it not run here, email [Kaleb@voltare.us](mailto:Kaleb@voltare.us). [Details →](https://github.com/kksimp/Mac.apk/wiki/For-Developers-and-Publishers)

## License

Mac.apk is **open source under the [GNU AGPL v3.0](https://github.com/kksimp/Mac.apk/blob/main/LICENSE)**, and a **commercial license** is available for companies that want to ship or offer it without the AGPL's terms. Using the official app is free. Details: [COMMERCIAL.md](https://github.com/kksimp/Mac.apk/blob/main/COMMERCIAL.md). The Mac.apk name and logo are covered separately in [TRADEMARKS.md](https://github.com/kksimp/Mac.apk/blob/main/TRADEMARKS.md).

## Disclaimer

Mac.apk is provided "as is", with no warranty of any kind. You are responsible for the Android apps you choose to load, and Voltare cannot test, audit, or vouch for any third-party APK. Voltare is not liable for damages, data loss, malware, security incidents, copyright disputes, or any other consequence of code that runs through Mac.apk.

**Only install APKs you trust.** An Android app running under Mac.apk runs as ordinary native code with your macOS user account's permissions. There is no phone-style app sandbox around it: it can read and write your files the same way any Mac app you run can. Treat an APK exactly as you would any program you download and run yourself, and get it from a source you trust.

## Third-party software

Mac.apk bundles several open-source components (ANGLE, MoltenVK and the Vulkan loader, Capstone, libpng, libvpx, stb\_vorbis, Bouncy Castle, a JRE built from OpenJDK together with the libraries its font and image code uses, such as FreeType, HarfBuzz and GLib, and the Android platform resources built from AOSP source). Each is used under its own licence; the full notices are in [THIRD\_PARTY\_NOTICES.txt](https://github.com/kksimp/Mac.apk/blob/main/THIRD_PARTY_NOTICES.txt), and a copy ships inside the app at `Contents/Resources/THIRD_PARTY_NOTICES.txt`.

Two of them, GLib and GNU gettext's libintl, are licensed under the LGPL. Their complete source for the exact versions in each release (upstream source, patches and build recipe) is attached to that release on the [Releases page](https://github.com/kksimp/Mac.apk/releases), with `LGPL-SOURCE-README.txt` explaining how to use a modified build.

## Links

- **Project page:** [https://projects.voltare.us/macapk](https://projects.voltare.us/macapk)
- **Wiki:** [https://github.com/kksimp/Mac.apk/wiki](https://github.com/kksimp/Mac.apk/wiki)
- **Commercial licensing:** [Kaleb@voltare.us](mailto:Kaleb@voltare.us)
- **Bug reports:** [Kaleb@voltare.us](mailto:Kaleb@voltare.us) or [open an issue](https://github.com/kksimp/Mac.apk/issues/new)
