---
title: I don't want the details
link: https://michaelheap.com/i-dont-want-the-details/
source: hnrss-org-frontpage
published: 2026-09-23T13:04:44Z
updated: 2026-09-23T13:04:44Z
first_seen: 2026-09-23T19:00:13.987032799Z
authors:
- mooreds
summary: 'Article URL: https://michaelheap.com/i-dont-want-the-details/ Comments URL: https://news.ycombinator.com/item?id=49815466 Points: 246 # Comments: 157'
content: extracted
html: 2026-09-23-i-don-t-want-the-details.html
preview:
  file: 2026-09-23-i-don-t-want-the-details.preview-023bb330723e.webp
  width: 256
  height: 134
  color: '#f0ede6'
images:
- source: https://michaelheap.com/.netlify/functions/og/i-dont-want-the-details
  original:
    file: 2026-09-23-i-don-t-want-the-details.image-432e85d22256.png
    width: 1200
    height: 630
  variants:
  - file: 2026-09-23-i-don-t-want-the-details.image-97629e2cea8a.webp
    width: 320
    height: 168
  - file: 2026-09-23-i-don-t-want-the-details.image-56c6af3d5a30.webp
    width: 640
    height: 336
  - file: 2026-09-23-i-don-t-want-the-details.image-d18265f80802.webp
    width: 1200
    height: 630
  color: '#f9f5ee'
---

A few months ago I was dragged into a call with my engineering counterpart and their boss (who happens to be our SVP of engineering). Something had gone wrong that shouldn't have. Nothing catastrophic, but important enough that I was now on a call with an SVP.

I started to explain how it happened when they cut me off with "Michael, I don't want the details".

They continued:

> I know that if we get into the details, the reasons will be perfectly reasonable. You'll explain what happened, I'll understand why everyone made the decisions they made, and I'll empathise with you.
>
> Then it'll happen again.
>
> So I don't want the details. I want to know what we're changing.

At first I thought "I don't want the details" sounded dismissive. How can they make informed decisions without understanding the details?

Then I realised that "I don't want the details" wasn't being dismissive. The executive assumed that we were competent, and was saying "I already believe you. Now let's talk about what happens next".

## Asking the right question

After something goes wrong, most organizations ask "Why did this happen?" This is a question we're all familiar with answering.

We write up timelines. We reconstruct decisions. We explain dependencies. At the end of it, we hand over a document that contains the specific combination of events that led to the incident.

Everyone nods their head, says "that makes sense", and we all move on with our day.

Understanding an issue is not the same as fixing it. A good explanation can make things worse. Once everyone agrees that the behaviour was reasonable, the urgency to change anything disappears.

When an incident is an unfortunate but understandable sequence of events where no-one is at fault nothing changes. Then the same thing happens six months later, and everyone is left wondering how we landed here again.

To drive change in your organization, don't ask "why did this happen?".

Instead, ask:

**What are we changing so that the same class of failure is less likely next time?**

## Reasonable people

The SVP wasn't interested in understanding how the issue happened or who was involved. They didn't want to be convinced that everyone involved behaved reasonably. That's a baseline expectation.

Their question became:

**“Given that reasonable people produced this outcome, what needs to change?”**

Consider these examples:

> "We missed it because Alice was on holiday and Bob thought the Widgets team owned it".

Okay. How do we make ownership unambiguous when someone is unavailable?

> "The requirements changed three days before launch."

Of course they did! What happens when requirements change inside the launch window?

> "The alert fired, but the on-call engineer had already dealt with twenty low-value alerts that evening".

Makes sense. How do we improve the signal to noise ratio of our alerts?

Focus on changing the system. The people are usually not what needs changing.

## A good explanation is not a fix

If your postmortem is full of sentences like "we should involve support earlier" and "we need to communicate better", or my personal favourite, "we'll be more careful next time", you have a collection of hopes dressed up as progress.

If your corrective action depends on people remembering a conversation from six months ago, you don't have a corrective action. You have organizational folklore. If everyone involved in the incident left the company tomorrow, would the fix still work? If the answer is *no*, the people may have learned something while the system is still destined to fail.

For a postmortem to drive lasting change, ask:

**If the same situation happened tomorrow, what would cause a different outcome?**

A process that forces a decision at this point is an improvement. A system that prevents this class of mistake is stronger still.

## Process for process' sake

You can take "the system prevents this class of mistake" too far.

Not every failure deserves a new process. That's how you build environments that no-one wants to work in. Sometimes the cost of preventing recurrence is higher than the cost of occasionally accepting the failure, and that's ok.

But you need to accept failure with your eyes open. "We are consciously accepting this risk" is very different from "we said we'd try harder and everyone felt better".

## Trust

I still think about what the SVP said a lot. What sounded like impatience was a declaration of trust. They didn't need me to prove that the people involved were competent or well intentioned. They were willing to start there. If the investigation showed otherwise, we could deal with that separately.

What they didn't want was for empathy to become the mechanism by which the organisation absolved itself of having to change.

People are usually making the best decisions they can with the information, incentives and constraints around them. That's why fixing the people is often the wrong answer.

Sometimes the most useful thing a leader can say is:

**I believe you. I don’t need the details. Tell me what we're changing.**
