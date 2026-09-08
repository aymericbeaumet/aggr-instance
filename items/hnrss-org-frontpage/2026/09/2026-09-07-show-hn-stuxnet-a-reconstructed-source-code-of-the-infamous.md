---
title: 'Show HN: Stuxnet – A reconstructed source code of the infamous cyber-weapon'
link: https://github.com/Sadpainy/Stuxnet
source: hnrss-org-frontpage
published: 2026-09-07T22:12:38Z
updated: 2026-09-07T22:12:38Z
first_seen: 2026-09-08T04:23:50.420238947Z
authors:
- CMDDestory
summary: 'Stuxnet! Here reproduced by me. Only researchs educations purposes. Comments URL: https://news.ycombinator.com/item?id=49603546 Points: 125 # Comments: 45'
content: extracted
html: 2026-09-07-show-hn-stuxnet-a-reconstructed-source-code-of-the-infamous.html
preview:
  file: 2026-09-07-show-hn-stuxnet-a-reconstructed-source-code-of-the-infamous.preview-a661f624c236.webp
  width: 256
  height: 128
  alt: Stuxnet, Here reproduced by me, Only for researchs educations purposes. It set work on WindowsXP and Windows 7 only. - Sadpainy/Stuxnet
  color: '#f0f0f1'
images:
- source: https://opengraph.githubassets.com/c5316ba13e1810951e7042d588e3c5ddfa3d061b95fbafc2d5f3673a0a17c762/Sadpainy/Stuxnet
  original:
    file: 2026-09-07-show-hn-stuxnet-a-reconstructed-source-code-of-the-infamous.image-e1254272725b.png
    width: 1200
    height: 600
  variants:
  - file: 2026-09-07-show-hn-stuxnet-a-reconstructed-source-code-of-the-infamous.image-b6271a96419b.webp
    width: 48
    height: 24
  - file: 2026-09-07-show-hn-stuxnet-a-reconstructed-source-code-of-the-infamous.image-ecd7a0b6c4c4.webp
    width: 320
    height: 160
  - file: 2026-09-07-show-hn-stuxnet-a-reconstructed-source-code-of-the-infamous.image-c4d2510c9167.webp
    width: 640
    height: 320
  - file: 2026-09-07-show-hn-stuxnet-a-reconstructed-source-code-of-the-infamous.image-d8fba3dba8e2.webp
    width: 960
    height: 480
  - file: 2026-09-07-show-hn-stuxnet-a-reconstructed-source-code-of-the-infamous.image-cbfb4a0d4339.webp
    width: 1200
    height: 600
  color: '#fdfdfd'
---

[![Build Status](https://camo.githubusercontent.com/b0c6c6845a74cb65a7f0a32bdcfd8fbf80eeb40026c4029af424ab371c94b8bd/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f6275696c642d70617373696e672d627269676874677265656e)](https://github.com/Sadpainy/Stuxnet/blob/main) [![License: GPL v3](https://camo.githubusercontent.com/48bf9b56d44f38db53ce21294cf0b9487d0a3734ab3ba1fe4c69858ae20db2c1/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f4c6963656e73652d47504c76332d626c75652e737667)](https://www.gnu.org/licenses/gpl-3.0) [![Platform](https://camo.githubusercontent.com/6682970949ee789e20073a18954455d96912242b7b7119f73005eb05343bbedb/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f706c6174666f726d2d57696e646f777325323025374325323057696e434525323025374325323053434144412d6c6967687467726579)](https://github.com/Sadpainy/Stuxnet/blob/main)

This repository contains a strictly educational and research-oriented reconstruction of the infamous Stuxnet worm. It is the product of countless hours of reverse engineering work conducted by the global security research community on the original binary samples discovered in 2010.

Disclaimer: This code is provided solely for academic study, malware analysis training, and defensive research. It is not intended to be used for any malicious purposes, nor is it a deployable piece of malware. The authors and contributors do not condone illegal or unethical activities.

Table of Contents

Overview

Core Components

Technical Architecture

Build Instructions

Usage

Legal and License

Acknowledgements

## Overview

[](https://github.com/Sadpainy/Stuxnet#overview)

Stuxnet is widely recognized as the first known cyber-weapon designed to cause physical destruction to industrial control systems (ICS). It specifically targeted Siemens Step 7 software and S7-300/400 PLCs, ultimately manipulating frequency converter drives to damage centrifuge rotors.

This repository is a reconstructed source code derived from the decompiled binaries. It preserves the original logic and attack vectors while structuring the codebase for readability and analysis.

Key Characteristics

Target: Siemens SIMATIC WinCC, Step 7, and S7 PLCs.

Propagation: USB drives (LNK exploits), Network shares (Print Spooler), Peer-to-Peer (P2P).

Payload: Modification of PLC block logic (OB1/OB35) to alter motor frequencies.

Stealth: Advanced Rootkit capabilities (MRxCls.sys, MRxNet.sys) for file, process, and registry hiding.

## Core Components

[](https://github.com/Sadpainy/Stuxnet#core-components)

The repository is organized by the primary modules identified during the analysis of the original malware.

Module: Loader/Dropper Filename: winsta.exe, ~WTR4141.tmp Description: Entry point responsible for initial infection, privilege escalation, and deployment of other components.

Module: Privilege Escalation Filename: ~WTR4132.tmp Description: Exploits the Win32k.sys vulnerability to gain system-level privileges.

Module: S7 Hook Library Filename: s7otbxdx.dll Description: Malicious replacement of the original s7otbxsx.dll. Intercepts communication between Step 7 and the PLC.

Module: Step7 Hook Library Filename: s7aaapix.dll Description: Intercepts AUT (Automation Tool) API calls within the Step 7 engineering environment.

Module: Rootkit (File System) Filename: mrxcls.sys Description: Kernel-mode driver used to hide Stuxnet files, processes, and registry keys via SSDT hooking.

Module: Rootkit (Network) Filename: mrxnet.sys Description: Filters file system requests to hide malicious files and enables P2P propagation.

Module: Payload (Attack) Filename: s7plcmain Description: The core logic responsible for the "Frequency Tampering" attack that damages the centrifuges.

## Technical Architecture

[](https://github.com/Sadpainy/Stuxnet#technical-architecture)

The following describes the high-level execution flow of the Stuxnet framework.

Stage 1: Initial Infection Vector (USB/Network) Stage 2: Dropper and Escalation Stage 3: Check Environment Stage 4a: Target Found (Siemens Software) -> Install S7 Hooks Stage 4b: Non-target -> Self-Destruct/Idle Stage 5: Monitor PLC Writes Stage 6: Detect OB1/OB35 Write -> Inject Payload Stage 7: Modify Frequency Output Stage 8: Physical Damage to Centrifuges Stage 9: Install Rootkit (MRxCls) Stage 10: Hide Files and Registry Stage 11: Load Network Module (MRxNet) Stage 12: P2P Propagation

Execution Flow

1. Environment Reconnaissance: The worm checks for the presence of specific Siemens software (WinCC, Step 7) and specific target PLCs (S7-315, S7-417).

2. DLL Injection: It intercepts the s7blk\_write function call.

3. Code Injection: When a user downloads a project to the PLC, the malicious code is appended to the OB1/OB35 blocks.

4. Physical Impact: The PLC executes the manipulated code, causing the connected variable frequency drives (VFDs) to spin at abnormal frequencies (high/low), resulting in mechanical damage.

Build Instructions

Important: This codebase is designed for static analysis and debugging in a controlled virtual environment. It is not intended for live deployment on any critical infrastructure.

Requirements

Build Environment: Microsoft Visual Studio 2019/2022 (Windows) or mingw-w64.

Target OS: Windows XP / Windows 7 (for driver compatibility).

Driver Kit: Windows Driver Kit (WDK) 7600 (if compiling kernel drivers).

Building the User-Mode Modules

Clone the repository

git clone [https://github.com/Sadpainy/Stuxnet.git](https://github.com/Sadpainy/Stuxnet.git) cd stuxnet-analysis

Build the main dropper

cd winsta nmake /f Makefile.win

Build the S7 hook library

cd ../s7otbxdx cl /LD s7otbxdx.c user32.lib ws2\_32.lib

## Usage

[](https://github.com/Sadpainy/Stuxnet#usage)

This code is intended for:

Malware Analysis: Understanding the specific code logic used in advanced persistent threats (APTs).

Defensive Research: Developing detection signatures for ICS security tools (e.g., YARA rules, Snort signatures).

Academic Study: Examining the intersection of cybersecurity and critical infrastructure protection.

Analysis Setup

1. Isolate Environment: Use a virtual machine (VMWare/VirtualBox) with Host-Only networking enabled. Disable internet connectivity.

2. Load Modules: Analyze the .dll and .sys files using tools such as IDA Pro, Ghidra, or x64dbg.

3. Monitor Activity: Use Process Monitor (ProcMon), Process Hacker, and Wireshark to observe the behavior.

Legal and License

## License

[](https://github.com/Sadpainy/Stuxnet#license)

This project is licensed under the GNU General Public License v3.0. See the LICENSE file for details.

## Disclaimer

[](https://github.com/Sadpainy/Stuxnet#disclaimer)

The code in this repository is a product of reverse engineering for educational purposes only. The original authors of the Stuxnet worm are anonymous, but the reconstruction contained herein is the work of independent security researchers.

The authors do not claim ownership of the original malware.

The code is provided "AS IS" without warranty of any kind.

The authors are not responsible for any misuse or damage caused by this code.

By using this repository, you acknowledge that you are solely responsible for ensuring compliance with all applicable laws and regulations.

## Acknowledgements

[](https://github.com/Sadpainy/Stuxnet#acknowledgements)

This research and reconstruction would not have been possible without the extensive analysis and threat intelligence provided by global cybersecurity vendors.

Symantec (W32.Stuxnet dossier)

Kaspersky Lab (The Stuxnet saga)

ESET (Stuxnet under the microscope)

Amr Thabet and Christian Roggia (research-virus/stuxnet)

This is an academic reconstruction. Use it to build stronger defenses, not to cause harm.
