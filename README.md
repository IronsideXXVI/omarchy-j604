# Omarchy on the MacBook Pro 14-inch (M4, 2024): J604 bring-up

This repository is the working set for running [Omarchy](https://github.com/omacom/omarchy) on the Apple MacBook Pro 14-inch with the base M4 chip (Mac16,1, board J604, SoC T8132, the same SoC as the M4 MacBook Air J713/J715). It's part of the Omarchy for Apple Silicon effort, and the J604 is a daily machine that's available for testing.

## Principle

The J604 runs the official Omarchy **edge** channel (switched with the official `omarchy-channel-set edge` on 2026-11-09): `omarchy-dev` and `omarchy-settings-dev` built from the `quattro` branch, plus the official `omarchy-mac`. This repository carries only what the stock packages lack for this Mac:

- the hardware enablement (kernel, boot chain, Mesa admission, audio profiles, firmware extraction, initramfs and unlock-prompt fixes, Touch ID eligibility, the Touch ID library update the 7.3 kernel's Secure Enclave interface needs, and the VA-API driver alias the 7.3 kernel's renamed GPU driver needs);
- five userland additions:
  - the keyboard-backlight series in `omarchy-dev` / `omarchy-settings-dev` (the backlight comes on with activity, goes off after 10 s idle, Ctrl+F1/F2 change it, and Super+K leaves out keys a MacBook doesn't have);
  - built-in panel defaults: `omarchy-settings-dev` reads a `panel` line from the platform package, and `omarchy-mac` names this Mac's panel there (10-bit, Display P3). A setting in the user's `monitors.lua` still wins;
  - blur only where it can show (the owner's decision, to be proposed upstream): Hyprland blurs only inside a surface's declared blur region, and the Omarchy bar declares its strip as that region. The look is unchanged; the 120 Hz composite rate with blur on is still open (see `STATUS.md`);
  - less GPU memory for the shell: `omarchy-dev` asks for no depth or stencil buffer in the shell's windows, which draw flat 2D layers (pixel check: idle desktop identical; with a toast and with the panel open, 17 edge pixels differ by one step (1/255) in one channel; nothing visible);
  - the NVMe scheduler on every namespace: `omarchy-settings-dev` carries omacom/omarchy#18228, so the udev rule retries the `kyber` write while a sibling namespace switches, instead of leaving all but one on the kernel's pick.

Everything else is stock. Each piece is a patch on top of an official recipe, built with a local `pkgrel`. Earlier userland changes that are not installed are listed in [`not-installed/README.md`](not-installed/README.md).

It uses the official chain only: the [linux-aurora](https://github.com/omacom/linux-aurora) kernel with a J604 patch series, Aurora's [aura](https://github.com/aurora-silicon/aura) boot loader and U-Boot, the official installer, Omarchy edge, and normal `omarchy update`.

Changes go to the official repositories as reviewed pull requests, one topic at a time, in whatever shape the team prefers. Until each one lands, this repository holds the full set so anyone can read and rebuild it. Much of the kernel work is other people's: carried with its authorship, or already in Aurora's 7.3 base (see Credits and `CREDITS.md`).

## Status

**Current release status (2026-11-24):** `linux-aurora-next` 1.1307 is the promoted boot default, with Mesa/Vulkan 1.8303 and Hyprland 0.57.2-21.607. The separate `linux-aurora` slot is the development slot: it held 1.1289, the previous default, when 1.1307 was committed, and takes the development kernels after it, which are not promoted or included in this repository's 1.1307 kernel series. Versions 1.1290 to 1.1306 were not released or published. A temporary trial boot does not change which release is the default.

**GPU correctness is still open.** The two verifying stress tests have not yet been run on 1.1307, 1.1289, 1.1258, 1.1230, 1.1212, 1.1196 or 1.1143. On 1.1132 with Mesa 1.8149 both passed: the concurrent render and compute stress test (`cr.concurrent`) and the deep-queue stress test (`queue.depth`), 3 of 3 seeds each with no GPU fault. That is a pass of two tests, not a correctness or conformance result. The earlier failures stay stated: `cr.concurrent` failed on every seed from 1.1014 to 1.1038, and `queue.depth` reproduced incorrect GPU readback data on every kernel up to 1.1052, including runs with no logged GPU faults. The timing measurements below remain measured results; they are not a correctness or conformance pass. One limit is known, and one that 1.1115 had stays bounded. The limit: the mixed-load cost stated for 1.1088 remains: the concurrent render and compute stress test passed and completed 61,013 to 62,064 checked results in 12 s on 1.1132 against 60,559 to 61,086 on 1.1125 (one boot each), and 1.1088 measured 8.6% fewer in 12 s than with the session check repeated in every pass (61,396 against 67,144); the cause is known, and a first fix was measured on a development kernel and did not meet its bar, so the cost remains. The bounded one: low-priority GPU work could starve, because the driver served the highest-priority queue that had work ready and waiting work did not gain priority with time. Run on its own for 60 s, the deep-queue stress test failed on 1.1115 in 5 of 5 runs with a queue not served for 15 s. Since 1.1121 a job that has waited 1000 ms is picked before every job that has not; on 1.1132 the same test passed for 60 s and for 120 s, and it has not yet been run on 1.1143, 1.1196, 1.1212, 1.1230, 1.1258, 1.1289 or 1.1307. The test beside a heavy compute client, which failed on 1.1088 and 1.1091 on 3 of 3 seeds, was not repeated on 1.1125, 1.1129, 1.1132, 1.1143, 1.1196, 1.1212, 1.1230, 1.1258, 1.1289 or 1.1307.

The dated host-tool checks are recorded in [`M1-TOOLS-GATE.md`](M1-TOOLS-GATE.md).

Only what's measured on the machine is listed as working. The full per-block status, with labels and dates, is in [`STATUS.md`](STATUS.md).

<!-- front:parity:begin (generated by front-page.py from the block table; do not edit by hand) -->

### Against macOS, block by block (kernel 1.1289, measured 2026-11-22 17:32 UTC)

Each line compares this machine under Omarchy with macOS on the same machine. The macOS reference times of the GPU rows were read with macOS 26B123 running as a guest under a hypervisor on this Mac, not natively, so a GPU figure at or above macOS is not shown to equal or beat macOS until a native reading of the same tests exists, which is still to be done. On another Mac with the same M4 GPU, running macOS natively, light compute and light drawing with 1 command queued took 33 to 36% less time than these references, so the figures above 100% at 1 command queued are not to be relied on. PARITY / BETTER / MATCH: measured at macOS or better. SHORT: measured below macOS. UNTESTED: no like-for-like number yet. "Measured on" is the newest kernel named in the measurement; details and numbers are in [`STATUS.md`](STATUS.md).

**Recorded block scores after promoted kernels** (whole-percent rounded composite scores, higher is better; dates and measurement limitations below apply):

| Kernel | Measured (UTC) | GPU | CPU | RAM | Display |
| --- | --- | --- | --- | --- | --- |
| 1.1289 | 2026-11-22 17:32 | 100 % | - | - | - |
| 1.1258 | 2026-11-22 11:55 | 98 % | - | - | - |
| 1.1230 | 2026-11-22 06:27 | 98 % | - | - | - |
| 1.1212 | 2026-11-22 01:44 | 98 % | - | - | - |
| 1.1196 | 2026-11-21 20:01 | 98 % | - | - | - |
| 1.1143 | 2026-11-21 12:43 | 96 % | - | - | - |
| 1.1132 | 2026-11-20 20:42 | 97 % | - | - | - |
| 1.1129 | 2026-11-20 20:02 | 96 % | - | - | - |
| 1.1125 | 2026-11-20 09:21 | 96 % | - | - | - |
| 1.1121 | 2026-11-20 07:44 | 96 % | - | - | - |
| 1.1115 | 2026-11-19 23:51 | 95 % | - | - | - |
| 1.1105 | 2026-11-19 16:55 | 95 % | - | - | - |
| 1.1104 | 2026-11-19 16:24 | 93 % | - | - | - |
| 1.1097 | 2026-11-19 13:22 | 91 % | - | - | - |
| 1.1091 | 2026-11-19 09:15 | 91 % | - | - | - |
| 1.1088 | 2026-11-19 07:16 | 90 % | - | - | - |
| 1.1078 | 2026-11-19 02:39 | 94 % | - | - | - |
| 1.1072 | 2026-11-18 23:12 | 96 % | - | - | - |
| 1.1070 | 2026-11-18 22:01 | 89 % | - | - | - |
| 1.1067 | 2026-11-18 20:37 | 88 % | - | - | - |
| 1.1065 | 2026-11-18 20:13 | 87 % | - | - | - |
| 1.1062 | 2026-11-18 18:45 | 84 % | - | - | - |
| 1.1058 | 2026-11-18 17:05 | 83 % | - | - | - |
| 1.1056 | 2026-11-18 16:20 | 81 % | - | - | - |
| 1.1052 | 2026-11-18 15:01 | 81 % | - | - | - |
| 1.1038 | 2026-11-18 02:34 | 81 % | - | - | - |
| 1.1036 | 2026-11-18 01:10 | 81 % | - | - | - |
| 1.1032 | 2026-11-17 23:32 | 80 % | - | - | - |
| 1.1030 | 2026-11-17 22:20 | 80 % | 100 % | 84 % | 103 % |
| 1.1024 | 2026-11-17 20:24 | 77 % | - | - | - |
| 1.1022 | 2026-11-17 19:27 | 72 % | - | - | - |
| 1.1017 | 2026-11-17 14:31 | 71 % | 100 % | - | 103 % |
| 1.1010 | 2026-11-17 06:06 | 67 % | 100 % | 83 % | 103 % |
| 1.1009 | 2026-11-17 05:23 | 47 % | 99 % | 83 % | 102 % |
| 1.1007 | 2026-11-17 02:29 | 46 % | 99 % | 98 % | 102 % |
| 1.1006 | 2026-11-17 00:59 | 44 % | 100 % | 97 % | 104 % |
| 1.1005 | 2026-11-17 00:22 | 44 % | 100 % | 97 % | 103 % |
| 1.1003 | 2026-11-16 22:44 | 44 % | 100 % | 98 % | 103 % |
| 1.1002 | 2026-11-16 22:06 | 44 % | 100 % | 97 % | 103 % |
| 1.1001 | 2026-11-16 20:29 | 44 % | 100 % | 98 % | 103 % |
| 1.1000 | 2026-11-16 20:02 | 44 % | 100 % | 98 % | 101 % |
| 1.999 | 2026-11-16 18:18 | 44 % | 100 % | 97 % | 103 % |
| 1.998 | 2026-11-16 16:59 | 44 % | 100 % | 98 % | 103 % |
| 1.997 | 2026-11-16 16:23 | 44 % | 99 % | 98 % | 103 % |

**1.1289 in detail (MEASURED throughput):** From the printed times at depth 64, light compute is 109.3% of macOS's depth-64 figure (19.3 µs), light drawing 106.0% (43.3 µs), heavy compute 98.6% and heavy drawing 99.3%, rounded to one decimal place. The legacy GPU headline of 100% is a rounded composite of five rows: idle power-off at 100%, light compute and light drawing each at the lowest of its three queue depths (1, 8 and 64 commands queued), and heavy compute and heavy drawing; a light row with a depth not yet compared with macOS counts at no more than 99.9% (earlier entries counted light compute at its depth-64 figure). Light compute takes 152.2 µs with 1 command queued (111.6% of macOS's 169.9 µs), 27.6 µs with 8 commands queued (98.6% of macOS's 27.2 µs) and 19.3 µs with 64 commands queued (109.3% of macOS's 21.1 µs); it is below macOS with 8 queued and counts at 98.6%, its lowest. Light drawing takes 162.2 µs with 1 command queued (168.2% of macOS's 272.8 µs), 48.2 µs with 8 commands queued (104.1% of macOS's 50.2 µs) and 43.3 µs with 64 commands queued (106.0% of macOS's 45.9 µs); for the first time it is at or above macOS at all three depths, and it counts at 104.1%, its lowest. Heavy compute and heavy drawing are below 100%. It does not establish GPU correctness. Kernel 1.1289 batches the fragment work of light drawing once per firmware run. Every draw used to write a barrier on the fragment ring ahead of its fragment work, plus its own submit record and doorbell. A hypervisor trace of macOS's render queues on this Mac shows no barrier work in 1693 work buffers, and the firmware runs draws in runs of four, so at 64 queued commands we paid four barriers, eight records and four doorbells per run. 0994 lets a draw that is queued behind at least one run of unfinished fragment work join a batch of at most one run: one barrier, five records and one doorbell per run. 0995 batches only when the draws already waiting fill the run in the same pass, so a shallow queue keeps the per-draw form and 1 queued command never batches. 0996 turns it on by default (`g16_render_fs_run=1`). Measured on development kernel 1.1285 in one boot, with the setting off and then on: light drawing at 64 queued commands took 43.29 µs instead of 48.77 µs (11.2% less); light drawing at 8 queued was 0.63% slower and at 1 queued 0.05% slower; light compute at 1 queued was 1.35% faster and at 64 queued 0.32% slower; no GPU faults. Measured on 1.1289 after the commit, against 1.1258 (one run each): light compute takes 152.2, 27.6 and 19.3 µs at 1, 8 and 64 queued commands instead of 150.2, 27.7 and 19.1 µs; light drawing takes 162.2, 48.2 and 43.3 µs instead of 161.9, 48.7 and 49.4 µs; heavy compute stays at 98.6% and heavy drawing reads 99.3% against 99.0%. Light compute at 8 queued is now the lowest light figure. One defect is still open: the audio coprocessor (AOP) can stall as the machine goes to sleep. That happened in 5 of 516 boots so far, on kernels with and without this change, and each time that boot had no speaker or microphone sound until a reboot. It is under investigation. Heavy drawing is compared on the short measured run only. Under sustained load it is not yet compared with macOS: after about two and a half minutes of back-to-back heavy drawing, throughput on this machine drops by 23–30% (the hottest sensor reads 107–114 °C after a single 10-second run). The cause is not identified and there is no macOS figure for the same sustained load.

CPU, RAM and display figures below are from the full measurement on 1.1030 at 22:25–22:31 UTC on 17 November.

CPU 100% is the mean of single-core throughput at 100%, all-core throughput at 97%, and like-for-like efficiency at 103% (26,358 MHz delivered at 7.105 W, from a cooled start at 38.1 C). All-core throughput remains below macOS.

RAM 84% is the mean of six legs. Bandwidth and latency average 98% without a scheduling hint (100% with the hint); latency is 97.81 ns versus 91.3 ns. The sixth leg is GPU memory at the idle desktop: 2,506 MiB resident 98 seconds after boot, before any benchmark, against a provisional macOS reference of 314 MiB (13%). That footprint is the largest remaining RAM gap. Since 1.1078 the kernel builds a program's render units as it needs them, and the two larger units back 32 MiB of their 256 MiB parameter-buffer windows (`g16_resident_lazy` 2 and `g16_pb_lazy` 1 by default); the six legs above are still those of the 1.1030 measurement, and STATUS.md gives the idle reading taken on 1.1078.

Display 103% uses the existing rail and full-frame white/black power comparison, with no new luminance claim. These measurements used an unlocked session with the display on.

| # | Block | State vs macOS | Measured on | Open |
| --- | --- | --- | --- | --- |
| 1 | CPU idle on AC | SHORT (close) | 1.914 | - |
| 2 | CPU idle wakeups | IPI PARITY / timer SHORT | 1.914 | - |
| 3 | CPU all-core load + fan | SHORT all-core 97%; single 100%; efficiency 103% | 1.1030 | Same instruction stream. The CPU headline is an aggregate, not closure of all-core throughput. |
| 4 | CPU benchmark speed | PARITY 1-core 99 % / all-core 98 % | 1.916 | - |
| 5 | Energy mode (power-saver) | PARITY | - | - |
| 6 | GPU compute speed | SHORT | 1.1289 | Light compute takes 152.2 µs with 1 command queued (111.6% of macOS's 169.9 µs), 27.6 µs with 8 commands queued (98.6% of macOS's 27.2 µs) and 19.3 µs with 64 commands queued (109.3% of macOS's 21.1 µs); it is below macOS with 8 queued and counts at 98.6%, its lowest. Heavy: 98.6% of macOS's 539.2 µs. Both verifying stress tests (`cr.concurrent` and `queue.depth`) passed on 1.1132 with Mesa 1.8149; they have not yet been run on 1.1289 or 1.1307. |
| 7 | GPU render speed | SHORT | 1.1289 | Light drawing takes 162.2 µs with 1 command queued (168.2% of macOS's 272.8 µs), 48.2 µs with 8 commands queued (104.1% of macOS's 50.2 µs) and 43.3 µs with 64 commands queued (106.0% of macOS's 45.9 µs); for the first time it is at or above macOS at all three depths, and it counts at 104.1%, its lowest. Heavy: 99.3% of macOS's 249.1 µs (below parity). Both verifying stress tests (`cr.concurrent` and `queue.depth`) passed on 1.1132 with Mesa 1.8149; they have not yet been run on 1.1289 or 1.1307. 2160p geometry (still well short of macOS) and sustained-load behaviour remain. |
| 8 | GPU idle power-off | PARITY | 1.1289 | GPU powered off and clock-gated for 100% of the idle sample. This alone is not a battery-life result. |
| 9 | RAM latency + bandwidth | SHORT latency 93%; bandwidth near parity; idle GPU memory 13% | 1.1030 | Bandwidth/latency aggregate: 98% unhinted, 100% hinted. Latency: 97.81 ns. GPU memory at the idle desktop, read 98 s after boot before any benchmark: 2506 MiB versus a provisional macOS reference of 314 MiB. With that leg the RAM headline is 84%. Latest idle reading, on 1.1307 after a fresh boot (median of two): 316.0 MiB, 99.5% of macOS's 314.4 MiB; the RAM headline is not recomputed from it. |
| 10 | Display: backlight drive + local dimming | PARITY | - | - |
| 11 | Display: idle refresh | PARITY | - | - |
| 12 | Display power (SoC rail) | SHORT | - | - |
| 13 | Display calibration | PARITY | 1.937 | - |
| 14 | True Tone | PARITY | 1.937 | Runs again on 1.934; on 1.932 a True Tone write while the display was powered down left the panel dark, addressed in 1.934's display driver |
| 15 | Auto-brightness | PARITY | 1.937 | - |
| 16 | XDR / HDR | UNTESTED | - | - |
| 17 | ProMotion video cadence | MATCH | 1.923 | - |
| 18 | Idle desktop power (whole machine) | PARITY at level 140 (whole machine) / SoC part SHORT ~+0.2 W | 1.939 | - |
| 19 | Sleep: s2idle | UNTESTED | - | the default sleep up to 1.930; 980 mW asleep against 179 mW for deep sleep |
| 20 | Sleep: deep | PARITY on wake (7 of 7 deep sleeps on 1.929) / UNTESTED: lid, key and power-button wakes | 1.934 | the lid-close default since 1.932; the 1.932 dark panel after a screen blank addressed in 1.934 (measured in its install check); lid, key and power-button wakes not tested (on the hands list) |
| 21 | Battery + charge limit | PARITY | - | - |
| 22 | Charging: MagSafe + USB-C PD | PARITY | - | - |
| 23 | Ports: idle power | PARITY | 1.926 | - |
| 24 | USB-C data speed | SHORT (from the code) | - | - |
| 25 | USB4 / Thunderbolt | UNTESTED (a fix is ready) | - | - |
| 26 | External display (HDMI / DP alt) | UNTESTED | 1.914 | - |
| 27 | Sensor: ALS | PARITY (cold boot); after a deep wake to verify | 1.929 | - |
| 28 | Sensor: accel + gyro | PARITY | 1.929 | - |
| 29 | Sensor: lid angle | PARITY | - | - |
| 30 | Sensor: SMC keys / hwmon | PARITY (keys published) | 1.921 | - |
| 31 | Camera: stream, first open, idle, apps | PARITY (stream) / UNTESTED (power: macOS number missing) | 1.939 | - |
| 32 | Camera: image settings + field of view | SHORT | 1.939 | - |
| 33 | Camera: ML noise reduction / semantic | SHORT | - | - |
| 34 | Neural engine | PARITY | 1.926 | - |
| 35 | Video decode HEVC10 4K | SHORT | 1.985 | - |
| 36 | Video decode H.264 / AV1 | PARITY (H.264) / SHORT (AV1) | 1.985 | - |
| 37 | Video encode H.264 / HEVC | Since 2026-11-23 22:25 UTC (libva-v4l2_request-avd 2.628) hardware H.264 through VA-API also encodes into Matroska: 300 of 300 frames, decoding to the same frames as the MP4 output; HEVC into Matroska still crashes. Before that: since 2026-11-16 03:45 UTC (kernel 1.946) the VA-API driver is back on its earlier version (libva-v4l2_request-avd 1.611, the 1.608 content). Hardware H.264 from CPU frames into MP4 and raw streams writes all 10 test frames and they decode back (checked on the installed driver at its first start); the MP4 header comes from the encoder's own first packet again, so it matches the stream, and the stock screen recorder's MP4 is correct again. Matroska output from the VA-API encoder crashes again and writes 0 bytes, until the proper fix lands: the encoder writing the sequence parameters the app puts in the file header. 1.610 (on 1.945) avoided that crash, but its header did not match the stream, and players that read only the header got 9 of 30 frames (H.264 Matroska), 0 of 30 (HEVC Matroska) and 9 of 30 (MP4) (measured). The kernel encoder: v4l2-ctl encodes a frame in 16.28 ms, as on macOS, and FFmpeg's h264_v4l2m2m reaches 55 fps (on 1.922) | 1.985 | - |
| 38 | JPEG engine | BETTER | - | - |
| 39 | ProRes | BETTER (4:2:2 encode, measured on 1.930) | - | bound by default since 1.932; no stock application uses it yet |
| 40 | Scaler | N/A (Omarchy) | - | - |
| 41 | Media power + requests at idle | PARITY | - | - |
| 42 | Touch ID | UNTESTED | - | not yet tested by hand |
| 43 | Speakers | PARITY | 1.929 | - |
| 44 | Microphones | PARITY (geometry + steering + tone to macOS's curve, from the code) / level+AGC to verify | - | - |
| 45 | Headphone jack | SHORT (plug test) | 1.913 | - |
| 46 | Wi-Fi: country / regulatory | PARITY | - | - |
| 47 | Wi-Fi: throughput | UNTESTED | 1.920 | - |
| 48 | Bluetooth | UNTESTED | 1.923 | - |
| 49 | Storage: NVMe | SHORT? (write ~91 %), read UNTESTED | 1.911 | - |
| 50 | Storage: SD reader | PARITY (from the code) | 1.926 | - |
| 51 | Keyboard + backlight | PARITY | - | - |
| 52 | Trackpad + haptics | UNTESTED | 1.929 | - |
| 53 | Sleep: asleep draw (deep sleep vs macOS sleep, 10 min) | UNTESTED (ours measured, macOS not measured yet) | 1.945 | ours 166 mW in deep sleep (160-171 mW over 600 s asleep, read from the battery charge counter); the macOS number is not measured yet |
| 54 | Camera: open-to-first-frame + frame pacing | SHORT (cold first open) / BETTER (re-open, fw_keep_ms default-on in 1.940) / PARITY (30 fps pacing) | 1.940 | - |
| 55 | Camera: formats + stills | PARITY (video) / BETTER (1080p60, default-on in 1.940) / SHORT (Desk View) | 1.940 | - |
| 56 | Wake sources (keyboard / trackpad, lid, power button) | UNTESTED (armed) | 1.929 | armed in the kernel; only the RTC alarm wake is measured on deep sleep; lid, key and power-button wakes not tested (on the hands list) |
| 57 | Input + sensors after a deep wake | PARITY (firmware side) / UNTESTED (typing, Touch ID) | 1.929 | - |
| 58 | Wi-Fi idle power | SHORT (the Wi-Fi chip draws 2-4 times what macOS draws idle) | 1.947 | idle desktop with power save off (the stock setting): 477 mW on the Wi-Fi rail against 118 mW on macOS; power save on with a 20 ms sleep return read 114 mW, with average ping 12.4 ms instead of 6.6 (measured on 1.945). That setting is on by default since 1.947; its rail, throughput and wake from idle on 1.947 are being measured |

<!-- front:parity:end -->

| Area | Status (older summary from 2026-11-10; the Power row brought to 1.934; the table above is current) |
| --- | --- |
| Boot chain (m1n1, U-Boot, Limine, disk unlock), storage, keyboard and trackpad, USB, battery, lid | Working |
| CPU | Working: 10 cores, cpufreq and cpuidle (performance cores up to 4.46 GHz requested, efficiency 2.89 GHz); one busy performance core runs at about 4.36 GHz and two to four at about 3.9 GHz, as under macOS with all cores loaded; since 1.723 the reported frequency is the reached one |
| GPU | Accelerated (OpenGL 4.6 / Vulkan 1.4); idle GPU power 0.041 W on 1.710, 0.076 W on 1.711 |
| Internal display | 3024x1964 at 120 Hz, scale 2, 10-bit (XRGB2101010) and Display P3 as package defaults, with no user setting. The panel link is most likely 8 bits per channel with dithering (not yet verified). Adaptive sync (VRR) on since `omarchy-mac` 2.18 |
| Glassy bar | Stock holds about 59 fps on this 120 Hz panel (re-measured on 1.731). 119 fps needs the change in [omacom/omarchy#18176](https://github.com/omacom/omarchy/pull/18176), which is not carried here. With blur on, the desktop does not composite 120 times a second (open: 80 per second with windowed 60 fps video on 1.856 with the carried Hyprland and bar pair, see `STATUS.md`) |
| Smoothness at 120 Hz | Both fixes on by default since 1.718: a full-window animation runs at 120 fps (about 90 before), scrolling at 120 fps, and 24, 30 and 60 fps video in Chromium stays on its cadence in our tests. YouTube fullscreen 1080p60 since 1.730: 1.8-3.4 % of frames off cadence, 0 dropped (1.6-7.2 % before, 23 % before pacing). 4K60 in Chromium is held back by its software decoding; stock mpv plays the same 4K60 clip with 0 of 600 frames off (see `STATUS.md`). After idle, a terminal open/close animation sometimes ran below about 68 fps (cause: the frame pacing's start value after idle); 1.804 carries the fix, and its first 8-burst check held no frame |
| Camera | Working, tuned: 1080p30 and 720p30 capture only today (the sensor has more modes; work in progress) with the sensor tuning file loaded, seen by stock apps and Chromium; colour close to neutral in a bright scene, not yet judged by eye |
| Wi-Fi and Bluetooth | Working |
| Audio | Speakers (with speaker protection), headphone jack and the three-microphone array working |
| Media | Hardware decode (H.264, HEVC, VP9) working in stock mpv through VA-API, with kernel 1.704 and the VA-API driver alias (checked at 1080p, and 4K for H.264; 10-bit HEVC decodes through VA-API as real 10-bit in short checks; AV1 bit-exact through VA-API in ffmpeg, 10-bit up to 7680x4320 since `libva-v4l2_request-avd` 1.606); Chromium decodes H.264 and VP9 profile 0 on the hardware decoder as shipped with `omarchy-mac` 2.14, while AV1, 10-bit VP9 and HEVC play in software in Chromium because its V4L2 path cannot lay out 10-bit P010 frames; since our `chromium` 1.605 (four patches, installed 2026-11-11) hardware-decoded H.264 and VP9 show their BT.709 colours in Chromium (51.0-51.6 dB from software decoding, measured on the screen); a complete fix is in work (stock omarchy-mac turns Chromium's hardware decode off on Apple Silicon; video encode in Chromium is software only); JPEG hardware encode and decode working (decode since 1.728); H.264 and HEVC hardware encode working through stock FFmpeg since 1.722, and the stock screen recorder records in hardware since 1.726 (10-bit HEVC works through V4L2 directly; stock FFmpeg cannot send 10-bit frames to V4L2 encoders) |
| Touch ID | Working for sudo, polkit and the lock screen |
| Keyboard backlight | Working with the local `omarchy-dev` / `omarchy-settings-dev` builds |
| Neural Engine | Working: on by default (`/dev/accel/accel0`), runs programs bit-exact and powers off when idle (measured on 1.711) |
| Power | Deep sleep: the lid-close default since 1.932 (2026-11-15). The dark panel after a screen blank seen on 1.932 is addressed in 1.934's display driver and was measured in its install check; lid, key and power-button wakes are not yet tested by hand (see the table above and the top of `STATUS.md`). Before that, the first full sleep, wake and resume on the J604 was measured on 1.922, followed 7 minutes later by a cold reset at the start of a second test, left by the wake. The older deep-sleep lines in this row predate it. Suspend (s2idle) working again since 1.710 (it was refused on 1.705-1.708). Idle on AC is about 10.5 W with the stock edge `performance` power profile (see `STATUS.md`); since 1.802 the idle performance cores drop to their slowest clock (rail 994 mW against 2360 mW), and since 1.809 the drop waits for a clock change in flight (rail 383 mW; SMC system total 9.82 W to 9.07 W in an A/B; the wall figure is not measured yet). Deep sleep in progress: the reset during a test sleep is fixed since 1.736; changes for the two GPU items that block real suspend-to-RAM are in 1.803 (off by default); the first real suspend-to-RAM trial (1.804) and a test told to stop before the power-down (1.807) both hung and needed the power button; staged tests on 1.810 and 1.811 found the stop in resume, and a guard for it is in 1.811 (off by default), but no real power-down has been completed, so deep sleep is still not working; since 1.813 `deep` is not offered on this Mac, the T8132 sleep sequence ported from Aurora is carried since 1.821, switched off by default; the first real sleep with it (1.829) did not come back and the machine reset itself; on 1.833 the kernel was measured powering part of the SSD controller's PCIe block off under the running SSD controller, and staged runs of the sequence that stop before the sleep did not come back on 1.836 (switched off by hand) or on 1.841 (the machine reset itself); deep sleep is not working, and suspend-to-idle is the only sleep state |
| In progress | Deep sleep in real use (lid, keys, power button), camera colour, Thunderbolt/USB4, external displays and HDMI |

## Install

The J604 runs the official Omarchy for Apple Silicon install on the edge channel. The stock packages can't boot this Mac yet (see `STATUS.md`), so the J604 builds of the packages below replace the official ones through their local `pkgrel`. Any official update with a higher version replaces them again.

There are no prebuilt packages in this repository. To rebuild a component, either apply its patches to the source commit on line 1 of its `BASE` file, or build its recipe:

1. Take the official recipe named on line 3 of `BASE`.
2. Copy this directory's `recipe/` files over it.
3. Run `makepkg`.

Until these changes land in the official packages, these builds target the J604 only.

## Versions

The kernel and graphics entries below reflect the promoted versions as of 2026-11-18. Other entries retain their earlier dated install records; see [`STATUS.md`](STATUS.md) for their scope. Development-slot experiments are separate from the source versions carried here:

| Package | Official base | Local pkgrel |
| --- | --- | --- |
| `linux-aurora-next`, `linux-aurora-next-headers` (promoted default; the separate `linux-aurora` development slot held 1.1289 at the commit and takes the development kernels after it) | 7.3.0.aurora1, aurora-silicon/linux `integration/aurora` (`da8e1acf3656`, Aurora's tip), 999 patches | `1.1307` |
| `aura` | 1.0.0b13, aurora-silicon/aura (`f0f180afe5`), aurora-silicon/alarm-aurora recipe, 25 patches | `1.619` (1.616's payload under a newer number; replaced `m1n1-aurora` on 2026-11-12) |
| `uboot-aurora` | 2026.07.aurora11, aurora-silicon/u-boot (`250385f8c8`, the version Aurora packages), 1 patch | `1.604` |
| `mesa-aurora`, `vulkan-aurora` | 1:26.3.0_devel.20261122, omacom/mesa (`e705eca833f`) | `1.8303` (since 2026-11-24 14:01 UTC: the 1.8404 series on the base the official recipe now pins; `0003` is dropped because the base has it, and `0007` keeps its per-draw blend terms valid across the base's trimmed root upload and its fast flush path; the idle desktop holds 294.35 MiB of GPU memory after a fresh boot against macOS's 314.4 MiB; 1.8404, since 2026-11-24 00:46 UTC: 1.8192 plus one: `glFlush` validates the framebuffer and the sampler views before it flushes, so a render target the compositor has deleted is freed at the flush instead of at its next frame; with it the idle desktop holds 293.3 MiB of GPU memory after a fresh boot against macOS's 314.4 MiB; 1.8192, since 2026-11-23: 1.8187 plus one: EGL on Wayland frees the spare colour buffers of a window surface that has not drawn for 3 seconds, so an idle shell no longer keeps full-size buffers it has stopped using; 1.8187, since 2026-11-22: 1.8149 plus one: at a flush the GPU driver drops deleted textures and retires finished batches, so memory the compositor has let go of is freed at the idle desktop; ships only together with hyprland 16.607) |
| `alsa-ucm-conf-aurora` | 10.aurora3, aurora-silicon/alsa-ucm-conf-asahi (`b9b2d0c82c`), omacom recipe (`9679db3370`) | `2.604` (since 2026-11-22: the official recipe at pkgrel 2, which adds the jack preamp capture volume patch, with the J604 jack and speaker patches carried; 1.605 before) |
| `aurora-audio` | 4.1.aurora3, aurora-silicon/asahi-audio (`06aec67a8a`), omacom recipe (`9679db3370`) | `6.604` (since 2026-11-22: the official recipe at pkgrel 6, which adds the J616 speaker and microphone files, with the J604 patches 0001-0011 carried unchanged; 3.612 before) |
| `speakersafetyd` | 2.0.1, omacom recipe (`9679db3370`) | `2.604` (since 2026-11-22: the official recipe at pkgrel 2 with the patches of 1.607 carried; 1.607 was built on the aurora-silicon/alarm-aurora recipe `4c7a46fede`) |
| `triforce-lv2` | 0.3.2, chadmed/triforce, omacom recipe (`9679db3370`) | `2.604` (since 2026-11-22: the official recipe at pkgrel 2 plus one fix: the steering vector phase, so the microphone array's geometry takes effect) |
| `asahi-fwextract` | 0.9.2, asahi-alarm/asahi-alarm-installer | `1.609` |
| `asahi-scripts` | 20261023, asahi-alarm/asahi-scripts, aurora-silicon/alarm-aurora recipe | `9.605` (the Stage 2 trial's guard timer counts from unlock) |
| `plymouth` | 26.134.222, Plymouth (Arch Linux recipe) | `3.605` (since 1.1129: the device manager stops watching udev events before it frees its monitor, so a quit from the splash can no longer crash `plymouthd` at boot) |
| `libfprint-git` | 1:1.94.100.r10.g6f9479c, libfprint (`6f9479c3d5`), omacom recipe | none since 2026-11-24: the official pkgrel 6 is installed and accepts Secure Enclave interface v4 and v5 itself (`5.604` was the local build until then) |
| `libva-v4l2_request-avd` | 1.3.r11.r169.gc653eaa, omacom/libva-v4l2_request (`c653eaa623`), omacom recipe | `2.629` (since 2026-11-23 23:14 UTC: 2.628 plus one: on this decoder each AV1 frame is submitted when the client ends the picture instead of being held until the next frame's parameters arrive (`V4L2R_AV1_HOLD=1` restores the hold); 2.628, since 2026-11-23 22:25 UTC: 2.623 plus five: waiting for a decoded frame no longer holds the driver's locks (`V4L2R_SYNC_UNLOCKED=0` restores the 2.623 waits), and the H.264 encoder takes packed sequence headers, so `h264_vaapi` encodes into Matroska; 2.623, since 2026-11-23: 2.622 plus one: a busy exported decode buffer is rebound too when the client exports the surface again after every decode, as FFmpeg hwmap and mpv's dma-buf path do; `V4L2R_REBIND_EXPORTED=0` keeps exported buffers pinned; 2.622, since 2026-11-22: 1.621 on the official recipe at pkgrel 2, which ships the `aurora_drv_video.so` alias itself, so the local alias is dropped; no driver change; 1.621, since 2026-11-22: 1.619 plus four patches: when every spare buffer is busy the driver waits for the spare that frees first instead of the surface's own buffer, so the decoder queue does not drain, and it never rebinds a surface while a derived image maps its buffer, with tests for both; AV1 4K decoding measured at 100.6 % (8-bit) and 100.2 % (10-bit) of macOS, from 96.7 % and 96.9 % with 1.619; 1.619, since 1.1212: 1.616 plus four patches, so a surface handed back while its decode buffer is still busy is decoded into an idle spare buffer, up to 4, instead of waiting for it; 1.616, since 1.1129: 1.611 plus one fix, the encoder refuses a coded-buffer size its field cannot hold, keeps its input surface until the picture ends and refuses another context's coded buffer; 1.611 is the 1.608 content again since 1.946: adds the `aurora_drv_video.so` alias, VA-API H.264/HEVC encoding on the Apple video encoder, AV1 10-bit decoding up to 7680x4320, the VP9 maximum frame size from the kernel and up to 16 bitstream buffers in flight; 1.610's packed sequence headers are out again because their file header did not match the stream, so VA-API encoding into HEVC Matroska still crashes, see STATUS.md; the alias is open as [omacom/omarchy-pkgs#2375](https://github.com/omacom/omarchy-pkgs/pull/2375), and its official `-2` will replace this build) |
| `chromium` | 158.0.8127.36, Arch Linux ARM recipe (`24b78507f0`) | `2.609` (since 2026-11-24: the nine patches of 1.608 on Chromium 158, with `0002` and `0007` regenerated for it; patches for hardware video on the Apple decoder: 10-bit P010 frames, BT.709 colours, decoder errors reported, HEVC only with Annex-B, frame errors, HDR metadata, capture buffers sized for the renderer; with `omarchy-mac` 2.19 Chromium decodes 10-bit VP9, AV1 and HEVC on the decoder; since 1.608 also the V4L2 video encoder, used only with Chromium's AcceleratedVideoEncoder feature, off by default) |
| `avd-fw` | 0.1, AsahiLinux/avd-fw (`v0.1`), omacom recipe (`c1fcff4dec`), 15 patches | `1.604` (aurora-silicon/avd-fw's queue transport and the T8132 SRAM aperture) |
| `gpu-screen-recorder` | 6.1.7, gpu-screen-recorder (Arch Linux recipe, built for aarch64) | `1.604` (captures monitors on the Apple display controller) |
| `omarchy-mac-boot` | 20261104, omacom/omarchy-mac-pkgs (`7e50e937b5`) | `1.616` |
| `omarchy-mac` | 0.2.7, omacom/omarchy-mac-pkgs (`35dc7edf69`), omacom recipe (`b3d2146ff4`) | `2.37` (since 2026-11-24: the official 0.2.7-2 recipe, which names `mesa-aurora` and `vulkan-aurora` as the GPU drivers of a Mac, with the same 24 J604 patches; 1.36, since 2026-11-23: the official 0.2.7-1 recipe with the 24 J604 patches of 2.35 rebased, 0001-0023 with two 0002s, so the recipes after 2.19 are here now: the Apple camera told the mains frequency of the timezone, the J604 Chromium hardware encoder, AV1 decoding in mpv on the Apple video decoder, True Tone (the built-in display's white follows the room light in macOS's two-second ramp, is sent to the panel beside the night light, and is not written while the panel is off), the display brightness and the keyboard backlight follow the room light on macOS's keyboard scale under the stock helper name, Chromium's flags re-applied when `omarchy-mac` itself updates, the CPU floor for Apple Silicon power profiles started after the profiles daemon, and Wi-Fi power save on for the `brcmfmac` driver only; 2.19 adds the panel line with adaptive sync, Chromium hardware video decode on the J604 (every codec with the J604 Chromium), and a Touch ID start-up race fix) |
| `omarchy-dev` | 4.0.0.r8269.g94a8c3c (edge), omacom/omarchy `quattro` (`94a8c3c332`), omacom recipe (`b3d2146ff4`) | `1.9` (since 2026-11-24: the official r8269 recipe with the same six patches rebased; 1.7, since 2026-11-23: the official r8160 recipe with `kbd-0101`..`0104`, `bar-0301` and `shell-0501` rebased; `pp-0401` is dropped because omacom/omarchy now carries the `omarchy-powerprofiles-floor` command itself; 1.61, since 2026-11-22: 1.4 plus `pp-0401` and `shell-0501`: the shell's windows ask for no depth or stencil buffer; 1.4: keyboard-backlight patches + the bar declares its strip as its blur region; look unchanged) |
| `omarchy-settings-dev` | 4.0.0.r8269.g94a8c3c (edge), omacom/omarchy `quattro` (`94a8c3c332`), omacom recipe (`b3d2146ff4`) | `1.6` (since 2026-11-24: the official r8269 recipe with the same seven patches rebased; 1.4, since 2026-11-23: the official r8160 recipe with the same patches rebased; 1.3, since 2026-11-22: 1.2 plus `sched-0301` = omacom/omarchy#18228: `kyber` on every NVMe namespace, in 5 of 5 boots in a row; 1.2: keyboard-backlight patches + built-in panel defaults, adaptive sync from the panel line) |
| `hyprland` | 0.57.2, hyprwm/Hyprland (`v0.57.2`), omacom recipe (`573dd5c7fb`) | `21.607` (since 2026-11-22: 16.607 plus one: on GLES 3.2 the blur stencil is a stencil-only texture, 1 byte per pixel instead of a depth-stencil texture, so Hyprland's GPU memory at the idle desktop after a fresh boot reads 126.0 MiB instead of 149.2 MiB; 16.607, since 2026-11-22, with Mesa 1.8187: 12.607 plus one: the compositor flushes once after its work buffer pool frees buffers, so the GPU driver releases them at the idle desktop instead of at the next frame; 12.607, since 2026-11-22, after kernel 1.1258: 4.607 plus one: the group bar gradients are drawn from textures one pixel wide instead of four textures the size of the monitor, with the same pixels, so the compositor holds about 93 MiB less GPU memory at the idle desktop on a fresh boot (MEASURED on kernel 1.1258: 204.7 MiB against 297.6 MiB with 4.607; group bar and lock screen pixels unchanged); 4.607, since 2026-11-22: 2.607 plus one: `render:use_fp16` auto picks FP16 work buffers only for HDR output, so SDR 10-bit output keeps a 4-byte-per-pixel work buffer instead of 8, about 69 MiB less GPU memory on the J604 panel; 2.607: Omarchy's three patches + one: blur only inside a surface's declared blur region, so the full-screen bar no longer makes Hyprland blur everything that changes under it; look unchanged) |

Also installed, unchanged from the official repositories: the rest of the edge userland; the Neural Engine stack (`omarchy-ane`, `omarchy-ane-dkms`, `mil-hwx-compiler`, `omarchy-mac-ml`), which we no longer rebuild. `j604-keyring` is a local keyring package that only signs the builds above; it has no source changes to publish.

Since 2026-11-12 the Mac boots through Aurora's aura boot loader: the Stage 2 on the ESP (`m1n1/boot-ctxr.bin`) holds aura, the J604 device tree and U-Boot, and `asahi-scripts`' `update-m1n1` writes it as a trial and commits it after a good boot. Kernel 1.902 is on Aurora's tip (`da8e1acf3656`); `STATUS.md` lists what 1.703 to 1.902 changed over 1.617.

## Layout

| Directory | What | Upstream it applies to | License |
| --- | --- | --- | --- |
| `kernel/` | `git format-patch` series (apply in `series` order); `BASE` names the commit it applies to and the official recipe | aurora-silicon/linux `integration/aurora` (built with the omacom `linux-aurora` recipe) | GPL-2.0 (see `kernel/COPYING`) |
| `aura/` | J604 patches and the `aura` recipe (Aurora's boot loader; it replaced `m1n1-aurora` on 2026-11-12, whose last published `m1n1/` is in commit 6c7bda4) | aurora-silicon/aura (aurora-silicon/alarm-aurora recipe) | MIT and GPL-2.0-only (see `aura/LICENSE`) |
| `uboot-aurora/` | J604 board patch and the `uboot-aurora` recipe | aurora-silicon/u-boot | GPL-2.0+ |
| `mesa/` | the P010 conversion fix, opt-in G16G sparse binding, aurora-silicon/mesa#218 (GBM NV12 and P010 buffers, author kept) the idle-surface buffer release in EGL on Wayland (`0012`) and the `mesa-aurora` recipe (builds `mesa-aurora` and `vulkan-aurora`) | aurora-silicon/mesa | MIT (main code) |
| `alsa-ucm-conf-aurora/` | J604 headphone jack, microphone and speaker UCM | aurora-silicon/alsa-ucm-conf-asahi | BSD-3-Clause |
| `aurora-audio/` | J604 speaker graph and microphone, plus upstream audio fixes | aurora-silicon/asahi-audio | MIT |
| `speakersafetyd/` | J604 speaker protection profile | AsahiLinux/speakersafetyd | MIT |
| `triforce-lv2/` | the steering vector phase fix (`0001`) on the official recipe | chadmed/triforce (omacom recipe) | GPL-2.0-or-later |
| `asahi-fwextract/` | camera tuning firmware extraction for the J604; keeps the BCM4388 rev C2 Wi-Fi firmware | asahi-alarm/asahi-alarm-installer | MIT |
| `libfprint-git/` | not installed since 2026-11-24 (the official package carries it); until then, Touch ID on the 7.3 kernel: the Apple Secure Enclave driver accepts biometric interface v4 and v5 (two upstream Aurora patches as one) | libfprint (omacom recipe) | LGPL-2.1-or-later |
| `libva-v4l2_request-avd/` | VA-API on the 7.3 kernel: the recipe adds the `aurora_drv_video.so` alias (upstream aurora-silicon/alarm-aurora aedcf240ca91). `patches/` held that recipe diff until 2.622 (the official pkgrel 2 ships the alias itself) and holds, since 1.605, a driver patch that adds VA-API H.264 and HEVC encoding on V4L2 stateful encoders such as the Apple video encoder, since 1.606 a second one that sizes surfaces exported before their first decode for 10-bit AV1 and VP9, since 1.607 the VP9 maximum frame size, and since 1.608 up to 16 bitstream buffers in flight (1.610's packed sequence headers were taken out again in 1.611), and since 1.616 a fix that bounds the encoder's coded-buffer sizes and holds its input surface (patch 0005, also in omacom/libva-v4l2_request#115), and since 1.619 four patches that decode into an idle spare buffer when a surface's own buffer is still busy (0006-0009; `V4L2R_REBIND=0` restores the wait), and since 1.621 four more: wait for the spare that frees first when every spare is busy (`V4L2R_REBIND_FIRST=0` restores 1.619), never rebind a surface while a derived image maps its buffer, and tests for both (0010-0013) | omacom/omarchy-pkgs recipe, omacom/libva-v4l2_request | GPL-3.0-or-later |
| `gpu-screen-recorder/` | the stock recorder captures monitors on the Apple display controller (a KMS-only device with no render node, handled like DisplayLink's) | gpu-screen-recorder (Arch Linux recipe) | GPL-3.0-only |
| `asahi-scripts/` | Aurora's recipe (its `update-m1n1` writes aura's Stage 2 as a trial and commits it after a good boot) + our two patches that add the `apple_nvmem_spmi` and `dwc3-apple` modules to the initramfs | asahi-alarm/asahi-scripts (aurora-silicon/alarm-aurora recipe) | MIT |
| `avd-fw/` | the open Apple Video Decoder firmware with aurora-silicon/avd-fw's queue transport and the T8132 SRAM aperture | AsahiLinux/avd-fw (omacom recipe) | MIT |
| `plymouth/` | SimpleDRM recognized by driver name, so the disk unlock prompt opens on the panel; since 3.605 the device manager stops watching udev events before it frees its monitor (a crash of `plymouthd` at boot) | Plymouth | GPL-2.0 |
| `omarchy-mac-boot/` | boot package fixes for the J604, and `update-m1n1` builds `boot.bin` with only the running Mac's own device tree | omacom/omarchy-mac-pkgs | MIT |
| `omarchy-mac/` | keyboard backlight idle timeout, Apple F-row key names, Touch ID on the J604, the 14-inch MacBook Pro panel line (10-bit, Display P3), Chromium and mpv hardware video, True Tone, room-light brightness, the CPU floor, Broadcom Wi-Fi power save | omacom/omarchy-mac-pkgs | MIT |
| `omarchy-dev/`, `omarchy-settings-dev/` | the keyboard-backlight series on the edge recipes (the same four patches in both); `omarchy-settings-dev` also carries the built-in panel defaults (`disp-0201`), and since 1.3 the NVMe scheduler retry (`sched-0301`); `omarchy-dev` since 1.4 the bar's blur region (`bar-0301`), since 1.61 the shell's buffers (`shell-0501`); the floor command (`pp-0401`, 1.61 only) is upstream since 1.7 | omacom/omarchy | MIT |
| `hyprland/` | blur only inside a surface's declared blur region (`0001`), FP16 work buffers in `render:use_fp16` auto only for HDR output (`0002`), group bar gradient textures one pixel wide (`0004`), one flush after the work buffer pool frees buffers (`0005`) and a stencil-only blur stencil on GLES 3.2 (`0006`), on the official recipe with Omarchy's own three patches | hyprwm/Hyprland (omacom `hyprland` recipe) | BSD-3-Clause |
| `not-installed/` | earlier userland changes that are not installed, and where they are | | |

Every component directory has the same shape:

- `BASE`: line 1 is the upstream source and the commit or tag the patches apply to; line 2 is the package version and local pkgrel; line 3 is the official recipe and commit the J604 recipe changes.
- `patches/`: the J604 changes.
- `recipe/`: the J604 `PKGBUILD` and the same patches. Every other file of the recipe is unchanged from the official recipe named in `BASE`.
- The upstream license text.

`kernel/` has `BASE`, `series`, `patches/`, the kernel's license files and, since 1.862, `recipe/` (the J604 `PKGBUILD` and kernel config; put the files of `patches/` next to it to build).

The kernel series still carries one test-only setting: the J604 device-tree file that patch 0157 adds (the 1.862 file 0122) (`t8132-j604-lab.dtsi`) enables the CPU performance-counter (PMU) node for a counter measurement, as the test-only patch 0616 did in 1.617. It is published because the installed kernel carries it, and it is not proposed upstream as it is; the node is enabled for good only after that measurement passes.

Each patch keeps its original author. Patch 0157 ("j604: integrate the J604 series onto Aurora 7.3", the 1.862 file 0122) folds the part of the 1.617 series that Aurora's 7.3 base does not already contain into one commit; the authors of that work are those of the 1.617 series (see the history of `kernel/`). Apart from the pull requests and issues listed in `UPSTREAM.md`, these patches have not been submitted upstream yet.

No Apple firmware, binaries or device dumps are in this repository. The firmware the machine needs is extracted on the machine itself at install time, as on every Omarchy Mac.

## Upstreaming

What goes to which upstream, in what order, and what's already open is in [`UPSTREAM.md`](UPSTREAM.md).<!-- front:prs:begin (generated by front-page.py from the PR ledger; do not edit by hand) -->

Open pull requests from this work (48, kernel 1.1289, measured 2026-11-22 17:32 UTC):

- [omacom/libva-v4l2_request#124](https://github.com/omacom/libva-v4l2_request/pull/124) (draft): tests: do not read the interrupt fixture's pid file half written
- [omacom/libva-v4l2_request#123](https://github.com/omacom/libva-v4l2_request/pull/123) (draft): tests: let resource-campaign accept heap growth the allocator accounts for
- [omacom/libva-v4l2_request#122](https://github.com/omacom/libva-v4l2_request/pull/122) (draft): ci: lower vm.mmap_rnd_bits for the 20.04 sanitizer job
- [omacom/libva-v4l2_request#121](https://github.com/omacom/libva-v4l2_request/pull/121) (draft): ci: strip the project prefix from meson test --list
- [omacom/libva-v4l2_request#120](https://github.com/omacom/libva-v4l2_request/pull/120) (draft): ci: give cppcheck libva's va_version.h
- [omacom/libva-v4l2_request#118](https://github.com/omacom/libva-v4l2_request/pull/118) (draft): context: when every rebind spare is busy, wait for the first to free
- [aurora-silicon/aura#447](https://github.com/aurora-silicon/aura/pull/447) (draft): sptm-virt: keep the NV2 and world per-CPU state in the heap
- [omacom/libva-v4l2_request#117](https://github.com/omacom/libva-v4l2_request/pull/117): context: decode into an idle CAPTURE buffer instead of waiting on a busy surface
- [aurora-silicon/linux#2696](https://github.com/aurora-silicon/linux/pull/2696) (draft): media: apple: avd: Program the T8132 AXI2AF bridge at power-up
- [omacom/libva-v4l2_request#116](https://github.com/omacom/libva-v4l2_request/pull/116): ci: pin the x86_64 jobs to ubuntu-24.04
- [aurora-silicon/mesa#607](https://github.com/aurora-silicon/mesa/pull/607) (draft): asahi: compile border-free twins on G15G/S/C and G16G (gen-9 descriptors)
- [aurora-silicon/mesa#606](https://github.com/aurora-silicon/mesa/pull/606) (draft): asahi: evaluate uniform-only G16G fragment arithmetic per draw
- [aurora-silicon/mesa#605](https://github.com/aurora-silicon/mesa/pull/605) (draft): asahi: G16G blend destination term first, uniform-only arithmetic before the pixel wait
- [aurora-silicon/mesa#604](https://github.com/aurora-silicon/mesa/pull/604) (draft): asahi: share negative-zero operands for AGX3B uniform clamps
- [omacom/omarchy-pkgs#3180](https://github.com/omacom/omarchy-pkgs/pull/3180): hyprland: keep wl-output-global-grace.patch from crashing the unit tests
- [aurora-silicon/linux#2615](https://github.com/aurora-silicon/linux/pull/2615): ASoC: tas2764: J604 amplifier init writes through the built-in quirks
- [omacom/omarchy-mac-pkgs#167](https://github.com/omacom/omarchy-mac-pkgs/pull/167): test: run only the update on the sudo boundary's PATH
- [omacom/omarchy#19590](https://github.com/omacom/omarchy/pull/19590): Let the balanced CPU floor follow the smallest core
- [omacom/omarchy-pkgs#3141](https://github.com/omacom/omarchy-pkgs/pull/3141): aurora-fwextract: keep the M4-family Wi-Fi firmware and camera tuning
- [omacom/mesa#51](https://github.com/omacom/mesa/pull/51): asahi: compile border-free twins on G16G
- [omacom/mesa#50](https://github.com/omacom/mesa/pull/50): asahi: evaluate uniform-only G16G fragment arithmetic per draw
- [omacom/mesa#49](https://github.com/omacom/mesa/pull/49): asahi: G16G blend destination term first, uniform-only arithmetic before the pixel wait
- [omacom/mesa#48](https://github.com/omacom/mesa/pull/48): asahi: share negative-zero operands for G16 uniform clamps
- [aurora-silicon/aura#271](https://github.com/aurora-silicon/aura/pull/271): usb: dwc3: never handle events inside event handling
- [omacom/omarchy-mac-pkgs#147](https://github.com/omacom/omarchy-mac-pkgs/pull/147): omarchy-mac: wait for the udev queue before speaker safety starts
- [omacom/libva-v4l2_request#115](https://github.com/omacom/libva-v4l2_request/pull/115) (draft): encode: H.264/HEVC on V4L2 stateful encoders, with packed sequence headers for container extradata
- [aurora-silicon/aura#269](https://github.com/aurora-silicon/aura/pull/269): memory: report the firmware RO range on every chip that sets CTRR_C
- [aurora-silicon/linux#2020](https://github.com/aurora-silicon/linux/pull/2020) (draft): sched/fair: Treat a task that runs without sleeping on a smaller CPU as misfit (MISFIT_RUNTIME, off by default)
- [aurora-silicon/linux#1998](https://github.com/aurora-silicon/linux/pull/1998): PCI: apple: power the J604's empty SD card reader down, as macOS does
- [aurora-silicon/linux#1995](https://github.com/aurora-silicon/linux/pull/1995) (draft): media: apple: avd: decode 10-bit HEVC into P030 (new V4L2 format)
- [aurora-silicon/linux#1994](https://github.com/aurora-silicon/linux/pull/1994): media: apple: prores: T8132 (M4) engine, gated by its initialization file
- [aurora-silicon/aura#266](https://github.com/aurora-silicon/aura/pull/266) (draft): T8132: recover a failed ANS start from off, set up each PCIe root port's DBI at its own device, arm the watchdog in an ESP Stage 2
- [aurora-silicon/linux#1965](https://github.com/aurora-silicon/linux/pull/1965): soc: apple: sleep: T8132: stop the SD card reader's port at the commit
- [aurora-silicon/linux#1962](https://github.com/aurora-silicon/linux/pull/1962) (draft): PCI: apple: radio: take the radio SIDs from apple,managed-sids and admit port 2
- [aurora-silicon/linux#1959](https://github.com/aurora-silicon/linux/pull/1959) (draft): HID: apple-mtp-haptic: let the click strength be set at run time
- [omacom/omarchy-mac-pkgs#143](https://github.com/omacom/omarchy-mac-pkgs/pull/143): omarchy-mac: give Apple Silicon power profiles the CPU frequency floor
- [omacom/omarchy-mac-pkgs#142](https://github.com/omacom/omarchy-mac-pkgs/pull/142): Follow the room light with the keyboard backlight
- [omacom/omarchy-pkgs#2758](https://github.com/omacom/omarchy-pkgs/pull/2758) (draft): omarchy-power-profiles-daemon: Apple Silicon CPU profiles as macOS runs them
- [aurora-silicon/linux#1894](https://github.com/aurora-silicon/linux/pull/1894) (draft): ASoC: tas2764: J604 amplifier shutdown as macOS does it
- [omacom/omarchy-mac-pkgs#134](https://github.com/omacom/omarchy-mac-pkgs/pull/134) (draft): Name the 14-inch MacBook Pro M4 panel: 10-bit, Display P3
- [omacom/omarchy-mac-pkgs#133](https://github.com/omacom/omarchy-mac-pkgs/pull/133): Wait only for the replayed input events before the passphrase prompt
- [omacom/omarchy-mac-pkgs#132](https://github.com/omacom/omarchy-mac-pkgs/pull/132): omarchy-mac-boot: dtb-overlays: read the device tree to the end
- [omacom/omarchy-mac-pkgs#131](https://github.com/omacom/omarchy-mac-pkgs/pull/131): omarchy-mac: start fprintd after the Touch ID activation, once a node
- [omacom/omarchy#18583](https://github.com/omacom/omarchy/pull/18583): Keep the keyboard backlight's restore level across a reboot
- [omacom/omarchy#18582](https://github.com/omacom/omarchy/pull/18582): Give a built-in panel the monitor settings its platform package names
- [omacom/omarchy#18228](https://github.com/omacom/omarchy/pull/18228): Retry the kyber write while a sibling disk switches scheduler
- [omacom/omarchy#18177](https://github.com/omacom/omarchy/pull/18177): Let a platform leave keys its keyboards lack out of the keybindings menu
- [omacom/omarchy#18176](https://github.com/omacom/omarchy/pull/18176): Blur the Glassy bar from the cached wallpaper blur

<!-- front:prs:end -->

 One has merged: [aurora-silicon/linux#1392](https://github.com/aurora-silicon/linux/pull/1392), the GPU fix for blank zero-copy video (our 0164, in Aurora's base since our 1.800), merged on 2026-11-10 and carried into omacom's kernel as [omacom/linux-aurora#438](https://github.com/omacom/linux-aurora/pull/438). Open so far:

- [omacom/omarchy#18176](https://github.com/omacom/omarchy/pull/18176): blur the Glassy bar from the cached wallpaper blur (59 to 119 fps on this panel, re-measured on 1.731); reopened; the same change as [#18191](https://github.com/omacom/omarchy/pull/18191)
- [omacom/omarchy#18228](https://github.com/omacom/omarchy/pull/18228): retry the kyber scheduler write while a sibling NVMe namespace switches
- [omacom/omarchy#18229](https://github.com/omacom/omarchy/issues/18229) (issue): `omarchy-mac`'s mpv default never installs because `omarchy-settings` owns `/etc/mpv/mpv.conf`; linked from [omacom/omarchy-mac-pkgs#66](https://github.com/omacom/omarchy-mac-pkgs/issues/66). An M1 user's measurements there showed that the stock default (`hwdec=auto-safe`) is the right one on the M1; our reply (2026-11-09) agrees and adds M4 numbers
- [omacom/linux-aurora#432](https://github.com/omacom/linux-aurora/issues/432) (issue): J604 test results
- [omacom/omarchy#18177](https://github.com/omacom/omarchy/pull/18177): keybindings menu leaves out absent keys; reopened on 2026-11-10, unchanged (the same change is our kbd-0104)
- [omacom/omarchy-mac-installer#212](https://github.com/omacom/omarchy-mac-installer/pull/212) (draft): collect the M4-family Wi-Fi firmware (BCM4388 rev C2) from the installed macOS, a port of aurora-silicon/aurora-installer 34f90a5 with its author Ryan Murray as co-author

The Aurora kernel has merged DjDeveloperr's J604 / M4 device trees (aurora-silicon/linux #1142 and follow-ups). We rebase onto them and send only what goes beyond them.

## Credits

This builds on the work of [Aurora Silicon](https://github.com/aurora-silicon) (the M4 kernel, including DjDeveloperr's M4 device trees and drivers), the [omacom](https://github.com/omacom) Omarchy for Apple Silicon team, and the [Asahi Linux](https://asahilinux.org) project, whose kernel, m1n1, Mesa, audio and installer work everything here stands on. `CREDITS.md` counts each author's patches.

## License

Each part keeps the license of the project it changes; see `LICENSE.md`.
