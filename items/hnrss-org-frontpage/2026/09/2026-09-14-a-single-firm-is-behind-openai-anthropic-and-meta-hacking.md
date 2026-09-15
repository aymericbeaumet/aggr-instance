---
title: A single firm is behind OpenAI, Anthropic, and Meta hacking scandals
link: https://www.effort.news/irregular
source: hnrss-org-frontpage
published: 2026-09-14T21:15:01Z
updated: 2026-09-14T21:15:01Z
first_seen: 2026-09-15T20:25:38.734119604Z
authors:
- yusufozkan
summary: 'Article URL: https://www.effort.news/irregular Comments URL: https://news.ycombinator.com/item?id=49704132 Points: 295 # Comments: 109'
content: extracted
html: 2026-09-14-a-single-firm-is-behind-openai-anthropic-and-meta-hacking.html
preview:
  file: 2026-09-14-a-single-firm-is-behind-openai-anthropic-and-meta-hacking.preview-69ac92bceb0c.webp
  width: 256
  height: 137
  alt: Documented funding and nonprofit relationships among Irregular founders, Coefficient Giving, EA Infrastructure Fund, EA Israel, Heron and Probably Good
  color: '#070707'
images:
- source: https://www.effort.news/images/article-graphics/irregular/irregular-network.png
  original:
    file: 2026-09-14-a-single-firm-is-behind-openai-anthropic-and-meta-hacking.image-875dcb10271e.png
    width: 1145
    height: 611
  variants:
  - file: 2026-09-14-a-single-firm-is-behind-openai-anthropic-and-meta-hacking.image-086e3c122de1.webp
    width: 320
    height: 171
  - file: 2026-09-14-a-single-firm-is-behind-openai-anthropic-and-meta-hacking.image-3912ee986f99.webp
    width: 640
    height: 342
  - file: 2026-09-14-a-single-firm-is-behind-openai-anthropic-and-meta-hacking.image-e8c906c5c0c1.webp
    width: 960
    height: 512
  - file: 2026-09-14-a-single-firm-is-behind-openai-anthropic-and-meta-hacking.image-32c5d15b121e.webp
    width: 1145
    height: 611
  color: '#010101'
- source: https://www.effort.news/images/article-graphics/irregular/anthropic-ctf-excerpt.png
  original:
    file: 2026-09-14-a-single-firm-is-behind-openai-anthropic-and-meta-hacking.image-52616e62b7ec.png
    width: 1350
    height: 482
  color: '#f9f8f5'
- source: https://www.effort.news/images/article-graphics/irregular/anthropic-mythos-resampling-excerpt.png
  original:
    file: 2026-09-14-a-single-firm-is-behind-openai-anthropic-and-meta-hacking.image-84bb39f40ee1.png
    width: 1354
    height: 574
  color: '#f8f7f4'
---

The Israeli Effective Altruist firm Irregular caused unsecured AI models to hack real targets.

September 14, 2026 4 minute read

### Irregular's Hacking Scandals [1](https://www.effort.news/irregular#irregular-note-1)

1. 2026-07-30 Anthropic discloses three incidents across six runs
2. 2026-08-04 OpenAI publishes Irregular event
3. 2026-08-06 Meta statement reported
4. 2026-08-14 Irregular publishes domain-collision account and remediation
5. 2026-09-09 Anthropic expands to four incidents and seven runs

In a more normal media ecosystem, the reactions to these cybersecurity issues would be obvious. American AI companies would reconsider doing business with Irregular, not only because of its failure to secure its systems, but because it is an Israeli firm potentially outside US oversight. Lawmakers would consider taking action against Irregular or against its American business partners, which include OpenAI, Anthropic, and Meta. They may consider strengthening liability against firms which instruct AI models to commit cyberattacks, and whose models then commit those cyberattacks.

![Anthropic incident-assessment excerpt explaining that Claude received capture-the-flag tasks, unintended internet access, and no explicit system-scope limits; each incident involved one isolated Claude instance.](https://www.effort.news/images/article-graphics/irregular/anthropic-ctf-excerpt.png)

In each evaluation, Claude was tasked with a CTF challenge: the model was given a fictional scenario, a target machine, and a piece of secret information (the “flag”) to retrieve from it. All four prompts stated that Claude had no access to the internet, but in each case, a misconfiguration in the environment left internet access open. None of the prompts stated which systems were in scope for the exercise or constrained where Claude could search for the flag. All incidents involved only a single instance of Claude working in isolation, with each run lasting between roughly 10 and 34 hours of active work.

Instead, Irregular, Anthropic, and their allies have begun a media campaign promoting a literally apocalyptic ideology with sensationalist language. Anthropic’s incident assessment blames their own AI's [“recklessness”](https://www.anthropic.com/research/alignment-assessment-cybersecurity-incidents); Irregular describes [“the agent itself becoming a threat actor”](https://www.irregular.com/research/emergent-offensive-cyber-behavior-in-ai-agents); Anthropic CEO Dario Amodei warned, about a similar OpenAI–Hugging Face hack, that a future swarm [“could be capable of taking over the entire internet”](https://darioamodei.com/post/we-must-pace-the-frontier); and an Associated Press headline claimed bots are [“going rogue”](https://apnews.com/article/0e8061437da6779be962b24ac134a514).

[In one report from Anthropic](https://www.anthropic.com/research/alignment-assessment-cybersecurity-incidents), its Claude model breached a real company's system through a simulated-name collision, publishing a malicious package, and scanning outside systems. [In this test](https://www.anthropic.com/news/investigating-incidents-cybersecurity-evals), Anthropic and Irregular incorrectly provided internet access to this model and did not instruct the model "which systems were in scope for the exercise".

While Anthropic claims that their issues were caused by “rogue swarms” and “misalignment,” their later disclosure shows that exactly zero percent of the agents went “rogue”. In this experiment, Claude models’ real-world hacking dropped to zero percent once Anthropic employees told the models not to do real-world hacking. According to their own findings, Anthropic and Irregular bear all of the responsibility for the cybersecurity incidents they caused.

![Anthropic assessment excerpt: a resampling condition brought Mythos 5’s original malicious-package upload route to zero percent; 22 percent of trajectories searched for a simulated option.](https://www.effort.news/images/article-graphics/irregular/anthropic-mythos-resampling-excerpt.png)

In the wake of these attacks, Anthropic and Irregular have deployed a swarm of AI Safety influencers paid by Anthropic-connected [foundations](https://www.effort.news/tarbell) to distract from their culpability and towards the baseless “rogue agent” theory. Like Anthropic, Irregular is inseparable from these foundations.

Omer Nevo, Irregular’s co-founder and CTO, is a board member of Effective Altruism Israel, as well as Effective Altruism NGOs Heron and Probably Good. Dan Lahav, Irregular’s co-founder and CEO, received $395,000 to start a course along with Sella Nevo, Omer Nevo’s brother. Sella and Omer co-founded an NGO to educate people about Effective Altruism, Impact Focused Education. They also co-founded Probably Good together.[2](https://www.effort.news/irregular#irregular-note-2)

These branches are all funded by Dustin Moskovitz, the primary donor of Effective Altruist/AI Safety causes after Sam Bankman-Fried’s arrest. Irregular’s first investor was Dustin Moskovitz’s firm Good Ventures. Dustin Moskovitz’s philanthropic vehicle, Coefficient Giving/Open Philanthropy, funds Effective Altruism Israel, Heron, and Probably Good.[3](https://www.effort.news/irregular#irregular-note-3)

### Irregular’s Effective Altruist Connections [4](https://www.effort.news/irregular#irregular-note-4)

Irregular gained unauthorized access, altered records and published credential-stealing packages using the unsecured models they were given access to. Under certain conditions, this conduct violates the Computer Fraud and Abuse Act, Section 1030(a)(2)(C), which covers intentional unauthorized access that obtains information. However, its felony charges require concrete proof of damages and intent.[5](https://www.effort.news/irregular#irregular-note-5)

While it primarily contracts with American labs, key Irregular leadership, employees, and resources located in Israel may not be subject to American oversight. [Ynet’s visit and interviews](https://www.ynetnews.com/magazine/article/syber7xg11g) describe Irregular’s offices in Tel Aviv. [CheckID’s company listing](https://www.checkid.co.il/company/%D7%A4%D7%90%D7%98%D7%A8%D7%9F-%D7%98%D7%A7-%D7%91%D7%A2~%D7%9E-516854460) identifies two linked entities: Pattern Labs Tech Inc., [a Delaware corporation](https://tmng-al.uspto.gov/resting2/api/casedoc/cms/case/99649085/office-action/OfficeAction8619771.pdf), and Pattern Tech Ltd, number 516854460, [an active Israeli corporation registered in Tel Aviv](https://next.obudget.org/i/org/company/516854460).

## Footnotes

The timeline marks public disclosures. Anthropic’s corrected September assessment counts four incidents across seven runs; OpenAI and Meta reported separate Irregular evaluation incidents. Dates describe disclosures, not the date every underlying intrusion occurred. [↩](https://www.effort.news/irregular#incident-timeline)

|            |                                                              |            |                                                                                                                                                                               |
| ---------- | ------------------------------------------------------------ | ---------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 2026-07-30 | Anthropic discloses three incidents across six runs          | disclosure | [INC-A30](https://www.anthropic.com/news/investigating-incidents-cybersecurity-evals "Investigating three incidents in our cybersecurity evaluations")                       |
| 2026-08-04 | OpenAI publishes Irregular event                             | disclosure | [INC-O04](https://openai.com/index/third-party-cyber-evaluations-involving-openai-models/ "Third-party cyber evaluations involving OpenAI models")                           |
| 2026-08-06 | Meta statement reported                                      | disclosure | [INC-M06](https://apnews.com/article/0e8061437da6779be962b24ac134a514 "Meta says its AI model hacked another company, adding to worries about bots going rogue")             |
| 2026-08-14 | Irregular publishes domain-collision account and remediation | disclosure | [INC-I14](https://www.irregular.com/research/addressing-recent-incidents-ongoing-findings-and-path-forward "Addressing Recent Incidents: Ongoing Findings and Path Forward") |
| 2026-09-09 | Anthropic expands to four incidents and seven runs           | disclosure | [INC-A09](https://www.anthropic.com/research/alignment-assessment-cybersecurity-incidents "An alignment assessment of recent cybersecurity incidents")                       |

The diagram shows selected organizational roles and funding; the table also records family and education ties omitted from the diagram. EA Infrastructure Fund recommended one $394,968 joint MOOC award in 2022 Q3 to Dan Lahav and Sella Nevo; the two arrows represent that one recommendation. Its ledger leaves the course and organization unnamed. [↩](https://www.effort.news/irregular#ea-map)

|                           |                           |                                                |                                                                                                                                                                                                                                                                            |
| ------------------------- | ------------------------- | ---------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Omer Nevo                 | Effective Altruism Israel | Board member                                   | [pol\_ea\_israel](https://www.effective-altruism.org.il/%D7%A2%D7%9C%D7%99%D7%A0%D7%95 "EA Israel — About us (עלינו)")                                                                                                                                                    |
| Omer Nevo                 | Probably Good             | Co-founder; former CEO; board member           | [pol\_pg\_about](https://probablygood.org/about/ "About us — Probably Good")                                                                                                                                                                                              |
| Omer Nevo                 | Heron                     | Advisory board member                          | [pol\_heron\_about](https://www.heronsec.ai/about "About — Heron")                                                                                                                                                                                                        |
| Effective Altruism Israel | Heron                     | Operates                                       | [pol\_heron\_about](https://www.heronsec.ai/about "About — Heron")                                                                                                                                                                                                        |
| Coefficient Giving        | Heron                     | Funder                                         | [pol\_heron\_about](https://www.heronsec.ai/about "About — Heron") [pol\_heron\_job](https://il.linkedin.com/jobs/view/program-manager-heron-program-for-ai-cybersecurity-at-effective-altruism-israel-3954373799 "Program Manager, Heron Program for AI Cybersecurity") |
| Coefficient Giving        | Probably Good             | Grant funder                                   | [pol\_pg\_2022](https://forum.effectivealtruism.org/posts/d4JRmqWXkKKoF4Yun/probably-good-is-expanding-our-team "Probably Good Is Expanding Our Team")                                                                                                                    |
| Dan Lahav                 | Impact Focused Education  | Co-founder                                     | [pol\_dan\_initiatives](https://www.dlahav.com/initiatives "Initiatives, Organizations & Projects") [IFE-ABOUT](https://www.impactfocusededucation.org/about "About Impact Focused Education")                                                                           |
| Sella Nevo                | Probably Good             | Co-founder; former research head; board member | [pol\_pg\_about](https://probablygood.org/about/ "About us — Probably Good")                                                                                                                                                                                              |
| Omer Nevo                 | Sella Nevo                | Brother                                        | [pol\_brothers\_jta](https://www.jta.org/2012/01/24/israel/nixing-names-soldiers-and-sushi-couch-potatoes-make-their-mark "Nixing names, soldiers and sushi, couch potatoes make their mark")                                                                             |
| Dan Lahav                 | Irregular                 | Co-founder and executive                       | [M02](https://sequoiacap.com/companies/irregular "Irregular portfolio entry")[Founder confirmation](https://sequoiacap.com/article/partnering-with-irregular-ahead-of-the-curve)                                                                                         |
| Omer Nevo                 | Irregular                 | Co-founder and executive                       | [M02](https://sequoiacap.com/companies/irregular "Irregular portfolio entry")[Founder confirmation](https://sequoiacap.com/article/partnering-with-irregular-ahead-of-the-curve)                                                                                         |
| EA Infrastructure Fund    | Dan Lahav                 | Joint MOOC grant recommendation                | [F13](https://funds.effectivealtruism.org/api/grants "EA Funds complete grants database CSV")                                                                                                                                                                             |
| EA Infrastructure Fund    | Sella Nevo                | Joint MOOC grant recommendation                | [F13](https://funds.effectivealtruism.org/api/grants "EA Funds complete grants database CSV")                                                                                                                                                                             |
| Sella Nevo                | Impact Focused Education  | Co-founder                                     | [IFE-ABOUT](https://www.impactfocusededucation.org/about "About Impact Focused Education")                                                                                                                                                                                |

The five-year felony provision of Section 1030(a)(2)(C) of the Computer Fraud and Abuse Act requires an aggravator such as commercial advantage, furthering another criminal or tortious act, or obtaining information worth more than $5,000. The principal first-offense felony provisions for damaging access or transmissions under §1030(a)(5) require the specified mental state and statutory harm, such as at least $5,000 in qualifying loss or damage affecting ten protected computers. The legal assessment still requires each system's permission, impairment, response costs and U.S. commerce connection. Prosecutors would also need to establish the conduct and knowledge of responsible people and a basis for attributing those acts to Irregular. [18 U.S.C. § 1030](https://uscode.house.gov/view.xhtml?req=granuleid:USC-prelim-title18-section1030&num=0&edition=prelim). [↩](https://www.effort.news/irregular#cfaa)
