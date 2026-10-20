---
title: AI coding agents generate more code, but not more software
link: https://arstechnica.com/ai/2026/10/ai-coding-agents-generate-more-code-but-not-more-software/
source: hnrss-org
published: 2026-10-12T17:14:01Z
updated: 2026-10-12T17:14:01Z
first_seen: 2026-10-12T22:35:36.108955126Z
authors:
- gumby
content: extracted
html: 2026-10-12-ai-coding-agents-generate-more-code-but-not-more-software.html
extra:
  comments_url: https://news.ycombinator.com/item?id=50059523
  num_comments: 223
  points: 272
---

Anyone who has even tangentially associated with computer programming knows that modern AI coding assistants and agents [can be incredibly efficient at generating huge amounts of functional code](https://arstechnica.com/ai/2026/01/developers-say-ai-coding-tools-work-and-thats-precisely-what-worries-them/). But coders making use of those tools also [know better than to trust the accuracy of that code](https://arstechnica.com/ai/2025/07/developer-survey-shows-trust-in-ai-coding-tools-is-falling-as-usage-rises/), meaning [substantial effort needs to be spent](https://arstechnica.com/ai/2025/05/time-saved-by-ai-offset-by-new-work-created-study-suggests/) reviewing any AI-generated output.

[A recent study](https://fion.ac/jellyfish.pdf) of actual coding practices across hundreds of firms finds that human code review forms a significant “bottleneck” for the overall efficiency of AI coding tools, resulting in “little evidence that firms increase software output or reduce employment” by using them. Any efficiency increased during the actual coding phase, the study authors find, is “absorbed by downstream constraints in the production process”; as “the code review process significantly increases in length, pull requests are more likely to require revisions, and reviewers leave more comments.”

## Cut once, measure twice

To come to these conclusions, Harvard University researchers Fiona Chen and James Stratton made use of aggregated analytics data from [Jellyfish](https://jellyfish.co/), which measures the granular output of engineering teams. That data encompasses 300 million individual “work events” (e.g., commits and pull requests) and issue management software data across more than 700,000 employees at over 700 relevant software development firms from 2021 through March of 2026.

To assess the impact of AI tools on these firms, the researchers used a mix of directly measured AI usage and analyses of GitHub activity to determine when each company started introducing either AI coding assistants (which can help auto-complete code primarily authored by humans) and/or AI coding agents (which primarily write and submit code autonomously based on prompts) into their workflows. The researchers then perform some complicated math to determine a “difference of differences” regression on key variables both before and after the introduction of these tools at different points in time across different organizations.

[![](https://cdn.arstechnica.net/wp-content/uploads/2026/10/aiagentcode.png)](https://cdn.arstechnica.net/wp-content/uploads/2026/10/aiagentcode.png)

If there’s one thing AI coding agents are good at, it’s churning out a lot of code.

If there’s one thing AI coding agents are good at, it’s churning out a lot of code. Credit: [Chen and Stratton](https://fion.ac/jellyfish.pdf)

In terms of raw code being produced, the results are clear and stark. The introduction of AI coding agents at a firm leads to a 30 percent increase in total lines of code generated, a 20 percent rise in the number of total commits, and a 23 percent increase in pull requests on average, the researchers write. But all that extra code doesn’t translate directly into improved software output on the firm level. On the contrary, the resolution rate for Issues and Epics (i.e. wholesale software features) tracked by tools like Jira did not change in a statistically significant way after AI tools were introduced (the researchers also found no “compositional shift” in the size or complexity of those Jira-tracked issues across the AI introduction).

The reason for that discrepancy can be found directly in the code review process, which takes markedly longer on average after the introduction of AI coding agents. Overall, the average “review process” time between a pull request getting submitted and it being merged into the codebase balloons 49 percent on average after AI agents are introduced. That effect can be seen in more granular data, too, with “the share of pull requests with changes requested nearly doubl\[ing\], and the number of comments per pull request increas\[ing\] by 35%” following the AI agent shift, the researchers write.

In response to this change, the researchers found a 14 percent increase in the share of workers performing code reviews after AI agents’ introduction. They also write that they “cannot attribute significant employment changes to AI” after looking at total active workers across Jellyfish and cross-referencing with LinkedIn data at those firms.

[![](https://cdn.arstechnica.net/wp-content/uploads/2026/10/aiagentpr.png)](https://cdn.arstechnica.net/wp-content/uploads/2026/10/aiagentpr.png)

Pull requests need changes a lot more often in the “agentic coding” era.

Pull requests need changes a lot more often in the “agentic coding” era. Credit: [Chen and Stratton](https://fion.ac/jellyfish.pdf)

While AI could also theoretically help with this review process, the researchers found that, so far, that impact has been marginal. Although 80 percent of measured firms used some form of AI code review by March 2026, AI agents were only responsible for 23.3 percent of all review comments and 10.8 percent of all pull requests, suggesting humans were still responsible for the vast majority of this work.

AI agents are still a relatively new part of the coding world, of course, and there have been significant updates and upgrades to their output even since this study’s March 2026 data cutoff. And while 95 percent of firms in the study have implemented AI coding agents by this point, many are doubtlessly still going through a learning process regarding when and how to best deploy them. These kinds of “coding time versus review time” trade-offs could improve as software engineering teams gain more experience with the pros and cons of siccing an AI agent on particular coding problems.

For now, though, letting AI write your code looks like a double-edged sword, with increases in coding speed counteracted by similar increases in human code review time and effort. It’s the kind of result that makes us wonder if the considerable time and expense to get AI coding agents working is really worth it for most companies.
