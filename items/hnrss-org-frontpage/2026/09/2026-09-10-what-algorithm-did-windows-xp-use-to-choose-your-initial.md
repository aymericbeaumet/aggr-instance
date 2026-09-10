---
title: What algorithm did Windows XP use to choose your initial user picture?
link: https://devblogs.microsoft.com/oldnewthing/20260909-00/?p=112683
source: hnrss-org-frontpage
published: 2026-09-10T09:04:14Z
updated: 2026-09-10T09:04:14Z
first_seen: 2026-09-10T13:32:54.375365065Z
authors:
- soheilpro
summary: 'Article URL: https://devblogs.microsoft.com/oldnewthing/20260909-00/?p=112683 Comments URL: https://news.ycombinator.com/item?id=49640646 Points: 172 # Comments: 79'
content: extracted
html: 2026-09-10-what-algorithm-did-windows-xp-use-to-choose-your-initial.html
preview:
  file: 2026-09-10-what-algorithm-did-windows-xp-use-to-choose-your-initial.preview-f2ac05282b2e.webp
  width: 110
  height: 145
  color: '#5e5755'
images:
- source: https://devblogs.microsoft.com/oldnewthing/wp-content/uploads/sites/38/2019/02/ShowCover.jpg
  original:
    file: 2026-09-10-what-algorithm-did-windows-xp-use-to-choose-your-initial.image-566f29379af9.jpg
    width: 110
    height: 145
  variants:
  - file: 2026-09-10-what-algorithm-did-windows-xp-use-to-choose-your-initial.image-b175d96d6271.webp
    width: 48
    height: 63
  color: '#030202'
- source: https://devblogs.microsoft.com/oldnewthing/wp-content/uploads/sites/38/2019/02/RaymondChen_5in-150x150.jpg
  original:
    file: 2026-09-10-what-algorithm-did-windows-xp-use-to-choose-your-initial.image-19dbb31aeed3.jpg
    width: 150
    height: 150
  variants:
  - file: 2026-09-10-what-algorithm-did-windows-xp-use-to-choose-your-initial.image-c4c4aff94af1.webp
    width: 48
    height: 48
  color: '#66798b'
---

I noted some time ago that [Windows XP chose your initial picture at random](https://devblogs.microsoft.com/oldnewthing/20040401-00/?p=39933 "The martial arts logon picture") from among the pictures in the %ALLUSERSPROFILE%\\Application Data\\Microsoft\\User Account Pictures\\Default Pictures directory. But it seems people want to know more.

> Has anyone attempted to figure out the RNG for how Windows XP determines what profile picture is used on first account creation?
>
> — Xeno (@XenoPanther) [December 11, 2025](https://twitter.com/XenoPanther/status/1999217479923413284)

The random number generator is our friend `RtlRandomEx`, using the current value of `GetTickCount()` as the initial seed.

The function uses a one-pass random selection algorithm. I can immediately think of two benefits of this decision. First, compared to the naïve two-pass algorithm of counting up all the items, then randomly picking a number from 1 to n, and then iterating a second time to find the item at that index, it’s more efficient because it reduces the amount of calls into the file system, which is where the bottleneck is. Furthermore, the one-pass algorithm avoids complications if the number of files in the directory changes while the code is running.

The one-pass algorithm is a special case of [reservoir sampling](https://en.wikipedia.org/wiki/Reservoir_sampling), where k is 1. This special case permits a tailored algorithm that is much simpler.

```
selectRandomFromIterator(iterator)
{
    var count = 0;
    var winner = null;

    while (iterator.moveNext()) {
        ++count;
        if (uniform_random(min: 1, max: count) == count) {
            winner = iterator.current();
        }
    }

    return winner;
}
```

The way this algorithm works is by observing that in a collection of n items, the last item has a 1/n chance of being randomly selected. If it isn’t selected, then you need to select randomly from the first n − 1 items, which you can solve recursively.

Playing the recursion forward, you start with the base case which is that if you have a list of 1 item, then your only choice is to chose that item. Otherwise, if you have a list of n items, first choose an item randomly from the first n − 1, and then switch to the nth item with a 1/n probability.

As a final safety check, the code stops after sampling 100 pictures. This avoids pathological behavior if somebody puts a million files in the Default Pictures directory.

## Author

![Raymond Chen](https://devblogs.microsoft.com/oldnewthing/wp-content/uploads/sites/38/2019/02/RaymondChen_5in-150x150.jpg)

Raymond has been involved in the evolution of Windows for more than 30 years. In 2003, he began a Web site known as The Old New Thing which has grown in popularity far beyond his wildest imagination, a development which still gives him the heebie-jeebies. The Web site spawned a book, coincidentally also titled The Old New Thing (Addison Wesley 2007). He occasionally appears on the Windows Dev Docs Twitter account to tell stories which convey no useful information.
