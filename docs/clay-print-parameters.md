# Clay print parameters

Working reference for tuning this machine (Ender-3 + auger, 1 mm nozzle). Findings are
attributed; see [sources.md](sources.md). Numbers from other people's rigs are **starting
points, not truth** — their delivery systems differ from our auger.

## Nozzle-to-layer-height ratio

The single most important geometric relationship in clay printing. Jonathan Keep tested
ratios of 1:1, 3:2, 2:1, 3:1 and 5:1 across 1, 1.6, 2 and 3 mm nozzles, holding clay
consistency and speed constant (25 mm/s, compressed-air delivery, Delta printer):

| Nozzle : layer | Verdict |
|---|---|
| 1:1 | **Fails.** The deposited bead sits lower than the nozzle, so the printhead loses contact with the print. Broke down at overhangs. |
| 3:2 | **Marginal.** Prints fine vertically and handles texture, but layers bond poorly and it collapsed early on a 45° overhang. |
| 2:1 | Works |
| 3:1 | Works |
| 5:1 | Works |

His own benchmark shape: 1.6 mm nozzle, 0.5 mm layer (≈3:1), medium consistency, two walls.

### Applied to our 1 mm nozzle

| Ratio | Layer height | Use |
|---|---|---|
| 2:1 | **0.5 mm** | What `Clay 0.5mm` currently ships. Fastest, but the loosest ratio that still works — least overhang margin. |
| 3:1 | 0.33 mm | Drop here first if overhangs slump or layers delaminate. Matches Keep's benchmark ratio. |
| 5:1 | 0.2 mm | Finest detail, slowest. |

**If prints slump on overhangs, change layer height before touching anything else** — that is
the variable his tests isolate most clearly.

## Speed

Keep's tests ran at **25 mm/s**, but with compressed-air delivery on a Delta. Our
`Clay 0.5mm` process uses **15 mm/s**, chosen conservatively for an auger on a bedslinger
(the moving bed adds inertia his Delta doesn't have). 25 mm/s is a reasonable thing to try
once flow is calibrated — raise speed only after E steps/mm is settled, never before, or
you're tuning two variables at once.

## Walls and infill

- Keep's benchmark uses a **double wall / two shells**, which printed with no slumping.
  Our process sets `wall_loops: 2` to match.
- Clay work is frequently printed as a single continuous spiral (vase mode). Orca calls this
  **Spiral vase**; it removes the Z seam and layer-change stutter that shows badly in clay.
  Worth enabling for open vessel forms.

## Delivery pressure (why our numbers won't match his)

Keep's rig used compressed air to feed clay to the printhead, with **more air pressure needed
for larger nozzles**, and the printhead screw speed set by the slicer. Our machine has no air
assist — the auger both meters *and* pressurises. That means:

- Our flow ceiling is auger torque, not tank pressure. See the LV8729 trimpot note in the
  main [README](../README.md#extruder-calibration).
- His volumetric numbers don't transfer. Ratios and layer geometry do.

## Related

- [clay-materials.md](clay-materials.md) — clay body selection, water, shrinkage
- [sources.md](sources.md) — attribution and links to the originals
