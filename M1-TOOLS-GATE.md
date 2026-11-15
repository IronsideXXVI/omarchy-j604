# Host tools first: the M1 tools gate

**The order of work since 2026-11-15 18:33 UTC (the owner's decision).** Before the work toward macOS parity takes the Mac for anything on the macOS side again, the tools on the host must work as well as they did on the previous host: easy, repeatable and correct. Those tools let us flip the J604 between Omarchy and macOS (macOS runs as a guest under the hypervisor), bring it back, trace and probe the hardware, and capture what macOS does. The host is the owner's M1 Mac running Linux, connected over USB. It replaced the MacBook Air used until 2026-11-14.

Until every row below reads PASS, the macOS halves of the measurements are not taken again. The numbers we compare against stay the ones captured earlier. In particular, the macOS side of the CPU power row and of the GPU rows is not re-measured, and the macOS run without the "use the performance cores" hint (needed for the strict RAM figure) is not captured yet. The Linux side is still measured after every kernel.

Rules: each row has one command that runs it and one line it prints. A row is PASS only on a measured run, never on reading code. "In a row" means consecutive runs with no fix in between; a fix resets the count. Nothing may need hands at the machine.

## Score (MEASURED; as scored at 21:20 UTC, after the third session and two gentle round-trip attempts)

G0 comes first: until five gentle round trips pass in a row, the Mac boots into macOS for nothing else.

| # | What it tests | PASS means | Now |
|---|---|---|---|
| G0 | A gentle round trip | 5 round trips in a row, Omarchy to the hypervisor's first boot stage and back to Omarchy, with only the return's own small guest, no forced resets and no capture boot | **Open, 0 of 5.** The first trip tonight (21:02 UTC) is not counted: the tool stopped on a false alarm while the Mac was fine, and the return from where it stopped took 126 s with no reset. The second trip (21:19 UTC) failed: after a clean restart the first boot stage came up silent (the second time today), so the series stopped and one forced restart brought the Mac back. |
| G1 | The USB link moves data intact | 20 reads of 64 MiB bit-exact, no stall left unrecovered, 3 sessions in a row | **Open, 0 of 3.** Session 2: the first 64 MiB block was bit-exact, the second froze after about 21 MiB. Session 1 gave no verdict. A fix (a lost USB completion in the boot loader, plus a detector for a stuck transfer) waits for a session that reaches the link test; session 3 stopped before it. |
| G2 | The link under a real capture | one capture boot with the largest readout: every chunk checksum good, 3 in a row | Open, not run yet |
| G3 | Flip Omarchy to macOS (guest up and answering) | 5 in a row, each under 6 minutes, no hands | **Open, 0 of 5.** Session 3 (19:52 UTC) failed: the first boot stage came up silent and the tool stopped, so the earlier pass (2 min 22 s) no longer counts toward the 5 in a row. |
| G4 | Return macOS to Omarchy (Linux unlocked and lit after it) | 5 in a row, each under 6 minutes | **Open, 0 of 5.** Session 3's return did not bring Linux back, and the Mac needed a restart by hand at 20:29 UTC; the earlier pass (5 min 41 s) no longer counts. |
| G5 | Return when the first boot stage stalls | the return still completes without hands | PASS, 1 of 1: a real silent first stage, recovered |
| G6 | Hand-back from any phase | the stop word brings Linux back in under 6 minutes from any phase | Open, dry-tested only |
| G7 | A bad start is refused | a failed pre-check keeps the Mac on Omarchy | PASS, 1 of 1: an input broken on purpose, start refused, no firmware variable written |
| G8 | Tracing is correct | a trace of a known boot on the M1 host equals the earlier capture of the same boot | Open: the registers written and their constant values match the earlier capture (8 of 8 and 12 of 12), but that capture was cut short by a link drop |
| G9 | Probing is correct | register, device-tree and PMU reads through the host equal Linux's own reads | Open: Linux half read, host half not run |
| G10 | Capture readers decode | each reader the work uses decodes a capture from the new host with 0 errors | Open: no complete GPU capture from the new host exists yet |
| G11 | A whole list | a list of 4 capture boots runs end to end, every verdict present, return clean | Open, not run |
| G12 | Parity with the previous host | each thing the previous host could do, shown on the new one | Open: 6 of 18 shown |
| G13 | Ease | one command to go, one to come back, one to ask where the Mac is, documented in under a page | Open: the commands exist, the page is drafted, not published yet |
| G14 | No stale state | after a failed session the next start needs no manual clean-up | 2 of 3 |
| G15 | No forced reset anywhere in the tools | (A) no reset or proxy-restart call left in the host scripts outside a short reviewed list; (B) in each session, 0 resets by the tools and 0 abrupt boot ends | **Open, 0 of 1.** 38 such calls were found at 20:58 UTC; a list of 13 reviewed exceptions was accepted at 21:13 UTC, and the rest are being removed. |
| G16 | Flip time | the M1 round trip (to macOS and back, with its own guest) at or under the previous host's, on counted G0 trips, with no forced resets | Measured, not yet on a counted G0 trip: about 230 s on the M1 against about 532 s on the previous host (to macOS 92 s against 96 s; back 138 s median against 436 s, which included a login by hand). |

This file is updated when the gate's score changes.
