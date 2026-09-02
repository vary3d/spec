# info.json (`vary3d.info` v1)

Listing seed next to the entry `.scad`. This is not a brief, not a variant file, and not the model long-form.

This file is **on-disk interchange** (a publish folder, a git folder, Import from folder). The site does **not** persist `info.json` as a document. Import maps fields onto a **draft model** (name, description, category, tags, license, origin, entry code, cover). Documentation comes from package [README.md](package-readme.md), not from this JSON. Do not put server ids, object-storage paths, or `status` / `visibility` in the file. `slug` identifies the folder only; the public URL slug is assigned after review.

Do **not** write `print`. Print, assembly, and customize notes live in the README `## Print` (and Files) sections.

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
  "cover": "cover.png"
}
```

`format` is always `vary3d.info`. `version` is `1`. `sourceLocale` defaults to `en` (name, short description, tags in English; the site may translate after publish).

## Write every time

| Field | Rule |
|---|---|
| `slug` | Same as the folder name |
| `name` | Short English title, ≤100 characters |
| `description` | ≤800. Cards show the first sentence or two — lead with object + mate/feature. More sentences are optional. No slice recipe or assembly steps |
| `category` | One of the six values below |
| `tags` | About 3 English tokens (max 5). Axes: object, mate, distinctive feature; scene only if the object name is generic. Do not repeat the category; do not pad |
| `license` | Default `MIT`. Do not use ND |
| `originType` | `original` or `fork` |
| `engineType` | `openscad` |
| `entry` | `model.scad` unless the entry file is named otherwise |
| `cover` | `cover.png` when you ship a cover |

## Categories

`practical_gadgets` · `maker_kits` · `mechanical_structures` · `educational_models` · `interactive_toys` · `general_assets`

## When the model is a fork

Set `originType` to `fork` and fill what you know: `sourceUrl`, `originalAuthor`, `sourceLicense`, `attribution`. Keep the upstream `LICENSE` file next to the code. Do not replace it with a Vary3D copyright.

Do **not** write `parentModelId` unless it is a real Vary3D model id you already have. Never invent one; omit the field.

## Do not write

`print` (use README `## Print`; validators reject this key). Platform ids (`id`, `userId`), R2 paths, `status` / `visibility`, `engineVersion`, `*I18n` blobs, or view counts. Those are server fields.
