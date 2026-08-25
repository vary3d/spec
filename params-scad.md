# Root `params.scad`

Optional file at the **bundle root** named exactly `params.scad`. It holds **shared parameters** for a multi-file package (wall, clearance, …). Single-file models do not need it.

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
// params.scad
wall = 2.4;
clearance = 0.2;

// bracket.scad (build root)
include <params.scad>
length = 40;
difference() {
    cube([length, 20, wall]);
}

// spacer.scad (other build root, not shared)
wall = 1.0;
cube(wall);
```

## Two buckets

| Section | Source | Shown when | Saved as |
|---|---|---|---|
| Global | Parse `params.scad` only | File exists **and** current root includes it | Package variant (`variants.json` → `package`) |
| This file | Parse the current root only (do not expand arbitrary includes) | Always, if it has its own assignments | File variant (`variants.json` → `files`) |

Put shared names only in `params.scad`. Do not re-assign the same name in the part file.

You cannot save a Package variant unless root `params.scad` exists, a part file includes it, and Global has at least one parameter.
