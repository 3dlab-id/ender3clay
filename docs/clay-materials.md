# Clay bodies for extrusion printing

What the clay itself has to do, and how body choice changes flow, drying and shrinkage.
Findings attributed in [sources.md](sources.md).

## Paste consistency

There is no instrument reading in common use — consistency is judged by hand and eye. The
target Keep describes, paraphrased:

- Not so wet that the surface looks wet.
- Not so stiff that slapped-together handfuls show no give at each layer.
- Handleable with clean dry hands without much clay transferring, though stickiness builds
  as you work a ball.

His useful caveat: six clays mixed by hand to what *felt* identical landed within 5% of each
other on a mechanical spike-drop test — so hand judgement is reproducible, **but those same
samples held meaningfully different amounts of water.** Equal feel does not mean equal water.

## Bodies tested

| Body | Detail |
|---|---|
| China / kaolin | Grolleg, Cornwall UK. Mixed from powder. |
| Porcelain | Recycled commercial bodies; ~50% non-clay (feldspar, flint). |
| Ball clay | Hyplas 71, Devon UK. Off-white secondary plastic clay. |
| Fine stoneware | Sio-2 164 ARTE/PRAI white, **40% fine grog** (0–0.2 mm). |
| Coarse stoneware | Ivanhoe (Potclays UK), **20% grog**, 40# (0–0.5 mm). Used for large prints. |
| Red clay | Recycled, sandy but fine and smooth. |

## Findings that affect printing

**Non-clay content is the lever.** Bodies carrying grog, sand, feldspar or flint need *less*
water at the same working consistency, extrude more easily, and shrink least. Pure clays
(china, ball) need the most water and shrink most. The mechanism is particle shape — platey
clay particles hold water between them; rounded non-clay particles don't.

**Less water dries faster**, which is the practical reason to prefer a grogged body when a
print has to survive its own weight while wet.

**Shrinkage is not just water loss.** China clay held the most water at print time yet did
*not* shrink most when bone dry — ball clay did. Don't predict shrinkage from wetness.

**Clay 3D prints shrink anisotropically** — vertical and horizontal rates differ, unlike
hand-formed clay. Measured at bisque (1000 °C), china clay shrank nearly twice as much
vertically as horizontally. At 1220 °C the smooth and coarse stoneware bodies converged at
14% in both directions. The general pattern: **the discrepancy shrinks as firing temperature
rises**, in bodies with high non-clay content.

Practical consequence: if a piece must hit a dimension, the compensation factor depends on
body *and* final firing temperature *and* axis. Test-fire before committing to a batch.

## For our setup

A **grogged stoneware** is the forgiving default — lowest water, easiest extrusion, least
shrinkage, most tolerant of an auger that is still being calibrated. Save porcelain and pure
china clay for after the machine is dialled in.

Note the grog grade against nozzle size: coarse stoneware at 0–0.5 mm grog through our
**1 mm nozzle** leaves only 2× clearance, which is a plausible clog/skip source. Fine grog
(0–0.2 mm) is the safer match at this nozzle.

## Related

- [clay-print-parameters.md](clay-print-parameters.md) — nozzle/layer/speed
- [sources.md](sources.md)
