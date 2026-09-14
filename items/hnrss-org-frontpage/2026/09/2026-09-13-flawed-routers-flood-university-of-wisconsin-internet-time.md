---
title: Flawed routers flood University of Wisconsin internet time server (2003)
link: https://pages.cs.wisc.edu/~plonka/netgear-sntp/
source: hnrss-org-frontpage
published: 2026-09-13T20:33:18Z
updated: 2026-09-13T20:33:18Z
first_seen: 2026-09-14T13:54:04.376507838Z
authors:
- walrus01
summary: 'Article URL: https://pages.cs.wisc.edu/~plonka/netgear-sntp/ Comments URL: https://news.ycombinator.com/item?id=49688391 Points: 100 # Comments: 12'
content: extracted
html: 2026-09-13-flawed-routers-flood-university-of-wisconsin-internet-time.html
preview:
  file: 2026-09-13-flawed-routers-flood-university-of-wisconsin-internet-time.preview-6d8bc3768fb5.webp
  width: 256
  height: 104
  alt: graph showing increased SNTP traffic
  color: '#bbc0de'
images:
- source: https://pages.cs.wisc.edu/~plonka/netgear-sntp/SNTPflood.png
  original:
    file: 2026-09-13-flawed-routers-flood-university-of-wisconsin-internet-time.image-3ba9d04784bf.png
    width: 735
    height: 300
  variants:
  - file: 2026-09-13-flawed-routers-flood-university-of-wisconsin-internet-time.image-c6ba79efc700.webp
    width: 48
    height: 20
  color: '#f3f2f2'
- source: https://pages.cs.wisc.edu/~plonka/netgear-sntp/slashdotted.gif
  original:
    file: 2026-09-13-flawed-routers-flood-university-of-wisconsin-internet-time.image-ef2151f5de64.gif
    width: 495
    height: 224
  variants:
  - file: 2026-09-13-flawed-routers-flood-university-of-wisconsin-internet-time.image-e31329902f2a.webp
    width: 48
    height: 22
  - file: 2026-09-13-flawed-routers-flood-university-of-wisconsin-internet-time.image-5827e822df84.webp
    width: 495
    height: 224
  color: '#f4f4f4'
---

***ABSTRACT***

In May 2003, the University of Wisconsin - Madison found that it was the recipient of a continuous large scale flood of inbound Internet traffic destined for one of the campus' public Network Time Protocol (NTP) servers. The flood traffic rate was hundreds-of-thousands of packets-per-second, and hundreds of megabits-per-second.

Subsequently, we have determined the sources of this flooding to be literally hundreds of thousands of real Internet hosts throughout the world. However, rather than having originated as a malicious distributed denial-of-service (DDoS) attack, the root cause is actually a serious flaw in the design of hundreds of thousands of one vendor's low-cost Internet products targeted for residential use. The unexpected behavior of these products presents a significant operational problem for UW-Madison for years to come.

This document includes the initial public disclosure of details of these products' serious design flaw. Furthermore, it discusses our ongoing, multifaceted approach toward the solution which involves the University, the products' manufacturer, the relevant Internet standards (RFCs), and the public Internet service and user communities.

**Table of Contents**

```
Flawed Routers Flood University of Wisconsin Internet Time Server
    Netgear Cooperating with University on a Resolution
    The Initial Flood
            Figure 1. The Initial Flood
        Blocking the Flood
    Background: Simple Network Time Protocol (SNTP)
            Figure 2. A SNTP Request Packet
            Figure 3. A Unicast SNTP Reply Packet
    The Flood Continues
            Figure 4. The Flood Continues: One Month Later
    Investigation
        Contacting Source Networks
            Figure 5. Email Notification to Peer Institution
        Gathering Background Information
        Examining the Netgear Code
    Contacting Netgear
            Figure 6. Email to Netgear Support
            Figure 7. Email from Netgear Support
    The Review Process
    The Flawed SNTP Client
        Impact to Netgear Customers
        Code Upgrades for Affected Netgear Products
            Figure 8. Affected Netgear Products
        Flawed Product Counts
            Figure 8a. Netgear SNTP Clients Per Day
    Suggested Fixes
        The Initial Fix: "Instant" Code
    Network Operational Options: To Serve or To Sever?
        Endgame A: UW-Madison Netgear Anycast Time Service
            Figure 10. A WiscNet BGP-based Anycast Time Service
        Endgame B: Attempt to Suppress the Requests
            Figure 11. Using the Global BGP Routing Table to Squelch Requests
        Endgame B: IP Resources Required
            Figure 12. IP Resources Required for BGP-based Suppression
    Inform the Internet Community
    Clarify Internet Best Current Practice and Protocol Standards
    Status, August 21, 2003
            Figure 13. The Most Recent Flood
    Afterthoughts
    Acknowledgements
    Analysis Tools
    References / Further Reading
    Frequently Asked Questions
        What is Netgear's liability for causing (however inadvertently) this denial of service for your network?
        Have you considered putting up a server which sends back fake answers to netgear clients, to cause people to upgrade?
        What is the expected life-time for these products?
        In figure 13, could that "shark fin" spike have anything to do with last week's power grid failure (Blackout 2003), and subsequent "rolling" restoration?
        Are there other devices than those mentioned which also suffer from the flaw which causes inadvertent flooding of your network?
        What was the effect of this article being slashdotted?
        Why is a traditional manufacturer recall/defect solution not a possibility?
        I'm with [the IT press], do you have some time to speak with me?
        How has this story been covered in the press?
```

* * *

## The Initial Flood

Figure 1 is a graph of inbound traffic to our campus over a 48 hour period, tuesday through thursday, May 13-15, 2003.

The first half of the graph shows typical traffic levels for our campus, with peak inbound packet rates of about 40,000 packets-per-second. However, as you can see, our inbound packet-per-second rate increased dramatically starting May 14 at about 8AM localtime, primarily from our commodity Internet Service Provider, WiscNet. At about 9:40AM this additional traffic began to cause problems with our measurement infrastructure and some of our legacy intra-campus routers. By 11AM we had identified the inbound flood traffic by protocol and port numbers. It was destined for our public time server and we blocked the incoming traffic upstream, at WiscNet's border routers, which alleviated the problem for the time being. This is a typical action for network operators to take in reaction to malicious Denial-of-Service flood attacks, of which we assumed this was one.

* * *

### Blocking the Flood

The traffic in question appeared to be Network Time Protocol (NTP) queries in that they consisted of 76-byte IP packets destined for UDP port number 123 (NTP). However, these packets had an unusual characteristic: although they appeared to come from many sources, they all had the same source port number: 23457. Therefore, it was possible to configure our routers to block just a subset of inbound queries to our NTP server, and continue to service the other legitimate requests normally. We just blocked all UDP traffic sourced from port 23457 and destined for port 123 (NTP) of the NTP server in question. (Note that the number 23457 seems hand-picked, as the number subsequent to 23456.) At this point we simply chalked it up to naivete on the part of the "attacker", which we presumed was forging many random source addresses, and left it at that, presuming that the flood would subside within hours as "script kiddie"-launched flood attacks often do.

* * *

## Background: Simple Network Time Protocol (SNTP)

Paraphrased from RFC2030 by Dave Mills:

> The Simple Network Time Protocol (SNTP) is an adaptation of the Network Time Protocol (NTP) used to synchronize computer clocks in the Internet. It is a simple, stateless remote-procedure call (RPC) system with accuracy and reliability expectations similar to the UDP/TIME protocol described in RFC-868. SNTP can be used when the ultimate performance of the full NTP implementation is not necessary.

Note that SNTP uses the same packet format as NTP. In this way, SNTP clients can utilize NTP servers, even though they do not implement the complexities of the full peer-to-peer NTP protocol.

SNTP conversations typically follow these steps:

1. A client that would like to know the time sends a UDP packet containing the SNTP request to the well-known NTP port number 123 of an NTP server, and awaits a reply.

   #### Figure 2. A SNTP Request Packet

   ```
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |LI | VN  |Mode |    Stratum    |     Poll      |   Precision   |
   | =0|= 1-4|= 3  |      = 0      |     = 0       |      = 0      |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |                          Root Delay                           |
   |                             = 0                               |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |                       Root Dispersion                         |
   |                             = 0                               |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |                     Reference Identifier                      |
   |                             = 0                               |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |                                                               |
   |                 Reference Timestamp (64 bits)                 |
   |                             = 0                               |
   |                                                               |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |                                                               |
   |                 Originate Timestamp (64 bits)                 |
   |                             = 0                               |
   |                                                               |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |                                                               |
   |                  Receive Timestamp (64 bits)                  |
   |                             = 0                               |
   |                                                               |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |                                                               |
   |                  Transmit Timestamp (64 bits)                 |
   |                             = n                               |
   |   (some number: zero, or the time of request sent by client)  |
   |                                                               |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   ```

2. The server responds with a UDP packet containing the SNTP reply from the well-known NTP port number 123 to the SNTP client.

   #### Figure 3. A Unicast SNTP Reply Packet

   ```
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |LI=| VN  |Mode |    Stratum    |     Poll      |   Precision   |
   |0-2|=req.|= 4  |    = 1 - 14   |   (ignore)    |   (ignore)    |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |                          Root Delay                           |
   |                           (ignore)                            |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |                       Root Dispersion                         |
   |                           (ignore)                            |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |                     Reference Identifier                      |
   |                           (ignore)                            |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |                                                               |
   |                 Reference Timestamp (64 bits)                 |
   |                           (ignore)                            |
   |                                                               |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |                                                               |
   |                 Originate Timestamp (64 bits)                 |
   |            (copied from request Transmit Timestamp)           |
   |                                                               |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |                                                               |
   |                  Receive Timestamp (64 bits)                  |
   |             (time request was received by server)             |
   |                                                               |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |                                                               |
   |                  Transmit Timestamp (64 bits)                 |
   |                 (time of reply sent by server)                |
   |                             = n                               |
   |                                                               |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   ```

3. Upon receiving the response, the client optionally uses the Originate Timestamp from the reply to validate the response, attempting to assure that it is indeed a response to this client's request. (If the reply were spoofed from another source, it would be unlikely to contain the correct value as the Originate Timestamp). Then it plucks the value from the "Transmit Timestamp", perhaps modifying it slightly to account for the estimated one-way end-to-end delay, and uses the result as the current time to set its local clock.

*Now, back to our story...*

* * *

## The Flood Continues

One month later, we discovered that the flood of inbound NTP traffic persisted at an even more incredibly high rate, as evidenced by Figure 4 which plots our router's discarded packet rates beginning in early June 2003, for the traffic in question.

In Figure 4, note that: (1) there are slight daily fluctuations in rate (perhaps due to diurnal user behavior), (2) generally, the rate stays at a value over 250,000 packets-per-second (and over 150 megabits-per-second), and (3) that the traffic rate increases throughout the time shown. The sharp drops in traffic rate in this figure are not due to the flood subsiding but rather were due to network maintenance and a temporary block upstream from the observation point.

Once we found that this flood was continuing and was still increasing in rate, we investigated further. By carefully removing the block on some ingress interfaces, we allowed a trickle of traffic through to the server and captured the packets including their payload. We learned that these packets appeared to be legitimate, well-formed Simple Network Time Protocol (SNTP) version 1 queries, albeit at an inexplicably high rate from each client host. For instance, during one trace, many clients produced about one query per second. This would be highly unusual for a properly constructed SNTP client, since an application which uses SNTP is merely interested in setting its own clock relatively accurately so that its host has some reasonable notion of the current time. One query per second is ridiculous, and is far from best practice for NTP client behavior.

Also, we discovered that many of the IP addresses could be resolved to DNS names and furthermore that the IP addresses all appeared to be valid sources for the given ingress interface from which we removed the block. This indicated that it was quite possible that the source addresses were not forged but instead were real Internet hosts running some very unusual SNTP client.

Alas, none of the client source hosts were within our local campus network. This meant we would need to recruit the help of staff at remote sites to aid in the investigation.

* * *

## Investigation

### Contacting Source Networks

Of the top talker source IP addresses from the aforementioned packet trace, I selected two client hosts from other universities with talented network staff who would be familiar with responding to such incidents.

The following is an email I sent to the Incident Response Team at one of those institutions to which one of the client host addresses belonged. (To maintain a modicum of anonymity, I have replaced the real SNTP client's IP address with 10.42.69.10 and have also removed the email domain names.)

#### Figure 5. Email Notification to Peer Institution

```
Date: Sat, 14 Jun 2003 04:34:11 -0500
From: Dave Plonka <plonka@localdomain>
To: abuse@[remotedomain]
Subject: sntp/ntp query flood from 10.42.69.10 to ntp1.cs.wisc.edu


[Organization] network abuse folks,

Since May 14, 2003 ~0800 central time, one of our campus' NTP
servers "ntp1.cs.wisc.edu" (128.105.39.11) has been the recipient of a
large-scale flood of Simple Network Time Protocol (SNTP) requests -
much more than it can service.  This dramatic increase in inbound SNTP
requests inexplicably continues even now.  To mitigate this flood we
are currently blocking over over 250K pkts/sec, exceeding 150
megabits/sec, and it has been continuing for weeks.

We are in the process of trying to determine if this flood is potentially
malicious or if it is an SNTP client misconfiguration or bug.

This traffic primarily consists of 76-byte UDP packets that are SNTP
version 1 queries from very many source host addresses directed to
ntp1.cs.wisc.edu port 123 (NTP).  Unusually, these requests all have a
UDP source port of 23457.  We have identified the host address
10.42.69.10 as just one of the sources.  (However, there are at
least tens of thousands of source host addresses.)

I have attached a timestamped log of a packet capture from the
afternoon of June 13, 2003 (Friday) evidencing the SNTP query packets
from the host 10.42.69.10 at an unusually high rate of about one
per second.  A packet decomposition (by tethereal) and hex dump of the
last packet (frame 998) in the log is included as well, which shows
them to be valid SNTP v1 queries as described in RFC 1361,
http://www.ietf.org/rfc/rfc1361.txt.

Could you assist us ASAP with this investigation by identifying that
host's operating system and what SNTP client code may be running on
that host?  It would be interesting to know if a process on
10.42.69.10 currently has UDP port 23457 bound and what code that
process is running.

Thanks,
Dave

P.S.  Our investigation so far has shown that Windows systems such as
2000 and XP have an "Internet Time" feature which is usually configured
to send SNTP requests to the Microsoft server "time.windows.com", but
this server can be changed.  I have yet to identify any SNTP client
that regularly uses UDP port 23457 as its source port.  (Note that
port number seems hand-picked, as the number subsequent to 23456.)

----------------------------------------------------------------------

  1 2003-06-13 16:32:24.8808 10.42.69.10 -> 128.105.39.11 NTP NTP
  7 2003-06-13 16:32:25.9611 10.42.69.10 -> 128.105.39.11 NTP NTP
 14 2003-06-13 16:32:27.0412 10.42.69.10 -> 128.105.39.11 NTP NTP
 21 2003-06-13 16:32:28.1215 10.42.69.10 -> 128.105.39.11 NTP NTP
 27 2003-06-13 16:32:29.2020 10.42.69.10 -> 128.105.39.11 NTP NTP
 33 2003-06-13 16:32:30.2821 10.42.69.10 -> 128.105.39.11 NTP NTP
 39 2003-06-13 16:32:31.3624 10.42.69.10 -> 128.105.39.11 NTP NTP
 45 2003-06-13 16:32:32.4427 10.42.69.10 -> 128.105.39.11 NTP NTP
 51 2003-06-13 16:32:33.5232 10.42.69.10 -> 128.105.39.11 NTP NTP
 56 2003-06-13 16:32:34.6049 10.42.69.10 -> 128.105.39.11 NTP NTP
 68 2003-06-13 16:32:36.7638 10.42.69.10 -> 128.105.39.11 NTP NTP
 74 2003-06-13 16:32:37.8441 10.42.69.10 -> 128.105.39.11 NTP NTP
 78 2003-06-13 16:32:38.9242 10.42.69.10 -> 128.105.39.11 NTP NTP
 84 2003-06-13 16:32:40.0050 10.42.69.10 -> 128.105.39.11 NTP NTP
 90 2003-06-13 16:32:41.0846 10.42.69.10 -> 128.105.39.11 NTP NTP
 96 2003-06-13 16:32:42.1647 10.42.69.10 -> 128.105.39.11 NTP NTP
<snip>
998 2003-06-13 16:35:13.3789 10.42.69.10 -> 128.105.39.11 NTP NTP

Frame 998 (90 on wire, 90 captured)
    Arrival Time: Jun 13, 2003 16:35:13.378978000
    Time delta from previous packet: 0.524605000 seconds
    Time relative to first packet: 168.498125000 seconds
    Frame Number: 998
    Packet Length: 90 bytes
    Capture Length: 90 bytes
Ethernet II
    Destination: 00:0a:41:db:58:00 (00:0a:41:db:58:00)
    Source: 00:0a:8b:bf:70:7c (00:0a:8b:bf:70:7c)
    Type: IP (0x0800)
Internet Protocol, Src Addr: 10.42.69.10 (10.42.69.10), Dst Addr: 128.105.39.11 (128.105.39.11)
    Version: 4
    Header length: 20 bytes
    Differentiated Services Field: 0x00 (DSCP 0x00: Default; ECN: 0x00)
        0000 00.. = Differentiated Services Codepoint: Default (0x00)
        .... ..0. = ECN-Capable Transport (ECT): 0
        .... ...0 = ECN-CE: 0
    Total Length: 76
    Identification: 0x2cc7
    Flags: 0x00
        .0.. = Don't fragment: Not set
        ..0. = More fragments: Not set
    Fragment offset: 0
    Time to live: 243
    Protocol: UDP (0x11)
    Header checksum: 0xb335 (correct)
    Source: 10.42.69.10 (10.42.69.10)
    Destination: 128.105.39.11 (128.105.39.11)
User Datagram Protocol, Src Port: 23457 (23457), Dst Port: 123 (123)
    Source port: 23457 (23457)
    Destination port: 123 (123)
    Length: 56
    Checksum: 0xb0bd (correct)
Network Time Protocol
    Flags: 0x0b
        00.. .... = Leap Indicator: no warning (0)
        ..00 1... = Version number: reserved (1)
        .... .011 = Mode: client (3)
    Peer Clock Stratum: unspecified or unavailable (0)
    Peer Polling Interval: invalid (0)
    Peer Clock Precision: 1.000000 sec
    Root Delay:    0.0000 sec
    Clock Dispersion:    0.0000 sec
    Reference Clock ID: Unindentified reference source ''
    Reference Clock Update Time: NULL
    Originate Time Stamp: NULL
    Receive Time Stamp: NULL
    Transmit Time Stamp: NULL

0000  00 0a 41 db 58 00 00 0a 8b bf 70 7c 08 00 45 00
0010  00 4c 2c c7 00 00 f3 11 b3 35 0a 2a 45 0a 80 69
0020  27 0b 5b a1 00 7b 00 38 b0 bd 0b 00 00 00 00 00
0030  00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
0040  00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
0050  00 00 00 00 00 00 00 00 00 00

```

Network staff from two universities investigated two source hosts which I reported as being sources of these anomalous SNTP queries. Both reported that a Netgear brand router was the source of the traffic. (Specifically, one was identified as model MR814.)

Now things started to make some sense. Many source hosts all using the same source port number could be explained by an embedded SNTP client in which the programmer hard-coded the source port number (23457).

* * *

### Gathering Background Information

While searching the web for background information on Netgear products' acclaimed NTP support, I came across the following quote (from [ICSA Labs Firewall Lab Report on the NETGEAR FR114P](http://www.icsalabs.com/html/communities/firewalls/newsite/certification/vendors_4/netgear/netgear.pdf)):

> The Netgear FR114P relied on a separate NTP-based time source to set the current date and time, as it did not have an internal battery and clock. The product is hard-coded with specific NTP time sources that are accessible through the public Internet. Even after configuring the product to access a specific NTP server, the product still attempted to access its hard-coded NTP time sources, while simultaneously accessing the time source specified
>
> **Conclusion**
>
> The Candidate Firewall Product met all the criteria elements in the Baseline and Residential modules and therefore has attained ICSA Labs Firewall Certification.

*Note that Netgear reports that the FR114P does not contain the specific SNTP flaws described here.* I just found it interesting, since that was the only NTP-related flaw about which I found information on the web.

* * *

### Examining the Netgear Code

In order to verify our hypothesis that the source of the flood of SNTP queries are the Netgear Platinum family products and to properly characterize this problem to the vendor, the Netgear code for a number of their products was downloaded and investigated.

Simply by using the Unix "strings" command, I was able to verify that indeed the Netgear code seems to contain the magic number 23457 (as a port number):

> ```
> $ strings RP614_4_12.bin |grep 23457
>  on 23457 port.
> $ strings MR814_4_11.bin |grep 23457
>  on 23457 port.
>                                                                                 
> ```

Using a similar technique, I found the following IP addresses embedded as ASCII strings in RP614\_4\_12.bin:

> ```
> 128.105.39.11 # ntp1.cs.wisc.edu (a.k.a. "caesar.cs.wisc.edu")
> 192.168.1.101
> 66.37.215.43
> 12.234.94.14
> 192.168.0.1
>                                                                                 
> ```

(Note that I added the DNS name comments for clarity; those strings did not occur in the binary file.)\
 Likewise, these IP addresses were embedded in MR814\_4\_11.bin:

> ```
> 0.0.0.0
> 12.234.94.142
> 66.37.215.43
> 192.168.0.102
> 128.105.39.11 # ntp1.cs.wisc.edu (a.k.a. "caesar.cs.wisc.edu")
> 192.168.0.1
> 192.168.1.101
>                                                                                 
> ```

Of 3 globally routable IP addresses therein, only 128.105.39.11 appears to be used as an NTP server. One of the others was an IP address previously used by the "dyndns.org" dynamic DNS name service. Netgear has reported to us that the remaining embedded globally routable IP addresses are no longer used and that they are part of dead code left over from debugging by one of the developers.

* * *

## Contacting Netgear

On June 16, 2003 I sent the following email message to Netgear support. Since this issue is more significant than the typical customer support inquiry, I also sent it directly to some Netgear employees (whose email addresses were culled from the web) asking them to communicate it to the proper people in engineering and/or to have someone contact me by phone or email.

#### Figure 6. Email to Netgear Support

```
Date: Mon, 16 Jun 2003 16:00:21 -0500
From: Dave Plonka <plonka@localdomain>
To: support@netgear.com
Subject: NETGEAR products abusing University of Wisconsin time server


NETGEAR support folks,

Since May 14, 2003, the publicly-advertised Internet time server
"ntp1.cs.wisc.edu" (a.k.a. "caesar.cs.wisc.edu", 128.105.39.11) at the
University of Wisconsin-Madison has been the recipient of a large-scale
flood of time queries apparently from NETGEAR products deployed
throughout the Internet.

Currently, based on our analysis we believe that the NETGEAR "Platinum"
products such as the RP614 and MR814 are the primary source of this
flood of traffic.  They likely will need to have their code changed to
mitigate what is essentially an accidental Denial-of-Service flood
against our NTP infrastructure.  The inbound aggregate traffic rate to
our network from NETGEAR products currently exceeds 250,000
packets-per-second and 150 megabits-per-second at our border routers,
apparently from at least tens-of-thousands of NETGEAR sources.  This
has also cost us numerous work hours of Internet traffic engineering,
troubleshooting, and abuse investigation.

Specifically, the code for the Platinum products appears to contain an
embedded Simple Network Time Protocol (SNTP) client, which sends
queries from UDP port 23457 to port 123 of the IP host 128.105.39.11.
Inexplicably, NETGEAR's products code ships with our server explictly
configured by IP address.  We have determined that at least the
following code images explicitly contain our server's IP address:
MR814_4_11.bin, MR814_v409.bin, RP614_4_0_0.bin, RP614_4_12.bin.

We believe this is inappropriate and not best current practice for
load-balancing and reliability of the Internet's NTP service.  In
addition to the sheer number of deployed products, these requests often
occur at a very fast rate from each device (for instance, one per
second) and therefore put an enormous load on our NTP server.

Please contact me as soon as possible regarding your products' default
SNTP client configration, possible SNTP client software bug, and the
resulting incident which required us to block your customer's SNTP
queries to our Network Time Protocol (NTP) server.

We look forward to hearing from you soon.

Dave

P.S. our NTP server is publicly advertised:

   http://www.ntp.org/
      http://www.eecis.udel.edu/~mills/ntp/servers.html
         http://www.eecis.udel.edu/~mills/ntp/clock2a.html

P.P.S. NTP best practice is described in the "Rules of Engagement" section
of this document:

   http://www.eecis.udel.edu/~mills/ntp/servers.html
```

After receiving no response for days, I called Netgear's headquarters, leaving messages with two executives explaining the seriousness of the situation. I also emailed members of Netgear's executive team by guessing their email addresses, based upon their email naming convention. I included a "Return-Receipt-To" header, and their Mail-eXchanger notified me that all were delivered successfully. Here's a portion of that message:

> At this point I have a complete write-up of this continuing incident, including traffic measurement statistics evidencing the flood and an analysis of its root cause ready to be released publicly.
>
> I absolutely need to hear from responsible parties at NETGEAR immediately, if NETGEAR wishes to begin a dialogue before this goes public. We're not expecting an immediate solution; in fact, I'm fairly certain there is no complete solution without UW-Madison's involvement.

On Thursday, June 19, I received a voicemail message from the director of support for Netgear. He confirmed that they have located some fault in their code. Soon afterward, I began to exchange email and voicemail with him. Having now established initial contact, we proceeded to work this issue outside of Netgear's support system, continuing on with the review process described below.

Netgear's support organization was completely unresponsive. Curiously, I did finally receive the email message below from Netgear's email-based customer support system, some 23 days after I submitted the problem report on June 16.

#### Figure 7. Email from Netgear Support

```
Date: Wed, 09 Jul 2003 09:35:46  +1000
From: support@esupport.netgear.com
Subject: RE:NETGEAR products abusing University of Wisconsin time server [#111678]
To: plonka@localdomain


Thank you for your email.  We apologize for the delay in responding.

Due to an unexpected increase in email volume we have been unable to
respond in a timely manner.  Your issue may have already been
resolved.  Please reply to this email if you still require assistance
and we will respond as quickly as we can.   If your issues is resolved
you do not need to reply and we will consider the case closed.

Again thank you for your patience and understanding.


Please help us serve you better by clicking here
mailto:support@netgear.com?subject=Feedback_us if you would like to
provide any other valuable feedback.  (Note: this feedback is not sent
to an agent so you will not receive a reply.)

```

* * *

## The Review Process

Shortly after beginning a dialogue with Netgear, I proposed the formation of a review team to discuss possible solutions. Netgear agreed, and a review team was formed with about fifteen members, a third from each of these areas:

- Netgear employees
- University employees
- Independent experts from their respective fields:

- Regional Internet Registries
- Internet Measurement Research
- Network Time Protocol

The independent experts agreed to participate without prematurely disclosing the details of the situation.

A number of action items and directions were developed during the review process. These included:

- Fix the SNTP Client
- Propose the Network Operational Options
- Inform the Internet Community
- Clarify Internet Best Current Practice and Protocol Standards

* * *

## The Flawed SNTP Client

The Flawed Netgear SNTP Client implementation in the products affecting UW-Madison has the following characteristics:

- Uses a *hard-coded* IP address for the NTP server 128.105.39.11, that of ntp1.cs.wisc.edu.

- Uses a fixed UDP source port number 23457. \
   This was incredibly advantageous as it allowed UW-Madison to identify and count the Netgear clients. However, due to the widespread use of Network Address Port Translation (NAPT, or NAT/PAT) upstream from some Netgear products, the SNTP request source port number is sometimes rewritten before the request packet reaches its destination.

  > **Note to network operators**: *Please do not* block UDP traffic involving port 23457 nor traffic involving our NTP server's IP address of 128.105.39.11. While we appreciate attempts to help, it may interfere with the best possible solution to this problem.

- Polls at *one second* intervals until it receives a response from the NTP server, after which it uses a longer poll interval such as one minute, ten minutes, two hours, or 24 hours, depending upon product model and firmware version.

* * *

### Impact to Netgear Customers

As of this writing (August 2003) the University is making its best effort to service the Netgear time requests. As such, users of the affected products should not normally notice any problems due to this flaw. Furthermore, based on experience so far, it seems that only a small subset of the customers are even aware of the time-related features of these products (which include logging, policy scheduling, and email notifications).

In parallel, Netgear has produced and continues to work on firmware that does not exhibit the aforementioned problems. Customers can upgrade to newer firmware versions, which are available for download from Netgear's support site. At the time of this writing (August 2003), the most current version of firmware available for the RP614v2, RP614, DG814, and MR814 models does not utilize UW-Madison's time service nor does it poll too frequently.

* * *

### Code Upgrades for Affected Netgear Products

Based on information supplied or confirmed by Netgear, the following products contained these SNTP design flaws. Where applicable, I have labeled each with the earliest version of code containing a fix:

* * *

### Flawed Product Counts

I have counted more than 500,000 unique Netgear sources that queried our time server in one day. This measurement likely underestimates the actual count because of Network Address Port Translation, which modifies the source IP address and port number, and because some broadband residential services drop the customer's link when the service is not in use.

As of June 30, 2003, Netgear reported a total of *707,147* affected products manufactured. Some simple math: If there are 700,000 errant SNTP clients each of which can generate one SNTP request per second to our time server, then the worst-case aggregate rate will be about 700,000 packets per second. Since each SNTP packet is 76 bytes in size, that is also 426 megabits per second of traffic.

Figure 8a shows the actual number of unique NTP Netgear client IP addresses observed per day by a router on UW-Madison's network. Theoretically, counting the clients in this way could overestimate the count if the clients' DHCP servers changes the client IP address frequently. However, based on the number of products reported as having been manufactured, it seems fairly accurate.

* * *

## Suggested Fixes

During the review process a number of improvements to the SNTP client were suggested. These included that an SNTP implementation:

- SHOULD use a poll interval within the range from 64 to 1024 seconds or longer
- SHOULD use local NTP server(s) or multicast when available, as configured by the operator or determined by a discovery mechanism such as via the DHCP "Network Time Protocol Servers Option", which is defined in section 8.3 of RFC 2132.
- MAY performance exponential backoff of poll interval (within the aforementioned range) upon failure to receive a response from the NTP server(s)
- MUST NOT use a shorter poll interval upon failure to receive a response from the NTP server(s)
- MUST allow the operator to configure the query behavior with respect to whether or not it is enabled or disabled and with respect to which candidate time servers can be queried.
- SHOULD use the Domain Name System to determine candidate server(s) IP address(es), so that the NTP server's zone administrator can influence the client behavior.
- SHOULD resolve the server IP address via DNS before each poll/query, so that the pertinent DNS entries' Time-To-Live values are respected.
- SHOULD support the existing NTP access-control mechanism by, upon receiving a valid \`kiss-of-death' packet, reporting the condition and discontinuing queries to the server in question until reinitialization.
- MAY use an implementation-defined fixed source port number

Some of these have been implemented in the initial fix but others are only under consideration. Hopefully these suggestions will be evaluated during an upcoming SNTP standardization effort.

* * *

### The Initial Fix: "Instant" Code

During the review process, we learned that Netgear already was having SNTP-related code changes developed for the RP614v2 product prior to my initial notification of the problems the flaw was causing to the University.

Regarding Firmware v5.13 RC7 for the RP614v2, Netgear made this new code available to me on July 10. My testing found that the modified SNTP client had these characteristics, much as they described:

- Now requires a DNS server to be configured (or learned via DHCP) before generating any SNTP queries.
- The code performs DNS queries for "time-a.netgear.com" and "time-b.netgear.com" at ten minute intervals until success, alternating names if no response is received. I verified also that it supported responses with CNAMEs or multiple A records as well.
- Following successful DNS resolution, it sends an NTP query to the resolved IP address and waits for a reply. If no reply comes in ten minutes, it again resolves the name, and requeries. It appears to give up after five retries.
- Whenever any configuration change is applied via the web interface, it causes the device's clock to be zeroed, the NTP server to be re-resolved, and subsequently queried.

However I also found these bugs:

- The SNTP client in this code does not appear to validate the NTP response packet. It will accept any incoming packet to port 23457 as a valid response even if the flags are set wrong (for instance, indicating that it is another client query rather than a server response).
- While the SNTP client is awaiting a response (after querying either time-a or time-b) it seems to accept any UDP response packet, even if the source IP address of that UDP packet is not that of the time server that it queried.

This code was made available for download on the Netgear web site for the RP614v2 on or about July 11, 2003.

Netgear continues to develop improvements to their SNTP client and has vetted the design with the review team.

* * *

## Network Operational Options: To Serve or To Sever?

These flawed devices are not easily reconfigurable. Representatives from both Netgear and UW-Madison believe that it is not a viable option to rely on Netgear's customers to upgrade to the newer firmware (the first of which was released in July) to correct the errant behavior.

Our review team has considered a number of possible options about how to deal with the errant Netgear time requests. While I won't discuss all the details here, the two primarily endgames on which we've focused are outlined below.

### Endgame A: UW-Madison Netgear Anycast Time Service

In this option we would deploy highly-reliable, redundant NTP servers at WiscNet's borders and route the inbound requests destined to 128.104.39.11 to them using BGP anycast. (Anycast is a technique that can often be employed to route traffic for some stateless RPC services, such as DNS or NTP, which are based upon UDP.) Implementing this option would likely include placing a pair of rack-mount NTP servers at each of three locations within WiscNet: UW-Madison, UW-Milwaukee, UW-Eau Claire. These are nearest the three current border Internet exchange points and therefore provide the most diverse paths for reliability of connectivity to the global Internet.

One distinct advantage of this configuration is that UW-Madison retains as much control as possible over its precious IPv4 address allocations. Because this BGP anycast deployment resides solely within WiscNet (which will honor a single /32 host-address route), this option consumes as little of UW-Madison's IP address space as possible - just the address to which Netgear time requests were directed.

Endgame A has some risk. Whether or not the servers' responses reach the requesting client host is not wholly within the University's control, consequently some amount of flooding will likely continue. There are many reasons other than server failure for disruptions in the end-to-end path between the SNTP clients and servers that could cause the clients not to receive the responses and to flood requests toward our servers anyway. These include asymmetric routing problems, firewalling policies, and disasters affecting any link between the clients and servers. Indeed, even while our time server is dutifully responding to all netgear SNTP requests, we still regularly observe that hundreds of them continue to flood. Apparently these "zombies" never receive our responses.

To limit the possibility of the multiple servers being simultaneously isolated from the Internet, one could consider an even more geographically diverse set of deployment locations, such as that done by the [AS112 Project](http://www.as112.net/), which effectively mitigates the damage caused to the Internet's root name servers by RFC1918-related queries.

Figure 10 is a diagram showing how this service would work. The Netgear SNTP requests heading toward UW-Madison are shown in green. Note that multiple NTP servers, all with the same IP address, are located in multiple locations. WiscNet's border routers divert the inbound SNTP requests to the nearest server. The server responses are shown in red. If any of the servers fail, the traffic should route to one of the remaining NTP servers with the same address.

* * *

### Endgame B: Attempt to Suppress the Requests

To prevent Netgear time requests from being forwarded to our network would require UW-Madison to sacrifice a block of IP address space within the class B network which includes the IP address of ntp1.cs.wisc.edu.

Because of the way the Internet's backbone routing is operated, and to keep the number of routes manageable, network routes are sometimes not respected unless they are sufficiently large. In today's Internet, that means a route might not be considered legitimate unless it represents 2,048 or 4,096 contiguous addresses. Respectively, network operators would call those size "/21" or "/20" (pronounced "slash twenty") blocks because they represent networks having netmasks of 21 or 20 contiguous bits.

Figure 11 is a diagram showing this configuration. The BGP updates originating from UW-Madison's border router are shown in red. The Netgear SNTP Requests are shown in green. The ICMP unreachable messages returned to the client by BGP-aware border routers throughout the Internet are shown in blue. These inform the client that the network in which the NTP server would reside is unreachable.

* * *

### Endgame B: IP Resources Required

This endgame that tries to suppress the forwarding of requests comes at a significant cost to the University - we may have to sacrifice, likely for the lifetime of the flawed products, as many as 4,096 IP addresses. Figure 12 shows how our existing 128.105.0.0/16 network could be divided, and the one slice "/20" block which would be excluded from the Internet's global BGP routing table.

The risks of endgame B include the possibility that some portions of the Internet might not be able to reach legitimate campus IP addresses that lie near the sacrificial, unadvertised block. Input from the backbone network operations community and real-world experience must determine which solution best serves the University and Internet community as a whole.

* * *

## Inform the Internet Community

The public release of this document is part of an effort to inform the Internet community of this flaw and the resulting floods, with the hope of minimizing the likelihood of such a mistake being repeated elsewhere. Note that Netgear was notified of our plan to publicly disclose the details herein weeks in advance of its release. Furthermore, earlier revisions of this document were provided to them for review.

Because of the scope of the resulting problem, with hundreds of thousands of ill-behaved Internet hosts distributed world-wide, and because of the scale and unexpected nature of the flooding, with aggregate rates which could completely fill some network links, I felt that it was important to inform others and solicit advice from experts.

Following this disclosure, it's my intent to find appropriate venues to further present and review the dangers and potential solutions to this and similar problems.

For instance, during the review process we learned that the Commonwealth Scientific & Industrial Research Organisation ([CSIRO](http://www.csiro.au)) in Australia is having similar trouble with about 85,000 SMC brand routers that poll the CSIRO time server twice a minute when they don't receive a response. A story about that incident, "[Rogue routers cause havoc for CSIRO](http://australianit.news.com.au/articles/0,7204,6716567%5e15340%5e%5enbv%5e15306-15318,00.html)", can be found here:

> [http://australianit.news.com.au/articles/0,7204,6716567%5e15340%5e%5enbv%5e15306-15318,00.html](http://australianit.news.com.au/articles/0,7204,6716567%5e15340%5e%5enbv%5e15306-15318,00.html)

While the scale of the CSIRO problem is orders-of-magnitude less, with floods of perhaps 2,800 packets per second and 1.7 megabits per second, it is strikingly similar and perhaps not as likely to be as responsibly addressed with the assistance of that manufacturer.

* * *

## Clarify Internet Best Current Practice and Protocol Standards

Also during the review process, some members of the review team began work on [Internet Drafts](http://www.rfc-editor.org/) to improve documentation pertinent to this issue. There are at least two such efforts currently in their infancy:

1. I am in the process of preparing an Internet Draft, currently titled "Embedding Globally Routable Internet Addresses Considered Harmful", which denounces the practice of embedding unique, globally routable IP addresses in Internet hosts, describes some of the resulting problems, and considers selected alternatives.
2. Members of the NTP community have revised and reviewed the existing Informational RFC2030 that describes SNTP. Through their efforts and perhaps those of other interested parties, it may be possible to revisit NTP and SNTP as a standards track protocol within the [IETF](http://www.ietf.org).

* * *

## Status, August 21, 2003

I'm pleased to report that Netgear has cooperated with us on the initial steps of this process and we are forging an agreement that will enable us to implement a suitable solution.

For the time being, UW-Madison continues to service Netgear SNTP requests in spite of receiving occasional large-scale floods of traffic from Netgear products. A recent incident is shown in Figure 13. The shark-fin shaped anomaly on the right is a flood of inbound Netgear time requests which grew to about 100,000 packets per second before subsiding.

Both the magnitude and duration of the Netgear-caused incidents continue to present a serious operational problem for UW-Madison. While essentially involved in a game of russian roulette at the moment, we are hoping to utilize the expertise of both UW-Madison and the Internet operator community to design and implement a good solution.

* * *

## Afterthoughts

Here are some questions, offered as food for thought, that were brought to mind by this case study:

- What does this unintentional Denial-of-Service flood indicate about the viability of some public Internet services?
- Can the Internet routing infrastructure be improved to enable less disruptive solutions to such problems?
- Are incidents such as this a likely side-effect of ubiquitous, low-cost, perhaps even disposable Internet hosts?
- Are the manufacturer, vendor, Internet operations, and user communities willing and able to cooperate to address such problems?

* * *

## Acknowledgements

The following provided assistance with the data gathering, analysis, and people networking:

- University of Wisconsin-Madison: Jeff Bartig, Jim Gast, Michael Hare, Adam Kunen, Dave Thompson
- University of Florida: Robert Bird, Greg Goddard
- Harvard University: Greg Mazzu
- k claffy, Nevil Brownlee, George Michaelson

I'd also thank the members of the review team, including those remaining anonymous. I'm certain we'll come to a better solution because of their participation.

* * *

## Analysis Tools

The following tools were used during this investigation:

- [MRTG](http://people.ee.ethz.ch/~oetiker/webtools/mrtg/)
- [RRDTOOL](http://people.ee.ethz.ch/~oetiker/webtools/rrdtool/)
- [RRGrapher](http://net.doit.wisc.edu/~plonka/RRGrapher/)
- [junipoll](http://net.doit.wisc.edu/~plonka/junipoll/)
- [flow-tools](http://www.splintered.net/sw/flow-tools)
- [Cflow](http://net.doit.wisc.edu/~plonka/Cflow/)
- [FlowScan](http://net.doit.wisc.edu/~plonka/FlowScan/)
- [tcpdump](http://www.tcpdump.org)
- [ethereal (now wireshark)](https://www.wireshark.org/)
- [sntp.pl](http://www.kloth.net/software/sntp.htm)
- [strings](http://www.gnu.org/manual/binutils-2.12/html_node/binutils_9.html#SEC9) ;^)

* * *

## References / Further Reading

1.  [RFC2030: Simple Network Time Protocol (SNTP) Version 4 for IPv4, IPv6 and OSI](http://www.ietf.org/rfc/rfc2030.txt) (status: Informational)
2.  [RFC1305: Network Time Protocol (Version 3) Specification, Implementation and Analysis](http://www.ietf.org/rfc/rfc1305.txt) (status: DRAFT STANDARD)
3.  [home of the Network Time Protocol (NTP) project](http://www.ntp.org/)
4.  [Public NTP Time Servers](http://www.eecis.udel.edu/~mills/ntp/servers.html)
5.  [Public NTP Secondary (stratum 2) Time Servers](http://www.eecis.udel.edu/~mills/ntp/clock2a.html)
6.  [RFC2132: DHCP Options and BOOTP Vendor Extensions](http://www.ietf.org/rfc/rfc2132.txt)
7.  [RFC1546: Host Anycasting Service](http://www.ietf.org/rfc/rfc1546.txt)
8.  [http://www.networksorcery.com/enp/protocol/sntp.htm](http://www.networksorcery.com/enp/protocol/sntp.htm)
9.  [an article on how to change Windows' time server configuration using regedit](http://www.winforums.org/viewthread.php?tid=1643)
10. [Basic Operation of the Windows Time Service](http://support.microsoft.com/default.aspx?scid=kb;EN-US;224799)
11. [http://www.microsoft.com/windows2000/docs/wintimeserv.doc](http://www.microsoft.com/windows2000/docs/wintimeserv.doc)
12. [Flawed Routers Flood University of Wisconsin Internet Time Server (NANOG 29 presentation)](http://www.nanog.org/mtg-0310/plonka.html)
13. [A Case Study in Internet Pathology: Flawed Routers Flood University's Network (LISA '03 talk)](http://net.doit.wisc.edu/~plonka/lisa/lisa2003/)

* * *

## Frequently Asked Questions

1. ### What is Netgear's liability for causing (however inadvertently) this denial of service for your network?

   > My work responsibilities are not ones that would make me a participant in such negotiations. However, as I reported, an agreement is being forged.
   >
   > Note to others: *Please do not ask me for financial or legal details.* This document is a technical summary of the situation and is not the vehicle by which to deliver such details.
   >
   > You may be interested in this news article: [http://www.doit.wisc.edu/news/story.asp?filename=322](http://www.doit.wisc.edu/news/story.asp?filename=322)

2. ### Have you considered putting up a server which sends back fake answers to netgear clients, to cause people to upgrade?

   > Yes, but we didn't consider it for long. Both Netgear and others on the review team agreed that it would likely have very little effect, since only a small subset of the customers seem to even be aware of the NTP-related features of the affected products. Besides, the proposed "dishonest" time server (which would report the wrong time) would have to be operated at the IP address that is currently that of our well-known *reliable* time server. It would be quite rude for it to suddenly become unreliable since the whole purpose of the public time service is to answer with the correct time. The University intends to provide the best possible service, regardless of how it's abused.

3. ### What is the expected life-time for these products?

   > Personally, I think its likely that many of them will be around until five to ten years from now. Its only a guess, but perhaps the half-life is five years, after which we'd expect to see less than 350,000 of the affected products remaining in use.

4. ### In figure 13, could that "shark fin" spike have anything to do with last week's power grid failure (Blackout 2003), and subsequent "rolling" restoration?

   > No, it did not coincide with the blackout that affected the east coast in August 2003. However, I did look for evidence of the blackout in the netgear SNTP traffic, and found only slight less during the outage. Relatively little of the Internet was actually affected by the power outage. Early reports were that only a few thousand BGP prefixes from only a couple hundred autonomous systems were offline. Those that were offline were unreachable in both directions, so we didn't receive requests from them until power was returned. The rolling restoration served to distribute the load to our server as those netgear clients came online.

5. ### Are there other devices than those mentioned which also suffer from the flaw which causes inadvertent flooding of your network?

   > A number of Netgear users have reported to me that Netgear model RO318, firmware version V3.26, also utilizes our time server and also logs ntp errors to its security log. I have not evaluated this product, so do not know what retry times it uses, but will report this to Netgear.

6. ### What was the effect of this article being slashdotted?

   > While having a significant effect on the web server, overall it was an insignificant level of traffic for the campus. The server handled 30 requests a second for a while.
   >
   > Here's a graph comparing the web server hits-per-second to the number of Netgear-sourced SNTP request flows-per-second, when this report was slashdotted. (Note that most Netgear clients were receiving replies from our server at this time, so they were not flooding requests.)
   >
   > ![](https://pages.cs.wisc.edu/~plonka/netgear-sntp/slashdotted.gif)

7. ### Why is a traditional manufacturer recall/defect solution not a possibility?

   > Both Netgear and other members of the review team felt that it was unlikely that all but a very small subset of the owners would return the affected device since they appear to be working fine. Also, very few customers have registered these products with the manufacturer, so it is impractical to contact them.

8. ### I'm with \[the IT press\], do you have some time to speak with me?

   > When this report was first presented, an astute audience member proposed that the IT press ostensibly plays an important role in evaluating whether or not consumer products comply with Internet standards and best current practice.
   >
   > Given that, if you're with the IT press and your publication does Internet product reviews or makes "Editor's Choice" awards, I am willing to correspond with you by email about this story. Specifically, the Internet community may benefit from our exploring the IT press' familiarity with Internet standards and best practices and the press' assessment of its ability to evaluate products.
   >
   > By tying the reporting of this story in with the product evaluation and recommendation function of the IT press, I'm hopeful that the community could reduce the likelihood of such flaws causing such problems. It's just an idea, let me know what you think.

9. ### How has this story been covered in the press?

   > While certainly not an exhaustive list, here is a sampling of coverage.\
   >  (Please be aware that some of these articles are misleading or incorrect about the details;\
   >  Only a few of them contacted me to check the facts.)
   >
   > - **Slashdot**: [Netgear Routers DoS UWisc Time Server](http://developers.slashdot.org/article.pl?sid=03/08/22/1454233&mode=thread&tid=126&tid=128&tid=156&tid=95)
   > - **The Inquirer**: [Wisconsin Uni page describes Netgear \`flawed routers'](http://www.theinquirer.net/?article=11183)
   > - **Wisconsin Technology Network**: [Flawed Routers Flood UW Server](http://www.wistechnology.com/FlawedRouters.php)
   > - **ZDNet**: [NetGear flaw triggers accidental DoS attack](http://zdnet.com.com/2100-1105_2-5068035.html)\
   >
   > - **CNET News.com**: [NetGear flaw triggers accidental DoS attack](http://news.com.com/2100-1002_3-5068035.html?tag=cd_mh)
   > - **ZDNet Australia**: [UPDATE: Netgear routers attack university](http://www.zdnet.com.au/newstech/communications/story/0,2000048620,20277694,00.htm)
   > - **Network World**: [Netgear router quirk perturbs college](http://www.nwfusion.com/news/2003/0901wisconsin.html)\
   >    (also in print Network World magazine)
   > - **TECHWORLD**: [NetGear sets off denial of service attack](http://www.techworld.com/news/index.cfm?fuseaction=displaynews&NewsID=409)
   > - **Network Computing**: [Industry Insights: Opportunity Knocks / Quality Counts](http://www.networkcomputing.com/1417/1417collee.html)\
   >
   > - **PC World**: [NetGear Routers Wage War on University](http://www.pcworld.com/howto/article/0,aid,112465,pg,2,00.asp)\
   >    (also in print in the November 2003 issue of PC World magazine)

* * *

Copyright 2003, Dave Plonka.\

* * *

```
$Id: index.wml,v 1.39 2006/07/19 15:20:28 plonka Exp $


$Log: index.wml,v $
Revision 1.39  2006/07/19 15:20:28  plonka
updated figure 8a

Revision 1.38  2005/04/28 16:13:02  plonka
updated figured 8a

added NANOG and LISA talks to references

added news story url to faq entry

fixed a typo

Revision 1.37  2004/09/28 18:37:16  plonka
fixed a typo

Revision 1.36  2004/05/19 22:54:31  plonka
updated figure 8a

Revision 1.35  2004/02/05 17:29:12  plonka
added figure 8a, Netgear SNTP Clients Per Day

Revision 1.34  2003/12/04 22:30:58  plonka
fixed a typo

Revision 1.33  2003/10/16 22:30:05  plonka
added a faq entry

Revision 1.32  2003/09/15 22:13:13  plonka
fixed some typos and the host address counts for /20 and /21 blocks

Revision 1.31  2003/09/12 18:09:49  plonka
added info about code upgrade for HR314

Revision 1.30  2003/09/10 15:03:26  plonka
fixed a typo

Revision 1.29  2003/09/10 14:58:50  plonka
added using DHCP "Network Time Protocol Servers Option" to "Suggested Fixes"

Revision 1.28  2003/08/30 02:22:20  plonka
added faq entry

Revision 1.27  2003/08/27 18:42:42  plonka
added graph evidencing the flash crowd when this report was slashdotted

Revision 1.26  2003/08/26 21:40:46  plonka
fixed some typos and reworded a couple sentences

Revision 1.25  2003/08/25 23:54:09  plonka
added faq entries

```
