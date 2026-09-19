---
title: Solid Queue 1.0 released
link: https://dev.37signals.com/solid-queue-v1-0/
source: dev-37signals-com
published: 2024-09-26T17:00:00Z
updated: 2024-09-26T17:00:00Z
first_seen: 2026-09-19T21:30:23.395188495Z
authors:
- Rosa Gutiérrez
summary: The first major release of Solid Queue is out, just in time for Rails World.
content: extracted
html: 2024-09-26-solid-queue-1-0-released.html
preview:
  file: 2024-09-26-solid-queue-1-0-released.preview-68fd81f3fb54.webp
  width: 256
  height: 134
  alt: 37signals Dev
  color: '#0b0b0b'
images:
- source: https://dev.37signals.com/assets/images/opengraph/solid-queue-v1-0.png
  original:
    file: 2024-09-26-solid-queue-1-0-released.image-a374d5e415a0.png
    width: 2400
    height: 1260
  color: '#000000'
extra:
  thumbnail: https://dev.37signals.com/assets/images/opengraph/solid-queue-v1-0.png
---

We’ve just released [Solid Queue v1.0.0](https://rubygems.org/gems/solid_queue/versions/1.0.0), right before speaking about it at Rails World. This version has come a long way since [we published the first version, 0.1.1, back in December 2023](https://dev.37signals.com/introducing-solid-queue/), with 132 merged pull requests and 126 closed issues, and the help of multiple contributors.

Apart from fixing many bugs and edge cases, we’ve enhanced Solid Queue with the following:

- Safe and atomic batch operations to discard, retry and unblock jobs, used from [Mission Control – Jobs](https://github.com/rails/mission_control-jobs).
- Enqueueing jobs in bulk (`enqueue_all` for Active Job’s `perform_all_later`).
- Recurring (cron-style) jobs.
- Proper logging and instrumentation.
- Lifecycle hooks for the supervisor and workers.
- A better installation, with a single schema file, separate DB configured by default and a binstub to easily start the supervisor.

More importantly, we’ve completely migrated [HEY](https://www.hey.com/), our email and calendar service, over from Resque. In fact, HEY Calendar was launched in January directly using Solid Queue for all jobs. Some issues we had after that launch is what drove batch operations support. We’ve also started moving jobs in our biggest app, [Basecamp 4](https://basecamp.com/).

* * *

## Our production setup

Currently in HEY we’re processing about 20 million jobs per day, using 800 workers, 4 dispatchers and 2 schedulers, spread over 74 VMs running in two datacenters, that we deploy using [Kamal](https://kamal-deploy.org/). Some of the queues are quite overprovisioned from our past Resque setup; we’ll reduce the number of workers, VMs and servers in the future, and reassign them to other work. These 74 VMs are sized differently depending on load and priority, with 2 to 6 cores (we have AMD EPYC 9454 48-Core processors) and 4G to 27G memory allocated for each of them. The physical servers where these live are running a mix of VMs for web, jobs and other apps, we don’t run dedicated machines only for HEY.

We’re running Solid Queue in a separate database from the main app. We also have the tables for Active Storage and Action Text in separate DBs. We run a primary and two replicas for each, and the four databases share the same physical servers. For the Solid Queue database, we’ve allocated 32 CPUs, 64G memory and 350G disk.

This is how our deploy configuration looks like, a bit simplified:

```
solid_queue_jobs:
  solid_queue_jobs:
    cmd: bin/jobs -c config/solid_queue/<%= ENV.fetch("JOB_POOL", "default") %>.yml --skip_recurring
  hosts:
    - hey-mail-jobs-101: mail_jobs
    - hey-mail-jobs-102: mail_jobs
    - hey-mail-jobs-103: mail_jobs
    - hey-mail-jobs-104: mail_jobs

# ...

    mail_jobs:
      JOB_POOL: mail
# ...
```

We use Mission Control – Jobs to manage the queues and [Prometheus and Yabeda](https://dev.37signals.com/kamal-prometheus/) for our monitoring needs. Here’s how some of the metrics we export for alerting look like:

```
module Yabeda
  module SolidQueue
    def self.install!
      Yabeda.configure do
        group :solid_queue

        gauge :jobs_failed_count, comment: "Number of failed jobs"
        gauge :jobs_scheduled_and_delayed_count, comment: "Number of scheduled jobs that have over 2 hours delay"

        collect do
          if ::SolidQueue.supervisor?
            solid_queue.jobs_failed_count.set({}, ::SolidQueue::FailedExecution.count)
            solid_queue.jobs_scheduled_and_delayed_count.set({}, ::SolidQueue::ScheduledExecution.where(scheduled_at: ..2.hours.ago).count)
          end
        end
      end
    end
  end
end
```

Then we start the metrics server like this:

```
SolidQueue.on_start do
  Yabeda::Prometheus::Exporter.start_metrics_server!
end
```

* * *

## PostgreSQL and SQLite: big thanks to the community

We use MySQL at 37signals so we don’t have as much expertise or visibility on issues involving the other two databases we support, PostgreSQL and SQLite. Thankfully, there are great experts in the community that are helping tremendously with that. Andrew Atkinson, the author of “High Performance PostgreSQL for Rails”, has published [a very informative article about using Solid Queue with PostgresSQL](https://andyatkinson.com/solid-queue-mission-control-rails-postgresql) that I recommend you to read if you are using Postgres. Hal Spitz contributed directly to Solid Queue [with a very helpful fix](https://github.com/rails/solid_queue/pull/231) for a PostgreSQL-only issue related to concurrency controls. Andy Croll, Marco Roth, Stephen Margheim, Nick Pezza and, especially, Mike Dalessio were of incredible help in [diagnosing and fixing a scenario](https://github.com/rails/solid_queue/issues/324) where the SQLite database used for Solid Queue could end up being corrupted. Stephen himself has published a number of articles on using SQLite effectively with Rails, [such as this one](https://fractaledmind.github.io/2024/04/15/sqlite-on-rails-the-how-and-why-of-optimal-performance/), and contributed valuable changes in that direction to the framework. I’m really grateful for all this.

* * *

## What’s next

After Rails World, we plan to focus again on Mission Control – Jobs for a bit, with the goal of releasing v1.0 soon. As for Solid Queue, since we’ll be working on migrating our biggest app, Basecamp 4, where we run about 4 times more jobs than in HEY, we’ll explore sharding support and other scalability improvements that I’m sure we’ll need as we progress with the work.

We hope you find this useful!

Sign up to get posts via email,\
 or [grab the RSS feed](https://dev.37signals.com/feed/posts.xml).
