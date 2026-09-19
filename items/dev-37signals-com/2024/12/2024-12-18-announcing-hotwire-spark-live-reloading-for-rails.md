---
title: 'Announcing Hotwire Spark: live reloading for Rails applications'
link: https://dev.37signals.com/announcing-hotwire-spark-live-reloading-for-rails/
source: dev-37signals-com
published: 2024-12-18T18:00:00Z
updated: 2024-12-18T18:00:00Z
first_seen: 2026-09-19T21:30:23.395188495Z
authors:
- Jorge Manrubia
summary: Improve your feedback loop with smooth automatic page updates.
content: extracted
html: 2024-12-18-announcing-hotwire-spark-live-reloading-for-rails.html
preview:
  file: 2024-12-18-announcing-hotwire-spark-live-reloading-for-rails.preview-58666f84b6e7.webp
  width: 256
  height: 134
  alt: 37signals Dev
  color: '#121212'
images:
- source: https://dev.37signals.com/assets/images/opengraph/announcing-hotwire-spark-live-reloading-for-rails.png
  original:
    file: 2024-12-18-announcing-hotwire-spark-live-reloading-for-rails.image-2792b42425f5.png
    width: 2400
    height: 1260
  color: '#000000'
extra:
  thumbnail: https://dev.37signals.com/assets/images/opengraph/announcing-hotwire-spark-live-reloading-for-rails.png
---

Today, we are releasing [Hotwire Spark](https://github.com/hotwired/spark), a live-reloading system for Rails Applications.

Reloading the browser automatically on source changes is a problem that has been well-solved for a long time. Here, we wanted to put an accent on smoothness. If the reload operation is very noticeable, the feedback loop is similar to just reloading the page yourself. But if it’s smooth enough — if you only perceive the intended change — the feedback loop becomes terrific.

To use, just install the gem in development:

```
group :development do
  gem "hotwire-spark"
end
```

It will update the current page on three types of change: HTML content, CSS, and Stimulus controllers. How do we achieve that desired smoothness with each?

- For **HTML content**, it morphs the `<body>` of the page into the new `<body>`. Also, it disconnects and reconnects all the Stimulus controllers on the page.
- For **CSS**, it reloads the changed stylesheet.
- For **Stimulus controllers**, it fetches the changed controller, replaces its module in Stimulus, and reconnects all the controllers.

We designed Hotwire Spark to shine with the [nobuild approach we use and recommend](https://dev.37signals.com/a-vanilla-rails-stack-is-plenty/). Serving CSS and JS assets as standalone files is ideal when you want to fetch and update only what has changed. There is no need to use bundling or any tooling. Hot Module Replacement for Stimulus controllers without any frontend building tool is pretty cool!

2024 has been a very special year for Rails. We’re thrilled to share Hotwire Spark before the year wraps up.

Wishing you all a joyful holiday season and a fantastic start to 2025.

Sign up to get posts via email,\
 or [grab the RSS feed](https://dev.37signals.com/feed/posts.xml).
