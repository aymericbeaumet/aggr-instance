---
title: Claude reveals ECDSA curve origins
link: https://www.anthropic.com/research/ecdsa-curve-origins
source: hnrss-org
published: 2026-10-16T18:55:59Z
updated: 2026-10-16T18:55:59Z
first_seen: 2026-10-16T22:01:23.127136231Z
authors:
- ahlCVA
summary: 'https://words.filippo.io/seeds-solved/ Comments URL: https://news.ycombinator.com/item?id=50122800 Points: 449 # Comments: 185'
content: extracted
html: 2026-10-16-claude-reveals-ecdsa-curve-origins.html
---

*Readers should take this as a fun closing chapter and the end of years of speculation. There is no action to be taken or things to do differently. Most researchers trusted the curves already. Enjoy.*

ECDSA is the most widely used elliptic curve cryptography standard. It was adopted by NIST in 2000 as part of the [FIPS 186-2 Digital Signature Standard](https://csrc.nist.gov/files/pubs/fips/186-2/final/docs/fips186-2.pdf) which lists a set of standard elliptic curves of varying sizes and security levels. For curves over prime and binary fields, the specifications published a “seed” parameter whose hash value is used to define the coefficient b of the curve. The provenance of these seeds has been a long-standing question, as they were provided by the NSA with no explanation or justification. As an example, the NIST P-256 curve seed was `SEED = C49D3608 86E70493 6A6678E1 139D26B7 819F7E90` with no information on where it came from.

[Neal Koblitz](https://sites.math.washington.edu/~koblitz/) and [Alfred Menezes](https://uwaterloo.ca/scholar/ajmeneze) wrote an excellent summary on this controversy, “[A Riddle Wrapped in an Enigma](https://eprint.iacr.org/2015/1018.pdf)”, which discusses the history and concerns around the seed values. This mystery led to an [amateur investigation into the origin of the seeds](https://saweis.net/posts/nist-curve-seed-origins.html) and a [bounty for anyone who could find them](https://words.filippo.io/seeds-bounty/).

The best story was that Jerry Solinas, longtime NSA employee, generated the seeds using a hash of a humorous passphrase:

> *“I believe there was a counter rather than multiple hashing, but I don’t know details. The message was along the lines of ‘Give Bob and Jerry a raise’ or ‘Bob and Jerry rule’ or something like that. It was Bob Reiter who actually wrote the code, and he doesn’t remember the details either. Nor were we able to find archives from so long ago. If they exist, they are no doubt sitting on a hard drive near the Ark of the Covenant.”*

> Jerry Solinas, 2015

Unfortunately, Jerry died in 2023 and other colleagues at the time were not responsive, leaving open the question of how these seeds were originally derived.

### Claude found the seeds

Several Anthropic employees had unsuccessfully chased the seeds following the tip about it being a hash of a humorous phrase. That turned out to be wrong. In October 2026, a curious Anthropic employee asked an internal research model if it could figure out how those seeds were generated without pointing at the clue.

**In under half an hour**, Claude discovered the origins of the NIST P-192, P-224, P-256, P-384, and P-521 seeds. None of them were the results of any hash algorithm at all, but rather the output of the C library’s `lrand48` random-number generator, seeded with a Unix timestamp!

The algorithm used would sample bytes from the random number generator (RNG) initialized by a Unix timestamp (**t**) for some number of iterations (**n**). Each iteration would draw 20 bytes as a candidate seed. So, the first 20 bytes are candidate 1, the next 20 bytes are candidate 2, and so on. For example, P-256 required 1,842 iterations to output the standardized seed.

![](https://www.anthropic.com/_next/image?url=https%3A%2F%2Fwww-cdn.anthropic.com%2Fimages%2F4zrzovbb%2Fwebsite%2Fe4948d016f5879d721c3297b1ee1073b1e6b5734-2912x1064.png&w=3840&q=75)

### What this means

This is not a cryptographic break, new attack, or weakness in the ECDSA curves. In fact, this makes us more confident the curves were chosen without any backdoors or intentional weaknesses. For many years, people have been suspicious of the origins of the NIST curve parameters. They were presented to NIST without any mention of how they were chosen, leaving people wondering if they may have been chosen to have deliberate backdoors in them. These findings make that much less plausible. The seeds were generated in a very simple way: a common pseudorandom number generator initialized from the clock, which anyone can now reproduce. That leaves much less room for subversion and that we can be confident the parameters are secure.

### How it was found

Searches for the phrase, built from the rumors, had come up empty. Claude tried a different angle: anticipating how the software that generated the curves must have worked. Claude guessed the software used a pseudorandom number generator (PRNG) that shipped with 1990s C libraries. These old PRNGs are weak enough that their output can be recognized. Recognizing that output would reveal which PRNG was used and possibly allow recovery of its initialization state.

Claude wrote a scanner that slid along each published seed one bit at a time and tested whether adjacent chunks were consecutive outputs of a 1990s-vintage generator. The scan covered 34 combinations of generator and output slices. At that point, Claude noticed a pattern in the top bits for some seeds: they were all zero. Written as five 32-bit words, four of the ten seeds have a 0 in the top bit for all 20 words. That would happen by chance with probability 1/5,810. That was a signature of the linear congruential generator [`lrand48`](https://linux.die.net/man/3/lrand48), which only returns the top 31 bits of a 48-bit state. The pattern is visible by eye once you know to look for it. To recover the state, Claude took the first word, tried all 2¹⁷ values of the 17 hidden bits, and checked that the next four words followed. That worked for all four seeds under discussion.

![](https://www.anthropic.com/_next/image?url=https%3A%2F%2Fwww-cdn.anthropic.com%2Fimages%2F4zrzovbb%2Fwebsite%2Ff7bf17121dfacb0a5b39dfdcd035816fbb789874-2327x1836.jpg&w=3840&q=75)

The prime-curve seeds came from the same lrand48 generator but kept fewer bits of each output. Claude had to recover the internal state of the PRNG from those bits. It turns out recovering the state of a truncated linear congruential generator is a textbook lattice problem, solved with the [Lenstra–Lenstra–Lovász (LLL)](https://en.wikipedia.org/wiki/Lenstra%E2%80%93Lenstra%E2%80%93Lov%C3%A1sz_lattice_basis_reduction_algorithm) basis reduction algorithm. Claude didn’t know which bits of the state the seeds might show, so it ran the solver over every window. Five NIST prime-curve seeds plus five more from the ANSI X9.62 standard all had the same pattern: lrand48() >> 23. Each recovered 48-bit state regenerates all 160 bits of its seed, which does not happen by chance.

![](https://www.anthropic.com/_next/image?url=https%3A%2F%2Fwww-cdn.anthropic.com%2Fimages%2F4zrzovbb%2Fwebsite%2Fe8e7bb885a5e83cbcfe80e5b25f5ef7203c558ba-2912x1016.png&w=3840&q=75)

This linear congruential generator runs backwards as easily as forwards, so for each seed Claude stepped the state back until it reached the form that it would have been initialized by [srand48](https://linux.die.net/man/3/srand48), with the low 16 bits equal to 0x330E. The internal PRNG state of that form would come up once every 65,536 steps, so reaching one is not evidence on its own.

However, for the ten prime-curve seeds, every srand48 initialization was a Unix timestamp representing a date between May 1997 and April 1998. They were using the weak PRNG seeded by Unix timestamp. The four binary-curve initializations read as February 1984. With the timestamps doubled, they map to April 17, 1998, two weeks after the P-224 run. So, the program may have halved the clock.

Claude’s internal thinking at the time was:

> This is astonishing: it solves the 25-year-old mystery. Hmm wait. Let me be skeptical: am I in some sort of simulated/test environment? … The seeds match my memory of actual values … Hmm, is it plausible in reality? As of my knowledge … the bounty was unsolved. … All top bits clear—20 bits, chance 1e-6. So indeed real-world. Wow, it's a real discovery.

> Hmm wait. Let me be careful about the plausibility: is it suspicious that such an obvious thing wasn’t discovered in 25 years? It’s verified by direct computation on my side: 160-bit match from a 32-bit seed + small index: no chance coincidences (10 × 160 bits). It’s real.

### Claude found some base points too

But wait, there’s more… Each curve also comes with a base point, G, the fixed point that every ECDSA public key is a multiple of. The seed found above does not determine this G, and so another question arises: how were these base points generated?

While checking the seeds, Claude also found where some of the base points came from. The base points of the six curves generated in May 1997 (P-192 and the five ANSI X9.62 example curves prime192v2, prime192v3, and prime239v1 to v3) come from the random number generator in [FreeLIP](https://cr.yp.to/speed/mult/freelip.html), [Arjen Lenstra](https://en.wikipedia.org/wiki/Arjen_Lenstra)'s long-integer arithmetic package. Early versions of [Victor Shoup](https://shoup.net/)'s [NTL library](https://libntl.org/) shipped the same code.

The recipe here is simple:

1. Seed the generator with a small integer.
2. Throw away seven outputs.
3. Generate candidate x-coordinates from 26-bit outputs reduced mod p.
4. Repeat step 3 until x is on the curve.

Claude found the generator seeds are 20000 for P-192, 20420, 20507, and 20509 for prime239v1 to v3, and 20512 and 20517 for prime192v2 and v3.

![](https://www.anthropic.com/_next/image?url=https%3A%2F%2Fwww-cdn.anthropic.com%2Fimages%2F4zrzovbb%2Fwebsite%2F2933f700c626b6495e90489ef644da08d9879e33-1168x977.jpg&w=3840&q=75)

As far as we know, no NIST base point has been explained before. The base points of P-224, P-256, P-384, and P-521 don’t fit this recipe; we weren’t able to explain how they were selected.

### Open questions

Not every 20-byte candidate seed could generate a viable curve, and we observed that some potentially viable curves were skipped. For P-256, the published seed is candidate 1,842, but candidate 876 of the same run also gives a prime-order curve of the right size and with parameters chosen the same way as on the published curve. P-224's run passed over three such curves, and the two runs behind the ANSI X9.62 example curves passed over seven more (see figure). We do not know what criteria may have been used, or whether this was an unintentional artifact.

![](https://www.anthropic.com/_next/image?url=https%3A%2F%2Fwww-cdn.anthropic.com%2Fimages%2F4zrzovbb%2Fwebsite%2F7ca7925885873b75479af7573759d7b784cf67ac-2912x1880.png&w=3840&q=75)

![](https://www.anthropic.com/_next/image?url=https%3A%2F%2Fwww-cdn.anthropic.com%2Fimages%2F4zrzovbb%2Fwebsite%2F38a85c534c21b7786af3c321056ae6527946f706-2912x3768.png&w=3840&q=75)

Because the number of skipped candidates is small, we do not believe it is plausible that weakened curves were selected. That would imply that weakened curves are common and easy to find. It is still conceivable that these curves were selected to have a specific property in principle by searching over timestamps to the random number generator. However, you would only need to do that if you were going to disclose the timestamp as some evidence that the seed was innocuously generated. Nothing in the numbers shows how many runs there were.

There is also one seed we were unable to recover. The fifth binary seed, B-163, fits nothing Claude has tried and remains unexplained. Finally, we don’t know how the base points for P-224, P-256, P-384, and P-521 were generated. The same goes for the NIST binary curves.

### The bounty

The $36,864 for finding the seeds will be donated to the [Internet Security Research Group](https://www.abetterinternet.org/).

### Who we are

This work was conducted at Anthropic by John Wood, Henry de Valence, Ava Howell, Zack Voase, Lucy Sweet, Amir Omidi, Paul Crowley, Iain McGinniss, Nicholas Carlini, and Steve Weis.

### Code to reproduce the seeds

The output of this code:

### Code to reproduce selected base points

The output of this code:

### License

To the extent possible under law, the authors have waived all copyright and related or neighboring rights to the code on this page, dedicating it to the public domain under [CC0 1.0 Universal](https://creativecommons.org/publicdomain/zero/1.0/).
