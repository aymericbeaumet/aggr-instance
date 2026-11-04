---
title: The Internals of PostgreSQL
link: https://www.interdb.jp/pg/index.html
source: hnrss-org
published: 2026-11-03T21:51:06Z
updated: 2026-11-03T21:51:06Z
first_seen: 2026-11-04T07:26:52.515125251Z
authors:
- ibobev
content: extracted
html: 2026-11-03-the-internals-of-postgresql.html
remote_preview:
  url: https://www.interdb.jp/pg/title3.png
---

![](https://www.interdb.jp/pg/title3.png)

I am officially open to new international opportunities! 🌍

After a temporary stay in Japan to focus on my health and personal responsibilities, I am fully recovered, ready, and excited to work abroad again.

I am highly interested in returning to Europe, as well as opportunities in the Southern Hemisphere and Southeast Asia.

Looking for:

- Challenging tasks in PostgreSQL.
- Roles offering international visa sponsorship OR Remote work that supports a digital nomad lifestyle.

If your team is hiring, let’s connect! Please feel free to reach out via e-Mail(hironobu @ interdb.jp or interdb.mx @ gmail.com) or [DM](https://www.linkedin.com/in/hironobu-suzuki-03b69015/).

###### What’s New

- 26 Sep 2026: 11th Anniversary! This content was launched on 26 September 2015.
- 28 Jun 2026: [Section 8.5, “Asynchronous I/O in PostgreSQL”](https://www.interdb.jp/pg/pgsql08/05.html) Published.
- 1 May 2026: [Chapter 12, “Logical Replication” (Beta Version)](https://www.interdb.jp/pg/pgsql12/index.html) Published.
- 14 Dec 2024: Added Conflicts in [Section 11.2.4](https://www.interdb.jp/pg/pgsql11/02.html#11-2-4).

Change History (since 3rd June, 2018)

### The Internals of PostgreSQL

PostgreSQL is a well-designed, open-source multi-purpose relational database system which is widely used throughout the world.

It is one huge system with the integrated subsystems, each of which has a particular complex feature and works cooperatively with each other. Although understanding of the internal mechanism is crucial for both administration and integration using PostgreSQL, its hugeness and complexity make it difficult.

The main purposes of this documentation are to explain how each subsystem works, and to provide the whole picture of PostgreSQL.

This documentation covers versions 19-beta (excluding the new buffer page lock mechanism) and earlier.

[Some academic papers](https://www.google.com/search?q=%22interdb.jp%22+site:acm.org) have referred to this documentation. The [Chinese version](https://item.jd.com/12527505.html) of this documentation was published in June 2019.

This documentation was first published on 26 September 2015.

Contents

- [Chapter 1](https://www.interdb.jp/pg/pgsql01/index.html). Database Cluster, Databases and Tables
- [Chapter 2](https://www.interdb.jp/pg/pgsql02/index.html). Process and Memory Architecture
- [Chapter 3](https://www.interdb.jp/pg/pgsql03/index.html). Query Processing
- [Chapter 4](https://www.interdb.jp/pg/pgsql04/index.html). Foreign Data Wrappers (FDW)
- [Chapter 5](https://www.interdb.jp/pg/pgsql05/index.html). Concurrency Control
- [Chapter 6](https://www.interdb.jp/pg/pgsql06/index.html). VACUUM Processing
- [Chapter 7](https://www.interdb.jp/pg/pgsql07/index.html). Heap Only Tuple (HOT) and Index-Only Scans
- [Chapter 8](https://www.interdb.jp/pg/pgsql08/index.html). Buffer Manager
- [Chapter 9](https://www.interdb.jp/pg/pgsql09/index.html). Write-Ahead Logging (WAL)
- [Chapter 10](https://www.interdb.jp/pg/pgsql10/index.html). Online Backup and Point-In-Time Recovery (PITR)
- [Chapter 11](https://www.interdb.jp/pg/pgsql11/index.html). Streaming Replication\
- [Chapter 12](https://www.interdb.jp/pg/pgsql12/index.html). Logical Replication (Beta Version)

###### Hironobu SUZUKI

I am a software programmer/engineer, the author of:

- [The Internals of PostgreSQL](http://www.interdb.jp/pg/)
- [The Engineer’s Guide To Deep Learning](http://www.interdb.jp/dl/)
- [pg\_plan\_inspector](https://github.com/s-hironobu/pg_plan_inspector)
- [pg\_tuner](https://github.com/s-hironobu/pg_tuner)

I graduated from graduate school in information engineering (M.S. in Information Engineering), have worked for several companies as a software developer and technical manager/director. I published seven books in the fields of database and system integration (4 PostgreSQL books and 3 MySQL books). In June 2019, the Chinese book of this documentation was published.

As a director of the Japan PostgreSQL Users Group (2010-2016), I organized the largest (non-commercial) technical seminar/lecture on PostgreSQL in Japan for more than six years, and also served as the program committee chair of the Japan PostgreSQL Conference in 2013 and as a member in 2008 and 2009.

In June 2022, [my interview article](https://postgresql.life/post/hironobu_suzuki/) was published in “PostgreSQL person of the week”.

Cuando era joven, vivió en Sudamérica por unos años. Recientemente, a veces vuelve a allí.

I am seeking a remote position focused on the application of ML and AI technologies to DBMS. For personal reasons, I will be living in Japan for several years.

      [Blog](https://www.interdb.jp/blog/)

#### Contact

Please read the following FAQ before sending messages.

FAQ

Since publishing my content, I’ve been fortunate to receive a lot of positive feedback, which is truly gratifying. However, unfortunately, I’ve encountered individuals in the past who tried to misuse my content for self-promotion [^1].

These experiences have shaped the approach I’ve outlined below.

1. Who can use this documentation freely?\
    If you are a teacher or a student belonging to an educational organization, you can freely use this documentation and figures in your study. Anyone can use this documentation and figures with noncommercial meetings and lectures, if you state the link to this site and the copyright; otherwise, contact me.
2. Is it available for commercial contents?\
    This content can be used under two options:\
   There was a comment on Hacker News that took this seriously, but of course, it’s a joke. To be clear, I have no intention of having any commercial ties to this. Regardless, you can imagine the kind of requests I get on a daily basis.
   - Revenue Share: You can leverage this content after a revenue share agreement is signed. Under this agreement, you’ll share 20% of the sales generated from using this content.
   - Full Buyout: In very rare cases, I consider requests for full commercial use of all content on this site. For a complete buyout of all content rights, the cost is €5,000,000.
3. Does the author need any help to write?\
    No. I am writing for my own enjoyment. Even if you send me your article, I will never include it in my documentation.
4. Why doesn’t the author use the GitBook or other e-book formats/sites?\
    I am using the best tools I need and I will decide what I use. I am always trying a lot of tools for better explanations. \
    Simply put, this documentation is optimized to read on html file and it is hard to convert to other formats. \
    I do not have any plan to make PDF version and Smartphone versions because of same reason.
5. Why doesn’t the author use RSS to notify the update?\
    I keep my domain and my server to provide my documentation; you can read when you want to read. What do you want more?
6. Why doesn’t the author waive the copyright of this documentation or use the creative commons license?\
    I’d like to ask you what problems you have by that I keep on having the copyright of my documentation.

After reading, send a message to my twitter in public.

or

If you use email, please **provide at least two SNS addresses (e.g. LinkedIn, Twitter) for verification purposes**. Due to the XZ backdoor incident, I no longer accept contact from anonymous individuals.

#### Copyright

© Copyright ALL Right Reserved, Hironobu SUZUKI.

For any inquiries regarding the use of this documentation or any of its figures, please contact me.

 Exception Educational institutions can use this documentation freely.

[^1]: See the source code.
