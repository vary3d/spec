# Package `README.md`

Long-form page for a publish folder: GitHub landing page **and** the text Import maps to site Documentation. Cards still use `info.name` / `description` / `cover` only.

Generate listing facts from `info.json`, `variants.json`, Customizer knobs, and cover PNGs. Do **not** invent a second blurb. Do **not** copy the upstream project README into this file. Preserve an existing `## Print` section when regenerating (do not invent print steps).

English unless the user asked for another listing language (`info.sourceLocale`).

On case-insensitive disks this spec file is named `package-readme.md` so it does not collide with this repository’s `README.md`.

## When to write

Recommended whenever the folder may live on git, and required if the model should have Documentation after Import. `validate-info.py` must not fail if this file is missing (a package can still play without Docs).

GitHub uses an ATX `#` title. Import **strips that H1** (the model name is already the page H1) and rewrites relative images to docs media.

## Section order

Fixed. Omit a bucket or section when it has nothing to show.

1. **Title + description** — `info.name` as `#` heading; `info.description` as the first paragraph. Optional listing cover as a GFM image (`![…](cover.png)`), not HTML. Import may drop a Hero that only repeats the model cover.
2. **Source** — fork and original use different copy (below). Expand the facts; do not write “see ORIGIN.md”.
3. **Files** — three buckets (below). Empty buckets omitted.
4. **Presets** — each `variants.json` item: title, description, image, parameter table. Not a File.
5. **Print** — printable parts: settings, orientation, why (bullets). Split: Print N× per token. Assembly MAY continue under this heading. Omit for non-printable sculpture.
6. **License** — license name and copyright line from `LICENSE`. Do not paste the full legal text.

Do **not** add a how-to-play line (Import from folder / open in OpenSCAD). Do **not** add a top-level **Parameters** section. Visible knobs sit under the Files bucket that owns them.

## Source

### Fork (`info.originType` is `fork`)

- Forked from: URL
- Original author
- Upstream license name
- This folder: packaging only (Customizer comments, listing, presets, file layout). Geometry is upstream.
- Libraries: third-party `use` / `include`, license, vendored path if any (detail MAY live only under Files / Libraries)

Do not claim Vary3D designed the part. Do not use original-model wording.

### Original (`info.originType` is `original`)

- Vary3D original. No upstream CAD.
- This folder: entry file, presets, listing copy
- Libraries: `none`, or a real `use` / vendored library

No Forked from, no `sourceUrl`, no Library badge.

Keep `ORIGIN.md` for catalog review. The README restates the same facts for GitHub readers.

## Files

Classify by **path and filename**, not by parsing OpenSCAD statements.

### Global

Only root `params.scad`. Not a build root; **do not render** a cover.

Must include the three-surface note in English (do not link the spec):

Shared sizes live in `params.scad` (Global on vary3d.com).

- vary3d.com: Global sliders on any file that includes `params.scad`.
- OpenSCAD app: opening the build root does not list those sliders (Customizer does not follow `include`). Edit `params.scad`, or use the site.
- OpenSCAD CLI: `openscad -D 'body_z=70.5' model.scad` on the entry file.

Then the visible Customizer table for `params.scad`. No `params.scad`: omit the whole Global bucket.

### Models

Package-root `.scad` files except `params.scad` and `geometry.scad`. These are exportable build roots. Image + filename + visible knobs for that file only. Entry MAY reuse `cover.png`. Extra roots use `covers/<stem>.png`. Extra Models that are each a full product do not imply Global.

### Libraries

- Root `geometry.scad` (modules only; not a build root)
- Subdirectories of `.scad` (vendored `scad-utils/`, BOSL2 copies, …)

Name, license, vendored path. **Do not render.** Do not open these as the Import entry.

| | Models | Presets |
|---|---|---|
| What | Another `.scad` to export | Named parameter set on a file already in the bundle |
| Image | Default cover for that build root | `covers/<preset>.png` from `cover-variants.py` |
| Example | Box / AA tray / 2×2 divider | Standard / Tall (`body_z`) |

`part=lid` is not a preset and not a Library.

## Presets

Image above the table. `params` keys only (not Hidden dump). Columns: name, value — no empty Range. If the item is a package variant, say that those keys are Global and still apply after switching build root.

No `variants.json`: omit the section.

## Print

Author this section (or keep it when regenerating). Typical bullets:

- **Settings:** material, layer, walls, infill, supports
- **Orientation:** how to place it on the bed (split: Print N× each token; `all` is preview)
- **Why:** why that orientation or recipe

## License

Fork: upstream copyright line (e.g. `Copyright (c) 2019 likeablob`). Original: this folder’s copyright. Full text stays in `LICENSE`.

## Images

Relative paths, no data URLs. Prefer GFM `![alt](path)` so Import can rewrite to docs media.

```text
cover.png
covers/standard.png
covers/battery-organizer.png
```

Preset covers: OpenSCAD `-D` on the preview entry so Global keys in `params.scad` take effect. Do not rewrite only the entry file’s top-level assignments. Do not render Global or Libraries.
