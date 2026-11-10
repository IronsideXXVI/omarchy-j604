# Omarchy on the MacBook Pro 14-inch (M4, 2024): J604 bring-up

This repository is the working set for running [Omarchy](https://github.com/omacom/omarchy) on the Apple MacBook Pro 14-inch with the base M4 chip (Mac16,1, board J604, SoC T8132, the same SoC as the M4 MacBook Air J713/J715). It's part of the Omarchy for Apple Silicon effort, and the J604 is a daily machine that's available for testing.

## Principle

The J604 runs the official Omarchy **edge** channel (switched with the official `omarchy-channel-set edge` on 2026-11-09): `omarchy-dev` and `omarchy-settings-dev` built from the `quattro` branch, plus the official `omarchy-mac`. This repository carries only what the stock packages lack for this Mac:

- the hardware enablement (kernel, boot chain, Mesa admission, audio profiles, firmware extraction, initramfs and unlock-prompt fixes, Touch ID eligibility, the Touch ID library update the 7.3 kernel's Secure Enclave interface needs, and the VA-API driver alias the 7.3 kernel's renamed GPU driver needs);
- two userland additions:
  - the keyboard-backlight series in `omarchy-dev` / `omarchy-settings-dev` (the backlight comes on with activity, goes off after 10 s idle, Ctrl+F1/F2 change it, and Super+K leaves out keys a MacBook doesn't have);
  - built-in panel defaults: `omarchy-settings-dev` reads a `panel` line from the platform package, and `omarchy-mac` names this Mac's panel there (10-bit, Display P3). A setting in the user's `monitors.lua` still wins.

Everything else is stock. Each piece is a patch on top of an official recipe, built with a local `pkgrel`. Earlier userland changes that are not installed are listed in [`not-installed/README.md`](not-installed/README.md).

It uses the official chain only: the [linux-aurora](https://github.com/omacom/linux-aurora) kernel with a J604 patch series, omacom [m1n1](https://github.com/omacom/m1n1) and U-Boot, the official installer, Omarchy edge, and normal `omarchy update`.

Changes go to the official repositories as reviewed pull requests, one topic at a time, in whatever shape the team prefers. Until each one lands, this repository holds the full set so anyone can read and rebuild it. Much of the kernel work is other people's: carried with its authorship, or already in Aurora's 7.3 base (see Credits and `CREDITS.md`).

## Status

Only what's measured on the machine is listed as working. The full per-block status, with labels and dates, is in [`STATUS.md`](STATUS.md).

| Area | Status (2026-11-10) |
| --- | --- |
| Boot chain (m1n1, U-Boot, Limine, disk unlock), storage, keyboard and trackpad, USB, battery, lid | Working |
| CPU | Working: 10 cores, cpufreq and cpuidle (performance cores up to 4.46 GHz requested, efficiency 2.89 GHz); one busy performance core runs at about 4.36 GHz and two to four at about 3.9 GHz, as under macOS with all cores loaded; since 1.723 the reported frequency is the reached one |
| GPU | Accelerated (OpenGL 4.6 / Vulkan 1.4); idle GPU power 0.041 W on 1.710, 0.076 W on 1.711 |
| Internal display | 3024x1964 at 120 Hz, scale 2, 10-bit (XRGB2101010) and Display P3 as package defaults, with no user setting. The panel link is most likely 8 bits per channel with dithering (not yet verified). VRR stays off, as in upstream Omarchy |
| Glassy bar | Stock holds about 59 fps on this 120 Hz panel. 119 fps needs the change in [omacom/omarchy#18176](https://github.com/omacom/omarchy/pull/18176), which is not carried here |
| Smoothness at 120 Hz | Both fixes on by default since 1.718: a full-window animation runs at 120 fps (about 90 before), scrolling at 120 fps, and 24, 30 and 60 fps video in Chromium stays on its cadence in our tests. YouTube fullscreen is not fully even yet (1080p60: 1.6-7.2 % of frames off cadence, 23 % before; see `STATUS.md`) |
| Camera | Working, tuned: 1080p and 720p capture with the sensor tuning file loaded, seen by stock apps and Chromium; colour not yet judged by eye |
| Wi-Fi and Bluetooth | Working |
| Audio | Speakers (with speaker protection), headphone jack and the three-microphone array working |
| Media | Hardware decode (H.264, HEVC, VP9) working in stock mpv through VA-API, with kernel 1.704 and the VA-API driver alias (checked at 1080p, and 4K for H.264; 10-bit HEVC decodes through VA-API as real 10-bit in short checks; AV1 not yet); JPEG hardware encode and decode working (decode since 1.728); H.264 and HEVC hardware encode working through stock FFmpeg since 1.722, and the stock screen recorder records in hardware since 1.726 (10-bit HEVC works through V4L2 directly; stock FFmpeg cannot send 10-bit frames to V4L2 encoders) |
| Touch ID | Working for sudo, polkit and the lock screen |
| Keyboard backlight | Working with the local `omarchy-dev` / `omarchy-settings-dev` builds |
| Neural Engine | Working: on by default (`/dev/accel/accel0`), runs programs bit-exact and powers off when idle (measured on 1.711) |
| Power | Suspend (s2idle) working again since 1.710 (it was refused on 1.705-1.708). Idle on AC is about 10.5 W with the stock edge `performance` power profile (see `STATUS.md`). Deep sleep in progress |
| In progress | Deep sleep, camera colour, fullscreen video cadence, Thunderbolt/USB4, external displays and HDMI |

## Install

The J604 runs the official Omarchy for Apple Silicon install on the edge channel. The stock packages can't boot this Mac yet (see `STATUS.md`), so the J604 builds of the packages below replace the official ones through their local `pkgrel`. Any official update with a higher version replaces them again.

There are no prebuilt packages in this repository. To rebuild a component, either apply its patches to the source commit on line 1 of its `BASE` file, or build its recipe:

1. Take the official recipe named on line 3 of `BASE`.
2. Copy this directory's `recipe/` files over it.
3. Run `makepkg`.

Until these changes land in the official packages, these builds target the J604 only.

## Versions

The J604 builds carried here, as installed on the machine on 2026-11-09 (checked with `pacman -Q`):

| Package | Official base | Local pkgrel |
| --- | --- | --- |
| `linux-aurora`, `linux-aurora-headers` | 7.3.0.aurora1, aurora-silicon/linux `integration/aurora` (`6ff31cbfa2`), 229 patches | `1.729` |
| `m1n1-aurora` | 1.6.1.aurora22, omacom/m1n1 (`bc79acc434`), 38 patches | `1.610` |
| `uboot-aurora` | 2026.07.aurora6, aurora-silicon/u-boot (`d30213eadc`) | `1.604` |
| `mesa-aurora`, `vulkan-aurora` | 1:26.3.0_devel.20261031, omacom/mesa (`a196911147`) | `1.8133` |
| `alsa-ucm-conf-aurora` | 10.aurora3, aurora-silicon/alsa-ucm-conf-asahi (`b9b2d0c82c`) | `1.605` |
| `aurora-audio` | 4.1.aurora3, aurora-silicon/asahi-audio (`06aec67a8a`) | `3.607` |
| `speakersafetyd` | 2.0.1, AsahiLinux/speakersafetyd | `1.605` |
| `asahi-fwextract` | 0.9.2, asahi-alarm/asahi-alarm-installer | `1.609` |
| `asahi-scripts` | 20261023, asahi-alarm/asahi-scripts | `1.2` |
| `plymouth` | 26.134.222, Plymouth (Arch Linux recipe) | `3.604` |
| `libfprint-git` | 1:1.94.100.r10.g6f9479c, libfprint (`6f9479c3d5`), omacom recipe | `5.604` (accepts Secure Enclave interface v4 and v5) |
| `libva-v4l2_request-avd` | 1.3.r11.r169.gc653eaa, omacom/libva-v4l2_request (`c653eaa623`), omacom recipe | `1.605` (adds the `aurora_drv_video.so` alias and VA-API H.264/HEVC encoding on the Apple video encoder; the alias is open as [omacom/omarchy-pkgs#2375](https://github.com/omacom/omarchy-pkgs/pull/2375), and its official `-2` will replace this build) |
| `gpu-screen-recorder` | 6.1.7, gpu-screen-recorder (Arch Linux recipe, built for aarch64) | `1.604` (captures monitors on the Apple display controller) |
| `omarchy-mac-boot` | 20261104, omacom/omarchy-mac-pkgs (`7e50e937b5`) | `1.616` |
| `omarchy-mac` | 0.2.3, omacom/omarchy-mac-pkgs (`ea0097a523`) | `2.12` (adds the panel line) |
| `omarchy-dev` | 4.0.0.r7641.gf97d3e7 (edge), omacom/omarchy `quattro` (`f97d3e7dc8`) | `1.1` (keyboard-backlight patches only) |
| `omarchy-settings-dev` | 4.0.0.r7641.gf97d3e7 (edge), omacom/omarchy `quattro` (`f97d3e7dc8`) | `1.1` (keyboard-backlight patches + built-in panel defaults) |

Also installed, unchanged from the official repositories: Hyprland 0.57.1 and the rest of the edge userland; the Neural Engine stack (`omarchy-ane`, `omarchy-ane-dkms`, `mil-hwx-compiler`, `omarchy-mac-ml`), which we no longer rebuild. `j604-keyring` is a local keyring package that only signs the builds above; it has no source changes to publish.

Since 2026-11-09 the kernel is on the Aurora 7.3 base, and `boot.bin` holds only the J604 device tree, which `omarchy-mac-boot` 1.616 now selects on its own (see `STATUS.md`, which lists what 1.703 to 1.729 changed over 1.617).

## Layout

| Directory | What | Upstream it applies to | License |
| --- | --- | --- | --- |
| `kernel/` | `git format-patch` series (apply in `series` order); `BASE` names the commit it applies to and the official recipe | aurora-silicon/linux `integration/aurora` (built with the omacom `linux-aurora` recipe) | GPL-2.0 (see `kernel/COPYING`) |
| `m1n1/` | patches and the `m1n1-aurora` recipe | omacom/m1n1 | MIT |
| `uboot-aurora/` | J604 board patch and the `uboot-aurora` recipe | aurora-silicon/u-boot | GPL-2.0+ |
| `mesa/` | T8132 patch, the "aurora" driver-name patch and the `mesa-aurora` recipe (builds `mesa-aurora` and `vulkan-aurora`) | omacom/mesa | MIT (main code) |
| `alsa-ucm-conf-aurora/` | J604 headphone jack, microphone and speaker UCM | aurora-silicon/alsa-ucm-conf-asahi | BSD-3-Clause |
| `aurora-audio/` | J604 speaker graph and microphone, plus upstream audio fixes | aurora-silicon/asahi-audio | MIT |
| `speakersafetyd/` | J604 speaker protection profile | AsahiLinux/speakersafetyd | MIT |
| `asahi-fwextract/` | camera tuning firmware extraction for the J604; keeps the BCM4388 rev C2 Wi-Fi firmware | asahi-alarm/asahi-alarm-installer | MIT |
| `libfprint-git/` | Touch ID on the 7.3 kernel: the Apple Secure Enclave driver accepts biometric interface v4 and v5 (two upstream Aurora patches as one) | libfprint (omacom recipe) | LGPL-2.1-or-later |
| `libva-v4l2_request-avd/` | VA-API on the 7.3 kernel: the recipe adds the `aurora_drv_video.so` alias (upstream aurora-silicon/alarm-aurora aedcf240ca91). `patches/` holds that recipe diff and, since 1.605, a driver patch that adds VA-API H.264 and HEVC encoding on V4L2 stateful encoders such as the Apple video encoder | omacom/omarchy-pkgs recipe, omacom/libva-v4l2_request | GPL-3.0-or-later |
| `gpu-screen-recorder/` | the stock recorder captures monitors on the Apple display controller (a KMS-only device with no render node, handled like DisplayLink's) | gpu-screen-recorder (Arch Linux recipe) | GPL-3.0-only |
| `asahi-scripts/` | adds the `apple_nvmem_spmi` and `dwc3-apple` modules to the initramfs | asahi-alarm/asahi-scripts | MIT |
| `plymouth/` | SimpleDRM recognized by driver name, so the disk unlock prompt opens on the panel | Plymouth | GPL-2.0 |
| `omarchy-mac-boot/` | boot package fixes for the J604, and `update-m1n1` builds `boot.bin` with only the running Mac's own device tree | omacom/omarchy-mac-pkgs | MIT |
| `omarchy-mac/` | keyboard backlight idle timeout, Apple F-row key names, Touch ID on the J604, the 14-inch MacBook Pro panel line (10-bit, Display P3) | omacom/omarchy-mac-pkgs | MIT |
| `omarchy-dev/`, `omarchy-settings-dev/` | the keyboard-backlight series on the edge recipes (the same four patches in both); `omarchy-settings-dev` also carries the built-in panel defaults (`disp-0201`) | omacom/omarchy | MIT |
| `not-installed/` | earlier userland changes that are not installed, and where they are | | |

Every component directory has the same shape:

- `BASE`: line 1 is the upstream source and the commit or tag the patches apply to; line 2 is the package version and local pkgrel; line 3 is the official recipe and commit the J604 recipe changes.
- `patches/`: the J604 changes.
- `recipe/`: the J604 `PKGBUILD` and the same patches. Every other file of the recipe is unchanged from the official recipe named in `BASE`.
- The upstream license text.

`kernel/` has `BASE`, `series`, `patches/` and the kernel's license files.

The kernel series still carries one test-only setting: the J604 device-tree file that patch 0157 adds (`t8132-j604-lab.dtsi`) enables the CPU performance-counter (PMU) node for a counter measurement, as the test-only patch 0616 did in 1.617. It is published because the installed kernel carries it, and it is not proposed upstream as it is; the node is enabled for good only after that measurement passes.

Each patch keeps its original author. Patch 0157 ("j604: integrate the J604 series onto Aurora 7.3") folds the part of the 1.617 series that Aurora's 7.3 base does not already contain into one commit; the authors of that work are those of the 1.617 series (see the history of `kernel/`). Apart from the pull requests and issues listed in `UPSTREAM.md`, these patches have not been submitted upstream yet.

No Apple firmware, binaries or device dumps are in this repository. The firmware the machine needs is extracted on the machine itself at install time, as on every Omarchy Mac.

## Upstreaming

What goes to which upstream, in what order, and what's already open is in [`UPSTREAM.md`](UPSTREAM.md). Nothing has merged yet. Open so far:

- [omacom/omarchy#18176](https://github.com/omacom/omarchy/pull/18176): blur the Glassy bar from the cached wallpaper blur (59 to 119 fps on this panel); reopened; the same change as [#18191](https://github.com/omacom/omarchy/pull/18191)
- [omacom/omarchy#18228](https://github.com/omacom/omarchy/pull/18228): retry the kyber scheduler write while a sibling NVMe namespace switches
- [omacom/omarchy#18229](https://github.com/omacom/omarchy/issues/18229) (issue): `omarchy-mac`'s mpv default never installs because `omarchy-settings` owns `/etc/mpv/mpv.conf`; linked from [omacom/omarchy-mac-pkgs#66](https://github.com/omacom/omarchy-mac-pkgs/issues/66). An M1 user's measurements there showed that the stock default (`hwdec=auto-safe`) is the right one on the M1; our reply (2026-11-09) agrees and adds M4 numbers
- [omacom/linux-aurora#432](https://github.com/omacom/linux-aurora/issues/432) (issue): J604 test results
- Closed without merging: [omacom/omarchy#18177](https://github.com/omacom/omarchy/pull/18177) (keybindings menu leaves out absent keys)

The Aurora kernel has merged DjDeveloperr's J604 / M4 device trees (aurora-silicon/linux #1142 and follow-ups). We rebase onto them and send only what goes beyond them.

## Credits

This builds on the work of [Aurora Silicon](https://github.com/aurora-silicon) (the M4 kernel, including DjDeveloperr's M4 device trees and drivers), the [omacom](https://github.com/omacom) Omarchy for Apple Silicon team, and the [Asahi Linux](https://asahilinux.org) project, whose kernel, m1n1, Mesa, audio and installer work everything here stands on. `CREDITS.md` counts each author's patches.

## License

Each part keeps the license of the project it changes; see `LICENSE.md`.
