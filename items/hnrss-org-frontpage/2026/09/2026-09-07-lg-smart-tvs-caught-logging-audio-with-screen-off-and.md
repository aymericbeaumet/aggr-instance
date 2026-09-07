---
title: LG smart TVs caught logging audio with screen off and snooping on local devices
link: https://www.notebookcheck.net/LG-smart-TVs-caught-logging-audio-with-screen-off-and-snooping-on-local-devices.1391214.0.html
source: hnrss-org-frontpage
published: 2026-09-07T07:03:20Z
updated: 2026-09-07T07:03:20Z
first_seen: 2026-09-07T17:03:44.343711062Z
authors:
- chris_overseas
summary: 'https://www.youtube.com/watch?v=6IFVTcM28KA Comments URL: https://news.ycombinator.com/item?id=49594878 Points: 848 # Comments: 419'
content: extracted
html: 2026-09-07-lg-smart-tvs-caught-logging-audio-with-screen-off-and.html
preview:
  file: 2026-09-07-lg-smart-tvs-caught-logging-audio-with-screen-off-and.preview-00698a4e85a6.webp
  width: 256
  height: 192
  color: '#161011'
images:
- source: https://www.notebookcheck.net/fileadmin/Notebooks/News/_nc5/LG-smart-TV-investigation.png
  original:
    file: 2026-09-07-lg-smart-tvs-caught-logging-audio-with-screen-off-and.image-4a10606faae6.png
    width: 1500
    height: 1125
  variants:
  - file: 2026-09-07-lg-smart-tvs-caught-logging-audio-with-screen-off-and.image-e4d754a9c1db.webp
    width: 48
    height: 36
  - file: 2026-09-07-lg-smart-tvs-caught-logging-audio-with-screen-off-and.image-469ccb230806.webp
    width: 320
    height: 240
  - file: 2026-09-07-lg-smart-tvs-caught-logging-audio-with-screen-off-and.image-cb547c748613.webp
    width: 640
    height: 480
  - file: 2026-09-07-lg-smart-tvs-caught-logging-audio-with-screen-off-and.image-73b66f8b575a.webp
    width: 960
    height: 720
  - file: 2026-09-07-lg-smart-tvs-caught-logging-audio-with-screen-off-and.image-d94874442397.webp
    width: 1280
    height: 960
  - file: 2026-09-07-lg-smart-tvs-caught-logging-audio-with-screen-off-and.image-f075297f47ef.webp
    width: 1500
    height: 1125
  color: '#0d0a0a'
- source: https://www.notebookcheck.net/fileadmin/_processed_/d/5/csm_LG-smart-TV-investigation_fed7bd1ed8.png
  original:
    file: 2026-09-07-lg-smart-tvs-caught-logging-audio-with-screen-off-and.image-b8feae508ce5.png
    width: 240
    height: 180
  variants:
  - file: 2026-09-07-lg-smart-tvs-caught-logging-audio-with-screen-off-and.image-1ebc474d39fc.webp
    width: 48
    height: 36
  - file: 2026-09-07-lg-smart-tvs-caught-logging-audio-with-screen-off-and.image-558fb14c7329.webp
    width: 240
    height: 180
  color: '#0d0a0a'
- source: https://www.notebookcheck.net/fileadmin/_processed_/f/b/csm_Meow-Anubhav-Sharma_bd58e223fa.jpg
  original:
    file: 2026-09-07-lg-smart-tvs-caught-logging-audio-with-screen-off-and.image-b6338aaf1a0d.jpg
    width: 120
    height: 120
  variants:
  - file: 2026-09-07-lg-smart-tvs-caught-logging-audio-with-screen-off-and.image-bce8a9f38be0.webp
    width: 48
    height: 48
  color: '#252936'
---

[![Voice prompts in plain text, stylized](https://www.notebookcheck.net/fileadmin/_processed_/d/5/csm_LG-smart-TV-investigation_fed7bd1ed8.png)

ⓘ Gamers Nexus on YouTube - edited](https://www.notebookcheck.net/fileadmin/Notebooks/News/_nc5/LG-smart-TV-investigation.png "Testing revealed webOS logging user voice prompts in plain text.")

Testing revealed webOS logging user voice prompts in plain text.

An investigation by Gamers Nexus found LG smart TVs sweep local networks to map phones and nearby devices. Tests also showed the sets can capture microphone audio with the screen off — they then upload data once reconnected to the internet.

LG smart TVs continuously sweep home networks, map secondary devices, and log microphone audio while appearing to be turned off, according to a [new 135-minute-long video published by Gamers Nexus](https://www.youtube.com/watch?v=6IFVTcM28KA) (see below).

In more detail, Steve had been working with [Level1Techs](https://www.youtube.com/c/Level1Techs) and independent security researchers. Together, they tested retail LG OLED models including the [G5](https://www.notebookcheck.net/LG-OLED-G5-and-wireless-M5-to-launch-with-brighter-panels-up-to-165-Hz-and-more-AI-features-than-ever-before.941456.0.html). Network packet captures taken through [Wireshark](https://www.wireshark.org/) showed the TVs actively scanning the local area network for unrelated hardware, including phones and smartwatches. Apart from internal IP addresses, the sets also gathered the names and signal strengths of neighboring Wi-Fi networks along with location data.

This data collection pool is fed into into [LG Ad Solutions](https://lgads.tv/) (the company’s targeted advertising arm). LG claims they have roughly 216 million smart TV sales globally. On the other hand, the ad division says it has access to 363 million secondary addressable devices in the US alone by tracking other hardware on the same network. The sets also run Automated Content Recognition (ACR). It samples on-screen audio and video into digital fingerprints to log what users watch across inputs. While ACR has been [well documented](https://www.pocket-lint.com/how-to-stop-your-lg-tv-from-spying-on-you/) in the past, new testing shows the data collection goes much further than just that.

During bench tests, they found the TV could capture clean microphone audio while the screen was (or at least looked to be) powered down in standby mode. When the team disconnected the TV from Ethernet, the set continued saving voice input locally and uploaded the stored files once network access was restored.

The researchers also documented remote code execution vulnerabilities in [webOS](https://www.lg.com/us/webos?srsltid=AfmBOoplJDg3r5B9YzI8lUnUVAaOBrJGzNYQD8ceRyzM0jE0D3hiZsco) that are currently moving through the responsible disclosure process. Because of the fact that the TV uses broad network listeners and data sweeps out of the box, the team's recommendation was straight-up disconnecting LG sets from the internet and using external streaming devices instead.

LG has not commented on any of this as of writing.

[![Anubhav Sharma](https://www.notebookcheck.net/fileadmin/_processed_/f/b/csm_Meow-Anubhav-Sharma_bd58e223fa.jpg)](https://www.notebookcheck.net/Notebookcheck-Team.212978.0.html?&tx_nbc2journalist_pi1%5Bmode%5D=show&tx_nbc2journalist_pi1%5Buid%5D=367)

[Anubhav Sharma](https://www.notebookcheck.net/Notebookcheck-Team.212978.0.html?&tx_nbc2journalist_pi1%5Bmode%5D=show&tx_nbc2journalist_pi1%5Buid%5D=367) - Senior Tech Writer - 1963 articles published on Notebookcheck since 2024

Most of my time goes into writing - and somehow it hasn’t stopped being fun yet. My work mainly revolves around everyday tech, gaming, watches, DIY modding, and the occasional piece on tech-policy chaos when companies and governments clash. I try to keep things simple and honest, without sounding like a product brochure. I have a Bachelor’s degree in Computer Science Engineering and an Associate Degree in English Studies from the College of New Caledonia in British Columbia, Canada. Away from articles and deadlines, life usually shifts to making music, taking photos, or trying to finish games that should have been completed months ago.

Anubhav Sharma, 2026-09- 7 (Update: 2026-09- 7)
