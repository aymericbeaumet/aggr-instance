---
title: Living in the Weimar era of Ruby
link: https://blog.julik.nl/2026/10/the-weimar-of-ruby
source: lobste-rs
published: 2026-10-12T10:59:26Z
updated: 2026-10-12T10:59:26Z
first_seen: 2026-10-17T13:09:10.048459971Z
authors:
- blog.julik.nl by julik
labels:
- ruby
- vibecoding
summary: Comments
content: extracted
html: 2026-10-12-living-in-the-weimar-era-of-ruby.html
---

Much has been said about the DHH keynote that he gave at RailsWorld. To me, it was the voice of a person in an existential crisis, but also a very jaded one. “Look how I don’t care anymore, and you should not care either” is not a great message when you are *the* headline speaker at the conference - let’s call it CareConf - that you have, yourself, lobbied for.

The message from the supreme leader was clear: we vibe-rewrote HEY in a more performant way, it worked great, we neither need nor care about Rails anymore. I honestly believe that, instead of amassing even more applause via vitriolic truth-bombing, he could have just left the toy he no longer loves - including its community - in peace.

But here is the thing: the rich and famous losing interest in the good stuff does not oblige the rest of us to. If anything, it frees up the room.

🧭 I am currently available for contract work. [Hire me](https://stanquette.nl/hire) to help make your Rails app better!

I must admit that for a few days I felt very depressed about it all. Not about “AIs writing all the code” - that much is clear to me, and I do not lament the changing landscape of the *craft* - I do lament the landscape of *usage*, which is a topic for another article. It is abundantly clear that the LLMs are here to stay, that typing every single line by hand is no longer sustainable, and that the scales have shifted profoundly. What made me depressed was just how little care there was about the Ruby ecosystem and community, and the disdain with which David flushed the entire Rails and Ruby culture down the drain for the sake of “running it on a single Raspberry Pi”, as he put it.

What I am actually observing is something different: a renaissance, even though quite a few prominent Rubyists have moved on to other stacks - which are now much more open to exploration, as you don’t have to write all that… gunk - let’s call it what it is - by hand. A detailed unpack of 7 little-endian integers is not particularly interesting, nor artistic - it’s just gunk, and I won’t miss it. Unless struct packing is needed to make it fast, but then we can always art-direct additionally.

There was an interesting time in German history, right between the two World Wars - the “interbellum”, the time of the Weimar Republic. It was very turbulent, and a lot of it wasn’t pretty in the slightest - social difficulties, unemployment, incredible inflation, squalid living conditions, the emergence of the wildest movements, cabarets and cults imaginable… but there was also something else. The “Roaring Twenties” of Weimar Germany also produced an incredible number of great, whimsical, fascinating artistic movements. From the Bauhaus to the Berlin gay cabarets, from Kurt Weill to Fritz Lang’s *Metropolis* - it was a booming time, and a lot of what appeared then is still valued and cherished today.

And it was a technological boom as well - the ever more widespread use of electricity, the large-scale deployment of modern pharmaceuticals, the subways, the automobile… fascinating times.

I know how that era ended, and no - that is not the part of the analogy I am after. The part I am after is this: the old order had lost interest, nobody was in charge, the ground was shifting under everybody’s feet - and *that* is precisely when the most interesting work got made. Great turmoil, great industrial change, no adult supervision. Sound familiar?

And the Ruby community has always been artistic, as early as the work of \_why. Aesthetics were, after all, what drew a lot of people into Rails. The fact that you didn’t need the parentheses. The fact that thingswerenotwritteninoneword as if invented by a Dutchman, that there was no “this is an attribute and this is a method” confusion… a lot of things about the language were just aesthetically brilliant.

And with the abundance of new tooling, the right mindset to look at the state of the ecosystem is this:

> You are right but it also feels short-sighted? You have the most powerful tool you ever had, you have become a 1000x maker, and you can't think of how to make your stack 10x better?
>
> Yes, Rust is fast, the type system catches bugs, but there is so much beyond it.
>
> \* Why not make Ruby 10x faster?\
> \* Why not make Ruby (and therefore Rails) really great at concurrency?\
> \* Why not add a banging type system to Ruby that can provide guarantees beyond Rust?\
> \* Why not embed SQLite3 into Rails with a replication protocol so you can really compress the stack beyond a single server?\
> \* How to preemptively fix most bugs from the most critical parts of Rails, such as database pools?
>
> Yes, who cares about ActiveSearch, but there are so many ways we can fundamentally improve our stacks that go beyond asking the agent to just crap out code. Phoenix is still the only framework that ships with presence, because it turns out it is deceptively a hard problem to solve correctly, but somehow all issues are solved?
>
> I understand some folks have no interest in doing any of the above, and that's fine, but then at least pass the baton to the people who want to do exactly that.
>
> PS: I say all of this with love ❤️
>
> [September 26, 2026](https://x.com/josevalim/status/2103751481757216781)

Why, of all the things we now *can do* in Ruby, do we choose to do Rust vibe-rewrites? Of course, the story goes, all of those apps we have will just magically transform into accelerated, opaque binary blobs of SIMD goodness - with the LLMs translating our every decision, explicit or implicit, with the rigours of comparison testing (or by reading tea leaves matrix coefficients). Why even spend precious tokens on improving the gardens we already live in if you can raze them to the ground and replace them with glorious concrete?

## But of course nobody ever does that

And, luckily, the razing and replacement is not the only thing that is happening. Otherwise the story would be all too sad.

See, there was a sizeable number of problems with the Ruby runtime and the “scene” that were, technically, just “too much”.

- It would be a pain to generate a PDF with layout - there were no libraries
- You could no longer build native GUI applications - there were no libraries
- You could not easily produce native gems - unless you were prepared to spell out the hundreds of `ubf_unblock` and `INT2FIX` with exquisite precision…

All of these were *shitty* problems. They were just too much for even a team of 3 to take on, and sometimes even more people would not have been enough. The barrier to getting them into applications - even if we had them - was high, as release cycles on Rails apps were not that quick at most companies. And a lot of people would just throw up their hands in desperation and utter The Spell Of Dread:

> Nobody does this sort of thing with Ruby anyway.

The playbook of the incantation of The Spell Of Dread is, as we all remember, that right after we chant “Rails developers are hard to hire”, whereupon all the people hating Ruby get to do a little dance confirming them to be right.

But right now, the equation has changed dramatically. Let me give you a simple example.

## One does not simply generate an invoice

One of the typically shitty problems with Ruby has been PDF generation. As recently as 18 months ago, a Rubyist willing to produce PDF files from their Rails app would be facing a choice of a number of options, all equally unappealing:

- Run [Gotenberg](https://gotenberg.dev) to have a Chromium render the HTML, then transform it into PDF
- Use [Ferrum](https://github.com/rubycdp/ferrum) to spin up a local Chromium and do the same
- Use [Prawn](https://github.com/prawnpdf/prawn) and fight its nearly per-character manual coordinate placement
- Use [wkhtmltopdf](https://github.com/wkhtmltopdf/wkhtmltopdf), which was abandoned nearly four years ago, and wrestle with building it and its requisite dependencies
- Use [hexapdf](https://hexapdf.gettalong.org) and be prepared to license it commercially due to the rules of the AGPL

Prawn was great back in the day, but very challenging - primarily because it mandates nearly full *manual* layout, without any proper metrics or composer features. A *composer* is the engine that helps you place and balance typeset characters so that they fit into a pre-sized block. Sometimes, you also want to adjust that block so that your layout looks better - something that CSS actually excels at. And you want hyphenation and SVG output too…

While Cursor and other “first wave” AI tools made wrestling Prawn somewhat more pleasant, it was still a long way from “easy” - in all aspects.

That’s the very definition of “doing X sucks in Ruby”, and it would be a fair argument to not use the platform at all - for some. Or for running extra microservices (or gigaservices, as any Docker image with Chromium inside should rather be named, given its size).

Now - fast forward to this September. In the span of just a few months we have not one, but two *excellent* libraries for generating PDFs.

[sghtmltopdf](https://github.com/waka/sghtmltopdf) by Waka creatively reimagines wkhtmltopdf by replacing its WebKit heart with a vibed Rust-based layout engine built on [Servo](https://servo.org) crates, which supports a significant subset of CSS layout directives - and a tasty dose of [Typst](https://github.com/typst/typst)-originating Rust libraries for font handling, metrics, line composition… I was able to move my entire invoice generation to it from Ferrum in about an hour. Initially it had some rough edges, but it took the community about a week to sand them away, and it became great. Not “manual certificate layout of 3 text blocks” great, but a genuine “6 page invoice with tables, blocks, and vector graphics” great, with the generation of a document taking double-digit milliseconds.

[stationery](https://github.com/zoolutions/stationery) by Mikael Henriksson takes a different approach - one closer to Prawn, with a healthy dose of [Phlex](https://www.phlex.fun) thrown in. It readily accepts the fact that a PDF is just… text - and, therefore, should be unashamedly output from any tool that is able to write a text file somewhere. It adapts Phlex-y view components and blocks, wraps them with some delightful layout primitives - and the result is an even more Ruby-spirited way to output PDFs, with performance comparable to the Rust motor of sghtmltopdf.

I was able to retool to both in a very short timespan, and still have a bit of difficulty picking one. We went from “absolute wasteland with all available options being very bad” to “you can pick between two great options”, very quickly.

## We do things because we want to

And it does not stop at PDFs. There are Qt bindings in development again - decent, *big* Qt bindings. Something that was horrible in Ruby for as long as I remember it existing. Schwad has resurrected Shoes in the form of [Scarpe](https://github.com/scarpe-team/scarpe) - not as a bleak Java jar requiring SWT, JRuby and the kitchen sink but as a proper graphics backend driven from Ruby scripts, all with native graphics. RMagick is having regular releases again and working beautifully. The [GD bindings](https://github.com/ggerman/ruby-libgd) are being worked on.

On my end - I have been confronted with some sizeable ETL projects that involved Rails, and a pain point I would always stumble into was Parquet. It was just not possible to output it easily from a Rails app - we always had to either downgrade to CSVs, or rig some weird CDC pipeline that would bypass the app altogether, just for the sake of producing a data lake. My opinion at the time was that Parquet is grossly overengineered, while you can do CSV with a blunt fork. And that it was a pain to work with in any environment which was not 100% Apache-big-data-culture native (read: not C++, Java or Python).

And that irked me. No, it can’t be that “our little toy language is just too slow and too small and who cares about it anyway” - Parquet files are just bytes. Another homegrown swizzled-and-zigzagged byte encoding convention, struct repeats, pluggable compression modules and native-width types, plus some facilities for encoding… it’s just *convoluted.* It is not so special as to require native code, and not so big as to require a whole *shelf of JARs* - it was just too big of a bother to address. The few libraries that did exist handled it through linking with `GObject` and who knows what else. And that desert lasted for more than a decade. Now I was able to build a full Parquet library - [herringbone](https://github.com/julik/herringbone) - in the span of a few days. Something that used to be a drag of a problem with Rails ETL is just a non-issue - there is a Ruby library that is convenient for both humans and agents, it streams, it uses native compression modules where appropriate, and it requires minimal user code - even with some fairly advanced things like Parquet encryption.

No, you can’t really one-shot such a library yet. You do need to know what you want, and you do need to iterate so that it becomes polished and fast. Or, to say it differently: you will one-shot it, but the result won’t be aesthetically good. Ruby stuff has to look (and feel) good. Now I no longer have to spend days figuring out the intricacies of Thrift bit packing (I don’t miss that crap either) - I can design the API top-down and guide the LLM through making it.

Previously, producing a library like this would have taken me a few months. Producing [zip\_kit](https://github.com/julik/zip_kit) took 3 months (amortized over a decade, with 2 months concentrated around its inception), and the Parquet spec is 5x the size of the ZIP APPNOTE document. If ZIP is an architectural alco-trip, Parquet is a fear-and-loathing type architecture astronautics adventure.

That’s not to mention the 6 or so micro-libraries I had to knock out just for calling some cloud services that didn’t bother providing Ruby SDKs. Or at least 2 feature flag service libraries that I know are both of questionable quality and/or packaging, and could very well use some reversing and redoing, in style.

## We do things because we need to

Another characteristic pain point with Ruby and Rails apps has been gRPC. I have yet to see a Rubyist who had to use gRPC in anger and had 0 issues with it. Hanging threads, fork unfriendliness, weird protobuf compatibility issues… the list goes on and on. Now Samuel just goes and [redoes an Async-friendly gRPC](https://github.com/socketry/async-grpc) - because we are no longer bound by the constraint of “big team at X is maintaining this thing and they got reorged for the 6th time this year, and nobody would ever be able to figure it out”. We can front-load the damn thing into the LLM wood chipper and have it help us figure it out, and then do it the way it should have been done from the beginning.

Protobufs, which are the foundation of gRPC, are also not quite a walk in the park in the Ruby world. It seems very simple, *mundane* even - how hard can it be to `pack` a few bytes into a binary string?

Turns out - you can make it as hard as the headcount of staff/principal engineers across your teams permits. The official Ruby protobuf gem requires native code. Every proto message is a handle to a native object, which cannot be marshalled or dispatched to another process easily. Introspection is limited and has a weird API. You need a separate package (`protoc`, the protobuf compiler) just to convert a `.proto` file into a Ruby file with some boilerplate in it denoting the fields you carry. It is, honestly, an utter overkill of a juggernaut for what amounts to a `Marshal.dump` - with a whole heap of native, compiled code to drag along.

And native code requires compilation. And ABI-compatible prebuilt gems for several platforms × versions of Ruby you need to support. And the recommended Ruby C API changes from version to version. And it hampers GC unless you are very, very careful, and requires meticulous GVL management to be performant…

Should we just cope with this and see this as normal? That’s the next target. If you want it too - get in touch, I might have something for you to play with.

## Damn lies, but statistics

Anecdotes are nice, but I wanted to check whether what I am seeing is just a bubble of my own timeline. So I went to the [rubygems dataset that ClickHouse hosts](https://clickhouse.com/blog/announcing-ruby-gem-analytics-powered-by-clickhouse) and counted something very dumb: how many gem *names* got occupied for the first time in the last twelve months, versus the twelve months before that. Not downloads, not versions - just “a name that did not exist on rubygems.org, and now does”.

The raw number is nonsense - it includes a few thousand yanked typosquats and a handful of accounts emitting a gem per API endpoint - so out went everything that is not indexed, and out went every prefix family of 20+ gems coming from three or fewer accounts. What is left is the boring, organic, one-off stuff - `sidekiq-buffered`, `schema_reaper`, `k8s-rails`, `rspec-hopper`. Of that there were about 4,600 in the year to October 2025, and about 8,900 in the year since: *1.9 times more.* The line sat at roughly 400 to 500 a month through all of 2025 and then jumped to 800 to 1,600 a month from March 2026 onwards. It is not a slope, it is a step.

That is the abundance of software everyone keeps talking about, and it has reached our toy little language too. I can’t tell you whether these are new people or the same people on a hot streak - the data doesn’t know, and neither do I - but twice as often, the thing someone needed got packaged and published rather than shrugged off with The Spell Of Dread. Make of the quality what you will - some of it is garbage, some of it is `herringbone`, and I will let you decide which is which - but the decision to *build the thing at all* is being made twice as often as a year ago. That is the renaissance I am talking about.

## No excuses

And it is not only libraries. Performance is a superb topic to tackle. The work [Sam Ruby is doing](https://github.com/rubys/roundhouse) on his Roundhouse cross-compiler is formidable, and can get us faster-running applications with some wild brute-force optimizations nobody thought possible. Tobi goes in and [redoes Campfire with vanilla Ruby instead of Rails](https://github.com/tobi/campfire-once-ruby-ractor) - Falcon, Ractors, plain modules over SQLite, byte-identical HTML - and gets pages served 15 to 27 times faster…

And all of that makes life better for everyone doing Ruby and Rails, *today.* Not “in a year when LLMs will catch up” or “your CISO finally approves GitHub Copilot”. Not “in a few months once we have our [governance strategy](https://codeberg.org/mosscap/governance) figured out”. Today.

We can literally go and fix the stuff that was extremely annoying for years, quickly, for relatively modest amounts of money.

The only thing we need is to be even moderately curious, instead of waiting for permission. Nobody in Berlin in 1925 was waiting for the Kaiser to come back and approve the cabaret - the Kaiser had retired to the Netherlands, and the cabaret was *better* for it.

We have work to do. He doesn’t:

> Sir, I'm already rich. The last thing I want is more customers 😂
>
> [August 19, 2026](https://x.com/dhh/status/2090138454839439830)
