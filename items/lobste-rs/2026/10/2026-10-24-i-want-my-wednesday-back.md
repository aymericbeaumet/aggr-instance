---
title: I want my Wednesday back
link: https://blog.luitjes.it/posts/i-want-my-wednesday-back/
source: lobste-rs
published: 2026-10-24T10:09:28Z
updated: 2026-10-24T10:09:28Z
first_seen: 2026-10-24T16:53:24.985256762Z
authors:
- blog.luitjes.it by lucasluitjes
labels:
- php
- security
summary: Comments
content: extracted
html: 2026-10-24-i-want-my-wednesday-back.html
---

*Or, why we’re not seeing mass-exploitation of php2shell yet.*

I saw wildly varying opinions on whether php2shell is serious, or if they cried wolf. Got curious, started digging into the PoCs. I’m not an exploit dev and maybe there were other factors, so take with a grain of salt. Egg on my face if mass exploitation starts right as I hit publish.

Best PoC of php2shell I’ve found: [https://github.com/9bie/php2shell-bypass-aslr-exp](https://github.com/9bie/php2shell-bypass-aslr-exp) (and a [fork with all docs translated to english](https://github.com/yanoshercohen/php2shell-bypass-aslr-exp-en))

To exploit, you need to do four consecutive requests to **the same php worker process**. And you need to know the exact php build (php version, OS version, architecture), or try exploits for every possible build.

What this PoC does is take a docker image that uses the default mod\_php number of workers, which is 150. Then it opens 149 connections that send half a request to keep the other workers busy, and does the four exploit requests against the 150th worker.

Even a moderately popular production website will have multiple web VMs/containers running multiple php workers behind a load balancer and constant bot traffic. That means you would have to try a wide range of number of connections to open. On top of that you likely won’t know the exact build version, so you would have to try exploits for every combination out there.

Let’s say you try this for the 50 most popular php/OS combinations, and any number of workers between 10 and 1010. That’s 50k attempts. But against constant background traffic, you would need to try each combination a number of times. Each of these attempts involves opening a whole lot of connections, so they each take some time. The PoC runs against localhost and takes 3-4 seconds, so let’s optimistically call it 10 seconds on the internet.

You can only do one attempt at a time, so that means multiple days of bruteforcing in a way that is essentially a DoS, since a large part of that range will exhaust the maximum number of connections. At this point the person running the site will likely notice, start blocking IP addresses or adding a WAF.

Most high-traffic websites already run WAFs/rate-limiting to deal with bot traffic, so you would need to run this attack from a great many IPs, and preferably clean ones that are harder to come by. But running this from many IPs also increases the chances that your timing will be off, reducing the number of plausible attempts even further.

That’s assuming the WAF doesn’t block your attempts entirely, because the easiest ways to exploit these vulnerabilities were known at the time of the announcement.

I’m sure people and agents will come up with clever techniques to reduce the required number of attempts eventually. But that has nothing to do with “this will get mass-exploited within 15 minutes.”

Against very low-traffic websites that do not have any bot protection, leak information about their version, running close to stock configuration, you might get lucky.

So sure, it’s a critical vulnerability, deserves attention, should be patched ASAP. Maybe even put in the announcement that if you’re running low-traffic sites, be extra sure to patch fast.

But to the AI security vendors who promoted this as THE GATES OF HELL WILL OPEN YOU HAVE TO PATCH WITHIN 15 MINUTES OR DIE? I want my Wednesday back.

Oh and the claims of it being exploited in the wild? At time of writing, they’re referring to exploitation *attempts*. In fact, HoneyLabs [so far](https://infosec.exchange/@HoneyLabs/117483559117381472) hasn’t seen a single exploit attempt that could even run successfully.
