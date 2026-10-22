---
title: The Economic Complexity of the Western Roman Empire
link: https://www.michelecoscia.com/?p=2613
source: hnrss-org
published: 2026-10-14T09:32:09Z
updated: 2026-10-14T09:32:09Z
first_seen: 2026-10-16T18:46:36.999527378Z
authors:
- mikk14
labels:
- archaeology
- classical archaeology
- eci
- economic complexity
- economics
- macroeconomics
- nestedness
- roman empire
content: extracted
html: 2026-10-14-the-economic-complexity-of-the-western-roman-empire.html
extra:
  comments_url: https://news.ycombinator.com/item?id=50084790
  num_comments: 27
  points: 131
---

I am one of those people who [think about the Roman Empire daily](https://knowyourmeme.com/memes/how-often-do-you-think-about-the-roman-empire) – possibly out of my worrying addiction to Crusader Kings III and similar games. I also spent some time working on [estimating economic complexity](https://www.michelecoscia.com/?p=240), a neat measure that can be used to explain economic development trajectories. One fateful day, I was attending a presentation by the excellent [Petra Heřmánková](https://www.au.dk/en/petra.hermankova@cas.au.dk) when the two thoughts collided in my head like protons in the Large Hadron Collider. Like a data science version of Young Frankenstein, I suddenly realized that we could calculate the Economic Complexity of the Roman Empire and yelled in the middle of her presentation: “It can be done!” [1](https://www.michelecoscia.com/?p=2613#easy-footnote-bottom-1-2613 "Ok, maybe it didn&#8217;t quite happen that way. Still.")

Well, it could, because we did it: Petra, [Matteo Mazzamurro](https://www.qmul.ac.uk/spcs/staff/academics/profiles/mmazzamurro.html), [Tom Brughmans](https://www.linkedin.com/in/tom-brughmans-9041b313/), and I just published “[The economic complexity of the Roman Empire as inferred from Latin inscriptions](https://www.michelecoscia.com/wp-content/uploads/2026/10/rome_eci.pdf)” in the journal [npj Complexity](https://www.nature.com/articles/s44260-026-00100-3). So this blog post is essentially the content of our memoir: “How we did it.”

The eureka moment was when we realized the potential of a splendid dataset: [LIST, or Latin Inscriptions in Space and Time](https://zenodo.org/records/10473706). This is the most complete collection to date of Latin inscriptions from the period of the Roman Empire that we know of. For each inscription, we have the coordinates of where it was found, as well as a digital transcription of its text. You can see in the image below a heatmap with the spatial distribution of these inscriptions.

[![](https://www.michelecoscia.com/wp-content/uploads/2026/10/image.png)](https://www.michelecoscia.com/wp-content/uploads/2026/10/image.png)

The key to connecting inscriptions to our objective is the same thing that links the economy to everything else: death. Many inscriptions are funerary; others are honorific. All were created with the more or less explicit purpose of saying: “Look at how awesome this guy [2](https://www.michelecoscia.com/?p=2613#easy-footnote-bottom-2-2613 "It&#8217;s always a guy, isn&#8217;t it. More on this later") was.” And how else can you boast about someone if not by saying what they actually did? Crucially, this includes their profession, leaving us with evidence of which economic activities – professions – took place where: at the latitude and longitude where the inscription was found.

You can already see why we cannot take this data at face value: if someone is vicariously boasting, they’re going to report only the deceased’s most honorable occupations. This introduces a status bias. That’s not the only bias: these inscriptions are part of the long-honored tradition of ignoring the fact that women exist and do stuff, too. Men outnumber women in the dataset two to one. More bias still: farmers didn’t have the money nor the interest in having inscriptions about themselves, purging from our records probably more than 90% of the workforce. Finally, we can only have the inscription in the dataset if someone found it and bothered digitizing it, which means that we also have a historiography bias.

The bulk of the work on the paper involved Petra researching how to correct for these biases and Matteo implementing analyses that account for them as best we can. The result was an estimate of how many people worked in each occupation in each province of the empire. The heatmap below shows provinces on the y-axis and occupations on the x-axis. Rows and columns are sorted by count, with Latium (Rome) at the top as the province with the most occupations, and farmers on the right as the most common occupation.

[![](https://www.michelecoscia.com/wp-content/uploads/2026/10/fig1_mcp-2048x692.png)](https://www.michelecoscia.com/wp-content/uploads/2026/10/fig1_mcp-scaled.png)

This pattern where most of the stuff happens in the top-left quadrant tingles the spider senses of anyone who has worked on economic complexity. It is a classic nested pattern and a harbinger of things to come. Specifically, the calculation of the Economic Complexity Index (ECI).

It’s hardly surprising that Latium had the highest ECI. More intriguing was the possibility of trying something different. Because the inscriptions have precise latitude and longitude coordinates, we don’t need to use Roman provinces to calculate ECI: any geographical division will do. For instance, we can use the borders of modern countries to calculate an ECI value for the territory that corresponds to modern-day France, but using data from Roman times.

Because I already had estimates of modern-day ECI lying around, it was easy for us to calculate the Spearman rank correlation between today’s ECI values and those obtained using data from the Roman Empire. This is cool because the territory of the Western Roman Empire corresponds to approximately 23 modern-day countries, giving us just about enough observations to test for a significant correlation.

And a significant correlation we found! This is remarkable with only 23 observations and, you know, a millennium and a half gap between the measurements of ECI. It suggests that being complex in the classical era carries a shared signal with being complex today, after a handful of centuries of history. We don’t yet have an explanation for this finding. We cannot establish whether there is a direct causal relationship between the two, or whether some common third factor could explain both. The observation is astounding nonetheless. It is at least consistent with the idea that economic complexity is sticky: our work cannot prove this idea, but we have definitely failed to disprove it.

There is a lot of work left to do. First, we need more and better bias corrections. Second, there is the problematic situationship we have with the Eastern Roman Empire. Those rascals used Greek, not Latin. There is [GIST](https://zenodo.org/records/10139110), the sister dataset of LIST, which focuses on Greek inscriptions. Unfortunately, GIST is not up to the required digitization standards yet, and gives off weird results that are more than a simple head-scratcher.

As a third and final action point, I am of the opinion that Carthage must be destroyed.

* * *

1. Ok, maybe it didn’t quite happen that way. Still.
2. It’s always a guy, isn’t it. More on this later
