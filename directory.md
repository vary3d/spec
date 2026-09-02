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

packages/<slug>/            # skill default: Import-ready copy (listing files live here)
  model.scad                # entry (required)
  info.json                 # listing seed (required to publish)
  cover.png                 # 4:3 listing cover (write when you ship a cover)
  variants.json             # named presets; write when there are ≥2 useful sets
  covers/                   # preset PNGs + one PNG per extra Model
  params.scad               # optional Global — kit only (see params-scad.md)
  geometry.scad             # optional module helpers; not a build root
  README.md                 # Long-form; GitHub + Import → Docs (see package-readme.md)
  LICENSE                   # upstream license text, unmodified
  ORIGIN.md                 # Forked from URL, original author, what this folder changed
  scad-utils/               # example vendored library (any .scad subdirectory)
```

`<slug>` must match `info.json` → `slug` (lowercase kebab-case). The parent directory name is **not** part of the format — **Import from folder** only reads the folder contents. Default parent directories when using the official skills: `models/<slug>/` for the design working copy (no listing files), `packages/<slug>/` for the Import-ready copy. The official catalog repo ([vary3d/library](https://github.com/vary3d/library)) stores the same Import-ready contents under `models/<slug>/`. User-specified paths always win. If a Node project already uses `packages/` for npm, pick another parent or the path the user named.

`info.json` and `variants.json` live in the **package** folder so tools and git can round-trip. After **Import from folder**, Vary3D copies their fields into the draft — it does not store those files as the server document. See [info.md](info.md) and [variants.md](variants.md). [README.md](package-readme.md) is the long-form page; Import maps it to Documentation (not to `description`).

## Required to play on Vary3D

- A valid `info.json` (`format`: `vary3d.info`)
- An entry `.scad` in the same folder (`entry` in `info.json`, usually `model.scad`)
- Customizer-friendly top-level assignments (see [customizer.md](customizer.md))

## Optional

| File | When |
|---|---|
| `cover.png` | Listing cover (4:3). Set `info.cover` to this path |
| `covers/*.png` | Preset images (`variants.json` `cover` paths) and one PNG per extra build root |
| `variants.json` | At least two meaningful named parameter sets |
| `params.scad` | A kit: complementary pieces on different files that must share wall / footprint / clearance (not a `part` split; not extra Models that are each a full product) |
| Extra root `.scad` | Other **Models** (exportable build roots), not `params.scad` / `geometry.scad` |
| `geometry.scad` / `*.scad` subdirs | **Libraries** — modules or vendored includes; not build roots; no cover render |
| `README.md` | Long-form (GitHub + site Docs). Recommended; Import maps it when present |
| `LICENSE` + `ORIGIN.md` | Always for forks; required in the official [library](https://github.com/vary3d/library). Import does not map them into the draft |

Do not treat this repo as a place to dump STL / 3MF. Geometry is generated in the browser from source.

## What not to ship in a publish folder

Skip generator scratch files: `.openscad-iter/`, `.openscad-preview/`, `.vary3d-iter/`, `brief.json`, `plan.json`, `*.stl`, `.DS_Store`. Vary3D folder import ignores those. `use` / `include` of other `.scad` files stay in the bundle.

Do **not** use `import("….stl")` as the publish entry. The site will not accept static non-`.scad` imports. A browser-playable package needs OpenSCAD source.
