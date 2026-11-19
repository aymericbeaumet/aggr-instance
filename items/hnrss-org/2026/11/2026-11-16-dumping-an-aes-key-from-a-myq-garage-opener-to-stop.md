---
title: Dumping an AES key from a MyQ garage opener to stop rebooting when offline
link: https://kusha.me/blog/stop_myq_homebridge_819lmb_rebooting/
source: hnrss-org
published: 2026-11-16T23:47:26Z
updated: 2026-11-16T23:47:26Z
first_seen: 2026-11-19T07:50:29.493483202Z
authors:
- kusha
content: extracted
html: 2026-11-16-dumping-an-aes-key-from-a-myq-garage-opener-to-stop.html
remote_preview:
  url: https://kusha.me/imgs/145d4d43b2f05e68a7ca.png
  alt: https://kusha.me/imgs/145d4d43b2f05e68a7ca.png
---

### Otherwise known as Chamberlain / LiftMaster 819LMB / MYQ-G0303 MyQ Home Bridge

![819LMB](https://kusha.me/imgs/145d4d43b2f05e68a7ca.png)

### The Problem

When isolated on an IoT VLAN with no WAN/internet access, the device constantly attempts to phone home to Chamberlain's cloud endpoints (presumably MQTT servers):

- `connect.myqdevice.com:8883`
- `connect1.myqdevice.com:8883`

If these domains are unresolved or sinkholed to a non-existent server / `0.0.0.0`, connection attempts hang and time out. Eventually, the internal watchdog reboots the device or its Wi-Fi module, causing it to go unavailable/unresponsive in HomeKit every 5–10 minutes.

### Naive/Semi-Solution

Provide the device DNS resolution for `connect.myqdevice.com` and `connect1.myqdevice.com` pointing to a local IP/service that will immediately refuse the connection (TCP RST). This is a half-baked solution: it stops the 5–10 minute crash loops, but still triggers a watchdog reboot every 30 minutes.

### 100% Fix: 30-Minute Watchdog & Local Cloud Spoofing

Unfortunately, there is a caveat to all of this. If the device doesn't successfully connect to the myQ cloud MQTT servers and receive a retained message (ping) back when subscribed to the appropriate topic, it reboots on a 30 minute timer. Meaning, every 30 minutes you have a small period of downtime where the homekit integration will be unresponsive.

To remediate this, you must dump the TLS-PSK AES-128 key on the device in order to setup your own MQTT server with the appropriate DNS. Basically, pretend you are the myQ MQTT server.

This is done by connecting to the Cortex-M4 CPU debug pads left conveniently by Chamberlain on the PCB.

![arduino debug](https://kusha.me/imgs/f2712a2c7f85e0ec8940.png)

To dump your device's TLS-PSK AES-128 key:

Requirements:

- Arduino (I used the Uno) + Arduino IDE
- 2 resistors between 220Ω to 1000Ω (I used a 560Ω)
  - This protects you from accidentally frying your 819lmb board from the Arduino's 5V pins
- Jumper wires
- Tape

- Take off the top shell (two screws)
- Find the CPU 10 pin debug pad. Pin 1 is marked with an arrow. See picture

![819lmb cpu debug pins](https://kusha.me/imgs/e7cbb6eb73d7ecaad914.png)

- Connect Pin 2 (SWDIO) -> Resistor -> D8 on the Arduino

- Connect Pin 4 (SWCLK) -> Resistor -> D9 on the Arduino

- Connect GND on the board (I used the power connector ground) -> GND on the Arduino

- Using the Arduino IDE, Compile/Upload the key dumper sketch: [Arduino Script](https://github.com/kushagharahi/myq-homebridge-jailbreak/blob/main/myq_aes_dumper/myq_aes_dumper.ino)

- Verify connections and power on the 819lmb

- Open the Serial Monitor at 115200 baud. Output should look something like this:

  ```
  =====================================================
  LiftMaster 819LMB Hardware Credentials Dumper
  =====================================================
  [*] Armed and waiting for probe contact on test pads...
      Pins: D8 (SWDIO) -> Pad 2, D9 (SWCLK) -> Pad 4, GND -> GND
  ........
  -----------------------------------------------------
  [+] Contact detected! ARM CoreSight IDCODE: 0x2BA01477
  [+] CPU Halted. PC=0x1F01A3AA, SP=0x107B90
  [*] Scanning flash space (0x1F000000 - 0x1F070000) for NVM signatures...
      Scanning 0x1F000000...
      Scanning 0x1F010000...
      Scanning 0x1F020000...
  [+] Discovered NVM function at 0x1F02F7EC (scan took 128866 ms)
  [+] Target nvm_rd_myq_serial  at: 0x1F02F7A8
  [+] Target nvm_rd_myq_aes_key at: 0x1F02F84C
  [+] Target stack and CPU state restored. Bridge resumed.

  =====================================================
  >>> EXTRACTION SUCCESSFUL! <<<
      Serial Number : <extracted_serial_number>
      AES-128 Key   : <extracted_aes_key>
  =====================================================

  Extraction complete. Probes may be safely disconnected.
  ```

#### Step 2: Run the Local Mosquitto Broker Container

Pull and run the Docker container with your extracted credentials

```
docker run -d -p 8883:8883 \
  -e MYQ_SERIAL=<extracted_serial_number> \
  -e MYQ_PSK=<extracted_aes_key> \
  ghcr.io/kushagharahi/myq-homebridge-jailbreak:latest
```

#### Step 3: Redirect DNS

In your local DNS server, point both domains to the IP of the machine running the container:

- `connect.myqdevice.com` → `<CONTAINER_HOST_IP>`
- `connect1.myqdevice.com` → `<CONTAINER_HOST_IP>`

#### Verification

- The green light should no longer be flashing, instead solid to indicate "cloud" connectivity
- The container logs should show something like

```
1794793990: New client connected from 10.42.0.1:32806 as <extracted_serial_number> (p4, c1, k360).
1794793990: Client <extracted_serial_number> negotiated SSLv3 cipher PSK-AES128-CBC-SHA
```

- Do a `GET` on `http://<device-local-ip>/jabout`, you should see `"internet_connected": "Yes",`.
  - Do another after 30 minutes and ensure your `powered_time` is over 30 minutes, meaning no reboots have occurred.
