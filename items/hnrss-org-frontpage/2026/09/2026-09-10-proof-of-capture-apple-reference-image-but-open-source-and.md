---
title: 'Proof of Capture: Apple Reference Image, but open source and using steganography'
link: https://merybenavente.me/blog/proof-of-capture
source: hnrss-org-frontpage
published: 2026-09-10T19:44:15Z
updated: 2026-09-10T19:44:15Z
first_seen: 2026-09-11T09:36:38.271580262Z
authors:
- merybenavente
summary: 'Article URL: https://merybenavente.me/blog/proof-of-capture Comments URL: https://news.ycombinator.com/item?id=49649222 Points: 106 # Comments: 58'
content: extracted
html: 2026-09-10-proof-of-capture-apple-reference-image-but-open-source-and.html
preview:
  file: 2026-09-10-proof-of-capture-apple-reference-image-but-open-source-and.preview-91b91d030c42.webp
  width: 256
  height: 247
  alt: Hand holding the display PCB over the 3D-printed camera enclosure with a Raspberry Pi Zero inside
  color: '#627f78'
images:
- source: https://merybenavente.me/blog/proof-of-capture/camera-enclosure.png
  original:
    file: 2026-09-10-proof-of-capture-apple-reference-image-but-open-source-and.image-e6925d9cbd75.png
    width: 1420
    height: 1370
  color: '#4baa6b'
- source: https://merybenavente.me/blog/proof-of-capture/lsb-diagram-v2.png
  original:
    file: 2026-09-10-proof-of-capture-apple-reference-image-but-open-source-and.image-9e56221192d4.png
    width: 1600
    height: 700
  variants:
  - file: 2026-09-10-proof-of-capture-apple-reference-image-but-open-source-and.image-decbed888386.webp
    width: 48
    height: 21
  color: '#faf7f0'
- source: https://merybenavente.me/blog/proof-of-capture/atecc608-board.png
  original:
    file: 2026-09-10-proof-of-capture-apple-reference-image-but-open-source-and.image-61355fdb337e.png
    width: 1414
    height: 1216
  color: '#564535'
- source: https://merybenavente.me/blog/proof-of-capture/soldering.jpg
  original:
    file: 2026-09-10-proof-of-capture-apple-reference-image-but-open-source-and.image-bcb3da2ba6d2.jpg
    width: 1500
    height: 2000
  variants:
  - file: 2026-09-10-proof-of-capture-apple-reference-image-but-open-source-and.image-8f128bef3a56.webp
    width: 48
    height: 64
  color: '#e3d5c5'
- source: https://merybenavente.me/blog/proof-of-capture/atecc608-hand.jpg
  original:
    file: 2026-09-10-proof-of-capture-apple-reference-image-but-open-source-and.image-fd3a9833fe5c.jpg
    width: 1500
    height: 2000
  variants:
  - file: 2026-09-10-proof-of-capture-apple-reference-image-but-open-source-and.image-15f8dc21b63e.webp
    width: 48
    height: 64
  color: '#eceae4'
- source: https://merybenavente.me/blog/proof-of-capture/apple-reference-image.webp
  original:
    file: 2026-09-10-proof-of-capture-apple-reference-image-but-open-source-and.image-06ae8f890b79.webp
    width: 1960
    height: 1307
  variants:
  - file: 2026-09-10-proof-of-capture-apple-reference-image-but-open-source-and.image-e03ab3d89f9b.webp
    width: 48
    height: 32
  - file: 2026-09-10-proof-of-capture-apple-reference-image-but-open-source-and.image-614f0d2affe5.webp
    width: 320
    height: 213
  color: '#030202'
- source: https://merybenavente.me/blog/proof-of-capture/printed-photo-v3.jpg
  original:
    file: 2026-09-10-proof-of-capture-apple-reference-image-but-open-source-and.image-c6e2bfca1995.jpg
    width: 1500
    height: 1380
  variants:
  - file: 2026-09-10-proof-of-capture-apple-reference-image-but-open-source-and.image-6e1e862636ee.webp
    width: 48
    height: 44
  color: '#070504'
---

Apple introduced yesterday **Apple Reference Image**: a way to cryptographically prove a photo was actually taken by a camera, instead of AI generated. During my time at the [Recurse Center](https://www.recurse.com/) this summer, [Alex Hornstein](https://d-lab.mit.edu/about/people/alex-hornstein) and I (two camera lovers) **built a camera with [proof of capture](https://github.com/merybenavente/proof_of_capture)**.

![Hand holding the display PCB over the 3D-printed camera enclosure with a Raspberry Pi Zero inside](https://merybenavente.me/blog/proof-of-capture/camera-enclosure.png)

The camera: a Raspberry Pi Zero, a display board, an ATECC608 crypto chip, a shutter button, and a 3D-printed enclosure.

## Prove what’s real at capture time

Back in 2019 I was deploying ML fact-checking tools, and even in the Will-Smith-eating-spaghetti era it was obvious that **generators outrun detectors**. Detection is a losing race: every improvement in the detector is training signal for the next generator. Our approach flips the problem: instead of trying to detect what's fake after the fact, **prove what's real at the moment of capture**.

The complexity of this is not at the technical level but in how to handle **photo edits** (should a cropped photo keep its signature?) and **metadata**: the moment you share an image, the EXIF gets stripped for privacy reasons, and any signature stored there is gone.

## Steganography and perceptual hashes

For our camera we used **steganography**: an invisible watermark hidden in the image pixels themselves, containing a signed **perceptual hash** (a hash of what the photo *looks like*, not its exact bytes). Because nothing lives in the metadata, the signature **survives compression and resizing**. Our first version hid an exact SHA-256 hash in the last bit of each pixel, and any JPEG recompression destroyed it; the current one signs a **pHash** and spreads it across the whole image as a frequency-domain watermark (**DWT + DCT**), which survives WhatsApp-grade compression and still detects content edits.

![Diagram: a photo’s pixel color channels feed a crypto chip, which produces a signature string embedded into the least significant bits of the image](https://merybenavente.me/blog/proof-of-capture/lsb-diagram-v2.png)

Simplified illustration: LSB replacement is shown for clarity, the actual embedding spreads the signature across a DWT + DCT watermark.

## The ATECC608 chip

The signing is handled by an **ATECC608** cryptographic chip (self-soldered!) that holds a public key for verifying and a private one for signing. Once minted, **the private key never leaves the chip**: not even the owner can read it. And if you tamper with it, the chip locks itself.

![ATECC608 breakout board with a padlock silkscreen around the chip](https://merybenavente.me/blog/proof-of-capture/atecc608-board.png)![ATECC608 breakout held by a helping-hands clip at the soldering station](https://merybenavente.me/blog/proof-of-capture/soldering.jpg)![Hand holding the soldered ATECC608 breakout board](https://merybenavente.me/blog/proof-of-capture/atecc608-hand.jpg)

The ATECC608 secure element.

## What Apple is doing

Apple does something similar: the sensor **signs every pixel at capture time**. But instead of embedding the signature into the image, Private Cloud Compute develops it into a "digital negative" that lives next to your photo. Their verification flow is neither public nor clear yet.

![Three iPhone screens showing Apple’s Reference Image flow: the photo, the authenticated reference image, and the comparison view](https://merybenavente.me/blog/proof-of-capture/apple-reference-image.webp)

Apple Reference Image: compare the authenticated capture against the photo to see if it was altered. [\[source\]](https://www.apple.com/newsroom/)

Something I don't like is that they're not using the existing open standard, **C2PA**, already used by Nikon, Sony, Leica and Adobe. And even though they're opening APIs so platforms can verify natively, **the root of trust stays inside Apple's Private Cloud Compute**. Apple... (sigh)

## To be fair...

Neither Proof of Capture, Apple Reference Image nor C2PA fully solve the problem. For example, a **screen attack** still works: photograph a screen displaying an AI image and you get a signed photo of a fake. But it's always nice seeing big actors interested in addressing this problem. The project is [open source](https://github.com/merybenavente/proof_of_capture). You can build your own for **under $100**. Closing with a printed photo from our Proof of Capture camera. Visit the [Recurse Center](https://www.recurse.com/) to see this piece of art. If you look closely enough you may see the hidden signature.

![Printed photo from the Proof of Capture camera taped to a workstation at the Recurse Center](https://merybenavente.me/blog/proof-of-capture/printed-photo-v3.jpg)

"Trust me, this photo is 100% authentic. Signed."
