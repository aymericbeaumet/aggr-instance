---
title: 'AI Ambition: ArtCraft Cloned Photoshop and Now Wants to "Rebuild Everything"'
link: https://latent.space/p/artcraft/
source: latent-space
published: 2026-11-13T04:17:25Z
updated: 2026-11-13T04:17:25Z
first_seen: 2026-11-13T07:09:29.179831632Z
authors:
- Richard MacManus
summary: 'ArtCraft used AI to rewrite Photoshop as open source software. For CEO Brandon Thomas, the vision is even bigger: to take back the internet!'
content: extracted
html: 2026-11-13-ai-ambition-artcraft-cloned-photoshop-and-now-wants-to.html
remote_preview:
  url: https://beehiiv-images-production.s3.amazonaws.com/uploads/asset/file/e61b607b-f9a5-4a6d-846b-f8792dd0a549/take-back-internet2.jpg?t=1794543381
  alt: 'AI Ambition: ArtCraft Cloned Photoshop and Now Wants to "Rebuild Everything"'
extra:
  thumbnail: https://beehiiv-images-production.s3.amazonaws.com/uploads/asset/file/e61b607b-f9a5-4a6d-846b-f8792dd0a549/take-back-internet2.jpg?t=1794543381
---

ArtCraft used AI to rewrite Photoshop as open source software. For CEO Brandon Thomas, the vision is even bigger: to take back the internet!

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/e61b607b-f9a5-4a6d-846b-f8792dd0a549/take-back-internet2.jpg?t=1794540983)

The theme of day two of the [AI Engineer Code Summit](https://ai.engineer/code/2026/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) in San Francisco was **increasing ambition using AI**. And there have been few projects as ambitious as [ArtCraft](https://getartcraft.com/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) in the AI era. So far the fledgling company has released **17 open source versions of popular software tools**, starting with a Photoshop alternative built from scratch in Rust called [PhotoCraft](https://getartcraft.com/apps/photocraft?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv).

You could feel the excitement in the room this morning when ArtCraft founder and CEO **Brandon Thomas presented a kind of manifesto for AI-based open source** at AIE Code. When he mentioned that ArtCraft had cloned Adobe’s Acrobat, with a product called PdfCraft, it got a round of applause from the audience. In wrapping up, his rallying cries of **“let's take back the entire internet”** and **“let's rebuild everything”** were greeted with whoops and cheers.

Thomas began his presentation by reliving the internet’s early history. In his view, the web started out with the right intentions, but things got screwed up once Web 2.0 hit the mainstream.

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/ff56b3f2-d06d-40b1-b413-df8478d128a7/image.jpeg?t=1794541831)

“We had a bunch of chat applications that were completely open,” he said, regarding the 1990s internet. “So, IRC is like Discord, but you can spin up your own servers that you actually own. You can use whatever programs to automate them and interrogate them without rules.”

His point wasn’t necessarily that everything was open source, but that **web users could freely build things.** With Flash, for example — a commercial animation tool eventually acquired by Adobe — “kids were making all kinds of animations.”

When Web 2.0 came along, with its superior broadband and smartphones, “it brought everybody into the fold.” Thomas acknowledged that this widened the scope of internet software, but at **the cost of that freedom to build with leading apps.**

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/7c8f16dc-1d40-4464-8376-84f4c9f6071a/image.jpeg?t=1794541831)

“The diffuse, experimental, early web started to disappear,” he said. “The APIs got taken away from us, and then we started seeing **distribution being gatekept by the big providers.”**

It seems Adobe in particular annoyed Thomas. He described getting billed with an unexpected fee when he cancelled a monthly plan.

“It's really frustrating that the web — that was once really, really promising and open and free — has turned into this extractive, owned, rent-seeking world.”

This inspired him to start ArtCraft, “with the ethos of **‘let's build everything open source.’”** A message even anti-AI people have gotten behind, according to Thomas.

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/609f1542-f774-480d-94e9-30c2e4c70be2/image.jpeg?t=1794541830)

In addition to PhotoCraft, they’ve copied other apps in the Adobe suite, cloned Figma, and created a Microsoft-like word processor called WordCraft. There are also independent apps such as SolidCraft, a community-led CAD project distributed through ArtCraft’s launcher.

Thomas reiterated that these AI-built applications are **100% open** — “not only the desktop apps, but the actual infrastructure as well.”

## PhotoCraft: The development process

ArtCraft is starting with rewriting familiar tools. Thomas’s longer-term vision is for users (such as artists who were previously using Photoshop) to tailor the software to their own workflows.

As he framed it, they want to **“undo what had happened during Web 2.0**, when everything became owned and consolidated and gatekept, and just give people back their applications.”

AI is the means to achieve that, he said.

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/cb390f74-b6ed-4ef3-8e0d-6dc8c1a49f61/image.jpeg?t=1794541831)

But aren’t there already open source alternatives to Photoshop, like GIMP?

“GIMP isn't really used by artists,” said Thomas during his presentation. “We actually observed **artists vibe coding their own workflows** on top of the software that we gave them.”

To begin creating PhotoCraft, the team gave an AI model a detailed prompt outlining Photoshop’s tools, data structures and file importers. After that, they “laid out **a large, large prompt** to say, let's build out the application.”

As part of that process, they made everything **MCP and CLI compatible**.

Within 24 hours, said Thomas, “we were looking at a pretty good approximation of what Photoshop was — and it kind of blew our minds away.”

Thomas noted that PhotoCraft is “still not at 100% parity” — he said **they’ve just passed 60%** — but the team has set an internal goal to reach as close to 100% as possible. ([The project’s GitHub](https://github.com/storytold/photocraft/blob/main/docs/target-app-parity.md?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) claims about 76% parity for feature breadth and about 46% for professional readiness.)

“Right now, we want to fill the muscle memory of all the artists that use Adobe. **So we are going for Adobe**, their interface and ‘look and feel’ directly.”

But after ArtCraft has achieved that, they plan to “build new apps that have **new interfaces that are more agentic.**”

## A big reaction

Thomas said he was surprised by the amount of attention PhotoCraft, in particular, has gotten — ranging from the New York Times requesting an interview, to a swelling of its open source community to **about 4,000 developers**.

“I'm still struggling to bear with what has happened. It struck a nerve with people. They felt sort of **maligned by Adobe and other tech companies**. And what we built has really resonated with them.”

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/53606565-9ba7-4581-b2eb-7d80bcd66def/image.jpeg?t=1794541831)

*After his presentation, Thomas was mobbed by AIE Code attendees.*

ArtCraft’s apps are being downloaded at the rate of about 1.7 million per day, which Thomas said is still accelerating.

In addition to the 17 official apps, there are around 300 being developed by the community, he added.

## Rust is key, but there have been technical challenges

Other than the AI technology, Thomas said that **Rust was a crucial part of making the apps performant** — he claimed that PhotoCraft opens ten times faster than Photoshop, for example.

“Rust was probably essential in this product's success,” he said. “We've seen people do it in Electron before, but **for high-quality creative apps, you really need something that's native.**”

For those engineers who are not experts in Rust, Thomas has good news for you.

“Do you need to know Rust to start using Rust today as an engineer? Definitely not. \[...\] Your agents will drive it just fine.”

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/e87c1680-5e2f-42ea-86a6-c624b807de89/image.png?t=1794541831)

However, Thomas admitted that not everything has gone swimmingly.

**“We've run into a lot of gotchas** — like permissions, GitHub, scaling, CI,” he said. “CI is falling over. We've been spinning up our own. We found a good place to get RAM on co-located servers.”

But they have found an equilibrium on what agents can do and what needs to be handled by people.

“If you're doing backend work or things that require schema migrations \[or\] API design, let your humans do that. But for desktop apps and games and anything that's basically immutable, **throw your agents at it and give them full confidence.”**

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/c78ba33c-dfba-4c34-ac42-be4ec4a697c7/image.png?t=1794541831)

And it turns out, there’s even an ArtCraft solution for you there.

“AutoCraft is a completely autonomous software factory that we're going to open source.”

## AI open source FTW

There’s no doubting **the ambition** of Brandon Thomas and his ArtCraft company.

**“We can open source the entire world,”** he proclaimed at the end of his presentation. “Let's own our apps, our infra, our protocols, our models — all of it.”

It’s a big vision, potentially even on the scale of Linux if its current downloads growth rate continues. But, ironically perhaps, what intrigues me most about ArtCraft is **the growth of its** ***human*** **contributor community.** After all, that’s what the original web got most right: human collaboration at scale.

The big difference from the 1990s is that now it’s agents doing the bulk of the development work.
