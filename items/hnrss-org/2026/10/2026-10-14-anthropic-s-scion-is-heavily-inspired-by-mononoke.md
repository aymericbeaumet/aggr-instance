---
title: Anthropic's Scion is heavily inspired by Mononoke
link: https://sunshowers.io/posts/anthropic-mononoke/
source: hnrss-org
published: 2026-10-14T20:30:54Z
updated: 2026-10-14T20:30:54Z
first_seen: 2026-10-14T21:35:38.678109349Z
authors:
- sunshowers
content: extracted
html: 2026-10-14-anthropic-s-scion-is-heavily-inspired-by-mononoke.html
extra:
  comments_url: https://news.ycombinator.com/item?id=50094694
  num_comments: 22
  points: 116
---

*Update 2026-10-15: I wrote [a followup](https://sunshowers.io/posts/anthropic-mononoke-2/) to answer questions and concerns.*

* * *

*Discuss on [Hacker News](https://news.ycombinator.com/item?id=50094694) or [Lobsters](https://lobste.rs/s/plcgps/anthropic_s_scion_is_heavily_inspired_by).*

Here is Anthropic [describing](https://www.anthropic.com/engineering/redesigning-source-control) ([archive link](https://ghostarchive.org/archive/hCFVA)) yesterday how their new version control system, Scion, uses a concept called “derived data” to make operations like blames more efficient:

[![A changeset records only what’s changed, so on its own, it can’t quickly answer questions like who last edited a line or which commits affected a file. To answer those, Scion builds additional data structures from the changesets, including dedicated structures for file blame and path history and skip lists for the commit graph. We call everything that isn’t directly part of the changeset “derived data,” and we compute it all through one generic mechanism. Each type of derived data is defined recursively, so its value for a changeset is computed from its values for that changeset’s parents. This mechanism has three benefits. First, we can easily add a new type of derived data when a new data structure would make some operation faster. Second, for each type, we can choose whether to compute it eagerly or defer the work, so we avoid materializing data we don’t need. And third, because the client never sees the internals of these data structures, we can optimize them without changing the client. We’ve added new types of derived data many times as the system has grown, such as new indices for performing copy detection or path-based file search.](https://sunshowers.io/images/scion-derived-data.png)](https://sunshowers.io/images/scion-derived-data.png)\
How Anthropic describes Scion’s derived data.

Here is me, back in 2022, describing in my Oxide candidate materials how the [Mononoke](https://github.com/facebook/sapling/tree/main/eden/mononoke) source control server I helped build uses a concept called “derived data” to make operations like blames more efficient:

[![Derived data. While Bonsai changesets work great for pushes, they work less well for things like diffs. Git diff commit1..commit2 is an O(n log n) operation where n is the number of changed files – with Bonsai changesets, you’d have to look at every single commit between commit1 and commit2 to compute the list of files changed. For a large repository this can be orders of magnitude slower (and compounds the pointer-chasing problem). To solve this, we moved the diff-friendly representation to being derived data (i.e. an index). The server creates a separate Merkle DAG with a Git-like tree/blob model, with pointers back to Bonsai changesets as required. Note that: Derived data can be computed and stored asynchronously, outside of the main critical section. The pointers between commits and trees are flipped around. With the Git data model, the text of a commit contains the hash of the root tree. With the Bonsai model, the text of the root tree contains the hash of the commit that introduced it. This gave us the ability to build out other derived indexes for whatever other operations we wanted. For example, git blame is quite slow on large git repositories, requiring a traversal of the entire repository (not just commits that touch the file). With Mononoke, we built out a blame-friendly index where each version of each file was a separate node in the Merkle DAG, with its parent set to the previous version of the file, and with pointers back to Bonsai changesets. This meant that blames became O(number of commits that touch the file) rather than O(number of commits in the repository). Over time, the team has added more indexes to handle specific use cases, and Mononoke now has 8 separate indexes. Adding a new index is a relatively cheap operation that doesn’t involve any fundamental changes to data structures.](https://sunshowers.io/images/mononoke-derived-data.png)](https://sunshowers.io/images/mononoke-derived-data.png)\
How I described Mononoke’s derived data.

I think the level of imitation goes well beyond having similar insights[^1]. The fact that Sapling was only credited for “hard-won lessons”, and that Mononoke was not mentioned at all, borders on plagiarism. The post comes off as Anthropic claiming that Claude designed all this, while the reality is that our (extraordinary) team at Meta came up with many of the insights that Claude dutifully applied almost a decade later. (We looked at plenty of prior art for Mononoke and Sapling too, from Google’s SrcFS to write-ahead logs in databases.)

I am very glad that the vision our team had back in 2017-18 was so powerful and enduring — when you go against the grain, you’re always a bit unsure if what you’re doing is genius or madness, and I think Mononoke’s design has stood the test of time. And, to be clear, I do not have a problem with Anthropic copying Mononoke’s design. (This is part of why we put out open source — to benefit humanity through the spread of good ideas.)

But as LLMs continue to assist in designing new systems, they are likely to index heavily on existing designs. Mononoke is [open source](https://github.com/facebook/sapling/tree/main/eden/mononoke) and in every LLM’s training set. If you ask [Opus 5.5](https://claude.ai/share/e6a66ec5-7757-4adb-901f-0e33b1c38536) or [GPT-6 Astra](https://chatgpt.com/share/6ace7be6-68f8-83ea-ad68-0f9d14448318) (thanks [Matthew Fowle](https://bsky.app/profile/jauntywunderkind.bsky.social/post/3mxsettjnoc22)) about prior art, they both immediately point out the similarities to Mononoke.

Giving credit to those that came before us is now easier — and more important — than ever. There is no longer an excuse.

[^1]: For an example of a system with similar insights but many key design differences, see [Vicent Marti’s blog post for Cursor](https://cursor.com/blog/git-at-any-scale).
