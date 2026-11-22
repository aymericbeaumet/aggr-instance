---
title: 87% of the Anthropic power will come from gas and 11% from coal in 2028
link: https://betterdatacenterproject.com/2026/11/16/how-ai-is-powered-a-breakdown-of-byop-and-grid-power/
source: hnrss-org
published: 2026-11-22T08:57:34Z
updated: 2026-11-22T08:57:34Z
first_seen: 2026-11-22T14:51:24.965070109Z
authors:
- happosai
content: extracted
html: 2026-11-22-87-of-the-anthropic-power-will-come-from-gas-and-11-from.html
remote_preview:
  url: https://i0.wp.com/betterdatacenterproject.com/wp-content/uploads/2026/11/anthropic-total-co2.png?fit=1200%2C1065&ssl=1
---

In [The AI Gas Rush: Risks and Implications of “Bring Your Own Power”](https://betterdatacenterproject.com/2026/10/08/the-ai-gas-rush/), we dug into how data centers are increasingly securing their own dedicated primary power generation rather than relying solely on utilities to procure power (“BYOP”). We identified 177 GW of planned BYOP gas capacity—enough to power 65% of all US homes. Conversely, we found renewables are much less common than gas for BYOP: we estimate that for current proposals, less than 20% of energy generation will come from renewable sources by 2032.

But BYOP projects aren’t the only new fossil sources that are coming online to meet the skyrocketing energy demands of AI data centers. [Another 200 GW of new fossil power plants](https://globalenergymonitor.org/projects/global-oil-gas-plant-tracker?popup=2638) is being built on the grid to meet growing demand, due in large part to data centers.[^1]

As a case study of how the AI buildout is driving both new BYOP and grid fossil infrastructure, we analyzed the compute commitments of a single company: Anthropic.

Map of Anthropic’s data centers and associated power plants. See our [full dataset](https://docs.google.com/spreadsheets/d/1XSqa0lJFJs6pQcmcTlBwUqLsQBEtDybNhjEiFGj-vbE/edit?gid=880500164#gid=880500164) for details of each site.

![](https://i0.wp.com/betterdatacenterproject.com/wp-content/uploads/2026/11/anthropic-power-split.png?resize=2048%2C428&ssl=1)\
Figure 1. Breakdown of power source for Anthropic’s compute.

**Finding 1: Anthropic has made no direct commitments to renewable energy in the U.S.** Though Anthropic has [stated](https://www.anthropic.com/policy/infrastructure) “We’re also working to broaden our energy mix with geothermal, wind, solar, and emerging technologies”, its only publicly disclosed commitments, contract, or investments in renewable power are in [Norway](https://marketwise.com/investing/bitdeer-4-7-billion-ai-data-center-lease-tied-to-anthropic-who-benefits/) and [Canada](https://www.globenewswire.com/news-release/2026/10/16/3382650/0/en/everwind-announces-10-billion-investment-in-point-tupper-data-centre-campus-and-renewable-energy-portfolio-across-nova-scotia.html) (as of November 2026). In contrast, Anthropic is directly procuring fossil power equipment—it recently “[bought commitments for 1.7 gigawatts of heavy-duty \[gas\] generators for $3.4bn](https://www.ft.com/content/41bf5c6a-9735-4f1a-a8e3-2bcccad58ba0?syn-25a6b1a6=1)”.

**Finding 2: Anthropic’s currently contracted data centers in the U.S. will be overwhelmingly powered by fossil fuels in the near term. By considering on-site gas and the grid power plants which are likely to meet Anthropic’s added load (i.e. marginal generators), we estimate about 87% of the company’s power will come from gas and 11% from coal in 2028**. As of October 2026, Anthropic has contracted [15 GW of compute](https://www.theinformation.com/articles/anthropic-clinched-517-billion-compute-deals-11-months); of this, about 5.9–9.6 GW is contracted to specific data center sites. For a sense of scale, 9.6 GW could power about 7 million homes or cover [18% of the peak power demand of California](https://www.ferc.gov/understanding-and-participating-california-iso-caiso-processes)—the world’s 4th largest economy.

This demand will be met with a mix of new on-site gas, new grid gas, and existing grid gas, coal, and nuclear. In the short term, no grid renewables are attributed to Anthropic’s power use, because—without commitments to new renewable capacity—new load on the grid will be met by dispatchable, marginal fossil resources.[^2] For more details, see our [full dataset](https://docs.google.com/spreadsheets/d/1XSqa0lJFJs6pQcmcTlBwUqLsQBEtDybNhjEiFGj-vbE/edit?gid=1671585594#gid=1671585594) and our methodology below.

![](https://i0.wp.com/betterdatacenterproject.com/wp-content/uploads/2026/11/anthropic-total-co2.png?w=1256&ssl=1)\
Figure 2. Anthropic’s emissions from power consumption expected in 2028. Emissions for Google, Microsoft, and Meta are annual Scope 2, location-based emissions from their most recent sustainability reports. California power sector emissions are for all power plants operating within the state (source: [EIA](https://www.eia.gov/state/seds/sep_co2/total/pdf/co2_CA.pdf)).

**Finding 3: Total CO2** **emissions from Anthropic’s power use at these sites could reach 17–38 million MtCO2e per year in 2028**, when this capacity is expected to be online. This range reflects uncertainty in Anthropic’s average power use, due to factors such as cooling needs and average utilization (see methodology below). In our mid-case, Anthropic will emit 32 million MtCO2e in 2028. These totals will continue to grow—possibly doubling by 2030, as some analysts expect Anthropic to [secure 30 GW by 2030](https://www.artemis.ai/anthropic-thesis).

To put these figures in context, Anthropic’s CO2 emissions from power use in 2028:

- could exceed the emissions of all of California’s power generation ([31.1 MMtCO2e in 2024](https://www.eia.gov/state/seds/sep_co2/total/pdf/co2_CA.pdf), not including imports).
- could exceed Meta, Microsoft, and Google’s *combined* Scope 2 emissions from 2025 (their location-based emissions, before renewable energy offsets, were [5.9 million](https://sustainability.atmeta.com/wp-content/uploads/2025/10/Meta_2025-Environmental-Data-Index.pdf), [12 million](https://cdn-dynmedia-1.microsoft.com/is/content/microsoftcorp/microsoft/msc/documents/presentations/CSR/2026-Microsoft-Environmental-Data-Fact-Sheet-PDF.pdf) and [15 million](https://sustainability.google/files/google-2026-environmental-report.pdf) MtCO2e, respectively)[^3]
- would represent a 16–36% increase over 2024 levels of the entire U.S. data center sector ([105 MMtCO2e](https://arxiv.org/abs/2411.09786)).
- would increase the entire U.S.’s power sector emissions ([1,485 MMtCO2e in 2025](https://www.eia.gov/environment/emissions/carbon/)) by 1.1–2.5%.

**Finding 4: Anthropic’s data centers are sited in areas where the grid mainly uses coal and gas.** We estimate the average carbon intensity of Anthropic’s power to be 1,033–1,076 lb CO2e/MWh, 35% higher than the U.S. grid average ([767 lb CO2e/MWh](https://www.epa.gov/egrid/summary-data)).[^4]

![](https://i0.wp.com/betterdatacenterproject.com/wp-content/uploads/2026/11/anthropic-emissions-intensity.png?resize=2048%2C1190&ssl=1)\
Figure 3. CO2e and NOx emissions factors for on-site and grid power serving each of Anthropic’s data center sites.

**Finding 5: 80% of on-site power uses inefficient simple cycle turbines and engines.** About 25% (2.5 GW) of Anthropic’s contracted power will come from on-site, behind-the-meter gas. 80% of this on-site capacity consists of inefficient simple cycle turbines and engines, which emit more pollution per unit of electricity than modern combined cycle plants (see our [report](https://betterdatacenterproject.com/2026/10/08/the-ai-gas-rush/) for more details on the types of power generation being used for on-site gas).

**Finding 6: Based on EPA’s nationally-averaged** [**benefit-per-ton estimates**](https://www.epa.gov/benmap/sector-based-pm25-and-ozone-benefit-ton-estimates)**, air pollution from Anthropic’s on-site and grid power use could cause $2–3 billion in annual health damages**, with total emissions of over 13,000 tons per year (tpy) of nitrogen oxides (NOx) and 2,000 tpy of fine particulate matter (PM2.5).

In October 2026, Anthropic outlined its “[approach to responsible data center development](https://www.anthropic.com/policy/infrastructure)”, stating: “We follow the air permitting process everywhere we operate and require the same of the plants powering our data centers. Those plants must also use emissions control technology, including selective catalytic reduction \[SCR\].” However, Anthropic has used compute capacity at SpaceX’s Colossus 2 facility [since May 2026](https://www.sec.gov/Archives/edgar/data/1181412/000162828026040364/spaceexplorationtechnologib.htm), which still receives most of its power from [temporary, unpermitted turbines](https://www.selc.org/news/xai-built-an-illegal-power-plant-to-power-its-data-center/), the majority of which are not equipped with SCR. Anthropic will use as much as 600 MW of power from Colossus 2 by the end of 2026 (see methodology below).

Anthropic has also [contracted 460 MW](https://www.bloomberg.com/news/articles/2026-08-26/anthropic-to-pay-nscale-45-billion-for-ai-computing-power) (and is in talks for [up to 920 MW](https://www.bloomberg.com/news/articles/2026-10-19/nscale-anthropic-in-talks-for-45-billion-cloud-computing-deal)) of power from Nscale’s [Monarch Compute Campus](https://oilandgaswatch.org/facility/cdf08f90-5579-4a86-97ae-0204faa2976f) in Mason, West Virginia. This facility will be powered by inefficient reciprocating engines which have significant formaldehyde and hazardous air pollutant (HAP) emissions compared to conventional gas power plants.

For more, see our [full dataset](https://docs.google.com/spreadsheets/d/1XSqa0lJFJs6pQcmcTlBwUqLsQBEtDybNhjEiFGj-vbE/edit?gid=1671585594#gid=1671585594) on Anthropic’s compute and our [report on BYOP](https://betterdatacenterproject.com/2026/10/08/the-ai-gas-rush/).

### Methodology

We gathered details about Anthropic’s data center commitments from [Epoch AI’s tracker](https://epoch.ai/data/ai-data-centers), Anthropic’s [announcements](https://www.anthropic.com/news/higher-limits-spacex) and [filings](https://www.sec.gov/Archives/edgar/data/2130357/000162828026068676/anthropicpbc-sx1ax1.htm), announcements and filings from partners (including [Amazon](https://www.aboutamazon.com/news/aws/aws-project-rainier-ai-trainium-chips-compute-cluster) and [SpaceX](https://www.sec.gov/Archives/edgar/data/1181412/000162828026040364/spaceexplorationtechnologib.htm)), and reporting from [The Information](https://www.theinformation.com/articles/anthropic-clinched-517-billion-compute-deals-11-months), [Semafor](https://www.semafor.com/article/10/29/2025/aws-massive-multi-state-ai-data-center-is-powering-anthropics-claude), and others.

We first identified the source for each data center’s power. For projects with on-site gas (Monarch, Nexus, Colossus 2) or new grid gas (including indirectly contracted projects such as the [Schahfer expansion](https://www.gem.wiki/Schahfer_Generating_Station) for the AWS New Carlisle site and [Entergy Mississippi’s gas plants](https://www.powermag.com/entergy-mississippi-adding-third-gas-fired-power-plant-to-superpower-strategy/) for AWS’s Madison and Ridgeland facilities) we used emissions factors from permits where available, or permit values from comparable facilities. For grid-connected facilities without associated new generation capacity, we assume these sites will be powered by marginal, dispatchable grid capacity. We used [EPA’s AVERT](https://www.epa.gov/avert) tool to model which power plants would meet the new incremental load from Anthropic’s facilities based on each site’s expected power use and region. AVERT uses historical EPA data for these power plants to model the expected short-run marginal emissions rate of CO2 and criteria pollutants (NOx, PM2.5, etc.). We use AVERT because we are interested in the short-term impacts from new data centers coming online in the next several years.

To calculate the amount of power used by each site, and in turn the total quantity of air pollutant emissions, we assumed [power usage effectiveness](https://datacenters.lbl.gov/sites/default/files/WP49-PUE%20A%20Comprehensive%20Examination%20of%20the%20Metric_v6.pdf) (PUE) ranges from 1.1–1.2 for all sites (typical values for hyperscale data centers are between 1.05–1.3), and power utilization ranges from 60–80%. See [LBNL’s 2024 Data Center Energy Usage report](https://escholarship.org/uc/item/32d6m0d1) for more details about how these parameters vary in typical hyperscale data centers. For our mid-case, we assumed a PUE of 1.15, a power utilization of 70%, and that all 9.6 GW of identified compute capacity is online in 2028.

### How much capacity is Anthropic leasing from xAI?

Anthropic’s initial announcement on May 6, 2026 stated the agreement with SpaceX provided [300 MW of compute (220,000 GPUs)](https://www.anthropic.com/news/higher-limits-spacex). From SpaceX’s [revised June S-1A](https://www.sec.gov/Archives/edgar/data/1181412/000162828026040364/spaceexplorationtechnologib.htm), this deal expanded to ~$1.25B/month for 325,000 NVIDIA GPUs,[^5] with an implied [IT power of ~490 MW](https://robonomics.substack.com/p/spacex-s-1-full-analysis). Based on Anthropic’s [October 2026 S-1](https://www.sec.gov/Archives/edgar/data/2130357/000162828026067168/anthropicpbc-sx1.htm), this deal has further expanded to $3.1B/month.[^6] It’s unlikely that the increase in $/month will result in a proportional (2.5x) increase in compute capacity, since a June 2026 [deal between Google and SpaceX](https://www.sec.gov/Archives/edgar/data/1181412/000162828026041150/spacexagreementfwp.htm) had a much higher cost (~$11.5/GPU-hr vs ~$5/GPU-hr). As a rough estimate, the scaled deal likely corresponds to an additional ~200,000 GPUs, with a total power of ~400 MW (uncertainty is probably ±25% for each).[^7]

[^1]: Data centers are driving 50–75% of demand growth, according to [IEA](https://www.iea.org/reports/energy-and-ai/executive-summary%20) and [McKinsey](https://www.mckinsey.com/industries/electric-power-and-natural-gas/our-insights/powering-ai-how-real-is-the-risk-of-overbuilding#/).

[^2]: For more discussion, see for example: Fassette, Camille. “[The emissions risks of AI data center buildout](https://watttime.org/news-and-insights/the-emissions-risks-of-ai-data-center-buildout/).” WattTime. February, 2025; EPRI. “[Powering Data Centers: U.S. Energy System and Emissions Impacts of Growing Loads](https://www.epri.com/research/products/000000003002031198).” October, 2024.

[^3]: Note: Anthropic’s emissions from power use will likely be classified as a mix of Scope 2 and Scope 3 emissions, depending on if they own the GPUs. For facilities they are leasing space in, the emissions will likely be accounted for as Scope 2 emissions for the owner/operator (e.g. Amazon for Project Rainier or SpaceX for Colossus). It should also be noted that the emissions of Microsoft, Google, and Meta are expected to grow as well, so the comparison between their 2025 emissions and Anthropic’s 2028 emissions is not meant to suggest that Anthropic will be the largest emitter among the tech companies.

[^4]: This average is based on AVERT’s marginal emissions for grid-connected units without new dedicated capacity. Using the average (non-marginal) carbon intensity for each eGRID subregion would give a carbon intensity of 839 lb CO2/MWh. However, marginal emissions are more representative of the near-term impacts of Anthropic’s added load to the grid.

[^5]: Pg. 13: “For example, in May 2026, we entered into Cloud Services Agreements with Anthropic PBC (“Anthropic”), an AI research and development public benefit corporation, with respect to access to compute capacity across COLOSSUS and COLOSSUS II. Compute capacity provided includes approximately 325,000 NVIDIA GPUs, backed by hyperscale-class CPUs, exabyte-scale storage and high-speed networking and interconnects purpose-built for AI workloads. Pursuant to these agreements, the customer has agreed to pay us $1.25 billion per month through May 2029, with capacity ramping in May and June 2026 at a reduced fee.”

[^6]: “Under the SpaceXAI Arrangements, we agreed to spend an aggregate of up to $120.5 billion over terms extending into 2029, consisting of approximately $3.1 billion in the aggregate per month once all capacity has been delivered, which we expect in December 2026, with different fees during capacity ramping periods.”

[^7]: See our [calculations](https://docs.google.com/spreadsheets/d/1XSqa0lJFJs6pQcmcTlBwUqLsQBEtDybNhjEiFGj-vbE/edit?gid=117490244#gid=117490244) and [EpochAI’s calculations](https://docs.google.com/spreadsheets/d/1oZpZ9nTTV6wW0xjc1pPOQmKS7NCyDzuG2HiycC1GqFs/edit?gid=2038650305#gid=2038650305) for more details.
