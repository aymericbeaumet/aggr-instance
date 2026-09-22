---
title: Looking forward to Git 2.56 – and 3.0
link: https://lwn.net/SubscriberLink/1094575/2385e98583715c2b/
source: hnrss-org-frontpage
published: 2026-09-21T23:16:51Z
updated: 2026-09-21T23:16:51Z
first_seen: 2026-09-22T09:53:19.444633104Z
authors:
- chmaynard
summary: 'Article URL: https://lwn.net/SubscriberLink/1094575/2385e98583715c2b/ Comments URL: https://news.ycombinator.com/item?id=49794736 Points: 127 # Comments: 54'
content: extracted
html: 2026-09-21-looking-forward-to-git-2-56-and-3-0.html
preview:
  file: 2026-09-21-looking-forward-to-git-2-56-and-3-0.preview-058f572d61e1.webp
  width: 70
  height: 81
  alt: LWN.net Logo
  color: '#948d76'
images:
- source: https://static.lwn.net/images/logo/barepenguin-70.webp
  original:
    file: 2026-09-21-looking-forward-to-git-2-56-and-3-0.image-d3c04bff3171.webp
    width: 70
    height: 81
  color: '#d6d7d8'
---

## \[LWN subscriber-only content\]

> ### Welcome to LWN.net
>
> The following subscription-only content has been made available to you by an LWN subscriber. Thousands of subscribers depend on LWN for the best news from the Linux and free software communities. If you enjoy this article, please consider [subscribing to LWN](https://lwn.net/subscribe/). Thank you for visiting LWN.net!

The [Git](https://git-scm.com/) source-code management system is at the core of development processes worldwide, so changes, especially incompatible changes, are of great interest to the developers involved. The Git 2.56 release, which can be expected around the end of September, is currently available in [release-candidate](https://lwn.net/ml/all/xmqqh5jpvzxo.fsf@gitster.g) form. It is not the most earth-shaking of releases, but the one that follows, which might be the long-awaited Git 3.0, may well be.

#### What's in Git 2.56

The Git 2.56 release contains something over 700 non-merge commits; it brings a number of nice improvements, but will not fundamentally change the Git experience for most users. One feature that offers some potential in that regard is the addition of the drop subcommand to the (still experimental) [git history](https://git-scm.com/docs/git-history) toolbox:

```
    $ git history drop commit-id
```

This command will cause the identified commit to be removed from the history of the current branch, replaying all commits that were added after it. It offers an easier way of removing an offending commit. Unfortunately, it still refuses to work if the history contains merge commits, making it unusable for many (or most) repositories.

The [git status](https://git-scm.com/docs/git-status) command will now suggest a [git pull](https://git-scm.com/docs/git-pull) command to update a branch that is behind the branch it tracks. It still, though, will say that a branch is "up to date" even if it lags behind an (unfetched) remote tracking branch.

The [git refs](https://git-scm.com/docs/git-refs) command exists to manipulate references at a low level; in 2.56, it has gained a number of new subcommands. Those commands, create, delete, update, and rename, perhaps surprisingly for Git, do exactly what their names suggest they would.

There are a number of minor usability tweaks. The new --delete-merged option to [git branch](https://git-scm.com/docs/git-branch) will remove local branches that have been merged into their remote tracking branches. An attempt to delete a branch with git branch -d will fail, with a useful message, if that branch is being used for bisection. Attempts to lock the configuration file will be retried on failures, avoiding annoyance when multiple commands try to modify the file at once. [git add](https://git-scm.com/docs/git-add) has a new --resolved option that only adds files with merge conflicts that have been resolved.

Beyond that, there is the usual long list of bug fixes, refactorings, and performance improvements. All told, 2.56 looks like a solid release, but it also shows the signs of a project that is holding back much of its more significant work for the future. That leads to the question of what comes next.

#### After 2.56?

In early September, Git maintainer Junio Hamano [asked the community](https://lwn.net/ml/all/xmqqmrtu50av.fsf@gitster.g) what the next release should be. Would it be best to put out the 3.0 release that the community has been working toward for some time, finishing the year on a high note? Or, instead, is there a need for one or more 2.x releases still before the 3.0 release can happen?

This is an important question, because 3.0 will contain a number of compatibility breaks that may make some users pause before upgrading. Of those, perhaps the most significant is the switch to using the SHA-256 hash function by default, rather than the SHA-1 hash that Git has used since the beginning. SHA-1 has long been deemed to be weak, which is worrisome for applications like Git. Hashes are used to identify every object (files, directory trees, commits) in the Git repository, and are used to verify the chain of commits leading to any given point. If SHA-1 can be broken, it can conceivably be used to modify the history of a repository in ways that are difficult or impossible to detect.

Git has long included defenses against the known SHA-1 attacks, and few people appear to be seriously worried about the potential for compromised repositories now. Still, it makes a lot of sense to move to a more secure hash function.

Non-experimental support for SHA-256 has existed in Git since [the 2.42 release](https://lwn.net/ml/git/xmqqr0nwp8mv.fsf@gitster.g/) in 2023, though some of the glue for interoperating with older repositories has taken longer. The biggest concern that has kept the Git developers from moving to SHA-256 by default for some time now has been support (or the lack thereof) at the major forge sites. GitLab has [had support](https://about.gitlab.com/blog/gitlab-now-supports-sha256-repositories/) since 2024, and Forgejo has support as well. The elephant that is still missing from this room, though, is GitHub; releasing a version of Git that creates GitHub-incompatible repositories is a worrisome prospect. It still is not clear when GitHub might add that support, but it is notable that brian m. carlson, a GitHub employee and a key developer behind the SHA-256 transition, [responded](https://lwn.net/ml/all/ap2tjx0z7kiFjDM9@fruit.crustytoothpaste.net) to Hamano's question by saying that news on that topic was coming, and that that having the next release be 3.0 might be the best choice.

That said, carlson has posted [one other change](https://lwn.net/ml/all/20260907195941.1024289-1-sandals@crustytoothpaste.net) that he would like to see added before 3.0 comes out. While Git has always managed hexadecimal numbers (specifically, object IDs) as lower-case strings, it has also accepted upper-case IDs. That leads to situations where two seemingly different IDs (f00f00 and F00F00, say) are actually the same. Seemingly, bugs and security vulnerabilities have arisen from this ambiguity. So carlson wants to change Git's behavior to only accept IDs in lower case. That is a change that, seemingly, should not cause problems for too many users, but there is almost certainly somebody somewhere who relies on that behavior.

Another significant change that has been waiting for the 3.0 release is the switch to the "reftable" mechanism. A [reference](https://git-scm.com/book/ms/v2/Git-Internals-Git-References) (more commonly "ref") in Git is an association between a name and an object in the repository; branches, tags, and remotes are all refs, for example. Git currently (by default) stores each ref as a file under .git/refs/ in the repository. If a repository contains a branch called foo, there will be a file, .git/refs/heads/foo, containing the ID of the commit at the head of that branch. There is also a mechanism to pull all of these refs together into a packed-refs file, which increases performance — to a point.

This mechanism works, but becomes increasingly inefficient as the number of refs grows. In some projects, the number of refs does indeed grow. According to [the project's reftable documentation](https://git-scm.com/docs/reftable), the Android repository contains over 800,000 refs. At that scale, looking up refs, or determining whether any refs pointing to a specific commit exist, can be an expensive operation. Slowing down development can be a yellow-card-level annoyance, leading to a desire to do something about the refs.

The [Git 2.45](https://lwn.net/ml/git/xmqq8r0ww0sj.fsf@gitster.g/) release added reftables as a more efficient way of storing refs. It is a binary file optimized for both space efficiency and quick access. Since then, it has been possible to create a repository that uses a reftable rather than the old file-based mechanism, but that has never been the default. Switching to reftable should have no visible consequences (other than better performance) for users of Git itself, but it can be a problem for users of other software packages that access Git repositories. In his email, carlson mentioned [libgit2](https://libgit2.org/) as a potential concern.

As it turns out, libgit2 will not be a blocker here; Patrick Steinhardt [let it be known](https://lwn.net/ml/all/ap50kgyenpRrsqln@pks.im) that he has added reftable support to libgit2, and that SHA-256 support had been enabled by default in August. So libgit2 should be ready for the Git 3.0 transition.

Finally, as mentioned by carlson, there is the Rust question. The project has [added some trial support](https://lwn.net/Articles/1042172/) for code written in Rust, but has hesitated to make Rust mandatory for the building of Git. That, too, is expected to change in the 3.0 release; after that, any platform that does not have a working Rust compiler will be unable to upgrade.

After listing these concerns, carlson suggested that they probably should not hold up the 3.0 release:

> I think anyone else who is not already extremely far along on SHA-256 (and reftable, for software working with local repositories) is likely not worth considering. JGit and Gitoxide were both informed that SHA-256 was coming in Git 3.0 at least a year ago, for instance. (I know because I did the informing.)
>
> Similarly, I am not aware of anyone who is seriously undertaking Rust support for platforms that do not already support it, so I don't think that should be a blocker, either.

That led to his conclusion that, most likely, designating the next release as 3.0 would be the best choice.

What will actually happen has not, yet, been announced publicly; that is waiting for Hamano to make a decision. As he put it: "this is not a popularity contest, nor is it even a democracy". In the nearly two weeks since his query was posted, though, there has been no strong opposition to moving into the 3.x era and leaving some of Git's early design decisions behind (though, naturally, repositories using SHA-1 and lacking reftables will continue to be fully supported). If the 3.0 release does not happen in 2026, it seems certain to show up shortly thereafter.
