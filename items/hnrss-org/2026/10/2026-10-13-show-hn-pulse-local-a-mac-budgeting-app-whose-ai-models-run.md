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
content: extracted
html: 2026-10-13-show-hn-pulse-local-a-mac-budgeting-app-whose-ai-models-run.html
extra:
  content:interactive: true
---

A complete budgeting app.\
Not a privacy demo.
---

Budgets and targets, rules, rollover, splits, cash flow, forecast. What the most used budgeting apps do, all on your machine.

![Pulse Local, the Spending screen](https://pulseos.finance/_next/image?url=%2Fimages%2Fpulse-local%2Fspending.webp&w=2048&q=90)

Spending. One figure for the month, every category against its plan. Sample data.

We checked the money maths against six months of a real ledger. It matched to the cent.

“Local” usually means your data.\
Not your question.
---

Many apps keep your ledger on your disk. Then you ask a question, and it goes to an online AI service with the rows that answer it.

Ask about your money.\
Keep the conversation private.
---

Ask where your money went, compare months, or check what is left. Pulse adds up your own transactions to answer the spending questions it supports. With more than 16 GB of memory, a second model checks the answer first.

Check the month and category. Labels above each answer show what Pulse counted. Click one to change it.

Keep the conversation going. Ask “by merchant” for a breakdown, or name another month to compare.

Your decisions stay yours. Pulse explains your figures and budgeting terms. What you do next is up to you.

Know the limits. Pulse can misread a question. When it is not sure, it says so instead of guessing. Answers written by the AI are labelled.

See what you can ask.\
Then make it your own.
---

Real replies from the app, on sample transactions. Follow-ups break a total down or compare it with another month.

Ask September 2026

Pick a question on the right. The replies are real: each one came out of the app, answered against a sample ledger.

0 asked 0 double-checked 0 written by the model 0 B sent

Questions

How often is it wrong?\
We measure it in the open.
---

BudgetBench is our benchmark for budgeting chat. It checks every figure in every answer. We tested Pulse on 337 conversations that it had never seen.

When Pulse is not sure, it says so instead of showing a figure.

## Local mode is global.

Pick your country once. Pulse writes amounts the way you do: 1.234,56 € in Berlin, ¥1,311 in Tokyo. A bank file from anywhere imports. Each account keeps its own currency, and nothing is ever converted.

![](https://pulseos.finance/images/pulse-local/flags/us.svg)![](https://pulseos.finance/images/pulse-local/flags/ca.svg)![](https://pulseos.finance/images/pulse-local/flags/br.svg)![](https://pulseos.finance/images/pulse-local/flags/gb.svg)![](https://pulseos.finance/images/pulse-local/flags/ie.svg)![](https://pulseos.finance/images/pulse-local/flags/de.svg)![](https://pulseos.finance/images/pulse-local/flags/fr.svg)![](https://pulseos.finance/images/pulse-local/flags/it.svg)![](https://pulseos.finance/images/pulse-local/flags/nl.svg)![](https://pulseos.finance/images/pulse-local/flags/pt.svg)![](https://pulseos.finance/images/pulse-local/flags/se.svg)![](https://pulseos.finance/images/pulse-local/flags/ch.svg)![](https://pulseos.finance/images/pulse-local/flags/pl.svg)![](https://pulseos.finance/images/pulse-local/flags/tr.svg)![](https://pulseos.finance/images/pulse-local/flags/jp.svg)![](https://pulseos.finance/images/pulse-local/flags/kr.svg)![](https://pulseos.finance/images/pulse-local/flags/cn.svg)![](https://pulseos.finance/images/pulse-local/flags/hk.svg)![](https://pulseos.finance/images/pulse-local/flags/in.svg)![](https://pulseos.finance/images/pulse-local/flags/sg.svg)![](https://pulseos.finance/images/pulse-local/flags/au.svg)![](https://pulseos.finance/images/pulse-local/flags/nz.svg)![](https://pulseos.finance/images/pulse-local/flags/ae.svg)![](https://pulseos.finance/images/pulse-local/flags/tn.svg)![](https://pulseos.finance/images/pulse-local/flags/za.svg)![](https://pulseos.finance/images/pulse-local/flags/ng.svg)

### Bring the file your bank gives you

Many kinds of bank file. CSV, Excel, OFX, QFX, QIF, MT940, CAMT.053, Norma 43 and CODA. PDF statements too, in beta. Pulse reads them all on your machine.

Read by what is inside. The file name does not matter. Pulse finds the columns in any order. It also reads files saved in Japanese Shift\_JIS or in older Windows formats.

No silent guesses. Pulse checks the rows against the balances and totals in the file. When it is not sure of a sign, a date or the currency, it tells you before it saves anything.

See the result first. The preview shows the category each row will get, and the account and currency Pulse found. A file in another currency gets a question first. Nothing is converted.

[How to import a bank file](https://pulseos.finance/pulse-local/start)

Same app. Any price.\
You pick.
---

We are a small team. Once Pulse Local is on your machine, it costs us nothing to run. Pay what you like, or use it free. Every feature, either way.

Use it free

$0

Every feature, no account, no nagging.

Help us build

$20

A thank-you that pays for the next release.

Stripe, then the download.

Keep it going

$40

For people who want this around for years.

Stripe, then the download.

Your call

Any

Type the number yourself. One dollar counts.

Stripe, then the download.

One payment, not a subscription, and the app never checks what you paid. Bank sync costs us money to run, so it has its own price: the Sync Pass, $29 a year for three banks, $8 a year for each extra, up to 25.

Beta for Mac with Apple Silicon. Signed and checked by Apple, so it opens like any other app, and it keeps itself up to date. On Windows? [Tell us](mailto:niz@pulseos.finance?subject=Pulse%20Local%20beta&body=Which%20machine%20do%20you%20run%3F) and we will write when it is ready.

## What you give up.

No bank connection

Import a bank file from any bank, in any currency. PDF import is in beta. Live bank sync needs a server, so it lives in Connected mode.

One machine

No sync between devices. Your ledger exists nowhere else.

Mac first, English chat

Needs a Mac with Apple Silicon; older Intel Macs are not supported. Windows is next. Chat works best in English.

Small models, careful answers

Small AI models are weaker than the big online ones on open questions, so Pulse does the maths itself. A checked money answer takes 12 to 14 seconds. When Pulse is not sure, it says so. On BudgetBench, a sealed test of conversations it had never seen, about 7 answers in 100 still showed a wrong figure.
