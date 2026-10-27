---
title: Wikipedia's Cleanup of AI-Generated Content
link: https://spectrum.ieee.org/wikipedia-wikiproject-ai-writing-cleanup
source: hnrss-org
published: 2026-10-27T11:08:15Z
updated: 2026-10-27T11:08:15Z
first_seen: 2026-10-27T16:00:47.207930047Z
authors:
- rbanffy
content: extracted
html: 2026-10-27-wikipedia-s-cleanup-of-ai-generated-content.html
remote_preview:
  url: https://spectrum.ieee.org/media-library/surreal-collage-of-a-womans-face-with-geometric-shapes-pencil-and-wikipedia-globe.png?id=68486571&width=1200&height=600&coordinates=0%2C167%2C0%2C168
---

Ellia Lebleu is a founding member of [the WikiProject AI Cleanup](https://en.wikipedia.org/wiki/Wikipedia:WikiProject_AI_Cleanup), a volunteer effort launched in 2023 to find and remove AI-generated material from [Wikipedia](https://spectrum.ieee.org/wikipedia-at-25) that breaks the website’s guidelines. When the cleanup began, [AI writing](https://spectrum.ieee.org/ai-scientist-research-paper-format) was often betrayed by conspicuous clichés. Today, the project’s volunteers are developing tools to detect subtler forms of machine-generated content and debating AI’s role in the world’s most widely used encyclopedia.

**When did you realize [Wikipedia](https://spectrum.ieee.org/tag/wikipedia) had a problem with AI-generated material?**

**Ellia Lebleu:** It was around December 2023. A few other editors and I were chatting on Wikipedia’s Discord server, and we had all seen very strange articles with the same odd characteristics. They repeated phrases like “stands as a testament” or “rich cultural heritage.” They sometimes had conclusion sections, or sections called “Why is this important?” That was very typical of [ChatGPT](https://spectrum.ieee.org/tag/chatgpt) in 2023 and early 2024 but very odd for Wikipedia articles—and very easy to spot. Then I decided, let’s create an official project on Wikipedia so more people can deal with this.

### Ellia Lebleu

Ellia Lebleu is a founding member of the WikiProject AI Cleanup. Lebleu is pursuing a Ph.D. in AI at the [Inria Center of the University of Rennes](https://www.inria.fr/fr/centre-inria-universite-rennes), part of France’s national research institute for digital science and technology.

**How do editors spot AI-generated writing today?**

**Lebleu:** By 2025, the signs of AI writing were becoming subtler and more structural. For example, AI loves the “rule of three,” negative parallelisms, and other structures of speech that aren’t conclusive by themselves but become meaningful as they accumulate. We also look beyond the article. If someone writes five articles of thousands of words each in 5 minutes, there is reason to suspect AI.

**Does the AI problem just appear in the main text, or also in citations?**

**Lebleu**: Fake or misleading citations have become a major worry. Sometimes an AI hallucinates a citation entirely, although that’s much rarer today because of retrieval-augmented generation that allows the AI to find sources. More often, it finds a real source but gets its content completely wrong. AI citations that don’t actually support what is being written are extremely commonplace.

**English Wikipedia recently** [**prohibited editors**](https://en.wikipedia.org/wiki/Wikipedia:Writing_articles_with_large_language_models) **from using LLMs to generate articles. How much AI-generated content is still getting through?**

**Lebleu:** I’ve been working with \[AI-detection company\][Pangram](https://www.pangram.com/) to estimate the proportion of AI-generated content. I tested it on Wikipedia content from before ChatGPT and found no false positives, so its results provide a very conservative lower bound. In late 2025, it flagged perhaps 20 percent of new articles and drafts as AI-generated. In early 2026, that went down to 15 or 16 percent. But we don’t yet know whether that’s because our recent policies have had an effect or because models are getting more powerful and Pangram hasn’t had time to adapt.

**Is Wikipedia using AI to combat AI?**

**Lebleu:** Another Wikipedia editor has made an AI source verifier, which goes through the sources on a page and checks whether they match the claims. Since it is itself an AI model, there is no guarantee that it will be correct every time. But if you want something that works at scale and filters out obvious cases, it’s a good first step. The Wikimedia Foundation is also working on a tool called [Paste Check](https://www.mediawiki.org/wiki/Edit_check/Paste_Check), which can flag text pasted directly from some AI [chatbots](https://spectrum.ieee.org/tag/chatbots).

And we’ve seen Claude sometimes say, “I can’t write a Wikipedia article; it would go against Wikipedia’s policies.” If all major AI companies implemented that kind of prompt refusal, it wouldn’t solve the problem entirely, because there are still open-weight models that can be tweaked to avoid restrictions. But it would help with people who use commercial models and aren’t especially familiar with the technology.

**Can you tell if the rule prohibiting AI writing is working?**

**Lebleu**: It’s still a little early to tell. We see possible effects, such as prompt refusal and the apparent slight drop in AI-generated content. But again, we don’t yet know if that is because models are getting better or because beginners are learning the rules. The vast majority of AI-generated content is submitted by beginner editors, so we’ve made many efforts to make the rule very visible in welcome messages, editing help, and advice.

**What if AI becomes capable of writing truly reliable Wikipedia articles?**

**Lebleu:** That’s one of the big questions we’re facing. When the guideline was passed, it was for very practical reasons: We were flooded with AI content, we didn’t have good tools, and we needed to put a stop to it. But since then, it has taken on a more philosophical dimension. Many people have called Wikipedia “the last good place on the internet.” Being a place that was, in theory, free from AI slop has become part of Wikipedia’s identity.

A lot of Wikipedia’s communication is now centered on the idea that we’re a human-written encyclopedia. Even AI companies find that interesting. It can be valuable to have a purely human-written corpus on which to \[train\] AI systems. The fact that Wikipedia is still that, and will remain that for at least the near future, makes it a precious resource that isn’t replicated anywhere else.

But will the people who read and contribute to Wikipedia want it to become partially AI-generated if that makes it more comprehensive? We could preserve Wikipedia’s identity as a human-written corpus while also offering an AI-assisted option.

**Has Wikipedia reckoned with the idea of “Google Zero”—a possible future in which websites no longer get traffic from [Google](https://spectrum.ieee.org/tag/google) search?**

**Lebleu:** If everything goes through AI search and websites plummet to zero visibility, how do we expose Wikipedia and show that it can be edited and that people can still contribute? Wikipedia is frequently cited in \[Google’s\] AI Overviews, but users may only see an AI answer citing Wikipedia. They may not realize that Wikipedia is a living project they can edit and contribute to.

The Wikimedia Foundation is working on an attribution framework that could give AI users more information about the article \[being cited\]: when it was last edited, how comprehensive it is, whether editors have flagged problems, and perhaps even a call to action. In the future, readers might even contribute through [AI models](https://spectrum.ieee.org/tag/ai-models): “The AI cited Wikipedia, but there seems to be an error. Can I make an edit request and add this other source I found?”

I can see a very strong symbiotic relationship between Wikipedia and AI models in the future. I don’t see Wikipedia disappearing, but I can see it taking a very different shape. Yesterday, AI was something to clean up. Today, it’s a tool to help us with the cleanup. Tomorrow, it will be the air we breathe.

*A condensed version of this article appears in the November 2026 print issue as “Ellia Lebleu.”*
