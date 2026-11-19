---
title: Pete warden on local AI, open models, voice interfaces, the future of hardware
link: https://thedataexchange.media/pete-warden-moonshine/
source: hnrss-org
published: 2026-11-19T15:38:12Z
updated: 2026-11-19T15:38:12Z
first_seen: 2026-11-19T23:46:32.431702950Z
authors:
- datascientist
labels:
- hardware
- local_edge_ai
- orm-pod
- uncategorized
content: extracted
html: 2026-11-19-pete-warden-on-local-ai-open-models-voice-interfaces-the.html
remote_preview:
  url: https://thedataexchange.media/wp-content/uploads/2026/11/pete-warden-local-ai.001.jpeg
---

**Subscribe:** [**Apple**](https://podcasts.apple.com/us/podcast/the-data-exchange-with-ben-lorica/id1487704458) **•** [**Spotify**](https://open.spotify.com/show/65oFWYoomwT8mLcdGkUGd0) **•**[**Overcast**](https://overcast.fm/itunes1487704458) **•** [**Pocket Casts**](https://pca.st/kv9n4vc0) **•** [**YouTube**](https://www.youtube.com/playlist?list=PLnTmH22EvTFTtWJRPTNzosDIDblnSg0PD) **•**  [**AntennaPod**](https://antennapod.org/deeplink/subscribe/?url=feeds.buzzsprout.com/682433.rss) **•** [**Podcast Addict**](https://podcastaddict.com/podcast/2476584) **•** [**Amazon**](https://music.amazon.com/podcasts/18e76845-60a8-4263-9c41-a7bdfe9d14a9/the-data-exchange-with-ben-lorica) **•**  [**RSS**](https://feeds.buzzsprout.com/682433.rss)**.**

[**Pete Warden**](https://petewarden.com/), co-creator of [**Moonshine Voice**](https://www.moonshine.ai/) and a longtime deep learning engineer, joins me to discuss why AI increasingly wants to run locally rather than in the cloud. We explore the rapid progress of open-weight models, what it takes to run AI on laptops and in browsers, and how low-latency speech models could make voice a more natural interface. We also discuss specialized versus end-to-end models, the economics of local AI, and emerging hardware approaches that could challenge today’s GPU-centric infrastructure. \[This episode originally aired on [**Generative AI in the Real World**](https://learning.oreilly.com/playlists/42123a72-1108-40f1-91c0-adbfb9f4983b/), a podcast series I’m hosting for O’Reilly.\]

***Subscribe to the [Gradient Flow Newsletter](https://gradientflow.substack.com/?utm_source=gradientflow&utm_medium=DEpodcast)***

**Interview highlights – key sections from the video version:**

- - 1.  [Local AI Is More Capable Than Most People Realize](https://youtu.be/HQp0t-vP9-I?t=42)
    2.  [Open Models, Quantization, and Running LLMs on Laptops](https://youtu.be/HQp0t-vP9-I?t=356)
    3.  [Can Local AI Replace Cloud Models for Knowledge Work?](https://youtu.be/HQp0t-vP9-I?t=626)
    4.  [Privacy, Personal AI, and Models Built Into Operating Systems](https://youtu.be/HQp0t-vP9-I?t=814)
    5.  [Voice as a Computing Interface Beyond Siri and Alexa](https://youtu.be/HQp0t-vP9-I?t=1003)
    6.  [Why Latency Matters for Voice AI and User Experience](https://youtu.be/HQp0t-vP9-I?t=1245)
    7.  [How Local AI Changes the Economics of Applications](https://youtu.be/HQp0t-vP9-I?t=1372)
    8.  [Designing Voice Interfaces for Failure and Human Feedback](https://youtu.be/HQp0t-vP9-I?t=1461)
    9.  [Compound AI Versus End-to-End Speech Models](https://youtu.be/HQp0t-vP9-I?t=1666)
    10. [Multimodal Models Versus Specialized Speech Models](https://youtu.be/HQp0t-vP9-I?t=1958)
    11. [Running AI Models Directly in the Browser](https://youtu.be/HQp0t-vP9-I?t=2136)
    12. [Why AI Wants to Be Local and Why Enterprises Need Control](https://youtu.be/HQp0t-vP9-I?t=2341)
    13. [Apple, NVIDIA, and Rethinking GPUs for AI Inference](https://youtu.be/HQp0t-vP9-I?t=2625)
    14. [High-Bandwidth Flash and the Future of AI Hardware](https://youtu.be/HQp0t-vP9-I?t=2764)

[Jump to transcript](https://thedataexchange.media/pete-warden-moonshine/#transcript)

* * *

[YouTube video player](https://www.youtube.com/watch?v=HQp0t-vP9-I)

* * *

**Related content**:

- A [video version of this conversation](https://youtu.be/HQp0t-vP9-I) is available [on our YouTube channel](https://www.youtube.com/c/GradientFlow?sub_confirmation=1).
- [Hacker News thread for this episode](https://news.ycombinator.com/item?id=50561508)
- [The local AI question I had backwards](https://gradientflow.substack.com/p/the-local-ai-question-i-had-backwards)
- [I changed my mind about world models](https://gradientflow.substack.com/p/this-robotics-result-surprised-me)
- [Previewing the Debate Over Frontier AI Regulation](https://gradientflow.com/proposals-to-regulate-frontier-ai-labs/)
- [Your Model Is Not Your Moat](https://gradientflow.substack.com/p/i-think-ai-teams-are-defending-the)
- **Richard Ho** →[Inside OpenAI’s First Chip](https://thedataexchange.media/richard-ho-openai/)
- **Joel Hron** →[Why Build Your Own AI Model When Frontier Models Already Exist?](https://thedataexchange.media/joel-hron-thomson-reuters/)
- **Pete Warden** →[TinyML, Sensor-Driven AI, and Advances in Large Language Models](https://thedataexchange.media/pete-warden-useful-sensors/)

* * *

**Support our work by subscribing to our newsletter📩**

* * *

### Transcript

Below is a **polished and edited transcript**.

**Ben Lorica.** All right. Today we have an old friend, Pete Warden of Useful Sensors, and their main website is actually at moonshine.ai. If you go to the website, you’ll see that they’re very much about voice on-device, and we will definitely talk to Pete about all things speech and all things edge computing. But I do want to take advantage of Pete being one of the OG deep learning engineers to get his sense of what’s going on these days. And with that, Pete, welcome back to the podcast.

**Pete Warden.** Thanks for having me, Ben.

**Ben Lorica.** All right. So I would say, Pete, I don’t know if this is a fair characterization, but one of the main thrusts of your work is the whole idea that local AI does not have to be compromised. In fact, it’s something that people should take more seriously. As I mentioned, we’ll definitely talk about your work on speech models on-device. But broadly speaking, Pete, for our audience that doesn’t really follow local models, where are we for someone who wants to run a model on a laptop, not a phone, just a laptop? What’s the state of ease of use? I guess one of the obstacles is that it’s always easier to use a cloud API, right? So where are we in terms of the tooling around local AI?

**Pete Warden.** Yeah. So at the moment, it’s like many open-source projects. Every year is going to be the year of the Linux desktop. I think there’s always going to be a bit of a lag in usability for the average user with this stuff, but I think there’s an even bigger lag in marketing because it’s very hard to make money off local models.

That means the big companies like Anthropic and OpenAI, and even the big tech companies generally, aren’t going to push that. If you give them a choice between a SaaS service that you can actually make money on and this other thing where you’re not even going to know that people are using it because they’re not contacting your service, every company is going to go for the one that has an easy subscription revenue model.

That means you have a massive amount of marketing around all of these tools that are basically a little text box on a website. And so it means that most people have never heard of these local models.

Now, if you use something like Ollama, which is probably the most well-known, I think people will be pleasantly surprised by how straightforward it is to use and by the quality of the models. I saw some news recently that Anthropic is actually a bit concerned because people are turning to more specialized models versus always going for the most sprawling, expensive model that’s available.

Local models are actually doing very well in the benchmarks. They continue to be only a few months behind the capabilities of the big commercial Anthropic and OpenAI models. But I also believe that local models have gotten to the point where they can do the things that people want to use them for.

What I’m really hoping is that it’s one of those situations, for those of us old enough to remember the dot-com boom, when people were paying thousands of dollars for SPARC servers or using Microsoft IIS and paying massive amounts of money because web servers were seen as such an arcane thing. Then, of course, Apache, Nginx, and all of these alternatives, once people actually had time to look around and the alternatives had a little bit of time to improve, just wiped the floor with the commercial alternatives.

I don’t know if we’re going to quite get there, but that’s the pattern I’m seeing.

**Ben Lorica.** Yeah, my sense is that open models, whatever your definition of open is, mainly open weights, I guess, will ultimately prevail. It just seems like that makes a lot more sense to me.

As you mentioned, in the early days of Linux versus Solaris, or even Postgres and MySQL versus Oracle, it was far from clear that these open-source projects would ever get to the point of us having to take them seriously. But ultimately, I think they’re such important parts of the stack that the open alternatives are what people will opt for.

Speaking of which, Pete, you mentioned Ollama, right? And obviously, there are other alternatives. Let’s say, for the sake of argument, that they are in fact easy to install and use. As far as the models are concerned, you have things like Gemma, I suppose, and other small models, right? What kind of hardware does someone need to have in order to run these models so that they don’t have to wait 20 minutes for each prompt?

**Pete Warden.** So a lot of it comes down to your model choice.

**Ben Lorica.** Yeah.

**Pete Warden.** You can get very capable models that are like 9 billion parameters these days. Especially if you look around at the Qwen family of models, they tend to be doing very well, though there are a bunch of other alternatives.

**Ben Lorica.** But translate that, Pete. To someone with a normal laptop, is 9 billion big or small?

**Pete Warden.** So the key thing is whether you can fit that into your graphics card’s memory. With weight quantization, 9 billion, if it was eight bits, is like nine gigabytes. A lot of decent midrange laptops that are shipping now have more than that.

But using Q4, and there are all sorts of schemes, you can actually get that down into a four-gigabyte graphics card, and that’s widely available. There are a lot of even older laptops that have that amount of GPU memory, VRAM, in them.

So really, the hardware barriers are a lot lower than people think. And the other piece of good news is that all of the hardware manufacturers are looking at this. They’re slightly hobbled by the DRAM craziness, the price increases. But if you look at the new M5 Studio from Apple, they have a configuration that’s 256 gigabytes of system memory.

Apple does a great job of actually making that available to the accelerators that they build into all of their Macs now, too. And with that, you’re looking at something like a model with hundreds of billions of parameters that you can actually run at a decent speed.

Once you get up into those numbers, you’re really looking at something where it can be hard to tell if you’re using Claude or if you’re using an open-weights model at that point. They have different characteristics, but one isn’t clearly worse than the other.

**Ben Lorica.** So as I mentioned, we’ll definitely dive deep into voice and speech in a second here. But sticking with this notion of local models, obviously privacy and IP security are arguments for it.

Do you think, if we look ahead, let’s say 12 to 24 months, these local models are going to be capable enough that people doing knowledge work in an enterprise might even consider them here and there?

For example, someone who travels a lot is on an airplane, not connected to the internet, and needs to keep writing a document or generating images or even coding, vibe coding, on a laptop. Do you think we’ll get to the point where, with these local models, you essentially feel like you’re not compromising when you’re doing your knowledge work?

**Pete Warden.** I think you can actually get to that point right now, and I know that’s a fairly bold statement.

**Ben Lorica.** But when you say you, does that mean Pete Warden, who’s going to compile all of these? We want normal people. We want civilians, Pete.

**Pete Warden.** I think the biggest barriers are usability. If you look at the benchmarks, it really is no longer a question of model quality.

**Ben Lorica.** Model quality.

**Pete Warden.** Yeah, a question of model quality. There are still things around the edges where you might prefer some of the commercial online models, but it’s not clear-cut.

And people are finding that some of the models actually work better, especially if you’re dealing with things like, I don’t want to call it censorship because I feel like that’s a loaded term, but a lot of commercial models have restrictions. They may be intended to prevent a bad use case, but pretty often they can also trip you up even if you’re trying to do something benign.

I know a lot of people who work in cybersecurity, for example, who are white hats, but they’re having to work with a lot of stuff where they’re asking questions about malicious actions. With the commercial models, you’re going to keep tripping over the restrictions and the guardrails that are built in with good intentions, but they can really impact your work.

**Ben Lorica.** Right, right, right. By the way, Pete, the model is one component, but then there are the things surrounding the model, the so-called harness, or even context, memory, knowledge.

You can imagine someone, and this is actually something I’ve thought about. You know how people are starting to use these LLMs as therapists or friends, right?

**Pete Warden.** Yes.

**Ben Lorica.** Then if you’re going to do that, why not do that on a local model? Why do it on something connected to the cloud?

I guess one argument against it is that if you do it locally, you’re responsible for model upgrades and things like that, right? Whereas if you use one of these digital or AI therapist services, they take care of all your history and all of this.

Do you think it will become easier for consumers to use local models that are essentially something they trust 100% because they own it?

**Pete Warden.** Yeah, I mean, these should be part of the OS. At this point, it’s really an operating system capability that should be available to apps, and I feel like Apple is moving in this direction.

**Ben Lorica.** I see. Yeah, that makes a lot of sense.

**Pete Warden.** Yeah, they’re getting the hardware capabilities.

The other thing I would say is that local doesn’t have to mean on your laptop. Especially as an enterprise user, you can host on-prem or even spin up a cloud instance that your enterprise has control over, run these models there, and have the same experience.

One thing I do want to emphasize is that people are finally having to pay something approaching the real cost of running these models through Anthropic and OpenAI, and hosting locally or hosting on-prem can cost a lot less, too.

**Ben Lorica.** Although if you look at their consumer offerings, it’s still insanely cheap, right? So $20 a month. I mean, you probably blow through that in a day.

**Pete Warden.** Yeah. Especially on the enterprise side, there are a bunch of options that don’t require high-end NVIDIA GPUs. You still need those for training, but for inference, for just running models, as an enterprise, you might already have the capacity lying around.

**Ben Lorica.** So another important aspect of what you’ve been doing is getting people excited about voice and speech, right? This notion that speech could become a basic computing primitive. It doesn’t necessarily have to feel like some remote service.

I still remember you demonstrating to me this voice transcription service in a loud coffee shop near Precita Park.

**Pete Warden.** Yes. Right. Yeah.

**Ben Lorica.** This was several years ago. The accuracy of this thing was insane, and it was running not even on a laptop. I think it was just running on a Raspberry Pi.

**Pete Warden.** That’s right. Yeah.

**Ben Lorica.** And so talk to us about the state of voice as an actual interface, similar to a mouse or a keyboard. Where are we?

**Pete Warden.** I think a lot of people are skeptical of voice because they’ve used Siri and Alexa, and it’s a very stilted way of communicating.

But the question I often ask people when I’m having a conversation about this stuff and I encounter that resistance is, when you’re sitting on the couch and you’re trying to decide what TV show to watch with your significant other next to you, do you text them or do you talk to them?

**Ben Lorica.** Right.

**Pete Warden.** Apart from a few Gen Z people, almost everybody says talk to them.

And so following on from that, it’s like, okay, well, it’s not that people don’t like voice interfaces. It’s that people don’t like bad voice interfaces.

So the problem becomes an engineering and user experience problem of how do we make voice interfaces that are pleasant and efficient to use?

I think once we do that, voice can really take its place as maybe even a more frequent input medium than the keyboard or touchscreen.

**Ben Lorica.** Yeah, because I think the general impression people have is that voice and speech are much less deterministic than a keyboard or a touchscreen, right?

Although people now interact with their LLMs and misspell things right and left, and it doesn’t seem to matter.

But the impression people have is that there’ll be problems with accents, background noise, phrasing, and so on and so forth. So where are we in terms of all of these things?

**Pete Warden.** We have solutions available to most of those problems, or at least big improvements. But because there’s no commercial incentive to pull them all together, it really hasn’t hit the mainstream yet.

I feel like we’re still dealing with interfaces that are from the previous generation. They haven’t taken on board the LLM era fully, and they’re still suffering from a lot of those problems you listed.

And that’s why I think open source can be a really strong lever.

**Ben Lorica.** So as you’ve been trying to solve a lot of these interface problems, what are some of the lessons that you’ve learned? In fact, maybe some of them might even apply to people who are not working with voice but are just building AI applications.

I think one of the things you mentioned to me years ago is that with voice, you don’t have to wait until someone has finished talking to start doing work, right? So what are some of these interface lessons?

**Pete Warden.** I think latency is a massive deal. That is something you just don’t get with cloud-based voice interfaces.

You’re usually waiting at least a couple of seconds, and that’s long enough for people to wonder if they were actually heard.

**Ben Lorica.** Yeah, it’s like being on a Zoom call or phone call with someone, and then there’s a pause.

**Pete Warden.** Yeah, and they’re just sitting there. It feels like the other person is just sitting there, ignoring you.

We know from general user experience research that you really want to be in that 200-millisecond or lower range to feel responsive.

I often ask people to imagine that you press a key on the keyboard and it takes two seconds for the key to show up in the window. That would be very awkward.

**Ben Lorica.** By the way, this is something that people who work with LLMs also have to deal with, but on the output side, right? They want to make sure that the response is not too long. Otherwise, people get impatient, right?

**Pete Warden.** Yeah, absolutely.

And I do want to mention another thing, which I think might be helpful to people thinking about local AI in general as well.

When you’re no longer paying per token, when you’re no longer having to pay to use a voice interface, the way you can use these things, the world of possibilities, becomes so much wider.

If you imagine that every app developer had to pay a few cents every time somebody touched an icon on the touchscreen in their app, you would design apps very differently, and a lot of apps wouldn’t exist because of that tax.

So once LLMs are built into the OS and just generally available with no cost, and the same with voice, that is not just an incremental change going from a cent to zero. That massively increases the number of viable things that you can use those for.

**Ben Lorica.** So I imagine, Pete, this is one thing where you’ve done a lot of work, which is basically designing for failure, right? Because in voice, a lot of things can go wrong.

What are some of the lessons there in terms of designing your applications with failure in mind?

**Pete Warden.** One of my favorite examples of designing for failure is the use of snippets in Google Search.

Google Search, from the beginning, knew they couldn’t always get the number one result right. There’s a reason you didn’t just hit “I’m Feeling Lucky.”

**Ben Lorica.** Yeah, exactly. “I’m Feeling Lucky.”

**Pete Warden.** What was genius about their approach was that they actually let you browse, just scan with your eyes.

They hoped to get the right result in the top three most of the time, and then you actually narrow it down by looking at the snippets, which tell you enough to click on the right one.

And the same thing for voice. The more you can actually read the body language of somebody, as we’re talking, you’re nodding a little bit to indicate what’s happening.

**Ben Lorica.** Wait a minute, I’m only voice, so you can’t see me.

**Pete Warden.** No, I can see.

**Ben Lorica.** No, no, but in general, right? In general.

**Pete Warden.** Well, that’s actually a whole other part of this that I haven’t mentioned.

I think having a little camera that’s completely local-only will be a massive part of this. But even without a camera, if you hear supportive noises, or if you start giving a response and somebody reacts, all of those things that we recognize as people, we respond to them and ask more questions versus assuming that we picked the right thing.

So really trying to understand all of those cues that we use as people. Some of them are verbal, some of them are body language and visual, but really having them all understood locally.

And ensuring that all of those recordings never get recorded, because you can verify that locally. You don’t know what happens to audio and video once you send it up to the cloud. It’s impossible to audit, basically.

With local stuff, you can audit. So I’m hoping that opens up the chance to use cameras without increasing the risk to users around privacy.

**Ben Lorica.** So there are the people who work on speech models, and there’s been a lot of work in that area.

In fact, one thing that I think is vastly underappreciated is that people talk about deep learning having an ImageNet moment, but actually the first impressive deep learning models were in speech, before ImageNet, right?

So there are people working on speech models. They’ve even talked about end-to-end models, but obviously we have LLMs.

For the people who don’t follow this closely, Pete, what’s the relationship between the LLMs and the speech models, or do people not use LLMs? Do they just use speech models?

**Pete Warden.** There’s been a massive debate, a split between what I think of as LLM maximalists, who expect everything to be an end-to-end model.

**Ben Lorica.** “Machines of Loving Grace.”

**Pete Warden.** Yes, exactly. And I see usually Anthropic, OpenAI, the big AI companies.

**Ben Lorica.** Or, you know, the hype machine has to be fed, right?

**Pete Warden.** Well, the hype machine has to be fed, and specifically, if the future is end-to-end models, then only the people with the most money can actually build and train them.

Now, I’m very strongly in what Berkeley calls the compound AI camp, where you actually build your applications out of multiple models, each of which has a specialized task.

So there, instead of having a model that takes speech in and outputs speech audio, you actually have a model that takes speech audio in, converts it to text, feeds that into an LLM, and the LLM then outputs text, which is converted into speech.

Now, every time you convert the audio into text, you actually lose a bunch of expressiveness. It’s like a bottleneck of information.

You can’t tell how somebody’s asking it, whether they’re asking it with a particular intonation and all of this other stuff.

**Ben Lorica.** Can’t you, as you’re doing that translation, add metadata?

**Pete Warden.** You kind of can, but the whole point of compound AI is that you can take models from different sources and plug them together. So you need some kind of common language, and at the moment, the only thing that all LLMs speak is text input and text output.

But the advantages are that you can actually train all of the models independently. With a speech end-to-end model, if you have a new accent, then if it’s a true end-to-end model, you have to do a whole bunch of training.

If you have a new input accent, if you have new output voices or new capabilities, if it’s a true end-to-end model, then you’re having to train the whole thing.

If it’s not actually, if end-to-end is just a marketing term, then you can train individual components, individual modules separately.

But then you’re always agreeing on some settled interface data, like some schema, some language. Even if it’s not text, even if it’s an embedding, you still have to have those fixed points.

So yeah, I see end-to-end as much more of a marketing term and something to justify the insane amounts of money that are flowing into these companies.

**Ben Lorica.** Pete, what about, for our listeners who’ve heard of the term multimodal, right? Gemini is supposed to be multimodal.

And coincidentally, I just spoke to someone who works in the visual reasoning and understanding space, and they said actually Gemini is quite poor in terms of understanding video.

So what is the state of these multimodal models for your domain, speech?

**Pete Warden.** They’re actually often pretty good. Qwen, 1.7 billion, I think it is, actually does a better job than Whisper, a significantly better job on some languages like Mandarin. And the same goes for Gemma.

The trouble is that, as general-purpose models, one of the failure cases with Gemma is that you upload audio of some speech, it transcribes it, but it then gets confused by the content of that speech and thinks you’re asking it a question, say, if there was a question in the transcribed speech.

So it’s one of these things where you’re trying to use a Swiss Army knife, and it’s a lot easier to use an actual wood saw versus the thing that can do everything and has a little saw attachment with it.

So that’s another drawback of the end-to-end models.

**Ben Lorica.** By the way, obviously you’re the king on this topic. General-purpose models are dead on arrival on the edge, right?

**Pete Warden.** I mean, right now, only…

**Ben Lorica.** For the types of, I mean, some of the really small pieces of hardware that you work on.

**Pete Warden.** One of the things I recently did was get a full voice interface where you could connect to Wi-Fi, set the password, and all of this sort of stuff, running on a Raspberry Pi Pico microcontroller that only has 500 kilobytes of RAM.

Now, there, you’re never going to have a general-purpose model that’s going to work well for you. But as always…

**Ben Lorica.** And by the way, one of the things that you’ve also worked on, which is interesting, and which I don’t see a lot of people working on, is taking the browser more seriously, right?

So tell us, what’s the motivation behind that, and what has been the reception?

**Pete Warden.** Yeah, so it’s tricky because I sometimes think of “We’ve done X, but in the browser” as Hacker News bait, because that always gets a lot of attention as a novelty. But it’s often hard to find practical uses for a lot of these things.

With the voice capabilities, with voice input and voice output, what I’ve seen is that it makes it part of the browser operating system, or feel like part of the browser operating system.

And once you’re able to transcribe fast and accurately in the browser, it’s a way for people to easily start experimenting with this stuff, and there aren’t any costs for this at the moment.

I’ve tried to put a few examples. If you go to Moonshine AI…

**Ben Lorica.** But the whole notion of using the browser as a point of inference, right? You can imagine other models doing that too, not just yours, which focuses on speech.

You can imagine text, I mean, browser inference for text kinds of applications, right?

**Pete Warden.** Well, if you’re running Chrome, you’ve already got a very capable model installed. You’ve got a 4-billion-parameter model.

I actually did a blog post last year showing how you could just go through Chrome settings and start using it as a chatbot, just typing in the window and getting responses.

I think that’s an example of the models becoming part of the operating system, because Chrome is essentially an operating system that sits on top of other operating systems.

You can already access that built-in LLM, I think now from any website. It used to be just extensions, but now I think any website can access that from JavaScript.

And again, we’re still figuring out what the best use cases are for that. But once it becomes free and you can rely on it being everywhere, I really feel like it’s an iPhone moment for a lot of these new LLM technologies.

**Ben Lorica.** Or even what you mentioned earlier, which is for people on a laptop, it’ll be part of the OS somehow, right?

So looking ahead, Pete, and again, for our listeners, you can follow Pete’s work at Moonshot AI. Moonshine. Ah, Moonshine. Moonshine.ai. And a lot of the work is open source, and you can take advantage of that.

So looking ahead, Pete, 12 to 24 months. I hate to put you on the spot. Where are we in terms of, I guess, local or edge models, number one, and number two, just the state of open models in general?

**Pete Warden.** Yeah, and I’m always terrible at predictions.

**Ben Lorica.** But one thing Pete is super good at, for our listeners, is knowing what to work on. So that’s an indication. Just tell us what you’re working on.

**Pete Warden.** Yes. So what I see from being deep in the technical side, most of what I end up working on is because I’ve been working on something, and it becomes blindingly obvious that the only reasonable technical solution is X, and so of course I want to work on X.

And what I see, and what I’ve seen for the last few years, is the gravitational pull of running this stuff locally or on-prem, or however you want to call it.

I often say, “AI wants to be local.”

If you think about something like Google Search, you need massive databases, you need armies of servers, you need all of this stuff. If you look at what Claude and ChatGPT are doing, you can run that entirely on one laptop with no internet connectivity.

So the shape of these models is perfect for running locally, unlike almost everything else we’ve seen over the last couple of decades, where the cloud has made more sense.

So what I expect is all of these pressures: cost for enterprises especially, worries about privacy, worries about capabilities going away, worries about changes.

If you built an application on top of a particular version of Claude, even if you’re using the same version in the dropdown, what’s actually happening behind the scenes can change from day to day.

So if you want stability, you actually need to host your own, at least in your own cloud infrastructure, if not locally on users’ laptops.

And I can keep going on and on.

**Ben Lorica.** Yeah. And then the other thing, too, is I think a lot of enterprises are recognizing that the moat, or their moat, is really not the model. It’s that compounding loop around the model, right?

As you use the model more, you learn more and things like that. So why in the world would you send that to a commercial proprietary model?

**Pete Warden.** And why not do fine-tuning on your own model?

**Ben Lorica.** Yeah, I mean, with that data.

**Pete Warden.** Yeah, exactly.

**Ben Lorica.** And then, as you point out, you actually control your timing and roadmap as well, because you have the model and you know when it’s safe to replace that model with something else.

No one is going to replace that model underneath you, right?

**Pete Warden.** Nobody’s going to start charging 10 times as much overnight.

**Ben Lorica.** Right, right, right. And then also, even if they keep the cost the same, they may weaken the model for some reason, right?

**Pete Warden.** Yep. Yeah, and it could even be pressure from outside.

**Ben Lorica.** So by the way, one last question for you, Pete, because obviously you work in a space where you really care about hardware, right?

Right now, we’re in a world where Jensen Huang seems to own everything. So what’s your prediction about the state of AI hardware?

Are we going to get more diversification? Is AMD going to up its game, or what’s going on in hardware in general?

**Pete Warden.** So watch Apple.

And disclaimer, I worked at Apple for five years in the 2000s. My wife worked there for 13 years.

But if you look at what they’re doing, they’re getting lots of RAM, increasing the RAM speed so that you can actually run models much faster, and they have their own inference-only accelerators.

I think with those three, you don’t need a GPU.

**Ben Lorica.** So you just run everything on a CPU?

**Pete Warden.** Well, run everything on a different kind of accelerator.

**Ben Lorica.** Oh, okay. That’s not a…

**Pete Warden.** GPU.

**Ben Lorica.** I see. I see. So CPU…

**Pete Warden.** Could actually be pretty fast. Obviously, the…

**Ben Lorica.** The NVIDIA answer is why we have all the HBM, high-bandwidth memory, on the GPU, right?

**Pete Warden.** And I like to think of GPUs as being designed for large batches and training.

I like to think of a GPU as trying to use an oil tanker to go and do your shopping.

They’re fantastic for these large training jobs, but they’re complete overkill for inference. Inference has a very different shape to the workload that it places on hardware.

**Ben Lorica.** One thing that I’ve started to pay attention to lately, and it’s very, very early, is high-bandwidth flash.

**Pete Warden.** Absolutely.

**Ben Lorica.** Right. So what’s your take?

**Pete Warden.** So that’s another example.

I see the massive amount of money that NVIDIA is pulling in, and that also has its own gravitational pull on all of these hardware companies, where they’re looking from a technical side.

I don’t have any special insights. I’ve just worked with these things. I know what the workloads look like. Other people are looking at those too, and they’re like, “Oh, I see. We have to stream all of the weights. All of the weights have to be processed for every token.”

So if you have something, even if you’re running on the CPU, if you have something that’s got high enough bandwidth to pull the 27 billion weights in a fraction of a second, then the rest of it is fairly easy in terms of actually doing the processing. It’s very much memory-bound, usually.

So it’s been awesome seeing high-bandwidth flash, what Apple is doing, increasing the bandwidth on regular RAM.

I think we’re going to see a lot of really imaginative solutions now that people understand what the workload looks like.

**Ben Lorica.** Yeah, and with that, thank you, Pete. And again, the website is moonshine.ai.
