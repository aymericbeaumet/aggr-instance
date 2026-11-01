---
title: The support case of a customer who said that all they could see was a black screen
link: https://devblogs.microsoft.com/oldnewthing/20261029-00/?p=112808/
source: lobste-rs
published: 2026-10-31T01:01:21Z
updated: 2026-10-31T01:01:21Z
first_seen: 2026-11-01T16:16:54.446869743Z
authors:
- devblogs.microsoft.com via ubernostrum
labels:
- historical
- windows
summary: Comments
content: extracted
html: 2026-10-31-the-support-case-of-a-customer-who-said-that-all-they-could.html
remote_preview:
  url: https://devblogs.microsoft.com/oldnewthing/wp-content/uploads/sites/38/2019/02/ShowCover.jpg
---

If you hold a PC’s power button for 12 seconds, it will turn off at the hardware level without notifying the operating system. This is a safety feature so that you can turn of the computer even if the operating system’s “Shut down” command isn’t working.

After eight seconds of the power button being held down, the Windows kernel realizes that the user is trying to do a force-power-off of the system, and it quickly takes a kernel dump that can be uploaded to the Windows Error Reporting service the next time you turn the system on. (Sometimes four seconds isn’t enough time to get enough information, and the kernel dump is incomplete. I believe that on Windows Insider builds of Windows, you get a message that says something like “Please let go of the power button so we can collect more information, and then we will turn off the computer.” This gives the kernel more time to finish capturing its kernel dump.)

These emergency kernel dumps are internally classified as “long power button hold” (LPBH) failures. One of the pieces of information recorded in these emergency kernel dumps is whether the screen consists entirely of black pixels. Those failures are sub-classified as “black screen LPBH”, and they go to a special virtual team for analysis. (I happen to be a member of this black screen LPBH team.)

Sometimes the problem is a broken video driver. Sometimes the video driver is doing just fine, but the desktop compositor is just handing black pixels to the video driver. Sometimes the desktop compositor is doing just fine, but nobody is drawing anything to the screen. And those cases are further subdivided further into cases like “there is nobody around to draw anything” or “the person who normally draws stuff is hung.”

Back in the Windows 3.1 days, there was a customer product support call that would today have been classified as a “black screen” bug: The customer reported that they turned on the computer, and Windows started up, but when the logo went away, all they saw was a blank screen.

After some questioning, the customer support engineer put the story together: It turned out that this customer really liked the color black, so they decided to set up a [goth](https://en.wikipedia.org/wiki/Gothic_fashion) theme. They set the wallpaper color to black. They set the title bar color to black. They set the button color to black. They set the window background color to black. They set the window text color to black. Every possible color you could pick in the Colors control panel was set to black.

Once you get yourself into this situation, it’s hard to get out because you can’t see anything.

I don’t know how the customer support engineer fixed the problem, but I’m guessing that they booted into MS-DOS, ran `EDIT.COM`, and [deleted the `[colors]` section from the `win.ini` file](https://jeffpar.github.io/kbarchive/kb/083/Q83433/), thereby returning all the system colors to defaults.

## Author

![Raymond Chen](https://devblogs.microsoft.com/oldnewthing/wp-content/uploads/sites/38/2019/02/RaymondChen_5in-150x150.jpg)

Raymond has been involved in the evolution of Windows for more than 30 years. In 2003, he began a Web site known as The Old New Thing which has grown in popularity far beyond his wildest imagination, a development which still gives him the heebie-jeebies. The Web site spawned a book, coincidentally also titled The Old New Thing (Addison Wesley 2007). He occasionally appears on the Windows Dev Docs Twitter account to tell stories which convey no useful information.
