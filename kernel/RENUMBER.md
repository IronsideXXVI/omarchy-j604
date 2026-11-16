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

New in 1.816: 0250 (`arm64: apple: let a core powered down alone be tried again, for bring-up`), 0251 (`soc: apple: cluster-pwrgate: optionally log the T8132 sleep words`), 0252 (`arm64: dts: apple: t8132-j604: wake only from the sources macOS uses`).

New in 1.820: 0253 (`phy: apple: atc: keep a T8132 CIO link when its USB host stops`), 0254 (`phy: apple: atc: enable the dummy PHY on T8132 SuperSpeed ports`), 0255 (`drm/apple: dpavservep: probe again when the EDID is not read yet`), 0256 (`selftests: apple-dcp: stub the EDID re-probe in the rate harness`), 0257 (`drm/apple: afk: one initializer for an endpoint's locks and queues`), 0258 (`arm64: cpufeature: hide WFxT on Apple M4+ without a forced override`), 0259 (`soc: apple: sep: publish the chardevs without holding their slot locks`), 0260 (`power: supply: macsmc: hold the charge with CHLS where CHTE and CH0C are absent`), 0261 (`arm64: dts: apple: t8132: describe the AOP's sensor services`), 0262 (`HID: dockchannel-hid: write each frame to the FIFO under one lock`), 0263 (`Input: macsmc-input - confirm a lid-open report during system sleep`), 0264 (`arm64: dts: apple: t8132-j604: take the AOP sensor nodes from t8132.dtsi`).

New in 1.821: 0265 (`arm64: apple: stop parked cores for a T8132 sleep commit, as macOS does`), 0266 (`soc: apple: s2r: add the T8132 sleep sequence macOS uses, for bring-up`), 0267 (`dt-bindings: power: apple,pmgr-s2r: describe the T8132 sequence`), 0268 (`arm64: dts: apple: t8132-j604: describe the T8132 sleep sequence's windows`), 0269 (`soc: apple: s2r: report the CPU stop word around a T8132 sleep`), 0270 (`drm/asahi: g17: restart the GPU firmware across a firmware-driven suspend`), 0271 (`drm/asahi: G16: ride out dark sensors while the GPU is idle`), 0272 (`drm/asahi: G16: stop the firmware for a deep sleep and restart it warm`), 0273 (`Revert "soc: apple: pmp: release the GPU's AGX vote at the sleep's commit"`), 0274 (`arm64: dts: apple: t8132: map the whole PMP SRAM`), 0275 (`soc: apple: pmp: keep T8132's __DATA across a deep sleep`), 0276 (`soc: apple: pmp: keep the GPU's AGX vote through a deep sleep`).

New in 1.823: 0277 (`arm64: dts: apple: t8132-j604: point the P cluster's power gate at +0x60`), 0278 (`mmc: sdhci-pci-gli: optionally runtime suspend an idle GL9755`), 0279 (`drm/apple: m4: power the internal DCP down for any deep sleep`).

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


# Kernel patch numbers: 1.858 to 1.862

Kernel 1.862 rebases the J604 series from `a82a1ebda940` onto aurora-silicon/linux `integration/aurora` `cc7696978e4c` (Aurora's tip on 2026-11-12), and the patches are numbered again from 0001 (353 patches). 271 of the 368 1.858 files are carried with the same subject; the other 97 are not in 1.862: Aurora's tip has them or its own version of them (among them its own suspend-to-RAM driver, which replaces our sleep series), or the rebase left them out (see the 1.862 entry in `STATUS.md`). Patches that are new in 1.862 are the rebase's own commits and later J604 work. `STATUS.md` and `UPSTREAM.md` keep naming patches by the number they had when they were added; this table maps the 1.858 files to the 1.862 files.

| 1.858 | 1.862 |
| --- | --- |
| 0001 | 0001 |
| 0002 | not in 1.862 |
| 0003 | 0002 |
| 0004 | 0003 |
| 0005 | 0004 |
| 0006 | 0005 |
| 0007 | 0006 |
| 0008 | 0007 |
| 0009 | 0008 |
| 0010 | 0009 |
| 0011 | 0010 |
| 0012 | 0011 |
| 0013 | 0012 |
| 0014 | 0013 |
| 0015 | 0014 |
| 0016 | 0015 |
| 0017 | 0016 |
| 0018 | not in 1.862 |
| 0019 | not in 1.862 |
| 0020 | 0334 |
| 0021 | 0017 |
| 0022 | 0018 |
| 0023 | 0019 |
| 0024 | not in 1.862 |
| 0025 | 0020 |
| 0026 | 0021 |
| 0027 | 0022 |
| 0028 | 0023 |
| 0029 | 0024 |
| 0030 | 0025 |
| 0031 | 0026 |
| 0032 | 0027 |
| 0033 | 0028 |
| 0034 | 0029 |
| 0035 | 0030 |
| 0036 | 0031 |
| 0037 | 0032 |
| 0038 | 0033 |
| 0039 | 0034 |
| 0040 | 0035 |
| 0041 | 0036 |
| 0042 | 0037 |
| 0043 | 0038 |
| 0044 | 0039 |
| 0045 | 0040 |
| 0046 | 0041 |
| 0047 | 0042 |
| 0048 | 0043 |
| 0049 | 0044 |
| 0050 | 0045 |
| 0051 | 0046 |
| 0052 | 0047 |
| 0053 | 0048 |
| 0054 | 0049 |
| 0055 | 0050 |
| 0056 | 0051 |
| 0057 | 0052 |
| 0058 | 0053 |
| 0059 | 0054 |
| 0060 | 0055 |
| 0061 | 0056 |
| 0062 | 0057 |
| 0063 | 0058 |
| 0064 | 0059 |
| 0065 | 0060 |
| 0066 | 0061 |
| 0067 | 0062 |
| 0068 | 0063 |
| 0069 | 0064 |
| 0070 | 0065 |
| 0071 | 0066 |
| 0072 | 0067 |
| 0073 | 0068 |
| 0074 | 0069 |
| 0075 | 0070 |
| 0076 | 0071 |
| 0077 | 0072 |
| 0078 | 0073 |
| 0079 | 0074 |
| 0080 | 0075 |
| 0081 | 0076 |
| 0082 | 0077 |
| 0083 | not in 1.862 |
| 0084 | not in 1.862 |
| 0085 | not in 1.862 |
| 0086 | not in 1.862 |
| 0087 | not in 1.862 |
| 0088 | not in 1.862 |
| 0089 | not in 1.862 |
| 0090 | not in 1.862 |
| 0091 | not in 1.862 |
| 0092 | 0078 |
| 0093 | not in 1.862 |
| 0094 | not in 1.862 |
| 0095 | not in 1.862 |
| 0096 | not in 1.862 |
| 0097 | not in 1.862 |
| 0098 | not in 1.862 |
| 0099 | not in 1.862 |
| 0100 | not in 1.862 |
| 0101 | not in 1.862 |
| 0102 | 0079 |
| 0103 | not in 1.862 |
| 0104 | 0080 |
| 0105 | 0081 |
| 0106 | 0082 |
| 0107 | 0083 |
| 0108 | 0084 |
| 0109 | 0085 |
| 0110 | not in 1.862 |
| 0111 | 0086 |
| 0112 | 0087 |
| 0113 | 0088 |
| 0114 | 0089 |
| 0115 | 0090 |
| 0116 | 0091 |
| 0117 | 0092 |
| 0118 | 0093 |
| 0119 | 0094 |
| 0120 | 0095 |
| 0121 | 0096 |
| 0122 | 0097 |
| 0123 | 0098 |
| 0124 | 0099 |
| 0125 | 0100 |
| 0126 | 0101 |
| 0127 | 0102 |
| 0128 | 0103 |
| 0129 | 0104 |
| 0130 | 0105 |
| 0131 | 0106 |
| 0132 | 0107 |
| 0133 | 0108 |
| 0134 | 0109 |
| 0135 | 0110 |
| 0136 | 0111 |
| 0137 | 0112 |
| 0138 | 0113 |
| 0139 | 0114 |
| 0140 | 0115 |
| 0141 | not in 1.862 |
| 0142 | 0116 |
| 0143 | 0117 |
| 0144 | 0118 |
| 0145 | 0119 |
| 0146 | 0120 |
| 0147 | 0121 |
| 0148 | 0122 |
| 0149 | 0123 |
| 0150 | 0124 |
| 0151 | 0125 |
| 0152 | 0126 |
| 0153 | 0127 |
| 0154 | not in 1.862 |
| 0155 | not in 1.862 |
| 0156 | 0128 |
| 0157 | not in 1.862 |
| 0158 | 0129 |
| 0159 | 0130 |
| 0160 | 0131 |
| 0161 | 0132 |
| 0162 | 0133 |
| 0163 | 0134 |
| 0164 | 0135 |
| 0165 | 0136 |
| 0166 | 0137 |
| 0167 | not in 1.862 |
| 0168 | 0138 |
| 0169 | 0139 |
| 0170 | 0140 |
| 0171 | 0141 |
| 0172 | 0142 |
| 0173 | 0143 |
| 0174 | 0144 |
| 0175 | 0145 |
| 0176 | 0146 |
| 0177 | 0147 |
| 0178 | 0148 |
| 0179 | 0149 |
| 0180 | 0150 |
| 0181 | 0151 |
| 0182 | 0152 |
| 0183 | 0153 |
| 0184 | 0154 |
| 0185 | 0155 |
| 0186 | 0156 |
| 0187 | 0157 |
| 0188 | 0158 |
| 0189 | 0159 |
| 0190 | 0160 |
| 0191 | 0161 |
| 0192 | 0162 |
| 0193 | 0163 |
| 0194 | 0164 |
| 0195 | not in 1.862 |
| 0196 | 0165 |
| 0197 | 0166 |
| 0198 | 0167 |
| 0199 | 0168 |
| 0200 | 0169 |
| 0201 | 0170 |
| 0202 | 0171 |
| 0203 | 0172 |
| 0204 | 0173 |
| 0205 | 0174 |
| 0206 | 0175 |
| 0207 | 0176 |
| 0208 | 0177 |
| 0209 | 0178 |
| 0210 | 0179 |
| 0211 | 0180 |
| 0212 | 0181 |
| 0213 | 0182 |
| 0214 | 0183 |
| 0215 | not in 1.862 |
| 0216 | 0184 |
| 0217 | 0185 |
| 0218 | not in 1.862 |
| 0219 | 0186 |
| 0220 | 0187 |
| 0221 | 0188 |
| 0222 | 0189 |
| 0223 | 0190 |
| 0224 | 0191 |
| 0225 | 0192 |
| 0226 | not in 1.862 |
| 0227 | not in 1.862 |
| 0228 | 0193 |
| 0229 | not in 1.862 |
| 0230 | 0194 |
| 0231 | 0195 |
| 0232 | 0196 |
| 0233 | 0197 |
| 0234 | 0198 |
| 0235 | 0199 |
| 0236 | 0200 |
| 0237 | not in 1.862 |
| 0238 | not in 1.862 |
| 0239 | not in 1.862 |
| 0240 | 0201 |
| 0241 | not in 1.862 |
| 0242 | not in 1.862 |
| 0243 | not in 1.862 |
| 0244 | not in 1.862 |
| 0245 | 0202 |
| 0246 | not in 1.862 |
| 0247 | 0203 |
| 0248 | not in 1.862 |
| 0249 | 0204 |
| 0250 | 0205 |
| 0251 | 0206 |
| 0252 | 0207 |
| 0253 | 0208 |
| 0254 | not in 1.862 |
| 0255 | not in 1.862 |
| 0256 | not in 1.862 |
| 0257 | 0209 |
| 0258 | not in 1.862 |
| 0259 | not in 1.862 |
| 0260 | not in 1.862 |
| 0261 | 0210 |
| 0262 | not in 1.862 |
| 0263 | 0211 |
| 0264 | 0212 |
| 0265 | 0213 |
| 0266 | not in 1.862 |
| 0267 | 0214 |
| 0268 | not in 1.862 |
| 0269 | not in 1.862 |
| 0270 | not in 1.862 |
| 0271 | 0215 |
| 0272 | 0291 |
| 0273 | 0216 |
| 0274 | 0217 |
| 0275 | 0218 |
| 0276 | 0219 |
| 0277 | 0220 |
| 0278 | 0221 |
| 0279 | 0222 |
| 0280 | not in 1.862 |
| 0281 | 0223 |
| 0282 | not in 1.862 |
| 0283 | 0224 |
| 0284 | not in 1.862 |
| 0285 | 0225 |
| 0286 | not in 1.862 |
| 0287 | 0226 |
| 0288 | 0227 |
| 0289 | 0228 |
| 0290 | 0229 |
| 0291 | 0230 |
| 0292 | not in 1.862 |
| 0293 | not in 1.862 |
| 0294 | 0231 |
| 0295 | 0232 |
| 0296 | not in 1.862 |
| 0297 | not in 1.862 |
| 0298 | not in 1.862 |
| 0299 | not in 1.862 |
| 0300 | 0233 |
| 0301 | not in 1.862 |
| 0302 | not in 1.862 |
| 0303 | 0234 |
| 0304 | not in 1.862 |
| 0305 | 0235 |
| 0306 | 0236 |
| 0307 | not in 1.862 |
| 0308 | not in 1.862 |
| 0309 | not in 1.862 |
| 0310 | not in 1.862 |
| 0311 | not in 1.862 |
| 0312 | 0237 |
| 0313 | 0238 |
| 0314 | 0239 |
| 0315 | 0240 |
| 0316 | 0241 |
| 0317 | 0242 |
| 0318 | not in 1.862 |
| 0319 | not in 1.862 |
| 0320 | 0243 |
| 0321 | not in 1.862 |
| 0322 | not in 1.862 |
| 0323 | not in 1.862 |
| 0324 | not in 1.862 |
| 0325 | 0244 |
| 0326 | 0245 |
| 0327 | 0246 |
| 0328 | 0247 |
| 0329 | 0248 |
| 0330 | not in 1.862 |
| 0331 | not in 1.862 |
| 0332 | 0249 |
| 0333 | 0250 |
| 0334 | not in 1.862 |
| 0335 | not in 1.862 |
| 0336 | 0251 |
| 0337 | not in 1.862 |
| 0338 | 0252 |
| 0339 | 0253 |
| 0340 | 0254 |
| 0341 | 0255 |
| 0342 | 0256 |
| 0343 | 0257 |
| 0344 | not in 1.862 |
| 0345 | not in 1.862 |
| 0346 | 0258 |
| 0347 | 0259 |
| 0348 | 0260 |
| 0349 | not in 1.862 |
| 0350 | 0261 |
| 0351 | not in 1.862 |
| 0352 | not in 1.862 |
| 0353 | not in 1.862 |
| 0354 | 0262 |
| 0355 | not in 1.862 |
| 0356 | 0263 |
| 0357 | not in 1.862 |
| 0358 | not in 1.862 |
| 0359 | 0264 |
| 0360 | 0265 |
| 0361 | 0266 |
| 0362 | 0267 |
| 0363 | not in 1.862 |
| 0364 | 0268 |
| 0365 | not in 1.862 |
| 0366 | 0288 |
| 0367 | not in 1.862 |
| 0368 | not in 1.862 |


# Kernel patch numbers: 1.869 to 1.870

Kernel 1.870 rebases the J604 series from `cc7696978e4c` onto aurora-silicon/linux `integration/aurora` `abd6d4a09ffe` (Aurora's tip on 2026-11-13), and the patches are numbered again from 0001 (382 patches). 376 of the 380 1.869 files are carried with the same subject; the other 4 are in Aurora's tip now. Patches that are new in 1.870 are the rebase's own commit and later J604 work. `STATUS.md` and `UPSTREAM.md` keep naming patches by the number they had when they were added; this table maps the 1.869 files to the 1.870 files.

| 1.869 | 1.870 |
| --- | --- |
| 0001 | 0001 |
| 0002 | 0002 |
| 0003 | 0003 |
| 0004 | 0004 |
| 0005 | 0005 |
| 0006 | 0006 |
| 0007 | 0007 |
| 0008 | 0008 |
| 0009 | 0009 |
| 0010 | 0010 |
| 0011 | 0011 |
| 0012 | not in 1.870 |
| 0013 | 0012 |
| 0014 | 0013 |
| 0015 | 0014 |
| 0016 | 0015 |
| 0017 | 0016 |
| 0018 | 0017 |
| 0019 | 0018 |
| 0020 | 0019 |
| 0021 | 0020 |
| 0022 | 0021 |
| 0023 | 0022 |
| 0024 | 0023 |
| 0025 | 0024 |
| 0026 | 0025 |
| 0027 | 0026 |
| 0028 | 0027 |
| 0029 | 0028 |
| 0030 | 0029 |
| 0031 | 0030 |
| 0032 | 0031 |
| 0033 | 0032 |
| 0034 | 0033 |
| 0035 | 0034 |
| 0036 | 0035 |
| 0037 | 0036 |
| 0038 | 0037 |
| 0039 | 0038 |
| 0040 | 0039 |
| 0041 | 0040 |
| 0042 | 0041 |
| 0043 | 0042 |
| 0044 | 0043 |
| 0045 | 0044 |
| 0046 | 0045 |
| 0047 | 0046 |
| 0048 | 0047 |
| 0049 | 0048 |
| 0050 | 0049 |
| 0051 | 0050 |
| 0052 | 0051 |
| 0053 | 0052 |
| 0054 | 0053 |
| 0055 | 0054 |
| 0056 | 0055 |
| 0057 | 0056 |
| 0058 | 0057 |
| 0059 | 0058 |
| 0060 | 0059 |
| 0061 | 0060 |
| 0062 | 0061 |
| 0063 | 0062 |
| 0064 | 0063 |
| 0065 | 0064 |
| 0066 | 0065 |
| 0067 | 0066 |
| 0068 | 0067 |
| 0069 | 0068 |
| 0070 | 0069 |
| 0071 | 0070 |
| 0072 | 0071 |
| 0073 | 0072 |
| 0074 | 0073 |
| 0075 | 0074 |
| 0076 | 0075 |
| 0077 | 0076 |
| 0078 | 0077 |
| 0079 | 0078 |
| 0080 | 0079 |
| 0081 | 0080 |
| 0082 | 0081 |
| 0083 | 0082 |
| 0084 | 0083 |
| 0085 | 0084 |
| 0086 | 0085 |
| 0087 | 0086 |
| 0088 | 0087 |
| 0089 | 0088 |
| 0090 | 0089 |
| 0091 | 0090 |
| 0092 | 0091 |
| 0093 | 0092 |
| 0094 | 0093 |
| 0095 | 0094 |
| 0096 | 0095 |
| 0097 | 0096 |
| 0098 | 0097 |
| 0099 | 0098 |
| 0100 | 0099 |
| 0101 | 0100 |
| 0102 | 0101 |
| 0103 | 0102 |
| 0104 | 0103 |
| 0105 | 0104 |
| 0106 | 0105 |
| 0107 | 0106 |
| 0108 | 0107 |
| 0109 | 0108 |
| 0110 | 0109 |
| 0111 | 0110 |
| 0112 | 0111 |
| 0113 | 0112 |
| 0114 | 0113 |
| 0115 | 0114 |
| 0116 | 0115 |
| 0117 | 0116 |
| 0118 | 0117 |
| 0119 | 0118 |
| 0120 | 0119 |
| 0121 | 0120 |
| 0122 | 0121 |
| 0123 | 0122 |
| 0124 | 0123 |
| 0125 | not in 1.870 |
| 0126 | 0124 |
| 0127 | 0125 |
| 0128 | 0126 |
| 0129 | 0127 |
| 0130 | 0128 |
| 0131 | 0129 |
| 0132 | 0130 |
| 0133 | 0131 |
| 0134 | 0132 |
| 0135 | 0133 |
| 0136 | 0134 |
| 0137 | 0135 |
| 0138 | 0136 |
| 0139 | 0137 |
| 0140 | 0138 |
| 0141 | 0139 |
| 0142 | 0140 |
| 0143 | 0141 |
| 0144 | 0142 |
| 0145 | 0143 |
| 0146 | 0144 |
| 0147 | 0145 |
| 0148 | 0146 |
| 0149 | 0147 |
| 0150 | 0148 |
| 0151 | 0149 |
| 0152 | 0150 |
| 0153 | 0151 |
| 0154 | 0152 |
| 0155 | 0153 |
| 0156 | 0154 |
| 0157 | 0155 |
| 0158 | 0156 |
| 0159 | 0157 |
| 0160 | 0158 |
| 0161 | 0159 |
| 0162 | 0160 |
| 0163 | 0161 |
| 0164 | 0162 |
| 0165 | 0163 |
| 0166 | 0164 |
| 0167 | 0165 |
| 0168 | 0166 |
| 0169 | 0167 |
| 0170 | 0168 |
| 0171 | 0169 |
| 0172 | 0170 |
| 0173 | 0171 |
| 0174 | 0172 |
| 0175 | 0173 |
| 0176 | 0174 |
| 0177 | 0175 |
| 0178 | 0176 |
| 0179 | 0177 |
| 0180 | 0178 |
| 0181 | 0179 |
| 0182 | 0180 |
| 0183 | 0181 |
| 0184 | not in 1.870 |
| 0185 | 0182 |
| 0186 | 0183 |
| 0187 | 0184 |
| 0188 | 0185 |
| 0189 | 0186 |
| 0190 | 0187 |
| 0191 | 0188 |
| 0192 | 0189 |
| 0193 | 0190 |
| 0194 | 0191 |
| 0195 | 0192 |
| 0196 | 0193 |
| 0197 | 0194 |
| 0198 | 0195 |
| 0199 | 0196 |
| 0200 | 0197 |
| 0201 | 0198 |
| 0202 | 0199 |
| 0203 | 0200 |
| 0204 | 0201 |
| 0205 | 0202 |
| 0206 | 0203 |
| 0207 | 0204 |
| 0208 | 0205 |
| 0209 | 0206 |
| 0210 | not in 1.870 |
| 0211 | 0207 |
| 0212 | 0208 |
| 0213 | 0209 |
| 0214 | 0210 |
| 0215 | 0211 |
| 0216 | 0212 |
| 0217 | 0213 |
| 0218 | 0214 |
| 0219 | 0215 |
| 0220 | 0216 |
| 0221 | 0217 |
| 0222 | 0218 |
| 0223 | 0219 |
| 0224 | 0220 |
| 0225 | 0221 |
| 0226 | 0222 |
| 0227 | 0223 |
| 0228 | 0224 |
| 0229 | 0225 |
| 0230 | 0226 |
| 0231 | 0227 |
| 0232 | 0228 |
| 0233 | 0229 |
| 0234 | 0230 |
| 0235 | 0231 |
| 0236 | 0232 |
| 0237 | 0233 |
| 0238 | 0234 |
| 0239 | 0235 |
| 0240 | 0236 |
| 0241 | 0237 |
| 0242 | 0238 |
| 0243 | 0239 |
| 0244 | 0240 |
| 0245 | 0241 |
| 0246 | 0242 |
| 0247 | 0243 |
| 0248 | 0244 |
| 0249 | 0245 |
| 0250 | 0246 |
| 0251 | 0247 |
| 0252 | 0248 |
| 0253 | 0249 |
| 0254 | 0250 |
| 0255 | 0251 |
| 0256 | 0252 |
| 0257 | 0253 |
| 0258 | 0254 |
| 0259 | 0255 |
| 0260 | 0256 |
| 0261 | 0257 |
| 0262 | 0258 |
| 0263 | 0259 |
| 0264 | 0260 |
| 0265 | 0261 |
| 0266 | 0262 |
| 0267 | 0263 |
| 0268 | 0264 |
| 0269 | 0265 |
| 0270 | 0266 |
| 0271 | 0267 |
| 0272 | 0268 |
| 0273 | 0269 |
| 0274 | 0270 |
| 0275 | 0271 |
| 0276 | 0272 |
| 0277 | 0273 |
| 0278 | 0274 |
| 0279 | 0275 |
| 0280 | 0276 |
| 0281 | 0277 |
| 0282 | 0278 |
| 0283 | 0279 |
| 0284 | 0280 |
| 0285 | 0281 |
| 0286 | 0282 |
| 0287 | 0283 |
| 0288 | 0284 |
| 0289 | 0285 |
| 0290 | 0286 |
| 0291 | 0287 |
| 0292 | 0288 |
| 0293 | 0289 |
| 0294 | 0290 |
| 0295 | 0291 |
| 0296 | 0292 |
| 0297 | 0293 |
| 0298 | 0294 |
| 0299 | 0295 |
| 0300 | 0296 |
| 0301 | 0297 |
| 0302 | 0298 |
| 0303 | 0299 |
| 0304 | 0300 |
| 0305 | 0301 |
| 0306 | 0302 |
| 0307 | 0303 |
| 0308 | 0304 |
| 0309 | 0305 |
| 0310 | 0306 |
| 0311 | 0307 |
| 0312 | 0308 |
| 0313 | 0309 |
| 0314 | 0310 |
| 0315 | 0311 |
| 0316 | 0312 |
| 0317 | 0313 |
| 0318 | 0314 |
| 0319 | 0315 |
| 0320 | 0316 |
| 0321 | 0317 |
| 0322 | 0318 |
| 0323 | 0319 |
| 0324 | 0320 |
| 0325 | 0321 |
| 0326 | 0322 |
| 0327 | 0323 |
| 0328 | 0324 |
| 0329 | 0325 |
| 0330 | 0326 |
| 0331 | 0327 |
| 0332 | 0328 |
| 0333 | 0329 |
| 0334 | 0330 |
| 0335 | 0331 |
| 0336 | 0332 |
| 0337 | 0333 |
| 0338 | 0334 |
| 0339 | 0335 |
| 0340 | 0336 |
| 0341 | 0337 |
| 0342 | 0338 |
| 0343 | 0339 |
| 0344 | 0340 |
| 0345 | 0341 |
| 0346 | 0342 |
| 0347 | 0343 |
| 0348 | 0344 |
| 0349 | 0345 |
| 0350 | 0346 |
| 0351 | 0347 |
| 0352 | 0348 |
| 0353 | 0349 |
| 0354 | 0350 |
| 0355 | 0351 |
| 0356 | 0352 |
| 0357 | 0353 |
| 0358 | 0354 |
| 0359 | 0355 |
| 0360 | 0356 |
| 0361 | 0357 |
| 0362 | 0358 |
| 0363 | 0359 |
| 0364 | 0360 |
| 0365 | 0361 |
| 0366 | 0362 |
| 0367 | 0363 |
| 0368 | 0364 |
| 0369 | 0365 |
| 0370 | 0366 |
| 0371 | 0367 |
| 0372 | 0368 |
| 0373 | 0369 |
| 0374 | 0370 |
| 0375 | 0371 |
| 0376 | 0372 |
| 0377 | 0373 |
| 0378 | 0374 |
| 0379 | 0375 |
| 0380 | 0376 |

## 1.883 to 1.885

1.884 takes out the two device-tree patches of 1.883, 0487 (`arm64: dts: apple: t8132-j604: write the HDMI power enables as the ADT does`) and 0489 (`arm64: dts: apple: t8132-j604: add the ATC PHY CIO calibration`), which never ran. 1.883's 0488 is 0487 since 1.884, 0490 is 0488, and 0491-0499 are 0489-0497.

New in 1.884: 0498 (`soc: apple: sleep: a T8132 bitmask to leave J713 commit-time writes out`), 0499 (`soc: apple: sleep: optional T8132 commit cleans and a dirty-line probe`), 0500 (`soc: apple: sleep: log and optionally set ACC_OVRD before a T8132 commit`), 0501 (`soc: apple: clpc: grade the J604's CPU floor by frequency`), 0502 (`drm/asahi: G16: let a compute command go behind its running predecessor`).

New in 1.885: 0503 (`soc: apple: sleep: prove the T8132 stub record path and add a read ladder`), 0504 (`soc: apple: sleep: let a T8132 sleep resume through #1314's own stub`), 0505 (`soc: apple: sleep: read the T8132 wake core's CTRR_D/CTXR_D, add go-hib`).

New in 1.887 (1.886 was not installed on its own): 0506 (`drm/asahi: G16: optionally bump the submit generation cells before a doorbell`), 0507 (`drm/asahi: G16: optionally keep up to four render commands resident`), 0508 (`drm/asahi: G16: optionally ring one doorbell per render publication`), 0509 (`soc: apple: sleep: optional SPTM-style warm entry in the T8132 stub`), 0510 (`media: apple: isp: optionally park the T8132 ISP cores as macOS does`), 0511 (`media: apple: isp: optionally write the T8132 interrupt enables macOS writes`), 0512 (`soc: apple: sleep: t8132: probe stub and a second stub config copy`), 0513 (`drm: apple: backlight: make the fade length and step period parameters`), 0514 (`pmdomain: apple: pmp-report: optional T8132 SOC-DEV-DVFS hold for a trial`), 0515 (`drm/asahi: G16: optionally make resident render units compact`).

New in 1.888: 0516 (`soc: apple: sleep: t8132: optionally arm the wake before DCS down`), 0517 (`soc: apple: mcc: log T8132's AMCC region locks at probe`).

New in 1.902 (1.889 to 1.900 were not installed on their own; 1.901 had the same patches plus a memory-controller state read at probe as its 0531, and was replaced): 0518 (`soc: apple: sleep: t8132: arm the wake at .prepare_late and check it`), 0519 (`soc: apple: sleep: t8132: optionally commit exactly as #1314 does`), 0520 (`drm/asahi: G16: keep four render commands resident by default`), 0521 (`media: apple: isp: write the T8132 interrupt enables macOS writes by default`), 0522 (`arm64: configs: asahi: make transparent huge pages the default`), 0523 (`drm/asahi: G16: rotate render commands over every resident unit`), 0524 (`drm/asahi: G16: keep eight render commands resident by default`), 0525 (`drm/asahi: G16: reuse unexposed PB managers instead of building one per command`), 0526 (`arm64: dts: apple: t8132-j604: give the AVD its PMP performance request`), 0527 (`arm64: dts: apple: t8132-j604: give the video encoder and scaler their PMP requests`), 0528 (`accel/ane: say when and why the H16 engine powers down`), 0529 (`media: apple: isp: park the T8132 ISP cores at power-down by default`), 0530 (`drm/apple: m4: run the J604's mini-LED with macOS's BLM settings`), 0531 (`drm/apple: m4: run the J604 panel's image processing as macOS does`), 0532 (`spi: apple-qspi: runtime suspend the T8132 controller`), 0533 (`drm/apple: m4: give the internal T8132 panel a colour matrix`), 0534 (`media: apple: isp: set up the T8132 ISP DARTs as macOS does at power-up`), 0535 (`media: apple: isp: name the T8132 PMP scratch registers to the firmware`), 0536 (`soc: apple: sleep: t8132: optionally stop AMCC1 at the commit too`), 0537 (`drm/apple: m4: send the J604 panel's desktop compensation values`), 0538 (`spi: apple-qspi: check the boot NOR's JEDEC ID at probe`), 0539 (`arm64: dts: apple: t8132-j604: let the nvram partition be written`), 0540 (`media: apple: isp: use macOS's T8132 auto exposure stability thresholds`).

New in 1.903: 0541 (`soc: apple: sleep: run T8132 sleep exactly as merged upstream`), 0542 (`arm64: dts: apple: t8132-j604: enable the sleep controller`), 0543 (`soc: apple: rtkit-helper: Never stack an unanswered AP idle notice`), 0544 (`dt-bindings: soc: apple: sleep: add apple,ps-keep-on`), 0545 (`soc: apple: sleep: leave the words in apple,ps-keep-on alone`), 0546 (`arm64: dts: apple: t8132-j604: keep the J713-only sleep words`), 0547 (`soc: apple: clpc: leave the J604's E cluster uncapped in low-power`), 0548 (`drm/apple: m4: keep the J604 panel in its XDR pipeline (skeleton)`), 0549 (`drm/apple: m4: hold and ramp the panel level after HDR as macOS does`), 0550 (`drm/apple: m4: use macOS's SDR words and ramp shape, XDR on by default`), 0551 (`drm/apple: m4: compose a panel white-point adaptation with the CTM`), 0552 (`ASoC: tas2764: per-amplifier Apple shutdown dance, on for the J604`), 0553 (`arm64: dts: apple: t8132-j604: JPEG engines' PMP performance request`), 0554 (`accel/ane: power the H16 engine off when idle with programs loaded`), 0555 (`accel/ane: report a quarantined H16 engine through "wedged"`), 0556 (`drm/asahi: G16: drain the GPU and publish its idle floor at suspend-to-idle`), 0557 (`media: apple: isp: send macOS's T8132 exposure and tone session settings`), 0558 (`media: apple: isp: start the IMX958 temporal filter as macOS does`), 0559 (`media: apple: isp: keep the T8132 ISP down 1.5 s between two boots`).

New in 1.905 (1.904 was not kept): 0560 (`dt-bindings: leds: pwm: Add an optional duty cycle range`), 0561 (`leds: pwm: Add an optional duty cycle range`), 0562 (`arm64: dts: apple: t8132-j604: Drive the keyboard light on macOS's scale`), 0563 (`arm64: dts: apple: t8132: Use the trivial country code map for Wi-Fi`), 0564 (`ASoC: tas2764: apple,init-writes, and the J604 amps' missing init writes`), 0565 (`ASoC: cs42l84: apple,init-writes, and the J604's macOS probe writes`), 0566 (`drm/asahi: G16: allow the firmware's idle power-off after start`), 0567 (`drm/asahi: G16: let one compute command go behind its predecessor by default`), 0568 (`arm64: dts: apple: t8132-j604: hold the scaler's PMP request while the AVD decodes`), 0569 (`arm64: dts: apple: t8132-j604: name the SIO's registers`).

New in 1.907 (1.906 did not start and was not kept): 0570 (`soc: apple: sleep: settle the woken core before the T8132 resume`), 0571 (`drm/asahi: G16: keep the firmware's power-assert tally so idle cores can power off`), 0572 (`wifi: brcmfmac: Say which country the firmware runs`), 0573 (`media: apple: isp: boot the T8132 firmware again when a boot fails`).

New in 1.908: 0574 (`soc: apple: sleep: give the T8132 resume stub its own vectors`), 0575 (`drm/asahi: G16: let up to three compute commands queue behind a running one`), 0576 (`media: apple: isp: power the T8132 ISP down as macOS does`).

New in 1.909: 0577 (`soc: apple: name the section a stalled boot stopped in`), 0578 (`iommu: apple-dart: leave a powered-down DCP DART alone in its IRQ handler`), 0579 (`soc: apple: sleep: tell the woken core's faults apart by time`), 0580 (`media: apple: isp: keep the detecting firmware up into the first stream`).

New in 1.910: 0581 (`soc: apple: sleep: mark the T8132 wake's progress in WD2`), 0582 (`drm/asahi: G16: import Globals again after the power-asserting controls`), 0583 (`drm/asahi: G16: keep up to seven compute commands queued behind a running one`), 0584 (`media: apple: isp: set the T8132 white balance colour space as macOS does`), 0585 (`iio: aop_motion: run the reports only while someone reads them`), 0586 (`ASoC: cs42l84: detect the jack on ring sense under macOS's probe sequence`), 0587 (`soc: apple: clpc: hold an all-core-busy J604 cluster where macOS does`).

New in 1.913: 0588 (`thermal: apple-pmp: poll the raw sensor every 50 ms while cool`), 0589 (`media: apple: isp: drop the T8132 first-open workarounds`), 0590 (`arm64: dts: apple: t8132-j604: publish the SMC's power channels`), 0591 (`soc: apple: sleep: mark __cpu_setup on the T8132 wake, clear OS lock`), 0592 (`ASoC: cs42l84: acknowledge the remote status latch under macOS's probe sequence`), 0593 (`drm/asahi: G16: admit twice as many jobs per queue as it has units`).

New in 1.914: 0594 (`arm64: dts: apple: t8132-j604: write the HDMI converter power the ADT way`), 0595 (`drm/asahi: G16: count doorbells in the Globals tally as macOS does`).

New in 1.915: 0596 (`soc: apple: sleep: mark the T8132 wake up to init_kernel_el`), 0597 (`pmdomain: apple: pmp-report: T8132 memory bandwidth vote`).

New in 1.916: 0598 (`soc: apple: keep the boot crumbs in PMU scratch that holds them`), 0599 (`drm/apple: keep an idle external DCP powered by default`), 0600 (`drm/asahi: G16: publish the performance selectors macOS publishes`), 0601 (`arm64: dts: apple: t8132-j604: add the ATC PHY CIO calibration`).

New in 1.917: 0602 (`soc: apple: sleep: T8132 wake variants as runtime parameters`), 0603 (`dt-bindings: power: apple,t8140-pmp-report: Add apple,bw-vote`), 0604 (`pmdomain: apple: pmp-report: T8132 bandwidth vote from powered devices`), 0605 (`arm64: dts: apple: t8132-j604: AVD and VENC bandwidth votes`), 0606 (`ASoC: tas2764: keep the TDM clock error masked on dancing amplifiers`), 0607 (`media: apple: avd: don't overrun the SRAM ring at the counter wrap`), 0608 (`drm/asahi: G16: write the two Globals words macOS sets after init`), 0609 (`usb: dwc3: apple: allow SuperSpeed Plus on T8132 ports by default`), 0610 (`media: apple: isp: send macOS's T8132 colour pipeline settings`), 0611 (`media: apple: isp: add switches for macOS's SIF format and MBNR chroma`).

New in 1.918: 0612 (`soc: apple: sleep: mark the T8132 way into a deep sleep in WD2`). New in 1.919: 0613 (`accel/ane: keep an idle H16 engine powered by default`), 0614 (`soc: apple: keep the boot crumbs where a watchdog reset leaves them`), 0615 (`soc: apple: sleep: T8132 PMU SPMI wake variants as runtime parameters`), 0616 (`soc: apple: rtkit: send the AP idle notice with interrupts off`), 0617 (`soc: apple: sleep: optionally send the SMC's sleep notice from .enter`), 0618 (`mfd: macsmc: send the sleep notice from .enter when asked`).

New in 1.920: 0619 (`soc: apple: sleep: T8132: don't read a powered-down SIO in the gate check`), 0620 (`soc: apple: sleep: T8132 display power-off variants as runtime parameters`), 0621 (`Revert "accel/ane: keep an idle H16 engine powered by default"`), 0622 (`media: apple: avd: hevc: test switch for the packed 10-bit output mode`), 0623 (`drm/asahi: G16: leave the performance selectors to the host governor by default`), 0624 (`drm/asahi: G16: put the Globals words macOS has at INIT in place before INIT`), 0625 (`Revert "arm64: dts: apple: t8132-j604: AVD and VENC bandwidth votes"`), 0626 (`soc: apple: record what a boot died of where a watchdog reset leaves it`).

New in 1.921: 0627 (`dmaengine: apple-sio: keep the domain up until the sleep's commit stop`), 0628 (`pmdomain: apple: pmgr-pwrstate: hold a domain off at probe for a wakeup path`), 0629 (`soc: apple: sleep: T8132: power the displays back after a sleep`), 0630 (`media: apple: avd: test switch for a gap before the reference area`), 0631 (`arm64: dts: apple: t8132-j604: publish the SMC's voltage and current channels`), 0632 (`arm64: dts: apple: t8132-j604: publish the SMC's other temperature channels`).

New in 1.922: 0633 (`PCI: apple: bring the J604's ports and DARTs back after a deep sleep`), 0634 (`soc: apple: sleep: clock back the PCIe ports no host restores after a wake`), 0635 (`soc: apple: name what an SError hit in the panic record`), 0636 (`HID: apple-mtp-haptic: let the click strength be set at run time`), 0637 (`media: apple: isp: add a switch for macOS's AE integration table`), 0638 (`drm/asahi: G16: put the render queues on scheduling row 0, as macOS does`), 0639 (`drm/asahi: G16: lower the Globals max power to macOS's post-INIT value after READY`).

New in 1.923: 0640 (`PCI: apple: restore each J604 DART behind its own port after a deep sleep`), 0641 (`soc: apple: sleep: T8132: stop the SD card reader's port at the commit`), 0642 (`media: apple: isp: add a switch for macOS's back-end LA setting`), 0643 (`iommu: apple-dart: let a sleep leave idle DARTs to their runtime resume`), 0644 (`iommu: apple-dart: leave a DART no device uses alone after a deep sleep`), 0645 (`soc: apple: sleep: name the power words a wake found off`).

New in 1.926: 0646 (`soc: apple: rtkit-helper: say so when the MTP lost its CPU in a deep sleep`), 0647 (`PCI: apple: put the SD reader's root port back when the reader powers up`), 0648 (`PCI: apple: accept an MSI affinity the AIC can't steer`), 0649 (`soc: apple: sleep: arm extra wake sources from a runtime list`), 0650 (`media: apple: isp: add a switch for macOS's MBNR configuration`), 0651 (`drm/asahi: G16: keep the render queues on row 2 by default`), 0652 (`PCI: apple: hold the SD reader port's DART while the reader is off`), 0653 (`power: reset: macsmc: keep the panic record until a boot has run 30 s`).

New in 1.928: 0654 (`iommu/apple-dart: don't fail a gated DCP's power-up on a stream the wake set up`), 0655 (`soc: apple: print the CPU range locks at boot and after each sleep`), 0656 (`soc: apple: cpu-locks: survive registers a core lacks; boot lines off`), 0657 (`pmdomain: apple: pmp-report: hold the T8132 GFX power request as macOS does`), 0658 (`pmdomain: apple: pmp-report: T8132 GPU system-cache quota experiment`), 0659 (`thunderbolt: apple: offer the attached-dock sleep owner on the J604 too`), 0660 (`media: apple: isp: add a switch for macOS's camera calibration reads`), 0661 (`media: apple: isp: add a switch for macOS's face and body detection`), 0662 (`media: apple: isp: let another setfile be loaded at run time`), 0663 (`media: apple: isp: ML noise reduction on the neural engine (draft)`), 0664 (`soc: apple: ane-isp: build the link only where the ANE driver provides it`), 0665 (`media: apple: isp: configure ML noise reduction as macOS does on T8132`), 0666 (`pmdomain: apple: pmc: add a runtime floor under the AP's DCS vote`).

New in 1.929: 0667 (`HID: dockchannel: let keyboard input and trackpad clicks wake the system`), 0668 (`drm/asahi: g16: g16_render_patch, macOS render sequence words at runtime`), 0669 (`drm/asahi: g16: retire from a SCHED_FIFO poller after a firmware event`), 0670 (`media: apple: isp: add a digital zoom control`), 0671 (`media: v4l2: Add the P030 pixel format`), 0672 (`media: apple: avd: Decode 10-bit HEVC into P030 on request`), 0673 (`media: apple: avd: Test switches for the 10-bit HEVC output words`), 0674 (`dt-bindings: media: apple,prores: Name the T8132 initialization table`), 0675 (`media: apple: prores: Add the T8132 (M4) engine`), 0676 (`media: apple: prores: Bind the T8132 engine only on request`), 0677 (`iommu/apple-dart: after a deep sleep, put back the streams no table of ours used`), 0678 (`ASoC: tas2764: Bring an SDZ group back from a deep sleep as from power-on`), 0679 (`arm64: dts: apple: t8132: Name the ProRes windows; enable ProRes on the J604`).


# Kernel patch numbers: 1.930 to 1.932

Kernel 1.932 keeps the base (`da8e1acf3656`) and patches 0001-0653 of 1.930. Two 1.930 patches left the series: 0654 (`iommu/apple-dart: don't fail a gated DCP's power-up on a stream the wake set up`) and 0677 (`iommu/apple-dart: after a deep sleep, put back the streams no table of ours used`); 1.932's 0678 (`iommu: apple-dart: keep a gated DCP's power-up after a deep sleep`) replaces both. 1.930's other patches from 0655 on move up to close the gaps, and patches 0678-0693 are new in 1.932. `STATUS.md` and `UPSTREAM.md` keep naming patches by the number they had when they were added; this table maps the 1.930 files from 0654 on to the 1.932 files.

| 1.930 | 1.932 |
| --- | --- |
| 0654 | not in 1.932 |
| 0655 | 0654 |
| 0656 | 0655 |
| 0657 | 0656 |
| 0658 | 0657 |
| 0659 | 0658 |
| 0660 | 0659 |
| 0661 | 0660 |
| 0662 | 0661 |
| 0663 | 0662 |
| 0664 | 0663 |
| 0665 | 0664 |
| 0666 | 0665 |
| 0667 | 0666 |
| 0668 | 0667 |
| 0669 | 0668 |
| 0670 | 0669 |
| 0671 | 0670 |
| 0672 | 0671 |
| 0673 | 0672 |
| 0674 | 0673 |
| 0675 | 0674 |
| 0676 | 0675 |
| 0677 | not in 1.932 |
| 0678 | 0676 |
| 0679 | 0677 |

New in 1.932: 0678 (`iommu: apple-dart: keep a gated DCP's power-up after a deep sleep`), 0679 (`ASoC: tas2764: Detect a power-on reset of an SDZ group amp at resume`), 0680 (`soc: apple: sleep: optionally sleep one more SPMI slave on the debug port's bus`), 0681 (`media: apple: isp: keep a debugfs copy of the last frame's metadata`), 0682 (`media: apple: isp: restart a T8132 stream after sleep from a work item`), 0683 (`media: apple: isp: make the rest of macOS's module reads at open`), 0684 (`media: apple: isp: add an auto framing control on the digital zoom`), 0685 (`media: apple: isp: let a debugfs face box drive auto framing`), 0686 (`media: apple: isp: read the sensor NVM once a sensor config is selected`), 0687 (`media: apple: isp: optionally keep the firmware running after a stream`), 0688 (`drm/asahi: g16: Optionally idle the firmware's RTKit at suspend-to-idle`), 0689 (`dt-bindings: soc: apple: sleep: describe power words a machine lacks`), 0690 (`soc: apple: sleep: leave alone the power words a machine lacks`), 0691 (`arm64: dts: apple: t8132-j604: name the power words the J604 lacks`), 0692 (`media: apple: prores: Bind the T8132 engine by default`), 0693 (`drm/asahi: g16: Count why a compute command is not posted ahead`).


# Kernel patch numbers: 1.932 to 1.934

Kernel 1.934 keeps the base (`da8e1acf3656`) and all 693 patches of 1.932 under the same numbers.

New in 1.934: 0694 (`drm: apple: t8132: Send nothing to a power-gated DCP from the colour paths`), 0695 (`drm/asahi: g16: Optionally idle the firmware's RTKit when the GPU is idle`), 0696 (`drm/asahi: g16: Count why a render command is not posted ahead`), 0697 (`drm/asahi: g16: allow up to 12 compute units per VM`), 0698 (`drm/asahi: g16: Default to 12 compute units per VM`), 0699 (`media: apple: isp: read the module's lens and colour data after the config select`), 0700 (`media: apple: isp: add a test floor on the auto exposure gain`), 0701 (`media: apple: isp: send the zoom as a float when the firmware reports no factor`), 0702 (`media: apple: isp: optionally start the firmware when the camera is opened`), 0703 (`media: apple: isp: offer a Desk View format on T8132`), 0704 (`ASoC: cs42l84: Restore the codec and the jack state after system sleep`), 0705 (`drm: apple: t8132: Offer VRR on external outputs behind a switch`), 0706 (`soc: apple: pmp: log the values the firmware asks for, behind a parameter`).


# Kernel patch numbers: 1.934 to 1.937

Kernel 1.937 keeps the base (`da8e1acf3656`) and all 706 patches of 1.934 under the same numbers. 1.935 was not kept (it did not pass its install check) and 1.936, the same without 0722-0729, was not installed; their numbers are not used here.

New in 1.937: 0707 (`drm: apple: t8132: Power a gated DCP up before a timing change`), 0708 (`drm: apple: t8132: Refuse a call to a powered-down DCP without failing the session`), 0709 (`Revert "ASoC: cs42l84: Restore the codec and the jack state after system sleep"`), 0710 (`ASoC: cs42l84: Bring the codec back after a firmware sleep`), 0711 (`media: apple: avd: refuse VP9 and AV1 streams other than 4:2:0`), 0712 (`media: apple: avd: refuse HEVC 4:4:4`), 0713 (`media: apple: avd: re-check VP9 and AV1 stream parameters when a frame runs`), 0714 (`drm/asahi: g16: Log, and optionally restate, the power-assert cell at a real wake`), 0715 (`media: apple: isp: make macOS's module reads in its per-session order`), 0716 (`media: apple: isp: offer macOS's highlight recovery setup on T8132`), 0717 (`media: apple: isp: offer macOS's local thumbnail raw buffers on T8132`), 0718 (`media: apple: isp: offer macOS's two camera controls on T8132`), 0719 (`media: apple: isp: offer macOS's manual noise and detail knobs on T8132`), 0720 (`media: apple: isp: offer four more of macOS's stream settings on T8132`), 0721 (`media: apple: isp: test the macOS module reads, setfile override and switches`), 0722 (`drm/asahi: g16: Drop the runtime and suspend-to-idle RTKit idles`), 0723 (`media: apple: prores: Add the T8132 built-in initialization rows`), 0724 (`ASoC: apple: aop: Stop the PDM microphones across a firmware sleep`), 0725 (`media: apple: isp: reserve a control base and document the controls`), 0726 (`dt-bindings: npu: apple,ane: make the binding parse and validate again`), 0727 (`media: apple: isp: offer 1080p60 from the IMX958's 60 fps mode on T8132`), 0728 (`thunderbolt: apple: Optionally allow CL states on T8132`), 0729 (`thunderbolt: apple: Optionally wake from suspend-to-idle on a plug event`).


# Kernel patch numbers: 1.937 to 1.938

Kernel 1.938 keeps the base (`da8e1acf3656`) and all 729 patches of 1.937 under the same numbers.

New in 1.938: 0730 (`sched/fair: Treat a task that runs without sleeping on a smaller CPU as misfit`), 0731 (`sched/fair: Push a runtime misfit to an idle bigger CPU from the tick`), 0732 (`media: apple: avd: AV1 test switches for the 10-bit output and motion-field release`), 0733 (`drm: apple: t8132: Bring a quarantined display session back without a reboot`), 0734 (`drm: apple: t8132: Let the fault injector test the powered-down call net`), 0735 (`drm: apple: t8132: Re-arm the receive wait when a quarantined session comes back`), 0736 (`drm: apple: dptxep: Optionally wait for the Type-C PHY's DP mode before the lane count`).


# Kernel patch numbers: 1.938 to 1.939

Kernel 1.939 keeps the base (`da8e1acf3656`) and all 736 patches of 1.938 under the same numbers.

New in 1.939: 0737 (`soc: apple: clpc: let the all-core ceiling be set at run time`), 0738 (`soc: apple: clpc: optionally hold an idle cluster at its slowest state`), 0739 (`media: apple: avd: HEVC test switch for the 10-bit packed-format scratch`), 0740 (`drm/asahi: g16: Count why commands wait on their dependencies`), 0741 (`drm/asahi: g16: Latch the render units apart from the resident depth`), 0742 (`drm/asahi: g16: Optionally revive a fail-stopped GPU at the next real wake`), 0743 (`sched/features: Enable MISFIT_RUNTIME`).


# Kernel patch numbers: 1.939 to 1.940

Kernel 1.940 keeps the base (`da8e1acf3656`) and all 743 patches of 1.939 under the same numbers.

New in 1.940: 0744 (`hwmon: macsmc: Print voltage and current keys in the debugfs key list`), 0745 (`media: apple: isp: keep the camera firmware up 5 s after a stream`), 0746 (`media: apple: isp: offer 1080p60 by default on T8132`).


# Kernel patch numbers: 1.940 to 1.942

Kernel 1.942 keeps the base (`da8e1acf3656`) and all 746 patches of 1.940 under the same numbers. 1.941 was tried for one start and not kept, so it is not published.

New in 1.942: 0747 (`sched/fair: Optionally place a new task on an idle CPU of the highest capacity`), 0748 (`drm/asahi: g16: Optionally release the AGX power vote at the PMP`), 0749 (`media: apple: avd: av1: Test switch to read the reference's CDFs in place`), 0750 (`drm/asahi: g16: Keep up to 16 render commands resident by default`).


# Kernel patch numbers: 1.942 to 1.943

Kernel 1.943 keeps the base (`da8e1acf3656`) and all 750 patches of 1.942 under the same numbers; 0751 reverts 0750.

New in 1.943: 0751 (`Revert "drm/asahi: g16: Keep up to 16 render commands resident by default"`), 0752 (`drm/asahi: g16: Stop a fail-stopped firmware at the commit before a revive`).


# Kernel patch numbers: 1.943 to 1.945

Kernel 1.945 keeps the base (`da8e1acf3656`) and all 752 patches of 1.943 under the same numbers. 1.944 was started once as an experiment and not kept, so it is not published.

New in 1.945 (all off by default): 0753 (`sched/fair: FORK_BIG only while the machine is narrow; optional start lift`), 0754 (`pmdomain: apple: pmp-report: let t8132_gfx_request change at run time`), 0755 (`drm/asahi: g16: Optionally keep the device after a recovery's receipt fault`), 0756 (`drm/asahi: g16: Keep a recovered job's later receipts off the full chain`), 0757 (`media: apple: isp: zoom by the stream crop where the firmware has no zoom factor`).


# Kernel patch numbers: 1.945 to 1.946

Kernel 1.946 keeps the base (`da8e1acf3656`) and all 757 patches of 1.945 under the same numbers.

New in 1.946 (off by default): 0758 (`sched/fair: No FORK_BIG placement or start lift for a fork burst`).


# Kernel patch numbers: 1.946 to 1.947

Kernel 1.947 keeps the base (`da8e1acf3656`) and all 758 patches of 1.946 under the same numbers.

New in 1.947 (0765 is on by default and uses 0762; the other five are off by default): 0759 (`pmdomain: apple: pmp-report: reach a T8132 report the PMP was not running for`), 0760 (`cpufreq: schedutil: Add down_hold_us to keep a frequency through short dips`), 0761 (`drm/asahi: g16: Optionally publish an idle performance selector`), 0762 (`wifi: brcmfmac: Let a chip set the default PM2 sleep return`), 0763 (`drm/asahi: g16: Add a test switch that injects a post-recovery receipt fault`), 0764 (`media: apple: ave: let the client choose the SPS frame_num and POC words`), 0765 (`wifi: brcmfmac: Nap after 20 ms on the BCM4388`).


# Kernel patch numbers: 1.947 to 1.985

Kernel 1.985 keeps the base (`da8e1acf3656`) and all 765 patches of 1.947 under the same numbers.

1.948 and 1.982 did not boot and are not published; 1.949, 1.983 and 1.984 were never installed. New in 1.985 (0784 is on by default; 0785 changes no behaviour; the other eighteen are off by default): 0766 (`soc: apple: clpc: Optionally cap the E cluster while P cores are busy`), 0767 (`media: apple: isp: set the zoom crop at stream start and keep it within the output`), 0768 (`drm/asahi: g16: Count how deep each queue is when a job arrives`), 0769 (`drm/asahi: g16: Optionally notify the host only for renders to an idle pair`), 0770 (`drm/asahi: g16: Advance the queue ordinal past a recovered job's commands`), 0771 (`media: apple: ave: let the client choose the PPS QP, deblocking and scaling words`), 0772 (`sched/fair: Move a new CPU-bound task to a big CPU after a short first run`), 0773 (`wifi: brcmfmac: pcie: Optionally reset the chip at shutdown`), 0774 (`media: apple: isp: optionally enable macOS's per-frame metadata sections`), 0775 (`drm/asahi: g16: Optionally boost the submit path while work is in flight`), 0776 (`media: apple: isp: optionally send macOS's video HDR settings`), 0777 (`drm/apple: dcp: Optionally power the M4 panel down at shutdown`), 0778 (`drm/asahi: g16: Allow up to 16 compute units`), 0779 (`drm/asahi: g16: Optionally clear the FS completion event`), 0780 (`drm/asahi: g16: Time the rest of the poll pass`), 0781 (`drm/asahi: g16: Optionally check the session once per poll pass`), 0782 (`drm/asahi: g16: Optionally let a job's later compute commands go ahead`), 0783 (`media: apple: avd: program the T8132 AXI2AF bridge as macOS does (test switch)`), 0784 (`pmdomain: apple: pmp-report: release the T8132 GFX power request at shutdown`), 0785 (`drm/asahi: g16: Keep the engine statistics off the probe stack`).


# Kernel patch numbers: 1.985 to 1.992

Kernel 1.992 keeps the base (`da8e1acf3656`) and all 785 patches of 1.985 under the same numbers.

1.988 carried the same patches and was never installed (a packaging fault, not a kernel one; see STATUS.md). New in 1.992 (0792 is on by default; the other six are off by default or read-only): 0786 (`drm/asahi: g16: Optionally report GPU work to the CLPC`), 0787 (`drm/asahi: g16: Show the firmware's own power state in debugfs`), 0788 (`drm/asahi: g16: Optionally read the event ring first in a poll pass`), 0789 (`drm/asahi: g16: Count the running performance state while work is in flight`), 0790 (`cpufreq: schedutil: Let the driver set where down_hold_us starts`), 0791 (`cpufreq: apple-soc: Optionally set schedutil's starting values`), 0792 (`drm/asahi: g16: Check the session once per poll pass by default`).


# Kernel patch numbers: 1.992 to 1.990

Kernel 1.990 keeps the base (`da8e1acf3656`) and all 792 patches of 1.992 under the same numbers.

1.990 was planned before 1.992 and keeps its number; it carries all of 1.992. New in 1.990 (0796 and 0797 are on by default; the other three are off by default or read-only): 0793 (`drm/asahi: g16: Count compute commands that overlap their queue predecessor`), 0794 (`drm/asahi: g16: Optionally share one UMA pool among a VM's compute units`), 0795 (`nvme-apple: optionally wait longer for the final shutdown notification`), 0796 (`drm/asahi: g16: Boost the submit path by default`), 0797 (`drm/asahi: g16: Keep up to 16 compute commands in flight by default`).
