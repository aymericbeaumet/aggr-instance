---
title: Announcing Hotwire Native 1.2
link: https://dev.37signals.com/announcing-hotwire-native-v1-2/
source: dev-37signals-com
published: 2025-04-23T17:00:00Z
updated: 2025-04-23T17:00:00Z
first_seen: 2026-09-19T21:30:23.395188495Z
authors:
- Jay Ohms
summary: A big update to Hotwire Native for iOS and Android.
content: extracted
html: 2025-04-23-announcing-hotwire-native-1-2.html
preview:
  file: 2025-04-23-announcing-hotwire-native-1-2.preview-752c70617d01.webp
  width: 256
  height: 134
  alt: 37signals Dev
  color: '#242428'
images:
- source: https://dev.37signals.com/assets/images/opengraph/announcing-hotwire-native-v1-2.png
  original:
    file: 2025-04-23-announcing-hotwire-native-1-2.image-6df884576568.png
    width: 2400
    height: 1260
  color: '#07080b'
extra:
  thumbnail: https://dev.37signals.com/assets/images/opengraph/announcing-hotwire-native-v1-2.png
---

We’ve just launched Hotwire Native `v1.2` and it’s the biggest update [since the initial launch](https://dev.37signals.com/announcing-hotwire-native/) last year. The update has several key improvements, bug fixes, and more API consistency between platforms. And we’ve created all new iOS and Android demo apps to show it off!

* * *

## Improvements

There are a few significant changes in `v1.2` that are worth specifically highlighting.

### Route decision handlers

Hotwire Native apps route internal urls to screens in your app, and route external urls to the device’s browser. Historically, though, it wasn’t straightforward to customize the default behavior for unique app needs.

In `v1.2`, we’ve introduced the `RouteDecisionHandler` concept to iOS (formerly only on Android). Route decisions handlers offer a flexible way to decide how to route urls in your app. Out-of-the-box, Hotwire Native registers these route decision handlers to control how urls are routed:

- `AppNavigationRouteDecisionHandler`: Routes all internal urls on your app’s domain through your app.
- `SafariViewControllerRouteDecisionHandler`: (iOS Only) Routes all external `http`/`https` urls to a [SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller) in your app.
- `BrowserTabRouteDecisionHandler`: (Android Only) Routes all external `http`/`https` urls to a [Custom Tab](https://developer.chrome.com/docs/android/custom-tabs) in your app.
- `SystemNavigationRouteDecisionHandler`: Routes all remaining external urls (such as `sms:` or `mailto:`) through device’s system navigation.

If you’d like to customize this behavior you can register your own `RouteDecisionHandler` implementations in your app. [See the documentation](https://native.hotwired.dev/reference/navigation#route-decision-handlers) for details.

### Server-driven historical location urls

If you’re using Ruby on Rails, the [turbo-rails](https://github.com/hotwired/turbo-rails) gem provides the following historical location routes. You can use these to manipulate the navigation stack in Hotwire Native apps.

- `recede_or_redirect_to(url, **options)` — Pops the visible screen off of the navigation stack.
- `refresh_or_redirect_to(url, **options)` — Refreshes the visible screen on the navigation stack.
- `resume_or_redirect_to(url, **options)` — Resumes the visible screen on the navigation stack with no further action.

In `v1.2` there is now built-in support to handle these “command” urls with no additional path configuration setup necessary. We’ve also made improvements so they handle dismissing `modal` screens automatically. [See the documentation](https://native.hotwired.dev/reference/navigation#server-driven-routing-in-rails) for details.

### Bottom tabs

When starting with Hotwire Native, one of the most common questions developers ask is how to support native bottom tab navigation in their apps. We finally have an official answer! We’ve introduced a `HotwireTabBarController` for iOS and a `HotwireBottomNavigationController` for Android. And we’ve updated the demo apps for both platforms to show you exactly how to set them up.

* * *

## New demo apps

To better show off all the features in Hotwire Native, we’ve created new demo apps for iOS and Android. And there’s a [brand new Rails web app](https://hotwire-native-demo.dev/) for the native apps to leverage.

Clone the GitHub repos to build and run the demo apps to try them out:

- [iOS repo](https://github.com/hotwired/hotwire-native-ios)
- [Android repo](https://github.com/hotwired/hotwire-native-android)
- [Rails app](https://github.com/hotwired/hotwire-native-demo)

Huge thanks to [Joe Masilotti](https://masilotti.com/) for all the demo app improvements. If you’re looking for more resources, Joe even wrote a [Hotwire Native for Rails Developers](https://pragprog.com/titles/jmnative/hotwire-native-for-rails-developers/) book!

* * *

## Release notes

`v1.2` contains dozens of other improvements and bug fixes across both platforms. See the full release notes to learn about all the additional changes:

- [iOS release notes](https://github.com/hotwired/hotwire-native-ios/releases/tag/1.2.0)
- [Android release notes](https://github.com/hotwired/hotwire-native-android/releases/tag/1.2.0)

* * *

## Take a look

If you’ve been curious about using Hotwire Native for your mobile apps, now is a great time to take a look. We have documentation and guides available on [native.hotwired.dev](https://native.hotwired.dev/) and we’ve created really great demo apps for iOS and Android to help you get started.

Sign up to get posts via email,\
 or [grab the RSS feed](https://dev.37signals.com/feed/posts.xml).
