---
title: SDCC – Small Device C Compiler
link: https://sdcc.sourceforge.net/
source: hnrss-org-frontpage
published: 2026-09-19T02:32:36Z
updated: 2026-09-19T02:32:36Z
first_seen: 2026-09-19T13:22:28.237536491Z
authors:
- lioeters
summary: 'Article URL: https://sdcc.sourceforge.net/ Comments URL: https://news.ycombinator.com/item?id=49762744 Points: 100 # Comments: 20'
content: extracted
html: 2026-09-19-sdcc-small-device-c-compiler.html
preview:
  file: 2026-09-19-sdcc-small-device-c-compiler.preview-070aa961cffe.webp
  width: 88
  height: 31
  alt: Valid XHTML 1.0 Transitional
  color: '#d3b98d'
images:
- source: http://www.w3.org/Icons/valid-xhtml10
  original:
    file: 2026-09-19-sdcc-small-device-c-compiler.image-ebc656c16e05.png
    width: 88
    height: 31
  color: '#fecb65'
---

## What is SDCC?

**SDCC** is a ***retargettable, optimizing Standard C (ANSI C89, ISO C99, ISO C11, ISO C23) compiler suite*** that targets the ***Intel MCS51*** based microprocessors ***(8031, 8032, 8051, 8052, etc.)***, ***Maxim*** (formerly ***Dallas***) ***DS80C390*** variants, ***Freescale*** (formerly ***Motorola***) ***HC08*** based ***(hc08, s08)***, ***Zilog Z80*** based MCUs ***(Z80, Z80N, Z180, SM83, Rabbit 2000, 2000A, 3000A, 4000, SM83, TLCS-90, eZ80, R800)***, ***Padauk (pdk14, pdk15)***, ***STMicroelectronics STM8***, ***MOS 6502*** and ***WDC 65C02***. Work is in progress on supporting the ***Rabbit 5000, 6000***, ***Padauk pdk13*** and the ***f8*** and ***f8l*** targets; ***Microchip PIC16*** and ***PIC18*** targets are unmaintained. SDCC can be retargeted for other microprocessors.

SDCC suite is a collection of several components derived from different sources with different FOSS licenses. SDCC compiler suite include:

- **sdas** and **sdld**, a *retargettable assembler and linker*, based on **ASXXXX**, written by Alan Baldwin; (GPL).
- **sdcpp** *preprocessor*, based on **GCC cpp**; (GPL).
- **ucsim** *simulators*, originally written by Daniel Drotos; (GPL).
- **sdcdb** *source level debugger*, originally written by Sandeep Dutta; (GPL).
- **sdbinutils** *library archive utilities*, including sdar, sdranlib and sdnm, derived from GNU Binutils; (GPL)
- **SDCC run-time libraries**; (GPL+LE). Pic device libraries and header files are derived from Microchip header (.inc) and linker script (.lkr) files. Microchip requires that "The header files should state that they are only to be used with authentic Microchip devices" which would make them incompatible with the GPL.
- **gcc-test** *regression tests*, derived from **gcc-testsuite**; (no license explicitely specified, but since it is a part of GCC is probably GPL licensed)
- **packihx**; (public domain)
- **makebin**; (zlib/libpng License)
- **sdcc** *C compiler*, originally written by Sandeep Dutta; (GPL). Some of the features include:
  - extensive MCU specific language extensions, allowing effective use of the underlying hardware.
  - a host of standard optimizations such as *global subexpression elimination, loop optimizations (loop invariant, strength reduction of induction variables and loop reversing), constant folding* and *propagation, copy propagation, dead code elimination and jump tables for 'switch' statements.*
  - MCU specific optimizations, including a global register allocator.
  - adaptable MCU specific backend that should be well suited for other 8 bit MCUs
  - independent rule based peep hole optimizer.
  - a full range of data types: **char** (*8* bits, 1 byte), **short** (*16* bits, 2 bytes), **int** (*16* bits, 2 bytes), **long** (*32* bit, 4 bytes), **long long** (*64* bit, 8 bytes), **float** (4 byte IEEE), **\_Bool**/**bool** and **\_BitInt**.
  - the ability to add inline assembler code anywhere in a function.
  - the ability to report on the complexity of a function to help decide what should be re-written in assembler.
  - a good selection of automated regression tests.

**SDCC** was originally written by Sandeep Dutta and released under a **GPL** license. Since its initial release there have been numerous bug fixes and improvements. As of December 1999, the code was moved to SourceForge where all the "users turned developers" can access the same source tree. SDCC is constantly being updated with all the users' and developers' input.

## News

***2026-06-22: SDCC 4.6.0 released.***

A new release of SDCC, the portable optimizing compiler for STM8, MCS-51, DS390, HC08, S08, Z80, Z180, Rabbit, R800, SM83, eZ80 in Z80 mode, Z80N, TLCS-90, MOS 6502, WDC 65C02, Padauk and PIC microprocessors is now available. ([http://sdcc.sourceforge.net](http://sdcc.sourceforge.net)). Sources, documentation and binaries for GNU/Linux amd64, Windows x86 and amd64, macOS amd64 are available.

SDCC 4.6.0 New Feature List:

- C2y \_Countof operator
- C2y octal
- C2y if-declaration
- C2y Conditional operator with omitted second operand (originally a GNU extension)
- C99 compound literals
- C23 compound literals with storage class specifiers
- Experimental f8l port
- C2y signed bit-precise integer type of width 1
- C2y bit-precise integer types as fixed underlying type for enum
- C2y umaxabs
- C2y bit utilities
- realloc(ptr, 0) now follows C99 semantics instead of C90 semantics
- r4k port for Rabbit 4000
- Experimental r5k and r6k ports for Rabbit 5000 and 6000
- Support for Dynamic C calling convention in z80-related ports
- Substantially improved code generation for z80-related ports
- ez80 port replaces ez80\_z80 port
- C2y functions uabs, ulabs, ullabs
- Improved diagnostics on invalid C2y (some of which was UB up to C23)
- C23 constexpr (mostly)
- C2y containerof macro
- Diagnostics based on \[static assignment-expression\] array parameter syntax
- Warnings for array parameters where accesses fall outside the bounds
- Basic parameter forward declarations (GNU extension)
- C23 va\_start and variadic functions
- \_Optional qualifier
- Plain int bit-fields are signed
- strsep
- TLCS-870C(1) support in uCsim
- \_\_far support in Rabbit ports for using a 1 MB address space for data

***2026-06-14: SDCC 4.6.0 RC2 released.***

SDCC 4.6.0 Release Candidate 2 source, doc and binary packages for amd64 GNU/Linux, amd64 Windows, and amd64 macOS are available in corresponding folders at: [http://sourceforge.net/projects/sdcc/files/](http://sourceforge.net/projects/sdcc/files/).

***2026-05-28: SDCC 4.6.0 RC1 released.***

SDCC 4.6.0 Release Candidate 1 source, doc and binary packages for amd64 GNU/Linux, amd64 Windows, and amd64 macOS are available in corresponding folders at: [http://sourceforge.net/projects/sdcc/files/](http://sourceforge.net/projects/sdcc/files/).

***2025-10-15: SDCC got funding.***

SDCC is primarily developed by unpaid volunteer work; though once in a while there was some outside support, in particular by university employees being allowed to work on SDCC a bit during paid time, and SDCC developers receiving hardware samples from microcontroller vendors.\
 However, sometimes the limitations of this are felt. In particular when I've had a few free hours to work on SDCC, started working on a feature or bug, but was not able to finish the work during the time I had, or simply was not able to even fully track down the cause of the bug. And when it took a long time until I could work again on that feature or bug, it took extra time or effort to get into it again. Sometimes I would instead start work on another aspect of SDCC instead. Having funding available for working on SDCC is IMO really helpful in these situations - instead of having to stop work on SDCC to go back to other paid work, I can just keep working on the feature or bug, since this then is paid work. SDCC developers have been applying for funding for SDCC projects, and we are happy to announce that two important such applications succeeded recently.

The [NGI0 Commons Fund](https://nlnet.nl/commonsfund/) donates to [improve SDCC support for various target hardware, as well as implement machine-independent improvements to make SDCC more competitive vs. non-free compilers](https://sourceforge.net/p/sdcc/wiki/NGI0-Commons-SDCC). Hardware-specific improvements planned include improving support for Padauk's popular low-cost microcontrollers, improving support for the Rabbit microcontrollers common in older IoT devices, and improving support for Toshiba TLCS microcontrollers. The focus for machine-independent improvements will be in enhancing support for recent ISO C standards, an optimization to reduce memory usage for local variables, and implementing a link-time optimization to optimize out unused functions and objects. The latter is the one feature most-requested by SDCC users in recent years. This project in done jointly by five SDCC developers.

The [Sovereign Tech Fund](https://www.sovereign.tech/programs/fund/) comissioned work on [improving SDCC for safety and security of embedded firmware](https://sourceforge.net/p/sdcc/wiki/STF-SDCC). We will improve support for aspects of modern C standards and dialects relevant to safety and security, get SDCC ready for post-quantum cryptography, work on mitigations for potential side-channel attacks and improve the reliability of SDCC via extended testing also covering less-commonly used command-line parameter combinations. This project is done by one SDCC developer.

We can imagine all this coming together e.g. when writing firmware for an IoT device based on an eZ80 or Rabbit 4000 SoC. The SDCC user writing this firmware will benefit from the improved support for the target architecture, modern C features for efficiency and convenience, general high level optimizations (all part of the NGI0 Commons project), modern C features relevant for safety and security, to help avoid bugs in the user-written code, efficient side-channel-free code generated for modern cryptography algorithms (all part of the STF project). And thanks to improved testing and fixed compiler bugs, the firmware will compile and work very reliably (depending on the details part of the STF or the NGI0 project).

***January 28th, 2025: SDCC 4.5.0 released.***

A new release of SDCC, the portable optimizing compiler for STM8, MCS-51, DS390, HC08, S08, Z80, Z180, Rabbit, R800, SM83, eZ80 in Z80 mode, Z80N, TLCS-90, MOS 6502, WDC 65C02, Padauk and PIC microprocessors is now available. ([http://sdcc.sourceforge.net](http://sdcc.sourceforge.net)). Sources, documentation and binaries for GNU/Linux amd64, Windows x86 and amd64, macOS amd64 are available.

SDCC 4.5.0 New Feature List:

- Full atomic\_flag support for msc51 and ds390 ports
- Experimental f8 port
- ISO C2y case range expressions
- ISO C2y \_Generic selection expression with a type operand
- K&R-style function syntax (preliminarily with the semantics of non-K&R ISO-style functions)
- ISO C23 enums with user-specified underlying type
- struct / union in initializers

[Previous News](https://sdcc.sourceforge.net/previous.php)

## What Platforms are Supported?

**GNU/Linux on amd64**, **GNU/Linux on x86**, **Microsoft Windows on amd64**, and **macOS on amd64** are the primary, so called "officially supported" platforms.

**SDCC** compiles natively on **GNU/Linux** and **macOS** using [gcc](http://www.gnu.org). **Windows** release and snapshot builds are made by **cross compiling to mingw32** on a Linux host.

SDCC is known to also work on at least GNU/Linux on aarch64, GNU/Linux on ppc64, FreeBSD on aarch64.

**Windows** users can also try Cygwin ([https://www.cygwin.com/](https://www.cygwin.com/)) or may try the unsupported Microsoft Visual C++ build scripts.

## Downloading SDCC

See the [Sourceforge download page](https://sourceforge.net/projects/sdcc/files/) for the last released version including source and binary packages for **Linux - amd64**, **Microsoft Windows - x86**, **Microsoft Windows - amd64** and **Mac OS X - ppc and amd64**.

Major Linux distributions take care of SDCC installation packages themselves and you will find SDCC in their repositories. Unfortunately SDCC packages included in Linux disributions are often outdated. In this case users are encouraged to compile the latest official SDCC release or a recent snapshot build by themselves or download the pre-compiled binaries from [Sourceforge download page](https://sourceforge.net/projects/sdcc/files/).

In addition, SDCC should compile on any modern Unix-like OS; the following are included in automated regression testing, like the release packages:

- Linux - x86
- FreeBSD - aarch64

SDCC is always under active development. Please consider [downloading one of the snapshot builds](https://sdcc.sourceforge.net/snap.php) if you have run across a bug, or if the above release is more than two months old.

The latest development source code can be accessed using Subversion. The following will fetch the latest sources:

`svn checkout svn://svn.code.sf.net/p/sdcc/code/trunk/sdcc sdcc`

... will create the *sdcc* directory in your current directory and place all downloaded code there. You can browse the Subversion repository [here](https://sourceforge.net/p/sdcc/code/HEAD/tree/trunk/sdcc/).

Before reporting a bug, please check your SDCC version and build date using the -v option, and be sure to include the full version string in your bug report. For example:

`sdcc/bin > sdcc -v\

              SDCC : mcs51/gbz80/z80/avr/ds390/pic14/TININative/xa51 2.3.8 (Feb 10 2004) (UNIX)`

## Support for SDCC

**SDCC** and the included support packages come with fair amounts of documentation and examples. When they aren't enough, you can find help in the places listed below. Here is a short check list of tips to greatly improve your chances of obtaining a helpful response.

1. Attach the code you are compiling with SDCC. It should compile "out of the box". Snippets must compile and must include any required header files, etc. Incomplete information will hamper your chance of a timely response.
2. Specify the exact command you use to run SDCC, or attach your Makefile.
3. Specify the SDCC version (type "sdcc -v"), your platform and operating system.
4. Provide an exact copy of any error message or incorrect output.

**Please attempt to include these 4 important parts**, as applicable, in all requests for support or when reporting any problems or bugs with SDCC. Though this will make your message lengthy, it will greatly improve your chance that SDCC users and developers will be able to help you. Some SDCC developers are frustrated by bug reports without code provided that they can use to reproduce and ultimately fix the problem, so please be sure to provide sample code if you are reporting a bug!

- [Web Page](http://sdcc.sourceforge.net) - you are (X) here.
- Mailing list: \[use "BUG REPORTING" below if you believe you have found a bug.\]
  - Send to the developer list \<sdcc-devel.AT.lists.sourceforge.net> - for development work on SDCC
  - Send to the user list \<sdcc-user.AT.lists.sourceforge.net> - \[preferred\] all developers and all users.
  - [Subscribe to the user list](http://lists.sourceforge.net/mailman/listinfo/sdcc-user)
- [Bug Reporting](https://sourceforge.net/p/sdcc/bugs/new/) - if you have a problem using SDCC, we need to hear about it. Please attach **code to reproduce the problem**, and be sure to provide your email address so a developer can contact you if they need more information to investigate and fix the bug. Also report erroneous, missing or outdated documentation here.
- [SDCC Message Forum](https://sourceforge.net/p/sdcc/discussion/1864/) - an account on Sourceforge is needed if you're going to post and reply. Short easy online fill-in the blanks.
- [Open Knowledge Web Site](http://sdccokr.dl9sec.de/) - Run by Thorsten Godau \<thorsten.godau.AT.gmx.de>

## Who is SDCC?

- Sandeep Dutta \<sandeep.AT.users.sourceforge.net> - original author (SDCC's version of Torvalds)
- Jean Loius-VERN \<jlvern.AT.writeme.com> - substantial improvement in the back-end code generation.
- Daniel Drotos \<drdani.AT.mazsola.iit.uni-miskolc.hu> - Freeware simulator for 8051.
- Kevin Vigor \<kevin.AT.vigor.nu> - numerous enhancements and bug fixes to the Dallas ds390 tree.
- Johan Knol \<johan.knol.AT.users.sourceforge.net> - testing and patching ds390 tree, bug stompper extrodanaire
- Scott Dattalo \<scott.AT.dattalo.com> - sdcc for Microchip PIC controller target
- Karl Bongers \<karl.AT.turbobit.com> - mcs51 support, winbin builds, and an occasional bug.
- Bernhard Held \<bernhard.AT.bernhardheld.de> - snpshot builds and general housekeeping
- Frieder Ferlemann \<Frieder.Ferlemann.AT.web.de> - contributions to the documentation and last stages of code generation
- Jesus Calvino-Fraga \<jesusc.AT.ece.ubc.ca> - math functions, AOMF51, linker improvements
- Borut Ražem \<borut.razem.AT.gmail.com> - WIN32 MSC, cygwin and mingw ports, NSIS installer, preprocessor and front end improvements, bug fixing, snapshot builds on Distibuted Compile Farm, ...
- Vangelis Rokas \<vrokas.AT.otenet.gr> - PIC16 taget development for Microchip PIC18F microcontrollers
- Erik Petrich \<epetrich.AT.users.sourceforge.net> - Bug fixes and improvements for the front end, 8051, z80 and hc08
- Dave Helton \<dave.AT.kd0yu.com> - website design
- Paul Stoffregen \<paul.AT.pjrc.com> - mcs51 optimizations and website maintenance.
- Michael Hope \<michaelh.AT.juju.net.nz> - initial Z80 target, additional coding and bug fixes.
- Maarten Brock \<sourceforge.brock.AT.dse.nl> - several bug fixes and improvements, esp. for mcs51 target
- Raphael Neider \<RNeider.AT.web.de> - bug fixes and optimizations for PIC16, completion of the PIC14 target
- Philipp Klaus Krause \<pkk.AT.spth.de> - work on the STM8, f8, Z80, Z180, Rabbit, SM83, TLCS-90, eZ80 backends, compiler research
- Leland Morrison \<enigmalee.AT.sourceforget.net> - Rabbit 2000 support: the target code generator, sdasrab assembler and ucsim support
- Molnár Károly \<molnarkaroly.AT.users.sf.net> - adding pic devices, developing and maintaining pic device files generation scripts
- Ben Shi \<powerstudio1st.AT.163.com> - the front-end, the STM8 back-end, and the MCS-51 back-end maintain

SDCC has had help from a number of external sources, including:

- Alan Baldwin \<baldwin.AT.shop-pdp.kent.edu> - Initial version of ASXXXX  and  ASLINK.
- John Hartman \<noice.AT.noicedebugger.com> - Porting ASXXXX and ASLINK for 8051.
- Dmitry S. Obukhov \<dso.AT.usa.net> - malloc and serial I/O routines.
- Pascal Felber - Some of the Z80 related files are borrowed from the Gameboy Development Kit (GBDK).
- [The GCC development team](http://gcc.gnu.org/) - for GNU C preprocessor, the basis of sdcpp preprocessor and gcc test suite, partially included into the SDCC regression test suite
- [The GNU Binutils development team](http://www.gnu.org/software/binutils/) - for GNU Binutils, the basis of sdbinutils
- [Boost Community](http://www.boost.org/) - for Boost C++ libraries used in sdcc compiler
- [Timo Bingmann](http://idlebox.net/2007/stx-btree/) - for STX B+ Tree C++ Template Classes used in sdcc compiler
- Malini Dutta \<malini.AT.mediaone.net> - Sandeep's wife, for her patience and support.

* * *
