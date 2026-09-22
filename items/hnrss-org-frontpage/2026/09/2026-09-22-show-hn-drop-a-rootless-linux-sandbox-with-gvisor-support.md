---
title: 'Show HN: Drop – A rootless Linux sandbox with gVisor support'
link: https://droprun.sh/
source: hnrss-org-frontpage
published: 2026-09-22T13:52:47Z
updated: 2026-09-22T13:52:47Z
first_seen: 2026-09-22T18:47:44.447844335Z
authors:
- mixedbit
summary: 'I created Drop because I always felt uneasy installing and running third-party programs using my main user account. A single compromised dependency means a full compromise of the system. What is even worse, because I ship software from my computer, a single compromised dependency can lead to compromise of all the users of my software. Containers and VMs are one solution, but for local work, they are often detrimental to productivity. It takes effort to configure a machine with all the tools and configs needed for productive work, but a container or a VM will be stripped of all these tools. This is great for production deployments, where the aim is a reproducible system with minimal dependencies, but can get in the way of productive local work. Drop is language independent, but the workflow is inspired by Python''s virtualenv. With virtualenv the environment isolation is only a convention that relies on installed dependencies being good citizens. With Drop the isolation is enforced. Each Drop environment gets its own writable and easily disposable home dir, with only selected config files and dirs from the original home mounted, mostly read-only. Drop uses Linux namespaces for isolation (user, mount, network, PID, IPC, cgroup), doesn''t require root and, as an option, uses gVisor user-space kernel, which adds protection against exploiting host kernel vulnerabilities to escape the sandbox. I don''t want to make the introductory post too lengthy, but I''m here to answer any questions and give any additional technical details. Note: This is my 3rd submission of the project, the first two did not draw attention. Since then I have added support for gVisor and created a project website to better explain the concept and organize documentation. Comments URL: https://news.ycombinator.com/item?id=49801329 Points: 119 # Comments: 40'
content: extracted
html: 2026-09-22-show-hn-drop-a-rootless-linux-sandbox-with-gvisor-support.html
preview:
  file: 2026-09-22-show-hn-drop-a-rootless-linux-sandbox-with-gvisor-support.preview-5c00551a9c45.webp
  width: 256
  height: 134
  color: '#b0b0b2'
images:
- source: https://droprun.sh/og-image.png
  original:
    file: 2026-09-22-show-hn-drop-a-rootless-linux-sandbox-with-gvisor-support.image-c565eca6c76c.png
    width: 1200
    height: 630
  color: '#f9f9f9'
---

## Linux sandboxing that doesn’t get in your way

Isolate programs and coding agents without leaving your familiar work environment

[Install Drop](https://droprun.sh/docs/installation/)

## Use cases

- ### Isolate coding agents

  Run agents with `--dangerously-skip-permissions` and let Drop enforce permissions at the OS level. A hallucinated `rm -rf ~` doesn’t touch your actual home dir. A prompt injection targeting `~/.ssh` finds nothing. A connection to services running on localhost is rejected.

- ### Isolate third-party programs

  Install programs from PyPI, npm or any other source without giving them full access to your user account. If an installed program is malicious or compromised in a supply chain attack, the damage is contained within the sandbox.

## How it works

- ### Disposable, isolated environments

  Inspired by Python’s virtualenv, Drop lets you create and enter easily disposable environments. Each environment has its own home directory while the original home is hidden.

- ### Your existing distribution

  Unlike Docker/Podman, Drop uses your existing distribution, so there is no container setup work: every program you’ve already installed is available in the sandbox.

- ### Flexible config language

  High-level TOML config lets you specify which files, dirs and local network services should be exposed to the sandbox. By default, all Drop environments share a base config, so you can configure Drop once and then create new environments without any configuration work.

- ### Rootless

  Drop doesn’t require root to run. It runs within a Linux user namespace, with its own process, mount, network, IPC and cgroup namespaces. Drop drops all the user namespace capabilities before executing a sandboxed program, so the program cannot do privileged operations within the user namespace, like bind mounts.

- ### gVisor integration

  As an option, Drop supports running programs on the gVisor user-space kernel. This is an additional isolation layer that prevents programs from accessing the host kernel directly, significantly reducing the potential to exploit kernel vulnerabilities.
