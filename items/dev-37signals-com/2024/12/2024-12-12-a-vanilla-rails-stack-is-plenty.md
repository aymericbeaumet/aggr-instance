---
title: A vanilla Rails stack is plenty
link: https://dev.37signals.com/a-vanilla-rails-stack-is-plenty/
source: dev-37signals-com
published: 2024-12-12T18:00:00Z
updated: 2024-12-12T18:00:00Z
first_seen: 2026-09-19T21:30:23.395188495Z
authors:
- Jorge Manrubia
summary: Minimal dependencies, maximum productivity. Staying vanilla pays long term dividends for your Rails apps.
content: extracted
html: 2024-12-12-a-vanilla-rails-stack-is-plenty.html
preview:
  file: 2024-12-12-a-vanilla-rails-stack-is-plenty.preview-b280983a801b.webp
  width: 256
  height: 134
  alt: 37signals Dev
  color: '#0f0f0f'
images:
- source: https://dev.37signals.com/assets/images/opengraph/a-vanilla-rails-stack-is-plenty.png
  original:
    file: 2024-12-12-a-vanilla-rails-stack-is-plenty.image-02043735f41f.png
    width: 2400
    height: 1260
  color: '#000000'
extra:
  thumbnail: https://dev.37signals.com/assets/images/opengraph/a-vanilla-rails-stack-is-plenty.png
---

Minimal dependencies, maximum productivity. Staying vanilla pays long term dividends for your Rails apps.

If you have the luxury of starting a new Rails app today, here’s our recommendation: go vanilla.

- Fight hard before adding Ruby dependencies. Keep that Gemfile that Rails generates as close to the original one as possible.
- Fight even harder before adding Javascript dependencies. You don’t need React or any other front-end frameworks, nor a JSON API to feed those.
- [Hotwire](https://hotwired.dev/) is a fantastic, pragmatic, and ridiculously productive technology for the front end. Use it.
- The same goes for mobile apps: use [Hotwire Native](https://native.hotwired.dev/). With a hybrid approach you can combine the very same web app you have built with a wonderful native experience right where you want it. The productivity compared to a purely native approach is night and day.
- Embrace and celebrate rendering things on the server. It has become cool again.
- ERB templates and view helpers will take you as long as you need, and they are a fantastic common ground for designers to collaborate hands-on with the code.
- [#nobuild](https://world.hey.com/dhh/you-can-t-get-faster-than-no-build-7a44131c) is the simplest way to go; don’t close this door with your choices. Instead of bundling Javascript, use [import maps](https://github.com/rails/importmap-rails). Don’t bundle CSS, just use modern standard CSS goodies and serve them all with [Propshaft](https://world.hey.com/dhh/introducing-propshaft-ee60f4f6). If you have 100 Javascript files and 100 stylesheets, serve 200 standalone requests multiplexed over HTTP2. You will be delighted.
- Don’t add Redis to the mix. Use [solid\_cache](https://github.com/rails/solid_cache) for caching, [solid\_queue](https://github.com/rails/solid_queue) for jobs, and [solid\_cable](https://github.com/rails/solid_cable) for Action Cable. They will all work on your beloved relational database and are battle-tested.
- Test your apps with Minitest. Use fixtures and build a realistic set of those as you cook your app.
- Make your app a [PWA](https://world.hey.com/dhh/native-mobile-apps-are-optional-for-b2b-startups-in-2024-4c870d3e), which is fully supported by Rails 8. This may be more than enough before caring about mobile apps at all.
- Deploy your app with [Kamal](https://kamal-deploy.org/).

If you want heuristics, your `importmap.rb` should import Turbo, Stimulus, your app controllers, and little else. Your `Gemfile` should be almost identical to the one that Rails generates. I know it sounds radical, but going vanilla is a radical stance in this convoluted world of endless choices.

This is the Rails 8 stack we have chosen for our new apps at [37signals](https://37signals.com/). We are a tiny crew, so we care a lot about productivity. And we sell products, not stacks, so we care a lot about delighting our users. This is our [Omakase](https://rubyonrails.org/doctrine#omakase) stack because it offers the optimal balance for achieving both.

Vanilla means your app stays nimble. Fewer dependencies mean fewer future headaches. You get a tight integration out of the box, so you can focus on building things. It also maximizes the odds of having smoother future upgrades. Vanilla requires determination, though, because new dependencies always look shiny and shinier. It’s always clear what you get when you add them, but never what you lose in the long term.

It is certainly up to you. [Rails is a wonderful big tent](https://rubyonrails.org/doctrine#big-tent). These are our opinions.

If it resonates, choose vanilla!

* * *

Guess [what our advice is for architecting your app internals](https://dev.37signals.com/vanilla-rails-is-plenty/)?

Sign up to get posts via email,\
 or [grab the RSS feed](https://dev.37signals.com/feed/posts.xml).
