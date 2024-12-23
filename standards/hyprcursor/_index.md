---
weight: 1
title: hyprcursor
---

Hyprcursor is a new and efficient cursor theme format.

## Overview

The themes are created and managed with `hyprcursor-util`.

`hyprcursor-util` can create compiled (ready for use) themes from "working" themes.
Compilation involves compressing the data and packaging it.

## Working theme structure

The general overview looks like this:

```
root
┣ manifest.hl
┗ hyprcursors
  ┣ shapeA
  ┃ ┣ image.png
  ┃ ┣ image2.png
  ┃ ┗ meta.hl
  ┗ shapeB
    ┣ image.svg
    ┣ image2.svg
    ┗ meta.hl
```

### Manifest

The manifest file (named `manifest.hl` or `manifest.toml`) describes your theme.

The available properties are:

| name | description | type | required |
| -- | -- | -- | -- |
| cursors_directory | the directory where cursors are stored (in the example above, `hyprcursors`) | string | yes |
| name | the name of this theme | string | recommended |
| description | the description of this theme | string | no |
| version | the version of this theme | string | no |
| author | the author of this theme | string | no |

For hyprlang, an example would be:
```ini
cursors_directory = hyprcursors
name = My Theme
```

For toml, all values are under `[General]`:
```ini
[General]
cursors_directory = "hyprcursors"
name = "My Theme"
```

### Shapes and meta

Shapes describe a single shape in a theme. The meta describes the images.

Available properties are:

| name | description | type | exclusive\* |
| -- | -- | -- | -- |
| resize_algorithm | which resize algorithm to use for this shape. | bilinear, nearest, none | yes |
| hotspot_x\*\* | the absolute x coordinate of the hotspot. | float, 0.0 - 1.0 | yes |
| hotspot_y\*\* | the absolute y coordinate of the hotspot. | float, 0.0 - 1.0 | yes |
| nominal_size | the percentage that this size is compared to the selected theme size. E.g. 0.5 in a theme of 32x will result in 64x for this shape. | float, 0.1 - 2.0 | yes |
| define_override\*\*\* | defines an override this shape should take. E.g. define_override = arrow will make this shape also be responsible for a shape called "arrow" | string | no |
| define_size\*\*\* | defines a size variant. See size variants for more | size variant | no |


\* exclusive means that only one is allowed.

\*\* Hotspot is calculated from the top-left, and is a fraction of the total resulting pixel size of the shape.

\*\*\* These support smashing multiple statements into one with `;`, e.g. `define_override = arrow;pointer;default`.

As with the manifest, for toml, append `[General]` at the top.

#### Size variants

Size variants can take 2 or 3 arguments depending on whether they are animated or not:

```ini
# define_size = px size, filename
define_size = 64, image1.png
define_size = 32, image2.png
```

or
```ini
# define_size = px size, filename, delay in ms
define_size = 64, image1.svg, 500
define_size = 64, image2.svg, 500
```

Size defines have restrictions:
 - Animation delay _must_ be > 0.
 - Defining multiple images for the same size create an animation.
 - Defining both .png and .svg files in one meta is _not allowed_.
 - Files _must_ end with either .png or .svg and be a PNG or SVG respectively.
 - All cursor images _must_ have an aspect ratio of 1:1.
 - Animated svgs are _not supported_, create multiple svg files with each frame for animations.
 - Size parameter for .svg images is ignored, leaving it as 0 is recommended.
 - For png images, `px size` has to match the `.png`'s pixel size.

## Packaging

The packaged format is very similar to the working format. The process of packaging (done by `hyprcursor-util`) involves
taking every directory in the cursor shape subdirectory (in the above example, `hyprcursors/`) and turning it into a
zip file with the `.hlc` extension.

For example, for the structure mentioned in the example further up, we'd take `shapeA/` and turn it into a zip file named `shapeA.hlc`
with the following contents:

```
shapeA.hlc
┣ image.png
┣ image2.png
┗ meta.hl
```

Note there is no default subdirectory and all the files are in the zip's root.

