---
title: Docker is weird
link: https://codeandcake.dev/posts/2026-10-22-docker-is-weird
source: hnrss-org
published: 2026-10-22T23:45:56Z
updated: 2026-10-22T23:45:56Z
first_seen: 2026-10-23T04:34:35.440043099Z
authors:
- psxuaw
content: extracted
html: 2026-10-22-docker-is-weird.html
---

I decided to make my own container runtime for fun, and even though I’m still early on, I’ve learned that Docker is *weird*. Here’s some weird things I learned about Docker.

## Containers aren’t real

Unlike VMs, containers are just processes with special restrictions on them. Your kernel is still running that process! And if you know where to look, you can even find that process in /proc.

## Containers really aren’t real

There’s not really a linux feature that’s just “containers” either. Instead, containers are really 5 different linux kernel features in trenchcoat.

Namespaces hide what the host and container can see, cgroups limit container resource usage, and there’s a whole bunch of networking magic I don’t understand that lets you isolate a container’s network.

## Images are just folders

Images are just folders that Docker puts somewhere and then calls `chroot` (or `pivot_root`) to make that folder your container process’s new root directory. So if you run `docker run -it --rm ubuntu /bin/bash`, Docker is going to:

- Download the ubuntu image from Dockerhub.
- Put that image somewhere.
- Run `/some/weird/docker/path/bin/bash`

## Go is kind of a terrible language for a container runtime

Docker’s container runtime, `runc`, is written in Go, and Go has some design choices that make it less suited for a container runtime. Go’s runtime is natively multi-threaded. That’s a really cool feature, but it leads to some challenges in `runc`.

Many of the Linux syscalls a container runtime needs to make have to be done in a single-threaded environment, so `runc` has [a crazy C module](https://github.com/opencontainers/runc/tree/main/libcontainer/nsenter) that runs before Go’s runtime initializes so it can run those syscalls.

Go also doesn’t have fork! Fork only forks the main thread, so it’s kind of a footgun in a runtime like Go’s. But a container runtime really wants to fork, set up a bunch of restrictions, and then exec the process you actually put in `docker run`. To get around this, `runc` execs `/proc/self/exe` with a special command line arguement that identifies it as a child process to approximate a fork.

## Docker requires root!

This is pretty easy to miss if you, like me, run Docker in MacOS. Many of the Linux kernel APIs that container runtimes use require, or at least used to, to be run as root, so the Docker daemon usually runs as root. I can only imagine that container security is a very tough job.

This seems better these days. The big exception seems to be networking. If you’re running a rootless container runtime, it looks like it has to reimplement the whole networking stack in userspace to actually give you the same features!

## Learning is fun!

I’ve only just started my own container runtime, and I already feel like I’ve learned so much about Docker. Writing your own version of a tool you use is a great way to learn about it!
