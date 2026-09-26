---
title: The GNOME LLM Policy That I Want
link: https://blogs.gnome.org/alatiera/2026/09/23/the-gnome-llm-policy-that-i-want/
source: lobste-rs-top-1w
published: 2026-09-23T10:28:14Z
updated: 2026-09-23T10:28:14Z
first_seen: 2026-09-26T14:43:23.796397423Z
authors:
- blogs.gnome.org via joshsharp
labels:
- linux
- practices
- vibecoding
summary: Comments
content: extracted
html: 2026-09-23-the-gnome-llm-policy-that-i-want.html
preview:
  file: 2026-09-23-the-gnome-llm-policy-that-i-want.preview-c05b25af7827.webp
  width: 256
  height: 256
  alt: adwaita-l
  color: '#293df9'
images:
- source: https://blogs.gnome.org/alatiera/files/2026/09/adwaita-l-scaled.jpg
  original:
    file: 2026-09-23-the-gnome-llm-policy-that-i-want.image-79d3a6c130d9.jpg
    width: 2560
    height: 2560
  color: '#3629fc'
- source: https://blogs.gnome.org/alatiera/files/2026/09/adwaita-l-825x510.jpg
  original:
    file: 2026-09-23-the-gnome-llm-policy-that-i-want.image-ab9a90904698.jpg
    width: 825
    height: 510
  color: '#3528fc'
---

![](https://blogs.gnome.org/alatiera/files/2026/09/adwaita-l-825x510.jpg)

KDE is on the news because of [a *controversial* proposal to define an official “AI” (LLM) policy](https://invent.kde.org/plasma/plasma-workspace/-/work_items/187) ([archived link](https://archive.vn/6YccJ)). Other [projects have tried](https://www.debian.org/vote/2026/vote_002) their hand at similar policies and stances but, in my opinion, they miss the mark about the goal of such initiatives. I think that the point of these statements is shaping *social norms* and not micro-managing developer workflows. They should be about signalling what kind of behavior we want, and what kind we reject.

This proposal does not go into detail about the many problems that LLM have caused to society, workers, the environment. It goes without saying that all these ills are fundamentally opposed to the humanist spirit of GNOME.

With all that in mind, here is what I *personally* think a GNOME LLM policy could be:

## A GNOME Project LLM Policy

```
The GNOME Project prioritizes the social and human aspects of
collective software creation. Therefore:

1. LLMs ("AI") can not be used to create or modify
anything submitted to GNOME, or hosted on GNOME infrastructure.

You might be asked to prove your code meets this requirement.
You might be banned for trying to circumvent this policy.
```

#### Example Guidelines for Contributors

These are just a draft of the kind of criteria one could use to evaluate if a submission fits the policy.

- You must be able to personally reason and explain your changes
- You must be able to demonstrate knowledge of the problem space you are working on
- You must solve the underlying issue, not just its symptoms
- You must respect the time of fellow contributors
- You must not impersonate yourself through chatbots, agents, or other automated systems

## This Is About The Future Of GNOME

GNOME is not just software that happens to ship every six months. That is just a delusion we have been holding up for the last 30 years to keep our loose group of colleagues, friends, and acquaintances, together.

GNOME exists as a collective that find joy in reaching beyond our individual limitations to achieve something bigger. These people, this joy, are the whole point of the project. Contributors are not payroll, a liability, that we hope to downsize next quarter.

*“Come do free labor for a handful of corporations by reviewing chatbot output in your free time”* is not an attractive proposition to young talented people in 2026. If we want GNOME to continue we need to create an attractive and inviting social space where people are valued as people.

Just like the Foundation is moving to individual donations to stop depending on just a handful of companies, we need to look for the next 100 people that will donate a tiny bit of their time, instead of hoping that corporations will keep 10 overworked engineers on staff. We already have seen how companies will happily [abandon](https://blogs.gnome.org/chergert/2026/02/06/mid-life-transitions/)\
 a [whole chunk](https://www.hadess.net/2023/08/new-responsibilities.html) of GNOME on a whim.

GNOME is not just software, and it should protect the social and human aspects that make it special. Our success metric is the community and social bonds we create. In the most literal sense GNOME is about [the journey and the friends we make along the way](https://www.youtube.com/watch?v=1n3n2Ox4Yfk).

Free Palestine.

## FAQ

#### How do you enforce this?

You can not. People will still send LLM generated code. This policy makes it explicit that we do not welcome these careless submissions. We have a Code of Conduct that is 80% about telling other people what our values are, and 20% about handling unwanted behavior (“enforcing”). This is similar.

#### What if people simply lie about not using LLMs?

This is the same problem as authorship, in the copyright sense. Whenever we receive new code we have to assume that “beyond a reasonable doubt” said code has been authored by the person contributing it. We make our best guess. The attached guidelines are a suggestion to make these new guesses.

#### Ok. But what if people are really good at lying?

This policy is about the majority that will not even try to lie. See previous questions.
