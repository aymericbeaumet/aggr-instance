---
title: Searching for distant planets with Astra
link: https://openai.com/index/searching-for-distant-planets-with-astra
source: openai-com
published: 2026-11-24T11:00:00Z
updated: 2026-11-24T11:00:00Z
first_seen: 2026-11-25T01:32:55.949141972Z
labels:
- science
summary: See how two amateur astronomers used Codex and Astra to find potential exoplanets in public data from NASA’s Kepler and TESS space telescopes.
content: extracted
html: 2026-11-24-searching-for-distant-planets-with-astra.html
remote_preview:
  url: https://images.ctfassets.net/kftzwdyauwt9/318YLaAKlMKQFzOdMutjeX/8b6e4e516fd1a7894c97318838f43a0f/searching-for-distant-planets-with-astra-social.png?w=1600&h=900&fit=fill
  alt: Searching for distant planets with Astra, in white type over a black star field.
---

Astronomers have confirmed more than [6,400 planets beyond our solar system](https://exoplanetarchive.ipac.caltech.edu/) since the [first ones were announced in 1992](https://www.nature.com/articles/355145a0). A worldwide community of amateur astronomers has long been part of that search. Some comb through public telescope data for small, repeated dips in a star’s brightness that could signal an orbiting planet. Others use their own telescopes to watch for the next dip and share what they see with researchers.

Astronomers have used AI in this search for years. In 2017, researchers trained a neural network on 15,000 signals from NASA’s [Kepler Space Telescope](https://science.nasa.gov/mission/kepler/) that astronomers had already examined. From those examples, the model learned to recognize which patterns in starlight were most likely to come from a planet. When the researchers used it to examine weaker signals, one led them to an [eighth planet orbiting a star called Kepler-90](https://www.nasa.gov/news-release/artificial-intelligence-nasa-data-used-to-discover-eighth-planet-circling-distant-star/).

Now, tools like Astra are helping amateur astronomers design and run searches of their own, often on data that has already yielded the more obvious exoplanet signals. Kevin Bond, an engineer at OpenAI, came to astronomy through his love of photographing the night sky. He used Codex to search observations of about 100,000 stars from the Kepler telescope, a planet-hunting mission that ran from 2009 to 2018. Ethan Byrd, founder of the email infrastructure start-up Primitive, has always loved astronomy. Working independently of Bond, he tried several AI models before choosing Astra to help him analyze telescope data from about 19 million stars.

Both searches identified potential exoplanets. Bond and Byrd have now turned to astronomers for an independent assessment of their results. Bond contacted an astronomer at the SETI Institute, who ran an independent search and found two of the same signals in the Kepler data. Byrd sent one of his strongest candidates to researchers working on NASA’s Transiting Exoplanet Survey Satellite (TESS) mission. It’s in line for further analysis, and two more candidates are awaiting submission.

One of the most common ways to find an exoplanet is to watch the star it orbits. When a planet passes in front of its star from our point of view on Earth, it blocks a tiny amount of starlight. Astronomers call that crossing a transit. On a graph of the star’s brightness over time, known as a light curve, a transit shows up as a dip. If the dip keeps coming back at regular intervals, astronomers know when to watch for it again.

Searching for that pattern across thousands of stars is partly a coding problem. Astronomers have published methods for picking out faint, recurring dips in years of measurements. Bond used Codex to compare those methods, combine them in a search program, and test whether changes to the code improved the search. One AI agent scanned the light curves for promising signals. Another ran more detailed checks on the candidates the first one flagged.

![A photographer works at a laptop beside a tripod-mounted camera, lit by the screen and the city lights behind him.](https://images.ctfassets.net/kftzwdyauwt9/6710Vp9hZFgzFm8CHOCLrt/20f95c70c1d86c050770e61c0fc02f3d/exoplanets-working-at-laptop-square.jpg?w=3840&q=90&fm=webp)

![Hands at a laptop keyboard in the dark, with a star field visible on the screen.](https://images.ctfassets.net/kftzwdyauwt9/3He6T0iGapx53CIbUOaOxC/55df549454b5e85d7162d9c6f9b2bfbd/exoplanets-laptop-in-the-field.jpg?w=3840&q=90&fm=webp)

Bond ran the program on public data from the Kepler Space Telescope. The search flagged more than 200 signals, including 10 from previously confirmed planets. Finding those known planets gave Bond confidence that the program could pick out the kinds of signals he was looking for.

Two stars stood out. The first, KIC 5689121, is a relatively cool star about half the size of our sun in the constellation Lyra. The Codex search found a dip in brightness every 13.8 days. If a planet is causing it, the amount of light it blocks suggests it could be about 2.4 times Earth’s diameter.

The second star, KIC 6691030, is faint and lies in the northern sky. The Codex search found a dip in its brightness every 4.8 days, consistent with a planet smaller than Neptune. These were the two signals the SETI Institute astronomer found independently. Confirming what caused them could take months or years.

> “Kevin’s work shows how citizen scientists, equipped with AI, can help astronomers explore the Kepler Bonus Catalog—a vast collection of stars beyond Kepler’s original targets. His pipeline has surfaced promising candidates in data traditional searches may have missed. Our initial checks are encouraging, and follow-up observations will tell us more.”

Vishal Gajjar, associate director for international SETI programs at the SETI Institute

Byrd used Astra to design two searches. One looked broadly for exoplanet candidates. The other focused on stars’ habitable zones. A habitable zone is the range of distances from a star where a planet with a suitable atmosphere could support liquid water on its surface.

Byrd then asked Astra to review published methods, build the search software, and run initial checks on promising signals. His searches drew on observations from TESS, which NASA launched in 2018, and from other public telescope archives.

The first pass through the data turned up 49 candidates. Byrd sent one of his strongest candidates to researchers working on the TESS mission, where it is awaiting further analysis. His search is still running.

When astronomers see a star dim, they have to consider what else might explain it. Two stars passing in front of one another can produce a similar dip. Even a change in temperature aboard a telescope can affect its instruments enough to make a star appear to dim.

Astronomers can use statistics to show that an exoplanet is the most likely explanation for a series of dips. They can also collect more observations, sometimes using a different method. For example, an orbiting planet’s gravity tugs on its star, causing it to move slightly toward and away from us. That motion, called radial velocity, shows up as tiny shifts in the color of the star’s light. Astronomers then publish their findings in a peer-reviewed journal so others can examine the evidence.

For Bond and Byrd, each candidate is a question that can be tested. If a planet is passing in front of the star in Lyra every 13.8 days, astronomers can watch for the next crossing. Further observations can help establish whether the dip repeats as expected, while additional checks can help determine what is causing it.

![The Milky Way stretches diagonally across a star-filled sky above a silhouetted mountain ridge.](https://images.ctfassets.net/kftzwdyauwt9/2SSdNPM9DjCXsOEbYcsh8B/55dabb5bdee994566b1abc42f343519f/kevin-bond-milky-way-over-ridgeline.jpg?w=3840&q=90&fm=webp)

Photographs by Kevin Bond

![A copper-colored Moon with a pale rim hangs against a nearly black sky dotted with faint stars.](https://images.ctfassets.net/kftzwdyauwt9/3J85rc2RVvJoYQqv3OKoGo/743ec2f901fb39d6a9ddbf2eb2836374/kevin-bond-copper-moon-square.jpg?w=3840&q=90&fm=webp)

You can contribute without owning a telescope. NASA’s [Exoplanet Watch](https://science.nasa.gov/citizen-science/exoplanet-watch/how-to-contribute/) lets you request data and learn to plot a light curve yourself. Its broader [exoplanet citizen science programs](https://science.nasa.gov/exoplanets/citizen-science/) offer other ways to participate.

If you have access to a telescope, Exoplanet Watch walks users through how to collect data on transiting exoplanets and participate in observation campaigns. [AAVSO’s Exoplanet Observing Program](https://aavso.org/exoplanet-photometry/) can show you how to measure a dip in starlight and share your observations with the researchers studying that planet. And the [Unistellar Network Investigating TESS Exoplanets](https://science.unistellar.com/exoplanets/unite/) coordinates volunteers around the world with a certain type of telescope to observe the same star, helping astronomers catch transits that can last longer than a single night.

Astronomy has a long history of people around the world following their curiosity and sharing what they find. Bond and Byrd are part of that tradition. Their searches are still open, but they turned a love of the night sky into leads that other astronomers can investigate. We want more people to have that opportunity, in astronomy and across science.

If you want to follow along with new findings, practical guidance, and perspectives from researchers using AI to advance science [check out the OpenAI science blog](https://openai.com/news/science/).
