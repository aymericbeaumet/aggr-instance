---
title: Building resilient AI-powered software, and planning for when things go wrong
link: https://newsletter.pragmaticengineer.com/p/building-resilient-systems
source: newsletter-pragmaticengineer-com
published: 2026-10-13T16:03:33Z
updated: 2026-10-13T16:03:33Z
first_seen: 2026-10-13T18:08:43.541012300Z
authors:
- Gergely Orosz
summary: How to handle uncertainty with AI vendors and disaster recovery (DR) approaches to consider. An excerpt from the book Building Resilient Distributed Systems by Sam Newman
content: feed
html: 2026-10-13-building-resilient-ai-powered-software-and-planning-for.html
remote_preview:
  url: https://substackcdn.com/image/fetch/$s_!P4Ph!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fad7aee47-aa20-4fc7-8e43-ae2bf2360782_1320x716.png
---

Last week’s guest on the podcast [was Sam Newman](https://newsletter.pragmaticengineer.com/p/building-resilient-systems-with-sam), who had released a book I’ve personally long awaited titled **[Building Resilient Distributed Systems](https://www.oreilly.com/library/view/building-resilient-distributed/9781098163532/)**. It’s an especially timely book for many of us software engineers, as we’re seeing more outages with nearly all services and systems – much of it caused by more code deploys thanks to AI agents writing more code; and more bugs causing more incidents in production.

[![](https://substackcdn.com/image/fetch/$s_!a_hF!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc9e26f65-4b0a-4f2c-86a1-2d0f00b67a60_1456x1762.png)](https://substackcdn.com/image/fetch/$s_!a_hF!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc9e26f65-4b0a-4f2c-86a1-2d0f00b67a60_1456x1762.png)

*Sam’s new book,[Building Resilient Distributed Systems](https://www.oreilly.com/library/view/building-resilient-distributed/9781098163532/), in print form*

Sam’s book being “fresh out of the oven,” it naturally also touches on what AI changes as we design resilient systems. In fact, Sam told me that he took a total of three years to finishe the book – instead of the two he originally planned – because he went back and reworked all parts of the book, updating parts where AI is relevant to talk about.

I asked Sam if he would consider sharing an excerpt from the book, with sections especially relevant for anyone building systems that ought to be reliable. Sam graciously agreed, and today’s issue covers a few important topics, spread across two chapters from his book. We get into:

1. **Disaster recovery (DR**). Recovering from catastrophic failures: planning for cloud region/data center/site-wide outages, the concept of RPO and RTO, DRO objectives and SLOs.

2. **Disaster Recovery Patterns**. Backup-and-restore, pilot light failover, warm failover and multisite active-active.

3. **Infrastructure availability when planning for DR**. Ongoing GPU and CPU shortages mean you cannot expect infrastructure availability at all times, so you need to understand constraints your vendors have, and to prioritize critical services.

4. **Resiliency of AI-powered software**. Model provider resilience, the challenge of models changing, and the need for observability.

5. **How to handle uncertainty.** Tactics like being multi-model and multi-vendor, making costs transparent, and frequently analyzing vendors.

*Even though this article is pretty long, we cover less than a full chapter from the book's 16 chapters. If you build reliable systems for a living, I warmly recommend the book itself, and you can get it [from the publisher](https://www.oreilly.com/library/view/building-resilient-distributed/9781098163532/).*

*My usual disclaimer: as with all my recommendations, I was not paid to recommend this book, and none of the links are affiliate ones. See [my ethics statement](https://blog.pragmaticengineer.com/ethics-statement/) for more details.*

* * *

*The excerpts below are from [Building Resilient Distributed Systems](https://www.oreilly.com/library/view/building-resilient-distributed/9781098163532/), by Sam Newman. Copyright © 2026 Sam Newman. Published by O’Reilly Media, Inc. Used with permission.*

## 1\. Disaster Recovery

*Excerpt from Chapter 10: Disaster Recovery.*

We’ve already discussed how to handle a variety of failures that impact individual service instances. But what happens when a catastrophic failure causes widespread outage of large parts of your system? Fire, flood, electrical supply issues, and errors in the underlying cloud infrastructure are all outside of your control, and at one fell swoop, they can render your attempts to make your system more robust totally redundant.

Handling these situations falls under the auspices of disaster recovery (DR), a set of procedures that should kick in when something really bad happens. As with much of what we’ve looked at in this book, trade-offs abound. As you reduce your period of acceptable downtime or data loss, cost and complexity increase, so picking the right approach for your system can be a delicate balancing act. In this chapter, I’ll give you some concrete targets to work with to help drive your own DR approach.

### Cloud Region, Data Center, or Site?

The exact terminology you use to discuss your infrastructure will likely vary based on implementation details, and this in turn can make discussions around generally applicable patterns more difficult. When running on the public cloud, for example, you’ll probably be familiar with the concept of a cloud region, which is where your cloud resources live. When running on a private cloud or in a more traditional on-premises setup, you’ll instead think in terms of data centers.

As we discuss the various options around handling DR, the patterns that we’ll see later in this chapter can be used for either public or private cloud scenarios. Therefore, to keep things simple, I’ll use the term site as a general descriptor of where your computing infrastructure lives.

There are some specific issues to consider around mixing cloud vendors as part of a DR strategy, and we’ll look more at those in Chapter 11.

### How Good Does Your DR Need to Be?

A spectrum of DR options are available to you, and they can range greatly in terms of cost, complexity, and recovery. To help you decide which DR approach is most appropriate, we should first try to understand how to determine the appropriate requirements for your context.

A simple way to define technical requirements around DR is to make use of two concrete measurable targets, recovery point objective and recovery time objective. In Figure 10-1, you can see how these objectives relate to an incident:

[![](https://substackcdn.com/image/fetch/$s_!-Hph!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3af4a60c-dabe-4da4-b6e9-1227c4e7abfd_1318x392.png)](https://substackcdn.com/image/fetch/$s_!-Hph!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3af4a60c-dabe-4da4-b6e9-1227c4e7abfd_1318x392.png)

*Figure 10-1. The key objectives to define as part of selecting an appropriate DR strategy*

### Recovery Point Objective

The recovery point objective (RPO) defines the maximum amount of time that can have passed since the data was last backed up or synchronized. Put another way, RPO defines the upper duration of potential data loss. If the data for your system backs up once an hour, then it’s possible to lose up to an hour of data if your system has an incident. Therefore, your RPO would be an hour.

### Recovery Time Objective

The recovery time objective (RTO) defines how long it’s acceptable for your system to be unavailable in the wake of an incident. If all your computers became unavailable, perhaps because of a data center–wide outage, how long would it take you to have your services running in an alternative data center? Your RTO includes all the time needed to recover data from the last recovery point, make computing resources available for your workloads, and redirect traffic to your new site—so there are more variables to consider in terms of bringing your system back online than just time.

### You Can’t Have Everything

If asked what RPO and RTO you’d want in a vacuum, you’d probably give a very simple answer: you’d want an RPO of zero (i.e., a guarantee of no data loss), and an RTO of zero too. This is an understandable wish, but as you reduce your RPO and RTO, cost and complexity increase dramatically—and in technical terms, it’s not possible to guarantee a zero RPO or RTO in any case.

An additional complication is that different types of workloads might require different RPOs and RTOs. Consider a bank, for example. Having a low RPO to ensure that payment information isn’t lost would be key, and having a low RTO to ensure that people can still take money out of their account is vital. On the other hand, the part of the system that enables new customers to sign up for an account could have a higher RTO and RPO, as the impact to the business is less severe.

So when deciding on the appropriate RPO and RTO, there needs to be a discussion with stakeholders regarding end-user needs and regulatory and legislative requirements, which are balanced against the cost and complexity of implementing the required solution. For example, your product owner might initially say they need an RPO of 10 minutes, but if you could tell them it would take months of work to implement (during which other work has to be deprioritized), they might reduce their target, perhaps accepting an RPO that is easier for you to deliver. Your target RPO and RTO should also exceed anything specified in any service level agreements you have.

### DR Objectives and SLOs

At first glance, RPO and RTO would appear to make for good service level objectives (SLOs). After all, they are both important measures that directly impact user experience. However, there are some challenges around managing RPO and RTO as SLOs.

When determining if we are meeting an SLO, we need to collect lots of service level indicators (SLI) to confirm whether or not we are on target. So if we had an SLO that defined 99th percentile latency over a one-month period of less than 500 milliseconds, we would analyze the latency-related SLIs for that monthlong period to make sure we were meeting the target. With RPO and RTO, though, what would our SLI be?

The reality is that we don’t experience disasters regularly enough to gather useful SLIs. We can of course define related SLOs around things related to our DR approach. For example, it would be possible to define an SLO around the speed of data replication, as this is something we are doing in production and can programmatically measure. But at best SLOs are only tangentially measuring RPO and RTO.

So RPOs and RTOs don’t really fit neatly into the SLO concept. Of course, that doesn’t mean that you can’t still test your system to make sure it is delivering on your DR objectives—something we’ll discuss more later in the chapter.

## 4\. Disaster Recovery Patterns

*Another excerpt from Chapter 10: Disaster Recovery.*

With the discussion of RPO and RTO in mind, let’s take a look at some common patterns used to implement DR in the wake of a major incident. In Figure 10-2, we can see an overview of each of the four patterns we will explore, arranged (broadly speaking) in order of increasing complexity and improving RTO and RPO.

[![](https://substackcdn.com/image/fetch/$s_!P4Ph!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fad7aee47-aa20-4fc7-8e43-ae2bf2360782_1320x716.png)](https://substackcdn.com/image/fetch/$s_!P4Ph!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fad7aee47-aa20-4fc7-8e43-ae2bf2360782_1320x716.png)

Figure 10-2. The key objectives to define as part of selecting an appropriate DR strategy

If starting from scratch, many of these patterns can build on one another, allowing you to incrementally increase the complexity (and cost) of your DR solution over time. For example, a backup and restore approach requires rerouting, a pilot light setup will add the need for cross-site data replication and infrastructure automation, and a warm failover adds on the cost of having some infrastructure ready to go. So if you aren’t sure where to start, working your way through the patterns from left to right in Figure 10-2 makes a lot of sense.

If you have different RPO and RTOs for different parts of your system, you may also end up using a mix of these patterns. Note that they all require some form of rerouting of traffic, and most require some form of cross-site data replication. Those options are discussed later in this chapter.

### Pattern: Backup and Restore

*Recovery by restoring data from a backup and loading it into a replacement system.*

As outlined in Figure 10-3, with this pattern, data is periodically backed up to a separate site. In the event that the primary site fails, infrastructure needs to be provided at a new site, and the backed-up data restored.

[![](https://substackcdn.com/image/fetch/$s_!sl8H!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4c818d67-38f2-4d49-a15a-b45a8896d032_1316x346.png)](https://substackcdn.com/image/fetch/$s_!sl8H!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4c818d67-38f2-4d49-a15a-b45a8896d032_1316x346.png)

*Figure 10-3. The backup and restore DR pattern*

#### How it works

This pattern is very straightforward to implement, consisting of a few steps:

**1\. Back up data regularly.**

All data that you want to make available to the failover site needs to be regularly backed up, with the backup data stored somewhere other than the primary site.

**2\. Provision infrastructure on the failover site.**

This would ideally be automated, as manual infrastructure provisioning would likely lead to longer RTO and an increased chance of errors.

**3\. Restore data from backup to the failover site.**

Data is loaded from the backup location into the newly provisioned infrastructure.

**4\. Switch traffic from the primary to the failover site.**

Once infrastructure is available and the data is restored, traffic can be switched to the secondary.

Ideally, your failover site runs the same core technology stack as the primary site, meaning that any investment in infrastructure automation on your primary can also be used on the failover (for example, being able to reuse Terraform scripts). Keeping the code to automate setup of the primary and the failover the same is very useful, as it means that you’ll have a higher degree of confidence that spinning up the infrastructure on the failover site will work. If your failover site uses a different technology stack—meaning it’s not possible to share automated infrastructure provisioning code between the primary and secondary sites—then it becomes even more vital that your DR failover process is regularly tested. A caveat to spinning up infrastructure on demand here comes in terms of guaranteeing that infrastructure is actually available when you need it, something we’ll explore later in this chapter.

#### When to use it

This pattern is generally applicable in situations where your primary site is unavailable and you can tolerate a long RTO, and especially in those situations where you want to keep costs low. It’s important to note that regular tests of a backup and restore process are necessary to ensure that it actually works. One approach I’ve found useful is to perform a backup and restore to a secondary site as part of other regular activity (for example, a regular load test exercise).

The RPO of this pattern will depend on the frequency of backup. The RTO will be the longest of the patterns we’ll look at, and its time will depend primarily on how long it takes to restore data from the backup and provision replacement resources. With a fully automated process and smaller data volumes, this could be in the minutes to hours range. Manual recovery processes will likely extend these timelines greatly, as will the size of backups involved—terabytes of data take longer to send than gigabytes! Another major factor will be availability of the required computing resources. If you end up having to purchase replacement equipment, this pattern could easily result in a timeline of weeks or longer.

### Pattern: Pilot Light Failover

*Data is constantly synced to a failover site, with infrastructure provisioned after failover.*

With a pilot light setup, data is continually replicated from the primary to the failover site, as shown in Figure 10-4. Infrastructure is not provisioned until a failover is required, ensuring that costs can be kept down when working with public cloud infrastructure.

[![](https://substackcdn.com/image/fetch/$s_!2Jjt!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F11f6ee69-6374-40a9-b2f5-cebc53dd19eb_1914x610.png)](https://substackcdn.com/image/fetch/$s_!2Jjt!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F11f6ee69-6374-40a9-b2f5-cebc53dd19eb_1914x610.png)

*Figure 10-4. The pilot light failover DR pattern*

#### How it works

This pattern builds on a lot of the work done with a backup and restore, the main change being that the data is continually synchronized to the failover site. As such, the steps involved look fairly similar to the backup and restore pattern:

**1\. Replicate data continually.**

As data is committed to the primary, it is continually replicated to the failover site. This can be done in a variety of ways, such as by making use of native database technology or application-level replication.

**2\. Provision infrastructure on the failover site.**

The assumption is that databases and datastores are already present on the failover site, so only remaining resources like compute would need to be provisioned.

**3\. Switch traffic from the primary to the failover site.**

Once infrastructure is available and the data is restored, traffic can be switched to the failover.

From an implementation point of view, the new source of complexity comes from the replication of data from the primary to the failover. For systems designed with this process in mind, it might be somewhat trivial to implement. However, things become more complex when adapting existing systems if you’re making use of a data product that doesn’t support this replication strategy natively. This is worthy of a more in-depth discussion later in this chapter, especially as the next two patterns also rely on this capability.

#### When to use it

When compared to a backup and restore, the pilot light failover allows for a much shorter RPO because of more frequent data replication, while also reducing RTO by eliminating the time taken for data restoration.

The cost-saving of a pilot light failover really makes sense only in the context of the public cloud. Costs are reduced primarily in terms of not paying for infrastructure unless it’s used. But if your failover site is your own private infrastructure that you have to pay for regardless of whether it is used, then some form of a warm failover makes a lot more sense. At that point you’re already paying for the hardware, so you might as well use it.

### Pattern: Warm Failover

*Data is synced to a failover site, with at least some infrastructure already provisioned to handle traffic.*

With a warm failover, as shown in Figure 10-5, data is continually replicated from the primary to the failover site, with at least some infrastructure already provisioned in advance of the failover itself.

[![](https://substackcdn.com/image/fetch/$s_!0q1H!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6262c7c8-0610-4a9f-b3eb-618faee54dfe_1910x722.png)](https://substackcdn.com/image/fetch/$s_!0q1H!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6262c7c8-0610-4a9f-b3eb-618faee54dfe_1910x722.png)

*Figure 10-5. The warm failover DR pattern*

Having some infrastructure provisioned even before the failover occurs results in a reduction in RTO for some functionality. How much infrastructure is provisioned on the failover can vary, but the idea is that some capacity already exists to allow for a much faster failover.

A common approach would be to prioritize the subset infrastructure that is provided on the secondary site for the more critical capabilities of the system, allowing for a shorter RTO for the most important functionality.

**Cold, Warm, and Hot Failovers**: A brief note is warranted here regarding the term warm. You’ll find various solutions and vendor products promising cold, warm, or hot failover, with the only real distinguishing factor between these terms often being that a hot failover would give you a lower RTO in comparison to a warm failover, and a warm failover would give you a lower RTO than a cold one. Given that what constitutes hot or warm from vendor to vendor and from tech stack to tech stack varies so greatly, I just decided to pick warm and stick with it.

Naming things is hard.

*The “How it works” and “When to use it sections” are cut off from this excerpt, but are present inside the book.*

### Pattern: Multisite Active-Active

*The system is distributed across two or more sites, with writes being able to be sent to any site.*

With a multisite active-active setup, data is replicated constantly between two or more sites, as you can see in Figure 10-6. Traffic for any customer can be served from any site at any time.

[![](https://substackcdn.com/image/fetch/$s_!FylH!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0186c82d-c3d9-4067-8bc4-fd19425248ca_1908x988.png)](https://substackcdn.com/image/fetch/$s_!FylH!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0186c82d-c3d9-4067-8bc4-fd19425248ca_1908x988.png)

*Figure 10-6. The multisite active-active DR pattern*

This pattern sidesteps the need for a failover, as it is assumed that traffic for any user can switch between sites at any time. As such, it delivers the lowest RTO and RPO of any pattern we’ve looked at so far, though at much higher cost and complexity.

*The “How it works” and “When to use it sections” are cut off from this excerpt, but are present inside the book.*

## 3\. Infrastructure availability when planning for DR

*Yet another excerpt from Chapter 10: Disaster Recovery.*

At the time of writing, GenAI is being used by more and more industries. Whatever your views on the usefulness of this technology (or its broader societal impact), the nature of GenAI workloads for both training and inference purposes has driven ever-increasing demand for infrastructure, in turn leading to reduced capacity for many users. Even if you aren’t running what you consider to be specialized GenAI workloads (for example, those requiring specific GPUs), you may well see the impact of these shortages more broadly.

Aside from the ever-growing cost of GPUs, memory costs have spiked significantly in 2025 and 2026, impacting general server availability. I’ve already spoken to clients who are struggling to run “normal” (non-AI) workloads on some of the large public cloud providers, as they can’t secure enough capacity. Being able to shift regions in some cases is helping, but guarantees of future availability seem fairly weak. Even buying your own hardware doesn’t seem to be a quick fix, with lead times for servers seemingly extending further and further with each passing week.

This has to be taken into account when considering the patterns detailed previously. If your chosen approach assumes you can dynamically provision infrastructure when a failure occurs, you will have to look at what guarantees you have around the capacity you want. In a scenario where a major cloud vendor’s region has failed, you may find yourself fighting over scraps with everyone else.

There are three ways to handle this scenario: reserve capacity; prioritize critical services; or lastly, consider running a fully redundant set of infrastructure at all times.

Not every vendor offers the ability to reserve capacity, and even then it is limited to certain cloud services. On AWS, for example, you can pay a fee to guarantee the availability of EC2 instances (managed virtual machines) when you need them. This has a cost impact, as you pay a fee for the reservation whether or not you use the instances (the silver lining being that if you did end up using them, the cost would be cheaper than normal in-demand pricing). This doesn’t help you, though, when using cloud resources for which reservations aren’t possible—you can’t purchase guaranteed availability of Lambda invocations, for example.

Next, consider prioritizing critical services. If there is a cost to guaranteeing availability (or if only a limited amount of infrastructure is available), then it’s vital that this goes to workloads that have a higher criticality. This is where having separate RTOs and RPOs on a service-by-service basis comes into play.

Finally, if you could justify the complexity and expense of having a fully redundant set of infrastructure ready to run your system, then you’d have the confidence that your computing resources will be available, with the caveat that this option can be extremely costly. But that cost can be mitigated if you can make alternative use of the redundant resources.

Back in Chapter 4, I mentioned BankCo, where we ran calculations across a large pool of machines. A large portion of these computers were desktop PCs sitting idle in an office situated outside London. These PCs formed part of the bank’s DR strategy: in the event that traders had to evacuate the bank’s main location, they could relocate to the secondary site and continue working. It was a requirement that these standby PCs were constantly running to ensure that we knew that they worked, and so that updates could be pushed out to them. This was obviously back in the day when most of the critical work relied on thick-client applications, so they couldn’t be accessed via the web.

We were able to make use of these DR workstations to grow our pool of available workers. This meant that when the DR site wasn’t in operation (which was most of the time), we were able to carry out our calculations much faster, as we could parallelize the work over more machines. If the DR site’s machines were unavailable to us, we still had a set of our own servers on which we could run the calculations to suit our normal targets.

So if you are thinking about securing ring-fenced infrastructure as part of a DR approach, see if you can think of useful ways to put it to good use when it’s not needed for production workloads. You could use the spare infrastructure for a load test environment, for example; the load test could easily be aborted when DR is needed.

## 4\. Resiliency of AI-powered software

*Excerpt from Chapter 7: Resiliency and AI*

We’ve looked at how AI is changing how we build and operate our software. But we are also integrating AI into the products we build, using it to hopefully improve our software for our users. What new challenges does this create for ensuring our systems are resilient?

### Model Provider Resiliency

Foundation model providers OpenAI and Anthropic don’t have a great track record in terms of providing high levels of availability for their services, at least when compared to other managed service providers, like public cloud vendors. While I’m sure this will improve over time, having a potentially flaky third party in the mix of your critical functionality can be an issue.

A solution here is to consider ensuring that your software can still operate even if the model is unavailable. This could be achieved by limiting functionality in the case of the model being unavailable (or you getting rate limited or running out of tokens!)—this is an example of the graceful degradation pattern, which we’ll look at more in Chapter 11.

Another option is to have backup vendors for a given model to hedge against your normal model provider having availability issues. For example, you can use OpenAI models on Azure, a handy alternative if accessing the models via OpenAI’s own services proves to be too unstable, and in a similar way Anthropic’s models can also be accessed via Google’s Gemini Enterprise Agent Platform. When using backup vendors like this, there may be a lag in terms of how quickly the latest models may be available, but given that you may not always want to be on the bleeding edge of the models you use to embed in your software (something we’ll discuss in just a moment), this is probably fine.

### Model Changes

Consistency of behavior plays a big role in helping us ensure that our system is behaving appropriately. While dealing with the unexpected is a key concept in resiliency (or “graceful extensibility,” as David Woods put it), that doesn’t mean we want to build software where surprise is constant. Quite aside from the fact that LLMs are nondeterministic, small changes in models can also lead to quite different behavior.

I’ve seen reports where a model has started behaving differently, without any major change in behavior being flagged by the vendor. Additionally, moving to a newer version of a model can fundamentally change how your prompts may be handled.

It is therefore vital to have a good set of tests to validate that the AI portions of your code are behaving appropriately—and this should be a mix of traditional functional tests, together with the use of AI evaluation tools (evals). You want to make sure that when the unexpected occurs, you are ones who get surprised first, rather than your users.

I can recommend the second edition of Gayathri Mohan’s Full Stack Testing,3 for those of you wanting to explore testing in general, including verification of AI-based systems. For a deeper dive specifically into evals, I’d suggest Evals for AI Engineers by Shreya Shankar and Hamel Husain.

### Observability

The telemetry we receive from our own software and from third-party services alike is key to making sure our applications are working the way that we expect. As we discussed back in Chapter 3, we prefer a small variation of latency rather than a broad variation of latency. If the time it takes to successfully complete an operation varies wildly, it can be difficult to know where we should start triggering timeouts, for example. It also becomes more difficult to define latency-related SLOs.

Once you send a prompt to an AI model or instruct an agent to do something for you, the latency for the operation can vary greatly, and in ways that can be difficult to predict from the outset. Some of this can be put down to variability in the performance of the provider at a given point in time, but it can also be the result of rate limiting by the provider, or your prompt could just be taking longer to process.

In situations where the prompts sent to models come directly from your users, then you will likely have even less certainty about how long it should take for a model to come back to you. Has a call hung, or is it expecting a large output?

Some techniques to solve this can include dynamically setting timeouts based on the number of tokens being generated, or monitoring the streaming output from a model so you can track when the response starts coming back rather than waiting for the full response.

## 5\. How to handle uncertainty

*Excerpt from Chapter 7: Resiliency and AI*

In the previous sections, I’ve outlined why the world of AI is so uncertain right now. However, I want this book to focus on practical, actionable advice. With that in mind, I want to outline some key steps you can take to make sure that you’re prepared for the uncertainty ahead.

### Multimodel, Multivendor

With model behavior, speed, and quality changing from version to version, it’s vital that you build your software to support changing models. Not only do you want the ability to change models to make use of the latest and greatest, but if costs start to increase from one vendor, you want to make sure the barrier to finding another vendor isn’t too great.

It is worth highlighting that mixing models from different vendors in a single flow can exacerbate issues. For example, a flow that involved models from Anthropic, OpenAI, and Google’s Gemini would be impacted if any of those models had availability issues. All those models have to be available and functioning correctly at the time the flow is executed. Also, prompts that work against one model may not be entirely portable to another—a prompt tuned against one model will have to be reevaluated when moving to a different model, even if you stay within the same vendor ecosystem.

Taking a multimodel approach extends to your vendors too. If a vendor you are dependent on is in turn dependent on a single vendor’s model or API, you and your vendor become vulnerable to volatility in that model or to being impacted by a sudden price increase. Cursor was forced to pass on a significant cost increase to its customers in 2025, as it relied on the use of Anthropic’s services, and Anthropic changed its pricing structure, which led to Cursor having to pay much more. Coincidentally, Anthropic’s Claude Code is a direct competitor of Cursor. If Cursor had been able to use AI models from another firm, it might have been less vulnerable to this issue.

**Keep Your Options Open**: Make sure you aren’t wholly dependent on a single model or AI platform. If model quality degrades, or prices from OpenAI or Anthropic go up, you want to make sure that you have the flexibility to go elsewhere.

### Cost Transparency

Given concerns about rising costs from vendors, and given that pay-as-you-go pricing models can lead to surprising and unsustainable bills, having the ability to see on a regular basis how much money you are spending, and where, can be really important. This doesn’t need to be about limiting spend necessarily; it can be about understanding how you spend money and understanding what value that brings.

James Stanier, CTO at digital healthcare firm Nordhealth, has advocated for being able to see spend on a per-person basis. Rather than this being about shaming people who spend too much money, he makes the case that this visibility drives better understanding of how the software is being used and helps drive the adoption of tools that are proving to be valuable. In his 2025 talk at LeadDev Berlin, “LLMs: An Operator’s View”, Stanier explains that if you see a person spending a lot of money, this shouldn’t be seen as a bad thing, as typically it showsomeone trying to get more done. He cites an example of one of his QA engineers blowing through €1,000 in a single week on Claude Code, which was a much higher spend than normal. After pairing with the engineer in question to understand where this money was going, Stanier was able to see that this spend allowed the engineer in question to more than double his productivity.

Having the information about spend available to you, and being able to talk to people about how they use these tools and the benefits (or problems) they cause, will help you all make much better and more informed decisions.

It can also help you spot egregious overspend. Uber recently revealed that it had burned through its entire annual AI budget in just the first four months of the year, and Microsoft announced it was pulling Claude licenses from its employees, due in part to cost, apparently.5 These stories might cause one or two raised eyebrows, but they pale in comparison to the recent (admittedly unsubstantiated) report from Axios that an unnamed enterprise company spent $500 million on Claude licenses after failing to put cost controls into place.

**Tip**: Make your AI spend visible! It can help you understand how people are using it and also help you spot when the bills start climbing precipitously.

### More Frequent Vendor Analysis

Vendor management is a common activity that many firms carry out. It typically consists of a regular review of the main vendors that the organization makes use of to help it make better decisions in the future. For example, seeing that a large amount of money is being spent with a particular vendor despite the quality of service from said vendor degrading might cause an escalation to either get the price reduced or get the quality of service improved—or it might kick off a process to find a replacement.

In my last role at Thoughtworks, I helped lead our internal technology operations. Although my main job was to help provide support to internal delivery teams, I was also involved from time to time with our own vendor management activities. As part of these activities, we would regularly assess the general health of the vendors themselves. During the last review I was involved with, we were actively concerned about the health of one of our main technology suppliers; the supplier was in a very competitive market, and its market share was dropping. Although we were happy with the quality of service it provided (and with the cost), our fear was that wider market forces would drive it out of business. As a result, we decided to get to know the vendor much better but to also prepare a list of alternative solutions that we could select if the vendor in question didn’t survive.

Given the degree of uncertainty around the AI sector in general, I’d recommend regular vendor management activities. If you already have a normal vendor analysis cycle, consider increasing its frequency for AI vendors. If possible, find out if any vendors are in turn dependent on another provider, as in the case of Cursor being dependent on Anthropic’s cloud services. I’d be more concerned about a vendor that is in turn wholly dependent on a single company than I would be about a vendor that has more flexibility about whose services it uses. If you’ve never done vendor management before, consider instituting a lightweight process so you can build up a better understanding of how exposed you are to risk in this space.

With the concerns around the potential security vulnerabilities that come from the use of AI, I’d also recommend keeping your threat model up-to-date with regular threat assessments. As with vendor reviews, consider increasing the frequency of your normal threat assessments in areas with heavy use of AI, as this technology and how we use it is changing very quickly.

**Review Your AI Vendors Regularly**! Given the volatility in the space, make sure you review your AI vendors much more frequently than normal vendors.

### Summary

I realize that this chapter may have ended on somewhat of a negative note. But I wanted to be clear that we have real, practical use cases for AI in helping us build more resilient distributed systems. Most of the use cases I outlined in this chapter are being used daily by people all over the world. The key thing is that the solutions I have shared here still put humans at the heart of decision making. It’s clear that, due to the challenges I have covered here as well, we are a long way from a truly autonomous AI SRE.

Despite the obvious benefits of using AI, we do need to be fully aware of the downsides at both the micro and macro level. At the micro level, LLM-powered AI requires us to be OK with nondeterminism, whereas there are many activities we perform in the context of building resilient systems that need to be fully repeatable and deterministic. At the macro level, concerns about the AI sector as a whole mean we are dealing with an ever-changing (and hopefully) improving set of products built on very shaky financial foundations. It doesn’t matter how good the tooling is if it’s too expensive to use (or the company making it goes bust).

As is the nature of things, this uncertainty around AI’s role in our industry and society as a whole will fade as we learn more and as the market settles down. But be warned: I think we are still very much in the middle of the storm, and the calm waters may be several years away yet.

A lot of this chapter was about uncertainty, and one thing that can make the resiliency of a system quite uncertain is what happens when machines fail. So in Chapter 8, I’ll explore how to make sure our systems still work even when machines let us down.

## Takeaways

*Gergely, again.*

Thanks to Sam for sharing this excerpt of the book! While this was a long article in itself, we covered less than one of 16 chapters in the book. I found the book an excellent read, and recommend getting it if you’re building production systems:

[Get the ebook or print book](https://www.oreilly.com/library/view/building-resilient-distributed/9781098163532/)

**My sense is that AI is making “resiliency basics” more important than before**. Most of the book is not about AI – in fact, AI is barely mentioned in many of the chapters. But as AI agents generate more code and allow building software faster than before: we now have more software (or more complex software!) to operate.

And while there are AI tools and vendors that promise operating software to be easier with AI: I don’t really buy that it is, at least not at scale. Having the right monitoring and alerting in place is becoming more important, as is responding to outages, and being aware of tricky situations like thundering herds, retries, idempotency, timeouts, and many others.

**Preparing for disaster recovery seems like a practical thing to do when your service is important enough**. I wanted to include excerpts on disaster recovery, because I observe plenty of “disasters” (that is, widespread outages) happening across the industry where recovery time becomes surprisingly long. And some of this could be because the team had not prepared for, or exercised, disaster recovery beforehand.

A good example is how Coinbase’s global trading service [went offline](https://newsletter.pragmaticengineer.com/i/201622200/3-reliability-fail-no-automated-zone-failover-for-coinbases-global-trading-service) after a single AWS zone had problems. As I summarized the incident [in The Pulse](https://newsletter.pragmaticengineer.com/i/201622200/3-reliability-fail-no-automated-zone-failover-for-coinbases-global-trading-service), back in June:

> “Having no automated failovers is incredibly amateurish for an operation of Coinbase’s scale. Coinbase moves about 5.2 trillion dollars per year, and is valued at around $40B. The outage interrupted around $7 billion-worth of financial activity, based on my napkin math.”

It’s smart to learn from the mistakes of others, when we can: and the Coinbase incident felt like one of these.

I hope you found these topics relevant, and thanks again to Sam for writing the book, and sharing relevant parts with us.
