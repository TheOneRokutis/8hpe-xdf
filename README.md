# 8HP E-Series XDF (ZB 8646496)

TunerPro XDF definitions and patch entries for the ZF **8HP** transmission calibration used in **E-series native 8HP swaps** (E8x / E9x / E6x) - built around the E70 8HP calibration, **ZB 8646496**.

## What it does

- Map definitions for this calibration, organised into browsable folders (shift lines, shift strategy, torque converter, shift pressure, limits/margins, torque monitoring, and more) - a work in progress, still being expanded and refined.
- **Start in 2nd** - pulls away and holds 2nd at standstill, with 1st still selectable and engaging in manual mode (F-series-like behaviour: D2 at standstill, M1 works).
- **True manual** - manual mode holds the gear to the limiter; no forced upshift.
- A **`stage 3` folder** listing every map a stage-3-style tune touches, so you can see exactly what's involved and adjust those maps yourself.

The definition set is not final and could have mistakes - coverage, naming and documentation are still being worked on and will keep improving.

## Requirements

- **The calibration must be the exact ZB this was built for:**
  - **ZB 8646496** - program `7641033A`, data `A8646497`, HW `7641033` (E70 8HP).
  - Do not use these definitions on any other ZB.
- A clean read of your transmission calibration (from your flashing/read tool).
- TunerPro to open and apply.
- **Checksum correction is up to you** - these files do not correct checksums. After patching, run the bin through your checksum tool before flashing.
- A recovery method: keep a known-good stock calibration ready to re-flash.

## Usage

1. Open your calibration bin in TunerPro with the XDF.
2. Apply the `[PATCH]` entries (Start in 2nd / True manual) via the patch list, or edit maps manually by hand.
3. Save the bin, correct the checksum with your tool, then flash with your usual calibration flash flow.

## Community & Support

For questions, help, and general discussion regarding native 8HP swaps, join the Discord community:
[Join the 8HP Native Swap Discord](https://discord.gg/Pj3zseKPWk)

**All information regarding the native 8HP swap is completely free. If you know someone being charged money just for this information, tell them to join the Discord instead.**

## Disclaimer

These files modify transmission control unit calibration. Flashing modified calibration carries risk, up to rendering the TCU inoperable. Use only on hardware you own or are authorized to modify; always keep an unmodified backup of the original read; correct checksums before flashing; have a recovery method (stock re-flash) available. You are solely responsible for any use. Provided as-is, without warranty of any kind.

## License

[GNU GPLv3](LICENSE)
