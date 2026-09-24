---
title: VSCode's SSH Agent Is Bananas (2025)
link: https://fly.io/blog/vscode-ssh-wtf/
source: hnrss-org-frontpage
published: 2026-09-23T21:01:48Z
updated: 2026-09-23T21:01:48Z
first_seen: 2026-09-24T00:51:22.005242350Z
authors:
- Rapzid
summary: 'Article URL: https://fly.io/blog/vscode-ssh-wtf/ Comments URL: https://news.ycombinator.com/item?id=49822555 Points: 108 # Comments: 72'
content: extracted
html: 2026-09-23-vscode-s-ssh-agent-is-bananas-2025.html
preview:
  file: 2026-09-23-vscode-s-ssh-agent-is-bananas-2025.preview-5a16a743926b.webp
  width: 256
  height: 256
  color: '#b5a4df'
images:
- source: https://fly.io/static/images/fly-social-square.webp
  original:
    file: 2026-09-23-vscode-s-ssh-agent-is-bananas-2025.image-57c69b9027d5.webp
    width: 1200
    height: 1200
  color: '#fefefe'
- source: https://fly.io/static/images/thomas.webp
  original:
    file: 2026-09-23-vscode-s-ssh-agent-is-bananas-2025.image-ae9eabda6b6d.webp
    width: 192
    height: 192
  color: '#009afe'
---

Author

![Thomas Ptacek](https://fly.io/static/images/thomas.webp)

Name

Thomas Ptacek

@tqbf

[@tqbf](https://twitter.com/tqbf)

We’re interested in getting integrated into the flow VSCode uses to do remote editing over SSH, because everybody is using VSCode now, and, in particular, they’re using forks of VSCode that generate code with LLMs.

”hallucination” is what we call it when LLMs get code wrong; “engineering” is what we call it when people do.

LLM-generated code is [useful in the general case](https://nicholas.carlini.com/writing/2024/how-i-use-ai.html "") if you know what you’re doing. But it’s ultra-useful if you can close the loop between the LLM and the execution environment (with an “Agent” setup). There’s lots to say about this, but for the moment: it’s a semi-effective antidote to hallucination: the LLM generates the code, the agent scaffolding runs the code, the code generates errors, the agent feeds it back to the LLM, the process iterates.

So, obviously, the issue here is you don’t want this iterative development process happening on your development laptop, because LLMs have boundary issues, and they’ll iterate on your system configuration just as happily on the Git project you happen to be working in. A thing you’d really like to be able to do: run a closed-loop agent-y (“agentic”? is that what we say now) configuration for an LLM, on a clean-slate Linux instance that spins up instantly and that can’t screw you over in any way. You get where we’re going with this.

Anyways! I would like to register a concern.

Emacs hosts the spiritual forebearer of remote editing systems, a blob of hyper-useful Elisp called [“Tramp”](https://www.gnu.org/software/tramp/ ""). If you can hook Tramp up to any kind of interactive environment — usually, an SSH session — where it can run Bourne shell commands, it can extend Emacs to that environment.

So, VSCode has a feature like Tramp. Which, neat, right? You’d think, take Tramp, maybe simplify it a bit, switch out Elisp for Typescript.

You’d think wrong!

Unlike Tramp, which lives off the land on the remote connection, VSCode mounts a full-scale invasion: it runs a Bash snippet stager that downloads an agent, including a binary installation of Node.

I *think* this is [the source code](https://github.com/microsoft/vscode/tree/c9e7e1b72f80b12ffc00e06153afcfedba9ec31f/src/vs/server/node "")?

The agent runs over port-forwarded SSH. It establishes a WebSockets connection back to your running VSCode front-end. The underlying protocol on that connection can:

- Wander around the filesystem
- Edit arbitrary files
- Launch its own shell PTY processes
- Persist itself

In security-world, there’s a name for tools that work this way. I won’t say it out loud, because that’s not fair to VSCode, but let’s just say the name is murid in nature.

I would be a little nervous about letting people VSCode-remote-edit stuff on dev servers, and apoplectic if that happened during an incident on something in production.

It turns out we don’t have to care about any of this to get a custom connection to a Fly Machine working in VSCode, so none of this matters in any kind of deep way, but: we’ve decided to just be a blog again, so: we had to learn this, and now you do too.

Next post ↑

[Did Semgrep Just Get A Lot More Interesting?](https://fly.io/blog/semgrep-but-for-real-now/)

Previous post ↓

[AI GPU Clusters, From Your Laptop, With Livebook](https://fly.io/blog/ai-gpu-clusters-from-your-laptop-livebook/)
