---
title: 'Bliki: Generated Glossary'
link: https://martinfowler.com/bliki/GeneratedGlossary.html
source: martinfowler-com
published: 2026-11-12T14:15:00Z
updated: 2026-11-12T14:15:00Z
first_seen: 2026-11-12T18:49:11.379952736Z
authors:
- Martin Fowler
labels:
- bliki
content: extracted
html: 2026-11-12-bliki-generated-glossary.html
remote_preview:
  url: https://martinfowler.com/logo-sq.png
---

When we use LLMs in a specialized domain, such as the operations of any reasonably-large organization, there is a lot of specialized vocabulary. Such words and specialized meanings will be foreign to an LLM because they aren't in its training set, making it harder for the LLM to be effective when doing tasks in that domain. One technique to aid this is to use an LLM to generate a glossary of the terminology in that domain, and make this glossary available for any LLMs doing work in those circumstances.

The sources for the LLM can be any material within the organization concerned such as documents, emails, and chat logs. In some cases getting transcripts of audio or video may be helpful, although most organizations generate enough text to do the job.

The glossary-generating LLM should look for single words, and phrases of several words that regularly hang out together to form a meaningful term. The generator should be asked to identify terms that are new to it, but also known terms that are being used in unfamiliar ways. The latter can often cause the most confusion.

Once we have a list of specialized vocabulary we can ask the agent to produce a glossary, with definitions for each term. Some terms may be used in different ways, in which case the LLM should provide alternative definitions, with a note as to what context the differences appear in. The LLM can be asked to follow the output format of a dictionary, with a focus on precision and conciseness.

Once we have a draft glossary, then human experts in that domain should review it, and amend it for cases where the LLM has got the wrong ends of sticks.

We then put the glossary somewhere where any other LLM can use it. It should be in a form that's easy for the LLM to consult, and minimize the tokens the LLM should consume in order to find meaning. We should set up a schedule to review and refresh the glossary at regular intervals, by agents and humans. We should keep it in version control, so we can see how it changes over time.

Large organizations often have subtly different definitions in different places, essentially following the [Domain Driven Design](https://martinfowler.com/bliki/DomainDrivenDesign.html) notion of [Bounded Context](https://martinfowler.com/bliki/BoundedContext.html). Source code can be a particularly interesting bounded context, allowing folks to understand how terminology is used within a code base. Sometimes a single glossary can span several of bounded contexts, but at other times it's better to have separate glossaries for each one, allowing LLMs to consult whichever glossaries that are relevant to what they are doing. I'd urge everyone to resist any temptation to create a single, standard, glossary - I've yet to see that work. Often the value of a glossary isn't to nail down a single definition of a term, but instead to highlight its different usage in different contexts.

When there are different bounded contexts it can be interesting to compare different glossaries, or to get LLMs to compare glossaries and describe differences in usage. Comparing a source-code derived glossary to a human text can be particularly instructive.

If the glossary is large, it can be worthwhile to only include the terms and links to definitions as part of the initial context. The model can then use the links for only the full definitions it needs while working.

A glossary can be used in several ways within [Agentic Engineering](https://martinfowler.com/bliki/AgenticEngineering.html). Glossaries can inform generation of code, so that names within the generated software fit the particular vocabulary of the enterprise. [Sensors in a harness](https://martinfowler.com/articles/harness-engineering.html) can check for this usage, both in the internals of the software and in the user-interface.

When generating code or documents, we may introduce new bounded contexts. For example, internal names that may be used within an organization should not be exposed in a customer-facing UI, because such internal terms are likely to be only confuse a customer. In this case a customer has a different bounded context to the part of the enterprise serving them.

It's often useful for glossaries to include terms that should not be used, in which case it includes advice on what other term to use instead. Terms that are deprecated can be handled in this way, with glossary entries that indicate what period of time a term should be in effect.

In my conversations with people working with agents in large organizations, a common theme is the difficulty of specialized language. This encourages the activities of domain modeling, building ontologies, and constructing explicit [context layers in enterprise data](https://martinfowler.com/articles/making-data-ready-for-agentic-ai.html#context-layer). Getting LLMs to generate a glossary is a simple, but useful, part of this work.

## Acknowledgements

Eric Nagler and Unmesh Joshi related their experiences doing this activity and thus inspired this post. Amy Pellegrini, Annie Luxton, Arun Srinivasan, Chris Ford, Daniel Terhorst-North, Dave Farley, Eric Nagler, James Shore, Jason Yip, Jimmy Nilsson, Matteo Vaccari, Michaël Le Barbier, Pete Hodgson, Pramod Sadalage, Ravi Vats, Unmesh Joshi, and Valentyn Logvynskyi commented on drafts.
