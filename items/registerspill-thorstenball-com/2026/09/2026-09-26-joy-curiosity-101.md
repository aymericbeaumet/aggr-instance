---
title: 'Joy & Curiosity #101'
link: https://registerspill.thorstenball.com/p/joy-and-curiosity-101
source: registerspill-thorstenball-com
published: 2026-09-26T05:52:02Z
updated: 2026-09-26T05:52:02Z
first_seen: 2026-09-26T10:11:22.925913775Z
authors:
- Thorsten Ball
summary: Interesting & joyful things from the previous week
content: extracted
html: 2026-09-26-joy-curiosity-101.html
preview:
  file: 2026-09-26-joy-curiosity-101.preview-eebfd609c94e.webp
  width: 256
  height: 170
  color: '#acafb6'
images:
- source: https://substack-post-media.s3.amazonaws.com/public/images/046582be-a3ca-4927-a613-207cb8b36d93_1504x998.png
  original:
    file: 2026-09-26-joy-curiosity-101.image-981451fae0ca.png
    width: 1504
    height: 998
  color: '#a4abb7'
- source: https://substackcdn.com/image/fetch/$s_!bgBi!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F432601a4-318c-4e79-af8b-135f1cbe1929_941x32.tif
  original:
    file: 2026-09-26-joy-curiosity-101.image-ae8b4cc42dc1.png
    width: 941
    height: 32
  variants:
  - file: 2026-09-26-joy-curiosity-101.image-c1cbe6a86d17.webp
    width: 320
    height: 11
  - file: 2026-09-26-joy-curiosity-101.image-1754b700c7ed.webp
    width: 640
    height: 22
  - file: 2026-09-26-joy-curiosity-101.image-8e92f57a288e.webp
    width: 941
    height: 32
  color: '#000000'
- source: https://substackcdn.com/image/fetch/$s_!i5I-!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff887205b-ca58-41c0-8181-a25c9afd6c29_918x33.tif
  original:
    file: 2026-09-26-joy-curiosity-101.image-c3e67a23d6c2.png
    width: 918
    height: 33
  variants:
  - file: 2026-09-26-joy-curiosity-101.image-02efa639355f.webp
    width: 320
    height: 12
  - file: 2026-09-26-joy-curiosity-101.image-b1990fcebb81.webp
    width: 640
    height: 23
  - file: 2026-09-26-joy-curiosity-101.image-1114d3663e19.webp
    width: 918
    height: 33
  color: '#000000'
- source: https://substackcdn.com/image/fetch/$s_!8x--!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F074c8140-dde0-4f2f-aca2-121a9858b94d_961x37.tif
  original:
    file: 2026-09-26-joy-curiosity-101.image-fb6582921a00.png
    width: 961
    height: 37
  variants:
  - file: 2026-09-26-joy-curiosity-101.image-de1fe8887b8b.webp
    width: 320
    height: 12
  - file: 2026-09-26-joy-curiosity-101.image-0d5c8878e613.webp
    width: 640
    height: 25
  - file: 2026-09-26-joy-curiosity-101.image-2bf2f5c935a1.webp
    width: 960
    height: 37
  - file: 2026-09-26-joy-curiosity-101.image-9f97dadfc48b.webp
    width: 961
    height: 37
  color: '#000000'
---

Last week I was on [Matt Swanson](https://x.com/_swanson) ’s podcast and we ended up sharing thoughts and vague predictions about programming languages and frameworks. Matt said that he’d be going to Rails World the week after and that he wouldn’t be surprised if DHH said “Rails is over.” Prescient. DHH didn’t *exactly* say that Rails is over, but, well, some people did call the keynote a “funeral.”

But that shouldn’t be surprising, right? The way we’ve treated languages and frameworks for the past, say, twenty years is at odds with the fact that writing code by hand is on its way out.

I am not sure how exactly this will play out, but here are some loose thoughts:

- Frameworks are no longer the biggest developer productivity lever. Agents are a hundred times bigger.

- Syntax doesn’t really matter anymore, as long as agents can write it well.

- Tool ergonomics don’t matter that much either, do they? Previously, I loved that `go` comes with `go build` and `go test` and `go run` , but now I wouldn’t care if those commands were seventeen times longer.

- But I think shared abstractions are still worth it. A framework gives you a pre-defined way to access a database, to do auth, to divide things into production and development… That’s still handy. Not because it would cost tokens to build it myself (won’t matter in the future, see below), but because I just don’t want to think about it.

- What will matter a lot in the future: performance characteristics, resource usage, failure modes, observability, debuggability, deployments, rollbacks. And all of that needs to be legible to the agent. I’ve tried developing something with the often praised Cloudflare Durable Objects, on which you can run JavaScript, and it was a disaster: the agent constantly thought it was writing normal Node.js JavaScript; it didn’t know the runtime characteristics of the Durable Objects; it couldn’t easily access the logs. In fact, there *are no logs* that tell you when your code gets evicted and when it gets resumed… You want the opposite to be the case: the agent should know from looking at the codebase how it will be executed and how it can see that.

- The big force is that we’re switching from “I prefer this language because I enjoy working with it” to “I prefer this language because my agent can get great results with it.” Now, how would your preferences change if you switched from driving a car to controlling it remotely? You wouldn’t care about heated seats and AC, would you? But you’d care about how fast it can brake, I assume.

- Ecosystems will change. I can’t remember the last time I browsed through GitHub to find a library to do a thing. The old NPM credo of “many, many tiny modules” seems even sillier now than it did ten years ago.

- Sometimes I wonder whether the thing that makes some developers say that agents will change everything and others that they can’t write good code is the language they used with the agent. 99% of the code I’ve had agents write was in TypeScript. Not a language I love, but, hey, who cares? And agents seem great at it. I wonder what my thoughts would be if I still were writing Rust.

- Then again: I no longer think that “is the language well-represented in the training data?” matters as much as I thought it would. Intelligence generalizes and I’ve seen agents just *crush* custom DSLs that have not shown up in any data, ever. We previously said about humans that “if you learned 5 different languages, you kinda know them all” — maybe that’s what’s going to happen with agents too? And it kinda makes sense, right? Why wouldn’t a frontier model like Astra or Fable be able to use a new language as long as it can run it and have a feedback loop?

- Very pessimistic on the future of “paper-over” languages and frameworks. You know: this language but with nicer syntax. CoffeeScript, if you’re old enough to remember. Haml, Sass, Less — not sure. What about Elm or ClojureScript? Hmm.

- A lot of testing frameworks started with TDD in mind: you write a test in which you describe the behavior you want, you run the test to see it fail, you make it pass. Then, with growing adoption of tests, people started using those very same frameworks to add tests after everything already worked. Regression tests. Now we have the very same frameworks being used by agents to write tests god knows how and essentially no one looks at these billions of lines of test code that are generated every day now. Will we still have `describe` and `it` blocks in ten years? Just like some terminal emulators still mention baud rates? I do think that the models will get so good that they don’t “need” unit tests in the same way humans needed them: to make sure something works. But maybe they won’t stop writing them and we’ll end up with effectively useless tests piling up?

- I’d be incredibly surprised if formal methods and strong static typing had the boom that their fans say they will have. Vitamins, not painkillers; worse is better, etc.

- I have three little apps that I use every day and that I had the agent write, and I have zero clue what language they’re written in. I think it’s JavaScript?

- Porting from one language to another seems to be a *completely different* thing now compared to three years ago.

Again: I don’t know where we’ll end up, but I do think being aware of the forces at play is important. It’s also fun stuff to think about.

[![](https://substackcdn.com/image/fetch/$s_!bgBi!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F432601a4-318c-4e79-af8b-135f1cbe1929_941x32.tif)](https://substackcdn.com/image/fetch/$s_!bgBi!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F432601a4-318c-4e79-af8b-135f1cbe1929_941x32.tif)

*Before we get to the J&C juice: I’ll be in NYC with the Amp team Oct 5-11 and in SF the week after, Oct 12-17. My schedule will be busy and chaotic, but if you’re around and want to grab a coffee, let me know!*

[![](https://substackcdn.com/image/fetch/$s_!i5I-!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff887205b-ca58-41c0-8181-a25c9afd6c29_918x33.tif)](https://substackcdn.com/image/fetch/$s_!i5I-!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff887205b-ca58-41c0-8181-a25c9afd6c29_918x33.tif)

- [We recorded a new Raising An Agent episode](https://www.youtube.com/watch?v=3v7dIHh15dk) after I said to Quinn: “Man, I’m so full of hot takes today. I need to record a video.” So, there it is: an episode full of hot takes. From a whole engineering org making everyone use Qwen, to why GDP isn’t increasing if you aren’t letting your agents go *vertical* , to why you’re wasting time if you’re waiting on your agent instead of the other way around, to why I’ve been very disappointed by a lot of “software engineers” in the last two years.

- [Expect this to continue](https://jyn.dev/tokens-too-cheap-to-meter/) : “If we combine all this, we see about *2.5 orders of magnitude* decrease in token cost in the last year. Models are about 100x as cost-efficient per-task. Hardware is about 1.3x as energy-efficient per-token. Engines are about 1.4x as energy-efficient per-token.” The strongest force in technology today. I stand by what I wrote in [last week’s predictions about the future of software development](https://thorstenball.com/blog/2026/09/19/what-i-believe-about-the-future-of-software-development/) : “Tokens are the new computing paradigm. Everything will be re-made on top of it.”

- So, DHH lit the Ruby & Rails world on fire by [giving a keynote in which he doesn’t talk about Rails all that much](https://www.youtube.com/watch?v=vDjW_dRyKXY) . Instead, he said what others (*hey* , what’s up) have said for at least the last six months: writing code by hand is over; these agents are really good; choosing Ruby over other languages due to its developer friendliness doesn’t make a lot of sense anymore; old engineering tradeoffs should be revisited. But he said it in a way that only DHH can. He’s very, very good at boiling things down to their essence and turning them into statements that make you choose whether you’re for or against it. It’s impressive, honestly. Read the [Hacker News comments](https://news.ycombinator.com/item?id=49817680) to get a taste. Now, predicting the next six months, I wonder when the Omarchy community will run into the question of: wait, so we now have this malleable operating system, but… if agents write and debug all the code, why do we even need it?

- Thomas Dullien, or: [Halvar Flake](https://x.com/halvarflake) , gave a presentation: [An age of experimentation.](https://thomasdullien.github.io/about/slides/An-age-of-experimentation-BlueHat-Asia-2026.pdf) It’s really good and I highly recommend you click through it. Here, to give you a taste: “Claim: Determinism is dying, and it’s unclear how much will remain.”

- [New Peter Thiel interview](https://www.youtube.com/watch?v=B7yl7fEHeKM) ! Say about him what you want (and there’s a lot to say), but his ability to read the vibes in the world seems to be rarely matched. What makes this interview also interesting is that the interviewer is [Mathias Döpfner](https://en.wikipedia.org/wiki/Mathias_D%C3%B6pfner) , quite the controversial billionaire himself. At some point in the interview, Thiel compares the twenty richest people under 30 in the US with those in Germany and says that the twenty in Germany all inherited their wealth. Guess how Döpfner got his shares of Axel Springer SE? Heavily discounted and some as gifts, from Springer’s widow.

- [Attention is all you have](https://alicegg.tech/2026/09/21/attention) : “If, like me and most people, you spend the major part of your day focused on your device, there’s no doubt it’s affecting you. And when you let someone else dictate what appears on your screen, it’s the same as giving them the key to your brain.”

- “But there are [pleasures to be had from books](https://www.newyorker.com/culture/cultural-comment/pleasure-of-reading) beyond being lightly entertained. There is the pleasure of being challenged; the pleasure of feeling one’s range and capacities expanding; the pleasure of entering into an unfamiliar world, and being led into empathy with a consciousness very different from one’s own; the pleasure of knowing what others have already thought it worth knowing, and entering a larger conversation.”

- Martin Fowler: [I don’t like LLMs](https://martinfowler.com/articles/2026-dont-like-llms.html) . “I don’t like them. They talk to me in this grating LLM-voice, an uncanny valley of talking to a real human. They confidently bullshit me - often giving me useful, helpful answers. But also just making stuff up with the same assurance - and with only a veneer of fake remorse when I call them out on it.” He’s got a point. Many times a day I think to myself “*god, shut UP* ” when the model comes back with whatever the latest equivalent to “you’re absolutely right!” is: spines, seams, belts and braces (or suspenders). But… less and less so? I parse their output more like a receipt I get handed in a shop or in a restaurant: skip the stuff at the top, ignore the gibberish at the bottom, zoom in on the stuff there in the middle.

- I didn’t know about the Moving Image Archive but was delighted when I came across [its collection of animated maps](https://www.movingimagearchive.com/collection/animated-maps) .

- This seems very neat and makes me want to build something with Go: [Platform-independent SIMD](https://go.dev/blog/simd-experiment) .

- “I text him that night and I said, ‘Hey, I want to ask you a question. Can I coach you hard?’ \[…\] Then the next day he sees me in pre-practice, and I said, ‘Just let me explain this. Do you see yourself being in the Hall of Fame someday?’ And he said, ‘Yeah.’ And I said, ‘All right. Well, I don’t think your trajectory right now is steep enough to make that goal happen. I think your trajectory is five Pro Bowls, a couple All-Pro teams, phenomenal career, all-time leading rusher, but I think your trajectory, and I think your practices have to be…’ And then he starts complaining to me, and I said, [‘I thought you told me I could coach you hard.’](https://www.youtube.com/watch?v=2GbIJISDE-Q) And then, you know, it hit him.”

- [How to Unclench](https://howtounclench.com/) . I finally read this after it made a big splash last week. It’s a much faster read than I thought it would be. It’s like a neat little mini-book.

- [Robert O’Callahan:](https://robert.ocallahan.org/2026/09/goodbye-google.html) “I’m resigning from Google today. This has not been an easy decision. I love my colleagues and my work environment, and being paid handsomely to solve fun puzzles has been amazing. But my team’s goal is ultimately to make AI much cheaper and lower-latency, and I don’t think that’s good for people right now: I firmly believe AI progress is currently far too rapid (and I have doubts about the destination too).”

- “I started to explain how it happened when they cut me off with ‘Michael, [I don’t want the details’.](https://michaelheap.com/i-dont-want-the-details/) ” Good stuff.

- [Thomas H. Ptacek and Kurt Mackay are leaving fly.io](https://sockpuppet.org/blog/2026/09/25/what-even-is-an-os-now/) to build a phone: “Today, almost all software comes from expert strangers. But soon, strangers will stop supplying our apps, and instead ship just their building blocks. Sure, there will still be megaproject browsers and word processors. But there’ll be thousands of times more applications that pull in 1/7th of the guts of a word processor to solve some idiosyncratic work or home life problem for somebody who doesn’t know what a for-loop is. \[…\] That’s what we’re working on: a platform that is the device we would want to have in the world I just described. So: we’re building a phone.” I nod my head to the boldness of entering one of the most competitive markets of all time.

- It’s totally not the point of [this Obie Fernandez post](https://x.com/obie/status/2101779116056088732) , but I can’t stop thinking about this part here: “Trying to make a point, I hit enter to accept Fable’s first suggestion. Minutes later I hit enter again, and then again, choosing to delete some dead code. We start making a PR. My friend asks me to make sure it’s set to draft. Sure, whatever. Fable does its thing. My friend checks the diff. It’s a simple deletion of dead code and associated unit tests. I want to push on, but my friend begs me to stop. ‘You don’t understand Obie, I can’t just do what you’re doing, man.’ I challenge him to explain why not. He explains that he has a boss and teammates and that he can’t just make changes like that, he has to present plans and execute on them.” It made me remember what it feels like to work in such teams, where you can’t make decisions alone, where you’re not trusted to make a call like “I’m going to refactor this API” or “I’m going to delete this” or “I’m going to add a new feature that lets us…” without having reached a consensus with the team. Remembering that made me feel sad, thinking: “wow, imagine what it’s like to now have AI but no power, no trust, no freedom to use it?” There’s no way around it: if you box AI into this little corner where all it can do is change code on your local machine and help you push it up as a PR, you’re holding the leash at a fraction of its real size.

- [Intellectuals are F\*cking Idiots](https://markmanson.substack.com/p/intellectuals-are-fcking-idiots) : “Reality always wins. But Intellectuals are rewarded for their models, not reality. And the data and analysis that looks elegant on paper is often disastrous on the ground. Yet, when their models are contradicted by reality, most intellectuals don’t have the courage to accept the reality, instead they double down on their models …and *this* is what turns them into idiots.” If it’s nothing else, this was entertaining!

- I thought of this John Carmack post again, so here it is, again: [“Make better decisions and fill your products with ‘Give a Damn’!”](https://www.facebook.com/permalink.php?story_fbid=pfbid0iPixEvPJQGzNa6t2x6HUL5TYqfmKGqSgfkBg6QaTyHF5frXQi7eLGxC7uPQv5U5jl&id=100006735798590)

- This is fantastic: [Fixing the Portobello Police Station Clock](https://pointinthecloud.com/2026-04-11-211700.html) . It’s a hack, it’s nerdy, it’s real blogging. This is the type of stuff that made me fall in love with the Internet.

[![](https://substackcdn.com/image/fetch/$s_!8x--!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F074c8140-dde0-4f2f-aca2-121a9858b94d_961x37.tif)](https://substackcdn.com/image/fetch/$s_!8x--!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F074c8140-dde0-4f2f-aca2-121a9858b94d_961x37.tif)

No posts
