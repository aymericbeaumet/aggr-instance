---
title: Small programming tricks
link: https://will-keleher.com/posts/small-programming-tricks-matter/
source: hnrss-org-frontpage
published: 2026-09-16T15:56:47Z
updated: 2026-09-16T15:56:47Z
first_seen: 2026-09-16T20:24:49.200228376Z
authors:
- signa11
summary: 'Article URL: https://will-keleher.com/posts/small-programming-tricks-matter/ Comments URL: https://news.ycombinator.com/item?id=49729000 Points: 231 # Comments: 131'
content: extracted
html: 2026-09-16-small-programming-tricks.html
---

Day to day, I think a surprising amount of engineering productivity comes from small nuggets of knowledge: being aware that a language feature exists; knowing that an unexplained tcp delay is probably related to the TCP\_NO\_DELAY setting and Nagle’s algorithm; knowing the right `git` incantation to get out of a pickle; or knowing a trick with `sed` to rewrite a file.

In one sense, this is self-evident: anything you know is going to be made up of smaller pieces of knowledge. *Of course* those smaller pieces of knowledge matter.

But I think there are some nuggets of knowledge that are particularly valuable and don’t require a lot of supporting mental infrastructure. You don’t need to know any python to use `python3 -m http.server` to start a simple server in a directory, but it might still make your work marginally easier. Let me share a few examples:

- You probably know that `ctrl + r` allows searching your terminal’s command history, but if you install [fzf](https://github.com/junegunn/fzf), you can set it up so that `ctrl + r` does a fuzzy search. If you want even more power, [atuin](https://github.com/ellie/atuin) replaces your shell history with a searchable SQLite database. [per-directory-history](https://github.com/jimhester/per-directory-history) lets you switch back and forth between searching for commands that have been run in a specific directory or searching all previous commands. Finally, you can configure how much history to store: [stackoverflow question](https://unix.stackexchange.com/questions/273861/unlimited-history-in-zsh).
- You can `SELECT` without a `FROM`. This can be useful for testing out how a function in your database actually works or reminding yourself how `SELECT TRUE <> NULL` works.[1](https://will-keleher.com/posts/small-programming-tricks-matter/#fn:1)
- PostgresSQL and MySQL both support `explain analyze` which will actually run the query you’re trying to optimize and give you a ton more information about its performance.
- In regular expressions, `\b`, the [word boundary assertion](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Regular_expressions/Word_boundary_assertion), makes it easy to look for the beginnings or ends of words.
- You can use logarithms with metrics to get a sense of the distribution of values for a field you’re interested in:

  ```js
  const bucket = Math.floor(Math.log10(userInGroupCount))
  metrics.increment("my_metric", { bucket });
  ```

- Modern JS now supports [Array.flatMap](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/flatMap), [Object.entries](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/entries), and [Promise.withResolvers](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/withResolvers).
- In NodeJS, you can keep a connection open to an external resource by creating an [https.Agent](https://nodejs.org/api/https.html#class-httpsagent) and then providing it to your http requests: `fetch(url, {method, agent})`. This can have a dramatic impact on latency.
- `git log -S pattern` ([”git pickaxe”](https://git-scm.com/book/en/v2/Git-Tools-Searching)) can give you all commits that *added or removed* a string in a codebase. It’s amazingly useful especially with older codebases! (`git log -G pattern` is similar, but will also show when that line was moved)
- Similar to `cd -`, you can use `git checkout -` to check out your previous HEAD.
- [You probably don’t need `find`](https://will-keleher.com/posts/you-probably-dont-need-find/). A lot of `find` commands can be replaced with globs like `**/*.md`. Most shells support this out of the box, but with bash, you need to turn this on with `shopt -s globstar`.
- In a similar vein, most folks will probably want to use `rg` (ripgrep) rather than `grep`, `ack`, or `ag`.
- zsh’s advanced autocompletion features aren’t turned on by default:

  ```sh
  if type brew &>/dev/null; then
      FPATH="$(brew --prefix)/share/zsh/site-functions:${FPATH}"
  fi
  autoload -Uz compinit
  compinit
  ```

You might have already known all of these things! Or you might work in a domain that makes all of these little tricks totally useless. Even if this particular set of tricks isn’t useful for you, I bet you have your own stash of tricks that you’ve accumulated over the years that makes your work easier.

At a company, I think even more knowledge tends to be this sort of small high-leverage nugget:

- To debug $PROBLEM, use $DATA\_SOURCE.
- $PERSON knows a ton about $AREA and they’re happy to help if you get stuck
- There are good docs about $HARD\_THING $OVER\_HERE.
- When $THING happens, it means we should manually scale out.
- To do a rolling restart of a service, run $THIS\_COMMAND.
- This $UTIL makes $THAT\_PROBLEM easy to script.

At a previous company, I shared a trick on slack every day with the engineering team, both technical and company-specific, and folks found them pretty useful. Even if you knew 9/10 tricks, that 10th doc or technique might save you some time! And one trick per day was the right number to avoid overwhelming people with knowledge, and it could occasionally spark useful discussion. If you’re a more senior engineer at your company, you might think about doing something similar.

* * *

1. I think I first saw this technique on [Julia Evan’s blog](https://jvns.ca/), and I think her writing often perfectly encapsulates the idea behind this post: "small bits of knowledge are powerful! and fun! and approachable!!" [↩︎](https://will-keleher.com/posts/small-programming-tricks-matter/#fnref:1)
