---
title: Bitwarden Dual License Model
link: https://community.bitwarden.com/t/published-version-update-in-app-stores/102750
source: hnrss-org
published: 2026-10-10T14:32:50Z
updated: 2026-10-10T14:32:50Z
first_seen: 2026-10-10T20:00:28.353232832Z
authors:
- Cider9986
content: extracted
html: 2026-10-10-bitwarden-dual-license-model.html
extra:
  comments_url: https://news.ycombinator.com/item?id=50033407
  num_comments: 207
  points: 275
---

October 9, 2026, 3:04pm 1

Hello everyone!

Starting in the next release, the Bitwarden apps published to the various stores will be the commercially licensed builds. No action is needed, and the apps will work exactly as they do today.

- Bitwarden remains committed to open source security and transparency
- The GPLv3 OSS licensed version continues to be updated and published on GitHub
- All current features are available in both versions
- License details are on [GitHub](https://github.com/bitwarden/clients/blob/main/LICENSE.txt)
- Bitwarden remains committed to a robust, free forever plan for everyone

If you have any questions, please ask them in this thread. Thanks all!

**EDIT:**\
 The App Store clients are built directly from 100% publicly auditable code on GitHub. Nothing has been made closed source. You can still read, audit, and contribute to the codebase on GitHub, and that isn’t changing.

- **Bitwarden is still open source**
- **Bitwarden code can still be forked, even the commercial license code**
- **No change to self-hosting, the licensing change affects those who are reselling Bitwarden**
- **The free plan is here to stay permanently**

**Additional edit:**

Adding in some information that was surfaced in comments but not in the main post:

**What is behind this?**\
 Bitwarden has had a commercial license for some time. This has been focused on enterprise features, of which Bitwarden is developing more for those customers and for additional markets. See the [FedRAMP announcement](https://www.businesswire.com/news/home/20260915063633/en/Bitwarden-Announces-Commitment-to-Achieve-FedRAMP-Class-D-High-Certification) and [Privileged Controls](https://www.businesswire.com/news/home/20260929695265/en/Bitwarden-Introduces-Privileged-Controls-to-Address-PAM-Deployment-Friction-as-AI-Enhanced-Attacks-Raise-Credential-Risk) announcements, as examples. The Bitwarden License (commercial license) provides additional legal control over what people can do with the code.

**What’s going on with the app stores?**\
 At build time for a new release, Bitwarden creates two versions and publishes them on [GitHub](https://github.com/bitwarden/clients), the commercial version and the GPLv3 version. Until now Bitwarden has been pushing the GPLv3 version to app stores. The change is because anyone with a plan that would need commercial features would have had to go to GitHub to get the commercial version, something nobody can expect enterprises or government workers to do. Putting out the commercial licensed build on app stores means everyone can get the features they pay for (or don’t pay for!) from the same place.

**What features use the commercial license?**\
 New enterprise features, new products, and other items that provide Bitwarden a strategic competitive edge in new markets will be behind the commercial license. This in most cases will apply to business plans only. Some features for premium plans may also fall under the commercial license, but that will be evaluated on a case-by-case basis.

**Does this impact me?**\
 The vast majority of users will not see any impact. If you download Bitwarden from any of the main app stores you will have the same function and same code that is published online. Users that wish to only ever use the GPLv3 license will need to download and side-load the Bitwarden application onto their devices, however those users will not have access to any features that they pay for that is commercially licensed.

Happy to continue answering questions!

3 Likes

 [grb](https://community.bitwarden.com/u/grb) October 9, 2026, 3:16pm 2

Will the [Portable Desktop App link](https://bitwarden.com/download/?app=desktop&platform=windows&variant=portable) on Bitwarden’s [Download Page](https://bitwarden.com/download/) download a build that is commercially licensed or OSS licensed?

What differences will exist between the two parallel versions?

 [RyanL](https://community.bitwarden.com/u/RyanL) October 9, 2026, 3:20pm 4

Some future components will be published under the commercial license and will exist only in that build. Newly developed features will be evaluated on a case-by-case basis for which license applies to them.

1 Like

 [RyanL](https://community.bitwarden.com/u/RyanL) October 9, 2026, 3:28pm 5

That’s a great question. I’ll work to get that information.

Edit: The download page will also direct link to the commercial license.

 [grb](https://community.bitwarden.com/u/grb) October 9, 2026, 3:33pm 6

Ahhh, so the word “current” is doing some *heavy lifting* in the following statement:

But will the new features (those excluded from the GPLv3 OSS licensed builds) still be *open source*?

1 Like

 [grb](https://community.bitwarden.com/u/grb) October 9, 2026, 3:40pm 7

[@RyanL](https://community.bitwarden.com/u/ryanl) Given that there are already many features that are only unlocked for Premium subscriptions, will there now be up to **four** levels of functionality (as shown in the table below), and if so, will there be up to four different pricing tiers (including at least one “Free” option)?

| *Relative Cost* | GPLv3 License | Commercial License |
| --------------- | ------------- | ------------------ |
| **Non-Premium** | Free          | ???                |
| **Premium**     |   $           | $$$                |

 [RyanL](https://community.bitwarden.com/u/RyanL) October 9, 2026, 3:41pm 8

Direct downloads from the [Download the Bitwarden Password Manager App for iPhone, Android, Chrome, Safari, and More | Bitwarden](http://Bitwarden.com/download) page will also be under the commercial license.

Yes, Bitwarden continues development of open source code under GPLv3. Code under the commercial license is just as viewable and transparent on GitHub. This licensing model provides additional legal protection for select features.

3 Likes

 [davidmayr](https://community.bitwarden.com/u/davidmayr) (David Mayr) October 9, 2026, 6:52pm 9

This concerns me quite a bit tbh. It feels like the standard: Open Source → Semi Opensource → Open Source discontinued Rugpull quite a few companies have been doing lately. I especially got hit by this with MinIO, so I’m quite careful what I use now because MinIO has been a huge pain and switching from Bitwarden would be too.

I both pay for Bitwarden (although I don’t use any premium features. I got it to support Bitwarden so it can continue being an open source project. With the price increase I was already considering cancelling as I don’t use the premium stuff and with this it gives me even more reason to do so) but I also have Vaultwarden instances self-hosted for various projects to work in teams. Both are essential to me and I want both to work equally well. I currently neither plan to switch my personal Bitwarden to self hosted nor would I ever consider moving my self hosted instances to your servers.

So my questions are:

- Are there any new features coming to the OSS version, or is it staying as is with everything new being in the commercial version?
- What would I lose out on by not using the commercial version now or in the future?
- How are you guaranteeing that self-hosting will remain effortless and will it remain possible for third-party teams like Vaultwarden to continue to use the protocol?
- What version does the Linux Flatpak store receive? Will it remain the open source version or the commercial one?
- Will e.g. a commercial browser addon remain compatible with a open source client? (E.g. With the biometric thingy?)
- How are you guaranteeing to us that such a Rugpull will not happen now or in the future?

In any case Bitwarden will now be put on the list of things I will need to watch its development more closely in case this goes south.

Edit: I just saw in the comments that it was noted that it will be decided on case by case basis on what gets developed. Thats good for now I guess, yet the other questions remain open. (Ig it implicitly answers some other questions e.g. the self-hosting one too, but think of them more in the long run)

9 Likes

 [Lexx](https://community.bitwarden.com/u/Lexx) October 9, 2026, 6:52pm 10

While this might seem benign, Bitwarden would be well advised to review in detail what happened with the recent Coinkite fiasco. The security playing field is basically identical and the extremely real (and dramatically realized) risk is basically identical.

In summary: CoinKite ColdCard was also published open software but without a FOSS license. This, in turn, eliminated any possibility and motivation for the community to fork, audit, and review their software. This, in turn, allowed a, frankly, idiotic bug to fester. This in turn led to users’/customers’ loss of a few million dollars of value to an almost trivial exploit that can’t even be classified as a proper hack. Besides insane value loss, Coinkite is effectively dead as an enterprise and its principles doxed and will remain shunned the rest of their lives (deservedly so; I accidentally know one of them as a previous work colleague — not a bad guy, but this is deserved).

This is exactly where Bitwarden is heading now.

6 Likes

 [zfJames](https://community.bitwarden.com/u/zfJames) October 9, 2026, 7:16pm 11

This seems like a great reason for me to discontinue my Bitwarden subscription. Bitwarden is easily the best platform out there for password management but this is the obvious result of venture capital influence and attorneys.

What would keep me subscribed as a user if I have the requirement that I must be able to self-host a platform in order to keep using it (even if I am not currently self-hosting, which I am not)?

8 Likes

 [dwbit](https://community.bitwarden.com/u/dwbit) October 9, 2026, 7:18pm 12

Hey [@zfJames](https://community.bitwarden.com/u/zfjames) there is no requirement to self-host and you can continue using the clients from the app storefronts as normal.

 [zfJames](https://community.bitwarden.com/u/zfJames) October 9, 2026, 7:21pm 13

Thank you for your reply! I think my question was worded confusingly.

My requirement for a password manager is: It must give me the option to self host if I choose to do so.

That is a safeguard against my passwords and data getting locked in a box that I cannot get out of. With this new license and policy, it is clear to me that Bitwarden now does not meet my requirements. An unnamed list of “features” will now be developed entirely apart from what I can host that may or may not end up being critical for security.

Can you confirm if this is correct?

1 Like

 [dwbit](https://community.bitwarden.com/u/dwbit) October 9, 2026, 7:22pm 14

That is incorrect, there is no change to the ability to self-host with a Bitwarden subscription.

 [Nail1684](https://community.bitwarden.com/u/Nail1684) October 9, 2026, 7:28pm 15

I see no sign yet that vault export is getting removed. Quite the contrary, the [Credential Exchange Protocol/Format (CXP/CXF)](https://bitwarden.com/blog/security-vendors-join-forces-to-make-passkeys-more-portable-for-everyone/) was added recently to the mobile apps.

1 Like

 [zfJames](https://community.bitwarden.com/u/zfJames) October 9, 2026, 7:29pm 16

Thank you for clarifying! That does not change my negative stance on this change but does help clarify operation.

 [dwbit](https://community.bitwarden.com/u/dwbit) October 9, 2026, 7:39pm 17

For your question on third-party community servers, there is no change, they can continue to choose which features to support and build out on their server.

1 Like

 [Wreckage](https://community.bitwarden.com/u/Wreckage) October 9, 2026, 8:37pm 18

Not a great look, this seems a lot like openwashing. How can we trust that this is not the first step in a rug pull out of FOSS?

Can you clarify if commercial license exclusive features will be available for self hosted users? Are self hosted users now second class citizens?

 [dwbit](https://community.bitwarden.com/u/dwbit) October 9, 2026, 8:59pm 19

[@Wreckage](https://community.bitwarden.com/u/wreckage) no change to self-hosting. This only affects those who are repackaging and reselling Bitwarden.

 [grb](https://community.bitwarden.com/u/grb) October 9, 2026, 9:34pm 20

[@dwbit](https://community.bitwarden.com/u/dwbit) Could you please look into my unanswered question from above and see if some clarity can be provided?

1 Like

 [Wreckage](https://community.bitwarden.com/u/Wreckage) October 9, 2026, 9:35pm 21

I understand, but I worry more about the future. Will all future features still be available for self hosters? Or should we expect that some future features will not be available for self hosters?

While I have been happy to be a paying subscriber, self hosting capabilities is very important to me as it is my exit strategy if something goes wrong.
