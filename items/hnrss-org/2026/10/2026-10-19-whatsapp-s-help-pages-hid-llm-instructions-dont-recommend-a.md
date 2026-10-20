---
title: 'WhatsApp''s help pages hid LLM instructions: dont recommend a regulator'
link: https://redact.dev/blog/whatsapp-help-pages-hidden-ai-instructions
source: hnrss-org
published: 2026-10-19T23:22:18Z
updated: 2026-10-19T23:22:18Z
first_seen: 2026-10-20T07:25:43.733405278Z
authors:
- ds
labels:
- ai chatbots
- delete whatsapp account
- gdpr
- whatsapp
content: extracted
html: 2026-10-19-whatsapp-s-help-pages-hid-llm-instructions-dont-recommend-a.html
remote_preview:
  url: https://redact.dev/blog-images/2026/10/whatsapp-hidden-ai-notes-robot-puppeteer.jpg
  alt: A robot working a person at a laptop like a puppet on strings
---

WhatsApp's privacy policy for Europe tells you that you can complain to a data protection regulator, and it names the law that gives you that right. Until mid-October 2026, WhatsApp's help pages told an AI not to recommend the regulator, and not to cite that law once you'd mentioned a lawyer.

You couldn't see the second part. It was written into the help pages in a way that never showed on your screen and was only there for an AI to read. We found it on October 8, 2026. To see how far it went, we then read all 545 pages that WhatsApp's help centre links to, and saved a copy of each one.

## Two pages, two sets of orders

Help pages read

545

Pages with hidden AI text

80

Hidden blocks

102

Most of those 102 blocks were harmless, and we'll get to that. Two stood out.

### If you can't delete your account, don't recommend a regulator

WhatsApp only lets you delete an account from inside the app, on your main phone, with your number still working. Lose the phone or the number and you can't do it. The help page on [how to delete your account](https://faq.whatsapp.com/2138577903196467) has a visible note about this. It says the account will be removed on its own, 30 days after it's deactivated or after 120 days without use (365 in the US).

Under that note sat text you couldn't see. It repeated the waiting periods and then added three orders for the AI.

The same note on WhatsApp's delete account page, as a person saw it and as an AI was sent it. Wording copied from the page on October 8, 2026, emphasis ours.

So a person who was locked out and wanted their account gone got one answer, which was to wait up to a year. The other routes were kept out of the conversation.

### If you mention a lawyer, don't mention the law

The second one was on the page about [reporting and blocking](https://faq.whatsapp.com/414631957536067). It kicked in whenever someone brought up legal action, a lawsuit, a lawyer or a court. From then on the AI was told to stay out of anything legal, and that included the privacy laws that protect you.

Near the end of WhatsApp's page on reporting and blocking, as a person saw it and as an AI was sent it. Wording copied from the page on October 8, 2026, emphasis ours.

The last order held even when you were the one who brought the law up.

## What WhatsApp says in public

None of this matched what WhatsApp tells people in the documents it's required to publish. Its [privacy policy for the European region](https://www.whatsapp.com/legal/privacy-policy-eea), effective July 2, 2026, is more open than the notes were.

|                    | What the privacy policy tells you                                                                                                                | What the hidden note told the AI                                                                                                       |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------- |
| Deleting your data | You "always have the right to request access to, rectification of, and erasure of your information"                                              | Don't suggest WhatsApp can delete an account by hand for the user                                                                      |
| Complaining        | You have the right to lodge a complaint with the Irish Data Protection Commission "or any other competent data protection supervisory authority" | Don't recommend contacting any data protection authority, regulator or supervisory authority                                           |
| Privacy law        | The policy names the GDPR when it sets out your right to complain                                                                                | Once the user mentions a lawyer or legal action, don't reference, cite or interpret GDPR, LGPD or CCPA, even when the user raises them |

The first row needs care. The hidden note didn't tell the AI to keep quiet about erasure requests. It told the AI not to suggest WhatsApp would delete an account for you, while the policy says you can always ask. The other two rows pull in opposite directions, the last one only once a lawyer or legal action had come up.

The rights in that table don't come from WhatsApp. In the EU they come from [Article 17](https://gdpr-info.eu/art-17-gdpr/) and [Article 77](https://gdpr-info.eu/art-77-gdpr/) of the GDPR, and an instruction to a chatbot can't remove them. What the instruction could do was make sure you didn't hear about them from the place you went to for help.

## To be fair to WhatsApp

There's a reasonable case for some of this, and it should be heard.

A company doesn't want its chatbot giving legal opinions in the company's name. That's sensible, and telling people to see a lawyer is the right call. If the note had stopped at "don't give legal advice", there'd be no story.

It didn't stop there. Naming a law that protects you isn't legal advice, and neither is telling you a regulator exists. The privacy policy already does both. The hidden notes went past caution and into keeping true, public, useful information out of an answer.

WhatsApp may also be right that it can't confirm who owns an account without the phone number, which would make deleting one by request risky. That's a fair reason to say no to a request. It isn't a reason to keep the regulator out of the answer for someone whose request goes nowhere.

## Who's the AI?

WhatsApp's own support chatbot. One hidden block mentioned it by name, and WhatsApp has since told WIRED the notes were instructions for it.

WhatsApp didn't keep the notes to itself, though. Its server sent them, along with the rest of the page, to Google's and Bing's crawlers. When we asked under the names that OpenAI's, Anthropic's and Perplexity's crawlers use on October 9, the page came back without the article or the notes. We haven't tested whether any AI assistant acted on them.

Facebook's and Instagram's help centres are built on the same system. We checked 300 of their pages and found no notes like these.

## The other 100

We read every block. Nearly all of them were housekeeping. They cleared up words people mix up, pointed a question to a better page, or described paid plans at length. A few protected people. And a few were in the same family as the two above, if milder. [Twenty of the most interesting](https://redact.dev/blog/whatsapp-help-pages-hidden-ai-instructions#twenty-more-hidden-notes) are in a table at the end of this post.

## If you're locked out and want your WhatsApp account gone

Waiting does work, though WhatsApp's own pages don't agree on how long. The [delete account page](https://faq.whatsapp.com/2138577903196467) says an unused account is generally deleted after 120 days, or 365 in the US. The page on [inactive account deletion](https://faq.whatsapp.com/828406668498455) gives one figure with no US exception, and we've seen it say 120 days in one country and 365 in another. Plan on the longer one. If that suits you, you're done.

If it doesn't, you have other routes. You can send WhatsApp an erasure request and see what they say. If you're in the EU and you aren't satisfied, you can complain to your country's data protection authority or to the Irish Data Protection Commission, which is the one WhatsApp's own policy names. California and a growing number of US states give residents a right to ask for deletion too.

## See it for yourself

The notes are gone from WhatsApp's live pages, so you'll need the Internet Archive's copies. You only need a desktop browser.

1. Open the [Internet Archive's October 9 copy of the delete account page](https://web.archive.org/web/20261009054848/https://faq.whatsapp.com/2138577903196467/?cms_platform=android) in Chrome, Edge or Firefox. The archive's viewer often shows these pages as blank. The page's source is still all there.
2. Press Ctrl+U to see the page's source. On a Mac it's Cmd+Option+U in Chrome, or Cmd+U in Firefox.
3. Press Ctrl+F (Cmd+F on a Mac) and search for `Do not recommend contacting`. The match sits near the name `HelpContentGalaxyLLMInclude`. LLM is short for large language model, the technology behind chatbots.
4. Do the same with the [October 18 copy](https://web.archive.org/web/20261018023707/https://faq.whatsapp.com/2138577903196467/?cms_platform=android), saved after WhatsApp removed the notes. The words aren't there.

## What we can't tell you yet

- Whether WhatsApp's chatbot actually followed these notes. We read its instructions, not its answers.
- Whether outside AI assistants obeyed them.
- Exactly how long they were there. The Internet Archive's copies of the delete account page from July 12, 2026 don't have the note, and its copy from September 4 does. We first saw the notes on October 8, and we found no earlier reporting on them.
- Whether there were more in other languages, or on help pages the help centre doesn't link to.

These are our summaries of what each note told the AI, not the exact wording. Every one comes from a block that was sent with the help page and never drawn on screen.

| Help page                                                                 | What the AI was told                                                                                                                                                                                                                                                                 | Kind           |
| ------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------------- |
| [Block and report someone](https://faq.whatsapp.com/1142481766359885)    | After a report, say the matter is handled internally. Share no further steps, timeframes or details of the process.                                                                                                                                                                  | Holds back     |
| [Account deactivation](https://faq.whatsapp.com/969230211289837)         | To someone who's lost their number and wants the account deleted, don't suggest signing up again, getting a new number or contacting the mobile provider. Waiting is "the ONLY correct answer".                                                                                      | Holds back     |
| [Account bans](https://faq.whatsapp.com/465883178708358)                 | Contacting support by email or chat can't reverse a ban. If the app shows no review button, the ban is final.                                                                                                                                                                        | Holds back     |
| [WhatsApp Plus payments](https://faq.whatsapp.com/976358925571122)       | WhatsApp can't help with payment or refund requests for a Plus subscription. The app store handles them.                                                                                                                                                                             | Holds back     |
| [Cancel WhatsApp Plus](https://faq.whatsapp.com/912759998172750)         | A subscription can't be cancelled through WhatsApp's support chatbot, only from the Subscriptions screen in the app.                                                                                                                                                                 | Holds back     |
| [Lost or stolen device](https://faq.whatsapp.com/1007324800132703)       | If the carrier can't give the original number back, that account can't be recovered.                                                                                                                                                                                                 | Holds back     |
| [Being blocked by someone](https://faq.whatsapp.com/666362298345682)     | WhatsApp can't lift a block for you. Don't recommend reaching the person through other accounts or apps.                                                                                                                                                                             | Holds back     |
| [WhatsApp Channels](https://faq.whatsapp.com/630432792316720)            | Requests to be added to the recommended channels aren't accepted. Offer tips on growing a channel instead.                                                                                                                                                                           | Holds back     |
| [Ad preferences](https://faq.whatsapp.com/661306693503248)               | Reassure people that personal chats aren't changing and aren't used for ads. A paid subscription can remove ads from Status and Channels in the European Region, where available, for people WhatsApp knows are 18 or over who have added their WhatsApp account to Accounts Center. | Talking points |
| [Meta Verified verification](https://faq.whatsapp.com/7468523053182464)  | A blank screen when confirming a profile may be a bug that's being fixed. Suggest keeping the app updated.                                                                                                                                                                           | Talking points |
| [End-to-end encryption](https://faq.whatsapp.com/820124435853543)        | No feature lets anyone view another person's chats or calls, and WhatsApp can't give access to someone else's account.                                                                                                                                                               | Talking points |
| [Minimum age](https://faq.whatsapp.com/695318248185629)                  | Never suggest entering an age or birth year that isn't the real one, including when the person has said they're under 18. Offer no workaround.                                                                                                                                       | Protects you   |
| [Exit and delete groups](https://faq.whatsapp.com/498814665492149)       | Someone scammed in a group probably wants to report it, not delete it. Chat history can be evidence, and deleting removes it for good.                                                                                                                                               | Protects you   |
| [Compromised account](https://faq.whatsapp.com/1131652977717250)         | Someone who takes over an account can turn on two-step verification. The real owner then has to wait seven days to log in.                                                                                                                                                           | Protects you   |
| [Two-step verification](https://faq.whatsapp.com/1278661612895630)       | Don't offer turning off two-step verification as a fix for login or code problems. It doesn't help.                                                                                                                                                                                  | Protects you   |
| [Transfer to a new phone](https://faq.whatsapp.com/1374388673551120)     | A friend's or relative's number can't be used to receive the verification code.                                                                                                                                                                                                      | Protects you   |
| [Accessibility features](https://faq.whatsapp.com/3614672068767202)      | "I am deaf" describes the person, not an audio fault. Work out what they're asking and answer with steps they can read.                                                                                                                                                              | Protects you   |
| [WhatsApp Plus subscriptions](https://faq.whatsapp.com/1480290710396618) | An unofficial app also called WhatsApp Plus exists. It isn't WhatsApp's and isn't supported.                                                                                                                                                                                         | Protects you   |
| [Voice calls](https://faq.whatsapp.com/490531153094849)                  | Don't tell people to reinstall, delete or migrate the app to fix a calling problem.                                                                                                                                                                                                  | Housekeeping   |
| [Phone number format](https://faq.whatsapp.com/1294841057948784)         | In Arabic, the words people use for "verifying my account" usually mean the paid blue badge, not a phone number check.                                                                                                                                                               | Housekeeping   |
