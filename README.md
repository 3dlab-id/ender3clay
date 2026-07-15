# ender3clay

Marlin 2.1.2.1 firmware for a **Creality Ender-3 (Pro)** converted into a **clay / paste printer**.

- **Board:** Creality V4.2.x (`BOARD_CREALITY_V4`, STM32F103RE, 512 KB)
- **Drivers:** LV8729 (standalone step/dir)
- **Head:** non-heated paste extruder — dummy temperature sensor, cold extrusion enabled
- **Bed:** unheated, no auto-leveling probe (mechanical Z-min endstop, manual bed leveling)
- **Frame:** stock Ender-3, 234 × 234 × 245 mm

Config lives in `Marlin-2.1.2.1/Marlin/Configuration.h` and `Configuration_adv.h`.
Clay-specific changes are tagged with `CERAMIQUE` / `CLAY` comments. Base motion tuning
inherited from morganiron's Ender-3 clay project (`//morganiron` comments).

## Knowledge base

Clay-printing reference notes live in [`docs/`](docs/):

- [clay-print-parameters.md](docs/clay-print-parameters.md) — nozzle:layer ratios, speed, walls
- [clay-materials.md](docs/clay-materials.md) — clay body choice, water, shrinkage
- [sources.md](docs/sources.md) — attribution and links to the originals

## Build

Requires [PlatformIO](https://platformio.org/).

```bash
cd Marlin-2.1.2.1
pio run                 # default env is STM32F103RE_creality
```

Output: `.pio/build/STM32F103RE_creality/firmware-<date>-<time>.bin`

## Flash (Creality V4, SD-card method)

1. Copy **one** `firmware-*.bin` to the root of a FAT32 microSD card (ideally the only `.bin`).
2. Insert card, power-cycle the printer. Screen blanks ~10 s while flashing, then boots.
3. The bootloader skips a `.bin` whose name it already flashed — each build gets a fresh
   timestamped name, so reflashing just works.

## Slicing for clay

Import **`3DLAB CLAY Ender 3.orca_printer`** (OrcaSlicer → File → Import → Import Configs).
It bundles the printer, the `Clay` filament preset, and the `Clay 0.5mm` process.

Slice with a **clay profile that emits no temperature commands** — no `M104/M109/M140/M190`.
The dummy sensor never reaches a target, so any heat-and-wait command hangs the print on
"heating". Set nozzle/bed temps to 0 (or strip them from Start G-code), and tune flow/speed
for paste (much higher flow, slower speed than PLA).

The profile's Start G-code carries the settings that make clay work on stock firmware, so it
runs correctly **without reflashing**:

| Command | Why |
|---|---|
| `M302 S0` | allow cold extrusion |
| `M92 E1440` | auger E steps (firmware default of 130 is ~11x too low) |
| `M900 K0` | disable linear advance (meaningless for an auger) |

Machine limits are pinned to what the firmware can actually do (`M203 X150 Y150 E10`,
`M201 E1000`). Do not raise E limits without re-checking step rate: at 1440 steps/mm,
E60 would demand 86,400 steps/s, which the STM32F103 cannot issue.

## Extruder calibration

E steps/mm is the single most important number on this machine — it sets both print flow
and how fast the LCD `Motion > Extruder` menu turns the auger (RPM = feedrate x steps/mm).

`1440` is a **starting point** borrowed from the Tronxy Moore nozzle
([Instructables build](https://www.instructables.com/Ender-3-Clay-Printer-Conversion/)), valid
because both run NEMA17 at 1/16 (3200 steps/rev -> 2.22 mm E/rev, ~5.3 mm3/rev). Our auger
differs, so calibrate by weighing:

```gcode
M83              ; relative E
G1 E200 F336     ; 200 "mm" at ~5.6 mm/s (near real print E speed)
```

Weigh the extruded clay (`m` grams), and with clay density `rho` (~0.0018 g/mm3 wet — weigh a
known volume to confirm):

```
V_actual   = m / rho                        (mm3)
V_expected = 200 * 2.405 = 481 mm3          (the 1.75mm filament fiction)
E_steps    = 1440 * 481 / V_actual
```

Apply with `M92 E<value>` + `M500`, and update the Start G-code in the Orca profile to match.
Calibrate at print speed, not slowly — an auger's volume-per-step drops under backpressure.

Keep filament diameter at **1.75 mm** in the slicer. It is a fiction, but it is the fiction
E steps/mm is calibrated against; changing it silently rescales all flow.

> Motor current is set by the **trimpot on the LV8729**, not firmware — `E0_CURRENT` in
> `Configuration_adv.h` sits inside `#if AXIS_IS_TMC_CONFIG(E0)` and is dead code for these
> drivers. If the auger stalls or skips under clay backpressure, adjust the pot.

## First-run checklist

- Confirm the mechanical **Z-min endstop switch is wired** before homing Z (`G28`).
- Level the bed manually (four knobs + paper) — no auto bed leveling.
- Set the Z working offset with `M851 Z-<value>` + `M500`, or adjust the Z-switch bracket.
- Calibrate extruder E-steps (`DEFAULT_AXIS_STEPS_PER_UNIT` E value) for the paste system.
