---
title: Kamal 2.0 released
link: https://dev.37signals.com/kamal-2/
source: dev-37signals-com
published: 2024-09-26T17:00:00Z
updated: 2024-09-26T17:00:00Z
first_seen: 2026-09-19T21:30:23.395188495Z
authors:
- Donal McBreen
summary: Making deployments simpler and faster.
content: extracted
html: 2024-09-26-kamal-2-0-released.html
preview:
  file: 2024-09-26-kamal-2-0-released.preview-adffe61f9430.webp
  width: 256
  height: 134
  alt: 37signals Dev
  color: '#070707'
images:
- source: https://dev.37signals.com/assets/images/opengraph/kamal-2.png
  original:
    file: 2024-09-26-kamal-2-0-released.image-023aad8cd9b7.png
    width: 2400
    height: 1260
  color: '#000000'
extra:
  thumbnail: https://dev.37signals.com/assets/images/opengraph/kamal-2.png
---

We’ve just released version 2 of Kamal, our deployment tool for running web apps directly on VMs or bare metal servers.

Kamal 1.0 was designed with 37signal’s use case in mind — deploying an application across multiple hosts, served with an external load balancer.

With Kamal 2.0 we’ve focused on making it simpler to use at any scale, whether you are deploying your app to 50 servers or deploying 5 apps to a single server.

* * *

## What’s new?

- [kamal-proxy](https://github.com/basecamp/kamal-proxy), a new proxy for gapless deployments
- [Automatic HTTPS with Let’s Encrypt](https://kamal-deploy.org/docs/configuration/proxy/#ssl)
- [Deploy many applications to one server](https://kamal-deploy.org/docs/configuration/proxy/#host)
- Create [aliases](https://kamal-deploy.org/docs/configuration/aliases/) for common kamal commands
- Simplified [secret management](https://kamal-deploy.org/docs/configuration/environment-variables/#secrets), with [commands](https://kamal-deploy.org/docs/commands/secrets/) for pulling secrets from password managers

* * *

## New proxy

Kamal uses a proxy to seamlessly switch between application versions.

Previously we used [Traefik](https://traefik.io/traefik/), but its declarative discovery model made it a poor match for Kamal’s imperative design.

Kamal 2 use a custom-built proxy, [kamal-proxy](https://github.com/basecamp/kamal-proxy). This simplifies deployments by providing a 1-1 mapping between kamal commands and proxy commands.

This has allowed us to build in new features like maintenance mode, pausing requests and canary deployments that will be coming to Kamal soon.

* * *

## Upgrading

Kamal 2.0 will be installed by default in Rails 8.0. It’s not just for Rails applications though — you can use it for web apps written in any language or framework.

If you already use Kamal 1, we’ve written an [upgrade guide](https://kamal-deploy.org/docs/upgrading/overview/).

We are already running HEY deployments on Kamal 2 and are benefiting from faster deployments and simpler configuration. And we will have all our apps running it soon.

Sign up to get posts via email,\
 or [grab the RSS feed](https://dev.37signals.com/feed/posts.xml).
