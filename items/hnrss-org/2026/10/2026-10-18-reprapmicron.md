---
title: RepRapMicron
link: https://github.com/VikOlliver/RepRapMicron/wiki
source: hnrss-org
published: 2026-10-18T13:48:48Z
updated: 2026-10-18T13:48:48Z
first_seen: 2026-10-18T15:47:38.003788439Z
authors:
- RobotToaster
content: extracted
html: 2026-10-18-reprapmicron.html
extra:
  comments_url: https://news.ycombinator.com/item?id=50141076
  num_comments: 19
  points: 117
---

## μRepRap - The RepRapMicron

![μRepRap Logo](https://private-user-images.githubusercontent.com/4644768/436892773-59e6d080-767b-4553-bd26-09339e65fa82.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3OTI4NzAxODYsIm5iZiI6MTc5Mjg2OTg4NiwicGF0aCI6Ii80NjQ0NzY4LzQzNjg5Mjc3My01OWU2ZDA4MC03NjdiLTQ1NTMtYmQyNi0wOTMzOWU2NWZhODIucG5nP1gtQW16LUFsZ29yaXRobT1BV1M0LUhNQUMtU0hBMjU2JlgtQW16LUNyZWRlbnRpYWw9QUtJQVZDT0RZTFNBNTNQUUs0WkElMkYyMDI2MTAyNCUyRnVzLWVhc3QtMSUyRnMzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyNjEwMjRUMTkyNDQ2WiZYLUFtei1FeHBpcmVzPTMwMCZYLUFtei1TaWduYXR1cmU9MjFlMjRlYjg0YmZjNzJjNTJhYjYyZTg0ODUxNmVkZjM2NTI2YzkzNzUzYmI5NDVlMzMxZjJhZDZlNDg5ZGZhMiZYLUFtei1TaWduZWRIZWFkZXJzPWhvc3QmcmVzcG9uc2UtY29udGVudC10eXBlPWltYWdlJTJGcG5nIn0.RNfdKQf2hZl2Mrz2SC2TiwISyV7l2YFAEtc6KozM-NA) The µRepRap is intended to be a RepRap (a replicating 3D printer) capable of micron (1/1000th of a millimetre, or 1µm) and sub-micron fabrication, made with only common and easily accessible materials. This degree of accuracy was initially made possible by 3D printed microscope platforms designed by The OpenFlexure Project. Variations more suitable for prototyping and fabrication have since been developed, but kudos to them. Expectations for the initial μRepRap prototype were to demonstrate repeatable positioning to better than 3µm on a work area 10mm across and to produce a probe tip in lieu of a print head that is suitable for manufacture in a simple home workshop. This goal has effectively been achieved.

![image](https://private-user-images.githubusercontent.com/4644768/668039730-9ab74c35-6c5a-4132-b6c8-d0bddd409bd9.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3OTI4NzAxODYsIm5iZiI6MTc5Mjg2OTg4NiwicGF0aCI6Ii80NjQ0NzY4LzY2ODAzOTczMC05YWI3NGMzNS02YzVhLTQxMzItYjZjOC1kMGJkZGQ0MDliZDkucG5nP1gtQW16LUFsZ29yaXRobT1BV1M0LUhNQUMtU0hBMjU2JlgtQW16LUNyZWRlbnRpYWw9QUtJQVZDT0RZTFNBNTNQUUs0WkElMkYyMDI2MTAyNCUyRnVzLWVhc3QtMSUyRnMzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyNjEwMjRUMTkyNDQ2WiZYLUFtei1FeHBpcmVzPTMwMCZYLUFtei1TaWduYXR1cmU9MDVlYmU4MGY4NmZlOWRiMDUyZmYyNWRmNWQ2ZTE1ZjMzMDViMTU0NmZmMzZiNWNmMzg1ZWRhYmYzYTUyMTkxZCZYLUFtei1TaWduZWRIZWFkZXJzPWhvc3QmcmVzcG9uc2UtY29udGVudC10eXBlPWltYWdlJTJGcG5nIn0.PnAHwAnyN47LazSWsinSL7TH2cTnTj7bQI95-eLHEIA)

*0.5mm "Benchy" 3D test model printed with μRepRap, 4μm layer height*

## PIKA Version

This is the current recommended build. ["PIKA" V0.02 hardware](https://github.com/VikOlliver/RepRapMicron/wiki/PIKA-Build-Process) is producing repeatable positioning of approximately 1μm and reliable 10μm or better accuracy in an approximately 8mm cubic volume. Using a fine probe etched with the PIKA mechanism itself, UV resin droplets as small as 4μm have been deposited. With a fibre optic probe 9μm resin lines have been cured.

![image](https://private-user-images.githubusercontent.com/4644768/604790176-f85252d6-99b2-4fa2-a942-6779b82f343e.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3OTI4NzAxODYsIm5iZiI6MTc5Mjg2OTg4NiwicGF0aCI6Ii80NjQ0NzY4LzYwNDc5MDE3Ni1mODUyNTJkNi05OWIyLTRmYTItYTk0Mi02Nzc5YjgyZjM0M2UucG5nP1gtQW16LUFsZ29yaXRobT1BV1M0LUhNQUMtU0hBMjU2JlgtQW16LUNyZWRlbnRpYWw9QUtJQVZDT0RZTFNBNTNQUUs0WkElMkYyMDI2MTAyNCUyRnVzLWVhc3QtMSUyRnMzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyNjEwMjRUMTkyNDQ2WiZYLUFtei1FeHBpcmVzPTMwMCZYLUFtei1TaWduYXR1cmU9MjM5MDI0NDlmMGJkZjYzNjZjZDQ2MzNhZjdmMjZkY2NjY2ViM2M5YTI2NDU0OThlNDU3NWU3YmViZDllNWQ3MCZYLUFtei1TaWduZWRIZWFkZXJzPWhvc3QmcmVzcG9uc2UtY29udGVudC10eXBlPWltYWdlJTJGcG5nIn0.WFQvo-yY69RbZcBSzl-e0tLi0b4Ki5ZKvwGBaqewjg0)

*PIKA V0.02 Complete Circa May 2026*

## MAUS Version (Deprecated)

This was devised as a testbed for flexure components, and is only mentioned because it has been thoroughly documented with a bill of materials etc. It was updated to V0.05 to give compatibility with PIKA Axis Drivers, but the full documentation is for the "MAUS C" V0.04 (essentially a debugged V0.03). This achieved approximately 5μm positioning best case over a 4mm square, though motion is not linear. [Maus-C-Build-Process](https://github.com/VikOlliver/RepRapMicron/wiki/Maus-C-Build-Process) instructions have been created. If you want to download the pre-built STL files [go here to Printables](https://www.printables.com/model/1286343-reprapmicron-micron-accurate-3d-printer) and day-to-day progress is detailed on [the RepRap blog](http://blog.reprap.org).

![image](https://private-user-images.githubusercontent.com/4644768/546703683-6653488e-c352-46e9-ba1a-bec3b3af690f.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3OTI4NzAxODYsIm5iZiI6MTc5Mjg2OTg4NiwicGF0aCI6Ii80NjQ0NzY4LzU0NjcwMzY4My02NjUzNDg4ZS1jMzUyLTQ2ZTktYmExYS1iZWMzYjNhZjY5MGYucG5nP1gtQW16LUFsZ29yaXRobT1BV1M0LUhNQUMtU0hBMjU2JlgtQW16LUNyZWRlbnRpYWw9QUtJQVZDT0RZTFNBNTNQUUs0WkElMkYyMDI2MTAyNCUyRnVzLWVhc3QtMSUyRnMzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyNjEwMjRUMTkyNDQ2WiZYLUFtei1FeHBpcmVzPTMwMCZYLUFtei1TaWduYXR1cmU9ZTIwYTQ4OWU4ZGNjY2M2Mjc2ZTM5OTEzZjkwMWMyZjViYTUzYzIyMDQ4MmY0ZGY2Nzg5NmNhZjlkOTJiZWJjZiZYLUFtei1TaWduZWRIZWFkZXJzPWhvc3QmcmVzcG9uc2UtY29udGVudC10eXBlPWltYWdlJTJGcG5nIn0.QJUKwV-XH9O_s2i-Cmt3aN4foZEOZMPEL4ToW4xqvkY)

*MAUS 'L' V0.05 μRepRap, in development Feb 2026*

## Overview

Microelectronics and Micro-electromechanical Systems (MEMS) are essential components of most of the electronic wizardry we use in our everyday lives, whether we realise it or not. In the way that RepRap brought Open 3D fabrication to the masses, the aim of the µRepRap Project is to bring users the same capabilities on a much smaller scale and allow those components to evolve in the same way. The solution is effectively a very precise and compact CNC machine.

The objective has two related parts: To create a macro scale 3D printed micron fabrication and manipulation system, and while doing this develop the techniques needed to reproduce a similar system printed at the micron scale.

## Background

![image](https://private-user-images.githubusercontent.com/4644768/437644341-1117fefc-e405-4ab7-bd2a-c1083ba4295a.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3OTI4NzAxODYsIm5iZiI6MTc5Mjg2OTg4NiwicGF0aCI6Ii80NjQ0NzY4LzQzNzY0NDM0MS0xMTE3ZmVmYy1lNDA1LTRhYjctYmQyYS1jMTA4M2JhNDI5NWEucG5nP1gtQW16LUFsZ29yaXRobT1BV1M0LUhNQUMtU0hBMjU2JlgtQW16LUNyZWRlbnRpYWw9QUtJQVZDT0RZTFNBNTNQUUs0WkElMkYyMDI2MTAyNCUyRnVzLWVhc3QtMSUyRnMzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyNjEwMjRUMTkyNDQ2WiZYLUFtei1FeHBpcmVzPTMwMCZYLUFtei1TaWduYXR1cmU9MjFiMDA4MWIwMTc3ZDcxZDdiOWNhMTkxMDZjNjg0MjA0YWYxYTQzMWIwZmNlZTYwODM1ZTlkODQxM2U4Y2U4OCZYLUFtei1TaWduZWRIZWFkZXJzPWhvc3QmcmVzcG9uc2UtY29udGVudC10eXBlPWltYWdlJTJGcG5nIn0.DGgMGjI2qaGfEyVYtJTDn8TwBgkBhq32ENXTdzAutyY)

*A 1.3mm "Hello World" mechanically etched in Sharpie Marker on glass slide mid-2025*

Precision manufacturing began somewhere in the 1700's, and the first micron scale electronic devices were fabricated from silicon in the 1960’s. The [Intel 4004](https://en.wikipedia.org/wiki/Intel_4004), for example, was made with a feature size of 10μm. Ever since then the technology focused largely on silicon, with fabrication systems becoming ever more complex, esoteric, and costly. The techniques used are difficult for the average hobbyist to manage, and in many cases are downright dangerous.

In biology and medicine, equipment to measure and manipulate objects on the micron scale are relatively common - though these devices tend to be large, specialized, and expensive. Recently though, microscope platforms capable of sub-micron resolution were developed by [The OpenFlexure Project](https://openflexure.org), and these have created an interesting opportunity for developing micron scale fabrication.

If micron scale manufacturing can be achieved by RepRap-like technology, it is likely that these fields will be advanced in the same way that desktop fabrication was by the RepRap. The biological sciences will gain from inexpensive, rapidly-evolving equipment. The microelectronics field will regain the potential for independent communities develop on the micron scale, and break away from its fixation on silicon as its main platform. As with 3D printing, there will certainly be new developments in fields that do not currently even exist.

There is also the possibility (and intent) that this technology could replicate in the future at a yet smaller scale.

## Requirements

Micron scale 3D printing has many of the same requirements that The RepRap Project developed when initially printing on the macro scale:

- A 3-axis positioning system
- CAD/CAM software
- Axis zeroing sensors
- A deposition system
- Building material

In addition there is the practical aspect that humans are unable to directly manipulate micron scale assemblies and sub-assemblies. Novel systems are therefore needed to:

- Detach printed items
- Transport items
- Rotate and position items
- Conduct micron scale maintenance tasks
- Operate and interface with items

Similar issues will exist if and when a micron scale system is developed.

## Optics

While conventional optics are readily available to allow humans to initiate and inspect the fabrication processes, it is likely that some customisation of the optics will be desirable.

For the moment two USB microscopes are sufficient. One to observe the initial Z probe height during setup, and one for the XY positioning of the probe when commencing a print. A minimum of 8Mpixel resolution is recommended. Additionally a conventional bench microscope of 100x to 200x will be needed to examine the output.

![image](https://private-user-images.githubusercontent.com/4644768/659394589-3ec4efc2-0162-48ca-8936-6c8da4a4ccd7.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3OTI4NzAxODYsIm5iZiI6MTc5Mjg2OTg4NiwicGF0aCI6Ii80NjQ0NzY4LzY1OTM5NDU4OS0zZWM0ZWZjMi0wMTYyLTQ4Y2EtODkzNi02YzhkYTRhNGNjZDcucG5nP1gtQW16LUFsZ29yaXRobT1BV1M0LUhNQUMtU0hBMjU2JlgtQW16LUNyZWRlbnRpYWw9QUtJQVZDT0RZTFNBNTNQUUs0WkElMkYyMDI2MTAyNCUyRnVzLWVhc3QtMSUyRnMzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyNjEwMjRUMTkyNDQ2WiZYLUFtei1FeHBpcmVzPTMwMCZYLUFtei1TaWduYXR1cmU9MTNiYzcwYTllMjE0OTJhYjkzYTY1NDdmMjdiOWI1OTIwMDRiYjNhNDFmZDBhMWM2MmI5NzgyNGZhNTYyMTYxZiZYLUFtei1TaWduZWRIZWFkZXJzPWhvc3QmcmVzcG9uc2UtY29udGVudC10eXBlPWltYWdlJTJGcG5nIn0.xkP3ET5RGpfbUkCV3SzEtnse0LFqRlTR3RQ_qbTkT24)

*"Hello, World" and a 100μm square set in resin using UV and a 125/9 optical fibre*

Additionally, an optical fibre may be attached as the probe. This can be used to cure UV resin by projecting a sufficiently powerful UV source down the probe. [This section](https://github.com/VikOlliver/RepRapMicron/wiki/PIKA-UV-Head) describes how a 125/9 optical fibre patch lead and 3W UV LED module can be attached to the μRepRap as a print head.

## 3-Axis Control

The axes are driven by 400-step NEMA17 stepper motors, but 200-step motors will get quite close to 1μm. The driving hardware and firmware is left to user preference, though existing developments assume interactive control using GCODE on a serial connection. Arduino GRBL is adequate.

## Probe Print Head

The probe is used as a Z touch sensor, engraving tool, and deposition head. A robust probe for initial testing can be made from a 0.5mm hypodermic needle or 0.25mm accupuncture needle. These are sufficiently robust to use subtractively on resist-covered surfaces such as Sharpie marker on a glass slide.

For fine deposition an electrochemically etched wire point is recommended. Instructions for fabrication with minimal equipment are in the Probe documentation. The PIKA is capable of etching its own probes.

## Building Materials

Additive fabrication is currently implemented by transferring minute quantities of "Top Coat" UV nail gel resin with the probe. The reservoir is a smeared layer of resin. The probe accepts this resin by capillary action. When depositing, capillary action forms relatively consistent droplets. The number of consecutively deposited droplets and their consistency depend on the qualities of the resin and probe, but sub-15μm droplets are achievable.

Other resins and ionic gels may also be used. A UV LED in the print bed operated by the hardware controller sets the resin.

**Note:** Thin layers of 3D printer UV resin do not readily set in air as oxygen neutralises the activated photoinitiator.

Other substances may be transferred. They key limitation is that their solvent should not evaporate during deposition.

Materials that readily adhere to the resin (which may be partially cured first) such as gold leaf may be added.

## Funding

The RepRapMicron is not funded by any academic institution, or any organization. If you wish to collaborate on any part of the project, open a Github issue and be welcomed. If you wish to contribute financially, Vik's Workshop has a [Buy Me A Coffee](https://ko-fi.com/viksworkshop) link. Vik could do with quite a lot of coffee...

## Summary

The current μRepRap 3D printer is intended to provide a platform for further hardware and software development on the micron scale.

Much work remains to be done in determining what can be printed and in what materials. If objects cannot be printed high enough, or with overhangs, folded structures, hinges, and flexures will need to be developed to produce complex 3D shapes and mechanisms.

It is hoped that μRepRap may be used to eventually construct useful devices in its own right, and an ecosystem of fabrication utilities that operate on a yet smaller scale.
