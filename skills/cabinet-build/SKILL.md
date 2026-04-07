---
name: Cabinet Build
description: This skill should be used when the user asks to "design a cabinet", "build a dresser", "model drawers", "face frame cabinet", "frameless cabinet", "kitchen cabinets", "bathroom vanity", "bookcase", or any furniture carcass construction task. Provides cabinet and furniture construction knowledge for parametric OpenSCAD models.
version: 0.1.0
---

# Cabinet and Furniture Construction for Parametric CAD

Construction knowledge for designing cabinets, dressers, bookcases, and furniture carcasses in OpenSCAD. Covers both face-frame and frameless (European) construction.

## Cabinet Anatomy

A standard cabinet has these components:

1. **Carcass** — The box: two sides, top, bottom, back, and optional fixed shelves
2. **Face frame** — Solid hardwood frame on the front (face-frame style only)
3. **Doors/drawers** — Closing elements mounted to carcass or face frame
4. **Internal fittings** — Adjustable shelves, drawer slides, dividers
5. **Base/legs** — Toe kick, legs, or integrated base

## Standard Dimensions

### Kitchen Base Cabinets
| Parameter | Standard | Notes |
|-----------|----------|-------|
| Height | 34.5" | 36" with countertop |
| Depth | 24" | 21" for bathroom vanity |
| Width | 12"-48" | In 3" increments |
| Toe kick height | 4" | |
| Toe kick depth | 3" | |
| Countertop thickness | 1.5" | |

### Kitchen Wall Cabinets
| Parameter | Standard | Notes |
|-----------|----------|-------|
| Height | 30", 36", 42" | Common heights |
| Depth | 12" | |
| Bottom of cabinet from floor | 54" | Standard; 18" above countertop |

### Face Frame Dimensions
| Element | Width | Thickness |
|---------|-------|-----------|
| Stiles (vertical) | 1.5" | 3/4" |
| Rails (horizontal) | 1.5" | 3/4" |
| Center stile | 1.5" | 3/4" |
| Mullion | 3/4" | 3/4" |

## Material Assignments

Using the materials system from `materials/materials.scad`:

| Component | Material | Constant |
|-----------|----------|----------|
| Sides, top, bottom, shelves | Maple ply 1/2" | `MAPLE_PLY_THICK` |
| Back panel | Maple ply 3/8" | `MAPLE_PLY_THIN` |
| Drawer bottoms | Maple ply 3/8" | `MAPLE_PLY_THIN` |
| Face frame stiles & rails | Solid maple 3/4" | `MAPLE_FACE` |
| Cleats, nailers | Pine 3/4" | `PINE_INTERNAL` |

## Carcass Construction

### Face-Frame Style

Carcass is slightly narrower than nominal width (face frame overhangs):

```
carcass_width = cabinet_width - 2 * MAPLE_FACE;  // face frame stiles overhang
carcass_depth = cabinet_depth;
carcass_height = cabinet_height - toe_kick_height;
```

Sides are full depth and height. Top and bottom fit between sides:

```
top_bottom_width = carcass_width - 2 * MAPLE_PLY_THICK;
```

Back panel sits in rabbit (3/8" deep) along back edges of sides, top, and bottom.

### Frameless (European) Style

Carcass IS the nominal width. Edges get edge banding:

```
carcass_width = cabinet_width;
side_height = cabinet_height - toe_kick_height;
```

Sides, top, bottom all 3/4" (use `MAPLE_FACE` thickness or 3/4" ply). Hardware mounts directly to carcass edges.

## Drawer Construction

### Drawer Box Sizing

```
drawer_width = opening_width - 2 * slide_clearance;  // 1/2" per side for standard slides
drawer_depth = cabinet_depth - 1;  // 1" shorter than cabinet depth
drawer_height = opening_height - 1/2";  // clearance above
```

### Drawer Box Assembly

- **Sides:** 1/2" maple ply, full depth, full height
- **Front/back:** 1/2" maple ply, width minus 2 side thicknesses
- **Bottom:** 3/8" maple ply in 1/4" dado, 1/4" up from bottom edge
- **False front:** 3/4" solid maple or ply, overlays face frame by 3/8" all around

### Drawer Slide Specs

| Type | Clearance per side | Extension |
|------|-------------------|-----------|
| Side-mount ball bearing | 1/2" | Full or 3/4 |
| Under-mount (Blum) | varies | Full |
| Center-mount wood | 0 | 3/4 |

## Joinery for Modeling

Model these joints for accurate dimensions (even if simplified visually):

- **Dado:** 1/2" wide, 1/4" deep groove for shelves into sides
- **Rabbet:** 3/8" x 3/8" along back edges for back panel
- **Pocket holes:** Face frame joints (model as butt joints, note pocket hole locations)
- **Biscuit/dowel:** Carcass joints (model as butt joints)

## Adjustable Shelf System

32mm European system:
- Two columns of 5mm holes, 32mm apart vertically
- Columns set 37mm (1-7/16") from front and back edges
- Start first hole 37mm from bottom
- Shelf pins: 5mm or 1/4" diameter

## Hardware Clearances

- **European hinge:** 3mm (1/8") gap between door edge and carcass side
- **Overlay:** Full overlay = door covers entire face frame; half overlay = shares stile with adjacent door
- **Drawer front gap:** 1/16" between adjacent drawer fronts
- **Pull/knob:** Centered on rail height for doors; centered on drawer front height

## Additional Resources

### Reference Files

- **`references/kitchen-layout.md`** — Standard kitchen cabinet layout rules, work triangle, and common configurations
