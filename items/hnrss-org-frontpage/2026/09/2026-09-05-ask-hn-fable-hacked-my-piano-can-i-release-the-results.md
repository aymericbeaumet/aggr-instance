---
title: 'Ask HN: Fable hacked my piano, can I release the results?'
link: https://news.ycombinator.com/item?id=49577129
source: hnrss-org-frontpage
published: 2026-09-05T14:54:44Z
updated: 2026-09-05T14:54:44Z
first_seen: 2026-09-07T17:03:44.343711062Z
authors:
- jmpman
summary: 'I have a self playing piano, using a system called PianoDisc Protigy. They have an online store which sells music for their system, from various modern artists along with classics such as Bach and Beethoven. Last night I saw they had released some music from Eric Satre, a 19th century French composer, which I bought. Curious if I could have just used AI to create these files, I began experimenting with Astra and Fable. Feeding the output of one into the other to critique. After an hour of LLM discussion of Rubato and fermata, solenoid response times and proper sustain pedal technique, they settled on their ultimate version of Gymnopedie No 1. I then asked Fable to compare it to the open source version I''d downloaded from Mutopia, which it promptly ripped apart. No sustain, zero rubato, upside down balance. Ok, what about the version I''d just bought? The PianoDisc versions are mp3s encoded with the right channel carrying MIDI to be played on the piano, and the left channel containing any accompanying music to be played through attached speakers (who doesn''t want the harmonica on Piano Man?) I gave the mp3 to Fable, which promptly decoded the format, identifying the right channel carrying MIDI using a 2004.5 Hz square wave. It then went on to analyze the nuance of pedal lift and melody relative to the chords. Fable then asked if I wanted it to build an encoder to write my own MIDI files into the right channel of mp3s. Sounds great, and I instructed it to write the encoder. What it came back with was a python encoder PLUS a decoder. In the verbose explanation, it mentioned decoy notes. Curious, I asked it to explain the decoy notes. Apparently PianoDisc adds obfuscation into their format which is handled properly by their decoder, but would leave naively extracted MIDI unplayable on other systems. Fable created an encoder which adds those decoy notes, and a decoder which removes them. Am I allowed to publish the decoder? The encoder? Comments URL: https://news.ycombinator.com/item?id=49577129 Points: 234 # Comments: 137'
content: extracted
html: 2026-09-05-ask-hn-fable-hacked-my-piano-can-i-release-the-results.html
---

I have a self playing piano, using a system called PianoDisc Protigy. They have an online store which sells music for their system, from various modern artists along with classics such as Bach and Beethoven. Last night I saw they had released some music from Eric Satre, a 19th century French composer, which I bought. Curious if I could have just used AI to create these files, I began experimenting with Astra and Fable. Feeding the output of one into the other to critique. After an hour of LLM discussion of Rubato and fermata, solenoid response times and proper sustain pedal technique, they settled on their ultimate version of Gymnopedie No 1.

I then asked Fable to compare it to the open source version I'd downloaded from Mutopia, which it promptly ripped apart. No sustain, zero rubato, upside down balance.

Ok, what about the version I'd just bought?

The PianoDisc versions are mp3s encoded with the right channel carrying MIDI to be played on the piano, and the left channel containing any accompanying music to be played through attached speakers (who doesn't want the harmonica on Piano Man?)

I gave the mp3 to Fable, which promptly decoded the format, identifying the right channel carrying MIDI using a 2004.5 Hz square wave.

It then went on to analyze the nuance of pedal lift and melody relative to the chords.

Fable then asked if I wanted it to build an encoder to write my own MIDI files into the right channel of mp3s.

Sounds great, and I instructed it to write the encoder.

What it came back with was a python encoder PLUS a decoder.

In the verbose explanation, it mentioned decoy notes.

Curious, I asked it to explain the decoy notes.

Apparently PianoDisc adds obfuscation into their format which is handled properly by their decoder, but would leave naively extracted MIDI unplayable on other systems.

Fable created an encoder which adds those decoy notes, and a decoder which removes them.

Am I allowed to publish the decoder? The encoder?
