---
title: '9/UX: a Macintosh-inspired desktop for 9front'
link: https://progrium.github.io/9ux/
source: hnrss-org
published: 2026-11-27T21:48:14Z
updated: 2026-11-27T21:48:14Z
first_seen: 2026-11-28T14:03:39.806524745Z
authors:
- rbanffy
content: extracted
html: 2026-11-27-9-ux-a-macintosh-inspired-desktop-for-9front.html
remote_preview:
  url: https://progrium.github.io/9ux/images/docs/desktop.png
  alt: 'The 9/UX desktop: Desktop''s menu bar and volume icons, two spatial folder windows, and the libux sample app with a file just dropped onto its Notes field'
---

![The 9/UX desktop: Desktop's menu bar and volume icons, two spatial folder windows, and the libux sample app with a file just dropped onto its Notes field](https://progrium.github.io/9ux/images/docs/desktop.png)

 This is the real thing, running live in your browser: the rio fork in 9/UX mode, Desktop, and the libux sample app, compiled for 386 and run by [h9](https://progrium.github.io/hyper9/) on its own 386 emulator inside the page. Nothing is sent anywhere; it's about 23 MB to load, once. Click the screen to type; right-click (or Ctrl-click) is button 3, and Alt-click is button 2. It needs a recent Chrome or Edge; other browsers see a screenshot, [a real frame](https://progrium.github.io/9ux/images/docs/desktop.png) from the end-to-end drag-and-drop test, running headless on h9. [Open it in its own tab](https://progrium.github.io/9ux/demo/) for a bigger screen.

## About

9/UX brings the classic Mac and A/UX desktop (a spatial desktop, a global menu bar, consistent controls and dialogs, icons, and resource-driven presentation) to [9front](https://9front.org/), built the Plan 9 way: rio and libdraw draw the screen, 9P exposes every application's UI state and commands as files, namespaces define the working environment, the plumber opens documents, and ndb holds human-editable metadata.

> Keep the Macintosh's direct-manipulation desktop, but make every meaningful object, action, and resource visible as a Plan 9 file.

It is not an emulator, a Macintosh ABI, or an A/UX reimplementation. Native 9front programs stay first-class, and existing rio programs (acme, sam, rc windows, nested rio) keep working unchanged beside 9/UX apps.

The pieces:

- **rio fork** (`rio -u`): a desktop layer, a global menu bar that follows the focused app, title-bar chrome, and a drag broker, all under `/dev/ux`. With 9/UX mode off it's stock rio.
- **Desktop**: volume icons, spatial folder windows, rename, duplicate, Get Info, Trash, remembered layout, and double-click to open documents through the plumber. A remote 9P server works as a volume.
- **libux**, the Toolbox: windows, menus, dialogs, alerts, and controls, each app serving its control tree over 9P, so `echo activate >.../ctl` presses a button.
- **uxd** gathers every app's tree at `/mnt/ux` with per-app policy; **uxmeta** keeps associations, volumes and layout in ndb files; **resfs** serves classic `ICN#`, `icl8`, menu, dialog and string resources at `/mnt/res`; **clipd** is a typed clipboard kept in step with rio's snarf.

## Screenshots

These are real frames, not mockups. The GUI tests embed h9 with a headless screen, inject mouse and keyboard input, and drive the 9P files at the same time.

[![The end of the end-to-end demo: the libux sample with a text field set from rc, Desktop windows behind it, an rc window, and acme with a file opened from Desktop](https://progrium.github.io/9ux/images/e2e-demo.png)](https://progrium.github.io/9ux/images/e2e-demo.png)\
The last step of the end-to-end demo. From rc, the sample app's Name field was set to from rc and its Save button pressed through `ctl`; a double click in Desktop plumbed a text file to acme; and an ordinary rc window is still moved and typed into.

[![The libux sample app on 9front's own kernel and VESA framebuffer at 32 bits](https://progrium.github.io/9ux/images/vm-sample-32.png)](https://progrium.github.io/9ux/images/vm-sample-32.png)\
The libux sample on 9front's own kernel and VESA framebuffer in QEMU, at 32 bits per pixel. Its `ICN#` and `icl8` icons match the headless golden images pixel for pixel.

## Visual Design

Macintosh structure, Plan 9 colour: butter title bars from rio's selection yellow, acme's cream and olive, and rio's greygreen for the desktop, so 9/UX windows and legacy rio windows sit together on one screen. Everything reads in one bit, uses a sixteen-colour palette, and is drawn in whole pixels. Every graphic is original to 9/UX; it ships no Apple resources. The mockups below are generated from the same palette and metrics files as the code.

[![Buttons, check boxes, radio buttons, text fields and scroll bars in each state](https://progrium.github.io/9ux/images/mockups/controls@3x.png)](https://progrium.github.io/9ux/images/mockups/controls@3x.png)\
Controls in each state.

[![Window title bars in six themes: nine, nine-sea, butter-deep, nine-grey, seven and win](https://progrium.github.io/9ux/images/mockups/themes/titlebars@2x.png)](https://progrium.github.io/9ux/images/mockups/themes/titlebars@2x.png)\
Title bars in each theme.

[![The 9/UX icon set](https://progrium.github.io/9ux/images/mockups/icons@2x.png)](https://progrium.github.io/9ux/images/mockups/icons@2x.png)\
The icon set.

## Documentation

The [design document](https://github.com/progrium/9ux/blob/main/docs/design.md) describes 9/UX and its v0.1 acceptance criteria; [release.md](https://github.com/progrium/9ux/blob/main/docs/release.md) has where each one stands, the end-to-end demo, and the open issues. Every interface between components is a file protocol in [docs/protocols](https://github.com/progrium/9ux/tree/main/docs/protocols), and the [visual design](https://github.com/progrium/9ux/blob/main/assets/README.md) covers the palette, fonts, metrics, chrome, icons and cursors.

## Source Code

The source is on [GitHub](https://github.com/progrium/9ux), under the MIT license. Development runs on [h9](https://progrium.github.io/hyper9/), which runs 9front's own compilers, `mk` and rio directly from the host shell, without booting a VM. `scripts/h9.sh` clones and builds the pinned h9 on its first run:

```
git clone https://github.com/progrium/9ux
cd 9ux
scripts/h9.sh --rc 'mk'
scripts/h9.sh --gfx rio/7.out -u
scripts/test.sh
```

Use `rio/6.out` on amd64. [development.md](https://github.com/progrium/9ux/blob/main/docs/development.md) and [testing.md](https://github.com/progrium/9ux/blob/main/docs/testing.md) have the details.

## Warning!

9/UX is at v0.1 and under active development. Interfaces and behaviour change without notice.

Please report bugs and send questions through [GitHub issues](https://github.com/progrium/9ux/issues).

## Credits

The 9/UX mark in the menu bar is a pixel-art Glenda by [Dan Fessler](https://danfessler.com/). Glenda, [the Plan 9 bunny](https://9p.io/plan9/glenda.html), was created by [Renée French](https://www.instagram.com/reneefrench/).

## Related Links

- [h9](https://progrium.github.io/hyper9/), native 9front executables on host hypervisors
- [9front](https://9front.org/)
- [A/UX](https://en.wikipedia.org/wiki/A/UX), Apple's Unix with the Macintosh Desktop
