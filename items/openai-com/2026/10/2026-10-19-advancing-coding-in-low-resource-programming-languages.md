---
title: Advancing Coding in Low-Resource Programming Languages
link: https://openai.com/index/low-resource-programming-languages
source: openai-com
published: 2026-10-19T12:00:00Z
updated: 2026-10-19T12:00:00Z
first_seen: 2026-10-19T23:02:59.728557376Z
labels:
- publication
summary: Learn how OpenAI is improving ChatGPT across low-resource programming languages, with new evaluations for software engineering, SQLite, and COBOL migration.
content: extracted
html: 2026-10-19-advancing-coding-in-low-resource-programming-languages.html
---

Our latest model advances bring stronger capabilities for building and migrating software in low-resource programming languages such as COBOL, Fortran, and Visual Basic. These languages have less code and documentation available for models to learn from than languages like Python and JavaScript, yet support critical infrastructure and essential services around the world. COBOL [supports](https://www.ibm.com/think/topics/cobol) more than 40% of online banking systems, 80% of in-person credit card transactions and 95% of ATM transactions, while Fortran is used in [weather forecasting](https://repository.library.noaa.gov/view/noaa/71120) and [nuclear reactor safety analysis](https://www.nrc.gov/education-regulatory-research/research/computer-codes). Improving AI’s ability to work in these languages could help teams maintain and modernize the software that essential services depend on.

This work is guided by feedback from developers and enterprise customers who have told us that coding models often struggle with these languages. Maintaining and modernizing this software can be difficult: documentation may be incomplete, original developers may have left the company, and important business rules may only exist in code. Whether teams are fixing bugs or migrating to another language, they need to preserve the behavior their systems depend on.

To measure progress, we developed two evaluations. MultiLangBench tests whether a model can build a SQLite-compatible database engine from scratch across 20 different programming languages. Our COBOL migration evaluation tests whether a model can migrate existing programs to another language while preserving their behavior. Together, they test two complementary skills: building new software and preserving what existing software does.

Our latest models show substantial improvements across both evaluations, delivering much higher accuracy at lower cost.

On MultiLangBench, GPT‑6 Astra achieves an average test pass rate of 87.4% across 20 programming languages, up from 26.8% for GPT‑5.6 Sol, with improvements in every language tested. GPT‑6 Luna also outperforms GPT‑5.6 Sol, reaching 40.3% at approximately 78% lower estimated API cost.

On our COBOL migration evaluation, GPT‑6.1 Sol achieves 86.6% accuracy at low reasoning effort, compared with just 5.4% for GPT‑5.6 Sol, while costing 39% less. At higher reasoning effort, accuracy reaches 97.9%, nearly saturating the evaluation.

Together, these results show that our models are becoming significantly better at working with a wide range of programming languages, while also becoming more cost-efficient.

## 1\. Building SQLite from scratch (MultiLangBench)

MultiLangBench asks a model to build a SQLite-compatible database engine in a specified programming language. The model must organize a codebase, manage state, handle errors, and add new features while preserving existing functionality.

To compare implementations across languages, we use database files as a shared interface. The candidate program receives an initial database and a SQL query, then produces a modified database. We use the reference SQLite implementation to check whether the resulting database has the expected contents. This lets us apply the same tests regardless of the language used to build the engine.

We adapted tests from SQLite’s existing test suite to this format. The evaluation measures how much working functionality a model can build over an extended coding session, reporting both test pass rate and API cost.

At maximum reasoning effort, GPT‑6 Astra achieves an average test pass rate of 87.4% across the 20 languages, up from 26.8% for GPT‑5.6 Sol. Improvements extend to every language tested, including COBOL (10.9% to 72.1%), OCaml (25.6% to 85.8%), Fortran (21.4% to 83.5%), and Visual Basic (27.5% to 89.4%). Astra scores above 83% in 19 of the 20 languages, with COBOL remaining the hardest at 72%.

GPT‑6 Luna offers a lower-cost option that also improves on GPT‑5.6 Sol in every language tested. At maximum reasoning effort, it reaches an average test pass rate of 40.3%. Gains include COBOL (10.9% to 17.1%), OCaml (25.6% to 34.1%), Fortran (21.4% to 31.4%), and Visual Basic (27.5% to 41.6%). These improvements come at an estimated average API cost of $0.77 per sample, compared with $3.48 for GPT‑5.6 Sol, approximately 78% lower.

We are releasing the MultiLangBench evaluation setup and test suite on [GitHub](https://github.com/openai/MultiLangBench), along with results for each language. Developers can inspect what the evaluation tests, review performance in the languages they use, and identify gaps that matter for their own work.

## 2\. Migrating a COBOL codebase

Our second evaluation asks models to migrate COBOL programs to C++, Java, Kotlin, Python, or Rust while preserving what each program does.

Successful migration requires understanding both the program’s business logic and COBOL’s specific language behavior. COBOL programs often process financial transactions and business records, where details such as rounding rules and missing-record handling reflect business requirements. For example, COBOL’s fixed-point arithmetic discards excess decimal digits unless rounding is explicitly requested. A rewrite that rounds those values instead can run without errors while producing different financial results.

To test whether these behaviors are preserved, we evaluate our models on hidden tests that require byte-for-byte parity on observable outputs. A migration is considered successful only if all tests pass. Behavior-preserving capabilities like these are useful for teams maintaining and modernizing older systems, though applying it to production software still requires expert review and testing against the system’s business rules and dependencies.

On this evaluation, GPT‑6.1 Sol achieves 86.6% accuracy at low reasoning effort, compared with GPT‑5.6 Sol’s 5.4% at the same setting at just 61% of the API cost. At higher reasoning effort, accuracy reaches 97.9%, bringing the evaluation close to saturation. These evaluation results demonstrate progress toward more accurate, affordable code migration for teams modernizing the software their businesses depend on.

## Advancing capabilities for enterprise software

These evaluations, alongside direct feedback from developers and enterprise customers, help us see where our models are improving and where they still fall short.

Our goal is to help models apply software engineering skills across more languages: understanding unfamiliar repositories, planning changes, using build tools, running tests, and debugging failures.

You can try our latest models through the API on coding tasks in the languages your team uses. Your feedback on what works, where the models struggle, and which languages or workflows we should focus on next helps shape how we train and evaluate models to better support the software you depend on.
