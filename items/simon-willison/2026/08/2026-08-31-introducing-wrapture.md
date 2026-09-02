---
title: Introducing wrapture
link: https://simonwillison.net/2026/Aug/31/introducing-wrapture/
source: simon-willison
published: 2026-08-31T23:59:36Z
updated: 2026-08-31T23:59:36Z
first_seen: 2026-09-02T14:07:29.769486161Z
tags:
- graham-dumpleton
- monkey-patching
- python
- testing
- pytest
- observability
- ai-assisted-programming
- agentic-engineering
- opentelemetry
summary: 'Introducing wrapture New from Graham Dumpleton (of wrapt, mod_wsgi, and New Relic''s Python agent fame), who describes Wrapture as taking the monkeypatching ideas from wrapt and extending them to apply to testing and tracing at the same time. Wrapture (full documentation here) makes it easy to wrap any function or method such that all access can be traced, or can be overridden to return a different value. It acts as both an alternative to unittest.mock and a way to implement tracing against an existing project: Attaching observation to code you do not control, recording what flows through it, and doing so without disturbing the program being watched, is a problem I have never really stopped thinking about. Wrapture includes OpenTelemetry support and even has an entirely configuration-based mechanism for adding tracing to an existing Python project, which looks like this: capture = "summary" [[observe]] target = "domain:Calculator" name = ["outer", "inner"] [[sink]] type = "jsonlines" path = "trace.jsonl" This is still a very young project - just a few weeks old - but it''s off to a very promising start. Interestingly, this is also Graham''s first attempt at large entirely agent-driven project: Every line of code and documentation in wrapture was written by an AI assistant working under my direction. I want to be upfront about that, and equally upfront about what it was not. This was not vibe coding, where a one-shot prompt produces a pile of generated code and the person driving hopes for the best because they lack the knowledge to judge what came back. Vibe coding has earned its bad reputation. I engineered wrapture carefully from the start. I have spent a long time in this particular corner of Python and knew exactly what the result needed to be, and the AI was the means of producing it rather than the source of the design. In a follow-up post, Unit testing with wrapture, Graham shows the testing patterns supported by the new library: def test_stub_with_wrapture(): with wrapture.binding( Gateway, "charge" ).on_call.returns({ "id": "stub", "amount": 0} ): assert OrderService().place( 500 )["id"] == "stub" And this neat example of a test that calls and then modifies the return value from the original method: def test_pinned_result_with_wrapture(): charge = wrapture.binding( Gateway, "charge" ) charge.on_call.transforms_result( lambda r: {**r, "id": "ch_TEST"} ) with charge: assert OrderService().place( 500 ) == { "id": "ch_TEST", "amount": 500 } (In both of these examples the OrderService().place(...) method calls Gateway().charge(...).) Tags: graham-dumpleton, monkey-patching, python, testing, pytest, observability, ai-assisted-programming, agentic-engineering, opentelemetry'
content: feed
html: 2026-08-31-introducing-wrapture.html
---

**[Introducing wrapture](https://grahamdumpleton.me/posts/2026/08/introducing-wrapture/)**

New from Graham Dumpleton (of [wrapt](https://pypi.org/project/wrapt/), mod\_wsgi, and New Relic's Python agent fame), who describes Wrapture as taking the monkeypatching ideas from wrapt and extending them to apply to testing and tracing at the same time.

Wrapture ([full documentation here](https://wrapture.readthedocs.io/)) makes it easy to wrap any function or method such that all access can be traced, or can be overridden to return a different value.

It acts as both an alternative to `unittest.mock` and a way to implement tracing against an existing project:

> Attaching observation to code you do not control, recording what flows through it, and doing so without disturbing the program being watched, is a problem I have never really stopped thinking about.

Wrapture includes [OpenTelemetry support](https://wrapture.readthedocs.io/en/latest/otel-export.html) and even has an entirely configuration-based mechanism for adding tracing to an existing Python project, which looks like this:

capture = "summary"

[[observe]]
target = "domain:Calculator"
name = ["outer", "inner"]

[[sink]]
type = "jsonlines"
path = "trace.jsonl"

This is still a very young project - just a few weeks old - but it's off to a very promising start.

Interestingly, this is also Graham's first attempt at large entirely agent-driven project:

> Every line of code and documentation in wrapture was written by an AI assistant working under my direction. I want to be upfront about that, and equally upfront about what it was not. This was not vibe coding, where a one-shot prompt produces a pile of generated code and the person driving hopes for the best because they lack the knowledge to judge what came back. Vibe coding has earned its bad reputation. I engineered wrapture carefully from the start. I have spent a long time in this particular corner of Python and knew exactly what the result needed to be, and the AI was the means of producing it rather than the source of the design.

In a follow-up post, [Unit testing with wrapture](https://grahamdumpleton.me/posts/2026/09/unit-testing-with-wrapture/), Graham shows the testing patterns supported by the new library:

def test_stub_with_wrapture():
    with wrapture.binding(
        Gateway, "charge"
    ).on_call.returns({
        "id": "stub", "amount": 0}
    ):
        assert OrderService().place(
            500
        )["id"] == "stub"

And this neat example of a test that calls and then modifies the return value from the original method:

def test_pinned_result_with_wrapture():
    charge = wrapture.binding(
        Gateway, "charge"
    )
    charge.on_call.transforms_result(
        lambda r: {**r, "id": "ch_TEST"}
    )
    with charge:
        assert OrderService().place(
           500
        ) == {
            "id": "ch_TEST", "amount": 500
        }

(In both of these examples the `OrderService().place(...)` method calls `Gateway().charge(...)`.)

Tags: [graham-dumpleton](https://simonwillison.net/tags/graham-dumpleton), [monkey-patching](https://simonwillison.net/tags/monkey-patching), [python](https://simonwillison.net/tags/python), [testing](https://simonwillison.net/tags/testing), [pytest](https://simonwillison.net/tags/pytest), [observability](https://simonwillison.net/tags/observability), [ai-assisted-programming](https://simonwillison.net/tags/ai-assisted-programming), [agentic-engineering](https://simonwillison.net/tags/agentic-engineering), [opentelemetry](https://simonwillison.net/tags/opentelemetry)
