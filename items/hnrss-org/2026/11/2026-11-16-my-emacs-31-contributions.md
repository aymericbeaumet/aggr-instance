---
title: My Emacs 31 contributions
link: https://xenodium.com/my-emacs-31-contributions
source: hnrss-org
published: 2026-11-16T14:26:10Z
updated: 2026-11-16T14:26:10Z
first_seen: 2026-11-18T17:57:41.978411823Z
authors:
- xenodium
content: extracted
html: 2026-11-16-my-emacs-31-contributions.html
remote_preview:
  url: https://xenodium.com/share.png
---

For me, landing changes in Emacs upstream always felt somewhat out of reach, so I kinda postponed it for a long while. Perhaps the biggest hurdle was getting over the process to [assign copyright to the Free Software Foundation](https://www.fsf.org/licensing/assigning.html), which does feel a bit archaic (download, sign, and return a PDF) and slow (mostly waiting for an email response), though hey, I have nothing but sympathy. Open source is unsustainably tricky. As consumers, we often like free as in freedom and free as in beer, but neither pay the bills. We hope some company steps in to fund these projects, but that rarely happens. As a result, most open source projects are starved (even popular ones), run by volunteers working for free.

All that is to say, if the upstream Emacs contribution/onboarding process seems less than ideal, cut it some slack. The project as a whole primarily runs on the blood, sweat, and tears of unpaid contributors. If you can, fund the maintainers you rely on (or better yet, get your employer to do so).

If you've been thinking of contributing to Emacs upstream, don't procrastinate like me and just give it a try. Compared to the typical GitHub pull-request process, your first Emacs contribution may feel a little daunting and dated, but there's a lovely fuzzy light at the end of the tunnel. You see, Emacs has been [around for 50 years](https://www.theregister.com/applications/2026/11/01/mother-of-all-editors-emacs-is-50/5306727). Very few software projects get to live this long. Landing contributions on a project with such a rich history is a real treat!

But I digress… Let's get moving. Once I got over procrastinating, I finally got a handful of small contributions landed on the master branch. Thank you, Eli Zaretskii, Stefan Kangas, and Gerd Möllmann, for your help. From then on, I eagerly waited for them to formally make it to a stable release.

Emacs 31 shipped the lot.

## Hide Dired's absolute location

I do enjoy a [visual (and functional) tweak here and there](https://xenodium.com/my-emacs-eye-candy), and a Dired tweak felt small enough to be the perfect candidate for my very first Emacs contribution.

With that in mind, I tweaked `dired-hide-details-mode` by adding the `dired-hide-details-hide-absolute-location` user option, which hides the directory's absolute path, typically displayed at the top of Dired buffers ([bug#72272](https://debbugs.gnu.org/cgi/bugreport.cgi?bug=72272), [commit](https://cgit.git.savannah.gnu.org/cgit/emacs.git/commit/?id=7cbca90569472af5643905fca5b7ab2dea67f876)).

More on this in my [Hide another detail](https://xenodium.com/hide-another-detail) post.

![](https://xenodium.github.io/images/hide-another-detail/after.png)

## macOS speech recognition permission

At some point, I ran into a snag trying to run [macosrec](https://github.com/xenodium/macosrec) from my beloved `eshell`. The process just crashed with a cryptic `abort trap: 6`, so the issue wasn't super obvious. It turned out the utility required granting an OS permission that can only be requested by the host app (Emacs) if it includes the relevant description in its `Info.plist` file.

As of version 31, Emacs includes `NSSpeechRecognitionUsageDescription` in its `Info.plist` ([bug#72827](https://debbugs.gnu.org/cgi/bugreport.cgi?bug=72827), [commit](https://cgit.git.savannah.gnu.org/cgit/emacs.git/commit/?id=c36ff7d8e46c75cfadea6f4c1f4eadb3aef37f2d)).

## Send to… (aka macOS sharing)

`context-menu-mode` now includes a "Send to…" item, enabling sending the current file(s) or region text to external apps or services. You can also invoke `M-x send-to` directly. On macOS, it's powered by the native sharing picker (I use it frequently to [AirDrop](https://support.apple.com/en-us/119857) things to my iPhone). Elsewhere, it falls back to `shell-command-do-open` ([bug#76120](https://debbugs.gnu.org/cgi/bugreport.cgi?bug=76120), [commit](https://cgit.git.savannah.gnu.org/cgit/emacs.git/commit/?id=813013691a9997cbdd483e1e5cef39453ac07d6d), [follow-up fix](https://cgit.git.savannah.gnu.org/cgit/emacs.git/commit/?id=bd5b1c6b085b17870415009cbf63171e3d2fc91f)). These two implementations plug into new generic plumbing (`send-to-handlers`), so you could, for instance, add a handler sharing via Android's native share sheet (`android-browse-url` can do the heavy lifting) or KDE Connect on Linux (`kdeconnect-cli --share`).

More in my [Emacs send-to (aka macOS sharing) merged upstream](https://xenodium.com/emacs-send-to-aka-macos-sharing-merged-upstream) post.

![](https://xenodium.github.io/images/emacs-send-to-aka-macos-sharing-merged-upstream/context-menu.png)

![](https://xenodium.github.io/images/emacs-send-to-aka-macos-sharing-merged-upstream/send-to.png)

## macOS dictation is back

Emacs 30 lost macOS dictation when migrating to `NSTextInputClient`. Implementing `selectedRange` in `nsterm.m` brought it back ([bug#79070](https://debbugs.gnu.org/cgi/bugreport.cgi?bug=79070), [commit](https://cgit.git.savannah.gnu.org/cgit/emacs.git/commit/?id=6e64e0bd26b6c0f1c4e90c9bc0df37a2a9ac72da)). With LLMs/agents being a thing these days, dictating into your Emacs buffer may be just the thing to complement your AI workflow.

I covered this in my [macOS dictation returns to Emacs (fix merged)](https://xenodium.com/macos-dictation-returns-to-emacs-fix-merged) post.

![](https://xenodium.github.io/images/macos-dictation-returns-to-emacs-fix-merged/dictation.webp)

## Make it all sustainable

Are you getting value from any of my upstreamed changes or my Emacs packages? Please consider [sponsoring](https://github.com/sponsors/xenodium) to keep the work going. While my patches may seem relatively straightforward, some took [quite a bit of effort to land upstream](https://debbugs.gnu.org/cgi/bugreport.cgi?bug=76120).

My Emacs work not your cup of tea? No worries. As mentioned earlier, sponsor the maintainers you rely on instead.

powered by [LMNO.lol](https://LMNO.lol)

[privacy policy](https://lmno.lol/blog/privacy-policy) · [terms of service](https://lmno.lol/blog/terms-of-service)
