---
title: Jujutsu Primer
link: https://ethanlowman.com/posts/jj-primer/
source: hnrss-org
published: 2026-11-20T05:46:01Z
updated: 2026-11-20T05:46:01Z
first_seen: 2026-11-20T16:02:15.998933044Z
authors:
- renehsz
content: extracted
html: 2026-11-20-jujutsu-primer.html
remote_preview:
  url: https://ethanlowman.com/img/penrose.png
---

This guide is a brief(ish) introduction to the [Jujutsu](https://docs.jj-vcs.dev/latest/) (`jj`) version control system (VCS) for someone familiar with Git.

## [Why try Jujutsu?](https://ethanlowman.com/posts/jj-primer/#why-try-jujutsu)

`jj` provides primitives for version control that are simple to learn, but elegantly compose into powerful workflows. Features like automatic rebase and native undo soften the sharp edges of Git. Eventually, you’ll organically discover new powerful workflows that would be too cumbersome to attempt with most other VCS tools.

If you work in a Git repository, you can try it today. Jujutsu and Git can be used [side-by-side in the same repository](https://docs.jj-vcs.dev/latest/git-experts/#git-can-be-used-side-by-side-in-the-same-repository), so you can try out `jj` on a `git` repo on your own, without fully ejecting from `git`.

Personally, I think `jj` considerably boosts my effectiveness, because it allows me to keep VCS bookkeeping out of my working memory. Automatic rebasing also makes stacked diff workflows quite easy, and reduces thrashing of my working memory. It’s also just a fun tool to use.

## [Installing `jj`](https://ethanlowman.com/posts/jj-primer/#installing-jj)

Official installation instructions are [here](https://docs.jj-vcs.dev/latest/install-and-setup/), but here are a few common methods:

```bash
# macOS
brew install jj

# Arch Linux
pacman -S jujutsu

# NixOS (ideally from nixpkgs-unstable)
nix-shell -p jujutsu

# From source
# (Requires a recent Rust and ~/.cargo/bin on your PATH)
cargo install --locked --bin jj jj-cli
```

## [Setup and configuration](https://ethanlowman.com/posts/jj-primer/#setup-and-configuration)

In an existing `git` repository, run:

```bash
jj git init
```

From here on, `jj` and `git` repo state will be synchronized automatically.

To set your user details globally, run commands like these:

```bash
jj config set --user user.name "Some One"
jj config set --user user.email "someone@example.com"
```

You can edit `jj`’s TOML configuration for your user in an editor with:

```bash
jj config edit --user
```

In addition to the `--user` scope, `jj config` also has `--repo` and `--workspace` scopes. View the merged config with:

```bash
jj config list
```

## [Commits and changes](https://ethanlowman.com/posts/jj-primer/#commits-and-changes)

A **commit**, also called a **revision**, is essentially the same thing as a `git` commit. It represents a snapshot of repository state that descends from one or more parent repository snapshots. It is often visualized as a diff against a singular parent commit, or in the case of a merge commit, the diff against multiple parent commits once merged.

When using a Git repo backend, the `jj` **commit ID** is the same as the `git` commit ID: a hash formatted as a hex string. In the output of `jj log` below, the hex hashes `2bea5e3d` and `849165bc` on the right are commit IDs.

```bash
$ jj log
@  kpqxywon ethan@example.com 2026-11-15 12:17:12 849165bc
│  (empty) (no description set)
◆  royxmykx ethan@example.com 2026-11-15 12:10:46 main 2bea5e3d
│  Adjust config
~
```

The IDs on the left hand side (`royxmykx` and `kpqxywon`) are **change IDs**, which are stable identifiers for logical changes as they physically evolve over time. We’ll discuss these more in the next section.

Commit IDs and change IDs can be referenced in shorthand with an unambiguous (unique) prefix. `jj` helpfully highlights the shortest unique prefix of most IDs to make it easy to type them manually. For example, instead of copying and pasting `kpqxywon`, we can type in a prefix, down to the shortest unique prefix `k`.

We can inspect the commit `2bea5e3d` with `jj show`, using its short prefix `2`.

```bash
$ jj show 2
Commit ID: 2bea5e3d1992a60ba9ee52c6ce37c55adba9e8fb
Change ID: royxmykxtrkrqppotnrvutxlvrvqyxmy
Bookmarks: main main@git main@origin
Author   : Ethan Lowman <ethan@example.com> (2026-11-15 12:10:46)
Committer: Ethan Lowman <ethan@example.com> (2026-11-15 12:10:46)

    Adjust config

diff --git a/config.py b/config.py
index 7366a4e31d..174ffea06b 100644
--- a/config.py
+++ b/config.py
@@ -1,1 +1,1 @@
-DEFAULT_LIMIT = 10
+DEFAULT_LIMIT = 20
```

Many folks configure a custom diff formatter, like [Delta](https://github.com/dandavison/delta). See [configuration docs](https://docs.jj-vcs.dev/latest/config/#processing-contents-to-be-paged) if you’re interested.

## [Creating & evolving changes](https://ethanlowman.com/posts/jj-primer/#creating-evolving-changes)

Let’s take another look at our commit graph:

```bash
$ jj log
@  kpqxywon ethan@example.com 2026-11-15 12:17:12 849165bc
│  (empty) (no description set)
◆  royxmykx ethan@example.com 2026-11-15 12:10:46 main 2bea5e3d
│  Adjust config
~
```

On commit `849165bc`, you’ll see the symbol `@` on the graph node. This represents a snapshot of our **working copy**, or the code we have actually checked out in our local directory. We can inspect this special commit pointer in a variety of ways:

`jj show` shows commit details and a diff, which is empty here.

```bash
$ jj show
Commit ID: 849165bca19639dbd6ccb53e8c1340e78c36d59a
Change ID: kpqxywonksrltlrkyqukqqsxpltznztk
Author   : Ethan Lowman <ethan@example.com> (2026-11-15 12:17:12)
Committer: Ethan Lowman <ethan@example.com> (2026-11-15 12:17:12)

    (no description set)

```

`jj status` shows repo state with diff stats.

```bash
$ jj status
The working copy has no changes.
Working copy  (@) : kpqxywon 849165bc (empty) (no description set)
Parent commit (@-): royxmykx 2bea5e3d main | Adjust config
```

`jj diff` works much like `git diff`. Its output is empty here, since there are no changes in the current working copy relative to the parent commit.

```bash
$ jj diff
```

This output isn’t very interesting, since `@` has no changes. Let’s make an example change to see what happens. If we edit `main.py` and rerun `jj show`, we see this:

```bash
$ jj show
Commit ID: 66b59f20c534705cfb753c722222290e9185b22c
Change ID: kpqxywonksrltlrkyqukqqsxpltznztk
Author   : Ethan Lowman <ethan@example.com> (2026-11-15 12:26:43)
Committer: Ethan Lowman <ethan@example.com> (2026-11-15 12:26:43)

    (no description set)

diff --git a/main.py b/main.py
new file mode 100644
index 0000000000..c74cd2e0e0
--- /dev/null
+++ b/main.py
@@ -0,0 +1,2 @@ def my_new_feature():
+def my_new_feature():
+  pass
```

```bash
$ jj log
@  kpqxywon ethan@example.com 2026-11-15 12:26:43 66b59f20
│  (no description set)
◆  royxmykx ethan@example.com 2026-11-15 12:10:46 main 2bea5e3d
│  Adjust config
~
```

You’ll notice several things have changed. The working copy has been automatically committed, and `@` is no longer marked `(empty)`. The new commit ID for `@` is `66b59f20`, since we’ve added changes, but the change ID for `66b59f20` is still `kpqxywon`. `jj` has no staging area like in `git`, so we don’t have to manually add files. When we run a `jj` command to inspect or modify repo state after editing files, `jj` automatically snapshots the working copy as a new physical commit for the relevant logical change.

Let’s give this change a description (also called a commit message). We can either run `jj desc`, which opens an editor, or provide it directly with `-m`.

```bash
$ jj desc -m "Implement the feature"
Working copy  (@) now at: kpqxywon 6b89ffb2 Implement the feature
Parent commit (@-)      : royxmykx 2bea5e3d main | Adjust config
```

Notice again that the commit ID has changed, since the hash covers the commit message, but the change ID has remained stable.

Now, let’s say we’re ready to use this new feature, and we want to organize that change in a separate commit. We can create a new commit with `jj new`:

```bash
$ jj new
Working copy  (@) now at: nlrtlrxv 6cca2add (empty) (no description set)
Parent commit (@-)      : kpqxywon 6b89ffb2 Implement the feature
```

```bash
$ jj log
@  nlrtlrxv ethan@example.com 2026-11-15 12:35:49 6cca2add
│  (empty) (no description set)
○  kpqxywon ethan@example.com 2026-11-15 12:33:23 6b89ffb2
│  Implement the feature
◆  royxmykx ethan@example.com 2026-11-15 12:10:46 main 2bea5e3d
│  Adjust config
~
```

We’ve created an empty change `nlrtlrxv` on top of `kpqxywon`. `jj status` reflects that we are editing this new revision which does not yet differ from its parent.

```bash
$ jj status
The working copy has no changes.
Working copy  (@) : nlrtlrxv 6cca2add (empty) (no description set)
Parent commit (@-): kpqxywon 6b89ffb2 Implement the feature
```

`jj commit` is a shorthand for this common `jj desc` and `jj new` combo. Let’s make another change to use our new feature, and commit it.

```bash
$ jj commit -m "Use the feature"
Working copy  (@) now at: qmkrwlvp 91067ad0 (empty) (no description set)
Parent commit (@-)      : nlrtlrxv e1b06678 Use the feature
```

```bash
$ jj log
@  qmkrwlvp ethan@example.com 2026-11-15 12:41:30 91067ad0
│  (empty) (no description set)
○  nlrtlrxv ethan@example.com 2026-11-15 12:41:30 e1b06678
│  Use the feature
○  kpqxywon ethan@example.com 2026-11-15 12:33:23 6b89ffb2
│  Implement the feature
◆  royxmykx ethan@example.com 2026-11-15 12:10:46 main 2bea5e3d
│  Adjust config
~
```

To inspect the commit we just created (`n`), we can no longer use the bare `jj show`, which defaults to showing `@`. We have to pass the revision we want to inspect:

```bash
$ jj show n
Commit ID: e1b066782aa1a565c1e5d38ef27a308aa58e7e4c
Change ID: nlrtlrxvuuskxprvtuznquqzqzzwvnzk
Author   : Ethan Lowman <ethan@example.com> (2026-11-15 12:41:30)
Committer: Ethan Lowman <ethan@example.com> (2026-11-15 12:41:30)

    Use the feature

diff --git a/main.py b/main.py
index c74cd2e0e0..0714ec450b 100644
--- a/main.py
+++ b/main.py
@@ -1,2 +1,4 @@ def my_new_feature():
 def my_new_feature():
   pass
+
+do_something(my_new_feature())
```

We can describe empty commits to record intent before starting work. Let’s create a new commit on top of `k` to remind ourselves that we need to document the feature.

```bash
$ jj new k -m "Document the feature"
Working copy  (@) now at: qwyusntz 46a2ffda (empty) Document the feature
Parent commit (@-)      : kpqxywon 6b89ffb2 Implement the feature
Added 0 files, modified 1 files, removed 0 files
```

A separate `jj desc` would also work just fine instead of the `-m` flag on `jj new`. Here’s the current commit graph:

```bash
$ jj log
@  qwyusntz ethan@example.com 2026-11-15 12:46:35 46a2ffda
│  (empty) Document the feature
│ ○  nlrtlrxv ethan@example.com 2026-11-15 12:41:30 e1b06678
├─┘  Use the feature
○  kpqxywon ethan@example.com 2026-11-15 12:33:23 6b89ffb2
│  Implement the feature
◆  royxmykx ethan@example.com 2026-11-15 12:10:46 main 2bea5e3d
│  Adjust config
~
```

Note how we’ve created branching commits without creating a named branch. This highlights a major difference between `jj`’s branchless model and `git`’s branch-centric model of organizing work streams. Instead of juggling your work-in-progress on branches, you work on a graph of commits. Once ready to share, you push branches to a Git forge from your commit graph (more on that later).

## [Revsets](https://ethanlowman.com/posts/jj-primer/#revsets)

A revset, an idea borrowed from Mercurial, is an expression that selects a set of revisions from the commit graph. Most `jj` commands accept revsets in their arguments. When we ran `jj new k`, the argument `k` was actually a trivial revset that resolved to the single change with ID `kpqxywon`. The `jj log` command has an `-r <REVSET>` flag that we can use to preview a revset. Some demonstrative examples follow.

Parent of `@`:

```bash
$ jj log -r @-
○  kpqxywon ethan@example.com 2026-11-15 12:33:23 6b89ffb2
│  Implement the feature
~
```

Grandchildren of `r`:

```bash
$ jj log -r r++
@  qwyusntz ethan@example.com 2026-11-15 12:51:45 5b55579a
│  Document the feature
~

○  nlrtlrxv ethan@example.com 2026-11-15 12:41:30 e1b06678
│  Use the feature
~
```

Commits between `r` and `@`, inclusive:

```bash
$ jj log -r r::@
@  qwyusntz ethan@example.com 2026-11-15 12:51:45 5b55579a
│  Document the feature
○  kpqxywon ethan@example.com 2026-11-15 12:33:23 6b89ffb2
│  Implement the feature
◆  royxmykx ethan@example.com 2026-11-15 12:10:46 main 2bea5e3d
│  Adjust config
~
```

Commit `k` and all descendants:

```bash
$ jj log -r k::
@  qwyusntz ethan@example.com 2026-11-15 12:51:45 5b55579a
│  Document the feature
│ ○  nlrtlrxv ethan@example.com 2026-11-15 12:41:30 e1b06678
├─┘  Use the feature
○  kpqxywon ethan@example.com 2026-11-15 12:33:23 6b89ffb2
│  Implement the feature
~
```

Commits authored by `ethan@example.com` that also have ID `k` or `q`:

```bash
$ jj log -r 'author("ethan@example.com") & (k | q)'
@  qwyusntz ethan@example.com 2026-11-15 12:51:45 5b55579a
│  Document the feature
○  kpqxywon ethan@example.com 2026-11-15 12:33:23 6b89ffb2
│  Implement the feature
~
```

I’ll defer to the official documentation for exhaustive detail. The [Revsets language](https://docs.jj-vcs.dev/latest/revsets/) specification is easily my most referenced `jj` documentation page, so I would recommend skimming it to see what’s possible.

## [Navigating the commit graph](https://ethanlowman.com/posts/jj-primer/#navigating-the-commit-graph)

Let’s say in the course of documenting our feature in `qwyusntz` we realized we made a mistake in the implementation in parent change `kpqxywon`. We don’t need to manually commit or stash changes to switch tasks. We simply switch our working copy to `kpqxywon` with `jj edit`:

```bash
$ jj edit k
Working copy  (@) now at: kpqxywon 6b89ffb2 Implement the feature
Parent commit (@-)      : royxmykx 2bea5e3d main | Adjust config
Added 0 files, modified 0 files, removed 1 files
```

Our working copy once again shows the changes from `kpqxywon`.

```bash
$ jj show
Commit ID: 6b89ffb2a250f622a8638145d65fe8c9bb308dbf
Change ID: kpqxywonksrltlrkyqukqqsxpltznztk
Author   : Ethan Lowman <ethan@example.com> (2026-11-15 12:26:43)
Committer: Ethan Lowman <ethan@example.com> (2026-11-15 12:33:23)

    Implement the feature

diff --git a/main.py b/main.py
new file mode 100644
index 0000000000..c74cd2e0e0
--- /dev/null
+++ b/main.py
@@ -0,0 +1,2 @@ def my_new_feature():
+def my_new_feature():
+  pass
```

`jj` hasn’t lost track of the changes above our working copy, including our most recent edits to `q`:

```bash
$ jj log
○  qwyusntz ethan@example.com 2026-11-15 12:51:45 5b55579a
│  Document the feature
│ ○  nlrtlrxv ethan@example.com 2026-11-15 12:41:30 e1b06678
├─┘  Use the feature
@  kpqxywon ethan@example.com 2026-11-15 12:33:23 6b89ffb2
│  Implement the feature
◆  royxmykx ethan@example.com 2026-11-15 12:10:46 main 2bea5e3d
│  Adjust config
~
```

Instead of switching commits with `jj edit k`, we could have run `jj edit @-`. The revset `@-` is evaluated before switching the working copy, so this moves to the parent commit. The built-in commands `jj next [--edit]` and `jj prev [--edit]` provide another way to move up and down a stack of commits.

### [Automatic rebasing](https://ethanlowman.com/posts/jj-primer/#automatic-rebasing)

If we edit files in the working copy, the descendant commits will be automatically rebased upon the next `jj` invocation. For example, if we edit `my_new_feature`’s implementation in `main.py` and then run `jj log`, we’ll see that changes `k`, `n`, and `q` all get new commit IDs. `jj` also tells us that it performed a rebase.

```bash
$ jj log
Rebased 2 descendant commits onto updated working copy.
○  qwyusntz ethan@example.com 2026-11-15 13:03:41 7d293f19
│  Document the feature
│ ○  nlrtlrxv ethan@example.com 2026-11-15 13:03:41 41a43a84
├─┘  Use the feature
@  kpqxywon ethan@example.com 2026-11-15 13:03:41 d9f35831
│  Implement the feature
◆  royxmykx ethan@example.com 2026-11-15 12:10:46 main 2bea5e3d
│  Adjust config
~
```

## [Handling conflicts](https://ethanlowman.com/posts/jj-primer/#handling-conflicts)

If the automatic rebase happened to cause merge conflicts, `jj` doesn’t require us to fix them immediately, like `git rebase` would. Instead, `jj` records that the commit is conflicted and lets you carry on with your current task. This is massively helpful to reduce mental context switching, as your stacked commits grow more complex.

```bash
$ jj log
Rebased 2 descendant commits onto updated working copy.
○  qwyusntz ethan@example.com 2026-11-15 13:05:35 b96f8df4
│  Document the feature
│ ×  nlrtlrxv ethan@example.com 2026-11-15 13:05:35 e562bff4 (conflict)
├─┘  Use the feature
@  kpqxywon ethan@example.com 2026-11-15 13:05:35 c501c0bd
│  Implement the feature
◆  royxmykx ethan@example.com 2026-11-15 12:10:46 main 2bea5e3d
│  Adjust config
~
```

To view the conflicts, we can edit the conflicted commit…

```bash
$ jj edit n
Working copy  (@) now at: nlrtlrxv e562bff4 (conflict) Use the feature
Parent commit (@-)      : kpqxywon c501c0bd Implement the feature
Added 0 files, modified 1 files, removed 0 files
Warning: There are unresolved conflicts at these paths:
main.py    2-sided conflict
```

And inspect `main.py`.

```
def my_new_feature():
  print("Hello from the new feature!")
<<<<<<< conflict 1 of 1
%%%%%%% diff from: kpqxywon d9f35831 "Implement the feature" (parents of rebased revision)
\\\\\\\        to: kpqxywon c501c0bd "Implement the feature" (rebase destination)
-  pass
+  return 42
+++++++ nlrtlrxv 41a43a84 "Use the feature" (rebased revision)
  pass

do_something(my_new_feature())
>>>>>>> conflict 1 of 1 ends

```

In a text editor, we can resolve these conflicts, and the next time we run any `jj` command, we’ll see that `jj` has marked the commit as unconflicted:

```bash
$ jj status
Working copy changes:
M main.py
Working copy  (@) : nlrtlrxv 34f2e2a1 Use the feature
Parent commit (@-): kpqxywon c501c0bd Implement the feature
```

## [Undo and the op log](https://ethanlowman.com/posts/jj-primer/#undo-and-the-op-log)

Let’s say we made a mistake in that conflict resolution, and we want to undo it. In `git`, undoing an action, if it’s even possible, is often complicated. In `jj`, we can always run `jj undo`:

```bash
$ jj undo
Undid operation: 35beb0741f17 (2026-11-15 13:08:39) snapshot working copy
Restored to operation: f134ca987b59 (2026-11-15 13:07:18) edit commit e562bff4876f3ea76248c3f730e9e74d3e28e8a0
Working copy  (@) now at: nlrtlrxv e562bff4 (conflict) Use the feature
Parent commit (@-)      : kpqxywon c501c0bd Implement the feature
Added 0 files, modified 1 files, removed 0 files
Warning: There are unresolved conflicts at these paths:
main.py    2-sided conflict
New conflicts appeared in 1 commits:
  nlrtlrxv e562bff4 (conflict) Use the feature
Hint: To resolve the conflicts, start by creating a commit on top of
the conflicted commit:
  jj new nlrtlrxv
Then use `jj resolve`, or edit the conflict markers in the file directly.
Once the conflicts are resolved, you can inspect the result with `jj diff`.
Then run `jj squash` to move the resolution into the conflicted commit.
```

More generally, `jj` maintains a history of all actions in `jj op log`. We can inspect this log and `jj op restore` to a point in that history, or `jj op revert` an individual operation. If the `jj undo` itself was a mistake, we can `jj redo`.

The `Hint` in the output above is a bit of a spoiler for our next topic, the squash workflow.

## [Squash workflow](https://ethanlowman.com/posts/jj-primer/#squash-workflow)

Editing commits that have descendants can sometimes feel sketchy. If you’re iterating on a large diff that has left your working memory, it would be nice to separate your new changes from the existing diff. For example, we might like to inspect the diff that resolves a rebase conflict in isolation.

Let’s follow the instructions from the above output. We first create a new commit on top of the conflicted commit:

```bash
$ jj new n
Working copy  (@) now at: wqxolloz b878878a (conflict) (empty) (no description set)
Parent commit (@-)      : nlrtlrxv e562bff4 (conflict) Use the feature
Warning: There are unresolved conflicts at these paths:
main.py    2-sided conflict
```

```bash
$ jj log -r n::
@  wqxolloz ethan@example.com 2026-11-15 13:11:44 b878878a (conflict)
│  (empty) (no description set)
×  nlrtlrxv ethan@example.com 2026-11-15 13:05:35 e562bff4 (conflict)
│  Use the feature
~
```

Now we can resolve the conflicts in `main.py` in our editor, and rerun `jj log`:

```bash
$ jj log -r n::
@  wqxolloz ethan@example.com 2026-11-15 13:14:12 91181aec
│  (no description set)
×  nlrtlrxv ethan@example.com 2026-11-15 13:05:35 e562bff4 (conflict)
│  Use the feature
~
```

We can inspect the diff in `@` if we like, and when we’re happy with the conflict resolution, we can squash it into the parent commit:

```bash
$ jj squash
Working copy  (@) now at: vkywoywq f6b548b3 (empty) (no description set)
Parent commit (@-)      : nlrtlrxv 7208deb7 Use the feature
Existing conflicts were resolved or abandoned from 1 commits.
```

```bash
$ jj log -r n::
@  vkywoywq ethan@example.com 2026-11-15 13:15:39 f6b548b3
│  (empty) (no description set)
○  nlrtlrxv ethan@example.com 2026-11-15 13:15:39 7208deb7
│  Use the feature
~
```

The conflict is now resolved, and we can continue our work.

## [More commit graph operations](https://ethanlowman.com/posts/jj-primer/#more-commit-graph-operations)

One of the powerful qualities of `jj` is that it makes diffs malleable like clay. With automatic rebase and the confidence inspired by `jj undo`, we can move, combine, drop, and split diffs with ease.

Let’s try a few operations on our current commit graph. Here’s the starting point.

```bash
$ jj log
@  vkywoywq ethan@example.com 2026-11-15 13:15:39 f6b548b3
│  (empty) (no description set)
○  nlrtlrxv ethan@example.com 2026-11-15 13:15:39 7208deb7
│  Use the feature
│ ○  qwyusntz ethan@example.com 2026-11-15 13:05:35 b96f8df4
├─┘  Document the feature
○  kpqxywon ethan@example.com 2026-11-15 13:05:35 c501c0bd
│  Implement the feature
◆  royxmykx ethan@example.com 2026-11-15 12:10:46 main 2bea5e3d
│  Adjust config
~
```

### [Rebase](https://ethanlowman.com/posts/jj-primer/#rebase)

Let’s use `jj rebase` to coerce our branching commit graph into a linear history. Let’s say the documentation commit (`q`) should go between the implementation (`k`) and the usage (`n`). We could accomplish this by moving `nlrtlrxv` (and its empty descendant) on top of `qwyusntz`:

```bash
$ jj rebase -s n -o q
Rebased 2 commits to destination.
Working copy  (@) now at: vkywoywq 0405560c (empty) (no description set)
Parent commit (@-)      : nlrtlrxv 9920235b Use the feature
Added 1 files, modified 0 files, removed 0 files
```

```bash
$ jj log
@  vkywoywq ethan@example.com 2026-11-15 13:20:18 0405560c
│  (empty) (no description set)
○  nlrtlrxv ethan@example.com 2026-11-15 13:20:18 9920235b
│  Use the feature
○  qwyusntz ethan@example.com 2026-11-15 13:05:35 b96f8df4
│  Document the feature
○  kpqxywon ethan@example.com 2026-11-15 13:05:35 c501c0bd
│  Implement the feature
◆  royxmykx ethan@example.com 2026-11-15 12:10:46 main 2bea5e3d
│  Adjust config
~
```

Another way to accomplish the same effect would be to use `jj rebase -r q --before n`. Many `jj` commands have multiple ways to specify revsets. `jj rebase --help` documents these well.

Since arguments to `jj rebase` flags are all revsets, we can write a revset that selects a sequence of commits or commit subgraph, and move it elsewhere in the graph.

If we wanted to insert a commit below `k`, the most direct way is `jj new --before k`, but for demonstration purposes, we could alternatively create a new commit on top of `r` and rebase `k::` on top:

```bash
$ jj new r -m "Prepare for the feature"
Working copy  (@) now at: wxzmtyol be5ca29b (empty) Prepare for the feature
Parent commit (@-)      : royxmykx 2bea5e3d main | Adjust config
Added 0 files, modified 0 files, removed 2 files
```

```bash
$ jj log
@  wxzmtyol ethan@example.com 2026-11-15 13:23:29 be5ca29b
│  (empty) Prepare for the feature
│ ○  nlrtlrxv ethan@example.com 2026-11-15 13:20:18 9920235b
│ │  Use the feature
│ ○  qwyusntz ethan@example.com 2026-11-15 13:05:35 b96f8df4
│ │  Document the feature
│ ○  kpqxywon ethan@example.com 2026-11-15 13:05:35 c501c0bd
├─┘  Implement the feature
◆  royxmykx ethan@example.com 2026-11-15 12:10:46 main 2bea5e3d
│  Adjust config
~
```

```bash
$ jj rebase -s k -o w
Rebased 3 commits to destination.
```

```bash
$ jj log
○  nlrtlrxv ethan@example.com 2026-11-15 13:28:02 cda0de88
│  Use the feature
○  qwyusntz ethan@example.com 2026-11-15 13:28:02 ec9de16f
│  Document the feature
○  kpqxywon ethan@example.com 2026-11-15 13:28:02 722e7268
│  Implement the feature
@  wxzmtyol ethan@example.com 2026-11-15 13:23:29 be5ca29b
│  (empty) Prepare for the feature
◆  royxmykx ethan@example.com 2026-11-15 12:10:46 main 2bea5e3d
│  Adjust config
~
```

If you enjoy using TUIs, `jj arrange` provides a commit graph editor similar to `git rebase -i` for reordering commits.

### [Squash](https://ethanlowman.com/posts/jj-primer/#squash)

If we decided documentation should be in the same commit as implementation, we can squash:

```bash
$ jj squash --from q --into k
Rebased 1 descendant commits.
```

`jj` will open an editor and give us a chance to write a combined commit message.

```bash
$ jj log
○  nlrtlrxv ethan@example.com 2026-11-15 13:31:30 cb468825
│  Use the feature
○  kpqxywon ethan@example.com 2026-11-15 13:31:30 b93e4ef7
│  Implement and document the feature
@  wxzmtyol ethan@example.com 2026-11-15 13:23:29 be5ca29b
│  (empty) Prepare for the feature
◆  royxmykx ethan@example.com 2026-11-15 12:10:46 main 2bea5e3d
│  Adjust config
~
```

### [Abandon](https://ethanlowman.com/posts/jj-primer/#abandon)

To drop a commit from the graph, we can use `jj abandon`. Let’s drop the preparation commit:

```bash
$ jj abandon w
Abandoned 1 commits:
  wxzmtyol be5ca29b (empty) Prepare for the feature
Rebased 2 descendant commits onto parents of abandoned commits.
Working copy  (@) now at: xzwrrwyv d460fbd4 (empty) (no description set)
Parent commit (@-)      : royxmykx 2bea5e3d main | Adjust config
```

```bash
$ jj log
@  xzwrrwyv ethan@example.com 2026-11-15 13:33:49 d460fbd4
│  (empty) (no description set)
│ ○  nlrtlrxv ethan@example.com 2026-11-15 13:33:49 b1284f61
│ │  Use the feature
│ ○  kpqxywon ethan@example.com 2026-11-15 13:33:49 df93f0a6
├─┘  Implement and document the feature
◆  royxmykx ethan@example.com 2026-11-15 12:10:46 main 2bea5e3d
│  Adjust config
~
```

To abandon parts of a commit (hunks or changes to whole files), we can use the `jj diffedit` TUI (see [docs](https://docs.jj-vcs.dev/latest/cli-reference/#jj-diffedit)).

### [Split](https://ethanlowman.com/posts/jj-primer/#split)

The `jj split` TUI allows us to take a commit and split it into two consecutive commits by selecting which files or hunks should go in the first. Let’s use it to split up implementation and documentation again with `jj split -r k`:

The [jj-hunk](https://github.com/laulauland/jj-hunk) project provides a means to automate commit splitting. If you are so inclined, agents are quite good at using this tool to split up commits into a sequence of smaller changes for easier review.

## [Bookmarks and Git branches](https://ethanlowman.com/posts/jj-primer/#bookmarks-and-git-branches)

`jj` does not have a native concept of a branch, but it does have a primitive called bookmarks that map to Git branches. When we need to manipulate Git branches, we do it by way of bookmarks.

You can go pretty far using Git without realizing this, but a Git branch is just a pointer to a commit. `jj` explicitly models bookmarks this way. `jj` bookmarks are essentially a named label attached to a commit that we can move around as we modify our commit graph, and push to/pull from a remote.

We’ll discuss a few basic workflows around Git branches below, but you can find a more complete guide in the `jj` docs on [Working with GitHub](https://docs.jj-vcs.dev/latest/github/).

### [Pushing a new branch](https://ethanlowman.com/posts/jj-primer/#pushing-a-new-branch)

Assuming we initialized `jj` in a git repository that has remotes, we should have some output for `jj git remote list`.

```bash
$ jj git remote list
origin git@github.com:ethanal/example.git
```

Since `jj` is branchless, the commits we’ve created locally don’t have a branch name. Luckily, `jj` doesn’t require us to pick one, and generates one for us. Generated branch names free you from thinking of a unique branch name so you can instead focus on writing a good commit message (usually the more durable descriptor of a change).

To create a generated bookmark for change ID `nlrtlrxv` and push up a corresponding Git branch, we can run a command like the following:

```bash
$ jj git push -c n
Creating bookmark push-nlrtlrxvuusk for revision nlrtlrxv
Changes to push to origin:
  bookmark: push-nlrtlrxvuusk [add to b1284f61]
git: Enumerating objects: 17, done.
git: Counting objects: 100% (17/17), done.
git: Delta compression using up to 32 threads
git: Compressing objects: 100% (14/14), done.
git: Writing objects: 100% (16/16), 25.21 KiB | 6.30 MiB/s, done.
git: Total 16 (delta 1), reused 5 (delta 0), pack-reused 0 (from 0)
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
remote:
remote: Create a pull request for 'push-nlrtlrxvuusk' on GitHub by visiting:
remote:      https://github.com/ethanal/example/pull/new/push-nlrtlrxvuusk
remote:
```

Now `jj log` shows our change with a correspondingly named bookmark `push-nlrtlrxvuusk` associated:

```bash
$ jj log -r n
○  nlrtlrxv ethan@example.com 2026-11-15 13:33:49 push-nlrtlrxvuusk b1284f61
│  Use the feature
~
```

We’re free to create a PR from this branch on GitHub now. You can customize the bookmark naming scheme with the `git_push_bookmark` template config documented [here](https://docs.jj-vcs.dev/latest/config/#generated-bookmark-names-on-push). It’s a common pattern to format branch names like `<author name>/<change ID>`.

Since `jj git push -c` creates a branch name that corresponds to a change ID, it seems to encourage one evolving change per PR. I think it’s usually best to lean into these ergonomics, and have one change per PR.

### [Updating a branch](https://ethanlowman.com/posts/jj-primer/#updating-a-branch)

To update a branch, the general process is to either squash commits into a change that is already bookmarked, or create new commits and move the bookmark forward.

#### [Bookmark squash flow](https://ethanlowman.com/posts/jj-primer/#bookmark-squash-flow)

If you choose to squash commits into an existing bookmark (recommended to keep one change per PR), the flow is as follows.

First, create a new commit on top of our branch.

```bash
$ jj new push-nlrtlrxvuusk
Working copy  (@) now at: stzvpxow f14a0cf7 (empty) (no description set)
Parent commit (@-)      : nlrtlrxv b1284f61 push-nlrtlrxvuusk | Use the feature
Added 2 files, modified 0 files, removed 0 files
```

Then make our edits.

```bash
$ jj status
Working copy changes:
M README.md
Working copy  (@) : stzvpxow 6bdd43dc (no description set)
Parent commit (@-): nlrtlrxv b1284f61 push-nlrtlrxvuusk | Use the feature
```

Squash them into the bookmark branch.

```bash
$ jj squash --from @ --into push-nlrtlrxvuusk
Working copy  (@) now at: xryuuprx 04d65a03 (empty) (no description set)
Parent commit (@-)      : nlrtlrxv aeb869ae push-nlrtlrxvuusk* | Use the feature
```

We now see `push-nlrtlrxvuusk` has an asterisk next to it, indicating it has changes that haven’t been pushed to the remote. So let’s push them.

```bash
$ jj git push -b push-nlrtlrxvuusk
Changes to push to origin:
  bookmark: push-nlrtlrxvuusk [move sideways from b1284f61c417 to aeb869aef99b]
```

The remote branch now reflects the new changes. The above output says that we moved the branch sideways from one commit to another commit, which reflects that we did a force-push. `jj git push` runs the equivalent of a `git push --force-with-lease`, so if you’re ever unclear about what a push operation would do, you can use the `jj git push --dry-run` flag. `jj` protects you against most dangerous force pushes refusing to rewrite a configurable revset of immutable commits. The default immutable revset is sensible, but you can make it more or less conservative by [configuring your own immutable heads](https://docs.jj-vcs.dev/latest/config/#set-of-immutable-commits).

#### [Bookmark advance flow](https://ethanlowman.com/posts/jj-primer/#bookmark-advance-flow)

In this flow, we add new commits on top of our bookmark and then move the bookmark to point to our new commit.

Let’s first prepare the new commit.

```bash
$ jj new push-nlrtlrxvuusk
Working copy  (@) now at: pptrztmx 6e49ff1f (empty) (no description set)
Parent commit (@-)      : nlrtlrxv aeb869ae push-nlrtlrxvuusk | Use the feature
```

Then make our edits.

```bash
$ jj status
Working copy changes:
M README.md
Working copy  (@) : pptrztmx 89027165 (no description set)
Parent commit (@-): nlrtlrxv aeb869ae push-nlrtlrxvuusk | Use the feature
```

```bash
$ jj commit -m "Add a usage example"
Working copy  (@) now at: wunttkrp 044f2d98 (empty) (no description set)
Parent commit (@-)      : pptrztmx 2341ee4a Add a usage example
```

The bookmark does not automatically move to our new commit. This is the biggest difference between bookmarks and Git branches. For now `push-nlrtlrxvuusk` still points at the commit below:

```bash
$ jj log -r n::
@  wunttkrp ethan@example.com 2026-11-15 13:51:57 044f2d98
│  (empty) (no description set)
○  pptrztmx ethan@example.com 2026-11-15 13:51:57 2341ee4a
│  Add a usage example
○  nlrtlrxv ethan@example.com 2026-11-15 13:43:16 push-nlrtlrxvuusk aeb869ae
│  Use the feature
~
```

There are several ways to move the bookmark to the new commit. The easiest is probably `jj bookmark advance`, which moves the closest bookmark to the given commit:

```bash
$ jj bookmark advance --to @-
Advanced 1 bookmarks to pptrztmx 2341ee4a push-nlrtlrxvuusk* | Add a usage example
```

Now we’re ready to push the moved bookmark.

```bash
$ jj git push -b push-nlrtlrxvuusk
Changes to push to origin:
  bookmark: push-nlrtlrxvuusk [move forward from aeb869aef99b to 2341ee4aa554]
```

### [Checking out a remote branch](https://ethanlowman.com/posts/jj-primer/#checking-out-a-remote-branch)

Since `jj` is branchless, strictly speaking, we don’t “check out” a branch, but we can create a commit on top of a remote branch. First do the equivalent of a `git fetch`, importing remote bookmarks:

```bash
$ jj git fetch
```

And create a new empty commit on top of the remote branch:

```bash
$ jj new some-cool-feature@origin
```

Our working copy now contains all the commits on the remote branch. However, if we were to drop back to `git`, we see that from `git`’s perspective, we’re in a [detached head](https://git-scm.com/docs/git-checkout#_detached_head) state, not on any branch.

```bash
$ git status
Not currently on any branch.
nothing to commit, working tree clean
```

This is because `jj` is editing a commit above a bookmark, and does not have an equivalent to `git`’s “current branch” state.

### [Pulling in changes from the trunk](https://ethanlowman.com/posts/jj-primer/#pulling-in-changes-from-the-trunk)

Syncing with the trunk (e.g. `main`) is just a specific example of this flow. We would `jj git fetch` to update our `main` to the upstream `main`, and then rebase our commits on top. For example, the following would rebase `my-branch` on top of a new `main`:

```
jj rebase -b my-branch -o main
```

## [Staying oriented in the graph](https://ethanlowman.com/posts/jj-primer/#staying-oriented-in-the-graph)

`jj` commit graph manipulation is easiest when you have a live commit graph on screen. There are different ways you can accomplish this.

[jjui](https://github.com/idursun/jjui) is a TUI for `jj` that allows you to edit the visual graph directly. I have not personally used this, but I know people who like it.

I prefer to have an always-live `jj log` in the corner of my screen by way of [viddy](https://github.com/sachaos/viddy) (a nicer version of `watch` that supports color):

```bash
viddy -t -n1 jj log --ignore-working-copy --color=always
```

I recommend filtering down this log to a revset that represents your current work. Here’s a starting point:

```
visible() & (trunk() | (tracked_remote_bookmarks() & ~::trunk()) | (mutable() & mine())::)
```

You might also consider trying my experimental project [jj-stamp](https://github.com/ethanal/jj-stamp), which is a local code review GUI for the squash workflow. Even if you don’t use the squash review workflow, it makes a pretty good diff viewer with a live `jj log` on the side and a terminal for running `jj` operations.

## [Stacked PRs, in brief](https://ethanlowman.com/posts/jj-primer/#stacked-prs-in-brief)

One of the biggest productivity gains from `jj` comes in its ability to support stacked PR workflows. This merits its own post, so I’ll keep it short.

Large changes are easier to understand when they’re broken up into a series of logical steps. Breaking up a large change into smaller sequential atoms of review makes the whole change easier to understand. Stacked PRs attempt to leverage this fact to optimize merge throughput, while simultaneously removing pipeline stalls in the review process.

Let’s say we’re building a new feature. We can decide we want to split up implementation into three stages:

1. Refactor to make the feature easier to implement
2. Implement the feature
3. Use the feature in another system

`jj`’s ability to work on stacked changes with automatic rebase is a natural fit for implementing these three stages. Each of the three stages maps to a single commit, each with an associated bookmark/branch/PR:

```bash
$ jj log -r oomyzqmz::sxummoow
○  sxummoow ethan@example.com 2026-11-15 14:10:48 ethan/sxummoow b5f0af21
│  Use the feature
○  vqpzktsp ethan@example.com 2026-11-15 14:06:27 ethan/vqpzktsp fe52c25f
│  Implement the feature
○  oomyzqmz ethan@example.com 2026-11-15 14:01:19 ethan/oomyzqmz 07f69717
│  Refactor for the feature
~
```

After you implement the first change, you can put up a PR for review and immediately start working on the next change. Your work stream becomes a long queue where you pop off the bottom as PRs are ready to merge and push to the top, with no pipeline stalls.

It’s pretty easy to manually set up small PR stacks with `jj` and GitHub, but once you get hooked on the technique (making stacks 5–10 PRs deep), you’ll want a tool. `jj-pancake` is my entry into the ring of stacked PR tools. More on that soon.

## [Resources](https://ethanlowman.com/posts/jj-primer/#resources)

- [Official Jujutsu docs](https://docs.jj-vcs.dev/). I most often refer to the [revset spec](https://docs.jj-vcs.dev/latest/revsets/).
- [Steve Klabnik’s `jj` tutorial](https://steveklabnik.github.io/jujutsu-tutorial/) was my first introduction to `jj`. I don’t think it’s maintained as a durable resource (and I don’t promise mine will be!), but it’s still a good read.
- I follow the `jj` releases [RSS feed](https://github.com/jj-vcs/jj/releases/). Lately the releases have been packed with new features. Overall `jj` is quite stable, but the occasional bug is fixed promptly.
- The [Jujutsu discord](https://discord.gg/dkmfj3aGQN) is active and friendly!
- The [blog of East River Source Control](https://ersc.io/blog), a company that is building a new forge and code review platform intended to support Jujutsu well. The creator of Jujutsu, Martin von Zweigbergk, has joined as the CTO, and they have lots of other smart people as well. Perhaps a successor to GitHub one day? I’m rooting for them.
- [My personal `jj` config](https://github.com/ethanal/dotfiles/blob/main/jj-config.toml). You might find some useful aliases or tricks here, but I haven’t attempted to make it readable.
- The [`jj` config of Austin Seipp](https://github.com/thoughtpolice/a/blob/canon/tilde/aseipp/dotfiles/jj/config.toml), a `jj` maintainer. Cool tricks lie within.
