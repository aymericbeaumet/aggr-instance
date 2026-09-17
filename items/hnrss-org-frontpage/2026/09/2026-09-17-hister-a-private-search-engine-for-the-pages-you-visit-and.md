---
title: 'Hister: A private search engine for the pages you visit and the files you keep'
link: https://github.com/asciimoo/hister
source: hnrss-org-frontpage
published: 2026-09-17T16:25:37Z
updated: 2026-09-17T16:25:37Z
first_seen: 2026-09-17T20:33:50.623063168Z
authors:
- bookofjoe
summary: 'Article URL: https://github.com/asciimoo/hister Comments URL: https://news.ycombinator.com/item?id=49743097 Points: 291 # Comments: 94'
content: extracted
html: 2026-09-17-hister-a-private-search-engine-for-the-pages-you-visit-and.html
preview:
  file: 2026-09-17-hister-a-private-search-engine-for-the-pages-you-visit-and.preview-b53b42b6749e.webp
  width: 256
  height: 128
  alt: Your own search engine. Contribute to asciimoo/hister development by creating an account on GitHub.
  color: '#ecf0f3'
images:
- source: https://opengraph.githubassets.com/b168f1d3a4668b2df61c10ab1c524c6aa09423a79072eb8ebc4b90e486645dfc/asciimoo/hister
  original:
    file: 2026-09-17-hister-a-private-search-engine-for-the-pages-you-visit-and.image-f95b4e0c7f7d.png
    width: 1200
    height: 600
  variants:
  - file: 2026-09-17-hister-a-private-search-engine-for-the-pages-you-visit-and.image-884125e67a49.webp
    width: 320
    height: 160
  - file: 2026-09-17-hister-a-private-search-engine-for-the-pages-you-visit-and.image-f6a082937feb.webp
    width: 640
    height: 320
  - file: 2026-09-17-hister-a-private-search-engine-for-the-pages-you-visit-and.image-594392d5ecb0.webp
    width: 960
    height: 480
  - file: 2026-09-17-hister-a-private-search-engine-for-the-pages-you-visit-and.image-21496d4cfd94.webp
    width: 1200
    height: 600
  color: '#fefefe'
- source: https://github.com/asciimoo/hister/raw/master/webui/website/src/lib/assets/screenshot.png
  original:
    file: 2026-09-17-hister-a-private-search-engine-for-the-pages-you-visit-and.image-b48cb709952b.png
    width: 1052
    height: 710
  variants:
  - file: 2026-09-17-hister-a-private-search-engine-for-the-pages-you-visit-and.image-95376f647c32.webp
    width: 320
    height: 216
  - file: 2026-09-17-hister-a-private-search-engine-for-the-pages-you-visit-and.image-f8274d5ee844.webp
    width: 640
    height: 432
  - file: 2026-09-17-hister-a-private-search-engine-for-the-pages-you-visit-and.image-4b67b659f352.webp
    width: 1052
    height: 710
  color: '#e7e7e8'
- source: https://github.com/asciimoo/hister/raw/master/webui/website/src/lib/assets/demo.gif
  original:
    file: 2026-09-17-hister-a-private-search-engine-for-the-pages-you-visit-and.image-ef45f3ea1db7.gif
    width: 1021
    height: 613
  color: '#000000'
- source: https://github.com/asciimoo/hister/raw/master/webui/website/static/uruky.svg
  original:
    file: 2026-09-17-hister-a-private-search-engine-for-the-pages-you-visit-and.image-e727ad33b71e.png
    width: 261
    height: 90
  variants:
  - file: 2026-09-17-hister-a-private-search-engine-for-the-pages-you-visit-and.image-702c15a090e3.webp
    width: 261
    height: 90
  color: '#000000'
---

**Your own search engine**

Hister is a private search engine for the pages you visit and the files you keep. It indexes their full contents so you can find information again from the web interface, terminal, or an AI assistant connected through MCP.

[Try the demo](https://demo.hister.org/) · [Download Hister](https://github.com/asciimoo/hister/releases/latest) · [Read the quickstart](https://hister.org/docs/quickstart) · [Documentation](https://hister.org/docs)

[![Hister web interface](https://github.com/asciimoo/hister/raw/master/webui/website/src/lib/assets/screenshot.png)](https://github.com/asciimoo/hister/blob/master/webui/website/src/lib/assets/screenshot.png)

## Quickstart

[](https://github.com/asciimoo/hister#quickstart)

1. Download the binary for your platform from the [latest release](https://github.com/asciimoo/hister/releases/latest), then rename it to `hister` (`hister.exe` on Windows).

2. On Linux or macOS, make it executable:

   ```
   chmod +x hister
   ```

3. Start Hister on Linux or macOS:

   ```
   ./hister listen
   ```

   On Windows, run `.\hister.exe listen` in PowerShell.

   Keep this terminal open while using Hister. The server must be running to index pages and search them.

4. Open [http://127.0.0.1:4433](http://127.0.0.1:4433) and install the browser extension for [Firefox](https://addons.mozilla.org/en-US/firefox/addon/hister/) or [Chrome](https://chromewebstore.google.com/detail/hister/cciilamhchpmbdnniabclekddabkifhb).

5. Visit a web page with the extension enabled, then return to Hister and search for a phrase from that page to find your first indexed result.

No configuration is required for a local personal setup. See the [complete quickstart](https://hister.org/docs/quickstart) to choose what Hister indexes.

To search existing content, [import browser history](https://hister.org/docs/import#importing-browser-history), [index local directories](https://hister.org/docs/configuration#local-directory-indexing), or [import files](https://hister.org/docs/import#importing-files).

Alternative installation methods include Homebrew (`brew install hister`), Docker, and Nix. See the [installation guide](https://hister.org/docs/installing) for instructions.

## Features

[](https://github.com/asciimoo/hister#features)

- **Privacy focused**: No telemetry or mandatory cloud service. Run Hister locally or on infrastructure you control.
- **Full text indexing**: Search the actual contents of visited pages and local files, not only titles and URLs.
- **Automatic browser indexing**: Save newly visited pages with the Firefox or Chrome extension.
- **Powerful queries**: Use field filters, phrases, wildcards, negation, aliases, and result priorities.
- **Optional semantic search**: Find documents by meaning through an embeddings endpoint you configure.
- **Crawler and browser import**: Index websites or bring in existing browser history.
- **Web, terminal, and MCP clients**: Search from the browser, TUI, command line, or an AI assistant.
- **Multi user support**: Keep each user's documents and search results separate on a shared server.

[![Hister terminal interface](https://github.com/asciimoo/hister/raw/master/webui/website/src/lib/assets/demo.gif)](https://github.com/asciimoo/hister/blob/master/webui/website/src/lib/assets/demo.gif)

## Privacy

[](https://github.com/asciimoo/hister#privacy)

By default, Hister has no telemetry and no cloud sync. The browser extension sends indexed page content only to the Hister server you configure, apart from downloading page favicons. The server stores documents and search indexes on that server.

Optional semantic search sends document text to the embeddings endpoint you choose. Review the [privacy overview](https://hister.org/docs/intro#privacy) and [semantic search configuration](https://hister.org/docs/configuration#semantic-search) before enabling remote integrations.

## Why Hister?

[](https://github.com/asciimoo/hister#why-hister)

Unlike traditional search engines, Hister builds a personal search index from the web pages and files you choose to keep. Your content stays on your Hister server, making it useful for finding information you've already encountered without relying on a third-party search provider.

## Development

[](https://github.com/asciimoo/hister#development)

Requirements are Go 1.26, npm, and a C compiler for CGO dependencies.

```
git clone https://github.com/asciimoo/hister.git
cd hister
./manage.sh build
```

To work on the web app with hot reload and automatic Go rebuilds:

```
npm run serve:app
```

This starts a Vite development server and the Go backend with automatic rebuilds through [air](https://github.com/air-verse/air).

## Community and contributing

[](https://github.com/asciimoo/hister#community-and-contributing)

Join us on IRCNet in `#hister` or on [Discord](https://discord.gg/beEyuHxRSs).

Read [CONTRIBUTING.md](https://github.com/asciimoo/hister/blob/master/CONTRIBUTING.md) before submitting a change. Bugs and suggestions belong in the [issue tracker](https://github.com/asciimoo/hister/issues). For security reports, see [SECURITY.md](https://github.com/asciimoo/hister/blob/master/SECURITY.md).

## Sponsors

[](https://github.com/asciimoo/hister#sponsors)

[![Uruky](https://github.com/asciimoo/hister/raw/master/webui/website/static/uruky.svg)](https://uruky.com/)

## License

[](https://github.com/asciimoo/hister#license)

[AGPLv3](https://github.com/asciimoo/hister/blob/master/LICENSE) or any later version
