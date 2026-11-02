---
title: One-minute frequency disturbances from synchronization in power systems
link: https://www.nature.com/articles/s41560-026-02144-0
source: hnrss-org
published: 2026-10-31T10:14:08Z
updated: 2026-10-31T10:14:08Z
first_seen: 2026-11-02T00:33:20.199752031Z
authors:
- croes
content: extracted
html: 2026-10-31-one-minute-frequency-disturbances-from-synchronization-in.html
remote_preview:
  url: https://media.springernature.com/m685/springer-static/image/art%3A10.1038%2Fs41560-026-02144-0/MediaObjects/41560_2026_2144_Fig1_HTML.png
---

## Main

The transition to a sustainable energy system requires advanced control methods to deal with increasing complexity and uncertainty [1](https://www.nature.com/articles/s41560-026-02144-0#ref-CR1 "Denholm, P. et al. The challenges of achieving a 100% renewable electricity system in the United States. Joule 5, 1331–1352 (2021)."),[2](https://www.nature.com/articles/s41560-026-02144-0#ref-CR2 "Marot, A. et al. Perspectives on future power system control centers for energy transition. J. Mod. Power Syst. Clean Energy 10, 328–344 (2022)."). As variable renewable energy sources such as wind and solar replace conventional generation, maintaining power balance requires accurate forecasting and real-time control of storage, backup generation and flexible demand [3](https://www.nature.com/articles/s41560-026-02144-0#ref-CR3 "Morales, J. M., Conejo, A. J., Madsen, H., Pinson, P. & Zugno, M. Integrating Renewables in Electricity Markets: Operational Problems Vol. 205 (Springer Science & Business Media, 2013)."). At the same time, the electrification of sectors such as transport and heating is introducing millions of digitally controlled devices—electric vehicle chargers [4](https://www.nature.com/articles/s41560-026-02144-0#ref-CR4 "Muratori, M. Impact of uncoordinated plug-in electric vehicle charging on residential power demand. Nat. Energy 3, 193–201 (2018)."), heat pumps [5](https://www.nature.com/articles/s41560-026-02144-0#ref-CR5 "Liu, D., Carta, D., Xhonneux, A., Müller, D. & Benigni, A. Short-term control of heat pumps to support power grid operation. IEEE Open J. Ind. Electron. Soc. 5, 1221–1238 (2024).") and smart appliances [6](https://www.nature.com/articles/s41560-026-02144-0#ref-CR6 "Farhangi, H. The path of the smart grid. IEEE Power Energy Mag. 8, 18–28 (2010).")—into the grid. Furthermore, data centres add additional software-dependent loads [7](https://www.nature.com/articles/s41560-026-02144-0#ref-CR7 "Chen, X., Wang, X., Colacelli, A., Lee, M. & Xie, L. Electricity demand and grid impacts of AI data centers: challenges and prospects. Nexus 3, 100162 (2026)."). Together, these trends are reshaping the power system into a cyber–physical infrastructure, where physical devices and digital control are deeply intertwined. In this new paradigm, system reliability increasingly depends not only on the physical properties of generation and load but also on the design, timing and coordination of software-controlled devices across all layers of operation [8](https://www.nature.com/articles/s41560-026-02144-0#ref-CR8 "Xie, L., Singh, C., Mitter, S. K., Dahleh, M. A. & Oren, S. S. Toward carbon-neutral electricity and mobility: is the grid infrastructure ready? Joule 5, 1908–1913 (2021).").

While cyber–physical integration enables unprecedented levels of efficiency and flexibility, it also introduces new systemic risks [9](https://www.nature.com/articles/s41560-026-02144-0#ref-CR9 "Pasqualetti, F., Dörfler, F. & Bullo, F. Attack detection and identification in cyber–physical systems. IEEE Trans. Autom. Control 58, 2715–2729 (2013)."). Historical events underscore the stakes: inadequate situational awareness contributed to the 2003 North American blackout [10](https://www.nature.com/articles/s41560-026-02144-0#ref-CR10 "Andersson, G. et al. Causes of the 2003 major grid blackouts in North America and Europe, and recommended means to improve system dynamic performance. IEEE Trans. Power Syst. 20, 1922–1928 (2005)."), and software-driven control failures have played critical roles in recent major outages [11](https://www.nature.com/articles/s41560-026-02144-0#ref-CR11 "Liang, G., Weller, S. R., Zhao, J., Luo, F. & Dong, Z. Y. The 2015 Ukraine blackout: implications for false data injection attacks. IEEE Trans. Power Syst. 32, 3317–3318 (2017)."),[12](https://www.nature.com/articles/s41560-026-02144-0#ref-CR12 "Yan, R., Masood, N., Saha, T. K., Bai, F. & Gu, H. The anatomy of the 2016 South Australia blackout: a catastrophic event in a high renewable network. IEEE Trans. Power Syst. 33, 5374–5388 (2018)."). Beyond failures and malicious attacks, however, even benign control routines can become systemically relevant when many devices act coherently. Unintended synchronization can generate recurrent and potentially amplified oscillatory behaviour in large-scale cyber–physical energy systems.

Modern power systems exhibit oscillations across a broad range of time scales. Oscillations with characteristic periods around 1 min have previously been discussed as ultralow-frequency oscillations (ULFOs) and linked to resonance phenomena in hydropower turbine-governor systems [13](https://www.nature.com/articles/s41560-026-02144-0#ref-CR13 "Seppänen, J., Lehtonen, M., Kuivaniemi, M. & Haarla, L. Long term characteristics of ultra low frequency oscillations in the Nordic power system. In 2022 IEEE PES Innovative Smart Grid Technologies Conference Europe (ISGT-Europe), 1–5 (IEEE, 2022)."),[14](https://www.nature.com/articles/s41560-026-02144-0#ref-CR14 "Seppänen, J., Lehtonen, M., Kuivaniemi, M. & Haarla, L. On the relationship between ultra low frequency oscillation and power system inertia. In 2023 IEEE Power and Energy Society General Meeting (PESGM), 1–5 (IEEE, 2023)."),[15](https://www.nature.com/articles/s41560-026-02144-0#ref-CR15 "Odelbrink, C. Design of an Alternative Frequency Controller for Hydropower Stations: Reducing the 60-second Floating in the Grid Frequency of the Nordic Power System. Master’s thesis, Uppsala Univ. (2014);
https://www.diva-portal.org/smash/get/diva2:743757/FULLTEXT01.pdf"),[16](https://www.nature.com/articles/s41560-026-02144-0#ref-CR16 "Agneholm, E. & Jansson, E. A. FCP Project Summary Report (ENTSO-E AISBL, 2017);
https://www.fingrid.fi/globalassets/dokumentit/fi/yhtio/tki-toiminta/raportit/fcp-project-summary-report.pdf"). However, resonance phenomena in classical control systems differ fundamentally from cyber–physical synchronization effects. Oscillations without external timing signals do not naturally explain patterns that remain phase-coherent over long time scales or occur reproducibly across diverse power systems.

In this Article, we identify and explain a coherent 1-min oscillation in power systems worldwide caused by cyber–physical synchronization effects. We show that the grid frequency, and thus the power balance, exhibits a highly regular periodic pattern with a period of exactly 1 min. Searching for potential causes of this pattern, we provide empirical evidence that clock-synchronized control actions in battery-based uninterruptible power supplies (UPSs) generate periodic power-demand peaks, and we demonstrate that this signal propagates across multiple voltage levels up to the transmission interface. More broadly, our results point to a class of cyber–physical power oscillations driven by externally synchronized control software.

The identified synchronization mechanism is not specific to UPSs or battery systems, but reflects a general property of digitally controlled devices operating with external timing signals or periodic software routines. The rapid expansion of battery storage, data-centre infrastructure and Internet-of-Things (IoT) devices increases the relevance of cyber–physical risks in power-system operation. At the same time, decreasing system inertia makes power systems more sensitive to coherent power imbalances [17](https://www.nature.com/articles/s41560-026-02144-0#ref-CR17 "Milano, F., Dörfler, F., Hug, G., Hill, D. J. & Verbic, G. Foundations and challenges of low-inertia systems. In 2018 Power Systems Computation Conference (PSCC), 1–25 (2018)."). Indeed, we observe that the amplitude of the 1-min pattern has increased in several grids. The oscillation contributes to wear and tear, especially in hydropower turbines [18](https://www.nature.com/articles/s41560-026-02144-0#ref-CR18 "Yang, W. et al. Wear and tear on hydro power turbines–influence from primary frequency control. Renew. Energy 87, 88–95 (2016)."), and has motivated stricter load–frequency control requirements [19](https://www.nature.com/articles/s41560-026-02144-0#ref-CR19 "Technical Requirements for Frequency Containment Reserve Provision in the Nordic Synchronous Area (ENTSO-E, 2023);
https://www.svk.se/495d0c/siteassets/aktorsportalen/bidra-med-reserver/om-olika-reserver/fcr/fcr-technical-requirements-may-23.pdf"). Our results indicate that coherent cyber–physical oscillations can be mitigated directly at their source through improved control design.

## A universal 1-min pattern in the grid frequency

The grid frequency is a key observable in power-system operation, reflecting the balance between generation and load: it rises when generation exceeds demand and falls when demand outpaces supply. Maintaining the nominal frequency of 50 Hz or 60 Hz therefore requires continuous balancing by load–frequency control systems [20](https://www.nature.com/articles/s41560-026-02144-0#ref-CR20 "P1 – policy 1: load-frequency control and performance. In UCTE Operations Handbook
https://www.entsoe.eu/fileadmin/user_upload/_library/publications/entsoe/Operation_Handbook/introduction_v25.pdf
(Union for the Co-ordination of Transmission of Electricity, 2009).").

Large frequency deviations occur during contingencies and severe disturbances and can endanger system stability [21](https://www.nature.com/articles/s41560-026-02144-0#ref-CR21 "De Boeck, S. et al. Review of defence plans in Europe: current status, strengths and opportunities. CIGRE Sci. Eng. 5, 6–16 (2016)."),[22](https://www.nature.com/articles/s41560-026-02144-0#ref-CR22 "Hatziargyriou, N. et al. Stability Definitions and Characterization of Dynamic Behavior in Systems with High Penetration of Power Electronic Interfaced Technologies (IEEE PES, 2020);
https://doi.org/10.17023/zwwp-vv02"). Smaller but recurrent deviations arise continuously during normal operation through both stochastic fluctuations and deterministic mechanisms [23](https://www.nature.com/articles/s41560-026-02144-0#ref-CR23 "Schäfer, B., Beck, C., Aihara, K., Witthaut, D. & Timme, M. Non-Gaussian power grid frequency fluctuations characterized by Lévy-stable laws and superstatistics. Nat. Energy 3, 119–126 (2018)."),[24](https://www.nature.com/articles/s41560-026-02144-0#ref-CR24 "Report on Deterministic Frequency Deviations (ENTSO-E, 2020);
https://eepublicdownloads.entsoe.eu/clean-documents/Publications/SOC/Report_Deterministic_Frequency_Deviations_final_version.pdf").

To identify dominant recurrent patterns, we carry out a Fourier analysis of grid-frequency time series *f*(*t*) from multiple power systems worldwide (Fig. [1](https://www.nature.com/articles/s41560-026-02144-0#Fig1)). The investigated grids span five continents and range in size from approximately 400,000 customers in Iceland to more than 400 million customers in the Continental European synchronous area.

**Fig. 1: Fourier spectra of the grid frequency *f*(*t*) for 12 grids on different continents that exhibit a 1-min pattern.**

Measurements for the Continental European, British, Scandinavian and Australian grids are taken at the transmission grid level, whereas measurements for the other grids are taken at the distribution grid level. Details on the data sources and processing are described in the [Methods](https://www.nature.com/articles/s41560-026-02144-0#Sec7). The Fourier spectra are computed using SciPy’s Fast Fourier Transform implementation.

[Source data](https://www.nature.com/articles/s41560-026-02144-0#MOESM3)

Recurrent frequency deviations occur across a broad range of time scales. Figure [2](https://www.nature.com/articles/s41560-026-02144-0#Fig2) summarizes the dominant Fourier peaks and characteristic patterns observed in the Continental European grid.

**Fig. 2: Overview of recurrent frequency deviations in the Continental European power grid.**

**a**, The Fourier spectrum of the grid frequency exhibits pronounced peaks at different time scales. **b**, The peak at 60 min arises from electricity trading in hourly blocks [26](https://www.nature.com/articles/s41560-026-02144-0#ref-CR26 "Weissbach, T. & Welfonder, E. High frequency deviations within the European power system: origins and proposals for improvement. In 2009 IEEE/PES Power Systems Conference and Exposition, 1–6 (IEEE, 2009)."),[27](https://www.nature.com/articles/s41560-026-02144-0#ref-CR27 "Kruse, J., Schäfer, B. & Witthaut, D. Exploring deterministic frequency deviations with explainable AI. In 2021 IEEE International Conference on Communications, Control, and Computing Technologies for Smart Grids (SmartGridComm), 133–139 (IEEE, 2021)."). Generation is rapidly adjusted at the beginning of each hour, leading to characteristic power imbalances and DFDs. The figure shows the resulting daily frequency profile together with load and stylized stepwise generation. Trading in shorter market intervals is increasing and gives rise to an additional peak at a period of 15 min (ref. [28](https://www.nature.com/articles/s41560-026-02144-0#ref-CR28 "Böttcher, P. C., Hartmann, C., Benigni, A., Pesch, T. & Witthaut, D. Impact of market reforms on deterministic frequency deviations in the European power grid. Preprint at
https://doi.org/10.48550/arXiv.2602.09645
(2026).")). **c**, This work focuses on the peak at exactly 1 min. We identify cyber–physical power oscillations caused by clocked control algorithms as the root cause of this pattern. A related but less stable pattern is observed at a period of approximately 100 s. The figure shows the corresponding waveforms for different months. **d**, Inter-area oscillations are electromechanical power swings between different regions of a synchronous grid and typically occur at periods of one to several seconds [30](https://www.nature.com/articles/s41560-026-02144-0#ref-CR30 "Fritzsch, J., Tyloo, M. & Jacquod, P. Matrix perturbation theory of inter-area oscillations. In 2021 60th IEEE Conference on Decision and Control (CDC), 3507–3512 (IEEE, 2021)."). The figure shows an example recorded on 28 April 2025. **e**, Additional fast oscillatory phenomena include periodic power variations originating from data centres [29](https://www.nature.com/articles/s41560-026-02144-0#ref-CR29 "Vercellino, R. et al. Measurement of generative AI workload power profiles for whole-facility data center infrastructure planning. Preprint at
https://doi.org/10.48550/arXiv.2604.07345
(2026)."),[32](https://www.nature.com/articles/s41560-026-02144-0#ref-CR32 "Mishra, C., Vanfretti, L., Delaree, J., Purcell, T. & Jones, K. D. Understanding the inception of 14.7 Hz oscillations emerging from a data center. Sustain. Energy Grids Netw. 43, 101735 (2025).") and subsynchronous oscillations in inverter-dominated grids [33](https://www.nature.com/articles/s41560-026-02144-0#ref-CR33 "Cheng, Y. et al. Wind Energy Systems Sub-synchronous Oscillations: Events and Modeling Technical Report (IEEE PES, 2020);
https://doi.org/10.17023/faak-ta85").

[Source data](https://www.nature.com/articles/s41560-026-02144-0#MOESM4)

At slow time scales, pronounced peaks occur at periods of 15 min and 60 min together with their higher harmonics. These deterministic frequency deviations (DFDs) arise from electricity trading, where generation is adjusted approximately stepwise at the boundaries of market intervals while electrical demand evolves continuously [25](https://www.nature.com/articles/s41560-026-02144-0#ref-CR25 "Borsche, T., Ulbig, A. & Andersson, G. A new frequency control reserve framework based on energy-constrained units. In 2014 Power Systems Computation Conference, 1–7 (IEEE, 2014)."),[26](https://www.nature.com/articles/s41560-026-02144-0#ref-CR26 "Weissbach, T. & Welfonder, E. High frequency deviations within the European power system: origins and proposals for improvement. In 2009 IEEE/PES Power Systems Conference and Exposition, 1–6 (IEEE, 2009)."),[27](https://www.nature.com/articles/s41560-026-02144-0#ref-CR27 "Kruse, J., Schäfer, B. & Witthaut, D. Exploring deterministic frequency deviations with explainable AI. In 2021 IEEE International Conference on Communications, Control, and Computing Technologies for Smart Grids (SmartGridComm), 133–139 (IEEE, 2021)."),[28](https://www.nature.com/articles/s41560-026-02144-0#ref-CR28 "Böttcher, P. C., Hartmann, C., Benigni, A., Pesch, T. & Witthaut, D. Impact of market reforms on deterministic frequency deviations in the European power grid. Preprint at
https://doi.org/10.48550/arXiv.2602.09645
(2026)."). This phenomenon is not unique to the Continental European grid; it also appears in other markets such as Scandinavia (cf. Supplementary Fig. [1](https://www.nature.com/articles/s41560-026-02144-0#MOESM1)).

Additional recurrent patterns occur at shorter time scales, including oscillations due to load variations in data centres [29](https://www.nature.com/articles/s41560-026-02144-0#ref-CR29 "Vercellino, R. et al. Measurement of generative AI workload power profiles for whole-facility data center infrastructure planning. Preprint at
https://doi.org/10.48550/arXiv.2604.07345
(2026)."), inter-area oscillations between geographically separated groups of synchronous generators [30](https://www.nature.com/articles/s41560-026-02144-0#ref-CR30 "Fritzsch, J., Tyloo, M. & Jacquod, P. Matrix perturbation theory of inter-area oscillations. In 2021 60th IEEE Conference on Decision and Control (CDC), 3507–3512 (IEEE, 2021).") and persistent forced oscillations of previously unresolved origin [31](https://www.nature.com/articles/s41560-026-02144-0#ref-CR31 "Vanfretti, L., Bengtsson, S., Perić, V. S. & Gjerde, J. O. Spectral estimation of low-frequency oscillations in the Nordic grid using ambient synchrophasor data under the presence of forced oscillations. In 2013 IEEE Grenoble Conference, 1–6 (IEEE, 2013).") with periods of one to a few seconds. At even faster time scales, recent studies have identified sustained oscillations related to interactions of grid dynamics and data centres [32](https://www.nature.com/articles/s41560-026-02144-0#ref-CR32 "Mishra, C., Vanfretti, L., Delaree, J., Purcell, T. & Jones, K. D. Understanding the inception of 14.7 Hz oscillations emerging from a data center. Sustain. Energy Grids Netw. 43, 101735 (2025).") as well as subsynchronous oscillations in inverter-dominated grids [33](https://www.nature.com/articles/s41560-026-02144-0#ref-CR33 "Cheng, Y. et al. Wind Energy Systems Sub-synchronous Oscillations: Events and Modeling Technical Report (IEEE PES, 2020);
https://doi.org/10.17023/faak-ta85").

Against this background, we observe a remarkably narrow spectral peak at exactly 1 min in all investigated grids (Fig. [1](https://www.nature.com/articles/s41560-026-02144-0#Fig1)). The signal is particularly pronounced in large interconnected systems in Europe, North America and Australia, but remains clearly visible in smaller grids such as Malaysia, South Korea and South Africa, as well as in island systems such as Mallorca, Ireland and Iceland. Its extremely narrow spectrum indicates a precisely timed and phase-coherent process that remains stable over multiple years. Related oscillations with characteristic periods around 1 min have previously been described as ULFOs [13](https://www.nature.com/articles/s41560-026-02144-0#ref-CR13 "Seppänen, J., Lehtonen, M., Kuivaniemi, M. & Haarla, L. Long term characteristics of ultra low frequency oscillations in the Nordic power system. In 2022 IEEE PES Innovative Smart Grid Technologies Conference Europe (ISGT-Europe), 1–5 (IEEE, 2022)."). In addition, we observe a weaker peak around 100 s whose origin remains uncertain. What causes this universal 1-min pattern?

## Characterization of the 1-min pattern

To characterize the universal 1-min pattern, we divide the frequency time series *f*(*t*) into chunks of length *τ*. For every chunk, we compute a characteristic minutely profile *s*(*t*), referred to as the waveform in the following. More precisely, *s*(*t*) is the average over all minutes in the respective chunk for a given second of the minute *t* (see the [Methods](https://www.nature.com/articles/s41560-026-02144-0#Sec7) for details). Typically, we use chunks of length *τ* = 1 h to remove the hourly patterns induced by market effects, indicated by the subscript *h*. Furthermore, we define the hourly amplitude as

$${a}\_{{h}}=\\frac{1}{2}\\left(\\mathop{\\max }\\limits\_{t}({s}\_{{h}}(t))-\\mathop{\\min }\\limits\_{t}({s}\_{{h}}(t))\\right)$$

(1)

to characterize the strength of the pattern.

Figure [3a–c](https://www.nature.com/articles/s41560-026-02144-0#Fig3) summarizes the waveform properties for the Continental European grid. The amplitude exhibits a pronounced daily modulation, with weaker signals during the night and stronger amplitudes during daytime operation. By contrast, the waveform shape remains remarkably stable: the frequency is typically highest at the beginning of the minute and then decreases for a few seconds (night) up to half a minute (day). Comparable behaviour is observed across multiple grids (Supplementary Fig. [2](https://www.nature.com/articles/s41560-026-02144-0#MOESM1)), although the precise waveform shape differs between systems.

**Fig. 3: Characterization of the 1-min pattern.**

**a**, Averaged hourly waveforms over a day for the Continental European grid in 2018. The amplitude of the pattern varies throughout the day, yet the overall shape remains consistent: the frequency systematically decreases during the first half of each minute. The averaged hourly waveforms are obtained by computing the waveform for every hour in 2018 and subsequently averaging over all hours corresponding to the same time of day in local time, that is, Central European Time (CET) and Central European Summer Time (CEST). **b**, Monthly waveforms for the Continental European grid in 2018. The pattern is highly stable; only a slight seasonal profile in the amplitudes is visible. **c**, Weekly profile of the hourly amplitudes for the Continental European grid in 2018. The pattern is substantially weaker during the night. On weekends, the morning increase occurs later than on weekdays. Time is given in local time (CET/CEST). The solid line gives the hourly amplitude averaged over all occurrences of each weekday–hour combination in 2018 (for example, all Mondays at 16:00), and the shaded area corresponds to the standard deviation (s.d.). **d**, Long-term trends for the weekly averaged hourly amplitudes for the three major European grids from 2015 to 2025. We observe a seasonal pattern: the amplitudes are stronger in summer than in winter, except for the Christmas/New Year period. For the British and Scandinavian grids, the strength has increased substantially after 2015, reaching a peak in 2020, in conjunction with the COVID-19 lockdown. Signal amplitudes in the Scandinavian grid decreased again after the grid operators issued new regulations for the frequency containment reserve for normal operation (FCR-N) in 2023 (dashed line). The solid line shows the weekly averaged hourly amplitudes and the shaded area corresponds to the standard deviation.

[Source data](https://www.nature.com/articles/s41560-026-02144-0#MOESM5)

The waveform remains phase-coherent and stable over multiple years across all investigated grids. Such long-term coherence is difficult to reconcile with intrinsic oscillatory modes of the power system and instead points to drivers synchronized to a stable external clock.

These observations indicate a recurrent pattern in the underlying power imbalance. In general, the rate of change of frequency follows the instantaneous power imbalance in the grid. The relation between both quantities can be described by the aggregated swing equation [34](https://www.nature.com/articles/s41560-026-02144-0#ref-CR34 "Ulbig, A., Borsche, T. S. & Andersson, G. Impact of low rotational inertia on power system stability and operation. IFAC Proc. Vol. 47, 7290–7297 (2014)."),[35](https://www.nature.com/articles/s41560-026-02144-0#ref-CR35 "Machowski, J., Lubosny, Z., Bialek, J. W. & Bumby, J. R. Power System Dynamics: Stability and Control 3rd edn (Wiley, 2020)."),

$$\\frac{2{E}\_{\\mathrm{rot}}}{{f}\_{\\mathrm{ref}}}\\frac{{\\rm{d}}f}{{\\rm{d}}t}={\\Delta }P(t)-D\[\\,f(t)-{f}\_{\\mathrm{ref}}\],$$

(2)

where Δ*P*(*t*) denotes the power imbalance, *D* is a damping coefficient, *f*ref the reference frequency and *E*rot denotes the total rotational energy of online synchronous machines, which quantifies the system inertia. Accordingly, the observed decrease in frequency implies that demand exceeds generation during the first half of each minute.

Load–frequency control can amplify disturbances at specific frequencies via feedback effects in the power imbalance Δ*P*(*t*). Hydropower turbine-governor systems have therefore been suggested as a possible origin of oscillations with periods around 1 min (refs. [15](https://www.nature.com/articles/s41560-026-02144-0#ref-CR15 "Odelbrink, C. Design of an Alternative Frequency Controller for Hydropower Stations: Reducing the 60-second Floating in the Grid Frequency of the Nordic Power System. Master’s thesis, Uppsala Univ. (2014);
https://www.diva-portal.org/smash/get/diva2:743757/FULLTEXT01.pdf"),[16](https://www.nature.com/articles/s41560-026-02144-0#ref-CR16 "Agneholm, E. & Jansson, E. A. FCP Project Summary Report (ENTSO-E AISBL, 2017);
https://www.fingrid.fi/globalassets/dokumentit/fi/yhtio/tki-toiminta/raportit/fcp-project-summary-report.pdf")). Recent work has furthermore linked 1-min oscillations to periodic set-point updates in secondary frequency control [36](https://www.nature.com/articles/s41560-026-02144-0#ref-CR36 "Wei, F. et al. Multi-time-scale analysis on power grid frequency characteristics. IEEE Trans. Power Syst. 41, 884–899 (2026)."). We likewise observe a 1-min pattern in the secondary control set-point in German control areas (Supplementary Fig. [3](https://www.nature.com/articles/s41560-026-02144-0#MOESM1) and Supplementary Note [1](https://www.nature.com/articles/s41560-026-02144-0#MOESM1)).

To assess the role of hydropower dynamics, we perform complementary mechanistic and data-driven analyses (Supplementary Notes [2](https://www.nature.com/articles/s41560-026-02144-0#MOESM1) and [3](https://www.nature.com/articles/s41560-026-02144-0#MOESM1)). A mechanistic model of hydropower plant control in the Scandinavian grid reveals an internal resonance that can produce oscillatory responses with characteristic periods around 1 min (Supplementary Figs. [4](https://www.nature.com/articles/s41560-026-02144-0#MOESM1)–[9](https://www.nature.com/articles/s41560-026-02144-0#MOESM1)). In parallel, an explainable machine learning analysis identifies systematic correlations between hydropower commitment and the signal amplitude in the Continental European grid (Supplementary Fig. [10](https://www.nature.com/articles/s41560-026-02144-0#MOESM1)). However, neither analysis supports the interpretation that hydropower dynamics alone can explain the long-term phase coherence and cross-system reproducibility of the observed signal. Instead, the results indicate that hydropower can amplify externally driven coherent disturbances or generate transient incoherent oscillations with periods close to 1 min.

The characteristic shape of the 1-min pattern is remarkably stable, while its amplitude changes over time. Figure [3d](https://www.nature.com/articles/s41560-026-02144-0#Fig3) shows the long-term evolution for three European grids where sufficient historical data are available. We observe that the amplitude increased substantially in the British and Scandinavian grids after 2015, reaching peak values in 2020.

One important factor contributing to the observed increase is the reduction of effective system inertia during the transition from synchronous generation to converter-interfaced renewable power sources [17](https://www.nature.com/articles/s41560-026-02144-0#ref-CR17 "Milano, F., Dörfler, F., Hug, G., Hill, D. J. & Verbic, G. Foundations and challenges of low-inertia systems. In 2018 Power Systems Computation Conference (PSCC), 1–25 (2018)."). According to equation ([2](https://www.nature.com/articles/s41560-026-02144-0#Equ2)), lower inertia increases the frequency response to a given coherent power imbalance. A temporary increase during the coronavirus disease 2019 (COVID-19) lockdown in 2020 further suggests that changes in system conditions, including effective system inertia and demand composition, can enhance the signal amplitude (Supplementary Fig. [11](https://www.nature.com/articles/s41560-026-02144-0#MOESM1) and Supplementary Note [4](https://www.nature.com/articles/s41560-026-02144-0#MOESM1)).

In the Scandinavian grid, amplitudes regularly exceeded 10 mHz, raising concerns among grid operators and utilities because oscillatory operation contributes to wear and tear of hydropower turbines providing primary control [18](https://www.nature.com/articles/s41560-026-02144-0#ref-CR18 "Yang, W. et al. Wear and tear on hydro power turbines–influence from primary frequency control. Renew. Energy 87, 88–95 (2016)."). As a consequence, Scandinavian transmission system operators revised the requirements for frequency containment reserves in 2023 [19](https://www.nature.com/articles/s41560-026-02144-0#ref-CR19 "Technical Requirements for Frequency Containment Reserve Provision in the Nordic Synchronous Area (ENTSO-E, 2023);
https://www.svk.se/495d0c/siteassets/aktorsportalen/bidra-med-reserver/om-olika-reserver/fcr/fcr-technical-requirements-may-23.pdf"). The revised prequalification procedures include sine-wave tests in the range of 10–300 s together with stricter damping and activation requirements intended to suppress oscillatory responses (cf. Supplementary Note [5](https://www.nature.com/articles/s41560-026-02144-0#MOESM1)). Following these revisions, amplitudes above 10 mHz were no longer observed in 2025 (Fig. [3d](https://www.nature.com/articles/s41560-026-02144-0#Fig3)). These developments demonstrate that the recurrent 1-min oscillation already has operational relevance, even though current mitigation strategies primarily address amplification rather than the underlying source.

## Identifying the drivers of the 1-min pattern

Frequency deviations reflect imbalances between generation and load as described by equation ([2](https://www.nature.com/articles/s41560-026-02144-0#Equ2)). For the Continental European synchronous area, the overall network power-frequency characteristic is estimated as *K* = 26.68 GW Hz−1 (ref. [20](https://www.nature.com/articles/s41560-026-02144-0#ref-CR20 "P1 – policy 1: load-frequency control and performance. In UCTE Operations Handbook
https://www.entsoe.eu/fileadmin/user_upload/_library/publications/entsoe/Operation_Handbook/introduction_v25.pdf
(Union for the Co-ordination of Transmission of Electricity, 2009).")). Accordingly, a periodic frequency variation with an amplitude of 3 mHz corresponds to a coherent power variation of approximately 80 MW. Which devices generate this signal?

A comparison of measurements from the transmission and distribution levels in the Continental European grid shows that the waveform at the distribution level is slightly phase-advanced (Supplementary Figs. [12](https://www.nature.com/articles/s41560-026-02144-0#MOESM1)–[14](https://www.nature.com/articles/s41560-026-02144-0#MOESM1) and Supplementary Note [6](https://www.nature.com/articles/s41560-026-02144-0#MOESM1)). This suggests that the driving sources are located in the distribution system, while large synchronous generators respond to the resulting imbalance. We therefore investigate high-resolution measurements from local distribution networks.

Forschungszentrum Jülich operates a comprehensive measurement infrastructure in its campus distribution grid. We analyse high-resolution active-power measurements from multiple buildings and transformers to identify potential drivers of the signal (cf. Supplementary Fig. [15](https://www.nature.com/articles/s41560-026-02144-0#MOESM1) and Supplementary Note [7](https://www.nature.com/articles/s41560-026-02144-0#MOESM1)).

A clear 1-min component is observed only at a subset of measurement locations. In total, four buildings exhibit a pronounced periodic signal in the bus-level active-power measurements (Fig. [4](https://www.nature.com/articles/s41560-026-02144-0#Fig4) and Supplementary Figs. [16](https://www.nature.com/articles/s41560-026-02144-0#MOESM1)–[17](https://www.nature.com/articles/s41560-026-02144-0#MOESM1)). Importantly, all affected buildings contain battery-based UPSs, whereas none of the measured buildings without UPS installations show such a pattern.

**Fig. 4: Fourier spectrum of the active-power time series for selected PQ meters in a building on the Jülich research campus.**

**a**, An electrical diagram of Building 1 (cf. Supplementary Table [1](https://www.nature.com/articles/s41560-026-02144-0#MOESM1)) specifying the location of the PQ meters at the transformers. The building contains offices and computing infrastructures. **b**, For transformer 1 (PQ ID 007) we measure a 1-min pattern in the active-power demand. Attached to this transformer are offices, a heat pump and a UPS. For transformer 6 (PQ ID 005), we do not observe any notable patterns in the same measurement period. However, the noise level is also substantially higher, and potential patterns could be hidden.

[Source data](https://www.nature.com/articles/s41560-026-02144-0#MOESM6)

These observations identify battery energy management systems (EMSs), particularly in UPS applications, as prime drivers of the 1-min signal. Their control algorithms can induce periodic power-demand peaks at fixed 1-min intervals that propagate through the grid and contribute to coherent frequency variations. More generally, the results point to a broader class of cyber–physical synchronization effects arising from digitally controlled devices operating on externally synchronized clocks or periodic software routines.

To investigate this mechanism in detail, we analyse a UPS installation in Building 3 on the Jülich campus (Supplementary Table [1](https://www.nature.com/articles/s41560-026-02144-0#MOESM1)). Measurements include (1) the DC battery current, (2) AC currents and power flows upstream and downstream of the UPS and (3) transformer-level power flows up to the connection with the public 110-kV high-voltage grid (Fig. [5a](https://www.nature.com/articles/s41560-026-02144-0#Fig5)).

**Fig. 5: Recurrent current peaks in a UPS cause periodic 1-min power variations across all voltage levels up to the connection to the 110-kV high-voltage grid.**

**a**, An electrical diagram of the UPS at Building 3 (cf. Supplementary Table [1](https://www.nature.com/articles/s41560-026-02144-0#MOESM1)) and its connection to the public 110-kV grid, indicating the locations of all measurement devices used to trace the pattern. **b**, Recurrent narrow current peaks with 1-min periodicity are observed at the DC side of the battery. **c**, The self-consumption of the UPS is determined by subtracting the currents measured upstream and downstream of the UPS. The three curves represent the consumption of each phase (purple, dark green and light green for phases L1, L2 and L3, respectively). The drop observed in L3 can be attributed to a reduction in the consumption of the ventilation system or other auxiliary circuits, which are not monitored directly and therefore cannot be distinguished. Although no clear 1-min pattern is evident in the three-phase AC currents immediately before the grid side converter, Fourier analysis reveals a 1-min component. To exclude the influence of downstream loads, the AC output current after the building side inverter is subtracted such that only the internal consumption is considered. **d**, Fourier spectra show that the 1-min current pattern gives rise to a corresponding pattern in the active-power demand, which can be traced across two transformers and a bus bar up to the connection with the 110-kV grid.

[Source data](https://www.nature.com/articles/s41560-026-02144-0#MOESM7)

On the DC side of the UPS, at the battery terminals, narrow current peaks recurring every 1 min are observed (Fig. [5b](https://www.nature.com/articles/s41560-026-02144-0#Fig5)). The origin of these current peaks was independently confirmed by the manufacturer to be periodic self-tests executed by the battery EMS, confirming our interpretation. Similar minute-scale diagnostic routines are also documented for other commercial UPS systems (cf. Supplementary Note [8](https://www.nature.com/articles/s41560-026-02144-0#MOESM1)). On the AC side, after subtracting the demand of the building connected downstream of the UPS, these 1-min peaks are not clearly visible in the three-phase current time series. However, Fourier analysis reveals a persistent 1-min component in the currents (Fig. [5c](https://www.nature.com/articles/s41560-026-02144-0#Fig5)). The corresponding periodic power demand can be traced across several transformers and a bus bar, up to the transformer connecting to the public 110-kV grid (Fig. [5d](https://www.nature.com/articles/s41560-026-02144-0#Fig5)). These findings demonstrate that the pattern originating on the DC side propagates into the high-voltage grid.

We next investigate the temporal stability of the signal and the role of clock synchronization. Figure [6a](https://www.nature.com/articles/s41560-026-02144-0#Fig6) compares weekly waveforms *s*w(*t*) for three transformers supplying buildings equipped with UPS systems. The measurements reveal markedly different behaviours.

**Fig. 6: Waveform analysis of power time series for three different buildings measured at transformer level with known UPS.**

**a**, A comparison of weekly waveforms from 24 March 2024 to 3 July 2024. For Buildings 1 (cf. Transformer 1 with PQ ID 007 in Fig. [4](https://www.nature.com/articles/s41560-026-02144-0#Fig4)) and 4, the waveforms show a peak in power demand shortly after the beginning of each minute that is stable over the entire measurement period. For Building 3 (cf. Transformer 1 with PQ ID 023 in Fig. [5](https://www.nature.com/articles/s41560-026-02144-0#Fig5)), the waveform has a sawtooth shape that drifts slowly from week to week. **b**, The sawtooth step of the waveform at Building 3 (PQ ID 023) slowly drifts when NTP is disabled and is shifted by a discrete offset every night at 00:00 (UTC) when NTP is enabled.

[Source data](https://www.nature.com/articles/s41560-026-02144-0#MOESM8)

In Building 3, the waveform exhibits a slowly drifting sawtooth-like structure characterized by a sharp increase followed by a gradual decay. This behaviour is consistent with the response of the UPS and grid infrastructure to narrow periodic current peaks on the DC side, effectively acting as a low-pass filter.

By contrast, Buildings 1 and 4 show phase-stable waveforms with recurring power-demand peaks shortly after the beginning of each minute. Such coherent demand peaks naturally explain the emergence of the corresponding frequency pattern at grid level. Notably, the UPS systems in Buildings 1 and 4 originate from different manufacturers than the system installed in Building 3, emphasizing the importance of EMS implementation and control design.

Synchronization of digitally controlled devices is commonly achieved using the Network Time Protocol (NTP)[37](https://www.nature.com/articles/s41560-026-02144-0#ref-CR37 "Mills, D. L. Internet time synchronization: the network time protocol. IEEE Trans. Commun. 39, 1482–1493 (1991)."), which synchronizes local clocks to Coordinated Universal Time (UTC). To assess the influence of clock synchronization on UPS operation, we performed comparative measurements for the Building 3 EMS with NTP synchronization disabled and enabled.

With NTP disabled, the UPS self-test signatures show a slow drift with respect to UTC. With NTP enabled, the daily progression of the UPS self-test signatures becomes substantially more pronounced, reaching approximately 6 s per day (Fig. [6b](https://www.nature.com/articles/s41560-026-02144-0#Fig6), Supplementary Fig. [18](https://www.nature.com/articles/s41560-026-02144-0#MOESM1) and Supplementary Note [9](https://www.nature.com/articles/s41560-026-02144-0#MOESM1)). The progression is not continuous; instead, the phase remains stable during the day and is shifted at midnight. These observations indicate that periodic EMS routines operate on internally scheduled 1-min cycles whose timing is regularly reinitialized through external clock synchronization.

## Future developments and mitigation

The observed 1-min signal is already substantial in several power systems, although it has not reached a safety critical level. Nevertheless, several ongoing developments in modern energy systems make a further increase plausible and suggest that mitigation strategies should be implemented before the effect becomes more severe.

One important trend is the rapid deployment of digitally controlled energy devices. Battery capacities are increasing strongly [38](https://www.nature.com/articles/s41560-026-02144-0#ref-CR38 "European Market Outlook for Battery Storage 2025–2029 (SolarPower Europe, 2025);
https://api.solarpowereurope.org/uploads/SPE_European_Battery_Outlook_2025_62b89db476.pdf"),[39](https://www.nature.com/articles/s41560-026-02144-0#ref-CR39 "Ten-year Network Development Plan 2024: Opportunities for a More Efficient European Power System by 2050 (ENTSO-E, 2024);
https://eepublicdownloads.blob.core.windows.net/public-cdn-container/tyndp-documents/TYNDP2024/foropinion/Infrastructure_Gaps_Report.pdf") (Fig. [7a](https://www.nature.com/articles/s41560-026-02144-0#Fig7)), while data centres equipped with large UPS installations account for a growing fraction of electricity demand [7](https://www.nature.com/articles/s41560-026-02144-0#ref-CR7 "Chen, X., Wang, X., Colacelli, A., Lee, M. & Xie, L. Electricity demand and grid impacts of AI data centers: challenges and prospects. Nexus 3, 100162 (2026)."),[40](https://www.nature.com/articles/s41560-026-02144-0#ref-CR40 "Kamiya, G. & Bertoldi, P. Energy Consumption in Data Centres and Broadband Communication Networks in the EU Report No. JRC135926 (Publications Office of the European Union, 2024);
https://doi.org/10.2760/706491"). In Ireland, where data centres reached approximately 18% of national electricity demand in 2022 [41](https://www.nature.com/articles/s41560-026-02144-0#ref-CR41 "Data Centres Metered Electricity Consumption 2023: Key Findings (Central Statistics Office Ireland, 2024);
https://www.cso.ie/en/releasesandpublications/ep/p-dcmec/datacentresmeteredelectricityconsumption2023/keyfindings/"), the magnitude of the minute-scale frequency drop increased from approximately 3 mHz to 12 mHz between 2016 and 2021 (Fig. [7b](https://www.nature.com/articles/s41560-026-02144-0#Fig7)). More recent measurements show that individual frequency drops can exceed 20 mHz and, when superimposed on larger frequency excursions, can bring the system close to the limits of the target operating range (Supplementary Figs. [19](https://www.nature.com/articles/s41560-026-02144-0#MOESM1)–[21](https://www.nature.com/articles/s41560-026-02144-0#MOESM1) and Supplementary Note [10](https://www.nature.com/articles/s41560-026-02144-0#MOESM1)).

**Fig. 7: Future developments and mitigation of the 1-min signal.**

**a**, Battery energy storage system (BESS) capacities in Europe are increasing rapidly, as indicated by historical data [38](https://www.nature.com/articles/s41560-026-02144-0#ref-CR38 "European Market Outlook for Battery Storage 2025–2029 (SolarPower Europe, 2025);
https://api.solarpowereurope.org/uploads/SPE_European_Battery_Outlook_2025_62b89db476.pdf") and scenarios from ENTSO-E’s Ten-Year Network Development Plan (TYNDP)[39](https://www.nature.com/articles/s41560-026-02144-0#ref-CR39 "Ten-year Network Development Plan 2024: Opportunities for a More Efficient European Power System by 2050 (ENTSO-E, 2024);
https://eepublicdownloads.blob.core.windows.net/public-cdn-container/tyndp-documents/TYNDP2024/foropinion/Infrastructure_Gaps_Report.pdf"). **b**, Data centres equipped with large-scale UPS systems account for a rapidly increasing share of electricity demand. In Ireland, this coincides with a strong increase of the observed 1-min signal amplitude from 2016 to 2021 [40](https://www.nature.com/articles/s41560-026-02144-0#ref-CR40 "Kamiya, G. & Bertoldi, P. Energy Consumption in Data Centres and Broadband Communication Networks in the EU Report No. JRC135926 (Publications Office of the European Union, 2024);
https://doi.org/10.2760/706491"),[41](https://www.nature.com/articles/s41560-026-02144-0#ref-CR41 "Data Centres Metered Electricity Consumption 2023: Key Findings (Central Statistics Office Ireland, 2024);
https://www.cso.ie/en/releasesandpublications/ep/p-dcmec/datacentresmeteredelectricityconsumption2023/keyfindings/"). Ireland stands out because electricity consumption by data centres reached approximately 18% of national demand in 2022 (Germany: approximately 3% for comparison)[40](https://www.nature.com/articles/s41560-026-02144-0#ref-CR40 "Kamiya, G. & Bertoldi, P. Energy Consumption in Data Centres and Broadband Communication Networks in the EU Report No. JRC135926 (Publications Office of the European Union, 2024);
https://doi.org/10.2760/706491"),[41](https://www.nature.com/articles/s41560-026-02144-0#ref-CR41 "Data Centres Metered Electricity Consumption 2023: Key Findings (Central Statistics Office Ireland, 2024);
https://www.cso.ie/en/releasesandpublications/ep/p-dcmec/datacentresmeteredelectricityconsumption2023/keyfindings/"). **c**, The adoption of IoT devices is increasing worldwide [42](https://www.nature.com/articles/s41560-026-02144-0#ref-CR42 "Number of Internet of Things (IoT) Connected Devices Worldwide from 2019 to 2034, by Use Case (in Millions) (Statista, 2025);
https://www.statista.com/statistics/1194701/iot-connected-devices-use-case/"). Synchronized control routines may occur both in household appliances and network-connected IT devices. Examples include a smart baking oven and synchronized Raspberry-Pi clusters exhibiting periodic power patterns at 1-min and 30-s periods. **d**, Declining system inertia increases the frequency response to coherent power imbalances. In Mallorca, the withdrawal of a major coal-fired power plant [64](https://www.nature.com/articles/s41560-026-02144-0#ref-CR64 "Demanda eléctrica en tiempo real - Mallorca (sistema balear). Red Eléctrica de España
https://demanda.ree.es/visiona/baleares/mallorcaau/total
(accessed 22 May 2026).") from regular operation in 2019 coincided with an approximately 50% increase in the signal amplitude. **e**, The signal amplitude in the Scandinavian and Australian grids decreased substantially following revisions of primary frequency-control regulations (dashed vertical lines): in Scandinavia, the FCR-N requirements were revised in 2023 to suppress oscillations with a period of around 1 min [19](https://www.nature.com/articles/s41560-026-02144-0#ref-CR19 "Technical Requirements for Frequency Containment Reserve Provision in the Nordic Synchronous Area (ENTSO-E, 2023);
https://www.svk.se/495d0c/siteassets/aktorsportalen/bidra-med-reserver/om-olika-reserver/fcr/fcr-technical-requirements-may-23.pdf"), and in Australia, all scheduled generators have been required to provide primary frequency response (PFR) since 2020 (ref. [43](https://www.nature.com/articles/s41560-026-02144-0#ref-CR43 "Implementation of the National Electricity Amendment (Mandatory Primary Frequency Response) Rule 2020: Status as at 9 October 2020 (AEMO, 2020);
https://www.aemo.com.au/-/media/files/initiatives/primary-frequency-response/2020/pfr-implementation-report-12-oct-20.pdf")). **f**, The impact of a software update of the battery EMS of the UPS analysed in Fig. [5](https://www.nature.com/articles/s41560-026-02144-0#Fig5), designed to avoid strictly periodic current peaks.

[Source data](https://www.nature.com/articles/s41560-026-02144-0#MOESM9)

In parallel, the proliferation of IoT devices [42](https://www.nature.com/articles/s41560-026-02144-0#ref-CR42 "Number of Internet of Things (IoT) Connected Devices Worldwide from 2019 to 2034, by Use Case (in Millions) (Statista, 2025);
https://www.statista.com/statistics/1194701/iot-connected-devices-use-case/") introduces additional loads operating with externally synchronized clocks or periodic software routines. We observe corresponding periodic power signatures both in a smart baking oven and in Raspberry-Pi clusters (Fig. [7c](https://www.nature.com/articles/s41560-026-02144-0#Fig7), Supplementary Figs. [22](https://www.nature.com/articles/s41560-026-02144-0#MOESM1) and [23](https://www.nature.com/articles/s41560-026-02144-0#MOESM1) and Supplementary Note [11](https://www.nature.com/articles/s41560-026-02144-0#MOESM1)).

A second important trend is the reduction of system inertia during the transition from fossil-fuelled synchronous generation to converter-interfaced renewable generation. Lower inertia increases the frequency response to a given coherent power imbalance [17](https://www.nature.com/articles/s41560-026-02144-0#ref-CR17 "Milano, F., Dörfler, F., Hug, G., Hill, D. J. & Verbic, G. Foundations and challenges of low-inertia systems. In 2018 Power Systems Computation Conference (PSCC), 1–25 (2018).") and has been linked to stronger ULFOs [14](https://www.nature.com/articles/s41560-026-02144-0#ref-CR14 "Seppänen, J., Lehtonen, M., Kuivaniemi, M. & Haarla, L. On the relationship between ultra low frequency oscillation and power system inertia. In 2023 IEEE Power and Energy Society General Meeting (PESGM), 1–5 (IEEE, 2023)."). This effect is illustrated by the Mallorca grid, where the withdrawal of a major coal-fired power plant from regular operation at the end of 2019 coincided with an increase of the 1-min amplitude by approximately 50% (Fig. [7d](https://www.nature.com/articles/s41560-026-02144-0#Fig7), Supplementary Fig. [24](https://www.nature.com/articles/s41560-026-02144-0#MOESM1) and Supplementary Note [12](https://www.nature.com/articles/s41560-026-02144-0#MOESM1)). This increase is consistent with a reduction in effective inertia, although concurrent changes in frequency-control operation may also have contributed.

These examples indicate that the 1-min signal can grow through both sides of the frequency response: coherent power variations may increase as clocked devices proliferate, while the same power variations lead to larger frequency deviations as inertia decreases. This does not imply an imminent stability threat, but it makes the phenomenon relevant for future system operation, especially because deterministic deviations can add to stochastic fluctuations and other recurrent patterns (Supplementary Figs. [25](https://www.nature.com/articles/s41560-026-02144-0#MOESM1)–[30](https://www.nature.com/articles/s41560-026-02144-0#MOESM1) and Supplementary Note [13](https://www.nature.com/articles/s41560-026-02144-0#MOESM1)).

Several mitigation routes are available. System-level measures can reduce the frequency impact of deterministic disturbances. In particular, revisions of frequency-control regulations in the Scandinavian [19](https://www.nature.com/articles/s41560-026-02144-0#ref-CR19 "Technical Requirements for Frequency Containment Reserve Provision in the Nordic Synchronous Area (ENTSO-E, 2023);
https://www.svk.se/495d0c/siteassets/aktorsportalen/bidra-med-reserver/om-olika-reserver/fcr/fcr-technical-requirements-may-23.pdf") and Australian [43](https://www.nature.com/articles/s41560-026-02144-0#ref-CR43 "Implementation of the National Electricity Amendment (Mandatory Primary Frequency Response) Rule 2020: Status as at 9 October 2020 (AEMO, 2020);
https://www.aemo.com.au/-/media/files/initiatives/primary-frequency-response/2020/pfr-implementation-report-12-oct-20.pdf") grids substantially reduced the amplitude of the observed signal (Fig. [7e](https://www.nature.com/articles/s41560-026-02144-0#Fig7)). However, such measures primarily mitigate the frequency response rather than the underlying coherent power modulation.

The root cause of cyber–physical power oscillations can be addressed directly through control design. Clocked internal routines should avoid simultaneous power adjustments across many devices, for example, by using randomized scheduling, adaptive offsets or event-driven rather than strictly time-driven execution. This approach is technically feasible for utility-scale batteries and UPS systems, where software updates can be deployed centrally. Indeed, following our communication with the manufacturer, a software update was issued that removed the strict periodicity of the current peaks in one battery block of the investigated UPS system (Fig. [7f](https://www.nature.com/articles/s41560-026-02144-0#Fig7)). However, self-tests still occur at discrete times such that the root cause has been resolved only partially (Supplementary Fig. [31](https://www.nature.com/articles/s41560-026-02144-0#MOESM1) and Supplementary Note [9](https://www.nature.com/articles/s41560-026-02144-0#MOESM1)).

Mitigation may be more challenging for household-scale IoT devices, where software updates and control standards are less coordinated than in utility-scale infrastructure. These observations highlight the importance of early coordination between manufacturers, system operators and regulators as digitally synchronized energy devices continue to proliferate.

## Discussion

Cybersecurity and the management of power imbalances are widely recognized as central challenges for future power systems [44](https://www.nature.com/articles/s41560-026-02144-0#ref-CR44 "Frequency Stability in Long-term Scenarios and Relevant Requirements (ENTSO-E, 2021);
https://eepublicdownloads.azureedge.net/clean-documents/Publications/ENTSO-Egeneralpublications/211203_Long_term_frequency_stability_scenarios_for_publication.pdf"),[45](https://www.nature.com/articles/s41560-026-02144-0#ref-CR45 "Enhancing Cyber Resilience in Electricity Systems (International Energy Agency, 2021);
https://www.iea.org/reports/enhancing-cyber-resilience-in-electricity-systems"). Maintaining a stable frequency becomes increasingly difficult as system inertia declines [17](https://www.nature.com/articles/s41560-026-02144-0#ref-CR17 "Milano, F., Dörfler, F., Hug, G., Hill, D. J. & Verbic, G. Foundations and challenges of low-inertia systems. In 2018 Power Systems Computation Conference (PSCC), 1–25 (2018)."),[22](https://www.nature.com/articles/s41560-026-02144-0#ref-CR22 "Hatziargyriou, N. et al. Stability Definitions and Characterization of Dynamic Behavior in Systems with High Penetration of Power Electronic Interfaced Technologies (IEEE PES, 2020);
https://doi.org/10.17023/zwwp-vv02"), while cybersecurity is threatened by failures and malicious attacks [9](https://www.nature.com/articles/s41560-026-02144-0#ref-CR9 "Pasqualetti, F., Dörfler, F. & Bullo, F. Attack detection and identification in cyber–physical systems. IEEE Trans. Autom. Control 58, 2715–2729 (2013)."). Here, we highlight a more subtle systemic challenge arising from the cyber–physical coupling of modern power systems.

We show that the grid frequency in power systems across five continents exhibits a persistent and highly stable recurrence with a period of exactly 1 min. While aspects of such ULFOs have been reported previously [36](https://www.nature.com/articles/s41560-026-02144-0#ref-CR36 "Wei, F. et al. Multi-time-scale analysis on power grid frequency characteristics. IEEE Trans. Power Syst. 41, 884–899 (2026)."), the origin of their long-term coherence has remained unclear. We provide evidence that the observed signal is probably linked to unintended synchronization of digitally controlled devices whose control actions are scheduled by clocked software processes. When synchronized across many devices, such routines can generate coherent power variations at system scale. The growing relevance of this mechanism is illustrated by the Irish power system, where the initial frequency drop of the 1-min signal increased to 12 mHz in 2021, reaching substantially larger values during individual periods (Supplementary Fig. [20](https://www.nature.com/articles/s41560-026-02144-0#MOESM1)). This increase coincides with the rapid expansion of data centres equipped with large UPS installations, whose electricity demand reached approximately 18% of national consumption in 2022. The growing importance of data centres for power-system planning and operation has brought renewed attention to their dynamic interaction with the grid [7](https://www.nature.com/articles/s41560-026-02144-0#ref-CR7 "Chen, X., Wang, X., Colacelli, A., Lee, M. & Xie, L. Electricity demand and grid impacts of AI data centers: challenges and prospects. Nexus 3, 100162 (2026)."),[46](https://www.nature.com/articles/s41560-026-02144-0#ref-CR46 "Jiménez-Ruiz, A. & Milano, F. Data centre model for transient stability analysis of power systems. IET Gener. Transm. Distrib. 20, e70381 (2026)."), but the role of synchronized UPS control has largely been overlooked. Future work should develop detailed mechanistic models that capture the interaction between power-system operation and digitally controlled loads, including data centres and clock-synchronized devices.

More generally, many modern energy devices, including large-scale batteries, photovoltaic inverters and smart appliances, rely on embedded software and external time synchronization and may therefore contribute to coherent cyber–physical power oscillations. Beyond the highly coherent behaviour observed here, model-based studies suggest that even partial temporal coherence across large device populations may already produce measurable aggregate effects on the power balance [47](https://www.nature.com/articles/s41560-026-02144-0#ref-CR47 "Chatzivasileiadis, S. et al. Micro-flexibility: challenges for power system modeling and control. Electr. Power Syst. Res. 216, 109002 (2023)."). The rapid projected expansion of battery storage systems and IoT devices is therefore likely to increase the relevance of such synchronization effects in future power systems. At the same time, decreasing system inertia increases the frequency response to coherent power imbalances [17](https://www.nature.com/articles/s41560-026-02144-0#ref-CR17 "Milano, F., Dörfler, F., Hug, G., Hill, D. J. & Verbic, G. Foundations and challenges of low-inertia systems. In 2018 Power Systems Computation Conference (PSCC), 1–25 (2018)."), while hydropower turbine-governor dynamics may further amplify coherent oscillations through resonant feedback effects.

Mitigation can proceed along two complementary routes. System-level measures, such as stricter primary frequency-control requirements, can reduce the frequency impact of deterministic disturbances, as observed in the Scandinavian and Australian grids. However, such measures mainly suppress the response to coherent power modulation rather than its origin. Addressing the root cause requires improved control design, for example through randomized scheduling, adaptive offsets or event-driven instead of strictly time-driven routines. Our measurements demonstrate that such mitigation is technically feasible, although coordinated implementation across heterogeneous large-scale device populations remains challenging.

Our results highlight the increasingly important role of software and digital control in modern energy systems. Traditionally, power-system stability has primarily been analysed in terms of physical infrastructure and control dynamics. Increasing digitalization, however, requires an integrated perspective in which control logic, software timing and synchronization are treated as integral components of power-system stability. More broadly, our findings demonstrate that collective effects are central to future cyber–physical power systems and can emerge both through interactions in the physical grid and through digital interconnections.

## Methods

### Data sources and processing

To monitor grid behaviour, load profiles and power flows on the Jülich research campus, over 80 PQI-DA smart power quality (PQ) meters by a-eberle have been deployed [48](https://www.nature.com/articles/s41560-026-02144-0#ref-CR48 "PQI-DA Smart (a-eberle, 2025);
https://www.a-eberle.de/en/products/power-quality-analyzer-pqi-da-smart/"). These devices comply with the following PQ standards: EN 50160, IEEE 519, IEC 61000-2-2, IEC 61000-4-30, IEC 61000-2-12, IEC 61000-4-7, IEC 61000-2-4 and IEC 61000-4-15, as well as NRS 048. In particular, they are Class A devices that comply with the IEC 61000-4-30 Ed. 3 (2015) standards [49](https://www.nature.com/articles/s41560-026-02144-0#ref-CR49 "Electromagnetic compatibility (EMC) – Part 4-30: testing and measurement techniques – power quality measurement methods (IEC International Electrotechnical Commission, 2015)."), ensuring highly accurate measurements of frequency, voltage and current at the fundamental frequency, as well as other PQ phenomena. Communication with the meters relies on the Modbus Transmission Control Protocol, which provides data via standardized registers for various aggregation intervals, from 200 ms up to 2 h, as defined in the IEC standards [50](https://www.nature.com/articles/s41560-026-02144-0#ref-CR50 "Industrial communication networks – Fieldbus specifications – Part 5-15: application layer service definition – Type 15 elements (Modbus) (IEC International Electrotechnical Commission, 2019)."),[51](https://www.nature.com/articles/s41560-026-02144-0#ref-CR51 "Industrial communication networks – Fieldbus specifications – Part 6-15: application layer protocol specification – Type 15 elements (Modbus) (IEC International Electrotechnical Commission, 2019)."). During normal operation, key electrical quantities such as frequency, voltage, current and power are stored on the FIWARE-based Campus Information and Communications Technology platform [52](https://www.nature.com/articles/s41560-026-02144-0#ref-CR52 "Zimmer, M., Carta, D., Pesch, T. & Benigni, A. Signal recovery in power systems by correlated gaussian processes. IEEE Open J. Ind. Electron. Soc. 5, 692–703 (2024)."). We use data with 3-s resolution. Additional details are provided in Supplementary Note [7](https://www.nature.com/articles/s41560-026-02144-0#MOESM1).

Frequency data with a resolution of 1 s on transmission grid-level for the Continental European, British and Scandinavian grids are provided by the transmission system operators [53](https://www.nature.com/articles/s41560-026-02144-0#ref-CR53 "Frequency data for the CE grid. Netztransparenz
https://www.netztransparenz.de/de-de/Regelenergie/Daten-Regelreserve/
(accessed 22 May 2026)."),[54](https://www.nature.com/articles/s41560-026-02144-0#ref-CR54 "Frequency data for the GB grid. NESO
https://www.neso.energy/data-portal/system-frequency-data
(accessed 22 May 2026)."),[55](https://www.nature.com/articles/s41560-026-02144-0#ref-CR55 "Frequency data for the Nordic grid. Fingrid
https://data.fingrid.fi/en/datasets/339
(accessed 22 May 2026)."). The data have been pre-processed using the scripts provided in ref. [56](https://www.nature.com/articles/s41560-026-02144-0#ref-CR56 "Kruse, J., Schäfer, B. & Witthaut, D. Pre-processed power grid frequency time series. Zenodo
https://doi.org/10.5281/zenodo.3744120
(2021)."). We use data from 1 January 2015 until 31 December 2025. For Australia, frequency data is available with 4-s resolution from the transmission system operator [57](https://www.nature.com/articles/s41560-026-02144-0#ref-CR57 "Frequency and time deviation monitoring. AEMO
https://www.aemo.com.au/energy-systems/electricity/national-electricity-market-nem/system-operations/ancillary-services/frequency-and-time-deviation-monitoring
(accessed 22 May 2026)."). In addition, we obtained frequency data from Ireland for 2016 with 1-s resolution directly from the network operator EirGrid. Low-resolution frequency data in Ireland for 1 month in 2026 has been scraped directly from EirGrid’s Smart Grid Dashboard [58](https://www.nature.com/articles/s41560-026-02144-0#ref-CR58 "Smart grid dashboard: frequency data (2026). EirGrid Group
https://www.smartgriddashboard.com/all/frequency/
(accessed 22 May 2026).").

Frequency data for USA, Malaysia, South Korea, South Africa, Mallorca, Ireland (2021) and Iceland are measured on the distribution grid-level using the Electrical Data Recorder described in refs. [59](https://www.nature.com/articles/s41560-026-02144-0#ref-CR59 "Jumar, R., Maaß, H., Schäfer, B., Rydin Gorjão, L. & Hagenmeyer, V. Database of power grid frequency measurements. Preprint at
https://doi.org/10.48550/arXiv.2006.01771
(2020)."),[60](https://www.nature.com/articles/s41560-026-02144-0#ref-CR60 "Rydin Gorjão, L. et al. Open database analysis of scaling and spatio-temporal properties of power grid frequencies. Nat. Commun. 11, 6362 (2020)."); details are provided in Supplementary Note [14](https://www.nature.com/articles/s41560-026-02144-0#MOESM1). The Electrical Data Recorder measures voltage and current waveforms at high temporal resolution and derives the fundamental system frequency using a filtered zero-crossing method [61](https://www.nature.com/articles/s41560-026-02144-0#ref-CR61 "Förstner, E. et al. Experimental evaluation and systematic-error reduction of frequency estimation using the zero-crossing technique for dynamic power grids. In 2022 International Conference on Smart Grid Synchronized Measurements and Analytics (SGSMA) (eds Nordström, L. et al.) 1–6 (IEEE, 2022)."). This approach is well suited for capturing frequency variations on time scales of seconds and above, as considered in this study, whereas the analysis of faster dynamics or strongly modulated signals may require alternative frequency definitions and demodulation methods [62](https://www.nature.com/articles/s41560-026-02144-0#ref-CR62 "Cai, B., Wang, S. & Yuan, X. Amplitude/frequency continuous modulation law of oscillation signals in power systems and demodulation method. In 2025 IEEE International Conference on Power and Integrated Energy Systems (ICPIES), 211–218 (IEEE, 2025)."). These standalone measurements vary in length and cover approximately 5 years (Mallorca), 10 months (US West), 4 months (South Africa), 2.5 weeks (US East), 2.5 months (Iceland), 3.5 months (South Korea), 1 month (Ireland, 2021) and 1.5 weeks (Malaysia). A comparison of frequency data from the three different sources is provided in Supplementary Note [14](https://www.nature.com/articles/s41560-026-02144-0#MOESM1) and Supplementary Fig. [32](https://www.nature.com/articles/s41560-026-02144-0#MOESM1). The exact measurement periods are provided in Supplementary Figs. [33](https://www.nature.com/articles/s41560-026-02144-0#MOESM1)–[35](https://www.nature.com/articles/s41560-026-02144-0#MOESM1).

Frequency data from Belfort, Málaga and Porto for illustrating the inter-area oscillations over a period of 20 s, with a time resolution of 0.1 s in Fig. [2d](https://www.nature.com/articles/s41560-026-02144-0#Fig2), is provided by the Gridradar initiative [63](https://www.nature.com/articles/s41560-026-02144-0#ref-CR63 "MagnaGen GmbH. Gridradar
https://gridradar.net
(2021)."). The remaining data from Karlsruhe, Elk and Kaunas in Fig. [2d](https://www.nature.com/articles/s41560-026-02144-0#Fig2) are also measured by Electrical Data Recorder devices.

The data on electricity generation from the coal-fired power plant in Mallorca are taken from ref. [64](https://www.nature.com/articles/s41560-026-02144-0#ref-CR64 "Demanda eléctrica en tiempo real - Mallorca (sistema balear). Red Eléctrica de España
https://demanda.ree.es/visiona/baleares/mallorcaau/total
(accessed 22 May 2026)."). In our analysis, the 5-min resolution dataset is aggregated into daily averages.

If not explicitly stated otherwise, we use UTC to index the time series in order to avoid ambiguity due to daylight saving time. As we analyse the hourly amplitudes, we further remove full hours of time series data if a single data point is missing or corrupted. The missing data after pre-processing is summarized in Supplementary Figs. [33](https://www.nature.com/articles/s41560-026-02144-0#MOESM1)–[35](https://www.nature.com/articles/s41560-026-02144-0#MOESM1).

### Waveform and amplitude

Let *f*(*t*) be a discrete time series for some time interval *t* ∈ \[*t*0, *t*1\] of length *τ* = *t*1 − *t*0 with sampling time Δ*t*. In particular, we consider the grid frequency and the real power load. We compute the average waveform *s*(*t*) of a recurrent temporal pattern with period *T* by averaging over blocks of length *T*. That is, we divide the interval \[*t*0, *t*1\] into \\(N=\\frac{{t}\_{1}-{t}\_{0}}{T}\\) blocks of length *T* and average *f*(*t*) over these blocks.

$${s}\_{\[{t}\_{0},{t}\_{1}\],T}(t):=\\frac{1}{N}\\sum \_{n=0}^{N-1}f({t}\_{0}+nT+t)-{\\langle f(t)\\rangle }\_{\[{t}\_{0},{t}\_{1}\]},$$

(3)

for *t* ∈ \[0, *T*). From the waveform, we define the amplitude \\({a}\_{\[{t}\_{0},{t}\_{1}\],T}\\) of the pattern over the interval \[*t*0, *t*1\] as

$${a}\_{\[{t}\_{0},{t}\_{1}\],T}=\\frac{1}{2}\\left(\\mathop{\\max }\\limits\_{t\\in \[0,T)}{s}\_{\[{t}\_{0},{t}\_{1}\],T}(t)-\\mathop{\\min }\\limits\_{t\\in \[0,T)}{s}\_{\[{t}\_{0},{t}\_{1}\],T}(t)\\right).$$

(4)

To quantify how waveforms and amplitudes evolve over time, we divide the entire measurement period into non-overlapping 1-h segments and evaluate the waveform and its amplitude separately for each segment. Specifically, for each hour *h* we set *t*0 = *h* and *t*1 = *h* + 1 h, and denote the resulting waveform as *s**h*,*T*(*t*) and the corresponding amplitude as *a**h*,*T*. For simplicity, we drop the subscript \\(T=1\\,\\min\\) in the main text.

Similarly, we define daily *s*d,*T*(*t*), weekly *s*w,*T*(*t*) and monthly *s*m,*T*(*t*) waveforms and amplitudes by setting *τ* to 1 day, 1 week and 1 month, respectively, and averaging over the corresponding number of blocks *N*.

## Data availability

The datasets generated and analysed in this study that are not available from existing public sources are available via Zenodo at [https://doi.org/10.5281/zenodo.17366692](https://doi.org/10.5281/zenodo.17366692) (ref. [65](https://www.nature.com/articles/s41560-026-02144-0#ref-CR65 "De Din, E., Carta, D. & Hartmann, C. Data for ‘One-minute frequency disturbances from unintended synchronization in cyber–physical power systems’. Zenodo
https://doi.org/10.5281/zenodo.17366692
(2026).")), with two exceptions arising from third-party ownership. The Gridradar data used in the main text and [Supplementary Information](https://www.nature.com/articles/s41560-026-02144-0#MOESM1) are proprietary third-party data provided by MagnaGen GmbH and are not publicly available; access requires a subscription to the Gridradar service [63](https://www.nature.com/articles/s41560-026-02144-0#ref-CR63 "MagnaGen GmbH. Gridradar
https://gridradar.net
(2021)."). The 1-s resolution Irish frequency data from 2016 were provided by EirGrid (Irish transmission system operator) under a data-sharing agreement and are available from EirGrid on reasonable request, subject to an equivalent agreement. [Source data](https://www.nature.com/articles/s41560-026-02144-0#Sec11) are provided with this paper.

## References

1.  Denholm, P. et al. The challenges of achieving a 100% renewable electricity system in the United States. *Joule* **5**, 1331–1352 (2021).

    [Article](https://doi.org/10.1016%2Fj.joule.2021.03.028)  [Google Scholar](http://scholar.google.com/scholar_lookup?&title=The%20challenges%20of%20achieving%20a%20100%25%20renewable%20electricity%20system%20in%20the%20United%20States&journal=Joule&doi=10.1016%2Fj.joule.2021.03.028&volume=5&pages=1331-1352&publication_year=2021&author=Denholm%2CP)

2.  Marot, A. et al. Perspectives on future power system control centers for energy transition. *J. Mod. Power Syst. Clean Energy* **10**, 328–344 (2022).

    [Article](https://doi.org/10.35833%2FMPCE.2021.000673)  [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Perspectives%20on%20future%20power%20system%20control%20centers%20for%20energy%20transition&journal=J.%20Mod.%20Power%20Syst.%20Clean%20Energy&doi=10.35833%2FMPCE.2021.000673&volume=10&pages=328-344&publication_year=2022&author=Marot%2CA)

3.  Morales, J. M., Conejo, A. J., Madsen, H., Pinson, P. & Zugno, M. *Integrating Renewables in Electricity Markets: Operational Problems* Vol. 205 (Springer Science & Business Media, 2013).

4.  Muratori, M. Impact of uncoordinated plug-in electric vehicle charging on residential power demand. *Nat. Energy* **3**, 193–201 (2018).

    [Article](https://doi.org/10.1038%2Fs41560-017-0074-z)  [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Impact%20of%20uncoordinated%20plug-in%20electric%20vehicle%20charging%20on%20residential%20power%20demand&journal=Nat.%20Energy&doi=10.1038%2Fs41560-017-0074-z&volume=3&pages=193-201&publication_year=2018&author=Muratori%2CM)

5.  Liu, D., Carta, D., Xhonneux, A., Müller, D. & Benigni, A. Short-term control of heat pumps to support power grid operation. *IEEE Open J. Ind. Electron. Soc.* **5**, 1221–1238 (2024).

    [Article](https://doi.org/10.1109%2FOJIES.2024.3486560)  [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Short-term%20control%20of%20heat%20pumps%20to%20support%20power%20grid%20operation.&journal=IEEE%20Open%20J.%20Ind.%20Electron.%20Soc.&doi=10.1109%2FOJIES.2024.3486560&volume=5&pages=1221-1238&publication_year=2024&author=Liu%2CD&author=Carta%2CD&author=Xhonneux%2CA&author=M%C3%BCller%2CD&author=Benigni%2CA)

6.  Farhangi, H. The path of the smart grid. *IEEE Power Energy Mag.* **8**, 18–28 (2010).

    [Article](https://doi.org/10.1109%2FMPE.2009.934876)  [Google Scholar](http://scholar.google.com/scholar_lookup?&title=The%20path%20of%20the%20smart%20grid&journal=IEEE%20Power%20Energy%20Mag.&doi=10.1109%2FMPE.2009.934876&volume=8&pages=18-28&publication_year=2010&author=Farhangi%2CH)

7.  Chen, X., Wang, X., Colacelli, A., Lee, M. & Xie, L. Electricity demand and grid impacts of AI data centers: challenges and prospects. *Nexus* **3**, 100162 (2026).

    [Article](https://doi.org/10.1016%2Fj.ynexs.2026.100162)  [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Electricity%20demand%20and%20grid%20impacts%20of%20AI%20data%20centers%3A%20challenges%20and%20prospects.&journal=Nexus&doi=10.1016%2Fj.ynexs.2026.100162&volume=3&publication_year=2026&author=Chen%2CX&author=Wang%2CX&author=Colacelli%2CA&author=Lee%2CM&author=Xie%2CL)

8.  Xie, L., Singh, C., Mitter, S. K., Dahleh, M. A. & Oren, S. S. Toward carbon-neutral electricity and mobility: is the grid infrastructure ready? *Joule* **5**, 1908–1913 (2021).

    [Article](https://doi.org/10.1016%2Fj.joule.2021.06.011)  [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Toward%20carbon-neutral%20electricity%20and%20mobility%3A%20is%20the%20grid%20infrastructure%20ready%3F&journal=Joule&doi=10.1016%2Fj.joule.2021.06.011&volume=5&pages=1908-1913&publication_year=2021&author=Xie%2CL&author=Singh%2CC&author=Mitter%2CSK&author=Dahleh%2CMA&author=Oren%2CSS)

9.  Pasqualetti, F., Dörfler, F. & Bullo, F. Attack detection and identification in cyber–physical systems. *IEEE Trans. Autom. Control* **58**, 2715–2729 (2013).

    [Article](https://doi.org/10.1109%2FTAC.2013.2266831)  [MathSciNet](http://www.ams.org/mathscinet-getitem?mr=3125984)  [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Attack%20detection%20and%20identification%20in%20cyber%E2%80%93physical%20systems&journal=IEEE%20Trans.%20Autom.%20Control&doi=10.1109%2FTAC.2013.2266831&volume=58&pages=2715-2729&publication_year=2013&author=Pasqualetti%2CF&author=D%C3%B6rfler%2CF&author=Bullo%2CF)

10. Andersson, G. et al. Causes of the 2003 major grid blackouts in North America and Europe, and recommended means to improve system dynamic performance. *IEEE Trans. Power Syst.* **20**, 1922–1928 (2005).

    [Article](https://doi.org/10.1109%2FTPWRS.2005.857942)  [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Causes%20of%20the%202003%20major%20grid%20blackouts%20in%20North%20America%20and%20Europe%2C%20and%20recommended%20means%20to%20improve%20system%20dynamic%20performance&journal=IEEE%20Trans.%20Power%20Syst.&doi=10.1109%2FTPWRS.2005.857942&volume=20&pages=1922-1928&publication_year=2005&author=Andersson%2CG)

11. Liang, G., Weller, S. R., Zhao, J., Luo, F. & Dong, Z. Y. The 2015 Ukraine blackout: implications for false data injection attacks. *IEEE Trans. Power Syst.* **32**, 3317–3318 (2017).

    [Article](https://doi.org/10.1109%2FTPWRS.2016.2631891)  [Google Scholar](http://scholar.google.com/scholar_lookup?&title=The%202015%20Ukraine%20blackout%3A%20implications%20for%20false%20data%20injection%20attacks&journal=IEEE%20Trans.%20Power%20Syst.&doi=10.1109%2FTPWRS.2016.2631891&volume=32&pages=3317-3318&publication_year=2017&author=Liang%2CG&author=Weller%2CSR&author=Zhao%2CJ&author=Luo%2CF&author=Dong%2CZY)

12. Yan, R., Masood, N., Saha, T. K., Bai, F. & Gu, H. The anatomy of the 2016 South Australia blackout: a catastrophic event in a high renewable network. *IEEE Trans. Power Syst.* **33**, 5374–5388 (2018).

    [Article](https://doi.org/10.1109%2FTPWRS.2018.2820150)  [Google Scholar](http://scholar.google.com/scholar_lookup?&title=The%20anatomy%20of%20the%202016%20South%20Australia%20blackout%3A%20a%20catastrophic%20event%20in%20a%20high%20renewable%20network&journal=IEEE%20Trans.%20Power%20Syst.&doi=10.1109%2FTPWRS.2018.2820150&volume=33&pages=5374-5388&publication_year=2018&author=Yan%2CR&author=Masood%2CN&author=Saha%2CTK&author=Bai%2CF&author=Gu%2CH)

13. Seppänen, J., Lehtonen, M., Kuivaniemi, M. & Haarla, L. Long term characteristics of ultra low frequency oscillations in the Nordic power system. In *2022 IEEE PES Innovative Smart Grid Technologies Conference Europe (ISGT-Europe)*, 1–5 (IEEE, 2022).

14. Seppänen, J., Lehtonen, M., Kuivaniemi, M. & Haarla, L. On the relationship between ultra low frequency oscillation and power system inertia. In *2023 IEEE Power and Energy Society General Meeting (PESGM)*, 1–5 (IEEE, 2023).

15. Odelbrink, C. *Design of an Alternative Frequency Controller for Hydropower Stations: Reducing the 60-second Floating in the Grid Frequency of the Nordic Power System.* Master’s thesis, Uppsala Univ. (2014); [https://www.diva-portal.org/smash/get/diva2:743757/FULLTEXT01.pdf](https://www.diva-portal.org/smash/get/diva2:743757/FULLTEXT01.pdf)

16. Agneholm, E. & Jansson, E. A. *FCP Project Summary Report* (ENTSO-E AISBL, 2017); [https://www.fingrid.fi/globalassets/dokumentit/fi/yhtio/tki-toiminta/raportit/fcp-project-summary-report.pdf](https://www.fingrid.fi/globalassets/dokumentit/fi/yhtio/tki-toiminta/raportit/fcp-project-summary-report.pdf)

17. Milano, F., Dörfler, F., Hug, G., Hill, D. J. & Verbic, G. Foundations and challenges of low-inertia systems. In *2018 Power Systems Computation Conference (PSCC)*, 1–25 (2018).

18. Yang, W. et al. Wear and tear on hydro power turbines–influence from primary frequency control. *Renew. Energy* **87**, 88–95 (2016).

    [Article](https://doi.org/10.1016%2Fj.renene.2015.10.009)  [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Wear%20and%20tear%20on%20hydro%20power%20turbines%E2%80%93influence%20from%20primary%20frequency%20control&journal=Renew.%20Energy&doi=10.1016%2Fj.renene.2015.10.009&volume=87&pages=88-95&publication_year=2016&author=Yang%2CW)

19. *Technical Requirements for Frequency Containment Reserve Provision in the Nordic Synchronous Area* (ENTSO-E, 2023); [https://www.svk.se/495d0c/siteassets/aktorsportalen/bidra-med-reserver/om-olika-reserver/fcr/fcr-technical-requirements-may-23.pdf](https://www.svk.se/495d0c/siteassets/aktorsportalen/bidra-med-reserver/om-olika-reserver/fcr/fcr-technical-requirements-may-23.pdf)

20. P1 – policy 1: load-frequency control and performance. In *UCTE Operations Handbook* [https://www.entsoe.eu/fileadmin/user_upload/_library/publications/entsoe/Operation_Handbook/introduction_v25.pdf](https://www.entsoe.eu/fileadmin/user_upload/_library/publications/entsoe/Operation_Handbook/introduction_v25.pdf) (Union for the Co-ordination of Transmission of Electricity, 2009).

21. De Boeck, S. et al. Review of defence plans in Europe: current status, strengths and opportunities. *CIGRE Sci. Eng.* **5**, 6–16 (2016).

    [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Review%20of%20defence%20plans%20in%20Europe%3A%20current%20status%2C%20strengths%20and%20opportunities&journal=CIGRE%20Sci.%20Eng.&volume=5&pages=6-16&publication_year=2016&author=Boeck%2CS)

22. Hatziargyriou, N. et al. *Stability Definitions and Characterization of Dynamic Behavior in Systems with High Penetration of Power Electronic Interfaced Technologies* (IEEE PES, 2020); [https://doi.org/10.17023/zwwp-vv02](https://doi.org/10.17023/zwwp-vv02)

23. Schäfer, B., Beck, C., Aihara, K., Witthaut, D. & Timme, M. Non-Gaussian power grid frequency fluctuations characterized by Lévy-stable laws and superstatistics. *Nat. Energy* **3**, 119–126 (2018).

    [Article](https://doi.org/10.1038%2Fs41560-017-0058-z)  [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Non-Gaussian%20power%20grid%20frequency%20fluctuations%20characterized%20by%20L%C3%A9vy-stable%20laws%20and%20superstatistics&journal=Nat.%20Energy&doi=10.1038%2Fs41560-017-0058-z&volume=3&pages=119-126&publication_year=2018&author=Sch%C3%A4fer%2CB&author=Beck%2CC&author=Aihara%2CK&author=Witthaut%2CD&author=Timme%2CM)

24. *Report on Deterministic Frequency Deviations* (ENTSO-E, 2020); [https://eepublicdownloads.entsoe.eu/clean-documents/Publications/SOC/Report_Deterministic_Frequency_Deviations_final_version.pdf](https://eepublicdownloads.entsoe.eu/clean-documents/Publications/SOC/Report_Deterministic_Frequency_Deviations_final_version.pdf)

25. Borsche, T., Ulbig, A. & Andersson, G. A new frequency control reserve framework based on energy-constrained units. In *2014 Power Systems Computation Conference*, 1–7 (IEEE, 2014).

26. Weissbach, T. & Welfonder, E. High frequency deviations within the European power system: origins and proposals for improvement. In *2009 IEEE/PES Power Systems Conference and Exposition*, 1–6 (IEEE, 2009).

27. Kruse, J., Schäfer, B. & Witthaut, D. Exploring deterministic frequency deviations with explainable AI. In *2021 IEEE International Conference on Communications, Control, and Computing Technologies for Smart Grids (SmartGridComm)*, 133–139 (IEEE, 2021).

28. Böttcher, P. C., Hartmann, C., Benigni, A., Pesch, T. & Witthaut, D. Impact of market reforms on deterministic frequency deviations in the European power grid. Preprint at [https://doi.org/10.48550/arXiv.2602.09645](https://doi.org/10.48550/arXiv.2602.09645) (2026).

29. Vercellino, R. et al. Measurement of generative AI workload power profiles for whole-facility data center infrastructure planning. Preprint at [https://doi.org/10.48550/arXiv.2604.07345](https://doi.org/10.48550/arXiv.2604.07345) (2026).

30. Fritzsch, J., Tyloo, M. & Jacquod, P. Matrix perturbation theory of inter-area oscillations. In *2021 60th IEEE Conference on Decision and Control (CDC)*, 3507–3512 (IEEE, 2021).

31. Vanfretti, L., Bengtsson, S., Perić, V. S. & Gjerde, J. O. Spectral estimation of low-frequency oscillations in the Nordic grid using ambient synchrophasor data under the presence of forced oscillations. In *2013 IEEE Grenoble Conference*, 1–6 (IEEE, 2013).

32. Mishra, C., Vanfretti, L., Delaree, J., Purcell, T. & Jones, K. D. Understanding the inception of 14.7 Hz oscillations emerging from a data center. *Sustain. Energy Grids Netw.* **43**, 101735 (2025).

    [Article](https://doi.org/10.1016%2Fj.segan.2025.101735)  [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Understanding%20the%20inception%20of%2014.7%20Hz%20oscillations%20emerging%20from%20a%20data%20center.&journal=Sustain.%20Energy%20Grids%20Netw.&doi=10.1016%2Fj.segan.2025.101735&volume=43&publication_year=2025&author=Mishra%2CC&author=Vanfretti%2CL&author=Delaree%2CJ&author=Purcell%2CT&author=Jones%2CKD)

33. Cheng, Y. et al. *Wind Energy Systems Sub-synchronous Oscillations: Events and Modeling* Technical Report (IEEE PES, 2020); [https://doi.org/10.17023/faak-ta85](https://doi.org/10.17023/faak-ta85)

34. Ulbig, A., Borsche, T. S. & Andersson, G. Impact of low rotational inertia on power system stability and operation. *IFAC Proc.* *Vol.* **47**, 7290–7297 (2014).

    [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Impact%20of%20low%20rotational%20inertia%20on%20power%20system%20stability%20and%20operation&journal=IFAC%20Proc.&volume=47&pages=7290-7297&publication_year=2014&author=Ulbig%2CA&author=Borsche%2CTS&author=Andersson%2CG)

35. Machowski, J., Lubosny, Z., Bialek, J. W. & Bumby, J. R. *Power System Dynamics: Stability and Control* 3rd edn (Wiley, 2020).

36. Wei, F. et al. Multi-time-scale analysis on power grid frequency characteristics. *IEEE Trans. Power Syst.* **41**, 884–899 (2026).

    [Article](https://doi.org/10.1109%2FTPWRS.2025.3602044)  [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Multi-time-scale%20analysis%20on%20power%20grid%20frequency%20characteristics&journal=IEEE%20Trans.%20Power%20Syst.&doi=10.1109%2FTPWRS.2025.3602044&volume=41&pages=884-899&publication_year=2026&author=Wei%2CF)

37. Mills, D. L. Internet time synchronization: the network time protocol. *IEEE Trans. Commun.* **39**, 1482–1493 (1991).

    [Article](https://doi.org/10.1109%2F26.103043)  [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Internet%20time%20synchronization%3A%20the%20network%20time%20protocol&journal=IEEE%20Trans.%20Commun.&doi=10.1109%2F26.103043&volume=39&pages=1482-1493&publication_year=1991&author=Mills%2CDL)

38. *European Market Outlook for Battery Storage 2025–2029* (SolarPower Europe, 2025); [https://api.solarpowereurope.org/uploads/SPE_European_Battery_Outlook_2025_62b89db476.pdf](https://api.solarpowereurope.org/uploads/SPE_European_Battery_Outlook_2025_62b89db476.pdf)

39. *Ten-year Network Development Plan 2024: Opportunities for a More Efficient European Power System by 2050* (ENTSO-E, 2024); [https://eepublicdownloads.blob.core.windows.net/public-cdn-container/tyndp-documents/TYNDP2024/foropinion/Infrastructure_Gaps_Report.pdf](https://eepublicdownloads.blob.core.windows.net/public-cdn-container/tyndp-documents/TYNDP2024/foropinion/Infrastructure_Gaps_Report.pdf)

40. Kamiya, G. & Bertoldi, P. *Energy Consumption in Data Centres and Broadband Communication Networks in the EU* Report No. JRC135926 (Publications Office of the European Union, 2024); [https://doi.org/10.2760/706491](https://doi.org/10.2760/706491)

41. *Data Centres Metered Electricity Consumption 2023: Key Findings* (Central Statistics Office Ireland, 2024); [https://www.cso.ie/en/releasesandpublications/ep/p-dcmec/datacentresmeteredelectricityconsumption2023/keyfindings/](https://www.cso.ie/en/releasesandpublications/ep/p-dcmec/datacentresmeteredelectricityconsumption2023/keyfindings/)

42. *Number of Internet of Things (IoT) Connected Devices Worldwide from 2019 to 2034, by Use Case (in Millions)* (Statista, 2025); [https://www.statista.com/statistics/1194701/iot-connected-devices-use-case/](https://www.statista.com/statistics/1194701/iot-connected-devices-use-case/)

43. *Implementation of the National Electricity Amendment (Mandatory Primary Frequency Response) Rule 2020: Status as at 9 October 2020* (AEMO, 2020); [https://www.aemo.com.au/-/media/files/initiatives/primary-frequency-response/2020/pfr-implementation-report-12-oct-20.pdf](https://www.aemo.com.au/-/media/files/initiatives/primary-frequency-response/2020/pfr-implementation-report-12-oct-20.pdf)

44. *Frequency Stability in Long-term Scenarios and Relevant Requirements* (ENTSO-E, 2021); [https://eepublicdownloads.azureedge.net/clean-documents/Publications/ENTSO-Egeneralpublications/211203_Long_term_frequency_stability_scenarios_for_publication.pdf](https://eepublicdownloads.azureedge.net/clean-documents/Publications/ENTSO-Egeneralpublications/211203_Long_term_frequency_stability_scenarios_for_publication.pdf)

45. *Enhancing Cyber Resilience in Electricity Systems* (International Energy Agency, 2021); [https://www.iea.org/reports/enhancing-cyber-resilience-in-electricity-systems](https://www.iea.org/reports/enhancing-cyber-resilience-in-electricity-systems)

46. Jiménez-Ruiz, A. & Milano, F. Data centre model for transient stability analysis of power systems. *IET Gener. Transm. Distrib.* **20**, e70381 (2026).

    [Article](https://doi.org/10.1049%2Fgtd2.70381)  [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Data%20centre%20model%20for%20transient%20stability%20analysis%20of%20power%20systems&journal=IET%20Gener.%20Transm.%20Distrib.&doi=10.1049%2Fgtd2.70381&volume=20&publication_year=2026&author=Jim%C3%A9nez-Ruiz%2CA&author=Milano%2CF)

47. Chatzivasileiadis, S. et al. Micro-flexibility: challenges for power system modeling and control. *Electr. Power Syst. Res.* **216**, 109002 (2023).

    [Article](https://doi.org/10.1016%2Fj.epsr.2022.109002)  [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Micro-flexibility%3A%20challenges%20for%20power%20system%20modeling%20and%20control&journal=Electr.%20Power%20Syst.%20Res.&doi=10.1016%2Fj.epsr.2022.109002&volume=216&publication_year=2023&author=Chatzivasileiadis%2CS)

48. *PQI-DA Smart* (a-eberle, 2025); [https://www.a-eberle.de/en/products/power-quality-analyzer-pqi-da-smart/](https://www.a-eberle.de/en/products/power-quality-analyzer-pqi-da-smart/)

49. *Electromagnetic compatibility (EMC) – Part 4-30: testing and measurement techniques – power quality measurement methods* (IEC International Electrotechnical Commission, 2015).

50. *Industrial communication networks – Fieldbus specifications – Part 5-15: application layer service definition – Type 15 elements (Modbus)* (IEC International Electrotechnical Commission, 2019).

51. *Industrial communication networks – Fieldbus specifications – Part 6-15: application layer protocol specification – Type 15 elements (Modbus)* (IEC International Electrotechnical Commission, 2019).

52. Zimmer, M., Carta, D., Pesch, T. & Benigni, A. Signal recovery in power systems by correlated gaussian processes. *IEEE Open J. Ind. Electron. Soc.* **5**, 692–703 (2024).

    [Article](https://doi.org/10.1109%2FOJIES.2024.3423405)  [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Signal%20recovery%20in%20power%20systems%20by%20correlated%20gaussian%20processes&journal=IEEE%20Open%20J.%20Ind.%20Electron.%20Soc.&doi=10.1109%2FOJIES.2024.3423405&volume=5&pages=692-703&publication_year=2024&author=Zimmer%2CM&author=Carta%2CD&author=Pesch%2CT&author=Benigni%2CA)

53. Frequency data for the CE grid. *Netztransparenz* [https://www.netztransparenz.de/de-de/Regelenergie/Daten-Regelreserve/](https://www.netztransparenz.de/de-de/Regelenergie/Daten-Regelreserve/) (accessed 22 May 2026).

54. Frequency data for the GB grid. *NESO* [https://www.neso.energy/data-portal/system-frequency-data](https://www.neso.energy/data-portal/system-frequency-data) (accessed 22 May 2026).

55. Frequency data for the Nordic grid. *Fingrid* [https://data.fingrid.fi/en/datasets/339](https://data.fingrid.fi/en/datasets/339) (accessed 22 May 2026).

56. Kruse, J., Schäfer, B. & Witthaut, D. Pre-processed power grid frequency time series. *Zenodo* [https://doi.org/10.5281/zenodo.3744120](https://doi.org/10.5281/zenodo.3744120) (2021).

57. Frequency and time deviation monitoring. *AEMO* [https://www.aemo.com.au/energy-systems/electricity/national-electricity-market-nem/system-operations/ancillary-services/frequency-and-time-deviation-monitoring](https://www.aemo.com.au/energy-systems/electricity/national-electricity-market-nem/system-operations/ancillary-services/frequency-and-time-deviation-monitoring) (accessed 22 May 2026).

58. Smart grid dashboard: frequency data (2026). *EirGrid Group* [https://www.smartgriddashboard.com/all/frequency/](https://www.smartgriddashboard.com/all/frequency/) (accessed 22 May 2026).

59. Jumar, R., Maaß, H., Schäfer, B., Rydin Gorjão, L. & Hagenmeyer, V. Database of power grid frequency measurements. Preprint at [https://doi.org/10.48550/arXiv.2006.01771](https://doi.org/10.48550/arXiv.2006.01771) (2020).

60. Rydin Gorjão, L. et al. Open database analysis of scaling and spatio-temporal properties of power grid frequencies. *Nat. Commun.* **11**, 6362 (2020).

    [Article](https://doi.org/10.1038%2Fs41467-020-19732-7)  [Google Scholar](http://scholar.google.com/scholar_lookup?&title=Open%20database%20analysis%20of%20scaling%20and%20spatio-temporal%20properties%20of%20power%20grid%20frequencies&journal=Nat.%20Commun.&doi=10.1038%2Fs41467-020-19732-7&volume=11&publication_year=2020&author=Rydin%20Gorj%C3%A3o%2CL)

61. Förstner, E. et al. Experimental evaluation and systematic-error reduction of frequency estimation using the zero-crossing technique for dynamic power grids. In *2022 International Conference on Smart Grid Synchronized Measurements and Analytics (SGSMA)* (eds Nordström, L. et al.) 1–6 (IEEE, 2022).

62. Cai, B., Wang, S. & Yuan, X. Amplitude/frequency continuous modulation law of oscillation signals in power systems and demodulation method. In *2025 IEEE International Conference on Power and Integrated Energy Systems (ICPIES)*, 211–218 (IEEE, 2025).

63. MagnaGen GmbH. *Gridradar* [https://gridradar.net](https://gridradar.net) (2021).

64. Demanda eléctrica en tiempo real - Mallorca (sistema balear). *Red Eléctrica de España* [https://demanda.ree.es/visiona/baleares/mallorcaau/total](https://demanda.ree.es/visiona/baleares/mallorcaau/total) (accessed 22 May 2026).

65. De Din, E., Carta, D. & Hartmann, C. Data for ‘One-minute frequency disturbances from unintended synchronization in cyber–physical power systems’. *Zenodo* [https://doi.org/10.5281/zenodo.17366692](https://doi.org/10.5281/zenodo.17366692) (2026).

[Download references](https://citation-needed.springer.com/v2/references/10.1038/s41560-026-02144-0?format=refman&flavour=references)

## Acknowledgements

We thank F. Milano for stimulating discussions. We thank D. Gomila, P. Colet, F. Milano, N. Nemsow, F. Wald, J. Wachter, C. Yalcin, J. Lee, H. Kim, C. van Staden and S. Pütz for hosting an Electrical Data Recorder to support the collection of frequency measurements from various grids. We thank T. Veith from Gridradar for providing frequency data.

## Funding

C.H., E.D.D., D.C., J.K., U.O., R.J., B.S., T.P., A.B. and D.W. disclose support for this work from the Helmholtz Association (research programme ‘Energy System Design’). A.B. discloses support for this work from the German Federal Ministry for Research, Technology and Space (BMFTR) (grant no. 03SF0628). U.O. and B.S. disclose support for this work from the Helmholtz Association (grant no. VH-NG-1727), the Helmholtz Networking Fund (Helmholtz AI) and the National High-Performance Computing Center at KIT (computing time provided on the high-performance computer HoreKa). The National High-Performance Computing Center at KIT is jointly supported by the German Federal Ministry of Research, Technology and Space and the Ministry of Science, Research and the Arts of Baden-Württemberg, as part of the National High-Performance Computing (NHR) joint funding programme. HoreKa is partly funded by the German Research Foundation (DFG).

## Author information

### Authors and Affiliations

1. Institute of Climate and Energy Systems: Energy Systems Engineering (ICE-1), Forschungszentrum Jülich, Jülich, Germany

   Carsten Hartmann, Edoardo De Din, Daniele Carta, Johannes Kruse, Thiemo Pesch, Andrea Benigni & Dirk Witthaut

2. RWTH Aachen University, Aachen, Germany

   Carsten Hartmann & Andrea Benigni

3. Institute for Theoretical Physics, University of Cologne, Köln, Germany

   Florian Middelkoop, Johannes Kruse & Dirk Witthaut

4. TenneT TSO GmbH, Bayreuth, Germany

   Arndt Neubauer

5. Institute for Automation and Applied Informatics, Karlsruhe Institute of Technology, Eggenstein-Leopoldshafen, Germany

   Ulrich Oberhofer, Richard Jumar & Benjamin Schäfer

6. JARA-ENERGY, Aachen, Germany

   Andrea Benigni & Dirk Witthaut

Authors

1.  Carsten Hartmann
2.  Edoardo De Din
3.  Daniele Carta
4.  Florian Middelkoop
5.  Arndt Neubauer
6.  Johannes Kruse
7.  Ulrich Oberhofer
8.  Richard Jumar
9.  Benjamin Schäfer
10. Thiemo Pesch
11. Andrea Benigni
12. Dirk Witthaut

### Contributions

A.N. and D.W. conceived the research; C.H., J.K. and D.W. designed the research. U.O., R.J. and B.S. measured and pre-processed the frequency data. T.P. and D.C. organized the purchase and installation of the measurement devices used on Jülich campus. D.C. and E.D.D. set up the data acquisition and pre-processed the current and power data. C.H., U.O., B.S. and D.W. developed mechanistic and data-driven simulations. C.H. analysed the data and designed the figures with contributions from F.M., U.O. and E.D.D. C.H., E.D.D., D.C., U.O., T.P. and D.W. wrote the manuscript. A.B. acquired funding. All authors contributed to the discussion of the results and the editing of the manuscript.

### Corresponding author

Correspondence to [Dirk Witthaut](mailto:d.witthaut@fz-juelich.de).

## Ethics declarations

### Competing interests

The authors declare no competing interests.

## Peer review

### Peer review information

*Nature Energy* thanks Yingbiao Li, Andreas Ulbig and the other, anonymous, reviewer(s) for their contribution to the peer review of this work. [Peer reviewer reports](https://www.nature.com/articles/s41560-026-02144-0#MOESM2) are available.

## Additional information

**Publisher’s note** Springer Nature remains neutral with regard to jurisdictional claims in published maps and institutional affiliations.

## Rights and permissions

**Open Access** This article is licensed under a Creative Commons Attribution 4.0 International License, which permits use, sharing, adaptation, distribution and reproduction in any medium or format, as long as you give appropriate credit to the original author(s) and the source, provide a link to the Creative Commons licence, and indicate if changes were made. The images or other third party material in this article are included in the article’s Creative Commons licence, unless indicated otherwise in a credit line to the material. If material is not included in the article’s Creative Commons licence and your intended use is not permitted by statutory regulation or exceeds the permitted use, you will need to obtain permission directly from the copyright holder. To view a copy of this licence, visit [http://creativecommons.org/licenses/by/4.0/](http://creativecommons.org/licenses/by/4.0/).

[Reprints and permissions](https://s100.copyright.com/AppDispatchServlet?title=One-minute%20frequency%20disturbances%20from%20unintended%20synchronization%20in%20cyber%E2%80%93physical%20power%20systems&author=Carsten%20Hartmann%20et%20al&contentID=10.1038%2Fs41560-026-02144-0&copyright=The%20Author%28s%29&publication=2058-7546&publicationDate=2026-10-30&publisherName=SpringerNature&orderBeanReset=true&oa=CC%20BY)

## About this article

[](https://crossmark.crossref.org/dialog/?doi=10.1038/s41560-026-02144-0)

### Cite this article

Hartmann, C., Din, E.D., Carta, D. *et al.* One-minute frequency disturbances from unintended synchronization in cyber–physical power systems. *Nat Energy* (2026). https://doi.org/10.1038/s41560-026-02144-0

[Download citation](https://citation-needed.springer.com/v2/references/10.1038/s41560-026-02144-0?format=refman&flavour=citation)

- Received: 10 October 2025

- Accepted: 03 September 2026

- Published: 30 October 2026

- Version of record: 30 October 2026

- DOI: https://doi.org/10.1038/s41560-026-02144-0
