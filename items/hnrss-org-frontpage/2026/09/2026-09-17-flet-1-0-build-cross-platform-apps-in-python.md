---
title: Flet 1.0 – Build cross-platform apps in Python
link: https://flet.dev/
source: hnrss-org-frontpage
published: 2026-09-17T20:44:28Z
updated: 2026-09-17T20:44:28Z
first_seen: 2026-09-18T11:44:14.358449154Z
authors:
- absqueued
summary: 'Article URL: https://flet.dev/ Comments URL: https://news.ycombinator.com/item?id=49746290 Points: 132 # Comments: 62'
content: extracted
html: 2026-09-17-flet-1-0-build-cross-platform-apps-in-python.html
preview:
  file: 2026-09-17-flet-1-0-build-cross-platform-apps-in-python.preview-24beb6eb5b12.webp
  width: 120
  height: 120
  color: '#5d4a80'
images:
- source: https://flet.dev/img/pages/home/controls.svg
  original:
    file: 2026-09-17-flet-1-0-build-cross-platform-apps-in-python.image-3eee123d7a22.png
    width: 120
    height: 120
  variants:
  - file: 2026-09-17-flet-1-0-build-cross-platform-apps-in-python.image-24beb6eb5b12.webp
    width: 120
    height: 120
  color: '#fe005e'
- source: https://flet.dev/img/pages/home/python-packages.svg
  original:
    file: 2026-09-17-flet-1-0-build-cross-platform-apps-in-python.image-ab3194dd52d8.png
    width: 120
    height: 120
  variants:
  - file: 2026-09-17-flet-1-0-build-cross-platform-apps-in-python.image-092009862520.webp
    width: 120
    height: 120
  color: '#fe005e'
- source: https://flet.dev/img/pages/home/packaging.svg
  original:
    file: 2026-09-17-flet-1-0-build-cross-platform-apps-in-python.image-b830963ab9ba.png
    width: 120
    height: 120
  variants:
  - file: 2026-09-17-flet-1-0-build-cross-platform-apps-in-python.image-bd18a346bd71.webp
    width: 120
    height: 120
  color: '#00cafe'
- source: https://flet.dev/img/pages/home/web-support.svg
  original:
    file: 2026-09-17-flet-1-0-build-cross-platform-apps-in-python.image-46e823b79e12.png
    width: 120
    height: 97
  variants:
  - file: 2026-09-17-flet-1-0-build-cross-platform-apps-in-python.image-5be470875d2b.webp
    width: 120
    height: 97
  color: '#00cafe'
- source: https://flet.dev/img/pages/home/app-testing.svg
  original:
    file: 2026-09-17-flet-1-0-build-cross-platform-apps-in-python.image-687c39f63068.png
    width: 120
    height: 120
  variants:
  - file: 2026-09-17-flet-1-0-build-cross-platform-apps-in-python.image-e2adfdb41998.webp
    width: 120
    height: 120
  color: '#00cafe'
- source: https://flet.dev/img/pages/home/ai-assistance.svg
  original:
    file: 2026-09-17-flet-1-0-build-cross-platform-apps-in-python.image-a88c2cf37362.png
    width: 120
    height: 120
  variants:
  - file: 2026-09-17-flet-1-0-build-cross-platform-apps-in-python.image-24acbf8055ca.webp
    width: 120
    height: 120
  color: '#00cafe'
- source: https://flet.dev/img/pages/home/extensible.svg
  original:
    file: 2026-09-17-flet-1-0-build-cross-platform-apps-in-python.image-41cb605a0b57.png
    width: 120
    height: 115
  variants:
  - file: 2026-09-17-flet-1-0-build-cross-platform-apps-in-python.image-dc0591d17f97.webp
    width: 120
    height: 115
  color: '#00cafe'
- source: https://flet.dev/img/pages/home/accessible.svg
  original:
    file: 2026-09-17-flet-1-0-build-cross-platform-apps-in-python.image-ed60b0f40ce0.png
    width: 120
    height: 120
  variants:
  - file: 2026-09-17-flet-1-0-build-cross-platform-apps-in-python.image-093e02edb25d.webp
    width: 120
    height: 120
  color: '#00cafe'
---

[Meet Flet 1.0](https://flet.dev/blog/flet-1-0)

Your next app.\
Built in Python.
---

Build beautiful web, desktop, and mobile apps from one Python codebase.

No frontend experience required. Just Python.

One codebase.\
**Make yourself at home.**

- iOS
- Android
- Windows
- macOS
- Linux
- Web

A SIMPLE EXAMPLE

## Your first Flet app.

Start with a simple counter to see how Flet turns Python code into an interactive app.

1. ### Build the interface

   Use ready-made controls for the text and button. Arrange them with Python.

2. ### Add the behavior

   Connect the button to a Python function that increases the count.

TRY IT YOURSELF

[Try online](https://studio.flet.dev/gallery/getting-started/example/apps/templates/basic_counter)

Open Flet Studio.\
No installation needed.

[Try locally](https://flet.dev/docs/getting-started/installation)

Follow the installation guide for uv or pip.

counter.py

```
import flet as ftdef main(page: ft.Page):    counter = ft.Text("0", size=50, data=0)    def increment(e):        counter.data += 1        counter.value = str(counter.data)    page.floating_action_button = ft.FloatingActionButton(        icon=ft.Icons.ADD, on_click=increment    )    page.add(        ft.Container(            content=counter,            alignment=ft.Alignment.CENTER,            expand=True,        )    )ft.run(main)
```

flet run counter.py

BUILT FOR THE WHOLE JOURNEY

## More than a pretty interface.

The controls, libraries, and tools to take your app from an experiment to something you ship.

![](https://flet.dev/img/pages/home/controls.svg)

### Good-looking Python GUIs

150+ controls and services. Layouts, navigation, forms, and dialogs, with customizable colors, typography, and themes.

[Explore the controls](https://flet.dev/docs/controls)

![](https://flet.dev/img/pages/home/python-packages.svg)

### Your Python libraries. On mobile.

Bring NumPy, pandas, Pillow, and cryptography along. Prebuilt packages for iOS and Android save you the work of compiling native dependencies.

[Browse Python packages](https://flet.dev/docs/reference/binary-packages-android-ios)

![](https://flet.dev/img/pages/home/packaging.svg)

### Ready to ship

Package your app for desktop, mobile, and web with flet build. Prepare it for distribution, including the App Store and Google Play.

[Build and publish](https://flet.dev/docs/publish)

![](https://flet.dev/img/pages/home/web-support.svg)

### The web, your way

Run Python in the browser with Pyodide and WebAssembly, or keep your code on a server and send real-time UI updates.

[Explore web deployment](https://flet.dev/docs/publish/web)

![](https://flet.dev/img/pages/home/app-testing.svg)

### Test your app

Write pytest tests that tap buttons, enter text, and check user flows in your packaged app. Catch visual changes with screenshots on iOS and Android.

[Test your app](https://flet.dev/docs/getting-started/integration-testing)

![](https://flet.dev/img/pages/home/ai-assistance.svg)

### Give your AI the right context

Connect your coding assistant to Flet MCP for version-specific API information and tools to find examples, icons, and CLI options.

[Connect Flet MCP](https://flet.dev/docs/cookbook/flet-mcp)

![](https://flet.dev/img/pages/home/extensible.svg)

### Make it your own

Compose custom controls in Python or wrap Flutter packages in extensions to add new UI components and platform integrations.

[Build an extension](https://flet.dev/docs/extend/user-extensions)

![](https://flet.dev/img/pages/home/accessible.svg)

### Build for more people

Support screen readers with labels and custom semantics. Add keyboard shortcuts and inspect the accessibility information your UI exposes.

[Explore accessibility](https://flet.dev/docs/cookbook/accessibility)

MEET FLET STUDIO

An idea is a\
great place to start.
---

Open your browser. Pick an example, write some Python, or ask the AI agent for a hand. Run your app and share what you make.

[Create in Studio](https://studio.flet.dev) No installation required.

01

### Find your starting point

Explore the Gallery and make an example your own.

02

### Make it work your way

Edit the code, get help from AI, and see your app run.

03

### Share what you made

Send a link, or download your project and keep building locally.

[Explore the Gallery](https://studio.flet.dev/gallery)
