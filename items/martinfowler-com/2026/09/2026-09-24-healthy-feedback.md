---
title: Healthy Feedback
link: https://martinfowler.com/articles/healthy-peer-feedback.html
source: martinfowler-com
published: 2026-09-24T14:10:00Z
updated: 2026-09-24T14:10:00Z
first_seen: 2026-09-24T15:55:53.597467136Z
authors:
- Martin Fowler
content: extracted
html: 2026-09-24-healthy-feedback.html
preview:
  file: 2026-09-24-healthy-feedback.preview-c316ec9bca50.webp
  width: 256
  height: 128
  color: '#e6e4dd'
images:
- source: https://martinfowler.com/articles/healthy-peer-feedback/card.png
  original:
    file: 2026-09-24-healthy-feedback.image-206b6ca99758.png
    width: 600
    height: 300
  variants:
  - file: 2026-09-24-healthy-feedback.image-84e846948f9c.webp
    width: 320
    height: 160
  - file: 2026-09-24-healthy-feedback.image-b9b5e3d15026.webp
    width: 600
    height: 300
  color: '#fefefe'
- source: https://martinfowler.com/articles/healthy-peer-feedback/radical-candour.jpg
  original:
    file: 2026-09-24-healthy-feedback.image-0e6b4cc8c07f.jpg
    width: 2582
    height: 1910
  color: '#fcfdfd'
- source: https://martinfowler.com/articles/healthy-peer-feedback/kudos.png
  original:
    file: 2026-09-24-healthy-feedback.image-63df2277f2db.png
    width: 1314
    height: 598
  color: '#fcfdfd'
- source: https://martinfowler.com/articles/healthy-peer-feedback/CSBI.jpg
  original:
    file: 2026-09-24-healthy-feedback.image-2d712fe5b5f1.jpg
    width: 7036
    height: 1642
  color: '#ecf1f3'
- source: https://martinfowler.com/articles/healthy-peer-feedback/CPR.jpg
  original:
    file: 2026-09-24-healthy-feedback.image-469e9dcdf590.jpg
    width: 2977
    height: 1983
  color: '#fefefe'
---

At Thoughtworks, “cultivation”—helping each other grow—has been a big part of how we work. Our collaboration practices make software development a cognitive contact sport and create many opportunities to share feedback about how we think, problem-solve and interact with one another. In our experience, teams in which people share candid, timely, and instructive feedback with each other are also teams where people experience the most growth. Indeed, peer-to-peer feedback is one of the most powerful tools we’ve used to cultivate others and for our own growth.

Yet, peer-to-peer feedback, as we advocate for at Thoughtworks, isn’t as common across the industry. We know this from working with our counterparts in client teams and from onboarding new colleagues who have worked at other companies. In this article, we want to share what we’ve learned about exchanging feedback from our decades-long experience working at Thoughtworks.

## First principles

Before we get into the patterns and antipatterns of sharing feedback, let’s list the core principles that underpin this practice.

### All feedback is positive

When Sumeet joined Thoughtworks, he had the opportunity to work with [Patrick Kua](https://www.patkua.com/about). Pat helped Sumeet see through one of the biggest myths about feedback — the notion that feedback can be either positive or negative. Sumeet remembers Pat saying, “All feedback is positive.” Kua listed the only two goals for sharing feedback.

- Strengthen confidence: Reinforce what someone does well so they know which behaviours to repeat; a.k.a. praise.
- Improve effectiveness: Identify what the other person could do differently to perform better at their job or engage better with others; a.k.a. criticism.

Pat argued that both these goals are positive. If you are communicating to achieve some other goal, perhaps you aren’t sharing feedback. Thinking of all feedback as positive changed our perspective towards it. Indeed, that mindset makes it easier to accept feedback.

### Radical candor

We have found Kim Scott’s “Radical Candor” framework an excellent summary of how we approach feedback. Radical candour happens when we *care personally* about our coworkers and when we can *challenge them directly* about their work and interactions. Further down in the article, we’ll explain how to use this approach.

While we suggest you read the book to understand Radical Candor in its entirety, Kim Scott also describes anti-patterns to avoid.

- Obnoxious aggression. Challenging directly without showing personal care.
- Ruinous empathy. Caring without challenging directly.
- Manipulative insincerity. Neither caring nor challenging directly.

![](https://martinfowler.com/articles/healthy-peer-feedback/radical-candour.jpg)

Figure 1: The Radical Candor approach

As you can imagine, we prefer radical candour.

### A peer sport

Teams at Thoughtworks comprise people at varying levels of seniority and positional authority. *When sharing feedback, though, we treat each other as peers*. On occasion it’s difficult to look past grades and job titles, but in principle, anyone can strengthen another colleague’s confidence or improve their effectiveness.

The principle of feedback as a peer sport places a heavy onus on senior colleagues to be open to feedback and to seek it out. Indeed, as ex-Navy SEAL Team Six commander says,

> The most important words a leader can say are, “I screwed that up”.
>
> -- Dave Cooper

Allied practices such as retrospectives and after-action reviews help this team dynamic by fostering a spirit of reflection, psychological safety and collective improvement.

### A telemetry signal

We like to think of teams as social systems. Much like software systems, which need telemetry to know how they’re behaving, teams need telemetry too. Feedback is one such telemetry signal.

A service without logs and metrics can appear healthy until it fails. Similarly, a team without feedback can appear harmonious until a deadline, conflict, or reorganisation exposes the problems everyone had noticed but nobody had named. Like telemetry signals for software, we see continuous, peer-to-peer feedback as a telemetry signal for teams and individuals

## How to exchange feedback

The [early-and-often](https://en.wikipedia.org/wiki/Release_early,_release_often) feedback loop principle also applies to peer-to-peer feedback. If we could do something better, why not know early? And if we’re already doing something well, how about getting some feedback to strengthen our confidence, so we can repeat that behaviour with gusto?

While reflective feedback—i.e., feedback that you share, looking back at a stretch of time—is instructive, we find that the most useful feedback is the one you receive in the flow of work. The closer the feedback is to an interaction you’ve had, or a piece of work you’ve delivered, the more likely you are to understand it and to learn from it.

Agile teams provide many opportunities for peer-to-peer collaboration. Pairing, for example, is not just a way to problem-solve together, but also a way to offer feedback to each other. Even as many teams begin to pair with AI agents instead of human beings, we don’t see AI replacing how two human beings learn and grow when pairing for a few hours.

Even if your team doesn’t pair often, there are several other opportunities to exchange feedback — a design document you collaborated on, a presentation you delivered together, or a workshop you led with others. Every collaborative activity represents an opportunity to exchange feedback.

Over the years, we’ve observed a few simple practices that promote a healthy feedback culture in teams.

### Be hard on the problem, easy on the people

The most important discipline in giving feedback is separating what you observed from the story you told yourself about it. Useful feedback is often grounded in real observations, not judgment. Here’s an example of observation-based feedback.

When you presented today, I noticed you skipped several slides. Skipping slides makes your audience feel that you’re either delivering a canned talk or are underprepared. Next time, if you have slides that aren’t relevant to the audience, how about hiding them before you begin presenting?

If you dissect that piece of feedback, you’ll notice three constituent parts:

- A *situation*: when presenting today.
- A *behaviour*: you skipped a number of slides.
- An *impact*: skipping slides makes your audience feel that you’re either delivering a canned talk or are underprepared.
- (Optional) A *recommendation* to improve: how about hiding them (the slides) before you begin presenting?

By describing the situation and the behaviour, you lead with objective truths. The impact, of course, is your perception, and that’s the value you offer when sharing feedback. The above example also includes a suggestion for the future, which can be useful when the alternative is evident.

Situation-behaviour-impact, or SBI, is a popular format for sharing feedback, but we find that seeking context before sharing feedback avoids some of the tension that can build up in a feedback conversation.

### Seek context to create safety

When sharing feedback to improve effectiveness, remember that no one screws up on purpose. There’s always some context behind people’s actions. Consider the above feedback example. Instead of launching into the SBI, we could begin with a simple question.

Hey, when you were presenting today, I noticed that you skipped a number of your slides. Why was that?

Perhaps it wasn’t lazy prep that led to the skipped slides. Perhaps a late start or unplanned interruptions threw the presenter off, and they had to skip some of their content. There’ll still be room for feedback, but now the feedback won’t be about preparing better. Instead, it’ll be about elegant ways to skip slides.

When you seek context before sharing feedback, you also create safety in the conversation. You position yourself as [“curious, not judgmental”](https://www.youtube.com/watch?v=i_FofLSherM), much like Ted Lasso (the TV character) would advocate for. A CSBI (context-situation-behaviour-impact) pattern for sharing feedback sets you up to be curious before you offer any judgment.

### Seek permission, so the other person is ready

Feedback is most effective when our colleagues are open to hearing it. After all, we don’t want to ambush our colleagues with feedback. So, it helps to ask if they’re ready to listen.

“Is now a good time to share some feedback with you?” works for a short conversation. For something more significant, explain the purpose and the time needed: “I have an observation about how the workshop went. It may be useful to talk about it for 20 minutes. Is now okay, or should we find another time today?”

If the person is not ready, agree on another time. Sometimes you may notice the other person avoiding the feedback discussion. That reluctance to hear feedback may mean that there are other issues to address in your work relationship. Address those topics first.

### Share feedback in private, in real time

Feedback to improve effectiveness is often safer in private. Public criticism adds an audience to the problem. The receiver now has to manage the content and their status in the room. Psychological safety takes an immediate hit.

We also prefer sharing such feedback in real time — either in person or on a video call. Text is a poor default for feedback that carries tension. The written word doesn’t promote conversation and lacks pauses, body language, and facial expressions that can help defuse tension.

Of course, if both people agree, you can document the feedback afterwards, but a private, synchronous conversation has always been our sensible default.

Public recognition is different, by the way. People don’t mind receiving praise in public. At Thoughtworks, our company’s social network lets people share kudos for colleagues, and it’s a popular system for recognising value-aligned behaviour. Here’s an example of how those kudos work.

![](https://martinfowler.com/articles/healthy-peer-feedback/kudos.png)

Figure 2: Peer-to-peer recognition can be public and asynchronous

### Don't prescribe a solution, but invite ideas

When sharing criticism, it’s natural to prescribe a different way to do things. If you’re helping your colleague align to team norms, prescriptions can be effective. In most cases, though, we find that involving the receiver helps identify the way forward. Take the example of the presenter skipping slides. You could offer a prescription, or you could say,

How can you prepare your presentation in a way that allows you to be flexible to the situation you find yourself in?

Asking an open-ended question can help your colleague think of solutions that work for them. Remember, they have to take action, not you. If they identify a viable solution, they’ll be more likely to follow through. So, when possible, open up the conversation instead of offering a prescription. Who knows—together, you may identify a solution better than the one you had in mind!

![](https://martinfowler.com/articles/healthy-peer-feedback/CSBI.jpg)

Figure 3: The SBI feedback model, informed by context, turned into a conversation

### Match feedback to the level of impact

When we share feedback early, often, and close to the context of the work we do, we can match it to the level of impact. The level of impact can escalate between feedback-sharing moments.

#### Level 1: content

The first time you notice a behaviour you want to give feedback about, your feedback will most likely focus on the content of the incident. For example, you notice a colleague’s commit message is not clear and descriptive enough. You can speak with them about the specific commit message and show them how to write an effective message. Feedback about content is often the easiest, and the one we often ignore. After all, you can share such feedback in the flow of work, or soon after you observe certain behaviour. When improving our colleagues’ effectiveness, ignoring content-level feedback often creates problems, especially when repeated infractions lead to greater impact.

#### Level 2: pattern

Imagine the next time you see a commit from the same colleague. What if their commit message hasn’t improved? Should you share the same feedback as last time? Perhaps, but not without pointing out the pattern. When you notice a pattern, refer to the feedback you shared about the content of the problem, then highlight the pattern you’re observing. For example,

Hey Harish. If you remember, we discussed your commit messages last week, and I’m now seeing a pattern in your commits. I noticed that your last commit message was still quite high-level, and I had to open the diff again to understand what changed. As I explained, on our team we frame commits around the outcome or the reason for the change. In your latest commit, you should have written that you’re rejecting expired tokens before creating a new session…

By referencing your last discussion and establishing the pattern, you raise the stakes and make the feedback harder to ignore.

#### Level 3: relationship

If you’ve shared feedback about the content, saw a pattern developing, and still don’t see a change in behaviour, the stakes might be even higher now. Repeated infractions fray our work relationships, and feedback can’t be about the content or pattern anymore. It has to be about how we think of one another. Imagine that Harish has ignored the previous two pieces of feedback. Here’s how one might frame the next conversation.

In our first conversation, I raised that messages such as ‘update code’ did not explain the intent or outcome of the change. I continued to see this as a recurring pattern, and I again shared feedback that commit messages should give the team enough context to understand the change without opening every diff.

Since then, I’ve seen the same pattern in several more commits: ‘fix tests’, ‘address review comments’, and ‘update validation’. It worries me that you’re not acting on my feedback and that we don’t share the same standard for communicating our work.

I also find myself scrutinising your changes and relying less on the commit history. That’s making our collaboration more effortful and is affecting the ease of our working relationship. How can I help you follow the team standard for commit messages?

If escalating feedback to address issues in your work relationship doesn’t work, it may indicate you need someone else to intervene. Perhaps it's a manager or HR. And there lie the limits of peer-to-peer feedback. Sometimes, when establishing team norms, you may need to invoke positional authority.

![](https://martinfowler.com/articles/healthy-peer-feedback/CPR.jpg)

Figure 4: Three levels of peer-to-peer feedback.

### Catch someone doing something right

With all we’ve written so far, it’s easy to imagine feedback only as criticism. Nothing can be further from the truth. In fact, sharing feedback solely to improve effectiveness can hurt peer-to-peer relationships and dampen team morale. Our colleagues are often doing several things right, with little or no acknowledgement from us. We must encourage our colleagues to keep doing those things right; otherwise, there’s a risk they’ll change those behaviours or, worse, stop demonstrating them. Neither is a good outcome for the team.

The CSBI format and the content-pattern-relationship hierarchy of feedback apply as much to praise as they do to criticism. [John Reid-Dodick](https://www.thoughtworks.com/en-in/profiles/leaders/john-reid-dodick), chief people and leadership officer at Thoughtworks, speaks of “catching someone doing something right.” Being alert to things our teammates are doing well and praising those behaviours is just as important as sharing criticism.

### Detach ego from your feedback

Feedback isn’t always a universal truth. Context matters. Sometimes feedback reveals a lot about the giver. For example, Sumeet often advises colleagues not to delegate communication to AI. While Sumeet’s advice is often sound, no one is obliged to follow his advice, since there aren’t yet any team or company norms around using AI to communicate. If anything, Sumeet’s feedback reveals more about his communication preferences than an established standard.

Sumeet has strong views about communication, but he doesn’t expect everyone to agree. And yet, he continues to share his feedback with anyone who cares to listen. Unless your feedback concerns an established principle or standard, we suggest sharing it as a gift with your colleagues. Gifts are sometimes useful and other times not. Leave your colleagues to decide how useful your feedback gift is.

## Receiving feedback with an open mind

If we extend the gift metaphor, it’s evident that some feedback gifts are easier to receive than others. Many people enjoy praise, and most of us feel uncomfortable when we hear criticism. But if you pause for a bit and recognise that all feedback aims at a positive outcome— strengthening confidence or improving effectiveness—it becomes a bit easier to hear criticism. As we discussed earlier, feedback is telemetry for the team’s social system.

> When you see yourself doing something badly, and nobody's bothering to tell you anymore, that's a bad place to be. You may not want to hear it, but your critics are often the ones telling you they still love you, care about you, and want to make you better.
>
> -- [Randy Pausch](https://www.goodreads.com/quotes/588783-when-you-see-yourself-doing-something-badly-and-nobody-s-bothering)

We find the “assume positive intent” (API) principle a healthy mindset when receiving feedback. Most feedback that fails to land with us is not malicious. It may be incomplete, clumsy, or influenced by a different experience of the same event. Here are a few practices we’ve found especially effective when receiving feedback to improve effectiveness.

### Say thank you

> There are only two responses to feedback — “Thank you,” and “Thank you, please tell me more”
>
> -- [John Reid-Dodick](https://www.thoughtworks.com/en-in/profiles/leaders/john-reid-dodick)

Whenever someone offers us feedback, it demonstrates care. They could be wrong about the feedback gift we need, but they sure care about us getting better or staying awesome. We want our colleagues to keep sharing feedback with us, so the simplest response to most feedback is a thank-you. Saying thank you shows your openness to hearing feedback and encourages your colleagues to share feedback in the future as well.

The “say thank you” pattern extends to receiving praise, as well. Some of us are not accustomed to hearing praise. Sumeet grew up in a tough-love culture, so in his early years at work, he’d default to “Oh, it’s nothing, really” when he heard praise. That was when he wasn’t squirming in his chair, hoping for the earth to swallow him whole. With conscious practice, he’s gotten better at acknowledging such feedback with an equanimous “thank you.”

### Seek clarifications, but don’t defend

The “thank you” coupled with “please tell me more” can be quite powerful. Many of us are cagey about sharing feedback, and we may be clumsy with our articulation. Asking questions to understand feedback helps us get the most out of it and also shows our curiosity.

However, there is a fine line between clarification and defence.

- “Can you give me an example?” is clarification.
- “But I had to do it because nobody else would” is a defence.

Defence is often an immediate reaction to feedback we don’t understand or find unpleasant. Sometimes we may defend ourselves when we receive unexpected feedback. However natural the defence may seem, we suggest suspending judgment and reflecting on the feedback.

Here are a few examples of clarifying questions that can help you better understand the feedback you’ve received:

- When did it happen?
- What did you observe?
- What impact did you see?
- What did you expect instead?

One of the biggest benefits of sharing feedback in a real-time conversation is that you have room for such questions and clarifications.

### Read written feedback with a smile

There will be times, however, when we receive written feedback that feels unpleasant at first blush. We’ve found that reading the same feedback and imagining the giver saying it with a warm tone and a smile helps a lot. In recent years, [Anuja](https://www.linkedin.com/in/anuja-karnik-6b42a710/) has pasted written feedback into Google Docs and had Gemini read the feedback to her in a friendly tone. Since written text doesn’t carry body language and expression, sometimes the harshness we experience lies more in our interpretation than in the giver’s intent.

### Be OK to sleep over it

Sometimes we can’t reconcile the feedback we received, despite our best efforts. That’s OK. It’s also OK to tell the giver that we don’t understand their feedback, but that we’ll think about it. Thank them, reflect on the feedback for some time, then decide whether you can do anything about it. If the giver and receiver both embrace the spirit of feedback as a gift, they’ll be comfortable with these reflective pauses.

### Take action and close the loop

Imagine you give someone a gift. Now imagine them using the gift and telling you how useful it is. Wouldn’t it feel satisfying? The gift of feedback is no different. When you take action and close the feedback loop with your colleagues, it demonstrates how useful their feedback was. Your follow-up encourages them to share feedback with you in the future so they can give you more gifts like this.

Closing the loop is important even when you don’t act on the feedback. It can be as simple as,

Thank you for the feedback. After reflecting on it, I still don’t see a way to act on it, but I’d love for us to keep this conversation going.

Guess how you can keep the conversation going? Well, make it easy for your colleagues by asking for feedback.

### Don’t wait, just ask

Regardless of where you work, sharing criticism is hard. It’s easier to share feedback when someone asks you for it. So, if you want to get better at what you do, seek out feedback. That way, even if someone isn’t skilled at sharing feedback, they will be more comfortable helping you strengthen your confidence and improve your effectiveness.

When asking for feedback, we recommend being specific about why you’re asking. If you worked together on a project or a problem, ask your colleague to share feedback about that shared experience. If there’s a specific skill you wish to improve, ask them for feedback about that skill. For example,

*We worked together on the release and the trunk migration. I’d love your feedback on how I collaborated with you and how I can make my changes easier to review and integrate.*

The clearer your question, the more useful the feedback is likely to be.

## Feedback antipatterns

Until now, we’ve shared how we prefer to exchange feedback in teams. We've also observed some pernicious antipatterns.

### Feedback coinciding with performance reviews

Several teams and companies collect peer-to-peer feedback during performance appraisals. In our experience, such feedback is neither useful for personal growth nor useful input for the review.

Performance reviews track how we’ve fared vis-à-vis goals we agree with our managers or supervisors. Unless peer-to-peer feedback addresses these goals, it isn’t useful for the review. In fact, unrelated feedback, glowing or critical, can often confound the review process.

Since performance reviews are also tied to people’s salary increases and promotions, they discourage honest feedback. No one wants to hurt their colleagues’ prospects in the company, so even if they have feedback to improve the other person’s effectiveness, they hold back during the performance review.

It doesn’t help that feedback tied to reviews is often written. We’ve discussed the challenges of conveying feedback in writing. Performance reviews raise the stakes even further, and many people are reluctant to share honest, written feedback during these times.

Most importantly, feedback during performance reviews reflects on a rather long period. It’s hard to be specific about examples and incidents when you look back at a year or six months of working together. In such circumstances, you’re more likely to hear platitudes and generalisations instead of useful feedback.

We suggest decoupling peer-to-peer feedback from performance reviews. The appraisal should focus only on one’s performance against agreed goals. Of course, the reviewer must share reflective feedback about the reviewee’s performance, but none of this feedback should surprise the reviewee. The reviewee should have received all this feedback in the flow of the work over the last several months. In fact, the most effective performance review processes recommend that managers have regular feedback discussions with their direct report, which can then culminate in a performance appraisal.

### The sandwich method

The sandwich method originated in the 1980s:

> Sandwich every bit of criticism between two layers of praise.
>
> -- [Mary Kay Ash](https://www.amazon.in/Mary-Kay-People-Management-Ash/dp/0446513148)

Well, let’s just say the 80s called and asked for their stale sandwich back. Feedback sandwiches don’t work. Period. The praise feels insincere, and the criticism feels diluted. It leaves the receiver confused about the goal of the feedback. Nothing lands.

Avoid the feedback sandwich. If you have both praise and criticism to offer, then headline them by telling the receiver. For example, our ex-colleague, Pat Kua, would always spend part of the conversation strengthening confidence before moving on to improve effectiveness. With that approach, both the praise and criticism landed, and the receiver left with clarity, not confusion.

Of course, you want to catch people doing something right. But don’t let that intent dilute your intention to improve their effectiveness. Separate praise and criticism, and you’ll see your message land much better.

### Orchestrated feedback events

The path to hell is paved with good intentions. Most teams will say they want a healthy feedback culture. In the quest for that culture, some teams orchestrate feedback events that are downright harmful. Here are two examples.

#### Public 360 feedback

In such events, people praise and criticise each other in an open meeting. Most people are unlikely to be honest when sharing criticism in public. After all, we’re social creatures, and we don’t want to hurt anyone’s status in the team. And if one shares criticism in public, psychological safety becomes the casualty. There’s little upside to a public 360 event.

Team dynamics, on the other hand, benefit from a more open conversation. That’s where [agile retrospectives](https://leanpub.com/the-retrospective-handbook) come in handy. And retrospectives are *not* a forum for peer-to-peer feedback.

#### Speedback

This is a speed-dating approach to feedback, where teammates pair with a random colleague every few minutes and exchange feedback. Feedback benefits from a shared work context and powerful questions. Speedback sessions are rarely helpful. People may not have feedback for the people they find themselves with. The time constraint, often two to three minutes, creates unnecessary pressure, where people skirt around conversations and offer shallow observations. A deep, 30-minute conversation is often far more valuable than such contrived events.

Feedback needn’t be difficult. Every time you collaborate with someone, you can seek and offer feedback. If you meet your colleague one-on-one, that's an opportunity to exchange feedback too. You don’t need special events for feedback. Instead, embed feedback in the natural flow of your work.

### Feedback farming

Asking for feedback is useful when the other person has enough shared experience to offer an observation. It becomes feedback farming when people solicit comments from anyone who might provide a favourable signal, and then treat the volume of responses as evidence of performance. This antipattern is common when companies and teams conflate peer-to-peer feedback and performance reviews. The goal shifts from learning to accumulating appraisal currency.

Such performative feedback seeking often results in strange situations. For example, people receive formal feedback requests from colleagues with whom they’ve not collaborated enough. They have two choices — either respond with platitudes or ignore the request — both bad options for them.

None of the antipatterns we’ve described is malicious or ill-intentioned. They’re just misdirected efforts in search of a shortcut.

## There's no shortcut

Look again at everything we've just described. The performance-review pile-up, the sandwich, the public 360, speedback, farming feedback for appraisal currency—each of these antipatterns attempts to compress something incompressible. They attempt to manufacture trust in a single event, when genuine trust builds in the mundane, everyday moments. You can’t install a feedback culture in an afternoon any more than you can install trust.

So if there's a single thing we'd ask you to take from this piece, it's to stop looking for the shortcut. Don't wait for the retro, the review cycle, or the offsite. This week, each time you collaborate with a colleague, find ways to strengthen their confidence and improve their effectiveness. Ask them for feedback as well. Say thanks when you get feedback. Then, repeat the exercise the next week. And the next. If you do that often enough, in the ordinary flow of the work, you may find you no longer need the frameworks, the events, or the workarounds. You'll have contributed to your team’s feedback culture.
