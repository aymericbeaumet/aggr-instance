---
title: GrapheneOS' rewritten Messages app is released
link: https://github.com/GrapheneOS/Messaging/releases/tag/13
source: hnrss-org-frontpage
published: 2026-09-11T18:50:36Z
updated: 2026-09-11T18:50:36Z
first_seen: 2026-09-11T23:11:51.018809841Z
authors:
- microtonal
summary: 'Article URL: https://github.com/GrapheneOS/Messaging/releases/tag/13 Comments URL: https://news.ycombinator.com/item?id=49663373 Points: 155 # Comments: 89'
content: extracted
html: 2026-09-11-grapheneos-rewritten-messages-app-is-released.html
preview:
  file: 2026-09-11-grapheneos-rewritten-messages-app-is-released.preview-767efbb36382.webp
  width: 256
  height: 128
  alt: 'Notable changes in version 13: Version 13 replaces the legacy interface with Jetpack Compose and Material 3. It rebuilds every screen, adds new conversation controls and large-screen support, and f...'
  color: '#f7f5f7'
images:
- source: https://opengraph.githubassets.com/8f0c4809e2c11bfed73cfc4c4e8d4260b6cea167ccc47bfdcf0c2ebd6b69acf2/GrapheneOS/Messaging/releases/tag/13
  original:
    file: 2026-09-11-grapheneos-rewritten-messages-app-is-released.image-0479d93fa03a.png
    width: 1200
    height: 600
  variants:
  - file: 2026-09-11-grapheneos-rewritten-messages-app-is-released.image-540f102a7be5.webp
    width: 48
    height: 24
  color: '#fefefe'
---

Notable changes in version 13:

Version 13 replaces the legacy interface with Jetpack Compose and Material 3. It rebuilds every screen, adds new conversation controls and large-screen support, and fixes problems with crashes, notifications, security, and message handling.

## Interface

- Material 3 / Expressive design with shapes
- Two-pane conversation layout on large screens
- New adaptive app icon with a monochrome variant
- New onboarding covering SMS privacy, permissions, and default-app setup
- Correct display cutout and system bar handling in both orientations

## Conversation list

- Pin conversations
- Snooze notifications for 1, 8, or 24 hours, or indefinitely
- Mark conversations as unread from the menu or by swiping
- Swipe to archive or unarchive
- Quick actions from conversation avatars
- Redesigned multi-select actions for archiving, deletion, blocking, and notifications
- Indicators for unread, pinned, snoozed, notifications off, work profiles, and incoming MMS status
- Redesigned archive screen
- Pinning, archiving, and deletion now apply immediately

## Conversations

- Rebuilt message bubbles, grouping, selection, and link handling
- Select multiple messages for deletion
- Full-screen message details with copyable sender, recipients, timestamps, delivery status, size, type, and priority
- SMS segment and character counter
- Add, edit, or remove MMS subjects from the conversation overflow menu
- Blocked-sender banner with an unblock action
- Add participants to existing conversations or create groups from the new-chat screen
- Redesigned recipient picker with alphabetical sections, email addresses, formatted numbers, and multiple numbers per contact
- Emergency numbers cannot be dialed from conversations
- SIM fallback now uses the system default instead of the first SIM
- Top-bar actions for calls, contact cards, and conversation settings
- Attachment limits checked before sending, with visible send errors
- Messages arriving during conversation deletion are no longer destroyed

## Attachments and media

- Rebuilt media picker with photo and video capture, flash controls, and Android's embedded photo picker
- Redesigned audio recording with slide-to-cancel and hands-free locking
- Attachment captions
- Rewritten photo viewer with pinch-to-zoom, page indicators, details, and correct system bar handling
- Rewritten vCard viewer with avatars, contact-change refresh, and saving to contacts

## Sharing and forwarding

- New share picker with search, recent conversations, alphabetical contacts, and multi-select
- Edit shared content, add a subject, preview it, and choose a SIM
- Forwarding and the widget use the same picker
- The widget's new-message button opens the new-chat screen
- Missing shared text is read from its content URI; missing subjects use the shared title
- Sharing failures are now reported

## Settings

- Rewritten main, general, and per-SIM settings
- New Privacy section
- Rewritten licenses screen with generated license data
- Existing per-conversation notification settings are preserved

## Privacy and security

- YouTube link previews are opt-in and disabled by default
- Shared-content validation rejects `file:` URIs and private app files, and checks content URI permissions
- Shared text read from content URIs receives the same private-file checks
- Widget receivers are no longer exported, and widget intents are restricted to the app
- Pending intents use `FLAG_IMMUTABLE` where mutability is not required
- Allocation limits added to EXIF APP1, MMS PDU, and MMS content-type parsing
- Fixed GIF null dereferences, missing dimension checks before native transcoding, and signed-character colour corruption
- Updated the AOSP vCard parser with upstream fixes
- Bounded notification people lists, fixing issue a reported crash
- Onboarding explains that SMS is unencrypted and recommends end-to-end encryption for sensitive messages

## Crash fixes

- Fixed crashes in the widget, failed SMS database inserts, declined-call quick responses, settings for unsaved numbers, and share intents
- Share intents no longer silently drop attachments

## Messages and notifications

- Incoming SMS messages are imported immediately
- Failed-message notifications are delivered correctly
- Inline notification replies no longer open the home screen
- Unread notifications survive reboots
- Blocked conversations no longer play notification sounds
- Deleted conversations no longer receive notifications during sync
- Sync no longer overwrites archive changes
- Fixed a race between MMS downloads and notifications
- Pending MMS messages show their subject and download status
- Unknown group senders show their phone number
- Rewritten Class 0 message dialog with correct timeout and lifecycle handling
- Low-storage warnings are dismissible and also appear when the message database is full

## Multiple users and work profiles

- Secondary-user notifications show message content and use unique IDs
- Secondary users are told that pending MMS must be retrieved by the Owner user; the downloaded messages are then available to all users
- Work-profile conversations are labelled

## Accessibility

- Screen-reader labels for screens, panes, controls, and rows
- Timestamps announce full weekday names
- Copying message details is available as an accessibility action
- Fixed conversation-list focus and swipe-action state announcements
- Sending a message produces an announcement and sound

## Testing

- Expanded unit and instrumented tests for the new data, domain, and Compose UI layers
- Tests, builds, and static analysis run on every pull request
- License reports are generated and checked in CI

## Platform and dependencies

Updates to dependencies and required platform version:

- `minSdk` 36
- `targetSdk` 37
- `compileSdk` 37
- Android Gradle Plugin 9.3.2
- Kotlin 2.4.10
- Gradle 9.7.1
- Glide 5.0.9
- Guava 33.7.1
- libphonenumber 9.0.38
- Added Compose BOM 2026.08.00, Material 3 Adaptive, Navigation 3, Coil 3, and CameraX

A full list of changes from the previous release (version 12) is available through the [Git commit log between the releases](https://github.com/GrapheneOS/Messaging/compare/12...13).
