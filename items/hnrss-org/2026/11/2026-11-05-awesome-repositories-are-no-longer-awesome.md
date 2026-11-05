---
title: Awesome repositories are no longer awesome
link: https://erikgahner.github.io/posts/2026/awesome-repositories-are-no-longer-awesome/
source: hnrss-org
published: 2026-11-05T11:16:51Z
updated: 2026-11-05T11:16:51Z
first_seen: 2026-11-05T17:57:26.867095589Z
authors:
- erikgahner
content: extracted
html: 2026-11-05-awesome-repositories-are-no-longer-awesome.html
---

On GitHub, you will find a lot of repositories with the prefix 'awesome'. They usually contain curated knowledge and resources related to a certain domain, such as awesome-python (see, e.g., [this repository](https://github.com/sindresorhus/awesome/) for a list of awesome repositories). I used to see these awesome repositories as potential goldmines of good content. However, nowadays I see most of them as irrelevant and useless. There are two reasons for this, and they are both related to AI.

First, as many of these awesome lists get older, they contain a lot of outdated material if they are not actively maintained. You are in most cases better off simply asking a decent AI model for inspiration or the material you need. That is, I find it difficult to believe that most of these awesome lists are of interest to people anymore. If they remain useful, it will mostly be for agents looking to find and check relevant resources.

I had a quick look at the [awesome-R](https://github.com/qinwf/awesome-R) repository. The repository was last updated more than a year ago, and most of the resources included in the overview are either outdated or will not be of general interest to people using R. One of the first links in the repository is an overview of the top 50 downloaded packages - as of 2016! This is simply not a good resource if you want to explore R in any shape or form.

Second, a lot of these awesome repositories are being flooded with AI slop. Go to any random awesome repository these days and you will most likely find a lot of pull requests suggesting the most generic tools and resources. For example, take a look at this [awesome-free-online-tools](https://github.com/abdessamadbettal/awesome-free-online-tools) repository. It is a small, unknown repository with 13 stars but 91 open pull requests at the time of writing. I did not look at all of these pull requests, but the ones I looked at all suggested generic vibe-coded tools.

I found this resource because of [a pull request](https://github.com/erikgahner/awesome-statistics/pull/6) made to one of my repositories, a simple project where I store material I like related to statistics. The suggestion was to add StatOrigin ("a free, open database of industry statistics"), and once I had a quick look at the resource and the other pull requests made by the user, I could easily close the PR. This user alone has created at least six vibe-coded websites (StatOrigin, ScanChess, Sticker Printer Hub, QuizQuip, LiquidityPools, DefiLPKit) and submitted more than 100 pull requests to various awesome lists to add these websites.

It did not take me long to find examples where these tools were merged into the relevant repositories (see, e.g., [here](https://github.com/okhosting/awesome-open-data/pull/60), [here](https://github.com/public-apis/public-apis/pull/7571), and [here](https://github.com/marcelscruz/public-apis/pull/1343)). However, I also found examples where people did the basic work of checking out the material, such as [here](https://github.com/brandonhimpfen/awesome-online-learning/pull/39), where the PR was closed with the following explanation: "There is not yet sufficient evidence of qualified educator review, independent validation of the educational content, or demonstrated learning outcomes." Good stuff!

My point is not that these awesome repositories are now all bad. They can still be awesome. But as a general approach to finding good resources, awesome lists are less awesome than they used to be. As AI gets better at finding and creating resources (and creating pull requests for said resources), these awesome repositories will become less relevant and less useful.
