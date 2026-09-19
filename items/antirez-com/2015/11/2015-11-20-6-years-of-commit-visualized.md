---
title: 6 years of commit visualized
link: http://antirez.com/news/98
source: antirez-com
published: 2015-11-20T10:58:34Z
updated: 2015-11-20T10:58:34Z
first_seen: 2026-09-19T21:30:23.395188495Z
summary: 'Today I was curious about plotting all the Redis commits we have on Git, which are 90% of all the Redis commits. There was just an initial period where I used SVN but switched very soon. Full size image here: http://antirez.com/misc/commitsvis.png !~! Each commit is a rectangle. The height is the number of affected lines (a logarithmic scale is used). The gray labels show release tags. There are little surprises since the amount of commit remained pretty much the same over the time, however now that we no longer backport features back into 3.0 and future releases, the rate at which new patchlevel versions are released diminished. Major releases look more or less evenly spaced between 2.6, 2.8 and 3.0, but were more frequent initially, something that should change soon as we are trying to switch to time-driven releases with 3 new major release each year (that obviously will contain less new things compared to the amount of stuff was present in major releases that took one whole year). For example 3.2 RC is due for December 2015. Patch releases of a given major release tend to have a logarithmic curve shape. As a release mature, in general it gets less critical bugs. Also attention shifts progressively to the new release. I would love Github to give us stuff like that and much more. There is a lot of data in commits of a project that is there for years. This data should be analyzed and explored... it''s a shame that the graphics section is apparently the same thing for years. EDIT: The Tcl script used to generate this graph is here: https://github.com/antirez/redis/tree/unstable/utils/graphs/commits-over-time Comments'
content: extracted
html: 2015-11-20-6-years-of-commit-visualized.html
preview:
  file: 2015-11-20-6-years-of-commit-visualized.preview-a2aeceaced97.webp
  width: 256
  height: 94
  color: '#d8e7d8'
images:
- source: http://antirez.com/misc/commitsvis.png
  original:
    file: 2015-11-20-6-years-of-commit-visualized.image-46443ed56adb.png
    width: 1554
    height: 569
  color: '#fcfdfc'
---

## [6 years of commit visualized](https://antirez.com/news/98)

[antirez](https://antirez.com/user/antirez) 3956 days ago. 266459 views.

```
Today I was curious about plotting all the Redis commits we have on Git, which are 90% of all the Redis commits. There was just an initial period where I used SVN but switched very soon.

Full size image here: http://antirez.com/misc/commitsvis.png



Each commit is a rectangle. The height is the number of affected lines (a logarithmic scale is used). The gray labels show release tags.

There are little surprises since the amount of commit remained pretty much the same over the time, however now that we no longer backport features back into 3.0 and future releases, the rate at which new patchlevel versions are released diminished.

Major releases look more or less evenly spaced between 2.6, 2.8 and 3.0, but were more frequent initially, something that should change soon as we are trying to switch to time-driven releases with 3 new major release each year (that obviously will contain less new things compared to the amount of stuff was present in major releases that took one whole year).
For example 3.2 RC is due for December 2015.

Patch releases of a given major release tend to have a logarithmic curve shape. As a release mature, in general it gets less critical bugs. Also attention shifts progressively to the new release.

I would love Github to give us stuff like that and much more. There is a lot of data in commits of a project that is there for years. This data should be analyzed and explored... it's a shame that the graphics section is apparently the same thing for years.

EDIT: The Tcl script used to generate this graph is here: https://github.com/antirez/redis/tree/unstable/utils/graphs/commits-over-time
```

[blog comments powered by Disqus](https://disqus.com)
