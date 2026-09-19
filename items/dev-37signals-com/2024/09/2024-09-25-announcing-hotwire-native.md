---
title: Announcing Hotwire Native
link: https://dev.37signals.com/announcing-hotwire-native/
source: dev-37signals-com
published: 2024-09-25T17:00:00Z
updated: 2024-09-25T17:00:00Z
first_seen: 2026-09-19T21:30:23.395188495Z
authors:
- Jay Ohms
summary: The web-first framework for building native mobile apps.
content: extracted
html: 2024-09-25-announcing-hotwire-native.html
preview:
  file: 2024-09-25-announcing-hotwire-native.preview-175d15817d39.webp
  width: 256
  height: 134
  alt: 37signals Dev
  color: '#232327'
images:
- source: https://dev.37signals.com/assets/images/opengraph/announcing-hotwire-native.png
  original:
    file: 2024-09-25-announcing-hotwire-native.image-6b410af56fba.png
    width: 2400
    height: 1260
  color: '#07080b'
extra:
  thumbnail: https://dev.37signals.com/assets/images/opengraph/announcing-hotwire-native.png
---

As [Rails World 2024](https://rubyonrails.org/world/2024) is about to begin, we have an exciting [Hotwire](https://hotwired.dev) announcement! We’re launching a brand new, yet familiar, framework for building hybrid mobile apps.

* * *

## Background

But first, let me provide some context. Last year, shortly before [Rails World 2023](https://rubyonrails.org/world/2023), we open sourced [Strada](https://dev.37signals.com/announcing-strada/), which allows you to build high fidelity native features in your hybrid Turbo Native apps. The launch was met with a lot of excitement and development. However, there was a consistent piece of feedback that we heard: the relationship between the Hotwire umbrella of libraries, **Turbo** + **Stimulus** + **Strada**, was quite confusing.

Strada is meant to leverage Turbo Native apps, yet the documentation and marketing for Turbo Native apps has been nearly non-existent. Further, integrating Strada into your Turbo Native apps requires effort that shouldn’t be necessary.

* * *

## Hotwire Native

As we thought about it more, we saw an opportunity to make the relationship between the libraries much more clear. It made sense to consolidate the Turbo Native and Strada libraries under one roof, which would also allow us to more easily build new layers of out-of-the-box features for developers.

We’ve been working this year to make the idea a reality and the result is [Hotwire Native](https://native.hotwired.dev)! It’s the best way to build native apps for iOS and Android while leveraging your existing [Hotwire](https://hotwired.dev) web apps.

Hotwire Native is the consolidation of the Turbo Native and Strada libraries into a single framework for iOS and Android. Going forward, the Hotwire umbrella of libraries now includes: **Turbo** + **Stimulus** + **Native**.

A Hotwire Native app lets you take a unique web-first approach towards development with smaller teams:

- Leverage your **web screens** from your existing web app.
- Build **Bridge Components** (formerly Strada components) to add high fidelity features in your web screens.
- Build fully **native screens** for the most important areas of your app that require the highest fidelity.

Hotwire Native’s web-first approach means upgrading to native isn’t an all-or-nothing decision. You are free to choose specific screens or even specific components to write natively in Swift or Kotlin when you’re ready. It truly is progressive enhancement.

* * *

## Improvements

We didn’t simply consolidate the Turbo Native and Strada libraries and call it a day. We’ve made substantial improvements in key areas such as:

- Building a new [iOS](https://native.hotwired.dev/ios/getting-started) or [Android](https://native.hotwired.dev/android/getting-started) app requires just a few lines of code to get started.
- A whole new set of configuration options to make it easier than ever to customize your native app.
- A brand new [navigation layer](https://native.hotwired.dev/overview/basic-navigation) that handles all the [complex navigation](https://native.hotwired.dev/reference/navigation) stack situations that you may run into.
- More comprehensive [Path Configuration](https://native.hotwired.dev/overview/path-configuration) rules that are consistent across iOS and Android.
- [Bridge Components](https://native.hotwired.dev/overview/bridge-components) (formerly Strada components) work out-of-the-box without additional integration required.
- It’s easier than ever to replace web screens with [native screens](https://native.hotwired.dev/overview/native-screens) and route to them via URLs.

There’s plenty of other improvements throughout the [iOS](https://github.com/hotwired/hotwire-native-ios) and [Android](https://github.com/hotwired/hotwire-native-android) libraries. We have thorough documentation and examples that you can see at [native.hotwired.dev](https://native.hotwired.dev/).

* * *

## A big thank you

We couldn’t have done all this work without the incredible help from [Joe Masilotti](https://masilotti.com/). Joe has been an amazing advocate and educator for building Turbo Native apps for many years and he helped the 37signals team put together all the [great resources available](https://native.hotwired.dev/) for this launch.

And maybe most significantly, he made a big effort bringing his powerful [TurboNavigator library](https://github.com/joemasilotti/TurboNavigator) directly into the new [Hotwire Native iOS](https://github.com/hotwired/hotwire-native-ios) library. It’s the foundation of the new built-in navigation in the iOS library and it brings navigation parity to the [Hotwire Native Android](https://github.com/hotwired/hotwire-native-android) library.

Joe is rather famously known as the “Turbo Native guy” and now that Hotwire Native is public, he may want to consider a name change!

* * *

## Deprecations

In light of the new Hotwire Native libraries, in the near future we’ll be deprecating the existing **Turbo Native** and **Strada** libraries for iOS and Android. The code in those original libraries serves as the new foundation for Hotwire Native and we’ll be focused on Hotwire Native development going forward. You can start transitioning over to the new libraries whenever you’re ready and start taking advantage of all the new out-of-the-box capabilities.

* * *

## Check it out

Take a look at all the resources available on [native.hotwired.dev](https://native.hotwired.dev/) and we’re excited to see the new apps you build! We’re looking forward to your feedback and contributions!

Sign up to get posts via email,\
 or [grab the RSS feed](https://dev.37signals.com/feed/posts.xml).
