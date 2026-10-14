---
title: 'Show HN: Pulse Local, a Mac budgeting app whose AI models run on your machine'
link: https://pulseos.finance/
source: hnrss-org
published: 2026-10-13T23:13:23Z
updated: 2026-10-13T23:13:23Z
first_seen: 2026-10-14T07:29:10.775073046Z
authors:
- niztk
summary: 'Hi HN, I''m Niz. Pulse Local is a budgeting app for Mac (Apple Silicon, beta) that answers questions about your spending with models that run on your machine. Local mode needs no account. The ledger and your questions stay on the machine, and the app sends no analytics or crash logs. I felt there are not enough consumer apps that run locally with no calls to the outside world that help us manage sensitive corners of our lives. This app in its local variant only downloads a couple of models and occasional updates, it does not send any analytics nor data. Features-wise, Pulse local is a complete budgeting app that offers an assistant built on top of a fine-tuned model and thousands of rules embedded in the code itself. I''m sharing it for free with options to help me continue experimenting with fine-tuning and expanding along this direction. How it works: you import a CSV from any bank, in any currency. When you ask a question, a tuned Qwen3 0.6B model reads it into a structured query, and code, not the model, does the money math against the ledger. A 4B checker model double-checks each money answer; when the two readings disagree, the app says it is not sure instead of guessing. That change took wrong answers from about one in five to 5 in 494 questions across our last three tests. The write-up covers how we test, the designs that failed, and the limits of the claim: https://pulseos.finance/pulse-local/writing/agree-or-not-sur... The tuned weights are on Hugging Face (PulseOS/pulse-local-qwen3-0.6b). Limitations: the first start downloads about 2.9 GB of models. With the checker on, a money answer takes 12 to 14 seconds (you can turn the checker off). Small models can still misread an open-ended question. The app answers in English. Mac only for now; Windows is next. The app is free. An optional Connected mode (Sync Pass, $29/year) syncs US banks through Plaid; transactions pass through our server''s memory and are not stored. Feedback welcome, especially on wrong answers and on questions it should handle but does not. Comments URL: https://news.ycombinator.com/item?id=50080275 Points: 101 # Comments: 55'
content: feed
html: 2026-10-13-show-hn-pulse-local-a-mac-budgeting-app-whose-ai-models-run.html
---

Hi HN, I'm Niz. Pulse Local is a budgeting app for Mac (Apple Silicon, beta) that answers questions about your spending with models that run on your machine. Local mode needs no account. The ledger and your questions stay on the machine, and the app sends no analytics or crash logs.

I felt there are not enough consumer apps that run locally with no calls to the outside world that help us manage sensitive corners of our lives. This app in its local variant only downloads a couple of models and occasional updates, it does not send any analytics nor data.

Features-wise, Pulse local is a complete budgeting app that offers an assistant built on top of a fine-tuned model and thousands of rules embedded in the code itself. I'm sharing it for free with options to help me continue experimenting with fine-tuning and expanding along this direction.

How it works: you import a CSV from any bank, in any currency. When you ask a question, a tuned Qwen3 0.6B model reads it into a structured query, and code, not the model, does the money math against the ledger. A 4B checker model double-checks each money answer; when the two readings disagree, the app says it is not sure instead of guessing. That change took wrong answers from about one in five to 5 in 494 questions across our last three tests. The write-up covers how we test, the designs that failed, and the limits of the claim: [https://pulseos.finance/pulse-local/writing/agree-or-not-sur...](https://pulseos.finance/pulse-local/writing/agree-or-not-sure)

The tuned weights are on Hugging Face (PulseOS/pulse-local-qwen3-0.6b).

Limitations: the first start downloads about 2.9 GB of models. With the checker on, a money answer takes 12 to 14 seconds (you can turn the checker off). Small models can still misread an open-ended question. The app answers in English. Mac only for now; Windows is next.

The app is free. An optional Connected mode (Sync Pass, $29/year) syncs US banks through Plaid; transactions pass through our server's memory and are not stored.

Feedback welcome, especially on wrong answers and on questions it should handle but does not.

* * *

Comments URL: [https://news.ycombinator.com/item?id=50080275](https://news.ycombinator.com/item?id=50080275)

Points: 101

\# Comments: 55
