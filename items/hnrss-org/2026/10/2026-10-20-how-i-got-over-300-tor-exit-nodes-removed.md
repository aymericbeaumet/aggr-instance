---
title: How I got over 300 Tor exit nodes removed
link: https://kaba.cx/posts/20261020_Tor/
source: hnrss-org
published: 2026-10-20T13:39:24Z
updated: 2026-10-20T13:39:24Z
first_seen: 2026-10-20T17:55:58.869157456Z
authors:
- kabakabakaba
content: extracted
html: 2026-10-20-how-i-got-over-300-tor-exit-nodes-removed.html
remote_preview:
  url: https://kaba.cx/img/tor/relays_offline_hero.png
  alt: How I got over 300 Tor exit nodes removed
---

On Friday Oct 2, 09:35am, 333 infected exit nodes started a coordinated campaign to intercept SSH connections and gather as much unencrypted data as possible.

To give you a sense of scale, all of Tor has around 3000 exit relays, so roughly 10% were under their control.

The attackers had gained access to the servers two months earlier, in August, and quietly deployed their Ebury 1.8.3 payload (an OpenSSH backdoor and credential stealer for Linux).

## The Attack

The attack appears to have had two primary goals:

1. Gather as much unencrypted mail traffic as possible
2. Intercept SSH connections (MITM)

By blocking certain ports, they made their exit nodes handle only unencrypted mail traffic. An interesting writeup by one of the victims (a veteran Tor operator) goes into great detail about that: [lists.torproject.org/mailman3](https://lists.torproject.org/mailman3/hyperkitty/list/tor-relays@lists.torproject.org/thread/QHRD6YOX6RZRDR7NCHNDO7GLCEA6VFM6/)

In this post I'll focus on the second attack, the SSH MITM. If you happen to get a circuit that exits over one of those 333 infected exit nodes, you won't notice anything at first. Encrypted web traffic is forwarded as usual, but if you attempt an SSH connection over Port 22, you're unknowingly redirected to one of 12 SSH interception servers.

![Attacker Infrastructure](https://kaba.cx/img/tor/tor-ssh-incident.svg)

That's exactly what happened to me on Friday morning. My SSH connection got redirected, and I was greeted with this warning:

```

@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that a host key has just been changed.
The fingerprint for the ED25519 key sent by the remote host is
SHA256:6baCZCPsl+AfQuujUS81fMEE/C7UeJ+AwjrImqmDB3E.
Please contact your system administrator.
```

But if you ignore the warning and go ahead with the login, the attacker does three things:

1. Steals your password
2. Opens a connection to your server and observes each command you type (MITM)
3. Opens a second connection to your server and infects it with Ebury

## Getting them taken down

On Friday evening I contacted the [Tor Project's network health team](https://community.torproject.org/relay/community-resources/bad-relays/) with my findings. They confirmed this was something new and got to work by contacting the affected relay operators.

> *"We've not seen an attack like that before."* \
>  - Tor Network Health Team

![Malicious Tor exit nodes going offline](https://kaba.cx/img/tor/relays_offline_detail.svg)

1. **Fri Oct 2**: Interception started.
2. **Sun Oct 4**: The Tor network health team [made an announcement to the operators](https://lists.torproject.org/mailman3/hyperkitty/list/tor-relays@lists.torproject.org/thread/WOHPLKCSHYXFTBRZXDENDKJZMBMPLMG5/), who began shutting their exit nodes down.
3. **Sun Oct 4**: The relays were marked BadExit, so clients would no longer use them to exit traffic.
4. **Mon Oct 5**: The attackers restarted many of the relays, but since they were already flagged they could only serve as middle relays.
5. **Mon Oct 5**: All infected servers were shut down, partly by the operators and partly by the attackers.

In their announcement, the health team described the pattern:

> *"It seems the attacker is gaining access to Linux based hosts and when these machines are running non-exits they are editing the torrc file and turning them into exits to be able to carry out the SSH attack."* \
>  - Tor Network Health [in the announcement](https://lists.torproject.org/mailman3/hyperkitty/list/tor-relays@lists.torproject.org/thread/WOHPLKCSHYXFTBRZXDENDKJZMBMPLMG5/)

You can see the impact of those 333 exit nodes being shut down. On [metrics.torproject.org](https://metrics.torproject.org/relayflags.html?start=2026-09-02&end=2026-10-19&flag=Exit) a clear dip is visible on October 4th and has not recovered since.

![Tor exit node count dipping](https://kaba.cx/img/tor/relayflags-Exit-2026-09-02-2026-10-19.png)

 *Consider running a relay: [community.torproject.org/relay/setup/](https://community.torproject.org/relay/setup/)*

* * *

## The technical part: how I found them

In this part I'll go into detail about how I found all the infected exit nodes.

Using [exitmap](https://gitlab.torproject.org/tpo/network-health/exitmap), I built a simple script that did the following: it pinned an exit node, then opened an SSH connection from my server A to my server B.

- Server A checked whether it received the correct SSH fingerprint from server B.
- Server B checked whether the connection came from the pinned exit node or from one of the 12 SSH interception servers.

I ran the script against every single one of the ~3000 exit relays. Around a dozen served a fake fingerprint, and over 300 didn't intercept anything but redirected through those same interception servers. Redirecting alone was enough to confirm the exit was infected.

In some further analysis I used [sshesame](https://github.com/jaksi/sshesame) as a honeypot. Tricking the MITM into logging into the honeypot showed that they used paramiko (a Python SSH library) and also how they attempted to install Ebury using Perl.

```
    
> echo "U=$(id -u) S=$(command -v sudo >/dev/null 2>&1 && (sudo -n true 2>/dev/null && echo nopass || echo maybe) || echo none)"
> sudo bash -c 'echo <base64> | base64 -d | perl'
    
```

Checking for permissions first, then executing a base64-encoded command.

## Open Questions

The biggest question is still how they got into all those servers in the first place. One theory is that the hosting company itself was hacked (discussed here: [lists.torproject.org](https://lists.torproject.org/mailman3/hyperkitty/list/tor-relays@lists.torproject.org/thread/RXWGWYYYKBDEBNXC2C3NYIQ2HHHWXL7O/)). But that's just a theory floating around. In truth, no one really knows.

Another is who's behind it. The use of Ebury and the broad collection of unencrypted data point to a financially motivated crime group. Though this too is just speculation.

> *"Given how many servers had Ebury between 2009 and 2023 (400 000!) there are most likely other compromised Tor relays out there."* \
>  - Marc-Etienne M. Léveillé ([lists.torproject.org](https://lists.torproject.org/mailman3/hyperkitty/list/tor-relays@lists.torproject.org/thread/RXWGWYYYKBDEBNXC2C3NYIQ2HHHWXL7O/))

I also suspect they deliberately launched their attack on a Friday, hoping that no one would want to ruin their weekend by responding. All the more credit to the Tor Team for acting so quickly!

## Takeaway

The lesson is the same as always: extra security is a worthwhile investment. Disable password authentication in favor of ssh keys. I for one am going to buy myself some hardware keys today. :)

> *"At the end of the day, relays/bridges need to be treated as highly targeted production machines."* \
>  - George ([lists.torproject.org](https://lists.torproject.org/mailman3/hyperkitty/list/tor-relays@lists.torproject.org/thread/RXWGWYYYKBDEBNXC2C3NYIQ2HHHWXL7O/))
