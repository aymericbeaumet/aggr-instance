---
title: 'ExcalidrawZ: Local Native Excalidraw on macOS/iOS'
link: https://github.com/chocoford/ExcalidrawZ
source: hnrss-org
published: 2026-11-12T15:25:52Z
updated: 2026-11-12T15:25:52Z
first_seen: 2026-11-13T00:08:02.707796831Z
authors:
- Qision
content: extracted
html: 2026-11-12-excalidrawz-local-native-excalidraw-on-macos-ios.html
remote_preview:
  url: https://opengraph.githubassets.com/a4e76c531ea7daa85ba689f4c82eddbedb444cf471be04037ea8a28b755238e9/chocoford/ExcalidrawZ
  alt: Excalidraw app for mac. Powered by pure SwiftUI. Contribute to chocoford/ExcalidrawZ development by creating an account on GitHub.
---

[![ExcalidrawZ logo](<https://github.com/chocoford/ExcalidrawZ/raw/main/ExcalidrawZ/Assets.xcassets/AppIcon-macOS.imageset/ExcalidrawZ Icon 26-iOS-Default-83.5x83.5@3x.png?raw=true>)](https://excalidraw.com)

### A native Excalidraw client for macOS, iPadOS, and iOS. Powered by SwiftUI.

[![GitHub](https://camo.githubusercontent.com/4ae632304935c4168f024db56753d2a1897eba9ca1de8487cb6afb119ec01501/68747470733a2f2f696d672e736869656c64732e696f2f6769746875622f6c6963656e73652f63686f636f666f72642f457863616c69647261775a)](https://camo.githubusercontent.com/4ae632304935c4168f024db56753d2a1897eba9ca1de8487cb6afb119ec01501/68747470733a2f2f696d672e736869656c64732e696f2f6769746875622f6c6963656e73652f63686f636f666f72642f457863616c69647261775a) [![Twitter](https://camo.githubusercontent.com/9086f8956b68438f1572a404815e36383ab2633d1d871f0f93300462f1974ab8/68747470733a2f2f696d672e736869656c64732e696f2f747769747465722f75726c2f68747470732f747769747465722e636f6d2f636c6f7564706f7373652e7376673f7374796c653d736f6369616c266c6162656c3d466f6c6c6f7725323025343043686f636f666f7264)](https://x.com/Chocoford_) [![Discord](https://camo.githubusercontent.com/ac5ad94c84831563e60831dafa299b99778da778ab67b91c27ed954c4a88ad1f/68747470733a2f2f696d672e736869656c64732e696f2f646973636f72642f393434313630303932393134333139333631)](https://discord.gg/aCv6w4HxDg)

[![Donation to Chocoford](https://github.com/chocoford/chocoford/raw/main/public/Donation%20Button.png?raw=true)](https://www.chocoford.com/donation)

[Excalidraw](https://github.com/excalidraw/excalidraw) is an excellent drawing tool. ExcalidrawZ keeps the familiar Excalidraw canvas, then adds native file management, iCloud sync, history, export, AI assistance, MCP integration, and Apple-platform interaction details around it.

## Download

[![Download Link - App Store](https://github.com/chocoford/ExcalidrawZ/raw/main/assets/README/Download_on_the_App_Store_Badge_US-UK.svg)](https://apps.apple.com/app/excalidrawz/id6636493997)

**Non-App Store version**

1. Download the latest `.dmg` from [Releases](https://github.com/chocoford/ExcalidrawZ/releases).
2. Open the `.dmg` and drag ExcalidrawZ into Applications.

## Preview

[![App overview](https://github.com/chocoford/ExcalidrawZ/raw/main/assets/README/ExcalidrawZ%20-%20Overview.png)](https://github.com/chocoford/ExcalidrawZ/blob/main/assets/README/ExcalidrawZ%20-%20Overview.png)

## Features

### Native File Management

ExcalidrawZ manages drawings as real app documents instead of loose browser downloads.

- Organize drawings with groups and custom file sorting.
- Work with ExcalidrawZ library files, local folders, and temporary external files.
- Open and edit `.excalidraw`, `.excalidraw.png`, and `.excalidraw.svg` files directly.
- Sync app-managed files across macOS, iPadOS, and iOS with iCloud.
- Keep file history checkpoints so important changes can be reviewed and restored.

### File History

ExcalidrawZ records checkpoints for app-managed drawings, so you can review earlier versions and restore important work when needed.

[![File History](https://github.com/chocoford/ExcalidrawZ/raw/main/assets/README/File%20History.gif)](https://github.com/chocoford/ExcalidrawZ/blob/main/assets/README/File%20History.gif)

### Excalidraw Editing, Native Controls

The canvas remains compatible with Excalidraw while the surrounding app is built for Apple platforms.

- Customize toolbar tool order; number shortcuts follow your configured order.
- Use Apple Pencil on iPad, including familiar undo and redo gestures.
- Use mouse and trackpad scroll and zoom gestures on iPad and iPhone.
- Import PDFs and convert Mermaid diagrams into editable Excalidraw content.

### Collaboration

Use Excalidraw collaboration when you want to draw with others.

[![Collaboration](https://github.com/chocoford/ExcalidrawZ/raw/main/assets/README/ExcalidrawZ%20-%20Collaboration.gif)](https://github.com/chocoford/ExcalidrawZ/blob/main/assets/README/ExcalidrawZ%20-%20Collaboration.gif)

### AI Drawing Assistant

ExcalidrawZ includes an AI assistant designed for canvas work.

- Ask AI to read, create, and revise drawings.
- Attach images and let AI use visual context when needed.
- Use proposal previews before applying AI-generated content to the real canvas.
- Control whether each file is visible to AI.
- Use AI from compact iPhone toolbars, iPad floating panels, or the macOS inspector.

[![ai_chat](https://github.com/chocoford/ExcalidrawZ/raw/main/assets/README/ai_chat.gif)](https://github.com/chocoford/ExcalidrawZ/blob/main/assets/README/ai_chat.gif)

### MCP Services

ExcalidrawZ can expose the current app as an MCP server for compatible AI clients.

- Basic mode provides the familiar `excalidraw-mcp` drawing workflow.
- Optimized mode adds deeper ExcalidrawZ integration for current-file editing, file navigation, history, canvas inspection, export, library workflows, and math tools.
- Connect local MCP clients through the app-hosted HTTP endpoint.

[![MCP](https://github.com/chocoford/ExcalidrawZ/raw/main/assets/README/MCP.gif)](https://github.com/chocoford/ExcalidrawZ/blob/main/assets/README/MCP.gif)

### Math and Diagram Tools

ExcalidrawZ includes richer tools for technical drawings and study notes.

- Insert LaTeX-rendered formulas.
- Render function graphs with configurable axes and styles.
- Use reusable math templates and AI-assisted formula generation.
- Edit inserted math images later from the canvas.

[![Math&Diagram](https://github.com/chocoford/ExcalidrawZ/raw/main/assets/README/Math&Diagram.gif)](https://github.com/chocoford/ExcalidrawZ/blob/main/assets/README/Math&Diagram.gif)

### Locked Files and AI Visibility

For sensitive drawings, ExcalidrawZ adds protection around both storage and AI access.

- Lock files with local authentication.
- Encrypt locked files and their checkpoints.
- Use a Recovery Key for fallback access.
- Keep locked content hidden from AI; AI can work through proposal canvases instead.
- Use encrypted backups as an additional recovery path.

[![file_lock](https://github.com/chocoford/ExcalidrawZ/raw/main/assets/README/file_lock.gif)](https://github.com/chocoford/ExcalidrawZ/blob/main/assets/README/file_lock.gif)

### Share, Export, and Backup

ExcalidrawZ supports common export and sharing workflows.

- Export images, editable Excalidraw files, and PDF.
- Preserve editability when exporting `.excalidraw.png` and `.excalidraw.svg`.
- Share through clipboard, files, and the system share sheet.
- Archive app-managed files for backup.

[![Export editable image](https://github.com/chocoford/ExcalidrawZ/raw/main/assets/README/Export%20editable%20image.gif)](https://github.com/chocoford/ExcalidrawZ/blob/main/assets/README/Export%20editable%20image.gif)

## Supported Formats

ExcalidrawZ can open, import, edit, or export common Excalidraw-related formats:

- `.excalidraw`
- `.excalidraw.png`
- `.excalidraw.svg`
- PDF import and export
- image export

## Contact

Welcome to my [Discord server](https://discord.gg/aCv6w4HxDg) to share suggestions or report issues for ExcalidrawZ.

## Development Guide

- The Excalidraw core used by ExcalidrawZ is also open-source. You can find it [here](https://github.com/chocoford/excalidraw/tree/ExcalidrawZ-core).

### Signing configuration

Create `Config/Overrides.xcconfig` relative to the repository root, alongside `Config/Project.xcconfig`:

```
DEVELOPMENT_TEAM = YOUR_TEAM_ID
ICLOUD_CONTAINER = iCloud.com.yourname.ExcalidrawZ
APP_GROUP_IDENTIFIER = group.com.yourname.ExcalidrawZ
```

Replace these example values with identifiers registered to your Apple Developer team. The shared configuration leaves `DEVELOPMENT_TEAM` empty; set your team in the local overrides. The app and QuickLook extension must use the same App Group. All targets, including the extension and tests, inherit the team from `Config/Project.xcconfig`, which includes your local overrides. These overrides apply to all build configurations.

### Runtime configuration

Create `Config/Secrets.plist` relative to the repository root. The project already references this file and includes it in both app targets' resources. The app's configuration loader stops with `Unable to load Secrets.plist` if the file is missing or invalid.

For local drawing development, this minimal configuration is sufficient:

```text-xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>COLLAB_URL</key>
    <string>https://example.com</string>
</dict>
</plist>
```

`COLLAB_URL` must be a valid absolute URL. The example URL is a placeholder for local drawing work; to test collaboration, replace it with the URL of your deployed collaboration-enabled Excalidraw web client.

Optional string keys are `ONEDRIVE_CLIENT_ID`, `GOOGLE_DRIVE_MACOS_CLIENT_ID`, and `GOOGLE_DRIVE_IOS_CLIENT_ID`. Configure your own OAuth client IDs when developing the corresponding integrations.

`Config/Overrides.xcconfig` and `Config/Secrets.plist` are ignored by Git and should stay local.
