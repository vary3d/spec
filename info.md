# info.json (`vary3d.info` v1)

Listing seed next to the entry `.scad`. This is not a brief, not a variant file, and not print-only docs.

```json
{
  "format": "vary3d.info",
  "version": 1,
  "sourceLocale": "en",
  "slug": "angle-bracket",
  "name": "Honeycomb Angle Bracket",
  "description": "90-degree L-bracket with honeycomb lightening and M5 clearance holes.",
  "category": "practical_gadgets",
  "tags": ["bracket", "honeycomb", "m5"],
  "license": "MIT",
  "originType": "original",
  "engineType": "openscad",
  "entry": "model.scad",
  "cover": "cover.png",
  "print": {
    "settings": "PLA, 0.2mm layer, 2 walls, 15% gyroid infill, no supports.",
    "orientation": "Place a flange on the bed.",
    "why": "Keeps honeycomb walls near vertical."
  }
}
```

`format` is always `vary3d.info`. `version` is `1`. `sourceLocale` defaults to `en` (name, short description, tags in English; the site may translate after publish).

## Write every time

| Field | Rule |
|---|---|
| `slug` | Same as the folder name |
| `name` | Short English title, ≤100 characters |
| `description` | Short summary, ≤800. No slice recipe or assembly steps here |
| `category` | One of the six values below |
| `tags` | ≤10 English tokens; function/object; do not repeat the category name |
| `license` | Default `MIT`. Do not use ND |
| `originType` | `original` or `fork` |
| `engineType` | `openscad` |
| `entry` | `model.scad` unless the entry file is named otherwise |
| `cover` | `cover.png` when you ship a cover |

## Categories

`practical_gadgets` · `maker_kits` · `mechanical_structures` · `educational_models` · `interactive_toys` · `general_assets`

## When the model is a fork

Set `originType` to `fork` and fill what you know: `sourceUrl`, `originalAuthor`, `sourceLicense`, `attribution`. Keep the upstream `LICENSE` file next to the code. Do not replace it with a Vary3D copyright.

## Print notes

Optional `print` object maps to Docs on the site, not to a database column:

- `settings` — material, layer, walls, infill, supports
- `orientation` — how to place it on the bed
- `why` — why that orientation or recipe

Omit `print` for non-printable sculptures.

## Do not write

Platform ids (`id`, `userId`), R2 paths, `status` / `visibility`, `engineVersion`, `*I18n` blobs, or view counts. Those are server fields.
