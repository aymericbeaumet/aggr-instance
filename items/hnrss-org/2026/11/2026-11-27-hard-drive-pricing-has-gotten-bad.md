---
title: Hard Drive pricing has gotten bad
link: https://blog.ktz.me/black-friday-hard-drive-prices-are-messed-up/
source: hnrss-org
published: 2026-11-27T01:13:49Z
updated: 2026-11-27T01:13:49Z
first_seen: 2026-11-27T04:05:56.741343452Z
authors:
- zdw
labels:
- hardware
- technical
content: extracted
html: 2026-11-27-hard-drive-pricing-has-gotten-bad.html
remote_preview:
  url: https://pub-430e0c9ecb66464fa6999cd23a749d13.r2.dev/content/images/2026/11/black-friday-hard-drive-prices-are-messed-up/row-of-easystore-drives-on-a-cluttered-workbench.webp
---

For years now, I’ve made it an annual tradition to replace one or two hard drives on Black Friday. I typically run around ten physical disks, and swapping a couple out each year means I almost never have a drive older than five years in my systems. I’ve even previously made videos about [shucking disks around this time of year](https://www.youtube.com/watch?v=c2mvIbY9n28).

But current hardware pricing is totally screwed. So what is a lifelong self-hoster to do in times like these? With disk capacities increasing massively over the last 10 years I can now fit 20TB per slot. This means I could realistically go from ten spinners to five, and not really feel it too acutely. This will mean redoing my zpool layouts, which in turn means shuffling some data around as ZFS doesn’t like changes to the layout of existing pools. But, at these prices, I’m starting to think the hassle might be worth it.

Let’s take a look at disk prices over the past 8 years in today’s post.

The 20TB WD EasyStore I bought for **$249.99** in November 2024 is, today, on sale, **$549.99** this Black Friday. $300 more than 2 years ago for the same product, from the same store. It shows just how bad hard drive pricing has gotten ([BestBuy 20TB listing](https://www.bestbuy.com/product/wd-easystore-20tb-external-usb-3-0-hard-drive-black/6500985)).

Price per TB before tax.

Shucking is homelab speak for taking a perfectly good external drive out of its USB enclosure and sticking the SATA drive in your server. At $12.50/TB it was a fantastically affordable way to get massive amounts of storage. At $27.50/TB, absolutely not.

## I dug through my receipts

In November 2018 I bought five 10TB EasyStores for $179.99 each. At the time I wrote about doing the rounds of my local Best Buys to pickup more drives on Black Friday as quantities were limited at each location ([My 2018 server build log](https://blog.ktz.me/new-server-build-log/)).

| Order date   | Drives   | Each    | $/TB       |
| ------------ | -------- | ------- | ---------- |
| Nov 8, 2018  | 5 × 10TB | $179.99 | $18.00     |
| Oct 13, 2020 | 2 × 12TB | $174.99 | $14.58     |
| Nov 1, 2021  | 2 × 14TB | $199.99 | $14.29     |
| May 11, 2022 | 1 × 14TB | $249.99 | $17.86     |
| Aug 29, 2022 | 1 × 14TB | $239.99 | $17.14     |
| Oct 28, 2023 | 3 × 18TB | $199.99 | **$11.11** |
| Nov 21, 2024 | 1 × 20TB | $249.99 | $12.50     |
| May 5, 2025  | 1 × 20TB | $279.99 | $14.00     |

October 2023 was my best haul on a per-TB basis. Three 18TB drives put 54TB in my hands for $599.97. This year’s “deal” is fifty bucks shy of that *entire order*. Best Buy was still advertising those 18TB drives for $199.99 in November 2023. [November 2023 deal archive](https://slickdeals.net/f/17069239-18tb-wd-easystore-usb-3-0-external-hard-drive-200-free-store-pickup)

WD EasyStore 20TB, SKU 6500985. Prices are before tax.

## What about ServerPartDeals?

Buying refurbished or recertified enterprise drives used to be an easy way to bring the cost down. In August 2024 I bought four recertified 14TB Ultrastars from ServerPartDeals for **$529.96 before tax**. That was 56TB at **$9.46/TB**, even cheaper than the EasyStores and no shucking to do.

US dollars per TB. Hatched bars mark equivalent sold-out listings as of Nov 2026.

| Drive                                                         | 2024 price per drive                 | Nov 26, 2026 listing                                                                                                                                                                                                                                        | Change per TB |
| ------------------------------------------------------------- | ------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------- |
| WD Ultrastar HC530 14TB 4Kn, recertified (`WUH721414ALN604`)  | $129.99 · $9.29/TB                   | $204.99 · $14.64/TB · [Sold out](https://serverpartdeals.com/products/western-digital-ultrastar-dc-hc530-wuh721414aln604-0f31102-14tb-7-2k-rpm-sata-6gb-s-4kn-3-5-recertified-hard-drive)                                                                  | +58%          |
| WD Ultrastar HC530 14TB 512e, recertified (`WUH721414ALE6L4`) | $134.99 · $9.64/TB                   | $429.00 · $30.64/TB · [Sold out](https://serverpartdeals.com/collections/western-digital-recertified-all-caps/products/western-digital-ultrastar-dc-hc530-wuh721414ale6l4-0f31284-14tb-7-2k-rpm-sata-6gb-s-512e-512mb-3-5-se-manufacturer-recertified-hdd) | +218%         |
| Seagate Exos X20 20TB, refurbished (`ST20000NM007D`)          | $235.94 · $11.80/TB                  | $589.00 · $29.45/TB · [In stock](https://serverpartdeals.com/collections/sata/products/seagate-exos-x20-st20000nm007d-20tb-7-2k-rpm-sata-6gb-s-3-5-refurbished-hdd)                                                                                        | +150%         |
| Seagate Exos X22 20TB, recertified (`ST20000NM004E`)          | $224.99 · $11.25/TB · later refunded | $649.00 · $32.45/TB · [Sold out](https://serverpartdeals.com/collections/20tb-to-30tb-hard-drives)                                                                                                                                                         | +188%         |

The sad truth is that across the board prices are up a minimum of 58%, and sometimes as much as 218% in just a year or two.

## How we got down to three HDD makers

When the EasyStore is that bad, my first instinct is to shop around. Maybe I should try another retailer (to be clear I’m not blaming BestBuy for any of this), buy an enterprise drive, see what’s available recertified. But follow those listings back to the companies that actually make the drives and the choices narrow pretty quickly. There are three big HDD makers left. How did we end up with so few?

Maxtor bought Quantum’s hard drive business in 2001, then Seagate bought Maxtor in 2006. Hitachi combined IBM’s disk business with its own to form HGST in 2003, and Toshiba bought Fujitsu’s HDD business in 2009. Seagate bought Samsung’s HDD business in December 2011, and Western Digital bought HGST in March 2012. The FTC required WD to sell desktop-drive manufacturing assets to Toshiba to keep a third supplier in that market. That left Seagate, Western Digital and Toshiba.

Selected HDD acquisitions by closing year. The dashed arrow shows the desktop-drive assets transferred to Toshiba as a condition of the WD–HGST deal.

The 2011 Thailand floods hit while those last two deals were underway. WD halted production at all its Thai plants, and its average hard drive selling price went from $45 in fiscal 2011 to $62 in fiscal 2012. There was also a price-fixing case further up the supply chain. NHK Spring pleaded guilty in 2019 to fixing prices for the suspension assemblies that hold the read/write heads, and the court approved a $28.5 million fine. [WD’s 2012 annual report](https://www.sec.gov/Archives/edgar/data/106040/000119312512361018/d356205d10k.htm) · [Ninth Circuit opinion](https://cdn.ca9.uscourts.gov/datastore/opinions/2026/01/08/24-4470.pdf)

## Market collusion and cartel pricing

It’s hard to have anything positive to say in this post. The HDD manufacturing market is so consolidated nowadays - just like the memory market which is equally screwed - that it’s hard to see this as anything but mass market collusion by the HDD manufacturing cartel.

Three companies is an awfully short list when you need to buy a hard drive. And with 89% of WD’s revenue coming from its cloud business in fiscal 2026, I don’t reckon those of us buying a couple of drives for a home server have much impact here. Just like with Nvidia and Micron, consumer markets just aren’t valuable enough so we have to make do with the crumbs instead.

As of November 26, 2026, the 20TB is **2.2×** what I paid for the exact same SKU in 2024. It’s also $270 more than I paid for it in May 2025. And if you’re thinking you’ll just buy a smaller one, Best Buy’s 8TB Black Friday special is $299.99. That’s $37.50/TB and fifty bucks more than my 20TB cost two years ago. [Best Buy’s EasyStore listings](https://www.bestbuy.com/site/searchpage.jsp?id=pcat17071&st=easystore)

The jump gets sillier when you price a whole NAS. Four of those 20TB drives would have been $999.96 at my 2024 price. This week they’d be $2,199.96. That’s another $1,200 before you’ve bought a case, a board or a single stick of RAM. And the price per *usable* terabyte only gets worse once you set aside a drive for parity.

If I can wish for one thing in 2027, it would be for hardware pricing to return to reasonable levels. I fear it may take a lot longer than that, though. So making do with less is going to be the new normal for a while. And in that creative constraint, maybe we’ll have some fun along the way instead.
