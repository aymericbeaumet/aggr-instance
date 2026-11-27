---
title: Garage v2.6.0 released with web admin, v1.x EoL & other news
link: https://garagehq.deuxfleurs.fr/blog/2026-11-webadmin-release/
source: lobste-rs
published: 2026-11-26T12:40:23Z
updated: 2026-11-26T12:40:23Z
first_seen: 2026-11-27T22:38:26.539383534Z
authors:
- garagehq.deuxfleurs.fr via armael
labels:
- distributed
- release
- web
summary: Comments
content: extracted
html: 2026-11-26-garage-v2-6-0-released-with-web-admin-v1-x-eol-other-news.html
remote_preview:
  url: https://garagehq.deuxfleurs.fr/images/og-default.png
  alt: Garage, an S3 object store so reliable you can run it outside datacenters
---

*We are glad to announce the release of Garage v2.6.0 that includes a brand-new web administration UI funded by NLnet. We also discuss many other small news: Helm chart repository release, Garage v1.x EoL, Garage v3.0.0 beta 1 and an outlook on upcoming work on S3 versioning.*

* * *

The Garage project has been quite active lately, so we thought it would be a good time to write a blog post to present new features and give some information on what's coming next.

## Garage v2.6.0 and the web administration GUI

This is the culmination of almost two years of work by [Tixie Salander](https://guerilla.studio/). We started this collaboration in 2024 when it was clear that a user interface was one of the most sorely lacking feature of Garage. We are very grateful that NLnet has again engaged with us and provided funding to make this a reality.

Here's a minimal example to test 2.6.0 right away:

```
cat > garage.toml <<'EOF'
metadata_dir = "/var/lib/garage/meta"
data_dir = "/var/lib/garage/data"
db_engine = "lmdb"
replication_factor = 1
rpc_bind_addr = "[::]:3901"
[s3_api]
s3_region = "garage"
api_bind_addr = "[::]:3900"
[admin]
api_bind_addr = "[::]:3903"
EOF

TOKEN=$(openssl rand -base64 32)
echo "Admin token: $TOKEN"

docker run --name garage -p 3900:3900 -p 127.0.0.1:3903:3903 \
  -e GARAGE_RPC_SECRET=$(openssl rand -hex 32) \
  -e GARAGE_ADMIN_TOKEN="$TOKEN" \
  -e GARAGE_DEFAULT_BUCKET=testbucket \
  -e GARAGE_DEFAULT_ACCESS_KEY=GKthisisatestkey \
  -e GARAGE_DEFAULT_SECRET_KEY=$(openssl rand -hex 32) \
  -v $PWD/garage.toml:/etc/garage.toml:z -v garage:/var/lib/garage \
  dxflrs/garage:v2.6.0 /garage server --single-node --default-bucket
```

Running 2.6.0 or above, you can access the web interface on the garage admin API port, by default port 3903, so http://localhost:3903/ if you are running garage on your machine with the example above.

Logging in requires a garage admin token. The example above prints it on the command line. If you have set the `admin_token` configuration variable in your configuration file, you may use that token. Otherwise, you'll need to create a separate token from the command line with `garage admin-token create`.

One world of caution: **administering a remote cluster over plain HTTP is NOT safe**, as your admin token is transmitted in plain text with each request. You'll need to setup an HTTPS reverse proxy in front of your Garage node to encrypt the connection, or/and access your node through a VPN such as Wireguard to make sure no one can eavesdrop the credentials.

Once logged in the cluster, you will be greeted by a cluster overview screen that looks like the following (this is a live example from our Deuxfleurs cluster):

![Garage web admin overview screen](https://garagehq.deuxfleurs.fr/blog/2026-11-webadmin-release/webadmin-overview.png)

The web interface follows a similar pattern to the CLI: cluster monitoring, layout configuration, bucket and access credential management are all available as you expect. Here are example videos (now included in the Quick Start guide as well) showing the setting up of a new cluster from scratch:

From a development standpoint, the web UI currently lives in a [separate Git repository](https://git.deuxfleurs.fr/Deuxfleurs/garage-webadmin/) so please report any issues there and not in Garage's main issue tracker. Tixie will continue being the maintainer of the UI moving forward.

## Official Helm chart published in our public repository

This has also been one of the longstanding requests from our Kubernetes users, and it's now done! Starting today you can deploy Garage on Kubernetes easily thanks to our self-hosted Helm repository. Both "regular" (tgz+index.yml) helm chart repository as well as OCI artefacts are available. Instructions in the documentation [have been updated accordingly](https://garagehq.deuxfleurs.fr/documentation/cookbook/kubernetes/). Happy Helming!

Thanks to Boris M. and Maximilien for contributing this improvement to Garage, along the many contributors that participated in finding the right way to make this work.

The Garage helm chart follows its own version scheme, with the current version being 0.10.7.

## Garage v1.x officially end-of-life

The last release of the 1.x branch was v1.3.1 in January 2026, almost a year ago. Many significant bug fixes, including security fixes, are currently only present on the v2.x branch. At this point, it would be too much work to backport them to 1.x and it didn't seem there was any ask to do so from the community either. So we are officially deprecating Garage v1.x and urging all users still on this branch to update to v2.x as soon as possible. The release note has been updated accordingly.

## Garage v3.0.0 beta 1 release

We have published a first beta release of Garage v3.0.0. This release **does not yet include S3 versioning** (sorry!). This unfortunately needed to be a major release because of a few breaking changes we wanted to release without waiting for s3 versioning to come though:

- anonymous S3 read access to buckets
- bucket and object tagging
- replacing the unmaintained node-to-node SSB handshake protocol with the Noise protocol, something we have been able to achieve without any user-facing breaking changes thanks to the close proximity between the two

We are looking for testers for this beta so please give it a try and report any findings on our issue tracker!

This beta release also marks the first step toward having a reliable Fjall metadata engine so please report to us not only problems, but also *cases of successfully running a Garage cluster on Fjall with significant activity*, so that we can gain confidence that it works well and potentially mark it stable soon. This also means we'll take bug report for Fjall issues, something that we explicitly disregarded so far for the 2.x experimental implementation.

As per [the release notes](https://git.deuxfleurs.fr/Deuxfleurs/garage/releases/tag/v3.0.0-beta1), the most important breaking changes for users to be aware of are:

- To finally align with industry standard, most Prometheus metrics have been slighly renamed, we provide [an updated Grafana dashboard](https://git.deuxfleurs.fr/Deuxfleurs/garage/src/commit/0ed12c39962dd0d8481f5d9152663631cc20f4bb/script/telemetry/grafana-garage-dashboard-prometheus.json)

- The Fjall major version update breaks compatibility with the Garage 2.x Fjall as data files from older versions are not supported. If you were using Fjall on v2.x, you will need to migrate manually: convert your metadata to another format (LMDB or SQlite) with `garage node convert-db` before upgrading. You can convert back to Fjall once you successfully upgraded to v3.

The complete list of breaking changes can be found in the changelog linked above. Happy testing!

## Coming up: S3 versioning

Finally we want to close this post with an update on S3 versionning.

In 2027 we will start a new collaboration with [Armaël](https://isomorphis.me/) to implement what is certainly **the** most requested feature in Garage: S3 object versioning.

We understand that there has been growing impatience for this feature to be implemented in Garage, and rightfully so as it would enable new use-cases such as for long-term archiving where compliance is important, or as a backup target that cannot be tampered with.

We thank the community for their patience, and kindly ask them to understand why this hasn't been done before: while there have been several attempts by community members, this is a deep problem that requires intricate understanding of how Garage stores metadata, and implementing it without taking this into account leads to code that, while it might appear to work on the surface, is in fact incorrect. A correct implementation requires a significant rewrite of core parts of Garage, which we believe can currently be accomplished only by experienced maintainers. We will also take this rewrite as an opportunity to fix some performance problems, such as those arising with extremely big objects.

I would like to make an important note on object locking and WORM: we will provide an implementation of locking primitives/WORM per the S3 API specification, however note that due to Garage's lack of a consensus algorithm, *this implementation will not be fully correct*. This means that Garage will implement best-effort detection of access requests that would violate locks, but will not be able to guarantee that it upholds locks *in all cases*. More precisely: using conditional writes (`If-Match`, `If-None-Match`) as a substitute for a distributed mutex or to discriminate between concurrent write requests will obviously not work, but ensuring that a successfully uploaded object cannot be overwritten after the fact should work in most cases. We will take care of properly documenting what works and what doesn't, but in practice, this means that locking and WORM will be gated behind a feature flag (a configuration parameter) that requires cluster administrators to acknowledge that they might be running unsafe workloads if they don't understand exactly what the feature does.

Thanks for reading, and for your continued support in our journey to a robust and resilient distributed storage system.

Alex, Maximilien and Tixie, The Garage (and Garage UI!) Maintainers
