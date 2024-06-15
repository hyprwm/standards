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

root
┣ manifest.hl
┗ hyprcursors
  ┣ shapeA
  ┃ ┣ image.png
  ┃ ┣ image2.png
  ┃ ┗ meta.hl
  ┗ shapeB
    ┣ image.png
    ┣ image2.png
    ┗ meta.hl

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
| hotspot_x | the absolute x coordinate of the hotspot, from top left. | float, 0.0 - 1.0 | yes |
| hotspot_y | the absolute y coordinate of the hotspot, from top left. | float, 0.0 - 1.0 | yes |
| define_override\*\* | defines an override this shape should take. E.g. define_override = arrow will make this shape also be responsible for a shape called "arrow" | string | no |
| define_size\*\* | defines a size variant. See size variants for more | size variant | no |


\* exclusive means that only one is allowed.

\*\* These support smashing multiple statements into one with `;`, e.g. `define_override = arrow;pointer;default`.

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
define_size = 64, image1.png, 500
define_size = 64, image2.png, 500
```

Size defines have restrictions:
 - Animation delay _must_ be > 0.
 - Defining multiple images for the same size create an animation.
 - Defining both .png and .svg files in one meta is _not allowed_.
 - Files _must_ end with either .png or .svg and be a PNG or SVG respectively.
 - All cursor images _must_ have an aspect ratio of 1:1.
 - Animated svgs are _not supported_, create multiple svg files with each frame for animations.
 - Size parameter for .svg images is ignored, leaving it as 0 is recommended.
