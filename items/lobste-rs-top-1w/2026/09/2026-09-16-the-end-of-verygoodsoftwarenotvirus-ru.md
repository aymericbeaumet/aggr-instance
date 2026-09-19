---
title: The end of verygoodsoftwarenotvirus.ru
link: https://blog.verygoodsoftwarenotvirus.dev/posts/2026/09/12/the-end-of-verygoodsoftwarenotvirus-dot-ru/
source: lobste-rs-top-1w
published: 2026-09-16T16:58:43Z
updated: 2026-09-16T16:58:43Z
first_seen: 2026-09-19T21:30:23.395188495Z
authors:
- blog.verygoodsoftwarenotvirus.dev by verygoodsoftwarenotvirus
labels:
- web
summary: Comments
content: extracted
html: 2026-09-16-the-end-of-verygoodsoftwarenotvirus-ru.html
preview:
  file: 2026-09-16-the-end-of-verygoodsoftwarenotvirus-ru.preview-d47526b0df65.webp
  width: 221
  height: 256
  alt: a declined $199.99 charge to 101Domain, dated September 11 2026, with the reason "Incorrect card details"
  color: '#22212b'
images:
- source: https://blog.verygoodsoftwarenotvirus.dev/endofanera/payment_rejection.png
  original:
    file: 2026-09-16-the-end-of-verygoodsoftwarenotvirus-ru.image-3659503027ae.png
    width: 1058
    height: 1226
  color: '#1b1c25'
---

Last night, I got a notice that my domain renewal had been rejected by my payment provider (Privacy.com) because my domain provider (101Domain) tried to charge $200 for renewal of `verygoodsoftwarenotvirus.ru`.

![a declined $199.99 charge to 101Domain, dated September 11 2026, with the reason \"Incorrect card details\"](https://blog.verygoodsoftwarenotvirus.dev/endofanera/payment_rejection.png)

(note: the real issue behind this transaction was that the underlying card had expired, but it also would have rejected on amount)

If you run `whois verygoodsoftwarenotvirus.ru` right now, you get a few lines that, taken together, are the whole story:

```
created:       2014-11-10T00:34:07Z
paid-till:     2026-11-10T00:34:07Z
free-date:     2026-12-11
state:         REGISTERED, DELEGATED, UNVERIFIED
```

The `UNVERIFIED` in the last line is the problem.

## What actually changed

[Федеральный закон от 29.12.2025 № 569-ФЗ](http://kremlin.ru/acts/bank/52785) was signed at the end of last year, but the part that matters for me took effect on September 1st, eleven days ago as I write this.

At least, that’s what I’ve been able to surmise by asking AIs about it. I don’t speak Russian, and this doesn’t seem to be some major newsworthy move, but just a routine procedure. From what I can tell, the only way to keep my domain would be to establish some form of Russian connection. I could start a business, invest in one, attempt to emigrate, etc. I love committing to a bit, but that’s just a bit too far for my tastes.

So the practical effect is the same as the shorthand, even if the mechanism is more bureaucratic than a ban. My registration is paid through November 10th, 2026. I cannot renew it. On December 11th it goes free, and any person who can satisfy these new requirements can have it.

## The disclaimer I shouldn’t have to write

I have no connection to Russia. None. No family, no lineage, no investments, no heritage I’ve quietly been sitting on. I’ve never been there. I never learned the language and was never especially curious about it. I’m not an expert in any of its conflicts, but broadlly I do not support any government or leadership that indulges in war, and nothing about my owning this domain was ever a statement about any of that. I hadn’t realized until writing this post that I bought it eight months after Russia annexed Crimea.

I’ll say the other part too, since it’s honest: I’ve long known this was coming. I had briefly dual-deployed this blog to `verygoodsoftwarenotvirus.blog`, but that turned into a $60/year obligation I also wasn’t willing to tolerate (some parker has it now). The relationship between the two countries has been pointed in one direction for a long time, and anybody holding a novelty `.ru` has been holding it on borrowed time whether they thought about it or not. Mostly I didn’t think about it. What surprises me isn’t that it’s over. It’s that I got twelve years out of it.

## Why I bought it in the first place

In 2014 I was a security guard at a condominium complex in Austin, making $11 an hour. I’ve [written about this era before](https://blog.verygoodsoftwarenotvirus.dev/posts/the-story-of-porktrack/), so I’ll keep it short: I was newly married, I was not yet a software engineer, and I very badly wanted to become one.

What I had was a guardhouse computer and a lot of uninterrupted time. The machine belonged to a property management company and was intended for logging package deliveries. I had improperly installed Visual Studio and Android Studio on it, and between residents I wrote apps in C# and Java. I’d also installed FileZilla so I could upload stuff to my DigitalOcean droplets.

By the end I had quietly assembled, on a computer whose actual job was to record when the plumber showed up and who got what package, a complete and entirely unauthorized development environment.

The programs were not good. I wrote one that generated a random math problem and offered you a multiple-choice list of answers. I wrote one that generated random colors. That’s it, but I had made the computer do something, and at 2 AM in a guardhouse that felt enormous.

## Giovanni

My friend Giovanni was an actual working software engineer, which made him the closest thing I had to a professional peer, and the unwitting recipient of everything I made. I would compile these little apps and email him the binaries in zip folders. He used to joke, more or less, that

> for all I know you’ve figured out how to make a virus and are sending me one

So I did the only reasonable thing and registered `[[email protected]](https://blog.verygoodsoftwarenotvirus.dev/cdn-cgi/l/email-protection)`, so that the next binary would arrive with reassurance attached.

And then, because a bit is only worth doing if you’re willing to overcommit to it, I bought the domain. The entire plan was to host a zip file at `verygoodsoftwarenotvirus.ru` and tell Giovanni to download it.

I don’t remember whether I ever actually did it. I remember buying the domain to make it possible.

## What a joke turns into if you leave it alone long enough

I started working as a software engineer the following May. The handle came with me. It’s my GitHub username. It’s my handle everywhere I do anything programming-adjacent. It is, at times, more recognizable as me than my legal name is in most of the rooms I’m in.

Whenever people’d ask me about the username, I’d get to say “I own the `.ru`, too.” and it always makes them laugh. I have never had to explain the joke. The joke explains itself, and then the domain is the punchline that proves I meant it. It’s the single best return on investment I have ever gotten for what I think was about eight dollars.

## So: two domains

I’ve bought `verygoodsoftwarenotvirus.dev`, and this blog now publishes to both it and the `.ru` out of the same pipeline, for as long as the `.ru` keeps resolving.

When the registration lapses in December, the domain stops being mine, and so does the evidence that it ever was. Somebody will pick it up and probably put scam ads for boner pills on it. Sorry in advance.

So, for the record, here is [a snapshot of this blog from October 16th, 2024](https://web.archive.org/web/20241016063422/https://blog.verygoodsoftwarenotvirus.ru/), living at the address it was born at. Proof it was real, and proof it was mine, filed somewhere no registrar can revoke.

There’s something appropriately on-brand about a story bracketed by two machine-generated records. A whois entry at one end and a Wayback capture at the other. I don’t think I know how to be sentimental in a format that isn’t plaintext.
