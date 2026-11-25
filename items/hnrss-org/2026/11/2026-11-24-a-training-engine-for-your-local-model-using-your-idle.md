---
title: A training engine for your local model, using your idle compute
link: https://github.com/sous-labs/songe
source: hnrss-org
published: 2026-11-24T15:03:40Z
updated: 2026-11-24T15:03:40Z
first_seen: 2026-11-25T01:32:55.949141972Z
authors:
- bluepeter
content: extracted
html: 2026-11-24-a-training-engine-for-your-local-model-using-your-idle.html
remote_preview:
  url: https://repository-images.githubusercontent.com/1465322684/f6c97c41-6e40-4e48-83c6-1b20210672d6
  alt: A training engine for your local model, using your idle compute - sous-labs/songe
---

[![Songe: a bed on the moon, and the command pip install songe](https://camo.githubusercontent.com/82051f7fde58e6bc111457fa1d4129643a122446dd0a0b6ca82d1fed985f4d43/68747470733a2f2f736f75736c6162732e61692f6173736574732f736f6e67652d62616e6e65722d76312e676966)](https://souslabs.ai)

[Install](https://github.com/sous-labs/songe#-install) • [How it works](https://github.com/sous-labs/songe#-how-it-works) • [Commands](https://github.com/sous-labs/songe#-commands) • [Roadmap](https://github.com/sous-labs/songe/blob/main/ROADMAP.md) • [Discord](https://discord.gg/cBEywVkXjY) • [Website](https://souslabs.ai)

[![Join our Discord](https://camo.githubusercontent.com/3ecf03a81de0a5b385249049834bcdbfffd538b9c290ec5863eb06102ac8edb6/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f4a6f696e5f6f75725f446973636f72642d3538363546323f7374796c653d666f722d7468652d6261646765266c6f676f3d646973636f7264266c6f676f436f6c6f723d7768697465)](https://discord.gg/cBEywVkXjY)

[![PyPI version](https://camo.githubusercontent.com/90654e5322030eeebc52fc86b79f2eaa0de001e1a3ac5b193436b0f4f6397090/68747470733a2f2f696d672e736869656c64732e696f2f707970692f762f736f6e67652e737667)](https://pypi.org/project/songe/) [![Python versions](https://camo.githubusercontent.com/6294044b1c6b0a199a83a4c2bd9747f64a331f9c07056a66c50cb15a6dcca483/68747470733a2f2f696d672e736869656c64732e696f2f707970692f707976657273696f6e732f736f6e67652e737667)](https://pypi.org/project/songe/) [![License: Apache-2.0](https://camo.githubusercontent.com/1f26c51f1e777b85e637830b2dbdc424124909a6779beb24429527b26ac8230a/68747470733a2f2f696d672e736869656c64732e696f2f6769746875622f6c6963656e73652f736f75732d6c6162732f736f6e67652e737667)](https://github.com/sous-labs/songe/blob/main/LICENSE)

Use your local model as usual through opencode, pi or any OpenAI-compatible tool. While you work, Songe notices what happens after each of the model's actions: the tool result, the error, your next message. When your agent is quiet, it teaches the model to see those coming. A new version is used only if it passes its checks, and nothing ever leaves your machine.

## ✨ Highlights

- **Learns from how you work.** Your corrections, stated preferences and the tools' errors become lessons for your own model.
- **Two kinds of Dreams.** A Quick Dream adds your preferences to the model's instructions in minutes; a Deep Dream trains the model itself while you are away.
- **Safe by default.** A new version is used only if it does better on your own conversations; one click goes back to any earlier version.
- **Stays out of the way.** Runs only when your agent is quiet, memory is free and the battery is fine, and stops the moment you are back.
- **Private.** No account, no telemetry, no cloud. Songe never runs your tools.

## 📥 Install

With [uv](https://docs.astral.sh/uv/) or [pipx](https://pipx.pypa.io/) (Python 3.11 or later):

```shell
uv tool install songe        # or: pipx install songe
songe init
```

No uv or pipx yet? Either installs in one line:

```shell
curl -LsSf https://astral.sh/uv/install.sh | sh    # uv (or: brew install uv)
brew install pipx                                  # pipx on macOS (Debian/Ubuntu: sudo apt install pipx)
```

Both give Songe its own environment in a lasting place, which its background service needs.

macOS with Homebrew:

```shell
brew tap sous-labs/tap
brew trust sous-labs/tap     # newer Homebrew asks this once
brew install songe
songe init
```

`songe init` finds your model server and your agents, shows what it will change, and asks once:

```text-shell-session
$ songe init

  Models    3 from LM Studio, listed under Songe
  Agent     opencode → Songe models · default granite-4.2-3b-MLX-4bit
  Agent     pi → Songe models · choose one with /model
  Data      ~/Library/Application Support/songe
  Service   starts at login
  Learning  new conversations · starts when idle
  Replace   loaded copy when a better version is ready

Install? (Y/n)
```

Then pick a Songe model in your agent (opencode lists them as `songe/songe/<name>`), and work as usual.

Plain pip, and what you need

`pip install songe` works inside a virtual environment; outside one, Homebrew's and most Linux distributions' Python refuse it.

You need a model served locally, and an agent:

|               | Supported                                                    | Tested end to end               |
| ------------- | ------------------------------------------------------------ | ------------------------------- |
| Model servers | LM Studio, vLLM, Ollama, llama.cpp, mlx-lm                   | LM Studio (macOS), vLLM (Linux) |
| Agents        | opencode and pi (set up for you); any OpenAI-compatible tool | opencode, pi                    |
| Training      | Apple Silicon (MLX); NVIDIA and AMD (PyTorch)                | Apple Silicon, NVIDIA           |

Machines without a GPU still serve and record, but do not train. Tried Songe on another setup? Share the result on the [testing board](https://github.com/sous-labs/songe/issues/15).

## 🌙 How it works

```
 your agent ──► Songe (127.0.0.1:7613) ──► your model server
                   │ records what happened
                   ▼
            when you are away: a Dream
   read lessons ─► practice ─► train ─► check ─► use only if better
```

|         | Quick Dream                                                           | Deep Dream                                                         |
| ------- | --------------------------------------------------------------------- | ------------------------------------------------------------------ |
| Takes   | a few minutes                                                         | an hour or more                                                    |
| Does    | adds the preferences you stated or showed to the model's instructions | writes practice from your lessons and trains a small adapter on it |
| You see | each new preference, with a before/after where the answer changes     | why the new version was or was not used                            |
| Undo    | undo the Quick Dream, or remove one preference                        | go back to any earlier version                                     |

With **auto-dream** on, Dreams start when your agent has been quiet for a while. Or press **Dream** in the dashboard (`songe open`). A Dream never just disappears: it keeps going on the page, or says why it stopped.

How it learns, in more detail

- **Reading.** Songe reads each conversation for the moments that teach something: your corrections and stated rules, retries, tool errors.
- **Practice.** For each lesson your model writes short practice scenes in your agent's own tools. Your real conversations are never trained on.
- **Training.** The model as it was, shown the lesson, is the teacher; the same model with a small adapter, shown only the scene, is the student (on-policy distillation). Earlier skills are rehearsed so they stay.
- **Checking.** At the moments you reacted to, the version in use and the new one each answer, and a judge compares what they do. The new version is used only if it does clearly better, does not change what already worked, and its answers stay healthy.

Before the first training Songe measures how much your computer can learn at once, and trains on what fits. See [DESIGN.md](https://github.com/sous-labs/songe/blob/main/DESIGN.md).

## ⌨️ Commands

| Command                           | What it does                                          |
| --------------------------------- | ----------------------------------------------------- |
| `songe`                           | what it is doing                                      |
| `songe status --training [MODEL]` | resolved Deep Dream settings and where they come from |
| `songe open`                      | the dashboard in your browser                         |
| `songe models`                    | each model, and whether it dreams on its own          |
| `songe models MODEL on&#124;off`  | auto-dream for one model                              |
| `songe sleep`                     | start a Deep Dream now                                |
| `songe undo MODEL`                | go back one version                                   |
| `songe pause` / `songe resume`    | stop and restart learning                             |
| `songe doctor`                    | check the setup                                       |
| `songe forget`                    | delete recorded conversations (learned versions stay) |
| `songe uninstall`                 | stop the service and restore your agent settings      |

What it changes on your machine

- Its data folder: `~/Library/Application Support/songe` on macOS, `$XDG_DATA_HOME/songe` on Linux, `%LOCALAPPDATA%\songe` on Windows.
- A `songe/` folder in LM Studio's models folder with the current learned version. Your own models are never touched.
- One provider entry in opencode and pi, with a backup.
- A background service that starts at login.

Upgrade and remove

`uv tool upgrade songe`, `pipx upgrade songe` or `brew upgrade songe`: the service moves to the new version by itself in a quiet moment.

`songe uninstall` stops the service and restores your agent settings; add `--purge` to also delete its data and learned versions. Run it before `brew uninstall songe`.

## 🧭 Roadmap and contributing

Where Songe is going: [ROADMAP.md](https://github.com/sous-labs/songe/blob/main/ROADMAP.md). Ideas and bugs are welcome as [issues](https://github.com/sous-labs/songe/issues). Questions and results from your nights go in [Discord](https://discord.gg/cBEywVkXjY). To contribute, see [CONTRIBUTING.md](https://github.com/sous-labs/songe/blob/main/CONTRIBUTING.md). Security reports: [SECURITY.md](https://github.com/sous-labs/songe/blob/main/SECURITY.md).

Licensed under [Apache-2.0](https://github.com/sous-labs/songe/blob/main/LICENSE).
