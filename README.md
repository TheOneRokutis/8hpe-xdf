# 8HP E-Series XDF (ZB 8646496)

TunerPro XDF definitions and patch entries for the ZF **8HP** transmission calibration used in **E-series native 8HP swaps** (E8x / E9x / E6x) - built around the E70 8HP calibration, **ZB 8646496**.

## What it does

- Map definitions for this calibration, organised into browsable folders (shift lines, shift strategy, torque converter, shift pressure, limits/margins, torque monitoring, and more) - a work in progress, still being expanded and refined.
- **Start in 2nd** - pulls away and holds 2nd at standstill, with 1st still selectable and engaging in manual mode (F-series-like behaviour: D2 at standstill, M1 works).
- **True manual** - manual mode holds the gear to the limiter; no forced upshift.
- **Wheel circumference (Radumfang)** - an editable value in the **Patches** category that sets the tire's rolling circumference the TCU works with. Getting this right is what makes the learned axle ratio correct (see below).
- A **`stage 3` folder** listing every map a stage-3-style tune touches, so you can see exactly what's involved and adjust those maps yourself.

The definition set is not final - coverage, naming and documentation are still being worked on and will keep improving.

## Wheel circumference (Radumfang)

The TCU converts its output-shaft speed into vehicle speed using two programmed numbers: the **wheel circumference** and the axle ratio. It also **learns** the axle ratio while driving, by comparing its own output-shaft speed against the wheel speeds reported by the ABS/DSC - using the programmed circumference to do it. If the programmed circumference doesn't match the car, that comparison is skewed and **the learned axle ratio comes out wrong**, which shows up as poor stop/creep behaviour and odd shift decisions. The speedometer still looks fine, which is exactly why this goes unnoticed.

This calibration is shared across several platforms and carries a default wheel size (2249 mm - an 18" SUV size) that is often not the car's. **If your learned axle ratio does not match your real diff ratio, this is the cause.**

**Check it** (Tool32, SGBD `gsb233.prg`):

- `status_radumfang` → `STAT_RADUMFANG_WERT` - the programmed circumference (mm).
- `status_lernfkt` → `STAT_HA_WERT` (learned rear-axle value), `STAT_HA_LERNFLAG`, plus the 11 measurement samples.

**Fix it:** open the item **Radumfang - dynamic wheel circumference (mm)** - it sits in the **Patches** category - and enter your tire's **rolling circumference**, *not* the geometric one:

- formula: `pi x (rim_inch x 25.4 + 2 x width_mm x profile_% / 100) x 0.97`
- or use the tire maker's published dynamic rolling circumference
- or use the calculator: **https://theonerokutis.github.io/8hpe-xdf/**

The TCU only ever accepts the closest value from its fixed ratio list - `2.00 2.35 2.47 2.56 2.65 2.81 2.93 3.08 3.15 3.23 3.38 3.46 3.64 3.73 3.91 4.10 4.27` - so the number only needs to be within roughly -1 % / +2.5 % of the reference. Do **not** use this setting to correct the speedometer - that is the ABS/DSC coding, not the transmission.

## After flashing - reset the learned value

Changing the wheel circumference (or the diff ratio) only takes effect once the TCU re-learns:

1. Flash the modified calibration as usual.
2. In Tool32 (`gsb233.prg`) run **`steuern_lernfkt_ruecksetzen`** with argument **`0`** - this clears the learned rear-axle value and its measurement samples.
3. Drive above ~24 km/h for a minute or two - the TCU samples about once per second, up to 11 samples.
4. Re-read `status_lernfkt`: `STAT_HA_WERT` should now match your real diff ratio, and `STAT_HA_LERNFLAG` should read *eingelernt*. `status_radumfang` should show the value you programmed.
5. If the learned value is one step off (e.g. 3.91 instead of 3.73), multiply the value you entered by (real ratio ÷ learned value) and repeat.

Without the reset, the TCU keeps its old learned value and the change has no effect.

## Requirements

- **The calibration must be the exact ZB this was built for:**
  - **ZB 8646496** - program `7641033A`, data `A8646497`, HW `7641033` (E70 8HP).
  - Do not use these definitions on any other ZB.
- **A full 2 MB image is required.** These definitions are addressed as a 2 MB file in which the calibration sits at `0x180200` and the program area above it reads back as `FF` (blank). A calibration-only file (about 512 KB) will not line up.
- A clean read of your transmission calibration (from your flashing/read tool).
- **Suggested tool: [8HPe Transmission Quickflash](https://www.bimmertuningtools.com/product/8hpe-transmission-quickflash/)** - it reads and writes exactly this 2 MB image, and **corrects the checksums automatically** when flashing.
- TunerPro to open and apply.
- **Checksum correction:** handled automatically if you flash with 8HPe Quickflash; with any other tool it is up to you - these files do not correct checksums, so run the bin through your checksum tool before flashing.
- A recovery method: keep a known-good stock calibration ready to re-flash.

## Usage

1. Open your 2 MB calibration image in TunerPro with the XDF.
2. Apply the `[PATCH]` entries (Start in 2nd / True manual) via the patch list, or edit maps manually by hand.
3. For the wheel circumference, set **Radumfang - dynamic wheel circumference (mm)** to your tire's rolling circumference (see above).
4. Save the bin, then flash with your usual calibration flash flow.
5. After flashing, reset the learned value and drive to re-learn - see **After flashing** above.

## Community & Support

For questions, help, and general discussion regarding native 8HP swaps, join the Discord community:

[Join the 8HP Native Swap Discord](https://discord.gg/Pj3zseKPWk)

**All information regarding the native 8HP swap is completely free. If you know someone being charged money just for this information, tell them to join the Discord instead.**

## Disclaimer

These files modify transmission control unit calibration. Flashing modified calibration carries risk, up to rendering the TCU inoperable. Use only on hardware you own or are authorized to modify; always keep an unmodified backup of the original read; correct checksums before flashing; have a recovery method (stock re-flash) available. You are solely responsible for any use. Provided as-is, without warranty of any kind.

## License

[GNU GPLv3](LICENSE)
