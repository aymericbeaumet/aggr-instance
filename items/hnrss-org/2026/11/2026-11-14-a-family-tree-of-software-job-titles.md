---
title: A family tree of software job titles
link: https://zwischenzugs.com/2026/11/03/a-family-tree-of-software-job-titles/
source: hnrss-org
published: 2026-11-14T17:45:19Z
updated: 2026-11-14T17:45:19Z
first_seen: 2026-11-15T00:13:30.261415141Z
authors:
- umerf
labels:
- technical
content: extracted
html: 2026-11-14-a-family-tree-of-software-job-titles.html
remote_preview:
  url: https://raw.githubusercontent.com/ianmiell/sw-job-title-family-tree/main/job_title_family_tree.png
  alt: The family tree
---

For a few years I was ‘Head of DevOps’. Before I got that job title I’d done much the same work under other names, and since then I’ve watched the same work get called SRE, and then it seemed to morph towards platform engineering. If you’ve been in this industry for any length of time you’ll have seen titles come and go like this, and you might have wondered where they all came from.

So I (with the help of Mr Claude) built this family tree of software job titles, from the 1940s to now.

It’s here: [ianmiell.github.io/sw-job-title-family-tree](https://ianmiell.github.io/sw-job-title-family-tree/)

[![The family tree](https://raw.githubusercontent.com/ianmiell/sw-job-title-family-tree/main/job_title_family_tree.png)](https://ianmiell.github.io/sw-job-title-family-tree/)

## What It Is

There are 104 titles, from ‘Computer’ (a person, not a machine) at the top to ‘AI Agent / Context Engineer’ at the bottom.

Time runs from top to bottom, in five-year rows. The titles sit in seven coloured columns by family:

- Development
- Analysis, architecture and leadership
- Operations and infrastructure
- Networks and security
- Data and AI
- Quality and test
- Support and customer-facing

A solid arrow means the title grew out of its parent. A dashed arrow means influence, or a common career move. Landmark titles are in bold.

On the site you can click a family to see just that column (here’s [Data & AI](https://ianmiell.github.io/sw-job-title-family-tree/#data)), or click a title to get its history, background and connections (here’s [SRE](https://ianmiell.github.io/sw-job-title-family-tree/#sre)).

The whole thing is one Graphviz file and one JSON file of descriptions. Each title is one line in the `.dot` file, and the picture is rendered with:

```
$ dot -Tsvg job_title_family_tree.dot -o job_title_family_tree.svg
```

Adding a title is three small edits: a node in its family’s column, an entry in the row for its five-year period, and its parent arrows.

A word on the dates. They’re roughly when a title became *common*, not its first recorded use, and they’re my best estimates, not sourced citations. Some are well known (SRE at Google in 2003, ‘DevOps’ coined in 2009, ‘Data Scientist’ in 2008). Plenty are a ‘c.’ and a shrug.

## Six Things I Noticed

Drawing it out made a few patterns hard to miss.

### 1) Coding Started Out as the Clerical Bit

In 1947, Goldstine and von Neumann’s ‘Planning and Coding of Problems’ split the work of using a computer in two: mathematicians planned the problem, and coders translated the plan into machine instructions. Coding was treated as lower-status clerical work, and much of it was done by women.

It turned out that coding eventually became the hard, creative part, and the coder merged into the programmer.

Go back further and it’s stranger still. Several of the first ENIAC programmers were recruited from the Army’s human computers, people whose job was to compute by hand. So the job gave its name to the machine, and its people to programming.

### 2) Titles Break Apart, Then Join Back Up

The webmaster turned up around 1993, and was often a single person who wrote the HTML, ran the webserver (often a box under a desk), answered the email, and kept the site up (sometimes by kicking it, more often by plugging it back in after kicking it).

By the mid-2000s the title had all but gone, as it was unfashionably ancient. The code went to web developers, the servers to sysadmins, and the content to editors and designers.

Then the web developer split into front-end and back-end around 2005, and the two joined back up again as ‘full-stack’ around 2008.

This is the centralisation/distribution see-saw I’ve [written](https://zwischenzugs.com/2020/11/30/gitops-decisions/) about [before](https://zwischenzugs.com/2022/01/17/why-its-great-to-be-a-consultant/), playing out in job titles.

### 3) Some Titles Don’t Last

The prompt engineer made headlines in 2023 with a few eye-wateringly well-paid job listings. As the models got better, the skill became part of other jobs, and on the tree it gets absorbed into the AI engineer almost as soon as it appears.

The scrum master is a slower version of the same story: demand peaked in the 2010s, and many organisations have since folded it back into engineering management or delivery roles.

The newest title on the tree is the ‘AI Agent / Context Engineer’ (c. 2025). The description for it currently says “time will tell whether it stays a separate title”. I’m not taking bets.

### 4) Names Get Recycled

‘Systems Engineer’ is two different jobs on the tree. At IBM in the 1960s it was a pre-sales role, working with customers to design installations and get them running. In the 1990s, thanks largely to Microsoft’s MCSE certification, it meant a more senior, design-oriented sysadmin. Same title, different branch, no relation.

‘Configuration management’ started as a 1960s US defence discipline for tracking hardware changes, became software version control in the 1970s, and then came back decades later as the name for tools like Puppet and Chef.

### 5) The New Thing Is Often an Old Thing

The ‘forward deployed engineer’ (FDE) is one of the hottest titles of 2025, as OpenAI, Anthropic and plenty of AI startups hire FDEs to get AI working inside their customers’ organisations.

Palantir built its business around them from about 2010: software engineers embedded with a customer, often on site, building whatever is needed to make the product work for that customer’s problem.

Which sounds a lot like IBM’s systems engineers of the 1960s, who worked with customers before and after the sale, designing installations and getting systems running. (It also sounds a lot like old-style consulting, but I would say that.)

### 6) The DevOps Debate Is There

Ben Treynor Sloss defined SRE as “what happens when you ask a software engineer to design an operations team”. DevOps was meant as a culture of developers and operations working together, but by about 2011 ‘DevOps Engineer’ had become a job title, usually for whoever did the automation. On the tree, the arrow between SRE and DevOps points both ways. Platform engineering has both of them as parents, and was sold around 2022 as ‘the successor to DevOps’.

The Dev/Ops divide has never been resolved. It just keeps getting new names.

## Why Bother?

When I’m asked what the difference is between a DevOps engineer, an SRE and a platform engineer, the honest answer is always a little history lesson. This is the shortest version of that lesson I’ve managed so far.

The dates are best guesses, and I’ll have missed titles, mangled some lineages and drawn arrows people will argue with. If you know better, let me know, or raise a PR on [the repo](https://github.com/ianmiell/sw-job-title-family-tree).

Which brings me back to the coders.

In 1947 coding was the clerical bit, and the real work was in the planning. Wind forward eighty years and you’ll hear much the same said about coding again, because AI. Last time, the coders turned out to be doing the hard part.

I’ll leave you to draw that arrow yourself.
