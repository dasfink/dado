---
name: Woodworking CAD
description: This skill should be used when the user asks to "create an OpenSCAD model", "design furniture", "write SCAD code", "set up a new project", "render a preview", "export STL or DXF", mentions BOSL2, materials, parametric design, or any OpenSCAD woodworking task. Provides core patterns for parametric woodworking CAD with OpenSCAD.
version: 0.1.0
---

# Woodworking CAD with OpenSCAD

Core skill for generating parametric OpenSCAD models of woodworking projects. Covers project structure, materials system, BOSL2 patterns, rendering, and export workflows.

## Project Structure

Organize every project under `projects/<name>/` with these conventions:

- `config.scad` — All parametric dimensions, room context, and configurables
- `assembly.scad` — Top-level file that includes all modules and composes the final model
- One `.scad` file per major component (e.g., `carcass.scad`, `face-frame.scad`, `drawer.scad`)
- All dimensions in **inches**

## Required Includes

Every SCAD file must start with:

```openscad
include <BOSL2/std.scad>
include <materials/materials.scad>
```

**Critical:** Use `include` (not `use`) for `materials.scad` — variables require `include` to be accessible. Use `use` only for libraries that define modules without top-level variables.

For framing projects with dimensional lumber:

```openscad
use <dimensional-lumber/dimensional_lumber.scad>
```

## Materials System

Reference materials from `materials/materials.scad`:

| Constant | Value | Use |
|----------|-------|-----|
| `MAPLE_PLY_THIN` | 3/8" | Drawer bottoms, cabinet backs |
| `MAPLE_PLY_THICK` | 1/2" | Cabinet sides, shelves, partitions |
| `MAPLE_FACE` | 3/4" | Face frames, stiles, rails, edge banding |
| `PINE_INTERNAL` | 3/4" | Cleats, nailers, stretchers, internal structure |

Color modules for visualization:

- `maple_ply_panel(width, height, thickness)` — light warm tan
- `maple_face(width, height, thickness)` — slightly lighter/warmer
- `pine_part(width, height, thickness)` — pale yellow

Sheet goods: `SHEET_WIDTH = 48`, `SHEET_LENGTH = 96` (4x8 sheets).

## BOSL2 Patterns

### Attachments

Position parts relative to each other using BOSL2 attachments instead of manual `translate()`:

```openscad
cuboid([width, depth, height], anchor=BOTTOM)
    attach(TOP) cuboid([shelf_w, shelf_d, MAPLE_PLY_THICK]);
```

Common anchors: `BOTTOM`, `TOP`, `LEFT`, `RIGHT`, `FRONT`, `BACK`, `CENTER`.

### Distributors

Space repeated elements evenly:

```openscad
zcopies(spacing=shelf_spacing, n=num_shelves)
    maple_ply_panel(width, depth);
```

### Boolean Operations

Cut joinery with `diff()`:

```openscad
diff()
    cuboid([w, d, h])
        tag("remove") attach(TOP) cuboid([dado_w, dado_d, dado_depth]);
```

## Config Pattern

Define all dimensions in `config.scad` for parametric control:

```openscad
// config.scad
CABINET_WIDTH = 36;
CABINET_HEIGHT = 34.5;
CABINET_DEPTH = 24;
FACE_FRAME_STILE_W = 1.5;
FACE_FRAME_RAIL_H = 1.5;
DRAWER_SLIDE_CLEARANCE = 0.5;  // per side
```

Then in component files: `include <config.scad>`

## Rendering and Export

Use the OpenSCAD MCP server for previews and exports:

- **`render_single`** — Quick PNG preview. Specify camera position for best angle.
- **`render_perspectives`** — Front/back/left/right/top/iso views in one call.
- **`compare_renders`** — Before/after when iterating on a design.
- **`export_model`** — Final output to STL (3D printing/CNC), DXF (2D laser/CNC), SVG, or 3MF.

### CLI Fallback

```bash
openscad -o output.png --camera=0,0,0,0,0,0,200 input.scad
openscad -o output.stl input.scad
openscad -o output.dxf input.scad
```

## Modeling Best Practices

1. **Start with config** — Define all dimensions parametrically before modeling
2. **One module per component** — Keep files focused on a single assembly
3. **Use color modules** — Always use `maple_ply_panel()`, `maple_face()`, `pine_part()` for visualization
4. **Real dimensions** — Model at actual size in inches; account for material thickness
5. **Clearances matter** — Include drawer slide clearance (1/2" per side), wood movement gaps (1/16"), and hardware offsets
6. **Exploded views** — Add an `explode` parameter to assemblies for documentation renders
7. **No magic numbers** — Every dimension traces back to a named constant in config

## Additional Resources

### Reference Files

For detailed BOSL2 techniques and advanced patterns:
- **`references/bosl2-patterns.md`** — Common BOSL2 attachment and positioning patterns for furniture
- **`references/cut-list-generation.md`** — Generating cut lists from parametric models
