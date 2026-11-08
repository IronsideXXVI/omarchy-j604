# Omarchy on the MacBook Pro 14-inch (M4, 2024): J604 bring-up

This repository is the working set for running [Omarchy](https://github.com/omacom/omarchy) on the Apple MacBook Pro 14-inch with the base M4 chip (Mac16,1, board J604, SoC T8132, the same SoC as the M4 MacBook Air J713/J715). It's part of the Omarchy for Apple Silicon effort, and the J604 is a daily machine that's available for testing.

It uses the official chain only: the [linux-aurora](https://github.com/omacom/linux-aurora) kernel with a J604 patch series, omacom [m1n1](https://github.com/omacom/m1n1) and U-Boot, the official installer, Omarchy 4.5, and normal `omarchy update`. Each change is a patch on top of an official recipe, built with a local `pkgrel`.

Changes go to the official repositories as reviewed pull requests, one topic at a time, in whatever shape the team prefers. Until each one lands, this repository holds the full set so anyone can read and rebuild it. Most of the kernel series is other people's work, carried with its authorship (see Credits and `CREDITS.md`).

## Status

Only what's measured on the machine is listed as working.

| Area | Status |
| --- | --- |
| Device trees, storage, keyboard and trackpad, USB, battery | Working |
| Internal display | 3024x1964 at 120 Hz, 8-bit today. 10-bit, managed P3, EDR and VRR are in the next build |
| GPU | Accelerated (OpenGL 4.6 / Vulkan 1.4), Glassy blur at 119 fps. GPU idle power in progress |
| Wi-Fi and Bluetooth | Working |
| Audio | Speakers and headphone jack working; mics not validated yet |
| Camera and video decode | H.264 hardware decode (4K at about 120 fps); camera in progress |
| Power | 10 cores at full clocks, suspend on lid close. Deep sleep in bring-up (the m1n1 warm-boot entry is verified; the T8132 sleep sequence is next) |
| Thunderbolt and external displays | In progress |
| Touch ID | Working for sudo, polkit and the lock screen |
| Also | Auto-brightness from the light sensor, keyboard backlight |

The `linux-aurora` series here is the build being installed now, so it's ahead of the table: it already carries the next-build display work (Display P3, EDR/HDR and adaptive sync on the J604 panel, patches 0383-0393, 0555-0560, 0613-0614) and the kernel side of the T8132 sleep sequence (0471-0487, 0590-0591). The table changes only once those are measured on the machine. The m1n1 warm-boot entry for deep sleep isn't in this repository yet.

## Install

1. Install Omarchy with the official Omarchy for Apple Silicon installer, as on any supported Mac.
2. The J604 builds of the packages below replace the official ones through their local `pkgrel`. Any official update with a higher version replaces them again.

There are no prebuilt packages in this repository. To rebuild a component, apply its series to the commit in its `BASE` file, or build its `recipe/` with `makepkg`. Until these changes land in the official packages, these builds target the J604 only.

## Versions

| Package | Official base | Local pkgrel here |
| --- | --- | --- |
| `linux-aurora` | 7.2.11.aurora15, omacom/linux-aurora `integration/aurora` | `1.612` (the build being installed now; the status above was measured on the installed builds) |
| `m1n1-aurora` | 1.6.1.aurora22, omacom/m1n1 | `1.3` (installed) |
| `omarchy` | 4.5.0, omacom/omarchy | `3.610` (installed) |
| `omarchy-settings` | 4.5.0, omacom/omarchy | `3.610` (installed) |
| `omarchy-mac` | 0.2.3, omacom/omarchy-mac-pkgs | `2.7` (installed) |

## Layout

| Directory | What | Upstream it applies to | License |
| --- | --- | --- | --- |
| `kernel/` | `git format-patch` series (apply in `series` order); `BASE` names the commit it applies to | omacom/linux-aurora | GPL-2.0 (see `kernel/COPYING`) |
| `m1n1/` | patches and the `m1n1-aurora` recipe | omacom/m1n1 | MIT (see `m1n1/LICENSE`) |
| `omarchy/` | patches and the `omarchy` recipe | omacom/omarchy | MIT (see `omarchy/LICENSE`) |
| `omarchy-settings/` | patches and the `omarchy-settings` recipe | omacom/omarchy | MIT (see `omarchy-settings/LICENSE`) |
| `omarchy-mac/` | patches and the `omarchy-mac` recipe | omacom/omarchy-mac-pkgs | MIT (see `omarchy-mac/LICENSE`) |

Each patch keeps its original author. Patches whose subject starts with "J604 local build" are carried only here so far. Patches made with an AI coding assistant say so in an `Assisted-by:` trailer, and pull requests disclose it per each repository's policy.

No Apple firmware, binaries or device dumps are in this repository. The firmware the machine needs is extracted on the machine itself at install time, as on every Omarchy Mac.

## Upstreaming

Opened so far:
- [omacom/omarchy#18176](https://github.com/omacom/omarchy/pull/18176): blur the Glassy bar from the cached wallpaper blur (59 to 119 fps)
- [omacom/omarchy#18177](https://github.com/omacom/omarchy/pull/18177): the keybindings menu skips keys a keyboard lacks

Ready to send next:
- the J604 ambient light sensor (binding + DT)
- a KUnit fix for the AV lifetime tests
- J604 DT pieces for storage, sensors, motion/lid, PMU, trackpad and video decode
- then display, USB4/Thunderbolt, GPU power, Neural Engine, sleep and Touch ID
- J604 kboot defaults for m1n1

Where the kernel work lands, and in what PR shape, follows the team's answer. Where someone already has an open pull request for the same thing, we test theirs instead of sending a second one. Our build carries everything until it merges, so nobody waits on us.

## Credits

This builds on the work of [Aurora Silicon](https://github.com/aurora-silicon) (the M4 kernel, including DjDeveloperr's M4 device trees and drivers), the [omacom](https://github.com/omacom) Omarchy for Apple Silicon team, and the [Asahi Linux](https://asahilinux.org) project, whose kernel, m1n1 and Mesa work everything here stands on. `CREDITS.md` counts each author's patches.

## License

Each part keeps the license of the project it changes; see `LICENSE.md`.
