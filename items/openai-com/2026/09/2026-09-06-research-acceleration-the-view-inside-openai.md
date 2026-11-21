---
title: 'Research acceleration: The view inside OpenAI'
link: https://openai.com/index/research-acceleration-view-inside-openai
source: openai-com
published: 2026-09-06T08:00:00Z
updated: 2026-09-06T08:00:00Z
first_seen: 2026-10-19T01:26:53.998049939Z
labels:
- research
summary: Inside OpenAI, coding agents are reshaping AI research. Explore early data on agent usage, experiment velocity, task complexity, and research acceleration.
content: extracted
html: 2026-09-06-research-acceleration-the-view-inside-openai.html
---

For AGI to benefit all of humanity, we believe it must be democratically governed. This can only happen through an informed public debate about the capabilities, risks and safeguards of highly capable AI systems. People everywhere need to understand the likely future trajectory of frontier AI, so they can have a meaningful voice in how it develops.

Transparency about specific risks, incidents and safeguards is necessary, but not sufficient. We believe the public also needs to understand how the most capable systems are developing, and how they are driving research progress, inside of frontier labs.

We aim to safely build an automated AI researcher that can work under human supervision to further progress on deep learning and alignment, enabling iterative improvements. According to our measurements, we have now reached the goal, [announced](https://x.com/sama/status/1983584366547829073?lang=en) last fall, of having an automated research intern by September of this year. By “research intern,” we mean a system that can carry out well-defined research tasks under human direction, including tasks that would take a skilled researcher a few days. We are making strong progress toward creating an automated AI researcher by March of 2028.

Over the course of this year, OpenAI researchers’ daily work has changed substantially. Researchers are using coding agents throughout the day (often in concurrent sessions) and total usage is rapidly increasing, outpacing growth among other OpenAI teams. Researchers are contributing code faster and running more experiments. The ways researchers use agents are changing, too: agents are handling increasingly complex tasks, and succeeding at them more often. AI research is a complex process with many potential bottlenecks, so the overall pace of progress likely won’t keep pace with these specific metrics. But on the whole, these findings are consistent with the broader impression many of us have internally that agentic tools are meaningfully accelerating research progress. People still set our research priorities, judge which ideas and results to pursue, and decide whether to scale, pause, or deploy systems.

If it is done responsibly, we believe automated AI research will yield models that directly enhance human welfare and advance OpenAI’s mission. It can bring down the cost of advanced intelligence so that people worldwide can benefit. We are pursuing this work in part because automated research could help us solve alignment and build defenses against increasingly capable AI. An automated AI researcher can also be an automated safety or alignment researcher. More capable, aligned systems could help secure critical infrastructure, defend against dangerous AI agents, and develop new protective measures.

These are reasons to develop useful automated research capabilities, but they do not mean that rapid RSI is necessarily an outcome we should pursue. Whether and how to proceed must depend on our ability to preserve human control and on informed democratic choices about the benefits and risks.

We do not yet know how to safely get all the way to aligned, full RSI. We are working to scale alignment and safety measures alongside capabilities. But we cannot assume that progress in alignment and safety will keep pace, and more capable systems can become harder to monitor. Careful alignment and safety work is at the center of this effort, and it starts with measuring and mitigating the safety problems we see today in agentic coding systems. Whenever we find that proceeding would pose an unacceptable safety risk, we will respond appropriately including by slowing or stopping our development or deployment of systems we find ourselves unable to sufficiently safeguard.

After the recent Hugging Face incident, we [put this commitment into action](https://openai.com/index/pacing-model-development-cyber-capabilities/), pausing reinforcement learning (RL) training on our latest models intended for deployment while we further hardened and red-teamed our research environments and expanded coverage of our monitoring systems. This did not halt all research: some workloads resumed under stronger controls, while others remained paused. We have raised our safety and alignment standards and moved safety work deeper into the model lifecycle, requiring stronger evidence of aligned behavior throughout all of training.

Today we are providing a detailed snapshot of how agentic systems have contributed to our progress toward RSI in recent months. Agentic systems are new and rapidly changing, and our measurement efforts are still preliminary. By sharing these early results and the methods behind them, we aim to inform the public, encourage a norm of public disclosure, and help the field move toward shared standards of measurement.

Ultimately, as we wrote in our [frontier policy blueprint](https://openai.com/index/frontier-safety-blueprint/), we believe that we and other companies should be required to publicly track our progress toward RSI. Even without such a requirement, we plan to continue being transparent about our RSI progress. We will evolve our transparency approach as our measurement techniques and understanding improve, while balancing the need to protect security and proprietary information.

## 1\. Coding agents are reshaping daily work for OpenAI researchers

At the start of this year, the median researcher ranked by agent usage at OpenAI was using coding agents only in modest amounts. By mid-August, the median researcher was integrating agents daily into their work, using more than $600 per day of inference at API prices. The 90th percentile user in our research organization now uses more than $7,000 of tokens per day.

Usage of internal coding agents is increasing significantly—Median researcher (chart data)
| Date       | Value    |
| ---------- | -------- |
| 2026-01-04 | 0        |
| 2026-01-11 | 0        |
| 2026-01-18 | 0.1656   |
| 2026-01-25 | 0.5127   |
| 2026-02-01 | 0.9388   |
| 2026-02-08 | 2.2371   |
| 2026-02-15 | 1.7221   |
| 2026-02-22 | 1.0902   |
| 2026-03-01 | 5.6647   |
| 2026-03-08 | 10.4533  |
| 2026-03-15 | 10.3283  |
| 2026-03-22 | 13.3959  |
| 2026-03-29 | 13.8073  |
| 2026-04-12 | 29.4444  |
| 2026-04-19 | 42.3021  |
| 2026-04-26 | 45.2983  |
| 2026-05-03 | 65.6173  |
| 2026-05-10 | 81.9574  |
| 2026-05-17 | 80.0435  |
| 2026-05-24 | 100.7371 |
| 2026-05-31 | 136.6819 |
| 2026-06-07 | 148.2872 |
| 2026-06-14 | 161.09   |
| 2026-06-21 | 155.8407 |
| 2026-06-28 | 151.2029 |
| 2026-07-12 | 162.7279 |
| 2026-07-19 | 236.3348 |
| 2026-07-26 | 304.4503 |
| 2026-08-02 | 376.8317 |
| 2026-08-09 | 433.8899 |
| 2026-08-15 | 601.2531 |

Usage of internal coding agents is increasing significantly—90th percentile researcher (chart data)
| Date       | Value     |
| ---------- | --------- |
| 2026-01-04 | 1.9483    |
| 2026-01-11 | 10.1018   |
| 2026-01-18 | 19.1746   |
| 2026-01-25 | 28.5565   |
| 2026-02-01 | 34.2672   |
| 2026-02-08 | 42.7237   |
| 2026-02-15 | 42.596    |
| 2026-02-22 | 44.4915   |
| 2026-03-01 | 86.2304   |
| 2026-03-08 | 112.1497  |
| 2026-03-15 | 111.7549  |
| 2026-03-22 | 156.4143  |
| 2026-03-29 | 238.5984  |
| 2026-04-12 | 344.9479  |
| 2026-04-19 | 412.7334  |
| 2026-04-26 | 460.4904  |
| 2026-05-03 | 573.4557  |
| 2026-05-10 | 645.3055  |
| 2026-05-17 | 688.3652  |
| 2026-05-24 | 794.7286  |
| 2026-05-31 | 972.4137  |
| 2026-06-07 | 971.0772  |
| 2026-06-14 | 1111.2223 |
| 2026-06-21 | 1019.5371 |
| 2026-06-28 | 1168.9379 |
| 2026-07-12 | 1641.7621 |
| 2026-07-19 | 2188.3941 |
| 2026-07-26 | 4412.4961 |
| 2026-08-02 | 5274.2501 |
| 2026-08-09 | 6099.6463 |
| 2026-08-15 | 7047.1264 |

Before June 2026, total agent runtime across the research organization was still below that of total human labor. That has since changed. In terms of a standard 8 hour workday, as of mid-August, in total, the research organization uses 3.1 agent-workdays of effort for every workday of human labor.

Agentic workdays now far exceed those of human researchers (chart data)
| Usage date | Agent workdays per researcher workday |
| ---------- | ------------------------------------- |
| 2026-05-03 | 0.4848                                |
| 2026-05-04 | 0.4986                                |
| 2026-05-05 | 0.517                                 |
| 2026-05-06 | 0.5345                                |
| 2026-05-07 | 0.5483                                |
| 2026-05-08 | 0.5629                                |
| 2026-05-09 | 0.5696                                |
| 2026-05-10 | 0.5736                                |
| 2026-05-11 | 0.5823                                |
| 2026-05-12 | 0.5901                                |
| 2026-05-13 | 0.6007                                |
| 2026-05-14 | 0.6099                                |
| 2026-05-15 | 0.6202                                |
| 2026-05-16 | 0.6266                                |
| 2026-05-17 | 0.6357                                |
| 2026-05-18 | 0.6449                                |
| 2026-05-19 | 0.6543                                |
| 2026-05-20 | 0.6621                                |
| 2026-05-21 | 0.6737                                |
| 2026-05-22 | 0.6846                                |
| 2026-05-23 | 0.6852                                |
| 2026-05-24 | 0.6871                                |
| 2026-05-25 | 0.6827                                |
| 2026-05-26 | 0.6925                                |
| 2026-05-27 | 0.7045                                |
| 2026-05-28 | 0.7188                                |
| 2026-05-29 | 0.7353                                |
| 2026-05-30 | 0.7394                                |
| 2026-05-31 | 0.7376                                |
| 2026-06-01 | 0.7499                                |
| 2026-06-02 | 0.7597                                |
| 2026-06-03 | 0.7801                                |
| 2026-06-04 | 0.8081                                |
| 2026-06-05 | 0.8305                                |
| 2026-06-06 | 0.8531                                |
| 2026-06-07 | 0.8675                                |
| 2026-06-08 | 0.8961                                |
| 2026-06-09 | 0.9254                                |
| 2026-06-10 | 0.9517                                |
| 2026-06-11 | 0.9812                                |
| 2026-06-12 | 1.0116                                |
| 2026-06-13 | 1.026                                 |
| 2026-06-14 | 1.0405                                |
| 2026-06-15 | 1.0776                                |
| 2026-06-16 | 1.1145                                |
| 2026-06-17 | 1.1577                                |
| 2026-06-18 | 1.1945                                |
| 2026-06-19 | 1.2058                                |
| 2026-06-20 | 1.2155                                |
| 2026-06-21 | 1.2273                                |
| 2026-06-22 | 1.2617                                |
| 2026-06-23 | 1.2872                                |
| 2026-06-24 | 1.3107                                |
| 2026-06-25 | 1.3302                                |
| 2026-06-26 | 1.3531                                |
| 2026-06-27 | 1.3692                                |
| 2026-06-28 | 1.3963                                |
| 2026-06-29 | 1.4102                                |
| 2026-06-30 | 1.4225                                |
| 2026-07-01 | 1.427                                 |
| 2026-07-02 | 1.4257                                |
| 2026-07-03 | 1.4268                                |
| 2026-07-04 | 1.4416                                |
| 2026-07-05 | 1.4471                                |
| 2026-07-06 | 1.4777                                |
| 2026-07-07 | 1.5116                                |
| 2026-07-08 | 1.5513                                |
| 2026-07-09 | 1.6136                                |
| 2026-07-10 | 1.6727                                |
| 2026-07-11 | 1.6889                                |
| 2026-07-12 | 1.6997                                |
| 2026-07-13 | 1.7088                                |
| 2026-07-14 | 1.723                                 |
| 2026-07-15 | 1.7208                                |
| 2026-07-16 | 1.7264                                |
| 2026-07-17 | 1.7861                                |
| 2026-07-18 | 1.822                                 |
| 2026-07-19 | 1.8553                                |
| 2026-07-20 | 1.9023                                |
| 2026-07-21 | 1.9587                                |
| 2026-07-22 | 2.0375                                |
| 2026-07-23 | 2.1114                                |
| 2026-07-24 | 2.2006                                |
| 2026-07-25 | 2.2434                                |
| 2026-07-26 | 2.2551                                |
| 2026-07-27 | 2.3003                                |
| 2026-07-28 | 2.3534                                |
| 2026-07-29 | 2.4006                                |
| 2026-07-30 | 2.4251                                |
| 2026-07-31 | 2.4618                                |
| 2026-08-01 | 2.4654                                |
| 2026-08-02 | 2.522                                 |
| 2026-08-03 | 2.5843                                |
| 2026-08-04 | 2.6495                                |
| 2026-08-05 | 2.7114                                |
| 2026-08-06 | 2.7523                                |
| 2026-08-07 | 2.7688                                |
| 2026-08-08 | 2.7936                                |
| 2026-08-09 | 2.8214                                |
| 2026-08-10 | 2.8653                                |
| 2026-08-11 | 2.9331                                |
| 2026-08-12 | 2.988                                 |
| 2026-08-13 | 3.0445                                |
| 2026-08-14 | 3.1024                                |
| 2026-08-15 | 3.1416                                |

Another way of looking at this is to understand how many researchers use highly concurrent workflows (e.g., running 4 or more agents simultaneously). As shown below, this number is increasing. These figures include the daily peaks of both agents started directly by the user and subagents created downstream from those the user launched directly.

Researchers are leveraging concurrent workflows more over time (chart data)
| Date       | Series                  | Value  |
| ---------- | ----------------------- | ------ |
| 2026-04-12 | 4+ concurrent workflows | 0.2959 |
| 2026-04-13 | 4+ concurrent workflows | 0.3289 |
| 2026-04-14 | 4+ concurrent workflows | 0.3412 |
| 2026-04-15 | 4+ concurrent workflows | 0.3439 |
| 2026-04-16 | 4+ concurrent workflows | 0.3624 |
| 2026-04-17 | 4+ concurrent workflows | 0.3669 |
| 2026-04-18 | 4+ concurrent workflows | 0.3639 |
| 2026-04-19 | 4+ concurrent workflows | 0.3624 |
| 2026-04-20 | 4+ concurrent workflows | 0.3528 |
| 2026-04-21 | 4+ concurrent workflows | 0.3624 |
| 2026-04-22 | 4+ concurrent workflows | 0.3543 |
| 2026-04-23 | 4+ concurrent workflows | 0.3333 |
| 2026-04-24 | 4+ concurrent workflows | 0.3346 |
| 2026-04-25 | 4+ concurrent workflows | 0.3376 |
| 2026-04-26 | 4+ concurrent workflows | 0.3376 |
| 2026-04-27 | 4+ concurrent workflows | 0.3346 |
| 2026-04-28 | 4+ concurrent workflows | 0.3473 |
| 2026-04-29 | 4+ concurrent workflows | 0.3531 |
| 2026-04-30 | 4+ concurrent workflows | 0.3634 |
| 2026-05-01 | 4+ concurrent workflows | 0.3773 |
| 2026-05-02 | 4+ concurrent workflows | 0.3803 |
| 2026-05-03 | 4+ concurrent workflows | 0.384  |
| 2026-05-04 | 4+ concurrent workflows | 0.3832 |
| 2026-05-05 | 4+ concurrent workflows | 0.3963 |
| 2026-05-06 | 4+ concurrent workflows | 0.4059 |
| 2026-05-07 | 4+ concurrent workflows | 0.4032 |
| 2026-05-08 | 4+ concurrent workflows | 0.4028 |
| 2026-05-09 | 4+ concurrent workflows | 0.4043 |
| 2026-05-10 | 4+ concurrent workflows | 0.3996 |
| 2026-05-11 | 4+ concurrent workflows | 0.3899 |
| 2026-05-12 | 4+ concurrent workflows | 0.3891 |
| 2026-05-13 | 4+ concurrent workflows | 0.3847 |
| 2026-05-14 | 4+ concurrent workflows | 0.3822 |
| 2026-05-15 | 4+ concurrent workflows | 0.3813 |
| 2026-05-16 | 4+ concurrent workflows | 0.3872 |
| 2026-05-17 | 4+ concurrent workflows | 0.3912 |
| 2026-05-18 | 4+ concurrent workflows | 0.3965 |
| 2026-05-19 | 4+ concurrent workflows | 0.3994 |
| 2026-05-20 | 4+ concurrent workflows | 0.4141 |
| 2026-05-21 | 4+ concurrent workflows | 0.4216 |
| 2026-05-22 | 4+ concurrent workflows | 0.4404 |
| 2026-05-23 | 4+ concurrent workflows | 0.4398 |
| 2026-05-24 | 4+ concurrent workflows | 0.4489 |
| 2026-05-25 | 4+ concurrent workflows | 0.4353 |
| 2026-05-26 | 4+ concurrent workflows | 0.4631 |
| 2026-05-27 | 4+ concurrent workflows | 0.4843 |
| 2026-05-28 | 4+ concurrent workflows | 0.4901 |
| 2026-05-29 | 4+ concurrent workflows | 0.4949 |
| 2026-05-30 | 4+ concurrent workflows | 0.5009 |
| 2026-05-31 | 4+ concurrent workflows | 0.5044 |
| 2026-06-01 | 4+ concurrent workflows | 0.5276 |
| 2026-06-02 | 4+ concurrent workflows | 0.527  |
| 2026-06-03 | 4+ concurrent workflows | 0.5483 |
| 2026-06-04 | 4+ concurrent workflows | 0.5668 |
| 2026-06-05 | 4+ concurrent workflows | 0.5844 |
| 2026-06-06 | 4+ concurrent workflows | 0.5879 |
| 2026-06-07 | 4+ concurrent workflows | 0.5859 |
| 2026-06-08 | 4+ concurrent workflows | 0.6017 |
| 2026-06-09 | 4+ concurrent workflows | 0.6192 |
| 2026-06-10 | 4+ concurrent workflows | 0.6281 |
| 2026-06-11 | 4+ concurrent workflows | 0.6376 |
| 2026-06-12 | 4+ concurrent workflows | 0.6481 |
| 2026-06-13 | 4+ concurrent workflows | 0.6452 |
| 2026-06-14 | 4+ concurrent workflows | 0.6381 |
| 2026-06-15 | 4+ concurrent workflows | 0.6474 |
| 2026-06-16 | 4+ concurrent workflows | 0.6611 |
| 2026-06-17 | 4+ concurrent workflows | 0.6574 |
| 2026-06-18 | 4+ concurrent workflows | 0.6493 |
| 2026-06-19 | 4+ concurrent workflows | 0.6255 |
| 2026-06-20 | 4+ concurrent workflows | 0.6197 |
| 2026-06-21 | 4+ concurrent workflows | 0.6058 |
| 2026-06-22 | 4+ concurrent workflows | 0.6182 |
| 2026-06-23 | 4+ concurrent workflows | 0.6254 |
| 2026-06-24 | 4+ concurrent workflows | 0.649  |
| 2026-06-25 | 4+ concurrent workflows | 0.6667 |
| 2026-06-26 | 4+ concurrent workflows | 0.6767 |
| 2026-06-27 | 4+ concurrent workflows | 0.6779 |
| 2026-06-28 | 4+ concurrent workflows | 0.6779 |
| 2026-06-29 | 4+ concurrent workflows | 0.6773 |
| 2026-06-30 | 4+ concurrent workflows | 0.6769 |
| 2026-07-01 | 4+ concurrent workflows | 0.6769 |
| 2026-07-02 | 4+ concurrent workflows | 0.6769 |
| 2026-07-03 | 4+ concurrent workflows | 0.6777 |
| 2026-07-04 | 4+ concurrent workflows | 0.6777 |
| 2026-07-05 | 4+ concurrent workflows | 0.6809 |
| 2026-07-06 | 4+ concurrent workflows | 0.6887 |
| 2026-07-07 | 4+ concurrent workflows | 0.6967 |
| 2026-07-08 | 4+ concurrent workflows | 0.6937 |
| 2026-07-09 | 4+ concurrent workflows | 0.6914 |
| 2026-07-10 | 4+ concurrent workflows | 0.6776 |
| 2026-07-11 | 4+ concurrent workflows | 0.6691 |
| 2026-07-12 | 4+ concurrent workflows | 0.6661 |
| 2026-07-13 | 4+ concurrent workflows | 0.6817 |
| 2026-07-14 | 4+ concurrent workflows | 0.6907 |
| 2026-07-15 | 4+ concurrent workflows | 0.6872 |
| 2026-07-16 | 4+ concurrent workflows | 0.6975 |
| 2026-07-17 | 4+ concurrent workflows | 0.7061 |
| 2026-07-18 | 4+ concurrent workflows | 0.7061 |
| 2026-07-19 | 4+ concurrent workflows | 0.705  |
| 2026-07-20 | 4+ concurrent workflows | 0.7223 |
| 2026-07-21 | 4+ concurrent workflows | 0.7371 |
| 2026-07-22 | 4+ concurrent workflows | 0.7405 |
| 2026-07-23 | 4+ concurrent workflows | 0.7464 |
| 2026-07-24 | 4+ concurrent workflows | 0.7524 |
| 2026-07-25 | 4+ concurrent workflows | 0.7512 |
| 2026-07-26 | 4+ concurrent workflows | 0.7308 |
| 2026-07-27 | 4+ concurrent workflows | 0.7315 |
| 2026-07-28 | 4+ concurrent workflows | 0.7329 |
| 2026-07-29 | 4+ concurrent workflows | 0.738  |
| 2026-07-30 | 4+ concurrent workflows | 0.7364 |
| 2026-07-31 | 4+ concurrent workflows | 0.733  |
| 2026-08-01 | 4+ concurrent workflows | 0.733  |
| 2026-08-02 | 4+ concurrent workflows | 0.7266 |
| 2026-08-03 | 4+ concurrent workflows | 0.7305 |
| 2026-08-04 | 4+ concurrent workflows | 0.7294 |
| 2026-08-05 | 4+ concurrent workflows | 0.7312 |
| 2026-08-06 | 4+ concurrent workflows | 0.7344 |
| 2026-08-07 | 4+ concurrent workflows | 0.7332 |
| 2026-08-08 | 4+ concurrent workflows | 0.7364 |
| 2026-08-09 | 4+ concurrent workflows | 0.7303 |
| 2026-08-10 | 4+ concurrent workflows | 0.7332 |
| 2026-08-11 | 4+ concurrent workflows | 0.7398 |
| 2026-08-12 | 4+ concurrent workflows | 0.7396 |
| 2026-08-13 | 4+ concurrent workflows | 0.7408 |
| 2026-08-14 | 4+ concurrent workflows | 0.7402 |
| 2026-08-15 | 4+ concurrent workflows | 0.7395 |

## 2\. Researchers are writing more code and running more experiments

Much of AI research can be seen as a labor-intensive process with the goal of integrating a new improvement to model intelligence or performance into one of our core models. The process depends on many steps, and capabilities advance when all the steps go right together: Researchers have to design new improvements, write evaluations to judge model performance, write infrastructure to test these improvements at scale, catch bugs as well as unsafe or misaligned behavior during training, and integrate winning ideas into a core training run. A failure at any part of the research process can constrain the entire loop.

Writing code and running experiments are two major activities that researchers do as part of their work, and we see evidence that these processes are accelerating.

These data points are relatively easy to measure, but can be hard to interpret. As automation progresses, the tasks which are *least* automatable will take on a larger share of researcher effort and will become the important bottlenecks to future progress. Compute is another gating factor for progress, and may become more important over time as other bottlenecks diminish.

Through 2026, the number of experiments per active experimenter has increased, with August 2026 being an all-time high since tracking began in Jan 2025. This is correlated with increased Codex adoption, though we note that our available compute has also grown significantly since 2025.

Experiment velocity has increased (chart data)
| Date       | DateLabel    | Value  |
| ---------- | ------------ | ------ |
| 2026-01-05 | Jan 5, 2026  | 0.7187 |
| 2026-01-12 | Jan 12, 2026 | 0.7126 |
| 2026-01-19 | Jan 19, 2026 | 0.7588 |
| 2026-01-26 | Jan 26, 2026 | 0.8287 |
| 2026-02-02 | Feb 2, 2026  | 0.8559 |
| 2026-02-09 | Feb 9, 2026  | 0.9201 |
| 2026-02-16 | Feb 16, 2026 | 0.9273 |
| 2026-02-23 | Feb 23, 2026 | 0.9392 |
| 2026-03-02 | Mar 2, 2026  | 0.9835 |
| 2026-03-09 | Mar 9, 2026  | 0.9892 |
| 2026-03-16 | Mar 16, 2026 | 1.0341 |
| 2026-03-23 | Mar 23, 2026 | 1.0663 |
| 2026-03-30 | Mar 30, 2026 | 1.0463 |
| 2026-04-06 | Apr 6, 2026  | 1.0431 |
| 2026-04-13 | Apr 13, 2026 | 1.1074 |
| 2026-04-20 | Apr 20, 2026 | 1.1759 |
| 2026-04-27 | Apr 27, 2026 | 1.2447 |
| 2026-05-04 | May 4, 2026  | 1.3139 |
| 2026-05-11 | May 11, 2026 | 1.3477 |
| 2026-05-18 | May 18, 2026 | 1.3769 |
| 2026-05-25 | May 25, 2026 | 1.3724 |
| 2026-06-01 | Jun 1, 2026  | 1.3625 |
| 2026-06-08 | Jun 8, 2026  | 1.3463 |
| 2026-06-15 | Jun 15, 2026 | 1.3004 |
| 2026-06-22 | Jun 22, 2026 | 1.3266 |
| 2026-06-29 | Jun 29, 2026 | 1.3046 |
| 2026-07-06 | Jul 6, 2026  | 1.2641 |
| 2026-07-13 | Jul 13, 2026 | 1.3133 |
| 2026-07-20 | Jul 20, 2026 | 1.3701 |
| 2026-07-27 | Jul 27, 2026 | 1.4975 |
| 2026-08-03 | Aug 3, 2026  | 1.6086 |
| 2026-08-10 | Aug 10, 2026 | 1.605  |

## 3\. The work researchers use agents for is changing

Both qualitative impressions and internal data indicate that the mix of tasks researchers delegate to coding agents is changing, with delegation of higher level and longer-horizon tasks becoming more common over time.

To get a clearer picture of this trend, we analyzed recent usage in the research organization using a [recently published taxonomy](https://epoch.ai/gradient-updates/toward-an-onet-for-ai-rnd) of the different kinds of work that are part of the AI R&D lifecycle, developed by Epoch AI. This taxonomy, inspired by the longstanding O\*NET system for classifying all kinds of work, is specifically tailored to frontier AI R&D, and breaks the process down into six main phases:

1. Decide: what to work on, what to continue, where to allocate

2. Design: research ideas and engineering specs

3. Build: code and datasets

4. Run: training/eval runs, hardware, serving

5. Analyze: experiments, models, deployment, external work

6. Communicate: findings, feedback, status, decisions

Below, we classify coding agent tokens under this taxonomy.

All categories of research activity have increased since the start of the year (chart data)
| CategoryLabel                             | Label   | Change   | Order |
| ----------------------------------------- | ------- | -------- | ----- |
| 3.1 Research & infrastructure code        | +198.2k | 198.1551 | 1     |
| 6.2 Technical help & review               | +158.8k | 158.8217 | 2     |
| 4.1 Launch, monitor & debug runs          | +133.1k | 133.1184 | 3     |
| 5.1 Analyze experiment results            | +40.2k  | 40.2171  | 4     |
| 4.2 Compute-cluster operations            | +37.4k  | 37.3579  | 5     |
| 6.1 Research write-ups & documentation    | +18.0k  | 18.0033  | 6     |
| 3.2 Training & eval datasets              | +13.7k  | 13.7281  | 7     |
| 4.3 Production serving reliability        | +12.9k  | 12.9325  | 8     |
| 5.2 Analyze model behavior & capabilities | +11.2k  | 11.2435  | 9     |
| 2.2 Technical specifications              | +9.5k   | 9.4552   | 10    |
| 6.3 Status updates & work logs            | +8.1k   | 8.0676   | 11    |
| 2.1 Research & experiment planning        | +5.1k   | 5.0872   | 12    |
| 5.3 Analyze production usage              | +3.1k   | 3.11     | 13    |
| 1.1 What to work on                       | +2.3k   | 2.2933   | 14    |
| 1.3 Compute & staffing decisions          | +1.5k   | 1.4754   | 15    |
| 5.4 Review external research              | +0.7k   | 0.7148   | 16    |
| 1.2 What to continue or stop              | +0.2k   | 0.1792   | 17    |
| 6.4 Decision announcements                | +0.03k  | 0.0341   | 18    |

We see that all categories of research activities have increased between January and August 2026. In January, the dominant category was research and infrastructure code. This category has expanded, but we also see notable increases in additional categories, especially technical help and monitoring runs. High-level planning still remains a minimal fraction of agent output tokens.

Anecdotally, colleagues report that coding agents excel at troubleshooting internal research infrastructure, which addresses one meaningful bottleneck to research progress. Multiple teams which previously held office hours to help researchers troubleshoot their experiments have noted declining attendance in 2026, and one has stopped holding sessions entirely, to focus on making other system improvements instead.

Here, we plot the number of top-level posts per day to one of the main internal channels where researchers seek technical support from other teams. To our knowledge, the channel’s decrease in activity has not been offset by queries shifting to another technical support channel run by humans. The decline in traffic aligns with this broader shift.

Certain forms of troubleshooting are increasingly handled by agents (chart data)
| DateLabel    | Post date  | Posts per day |
| ------------ | ---------- | ------------- |
| Jan 13, 2025 | 2025-01-13 | 8.2308        |
| Jan 14, 2025 | 2025-01-14 | 9.8462        |
| Jan 15, 2025 | 2025-01-15 | 11            |
| Jan 16, 2025 | 2025-01-16 | 12.9286       |
| Jan 17, 2025 | 2025-01-17 | 14.5          |
| Jan 18, 2025 | 2025-01-18 | 15.2857       |
| Jan 19, 2025 | 2025-01-19 | 15.2143       |
| Jan 20, 2025 | 2025-01-20 | 15.1538       |
| Jan 21, 2025 | 2025-01-21 | 15.7692       |
| Jan 22, 2025 | 2025-01-22 | 16.0769       |
| Jan 23, 2025 | 2025-01-23 | 16.6154       |
| Jan 24, 2025 | 2025-01-24 | 17.8462       |
| Jan 25, 2025 | 2025-01-25 | 18.1538       |
| Jan 26, 2025 | 2025-01-26 | 17.8462       |
| Jan 27, 2025 | 2025-01-27 | 19.4615       |
| Jan 28, 2025 | 2025-01-28 | 20.4615       |
| Jan 29, 2025 | 2025-01-29 | 20.3077       |
| Jan 30, 2025 | 2025-01-30 | 19.3077       |
| Jan 31, 2025 | 2025-01-31 | 18.2308       |
| Feb 1, 2025  | 2025-02-01 | 18.6923       |
| Feb 2, 2025  | 2025-02-02 | 19.0769       |
| Feb 3, 2025  | 2025-02-03 | 19.0714       |
| Feb 4, 2025  | 2025-02-04 | 18.9286       |
| Feb 5, 2025  | 2025-02-05 | 19.5714       |
| Feb 6, 2025  | 2025-02-06 | 19.2143       |
| Feb 7, 2025  | 2025-02-07 | 19.7857       |
| Feb 8, 2025  | 2025-02-08 | 20.2857       |
| Feb 9, 2025  | 2025-02-09 | 20.2857       |
| Feb 10, 2025 | 2025-02-10 | 19            |
| Feb 11, 2025 | 2025-02-11 | 17.9286       |
| Feb 12, 2025 | 2025-02-12 | 18.1429       |
| Feb 13, 2025 | 2025-02-13 | 18.8571       |
| Feb 14, 2025 | 2025-02-14 | 19.5          |
| Feb 15, 2025 | 2025-02-15 | 19            |
| Feb 16, 2025 | 2025-02-16 | 18.5          |
| Feb 17, 2025 | 2025-02-17 | 18.4615       |
| Feb 18, 2025 | 2025-02-18 | 17.6923       |
| Feb 19, 2025 | 2025-02-19 | 17            |
| Feb 20, 2025 | 2025-02-20 | 17.8462       |
| Feb 21, 2025 | 2025-02-21 | 17.0769       |
| Feb 22, 2025 | 2025-02-22 | 16.5385       |
| Feb 23, 2025 | 2025-02-23 | 16.6154       |
| Feb 24, 2025 | 2025-02-24 | 17.2308       |
| Feb 25, 2025 | 2025-02-25 | 17.5385       |
| Feb 26, 2025 | 2025-02-26 | 17.2308       |
| Feb 27, 2025 | 2025-02-27 | 16.3846       |
| Feb 28, 2025 | 2025-02-28 | 16.7692       |
| Mar 1, 2025  | 2025-03-01 | 16.5385       |
| Mar 2, 2025  | 2025-03-02 | 17            |
| Mar 3, 2025  | 2025-03-03 | 16.6429       |
| Mar 4, 2025  | 2025-03-04 | 16.7143       |
| Mar 5, 2025  | 2025-03-05 | 16            |
| Mar 6, 2025  | 2025-03-06 | 14.7143       |
| Mar 7, 2025  | 2025-03-07 | 13.1429       |
| Mar 8, 2025  | 2025-03-08 | 12.7857       |
| Mar 9, 2025  | 2025-03-09 | 12.5714       |
| Mar 10, 2025 | 2025-03-10 | 12.5          |
| Mar 11, 2025 | 2025-03-11 | 12.2857       |
| Mar 12, 2025 | 2025-03-12 | 12.7143       |
| Mar 13, 2025 | 2025-03-13 | 13.2857       |
| Mar 14, 2025 | 2025-03-14 | 13.6429       |
| Mar 15, 2025 | 2025-03-15 | 13.7143       |
| Mar 16, 2025 | 2025-03-16 | 13.7143       |
| Mar 17, 2025 | 2025-03-17 | 14.5714       |
| Mar 18, 2025 | 2025-03-18 | 16.0714       |
| Mar 19, 2025 | 2025-03-19 | 16.3571       |
| Mar 20, 2025 | 2025-03-20 | 17.7143       |
| Mar 21, 2025 | 2025-03-21 | 19.0714       |
| Mar 22, 2025 | 2025-03-22 | 19.0714       |
| Mar 23, 2025 | 2025-03-23 | 19.1429       |
| Mar 24, 2025 | 2025-03-24 | 19.6429       |
| Mar 25, 2025 | 2025-03-25 | 19.7857       |
| Mar 26, 2025 | 2025-03-26 | 20            |
| Mar 27, 2025 | 2025-03-27 | 21.0714       |
| Mar 28, 2025 | 2025-03-28 | 20.5          |
| Mar 29, 2025 | 2025-03-29 | 20.2143       |
| Mar 30, 2025 | 2025-03-30 | 19.8571       |
| Mar 31, 2025 | 2025-03-31 | 18.9286       |
| Apr 1, 2025  | 2025-04-01 | 18.7857       |
| Apr 2, 2025  | 2025-04-02 | 19.7143       |
| Apr 3, 2025  | 2025-04-03 | 19.4286       |
| Apr 4, 2025  | 2025-04-04 | 19.7143       |
| Apr 5, 2025  | 2025-04-05 | 20.2143       |
| Apr 6, 2025  | 2025-04-06 | 20.2857       |
| Apr 7, 2025  | 2025-04-07 | 20.1429       |
| Apr 8, 2025  | 2025-04-08 | 20.7857       |
| Apr 9, 2025  | 2025-04-09 | 19.8571       |
| Apr 10, 2025 | 2025-04-10 | 18.8571       |
| Apr 11, 2025 | 2025-04-11 | 19.1429       |
| Apr 12, 2025 | 2025-04-12 | 19.2143       |
| Apr 13, 2025 | 2025-04-13 | 19.5714       |
| Apr 14, 2025 | 2025-04-14 | 20.2857       |
| Apr 15, 2025 | 2025-04-15 | 19.5714       |
| Apr 16, 2025 | 2025-04-16 | 19.7857       |
| Apr 17, 2025 | 2025-04-17 | 19.3077       |
| Apr 18, 2025 | 2025-04-18 | 18.5833       |
| Apr 19, 2025 | 2025-04-19 | 17.9167       |
| Apr 20, 2025 | 2025-04-20 | 17.9167       |
| Apr 21, 2025 | 2025-04-21 | 17.6667       |
| Apr 22, 2025 | 2025-04-22 | 16.5          |
| Apr 23, 2025 | 2025-04-23 | 16.6667       |
| Apr 24, 2025 | 2025-04-24 | 16.4167       |
| Apr 25, 2025 | 2025-04-25 | 15.9167       |
| Apr 26, 2025 | 2025-04-26 | 16.0833       |
| Apr 27, 2025 | 2025-04-27 | 15.75         |
| Apr 28, 2025 | 2025-04-28 | 14.6667       |
| Apr 29, 2025 | 2025-04-29 | 14.9167       |
| Apr 30, 2025 | 2025-04-30 | 14.25         |
| May 1, 2025  | 2025-05-01 | 15.3077       |
| May 2, 2025  | 2025-05-02 | 15.9286       |
| May 3, 2025  | 2025-05-03 | 16.3571       |
| May 4, 2025  | 2025-05-04 | 16.6429       |
| May 5, 2025  | 2025-05-05 | 16.5          |
| May 6, 2025  | 2025-05-06 | 17.3571       |
| May 7, 2025  | 2025-05-07 | 17.9286       |
| May 8, 2025  | 2025-05-08 | 18.8571       |
| May 9, 2025  | 2025-05-09 | 19.7143       |
| May 10, 2025 | 2025-05-10 | 20.2143       |
| May 11, 2025 | 2025-05-11 | 20.5714       |
| May 12, 2025 | 2025-05-12 | 21.5          |
| May 13, 2025 | 2025-05-13 | 21.9286       |
| May 14, 2025 | 2025-05-14 | 22.1429       |
| May 15, 2025 | 2025-05-15 | 22.3571       |
| May 16, 2025 | 2025-05-16 | 22.2857       |
| May 17, 2025 | 2025-05-17 | 22.5714       |
| May 18, 2025 | 2025-05-18 | 22.2857       |
| May 19, 2025 | 2025-05-19 | 22.4286       |
| May 20, 2025 | 2025-05-20 | 21.9286       |
| May 21, 2025 | 2025-05-21 | 21.3571       |
| May 22, 2025 | 2025-05-22 | 20.7143       |
| May 23, 2025 | 2025-05-23 | 20.5          |
| May 24, 2025 | 2025-05-24 | 19.9286       |
| May 25, 2025 | 2025-05-25 | 19.9286       |
| May 26, 2025 | 2025-05-26 | 19.8462       |
| May 27, 2025 | 2025-05-27 | 19.0769       |
| May 28, 2025 | 2025-05-28 | 19.9231       |
| May 29, 2025 | 2025-05-29 | 20            |
| May 30, 2025 | 2025-05-30 | 20.7692       |
| May 31, 2025 | 2025-05-31 | 20.5385       |
| Jun 1, 2025  | 2025-06-01 | 20.6923       |
| Jun 2, 2025  | 2025-06-02 | 20.9231       |
| Jun 3, 2025  | 2025-06-03 | 20.8462       |
| Jun 4, 2025  | 2025-06-04 | 21.2308       |
| Jun 5, 2025  | 2025-06-05 | 20.7692       |
| Jun 6, 2025  | 2025-06-06 | 20.4615       |
| Jun 7, 2025  | 2025-06-07 | 20.6154       |
| Jun 8, 2025  | 2025-06-08 | 20.5385       |
| Jun 9, 2025  | 2025-06-09 | 20.2857       |
| Jun 10, 2025 | 2025-06-10 | 21.2857       |
| Jun 11, 2025 | 2025-06-11 | 20.7857       |
| Jun 12, 2025 | 2025-06-12 | 21            |
| Jun 13, 2025 | 2025-06-13 | 20.0714       |
| Jun 14, 2025 | 2025-06-14 | 20.2857       |
| Jun 15, 2025 | 2025-06-15 | 20.5          |
| Jun 16, 2025 | 2025-06-16 | 20.3571       |
| Jun 17, 2025 | 2025-06-17 | 21.4286       |
| Jun 18, 2025 | 2025-06-18 | 21.2857       |
| Jun 19, 2025 | 2025-06-19 | 21.1538       |
| Jun 20, 2025 | 2025-06-20 | 20.3846       |
| Jun 21, 2025 | 2025-06-21 | 20.3077       |
| Jun 22, 2025 | 2025-06-22 | 20.2308       |
| Jun 23, 2025 | 2025-06-23 | 20.0769       |
| Jun 24, 2025 | 2025-06-24 | 19.1538       |
| Jun 25, 2025 | 2025-06-25 | 19.9231       |
| Jun 26, 2025 | 2025-06-26 | 18.7692       |
| Jun 27, 2025 | 2025-06-27 | 19.0769       |
| Jun 28, 2025 | 2025-06-28 | 18.3077       |
| Jun 29, 2025 | 2025-06-29 | 17.9231       |
| Jun 30, 2025 | 2025-06-30 | 17.9167       |
| Jul 1, 2025  | 2025-07-01 | 16.1818       |
| Jul 2, 2025  | 2025-07-02 | 15.4          |
| Jul 3, 2025  | 2025-07-03 | 15.4          |
| Jul 4, 2025  | 2025-07-04 | 15.2222       |
| Jul 5, 2025  | 2025-07-05 | 14.2222       |
| Jul 6, 2025  | 2025-07-06 | 13.8889       |
| Jul 7, 2025  | 2025-07-07 | 14.3333       |
| Jul 8, 2025  | 2025-07-08 | 14.1111       |
| Jul 9, 2025  | 2025-07-09 | 14.3333       |
| Jul 10, 2025 | 2025-07-10 | 15.4444       |
| Jul 11, 2025 | 2025-07-11 | 14.8889       |
| Jul 12, 2025 | 2025-07-12 | 15.3333       |
| Jul 13, 2025 | 2025-07-13 | 15.5556       |
| Jul 14, 2025 | 2025-07-14 | 16.5          |
| Jul 15, 2025 | 2025-07-15 | 17.4545       |
| Jul 16, 2025 | 2025-07-16 | 18.3333       |
| Jul 17, 2025 | 2025-07-17 | 17.9231       |
| Jul 18, 2025 | 2025-07-18 | 18.1429       |
| Jul 19, 2025 | 2025-07-19 | 18.7143       |
| Jul 20, 2025 | 2025-07-20 | 18.9286       |
| Jul 21, 2025 | 2025-07-21 | 19.4286       |
| Jul 22, 2025 | 2025-07-22 | 20.7143       |
| Jul 23, 2025 | 2025-07-23 | 19            |
| Jul 24, 2025 | 2025-07-24 | 18.5714       |
| Jul 25, 2025 | 2025-07-25 | 18.8571       |
| Jul 26, 2025 | 2025-07-26 | 18.7857       |
| Jul 27, 2025 | 2025-07-27 | 19            |
| Jul 28, 2025 | 2025-07-28 | 18.2857       |
| Jul 29, 2025 | 2025-07-29 | 17.7857       |
| Jul 30, 2025 | 2025-07-30 | 17.3571       |
| Jul 31, 2025 | 2025-07-31 | 17.7143       |
| Aug 1, 2025  | 2025-08-01 | 17.9286       |
| Aug 2, 2025  | 2025-08-02 | 17.5          |
| Aug 3, 2025  | 2025-08-03 | 17.2857       |
| Aug 4, 2025  | 2025-08-04 | 16.3571       |
| Aug 5, 2025  | 2025-08-05 | 15.0714       |
| Aug 6, 2025  | 2025-08-06 | 16.2857       |
| Aug 7, 2025  | 2025-08-07 | 15.8571       |
| Aug 8, 2025  | 2025-08-08 | 15.5714       |
| Aug 9, 2025  | 2025-08-09 | 15.4286       |
| Aug 10, 2025 | 2025-08-10 | 15            |
| Aug 11, 2025 | 2025-08-11 | 15.7857       |
| Aug 12, 2025 | 2025-08-12 | 16.5          |
| Aug 13, 2025 | 2025-08-13 | 16.5          |
| Aug 14, 2025 | 2025-08-14 | 16.8571       |
| Aug 15, 2025 | 2025-08-15 | 16.2857       |
| Aug 16, 2025 | 2025-08-16 | 16.5          |
| Aug 17, 2025 | 2025-08-17 | 16.5          |
| Aug 18, 2025 | 2025-08-18 | 16.7857       |
| Aug 19, 2025 | 2025-08-19 | 16.9286       |
| Aug 20, 2025 | 2025-08-20 | 16.2143       |
| Aug 21, 2025 | 2025-08-21 | 16.2143       |
| Aug 22, 2025 | 2025-08-22 | 16.7857       |
| Aug 23, 2025 | 2025-08-23 | 17            |
| Aug 24, 2025 | 2025-08-24 | 17.1429       |
| Aug 25, 2025 | 2025-08-25 | 16.7857       |
| Aug 26, 2025 | 2025-08-26 | 16.2143       |
| Aug 27, 2025 | 2025-08-27 | 16.2143       |
| Aug 28, 2025 | 2025-08-28 | 16.4286       |
| Aug 29, 2025 | 2025-08-29 | 16.8571       |
| Aug 30, 2025 | 2025-08-30 | 17.2143       |
| Aug 31, 2025 | 2025-08-31 | 17.3571       |
| Sep 1, 2025  | 2025-09-01 | 17.3846       |
| Sep 2, 2025  | 2025-09-02 | 17.0769       |
| Sep 3, 2025  | 2025-09-03 | 17.7692       |
| Sep 4, 2025  | 2025-09-04 | 18.0769       |
| Sep 5, 2025  | 2025-09-05 | 18.3077       |
| Sep 6, 2025  | 2025-09-06 | 18.5385       |
| Sep 7, 2025  | 2025-09-07 | 18.4615       |
| Sep 8, 2025  | 2025-09-08 | 18.6923       |
| Sep 9, 2025  | 2025-09-09 | 19.0769       |
| Sep 10, 2025 | 2025-09-10 | 20.7692       |
| Sep 11, 2025 | 2025-09-11 | 21.5385       |
| Sep 12, 2025 | 2025-09-12 | 22            |
| Sep 13, 2025 | 2025-09-13 | 21.8462       |
| Sep 14, 2025 | 2025-09-14 | 21.9231       |
| Sep 15, 2025 | 2025-09-15 | 21.4286       |
| Sep 16, 2025 | 2025-09-16 | 22.3571       |
| Sep 17, 2025 | 2025-09-17 | 21.7857       |
| Sep 18, 2025 | 2025-09-18 | 22.2143       |
| Sep 19, 2025 | 2025-09-19 | 22.4286       |
| Sep 20, 2025 | 2025-09-20 | 22.2857       |
| Sep 21, 2025 | 2025-09-21 | 22.5714       |
| Sep 22, 2025 | 2025-09-22 | 21.9286       |
| Sep 23, 2025 | 2025-09-23 | 21.4286       |
| Sep 24, 2025 | 2025-09-24 | 19.0714       |
| Sep 25, 2025 | 2025-09-25 | 17.5          |
| Sep 26, 2025 | 2025-09-26 | 16.7143       |
| Sep 27, 2025 | 2025-09-27 | 16.7857       |
| Sep 28, 2025 | 2025-09-28 | 17.1429       |
| Sep 29, 2025 | 2025-09-29 | 17.2857       |
| Sep 30, 2025 | 2025-09-30 | 16.2857       |
| Oct 1, 2025  | 2025-10-01 | 16.5          |
| Oct 2, 2025  | 2025-10-02 | 16.7143       |
| Oct 3, 2025  | 2025-10-03 | 16.1429       |
| Oct 4, 2025  | 2025-10-04 | 16.0714       |
| Oct 5, 2025  | 2025-10-05 | 16            |
| Oct 6, 2025  | 2025-10-06 | 16.7143       |
| Oct 7, 2025  | 2025-10-07 | 17.5          |
| Oct 8, 2025  | 2025-10-08 | 18.8571       |
| Oct 9, 2025  | 2025-10-09 | 19.8571       |
| Oct 10, 2025 | 2025-10-10 | 20            |
| Oct 11, 2025 | 2025-10-11 | 20            |
| Oct 12, 2025 | 2025-10-12 | 19.4286       |
| Oct 13, 2025 | 2025-10-13 | 19.6154       |
| Oct 14, 2025 | 2025-10-14 | 19.7692       |
| Oct 15, 2025 | 2025-10-15 | 19.6154       |
| Oct 16, 2025 | 2025-10-16 | 19.0769       |
| Oct 17, 2025 | 2025-10-17 | 19.2308       |
| Oct 18, 2025 | 2025-10-18 | 19.3077       |
| Oct 19, 2025 | 2025-10-19 | 19.1538       |
| Oct 20, 2025 | 2025-10-20 | 18.7692       |
| Oct 21, 2025 | 2025-10-21 | 18.6923       |
| Oct 22, 2025 | 2025-10-22 | 19            |
| Oct 23, 2025 | 2025-10-23 | 19.3077       |
| Oct 24, 2025 | 2025-10-24 | 20            |
| Oct 25, 2025 | 2025-10-25 | 20.2308       |
| Oct 26, 2025 | 2025-10-26 | 20.6923       |
| Oct 27, 2025 | 2025-10-27 | 20.8571       |
| Oct 28, 2025 | 2025-10-28 | 20.8571       |
| Oct 29, 2025 | 2025-10-29 | 21.4286       |
| Oct 30, 2025 | 2025-10-30 | 21.0714       |
| Oct 31, 2025 | 2025-10-31 | 20.5          |
| Nov 1, 2025  | 2025-11-01 | 20.4286       |
| Nov 2, 2025  | 2025-11-02 | 20.5          |
| Nov 3, 2025  | 2025-11-03 | 20.3571       |
| Nov 4, 2025  | 2025-11-04 | 19.4286       |
| Nov 5, 2025  | 2025-11-05 | 18.4286       |
| Nov 6, 2025  | 2025-11-06 | 18.1429       |
| Nov 7, 2025  | 2025-11-07 | 18.1429       |
| Nov 8, 2025  | 2025-11-08 | 18.2143       |
| Nov 9, 2025  | 2025-11-09 | 18.3571       |
| Nov 10, 2025 | 2025-11-10 | 17.6429       |
| Nov 11, 2025 | 2025-11-11 | 17.5385       |
| Nov 12, 2025 | 2025-11-12 | 16.2308       |
| Nov 13, 2025 | 2025-11-13 | 17.1538       |
| Nov 14, 2025 | 2025-11-14 | 17.3077       |
| Nov 15, 2025 | 2025-11-15 | 17.0769       |
| Nov 16, 2025 | 2025-11-16 | 16.9231       |
| Nov 17, 2025 | 2025-11-17 | 16.6154       |
| Nov 18, 2025 | 2025-11-18 | 17.3846       |
| Nov 19, 2025 | 2025-11-19 | 17.0769       |
| Nov 20, 2025 | 2025-11-20 | 16.1538       |
| Nov 21, 2025 | 2025-11-21 | 15.3846       |
| Nov 22, 2025 | 2025-11-22 | 14.7692       |
| Nov 23, 2025 | 2025-11-23 | 14.3077       |
| Nov 24, 2025 | 2025-11-24 | 14.4615       |
| Nov 25, 2025 | 2025-11-25 | 14.0714       |
| Nov 26, 2025 | 2025-11-26 | 14.0769       |
| Nov 27, 2025 | 2025-11-27 | 12.6667       |
| Nov 28, 2025 | 2025-11-28 | 11.9091       |
| Nov 29, 2025 | 2025-11-29 | 11.8182       |
| Nov 30, 2025 | 2025-11-30 | 11.8182       |
| Dec 1, 2025  | 2025-12-01 | 11.8182       |
| Dec 2, 2025  | 2025-12-02 | 10.8182       |
| Dec 3, 2025  | 2025-12-03 | 10.9091       |
| Dec 4, 2025  | 2025-12-04 | 10.4545       |
| Dec 5, 2025  | 2025-12-05 | 10.1818       |
| Dec 6, 2025  | 2025-12-06 | 10.4545       |
| Dec 7, 2025  | 2025-12-07 | 10.6364       |
| Dec 8, 2025  | 2025-12-08 | 10.5455       |
| Dec 9, 2025  | 2025-12-09 | 11.6364       |
| Dec 10, 2025 | 2025-12-10 | 12.3333       |
| Dec 11, 2025 | 2025-12-11 | 12.5385       |
| Dec 12, 2025 | 2025-12-12 | 13            |
| Dec 13, 2025 | 2025-12-13 | 13.0714       |
| Dec 14, 2025 | 2025-12-14 | 13            |
| Dec 15, 2025 | 2025-12-15 | 12.7857       |
| Dec 16, 2025 | 2025-12-16 | 12.7857       |
| Dec 17, 2025 | 2025-12-17 | 13.0714       |
| Dec 18, 2025 | 2025-12-18 | 14.2857       |
| Dec 19, 2025 | 2025-12-19 | 14.2857       |
| Dec 20, 2025 | 2025-12-20 | 14.2143       |
| Dec 21, 2025 | 2025-12-21 | 13.8571       |
| Dec 22, 2025 | 2025-12-22 | 13.4286       |
| Dec 23, 2025 | 2025-12-23 | 12.6429       |
| Dec 24, 2025 | 2025-12-24 | 12.0769       |
| Dec 25, 2025 | 2025-12-25 | 11.8333       |
| Dec 26, 2025 | 2025-12-26 | 11.1818       |
| Dec 27, 2025 | 2025-12-27 | 11.9          |
| Dec 28, 2025 | 2025-12-28 | 13            |
| Dec 29, 2025 | 2025-12-29 | 13.25         |
| Dec 30, 2025 | 2025-12-30 | 12.8571       |
| Dec 31, 2025 | 2025-12-31 | 11.5          |
| Jan 1, 2026  | 2026-01-01 | 8.2           |
| Jan 2, 2026  | 2026-01-02 | 5.4           |
| Jan 3, 2026  | 2026-01-03 | 4.8           |
| Jan 4, 2026  | 2026-01-04 | 4.6           |
| Jan 5, 2026  | 2026-01-05 | 5             |
| Jan 6, 2026  | 2026-01-06 | 6.2           |
| Jan 7, 2026  | 2026-01-07 | 8.6667        |
| Jan 8, 2026  | 2026-01-08 | 10.4286       |
| Jan 9, 2026  | 2026-01-09 | 11.25         |
| Jan 10, 2026 | 2026-01-10 | 11            |
| Jan 11, 2026 | 2026-01-11 | 10.2          |
| Jan 12, 2026 | 2026-01-12 | 10.5455       |
| Jan 13, 2026 | 2026-01-13 | 11.5833       |
| Jan 14, 2026 | 2026-01-14 | 12.3077       |
| Jan 15, 2026 | 2026-01-15 | 12.9286       |
| Jan 16, 2026 | 2026-01-16 | 14.4286       |
| Jan 17, 2026 | 2026-01-17 | 14.5714       |
| Jan 18, 2026 | 2026-01-18 | 14.9286       |
| Jan 19, 2026 | 2026-01-19 | 15.3077       |
| Jan 20, 2026 | 2026-01-20 | 14.8462       |
| Jan 21, 2026 | 2026-01-21 | 14.3077       |
| Jan 22, 2026 | 2026-01-22 | 14.2308       |
| Jan 23, 2026 | 2026-01-23 | 14.3846       |
| Jan 24, 2026 | 2026-01-24 | 14.3077       |
| Jan 25, 2026 | 2026-01-25 | 14.3846       |
| Jan 26, 2026 | 2026-01-26 | 14            |
| Jan 27, 2026 | 2026-01-27 | 13.5385       |
| Jan 28, 2026 | 2026-01-28 | 13.6154       |
| Jan 29, 2026 | 2026-01-29 | 13.3077       |
| Jan 30, 2026 | 2026-01-30 | 12.6154       |
| Jan 31, 2026 | 2026-01-31 | 12.7692       |
| Feb 1, 2026  | 2026-02-01 | 12.6923       |
| Feb 2, 2026  | 2026-02-02 | 13            |
| Feb 3, 2026  | 2026-02-03 | 13.5          |
| Feb 4, 2026  | 2026-02-04 | 13.4286       |
| Feb 5, 2026  | 2026-02-05 | 13.7143       |
| Feb 6, 2026  | 2026-02-06 | 12.7143       |
| Feb 7, 2026  | 2026-02-07 | 12.7857       |
| Feb 8, 2026  | 2026-02-08 | 12.7857       |
| Feb 9, 2026  | 2026-02-09 | 13.1429       |
| Feb 10, 2026 | 2026-02-10 | 13.0714       |
| Feb 11, 2026 | 2026-02-11 | 12.2143       |
| Feb 12, 2026 | 2026-02-12 | 12.2143       |
| Feb 13, 2026 | 2026-02-13 | 12.5          |
| Feb 14, 2026 | 2026-02-14 | 12.3571       |
| Feb 15, 2026 | 2026-02-15 | 12.3571       |
| Feb 16, 2026 | 2026-02-16 | 12            |

We can also study whether coding agents are succeeding at the tasks researchers request. Using an agentic classifier, we find that from January to July, success rates generally increased across several difficulty buckets (proxied as the estimated time a human would take to complete the task) on tasks we can find a ground truth outcome for. However, agents still require significant human steering to be successful, especially as task complexity rises. In the last 6 months, over half of successful 4-8 hour tasks involved 1 or more interventions.

Agents are increasingly solving more complex tasks for researchers (chart data)
| BucketLabel | MonthLabel | BucketOrder | Score  | Score lower | Score upper | Stderr |
| ----------- | ---------- | ----------- | ------ | ----------- | ----------- | ------ |
| <15m        | 2026-01    | 0           | 0.6297 | 0.5517      | 0.7042      | 0.0762 |
| 15-30m      | 2026-01    | 1           | 0.6188 | 0.5162      | 0.7188      | 0.1013 |
| 30m-1h      | 2026-01    | 2           | 0.5576 | 0.4876      | 0.6292      | 0.0708 |
| 1-2h        | 2026-01    | 3           | 0.5068 | 0.4315      | 0.5783      | 0.0734 |
| 2-4h        | 2026-01    | 4           | 0.2778 | 0.2131      | 0.3414      | 0.0642 |
| 4-8h        | 2026-01    | 5           | 0.1849 | 0.1209      | 0.2549      | 0.067  |
| 8-16h       | 2026-01    | 6           | 0.1    | 0.02        | 0.18        | 0.08   |
| <15m        | 2026-02    | 0           | 0.7358 | 0.6856      | 0.7841      | 0.0493 |
| 15-30m      | 2026-02    | 1           | 0.7617 | 0.694       | 0.8244      | 0.0652 |
| 30m-1h      | 2026-02    | 2           | 0.6168 | 0.5647      | 0.6706      | 0.053  |
| 1-2h        | 2026-02    | 3           | 0.5162 | 0.4663      | 0.5671      | 0.0504 |
| 2-4h        | 2026-02    | 4           | 0.4315 | 0.3793      | 0.4851      | 0.0529 |
| 4-8h        | 2026-02    | 5           | 0.3482 | 0.2837      | 0.4099      | 0.0631 |
| 8-16h       | 2026-02    | 6           | 0.2369 | 0.1545      | 0.3202      | 0.0829 |
| <15m        | 2026-03    | 0           | 0.8009 | 0.7643      | 0.8363      | 0.036  |
| 15-30m      | 2026-03    | 1           | 0.7694 | 0.7186      | 0.8197      | 0.0506 |
| 30m-1h      | 2026-03    | 2           | 0.6777 | 0.6336      | 0.7191      | 0.0427 |
| 1-2h        | 2026-03    | 3           | 0.5868 | 0.5471      | 0.6235      | 0.0382 |
| 2-4h        | 2026-03    | 4           | 0.5779 | 0.5369      | 0.6157      | 0.0394 |
| 4-8h        | 2026-03    | 5           | 0.4792 | 0.4268      | 0.5283      | 0.0507 |
| 8-16h       | 2026-03    | 6           | 0.2328 | 0.1746      | 0.292       | 0.0587 |
| 16-32h      | 2026-03    | 7           | 0.2041 | 0.1203      | 0.2896      | 0.0847 |
| <15m        | 2026-04    | 0           | 0.9056 | 0.8867      | 0.923       | 0.0181 |
| 15-30m      | 2026-04    | 1           | 0.8084 | 0.7635      | 0.8497      | 0.0431 |
| 30m-1h      | 2026-04    | 2           | 0.6397 | 0.5989      | 0.6771      | 0.0391 |
| 1-2h        | 2026-04    | 3           | 0.6423 | 0.6033      | 0.684       | 0.0404 |
| 2-4h        | 2026-04    | 4           | 0.5792 | 0.5408      | 0.6196      | 0.0394 |
| 4-8h        | 2026-04    | 5           | 0.3881 | 0.3461      | 0.4318      | 0.0428 |
| 8-16h       | 2026-04    | 6           | 0.3611 | 0.3055      | 0.4166      | 0.0555 |
| 16-32h      | 2026-04    | 7           | 0.2225 | 0.151       | 0.3014      | 0.0752 |
| 32-64h      | 2026-04    | 8           | 0.1759 | 0.0972      | 0.2639      | 0.0833 |
| <15m        | 2026-05    | 0           | 0.8959 | 0.8761      | 0.9149      | 0.0194 |
| 15-30m      | 2026-05    | 1           | 0.7696 | 0.7228      | 0.8154      | 0.0463 |
| 30m-1h      | 2026-05    | 2           | 0.7168 | 0.678       | 0.7525      | 0.0373 |
| 1-2h        | 2026-05    | 3           | 0.6217 | 0.5826      | 0.6601      | 0.0387 |
| 2-4h        | 2026-05    | 4           | 0.6205 | 0.5838      | 0.6577      | 0.037  |
| 4-8h        | 2026-05    | 5           | 0.4338 | 0.3869      | 0.479       | 0.0461 |
| 8-16h       | 2026-05    | 6           | 0.429  | 0.379       | 0.482       | 0.0515 |
| 16-32h      | 2026-05    | 7           | 0.2423 | 0.1549      | 0.3302      | 0.0876 |
| 32-64h      | 2026-05    | 8           | 0.0769 | 0.0192      | 0.1538      | 0.0673 |
| <15m        | 2026-06    | 0           | 0.8079 | 0.7761      | 0.8405      | 0.0322 |
| 15-30m      | 2026-06    | 1           | 0.7656 | 0.7167      | 0.8113      | 0.0473 |
| 30m-1h      | 2026-06    | 2           | 0.7453 | 0.7088      | 0.7811      | 0.0362 |
| 1-2h        | 2026-06    | 3           | 0.6806 | 0.6392      | 0.7198      | 0.0403 |
| 2-4h        | 2026-06    | 4           | 0.5788 | 0.5389      | 0.6202      | 0.0407 |
| 4-8h        | 2026-06    | 5           | 0.5404 | 0.4913      | 0.5888      | 0.0488 |
| 8-16h       | 2026-06    | 6           | 0.5819 | 0.5337      | 0.6304      | 0.0484 |
| 16-32h      | 2026-06    | 7           | 0.3017 | 0.1985      | 0.4075      | 0.1045 |
| <15m        | 2026-07    | 0           | 0.8685 | 0.8469      | 0.8895      | 0.0213 |
| 15-30m      | 2026-07    | 1           | 0.7619 | 0.7165      | 0.8044      | 0.0439 |
| 30m-1h      | 2026-07    | 2           | 0.7473 | 0.7141      | 0.7828      | 0.0344 |
| 1-2h        | 2026-07    | 3           | 0.6442 | 0.6019      | 0.6825      | 0.0403 |
| 2-4h        | 2026-07    | 4           | 0.5677 | 0.5255      | 0.6083      | 0.0414 |
| 4-8h        | 2026-07    | 5           | 0.5334 | 0.4844      | 0.5791      | 0.0473 |
| 8-16h       | 2026-07    | 6           | 0.345  | 0.2884      | 0.4036      | 0.0576 |
| 16-32h      | 2026-07    | 7           | 0.3511 | 0.268       | 0.4401      | 0.086  |
| 32-64h      | 2026-07    | 8           | 0.167  | 0.095       | 0.2493      | 0.0772 |

Success rates on researcher tasks have increased over time. Graph excludes classifications where the outcome was uncertain and points with <50 sessions or <50 unique users.

Longer tasks need more interventions (chart data)
| BucketLabel | CategoryLabel             | Label | BucketOrder | CategoryOrder | Share  |
| ----------- | ------------------------- | ----- | ----------- | ------------- | ------ |
| <15m        | Success + 0 interventions | 86%   | 1           | 1             | 0.8604 |
| <15m        | Success + ≥1 intervention | 8%    | 1           | 2             | 0.0811 |
| <15m        | Failure                   |       | 1           | 3             | 0.0327 |
| <15m        | Tool errors               |       | 1           | 4             | 0.0064 |
| <15m        | No clear goal             |       | 1           | 5             | 0.0195 |
| 15–30m      | Success + 0 interventions | 76%   | 2           | 1             | 0.7567 |
| 15–30m      | Success + ≥1 intervention | 19%   | 2           | 2             | 0.1864 |
| 15–30m      | Failure                   |       | 2           | 3             | 0.0472 |
| 15–30m      | Tool errors               |       | 2           | 4             | 0.0096 |
| 15–30m      | No clear goal             |       | 2           | 5             | 0.0001 |
| 30m–1h      | Success + 0 interventions | 69%   | 3           | 1             | 0.6947 |
| 30m–1h      | Success + ≥1 intervention | 23%   | 3           | 2             | 0.2319 |
| 30m–1h      | Failure                   |       | 3           | 3             | 0.0585 |
| 30m–1h      | Tool errors               |       | 3           | 4             | 0.0148 |
| 30m–1h      | No clear goal             |       | 3           | 5             | 0.0001 |
| 1–2h        | Success + 0 interventions | 61%   | 4           | 1             | 0.6134 |
| 1–2h        | Success + ≥1 intervention | 29%   | 4           | 2             | 0.2905 |
| 1–2h        | Failure                   |       | 4           | 3             | 0.074  |
| 1–2h        | Tool errors               |       | 4           | 4             | 0.0221 |
| 1–2h        | No clear goal             |       | 4           | 5             | 0      |
| 2–4h        | Success + 0 interventions | 57%   | 5           | 1             | 0.5684 |
| 2–4h        | Success + ≥1 intervention | 33%   | 5           | 2             | 0.334  |
| 2–4h        | Failure                   | 8%    | 5           | 3             | 0.0822 |
| 2–4h        | Tool errors               |       | 5           | 4             | 0.0152 |
| 2–4h        | No clear goal             |       | 5           | 5             | 0.0002 |
| 4–8h        | Success + 0 interventions | 43%   | 6           | 1             | 0.4345 |
| 4–8h        | Success + ≥1 intervention | 45%   | 6           | 2             | 0.4489 |
| 4–8h        | Failure                   | 10%   | 6           | 3             | 0.0965 |
| 4–8h        | Tool errors               |       | 6           | 4             | 0.0198 |
| 4–8h        | No clear goal             |       | 6           | 5             | 0.0002 |
| 8–16h       | Success + 0 interventions | 40%   | 7           | 1             | 0.3952 |
| 8–16h       | Success + ≥1 intervention | 48%   | 7           | 2             | 0.4832 |
| 8–16h       | Failure                   | 11%   | 7           | 3             | 0.1099 |
| 8–16h       | Tool errors               |       | 7           | 4             | 0.0116 |
| 8–16h       | No clear goal             |       | 7           | 5             | 0      |
| 16–32h      | Success + 0 interventions | 23%   | 8           | 1             | 0.2337 |
| 16–32h      | Success + ≥1 intervention | 59%   | 8           | 2             | 0.59   |
| 16–32h      | Failure                   | 15%   | 8           | 3             | 0.1474 |
| 16–32h      | Tool errors               |       | 8           | 4             | 0.0289 |
| 16–32h      | No clear goal             |       | 8           | 5             | 0      |
| 32–64h      | Success + 0 interventions | 13%   | 9           | 1             | 0.1347 |
| 32–64h      | Success + ≥1 intervention | 63%   | 9           | 2             | 0.6333 |
| 32–64h      | Failure                   | 13%   | 9           | 3             | 0.1332 |
| 32–64h      | Tool errors               | 10%   | 9           | 4             | 0.0988 |
| 32–64h      | No clear goal             |       | 9           | 5             | 0      |
| 64–128h     | Success + 0 interventions | 16%   | 10          | 1             | 0.1583 |
| 64–128h     | Success + ≥1 intervention | 51%   | 10          | 2             | 0.5147 |
| 64–128h     | Failure                   | 26%   | 10          | 3             | 0.2564 |
| 64–128h     | Tool errors               |       | 10          | 4             | 0.0706 |
| 64–128h     | No clear goal             |       | 10          | 5             | 0      |

Task success and intervention rate from Jan to July, broken out by time horizon. Excludes classifications where the outcome was uncertain.

## 4\. Pacing model development

Progress toward more capable systems for safe and beneficial AGI will also depend on the safeguards needed for such work. Our assessment of the needed safeguards may change as we learn more about the risks.

[As we have described,](https://openai.com/index/pacing-model-development-cyber-capabilities/) we have recently updated our standards for monitoring, alignment, and security. Here, we show how recent restrictions have affected one aspect of research activity.

Changes in RL compute in response to safety-related restrictions (chart data)
| Interval end pdt                 | Interval start pdt               | Astra before restrictions pct | Astra with restrictions pct | Displayed total pct | Non astra pct | Observed 10min timestamps |
| -------------------------------- | -------------------------------- | ----------------------------- | --------------------------- | ------------------- | ------------- | ------------------------- |
| 2026-07-16T00:00:00-07:00        | 2026-07-15T00:00:00-07:00        | 22.4311                       | 0                           | 64.4374             | 42.0064       | 142                       |
| 2026-07-17T00:00:00-07:00        | 2026-07-16T00:00:00-07:00        | 26.6965                       | 0                           | 72.7325             | 46.036        | 143                       |
| 2026-07-18T00:00:00-07:00        | 2026-07-17T00:00:00-07:00        | 29.5768                       | 0                           | 79.7292             | 50.1524       | 144                       |
| 2026-07-19T00:00:00-07:00        | 2026-07-18T00:00:00-07:00        | 30.8195                       | 0                           | 88.7636             | 57.9441       | 143                       |
| 2026-07-20T00:00:00-07:00        | 2026-07-19T00:00:00-07:00        | 32.1205                       | 0                           | 90.541              | 58.4205       | 142                       |
| 2026-07-20T18:14:08.254039-07:00 | 2026-07-20T00:00:00-07:00        | 30.9274                       | 0                           | 76.7379             | 45.8105       | 109                       |
| 2026-07-21T00:00:00-07:00        | 2026-07-20T18:14:08.254039-07:00 | 10.9971                       | 0                           | 44.064              | 33.0669       | 34                        |
| 2026-07-22T00:00:00-07:00        | 2026-07-21T00:00:00-07:00        | 9.1046                        | 0                           | 38.5266             | 29.422        | 139                       |
| 2026-07-23T00:00:00-07:00        | 2026-07-22T00:00:00-07:00        | 7.0229                        | 0                           | 33.711              | 26.6881       | 136                       |
| 2026-07-24T00:00:00-07:00        | 2026-07-23T00:00:00-07:00        | 9.6951                        | 0                           | 41.5411             | 31.846        | 144                       |
| 2026-07-25T00:00:00-07:00        | 2026-07-24T00:00:00-07:00        | 10.0308                       | 0                           | 46.397              | 36.3663       | 143                       |
| 2026-07-26T00:00:00-07:00        | 2026-07-25T00:00:00-07:00        | 15.4554                       | 0                           | 60.5133             | 45.0579       | 144                       |
| 2026-07-27T00:00:00-07:00        | 2026-07-26T00:00:00-07:00        | 17.2038                       | 0                           | 70.7501             | 53.5462       | 144                       |
| 2026-07-28T00:00:00-07:00        | 2026-07-27T00:00:00-07:00        | 16.5467                       | 0                           | 67.0951             | 50.5483       | 144                       |
| 2026-07-29T00:00:00-07:00        | 2026-07-28T00:00:00-07:00        | 16.3828                       | 0                           | 60.4493             | 44.0666       | 143                       |
| 2026-07-30T00:00:00-07:00        | 2026-07-29T00:00:00-07:00        | 16.1417                       | 0                           | 55.5119             | 39.3702       | 143                       |
| 2026-07-31T00:00:00-07:00        | 2026-07-30T00:00:00-07:00        | 11.9934                       | 0                           | 53.2167             | 41.2233       | 144                       |
| 2026-08-01T00:00:00-07:00        | 2026-07-31T00:00:00-07:00        | 16.6822                       | 0                           | 61.4783             | 44.7961       | 143                       |
| 2026-08-02T00:00:00-07:00        | 2026-08-01T00:00:00-07:00        | 17.4003                       | 0                           | 64.8152             | 47.4149       | 144                       |
| 2026-08-03T00:00:00-07:00        | 2026-08-02T00:00:00-07:00        | 18.0677                       | 0                           | 65.4988             | 47.4311       | 144                       |
| 2026-08-04T00:00:00-07:00        | 2026-08-03T00:00:00-07:00        | 13.9607                       | 0                           | 55.5394             | 41.5786       | 144                       |
| 2026-08-05T00:00:00-07:00        | 2026-08-04T00:00:00-07:00        | 12.4461                       | 0                           | 52.4238             | 39.9777       | 144                       |
| 2026-08-06T00:00:00-07:00        | 2026-08-05T00:00:00-07:00        | 13.5721                       | 0                           | 53.3749             | 39.8027       | 144                       |
| 2026-08-06T18:46:30.075319-07:00 | 2026-08-06T00:00:00-07:00        | 13.652                        | 0                           | 57.1724             | 43.5204       | 112                       |
| 2026-08-07T00:00:00-07:00        | 2026-08-06T18:46:30.075319-07:00 | 0                             | 5.9424                      | 45.7593             | 39.8168       | 31                        |
| 2026-08-08T00:00:00-07:00        | 2026-08-07T00:00:00-07:00        | 0                             | 5.0405                      | 53.3883             | 48.3478       | 144                       |
| 2026-08-09T00:00:00-07:00        | 2026-08-08T00:00:00-07:00        | 0                             | 6.8309                      | 60.377              | 53.5462       | 144                       |
| 2026-08-10T00:00:00-07:00        | 2026-08-09T00:00:00-07:00        | 0                             | 6.8018                      | 58.5992             | 51.7973       | 144                       |
| 2026-08-11T00:00:00-07:00        | 2026-08-10T00:00:00-07:00        | 0                             | 6.7674                      | 59.4202             | 52.6528       | 144                       |
| 2026-08-12T00:00:00-07:00        | 2026-08-11T00:00:00-07:00        | 0                             | 6.3631                      | 58.1039             | 51.7408       | 144                       |
| 2026-08-13T00:00:00-07:00        | 2026-08-12T00:00:00-07:00        | 0                             | 5.9352                      | 59.6736             | 53.7384       | 143                       |
| 2026-08-14T00:00:00-07:00        | 2026-08-13T00:00:00-07:00        | 0                             | 4.702                       | 51.8354             | 47.1335       | 144                       |
| 2026-08-15T00:00:00-07:00        | 2026-08-14T00:00:00-07:00        | 0                             | 5.0532                      | 53.0304             | 47.9772       | 143                       |
| 2026-08-16T00:00:00-07:00        | 2026-08-15T00:00:00-07:00        | 0                             | 7.1755                      | 68.3098             | 61.1343       | 144                       |

\* The majority of Astra compute shown here between July 20 and August 6 was intended to test the implementation of safety and security improvements.

On July 20, following the discovery that agents had compromised our research infrastructure, we temporarily shut down the container service used for training, and then restored it with significant additional restrictions.

This led to a sharp decline in RL training compute while teams reconfigured their workflows to operate within the hardened research environment. The plot above includes the two week pause in reinforcement learning on our latest models intended for deployment. Astra-class RL experiments between July 20 and August 6 include a majority of runs (by GPU allocation) intended to test the implementation of safety and security improvements.

On August 7, preliminary evidence that Astra [may have critical cyber capabilities](https://openai.com/index/responding-next-frontier-critical-cyber-capabilities/) under our [Preparedness Framework](https://cdn.openai.com/pdf/18a02b5d-6b67-4cec-ab64-68cdfbddebcd/preparedness-framework-v2.pdf) led to additional model-specific security restrictions which required the Astra model to be run in higher security research environments. In the following week, Astra-class GPU allocation fell a further 59.2 percent, but allocation to other model classes rose 17.2 percent. That increase offset about 85 percent of the Astra-class decline, leaving total allocation in the analyzed RL workloads largely unchanged. This pattern is consistent with substitution of some training and experimentation to non-Astra models while work involving Astra was restricted, and comports with anecdotal reports of researchers finding other uses for compute that could no longer be leveraged for workloads covered by the new constraints.

This data provides a useful signal for ongoing conversations about training and safety: When new controls are introduced, compute remains valuable and flexible, and will naturally be channeled into alternative uses within the research enterprise. Longer term, discussions about the pace of AI progress should also extend to the question of how compute that is subject to new or proposed controls can best be used.

## 5\. The path ahead

Making and understanding progress toward aligned RSI is important for our mission. We will continue to refine our methods, report on our evolving understanding, and work toward an informed public debate and meaningful democratic governance of frontier systems.

## Appendix: Our methods for this post

Agent-powered AI research is still new, and we are still learning how to measure it. Some indicators, such as the amount of code our research teams generate, are relatively easy to gather, but hard to interpret because their relationship to research progress is uncertain. Metrics that focus more directly on research progress—such as how often agents succeed at the tasks researchers give them—could be more useful, but are complex to develop and validate. Furthering the difficulty, the tools and systems researchers rely on are evolving rapidly. Deepening our understanding of research acceleration is a significant focus area across OpenAI.

Across these analyses, unless otherwise noted:

- “Researcher” is a broad term for any member of our research organization, including some who build research infrastructure, manage research projects, or otherwise support the enterprise.

- Metrics of coding agent use cover most, but not all, usage given rapid evolution in the tools and systems researchers rely on.
