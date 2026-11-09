# Upstream plan for the J604 work

Last updated 2026-11-09. This page lists what we plan to send to the official projects, in what order, how ready each piece is, and what is already open. **Nothing has merged yet.**

Labels:
- **MEASURED**: run on the J604, or measured in our build and test runs.
- **SOURCE**: read in upstream code, docs, issues or pull requests.
- **CODE**: read from our patches.
- **INFERENCE**: our reasoning.
- **TO-VERIFY**: open question.

## Open upstream now (SOURCE, checked 2026-11-09)

| Link | What | State |
| --- | --- | --- |
| [omacom/linux-aurora#432](https://github.com/omacom/linux-aurora/issues/432) | J604 test results (issue); points here for the status (`STATUS.md`) and this plan | open |
| [omacom/omarchy#18176](https://github.com/omacom/omarchy/pull/18176) | Blur the Glassy bar from the cached wallpaper blur (59 to 119 fps on this 120 Hz panel) | open, reopened. The same change as [#18191](https://github.com/omacom/omarchy/pull/18191) by raffsmagic; whichever the maintainers prefer, we test it |
| [omacom/omarchy#18228](https://github.com/omacom/omarchy/pull/18228) | Retry the kyber scheduler write while a sibling NVMe namespace switches (Linux 7.2 returns EBUSY) | open |
| [omacom/omarchy#18229](https://github.com/omacom/omarchy/issues/18229) | Issue: `omarchy-mac`'s mpv default never installs because `omarchy-settings` owns `/etc/mpv/mpv.conf`. Linked from the related [omacom/omarchy-mac-pkgs#66](https://github.com/omacom/omarchy-mac-pkgs/issues/66) | open |
| [omacom/omarchy#18177](https://github.com/omacom/omarchy/pull/18177) | Keybindings menu leaves out keys a keyboard lacks | closed, not merged. The same change is now kbd-0104 in our edge build (see below) |

## Direction we follow (SOURCE: Omarchy M team decisions and chat, Sep–Nov 2026)

- Integrate into the main omacom repositories wherever practical: omacom/omarchy, omacom/omarchy-pkgs, omacom/omarchy-mac-pkgs, omacom/linux-aurora, omacom/m1n1.
- **The kernel work for M4, M5 and M6 is merged together with Linux 7.3.** Our kernel series is being rebased onto aurora-silicon/linux `integration/aurora` 7.3.0 for that merge, rather than sent as a separate J604 tree.
- Results are reported through [omarchy-m-testing.org](https://omarchy-m-testing.org), one result per check. "Not tested" is the default; an implementation alone is not a pass.
- Agent-assisted work is disclosed the way each repository asks: the Linux kernel trees use an `Assisted-by:` trailer and a human `Signed-off-by:`; repositories without a policy get a plain note in the pull request description.

## How we work

- **Stock userland, edge channel.** The J604 runs stock Omarchy edge. The only userland change we carry is the keyboard-backlight series; everything else we carry is hardware enablement (see `STATUS.md`).
- **One topic per pull request**, small and reviewable, against each repository's default branch and rules.
- **Dylan reviews and signs off every pull request and issue** before it is opened, under his own name and sign-off.
- **Credit.** Work that is someone else's (much of the kernel series we carry) is never sent as ours. Where we build on it, we credit it and coordinate with its author. Where someone already has an open pull request for the same thing, we test theirs instead of sending a second one.

## Overlap with work already upstream (SOURCE, checked 2026-11-08)

The Aurora kernel (aurora-silicon/linux) has merged DjDeveloperr's J604 / M4 device trees: #1142 (J604 and M4 trees) and follow-ups (#1166 new M4 file layout, #1170/#1171 M4 GPUs on by default, #1178/#1179/#1207 J604 display enable and panel). Also related: Ryan's open #1035 (M4 CPU DVFS), aeries0x's open #1186 (J604 power-manager policy and boot framebuffer), and Eryk's J713 work (camera, USB4 routers, external display, audio with Erik).

What that means for us:
- Our base J604 enablement (SMC, storage, keyboard and trackpad, Wi-Fi/BT, USB2, GPU and display resources) largely duplicates #1142, so **we do not send it**; we rebase onto Aurora's tree and send only what goes beyond it.
- What is only in ours (MEASURED offline 2026-11-08, our J604 device tree against Aurora's): speakers, ANE and AVD nodes, AOP sensors (ambient light, lid angle, accelerometer, gyroscope), CPU performance controllers and PMU, the display crossbar, sleep power-manager nodes, the USB4 routers and the SD reader. Aurora already has the drivers and bindings for most of that, so most of it is device-tree work on Aurora's layout.
- For cpufreq/cpuidle we test and review #1035; for the power-manager policy and boot framebuffer, #1186.
- Our display work must be compared with #1178/#1179/#1207 first; only the remaining delta goes out.

**Where kernel changes go:** SoC- and board-level changes go to aurora-silicon/linux first, coordinated with DjDeveloperr and Eryk; omacom/linux-aurora receives them at its 7.3 merge. A change that only matters to Omarchy's packaging of the kernel goes to omacom/linux-aurora directly. We confirm this split with the kernel maintainers before the first kernel pull request (TO-VERIFY).

## Wave 1: small userland and packaging changes

Each needs Dylan's sign-off.

| # | Change | Target | Readiness | Evidence |
| --- | --- | --- | --- | --- |
| 1 | kyber retry | omacom/omarchy | **Open: #18228** | MEASURED 2026-11-09: two namespaces stay on `mq-deadline` with stock |
| 2 | mpv.conf ownership | omacom/omarchy issue, cc omarchy-mac-pkgs | **Open: issue #18229**, linked from omarchy-mac-pkgs#66; the fix follows the maintainers' choice | MEASURED 2026-11-09 |
| 3 | Glassy bar xray (59 to 119 fps) | omacom/omarchy | **Open: #18176** (reopened), same change as #18191 | MEASURED 2026-11-09: stock 59 fps on this panel |
| 4 | Keyboard backlight: idle timeout (#12558 by Daniel Fernau, existing PR), a platform default for the timeout (`idle.json`), the restore level kept across a reboot, and the menu leaving out absent keys (the #18177 change) | omacom/omarchy (`quattro`); the platform default data in omacom/omarchy-mac-pkgs | Drafts ready; #12558 is someone else's open PR, which we test and support rather than resend | MEASURED 2026-11-09 on the J604 with the 1.3 builds |
| 5 | Touch ID eligibility for the J604 (`apple,t8132` + `apple,j604`) + test | omacom/omarchy-mac-pkgs | Waits for the kernel side (#20) | MEASURED 2026-11-09: sudo, polkit, lock screen |
| 6 | Plymouth: recognise SimpleDRM by driver name | freedesktop plymouth (GitLab merge request) | Ready (check for a duplicate first) | CODE + observed on the J604 before the fix |
| 7 | asahi-scripts: add `apple_nvmem_spmi` to the initramfs hook | asahi-alarm/asahi-scripts | Needs measurement | CODE |
| 8 | asahi-fwextract: extra camera sensor tuning files | asahi-alarm / AsahiLinux | Needs a camera capture first | CODE |
| 9 | omarchy-mac-boot generic fixes (`dtb-overlays` under `pipefail`, vendor-firmware hook `timeout`) | omacom/omarchy-mac-pkgs | Ready (boot-critical: attach cold-boot evidence) | CODE; in daily use on the J604 |
| 10 | Panel defaults for verified Apple panels (P3, VRR, 10-bit) as per-panel package data | omacom/omarchy-mac-pkgs (data) + omacom/omarchy (default rule) | Built, not installed; needs measurement on the J604 and other Macs. If the panel link is dithered, the claim is "10-bit compositing" | INFERENCE 2026-11-09: link most likely 8 bpc with dithering |

## Wave 2: the hardware series

Ordered by dependency. Kernel items assume Aurora's J604 tree and the 7.3 rebase as the base.

| # | Change | Target | Readiness |
| --- | --- | --- | --- |
| 11 | Ambient light sensor: T8132 AOP ALS node + J604 enable | aurora-silicon/linux | Needs the 7.3 rebase; MEASURED working 2026-11-08 |
| 12 | J604 device-tree delta on Aurora's tree: speakers, AOP sensors, ANE/AVD nodes, display crossbar, SD reader, USB4 routers, sleep power-manager nodes | aurora-silicon/linux | Needs the 7.3 rebase; each block measured before it is sent (speakers after #19) |
| 13 | m1n1: J604 admission on the M4 hand-off paths, as a per-board table | omacom/m1n1; Aurora's boot loader for SoC-level parts | Needs rebase and coordination; whether Aurora's boot loader already admits the J604 another way is TO-VERIFY |
| 14 | U-Boot: J604 device-tree import | aurora-silicon/u-boot (mailing list only) | Needs Dylan's sign-off and a mail setup |
| 15 | Display delta (P3, VRR, EDR/HDR, brightness fade, power-down while blanked, panel data parsing) | aurora-silicon/linux | Compare with #1178/#1179/#1207 first; needs measurement |
| 16 | GPU idle power-off for G16 | aurora-silicon/linux | Needs idle power measured across boots |
| 17 | Mesa: admit the T8132 GPU | omacom/mesa: answer omacom/mesa#25 with conformance results | Needs a Vulkan/GL conformance run on the J604 |
| 18 | m1n1 hand-offs used by later blocks (speaker amp config, camera, ANE and encoder firmware, external display, PCIe port power, sleep warm-boot) | omacom/m1n1 | One PR per hand-off, as each block reaches Working |
| 19b | BCM4388 wideband speech flag (in our 1.617 as patch 0743) | aurora-silicon/linux | Needs the Bluetooth headset microphone check |
| 19 | Audio: J604 UCM, speaker DSP graph, microphone config, `speakersafetyd` safety profile | AsahiLinux asahi-audio / alsa-ucm-conf-asahi / speakersafetyd, then the omarchy-pkgs recipes | Speaker safety review with the audio maintainers first |
| 20 | Touch ID kernel side (Secure Enclave sensor node for T8132) | aurora-silicon/linux, with the Touch ID owners | Coordinate, don't duplicate; unblocks #5 |
| 21 | CPU PMU node for the J604 (kernel patch 0616 is test-only today) | aurora-silicon/linux | Needs the counter measurement first |

Not ready to propose (still being brought up, see `STATUS.md`): camera, microphones, ANE power-up, video encoder, external displays and HDMI, Thunderbolt/USB4, deep sleep.

## Kernel: what's next

- Installed: `linux-aurora` 7.2.11.aurora15, local `1.617` (the series in `kernel/`, 766 patches).
- Planned: local `1.618`, with the fix for the camera probe (one driver owns the ISP DART's `dapf` window; see `STATUS.md`).
- In progress: the rebase of the J604 series onto aurora-silicon/linux `integration/aurora` 7.3.0, keeping only what goes beyond Aurora's own J604 / M4 work.
