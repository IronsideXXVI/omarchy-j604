# Kernel patch numbers: 1.736 to 1.800

Kernel 1.800 rebases the J604 series from aurora-silicon/linux `integration/aurora` `6ff31cbfa2` onto `a82a1ebda940`, and the patches are numbered again from 0001. `STATUS.md` and `UPSTREAM.md` name patches by the number they had when they were added (the 1.703 to 1.736 series). This table maps those numbers to the files in `patches/`.

New in 1.800: 0221 (`arm64: dts: apple: t8132-j604: keep the MTP handoff of this boot chain`) and 0222 (`soc: apple: t8132: keep the J604's own PMGR domain policy for now`).

New in 1.802: 0228 (`cpufreq: apple-soc: point the idle drop at the cluster's CPUs at init`), the fix that makes 0240-0241 (1.800: 0223-0224) run.

New in 1.803: 0229 (`drm/asahi: g16: stop the firmware at the sleep's commit with g16_deep_sleep`), 0230 (`soc: apple: pmp: release the GPU's AGX vote at the sleep's commit`), 0231 (`drm/asahi: g16: end the runtime parameters' sysfs values with a newline`).

New in 1.804: 0232 (`drm/apple: m4: do not let one interval after idle open the swap_pace gate`), 0233 (`media: apple: avd: decode H.264 in frame-based mode`).

New in 1.805: 0234 (`arm64: dts: apple: t8132-j604: report the always-zero SMC temperatures as no data`), 0235 (`arm64: dts: apple: t8132-j604: Describe the DP audio of the second external display processor`), 0236 (`drm: apple: dcp: let the T8132 HDMI output's idle release engage`).

New in 1.807: 0237 (`soc: apple: s2r: optionally arm the bring-up watchdog when ->enter starts`).

New in 1.808: 0238 (`soc: apple: s2r: leave a breadcrumb in the PMU for each late sleep step`), 0239 (`soc: apple: sleep: optionally log the AP wake masks when they are undone`), 0240 (`cpufreq: apple-soc: optionally wait for a transition before an idle drop`).

New in 1.809: 0241 (`soc: apple: s2r: send the enclave no sleep request in a run set to abort`), 0242 (`soc: apple: s2r: keep the bring-up watchdog armed through resume`), 0243 (`soc: apple: s2r: refuse an abort_before that no step reaches`), 0244 (`soc: apple: s2r: breadcrumb the abort, the resume entry and the end of a sleep`), 0245 (`cpufreq: apple-soc: wait for a transition before an idle drop by default`).

New in 1.810: 0246 (`soc: apple: s2r: let one bring-up abort still tell the enclave`).

New in 1.811: 0247 (`iommu: apple-dart: optionally save a DART without a sleep list`).

New in 1.813: 0248 (`soc: apple: s2r: check what suspend-to-RAM needs before offering it`), 0249 (`arm64: apple: describe the T8132 CPU complexes, used only on request`).

| 1.736 | 1.800 | Subject |
|---|---|---|
| 0001 | 0001 | dt-bindings: pci: apple,pcie: Describe the three T8132 root ports |
| 0002 | 0002 | arm64: dts: apple: t8132-j604: Enable the internal keyboard and trackpad |
| 0003 | 0003 | soc: apple: pmp: admit the J604 to the retained display vote |
| 0004 | 0004 | drm/asahi: g16: Check the board before taking the PMP telemetry lease |
| 0005 | superseded: aurora-silicon/linux#1329 (`833be9cc6a`) selects the same performance states from the ADT layout | drm/asahi: g16: Select the J604 performance states |
| 0006 | 0005 | soc: apple: pmp-display: Take the GPU vote on the J604 as well |
| 0007 | 0006 | soc: apple: pmp-display: Name the PMP table checks after T8132 |
| 0008 | 0007 | drm/asahi: g16: Keep the probe within the kernel stack |
| 0009 | 0008 | drm/asahi: g16: Accept the sampler heap of render and compute commands |
| 0010 | 0009 | arm64: dts: apple: t8132-j604: Power down the unused ISP |
| 0011 | 0010 | iommu/apple-dart: Requalify only the changed DCP SID23 leaves |
| 0012 | 0011 | drm/asahi: g16: add a testing knob for idle core power-off |
| 0013 | 0012 | arm64: dts: apple: t8132-j604: Let the camera power the ISP |
| 0014 | 0013 | dt-bindings: soc: apple: apple,t6041-aop: Add the T8132 ALS and LAS services |
| 0015 | 0014 | selftests: apple-dcp: stub the m4 HDR atomic check in the VRR unit |
| 0016 | 0015 | selftests: apple-dcp: stub the m4 panel VRR request in the VRR unit |
| 0017 | 0016 | arm64: dts: apple: t8132: Describe the ACIO3 USB4 host router |
| 0018 | 0017 | thunderbolt: test: allocate the host DP tunnels of the HBR3 tests with this tree's call |
| 0019 | 0018 | arm64: dts: apple: t8132-j604: Describe the trackpad actuator |
| 0020 | 0019 | dt-bindings: arm: pmu: Add the Apple T8132 (M4) PMU |
| 0021 | 0020 | drivers/perf: apple_m1: Match the T8132 (M4) PMU |
| 0022 | 0021 | tools/asahi: Point the M4 PMU model test at the T8132 entry |
| 0023 | 0022 | arm64: dts: apple: t8132: Describe the CPU PMU |
| 0024 | 0023 | arm64: dts: apple: t8132: describe the JPEG engines |
| 0025 | 0024 | media: apple: jpeg: test the T8132 revision under the T6041 match |
| 0026 | 0025 | irqchip/apple-aic: Save and restore AICv2 state across system sleep |
| 0027 | 0026 | dt-bindings: power: Add Apple PMGR system sleep controls |
| 0028 | 0027 | dt-bindings: power: Add Apple PMGR suspend-to-RAM controls |
| 0029 | 0028 | dt-bindings: arm: apple: Add CPU power controls |
| 0030 | 0029 | arm64: Add CPU hotplug for Apple spin-table CPUs |
| 0031 | 0030 | arm64: apple_cpu: Power gate CPU clusters with no online CPU |
| 0032 | 0031 | mfd: macsmc: Add a system sleep handshake |
| 0033 | 0032 | mfd: macsmc: Notify the SMC of AP sleep for suspend-to-RAM |
| 0034 | 0033 | irqchip/apple-aic: Save and restore AICv3 state across system sleep too |
| 0035 | 0034 | mfd: macsmc: Keep the suspend-to-idle SMC handshake opt-in |
| 0036 | 0035 | arm64: apple_cpu: Support T8132, which has no CYC_OVRD |
| 0037 | in Aurora's base (same content as `46f876e27b`) | tools: asahi: give the T6030 graph harness the two names the extracted report probe now uses |
| 0038 | in Aurora's base (same patch) | iommu: apple-dart: generalize the external display power gating |
| 0039 | in Aurora's base (same patch) | dt-bindings: iommu: apple,dart: describe the clock protection slice |
| 0040 | in Aurora's base (same patch) | pmdomain: apple: pmc: let a consumer switch a pipe's interface enables |
| 0041 | in Aurora's base (same patch) | dt-bindings: power: apple,pmc: consumer-switched interface enables |
| 0042 | in Aurora's base (same content as `6fde3b9bb2`) | soc: apple: pmp-display: let the J713 DCP release its DISP vote |
| 0043 | in Aurora's base (same patch) | dt-bindings: display: apple,dcp: name the display pipe's PMC interface |
| 0044 | 0036 | soc: apple: pmp-display: let the J604 DCP release its DISP vote |
| 0045 | 0037 | drm/asahi: g16: expose g16_idle_power_off in sysfs and switch it at runtime |
| 0046 | 0038 | tools/asahi: pmgr-bus-tests: cover the T8132 PMGR banks |
| 0047 | 0039 | drm/apple: give the J516C panel an EDID |
| 0048 | 0040 | drm: apple: answer eDP for the J313 backlight and cap it at 394 nits |
| 0049 | 0041 | drm: apple: clamp J313 backlight readback to the panel maximum |
| 0050 | 0042 | drm/apple: power-cycle the J313 panel the boot loader handed over |
| 0051 | 0043 | drm/apple: m4: select the internal panel profile by board |
| 0052 | 0044 | drm/apple: m4: add the MacBook Pro (14-inch, M4) panel record |
| 0053 | 0045 | drm: apple: iomfb_m4: log rejected allocate_buffer requests |
| 0054 | superseded: aurora-silicon/linux#1389 (`9b2acdbbb2`) accepts the same buffer alignment on the J604 | drm: apple: iomfb_m4: accept the J604 allocate_buffer alignment |
| 0055 | 0046 | drm/apple: m4: Bound firmware calls by progress, not by one fixed total |
| 0056 | 0047 | drm/apple: iomfb_m4: Report what the startup health checks cost |
| 0057 | 0048 | drm/apple: m4: time-stamp the J604 panel's swaps for 120 Hz |
| 0058 | 0049 | drm/apple: iomfb_m4: Say which startup health check cost the most |
| 0059 | 0050 | drm/apple: m4: fade between panel backlight levels |
| 0060 | 0051 | drm/apple: m4: describe the J604 panel as Display P3 in its EDID |
| 0061 | 0052 | drm/apple: test the raw ADT panel parser |
| 0062 | 0053 | drm/apple: read the panel peak luminance from raw ADT |
| 0063 | 0054 | drm/apple: m4: show HDR content as EDR on the J604 panel (opt-in) |
| 0064 | 0055 | drm/apple: test the T8132 panel EDR words and the HDR panel EDID |
| 0065 | 0056 | drm/apple: m4: adaptive sync on the J604 panel, as an experiment |
| 0066 | 0057 | drm/apple: read the panel's bright-surroundings SDR maximum from raw ADT |
| 0067 | 0058 | drm/apple: m4: raise the J604 panel's SDR ceiling for bright surroundings |
| 0068 | 0059 | drm/apple: m4: power the internal DCP and its domains down while blanked |
| 0069 | 0060 | drm/apple: m4: let the J604 DCP power its display domains down |
| 0070 | 0061 | drm/apple: m4: keep the panel EDID path off an external T8132 DCP |
| 0071 | 0062 | phy: apple: atc: run a T8132 PHY with no USB side as a four-lane DP PHY |
| 0072 | 0063 | arm64: dts: apple: t8132: describe the scaler and its DART |
| 0073 | 0064 | accel/ane: identify the T8132 (M4) Neural Engine firmware |
| 0074 | 0065 | arm64: dts: apple: t8132: describe the Neural Engine and its DARTs |
| 0075 | 0066 | media: apple: ave: probe the T8132 (M4) encoder, probe only |
| 0076 | 0067 | dt-bindings: power: apple,pmgr-s2r: describe asahi,s2r-reset-vector |
| 0077 | 0068 | drm: apple: dcp: drive the HDMI port of a T8132 dcpext |
| 0078 | 0069 | drm/apple: m4: let fade steps ride on client presents |
| 0079 | 0070 | drm/apple: av: give the KUnit lifetime endpoints an orphan wait queue |
| 0080 | 0071 | accel/ane: take the H16 DAPF record count from the platform |
| 0081 | 0072 | accel/ane: map the H16 XL window only on SoCs that have one |
| 0082 | 0073 | accel/ane: let the T8132 Neural Engine power up without engine tunables (opt-in) |
| 0083 | 0074 | drm/apple: edid: describe a built-in panel's variable refresh range |
| 0084 | 0075 | drm/apple: m4: offer adaptive sync on the J604 panel by default |
| 0085 | 0076 | drm/apple: m4: offer HDR on the J604 panel by default |
| 0086 | 0077 | drm/apple: m4: power the J604 display down while blanked by default |
| 0087 | 0078 | drm/asahi: g16: power idle cores off by default |
| 0088 | 0079 | drm/apple: m4: present 10-bit XRGB/ARGB2101010 on the J604 panel |
| 0089 | 0080 | drm/apple: give the J604 panel EDID its luminances and input depth |
| 0090 | 0081 | drm/apple: m4: let the J604 panel's adaptive sync go down to 24 Hz |
| 0091 | 0082 | drm/apple: m4: show wide-gamut SDR frames in BT.2020 on the J604 panel |
| 0092 | 0083 | PM: suspend: Go back to sleep from a dark wake |
| 0093 | 0084 | arm64: restore Apple CPU state after a firmware-driven suspend |
| 0094 | 0085 | spmi: apple: quiet the response vector for a firmware suspend |
| 0095 | 0086 | pinctrl: apple: put configured pins back after a firmware-driven suspend |
| 0096 | 0087 | cpufreq: apple: take the cluster's state after a sleep as the expected one |
| 0097 | 0088 | soc: apple: mcc: write the system level cache back before a deep sleep |
| 0098 | 0089 | dt-bindings: mailbox: apple: add the secure RTBuddy port |
| 0099 | 0090 | nvme-apple: take ANS through a firmware-driven suspend |
| 0100 | 0091 | nvme-apple: Take T8132 ANS through a firmware-driven sleep |
| 0101 | 0092 | soc: apple: aop: stop the endpoints for a firmware-driven suspend |
| 0102 | 0093 | ASoC: apple: take audio through a firmware-driven suspend |
| 0103 | 0094 | soc: apple: sep: tell the enclave about a firmware-driven suspend |
| 0104 | 0095 | soc: apple: sep: power the fingerprint sensor off for system sleep |
| 0105 | 0096 | usb: core: hcd: don't release root hub PHYs twice after a failed resume |
| 0106 | 0097 | usb: xhci-plat: let a resume quirk say the controller lost its state |
| 0107 | 0098 | media: apple: isp: stop and restart T8140 firmware across a deep sleep |
| 0108 | 0099 | Input: macsmc-input: wake on the lid and button from a suspend's start |
| 0109 | 0100 | power: supply: macsmc: report the supplies again after a sleep |
| 0110 | 0101 | dt-bindings: power: apple,pmgr-pwrstate: add apple,sleep-driver-owned |
| 0111 | 0102 | soc: apple: mtp: tell the MTP about a firmware-driven suspend |
| 0112 | 0103 | dt-bindings: soc: apple: add the CPU cluster power gates |
| 0113 | 0104 | dt-bindings: soc: apple: cluster-pwrgate: Add the T8132 compatible |
| 0114 | 0105 | soc: apple: sep: Take the PM callbacks by address |
| 0115 | 0106 | selftests: apple-dcp: Give the route stub its sleep_parked flag |
| 0116 | 0107 | usb: typec: tipd: sn201202x: Wake after a failed wake in suspend |
| 0117 | 0108 | drm/apple: m4: power the display down for a sleep that powers the SoC down |
| 0118 | 0109 | drm/apple: m4: test when the display lets a SoC power-down sleep go ahead |
| 0119 | 0110 | selftests: apple-dcp: take the ATC PHY's new mux and restore helpers |
| 0120 | 0111 | drm/apple: t8132_ext: offer Pre-multiplied so dcpext outputs probe |
| 0121 | 0112 | drm/apple: plane: no blend property where only coverage blends |
| 0122 | 0113 | thunderbolt: apple: power ports off for a sleep that powers the SoC down |
| 0123 | 0114 | drm/asahi: g16: say why a SoC power-down sleep is refused |
| 0124 | 0115 | media: apple: avd: tests: give the recovery fixture the variant quirks |
| 0125 | 0116 | media: apple: avd: send the v5 HEVC header on T8132 |
| 0126 | 0117 | media: apple: avd: test the v5 HEVC header across the variant table |
| 0127 | 0118 | accel/ane: pin the J604 Neural Engine preload digest |
| 0128 | 0119 | accel/ane: do not report the identify-only stop as a refusal |
| 0129 | 0120 | phy: apple: atc: serve USB2 when a T8132 USB3 attach finds the common block in use |
| 0130 | 0121 | phy: apple: atc: say whether a port can run SuperSpeed |
| 0131 | 0122 | drm/apple: m4: let a dcpext take a sleep that powers the SoC down |
| 0132 | 0123 | dt-bindings: hwmon: apple,smc-hwmon: discover next to described temperatures |
| 0133 | 0124 | drm/apple: read the panel's thermal minimum cap from the ADT |
| 0134 | 0125 | drm/apple: m4: add an XDR guard for panel levels above the user maximum |
| 0135 | 0126 | drm/apple: m4: bound the internal panel's levels with the XDR guard |
| 0136 | 0127 | selftests: apple-dcp: define DATA_CONNECTION in the lifecycle tipd mock |
| 0137 | 0128 | phy: apple: atc: run DP Alt Mode on a T8132 Type-C port's four lanes |
| 0138 | 0129 | phy: apple: atc: tell whether a Type-C port runs DP Alt Mode |
| 0139 | 0130 | drm: apple: dcp: route DP Alt Mode on a T8132 dcpext |
| 0140 | 0131 | selftests: apple-dcp: extract the T8132 DP Alt Mode helpers |
| 0141 | 0132 | drm/apple: m4: decide when an XDR probe swap may be given up |
| 0142 | 0133 | drm/apple: m4: choose 10 bpc and HDR10 colour on external links |
| 0143 | 0134 | drm/apple: t8132: offer HDR10 and 10 bpc on external outputs |
| 0144 | 0135 | drm: apple: audio: bind a T8132 external DCP's DP audio on its own |
| 0145 | 0136 | Bluetooth: hci_bcm4377: advertise wideband speech on the BCM4388 |
| 0146 | 0137 | drm/apple: m4: decide the EDID and SDR primaries of an external sink |
| 0147 | 0138 | drm/apple: m4: let external frames be 10 bits per component |
| 0148 | 0139 | dt-bindings: iommu: apple,dart: require the filter ranges with a dapf reg |
| 0149 | 0140 | accel/ane: pm: hand back the runtime-PM votes a device's links hold |
| 0150 | 0141 | accel/ane: h16: let the ANE DARTs suspend once the device is bound |
| 0151 | 0142 | drm/apple: m4: decide when an idle external DCP may power down |
| 0152 | 0143 | media: apple: prores: probe only on the T8132 |
| 0153 | 0144 | arm64: dts: apple: t8132: describe the ProRes encoder and its DART |
| 0154 | 0145 | drm/apple: m4: let the content decide the EDR headroom |
| 0155 | 0146 | drm/apple: say a T8132 dcpext drives its Type-C ports, not that they are unusable |
| 0156 | 0147 | arm64: configs: asahi: enable CONFIG_EDAC for EDAC_APPLE_SOC |
| 0157 | 0148 | j604: integrate the J604 series onto Aurora 7.3 (shipping snapshot) |
| 0158 | 0149 | iommu/apple-dart: admit the J604's internal display DARTs at boot again |
| 0159 | 0150 | ASoC: apple: macaudio: J604: match the J616s config again, not the J614s |
| 0160 | 0151 | arm64: dts: apple: t8132-j604: Run the Mesa sensor in SPI mode 1 again |
| 0161 | 0152 | arm64: dts: apple: t8132-j604: keep the unmanaged radio contract |
| 0162 | 0153 | arm64: dts: apple: t8132-j604: point dcpext0 at the J604's ATC2 PHY |
| 0163 | 0154 | soc: apple: mailbox: keep the send wait killable, not interruptible |
| 0164 | in Aurora's base: merged as aurora-silicon/linux#1392 (`9554b061cb25`) | rust: drm: gpuvm: lock a shared reservation once in exec_lock |
| 0165 | 0155 | soc: apple: s2r: offer suspend-to-RAM whenever the boot firmware opts in |
| 0166 | 0156 | PM: suspend: add suspend_set_ops_keep_s2idle() |
| 0167 | 0157 | soc: apple: s2r: keep suspend-to-idle the default mem sleep |
| 0168 | in Aurora's base (same patch) | thunderbolt: apple: Do not start the NHI without its DART |
| 0169 | in Aurora's base (same patch) | thunderbolt: Unregister XDomains when the domain is removed |
| 0170 | in Aurora's base (same patch) | thunderbolt: Refuse XDomain DMA paths once the XDomain is unplugged |
| 0171 | in Aurora's base (same patch) | thunderbolt: Ignore DROM entries for ports the router does not implement |
| 0172 | in Aurora's base (same patch) | usb: typec: tipd: preserve dry result during LDCM startup |
| 0173 | in Aurora's base (same patch) | usb: typec: tipd: disable LDCM work during removal |
| 0174 | in Aurora's base (same patch) | thunderbolt: apple: Restart the port when the peer link is lost |
| 0175 | in Aurora's base (same patch) | usb: typec: tipd: Take the USB4 router again after it rebinds |
| 0176 | 0158 | phy: apple: atc: add a lab switch for USB3 over USB4 on T8132 |
| 0177 | 0159 | drm/asahi: g16: make the governor's idle floor a runtime parameter |
| 0178 | 0160 | drm/asahi: g16: post the TA record before the FS record |
| 0179 | 0161 | drm/asahi: g16: make the completion repoll interval adjustable at runtime |
| 0180 | 0162 | drm/asahi: g16: report command start and retirement latency |
| 0181 | 0163 | drm/asahi: g16: try a firmware stop and warm restart across suspend-to-RAM |
| 0182 | 0164 | drm/asahi: g16: let an idle J604 GPU drop to S1 |
| 0183 | 0165 | soc: apple: cluster-pwrgate: report the words of a disabled node |
| 0184 | 0166 | soc: apple: cluster-pwrgate: let bring-up set a disabled node's enables |
| 0185 | 0167 | soc: apple: s2r: leave suspend-to-idle alone in ->begin() |
| 0186 | 0168 | drm/asahi: g16: histogram the firmware start delay of render commands |
| 0187 | 0169 | accel/ane: power up the J604 Neural Engine by default |
| 0188 | 0170 | accel/ane: power the H16 engine off while it is idle |
| 0189 | 0171 | drm/asahi: g16: optionally publish a render command behind a running one |
| 0190 | 0172 | drm/apple: m4: optionally give J604 panel swaps a presentation target |
| 0191 | 0173 | drm/asahi: g16: split the firmware start delay by VM switches |
| 0192 | 0174 | phy: apple: atc: add a test switch for DP Alt Mode pin assignment D on T8132 |
| 0193 | 0175 | drm: apple: dcp: let an idle T8132 HDMI output release its PHY and crossbar |
| 0194 | 0176 | arm64: dts: apple: t8132-j604: Enable the SIO and the HDMI port's audio |
| 0195 | 0177 | drm/apple: m4: optionally pace J604 panel swaps onto the 120 Hz grid |
| 0196 | 0178 | drm/apple: m4: learn the swap_pace lead from completed swaps |
| 0197 | 0179 | drm/apple: m4: add knobs for the J604 panel's adaptive-sync request |
| 0198 | 0180 | drm/asahi: g16: optionally reuse a leased PB manager of the same VM |
| 0199 | 0181 | drm/apple: m4: keep swap_pace clear of the tick boundaries |
| 0200 | 0182 | drm/apple: m4: make the media rate word of the adaptive-sync request selectable |
| 0201 | 0183 | drm/asahi: g16: reuse a leased PB manager of the same VM by default |
| 0202 | 0184 | drm/apple: m4: learn the swap_pace floor from unpaced swaps only |
| 0203 | 0185 | drm/apple: m4: count swap_pace outcomes in debugfs |
| 0204 | 0186 | drm/apple: m4: hold J604 panel frames for the grid only at video rates |
| 0205 | 0187 | drm/apple: m4: pace J604 panel video onto the 120 Hz grid by default |
| 0206 | 0188 | drm/apple: m4: let held J604 panel swaps start sooner after the tick between |
| 0207 | 0189 | drm/apple: m4: learn the swap_pace floor with swap_pace on |
| 0208 | 0190 | media: apple: scaler: fill alpha for every RGB destination |
| 0209 | 0191 | dt-bindings: media: apple,t8140-ave: name the T8132 encoder resources |
| 0210 | 0192 | arm64: dts: apple: t8132: name the video encoder resources |
| 0211 | 0193 | arm64: dts: apple: t8132-j604: power the video encoder, probe only |
| 0212 | 0194 | media: apple: ave: add T8132 start-up trial controls in debugfs |
| 0213 | 0195 | soc: apple: s2r: pass the boot firmware's wake hold requests |
| 0214 | 0196 | drm/apple: m4: leave an idle external DCP down across a sleep |
| 0215 | 0197 | cpufreq: apple-soc: show the cluster's DVFS command and status words |
| 0216 | 0198 | drm/apple: m4: do not hold the first J604 panel frames after idle |
| 0217 | 0199 | media: apple: ave: start the T8132 (M4) encoder |
| 0218 | 0200 | media: apple: ave: take P010 before the coded format is set |
| 0219 | 0201 | cpufreq: apple-soc: read the T8132 current state from the status register |
| 0220 | 0202 | drm/apple: m4: optional real-time sleep for held J604 panel swaps |
| 0221 | 0203 | drm/apple: m4: learn the swap_pace floor outside video only |
| 0222 | 0204 | media: apple: ave: code a visible picture inside a larger buffer |
| 0223 | 0205 | media: apple: scaler: scale to another size |
| 0224 | in Aurora's base (same patch) | hwmon: macsmc: report the M4 Pro GPU sensors' powered-down placeholders as no data |
| 0225 | 0206 | arm64: dts: apple: t8132-j604: report the GPU temperature placeholders as no data |
| 0226 | 0207 | drm/apple: m4: learn the swap_pace floor from unspaced swaps only |
| 0227 | 0208 | drm/apple: m4: hold J604 panel swaps only for ticks the panel makes |
| 0228 | 0209 | media: apple: jpeg: decode baseline JPEG on T8132 |
| 0229 | 0210 | media: apple: scaler: give the horizontal step words the reciprocal |
| 0230 | 0211 | drm/apple: m4: do not take a swap_pace floor above 600 us |
| 0231 | 0212 | soc: apple: add bring-up switches to leave out suspend-to-RAM notices |
| 0232 | 0213 | drm/apple: add a bring-up switch to suspend the DCPs as for suspend-to-idle |
| 0233 | 0214 | drm/apple: m4: compute the swap_pace floor cap in 64 bits |
| 0234 | 0215 | soc: apple: add bring-up switches to split the suspend-to-RAM devices stage |
| 0235 | 0216 | arm64: dts: apple: t8132: stop the SIO coprocessor window at its mailbox |
| 0236 | 0217 | arm64: dts: apple: t8132-j604: take the SIO window from t8132-sio.dtsi |
| 0237 | 0218 | media: apple: avd: apply request controls the capture buffers fit |
| 0238 | 0219 | dt-bindings: power: reset: apple,smc-reboot: add the failed-boot record cells |
| 0239 | 0220 | power: reset: macsmc-reboot: print what ended the previous boot |
| 0240 | 0223 | cpuidle: apple: let the cluster driver slow an all-idle cluster (M4) |
| 0241 | 0224 | cpufreq: apple-soc: drop the all-idle T8132 P cluster to its slowest state |
| 0242 | 0225 | soc: apple: pm_shim: find the SEP power state through the sleep node |
| 0243 | 0226 | iommu/apple-dart: add a bring-up switch for a test mem sleep that is not firmware-driven |
| 0244 | dropped: its test sleep is no longer needed, and it does not apply on the J604 | PCI: apple: add a bring-up switch for a test mem sleep that is not firmware-driven |
| 0245 | 0227 | phy: apple: atc: add a bring-up switch for a test mem sleep that is not firmware-driven |
