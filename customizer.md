# Customizer comments

Vary3D reads **top-level assignments before the first `module` or `function`**. Those become sliders on the site. Assignments inside the first module without a literal default are not knobs.

```openscad
// Short English one-liner: what the part is.

$fa = 4;
$fs = 0.4;

/* [Dimensions] */

// Length of each flange from the outer corner.
flange_length = 80; // [40:1:150]

/* [Features] */

// Cable channel through the front lip.
cable_slot = "yes"; // [yes:Yes, no:No]

/* [Rendering] */

bracket_color = "#2A9D90"; // color

angle_bracket();

module angle_bracket(
    flange_length = flange_length,
    cable_slot = cable_slot,
    bracket_color = bracket_color
) {
    color(bracket_color)
    union() {
        // ...
    }
}
```

## Minimum set

| Rule | How |
|---|---|
| Groups | `/* [Group Name] */` on its own line, English |
| Description | `// …` on the **line above** the assignment, English, written for the slider user |
| Numeric slider | `name = 80; // [40:1:150]` → min : step : max |
| Enum | `mode = "yes"; // [yes:Yes, no:No]` |
| Color | Parameter name ends with `_color`; default `"#RRGGBB"` (or an SVG color name); trailing `// color` |
| Hidden | Put derived constants under `/* [Hidden] */` |
| Names | Full `snake_case`: `flange_length`, not `fl` |
| Booleans | Prefer `"yes"` / `"no"` enums over bare `true` / `false` |

Comments, group titles, and parameter descriptions default to **English**. They are listing copy; the site may translate after publish.

Assignments must be **literals**. Do not put `width = base * 2;` before the first module — that is not an editable slider. Derived values go inside the module.

Helper modules with no literal defaults must not be the first `module` in the file (or they steal the Customizer parse window). Put helpers **after** the main module.

## Color knobs

```openscad
/* [Rendering] */

// Hull render color.
hull_color = "#2A9D90"; // color

color(hull_color) hull_body();
```

- Trailing `// color` (or `// [color]`) is required for a color picker.
- Default single-part color on Vary3D is `"#2A9D90"`.
- STL export has no color; use 3MF if you need color in the file.
