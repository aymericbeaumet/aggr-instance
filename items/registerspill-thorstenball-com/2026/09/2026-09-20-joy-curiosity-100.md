---
title: 'Joy & Curiosity #100'
link: https://registerspill.thorstenball.com/p/joy-and-curiosity-100
source: registerspill-thorstenball-com
published: 2026-09-20T06:23:08Z
updated: 2026-09-20T06:23:08Z
first_seen: 2026-09-20T09:45:30.023743147Z
authors:
- Thorsten Ball
summary: Interesting & joyful things from the previous week
content: extracted
html: 2026-09-20-joy-curiosity-100.html
preview:
  file: 2026-09-20-joy-curiosity-100.preview-a2dd3d0c24f4.webp
  width: 256
  height: 184
  color: '#3a3452'
images:
- source: https://substack-post-media.s3.amazonaws.com/public/images/041e8138-7f0d-4bab-9df6-a0f42d7d6c69_1554x1114.png
  original:
    file: 2026-09-20-joy-curiosity-100.image-5b6b03d86076.png
    width: 1554
    height: 1114
  color: '#0e1415'
- source: https://substackcdn.com/image/fetch/$s_!ZkfB!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F880e8009-2a8b-497c-81c1-7ecce11895d6_954x33.tif
  original:
    file: 2026-09-20-joy-curiosity-100.image-ebb2fb04b790.png
    width: 954
    height: 33
  variants:
  - file: 2026-09-20-joy-curiosity-100.image-625194d301a2.webp
    width: 320
    height: 11
  - file: 2026-09-20-joy-curiosity-100.image-f97e644ce6fe.webp
    width: 640
    height: 22
  - file: 2026-09-20-joy-curiosity-100.image-e2d297682426.webp
    width: 954
    height: 33
  color: '#000000'
- source: https://substackcdn.com/image/fetch/$s_!N37b!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb4fbfc70-8ff4-4fae-8b8d-a318a9183380_960x43.tif
  original:
    file: 2026-09-20-joy-curiosity-100.image-dd576b549572.png
    width: 960
    height: 43
  variants:
  - file: 2026-09-20-joy-curiosity-100.image-93a526cb6ead.webp
    width: 320
    height: 14
  - file: 2026-09-20-joy-curiosity-100.image-fffdabd92be0.webp
    width: 640
    height: 29
  - file: 2026-09-20-joy-curiosity-100.image-b860055ccc04.webp
    width: 960
    height: 43
  color: '#000000'
---

It’s the week of [Jev](https://typesafe.ai/blog/introducing-system-one-models-and-jev) ! I’m really, really, really, really excited about it. I mean: really.

It’s like someone blew up a confetti bomb in the world of LLMs and now you realize how grey everything looked before.

But Jev is *not* an LLM. It’s a model “built to make fast, structured decisions that software can use directly.” TypeSafe says we should think of Jev “as a frontier-intelligence function call: unstructured state in, typed probabilistic decisions out.”

I explained it as a “smart if-statement” to someone and my [only slightly longer explanation is this](https://x.com/seejayhess/status/2101013312741777877) :

> Think of how you’d get an LLM to decide between a fixed set of options.
>
> Then imagine it orders of magnitude faster and cheaper.
>
> “What’s the best label for this?”
>
> “Should I click here or there?”
>
> “What’s the next line I should look at?”
>
> “Do I go left or right?”
>
> “Invalid or valid?”
>
> “Which of these widgets should I show?”

I had Amp build a little Copilot-style autocomplete for a shell, with [Jev picking the next most likely command from shell history](https://x.com/thorstenball/status/2100858434904109099) . Then Amp built a Neovim plugin (and called it hunch.nvim, which is a *great* name) that [uses Jev to predict the line you next most likely want to jump to](https://x.com/thorstenball/status/2101215311953313815) . Now, let’s linger on this a bit.

Two years ago, that was what Cursor was famous for. Yes, Cursor did and does more than that and the quality isn’t close, but… when we were working on Zed’s Edit Predictions we had to fine-tune a model to get into the same league! Now it’s a single API call and the latency is 200ms. That is incredible!

Then I built a prototype that uses [Jev to turn the Amp Dial](https://x.com/thorstenball/status/2101266587332317308) , switching between models based on your prompt.

Yes, all of this was possible before, but it’s so fast and so cheap that I still can’t believe it.

Sometimes a change in cost and performance is what creates a whole new category of technology. In my room, there are lightbulbs that contain computers, that can talk over a local network with me. Yes, we had computers in homes in the 70s and 80s, but no one would’ve ever thought that we’d have so many computers that are so tiny and cheap that we’d put them in freaking lightbulbs.

That’s what makes me so excited about Jev. It feels like we now have a truly smart Lego brick that we can use *everywhere* . Fun times.

[![](https://substackcdn.com/image/fetch/$s_!ZkfB!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F880e8009-2a8b-497c-81c1-7ecce11895d6_954x33.tif)](https://substackcdn.com/image/fetch/$s_!ZkfB!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F880e8009-2a8b-497c-81c1-7ecce11895d6_954x33.tif)

- [What I believe about the future of software development.](https://thorstenball.com/blog/2026/09/19/what-i-believe-about-the-future-of-software-development/) I posted this originally on X, saying that m ost predictions I see are still way too conservative, and [it completely blew up](https://x.com/thorstenball/status/2101305394190557466) .

- “ [I don’t like passkeys](https://hawksley.dev/blog/i-dont-like-passkeys) ”. Passkeys are such a weird technology. I can see how they’re technically brilliant and solve a lot of issues, but it does feel like Google and Apple and 1Password invited The Guy Who Invented Cookie Banners and said: what would *you* do, how would you roll this out?

- Colossus published [a very long Mark Zuckerberg profile](https://colossus.com/article/mark-zuckerberg-profile/) . Fascinating read. It’s very well written and somehow managed to make me think thoughts about Zuckerberg that I haven’t thought before, which is quite the feat, considering that we’ve all been aware of Zuckerberg for, what, nearly twenty years now?

- [Einride and Lidl Launch First Autonomous Cab-less Truck on German Public Road](https://www.einride.tech/press/einride-and-lidl-launch-first-autonomous-cab-less-truck-on-german-public-road) . As an Aldi man myself, let me say: hell yeah, let’s go, Lidl!

- [How To Write With An LLM.](https://sockpuppet.org/blog/2026/09/17/how-to-write-with-an-llm/) I like this! I still don’t know how to use LLMs for writing, because I never want them to write something for me and even seeing how they would write it seems to poison my brain. I should probably add an “only tell me what to change and why, but never ever show me how you’d write it” to my system prompts.

- [Marc Brooker, Distinguished Engineer at AWS](https://x.com/marcjbrooker/status/2101005954708021604) : “I believe that, long-term, humans have no role in routinely reviewing code. \[…\] The idea that humans will reliably look through code to find the increasingly rare issues that automated tools miss seems like a fantasy.” Yep.

- I wanted to link to [Powermove](https://trypowermove.com/) here and say “look, editable software! It’s happening! Jellyware!” but now realize that it’s not quite that yet. It’s a video editor with an agent inside, but it doesn’t seem like you can edit the video editor itself. That’s coming, though.

- [We are all Product Engineers now](https://seldo.com/posts/we-are-all-product-engineers-now/) : “The cost of writing code collapsed, and the cost of reviewing, fixing and operating it is following, and I’m assuming it gets there. What’s left of making software is finding out what people actually want, defining it precisely, and making it pleasant to use. That cost is per piece of software and doesn’t transfer, so as the amount of software goes to infinity, which it will because there’s no ceiling on demand, that cost becomes the whole job. That job is called a product engineer.” Obviously agree, but what I didn’t know about was Google’s APM program: “Formalized training of product people barely exists. Google’s APM program, which Marissa Mayer started in 2002 and which is the template everyone copies, takes about fifty people a year out of something like twelve thousand applicants.” Would love to read more about it.

- “ I asked Astra to create [an interactive aquarium wallpaper for my Mac.](https://x.com/chaseleantj/status/2100663203076128908) The fish respond to the cursor!” [Beautiful!](https://github.com/chaseleantj/desktop-habitats)

- John Gruber, Daring Fireball, with [Thoughts and Observations on Apple’s ‘Surprise and Shine’ Event; the Announcements of the iPhones 18 Pro, AirPods 5, Apple Watches Series 12 and Ultra 4, and the iPhone Duo; and the Dawn of the Ternus, John Ternus Era at Apple](https://daringfireball.net/2026/09/thoughts_and_observations_on_apples_surprise_and_shine_event) . Yes, that’s the title. The whole thing is Peak Gruber, I love it. What a writer. Now, I really do enjoy his words and sentences, but let me also use this occasion to say how much I admire him as a Pedantic Punctuation Pro: the numbered lists vs. the bulleted lists, the space between the numbers and the colon in aspect ratios, using × in display resolutions, … You could show me this sentence without any other context and I’d say it was written by Gruber: “The original iPhone (2007) display was precisely 3 : 2 (480 × 320 pixels, and let’s call it 1.5 : 1 for comparison’s sake to the following ratios), and this remained true through the iPhone 4 and 4S (960 × 640 pixels, 2× retina).”

- This was a very entertaining and fascinating read: [why I can’t stop thinking about Papua New Guinea and what I think everyone should know about it](https://notnottalmud.substack.com/p/why-i-cant-stop-thinking-about-papua) . I’ve become somewhat of a Papua New Guinea Head myself (that’s what they call us (no, they don’t)), after reading this piece,[They Burn Witches Here](https://highline.huffingtonpost.com/articles/en/they-burn-witches-here/) , nearly a decade ago. I couldn’t shut up about it at work. For two weeks straight: “Dude, did you know that in Papua New Guinea…” Until one day a colleague said: “Yeah, I did know.” Turns out that colleague, Nick Skelton, was a tour guide in PNG (as we call it) and [even wrote a book about it](https://www.goodreads.com/en/book/show/36554620-two-months-a-savage) , which I immediately ordered and read.

- [Moats & the Barbell-ification of Software](https://x.com/mvernal/status/2099885132379500562) : “Long term, I think the evolution of the software industry might mirror what happened to newspapers in the 1990s. There will be a smaller number of very large software companies. \[…\] I also think there will be one large software company by industry (e.g., Legal, Finance, Medicine) \[…\] I think most mid-sized point solutions will likely be consolidated or die off. The optimal strategy for the winner will be to do it all. \[…\] Lastly, I think there will be an explosion of “small” software. Most of this will be people building software for themselves or their own companies, but I think there might also be an explosion of small software businesses that make niche software, similar to the D2C explosion of the 2010s (powered by Shopify and Meta Ads).”

- [AI-generated posters don’t have to be horrible.](https://john.hartnup.uk/2026/06/07/ai-event-posters.html) Yes! Exactly! Now, read this, and then imagine you’re a person who can come up with all these styles without having to ask ChatGPT first. And then, on top of that, imagine that the very same person also knows something about music, and literature, and politics. Imagine how they could combine what they know and mix and remix. *That* , I think, will be valuable in the future.

- [Window Sweaters](https://github.com/saragordic/window-sweaters) : “A little Mac app I made to give my windows sweaters. 🧶 Knitted borders, colours inspired by your favourite apps, and a cosier desktop.”

- This is [one of the funniest tweets of all time](https://x.com/GraniteDhuine/status/2100514962376454608?s=20) .

[![](https://substackcdn.com/image/fetch/$s_!N37b!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb4fbfc70-8ff4-4fae-8b8d-a318a9183380_960x43.tif)](https://substackcdn.com/image/fetch/$s_!N37b!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb4fbfc70-8ff4-4fae-8b8d-a318a9183380_960x43.tif)

No posts
