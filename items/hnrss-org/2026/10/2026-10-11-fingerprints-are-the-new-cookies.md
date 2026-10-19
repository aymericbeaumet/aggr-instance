---
title: Fingerprints are the new cookies
link: https://royfish.dev/fingerprints-are-the-new-cookies
source: hnrss-org
published: 2026-10-11T22:33:39Z
updated: 2026-10-11T22:33:39Z
first_seen: 2026-10-12T05:46:15.112348298Z
authors:
- royfish
content: extracted
html: 2026-10-11-fingerprints-are-the-new-cookies.html
extra:
  comments_url: https://news.ycombinator.com/item?id=50048642
  num_comments: 107
  points: 227
---

**You're protected from the methods I use here.** You disabled JavaScript, so this fingerprinting trick can't run at all. You're still vulnerable to network-based methods. Read on to see what it does to everyone else.

## From now on I’ll call you:

…

computing…

Try refreshing the page. Clear the cookies. Connect to a VPN.

You’re still the **…**.

Now open it on your phone or some other device and your nickname will be different and just as consistent.

Is it different in incognito? Good. But it’s always the same? Bad news.

**Imagine this:**

Most websites you visit get paid to show ads and indirectly run my script that lets me know you’re the …. I can use it to decide what ads you’ll see. But more importantly, if you’ve made an account on that website, I can offer to buy your data - whatever they can share under their privacy policy: your name, activity, conversations with AI…

Pretty soon I’d know more about you [than your friends do](https://www.pnas.org/doi/10.1073/pnas.1418680112) - perhaps more than you know yourself.

### “So what?” - you say.

> “I have nothing to hide. And I prefer my ads to be relevant… sometimes they show me exactly what I need! And the data is anonymised anyway…”

See, the thing is, it’s not about you at all. It’s not really about the ads anymore either. Nobody cares about you. And if the police or your government happen to care, I’ll gladly take their money too.

I look at all this data and I don’t see people. I see patterns. Patterns emerging out of patterns. A living, digital fractal state of the economy, society and human psyche. The crystal ball of the 21st century and it’s made of silicon! **That’s** the product.

**And to the highest bidder it goes!**

What does that make you then? A gear? An ox? A volunteer?

* * *

Remember the big fuss about third-party cookies? It was so big that the EU had already rolled out one of its greatest inventions - consent banners.

If EU ships faster than tech companies, that can only mean the tech companies are sitting on their hands. And one of those tech companies happens to hold two-thirds of the browser market.

What was the play?

Let’s give it up for Google - the biggest ad company on Earth!

**ACT I. PRESSURE** - [“PRIVACY IS PARAMOUNT TO US, IN EVERYTHING WE DO.”](https://blog.google/products-and-platforms/products/chrome/building-a-more-private-web/)

It’s 2019 and the cookies are still hot. It’s become bad PR and Safari and Firefox already block them.

A year later Google pledges: we will destroy cookies!! And establish a new, privacy-first reign.

**ACT II. DISTRACTION** - [“INSTEAD OF DEPRECATING THIRD-PARTY COOKIES, WE WOULD INTRODUCE A NEW EXPERIENCE”](https://privacysandbox.google.com/blog/privacy-sandbox-update)

5 years later: people are now preoccupied with far greater worries than cookies, and the solutions Google has developed (Privacy Sandbox) kinda suck and have raised antitrust concerns so nobody uses them.

“You know what, let’s just keep the cookies. We’ll mask your IP address\* though!” (\* but only when incognito while authenticated) (\*\* in the end they didn’t even deliver on that)

**ACT III. ACCELERATION** - [YOU CAN NOW USE DEVICE FINGERPRINTING IN OUR AD PRODUCTS](https://privacymatters.dlapiper.com/2025/01/googles-u-turn-on-device-fingerprinting-icos-response-and-subsequent-guidance/)

Now there’s a better way to know who’s who!

## How does fingerprinting work?

Nice, let’s nerd out properly.

You visit a website. Your browser sends a request to the server and the server returns code for the browser to run.

The request already reveals information about you; what browser you use, the language it’s in, the OS that runs it and your IP address, which the server needs to send you the response. That often reveals your location.

Then the code runs.

See, we’ve made a lot of civilizational progress in making browsing safe. [Long gone are the days when getting a virus just by visiting a website was a daily concern](https://www.darkreading.com/vulnerabilities-threats/browser-exploits-wane-users-become-attack-surface). The thing is, the definition of safety varies depending on who you ask. The broad consensus ends at isolation: a website’s code can’t see the data stored by other websites, and it can’t see your files or other applications without permission. **Chrome** and **Edge** stop here. Then you have **Safari** (especially on iOS/macOS 26+) and **Firefox** extending that definition to privacy, but they still don’t fully protect you from fingerprinting. **Brave** and **Tor** are the leading privacy-first browsers, with up and coming **Mullvad Browser** (Mullvad being, in my opinion, one of the best VPN providers out there).

The code runs on your hardware, which can be measured in ways that produce consistent and unique readings. There’s not much that can be done about this really, so there’s only two established solutions:

1. feed a little bit of noise here and there. It’s called **farbling** and that’s what Brave does to make your fingerprint inconsistent.
2. make everything look the same, always, so there’s no telling browser users apart. Tor and Mullvad adopt this approach.

## Ok, let’s create a fingerprint

So there are two sources for data that can make up your fingerprint; the network request and the code environment.

**When you look at network, the data is generally either fragile or generic:**

- the IP address; in a ‘happy path’ where one person always uses the same IP address, it might be the only data you need. But commonly people do a lot of browsing on mobile networks, they share the WiFi with others, they use VPNs or their home network’s IP address is rotated by the ISP. It’s fragile as such.
- headers like `User-Agent` or `Accept-Language`; reveal your browser, OS, versions and the language set.

**When you can execute code on the device, you have ways to measure its hardware qualities (without any permission prompts!) and these are surprisingly stable:**

- GPU; behaviour, vendor, driver
- CPU; behaviour, vendor, number of threads
- memory size
- display data; resolution, pixel ratio, refresh rate
- audio processing
- device model

**And on the software level; installed fonts, extensions, system info…**

Millions of devices use the latest Chrome, there are millions of Macs with the M4 chip and there are millions of browsers in French.

But when you look at devices that run **the latest Chrome in French on an M4**, that number now drops significantly. Add to it [hardware measurements](https://arxiv.org/abs/2201.09956) and it gets so low it becomes practically unique.

The fingerprint doesn’t need to be truly, globally unique - it just needs to be unique enough to tell people apart in your sample size.

A **ZIP code** (inferred from IP), a **birth date** (you’ve given away on some platform) and **sex** (you fill it in everywhere) together are enough to uniquely identify over 60% of Americans.

You can always add another variable to decrease the potential pool of candidates.

## Information Entropy

Have you ever played [Akinator](https://akinator.com/)? You think of a character, a genie asks you some twenty questions, and somehow it knows. That’s fingerprinting in a nutshell, except your browser answers the questions for you. Once you understand information entropy, Akinator’s magic turns into a simple trick.

Let’s simplify with that game where you have to guess the character on a board through questions like “Is my character a man?”. If I say “yes”, and half of the characters on your board are men, you’re now considering only half of the total candidates. Now we can say that my answer gave you `log₂(100 / 50) = log₂(2) = 1` **bit of information**, because it cut the pool of candidates in half.

The bits you get from one answer are called its surprisal: the rarer the answer, the more it tells you.

- a trait shared by 1 in 2 people is worth 1 bit (is a man),
- 1 in 8 is worth 3 bits (has a mustache),
- and 1 in 128 is worth 7 bits (has one eye).

Surprisal quantifies the value of the answer, and entropy quantifies the value of the question. If 1 person in 128 is one-eyed, then asking about it will yield ‘no’ 127 times in 128.

| Answer | Probability | Surprisal                        | Entropy contribution        |
| ------ | ----------- | -------------------------------- | --------------------------- |
| Yes    | 1/128       | `log₂(128 / 1) = 7 bits`         | `1/128 x 7 = ~0.055`        |
| No     | 127/128     | `log₂(128 / 127) = ~0.0113 bits` | `127/128 x 0.0113 = ~0.011` |

Summing up the contribution of each possible answer, the question itself is worth ~0.066 bits of entropy - that’s the average amount of bits you gain from an answer.

When we combine traits that are **independent** we simply add up their bits or multiply the ratios, so a man with a mustache and one eye would mean 11 bits, or 1 in 2048. Alas, as [Thomas Bayes](https://en.wikipedia.org/wiki/Thomas_Bayes) would have you know, nothing is ever so simple in practice. A mustache is already a strong indicator of a man, and a man is probably more likely to have one eye. The traits are not independent so the bits don’t really add up.

Some interesting applications of this type of reasoning:

- In **medical diagnostics**: A positive result from a 99% accurate test for a disease known to affect 0.01% of the population is about 100 times more likely to be wrong than you are to have the disease. You need more evidence!
- In **cryptography**: A password made out of four random words is as strong as eight random keyboard characters (but much easier to remember!)\
   ![xkcd 936](https://imgs.xkcd.com/comics/password_strength.png)
- In **forensic science**: singling out one person in a city of a million takes about 20 bits. A witness description that matches 1 person in 1,000 provides only 10 bits, which still leaves a thousand possible suspects.

## So what can I do about it?

Test your browser:

- [coveryourtracks.eff.org](https://coveryourtracks.eff.org/) shows you how unique your fingerprint is, with entropy for each measured data point.

If you’re using Chrome you’re basically walking around with an ID on your forehead.

Safari and Firefox offer some level of protection, but you’re still identifiable by advanced trackers.

**Brave**, **Tor** and **Mullvad Browser** are the only major browsers at the moment that implement advanced protections against fingerprinting.

If you don’t feel like switching browsers, I get it. I am a big [Arc](https://arc.net/) fan myself and unfortunately it’s discontinued and will never have any privacy features. There are extensions like [JShelter](https://jshelter.org/) - they disrupt fingerprinting scripts by intercepting browser API calls. But they also break some websites. Same problem applies to Firefox’s Resist Fingerprinting feature.

The ironic truth is, privacy extensions make you more unique, since fewer people have them. So a smart tracker could detect that, ignore the signals disrupted by the extension and focus on the ones that are not.

**No extension can be as effective as engine-level protection** built into the browser. So if you don’t want to be tracked, you gotta commit.

## methods used on this page

I’m simply creating a hash out of a few measurements of your device. Any of these change and the hash is now different. Professional applications are much more complex than this and are able to maintain identification line when single variables change.

1. `OfflineAudioContext`

   Renders a short tone through a compressor without playing it; tiny float differences in the result depend on your browser’s audio engine and your CPU.

   ```
   const off = new OfflineAudioContext(1, 5000, 44100);
   // … a 1 kHz triangle wave into a dynamics compressor …
   const data = (await Promise.race([off.startRendering(), timeout])).getChannelData(0);
   let sum = 0;
   for (let i = 4500; i < 5000; i++) sum += Math.abs(data[i]);
   return sum.toFixed(3);
   ```

2. `WebGL`

   Reads your GPU’s vendor and model (without version numbers), what the GPU and its driver support, and the pixels of a small rendered triangle.

   ```
   const gl = document.createElement('canvas').getContext('webgl');
   const dbg = gl.getExtension('WEBGL_debug_renderer_info');
   return gpuName(gl.getParameter(dbg ? dbg.UNMASKED_RENDERER_WEBGL : gl.RENDERER) || '');
   // "ANGLE (Apple, ANGLE Metal Renderer: Apple M4, Unspecified Version)" -> "apple m4"
   // …
   return fnv1a(new TextEncoder().encode([...(gl.getSupportedExtensions() || []).sort(), ...limits, ...precision].join('|')));
   // …
   gl.drawArrays(gl.TRIANGLES, 0, 3);
   gl.readPixels(0, 0, canvas.width, canvas.height, gl.RGBA, gl.UNSIGNED_BYTE, px);
   ```

3. `canvas`

   Draws text in a few fonts, emoji and a gradient, then reads the pixels back; anti-aliasing and font rendering depend on your OS, GPU and fonts.

   ```
   ctx.font = '15px "Times New Roman", serif';
   ctx.fillText('Sphinx of black quartz 🦊', 4, 22);
   // …
   const data = ctx.getImageData(0, 0, 240, 60).data;
   ```

4. `Intl`

   Your time zone and your date and number settings; a VPN changes your IP location, but not your time zone.

   ```
   const date = Intl.DateTimeFormat().resolvedOptions();
   // …
   return [date.timeZone, date.locale, date.calendar, date.numberingSystem, hourCycle, (1234567.891).toLocaleString()].join('/');
   ```

5. `navigator`

   Your number of CPU cores, your memory size (Chrome only) and your browser’s language.

   ```
   return [navigator.hardwareConcurrency, navigator.deviceMemory, navigator.language].join('/');
   ```

6. `crypto.subtle`

   Hashes all the values with SHA-256, and the hash picks a trait and an animal for your nickname.

   ```
   const digest = await crypto.subtle.digest('SHA-256', bytes);
   // …
   nickname: stranger || `${pick(TRAITS, hex, 0)} ${pick(ANIMALS, hex, 8)}`,
   ```
