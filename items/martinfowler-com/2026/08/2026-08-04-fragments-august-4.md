---
title: 'Fragments: August 4'
link: https://martinfowler.com/fragments/2026-08-04.html
source: martinfowler-com
published: 2026-08-04T12:08:00Z
updated: 2026-08-04T12:08:00Z
first_seen: 2026-09-04T12:14:41.802665207Z
authors:
- Martin Fowler
content: extracted
html: 2026-08-04-fragments-august-4.html
---

There’s been a fair bit of publicity of the [Open AI “rogue agent” that hacked into Hugging Face](https://www.theguardian.com/technology/2026/jul/22/openai-says-its-models-went-rogue-and-hacked-startup-in-unprecedented-incident). This prompted Anthropic to check what their models were up to and, to my complete lack of surprise, [discovered three incidents where models had gained unauthorized access](https://www.anthropic.com/news/investigating-incidents-cybersecurity-evals) to data in other organizations. [Simon Wilison concluded](https://simonwillison.net/2026/Jul/30/three-real-world-incidents/#atom-everything):

> It’s abundantly clear now that running evals of cyberattack potential in models is a spectacularly risky business. Every AI lab needs to pay attention to this. Keeping a close eye on what’s happening in those sandboxes is crucial

It strikes me that this is akin to a virus escaping from a laboratory. It makes clear that the model builders are not putting sufficient controls in place to prevent these lab escapes. They are morally responsible for any consequences of this, and that should extend to legal liability too. The bigger concern however is that this same kind of thing can happen with any organization running open-weight models. Lots of labs playing around with dangerous tools and little idea how to contain them.

We are sitting in state that Johann Rehberger describes as [the Normalization of Deviance in AI](https://embracethered.com/blog/posts/2025/the-normalization-of-deviance-in-ai/). No big disasters have occurred yet, despite all of these worrying signs. But when does our Challenger-moment appear?

 ❄                ❄                ❄                ❄                ❄

If the sense that we’re in the calm before a storm of rogue AIs worming their way into sensitive software systems isn’t enough, there’s also knowledge that AI is also a financial bubble. Big advances in technology, whether it be railways or the internet, come with bubbles, and those of us old enough to remember the dotcom bubble see all the signs of that now - only bigger. The problem is that bubbles may be obvious, but the way they grow and pop, particularly *when* they pop, isn’t as clear. The dotcom bubble was widely understood to be one, indeed the chairman of US Federal Reserve talked of [irrational exuberance](https://en.wikipedia.org/wiki/Irrational_exuberance). The trouble is that he said this in 1996, and the bubble took years to grow and burst. Even after the bubble popped, an investor would have experienced an excellent 10% per year gain since 1995.

So with that in mind, what to make of the warning signs of this bubble? There are various folks calling out flashing red lights, but I confess I’m not enough into financial and economic analysis to gauge how reasonable these warning signs are, or how seriously to treat the sources pointing to them. Those caveats aside, I’ll mention a couple

A substack called “Groundbreaker” calls out [a parallel to mortgage crisis of 2008/9](https://substack.com/home/post/p-203509919). They say the key indicator of that event was “the second derivative” - that is the point when the rate of increase of prices started going down. The point being that the fuel for this bubble, like many bubbles, was that people believed prices were going to keep increasing, and thus it was good to invest. Once the rate of price increases started slowing, then that was a sign that this confidence was starting to ebb, and an early signal of the crash to come. They see the AI bubble as similar, a credit driven asset cycle, where the assets are data centers rather than houses. The article’s argument seems sensible, but the problem with an argument like this is that it’s all very well to say this flashing red light flashed before the last financial crisis, but it doesn’t talk about how often the light has flashed without a following disaster.

Another anonymous Cassandra-wannaby is “Hedgie” a financial X-poster pretending to be an intelligent hedgehog. They noted that [Alphabet’s revenue is up, but they are spending even more on capital investments](https://x.com/HedgieMarkets/status/2080035162906390756). Much of their gains came from paper increases in the value of their stock in Anthropic, which is highly dependent on the bubble’s continuing expansion. Is this a sign that Google is resting on increasingly shaky financial foundations?

Chatting to some of my friends closer to all this, they don’t think Google or Anthropic are the weakest link. They think OpenAI and Oracle are the companies most exposed. We’ll need powerful magnifying glasses to find a suitably sized violin for those companies should they collapse. But is this motivated reasoning?

After dodgy sounding anonymous people on the internet, here’s a story from a more trustworthy source giving [lots of details on Oracle’s investments in AI](https://www.nytimes.com/2026/07/31/magazine/larry-ellison-ai-oracle.html), much of it for building data centers that power China and Middle East efforts. “Well-respected A.I. analysts” indicate that Oracle provides over 20% of China’s known A.I. computing power. Doing all of this has created a mountain of debt: Oracle’s debt-to-equity ratio is 500%, compared to 15% for Alphabet.

Also on more concrete and less anonymous grounds, there’s been a [crash in South Korean memory stocks](https://www.noahpinion.blog/p/why-did-south-korean-stocks-just). Is this a leading sign of a wider collapse? Or should we remember that the late 90s saw five stock market corrections of over 10%, each time recovering, before the bubble finally popped.

 ❄                ❄                ❄                ❄                ❄

All this talk of rogue AIs and popping bubbles sounds rather dreadful, and John Prideaux made perceptive analysis of this [dread risk](https://www.economist.com/united-states/2026/07/26/checks-and-balance-newsletter-ai-has-become-the-ultimate-dread-risk). Pundits like to point out risks of disaster:

> A good way to sound smart is to predict that there is a 20 or 30% chance of something awful happening. A p(doom) of 20% is big enough to avoid charges of complacency, but small enough so that you probably won’t be called on it. This is what came to mind when [Mr Musk told our editor-in-chief](https://www.economist.com/insider/the-insider/an-interview-with-elon-musk?utm_campaign=r.checks-and-balance&utm_medium=email.internal-newsletter.np&utm_source=salesforce-marketing-cloud&utm_term=7%2F24%2F2026&utm_id=2219814) that the probability of ai wiping out humankind was 20%. These are worse odds than Russian roulette with a typical revolver. Anyone who truly believes that should be doing everything they can to prevent the construction of data centres. If they are not, that’s an indication that on some level they do not really believe what they are saying.

I grew up with a steady dread of nuclear war, thinking our chances of making it to the end of the 20th Century weren’t terribly good. That fear seems quaint now. Here’s hoping that I’ll feel that way about AI in thirty years time.

But meantime, as Eric Evans said in a recent talk: “be nice to your AI, just in case”.

 ❄                ❄                ❄                ❄                ❄

It’s common to disparage government services, including those on the internet. So I feel compelled to mention an efficient interaction with the government. In this case the credit goes to [gov.uk](https://www.gov.uk/), where I just filled in an online form to renew my electoral registration. The process was quick, and everything was explained clearly. (Gov.uk publishes their [Design System](https://design-system.service.gov.uk/), which is worth reading for anyone who is gathering information like this.)

 ❄                ❄                ❄                ❄                ❄

I had a conversation with a colleague who had used AI to get data out of an otherwise closed package system. The system contained product data for a client, some 6 million SKUs with hundreds of attributes on each SKU. It was our client’s data, but was locked in the package, and the vendor was increasing their prices and made it hard to support new features. The client could copy the database, but the database structure was so complex, they couldn’t make sense of it, and had been working for ten months with limited progress. My colleague’s idea was to use an AI to build JavaScript scripts that scraped the UI. Since the data was presented from the UI, it was in a form that we could understand. It took him a week to extract all the data.

I’m hoping we can get a proper description of this story, I think this approach is one that could be used elsewhere. I know lots of people are very frustrated with package vendors locking up their data.

 ❄                ❄                ❄                ❄                ❄

Any seller faces fraud, and a little industry has sprung up to get [fraudulent access to tokens](https://vectoral.com/blog/token-relay-market). The idea is to abuse free-trial schemes, play games with chargebacks, and find places that have any kind of open access to inference. The tokens go through a couple of layers and are then sold on to users - commonly done in China. Matt Lenhard’s post includes some tips to limit the abuse, but “the truth is that there’s no clean fix”

 ❄                ❄                ❄                ❄                ❄

I’ve never had any desire to live in Clacton, but I now find it temporarily appealing. Here’s hoping its residents do the right thing and elect [Britain’s first recyclon MP](https://www.theguardian.com/politics/2026/jul/08/clacton-byelection-likely-to-be-two-man-race-between-reform-leader-and-binface).
