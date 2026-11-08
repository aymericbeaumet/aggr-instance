---
title: The Port That Does Not Exist
link: https://acid.vegas/blog/the-port-that-does-not-exist/
source: lobste-rs
published: 2026-11-06T23:03:12Z
updated: 2026-11-06T23:03:12Z
first_seen: 2026-11-08T00:25:04.111827219Z
authors:
- acid.vegas via drmorr
labels:
- linux
- networking
- port-0
- scanning
- tcp
summary: Comments
content: extracted
html: 2026-11-06-the-port-that-does-not-exist.html
remote_preview:
  url: https://acid.vegas/blog/the-port-that-does-not-exist/header.png
---

[ACIDVEGAS most dangerous motherfuck x come get some](https://acid.vegas/) [![](https://acid.vegas/i/crowdkill.gif)](https://acid.vegas/assets/hardcore.wav) 2026-11-06

Port 0 is the port you are not supposed to use, so nobody looks there. So we put an IRC network on it. Then I got curious whether anyone else had, and scanned all of IPv4 for port 0. Turns out we weren't alone, and some of our neighbours really shouldn't be there.

Ask any program to listen on port 0 and it will hand you a socket on port 47193...or 58037...or whatever the kernel felt like that day. Port 0 is not a port you can have. It is the number you pass when you want the operating system to pick one for you.

But a TCP header is just bytes. The destination port field is 16 bits wide, and `0x0000` fits in 16 bits as well as anything else does. Nothing on the wire stops a packet from being addressed to port 0. Your tools will refuse it, your browser will block it, and your scanner's default port list skips it, because everyone agrees it isn't a real port.

Which makes it a pretty good place to hide things, whether anyone meant to or not. So we did some TCP cowboy shit & port scanned the entire fucking internet for open ports on port 0, which uncovered some interesting unexpected findings.

## What Port 0 Actually Is

The standards are thin on this. [RFC 9293](https://www.rfc-editor.org/rfc/rfc9293), the 2022 consolidation of the TCP spec, defines the source and destination port as "16 bits" and leaves it there. It has no rule against zero.

IANA lists `0/tcp` and `0/udp` as **Reserved**, pointing at [RFC 6335](https://www.rfc-editor.org/rfc/rfc6335). That RFC splits the port space into System (0–1023), User (1024–49151) and Dynamic (49152–65535) ranges, and explains what "Reserved" means:

> Reserved port numbers include values at the edges of each range, e.g., 0, 1023, 1024, etc., which may be used to extend these ranges or the overall port number space in the future.

So port 0 is not forbidden. It is set aside. The only place zero has a defined protocol meaning is UDP: [RFC 768](https://www.rfc-editor.org/rfc/rfc768) makes the source port optional, and "if not used, a value of zero is inserted." TCP has nothing like that.

## Why You Can't Listen On It

Port 0's reputation comes from the sockets API, not the wire. When you call `bind()` with `sin_port = 0`, you are not asking for port 0. You are asking for *any* port. The `ip(7)` man page lists every case where Linux hands out an ephemeral port: `bind()` with port 0, `listen()` on an unbound socket, `connect()` on an unbound socket, and `sendto()` on an unbound datagram socket.

The range comes from `net.ipv4.ip_local_port_range`, which defaults to `32768 60999` on Linux, not the 49152–65535 that IANA calls Dynamic. And asking for port 0 needs no privileges. `__inet_bind()` only checks `CAP_NET_BIND_SERVICE` when the requested port is non-zero.

How the kernel picks is more interesting than I expected. In `net/ipv4/inet_connection_sock.c` (v6.8), `inet_csk_find_open_port()` starts at a random offset and then does this:

```
offset = get_random_u32_below(remaining);
/* __inet_hash_connect() favors ports having @low parity
 * We do the opposite to not pollute connect() users.
 */
offset |= 1U;
```

It walks the range in steps of two. The function above it says it outright: *"We try to allocate an odd port (and leave even ports for connect())."* Outgoing connections go through `__inet_hash_connect()` in `inet_hashtables.c`, which implements RFC 6056 Algorithm 4 (double-hash port selection) and takes the *even* ports first. The kernel splits the range by parity so servers asking for "any port" and clients making outbound connections don't fight over the same numbers. If `SO_REUSEADDR` is set, `bind()` also tries the lower half of the range before the upper half.

That is easy to test. On a stock Ubuntu box running Linux 6.8:

| Test                               | Result                                       |
| ---------------------------------- | -------------------------------------------- |
| 2,000 × `bind(("127.0.0.1", 0))`   | ports 32777–60997, **2,000 / 2,000 odd**     |
| same, with `SO_REUSEADDR`          | ports 32773–46835 (lower half only), all odd |
| `listen()` with no `bind()`        | port 58037                                   |
| 300 × `connect()` with no `bind()` | source ports 32772–60998, **300 / 300 even** |

So when your program says "listen on port 0", the kernel hears "give me an odd number between 32768 and 61000." No normal program can listen on port 0.

## So We Did It Anyways

![Toad on a skateboard: call the cops, I dont give a fuck](https://acid.vegas/blog/the-port-that-does-not-exist/did-it-anyway.jpg)

You can bypass all of this with raw sockets, but if you just want to cheat, you can simply forward the port to another with iptables/nftables. As a fun proof of concept, I set up my IRC network at `irc.supernets.org` to accept plaintext connections on port **0** and SSL connections on port **1**. JUST TO FLEX. 😎

```
-A PREROUTING -p tcp -m tcp --dport 0 -j REDIRECT --to-ports 6667
-A PREROUTING -p tcp -m tcp --dport 1 -j REDIRECT --to-ports 6697
```

A SYN arrives for port 0, netfilter rewrites the destination to 6667 where the ircd is actually listening, and the reply is rewritten back on the way out. The ircd never knows. Port 1 does the same for TLS on 6697.

## Talking To Port 0 Is Its Own Problem

Getting a server onto port 0 was the easy part. Getting a *client* there is where tools give up. I pointed everything I had at our own server on port 0:

| Client                            | Result                              |
| --------------------------------- | ----------------------------------- |
| OpenBSD `nc`                      | `nc: port number too small: 0`      |
| `telnet` (inetutils)              | `Port number 0 is out of range.`    |
| Chromium                          | `net::ERR_UNSAFE_PORT` (port 1 too) |
| `socat`                           | connects, works                     |
| Python `asyncio`                  | connects, works                     |
| Python `ssl` on a blocking socket | `BlockingIOError`                   |

That last one is strange, because the TCP connection underneath it is fine. The problem is that the kernel says it isn't, and it does that on purpose. Here is `inet_getname()` in `net/ipv4/af_inet.c`, which backs `getpeername()`:

```
if (peer) {
	if (!inet->inet_dport ||
	    (((1 << sk->sk_state) & (TCPF_CLOSE | TCPF_SYN_SENT)) &&
	     peer == 1)) {
		release_sock(sk);
		return -ENOTCONN;
	}
```

Linux uses "destination port is 0" as its internal marker for "this socket isn't connected to anything." A socket that really is connected to port 0 trips that check, so `getpeername()` returns `ENOTCONN` on a working connection.

Python's `ssl.wrap_socket()` calls `getpeername()` to decide whether the socket is connected, gets `ENOTCONN`, decides it isn't, switches to non-blocking and tries a `recv(1)`, gets `EAGAIN`, and raises `BlockingIOError`. The TCP connection is fine. It just reports that it doesn't exist. Python's `asyncio` does TLS through memory buffers instead of `wrap_socket()`, never asks, and works.

## Scanning The Internet For Port 0

You can't scan port 0 with `connect()`-based tools for all the reasons above, but masscan doesn't use the kernel's TCP stack. It builds its own SYN packets, so port 0 is just another number.

**1,173,626** addresses answered with a SYN-ACK.

That sounds like a lot of people using port 0. It isn't. Grouped by `/24`, the responses are packed into a few thousand dense blocks:

| Hosts answering per /24 | /24s   | IPs     |
| ----------------------- | ------ | ------- |
| 1                       | 28,370 | 28,370  |
| 2 – 10                  | 10,253 | 34,654  |
| 11 – 100                | 1,747  | 53,022  |
| 101 – 200               | 865    | 138,300 |
| 201 – 256               | 3,751  | 919,280 |

8% of those `/24`s hold 78% of the responding IPs. Whole blocks answer on every address: `172.65.0.0/16` returned 65,534 of 65,536. That isn't 65,534 servers. It's a firewall or router at the edge answering the SYN for everything behind it. **1,116,675** addresses completed the handshake and never sent a single byte.

## 94% Of The Banners Are A Load Balancer Complaining

56,951 addresses did send something. Almost all of it looked like this:

```
BIG-IP: [0x31dbae9:14718] {peer} TCP retransmit timeout
BIG-IP: [0x31dbae9:4992] {peer} TCP RST from remote system
BIG-IP: [0x2bd2afd:5226] SYN timeout (server not responding)
```

F5 BIG-IP load balancers write the reason for a reset into the RST packet, and masscan saved it as a banner. That's **53,741** of the 56,951 (94%), and 46,583 of them are in `132.246.0.0/16` alone. Drop those, plus 305 hosts returning the same fixed `(Ref.Id: ?...?)` token, and **2,905 addresses** are left. That's about 0.25% of everything that answered.

## What's Actually On Port 0

| Service                | IPs | Notes                                                                                    |
| ---------------------- | --- | ---------------------------------------------------------------------------------------- |
| SSH                    | 748 | 653 OpenSSH (9 of them OpenSSH\_for\_Windows), 72 dropbear, one `SSH-1.99-OpenSSH_3.9p1` |
| MySQL / MariaDB        | 215 | versions 3.23 through 11.8, mostly 5.x                                                   |
| Telnet                 | 211 | router logins, power switches, rack PDUs                                                 |
| HTTP                   | 159 | router admin pages, `micro_httpd`, building controllers                                  |
| FTP                    | 105 | FileZilla, Microsoft FTP, ProFTPD, GNU inetutils, vsFTPd, Serv-U, lots of NAS boxes      |
| SMTP                   | 80  | Postfix, Exchange, Stalwart, spam gateways (masscan called them FTP)                     |
| DVR / camera protocols | 44  | binary recorder protocols                                                                |
| sshd refusing          | 43  | `Exceeded MaxStartups` instead of a banner                                               |
| VNC                    | 39  | RFB 3.3 through 4.1, one with no authentication                                          |
| OpenVPN                | 21  | TCP mode, server hard-reset packets                                                      |
| Phone system call logs | 21  | Avaya IP Office SMDR, more on this below                                                 |

Nobody is running anything exotic on port 0. It's the same stuff that's on every other port: SSH, databases, telnet, FTP. What stands out is how much of it is embedded gear. dropbear, `micro_httpd`, NAS certificates from QNAP, Synology and Buffalo, DVRs, power switches. Some of it clearly comes from one device model rolled out across an ISP:

- **VERO S.A (Brazil)**: 145 addresses across six ASNs, all registered as "America-NET Ltda." 132 show the identical telnet `login:` prompt. The other 13 print a CLI or shell error instead.
- **ALFANET (Azerbaijan)**: 32 addresses, every one `SSH-2.0-dropbear_0.48`.
- **Great Plains Communications (US)**: 20 of the 23 "Residential Gateway" login pages.
- **INVITECH (Hungary)**: 17 FTP servers announcing `AMG1001-T10A FTP version 1.0`, with clocks set to early 2000 or 2036.
- **Telekom Srbija**: 13 identical `MySQL 4.1.22-community-nt`, an ancient Windows MySQL build.

My guess is that nobody at those ISPs picked port 0 on purpose: a port-forward or NAT rule shipped in a CPE config with the external port left at zero, and the device honored it. Then there are the hosting providers. GigaCloud (Ukraine), VDCBY (Belarus) and SiS Distribution (Thailand) have SSH on port 0 with 18, 13 and 10 different SSH version strings. Different builds means different machines, so these are individual customers, not one firmware image.

GigaCloud also accounts for 16 of the 80 mail servers on port 0, mostly Ukrainian organisations: `mail.dtek-kem.com.ua` (DTEK, the Ukrainian energy company), `smtp01.health24.ua`, `mx.eastenergy.com.ua`, `mail.citrus.com.ua`, and two government servers, `mail.depfin.dn.gov.ua` and `mail.sed-rada.gov.ua`. Why they all share this setup, I don't know. Elsewhere in mail: a Uruguayan government host, `prod-carbonium01.rionegro.gub.uy`, and `mssante.elsan.care` (62.106.147.123), a submission server for MSSanté, the French national secure health messaging system, run by the private hospital group Elsan. When masscan poked it, it replied:

```
220 mssante.elsan.care MSSPRO LPS AUTH_CLI submission server
501 2.7.0 Error: I can break rules, too. Goodbye.
```

Weird...

The rest of the router logins look the same as they would on port 80. A D-Link DSL-2740B (37.103.36.12 and two others), with "admin" already filled in as the username: two in French at Orange and Nerim in France, one in English at Wind Tre in Italy. A Linksys "Router Access" page in El Salvador (200.31.162.80). A plain "Sign in" form titled "Router Admin" on seven cloud servers: four at Hetzner, two at Online SAS, one at DigitalOcean.

![A Residential Gateway router login page served on TCP port 0](https://acid.vegas/blog/the-port-that-does-not-exist/residential-gateway.png) ![D-Link DSL-2740B router login page with the username prefilled as admin, served on TCP port 0](https://acid.vegas/blog/the-port-that-does-not-exist/dlink.png) ![Linksys Router Access login page served on TCP port 0](https://acid.vegas/blog/the-port-that-does-not-exist/linksys.png)

One didn't bother with a login at all. A 3G/4G "Wireless Mobile Router" on China Unicom (183.93.33.155) serves its full status page on port 0 to anyone who asks: its WAN IP, LAN, WAN and wireless MAC addresses, SSID, firmware build, uptime, memory usage and whether DHCP is on.

![Chinese 3G/4G Wireless Mobile Router status page showing WAN IP, MAC addresses and SSID without authentication, served on TCP port 0](https://acid.vegas/blog/the-port-that-does-not-exist/4g-router.png)

## Some Of It Runs Buildings

Browsers won't open a URL with `:0` in it, so to see the web interfaces I ran a small relay: listen on a local port, connect out to the target on port 0, pipe the bytes through, and point headless Chromium at the local port. 160 addresses speak HTTP on port 0, and 85 rendered a page.

![Gira HomeServer 4 start page with visualization, debug and HS-Monitor links, served on TCP port 0](https://acid.vegas/blog/the-port-that-does-not-exist/gira.png)

A **Gira HomeServer 4** (46.186.48.86), a KNX home and building automation controller, with its start page, debug page and monitor links sitting on port 0.

![Trane Tracer Synchrony building controller login page served on TCP port 0](https://acid.vegas/blog/the-port-that-does-not-exist/trane.png)

A **Trane Tracer Synchrony** login (201.171.202.25), which is HVAC and building control.

## Some Of It Runs Machines

The web pages were the easy part to read. The more interesting devices don't speak HTTP. They send binary the moment you connect, and figuring out what they are meant matching bytes against nmap's probe database, Wireshark dissectors and, in one case, a disassembled vendor client.

**A Beckhoff PLC with its screen open to anyone.** 46.178.29.102, on Proximus in Belgium, sends this on connect:

```
30 01 00 00 05 6d 6d 6a 56 05 00 00 00 03 00 00 ...  "CBx063" ... "CX5120" ...
```

The CX5120 is a Beckhoff embedded PC running Windows CE, the kind that runs TwinCAT and drives a machine. The protocol is CERDISP, Windows CE Remote Display, which Beckhoff's own security guideline lists as open on TCP 987 by default. I disassembled Beckhoff's client, `CERHOST.exe` 2.0.4.0. It reads a 0x130-byte info block first, which is the `30 01 00 00` at the start of the banner. Then it checks a magic word: `0x6a6d6d05` means no password, `0x6a6d6d06` means password required. This one sends `05 6d 6d 6a`. Then a 1366×768 resolution (`56 05`, `00 03`) and screen update data. CERHOST also sends mouse and keyboard input. So this is a PLC's screen that anyone can watch and operate, with no password, on port 0.

**An Omron PLC that was too busy to talk.** 194.49.59.232 (AireNetworks, Spain) answered with an Omron FINS/TCP frame:

```
46 49 4e 53 00 00 00 10 00 00 00 01 00 00 00 20 00 00 00 00 00 00 00 66
F  I  N  S  length=16   cmd=1       error=0x20               server node 0x66
```

Command 1 is the server's Node Address Data Send, and error code `0x20` is "All connections are in use." It is a FINS endpoint, the protocol used to read and write Omron PLC memory, and it turned us away only because its connection slots were full. Five days later it said the same thing. A second host, 14.49.206.64 (KT, Korea), repeats an Omron Host Link frame, `@FFF07F000000018C3D*`, over and over. The XOR of everything before `3D` is `0x3D`, so the checksum is valid, but unit `FF` is outside the normal 00–31 range. The framing is Omron. The device behind it I couldn't pin down.

**A Modbus master.** 155.93.146.109 (South Africa) doesn't wait to be polled. It polls you: `10 04 00 00 00 02 72 8a` is slave 16, Read Input Registers 0–1, with a valid CRC-16/MODBUS. Something out there is a SCADA master on port 0 asking whoever connects for two registers. Another host, 85.198.27.114 (Iran), repeats a `7e`-framed 7-byte payload with a valid Modbus CRC, which looks like a serial-to-TCP bridge.

**A power quality meter.** 166.169.193.134 on Verizon Wireless answers telnet with `SEL-735 Telnet Server`. The SEL-735 is a Schweitzer Engineering Laboratories revenue and power quality meter, the kind utilities put on substations and feeders. This one is on a cellular modem.

**Factory telemetry.** 175.202.128.103 (KT, Korea) sends an HTTP POST *at* whoever connects:

```
POST /highnet/v1/POST/MachineStatus?StationCode=20209263 HTTP/1.0
host: 52.83.191.199
User-Agent: uIP/1.0 (http://www.kq-china.net/)
Data={"StationCode":"20209263","PickCard":[{"PickCardCode":"21032380","StartTime":"2026-10-7 10:3:41","MachineStatus":"0",...
```

kq-china.net is Zhejiang Keqiang Intelligent Control, which makes monitoring systems for injection moulding machines. uIP is Adam Dunkels' tiny embedded TCP/IP stack. A collector on a factory floor is pushing per-machine status to whoever connects, and masscan, confused, filed it as Minecraft.

**A Swedish alarm.** 82.183.26.214 (Telenor, Sweden) sends an STX/ETX frame containing `"Kommunikationslarm medelvärde"`, Swedish for "communication alarm, mean value." I don't know the device.

## Alarm Panels, Time Clocks And Smart Homes

**Burglar alarm communicators.** Five hosts, three in Mexico and two in Spain, send a greeting of about 56 bytes, with a 2-byte length prefix up front. That matches the README of DSC-TLink, a reverse-engineered client for DSC's alarm communicators: "opening a TCP connection with the TL280 on port 3062. The TL280 will immediately send a packet of 56 bytes." The TL280 family is the IP/cellular communicator on DSC PowerSeries Neo alarm panels, and this is the DLS path used to program and monitor the panel. Here's 213.181.95.33:

```
00 37 01 00 00 7e 00 30 e0 05 05 05 05 00 27 03 2b 54 03 00 02 0a 89 34 04 26 21 05
05 04 41 9f 03 00 00 50 03 02 01 03 00 00 02 04 05 06 00 03 4f 62 06 0e ea 79 06 6a 7f
```

(The `0a` before the ICCID came out of the scan as `20`, because our banner storage squashed whitespace bytes into spaces. I put it back.) The CRC-16 over the 42 bytes starting at `05 05 05 00 27` (polynomial `0xD003`, from DSC-TLink's `DLSProNetHeader.cs`) comes out `0xea79`, which is the `ea 79` that follows. The MAC starts `00:03:4F`, the IEEE OUI for Sur-Gard Security, DSC's receiver brand. And the device ID field on the cellular models is the SIM card's ICCID:

| IP             | ISP            | Type   | SW    | Device ID                 | MAC               |
| -------------- | -------------- | ------ | ----- | ------------------------- | ----------------- |
| 79.112.115.9   | Digi Spain, ES | `032b` | 52/01 | ICCID 8934075700125305307 | 00:03:4F:0C:1B:D7 |
| 213.181.95.33  | Fibraworld, ES | `032b` | 54/03 | ICCID 8934042621050504419 | 00:03:4F:62:06:0E |
| 189.153.252.92 | UNINET, MX     | `032b` | 54/01 | ICCID 8952050102551818989 | 00:03:4F:60:7D:38 |
| 187.212.33.29  | UNINET, MX     | `0329` | 54/03 | (MAC, zero-padded)        | 00:03:4F:62:D7:05 |
| 201.124.95.69  | UNINET, MX     | `0329` | 50/03 | (MAC, zero-padded)        | 00:03:4F:20:24:60 |

All three ICCIDs pass the Luhn check, and their prefixes say where the SIMs are from: `89 34` is Spain, `89 52` is Mexico. According to DSC-TLink, the AES key for the rest of the session is built from data in this greeting.

**Alarm communicators that tell you the password.** Three Spanish hosts, 109.167.87.236, 85.87.152.80 and 88.3.213.39, run telnet on an Alwon E20, a Notifier/Honeywell IP/GPRS alarm communicator module:

```
Alwon E20 Telnet Input password (defaul 1234):
```

Notifier's manual agrees: telnet password, "1234 por defecto." I didn't try it.

**Time clocks.** 187.235.160.47 and one other host at UNINET in Mexico repeat `a5 00 00 00 00 7f 00 00 e0 5c`. That's an Anviz time-and-attendance frame: `a5`, a 4-byte device ID of 0, command `7f`, zero length, and a CRC-16/MCRF4XX of `0x5ce0` sent low byte first. Open-source Anviz SDKs call `0x7f` a ping.

**Home automation.** Five hosts answer `*#*1##`, the OpenWebNet ACK used by BTicino and Legrand home automation gateways: 190.113.106.159 (Costa Rica), 171.242.179.164 (Vietnam), 187.145.124.63 (Mexico), 105.157.199.125 (Morocco) and 213.7.222.134 (Cyprus). 219.241.160.119 in Korea is a `SiHAS HCM-300 Ver 2.12 >` prompt, a Shina System smart heating thermostat. And 192.119.63.143 (wilhelm.tel, Germany) is a mediola a.i.o. Gateway that sends its encrypted cloud request to whoever connects: `POST /ra/d HTTP/1.1 host: mediola-ws.elasticbeanstalk.com`.

**A debug menu.** Three hosts on SK Telecom in Korea, 42.42.150.116, 42.42.203.12 and 42.42.201.80, greet you with this:

```
##################################
# DEBUG LOGIN!!
# F/W Version : EBH0920.0
# Dev ID : ldg4839
# IP : 192.168.225.100
##################################
###### MENU DISPLAY ######
l : Log On/Off.  n : NTP Query.  d : DNS Query.  i : SysEnv Informaton.
t : Time Display.  s : Server Data Send.  h : Menu Display.
```

`192.168.225.x` is the default subnet on Qualcomm LTE modules like the Quectel EC2x, so it's probably a cellular IoT device. Which product, I don't know.

**A power strip.** 64.201.180.254 (Gatewest, Canada) runs a `Remote Power Controller v1.41i` that prints its whole outlet table, circuit, name, status and boot delay, before asking for a login. Others ask only for a password but label themselves first: `MGTN CO - Right Rack - Bottom`, `Power Switch`, `151 Front Street`.

## Remote Desktops, SAP And Cameras

**SAProuters leaking their build.** Four hosts are SAProuter, the SAP gateway that relays connections into SAP systems. They accepted the connection, waited 5 seconds for a route request that never came, and then sent back an `NI_RTERR` error packet with a lot of detail:

```
NI_RTERR ... *ERR* connection timed out -5 NI (network interface) 754 40
D:/depot/bas/754_REL/src/base/ni/nirout.cpp 8213
RTPENDLIST::timeoutPend: no route received within 5s (CONNECTED)
Tue Oct 6 10:56:27 2026 ... SAProuter 40.4 on 'ADSJ'
```

| IP             | ISP               | Kernel | Build path                            | Hostname       |
| -------------- | ----------------- | ------ | ------------------------------------- | -------------- |
| 37.61.221.36   | velia.net, DE     | 754    | `D:/depot/bas/754_REL/.../nirout.cpp` | `ADSJ`         |
| 37.61.219.146  | velia.net, DE     | 753    | `/bas/753_REL/.../nirout.cpp`         | `hsaprouter`   |
| 60.16.63.41    | China Unicom, CN  | 745    | `D:/depot/bas/745_REL/.../nirout.cpp` | `rbjsaprouter` |
| 154.236.187.38 | Etisalat Misr, EG | 753    | `/bas/753_REL/.../nirout.cpp`         | `ba-srt`       |

Every one gives away the SAProuter version (40.4), the SAP kernel release, the router's hostname, the server's local clock, and whether it's a Windows build (`D:/`) or not.

**RustDesk.** Four hosts, 93.118.140.111, 78.38.90.162 (Iran), 39.48.42.201 and 103.213.111.82 (Pakistan), open with `HJ`. That isn't a magic string. In RustDesk's `bytes_codec.rs`, the low two bits of the first byte give the header size and the rest gives the length, so `0x48` means an 18-byte message follows. `0x4a` is protobuf field 9, `Hash`, which carries a salt and a challenge:

```
48 | 4a 10 | 0a 06 "usjk79" | 12 06 "zrktdr"
len  Hash   salt             challenge
```

The salt stays the same per host. The challenge is six random characters, new on every connection. That's the password challenge a RustDesk client answers before it gets the desktop. 103.213.111.82 sends a 32-character salt, which is what newer versions use.

**Winflector.** Eight hosts run Winflector, a Polish remote application server. 193.32.180.51 sends `Terminal client version too old - please update to version 4.0.0.1` and then an RSA public key in DER form, with exponent 17, which is Crypto++'s default. The versions are sent byte-reversed: `04 03 00 04` is 4.0.3.4. All six versions I saw are on Winflector's own version list. The hosts are in Cape Verde (156.238.40.243, 156.238.47.54 and one more), Malaysia (202.187.2.203 and one more), Poland (193.32.180.51), Ghana (80.87.66.51) and India (49.204.146.227).

**VNC with no password.** Of the 39 VNC servers, one doesn't ask for anything. 107.174.67.223 answers with security type none and a desktop name of `resolv.moe, meow meow meow :3 haiiii!!`, and an 850×591 framebuffer. Four more just send `RFB 005.000`.

**Phone switches.** 117.236.74.58 (BSNL, India) is `Asterisk Call Manager/2.10.5`, the Asterisk Manager Interface from Asterisk 13.26–13.34. Once logged in, AMI can originate calls and run console commands. 38.229.159.25 (France) and 41.60.171.20 (Liquid) send `04 00 fb ff LAPK`, the 3CX tunnel protocol.

**Cameras and NAS.** exacqVision video surveillance servers on 8 hosts, including two at OneCleveland (204.156.216.130 and 204.156.198.238), send a 56-byte header ending in three values that change on every connection. Six QNAP NAS boxes expose RTRR (Realtime Remote Replication, `ab ca a5 5d ...`), on 202.184.138.61 in Malaysia and five others in Austria, Sri Lanka, Spain and Italy. Two Rifatron DVRs send `RIFA`, 42.200.149.78 in Hong Kong and 14.241.231.170 in Vietnam. One more is a UMS WebViewer DVR stream in Japan (123.242.213.10).

**The rest of the RPC zoo.** 223.76.20.55 (China Mobile) is ZeroC Ice, `IceP` 1.0 Validate Connection. 191.13.74.178 (Telefonica, Brazil) sends just `ac ed 00 05`, the header of a Java serialization stream, and then nothing else. 119.152.239.20 (Pakistan) sends a bare HTTP/2 SETTINGS frame with the same bytes as a default grpc-go server. 212.80.175.132 (Spain) has things backwards and sends *us* a full TLS 1.2 ClientHello. And 116.75.83.213 (Hathway, India) responds with `RP-Version: 5.0` and `RP-Application-Reason: 80108bff`, which is Sony PlayStation Remote Play, most likely a PS4.

## The Phone System That Reads You Its Call Log

21 addresses don't wait to be asked. Connect to port 0 and they start streaming comma-separated text, about 7.5 KB of it per host in our capture:

```
2026/09/23 14:31:49,00:00:11,0,24,O,<number>,<number>,,0,1002639,0,E24,<name>,T9031,Line 31.1,0,0,...
```

That's SMDR (Station Message Detail Recording) output from an Avaya IP Office phone system. The fields line up with Avaya's documented layout: call start, connected time, ring time, caller, direction, called and dialled number, a call ID that starts at 1,000,000, then the two parties as `E` for extension or `T` for trunk, with names like "Line 31.1". Each host handed over 37 to 62 calls with real phone numbers, staff names, extension numbers and internal addresses. The oldest record is from December 2018. The newest is from a few days before the scan. Several hosts start with records that are years old, which I can't explain: 94.30.72.110 opened with calls from 2018-12-21, 31.10.39.182 with 2023-09-18.

Nothing was sent to get this. IP Office pushes SMDR records to whatever connects to its SMDR port, and on these 21 hosts that port is reachable on port 0. masscan stops reading at about 7.5 KB, so that's only the first few dozen calls per host.

| IP              | ISP              | Records | Record dates            |
| --------------- | ---------------- | ------- | ----------------------- |
| 109.170.131.14  | Wavenet, GB      | 41      | 2026-09-03 → 09-04      |
| 140.141.189.204 | i3 Broadband, US | 46      | 2025-10-24 → 10-30      |
| 149.40.120.214  | ITS-TG, GB       | 42      | 2026-09-22              |
| 164.39.146.134  | Gamma, GB        | 62      | 2026-09-24 → 09-25      |
| 164.39.207.103  | Gamma, GB        | 54      | 2026-03-27 → 03-31      |
| 176.35.140.144  | Wavenet, GB      | 62      | 2025-09-11 → 2026-07-22 |
| 176.35.24.241   | Wavenet, GB      | 60      | 2026-09-16 → 09-18      |
| 194.75.212.229  | BT, GB           | 42      | 2026-09-23 → 09-24      |
| 195.21.76.203   | ITS-TG, GB       | 60      | 2026-09-17 → 09-21      |
| 196.40.184.138  | XISASN, ZA       | 61      | 2024-03-27 → 03-28      |
| 212.140.248.34  | BT, GB           | 49      | 2024-10-10              |
| 213.218.204.214 | Gamma, GB        | 58      | 2026-09-25              |
| 31.10.39.182    | ULGRP, GB        | 55      | 2023-09-18 → 09-19      |
| 31.121.1.250    | BT, GB           | 41      | 2024-02-08              |
| 37.235.126.101  | Lightspeed, GB   | 44      | 2026-09-16 → 09-17      |
| 81.130.159.34   | BT, GB           | 41      | 2026-10-01              |
| 81.174.163.44   | Plusnet, GB      | 62      | 2026-09-27 → 09-28      |
| 87.127.54.146   | Entanet, GB      | 59      | 2026-06-17              |
| 87.242.249.105  | TalkTalk, GB     | 42      | 2026-09-02 → 09-07      |
| 87.74.60.78     | CW, GB           | 48      | 2026-09-28              |
| 94.30.72.110    | Wavenet, GB      | 37      | 2018-12-21              |

19 of the 21 are in the UK, on BT, Wavenet, Gamma, TalkTalk, Plusnet and a handful of smaller business ISPs. One is in the US and one in South Africa. However these ended up on port 0, anyone who connects can read the company's call history.

## What I Take From This

- **Port 0 is a sockets-API concept more than a network one.** On the wire it's just a number. The idea that it "doesn't exist" comes from `bind()`, and a single NAT rule gets around it.
- **Tools are worse at this than networks are.** `nc` and `telnet` refuse to try, Chromium blocks it, and the Linux kernel's own `getpeername()` reports a working port 0 connection as not connected. A lot of software assumes port 0 means "nothing is here."
- **Most of what is on port 0 looks accidental.** The same banner across a whole ISP, CPE login pages, one dropbear version on every box at an ISP: that reads like a port-forward rule with the external port left at zero, not anyone choosing port 0. I haven't proven it.
- **Port 0 is not obscurity.** A Beckhoff PLC screen with no password, an Omron FINS endpoint, a Modbus master, DSC alarm communicators, alarm modules that print their default password, SAProuters and RustDesk boxes. All of it was found by a single SYN scan.
- **The worst thing on port 0 isn't a login page.** It's 21 phone systems reading out their call history, names and numbers included, to anyone who connects.

Come say hi on the port that doesn't exist. If your client lets you.

\- - -

Sources & References:

- [RFC 9293](https://www.rfc-editor.org/rfc/rfc9293), Transmission Control Protocol (TCP)
- [RFC 6335](https://www.rfc-editor.org/rfc/rfc6335), IANA Procedures for the Management of the Service Name and Transport Protocol Port Number Registry
- [RFC 6056](https://www.rfc-editor.org/rfc/rfc6056), Recommendations for Transport-Protocol Port Randomization
- [RFC 768](https://www.rfc-editor.org/rfc/rfc768), User Datagram Protocol
- [mubix/lmo](https://github.com/mubix/lmo), LetMeOutOfYour.net egress testing resources
- [Service Name and Transport Protocol Port Number Registry](https://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml), IANA
- [net/ipv4/inet\_connection\_sock.c](https://github.com/torvalds/linux/blob/v6.8/net/ipv4/inet_connection_sock.c), [inet\_hashtables.c](https://github.com/torvalds/linux/blob/v6.8/net/ipv4/inet_hashtables.c), [af\_inet.c](https://github.com/torvalds/linux/blob/v6.8/net/ipv4/af_inet.c), Linux v6.8
- [ip(7)](https://man7.org/linux/man-pages/man7/ip.7.html), Linux man-pages
- [masscan](https://github.com/robertdavidgraham/masscan), Robert Graham
- [iptoasn.com](https://iptoasn.com), IP to ASN data
- [IP Office SMDR Output Fields](https://support.avaya.com/elmodocs2/ip_office/DOCS3_0/DATA/Additional/mergedProjects/smdr/html/ipofficesmdroutputfields.htm), Avaya
- [ssl\_pkt.h](https://github.com/OpenVPN/openvpn/blob/master/src/openvpn/ssl_pkt.h), OpenVPN
- [nmap-service-probes](https://github.com/nmap/nmap/blob/master/nmap-service-probes), Nmap
- [packet-omron-fins.c](https://github.com/wireshark/wireshark/blob/master/epan/dissectors/packet-omron-fins.c), [packet-saprouter.c](https://github.com/wireshark/wireshark/blob/master/epan/dissectors/packet-saprouter.c), Wireshark
- [NeoHub / DSC-TLink](https://github.com/BrianHumlicek/NeoHub), Brian Humlicek
- [RustDesk](https://github.com/rustdesk/rustdesk), `bytes_codec.rs`, `message.proto`, `connection.rs`
- [MxLabs/Anviz](https://github.com/MxLabs/Anviz), Anviz SDK
- [ghostport](https://github.com/vxfemboy/ghostport)
- [Winflector](https://www.winflector.com/), OTC S.A.

Connect to SuperNETs on port 0 (plaintext) or port 1 (TLS) at irc.supernets.org, or reach out at acid.vegas@acid.vegas. We hang out in `#war`.
