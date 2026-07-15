# Sources

Where the knowledge in `docs/` comes from, and the rule for adding to it.

## How to use this directory

These notes record **facts and measurements** — ratios, temperatures, materials, test
outcomes — restated in our own words and reorganised around this machine. They are not a
copy of any source, and they are deliberately not a substitute for reading the originals.

**Do not paste source text, prose or images into this repo.** `github.com/3dlab-id/ender3clay`
is public, so anything committed here is republished to the world. Facts are free to use;
someone else's expression of them is not. Cite and link instead.

## Jonathan Keep

Ceramic artist; his research is the basis of most of `docs/`. All three documents are free to
download from his site, but they are **copyright © Jonathan Keep, all rights reserved** — no
reuse licence is offered. Read them at the source.

- **Clay 3D Printing** (index) — <https://keep-art.co.uk/clay-3d-printing>
- **A Guide to Clay 3D Printing** (2020, 80 pp.) — <https://keep-art.co.uk/guide-to-clay-3d-printing>
  Book-length introduction: printers, printheads, delivery, slicing, drying, file prep.
- **Testing the relationship between Nozzle Size, Layer Height, Print Speed and Material
  Consistency** (11 pp.) — <https://keep-art.co.uk/layer-speed-nozzle-clay>
  Source for [clay-print-parameters.md](clay-print-parameters.md). Nozzle:layer ratio tests
  (1:1 → 5:1) across 1/1.6/2/3 mm nozzles, 25 mm/s, compressed-air Delta (CERAMBOT).
- **Testing of Six Clays for Extrusion 3D Printing** (8 pp.) — <https://keep-art.co.uk/six-clays-for-3d-printing>
  Source for [clay-materials.md](clay-materials.md). Water ratio, extrusion force, flow rate
  and shrinkage (dry / 1000 °C / 1220 °C) across six bodies.

Caveat carried into our notes: Keep's rig is a **compressed-air Delta**. Our machine is an
auger on a bedslinger. Geometry and material findings transfer; flow, pressure and speed
numbers do not.

## Ender-3 Clay Printer Conversion (Instructables)

<https://www.instructables.com/Ender-3-Clay-Printer-Conversion/>

Source of the `M92 E1440` auger E-steps starting point, the `M302 S0` cold-extrusion
approach, and the LCD priming workflow (`Motion > Extruder`). Uses a Tronxy Moore 1 nozzle on
TH3D firmware, 8-bit V1.1.4 board — our board is 32-bit Creality V4, but both run 1/16
microstepping, so E steps transfer in principle. Their author later moved from a NEMA 17 to a
NEMA 23 after the smaller motor skipped steps under clay backpressure.

Note: the site is JS-rendered — `curl` and plain fetches return navigation only.

## morganiron / CERAMIQUE

The inherited Marlin config (`//morganiron` and `CERAMIQUE` comments) came from an earlier
Ender-3 clay project. Treat its E-axis values as **untrusted**: `DEFAULT_AXIS_STEPS_PER_UNIT`
shipped E=130 marked *"A CALIBRER"* (to be calibrated) with a contradictory comment from a
different author about a plastic extruder. That placeholder was the root cause of the
~11× under-extrusion diagnosed 2026-07-15. XYZ motion values appear sound.
