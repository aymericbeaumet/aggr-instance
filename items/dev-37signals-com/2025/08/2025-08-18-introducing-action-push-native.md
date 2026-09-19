---
title: Introducing Action Push Native
link: https://dev.37signals.com/introducing-action-push-native/
source: dev-37signals-com
published: 2025-08-18T17:00:00Z
updated: 2025-08-18T17:00:00Z
first_seen: 2026-09-19T21:30:23.395188495Z
authors:
- Jacopo Beschi
summary: A Rails gem for sending push notifications to mobile platforms.
content: extracted
html: 2025-08-18-introducing-action-push-native.html
preview:
  file: 2025-08-18-introducing-action-push-native.preview-e7bca0f8db4b.webp
  width: 256
  height: 134
  alt: 37signals Dev
  color: '#0b0b0b'
images:
- source: https://dev.37signals.com/assets/images/opengraph/introducing-action-push-native.png
  original:
    file: 2025-08-18-introducing-action-push-native.image-8d05c4fc9fd9.png
    width: 2400
    height: 1260
  color: '#000000'
extra:
  thumbnail: https://dev.37signals.com/assets/images/opengraph/introducing-action-push-native.png
---

**Note:** Shortly after releasing this gem, we renamed it from *Action Native Push* to *Action Push Native*, in case you arrived here looking for the gem of the former name. [More details here](https://github.com/basecamp/action_push_native/pull/28).

* * *

We’ve open-sourced [Action Push Native](https://github.com/basecamp/action_push_native), a Rails gem for sending push notifications to mobile platforms. It supports both Apple and Google push notification services.

* * *

## Why did we build it?

We created it to migrate off Amazon SNS and Pinpoint, as part of our broader [cloud exit](https://basecamp.com/cloud-exit). We’re using it in [Basecamp](https://basecamp.com/) and [HEY](https://www.hey.com/) to send more than 10 million push notifications per day without a hitch.

Action Push Native relies on HTTP/2 persistent connections to the Apple Push Notification service, which significantly reduced job duration compared to our previous HTTP/1 setup with AWS Pinpoint:

* * *

## How does it work?

The gem connects directly to the Apple (APNs) and Google (FCM) push notification services. It handles retries, rate-limiting, and deleting dead devices automatically. Configure each platform with your credentials, and you can start sending notifications like this:

```
device = ApplicationPushDevice.create! \
  name: "iPhone 16",
  token: "6c267f26b173cd9595ae2f6702b1ab560371a60e7c8a9e27419bd0fa4a42e58f",
  platform: "apple"

notification = ApplicationPushNotification.new \
  title: "Hello world!",
  body:  "Welcome to Action Push Native"

notification.deliver_later_to(device)
```

[Version 0.1.0](https://rubygems.org/gems/action_push_native) is available now. You can read more on [GitHub](https://github.com/basecamp/action_push_native#readme).

We hope you find it useful!

Sign up to get posts via email,\
 or [grab the RSS feed](https://dev.37signals.com/feed/posts.xml).
