---
title: 'datasette #3046: Don''t serve link: rel=next HTTP headers to Firefox for HTML pages'
link: https://simonwillison.net/2026/Nov/7/link-rel-next/
source: simonwillison-net
published: 2026-11-07T23:57:46Z
updated: 2026-11-07T23:57:46Z
first_seen: 2026-11-08T00:25:04.111827219Z
labels:
- datasette
- firefox
- html
- mozilla
summary: 'datasette #3046: Don''t serve link: rel=next HTTP headers to Firefox for HTML pages I was looking at the Firefox network panel today when I noticed that requests to a Datasette table page were triggering an HTTP prefetch to the next page of rows in that table. This was happening because Datasette pages serve an HTTP link header like this: link: <https://latest.datasette.io/fixtures/compound_three_primary_keys?_next=a%2Cd%2Cv>; rel="next" This is bad. A tiny fraction of the visitors to one of those pages will click the "next" link, which means their browser is fetching unnecessary data and the Datasette server is having to render an entire page that then gets thrown away. I dug in and learned that Firefox has an issue that has been open for 24 years about this behavior... And one of the early comments on that issue links to my blog post from 2002 complaining about this feature. I hadn''t just forgotten that I had written about it, I had forgotten the feature entirely! Tags: firefox, html, mozilla, datasette'
content: extracted
html: 2026-11-07-datasette-3046-don-t-serve-link-rel-next-http-headers-to.html
---

**[datasette #3046: Don't serve link: rel=next HTTP headers to Firefox for HTML pages](https://github.com/simonw/datasette/issues/3046)**. I was looking at the Firefox network panel today when I noticed that requests to a Datasette table page were triggering an [HTTP prefetch](https://developer.mozilla.org/en-US/docs/Glossary/Prefetch) to the next page of rows in that table.

This was happening because Datasette pages serve an HTTP `link` header like this:

`link: <https://latest.datasette.io/fixtures/compound_three_primary_keys?_next=a%2Cd%2Cv>; rel="next"`

This is bad. A tiny fraction of the visitors to one of those pages will click the "next" link, which means their browser is fetching unnecessary data and the Datasette server is having to render an entire page that then gets thrown away.

I dug in and learned that Firefox has [an issue](https://bugzilla.mozilla.org/show_bug.cgi?id=175418) that has been open for 24 years about this behavior...

And one of the early comments on that issue links to [my blog post from 2002](https://simonwillison.net/2002/Oct/17/mozillaPrefetching/) complaining about this feature. I hadn't just forgotten that I had written about it, I had forgotten the feature entirely!
