# variants.json (`vary3d.variants` v1)

Named parameter presets. No images in this file.

This file is **on-disk interchange** (export / import, skill packages, git). The site does **not** store `vary3d.variants` JSON as the database row. On import each preset becomes a **draft variant** whose `parametersJson` uses the site envelope (`__vary`). Do **not** write `__vary` into this file; importers add it.

```json
{
  "format": "vary3d.variants",
  "version": 1,
  "files": {
    "model.scad": [
      {
        "name": "M5",
        "description": "Clearance for M5 hardware",
        "params": {
          "bolt_clearance_dia": 5.5,
          "pcd": 28
        }
      },
      {
        "name": "M4",
        "params": {
          "bolt_clearance_dia": 4.5,
          "pcd": 24
        }
      }
    ]
  }
}
```

`format` is always `vary3d.variants`. `version` is `1`. Write this file when there are **at least two useful presets**.

## Top-level fields

| Field | Required | Notes |
|---|---|---|
| `format` | yes | `vary3d.variants` |
| `version` | yes | `1` |
| `exportedAt` | no | ISO-8601 |
| `modelHint` | no | `{ "id"?, "slug"? }` — importers must not fail if it disagrees |
| `files` | * | File variants, keyed by bundle-relative `.scad` path |
| `package` | * | Package variants (shared / global parameters) |

\* At least one of `files` or `package` must be a non-empty collection.

## File variants

Keys in `files` are relative paths with `/`, no `..`, no leading `/`, no Windows drives. Examples: `model.scad`, `lib/helper.scad`.

Each item:

| Field | Required | Notes |
|---|---|---|
| `name` | yes | Preset title |
| `description` | no | Short English blurb |
| `params` | yes | Local parameters for **that file** only. No `__vary`. Prefer no keys starting with `_` |

## Package variants

Use `package` when the model has a root [params.scad](params-scad.md) and you want a preset of **shared** parameters (wall, clearance, …).

```json
{
  "format": "vary3d.variants",
  "version": 1,
  "package": [
    {
      "name": "Tight clearance",
      "previewEntryPath": "bracket.scad",
      "params": {
        "clearance": 0.15,
        "wall": 2.4
      }
    }
  ]
}
```

| Field | Required | Notes |
|---|---|---|
| `name` | yes | Preset title |
| `description` | no | |
| `previewEntryPath` | no | Which part to preview; default is the current / soft entry |
| `params` | yes | Snapshot of **global** parameters from `params.scad` |

Omit empty `package` arrays.

## English

Preset `name` and `description` are English source strings. The site may translate them after publish.
