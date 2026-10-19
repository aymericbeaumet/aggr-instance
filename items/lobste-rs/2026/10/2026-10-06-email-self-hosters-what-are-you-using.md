---
title: Email Self Hosters - what are you using?
link: https://lobste.rs/s/rwloew/email_self_hosters_what_are_you_using
source: lobste-rs
published: 2026-10-06T08:09:53Z
updated: 2026-10-06T08:09:53Z
first_seen: 2026-10-06T19:48:54.930461426Z
authors:
- by vetch
labels:
- ask
- email
summary: At the moment I self host using maddy for a bunch of domains that need a mailbox or catch all. There are some minor issues with it - some are me, some are who knows. The biggest bug bear is that the native email client on iOS takes an age to connect and download messages which causes some issues with OTP emails. So for those out there that do host their own email - what are you running, what would you change?
content: extracted
html: 2026-10-06-email-self-hosters-what-are-you-using.html
---

I'm using Stalwart. For clients I'm using [sterna](https://codeberg.org/emon/sterna-mail) on mobile and [aerc](https://aerc-mail.org/) on the desktop. I'm using JMAP instead of IMAP. One thing I really like about Stalwart is that is using JMAP to update its configuration. It is currently a little rough, but I do think it will much better in the end. It allows one to have a declarative configuration of resources like Folders, tags, aliases, sieve scripts, etc.

The installation recommends you to use `curl | sh -` (no, thanks), but the script is well written so it can be used to learn how to setup Stalwart. Given how much care went into the installation script, I really do wonder why they don't also write down a document. Their docs as another comment mentions are verbose and not very useful. I've yet to figure out how to tell its spam filter about a false positive.

Stalwart does its best to work out of the but you still need to troubleshoot things (e.j. use [https://www.learndmarc.com/](https://www.learndmarc.com/) to check your configuration). There are things like reverse DNS that Stalwart can't help you setup.

Updating Stalwart so far has been a breeze. Just download the new executable, unzip and restart the service. There was a big change from 0.15 to 0.16 but so far no manual upgrade process has been needed.

Besides their docs, their support portal is mostly an LLM answering questions, which as one might guess is not very useful.

Some small QoL changes I'd like to see:

- A toggle to remove all the upsell entries in the menu
- Their stallwart-cli to integrate with the system's keychain (SecretService in Linux)
- A preview of how would a sieve script would have classifiy the existing emails in the inbox.

So far my experience with Stalwart has positive. Its a good way to start self-hosting email, but I do worry about how much they are pushing LLMs into their main product. I may end up moving to another server later.

One thing I'd like to improve later is having a jmap based syncing tool. Similar to [https://github.com/elizagamedev/mujmap](https://github.com/elizagamedev/mujmap). (While reading the mumap code and dependencies I was surprised to learn that notmuch is not thread safe).
