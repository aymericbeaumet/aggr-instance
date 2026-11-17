---
title: Running the full server-side Datasette Python app in the browser with service workers and Pyodide
link: https://simonwillison.net/2026/Nov/17/datasette-lite/
source: simonwillison-net
published: 2026-11-17T16:48:33Z
updated: 2026-11-17T16:48:33Z
first_seen: 2026-11-17T18:26:16.162530996Z
labels:
- datasette
- datasette-lite
- javascript
- projects
- pyodide
- python
- service-workers
- urls
- webassembly
summary: 'Datasette Lite is my port of the Datasette Python application to run directly in the browser using WebAssembly and Pyodide. I''ve released a complete rewrite of Datasette Lite using service workers, and it''s now almost indistinguishable from the real server-side application. You can now do all of the following, using a carefully crafted URL to lite.datasette.io: Load client-side Datasette to browse any SQLite database, JSON file, CSV file or SQL script Select a released version of Datasette to load from PyPI Load a specific commit, tag, or branch name from the Datasette GitHub repository Pick from over 100 versions of SQLite to use with Datasette Install additional Datasette plugins from PyPI or URLs to wheel files, including JavaScript visualization tools The ability to load specific Datasette references, plugins, and SQLite versions makes this a fantastic tool for linking to demos of work-in-progress features or bug reproductions, without needing to spin up a full web server to serve that demo. As an example, consider the following URL: https://lite.datasette.io/?install=datasette-maplibre&url=https://gist.githubusercontent.com/simonw/39fa5d399465766039549912e2b3d291/raw/f6dfba99a75cbb8b5d9386223220eb52272767ee/demo.db&metadata=https://gist.githubusercontent.com/simonw/39fa5d399465766039549912e2b3d291/raw/f6dfba99a75cbb8b5d9386223220eb52272767ee/metadata.yml#/demo/capitals_and_countries%23map=0/4.2/180&color=continent Follow that link and, after a short loading screen, you''ll find yourself on a page that looks like this: That''s Datasette running the new datasette-maplibre plugin, loading up a database of GeoJSON shapes of countries and displaying both a combined map and individual boundary maps in a table. Let''s break down that URL to see what it''s doing: https://lite.datasette.io/ ?install=datasette-maplibre &url=https://.../demo.db &metadata=https://.../metadata.yml #/demo/capitals_and_countries%23map=0/4.2/180&color=continent The ?install=datasette-maplibre installs that additional plugin from PyPI. You can pass &install=... as many times as you like to load more plugins. &url=https://.../demo.db loads that SQLite database from the provided URL. These URLs need to have open CORS headers - I host files like this in Gists, because they provide an easy way to get open CORS headers on a web-hosted file. &metadata=https://.../metadata.yml loads this metadata.yml file. The metadata file adds HTML descriptions to various tables, and to the Datasette homepage. Visit that homepage here for a guided tour of the other features of that plugin, all defined in the metadata file. #/demo/capitals_and_countries%23map=0/4.2/180&color=continent is a fragment hash that turns into a location within Datasette - here it''s loading the /demo/capitals_and_countries SQL view, then appending another fragment hash to it (%23 is a URL-encoded #) which tells the map plugin to zoom to a certain location and to set the color of the shapes on the map based on the continent column in the data. That URL is doing a lot of work! I wrote more about this trick for building URL-addressable software in URL-addressable Pyodide Python environments last year. The full set of supported URL arguments is described in the Datasette Lite README. I''ve already started using Datasette Lite as part of the Datasette development process. Here''s a recent issue describing a pagination bug - I posted a comment there demonstrating the old, broken behavior and the new fix using links to Datasette Lite: Demo of the bug: https://lite.datasette.io/?ref=1.0a42&sql=https://gist.github.com/simonw/75ececf61d50e6e8d50f0c150150ee81#/data/scores?_size=2&_sort=score - click next and the second page is empty. Here''s that same page with the bug fixed (running on 1.0a43): https://lite.datasette.io/?ref=1.0a43&sql=https://gist.github.com/simonw/75ececf61d50e6e8d50f0c150150ee81#/data/scores?_size=2&_sort=score How this works, using service workers Datasette Lite is not a new project. I launched the first version back in 2022, originally as a bit of a joke (running server-side apps in the browser is pretty funny) but one that quickly started proving its value. The problem with that initial version is that it was a hack. It worked by running Datasette in a Pyodide web worker, simulating requests through that stack, then injecting the resulting HTML back into the top level page using innerHTML. This worked for basic Datasette, but meant that any feature that used JavaScript failed to work, because script tags in innerHTML don''t execute, and the web worker tricks it used failed to load additional JavaScript or CSS files. The new version takes a completely different approach based on Service Workers. These allow JavaScript to intercept HTTP requests for new pages and resources and serve them from client-side code instead. Datasette Lite registers a service worker for the entire lite.datasette.io domain. The parent page then creates an iframe that sends requests for Datasette pages to URLs that look like this: /app/aa94e6851595/ # the homepage (HTML) /app/aa94e6851595/data/places # a table page (HTML) /app/aa94e6851595/-/static/navigation-search.js # (JavaScript) /app/aa94e6851595/-/static/app.css # (CSS) Those are regular Datasette URLs, but with a /app/aa94e6851595/ prefix. What''s that about? The /app/<session>/ prefix is necessary because of multiple tabs. Consider the URL above, which loads the demo instance with the datasette-maplibre plugin. That URL defines a full, custom Datasette instance. You might have that one running in a tab, then one of my other demo links running in another. The service worker for lite.datasette.io is shared between all of those tabs. If it routed to a single instance of Datasette then every tab would share that underlying instance, no matter what plugins or other settings their URL attempted to configure. Instead, the parent page generates a random session ID and uses it as a prefix for all of the subsequent hidden asset and page loads. The service worker can then use that session ID to route those requests back through the parent page to a web worker running a dedicated Datasette instance for that particular session. (We still need a web worker because Pyodide in a service worker would not be able to maintain state beyond around 30 seconds, depending on the browser. There are a lot of moving parts here!) I had Claude Fable 5.2 generate several SVG diagrams to help explain this. Here''s my favorite of those, now embedded in the Datasette Lite README: This system started life back in May as an AI research spike generated using Claude Opus 4.8. It wasn''t until November (and Claude Fable 5.2) that I felt familiar enough with how it all works to confidently ship it to production! Bonus feature: pick your version of SQLite I added this feature last night as an off-shoot of investigating a potential bug in SQLite. Datasette is meant to work against any released version of SQLite since 3.25 (released in September 2018). There have been 87 further releases since then. Being able to fire up Datasette against a specific version of SQLite is really useful for demonstrating new SQLite features and debugging weird issues. In the past I''ve used various tricks to get different SQLite versions running with Python on my Mac, but it''s not a trivial process. WebAssembly is perfect for this. I can load up a custom SQLite build entirely in my browser, test it there, then discard it when I close my tab. datasette/pysqlite3-wasm-wheels is my new repository which contains WebAssembly compiled Python wheels for pysqlite3 for 163 different SQLite versions. Each version gets a file with a name like this: pysqlite3-0.6.2-3.54.1-cp314-cp314-pyemscripten_2026_0_wasm32.whl That''s for SQLite 3.54.1, the most recent version. Datasette Lite now takes a special ?sqlite= URL parameter, which will accept a version number like 3.54.1, resolve the path to the corresponding wheel file, then fetch that wheel and install it into the Pyodide environment and configure Datasette to use it. Once I''d built the wheels, I decided to have Claude Fable 5.2 build me [this bonus tool](https://datasette.github.io/pysqlite3-wasm-wheels/?versions=3.54.1,3.54.0&sql=select+json_object(%27a%27%2C+(select+json_array(1%29%29%29) for comparing a SQL query across any selected versions of SQLite. Here''s an example showing how select json_object(''a'', (select json_array(1))) differs between SQLite 3.54.0 and 3.54.1: This should work for every other Python app, too Nothing about how Datasette Lite works is unique to Datasette. Almost any Python application should be able to pull the same trick - running entirely in the browser using WebAssembly and Pyodide, as a cheap way to provide preview environments, demonstrate features, and recreate bugs. C and Rust extensions used to be a blocker for this kind of thing, but any good coding agent can compile those to Pyodide-compatible WebAssembly now. You can even publish them to PyPI as of PEP 783 in April 2026. One catch is databases. SQLite is no problem at all, but running PostgreSQL or MySQL in WebAssembly, while possible, is still a path less trodden. I''d love to see WebAssembly preview environments like these become a common pattern across the Python ecosystem. For Hacker News: Full server-side Datasette Python app in the browser with service workers Tags: javascript, projects, python, urls, datasette, webassembly, service-workers, pyodide, datasette-lite'
content: extracted
html: 2026-11-17-running-the-full-server-side-datasette-python-app-in-the.html
remote_preview:
  url: https://static.simonwillison.net/static/2026/datasette-lite-diag.webp
  alt: 'Architecture diagram titled "What happens when you click a link in Datasette Lite", with a legend showing a solid arrow for "request" and a dashed arrow for "response". A blue box labeled "index.html: the page you opened - Each tab picks a random session and starts one Web Worker." contains two whit'
---

[Datasette Lite](https://lite.datasette.io/) is my port of the [Datasette](https://datasette.io/) Python application to run directly in the browser using WebAssembly and [Pyodide](https://pyodide.org/). I’ve released a complete rewrite of Datasette Lite using service workers, and it’s now almost indistinguishable from the real server-side application.

You can now do all of the following, using a carefully crafted URL to **lite.datasette.io**:

- Load client-side Datasette to browse any SQLite database, JSON file, CSV file or SQL script
- Select a released version of Datasette to load from PyPI
- Load a specific commit, tag, or branch name from the Datasette GitHub repository
- Pick from over 100 versions of SQLite to use with Datasette
- Install additional Datasette plugins from PyPI or URLs to wheel files, including JavaScript visualization tools

The ability to load specific Datasette references, plugins, and SQLite versions makes this a fantastic tool for linking to demos of work-in-progress features or bug reproductions, without needing to spin up a full web server to serve that demo.

As an example, consider the following URL:

[https://lite.datasette.io/?install=datasette-maplibre&url=https://gist.githubusercontent.com/simonw/39fa5d399465766039549912e2b3d291/raw/f6dfba99a75cbb8b5d9386223220eb52272767ee/demo.db&metadata=https://gist.githubusercontent.com/simonw/39fa5d399465766039549912e2b3d291/raw/f6dfba99a75cbb8b5d9386223220eb52272767ee/metadata.yml#/demo/capitals_and_countries%23map=0/4.2/180&color=continent](https://lite.datasette.io/?install=datasette-maplibre&url=https://gist.githubusercontent.com/simonw/39fa5d399465766039549912e2b3d291/raw/f6dfba99a75cbb8b5d9386223220eb52272767ee/demo.db&metadata=https://gist.githubusercontent.com/simonw/39fa5d399465766039549912e2b3d291/raw/f6dfba99a75cbb8b5d9386223220eb52272767ee/metadata.yml#/demo/capitals_and_countries%23map=0/4.2/180&color=continent)

Follow that link and, after a short loading screen, you’ll find yourself on a page that looks like this:

![Screenshot of a web map interface. Toolbar: \"View\" set to \"Clusters\", \"Colour by\" set to \"continent\", plus \"Filter to this area\" and \"Download GeoJSON\" buttons. A world map shows countries outlined and shaded by continent, with coloured dots and numbered cluster circles. A popup for a point in South America reads: capital Caracas, country Venezuela, continent South America, capital_population 2985000, latitude 10.502944, longitude -66.918983. Legend: Africa (52), Asia (45), Europe (39), North America (16), South America (13), Oceania (6). Text: \"All 171 rows are on the map\". Below, a table with columns country, continent, capital_population, latitude, longitude, boundary. First row: Afghanistan, Asia, 3277000, 34.518636, 69.181314, with a small map in the boundary column showing Afghanistan outlined and shaded in blue.](https://static.simonwillison.net/static/cors-allow/2026/datasette-maplibre.webp)

That’s Datasette running the new [datasette-maplibre](https://github.com/datasette/datasette-maplibre) plugin, loading up a database of GeoJSON shapes of countries and displaying both a combined map and individual boundary maps in a table.

Let’s break down that URL to see what it’s doing:

```
https://lite.datasette.io/
  ?install=datasette-maplibre
  &url=https://.../demo.db
  &metadata=https://.../metadata.yml
  #/demo/capitals_and_countries%23map=0/4.2/180&color=continent
```

The `?install=datasette-maplibre` installs that additional plugin from PyPI. You can pass `&install=...` as many times as you like to load more plugins.

`&url=https://.../demo.db` loads that SQLite database from the provided URL. These URLs need to have [open CORS headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)—I host files like this in [Gists](https://gist.github.com/), because they provide an easy way to get open CORS headers on a web-hosted file.

`&metadata=https://.../metadata.yml` loads [this metadata.yml file](https://gist.githubusercontent.com/simonw/39fa5d399465766039549912e2b3d291/raw/f6dfba99a75cbb8b5d9386223220eb52272767ee/metadata.yml). The metadata file adds HTML descriptions to various tables, and to the Datasette homepage. Visit [that homepage here](https://lite.datasette.io/?install=datasette-maplibre&url=https://gist.githubusercontent.com/simonw/39fa5d399465766039549912e2b3d291/raw/f6dfba99a75cbb8b5d9386223220eb52272767ee/demo.db&metadata=https://gist.githubusercontent.com/simonw/39fa5d399465766039549912e2b3d291/raw/f6dfba99a75cbb8b5d9386223220eb52272767ee/metadata.yml#/) for a guided tour of the other features of that plugin, all defined in the metadata file.

`#/demo/capitals_and_countries%23map=0/4.2/180&color=continent` is a fragment hash that turns into a location within Datasette—here it’s loading the `/demo/capitals_and_countries` SQL view, then appending *another* fragment hash to it (`%23` is a URL-encoded `#`) which tells the map plugin to zoom to a certain location and to set the color of the shapes on the map based on the `continent` column in the data.

That URL is doing a lot of work! I wrote more about this trick for building URL-addressable software in [URL-addressable Pyodide Python environments](https://simonwillison.net/2025/Feb/13/url-addressable-python/) last year.

The full set of supported URL arguments is [described in the Datasette Lite README](https://github.com/simonw/datasette-lite#load-a-different-datasette-version).

I’ve already started using Datasette Lite as part of the Datasette development process. Here’s [a recent issue](https://github.com/simonw/datasette/issues/3056#issuecomment-6902484982) describing a pagination bug—I posted a comment there demonstrating the old, broken behavior and the new fix using links to Datasette Lite:

> Demo of the bug: [https://lite.datasette.io/?ref=1.0a42&sql=https://gist.github.com/simonw/75ececf61d50e6e8d50f0c150150ee81#/data/scores?_size=2&_sort=score](https://lite.datasette.io/?ref=1.0a42&sql=https://gist.github.com/simonw/75ececf61d50e6e8d50f0c150150ee81#/data/scores?_size=2&_sort=score)—click next and the second page is empty.
>
> Here’s that same page with the bug fixed (running on 1.0a43): [https://lite.datasette.io/?ref=1.0a43&sql=https://gist.github.com/simonw/75ececf61d50e6e8d50f0c150150ee81#/data/scores?_size=2&_sort=score](https://lite.datasette.io/?ref=1.0a43&sql=https://gist.github.com/simonw/75ececf61d50e6e8d50f0c150150ee81#/data/scores?_size=2&_sort=score)

#### How this works, using service workers

Datasette Lite is not a new project. I launched the first version [back in 2022](https://simonwillison.net/2022/May/4/datasette-lite/), originally as a bit of a joke (running server-side apps in the browser is pretty funny) but one that quickly started proving its value.

The problem with that initial version is that it was a *hack*. It worked by running Datasette in a Pyodide web worker, simulating requests through that stack, then injecting the resulting HTML back into the top level page using `innerHTML`.

This worked for basic Datasette, but meant that any feature that used JavaScript failed to work, because script tags in `innerHTML` don’t execute, and the web worker tricks it used failed to load additional JavaScript or CSS files.

The new version takes a completely different approach based on [Service Workers](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API). These allow JavaScript to intercept HTTP requests for new pages and resources and serve them from client-side code instead.

Datasette Lite registers a service worker for the entire `lite.datasette.io` domain. The parent page then creates an iframe that sends requests for Datasette pages to URLs that look like this:

```
/app/aa94e6851595/ # the homepage (HTML)
/app/aa94e6851595/data/places # a table page (HTML)
/app/aa94e6851595/-/static/navigation-search.js # (JavaScript)
/app/aa94e6851595/-/static/app.css # (CSS)
```

Those are regular Datasette URLs, but with a `/app/aa94e6851595/` prefix. What’s that about?

The `/app/<session>/` prefix is necessary because of multiple tabs.

Consider the URL above, which loads the demo instance with the `datasette-maplibre` plugin. That URL defines a full, custom Datasette instance. You might have that one running in a tab, then one of my other demo links running in another.

The service worker for `lite.datasette.io` is shared between all of those tabs. If it routed to a single instance of Datasette then every tab would share that underlying instance, no matter what plugins or other settings their URL attempted to configure.

Instead, the parent page generates a random session ID and uses it as a prefix for all of the subsequent hidden asset and page loads. The service worker can then use that session ID to route those requests back through the parent page to a web worker running a dedicated Datasette instance for that particular session.

(We still need a web worker because Pyodide in a service worker would not be able to maintain state beyond around 30 seconds, depending on the browser. There are a lot of moving parts here!)

I had Claude Fable 5.2 [generate several SVG diagrams](https://gisthost.github.io/?4192a5a24136cad4c8490ccb15e87d83) to help explain this. Here’s my favorite of those, now embedded in the Datasette Lite README:

![Architecture diagram titled \"What happens when you click a link in Datasette Lite\", with a legend showing a solid arrow for \"request\" and a dashed arrow for \"response\". A blue box labeled \"index.html: the page you opened - Each tab picks a random session and starts one Web Worker.\" contains two white boxes: \"iframe Datasette pages at /app/session/…\" and \"shell.js the page's own script: relays\". Below it is a yellow box labeled \"sw.js: the service worker - Intercepts requests for /app/session/….\" To the right is a green box marked 4 and labeled \"webworker.js: a Web Worker - Pyodide: Python in WebAssembly\" containing three stacked white boxes: \"ASGI bridge + cookie jar\", \"Datasette ASGI app\" and \"SQLite databases\". Numbered arrows connect them: solid arrow 1 from the iframe down to sw.js, solid arrow 2 from sw.js up to shell.js, solid arrow 3 from shell.js across to the Web Worker, dashed arrow 5 from the Web Worker back to shell.js, dashed arrow 6 from shell.js down to sw.js, and dashed arrow 7 from sw.js up to the iframe. A numbered key at the bottom reads: 1 The page requests /app/session/fixtures. 2 sw.js intercepts it and posts it to the owning tab. 3 shell.js forwards it to the Web Worker. 4 Python adds cookies and calls Datasette, which queries SQLite. 5 The status, headers and body return to the page, 6 then to sw.js over a MessagePort, 7 which hands the browser a Response. The iframe renders it.](https://static.simonwillison.net/static/2026/datasette-lite-diag.webp)

This system started life [back in May](https://simonwillison.net/2026/May/30/pyodide-asgi-browser/) as an [AI research spike](https://github.com/simonw/research/tree/main/pyodide-asgi-browser#readme) generated using Claude Opus 4.8. It wasn’t until November (and Claude Fable 5.2) that I felt familiar enough with how it all works to confidently ship it to production!

#### Bonus feature: pick your version of SQLite

I added this feature [last night](https://github.com/simonw/datasette-lite/commit/de661dbee42a68304811a0035b78e6b683203cd1) as an off-shoot of investigating [a potential bug in SQLite](https://simonwillison.net/2026/Nov/16/llm/).

Datasette is meant to work against any released version of SQLite since 3.25 (released in September 2018). There have been 87 further releases since then.

Being able to fire up Datasette against a specific version of SQLite is really useful for demonstrating new SQLite features and debugging weird issues.

In the past I’ve [used various tricks](https://til.simonwillison.net/sqlite/sqlite-version-macos-python) to get different SQLite versions running with Python on my Mac, but it’s not a trivial process.

WebAssembly is perfect for this. I can load up a custom SQLite build entirely in my browser, test it there, then discard it when I close my tab.

[datasette/pysqlite3-wasm-wheels](https://github.com/datasette/pysqlite3-wasm-wheels) is my new repository which contains WebAssembly compiled Python wheels for [pysqlite3](https://github.com/coleifer/pysqlite3) for 163 different SQLite versions. Each version gets a file with a name like this:

`pysqlite3-0.6.2-3.54.1-cp314-cp314-pyemscripten_2026_0_wasm32.whl`

That’s for SQLite 3.54.1, the most recent version.

Datasette Lite now takes a special `?sqlite=` URL parameter, which will accept a version number like `3.54.1`, resolve the path to the corresponding wheel file, then fetch that wheel and install it into the Pyodide environment and configure Datasette to use it.

Once I’d built the wheels, I decided to have Claude Fable 5.2 build me \[this bonus tool\]([https://datasette.github.io/pysqlite3-wasm-wheels/?versions=3.54.1,3.54.0&sql=select+json_object(%27a%27%2C+(select+json_array(1%29%29%29)](https://datasette.github.io/pysqlite3-wasm-wheels/?versions=3.54.1,3.54.0&sql=select+json_object(%27a%27%2C+(select+json_array(1%29%29%29)) for comparing a SQL query across any selected versions of SQLite. Here’s an example showing how `select json_object('a', (select json_array(1)))` differs between SQLite 3.54.0 and 3.54.1:

![Screenshot of a web tool for comparing SQL across SQLite versions. Heading \"SQLite versions (163 available)\" sits above a filter text input with placeholder \"Filter, e.g. 3.4\", a \"Clear selection\" button and a blue \"Start REPL with 2 versions\" button. Below is a scrollable grid of pill-shaped checkboxes, one per version, running from 3.54.1 down past 3.30.0, with 3.54.1 and 3.54.0 checked. A \"REPL\" heading follows, with two green status badges reading \"3.54.1 ready\" and \"3.54.0 ready\", then a textarea containing \"select json_object('a', (select json_array(1)))\" beside a blue \"Run\" button, and a row of clickable example queries labeled \"Examples:\". The output section shows the prompt \"sqlite> select json_object('a', (select json_array(1)))\" followed by an orange warning \"Versions differ: 2 different results\" and two side-by-side yellow result panels, each with the query as its column header. The \"SQLite 3.54.1\" panel returns a JSON object where key \"a\" has a string value: the array containing 1, wrapped in double quotes. The \"SQLite 3.54.0\" panel returns a JSON object where key \"a\" has an actual array containing 1, with no quotes around it.](https://static.simonwillison.net/static/2026/compare-sqlite.webp)

#### This should work for every other Python app, too

Nothing about how Datasette Lite works is unique to Datasette. Almost any Python application should be able to pull the same trick—running entirely in the browser using WebAssembly and Pyodide, as a cheap way to provide preview environments, demonstrate features, and recreate bugs.

C and Rust extensions used to be a blocker for this kind of thing, but any good coding agent can compile those to Pyodide-compatible WebAssembly now. You can [even publish them to PyPI](https://github.com/pypi/warehouse/pull/19804) as of PEP 783 in April 2026.

One catch is databases. SQLite is no problem at all, but running PostgreSQL or MySQL in WebAssembly, while possible, is still a path less trodden.

I’d love to see WebAssembly preview environments like these become a common pattern across the Python ecosystem.

For Hacker News: Full server-side Datasette Python app in the browser with service workers
