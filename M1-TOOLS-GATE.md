# Host tools first: the M1 tools gate

**The order of work since 2026-11-15 18:33 UTC (the owner's decision).** Before the work toward macOS parity takes the Mac for anything on the macOS side again, the tools on the host must work as well as they did on the previous host: easy, repeatable and correct. Those tools let us flip the J604 between Omarchy and macOS (macOS runs as a guest under the hypervisor), bring it back, trace and probe the hardware, and capture what macOS does. The host is the owner's M1 Mac running Linux, connected over USB. It replaced the MacBook Air used until 2026-11-14.

Until every row below reads PASS, the macOS halves of the measurements are not taken again. The numbers we compare against stay the ones captured earlier. In particular, the macOS side of the CPU power row and of the GPU rows is not re-measured, and the macOS run without the "use the performance cores" hint (needed for the strict RAM figure) is not captured yet. The Linux side is still measured after every kernel.

Rules: each row has one command that runs it and one line it prints. A row is PASS only on a measured run, never on reading code. "In a row" means consecutive runs with no fix in between; a fix resets the count. Nothing may need hands at the machine.

## Score (MEASURED; as scored after the second session, 19:35 UTC; a third session is running)

| # | What it tests | PASS means | Now |
|---|---|---|---|
| G1 | The USB link moves data intact | 20 reads of 64 MiB bit-exact, no stall left unrecovered, 3 sessions in a row | **Open, 0 of 3.** Session 2: the first 64 MiB block was bit-exact, the second froze after about 21 MiB. Session 1 gave no verdict. A fix (a lost USB completion in the boot loader, plus a detector for a stuck transfer) is under test in session 3. |
| G2 | The link under a real capture | one capture boot with the largest readout: every chunk checksum good, 3 in a row | Open, not run yet |
| G3 | Flip Omarchy to macOS (guest up and answering) | 5 in a row, each under 6 minutes, no hands | **1 of 5:** 2 min 22 s. Session 1 took 7.6 min (over the bar, not counted). |
| G4 | Return macOS to Omarchy (Linux unlocked and lit after it) | 5 in a row, each under 6 minutes | **1 of 5:** 5 min 41 s, on the second attempt. Session 1 took 8 min 53 s (not counted). |
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

This file is updated when the gate's score changes.
