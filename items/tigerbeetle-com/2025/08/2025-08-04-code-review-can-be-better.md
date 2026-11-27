---
title: Code Review Can Be Better
link: https://tigerbeetle.com/blog/2025-08-04-code-review-can-be-better
source: tigerbeetle-com
published: 2025-08-04T00:00:00Z
updated: 2025-08-04T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- matklad
summary: 'Slightly unusual genre today: a negative result about our git-review tool for a different take on code review process, which we decided to shelve, at least for the time being.'
content: extracted
html: 2025-08-04-code-review-can-be-better.html
remote_preview:
  url: https://tigerbeetle.com/blog/2025-08-04-code-review-can-be-better/banner-6257f099.webp
---

Slightly unusual genre today: a [negative result](https://github.com/tigerbeetle/tigerbeetle/pull/3129) about our [`git-review`](https://github.com/tigerbeetle/tigerbeetle/pull/2732) tool for a different take on code review process, which we decided to shelve, at least for the time being.

A lot of people are unsatisfied with GitHub’s code review process. One of the primary issues is that GitHub poorly supports stacked pull requests and [interdiff reviews](https://gist.github.com/thoughtpolice/9c45287550a56b2047c6311fbadebed2). While I also see interdiff as valuable, it’s not the reason why I decided to experiment with `git-review`. I have two other problems with GitHub, and with every single other code review system, with the exception of [the thing that Jane Street uses internally](https://www.janestreet.com/tech-talks/janestreet-code-review/):

- review state is not stored as a part of repository itself,
- review is done via remote-first web-interface.

Let’s start with the second one.

By the way of analogy, I don’t use GitHub’s web editor to write code. I clone a repository locally, and work in my editor, which is:

- fully local, memory/nvme latencies, no HTTP round-trips,
- tailored to my specific odd workflow.

When I review code, I like to pull the source branch locally. Then I soft-reset the code to mere base, so that the code looks as if it was written by me. Then I fire up magit, which allows me to effectively navigate both through the diff, and through the actual code. And I even use git staging area to mark files I’ve already reviewed:

![code review in magit](https://tigerbeetle.com/blog/2025-08-04-code-review-can-be-better/magit-review-97ebd8dd.webp)\
code review in magit

Reviewing *code* rather than diff is so powerful: I can run the tests, I can go to definition to get the context, I can try out my refactoring suggestions in-place, with code completion and the other affordances of my highly sophisticated code editor.

Alas, when I want to actually leave feedback on the PR, I have to open the browser, navigate to the relevant line in the diff, and (after waiting for several HTTP round-trips) type my suggestion into a text area. For some reason, the text area also regularly lags for me, especially on larger diffs.

Two things are wrong here. On the interface side, review feedback is text related to the code. The most natural interface is to just leave review comments as inline comments in the code, or even to fix the code directly:

```
// CR(matklad): Hm, this check seems imprecise to me.
// Shouldn't we compare `replica.view` instead of `header.view` here?
if (header.view != view) return;
```

And on the implementation side, because the data is stored in a remote database, rather than in a local git repository, we get all those latency-inducing round-trips (not to mention vendor lock in).

Thus, [`git-review`](https://github.com/tigerbeetle/tigerbeetle/pull/2732) was born. The idea is simple:

- Code review is a single commit which sits on top of the PR branch.
- That commit adds a bunch of code comments with specific markers.
- Review process involves both the author and the reviewer modifying this top commit (so, there’s a fair amount of `git push --force-with-lease` involved).
- The review concludes when all threads were marked as `//? resolved` and an explicit revert commit is added on top (such that review is preserved in the history).

It didn’t exactly fail, but wasn’t a spectacular success. When it comes to writing tools, I am a huge fan of getting-rich-quick schemes, of writing 500 lines of [hacky, use-case specific code](https://matklad.github.io/2024/02/10/window-live-constant-time-grep.html) which works better for *that* use-case than [something serious](https://burntsushi.net/ripgrep/). A simple script for converting *your* `.md` to `.html` [can be cheaper to maintain](https://tigerbeetle.com/blog/2025-02-27-why-we-designed-tigerbeetles-docs-from-scratch/) than a general purpose solution.

I had a hope that “code review is just a commit” would be the secret to keep implementation complexity low. Sadly, the devil is in the details in this particular case.

The basic idea, that reviewing is leaving comments in code, works as well as I had expected (that is, it’s really, really awesome). But modifying code under review turned out to be tricky. If a reviewer requests a change, and you apply it to some deep commit, or even add a new commit on top, you now have to solve mere conflicts with the review comments themselves, as they are often added at the [hunk](https://git-scm.com/book/en/v2/Git-Tools-Interactive-Staging) boundaries. And then, while `--force-with-lease` is workable, it also adds friction. There is an impedance mismatch here, where, for code, we want very strong, hash-chained intentional sequence of state-transitions, while for review we would be more happy with more lax conflict-free merging rules. It *might* be solved with more tooling to “push” and “pop” review comments on top of pristine review branch, but that seems to push well beyond my 500 line limit.

Then, there’s a second change. It seems like [upstream git might be getting a Gerrit-style Change-Id](https://lore.kernel.org/git/CAESOdVAspxUJKGAA58i0tvks4ZOfoGf1Aa5gPr0FXzdcywqUUw@mail.gmail.com/T/#u) for tracking revisions of a single commit over rebases. If that happens, we might actually get first class support for per-commit interdiff review! But that would be somewhat incompatible with `git-review` approach, which adds an entire separate commit to the branch. But, perhaps, in the `Change-Id` world, we could be adding review comments to the commits themselves, and, rather that adding a revert at the conclusion of review, instruct git to store all revisions of a particular `Change-Id`.

Anyway, we are begrudgingly back to web-interface based code reviews for now. Hopefully someone is inspired enough to fix this properly one day!

If you’ve been thinking along similar lines, the following links are worth checking out:

- [Fossil](https://fossil-scm.org/home/doc/trunk/www/index.wiki) is an SCM system which stores everything in the repository.
- [NoteDb](https://gerrit-review.googlesource.com/Documentation/note-db.html) backend for Gerrit. Gerrit started with tracking review state in a separate database, but then moved storage into git.
- [git-bug](https://github.com/git-bug/git-bug) uses git to store information about issues.
- [git-appraise](https://github.com/google/git-appraise) uses git to store information about reviews.
- [prr](https://doc.dxuuu.xyz/prr/index.html) which implements in-editor review interface on top of GitHub’s Web API
- [How Jane Street Does Code Review](https://www.janestreet.com/tech-talks/janestreet-code-review/) shows that a better world is possible, and that it is already here, just not everywhere.
- [git-pr](https://pr.pico.sh) similar project in spirit that leverages git native features to replace the entire pull request workflow.
- [flirt](https://blog.buenzli.dev/flirt-native-backend/#storing-review-information-in-a-git-repository) seems to be the most advanced implementation of local-first review so far.
