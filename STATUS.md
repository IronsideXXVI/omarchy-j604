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
| Desktop | `omarchy-dev` 4.0.0.r7641.gf97d3e7, local pkgrel `1.1` (stock edge + the keyboard-backlight series, 4 patches); `omarchy-settings-dev` same version, local `1.1` (the same 4 patches + the built-in panel defaults, 1 patch); Hyprland 0.57.1, stock | official recipes + 4 / 5 patches |
| Mac add-on | `omarchy-mac` 0.2.3, local `2.12`: keyboard-backlight idle default, Apple F-row key names, Touch ID eligibility, the 14-inch MacBook Pro panel line (10-bit, Display P3) | official recipe + 4 patches |
| Kernel | `linux-aurora` 7.3.0.aurora1, local `1.716` (203 patches on aurora-silicon/linux `integration/aurora` `6ff31cbfa2`: 1.706 plus 0177-0203), running `7.3.0-1-1.716-ARCH` since 2026-11-10 | official recipe (source moved to the 7.3 base) + J604 series |
| Boot chain | `m1n1-aurora` 1.6.1.aurora22 (local `1.608`), `uboot-aurora` 2026.07.aurora6 (local `1.604`), `omarchy-mac-boot` 20261104 (local `1.616`), `plymouth` 26.134.222 (local `3.604`), `asahi-scripts` 20261023 (local `1.2`); boot image (`boot.bin`): m1n1 + the 1.711 J604 device tree + U-Boot, 4,700,195 bytes | official recipes + J604 patches |
| Graphics | `mesa-aurora` / `vulkan-aurora` with the T8132 GPU admitted and the 7.3 kernel's "aurora" driver name accepted (local `1.8133`) | official recipe + 2 patches |
| Audio | `alsa-ucm-conf-aurora` (local `1.605`), `aurora-audio` (local `3.607`), `speakersafetyd` (local `1.605`) with J604 profiles | official recipes + J604 files |
| Firmware | `asahi-fwextract` 0.9.2 (local `1.609`): extra camera tuning files, and keeps the M4 (BCM4388 rev C2) Wi-Fi firmware when firmware is re-extracted | official recipe + 2 patches |
| Touch ID library | `libfprint-git` 1:1.94.100.r10.g6f9479c, local `5.604`: accepts Secure Enclave biometric interface v4 and v5 | official recipe + 1 patch |
| Video decode driver | `libva-v4l2_request-avd` 1.3.r11.r169.gc653eaa, local `1.604`: adds the `aurora_drv_video.so` alias, so VA-API finds the decoder on the 7.3 kernel (until omacom/omarchy-pkgs#2375 ships) | official recipe + 1 recipe line |
| Neural Engine userland | `omarchy-ane`, `omarchy-ane-dkms`, `mil-hwx-compiler`, `omarchy-mac-ml`: official edge builds, not carried by us | official |

Kernel 7.3 gate (MEASURED 2026-11-09, first boot of 1.703 with its J604 device tree): no failed units; no oops, panic or kernel warning; display, GPU, speakers (`speakersafetyd` active), keyboard backlight, Touch ID sensor, camera driver, Wi-Fi, Bluetooth and the SD reader controller bound; no deferred probes. Rows below that this gate did not check keep the build and date they were measured on.

Kernel 1.704 gate (MEASURED 2026-11-09, first boot of 1.704, one reboot): Hyprland running; no failed units; no driver probe failures; no firmware load failures; PCIe links up with no failures. The install left `boot.bin` unchanged (same J604 device tree as 1.703).

## What changed with kernel 7.3 (1.703 to 1.716, over 1.617)

CODE unless a line says otherwise.

- **New base.** The series now applies to aurora-silicon/linux `integration/aurora` `6ff31cbfa2` (7.3.0). The official `linux-aurora` recipe still pins 7.2.11; our build keeps that recipe and moves its source to the 7.3 base. Aurora's base already contains DjDeveloperr's J604 / M4 device trees and much of what 1.617 carried, so the J604 series is 163 patches instead of 766.
- **0001-0156**: the J604 commits Aurora's base does not have, re-applied. Most of what 1.617 added over 1.614 is now 0142-0155 (external display 10-bit / HDR10 / EDID / DP audio and idle power-down, ProRes encoder probing on the T8132, ANE DART suspend, Bluetooth wideband speech, and the ISP DART filter ranges in 0148).
- **0157**: one integration commit for the rest of the 1.617 J604 series, resolved onto Aurora's files. The J604 device-tree differences from Aurora's `t8132-j604.dts` are in one file, `t8132-j604-lab.dtsi` (single fan, the mTPL pressure key, JPEG match, and the fix that lets the camera driver bind).
- **0158**: the internal display's DARTs are admitted at boot on the J604 again, as on 1.617. On the 7.3 base Aurora's gate needs J713 kernel parameters that an Omarchy install does not carry, so the display stayed on the boot framebuffer (MEASURED 2026-11-09 on 1.701).
- **0159**: the sound driver binds the J604 with the J616s configuration again, as on 1.617. Aurora 7.3's own J604 entry uses a configuration without speakers, and it came first, so the speakers stayed disabled (MEASURED 2026-11-09 on 1.701).
- **0160**: the Touch ID sensor runs in SPI mode 1 (MEASURED 2026-11-09: "mode 1 (CPOL=0 CPHA=1)").
- **0161**: the J604 keeps the PCIe radio setup it had on 1.617; with the MacBook Air setup from the shared device-tree file, the Wi-Fi port failed (CODE). With 0161, Wi-Fi and Bluetooth are bound on the 7.3 device tree (MEASURED 2026-11-09).
- **0162**: the external display controller dcpext0 uses the J604's ATC2 PHY (MEASURED 2026-11-09: it starts, with no probe timeout).
- **0163**: a mailbox send that waits can be killed again, but not interrupted (as in 1.617).
- **0164** (1.704, the only change over 1.703): the GPU driver's GPUVM exec lock takes a reservation that two GEM objects share only once (`DRM_EXEC_IGNORE_DUPLICATES`, as the C users of `drm_gpuvm_exec` and `drm_exec` do). Without it, a client that closes an imported dma-buf and imports the same dma-buf again while the GPU is still busy got its next submission refused with EFAULT: `VK_ERROR_DEVICE_LOST` and a blank picture for 1080p zero-copy video (MEASURED 2026-11-09 on 1.703). The same code is in 1.617, and upstream aurora-silicon/linux `integration/aurora` still has it (SOURCE, checked 2026-11-09 at `d7303c39e215`). 1.704 is otherwise the same build as 1.703: same kernel config, byte-identical device trees, the package's own tests pass as on 1.703 (MEASURED 2026-11-09, build), so `boot.bin` keeps the same J604 device tree. With 1.704, stock mpv's zero-copy video shows the correct picture at 1080p and the kernel logs no submit error (MEASURED 2026-11-09).
- **0165-0167** (1.705): the suspend-to-RAM driver is now offered whenever the boot firmware publishes its mailbox and reset vector and every core's reset vector is locked to it; suspend-to-idle stays the default (a new `suspend_set_ops_keep_s2idle()` helper). On 1.705 `/sys/power/mem_sleep` reads `[s2idle] deep` (MEASURED 2026-11-09). A real deep sleep is not possible yet: the GPU driver refuses any sleep that powers the SoC down once its firmware has run, because the M4 GPU has no firmware stop and warm restart yet (CODE).
- **0168-0175** (1.705): eight USB4/Thunderbolt and USB-C PD fixes from aurora-silicon/linux `integration/aurora` that landed after our base and apply to the M4 (NHI started only with its DART, XDomain teardown and DMA-path checks, DROM entries for missing ports, the TI PD controller's LDCM startup and removal, a port restart when the peer link is lost, the USB4 router taken again after a rebind). Kept with their upstream authors. They act only when a USB4/Thunderbolt partner is plugged in; nothing plugged in behaves as on 1.704 (MEASURED 2026-11-09, gate).
- **0176** (1.706): a module parameter of the USB-C PHY driver (`t8132_usb4_pipe`, off by default) that lets USB3 run over a USB4 link for testing; with the default the driver makes the same decision as before (CODE). Nothing plugged in behaves as on 1.705 (MEASURED 2026-11-10, gate).
- **0177** (1.707): the GPU governor's idle floor becomes a runtime parameter; the default is unchanged (CODE).
- **0178-0180** (1.708): GPU command submission experiments for the browser animation stutter, all with defaults that behave as 1.707: an order switch for the two render records, an adjustable completion re-poll interval, and start/retire latency counters in debugfs. They do not fix the stutter (MEASURED 2026-11-10: 90.5 fps in the same animation test as on 1.707); the stutter stays open.
- **0181-0185** (1.710): power. 0181 adds an off-by-default switch to try a GPU firmware stop and warm restart across suspend-to-RAM; 0182 lets an idle J604 GPU drop to its lowest power state after 100 ms without work (a default change, J604 only); 0183-0184 report and, behind a write-only runtime switch, set the CPU cluster power-gate words. **0185 fixes a regression**: from 1.705 to 1.708 the suspend-to-RAM driver also ran for suspend-to-idle, and the GPU driver then refused the sleep, so suspend (and suspend on lid close) did not happen (MEASURED 2026-11-10 on 1.708: "refusing a sleep that powers the SoC down", "Some devices failed to suspend"). On 1.710 a suspend-to-idle cycle enters and resumes again (MEASURED 2026-11-10, one `rtcwake -m freeze` cycle).
- **0186** (1.710): a debugfs histogram of the GPU firmware start delay, to choose the next stutter fix (no behaviour change).
- **0187-0188** (1.710): the Neural Engine driver powers the engine up at boot on the J604 and powers it off while idle (CODE). On 1.711, with no option set, the engine powers up at boot, runs programs bit-exact and powers off after 1 s idle (MEASURED 2026-11-10).
- **0189-0193** (1.711), all off by default, so with the defaults the GPU and display behave as on 1.710 (CODE): 0189 lets the GPU publish a render command behind a running one (`aurora.g16_flights`, default 1), 0190 gives J604 panel swaps a presentation target (`appledrm.swap_target`, default 0), 0191 splits the firmware start-delay histogram by address-space switches (debugfs only), 0192 is a test switch for DP Alt Mode pin assignment D, and 0193 lets an idle HDMI output release its PHY and crossbar.
- **0194** (1.711): a device-tree change that enables the J604's SIO coprocessor and the HDMI port's audio. On 1.711 the SIO firmware starts (protocol v10), the HDMI audio device binds, and nothing is deferred (MEASURED 2026-11-10). Sound to an HDMI display is not tested (TO-VERIFY). With the new device tree, `boot.bin` is 4,700,195 bytes: the stock `update-m1n1` wrote it from the 1.711 package, and a trial boot confirmed it (MEASURED 2026-11-10).
- **0195** (1.712): `appledrm.swap_pace` (default off) holds an internal-panel swap that would land between two 120 Hz refreshes until the next one, so frames stay on the 120 Hz grid; with the default the display behaves as on 1.711 (CODE). The device tree is the same as in 1.711, so `boot.bin` is unchanged (CODE: same device-tree checksum).
- **0196-0197** (1.713), both off by default, so with the defaults the display behaves as on 1.712 (CODE): 0196 lets `swap_pace` learn its lead from the shortest start-to-completion time of recent swaps (`appledrm.swap_pace_lead_us=0`, used only while `swap_pace` is on); 0197 adds `appledrm.panel_adaptive` and `appledrm.panel_media_hz`, which send the panel's adaptive-sync request without the compositor's VRR, for measuring. The device tree is the same as in 1.711, so `boot.bin` is unchanged (CODE: same device-tree checksum).
- **0198-0199** (1.714), both off by default, so with the defaults the GPU and display behave as on 1.713 (CODE): 0198 lets a render command reuse the parameter-buffer manager of a finished command of the same program and skip its setup step (`aurora.g16_pb_reuse`), to measure whether that removes part of the fixed start cost per command; 0199 makes `swap_pace` start a held swap a quarter tick clear of the refresh boundaries, so a held frame lands on the 120 Hz grid. The device tree is the same as in 1.711, so `boot.bin` is unchanged (CODE: same device-tree checksum).
- **0200** (1.715): `appledrm.panel_media_word` chooses which word of the panel's adaptive-sync request carries the media target rate (`panel_media_hz`, 0197); nothing is sent differently while `panel_media_hz` is 0, the default, so the display behaves as on 1.714 (CODE). The device tree is the same as in 1.711, so `boot.bin` is unchanged (CODE: same device-tree checksum).
- **0201-0203** (1.716): 0201 makes `aurora.g16_pb_reuse` on by default, so a render command reuses the buffer manager of a finished command of the same program instead of setting up a new one; writing 0 to `/sys/module/aurora/parameters/g16_pb_reuse` turns it off at run time (CODE). 0202 makes `swap_pace` learn the firmware's floor only from swaps it did not hold (still off by default), and 0203 adds `swap_pace` counters to debugfs with no behaviour change (CODE). The device tree is the same as in 1.711, so `boot.bin` is unchanged (CODE: same device-tree checksum).
- 1.709 was a trial of a device-tree change for the SIO and the HDMI port's audio; it failed its boot gate (an SIO register window overlapped its mailbox, and a parent power domain was disabled) and was rolled back before it shipped (MEASURED 2026-11-10). The corrected change is 0194 in 1.711.
- Two 7.3 base changes need userland updates, both installed: the GPU's DRM driver is now called "aurora" (Mesa 1.8133), and the Secure Enclave biometric interface is version 5 (`libfprint-git` 5.604). The renamed GPU driver also needs a VA-API driver alias: `libva-v4l2_request-avd` 1.604, installed again with kernel 1.704 on 2026-11-09 (the first install was rolled back because of the GPU fault that 0164 fixes; see Media engines).
- **Boot image.** Since 2026-11-09 `boot.bin` holds only the J604 device tree of the kernel package (4,700,171 bytes from 1.703 to 1.710); it booted and was confirmed (MEASURED 2026-11-09). With the 1.711 device tree it is 4,700,195 bytes, written by `update-m1n1` and confirmed by a trial boot (MEASURED 2026-11-10). The default `update-m1n1` takes all 137 device trees of the 7.3 package (13.58 MB), which m1n1 stage 1 refuses (MEASURED 2026-11-09; see `UPSTREAM.md` note (b)). Since `omarchy-mac-boot` 1.616 (installed 2026-11-09) `update-m1n1` builds that J604-only image itself: its config picks the one device tree of the newest kernel whose first compatible is the running Mac's. The image it wrote was byte-identical to the confirmed one every time (three runs, at the 1.615 and the 1.616 install), and the official boot check passes (MEASURED 2026-11-09).

The stock official packages cannot boot this Mac yet (MEASURED 2026-11-09 on stable aurora14 and edge aurora15). The stock `linux-aurora` ships a J604 device tree that describes only the CPUs, interrupt controller, power states, pin control and serial port: no storage controller, display, keyboard or trackpad, USB, Wi-Fi or GPU. Stock m1n1 and U-Boot accept only the J713 (MacBook Air M4) on their M4 hand-off paths (INFERENCE 2026-11-09, from reading their source).

## At a glance

| Block | State | Evidence |
| --- | --- | --- |
| CPU frequency scaling and idle (10 cores, full clocks) | Working | MEASURED 2026-11-09 |
| CPU performance counters | Working: cycles and instructions count correctly on both core types (test-only DT setting) | MEASURED 2026-11-09 (1.704) |
| GPU (OpenGL 4.6 / Vulkan 1.4) | Working | MEASURED 2026-11-09 |
| GPU idle power | 0.041 W at idle on 1.710, 0.076 W on 1.711 (0.32-0.34 W on 1.704-1.707) | MEASURED 2026-11-10 |
| RAM (16 GB) | Working | MEASURED 2026-11-09 |
| Internal display (3024x1964 at 120 Hz, scale 2) | Working | MEASURED 2026-11-09 |
| 10-bit | Package default: compositor scans out XRGB2101010 with no user setting; panel link most likely 8 bpc with dithering | MEASURED 2026-11-09 (scanout); INFERENCE 2026-11-09 (link depth) |
| Display P3 | Package default (`cm = dp3`, kernel colour target Display P3); passed the visual check | MEASURED 2026-11-09 (setting, and the owner's visual check) |
| VRR | Off, fixed 120 Hz: the upstream Omarchy default (since 58443c82c), kept by the owner's decision | MEASURED 2026-11-09 |
| EDR / HDR | Panel and connector advertise HDR; the desktop runs SDR; no EDR headroom path yet | MEASURED 2026-11-09 (properties); TO-VERIFY 2026-11-09 (HDR output) |
| Glassy bar frame rate | Stock: about 59 fps at 120 Hz; 119 fps only with omacom/omarchy#18176 | MEASURED 2026-11-09 |
| Smoothness at 120 Hz (animation, video) | Animation: fixed by default on 1.716 (full-window animation 120.6 fps, 0 frames over 20 ms; about 90 fps before). Video: not fixed, 1080p60 in Chromium judders; a switch that is off by default puts it on its cadence but still slows GPU-heavy animation. Ordinary page scrolling holds 120 fps | MEASURED 2026-11-09/10 (1.704-1.716) |
| Brightness | Working (panel backlight) | MEASURED 2026-11-09 |
| Automatic brightness | Not shipped | MEASURED 2026-11-09 (not in the installed packages) |
| External display (USB-C / Thunderbolt), HDMI | In progress: both external display controllers start, the HDMI port and the USB-C display routes are registered, and on 1.711 the HDMI port's audio device binds; no monitor tested | MEASURED 2026-11-09/10 (nothing plugged in); TO-VERIFY (picture) |
| USB-C data | Working (USB devices) | MEASURED 2026-11-09 |
| Thunderbolt / USB4 | In progress: the three USB4 host routers bind with no errors; with nothing plugged in there is no USB4 domain, by design; no device tested | MEASURED 2026-11-09/10 (1.704-1.707); TO-VERIFY (devices) |
| External display 10-bit / HDR10 / EDID / DP audio, idle power-down | In the kernel since 1.617 (kept in 1.703), not tested | TO-VERIFY 2026-11-09 |
| SD card reader | In progress (controller bound on 1.704, no card tested) | MEASURED 2026-11-09 (controller); TO-VERIFY 2026-11-09 (card) |
| Storage (NVMe) | Working | MEASURED 2026-11-09 |
| Keyboard, trackpad | Working | MEASURED 2026-11-09 |
| Keyboard backlight | Working (on with activity, off after 10 s idle, Ctrl+F1/F2) | MEASURED 2026-11-09 |
| Wi-Fi | Working | MEASURED 2026-11-09 |
| Bluetooth | Working | MEASURED 2026-11-09 |
| Speakers (with speaker protection) | Working | MEASURED 2026-11-06 (playback); 2026-11-09 (owner's playback check on the 7.3 kernel; `speakersafetyd` active on 1.703) |
| Headphone jack | Working | MEASURED 2026-11-06 |
| Microphones | Working: three-mic array captures room sound; the default source is the processed mono microphone | MEASURED 2026-11-09 (1.704) |
| Camera | Working, tuned: 1080p30 and 720p30 (NV12) capture with no dropped frames; the sensor tuning file (isp_2224_01XX.dat) loads; seen by PipeWire, the Omarchy webcam tools and Chromium with no user settings. Colour accuracy and the camera-in-use indicator not verified | MEASURED 2026-11-10 (1.712 + asahi-fwextract 1.609); TO-VERIFY (colour by eye, indicator) |
| Hardware video decode (H.264, HEVC, VP9) | Working in stock mpv (`hwdec=auto-safe`, no user settings): VA-API zero-copy with kernel 1.704 and the VA-API driver alias (`libva-v4l2_request-avd` 1.604); picture identical to software decoding at 1080p (H.264, HEVC, VP9) and 4K (H.264), no GPU submit errors. Not checked: AV1, 10-bit and HDR, 4K HEVC/VP9, other players and browsers, battery effect | MEASURED 2026-11-09 (1.704 + 1.604); TO-VERIFY 2026-11-09 (not checked) |
| Video encode (AVE), JPEG, scaler, ProRes | In progress (ProRes driver in the kernel since 1.617, nodes disabled, not loaded) | TO-VERIFY 2026-11-09 |
| Neural Engine (ANE) | Working: on by default, /dev/accel/accel0, programs bit-exact; powers off when idle | MEASURED 2026-11-10 (1.711) |
| Ambient light sensor | Sensor reads; nothing uses it by default | MEASURED 2026-11-08 |
| Accelerometer, gyroscope, lid angle | Working at rest (plausible readings); lid-angle tracking while moving not checked | MEASURED 2026-11-09 (1.704, at rest); TO-VERIFY 2026-11-09 (moving lid) |
| Lid switch, suspend on lid close | Lid switch working; the suspend itself was refused on 1.705-1.708 and works again on 1.710 (see Suspend) | MEASURED 2026-11-09 (switch), 2026-11-10 (suspend); TO-VERIFY 2026-11-10 (a lid close on 1.710) |
| SMC sensors, fan, thermal readouts | Working (readouts) | MEASURED 2026-11-09 |
| Battery and charger | Working | MEASURED 2026-11-09 |
| Suspend (s2idle) | Working on 1.710 to 1.716; refused on 1.705-1.708 (regression, fixed by 0185). Each cycle logs a resume failure for both external display controllers | MEASURED 2026-11-10 |
| Idle power on AC | About 10.5 W with the stock edge `performance` power profile (7.5-7.7 W before 2026-11-09 04:56Z) | MEASURED 2026-11-09 |
| Deep sleep (suspend to RAM) | In progress: offered since 1.705 (suspend-to-idle stays the default); the first full trial, on 1.710, FAILED: the machine reset on wake | MEASURED 2026-11-09 (offered), 2026-11-10 (trial) |
| Battery life vs macOS | Not measured yet | TO-VERIFY 2026-11-09 |
| Touch ID (sudo, polkit, lock screen) | Working (sudo also on the 7.3 kernel, owner's check) | MEASURED 2026-11-09 |
| Boot chain (m1n1, U-Boot, Limine, disk unlock) | Working, no failed units on 1.703 and 1.704 with the 7.3 J604 device tree | MEASURED 2026-11-09 |

## Per block

### CPU
- **Working.** MEASURED 2026-11-09: 10 cores online; `apple-cpufreq` maximums 4464 MHz (performance) and 2892 MHz (efficiency); `apple_idle` cpuidle.
- On AC with the stock edge power profile (`performance`), both CPU policies run the `performance` governor and idle at their top clock (MEASURED 2026-11-09). See Power.
- The PMU counter device (`apple_t8132_pmu`, 8 counters) is present (MEASURED 2026-11-09 on 1.614). Its device-tree node is enabled for a test only (patch 0616 in 1.617; the J604 device-tree file of patch 0157 in 1.703): it is enabled for good only after a counter measurement passes. That measurement passed on 1.704 (MEASURED 2026-11-09): a fixed user-space loop retires the same instruction count on an efficiency and a performance core; the efficiency core's cycles match its 2.89 GHz clock. On a performance core the cycle count shows about 3.93 GHz while cpufreq reports 4.46 GHz: the cpufreq driver reads back the requested state on this chip, not the delivered one (CODE), so the reported performance-core clock is a request, not a measurement.
- Carried by: kernel series (CPU nodes, cluster frequency control, cpuidle, energy model). Aurora has an open pull request for M4 DVFS (#1035), which we test instead of sending our own.

### GPU
- **Working.** MEASURED 2026-11-09: the 10-core G16 GPU runs OpenGL 4.6 and Vulkan 1.4; on the 7.3 kernel its DRM driver is called `aurora` (on 7.2 it was `asahi`).
- GPU idle power: on 1.710 the GPU rail reads 0.041 W over 8 s at idle, down from 0.32-0.34 W on 1.704-1.707, and 0.076 W on 1.711 (MEASURED 2026-11-10). Since 0182 an idle J604 GPU drops to its lowest power state after 100 ms without work; animation frame pacing and stock-mpv hardware video were the same with it (MEASURED 2026-11-10).
- Carried by: kernel series and `mesa-aurora`/`vulkan-aurora` with two patches: one admits the T8132 GPU (stock Mesa refuses this GPU on purpose, omacom/mesa#25); the other is upstream aurora-silicon/mesa 6edf62bcfd0, which accepts the "aurora" DRM driver name of the 7.3 kernel as well as "asahi" (SOURCE 2026-11-09). Both names are in the built libraries, and with 1.8133 installed OpenGL and Vulkan report the Apple M4 GPU (G16G B0) on the 7.3 kernel, not llvmpipe (MEASURED 2026-11-09).

### Internal display
- **Working.** MEASURED 2026-11-09: 3024x1964 at 120 Hz, scale 2.
- Package defaults since 2026-11-09 (MEASURED 2026-11-09 after a reboot, with no display line in the owner's `monitors.lua`): 3024x1964 at 120 Hz (the panel's only mode), scale 2, 10-bit (the compositor scans out XRGB2101010) and Display P3 (Hyprland `cm = dp3`; the kernel logs "panel SDR colour: Display P3"; the connector stays on its default colorspace).
  - Carried by: `omarchy-mac` 2.12, whose `displays.conf` names the panel (`panel eDP 3024x1964 bitdepth=10 cm=dp3`), and `omarchy-settings-dev` (1.4 on r7634, 1.1 on r7637, now 1.1 on r7641, same patch), whose `panel-defaults.lua` applies that line to the built-in panel's monitor rules. A `bitdepth` or `cm` set in `monitors.lua` still wins. Mode and scale need no package setting: Hyprland picks 3024x1964 at 120 Hz and scale 2 on its own (CODE).
  - The panel link itself is most likely 8 bits per channel with dithering (INFERENCE 2026-11-09). The P3 picture passed the owner's visual check (MEASURED 2026-11-09: "Looks great").
- VRR: off (MEASURED 2026-11-09). That is the upstream Omarchy default: Omarchy turned VRR on and took it out again in 58443c82c because it adds a small lag (SOURCE). The panel supports 24-120 Hz. It stays off: a fixed 120 Hz, by the owner's decision (2026-11-09).
- EDR / HDR (MEASURED 2026-11-09 on 1.704, properties only): the panel's EDID carries an HDR static metadata block (SDR and PQ transfer functions), and the internal connector exposes HDR output metadata and colorspace properties. The desktop runs SDR (10-bit, Display P3, 80-nit SDR reference) with the backlight in its normal range; no path that hands EDR headroom above that range to the panel is exposed yet. 1.617 adds groundwork that takes the EDR headroom from the content (CODE); HDR output TO-VERIFY 2026-11-09.
- Glassy bar: stock holds about 59 fps on this 120 Hz panel; 119 fps needs the xray change in omacom/omarchy#18176 (MEASURED 2026-11-09). That change is not in our packages; the owner's own user toggle restores it locally.
- Smoothness at 120 Hz: **animation fixed by default since 1.716; video cadence not fixed yet.**
  - A full-window CSS animation in Chromium runs at about 90 fps instead of 120 (90.5 fps on 1.704, 1.708 and 1.710; MEASURED 2026-11-09/10). The 1.708 changes (0178-0180) did not change it. 1.711 adds two experiments behind switches that are off by default (0189 two render commands in flight, 0190 a presentation target for panel swaps); with the defaults it behaves as 1.710 (CODE). Turning the presentation target on (`appledrm.swap_target=2`) on 1.711 made 1080p60 video much worse (about 12 fps) and the display needed a reboot to recover, so it stays off (MEASURED 2026-11-10). 1.712 adds a host-side alternative, `appledrm.swap_pace` (0195), also off by default. Turned on, it gave no visible gain: 1080p60 video still used both phases of the 120 Hz refresh and the animation stayed at 90 fps (MEASURED 2026-11-10 on 1.712), because it waits only until the odd tick is out of reach, so a held frame still lands on either phase about half the time (CODE).
  - The panel's adaptive-sync request, tested with the compositor's VRR turned on for one run each: 60 fps video in Chromium went from 60 % to 91 % of frames on the 16.7 ms cadence, but 24 fps video got worse (71 % to 33 % on its 41.7 ms cadence) (MEASURED 2026-11-10 on 1.711 and 1.712). So it is not a fix; 1.713 adds switches to send that request on its own (0197, off by default).
  - 1.714 adds two switches, both off by default. Turned on at run time, each one fixed its half in a first test, one run each (MEASURED 2026-11-10 on 1.714, settings put back afterwards):
    - `aurora.g16_pb_reuse=1` (0198): the full-window animation went from 90.3 to 119.2 fps with 0 frames over 20 ms, and page scrolling stayed at 120 fps. Screenshots of a GPU-rendered test page were byte-identical with and without it. Skipping the per-command buffer-manager setup removes about 0.45 ms of the 0.7 ms start cost.
    - `appledrm.swap_pace=1` with `appledrm.swap_pace_lead_us=1750` (0199): 1080p60 video in Chromium was presented 100 % on the 16.7 ms cadence, all on one phase (59-60 % without), and 24 fps video had every frame on its 41.7 ms cadence. In that 60 fps run Chromium dropped 2 of 722 frames (0-8 without pacing); whether pacing adds drops is open. A held frame is shown up to one 4.17 ms tick later. Scrolling (120 fps) and mpv (every frame on cadence) were fine with it, but the GPU-heavy full-window animation dropped from 90 to 69 fps (85 fps with a 1.0 ms lead), so it is not ready as a default.
    - 1.716 turns `g16_pb_reuse` on by default (0201). With the default settings of 1.716 the full-window animation runs at 120.6 fps with 0 frames over 20 ms (MEASURED 2026-11-10; 90.5 fps with 129 frames over 20 ms on 1.708). `swap_pace` stays off by default, so the video points above still apply.
  - 1080p60 video in Chromium (software decode, the stock browser setting) dropped about 1 frame per second on 1.704 and 1.708 (6-10 of about 720 frames in 12 s). On 1.710 a run dropped 0 of 602 frames, but 28 % of the frames were shown off the 16.7 ms cadence, at 12.5 or 20.9 ms (MEASURED 2026-11-09/10). That is the visible judder.
  - Ordinary page scrolling holds 120 fps (MEASURED 2026-11-10 on 1.710: 0 frames over 12 ms).
  - The compositor settings are not the main cause: blur off gave the same 90 fps (MEASURED 2026-11-09). Under investigation in the GPU driver: the M4 GPU backend runs one render command at a time, and every command pays a fixed firmware start cost of about 0.7 ms before it runs (MEASURED 2026-11-10 on 1.710; CODE). For video, the panel shows each frame at the next 4.17 ms step, so a frame that arrives late is shown 4.17 ms late (MEASURED 2026-11-10).
- Brightness: the panel backlight works (MEASURED 2026-11-09). There is no automatic brightness in the installed packages.

### External displays, HDMI
- **In progress.** The kernel has the external display controllers, crossbar, DP PHY and HDMI bridge. 1.617 adds 10-bit scanout, HDR10, the sink's EDID and primaries, DP audio for an external controller, and power-down of an idle external controller (CODE). On 1.703 the external display controller dcpext0 starts with the J604's PHY (MEASURED 2026-11-09). No external monitor has been tested on the current build (TO-VERIFY 2026-11-09).
- HDMI audio: on 1.711 the SIO coprocessor runs (firmware protocol v10) and the HDMI port's audio device binds, with nothing deferred (MEASURED 2026-11-10). Sound to an HDMI display is not tested (TO-VERIFY).

### USB-C, Thunderbolt / USB4, SD card
- USB-C data: **Working** for USB devices (MEASURED 2026-11-09).
- Thunderbolt / USB4: **In progress.** The three USB4 host routers bind and register their USB-C switches, with no Thunderbolt or USB4 error lines (MEASURED 2026-11-09/10 on 1.704-1.707). With nothing plugged in there is no USB4 domain: the host router powers up only when the USB-C controller reports a USB4 or Thunderbolt partner (CODE), as on 1.6xx. No USB4 or Thunderbolt device has been tested yet (TO-VERIFY).
- USB-C before the disk unlock: since `asahi-scripts` 1.2 the initramfs carries the `dwc3-apple` module, so the USB-C controllers bind before the unlock prompt (MEASURED 2026-11-10 on 1.708: no "waiting for the USB role switch" at boot). Typing the passphrase on a USB-C keyboard is TO-VERIFY.
- SD reader: the controller (`sdhci-pci`, ADMA 64-bit) is bound on 1.704 with no error lines (MEASURED 2026-11-09); no card tested (TO-VERIFY 2026-11-09).

### Storage
- **Working.** MEASURED 2026-11-09: Apple NVMe with three namespaces; root on LUKS2 + btrfs.
- Stock udev rule: two of three namespaces stay on `mq-deadline` instead of `kyber` on Linux 7.2 (MEASURED 2026-11-09; not rechecked on 7.3, TO-VERIFY 2026-11-09). Fix proposed in omacom/omarchy#18228. Harmless for function (INFERENCE 2026-11-09).

### Keyboard, trackpad, keyboard backlight
- **Working.** MEASURED 2026-11-09: keyboard and multi-touch trackpad, also at the disk-unlock prompt.
- Keyboard backlight: **Working** with the local `omarchy-dev` / `omarchy-settings-dev` and `omarchy-mac` 2.12 builds (MEASURED 2026-11-09 on `omarchy-dev` 1.3 / `omarchy-settings-dev` 1.4; the r7637 and r7641 builds, local 1.1, carry the same patches byte for byte): on with activity, off after 10 s idle, Ctrl+F1/F2 change the level, and Super+K lists no keys the MacBook lacks.

### Wi-Fi and Bluetooth
- **Working.** MEASURED 2026-11-09: BCM4388 Wi-Fi connected; Bluetooth powered (also on 1.703 with the 7.3 device tree). A2DP playback MEASURED 2026-11-06.
- Bluetooth headset microphone (wideband): 1.617 makes the BCM4388 advertise wideband speech (CODE); headset check TO-VERIFY 2026-11-09.

### Audio
- Speakers: **Working**, with `speakersafetyd` protecting them (MEASURED 2026-11-06 playback; 2026-11-09 the owner's own playback on the 7.3 kernel, and the daemon active on 1.703).
- Headphone jack: **Working** (MEASURED 2026-11-06).
- Microphones: **Working** (MEASURED 2026-11-09 on 1.704, 3 s of room noise, nothing played): the raw array device has three live, distinct channels (about -90 to -95 dBFS in a quiet room); the default source "MacBook Microphone" (processed, mono output on two channels) records about -66 dBFS of room noise, not digital silence. Speech quality not judged.
- Carried by: J604 files in `alsa-ucm-conf-aurora`, `aurora-audio` and `speakersafetyd`, and the speaker amp configuration m1n1 passes on. Without `speakersafetyd` the kernel keeps the speakers muted.

### Camera
- **Working, tuned.** MEASURED 2026-11-10 on kernel 1.712 with asahi-fwextract 0.9.2-1.609: the FaceTime HD camera (`apple-isp`, /dev/video4) captures 1920x1080 and 1280x720 at 30 fps (NV12) with contiguous frame numbers and no driver errors, powers down between streams, and loads its sensor tuning file `isp_2224_01XX.dat` (taken from the camera daemon in the J604's own 25G83 recovery image, the way the official installer collects it). PipeWire lists it as a video source, the Omarchy webcam picker selects it, and Chromium opens it with stock settings (MEASURED 2026-11-09/10). With the tuning the picture is brighter (mean luma 32 -> 50 in the same scene); a slight magenta tint remains in an indoor night scene, not judged by eye yet. Not verified: the camera-in-use indicator under Linux (the driver notes that macOS draws it through secure display firmware).
  - Earlier, without the tuning file (1.704 and 1.705), the picture was dark with a magenta cast (MEASURED 2026-11-09/10).
  - Installing the tuning file re-extracts the firmware on the EFI partition. The stock `asahi-fwextract` 0.9.2-1 drops the M4 Wi-Fi firmware (BCM4388 rev C2) when it does that: one stock run left 0 of its 60 files (MEASURED 2026-11-10, rolled back before any reboot). With 1.609 the re-extraction keeps all of them, Wi-Fi and Bluetooth stay up after the reboot (MEASURED 2026-11-10).
  - The re-extraction also needs about the size of the vendor firmware free on the EFI partition (about 118 MiB here). We made room by moving two old boot images and an old snapshot kernel image off it (MEASURED 2026-11-10).
  - Earlier: on 1.701-1.703 the driver bound with no -EBUSY but no picture was taken.
  - On 1.617 its probe failed with -EBUSY (error -16): the ISP's DART (`apple-dart`) mapped its address-filter (`dapf`) register window, and the ISP driver then asked for the same window exclusively (MEASURED 2026-11-09 on 1.617). 1.703 carries the fix (one driver owns the window).
  - On 1.614 the probe also failed, earlier, with -19: the ISP's DART had no filter ranges and did not probe (MEASURED 2026-11-09 on 1.614).
- Carried by: kernel ISP series, m1n1 camera firmware hand-off, `asahi-fwextract` (extra sensor tuning files; since 1.609 it also keeps the BCM4388 rev C2 Wi-Fi firmware).

### Media engines
- H.264 hardware decode: worked on 1.617 (MEASURED 2026-11-08, 4K through VA-API). With stock settings on 1.617, mpv's system default did not use it (omacom/omarchy#18229).
  - On the 7.3 kernel: **Working in stock mpv** since 2026-11-09 (kernel 1.704 + `libva-v4l2_request-avd` 1.604; results below). The GPU's DRM driver is now called "aurora", so libva looks for `aurora_drv_video.so`. Stock `libva-v4l2_request-avd` -1 ships only the `asahi` name, so VA-API did not load and mpv with the stock config decoded in software, with a correct picture (MEASURED 2026-11-09). `libva-v4l2_request-avd` 1.604 adds the `aurora` alias, as upstream aurora-silicon/alarm-aurora aedcf240ca91 does (SOURCE).
  - The first install of 1.604, on kernel 1.703, lasted about 12 minutes on 2026-11-09 (21:31 to 21:43 UTC) and was rolled back the same day. With VA-API loading, stock mpv (`hwdec=auto-safe`) took the zero-copy path into its Vulkan output and showed an empty window at 1080p for H.264, HEVC and VP9 (3 of 3 runs each), with `VK_ERROR_DEVICE_LOST` and a kernel GPU submit error (`EFAULT`); 4K H.264 was correct (MEASURED 2026-11-09). With a copy path (`--hwdec=vaapi-copy`) all three codecs decoded in hardware with a correct picture while it was installed (MEASURED 2026-11-09).
  - Cause found (2026-11-09): a kernel bug, not the alias. mpv re-imports the decoder surface every frame and closes the old handle; when the same dma-buf is imported again while the closed import still has GPU mappings (cleanup waits for the GPU's in-flight jobs), two GEM objects share one reservation, the GPU driver's exec lock meets it twice, and the submission is refused with EFAULT (MEASURED 2026-11-09 on 1.703 with an ioctl trace of mpv; CODE). At 4K the re-import got the old GPU address back, which removes the stale mapping first, so 4K was not hit (MEASURED 2026-11-09; the allocator reasoning is INFERENCE). The bug is old: 1.617 has the same code (CODE); it was not seen before because VA-API zero-copy into Vulkan had not run on this Mac. Patch 0164 in kernel 1.704 fixes it (CODE).
  - With kernel 1.704, 1.604 was installed again (2026-11-09, 22:22 UTC). Stock mpv with the shipped `/etc/mpv/mpv.conf` (`hwdec=auto-safe`) and no overrides decoded 1080p H.264, HEVC and VP9 and 4K H.264 in hardware ("Using hardware decoding (vaapi)", the zero-copy path) in every run (3 seek+pause and 3 play-from-start per clip); the picture matched software decoding in every check (difference 0.00), and the kernel logged no GPU submit error (MEASURED 2026-11-09).
  - Not checked yet: AV1, 10-bit and HDR clips, 4K HEVC/VP9, the VA profile list, other players and browsers, battery effect (TO-VERIFY 2026-11-09).
  - Carried by: kernel 1.704 (patch 0164) and `libva-v4l2_request-avd` 1.604, until the official `-2` from omacom/omarchy-pkgs#2375 (the same change) replaces it.
- Video encode (AVE), JPEG, scaler, ProRes: **In progress** (TO-VERIFY 2026-11-09). The kernel carries the ProRes encoder driver from Aurora (since 1.617); its device-tree nodes are disabled and the module does not load (MEASURED 2026-11-09 on 1.617).

### Neural Engine (ANE)
- **Working on 1.711** (MEASURED 2026-11-10, default settings): the engine powers up at boot (firmware H16 26.6.2, build 25G83), `/dev/accel/accel0` appears, the add-128 and 128x128 matrix-multiply programs give bit-exact results, the engine's 5 power domains and 3 DARTs power off 1 s after the last request and come back on the next one, and the kernel logs no ANE errors.
- Before that, MEASURED 2026-11-09/10 (kernels 1.704 and 1.708): loaded with `ane_t6021.h16_power_unqualified=1`, the M4 Neural Engine powers up, its firmware starts (H16 26.6.2, build 25G83), and `/dev/accel/accel0` appears.
  - Programs compiled by the official `mil-hwx-compiler` (a 128x128 matrix multiply, compiled on the laptop itself) and the kernel's add-128 test program give bit-exact results against a CPU reference, in about 0.2-0.4 ms per run. A softmax program stays within 9 ULP (bound 32).
  - The engine powers off and boots again across a suspend cycle, and gives the same results afterwards.
  - Without the option, 1.704-1.708 stop after identifying the firmware (no `/dev/accel`). 1.710 (0187-0188) powers it up by default and off while idle; measured on 1.711 above.
  - The official `omarchy-ane` tools do not support the M4 yet (`omarchy-ane-smoke`: "t8132: no omarchy-ane driver for this chip").
- m1n1 1.608 hands the preloaded ANE firmware over (m1n1 patch 0028). 1.617 lets the ANE's DARTs suspend once the device is bound (CODE). The ANE userland (`omarchy-ane`, `omarchy-ane-dkms`, `mil-hwx-compiler`, `omarchy-mac-ml`) is the official edge build; our earlier rebuilds are withdrawn. `omarchy-ane-dkms` skips its build on our kernels by design, because they ship the ANE driver in the kernel (MEASURED 2026-11-09: the same skip on 1.614, 1.617, 1.701 and 1.702).

### Sensors
- Ambient light: the sensor reads (MEASURED 2026-11-08). Nothing uses it by default; automatic brightness is not shipped.
- Accelerometer, gyroscope, lid angle (MEASURED 2026-11-09 on 1.704, at rest): the accelerometer reads 9.80 m/s2 total with the machine flat, the gyroscope reads near zero, the lid-angle sensor reads 127 degrees with the lid open, and the ambient light sensor changes with the room. Lid-angle tracking while the lid moves: TO-VERIFY 2026-11-09.
- Lid switch: **Working**, suspend on lid close (MEASURED 2026-11-09).
- SMC temperatures, fan, power rails: **Working** readouts (MEASURED 2026-11-09). Fan control parity with macOS: not started.
- Battery and charger: **Working** (MEASURED 2026-11-09).

### Power
- Suspend (s2idle): **Working on 1.710 to 1.716** (MEASURED 2026-11-10, on each: `rtcwake -m freeze -s 8` returns, the journal shows suspend entry and exit, nothing refused, the desktop is alive). Each cycle also logs a resume failure for both external display controllers (`Retained DCP session quarantined: -62`, then `PM: failed to resume: error -62`); the internal panel's controller is not affected. This happens on every boot with a suspend cycle since 1.710 and on none without (MEASURED 2026-11-10). An external display after a suspend may need a reboot (INFERENCE; not tested).. **Refused on 1.705-1.708** (MEASURED 2026-11-10 on 1.708): the suspend-to-RAM driver added in 1.705 also ran for suspend-to-idle and the GPU driver refused the sleep; 0185 (1.710) fixes it.
- Deep sleep (suspend to RAM): **In progress.** Since 1.705 the kernel offers it (`[s2idle] deep`, MEASURED 2026-11-09); the boot firmware already publishes what it needs (no m1n1 change). Suspend-to-idle stays the default, so closing the lid behaves as before. 1.710 adds the GPU firmware stop and warm restart a full deep sleep needs, behind a switch that is off by default (0181). The first full suspend-to-RAM trial, on 1.710 with that switch on, FAILED: the machine went down and came back with a cold boot about 10 s after the wake alarm, so the wake did not reach the kernel; no hang, no power cycle needed (MEASURED 2026-11-10). No log of the attempt survived, so where it stopped is not known yet (TO-VERIFY).
- Idle power on AC: about 10.5 W (SMC system power, 120 s idle, screen on; MEASURED 2026-11-09 on 1.617), against 7.5-7.7 W in the same check on 1.614 before 2026-11-09 04:56Z.
  - The cause is userland, not the kernel: since that time the J604 runs the edge `omarchy-power-profiles-daemon` 0.30-7, which adds Apple Silicon CPU profiles (SOURCE: its package description). With the stock profile on AC, `performance`, both CPU policies use the `performance` governor, so the cores idle at their top clock (MEASURED 2026-11-09). The earlier 0.30-3 had no Apple Silicon profile, and the kernel default `schedutil` applied.
  - This is the stock edge default, left as it is. A comparison with the `balanced` profile is TO-VERIFY.
- Battery life vs macOS: not measured yet (TO-VERIFY 2026-11-09).

### Touch ID
- **Working.** MEASURED 2026-11-09: used for sudo, polkit and the lock screen.
- Carried by: kernel series (Secure Enclave sensor node; on 1.703 the sensor runs in SPI mode 1), one `omarchy-mac` change (stock lists the J604 as not eligible), and `libfprint-git` 5.604.
- The 7.3 kernel base moves the Secure Enclave biometric interface from version 4 to 5 (SOURCE: aurora-silicon/linux f70f1acd35). Stock `libfprint-git` -5 accepts only version 4, so on a 7.3 kernel fprintd ignores the sensor ("interface version 5, expected 4"; MEASURED 2026-11-09). `libfprint-git` 5.604 (installed 2026-11-09) accepts 4 and 5; with it fprintd finds the sensor on the 7.3 kernel (MEASURED 2026-11-09), and Touch ID for sudo works there by the owner's check on 1.702 (MEASURED 2026-11-09). The prints enrolled before the move still match (INFERENCE 2026-11-09: no new enrolment was made).

### Boot chain
- **Working.** MEASURED 2026-11-09 (first boot of 1.703 with the 7.3 J604 device tree): m1n1 → U-Boot → Limine → kernel; no failed units; no kernel warnings or oopses. Passphrase prompt with the keyboard working (MEASURED 2026-11-09 on 1.617).
- `boot.bin` holds m1n1 1.608, the J604 device tree of the 1.711 package and U-Boot (4,700,195 bytes, MEASURED 2026-11-10). The all-device-tree image the stock `update-m1n1` default builds for the 7.3 package is refused by m1n1 stage 1 (MEASURED 2026-11-09).
- `omarchy-mac-boot` 1.616 makes `update-m1n1` build that J604-only image itself (MEASURED 2026-11-09: byte-identical to the confirmed image in all three runs, at the 1.615 and the 1.616 install; the official boot check passes). Its config, `/etc/default/update-m1n1`, sets `DTBS` to the one device tree of the newest kernel whose first compatible is the running Mac's, and keeps the stock default in every other case (an explicit `DTBS`, `OMARCHY_DTB_MACHINE=0`, no `fdtget`, a chroot, zero or several matches, or an overlay that names that device tree) (CODE). The boot check reads the same file and expects the same image. The package depends on `dtc` for `fdtget`: without it the config falls back to the all-device-tree image (CODE).
- `omarchy-mac-boot` (since 1.614) drops the earlier first-boot drop-in for `omarchy-plymouth-quit.service`, a unit stock does not ship.

## Known differences from stock Omarchy on this Mac

1. **mpv hardware decode default never lands** (all Apple Silicon Macs): `omarchy-settings` owns `/etc/mpv/mpv.conf`, so `omarchy-mac`'s Apple default is never installed (MEASURED 2026-11-09). Issue: omacom/omarchy#18229.
2. **kyber fails on two NVMe namespaces with Linux 7.2** (MEASURED 2026-11-09; not rechecked on 7.3). Fix: omacom/omarchy#18228.
3. **Touch ID not offered by stock `omarchy-mac`** on the T8132. We carry a two-line change.
4. **Unlock prompt can go black while typing** with stock Plymouth on device-tree Macs. We carry a small Plymouth patch.
5. **Glassy bar at about 59 fps** on this 120 Hz panel with stock (MEASURED 2026-11-09). 119 fps needs omacom/omarchy#18176.
6. **CPU power profile on AC.** The edge `omarchy-power-profiles-daemon` 0.30-7 applies its `performance` profile on AC to Apple Silicon, so the CPUs idle at their top clock and idle power on AC is about 10.5 W (MEASURED 2026-11-09). This is the stock default, not a J604 change.
7. **Built-in panel defaults.** Stock Omarchy drives every output at 8 bits per channel and gives Apple Silicon displays Hyprland's EDID colour preset. On this Mac our `omarchy-mac` 2.12 and `omarchy-settings-dev` 1.4 make the built-in panel 10-bit and Display P3 by default (MEASURED 2026-11-09); external monitors are left alone, and a `monitors.lua` setting still wins. Both changes are drafted for upstream (see `UPSTREAM.md`).
