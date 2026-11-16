---
title: 'Protomaps: Self-hosting map tiles has gotten cheap and easy'
link: https://bruceroettgers.eu/blog/protomaps/
source: hnrss-org
published: 2026-11-15T19:26:22Z
updated: 2026-11-15T19:26:22Z
first_seen: 2026-11-16T00:19:08.776402186Z
authors:
- bcye
content: extracted
html: 2026-11-15-protomaps-self-hosting-map-tiles-has-gotten-cheap-and-easy.html
---

PMTiles is a tile format which works via HTTP Range requests and can be deployed as a single file to a storage bucket.

I came across Protomaps in research for my latest project, [Mapboard](https://mapboard.bruceroettgers.eu).[^1] For most non-commercial projects, commercial APIs become prohibitively expensive if you ever go viral (Mapbox, for instance, charges [$5 per 1k map loads](https://www.mapbox.com/pricing) once you’ve exhausted the free tier).

PMTiles, part of the [Protomaps](https://protomaps.com/) ecosystem, is a tile format that ships in a single file, accessible via Range requests, and is thus extremely portable and easy to deploy. For CDNs that support caching Range requests of large files, it requires no effort besides down- and uploading it to storage. For others, like Cloudflare, you can [utilize an edge function](https://docs.protomaps.com/deploy/cloudflare) to translate between the traditional x/y/z URL path and Range requests.

## Getting some Maps

If all you want is the world map, getting it is as simple as going to the [build page](https://maps.protomaps.com/builds/) and downloading the latest daily build.

However, more fun can be had with the [CLI](https://github.com/protomaps/go-pmtiles). For example, you can [extract data](https://docs.protomaps.com/guide/getting-started#_3-extract-any-area) for only a bounding box or specific zoom levels, to get a significantly smaller file.

## Deploying to Bunny.net

One great CDN for deploying this is [Bunny.net](https://bunny.net?ref=9o9z1a5ws8) *(affiliate link)*. They support HTTP Range requests, don’t collect personal data by default,[^2] are headquartered in the EU, and offer low-cost access to a global network of CDN and edge storage servers.

[Pricing](https://bunny.net/pricing/) varies based on region and replication factor of the edge storage. But generally you pay $0.01/GB ($0.005/GB on the smaller Volume network) for bandwidth to Europe and North America, and $0.01/GB/month (no replication) to $0.055/GB/month (maximum replication) for storage. A high degree of replication mostly matters for latency in case of a cache miss (as the data is further replicated when it’s cached at the CDN PoPs).

In practical terms, storing the 139GB world map with replication to most continents (5x) will run you about $5/month in storage fees. Loading a map of Europe on a laptop and zooming in on a city causes about 5MB of data to be transmitted, which would be $0.00005 — further navigations and reloads will be less expensive as more of the map gets cached locally. At $0.05/1k map loads that could be 100x cheaper than Mapbox. However, the actual bandwidth of course varies (less for people that bounce immediately, possibly tens of MB for heavy users). For Mapbox you only pay a flat fee per visit.

### Uploading the file

It’s a good idea to create a storage zone with S3 compatibility enabled, as uploading via the HTTP API timed out after 15 minutes for me.

Then, with [rclone](https://rclone.org/) installed, you can download and stream the file to the Bunny storage zone. Preferably in [tmux](https://en.wikipedia.org/wiki/Tmux) or from a temporary VPS, if your connection isn’t great.

```bash
rclone config create bunnycdn s3 \
  provider=Other \
  access_key_id="<zone-name>" \
  secret_access_key="<storage-key>" \
  endpoint="https://<primary-region>-s3.storage.bunnycdn.com"

rclone copyurl "<build-url>" "bunnycdn:<zone-name>/<filename>.pmtiles" \
  --s3-chunk-size 256M \
  --progress
```

### Configuring the pull zone

Theoretically, at this point you just need to create a pull zone, select the storage zone as an origin, and you’re done. Practically, it would be sensible to change some settings.

Allowed CORS origins can be configured under Security > General > Allowed referrers, so that other websites cannot embed your tiles.

You’ll also want to verify under Caching that “Optimize for large object delivery” is enabled, as this makes it possible to cache Range requests.

You might also want to dissuade people from downloading the whole file and leaving you with a large CDN bill. The most practical way I’ve found for this is limiting the download speed to 10kB/s after 10MB transmitted in a single request (using the “Download speed limits” and “Limit after” settings under “Network limits”). Regular usage should never reach this amount. You may further want to introduce a requests/second limit (do note that legitimate usage can also quickly reach high numbers here) or a monthly bandwidth limit for the pull zone.

## Embedding the Map

With the tiles deployed, the only thing left to do is to embed them in your website via MapLibre GL, for example. To do this, you’ll need to install the `pmtiles` npm package and add the protocol and a style to MapLibre as detailed in the [official installation guide](https://docs.protomaps.com/pmtiles/maplibre).

Congratulations, you now host your own cost-effective and highly available map of the world!

notes (2)

## newsletter

Stay updated on new blog posts and projects by me; sent less than once per week.

## written by

Hi, I’m Bruce, a Computer Science student at ETH Zurich. I build mobile apps, websites and other products, and I write about the interesting things I run into along the way. [More about me →](https://bruceroettgers.eu/)

Please send an email if you wish to comment on this post. I will share select replies here with the author's permission.

[^1]: Basically, Excalidraw but on a map for travel planning. Launch post coming soon.

[^2]: The last octet of IP addresses is dropped. If you transmit personal data via the CDN, that is of course still processed.
