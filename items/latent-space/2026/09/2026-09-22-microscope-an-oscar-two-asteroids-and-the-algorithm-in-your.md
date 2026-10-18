---
title: '🔬 An Oscar, Two Asteroids, and the Algorithm in Your sklearn: John Platt on AI for Science'
link: https://www.latent.space/p/john-platt
source: latent-space
published: 2026-09-22T21:07:39Z
updated: 2026-09-22T21:07:39Z
first_seen: 2026-09-27T19:29:15.293681927Z
authors:
- Brandon Anderson
summary: We talked to Google’s Oscar winning “Giganerd” about automating science, solving climate change, and how future generations can contribute to science in the age of superintelligent AI
content: extracted
html: 2026-09-22-microscope-an-oscar-two-asteroids-and-the-algorithm-in-your.html
preview:
  file: 2026-09-22-microscope-an-oscar-two-asteroids-and-the-algorithm-in-your.preview-e5f680fdbb93.webp
  width: 256
  height: 128
  color: '#a38788'
images:
- source: https://substackcdn.com/image/fetch/$s_!rySF!,w_1200,h_600,c_fill,f_jpg,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-video.s3.amazonaws.com%2Fvideo_upload%2Fpost%2F216845528%2Fe4cee8d3-c70c-481f-9d08-d2abac353516%2Ftranscoded-1790058174.png
  original:
    file: 2026-09-22-microscope-an-oscar-two-asteroids-and-the-algorithm-in-your.image-00e7bc1017e2.jpg
    width: 1200
    height: 600
  color: '#77869a'
extra:
  audio_type: audio/mpeg
  audio_url: https://api.substack.com/feed/podcast/216845528/5d04ab99938b8bf3aee2b0e2099bfa53.mp3
  duration_seconds: 7286
---

How often do you get to talk to a guest who has both an Academy Award and who invented textbook machine learning algorithms? John Platt has an [Oscar](https://www.atogt.com/askoscar/display-person.php?id=78091&var=0), two [textbook](https://en.wikipedia.org/wiki/Platt_scaling) [algorithms](https://en.wikipedia.org/wiki/Sequential_minimal_optimization), two named asteroids, and an [Erdos-Bacon number](https://en.wikipedia.org/wiki/Erd%C5%91s%E2%80%93Bacon_number) of 6. This was easily the most fun bio of all the guests we’ve read to date. And the result was an epic and fun chat covering Google’s [Empirical Research Assistance](https://research.google/blog/empirical-research-assistance-era-from-nature-publication-to-catalyzing-computational-discovery/) (ERA), how AI can help battle climate change, and tons of great stories about the co-evolution of science and AI.

John’s colleague Dave Bacon likes to tease John that his career has been defined by being twenty years early to the next big thing. This may be convolutional neural networks (some credit him with coining the term), fusion research, quantum computing. John and Google have been working on solving some of humanity’s hardest problems with AI and computation for well over a decade now. Recently John and his team set their sights on using AI to solve any scientific problem that can be written down as a score.

John’s team has taken on many hard scientific problems over the years. In solving these, they noticed a pattern, many scientific problems can be reduced to what John calls a “scoreable task”. Once you have the score function, the goal is to find some code that maximizes the score. The hard part is in formulating the score, but once you have the score finding the maximizer can still be quite a lot of effort.

John’s team set out to automate solutions to this general problem. This came out of the idea of an “auto-Kaggle” AI, which can solve any Kaggle problem you can throw at it. Kaggle is owned by Google, so all the data was ready and easily available to them!

The result is Google’s Empirical Research Assistance or ERA ( [paper](https://www.nature.com/articles/s41586-026-10658-6), [github](https://github.com/google-research/era/tree/main/era_applications), [blog](https://research.google/blog/accelerating-scientific-discovery-with-ai-powered-empirical-software/)). [1](https://www.latent.space/p/john-platt#footnote-1) ERA is surprisingly simple conceptually. Gemini (or your LLM of choice) keeps a running tree of past experiments (notebooks) and where they’re going. It’s a close cousin of [Monte Carlo Tree Search](https://en.wikipedia.org/wiki/Monte_Carlo_tree_search): at each iteration the [Upper Confidence Bound](https://en.wikipedia.org/wiki/Multi-armed_bandit#Upper_Confidence_Bound_\(UCB\)_bandit_algorithm) rule picks which notebooks are most promising to mutate. This is optimistic, not greedy, so sometimes even the fifth-best notebook gets chosen. Gemini then proposes mutations for each one, about ten at a time. The history of each branch is shared, so different leaves can learn from each other.

> “It’s almost like having a hyper-eager grad student who doesn’t sleep.”

[Evolutionary algorithms](https://en.wikipedia.org/wiki/Genetic_programming) have been around since the 70s, but this works because Gemini actually knows where to look! What’s even more interesting is that there was a step change between Gemini 2.0 and 2.5, and this went from just not working to working great.

ERA is so powerful that John and his team solved many outstanding problems with it, resulting in at least [ten papers](https://github.com/google-research/era/tree/main/era_applications/pdfs). Some of these were climate change related, which we talk about in the next section.

So, we had to ask: if you have an optimization god how do you avoid fooling yourself? John’s answer is that ERA provides predictive models. It’s up to the scientist to make sure they’re truly descriptive. Some of this just involves good old-fashioned careful machine learning science. “It’s a power tool. It can slice your fingers off.” This led to some fun discussion about Kaggle competitions, and the fun ways people can overfit to datasets without meaningfully solving the problem you actually care about: Google’s contrail-detection competition was won by entrants who noticed a half-pixel error in the labels (is the origin at the corner of the pixel or the center?) and this [turned out to be a part of the winning special sauce](https://www.kaggle.com/competitions/google-research-identify-contrails-reduce-global-warming/writeups/jun-koda-1st-place-solution). Great for winning $15,000, not so helpful if you actually want to solve contrails.

> “People themselves will act like these LLMs and try to reward hack. It goes back to [Goodhart’s law](https://en.wikipedia.org/wiki/Goodhart%27s_law): any metric that becomes a target is no longer good as a metric.”

His advice for where to start instead?

> “Always just fit linear regression. Just do it. Just do it. Just do it. Or SVM.”

John and his team have worked extensively to mitigate the effects of climate change. We talked about several of their initiatives.

Perhaps the most interesting result we talked about was reducing the effects of condensation trails (contrails) from airplanes. Those little streaks you see running behind planes somehow account for [1% of all human-induced global warming](https://doi.org/10.1016/j.atmosenv.2020.117834)?!? Some of these trails of ice crystals can hang out for days. These crystals are black in the infrared, acting like a thermal blanket that traps heat day and night.

It’s easy to understand what’s happening here, a region of atmosphere becomes “ice supersaturated”, [2](https://www.latent.space/p/john-platt#footnote-2) and a tiny bit of exhaust seeds water vapor that instantly crystallizes. The scale here is astounding, with a single gram of exhaust resulting in ten kilograms of ice crystals.

The solution to all of this is quite simple, in principle! We know what parts of the atmosphere are most likely for the trails to form. Just have the planes drop a flight level or two. Problem solved, right? Well, the hard part is accounting for how much warming was prevented. This is a counterfactual problem, parts of which stumped John’s team for over two years. They had a working model for [the heat-trapping half](https://doi.org/10.5194/amt-19-1951-2026), but not for the reflected sunlight. ERA was able to find a simple model with some confounders they hadn’t considered. Cracked it!

Modeling climate generally is a hard problem. Climate is best thought of an attractor of many different possible weather outcomes. [3](https://www.latent.space/p/john-platt#footnote-3) This makes it much harder to model.

> “Weather is where you are on the [attractor](https://en.wikipedia.org/wiki/Lorenz_system), and climate is the statistics of the attractor. The problem with climate is that we’re altering it. The attractor itself is changing, it’s moving.”

John and his team have worked on treating both the symptoms and the disease of climate change, with several other works in the area. Another fun example we briefly cover is FireSat, a way of using a constellation of satellites to rapidly identify fires before they grow too big to put out. For anyone living in California, you understand the problem. In dry years a small fire can result in hundreds of thousands of acres. If you could find this fire when it’s the size of a room, it could be put out. By the time it hits an acre we have a much harder problem.

By now it should be clear John has an incredible and unique view over the intersection of science, computation, and AI. John talked about a class on [physics of computation](https://en.wikipedia.org/wiki/Feynman_Lectures_on_Computation) [4](https://www.latent.space/p/john-platt#footnote-4) he took with Richard Feynman back in 1982. This was when quantum computing was an ill-defined concept with no theory or experimental backing. John recalls every Tuesday was a guest lecture, and every Thursday was Feynman explaining why the Tuesday guest was wrong. John also recalls doing science back when there was essentially no compute, a million operations per second was cutting edge.

What is John’s recommendation: the most important skill is developing deep domain expertise. There’s no other way to develop taste than to tackle hard problems. One surprising part of this is that John recommends spending time doing things the old fashioned way. Play with tools, and just implement things yourself.

> “You could drive up the mountain, or you could hike up the mountain, and maybe it’s okay, even fun, to occasionally hike.”

Summing it up, John’s message to the audience is that there will still be a place for scientists, and that if anything it will just open up more opportunities for “the creative stuff, the rigorous stuff, the philosophy stuff.” But don’t forget to spend time doing the grunt work.

> “There just seems to be this strong impetus in the world to optimize and squeeze everything out. But you do lose something when you hyper-optimize. It’s overfit.”

And whatever tools you end up using, John’s advice is the same one [Feynman gave him](https://calteches.library.caltech.edu/51/2/CargoCult.htm) forty years ago: you must not fool yourself, and you are the easiest person to fool.

We had a great time talking with John. We hope you enjoy!

[www.youtube.com](https://www.youtube.com/watch?v=2xBSGluFkG0)

- Fusion is three years away, not thirty, if you ask John. And why the [Lawson criterion](https://en.wikipedia.org/wiki/Lawson_criterion) means every fusion approach has an Achilles heel.

- Why superconducting qubits are still finicky.

- The asteroid he named after his mom, which turned out to have a moon.

- The looming [helium shortage](https://en.wikipedia.org/wiki/Federal_Helium_Reserve) nobody talks about.

- How NeurIPS started as people crashing a private workshop at Snowbird, and why [Hopfield networks are all you need](https://arxiv.org/abs/2008.02217).

- Being Carver Mead’s sysadmin on a VAX with an 80 MB disk the size of a dishwasher.

- Finding asteroids in 1985 with film, a stereoscope, and a letter to Brian Marsden. The [Vera Rubin Observatory](https://en.wikipedia.org/wiki/Vera_C._Rubin_Observatory) found 11,000 in six weeks.

- The Feynman effect: total clarity in the room, none once you leave.

- Quantum echoes, the [NISQ era](https://en.wikipedia.org/wiki/Noisy_intermediate-scale_quantum_era), and why he thinks quantum is neither thirty years away nor tomorrow.

- A startup that wants to inject mercury into a fusion reactor and sell the transmuted gold. “It might not work.”

- John’s 20% time rule for his own group: do stuff for learning, and you don’t even have to tell him what.

[1](https://www.latent.space/p/john-platt#footnote-anchor-1)

The ERA [GitHub repo](https://github.com/google-research/era/tree/main/era_applications) features an open source implementation that ran Gemini but can be used with any LLM. ERA is not currently available as a Google product.

[2](https://www.latent.space/p/john-platt#footnote-anchor-2)

“Ice-supersaturated” is about water vapor, not liquid water. Cold air can hold a given amount of vapor, and there are two different limits: the amount in equilibrium with liquid water, and the smaller amount in equilibrium with ice. Below freezing, a pocket of air can sit between those two limits. It has more vapor than ice can tolerate, but not enough to condense into droplets, and ice won’t form directly from vapor without a seed. So the vapor just hangs there, metastable, sometimes for days, until something seeds it.

[3](https://www.latent.space/p/john-platt#footnote-anchor-3)

We recently covered the weather-climate crossover in our episode with [Anima Anandkumar](https://www.latent.space/p/anima), and we plan on covering both weather and climate more in future episodes.

[4](https://www.latent.space/p/john-platt#footnote-anchor-4)

This was really about quantum computing, but in the early days before anyone really knew what this meant and it was just a vague idea Feynman and a few others were kicking around.
