---
title: AI is forcing us to pay off 40 years of technical debt
link: https://spectrum.ieee.org/ai-in-cybersecurity-php2shell-vulnerability
source: hnrss-org
published: 2026-11-04T11:52:17Z
updated: 2026-11-04T11:52:17Z
first_seen: 2026-11-04T15:43:16.719955117Z
authors:
- pseudolus
content: extracted
html: 2026-11-04-ai-is-forcing-us-to-pay-off-40-years-of-technical-debt.html
remote_preview:
  url: https://spectrum.ieee.org/media-library/image.jpg?id=68544156&width=1200&height=600&coordinates=0%2C312%2C0%2C313
---

In late October, a [slew](https://www.wiz.io/blog/cve-2026-111497-brotli-rce-node-js) of [major](https://thehackernews.com/2026/10/ruby-fixes-three-regex-engine-bugs.html) [cybersecurity](https://scotthelme.co.uk/eight-bits-of-trust-emudhras-ca-keys-cracked-in-milliseconds/) issues came to light. But none were bigger than the so-called [PHP2Shell vulnerability](https://thephp.foundation/blog/2026/10/21/todays-php-security-release-and-how-were-strengthening-php-security/#what-we-need-from-you), which put a plethora of websites, including those of [NASA](https://spectrum.ieee.org/tag/nasa), [Wikipedia](https://spectrum.ieee.org/tag/wikipedia), the White House, and all other sites that run on [Wordpress](https://spectrum.ieee.org/tag/wordpress) or Drupal (among others) at risk of exploits capable of both reading sensitive data and running malicious code on the host server.

As far as anyone is aware, disaster has been averted: The hack was discovered by [cybersecurity](https://spectrum.ieee.org/ai-cybersecurity-mythos) researchers who reported it to the PHP Foundation, and a patch was released before any bad actors got their hands on the exploit. However, PHP2Shell may be the canary in the coal mine, serving as a warning sign for what’s to come in the age of AI.

“One of the things we’re witnessing now is that all the legacy software that was written by humans over the last 40 years is being exposed,” says [Matt Suiche](https://www.linkedin.com/in/msuiche/), AI security research lead at the [cybersecurity](https://spectrum.ieee.org/tag/cybersecurity) startup [Tolmo](https://tolmo.com/). “Now it is time for the technical debt to be paid off.”

## PHP Legacy Bug and Memory Errors

PHP, which originally stood for “personal home page,” is a programming language created back in 1994 and largely used by legacy sites. The bug underlying the PHP2Shell vulnerability was [reported](https://calif.io/php2shell/) to the PHP Foundation as far back as 2024. The bug caused a common type of issue called an out-of-memory error, in which the amount of memory allocated to a piece of data is too small to hold the data. Normally, this type of error causes the program to crash—an unpleasant but not inherently dangerous result. And so the bug remained unaddressed.

“If you look at any at any point of time, there will be a bunch of bug reports that are waiting to be fixed, which are just crashes. Most people don’t take them very seriously,” says [Suman Jana](https://www.cs.columbia.edu/~suman/), an associate professor of computer science at [Columbia University](https://spectrum.ieee.org/tag/columbia-university).

“It’s just that it’s a lot of very specific reasoning that is painful to do. And AI, I think, has done a very good job at that.” **–Suman Jana, Columbia University**

Earlier this year, the cybersecurity firm [Calif](https://calif.io/) offered to independently audit PHP, sponsored by [OpenAI](https://openai.com/). Within two weeks, a researcher at Calif, aided by AI tools, found a way to use the out-of-memory bug to run arbitrary code on the host server, allowing a malicious actor to install [malware](https://spectrum.ieee.org/tag/malware) or read critical data. Now, it was no longer a minor bug—it was a critical vulnerability, and the Calif team alerted the PHP foundation to start working on a patch immediately.

Jana says actually exploiting the bug requires carefully working out a way to jump to a specific point in the memory for a particular program running on the server, and work out exactly which bits to overwrite in what order to gain control of the system. “It’s not really rocket science,” he says. “It’s just that it’s a lot of very specific reasoning that is painful to do. And AI, I think, has done a very good job at that.”

To be clear, the researchers at Calif (as well as [OpenAI](https://spectrum.ieee.org/tag/openai) and Wiz, a cybersecurity company that confirmed and expanded the vulnerabilities) had deep knowledge of both cybersecurity and the PHP language. “I’m not claiming that AI can do it all by itself. It clearly cannot,” Jana says. “But it is improving a lot, so the top 0.1 percent \[of experts\] can get a lot more stuff done, and also just below the 0.1 percent now will be able to go there without understanding all the nitty-gritty details.”

## AI Tools and Formal Verification in Security

After the exploits were disclosed to the PHP Foundation, a team of four engineers spent two months developing a patch to release as quickly as possible.

“There is a fundamental asymmetry in finding bugs and fixing bugs,” Jana says. “Because when you’re fixing bugs you have to make sure that you know all parts of that attack are closed, and also that you don’t break any existing functionality.”

This asymmetry puts a lot of strain on the defenders, especially for open-source software such as PHP. [Maintaining open-source projects](https://spectrum.ieee.org/open-source-crisis) that don’t have clear revenue streams despite being widely used is a long-standing challenge that precedes AI hacking tools. But those tools make the issue more pressing than ever.

One way to stay ahead of such issues going forward is to empower the defenders with more money and access to frontier cybersecurity [AI models](https://spectrum.ieee.org/tag/ai-models). “These frontier labs are donating some of their token budgets to look into these bugs, which I think is great,” Jana says. “I think that should happen more, either through some concerted effort or through donations.”

Jana is also optimistic that formal verification tools to automate the patching side of the equation are also advancing quickly, including a new [release](https://www.anthropic.com/research/formal-verification) from Anthropic on 22 October. “The patch is LLM-generated, and then an LLM generates the proof that the patch is correct,” says Jana. “The only thing not done by an LLM is the proof checker, because somebody has to check the proof, because LLMs hallucinate. So, you want something that is formal.”

Hopefully, the combination of specialized AI models and formal verification tools will make patching vulnerabilities nearly as automatable as finding them.

[Matthew Green](https://engineering.jhu.edu/faculty/matthew-green/), an associate professor of computer science at [Johns Hopkins University](https://spectrum.ieee.org/tag/johns-hopkins-university) [Information Security](https://spectrum.ieee.org/tag/information-security) Institute, is less optimistic. “Nobody’s going to fix this problem until something really bad happens,” he says. “And PHP2Shell wasn’t bad enough to be really bad. So when something really bad happens, which is going to be maybe sometime in early 2027, something bad that normal people are upset by, then suddenly people will start doing this. We’re going to fund everything.”

In the meantime, Green says, update your software. “If your phone is like, ‘Hey, there’s a security update available, do you want to install it,’ don’t wait till next week. Just do it.”

One thing’s for sure—PHP2Shell is just the beginning. “This won’t be the last one for sure,” Jana says. “It’s just who finds it. Hopefully, the white-hat [hackers](https://spectrum.ieee.org/tag/hackers) find it first, like this one.”
