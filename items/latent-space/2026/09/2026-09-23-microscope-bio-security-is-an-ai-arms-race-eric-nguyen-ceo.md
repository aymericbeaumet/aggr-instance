---
title: 🔬Bio-security is an AI Arms Race - Eric Nguyen (CEO, Radical Numerics)
link: https://www.latent.space/p/bio-security-is-an-ai-arms-race-eric
source: latent-space
published: 2026-09-23T13:27:18Z
updated: 2026-09-23T13:27:18Z
first_seen: 2026-09-27T19:29:15.293681927Z
authors:
- RJ Honicky
summary: Radical Numerics is using biological chain-of-thought and multimodal perception to keep up with the bio-defense arms race, design new genomes and gain insights into biology itself.
content: extracted
html: 2026-09-23-microscope-bio-security-is-an-ai-arms-race-eric-nguyen-ceo.html
preview:
  file: 2026-09-23-microscope-bio-security-is-an-ai-arms-race-eric-nguyen-ceo.preview-5fa7966ca27c.webp
  width: 256
  height: 128
  color: '#8f6057'
images:
- source: https://substackcdn.com/image/fetch/$s_!sMML!,w_1200,h_600,c_fill,f_jpg,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-video.s3.amazonaws.com%2Fvideo_upload%2Fpost%2F216723291%2Fc5daecec-8262-4632-b94a-ac5fcbb74077%2Ftranscoded-1790120123.png
  original:
    file: 2026-09-23-microscope-bio-security-is-an-ai-arms-race-eric-nguyen-ceo.image-c7d29547a5f4.jpg
    width: 1200
    height: 600
  color: '#080706'
- source: https://substackcdn.com/image/fetch/$s_!2q_D!,w_120,h_120,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fpbs.substack.com%2Fprofile_images%2F1100512198139498497%2FutHSJ4st.png
  original:
    file: 2026-09-23-microscope-bio-security-is-an-ai-arms-race-eric-nguyen-ceo.image-eeaab6cbb319.jpg
    width: 120
    height: 120
  color: '#a69a8a'
- source: https://substackcdn.com/image/fetch/$s_!d1Xz!,w_60,h_60,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fpbs.substack.com%2Fprofile_images%2F1945829182715432960%2FBolQx8R0.jpg
  original:
    file: 2026-09-23-microscope-bio-security-is-an-ai-arms-race-eric-nguyen-ceo.image-130529adf0f2.jpg
    width: 60
    height: 60
  color: '#484855'
- source: https://substackcdn.com/image/fetch/$s_!_dqm!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F051507bc-ebaf-43e9-9b9c-0037b1ee45e9_1392x700.png
  original:
    file: 2026-09-23-microscope-bio-security-is-an-ai-arms-race-eric-nguyen-ceo.image-6343de52cf29.jpg
    width: 1392
    height: 700
  color: '#fdfdfd'
- source: https://i.ytimg.com/vi/B7DdNj_VjcU/hqdefault.jpg
  original:
    file: 2026-09-23-microscope-bio-security-is-an-ai-arms-race-eric-nguyen-ceo.image-ef5db4260066.jpg
    width: 480
    height: 360
  color: '#020202'
extra:
  audio_type: audio/mpeg
  audio_url: https://api.substack.com/feed/podcast/216723291/8511fc2825689ad610a1ca70864be49f.mp3
  duration_seconds: 5519
---

The OpenAI → Hugging Face attack has people asking “what else do we need to worry about?” and Anthropic’s filters flag two things: cyber-security and biology. The natural question is: what about bio-security, then?

Clem Delangue argues that cyber-warfare defensive capabilities need to be open and to keep pace with frontier models’ attack capabilities

> clem 🤗 @ClementDelangue
>
> So proud of our security team! They caught, contained & publicly disclosed an attack unlike anything we've seen before, and did it at record speed. Also massively grateful to @Zai\_org: they shared GLM5.2 as open weights (for free!) with the world and it became a key part of our…
>
> > Adrien Carreira @XciD\_
> >
> > Hardest IR of my career: one narrow objective, endless parallel paths, machine speed. One takeaway, we fought back with open models, in the open. AI security won’t be solved by one company in secret. Open source puts these tools in every defender’s hands
>
> [@ClementDelangue on X](https://x.com/ClementDelangue/status/2079913058554585089)

Radical Numerics co-founder Eric Nguyen sat down with us and explained why the same models that increase biological capability can also keep defense from falling behind.

While he was at Stanford, Eric couldn’t get traction on Genomic Language Models (GLMs) for a long time. Biologists didn’t believe it would work, didn’t think they could verify the output, and didn’t see important applications beyond what they could already do. He kept pushing, eventually helping lead the development of Evo and contributing to Evo 2 at Arc Institute. Those models were later used by a separate Arc/Stanford team to [generate entire bacteriophage genomes that were synthesized into functional viruses](https://www.biorxiv.org/content/10.1101/2025.09.12.675911v1)!

[![](https://substackcdn.com/image/fetch/$s_!_dqm!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F051507bc-ebaf-43e9-9b9c-0037b1ee45e9_1392x700.png "")](https://substackcdn.com/image/fetch/$s_!_dqm!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F051507bc-ebaf-43e9-9b9c-0037b1ee45e9_1392x700.png)

Early ChatGPT spit out poems and email, and early DNA language models like Evo and Evo-2 could build a genome from scratch. DNA is different, however, from natural language in that it has a very small alphabet (4 characters ACTG) and that its sequences are very long:

- 60K for an average human gene

- long being up to 2.3M

- the whole human genome around 3B.

[Innovation in long-context models](https://github.com/togethercomputer/stripedhyena) made this possible about 3 years ago (footnote: striped hyena), long before the frontier labs were building 1M+ context models.

Now Eric and other AI x Bio luminaries [1](https://www.latent.space/p/bio-security-is-an-ai-arms-race-eric#footnote-1) have founded Radical Numerics to build and scale GLMs to tack a wide range of biological problems, extending well beyond generating DNA.

Their GLMs already do pretty well with RNA and protein because there are clear markers in the DNA sequence for genes (RNA sequences the perform many functions) and specific genes that encode proteins. This means that the models already generalize to multiple “languages,” before even attempting to train in other modalities, such as 3d protein structure, epigenetics and natural language.

If a model thinks in the DNA language, maybe it understands the imprint that environment left on different genomes as well? Perhaps the model has learned the functional relationship between different sequences, and could extrapolate to new sequences based on that?

> And so what we wanted to showcase was that if we show the model progressively better RNAs in a series of steps with its score, right? So you have like low scores first and then you gradually move up the chain. Can the model continue that trajectory on its own? And then in the final step, does it self optimize to a point where it's like the best score it can get? That was the experiment. Can we do that? And so we took a data set, a large data set of aptamers. We held out a portion of the best performing ones and we showed it only the lower ones, but then we ranked it, right? So we showcase lower scores with the RNA aptamers and then progressively got higher, and then ask the model to just like continue with that pattern. And it turns out it was able to recapitulate some of those higher scores that we had not shown it yet.

So, voila: chain-of-thought, thinking in DNA!

But much as long-context inference, chain-of-though and multi-modal perception unlocked sophisticated reasoning in natural language LLMs, these capabilities in GLMs are enabling increasingly sophisticated “biological intelligence,” and along with it, greater danger.

According to Eric, defense is currently losing this battle, but Radical Numerics argues to push the frontier harder!

I won’t spoil the details for you. In the episode we talk in detail about:

- Biosecurity as an arms race — and how defense can keep up

- The genome as the imprint of the environment on DNA

- Going truly multi-modal

- How chain-of-though works when you “think” in the language of DNA

[www.youtube.com](https://www.youtube.com/watch?v=B7DdNj_VjcU)

[1](https://www.latent.space/p/bio-security-is-an-ai-arms-race-eric#footnote-anchor-1)

**Eric Nguyen**: Co-founder and CEO, holding a PhD in Bioengineering & AI from Stanford University. He previously helped develop large-scale genome language models like Evo and Evo 2\
**Michael Poli**: Chief AI Scientist, holding a Stanford PhD and a former founding scientist at Liquid AI.\
**Stefano Massaroli**: President, a former postdoc with Yoshua Bengio and a founding team member at Liquid AI.\
**Armin W. Thomas**: CTO, a former Stanford postdoc who worked with Chris Ré and was previously at Liquid AI.
