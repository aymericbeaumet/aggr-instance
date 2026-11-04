---
title: Death comes for XSLT
link: https://dbushell.com/2026/11/03/xslt-rip/
source: lobste-rs
published: 2026-11-03T15:19:50Z
updated: 2026-11-03T15:19:50Z
first_seen: 2026-11-04T00:52:13.198927099Z
authors:
- dbushell.com via nytpu
labels:
- browsers
- web
summary: Comments
content: extracted
html: 2026-11-03-death-comes-for-xslt.html
remote_preview:
  url: https://dbushell.com/images/articles/2026-11-03-xslt-rip.png
---

Last halloween, [Google decided to kill XSLT](https://groups.google.com/a/chromium.org/g/blink-dev/c/CxL4gYZeSJA/m/yNs4EsD5AQAJ). That was not a [unilateral decision](https://meyerweb.com/eric/thoughts/2025/08/22/no-google-did-not-unilaterally-decide-to-kill-xslt/) mind you, Google said “jump” and other browser vendors said “how high?”

In response, I published my [Intent to Deprecate and Remove Google Chrome](https://dbushell.com/2025/11/01/intent-to-deprecate-and-remove-google-chrome/). A performative declaration of course, I don’t install spyware.

Fearing my parody lacked seriousness, I created [XSLT.RIP](https://xslt.rip/) to ensure the developer world was informed of Google’s [death note](https://en.wikipedia.org/wiki/Death_Note). This did the trick, although a [few commenters](https://news.ycombinator.com/item?id=45873434) believed it was satire. There is subtlety to [XSLT.RIP](https://xslt.rip/). The website itself is built with XSLT and when Google kills XSLT they’ll kill my website.

The web standards process and its stewards (*Big Browser*) used to adhere to a social contract. Backwards compatibility was a major design principle. Under no circumstances do you ever purposefully break the web.

- [“Don’t break the web”](https://www.chromium.org/developers/release-process/#dont-break-the-web)
- [“Don’t break the web”](https://developer.mozilla.org/en-US/docs/Learn_web_development/Getting_started/Web_standards/The_web_standards_model#dont_break_the_web)
- [“Don’t break the web”](https://github.com/tc39/how-we-work/blob/main/terminology.md#web-compatibilitydont-break-the-web)

Google decided to renege and break the web.

[XSLT.RIP](https://xslt.rip/) will be broken soon and I’ll sit back and watch Google break it. [17 November 2026](https://doctor-seo.net/en/blog/news/chrome-edge-removing-xslt/) appears to be the date of execution. Some Chromium-based browsers have been running a [man-in-the-middle attack](https://en.wikipedia.org/wiki/Man-in-the-middle_attack) by injecting propaganda into XSLT websites.

> This site uses XSLT; that functionality is being removed from this browser very soon. When that happens, this page will likely no longer display correctly. You might be able to install a browser extension that allows you to continue viewing it. Otherwise, you should contact the maintainer of the site for further information.
>
> Source: [This site uses XSLT: what Chrome’s banner means](https://xsltplayground.com/blog/chrome-xslt/this-site-uses-xslt-warning/) - XSLT Playground

This appears to be an `<xslt-warning-banner>` custom element that fails to render when it violates [content security policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CSP).

![Two examples of the XSLT warning banner quoted above. The first is white text on a red background, the second is broken default HTML styles.](https://dbushell.com/images/blog/2026/xslt-banner.avif)

Two examples of the XSLT warning banner quoted above. The first is white text on a red background, the second is broken default HTML styles.

I’m testing on [ungoogled-chromium](https://github.com/ungoogled-software/ungoogled-chromium) so who knows how it’s intended to look.

[The line of death](https://textslashplain.com/2017/01/14/the-line-of-death/) is another “social contract” that Google has forgotten. They’re blaming website owners and acting like questionable purveyors of free public Wi-Fi. This is arrogant and intrusive.

“contact the maintainer of the site” — you broke the web Google, own it at least.
