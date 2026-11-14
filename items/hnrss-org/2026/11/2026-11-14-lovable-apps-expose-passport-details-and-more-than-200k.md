---
title: Lovable apps expose passport details and more than 200K private messages
link: https://www.crissyfield.de/research/lovable-en/
source: hnrss-org
published: 2026-11-14T17:43:10Z
updated: 2026-11-14T17:43:10Z
first_seen: 2026-11-14T20:36:53.628480974Z
authors:
- armcat
content: extracted
html: 2026-11-14-lovable-apps-expose-passport-details-and-more-than-200k.html
remote_preview:
  url: https://www.crissyfield.de/images/research-lovable-opengraph.jpg
---

![Hero image](https://www.crissyfield.de/images/research-lovable-hero.jpg)

## Lovable: Many Apps Built on the Platform Expose Sensitive Data

**AI makes it easier to build your own web applications, but handling sensitive data still requires special care. We examined 159,041 Lovable projects and found many cases where personal and confidential information was accessible without logging in.**

[Diesen Artikel auf Deutsch lesen](https://www.crissyfield.de/research/lovable/)

![Logo](https://www.crissyfield.de/images/research-lovable-about.png)

## About Lovable

Lovable is a Swedish AI startup founded in Stockholm in 2023 by Anton Osika and Fabian Hedin. The platform turns plain-language descriptions into web applications and is designed to be accessible even to *people with little or no programming experience*.

Lovable grew out of Osika’s open-source project “GPT Engineer”, released in 2023. The project evolved into the GPT Engineer App before being renamed Lovable in late 2024 and launching publicly that November.

[Visit the Lovable website](https://lovable.dev/)

## The Promise

Lovable can build impressively sophisticated web projects with AI, from static websites to full applications with user management and database connections. But as soon as an application handles confidential information, one question becomes crucial: **Is that data protected against unauthorized access?**

[In its marketing](https://lovable.dev/blog/what-is-vibe-coding), [Lovable repeatedly](https://lovable.dev/blog/2025-01-30-why-non-tech-founders-no-longer-need-a-developer-to-launch-software) [emphasizes](https://lovable.dev/en/founders) [that no technical knowledge is needed to build projects](https://lovable.dev/vibecoding). This can give the impression that the platform takes care of security too. Yet Lovable’s Terms & Conditions place responsibility for applications and data handling on the customers themselves.

That leaves a troubling gap between the promise of an easy-to-use, all-in-one service and the responsibilities customers are expected to take on.

Against this backdrop, we investigated how well sensitive data in Lovable applications is protected, in collaboration with [Handelsblatt](https://www.handelsblatt.com/). [Lina Knees reported on the findings on November 2, 2026 (in German)](https://www.handelsblatt.com/technik/ki/lovable-experten-finden-datenluecke-bei-beliebter-ki-coding-plattform/100264923.html).

## Research Question

We wanted to find out whether sensitive data in Lovable applications was accessible without logging in. We looked separately at data sent when a website first loaded and additional content we checked by making further read-only requests with the same API key.

The result: **We found many clear examples of sensitive data left unprotected.**

## Methodology

We focused on the database service [Supabase](https://supabase.com/). Its integration is relatively easy to analyze, and we believe it is the most widely used database service among Lovable projects. Lovable’s own database features, introduced in late 2025, are still less widely used. Similar misconfigurations may occur there too, but we did not examine them in this analysis.

![Analysis Workflow](https://www.crissyfield.de/images/research-lovable-workflow-en.png)

Using datasets from the open web archive [Common Crawl](https://commoncrawl.org/), we identified and analyzed **159,041 Lovable projects** from *December 2025 to September 2026*. Of these, 34,545 used Supabase.

**24,664 websites** sent database records from Supabase as soon as they loaded, without requiring a login or any other interaction. That alone is not a security issue: the data may be intended for public display. But by making additional read-only requests, we were able to retrieve more database content. We found records containing email addresses in **10,560 tables** across **7,403** of the 24,664 websites (30%).

## Limitations

Email addresses can help identify records containing personal data, but they may also be public contact addresses or test data. At the same time, this approach misses personal records that contain no email address. These effects partly offset one another, though we could not establish to what extent. The 30% figure should therefore not be read as a confirmed data leak rate. It applies only to the 24,664 websites we examined more closely. Our sample, drawn from Common Crawl, is also not representative of all Lovable applications.

We retrieved only parts of the datasets rather than downloading them in full. The figures below reflect the records we actually downloaded; the affected datasets may contain more. We are not publishing details of how to access them, as doing so could make further access easier.

## Data Leaks

We found many serious cases of exposed data, including:

- A German lifestyle forum exposed **more than 80,000 profiles** containing full names, email addresses, dates of birth, and gender information. **More than 200,000 chat messages** were also accessible.

- Several event organizers and restaurants exposed **tens of thousands of records relating to ticket and food sales**, with no login required. The records we inspected included email addresses, phone numbers, postal addresses, payment information, and ticket codes.

- A property management company exposed AI-generated summaries of internal emails containing sensitive information about tenants, property owners, and business partners.

- In a particularly serious case, a service provider exposed **more than 1,000 records containing identity document details**, including **passport numbers**, **expiration dates**, and **nationalities**. These highly sensitive identity details could be used for identity theft.

- Other accessible content included tens of thousands of **contracts**, **invoices**, **personal photo albums**, and **internal WhatsApp communications**.

- We also found numerous newsletter subscriptions, waiting lists, and contact and support inquiries. In one case, **280,000 emails sent** to a provider’s customers were accessible.

Depending on what it contains, exposed data like this could be used for phishing, ticket fraud, or identity theft. We could not establish whether any of it had been misused.

Handelsblatt contacted Lovable with the findings of our investigation.

## Implications

Our analysis shows that many Lovable applications leave even highly sensitive data unprotected, with potentially serious consequences for the people affected.

Access control mistakes can happen in any application, whether written by people or built with AI. With Lovable, database integrations follow recurring patterns, making it easier for attackers to look for similar misconfigurations across many applications. An approach that works against one app may then work against others with little extra effort. Users without a technical background also face a harder task spotting and preventing these flaws.

An easy-to-use development platform does not remove the need for an expert review of access permissions before an application handles confidential data.

Automated checks **are no substitute for a professional security assessment**.

Before an application handles real customer or business data, its access permissions, interfaces, and separation between user accounts should be reviewed. These checks should be repeated after significant changes. If the necessary expertise is not available in-house, bring in qualified support.

If you would like help securing your application, please get in touch.

*Note: This research report presents a more recent analysis and updated figures compared with the Handelsblatt article published on November 2, 2026.*

![](https://www.crissyfield.de/images/talk-to-the-expert.svg)

Get in touch for an informal conversation. We'll look at your current situation and work with you to find the right solutions.

[Contact an expert](mailto:hello@crissyfield.de)
