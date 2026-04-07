# BOSL2 Patterns for Woodworking

## Attachment System

BOSL2's attachment system replaces manual `translate()` + `rotate()` chains with semantic positioning.

### Core Concepts

Every BOSL2 shape has named anchor points. `anchor=` sets the shape's origin point; `attach()` places child shapes relative to parent faces.

```openscad
// A cabinet side panel, anchored at its bottom-left-front corner
cuboid([MAPLE_PLY_THICK, CABINET_DEPTH, CABINET_HEIGHT], anchor=BOTTOM+LEFT+FRONT);
```

### Positioning Panels

Cabinet side panels positioned relative to a bottom:

```openscad
// Bottom panel
cuboid([CABINET_WIDTH, CABINET_DEPTH, MAPLE_PLY_THICK], anchor=BOTTOM) {
    // Left side, rising from the left edge of the bottom
    position(LEFT+BOTTOM)
        cuboid([MAPLE_PLY_THICK, CABINET_DEPTH, CABINET_HEIGHT], anchor=BOTTOM+RIGHT);
    // Right side
    position(RIGHT+BOTTOM)
        cuboid([MAPLE_PLY_THICK, CABINET_DEPTH, CABINET_HEIGHT], anchor=BOTTOM+LEFT);
}
```

### Dado Joints

Cut dadoes for shelves using `diff()` and `tag("remove")`:

```openscad
diff()
    // Side panel
    cuboid([MAPLE_PLY_THICK, CABINET_DEPTH, CABINET_HEIGHT], anchor=BOTTOM)
        // Dado groove for shelf
        tag("remove")
            position(FRONT+BOTTOM) up(shelf_height)
                cuboid([MAPLE_PLY_THICK/2, CABINET_DEPTH, MAPLE_PLY_THICK], anchor=FRONT+BOTTOM);
```

### Face Frame Assembly

Build face frames with stiles and rails:

```openscad
module face_frame(width, height, stile_w, rail_h) {
    // Left stile (full height)
    left(width/2 - stile_w/2)
        cuboid([stile_w, MAPLE_FACE, height], anchor=BOTTOM);
    // Right stile
    right(width/2 - stile_w/2)
        cuboid([stile_w, MAPLE_FACE, height], anchor=BOTTOM);
    // Top rail (between stiles)
    up(height - rail_h/2)
        cuboid([width - 2*stile_w, MAPLE_FACE, rail_h]);
    // Bottom rail
    up(rail_h/2)
        cuboid([width - 2*stile_w, MAPLE_FACE, rail_h]);
}
```

### Distributors for Repeated Elements

Evenly space drawer dividers or shelf pins:

```openscad
// 4 shelves evenly spaced
zcopies(spacing=CABINET_HEIGHT/5, n=4)
    maple_ply_panel(CABINET_WIDTH - 2*MAPLE_PLY_THICK, CABINET_DEPTH - 0.25);

// Shelf pin holes along a side panel
back(1) right(1)  // offset from edges
    grid_copies(spacing=[0, 32], n=[1, 8])  // 32mm system
        zcyl(d=5/25.4, h=MAPLE_PLY_THICK + 0.01, anchor=BOTTOM);  // 5mm holes
```

### Rotation and Orientation

Orient panels for different planes:

```openscad
// Horizontal panel (shelf/bottom) — default cuboid orientation works
cuboid([width, depth, thickness]);

// Vertical panel (side) — rotate or use anchor to stand it up
cuboid([thickness, depth, height], anchor=BOTTOM);

// Back panel — thin, set back
back(depth/2)
    cuboid([width, MAPLE_PLY_THIN, height]);
```

### Color-Coded Assembly Visualization

Use material modules with transparency for assembly views:

```openscad
module assembly(explode=0) {
    // Carcass in maple ply
    color(COLOR_MAPLE_PLY, 0.8)
        carcass();
    
    // Face frame in solid maple, offset forward
    fwd(explode)
        color(COLOR_MAPLE_SOLID)
            face_frame();
    
    // Internal structure in pine
    color(COLOR_PINE, 0.6)
        internal_cleats();
}
```

## Common Furniture Modules

### Drawer Box

```openscad
module drawer_box(width, depth, height) {
    // Sides
    for (m = [LEFT, RIGHT])
        position(m)
            maple_ply_panel(depth, height, MAPLE_PLY_THIN);
    // Front and back
    for (m = [FRONT, BACK])
        position(m)
            maple_ply_panel(width - 2*MAPLE_PLY_THIN, height, MAPLE_PLY_THIN);
    // Bottom (set into dado)
    up(MAPLE_PLY_THIN)
        maple_ply_panel(width - 2*MAPLE_PLY_THIN - 0.25,
                        depth - MAPLE_PLY_THIN - 0.25,
                        MAPLE_PLY_THIN);
}
```

### Edge Banding

Apply solid maple edge to plywood:

```openscad
module edge_band(length, panel_thickness, band_width=0.75) {
    color(COLOR_MAPLE_SOLID)
        cuboid([band_width, MAPLE_FACE, length]);
}
```
