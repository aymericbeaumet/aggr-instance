# Aggr

This repository setups [aggr](https://github.com/aymericbeaumet/aggr) with one config
file and one GitHub workflow. The workflow fetches the sources every half hour, appends new items
to the `aggr` branch, and publishes a static reader on GitHub Pages.

## Make it yours

1. Fork this repository.
2. Edit the title and `[[sources]]` in `aggr.toml`. A source normally needs only a name, its
   canonical website URL, and an optional category; aggr discovers feeds automatically.
3. Enable workflows in the fork's Actions tab.
4. Run the `aggr` workflow once.

Your reader appears at `https://<you>.github.io/<repo>/`. Add a custom domain in Settings →
Pages if you want one; no repository changes are required. More source types, labels, categories,
themes and defaults are documented in the [aggr README](https://github.com/aymericbeaumet/aggr).
