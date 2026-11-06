---
title: 'Show HN: Firenook – Firebase emulators in Rust: 40× faster writes, 10× less RAM'
link: https://firenook.dev/
source: hnrss-org
published: 2026-11-05T13:07:29Z
updated: 2026-11-05T13:07:29Z
first_seen: 2026-11-06T11:27:16.888185679Z
authors:
- sanjevirau
summary: 'Hi HN, we have been developing with Firebase for a long time, and the only way Firebase developers can build apps without directly costing them is using the official Firebase Emulator Suite. However, the issue with this suite is, it''s dependent and running on Java with a old Console UI (not updated in years), that was not built to support tons and millions of data and has pretty bad developer experience. For a long time, we were hitting Out of Memory on even high specs laptop while developing and the worst part is while this happens all the persists data keep getting lost. We were honestly frustrated with this, and thought what if we built the Firebase Emulator Suite the right way, as it always should have been. Introducing Firenook, a faster drop-in replacement for Firebase Emulator Suite for Firebase developers. It''s 40x faster writes and needs 10x less RAM to run it. We took the right approach of first building it Rust as a single engine, so that there will be no Java dependencies and built from scratch a new Console UI using modern web stacks. The result is, now we were able to run 2 millions datas loaded to Firenook with only taking around 2GB ram compared to 22GB of the official suite. Things are way faster and rebuilt Console UI gives best developer experience from logging Firebase Functions, to editing Firestore documents in JSON and many more. We developed this throughly for around 2 months now and ran this internally for a month before we decided it''s time to release it to public so that other devs can make use of Firenook and also contribute it to make it better. Firenook is still new and I''m sure it misses things. So please try it our on your project and let us if there are any issues. Thank you. Comments URL: https://news.ycombinator.com/item?id=50378643 Points: 123 # Comments: 17'
content: extracted
html: 2026-11-05-show-hn-firenook-firebase-emulators-in-rust-40x-faster.html
remote_preview:
  url: https://firenook.dev/og/home.png
  alt: 'Firenook: A faster drop-in replacement for the Firebase Local Emulator Suite'
---

[0.2.1 What’s new](https://firenook.dev/changelog/0.2.1)

## A faster drop-in replacement for the Firebase Local Emulator Suite

Keep your `firebase.json`, SDKs and ports, and change one word in your script. No Java to install, and your data survives restarts.

40×

faster writes 5.1 ms against 208.2 ms, median

10×

less memory 2.11 GiB against 22.57 GiB at peak

0.59 s

to restart with your data against 30.8 s

```
  ╭──────────────────────────────────────────────────────────────────────────────────────────────────────╮
  │                                                                                                      │
  │       ▄█                                                                                             │
  │    ▄███▀ ▄     firenook  0.2.0                                                                       │
  │  ▄███▀ ███▄    demo-shop · picking up the data saved just now                                        │
  │  ▀██▀ ███▀     Node 24.20.0 · data in .firenook/state                                                │
  │      █▀▀                                                                                             │
  │                                                                                                      │
  ├─ Start-up ────────────────────────────────────────────────────────────────────────────────── 796 ms ─┤
  │                                                                                                      │
  │  Firestore   ✓ 255 ms  1,207,802 documents                                                           │
  │  Auth        ✓ 255 ms  64,000 users                                                                  │
  │  Storage     ✓ 255 ms  5,400 objects                                                                 │
  │  Functions   ✓ 423 ms  4 functions                                                                   │
  │  Pub/Sub     ✓   0 ms  1 schedule                                                                    │
  │                                                                                                      │
  ├─ ✓ All emulators ready ──────────────────────────────────────────────────────────────────────────────┤
  │                                                                                                      │
  │  Console       http://127.0.0.1:4000/console                                                         │
  │  Ports         Firestore 8080   Auth 9099   Storage 9199   Functions 5001   Pub/Sub 8085   Hub 4400  │
  │                                                                                                      │
  ╰──────────────────────────────────────────────────────────────────────────────────────────────────────╯

  ── Logs ────────────────────────────────────────────────────────────────────────────────────────────────

     21:05:40    ✓ checkout        117 ms  http · POST / · 201
     21:05:40    ✓ sendReceipt       6 ms  firestore · orders/qXoV1oOgJ0IILQs5psZP created
                 │ receipt queued   order qXoV1oOgJ0IILQs5psZP · total 36
     21:05:42    ✓ checkout          4 ms  http · POST / · 201
     21:05:42    ✓ sendReceipt       1 ms  firestore · orders/ZskDC6u3kTKSGalf4o5L created
                 │ receipt queued   order ZskDC6u3kTKSGalf4o5L · total 32
     21:05:43    ✕ stripeWebhook     1 ms  http · POST / · 500
                 │ Unhandled error Error: No signatures found matching the expected signature for payload
                 │   at functions/index.js:24:11
                 │ + 9 more lines · open in console


  ────────────────────────────────────────────────────────────────────────────────────────────────────────
     ● running 6s · 5 calls · 1 failed                              o console   r reload   ? keys   q quit
```

```
  ╭────────────────────────────────────────────────────────────╮
  │                                                            │
  │       ▄█                                                   │
  │    ▄███▀ ▄     firenook  0.2.0                             │
  │  ▄███▀ ███▄    demo-shop · picking up the data saved jus…  │
  │  ▀██▀ ███▀     Node 24.20.0 · data in .firenook/state      │
  │      █▀▀                                                   │
  │                                                            │
  ├─ Start-up ──────────────────────────────────────── 602 ms ─┤
  │                                                            │
  │  Firestore   ✓ 240 ms  1,207,804 documents                 │
  │  Auth        ✓ 240 ms  64,000 users                        │
  │  Storage     ✓ 240 ms  5,400 objects                       │
  │  Functions   ✓ 299 ms  4 functions                         │
  │  Pub/Sub     ✓   0 ms  1 schedule                          │
  │                                                            │
  ├─ ✓ All emulators ready ────────────────────────────────────┤
  │                                                            │
  │  Console       http://127.0.0.1:4000/console               │
  │  Ports         Firestore 8080   Auth 9099   Storage 9199   │
  │                Functions 5001   Pub/Sub 8085   Hub 4400    │
  │                                                            │
  ╰────────────────────────────────────────────────────────────╯

  ── Logs ──────────────────────────────────────────────────────

     21:05:49    ✓ checkout        115 ms  http · POST / · 201
     21:05:49    ✓ sendReceipt       6 ms  firestore · orders/E…
                 │ receipt queued
                 │ order EYi9LKG7xQ53eLrWktHm · total 36
     21:05:50    ✓ checkout          6 ms  http · POST / · 201
     21:05:50    ✓ sendReceipt       2 ms  firestore · orders/C…
                 │ receipt queued
                 │ order CU0LwsLgczq054BqsEuH · total 32
     21:05:51    ✕ stripeWebhook     1 ms  http · POST / · 500
                 │ Unhandled error Error: No signatures found
                 │ matching the expected signature for payload
                 │   at functions/index.js:24:11
                 │ + 9 more lines · open in console


  ──────────────────────────────────────────────────────────────
     ● running 6s · 5 calls · 1 failed
     o console   r reload   ? keys   q quit
```

```
$ npx firebase emulators:start
```

Firenook’s own screen, recorded from 0.2.0 on a demo shop: it starts with the 1.2 million documents, 64,000 users and 5,400 files it kept from its last run, takes two orders, and shows a webhook that fails, with its stack trace.

## Change one word

Same flags, same ports, same export format. Your project stays as it is, and firebase-tools stays for deploying.

package.json

```
-  "emulators": "firebase emulators:start --import ./data"+  "emulators": "firenook emulators:start --import ./data"
```

- ### firebase.json and .firebaserc

  Read the way the Firebase CLI reads them.

- ### Rules, indexes, Functions

  Your code and config stay as they are.

- ### Your SDKs

  connectFirestoreEmulator, FIRESTORE\_EMULATOR\_HOST and the rest connect unchanged.

- ### Your data

  Imports and exports the official format, both ways.

## Eight emulators, on the ports you already use

Your SDKs connect to them as they connect to Google’s: the same hosts, the same ports, the same environment variables.

### [Firestore](https://firenook.dev/firestore-emulator)

8080

Native and web SDKs, realtime listeners, Security Rules, named databases, and the official import and export.

orders 12 documents

- XXfs8ZLaWvmMOeaVAcw8
- 5XSNbW90qkxiTbqQilu1
- mqr7wc5Vjb6d5y75tfKy
- ijO11GokICQ0ki9Ta4p9
- c0yT9Jkt9NGpiX7HMV9r

createdAt

2026-10-23 13:06:17.872 UTC

items

\[{ price: 54, qty: 1, sku: "hoodie" }, { price: 28, qty: 1, sku: "tee" }\]

total

82

### [Authentication](https://firenook.dev/compatibility#auth)

9099

Every operation of the official Auth emulator: password, email link, phone, MFA, passkeys, tenants and blocking functions.

Users 12

- Amara Abara amara@example.com password
- Elif Demir elif@example.com password
- Ines Duarte ines@example.com password

### [Cloud Storage](https://firenook.dev/compatibility#storage)

9199

The Firebase and Cloud Storage APIs, with Security Rules evaluated natively, `firestore.get()` included.

demo-shop.appspot.com / products 8 files

- cap.svg image/svg+xml 404 B
- hoodie.svg image/svg+xml 408 B
- mug.svg image/svg+xml 404 B

### [Cloud Functions](https://firenook.dev/compatibility#functions)

5001

HTTP, callable and streaming functions, and every trigger: Firestore, Storage, Auth, Pub/Sub, Eventarc, schedules and task queues. Code reloads on save.

Function calls 25

| Time     | Function      | Trigger   | Summary                             | Status | Duration |
| -------- | ------------- | --------- | ----------------------------------- | ------ | -------- |
| 13:06:19 | stripeWebhook | http      | POST /                              | 500    | 1.1 ms   |
| 13:06:18 | sendReceipt   | firestore | orders/mzbnHY1n79leG5ioi2nD created | 200    | 1.8 ms   |
| 13:06:18 | checkout      | http      | POST /                              | 201    | 6.3 ms   |
| 13:06:18 | sendReceipt   | firestore | orders/c0yT9Jkt9NGpiX7HMV9r created | 200    | 1.7 ms   |
| 13:06:18 | checkout      | http      | POST /                              | 201    | 5.0 ms   |

### [Pub/Sub](https://firenook.dev/pubsub-emulator)

8085

Every RPC over gRPC and HTTP/JSON: push, ordering, filters, dead-letter topics, seek, snapshots and schemas.

nightlyReport, every day 02:00: next run 2026-10-24 02:00 UTC

### [Cloud Tasks](https://firenook.dev/compatibility#tasks)

9499

Queues for onTaskDispatched, with the official retry and rate-limit behaviour.

### [Eventarc](https://firenook.dev/compatibility#eventarc)

9299

Custom events delivered to onCustomEventPublished, and the Firebase services’ own.

### [Extensions](https://firenook.dev/compatibility#extensions)

5001

Extensions from local, vendored or registry sources, offline once vendored.

The orders, users, files and calls above are from a run of the demo project, as Firenook returned them. Realtime Database, Hosting, App Hosting and Data Connect are not emulated; a `firebase.json` that configures them still starts, with a warning. [What each service covers](https://firenook.dev/compatibility).

## Measured on a real application

Two hours of a production app’s traffic against each emulator in turn, every operation timed by the client.

### Memory at peak

10× less

Official

22.6 GiB

Firenook

2.11 GiB

### Write, median

40× faster

Official

208 ms

Firenook

5.1 ms

### Listener update, median

19× faster

Official

209 ms

Firenook

11 ms

### Restart with data kept

52× sooner

Official

30.8 s

Firenook

0.59 s

### orderBy, limit 20

8× faster

Official

96.3 ms

Firenook

12.0 ms

### CPU over two hours

61% less

Official

1,586 s

Firenook

616 s

The app holds 211,205 Firestore documents and 6.7 GB in Storage; both emulators ran on one server. Firenook answered 25 of 26 query shapes faster and tied on the last. [How it was measured](https://firenook.dev/benchmarks).

## Your data stays where you left it

Add `--keep-data` and Firestore, Auth and Storage come back with all your data in about half a second, even with millions of documents. No export at every stop, no import at every start.

A crash loses at most the last second of writes, where the official emulator loses everything since its last export. A stop always closes the data cleanly.

`npx firenook emulators:start --keep-data`

Official emulator

Stop: export, 50–57 s Start: import, 21.6–23.6 s

Firenook

Stop, 0.04–0.30 s Start, 0.54–0.57 s

Stopping with the data saved, then starting again with it, on one time scale. The dim blocks are the spread between the fastest and the slowest runs.

```
$ npx firenook emulators:exec "node --test checkout.test.mjs"
firenook: Starting emulators: auth, functions, firestore, pubsub, storagefirenook functions: Loaded functions definitions from source: checkout, sendReceipt, stripeWebhook, nightlyReport.All emulators ready✔ checkout takes an order (173.227708ms)ℹ pass 1ℹ fail 0firenook suite stopped cleanly
```

exit 0 **1.39 s** from start to stop, the test included

A run of the demo project’s test through firenook emulators:exec on firenook 0.2.0, with some lines left out.

## Run your tests against it

`emulators:exec` starts the suite, runs your command once everything is ready, with the emulator variables set, then stops and passes on the exit code.

CI gets plain lines, the same ones the official emulator prints, and there is no Java to set up first. Firenook needs only Node 24.

`npx firenook emulators:exec "npm test"`

## The console the Firebase emulators should have had

Google’s Emulator UI has no page for your Functions, Pub/Sub, Cloud Tasks or Eventarc, only a log. Firenook’s console covers every service it runs, live, on the Emulator UI’s port.

Every call, with what triggered it, its outcome, its duration and its logs.

![The console’s function calls: every call with its time, function, trigger, summary, outcome and duration, one of them a 500](https://firenook.dev/_astro/invocations-dark.CWGnqWbs_jxCnL.webp)

Light and dark, works offline, sends nothing anywhere. Bookmarks to the official UI’s pages open the matching console page. [Every page of the console](https://firenook.dev/console).

## Give your agent the emulator, not production

`firenook mcp` gives a coding agent 54 tools over the running suite. It reads the log and each function call with its error, asks the Security Rules why a request is denied, and works with your data. Nothing leaves your machine.

`npx firenook mcp add`

Sets it up for Claude Code, Cursor and VS Code in the project’s own files. Other agents run `npx firenook mcp`. [How agents use it](https://firenook.dev/mcp).

1. You

   The Stripe webhook answers 500 on my machine. Why?

2. **functions\_list\_calls** outcome: "failed"

   failed

   500 stripeWebhook http POST /, 1.15 ms at 13:06:19

3. **functions\_get\_call** id: "e72ca3-25"

   error

   Error: No signatures found matching the expected signature for payload

   at

   functions/index.js:24:11

   headers

   host, connection, content-type, accept, accept-language, sec-fetch-mode, user-agent, accept-encoding, content-length

   body

   {"type":"checkout.session.completed"}

4. **rules\_evaluate** service: "storage", method: "create", path: "products/mug.svg"

   verdict

   deny as unauthenticated

   line 9

   allow read, write: if false;

The question is an example. The tool calls and their answers are from a run of the demo project on firenook 0.2.0, as Firenook gave them.

Firestore

Read, query, count, write and delete documents

Authentication

Manage users, read sign-in codes, mint ID tokens, set claims

Storage

List, read, upload, copy, move and delete files

Functions and logs

Invoke a function, read each call and the log, replay a call after a fix

Security Rules

Ask why a request is denied, test a draft, list denials

Pub/Sub, Tasks, Eventarc

Publish, peek, run tasks and schedules now, replay events

Do I still need firebase-tools?

Yes, for deploying and everything else the Firebase CLI does. Firenook replaces only the emulators: install it next to firebase-tools and run your emulators with it.

Does Firenook need Java?

No. Its services are native code in one npm package, with an engine for macOS, Linux and Windows, and nothing else is downloaded at install or start. Node 22, 24 or 26 runs your Functions, as it does in production.

Can I go back to the official emulator?

Yes. Firenook imports and exports the official format. While Firenook runs, export with npx firenook emulators:export ./export, then start the official emulator with --import ./export.

Which services does it emulate?

Firestore, Authentication, Cloud Storage, Cloud Functions, Extensions, Pub/Sub, Cloud Tasks and Eventarc. Realtime Database, Hosting, App Hosting and Data Connect are not emulated; a firebase.json that configures them still starts, with a warning.

Is it ready for my team?

0.2.0 is the first release for public use. Every release passes conformance replays against the official emulator and packed installs on all five platforms, and Firenook passed a two-hour benchmark against the official emulator on a real application. Commit your lockfile and upgrade on purpose.

Is it free?

Yes. Firenook is open source under the Apache License 2.0.

Is Firenook made by Google?

No. Firenook is an independent open-source project, not affiliated with or endorsed by Google. Firebase is a trademark of Google LLC.

## Switch in three steps

One package, nothing else to install: Node 22, 24 or 26 on macOS, Linux with glibc 2.28 or newer, or Windows x64. Starting a new project instead? `npx firenook init` sets one up.

1. ### Install it next to firebase-tools

   Keep firebase-tools for deploying: Firenook has no deploy or login command.

   `npm install --save-dev firenook`

2. ### Check your project

   This reads firebase.json the way the Firebase CLI does, and changes nothing.

   `npx firenook init --adopt --dry-run`

3. ### Swap the command

   Same flags and ports, so your app connects as before and your data imports as it is.

   `npx firenook emulators:start`
