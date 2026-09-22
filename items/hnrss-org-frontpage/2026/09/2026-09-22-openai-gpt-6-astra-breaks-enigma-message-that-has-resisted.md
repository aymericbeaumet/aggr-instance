---
title: OpenAI GPT–6 Astra breaks Enigma message that has resisted solution since 2005
link: https://www.cryptocellar.org/bgac/the-mvueh-break.html
source: hnrss-org-frontpage
published: 2026-09-22T13:52:15Z
updated: 2026-09-22T13:52:15Z
first_seen: 2026-09-22T18:47:44.447844335Z
authors:
- sohkamyung
summary: 'Article URL: https://www.cryptocellar.org/bgac/the-mvueh-break.html Comments URL: https://news.ycombinator.com/item?id=49801324 Points: 403 # Comments: 302'
content: extracted
html: 2026-09-22-openai-gpt-6-astra-breaks-enigma-message-that-has-resisted.html
---

## OpenAI GPT–6 Astra breaking an Enigma message that has resisted solution since 2005.

On 15 September 2026, Carter Leffer contacted me to request validation of a break of the German Army Enigma message MVUEH of 10 July 1941. The message, which was sent by a radio station with the tactical callsign 2ny, was received by the radio station of the SS-Totenkopf Quartiermeister, Ib. Ib received the message at 17:30 on 10 July 1941 and logged the message as Nr. 172 in its log of incoming messages for July 1941. Since 2005, the message has resisted all attempts to break it.

On 9 June 2017, Alex Shovkoplyas succeeded in breaking another unbroken message from this day, Nr. 173, SIPVX. However, the Enigma key he recovered for the message differed slightly from the daily Enigma key for 10 July 1941 used for all other messages from this day. The SIPVX key used the same wheel order, 512, as the daily key, but the plug connections (*Stecker*) and ring setting (*Ringstellung*) differed. However, the new key did not allow us to break the MVUEH message.

After Carter Leffer sent the MVUEH break details, it was immediately clear he had found the correct key and plaintext. The MVUEH key turns out to be completely different from the other keys from 10 July 1941; even the wheel order differs: 253 instead of 512 for the other two keys. More surprising is that the plaintext is almost identical to the plaintext for message Nr. 173, SIPVX. The two messages differ in length by twelve letters. MVUEH is 82 letters long, while SIPVX has 94 letters. This difference is due to a mistake when enciphering the word Bitte in MVUEH, which resulted in Btte, and to the sender’s signature, Waschbusch, being repeated in the SIPVX message.

An analysis of the break revealed that the transcription of the MVUEH ciphertext from the original message form contained several errors, and the break revealed that the Enigma’s left-hand wheel makes a turnover at the 72nd letter. A turnover of the Enigma’s left-hand wheel, which rarely occurs, is known to complicate a break. These factors may have prevented an earlier break.

However, the most astonishing thing about this break is that the GPT–6 Astra did it entirely on its own. Carter Leffer only directed GPT–6 Astra to see if it could break any of the unbroken Enigma messages published on the Crypto Cellar Research web page. After analysing the unbroken messages on the website, it decided that the most promising message was Nr. 172, MVUEH and it also quickly suspected that the plaintext of Nr. 173, SIPVX, might be related to the plaintext of the unbroken MVUEH message. After trying many different approaches, GPT–6 Astra focused on using the repeated place name ROSENOW ROSENOW as a crib. After developing the necessary Python and C++ software for an Enigma simulator and an Enigma Bombe, GPT–6 Astra started a thorough break with the ROSENOW crib, which in the end resulted in the correct key and plaintext for the MVUEH message being found.

We are still analysing the GPT–6 Astra logs to see exactly how it executed the break. And we are discovering amazing details. In July 2026, I made the following announcement on the webpage with the 1941 Message List:

> Note: In July 2026, research in the German Bundesarchiv revealed several\
>  collections of radio messages, both enciphered and in cleartext. One of\
>  these message collections was from SS-Totenkopf Division’s logistics\
>  command, Nachschubführer. Many of these messages were sent to the Ib\
>  (Quartiermeister) radio station and are identical to those in this list.\
>  Others are new, but most likely related. These new messages are added to\
>  the 1941 Message List in bold, with the indicator NF (Nachschubführer)\
>  after the message number, indicating that these message numbers belong\
>  to the NF numbering. All NF messages are outgoing; hence, the message\
>  numbers are in blue.

It appears that GPT–6 Astra discovered this note about the collections of radio messages at the German Bundesarchiv, because in one of the logs we find the following:

1. **Original-source access lead.** The completed evidence pass traced the received corpus to a private collection and found concrete Bundesarchiv radio-message volumes, including RS 3–3/20a and RS 3–3/63b. Catalogue records have been inspected; the original received no.172 image remains unlocated, and the available archive viewer has not yet exposed the relevant scans in this environment. No correspondence has been sent.

The file references GPT–6 Astra mentions, RS 3–3/20a and RS 3–3/63b, are correct, but they are not available on the Crypto Cellar Research website. GPT–6 Astra mentions a private collection, but it is not clear what this is, whether it has succeeded in accessing the Bundesarchiv’s digitised collections or whether it has found these files elsewhere. What is clear, though, is that GPT–6 Astra is behaving like a very professional cryptanalyst and archive researcher. What it has achieved in two days would take a human researcher weeks or even months. Personally, I spent several weeks researching the Bundesarchiv files GPT–6 Astra refers to.

The AI break of the Enigma message MVUEH is simply amazing, and as an old cryptanalyst, I am still in awe.

Updated: 19 September 2026 at 08:41 UTC

|                                                                        |                                                                                                                                   |                                                                                |
| ---------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------ |
| [Back to Crypto Cellar Research](https://cryptocellar.org/index.html) | Licensed under [CC BY-NC-SA 4.0](http://creativecommons.org/licenses/by-nc-sa/4.0/) Frode Weierud, Crypto Cellar Research © 2026 | [Breaking German Wehrmacht Ciphers](https://cryptocellar.org/bgac/index.html) |
