---
title: 'Introducing Upright: An Open Source Synthetic Monitoring System'
link: https://dev.37signals.com/introducing-upright/
source: dev-37signals-com
published: 2026-02-16T18:00:00Z
updated: 2026-02-16T18:00:00Z
first_seen: 2026-09-19T21:30:23.395188495Z
authors:
- Lewis Buckley
summary: We’re open-sourcing Upright, the synthetic monitoring system we built to watch over Basecamp, HEY, Fizzy and our many other services.
content: extracted
html: 2026-02-16-introducing-upright-an-open-source-synthetic-monitoring.html
preview:
  file: 2026-02-16-introducing-upright-an-open-source-synthetic-monitoring.preview-8aff0a998e41.webp
  width: 256
  height: 134
  alt: 37signals Dev
  color: '#1a1a1a'
images:
- source: https://dev.37signals.com/assets/images/opengraph/introducing-upright.png
  original:
    file: 2026-02-16-introducing-upright-an-open-source-synthetic-monitoring.image-24c7aef8de41.png
    width: 2400
    height: 1260
  color: '#010101'
extra:
  thumbnail: https://dev.37signals.com/assets/images/opengraph/introducing-upright.png
---

We’re open-sourcing [Upright](https://github.com/basecamp/upright), the synthetic monitoring system we built to watch over Basecamp, HEY, Fizzy and our many other services.

Upright runs health checks from multiple geographic locations and tells us when something breaks. It’s a Rails engine that you deploy to cheap VPS nodes around the world using [Kamal](https://dev.37signals.com/kamal-2/). Each node runs probes against your services and reports back via Prometheus metrics, so you can alert with AlertManager and visualize with Grafana.

* * *

## Why we built it

We’ve used Pingdom for years and it’s generally worked fine, but has been somewhat of a black box. We couldn’t customize checks, couldn’t run authenticated browser flows without handing over more cash, and couldn’t control exactly where checks ran from. Occasionally, probes would fail and trigger a page, but the issue would resolve itself before we could investigate.

We wanted something we owned, something we could extend, and something that fit into the open source observability stack we already use.

Upright started as an internal tool to complement our existing monitoring. It now handles all of our synthetic monitoring across five global sites.

* * *

## What it does

Upright supports four types of probes:

**Playwright probes** are the headline feature. These are real browser checks that can log into your app, navigate through flows, and verify that things work end-to-end. When a check fails, Upright captures a video recording and logs so you can see exactly what went wrong. You write them as Ruby classes using the [Playwright API](https://playwright-ruby-client.vercel.app/docs/api/page).

**HTTP probes** are simple health checks — hit a URL, check the status code. They run every 30 seconds from every site. Logs are in a familiar cURL format.

**SMTP probes** verify EHLO responsiveness, STARTTLS support, and SSL certificate validity.

**Traceroute probes** map the network path to your services with hop-by-hop latency tracking, thanks to MTR reports.

All probes run from every site you deploy to, optionally staggered by a few seconds. Upright uses this multi-site data to distinguish between real outages and regional blips. If one site in Amsterdam can’t reach your app but four others can, that’s a regional issue, not a full outage.

* * *

## How it’s built

Upright is a Rails engine. You create a new Rails app, add the gem, run the install generator, and you have a working monitoring system. It uses:

- **SQLite** for probe results storage
- [**Solid Queue**](https://dev.37signals.com/solid-queue-v1-0/) for background and recurring jobs
- **Kamal** for multi-server deployment
- [**Prometheus**](https://dev.37signals.com/prometheus-metrics-at-37signals/) for metrics and uptime queries
- **AlertManager** for notifications
- **OpenTelemetry** for tracing and logging
- **Dark and light mode** for the dashboard UI

The architecture is simple: deploy Upright to a few cheap VPS nodes (DigitalOcean, Hetzner, etc.), point DNS subdomains at each one, and configure your probes. Our setup runs across five sites — four DigitalOcean droplets (2 vCPU, 4GB RAM, ~$24/mo each) and one Hetzner server (3 vCPU, 4GB RAM) — for about $110/mo total. You could run a minimal two-site setup purely on Hetzner for under $20/mo.

Each node identifies itself via environment variables and runs probes independently. Metrics are scraped locally with an [OpenTelemetry collector](https://dev.37signals.com/kamal-prometheus/), and then fed back to the primary site which runs Prometheus and Alertmanager.

We actually send the metrics to three Prometheus instances, one in each of our datacenters as well as the primary Upright instance, providing full redundancy.

* * *

## Getting started

```
rails new my-upright --database=sqlite3 --skip-test
cd my-upright
bundle add upright
bin/rails generate upright:install
bin/rails db:migrate
bin/dev
```

The install generator creates everything you need: Prometheus and AlertManager configs, OpenTelemetry collector config, probe directories, Kamal deploy templates, and a Docker Compose file for local development services.

Define an HTTP probe in `probes/http_probes.yml`:

```
- name: "37signals.com"
  url: "https://37signals.com/"

- name: "Help Ticket Creation"
  url: "https://help.37signals.com/billing/tickets/new"
  expected_status: 301
```

Or write a Playwright probe that logs into your app and checks a real user flow:

```
class Probes::Playwright::Bc4ProjectProbe < Upright::Probes::Playwright::Base
  authenticate_with_form :bc4

  def probe_name = "Basecamp 4: view project"

  def check
    page.goto("https://3.basecamp.com/5228151")

    page.get_by_text("37s Monitoring HQ").visible?
  end
end
```

Check out the [README](https://github.com/basecamp/upright) for the full setup guide, including multi-site Kamal deployment, alerting rules, and authentication configuration.

* * *

## Try it out

Upright is available now on [RubyGems](https://rubygems.org/gems/upright) and [GitHub](https://github.com/basecamp/upright) under the MIT license.
