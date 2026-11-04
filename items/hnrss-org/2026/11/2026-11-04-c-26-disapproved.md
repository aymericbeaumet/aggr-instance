---
title: C++26 Disapproved
link: https://andreasfertig.com/blog/2026/11/cpp26-dis-failed-how-does-cpp-survive/
source: hnrss-org
published: 2026-11-04T00:33:22Z
updated: 2026-11-04T00:33:22Z
first_seen: 2026-11-04T07:26:52.515125251Z
authors:
- thatxliner
content: extracted
html: 2026-11-04-c-26-disapproved.html
remote_preview:
  url: https://andreasfertig.com/img/2026-11-03-cpp26-failed-li.png
---

On October 23, 2026, the unthinkable happened. It was supposed to be a mere formality: C++26 was up for a vote by the ISO National Bodies (NBs). The options are always approval, rejection, or abstention. Comments are also allowed. Since 1998, this process had run quietly and smoothly. Not so on October 23, 2026. After a twelve-week voting period, the result is in: **Disapproved**!

*I would like to mention here that I am a member of the C++ Standardization Committee and, together with my colleagues, represent Germany’s vote. ISO rules also stipulate that certain votes are internal to ISO, and the results—or the breakdown of the results—may not be published. This article contains my own personal views.*

## The Current Status

As far as I know, this is the first time in the history of C++ that DIS ballot failed. When I searched the ISO system, I could only find one other DIS that was not approved: ISO/IEC DIS 26926 C++/CLI Language Specification dated August 11, 2006. It’s possible that my search privileges aren’t sufficient to view everything, but all ISO programming languages should be visible to me. The sad end of ISO/IEC DIS 26926: the project was halted. I mention this upfront to clarify the significance of this vote.

In the following, I will use various terms from everyday ISO terminology. At [https://isocpp.org/std/iso-iec-jtc1-procedures](https://isocpp.org/std/iso-iec-jtc1-procedures), there is an overview of the various stages of an ISO standard. DIS is the penultimate stage. As the overview also shows, a two-thirds majority is required. This majority was not achieved.

The reason for the rejection is quite clear from the comments submitted by the National Bodies (NBs): contracts. Put simply, a number of NBs believe that this feature is not yet ready for release.

## The Process

Now it gets complicated. The C++ Standardization Committee, also known as WG21, follows ISO rules but also has some additional rules. One of these is (or was) that opinions are always heard whenever new information becomes available. If an opinion was heard and a feature was nevertheless not changed as desired, the person respects the outcome and accepts that many others disagree.

However, the voting body at the NB level may also consist of other individuals, or the NBs are, of course, free to vote differently.

The previous practice was that NBs would only vote against a standard if new information became available or if errors in the process or similar issues were identified. This changed on October 23, 2026.

It’s difficult to judge in this complex situation; the NBs who want C++26 without Contracts argue that there is new information. I myself find it hard to share this view. A new paper with old arguments is not new information to me.

## The History of Contracts in C++

The feature has been on the wish list for a long time. I was already in the room in Cologne back in 2019, at the time for C++20, when Contracts, in a different form back then, were being included in the standard. I was also in the room when the feature was removed from C++20 again during the same meeting. As a result, a study group was formed to address the topic of Contracts and its various aspects from 2020 to approximately 2024. Afterward, the study group submitted its work and proposals to the Evolution Working Group (EWG), the central body for language extensions. There, the topic was discussed further over several meetings before being included in C++26 by a majority vote.

In the next step, WG21 voted on the C++26 draft in March 2026, which included Contracts. Here, too, a large majority was achieved, though not without dissenting votes.

And that brings us to the next phase: DIS. At this stage, there were enough “no” votes that a two-thirds majority was not reached.

## What happens next?

The big problem: what happens next? ISO, of course, has a process for this. It offers various options:

1. Submit a *revised* draft for a new DIS vote.
2. Submit a *revised* draft to the committee (that would be the CD phase).
3. Publish a *revised* draft as a Technical Specification.
4. Cancel the project.

All of these possibilities mean a delay in the next C++ standard. Whether it will still be called C++26 is also up in the air, but that’s a much smaller issue.

The difficulty with all four options is that they require a majority within WG21. However, this working group voted to move C++26 *with* Contracts into the next voting phase leading to its release. And it did so with more than just the required two-thirds majority. The feature didn’t come out of nowhere, either; it was developed over the course of years through the work and possibly personal commitment of many people. It’s hard to imagine that this group would simply want to abandon its work, which has received approval on multiple occasions so far. The same applies to the group that is dissatisfied with Contracts. This group, too, has invested effort, in some cases even in alternative implementations. It’s hard to imagine that, after these groups voted “no” in the DIS vote, the result would change without Contracts being removed.

Even the last option *removing C++26* requires a majority in WG21. As I understand it, this step would mean reverting to C++23 and proceeding to C++29 without a single change. This would affect the work of even more people. WG21 would then be tasked with incorporating features that have already been successfully included in C++26 into C++29. Since this can only be done through a paper backed by at least one person advocating for it, features could be omitted if that person lacks the motivation or time.

Of course, certainty about the voting outcome can only be achieved if the respective path is actually taken.

Unfortunately, the way forward is currently more than uncertain. Perhaps this also explains why ISO/IEC DIS 26926 marked the end of this activity. Consensus is needed to move forward. Trust is always required for consensus. Unfortunately, that trust has just been shaken.

The next C++ standard, and thus the future of C++, is currently at an impasse.

The good news is that the next meeting will take place on November 16, which is just a few days away. The top priority will be to break the deadlock. This will very likely have implications for C++29 as well; in fact, there is currently no reason to continue working on this project.

## My Personal Opinion

Normally, my articles are technically focused and free of personal opinion. Regarding the rejected DIS, I can only say that I consider it a massive disaster and feel sorry for the entire C++ community. Regardless of whether someone likes Contracts, considers them important, or sees major weaknesses in them, the topic has been worked on for years and has received a majority vote multiple times in its current form. By voting “no,” an established system, one based primarily on trust and respectful collaboration, has been shaken. Every ISO standard is simply a set of compromises. Everyone loses sometimes, and everyone wins sometimes. Overall, the result is one in which all participants have given their best. During the DIS vote, the losing parties refused to accept their loss. The question that primarily determines *how to move forward* is whether the group can rebuild trust.

Andreas
