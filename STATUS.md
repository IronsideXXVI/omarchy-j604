# J604 bring-up status (MacBook Pro 14-inch, M4, 2024)

Board `apple,j604`, SoC `apple,t8132` (M4, 4 performance + 6 efficiency cores, 10-core GPU, 16 GB). Last updated 2026-11-09.

## How to read this page

- **Working**: measured on this Mac, with the date of the measurement.
- **In progress**: code exists (in our kernel, boot loader or packages) or part of it runs, but the block is not yet measured working end to end.
- **Not started**: no code yet.
- Every row carries an evidence label and a date:
  - **MEASURED** (date): run on this Mac.
  - **INFERENCE** (date): our reasoning, not a measurement.
  - **TO-VERIFY** (date): open question, or a claim not yet checked on the current build.
- A driver that loads, or a device that appears, is not a pass on its own. Where that is all we know, the row says so.
- "Current build" means the packages listed below, installed on 2026-11-09.

## What the machine runs today

MEASURED 2026-11-09 (`pacman -Q` and `uname -r` on the machine):

| Layer | What is installed | Source |
| --- | --- | --- |
| Channel | Omarchy **edge** since 2026-11-09 (official `omarchy-channel-set edge`) | official |
| Desktop | `omarchy-dev` 4.0.0.r7634.gcbd0029, local pkgrel `1.3` (stock edge + the keyboard-backlight series, 4 patches); `omarchy-settings-dev` same version, local `1.4` (the same 4 patches + the built-in panel defaults, 1 patch); Hyprland 0.57.1, stock | official recipes + 4 / 5 patches |
| Mac add-on | `omarchy-mac` 0.2.3, local `2.12`: keyboard-backlight idle default, Apple F-row key names, Touch ID eligibility, the 14-inch MacBook Pro panel line (10-bit, Display P3) | official recipe + 4 patches |
| Kernel | `linux-aurora` 7.2.11.aurora15, local `1.617` (766 patches), running `7.2.11-15-1.617-ARCH` since 2026-11-09 | official recipe + J604 series |
| Boot chain | `m1n1-aurora` 1.6.1.aurora22 (local `1.608`), `uboot-aurora` 2026.07.aurora6 (local `1.604`), `omarchy-mac-boot` 20261104 (local `1.614`), `plymouth` 26.134.222 (local `3.604`), `asahi-scripts` 20261023 (local `1.1`) | official recipes + J604 patches |
| Graphics | `mesa-aurora` / `vulkan-aurora` with the T8132 GPU admitted (local `1.8132`) | official recipe + 1 patch |
| Audio | `alsa-ucm-conf-aurora` (local `1.605`), `aurora-audio` (local `3.607`), `speakersafetyd` (local `1.605`) with J604 profiles | official recipes + J604 files |
| Firmware | `asahi-fwextract` 0.9.2 (local `1.608`): extra camera tuning files | official recipe + 1 patch |
| Neural Engine userland | `omarchy-ane`, `omarchy-ane-dkms`, `mil-hwx-compiler`, `omarchy-mac-ml`: official edge builds, not carried by us | official |

Next kernel (not installed): the J604 series is being rebased onto aurora-silicon/linux `integration/aurora` 7.3.0. The camera fix below goes into that kernel.

## What changed in kernel 1.617 (over 1.614)

CODE (the 30 patches 0737-0766 in `kernel/`). Each item is code in the installed kernel; none of it is a measured result unless a row below says so.

- **External displays (USB-C / Thunderbolt):** 10 bits per component scanout, HDR10 colour and static metadata, the sink's EDID and SDR primaries, and DisplayPort audio for a T8132 external display controller bound on its own (patches 0739-0748, 0754). No external monitor has been tested yet.
- **Idle external display controllers power down** when nothing is attached, and power up on request (0755-0757).
- **Internal panel:** recovery when the panel refuses a brightness-range probe, instead of losing the session (0737, 0738); EDR headroom groundwork that takes the headroom from the content (0764, 0765). No EDR claim.
- **ProRes encoder driver** imported from Aurora, probing only on the T8132 (0759-0763). Its device-tree nodes are disabled, so the driver does not load.
- **Neural Engine:** the ANE's DARTs may suspend once the device is bound (0752, 0753).
- **ISP DART filter ranges** described in the device tree, and the DART probes without them too (0749-0751). See the camera row: this is what now collides with the ISP driver.
- **Bluetooth:** the BCM4388 advertises wideband speech (0743), for headset microphones.
- **SIO** (the DisplayPort audio coprocessor) held off until its power domain is qualified (0758). On 1.617 its earlier deferred probe is gone (MEASURED 2026-11-09).
- 0766 only changes a log message about the external display controller's Type-C ports.

The stock official packages cannot boot this Mac yet (MEASURED 2026-11-09 on stable aurora14 and edge aurora15). The stock `linux-aurora` ships a J604 device tree that describes only the CPUs, interrupt controller, power states, pin control and serial port: no storage controller, display, keyboard or trackpad, USB, Wi-Fi or GPU. Stock m1n1 and U-Boot accept only the J713 (MacBook Air M4) on their M4 hand-off paths (INFERENCE 2026-11-09, from reading their source).

## At a glance

| Block | State | Evidence |
| --- | --- | --- |
| CPU frequency scaling and idle (10 cores, full clocks) | Working | MEASURED 2026-11-09 |
| CPU performance counters | Counter device present (test-only DT node, kernel patch 0616); counter accuracy not yet checked | MEASURED 2026-11-09 (device); TO-VERIFY 2026-11-09 (accuracy) |
| GPU (OpenGL 4.6 / Vulkan 1.4) | Working | MEASURED 2026-11-09 |
| GPU idle power | In progress | TO-VERIFY 2026-11-09 |
| RAM (16 GB) | Working | MEASURED 2026-11-09 |
| Internal display (3024x1964 at 120 Hz, scale 2) | Working | MEASURED 2026-11-09 |
| 10-bit | Package default: compositor scans out XRGB2101010 with no user setting; panel link most likely 8 bpc with dithering | MEASURED 2026-11-09 (scanout); INFERENCE 2026-11-09 (link depth) |
| Display P3 | Package default (`cm = dp3`, kernel colour target Display P3); passed the visual check | MEASURED 2026-11-09 (setting, and the owner's visual check) |
| VRR | Off, fixed 120 Hz: the upstream Omarchy default (since 58443c82c), kept by the owner's decision | MEASURED 2026-11-09 |
| EDR / HDR | No claim on the current build | TO-VERIFY 2026-11-09 |
| Glassy bar frame rate | Stock: about 59 fps at 120 Hz; 119 fps only with omacom/omarchy#18176 | MEASURED 2026-11-09 |
| Brightness | Working (panel backlight) | MEASURED 2026-11-09 |
| Automatic brightness | Not shipped | MEASURED 2026-11-09 (not in the installed packages) |
| External display (USB-C / Thunderbolt), HDMI | In progress | TO-VERIFY 2026-11-09 |
| USB-C data | Working (USB devices) | MEASURED 2026-11-09 |
| Thunderbolt / USB4 | In progress (no USB4 domains found) | MEASURED 2026-11-09 |
| External display 10-bit / HDR10 / EDID / DP audio, idle power-down | In the kernel since 1.617, not tested | TO-VERIFY 2026-11-09 |
| SD card reader | In progress (controller up, no card tested) | TO-VERIFY 2026-11-09 |
| Storage (NVMe) | Working | MEASURED 2026-11-09 |
| Keyboard, trackpad | Working | MEASURED 2026-11-09 |
| Keyboard backlight | Working (on with activity, off after 10 s idle, Ctrl+F1/F2) | MEASURED 2026-11-09 |
| Wi-Fi | Working | MEASURED 2026-11-09 |
| Bluetooth | Working | MEASURED 2026-11-09 |
| Speakers (with speaker protection) | Working | MEASURED 2026-11-06 (playback); 2026-11-09 (sink and `speakersafetyd` active) |
| Headphone jack | Working | MEASURED 2026-11-06 |
| Microphones | In progress | TO-VERIFY 2026-11-09 |
| Camera | In progress: `apple-isp` unbound (probe fails with -EBUSY on 1.617); OPEN, fix goes into the 7.3 kernel | MEASURED 2026-11-09 |
| Hardware video decode (H.264) | Working | MEASURED 2026-11-08 |
| Video encode (AVE), JPEG, scaler, ProRes | In progress (ProRes driver in 1.617, nodes disabled, not loaded) | TO-VERIFY 2026-11-09 |
| Neural Engine (ANE) | Driver bound, not powered up; no `/dev/accel` | MEASURED 2026-11-09 |
| Ambient light sensor | Sensor reads; nothing uses it by default | MEASURED 2026-11-08 |
| Accelerometer, gyroscope, lid angle | In progress (devices present, readings unchecked) | TO-VERIFY 2026-11-09 |
| Lid switch, suspend on lid close | Working | MEASURED 2026-11-09 |
| SMC sensors, fan, thermal readouts | Working (readouts) | MEASURED 2026-11-09 |
| Battery and charger | Working | MEASURED 2026-11-09 |
| Suspend (s2idle) | Working | MEASURED 2026-11-09 |
| Idle power on AC | About 10.5 W with the stock edge `performance` power profile (7.5-7.7 W before 2026-11-09 04:56Z) | MEASURED 2026-11-09 |
| Deep sleep (suspend to RAM) | In progress (only s2idle offered) | MEASURED 2026-11-09 |
| Battery life vs macOS | Not measured yet | TO-VERIFY 2026-11-09 |
| Touch ID (sudo, polkit, lock screen) | Working | MEASURED 2026-11-09 |
| Boot chain (m1n1, U-Boot, Limine, disk unlock) | Working, no failed units on 1.617 | MEASURED 2026-11-09 |

## Per block

### CPU
- **Working.** MEASURED 2026-11-09: 10 cores online; `apple-cpufreq` maximums 4464 MHz (performance) and 2892 MHz (efficiency); `apple_idle` cpuidle.
- On AC with the stock edge power profile (`performance`), both CPU policies run the `performance` governor and idle at their top clock (MEASURED 2026-11-09). See Power.
- The PMU counter device (`apple_t8132_pmu`, 8 counters) is present (MEASURED 2026-11-09 on 1.614). It comes from kernel patch 0616, which is marked test-only: the node is enabled for good only after a counter measurement passes (TO-VERIFY 2026-11-09).
- Carried by: kernel series (CPU nodes, cluster frequency control, cpuidle, energy model). Aurora has an open pull request for M4 DVFS (#1035), which we test instead of sending our own.

### GPU
- **Working.** MEASURED 2026-11-09: the `asahi` driver drives the 10-core G16 GPU; OpenGL 4.6 and Vulkan 1.4.
- GPU idle power: in progress (TO-VERIFY 2026-11-09). The idle power-off is in the kernel; its power across boots is still being measured.
- Carried by: kernel series and `mesa-aurora`/`vulkan-aurora` with one patch that admits the T8132 GPU. Stock Mesa refuses this GPU on purpose (omacom/mesa#25).

### Internal display
- **Working.** MEASURED 2026-11-09: 3024x1964 at 120 Hz, scale 2.
- Package defaults since 2026-11-09 (MEASURED 2026-11-09 after a reboot, with no display line in the owner's `monitors.lua`): 3024x1964 at 120 Hz (the panel's only mode), scale 2, 10-bit (the compositor scans out XRGB2101010) and Display P3 (Hyprland `cm = dp3`; the kernel logs "panel SDR colour: Display P3"; the connector stays on its default colorspace).
  - Carried by: `omarchy-mac` 2.12, whose `displays.conf` names the panel (`panel eDP 3024x1964 bitdepth=10 cm=dp3`), and `omarchy-settings-dev` 1.4, whose `panel-defaults.lua` applies that line to the built-in panel's monitor rules. A `bitdepth` or `cm` set in `monitors.lua` still wins. Mode and scale need no package setting: Hyprland picks 3024x1964 at 120 Hz and scale 2 on its own (CODE).
  - The panel link itself is most likely 8 bits per channel with dithering (INFERENCE 2026-11-09). The P3 picture passed the owner's visual check (MEASURED 2026-11-09: "Looks great").
- VRR: off (MEASURED 2026-11-09). That is the upstream Omarchy default: Omarchy turned VRR on and took it out again in 58443c82c because it adds a small lag (SOURCE). The panel supports 24-120 Hz. It stays off: a fixed 120 Hz, by the owner's decision (2026-11-09).
- EDR / HDR: no claim on the current build (TO-VERIFY 2026-11-09). 1.617 adds groundwork that takes the EDR headroom from the content (CODE); it needs an HDR client to be measured.
- Glassy bar: stock holds about 59 fps on this 120 Hz panel; 119 fps needs the xray change in omacom/omarchy#18176 (MEASURED 2026-11-09). That change is not in our packages; the owner's own user toggle restores it locally.
- Brightness: the panel backlight works (MEASURED 2026-11-09). There is no automatic brightness in the installed packages.

### External displays, HDMI
- **In progress.** The kernel has the external display controllers, crossbar, DP PHY and HDMI bridge. 1.617 adds 10-bit scanout, HDR10, the sink's EDID and primaries, DP audio for an external controller, and power-down of an idle external controller (CODE). No external monitor has been tested on the current build (TO-VERIFY 2026-11-09).

### USB-C, Thunderbolt / USB4, SD card
- USB-C data: **Working** for USB devices (MEASURED 2026-11-09).
- Thunderbolt / USB4: **In progress.** No USB4 domains are found on the current build (MEASURED 2026-11-09).
- SD reader: controller up, no card tested (TO-VERIFY 2026-11-09).

### Storage
- **Working.** MEASURED 2026-11-09: Apple NVMe with three namespaces; root on LUKS2 + btrfs.
- Stock udev rule: two of three namespaces stay on `mq-deadline` instead of `kyber` on Linux 7.2 (MEASURED 2026-11-09). Fix proposed in omacom/omarchy#18228. Harmless for function (INFERENCE 2026-11-09).

### Keyboard, trackpad, keyboard backlight
- **Working.** MEASURED 2026-11-09: keyboard and multi-touch trackpad, also at the disk-unlock prompt.
- Keyboard backlight: **Working** with the local `omarchy-dev` 1.3 / `omarchy-settings-dev` 1.4 and `omarchy-mac` 2.12 builds (MEASURED 2026-11-09): on with activity, off after 10 s idle, Ctrl+F1/F2 change the level, and Super+K lists no keys the MacBook lacks.

### Wi-Fi and Bluetooth
- **Working.** MEASURED 2026-11-09: BCM4388 Wi-Fi connected; Bluetooth powered. A2DP playback MEASURED 2026-11-06.
- Bluetooth headset microphone (wideband): 1.617 makes the BCM4388 advertise wideband speech (CODE); headset check TO-VERIFY 2026-11-09.

### Audio
- Speakers: **Working**, with `speakersafetyd` protecting them (MEASURED 2026-11-06 playback; 2026-11-09 sink and daemon active).
- Headphone jack: **Working** (MEASURED 2026-11-06).
- Microphones: **In progress** (TO-VERIFY 2026-11-09).
- Carried by: J604 files in `alsa-ucm-conf-aurora`, `aurora-audio` and `speakersafetyd`, and the speaker amp configuration m1n1 passes on. Without `speakersafetyd` the kernel keeps the speakers muted.

### Camera
- **In progress.** MEASURED 2026-11-09: the `apple-isp` driver is not bound, so there is no camera device.
  - On 1.617 its probe fails with -EBUSY (error -16): the ISP's DART (`apple-dart`) maps its address-filter (`dapf`) register window, and the ISP driver then asks for the same window exclusively.
  - On 1.614 the probe also failed, earlier, with -19: the ISP's DART had no filter ranges and did not probe (MEASURED 2026-11-09 on 1.614).
  - The camera power domain stays on while the ISP is unbound (INFERENCE 2026-11-09).
  - OPEN. The fix (one driver owns the `dapf` window) goes into the 7.3 kernel. The same -EBUSY lines are in the log of the 2026-11-09 17:11Z boot.
- Carried by: kernel ISP series, m1n1 camera firmware hand-off, `asahi-fwextract` (extra sensor tuning files).

### Media engines
- H.264 hardware decode: **Working** (MEASURED 2026-11-08, 4K through VA-API). With stock settings, mpv's system default does not use it (omacom/omarchy#18229).
- Video encode (AVE), JPEG, scaler, ProRes: **In progress** (TO-VERIFY 2026-11-09). 1.617 carries the ProRes encoder driver from Aurora; its device-tree nodes are disabled and the module does not load (MEASURED 2026-11-09).

### Neural Engine (ANE)
- **In progress.** MEASURED 2026-11-09: the ANE is detected and the `ane_t6021` driver is bound; on the T8132 the driver stops before power-up, by design for now. There is no `/dev/accel`.
- m1n1 1.608 hands the preloaded ANE firmware over (m1n1 patch 0028). 1.617 lets the ANE's DARTs suspend once the device is bound (CODE). The ANE userland (`omarchy-ane`, `omarchy-ane-dkms`, `mil-hwx-compiler`, `omarchy-mac-ml`) is the official edge build; our earlier rebuilds are withdrawn.

### Sensors
- Ambient light: the sensor reads (MEASURED 2026-11-08). Nothing uses it by default; automatic brightness is not shipped.
- Accelerometer, gyroscope, lid angle: devices present, readings unchecked (TO-VERIFY 2026-11-09).
- Lid switch: **Working**, suspend on lid close (MEASURED 2026-11-09).
- SMC temperatures, fan, power rails: **Working** readouts (MEASURED 2026-11-09). Fan control parity with macOS: not started.
- Battery and charger: **Working** (MEASURED 2026-11-09).

### Power
- Suspend (s2idle): **Working** (MEASURED 2026-11-09; the kernel offers only `s2idle`).
- Deep sleep (suspend to RAM): **In progress**; kernel and m1n1 parts are built and off (TO-VERIFY 2026-11-09).
- Idle power on AC: about 10.5 W (SMC system power, 120 s idle, screen on; MEASURED 2026-11-09 on 1.617), against 7.5-7.7 W in the same check on 1.614 before 2026-11-09 04:56Z.
  - The cause is userland, not the kernel: since that time the J604 runs the edge `omarchy-power-profiles-daemon` 0.30-7, which adds Apple Silicon CPU profiles (SOURCE: its package description). With the stock profile on AC, `performance`, both CPU policies use the `performance` governor, so the cores idle at their top clock (MEASURED 2026-11-09). The earlier 0.30-3 had no Apple Silicon profile, and the kernel default `schedutil` applied.
  - This is the stock edge default, left as it is. A comparison with the `balanced` profile is TO-VERIFY.
- Battery life vs macOS: not measured yet (TO-VERIFY 2026-11-09).

### Touch ID
- **Working.** MEASURED 2026-11-09: used for sudo, polkit and the lock screen.
- Carried by: kernel series (Secure Enclave sensor node) and one `omarchy-mac` change (stock lists the J604 as not eligible).

### Boot chain
- **Working.** MEASURED 2026-11-09 (first boot of 1.617): m1n1 → U-Boot → Limine → kernel; passphrase prompt with the keyboard working; no failed units; no kernel warnings or oopses.
- `omarchy-mac-boot` 1.614 drops the earlier first-boot drop-in for `omarchy-plymouth-quit.service`, a unit stock does not ship.

## Known differences from stock Omarchy on this Mac

1. **mpv hardware decode default never lands** (all Apple Silicon Macs): `omarchy-settings` owns `/etc/mpv/mpv.conf`, so `omarchy-mac`'s Apple default is never installed (MEASURED 2026-11-09). Issue: omacom/omarchy#18229.
2. **kyber fails on two NVMe namespaces with Linux 7.2** (MEASURED 2026-11-09). Fix: omacom/omarchy#18228.
3. **Touch ID not offered by stock `omarchy-mac`** on the T8132. We carry a two-line change.
4. **Unlock prompt can go black while typing** with stock Plymouth on device-tree Macs. We carry a small Plymouth patch.
5. **Glassy bar at about 59 fps** on this 120 Hz panel with stock (MEASURED 2026-11-09). 119 fps needs omacom/omarchy#18176.
6. **CPU power profile on AC.** The edge `omarchy-power-profiles-daemon` 0.30-7 applies its `performance` profile on AC to Apple Silicon, so the CPUs idle at their top clock and idle power on AC is about 10.5 W (MEASURED 2026-11-09). This is the stock default, not a J604 change.
7. **Built-in panel defaults.** Stock Omarchy drives every output at 8 bits per channel and gives Apple Silicon displays Hyprland's EDID colour preset. On this Mac our `omarchy-mac` 2.12 and `omarchy-settings-dev` 1.4 make the built-in panel 10-bit and Display P3 by default (MEASURED 2026-11-09); external monitors are left alone, and a `monitors.lua` setting still wins. Both changes are drafted for upstream (see `UPSTREAM.md`).
