# Sauna Parametric Config Template

Use this as a starting point for `projects/sauna/config.scad`:

```openscad
// config.scad — Sauna parametric dimensions (inches)
include <BOSL2/std.scad>
include <materials/materials.scad>

// ============================================================
// Building Envelope
// ============================================================
BUILDING_LENGTH = 144;        // 12' exterior
BUILDING_WIDTH = 96;          // 8' exterior
WALL_STUD = 3.5;             // 2x4 framing
EXTERIOR_SHEATHING = 0.5;    // plywood/OSB
SIDING_THICKNESS = 0.6875;   // 11/16" beveled cedar
INTERIOR_PANELING = 0.75;    // T&G cedar
WALL_HEIGHT = 84;            // 7' walls

// Overhangs
EAVE_OVERHANG = 24;          // 2' preferred
GABLE_OVERHANG = 12;         // 1' minimum

// ============================================================
// Room Division
// ============================================================
// Interior length: BUILDING_LENGTH - 2 * (WALL_STUD + EXTERIOR_SHEATHING)
INTERIOR_LENGTH = BUILDING_LENGTH - 2 * (WALL_STUD + EXTERIOR_SHEATHING);
COMMON_WALL_POS = INTERIOR_LENGTH / 2;  // midpoint
HOT_ROOM_DEPTH = COMMON_WALL_POS - WALL_STUD/2 - INTERIOR_PANELING;
CHANGING_ROOM_DEPTH = COMMON_WALL_POS - WALL_STUD/2;

// Interior width
INTERIOR_WIDTH = BUILDING_WIDTH - 2 * (WALL_STUD + EXTERIOR_SHEATHING);

// ============================================================
// Floor System
// ============================================================
FLOOR_JOIST_DEPTH = 5.5;     // 2x6 nominal
FLOOR_JOIST_SPACING = 16;    // on center
SUBFLOOR_THICKNESS = 0.75;   // 3/4" CDX plywood
FLOOR_RIGID_FOAM = 2;        // 2" rigid foam (R-10)

// ============================================================
// Ceiling
// ============================================================
CEILING_RAFTER_DEPTH = 3.5;  // 2x4 (use 5.5 for 2x6)
CEILING_RAFTER_SPACING = 16; // on center
CEILING_HEIGHT = WALL_HEIGHT; // 7' from subfloor

// ============================================================
// Hot Room Door
// ============================================================
DOOR_WIDTH_ROUGH = 26;
DOOR_HEIGHT_ROUGH = 77;      // 6'5"
DOOR_WIDTH_FINISH = 24;
DOOR_HEIGHT_FINISH = 75;     // 6'3"
DOOR_THICKNESS = 2.25;       // plywood + cedar both sides
DOOR_JAMB_WIDTH = WALL_STUD + 2 * INTERIOR_PANELING;  // ~5"

// ============================================================
// Stove Area
// ============================================================
STOVE_PLATFORM_SIZE = 32;    // 32" x 32" (4 x 16" pavers)
PAVER_THICKNESS = 1;
STOVE_CLEARANCE = 7;         // from heat shield to cement board
CEMENT_BOARD_THICKNESS = 0.5;
CEMENT_BOARD_AIR_GAP = 1;    // between double layers
STOVE_PIPE_DIA = 6;          // black stove pipe
CHIMNEY_PIPE_OD = 8;         // insulated chimney pipe

// Chimney openings
CHIMNEY_CEILING_OPENING = 12; // 12" x 12" framed box
CHIMNEY_ROOF_OPENING = 12;   // 12" diameter

// ============================================================
// Benches
// ============================================================
UPPER_BENCH_WIDTH = 24;
LOWER_BENCH_WIDTH = 24;
BENCH_THICKNESS = 3.5;       // 2x4 cedar
UPPER_BENCH_FROM_CEILING = 44;  // sitting height clearance
UPPER_BENCH_CLEAT = UPPER_BENCH_FROM_CEILING + BENCH_THICKNESS; // 47.5"
LOWER_BENCH_TUCK = 4;        // overlap under upper bench
BENCH_CLEARANCE = 0.125;     // 1/8" per side

// Bench frame
BENCH_CROSS_SUPPORT_WIDTH = 2;  // 2x4 ripped to 2"
BENCH_DECK_BOARDS = 5;         // per bench
BENCH_DECK_BOARD_WIDTH = 3.5;  // 2x4 cedar face

// ============================================================
// Windows
// ============================================================
TRANSOM_WIDTH = 30;
TRANSOM_HEIGHT = 18;

// ============================================================
// Ventilation
// ============================================================
INTAKE_VENT_HEIGHT = 12;     // from floor, near stove
EXHAUST_VENT_HEIGHT = 12;    // from ceiling, opposite corner

// ============================================================
// Insulation
// ============================================================
WALL_INSULATION_R = 13;
CEILING_INSULATION_R = 26;   // R-13 + R-13 cross layer (2x4)
// Use 32 for 2x6 rafters: R-19 + R-13

// ============================================================
// Drip Edge
// ============================================================
DRIP_EDGE_WIDTH = 1.5;       // after ripping 2x6
DRIP_EDGE_HEIGHT = 3;
DRIP_EDGE_CHAMFER = 0.75;    // 45-degree chamfer

// ============================================================
// Exterior
// ============================================================
SIDING_WIDTH = 8;            // 8" beveled cedar
SIDING_OVERLAP = 1.25;       // typical overlap
```
