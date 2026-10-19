---
title: What's Wrong with GitHub
link: https://www.gnu.org/philosophy/whats-wrong-with-github.html
source: lobste-rs
published: 2026-10-11T09:37:56Z
updated: 2026-10-11T09:37:56Z
first_seen: 2026-10-13T03:09:55.529316228Z
authors:
- gnu.org via tusharhero
labels:
- vcs
summary: Comments
content: extracted
html: 2026-10-11-what-s-wrong-with-github.html
---

Software developers, the GNU Project urges you to avoid hosting your repositories on GitHub. This is to avoid being on the receiving end of harmful practices, and avoid leading others to be victims too. This article focuses on a few of GitHub's worst problems, and the wrongs and harm they can do: [(1)](https://www.gnu.org/philosophy/whats-wrong-with-github.html#bad-licensing) GitHub promotes vague and confusing licensing practices; [(2)](https://www.gnu.org/philosophy/whats-wrong-with-github.html#nonfree-js) it demands that visitors run nonfree JavaScript for most operations; [(3)](https://www.gnu.org/philosophy/whats-wrong-with-github.html#saass) some of the “services” it offers are actually SaaSS (Service as a Software Substitute) dis-services; [(4)](https://www.gnu.org/philosophy/whats-wrong-with-github.html#centralizing-git) it attempts to subvert the distributed architecture of Git.

While Git is free software, GitHub is not software at all. It is a website that provides a range of features, from a simple repository-hosting service to a variety of [SaaSS](https://www.gnu.org/philosophy/who-does-that-server-really-serve.html) dis-services. The sheer variety of features GitHub offers makes writing a fair critique of the site difficult, because each one calls for an evaluation on moral grounds—both for the job it does, and for the implementation of that job.

This article focuses on a few of GitHub's worst problems, beginning with an area where GitHub has shown at least some improvement over the years, mentioning the website's near-uselessness for users who refuse the [JavaScript Trap](https://www.gnu.org/philosophy/javascript-trap.html) and the SaaSS “AI” [^1] dis-services, and ending on GitHub's model of being a central hub in a distributed system.

GitHub seems to operate as much like a walled garden as it can without outright breaking compatibility with Git. Because Git is free software, GitHub cannot forcibly [tether](https://www.gnu.org/proprietary/proprietary-tethers.html) Git to itself, but it can bait users to adopt workflows that nonetheless tie the users to GitHub.

Sometimes the worst handcuffs are not iron, but silk. GitHub offers convenience in centralization, and this makes little frictions insidious: one-click merge only works from a pull request, which can only be made from a “fork” hosted on GitHub; links to GitHub accounts replace email addresses in the web UI; continuous integration runs on GitHub servers by default. Little by little, the Internet-spanning plains of Git become the walled garden of GitHub.

The full solution to the problems caused by hosting your package on GitHub is to host your package on a site that [doesn't have those problems](https://www.gnu.org/software/repo-criteria-evaluation.html). But if you can't do that right away, at least publish an email address in your README so people who don't use GitHub can communicate with you about your program, host the web pages about using your program on a website that doesn't require nonfree software to read those pages, and always refer to the web pages there.

### Promotion of Vague and Unclear Licensing Practices

When GitHub first began offering repository hosting in early 2008, it was completely silent on the matter of properly licensing free software, and this silence led to a proliferation of public repositories with no declared license whatsoever. In 2013, GitHub set up a “[Choose a License](https://choosealicense.com/)” site to help users select free licenses for new software.

As of this writing in 2026, that site gives ill-advised advice for the application of the GPL, stating that merely copying the text of the license into a file in a repository is sufficient.

The FSF's lawyers recommend to us, and in turn we recommend to developers, to [put a license notice in each substantive file](https://www.gnu.org/licenses/gpl-howto.html#why-license-notices), to ensure that users are aware of their rights and obligations under the GPL. GitHub mentions this only as an “optional step.”

Without the standard GPL notice, there is nothing to say whether the program is licensed under GPL version N only, or under [GPL version N-or-later](https://www.gnu.org/licenses/identify-licenses-clearly.html). GitHub mentions this only in an aside about SPDX license identifiers.

### Extensive Use of Nonfree JavaScript

GitHub's website sends lots of JavaScript code to the browser, and much of that code is nonfree. Because the site fails to gracefully degrade, users who refuse the [JavaScript Trap](https://www.gnu.org/philosophy/javascript-trap.html) find that any interaction beyond simply viewing repositories is impossible, and even many of the file-viewing links no longer work.

There are workarounds for this. The standard free Git command-line tools can clone repositories from GitHub, and there is a public REST API and free command-line tool that can handle at least some interaction with the site.

However, the command-line tool GitHub provides for that REST API, while technically free software under [Expat license terms](https://www.gnu.org/licenses/license-list.html#Expat), nonetheless cannot be recommended because GitHub has decided to add spyware features in the form of “telemetry” to it. This is exceptionally ridiculous, because the tool's main purpose is to use a web-based API; GitHub could obtain usage information from their own server logs rather than from users' computers.

### GitHub Copilot is SaaSS

GitHub started a controversy with the introduction of “GitHub Copilot” that has only grown with the capabilities of the tool. GitHub Copilot began as an “AI” [^1] code-completion tool, with an early demonstration of producing the body of a function given only its header comment, and has grown into an “AI” “agent” claimed to be able to function as a development team member.

Copilot has always been controversial. The initial code-completion tool used a model that had been trained on a wide variety of code, under a wide variety of licenses, and was quickly found to be able to reproduce lengthy sections of well-known text verbatim, including the preamble of the GNU General Public License and the Zen of Python. This ability to regurgitate its training material has led to doubts about the legal status of code suggested by Copilot. GitHub offers indemnification to higher-tier Copilot subscribers, although it remains unclear how indemnity would work if the demand were to be GPL compliance rather than money.

Later expansions of Copilot's capabilities have brought more controversy. GitHub added a “feature” allowing users to have Copilot write a bug report, which is then submitted as an issue with minimal indication that Copilot was used to generate it. These Copilot-assisted issues have been drowning maintainers of projects on GitHub in slop when users fail to proofread their issues and correct Copilot's errors. GitHub, of course, refuses to allow maintainers to block Copilot-generated issues from being submitted.

The legal risks of using LLM code suggestions and the social harms of burying maintainers in slop are universal, applying even to free models run locally, but what GitHub does is bad in another way: Copilot is [SaaSS](https://www.gnu.org/philosophy/who-does-that-server-really-serve.html), and its models run on remote servers where users have no control over them. This has produced some of the usual SaaSS abuses: forced “upgrades” that users find to be inferior to the previous versions, and price increases at the whim of the vendor.

Copilot subscriptions originally provided access to basic features (mostly the code suggestions) and a certain number of “premium requests” (other interactions and more complex agentic tasks) each month. In early 2026, GitHub announced a policy change: all subscriptions would be forced to move to a token-based billing model that charges for the actual computing resources used, which is only loosely under the user's control. GitHub could impose this change on users because Copilot is SaaSS.

### Centralizing Git

The above practices are directly inimical to software freedom, but GitHub also undermines the community in deeper, long-term ways that are less immediately evident.

Git was designed as a distributed version-control system, with no central points-of-failure anywhere in its architecture. This decision was in part simply making Git as different as possible from CVS, in part a reflection of the bazaar development process around the kernel Linux, and possibly in part a lesson learned from the [BitKeeper fiasco](https://www.gnu.org/philosophy/mcvoy.html) [^2]. GitHub seeks to reintroduce elements of centralization that Torvalds explicitly rejected when designing Git, and then encourages users to build workflows around that centralization.

#### GitHub “Achievements”

GitHub has a system of profile badges that can be earned by various activities on the site, analogous to achievements in newer video games. In fact, they are called “achievements” in the documentation. This system is another example of gamification where software (or “services”) attempt to [addict users](https://www.gnu.org/proprietary/proprietary-addictions.html).

Of course, most of GitHub's “achievements” only recognize activities associated with the site, a nudge to increase users' dependency upon GitHub.

#### Mixed User/Organization Model

GitHub started with a simple premise of providing Git repository hosting. Aligned with this, GitHub organizes repositories under user accounts, and allows all users to make repositories. This is a simple bulletin-board model where users can freely publish their own versions of free packages, and submit changes upstream by asking package maintainers to pull from their copies. This is how Git was designed to work, and this early form of GitHub made that workflow accessible to users lacking the skills or resources to set up their own Git servers.

The problem with a bulletin-board model is that it can make locating a canonical version of a package needlessly difficult, in contrast to traditional forges organized around projects with designated maintainers. Designating a maintainer at a forge can be as anarchic as “whoever registers it first,” as carefully managed as the GNU Project, or anywhere in between.

This was not a problem for Linux because Linus' repository is widely known to be the central canonical reference in that project. Remember that Git was originally designed specifically to support developing the kernel Linux. Its usefulness elsewhere was a bonus.

There is also a problem inherent to Git's distributed model that GitHub makes worse: the bulletin-board model leads to a proliferation of forks, since even the simplest patch must be prepared by first cloning the repository, thereby creating a fork of the package. These forks are often unmaintained or even abandoned after their original purpose is served.

The opportunities for confusion led to a tendency to create “organization” accounts at GitHub, a practice now explicitly supported. However, for historical reasons, organizations and users share the same namespace at GitHub, and this leaves considerable room for confusion, made worse by…

#### GitHub's Centralized Fork/Pull-Request Workflow

While Git was originally designed to support this model of distributed development, GitHub insidiously inserts itself in the middle. Git allows the mirror from which a pull is drawn to be anywhere on the Internet, but GitHub only supports pulling from… GitHub. While it *is* possible to use Git to pull from elsewhere, perform a merge locally, and push to GitHub, GitHub carefully creates a path of least resistance entirely dependent on GitHub.

There is no technical reason why GitHub could not allow pull requests to reference remote Git servers, or even accept pull requests referencing remote servers by email, which is the traditional “Git way” of handling them. GitHub chooses to tie GitHub pull requests exclusively to itself.

GitHub further worsens these problems with its concept of “forks,” distinct from Git's basic feature of cloned repositories. GitHub displays the number of forks of a repository as a sort of popularity-contest badge, and implements forks in a way that renders repositories at GitHub questionably trustworthy, as material that appears only in a fork can nonetheless be retrieved using the parent repository's URL, enabling malicious parties to confuse users.

#### GitHub Actions

GitHub offers a modular automated workflow system called GitHub Actions, another way to make your package dependent on GitHub. GitHub Actions can be used to run arbitrary scripts in response to commits or pull requests. The scripts by default run on GitHub's virtual servers, although repository owners can specify to run their scripts on their own servers.

While repository owners can self-host runners for their own actions, the standard runner, despite being free software, nonetheless contains a universal backdoor in the form of an automatic update feature, and depends on SaaSS infrastructure controlled by GitHub to tell it what to run.

GitHub's action-dispatch infrastructure makes some attempt to enforce the universal backdoor by refusing to dispatch actions to runners that it considers to be running out-of-date software.

While the dispatch infrastructure that coordinates running actions is SaaSS, the actions themselves are not SaaSS because users control which actions are run. Even though some actions are nonfree, repository owners can simply refuse to attach nonfree actions to their repositories.

GitHub Actions can be used to build very complex automated workflows, ensnaring users deeper into GitHub's web, but there is a free tool available that can help users break out of this trap. Published at [nektosact.com](https://nektosact.com), the `act` program is free software that can run workflows using GitHub Actions locally on demand without depending on GitHub's infrastructure.

#### GitHub's Communications Vacuum

GitHub attempts to draw in and centralize as much of the community around a package as possible. While any software forge can be expected to offer trackers for bugs, issues, patches, etc., and to require an account at the site to participate in those web-based discussions, previous forges such as SourceForge and GNU Savannah also offer mailing lists to facilitate collaboration by email with minimal interaction with the forge itself.

Git was designed to support the Internet-spanning development project that maintains the kernel Linux. Therefore, Git needs a concept of user identity that also spans the Internet, and the Git developers found a simple solution: email addresses, including an optional name.

This was an obvious choice for another reason: development of the kernel Linux is primarily coordinated using mailing lists. That project has its own mailing list infrastructure, and Git was also originally designed to facilitate processing patches exchanged by email.

GitHub evidently disagrees with these design decisions, because it not only fails to provide mailing lists, but also absolutely refuses to display email addresses in its main web UI. If a commit has an email address associated with a GitHub account, a link to that account is shown, otherwise only a name is shown with no link at all.

One of the moral principles of the free software movement is that the barriers to users becoming developers themselves should be minimal and only technical (and those are acceptable only because barriers such as requiring knowledge of programming fundamentally cannot be eliminated). Obscuring email addresses as GitHub does has the effect of isolating users from existing developers. The requirement to create an account at GitHub to participate in discussions is also a subtle social barrier to users who do not yet think of themselves as developers.

### Footnotes

[^1]: The term “AI” is placed in quotes because it has become a marketing term for “[Artificial Intelligence](https://www.gnu.org/philosophy/words-to-avoid.html#ArtificialIntelligence)” using LLMs, and while those systems are certainly artificial, they are far from intelligent.

[^2]: BitKeeper was a nonfree version control system used from 2002 to 2005 for developing the kernel, Linux, that is used in the [GNU/Linux](https://www.gnu.org/gnu/linux-and-gnu.html) operating system.
