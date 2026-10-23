---
title: 'Show HN: OnKey – I''m a bad singer, so I built an app to show where I''m off-key'
link: https://github.com/dsmurrell/onkey
source: hnrss-org
published: 2026-10-22T14:04:44Z
updated: 2026-10-22T14:04:44Z
first_seen: 2026-10-23T14:22:19.033652984Z
authors:
- dsmurrell
summary: 'Hi all I was pretty terrible at singing, now I’d say I’m bad but not terrible. This is because I’ve been using a singing app (won’t name it here) that scores me on my pitch and timing throughout the song so that I can improve. Lately however, the app I’ve been using has flooded their song list with songs that aren’t originals. I didn’t like this at all as for me, it’s easier to try to improve when you’re singing along to the original. So, I asked an agent (Claude Code) if it could write some code to listen to Spotify and also listen to me and see if it could detect both pitches and score me. It impressed me by writing some Python and showing me my pitch in the terminal! It couldn’t go all the way though and detect the singer’s pitch in real time. So I found the next best setup and built OnKey, a Mac app that needs to listen to the song once (it doesn’t save it) and then when it’s learnt the song (takes a couple of minutes), you can sing along to the pitch track that it’s learnt. OnKey is free to use and open source under the MIT license. It can listen to Spotify, Apple Music, and YouTube playing in your browser with the right permissions. During the build I found that someone had done something similar in Nightingale, but this requires a library of songs exported to disk and who has this in 2026 (maybe some of you ;) - but we need to help the normies too). You can check out a demo video on the site of me singing (warning given) here: https://singonkey.app The app sends out to find the song info (short Shazam fingerprint, and looks up song details), lyrics (from LRCLIB), and checks for updates. The app never sends the music (it doesn’t save this to disk) and your voice. Everything you’d expect to stay private does. It’s still weak in a few places: - when the song’s backing is noisy it’s hard to pick the pitch track out - sometimes it gets a note an octave off - it’s limited to Apple Silicon Macs, M1 or later, and only macOS 14.2 up because extra drivers aren’t required past this point - the scoring is built for beginners like me so it’s fairly easy on you Would love for people to try it out and let me know what works and what doesn’t for them! Comments URL: https://news.ycombinator.com/item?id=50195488 Points: 104 # Comments: 51'
content: extracted
html: 2026-10-22-show-hn-onkey-i-m-a-bad-singer-so-i-built-an-app-to-show.html
remote_preview:
  url: https://repository-images.githubusercontent.com/1424878564/24f58d68-d92e-493a-8534-6564902cb5a4
  alt: Learn to sing the songs you love. A free, open-source singing trainer for Mac that listens to the music you play. - dsmurrell/onkey
---

**Learn to sing the songs you love.** · [singonkey.app](https://singonkey.app)

*Saving shower singers from themselves since 2026.*

[![OnKey's singing lane: note bars scroll towards the playhead and the singer's pitch is drawn over them, green where it's on the note](https://camo.githubusercontent.com/e76e6ba9648be3d0b5f61a0469a06ef25621dc69f3d0f91fd9581b77a06307ed/68747470733a2f2f6d656469612e73696e676f6e6b65792e6170702f6f6e6b65792d6c616e652d322e676966)](https://singonkey.app) \
Me singing along to a song OnKey learned from one play. [The full demo, with sound](https://singonkey.app).

OnKey is a free, open-source singing trainer for the Mac. Play a song in Spotify, Apple Music, or YouTube in a browser, and sing along. The first time, you get the lyrics, and OnKey learns the singer's melody when the song ends. After that, the notes scroll towards you, your pitch is drawn over them, and you get a score with a report on every line.

OnKey doesn't provide, stream or download music. Your own music app plays it.

## Install

You need a Mac with Apple Silicon (M1 or later) and macOS 14.2 or later.

[Download `OnKey.dmg`](https://github.com/dsmurrell/onkey/releases/latest/download/OnKey.dmg) (from the [latest release](https://github.com/dsmurrell/onkey/releases/latest)), open it and drag OnKey to Applications.

Setup asks for the microphone, to hear you, and for system audio recording, to hear the music app. It also asks whether OnKey may follow Spotify and Apple Music: choose **Listen only** and it follows songs by their sound alone. [Getting started](https://singonkey.app/docs/) walks through it, and the [FAQ](https://singonkey.app/docs/faq/) answers the usual questions.

## How it works

When a song ends, OnKey learns it on your Mac: a model separates the lead vocal from the music, CREPE tracks the singer's pitch, and the GAME note model turns it into one bar per syllable. On later plays it finds its place from the music itself and tracks your voice live. Singing an octave down counts, and being a touch late isn't punished. [How it works](https://singonkey.app/docs/how-it-works/) and [Scoring](https://singonkey.app/docs/scoring/) have the details.

## Privacy

OnKey never saves the music it listens to. It analyses the sound in memory as it plays. Your recordings and the songs it learns stay on your Mac (a take sung over speakers can contain some of the music). There's no account, no analytics and no crash reporting.

These requests do go out:

- Shazam (Apple): a short signature of the music app's sound, not the audio and never your mic, to name a song OnKey hasn't learned when the music app doesn't say what's playing.
- LRCLIB: the song's artist, title, album and length, to fetch lyrics.
- iTunes Search and Lookup (Apple): an artist and title, or the ID Shazam returned, for covers and the song's length. The cover images come from Spotify's, Apple's or YouTube's image hosts.
- GitHub: a check for a new version a minute after launch and once a day, and its download.

Settings has a switch for Shazam, for lyrics and for the update check. [Privacy](https://singonkey.app/docs/privacy/) lists everything OnKey keeps and sends, and [scripts/release-hosts.txt](https://github.com/dsmurrell/onkey/blob/main/scripts/release-hosts.txt) lists every web address in a release.

## Using OnKey fairly

OnKey is for practising at home, with music you're entitled to play. Please:

- Keep what it learns to yourself. Its transcription of each song's melody is for your practice, so don't share or publish it.
- To share a take, use **Share** on its page. It makes a short video of your voice and your pitch line, with no music, and you can add the song from Instagram's own music library, which is licensed for this. Headphone takes are best: sung on speakers, some of the song is in your recording. Don't post screen recordings with the music playing.
- Use a licensed karaoke service for a bar or an event. OnKey's lyrics come from a free community database.
- Don't modify OnKey to save the music.

Rights holders can reach me at [hello@singonkey.app](mailto:hello@singonkey.app) ([LEGAL.md](https://github.com/dsmurrell/onkey/blob/main/LEGAL.md)).

## Help

Use **Report a problem** in the app (Settings, the Help menu, or a take's page). It saves a zip of numbers and logs on your Mac, with no audio and no recordings of you, for you to attach to an [issue](https://github.com/dsmurrell/onkey/issues) if you want to. [Troubleshooting](https://singonkey.app/docs/troubleshooting/) covers the common problems. Questions, or a song that's out of step? Ask in [OnKey's Discord](https://discord.gg/afHvqTNS6r), or email [hello@singonkey.app](mailto:hello@singonkey.app).

## Building from source

If you'd rather not trust my build, you can build your own. You need an Apple Silicon Mac with the Xcode command line tools, Rust, Node, and Python 3:

```shell
git clone https://github.com/dsmurrell/onkey && cd onkey
npm install && npm --prefix ui install
python3 -m venv .venv && .venv/bin/pip install -r scripts/requirements-models.txt
PYTHON=.venv/bin/python scripts/fetch-models.sh   # downloads the models and checks them by SHA-256
npx tauri build
```

Your build runs on your own Mac. The one thing it can't do is ask Shazam to name a new song, which needs my Apple signature. Spotify and Apple Music say what's playing themselves, so that only affects YouTube and Listen only. [CONTRIBUTING.md](https://github.com/dsmurrell/onkey/blob/main/CONTRIBUTING.md) has the details, the tests, and how to check a release's checksums. My releases are signed with my Apple Developer ID and notarised by Apple (`spctl -a -vv /Applications/OnKey.app` shows who signed it).

## Licence and credits

MIT. See [LICENSE](https://github.com/dsmurrell/onkey/blob/main/LICENSE). Modified versions can't be called OnKey or use its icon. If you build on OnKey's code, a credit linking back here is appreciated.

OnKey learns songs with CREPE, GAME, wav2vec 2.0 and UVR's MDX-Net Karaoke 2. The [credits](https://singonkey.app/docs/credits/) list each one, and [THIRD\_PARTY\_NOTICES.md](https://github.com/dsmurrell/onkey/blob/main/THIRD_PARTY_NOTICES.md) has the full notices and licence texts.

[![](https://github.com/dsmurrell/onkey/raw/main/site/public/dsmurrell.png)](https://github.com/dsmurrell/onkey/blob/main/site/public/dsmurrell.png) OnKey is made by [Daniel Murrell](https://dsmurrell.com).
