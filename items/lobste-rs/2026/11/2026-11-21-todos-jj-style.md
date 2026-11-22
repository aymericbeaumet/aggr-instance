---
title: TODOs JJ Style
link: https://bwc9876.dev/blog/jj_todo/
source: lobste-rs
published: 2026-11-21T15:17:18Z
updated: 2026-11-21T15:17:18Z
first_seen: 2026-11-22T14:51:24.965070109Z
authors:
- bwc9876.dev via Raicuparta
labels:
- vcs
summary: Comments
content: extracted
html: 2026-11-21-todos-jj-style.html
remote_preview:
  url: https://bwc9876.dev/_astro/cow.Ba3ajqtI_Z5zeAh.webp
  alt: Ben C's Profile Picture
---

```
 _______________________
< todo: fill out cowsay >
 -----------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||
```

I love using [JJ](https://jj-vcs.dev) and recently I’ve found a cool way to handle TODOs that works for me.

## In-Tree TODO is Kinda Annoying

Usually I’ll put them in a `TODO.md` file which is pretty standard. But, I forget about this file quickly and am not very good at remembering to cross stuff off. Also since it’s in-tree I’ll often not want to update it, and then forget to do so later.

## Empty Commits to the Rescue

JJ has a cool feature where you can make empty changes, and even write descriptions for them before actually doing work. So what if I just make all the stuff I wanna do later as empty commits?

![A JJ tree showing a branch of empty commits, the root is called “private: TODO base” and has many empty children with descriptions](https://bwc9876.dev/_astro/jj-todo-tree.CbZTp8jv_2gvqxu.webp)

Pretty nice! When I want to start working on something I’ll rebase it out of my TODO tree and onto some other change.

I need to rebase my “TODO base” forward when i make changes, but because it’s empty it will never have conflicts and can pretty easily move around.

TODOs that depend on other TODOs also naturally fit here, I just make new empty commits on top of other empty commits.

## Keeping Them Private

I have `git.private-commits` configured to make sure nothing with the `private:` prefix will get pushed. This ensures my TODOs stay on my local system. Of course, there are benefits to not doing this.

```nix
# In Home Manager...

programs.jujutsu.settings.git.private-commits = "description('private:*')";
```

Anyway that’s about it, not very ground breaking but something I like.
