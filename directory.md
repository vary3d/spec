# Model directory

One folder per model. Put it at `models/<slug>/` in a git repo (library, skill output, or your own project).

```text
models/<slug>/
  model.scad        # entry OpenSCAD file (required)
  info.json         # listing seed (required to publish)
  cover.png         # 4:3 listing cover (write when you ship a cover)
  variants.json     # named presets; write when there are ≥2 useful sets
  covers/           # one PNG per preset (optional; paths in variants.json)
  params.scad       # optional; package-wide parameters (see params-scad.md)
  LICENSE           # upstream license text, unmodified
  ORIGIN.md         # Forked from URL, original author, what this folder changed
```

`<slug>` must match `info.json` → `slug`. Use lowercase kebab-case.

`info.json` and `variants.json` live in this folder so tools and git can round-trip a package. After **Import from folder**, Vary3D copies their fields into the draft — it does not store those files as the server document. See [info.md](info.md) and [variants.md](variants.md).

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

Skip generator scratch files: `.openscad-iter/`, `.vary3d-iter/`, `brief.json`, `plan.json`, `*.stl`, `.DS_Store`. Vary3D folder import ignores those. `use` / `include` of other `.scad` files stay in the bundle.

Do **not** use `import("….stl")` as the publish entry. Review rejects static non-`.scad` imports. A browser-playable package needs OpenSCAD source.
