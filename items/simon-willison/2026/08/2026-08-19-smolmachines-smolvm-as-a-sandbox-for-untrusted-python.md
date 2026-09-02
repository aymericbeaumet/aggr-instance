---
title: smolmachines / smolvm as a sandbox for untrusted Python & JavaScript
link: https://simonwillison.net/2026/Aug/19/smolmachines-untrusted-sandbox/
source: simon-willison
published: 2026-08-19T23:16:00Z
updated: 2026-08-19T23:16:00Z
first_seen: 2026-09-02T14:07:29.769486161Z
tags:
- research
- sandboxing
- ai
- github-actions
- generative-ai
- llms
- coding-agents
- claude-mythos-fable
summary: 'Research: smolmachines / smolvm as a sandbox for untrusted Python & JavaScript I tasked Claude Fable 5 running in Claude Code for web with the following research task: Put https://smolmachines.com through its paces as a fast secure sandbox. Explore what it would take to use this to run untrusted Python and JavaScript code in a way that is limited in what RAM and CPU time it can take up (protection against "while true") with no network access and filesystem access only to designated files Goal is to be able to use this to execute user-provided tasks for things like data transformations It quickly ran into a problem: the Claude Code for web environment can''t run smol machines. Quoting the notes it wrote: This Claude Code container: Linux 6.18.5-fc-v20 (itself a Firecracker guest), 4 vCPU, 15GB RAM. No /dev/kvm, no vmx/svm CPU flags → no nested virt. smolvm machine run fails as expected: "kvm not available". Plan B: GitHub Actions ubuntu runners DO expose /dev/kvm → run the real test battery via a temporary workflow on this branch, collect logs, remove workflow in final commit. And Plan B is what it did, installing smolvm and running these tests directly in a GitHub Actions runner against that branch. That was a creative solution to the environmental limits posed by Claude Code for web. Another example of Fable being relentlessly proactive. Tags: research, sandboxing, ai, github-actions, generative-ai, llms, coding-agents, claude-mythos-fable'
content: feed
html: 2026-08-19-smolmachines-smolvm-as-a-sandbox-for-untrusted-python.html
---

**Research:** [smolmachines / smolvm as a sandbox for untrusted Python & JavaScript](https://github.com/simonw/research/tree/main/smolmachines-untrusted-sandbox#readme)

I tasked Claude Fable 5 running in Claude Code for web with the following research task:

> `Put https://smolmachines.com through its paces as a fast secure sandbox. Explore what it would take to use this to run untrusted Python and JavaScript code in a way that is limited in what RAM and CPU time it can take up (protection against "while true") with no network access and filesystem access only to designated files`
>
> `Goal is to be able to use this to execute user-provided tasks for things like data transformations`

It quickly ran into a problem: the Claude Code for web environment can't run [smol machines](https://smolmachines.com). Quoting the [notes it wrote](https://github.com/simonw/research/blob/5e6861e54441472d194de96b49b901fd99ebc153/smolmachines-untrusted-sandbox/notes.md#environment-check):

> - This Claude Code container: Linux 6.18.5-fc-v20 (itself a Firecracker guest), 4 vCPU, 15GB RAM. **No /dev/kvm, no vmx/svm CPU flags** → no nested virt.
> - `smolvm machine run` fails as expected: "kvm not available".
> - Plan B: GitHub Actions ubuntu runners DO expose /dev/kvm → run the real test battery via a temporary workflow on this branch, collect logs, remove workflow in final commit.

And Plan B is [what it did](https://github.com/simonw/research/blob/5e6861e54441472d194de96b49b901fd99ebc153/.github/workflows/smolvm-sandbox-test.yml), installing smolvm and running [these tests](https://github.com/simonw/research/blob/5e6861e54441472d194de96b49b901fd99ebc153/smolmachines-untrusted-sandbox/run-tests.sh) directly in a GitHub Actions runner against that branch.

That was a creative solution to the environmental limits posed by Claude Code for web. Another example of Fable being [relentlessly proactive](https://simonwillison.net/2026/Jun/11/fable-is-relentlessly-proactive/).

Tags: [research](https://simonwillison.net/tags/research), [sandboxing](https://simonwillison.net/tags/sandboxing), [ai](https://simonwillison.net/tags/ai), [github-actions](https://simonwillison.net/tags/github-actions), [generative-ai](https://simonwillison.net/tags/generative-ai), [llms](https://simonwillison.net/tags/llms), [coding-agents](https://simonwillison.net/tags/coding-agents), [claude-mythos-fable](https://simonwillison.net/tags/claude-mythos-fable)
