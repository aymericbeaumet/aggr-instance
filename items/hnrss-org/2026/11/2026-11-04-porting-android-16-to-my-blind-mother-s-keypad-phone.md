---
title: Porting Android 16 to my blind mother's keypad phone
link: https://z1x.us/blog/four-hundred-bucks-of-e-waste
source: hnrss-org
published: 2026-11-04T15:18:59Z
updated: 2026-11-04T15:18:59Z
first_seen: 2026-11-08T18:46:19.638047103Z
authors:
- z1xus
labels:
- accessibility
- android
- reverse engineering
content: extracted
html: 2026-11-04-porting-android-16-to-my-blind-mother-s-keypad-phone.html
remote_preview:
  url: https://z1x.us/blog/four-hundred-bucks-of-e-waste/embed.png
  alt: four hundred bucks of e-waste
---

## introduction

my mother's phone started dropping calls

it was a [nokia 1280](https://en.wikipedia.org/wiki/Nokia_1280). i kept it alive for years, multiple battery replacements and all, but it was an old nokia 1280 and it ended like an old nokia 1280

you might be asking who the fuck uses that phone in the big 2026. i think your thinking is a bit linear. theres a use case for almost anything. my mother is visually impaired, and for her the familiarity of the buttons mattered the most. she never cared to learn how to use a smartphone or watch youtube on one (for that she has a shitty fire tv stick that she voice prompts, which i might also root one day)

so we went looking for a new phone and stopped on a [kapsys smartvision3](https://www.kapsys.com/en/produit/smartphones-smartvision-3/). bought it used from ebay for 400 bucks. a whole fucking lot

i didnt research it much, and thats on me. but it was presented as the perfect solution: a real keypad, a talking interface, [be my eyes](https://www.bemyeyes.com) built in, a pile of certifications for blind people. nothing could go wrong right?

oh god was i mistaken

this thing is a piece of e-waste. i didnt even look at the specs when i bought it. by the time i did there was no way back. it discharged like crazy and it lagged. their os came with a bunch of bloatware of their own, and to fix that, they shipped their own "booster" app. do i need to say it was also android 11?

half a year had passed since she got it. she still hadnt learned anything on it apart from pressing the assistant button and asking it questions, while the whole thing crashed around her. that annoyed me more than it should have. my blood boiled every time she brought me its answers instead of asking me, and i dont even know why. but thats not the point

i was pissed at the manufacturer. kapsys promised [updates till 2027](https://sastltd.zohodesk.eu/portal/en/kb/articles/smartvision-3-updating-to-android-13). the patch level on her phone said april 2024. half the apps crashed. telegram, the app everyone uses to call her because calls on her sim cost a fortune in roaming, didnt work in the background no matter what i did to stop android from killing it. the device was simply too weak

so one thursday evening i decided its time

## what i was working with

my first idea was debloating. i looked into [universal android debloater ng](https://github.com/Universal-Debloater-Alliance/universal-android-debloater-next-generation), a tool that switches off preinstalled apps, but disabling apps on a rom thats rotten underneath never felt like enough. i searched [xda](https://xdaforums.com) and [4pda](https://4pda.to), the two big android modding forums. nothing on this phone. not a rom, not a root guide, not even someone complaining

so i plugged it in and started poking at it over adb

- android 11, security patch from april 2024
- the drivers underneath are even older, december 2021 (how??)
- mediatek helio p60, 4 gb of ram
- kernel 4.14.186. remember this number ![:ominouscat:](https://z1x.us/blog/four-hundred-bucks-of-e-waste/ominouscat.webp ":ominouscat:")
- twenty-five heavy apps fighting over those 4 gb

the crashes had a boring cause. the google app had updated itself from the play store and was crash looping, and the phone sends all voice input through it, so dictation died with it. kapsys also says android 12 is ["not possible"](https://sastltd.zohodesk.eu/portal/en/kb/articles/smartvision-3-updating-to-android-13) on this phone

i dont really take "not possible" well ![:catjlb:](https://z1x.us/blog/four-hundred-bucks-of-e-waste/catjlb.webp ":catjlb:")

## what kapsys actually built

the plan was to put a modern android on it. but the whole point of this phone is the keypad. arrows and a middle key to move around, real number keys like on her nokia, and the phone reads out whatever she lands on. flash something else and she loses all of that. so first i had to know how kapsys did it

im not much of an android dev. the last time i flashed a custom rom i was 13, and i had never built one. honestly not very good at reading decompiled java either. so i pulled every kapsys app and the android system files off the phone, decompiled them with [jadx](https://github.com/skylot/jadx), and handed the pile to a clanker to audit while i looked at the keypad myself

my guess was that some kapsys app catches the key presses and controls [talkback](https://support.google.com/accessibility/android/answer/6283677), google's screen reader. half an hour later the audit had rejected my thesis: kapsys patched the android itself. every key press on android goes through a system library called `libinput.so`, and kapsys added a function to it, literally called `SV3KeyboardForTalkback`, that turns arrow presses into talkback commands. they also patched the dialer, the status bar and settings

so i couldnt just move their apps onto a new android... fine, i'll rebuild it

## backups (lol)

contacts, sms, calendar and photos came off fine. app data did not. `adb backup` wants you to confirm on the phone, i pressed allow three times, and every time the file stayed at zero bytes. she barely used apps anyway, so i just ![:catwave:](https://z1x.us/blog/four-hundred-bucks-of-e-waste/catwave.webp ":catwave:")

the real backup was supposed to be a full copy of the phone's storage. on mediatek phones you do that with [mtkclient](https://github.com/bkerler/mtkclient) through brom, a tiny program burned into the chip that works even when everything else is broken. you get into it by holding the volume buttons while plugging in the cable

for forty minutes i held every button combination known to man while plugging the cable in. THIS SHIT DOES NOT WORK !!!! the phone showed up for about four seconds, then booted android like nothing happened. every. single. time.

\~/dev/sv3

```
> uv run mtk.py rl backup/raw --skip userdata
For brom mode, press and hold vol up, vol dwn, or all hw buttons and connect usb.
For preloader mode, don't press any hw button and connect usb.
If it is already connected and on, hold power for 10 seconds to reset.
..........Preloader
Preloader - [LIB]: Status: Handshake failed, retrying...
Port - Hint:
Power off the phone before connecting.
For brom mode, press and hold vol up, vol dwn, or all hw buttons and connect usb.
...........

~ z1xus fish
~  sv3 ~/dev/sv3  
```

i gave up. the only thing i was really scared of losing was the imei (no imei, no calls), and unlocking doesnt touch it. how bad could it be

there are two kinds of people: those who make backups, and those who are about to

## first flash

unlocking it took one toggle in developer options and one fastboot command. no account, no waiting period, no unlock code to request. thank you kapsys, honestly, thats the one thing you got right

\~/dev/sv3

```
> fastboot flashing unlock
(bootloader) Start unlock flow
OKAY [ 69.642s]
Finished. Total time: 69.642s
> fastboot getvar unlocked
unlocked: yes

~ z1xus fish
~  sv3 ~/dev/sv3  
```

i went with [lineageos 23.2](https://github.com/MisterZtr/LineageOS_gsi), android 16, as a gsi. thats a generic android build that runs on almost any phone by reusing the phone's own drivers. i switched off the boot signature check (more on that later), made room for it on the storage and flashed

i expected a boot animation. i got this

Android Recovery

KAPSYS/SmartVision3\_EEA/SmartVision3

11/RP1A.200720.011/1713780416

user/release-keys

Use volume up/down and power.

Android Recovery

KAPSYS/SmartVision3\_EEA/SmartVision3

11/RP1A.200720.011/1713780416

user/release-keys

Use volume up/down and power.

Android Recovery

KAPSYS/SmartVision3\_EEA/SmartVision3

11/RP1A.200720.011/1713780416

user/release-keys

Use volume up/down and power.

-- Wiping data...

Formatting /data...

Formatting /metadata...

Data wipe complete.

Android Recovery

KAPSYS/SmartVision3\_EEA/SmartVision3

11/RP1A.200720.011/1713780416

user/release-keys

Use volume up/down and power.

i picked factory data reset. it rebooted, and went straight back into recovery

## guessing

"insanity is doing the same thing over and over and expecting different results." einstein never said that, but i did prove it that evening

this is where the evening got long. the phone shows the logo for about fifteen seconds and drops into recovery, and recovery has no way to show me what went wrong. so i started bruteforcing my way in. change one thing, flash, reboot, stare at a black screen, repeat. i put back a partition i had deleted, removed her encryption settings, tried android 14 instead of 16, even chased an audio bug that killed some other mediatek phone. WHY ARE YOU IN RECOVERY AGAIN.

the only lead i found that night was a gigaset gs4 owner on the /e/os forum. same mediatek chip, stock android 11, kernel 4.14.186 (told you). their android 14 gsi showed the gigaset logo and then android recovery

eventually i opened "view recovery logs" on the phone and photographed the screen. 397 lines, one photo at a time

\[ 0.003902\] platform device\_type is unknown

\[ 0.003976\] platform uses default device\_type emmc

\[ 0.003985\] Error: Retry fail /cache partition not found

\[ 0.003992\] recovery filesystem table

\[ 0.003998\] ================================

\[ 0.004003\] 0 /system ext4 system\_a 0

\[ 0.004009\] 1 /vendor ext4 vendor\_a 0

\[ 0.004014\] 2 /product ext4 product\_a 0

\[ 0.004020\] 3 /metadata ext4 /dev/block/platform/bootdevice/by-name/md\_udc 0

\[ 0.004026\] 4 /data ext4 /dev/block/platform/bootdevice/by-name/userdata 0

\[ 0.004032\] 5 /mnt/vendor/protect\_f ext4 /dev/block/platform/bootdevice/by-name/protect1 0

\[ 0.004037\] 6 /mnt/vendor/protect\_s ext4 /dev/block/platform/bootdevice/by-name/protect2 0

then i ran all the photos through ocr, and it fucked up the formatting completely

\~/Downloads

```
> head -4 recovery-log.txt
[    0.003902] platform device_type is unknown[span_0](start_span)[span_0](end_span)
[    0.003976] platform uses default device_type emmc[span_1](start_span)[span_1](end_span)
[    0.003985] Error: Retry fail /cache partition not found[span_2](start_span)[span_2](end_span)
[    0.003992] recovery filesystem table[span_3](start_span)[span_3](end_span)

~ z1xus fish
~  Downloads ~/Downloads  
```

i knew a clanker would chew through it easily, so i threw the whole thing at one. it saw the phone had rebooted on a timer and decided the bootloader ran out of boot attempts

![a soaking wet cat staring straight into the camera](https://z1x.us/blog/four-hundred-bucks-of-e-waste/soggy-cat.webp)

it hadnt. a recovery log only knows about recovery anyway, not the boot that died before it

## second opinions

a few hours in i was going in circles, so i wrote everything down and asked two different models for a second opinion. they mostly agreed, which was a nice change from me disagreeing with myself every half hour

the problem was the signature check i switched off. android checks a signed file called vbmeta at boot, and it has a "dont check anything" flag, which is what i used. but my vbmeta wasnt signed at all, and this phone refuses an unsigned one before it even reads the flag. the fix was boring: same flag, just signed with google's public test key

still no boot. but at least now the phone fell back into fastboot by itself instead of recovery. small wins ![:catthumbsup:](https://z1x.us/blog/four-hundred-bucks-of-e-waste/catthumbsup.webp ":catthumbsup:")

## mtkclient, round two

the phone died somewhere later now, and to see where i needed the kernel's last words. mediatek phones keep them in `expdb`, a crash log that survives reboots. which meant mtkclient again. it got past the handshake this time, and froze right after

\~/dev/sv3

```
> grep -E 'Status|Jump|sync|status' /tmp/mtk.log | tail -6
Preloader - Status: Waiting for PreLoader VCOM, please reconnect mobile/iot device to brom mode
Preloader - [LIB]: Status: Handshake failed, retrying...
DAXFlash - Successfully uploaded stage 1, jumping ..
Preloader - Jumping to 0x200000: ok.
DAXFlash - Successfully received DA sync
    status = self.status()  # 0xC0070004

~ z1xus fish
~  sv3 ~/dev/sv3  
```

the phone hung with no button working. mtkclient switches off the watchdog, the timer that normally resets a stuck phone. the only way out was holding power for fifteen seconds. a tiny script that starts mtkclient the exact second the phone shows up did the trick. a few hours later than planned i finally had the full copy, about seven flashes too late

and inside `expdb`

\~/sv3-backup/raw

```
> strings -n 8 expdb.bin | grep -n reboot | head -6
385:[RTC]rtc_2sec_reboot_check 0x2575, without 2sec reboot, type 0x2
439:[PLFM] WDT reboot bypass power key!
1417:RAM_CONSOLE. set reboot reason info done
1803:RAM_CONSOLE. set reboot reason info done
1969:.(0)[1:init]reboot: Restarting system with command 'recovery'
1984:-(0)[1:init] SyS_reboot+0x15c/0x228

~ z1xus fish
~  raw ~/sv3-backup/raw  
```

`[1:init]` means process number 1. thats init, the first program android starts, the one that mounts everything and launches the rest. init itself was rebooting the phone into recovery. something made it give up

after the copy i ran mtkclient's `reset`, expecting a reboot. the phone just turned off. reset means power off. WHO NAMES POWER OFF RESET?? ![:catcrazy:](https://z1x.us/blog/four-hundred-bucks-of-e-waste/catcrazy.webp ":catcrazy:") so i tried a proper reboot through mtkclient instead, and it froze again. on mediatek the watchdog is what actually reboots the phone, and mtkclient had switched it off. guess were holding the power button for fifteen seconds again

## midnight

the phone had to work by morning. its her only phone, and it was a school night for me, so i couldnt just keep going until it worked

change of plans: just get stock back. i had copied the system files off the phone in the very first hour, so i tried to turn them back into something flashable. that copy was useless. android hides some files from adb, and 87 of them were silently missing, including `init` itself. tar didnt complain once. thanks tar. really. ![:wtfcat:](https://z1x.us/blog/four-hundred-bucks-of-e-waste/wtfcat.webp ":wtfcat:") my "full backup" from hour one was a straight fucking lie

what saved me was kapsys's own update app. it calls kapsys's update api: you log in with the phone's imei and serial number, and it hands you the full official firmware. i flipped the phone over, read the imei off the label, and a few minutes later i had a 1.8 gb zip with every partition inside

why didnt i look for that first? good question. i asked myself that too

the phone was stuck again, black screen with a faint glow, and for a minute i genuinely thought i had bricked it

\~

```
> lsusb | grep -i mediatek
Bus 001 Device 127: ID 0e8d:2000 MediaTek Inc. MT65xx Preloader

~ z1xus fish
~  home ~  
```

i caught it in the few seconds it shows up, pushed it into fastboot, and flashed kapsys's firmware back. one factory reset later it booted. stock android 11 again, around 1am, about eight hours after the unlock. i put her photos back and went to sleep

## the slow part

before bed i turned to my dear llm once again and tasked it with finding out, overnight, how anyone gets android 14+ running on a phone this old. the answer hurt

the gsi i used sends a crashing init to recovery, where normal android would send it to the bootloader. i spent a whole evening reading crashes as "please open recovery". and that 4.14 kernel throws away most log lines unless you boot it with a special flag, so init's actual error never got written down

it also pointed me to dsu, dynamic system updates. you install a test android as a file, boot into it, and if it explodes, one reboot brings you back to stock. her phone stays untouched. THIS EXISTED THE WHOLE TIME??

## init threw an exception

phones like this keep two copies of android, slot a and slot b, so updates can go into the unused one. her stock android lived in slot a. slot b was empty. so i put a test build in slot b, with the logging flag on, set to stop before it touched any of her data

this is the crash i had been chasing since thursday 👇

\~/sv3-backup/slot-b

```
> grep -n -A13 'took 282ms' ramoops.txt
865:[   10.894032] init: Command 'mount_all /vendor/etc/fstab.mt6771 --early' action=fs took 282ms and succeeded
866:[   10.900986] init: InitFatalReboot: signal 6
867:[   10.924449] init:   #00 pc 00000000001122f0  /system/bin/init (android::init::InitFatalReboot(int)+228)
868:[   10.926336] init:   #01 pc 00000000001127a8  /system/bin/init (android::init::InstallRebootSignalHandlers()::$_0::__invoke(int)+32)
869:[   10.928460] init:   #02 pc 0000000000000628  [vdso] (__kernel_rt_sigreturn)
870:[   10.929541] init:   #03 pc 0000000000075888  /system/lib64/bootstrap/libc.so (abort+152)
871:[   10.931212] init:   #04 pc 00000000000804c8  /system/lib64/libc++.so (__abort_message+256)
872:[   10.933067] init:   #05 pc 000000000007f03c  /system/lib64/libc++.so (demangling_terminate_handler()+224)
873:[   10.935010] init:   #06 pc 000000000007f50c  /system/lib64/libc++.so (std::__terminate(void (*)())+12)
874:[   10.936932] init:   #07 pc 00000000000827d4  /system/lib64/libc++.so (__cxa_rethrow+236)
875:[   10.938694] init:   #08 pc 00000000000fd13c  /system/lib64/libc++.so (std::__1::__fs::filesystem::detail::ErrorHandler<void>::report(...))
876:[   10.941565] init:   #09 pc 00000000000fd028  /system/lib64/libc++.so (std::__1::__fs::filesystem::detail::create_file_status(...))
877:[   10.944906] init:   #10 pc 0000000000105818  /system/lib64/libc++.so (std::__1::__fs::filesystem::__status(...))
878:[   10.947597] init:   #11 pc 00000000000f5fd0  /system/bin/init (android::init::(anonymous namespace)::SetMountProperty(android::init::MountHandlerEntry const&, bool)+298)

~ z1xus fish
~  slot-b ~/sv3-backup/slot-b  
```

init asks "does this file exist?". the phone's 2021 security rules dont let init look at that file. android 16 asks in a way that crashes instead of just getting a "no", nobody catches the crash, and init dies. when init dies, the phone reboots. and the security rules were set to deny it silently, so nothing ever showed up in a log

the test fix was one line in the security rules that lets init through. it went right past the crash on the first try

## the dsu detour

with that fix in, i installed the whole thing through dsu. it still took a couple of my own bugs before it would boot

while that was rebuilding i finally vented about it

a few minutes later it fully booted. android 16, may 2026 security patch. on a phone its manufacturer said would never see android 12

![a fluffy white cat lunging straight at the camera](https://z1x.us/blog/four-hundred-bucks-of-e-waste/cat-booted.webp)

## better than kapsys

copying kapsys's apps over was never really an option. some of them crashed on android 16 until i patched them, and the keypad magic lived in their patched android anyway, which wasnt coming with me. so i wrote one small app that does the keypad, the home screen and the keyboard. the home screen uses 20 mb of ram, kapsys's took 63

kapsys faked talkback swipes from inside android, so i tried faking keys in the android emulator first. right arrow: talkback moves. right arrow again: nothing. again. nothing. talkback can tell a fake key from a real one and just ignores the fake ones ![:tabbythumbsup:](https://z1x.us/blog/four-hundred-bucks-of-e-waste/tabbythumbsup.webp ":tabbythumbsup:")

what it doesnt ignore is an accessibility app grabbing its cursor and moving it by hand, which is what mine does now

after that it was a night of living on the phone and fixing whatever annoyed me

permission prompts went first. she has no idea what those are. no app can press "allow" for her, android makes very sure of that, but my app lives inside the rom and hands every app everything the moment its installed. you might say thats a privacy nightmare. i'd say its an accessibility feature, and the only person who installs apps on that phone is me anyway. the lock screen went too, kapsys didnt have one either

telegram ignored the middle key on its contact list, because those rows only react to touch. wow. great accessibility, telegram. now the middle key just pokes the middle of the row like a finger would

talkback kept speaking english on a ukrainian phone. no speech engine ships a voice for her exact language and region combo, talkback asked for one anyway, got nothing, and fell back to english without telling anyone. one setting. then a ukrainian voice and automatic language detection on top, because the default voice reading ukrainian sounds terrible

i also decided to write a help section for her. it went through several versions, until it was so simple it even explained which side is left and which is right

she has no idea what the fuck most notifications are, and she doesnt care either. anything from android itself, the play store, play protect or the google app now dies the second it pops up. the stock sounds were ass too, so the ringtone, notification and alarm got soft ones

and the shortcut tone. holding both volume keys turns talkback on and off, and android plays a sound when you do it. a very loud sound. WHY IS IT SO LOUD. (ok it sounds tame here. on her phone, at full volume, past midnight, it really wasnt) i made it quieter four times before i realised i was editing a file that wasnt being played at all. android was playing the default notification sound the whole time. swapped that one too. its basically silent now

## flashing it for real

dsu was android on a free trial: one wrong reboot and she was back on kapsys. at 3am on friday night i stopped testing, took one last backup and flashed it for real. it booted first try, and nothing crashed

i flashed twice more that night and her data stayed. the second time i swapped talkback for a copy i patch myself, because it announced "screen off" and "device unlocked" every time she touched the phone. an overlay would have been the clean way to change that, and talkback said no, those two strings arent on its short list of things phone makers may touch. fine. the build script now blanks them straight inside talkback's resource file and signs it with my own key, which also locks the play store out of updating it. i was scared android would wipe talkback's settings over the new signature. it logged "signature changed; retaining data" and moved on

## what got better

apps that crashed on kapsys, whatsapp included, just open now. the whole phone is way faster, and with the bloat gone theres actually ram left for the apps she uses. and although i never benchmarked the battery, she says it lasts longer now, and im taking her word for it

on kapsys we shared her location through google. a new rom felt like a good time to drop that, and now her phone reports to my own [owntracks](https://owntracks.org) server instead, every fifteen minutes or every hundred meters, and i can ask it where she is whenever i want

## loose ends

when she wakes the phone with the time row selected, talkback says the time, then two seconds later reads the whole row again, time included. i threw away three workarounds. eventually i want it fixed properly, but i think kapsys had the same problem anyway

bluetooth crashed 31 times after i turned on bluetooth scanning for location. scanning is off and bluetooth is untested. she doesnt use it, but it would be nice to fix anyway. and its still a debug build with a backdoor for me, with init keeping that one security exception

## the assistant button

she presses it a lot. on stock it only ever opened google's assistant, because apparently reading one android setting was too much engineering for a €400 phone. on mine it follows whatever you set as default, so obviously i tried chatgpt first. it cant even set a timer, which is like half of what she uses it for. so its back on gemini

she still asks it questions instead of me. at least now it doesnt crash halfway through the answer
