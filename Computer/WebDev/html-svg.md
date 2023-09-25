---
title: HTML - SVG
date: 2023-07-30 (Sun)
---

# Basic Syntax

Within `<svg></svg>`, put svg subnodes to defines lines and shapes. Their
subnodes specify drawing directives like 2d context methods from `<canvas>`.

`<svg>` tag has following attributes:

- `width`
- `height`
- `xmlns="http://www.w3.org/2000/svg"`: don't know what does this do

# Style

## Basic styling attributes

For each svg subnodes, there are different attributes to set the style.

- `stroke` sets the stroke colour
- `fill` sets the fill colour
- `stroke-width`
- `fill-opacity`

## Stroking

- `stroke="red"`
- `stroke-width="4"`
- `stroke-linecap` can be `butt`, `round` or `square`
- `stroke-dasharray="10,10"`, or can equal `20,10,5,5,5,10`. Draw **dashed
  lines**.

## Shorthand styling

You can use the `style` attribute for a short hand version:
`style="fill:rgb(0,0,255);stroke-width:10;stroke:rgb(0,0,0):opacity:0.5"`

## Gradients

```html
<svg>
  <defs>
    <linearGradient id="grad1" x1="0%" y1="0%" x2="100%" y2="0%">
      <stop offset="0%" style="stop-color:rgb(255,255,0);stop-opacity:1" />
      <stop offset="100%" style="stop-color:rgb(255,0,0);stop-opacity:1" />
    </linearGradient>
  </defs>
  <ellipse cx="100" cy="70" rx="85" ry="55" fill="url(#grad1)" />
</svg>
```

# Path

## Syntax

`<path d="...">`. The value of attribute `d` are a space-separated list of path
commands.

## Path commands

| Command                     | Description                 |
| --------------------------- | --------------------------- |
| `M x y`                     | Move to coord               |
| `L x y`                     | Line to coord               |
| `H x`                       | Hoirzontal line to          |
| `V y`                       | Verticla line to            |
| `Z`                         | Close path (to begin point) |
| `C cx1 cy1, cx2 cy2, x y`   | Bezier curve to             |
| `S cx2 cy2, x y`            | Bezier curve to             |
| `Q cx1 cy1, x y`            | Quadratic curve to          |
| `T x y`                     | Quadratic curve to          |
| `A rx ry θ l-arc sweep x y` | Arc to                      |

All commands has a lowercase version, in which the coordinates are interpreted
as **relative** coordinates from the starting point.

## S and Q

Their first control point's coordinates are taken from the previous curve's last
control point's coordinate after a reflection at the starting point. I.e. it is
draw smooth curves when chaining multiple bezier and quadratic curves.

## Arc

- `rx` and `ry` are radius of a eclipse along x- and y-axis.
  - In case `rx` and `ry` are not enough for the destination, they implicitly
    increase to the minimal value required
- `θ` is the angle in degree rotating the eclipse
- `l-arc` is a 1 / 0 flag choosing the larger arc or smaller arc
- `sweep` is a 1 / 0 flag choosing the upper arc or lower arc
- `x` and `y` are the location of final destination

# Lines, Shapes, Texts

## Common attributes

For shapes:

- `x` x-coordinate of top left corner
- `y` y-coordinate of top left corner

## Line

```html
<line x1="0" y1="0" x2="200" y2="200" />
```

## Polyline

```html
<polyline points="20,20 40,25 60,40 80,120 120,140 200,280" />
```

## Circle

```html
<circle cx="50" cy="50" r="40" />
```

## Ellipse

```html
<ellipse cs="100" cy="70" rx="85" ry="55" />
```

## Rectangle

```html
<rect width="400" height="100" />
```

## Rounded rectangle

```html
<rect rx="20" ry="20" width="150" height="150" />
```

## Polygon

```html
<polygon points="100,10 40,198 190,78 10,78 160,198" />
```

## Text

```html
<text x="0" y="15" fill="red">Good</text>
```

# Groups

`<g>` SVG element is a container used to **group** other SVG elements.

- Attributes of `<g>` are inherited by its children.
- Transformations applied to `<g>` are performed on its child elements
- Also to group multiple elements to be referenced later with the `<use>`
  element.

# Reusing Elements

## Use

`<use>` takes nodes in SVG and duplicates them.

Attributes in `<use>` **merges** with attributes in the original element, but do
**NOT** override them.

### Technical mechanism

The effect is the same as if the nodes were deeply cloned into a non-exposed
DOM, then pasted where the `use` element is.

### Example

```html
<circle id="myCircle" cx="5" cy="5" r="4" stroke="blue" />
<use href="#myCircle" x="10" fill="blue" stroke="red" />
<!-- stroke="red" has no effect, it cannot override existing attributes -->
```

More useful is to duplicate `<g>` that groups multiple elements.

# SVG Views

## Viewbox attribute

`viewBox` defines the position and dimension. The value is a list of four
numbers: `min-x`, `min-y`, `width`, and `height`. `min-x` and `min-y` are
coordinates of top left corner.

```html
<svg viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg"></svg>
```

## View element

`<view>` defines a way to view the image, like a zoom level or a detail view. It
can be used when sourcing an `svg` file with `mysvg.svg#{view_id}`, see
[view - mdn web docs](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/view).

# Transform

```html
<g
  transform="rotate(-10 50 100)
             translate(-36 45.5)
             skewX(40)
             scale(1 0.5)"
></g>
```

- `rotate(a, x, y)`
  - a: angle in degree
  - x: optional; x-coordinate of rotation origin
  - y: optional: y-coordinate of rotation origin

# Resizing SVG

To resizing SVG as a whole, you don't specify the `height="200"` and
`width="200"` but only the `viewBox = "0 0 200 200"`. Then you can use CSS to
resize the SVG element to any height or width.

# Hover Tooltip

Add `<title>tooltip text</title>` inside an SVG element to make hover tooltip
for the element.

# 🧭 Navigation

- [🔼 Back to top](#)
- [◀️ Back](../../index.md)
- [🔖 Parent index](../../index.md)
- [📑 Notes Index](../../index.md)
- [🗃️ Master Index](../../../index.md)
