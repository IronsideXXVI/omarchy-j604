# Licensing

This repository collects patches and package recipes for several projects. Each part keeps the license of the project it changes:

| Path | License | Upstream license file |
| --- | --- | --- |
| `kernel/` | GPL-2.0 WITH Linux-syscall-note; device tree sources GPL-2.0+ OR MIT, as marked in each file | `kernel/COPYING`, `kernel/LICENSES/` (copied from the kernel tree) |
| `aura/` | MIT for the m1n1 code it is built from (Copyright The Asahi Linux Contributors); GPL-2.0-only for Aurora Silicon's contributions; third-party parts as listed upstream | `aura/LICENSE` (copied from aurora-silicon/aura 54678354) |
| `avd-fw/` | MIT | `avd-fw/LICENSE` (copied from AsahiLinux/avd-fw v0.1) |
| `uboot-aurora/` | GPL-2.0+ (U-Boot's default; individual files carry their own SPDX identifiers) | `uboot-aurora/LICENSE` and `uboot-aurora/LICENSE.README` (copied from aurora-silicon/u-boot `Licenses/gpl-2.0.txt` and `Licenses/README`) |
| `mesa/` | MIT for the main Mesa code; other components as listed in Mesa's license page | `mesa/LICENSE` (copied from omacom/mesa `docs/license.rst`) |
| `alsa-ucm-conf-aurora/` | BSD-3-Clause, Copyright (c) 2022, Asahi Linux Contributors | `alsa-ucm-conf-aurora/LICENSE` (copied from aurora-silicon/alsa-ucm-conf-asahi `LICENSE.asahi`) |
| `aurora-audio/` | MIT, Copyright (c) 2022 The Asahi Linux Contributors | `aurora-audio/LICENSE` (copied from aurora-silicon/asahi-audio) |
| `speakersafetyd/` | MIT, Copyright The Asahi Linux Contributors | `speakersafetyd/LICENSE` (copied from AsahiLinux/speakersafetyd 2.0.1) |
| `asahi-fwextract/` | MIT, Copyright (c) 2021 The Asahi Linux contributors | `asahi-fwextract/LICENSE` (copied from asahi-alarm/asahi-alarm-installer v0.9.2) |
| `asahi-scripts/` | MIT, Copyright The Asahi Linux Contributors | `asahi-scripts/LICENSE` (copied from asahi-alarm/asahi-scripts 20261023) |
| `libfprint-git/` | LGPL-2.1-or-later | `libfprint-git/LICENSE` (copied from libfprint 6f9479c3d5 `COPYING`) |
| `libva-v4l2_request-avd/` | GPL-3.0-or-later (the driver, as its recipe states); the recipe diff in `patches/` follows the recipe's MIT license | `libva-v4l2_request-avd/LICENSE` (copied from omacom/libva-v4l2_request c653eaa623 `COPYING`) |
| `plymouth/` | GPL-2.0 | `plymouth/LICENSE` (copied from Plymouth 26.134.222 `COPYING`) |
| `omarchy-mac-boot/` | MIT, Copyright (c) David Heinemeier Hansson | `omarchy-mac-boot/LICENSE` (copied from omacom/omarchy-mac-pkgs) |
| `omarchy-mac/` | MIT, Copyright (c) David Heinemeier Hansson | `omarchy-mac/LICENSE` (copied from omacom/omarchy-mac-pkgs) |
| `omarchy-dev/`, `omarchy-settings-dev/` | MIT, Copyright (c) David Heinemeier Hansson | `omarchy-dev/LICENSE`, `omarchy-settings-dev/LICENSE` (copied from omacom/omarchy) |
| `*/recipe/PKGBUILD` | The license of the official recipe each one changes: MIT for the omacom/omarchy-pkgs recipes (`uboot-aurora`, `avd-fw`, `mesa`, `alsa-ucm-conf-aurora`, `aurora-audio`, `omarchy-mac-boot`, `omarchy-mac`, `omarchy-dev`, `omarchy-settings-dev`, `libfprint-git`, `libva-v4l2_request-avd`); 0BSD for the Arch Linux `plymouth` recipe; the asahi-alarm/PKGBUILDs recipes (`speakersafetyd`, `asahi-fwextract`) and the aurora-silicon/alarm-aurora recipes (`aura`, `asahi-scripts`) as published there | the official recipe named on line 3 of each `BASE` |
| `README.md`, `LICENSE.md`, `CREDITS.md`, `STATUS.md`, `UPSTREAM.md`, `not-installed/README.md` | MIT | `omarchy-mac/LICENSE` terms |

Every patch keeps its original author. Contributions from other people are redistributed under their projects' licenses.
