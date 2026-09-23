---
title: The Situation Report
link: https://www.anthropic.com/features/ebola-response
source: anthropic-com-news
published: 2026-09-22T00:00:00Z
first_seen: 2026-09-23T14:55:20.019910144Z
summary: Opus 5.5 performs at the level of Claude Fable 5.1 on most work and costs 40% less to run than Opus 5.
content: extracted
html: 2026-09-22-the-situation-report.html
preview:
  file: 2026-09-22-the-situation-report.preview-c6df668a9806.webp
  width: 256
  height: 144
  alt: Video thumbnail
  color: '#6a6255'
images:
- source: https://www.anthropic.com/_next/image?url=https%3A%2F%2Fcdn.sanity.io%2Fimages%2F4zrzovbb%2Fwebsite%2F9cd992b121924d66c6cfedb5610a88e857b5097e-1280x720.jpg&w=3840&q=75
  original:
    file: 2026-09-22-the-situation-report.image-3832130987f7.webp
    width: 1280
    height: 720
  variants:
  - file: 2026-09-22-the-situation-report.image-a42d87ecc8c9.webp
    width: 320
    height: 180
  color: '#1a1714'
- source: https://cdn.sanity.io/images/4zrzovbb/website/c913a9b3bb258377506ff421c35c4b01d8a168ea-2000x1334.webp?w=1920&h=1280&fit=crop&fm=webp&q=82
  original:
    file: 2026-09-22-the-situation-report.image-5959e3e0d02d.webp
    width: 1920
    height: 1280
  variants:
  - file: 2026-09-22-the-situation-report.image-9b8f90bc1c4d.webp
    width: 320
    height: 213
  color: '#756948'
- source: https://cdn.sanity.io/images/4zrzovbb/website/a4c2e3d145641978458d5bff162576d8b06593a9-2000x1334.webp?w=1920&h=1280&fit=crop&fm=webp&q=82
  original:
    file: 2026-09-22-the-situation-report.image-e21676c92e3e.webp
    width: 1920
    height: 1280
  variants:
  - file: 2026-09-22-the-situation-report.image-793ba253c141.webp
    width: 320
    height: 213
  color: '#83776b'
- source: https://cdn.sanity.io/images/4zrzovbb/website/354dc965271709410e05a90e55bd84623c0b39f9-2000x1334.webp?w=1920&h=1280&fit=crop&fm=webp&q=82
  original:
    file: 2026-09-22-the-situation-report.image-120f61e9827e.webp
    width: 1920
    height: 1280
  variants:
  - file: 2026-09-22-the-situation-report.image-bd35b45d3cf6.webp
    width: 320
    height: 213
  color: '#466a88'
---

S ince May, teams on the ground in the Democratic Republic of the Congo (DRC) have been combating a rare strain of Ebolavirus called Bundibugyo (BDBV). Every day, *relais communautaires* knock on doors of homes, offer counsel, learn of recoveries and deaths, and refer anyone with suspicious symptoms to get care.

In an outbreak like this one, a health worker's notebook is often the first and most complete record of when a person got sick, who they touch, and where they went before they knew they were infected. In the BDBV outbreak, almost half of the nearly 8,000 confirmed cases have died. How quickly someone is found—before their condition turns critical and while they can still be treated—often determines whether they survive.

Once a vaccine becomes available, that same contact information will tell responders exactly where it needs to go. A faster, more accurate picture of who was exposed means people can be found and protected before they ever fall ill. But the speed of this response is limited by how quickly the data can be passed along the chain from the notebooks of health workers, to the labs confirming diagnoses, and to the ministries and funders deciding where resources go next.

Health facilities send case information to their districts, often over WhatsApp, and send biological samples to local labs for sequencing. Each district puts case information onto PowerPoint slides and sends it on to the outbreak coordination center. At the provincial health ministry, the staff compiling the reports work late nights pulling together this information into slide decks. These decks turn into the published situation report, or sitrep.

![A health worker in a clear plastic apron over green scrubs pulls a heavy red rubber glove on over white medical gloves. Around them, rubber boots dry upside down on wooden stakes and pairs of red and green gloves are laid out on the grass, inside an enclosure of orange mesh fencing.](https://cdn.sanity.io/images/4zrzovbb/website/c913a9b3bb258377506ff421c35c4b01d8a168ea-2000x1334.webp?w=1920&h=1280&fit=crop&fm=webp&q=82)\
**Rwampara treatment center**

The creation of sitreps, and the greater outbreak response, has recently started to look different in eastern DRC, thanks to a partnership convened by the Coalition for Epidemic Preparedness Innovations ( [CEPI](https://cepi.net/) ) with, among others, the WHO Regional Office for Africa ( [WHO AFRO](https://www.afro.who.int/) ) and the Institut National de Recherche Biomédicale ( [INRB](https://inrb.net/) ). These organizations have been working with Anthropic's Beneficial Deployments and Applied AI teams to use Claude to analyze community health workers’ field data, to systematically monitor and review research evidence, and to use our scientific workbench.

> “I have fought this virus for fifty years. The tools have changed completely, but the rule has not: you beat Ebola by knowing where it is today, not where it was last week.”
>
> Dr. Jean-Jacques Muyembe, Director General of DRC’s National Institute of Biomedical Research, special adviser to the director general of the Africa CDC, and co-discoverer of the Ebolavirus in 1976

The emergencies hub of WHO AFRO is headquartered in Dakar, Senegal. The response team handles about a hundred public health events a year, often deploying on missions to affected regions to gather data and work on processes.

Tendai Muza works on data systems and analytics at WHO AFRO. With his colleagues Tamayi Mlanda and Gianni Donkor Muza, he built a [Claude skill](https://code.claude.com/docs/en/skills) to accelerate the sitrep process. With the skill, Claude knows to pull the case and lab numbers out of each health zone’s PowerPoint deck, check them against the previous day’s report, flag any change in the trend (and explain what might be going on with it), and then summarize the districts’ reports. A sitrep that used to take all day to put together can now take under an hour.

For the data teams, in addition to streamlining the sitrep process with Claude, they are also using it for analysis and disease modeling. Paul Ouma of the WHO AFRO team said, “Previously we couldn't even start thinking of which disease model might be better. We could only run with one, because of time constraints.” Now they can run multiple models at a time with Claude and build forecasts to help, for example, logistics staff decide where treatment centers should be built.

There is currently no approved vaccine for BDBV, but there is a vaccine, Ervebo, for the Zaire strain. When BDBV emerged, CEPI invited proposals to advance potential vaccine candidates and related vaccine science. Polina Brangel, R&D Data Innovation Lead at CEPI told us, "Claude does not decide which cohorts of serological samples should be analyzed to generate evidence on Ervebo's potential cross-reactivity against BDBV — that remains a scientific judgement made by experts.” What Claude currently does is “organize complex, multi-factor data in a way that allows the human eye to compare across a wide range of proposals and draw conclusions within a much shorter timeframe.”

CEPI used Claude to build a dashboard that helped more quickly and robustly track the actions and work needed to be done by teams to advance vaccine development.

> “This has been an opportunity to learn, in real time, where AI could make a practical difference for teams working under pressure.”
>
> Polina Brangel, R&D Data Innovation Lead at CEPI

In outbreak labs—where samples collected by health workers are sequenced to confirm cases and trace how the virus is changing–the bottleneck to results is often the computer analysis of a virus’s genetic data (known as bioinformatics). Reading the virus’s genome can reveal which cases are linked, whether the virus is spreading through chains that haven’t been spotted by case counts, and whether the virus is changing in ways that could undermine tests, treatments or vaccines.

![Seen from above, a lab worker in a blue gown reaches through the sealed glove ports of a green-framed glovebox, blue nitrile gloves over dark green sleeves. One hand holds a black marker ready to write; the other holds a small round vial cap. Inside the chamber, trays of sample tubes and dark vial caps sit on the white work surface.](https://cdn.sanity.io/images/4zrzovbb/website/a4c2e3d145641978458d5bff162576d8b06593a9-2000x1334.webp?w=1920&h=1280&fit=crop&fm=webp&q=82)\
**Handling samples in the glovebox**

The INRB lab has produced essentially all of the BDBV genomes sequenced from DRC cases in the current outbreak. Each of their sequencing machines produces millions of short fragments of genetic code. Piecing together a virus's full genome from those fragments has traditionally meant typing specialized commands into a programmer's terminal. But [Claude Science,](https://claude.com/product/claude-science) an AI workbench for scientists, can be prompted in plain language to assemble the genome and build the virus's family tree. Especially during major outbreaks, bioinformatics capacity is limited, and Claude Science significantly lowers the barrier to produce analysis that can be used to trace new infections. It can also help estimate the scale of the outbreak, and identify new variants.

WHO Africa is already thinking about applying these tools to other outbreaks. For an outbreak of chikungunya—a debilitating disease spread mosquitoes—it is using Claude to clean and validate “line lists” that describe an outbreak by person, place, and time. This is work that would have taken much longer without AI.

Next, the WHO team wants to use Claude to stitch together historical disease databases, so that decisions in the next outbreak can draw on the last ones. In the past, this has been too time consuming to do in the midst of a crisis.

![A health worker's outstretched hands, lathered in white soap, rub together beside a poster on a blue-and-cream wall titled \"Le lavage des mains — Comment?\" The poster illustrates the World Health Organization's handwashing technique step by step in French. Below it, a pump bottle of green liquid soap stands ready.](https://cdn.sanity.io/images/4zrzovbb/website/354dc965271709410e05a90e55bd84623c0b39f9-2000x1334.webp?w=1920&h=1280&fit=crop&fm=webp&q=82)\
**Handwashing drill, Kitatumba Hospital**

Global health partners are beginning to understand how AI tools can bring the greatest benefit during an outbreak. The Bundibugyo Ebola response still has a long way to go. Containing it will take time–although there are some [promising signs](https://www.statnews.com/2026/09/16/ebola-outbreak-drc-who-cautious-optimism/) of the outbreak slowing in Ituri. Response teams are helping the world learn what responsible, practical AI deployment can look like in real-world public health situations, where time is of the essence.
