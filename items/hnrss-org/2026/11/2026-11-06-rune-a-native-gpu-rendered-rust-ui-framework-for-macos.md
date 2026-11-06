---
title: 'Rune: A native GPU-rendered Rust UI framework for macOS, Windows, Linux and web'
link: https://github.com/anomalyco/rune
source: hnrss-org
published: 2026-11-06T16:31:32Z
updated: 2026-11-06T16:31:32Z
first_seen: 2026-11-06T23:27:52.101188226Z
authors:
- mik3y
content: extracted
html: 2026-11-06-rune-a-native-gpu-rendered-rust-ui-framework-for-macos.html
remote_preview:
  url: https://opengraph.githubassets.com/ac05cefb77f348a22ee3f3af049abbb71c05b1320a411b933b826d4145f0eb74/anomalyco/rune
  alt: 'Rune: Native UI framework. Contribute to anomalyco/rune development by creating an account on GitHub.'
---

A native GPU-rendered Rust UI framework for macOS, Windows, Linux and the web, with its own renderer, text, layout and springs.

Every app can run headless, so an agent can render to PNG, dump the element tree and drive it with scripted input.

[![Rune in motion: real apps, rune-kit components and GPU effects](https://github.com/anomalyco/rune/raw/dev/demo/rune-poster.png)](https://github.com/anomalyco/rune/blob/dev/demo/rune.mp4)

[![A coding agent app built with Rune](https://github.com/anomalyco/rune/raw/dev/demo/opencode/session-top.png)](https://github.com/anomalyco/rune/blob/dev/demo/opencode/session-top.png)

[![Lit chat interface](https://github.com/anomalyco/rune/raw/dev/demo/light/chat.png)](https://github.com/anomalyco/rune/blob/dev/demo/light/chat.png) [![Split diff component](https://github.com/anomalyco/rune/raw/dev/demo/kit/diff-split-dark.png)](https://github.com/anomalyco/rune/blob/dev/demo/kit/diff-split-dark.png)

[![Liquid metal effect](https://github.com/anomalyco/rune/raw/dev/demo/fx/liquid-metal.png)](https://github.com/anomalyco/rune/blob/dev/demo/fx/liquid-metal.png) [![Settings window](https://github.com/anomalyco/rune/raw/dev/demo/settings-dark.png)](https://github.com/anomalyco/rune/blob/dev/demo/settings-dark.png)

```rust
fn view(s: &State, _: &mut Cx<State>) -> El {
    let t = tok::theme(true);
    let label = text(if s.on { "Lights on" } else { "Lights off" })
        .font(type_::TITLE).color(t.text);
    let knob = el().key("knob").size(18.0, 18.0).radius(9.0).fill(t.knob)
        .absolute(if s.on { 18.0 } else { 2.0 }, 2.0);
    let switch = el().key("switch").role("switch").label("Lights").checked(s.on)
        .focusable(true).size(38.0, 22.0).radius(11.0)
        .fill(if s.on { tok::accent(0, true) } else { t.control_off })
        .motion(motion::SNAPPY)
        .on_press(|s: &mut State| s.on = !s.on)
        .child(knob);
    col().w_full().h_full().center().gap(16.0).fill(t.bg).child(label).child(switch)
}
```

## Use it with your agent

Give your coding agent this repo. Have it start from [`examples/hello`](https://github.com/anomalyco/rune/blob/dev/examples/hello) and [`crates/rune/README.md`](https://github.com/anomalyco/rune/blob/dev/crates/rune/README.md), then check its own work headlessly by rendering PNGs and inspecting the element tree.

> Build me a with Rune (github.com/anomalyco/rune). Start from examples/hello and check your work headlessly.

## Add it

```toml
rune = { git = "https://github.com/anomalyco/rune", rev = "<dev rev>" }
```

Rune is pre-release. Pin a rev of `dev`; `dev` is never rewritten.

[Docs](https://github.com/anomalyco/rune/blob/dev/crates/rune/README.md) · [Components](https://github.com/anomalyco/rune/blob/dev/crates/rune-kit/README.md) · [Examples](https://github.com/anomalyco/rune/blob/dev/examples) · [Changelog](https://github.com/anomalyco/rune/blob/dev/CHANGELOG.md)

## Contributing

We don't take pull requests. Have your agent [open an issue](https://github.com/anomalyco/rune/issues) describing the bug or idea, and our agents usually turn it around fast.

MIT, © Anomaly.
