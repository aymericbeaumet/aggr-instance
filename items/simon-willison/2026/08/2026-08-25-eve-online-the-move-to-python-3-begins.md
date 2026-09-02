---
title: 'EVE Online: The Move to Python 3 Begins!'
link: https://simonwillison.net/2026/Aug/25/eve-online-move-to-python-3/
source: simon-willison
published: 2026-08-25T22:59:30Z
updated: 2026-08-25T22:59:30Z
first_seen: 2026-09-02T14:07:29.769486161Z
tags:
- eve-online
- migrations
- python
- python3
- stackless
summary: 'EVE Online: The Move to Python 3 Begins! EVE Online has been one of the most interesting case studies in Python at scale for over twenty years now. They''ve been running on Stackless Python since their launch in 2003, and their last major upgrade was 16 years ago, to Stackless Python 2.7 in 2010. Their upgrade to Python 3 will start using the futurize script against 2.4 million lines of code, followed by careful manual review of the ~20,000 places where Python 2 and 3 behavior differ - for example 1 / 2 is 0 in Python 2 but is 0.5 in Python 3. There''s nothing in this announcement about how they plan to replace Stackless, but at their conference last year they presented Scheduling in Carbon: Leaving Stackless Python Behind describing how they replaced Stackless in the Carbon engine for their more recent game EVE Frontier, using their (now open source) carbonengine/scheduler library. Via Lobster.rs Tags: eve-online, migrations, python, python3, stackless'
content: feed
html: 2026-08-25-eve-online-the-move-to-python-3-begins.html
---

**[EVE Online: The Move to Python 3 Begins!](https://www.eveonline.com/news/view/the-move-to-python-3-begins)**

EVE Online has been one of the most interesting case studies in Python at scale for over twenty years now.

They've been running on [Stackless Python](https://github.com/stackless-dev/stackless/wiki/) since their launch in 2003, and their last major upgrade was 16 years ago, to Stackless Python 2.7 [in 2010](https://www.eveonline.com/news/view/stackless-python-2.7).

Their upgrade to Python 3 will start using the [futurize](https://python-future.org/futurize.html) script against 2.4 million lines of code, followed by careful manual review of the ~20,000 places where Python 2 and 3 behavior differ - for example `1 / 2` is `0` in Python 2 but is `0.5` in Python 3.

There's nothing in this announcement about how they plan to replace Stackless, but at their conference last year they presented [Scheduling in Carbon: Leaving Stackless Python Behind](https://youtu.be/-x299qHLQs0) describing how they replaced Stackless in the Carbon engine for their more recent game EVE Frontier, using their (now open source) [carbonengine/scheduler](https://github.com/carbonengine/scheduler) library.

Via [Lobster.rs](https://lobste.rs/s/e1oalq/move_python_3_begins)

Tags: [eve-online](https://simonwillison.net/tags/eve-online), [migrations](https://simonwillison.net/tags/migrations), [python](https://simonwillison.net/tags/python), [python3](https://simonwillison.net/tags/python3), [stackless](https://simonwillison.net/tags/stackless)
