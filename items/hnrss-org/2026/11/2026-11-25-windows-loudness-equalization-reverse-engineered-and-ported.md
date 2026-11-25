---
title: Windows Loudness Equalization, reverse-engineered and ported to PipeWire
link: https://github.com/xoxorwr/pw-loudness-eq
source: hnrss-org
published: 2026-11-25T11:34:25Z
updated: 2026-11-25T11:34:25Z
first_seen: 2026-11-25T20:07:11.161777435Z
authors:
- ThrowNotAway
content: extracted
html: 2026-11-25-windows-loudness-equalization-reverse-engineered-and-ported.html
remote_preview:
  url: https://opengraph.githubassets.com/0743e93c2a19b4a2785fa754fce7546f904ebeac55f11f07029d9df3d4abfa35/xoxorwr/pw-loudness-eq
  alt: Windows Loudness Equalization, reverse-engineered and ported to PipeWire - xoxorwr/pw-loudness-eq
---

Windows' Loudness Equalization for PipeWire, reverse engineered from `WMALFXGFXDSP.dll`. It evens out the volume of each application, so quiet and loud parts play at a similar level.

[![The Loudness Equalization window](https://github.com/xoxorwr/pw-loudness-eq/raw/master/docs/screenshot.png)](https://github.com/xoxorwr/pw-loudness-eq/blob/master/docs/screenshot.png)

- **[`modules/loudness/`](https://github.com/xoxorwr/pw-loudness-eq/blob/master/modules/loudness)**: the PipeWire module and the DSP plugin. It processes every app's playback stream.
- **[`modules/limiter/`](https://github.com/xoxorwr/pw-loudness-eq/blob/master/modules/limiter)**: the output limiter of Windows' audio engine (`AudioEng.dll`), on every playback device. It keeps the boost from clipping.
- **[`app/`](https://github.com/xoxorwr/pw-loudness-eq/blob/master/app)**: an optional window (GTK3) to turn it on or off, globally or per app, and set the release time.

## Install

Requires PipeWire ≥ 1.4, and GTK 3 for the window.

```shell
# Arch:   sudo pacman -S pipewire gtk3 pkgconf make gcc
# Debian: sudo apt install libpipewire-0.3-dev libspa-0.2-dev libgtk-3-dev pkg-config make gcc
make
sudo make install
systemctl --user restart pipewire pipewire-pulse wireplumber
```

To install only one part, use `make -C modules/loudness`, `make -C modules/limiter` or `make -C app`. To uninstall, run `sudo make uninstall` and restart the same services.

## Usage

Open **Loudness Equalization** from your application menu, or run `pw-loudness-eq`.

The settings live in a PipeWire metadata object, so they can also be scripted:

```shell
pw-metadata -n loudness-eq 0 enabled false                               # turn off
pw-metadata -n loudness-eq 0 release 6                                   # release time, 2 (fast) to 7 (slow)
pw-metadata -n loudness-eq 0 disabled-apps '["Discord"]' Spa:String:JSON  # leave these apps alone
pw-metadata -n loudness-eq                                               # show settings
```

Changes apply immediately and are kept across restarts.

## Configuration

Defaults are in `/usr/share/pipewire/pipewire.conf.d/60-loudness-eq.conf`. To override them, copy it to `~/.config/pipewire/pipewire.conf.d/`.

| Arg            | Default |                                                                                                                     |
| -------------- | ------- | ------------------------------------------------------------------------------------------------------------------- |
| `enabled`      | `true`  | Start enabled                                                                                                       |
| `release`      | `4`     | Windows "Release Time", 2 (fast) to 7 (slow). 4 is the Windows default                                              |
| `stream.rules` |         | Never process some apps: `[ { matches = [ { application.name = "Discord" } ] actions = { loudness-eq = false } } ]` |

Settings changed at runtime are saved in `~/.local/state/pw-loudness-eq/state.conf` and take precedence over this file.

## How it works

The loudness module sets `audioconvert.filter-graph.90` on each app's playback stream, so the plugin runs inside that stream, before its volume and before mixing, like a Windows stream effect.

The limiter module does what Windows' audio engine does on every playback device: after the mix and the software volume, it runs the output through a look-ahead limiter. It's always on, independent of Loudness Equalization, as on Windows.

Not processed: JACK clients, streams with more than 8 channels, and Flatpak apps that use PipeWire directly (those going through PulseAudio are processed).

### The algorithm

A broadband automatic gain control. Despite the name, it doesn't equalize the spectrum.

1. Frame size N = 2^⌈log2(fs/100)⌉ (512 at 44.1/48 kHz), with 50% overlap.
2. Each channel is windowed with `w[n] = 16384·(1 − cos(2π(n+½)/N)) / (N/2)` and run through a real FFT to get the power spectrum.
3. Power is summed into 25 bands with edges at 100, 200, 300, 400, 510, 630, 770, 920, 1080, 1270, 1480, 1720, 2000, 2320, 2700, 3150, 3700, 4400, 5300, 6400, 7700, 9500, 12000, 15500 and 24000 Hz. Channels are summed together.
4. Loudness `L = 1.72 · Σ max(E_b − T_b, 1e-32)^0.25`, with T = `1000, 100, 31.6, 10, 3.16, 2, 1, 1, .316, .316, 1×11, 4, 10, 100, 1000`.
5. Target gain `= min((1300/L)^1.6, 10)`, so at most +20 dB.
6. When the target is lower than the current gain, the gain jumps to it instantly. Otherwise it moves toward it by `w = (N/128)·(44100/fs)·0.1^min(r,3)·0.316^(min(r,7)−min(r,3))` per block. At r = 4 that's a time constant of about 4.6 s.
7. The gain is crossfaded linearly across each half-frame. Latency is N samples.

The boost can push pure tones such as a whistle well above full scale; the limiter catches that.

### The limiter

Ported from `CAudioLimiter` in `AudioEng.dll`. `audiodg.exe` puts it on every render device, after its effects, before the device.

1. The output is delayed by L frames (64 at 44.1/48 kHz; 16, 32 or 128 at other rates) to look ahead. That's the device's added latency.
2. When a frame entering the delay peaks above the tracked peak (initially 0.985), the gain ramps down linearly over those L frames to `0.985 / peak`, reaching it as that frame leaves. All channels share one gain.
3. The gain only recovers once the signal peaks at half the tracked peak or less: it rises by `e^(2.205/fs)` per frame (about 19 dB/s) while the tracked peak decays by the same factor.

On Windows the device's software volume comes before the limiter; in PipeWire it comes after the device's filter graphs. So the plugin gets the volume of each channel (kept in sync by the module), measures peaks on the signal as it will be after the volume, and applies only its gain. Once PipeWire applies the volume, the result is the same as Windows'. Hardware volume comes after the limiter on both.

## Development

```shell
make check                                              # DSP and plugin tests, both modules
modules/loudness/build/leq-wav [-r release] in.wav out.wav       # process a file offline
```
