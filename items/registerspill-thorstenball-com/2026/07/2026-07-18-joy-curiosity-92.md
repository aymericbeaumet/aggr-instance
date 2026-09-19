---
title: 'Joy & Curiosity #92'
link: https://registerspill.thorstenball.com/p/joy-and-curiosity-92
source: registerspill-thorstenball-com
published: 2026-07-18T13:08:38Z
updated: 2026-07-18T13:08:38Z
first_seen: 2026-09-19T21:30:23.395188495Z
authors:
- Thorsten Ball
summary: Interesting & joyful things from the previous week
content: extracted
html: 2026-07-18-joy-curiosity-92.html
preview:
  file: 2026-07-18-joy-curiosity-92.preview-01c5e4dede2f.webp
  width: 256
  height: 140
  color: '#2d2c34'
images:
- source: https://substack-post-media.s3.amazonaws.com/public/images/6ad45306-50b8-47ef-abf8-a5b3c9c3c36f_1510x824.png
  original:
    file: 2026-07-18-joy-curiosity-92.image-181c7bac0080.png
    width: 1510
    height: 824
  color: '#0b0707'
- source: https://substackcdn.com/image/fetch/$s_!r3Ot!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F22ede80c-e5d0-4242-b474-bf2a372655e1.tif
  original:
    file: 2026-07-18-joy-curiosity-92.image-1c216b12af70.png
    width: 957
    height: 38
  variants:
  - file: 2026-07-18-joy-curiosity-92.image-44e99bb87104.webp
    width: 320
    height: 13
  - file: 2026-07-18-joy-curiosity-92.image-0e4e2bbd22b1.webp
    width: 640
    height: 25
  - file: 2026-07-18-joy-curiosity-92.image-f6e047f40afc.webp
    width: 957
    height: 38
  color: '#000000'
- source: https://substackcdn.com/image/fetch/$s_!3QQ1!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fcdb4120e-8569-4bae-918b-7bf40982e1d9.tif
  original:
    file: 2026-07-18-joy-curiosity-92.image-a18bee6a6c6f.png
    width: 945
    height: 46
  variants:
  - file: 2026-07-18-joy-curiosity-92.image-4c83b4d83c32.webp
    width: 320
    height: 16
  - file: 2026-07-18-joy-curiosity-92.image-e84a46e394a7.webp
    width: 640
    height: 31
  - file: 2026-07-18-joy-curiosity-92.image-e8c0d7762c16.webp
    width: 945
    height: 46
  color: '#000000'
---

This week was *busy*: we shipped a lot of things, recorded quite a few things, I worked on my Laracon talk, chatted with amazing programmers in different timezones, and our local public swimming pool celebrated its 50th birthday.

There wasn’t a lot of reading this week, but a lot of *thinking* and wondering and questioning and *aha!*-ing. And I kinda can’t shut up about it. So…

Some of you might remember my original description of this newsletter: “It’s very informal; it’s what I’d send you if you were to ask me what’s on my mind this week in an email.”

Now, here is what I’d *say* if you were to ask me what’s on my mind this week, on a phone call:

0:00

-18:54

Audio playback is not supported on your browser. Please upgrade.

[![](https://substackcdn.com/image/fetch/$s_!r3Ot!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F22ede80c-e5d0-4242-b474-bf2a372655e1.tif)](https://substackcdn.com/image/fetch/$s_!r3Ot!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F22ede80c-e5d0-4242-b474-bf2a372655e1.tif)

- Finally: [Amp now has subscriptions](https://ampcode.com/news/subscriptions) . Yes, you read that right. Go there, get a subscription, pair it with your ChatGPT subscription so you get infinite GPT-5.6 tokens, and spawn those orbs.

- We also shipped [agent-to-agent communication in Amp](https://ampcode.com/news/from-agent-to-agent) : agents can now spawn other agents — anywhere you have a Amp instance running or in an orb — and then send messages and *files* to them. The number of “holy shit, it just …” messages this produced in our internal Slack is insane. To give you a taste: this morning I was hacking on something in our orb, but the agent in the orb lacked the permissions to upload an asset to our bucket. So the agent said: hey, start `amp —no-tui` on your machine, where you have permissions, then I’ll start a thread there, send it that asset, and ask it to upload the file. And… it fucking did it! Exactly like that! I started `amp —no-tui`, I saw the new thread being created, I watched both of them, and saw how they sent messages to each other. Wild!

- Raising An Agent is back! Here’s the first episode of the new season: [The Local Dev Env is Dead](https://www.youtube.com/watch?v=Kpg_D5MUWnE) . Or [listen on Spotify](https://open.spotify.com/episode/5zW7gvw1SgZXqgZEq4VYBo) .

- I know what you’re thinking: “Thorsten, your voice… I need more of it.” Don’t worry, I got you. Dominic and Morten invited me to be a guest on the go podcast and our episode came out this week: [Agentic engineering is here to stay with Thorsten Ball](https://gopodcast.dev/episodes/093-agentic-engineering-is-here-to-stay-with-thorsten-ball) .

- Yes, I know, you want more. I got more: [Evan Phoenix, Quinn and I shipping from orbs to a Miren cluster](https://x.com/evanphx/status/2077116272840699908) . There’s coding in there, but also a lot of talking about coding and developer tools and the future of software and deployments. Evan has shipped and worked on more successful developer tooling than basically anyone else (Rubinius! Puma! Hashicorp!), so him sharing his thoughts was a treat.

- You want more? Okay, okay. [I recorded a short video with thoughts](https://x.com/thorstenball/status/2076657564394168640) on this race we’re in and the forces at play that I don’t think most developers are aware of. Titles I considered: “man, it must suck to be a model house”, “the tectonic forces shaping software”, “once the compute is available, this will all change again”, and “there won’t be a rugpull”

- Hardcore Kindle reader here, but this really, really made me want to buy physical books again, even though it should probably serve as a warning to not do that: “Mendel Uminer faced a crisis when his landlord objected to [the 10,000 volumes in his New York studio apartment.](https://www.nytimes.com/2026/07/09/style/too-many-books-new-york-city-apartment-scholar-landlord.html?unlocked_article_code=1.wVA.jiKa.YLwm2QauW-7M&smid=threads-share) ”

- Fabien Sanglard: [Don’t you mean extinct?](https://fabiensanglard.net/extinct/index.html) Lovely article. You need to read this for the Phil Tippett story alone.

- Another *amazing* Fabien Sanglard article: [Jurassic Park computers in excruciating detail.](https://fabiensanglard.net/jurrasic_park_computers/index.html) Jurassic Park is one of my favorite movies of all time. Easy top five. I love it. And I’m sure its depiction of computers had an influence on my life. But one thing I hadn’t noticed until reading this article is that Oppenheimer portrait.

- [After 7 years in production, Scarf has reluctantly moved away from Haskell](https://avi.press/posts/2026-07-10-after-7-years-in-production-scarf-has-reluctantly-moved-away-from-haskell.html) . I’m honestly not sure what to say about the skeptics any more. Maybe this: for decades now, some programmers were really lucky in that what they loved to do, what they saw as their craft and their passion, was valuable to businesses. Now that’s not the case anymore. And if you don’t adapt, then, well, you end up not being valuable to businesses anymore.

- Holy moly: [Measuring input latency on Linux: X11 vs Wayland, VRR, and DXVK](https://marco-nett.de/blog/measuring-input-latency-on-linux-x11-vs-wayland-vrr-dxvk/) . If you’re in the mood to find out how deep a rabbit hole can go, then read this. Very fascinating. Also: I can’t believe it’s 2026 and we’re still talking about X11 vs. Wayland. Wayland was released in 2008.

- [John Carmack is smart](https://x.com/id_aa_carmack/status/2074248758422864226?s=46) .

- [Linus Torvalds on the Linux kernel mailing list](https://lore.kernel.org/linux-media/CAHk-=wi4zC+Ze8e+p3tMv8TtG_80KzsZ1syL9anBtmEh5Z40vg@mail.gmail.com/) : “There are other questions around AI (like what the economy of it will actually look like in the end), but ‘is it useful’ is no longer one of those questions. Anybody who doubts that clearly hasn’t actually used it. Yes, it can also be a somewhat painful tool, both for maintainer

  workloads and just from a ‘it keeps finding embarrassing bugs’ standpoint. But the solution is not to put your head in the sand and sing ‘La La La, I can’t hear you’ at the top of your voice like some people seem to do.”

- There is this whole debate going on around whether you should still read the code agents produce. antirez now chimed in too: [Control the ideas, not the code](https://antirez.com/news/169) . He writes: “But if I had my hands free, you know what I would do, instead? Use all the time that the review is taking me to do more QA, to think at the next optimization idea and apply it, and to use LLMs to write a DESIGN.md file where each data structure is described in human language, with the ideas it contains, the implementation tricks, the design. That, in the future, is going to be much more useful.” I think he’s onto something here. I personally do spot checks of code and mostly don’t care about single functions anymore, except when the blast radius would be huge or when it’s super critical. But it ties back to what antirez writes: I want to control the ideas, not the code.

- And here’s Sean Goedecke: [In defense of not understanding your codebase](https://www.seangoedecke.com/in-defense-of-not-understanding-your-codebase/) . As always, very good post. I agree. People who say “you have to review every line” make me think that either they haven’t worked with (a) a model that was released in 2026 or (b) other people in a multi-team engineering org.

- Justin Jackson on [the value of software in a Fable world](https://justinjackson.ca/software-fable) : “Generally, the economy rewards difficulty and rarity. If something is hard to do or make, you get to charge more. So if AI reduces the time it takes to build software, what happens to the value of software?” See also [my thoughts on software & oil here](https://registerspill.thorstenball.com/p/joy-and-curiosity-78) .

- [What working at Google feels like](https://newsletter.goodtechthings.com/p/heres-the-sad-state-of-the-cloud) : “ Have you ever changed a flat tire on the shoulder of the freeway? When you get out of your car and stand by the side of the road, you realize you are in a world scaled for 30,000-pound tractor trailers, not for human beings. The white stripes between the lanes that usually flash by like little punctuation marks are actually ten feet long and thirty feet apart . Even the road signs look absurdly out of scale, like those textbook-sized large-print editions of bestsellers the library stocks for elderly people. And then one of those tractor trailers hurtles by at seventy miles per hour, inches from your fragile body, and it feels like God himself is screaming in your face. That’s exactly what working at Google feels like at first. You realize very quickly you are inhabiting an environment that was not designed for humans to experience with their naked minds. Even the smallest problems—which customers are we allowed to send this email to? What countries will have special legal requirements around the deployment of this tiny feature?—are so mind-numbingly complex that they make you want to shrink down into a fetal ball on the side of the freeway and just let the traffic shriek past.”

- “ [I had a french professor who once said](https://x.com/boywaif/status/1602301368705978369?lang=en) if you just did something like going to the supermarket and experienced it fully without the goggles of habit and catégories you would go crazy with pure sense and joy. I think about it all the time. In a way this is all for him.” And I had a french teacher once who said that bean soup is the poor man’s piano. I think about him every time I eat beans. So there’s that.

[![](https://substackcdn.com/image/fetch/$s_!3QQ1!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fcdb4120e-8569-4bae-918b-7bf40982e1d9.tif)](https://substackcdn.com/image/fetch/$s_!3QQ1!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fcdb4120e-8569-4bae-918b-7bf40982e1d9.tif)

No posts
