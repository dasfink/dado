---
name: Sauna Build
description: This skill should be used when the user asks to "design a sauna", "model a sauna", "build a sauna", "sauna dimensions", "sauna benches", "sauna stove", "sauna ventilation", "sauna insulation", or any sauna construction task. Provides comprehensive sauna construction knowledge for generating parametric OpenSCAD models.
version: 0.1.0
---

# Sauna Construction for Parametric CAD

Construction knowledge for designing and modeling saunas in OpenSCAD. Based on proven 8x12 sauna builds with wood-burning stoves. All dimensions in inches.

## Building Envelope

Standard footprint: **96" x 144"** (8' x 12') exterior — under 100 sq ft (typically no permit needed).

Divides into two rooms at the midpoint of the 12' wall:
- **Hot room:** ~67" x 89" interior (6' x 8' option or 7' x 7' option)
- **Changing room:** remaining space

Wall height: **84"** (7') recommended for hot room ceiling. 7'6" acceptable. Avoid 8' — wastes 48 cubic feet of heating volume.

Overhangs: minimum 12" eave and gable; 24" preferred.

## Wall Assembly (Outside to Inside)

1. Beveled cedar siding (11/16")
2. Sheathing/housewrap
3. 2x4 studs 16" OC
4. R-13 fiberglass insulation in cavity
5. Foil vapor barrier (Reflectix or foil bubble wrap, rated to 180+ deg F)
6. T&G cedar paneling (3/4")

**Interior finished wall thickness:** ~5" (3.5" framing + 0.75" paneling + vapor barrier)

## Floor System

- 2x6 pressure-treated joists, 16" OC
- 3/4" CDX plywood subfloor (sealed with Kilz)
- 2" rigid foam insulation (R-10) between joists
- Sloped cement board floor in hot room with center drain
- Cedar duck board pallets on top

## Stove Area — Critical Safety Dimensions

- Stove platform: 32" x 32" (4 x 16" step stones)
- **Clearance: 7" minimum** from stove heat shield to cement board wall
- Cement board surround: double-layer 1/2" Durock with 1" air gap
- Chimney ceiling opening: 12" x 12" framed box
- Chimney pipe: 6" stove pipe (black), 8" OD insulated chimney pipe (Selkirk SuperVent)
- Roof opening: 12" diameter (8" pipe + 2" clearance each side)

## Bench Dimensions

| Parameter | Value |
|-----------|-------|
| Upper bench width | 24" |
| Lower bench width | 24" |
| Upper bench from ceiling | 44" (sitting clearance, "two fists overhead") |
| Bench thickness | 3.5" (2x4 cedar) |
| Upper cleat position | 47.5" down from ceiling (44" + 3.5") |
| Lower bench tuck-under | 4" under upper bench |
| Side clearance | 1/8" per side |

Build using "picture frame" method: 2x4 cedar frame with cross supports at 1/3 and 2/3 points, 5 deck boards per bench. Triangle corbel supports for spans over 7'.

## Door Specifications

- Finished: 24" wide x 75" tall (6'3")
- Rough opening: 26" x 77" (6'5")
- **Outswing only** (safety requirement)
- Thickness: 2.25" (1/2" plywood + 3/4" T&G cedar each side)
- Jamb width: ~5" (3.5" wall + 2 x 0.75" paneling)
- Gap at bottom required for ventilation intake

## Ventilation (All Three Required)

1. **Door gap** — natural intake from changing room
2. **Intake vent** — 12" from floor, near stove (same or adjacent wall)
3. **Exhaust vent** — 12" from ceiling, opposite corner from stove

Adjustable covers on intake and exhaust vents.

## Insulation Requirements

| Location | Material | R-Value |
|----------|----------|---------|
| Walls | R-13 fiberglass in 2x4 cavity | R-13 |
| Ceiling (2x4 rafter) | R-13 + R-13 cross layer | R-26 |
| Ceiling (2x6 rafter) | R-19 + R-13 cross layer | R-32 |
| Floor | 2" rigid foam between joists | R-10 |

Foil vapor barrier: complete envelope, all seams taped with aluminum foil tape.

## Paneling

- Western Red Cedar 1x6 T&G (5" face, 3/4" thick)
- 17 boards for 7' wall (84" / 5" = 16.8)
- Installation order: back wall, ceiling, side walls, front wall
- ~968 lineal feet for complete 8x12 interior
- Tongue-nail at 45 degrees, 1.5" finish nails

## Construction Sequence

See `references/construction-sequence.md` for the full 32-step build order with dependencies.

## Key Parametric Values for OpenSCAD

See `references/parametric-config.md` for a complete config.scad template with all sauna dimensions parameterized.

## Additional Resources

### Reference Files

- **`references/parametric-config.md`** — Complete OpenSCAD config template with all parametric dimensions
- **`references/construction-sequence.md`** — Full 32-step build order with dependencies
- **`references/materials-list.md`** — Complete bill of materials for 8x12 sauna from shed stage
