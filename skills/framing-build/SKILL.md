---
name: Framing Build
description: This skill should be used when the user asks to "design a shed", "model a lean-to", "build a loft bed", "frame a wall", "dimensional lumber framing", "roof framing", "floor joists", "structural framing", or any project involving dimensional lumber construction. Provides framing knowledge for parametric OpenSCAD models.
version: 0.1.0
---

# Structural Framing for Parametric CAD

Construction knowledge for designing sheds, lean-tos, loft beds, and other framed structures in OpenSCAD using dimensional lumber.

## Dimensional Lumber Sizes

**Critical:** Nominal sizes differ from actual sizes. Always model with actual dimensions.

| Nominal | Actual (inches) | Common Use |
|---------|----------------|-----------|
| 2x2 | 1.5 x 1.5 | Furring, light framing |
| 2x4 | 1.5 x 3.5 | Wall studs, light framing |
| 2x6 | 1.5 x 5.5 | Floor joists, rafters, headers |
| 2x8 | 1.5 x 7.25 | Floor joists, headers, beams |
| 2x10 | 1.5 x 9.25 | Floor joists, ridge boards |
| 2x12 | 1.5 x 11.25 | Ridge boards, heavy joists |
| 4x4 | 3.5 x 3.5 | Posts, columns |
| 4x6 | 3.5 x 5.5 | Beams, heavy posts |
| 6x6 | 5.5 x 5.5 | Heavy posts, beams |

Use the `dimensional-lumber` library:

```openscad
use <dimensional-lumber/dimensional_lumber.scad>
```

## Wall Framing

### Standard Wall Assembly

- **Bottom plate:** 2x4 flat, full wall length
- **Top plate:** Double 2x4 flat, full wall length (overlap at corners)
- **Studs:** 2x4, 16" OC (or 24" OC for non-load-bearing)
- **King stud:** Full-height stud beside openings
- **Jack stud (trimmer):** Shortened stud supporting header
- **Header:** Doubled lumber spanning opening (size depends on span)
- **Cripple studs:** Short studs above/below openings, maintaining 16" OC layout

### Header Sizing

| Opening Width | Header Size | Notes |
|--------------|-------------|-------|
| Up to 4' | 2x6 doubled | Light loads |
| 4' to 6' | 2x8 doubled | Standard |
| 6' to 8' | 2x10 doubled | Or LVL beam |
| 8' to 10' | 2x12 doubled | Or engineered |

Headers: two boards with 1/2" plywood spacer to match 3.5" wall depth.

### Wall Height

Standard: 92-5/8" studs + bottom plate (1.5") + double top plate (3") = 97-1/8" (~8'1"). Pre-cut studs at 92-5/8" are standard for 8' ceilings with drywall.

## Floor Framing

### Joist Sizing (Residential, 40 PSF live load)

| Span | Joist Size | Spacing |
|------|-----------|---------|
| Up to 8' | 2x6 | 16" OC |
| 8' to 12' | 2x8 | 16" OC |
| 12' to 15' | 2x10 | 16" OC |
| 15' to 18' | 2x12 | 16" OC |

- **Rim joist (band board):** Same depth as joists, caps the ends
- **Blocking:** Solid blocking or cross-bridging at midspan for joists over 8'
- **Cantilever:** Maximum 1/4 of back-span (e.g., 2' cantilever needs 8' back-span)

### Subfloor

- 3/4" CDX plywood or 3/4" OSB (T&G preferred)
- Glued and screwed to joists
- Stagger seams; ends land on joists

## Roof Framing

### Common Rafter Layout

```
rise = span/2 * pitch;  // e.g., 6/12 pitch: rise = span/2 * 0.5
rafter_length = sqrt((span/2)^2 + rise^2) + overhang;
```

### Roof Pitch Reference

| Pitch | Rise per Foot | Angle | Use |
|-------|--------------|-------|-----|
| 2/12 | 2" | 9.5° | Low slope (membrane roof) |
| 4/12 | 4" | 18.4° | Minimum for shingles |
| 6/12 | 6" | 26.6° | Standard residential |
| 8/12 | 8" | 33.7° | Steeper residential |
| 12/12 | 12" | 45° | Steep, A-frame |

### Shed Roof (Single Slope)

Simplest structure for sheds and lean-tos:
- Front wall taller than back wall
- Rafters run front to back
- Minimum 2/12 pitch for asphalt shingles, 1/4"/ft for metal

### Rafter Sizing

| Span | Rafter Size | Spacing |
|------|-----------|---------|
| Up to 8' | 2x4 | 16" OC |
| 8' to 12' | 2x6 | 16" OC |
| 12' to 16' | 2x8 | 16" OC |
| 16' to 20' | 2x10 | 16" OC |

## Loft Bed Framing

Special considerations for loft beds:

- **Posts:** 4x4 minimum (6x6 for heavy-duty)
- **Under-bed clearance:** 60" minimum for desk use, 48" for storage
- **Side rails:** 2x6 or 2x8, bolted to posts with 3/8" carriage bolts
- **Slat support:** 2x2 cleats along rails, 1x4 slats across
- **Guard rails:** Required on all open sides, 36" above mattress top
- **Mattress platform:** Slats on cleats, or 3/4" plywood on cleats
- **Stairs vs ladder:** Stairs at 60-68° angle preferred over vertical ladder for daily use

### Connection Types

| Joint | Fastener | Use |
|-------|----------|-----|
| Post to rail | 3/8" x 6" carriage bolt | Primary structure |
| Rail to rail | 3" structural screws | Secondary connections |
| Cleat to rail | 2.5" screws, 12" OC | Slat support |
| Slat to cleat | 1.5" screws | Mattress platform |
| Pocket hole | 2.5" pocket screws | Face frame joints |

## OpenSCAD Modeling Tips

### Framing Module Pattern

```openscad
module stud_wall(length, height, stud_spacing=16) {
    num_studs = floor(length / stud_spacing) + 1;
    
    // Bottom plate
    color(COLOR_PINE)
        cuboid([length, 3.5, 1.5], anchor=BOTTOM+FRONT+LEFT);
    
    // Studs
    for (i = [0:num_studs-1])
        right(i * stud_spacing)
            up(1.5)
                color(COLOR_PINE)
                    cuboid([1.5, 3.5, height - 4.5], anchor=BOTTOM+FRONT+LEFT);
    
    // Double top plate
    up(height - 3)
        color(COLOR_PINE)
            cuboid([length, 3.5, 3], anchor=BOTTOM+FRONT+LEFT);
}
```

### Room Shell Pattern

Model the room context for in-situ visualization:

```openscad
module room_shell(length, width, height, wall_thickness=3.5) {
    color("lightgray", 0.2)
    diff()
        cuboid([length, width, height], anchor=BOTTOM)
            tag("remove")
                cuboid([length-2*wall_thickness, width-2*wall_thickness, height+1], anchor=BOTTOM);
}
```

## Additional Resources

### Reference Files

- **`references/shed-plans.md`** — Common shed sizes, layouts, and framing plans
