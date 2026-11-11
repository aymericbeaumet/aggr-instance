---
title: aws-emulator
link: https://simonwillison.net/2026/Nov/11/aws-emulator/
source: simonwillison-net
published: 2026-11-11T21:18:30Z
updated: 2026-11-11T21:18:30Z
first_seen: 2026-11-11T21:51:32.649893059Z
labels:
- agentic-engineering
- ai
- amazon
- amazon-web-services
- docker
- generative-ai
- llms
- rust
- testing
- werner-vogels
summary: 'aws-emulator Finally, an official emulator from AWS to run tests against a local clone of their many different services without having to provision a paid API key to do so. It''s written in Rust by a huge swarm of agents - 29,684 commits in the first three weeks of the project. Swaminathan (Swami) Sivasubramanian wrote about how it came to be in a guest post on Werner Vogels'' blog. It currently requires Docker to run on macOS or Windows. I''m hoping they fix that in the future - it would be great if you could download a single binary and use that as part of a standard test suite in other languages and on other platforms. Tags: amazon, amazon-web-services, testing, werner-vogels, ai, rust, docker, generative-ai, llms, agentic-engineering'
content: extracted
html: 2026-11-11-aws-emulator.html
---

**[aws-emulator](https://github.com/aws/aws-emulator)**. Finally, an official emulator from AWS to run tests against a local clone of their many different services without having to provision a paid API key to do so.

It's written in Rust by a huge swarm of agents - 29,684 commits in the first three weeks of the project. Swaminathan (Swami) Sivasubramanian wrote about [how it came to be](https://www.allthingsdistributed.com/2026/11/aws-emulator-and-the-engineers-job-when-agents-write-the-code.html) in a guest post on Werner Vogels' blog.

It currently requires Docker to run on macOS or Windows. I'm hoping they fix that in the future - it would be great if you could download a single binary and use that as part of a standard test suite in other languages and on other platforms.
