---
title: 'Show HN: I wrote an entire Vim manual over 4 years'
link: http://themouseless.dev/vim/
source: hnrss-org
published: 2026-11-26T14:44:22Z
updated: 2026-11-26T14:44:22Z
first_seen: 2026-11-27T13:45:44.456295755Z
authors:
- thanato0s
summary: 'I''m glad to announce that I''m releasing my new book, Learning to Play Vim. When I started using Vim more than a decade ago, I wished there was a manual out there teaching how to use this fantastic editor step by step, beginning with the foundations up to more advanced features. Of course at the time I did the vimtutor, but it was too dense for me to remember much from it. I also read the entire Vim manual (`:help manual`), but I didn''t like it: it was too detailed in places (for example `02.8 Finding help` completely lost me for a while), and other features didn''t get the treatment they deserved. So I decided to write my own Vim manual. It took me more than 4 years and 2987 focused pomodoros (1244 hours) of work to finally get it done! The result: - 21 chapters showing the most important Vim features step by step. - 170 exercises with solutions. - Many real-life examples to show how Vim can be used and configured for your own needs. - The related Vim''s help pages to dig further into each feature. Most configs are both written in Vimscript and Lua for Neovim users. This book explores the common features between Vim and Neovim, so don''t expect anything about Neovim-specific features (like treesitter or LSP for example). - The landing page: https://themouseless.dev/vim/ - A sample of the book with the entire table of contents and the first three chapters covering most of the basics: https://themouseless.dev/content/learning_to_play_vim_sample.... I wrote this with my bare hands, no LLM involved to spoil the fun. Comments URL: https://news.ycombinator.com/item?id=50655868 Points: 230 # Comments: 72'
content: extracted
html: 2026-11-26-show-hn-i-wrote-an-entire-vim-manual-over-4-years.html
remote_preview:
  url: https://themouseless.dev/images/book_cover_mouseless_2_300.jpg
---

Have You Ever Played \
With Your Editor?
---

Compose your own Vim symphony

Vim was too complicated. It wasn't worth the effort to learn it. Or so I thought.

I tried anyway.

I could write and edit my content after a couple of hours. Why everybody thought it was so hard?

Even better: I was more productive, I could focus solely on what I was writing, and I had so much fun!

The fun never stopped. Even after playing with Vim for almost a decade.

## What Will This Book Bring You?

A step-by-step approach to **learn easily without feeling overwhelmed.**

Practice makes perfect: **no less than 170 fun exercises** to build your muscle memory.

How to customize your editor for **your own specific needs.**

Keeping your hands on your keyboard will **save your cognitive energy**, and **improve your productivity.**

The print version will be available in a couple of weeks.\
You can subscribe to the newsletter to get notified.

![photo of the author of the book Matthieu Cneude](https://themouseless.dev/vim/images/index/matthieu.jpg)

Hi! I'm Matthieu. Nice to see you here!

I've been coding as a hobby for 25 years, professionally for 15+ years.

I've worked from small to big companies, mentoring and leading developer teams.

Teaching is very rewarding to me. That's why I love writing for my blog, [The Valuable Dev.](https://thevaluable.dev/)

* * *

## FAQ

### There are many resources about Vim out there: what's different with this one?

- Each chapter builds on the previous one, to show you more advanced techniques as you progress.
- Practice makes perfect: each chapter comes with exercises to increase your skills, but also to dive deeper and uncover useful subtleties.
- The book tries to focus on real-life, practical examples, inspired from my own (and friend's) use of Vim.
- The book is aimed to be as complete as possible, without going into useless, boring details.
- Even if you use Vim for a long time, I'm pretty sure you'll learn something in this book. If not, ask for a refund!
- There's a rich annexe section, to serve as a reference when you're working with Vim.
- The book is designed to be fun to read (we're here to play!) with a minimum of fluff. I don't want to tell the story of my grandma for 392 pages.

### What this book is NOT good for?

This book is not for you if:

- You're not ready to invest some time on the short term to be more effective on the long term.
- You want absolutely all details about everything. Vim's internal help will do a better job than this book (or any other).
- You want to learn the basics, but you're not interested to go further. In that case, most free tutorials out there will be enough.
- You want to reproduce all the functionalities of your IDE without compromises. Vim is powerful (and can do much more than most people think), but it's not your IDE.
- You're more interested in plugins than Vim's internal functionalities. Learning to Play Vim is more about the latter.

### Why should I use Vim?

- Vim is a free, mature, and stable editor. It has many useful functionalities you won't find anywhere else.
- Vim is lighter and faster than most IDEs. It's not a problem to have multiple instances running in parallel, even on old computers.
- You can customize Vim according to your own needs, to focus on your code (or your writing).
- Vim is fun to play with! To me, it's like a game. The goal: to use as less keystrokes as possible.
- You can configure IDE-like features (autocompletion, jump to definition, debugging...) for many programming languages. \
  No need to have multiple editors or IDEs anymore!
- Vim allows you to write and edit using only the keyboard, improving your efficiency.
- Switching between the keyboard and the mouse costs cognitive energy, and breaks your flow. No need to use the mouse with Vim.
- There are countless plugins for Vim out there, enabling a more modern experience, similar to an IDE.
- You can use any shell command directly in Vim, expanding drastically its functionalities.
- Many command-line tools use some Vim keystrokes. It makes them easier to learn.
- Popular shells (like Bash or Zsh) have a "Vi" mode, allowing you to use some Vim keystrokes to edit your commands.
- Vim is sometimes the only option for editing files on remote server or docker containers.

### Is it true that Vim has a steep learning curve?

Despite its reputation, Vim is not hard to learn. It works differently than most editor or IDE out there, that's why many people find it difficult. If you accept that it requires some adaptation, you can learn the basics in a couple of hours.

Vim is a good example of a tool easy to learn but hard to master: you'll add new functionalities to your workflow overtime, improving your editing experience and your efficiency.

That's why this book takes a step-by-step approach.

### Is this book about Vim, Neovim, or GVim?

This book is exclusively about Vim and Neovim running in a \*nix shell (Unix-like shell like Zsh or Bash). This book is not about Vim running in a GUI, like GVim for example.

### Do you use LLMs ("AI") to write this book?

No.
