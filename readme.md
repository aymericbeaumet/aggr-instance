# aggr-instance

This repository is a ready-to-run [aggr](https://github.com/aymericbeaumet/aggr) instance. Its
workflow checks the sources every half hour, keeps the reading archive on the `aggr` branch, and
publishes a fast, installable static site on GitHub Pages. The stable `@v1` workflow automatically
uses the latest compatible aggr release.

## Make it yours

1. **[Fork this repo](https://github.com/aymericbeaumet/aggr-instance/fork).**
2. Enable workflows in the fork's Actions tab and run the `aggr` workflow once.
3. Edit the title and sources in `aggr.toml` whenever you like.

Your reader appears at `https://<you>.github.io/<repo>/`. Add a custom domain in
Settings → Pages if you want one; no repository changes are required.

Source discovery, labels, categories, themes, and defaults are documented in the
[aggr README](https://github.com/aymericbeaumet/aggr).
