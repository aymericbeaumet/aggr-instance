---
title: Getting debug access on a Brother printer over Telnet
link: https://inventor7777.github.io/blog/2026/10/22/debug-access-on-brother/
source: hnrss-org
published: 2026-10-26T13:54:39Z
updated: 2026-10-26T13:54:39Z
first_seen: 2026-10-29T15:30:03.869733718Z
authors:
- inventor7777
content: extracted
html: 2026-10-26-getting-debug-access-on-a-brother-printer-over-telnet.html
remote_preview:
  url: https://inventor7777.github.io/donut.png
---

As it turns out, the Brother NC-8700w Command Console is more interesting than it first appears.

## Some context

I’ve always enjoyed seeing how things work. It started with hardware and trying to take interesting devices apart, and now I do the same with software. And as I’ve learned how software works, one of the goals I like is accessing hidden features. On Linux based devices, that’s getting a root Linux shell. But what about the printer?

## The unsuspecting subject

Is the family Brother MFC-L8900CDW. Unlike the reputations of certain other printer brands and models, this one connects and prints every time. mDNS doesn’t randomly stop working, the paper doesn’t randomly jam, and the toner lasts forever. However, the vendor CLI is limited when it comes to deep system information. *(one of these functions is not like the other)*

## The journey begins

The first step to figuring things out in this day and age is getting a firmware binary and then using AI to attack it from all angles. *(Some people are, of course, skilled enough to do this themselves, but I am not one of those people.)* On some devices you can straight up download a firmware image, but in Brother’s case, they expect you to use their own downloader. My AI agent first tried just going to the link found in the Brother firmware updater app, but their server rejected a guessed XML request. So after digging a bit deeper into the firmware update tool, we found the list of models and stuff that it accepts. While writing this, I also discovered [an interesting tool](https://github.com/CauldronDevelopmentLLC/oh-brother) that might have saved me some reversing trouble had I known about it, but the Brother app had enough info to work it out manually. While Brother could change the format at any time, this is the request that worked for my printer.

```bash
curl -sS \
  'https://firmverup.brother.co.jp/kne_bh7_update_nt_ssl/ifax2.asmx/fileUpdate' \
  -H 'Content-Type: text/xml' \
  --data-binary @- <<'XML'
<REQUESTINFO>
  <FIRMUPDATETOOLINFO>
    <FIRMCATEGORY>MAIN</FIRMCATEGORY>
    <OS>MAC</OS>
    <INSPECTMODE>0</INSPECTMODE>
  </FIRMUPDATETOOLINFO>
  <FIRMUPDATEINFO>
    <MODELINFO>
      <NAME>MFC-L8900CDW series</NAME>
      <SPEC>0001</SPEC>
      <DRIVER>EWS</DRIVER>
      <FIRMINFO>
        <FIRM>
          <ID>MAIN</ID>
          <VERSION>ZU2605160228</VERSION>
        </FIRM>
      </FIRMINFO>
    </MODELINFO>
    <DRIVERCNT>1</DRIVERCNT>
    <LOGNO>2</LOGNO>
    <NEEDRESPONSE>1</NEEDRESPONSE>
  </FIRMUPDATEINFO>
</REQUESTINFO>
XML
```

You can replace the firmware version with `B0000000000` instead, and that also works.

In response, Brother’s API returned:

```plaintext
<RESPONSEINFO>
  <FIRMUPDATEINFO>
    <VERSIONCHECK>0</VERSIONCHECK>
    <MEMORYVERSION>a</MEMORYVERSION>
    <FIRMID>MAIN</FIRMID>
    <LATESTVERSION>ZV2608181949</LATESTVERSION>
    <PATH>http://update-akamai.brother.co.jp/CS/D009UD_ZV.djf</PATH>
    <DLTIME>175000</DLTIME>
  </FIRMUPDATEINFO>
</RESPONSEINFO>
```

Some very descriptive versions, I must say…

## The firmware has landed

Now that the firmware was in hand, GPT-6.1 Sol got to work.

Here is what it found, in its own words:

```ai
Conclusion
The main application firmware uses the TRON-family T-Kernel real-time
operating system on 32-bit little-endian ARM. Confidence: high. Exact
T-Kernel release, CPU core, and SoC have not been established.
This is positive T-Kernel evidence, not just an absence of Linux strings.

Evidence in extracted/ZlibRo.bin (hex offsets relative to that file)
0x004b0168: T-Kernel/OS SVC in execution
0x019c0450: Software Interrupt (SWI) #6 (T-Kernel/OS & Extended SVC)
0x019c050f: Software Interrupt (SWI) #9 (T-Kernel/DS)
0x019c7d7b: OS_Ver
0x019c7d82: TSysName
0x019c7d8b: T-Kernel
0x019c7d94: TMaxTskId (followed by semaphore/mutex/etc. configuration names)
0x0002a624: TASK STACK INFORMATION for ITRON
0x002b3014: MFC-L8900CDW series (also at 0x00c40f4c and 0x00c4148c)

Evidence in extracted/KernelRo.bin
0x0000e188: T-Monitor Initialize
0x0000e8a5: TM> prompt
0x00009154: Undefined Instruction (ARM)
0x00009170: Undefined Instruction (THUMB)
ARM exception registers R0-R15/CPSR and ARM processor modes are present.
The opening words decode as ARM little-endian instructions, including
SUB lr, lr, #4 (0xe24ee004) and PUSH {r3,r12,lr} (0xe92d5008).
The RTOS kernel is a separate 141,932-byte section loaded at 0x40100000.

Storage and libraries: embedded identification strings, not a security audit
ZlibRo.bin 0x010050e4: Datalight Reliance Nitro v3.0.6 Build 2306EC
ZlibRo.bin 0x003302e4: RTOS Edition for arm; compiled Dec 21 2015
ZlibRo.bin 0x010d6b50: Datalight FlashFX Tera v3.1 Build 2772E
ZlibRo.bin 0x00f391e0: Lua 5.2.3 version identification
ZlibRo.bin 0x008ddaa4: OpenSSL 1.0.1c; other sections identify OpenSSL 1.0.2
ZlibRo.bin 0x005377b8: libpng version 1.6.43
ZlibRo.bin 0x00692ec8: gSOAP/2.7
ZlibRo.bin 0x019bb300: wpa_supplicant v0.5.7-Devicescape
Version labels can survive vendor backports; they do not establish
which vulnerabilities are present or that every bundled module is active.

Flash layout and extraction
The update wrapper starts with PJL EXECUTE BRDOWNLOAD / PROGRAM=FIRM.
This package maps file offset 0x93 to flash address 0xe0060000.
A table of 20-byte labels plus 32-bit little-endian values begins at
package offset 0x8497. It names the kernel, page table, and zlib sections.
The main compressed stream starts at package offset 0x2b2e7, after its
4-byte length. Three streams are stored sequentially. Each stream passed
zlib checksum/end checks and exactly matches the table's expanded size.
The RW source addresses describe their installed flash destinations;
the update package packs those streams immediately after the RO stream.

Output                    Bytes          Load address
PageTable.bin             33,792         0xe0060000
KernelRo.bin             141,932         0x40100000
ZlibRo.bin            28,186,916         0x40222b00
ZlibRw.bin                   700         0x41e00d00
Zlib2Rw.bin            1,526,396         0x43cb8f00

Binwalk found embedded images and other assets in the expanded main
section, but no recognizable Linux filesystem/kernel image. This does
not prove that no separate chip runs other firmware.

Hardware
Brother's model datasheet specifies an 800 MHz main processor, 133 MHz
subprocessor, and 512 MB memory (regional model variations are possible).
The image contains SubCPU/print-engine control messages. This is consistent
with an RTOS application coordinating printing, scanning, UI, networking,
and a separate engine controller. We have not analyzed SUB1 1.35.
Generic CPU vendor names in diagnostic tables do NOT identify the SoC.
The kernel has a T-Monitor debugger/monitor, but that does not establish
that it is accessible through the network telnet port. The firmware also
contains Brother network-configuration telnet commands. A telnet service
is not by itself evidence of a Unix shell.

Sources and comparison material
TRON Forum: T-Kernel and T-Monitor roles
https://www.tron.org/tron-project/what-is-t-kernel/t-kernel/
TRON Forum: official source downloads
https://www.tron.org/source-code-download/
Brother model datasheet (processor/subprocessor/memory)
https://www.brother.lt/-/media/product-downloads/devices/printers/mfc/mfcl8900cdw/mfc-l8900cdw.ashx
STAR Labs: independently inspected a different Brother model using
T-Kernel and similar named/compressed segments. Used as a format lead;
our OS conclusion is based on this package's own extracted evidence.
https://starlabs.sg/blog/2025/11-breaking-into-a-brother-mfc-j1010dw-three-security-flaws-in-a-seemingly-innocent-printer/

...

Configuration records in ZlibRo.bin are 20 bytes each:
  integer value; name pointer; type; count; pointer to own value.
Records for TmConsEnable/TmDebugPort/TmBaudRate have values 0/0/115200.
Kernel code at 0x40109344 reads TmConsEnable and at 0x40109360-0x40109368
skips initial serial-console initialization when that value is zero.
These are compiled defaults, not measurements of current RAM settings.
Separate application records ConsDrvEnable=1 and DEBUGPORT=2 exist;
these names alone do not establish UART pin numbers or a remote route.

T-Monitor character I/O accesses memory-mapped hardware at 0xd0c00000:
  0x4010e058: output character routine
  0x4010e0d0: serial initialization, writes T-Monitor Initialize
  0x4010e1a0: input character routine
  0x4010e7c4 and 0x4010e8c0: interactive loops with TM> prompt
Input/output can lazily initialize the serial block even when initial
console initialization was skipped, so TmConsEnable=0 does not prove
that later monitor entry is impossible.

At 0x4010e280 the command dispatcher loads table address 0x41e00e18.
It walks 12-byte records (name, alias, handler), stopping at a null name.
The initial table is in extracted/ZlibRw.bin at offset 0x118:
  Help, H, handler 0x4010e318
  null, null, handler 0x4010e1f8
The terminator's handler prints Unsupported command. No full memory,
register, breakpoint, or step-command suite is present in this initial
table. This narrows the generic T-Monitor description from earlier.

Separate Brother debug console
The application contains telnetddebug and /telnetdebug_q identifiers,
Enter debug user mode text, and help strings for debug symbols, task
stacks, heap usage, uptime, and console output redirection. Their presence
is a lead; it does not establish successful access or T-Monitor bridging.
```

If you skipped past that, I can’t blame you. It could be interesting to some folks, though, which is why I included it.

## Printer’s tech stack

The printer runs **T-Kernel, a TRON-family real-time OS**.

| Component          | Info                                                                 |
| ------------------ | -------------------------------------------------------------------- |
| CPU architecture   | 32-bit ARM, little-endian                                            |
| Storage filesystem | Datalight Reliance Nitro 3.0.6                                       |
| Flash management   | Datalight FlashFX Tera 3.1                                           |
| Scripting          | Lua 5.2.3                                                            |
| Other libraries    | OpenSSL, gSOAP, libpng, wpa\_supplicant                              |
| Firmware structure | Separate kernel, compressed application code, embedded web/UI assets |

Brother specifies an **800 MHz main processor, 133 MHz subprocessor, and 512 MB RAM**. The firmware contains messages for communicating with that subprocessor and controlling the print engine. [Brother datasheet](https://www.brother.lt/-/media/product-downloads/devices/printers/mfc/mfcl8900cdw/mfc-l8900cdw.ashx)

## The interesting part: getting debug access without modifying anything

This is probably the part you came for!

If you have telnet enabled on your printer (easy to enable via the web UI), it is very easy to access the three debugging modes.

> Use these at your own risk! You can seriously mess up the system if you modify the parameters incorrectly.

Log into the command console using your admin password and just hit enter when it asks for username. Once you’re at the `Local>` prompt, simply type `SET USER 640184.571119`

```plaintext
inventor7777@Mac-Studio - 20:17 ~ % telnet 192.168.50.16
Trying 192.168.50.16...
Connected to 192.168.50.16.
Escape character is '^]'.

Welcome. Type <return>, enter password at # prompt

# 
Brother NC-8700w Command Console
 Copyright(C) 2000-2017 Brother Industries, Ltd.
 KASAGO TCP/IP Copyright (c) 2011 ZUKEN ELMIC, Inc.

 Interface 1 : NC-9200h, Ethernet 10/100/1000BASE-T      (Inactive)
 Interface 2 : NC-8700w, IEEE 802.11b/g/n                  (Active)
 Interface 2 is selected for configuration

 Type HELP for command list



Enter username> 
Local> SET USER 640184.571119

Enter debug user mode.

Local> HELP

EXIT/^D                  Exit program
HElp                     Information on available commands
INitialize               Reset unit
CLear                    Remove configuration item
SET                      Modify unit parameters
SHow                     Display unit parameters
ZEro                     Zero statistical counts
DEbug                                Execute debug functions
REDirect                             Redirect printf() output
WATch [<addr>[ <intvl/cond>[ <unit>]]] Watch memory function
SNTPDEBUG                SNTP for debug functions

Type 'HElp <cmd>' for more information
```

The firmware also contains **wireless QA** and **wireless development** access modes, which I haven’t looked deeply into. Wireless QA is accessible via `SET USER 031227`, and wireless development is available via `SET USER 031227.040119`

I haven’t played around with the debug console *(or the CLI in general, for that matter)* very much, but the firmware reveals a TON of commands. Instead of making this post crazy long, my AI compiled [this HTML of every command available](https://inventor7777.github.io/brother-mfc-commands-list.html). I haven’t tested them all, so take it with a grain of salt, but GPT-6.1 Sol is actually pretty smart!

Again, I did *not* extensively test any of these commands as I prefer to have a working printer, but it’s pretty interesting. I did check the heap using `DEBUG SHOW HEAP`, and it really does give a full list of all running tasks and how much heap they use. Neat!

If this post didn’t satisfy your hunger for Brother printer reverse engineering, another interesting read is [this Star Labs Brother investigation](https://starlabs.sg/blog/2025/11-breaking-into-a-brother-mfc-j1010dw-three-security-flaws-in-a-seemingly-innocent-printer/), where they dive a bit deeper into how these printers actually work.
