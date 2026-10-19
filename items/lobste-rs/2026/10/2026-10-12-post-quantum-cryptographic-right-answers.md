---
title: (Post-Quantum) Cryptographic Right Answers
link: https://words.filippo.io/right-answers/
source: lobste-rs
published: 2026-10-12T13:44:38Z
updated: 2026-10-12T13:44:38Z
first_seen: 2026-10-13T11:00:50.474439149Z
authors:
- words.filippo.io via jtdowney
labels:
- cryptography
summary: Comments
content: extracted
html: 2026-10-12-post-quantum-cryptographic-right-answers.html
---

Publishing “Cryptographic Right Answers” for developers is a tradition by now. [Colin Percival started in 2009](https://www.daemonology.net/blog/2009-06-11-cryptographic-right-answers.html), [Thomas Ptacek followed up in 2015](https://gist.github.com/tqbf/be58d2d39690c3b366ad), and then [Latacora carried on in 2018](https://www.latacora.com/blog/cryptographic-right-answers/) and [2024](https://www.latacora.com/blog/post-quantum-cryptographic-right-answers/#pqc-right-answers).

It might feel too soon for an update, but *so much has changed*. The timeline of the post-quantum transition [dramatically shifted in early 2026](https://words.filippo.io/crqc-timeline/), and there is now consensus that it needs to complete by 2029. 2029! We have three years!

The good news is that, despite what you might have heard, [symmetric cryptography is going to be fine, because Grover does not matter](https://words.filippo.io/128-bits/). The bad news is that all asymmetric answers changed compared to 2018 and most even compared to 2024.

The answers below assume you are not constrained by any specific compliance regime, although they are mostly compatible with FIPS 140.[^1] If you need to follow something stricter like CNSA 2.0, you’ll know. Just substitute as appropriate.

Finally, remember the other big change of 2026: LLMs are very good at finding bugs now. “Get a review from the best model you can afford” is now firmly a Cryptographic Right Answer, along with the long-time classic “use a memory-safe language.”

If you’re in a rush, here’s the cheat sheet:

- **Encrypting data**: If you’re in the cloud, KMS. Otherwise, XAES-256-GCM or XChaCha20Poly1305 with random nonces. If you’re encrypting more than a few MB, use Cobblestone-128.
- **Symmetric key length**: Don’t stress it. 256-bit keys if you need to pick.
- **Symmetric “signatures”**: HMAC-SHA-256.
- **Hash algorithm**: SHA-2 is still fine, but use a real multihash (SequenceHash) if needed.
- **Random IDs**: 32 random bytes, a.k.a. 256-bit random numbers.
- **Password handling**: Argon2id or scrypt. But have you considered passkeys instead?
- **Key exchange**: X25519+ML-KEM-768 in a secure, standard protocol.
- **Asymmetric encryption**: HPKE with (MLKEM768-X25519 a.k.a. X-Wing, HKDF-SHA256, AES-128-GCM), or age with PQ keys.
- **Asymmetric signatures**: ML-DSA-44.
- **Website or client-server application security**: If you’re in the cloud, use its ALB/NLB. Otherwise, rustls, Go’s crypto/tls, BoringSSL, AWS-LC, or s2n-tls with a popular, updated ACME client.
- **Online backups**: restic with a long random password.

Now let’s dive in.

### Encrypting data

If you’re in the cloud, **KMS**. Otherwise, **XAES-256-GCM** or **XChaCha20Poly1305** with random nonces. If you’re encrypting more than a few MB, use [**Cobblestone-128**](https://c2sp.org/chunked-encryption).

*Percival, 2009*: AES-CTR with HMAC.\
 *Ptacek, 2015*: NaCl/libsodium default, or ChaCha20Poly1305, or AES-GCM.\
 *Latacora, 2018*: KMS, or XSalsa20+Poly1305.\
 *Latacora, 2024*: KMS, or NaCl’s secretbox, or XChaCha20Poly1305.

[Latacora’s reasoning for why and when you’d use your cloud’s KMS](https://www.latacora.com/blog/cryptographic-right-answers/#encrypting-data) is still solid, so start there.

If you’re encrypting locally, you want an AEAD where you can avoid worrying about tracking nonces or how many messages you encrypted, so you want random nonces of at least 128 bits.[^2]

NaCl/libsodium’s secretbox is just a [paper-thin wrapper](https://words.filippo.io/nacl-api/) around a less common AE construction (XSalsa20+Poly1305), so instead I recommend [XAES-256-GCM](https://words.filippo.io/xaes-256-gcm/) or XChaCha20Poly1305. The former is an efficient composition of a standard KDF and AES-GCM, packaged as an extended-nonce AEAD, so even if your library doesn’t provide it under that name you can assemble it from the KDF and AES-GCM APIs.[^3]

Regular AES-GCM and ChaCha20Poly1305 with random nonces are only ok if you *know for certain* you won’t encrypt more than a billion messages with one key. AES-GCM-SIV is also a good AEAD choice if you happen to have it everywhere you need it, but that’s kinda unlikely. Some even newer options have nice properties like key commitment, but are not commonly available yet.

Since you have an AEAD, consider putting context in the additional data, like the user ID you’re encrypting for, or the version of the protocol. Anything not secret that you will definitely still know at decrypt time.

Very important is how you handle large messages, which KMS will not solve for you. *Do not* pass one large message to an AEAD, or you might be forced to process or store the plaintext before having authenticated it at decrypt time. *Absolutely do not* use unauthenticated stream ciphers. Instead, you should use STREAM (which is not a stream cipher but a way to use AEADs on a stream… I know, I know, terrible name). There are a lot of options—too many, in fact—and shamefully none are the consensus answer. [Tink has one](https://developers.google.com/tink/encrypt-large-files-or-data-streams#streaming_aead), [libsodium has one](https://libsodium.gitbook.io/doc/secret-key_cryptography/secretstream), [the AWS Encryption SDK has one](https://github.com/aws/aws-encryption-sdk-python/blob/a6c0413bb0ddc9bccdefad9f1d2c031e0bc4db29/examples/src/file_streaming_example.py), [FLOE is one](https://c2sp.org/FLOE), age has one, and the list goes on. I recommend [Cobblestone-128](https://c2sp.org/chunked-encryption), which is a STREAM variant derived from TLS, and is available in pyca/cryptography and soon in the Go standard library (and in the meantime at [filippo.io/cobblestone](https://filippo.io/cobblestone)).[^4]

*Avoid*: unauthenticated modes (CBC, CTR, raw stream ciphers), random 96-bit nonces unless you spent a lot of time thinking about how many messages you encrypt, rolling your own AEAD construction.

### Symmetric key length

**Don’t stress it.** 256-bit keys if you need to pick.

*Percival, 2009*: 256-bit AES keys.\
 *Ptacek, 2015*: 256-bit keys.\
 *Latacora, 2018*: 256-bit keys.\
 *Latacora, 2024*: 256-bit keys.

Remember that Grover doesn’t actually matter and 128-bit brute-force attacks don’t exist. Still, 256-bit keys rule out multi-target attacks, which is nice.

However, don’t make other decisions based on this. For example, the recommended Cobblestone suite uses 128-bit keys and that’s fine because it has a careful analysis of multi-target attacks. Likewise, don’t go changing the default cipher suite preferences of your TLS library to enforce 256-bit keys; there’s no need.

AES-256 is slower than AES-128 because unfortunately Rijndael ties the number of rounds to the key size, but this probably won’t matter in practice.

*Avoid*: anything other than 128 or 256 bits, worrying too much about size, keys that are not uniform (e.g. using a password or ASCII hex string directly as a key).

### Symmetric “signatures”

**HMAC-SHA-256**.

*Percival, 2009*: HMAC.\
 *Ptacek, 2015*: HMAC.\
 *Latacora, 2018*: HMAC.\
 *Latacora, 2024*: HMAC.

Yay for HMAC.

Just be careful to sidestep canonicalization and parser alignment bugs by always authenticating a verbatim string of bytes, not something you need to recompute.

*Good:*

```
{
    "inner": "{\"foo\":123}",
    "hmac": "MbPsZq7QGCllJgDnSSy/cyM10HjqiVu35UO7yyY5d3w="
}
```

Check the HMAC and then process or store or forward just `{"foo":123}`.

*Bad:*

```
{
    "foo": 123,
    "hmac": "MbPsZq7QGCllJgDnSSy/cyM10HjqiVu35UO7yyY5d3w="
}
```

You can use KMAC or keyed BLAKE3 if you already had a reason to use their underlying hashes. HMAC-SHA-512 is also fine.

Remember to compare tags with dedicated constant-time functions.

*Avoid*: rolling your own [YoloMAC](https://blog.trailofbits.com/2024/08/21/yolo-is-not-a-valid-hash-construction/#yolomac), encrypted hashes, standalone one-time MACs like Poly1305 or GMAC.

### Hash algorithm

**SHA-2** is still fine, but use a real multihash if needed.

*Percival, 2009*: SHA-256 (SHA-2), plan on SHA-3.\
 *Ptacek, 2015*: SHA-2.\
 *Latacora, 2018*: SHA-2.\
 *Latacora, 2024*: SHA-2.

The best option, if you have it available, is still SHA-512/256, which isn’t vulnerable to length-extension attacks. Otherwise, SHA-256 and SHA-512 are both fine.[^5]

SHA-3 is unnecessary (because SHA-2 is fine) and slow,[^6] but not *wrong*. If you already have Keccak (SHA-3) around for ML-KEM and ML-DSA, and you want to use it for hashing, draw 32 bytes from SHAKE128.

BLAKE2 is a weird choice these days. BLAKE3 is very fast, but complex and uncommon enough that you want to have a good reason to use it and a good reason to trust your implementation.

Multihashing is when you need to hash multiple *things* at once. For example, multiple columns of a row, or a filename along with the file contents. It’s subtler than it sounds, so use **[SequenceHash](https://blog.trailofbits.com/2026/10/02/sequencehash-multihashing-for-the-rest-of-us/)** with SHA-2, or TupleHash (SP 800-185) with SHA-3.

*Avoid*: SHA-1, MD5, all other hash functions, [rolling your own multihash](https://blog.trailofbits.com/2024/08/21/yolo-is-not-a-valid-hash-construction/#yolomultihash), truncating to less than 256 bits unless a cryptographer told you collisions are not a concern.

### Random IDs

**32 random bytes**, a.k.a. 256-bit random numbers.

*Percival, 2009*: 256-bit random numbers.\
 *Ptacek, 2015*: 256-bit random numbers.\
 *Latacora, 2018*: 256-bit random numbers.\
 *Latacora, 2024*: 256-bit random numbers.

Note that UUIDv4 and APIs like [rand.Text](https://pkg.go.dev/crypto/rand#Text) will give you ~128-bit values instead. That’s usually fine, unless you need more than 280 trillion (2⁴⁸) unique values. 256 bits is overkill, but why not.

Use your operating system’s CSPRNG: `getrandom` on Linux, `ProcessPrng` on Windows, `arc4random_buf` on macOS and BSD, `getRandomValues` on the web. A good library will wrap these for you.

*Avoid*: non-cryptographic PRNGs, userspace random number generators like OpenSSL’s, haveged, CPU jitter entropy.[^7]

### Password handling

**Argon2id** or **scrypt**. But have you considered **passkeys** instead?

*Percival, 2009*: scrypt or PBKDF2.\
 *Ptacek, 2015*: scrypt, then bcrypt, then PBKDF2 if you must.\
 *Latacora, 2018*: scrypt, then Argon2, then bcrypt, then PBKDF2 if you must.\
 *Latacora, 2024*: Argon2id, then scrypt, then bcrypt, then PBKDF2 if you must.

Everything my predecessors said is still spot on: “you can throw a dart at a wall to pick one of these.” I dropped bcrypt because the 72-byte truncation [bites](https://josephscott.org/blog/2024/11/okta-bcrypt-and-52-character-usernames/). One tip: you could argue to your auditors that scrypt is FIPS 140 compliant because its actual KDF step is PBKDF2, which is what they want. I heard it works.

For parameters, ignore RFC 9106 which apparently wants to murder your production system, and use `m=64 MiB, t=2, p=1` for Argon2id (libsodium’s INTERACTIVE preset) or `N=2^16, r=8, p=1` for scrypt. They use the same memory, but the scrypt ones will take ~1.5× the CPU time and ¼ the GPU time, which is why you should prefer Argon2id. If you’re doing non-interactive key derivation, feel free to crank up Argon2id’s *m*, and then *t* (or just *N*[^8] for scrypt) as far as tolerable.

Much more importantly, do you really need passwords? If you only need them for logins, could you use phishing-proof passkeys instead? Maybe with [email magic links as a fallback](https://rmondello.com/2025/01/02/magic-links-and-passkeys/)? Or just support SSO (as long as it’s OIDC and not SAML)?

(Note that in 2026, passkeys are still not PQ. That’s mostly fine. They’re still phishing-proof and credential-stuffing-proof. All a quantum attacker can do is log in to a specific site if they obtain its database or intercept a login.)

*Avoid*: regular hashes, spending time thinking about password hashes instead of focusing on passkeys, phishable variants of email-based logins.[^9]

### Key exchange

**X25519+ML-KEM-768** in a secure, standard protocol.

*Percival, 2009*: 2048-bit Group #14 with a generator of 2.\
 *Ptacek, 2015*: Probably still DH-2048, or NaCl.\
 *Latacora, 2018*: Probably nothing. Or Curve25519.\
 *Latacora, 2024*: X25519+ML-KEM-768.

The 2018 version of the Latacora answers is correct in asking “why are you as a developer thinking about this, and what does it even mean?!” Don’t roll your own key exchange protocol. Just pick X25519MLKEM768 in TLS, mlkem768x25519-sha256 in SSH, and/or MLKEM768-X25519 in HPKE and age. If you really need to obtain some keys and you know what you’re doing, both TLS and HPKE have “exporters” to derive them.

Non-PQ solutions are an active, urgent liability due to harvest-now-decrypt-later attacks, and PQ alternatives range from impractical (HQC-KEM, FrodoKEM) to impractical and broken ([Classic McEliece](https://groups.google.com/a/list.nist.gov/g/pqc-forum/c/2kfyUnLObWQ)).

For key exchange, using an ECC hybrid is cheap enough to be worth it, and indeed ML-KEM is usually deployed alongside ECDH. However, if picking or implementing the hybrid is going to make you stay even a single month longer on a classical, non-PQ key exchange, go straight to pure ML-KEM-768 now.

P-256+ML-KEM-768 is also fine, just less common. NIST P curves got a bad rep for many years, but I think it’s 90% because they came out before we knew how to do cryptography engineering particularly well (and because of… let’s say non-[NPOV](https://en.wikipedia.org/wiki/Wikipedia:Neutral_point_of_view) proselytizing). Almost none of the attacks against ECDH on NIST P curves even matter for ephemeral key exchanges.

*Avoid*: anything and everything without ML-KEM or X-Wing (a specific X25519+ML-KEM-768 combiner) in its name.

### Asymmetric encryption

**HPKE with (MLKEM768-X25519 a.k.a. X-Wing, HKDF-SHA256, AES-128-GCM)**, or **age with PQ keys**.

*Percival, 2009*: RSAES-OAEP with SHA-256, MGF1+SHA-256, and exponent 65537.\
 *Ptacek, 2015*: NaCl.\
 *Latacora, 2018*: NaCl/libsodium (box / crypto\_box).\
 *Latacora, 2024*: Tricky, wait for established PQ solutions.

This is one of the most improved answers in recent years, as we now have a standard to do hybrid encryption: HPKE. (“Hybrid” as in asymmetric + symmetric, not as in classical + post-quantum, yay for overloaded names.)

MLKEM768-P256 is also ok as a choice of KEM, but less available. Pure ML-KEM-768 is fine if it’s all you have. All KDF and AEAD choices are fine, as well.

If your message is large, do the same chunking Cobblestone does, with multiple sequential HPKE Seal/Open calls in place of the AEAD.

Note that the PQ HPKE KEMs are maddeningly still a draft.[^10] Worse, they are unnecessarily spread over four drafts, such that I had to write [filippo.io/hpke-pq](https://filippo.io/hpke-pq). However, I made the authors solemnly swear they will not change the wire format, and then we all shipped them in our cryptographic libraries, so you can and should use them.

If you need something higher-level, with multiple recipients or a CLI, use age[^11] with its new `age1pq`-prefixed keys, based on MLKEM768-X25519. PQ support is also rolling out for plugins like the YubiKey and the Secure Enclave ones.

*Avoid*: again anything not based on ML-KEM (RSA and NaCl’s box[^12] in particular), OpenPGP, [encrypting emails](https://www.latacora.com/blog/2020/02/19/stop-using-encrypted-email/).

### Asymmetric signatures

**ML-DSA-44**.

*Percival, 2009*: RSASSA-PSS with SHA-256, MGF1+SHA-256, and exponent 65537.\
 *Ptacek, 2015*: NaCl, Ed25519, or RFC 6979.\
 *Latacora, 2018*: NaCl or Ed25519.\
 *Latacora, 2024*: Ed25519 if short-lived, otherwise Ed25519+ML-DSA-65.

In January 2026, I would have told you to wait and see, and then [everything started moving a lot faster](https://words.filippo.io/crqc-timeline/). You need to move to PQ for auth, too.

Just use ML-DSA-44. If your message needs pre-hashing, use “external-μ (Mu)” signing, not HashML-DSA. Store keys as 32-byte seeds exclusively.

The two PQ alternatives are FN-DSA, which is stuck somewhere in the US Department of Commerce, and SLH-DSA, which is based on hash functions. The latter can make sense for some specialist use cases, but is slow, large, and not widely available in implementations.

Why not a hybrid or “composites”? Hybrid auth is [more complex](https://mailarchive.ietf.org/arch/msg/tls/CI9HgxA2UKBIyNj0xNSumH3Qogg/) and less useful than hybrid key exchange. I need to write a long-form post about this, but in short either you already have a key rotation strategy that you can use to rotate back to RSA/ECDSA/Ed25519 in the unlikely case of a cryptanalytic break, or you have such a long time horizon that you should assume quantum computers will break the classical half anyway.

Also, ML-DSA is just such a better signature algorithm than its predecessors. It has hedged randomness (which is better than both random and deterministic nonces), built-in context, extra properties like SUF-CMA, and is relatively simple to implement and test.

Why the -44 parameter set, when I’m recommending ML-KEM-768? Unlike ML-KEM-512 (NIST category 1), ML-DSA-44 sits at NIST category 2, with some margin above the 128-bit security level already.

The one exception to the no-composites rule is SSH keys. OpenSSH 10.6 picked `ssh-mldsa44-ed25519`. I think they [made a mistake](https://mailarchive.ietf.org/arch/msg/ssh/4sb0aPTWeX4KA3GgK77ns17UHrU/), but it would be a mistake for you to go off the beaten path there.

Note that asymmetric signatures are overused, and especially now that they are not cheap, you should try to avoid them. Consider whether you could use a simple HMAC. Maybe you can replace signed JWTs with random session tokens. If instead you do want to commit to something publicly, consider whether you should use [a transparency log](https://github.com/transparency-dev/tessera), and get a more practically useful notion of non-repudiability (and potentially smaller sizes, if you amortize the signature over multiple proofs).

*Avoid*: all non-PQ options like RSA/ECDSA/Ed25519, stateful signatures (LMS/XMSS), HashML-DSA, ML-DSA “[semi-expanded keys](https://cs.opensource.google/go/go/+/master:src/crypto/internal/fips140/mldsa/semiexpanded.go;l=13-22;drc=c888fd67f00f77446e563f90e3307de7b6b3a910),” composite signatures, OpenPGP.

### Website or client-server application security

If you’re in the cloud, use its **ALB/NLB**. Otherwise, **rustls**, **Go’s crypto/tls**, **BoringSSL**, **AWS-LC**, or **s2n-tls** with a **popular, updated ACME client**.

*Percival, 2009*: OpenSSL for websites, custom RSA protocol for applications.\
 *Ptacek, 2015*: OpenSSL, or BoringSSL if you can. Or just AWS ELBs.\
 *Latacora, 2018*: AWS ALB/ELB or OpenSSL, with Let’s Encrypt.\
 *Latacora, 2024*: AWS ALB/ELB or OpenSSL, with Let’s Encrypt.

Not recommending OpenSSL will probably be my least practical suggestion, but OpenSSL has been making consistently puzzling and problematic choices for years now. The pyca/cryptography authors, in [a long post about OpenSSL’s issues](https://cryptography.io/en/latest/statements/state-of-openssl/), summarize it as:

> Our experience with OpenSSL has been on a negative trajectory for several years.

They even [presented at the 2025 OpenSSL Conference](https://www.youtube.com/watch?v=RUIguklWwx0) (!?), apparently fruitlessly. They’re not alone in criticizing OpenSSL: here are [HAProxy](https://cdn.haproxy.com/blog/state-of-ssl-stacks) from the server side, and [curl](https://daniel.haxx.se/blog/2025/06/23/a-family-of-forks/) from the client side.

I recommend rustls or Go’s crypto/tls,[^13] which are memory safe. If you need to stick to C, both BoringSSL and AWS-LC are very competently maintained OpenSSL forks, and the latter, unlike BoringSSL, has versioned releases. s2n-tls and rustls-ffi are interesting options if you don’t need libssl compatibility.

Whatever you use, make sure it prioritizes the X25519MLKEM768 key exchange, which requires TLS 1.3. Your cloud load balancer might require you to select a PQ security policy. On RHEL 9 and derivatives, [you need to opt in](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/security_hardening/using-the-system-wide-cryptographic-policies_security-hardening#post_quantum_cryptography).

Previous lists split websites and client-server applications. I merged them because these days you’ll just use the same TLS stack for both. What changes is the PKI. For applications where you control both ends, consider making your own ML-DSA-44 X.509 certificates, self-signed or from a private root. For websites, wire up a *popular*, well-supported ACME client and keep it up to date. Merkle Tree Certificates will roll out in 2027, and you want to be well positioned to be included in the transition.

*Avoid*: rolling your own transport protocol, TLS libraries that failed at their core job before (GnuTLS[^14], Mbed TLS[^15] [^16] [^17], wolfSSL[^18] [^19] [^20] [^21] [^22] [^23] [^24] [^25], Botan[^26] [^27]), offbeat TLS libraries, OpenSSL before version 3.5 (the first with X25519MLKEM768), any OpenSSL if you can, TLS 1.0–1.1, TLS 1.2 if you can, rolling your own ACME client.

### Online backups

**restic** with a long random password.

*Percival, 2009*: Tarsnap.\
 *Ptacek, 2015*: Tarsnap.\
 *Latacora, 2018*: Tarsnap.\
 *Latacora, 2024*: Tarsnap.

[I checked restic’s encryption a very long time ago](https://words.filippo.io/restic-cryptography/) and it was slightly weird but good. Fully symmetric, so it’s not scared of quantum computers. Doesn’t look like anything changed since. If you need cloud storage, [rsync.net has a “secret” plan for restic users](https://www.rsync.net/products/restic.html), and they support [an append-only mode](https://www.rsync.net/resources/notes/2025-q4-rsync.net_technotes.html).

We all love Tarsnap, and I feel like I am betraying my elders right now… but its asymmetric cryptography layer is all RSA. *Technically*, I think the only things that truly break are the lower-capability keys: a write-only key can decrypt archives and a read-only key can forge archives. Server authentication is HMAC, and the RSA public keys are never sent to the server. In that sense, Tarsnap is as secure as restic, which is an impressive feat for a 2008 design. However, this feels like a major footgun with impending quantum computers.

## The picture

Kinda incredible that you can just walk up to this thing.

![The Colosseum, at night, pictured from the ground looking up.](https://assets.buttondown.email/images/cc14aaa4-8914-48a4-9b5d-83f0ed53dd7a.jpeg?w=960&fit=max)

Follow me on Bluesky at [@filippo.abyssdomain.expert](https://bsky.app/profile/filippo.abyssdomain.expert) or on Mastodon at [@filippo@abyssdomain.expert](https://abyssdomain.expert/@filippo).

[^1]: FIPS 140 still has some very ugly bits around module validation (runtime self-tests, the integrity check, and the worsening RNG mess) but the actual cryptography algorithms these days are “good, actually.”

[^2]: You might read that GCM can take 128-bit nonces, but when you do that it doesn’t have a separate CTR counter anymore, and messages with different GCM nonces can end up sharing CTR keystream, which makes the analysis more complicated.

[^3]: Full disclosure: XAES-256-GCM is my design, but there are now multiple third-party implementations, including as part of Google’s Tink libraries, there’s [a security analysis by Amazon Web Services](https://eprint.iacr.org/2025/878.pdf), and it’s [used by Microsoft in an official SDK](https://learn.microsoft.com/en-us/industry/healthcare/dragon-copilot/sdk/ambient/security-guidance#encryption-at-rest).

[^4]: Again, full disclosure, this is my design, but it’s adopted externally enough that I feel comfortable recommending it.

[^5]: On 64-bit machines, SHA-512/256 and SHA-512 should be faster than SHA-256 thanks to their larger block size, except right now hardware support for SHA-256 is more widespread, so SHA-256 is faster in practice. They are both fast, so you probably don’t care.

[^6]: Partially because of choices NIST made to make SHA3-256 strictly as secure as SHA-256, even where SHA-256 was overkill. SHA-256 and SHA3-256 have 128 bits of collision resistance and 256 bits of second preimage resistance. It’d be enough to have 128 bits of both, which is what you get from 256 bits of SHAKE128 output. Unfortunately, it doesn’t have a SHA name. TurboSHAKE128 (RFC 9861) would be even better, but it is neither FIPS 140 nor widespread.

[^7]: People outside the FIPS 140 circle of hell don’t seem to have noticed, but new NIST rules require FIPS 140 modules to have an “internal” entropy source now. If your FIPS 140 “module” is a piece of software… what do you do? Everyone’s answer is “CPU jitter”: run a tight loop that accesses uncached memory and take high-resolution timestamps, hoping the frequency mismatch between CPU and main memory will make the measurements unpredictable. IMHO, this is the worst part of FIPS 140 since the module integrity hash, and it will hurt people. The one sane way to do it is to go through the motions, and then at the last moment mix the result with kernel entropy, like Go does. However, that’s *harder* to justify to the validation labs than doing just CPU jitter, so I’m afraid we’ll see pure CPU jitter in production a lot.

[^8]: Argon2id parameters mostly do what you’d imagine, [scrypt parameters do not](https://words.filippo.io/the-scrypt-parameters/).

[^9]: A link with an opaque random token that logs in the opening browser is very hard to phish. A link that logs in the browser that *started* the login is very easy to phish. Short numeric codes are moderately easy to phish. (You can get the best of both worlds by using the Broadcast Channel API to pass the token from the new tab to the original one.)

[^10]: In a different venue I said a serious engineering community would have a post-mortem about having wasted two of the five years we had for the transition to post-quantum cryptography, only to produce specs that changed *nothing* compared to what we had in 2024. But that’s for another time.

[^11]: Again again, full disclosure: my design, but pretty uncontroversial pick.

[^12]: I truly don’t understand why my predecessors recommended NaCl’s box. It’s an [incredibly sharp and low-level API](https://words.filippo.io/nacl-api/), pretending to be a high-level safe API.

[^13]: One last time, full disclosure: I am a crypto/tls maintainer.

[^14]: [CVE-2020-13777](https://nvd.nist.gov/vuln/detail/CVE-2020-13777), GnuTLS 3.6.4–3.6.13: the session ticket key is all zeroes, breaking confidentiality in TLS 1.2 and allowing authentication bypass in TLS 1.3.

[^15]: [CVE-2023-45199](https://nvd.nist.gov/vuln/detail/CVE-2023-45199), Mbed TLS 3.2–3.4: pre-auth remote code execution via an oversized key share in the TLS 1.3 handshake.

[^16]: [CVE-2026-34873](https://nvd.nist.gov/vuln/detail/CVE-2026-34873), Mbed TLS 3.5.0–4.0.0: TLS 1.3 session resumption allows client impersonation.

[^17]: [CVE-2026-25835](https://nvd.nist.gov/vuln/detail/CVE-2026-25835), Mbed TLS 2.18.0–3.6.5 and 4.0.0: the RNG state is cloned across fork, so parent and child produce the same random values.

[^18]: [CVE-2026-5194](https://nvd.nist.gov/vuln/detail/CVE-2026-5194), wolfSSL 3.12.0–5.9.0: signatures by ECDSA certificates can be forged by providing an ML-DSA or Ed25519 signature algorithm identifier and a trivial ECDSA signature over the zero-length digest.

[^19]: [CVE-2026-5501](https://nvd.nist.gov/vuln/detail/CVE-2026-5501), wolfSSL through 5.9.0: the OpenSSL compatibility layer skips the leaf’s signature check when any valid end-entity cert is supplied as an “intermediate.”

[^20]: [CVE-2026-11310](https://nvd.nist.gov/vuln/detail/CVE-2026-11310), wolfSSL 5.8.4–5.9.1: the OpenSSL compatibility layer lets caller-supplied untrusted intermediates act as trust anchors.

[^21]: [CVE-2026-11999](https://nvd.nist.gov/vuln/detail/CVE-2026-11999), wolfSSL 5.7.4–5.9.1: the OpenSSL compatibility layer accepts any chain longer than the maximum path depth of 100, without reaching a trust anchor.

[^22]: [CVE-2026-55960](https://nvd.nist.gov/vuln/detail/CVE-2026-55960), wolfSSL 5.6.4–5.9.1: a raw public key is always accepted in place of a certificate, bypassing authentication.

[^23]: [CVE-2026-93302](https://nvd.nist.gov/vuln/detail/CVE-2026-93302), wolfSSL 5.3.0–5.9.2: trusted-peer certificates are matched by name rather than public key.

[^24]: [CVE-2026-89136](https://nvd.nist.gov/vuln/detail/CVE-2026-89136), wolfSSL 5.6.0–5.9.2: clients still accept an unsolicited raw public key from the server, bypassing authentication.

[^25]: [CVE-2026-89135](https://nvd.nist.gov/vuln/detail/CVE-2026-89135), wolfSSL 5.8.4–5.9.2: a failed chain verification in the OpenSSL compatibility layer permanently adds the attacker’s CA to the trust store used by native TLS, OCSP, and CRL checks.

[^26]: [CVE-2026-34580](https://nvd.nist.gov/vuln/detail/CVE-2026-34580), Botan 3.11.0: an end-entity certificate whose DN matches a trusted root is accepted as if it were that root.

[^27]: [CVE-2026-34582](https://nvd.nist.gov/vuln/detail/CVE-2026-34582), Botan before 3.11.1: TLS 1.3 servers process application data before Finished, so a client can skip Certificate, CertificateVerify, and Finished entirely, a full client authentication bypass.
