---
title: 'Show HN: BetterWispr – Free, open-source dictation for Mac'
link: https://betterwispr.com/
source: hnrss-org
published: 2026-10-11T12:01:28Z
updated: 2026-10-11T12:01:28Z
first_seen: 2026-10-11T20:11:56.661477988Z
authors:
- kartik017
content: extracted
html: 2026-10-11-show-hn-betterwispr-free-open-source-dictation-for-mac.html
extra:
  comments_url: https://news.ycombinator.com/item?id=50042333
  num_comments: 73
  points: 107
---

Open source dictation for Mac

Speak it messy.\
Send it clean.
---

Hold ⌥ Space in any app and just talk. BetterWispr drops the ums and false starts, spells names your way and writes in the style you set for each app. Speech stays on your Mac.

Free · macOS 14+ · Apple Silicon recommended

- No account
- Local speech models
- Meeting notes built in

![](https://betterwispr.com/assets/hero-statue.png)

 Typing 0.0s 0 wpm

Can we move the design review to Thursday at 3?

 BetterWispr 0.0s Listening

Can we move the design review to Thursday at 3?

Illustration, not a benchmark. Simulated at 40 wpm typing and 150 wpm speaking with a Parakeet model, with time for transcription.

## Speak in any app. BetterWispr handles the rest.

### Stays on your Mac

Built in models transcribe your speech on your Mac, with no account and no cloud fallback. Models download once, then work offline.

### Hold to talk

Hold ⌥ Space while you speak and let go to finish. Switch to press to toggle, or record any shortcut with ⌘, ⌥ or ⌃, or an F key.

### Pick your model

Nine built in models from Apple speech, Parakeet and Whisper. Parakeet covers English, Japanese and 25 European languages, and Whisper covers more. Or choose your own Sarvam AI, Smallest AI or OpenAI compatible API.

### Meeting notes

Notetaker transcribes your mic as Me and other apps as Them, next to your own notes. Apple Intelligence or Ollama then writes the summary, decisions and action items on your Mac.

### A style for every app

Set a tone for personal chats, work apps, email and everything else. Casual drops the final period, very casual drops capitals too, and formal leaves your words alone. Tones apply to English.

### Cleanup you choose

Light, the default, drops English ums, repeats and false starts. Medium also edits for clarity with your notes model, on your Mac with Apple Intelligence or Ollama. None keeps every word.

### Voice commands

Say comma, question mark or new paragraph as you talk. Say scratch that to delete the last sentence. Works in English.

### Learns your words

Add names and terms as hints and replacements. Fix a word in History, or right after a paste, and BetterWispr learns it.

### Types where you were

The text lands in the app you were using. If paste is blocked, it is copied to your clipboard so nothing is lost.

### History you control

Dictations stay on your Mac with the raw and the corrected text. Use Original brings back exactly what you said, and history turns off whenever you like.

### Insights

See how many words you have spoken, your pace, the words cleanup saved you, which apps you dictate into and your daily streak. All of it is counted from history on your Mac.

### Updates you can trust

New versions arrive from GitHub Releases through Sparkle and install only when their EdDSA signature matches.

### Free and open source

Apache 2.0 licensed. Read every line on GitHub, report an issue, or build it yourself with make dev.

From thought to text

One shortcut.\
Your everyday apps.
---

1. 01

   ### Set up once

   Install BetterWispr and follow the welcome guide through permissions and your first local speech model.

2. 02

   ### Hold and speak

   Place your cursor in a text field. Hold Option + Space and say what you want to write.

3. 03

   ### Release to type

   Let go to transcribe. Clean text arrives in the app you were using, in the style you set for it.

What people are saying

## Your voice. In your words.

Posts and replies about BetterWispr on Hacker News and X.

Local by design

Your words stay \
on your Mac.
---

Models run on your Mac. Downloads happen when you choose to install them. Your speech doesn’t need a round trip to a transcription server.

[How your data is handled](https://betterwispr.com/privacy)

From the blog

## Your voice. Your call.

There’s more than one good dictation app. Our comparisons cover local processing, offline use and writing workflows, with official sources.

[All posts](https://betterwispr.com/blog)

Before you download

## A few good questions.

Is BetterWispr free?

Yes. BetterWispr is free and open source under Apache 2.0, with no dictation subscription, account or API key requirement. Model files use storage on your Mac.

Which Macs does it support?

BetterWispr requires macOS 14 or later. Apple Silicon is recommended for local model inference. Model availability and performance depend on your hardware and language.

Does it work without internet?

Yes, once a compatible local model and its required assets are installed. Downloadable models need an explicit installation first. Apple on device speech also depends on supported languages and OS assets. There is no cloud transcription fallback.

How do I dictate?

Focus a text field, hold Option + Space, speak, and release to transcribe. You can switch to press to toggle or choose a different shortcut in Settings. With a Parakeet model, the capsule shows your words as you speak. The text BetterWispr pastes is the transcription of the whole recording, made when you release.

Why does it need permissions?

Microphone permission lets BetterWispr record your voice. Accessibility permission allows automatic insertion into another app. Apple speech also requires Speech Recognition permission. You control these in System Settings.

What if automatic paste does not work?

BetterWispr checks the target app before inserting text. If it cannot safely paste, retrieve the transcript from the dashboard and copy it yourself. Secure fields and some apps may block automatic insertion.

Which languages are supported?

Language support depends on the selected model. Parakeet TDT v3 covers 25 European languages, Parakeet Japanese covers Japanese, and Whisper offers broader multilingual recognition. Apple speech depends on your locale and installed assets. Filler cleanup, styles and voice commands work in English. Test your chosen model with your own speech.

Does it rewrite everything I say?

Only as much as you choose. Auto cleanup starts at Light, which removes English filler words, repeats and false starts. Choose None to keep every word, or Medium to also edit English for clarity with the notes model you chose in Models. Apple Intelligence and Ollama do that on your Mac. Claude Code, Codex and API connections send the dictation to that provider, and only if you pick one. Styles adjust punctuation and capitalization for each kind of app. History keeps the raw transcript, and Use Original restores it.

Can it take meeting notes?

Yes. Notetaker transcribes your microphone as Me and, on macOS 14.2 or later, other apps’ audio as Them, while you write your own notes. Afterwards, Apple Intelligence on supported Macs running macOS 26 or a local Ollama model writes the summary, decisions and action items on your Mac. Claude Code, Codex and OpenAI compatible endpoints are optional and send the transcript to that provider. Without a notes model, the transcript and your notes are still saved.

Does it learn my words?

Yes. Add names and terms in Vocabulary, or fix a misheard word in History or right after it is pasted. BetterWispr adds the correction to Vocabulary and spells it your way next time. You can turn learning off in Settings.

Support the project

Free to use.\
Fueled by coffee.
---

BetterWispr has no subscription and no account. If it saves you some typing, you can support its development with a donation of any amount.

[Buy me a coffee](https://buymeacoffee.com/code_kartik)

Your next thought, in your own words

## Give your keyboard a break.

Local speech models. The apps you already use. A little more room to think.

GodelNumbering [on Hacker News](https://news.ycombinator.com/item?id=50046250)

> “Just tried it out as I was looking for something that does this exact thing well (macbook). So far I dictated about 5 different things in different contexts, it worked without a single error!”

macOS 14+ · Apple Silicon recommended · No account in the app
