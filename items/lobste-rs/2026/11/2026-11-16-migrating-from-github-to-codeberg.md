---
title: Migrating from GitHub to Codeberg
link: https://yorickpeterse.com/articles/migrating-from-github-to-codeberg/
source: lobste-rs
published: 2026-11-16T05:10:34Z
updated: 2026-11-16T05:10:34Z
first_seen: 2026-11-17T01:00:47.489879523Z
authors:
- yorickpeterse.com by yorickpeterse
labels:
- programming
- vcs
summary: Comments
content: extracted
html: 2026-11-16-migrating-from-github-to-codeberg.html
remote_preview:
  url: https://yorickpeterse.com/images/migrating-from-github-to-codeberg/server_closet.avif?hash=f48a500867aeec938ac42a56eef58961c1b526a4fb9d79187499f3afcfd5639d
  alt: OVH would be proud of this setup
---

Nov 13, 2026

I created my GitHub account back in May 2009, and like most developers much of my work since then has taken place on GitHub; both private and public work. In August 2015 I joined GitLab, and in the years followed I primarily used GitLab for my own work as well as for [Inko](https://inko-lang.org/). Part of this was because at the time GitHub was neglecting its product, and because I wanted to eat my own dog food so to speak.

In 2023, [I migrated Inko along with my personal projects back to GitHub](https://inko-lang.org/news/inko-has-migrated-back-to-github/). One of the goals was to make it easier to contribute to Inko and to increase awareness and popularity of it, as GitHub is where most people go to look for new projects; especially as the quality of search engines has degraded over the years. Another goal was that GitLab.com never took off as a popular alternative to GitHub in spite of it's promise, something that I'm still frustrated about, especially considering the effort made towards this goal by myself and many of my (former) GitLab colleagues from that time.

While moving back to GitHub did somewhat increase the amount of contributors to Inko, measuring the impact on visibility is a lot more difficult. Sure, the number of stars of the project has increased but that's ultimately a measurement of how many people use *GitHub* and not so much how many actually *like* the project and want to use it. For example, I myself at times star projects as a way of just bookmarking them so I can look into them deeper into the future; not so much because I think to myself "Damn, this is cool and I want to use this".

What *has* been clear though is the increase in low-quality/wasteful interactions on GitHub. This has always been an issue on GitHub due to the relative low barrier to entry, but at least some amount of work was still required. Thanks to the ongoing digital opioid crisis rise of "AI", this barrier dropped through the floor half way to China, for now one can "contribute" by just prompting a digital talking parrot high on cocaine Claude Code to submit pull requests containing thousands of lines of pure nonsense.

While the slop "contributions" I've had to deal with personally have been small enough/simple enough to deal with so far, the writing is on the wall: GitHub is no longer the place where developers collaborate, but rather a place where "agents" projectile-vomit slop that serves no other purpose but to boost their owner's ego.

Ignoring this, there are also issues with Microsoft/GitHub in general not related to AI, that made me want something that feels more in the spirit of free software.

Which brings me to [Codeberg](https://codeberg.org/). Codeberg, for those living under a rock, is a centralized (and probably the most popular) instance of [Forgejo](https://forgejo.org/), which is a fork of [Gitea](https://about.gitea.com/) which in turn is a fork of [Gogs](https://gogs.io/).

What I like about Codeberg in particular is it's [stance on hosting non-free software](https://docs.codeberg.org/getting-started/faq/#can-i-host-content-without-a-free-and-open-source-license%3F), its ["no AI here"](https://codeberg.org/Codeberg/org/src/branch/main/TermsOfUse.md#2-allowed-content-usage) stance, and it's organisational structure that should make enshittification less likely to happen. I'm not alone feeling drawn to Codeberg, with for example [Zig](https://ziglang.org/news/migrating-from-github-to-codeberg/) moving from GitHub to Codeberg for similar reasons about a year ago.

After thinking about moving to Codeberg for a while, I started the process of doing so a few weeks ago, starting with my personal repositories, followed by the repositories used for Inko. In total this meant going through 70+ repositories, though I did not migrate all them. For example, projects for which development ended years ago were not migrated.

"But Yorick this article is boring so far, I want the juicy details!" Yes yes, I know! Let's get to it.

Note that what follows below is a bit rough on the edges, as I have this recurring problem of thinking "I should write about this process as I do it", then proceed to *not* do that, and then have to recall everything from memory when I *do* write down my findings later.

## Table of contents

- [Migrating repositories](https://yorickpeterse.com/articles/migrating-from-github-to-codeberg/#migrating-repositories)
- [Replacing GitHub Actions with Forgejo Actions](https://yorickpeterse.com/articles/migrating-from-github-to-codeberg/#replacing-github-actions-with-forgejo-actions)
- [Self-hosting Forgejo Actions runners](https://yorickpeterse.com/articles/migrating-from-github-to-codeberg/#self-hosting-forgejo-actions-runners)
- [Building container images](https://yorickpeterse.com/articles/migrating-from-github-to-codeberg/#building-container-images)
- [Hosting private projects](https://yorickpeterse.com/articles/migrating-from-github-to-codeberg/#hosting-private-projects)
- [Codeberg reliability and performance](https://yorickpeterse.com/articles/migrating-from-github-to-codeberg/#codeberg-reliability-and-performance)
- [Storage quotas](https://yorickpeterse.com/articles/migrating-from-github-to-codeberg/#storage-quotas)
- [Conclusion](https://yorickpeterse.com/articles/migrating-from-github-to-codeberg/#conclusion)

## Migrating repositories

The first step was to determine *what* I wanted to migrate. If you only have a few repositories then the process is pretty easy, but when migrating 70+ repositories the process becomes a bit more difficult. The approach I took was pretty simple: actively maintained or otherwise interesting projects (e.g. from a historical perspective or those worth showcasing) were migrated, others were archived on GitHub and left as-is.

Next, I created [an issue on GitHub](https://github.com/inko-lang/inko/issues/1021) to track the progress and all the steps necessary (later moved to [a Codeberg issue](https://codeberg.org/inko-lang/inko/issues/1021)). Most notably, this issue contains a large list of TODOs to check off. TODOs are great, except for the part where GitHub at times resets the state of one item marked as done when you mark another item as done too quickly after the first one. Coding is apparently solved thanks to AI, but reliably updating a checkbox as "checked" is apparently still too difficult.

To migrate the repositories, I used Codeberg's (technically Forgejo's) [repository migration feature](https://codeberg.org/repo/migrate). While I likely could've automated this process, I ended up doing this by hand as I figured the time to properly automate it would likely exceed the time spent doing the work by hand, though in hindsight I think automating it probably would've been faster. The rest of the time is mainly spent waiting for the importer to do its work, followed by adjusting the relevant settings of each project (e.g. the available merge strategies for pull requests).

One minor nuisance is that when the GitHub importer hits a GitHub rate limit it won't notify you about this in any way, instead it just retains the last status (e.g. "Migrated 500 issues") until the rate limit is reset at which point it will continue its work. The first time this happened I thought the importer got stuck, until about an hour later it resumed work and managed to import all remaining data without any further issues.

Another minor nuisance is that the importer makes no attempt at linking GitHub users to any existing Codeberg issues, instead it essentially uses fake users (albeit with the corresponding GitHub usernames). This means that for example issues assigned to users on GitHub need to be manually assigned to their corresponding Codeberg users.

Oh, and one more thing: GitHub introduced the concept of "issue types" ("Bug", "Feature", etc) but Forgejo has no equivalent. If you make use of this, make sure to label these issues accordingly so you don't lose that information during a migration.

## Replacing GitHub Actions with Forgejo Actions

Codeberg offers two systems for continuous integration: [Woodpecker](https://woodpecker-ci.org/) and [Forgejo Actions](https://forgejo.org/docs/latest/user/actions/basic-concepts/). The latter is essentially a bootleg version of GitHub Actions, complete with everything that makes GitHub Actions a pain to deal with (e.g. semi Turing complete YAML). I went with Forgejo Actions because of its better integration with Forgejo and because at this stage I didn't feel like *also* rewriting all CI configuration for an entirely different platform.

On the surface migrating from GitHub Actions to Forgejo Actions seems straightforward, but there are various subtle differences that can make this more annoying than anticipated.

For example, Codeberg offers a [set of shared runners](https://codeberg.org/actions/meta#available-runners) but these come with strict constraints on job times and limited resources, such that they are only suitable for the most basic of workloads. The runners are also limited to AMD64 Linux, so if you need to run tests on for example macOS you're out of luck.

One important detail that *isn't* documented (at least that I could find) is that these shared runners *don't* pull container images if they already exist on the runner, or at least not always. So if you have a job that uses the image `foo:latest` and you update the tag to point to a newer version, you may need to wait an unspecified amount of time before the shared runners use this new version. In practise this makes it impossible to effectively use images/tags such as `alpine:latest` or `alpine:3`. Using digests instead of tags should work just fine, but doing so across many projects can quickly become annoying to manage, and it isn't always practical (e.g. when you rely on nightly container builds).

I ended up setting up my own Forgejo runners to work around these issues, using a Mac Mini M1 for ARM64 builds and a small home server that I already had set up for other purposes for AMD64 builds. I'll cover this in more detail below.

UI wise Forgejo Actions is a bit bare-bones compared to GitHub, so don't expect any fancy graphs showing your jobs and those that depend on them and so on, instead you just get a flat list of all jobs and their output.

While Forgejo Actions is based on GitHub Actions, it's not fully compatible with it. For example, more recent versions of [actions/upload-artifact](https://github.com/actions/upload-artifact) and [actions/download-artifact](https://github.com/actions/download-artifact) don't work with Forgejo. Similarly, when using GitHub Actions it's not necessary for custom container images to include Node.js, while Forgejo Actions *does* require this.

Given that Forgejo Actions is based on GitHub Actions it also suffers from the same problems, such as YAML being frustrating to work with, and it being difficult to effectively test CI config changes without pushing the changes to a branch and hoping for the best. I did briefly look at Woodpecker but it doesn't seem to be *that* different, and due to it being less well integrated into Forgejo I didn't bother looking into this further.

## Self-hosting Forgejo Actions runners

The current runner setup that I have involves two computers:

- An M1 Mac Mini I bought second-hand a few years ago for about €200 (an absolute steal)
- A Deskmini X300 with a Ryzen 5 5600G and 16 GiB of RAM, built a few years ago with the main purpose being some home automation and streaming of Blu-ray movies through [Jellyfin](https://jellyfin.org/). Had I known about the RAMpocalypse at the time I would've built it with 32 GiB or maybe even 64 GiB of RAM, but alas here we are

Both sit in a professional rack unit a utilities closet on the ground floor, complete with excellent cable management (Deskmini in the top left, Mac Mini on the bottom right between the router and switch):

![OVH would be proud of this setup](https://yorickpeterse.com/images/migrating-from-github-to-codeberg/server_closet.avif?hash=f48a500867aeec938ac42a56eef58961c1b526a4fb9d79187499f3afcfd5639d)

For Inko I want to test on all platforms it supports, which is Linux, macOS and FreeBSD, and on the two supported architectures: AMD64 and ARM64. As I don't have access to any Intel Mac hardware, testing against AMD64 macOS is no longer possible. This is unfortunate, but with Apple deprecating Intel hardware this was bound to happen at some point.

The way the runners are managed/updated differs a bit per platform. The Linux host on the Deskmini and the Linux ARM64 runner both use Fedora through [bootc](https://bootc.dev/) and are set to automatically pull and apply updates from my private Forgejo instance/container registry. The FreeBSD VMs are updated by building a new disk image, turning the VM off and then swapping the existing disk image with the newly built one.

The workload is divided across the hardware as follows:

- The Mac Mini runs macOS tests directly on the host, while FreeBSD ARM64 and Linux ARM64 tests run in a virtual machine (using [UTM](https://getutm.app/)). The FreeBSD VM is limited to 2 CPU cores and 4 GiB of memory, while the Linux VM is limited to 4 CPU cores and 4 GiB of memory
- The Deskmini runs Linux tests in containers, restricted to 4 CPU cores and 4 GiB of memory. FreeBSD AMD64 tests run in a VM with the same constraints as the FreeBSD VM running on the Mac Mini
- A runner that runs on my Netcup RS 1000 G12.5 root server, used to also serve this website and a private Forgejo instance. This runner is limited to only 2 CPU cores and mainly used for small jobs such as running linters

All runners are limited to only running a single job at a time. The exact division of CPU cores per VM/container is something I'm still experimenting with.

The goal of this setup is to prevent one job from trashing the CPU and thus affecting other jobs. For example, when I experimented with each job having access to all CPU cores, various tests that rely on timeouts (e.g. because they expect operation A to time out and operation B to succeed) would sporadically fail due to the heavy contention caused by the many threads and processes the jobs would run.

Using the current setup, a single pipeline for Inko takes between 15 and 20 minutes. This involves running various linters, testing across three platforms and two architectures, building and upload documentation, and building container images for two architectures.

While this isn't too bad, with slightly more powerful hardware I'm certain the time could be cut in half. For ARM64 builds I can't do much as an M1 Mac Mini just isn't *that* powerful (or at least not powerful enough for this scenario). For AMD64 builds I could relax the constraints and speed up build times somewhat, but this comes at the cost of noise: the Noctua CPU fan of the Deskmini can get pretty loud at high load and this noise can be heard across the entire ground floor. As I *hate* fan noise ("WWWWWRRRRRRRRRRRRRRRRRR"), this is not an option.

I've been experimenting with using VPS' to take over the load, but the results so far are mixed. For example, I tested a Netcup 500 ARM G12.5 VPS (2 virtual cores and 4 GiB of RAM) but found it to be about 2 times slower compared to the FreeBSD VM running on the Mac Mini. I ran a similar test on Hetzner and found their servers to be about 2.5 times slower. Based on this and some other tests I ran, it appears that to achieve performance similar to my local VMs I need to take whatever CPU cores they have access to and double that, so a local VM with 2 cores would translate to a VM with *at least* 4 "cores".

Note the "cores" part because different providers take different approaches to access to the CPU: in some cases a "vCore" translates to a thread or even part of a thread (e.g. a thread with additional usage limits), while in other instances (e.g. Netcup's root server offering) it translates to a thread not concurrently shared with other VMs and without any utilisation limit.

The bottleneck here is money: most reputable providers charge somewhere between €15 and €20 for 4 "cores" per server, with some offering budget options (e.g. Netcup) that cost less but likely come with (significant) resource constraints (e.g. more CPU steal time or poor disk performance). Assuming I want to move all runners out of my home network, that means 6 virtual machines (`3 operating
systems * 2 architectures`), for a total of somewhere between €90 and €120 per month. This also assumes that I wouldn't need bigger servers, at which point you may end up paying double this estimate.

One experiment I'm thinking of is to set up a Netcup 1000 ARM G12.5 VPS (4 virtual cores) for FreeBSD ARM64 builds, and a Netcup Lite 2 G12.5s VPS (4 cores from their lite/budget tier) for FreeBSD AMD64 builds. This way I can free up 2 cores on both the Deskmini and the Mac Mini for other jobs, though I have to give this some more thought based on the somewhat lackluster performance of the 2-core ARM servers I tried so far.

## Building container images

Another challenge with the shared runners, and one worth discussing in more detail, is that the shared runners don't allow for nested Podman/Docker builds. This means you can't use the various existing Docker actions (e.g. [docker/build-push-action](https://github.com/docker/build-push-action/)) for building container images and instead you have to use [Buildah](https://buildah.io/).

While annoying at first, I think the end result is actually *better* than using existing actions as your CI pipeline ends up depending less on some pile of JavaScript and YAML that resides in some GitHub repository.

For building images I use Buildah, as it doesn't require the ability to run Podman/Docker inside itself. I found some existing articles that discuss this topic and they suggested to use the `vfs` storage driver, but after testing this for myself I found that this doesn't perform well once you start building container images larger than 1 GiB or so. Instead, I settled on the following:

Linux runners are configured such that jobs run in containers with at least the following option Podman:

```
--device /dev/fuse
```

This doesn't require privileged access and shouldn't pose any security risks (that I know of at least).

Next, we need to make sure that `/var/lib/containers/storage` in the container is a volume that resides on the host. If this is instead a regular directory, building the container may take considerably longer depending on the amount of layers and images produced when building an image.

There are two ways of doing this. The first option is to use a specific directory that exists on the host and configure the Forgejo runner to allow jobs to mount this directory. For example:

```yaml
container:
  options: "--device /dev/fuse"
  valid_volumes:
    - /var/lib/forgejo-runner/containers
```

This allows jobs to then mount this directory as follows:

```yaml
jobs:
  manifest:
    name: OCI manifest
    runs-on: home-linux-amd64
    container:
      volumes:
        - /var/lib/forgejo-runner/containers:/var/lib/containers/storage:z
    needs:
      - build
    steps:
      ...
```

While this works and allows jobs to reuse existing data, it comes with two downsides:

1. Concurrent jobs may mess with this data in ways that could cause build failures, such as when one job removes all data while another job makes use of it
2. The directory may grow in size quickly, depending on the images you're building and how often you build them

A second and better option is to instead use anonymous volumes like so:

```yaml
container:
  options: "--device /dev/fuse --volume /var/lib/containers/storage"
  valid_volumes: []
```

CI jobs then *don't* need any additional configuration, i.e. the following is all you need:

```yaml
jobs:
  manifest:
    name: OCI manifest
    runs-on: home-linux-amd64
    needs:
      - build
    steps:
      ...
```

The Forgejo runner runs Docker/Podman jobs using the `--rm` option, so when the job finishes the anonymous volume is removed automatically. The downside is that depending on the amount of data stored in this directory, tearing down the container may take a little longer.

If you have a lot of RAM available you can also use a tmpfs mount, but given the containers storage can accumulate a lot of data quickly I don't think this is a good idea.

With the above setup in place, Buildah is then invoked as follows (where `NAME` is the name to use for the built image):

```
buildah build --pull=newer --isolation=chroot --layers --tag NAME
```

The `--pull=newer` option ensures that any images used in `FROM` entries in the `Dockerfile`/`Containerfile` are updated if necessary. `--isolation=chroot` is used to avoid the need for running Podman inside Podman, and `--layers` ensure that layers are cached and reused where possible (which unlike when using Podman isn't the default behavior).

The result of this approach is that Buildah is able to use the "overlay" storage driver (the default), storing the data on the host such that it can be reused between builds. I also found that if you *don't* store the containers data outside of the container, cleaning up the container at the end can take a long time (in the order of several minutes).

A downside is that jobs can mess with this shared volume, such that you *don't* want to use this if your runners are meant to run untrusted workloads. This isn't relevant for my use case though and as such I'm fine with this caveat.

To keep the CI configuration simple, I don't include the above buildah commands directly in the various YAML files. Instead, I have a [just](https://just.systems/) file that defines a bunch of tasks for each project (e.g. those for releasing a new version), including tasks to build and push images. The general structure is as follows:

```just
# justfile
mod release 'just/release.just'
mod image 'just/image.just'
```

Then `just/image.just` is defined as follows (using [idoc](https://codeberg.org/inko-lang/idoc) as an example here):

```just
set unstable
set no-cd

# The user to log in as to the registry.
USER := "yorickpeterse"

# The host of the registry.
HOST := "codeberg.org"

# The namespace of the package.
NAMESPACE := "inko-lang"

# The name of the package.
NAME := "idoc"

# The Git commit we're building for.
GIT_SHA := `git rev-parse HEAD`

# Build the image
build tag="latest" inko_tag="latest":
    buildah build \
        --pull=newer \
        --isolation=chroot \
        --layers \
        --build-arg "SHA={{ GIT_SHA }}" \
        --build-arg "VERSION={{ tag }}" \
        --build-arg "INKO_TAG={{ inko_tag }}" \
        --tag \
        "{{ fqn(tag) }}"

# Log in to the container registry
login pass:
    @buildah login {{ HOST }} --username "{{ USER }}" --password "{{ pass }}"

# Push the image to the registry
push tag:
    buildah push "{{ fqn(tag) }}"

# Tag an image as the latest image.
latest tag:
    buildah tag "{{ fqn(tag) }}" "{{ fqn('latest') }}"

# Create and push a manifest of multi-arch tags
manifest tag:
    buildah manifest create \
        --all "{{ fqn('manifest') }}" \
        "{{ fqn(f'{{ tag }}-amd64') }}"
    buildah manifest push "{{ fqn('manifest') }}" "docker://{{ fqn(tag) }}"
    buildah manifest rm "{{ fqn('manifest') }}"

# Prune all images
clean:
    buildah prune --all --force

fqn(tag) := f"{{ HOST }}/{{ NAMESPACE }}/{{ NAME }}:{{ replace_regex(tag, 'v(\d+\.\d+\.\d+)', '$1') }}"
```

Then to build and push an image, you run the following:

```
just image build latest
just image push latest
```

The above setup also allows for multi-arch images. You can use this [reusable workflow](https://codeberg.org/inko-lang/idoc/src/commit/945361f5daa7fc9b6109d7bea8a10e90b6c855ec/.forgejo/workflows/image.yml) as an example, but it essentially comes down to the following:

1. For each architecture, run a job that builds an image with a tag in the format `NAME:TAG-amd64` (i.e. `idoc:latest-amd64`) and push it to the registry
2. Create a manifest named `NAME:TAG` that combines these architecture specific tags and push it to the registry

In theory the architecture specific tags aren't necessary as you can use digests instead. This requires that you somehow store these digests in a build artifact such that it can be reused by a "create and push manifest" job. I felt it was easier to use the above approach, hence I went with that.

## Hosting private projects

While Codeberg allows for private repositories, this is limited to specific scenarios such as a private repository that supports a free-software project (e.g. a private project used for reporting security vulnerabilities). I have a few repositories used to build bootc server images/updates, and to track some private data (e.g. tasks to do around the house), none of which fall within the scope of what Codeberg allows.

I ended up migrating these projects to a private Forgejo instance hosted on the same server that hosts this website. The setup is a bit creative: I use [shost](https://codeberg.org/yorickpeterse/shost) for hosting a few websites, and has some logic related to TLS certificates that I want to dog-food, and as such I don't want to run a reverse proxy in front of it. This means port 443 is unavailable. While I could run Forgejo on a different port (e.g. 4430), this makes it a bit more annoying to work with.

Fortunately, there's a workaround: Netcup gives you access to a /64 IPv6 block, and pretty much every network I use on a regular basis supports IPv6. The way I have things set up is that the server has three IP addresses assigned:

1. `152.53.115.141` and `2a0a:4cc0:80:47b9::1`, used for the various sites served by shost
2. `2a0a:4cc0:80:47b9::2` used for the Forgejo instance

Then "routing" and binding of the services is done as follows:

1. shost binds itself to address `::` and port 443 on the host, with `::` meaning it binds to *all* IPv4 and IPv6 addresses available. This way it can handle both IPv4 and IPv6 connections
2. Forgejo binds itself to port 443 in a Podman network bridge. Because this is its own network, it won't conflict with shost
3. The firewall is configured so that HTTP(s) and SSH traffic to `2a0a:4cc0:80:47b9::2` is redirected to the IPv6 address of the Forgejo instance running in the Podman bridge

The firewall in this case is [firewalld](https://firewalld.org/) and the routing configuration is as follows (here `fd9d:b34d:66b6:ff2d::2` is the IPv6 address the Forgejo instance uses for the bridge, not the IPv6 address used by the host):

```xml
<?xml version="1.0" encoding="utf-8"?>
<zone>
  <short>Public</short>
  <description>The default zone</description>

  <!-- ... other stuff here ... -->

  <rule family="ipv6">
    <destination address="2a0a:4cc0:80:47b9::2" />
    <forward-port port="443" protocol="tcp" to-port="443" to-addr="fd9d:b34d:66b6:ff2d::2" />
  </rule>

  <rule family="ipv6">
    <destination address="2a0a:4cc0:80:47b9::2" />
    <forward-port port="80" protocol="tcp" to-port="80" to-addr="fd9d:b34d:66b6:ff2d::2" />
  </rule>

  <rule family="ipv6">
    <destination address="2a0a:4cc0:80:47b9::2" />
    <forward-port port="22" protocol="tcp" to-port="22" to-addr="fd9d:b34d:66b6:ff2d::2" />
  </rule>

  <forward/>
</zone>
```

And the corresponding [quadlets](https://docs.podman.io/en/latest/markdown/podman-systemd.unit.5.html) for the Forgejo instance:

```
# forgejo.container
[Container]
Image=codeberg.org/forgejo/forgejo@sha256:5ef02fbcf22beef56b8c844952ffad58cfe177b6f98e46e6abb5df90b88dc8d3
Pull=missing
ContainerName=forgejo
...
Network=containers.network
IP=10.10.0.2
IP6=fd9d:b34d:66b6:ff2d::2
...

# containers.network
[Network]
Driver=bridge
InterfaceName=containers
IPv6=true
DNS=127.0.0.53
Subnet=10.10.0.0/16
Gateway=10.10.0.1
Subnet=fd9d:b34d:66b6:ff2d::/64
Gateway=fd9d:b34d:66b6:ff2d::1
NetworkDeleteOnStop=true
```

The result is that I don't need a reverse proxy, an extra server, or the use of a non-standard port for Forgejo. The caveat is that you won't be able to access the instance from IPv4-only networks.

The reason shost binds directly to the host instead of using dedicated Podman network is because of [this Podman issue](https://github.com/podman-container-tools/podman/discussions/29423).

One question that may arise is why not host *all* my projects on my own Forgejo instance and make it available to the public, instead of using Codeberg. This is indeed something I thought of, and there are three main reasons for this:

1. Hosting projects on Codeberg (or any centralized forge for that matter) makes it easier to interact with different projects as you can use the same account, similar to what made GitHub popular in the first place
2. Opening your Forgejo instance up to the public means also dealing with an onslaught of spam, abusive bot traffic, and so on, and Forgejo in its current form doesn't have good tools to deal with this from what I've gathered
3. Hosting repositories along with all their forks may end up consuming a lot of disk space. While Forgejo supports [an alternative workflow](https://forgejo.org/docs/latest/user/git-cli/agit-support/) that doesn't require forking, it's sufficiently clunky that I suspect 99% of people can't be bothered; myself included

Of course hosting on Codeberg doesn't fundamentally *solve* these problems, but rather it means they are not *my* problems.

## Codeberg reliability and performance

One reason one might want to move away from GitHub is it's poor reliability in recent years. Unfortunately, the Codeberg experience in this regard is a bit of a hit and miss. While Codeberg performs reasonably well, it's *a lot* slower compared to a self-hosted instance.

Of course that makes sense given that a typical self-hosted instance has a fraction of the data and traffic, but it's still annoying. For example, a `git
push` typically takes between 5 and 10 seconds to complete and various pages on Codeberg can easily take seconds to finish loading. There have also been several outages over the last few weeks, most of which (including at least one partial outage that lasted for around an hour) with exactly zero communication from Codeberg. In fact, in at least one instance the Mastodon instance used for status updates was itself unavailable during a Codeberg outage.

Now to be fair, running a large publicly accessible code forge is no easy task, so I have to give Codeberg some slack here. I'm also optimistic that it *will* improve because Codeberg's whole mission is to make Codeberg useful, rather than making a big fat profit at all costs.

## Storage quotas

Codeberg enforces somewhat strict storage quotas on user accounts and organisations. For my personal account the limit is "only" 200 GiB for assets (that includes container images), but I think that's because my account has been around for a while. For the Inko organisation the limit is 1.5 GiB, which isn't enough for the various container images we build and store. While I submitted a [request to increase the quota](https://codeberg.org/Codeberg-e.V./requests/issues/2652), this request has yet to receive a reply at the time of writing. So until this quota is increased, Inko is forced to continue using GitHub's container registry for some of its container images, which feels a little odd.

If you're in a similar scenario, my advice is to create the quota request as early as possible as it may take several weeks for it to be processed.

## Conclusion

I think Codeberg is a promising alternative, in spite of some paper cuts you'll run into when migrating to it. Having to set up your own CI runners is a bit tedious, but there may be [some solutions to that in the future](https://felinebuildservices.org/). I also keep toying with the idea of setting up a small business that provides hosted Forgejo runner, though it's difficult to determine at this stage what (if any) interest there would be in this. Given Codeberg's mission I also feel it can only get better from here on, but only time will tell.
