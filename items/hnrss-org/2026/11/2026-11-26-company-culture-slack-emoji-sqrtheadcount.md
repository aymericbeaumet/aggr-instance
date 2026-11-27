---
title: Company Culture = Slack Emoji ÷ √Headcount
link: https://lia.mg/posts/measuring-company-culture-with-slack-emoji/
source: hnrss-org
published: 2026-11-26T20:49:49Z
updated: 2026-11-26T20:49:49Z
first_seen: 2026-11-27T04:05:56.741343452Z
authors:
- liamg
content: extracted
html: 2026-11-26-company-culture-slack-emoji-sqrtheadcount.html
remote_preview:
  url: https://avatars.githubusercontent.com/u/1187060?v=4
  alt: avatar
---

Companies spend an enormous amount of time and money trying to measure - and even worse, *decide* - culture. Engagement surveys, pulse checks, eNPS, offsites where someone from HR asks you to describe the company as a type of animal. Office posters make claims like “our culture is honesty, hard work and fun”, embarrassing the paper they’re printed on. I once applied to a company that asked candidates which kitchen utensil they most identified as, in order to decide if they were a “culture fit”. I never found out what the right answer was, but I got the job after choosing a meat cleaver, so I’m not sure what that says about either of us.

The results of these things - heavily modified by HR - are compiled into a slide deck, presented at an all-hands, and then quietly repurposed for the careers page. There, they sit beside a photo of six engineers smiling with their mouths but not their eyes at a “hackathon”, which is what the company calls overtime when it’s paid in pizza, at a rate that works out to roughly £1.40 an hour. The salary is listed as “competitive”.

There is a better way to measure culture. It is cheaper, more accurate (in my anecdotal experience), and requires no participation from anyone - which is ideal, because nobody wants to participate in these corporate séances anyway.

## The Formula

Take the number of custom emoji in your Slack workspace and divide it by the square root of the number of users. That’s it.

\\\[C = \\frac{E\_{custom}}{\\sqrt{U}}\\\]

The square root is important. Emoji are a shared resource - a single `:this-is-fine:` can serve an entire engineering department for years - so a company of 10,000 people should not be expected to have 1,000 times more emoji than a company of 10. That would be unhinged.

We’ll call this the **Emoji Coefficient**.

## Interpreting Your Score

*C < 5*

Your workspace contains the default emoji set, a company logo, and `:party-parrot:`, which was added by someone in 2019 who has since left. Nobody uses it any more because it comes across as *unprofessional*. Communication is conducted exclusively via 👍. People are here for the salary, and they would like you to know that.

*5 ≤ C < 20*

A healthy, functional organisation. There is an emoji of the CTO’s face. It is used both supportively and as a weapon, depending on the context.

*20 ≤ C < 50*

Thriving. `:this-is-fine:` has been used over 4,000 times in `#incidents`. Management considers this a sign of engagement.

*C ≥ 50*

Late-stage culture. Every variant of `:thumbsup:` now carries subtext. The plain default one is considered passive-aggressive. The emoji have begun to reference themselves: `:old-man-yells-at-old-man:` and `:party-parrot-holding-a-party-parrot:`. HR has requested a glossary, but no one person understands the memeology or the history of `:cowboy-spongebob-on-fire-crying:`. They’re afraid it might mean something rude. It does.

## The Participation Rate

The Emoji Coefficient alone is not enough. A healthy score can conceal a dark truth: that every single emoji was uploaded by one person, usually a backend engineer who has been there since the beginning and knows where the bodies are buried.

To account for this, we also measure the percentage of users who have uploaded at least one emoji. These are your *culture leaders*. Their reward is a shoutout at the all-hands (it’s customary to mispronounce their name).

\\\[P = \\frac{U\_{uploaders}}{U} \\times 100\\\]

If P is below 20%, things are not good. Your culture is being carried by a handful of people, and when they leave, they will take it with them. The emoji will remain, but nobody will remember what `:fire-extinguisher-thumbsup:` was for.

Above 20%, the culture has the gene pool it needs to succeed.

## Examples of Cultural Success

For reference, here are some real emoji from [slackmojis.com](https://slackmojis.com/):

## Caveats

As with any metric, the Emoji Coefficient can be gamed. A sufficiently motivated middle manager could upload 600 variations of the company logo to drive up the score ahead of their performance review, the outcome of which, unbeknownst to them, was decided in March. This should be detected and treated as what it is: a cry for help. Fortunately, it will also be immediately visible in the Participation Rate.

It is also worth monitoring the *rate of change*. A sudden spike in custom emoji is generally a positive sign. A sudden spike in emoji like `:rip:`, `:press-f:` and `:linkedin:` usually coincides with a reorg, and should be cross-referenced with the number of users, which will shortly be going down. Interestingly, this will improve your score. Mass layoffs are, of course, yet another way in which the score can be gamed.

## What If You Don’t Use Slack?

If your company uses Microsoft Teams, there is no equivalent formula. There doesn’t need to be. If you use Teams, your lack of culture is probably the least of your problems.

If you use something else, I don’t really have any advice for you. I only added this section to blast Teams.

## Conclusion

Culture cannot be manufactured with values posters or mandatory fun. It emerges organically, when an engineer decides that what the team *really* needs is an emoji of an exceptionally surprised cauliflower.
