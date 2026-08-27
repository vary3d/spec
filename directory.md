# Package directory

One folder per **importable** model. The interchange unit is that folder’s contents (`model.scad` + `info.json`, …). **Import from folder** does not care what the parent directory is called.

Default locations in a project that uses the official skills:

| Tree | Skill | Role |
|---|---|---|
| `models/<slug>/` | openscad-customizer | Design working copy. Entry `model.scad`. May contain `brief.json`, `plan.json`, `.openscad-preview/`. **Not** a publish folder — no `info.json` / covers here. |
| `packages/<slug>/` | vary3d-package | Import-ready copy. Same slug. Listing files live here. |

```text
models/<slug>/              # design (working copy)
  model.scad
  brief.json                # complex parts only
  .openscad-preview/        # not for git

packages/<slug>/            # publish / Import from folder
  model.scad                # entry (required)
  info.json                 # listing seed (required to publish)
  cover.png                 # 4:3 listing cover (write when you ship a cover)
  variants.json             # named presets; write when there are ≥2 useful sets
  covers/                   # one PNG per preset (optional; paths in variants.json)
  params.scad               # optional; package-wide parameters (see params-scad.md)
  LICENSE                   # upstream license text, unmodified
  ORIGIN.md                 # Forked from URL, original author, what this folder changed
```

`<slug>` must match `info.json` → `slug` (lowercase kebab-case). Official catalog git layout uses `packages/<slug>/`. User-specified paths always win. If a Node project already uses `packages/` for npm, pick another parent or the path the user named — CAD-first trees use `packages/<slug>/`.

`info.json` and `variants.json` live in the **package** folder so tools and git can round-trip. After **Import from folder**, Vary3D copies their fields into the draft — it does not store those files as the server document. See [info.md](info.md) and [variants.md](variants.md).

## Required to play on Vary3D

- A valid `info.json` (`format`: `vary3d.info`)
- An entry `.scad` in the same folder (`entry` in `info.json`, usually `model.scad`)
- Customizer-friendly top-level assignments (see [customizer.md](customizer.md))

## Optional

| File | When |
|---|---|
| `cover.png` | Listing cover (4:3). Set `info.cover` to this path |
| `covers/*.png` | One image per named preset; optional `cover` paths in `variants.json` |
| `variants.json` | At least two meaningful named parameter sets |
| `params.scad` | Multi-file packages that share wall, clearance, etc. |
| Extra `.scad` | Parts included or used by the entry file |
| `LICENSE` + `ORIGIN.md` | Always for forks; required in the official [library](https://github.com/vary3d/library) |

Do not treat this repo as a place to dump STL / 3MF. Geometry is generated in the browser from source.

## What not to ship in a publish folder

Skip generator scratch files: `.openscad-iter/`, `.openscad-preview/`, `.vary3d-iter/`, `brief.json`, `plan.json`, `*.stl`, `.DS_Store`. Vary3D folder import ignores those. `use` / `include` of other `.scad` files stay in the bundle.

Do **not** use `import("….stl")` as the publish entry. The site will not accept static non-`.scad` imports. A browser-playable package needs OpenSCAD source.
