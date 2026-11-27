---
title: Using Kamal 2.0 in Production
link: https://fly.io/blog/kamal-in-production/
source: fly-io
published: 2025-05-29T00:00:00Z
updated: 2026-11-05T00:08:46Z
first_seen: 2026-11-27T00:01:05.630507547Z
content: extracted
html: 2025-05-29-using-kamal-2-0-in-production.html
remote_preview:
  url: https://fly.io/blog/kamal-in-production/assets/production.jpg
  alt: Robots putting widgets in boxes on an assembly line
extra:
  thumbnail: https://fly.io/blog/kamal-in-production/assets/production.jpg
---

Author

![Sam Ruby](https://fly.io/static/images/sam.webp)

Name

Sam Ruby

bsky:intertwingly.net

[bsky:intertwingly.net](https://bsky.app/profile/intertwingly.net)

![Robots putting widgets in boxes on an assembly line](https://fly.io/blog/kamal-in-production/assets/production.jpg)\
 Image by [Annie Ruygt](https://annieruygtillustration.com/)

[Agile Web Development with Rails 8](https://pragprog.com/titles/rails8/agile-web-development-with-rails-8/ "") is off to production, where they do things like editing, indexing, pagination, and printing. In researching the chapter on Deployment and Production, I became very dissatisfied with the content available on Kamal. I ended up writing my own, and it went well beyond the scope of the book. I then extracted what I needed from the result and put it in the book.

Now that I have some spare time, I took a look at the greater work. It was more than a chapter and less than a book, so I decided to publish it [online](https://rubys.github.io/kamal-in-production/ "").

This took me only a matter of hours. I had my notes in the XML grammar that Pragmatic Programming uses for books. I asked GitHub Copilot to convert them to Markdown. It did the job without my having to explain the grammar. It made intelligent guesses as to how to handle footnotes and got a number of these wrong, but that was easy to fix. On a lark, I asked it to proofread the content, and it did that too.

* * *

Don’t get me wrong, Kamal is great. There are plenty of videos on how to get toy projects online, and the documentation will tell you what each field in the configuration file does. But none pull together everything you need to deploy a real project. For example, [there are seven things you need to get started](https://rubys.github.io/kamal-in-production/Assemble/ ""). Some are optional, some you may already have, and all can be gathered quickly **if you have a list**.

Kamal is just one piece of the puzzle. To deploy your software using Kamal, you need to be aware of the vast Docker ecosystem. You will want to set up a builder, sign up for a container repository, and lock down your secrets. And as you grow, you will want a load balancer and a managed database.

And production is much more than copying files and starting a process. It is ensuring that your database is backed up, that your logs are searchable, and that your application is being monitored. It is also about ensuring that your application is secure.

My list is opinionated. Each choice has a lot of options. For SSH keys, there are a lot of key types. If you don’t have an opinion, go with what GitHub recommends. For hosting, there are a lot of providers, and most videos start with Hetzner, which is a solid choice. But did you know that there are separate paths for Cloud and Robot, and when you would want either?

A list with default options and alternatives highlighted was what would have helped me get started. Now it is available to you. The [source is on GitHub](https://github.com/rubys/kamal-in-production/ ""). [CC0 licensed](https://creativecommons.org/public-domain/cc0/ ""). Feel free to add side pages or links to document Digital Ocean, add other monitoring tools, or simply correct a typo that GitHub Copilot and I missed (or made).

* * *

And if you happen to be in the south eastern part of the US in August, come see me talk on this topic at the [Carolina Code Conference](https://blog.carolina.codes/p/announcing-our-2025-speakers ""). If you can’t make it, the presentation will be recorded and posted online.

Next post ↑

[My AI Skeptic Friends Are All Nuts](https://fly.io/blog/youre-all-nuts/)

Previous post ↓

[parking\_lot: ffffffffffffffff...](https://fly.io/blog/parking-lot-ffffffffffffffff/)
