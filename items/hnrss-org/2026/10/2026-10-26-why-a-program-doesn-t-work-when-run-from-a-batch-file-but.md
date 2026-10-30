---
title: Why a program doesn't work when run from a batch file, but works when clicked
link: https://devblogs.microsoft.com/oldnewthing/20261026-00/?p=112797/
source: hnrss-org
published: 2026-10-26T21:14:43Z
updated: 2026-10-26T21:14:43Z
first_seen: 2026-10-30T18:48:04.665156877Z
authors:
- ibobev
content: extracted
html: 2026-10-26-why-a-program-doesn-t-work-when-run-from-a-batch-file-but.html
remote_preview:
  url: https://devblogs.microsoft.com/oldnewthing/wp-content/uploads/sites/38/2019/02/ShowCover.jpg
---

![like](https://devblogs.microsoft.com/oldnewthing/wp-content/themes/devblogs-evo/images/emojis/like.svg)![mind blown](https://devblogs.microsoft.com/oldnewthing/wp-content/themes/devblogs-evo/images/emojis/mind-blown.svg) 5 reactions

![](https://devblogs.microsoft.com/oldnewthing/wp-content/uploads/sites/38/2019/02/RaymondChen_5in-150x150.jpg)

A customer asked for help writing a batch file to launch a program. For some reason, the program displayed an error message when the batch file ran it, whereas it worked fine when they double-clicked the program from Explorer.

I immediately suspected that they were not setting the current directory.

Some time ago, I noted that when you double-click a file in Explorer, [Explorer sets the current directory of the program to the directory containing the thing that you double-clicked](https://devblogs.microsoft.com/oldnewthing/20101110-00/?p=12313 "Your debugging code can be a security vulnerability: Loading optional debugging DLLs without a full path"). This is done to accomodate programs that look for ancillary files in the current directory. The example I had was a hypothetical LitWare Writer program that, when given an `ABC.LIT` file, looks for related files like `ABC.LTC` (the table of contents) and `ABC.LDC` (the custom dictionary) in the current directory.

This also happens if you double-click the `.exe` file: The current directory is set to the directory that contains the executable.

My guess is that the customer’s program looks for ancillary files relative to its current directory, but the batch file ran the program by its full path without changing the current directory to match the program’s path.

The customer confirmed that explicitly changing the directory fixed the program. (More specifically, they used `pushd` to change the directory and then `popd` to restore the original directory.)

**Bonus chatter**: It didn’t come to it, but my second idea would have been to check whether the program had [set a custom entry under `App Paths`](https://devblogs.microsoft.com/oldnewthing/20110725-00/?p=10073 "How is it possible to run Wordpad by just typing its name even though it isn't on the PATH?") to ask for additional directories to be added to the `PATH` environment variable when the program is run.

## Author

![Raymond Chen](https://devblogs.microsoft.com/oldnewthing/wp-content/uploads/sites/38/2019/02/RaymondChen_5in-150x150.jpg)

Raymond has been involved in the evolution of Windows for more than 30 years. In 2003, he began a Web site known as The Old New Thing which has grown in popularity far beyond his wildest imagination, a development which still gives him the heebie-jeebies. The Web site spawned a book, coincidentally also titled The Old New Thing (Addison Wesley 2007). He occasionally appears on the Windows Dev Docs Twitter account to tell stories which convey no useful information.

## Stay informed

Get notified when new posts are published.

Follow this blog

-
- [![youtube](https://devblogs.microsoft.com/oldnewthing/wp-content/themes/devblogs-evo/images/social-icons/youtube.svg)](https://www.youtube.com/playlist?list=PLlrxD0HtieHge3_8Dm48C0Ns61I6bHThc "youtube")
-
-
