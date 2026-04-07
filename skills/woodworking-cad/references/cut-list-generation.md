# Cut List Generation from Parametric Models

## Approach

OpenSCAD is a modeling tool, not a BOM generator. To produce cut lists from parametric models, embed part metadata in the SCAD code and extract it.

## Method 1: Echo-Based Cut List

Add `echo()` statements to each part module:

```openscad
module side_panel() {
    echo(str("CUT: Maple Ply 1/2\" | ", CABINET_HEIGHT, " x ", CABINET_DEPTH, " | Side Panel | Qty: 2"));
    maple_ply_panel(CABINET_DEPTH, CABINET_HEIGHT);
}

module shelf() {
    shelf_w = CABINET_WIDTH - 2*MAPLE_PLY_THICK;
    echo(str("CUT: Maple Ply 1/2\" | ", shelf_w, " x ", CABINET_DEPTH - 0.25, " | Shelf | Qty: ", NUM_SHELVES));
    maple_ply_panel(shelf_w, CABINET_DEPTH - 0.25);
}
```

Run OpenSCAD and capture output:

```bash
openscad -o /dev/null assembly.scad 2>&1 | grep "^ECHO: \"CUT:" | sort | uniq
```

## Method 2: Structured Part Registry

Define a parts list in config.scad:

```openscad
// Part registry: [name, material, width, length, qty]
PARTS = [
    ["Side Panel",   "Maple Ply 1/2\"", CABINET_DEPTH, CABINET_HEIGHT, 2],
    ["Bottom",       "Maple Ply 1/2\"", CABINET_WIDTH - 2*MAPLE_PLY_THICK, CABINET_DEPTH, 1],
    ["Back",         "Maple Ply 3/8\"", CABINET_WIDTH - 0.25, CABINET_HEIGHT - 0.25, 1],
    ["Shelf",        "Maple Ply 1/2\"", CABINET_WIDTH - 2*MAPLE_PLY_THICK, CABINET_DEPTH - 0.25, NUM_SHELVES],
    ["Face Stile",   "Solid Maple 3/4\"", STILE_W, CABINET_HEIGHT, 2],
    ["Face Rail",    "Solid Maple 3/4\"", CABINET_WIDTH - 2*STILE_W, RAIL_H, 2],
];
```

## Sheet Optimization Notes

When generating cut lists for plywood:
- Standard sheet: 48" x 96" (4' x 8')
- Account for saw kerf: 1/8" per cut
- Grain direction matters for visible faces
- Group parts by material thickness
- Prioritize rip cuts (along 8' length) over cross cuts

## Hardware List

Track hardware alongside cut lists:

```openscad
echo(str("HARDWARE: ", "Drawer Slides 22\" | Qty: ", NUM_DRAWERS, " pair"));
echo(str("HARDWARE: ", "European Hinges 110° | Qty: ", NUM_DOORS, " pair"));
echo(str("HARDWARE: ", "Shelf Pins 1/4\" | Qty: ", NUM_SHELVES * 4));
```
