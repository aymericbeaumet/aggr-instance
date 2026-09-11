---
title: Soft-deprecating re.match()
link: https://simonwillison.net/2026/Sep/11/soft-deprecating-re-match/
source: simonwillison-net
published: 2026-09-11T14:47:57Z
updated: 2026-09-11T14:47:57Z
first_seen: 2026-09-11T17:43:44.917817170Z
labels:
- python
- regular-expressions
summary: 'Soft-deprecating re.match() Python has a concept of soft deprecation, where APIs are marked as "should no longer be used to write new code" without any promise/threat to remove them in the future. Python 3.15 release manager Hugo van Kemenade describes how in the upcoming 3.15 release soft deprecation has come for the venerable but deeply confusing re.match() function. It''s now available with the much clearer alternative re.prefixmatch() name - reflecting how it anchors at the beginning of the string but not the end. Most of the time you probably want re.search() (match this pattern anywhere in the string) or re.fullmatch() (match the entire string) instead. Via Lobste.rs Tags: python, regular-expressions'
content: extracted
html: 2026-09-11-soft-deprecating-re-match.html
---

**[Soft-deprecating re.match()](https://hugovk.dev/blog/2026/soft-deprecating-re.match/)** ([via](https://lobste.rs/s/u7dr96/soft_deprecating_re_match "Lobste.rs")) Python has a concept of [soft deprecation](https://peps.python.org/pep-0387/#soft-deprecation), where APIs are marked as "should no longer be used to write new code" without any promise/threat to remove them in the future.

Python 3.15 release manager Hugo van Kemenade describes how in the upcoming 3.15 release soft deprecation has come for the venerable but deeply confusing `re.match()` function. It's now available with the much clearer alternative `re.prefixmatch()` name - reflecting how it anchors at the beginning of the string but not the end.

Most of the time you probably want `re.search()` (match this pattern anywhere in the string) or `re.fullmatch()` (match the entire string) instead.
