---
title: Passkeys are not a trap
link: https://scotthelme.co.uk/passkeys-are-not-a-trap/
source: hnrss-org
published: 2026-10-14T15:15:45Z
updated: 2026-10-14T15:15:45Z
first_seen: 2026-10-15T00:11:15.830933315Z
authors:
- speckx
labels:
- passkeys
content: extracted
html: 2026-10-14-passkeys-are-not-a-trap.html
extra:
  comments_url: https://news.ycombinator.com/item?id=50088935
  num_comments: 611
  points: 435
---

A post called [Passkeys are a trap. Do not use them.](https://nonfunctional.substack.com/p/passkeys-are-a-trap-do-not-use-them?utm_source=scotthelme.co.uk) has been doing the rounds over the last few days, making it pretty high on Hacker News, and I've had it sent to me many times with an 'is this true?' kind of vibe. It's written for "non-technical citizens of the web", and the headline advice is to stop using passkeys and go back to TOTP codes and passwords.

### The Argument

The post makes what appears to be one central claim: Passkeys support something called attestation, which lets a website ask for cryptographic proof of what made the passkey.

The author calls this a "hidden dial" and argues that sites will slowly turn it up, until you're only allowed to log in with approved software on an approved operating system, with no modifications, no LineageOS and no sharing your Netflix password. Throw in uBlock Origin, the FIDO Alliance membership list and the UK's image-scanning proposals, and the conclusion is that passkeys are "not designed to prioritize your interests". \<that-escalated-quickly.gif>

It's a compelling story. It just doesn't match how any of this actually works...

![](https://storage.ghost.io/c/ee/88/ee889f88-37ef-43e5-9180-f9b88ee6261d/content/images/2026/10/image.png)

### Your Passkeys Don't Attest

The passkeys most people use are synced passkeys, stored in iCloud Keychain, Google Password Manager, 1Password, Bitwarden or whatever other software of your choosing. These passkeys don't give websites an attestation statement at all, and though a site can ask, what comes back is `none`. I know this because I wrote an [opensource passkeys library](https://scotthelme.co.uk/open-sourcing-passkeys-php-a-security-focused-webauthn-library-for-php/?utm_source=scotthelme.co.uk) and made it available to everyone.

If a consumer website decided tomorrow that it would only accept attested passkeys, it would lock out almost all of its own users, instantly. What website is going to do that? Besides, the guidance for consumer sites has been to not even request attestation for *years* now.

### Attestation Isn't What You Think It Is

The post describes attestation as proving "that an approved passkey manager, operating system, and firmware are in use" which is simply not true.

When it is present, WebAuthn attestation tells the site the make and model of the authenticator, identified by its `AAGUID`, with a certificate chain from the vendor to back it up. It says nothing about your OS, your browser, your extensions, whether your phone is rooted or jailbroken, your firmware version, or even what colour it is.

### The YubiKey Recommendation

This is the part that surprised me the most. For people who want stronger security, the post recommends a hardware security key like a YubiKey, because it "avoids OS-wide attestation".

YubiKeys are the authenticators that *do* attestation. Every one of them ships with a Yubico-issued attestation certificate, and enterprise attestation exists specifically so organisations can require approved hardware keys. I'd still recommend a YubiKey to anyone who wants one, they're excellent and I use them, but if attestation is the thing you're worried about, the post's own recommendation pushes you towards attestation rather than away from it!

### TOTP Is a Step Backwards

The post holds up TOTP as "already" a great solution. TOTP is better than a password alone, absolutely, but it has one big weakness: you can type the code into the wrong website, [just ask my good friend Troy Hunt](https://www.troyhunt.com/a-sneaky-phish-just-grabbed-my-mailchimp-mailing-list/?utm_source=scotthelme.co.uk). Phishing kits like Evilginx sit in the middle, relay your password and code to the real site in real time, and walk away with your session. This isn't theoretical, it's how a huge number of account takeovers happen today and we have the evidence to prove it.

This is the problem that passkeys solve. A passkey is bound to the domain it was created for, so your browser simply won't use your `example.com` passkey on `examp1e.com`. There's no code to read out and nothing for the user to get wrong. The author even concedes this in the addendum, saying that passkeys are domain-bound and resist man-in-the-middle attacks, but still tells readers to avoid them. Swapping a phishing-resistant credential for a phishable one is a really bad trade...

### What You Should Do

Keep using passkeys! Wherever a site offers one, take it, and store it in a password/passkey manager you trust. If you want to be able to move your credentials around, pick a provider that supports that. If you're worried about attestation creeping into consumer logins, then that's worth pushing back on, loudly (if it ever happens). But it hasn't happened, the biggest providers don't support it, and avoiding passkeys today while you wait for it won't protect you from anything, it just leaves you open to phishing.

### Sources

I have some experience in this area:

I created [WhyNoPasskeys.com](https://whynopasskeys.com/?utm_source=scotthelme.co.uk) to track the availability of passkeys on the Web.

[I built and deployed passkeys support on my own SaaS company.](https://scotthelme.co.uk/launching-passkeys-support-on-report-uri/?utm_source=scotthelme.co.uk)

[We built our own passkeys library and had it security reviewed.](https://scotthelme.co.uk/bringing-in-the-experts-having-our-passkeys-implementation-security-tested/?utm_source=scotthelme.co.uk)

I wrote about [Security considerations when using Passkeys on your website](https://scotthelme.co.uk/security-considerations-when-using-passkeys-on-your-website/?utm_source=scotthelme.co.uk).

I wrote [Passkeys 101: An Introduction to Passkeys and How They Work](https://scotthelme.co.uk/passkeys-101-an-introduction-to-passkeys-and-how-they-work/?utm_source=scotthelme.co.uk).

I made our [passkeys library opensource](https://scotthelme.co.uk/open-sourcing-passkeys-php-a-security-focused-webauthn-library-for-php/?utm_source=scotthelme.co.uk) for everyone to use.

I've done [extensive security testing](https://scotthelme.co.uk/passkeys-permissions-policy-and-bug-hunting-in-1passwords-webauthn-wrapper/?utm_source=scotthelme.co.uk) of passkeys implementations.

I've openly [acknowledged the limitations of not using attestation](https://scotthelme.co.uk/xss-is-deadly-for-passkeys-the-hidden-risk-of-attestation-none/?utm_source=scotthelme.co.uk) on passkeys.

I think most of all that it's important to say I use passkeys everywhere I can because they're currently the best thing we have available to protect our accounts. And they're not a trap.

Have you enjoyed this post or found it helpful? \
☕️ Consider [buying me a coffee](https://donate.stripe.com/4gMeVd3xm0sOcRzexF93y00) to say thanks! \
🔔 Subscribe for [free notifications](https://scotthelme.ghost.io/#/portal/signup) when I publish! \
🤩 Become a [member](https://scotthelme.ghost.io/#/portal/signup) and support my content!

Tags: [Passkeys](https://scotthelme.co.uk/tag/passkeys/)
