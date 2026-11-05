---
title: Discord is ditching Electron and its new app uses less RAM
link: https://www.theverge.com/news/1027379/discord-ditching-electron-web-app
source: hnrss-org
published: 2026-11-05T13:05:28Z
updated: 2026-11-05T13:05:28Z
first_seen: 2026-11-05T17:57:26.867095589Z
authors:
- archb
content: extracted
html: 2026-11-05-discord-is-ditching-electron-and-its-new-app-uses-less-ram.html
remote_preview:
  url: https://platform.theverge.com/wp-content/uploads/sites/2/2025/03/acastro_STK062_01.jpg?quality=90&strip=all&crop=0%2C10.732984293194%2C100%2C78.534031413613&w=1200
  alt: Discord logo
---

[![Tom Warren](https://platform.theverge.com/wp-content/uploads/sites/2/2025/01/Tom_BLURPLE.jpg?quality=90&strip=all&crop=0%2C0%2C100%2C100&w=96)](https://www.theverge.com/authors/tom-warren)

 Tom Warren

is a senior correspondent and author of [*Notepad*](https://www.theverge.com/notepad-microsoft-newsletter), who has been covering all things Microsoft, PC, and tech for over 20 years.

Discord is finally giving up on Electron, the cross-platform framework that lets developers build desktop apps using web technologies. Electron web apps are notoriously heavy on system resources, and Discord is now moving its desktop app fully to Rust — a modern programming language designed for speed and memory safety.

“We’ve been early adopters of Rust since 2018 and a lot of our infra, voice tech, and even some of the current client is already Rust, the holy grail was getting the whole client there,” explains Discord chief technology officer Stanislav Vishnevskiy in a [post on X](https://x.com/svishnevskiy/status/2118081920194203799). “That means Electron goes away, and so does React Native, mobile keeps native UI but shares app code with desktop via Rust.”

Vishnevskiy also shared an image of the Discord Windows client using just 290MB of RAM, which is a clear improvement as you only have to participate in a few servers and RAM usage can easily hit 1GB or more using the current Windows app.

It’s not clear when Discord will make the full switch over to Rust, but Vishnevskiy says the company “hope to get it into testing with users soon.”

**Follow topics and authors** from this story to see more like this in your personalized homepage feed and to receive email updates.

- Tom Warren
-
-
-
-
-
