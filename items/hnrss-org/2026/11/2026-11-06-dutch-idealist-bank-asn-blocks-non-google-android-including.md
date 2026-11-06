---
title: Dutch 'idealist' bank ASN blocks non-Google Android, including GrapheneOS
link: https://github.com/PrivSec-dev/banking-apps-compat-report/issues/105
source: hnrss-org
published: 2026-11-06T09:27:12Z
updated: 2026-11-06T09:27:12Z
first_seen: 2026-11-06T18:21:10.909146084Z
authors:
- microtonal
content: extracted
html: 2026-11-06-dutch-idealist-bank-asn-blocks-non-google-android-including.html
remote_preview:
  url: https://opengraph.githubassets.com/59dee268de1876a4d484cfbf630dc45b1c310f39f9783a1eab71556777dbd200/PrivSec-dev/banking-apps-compat-report/issues/105
  alt: Is there an existing issue for this? I have searched the existing issues App name ASN Bank Link to app https://play.google.com/store/apps/details?id=nl.devolksbank.asn.bankieren App version 2.40.2 ...
---

Worth reading the linked GitHub issue to the end: yesterday someone asked EPI (the Wero operator) directly, and the reply says hardware key attestation "proves a key was generated in a TEE or StrongBox but not app or OS integrity", which is why they want Play Integrity. That claim is wrong, and the gap matters for this whole thread.

I'm on the other side of this as a developer: a small Android app where I need some assurance that a capture came from an unmodified device, so I run both Play Integrity and the Keystore key attestation API.

What the attestation certificate actually carries, per the Android spec: the challenge you supplied, the security level (software / TEE / StrongBox), a RootOfTrust extension with verifiedBootState, the verifiedBootKey hash and deviceLocked, and an attestationApplicationId with the package name and signing certificate digest of the app that created the key. That is OS integrity (verified boot, locked bootloader) and app identity, signed by the chip, chained to Google's published roots. GrapheneOS publishes its verified boot key hashes as a signed JSON and documents how to accept them. So "a list of signing keys to be trusted" is not hypothetical, it is the documented path, and EPI's stated reason doesn't hold.

Play Integrity, by contrast, is one SDK call plus one server call to Google, and you get back an array like \["MEETS\_DEVICE\_INTEGRITY"\]. Whether GrapheneOS is in or out is decided on Google's side; nothing in the token tells you why. In my data (64 verdicts from beta testers' phones) I have never once seen MEETS\_STRONG\_INTEGRITY, so in practice everyone is checking the weaker level anyway.

The honest cost of the attestation path: you parse X.509 and the ASN.1 extension yourself and you own the allowlist. My first validator unwrapped the challenge wrong and flagged 13 of my 19 stored keys as mismatched, and choked on DER lengths for 2 more. Once fixed, every real phone chained to one of Google's two roots (7 TEE, 4 StrongBox), and the only software-attested keys came from Google's own Play reviewer, who tests apps in an emulator. Today my server trusts the chain for what the key is, doesn't read RootOfTrust yet, and still gates publishing on the Play verdict, so my app would block GrapheneOS too. Not for security: because the easy check was done first, and "Google said so" is easier to defend in front of an auditor than "I maintain a list of key hashes".
