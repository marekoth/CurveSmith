# CurveSmith - Precision PBO & Curve Optimizer Tuner

A lightweight, automated Curve Optimizer (CO) calibrator and low-level SMU tuning utility for AMD Ryzen processors.

Instead of spending an entire weekend manually tweaking offsets in BIOS and guessing which core crashed your system, CurveSmith dials in stable, per-core undervolts through direct hardware mailbox calls and targeted single-core stress routines.

## Why CurveSmith?

- **Fast Calibration (~30 Mins for 8 Cores):** Traditional tools brute-force offsets from 0 to -30 step-by-step, taking hours. CurveSmith uses an adaptive search engine that reads CPPC priority and previous core behavior to home in on silicon limits quickly, averaging ~30 minutes on an 8-core CPU.
- **Full Desktop Architecture Coverage:** Native SMU profiles and tailored workloads for:
  - Zen 3 & Zen 3+ (Vermeer / Ryzen 5000, 5000X3D)
  - Zen 4 (Raphael / Ryzen 7000, 7000X3D)
  - Zen 5 (Granite Ridge / Ryzen 9000, 9000X3D)
- **Direct Hardware Mailbox (Ring-0):** Applies CO offsets and boost overrides live via the PawnIO kernel driver. No Ryzen Master background services or BIOS reboots needed between offset adjustments.
- **Automated Crash & Reboot Resilience:** Finding silicon limits causes reboots by design. If the system crashes or drops a WHEA error mid-calibration, CurveSmith detects it upon reboot, registers the failing offset as a hard boundary, snaps back to the last verified safe baseline, and resumes testing automatically.
- **Architecture-Specific Stress Engines:** Binds directly to matching AVX2 / AVX-512 workload binaries (Kagari for Zen 3, Kizuna for Zen 4, Komari for Zen 5 dual 512-bit) with core affinity pinning and load-shock cycles to expose instability instantly.
- **Static Manual OC Mode:** Full-control fallback mode for non-X3D silicon to test static voltage and all-core frequency limits outside AMD FIT regulation.
- **Bilingual UI (EN / TR):** Complete native interface in English and Turkish with system locale auto-detection and an instant toggle.

## How It Works

1. **Topology & CPPC Mapping:** Queries hardware APIC IDs and MSR registers to map physical CCD/Core positions and read AMD preferred core rankings.
2. **Targeted Load Shocks:** Drives single-core workloads through y-cruncher kernels while rapidly toggling thread states to stress transient voltage drops (vDroop).
3. **Adaptive Range Refinement:** Narrows the stable margin between known-safe and known-failing offsets using bounded binary steps rather than linear 1-by-1 stepping.
4. **Permanent Slot Profiles:** Lets you save up to 5 tuning profiles to hardware slots with an option to auto-apply your daily profile on Windows startup.

## Requirements

- **OS:** Windows 10 / 11 (64-bit)
- **CPU:** AMD Ryzen Desktop Processor (Zen 3 through Zen 5)
- **Privileges:** Administrator rights (required for Ring-0 SMU and MSR communication)
- **Driver:** PawnIO low-level driver (packaged with the setup, prompts cleanly on first launch if missing)

## Installation

1. Download the latest `CurveSmith_Setup.exe` from the Releases section.
2. Run the installer with administrator privileges.
3. Launch CurveSmith. If PawnIO is not installed on the system, the application will guide you through the one-click driver setup.
4. Select your target boost offset and start calibration.

## Disclaimer

CurveSmith interacts directly with processor voltage planes and frequency scalers. Modifying hardware parameters outside factory specifications carries inherent risks of instability, data loss, or component degradation. All tuning is performed at your own risk.
