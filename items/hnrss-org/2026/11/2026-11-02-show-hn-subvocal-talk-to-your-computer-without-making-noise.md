---
title: 'Show HN: Subvocal – Talk to your computer without making noise'
link: https://news.ycombinator.com/item?id=50340088
source: hnrss-org
published: 2026-11-02T17:16:17Z
updated: 2026-11-02T17:16:17Z
first_seen: 2026-11-02T23:50:57.210604418Z
authors:
- aarushyadav
summary: 'Hi HN, we''re Aarush and Arnav from The Subvocal Company (YC F26) (https://subvocal.company/). We’re building smart glasses that let you talk to your computer without talking out loud. You can mouth words silently and discreetly and the glasses decode them to text at the speed of speech. Here''s a video of Dave from YC trying it out: https://www.youtube.com/watch?v=9QdX1eObVYI (Edit: you don''t actually have to move your lips or even open your mouth for this to work! Since the RF can see inside your mouth the system can be fine tuned on "internal speech". This isn''t the automatic tongue movements that occur when "thinking" words to yourself, but more like mouthing with your lips closed. It''s a bit weird to do, but with practice, we have been able to get it down pretty well.) Arnav and I were studying CS at Georgia Tech at the beginning of the year, with no experience in silent speech or sensing. We were both AI-native builders and power users of Wispr Flow. What bugged us was not being able to use these tools in the library (or an office) or in public. At Stanford’s TreeHacks in February, we decided to build the most technically ridiculous thing we could think of. We’d read about AlterEgo, a silent speech system out of MIT’s Media Lab, and wanted to recreate it in 36 hours. We built a crude prototype, sticking EMG electrodes onto my face: https://drive.google.com/file/d/1mwhv5HZMz3UTevdrgwS5O7g2jWV... We got it to kind of work at a letter level in a very janky way. It wasn’t good enough to win the hackathon, but convinced us to go all in. The first time I typed a sentence without anyone noticing, I felt the magic of the technology. We quickly realized EMG (reading muscle electrical signals) wouldn’t work: - Signal changes due to sweat - Electrical interference - Placement variability - Most importantly, no one wants to stick electrodes onto their face!! So we switched to RF and built the glasses you see now. They emit radio waves, similar to Bluetooth, that pass through your face. As they cross the tongue, lips and soft palate, they’re modulated by silent mouthing. We train foundation models on this data to decode these perturbations to text. Despite prior academic research, getting this to work was a huge engineering challenge. First we built an extremely low noise RF sensing stack. Then we had to place antennas so their fields overlapped maximally with the articulators. Our first attempt on glasses failed completely, so we reverted to old habits and taped antennas on our face: https://drive.google.com/file/d/1ks0dS0eZiTV9EqSFsyB_w5BBjpc... Since taping antennas consistently in the same spot and angle was hard, we created this weird but effective head rig to hold them in place: https://drive.google.com/file/d/1rhzNFpdDzr7oHk0isFY70dkoV4C... This worked, but obviously wasn’t a product yet. So we worked deep in simulations to design glasses that could hold the antennas. That evolved into our design now, with antennas embedded within the frames. Once we established there was a signal, the next hurdle was cross-user generalization. Everyone’s facial anatomy, and hence RF signal patterns, are vastly different. We tried algorithmic workarounds but realized the only lever was to massively scale pretraining data. So that’s what we did. We set up booths in our office and paid people to read prompts wearing our glasses, to pretrain our models on. This produced a beautiful scaling curve, with errors falling quickly as we scaled our dataset and cross-user performance becoming genuinely compelling (as you see in the video above; Dave only had a short 20-sentence calibration). We always heard about building things from first principles but never understood it before this. We tried tackling it top down but completely failed. Only when we broke each component (the antenna, electronics, ML) down to its fundamentals to understand and optimize it did we get it to work really well. Over the next few months we’re continuing R&D to turn our prototype glasses into the slick product on our website and ship in Q1 2027. We’re also scaling up our dataset to improve performance on edge cases. For anyone who loves voice dictation but hates using it around people, we’ve opened preorders for our first batch of 500 units, with a $200 discount off the final price. Allocation is running out quickly, so get yours at https://subvocal.co/preorder! Comments URL: https://news.ycombinator.com/item?id=50340088 Points: 478 # Comments: 263'
content: feed
html: 2026-11-02-show-hn-subvocal-talk-to-your-computer-without-making-noise.html
---

Hi HN, we're Aarush and Arnav from The Subvocal Company (YC F26) ([https://subvocal.company/](https://subvocal.company/)). We’re building smart glasses that let you talk to your computer without talking out loud. You can mouth words silently and discreetly and the glasses decode them to text at the speed of speech.

Here's a video of Dave from YC trying it out: [https://www.youtube.com/watch?v=9QdX1eObVYI](https://www.youtube.com/watch?v=9QdX1eObVYI)

(Edit: you don't actually have to move your lips or even open your mouth for this to work! Since the RF can see inside your mouth the system can be fine tuned on "internal speech". This isn't the automatic tongue movements that occur when "thinking" words to yourself, but more like mouthing with your lips closed. It's a bit weird to do, but with practice, we have been able to get it down pretty well.)

Arnav and I were studying CS at Georgia Tech at the beginning of the year, with no experience in silent speech or sensing. We were both AI-native builders and power users of Wispr Flow. What bugged us was not being able to use these tools in the library (or an office) or in public. At Stanford’s TreeHacks in February, we decided to build the most technically ridiculous thing we could think of. We’d read about AlterEgo, a silent speech system out of MIT’s Media Lab, and wanted to recreate it in 36 hours. We built a crude prototype, sticking EMG electrodes onto my face:

[https://drive.google.com/file/d/1mwhv5HZMz3UTevdrgwS5O7g2jWV...](https://drive.google.com/file/d/1mwhv5HZMz3UTevdrgwS5O7g2jWV0K9r2/view?usp=sharing)

We got it to kind of work at a letter level in a very janky way. It wasn’t good enough to win the hackathon, but convinced us to go all in. The first time I typed a sentence without anyone noticing, I felt the magic of the technology.

We quickly realized EMG (reading muscle electrical signals) wouldn’t work:

\- Signal changes due to sweat

\- Electrical interference

\- Placement variability

\- Most importantly, no one wants to stick electrodes onto their face!!

So we switched to RF and built the glasses you see now. They emit radio waves, similar to Bluetooth, that pass through your face. As they cross the tongue, lips and soft palate, they’re modulated by silent mouthing. We train foundation models on this data to decode these perturbations to text.

Despite prior academic research, getting this to work was a huge engineering challenge. First we built an extremely low noise RF sensing stack. Then we had to place antennas so their fields overlapped maximally with the articulators. Our first attempt on glasses failed completely, so we reverted to old habits and taped antennas on our face:

[https://drive.google.com/file/d/1ks0dS0eZiTV9EqSFsyB_w5BBjpc...](https://drive.google.com/file/d/1ks0dS0eZiTV9EqSFsyB_w5BBjpcBAfX-/view?usp=sharing)

Since taping antennas consistently in the same spot and angle was hard, we created this weird but effective head rig to hold them in place:

[https://drive.google.com/file/d/1rhzNFpdDzr7oHk0isFY70dkoV4C...](https://drive.google.com/file/d/1rhzNFpdDzr7oHk0isFY70dkoV4CAF1pT/view?usp=sharing)

This worked, but obviously wasn’t a product yet. So we worked deep in simulations to design glasses that could hold the antennas.

That evolved into our design now, with antennas embedded within the frames. Once we established there was a signal, the next hurdle was cross-user generalization. Everyone’s facial anatomy, and hence RF signal patterns, are vastly different. We tried algorithmic workarounds but realized the only lever was to massively scale pretraining data.

So that’s what we did. We set up booths in our office and paid people to read prompts wearing our glasses, to pretrain our models on. This produced a beautiful scaling curve, with errors falling quickly as we scaled our dataset and cross-user performance becoming genuinely compelling (as you see in the video above; Dave only had a short 20-sentence calibration).

We always heard about building things from first principles but never understood it before this. We tried tackling it top down but completely failed. Only when we broke each component (the antenna, electronics, ML) down to its fundamentals to understand and optimize it did we get it to work really well.

Over the next few months we’re continuing R&D to turn our prototype glasses into the slick product on our website and ship in Q1 2027. We’re also scaling up our dataset to improve performance on edge cases.

For anyone who loves voice dictation but hates using it around people, we’ve opened preorders for our first batch of 500 units, with a $200 discount off the final price. Allocation is running out quickly, so get yours at [https://subvocal.co/preorder](https://subvocal.co/preorder)!

* * *

Comments URL: [https://news.ycombinator.com/item?id=50340088](https://news.ycombinator.com/item?id=50340088)

Points: 478

\# Comments: 263
