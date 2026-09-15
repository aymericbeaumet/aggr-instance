---
title: How Cars24 scales conversations and builds faster with OpenAI
link: https://openai.com/index/cars24
source: openai-com-news
published: 2026-07-16T00:00:00Z
updated: 2026-07-16T00:00:00Z
first_seen: 2026-09-07T17:03:44.343711062Z
replicated_at: 2026-09-15T01:23:10.737129797Z
summary: Cars24 uses OpenAI-powered voice and chat agents to handle 1M+ monthly conversation minutes, recover 12% of lost leads, and bring agentic workflows to teams across the company.
content: extracted
html: 2026-07-16-how-cars24-scales-conversations-and-builds-faster-with.html
preview:
  file: 2026-07-16-how-cars24-scales-conversations-and-builds-faster-with.preview-5737f885b968.webp
  width: 256
  height: 144
  alt: Cars24 logo on blue customer story artwork
  color: '#c2c7ea'
images:
- source: https://images.ctfassets.net/kftzwdyauwt9/7CDBt34djpI5pY21NFpVyc/c2acb8f59210c75f3ca156a8249cdded/cars24-seo.png?w=1600&h=900&fit=fill
  original:
    file: 2026-07-16-how-cars24-scales-conversations-and-builds-faster-with.image-7c5d4fb40c71.png
    width: 1600
    height: 900
  variants:
  - file: 2026-07-16-how-cars24-scales-conversations-and-builds-faster-with.image-acf2cb3f6db5.webp
    width: 320
    height: 180
  - file: 2026-07-16-how-cars24-scales-conversations-and-builds-faster-with.image-276cbdfa5484.webp
    width: 640
    height: 360
  - file: 2026-07-16-how-cars24-scales-conversations-and-builds-faster-with.image-a0de0ef77b60.webp
    width: 960
    height: 540
  - file: 2026-07-16-how-cars24-scales-conversations-and-builds-faster-with.image-6235f5f82ff8.webp
    width: 1280
    height: 720
  - file: 2026-07-16-how-cars24-scales-conversations-and-builds-faster-with.image-fe2270c57a12.webp
    width: 1600
    height: 900
  color: '#fefefe'
- source: https://images.ctfassets.net/kftzwdyauwt9/527iuIxnFrheVEhh811agF/46ddb0584482c025a9981af5f92afe33/Cars24_UI_2.png?w=3840&q=90&fm=webp
  original:
    file: 2026-07-16-how-cars24-scales-conversations-and-builds-faster-with.image-99de4e80a5f4.webp
    width: 1080
    height: 1080
  color: '#96cafc'
- source: https://images.ctfassets.net/kftzwdyauwt9/6F8N9Pgo43YXWdvtC6oKUh/4184f0437f3a1306f8ff93a0082716ea/Cars24_UI.png?w=3840&q=90&fm=webp
  original:
    file: 2026-07-16-how-cars24-scales-conversations-and-builds-faster-with.image-2b11cd6f6124.webp
    width: 1080
    height: 1080
  color: '#fbfbfc'
---

Cars24 operates one of the world’s largest AI native automotive ecosystems for buying and selling cars in India, with additional operations in the UAE and Australia. The company supports the full car ownership journey, from discovery and financing to resale and post-purchase services, while helping extend the lifecycle of vehicles through a more efficient and accessible pre-owned car ecosystem, in a market where most transactions remain manual, regulated, and fragmented.

## Scaling a complex, conversation-driven marketplace

Unlike traditional e-commerce, car buying and selling in India rarely happens in a single session. Much of the process happens outside the app, across calls, document checks, and follow-ups that can take days or weeks. As Cars24 scaled, this created a core challenge: how to deliver consistent, high-quality experiences across millions of interactions without continuously expanding operational teams.

To address this, Cars24 used OpenAI’s technology to build voice and chat agents for buying, selling, financing, follow-up, and support. The company also rolled out ChatGPT Enterprise and Codex across its central organization, helping employees in engineering, finance, legal, marketing, and operations build AI-powered workflows of their own.

## Automating the full customer journey with AI agents

Cars24 began with one of the most constrained parts of the business: the middle and bottom of the sales funnel, where conversation drives conversion. Buying or selling a car does not end in a few clicks. Customers need help comparing options, booking visits, preparing documents, arranging financing, and following up after a visit.

Using OpenAI APIs, Cars24 built AI-powered voice and chat agents that support customers across that journey. When a buyer calls Cars24, an AI agent asks about their budget, family size, commute needs, and preferred car type. It recommends cars from the Cars24 catalog, books a test drive, and can help the customer explore financing.

![CarGPT test drive booking screen with date and time slot selections](https://images.ctfassets.net/kftzwdyauwt9/527iuIxnFrheVEhh811agF/46ddb0584482c025a9981af5f92afe33/Cars24_UI_2.png?w=3840&q=90&fm=webp)

![CarGPT recommending Volkswagen Virtus models with options to view details or book a test drive](https://images.ctfassets.net/kftzwdyauwt9/6F8N9Pgo43YXWdvtC6oKUh/4184f0437f3a1306f8ff93a0082716ea/Cars24_UI.png?w=3840&q=90&fm=webp)

The agent then follows up before the test drive to confirm the visit, suggest alternative cars if preferences have changed, and collect additional details for financing. After the visit, an AI agent checks whether the customer wants to move forward, book another visit, or explore a different option. After purchase, agents continue to support customers with feedback, warranty questions, returns, and after-sales service.

For sellers, the workflow follows a similar path. An AI agent collects vehicle details, schedules an inspection, sends reminders, helps reschedule missed appointments, and gathers competitive insights if a customer has sold elsewhere. For leads that used to drop out after 10 days, AI agents now re-engage customers, qualify renewed intent, and return them to the funnel when Cars24 can serve the price they are looking for.

> “Buying a car in India is a journey, not a transaction. For years, the experience depended on who picked up the phone. AI changes that. Today, we handle over a million conversation minutes a month through AI, giving every customer a high-quality experience at any scale.”

—Vikram Chopra, Builder, Cars24

## Embedding Codex into the software development lifecycle

Cars24 has also deployed Codex across its software development lifecycle, treating it as a participant in day-to-day work rather than a standalone coding tool.

Product managers use Codex to create and refine Linear tickets. Engineering teams tag Codex into bug reports so it can pick up defined tasks. Codex also summarizes work across GitHub and posts updates for teams, reducing the number of standups needed to keep work moving.

Cars24 reoriented its project management workflows around Linear in a matter of weeks, creating a cleaner path for Codex to support day-to-day work. Across product, design, and engineering teams, Codex now helps move work from ticket creation and task grooming to implementation, bug resolution, and updates.

> “I thought Codex would make our engineers faster. What surprised me was how quickly it spread beyond engineering. Product managers, finance teams, and even day-to-day workflows started changing. That is when I realised we had not just changed how we write code but had changed how the entire company thinks about getting work done.”

—Jayesh Gupta, Builder—AI & Innovation, Cars24

## Extending agentic workflows to every team

One of the most significant shifts has been the adoption of Codex beyond engineering, with AI becoming an operating layer that helps teams solve their own problems, build their own tools, and improve the business faster.

In finance and investor relations, for instance, Cars24 teams use Codex to pull numbers from their systems of record, run analysis, and prepare investor reporting workflows without manually chasing inputs from multiple business heads.

Another finance workflow uses Codex to review purchase requests and purchase orders above a defined threshold. The automation checks for anomalies, flags concerns, and auto-approves requests where no issues are found.

These examples reflect a broader shift inside Cars24: employees are increasingly building the tools they need instead of waiting for centralized engineering support. Some teams have built “chief of staff” agents that connect Slack, Gmail, WhatsApp, and other systems to manage communication, scheduling, hiring workflows, and follow-ups.

## Building an AI-first operating model

Cars24’s experience shows how AI can move from pilots to production when it is tied to specific business workflows. The company started with high-volume customer conversations, where OpenAI-powered agents could improve lead coverage, support conversion, and create a more consistent experience across the buying and selling journey.

Internally, Cars24 has deployed ChatGPT Enterprise and Codex to about 600 employees across its central organization, with 85% to 90% daily active usage. Teams in engineering, finance, legal, marketing, and operations now use AI to build workflows, automate recurring tasks, and move work across systems more efficiently.

For Cars24, AI is becoming both a customer engagement layer and an internal operating layer, helping the company serve more customers, build faster, and scale with greater consistency.
