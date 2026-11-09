# Omarchy on the MacBook Pro 14-inch (M4, 2024): J604 bring-up

This repository is the working set for running [Omarchy](https://github.com/omacom/omarchy) on the Apple MacBook Pro 14-inch with the base M4 chip (Mac16,1, board J604, SoC T8132, the same SoC as the M4 MacBook Air J713/J715). It's part of the Omarchy for Apple Silicon effort, and the J604 is a daily machine that's available for testing.

## Principle

The J604 runs the official Omarchy **edge** channel (switched with the official `omarchy-channel-set edge` on 2026-11-09): `omarchy-dev` and `omarchy-settings-dev` built from the `quattro` branch, plus the official `omarchy-mac`. This repository carries only what the stock packages lack for this Mac:

- the hardware enablement (kernel, boot chain, Mesa admission, audio profiles, firmware extraction, initramfs and unlock-prompt fixes, Touch ID eligibility);
- one userland addition: the keyboard-backlight series in `omarchy-dev` / `omarchy-settings-dev` (the backlight comes on with activity, goes off after 10 s idle, Ctrl+F1/F2 change it, and Super+K leaves out keys a MacBook doesn't have).

Everything else is stock. Each piece is a patch on top of an official recipe, built with a local `pkgrel`. Earlier userland changes that are not installed are listed in [`not-installed/README.md`](not-installed/README.md).

It uses the official chain only: the [linux-aurora](https://github.com/omacom/linux-aurora) kernel with a J604 patch series, omacom [m1n1](https://github.com/omacom/m1n1) and U-Boot, the official installer, Omarchy edge, and normal `omarchy update`.

Changes go to the official repositories as reviewed pull requests, one topic at a time, in whatever shape the team prefers. Until each one lands, this repository holds the full set so anyone can read and rebuild it. Most of the kernel series is other people's work, carried with its authorship (see Credits and `CREDITS.md`).

## Status

Only what's measured on the machine is listed as working. The full per-block status, with labels and dates, is in [`STATUS.md`](STATUS.md).

| Area | Status (2026-11-09) |
| --- | --- |
| Boot chain (m1n1, U-Boot, Limine, disk unlock), storage, keyboard and trackpad, USB, battery, lid | Working |
| CPU | Working: 10 cores at full clocks (performance 4.46 GHz, efficiency 2.89 GHz), cpufreq and cpuidle |
| GPU | Accelerated (OpenGL 4.6 / Vulkan 1.4). GPU idle power in progress |
| Internal display | 3024x1964 at 120 Hz, scale 2, 10-bit compositor scanout (XRGB2101010) through one user setting in `monitors.lua`. The panel link is most likely 8 bits per channel with dithering (not yet verified). P3 and VRR as panel defaults are built, not installed (pending) |
| Glassy bar | Stock holds about 59 fps on this 120 Hz panel. 119 fps needs the change in [omacom/omarchy#18176](https://github.com/omacom/omarchy/pull/18176), which is not carried here |
| Wi-Fi and Bluetooth | Working |
| Audio | Speakers (with speaker protection) and headphone jack working; microphones in progress |
| Media | H.264 hardware decode working; video encode in progress |
| Touch ID | Working for sudo, polkit and the lock screen |
| Keyboard backlight | Working with the local `omarchy-dev` / `omarchy-settings-dev` 1.3 builds |
| Neural Engine | Driver bound; not powered up on the T8132 yet (the driver stops before power-up by design), no `/dev/accel` |
| Power | Suspend (s2idle) on lid close working. Idle on AC is about 10.5 W with the stock edge `performance` power profile (see `STATUS.md`). Deep sleep in progress |
| In progress | GPU idle power, deep sleep, camera, microphones, Thunderbolt/USB4, external displays and HDMI |

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
| `linux-aurora`, `linux-aurora-headers` | 7.2.11.aurora15, omacom/linux-aurora `integration/aurora` (`b798f2b0c7`), 766 patches | `1.617` |
| `m1n1-aurora` | 1.6.1.aurora22, omacom/m1n1 (`bc79acc434`), 37 patches | `1.608` |
| `uboot-aurora` | 2026.07.aurora6, aurora-silicon/u-boot (`d30213eadc`) | `1.604` |
| `mesa-aurora`, `vulkan-aurora` | 1:26.3.0_devel.20261031, omacom/mesa (`a196911147`) | `1.8132` |
| `alsa-ucm-conf-aurora` | 10.aurora3, aurora-silicon/alsa-ucm-conf-asahi (`b9b2d0c82c`) | `1.605` |
| `aurora-audio` | 4.1.aurora3, aurora-silicon/asahi-audio (`06aec67a8a`) | `3.607` |
| `speakersafetyd` | 2.0.1, AsahiLinux/speakersafetyd | `1.605` |
| `asahi-fwextract` | 0.9.2, asahi-alarm/asahi-alarm-installer | `1.608` |
| `asahi-scripts` | 20261023, asahi-alarm/asahi-scripts | `1.1` |
| `plymouth` | 26.134.222, Plymouth (Arch Linux recipe) | `3.604` |
| `omarchy-mac-boot` | 20261104, omacom/omarchy-mac-pkgs (`7e50e937b5`) | `1.614` |
| `omarchy-mac` | 0.2.3, omacom/omarchy-mac-pkgs (`ea0097a523`) | `2.7` |
| `omarchy-dev`, `omarchy-settings-dev` | 4.0.0.r7634.gcbd0029 (edge), omacom/omarchy `quattro` (`cbd0029fdd`) | `1.3` (keyboard-backlight patches only) |

Also installed, unchanged from the official repositories: Hyprland 0.57.1 and the rest of the edge userland; the Neural Engine stack (`omarchy-ane`, `omarchy-ane-dkms`, `mil-hwx-compiler`, `omarchy-mac-ml`), which we no longer rebuild. `j604-keyring` is a local keyring package that only signs the builds above; it has no source changes to publish.

Coming next (not installed): a rebase of the J604 series onto aurora-silicon/linux `integration/aurora` 7.3.0 is in progress. `STATUS.md` lists what 1.617 added over 1.614.

## Layout

| Directory | What | Upstream it applies to | License |
| --- | --- | --- | --- |
| `kernel/` | `git format-patch` series (apply in `series` order); `BASE` names the commit it applies to and the official recipe | omacom/linux-aurora | GPL-2.0 (see `kernel/COPYING`) |
| `m1n1/` | patches and the `m1n1-aurora` recipe | omacom/m1n1 | MIT |
| `uboot-aurora/` | J604 board patch and the `uboot-aurora` recipe | aurora-silicon/u-boot | GPL-2.0+ |
| `mesa/` | T8132 patch and the `mesa-aurora` recipe (builds `mesa-aurora` and `vulkan-aurora`) | omacom/mesa | MIT (main code) |
| `alsa-ucm-conf-aurora/` | J604 headphone jack, microphone and speaker UCM | aurora-silicon/alsa-ucm-conf-asahi | BSD-3-Clause |
| `aurora-audio/` | J604 speaker graph and microphone, plus upstream audio fixes | aurora-silicon/asahi-audio | MIT |
| `speakersafetyd/` | J604 speaker protection profile | AsahiLinux/speakersafetyd | MIT |
| `asahi-fwextract/` | camera tuning firmware extraction for the J604 | asahi-alarm/asahi-alarm-installer | MIT |
| `asahi-scripts/` | adds the `apple_nvmem_spmi` module to the initramfs | asahi-alarm/asahi-scripts | MIT |
| `plymouth/` | SimpleDRM recognized by driver name, so the disk unlock prompt opens on the panel | Plymouth | GPL-2.0 |
| `omarchy-mac-boot/` | boot package fixes for the J604 | omacom/omarchy-mac-pkgs | MIT |
| `omarchy-mac/` | keyboard backlight idle timeout, Apple F-row key names, Touch ID on the J604 | omacom/omarchy-mac-pkgs | MIT |
| `omarchy-dev/`, `omarchy-settings-dev/` | the keyboard-backlight series on the edge recipes (the same four patches in both; both packages build from the same patched source) | omacom/omarchy | MIT |
| `not-installed/` | earlier userland changes that are not installed, and where they are | | |

Every component directory has the same shape:

- `BASE`: line 1 is the upstream source and the commit or tag the patches apply to; line 2 is the package version and local pkgrel; line 3 is the official recipe and commit the J604 recipe changes.
- `patches/`: the J604 changes.
- `recipe/`: the J604 `PKGBUILD` and the same patches. Every other file of the recipe is unchanged from the official recipe named in `BASE`.
- The upstream license text.

`kernel/` has `BASE`, `series`, `patches/` and the kernel's license files.

One kernel patch is a test-only change that the installed 1.617 build does ship: `0616-TEST-ONLY-arm64-dts-apple-t8132-j604-Enable-the-CPU-.patch` enables the CPU performance-counter (PMU) node on the J604 for a counter measurement. It is published because the installed kernel carries it, and it is not proposed upstream as it is; the node is enabled for good only after that measurement passes.

Each patch keeps its original author. Patches whose subject starts with "J604 local build" are carried only here so far. Patches made with an AI coding assistant say so in an `Assisted-by:` trailer, and pull requests disclose it per each repository's policy. Apart from the pull requests and issues listed in `UPSTREAM.md`, these patches have not been submitted upstream yet.

No Apple firmware, binaries or device dumps are in this repository. The firmware the machine needs is extracted on the machine itself at install time, as on every Omarchy Mac.

## Upstreaming

What goes to which upstream, in what order, and what's already open is in [`UPSTREAM.md`](UPSTREAM.md). Nothing has merged yet. Open so far:

- [omacom/omarchy#18176](https://github.com/omacom/omarchy/pull/18176): blur the Glassy bar from the cached wallpaper blur (59 to 119 fps on this panel); reopened; the same change as [#18191](https://github.com/omacom/omarchy/pull/18191)
- [omacom/omarchy#18228](https://github.com/omacom/omarchy/pull/18228): retry the kyber scheduler write while a sibling NVMe namespace switches
- [omacom/omarchy#18229](https://github.com/omacom/omarchy/issues/18229) (issue): `omarchy-mac`'s mpv default never installs because `omarchy-settings` owns `/etc/mpv/mpv.conf`; linked from [omacom/omarchy-mac-pkgs#66](https://github.com/omacom/omarchy-mac-pkgs/issues/66)
- [omacom/linux-aurora#432](https://github.com/omacom/linux-aurora/issues/432) (issue): J604 test results
- Closed without merging: [omacom/omarchy#18177](https://github.com/omacom/omarchy/pull/18177) (keybindings menu leaves out absent keys)

The Aurora kernel has merged DjDeveloperr's J604 / M4 device trees (aurora-silicon/linux #1142 and follow-ups). We rebase onto them and send only what goes beyond them.

## Credits

This builds on the work of [Aurora Silicon](https://github.com/aurora-silicon) (the M4 kernel, including DjDeveloperr's M4 device trees and drivers), the [omacom](https://github.com/omacom) Omarchy for Apple Silicon team, and the [Asahi Linux](https://asahilinux.org) project, whose kernel, m1n1, Mesa, audio and installer work everything here stands on. `CREDITS.md` counts each author's patches.

## License

Each part keeps the license of the project it changes; see `LICENSE.md`.
