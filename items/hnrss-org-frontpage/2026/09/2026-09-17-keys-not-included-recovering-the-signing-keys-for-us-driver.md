---
title: 'Keys Not Included: recovering the signing keys for US driver''s license barcodes'
link: https://ryan.science/blog/keys-not-included
source: hnrss-org-frontpage
published: 2026-09-17T03:03:23Z
updated: 2026-09-17T03:03:23Z
first_seen: 2026-09-17T07:35:27.698192978Z
authors:
- Ryan5453
summary: 'Article URL: https://ryan.science/blog/keys-not-included Comments URL: https://news.ycombinator.com/item?id=49735930 Points: 135 # Comments: 47'
content: extracted
html: 2026-09-17-keys-not-included-recovering-the-signing-keys-for-us-driver.html
preview:
  file: 2026-09-17-keys-not-included-recovering-the-signing-keys-for-us-driver.preview-9276e2a10fdf.webp
  width: 256
  height: 243
  color: '#60666a'
images:
- source: https://ryan.science/tessa.png
  original:
    file: 2026-09-17-keys-not-included-recovering-the-signing-keys-for-us-driver.image-79877f89e6c6.png
    width: 320
    height: 304
  color: '#98caf7'
---

In Insecure by Design I argued that the AAMVA should require states to cryptographically sign the PDF417 barcode on a driver's license and mentioned that two states, New York and Virginia, already sign theirs with something proprietary. A Mobile Driver's License (mDL) or a passport follows a public standard, so anyone can check one but a New York or Virginia license can only be checked by the DMV that issued it.

Virginia published a calibration sheet that lays out the AAMVA format of the barcode including the state's own subfiles. It names a "ninety-character security object" and stops there. But then California did exactly what I asked: [Verifying Digital Signatures on California DL/ID Documents](https://www.dmv.ca.gov/portal/file/verifying-digital-signatures-on-california-dlid-documents-pdf/) explains exactly how to use the DMV's public key to check whether a California ID is real.

* * *

On the back of any U.S. state ID or DL is a standard AAMVA PDF417 barcode. Most of it is exactly the documented, plaintext data element structure I described in Insecure by Design: three-letter codes and their values, DCS/DAC/DAD for the name, DBB/DBA/DBD for the dates of birth, expiry, and issue, DAQ for the license number, DCF for the document discriminator, and so on. The header carries an Issuer Identification Number that says which jurisdiction printed the card: 636000 is Virginia, 636001 is New York, 636004 is North Carolina, and 636014 is California.

The interesting part is the jurisdiction-specific subfiles, a section prefixed with `Z` plus a state letter (`ZV` in Virginia, `ZN` in New York, `ZC` in California). AAMVA reserves these subfiles for whatever a state wants to add, and they stay fully standards-compliant because a generic scanner reading the mandatory subfile simply skips over them.

On October 1st, 2025, the California DMV announced its new license and ID design in a press release. Most of it is the usual catalogue of physical security features, and then there is this [1](https://ryan.science/blog/keys-not-included#ref-canews):

> "The DMV will add a digital security signature to one of the two barcodes on the back of the cards. California will be one of the first states to add this kind of digital signature."

California does not produce their IDs in house - they contract it out to a third-party vendor called IDEMIA, who produces ~61% of IDs/DLs in the U.S. (by jurisdiction, not physical quantity). IDEMIA was likely the one who integrated this digital signature system for CA, and they did an incredible job, though the design credit goes to the W3C spec they used, written largely at Digital Bazaar.

Inside California's `ZC` subfile is a complete [W3C Verifiable Credential Barcode](https://w3c.github.io/vc-barcodes/), a credential compressed with CBOR-LD and signed with the `ecdsa-xi-2023` cryptosuite [2](https://ryan.science/blog/keys-not-included#ref-cadmv). What gets signed is spelled out: a bitstring selects which AAMVA fields are covered, they are formatted as code-plus-value, joined with newlines, sorted, and hashed. The credential points at `did:web:credentials.dmv.ca.gov`, which resolves to a plain JSON document at a well-known URL on the DMV's own domain [3](https://ryan.science/blog/keys-not-included#ref-cadid). The URL leads to the barcode-signing public key, labeled `#vm-vcb-1`, a normal P-256 public key sitting there for anyone to download. California even ships an open-source verifier with real valid and revoked test barcodes, so anyone can trivially run the verification process themselves.

After California announced they included a signature in their barcodes it wasn't until the following April that they released the documentation on what signs what, which fields are covered, what a verifier should reconstruct, or where the key lives. For roughly six months, California was in precisely the position that New York and Virginia are in now: a real cryptographic signature, on a real card, in circulation, publicly announced, and no way for anyone outside the DMV to check it. Or so I thought.

* * *

Canadian Bank Note (CBN) is a (unsurprisingly) Canadian company that manufactures DL/IDs for five U.S. states: New York, Virginia, North Carolina, South Carolina, and Wisconsin. I discovered that CBN actually implements digital signatures for all five states, not just NY and VA. I don't have South Carolina or Wisconsin barcodes myself - people who do decoded theirs and confirmed the field is there. But again, this signature is largely useless since there (was) no way to actually verify it.

Inside CBN's security feature is a string that is [Ascii85](https://en.wikipedia.org/wiki/Ascii85) encoded. Strip that encoding away and you are left with a short binary blob, and that blob is a [DER](https://en.wikipedia.org/wiki/X.690#DER_encoding) structure: a `SEQUENCE` containing two `INTEGER`s, each 256 bits wide. A SEQUENCE of two 256-bit integers named *r* and *s* is the canonical on-the-wire encoding of an ECDSA signature over the P-256 curve (also called secp256r1).

These blobs come out to 70, 71, or 72 bytes depending on the card, which is exactly what ECDSA produces, because *r* and *s* are effectively random integers and DER drops or adds a leading byte depending on whether the high bit of each happens to be set. A fixed-length hash or a counter would not vary like that, but a pair of random 256-bit integers does. Across three real New York cards, two North Carolina cards, and six Virginia samples, every single one fit this structure.

ECDSA has a specific cryptographic property that given a signature and the message it signed, you can mathematically recover the public key that produced it [4](https://ryan.science/blog/keys-not-included#ref-sec1). Each signature yields a small set of candidate keys rather than one, but if you have several signatures from the same signer, the real key is the single candidate they all share.

However, to recover this key, you need the exact signed message, which is entirely undocumented. I tried bruteforcing hundreds of thousands of possible orderings and formats but eventually figured out (with Claude's help) that unlike California's implementation, all of it gets signed. The signature field itself is part of the signed message. Before signing, the encoder fills the field with a placeholder (`0`), repeated for the field's exact length - signs the entire payload including that placeholder, and then writes the real signature over the top of it. To verify, you put the placeholder back.

On a New York card the payload is 484 bytes and the `ZNB` value sits at bytes 393 to 483. To check it:

1. Take the payload exactly as decoded, byte for byte.
2. Overwrite the signature value in place with `0` repeated to the same length: `payload[:393] + b"0" * 90 + payload[483:]`.
3. SHA-256 the result.
4. Verify the DER signature against that digest with ECDSA P-256.

* * *

Three real New York cards produce one key that all three pairs agree on. Six Virginia samples produce one key that all fifteen pairs agree on. Two North Carolina cards produce one key that both agree on, which is the minimum it takes.

That leaves South Carolina and Wisconsin. If you have two cards from either, you can recover its key yourself here.

New York IIN 636001 field ZNB 02851d63a281796be0ca11189f03028abf80e032838f83215889b9e708eac16482 Virginia IIN 636000 field ZVA 02d0f2823d63c854566c5da2cb07e114dbad16f874c2422f74806fe3e2f4775f1d North Carolina IIN 636004 field ZND 0398956191b3fd2704362fdd8500dc0d4419c9379c5187cefd223dd4ee566da9e8

All three are P-256 public keys in compressed form, and every card I have access to verifies against them. Change even a single byte of a surname and it is instantly able to be detected as inauthentic. These are public keys, which are meant to be published - recovering one lets anyone check a signature, not forge one.

I built a little demo to check the signatures across California, New York, Virginia, and North Carolina: take a picture of the barcode and check it here. It decodes the PDF417 barcode, parses the AAMVA format for any jurisdiction, and verifies the signature for the four I have a key for. Everything runs in your browser so I will never see any image or extracted data from your ID. The keys are per-jurisdiction and the construction is shared across the vendor's states. Virginia's signatures fail under New York's key and vice versa, which is the right design, because it means one state's compromise doesn't take the others with it. A valid signature only proves the state issued that data. The photo is not signed (not included in the barcode at all), so a genuine barcode copied onto a counterfeit still passes.

I ran my finished verifier against a counterfeit New York sample: to a blind eye its barcode is a pretty good clone of a real New York card. Same 484-byte payload length, same subfile directory, same field widths and padding, same card-revision date. More importantly, the `ZNB` field is not empty and not garbage: it contains a well-formed 71-byte DER ECDSA signature, correctly Ascii85-encoded, with the right prefix and a plausible length. But it fails the cryptographic check instantly, because it was signed with a throwaway key, not the state's.

States do not design their own barcodes. They run procurements, and a very small number of companies build what gets printed. You can view what vendor produces IDs for what state here. Canadian Bank Note produces the cards for five: New York, North Carolina, South Carolina, Virginia and Wisconsin. IDEMIA produces them for thirty-one of the fifty-one US jurisdictions.

California's cards are made by IDEMIA, under a twelve-year contract awarded in 2022, and IDEMIA began issuing all of California's licenses and IDs in October 2025 [5](https://ryan.science/blog/keys-not-included#ref-idemia-ca). The Verifiable Credential Barcode is part of the card design IDEMIA built with the California DMV. So the open, documented, publicly-verifiable implementation is not some California-specific civic virtue. It is a product one vendor has already built, shipped, and operates at the scale of the largest state in the country.

The issue is that they never shipped this anywhere else. Of the thirty-one jurisdictions IDEMIA produces cards for, **exactly one** has any public evidence of a verifiable signed barcode. Whatever this feature is, it is not spreading, and the constraint plainly isn't engineering, because the engineering is finished and running in the largest of those thirty-one.

In August 2025 IDEMIA launched a redesigned driver's license for Texas, and their announcement lists the security features: tamper-resistant polycarbonate, laser engraving, tactile surfaces, a Texas-shaped optical variable window, a laser-engraved star for REAL ID compliance [6](https://ryan.science/blog/keys-not-included#ref-idemia-tx). Every one of those is a physical anti-counterfeiting measure, verified by a human squinting at a card. There is no mention of a digital signature.

* * *

**IDEMIA has already solved this and should ship it everywhere.** The engineering is done, the standard is published, the trust infrastructure is running in production for California. Every subsequent card program that launches without it is a choice, and it is a strange one, because the feature is finished and paid for. A verifiable barcode should be the default line item in every contract they sign, not a premium a state has to know to ask for.

**Canadian Bank Note should document what they already built.** This is the cheaper ask by an enormous margin, because CBN does not need to build anything at all. All that is missing is a page on a website: here is the signed-message construction, and here is each state's public key. That is a weekend of documentation standing between a security feature that protects nobody and one that protects everyone who scans a card.

Which loops back to where Insecure by Design ended. I asked whether the barcode would ever get a real signature. The answer, in these states, was yes, and it did not matter, because a signature is a public act or it is nothing. California, through the same vendor that serves much of the country, quietly demonstrated the whole thing done correctly, on the same math, on the document most Americans actually carry, and published the keys to prove it. The technology was never the obstacle. It was finished, sitting in a product catalog. The willingness to be verified was the obstacle, and it still is.

* * *

## References

1. California DMV. (2025, October 1). *DMV to Release New California Driver's License and Identification Card Design with Advanced Security Features*. [https://www.dmv.ca.gov/portal/news-and-media/dmv-to-release-new-california-drivers-license-and-identification-card-design-with-advanced-security-features/](https://www.dmv.ca.gov/portal/news-and-media/dmv-to-release-new-california-drivers-license-and-identification-card-design-with-advanced-security-features/)
2. California DMV. (2026, April). *Verifying Digital Signatures on California DL/ID Documents: Technical Overview*. (PDF metadata: created 7 April 2026, modified 9 April 2026.) [https://www.dmv.ca.gov/portal/file/verifying-digital-signatures-on-california-dlid-documents-pdf/](https://www.dmv.ca.gov/portal/file/verifying-digital-signatures-on-california-dlid-documents-pdf/)
3. California DMV. (n.d.). *DID document for credentials.dmv.ca.gov*. [https://credentials.dmv.ca.gov/.well-known/did.json](https://credentials.dmv.ca.gov/.well-known/did.json)
4. Certicom Research. (2009). *SEC 1: Elliptic Curve Cryptography* (Version 2.0), §4.1.6, Public Key Recovery Operation. [https://www.secg.org/sec1-v2.pdf](https://www.secg.org/sec1-v2.pdf)
5. IDEMIA. (2025, October 10). *IDEMIA Public Security to Launch New State Driver's Licenses and IDs with Innovative Design for California in Partnership with the California Department of Motor Vehicles*. [https://www.idemia.com/press-release/idemia-public-security-launch-new-state-drivers-licenses-and-ids-innovative-design-california-partnership-california-department-motor-vehicles-2025-10-10](https://www.idemia.com/press-release/idemia-public-security-launch-new-state-drivers-licenses-and-ids-innovative-design-california-partnership-california-department-motor-vehicles-2025-10-10)
6. IDEMIA. (2025, August 28). *IDEMIA Public Security to Launch New State Driver's Licenses and IDs with Enhanced Security Features for Texas in Partnership with the Texas Department of Public Safety*. [https://www.idemia.com/press-release/idemia-public-security-launch-new-state-drivers-licenses-and-ids-enhanced-security-features-texas-partnership-texas-department-public-safety-2025-08-28](https://www.idemia.com/press-release/idemia-public-security-launch-new-state-drivers-licenses-and-ids-enhanced-security-features-texas-partnership-texas-department-public-safety-2025-08-28)
