---
title: I expected better from Google
link: https://www.minitap.ai/blog/i-expected-better-from-google
source: lobste-rs-top-1w
published: 2026-09-17T10:01:13Z
updated: 2026-09-17T10:01:13Z
first_seen: 2026-09-19T21:30:23.395188495Z
authors:
- minitap.ai via FedericoSchonborn
labels:
- law
summary: Comments
content: extracted
html: 2026-09-17-i-expected-better-from-google.html
preview:
  file: 2026-09-17-i-expected-better-from-google.preview-c4c894941ed6.webp
  width: 256
  height: 184
  alt: An earlier Artemis package file lists Pierre-Louis Favreau, Jean-Pierre Lo, and Nicolas Dehandschoewercker as authors.
  color: '#f6f6f2'
images:
- source: https://cdn.sanity.io/images/5b5hto6c/production/8a4a9cd2ceb3a1679ca5033e6209fc7d558e81a0-1360x980.png
  original:
    file: 2026-09-17-i-expected-better-from-google.image-ed777dab081f.png
    width: 1360
    height: 980
  color: '#fbfcfc'
- source: https://cdn.sanity.io/images/5b5hto6c/production/4fa1126fcee7ceacdca0fa09a4f800518b9d5409-1360x980.png
  original:
    file: 2026-09-17-i-expected-better-from-google.image-39510c998728.png
    width: 1360
    height: 980
  color: '#fbfcfc'
- source: https://cdn.sanity.io/images/5b5hto6c/production/a6cbba90bfc6899f37b1bde27cb525fc92a322c4-2640x1380.png
  original:
    file: 2026-09-17-i-expected-better-from-google.image-51ee4e205a2e.png
    width: 2640
    height: 1380
  color: '#fafbfc'
---

I was in the office with the team when we heard that Google had released Artemis, a project for automating mobile devices. We opened the repository and started looking through it. My reaction was pretty immediate:

“What the fuck? We wrote this.”

We built [mobile-use](https://github.com/minitap-ai/mobile-use) as an open-source research experiment to see whether we could get AI agents to reliably interact with phones. We worked on that through February, then focused on a newer, more powerful, closed-source version that now powers QA across web and mobile at Minitap.

Finding familiar code in another project is something you expect when you publish your work for people to use. Finding it under Google’s name, without an acknowledgment of where it came from, is harder to take.

Then we looked at the history and found our names in an older version. They had been removed.

I’m disappointed in Google. And I think this matters beyond our own repository, because open source depends on people being willing to keep sharing their work. The way that work is treated matters.

## What we recognised

The examples were specific. Parts of the code for connecting to Android devices matched our implementation exactly. The Hopper agent’s instructions were identical, word for word. Those matches were still present in the Artemis version we checked on September 11: here is the [Artemis device code](https://github.com/google/artemis/blob/086078819209c7139d6f833cfdc6d5cc80d9f19a/artemis/clients/adb_tunnel.py#L91-L306) alongside [ours](https://github.com/minitap-ai/mobile-use/blob/cc20e3d52bf6dac1dff07f7b894520b8aeb051b7/minitap/mobile_use/clients/adb_tunnel.py#L82-L297), and the [Artemis prompt](https://github.com/google/artemis/blob/086078819209c7139d6f833cfdc6d5cc80d9f19a/artemis/agents/hopper/hopper.md) alongside [ours](https://github.com/minitap-ai/mobile-use/blob/cc20e3d52bf6dac1dff07f7b894520b8aeb051b7/minitap/mobile_use/agents/hopper/hopper.md).

Fun fact about Hopper: we didn’t know what to call that agent. Jean-Pierre, one of our engineers, likes Minecraft and thought the name was cool, so we went with it. That was the reasoning. Seeing the same name attached to the same instructions in Google’s repo felt very familiar.

An example for keeping an agent inside WhatsApp had the same task of sending Happy New Year messages to Alice, Bob, and Charlie, with the same comments and cleanup steps. That was still there too. Compare the [Artemis messaging example](https://github.com/google/artemis/blob/086078819209c7139d6f833cfdc6d5cc80d9f19a/artemis/sdk/examples/app_lock_messaging.py#L43-L87) with [the mobile-use example](https://github.com/minitap-ai/mobile-use/blob/cc20e3d52bf6dac1dff07f7b894520b8aeb051b7/minitap/mobile_use/sdk/examples/app_lock_messaging.py#L10-L54).

Older versions even shared a bug: a helper would write a results file, then fail when it tried to read its own output on the next run. We reproduced the same failure in [both](https://github.com/google/artemis/blob/206a9ce921367ca10091c79ad00e116f195850ff/artemis/utils/media.py#L47-L207) [implementations](https://github.com/minitap-ai/mobile-use/blob/cc20e3d52bf6dac1dff07f7b894520b8aeb051b7/minitap/mobile_use/utils/media.py#L30-L190). Artemis has since fixed it.

The author history was harder to understand. An [earlier package file](https://github.com/google/artemis/blob/14e02c4c27bc5b3c0d07e9a7b3b20141d6af9348/pyproject.toml#L20-L24) listed Pierre-Louis Favreau, Jean-Pierre Lo, and Nicolas Dehandschoewercker. The [version that replaced it](https://github.com/google/artemis/blob/206a9ce921367ca10091c79ad00e116f195850ff/pyproject.toml#L20-L22) removed all three names and substituted another author. The only change to the files was the author list. [GitHub’s activity record](https://github.com/google/artemis/activity?activity_type=force_push) shows that the replacement happened through a force push in August, before our investigation this September.

[![An earlier Artemis package file lists Pierre-Louis Favreau, Jean-Pierre Lo, and Nicolas Dehandschoewercker as authors.](https://cdn.sanity.io/images/5b5hto6c/production/8a4a9cd2ceb3a1679ca5033e6209fc7d558e81a0-1360x980.png)](https://cdn.sanity.io/images/5b5hto6c/production/8a4a9cd2ceb3a1679ca5033e6209fc7d558e81a0-1360x980.png)

*Our names in the earlier Artemis package file. GitHub now marks this version as detached; its [activity record](https://github.com/google/artemis/activity?activity_type=force_push) shows it was previously on the main branch. [File](https://github.com/google/artemis/blob/14e02c4c27bc5b3c0d07e9a7b3b20141d6af9348/pyproject.toml#L20-L24).*

[![The replacement Artemis package file lists a different author in place of the three Minitap names.](https://cdn.sanity.io/images/5b5hto6c/production/4fa1126fcee7ceacdca0fa09a4f800518b9d5409-1360x980.png)](https://cdn.sanity.io/images/5b5hto6c/production/4fa1126fcee7ceacdca0fa09a4f800518b9d5409-1360x980.png)

*The author list after the replacement. [Source](https://github.com/google/artemis/blob/206a9ce921367ca10091c79ad00e116f195850ff/pyproject.toml#L20-L22).*

Artemis contains its own engineering work too. It can acknowledge that work and the mobile-use code it incorporates in the same place. Yet the [README we checked](https://github.com/google/artemis/blob/086078819209c7139d6f833cfdc6d5cc80d9f19a/README.md) did not credit mobile-use. We have put the detailed comparisons, the archived files, and the timeline in a [public factual record](https://marvelous-doll-1e9.notion.site/Factual-record-of-events-Artemis-mobile-use-3d8cf438d3ea8060b4eecc83fb3854b9).

## Sharing code should make collaboration easier

We chose open source because we want other people to build on mobile-use. Someone should be able to take it in a direction we would never have considered. They should be able to improve it, turn it into a product, or build a competing project, within the license’s terms.

There are ordinary ways to do that while keeping its origins clear. A [fork](https://docs.github.com/en/pull-requests/reference/forks) links back to the original project. An imported copy can document its source. Copyright and attribution notices can stay with the code. A README can explain which parts came from elsewhere and what the new team has added.

Some of this is established engineering practice. Some is required by the license. The mobile-use source used for the current comparisons carries Apache 2.0, whose redistribution conditions include preserving applicable copyright and attribution notices and identifying changes. It also provides for retaining relevant attribution from an upstream `NOTICE` file when that file is included in the distribution. [Apache 2.0, section 4](https://www.apache.org/licenses/LICENSE-2.0)

That information is useful to everyone who comes next. It helps people find the original maintainers, understand a design decision, report a bug, or share a fix. It gives contributors a record of what they built. It lets a new project explain its own contribution honestly.

I should not need to recover an old version of a repository to discover that relationship. Maintainers already spend time answering questions, reviewing contributions, and keeping their projects working. Chasing missing attribution after a larger company republishes their work is another cost of sharing. An ecosystem that makes this routine gives people another reason to stop doing it.

I want more people publishing useful software, with more confidence that their work will still be recognised when it travels.

## Our benchmark results have also been hard to get heard

We had already spent months asking for our newer results to be reflected on [AndroidWorld’s leaderboard](https://docs.google.com/spreadsheets/d/1cchzP9dlTZ3WXQTfYNhh3avxoLipqHN75v1Tb86uhHo/edit?gid=0).

The maintainer applied our earlier submissions, up to 91.4%, with the last confirmation in December 2025. We then submitted 94.8%, followed by 100% in our evaluation in January. We sent two follow-ups. Those four emails received no reply. Our [results and task traces](https://www.minitap.ai/benchmark) are available for inspection.

As of September 11, the sheet still showed mobile-use at 91.4%, while Artemis appeared at 99.1%. These are self-reported results; the leaderboard explicitly says it does not independently verify them. That qualification applies to our reported 100% too.

Artemis’s own [comparison chart](https://github.com/google/artemis/blob/086078819209c7139d6f833cfdc6d5cc80d9f19a/docs/assets/androidworld_leaderboard.png) omitted us. It included DroidRun at the same listed score as mobile-use, and an unrelated [MadeAgents project called MobileUse](https://github.com/MadeAgents/mobile-use) at a lower score.

[![Artemis’s published AndroidWorld comparison chart includes DroidRun and MadeAgents’ MobileUse but omits Minitap’s mobile-use.](https://cdn.sanity.io/images/5b5hto6c/production/a6cbba90bfc6899f37b1bde27cb525fc92a322c4-2640x1380.png)](https://cdn.sanity.io/images/5b5hto6c/production/a6cbba90bfc6899f37b1bde27cb525fc92a322c4-2640x1380.png)

*This is Artemis’s own chart. The “MobileUse” near the bottom is a different project. [Source](https://github.com/google/artemis/blob/086078819209c7139d6f833cfdc6d5cc80d9f19a/docs/assets/androidworld_leaderboard.png).*

We have no evidence connecting the unanswered emails or the chart omission to the removal of our names. But this is another part of our experience of trying to get the public record to reflect our work. We would like a response to those submissions and an explanation of the comparison.

## We’ve contacted Google

We’ve already contacted the Google team. There is also a [public issue on the Artemis repository](https://github.com/google/artemis/issues/40). We’ve asked them to acknowledge that Artemis is derived in part from mobile-use, credit the people behind it, and put the attribution right. I’ll update this post when we have something substantive to share.

## What disappoints me

**I’m disappointed in Google.** This is the company that gave us [Kubernetes](https://opensource.google/projects/kubernetes) and [TensorFlow](https://blog.google/innovation-and-ai/products/tensorflow-smarter-machine-learning-for/). When it launched Chrome, it explicitly credited WebKit and Firefox, writing: “We owe a great debt to many open source projects.” Google has a history of doing this well, and [published guidance](https://opensource.google/documentation/reference/releasing/preparing#third-party-components) for handling other people’s code. That is exactly why I expected better. [The original Chrome announcement](https://googleblog.blogspot.com/2008/09/fresh-take-on-browser.html).

**I’m disappointed for my team.** They did an insane job on mobile-use. Through December, January, and February, they were focused on making this work. I saw the effort that went into it. These are their implementations, their examples, their weird Minecraft-inspired agent names. I want them to see that work used and feel proud of it. Having to dig through somebody else’s repository to establish that they wrote it is a pretty shitty outcome.

**I’m disappointed for the open-source community.** It deserves better treatment than this. People put their work out there so others can learn from it and build on it. The people and projects behind that work deserve to remain visible. If even a release from Google means maintainers have to chase their own attribution, we are making it harder for people to feel good about sharing. That is not the kind of open source I want to be part of.

On the bright side, the version powering Minitap today is closed source. And it’s a completely different beast by now.
