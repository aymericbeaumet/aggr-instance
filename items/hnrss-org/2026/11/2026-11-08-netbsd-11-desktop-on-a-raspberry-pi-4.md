---
title: NetBSD 11 desktop on a Raspberry Pi 4
link: https://neongd.com/articles/netbsd11-rpi4.gmi
source: hnrss-org
published: 2026-11-08T12:07:17Z
updated: 2026-11-08T12:07:17Z
first_seen: 2026-11-10T19:38:28.320395710Z
authors:
- jaypatelani
content: extracted
html: 2026-11-08-netbsd-11-desktop-on-a-raspberry-pi-4.html
---

Update: I noticed that the post appeared on Hacker News. It got more publicity than I thought it would. But maybe it's a good thing. It shows that there is an increased interest in NetBSD. And I suppose it's in part because people are looking for AI-free alternatives and there aren't many options out there.

I found some of the comments on HN valid, and updated the guide at places (like writing the image to the SD card).

---

Now that I moved this capsule to a VPS, my Raspberry Pi became available for other use. NetBSD is the only open-source operating system in development that I know of, that doesn't include AI slop. At least for now? I decided to give it a go and try to install it on the Pi. How hard can it be? It turns out, not that much. I made a small guide on how I did it, maybe it helps someone.

I assume you're preparing the SD card for the Pi on Linux.

Get the latest image for arm64:

[https://nycdn.netbsd.org/pub/NetBSD-daily/netbsd-11/latest/evbarm-aarch64/binary/gzimg/arm64.img.gz](https://nycdn.netbsd.org/pub/NetBSD-daily/netbsd-11/latest/evbarm-aarch64/binary/gzimg/arm64.img.gz)

Get the latest Raspberry Pi UEFI firmware:

[https://github.com/pftf/RPi4/releases](https://github.com/pftf/RPi4/releases)

Extract the NetBSD image:

```
$ gunzip arm64.img.gz
```

Insert the microSD card in your laptop (I used a cheap USB SD card reader).

Find out which device it is:

```
$ lsblk
```

In my case it was 'sda'.

Write the image to the SD card:

```
$ sudo dd if=arm64.img of=/dev/sda bs=4M status=progress conv=fsync
```

Or if you fancy graphical tools, you can also use one like Balena's Etcher:

[https://etcher.balena.io/#download-etcher](https://etcher.balena.io/#download-etcher)

In the UI, select the "Flash from file" option and find the previously extracted NetBSD arm64 image. For destination select the SD card (make sure it's really the SD card!). Flash the image (give sudo access).

Now extract the firmware:

```
$ mkdir rpi_firmware
$ unzip -d rpi_firmware RPi4_UEFI_Firmware_v1.53.zip 
```

Create a directory for the NetBSD boot partition mount:

```
$ sudo mkdir -p /mnt/netbsd-boot
```

Mount the SD card's first partition there:

```
$ sudo mount -t vfat /dev/sda1 /mnt/netbsd-boot
```

Copy all the files from the rpi\_firmware directory to /mnt/netbsd-boot (overwrite everything).

```
$ sudo cp -r rpi_firmware /mnt/netbsd-boot/
```

This step is needed so the UEFI BIOS can actually boot the NetBSD partitions on the SD card.

Unmount the partition:

```
$ sudo umount /mnt/netbsd-boot
```

Now you can insert the SD card into your Raspberry Pi.

Turn it on and press ESC to enter the UEFI setup menu.

Open Boot Maintenance Manager -> Boot Options -> Change Boot Order, move the SD card to the top (with the + key), then choose Commit Changes and Exit.

NetBSD should now boot. It will start with resizing the root filesystem.

When finished, you can login with "root".

Set a password with 'passwd'.

It's recommended to create a user right away, but you can do that later too:

```
$ useradd -m -G wheel YOUR_USER
$ passwd YOUR_USER 
```

## Make WiFi work

Now let's connect to the WiFi.

If we type 'ifconfig', we can't see the wireless adapter there. That's a problem.

In dmesg however we can see that the kernel recognized it and found its driver, but couldn't autoconfigure it:

```
$ dmesg | grep -iE 'bwfm|firmware|sdio|wifi'
```

bwfm0: Found firmware file: brcmfmac43455-sdio.bin

bwfm0: autoconfiguration error: NVRAM file not available

Let's fix this:

```
$ cd /libdata/firmware/if_bwfm
$ ln -s brcmfmac43455-sdio.raspberrypi,4-model-b.txt "brcmfmac43455-sdio.Raspberry Pi 4 Model B.txt"
```

Reboot.

ifconfig now shows the bwfm0 device. Let's configure our WiFi access point.

Edit the rc.conf:

```
$ vi /etc/rc.conf
```

Add this to the bottom:

```
ifconfig_bwfm0="up"
wpa_supplicant=YES
```

Edit the wpa\_supplicant.conf:

```
$ vi /etc/wpa_supplicant.conf
```

Add this to the bottom (pay attention to case sensitivity):

```
network={
    ssid="Your_WiFi_SSID_name"
    psk="YourWifiPassword"
    scan_ssid=0
}
```

(Quick Vi help: you enter the interactive (edit) mode by pressing 'i'. You delete a character by leaving the interactive mode with ESC and pressing 'x' on the character you want to delete. You save the file by pressing ':' outside the interactive mode to enter the command mode and typing 'wq' \<enter>, as for write and quit.)

If everything went well, after reboot (or wpa supplicant restart) you should be online.

ifconfig should show 'status: active' and the IP address it received on dhcp.

From this point on it's entirely up you how you want to configure the OS and what you want to install. Next are some things I did to make it more usable for me.

## Installing software

NetBSD has pkg\_src, which is way of installing and building packages from source, and pkgin, which is a way to install prebuild binary packages.

Edit ~/.profile and remove the comment from the export line setting PKG\_PATH, so we can install binary packages.

Then do a logout with 'exit' and log in again.

Let's add pkgin, the binary package manager:

```
$ pkg_add pkgin
```

Now install some software. For example:

```
$ pkgin update
$ pkgin install nano fastfetch doas
```

Doas works in a simiar way as sudo, to execute admin commands as a regular user.

Create the doas config with 'vi /etc/pkg/etc/doas.conf' and add this line:

```
permit persist YOUR_USER
```

## Set up X11

Enable the graphical login manager:

```
$ cp /etc/X11/xinit/xinitrc ~/.xinitrc
$ chmod u+w ~/.xinitrc
$ ln -s .xinitrc ~/.xsession
$ echo 'xdm=YES' >> /etc/rc.conf
$ service xdm start
```

Make sure that you create .xinitrc and .xsession in your user's home (and also in /root if you would ever want to login as root).

For more information see here:

[https://www.netbsd.org/docs/guide/en/chap-x.html](https://www.netbsd.org/docs/guide/en/chap-x.html)

Log in with your regular user (press enter after typing in the user name to enter the password field).

This will start NetBSD's built in ctwm window manager. It's very simple, but efficient.

## NsCDE - Not so Common Desktop Environment

If you want to make it actually usable though, I recommend NsCDE, a modern desktop environment in the style of CDE, based on fvwm:

[https://github.com/NsCDE/NsCDE](https://github.com/NsCDE/NsCDE)

First, install every depencency it needs. Let's do all this as root:

```
su -
pkgin install fvwm3 ksh93-1.0.10nb3 py313-yaml py313-qt5 py313-psutil coreutils xclip 
pkgin install gtail ImageMagick xdotool dunst stalonetray gsed xterm git xscreensaver-615nb2 
```

Make python3 available:

```
mkdir -p "$HOME/bin"
ln -s /usr/pkg/bin/python3.13 "$HOME/bin/python3"
export PATH="$HOME/bin:/usr/pkg/bin:/usr/pkg/sbin:/usr/X11R7/bin:$PATH"
python3 --version
```

(Do the same thing for your user's home too).

Create this folder as NsCDE might expect it:

```
mkdir -p /etc/xdg/menus
```

Get and build NsCDE:

```
cd /tmp
git clone https://github.com/NsCDE/NsCDE.git
cd NsCDE
./configure
```

Check for warnings. If any package is still missing, install it. Find the available packages with

```
pkgin search <packagename>
```

Then build NsCDE:

```
make
```

If everything went well:

```
make install
```

To make the shell more usable edit ~/.shrc both for root and your user and add these to the interactive mode settings:

```
set -E
set -o emacs
PS1='$PWD $ '
HISTFILE="$HOME/.sh_history"
HISTSIZE=1000
HISTAPPEND=1
export HISTFILE HISTSIZE HISTAPPEND
```

Now edit the ~/.xinitrc for your user (and for root if you ever would want to login as root) and change the exec line on the bottom to:

```
export PATH="$HOME/bin:/usr/pkg/bin:/usr/pkg/sbin:/usr/local/bin:/usr/X11R7/bin:$PATH
export ENV="$HOME/.shrc"
exec /usr/local/bin/nscde
```

Reboot and login with your user. NsCDE first launch setup should start, where you can configure the theme and integration with GTK and QT.

If you click the up arrow above the multimedia icon on the task bar you'll find a Volume Control menu option. But clicking it will give you the error message saying that you don't have a volumectrl application installed. Same with the File Manager. Let's fix this:

```
doas pkgin install xmix pcmanfm 
```

Edit ~/.NsCDE/NsCDE.conf and add this:

```
InfoStoreAdd volumectrl xmix
```

Replace the 'InfoStoreAdd filemgr true' with:

```
InfoStoreAdd filemgr pcmanfm
```

As a start we can also install a graphical web browser:

```
$ doas pkgin install dillo
```

Now you're probably getting a hang of it, have fun with your new obscure operating system!

## Screenshot:

[NetBSD 11 running NsCDE on a Raspberry Pi \[image, png, 250Kb\]](https://neongd.com/articles/img/netbsd11.png)
