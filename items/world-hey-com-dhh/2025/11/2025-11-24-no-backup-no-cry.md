---
title: No backup, no cry
link: https://world.hey.com/dhh/no-backup-no-cry-274e0c31
source: world-hey-com-dhh
published: 2025-11-24T10:40:13Z
updated: 2025-11-24T10:40:13Z
first_seen: 2026-09-19T21:30:23.395188495Z
authors:
- David Heinemeier Hansson
content: extracted
html: 2025-11-24-no-backup-no-cry.html
preview:
  file: 2025-11-24-no-backup-no-cry.preview-369f956b88ea.webp
  width: 256
  height: 256
  color: '#8f766d'
images:
- source: https://world.hey.com/dhh/avatar-e58d6b2d72626dd1d1901253507aa2ea75136c6c
  original:
    file: 2025-11-24-no-backup-no-cry.image-79775210ce53.jpg
    width: 300
    height: 300
  color: '#b9d8c7'
---

I haven't done a full-system backup since back in the olden days before [Dropbox](https://www.dropbox.com/) and Git. Every machine I now own is treated as a stateless, disposable unit that can be stolen, lost, or corrupted without consequences. The combination of full-disk encryption and distributed copies of all important data means there's just no stress if anything bad happens to the computer.

But don't mistake this for just a "everything is in the cloud" argument. Yes, I use Dropbox and GitHub to hold all the data that I care about, but the beauty of these systems is that they work with local copies of that data, so with a couple of computers here and there, I always have a recent version of everything, in case either syncing service should go offline (or away!).

The trick to making this regime work is to stick with it. This is especially true for Dropbox. It's where everything of importance needs to go: documents, images, whatever. And it's instantly distributed on all the machines I run. Everything outside of Dropbox is essentially treated as a temporary directory that's fully disposable.

It's from this principle that I built [Omarchy](https://omarchy.org/) too. Given that I already had a way to restore all data and code onto a new machine in no time at all, it seemed so unreasonable that the configuration needed for a fully functional system still took hours on end. Now it's all encoded in an ISO setup that installs in two minutes on a fast computer.

Now it's true that this method relies on both multiple computers and a fast internet connection. If you're stuck on a rock in the middle of nowhere, and you somehow haven't discovered the glory of Starlink, maybe just stick to your old full-disk backup ways. But if you live in the modern world, there ought to be no reason why a busted computer is a calamity of data loss or a long restore process.
