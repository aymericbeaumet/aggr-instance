---
title: Hill-climbing MAI models for GitHub Copilot and Excel
link: https://microsoft.ai/news/hill-climbing-mai-models-for-github-copilot-and-excel/
source: microsoft-ai
published: 2026-07-23T16:30:00Z
updated: 2026-07-23T16:30:00Z
first_seen: 2026-09-08T10:17:10.166315Z
authors:
- alenabu@microsoft.com
summary: The post Hill-climbing MAI models for GitHub Copilot and Excel appeared first on Microsoft AI.
content: extracted
html: 2026-07-23-hill-climbing-mai-models-for-github-copilot-and-excel.html
preview:
  file: 2026-07-23-hill-climbing-mai-models-for-github-copilot-and-excel.preview-d4d36f4683de.webp
  width: 256
  height: 190
  color: '#948266'
images:
- source: https://microsoft.ai/wp-content/uploads/2026/07/Screenshot-2026-07-23-at-8.15.14-AM-1024x761.png
  original:
    file: 2026-07-23-hill-climbing-mai-models-for-github-copilot-and-excel.image-13ca4096af86.png
    width: 1024
    height: 761
  variants:
  - file: 2026-07-23-hill-climbing-mai-models-for-github-copilot-and-excel.image-fe4ea8d3bb04.webp
    width: 48
    height: 36
  - file: 2026-07-23-hill-climbing-mai-models-for-github-copilot-and-excel.image-2fba961dd7ef.webp
    width: 320
    height: 238
  - file: 2026-07-23-hill-climbing-mai-models-for-github-copilot-and-excel.image-ffb73de3d978.webp
    width: 640
    height: 476
  - file: 2026-07-23-hill-climbing-mai-models-for-github-copilot-and-excel.image-ef8bd3412b49.webp
    width: 960
    height: 713
  - file: 2026-07-23-hill-climbing-mai-models-for-github-copilot-and-excel.image-8855dfac84c8.webp
    width: 1024
    height: 761
  color: '#48492a'
- source: https://microsoft.ai/wp-content/uploads/2026/07/hillclimb_chart_2.png
  original:
    file: 2026-07-23-hill-climbing-mai-models-for-github-copilot-and-excel.image-2e913032557a.png
    width: 1414
    height: 792
  variants:
  - file: 2026-07-23-hill-climbing-mai-models-for-github-copilot-and-excel.image-be19429fccf0.webp
    width: 48
    height: 27
  color: '#fdf8ec'
- source: https://microsoft.ai/wp-content/uploads/2026/07/Edge_HillClimb_03-e1784772446649-2880x1647.png
  original:
    file: 2026-07-23-hill-climbing-mai-models-for-github-copilot-and-excel.image-295a27feca0d.png
    width: 2880
    height: 1647
  variants:
  - file: 2026-07-23-hill-climbing-mai-models-for-github-copilot-and-excel.image-ee1fc0efbdf6.webp
    width: 48
    height: 27
  - file: 2026-07-23-hill-climbing-mai-models-for-github-copilot-and-excel.image-1cf88a4f7c6c.webp
    width: 320
    height: 183
  - file: 2026-07-23-hill-climbing-mai-models-for-github-copilot-and-excel.image-d2693476467d.webp
    width: 640
    height: 366
  - file: 2026-07-23-hill-climbing-mai-models-for-github-copilot-and-excel.image-7209fc63e782.webp
    width: 960
    height: 549
  - file: 2026-07-23-hill-climbing-mai-models-for-github-copilot-and-excel.image-3a3e53f944d1.webp
    width: 1280
    height: 732
  - file: 2026-07-23-hill-climbing-mai-models-for-github-copilot-and-excel.image-d73ad72bc984.webp
    width: 1600
    height: 915
  - file: 2026-07-23-hill-climbing-mai-models-for-github-copilot-and-excel.image-f0c369ea82bf.webp
    width: 2880
    height: 1647
  color: '#d8e7d7'
---

Models

 Superintelligence team

 July 23, 2026

 Models

 Superintelligence team

**Better models , fewer parameters, less tokens**

At Build in June, we introduced our [h ill – climbing machine](https://microsoft.ai/news/building-a-hillclimbing-machine-launching-seven-new-mai-models/) , our integrated data, model, and harness flywheel. Today we are excited to share two examples inside Microsoft: MAI models specialized for agentic workloads in GitHub Copilot and Excel.

Early results are promising. In our live product deployment, we see that our MAI model deployed in Excel is on par with GPT-5.6 for the most common tasks while being more cost-efficient.

The figure below shows how MAI-Code-1-Flash, post-trained within the GitHub Copilot harness, was used as the starting checkpoint to climb on Excel evaluations, resulting in two highly efficient, specialized models.

![Line graph showing pass rates on SWE Bench (VS Code and Base) across model checkpoints, with data points labeled \"Code\" and \"Excel.\" Pass rates increase over checkpoints, starting below 72% and peaking at 86%.](https://microsoft.ai/wp-content/uploads/2026/07/hillclimb_chart_2.png)

**MAI-Code-1-Flash in GitHub Copilot**

Since launching MAI-Code-1-Flash in GitHub Copilot in June, millions of developers have been using it for their day-to-day work, where it’s outperforming other similarly sized models while using fewer tokens.

- It has an approximately 10% higher code accept rate than GPT 5.4 Mini and Claude Haiku 4.5 in VS Code.

- Developers were 6% more likely to return across multiple days than with GPT 5.4 Mini and 11% more likely than with Claude Haiku 4.5 .

- It has 10% lower median token usage than GPT-5.4 mini and Claude Haiku 4.5, with more user-initiated turns.

**MAI model live in Excel**

Excel offered a test for whether the capabilities built into MAI-Code-1-Flash could transfer beyond the domain they were trained for, moving from agentic coding to agentic knowledge work. To do so, we further trained our MAI-Code-1-Flash checkpoint in an Excel reinforcement learning environment to learn about tools and knowledge workflows in spreadsheets. The result is a model with a command of Excel workflows that is more efficient and less expensive to run.

![Flowchart titled \"The Excel climb\" showing an Excel RL Environment with action, review, execute, and update steps, linked to input, output, grader, and updated model weights.](https://microsoft.ai/wp-content/uploads/2026/07/Edge_HillClimb_03-e1784772446649-2880x1647.png)

User feedback from production traffic indicates that the quality of the MAI model in Excel is on par with GPT-5.6 for the most common tasks. In addition to the direct model cost savings, this smaller, more efficient model can be served on both Nvidia H100 and A100 class GPUs rather than requiring only the latest-generation accelerators, which significantly lowers the cost of deployment for Microsoft.

**Training agentic models from inside the product stack**

These results point toward a broader strategy. By having access to the entire product stack—the model, the harness that runs it, the agents, and product-specific evaluations—we can hill-climb to train efficient, powerful models capable of tasks previously handled by larger, more expensive ones.

Beyond GitHub Copilot and Excel, we’re currently extending this hill-climbing approach to train efficient models across Microsoft’s family of agentic products: Copilot Chat, Outlook, PowerPoint, and more.

## Related Stories
