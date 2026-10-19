---
title: Parsing JSON-structured Docker logs with the OpenTelemetry Collector
link: https://dev.37signals.com/parsing-json-docker-logs-with-opentelemetry-collector/
source: dev-37signals-com
published: 2026-10-19T17:00:00Z
updated: 2026-10-19T17:00:00Z
first_seen: 2026-10-19T19:14:52.283302801Z
authors:
- Lewis Buckley
summary: How we use the filelog receiver’s operator chain to parse, recombine, and route JSON-structured log lines from Docker containers.
content: extracted
html: 2026-10-19-parsing-json-structured-docker-logs-with-the-opentelemetry.html
remote_preview:
  url: https://dev.37signals.com/assets/images/opengraph/parsing-json-docker-logs-with-opentelemetry-collector.png
  alt: 37signals Dev
extra:
  thumbnail: https://dev.37signals.com/assets/images/opengraph/parsing-json-docker-logs-with-opentelemetry-collector.png
---

How we use the filelog receiver’s operator chain to parse, recombine, and route JSON-structured log lines from Docker containers.

We deploy our Rails apps with [Kamal](https://dev.37signals.com/kamal-2/), which runs them as Docker containers. Alongside the [Prometheus metrics collection](https://dev.37signals.com/kamal-prometheus/) we set up earlier, we run the [OpenTelemetry Collector](https://opentelemetry.io/docs/collector/) on each host to ship logs to [Loki](https://grafana.com/oss/loki/). Our Rails apps emit one JSON object per request to stdout: controller, status code, duration and the full Rails log output, all in a single line. Docker’s `json-file` logging driver then wraps each of those lines in *another* layer of JSON. Here’s how we use the collector’s `filelog` receiver to unwrap and parse them.

* * *

## No built-in Docker log receiver

The OpenTelemetry Collector doesn’t have a dedicated Docker log receiver. There’s a [dockerstats receiver](https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/main/receiver/dockerstatsreceiver) for container metrics, but nothing equivalent for logs. The `filelog` receiver’s `container` operator *can* parse Docker’s `json-file` format, but it only unwraps the outer JSON envelope. It recombines split lines for the CRI format used by containerd and CRI-O, but not for Docker’s format, and it doesn’t handle non-JSON content mixed into your log stream. Most Rails apps write some plain text to stdout too.

So collecting Docker container logs with the OTel Collector means putting together a chain of operators yourself. It’s not hard once you know the pieces, but it’s not obvious either.

* * *

## The problem: JSON inside JSON

Docker’s `json-file` driver wraps every line your app emits in a JSON envelope:

```json
{"log":"{\"@timestamp\":\"2026-03-14T12:00:00Z\",\"rails\":{\"controller\":\"PostsController\"},\"http\":{\"response\":{\"status_code\":200}},\"original\":\"Started GET /posts...\"}\n","stream":"stdout","time":"2026-03-14T12:00:00.123Z"}
```

The `log` field contains your app’s JSON as an escaped string. And if your app’s line is longer than 16KB, Docker splits it across multiple records, each one a separate JSON object with part of the `log` value. Only the last part ends with a newline. That newline is the only way to tell a partial record from a complete one.

* * *

## The operator chain

The filelog receiver lets you define a chain of operators that process log entries in sequence. Here’s our setup:

```yaml
receivers:
  filelog:
    include: [/var/lib/docker/containers/*/*-json.log]
    start_at: end
    operators:
      - type: container
        add_metadata_from_filepath: false

      - type: filter
        expr: 'body matches "^[^{]" and body matches "[^}]\\n$"'

      - type: recombine
        combine_field: body
        source_identifier: attributes["log.file.name"]
        combine_with: ""
        is_last_entry: body matches "\n$"
        max_unmatched_batch_size: 0

      - type: json_parser
        if: body matches "^\\{"
        parse_from: body
        parse_to: attributes
        on_error: send
```

Let’s walk through each step.

### 1\. `container`: unwrap the Docker envelope

The `container` operator parses Docker’s `json-file` format (and CRI format too). It extracts the `log` field as the body, `stream` as an attribute, and `time` as the timestamp. After this step, the body is your app’s raw log line, or part of it, but still a string, not yet parsed.

`add_metadata_from_filepath: false` turns off the Kubernetes metadata the operator reads from a pod’s log file path. Docker’s log paths don’t contain that metadata, and with the option on, the operator logs an error for every record unless the receiver also adds the file path (`include_file_path: true`).

### 2\. `filter`: drop non-JSON lines

Not everything your app writes to stdout is JSON. Puma access logs, Ruby warnings, and middleware timing lines (like `Rack handled in 18.27ms. 0 GCs.`) are plain text. We don’t want these in Loki, and some of our pipelines pick up lines by container name alone, so we drop them here.

We need to be careful because of how Docker splits long lines. The first chunk of a split line starts with `{` but the middle chunks won’t, and only the final chunk ends with a newline. We can’t just drop everything that doesn’t look like JSON, or we’d lose pieces of split log lines.

The filter drops lines that start with a non-`{` character **and** don’t end with `}\n`:

```plaintext
body matches "^[^{]" and body matches "[^}]\\n$"
```

A complete JSON line starts with `{`, so the first condition is false and it passes through. A middle chunk of a split JSON line has no newline at the end, so the second condition is false and it also passes through. Only standalone plain-text lines match both conditions and get dropped.

### 3\. `recombine`: reassemble split JSON

Docker’s `json-file` driver splits log lines at 16KB boundaries. A single JSON log from your app might become two or three Docker log entries. The `container` operator handles this for CRI format but not for Docker’s `json-file` format, so we do it ourselves.

The recombine operator concatenates consecutive entries until it sees one whose body ends with a newline, which is how Docker marks the end of a line:

```yaml
- type: recombine
  combine_field: body
  source_identifier: attributes["log.file.name"]
  combine_with: ""
  is_last_entry: body matches "\n$"
  max_unmatched_batch_size: 0
```

`source_identifier` ensures we only recombine entries from the same log file, so logs from different containers don’t get mixed up. That’s important when Kamal runs multiple containers on the same host during deploys. `combine_with: ""` concatenates without adding any separator, since Docker’s splits are at arbitrary byte boundaries.

`max_unmatched_batch_size: 0` turns off a default limit of 100 entries. Without it, recombine sends on what it has after 100 partial records, so any line over 1.6MB gets cut in two.

### 4\. `json_parser`: parse the app JSON

Now the body contains the complete, reassembled JSON string from your app. The `json_parser` operator parses it and writes the resulting fields into the log record’s attributes:

```yaml
- type: json_parser
  if: body matches "^\\{"
  parse_from: body
  parse_to: attributes
  on_error: send
```

After this, `attributes["rails"]["controller"]` is `"PostsController"`, `attributes["http"]["response"]["status_code"]` is `200`, and so on. The log record is now fully structured and ready for routing, transformation, and export.

The `if` skips anything that doesn’t start with `{`, so the parser doesn’t log an error for every piece of plain text that reaches it. `on_error: send` passes a record that fails to parse on with its raw body, and the collector logs the error. We count those errors in the collector’s own logs, and that’s how we found the two problems below.

* * *

## Two things we got wrong

Our first version of this chain ended a line on `}\n` rather than `\n`, and left `max_unmatched_batch_size` at its default. Both lost log lines.

When a line is an exact multiple of 16KB before its newline, Docker’s split falls between the closing `}` and the `\n`. The last record is then just `"\n"`. Neither record matched `}\n$`, so recombine kept going and joined the next log line on as well. That gave us two JSON objects in one body, `invalid character '{' after top-level value` from the parser, and two lost lines. It only happens to about 1 in 16,384 long lines, but on Basecamp that was around 320 times an hour.

The 100-record limit cut lines over 1.6MB in two. The first part failed to parse with `unexpected end of JSON input` and the rest was dropped. That was about 110 lines every 6 hours on Basecamp.

We now test the receiver config by running the collector against sample Docker log files, including a line split just before its newline and a line in more than 100 records, and checking which records come out.

* * *

## What comes next

With structured attributes on each record, you can use the [routing connector](https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/main/connector/routingconnector) to send different log formats to different pipelines, and the [transform processor](https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/main/processor/transformprocessor) to normalize fields, set severity levels, and enrich logs before exporting them.

The operator chain runs *inside* the receiver, before logs enter the pipeline. Getting the unwrap → filter → recombine → parse sequence right here means your processors only ever see clean, fully-parsed log records.

* * *

- [Other posts in the “On-prem observability” series](https://dev.37signals.com/series/on-prem-observability/)
