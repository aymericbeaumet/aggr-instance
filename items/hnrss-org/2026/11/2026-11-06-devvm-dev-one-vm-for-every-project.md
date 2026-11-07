---
title: 'Devvm.dev: One VM for every project'
link: https://devvm.dev/
source: hnrss-org
published: 2026-11-06T17:30:22Z
updated: 2026-11-06T17:30:22Z
first_seen: 2026-11-07T03:08:00.597922242Z
authors:
- luispa
content: extracted
html: 2026-11-06-devvm-dev-one-vm-for-every-project.html
remote_preview:
  url: https://devvm.dev/images/social-card.png
---

[Free and open source](https://github.com/devvmhq/devvm)

## One VM for every project

An isolated sandbox with your project’s toolchain, ready for you or a coding agent to start coding.

[Install](https://devvm.dev/docs/getting-started/install/) [Read the docs](https://devvm.dev/docs/)

## Quick start

### 1\. Go to your project

Open a terminal and `cd` into any project folder.

### 2\. Run devvm

devvm shows what the new VM gets. Confirm to make it, or customize it.

### 3\. Start your agent

devvm opens a shell in the VM, whose prompt starts with `(devvm)`, so start your agent there.

Claude Code needs your Claude login before its first start, which `devvm agent login claude` makes from the VM and keeps on your machine ([your first VM](https://devvm.dev/docs/getting-started/first-vm/#claude-codes-login)). The next time you run devvm in the folder, it opens the same VM, and `devvm tools` adds an agent to a VM you already have.

## What’s supported

### Toolchains

`devvm` chooses the toolchains your project’s files call for, and you can choose more when you customize a new VM or name them with `--tools` ([toolchains](https://devvm.dev/docs/using/toolchains/)).

### Coding agents

Claude Code, Codex and opencode run in the VM. Choose one when you customize a new VM, or choose them for every new VM in `devvm setup` ([coding agents](https://devvm.dev/docs/using/coding-agents/)).

### Layers

devvm ships the `docker`, `nix` and `desktop` layers, which you choose when you customize a new VM, and `devvm layer new` creates your own custom layers you can apply to any of your VMs ([layers](https://devvm.dev/docs/using/images/#layers)).

## Why use devvm?

- Developers face attacks every day, such as [supply chain attacks](https://en.wikipedia.org/wiki/Supply_chain_attack), leaked secrets, [prompt injections](https://en.wikipedia.org/wiki/Prompt_injection) and rogue agents.
- A sandbox limits the [blast radius](https://en.wikipedia.org/wiki/Blast_radius#Uses_in_software) of an attack to what’s inside it.
- You know you *should* run your code and agents in a sandbox, but most tools need configuration and setup that’s hard to get right.
- `devvm` ships with sensible defaults so you can start coding in a sandbox with a single command.

## Developer ready, zero config

- The base image comes with [optional extras](https://devvm.dev/docs/using/customizing/#base-image-features) that you choose, such as oh-my-bash, fzf, zoxide (`z`), tree and tldr.
- Every VM gets your git name and email, so the commits made in it are authored by you or your agent.
- Ports forward on their own, so a dev server on `:5173` in the VM opens at `localhost:5173` on your machine ([ports](https://devvm.dev/docs/using/ports-size/#ports)).
- `devvm code` opens the VM in VS Code over Remote-SSH ([devvm code](https://devvm.dev/docs/using/ssh/#devvm-host-and-devvm-code)).

## Isolated sandbox

- The VM mounts your project folder at the same path. Apart from your agents’ settings from devvm’s own store, which it gets read-only, it sees nothing else in your home folder ([what a VM can reach](https://devvm.dev/docs/safety/#what-a-vm-can-reach)).
- New VMs get the allowlist [network tier](https://devvm.dev/docs/safety/network/) by default, so they reach only an allowlist of hosts, through a proxy on your machine. You can choose a looser tier, internet or open, or a stricter one, none.
- On macOS, `devvm` locks `.git`’s hooks and config, so the VM can’t change what git runs on your machine ([.git protection](https://devvm.dev/docs/safety/git-protection/)).
- With [`--clone`](https://devvm.dev/docs/using/clone/), the VM works on a clone of the repo of its own instead of your folder, so nothing in it can touch your working tree or `.git`, and its work reaches your repo only when you fetch it.
- No sandbox closes every gap, so [attack vectors](https://devvm.dev/docs/safety/attack-vectors/#narrowed-not-closed) lists what’s left open, and [staying safe](https://devvm.dev/docs/safety/#the-habits) lists the habits that close most of it.

## Ready in seconds

- The first VM builds a base image, and every later VM is a copy of it that starts in seconds ([images](https://devvm.dev/docs/using/images/)).
- `devvm` builds each version of a toolchain once and shares it read-only, so all VMs use the same copy ([shared tools](https://devvm.dev/docs/using/images/#shared-tools)).

## Made for coding agents

- The agent runs in the VM, so an attack on it stays in the sandbox. It gets your settings, skills and MCP servers but never your history ([coding agents](https://devvm.dev/docs/using/coding-agents/)).
- On every network tier, their secrets, such as your Claude Code token and your Codex login, stay on your machine, because the VM gets placeholders and devvm’s proxy puts the real ones into its requests on the way out.

## Make it yours

- Your own layers are setup scripts that `devvm` builds into an image: `devvm layer new mailpit` starts one, and `devvm --layers mailpit` makes a VM with it ([your own layers](https://devvm.dev/docs/using/customizing/#your-own-layers)).
- `devvm setup --layers` gives every new VM the layers you choose, so your shell setup, aliases or dotfiles can go in a layer of your own that each VM gets unless you leave it out ([extras of your own](https://devvm.dev/docs/using/customizing/#extras-of-your-own)).
- `--from` starts the base image from a container image instead of Ubuntu’s, such as `node:24` or your team’s own ([starting from a container image](https://devvm.dev/docs/using/from/)).
- When the project has a `devcontainer.json`, `devvm` sets up its VM from the file’s container image, ports, environment and commands ([devcontainer.json](https://devvm.dev/docs/using/devcontainer/)).

## FAQ

### What does it run on?

`devvm` runs on macOS and on Linux, on arm64 or x86\_64. It needs [Lima](https://lima-vm.io/), which runs the VMs with Apple’s Virtualization framework on macOS and QEMU on Linux ([backends](https://devvm.dev/docs/backends/)). Homebrew installs Lima along with devvm, and on Linux, Lima also needs QEMU and access to `/dev/kvm`. [Installing](https://devvm.dev/docs/getting-started/install/) lists every requirement, and [known limits](https://devvm.dev/docs/using/limits/#linux-hosts) lists what Linux hosts lack, such as `.git` protection.

### Does the agent run on my machine?

No. You start the agent in the VM’s shell, so it runs in the VM, and you never install it on your machine. There it sees only [what the VM can reach](https://devvm.dev/docs/safety/#what-a-vm-can-reach).

### Is it free?

Yes. `devvm` is open source under the [MIT license](https://github.com/devvmhq/devvm/blob/main/LICENSE).

## Start coding in a sandbox

Install `devvm`, then run `devvm` in any project folder.

[Install](https://devvm.dev/docs/getting-started/install/) [Read the docs](https://devvm.dev/docs/)
