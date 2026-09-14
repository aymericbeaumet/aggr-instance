---
title: commit-rewriter 0.1
link: https://simonwillison.net/2026/Sep/14/commit-rewriter/
source: simonwillison-net
published: 2026-09-14T00:28:10Z
updated: 2026-09-14T00:28:10Z
first_seen: 2026-09-14T01:32:35.257899680Z
labels:
- ai-assisted-programming
- git
- projects
- python
summary: 'Release: commit-rewriter 0.1 I built this little web app the other day to help edit the commit messages for the Datasette security releases. The initial commits were full of coding agent cruft and references to issue IDs from our private repository, so they weren''t fit for publication. If you want to edit the commit messages for a repository you can run it like this: uvx commit-rewriter path/to/repo Omit the path if you are already in the directory for that repo. When you submit your edits the tool creates a timestamped branch of your current repo state - to allow you to revert if you need to - and then rewrites every commit from the first one you edited to the most recent. Tags: git, projects, python, ai-assisted-programming'
content: extracted
html: 2026-09-14-commit-rewriter-0-1.html
preview:
  file: 2026-09-14-commit-rewriter-0-1.preview-87743dedea7a.webp
  width: 256
  height: 192
  alt: Screenshot of the commit-rewriter web interface. A heading reads commit-rewriter above the repository path and current branch and commit hash, with a short description of the tool. A toolbar shows a pending edits count with Discard drafts and Rewrite commit messages buttons, followed by a search box
  color: '#f4f6f7'
images:
- source: https://static.simonwillison.net/static/2026/commit-rewriter.webp
  original:
    file: 2026-09-14-commit-rewriter-0-1.image-a2feb4c7ed85.webp
    width: 1024
    height: 768
  color: '#f8fafb'
---

I built this little web app the other day to help edit the commit messages for the [Datasette security releases](https://datasette.io/blog/2026/september-security-releases/). The initial commits were full of coding agent cruft and references to issue IDs from our private repository, so they weren't fit for publication.

If you want to edit the commit messages for a repository you can run it like this:

```
uvx commit-rewriter path/to/repo
```

Omit the path if you are already in the directory for that repo.

![Screenshot of the commit-rewriter web interface. A heading reads commit-rewriter above the repository path and current branch and commit hash, with a short description of the tool. A toolbar shows a pending edits count with Discard drafts and Rewrite commit messages buttons, followed by a search box for message, author, or hash and an Edited only checkbox. A left sidebar titled Navigate commits lists recent commit messages with their short hashes. The main panel shows a card for each commit with its hash, author and timestamp, an editable text area containing the commit message, and a View full formatted diff toggle.](https://static.simonwillison.net/static/2026/commit-rewriter.webp)

When you submit your edits the tool creates a timestamped branch of your current repo state - to allow you to revert if you need to - and then rewrites every commit from the first one you edited to the most recent.
