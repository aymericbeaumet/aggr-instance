---
title: Apple's git still trusts the compromised eMudhra certificates
link: https://emudhra.we-have-a-problem.com/apple.html
source: hnrss-org
published: 2026-10-28T11:03:40Z
updated: 2026-10-28T11:03:40Z
first_seen: 2026-10-28T17:21:45.580541782Z
authors:
- galadran
content: extracted
html: 2026-10-28-apple-s-git-still-trusts-the-compromised-emudhra.html
---

[← Main Page](https://emudhra.we-have-a-problem.com/)

**28th October 2026**

Apple was notified of the eMudhra incident on 17th October. As of 28th October, the developer tools Apple ships with macOS, including git, curl and python3, still trust the compromised certificates.

Worse, macOS 27.2, released on 26th October, did not fix the issue and undid the workaround that users had applied by hand, leaving them vulnerable again.

Meanwhile, [dozens of other projects](https://emudhra.we-have-a-problem.com/#developers), many run by volunteers, shipped fixes days ago.

## [Why does this matter?](https://emudhra.we-have-a-problem.com/apple.html#why)

Anyone can intercept any HTTPS connection these tools make.

For git, that means clones and pulls from `https://` remotes can deliver attacker-chosen code, pushes can be altered in transit, and your credentials, such as GitHub tokens, can be intercepted.

To check whether your git is affected, run:

```
git ls-remote https://git-emudhra.we-have-a-problem.com/check.git
```

A certificate error means your git is protected. If git lists a commit, your git is affected. Your connections to GitHub, or any other `https://` git remote, can be intercepted and tampered with.

To see an attack end to end, run the command below in a temporary directory. It clones a repository from a test server impersonating GitHub. The `-c` option redirects `github.com` to our test server for this one command only. The server presents a `github.com` certificate issued by one of the compromised eMudhra intermediates.

```
git -c http.curloptResolve=github.com:443:15.235.19.186 clone https://fake-user@github.com/galadran/emudhra-git-test
```

A patched git will refuse with a certificate error. If the clone succeeds, you'll see a `README.md` starting with **INTERCEPTED**.

A real attacker would not need the extra option. Anyone on the network path between you and GitHub can redirect your traffic themselves.

You can replace the demo repository with any other, public or private. Keep the `fake-user@` prefix. Without it, git would send your GitHub credentials to the test server. For example:

```
git -c http.curloptResolve=github.com:443:15.235.19.186 clone https://fake-user@github.com/Homebrew/brew
```

curl, python3 and the other tools Apple ships are affected in the same way. To check curl, run:

```
curl https://dv-g3b-noct-emudhra.we-have-a-problem.com/
curl https://dv-g3b-aaa-noct-emudhra.we-have-a-problem.com/
```

A certificate error means curl is protected.

## [What should Apple do?](https://emudhra.we-have-a-problem.com/apple.html#ask)

Apple has already protected Safari and other apps built on its networking framework.

Apple needs to ship a fix for macOS developers as soon as possible, for example by removing the affected roots from `/etc/ssl/cert.pem`.

## [What can developers do in the meantime?](https://emudhra.we-have-a-problem.com/apple.html#workaround)

The affected roots can be removed by hand. The [macOS workaround](https://emudhra.we-have-a-problem.com/#macos-curl) on the main page walks through the steps. Apple's [guidance](https://support.apple.com/en-us/149555) states that macOS updates may revert the fix, so developers need to repeat the checks above and the workaround after every update until Apple ships a fix.

Security teams should also consider whether macOS runners are used in their CI or other build infrastructure and evaluate the supply chain risk.

* * *

Site published by [Dennis Jackson](https://github.com/dennisjackson). This site is the work of [many contributors](https://github.com/galadran/we-have-a-problem.com/graphs/contributors) from the community. The source is on [GitHub](https://github.com/galadran/we-have-a-problem.com) and PRs are welcome. Licensed under the [Apache License, Version 2.0](https://github.com/galadran/we-have-a-problem.com/blob/main/LICENSE).
