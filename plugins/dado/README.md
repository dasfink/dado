# Dado

Parametric woodworking CAD plugin for Claude Code. Turns Claude into a woodworking design assistant using OpenSCAD.

## What it does

- Generates parametric OpenSCAD models for furniture and structures
- Renders previews and exports STL/DXF via the OpenSCAD MCP server
- Provides construction knowledge for saunas, cabinets, sheds, and loft beds
- Reviews SCAD files for correctness and best practices

## Prerequisites

- [OpenSCAD](https://openscad.org/) installed (`brew install openscad` on macOS)
- [uv](https://github.com/astral-sh/uv) for running the MCP server
- BOSL2 library in your project's `libraries/` directory

## Skills

| Skill | Triggers on |
|-------|------------|
| `woodworking-cad` | Any OpenSCAD/SCAD work, materials, BOSL2, project setup |
| `sauna-build` | Sauna design, construction, stove, benches, ventilation |
| `cabinet-build` | Cabinets, drawers, face frames, furniture carcasses |
| `framing-build` | Sheds, lean-tos, loft beds, dimensional lumber framing |

## Agent

- `scad-reviewer` — Reviews .scad files for material usage, BOSL2 patterns, and structural correctness

## MCP Server

Bundles the [openscad-mcp](https://github.com/quellant/openscad-mcp) server:
- `render_single` — PNG preview with camera control
- `render_perspectives` — multi-view render
- `compare_renders` — before/after comparison
- `export_model` — STL, DXF, SVG, 3MF export

## Installation

```bash
claude --plugin-dir /path/to/dado
```

Or install from marketplace once published.
