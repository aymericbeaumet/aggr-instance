---
title: Credit card skimming scripts using WebRTC found on popular shopping sites
link: https://www.infohawk.com/news/2026-11-27-black-friday-webrtc-skimmer
source: hnrss-org
published: 2026-11-27T15:40:31Z
updated: 2026-11-27T15:40:31Z
first_seen: 2026-11-27T22:38:26.539383534Z
authors:
- scanr
content: extracted
html: 2026-11-27-credit-card-skimming-scripts-using-webrtc-found-on-popular.html
remote_preview:
  url: https://www.infohawk.com/_app/immutable/assets/2026-11-27-black-friday-webrtc-skimmer.BQAmEGg5.png
  alt: Credit card skimming scripts using WebRTC found on popular shopping sites — InfoHawk News
---

[All news](https://www.infohawk.com/news)

November 27, 2026

All the sites we contacted took action; but the larger consumer web ecosystem should be prepared to mitigate these attack vectors

A card-skimming script was planted on the online stores of Lascana, Samsonite, American Tourister, Revolve and FWRD in the week before Black Friday. After our reports to affected companies, all have had the script removed (on November 24th or 25th) ahead of Black Friday. We observed it in action on one of the five websites: Lascana, where it sent our test card's details to a 3rd party. We believe the script was built to take a card number, expiry and security code, along with a user's name, address, phone and email, and send them to malefactors as a user entered their payment credentials. It did this before the store checked the payment, so a card could be taken even if the order was declined or cancelled, and nothing would look wrong to the shopper. Across all of these sites, we do not know how many, if any, shopper details or payment credentials may have been captured.

![Illustration: a Black Friday checkout order summary with a Place order button, and a dotted line from the button to a red box labelled 'Not in your cart' listing card number, expiry, CVV, name, address, phone and email. Text: Is there extra code on your payment page? A skimmer hidden in the checkout of consumer websites attempts to skim card details over WebRTC, making detection more difficult.](https://www.infohawk.com/_app/immutable/assets/hero.Bde-_Sb9.png)

## Which sites were affected

| Consumer site                                                                         | Delivery method                     | Status at time of publication                                                       |
| ------------------------------------------------------------------------------------- | ----------------------------------- | ----------------------------------------------------------------------------------- |
| Lascana [www.lascana.com](https://www.lascana.com/)                                  | Altered Quorum\* marketing pixel    | Both Lascana and Quorum replied and removed the code on the 24th, see details below |
| Samsonite [shop.samsonite.com](https://shop.samsonite.com/)                          | Edited Seers† cookie-banner setting | Neither party replied to us, but the code was removed on November 24th at 14:25 UTC |
| American Tourister [shop.americantourister.com](https://shop.americantourister.com/) | Edited Seers† cookie-banner setting | Neither party replied to us, but the code was removed on November 24th at 14:25 UTC |
| Revolve [www.revolve.com](https://www.revolve.com/)                                  | Altered file on Revolve's own CDN   | Revolve confirmed removal on November 25th                                          |
| FWRD [www.fwrd.com](https://www.fwrd.com/)                                           | Altered file on Revolve's own CDN   | Revolve confirmed removal on November 25th                                          |

On November 24, we notified all of the named parties in this article and others, including: Amazon Web Services about the storage buckets serving the script, Cloudflare about the services the malefactor used, the domain registrars, the pixel vendor, the consent-banner provider and the affected retailers and platforms. Lascana replied to us and removed the Quorum pixel from their site by November 24th at 15:48 UTC. Quorum confirmed the identified malicious code was removed from their pixel tracker, and they restored the clean marketing pixel as of November 24th at 21:09 UTC.

Revolve provided this statement on behalf of REVOLVE and FWRD: *“On November 25, InfoHawk notified our team of a potential security issue impacting mobile and tablet versions of our websites which we resolved the same day. We appreciate the outreach, and are actively investigating the issue to determine any potential impact.”*

\* Quorum is an ad-attribution platform that links ad views to store visits and online sales ([quorum.inc](https://quorum.inc/)).

† Seers is a cookie consent management platform and data privacy compliance company ([seers.ai](https://seers.ai/)).

## How the attack works

This attack uses WebRTC, the technology that allows you to make video calls in your web browser. WebRTC lets a web page open its own channel to another party and pass data (e.g. audio/video packets) over it. It has likely been used (both to load the skimmer and to send the stolen card details out) because security rules and monitoring tools often do not watch this communications channel as closely. For example, [Meta briefly used WebRTC in a creative way in 2025](https://www.techradar.com/pro/security/meta-halts-phone-and-browser-tracking-tools-after-researchers-expose-user-tracking), according to researchers [looking at](https://www.cs.ru.nl/masters-theses/2026/R_te_Wechel___Misuse_of_WebRTC_on_the_Web_A_Measurement_Study.pdf) the technology as a compromise vector. This form of attack appears to target mobile devices, and we only saw the server enable the skimmer on checkout pages, so desktop checks and homepage scans would likely not see it. The skimmer's traffic appeared to run through Cloudflare's relay, so it would blend in with the WebRTC traffic and hide its true endpoint.

### The skimmer and card details both traveled over WebRTC

How the kit is designed to work on a store page, in 7 steps. Only step 4 is an ordinary web request, which a `connect-src` rule can block; steps 5 to 7 run over the WebRTC data channel, outside `connect-src`.

1. Store page

   ### Bootstrapping the attack

   The loader gets onto the page through a consent-banner setting, a retailer CDN file, a shared marketing pixel or the store's own HTML.

2. Store page

   ### Loading scripts from S3 buckets

   It loads `Insds.js` or `gtage.js` from an Amazon S3 bucket.

3. Store page

   ### Targeting the attack to mobile checkouts

   It runs only on small or touch screens. The Samsonite version runs on `/checkout` only.

4. Web request · connect-src can block it

   ### Establishing the WebRTC connection

   `GET /_cf/ice` fetches the relay details, and `POST /_cf/rel` sends the WebRTC offer, to a brand-named `northbridgeads[.]com` host.

5. WebRTC data channel · outside connect-src

   ### Data exchange via TURN relay

   Every exchange that returned relay details used `turn.cloudflare.com`. The malefactor's answers listed only Cloudflare relay addresses; through that relay it sends code and receives data.

6. WebRTC data channel · code in

   ### Attack code is loaded and executed

   The skimmer arrives in chunks over the channel, is checked against a SHA-256 digest, then run with `eval`.

7. WebRTC data channel · data back

   ### Data goes back the same way

   At Lascana's checkout‡, the skimmer sent our test card details this way.

‡ This was the only example of live card details being sent off-site that we captured.

### Worried that your card was compromised?

Check your recent statements for charges you don't recognize, especially on debit cards. Call your bank or card issuer right away, report the fraud and ask for a new card number. Follow up with a written dispute within 60 days of the statement that shows the charge.

Our understanding of US federal law is that your liability for unauthorized credit card charges is capped at $50, and is generally zero if only your card number was stolen, as in a skimming attack like this one. Debit cards have weaker protections that depend on how quickly you report, so act fast. For more information, see “[Additional resources](https://www.infohawk.com/news/2026-11-27-black-friday-webrtc-skimmer#resources)” below.

## What site owners should immediately do

On every page that takes payment, set a `connect-src` rule that lists only the hosts you use.

That blocks the two web requests the script needs before WebRTC opens. Without them the skimmer never arrives, whichever file was tampered with.

## What security teams should look for

We saw the skimmer delivered in three different ways across five websites run by different companies, and all of them appear to rely on the same attacker infrastructure. That makes it likely that other sites are affected too, so we encourage online retailers to check their own sites for the signs below.

1. **Script sources:** search page source, tag managers, consent-banner settings and CDN files for scripts from `<brand>-static.s3.amazonaws[.]com` (files `Insds.js`, `gtage.js`). We believe this is the malicious code.
2. **Tracking pixels:** if you load `qscripts.s3.amazonaws[.]com/pscript.js`, check its last line for one of the above CDN buckets, and if present, remove the tracking pixel. We recommend reviewing all similar tracking pixels and their exposure to sensitive fields or actions on your website.
3. **Seers banners:** If you have integrated Seers, open your banner config (`cdn.seersco.com/banners/<account>/<domain>/cb.js`) and check that every URL field holds only a URL. In the affected configs, the privacy-policy link field also held a hidden image, whose `onerror` event handler loaded the malicious code.
4. **Checkout traffic:** on a phone-sized touch screen, at the payment step, look for `/_cf/ice` and `/_cf/rel` requests to `*.northbridgeads[.]com` and a relay-only WebRTC connection used by the malicious code. Desktop checks miss it.
5. **Strings:** `KghRtc`, `rtc-signal-v1` and `quickfetch` are referenced in the malicious code and may warrant further investigation if present.

## The industry is not ready for the scale of supply chain attacks coming

This attack takes advantage of a few critical design decisions in websites that, if improperly configured, allow malicious actors to broadly deploy and trigger exploits across the open web.

- Websites load third-party code, often through a tag manager, from servers they don't control. The vendor can change that code at any time, and it can pull in further third-party code the site never reviewed.
- That code runs as an ordinary script with the same rights as the site's own code. It can read and rewrite the page, watch what users type, and load more code.
- Login, personal-information and payment pages often load the same marketing scripts as the rest of the site. Card fields should sit in an isolated payment frame, and third-party tags should be kept off these pages entirely.

Loading third-party tracking pixels on checkout, payment and login pages is far too common a practice today. Each pixel can run whatever code its vendor's file holds, including a skimmer, and whoever controls that file can change it without the site noticing. Every site should review which tracking pixels load on pages that take payment or sign-in details, who can change those files, and whether they need to be there at all.

A similar weakness was exploited at scale on 16 November 2026. An attacker reportedly used a flaw in [Optimizely's Web Experimentation](https://krebsonsecurity.com/2026/11/attackers-used-optimizely-to-a-b-test-malware-on-top-websites/) product to add malicious code to customers' projects, and [Optimizely's](https://www.optimizely.com/field-notes/articles/web-experimentation-security-incident-update) own servers delivered it to visitors of 178 organizations' sites, including Zoom, the BBC and CNN, with no change to those sites' own code.

## Additional resources

- [**Mastercard, "What is digital skimming? Your guide to staying safe while shopping online"**](https://newsroom.mastercard.com/news/perspectives/2024/what-is-digital-skimming-your-guide-to-staying-safe-while-shopping-online). Published October 10, 2024, it explains how criminals sneak malicious code into e-commerce websites to steal the payment data of customers who use their cards there.
- [**Visa, "Why hackers are prioritizing account data attacks in digital payments"**](https://corporate.visa.com/en/sites/visa-perspectives/security-trust/account-data-attacks.html). This describes how hackers inject malicious code onto the checkout pages of merchant websites to harvest payments account data and personally identifiable information (PII) entered by customers.
- [**Visa Zero Liability**](https://www.visa.com/en-us/personal/security/zero-liability-policy.html)
- [**Mastercard Zero Liability**](https://www.mastercard.com/us/en/personal/protection-and-security/zero-liability-protection.html)
- [**CISA/FBI E-Skimming fact sheet**](https://www.cisa.gov/sites/default/files/publications/NCSAM_ESkimming_2020.pdf). It explains how cyber criminals introduce skimming code on e-commerce payment card processing web pages to capture credit card and personally identifiable information and send the stolen data to a domain under their control. It's from 2020, but it's still the clearest government explainer.
- [**FBI Internet Crime Complaint Center**](https://www.ic3.gov/)
