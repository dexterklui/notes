---
title: Canvas
date: 2023-07-30 (Sun)
---

# Coordinate system

Top left corner at (0, 0). Button right corner at (`width`, `height`).

# Create a Canvas

## HTML element

```html
<canvas width="800px" height="500px">
  Your browser doesn't support canvas
</canvas>
```

## Set up a pen in 2D context

```javascript
const canvas = document.querySelector("#canvas");
const ctx = canvas.getContext("2d");
```

Then you can start drawing on the canvas by calling methods on `ctx`.

## Accessibility

- Canvas content is not accessible to screen readers.
- Include attribute `role="presentation"` for purely decorative canvas
- If canvas has descriptive content:
  - Include descriptive text as the value of attribute `aria-label`; or
  - Include fallback content within tag `<cavas>`

# Draw Methods

- `stoke()`
- `fill()`
  - Any open shapes are closed automatically when calling `fill()`.
  - It can accept a filling rule: `nonzero` (default) or `evenodd`

Both methods can accept a `Path2D` object to draw the stored path.

# Path Methods

- `beginPath()` resets the list of subpaths
  - Note that the first path construction command after `beginPath()` is always
    treated as `moveTo()`. So you might as well do a `moveTo()` yourself as a
    first command.
- `closePath()` straight line to start of current subpath to make shape
  - Does nothing if path already closed or has only one point
- `moveTo(x, y)`
- `lineTo(x, y)`
- `arc(x, y, r, θ1, θ2, counterclockwise)`
  - θ measured in **rad** from positive x-axis
- `arcTo(x1, y1, x2, y2, radius)` draws rounded corner tangential to starting
  line (x0, y0) to (x1, y1) and ending line (x1, y1) to (x2, y2).
- `quadraticCurveTo(cx, cy, x, y)`
- `bezierCurveTo(cx1, cy1, cx2, cy2, x, y)`
- `rect(x, y, w, h)`: a `moveTo()` is automatically executed first

# Polygon Methods

- `fillRect(x, y, w, h)`
- `strokeRect(x, y, w, h)`
- `clearRect(x, y, w, h)` clears an area making it fully transparent

# Style

## Colour and opacity

Default colour is black. To change set the field `fillStyle` and `strokeStyle`.
`globalAlpha` from 0 to 1 (opaque).

Default is to combine alpha value, for other composition operations, e.g.
replacing existing pixel with a transparent colour entirely, check
[Composition and blending](#composition-and-blending).

## Line styles

| Property                | Description                           |
| ----------------------- | ------------------------------------- |
| `lineWidth`             | Width of stroke line                  |
| `lineCap`               | Appearance ends of lines              |
| `lineJoin`              | Appearance of line corners            |
| `miterLimit`            | Control how think the junction become |
| `getLineDash()`         | Return line dash pattern array        |
| `setLineDash(segments)` | Set line dash pattern                 |
| `lineDashOffset`        | Specify where to start a dash array   |

### `lineWidth`

Default is 1.0. A line extends half it's `lineWidth` from centre on both side.

To create a crisp line, you need to make sure a line extends to pixel boundary
under certain scenarios. In short, a odd-width line should be drawn at half
pixel. See
[A `lineWidth` example - mdn web docs](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API/Tutorial/Applying_styles_and_colors#a_linewidth_example).

### `lineCap`

| Value    | Description                                            |
| -------- | ------------------------------------------------------ |
| `butt`   | Default; ends of lines are squared off at endpoints    |
| `round`  | Ends of lines are rounded w/ radius = half `lineWidth` |
| `square` | Like `butt` but w/ extra height = half `lineWidth`     |

### `lineJoin`

| Value   | Description                                             |
| ------- | ------------------------------------------------------- |
| `round` | Round off corners                                       |
| `bevel` | Fill the gap with additional triangle                   |
| `miter` | Default; extend outside edge to connect at sharp corner |

### `miterLimit`

When `lineJoin = miter`, determines how far the outside connection point can be
placed from the inside connection point (especially at small angle). Corners
exceeding the limit will use `bevel` `lineJoin` instead.

### `setLineDash` and `lineDashOffset`

- `setLineDash()` accepts a list of numbers that specifies distances to
  alternately draw a line and a gap.
- `lineDashOffset` sets an offset where to start the pattern.

## Other styles

mdn web docs:

- [Gradients](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API/Tutorial/Applying_styles_and_colors#gradients)
- [Patterns](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API/Tutorial/Applying_styles_and_colors#patterns)
- [Shadows](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API/Tutorial/Applying_styles_and_colors#shadows)
- [Canvas fill rules](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API/Tutorial/Applying_styles_and_colors#canvas_fill_rules)

# Text Methods

| Property                 | Description                                                       |
| ------------------------ | ----------------------------------------------------------------- |
| `fillText(text, x, y)`   | Draw filled text                                                  |
| `strokeText(text, x, y)` | Draw text without fill                                            |
| `font`                   | Font properties                                                   |
| `textAlign`              | `start`, `end`, `left`, `right`, `center`                         |
| `textBaseline`           | `top`, `hanging`, `middle`, `alphabetic`, `ideographic`, `bottom` |
| `direction`              | `ltr`, `rtl`, `inherit`                                           |

```javascript
ctx.font = "30px Arial";
ctx.fillText("Hello world", 10, 50);
```

# Path2D Object

Use `Path2D` object.

## Creating Path2D

```javascript
new Path2D(); // empty path object
new Path2D(path); // copy from another Path2D object
new Path2D(d); // path from SVG path string
```

## Adding paths

You can use all [path methods](#path-methods) as `Path2D` instance methods to
add paths to it. You can also use `addPath(path, [transform])` to add a path
from another `Path2D` object to current path with an optional transformation
matrix.

# Saving and Restoring

## Settings

`CavasRenderingContext2D` has the following methods:

- `save()` saves the entire state of the canvas on a **stack**.
- `restore()` pop the save stack to restore

A drawing state consists of

- Transformation: `translate`, `rotate`, `scale`
- Style setting
- Current clipping path

## Drawings

`CanvasRenderingContext2D`'s method:

| Method                     | Description                             |
| -------------------------- | --------------------------------------- |
| `getImageData(x, y, w, h)` | Get `ImageData` representing pixel data |
| `putImageData(data, x, y)` | Paint `ImageData`                       |

Note that these methods are not affected by canvas transformation matrix.
`putImageData()` also can accept four more arguments (`x, y, w, h`) to specify
the area within the image data for painting.

Be **warned** that when using `putImageData()`, even if a pixel is transparent,
the transparent value will replace the original value at that pixel. That is,
the original drawing will be lost even if it is located in a transparent region
of the image data. If you want to keep those original drawing, you need to draw
the image data on a temporary canvas, and then use that canvas in `drawImage()`
method:

```javascript
const tmpCanvas = document.createElement("canvas");
tmpCanvas.width = imageDataWidth;
tmpCanvas.height = imageDataHeight;
const ctx = tmpCanvas.getContext("2d");
ctx.putImageData(imageData, 0, 0);
targetCanvas.drawImage(tmpCanvas, x, y);
```

## Download as an image

`HTMLCanvasElement.toDataURL("image/png").replace("image/png", "image/octet-stream");`

# Transformation

The following transform the coordinate system (not existing drawings).

| Method                        | Description                                |
| ----------------------------- | ------------------------------------------ |
| `translate(x, y)`             | Translate origin                           |
| `rotate(rad)`                 | Rotate at origin clockwise from +ve x-axis |
| `scale(x, y)`                 | Apply scale factor; negative value mirrors |
| `transform(a, b, c, d, e, f)` | Apply transformation matrix                |

# Composition and blending

Field `globalCompositeOperation` handles what composition type is used when
drawing on the canvas. See
[w3schools](https://www.w3schools.com/tags/canvas_globalcompositeoperation.asp)

# Libraries for Advanced Usage

- [phaser](https://phaser.io)
- [physics-cannonjs](https://sbcode.net/threejs/physics-cannonjs)

# Links

- [mdn web docs](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API)

# 🧭 Navigation

- [🔼 Back to top](#)
- [◀️ Back](index.md)
- [🔖 Parent index](index.md)
- [📑 Notes Index](../../index.md)
- [🗃️ Master Index](../../../index.md)
