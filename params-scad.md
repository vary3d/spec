# Root `params.scad`

Optional file at the **bundle root** named exactly `params.scad`. It holds **shared parameters** for a multi-file package (wall, clearance, …). Write it when complementary pieces live on **different** files and must share wall, footprint, or clearance so they mate. **Kit test:** opening file A cannot export the piece that lives only in file B (`box.scad` has no lid). Single-file models do not need it. Do not write it for a `part` split of one article (that is one file + `part`), for extra Models that are each a full product (an organizer that already includes the box and exports tray/box via `part`), for files that do not assemble, or for helper/library `.scad`.

UI labels this **Global**. Sentences should say **parameters**; the filename stays `params.scad`.

## File

| Rule | Detail |
|---|---|
| Path | Root `params.scad` only. `lib/params.scad` is **not** Global |
| Contents | Top-level Customizer assignments. Do not `include` other files from here (parameter extract does not recurse) |
| Geometry | Selecting `params.scad` is **not** a build root. Preview a part file instead |

Without this file, the model behaves like a single-file Customizer (no Global section).

## Include is the switch

Global parameters apply only when the **current build root** contains:

```openscad
include <params.scad>
```

or `include <./params.scad>`.

These do **not** count:

- `use <params.scad>` (does not import variables)
- `include <lib/params.scad>`
- A commented-out include

```openscad
// params.scad — shared by the box + lid kit so the lid fits the box
wall = 2.4;
clearance = 0.2;

// box.scad (build root)
include <params.scad>
inner = 40;
cube([inner + 2*wall, inner + 2*wall, wall]);

// lid.scad (build root) — mates with the box on the same wall + clearance
include <params.scad>
inner = 40;
outer = inner + 2*wall;
cube([outer + 2*clearance, outer + 2*clearance, wall]);
```

## Two buckets

| Section | Source | Shown when | Saved as |
|---|---|---|---|
| Global | Parse `params.scad` only | File exists **and** current root includes it | Package variant (`variants.json` → `package`) |
| This file | Parse the current root only (do not expand arbitrary includes) | Always, if it has its own assignments | File variant (`variants.json` → `files`) |

Put shared names only in `params.scad`. Do not re-assign the same name in the part file.

You cannot save a Package variant unless root `params.scad` exists, a part file includes it, and Global has at least one parameter.

## Three surfaces

Compilation always sees `include` (the mesh uses Global values). **Sliders do not.** Do not duplicate Global names on the build root just to feed the desktop Customizer.

| Surface | Global sliders | How to change a Global value |
|---|---|---|
| [vary3d.com](https://vary3d.com) | Yes — the site parses `params.scad` when the current root includes it | Drag **Global** |
| OpenSCAD GUI, current build root open | **No** — Customizer does not follow `include` | Edit `params.scad`, or use the site |
| OpenSCAD CLI | No panel | `-D body_z=70.5` on the **entry** file. OpenSCAD treats `-D` as an assignment at the end of that file, so it overrides `include` |

Cover renders should pass those `-D` flags into OpenSCAD (not rewrite the entry file). Rewriting only the build root cannot see names that live in `params.scad`. Prepending `body_z = …` *before* `include <params.scad>` loses: the include assigns last.

A [package README](package-readme.md) Files / Global bucket must spell this out **in prose** when `params.scad` exists. Do not link here from that README.
