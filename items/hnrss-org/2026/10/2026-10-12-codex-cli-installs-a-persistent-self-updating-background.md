---
title: Codex CLI installs a persistent, self-updating background daemon by default
link: https://github.com/openai/codex/issues/48195
source: hnrss-org
published: 2026-10-12T18:29:04Z
updated: 2026-10-12T18:29:04Z
first_seen: 2026-10-12T22:35:36.108955126Z
authors:
- whimblepop
content: extracted
html: 2026-10-12-codex-cli-installs-a-persistent-self-updating-background.html
extra:
  comments_url: https://news.ycombinator.com/item?id=50060694
  num_comments: 212
  points: 375
---

### What variant of Codex are you using?

CLI — 0.157.0, installed via `npm install -g @openai/codex`, Linux (WSL2)

### What feature would you like to see?

Please make `daemon_auto_start` **opt-in (default `false`) for the CLI**, or at least ask once before turning it on.

Since 0.157.0, just opening an interactive `codex` session:

1. **Starts a persistent background daemon** (`codex app-server --managed-daemon`). It runs in its own session with no TTY and keeps running after every Codex window is closed (reparented to PID 1) until `codex app-server daemon stop` or a reboot.
2. **Copies the whole platform package** (~374 MB: `codex`, `codex-code-mode-host`, bundled `bwrap`/`zsh`/`rg`) into `~/.codex/packages/app-server-daemon/releases/<version>/` — byte-identical to what npm had just installed — and runs the daemon from that copy. From then on the backend's version is decoupled from the CLI version I installed with my package manager.
3. **Starts a detached updater** (`codex app-server daemon pid-update-loop`) that checks for updates 5 minutes after start and then hourly, downloads new releases and restarts the backend (which can kill active turns, see [app-server daemon: auto-update force-kills active turns after a 60s drain budget, and cannot be disabled #40969](https://github.com/openai/codex/issues/40969)).
4. **Pins the environment of whichever session happened to start it.** I launch Codex through a wrapper that sets `HTTPS_PROXY` per invocation; the daemon keeps the first session's proxy variables and its outbound connections go through that proxy, whatever later sessions are launched with. [Hooks in --remote mode receive the daemon's cwd, not the session's #42457](https://github.com/openai/codex/issues/42457) and [Hooks spawned by the app-server daemon cannot identify the client terminal, breaking terminal-targeting hooks #37537](https://github.com/openai/codex/issues/37537) (daemon cwd / terminal instead of the session's) look like the same root cause.

For a CLI, the least surprising default is that the backend lives and dies with the window, runs the binary I installed, and doesn't update itself in the background. A shared, self-updating daemon makes sense for Remote Control / the desktop app, so it would be fine to enable it there, or when the user explicitly runs `codex app-server daemon start` / `bootstrap`.

If it stays on by default, please at least:

- exit an auto-started daemon when its last client disconnects;
- not copy/self-update when the CLI was installed by a package manager (npm/brew/pacman) — run the installed binary instead ([Support externally managed app-server daemon executables #41188](https://github.com/openai/codex/issues/41188), [npm update leaves app-server on old version, but daemon update/restart refuse recovery #46468](https://github.com/openai/codex/issues/46468), [Codex 0.157.0 no longer starts with the Arch Linux package unless --no-daemon is used #48050](https://github.com/openai/codex/issues/48050)).

### Additional information

**Bug found while turning it off:** `codex app-server daemon stop` stops the app-server but leaves the `pid-update-loop` updater running (still alive 70 s later, had to kill it manually).

Workaround I'm using:

```shell
codex features disable daemon_auto_start
codex app-server daemon stop
pkill -f 'app-server daemon pid-update-loop'   # not stopped by `daemon stop`
rm -rf ~/.codex/packages/app-server-daemon
```

With the flag off, a new TUI session no longer spawns a daemon.

Related: [#40969](https://github.com/openai/codex/issues/40969), [#41188](https://github.com/openai/codex/issues/41188), [#46468](https://github.com/openai/codex/issues/46468), [#48050](https://github.com/openai/codex/issues/48050), [#42457](https://github.com/openai/codex/issues/42457), [#37537](https://github.com/openai/codex/issues/37537)
