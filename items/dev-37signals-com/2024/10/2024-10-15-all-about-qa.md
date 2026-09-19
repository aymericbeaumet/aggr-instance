---
title: All about QA
link: https://dev.37signals.com/all-about-qa/
source: dev-37signals-com
published: 2024-10-15T17:00:00Z
updated: 2024-10-15T17:00:00Z
first_seen: 2026-09-19T21:30:23.395188495Z
authors:
- Michael Berger, Gabriel Monette
summary: A look at how we test our products within the Shape Up framework.
content: extracted
html: 2024-10-15-all-about-qa.html
preview:
  file: 2024-10-15-all-about-qa.preview-a1075b5d9297.webp
  width: 256
  height: 134
  alt: 37signals Dev
  color: '#090909'
images:
- source: https://dev.37signals.com/assets/images/opengraph/all-about-qa.png
  original:
    file: 2024-10-15-all-about-qa.image-f716399bfad4.png
    width: 2400
    height: 1260
  color: '#000000'
extra:
  thumbnail: https://dev.37signals.com/assets/images/opengraph/all-about-qa.png
---

Quality Assurance (QA) is a team of two at 37signals: Michael, who created the department 12 years ago, and Gabriel, who joined the team in 2022. Together, we have a hand in projects across all of our products, from kickoff to release. Our goal is to help designers and programmers ship their best work. Our process revolves around manual testing and has been tuned to match the rhythm of Shape Up. Here, we’ll share the ins and outs of our methods and touch on a few of the tools we use along the way.

* * *

## Kicking things off

At 37signals we run projects in six-week cycles informed by [Shape Up](https://basecamp.com/shapeup). At the beginning of each cycle, Brian, our Head of Product, posts a kick-off message detailing what we plan to ship. This usually consists of new features and improvements for [Basecamp](https://basecamp.com), [HEY](https://www.hey.com/), or a [ONCE](https://once.com/) product. Each gets its own Basecamp project, and each project includes a pitch. The pitch lays out the problem or need, a proposed solution, and the [“appetite”](https://basecamp.com/shapeup/1.2-chapter-03#setting-the-appetite) or time budget. The kick-off is also QA’s cue to dive in! We offer early feedback, ask questions or illuminate things that aren’t covered, and give extra consideration to flows and interactions that may require extra work on the accessibility front. We then step back and let the teams focus, design, and build things for a while.

* * *

## The right time to test

We wait until the feature or product reaches a usable state to start testing in earnest. This helps us keep a fresh perspective, unencumbered by the knowledge of compromises made along the way.

We use a [Card Table](https://basecamp.com/learn/beyond-the-basics/card-table) within our QA Team project to track what’s ready for testing or in progress. Teams add a card to the *Ready for QA* (Triage) section when the time is right. The table is kept simple with just two columns, *In Progress* and *Pending Input*, for when we’ve completed our test run and the team is addressing the feedback. Depending on the breadth and complexity of the work being tested, this flow can take anywhere from a few hours to a few days.

* * *

## A holistic approach to QA

Once we take on a request, we explore and scrutinize the feature much like an (extremely zealous!) customer would. We want to help teams ship the most polished features they can. We look out for bugs of all kinds: performance issues, visual glitches, unexpected changes, and so on, but perhaps most importantly, we offer feedback on the usability of the feature. We guide our feedback with questions like:

- Is this feature easy to discover and access? Is it in the right spot?
- Does it interact in an unexpected way with another part of the app?
- How does the change play with our mobile apps?
- Does this solve the problem in a way that customers will find obvious?

Critically, what we raise with this type of QA testing are suggestions, not must-haves. The designer and programmer working on the feature make the call on what to address and what to shelve.

We document this feedback in a dedicated Card Table within the feature’s Basecamp project. The designer and programmer will then review the cards we’ve added to Triage and direct them to the *In Progress* and *Not Now* columns as appropriate. From *In Progress*, cards are moved to a column called *QA* to confirm fixed, then finally to *Done*.

* * *

## More focus, less bloat

Our overall approach to testing is guided exploration. We don’t maintain an exhaustive collection of test cases to dogmatically review each time we test a feature. We’ve tried using dedicated test plan tools and comprehensive spreadsheets of test cases upon test cases; the time spent certifying every little thing was considerable, yet it didn’t translate into finding more issues. Worse, it left us with less time to spend sitting with the feature in a more subjective way. We’ve landed on a more pragmatic approach.

We’ve boiled down the test plan to a concise list of considerations that live in Basecamp as to-do list templates, one for each product. Instead of a multitude of test cases, each template contains around 100 items. These act as pointers, touching on overall concepts (like commenting, dark mode, email notifications), specific areas of the app, and platform-specific considerations. We reflect on the work presented and how it ties into these areas. Some examples from recent projects have been:

- Did we update exporting to consider this new addition of time tracking entries?
- Are email notifications properly reflecting the new Steps feature we added to Card Table?
- How about print styles, do they look good?

We create a to-do list via the template directly in the project we are working on, and use that as our reference for reviewing the work. We also ask the feature team if there are areas that deserve extra attention. Being flexible and discerning about how much time and coverage we use in our testing allows us to cover anywhere from 4 to 12+ projects in a very short span of time.

We love working as a team of two and being able to riff on how to approach testing a feature. Sometimes, we divide and conquer; other times, both of us review the work. Fresh eyes provide a good chance of catching something new. Gabriel has a better knack for Android conventions and Michael for iOS, but we actively avoid over-specializing. Keeping up with multiple platforms requires extra effort, but it’s worth it when considering the consistency of the experience across all of them.

* * *

## Accessibility

As part of our review, we test the accessibility of the changes. We use a combination of keyboard navigation and at least one screen reader on each platform to vet how well the feature will work for someone who relies on accessible technology. We also use browser extensions like [axe](https://www.deque.com/axe/browser-extensions/) and [Accessibility Insights for Web](https://accessibilityinsights.io/docs/web/overview/) to validate semantics of the code and [Headings Map](https://addons.mozilla.org/en-US/firefox/addon/headingsmap/) to make sure heading levels are sequential. At times, we bring in customers who use a screen reader full-time to help us validate whether everything makes sense and learn where things can improve. Our new colleague, Bruno, is a full-time user of the NVDA screen reader and can offer this sort of direct feedback on how a feature or flow works for him.

* * *

## Explorations in tooling

A recent addition to our toolkit is a visual regression tool built on [BackstopJS](https://garris.github.io/BackstopJS/) with the help of our colleague Lewis. Whenever we review work, we can run the suite of tests — mostly a list of URLs for various pages around the app — first pointed to production, then against a beta environment where the new feature is staged. Any visual differences will be flagged in a report we review, then write up bug report cards for the team if needed.

* * *

## Walking the walk

Part of what enables us to keep our process minimal is that we use our products daily, both on the job and in our everyday lives. This affords us an intimate understanding of how they work and how they can be improved. We’re passionate about what we do. We find ourselves fortunate to work with each other and with so many talented colleagues.

* * *

We hope this post has given you some helpful insight into the way we do things! If you have questions or if there are topics you’d like us to cover in future posts, drop us an email at [qa@37signals.com](mailto:qa@37signals.com).
