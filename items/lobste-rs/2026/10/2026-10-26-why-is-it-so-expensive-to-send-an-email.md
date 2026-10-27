---
title: Why is it so expensive to send an email?
link: https://evancz.dev/why-is-it-so-expensive-to-send-an-email
source: lobste-rs
published: 2026-10-26T13:25:41Z
updated: 2026-10-26T13:25:41Z
first_seen: 2026-10-27T20:37:14.562886086Z
authors:
- evancz.dev via wolfadex
labels:
- email
summary: Comments
content: extracted
html: 2026-10-26-why-is-it-so-expensive-to-send-an-email.html
---

Sending an email is essentially the same as making an HTTPS request. You open a TCP connection to port 25, you do a little SSL handshake, and you send some data through. Most personal computers out there can easily do this thousands of times per second, so **how can companies charge $15 to send 1000 emails?!** Why do I need some 3rd party company involved at all?! I used to work at Google specifically on GMail, and nothing from my little corner of the product on the “tabbed inbox” team led me to a clear answer...

**Unfortunately for me, I was determined to figure it out for myself!**

I mostly program with Haskell, and I call C code as necessary for things like OpenSSL and SQLite. I already had a setup for making HTTPS requests from my Haskell code with OpenSSL. I had code for working with HTTP 1.1 messages. How hard could it be to adapt this to email? To my surprise, it turns out that the actual “sending” part is not that hard. The real problem was ultimately out of my hands...

## Simple Mail Transfer Protocol

Just like the original plaintext Hypertext Transfer Protocol (HTTP 1.1), there is a correspondingly goofy plaintext protocol for sending emails called Simple Mail Transfer Protocol (SMTP). The original version of the protocol involves sending a bunch of messages back and forth and back and forth between the sender and receiver. It looks kind of like a little conversation. Here is an example where I start my messages with keywords like `HELO` and `MAIL FROM` and the receiver starts her messages with status codes like `220` and `250`:

```
(Evan opens the TCP connection)
220 mail.example.com
HELO evancz.dev
250 Hello evancz.dev, I am glad to meet you
MAIL FROM:<devnull@evancz.dev>
250 OK
RCPT TO:<alice@example.com>
250 OK
DATA
354 End data with <CR><LF>.<CR><LF>
From: "Evan from Elm" <devnull@evancz.dev>
To: "Alice from Cryptography" <alice@example.com>
Date: Wed, 21 Oct 1981 8:33:40 +0100
Subject: Big Fan!

Hello Alice from Cryptography,

I loved your collaboration with Rivest, Shamir, and Adleman
in 1977. I have been eagerly awaiting the invention of email,
so that I could express my admiration through this plaintext
transfer protocol!

Best,
Evan from Elm
.
250 OK: queued as 12345
QUIT
221 Bye
(Alice closes the connection)
```

So to send a single email, you originally had to do SEVEN roundtrips! And TLS did not exist at that time, so you did not even get any encryption after all those roundtrips!

That said, it is not that hard to send and receive a bunch of goofy plaintext, so I set it up and sent an email to myself. It worked! But... it showed up in spam.

## Wait, who sent that message?!

The original protocol has a very serious flaw. **How does `mail.example.com` know that I am `devnull@evancz.dev` and not an imposter?** The original protocol lets you just claim whatever sender you want, so anybody in the whole world could easily spoof the domain and sender.

So these days you need to add some `TXT` records to the DNS for your domain. This way the receiver can go look up some public encryption keys for `evancz.dev` and confirm that whoever sent the email at least has access to the private key.

At this point I started wondering, wait, how do you prevent someone from spoofing the DNS records? Like what if [the NSA and AT&T were collaborating](https://www.propublica.org/article/nsa-spying-relies-on-atts-extreme-willingness-to-help) or what if [the CIA had a history of creating companies like Crypto AG to backdoor cryptographic systems from the very beginning](https://www.washingtonpost.com/graphics/2020/world/national-security/cia-crypto-encryption-machines-espionage/)? It seems like a problem... But hey, I just want to send an email and it looks like all you have to do is set up these DNS records.

So I set up a private key, added some DNS records, ran my emails through some OpenSSL functions, and I started sending emails again!

Now my emails were showing up in my inbox in GMail! Heck yeah! The technical details were not hard, only sort of annoying to figure out. So why does it cost $15 to send 1000 emails?!

When the database backend for Elm went into private alpha, I decided to try sending the transactional emails myself. Just some Haskell and C code making TCP connections! My thinking was that if I ever set up a hosting service to support Elm development, maybe it would be cool to handle emails as well. The whole goal of Elm is to filter out all the irrelevant details and let the programmer focus on the actually meaningful parts, so it seemed like there might be something there!

Okay, but then I noticed that the emails were *only* showing up in GMail. People who used Microsoft Outlook were not even seeing them in spam...

## Old and Goofy Spam Filters

Once someone adds the DNS records to verify their identity, it is still possible for them to just start blasting out spam. I was not sending spam, but I was now interacting with a baroque mosaic of spam detection filters that sometimes work in funny ways.

Each email inbox provider has different algorithms for determining “the reputation” of the sender. Some inboxes are able to mostly assess the reputation of the sender’s domain. “Are the emails from this domain good?” Other inboxes seem to care more about the particular IP address. “If one domain on a given IP address is bad, maybe other domains on that IP address are bad too!” Other inboxes seem to really care about the IP address **block**. “If someone is sending spam from `100.0.0.0`, then maybe the whole `100.0.*.*` block is a problem!”

So it is possible that an inbox like Microsoft Outlook really cares about IP address **blocks**. Maybe they do not have a very fine-grained way of assessing individual IP addresses, or maybe this is just a good for a filter for “big business” users. I have no idea, and I have no special inside knowledge. The point is that this appears to be true for some of the many many inbox services out there. Even if it is not true this month, it may be true next month as the developers at Yahoo Mail, Zoho Mail, Proton Mail, etc. etc. adapt to whatever spammers are getting up to recently.

I host my servers with a smaller cloud provider, so I have access to whichever IP address blocks they offer. I can get a fixed IP address and treat it nicely, but again, some email inboxes seem to really care about the entire IP address **block**. That means that if *anyone* is sending spam through my cloud services provider, my whole IP address **block** may get blacklisted.

So finally we come to the reason why sending emails costs so much.

## My Personal Conclusion

Actually setting up the code to send emails is a relatively doable one-time thing. From a technical perspective, it is essentially the same as sending an HTTPS request.

But to deliver to all different email addresses, you need to be trusted by inbox providers with the oldest and goofiest spam filters. Since some inboxes seem to really care about IP address blocks, **this means you probably need to get your own IP address block.** And from there, you need to send a pretty high volume of emails through it per day.

My emails went through to certain inbox providers just fine, so it must be *possible* for them to avoid heavily relying on IP address block blacklists. The particular feature I worked on at Google was the “tagged inbox” where emails get sorted into primary, promotions, social, updates, and forums. I never really talked to the spam people down the hall, but maybe things like the human categorization from tagged inbox helps them get a better signal on domain names such that they can rely less on the IP address block blacklists? I honestly have no idea. It was a big office, and the “more exciting” gossip mostly came from the other side of the building where the Google+ people were developing a Facebook killer in the era of “social search”.

In any case, this is where my personal exploration ended. Hopefully it helps readers who have never tried to send emails from scratch, and I will be curious to hear from the collective “spam people down the hall” if there is anything I got wrong! (I am sure there is!)
