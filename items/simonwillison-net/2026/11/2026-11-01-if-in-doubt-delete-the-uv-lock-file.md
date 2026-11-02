---
title: If in doubt, delete the uv.lock file
link: https://simonwillison.net/2026/Nov/1/delete-uv-lock-first/
source: simonwillison-net
published: 2026-11-01T23:24:22Z
updated: 2026-11-01T23:24:22Z
first_seen: 2026-11-02T00:33:20.199752031Z
labels:
- python
- til
- uv
summary: 'I''ve been getting the occasional confusing Python dependency error since switching over to Python 3.15 and the rule I''m having to remember is: If in doubt, delete the uv.lock file Most of my projects are libraries which don''t include a uv.lock file, since it''s rude for a library dependency to lock versions for anything else that uses it. Running uv run pytest etc tends to leave uv.lock files lying around, which are listed in .gitignore and so do not get committed to my repositories. But... sometimes that uv.lock pins a version of a dependency that doesn''t yet have a Python 3.15 wheel. An example from earlier today: error: Failed to build `watchfiles==1.1.1` cause: Call to `maturin.build_wheel` failed error: the configured Python interpreter version (3.15) is newer than PyO3''s maximum supported version (3.14) Deleting that file and running uv run pytest again usually fixes the problem. Tags: uv, til, python'
content: extracted
html: 2026-11-01-if-in-doubt-delete-the-uv-lock-file.html
---

I've been getting the occasional confusing Python dependency error since switching over to Python 3.15 and the rule I'm having to remember is:

**If in doubt, delete the uv.lock file**

Most of my projects are libraries which don't include a `uv.lock` file, since it's rude for a library dependency to lock versions for anything else that uses it.

Running `uv run pytest` etc tends to leave `uv.lock` files lying around, which are listed in `.gitignore` and so do not get committed to my repositories.

But... sometimes that `uv.lock` pins a version of a dependency that doesn't yet have a Python 3.15 wheel. An example from earlier today:

```
error: Failed to build `watchfiles==1.1.1`
  cause: Call to `maturin.build_wheel` failed

error: the configured Python interpreter version (3.15) is newer
than PyO3's maximum supported version (3.14)
```

Deleting that file and running `uv run pytest` again usually fixes the problem.
