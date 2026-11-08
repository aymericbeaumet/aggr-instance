---
title: 'ISponsorBlockTV: SponsorBlock client for all YouTube TV clients'
link: https://github.com/dmunozv04/isponsorblocktv
source: hnrss-org
published: 2026-11-08T03:47:19Z
updated: 2026-11-08T03:47:19Z
first_seen: 2026-11-08T18:46:19.638047103Z
authors:
- xnx
content: extracted
html: 2026-11-08-isponsorblocktv-sponsorblock-client-for-all-youtube-tv.html
remote_preview:
  url: https://opengraph.githubassets.com/3300b929f43979f1d073f192ed304654cc0ea55845b952eec9d6cbfecddc8c0b/dmunozv04/iSponsorBlockTV
  alt: SponsorBlock client for all YouTube TV clients. Contribute to dmunozv04/iSponsorBlockTV development by creating an account on GitHub.
---

[![ghcr.io Pulls](https://camo.githubusercontent.com/fc2f1ee8201411f0ec2494126cf2da647166dfe60479c53cb5fefd18557361ed/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f64796e616d69632f6a736f6e3f75726c3d687474707325334125324625324669706974696f2e6769746875622e696f2532466261636b616765253246646d756e6f7a7630342532466953706f6e736f72426c6f636b54562532466973706f6e736f72626c6f636b74762e6a736f6e2671756572793d646f776e6c6f616473266c6f676f3d676974687562266c6162656c3d676863722e696f25323070756c6c73267374796c653d666c6174)](https://ghcr.io/dmunozv04/isponsorblocktv) [![Docker Pulls](https://camo.githubusercontent.com/fbddf5390b3ab814be172a489ae2fc51d164af40ca117715ad8b6b6929afe9c9/68747470733a2f2f696d672e736869656c64732e696f2f646f636b65722f70756c6c732f646d756e6f7a7630342f6973706f6e736f72626c6f636b74763f6c6f676f3d646f636b6572267374796c653d666c6174)](https://hub.docker.com/r/dmunozv04/isponsorblocktv/) [![GitHub Release](https://camo.githubusercontent.com/2aa5079bfd2bc5fbacd8c8a70cff7c438e9eae8a722ce451d8d9074092ada6f1/68747470733a2f2f696d672e736869656c64732e696f2f6769746875622f762f72656c656173652f646d756e6f7a7630342f6973706f6e736f72626c6f636b74763f6c6f676f3d476974487562267374796c653d666c6174)](https://github.com/dmunozv04/iSponsorBlockTV/releases/latest) [![GitHub Repo stars](https://camo.githubusercontent.com/efa21b6d6aa6a9a04090cbc6a409d0988eefe280b8c7ee78fd94f2d46e7d46f4/68747470733a2f2f696d672e736869656c64732e696f2f6769746875622f73746172732f646d756e6f7a7630342f6973706f6e736f72626c6f636b74763f7374796c653d666c6174)](https://github.com/dmunozv04/isponsorblocktv)

iSponsorBlockTV is a self-hosted application that connects to your YouTube for TV app (see compatibility below) and automatically skips segments (like Sponsors or intros) in YouTube videos using the [SponsorBlock](https://sponsor.ajay.app/) API. It can also auto mute and press the "Skip Ad" button the moment it becomes available on YouTube ads.

## Installation

Check the [wiki](https://github.com/dmunozv04/iSponsorBlockTV/wiki/Installation)

## Compatibility

Legend: ✅ = Working, ❌ = Not working, ❔ = Not tested

Open an issue/pull request if you have tested a device that isn't listed here.

| Device             | Status |
| ------------------ | ------ |
| Apple TV           | ✅\*    |
| Samsung TV (Tizen) | ✅      |
| LG TV (WebOS)      | ✅      |
| Android TV         | ✅      |
| Chromecast         | ✅      |
| Google TV          | ✅      |
| Roku               | ✅      |
| Fire TV            | ✅      |
| CCwGTV             | ✅      |
| Nintendo Switch    | ✅      |
| Xbox One/Series    | ✅      |
| Playstation 4/5    | ✅      |

\*Ad muting won't work when using AirPlay to send the audio to another speaker.

\*\* Shorts aren't fully supported due to limitations on YouTube's side. A single short can be seen by either selecting the "Disconnect" option in the warning shown or by long pressing the thumbnail to open the menu and clicking play from there

## Usage

Run iSponsorBlockTV on a computer that has network access. It doesn't need to be on the same network as the device, only access to youtube.com is required.

Auto discovery will require the computer to be on the same network as the device during setup. The device can also be manually added to iSponsorBlockTV with a YouTube TV code. This code can be found in the settings page of your YouTube TV application.

## Libraries used

- [pyytlounge](https://github.com/FabioGNR/pyytlounge) Used to interact with the device
- asyncio and [aiohttp](https://github.com/aio-libs/aiohttp)
- [async-cache](https://github.com/iamsinghrajat/async-cache)
- [Textual](https://github.com/textualize/textual/) Used for the amazing new graphical configurator
- [ssdp](https://github.com/codingjoe/ssdp) Used for auto discovery

## Projects using this project

- [Home Assistant Addon](https://github.com/bertybuttface/addons/tree/main/isponsorblocktv)

## Contributing

1. Fork it ([https://github.com/dmunozv04/iSponsorBlockTV/fork](https://github.com/dmunozv04/iSponsorBlockTV/fork))
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create a new Pull Request

## Contributors

[![Contributors](https://camo.githubusercontent.com/7f2198562c5936c544b2e090d1689691f187980b22e3d9617abf8dfd53769ea2/68747470733a2f2f636f6e747269622e726f636b732f696d6167653f7265706f3d646d756e6f7a7630342f6953706f6e736f72426c6f636b5456)](https://github.com/dmunozv04/iSponsorBlockTV/graphs/contributors)

Made with [contrib.rocks](https://contrib.rocks).

## License

[![GNU GPLv3](https://camo.githubusercontent.com/9a7ffd111ccebe8def4a319a2f8b8892c71ec96874fe609d9e201ffe94df9faf/68747470733a2f2f7777772e676e752e6f72672f67726170686963732f67706c76332d3132377835312e706e67)](https://www.gnu.org/licenses/gpl-3.0.en.html)
