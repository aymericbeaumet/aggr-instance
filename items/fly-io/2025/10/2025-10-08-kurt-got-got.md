---
title: Kurt Got Got
link: https://fly.io/blog/kurt-got-got/
source: fly-io
published: 2025-10-08T00:00:00Z
updated: 2026-11-05T00:08:46Z
first_seen: 2026-11-27T00:01:05.630507547Z
content: extracted
html: 2025-10-08-kurt-got-got.html
remote_preview:
  url: https://fly.io/blog/kurt-got-got/assets/Kurt_Got_Got.webp
extra:
  thumbnail: https://fly.io/blog/kurt-got-got/assets/Kurt_Got_Got.webp
---

Author

![Thomas Ptacek](https://fly.io/static/images/thomas.webp)

Name

Thomas Ptacek

@tqbf

[@tqbf](https://twitter.com/tqbf)

![](https://fly.io/blog/kurt-got-got/assets/Kurt_Got_Got.webp)\
 Image by [Annie Ruygt](https://annieruygtillustration.com/)

We know. Our Twitter got owned. We knew within moments of it happening. We know exactly how it happened. Nothing was at risk other than our Twitter account (and one Fly.io employee’s self-esteem). Also: for fuck’s sake.

Here’s what happened: Kurt Mackey, our intrepid CEO, got phished.

Had this been an impactful attack, we would not be this flippant about it. For this, though, any other tone on our part would be false.

## How They Got Kurt

Two reasons: one, it was a pretty good phishing attack, and two, Twitter fell outside the “things we take seriously” boundary.

The phishing attack was effective because it exploited a deep psychological vulnerability in our management team: we are old and out of touch with the youths of today.

For many months now, we’ve had an contractor/intern-type-person Boosting Our Brand on Twitter by posting dank developer memes (I think that’s what they’re called). The thing about this dankery is that we don’t really understand it. I mean, hold on, we know what the memes mean technically. We just don’t get why they’re funny.

However, in pushing back on them, we’re up against two powerful forces:

1. The dank memes appear to perform better than the stuff we ourselves write on Twitter.
2. We are reliably informed by our zoomer children that we are too cringe to be trusted on these matters.

Here’s the phish Kurt got:

![A pretty-plausible Twitter alert](https://fly.io/blog/kurt-got-got/assets/phish.webp?2/3&center)

Diabolical. Like a scalpel expertly wielded against Kurt’s deepest [middle-aged-dude](https://theonion.com/cool-dad-raising-daughter-on-media-that-will-put-her-en-1819572981/ "") insecurity. Our ruthless attackers clinically designed this email to trigger an autonomic Kurt response: “oh, what the fuck is this, and why did we post it?”

ATO is cool-kid for “got owned”

I’m getting a little ahead of the story here. We knew our X.com account had suffered an ATO because a bunch of us simultaneously got another email saying that the [@flydotio](https://twitter.com/flydotio "") account’s email address now pointed to `achilles19969@gmail.com`. Our immediate response was to audit all accesses to the login information in [1Password](https://1password.com/ ""), to cut all access for anybody who’d recently pulled it; your worst-case assumption in a situation like this is that someone’s endpoint has been owned up.

Fortunately, nobody lost access for very long. I called Kurt to let him know why he was being locked out, and 5 seconds later, he’d [realized what had happened.](https://archive.is/6rVqf "") **Don’t click anything there.**

## Why It Worked

That’s the right question to ask, isn’t it? How could this have been possible in the first place?

Contrary to one popular opinion, you don’t defeat phishing by training people not to click on things. I mean, tell them not to, sure! But eventually, under continued pressure, everybody clicks. [There’s science on this](https://people.cs.uchicago.edu/~grantho/papers/oakland2025_phishing-training.pdf ""). The cool kids haven’t done phishing simulation training in years.

What you’re supposed to do instead is use phishing-resistant authentication. This is almost the whole backstory for [U2F, FIDO2](https://www.imperialviolet.org/tourofwebauthn/tourofwebauthn.html "") and [Passkeys](https://support.apple.com/en-us/102195 "").

Phishing-resistant authentication works by mutual authentication (or, if you’re a stickler, by origin- and channel-binding). Phishes are malicious proxies for credentials. Modern MFA schemes like FIDO2 break that proxy flow; your browser won’t send real credentials to the fake site.

there’s more to it than this, but, broad strokes.

This is, in fact, how all of our infrastructure is secured at Fly.io; specifically, we get [everything behind an IdP](https://fly.io/blog/soc2-the-screenshots-will-continue-until-security-improves/#what-soc2-made-us-do "") (in our case: Google’s) and have it require phishing-proof MFA. You’re unlikely to phish your way to viewing logs here, or to refunding a customer bill at Stripe, or to viewing infra metrics, because all these things require an SSO login through Google.

Twitter, on the other hand. Yeah, so, about that. You may have heard that, a few years back, there were some goings-on involving Twitter. Many of us at Fly.io [decamped for Mastodon](https://hachyderm.io/@flydotio ""), and [later to Bluesky.](https://bsky.app/profile/did:plc:j7herf6n4xiig2yg7fqdmkci "") There was a window of time in 2023-2024 where it looked as if Twitter might not be a long term thing for us at all.

† (to whom I sincerely apologize for having assumed they had been owned up and were the proximate cause of the hack)

As a result, Twitter had been a sort of legacy shared account for us, with credentials managed in 1Password and shared with our zoomer contractor†.

Which is why Kurt was in a position to pull credentials from 1Password and log in to members-x.com in response to an email from alerts-x.com.

Still: we could have dodged this attack with hygiene: Kurt complains that “x.com” is an extremely phishable domain, and, sure, but also: the 1Password browser plugin would have noticed that “members-x.com” wasn’t an “x.com” host.

## What Took So Long

The attacker immediately revoked all tokens and set up new 2FA, so while we were quickly able to reset our password, we couldn’t lock them out of our account without an intervention from X.com, which took something like 1 5 hours to set up.

(That’s not a knock on X.com; 15 hours for a 2FA reset isn’t outside industry norms).

We’re obviously making a lot of noise about this now, but we were pretty quiet during the incident itself (beyond just “We know. We knew 45 seconds after it happened. We know exactly how it happened. It’s just a Twitter thing.”)

That’s because, in the grand scheme of things, the attack was pretty chill: [a not-very-plausible crypto scam](https://archive.is/PTO2M "") that presumably generated $0 for the attackers, 15+ hours of `brand damage`, and extra security engineering cycles burnt on watchful waiting. Our users weren’t under attack, and the account wasn’t being used to further intercept customer accounts. At one point, the attackers apparently deleted our whole Twitter history, which, like, don’t threaten us with a good time. So we let it roll, until we got our account recovered the next morning.

## The Moral Of The Story Is

“Really the biggest takeaway for me is that Kurt reads his email.”

Obviously Kurt loses his commit access. The time comes in the life of every CEO, and now it comes for him.

Also, we’ll finally have a population sample for “incident response” in [our next SOC2](https://fly.io/blog/soc2-the-screenshots-will-continue-until-security-improves/ "").

Maybe we’ll post more on Twitter. Or maybe we’ll double down on Zoomer memes. I don’t know. Social media is really weird right now. Either way: our Twitter access is Passkeys now.

seriously don’t click anything on that page

If you were inclined to take us up on an “airdrop” to “claim a share” of the “token” powering Fly.io, the site is [still up](https://archive.is/PTO2M ""). You can connect your wallet it it! You’ll lose all your money. But if we’d actually done an ICO, you’d have lost all your money anyways.

Somebody involved in pulling this attack off had to come up with “own a piece of the sky!”, and I think that’s punishment enough for them.

Whatever you’re operating that isn’t behind phishing-resistant MFA, or, better yet, an SSO IdP that requires phishing-resistant MFA: that thing is eventually going to get phished. Dance around the clown-fire of our misfortune if you must, but let us be a lesson to you as well.

Next post ↑

[Corrosion](https://fly.io/blog/corrosion/)

Previous post ↓

[Litestream v0.5.0 is Here](https://fly.io/blog/litestream-v050-is-here/)
