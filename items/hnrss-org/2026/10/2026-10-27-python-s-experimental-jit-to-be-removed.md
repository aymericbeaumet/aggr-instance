---
title: Python's experimental JIT to be removed
link: https://lwn.net/SubscriberLink/1102102/0c00e4d550f25f8c/
source: hnrss-org
published: 2026-10-27T11:17:57Z
updated: 2026-10-27T11:17:57Z
first_seen: 2026-10-27T16:00:47.207930047Z
authors:
- f311a
content: extracted
html: 2026-10-27-python-s-experimental-jit-to-be-removed.html
remote_preview:
  url: https://static.lwn.net/images/logo/barepenguin-70.webp
  alt: LWN.net Logo
---

\[+\] Proceed to the article

Python's [experimental just-in-time (JIT) compiler](https://docs.python.org/3.13/whatsnew/3.13.html#an-experimental-just-in-time-jit-compiler), which was added to CPython in 2024 for the [3.13 release](https://www.python.org/downloads/release/python-3130/), is on its way out. In June, the Python steering council [announced](https://discuss.python.org/t/an-announcement-from-the-steering-council-regarding-the-jit-project/107638) that it was pausing work on the JIT in CPython's main branch until there was an accepted [Python Enhancement Proposal](https://peps.python.org/pep-0001) (PEP) that addressed concerns about maintenance, compatibility, and set clear success metrics as well as a timeline for completion. After creating [PEP 836](https://peps.python.org/pep-0836/) ("JIT Go Brrr: The Path to a Supported JIT Compiler for CPython") to try to meet the council's request, and after months of discussion, the JIT maintainers have withdrawn the PEP. For now, it appears that development on the once-promising project has come to an end.

The JIT was [added to CPython by Brandt Bucher](https://github.com/python/cpython/pull/113465) in early 2024. Generally, such a major feature would be expected to land only after the council had approved a [standards-track PEP](https://peps.python.org/pep-0001/#pep-types:~:text=A%20Standards%20Track%20PEP). Bucher had given a talk ([YouTube video](https://www.youtube.com/watch?v=HxSHIpEQRjs), [slides](https://github.com/brandtbucher/brandtbucher/blob/master/2023/10/10/a_jit_compiler_for_cpython.pdf)) about the JIT at a Python core developer sprint, and [announced his intent](https://github.com/python/cpython/pull/113465#issuecomment-1911279536) to merge the feature—but he had not taken the step of authoring or submitting a PEP for its approval. He was [prodded into doing so](https://github.com/python/cpython/pull/113465#issuecomment-1969415137) by steering council member Thomas Wouters, who was also serving as the release manager for Python 3.13.

Bucher and Savannah Ostrowski drafted [PEP 744](https://peps.python.org/pep-0744/) ("JIT Compilation"), which was an [informational PEP](https://peps.python.org/pep-0001/#pep-types:~:text=An%20Informational%20PEP%20describes%20a%20Python%20design%20issue)—meaning it only provides information to the Python community and does not represent formal consensus to proceed on a feature or project. LWN [covered](https://lwn.net/Articles/970397/) the discussion of PEP 744 in April 2024, and later covered Bucher's [talk on the JIT](https://lwn.net/Articles/977855/) at PyCon US 2024, as well as the [follow-up talk](https://lwn.net/Articles/1029307/) at PyCon US 2025.

#### PEP it up

Work on the JIT had been proceeding on the basis of PEP 744 until this June, when the council had put forward its formal request for the JIT maintainers to create a standards-track PEP. One might wonder, [as Antoine Pitrou did](https://discuss.python.org/t/an-announcement-from-the-steering-council-regarding-the-jit-project/107638/2), why the council had decided that it was time to speak up and ask for a new PEP. Wouters [said](https://discuss.python.org/t/an-announcement-from-the-steering-council-regarding-the-jit-project/107638/4) that there was no "single triggering event for this request", but that the council had been discussing the topic for a while and indicated that it was looking to ensure the JIT work did not interfere with other projects. "There's been work trying to get the JIT to play nice with instrumentation, debugging, and profiling. There's also external JITs (like CinderX), JIT-adjacent things (like Numba and PyTorch's compile), and discussions about a tier 3 JIT." The council had waited until after the Python 3.15 beta release "to be clear that we're OK with the status quo for 3.15".

The council asked for a PEP that would make the case for the JIT "as a supported, non-experimental part of CPython: its guarantees, its maintenance commitments, and its impact on redistributors". The council set a deadline of six months for the PEP to be submitted and accepted; failing that, the council would require the JIT code to be removed from the CPython main branch.

Bucher, Ostrowski, and Ken Jin drafted PEP 836 and [published it on the Python forum](https://discuss.python.org/t/pep-836-jit-go-brrr-the-path-to-a-supported-jit-compiler-for-cpython/108010) for community feedback on July 3. It generated quite a bit of discussion, some favorable and some critical. One of the key themes in the comments, such as [this one](https://discuss.python.org/t/pep-836-jit-go-brrr-the-path-to-a-supported-jit-compiler-for-cpython/108010/19) by Wouters, and [this comment](https://discuss.python.org/t/pep-836-jit-go-brrr-the-path-to-a-supported-jit-compiler-for-cpython/108010/28) by Mark Shannon, was that the draft felt like it should be two separate PEPs. Shannon, for example, thought that the PEP should only cover goals for accepting the JIT work and felt that it should leave out implementation details. There were also quite a few calls for additional information.

Bucher [pushed back](https://discuss.python.org/t/pep-836-jit-go-brrr-the-path-to-a-supported-jit-compiler-for-cpython/108010/21) a bit on the suggestion that it should be two PEPs, and Ostrowski [defended](https://discuss.python.org/t/pep-836-jit-go-brrr-the-path-to-a-supported-jit-compiler-for-cpython/108010/22) the structure of the PEP as it was. "Outlining goals like we have in the PEP without being able to share the plan and everything we've thought through alongside it would make me uncomfortable. To me, goals without plans behind them aren't really goals at all."

When we [last looked at this topic](https://lwn.net/Articles/1090385/) in September, the discussion about the PEP had slowed down without a commitment to develop new PEPs or a formal statement from the council. The last word from the council at that time, in the form of a [summary of its August 6 meeting](https://discuss.python.org/t/2026-08-06-python-steering-council-meeting-summary/108612), was that it had met with the PEP authors and was "discussing the state of the PEP and whether it answered the questions the SC asked in its request for a PEP".

#### Experimentation in main

On September 21, Wouters [posted](https://discuss.python.org/t/pep-836-jit-go-brrr-the-path-to-a-supported-jit-compiler-for-cpython/108010/53) a statement from the steering council, minus Ostrowski (who is currently a member of the steering council but did not participate since she is a co-author of the PEP), that summarized its position. The statement said that PEP did answer all of the questions that were highlighted in the council's request, "although perhaps some less convincingly than others".

The council wished to make clear that it was not looking for the criteria that the JIT should meet to be a supported feature, or whether it should be considered such. Instead, the council was evaluating whether the JIT should continue as an experiment in CPython's main branch.

> Part of the problem with allowing experimentation in main is the lack of unified design, and the uncertainty that core developers, package maintainers and users face as the experimentation changes behaviours and API surfaces.

The council said that the PEP was not specific enough about support plans, performance criteria, and too vague or too optimistic about a number of other areas. The statement included a ten-point list of items that the council wanted "to see either spelled out in the PEP, or at least discussed and rejected". The council's list expressed concerns ranging from memory impact to security. For example, on the topic of platform parity, the council wanted to ensure that the JIT was designed to be easily adopted for new platforms. It wanted assurance that the JIT's design would not be tied to specific platform or require platform-specific knowledge to maintain.

Another area that the council called out was user impact. It said that the PEP was "overly optimistic" about user-visible differences with the JIT enabled. There were many cases where users depend on idiosyncrasies in the interpreter design, "as we've seen with other Python implementations or even just between major Python versions", and it wanted clarification on how those differences would be handled. Would they be considered bugs, how would situations be handled if a difference in behavior was unavoidable, and so on.

Security was "an underserved area in the PEP", according to the council; it suggested that a discussion with the [Python security response team](https://devguide.python.org/security/psrt/) was in order and that a security review should be part of the criteria for supported status as well.

PEP 836 specified a [compatibility review](https://peps.python.org/pep-0836/#compatibility-review) milestone to correspond with the first Python 3.17 release candidate—which would be expected sometime in August 2028—that would involve running the test suites of the top PyPI packages to see if the JIT broke them. The council wanted that timeline moved up significantly. "We think that's way, way too late, and the compatibility review should be an ongoing and continuous process starting as soon as possible".

The council also worried about the JIT's design decisions being made "without critical review by the larger group of developers ultimately responsible for \[CPython\] development, maintenance and support". The bottom line, it said, was that PEP 836 "mostly answers the questions we asked" with no missing pieces, but it wanted more discussion on those topics so it could "gauge community support and consensus before we're asked to pronounce on this PEP".

Jin [replied](https://discuss.python.org/t/pep-836-jit-go-brrr-the-path-to-a-supported-jit-compiler-for-cpython/108010/55) a few days later to thank the council for its detailed feedback and said that the group would work on its suggestions.

#### Withdrawn

After some consideration, it appears, the PEP's authors have decided not to continue pursing the council's approval. On October 19, Ostrowski (who is also release manager for Python 3.16 and 3.17) [announced](https://discuss.python.org/t/pep-836-jit-go-brrr-the-path-to-a-supported-jit-compiler-for-cpython/108010/57) that the PEP was being withdrawn:

> The Steering Council rightly sets a high bar for a proposal of this scope, and we don't feel we have the personal bandwidth to carry the current version forward. We continue to believe in the technical direction outlined in the PEP, including free-threaded support and a method-based frontend, and remain optimistic about the JIT's potential in CPython. We'll make the reference implementation available and link it from the withdrawn PEP so others who are interested can explore and build on that.

Barry Scott [asked](https://discuss.python.org/t/pep-836-jit-go-brrr-the-path-to-a-supported-jit-compiler-for-cpython/108010/59) if that meant that work on the JIT was stopped. Brett Cannon [replied](https://discuss.python.org/t/pep-836-jit-go-brrr-the-path-to-a-supported-jit-compiler-for-cpython/108010/61) that the work would be stopped in the main repository, but it would be up to the community whether the work would continue elsewhere. Dunc Graham [suggested](https://discuss.python.org/t/pep-836-jit-go-brrr-the-path-to-a-supported-jit-compiler-for-cpython/108010/62) that there might be time for an alternative PEP to be submitted before the council's deadline. Cannon [said](https://discuss.python.org/t/pep-836-jit-go-brrr-the-path-to-a-supported-jit-compiler-for-cpython/108010/63) that was not likely, "as all the people actively driving the JIT were either authors on that PEP or contributors to it".

With the PEP withdrawn, the JIT will likely be removed from CPython main fairly soon to ensure that any problems caused by its removal can be addressed early in the [Python 3.16 development cycle](https://peps.python.org/pep-0826/). It is unclear whether the original drivers of the JIT work intend to continue working on it at all, or if it will simply be left for others to pick up if there is enough community interest. I have emailed the PEP authors for clarification, but have not yet received a reply.

> **Did you like this article?**? [Subscribe now](https://lwn.net/Promo/slink-d-obnoxious/claim) at the special discounted rate to get a lot more like it.
