---
title: 'Show HN: Capsule – Single-file web apps that save their data into SQLite'
link: https://withcapsule.app/
source: hnrss-org-frontpage
published: 2026-09-15T13:31:40Z
updated: 2026-09-15T13:31:40Z
first_seen: 2026-09-15T17:16:33.056020916Z
authors:
- bashtian
summary: 'Hey HN, I always had the problem that building HTML pages is really simple now, but trying to save data required hosting it somewhere, and sharing it afterwards was not easy. Over the last few months, I''ve been building an app called Capsule (it’s also the file extension name) written in Rust with Tauri 2.0 that allows packing an HTML app and its data into a single SQLite file. The HTML file and any related assets are directly embedded in the database. User data can either be saved as a localStorage key/value store or via a MongoDB-inspired collections API as documents, saved in a table in the file. You can also save other assets, like PDF files or images, directly in the database to keep different documents together. All data can be easily exported to CSV or JSON if needed. Privacy and security were a big priority for me, so documents cannot do anything out of the box. They don’t have direct access to the file system and they require permission to access the internet. The permission model is still something I’m working to improve. Capsule documents can also use local or remote AI models for document specific AI features. One downside with this approach is that multiple people working on it will create different copies. To make it possible to merge different copies of the same file, each data entry has a unique UUID and timestamp. I’m planning to open up the file format specification for the 1.0 version of the app so other apps can read or write Capsule files. You can try it out in the web preview at https://withcapsule.app/preview with pre-built templates or use any AI provider of your choice to create a custom, Capsule-optimized app by using the following prompt: "Please read the app wizard instructions at https://withcapsule.app/prompt.txt and help me design an app.“ I’m still working on the file format but there are migrations for each new version, so data should never be lost when using newer versions of the app in the future. Please let me know if you have any ideas or use cases where this might make sense or does not work. Comments URL: https://news.ycombinator.com/item?id=49712278 Points: 151 # Comments: 74'
content: extracted
html: 2026-09-15-show-hn-capsule-single-file-web-apps-that-save-their-data.html
preview:
  file: 2026-09-15-show-hn-capsule-single-file-web-apps-that-save-their-data.preview-1b8436b6e88c.webp
  width: 256
  height: 256
  color: '#1a483b'
images:
- source: https://withcapsule.app/og-image.png
  original:
    file: 2026-09-15-show-hn-capsule-single-file-web-apps-that-save-their-data.image-c66495704cf4.png
    width: 512
    height: 512
  color: '#289575'
---

Capsule packs your entire app — UI, data, and everything — into a single portable `.capsule` file. No cloud. No accounts. Just share it.

[](https://withcapsule.app/#why-capsule)

* * *

01 / PROMPT YOUR IDEA

#### Describe what you need

Describe the application, layout, or features you want to build.

02 / INSTANT CAPSULE APP

#### Self-contained output

Generates a complete single-file `.capsule` container with HTML UI, schema, and local SQLite data.

03 / ITERATE ON THE FLY

#### Live AI updates

Modify features, dark mode, or schemas on the fly via direct AI prompts or MCP coding tools.

Choose an AI assistant below to start building your app.

* * *

01 / PORTABILITY

### What If an App Was Just a Document?

Forget cloud accounts, servers, and subscriptions. Capsule bundles your user interface, media assets, and local database into a single, portable `.capsule` file.

Send it via WhatsApp, AirDrop, or email just like a PDF or Word document. When the recipient taps the file, it launches instantly with all your data preloaded, ready to use.

💬

**Share Apps in Chat**

Send interactive trackers, portfolios, or tools in standard message threads. Tapping open works immediately.

🎨

**Zero Vendor Lock-in**

Capsules use standard HTML and CSS. Your code and data belong to you, entirely free of cloud silos.

02 / PRIVACY FIRST

### 100% Private by Design

Capsule keeps your personal data where it belongs, on your device. Everything you create is saved directly into the file, giving you complete ownership with zero cloud servers.

Write tasks, list recipe notes, or save project logs. There is no cloud storage, no account registration, and no network requirement. Everything is secured right inside the file.

🔒

**Offline-First Storage**

Works 100% offline. Access your apps on a plane, on the subway, or completely disconnected.

🔑

**Secure Local Vault**

Data stays safely packed inside the single file. Absolute protection from server breaches.

03 / CROSS-PLATFORM

### One File, Every Operating System

Capsule files are completely cross-platform by default. Open the exact same file on macOS, Windows, or Linux without conversion or special setup.

Your apps launch with full desktop performance on any computer, with native iOS and Android support coming soon.

⚡

**Instant Desktop Launch**

Launches seamlessly on macOS, Windows, and Linux with zero setup or configuration.

📱

**Mobile Support Coming Soon**

Open and run the exact same `.capsule` files on iOS and Android devices.

* * *

GET CAPSULE

## Ready to run portable apps?

Capsule is completely free. Download the host player for your platform and open any `.capsule` file in seconds.

### Desktop

macOS

macOS 12 Monterey or later

Windows

Windows 10 or later (64-bit)

Linux

Ubuntu / Debian / Fedora

### Web

Web

Runs in your browser for a quick preview. Cannot open or save files directly on your computer.

### Mobile (Coming Soon)

iOS

iOS 16 or later

App Store

Android

Android 9 or later

Google Play
