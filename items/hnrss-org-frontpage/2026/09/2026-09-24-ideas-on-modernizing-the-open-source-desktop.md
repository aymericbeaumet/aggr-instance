---
title: Ideas on modernizing the open-source desktop
link: https://lwn.net/SubscriberLink/1095425/2d9f411252325784/
source: hnrss-org-frontpage
published: 2026-09-24T02:52:09Z
updated: 2026-09-24T02:52:09Z
first_seen: 2026-09-24T11:38:28.368851050Z
authors:
- signa11
summary: 'Article URL: https://lwn.net/SubscriberLink/1095425/2d9f411252325784/ Comments URL: https://news.ycombinator.com/item?id=49825642 Points: 163 # Comments: 167'
content: extracted
html: 2026-09-24-ideas-on-modernizing-the-open-source-desktop.html
preview:
  file: 2026-09-24-ideas-on-modernizing-the-open-source-desktop.preview-a34bdd5b6f35.webp
  width: 192
  height: 256
  alt: '[Scott Jenson speaking]'
  color: '#655c55'
images:
- source: https://static.lwn.net/images/2026/scott-jenson-sm.png
  original:
    file: 2026-09-24-ideas-on-modernizing-the-open-source-desktop.image-8d0a48304dc8.png
    width: 300
    height: 400
  variants:
  - file: 2026-09-24-ideas-on-modernizing-the-open-source-desktop.image-f3a4ba55bde1.webp
    width: 300
    height: 400
  color: '#a39c93'
- source: https://static.lwn.net/images/2026/wm-demo-sm.png
  original:
    file: 2026-09-24-ideas-on-modernizing-the-open-source-desktop.image-940f04800b45.png
    width: 800
    height: 450
  variants:
  - file: 2026-09-24-ideas-on-modernizing-the-open-source-desktop.image-b112bd3e7d1d.webp
    width: 320
    height: 180
  - file: 2026-09-24-ideas-on-modernizing-the-open-source-desktop.image-4ce10eeb1282.webp
    width: 640
    height: 360
  - file: 2026-09-24-ideas-on-modernizing-the-open-source-desktop.image-b3464289af16.webp
    width: 800
    height: 450
  color: '#000000'
---

Scott Jenson has been working on user interfaces (UIs) and user experience (UX) for many years at Apple, Google, and other companies. Now, he's trying to convince open-source projects to experiment more and drive the desktop beyond the age-old "[windows, icons, menus, pointer](https://en.wikipedia.org/wiki/WIMP_\(computing\))" (WIMP) model. At [Akademy 2026](https://akademy.kde.org/2026/), KDE's annual developer conference, he shared his complaints and ideas in a talk aimed at convincing those in attendance to take the lead on desktop design.

He introduced himself as someone who has been doing UX for a long time, much of that time at Google. He said that over that time he had noticed a cultural shift, "because back in 2005 when I joined, we weren't evil; we were really trying to do the right thing". He had worked on the Chrome team for a while, which he said was "filled with people who wanted nothing but open-source, open-web" projects. Things had changed radically, which led him to leave Google, "but I'm trying to atone for my sins working for these companies".

> This LWN.net subscription-only content has been made available to you by an LWN subscriber. To see more of this content, please take advantage of the following special offer.
>
> ### Free trial subscription
>
> Try LWN for free for 1 month: no payment or credit card required. [Activate your trial subscription now](https://lwn.net/Promo/slink-trial-middle/claim) and see why thousands of readers subscribe to LWN.net.

He now does work for Mastodon and Home Assistant, "just kind of trying to squeeze a little bit more good UX design into open source". Jenson said he did not want to be confrontational, but he thought that open source needs a lot of help in the area of UX design. That is challenging, because open-source projects are understaffed.

#### Innovation

He was at Akademy to discuss his ideas because he had given a talk similar to this at Ubuntu's conference last year, which proved to be popular. "It got more than half a million views on YouTube—which by YouTube standards is trivial, but by Scott standards is enormous." That told him that the topic of desktop UX was something that people wanted to talk about. This time around, he also wanted to show a few examples of prototypes he had to demonstrate his ideas.

[![[Scott Jenson speaking]](https://static.lwn.net/images/2026/scott-jenson-sm.png "Scott Jenson speaking")](https://lwn.net/Articles/1095521/#scott)

He thinks that desktop UX is very different from other kinds of UX. Jenson related a story about working on the Macintosh's Finder application, which is a file manager. The team "wanted to do ellipses" in file names when showing a file in the Finder that had a name too long to display in its entirety. The developers wanted to put the ellipsis at the end of the file name, but he thought that would lose too much information, and suggested that the ellipses should be in the middle of the file names. "It's now considered to be one of those things that's an exciting little attention-to-detail issue the Macintosh does."

Another UX detail that he worked on is the way that clicking and dragging files from one window to another works on the Mac. A mouse click action has two parts: when the user depresses the button (mouse down) and when the user releases the button (mouse up). On the Mac, item selection happens on mouse down, but the window only raises on the mouse up action. In other words, if a user drags an icon from a Finder window into another window, the first window is not raised.

On most Linux systems, though, if a user is trying to click and drag a file from a window in the background it will cause the window to raise as soon as the user does the "mouse down" action. That complicates trying to click and drag a file from one window to another. That is a fundamental action that means that users can't move data into applications quite as easily on Linux as they can on the Mac.

He said that he had talked about that issue two years ago on Mastodon, which raised a response from a KDE developer who thought it was a good idea to separate the mouse down and mouse up actions when dragging data between windows. Then they implemented it a few hours later.

"I forgot who that was. So if anybody here did that", he said, but before he could finish the sentence a voice from somewhere in the room responded, "you're welcome". Jenson yelled "thank you! I've been wanting to meet you for two years!" That was an example, he said, of how open source can be awesome: someone can just say "that's a good idea, I'll just do it" and then rapidly implement and push the feature to the project.

#### Stabilized

The point he was really trying to make, though, is that the desktop UX had not really changed in 20 years. There were plenty of new ideas and improvements in the 1980s and 1990s, "and then it just stabilized, and not much has really changed". But people are doing much more with their computers today, and he thinks that there are ways for desktop UX to grow.

Much of the UX for Linux systems was originally copied from Windows and the Mac, "KDE even says it's for Windows users, that it wants to be similar to Windows for users". There's nothing wrong with that, but he felt that the Linux community "in general" had been leaning on Microsoft and Apple to do all of the UX research and testing, "and frankly, \[make\] the mistakes".

Some might point out that GNOME has [conducted user research](https://blogs.gnome.org/shell-dev/2021/02/15/shell-ux-changes-the-research/) for some of its design changes, and KDE's [human-interface guidelines (HIG)](https://develop.kde.org/hig/) were [also developed based on research](https://develop.kde.org/hig/) by its contributors. However, it would be fair to say that these are exceptions rather than the norm; certainly, no one is funding or conducting regular UX design research and development for the Linux desktop overall.

He said that it is smart to let Apple and Microsoft do these things first "so we can draft behind them", but the problem is that "they've just kind of given up to an extent". Apple has been letting its desktop UX, and desktop hardware, "languish for years and years and years" because it had shifted its focus to the iPhone and iPad devices. "I mean, like, Apple wanted the Mac to die. They wanted the world to move over to the iPad. Guess what? It didn't."

Apple has since turned its attention back to the Mac, but the things they've done since—such as the [Liquid Glass design language](https://en.wikipedia.org/wiki/Liquid_Glass)—have not been really popular with users. The features that Apple tends to add, he said, are "basically about tying it closer to the iPhone, tying you closer into their ecosystem". That isn't about improving the desktop, it's about increasing their moat.

Microsoft "is just making one mistake after another". He cited the company's attempts to force users to store files using its [OneDrive](https://en.wikipedia.org/wiki/OneDrive) cloud-storage service, the privacy-invasive [Windows Recall](https://en.wikipedia.org/wiki/Windows_Recall) feature that drew widespread criticism, and "ads everywhere" on the Windows desktop. His point was not to bash Apple or Microsoft, though the audience didn't seem to object to this, it was to point out that the Linux desktop could not follow their example any longer. "The whole Linux community has been just waiting for Apple and Microsoft, more or less, to take all the risks, and they're not taking any more risks."

#### Three pushbacks

Jenson emphasized that he didn't want to change everything, but he wanted to see experiments and additional development aimed at improving the desktop experience. However, he said that he gets pushback when he tells people that desktop innovation has stopped, and that the Linux desktop "can't just patch and bug fix our way into the future, we have to start taking leadership ourselves".

He grouped the pushback into three categories. The first objection he receives is "mobile won; the desktop is old, just do what mobile does". He dismissed that by saying that mobile has indeed won the consumer market, social media, and other areas, but "it didn't win productivity". People still do their work on computers, because "desktops are silently awesome; they have keyboards and big screens, and they can do amazing things". However, people have lost sight of that and don't really appreciate how good desktops are. "I think we should understand why they're good, and then what we can do to make them better."

The second objection he hears is that there are only so many ways that the WIMP model can work. "Just stop trying to fix it; it's done." He disagreed with that as well, and thought that there was more to be done. The final, particularly strong, objection from the tech community is "don't touch my stuff". People get angry when they have worked hard to get their desktop environment just as they like it and something changes to disrupt that. "And I get that. As I said before, I don't want to change everything." But the world is changing, "and we need to grow our way gently into some new things."

#### What is UX?

Thinking about where to go next with desktop design gets to the question "what is UX?" He said that term was misunderstood and that people, especially manager types, "they think it's just pixels, but UX is so much more than pixels". It helps to think of UX as having layers, he said. There is style, which consists of white space, iconography, color schemes, and yes, pixels. But there is also structure, he said, which is how the user moves through the application, the navigation model.

Strategy is another part of UX; understanding who the target user is, and determining what they want. Deciding what is important, and what is not; what will the development team prioritize and what will it ignore? "The fastest way a UX designer can save your team time is to help you say no."

There is also a layer he called "stuff": the lower-level limitations of the underlying technology that a platform has which influence UX. For example, he said, if an application is being created for DOS, "you're not going to write a nice Macintosh application. The technology dictates what you can do". The desktop has been evolving for 40 years and has "historical cruft" that carries expected standards.

> We just have a bunch of stuff that we just accepted. And I think if we're going to fix the desktop, we have to understand this lower-level limitation, and how do we want to fix that? We just accept that mobile and desktop has stuff today, and we just don't think about fixing it.

People forget, he said, that the original Macintosh screen was "a whopping 342 pixels high". Naturally, windows overlapped a lot. Since then, "we just assumed that Windows have to overlap". But many people have "giant, giant screens", and the existing window-manager model does not work so well for those devices. "I can understand why people use tiled window systems and so forth, because this existing model just does not work very well."

#### Understanding the future

Early in the talk, he had referenced a saying by [Alan Kay](https://en.wikipedia.org/wiki/Alan_Kay), who had led design of the first WIMP interface at Xerox PARC: "Perspective is worth 80 IQ points." Jenson now circled back to Kay: "Who, as you can tell, I really like a lot. \[...\] He talks about three steps to understand stuff". The first step, he said, was to think about the present, "what is happening right now?" The second is "don't forget the past, know what work has happened before", and the final step is to ensure that "your questions are really clear". Too many people rush to a solution, he said, but they should be rushing to questions. If the questions are clear, then the answers will be better.

He did not want to make the talk about AI, he said, but he had to address it. "I have to talk about AI, and it pisses me off because there's so many cool things we could be talking about, and AI just sucks up all the oxygen, and it's all you can talk about". But, he conceded, there are many things happening with AI on the desktop, albeit in a weird way.

He explained some of the current efforts to integrate AI with the desktop as "trying to sneak AI in" while keeping the UI exactly the same. Other efforts try to use AI to understand what is on the screen and use chatbots to interact with the interface, "which I think is kind of missing the point here". That is, until last summer when Anthropic introduced Claude Code. "It completely changed things, because was looking down into your filesystem" and drew context from the content of a user's files. That allowed it to do more interesting things and interact with the system.

After mentioning Claude, he paused to say he was only speaking about frontier models because everyone else is discussing them. "I have a lot of personal problems with the frontier models. They are ethical and environmental disasters. I am not very excited by them". He said he was excited by [Apertus](https://apertus-ai.org/), which is billed as a fully open, responsible model for sovereign AI created by the [Swiss AI Initiative](https://www.swiss-ai.org/). "I do think it's possible to talk about ethically trained small language models running locally, and you don't have to sell your soul to the devil."

#### The curse of direct manipulation

The key point he wanted to make was not about generative AI uses, but about how giving it access to the filesystem improved its usability. It now had context and could work with the user's data, but it was working at the filesystem level and not using the desktop UX.

He talked about the desktop building blocks, basically the familiar WIMP model plus the desktop clipboard, as the way that users move data around. Expert users can move data around quickly, he said, but there's a problem: the desktop is stateless. He called this the curse of direct manipulation; there is no working memory to the desktop. "If you copy a few too many things to the clipboard, oh sorry, it's gone".

Jenson theorized that the reason there are so many desktop environments, window managers, and Wayland compositors for Linux—as well as alternative clipboard managers—is because the desktop isn't quite working right for users. He thought it was really about working memory: "How do I manage my data in such a way that I can remember it across all of these things and actually use it." His perspective shifted to asking "what would we do to working memory to improve the desktop UX?"

AI may have given him the idea, but he added that he wanted to solve the desktop UX for people first. "If the AI can use it, great. I don't care. That comes later."

#### Remember your past

There is prior art in trying to create systems that help provide users with more context and working memory related to their data. He referenced [a paper](https://dl.acm.org/doi/10.1145/381854.381893) called "Lifestreams: a storage model for personal data". That paper made the case that desktop systems are "are ill-equipped to manage the electronic information and events of the typical computer user", and introduced the Lifestreams idea as a metaphor for dynamically organizing a user's computer workspace.

That paper influenced the ill-fated [WinFS](https://en.wikipedia.org/wiki/WinFS) project from Microsoft, which was an attempt to merge data storage and management based on relational database systems. It was demonstrated in 2003 and scheduled to ship sometime later, but ultimately the project was shelved in 2006. On the free-software side of the house, there was the [semantic desktop](https://en.wikipedia.org/wiki/Semantic_desktop) project [NEPOMUK](https://en.wikipedia.org/wiki/NEPOMUK_\(software\)), which was a research project funded by the European Union.

A version of NEPOMUK was [included in KDE 4](https://web.archive.org/web/20130317083409/http://nepomuk.kde.org/node/1), with the idea was that it would allow applications "to use information from all over the desktop, the web, other devices, and combine it into one coherent interface". It was later removed and replaced with the [Baloo](https://community.kde.org/Baloo) file-indexing and search framework for the Plasma desktop, which has some overlap with NEPOMUK's functionality but with a reduced scope of features.

Jenson said that he is concerned that people see the failure of those projects as proof that the ideas were wrong. He argued that is not the case, "I would say that it is the hardware and systems at the time let the vision down. I think it's time to rethink these projects" in light of newer hardware and better systems.

#### Prototypes

Having visited the present and the past, as Kay recommended, he had come up with a few questions that he thought worth exploring related to desktop UX. The first was "what would happen if we only designed for large monitors?" What would it unlock if there were desktop designs that ignored laptops and only focused on widescreen monitors? Another question he had was how could the desktop capture a user's intent over time?

Those questions led to the prototypes that he said he was a little nervous to show the audience "because technical people love to find mistakes". The demos are about [29 minutes](https://media.ccc.de/v/kde2026-7-are_we_really_going_to_use_the_same_desktop_ux_forever#t=1736) into the [video of Jenson's talk](https://media.ccc.de/v/kde2026-7-are_we_really_going_to_use_the_same_desktop_ux_forever). His slides have not yet been published but he told me at the event that they would be at some point.

The first demo was to show how a desktop might make better use of a widescreen display. He noted that the center of a widescreen monitor is good for working with an application, but it becomes more difficult to work on the sides of the monitor. "It's good for peripheral vision, but not good for working".

He demonstrated a desktop layout, a screenshot of which is below, that would put the focus on the window or windows in the middle of the monitor. He had included a grid in the background that tapers off from the middle of the screen to simulate a widescreen monitor. The idea was that a widescreen-first desktop would emphasize information in the middle of the screen, and then use an [Exposé](https://www.cultofmac.com/expose)-like tiling model to arrange the rest of the desktop's windows. This would make the windows that were not in use easier to interact with, he thought, and be a better model than virtual desktops for managing lots of windows. "I'm not against virtual desktops. Virtual desktops are used by very organized and intelligent people. I know my audience." However, he said, most people can't deal with virtual desktops.

> [![[Window manager widescreen demo]](https://static.lwn.net/images/2026/wm-demo-sm.png "Window manager widescreen demo")](https://lwn.net/Articles/1095521/#wm)

He also demonstrated dragging a music-player window all the way to one side of the screen, which he called "the stash area". As it moved to the edge, it transformed into a widget with just a play button instead of showing the full window. He thought that it would be possible to do the things he was demonstrating with Wayland as it is today.

Jenson put up another demonstration of an improved clipboard model that copied ideas from the [Obsidian](https://obsidian.md/) editor's [Canvas](https://obsidian.md/canvas) feature. Canvas allows users to drag and drop images, text, and files to create a visual layout of information. A user could create a document and drag information from a web browser into the document, drop a file in from another source, "the idea here is that this is now effectively a collection of stuff that I have gathered for this document. And if I close it and then come back tomorrow, it's all still there". He added that a local AI would be able to come in and organize the information a user had gathered, "oh, look, you've got a bunch of hotels, let me organize them for you".

The final idea he discussed was a way of gathering "privacy-preserving data" to help give users a better visualization of things like web-browsing history. A web browser will only give the user a view of their browsing history that "is not very useful" he said, so he tried to gather data "not using AI, just using simple math" that would use attention signals—such as how long the user was on a web page—to tell a story of where they spent their time on the desktop. "I basically came up with this spatial associative episodic memory prompt" to organize data in a helpful, interesting way. He thought that working memory was a good way to explore new ideas for desktop UX.

> So the goal with this talk is to say, let's start to take this apart. And I really hope that my prototypes cause you guys to come to me and say, We forgot about this, and let's talk about that. \[...\] So I just want us to try, because no one else is going to try, and I think someone's got to start.

There was time for a single question. An audience member asked how Jenson's model would protect against all of the privacy concerns that Microsoft faced with Windows Recall. "How are we going to make sure that this history remains only accessible by the user and cannot be hacked in like it could happen with Windows Recall?"

Jenson answered that his idea was to gather telemetry data, "not interesting information", which would significantly reduce the attractiveness of the data to would-be attackers. He said he did not want to underplay the issue, though, and wanted to build prototypes to see if the idea was good first, and then explore ways to encrypt or protect the data. He agreed that even this data could be valuable information. He added that the biggest problem with Windows Recall was "how stupidly they protected it" and he was sure others could do a lot better job.

\[I would like to thank the Linux Foundation, LWN's travel sponsor, for its assistance with my trip to Graz, Austria for Akademy 2026.\]

| Index entries for this article                           |                                                                         |
| -------------------------------------------------------- | ----------------------------------------------------------------------- |
| [Conference](https://lwn.net/Archives/ConferenceIndex/) | [Akademy/2026](https://lwn.net/Archives/ConferenceIndex/#Akademy-2026) |
