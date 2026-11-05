---
title: Your Cryptography Conspiracy Theory Is Wrong
link: https://soatok.blog/2026/10/29/your-cryptography-conspiracy-theory-is-wrong/
source: lobste-rs
published: 2026-11-04T14:58:11Z
updated: 2026-11-04T14:58:11Z
first_seen: 2026-11-05T09:48:06.467196556Z
authors:
- soatok.blog via fanf
labels:
- bullshit
- conspiracy theories
- crypto
- cryptography
- national security agency
- online privacy
- society
summary: Comments
content: extracted
html: 2026-11-04-your-cryptography-conspiracy-theory-is-wrong.html
remote_preview:
  url: https://i0.wp.com/soatok.blog/wp-content/uploads/2026/10/BlogHeader-2026-ConspiracyTheories-1.png?fit=1200%2C675&ssl=1
  alt: Your Cryptography Conspiracy Theory is Wrong
---

This conversation plays out hundreds of times a day, but (thankfully) only a handful of those involve me:

- Person: “I have \[problem\].”
- Cryptography nerd: “Oh, you probably want to use \[sensible recommendation\].”
- Person: “Ah, well, can I really trust that? The NSA probably backdoored it 30 years ago. Dual\_EC, amirite?!”
- Cryptography nerd: …
- *Koblitz and Menezes, unheard: “[You fucking wot m8?](https://eprint.iacr.org/2015/1018)“*

Conspiracy theories about the NSA and/or NIST are a dime a dozen these days. And, ever since the Snowden disclosures in 2013, I am politically primed to *directionally agree* with distrusting the US government wholesale.

But most of the technical arguments baked into folks’ conspiracy theories **make no goddamn sense**.

> This distrust is not exclusive to only the US government.
>
> The goal of this blog post is not to convince you that you should trust *any* government; just that they are not magical or omniscient.

To that end, I’m going to explore some of the history of NSA, NIST, and cryptography standardization. Then, to explore some of the conspiracy theories that were being floated about such activity, and what we’ve learned in the years since (especially in the past month). Finally, I’m going to examine some of the current conspiracy theories floating around message boards and social media today, and explain why they’re almost certainly wrong.

Buckle up, this is going to be a tedious one.

## Why should we listen to you?

If you find yourself asking a question like this, the correct answer is simply, “You shouldn’t.”

After all, I have never held (let alone even applied for) a security clearance, and have no access to any non-public information. I’m also not a current or former government employee, and have no aspirations to ever become one. I have access to all the same facts anyone else does.

In balance, I have no horse in the race of who trusts whom. I’m just tired of hearing the same poorly reasoned arguments all the damn time, and would like to stop having to repeat myself when I encounter one.

## Historical NSA Shenanigans

The sources for this, by the way, are [the declassified United States Senate Select Committee on Intelligence](https://cryptome.org/2012/05/nsa-crypto-des.pdf) and [United States Cryptologic History](https://archive.org/details/NSA-American-Cryptology-Cold-War/American%20Cryptology-Book%20III/).

### DES

[YouTube video player](https://www.youtube.com/watch?v=kJQP7kiw5Fk)

No, not *that* DES.

The Data Encryption Standard (DES) was published in the Federal Register in 1975 and then standardized in 1977.

We consider it weak these days, but DES was a real game-changer for its time: It offered a standard block cipher to use for sensitive (but not classified) commercial applications. Prior to DES, the history of cryptography was largely about military uses (except a few quirky mathematicians here and there), going back before [Caesar’s cipher](https://en.wikipedia.org/wiki/Caesar_cipher).

From declassified documents, we now know the full story, but from the time that DES was first standardized, up until recent years, there have been a concerns about [NSA’s involvement](https://en.wikipedia.org/wiki/Data_Encryption_Standard#NSA's_involvement_in_the_design).

> We sent the S-boxes off to Washington. They came back and were all different.
>
> Alan Konheim, quoted in *[Applied Cryptography](https://onlinelibrary.wiley.com/doi/book/10.1002/9781119183471)*, p. 280

To say that academics did not trust DES would be putting it mildly: Martin Hellman (who, with Whitfield Diffie and Ralph Merkle, *invented* public-key cryptography) sent a letter in 1975 to the National Bureau of Standards that stated:

> Whit Diffie and I have become concerned that the proposed data encryption standard, while probably secure against commercial assault, may be extremely vulnerable to attack by an intelligence organization.
>
> Martin Hellman, re-published in [this paper](https://faculty.nps.edu/dedennin/publications/DES-15Years.pdf) in 1990.

At the heart of the concern about DES was something called an S-box, which simply maps an input to an output, but whose purpose is to provide a non-linear transformation to obscure the relationship between the ciphertext and the key.

The S-box used by DES was chosen by NSA. This led a lot of people to suspect there was a backdoor in the selection of the S-boxes for DES. **It turned out to be the opposite, of course**: The NSA had discovered differential cryptanalysis in secret, and had selected S-boxes that were resistant to this technique, but did not want to reveal it publicly (lest they lose their advantage over rival nations).

There’s a lot more details to this story than I want to bore anyone with, so I recommend reading up on the history of cryptography and cryptanalysis if you’re interested in such things.

In balance, [at the advice of NSA](https://www.schneier.com/blog/archives/2004/10/the_legacy_of_d.html), the final DES design did only offer 56 bits of security (down from an original key size of 128 bits). This short key ultimately allowed [the EFF to crack DES in 1998](https://w2.eff.org/Privacy/Crypto/Crypto_misc/DESCracker/) with a hardware cost of about $250,000.

#### What does this actually tell us?

When IBM was designing DES, with feedback from NSA, the stated purpose was to use for commercial cryptographic applications in the United States.

Fact: NSA used secret cryptanalytic capabilities to ensure the resulting block cipher that became DES was *protected from the attack they knew about* without revealing said technique.

Fact: NSA recommended reducing the key length from 128 bits to 64 bits (with 8 of the key bits serving as parity bits, thus making it only have 56-bit security).

This both strengthened and weakened DES in different ways. A 56-bit key can be broken by a distributed computing effort in under 24 hours, as of 2012.

[YouTube video player](https://www.youtube.com/watch?v=gkPvZDcrLFk)

DES’s 56-bit keys are at the heart of Moxie’s MSCHAPv2 break.

This is confusing until you remember NSA has two opposing missions, with (unfortunately) military jargon names.

- COMSEC (Communications Security) — basically blue-teamers
- SIGINT (Signals Intelligence) — basically red-teamers

The COMSEC folks have a mission to secure the US government and protect US interests (read: private sector).

The SIGINT folks have a mission to spy on other nations on behalf of the US.

In theory, the SIGINT folks aren’t supposed to spy on Americans *at all*. In practice, their Five Eyes partners gives them a stupid loophole: An allied nation can simply do the spying for them, so long as federal prosecutors are willing to engage in a little bit of [parallel construction](https://en.wikipedia.org/wiki/Parallel_construction).

Take note of the DEA section of the previously linked Wikipedia page.

You cannot trust the government, but if you know the mission each department operates under, you can understand *their incentive structures*. Knowing their incentive structures lets you reason about their actual activities–or, failing that, create conspiracy theories with a cohesive narrative.

The COMSEC mission can be related to the S-box strengthening; the SIGINT mission, to the weaker key length.

### Clipper Chip

Okay, so the DES thing was a messy affair. Let’s look at an example of the NSA clearly being on the wrong side of privacy.

The [Clipper Chip](https://en.wikipedia.org/wiki/Clipper_chip), which was designed and promoted by the NSA in 1993 to secure voice messages, with a backdoor.

Clipper Chip was an early example of a NOBUS (Nobody But Us can exploit it) backdoor. Unlike DES’s 56-bit key, the cipher used (Skipjack) was promoted up to a whopping 80-bits.

But before you celebrate, be aware that there was a “key escrow” feature that allowed law enforcement to decrypt plaintext, and a 16-bit hash in the “Law Enforcement Access Field” (LEAF) that turned out to be a pretty stupid vulnerability, as [Matt Blaze discovered in 1994](https://www.wired.com/1994/09/clipping-clipper-matt-blaze/). If you want a technical breakdown, [Professor Matt Green’s blog post](https://blog.cryptographyengineering.com/2015/07/20/a-history-of-backdoors/) covers this (and other backdoors).

Not only was the Clipper Chip a terrible idea to begin with, it was also technically flawed (to the point of embarrassment). This was the first of many issues in what became known as the Crypto Wars.

#### What does this actually tell us?

When the NSA colluded with law enforcement to spy on Americans’ private conversations, their attempt was met with public backlash from independent technologists (Matt Blaze, the Electronic Frontier Foundation, etc.).

The cryptography community outside the military-industrial complex *today* is largely primed to call out Clipper Chip style bullshit whenever it happens. (See also: the EU’s ChatControl.)

![Soatok pointing at a blackboard.](https://i0.wp.com/soatok.blog/wp-content/uploads/2023/10/SoatokLecture.png?w=512&ssl=1)\
AJ\_LovesDinos

### Dual\_EC\_DRBG

In 2006, NIST published [SP 800-90](https://csrc.nist.gov/pubs/sp/800/90/final), which recommended deterministic random bit generators (DRBGs):

- **Hash\_DRBG** — generating random bits from cryptographic hash functions computed over input entropy sources
- **HMAC\_DRBG** — generating random bits from HMAC (which is kind of like if a hash function had a secret key)
- **CTR\_DRBG** — turning a block cipher into a random bit stream by using Counter Mode

The three I listed above are… well, they’re fine. Pretty much everyone uses hash functions or block ciphers to squeeze random bits out of smaller amounts of entropy.

But the fourth entry in the NIST standard was an odd duck. Dual\_EC\_DRBG was several times slower than the other recommendations and was [immediately flagged as having a backdoor](https://eprint.iacr.org/2006/190) (which [other cryptographers agreed with](https://web.archive.org/web/20140621062515/http://archive.wired.com/politics/security/commentary/securitymatters/2007/11/securitymatters_1115)).

> I don’t understand why the NSA was so insistent about including Dual\_EC\_DRBG in the standard. It makes no sense as a trap door: It’s public, and rather obvious. It makes no sense from an engineering perspective: It’s too slow for anyone to willingly use it. And it makes no sense from a backwards-compatibility perspective: Swapping one random-number generator for another is easy.
>
> My recommendation, if you’re in need of a random-number generator, is not to use Dual\_EC\_DRBG under any circumstances. If you have to use something in SP 800-90, use CTR\_DRBG or Hash\_DRBG.
>
> In the meantime, both NIST and the NSA have some explaining to do.
>
> Bruce Schneier, Wired, 2007

Of course, because of Edward Snowden, we know today that cryptographers [were completely correct about Dual\_EC\_DRBG being a backdoor by the NSA](https://www.bbc.com/news/technology-24048343).

Ever since that revelation, no discussion about NOBUS (Nobody But Us) backdoors would be complete without at least mentioning Dual\_EC\_DRBG.

What almost nobody foresaw, however, was that a rival nation state would [replace the public key baked into Dual\_EC’s design with their own while backdooring Juniper Networks](https://dl.acm.org/doi/10.1145/2976749.2978395).

*Kind of a stupid fucking approach to NOBUS, ain’t it?*

#### What does this actually tell us?

When the incentives of the NSA aligned with malicious government conspiracies against the American people (sorry to keep specifying this, by the way, but they’re pretty much always malicious to folks outside the USA) and standardization processes, it didn’t fly under the radar and basically every cryptographer said “Hell no” to using it.

Whether NSA is capable of being sneakier while corrupting NIST documents remains to be seen.

![Soatok yelling at his computer](https://i0.wp.com/soatok.blog/wp-content/uploads/2024/08/SoatokNerdRage.png?w=512&ssl=1)\
AJ\_LovesDinos

### The Snowden Disclosures

There’s far too much to talk about here, and of varying usefulness. Consult [the Snowden Archives](https://snowden.xsurveillance.site/) if you want to read more. There were a lot of codenames. There was a lot of jargon.

It spawned a lot of memes.

![SSL added and removed here! :^)](https://i0.wp.com/soatok.blog/wp-content/uploads/2026/10/image-2.png?w=500&ssl=1)\
From an NSA slide about Google’s internal networks, part of MUSCULAR

Some of the codenames (e.g., FOXACID) are the sort of thing that cause eyebrows to raise among computer nerds that are familiar with furries and [vore](https://en.wikipedia.org/wiki/Vorarephilia).

Here’s a lightning round of some of what we learned from Snowden:

1. Bulk collection of Americans’ phone metadata, despite [James Clapper committing perjury to the contrary](https://verdict.justia.com/2013/06/28/will-director-of-national-intelligence-james-clapper-be-prosecuted-for-lying-to-congress-regarding-the-nsas-surveillance)
2. **[PRISM](https://en.wikipedia.org/wiki/PRISM)**: mass passive surveillance of the Internet, which is largely why TLS became ubiquitous in the years since.
3. Tempora: copied data from undersea cables for NSA to analyze
4. [BULLRUN / EDGEHILL](https://www.nytimes.com/2013/09/06/us/nsa-foils-much-internet-encryption.html) — see also: [Dual\_EC\_DRBG](https://soatok.blog/2026/10/29/your-cryptography-conspiracy-theory-is-wrong/#dual-ec), but also [RSA Security](https://blog.cloudflare.com/how-the-nsa-may-have-put-a-backdoor-in-rsas-cryptography-a-technical-primer)
5. XKeyscore: basically a private version of Palantir’s product today

There’s so much more we could talk about, but what’s notably absent from Snowden’s leaks is any secret cryptanalytic attacks against standardized cryptography.

Notably, they couldn’t break Tor in any way that wasn’t already known to the Tor developers at the time.

Hell, there were myriad conspiracy theories in late 2013 about the Silk Road marketplace being taken down by a NSA attack on Tor. The slide decks Snowden leaked suggest otherwise.

#### What does this actually tells us?

A lot. Far too much, in fact, for me to dissect it all in this blog post!

But the pertinent detail is this: The absence of cryptanalytic capabilities in Snowden’s leaks can be interpreted one of several ways.

1. The private sector has largely caught up with NSA
2. NSA’s compartmentalization was **somehow** successful at preventing Snowden from discovering these mathematical techniques, but not all the other damning evidence
3. The NSA didn’t bother trying to beat the cryptography because it was, as is often the case today, so much easier to simply bypass it entirely

Personally, I think any of these interpretations is valid. I’d put my money on number 3, of course. After all, that’s what PRISM largely did.

![Soatok glitching out](https://i0.wp.com/soatok.blog/wp-content/uploads/2025/04/SoatokGlitch.png?w=512&ssl=1)\
AJ\_LovesDinos

## People Can’t Be Normal About This

I omitted several controversies from the previous section, because it was getting too long and I feel I justified my point already. However, I’m sure some people will cry foul at omissions as an attempt at deception, so I need to also mention one that was recently solved.

### The NIST P-Curve Seeds

> The what now?

Elliptic curve cryptography is *a whole topic* I’ve talked about on this blog before, at length, but I’m going to simplify for the sake of being somewhat comprehensible to people who don’t make Frobenius trace jokes on IRC.

NIST published [FIPS 186-2 in 2000](https://csrc.nist.gov/pubs/fips/186-2/final) (and later [updated it in 2001](https://csrc.nist.gov/pubs/fips/186-2/upd1/final)), which provided several elliptic curve groups (P-192, P-224, P-256, P-384, P-521).

Each elliptic curve group was parametrized by a seed, `s`, which was never explained by NSA. The output of the SHA-1 hash of `s` was used to calculate the parameters for each elliptic curve group.

Jerry Solinas, who was the mathematician and cryptographer that published the P-curves, couldn’t remember how the seeds were chosen.

In the wake of Dual\_EC\_DRBG and BULLRUN, any unexplained value being the basis of several NIST standardized elliptic curve groups caused many people to be itchy.

For a while, Daniel J. Bernstein’s Curve25519 algorithms and software gained popularity in the applied cryptography spaces. This was probably a good thing, overall, but there were [some sharp edges](https://moderncrypto.org/mail-archive/curves/2017/000898.html) (that later work [has since improved upon](https://ristretto.group/)).

Speaking from experience, many of these decisions post-Snowden were the result of paranoia about the NSA possibly adding a NOBUS backdoor to the NIST P-Curves too, rather than merely for the technical merits of Curve25519.

However, Koblitz and Menezes already tackled that in a paper titled, “[A Riddle Wrapped In An Enigma](https://eprint.iacr.org/2015/1018).” To summarize their mathematical arguments: If NSA was aware of a secret weakness that only occurs in some number of elliptic curve groups, and wanted to make a NOBUS backdoor that their adversaries couldn’t figure out, it would need to be a structural weakness that only occurs extremely rarely.

So rarely, in fact, that not only would the computational resources available to Jerry Solinas in the late 1990s would not have been sufficient, but you still couldn’t do it today.

> This scenario is highly implausible for several reasons. First, the class of weak curves must be extremely large in order to obtain a weak curve with the seeded-hash method. For concreteness, suppose that p is a fixed 256-bit prime. There are roughly ![2^{257}](https://s0.wp.com/latex.php?latex=2%5E%7B257%7D&bg=0a0a0a&fg=f0f0f0&s=3&c=20201002) isomorphism classes of elliptic curves defined over ![F_p](https://s0.wp.com/latex.php?latex=F_p&bg=0a0a0a&fg=f0f0f0&s=3&c=20201002). Let ![s](https://s0.wp.com/latex.php?latex=s&bg=0a0a0a&fg=f0f0f0&s=3&c=20201002) be the proportion of elliptic curves over ![F_p](https://s0.wp.com/latex.php?latex=F_p&bg=0a0a0a&fg=f0f0f0&s=3&c=20201002) that are believed (by everyone except hypothetically the NSA in 1997) to be safe. This class of curves includes essentially all elliptic curves of prime order (with the exception of prime-field anomalous curves and those that succumb to the Weil/Tate pairing attack). Since the proportion of 256-bit numbers that are prime is approximately ![1/(256 ln 2) \approx 2^{\neg 8}](https://s0.wp.com/latex.php?latex=1%2F%28256+ln+2%29+%5Capprox+2%5E%7B%5Cneg+8%7D&bg=0a0a0a&fg=f0f0f0&s=3&c=20201002), the proportion of curves that are strong is at least ![2^{\neg 8}](https://s0.wp.com/latex.php?latex=2%5E%7B%5Cneg+8%7D&bg=0a0a0a&fg=f0f0f0&s=3&c=20201002). Now suppose that the proportion of these curves that the NSA knew how to break were ![2^{\neg 40}](https://s0.wp.com/latex.php?latex=2%5E%7B%5Cneg+40%7D&bg=0a0a0a&fg=f0f0f0&s=3&c=20201002). In this hypothetical scenario the NSA could select such a weak curve by trying about ![2^{48}](https://s0.wp.com/latex.php?latex=2%5E%7B48%7D&bg=0a0a0a&fg=f0f0f0&s=3&c=20201002) seeds. The number of NSA-weak curves would then be approximately ![2^{209}](https://s0.wp.com/latex.php?latex=2%5E%7B209%7D&bg=0a0a0a&fg=f0f0f0&s=3&c=20201002). This would mean that this huge class of weak curves was known to the NSA in 1997 but is still undiscovered by outside researchers in 2015. **It is highly unlikely that such a large family of weak elliptic curves would have escaped detection by the cryptographic research community from 1997 to the present.**
>
> Koblitz and Menezes, 2015 (emphasis mine)

This quote highlights my broader point in this whole blog post: The NSA isn’t magical. (Nor is AI. Nor are quantum computers.) The NSA’s capabilities and budgets are finite. Their incentives are defined by their mission.

In 2023, Filippo Valsorda and several others (including myself) pledged [a bounty](https://words.filippo.io/seeds-bounty/) (with extra if the award is donated to charity) to anyone that could solve the mystery of the NIST P-curve seeds.

Earlier this month, Anthropic [successfully claimed that bounty](https://words.filippo.io/seeds-solved/), and elected to donate the bounty to the ISRG (the non-profit that operates [the Let’s Encrypt certificate authority](https://letsencrypt.org/)).

There’s a poetic irony here: My pledged donation is going towards the organization that led the way to making TLS ubiquitous online since 2015 in the wake of the NSA’s global mass surveillance… **because one of the NSA mysteries was finally solved**.

This is a nice segue into the final topic I want to discuss today.

## Post-Quantum Cryptography

I’ve already covered most of what I want to say here in a previous post titled, *[Hybrid Constructions: The Post-Quantum Safety Blanket](https://soatok.blog/2026/04/13/hybrid-constructions-the-post-quantum-safety-blanket/)*.

I was tagged in a Fediverse post yesterday that punctuated the sort of discourse cryptographers have already seen in the IETF mailing list threads and on technical forums in the past few months:

> NIST works for US fash regime that’s trying to help their AI scammer buddies pivot to quantum scams.
>
> That’s the generous interpretation.
>
> The worse one is that NSA has known flaws/backdoors in the post quantum systems and wants to shut down the actually-secure ones.
>
> [@dalias@hachyderm.io](https://web.archive.org/web/20261029071333/https://hachyderm.io/@dalias/117517818813433375) – (To be clear, I chose this comment as an example because it’s more coherent than the others I’ve seen in 2026. Do NOT be a jerk to anyone.)

This was a reply to a quote-post of [another thread](https://web.archive.org/web/20261029071728/https://defcon.social/@infosec/117516999211815499) that described NIST publishing its final plan for [IR 8547](https://csrc.nist.gov/pubs/ir/8547/final): the transition to post-quantum cryptography.

The NSA announced, in 2015, [its plans to migrate to “quantum resistant algorithms”](https://web.archive.org/web/20151016020521/https://www.nsa.gov/ia/programs/suiteb_cryptography/).

For the decade or so since, the NIST Post-Quantum Cryptography standardization effort has largely revolved around [the NIST pqc-forum](https://groups.google.com/a/list.nist.gov/g/pqc-forum). You can read the archived email threads, publicly, going back to the forum’s inception. Most of the emails are dry, boring, technical, and/or procedural.

But in the beginning? It was a *bloodbath* of cryptanalytic attacks against proposed KEMs and signature algorithms.

The NIST recommendations and standards for post-quantum cryptography were a transparent effort involving *virtually anyone in the cryptography and security communities that wanted to participate* spanning from 2015 until now, and it is still ongoing.

The selected algorithms (i.e., ML-KEM, formerly known as CRYSTALS-Kyber) were designed by international teams. The principal submitter of ML-KEM, Peter Schwabe, even worked with Daniel J. Bernstein on [NaCl in the 2010s](https://nacl.cr.yp.to/) (and was [one of Bernstein’s Ph.D Students](https://cryptojedi.org/peter/index.shtml)).

> Bernstein, for those not following along, was a large source of irritation on the TLS Working Group mailing list earlier this year for his weird anti-ML-KEM and anti-ML-DSA tirades and concern-trolling.
>
> At one point, he tried to summon Twitter mobs to join the mailing list and pack the vote… except, that’s not how Rough Consensus works at IETF at all!
>
> Today, many “NIST backdoored post-quantum” conspiracy theories point to Bernstein to support their points.
>
> It’s all very stupid and disappointing.

Furthermore, there [isn’t enough structure in ML-KEM, in particular](https://keymaterial.net/2025/11/27/ml-kem-mythbusting/), to squeeze in a public key to make any plausible weakness truly NOBUS.

### How plausible is an NSA backdoor here, really?

I’m no Neil Koblitz or Alfred Menezes, so forgive me if my rebuttal doesn’t contain the mathematical rigor of those two.

Here are some facts to consider.

1. The federal government (through [CNSA 2.0](https://media.defense.gov/2025/May/30/2003728741/-1/-1/0/CSA_CNSA_2.0_ALGORITHMS.PDF)) is being told to adopt ML-KEM-1024 and ML-DSA-87. Hybrid constructions are not blessed for protecting federal government systems.
2. As we saw with [the DES S-boxes](https://soatok.blog/2026/10/29/your-cryptography-conspiracy-theory-is-wrong/#des), the NSA is known to sometimes make decisions that *improve* security (though may conceal an attack on competing designs).
3. NSA has a dual mission: COMSEC and SIGINT, as discussed above.
4. NSA has tried to subvert standards before ([DUAL\_EC\_DRBG](https://soatok.blog/2026/10/29/your-cryptography-conspiracy-theory-is-wrong/#dual-ec)).

Is it plausible that the NSA would be so confident in the NOBUS status of a secret flaw or backdoor in ML-KEM (which they did not design) that they’d **move the entire fucking federal government’s systems for TOP SECRET classified information** to adopt it [as fast as possible](https://www.nsa.gov/Press-Room/Press-Releases-Statements/Press-Release-View/Article/4615285/nsa-announces-post-quantum-cryptography-measures-to-safeguard-national-security/)?

I don’t know, man. That sounds kind of fucking stupid to me.

In my opinion, a more compelling conspiracy theory is that NSA already secretly built a quantum computer in [their Utah data center](https://en.wikipedia.org/wiki/Utah_Data_Center), and it’s already being used to spy on everyone today.

It’s a better fit than “ML-KEM is a NSA backdoor, the European cryptographers that designed it are all stupid, and the NSA is eager to move the US government onto a flawed or broken cryptosystem because SIGINT is all they care about, even at the expense of COMSEC, and they also can’t just *look for the plaintext* (which already works 99% of the time) they *gotta* be able to crack the encryption” which is the essence of the conspiracy theories that keep hitting my inbox today.

Conspiracy theories aren’t harmless. They create room for [grifters and hucksters](https://soatok.blog/2021/02/09/crackpot-cryptography-and-security-theater/) to deceive well-intentioned non-experts.

## In Closing

If you convinced yourself of a wrong conspiracy theory, that doesn’t mean you are dumb or unlovable.

NSA is certainly up to something. It’s their entire fucking job to be up to something. But it’d very unlikely to be a post-quantum backdoor no matter how much energy is expended on discussion boards and social media trying to convince people of that narrative.

Since neither you nor I have any information on what they’re actually up to, it’s tempting to look at the absence of evidence and fill-in-the-blanks and pattern-match based on what we already know today. That’s not stupidity.

Unfortunately, most of the proponents of these conspiracy theories will cherry-pick the most damning examples (i.e., Dual\_EC\_DRBG, through the lens of Snowden) while ignoring the messier and more nuanced aspects of the NSA’s \[mis\]conduct with the larger cryptography community.

So, please: Stay curious and humble. Better and more likely conspiracy theories exist, and may even be true, but you’ll never reach them if you short-circuit your analysis on stale low-hanging fruit and stop wondering.

![Soatok hugging a giant pink heart while making a blep face](https://i0.wp.com/soatok.blog/wp-content/uploads/2024/05/SoatokHeart.png?w=512&ssl=1)\
I believe in you.\
Art: AJ\_LovesDinos
