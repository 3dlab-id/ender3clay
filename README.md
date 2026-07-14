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

Slice with a **clay profile that emits no temperature commands** — no `M104/M109/M140/M190`.
The dummy sensor never reaches a target, so any heat-and-wait command hangs the print on
"heating". Set nozzle/bed temps to 0 (or strip them from Start G-code), and tune flow/speed
for paste (much higher flow, slower speed than PLA).

## First-run checklist

- Confirm the mechanical **Z-min endstop switch is wired** before homing Z (`G28`).
- Level the bed manually (four knobs + paper) — no auto bed leveling.
- Set the Z working offset with `M851 Z-<value>` + `M500`, or adjust the Z-switch bracket.
- Calibrate extruder E-steps (`DEFAULT_AXIS_STEPS_PER_UNIT` E value) for the paste system.
