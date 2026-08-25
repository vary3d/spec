# Vary3D Spec

Parametric OpenSCAD in the browser — change numbers, export STL / 3MF.

This repository is the **publish format** for models that run on **[vary3d.com](https://vary3d.com)**: folder layout, listing JSON, parameter variants, Customizer comments, and optional package-wide `params.scad`.

It is not the website source, not an STL warehouse, and not an OpenSCAD include library.

The format does **not** upload models for you. Keep the upstream license; Forked from is required when the design is not original. Guests can still tweak parameters in the browser and export STL / 3MF.

## On disk, not the server

`info.json` and `variants.json` are an **on-disk interchange format** (git, skill output, Import from folder). They are **not** how Vary3D stores a model.

On import the site **maps** the files into draft models and draft variants: listing columns, OpenSCAD on object storage, documentation from `print`, and per-variant parameter snapshots. It does **not** keep the JSON documents as blobs. `info.slug` is a folder hint only; the site assigns a public slug after review. Variant files must not include the site’s `__vary` envelope — that is added when the snapshot is saved.

## Documents

| Doc | Topic |
|---|---|
| [directory.md](directory.md) | `models/<slug>/` layout |
| [info.md](info.md) | `info.json` (`vary3d.info` v1) |
| [variants.md](variants.md) | `variants.json` (`vary3d.variants` v1) |
| [customizer.md](customizer.md) | Minimum Customizer comments |
| [params-scad.md](params-scad.md) | Root `params.scad` (shared parameters) |

Agent authors: [vary3d/skills](https://github.com/vary3d/skills). Catalog: [vary3d/library](https://github.com/vary3d/library).

## Issues

Use Issues for this format (ambiguous fields, missing examples).

Do not file site, account, payment, or moderation bugs here.

## License

MIT. See [LICENSE](LICENSE). Spec text describes the format; models you publish keep their own licenses.

## Security

See [SECURITY.md](SECURITY.md). Email **security@vary3d.com**. Do not post proofs of concept in public issues.
