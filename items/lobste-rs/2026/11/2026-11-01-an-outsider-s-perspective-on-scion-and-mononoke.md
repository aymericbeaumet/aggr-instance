---
title: An outsider's perspective on Scion and Mononoke
link: https://typesanitizer.com/blog/scion.html
source: lobste-rs
published: 2026-11-01T15:22:36Z
updated: 2026-11-01T15:22:36Z
first_seen: 2026-11-02T06:43:52.954701233Z
authors:
- typesanitizer.com by typesanitizer
labels:
- vcs
summary: Comments
content: extracted
html: 2026-11-01-an-outsider-s-perspective-on-scion-and-mononoke.html
---

Recently, Anthropic wrote a blog post about their new [centralized version control system Scion](https://www.anthropic.com/research/redesigning-source-control). As someone working on the Source Control team at Databricks, where we have a huge monorepo and are exploring different options, I figured it might be useful to share my perspective here.

(Obligatory disclaimer: This post represents my own views, not my employer’s.)

I recommend reading Anthropic’s post before continuing to read this one. You may also want to pre-read Rain Paharia’s [Anthropic’s Scion is heavily inspired by Mononoke](https://sunshowers.io/posts/anthropic-mononoke/) and the follow-up [More on Anthropic’s Scion ripping off Mononoke](https://sunshowers.io/posts/anthropic-mononoke-2/).

## My history with version control

Before we get into the weeds, it might be helpful to you to know where I’m coming from. If you’re not interested in this, you can jump to the [Technical Commentary](https://typesanitizer.com/blog/scion.html#technical-commentary) section.

* * *

I have never worked at Facebook, but one of my all-time favorite tech talks is Durham Goode’s [Scaling Mercurial at Facebook](https://www.youtube.com/watch?v=gOVD-DrUpwQ) from Git Merge 2017. Goode was the longest-serving and most senior member on Facebook’s Source Control team. ([source](https://news.ycombinator.com/item?id=50106738))

The talk covers various interesting aspects for both the server side and the client side. For the server side, some of the topics include solving push contention with server-side rebasing (pushrebase), using scalable object storage for commits, moving various source control queries to the server, etc. For the client, there’s mention of the smartlog and other CLI commands for easier manipulation of history (compared to rebasing in Git).

However, more so than the technical aspects of the talk (which are interesting in their own right!), the main thing that has stayed with me throughout these years is the *deep care for users* that is reflected in Goode’s tone and words. It’s a bit difficult to convey this without the power of voice, You should watch the presentation! but here are some quotes which reflect this:

In the context of `hg smartlog`: `hg smartlog` gives you a focused view of *your* branches and how they relate to the development trunk.

> I know Git can produce this kind of UI if you know the incantation, but the critical thing here is not that it can be done; it’s that it becomes the default view of their source-control environment. That is where the power comes from: making it really intuitive for users to use source control and understand the concepts without reading a man page.

In the context of `hg absorb`: `hg absorb` auto-magically identifies the right commits in a stack to squash fixes into and rewrites history accordingly. This makes it easier to work with stacked changes.

> This is the type of UI that we need to be inventing for our users: ones that match their intent and do it to the best of their ability, without making the user understand everything they have to do. It’s a little bit magic, but it’s magic in a very clean way. I think a lot of user experiences could benefit from this approach.

On recovering from mistakes:

> If you want to undo an amend or undo parts of a commit in Mercurial, you just run unamend and uncommit. You don’t have to understand a reflog; you don’t have to get a PhD to understand reset anymore. It just works, and it gives users confidence that they can recover from mistakes. That’s one of the most important things we can do: provide user experiences that give them confidence in what they’re doing.

Near the end, summing things up:

> On the user experience side, we as a source control community need to do a better job of building UIs that are accessible, intuitive, and that enable novice users to be power users. Everything I just talked about is how we are trying to do that for our users at Facebook. If the Git community could do that, the massive number of Git users taking advantage of it has the potential to redefine productivity for an entire generation of users.

Over the years, the possibility of these workflows lingered in my mind as I continued working with Git. I tried building my own wrappers on top of the `git` CLI to do some of these things, but I couldn’t get the performance to be reasonable since everything shelled out to `git` all the time, and I didn’t understand Git’s internals well enough to do something better.

I ended up sticking with vanilla Git until Waleed Khan Waleed worked at Facebook on dev tools, and later did work at Twitter related to version control. created [git-branchless](https://github.com/arxanas/git-branchless), with support for a smartlog, in-memory rebases, undo and more. I eagerly adopted it. I also used Stephen Jung’s [git-absorb](https://github.com/tummychow/git-absorb) quite a bit. Even today, the initial bit of `git absorb`’s README cites the mention of `hg absorb` on a Mozilla mailing list as the key inspiration.

Finally, earlier this year (2026), I mostly switched to using [jj](https://www.jj-vcs.dev/latest/), after having bounced off of it a couple of times. jj was created by Martin von Zweigbergk, who previously contributed to Git and later joined Google and worked on their internal Mercurial client for the monorepo. Source: [Mercurial at Google (2023)](https://mercurial.paris/download/Mercurial%20at%20Google.pdf).

So even though I have almost exclusively worked with Git throughout my career, I am in a way indebted to the work on Mercurial by Goode and numerous other contributors.

* * *

I’ve talked a bit about my personal usage of version control tooling. Let me briefly describe work I’ve done related to version control.

From late 2021 to late 2025, I worked at Sourcegraph on code indexing and navigation. For the last two of those years, I worked more on the backend written in Go, where the code navigation was coupled to version control. This is because code navigation data could be uploaded for ~any commit.

During this time, one of the recurring sources of incidents was the (non-)incremental updates for the commit graph structure that was mirrored into Postgres, in order to be able to efficiently compute what code navigation data (stored in Postgres) was “visible” at which commit.

I recall being frustrated with that, and half-jokingly proposing that it would be good to move to a more custom storage solution or have custom indexes next to Git’s storage itself. That proposal didn’t particularly go anywhere (not that I expected it to), but the frustration with Git as a hard-to-operate database stayed with me.

Since around March 2026, I’ve been working on Source Control at Databricks. Some of the things I’ve worked on include:

- Enforcing consistent Git configurations across our devboxes.
- Creating various safety nets to prevent agents from messing up Git repos in ways that trigger pathological performance.
- Root-causing and fixing various client-side Git performance issues.

So in a way, I’ve been ~responsible for monitoring and maintaining the health of thousands of git checkouts (of our huge internal monorepo Depending on the specifics of compression, the history of the `master` branch ends up in the range of 20-50GB.) for the past few months. (Life lesson: don’t get upset at a database for being hard-to-operate lest the monkey’s paw curls? 😄)

Apart from my main work, I have spent dozens of hours studying jj, [Mononoke](https://github.com/meta/sapling/blob/main/eden/mononoke/README.md) (the open-source server for Facebook’s VCS), [TigerBeetle](https://github.com/tigerbeetle/tigerbeetle) and, to a lesser extent, RocksDB and papers on LSM trees. I also prototyped a minimal code storage server in [Tiger Style](https://tigerstyle.dev/).

Over this time, I was also fortunate enough to attend Git Merge 2026. Interestingly enough, the sponsors included OpenAI, Google, GitHub, Cursor, etc., and the attendees included a bunch of employees from Meta who work on Sapling/Mononoke, but no Anthropic employees were present AFAICT.

* * *

Whew, that was a lot of words! At least, I hope that this helps you understand where I’m coming from when I write the points below.

This section covers what I think of the overall design and development processes that have been described in the post on Scion.

### The familiar bits

Having studied Sapling/Mononoke, and having spent some time thinking about how to build a version control server, the following bits felt obvious I understand that the post is written for a broader audience, so it makes sense to repeat things which may be “obvious” to people who work in the area. That said, I think there’s value in separating out/clarifying to readers what is common knowledge within a given area vs what is genuinely novel. For example, if you were writing an academic paper, common knowledge would go into a Background section, not into the later sections. (and hence not worth emphasizing) and/or copied (without clear attribution):

- Representing commits with changesets instead of [Git-style trees](https://git-scm.com/book/en/v2/Git-Internals-Git-Objects): Copied from Mononoke. Attribution for this was added in a later update to the post after Rain’s blog hit the front page of HN.
- Client-server asymmetry: No huge monorepo that I know of actually has symmetry here. Even with Git, there are certain configuration options which are only meant for the client or the server.
- APIs for version control: This is a natural consequence of client-server asymmetry.
- Horizontal scalability: For a VCS server, horizontal scalability largely comes for free because it’s borrowed from the storage for the objects.
  - Specifically, one of the things which makes caching tricky is handling cache invalidation, but that doesn’t apply to a content-addressed store.
- “Commits must be cheap, incremental, and lock-free”: Once you’ve decided “commits are immutable content-addressed objects,” this falls out naturally.
- “The working copy is optional, and sparse by design”: At this scale, nobody materializes millions of files on disk all the time everywhere.
- Per-directory ACLs: These are supported by Perforce, Piper and Mononoke.
- No merge queue with full CI: I’m reading between the lines here – the wording in the post “What does CI even mean when hundreds or even thousands of changes can merge while tests are running? We haven’t been able to effectively replicate the industry standard of a global repository lock on the merge queue” doesn’t make it clear what exactly it is referring to. Mononoke does the same. At a smaller scale, Uber’s [SubmitQueue](https://github.com/uber/submitqueue) also does not run full CI.

### The surprising/novel bits

#### The speed of migration

> In order to support this growth, we built Scion, an in-house version control system, and fully migrated our monorepo from git in five months. We also built and shipped a code review surface (called Antique), code search, a rich source control IAM system, novel ways of managing and landing large amounts of reviewed code, and more, all in that same time period.

This was done by a team of 7 people and an undisclosed number of Claude tokens.

On reading this, my first thought was, “That’s still quite a lot of work.” With Git, it’s very easy for people (or agents) to come up with complex operations to do specific things, which can be difficult to easily map to other VCSes as-is. So it’s impressive that they managed to rip out Git fully regardless. It’s not 100% clear in the post if they’ve done so, but I’m assuming this based on the wording in the post.

I’m guessing that one or more of the following are true: (1) their monorepo lacks a file-based OWNERS system (2) the OWNERS system largely goes through with automated approvals from Claude (and hence does not bottleneck on human approvals) or (3) they used some targeted bypasses to go in and change people’s code to use their custom `scn` client instead of `git`.

#### “Superfan” Claudes

> we created Slack channels where we tasked Claude with being obsessively focused on the health of a single component. We call these Claudes “superfans.” We have a scion-daemon superfan, a scion-cache superfan, an Antique merge queue superfan \[..\] They have access to our telemetry and logs, and act like an experienced engineer who has nothing better to do than to refresh charts and look for anything strange. We also have traditional alerts for all of our systems, but our superfans can detect subtle and novel issues that make these alerts seem one-dimensional by comparison.

This is a pattern which I (personally) have not heard of being deployed elsewhere, presumably in large part due to the cost of frontier-model tokens. I’m guessing this will become more widespread as token prices fall.

It would’ve been nice if they’d given some examples of what “subtle and novel issues” these superfans found.

### The odd bits

#### Discouraging refs in the dev loop

> The development lifecycle in Scion discourages refs (pointers to commits, such as branches and tags), except for the main ref or release branches, because global state that every server has to agree on is a looming scaling problem.

This point didn’t quite add up for me. The amount of data associated with a ref is very small – it’s the ref’s name and the ID of the object it points to. If you separate out temporary refs (for landing/organizing work) and long-lived refs (for release branches), then temporary refs can be automatically cleaned up and would be proportional to the number of changes in flight. The long-lived refs would keep adding up anyway (and potentially contribute to whatever scaling problem is anticipated).

So how does not using refs in the dev loop help?

I can see a downstream *correctness* argument for avoiding refs, where it’s easy to accidentally write a job which (if it works with refs) accidentally pulls in two different commits (as the ref moves) instead of detecting ref movement and triggering failure/retry. However, it’s also easy to make a different kind of mistake just working with commit IDs – you could keep running the job for a commit that has become unreachable due to a push.

#### Inefficiencies in resource utilization

There are several points in the article related to resource utilization which didn’t add up in my head.

> Beyond these, there are standard workarounds for large monorepos, like internal git mirrors and repository snapshots (seeds) kept in blob storage. We’ve used all of them—at our peak, we operated several hundred internal git mirrors to deal with the load.

At Databricks, we do something similar with reference repos which are updated every ~2 hours, and we have fewer than 10 git mirrors to deal with CI load, despite the repo size being within the ~same order of magnitude. Even with this many mirrors, the CPU load is well under control, and increasing the frequency of snapshots from 2h to, say, 1h or even 30min – which would reduce CPU load on the mirrors – would be doable with a bit of work.

So it’s difficult for me to think of what kind of workload would necessitate “several hundred” git mirrors. Maybe very wide fan-outs for tests in CI?

Similarly, there’s another point about maintaining the mirrors later on:

> At our peak, we ran several hundred git mirrors, which significantly strained capacity and no longer scaled once the repo outgrew the mirrors’ disks or memory limits

It’s unclear how a repo with 100GB of history on the main branch could outgrow the disks on modern cloud instances, which can have several TB of storage. Yes, it requires some level of care wrt. maintenance, but disk limits and memory limits should generally not be the major problems here. Memory in particular is even more odd, because most accesses will likely be for the most recent data, which can easily stay resident in a few GB of RAM at most.

#### Performance numbers

Many of the pre-migration numbers provided on the chart (which make the Scion numbers look impressive) seem way too high, and some of the ones post-migration also seem unusually high. As one example, the charts show that the p50 for ‘pull latest’ fell from 87s → 3.5s. Each of these feels off for different reasons. This is one of the places where the post – by going wide instead of deep – makes it difficult to understand what’s actually being said. For example, percentiles can easily be skewed downwards due to automated background loops (esp. those running at high frequency) if most operations short-circuit to no-ops.

- Pre-migration: At our scale, fetching the changes at the start of a day can take over a minute, but after that, fetches are much faster. So even the p90 for fetch operations ends up being under 1 minute. Granted, “fetch” and “pull latest for the trunk” are not equivalent operations. However, I’m not sure what it would take for things to be so bad for the p50 to be at 87s if you do background pre-fetching once a devbox has booted up.

- Post-migration: 3.5s seems surprisingly high, given everything else. With sparse checkouts, presumably this number should be proportional to the size of the diff within that subset between pull operations. For example, naively, I’d expect network to be the limiting factor: even at 2Gbps GCP doesn’t appear to have [ingress bandwidth limits](https://docs.cloud.google.com/compute/docs/network-bandwidth#vm-in) for instances within a VPC, so I’m using the “2Gbps per vCPU” (for egress) as a rough guide/underestimate of available bandwidth. you could transfer 875MB in 3.5s. I can’t imagine the amount of data being transferred for a p50 “pull latest” being even close to that, so there must be something else that’s the bottleneck. Maybe the query for determining what data should be pulled is the slowest bit? Too many RPCs? Serialization/deserialization overhead? 🤷

### Squatting on the shoulders of giants

> Before we explore what we built, we should acknowledge how unusual our workload is. Unsurprisingly, Anthropic uses Claude to write software far more than almost any other company, so we’re often the first to see where tools built for human scale start to strain. Source control is just one example of this. While we don’t expect many organizations to have these problems in the very near term, we suspect it won’t be too long before others who use AI coding tools experience these same pressure points, which is why we wanted to share what we built and what we learned.

The vibe of this paragraph captures the vibe of the rest of the article. It is simultaneously:

1. Factually inaccurate: Even pre-LLMs, Google, Facebook and others have run into scaling issues with source control and solved them in different ways.
2. Self-congratulatory: This one needs no explanation. Special shout-out to “A git server doesn’t have to work the same way as a git client does; novel server architectures exist that provide significant scaling improvements.” – Erm, what exactly does Anthropic think that GitHub, GitLab, Cursor, Entire, ERSC etc. have been working on since the start of 2026?
3. Overstating the contribution: “share what we built and what we learned”, except that there’s very little of substance that’s not a copy of something that’s already available. And for Mononoke, the entire source code is available to study, not just some blog posts.

As Rain put in their post [More on Anthropic’s Scion ripping off Mononoke](https://sunshowers.io/posts/anthropic-mononoke-2/):

> It is always good to situate your work in the larger context. We all stand on the shoulders of giants! There is nothing wrong with borrowing and adapting existing work to new contexts — that is the vast majority of what I do as an engineer. When you position your work as derivative, you are acknowledging those that came before you, as you hope to be acknowledged by those who come after you.

Given how they’ve framed their work and how they’ve recognized prior work only with cursory nods (this changed somewhat after they got called out for it!), I’m now much less surprised by their absence at Git Merge 2026. Historically, companies and developers have joined communities to learn from other people’s expertise, and to be able to recruit people interested in the same areas.

Now that companies can increasingly point LLMs to clone your codebase, it is understandable that some of them no longer feel the need to engage with the corresponding communities in the same manner.

### Words from nowhere for nobody

One of the major problems with the blog post is that it is unclear as to what primary purpose it is meant to serve and who the target audience is.

- Is it meant as a doc for organizations to encourage them to build more ambitious projects (like a version control system) in-house with Claude?
  - If so, it seems a bit too modest, and has too many technical details.
- Is it meant as a recruiting pitch? (Come join us, we’ll give you a ton of free tokens and you get to work with smart people?)
  - I guess it technically got to the front page of Hacker News, so it served that purpose to some extent.
- Is it meant to convey some cool factor/novelty/innovation in Source Control?
  - If so, it fails to place the work in the larger context of source control for monorepos as it has evolved over the years.

Perhaps because of the lack of a clear audience in mind, even though the post was (thankfully!) not LLM-written, many parts of it suffer from LLM-isms like:

1. Stating the obvious: See the many points in earlier sections.
2. Meaningless comparisons: There are many throughout the post, but here’s one example. “Fetched naively, that would be over 600 PB of git traffic per day.” – here, the 600 PB number is based on 25 million checkouts × 25 GB per compressed copy of the checkout. Given that most clients already have a copy of the repo, this is not a meaningful baseline to compare against unless the daily churn and growth rates add up to close to 100%.
3. Going broad instead of deep: Many charts and numbers are provided without sufficient context, used as window dressing, and discussed briefly; then the post moves on to the next thing.
   - As a concrete example, there are multiple mentions of the growth in PR counts, but there’s no separation between automation PRs and human-in-the-loop PRs. One can try inferring this from the charts (e.g. the Sept data shows about 10K PRs/day landing on weekends), but that still leaves a lot of open questions. For example, given the jump from 15K PRs on Sept 7 → 25K PRs on Sept 8 → 30K PRs on Sept 9, how many of the 30K PRs on Sept 9 are from runaway automations?

### What about the user experience?

Historically, using version control has been a fairly human-centric activity. While you might have a sprinkling of deterministic automations, such as for bumping dependencies, reverting PRs which introduced regressions, etc., the main user was the developer.

As increasingly more companies use LLM agents for increasingly more parts of the development lifecycle, it is understandable that LLMs become the primary “user” for a VCS, and the developer is abstracted away from the VCS. In such a world, if you’re focused on the LLM as the “user”, you could ask questions about metrics like error rates/undo rates/latency for tool calls.

If you take a broader view and are interested in the end-to-end developer experience, you might still care about questions such as “what are the problems people face when trying to use the VCS via LLMs”, as well as “which operations are slow but frequent, and could be replaced with fast and deterministic tooling instead.”

But the post really does not touch upon either of those points, and overall ends up having no clear narrative arc as a result.

The post starts out with a relatively vague motivation:

> the latent demand for merging code is practically infinite, and every bottleneck we remove immediately leads to a further increase in the amount of code we produce. In order to support this growth, we built Scion \[..\]

However, near the end of the post, the wins are stated in terms of reduced CPU utilization (which translates to increased capacity for other things) and simplified systems (I understand why this is a win, but it seems like this was not the original motivation for pursuing this path).

I understand that the new VCS is able to support the current growth, but the implied thesis that the VCS was the bottleneck and removing that bottleneck actually unlocked the growth lacks compelling evidence in the post – one is left to wonder how much of the growth is attributable to increased background automations vs. model changes vs. other factors.

For example, were people frequently blocked from landing PRs *because* CI was at capacity and runs remained queued for a long time? If so, did freeing up capacity help reduce the time from PR creation to merging, which in turn led to more PRs being created? Maybe! But the reader is basically left to just guess all of the intermediate details without a clear connecting thread.

## Closing thoughts

When I first started reading the post, I went in with high expectations. After all, it has historically been rare for companies to build their own version control systems, and given how much money and talent Anthropic have at their disposal, I thought the post would’ve been a lot more interesting than it actually turned out to be.

Overall, had Anthropic clearly stated up front what parts of the design were copied from other systems with proper attribution, and focused the post on how they managed the migration, I think the post would’ve been more honest, more streamlined and also more useful to people outside Anthropic. Specifically:

- For the migration: sharing things like how they handled dependencies between various bits of work, making changes to non-centrally-owned tools, decoupling things, client libraries, project management, etc. – that could’ve potentially been useful.
- For the design:
  - Publishing RPC interfaces, database schemas and/or pseudocode for any of the interesting algorithms.
  - Discussing alternative designs that were considered and rejected along with reasons.

The post does none of that, It’s understandable that they wouldn’t want to open-source the code itself, as untangling it from the rest of the monorepo and having development in multiple places would add friction for themselves for unclear benefit. presents several previously established ideas as seemingly original without a clear separation between the two, and frames the high-level description as a “gift” to the developer ecosystem/Source Control community.

No, thank you.
